---
title: Metapacote Microsoft.AspNetCore.App para ASP.NET Core 2.1 e posterior
author: Rick-Anderson
description: O metapacote Microsoft.AspNetCore.App inclui todos os pacotes do ASP.NET Core e Entity Framework Core compatíveis.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage-app
ms.openlocfilehash: 95fd6b7e73cf325674f1c1e03f9eea88cbc1af13
ms.sourcegitcommit: f3538693a12cf55b7f124a6519677239170b7c43
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2018
ms.locfileid: "43114769"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a>Metapacote Microsoft.AspNetCore.App para ASP.NET Core 2.1

Este recurso exige o ASP.NET Core 2.1 e posterior direcionado ao .NET Core 2.1 e posterior.

O [metapacote](/dotnet/core/packages#metapackages) [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) para ASP.NET Core:

* Não tem dependências de terceiros, com exceção de [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/) e [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/). Essas dependências de terceiros são consideradas necessárias para garantir o funcionamento dos principais recursos das estruturas.
* Inclui todos os pacotes com suporte pela equipe do ASP.NET Core, exceto aqueles que contêm dependências de terceiros (que não sejam aqueles mencionados anteriormente).
* Inclui todos os pacotes com suporte pela equipe do Entity Framework Core, exceto aqueles que contêm dependências de terceiros (que não sejam aqueles mencionados anteriormente).

Todos os recursos do ASP.NET Core 2.1 e posterior e do Entity Framework Core 2.1 e posterior são incluídos no pacote `Microsoft.AspNetCore.App`. Os modelos de projeto padrão direcionados para ASP.NET Core 2.1 e posterior usam este pacote. Recomendamos que aplicativos voltados para o ASP.NET Core 2.1 e posterior e o Entity Framework Core 2.1 e posterior usem o pacote `Microsoft.AspNetCore.App`.

O número de versão do metapacote `Microsoft.AspNetCore.App` representa a versão do ASP.NET Core e a versão do Entity Framework Core.

O uso do metapacote `Microsoft.AspNetCore.App` fornece restrições de versões que protegem seu aplicativo:

* Se um pacote incluído tem uma dependência (não direta) transitiva em um pacote no `Microsoft.AspNetCore.App`, e os números de versão forem diferentes, o NuGet gera um erro.
* Outros pacotes adicionados ao seu aplicativo não podem alterar a versão dos pacotes incluídos no `Microsoft.AspNetCore.App`.
* A consistência de versão garante uma experiência confiável. `Microsoft.AspNetCore.App` foi projetado para evitar combinações de versão não testado de bits relacionados que estão sendo usados juntos no mesmo aplicativo.

Aplicativos que usam o metapacote `Microsoft.AspNetCore.App` aproveitam automaticamente a estrutura compartilhada do ASP.NET Core. Quando você usa o metapacote `Microsoft.AspNetCore.App`, **nenhum** ativo dos pacotes NuGet do ASP.NET Core referenciados é implantado com o aplicativo, porque a estrutura compartilhada do ASP.NET Core contém esses ativos. Os ativos na estrutura compartilhada são pré-compilados para melhorar o tempo de inicialização do aplicativo. Para obter mais informações, veja "estrutura compartilhada" em [Empacotamento de distribuição do .NET Core](/dotnet/core/build/distribution-packaging).

O seguinte arquivo de projeto referencia o metapacote `Microsoft.AspNetCore.App` do ASP.NET Core e representa um modelo típico do ASP.NET Core 2.1:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" Version="2.1.1" />
  </ItemGroup>

</Project>
```

O número de versão na referência `Microsoft.AspNetCore.App` **não** garante que a versão da estrutura compartilhada será usada. Por exemplo, suponha que a versão `2.1.1` foi especificada, mas `2.1.3` está instalada. Nesse caso, o aplicativo usa `2.1.3`. Embora não seja recomendado, é possível desabilitar o comportamento de roll forward (patch e/ou secundário). Para obter mais informações sobre o comportamento de roll forward da versão do pacote, consulte [dotnet host roll-forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

## <a name="update-aspnet-core"></a>Atualizar o ASP.NET Core

O `Microsoft.AspNetCore.App` [metapacote](/dotnet/core/packages#metapackages) não é um pacote tradicional atualizado do NuGet. Semelhante ao `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` representa um tempo de execução compartilhado, que tem semântica de controle de versão especial tratada fora do NuGet. Para obter mais informações, veja [Pacotes, metapacotes e estruturas](/dotnet/core/packages).

Para atualizar o ASP.NET Core:

* Em computadores de desenvolvimento e servidores de compilação: baixe e instale o [SDK do .NET Core](https://www.microsoft.com/net/download).
* Nos servidores de implantação: baixe e instale o [tempo de execução do .NET Core](https://www.microsoft.com/net/download).

 Os aplicativos efetuarão roll forward para a versão mais recente instalada na reinicialização do aplicativo. Não é necessário atualizar o número de versão `Microsoft.AspNetCore.App` no arquivo de projeto. Para obter mais informações, consulte [Roll forward de aplicativos dependentes de estrutura](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward).

Se seu aplicativo tiver usado `Microsoft.AspNetCore.All`, veja [Migração do Microsoft.AspNetCore.All para Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).
