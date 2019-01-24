---
uid: whitepapers/aspnet-web-deployment-content-map
title: Wdrażanie sieci Web platformy ASP.NET — zalecane zasoby | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten temat zawiera łącza do dokumentacji zasoby o tym, jak wdrożyć (opublikować) ASP.NET sieci web aplikacji usług IIS przy użyciu programu Visual Studio 2010, Visual Web De...
ms.author: riande
ms.date: 03/14/2014
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 6df0c9d2f38ad1d39abd62787c600ef80da8e8e0
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836224"
---
<a name="aspnet-web-deployment---recommended-resources"></a>Wdrożenie internetowe na platformie ASP.NET — zalecane zasoby
====================
> Ten temat zawiera łącza do dokumentacji zasoby o tym, jak wdrożyć (opublikować) ASP.NET sieci web aplikacji usług IIS przy użyciu programu Visual Studio 2010, Visual Web Developer 2010 i nowszych wersjach.
> 
> Jeśli znasz bardzo blogu, [stackoverflow](http://stackoverflow.com) wątku lub dowolny link, który powinien być przydatny, [Wyślij do nas wiadomość e-mail](mailto:aspnetue@microsoft.com?subject=Deployment%20Content%20Map) z linkiem.
> 
> > [!NOTE] 
> > 
> > Wiele z tych zasobów opisano funkcje wdrażania, które są dostępne tylko w przypadku instalowania najnowszych wersji [Visual Studio Web publikowanie aktualizacji](https://go.microsoft.com/fwlink/?LinkID=208120). Niektóre funkcje są dostępne tylko w programie Visual Studio 2012 lub Visual Studio 2013.


Ten temat zawiera następujące sekcje:

- [Omówienie opcji wdrażania dla projektów sieci web](#understanding)
- [Wyszukiwanie dostawcy usług dla aplikacji ASP.NET hosta](#findinghosting)
- [Wdrażanie aplikacji sieci web w programie Visual Studio](#fromvs)
- [Wdrażanie aplikacji sieci web przez tworzenie i instalowanie pakietu wdrożeniowego sieci web](#package)
- [Wdrażanie aplikacji sieci web, za pomocą procesu ciągłej integracji (CI)](#ci)
- [Aby zmienić ustawienia w pliku app.config lub pliku Web.config docelowego podczas wdrażania za pomocą transformacje pliku Web.config](#transforms)
- [Za pomocą narzędzia Web Deploy parametrów, aby zmienić ustawienia w aplikacji sieci web docelowego podczas wdrażania](#webdeployparms)
- [Upewnij się, że aplikacja jest w trybie offline podczas wdrażania](#appoffline)
- [Wdrażanie bazy danych lub zmiany do bazy danych jako część wdrożenia aplikacji sieci web](#databasewithweb)
- [Wdrażanie bazy danych niezależnie od wdrażania aplikacji sieci web](#databaseseparate)
- [Wdrażanie aplikacji sieci web, który używa aplikacji ASP.NET usługach, takich jak członkostwo i profilowania](#aspnetmembership)
- [Prekompilowanie wdrożenia](#precompiling)
- [Wdrażanie aplikacji sieci web w sieci intranet](#intranet)
- [Automatyzację typowych zadań wdrażania, które nie są zautomatyzowane gotowe](#automating)
- [Konfigurowanie serwerów sieci web, dzięki czemu deweloperzy mogą wdrażanie aplikacji sieci web do nich za pomocą narzędzia Web Deploy](#configuringservers)
- [Konfigurowanie serwerów dla dostawcy usług hostingowych](#hostingprovider)
- [Rozwiązywanie problemów z wdrażaniem](#troubleshooting)
- [Uzyskiwanie pomocy dotyczącej zapytania określonego wdrożenia](#gettinghelp)
- [Dodatkowe zasoby](#additional)


<a id="understanding"></a>


## <a name="understanding-deployment-options-for-web-projects"></a>Omówienie opcji wdrażania dla projektów sieci web

- [Omówienie wdrażania w sieci Web dla programu Visual Studio i platformy ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx) (MSDN).
- [Jak wdrożyć witrynę sieci Web platformy Azure Windows](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Opisano opcje i linki do zasobów dla wdrażania projektów sieci web do Windows Azure Web Sites, w tym [ciągłe dostarczanie](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (automatyczne z [kontroli źródła](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) oraz przy użyciu programu Visual Studio.
- [Ulepszenia publikowania w sieci Web programu Visual Studio 2012](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (klip wideo Scott hanselman).
- [Omówienie wpis dla wdrażania w Internecie w VS 2010](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (blog Vishal Joshi). Starsze wpis w blogu, ale niektóre zasoby programu Visual Studio 2010 łączy informacje, które nadal są prawidłowe dla programu Visual Studio 2012.


<a id="findinghosting"></a>


## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Wyszukiwanie dostawcy usług dla aplikacji ASP.NET hosta

- [ASP.NET Hosting](https://asp.net/hosting)


<a id="fromvs"></a>


## <a name="deploying-a-web-application-from-visual-studio"></a>Wdrażanie aplikacji sieci web w programie Visual Studio

- [Jak wdrożyć witrynę sieci Web platformy Azure Windows](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Opisano opcje i zamieszczono linki do zasobów do wdrażania projektów sieci web do Windows Azure Web Sites. Zawiera sekcję o wdrażaniu w programie Visual Studio.
- [Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12-trzyczęściowej serii samouczków, przedstawia sposób wdrażania aplikacji sieci web przy użyciu bazy danych programu SQL Server. Dla bazy danych wdrożenie używa dostawcy dbDacFx i migracje Code First Framework jednostki. Zawiera również informacje o [przekształcenia pliku Web.config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md), [wdrażania poszczególnych plików](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), [wiersza polecenia deployment](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md), i [jak Dostosowywanie programu Visual Studio w sieci web potoku publikowania przez edycję plików .pubxml](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). Dotyczy wszystkich projektów sieci web platformy ASP.NET, w tym Web Forms, MVC i interfejs API sieci Web).
- [Instrukcje: Wdrażanie publikowania projektu sieci Web za pomocą jednego kliknięcia w programie Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (informacje referencyjne dotyczące programu Visual Studio Web Publish kreatora.)
- [Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Jest to starszą wersję **wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio** wymienione w górnej części tej sekcji. Przede wszystkim ułatwia teraz informacje dotyczące wdrażania baz danych programu SQL Server Compact i migrację z programu SQL Server Compact do pełnej wersji programu SQL Server.
- [Przy użyciu tabel magazynu .NET obejmujące wiele warstw aplikacji, kolejek i obiektów blob](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (witryny Microsoft Azure). 5-częściową serię samouczków pokazano, jak utworzyć projekt programu MVC oraz wdrożyć ją z usługą Windows Azure w chmurze.


<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Wdrażanie aplikacji sieci web przez tworzenie i instalowanie pakietu wdrożeniowego sieci web

- [Instrukcje: Utwórz pakiet wdrażania sieci Web w programie Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx) (MSDN).
- [Instrukcje: Zainstaluj pakiet wdrożeniowy, przy użyciu pliku pliku deploy.cmd utworzone przez program Visual Studio](https://msdn.microsoft.com/library/ff356104.aspx) (MSDN).
- [Przy użyciu pakietu Narzędzia Web Deploy do wdrażania usług IIS w oknie programu dev i do hosta innej](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) (blog Sayed Hashimi). Jak za pomocą Menedżera usług IIS zainstalowanie pakietu wdrożeniowego w usługach IIS na komputerze lokalnym oraz na hosting firmy, która obsługuje Menedżera usług IIS do administracji zdalnej.
- [Tworzenie sieci Web wdrażanie pakietu z programu Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (IIS.NET w witrynie). Zawiera instrukcje dotyczące tworzenia pakietów z wiersza polecenia i instalacji.
- [Publikowanie dowolnym pakietu](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (blog Sayed Hashimi). Wprowadza pakietu NuGet, który automatyzuje proces przekształcenia pliku Web.config dla wielu środowisk docelowego, dzięki czemu można wdrożyć jeden pakiet na wielu serwerach. Zobacz też [wideo PackageWeb](https://www.youtube.com/watch?v=-LvUJFI8CzM) przez Sayed Hashimi.

Zobacz też poniższej sekcji.


<a id="ci"></a>


## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Wdrażanie aplikacji sieci web, za pomocą procesu ciągłej integracji (CI)

- [Ciągła integracja i ciągłe dostarczanie (tworzenie rzeczywistych aplikacji w chmurze przy użyciu platformy Windows Azure).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Rozdział E-book, który zawiera wprowadzenie do ciągłej integracji i ciągłego dostarczania.
- [Jak wdrożyć witrynę sieci Web platformy Azure Windows](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Opisano opcje i linki do zasobów dla wdrażania projektów sieci web do Windows Azure Web Sites. Zawiera sekcję poświęconą automatyzowaniu wdrożeń z kontroli źródła.
- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). 40 części serii samouczków pokazano, jak zautomatyzować wdrażanie w procesie ciągłej integracji przy użyciu programu Visual Studio 2010 i Team Foundation Server 2010.
- [W aparacie kompilacji firmy Microsoft: Przy użyciu programu MSBuild i Team Foundation Build, Sayed Hashimi i William Bartholomew](http://msbuildbook.com). To książki, a nie zasobu sieci web, ale jest podstawy do nauki, jak skonfigurować program MSBuild w scenariuszach ciągłej integracji.
- [Pakiet rozszerzenia programu MSBuild](https://github.com/mikefourie/MSBuildExtensionPack). Zawiera zadania wdrażania.
- [Przewodnik dostosowywania kompilacji Foundation Team](https://aka.ms/vsarsolutions). Dokumentacja wg ALM Rangers na temat konfigurowania serwera Team Foundation Server obejmuje wdrażanie w Internecie i obejmuje samouczkami i klipami wideo.
- [Przekształcenia SlowCheetah XML z serwera CI](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (blog Sayed Hashimi). Opis sposobu użycia SlowCheetah dodatku programu Visual Studio do przekształcania pliku app.config i inne pliki XML.

Zobacz też [upewnić się, że aplikacja jest w trybie offline podczas wdrażania](aspnet-web-deployment-content-map.md#appoffline) nowszych na tej stronie.


<a id="transforms"></a>


## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Aby zmienić ustawienia w pliku app.config lub pliku Web.config docelowego podczas wdrażania za pomocą transformacje pliku Web.config

- [Przekształcenia pliku Web.config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md).
- [Składnia przekształcania Web.config wdrażanie projektu sieci Web przy użyciu programu Visual Studio](https://msdn.microsoft.com/library/dd465326.aspx) (MSDN).
- [Web Tools 2012.2 — przekształcenia pliku web.config](https://www.youtube.com/watch?v=HdPK8mxpKEI) (wideo w serwisie YouTube przez Sayed Hashimi). Pokazuje, jak skonfigurować i Wyświetl podgląd przekształcenia pliku Web.config.
- [Jak wyłączyć transformacji pliku Web.config?](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [Kiedy używać narzędzia Web Deploy parametrów zamiast transformacje pliku Web.config?](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [XDT (Przekształcanie dokumentów XML), wydanej w dniu codeplex.com](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (blog programowanie aplikacji sieci Web platformy .NET i narzędzi). Ogłasza dostępności kodu źródłowego dla silnika transformacji pliku Web.config i zawiera listę niektórych narzędzi, które go używają.
- [Windows Azure Web Sites: Jak aplikacja ciągi and Connection Strings Work](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog Microsoft Azure). Przekształca alternatywę do pliku Web.config, jeśli środowisko docelowym jest Windows Azure Web Sites, a chcesz przekształcić `appSettings` lub `connectionStrings`.


<a id="webdeployparms"></a>


## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>Za pomocą narzędzia Web Deploy parametrów, aby zmienić ustawienia w aplikacji sieci web docelowego podczas wdrażania

- [Instrukcje: Użyj narzędzia Web Deploy parametrów w pakiecie wdrożeniowym sieci Web](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN).
- [MSDeploy: Jak zaktualizować ustawienia aplikacji na publikowanie oparte na profilu publikowania](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (blog Sayed Hashimi). Pokazuje, jak zintegrować narzędzia Web deploy parametrów do programu Visual Studio profilów publikowania.
- [Sieci Web wdrażanie parametryzacji](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (IIS.NET w witrynie).
- [Sieci Web wdrażanie parametryzacji w działaniu](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (blog Vishal Joshi).
- [Vs parametryzacji wdrażania sieci Web. Przekształcenia pliku Web.config](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (blog Vishal Joshi).
- [Windows Azure Web Sites: Jak aplikacja ciągi and Connection Strings Work](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog Microsoft Azure). Alternatywa dla sieci Web wdrożenia parametrów, jeśli środowisko docelowym jest Windows Azure Web Sites, a użytkownik chce zdefiniować parametry `appSettings` lub `connectionStrings`.


<a id="appoffline"></a>


## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Upewnij się, że aplikacja jest w trybie offline podczas wdrażania

- [Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji kodu](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Zobacz sekcję **nastavit aplikaci offline podczas wdrażania.**
- [Przełączania aplikacji w tryb Offline przed opublikowaniem](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (IIS.net w witrynie). Wyjaśnia funkcji wbudowanych w Deploy 3.0 w sieci Web, która automatyzuje operacje przenoszenia aplikacji\_offline.htm pliku. Ta funkcja nie działa z aplikacji niestandardowej\_offline.htm plików.
- [Sposób wykonania aplikacji sieci web w trybie offline podczas publikowania](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) (blog Sayed Hashimi). Jak zautomatyzować proces przy użyciu niestandardowych aplikacji\_offline.htm pliku.
- [Sieci Web publikowanie aktualizacji dla aplikacji w trybie offline i usechecksum](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (blog tworzenie aplikacji sieci Web firmy Microsoft). Inną opcją do automatyzowania korzystanie z aplikacji\_offline.htm pliku.
- [Sieci Web wdrażanie RTW 3.5](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (IIS.net w witrynie). Nowa funkcja w sieci Web wdrażanie 3.5 w aplikacji niestandardowej\_offline.htm plików.


<a id="databasewithweb"></a>


## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>Wdrażanie bazy danych lub zmiany do bazy danych jako część wdrożenia aplikacji sieci web

- [Konfigurowanie wdrażania bazy danych w programie Visual Studio](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment) (MSDN). Omówienie opcji wdrażania bazy danych w projekcie sieci web.
- [Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12-trzyczęściowej serii samouczków, przedstawia wdrożenie bazy danych przy użyciu dostawcy dbDacFx i migracje Code First Framework jednostki.
- [Instrukcje: Wdrażanie sieci Web projektu za pomocą jednego kliknięcia publikowania w programie Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (MSDN).
- [Wdrażanie bezpiecznej aplikacji ASP.NET MVC 5 z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL w witrynie sieci Web platformy Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Długie samouczek, który zostanie skompilowana i wdrożona aplikacja, która korzysta z jednego serwera SQL bazy danych, zarówno dla danych członkostwa i aplikacji.
- [Wdrażanie aplikacji sieci Web ASP.NET za pomocą programu SQL Server Compact przy użyciu programu Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). 12-trzyczęściowej serii samouczków, przedstawia sposób wdrażania baz danych programu SQL Server Compact i jak przeprowadzić migrację z programu SQL Server Compact do pełnej wersji programu SQL Server.

Zobacz również wdrażanie aplikacji sieci web, tworząc i instalowanie pakietu wdrożeniowego sieci web i wdrażanie aplikacji sieci web, za pomocą procesu ciągłej integracji (CI) we wcześniejszej części tej strony.


<a id="databaseseparate"></a>


## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Wdrażanie bazy danych niezależnie od wdrażania aplikacji sieci web

- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN).
- [W tym danych w projekcie bazy danych programu SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (blog zespołu programu SQL Server Data Tools). Jak wdrożyć zarówno schematu, jak i dane, podczas wdrażania bazy danych.
- [Jak wdrożyć bazę danych na platformie Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (witryny Microsoft Azure)
- [Migrowanie baz danych do usługi Windows Azure SQL Database (dawniej SQL Azure)](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN).
- [Migrowanie bazy danych SQL Azure za pomocą narzędzi SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (blog zespołu programu SQL Server Data Tools).
- [Migrowanie aplikacji opartych na danych na platformie Windows Azure](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN).
- [Migrowanie bazy danych SQL Server do usługi Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).


<a id="aspnetmembership"></a>


## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Wdrażanie aplikacji sieci web, który używa aplikacji ASP.NET usługach, takich jak członkostwo i profilowania

- [Wdrażanie bezpiecznej aplikacji ASP.NET MVC 5 z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL w witrynie sieci Web platformy Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Długie samouczek, który zostanie skompilowana i wdrożona aplikacja, która korzysta z jednego serwera SQL bazy danych, zarówno dla danych członkostwa i aplikacji.
- [ASP.NET Identity](https://asp.net/identity/). Zasoby dla produktu ASP.NET Identity.
- [Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). serii 12 części przedstawiono sposób wdrażania bazy danych członkostwa ASP.NET.
- [Konfigurowanie witryny sieci Web, która korzysta z usług aplikacji](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). Dla witryny sieci web projektów ale jest również istotne dla projektów aplikacji sieci web.
- [Użytkownicy i role w produkcyjnej witrynie internetowej](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). Dla witryny sieci web projektów ale jest również istotne dla projektów aplikacji sieci web.


<a id="precompiling"></a>


## <a name="precompiling-for-deployment"></a>Prekompilowanie wdrożenia

- [Omówienie wstępnej kompilacji projektu aplikacji sieci Web platformy ASP.NET](https://msdn.microsoft.com/library/aa983464.aspx) (MSDN).
- [Pakowanie/publikowanie kartę sieci Web, właściwości projektu](https://msdn.microsoft.com/library/dd410108.aspx) (MSDN).
- [Zaawansowane Prekompilowanie okno dialogowe Ustawienia](https://msdn.microsoft.com/library/hh475319.aspx) (MSDN).


<a id="intranet"></a>


## <a name="deploying-an-intranet-web-application"></a>Wdrażanie aplikacji sieci web w sieci intranet

- [Opcja uwierzytelniania organizacyjnego lokalnego (AD FS) za pomocą programu ASP.NET w programie Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (Blog autorstwa Vittorio Bertocci.).
- [Sposób tworzenia witryny intranetowej przy użyciu platformy ASP.NET MVC](https://msdn.microsoft.com/library/gg703322(VS.98).aspx) (MSDN). Starsze writen wskazówki dla programu Visual Studio 2010, nie odzwierciedla istotne zmiany w szablonach projektu sieci intranet, wprowadzone w programie Visual Studio 2013.


<a id="automating"></a>


## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Automatyzację typowych zadań wdrażania, które nie są zautomatyzowane gotowe

- [Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie dodatkowych plików](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Ustawianie uprawnień do folderów na publikowanie w sieci Web](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (blog Sayed Hashimi).
- [Jak rozszerzyć pliku obiektów docelowych, aby uwzględnić ustawienia rejestru dla pakietu projektu sieci web](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (blog narzędzi do programowania).
- [Rozszerzanie transformacje XML (Web.config)](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (blog Sayed Hashimi). Przedstawia sposób utworzenia niestandardowych przekształceń XDT.
- [Web Take dostawcy niestandardowego narzędzia wdrażania (MSDeploy) 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (blog Sayed Hashimi). Pokazuje, jak utworzyć niestandardowego dostawcę narzędzia Web Deploy.
- [Pakowanie i wdrażanie składników COM](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (blog narzędzi do programowania).
- [Jak zestawy .NET pakietów](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) (blog narzędzi do programowania). Jak wdrożyć zestawy w pamięci GAC.
- [Wszystko — inicjowanie Your Windows maszyny Wirtualnej platformy Azure dla serwera sieci Web usług IIS, narzędzie Web Deploy i inne rzeczy skryptu](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (blog Tugberk Ugurlu).


<a id="configuringservers"></a>


## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Konfigurowanie serwerów sieci web, dzięki czemu deweloperzy mogą wdrażanie aplikacji sieci web do nich za pomocą narzędzia Web Deploy

- [Instalowanie i konfigurowanie narzędzie Web Deploy dla administratora i użytkowników niebędących administratorami wdrożeń](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (IIS.net w witrynie).


<a id="hostingprovider"></a>


## <a name="configuring-servers-for-a-hosting-provider"></a>Konfigurowanie serwerów dla dostawcy usług hostingowych

- [Przewodniku dotyczącym wdrażania hostingu platformy Microsoft ASP.NET 4](https://go.microsoft.com/fwlink/?LinkId=191365) (Centrum pobierania Microsoft).
- [Generowanie pliku XML profilu](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (IIS.net w witrynie).


<a id="troubleshooting"></a>


## <a name="troubleshooting-deployment-problems"></a>Rozwiązywanie problemów z wdrażaniem

- [Rozwiązywanie problemów z systemu Windows Azure Web Sites w programie Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (witryny Microsoft Azure).
- [Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Rozwiązywanie problemów z](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md).
- [Wdrażanie rozwiązywania typowych problemów z siecią Web](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Kody błędów wdrażania w sieci Web](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (IIS.net w witrynie).
- [Narzędzie Web Deployment — często zadawane pytania dla programu Visual Studio i platformy ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN).
- [Podstawowe różnice między usługami IIS a programem ASP.NET Development Server](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Typowe różnice konfiguracji między środowiskami deweloperskim i produkcyjnym](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md).
- [Hostowanie aplikacji ASP.NET w trybie średniego zaufania](http://www.4guysfromrolla.com/articles/100307-1.aspx) (4 Guys z witryny Rolla).


<a id="gettinghelp"></a>


## <a name="getting-help-with-a-specific-deployment-question"></a>Uzyskiwanie pomocy dotyczącej zapytania określonego wdrożenia

- [Konfiguracja platformy ASP.NET i wdrażanie forum](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).


<a id="additional"></a>


## <a name="additional-resources"></a>Dodatkowe zasoby

Ta sekcja zawiera linki do dodatkowych zasobów, które są przydatne w przypadku dowiedzieć się więcej na temat używania narzędzia do wdrażania programu Visual Studio i usług IIS.

Następujących blogów często zawierają informacje dotyczące wdrażania sieci web programu Visual Studio:

- [Narzędzia Web Tools rozwoju na blogu Microsoft](https://blogs.msdn.com/b/webdevtools/).
- [Blog Sayed Hashimi](http://www.sedodream.com/).

W następujących zasobach udostępniono dokumentacji dotyczącej narzędzia Web Deploy, framework usług IIS, która korzysta z programu Visual Studio do wykonania zadania wdrażania projektu aplikacji sieci web. Możesz zadawać pytania dotyczące narzędzia Web Deploy w [forum narzędzie Web Deployment](https://go.microsoft.com/fwlink/?LinkId=149411) w witrynie sieci web IIS.net.

- [Wprowadzenie do sieci Web wdrażanie](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Instalowanie i konfigurowanie sieci Web wdrażanie](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [Skrypty programu PowerShell dla sieci Web automatyzacji wdrażania instalacji](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Narzędzia wdrażania Web](https://go.microsoft.com/fwlink/?LinkId=151481). Tabeli najwyższego poziomu węzła zawartość dla narzędzia Web Deploy dokumentacji w witrynie TechNet. Zawiera przydatne informacje, ale większość TechNet stron nie zostały zaktualizowane przez lata.
- [Namespace składnika Microsoft.Web.Deployment](https://go.microsoft.com/fwlink/?LinkId=148630). Dokumentacja interfejsu API nie został zaktualizowany od czasu wersji 1.0.
- [Blog zespołu wdrażania sieci Web firmy Microsoft](https://blogs.iis.net/msdeploy/default.aspx).
- [Publikowanie kartę w witrynie sieci web IIS.net](https://www.iis.net/learn/publish).
