---
uid: web-pages/readme/beta3
title: Sieci Web macierzy i plik Readme programu ASP.NET w wersji Beta 3 stron sieci Web (Razor) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Macierzy sieci Web i plik Readme sieci Web ASP.NET w wersji Beta 3 stron (Razor)
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/10/2011
ms.topic: article
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: 5ef7a6f44758cf94fc19d6fbab3cc4b7bce8e8e5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892884"
---
<a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="09ea5-103">Macierzy sieci Web i plik Readme sieci Web ASP.NET w wersji Beta 3 stron (Razor)</span><span class="sxs-lookup"><span data-stu-id="09ea5-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>
====================
> <span data-ttu-id="09ea5-104">Macierzy sieci Web i plik Readme sieci Web ASP.NET w wersji Beta 3 stron (Razor)</span><span class="sxs-lookup"><span data-stu-id="09ea5-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="09ea5-105">9 listopada 2010</span><span class="sxs-lookup"><span data-stu-id="09ea5-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="09ea5-106">Spis treści</span><span class="sxs-lookup"><span data-stu-id="09ea5-106">Contents</span></span>

- [<span data-ttu-id="09ea5-107">Omówienie</span><span class="sxs-lookup"><span data-stu-id="09ea5-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="09ea5-108">Instalacja</span><span class="sxs-lookup"><span data-stu-id="09ea5-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="09ea5-109">Nowe funkcje, zmiany i znane problemy w wersji Beta 3</span><span class="sxs-lookup"><span data-stu-id="09ea5-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="09ea5-110">Problemy z instalacją programu WebMatrix</span><span class="sxs-lookup"><span data-stu-id="09ea5-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="09ea5-111">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="09ea5-111">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="09ea5-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="09ea5-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="09ea5-113">Instalowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="09ea5-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="09ea5-114">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="09ea5-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="09ea5-115">Inne problemy</span><span class="sxs-lookup"><span data-stu-id="09ea5-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="09ea5-116">Aby uzyskać więcej informacji</span><span class="sxs-lookup"><span data-stu-id="09ea5-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="09ea5-117">Omówienie</span><span class="sxs-lookup"><span data-stu-id="09ea5-117">Overview</span></span>

> <span data-ttu-id="09ea5-118">Microsoft WebMatrix Beta jest stos programowanie wolnej sieci web, który instaluje w minutach.</span><span class="sxs-lookup"><span data-stu-id="09ea5-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="09ea5-119">Serwer sieci web jest zintegrowany z bazy danych i programowania platformy do utworzenia jednym zintegrowanym interfejsie.</span><span class="sxs-lookup"><span data-stu-id="09ea5-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="09ea5-120">Program WebMatrix w wersji Beta umożliwia upraszcza sposób kodu, testowania i publikowanie własnych sieci Web ASP.NET i PHP, lub można użyć programu WebMatrix w wersji Beta można uruchomić nowej witryny sieci Web przy użyciu popularnych aplikacji open source, takich jak DotNetNuke, Umbraco, WordPress lub Joomla.</span><span class="sxs-lookup"><span data-stu-id="09ea5-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="09ea5-121">Program WebMatrix w wersji Beta używa tego samego zaawansowanego serwera, aparatu bazy danych i środowiska struktur, które zostanie uruchomione witryny sieci Web w Internecie, dzięki czemu przejście od projektowania do produkcji płynne i bezproblemowe.</span><span class="sxs-lookup"><span data-stu-id="09ea5-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="09ea5-122">Instalacja</span><span class="sxs-lookup"><span data-stu-id="09ea5-122">Installation</span></span>

> <span data-ttu-id="09ea5-123">Aby zainstalować program WebMatrix w wersji Beta 3, należy użyć [3.0 Instalatora platformy sieci Web Microsoft](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="09ea5-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="09ea5-124">Po zainstalowaniu Instalatora platformy sieci Web, można ją zainstalować program WebMatrix w wersji Beta 3.</span><span class="sxs-lookup"><span data-stu-id="09ea5-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="09ea5-125">Jeśli masz problemy podczas instalacji, zapoznaj się [Rozwiązywanie problemów dotyczących Instalatora platformy sieci Web firmy Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="09ea5-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="09ea5-126">Instrukcje dotyczące publikowania aplikacji</span><span class="sxs-lookup"><span data-stu-id="09ea5-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="09ea5-127">Zobacz [instrukcje krok po kroku dotyczące publikowania aplikacji](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="09ea5-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="09ea5-128">Nowe funkcje i zmiany, problemy andKnown</span><span class="sxs-lookup"><span data-stu-id="09ea5-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="09ea5-129">Instalacja wersji Beta programu WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="09ea5-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="09ea5-130">Problem: Program WebMatrix w wersji Beta 3 jest dostępna tylko na platformach, które obsługuje program Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="09ea5-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="09ea5-131">.NET Framework w wersji 4 jest wymagany dla programu WebMatrix w wersji Beta.</span><span class="sxs-lookup"><span data-stu-id="09ea5-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="09ea5-132">W niektórych przypadkach Instalator programu WebMatrix w wersji Beta umożliwią spróbuje zainstalować na platformie, która nie wchodzi w skład zestawu obsługiwanej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="09ea5-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="09ea5-133">W szczególności Windows Vista bez dodatku SP1 dla aktualizacji będzie można rozpocząć instalację programu WebMatrix w wersji Beta, ale składników .NET Framework 4 spowoduje niepowodzenie i zablokowanie instalacji.</span><span class="sxs-lookup"><span data-stu-id="09ea5-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="09ea5-134">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-134">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-135">Zainstaluj na obsługiwanych platform, w tym:</span><span class="sxs-lookup"><span data-stu-id="09ea5-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="09ea5-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="09ea5-136">Windows 7</span></span>
> - <span data-ttu-id="09ea5-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="09ea5-137">Windows Server 2008</span></span>
> - <span data-ttu-id="09ea5-138">Windows Server 2008 z dodatkiem R2</span><span class="sxs-lookup"><span data-stu-id="09ea5-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="09ea5-139">Windows Vista z dodatkiem SP1 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="09ea5-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="09ea5-140">Windows XP z dodatkiem SP3</span><span class="sxs-lookup"><span data-stu-id="09ea5-140">Windows XP SP3</span></span>
> - <span data-ttu-id="09ea5-141">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="09ea5-141">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="09ea5-142">Problem: Nie można zainstalować program WebMatrix w wersji Beta 3, jeśli zainstalowano program Microsoft Visual Studio 2008 bez programu Microsoft Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="09ea5-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="09ea5-143">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-143">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-144">Zainstaluj [programu Microsoft Visual Studio 2008 z dodatkiem SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) z Centrum pobierania Microsoft.</span><span class="sxs-lookup"><span data-stu-id="09ea5-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="09ea5-145">Problem: Niektóre zestawy dla programu SQL Server Compact 4.0 nie są zainstalowane w GAC</span><span class="sxs-lookup"><span data-stu-id="09ea5-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="09ea5-146">Podczas instalowania programu SQL Server Compact 4.0 na komputerze 64-bitowy komputer ma tylko .NET Framework 3.5 SP1 Client Profile zainstalowany zarządzanych zestawów dla programu SQL Server Compact 4.0 nie są umieszczane w globalnej pamięci podręcznej zestawów (GAC).</span><span class="sxs-lookup"><span data-stu-id="09ea5-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="09ea5-147">Zarządzanych zestawów, które nie są zainstalowane w GAC są:</span><span class="sxs-lookup"><span data-stu-id="09ea5-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="09ea5-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span><span class="sxs-lookup"><span data-stu-id="09ea5-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="09ea5-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span><span class="sxs-lookup"><span data-stu-id="09ea5-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="09ea5-150">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-150">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-151">Odinstaluj program SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="09ea5-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="09ea5-152">Pobierz i zainstaluj pełną wersję programu .NET Framework 3.5 z dodatkiem SP1 z następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="09ea5-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="09ea5-153">Microsoft .NET Framework 3.5 z dodatkiem Service pack 1 (pełny pakiet)</span><span class="sxs-lookup"><span data-stu-id="09ea5-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="09ea5-154">Następnie ponownie zainstalować program SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="09ea5-154">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="09ea5-155">Problem: Nie można odinstalować za pomocą wiersza polecenia programu SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="09ea5-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="09ea5-156">Dezinstalacja przy użyciu opcji wiersza polecenia programu SQL Server Compact nie działa w tej wersji.</span><span class="sxs-lookup"><span data-stu-id="09ea5-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="09ea5-157">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-157">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-158">Użyj *programy i funkcje* w Panelu sterowania systemu Windows, aby odinstalować program Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="09ea5-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="09ea5-159">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="09ea5-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="09ea5-160">W tej sekcji dokumentu opisano nowe funkcje, zmiany i znane problemy z wersji Beta 3 programu ASP.NET Web Pages o składni Razor.</span><span class="sxs-lookup"><span data-stu-id="09ea5-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="09ea5-161">Nowe funkcje</span><span class="sxs-lookup"><span data-stu-id="09ea5-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="09ea5-162">Zmiany</span><span class="sxs-lookup"><span data-stu-id="09ea5-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="09ea5-163">Problemy</span><span class="sxs-lookup"><span data-stu-id="09ea5-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="09ea5-164">Nowe funkcje w wersji Beta 3 dla stron ASP.NET Web Pages o składni Razor</span><span class="sxs-lookup"><span data-stu-id="09ea5-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="09ea5-165">Nowy: Metoda "Html.Raw" renderuje niekodowany kod znaczników</span><span class="sxs-lookup"><span data-stu-id="09ea5-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="09ea5-166">Nowe `Html.Raw` metoda umożliwia renderowania kodu znaczników HTML jako kod znaczników zamiast renderowania zakodowanego wyjścia.</span><span class="sxs-lookup"><span data-stu-id="09ea5-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="09ea5-167">(Domyślnie ASP.NET Razor koduje ciągi przed ich renderowaniem.) Składnia jest następująca:</span><span class="sxs-lookup"><span data-stu-id="09ea5-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="09ea5-168">Poniższy przykład przedstawia użycie `Html.Raw`:</span><span class="sxs-lookup"><span data-stu-id="09ea5-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]


<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="09ea5-169">Zmiany w wersji Beta 3 dla stron ASP.NET Web Pages o składni Razor</span><span class="sxs-lookup"><span data-stu-id="09ea5-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="09ea5-170">Zmień: Metoda "HrefAttribute" usunięte</span><span class="sxs-lookup"><span data-stu-id="09ea5-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="09ea5-171">`HrefAttribute` Metody `WebPage` klasy został usunięty.</span><span class="sxs-lookup"><span data-stu-id="09ea5-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="09ea5-172">Tego pomocnika użytego do kodowania znaków niebezpieczny w adresach URL.</span><span class="sxs-lookup"><span data-stu-id="09ea5-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="09ea5-173">Nie jest już wymagane, ponieważ ASP.NET Razor automatycznie koduje ciągi.</span><span class="sxs-lookup"><span data-stu-id="09ea5-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="09ea5-174">(Nowe `Html.Raw` metody do renderowania niekodowany ciągów.)</span><span class="sxs-lookup"><span data-stu-id="09ea5-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>


#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="09ea5-175">Zmiany: Składnia deklaratywne "@helper" pomocników zmienione</span><span class="sxs-lookup"><span data-stu-id="09ea5-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="09ea5-176">W wersji Beta 3 programu ASP.NET zmienia sposób analizuje wątków, które są tworzone przy użyciu `@helper` składni.</span><span class="sxs-lookup"><span data-stu-id="09ea5-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="09ea5-177">W zasadzie `@helper` składni teraz być analizowana jako blok kodu a nie jako kod znaczników, który może zawierać kod w bloku.</span><span class="sxs-lookup"><span data-stu-id="09ea5-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="09ea5-178">W związku z tym kodu wewnątrz elementu pomocniczego nie musi być ujęta w `@{ }` bloków.</span><span class="sxs-lookup"><span data-stu-id="09ea5-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="09ea5-179">Z drugiej strony, znaczników wewnątrz elementu pomocniczego musi być jawnie uwzględnione w elementów HTML lub ASP.NET Razor `<text></text>` tagów.</span><span class="sxs-lookup"><span data-stu-id="09ea5-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="09ea5-180">Na przykład następująca `@helper` składni działa w wersji Beta 3:</span><span class="sxs-lookup"><span data-stu-id="09ea5-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="09ea5-181">W wersji Beta 3 tego pomocnika należy zmienić, aby wyglądały jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="09ea5-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="09ea5-182">Zwróć uwagę, że `@{ }` znaków wokół początkowej kodu pomocnika nie jest już używana.</span><span class="sxs-lookup"><span data-stu-id="09ea5-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="09ea5-183">Jest to spowodowane zawartość pomocników są traktowane jako blok kodu domyślnie.</span><span class="sxs-lookup"><span data-stu-id="09ea5-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="09ea5-184">Pomocnik renderuje kod znaczników, który rozpoczyna się od otwarcia `<a>` tagu.</span><span class="sxs-lookup"><span data-stu-id="09ea5-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="09ea5-185">Jeśli pomocnika musi renderowania zwykłego tekstu lub tagi, które nie ma tagu zamykającego (na przykład `<meta>` tagów), musi należeć do renderowania zawartości `<text></text>` tagów.</span><span class="sxs-lookup"><span data-stu-id="09ea5-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>


#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="09ea5-186">Change: "WebPageContext.HttpContext" removed</span><span class="sxs-lookup"><span data-stu-id="09ea5-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="09ea5-187">`WebPageContext.HttpContext` Właściwość została usunięta.</span><span class="sxs-lookup"><span data-stu-id="09ea5-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="09ea5-188">Zamiast nich należy używać słów kluczowych `HttpContext.Current`.</span><span class="sxs-lookup"><span data-stu-id="09ea5-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="09ea5-189">( `WebPageContext.HttpContext` Właściwości po prostu to opakowana.)</span><span class="sxs-lookup"><span data-stu-id="09ea5-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>


#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="09ea5-190">Zmień: "Facebook" pomocnika przeniesiona do nowego pakietu</span><span class="sxs-lookup"><span data-stu-id="09ea5-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="09ea5-191">`Facebook` Pomocnika został przeniesiony do *Facebook.Helper* biblioteki, która obejmuje `Facebook` pomocnika i dodatkowe funkcje.</span><span class="sxs-lookup"><span data-stu-id="09ea5-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="09ea5-192">Należy zainstalować tę bibliotekę jako osobny pakiet, zgodnie z opisem w "Instalowanie pomocników Menedżera pakietów" samouczka [wprowadzenie stron ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).</span><span class="sxs-lookup"><span data-stu-id="09ea5-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>


#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="09ea5-193">Zmień: Typy członkostwo roli i zabezpieczeń przenosi do nowego zestawu</span><span class="sxs-lookup"><span data-stu-id="09ea5-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="09ea5-194">Następujące typy zostały przeniesione do `WebMatrix.WebData` zestawu:</span><span class="sxs-lookup"><span data-stu-id="09ea5-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`


#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="09ea5-195">Zmiany: Przeniesione do zestawu System.Web.WebPages.dll klasy "TagBuilder"</span><span class="sxs-lookup"><span data-stu-id="09ea5-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="09ea5-196">`TagBuilde` Klasy r został przeniesiony do zestawu System.Web.WebPages.dll.</span><span class="sxs-lookup"><span data-stu-id="09ea5-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="09ea5-197">Wcześniej to w zestawie, do którego jest częścią programu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="09ea5-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="09ea5-198">Ta zmiana oznacza, że jest konieczne instalowanie platformy ASP.NET MVC, aby można było używać `TagBuilder` klasy.</span><span class="sxs-lookup"><span data-stu-id="09ea5-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="09ea5-199">Klasa jest jednak nadal `System.Web.Mvc` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="09ea5-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="09ea5-200">Aby można było używać `TagBuilder` klasy (na przykład w niestandardowych ASP.NET Razor pomocnika), odwołanie do przestrzeni nazw (na przykład, dodając `@using System.Web.Mvc` kodu).</span><span class="sxs-lookup"><span data-stu-id="09ea5-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>


#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="09ea5-201">Zmiany: Składnia sprawdzania poprawności zostały zmienione; żądanie Klasa "Weryfikacja" usunięte</span><span class="sxs-lookup"><span data-stu-id="09ea5-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="09ea5-202">W wersji Beta 3, aby wyłączyć sprawdzanie poprawności dla poszczególnych pól lub zestaw pól, można wywołać `Validation.Exclude` metody, przekazując nazwy pola do wykluczenia z weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="09ea5-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="09ea5-203">Nowej składni jest dostępna w wersji Beta 3 dla pomijanie sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="09ea5-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="09ea5-204">`Validation` Metodę używaną w wersji Beta 3 został usunięty.</span><span class="sxs-lookup"><span data-stu-id="09ea5-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="09ea5-205">Jeśli nie można wyłączyć sprawdzania poprawności żądania, jeśli użytkownicy spróbuj przekazać kod znaczników HTML (na przykład za pomocą edytora tekstu sformatowanego, na stronie), witryna sieci Web wyświetli komunikat o błędzie, takich jak *wykryto potencjalnie niebezpieczną wartość Request.Form z klienta*i dane wejściowe użytkownika nie zostanie zaakceptowany.</span><span class="sxs-lookup"><span data-stu-id="09ea5-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="09ea5-206">Jeśli wyłączysz weryfikację żądań, należy ręcznie sprawdzić danych wejściowych użytkownika, aby upewnić się, że nie zawierają potencjalnie niebezpiecznego kodu znaczników lub skrypt, za pomocą wyglądać mniej więcej tak [skryptów V4.0 biblioteki Microsoft przed granic lokacji](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span><span class="sxs-lookup"><span data-stu-id="09ea5-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="09ea5-207">Aby wyłączyć automatyczne żądania weryfikacji, należy wywołać `Request.Unvalidated` metody przekazanie jej nazwę pola lub innego obiektu post, który chcesz pominąć weryfikacji żądania.</span><span class="sxs-lookup"><span data-stu-id="09ea5-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="09ea5-208">Ta metoda służy do pominięcia weryfikacji dla wszystkich elementów w `Form`, `QueryString`, `Cookies`, i `ServerVariables` kolekcji.</span><span class="sxs-lookup"><span data-stu-id="09ea5-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="09ea5-209">W poniższych przykładach pokazano, jak używać `Unvalidated` metody:</span><span class="sxs-lookup"><span data-stu-id="09ea5-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]


<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="09ea5-210">Znane problemy dotyczące stron ASP.NET Web Pages o składni Razor</span><span class="sxs-lookup"><span data-stu-id="09ea5-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="09ea5-211">Problem: Nieoczekiwanego zachowania w przypadku używania tabeli użytkownika niestandardowego dla członkostwa</span><span class="sxs-lookup"><span data-stu-id="09ea5-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="09ea5-212">Aby zainicjować dostawcy członkostwa dla witryny sieci Web platformy ASP.NET Razor, należy wywołać `WebSecurity.InitializeDatabaseConnection` metody.</span><span class="sxs-lookup"><span data-stu-id="09ea5-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="09ea5-213">(W programie WebMatrix, w szablonie witryny początkowej zawiera wywołanie tej metody w  *\_AppStart.cshtml* pliku.) Jeśli `autoCreateTables` parametr tej metody jest ustawiony na wartość true (domyślnie jest ustawiona wartość true w szablonie witryny początkowej), i jeśli nazwa tabeli nierozpoznany jest przekazywany do metody (drugi parametr), metoda nie zgłasza błąd.</span><span class="sxs-lookup"><span data-stu-id="09ea5-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="09ea5-214">Zamiast tego automatycznie tworzy tabeli.</span><span class="sxs-lookup"><span data-stu-id="09ea5-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="09ea5-215">Może to być spowodowane problemem, jeśli zamierzasz używać tabeli użytkownika niestandardowego dla członkostwa, ale przekazywania do nazwy tabeli niewłaściwy `WebSecurity.InitializeDatabaseConnection` metody.</span><span class="sxs-lookup"><span data-stu-id="09ea5-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="09ea5-216">Ponieważ metoda nie domyślnie Zgłoś błąd, jeśli tabela, które określisz nie istnieje, a zamiast tego tworzy nową tabelę, aplikacja może się pojawić się działać.</span><span class="sxs-lookup"><span data-stu-id="09ea5-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="09ea5-217">Jednak kod aplikacji, która opiera się na tabeli użytkownika niestandardowego (i pól w nim) po pewnym czasie mogą nie być z nieoczekiwane błędy.</span><span class="sxs-lookup"><span data-stu-id="09ea5-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="09ea5-218">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-218">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-219">Upewnij się, że nazwa przekazano `InitializeDatabaseConnection` zgodna metoda profilu użytkownika tabeli w bazie danych członkostwa lub upewnij się, że `autoCreateTables` parametr ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="09ea5-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="09ea5-220">Błąd "Nie można wygenerować wystąpienia użytkownika programu SQL Server" problemu:</span><span class="sxs-lookup"><span data-stu-id="09ea5-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="09ea5-221">Jeśli aplikacja sieci Web programu WebMatrix korzysta z programu SQL Server Express i działa IIS 7.5 w systemie Windows 7 lub Windows Server 2008 R2, można napotkać komunikat o błędzie wskazujący, że programu SQL Server nie można pobrać ścieżki lokalnej aplikacji użytkownika w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="09ea5-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="09ea5-222">**Obejście** upewnij się, czy konto systemu Windows działającą aplikację (zazwyczaj Usługa sieciowa) ma uprawnienia odczytu/zapisu dla folderów głównych aplikacji i podfoldery takich jak *aplikacji\_danych*.</span><span class="sxs-lookup"><span data-stu-id="09ea5-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="09ea5-223">Bardziej szczegółowe informacje są dostępne w artykule bazy wiedzy [problemów z wystąpień użytkownika programu SQL Server Express i projekty aplikacji sieci Web ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="09ea5-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="09ea5-224">Problem: W programie Visual Studio, przestrzeni nazw dla zestawów niestandardowych (dll) nie są importowane automatycznie</span><span class="sxs-lookup"><span data-stu-id="09ea5-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="09ea5-225">Jeśli używasz zestawów niestandardowych w projekcie w programie Visual Studio, przestrzeni nazw, zadeklarowanej w tych zestawów nie są automatycznie importowane w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="09ea5-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="09ea5-226">W związku z tym odwołania do typów niestandardowych mogą nie być rozpoznawane w czasie projektowania i są oznaczone jako nie został rozpoznany. w programie Visual Studio (przy użyciu "wężyk").</span><span class="sxs-lookup"><span data-stu-id="09ea5-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="09ea5-227">Ten problem występuje tylko w czasie projektowania w programie Visual Studio; sama aplikacja działa prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="09ea5-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="09ea5-228">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-228">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-229">Obejmują `using` instrukcji (`imports` w języku Visual Basic), która odwołuje się jednostek, które nie są rozpoznawane w czasie projektowania.</span><span class="sxs-lookup"><span data-stu-id="09ea5-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="09ea5-230">Problem: Visual Studio IntelliSense projektu szablonów i dostępna tylko na platformie ASP.NET MVC w wersji 3</span><span class="sxs-lookup"><span data-stu-id="09ea5-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="09ea5-231">Instalowanie składnika ASP.NET Web Pages nie również zainstalować narzędzi dla programu Visual Studio takie jak szablony IntelliSense i projektu dla aplikacji ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="09ea5-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="09ea5-232">**Obejście** Aby używać szablonów IntelliSense i projektu dla aplikacji ASP.NET Web Pages w programie Visual Studio, zainstalować platformy ASP.NET MVC 3 RC albo za pomocą Instalatora platformy sieci Web lub [autonomicznego Instalatora](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="09ea5-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="09ea5-233">Problem: "&lt;Pomocnika&gt; nie można odnaleźć klasy" Błąd</span><span class="sxs-lookup"><span data-stu-id="09ea5-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="09ea5-234">Po uaktualnieniu do wersji Beta 3, zostanie wyświetlony błąd który klasę pomocy (na przykład `Facebook` klasy) nie można odnaleźć.</span><span class="sxs-lookup"><span data-stu-id="09ea5-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="09ea5-235">Począwszy od wersji Beta 2 oraz kontynuowanie w wersji Beta 3, pomocników zostały przeniesione do pakietów, które jawnie należy zainstalować.</span><span class="sxs-lookup"><span data-stu-id="09ea5-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="09ea5-236">Aby uwzględnić te pakiety; nie są uaktualnieni istniejących lokacji dotyczy to również lokacji w *\My Documents\IISExpress* lub *\My Documents\My witryn sieci Web* folderów.</span><span class="sxs-lookup"><span data-stu-id="09ea5-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="09ea5-237">W szczególności zostanie wyświetlony ten błąd, użycie domyślnej witryny w *witryny* (WebSite1), który zawiera odwołanie do `Twitter` pomocnika.</span><span class="sxs-lookup"><span data-stu-id="09ea5-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="09ea5-238">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-238">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-239">Komentarz dla wywołań dowolnej pomocników w lokacji, uruchom  *\_Admin* strony i zainstaluj pakiet lub pakiety zawierające wątków, które chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="09ea5-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="09ea5-240">Po zainstalowaniu pakietu można usuń znaczniki komentarza wierszy, które odwołują się pomocników.</span><span class="sxs-lookup"><span data-stu-id="09ea5-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>


#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="09ea5-241">Problem: Wdrażanie zestawów w wersji Beta 3 ASP.NET Razor do folderu Bin może nie działać na witrynami</span><span class="sxs-lookup"><span data-stu-id="09ea5-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="09ea5-242">W przypadku wdrożenia do hostowania lokacji witryny sieci Web platformy ASP.NET Web Pages i zestawy ASP.NET Razor w wersji Beta 3 w przypadku wdrożenia w witrynie *Bin* folderu, mogą wystąpić błędy, takie jak:</span><span class="sxs-lookup"><span data-stu-id="09ea5-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="09ea5-243">Może to nastąpić, jeśli dostawca hostingu zainstalował zestawy ASP.NET Web Pages Beta 1 w pamięci podręcznej globalnego aplikacji serwera (GAC).</span><span class="sxs-lookup"><span data-stu-id="09ea5-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="09ea5-244">Zestawy w pamięci GAC uzyskać pierwszeństwo nad zestawów zainstalowanych lokalnie w *Bin* folderu.</span><span class="sxs-lookup"><span data-stu-id="09ea5-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="09ea5-245">**Obejście** skontaktuj się z dostawcą hostingu, aby potwierdzić, czy błędy pojawia się z powodu konfliktu między wersjami dostawcy zestawów i należy do Ciebie.</span><span class="sxs-lookup"><span data-stu-id="09ea5-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="09ea5-246">Jeśli tak, żądanie, że dostawca hostingu zaktualizować zestawów w pamięci podręcznej GAC serwera.</span><span class="sxs-lookup"><span data-stu-id="09ea5-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="09ea5-247">Problem: Odczytywanie źródła danych lub innych danych zewnętrznych za pośrednictwem serwera proxy</span><span class="sxs-lookup"><span data-stu-id="09ea5-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="09ea5-248">Jeśli serwer z systemem lokacji znajduje się za serwerem proxy, może być konieczne skonfigurowanie informacje dotyczące serwera proxy w *Web.config* pliku, aby można było odczytać informacje, które pochodzą z poza witryną.</span><span class="sxs-lookup"><span data-stu-id="09ea5-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="09ea5-249">Na przykład, jeśli używasz `ReCaptcha` pomocnika, pomocnika komunikuje się z usługą reCAPTCHA, ale może zostać zablokowany przez serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="09ea5-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="09ea5-250">Podobnie źródła danych, które są używane w programie ASP.NET Web Pages, np. źródła danych używane przez Menedżera pakietów, mogą wymagać konfiguracji serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="09ea5-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="09ea5-251">Jeśli masz problemy w pracy z zewnętrznej usługi lub Praca z pakietem źródła danych należy umieścić następujące elementy w katalogu głównego aplikacji *Web.config* pliku:</span><span class="sxs-lookup"><span data-stu-id="09ea5-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="09ea5-252">Aby uzyskać więcej informacji na temat konfigurowania serwera proxy, zobacz [ &lt;proxy&gt; elementu (ustawienia sieciowe)](https://msdn.microsoft.com/library/sa91de1e.aspx) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="09ea5-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="09ea5-253">Problemu: Błąd "Nie można załadować Microsoft.Web.Infrastructure.dll"</span><span class="sxs-lookup"><span data-stu-id="09ea5-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="09ea5-254">Jeśli wcześniej zainstalowana wersja Beta 1 programu ASP.NET Web Pages o składni Razor, a następnie zainstalować wersję Beta 3, wszystkie odpowiednie zestawy są zainstalowane w GAC, z wyjątkiem *Microsoft.Web.Infrastructure.dll*.</span><span class="sxs-lookup"><span data-stu-id="09ea5-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="09ea5-255">W rezultacie, po uruchomieniu ASP.NET Razor strony, zostanie wyświetlony błąd wskazuje, że *Microsoft.Web.Infrastructure.dll* nie można go załadować.</span><span class="sxs-lookup"><span data-stu-id="09ea5-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="09ea5-256">Ten problem nie występuje po załadowaniu w wersji Beta 3 na oczyszczonym komputerze.</span><span class="sxs-lookup"><span data-stu-id="09ea5-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="09ea5-257">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-257">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-258">W Panelu sterowania odinstaluj stron ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="09ea5-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="09ea5-259">Zainstaluj ponownie w wersji Beta 3.</span><span class="sxs-lookup"><span data-stu-id="09ea5-259">Then reinstall the Beta 3 release.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="09ea5-260">Problem: Odinstalowywanie programu .NET Framework w wersji 4 wyłącza stron ASP.NET Web Pages o składni Razor</span><span class="sxs-lookup"><span data-stu-id="09ea5-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="09ea5-261">Po odinstalowaniu programu .NET Framework w wersji 4 i zainstaluj go ponownie, ASP.NET Web Pages o składni Razor jest wyłączona.</span><span class="sxs-lookup"><span data-stu-id="09ea5-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="09ea5-262">Strony z *.cshtml* rozszerzenia nie działać prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="09ea5-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="09ea5-263">Strony ASP.NET Web Pages rejestruje zestawu w folderze głównym maszyny *Web.config* plików i usunięcie programu .NET Framework spowoduje usunięcie tego pliku.</span><span class="sxs-lookup"><span data-stu-id="09ea5-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="09ea5-264">Ponowna instalacja programu .NET Framework instaluje nową wersję pliku konfiguracji, ale nie dodać odwołanie do zestawu stron sieci Web programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="09ea5-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="09ea5-265">**Obejście** po ponownym zainstalowaniu programu .NET Framework, zainstaluj ponownie stron ASP.NET Web Pages o składni Razor.</span><span class="sxs-lookup"><span data-stu-id="09ea5-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="09ea5-266">Spowoduje to dodanie następujący element do *Web.config* w katalogu głównym komputera, który zazwyczaj znajduje się w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="09ea5-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]


#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="09ea5-267">Problem: Aplikacje wdrożone wcześniej z zestawami ASP.NET z folderu Bin występują błędy</span><span class="sxs-lookup"><span data-stu-id="09ea5-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="09ea5-268">Podczas wdrażania, kopii stron ASP.NET Web Pages zestawów (na przykład *Microsoft.WebPages.dll*) do *Bin* folderu witryny sieci Web na serwerze.</span><span class="sxs-lookup"><span data-stu-id="09ea5-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="09ea5-269">(To może się to zdarzyć automatycznie podczas wdrażania lub ponieważ dewelopera jawnie skopiowany zestawy.) Jednak po zainstalowaniu wersji Beta 3 błędy zachodzi, takich jak błędy, których nie można znaleźć niektórych typów.</span><span class="sxs-lookup"><span data-stu-id="09ea5-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="09ea5-270">Dzieje się tak, ponieważ liczba stron ASP.NET Web Pages typy zostały przeniesione w różnych przestrzeniach nazw w wersji Beta 3.</span><span class="sxs-lookup"><span data-stu-id="09ea5-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="09ea5-271">**Obejście problemu** </span><span class="sxs-lookup"><span data-stu-id="09ea5-271">**Workaround** </span></span>  
> <span data-ttu-id="09ea5-272">Wyczyść *Bin* folderu wdrożonej aplikacji skopiuj nowy zestawów do folderu (lub ponownego wdrażania aplikacji), a następnie ponownie uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="09ea5-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="09ea5-273">Problem: Adresy URL bez rozszerzeń nie zostanie znaleziony.cshtml/.vbhtml pliki w usługach IIS 7 i IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="09ea5-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="09ea5-274">W usługach IIS 7 lub usług IIS 7.5 żądania o adresie URL podobnie do następującej nie są w stanie odnaleźć strony, które mają *.cshtml* lub *.vbhtml* rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="09ea5-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="09ea5-275">Problem występuje, ponieważ ponowne zapisywanie adresów URL nie jest włączona domyślnie dla usług IIS 7 i IIS 7.5.</span><span class="sxs-lookup"><span data-stu-id="09ea5-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="09ea5-276">Scenariusz likeliest jest, że nie ma problem podczas testowania, lokalnie za pomocą usług IIS Express, ale wystąpić podczas wdrażania witryny sieci Web do obsługi witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="09ea5-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="09ea5-277">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="09ea5-278">Jeśli masz kontrolę nad komputerem serwera na komputerze serwera, zainstaluj aktualizację opisaną w [aktualizacja jest dostępna, że umożliwia niektórych obsługi usług IIS 7.0 lub 7.5 usług IIS do obsługi żądań, których adresy URL nie kończą się kropką](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="09ea5-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="09ea5-279">Jeśli nie masz kontrolę nad komputerem serwera (na przykład wdrażasz do obsługi witryny sieci Web), dodaj następującą wartość do witryny sieci Web *Web.config* pliku:</span><span class="sxs-lookup"><span data-stu-id="09ea5-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]


#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="09ea5-280">Problem: Przy użyciu stron ASP.NET MVC i platformy ASP.NET w sieci Web lub projektu aplikacji sieci Web w tej samej aplikacji</span><span class="sxs-lookup"><span data-stu-id="09ea5-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="09ea5-281">Jeśli były używane strony sieci Web ASP.NET w projekt aplikacji sieci Web lub aplikacji ASP.NET MVC, zostanie wyświetlony błąd który *WebPageHttpApplication* nie można odnaleźć.</span><span class="sxs-lookup"><span data-stu-id="09ea5-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="09ea5-282">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-282">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-283">Jeśli ten błąd, Zmień klasę podstawową, z którego pochodzi aplikacja.</span><span class="sxs-lookup"><span data-stu-id="09ea5-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="09ea5-284">W *Global.asax* zmień następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="09ea5-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="09ea5-285">następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="09ea5-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="09ea5-286">Ta opcja powoduje w celu zmiany, która została wprowadzona w wersji Beta 1 programu ASP.NET Web Pages o składni Razor.</span><span class="sxs-lookup"><span data-stu-id="09ea5-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="09ea5-287">Problem: Wdrażanie aplikacji na komputerze, na którym nie ma zainstalowany program SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="09ea5-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="09ea5-288">Aplikacje, które obejmują baz danych programu SQL Server Compact można uruchomić na komputerze, na którym program SQL Server Compact nie jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="09ea5-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="09ea5-289">Microsoft WebMatrix w wersji Beta 3 automatycznie kopiuje tych plików binarnych dla Ciebie i wykonuje odpowiednie *Web.config* pliku transformacji.</span><span class="sxs-lookup"><span data-stu-id="09ea5-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="09ea5-290">**Obejście** Jeśli musisz skopiować te pliki i upewnić *Web.config* zmiany pliku ręcznie, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="09ea5-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="09ea5-291">Kopiowanie zestawów aparatu bazy danych do *Bin* folderze (i jego podfolderach) aplikacji na komputerze docelowym:</span><span class="sxs-lookup"><span data-stu-id="09ea5-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="09ea5-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="09ea5-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="09ea5-293">Kopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **do** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="09ea5-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="09ea5-294">Kopiuj *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **do** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="09ea5-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="09ea5-295">W folderze głównym witryny sieci Web, należy utworzyć lub otworzyć *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="09ea5-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="09ea5-296">(W programie WebMatrix w wersji Beta 3, ten typ pliku jest dostępna po kliknięciu **wszystkie** w **wybierz typ pliku** okno dialogowe.)</span><span class="sxs-lookup"><span data-stu-id="09ea5-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="09ea5-297">Dodaj następujący element jako element podrzędny **&lt;konfiguracji&gt;** elementu (nie znajduje się w **&lt;system.web&gt;** element):</span><span class="sxs-lookup"><span data-stu-id="09ea5-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="09ea5-298">Problem: Bazy danych i WebGrid pomocników nie działają w trybie średniego zaufania w języku Visual Basic</span><span class="sxs-lookup"><span data-stu-id="09ea5-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="09ea5-299">Jeśli używasz programu Visual Basic (Tworzenie *.vbhtml* plików), `Database` i `WebGrid` pomocników nie będzie działać, jeśli aplikacja jest skonfigurowana do użycia w trybie średniego zaufania.</span><span class="sxs-lookup"><span data-stu-id="09ea5-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="09ea5-300">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-300">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-301">Tymczasowo ustawić aplikacji do korzystania z pełnego zaufania.</span><span class="sxs-lookup"><span data-stu-id="09ea5-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="09ea5-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="09ea5-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="09ea5-303">Problem: Właściwość "Szyfruj" nie został rozpoznany</span><span class="sxs-lookup"><span data-stu-id="09ea5-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="09ea5-304">SQL Server Compact 4.0 nie rozpoznaje `Encrypt` właściwość `SqlCeConnection` klasy.</span><span class="sxs-lookup"><span data-stu-id="09ea5-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="09ea5-305">Ta właściwość nie należy używać do szyfrowania plików bazy danych.</span><span class="sxs-lookup"><span data-stu-id="09ea5-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="09ea5-306">`Encrypt` Właściwość została uznana za przestarzałą w programu SQL Server Compact 3.5 wersji i została zachowana tylko w przypadku zgodności z poprzednimi wersjami.</span><span class="sxs-lookup"><span data-stu-id="09ea5-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="09ea5-307">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-307">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-308">Użyj `Encryption Mode` właściwość `SqlCeConnection` klasy do szyfrowania plików bazy danych programu SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="09ea5-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="09ea5-309">Poniższy przykład przedstawia sposób tworzenia zaszyfrowane programu SQL Server Compact 4.0 bazy danych przy użyciu `Encryption Mode` właściwości:</span><span class="sxs-lookup"><span data-stu-id="09ea5-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="09ea5-310">Aby zmienić tryb szyfrowania z istniejącej bazy danych programu SQL Server Compact 4.0, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="09ea5-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="09ea5-311">Aby zaszyfrować niezaszyfrowane bazy danych programu SQL Server Compact 4.0, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="09ea5-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]


#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="09ea5-312">Problem: Bibliotek środowiska uruchomieniowego programu Microsoft Visual C++ 2008 są wymagane</span><span class="sxs-lookup"><span data-stu-id="09ea5-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="09ea5-313">Natywnych bibliotek DLL programu SQL Server Compact 4.0 wymagają programu Microsoft Visual C++ 2008 bibliotek środowiska uruchomieniowego (x 86, IA64 i x 64), z dodatkiem Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="09ea5-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="09ea5-314">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-314">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-315">Zainstaluj program .NET Framework 3.5 SP1.</span><span class="sxs-lookup"><span data-stu-id="09ea5-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="09ea5-316">Spowoduje to również zainstalowanie Visual C++ 2008 środowiska uruchomieniowego bibliotek z dodatkiem SP1.</span><span class="sxs-lookup"><span data-stu-id="09ea5-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="09ea5-317">Biblioteki można pobrać z następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="09ea5-317">You can download the libraries from the following location:</span></span>   
>   
> [<span data-ttu-id="09ea5-318">Aktualizacja zabezpieczeń biblioteki ATL do programu Microsoft Visual C++ 2008 z dodatkiem Service Pack 1 pakietu redystrybucyjnego</span><span class="sxs-lookup"><span data-stu-id="09ea5-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span></span>](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> <span data-ttu-id="09ea5-319">Należy pamiętać, instalowanie programu .NET Framework 2.0, 3.0, lub nie 4 *nie* zainstalować Visual C++ 2008 środowiska uruchomieniowego bibliotek z dodatkiem SP1.</span><span class="sxs-lookup"><span data-stu-id="09ea5-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>


#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="09ea5-320">Problem: Jeśli program SQL Server Compact jest zainstalowany przed zainstalowaniem programu .NET Framework na komputerze, niezmiennej nazwy dostawcy nie jest zarejestrowany w pliku machine.config .NET Framework</span><span class="sxs-lookup"><span data-stu-id="09ea5-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="09ea5-321">SQL Server Compact można zainstalować na komputerze, na którym nie ma zainstalowany, ponieważ program SQL Server Compact wymagają programu .NET framework .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="09ea5-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="09ea5-322">Jeśli .NET Framework w wersji 3.5 ani 4 jest zainstalowany, przed zainstalowaniem programu SQL Server Compact, Instalator programu SQL Server Compact nie rejestruje jego niezmienna Nazwa dostawcy w *machine.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="09ea5-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="09ea5-323">Każda aplikacja, która korzysta z programu SQL Server Compact wpisu w *machine.config* pliku zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="09ea5-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="09ea5-324">Niezmienna Nazwa wpisu rejestracji w *machine.config* wygląda jak w poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="09ea5-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="09ea5-325">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-325">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-326">Odinstaluj program SQL Server Compact 4.0 CTP1.</span><span class="sxs-lookup"><span data-stu-id="09ea5-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="09ea5-327">Pobierz i zainstaluj pełnej wersji programu .NET Framework z następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="09ea5-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="09ea5-328">Microsoft .NET Framework 3.5 z dodatkiem Service pack 1 (pełny pakiet)</span><span class="sxs-lookup"><span data-stu-id="09ea5-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [<span data-ttu-id="09ea5-329">Program Microsoft .NET Framework 4.0 zlecenia (pełny pakiet)</span><span class="sxs-lookup"><span data-stu-id="09ea5-329">Microsoft .NET Framework 4.0 Release (Full Package)</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> <span data-ttu-id="09ea5-330">Następnie zainstaluj ponownie [programu SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span><span class="sxs-lookup"><span data-stu-id="09ea5-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>


<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="09ea5-331">Instalowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="09ea5-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="09ea5-332">Problem: Instalowanie aplikacji może długo trwać, jeśli folder Moje dokumenty użytkownika jest przekierowywany do udziału sieciowego</span><span class="sxs-lookup"><span data-stu-id="09ea5-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="09ea5-333">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-333">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-334">Brak.</span><span class="sxs-lookup"><span data-stu-id="09ea5-334">None.</span></span> <span data-ttu-id="09ea5-335">Aplikacja może potrwać kilka minut, aby zainstalować, ale zostanie zainstalowany poprawnie.</span><span class="sxs-lookup"><span data-stu-id="09ea5-335">The application might take a while to install, but will install correctly.</span></span>


<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="09ea5-336">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="09ea5-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="09ea5-337">Problem: Lokacji może nie działać po opublikowaniu, jeśli w polu "Docelowy adres URL" nie jest poprzedzona http:// lub https://</span><span class="sxs-lookup"><span data-stu-id="09ea5-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="09ea5-338">W **ustawienia publikowania** okno dialogowe, jeśli docelowy adres URL nie rozpoczyna się od `http://` lub `https://`, lokacji może nie działać po wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="09ea5-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="09ea5-339">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-339">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-340">Upewnij się, że przed opublikowaniem witryny, docelowy adres URL na **ustawień publikowania** okno dialogowe rozpoczyna się od `http://` lub `https://`.</span><span class="sxs-lookup"><span data-stu-id="09ea5-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="09ea5-341">Problem: Publikowanie bazy danych MySQL zakończy się niepowodzeniem z powodu błędu "nie można opublikować bazy danych.</span><span class="sxs-lookup"><span data-stu-id="09ea5-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="09ea5-342">Może to nastąpić w przypadku zdalnej bazy danych nie można uruchomić skryptu."</span><span class="sxs-lookup"><span data-stu-id="09ea5-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="09ea5-343">Błąd może wystąpić z kilku powodów.</span><span class="sxs-lookup"><span data-stu-id="09ea5-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="09ea5-344">Jedną z przyczyn, zostanie wyświetlony ten błąd jest Jeśli skryptu bazy danych zawiera znak pojedynczego cudzysłowu ('), a domyślny zestaw znaków miejsce docelowe danych MySQL nie jest UTF-8.</span><span class="sxs-lookup"><span data-stu-id="09ea5-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="09ea5-345">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-345">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-346">Ustaw domyślny zestaw znaków dla zdalnej bazy danych MySQL na UTF-8.</span><span class="sxs-lookup"><span data-stu-id="09ea5-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>


<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="09ea5-347">Inne problemy</span><span class="sxs-lookup"><span data-stu-id="09ea5-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="09ea5-348">Problem: Filtr/wyszukiwania nie działa w raportach Grupuj według: typ problemu</span><span class="sxs-lookup"><span data-stu-id="09ea5-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="09ea5-349">Po uruchomieniu raportu dla lokacji, jeśli wprowadzony tekst w *Filtruj według adresu URL* polu i kliknij przycisk *wyszukiwania*, nic się nie dzieje.</span><span class="sxs-lookup"><span data-stu-id="09ea5-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="09ea5-350">Wynika to z faktu ten formant nie działa podczas *Group By* ustawiony stan raportu *typu problemu*, co jest ustawieniem domyślnym.</span><span class="sxs-lookup"><span data-stu-id="09ea5-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="09ea5-351">**Obejście** w *Group By* kartę na wstążce kliknij *adres URL* do grupowania wpisy według ich źródłowy adres URL.</span><span class="sxs-lookup"><span data-stu-id="09ea5-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="09ea5-352">Pole tekstowe i przycisk do filtrowania pozycji są działa w tym stanie.</span><span class="sxs-lookup"><span data-stu-id="09ea5-352">The text box and button to filter the entries are functional while in this state.</span></span>


#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="09ea5-353">Problem: Aplikacji WCF nie udało się uruchomić z programem IIS Express</span><span class="sxs-lookup"><span data-stu-id="09ea5-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="09ea5-354">Przeglądanie aplikacji WCF powoduje błąd podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="09ea5-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="09ea5-355">*Nie można załadować pliku lub zestawu ' Microsoft.Web.Administration, Version = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 "lub jednej z jego zależności. System nie może odnaleźć określonego pliku.*</span><span class="sxs-lookup"><span data-stu-id="09ea5-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="09ea5-356">Dzieje się tak, ponieważ usługi IIS Express w wersji beta nie obsługuje WCF domyślnie.</span><span class="sxs-lookup"><span data-stu-id="09ea5-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="09ea5-357">**Obejście** użyj jednej z poniższych rozwiązań (obejście #2 wymaga systemu Microsoft Windows Vista lub nowszy):</span><span class="sxs-lookup"><span data-stu-id="09ea5-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="09ea5-358">Kopiuj *Microsoft.Web.dll* i *biblioteki Microsoft.Web.Administration.dll* zestawy z lokalizacji instalacji programu WebMatrix w celu *bin* katalog usługi WCF aplikacja.</span><span class="sxs-lookup"><span data-stu-id="09ea5-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="09ea5-359">Domyślnie program WebMatrix jest zainstalowany w *Microsoft WebMatrix* podfolderze systemu *Program Files* folderu.</span><span class="sxs-lookup"><span data-stu-id="09ea5-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="09ea5-360">W systemie Microsoft Windows Vista lub nowszego, należy utworzyć łącza symbolicznego do zestawów w *bin* katalogu przy użyciu następujących poleceń.</span><span class="sxs-lookup"><span data-stu-id="09ea5-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="09ea5-361">(To rozwiązanie ma tę zaletę, że nie tworzy kopię zestawy).</span><span class="sxs-lookup"><span data-stu-id="09ea5-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="09ea5-362">Zainstalowanie dwóch zestawów w pamięci podręcznej GAC.</span><span class="sxs-lookup"><span data-stu-id="09ea5-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="09ea5-363">W wierszu z podwyższonym poziomem uprawnień uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="09ea5-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]


#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="09ea5-364">Problem: Program WebMatrix w wersji Beta 3 nie jest w stanie do wykonania niektórych zadań, które wymagają podniesionych uprawnień</span><span class="sxs-lookup"><span data-stu-id="09ea5-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="09ea5-365">Program WebMatrix w wersji Beta 3 jest w stanie do wykonania niektórych zadań, które wymagają podniesionych uprawnień, takich jak instalowanie dodatkowych składników w następujących sytuacjach:</span><span class="sxs-lookup"><span data-stu-id="09ea5-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="09ea5-366">W systemie Windows Vista lub Windows 7 zalogowano się za pomocą konta, które nie ma uprawnień administracyjnych i kontroli konta użytkownika (UAC) jest wyłączona.</span><span class="sxs-lookup"><span data-stu-id="09ea5-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="09ea5-367">Używasz systemu Microsoft Windows XP lub Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="09ea5-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="09ea5-368">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-368">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-369">Większość zadań w programie WebMatrix w wersji Beta 3 nie wymaga uprawnień administracyjnych.</span><span class="sxs-lookup"><span data-stu-id="09ea5-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="09ea5-370">Dla tych, które wykonują można wykonać operacji jako administrator lub wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="09ea5-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="09ea5-371">W systemie Windows Vista lub Windows 7 należy włączyć funkcji Kontrola konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="09ea5-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="09ea5-372">W systemie Windows XP należy dodać użytkownika do grupy zabezpieczeń Administratorzy.</span><span class="sxs-lookup"><span data-stu-id="09ea5-372">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="09ea5-373">Problem: "Lokacji z galerii" jest wyłączona.</span><span class="sxs-lookup"><span data-stu-id="09ea5-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="09ea5-374">**Witryny sieci Web galerii** opcja jest wyłączona, jeśli Instalator platformy sieci Web 3.0 nie jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="09ea5-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="09ea5-375">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-375">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-376">Zainstaluj [Instalatora platformy sieci Web firmy Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="09ea5-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="09ea5-377">Problem: W systemie Windows Server 2003, usługi IIS Express nie uruchamia się dla użytkowników innych niż administracyjne</span><span class="sxs-lookup"><span data-stu-id="09ea5-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="09ea5-378">W systemie Windows Server 2003 podczas uruchamiania stronę lub uruchom usługi IIS Express, usługi IIS Express nie można uruchomić.</span><span class="sxs-lookup"><span data-stu-id="09ea5-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="09ea5-379">Dla stron sieci Web zostanie wyświetlony błąd wskazujący, że aplikacja została uruchomiona przez użytkownika niebędącego administratorem.</span><span class="sxs-lookup"><span data-stu-id="09ea5-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="09ea5-380">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-380">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-381">Uruchom program WebMatrix w wersji Beta 3 jako użytkownik z uprawnieniami administracyjnymi.</span><span class="sxs-lookup"><span data-stu-id="09ea5-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="09ea5-382">Aby uzyskać więcej informacji zobacz następujący artykuł bazy wiedzy:</span><span class="sxs-lookup"><span data-stu-id="09ea5-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="09ea5-383">Aplikacja, która jest uruchomiona przez użytkownika niebędącego administratorem nie może nasłuchiwać na ruch HTTP komputera, na którym aplikacja jest uruchomiona w systemie Windows Vista, Windows Server 2003 lub Windows XP.</span><span class="sxs-lookup"><span data-stu-id="09ea5-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="09ea5-384">Problem: Google Chrome nie jest dostępna jako opcja wykonywania</span><span class="sxs-lookup"><span data-stu-id="09ea5-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="09ea5-385">Google Chrome nie jest wyświetlana na liście przeglądarek w obszarze **Uruchom** na **Home** kartę.</span><span class="sxs-lookup"><span data-stu-id="09ea5-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="09ea5-386">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-386">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-387">Niektóre wersje programu Google Chrome nie rejestrują się poprawnie z funkcją programy domyślne w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="09ea5-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="09ea5-388">Jako obejście, należy uruchomić Google Chrome, kliknij polecenie *Dostosuj i kontroli Google Chrome* menu, kliknij przycisk *opcje*, a następnie kliknij przycisk *upewnij Google Chrome przeglądarce domyślne*.</span><span class="sxs-lookup"><span data-stu-id="09ea5-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="09ea5-389">Problem: Okno dialogowe "Klucz obcy" nie zezwala na wprowadzenie klucza podstawowego</span><span class="sxs-lookup"><span data-stu-id="09ea5-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="09ea5-390">**Klucz obcy** okno dialogowe nie pozwala na wprowadzanie nazwy klucza podstawowego z tabeli klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="09ea5-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="09ea5-391">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-391">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-392">Jest to zamierzone.</span><span class="sxs-lookup"><span data-stu-id="09ea5-392">This is intentional.</span></span> <span data-ttu-id="09ea5-393">Nie trzeba wprowadzić nazwę klucza podstawowego z tabeli klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="09ea5-393">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="09ea5-394">Problem: Przycisk "Relacji" jest wyłączony.</span><span class="sxs-lookup"><span data-stu-id="09ea5-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="09ea5-395">**Relacje** przycisku w obszarze **tabeli** karcie **baz danych** obszaru roboczego jest wyłączone dla bazy danych programu SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="09ea5-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="09ea5-396">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-396">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-397">Brak.</span><span class="sxs-lookup"><span data-stu-id="09ea5-397">None.</span></span> <span data-ttu-id="09ea5-398">SQL Server Compact nie obsługuje relacji między tabelami.</span><span class="sxs-lookup"><span data-stu-id="09ea5-398">SQL Server Compact does not support relationships between tables.</span></span>


#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="09ea5-399">Problem: Zapytania parametrycznego SQL zgłaszanie wyjątków</span><span class="sxs-lookup"><span data-stu-id="09ea5-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="09ea5-400">W przypadku programu SQL Server Compact 4.0, jeśli nie określisz typu danych takich jak `SqlDbType` lub `DbType` dla parametrów w zapytaniach parametrycznych, jest zgłaszany wyjątek podczas wykonywania kwerendy.</span><span class="sxs-lookup"><span data-stu-id="09ea5-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="09ea5-401">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="09ea5-401">**Workaround**</span></span>  
> <span data-ttu-id="09ea5-402">Jawnie ustawić typ danych dla parametrów, takich jak `SqlDbType` lub `DbType`.</span><span class="sxs-lookup"><span data-stu-id="09ea5-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="09ea5-403">Jest to szczególnie ważne w przypadku typów danych obiektów BLOB (`image` i `ntext`).</span><span class="sxs-lookup"><span data-stu-id="09ea5-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="09ea5-404">Użyj kodu podobne do poniższych:</span><span class="sxs-lookup"><span data-stu-id="09ea5-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="09ea5-405">Aby uzyskać więcej informacji</span><span class="sxs-lookup"><span data-stu-id="09ea5-405">For More Information</span></span>

<span data-ttu-id="09ea5-406">Aby uzyskać więcej informacji na temat programu WebMatrix w wersji Beta 3 zobacz następujące witryny sieci Web:</span><span class="sxs-lookup"><span data-stu-id="09ea5-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="09ea5-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="09ea5-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="09ea5-408">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="09ea5-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="09ea5-409">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="09ea5-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

* * *

<span data-ttu-id="09ea5-410">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="09ea5-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="09ea5-411">Wszelkie prawa zastrzeżone.</span><span class="sxs-lookup"><span data-stu-id="09ea5-411">All Rights Reserved.</span></span> <span data-ttu-id="09ea5-412">[Warunki użytkowania](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="09ea5-412">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
