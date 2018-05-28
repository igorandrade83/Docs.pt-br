---
title: Metapacote Microsoft.AspNetCore.App para ASP.NET Core 2.1 e posterior
author: Rick-Anderson
description: O metapacote Microsoft.AspNetCore.App inclui todos os pacotes do ASP.NET Core e Entity Framework Core compatíveis.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage-app
ms.openlocfilehash: 7c7f69a6176d3f7982a67106cb823ff42200b50e
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/18/2018
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a><span data-ttu-id="14185-103">Metapacote Microsoft.AspNetCore.App para ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="14185-103">Microsoft.AspNetCore.App metapackage for ASP.NET Core 2.1</span></span>

<span data-ttu-id="14185-104">Este recurso exige o ASP.NET Core 2.1 e posterior direcionado ao .NET Core 2.1 e posterior.</span><span class="sxs-lookup"><span data-stu-id="14185-104">This feature requires ASP.NET Core 2.1 and later targeting .NET Core 2.1 and later.</span></span>

<span data-ttu-id="14185-105">O [metapacote](/dotnet/core/packages#metapackages) [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) para ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="14185-105">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [metapackage](/dotnet/core/packages#metapackages) for ASP.NET Core:</span></span>

* <span data-ttu-id="14185-106">Não tem dependências de terceiros, com exceção de [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/) e [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/).</span><span class="sxs-lookup"><span data-stu-id="14185-106">Does not include third-party dependencies except for [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/), and [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/).</span></span> <span data-ttu-id="14185-107">Essas dependências de terceiros são consideradas necessárias para garantir o funcionamento dos principais recursos das estruturas.</span><span class="sxs-lookup"><span data-stu-id="14185-107">These 3rd-party dependencies are deemed necessary to ensure the major frameworks features function.</span></span>
* <span data-ttu-id="14185-108">Inclui todos os pacotes com suporte pela equipe do ASP.NET Core, exceto aqueles que contêm dependências de terceiros (que não sejam aqueles mencionados anteriormente).</span><span class="sxs-lookup"><span data-stu-id="14185-108">Includes all supported packages by the ASP.NET Core team except those that contain third-party dependencies (other than those previously mentioned).</span></span>
* <span data-ttu-id="14185-109">Inclui todos os pacotes com suporte pela equipe do Entity Framework Core, exceto aqueles que contêm dependências de terceiros (que não sejam aqueles mencionados anteriormente).</span><span class="sxs-lookup"><span data-stu-id="14185-109">Includes all supported packages by the Entity Framework Core team except those that contain third-party dependencies (other than those previously mentioned).</span></span>

<span data-ttu-id="14185-110">Todos os recursos do ASP.NET Core 2.1 e posterior e do Entity Framework Core 2.1 e posterior são incluídos no pacote `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="14185-110">All the features of ASP.NET Core 2.1 and later and Entity Framework Core 2.1 and later are included in the `Microsoft.AspNetCore.App` package.</span></span> <span data-ttu-id="14185-111">Os modelos de projeto padrão direcionados para ASP.NET Core 2.1 e posterior usam este pacote.</span><span class="sxs-lookup"><span data-stu-id="14185-111">The default project templates targeting ASP.NET Core 2.1 and later use this package.</span></span> <span data-ttu-id="14185-112">Recomendamos que aplicativos voltados para o ASP.NET Core 2.1 e posterior e o Entity Framework Core 2.1 e posterior usem o pacote `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="14185-112">We recommend applications targeting ASP.NET Core 2.1 and later and Entity Framework Core 2.1 and later use the `Microsoft.AspNetCore.App` package.</span></span>

<span data-ttu-id="14185-113">O número de versão do metapacote `Microsoft.AspNetCore.App` representa a versão do ASP.NET Core e a versão do Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="14185-113">The version number of the `Microsoft.AspNetCore.App` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="14185-114">O uso do metapacote `Microsoft.AspNetCore.App` fornece restrições de versões que protegem seu aplicativo:</span><span class="sxs-lookup"><span data-stu-id="14185-114">Using the `Microsoft.AspNetCore.App` metapackage provides version restrictions that protect your app:</span></span>

* <span data-ttu-id="14185-115">Se um pacote incluído tem uma dependência (não direta) transitiva em um pacote no `Microsoft.AspNetCore.App`, e os números de versão forem diferentes, o NuGet gera um erro.</span><span class="sxs-lookup"><span data-stu-id="14185-115">If a package is included that has a transitive (not direct) dependency on a package in `Microsoft.AspNetCore.App`, and those version numbers differ, NuGet will generate an error.</span></span>
* <span data-ttu-id="14185-116">Outros pacotes adicionados ao seu aplicativo não podem alterar a versão dos pacotes incluídos no `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="14185-116">Other packages added to your app cannot change the version of packages included in `Microsoft.AspNetCore.App`.</span></span>
* <span data-ttu-id="14185-117">A consistência de versão garante uma experiência confiável.</span><span class="sxs-lookup"><span data-stu-id="14185-117">Version consistency ensures a reliable experience.</span></span> <span data-ttu-id="14185-118">`Microsoft.AspNetCore.App` foi projetado para evitar combinações de versão não testado de bits relacionados que estão sendo usados juntos no mesmo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="14185-118">`Microsoft.AspNetCore.App` was designed to prevent untested version combinations of related bits being used together in the same app.</span></span>

<span data-ttu-id="14185-119">Aplicativos que usam o metapacote `Microsoft.AspNetCore.App` aproveitam automaticamente a estrutura compartilhada do ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="14185-119">Applications that use the `Microsoft.AspNetCore.App` metapackage automatically take advantage of the ASP.NET Core shared framework.</span></span> <span data-ttu-id="14185-120">Quando você usa o metapacote `Microsoft.AspNetCore.App`, **nenhum** ativo dos pacotes NuGet do ASP.NET Core referenciados é implantado com o aplicativo &mdash;, porque a estrutura compartilhada do ASP.NET Core contém esses ativos.</span><span class="sxs-lookup"><span data-stu-id="14185-120">When you use the `Microsoft.AspNetCore.App` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the ASP.NET Core shared framework contains these assets.</span></span> <span data-ttu-id="14185-121">Os ativos na estrutura compartilhada são pré-compilados para melhorar o tempo de inicialização do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="14185-121">The assets in the shared framework are precompiled to improve application startup time.</span></span> <span data-ttu-id="14185-122">Para obter mais informações, veja "estrutura compartilhada" em [Empacotamento de distribuição do .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="14185-122">For more information, see "shared framework" in [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging).</span></span>

<span data-ttu-id="14185-123">O seguinte arquivo de projeto *.csproj* referencia os metapacotes `Microsoft.AspNetCore.App` para o ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="14185-123">The following *.csproj* project file references the `Microsoft.AspNetCore.App` metapackage for ASP.NET Core:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>

```

<span data-ttu-id="14185-124">A marcação anterior representa um modelo típico de ASP.NET Core 2.1 e posterior.</span><span class="sxs-lookup"><span data-stu-id="14185-124">The preceding markup represents a typical ASP.NET Core 2.1 and later template.</span></span> <span data-ttu-id="14185-125">Ela não especifica um número de versão para a referência de pacote `Microsoft.AspNetCore.App`.</span><span class="sxs-lookup"><span data-stu-id="14185-125">It doesn't specify a version number for the `Microsoft.AspNetCore.App` package reference.</span></span> <span data-ttu-id="14185-126">Quando a versão não for especificada, uma versão [implícita](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md) será especificada pelo SDK, ou seja, `Microsoft.NET.Sdk.Web`.</span><span class="sxs-lookup"><span data-stu-id="14185-126">When the version is not specified, an [implicit](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md) version is specified by the SDK, that is, `Microsoft.NET.Sdk.Web`.</span></span> <span data-ttu-id="14185-127">Recomendamos que você conte com a versão implícita especificada pelo SDK, e não defina explicitamente o número de versão na referência de pacote.</span><span class="sxs-lookup"><span data-stu-id="14185-127">We recommend relying on the implicit version specified by the SDK and not explicitly setting the version number on the package reference.</span></span> <span data-ttu-id="14185-128">Você pode deixar um comentário do GitHub em [Discussão sobre a versão implícita Microsoft.AspNetCore.App](https://github.com/aspnet/Docs/issues/6430).</span><span class="sxs-lookup"><span data-stu-id="14185-128">You can leave a GitHub comment at [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/Docs/issues/6430).</span></span>

<span data-ttu-id="14185-129">A versão implícita é definida como `major.minor.0` para aplicativos portátil.</span><span class="sxs-lookup"><span data-stu-id="14185-129">The implicit version is set to `major.minor.0` for portable apps.</span></span> <span data-ttu-id="14185-130">O mecanismo de roll forward estrutura compartilhada executará o aplicativo na versão compatível mais recente entre as estruturas compartilhadas instaladas.</span><span class="sxs-lookup"><span data-stu-id="14185-130">The shared framework roll-forward mechanism will run the app on the latest compatible version among the installed shared frameworks.</span></span> <span data-ttu-id="14185-131">Para garantir que a mesma versão seja usada no desenvolvimento, no teste e na produção, certifique-se de que a mesma versão da estrutura compartilhada seja instalada em todos os ambientes.</span><span class="sxs-lookup"><span data-stu-id="14185-131">To guarantee the same version is used in development, test, and production, ensure the same version of the shared framework is installed in all environments.</span></span> <span data-ttu-id="14185-132">Para aplicativos independentes, o número de versão implícita é definido como `major.minor.patch` da estrutura compartilhada incluída no SDK instalado.</span><span class="sxs-lookup"><span data-stu-id="14185-132">For self contained apps, the implicit version number is set to the `major.minor.patch` of the shared framework bundled in the installed SDK.</span></span>

<span data-ttu-id="14185-133">Especificar um número de versão na referência `Microsoft.AspNetCore.App` **não** garante que a versão da estrutura compartilhada será escolhida.</span><span class="sxs-lookup"><span data-stu-id="14185-133">Specifying a version number on the `Microsoft.AspNetCore.App` reference does **not** guarantee that version of the shared framework will be chosen.</span></span> <span data-ttu-id="14185-134">Por exemplo, suponha que a versão "2.1.1" foi especificada, mas "2.1.3" está instalada.</span><span class="sxs-lookup"><span data-stu-id="14185-134">For example, suppose version "2.1.1" is specified, but "2.1.3" is installed.</span></span> <span data-ttu-id="14185-135">Nesse caso, o aplicativo usará "2.1.3".</span><span class="sxs-lookup"><span data-stu-id="14185-135">In that case, the app will use "2.1.3".</span></span> <span data-ttu-id="14185-136">Embora não seja recomendado, você pode desabilitar o roll forward (patch e/ou secundária).</span><span class="sxs-lookup"><span data-stu-id="14185-136">Although not recommended, you can disable roll forward (patch and/or minor).</span></span> <span data-ttu-id="14185-137">Para obter mais informações sobre como efetuar roll forward do host dotnet e como configurar seu comportamento, veja [Efetuar roll forward do host dotnet](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span><span class="sxs-lookup"><span data-stu-id="14185-137">For more information regarding dotnet host roll-forward and how to configure its behavior, see [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span></span>

<span data-ttu-id="14185-138">O `Microsoft.AspNetCore.App` [metapacote](/dotnet/core/packages#metapackages) não é um pacote tradicional que é atualizado do NuGet.</span><span class="sxs-lookup"><span data-stu-id="14185-138">The `Microsoft.AspNetCore.App` [metapackage](/dotnet/core/packages#metapackages) is not a traditional package that is updated from NuGet.</span></span> <span data-ttu-id="14185-139">Semelhante ao `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` representa um tempo de execução compartilhado, que tem semântica de controle de versão especial tratada fora do NuGet.</span><span class="sxs-lookup"><span data-stu-id="14185-139">Similar to `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` represents a shared runtime, which has special versioning semantics handled outside of NuGet.</span></span> <span data-ttu-id="14185-140">Para obter mais informações, veja [Pacotes, metapacotes e estruturas](/dotnet/core/packages).</span><span class="sxs-lookup"><span data-stu-id="14185-140">For more information, see [Packages, metapackages and frameworks](/dotnet/core/packages).</span></span>

<span data-ttu-id="14185-141">Se seu aplicativo tiver usado `Microsoft.AspNetCore.All`, veja [Migração do Microsoft.AspNetCore.All para Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).</span><span class="sxs-lookup"><span data-stu-id="14185-141">If your application previously used `Microsoft.AspNetCore.All`, see [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).</span></span>