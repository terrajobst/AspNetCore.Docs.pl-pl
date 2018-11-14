---
title: Używane analizatory interfejsu API sieci web
author: pranavkm
description: Więcej informacji o analizatorach interfejsu API sieci web w Microsoft.AspNetCore.Mvc.Api.Analyzers.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/analyzers
ms.openlocfilehash: 89424d89ec2b3125fd3c6b7c86fed2d292b153e6
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635364"
---
# <a name="use-web-api-analyzers"></a><span data-ttu-id="6f643-103">Używane analizatory interfejsu API sieci web</span><span class="sxs-lookup"><span data-stu-id="6f643-103">Use web API analyzers</span></span>

<span data-ttu-id="6f643-104">Platforma ASP.NET Core 2.2 wprowadza [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) pakiet NuGet zawierający analizatory internetowych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="6f643-104">ASP.NET Core 2.2 introduces the [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) NuGet package containing analyzers for web APIs.</span></span> <span data-ttu-id="6f643-105">Analizatory pracować kontrolerów, oznaczony za pomocą <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, podczas kompilowania [konwencje interfejsu API](xref:web-api/advanced/conventions).</span><span class="sxs-lookup"><span data-stu-id="6f643-105">The analyzers work with controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, while building on [API conventions](xref:web-api/advanced/conventions).</span></span>

## <a name="package-installation"></a><span data-ttu-id="6f643-106">Instalacja pakietu</span><span class="sxs-lookup"><span data-stu-id="6f643-106">Package installation</span></span>

<span data-ttu-id="6f643-107">`Microsoft.AspNetCore.Mvc.Api.Analyzers` można dodać jeden z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="6f643-107">`Microsoft.AspNetCore.Mvc.Api.Analyzers` can be added with one of the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6f643-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6f643-108">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6f643-109">Z **Konsola Menedżera pakietów** okna:</span><span class="sxs-lookup"><span data-stu-id="6f643-109">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="6f643-110">Przejdź do **widoku** > **innych Windows** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="6f643-110">Go to **View** > **Other Windows** > **Package Manager Console**.</span></span>
  * <span data-ttu-id="6f643-111">Przejdź do katalogu, w którym *ApiConventions.csproj* plik istnieje.</span><span class="sxs-lookup"><span data-stu-id="6f643-111">Navigate to the directory in which the *ApiConventions.csproj* file exists.</span></span>
  * <span data-ttu-id="6f643-112">Wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="6f643-112">Execute the following command:</span></span>

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

* <span data-ttu-id="6f643-113">Z **Zarządzaj pakietami NuGet** okno dialogowe:</span><span class="sxs-lookup"><span data-stu-id="6f643-113">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="6f643-114">Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6f643-114">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**.</span></span>
  * <span data-ttu-id="6f643-115">Ustaw **źródła pakietu** na stronie "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="6f643-115">Set the **Package source** to "nuget.org".</span></span>
  * <span data-ttu-id="6f643-116">Wprowadź "Microsoft.AspNetCore.Mvc.Api.Analyzers" w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="6f643-116">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
  * <span data-ttu-id="6f643-117">Wybierz pakiet "Microsoft.AspNetCore.Mvc.Api.Analyzers" z **Przeglądaj** kartę, a następnie kliknij przycisk **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="6f643-117">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the **Browse** tab and click **Install**.</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6f643-118">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6f643-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6f643-119">Kliknij prawym przyciskiem myszy *pakietów* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...** .</span><span class="sxs-lookup"><span data-stu-id="6f643-119">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**.</span></span>
* <span data-ttu-id="6f643-120">Ustaw **Dodawanie pakietów** okna **źródła** menu rozwijane "nuget.org".</span><span class="sxs-lookup"><span data-stu-id="6f643-120">Set the **Add Packages** window's **Source** drop-down to "nuget.org".</span></span>
* <span data-ttu-id="6f643-121">Wprowadź "Microsoft.AspNetCore.Mvc.Api.Analyzers" w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="6f643-121">Enter "Microsoft.AspNetCore.Mvc.Api.Analyzers" in the search box.</span></span>
* <span data-ttu-id="6f643-122">Wybierz pakiet "Microsoft.AspNetCore.Mvc.Api.Analyzers" w okienku wyników, a następnie kliknij przycisk **Dodaj pakiet**.</span><span class="sxs-lookup"><span data-stu-id="6f643-122">Select the "Microsoft.AspNetCore.Mvc.Api.Analyzers" package from the results pane and click **Add Package**.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6f643-123">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6f643-123">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6f643-124">Uruchom następujące polecenie z **zintegrowany Terminal**:</span><span class="sxs-lookup"><span data-stu-id="6f643-124">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6f643-125">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6f643-125">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="6f643-126">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="6f643-126">Run the following command:</span></span>

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

## <a name="analyzers-for-api-conventions"></a><span data-ttu-id="6f643-127">Analizatory konwencje interfejsu API</span><span class="sxs-lookup"><span data-stu-id="6f643-127">Analyzers for API conventions</span></span>

<span data-ttu-id="6f643-128">Otwarte dokumenty interfejsu API zawierają kody stanów i typów odpowiedzi, które mogą zwrócić akcji.</span><span class="sxs-lookup"><span data-stu-id="6f643-128">Open API documents contain status codes and response types that an action may return.</span></span> <span data-ttu-id="6f643-129">W programie ASP.NET Core MVC atrybuty takie jak <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> i <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> służą do dokumentowania akcji.</span><span class="sxs-lookup"><span data-stu-id="6f643-129">In ASP.NET Core MVC, attributes such as <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> and <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> are used to document an action.</span></span> <span data-ttu-id="6f643-130"><xref:tutorials/web-api-help-pages-using-swagger> Przechodzi do bardziej szczegółowo w dokumentacji interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="6f643-130"><xref:tutorials/web-api-help-pages-using-swagger> goes into further detail on documenting your API.</span></span>

<span data-ttu-id="6f643-131">Jedną z analizatory w pakiecie sprawdza kontrolerów, oznaczony za pomocą <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> i określa akcje, które nie całkowicie dokumentu ich odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="6f643-131">One of the analyzers in the package inspects controllers annotated with <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> and identifies actions that don't entirely document their responses.</span></span> <span data-ttu-id="6f643-132">Rozważmy następujący przykład:</span><span class="sxs-lookup"><span data-stu-id="6f643-132">Consider the following example:</span></span>

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=9)]

<span data-ttu-id="6f643-133">Poprzednią akcję dokumenty HTTP 200 Powodzenie zwracany typ, ale nie dokumentu kod stanu błędu HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="6f643-133">The preceding action documents the HTTP 200 success return type but doesn't document the HTTP 404 failure status code.</span></span> <span data-ttu-id="6f643-134">Analizator raporty dokumentacji brakuje kod stanu HTTP 404 jako ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="6f643-134">The analyzer reports the missing documentation for the HTTP 404 status code as a warning.</span></span> <span data-ttu-id="6f643-135">Podano opcję, aby rozwiązać ten problem.</span><span class="sxs-lookup"><span data-stu-id="6f643-135">An option to fix the problem is provided.</span></span>
