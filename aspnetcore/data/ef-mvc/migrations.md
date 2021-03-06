---
title: ASP.NET Core MVC com EF Core – migrações – 4 de 10
author: rick-anderson
description: Neste tutorial, você começa a usar o recurso de migrações do EF Core para gerenciar alterações do modelo de dados em um aplicativo ASP.NET Core MVC.
ms.author: tdykstra
ms.date: 03/15/2018
uid: data/ef-mvc/migrations
ms.openlocfilehash: f710b33ac1a6017b0e3d7e8c3e528675a41424bb
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38194164"
---
# <a name="aspnet-core-mvc-with-ef-core---migrations---4-of-10"></a>ASP.NET Core MVC com EF Core – migrações – 4 de 10

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Por [Tom Dykstra](https://github.com/tdykstra) e [Rick Anderson](https://twitter.com/RickAndMSFT)

O aplicativo web de exemplo Contoso University demonstra como criar aplicativos web do ASP.NET Core MVC usando o Entity Framework Core e o Visual Studio. Para obter informações sobre a série de tutoriais, consulte [o primeiro tutorial da série](intro.md).

Neste tutorial, você começa usando o recurso de migrações do EF Core para o gerenciamento de alterações do modelo de dados. Em tutoriais seguintes, você adicionará mais migrações conforme você alterar o modelo de dados.

## <a name="introduction-to-migrations"></a>Introdução às migrações

Quando você desenvolve um novo aplicativo, o modelo de dados é alterado com frequência e, sempre que o modelo é alterado, ele fica fora de sincronia com o banco de dados. Você começou estes tutoriais configurando o Entity Framework para criar o banco de dados, caso ele não exista. Em seguida, sempre que você alterar o modelo de dados – adicionar, remover, alterar classes de entidade ou alterar a classe DbContext –, poderá excluir o banco de dados e o EF criará um novo que corresponde ao modelo e o propagará com os dados de teste.

Esse método de manter o banco de dados em sincronia com o modelo de dados funciona bem até que você implante o aplicativo em produção. Quando o aplicativo é executado em produção, ele normalmente armazena os dados que você deseja manter, e você não quer perder tudo sempre que fizer uma alteração, como a adição de uma nova coluna. O recurso Migrações do EF Core resolve esse problema, permitindo que o EF atualize o esquema de banco de dados em vez de criar um novo banco de dados.

## <a name="entity-framework-core-nuget-packages-for-migrations"></a>Pacotes NuGet do Entity Framework Core para migrações

Para trabalhar com migrações, use o **PMC** (Console do Gerenciador de Pacotes) ou a CLI (interface de linha de comando).  Esses tutoriais mostram como usar comandos da CLI. Encontre informações sobre o PMC no [final deste tutorial](#pmc).

As ferramentas do EF para a CLI (interface de linha de comando) são fornecidas em [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Para instalar esse pacote, adicione-o à coleção `DotNetCliToolReference` no arquivo *.csproj*, conforme mostrado. **Observação:** é necessário instalar este pacote editando o arquivo *.csproj*; não é possível usar o comando `install-package` ou a GUI do Gerenciador de Pacotes. Edite o arquivo *.csproj* clicando com o botão direito do mouse no nome do projeto no **Gerenciador de Soluções** e selecionando **Editar ContosoUniversity.csproj**.

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
(Neste exemplo, os números de versão eram atuais no momento em que o tutorial foi escrito.)

## <a name="change-the-connection-string"></a>Alterar a cadeia de conexão

No arquivo *appsettings.json*, altere o nome do banco de dados na cadeia de conexão para ContosoUniversity2 ou outro nome que você ainda não usou no computador que está sendo usado.

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

Essa alteração configura o projeto, de modo que a primeira migração crie um novo banco de dados. Isso não é necessário para começar as migrações, mas você verá mais tarde o motivo pelo qual essa é uma boa ideia.

> [!NOTE]
> Como alternativa à alteração do nome do banco de dados, você pode excluir o banco de dados. Use o **SSOX** (Pesquisador de Objetos do SQL Server) ou o comando `database drop` da CLI:
> ```console
> dotnet ef database drop
> ```
> A seção a seguir explica como executar comandos da CLI.

## <a name="create-an-initial-migration"></a>Criar uma migração inicial

Salve as alterações e compile o projeto. Em seguida, abra uma janela Comando e navegue para a pasta do projeto. Esta é uma maneira rápida de fazer isso:

* No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e escolha **Abrir no Explorador de Arquivos** no menu de contexto.

  ![Abrir no item de menu do Explorador de Arquivos](migrations/_static/open-in-file-explorer.png)

* Insira "cmd" na barra de endereços e pressione Enter.

  ![Abrir a janela Comando](migrations/_static/open-command-window.png)

Insira o seguinte comando na janela de comando:

```console
dotnet ef migrations add InitialCreate
```

Você verá uma saída semelhante à seguinte na janela Comando:

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> Se você receber uma mensagem de erro *Nenhum comando "dotnet-ef" executável correspondente encontrado*, consulte [esta postagem no blog](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) para ajudar a solucionar o problema.

Se você receber uma mensagem de erro "*Não é possível acessar o arquivo... ContosoUniversity.dll porque ele está sendo usado por outro processo*", localize o ícone do IIS Express na Bandeja do Sistema do Windows, clique com o botão direito do mouse nele e, em seguida, clique em **ContosoUniversity > Parar Site**.

## <a name="examine-the-up-and-down-methods"></a>Examinar os métodos Up e Down

Quando você executou o comando `migrations add`, o EF gerou o código que criará o banco de dados do zero. Esse código está localizado na pasta *Migrations*, no arquivo chamado *\<timestamp>_InitialCreate.cs*. O método `Up` da classe `InitialCreate` cria as tabelas de banco de dados que correspondem aos conjuntos de entidades do modelo de dados, e o método `Down` exclui-as, conforme mostrado no exemplo a seguir.

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

As migrações chamam o método `Up` para implementar as alterações do modelo de dados para uma migração. Quando você insere um comando para reverter a atualização, as Migrações chamam o método `Down`.

Esse código destina-se à migração inicial que foi criada quando você inseriu o comando `migrations add InitialCreate`. O parâmetro de nome da migração ("InitialCreate" no exemplo) é usado para o nome do arquivo e pode ser o que você desejar. É melhor escolher uma palavra ou frase que resume o que está sendo feito na migração. Por exemplo, você pode nomear uma migração posterior "AddDepartmentTable".

Se você criou a migração inicial quando o banco de dados já existia, o código de criação de banco de dados é gerado, mas ele não precisa ser executado porque o banco de dados já corresponde ao modelo de dados. Quando você implantar o aplicativo em outro ambiente no qual o banco de dados ainda não existe, esse código será executado para criar o banco de dados; portanto, é uma boa ideia testá-lo primeiro. É por isso que você alterou o nome do banco de dados na cadeia de conexão anteriormente – para que as migrações possam criar um novo do zero.

## <a name="the-data-model-snapshot"></a>O instantâneo do modelo de dados

As migrações criam um *instantâneo* do esquema de banco de dados atual em *Migrations/SchoolContextModelSnapshot.cs*. Quando você adiciona uma migração, o EF determina o que foi alterado, comparando o modelo de dados com o arquivo de instantâneo.

Ao excluir uma migração, use o comando [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove). `dotnet ef migrations remove` exclui a migração e garante que o instantâneo seja redefinido corretamente.

Confira [Migrações do EF Core em ambientes de equipe](/ef/core/managing-schemas/migrations/teams) para obter mais informações de como o arquivo de instantâneo é usado.

## <a name="apply-the-migration-to-the-database"></a>Aplicar a migração ao banco de dados

Na janela Comando, insira o comando a seguir para criar o banco de dados e tabelas nele.

```console
dotnet ef database update
```

A saída do comando é semelhante ao comando `migrations add`, exceto que os logs para os comandos SQL que configuram o banco de dados são exibidos. A maioria dos logs é omitida na seguinte saída de exemplo. Se você preferir não ver esse nível de detalhe em mensagens de log, altere o nível de log no arquivo *appsettings.Development.json*. Para obter mais informações, consulte [Introdução ao log](xref:fundamentals/logging/index).

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

Use o **Pesquisador de Objetos do SQL Server** para inspecionar o banco de dados como você fez no primeiro tutorial.  Você observará a adição de uma tabela __EFMigrationsHistory que controla quais migrações foram aplicadas ao banco de dados. Exiba os dados dessa tabela e você verá uma linha para a primeira migração. (O último log no exemplo de saída da CLI anterior mostra a instrução INSERT que cria essa linha.)

Execute o aplicativo para verificar se tudo ainda funciona como antes.

![Página Índice de Alunos](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a>CLI (interface de linha de comando) vs. PMC (Console do Gerenciador de Pacotes)

As ferramentas do EF para gerenciamento de migrações estão disponíveis por meio dos comandos da CLI do .NET Core ou de cmdlets do PowerShell na janela **PMC** (Console do Gerenciador de Pacotes) do Visual Studio. Este tutorial mostra como usar a CLI, mas você poderá usar o PMC se preferir.

Os comandos do EF para os comandos do PMC estão no pacote [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools). Este pacote já está incluído no metapacote [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) e, portanto, não é necessário instalá-lo.

**Importante:** esse não é o mesmo pacote que é instalado para a CLI com a edição do arquivo *.csproj*. O nome deste termina com `Tools`, ao contrário do nome do pacote da CLI que termina com `Tools.DotNet`.

Para obter mais informações sobre os comandos da CLI, consulte [CLI do .NET Core](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).

Para obter mais informações sobre os comandos do PMC, consulte [Console do Gerenciador de Pacotes (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).

## <a name="summary"></a>Resumo

Neste tutorial, você viu como criar e aplicar sua primeira migração. No próximo tutorial, você começará examinando tópicos mais avançados com a expansão do modelo de dados. Ao longo do processo, você criará e aplicará migrações adicionais.
::: moniker-end

> [!div class="step-by-step"]
> [Anterior](sort-filter-page.md)
> [Próximo](complex-data-model.md)
