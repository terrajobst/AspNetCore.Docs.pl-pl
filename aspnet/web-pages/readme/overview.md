---
uid: web-pages/readme/overview
title: Plik Readme programu WebMatrix | Dokumentacja firmy Microsoft
author: rick-anderson
description: Program WebMatrix i plik Readme programu ASP.NET Web Pages (Razor) wersji 1.0
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/06/2011
ms.topic: article
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: c65ee58b8c13b0b4acb6e7c9b631c8235e791506
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="webmatrix-readme"></a><span data-ttu-id="8d010-103">Plik Readme programu WebMatrix</span><span class="sxs-lookup"><span data-stu-id="8d010-103">WebMatrix Readme</span></span>
====================
<span data-ttu-id="8d010-104">13 stycznia 2011</span><span class="sxs-lookup"><span data-stu-id="8d010-104">13 January 2011</span></span>

## <a name="contents"></a><span data-ttu-id="8d010-105">Spis treści</span><span class="sxs-lookup"><span data-stu-id="8d010-105">Contents</span></span>

> [!NOTE]
> <span data-ttu-id="8d010-106">Ten plik readme dotyczy wersji 1.0 programu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="8d010-106">This readme applies to the 1.0 release of WebMatrix.</span></span>


- [<span data-ttu-id="8d010-107">Omówienie</span><span class="sxs-lookup"><span data-stu-id="8d010-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="8d010-108">Instalacja</span><span class="sxs-lookup"><span data-stu-id="8d010-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="8d010-109">Sposób publikowania aplikacji</span><span class="sxs-lookup"><span data-stu-id="8d010-109">How to Publish Applications</span></span>](#InstructionsForPublishingApplications)
- [<span data-ttu-id="8d010-110">Zmiany i problemów</span><span class="sxs-lookup"><span data-stu-id="8d010-110">Changes and Issues</span></span>](#ChangesAndIssues)

    - [<span data-ttu-id="8d010-111">Instalacja programu WebMatrix w wersji 1.0</span><span class="sxs-lookup"><span data-stu-id="8d010-111">WebMatrix 1.0 Installation</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="8d010-112">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="8d010-112">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="8d010-113">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="8d010-113">WebMatrix</span></span>](#Known_Issues_WebMatrix)
    - [<span data-ttu-id="8d010-114">Usługi IIS Express</span><span class="sxs-lookup"><span data-stu-id="8d010-114">IIS Express</span></span>](#Known_Issues_IISExpress)
    - [<span data-ttu-id="8d010-115">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="8d010-115">SQL Server Compact</span></span>](#Known_Issues_SQLServerCompact)
    - [<span data-ttu-id="8d010-116">Instalowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="8d010-116">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="8d010-117">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="8d010-117">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
- [<span data-ttu-id="8d010-118">Aby uzyskać więcej informacji</span><span class="sxs-lookup"><span data-stu-id="8d010-118">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="8d010-119">Omówienie</span><span class="sxs-lookup"><span data-stu-id="8d010-119">Overview</span></span>

> <span data-ttu-id="8d010-120">Microsoft WebMatrix 1.0 jest stos programowanie wolnej sieci web, który instaluje w minutach.</span><span class="sxs-lookup"><span data-stu-id="8d010-120">Microsoft WebMatrix 1.0 is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="8d010-121">Serwer sieci web jest zintegrowany z bazy danych i programowania platformy do utworzenia jednym zintegrowanym interfejsie.</span><span class="sxs-lookup"><span data-stu-id="8d010-121">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="8d010-122">Program WebMatrix umożliwia upraszcza sposób kodu, testowania i publikowanie własnych ASP.NET i PHP witryny sieci Web, lub można użyć programu WebMatrix można uruchomić nowej witryny sieci Web przy użyciu popularnych aplikacji open source, takich jak DotNetNuke, Umbraco, WordPress lub Joomla.</span><span class="sxs-lookup"><span data-stu-id="8d010-122">You can use WebMatrix to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="8d010-123">Program WebMatrix korzysta z tego samego zaawansowanego serwera, aparatu bazy danych i środowiska struktur, które zostanie uruchomione witryny sieci Web w Internecie, dzięki czemu przejście od projektowania do produkcji płynne i bezproblemowe.</span><span class="sxs-lookup"><span data-stu-id="8d010-123">WebMatrix uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="8d010-124">Instalacja</span><span class="sxs-lookup"><span data-stu-id="8d010-124">Installation</span></span>

> <span data-ttu-id="8d010-125">Aby zainstalować program WebMatrix w wersji 1.0, należy najpierw zainstalować [3.0 Instalatora platformy sieci Web Microsoft](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="8d010-125">To install WebMatrix 1.0, you must first install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="8d010-126">Po zainstalowaniu Instalatora platformy sieci Web, można ją zainstalować program WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="8d010-126">After you've installed the Web Platform Installer, you can use it to install WebMatrix.</span></span>
> 
> <span data-ttu-id="8d010-127">Jeśli masz problemy podczas instalacji, zapoznaj się [Rozwiązywanie problemów dotyczących Instalatora platformy sieci Web firmy Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="8d010-127">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a><span data-ttu-id="8d010-128">Sposób publikowania aplikacji</span><span class="sxs-lookup"><span data-stu-id="8d010-128">How to Publish Applications</span></span>

> <span data-ttu-id="8d010-129">Zobacz [instrukcje krok po kroku dotyczące publikowania aplikacji](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="8d010-129">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a><span data-ttu-id="8d010-130">Zmiany i problemów</span><span class="sxs-lookup"><span data-stu-id="8d010-130">Changes and Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a><span data-ttu-id="8d010-131">Problemy z instalacją 1.0 programu WebMatrix</span><span class="sxs-lookup"><span data-stu-id="8d010-131">WebMatrix 1.0 Installation Issues</span></span>

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="8d010-132">Problem: WebMatrix 1.0 jest dostępna tylko na platformach, które obsługuje program Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="8d010-132">Issue: WebMatrix 1.0 is available only on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="8d010-133">.NET Framework w wersji 4 jest wymagany dla programu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="8d010-133">The .NET Framework version 4 is required for WebMatrix.</span></span> <span data-ttu-id="8d010-134">W niektórych przypadkach Instalator programu WebMatrix 1.0 umożliwi spróbuje zainstalować na platformie, która nie wchodzi w skład zestawu obsługiwanej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8d010-134">In certain cases, the WebMatrix 1.0 installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="8d010-135">W szczególności Windows Vista bez dodatku SP1 dla aktualizacji będzie można rozpocząć instalację programu WebMatrix, ale składników .NET Framework 4 spowoduje niepowodzenie i zablokowanie instalacji.</span><span class="sxs-lookup"><span data-stu-id="8d010-135">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="8d010-136">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-136">**Workaround**</span></span>  
> <span data-ttu-id="8d010-137">Zainstaluj na obsługiwanych platform, w tym:</span><span class="sxs-lookup"><span data-stu-id="8d010-137">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="8d010-138">Windows 7</span><span class="sxs-lookup"><span data-stu-id="8d010-138">Windows 7</span></span>
> - <span data-ttu-id="8d010-139">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="8d010-139">Windows Server 2008</span></span>
> - <span data-ttu-id="8d010-140">Windows Server 2008 z dodatkiem R2</span><span class="sxs-lookup"><span data-stu-id="8d010-140">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="8d010-141">Windows Vista z dodatkiem SP1 lub nowszym</span><span class="sxs-lookup"><span data-stu-id="8d010-141">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="8d010-142">Windows XP z dodatkiem SP3</span><span class="sxs-lookup"><span data-stu-id="8d010-142">Windows XP SP3</span></span>
> - <span data-ttu-id="8d010-143">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="8d010-143">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="8d010-144">Problem: Nie można zainstalować program WebMatrix w wersji 1.0, jeśli zainstalowano program Microsoft Visual Studio 2008 bez programu Microsoft Visual Studio 2008 z dodatkiem SP1</span><span class="sxs-lookup"><span data-stu-id="8d010-144">Issue: Cannot install WebMatrix 1.0 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="8d010-145">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-145">**Workaround**</span></span>  
> <span data-ttu-id="8d010-146">Zainstaluj [programu Microsoft Visual Studio 2008 z dodatkiem SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) z Centrum pobierania Microsoft.</span><span class="sxs-lookup"><span data-stu-id="8d010-146">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="8d010-147">Problem: Niektóre zestawy dla programu SQL Server Compact 4.0 nie są zainstalowane w GAC</span><span class="sxs-lookup"><span data-stu-id="8d010-147">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="8d010-148">Podczas instalowania programu SQL Server Compact 4.0 na komputerze 64-bitowy komputer ma tylko .NET Framework 3.5 SP1 Client Profile zainstalowany zarządzanych zestawów dla programu SQL Server Compact 4.0 nie są umieszczane w globalnej pamięci podręcznej zestawów (GAC).</span><span class="sxs-lookup"><span data-stu-id="8d010-148">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="8d010-149">Zarządzanych zestawów, które nie są zainstalowane w GAC są:</span><span class="sxs-lookup"><span data-stu-id="8d010-149">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="8d010-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span><span class="sxs-lookup"><span data-stu-id="8d010-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="8d010-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span><span class="sxs-lookup"><span data-stu-id="8d010-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="8d010-152">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-152">**Workaround**</span></span>  
> <span data-ttu-id="8d010-153">Odinstaluj program SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="8d010-153">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="8d010-154">Pobierz i zainstaluj pełną wersję programu .NET Framework 3.5 z dodatkiem SP1 z następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="8d010-154">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="8d010-155">Microsoft .NET Framework 3.5 z dodatkiem Service pack 1 (pełny pakiet)</span><span class="sxs-lookup"><span data-stu-id="8d010-155">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="8d010-156">Następnie ponownie zainstalować program SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="8d010-156">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="8d010-157">Problem: Nie można odinstalować za pomocą wiersza polecenia programu SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="8d010-157">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="8d010-158">Dezinstalacja przy użyciu opcji wiersza polecenia programu SQL Server Compact nie działa w tej wersji.</span><span class="sxs-lookup"><span data-stu-id="8d010-158">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="8d010-159">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-159">**Workaround**</span></span>  
> <span data-ttu-id="8d010-160">Użyj *programy i funkcje* w Panelu sterowania systemu Windows, aby odinstalować program Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="8d010-160">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="8d010-161">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="8d010-161">ASP.NET Web Pages</span></span>

<span data-ttu-id="8d010-162">W tej sekcji dokumentu opisano nowe funkcje, zmiany i znane problemy z wersji 1.0 ASP.NET Web Pages o składni Razor.</span><span class="sxs-lookup"><span data-stu-id="8d010-162">This section of the document describes new features, changes, and known issues with the 1.0 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="8d010-163">Nowe funkcje</span><span class="sxs-lookup"><span data-stu-id="8d010-163">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="8d010-164">Zmiany</span><span class="sxs-lookup"><span data-stu-id="8d010-164">Changes</span></span>](#Changes)
- [<span data-ttu-id="8d010-165">Problemy</span><span class="sxs-lookup"><span data-stu-id="8d010-165">Issues</span></span>](#Issues)

#### <a id="NewFeatures"></a>  <span data-ttu-id="8d010-166">Nowe funkcje</span><span class="sxs-lookup"><span data-stu-id="8d010-166">New Features</span></span>

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a><span data-ttu-id="8d010-167">Nowy: Ustawienie konfiguracji dodane do wyłączenia Menedżera pakietów</span><span class="sxs-lookup"><span data-stu-id="8d010-167">New: Configuration setting added to disable the package manager</span></span>

> <span data-ttu-id="8d010-168">Nowy `asp:AdminManagerEnabled` klucz jest dostępny dla `<appSettings>` element *web.config* pliku, który można całkowicie wyłączyć Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="8d010-168">A new `asp:AdminManagerEnabled` key is available for the `<appSettings>` element in the *web.config* file, which lets you completely disable the package manager.</span></span> <span data-ttu-id="8d010-169">Wartością domyślną dla tego elementu ma wartość true, co oznacza, że jeśli nie jest uwzględniony w *web.config* pliku Menedżera pakietów jest włączone.</span><span class="sxs-lookup"><span data-stu-id="8d010-169">The default value for this element is true, meaning that if it is not included in the *web.config* file, the package manager is enabled.</span></span> <span data-ttu-id="8d010-170">Aby wyłączyć Menedżera pakietów, Dodaj następujący element do *web.config* w katalogu głównym witryny sieci Web:</span><span class="sxs-lookup"><span data-stu-id="8d010-170">To disable the package manager, add the following element to the *web.config* file in the root of the website:</span></span>
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  <span data-ttu-id="8d010-171">Zmiany</span><span class="sxs-lookup"><span data-stu-id="8d010-171">Changes</span></span>

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a><span data-ttu-id="8d010-172">Zmień: klucz "webPages:AdminFolderVirtualPath" zmieniona na "asp: AdminFolderVirtualPath"</span><span class="sxs-lookup"><span data-stu-id="8d010-172">Change: "webPages:AdminFolderVirtualPath" key renamed to "asp:AdminFolderVirtualPath"</span></span>

> <span data-ttu-id="8d010-173">`webPages:AdminFolderVirtualPath` Klucz, który można dodać do *web.config* plik, aby określić lokalizację Menedżera pakietów została zmieniona na użyj `asp:` przestrzeni nazw zamiast `webPages` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="8d010-173">The `webPages:AdminFolderVirtualPath` key that can be added to the *web.config* file to specify the location of the package manager has been renamed to use the `asp:` namespace instead of the `webPages` namespace.</span></span> <span data-ttu-id="8d010-174">Jeśli używasz tego elementu, można zmienić jego nazwę w pliku konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="8d010-174">If you have used this element, you must rename it in the configuration file.</span></span>


#### <a id="Issues"></a>  <span data-ttu-id="8d010-175">Znane problemy</span><span class="sxs-lookup"><span data-stu-id="8d010-175">Known Issues</span></span>

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a><span data-ttu-id="8d010-176">Problem: Haseł użytkowników członkostwa nie został rozpoznany</span><span class="sxs-lookup"><span data-stu-id="8d010-176">Issue: Passwords for membership users no longer recognized</span></span>

> <span data-ttu-id="8d010-177">Algorytm tworzenie i przechowywanie haseł członkostwa (logowanie) został zmieniony na większe bezpieczeństwo.</span><span class="sxs-lookup"><span data-stu-id="8d010-177">The algorithm for creating and storing membership (login) passwords has been changed to be more secure.</span></span> <span data-ttu-id="8d010-178">W związku z tym nie zostanie rozpoznana haseł przechowywanych dla elementów członkowskich (użytkownicy) utworzone w wersji Beta programu ASP.NET Razor.</span><span class="sxs-lookup"><span data-stu-id="8d010-178">As a result, the passwords stored for members (users) created in Beta versions of ASP.NET Razor will not be recognized.</span></span> 
> 
> <span data-ttu-id="8d010-179">**Obejście** lokacji nie ma jeszcze wprowadzone do produkcji, usunięcie rekordów użytkowników z bazy danych członkostwa.</span><span class="sxs-lookup"><span data-stu-id="8d010-179">**Workaround** If the site has not yet been put into production, remove the user records from the membership database.</span></span> <span data-ttu-id="8d010-180">Jeśli baza danych znajduje się na żywo, programowo ponownie wygenerować istniejące hasła w bazie danych członkostwa.</span><span class="sxs-lookup"><span data-stu-id="8d010-180">If database is live, programmatically regenerate existing passwords in the membership database.</span></span>


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="8d010-181">Problem: Nieoczekiwanego zachowania w przypadku używania tabeli użytkownika niestandardowego dla członkostwa</span><span class="sxs-lookup"><span data-stu-id="8d010-181">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="8d010-182">Aby zainicjować dostawcy członkostwa dla witryny sieci Web platformy ASP.NET Razor, należy wywołać `WebSecurity.InitializeDatabaseConnection` metody.</span><span class="sxs-lookup"><span data-stu-id="8d010-182">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="8d010-183">(W programie WebMatrix, w szablonie witryny początkowej zawiera wywołanie tej metody w  *\_AppStart.cshtml* pliku.) Jeśli `autoCreateTables` parametr tej metody jest ustawiony na wartość true (domyślnie jest ustawiona wartość true w szablonie witryny początkowej), i jeśli nazwa tabeli nierozpoznany jest przekazywany do metody (drugi parametr), metoda nie zgłasza błąd.</span><span class="sxs-lookup"><span data-stu-id="8d010-183">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="8d010-184">Zamiast tego automatycznie tworzy tabeli.</span><span class="sxs-lookup"><span data-stu-id="8d010-184">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="8d010-185">Może to być spowodowane problemem, jeśli zamierzasz używać tabeli użytkownika niestandardowego dla członkostwa, ale przekazywania do nazwy tabeli niewłaściwy `WebSecurity.InitializeDatabaseConnection` metody.</span><span class="sxs-lookup"><span data-stu-id="8d010-185">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="8d010-186">Ponieważ metoda nie domyślnie Zgłoś błąd, jeśli tabela, które określisz nie istnieje, a zamiast tego tworzy nową tabelę, aplikacja może się pojawić się działać.</span><span class="sxs-lookup"><span data-stu-id="8d010-186">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="8d010-187">Jednak kod aplikacji, która opiera się na tabeli użytkownika niestandardowego (i pól w nim) po pewnym czasie mogą nie być z nieoczekiwane błędy.</span><span class="sxs-lookup"><span data-stu-id="8d010-187">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="8d010-188">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-188">**Workaround**</span></span>  
> <span data-ttu-id="8d010-189">Upewnij się, że nazwa przekazano `InitializeDatabaseConnection` zgodna metoda profilu użytkownika tabeli w bazie danych członkostwa lub upewnij się, że `autoCreateTables` parametr ma wartość false.</span><span class="sxs-lookup"><span data-stu-id="8d010-189">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a><span data-ttu-id="8d010-190">Problem: Komunikat o błędzie "Module administrator wymaga dostępu do ~/App\_danych"</span><span class="sxs-lookup"><span data-stu-id="8d010-190">Issue: Error message "The Admin Module requires access to ~/App\_Data"</span></span>

> <span data-ttu-id="8d010-191">W pewnych okolicznościach, w trakcie tworzenia użytkowników lub pracy z systemu członkostwa programu ASP.NET może spowodować błąd na stronie *modułu administrator wymaga dostępu do ~/App\_danych*.</span><span class="sxs-lookup"><span data-stu-id="8d010-191">Under some circumstances, trying to create users or otherwise work with the ASP.NET membership system can cause the page to display the error *The Admin Module requires access to ~/App\_Data*.</span></span> <span data-ttu-id="8d010-192">Dzieje się tak, jeśli konto, które uruchamiania usług IIS lub usług IIS Express nie ma uprawnień do tworzenia i zapisywania *aplikacji\_danych* folder w katalogu głównym witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8d010-192">This occurs if the account that IIS or IIS Express is running under does not have permissions to create and write to the *App\_Data* folder under the website root.</span></span> 
> 
> <span data-ttu-id="8d010-193">**Obejście** ręcznie utworzyć *aplikacji\_danych* folderu witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8d010-193">**Workaround** Manually create an *App\_Data* folder for the website.</span></span> <span data-ttu-id="8d010-194">Następnie upewnij się, że konto systemu Windows w obszarze (zazwyczaj Usługa sieciowa) jest uruchomiona aplikacja ma uprawnienia odczytu/zapisu dla folderów głównych aplikacji i podfoldery aplikacja do obróbki\_danych.</span><span class="sxs-lookup"><span data-stu-id="8d010-194">Then make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as App\_Data.</span></span> <span data-ttu-id="8d010-195">Bardziej szczegółowe informacje są dostępne w artykule bazy wiedzy [problemów z wystąpień użytkownika programu SQL Server Express i projekty aplikacji sieci Web ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="8d010-195">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="8d010-196">Błąd "Nie można wygenerować wystąpienia użytkownika programu SQL Server" problemu:</span><span class="sxs-lookup"><span data-stu-id="8d010-196">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="8d010-197">Jeśli aplikacja sieci Web programu WebMatrix korzysta z programu SQL Server Express i działa IIS 7.5 w systemie Windows 7 lub Windows Server 2008 R2, można napotkać komunikat o błędzie wskazujący, że programu SQL Server nie można pobrać ścieżki lokalnej aplikacji użytkownika w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="8d010-197">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="8d010-198">**Obejście** upewnij się, czy konto systemu Windows działającą aplikację (zazwyczaj Usługa sieciowa) ma uprawnienia odczytu/zapisu dla folderów głównych aplikacji i podfoldery takich jak *aplikacji\_danych*.</span><span class="sxs-lookup"><span data-stu-id="8d010-198">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="8d010-199">Bardziej szczegółowe informacje są dostępne w artykule bazy wiedzy [problemów z wystąpień użytkownika programu SQL Server Express i projekty aplikacji sieci Web ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="8d010-199">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a><span data-ttu-id="8d010-200">Problem: Pliki, które zawiera Menedżera pakietów zasobów lub Menedżera pakietów hasła są servable w usługach IIS 6.0 i starsze wersje</span><span class="sxs-lookup"><span data-stu-id="8d010-200">Issue: Files that contains package-manager resources or package-manager passwords are servable under IIS 6.0 and earlier</span></span>

> <span data-ttu-id="8d010-201">Wdrażanie aplikacji stron sieci Web platformy ASP.NET (Razor), która została skompilowana przy użyciu wersji RC2, a ta aplikacja zawiera *password.txt* lub *packagesources.txt* plików w obszarze *App\_ Data/admin*, usług IIS 6.0 posłuży pliku, jeśli jest to wymagane, potencjalnie udostępnianie haseł dla swojego wystąpienia Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="8d010-201">If you deploy an ASP.NET Web Pages (Razor) application that was built using the RC2 release, and if the application contains a *password.txt* or *packagesources.txt* file under */App\_Data/admin*, IIS 6.0 will serve the file if requested, potentially exposing the passwords for your package manager instance.</span></span> 
> 
> <span data-ttu-id="8d010-202">**Obejście** zmienić *password.txt* lub *packagesources.txt* pliku *password.config* lub *packagesources.config*. Domyślnie usługi IIS 6.0 nie obsługiwać pliki, których *.config* rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="8d010-202">**Workaround** Rename the *password.txt* or *packagesources.txt* file to *password.config* or *packagesources.config*. By default, IIS 6.0 does not serve files that have the *.config* extension.</span></span> <span data-ttu-id="8d010-203">(W usługach IIS 7, żadne pliki w *aplikacji\_danych* folderu są obsługiwane, dzięki czemu nie trzeba zmienić nazwy plików.)</span><span class="sxs-lookup"><span data-stu-id="8d010-203">(In IIS 7, no files in the *App\_Data* folder are served, so you do not need to rename the files.)</span></span>


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a><span data-ttu-id="8d010-204">Problem: Odinstalowanie pakietów zainstalowanych przy użyciu wersji Beta 3 nie całkowicie usunąć składniki pakietu</span><span class="sxs-lookup"><span data-stu-id="8d010-204">Issue: Uninstalling packages installed using the Beta 3 release does not completely remove package components</span></span>

> <span data-ttu-id="8d010-205">Jeśli zainstalowany pakiet w wersji Beta 3 za pomocą Menedżera pakietów, a następnie spróbuj odinstalować przy użyciu bieżącej wersji, pakiet nie został całkowicie odinstalowany.</span><span class="sxs-lookup"><span data-stu-id="8d010-205">If you installed a package using the package manager in the Beta 3 release and then try to uninstall it using the current release, the package is not completely uninstalled.</span></span> <span data-ttu-id="8d010-206">Za pomocą Menedżera pakietów **Odinstaluj** przycisku spowoduje usunięcie niektórych składników, ale pozostawia kod biblioteki pakietu i aktualizuje *package.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="8d010-206">Using the package manager's **Uninstall** button removes some components, but leaves the package's library code and does not update the *package.config* file.</span></span>
> 
> <span data-ttu-id="8d010-207">**Obejście problemu** </span><span class="sxs-lookup"><span data-stu-id="8d010-207">**Workaround** </span></span>  
> <span data-ttu-id="8d010-208">Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="8d010-208">Perform these steps:</span></span>  
> 1. <span data-ttu-id="8d010-209">Usuń *aplikacji\_Data\packages* folderu.</span><span class="sxs-lookup"><span data-stu-id="8d010-209">Delete the *App\_Data\packages* folder.</span></span> <span data-ttu-id="8d010-210">Spowoduje to usunięcie wszystkich pakietów.</span><span class="sxs-lookup"><span data-stu-id="8d010-210">This removes all packages.</span></span>   
> 2. <span data-ttu-id="8d010-211">Usuń *packages.config* w katalogu głównym witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8d010-211">Delete the *packages.config* file in the root of the website.</span></span>


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a><span data-ttu-id="8d010-212">Problem: W programie Visual Studio, wywoływania Menedżera pakietów opartych na sieci web przełącza do trybu offline</span><span class="sxs-lookup"><span data-stu-id="8d010-212">Issue: In Visual Studio, invoking the web-based package manager takes the application offline</span></span>

> <span data-ttu-id="8d010-213">Jeśli pracujesz w programie Visual Studio (nie WebMatrix) i użyj  *\_admin* funkcji, aby uruchomić Menedżera pakietów, Visual Studio przełącza do trybu offline i przesyła *aplikacji\_ offline.htm* w katalogu głównym witryny sieci Web, która zakłóca możliwość korzystania z Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="8d010-213">If you are working in Visual Studio (not WebMatrix) and use the *\_admin* functionality to start the package manager, Visual Studio takes the application offline and posts the *app\_offline.htm* into the website root, which disrupts your ability to use the package manager.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="8d010-214">Mimo że najczęściej zobaczysz tego zachowania, korzystając z interfejsu Menedżera pakietów opartych na sieci web, takie samo zachowanie występuje, gdy dodać, usunąć ani zmodyfikować plików w *aplikacji\_danych* folderu.</span><span class="sxs-lookup"><span data-stu-id="8d010-214">Although you would most typically see this behavior when using the web-based package manager interface, the same behavior occurs if you add, remove, or modify any files in the *App\_Data* folder.</span></span>
> 
> <span data-ttu-id="8d010-215">**Obejście problemu** </span><span class="sxs-lookup"><span data-stu-id="8d010-215">**Workaround** </span></span>  
> <span data-ttu-id="8d010-216">Aby pracować z pakietami w programie Visual Studio, należy użyć rozszerzenia NuGet zamiast Menedżera pakietów opartych na sieci web.</span><span class="sxs-lookup"><span data-stu-id="8d010-216">To work with packages in Visual Studio, use the NuGet extension instead of the web-based package manager.</span></span> <span data-ttu-id="8d010-217">Aby uzyskać informacje, zobacz [dokumentacji NuGet](https://docs.microsoft.com/nuget/).</span><span class="sxs-lookup"><span data-stu-id="8d010-217">For information, see the [NuGet documentation](https://docs.microsoft.com/nuget/).</span></span> <span data-ttu-id="8d010-218">Jeśli pracujesz z innymi plikami w *aplikacji\_danych* folderu, pomyśl o pozostawieniu plików, aby uniknąć tego problemu w innych miejscach.</span><span class="sxs-lookup"><span data-stu-id="8d010-218">If you are working with other files in the *App\_Data* folder, consider keeping the files elsewhere to avoid this issue.</span></span> <span data-ttu-id="8d010-219">Jeśli nie jest praktyczne, Usuń *aplikacji\_offline.htm* pliku ręcznie lub poczekaj, aż lokacji wraca do trybu online automatycznie (domyślnie 30 sekund).</span><span class="sxs-lookup"><span data-stu-id="8d010-219">If that's not practical, delete the *app\_offline.htm* file manually or wait until the site comes back online automatically (by default, after 30 seconds).</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="8d010-220">Problem: Visual Studio IntelliSense projektu szablonów i dostępna tylko na platformie ASP.NET MVC w wersji 3</span><span class="sxs-lookup"><span data-stu-id="8d010-220">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="8d010-221">Instalowanie składnika ASP.NET Web Pages nie również zainstalować narzędzi dla programu Visual Studio takie jak szablony IntelliSense i projektu dla aplikacji ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="8d010-221">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="8d010-222">**Obejście** Aby używać szablonów IntelliSense i projektu dla aplikacji ASP.NET Web Pages w programie Visual Studio, zainstalować platformy ASP.NET MVC 3 RC albo za pomocą Instalatora platformy sieci Web lub [autonomicznego Instalatora](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="8d010-222">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="8d010-223">Problem: Odczytywanie źródła danych lub innych danych zewnętrznych za pośrednictwem serwera proxy</span><span class="sxs-lookup"><span data-stu-id="8d010-223">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="8d010-224">Jeśli serwer z systemem lokacji znajduje się za serwerem proxy, może być konieczne skonfigurowanie informacje dotyczące serwera proxy w *web.config* pliku, aby można było odczytać informacje, które pochodzą z poza witryną.</span><span class="sxs-lookup"><span data-stu-id="8d010-224">If the server running the site is behind a proxy server, you might need to configure proxy information in the *web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="8d010-225">Na przykład, jeśli używasz `ReCaptcha` pomocnika, pomocnika komunikuje się z usługą reCAPTCHA, ale może zostać zablokowany przez serwer proxy.</span><span class="sxs-lookup"><span data-stu-id="8d010-225">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="8d010-226">Podobnie źródła danych, które są używane w programie ASP.NET Web Pages, np. źródła danych używane przez Menedżera pakietów, mogą wymagać konfiguracji serwera proxy.</span><span class="sxs-lookup"><span data-stu-id="8d010-226">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="8d010-227">Jeśli masz problemy w pracy z zewnętrznej usługi lub Praca z pakietem źródła danych należy umieścić następujące elementy w katalogu głównego aplikacji *web.config* pliku:</span><span class="sxs-lookup"><span data-stu-id="8d010-227">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *web.config* file:</span></span>
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> <span data-ttu-id="8d010-228">Aby uzyskać więcej informacji na temat konfigurowania serwera proxy, zobacz [ &lt;proxy&gt; elementu (ustawienia sieciowe)](https://msdn.microsoft.com/library/sa91de1e.aspx) w witrynie MSDN.</span><span class="sxs-lookup"><span data-stu-id="8d010-228">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="8d010-229">Problem: Odinstalowywanie programu .NET Framework w wersji 4 wyłącza stron ASP.NET Web Pages o składni Razor</span><span class="sxs-lookup"><span data-stu-id="8d010-229">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="8d010-230">Po odinstalowaniu programu .NET Framework w wersji 4 i zainstaluj go ponownie, ASP.NET Web Pages o składni Razor jest wyłączona.</span><span class="sxs-lookup"><span data-stu-id="8d010-230">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="8d010-231">Strony z *.cshtml* rozszerzenia nie działać prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="8d010-231">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="8d010-232">Strony ASP.NET Web Pages rejestruje zestawu w folderze głównym maszyny *web.config* plików i usunięcie programu .NET Framework spowoduje usunięcie tego pliku.</span><span class="sxs-lookup"><span data-stu-id="8d010-232">ASP.NET Web Pages registers an assembly in the machine root *web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="8d010-233">Ponowna instalacja programu .NET Framework instaluje nową wersję pliku konfiguracji, ale nie dodać odwołanie do zestawu stron sieci Web programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8d010-233">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="8d010-234">**Obejście** po ponownym zainstalowaniu programu .NET Framework, zainstaluj ponownie stron ASP.NET Web Pages o składni Razor.</span><span class="sxs-lookup"><span data-stu-id="8d010-234">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="8d010-235">Spowoduje to dodanie następujący element do *web.config* w katalogu głównym komputera, który zazwyczaj znajduje się w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="8d010-235">This adds the following element to the *web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="8d010-236">Problem: Adresy URL bez rozszerzeń nie zostanie znaleziony.cshtml/.vbhtml pliki w usługach IIS 7 i IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="8d010-236">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="8d010-237">W usługach IIS 7 lub usług IIS 7.5 żądania o adresie URL podobnie do następującej nie są w stanie odnaleźć strony, które mają *.cshtml* lub *.vbhtml* rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="8d010-237">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="8d010-238">Problem występuje, ponieważ ponowne zapisywanie adresów URL nie jest włączona domyślnie dla usług IIS 7 i IIS 7.5.</span><span class="sxs-lookup"><span data-stu-id="8d010-238">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="8d010-239">Scenariusz likeliest jest, że nie ma problem podczas testowania, lokalnie za pomocą usług IIS Express, ale wystąpić podczas wdrażania witryny sieci Web do obsługi witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8d010-239">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="8d010-240">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-240">**Workaround**</span></span>
> 
> - <span data-ttu-id="8d010-241">Jeśli masz kontrolę nad komputerem serwera na komputerze serwera, zainstaluj aktualizację opisaną w [aktualizacja jest dostępna, że umożliwia niektórych obsługi usług IIS 7.0 lub 7.5 usług IIS do obsługi żądań, których adresy URL nie kończą się kropką](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="8d010-241">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="8d010-242">Jeśli nie masz kontrolę nad komputerem serwera (na przykład wdrażasz do obsługi witryny sieci Web), dodaj następującą wartość do witryny sieci Web *web.config* pliku:</span><span class="sxs-lookup"><span data-stu-id="8d010-242">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *web.config* file:</span></span> 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="8d010-243">Problem: Wdrażanie aplikacji na komputerze, na którym nie ma zainstalowany program SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="8d010-243">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="8d010-244">Aplikacje, które obejmują baz danych programu SQL Server Compact można uruchomić na komputerze, na którym program SQL Server Compact nie jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="8d010-244">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="8d010-245">Microsoft WebMatrix 1.0 automatycznie kopiuje tych plików binarnych dla Ciebie i wykonuje odpowiednie *web.config* pliku transformacji.</span><span class="sxs-lookup"><span data-stu-id="8d010-245">Microsoft WebMatrix 1.0 automatically copies these binaries for you and performs the appropriate *web.config* file transforms.</span></span>
> 
> <span data-ttu-id="8d010-246">**Obejście** Jeśli musisz skopiować te pliki i upewnić *web.config* zmiany pliku ręcznie, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="8d010-246">**Workaround** If you need to copy these files and make the *web.config* file changes manually, do the following:</span></span>
> 
> 1. <span data-ttu-id="8d010-247">Kopiowanie zestawów aparatu bazy danych do *Bin* folderze (i jego podfolderach) aplikacji na komputerze docelowym:</span><span class="sxs-lookup"><span data-stu-id="8d010-247">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span>  
> 
>    - <span data-ttu-id="8d010-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span><span class="sxs-lookup"><span data-stu-id="8d010-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span></span>  
>        <span data-ttu-id="8d010-249">**Aby** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="8d010-249">**to** *\Bin*</span></span>
>    - <span data-ttu-id="8d010-250">Kopiuj <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>do</em></strong>\Bin\x86\*</span><span class="sxs-lookup"><span data-stu-id="8d010-250">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>to</em></strong>\Bin\x86\*</span></span>
>    - <span data-ttu-id="8d010-251">Kopiuj <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>do</strong><em>\Bin\amd64</em></span><span class="sxs-lookup"><span data-stu-id="8d010-251">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>to</strong><em>\Bin\amd64</em></span></span>
> 
> 2. <span data-ttu-id="8d010-252">W folderze głównym witryny sieci Web, należy utworzyć lub otworzyć *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="8d010-252">In the root folder of the website, create or open a *web.config* file.</span></span> <span data-ttu-id="8d010-253">(W wersji 1.0 programu WebMatrix, ten typ pliku jest dostępna po kliknięciu **wszystkie** w **wybierz typ pliku** okno dialogowe.)</span><span class="sxs-lookup"><span data-stu-id="8d010-253">(In WebMatrix 1.0, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="8d010-254">Dodaj następujący element jako element podrzędny `<configuration>` elementu (nie znajduje się w `<system.web>` element):</span><span class="sxs-lookup"><span data-stu-id="8d010-254">Add the following element as a child of the `<configuration>` element (not inside the `<system.web>` element):</span></span>
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="8d010-255">Problem: "Baza danych" i "WebGrid" pomocników nie działają w trybie średniego zaufania w języku Visual Basic</span><span class="sxs-lookup"><span data-stu-id="8d010-255">Issue: "Database" and "WebGrid" helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="8d010-256">Jeśli używasz programu Visual Basic (Tworzenie *.vbhtml* plików), `Database` i `WebGrid` pomocników nie będzie działać, jeśli aplikacja jest skonfigurowana do użycia w trybie średniego zaufania.</span><span class="sxs-lookup"><span data-stu-id="8d010-256">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="8d010-257">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-257">**Workaround**</span></span>  
> <span data-ttu-id="8d010-258">Jeśli używasz programu Visual Studio 2010, ten problem można rozwiązać przez zainstalowanie wersji dodatku Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="8d010-258">If you use Visual Studio 2010, you can resolve this problem by installing the Service Pack 1 release.</span></span> <span data-ttu-id="8d010-259">Dopóki z ostateczną wersją wersji z dodatkiem SP1 jest dostępny, można pobrać wersji Beta programu z dodatkiem SP1 z [Microsoft Visual Studio 2010 z dodatkiem Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) stronie w witrynie Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="8d010-259">Until the final version of the SP1 release is available, you can download the Beta version of SP1 from the [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) page on the Microsoft Download Center.</span></span>   
>   
> <span data-ttu-id="8d010-260">Jeśli to nie jest praktyczne lub jeśli nie używasz programu Visual Studio 2010, można tymczasowo ustawić aplikacji do korzystania z pełnego zaufania.</span><span class="sxs-lookup"><span data-stu-id="8d010-260">If this is not practical, or if you do not use Visual Studio 2010, you can temporarily set the application to use Full Trust.</span></span>


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a><span data-ttu-id="8d010-261">Problem: "ApplicationPart" zasoby są dostępne z zewnątrz</span><span class="sxs-lookup"><span data-stu-id="8d010-261">Issue: "ApplicationPart" resources are externally accessible</span></span>

> <span data-ttu-id="8d010-262">Jeśli zestaw zawiera obiekty, które pochodzi z `ApplicationPart` klasy, że zasobów zestawu są udostępniane przez `ResourceRouteHandler` klasy.</span><span class="sxs-lookup"><span data-stu-id="8d010-262">If an assembly contains objects that derives from the `ApplicationPart` class, that assembly's resources are exposed by the `ResourceRouteHandler` class.</span></span> <span data-ttu-id="8d010-263">Rozważmy na przykład następujący adres URL:</span><span class="sxs-lookup"><span data-stu-id="8d010-263">For example, consider the following URL:</span></span>  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> <span data-ttu-id="8d010-264">To żądanie pobiera wszystkie ciągi zasobów w *System.Web.WebPages.Administration.dll* zestawu.</span><span class="sxs-lookup"><span data-stu-id="8d010-264">This request downloads all of the resource strings in the *System.Web.WebPages.Administration.dll* assembly.</span></span> <span data-ttu-id="8d010-265">Pobierane są wszystkie zasoby osadzone (nawet te, które nie mają być obsługiwane jako zawartość statyczna).</span><span class="sxs-lookup"><span data-stu-id="8d010-265">All of the embedded resources (even those that are not intended to be served as static content) are downloaded.</span></span> <span data-ttu-id="8d010-266">Jeśli zasoby osadzone zawierają poufne informacje, to stanowią zagrożenie bezpieczeństwa.</span><span class="sxs-lookup"><span data-stu-id="8d010-266">If the embedded resources contain sensitive information, this can represent a security risk.</span></span> 
> 
> <span data-ttu-id="8d010-267">**Obejście problemu** </span><span class="sxs-lookup"><span data-stu-id="8d010-267">**Workaround** </span></span>  
> <span data-ttu-id="8d010-268">W przypadku utworzenia **ApplicationPart** obiektów, upewnij się, że zasoby osadzone skojarzone z tym **ApplicationPart** obiektu zestawu nie zawierają informacji poufnych.</span><span class="sxs-lookup"><span data-stu-id="8d010-268">If you create an **ApplicationPart** object, make sure that the embedded resources associated with that **ApplicationPart** object's assembly do not contain sensitive information.</span></span>


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a><span data-ttu-id="8d010-269">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="8d010-269">WebMatrix</span></span>

> [!NOTE]
> <span data-ttu-id="8d010-270">Aby uzyskać informacje dotyczące problemów z instalacją dla programu WebMatrix, zobacz [problemy z instalacją programu WebMatrix](#Known_Issues_Installation) we wcześniejszej części tego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="8d010-270">For information about installation issues for WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>


<span data-ttu-id="8d010-271">W tej sekcji dokumentu opisano znane problemy dotyczące środowiska programowania programu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="8d010-271">This section of the document describes known issues for the WebMatrix development environment.</span></span>

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a><span data-ttu-id="8d010-272">Problem: Zmiany w nazwa użytkownika lub hasło ciągu połączenia bazy danych w pliku web.config nie są widoczne w obszarze roboczym baz danych</span><span class="sxs-lookup"><span data-stu-id="8d010-272">Issue: Changes in the username or password of a database connection string in a web.config file are not reflected in the Databases workspace</span></span>

> <span data-ttu-id="8d010-273">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-273">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="8d010-274">W *web.config* pliku, Zmień nazwę bazy danych w parametrach połączenia (na przykład dodać "1" do niego).</span><span class="sxs-lookup"><span data-stu-id="8d010-274">In the *web.config* file, change the database name in the connection string (for example, add "1" to it).</span></span>
> 2. <span data-ttu-id="8d010-275">Zapisz *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="8d010-275">Save the *web.config* file.</span></span>
> 3. <span data-ttu-id="8d010-276">Kliknij przycisk **baz danych** i Odśwież.</span><span class="sxs-lookup"><span data-stu-id="8d010-276">Click **Databases** and refresh.</span></span>
> 4. <span data-ttu-id="8d010-277">Zmień nazwę bazy danych w parametrach połączenia w *web.config* pliku do oryginalnej nazwy bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8d010-277">Change the database name in the connection string in the *web.config* file back to the original database name.</span></span>
> 5. <span data-ttu-id="8d010-278">Zapisz *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="8d010-278">Save the *web.config* file.</span></span>
> 6. <span data-ttu-id="8d010-279">Kliknij przycisk **baz danych** i Odśwież.</span><span class="sxs-lookup"><span data-stu-id="8d010-279">Click **Databases** and refresh.</span></span>


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a><span data-ttu-id="8d010-280">Problem: Nie można usunąć folderami utworzonych przez program WebMatrix</span><span class="sxs-lookup"><span data-stu-id="8d010-280">Issue: Folders created by WebMatrix cannot be deleted</span></span>

> <span data-ttu-id="8d010-281">Jeśli program WebMatrix jest uruchomiona, korzystając z podwyższonym poziomem uprawnień (to znaczy uruchomiona za pomocą programu WebMatrix **Uruchom jako Administrator** opcji w systemie Windows), folderów, które zostały utworzone przy użyciu programu WebMatrix, nie można usunąć przy użyciu Eksploratora Windows.</span><span class="sxs-lookup"><span data-stu-id="8d010-281">If WebMatrix is running using elevated permissions (that is, you started WebMatrix using the **Run as Administrator** option in Windows), folders that are created by WebMatrix cannot be deleted using Windows Explorer.</span></span>
> 
> <span data-ttu-id="8d010-282">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-282">**Workaround**</span></span>  
> <span data-ttu-id="8d010-283">Uruchom Eksploratora Windows, korzystając z podwyższonym poziomem uprawnień.</span><span class="sxs-lookup"><span data-stu-id="8d010-283">Run Windows Explorer using elevated permissions.</span></span> <span data-ttu-id="8d010-284">Wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="8d010-284">Follow these steps:</span></span>  
> 
> 1. <span data-ttu-id="8d010-285">W systemie Windows, kliknij przycisk **Start**.</span><span class="sxs-lookup"><span data-stu-id="8d010-285">In Windows, click **Start**.</span></span>
> 2. <span data-ttu-id="8d010-286">Wprowadź "Eksploratora Windows", a następnie kliknij prawym przyciskiem myszy pozycję **Eksploratora Windows**.</span><span class="sxs-lookup"><span data-stu-id="8d010-286">Enter "Windows Explorer" and right-click the entry for **Windows Explorer**.</span></span>
> 3. <span data-ttu-id="8d010-287">Kliknij przycisk **Uruchom jako Administrator**.</span><span class="sxs-lookup"><span data-stu-id="8d010-287">Click **Run as Administrator**.</span></span> <span data-ttu-id="8d010-288">Następnie można usunąć foldery.</span><span class="sxs-lookup"><span data-stu-id="8d010-288">You can then delete the folders.</span></span>


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="8d010-289">Problem: WebMatrix 1.0 nie jest w stanie do wykonania niektórych zadań, które wymagają podniesionych uprawnień</span><span class="sxs-lookup"><span data-stu-id="8d010-289">Issue: WebMatrix 1.0 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="8d010-290">Program WebMatrix w wersji 1.0 nie jest w stanie do wykonania niektórych zadań, które wymagają podniesionych uprawnień, takich jak instalowanie dodatkowych składników w następujących sytuacjach:</span><span class="sxs-lookup"><span data-stu-id="8d010-290">WebMatrix 1.0 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="8d010-291">W systemie Windows Vista lub Windows 7 zalogowano się za pomocą konta, które nie ma uprawnień administracyjnych i kontroli konta użytkownika (UAC) jest wyłączona.</span><span class="sxs-lookup"><span data-stu-id="8d010-291">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="8d010-292">Używasz systemu Microsoft Windows XP lub Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="8d010-292">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="8d010-293">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-293">**Workaround**</span></span>  
> <span data-ttu-id="8d010-294">Większość zadań w programie WebMatrix 1.0 nie wymaga uprawnień administracyjnych.</span><span class="sxs-lookup"><span data-stu-id="8d010-294">Most tasks in WebMatrix 1.0 do not require administrative permission.</span></span> <span data-ttu-id="8d010-295">Dla tych, które wykonują można wykonać operacji jako administrator lub wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="8d010-295">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="8d010-296">W systemie Windows Vista lub Windows 7 należy włączyć funkcji Kontrola konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8d010-296">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="8d010-297">W systemie Windows XP należy dodać użytkownika do grupy zabezpieczeń Administratorzy.</span><span class="sxs-lookup"><span data-stu-id="8d010-297">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="8d010-298">Problem: "Lokacji z galerii" jest wyłączona.</span><span class="sxs-lookup"><span data-stu-id="8d010-298">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="8d010-299">**Witryny sieci Web galerii** opcja jest wyłączona, jeśli Instalator platformy sieci Web 3.0 nie jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="8d010-299">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="8d010-300">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-300">**Workaround**</span></span>  
> <span data-ttu-id="8d010-301">Zainstaluj [Instalatora platformy sieci Web firmy Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="8d010-301">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="8d010-302">Problem: Google Chrome nie jest dostępna jako opcja wykonywania</span><span class="sxs-lookup"><span data-stu-id="8d010-302">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="8d010-303">Google Chrome nie jest wyświetlana na liście przeglądarek w obszarze **Uruchom** na **Home** kartę.</span><span class="sxs-lookup"><span data-stu-id="8d010-303">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="8d010-304">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-304">**Workaround**</span></span>  
> <span data-ttu-id="8d010-305">Niektóre wersje programu Google Chrome nie rejestrują się poprawnie z funkcją programy domyślne w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="8d010-305">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="8d010-306">Jako obejście, należy uruchomić Google Chrome, kliknij polecenie *Dostosuj i kontroli Google Chrome* menu, kliknij przycisk *opcje*, a następnie kliknij przycisk *upewnij Google Chrome przeglądarce domyślne*.</span><span class="sxs-lookup"><span data-stu-id="8d010-306">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="8d010-307">Problem: Okno dialogowe "Klucz obcy" nie zezwala na wprowadzenie klucza podstawowego</span><span class="sxs-lookup"><span data-stu-id="8d010-307">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="8d010-308">**Klucz obcy** okno dialogowe nie pozwala na wprowadzanie nazwy klucza podstawowego z tabeli klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="8d010-308">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="8d010-309">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-309">**Workaround**</span></span>  
> <span data-ttu-id="8d010-310">Jest to zamierzone.</span><span class="sxs-lookup"><span data-stu-id="8d010-310">This is intentional.</span></span> <span data-ttu-id="8d010-311">Nie trzeba wprowadzić nazwę klucza podstawowego z tabeli klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="8d010-311">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a><span data-ttu-id="8d010-312">Problem: IntelliSense nie jest dostępna w programie WebMatrix dla Razor składni języka C# i Visual Basic</span><span class="sxs-lookup"><span data-stu-id="8d010-312">Issue: IntelliSense is not available in WebMatrix for Razor syntax, C#, or Visual Basic</span></span>

> <span data-ttu-id="8d010-313">IntelliSense jest obsługiwana w programie WebMatrix dla kodu HTML i CSS.</span><span class="sxs-lookup"><span data-stu-id="8d010-313">IntelliSense is supported in WebMatrix for HTML and CSS.</span></span> <span data-ttu-id="8d010-314">Jednak nie jest dostępna dla innych języków.</span><span class="sxs-lookup"><span data-stu-id="8d010-314">However, it is not available for other languages.</span></span> 
> 
> <span data-ttu-id="8d010-315">**Obejście problemu** </span><span class="sxs-lookup"><span data-stu-id="8d010-315">**Workaround** </span></span>  
> <span data-ttu-id="8d010-316">Brak.</span><span class="sxs-lookup"><span data-stu-id="8d010-316">None.</span></span>


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a><span data-ttu-id="8d010-317">Problem: Elementy, które nie są odpowiednie kontekstowej sugeruje IntelliSense dla kodu HTML i CSS</span><span class="sxs-lookup"><span data-stu-id="8d010-317">Issue: IntelliSense for HTML and CSS suggests elements that are not contextually appropriate</span></span>

> <span data-ttu-id="8d010-318">IntelliSense dla znaczników w programie WebMatrix obsługuje HTML za pomocą [XHTML 1.0 przejściowe schematu](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) i CSS przy użyciu [schematu CSS 2.1](http://www.w3.org/TR/CSS2/).</span><span class="sxs-lookup"><span data-stu-id="8d010-318">IntelliSense for markup in WebMatrix supports HTML using the [XHTML 1.0 Transitional schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) and CSS using the [CSS 2.1 schema](http://www.w3.org/TR/CSS2/).</span></span> <span data-ttu-id="8d010-319">Ponieważ IntelliSense jest oparta na tych określonych schematów, niektórych znaczników, atrybutów i właściwości mogą sugerowane, które nie są odpowiednie dla bieżącej definicji stylu lub strony.</span><span class="sxs-lookup"><span data-stu-id="8d010-319">Because IntelliSense is based on these specific schemas, certain tags, attributes, or properties might be suggested that are not appropriate for the current page or style definition.</span></span> <span data-ttu-id="8d010-320">Dla kodu HTML może on również prowadzić do sugestie nieoczekiwany w zawartości, które mogą być interpretowane jako źle sformułowane XHTML (na przykład gdy tagi nie są zamknięte).</span><span class="sxs-lookup"><span data-stu-id="8d010-320">For HTML, it can also lead to unexpected suggestions in content that might be interpreted as malformed XHTML (for example, when tags are not closed).</span></span> <span data-ttu-id="8d010-321">Ten problem może być bardziej zauważalne, jeśli punkt wstawiania znajduje się wewnątrz tagu niekompletne; w takim przypadku IntelliSense może sugerować otwierania nowych znaczników lub oferuje inne nieprawidłowe sugestie.</span><span class="sxs-lookup"><span data-stu-id="8d010-321">This issue might be more noticeable if the insertion point is inside an incomplete tag; in that case, IntelliSense might suggest new opening tags or offer other incorrect suggestions.</span></span> 
> 
> <span data-ttu-id="8d010-322">**Obejście problemu** </span><span class="sxs-lookup"><span data-stu-id="8d010-322">**Workaround** </span></span>  
> <span data-ttu-id="8d010-323">Dla kodu HTML upewnij się, że pracujesz na stronie XHTML poprawnie sformułowany, pełną.</span><span class="sxs-lookup"><span data-stu-id="8d010-323">For HTML, make sure that you are working within a well-formed, complete XHTML page.</span></span> <span data-ttu-id="8d010-324">CSS nie istnieje obejście.</span><span class="sxs-lookup"><span data-stu-id="8d010-324">For CSS, there is no workaround.</span></span>


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a><span data-ttu-id="8d010-325">Problem: IntelliSense nie zostanie wywołany, podczas pisania</span><span class="sxs-lookup"><span data-stu-id="8d010-325">Issue: IntelliSense is not invoked while you type</span></span>

> <span data-ttu-id="8d010-326">W czasie IntelliSense nie może być wywołany, jako wprowadzania w edytorze HTML i CSS.</span><span class="sxs-lookup"><span data-stu-id="8d010-326">At times, IntelliSense might not be invoked as HTML or CSS is being entered in the editor.</span></span> <span data-ttu-id="8d010-327">W szczególności to może się zdarzyć, gdy punkt wstawiania znajduje się bezpośrednio obok inny element lub na końcu pliku.</span><span class="sxs-lookup"><span data-stu-id="8d010-327">In particular, this might happen when the insertion point is directly next to another element or at the end of a file.</span></span> 
> 
> <span data-ttu-id="8d010-328">**Obejście problemu** </span><span class="sxs-lookup"><span data-stu-id="8d010-328">**Workaround** </span></span>  
> <span data-ttu-id="8d010-329">Upewnij się, że jest odstępem wokół punktu wstawiania i nie jest punkt wstawiania na końcu pliku.</span><span class="sxs-lookup"><span data-stu-id="8d010-329">Make sure that there is whitespace around the insertion point and that the insertion point is not at the end of a file.</span></span> <span data-ttu-id="8d010-330">IntelliSense można również ręcznie wywoływać, naciskając klawisze Ctrl + spacja.</span><span class="sxs-lookup"><span data-stu-id="8d010-330">You can also invoke IntelliSense manually by pressing Ctrl+Space.</span></span>


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a><span data-ttu-id="8d010-331">Problem: Bez interfejsu użytkownika jest dostępna dla wyłączenie IntelliSense</span><span class="sxs-lookup"><span data-stu-id="8d010-331">Issue: No UI is available for disabling IntelliSense</span></span>

> <span data-ttu-id="8d010-332">1.0 dla programu WebMatrix zapewnia nie interfejsu użytkownika lub gestu wyłączenie funkcji IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="8d010-332">WebMatrix 1.0 provides no UI or gesture for disabling IntelliSense.</span></span> 
> 
> <span data-ttu-id="8d010-333">**Obejście problemu** </span><span class="sxs-lookup"><span data-stu-id="8d010-333">**Workaround** </span></span>  
> <span data-ttu-id="8d010-334">Uruchom program WebMatrix za pomocą następujące polecenie, w tym przełącznika, które wyłączają IntelliSense:</span><span class="sxs-lookup"><span data-stu-id="8d010-334">Start WebMatrix using the following command, which includes a switch that disables IntelliSense:</span></span>  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a><span data-ttu-id="8d010-335">Usługi IIS Express</span><span class="sxs-lookup"><span data-stu-id="8d010-335">IIS Express</span></span>

<span data-ttu-id="8d010-336">Usługi IIS Express ma własny plik readme, który jest dostępny pod adresem URL:</span><span class="sxs-lookup"><span data-stu-id="8d010-336">IIS Express has its own readme file, which is available at the following URL:</span></span>

[<span data-ttu-id="8d010-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="8d010-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span></span>](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a><span data-ttu-id="8d010-338">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="8d010-338">SQL Server Compact</span></span>

<span data-ttu-id="8d010-339">SQL Server Compact ma własny plik readme, który jest dostępny pod adresem URL:</span><span class="sxs-lookup"><span data-stu-id="8d010-339">SQL Server Compact has its own readme file, which is available at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

<span data-ttu-id="8d010-340">Aby uzyskać informacje o problemach, które dotyczą instalowania programu SQL Server Compact jako część programu WebMatrix, zobacz [problemy z instalacją programu WebMatrix](#Known_Issues_Installation) we wcześniejszej części tego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="8d010-340">For information about issues that involve installing SQL Server Compact as part of WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>

### <a id="Known_Issues_Installing_Applications"></a>  <span data-ttu-id="8d010-341">Instalowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="8d010-341">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="8d010-342">Problem: Instalowanie aplikacji może długo trwać, jeśli folder Moje dokumenty użytkownika jest przekierowywany do udziału sieciowego</span><span class="sxs-lookup"><span data-stu-id="8d010-342">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="8d010-343">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-343">**Workaround**</span></span>  
> <span data-ttu-id="8d010-344">Brak.</span><span class="sxs-lookup"><span data-stu-id="8d010-344">None.</span></span> <span data-ttu-id="8d010-345">Aplikacja może potrwać kilka minut, aby zainstalować, ale zostanie zainstalowany poprawnie.</span><span class="sxs-lookup"><span data-stu-id="8d010-345">The application might take a while to install, but will install correctly.</span></span>


### <a id="Known_Issues_Publishing_Applications"></a>  <span data-ttu-id="8d010-346">Publikowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="8d010-346">Publishing Applications</span></span>

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a><span data-ttu-id="8d010-347">Problem: "wymaga się, że nie można uzyskać uprawnienia" błąd podczas publikowania bazy danych SQL Compact</span><span class="sxs-lookup"><span data-stu-id="8d010-347">Issue: "Required permissions cannot be acquired" error when publishing a SQL Compact Database</span></span>

> <span data-ttu-id="8d010-348">Program WebMatrix nie w pełni obsługuje wdrażanie obsługi plików binarnych dla programu SQL Server Compact do serwera z programem .NET Framework w wersji 3.5 i konfiguracji trybie średniego zaufania.</span><span class="sxs-lookup"><span data-stu-id="8d010-348">WebMatrix does not fully support deploying supporting binaries for SQL Server Compact to a server that is running .NET Framework version 3.5 with a medium trust configuration.</span></span>
> 
> <span data-ttu-id="8d010-349">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-349">**Workaround**</span></span>  
> <span data-ttu-id="8d010-350">Preferowany obejście polega na zainstalowaniu programu .NET Framework 4 na serwerze.</span><span class="sxs-lookup"><span data-stu-id="8d010-350">The preferred workaround is to install the .NET Framework 4 on the server.</span></span> <span data-ttu-id="8d010-351">Można również wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="8d010-351">Alternatively, do the following:</span></span>
> 
> 1. <span data-ttu-id="8d010-352">Dodaj następujące elementy do `SecurityClasses` sekcji *Web\_MediumTrust.config* pliku:</span><span class="sxs-lookup"><span data-stu-id="8d010-352">Add the following elements to the `SecurityClasses` section in *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. <span data-ttu-id="8d010-353">Utwórz nowy zestaw uprawnień na *Web\_MediumTrust.config* pliku wymagane następujące uprawnienia:</span><span class="sxs-lookup"><span data-stu-id="8d010-353">Create a new permission set in the *Web\_MediumTrust.config* file with the following required permissions:</span></span>
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. <span data-ttu-id="8d010-354">Zastosuj uprawnienia do programu SQL Server Compact, ustawiając następujące elementy *Web\_MediumTrust.config* pliku:</span><span class="sxs-lookup"><span data-stu-id="8d010-354">Apply the permission set to SQL Server Compact by putting the following elements in the *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a><span data-ttu-id="8d010-355">Problem: Galerii i PhpBB aplikacji sieci web wyświetla błąd "Usługa jest niedostępna" po opublikowaniu</span><span class="sxs-lookup"><span data-stu-id="8d010-355">Issue: Gallery and PhpBB web applications display a "Service is unavailable" error after publishing</span></span>

> <span data-ttu-id="8d010-356">W pewnych okolicznościach publikowania aplikacji powoduje błąd "Usługa jest niedostępna".</span><span class="sxs-lookup"><span data-stu-id="8d010-356">Under some circumstances, publishing an application causes a "service is unavailable" error.</span></span>
> 
> <span data-ttu-id="8d010-357">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-357">**Workaround**</span></span>  
> <span data-ttu-id="8d010-358">W programie WebMatrix, Dodaj ukośnik odwrotny (\) na końcu nazwy serwera w **ustawień publikowania** okna, a następnie opublikować ponownie aplikację.</span><span class="sxs-lookup"><span data-stu-id="8d010-358">In WebMatrix, add a backslash (\) to the end of the server name in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a><span data-ttu-id="8d010-359">Problem: Układ WWW Moodle i łącza są przerwane po opublikowaniu</span><span class="sxs-lookup"><span data-stu-id="8d010-359">Issue: Moodle website layout and links are broken after publishing</span></span>

> <span data-ttu-id="8d010-360">Po opublikowaniu aplikacji Moodle, aplikacja nie działa prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="8d010-360">After you publish a Moodle application, the application does not work correctly.</span></span>
> 
> <span data-ttu-id="8d010-361">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-361">**Workaround**</span></span>  
> <span data-ttu-id="8d010-362">W programie WebMatrix, Dodaj ukośnika (/) na końcu **nazwa witryny** w **ustawień publikowania** okna, a następnie opublikować ponownie aplikację.</span><span class="sxs-lookup"><span data-stu-id="8d010-362">In WebMatrix, add a slash (/) to the end of the **Site Name** field in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a><span data-ttu-id="8d010-363">Problem: Publikowanie nopCommerce zakończy się niepowodzeniem z powodu błędu bazy danych</span><span class="sxs-lookup"><span data-stu-id="8d010-363">Issue: Publishing nopCommerce fails with a database error</span></span>

> <span data-ttu-id="8d010-364">Program nopCommerce publikowania nie powiedzie się i zgłasza błąd bazy danych, takich jak "wstawiony nop\_tabeli dziennika nie powiodło się."</span><span class="sxs-lookup"><span data-stu-id="8d010-364">Publishing nopCommerce fails and reports a database error like "Insert into the nop\_log table failed."</span></span>
> 
> <span data-ttu-id="8d010-365">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-365">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="8d010-366">W programie WebMatrix, kliknij przycisk **Uruchom** można uruchomić program nopCommerce lokalnie.</span><span class="sxs-lookup"><span data-stu-id="8d010-366">In WebMatrix, click **Run** to launch nopCommerce locally.</span></span>
> 2. <span data-ttu-id="8d010-367">Zaloguj się do strony Administracja.</span><span class="sxs-lookup"><span data-stu-id="8d010-367">Log into the administration page.</span></span>
> 3. <span data-ttu-id="8d010-368">Kliknij przycisk **systemu** menu.</span><span class="sxs-lookup"><span data-stu-id="8d010-368">Click the **System** menu.</span></span>
> 4. <span data-ttu-id="8d010-369">Kliknij przycisk **dziennika** opcji.</span><span class="sxs-lookup"><span data-stu-id="8d010-369">Click the **Log** option.</span></span>
> 5. <span data-ttu-id="8d010-370">Kliknij przycisk **Wyczyść dziennik** przycisku.</span><span class="sxs-lookup"><span data-stu-id="8d010-370">Click the **Clear Log** button.</span></span>
> 6. <span data-ttu-id="8d010-371">Program nopCommerce ponownie opublikować.</span><span class="sxs-lookup"><span data-stu-id="8d010-371">Publish nopCommerce again.</span></span>


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a><span data-ttu-id="8d010-372">Problem: Silverstripe CMS Wyświetla "HTTP 500 PHP FCGI Error" po pobraniu opublikowanej witryny</span><span class="sxs-lookup"><span data-stu-id="8d010-372">Issue: Silverstripe CMS displays a "HTTP 500 PHP FCGI Error" when you download a published site</span></span>

> <span data-ttu-id="8d010-373">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-373">**Workaround**</span></span>  
> <span data-ttu-id="8d010-374">Po kliknięciu **pobierania opublikowane lokacji**, Pomiń `silverstripe-cache/manifest_main` w **Podgląd publikowania**.</span><span class="sxs-lookup"><span data-stu-id="8d010-374">After you click **Download published site**, skip `silverstripe-cache/manifest_main` in **Publish Preview**.</span></span> <span data-ttu-id="8d010-375">Ten plik jest używany na potrzeby buforowania i jest specyficzna dla każdego komputera.</span><span class="sxs-lookup"><span data-stu-id="8d010-375">This file is used for caching purposes and is specific to each computer.</span></span>


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a><span data-ttu-id="8d010-376">Problem: "Błąd serwera w"/"aplikacja" Wyświetla Subtext podczas pobierania opublikowanej witryny</span><span class="sxs-lookup"><span data-stu-id="8d010-376">Issue: Subtext displays "Server Error in '/' Application" when you download a published site</span></span>

> <span data-ttu-id="8d010-377">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-377">**Workaround**</span></span>  
> <span data-ttu-id="8d010-378">Otwórz witrynę *web.config* plików i zastąp nazwę użytkownika i hasło w ciągu połączenia bazy danych przy użyciu poświadczeń administratora programu SQL Server (poświadczenia "sa").</span><span class="sxs-lookup"><span data-stu-id="8d010-378">Open the site's *web.config* file and replace the user ID and password in the database connection string with the SQL Server administrator credentials (the "sa" credentials).</span></span>
> 
> <span data-ttu-id="8d010-379">Alternatywnie, wykonaj następujące kroki, aby nadać kontu użytkownika, użytkownik jest zalogowany przy użyciu `db_owner` uprawnienia:</span><span class="sxs-lookup"><span data-stu-id="8d010-379">Alternatively, follow these steps in order to give the user account you are logged in with `db_owner` permissions:</span></span>
> 
> 1. <span data-ttu-id="8d010-380">Zainstaluj program SQL Server Management Studio za pomocą Instalatora platformy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8d010-380">Install SQL Server Management Studio using the Web Platform Installer.</span></span>
> 2. <span data-ttu-id="8d010-381">Połączenie z lokalnym wystąpieniem programu SQL Server Express (domyślnie `.\SQLEXPRESS`).</span><span class="sxs-lookup"><span data-stu-id="8d010-381">Connect to the local SQL Server Express instance (by default, `.\SQLEXPRESS`).</span></span>
> 3. <span data-ttu-id="8d010-382">Kliknij przycisk **baz danych** &gt; *[localSubtextDatabase]* &gt; **zabezpieczeń** &gt; **użytkowników** &gt; *[localSubtextUser*] (domyślnie jest `subtextuser`], kliknij prawym przyciskiem myszy, a następnie kliknij przycisk **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="8d010-382">Click **Databases** &gt; *[localSubtextDatabase]* &gt; **Security** &gt; **Users** &gt; *[localSubtextUser*] (default is `subtextuser`], right-click, and click **Properties**.</span></span>
> 4. <span data-ttu-id="8d010-383">Wybierz **db\_właściciela** w sekcji członkostwo roli.</span><span class="sxs-lookup"><span data-stu-id="8d010-383">Select **db\_owner** in the role membership section.</span></span>


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="8d010-384">Problem: Lokacji może nie działać po opublikowaniu, jeśli w polu "Docelowy adres URL" nie jest poprzedzona http:// lub https://</span><span class="sxs-lookup"><span data-stu-id="8d010-384">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="8d010-385">W **ustawienia publikowania** okno dialogowe, jeśli docelowy adres URL nie rozpoczyna się od `http://` lub `https://`, lokacji może nie działać po wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="8d010-385">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="8d010-386">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-386">**Workaround**</span></span>  
> <span data-ttu-id="8d010-387">Upewnij się, że przed opublikowaniem witryny, docelowy adres URL na **ustawień publikowania** okno dialogowe rozpoczyna się od `http://` lub `https://`.</span><span class="sxs-lookup"><span data-stu-id="8d010-387">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="8d010-388">Problem: Publikowanie bazy danych MySQL zakończy się niepowodzeniem z powodu błędu "nie można opublikować bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8d010-388">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="8d010-389">Może to nastąpić w przypadku zdalnej bazy danych nie można uruchomić skryptu."</span><span class="sxs-lookup"><span data-stu-id="8d010-389">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="8d010-390">Błąd może wystąpić z kilku powodów.</span><span class="sxs-lookup"><span data-stu-id="8d010-390">The error can occur for a number of reasons.</span></span> <span data-ttu-id="8d010-391">Jedną z przyczyn, zostanie wyświetlony ten błąd jest Jeśli skryptu bazy danych zawiera znak pojedynczego cudzysłowu ('), a domyślny zestaw znaków miejsce docelowe danych MySQL nie jest UTF-8.</span><span class="sxs-lookup"><span data-stu-id="8d010-391">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="8d010-392">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-392">**Workaround**</span></span>  
> <span data-ttu-id="8d010-393">Ustaw domyślny zestaw znaków dla zdalnej bazy danych MySQL na UTF-8.</span><span class="sxs-lookup"><span data-stu-id="8d010-393">Set the default character set for the remote MySQL database to UTF-8.</span></span>


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a><span data-ttu-id="8d010-394">Problem: Niektóre łącza nie są widoczne w DotNetNuke po publikowania lub pobierania witryny</span><span class="sxs-lookup"><span data-stu-id="8d010-394">Issue: Some links are not visible in DotNetNuke after publishing or downloading the site</span></span>

> <span data-ttu-id="8d010-395">Jeśli publikowanie lub pobrać witryny DotNetNuke, konieczne może być wyczyścić pamięć podręczną, aby uzyskać nowe łącza pojawiać się w witrynie.</span><span class="sxs-lookup"><span data-stu-id="8d010-395">If you publish or download a DotNetNuke site, you might need to clear the cache to get the new links to appear on the site.</span></span>
> 
> <span data-ttu-id="8d010-396">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-396">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="8d010-397">Zaloguj się jako "Host".</span><span class="sxs-lookup"><span data-stu-id="8d010-397">Log in as "Host".</span></span>
> 2. <span data-ttu-id="8d010-398">Przejdź do menu hosta i wybierz **ustawienia hosta**.</span><span class="sxs-lookup"><span data-stu-id="8d010-398">Go to the host menu and select **Host Settings**.</span></span>
> 3. <span data-ttu-id="8d010-399">Przewiń w dół i w obszarze **Zaawansowane ustawienia**, rozwiń węzeł **ustawienia wydajności**.</span><span class="sxs-lookup"><span data-stu-id="8d010-399">Scroll down and under **Advanced Settings**, expand **Performance Settings**.</span></span>
> 4. <span data-ttu-id="8d010-400">Kliknij przycisk **Wyczyść pamięć podręczną** łącze do strony.</span><span class="sxs-lookup"><span data-stu-id="8d010-400">Click the **Clear Cache** link for pages.</span></span>
> 5. <span data-ttu-id="8d010-401">Przejdź do dolnej części strony, a następnie ponownie uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="8d010-401">Go to the bottom of the page and restart the application.</span></span>


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a><span data-ttu-id="8d010-402">Problem: Niektóre łącza w program AtomSite są przerwane po pobraniu opublikowanej witryny</span><span class="sxs-lookup"><span data-stu-id="8d010-402">Issue: Some links in AtomSite are broken after you download a published site</span></span>

> <span data-ttu-id="8d010-403">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-403">**Workaround**</span></span>  
> <span data-ttu-id="8d010-404">W *service.config* pliku *users.config* plik i wszystkie *.xml* plików, Zastąp ciąg adresu URL (na przykład `http://myhost.com/atomsite`) z lokalna (na przykład `http://localhost:1239`).</span><span class="sxs-lookup"><span data-stu-id="8d010-404">In the *service.config* file, *users.config* file, and all *.xml* files, replace the URL string (for example, `http://myhost.com/atomsite`) with the local one (for example, `http://localhost:1239`).</span></span>


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a><span data-ttu-id="8d010-405">Problem: Aplikacji opartych na MySQL, takich jak WordPress nie można opublikować i zgłoś błąd bazy danych</span><span class="sxs-lookup"><span data-stu-id="8d010-405">Issue: MySQL-based applications like WordPress fail to publish and report a database error</span></span>

> <span data-ttu-id="8d010-406">Domyślnie program WebMatrix instaluje MySQL z zestawem znaków UTF-8.</span><span class="sxs-lookup"><span data-stu-id="8d010-406">By default, WebMatrix installs MySQL with the UTF-8 character set.</span></span> <span data-ttu-id="8d010-407">Jeżeli instalujesz MySQL samodzielnie, i nie jest zestaw znaków UTF-8 (na przykład jest Latin1), proces publikowania dla baz danych może zakończyć się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="8d010-407">If you install MySQL on your own, and the character set is not UTF-8 (for example, it is Latin1), the publish process for databases might fail.</span></span>
> 
> <span data-ttu-id="8d010-408">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-408">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="8d010-409">Zmień zestaw MySQL UTF-8 znaków.</span><span class="sxs-lookup"><span data-stu-id="8d010-409">Change the character set for MySQL to UTF-8.</span></span> <span data-ttu-id="8d010-410">(Aby uzyskać więcej informacji, zobacz [zestaw znaków serwera i sortowanie](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) w witrynie sieci Web MySQL.)</span><span class="sxs-lookup"><span data-stu-id="8d010-410">(For details, see [Server Character Set and Collation](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) on the MySQL website.)</span></span>
> 2. <span data-ttu-id="8d010-411">Zainstaluj ponownie aplikację.</span><span class="sxs-lookup"><span data-stu-id="8d010-411">Reinstall the application.</span></span>
> 3. <span data-ttu-id="8d010-412">Ponownie opublikować aplikację.</span><span class="sxs-lookup"><span data-stu-id="8d010-412">Republish the application.</span></span>


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a><span data-ttu-id="8d010-413">Problem: "Pobierania opublikowane lokacji" kończy się niepowodzeniem dla aplikacji, które skonfigurowano w przeglądarce</span><span class="sxs-lookup"><span data-stu-id="8d010-413">Issue: "Download published site" fails for applications that have browser-based setup</span></span>

> <span data-ttu-id="8d010-414">Niektóre aplikacje (na przykład Kentico CMS) trzeba uruchomić je w przeglądarce w celu przeprowadzenia instalacji po instalacji, takich jak tworzenie bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8d010-414">Some applications (for example, Kentico CMS) require you to launch them in the browser in order to perform post-installation setup such as creating a database.</span></span> <span data-ttu-id="8d010-415">Jeśli spróbujesz opublikować aplikację, jak to bez zakończeniu instalacji opartej na przeglądarce, próby pobrania tej samej lokacji z serwerem zdalnym zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="8d010-415">If you publish an application like this without completing the browser-based setup, attempting to download the same site from a remote server will fail.</span></span>
> 
> <span data-ttu-id="8d010-416">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-416">**Workaround**</span></span>  
> <span data-ttu-id="8d010-417">Zakończenie instalacji opartej na przeglądarce przed opublikowaniem tej witryny.</span><span class="sxs-lookup"><span data-stu-id="8d010-417">Finish browser-based setup before publishing the site.</span></span>


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a><span data-ttu-id="8d010-418">Problem: "Pobierania opublikowane lokacji" kończy się niepowodzeniem z powodu błędu bazy danych DotNetNuke i Kooboo CMS</span><span class="sxs-lookup"><span data-stu-id="8d010-418">Issue: "Download published site" fails with a database error for DotNetNuke and Kooboo CMS</span></span>

> <span data-ttu-id="8d010-419">Jeśli próba pobrania aplikacji z serwera i poświadczenia administratora w ciągu połączenia bazy danych w **ustawień publikowania** okna dialogowego, może się pojawić następujący błąd w dzienniku publikowania:</span><span class="sxs-lookup"><span data-stu-id="8d010-419">If you try to download an application from a server and you have administrator credentials in the database connection string in the **Publish Settings** dialog, you might see the following error in the publish log:</span></span>
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> <span data-ttu-id="8d010-420">**Obejście problemu**</span><span class="sxs-lookup"><span data-stu-id="8d010-420">**Workaround**</span></span>  
> <span data-ttu-id="8d010-421">Jeśli jest to możliwe, ponownie opublikować lokacji (lub opublikować) przy użyciu poświadczeń administratora innych niż dla bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8d010-421">If practical, republish the site (or have it published) using non-administrator credentials for the database.</span></span>


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="8d010-422">Aby uzyskać więcej informacji</span><span class="sxs-lookup"><span data-stu-id="8d010-422">For More Information</span></span>

<span data-ttu-id="8d010-423">Aby uzyskać więcej informacji o programie WebMatrix w wersji 1.0 zobacz następujące witryny sieci Web:</span><span class="sxs-lookup"><span data-stu-id="8d010-423">For more information about WebMatrix 1.0, see the following websites:</span></span>

- [<span data-ttu-id="8d010-424">IIS.net</span><span class="sxs-lookup"><span data-stu-id="8d010-424">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="8d010-425">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8d010-425">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="8d010-426">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="8d010-426">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

<span data-ttu-id="8d010-427">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="8d010-427">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="8d010-428">Wszelkie prawa zastrzeżone.</span><span class="sxs-lookup"><span data-stu-id="8d010-428">All Rights Reserved.</span></span> <span data-ttu-id="8d010-429">[Warunki użytkowania](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d010-429">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
