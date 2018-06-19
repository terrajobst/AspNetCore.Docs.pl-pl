---
uid: whitepapers/aspnet-web-deployment-content-map
title: Wdrażanie sieci Web platformy ASP.NET — zalecane zasoby | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten temat zawiera linki do dokumentacji (publikowanie) ASP.NET web Zasoby dotyczące wdrażania aplikacji do usług IIS przy użyciu programu Visual Studio 2010, Visual De sieci Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2014
ms.topic: article
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 78ff183394b5ff92f789b50551d01d28f9bff93b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28048171"
---
<a name="aspnet-web-deployment---recommended-resources"></a>Wdrażanie sieci Web platformy ASP.NET — zalecane zasobów
====================
> Ten temat zawiera linki do dokumentacji (publikowanie) ASP.NET web Zasoby dotyczące wdrażania aplikacji do usług IIS przy użyciu programu Visual Studio 2010, Visual Web Developer 2010 i nowszych wersjach.
> 
> Jeśli znasz dużą blogu, [stackoverflow](http://stackoverflow.com) wątku lub innych łącza, które będą przydatne [Wyślij do nas wiadomość e-mail](mailto:aspnetue@microsoft.com?subject=Deployment Content Map) z łączem.
> 
> > [!NOTE] 
> > 
> > Wiele z tych zasobów opisano funkcje wdrażania, które są dostępne tylko w przypadku instalowania aktualną wersją z [aktualizacja programu Visual Studio Web publikowanie](https://go.microsoft.com/fwlink/?LinkID=208120). Niektóre funkcje są dostępne tylko w programie Visual Studio 2012 lub Visual Studio 2013.


Ten temat zawiera następujące sekcje:

- [Opis opcji wdrażania dla projektów sieci web](#understanding)
- [Znajdowanie hostingu dostawców dla aplikacji ASP.NET](#findinghosting)
- [Wdrażanie aplikacji sieci web w programie Visual Studio](#fromvs)
- [Wdrażanie aplikacji sieci web, tworząc i instalowanie pakietu wdrożeniowego sieci web](#package)
- [Wdrażanie aplikacji sieci web za pomocą procesu ciągłej integracji (CI)](#ci)
- [Używanie przekształceń pliku Web.config na zmianę ustawienia w pliku Web.config docelowego lub pliku app.config podczas wdrażania](#transforms)
- [Aby zmienić ustawienia aplikacji sieci web docelowym podczas wdrażania przy użyciu parametrów narzędzia Web Deploy](#webdeployparms)
- [Sprawdzanie, czy aplikacja jest offline podczas wdrażania](#appoffline)
- [Wdrażanie bazy danych lub zmiany do bazy danych jako część wdrożenia aplikacji sieci web](#databasewithweb)
- [Wdrożenie bazy danych niezależnie od wdrażania aplikacji sieci web](#databaseseparate)
- [Wdrażanie aplikacji sieci web, która używa aplikacji ASP.NET usług takich jak członkostwo i profilowania](#aspnetmembership)
- [Prekompilowanie dla wdrożenia](#precompiling)
- [Wdrażanie aplikacji sieci web w sieci intranet](#intranet)
- [Automatyzacji typowych zadań wdrażania, które nie są automatyzowane fabrycznej](#automating)
- [Konfigurowanie serwerów sieci web, dzięki temu deweloperzy wdrażać je za pomocą narzędzia Web Deploy aplikacji sieci web](#configuringservers)
- [Konfigurowanie serwerów dla dostawcy hostingu](#hostingprovider)
- [Rozwiązywanie problemów z wdrażaniem](#troubleshooting)
- [Uzyskiwanie pomocy dotyczącej zapytania określonego wdrożenia](#gettinghelp)
- [Dodatkowe zasoby](#additional)


<a id="understanding"></a>


## <a name="understanding-deployment-options-for-web-projects"></a>Opis opcji wdrażania dla projektów sieci web

- [Omówienie wdrażania w sieci Web dla platformy ASP.NET i Visual Studio](https://msdn.microsoft.com/library/dd394698.aspx) (MSDN).
- [Jak wdrożyć witrynę sieci Web platformy Azure z systemem Windows](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Opisano opcje i linki do zasobów podczas wdrażania projektów sieci web do systemu Windows Azure Web Sites, łącznie z [ciągłego dostarczania](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (automatycznych z [kontroli źródła](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) oraz przy użyciu programu Visual Studio.
- [Ulepszenia publikowania w sieci Web programu Visual Studio 2012](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (klip wideo, w którym Scott Hanselman).
- [Omówienie Post dotyczące wdrażania sieci Web w programie VS 2010](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (blog Vishal Joshi). Starsze wpis w blogu, ale niektóre z zasobów programu Visual Studio 2010 zawiera linki do informacji, które nadal są prawidłowe dla programu Visual Studio 2012.


<a id="findinghosting"></a>


## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Znajdowanie hostingu dostawców dla aplikacji ASP.NET

- [ASP.NET Hosting](https://asp.net/hosting)


<a id="fromvs"></a>


## <a name="deploying-a-web-application-from-visual-studio"></a>Wdrażanie aplikacji sieci web w programie Visual Studio

- [Jak wdrożyć witrynę sieci Web platformy Azure z systemem Windows](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Zawiera opis opcji oraz linki do zasobów wdrażania projektów sieci web do systemu Windows Azure Web Sites. Zawiera sekcję dotyczącą wdrażania w programie Visual Studio.
- [Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 części samouczka serii, przedstawia sposób wdrażania aplikacji sieci web z baz danych programu SQL Server. Dla bazy danych wdrażania używa dostawcy dbDacFx narzędzia i migracje Code First Framework jednostki. Zawiera także informacje o [przekształcenia pliku Web.config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md), [wdrażania poszczególnych plików](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), [wiersza polecenia deployment](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md), i [porady Dostosowywanie w sieci web programu Visual Studio potoku publikowania przez edycję plików .pubxml](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). Dotyczy wszystkich projektów sieci web ASP.NET, w tym interfejsów API sieci Web, MVC i formularzy sieci Web).
- [Porady: Wdrażanie publikowania projektu sieci Web za pomocą jednego kliknięcia w programie Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (informacje o Kreatorze Visual Studio publikowania w sieci Web.)
- [Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio programu SQL Server Compact](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). To jest starsza wersja **wdrożenia sieci Web ASP.NET przy użyciu programu Visual Studio** wymienionych w górnej części tej sekcji. Teraz głównie przydatne informacje dotyczące sposobu wdrażania baz danych programu SQL Server Compact i przeprowadzanie migracji z programu SQL Server Compact do pełnej wersji programu SQL Server.
- [Przy użyciu tabel magazynu .NET wielowarstwowych aplikacji, kolejek i obiektów blob](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (witryny Microsoft Azure). Seria samouczek, część 5, pokazuje, jak utworzyć projekt MVC i wdrożyć ją do usługi Windows Azure w chmurze.


<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Wdrażanie aplikacji sieci web, tworząc i instalowanie pakietu wdrożeniowego sieci web

- [Porady: Tworzenie pakietu wdrożeniowego sieci Web w programie Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx) (MSDN).
- [Porady: Instalowanie utworzony pakiet wdrażania przy użyciu pliku plikiem deploy.cmd przez program Visual Studio](https://msdn.microsoft.com/library/ff356104.aspx) (MSDN).
- [Przy użyciu pakietu Narzędzia Web Deploy do wdrażania usług IIS w oknie deweloperów i do hosta innej](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) (blog Sayed Hashimi). Jak używać Menedżera usług IIS do zainstalowania pakietu wdrożeniowego w usługach IIS na komputerze lokalnym i w hosting firmy, który obsługuje Menedżera usług IIS do administracji zdalnej.
- [Tworzenie sieci Web wdrażanie pakietu z programu Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (IIS.NET witryny sieci web). Zawiera instrukcje dotyczące instalacji i tworzenia pakietu wiersza polecenia.
- [Publikowanie dowolnym pakietu](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (blog Sayed Hashimi). Wprowadza pakietu NuGet, który automatyzuje proces transformacji pliku Web.config w wielu środowiskach docelowego, dzięki czemu można wdrożyć jeden pakiet na wielu serwerach. Zobacz też [PackageWeb wideo](https://www.youtube.com/watch?v=-LvUJFI8CzM) przez Sayed Hashimi.

Zobacz też poniższej sekcji.


<a id="ci"></a>


## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Wdrażanie aplikacji sieci web za pomocą procesu ciągłej integracji (CI)

- [Ciągłej integracji i ciągłego dostarczania (Tworzenie chmury rzeczywistych aplikacji z systemu Windows Azure).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Książka elektroniczna rozdział wprowadza ciągłej integracji i ciągłego dostarczania.
- [Jak wdrożyć witrynę sieci Web platformy Azure z systemem Windows](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Opisano opcje i linki do zasobów podczas wdrażania projektów sieci web do systemu Windows Azure Web Sites. Zawiera sekcję dotyczącą automatyzacji wdrażania z kontroli źródła.
- [Wdrażanie aplikacji sieci Web w scenariuszach dla przedsiębiorstw](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). 40 części samouczka serii, pokazuje, jak można zautomatyzować wdrożenie w procesie CI przy użyciu programu Visual Studio 2010 i Team Foundation Server 2010.
- [W programie Microsoft Build Engine: przy użyciu programu MSBuild i Team Foundation Build Sayed Hashimi i łączy Bartholomew](http://msbuildbook.com). Jest to książkę nie zasobu sieci web, ale jest przewodnik niezbędne do uczenia, jak skonfigurować program MSBuild dla scenariuszy ciągłej integracji.
- [Pakiet rozszerzenia MSBuild](https://github.com/mikefourie/MSBuildExtensionPack). Zawiera zadania wdrażania.
- [Team Foundation Build dostosowania przewodnik](https://aka.ms/vsarsolutions). Dokumentacja przez ALM Rangers na temat konfigurowania serwera Team Foundation Server dotyczy wdrożenia sieci web i obejmuje samouczki i filmy wideo.
- [Przekształca SlowCheetah XML z serwera CI](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (blog Sayed Hashimi). Wyjaśniono, jak używać SlowCheetah dodatek programu Visual Studio do przekształcania pliku app.config i inne pliki XML.

Zobacz też [upewnić się, że aplikacja jest niedostępny podczas wdrażania](aspnet-web-deployment-content-map.md#appoffline) dalszej części tej strony.


<a id="transforms"></a>


## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Używanie przekształceń pliku Web.config na zmianę ustawienia w pliku Web.config docelowego lub pliku app.config podczas wdrażania

- [Przekształcenia pliku Web.config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md).
- [Składnia transformacji pliku Web.config wdrażanie projektu sieci Web przy użyciu programu Visual Studio](https://msdn.microsoft.com/library/dd465326.aspx) (MSDN).
- [Sieci Web narzędzia 2012.2 - transformacji pliku web.config](https://www.youtube.com/watch?v=HdPK8mxpKEI) (wideo w serwisie YouTube przez Sayed Hashimi). Pokazuje, jak i w wersji preview transformacji pliku Web.config.
- [Jak wyłączyć transformacji pliku Web.config?](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [Kiedy używać narzędzia Web Deploy parametrów zamiast przekształcenia pliku Web.config?](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [XDT (transformacji dokumentów XML), wydanej w dniu codeplex.com](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (blog .NET projektowanie witryn sieci Web i narzędzia). Ogłasza dostępności kodu źródłowego dla aparatu transformacji pliku Web.config i wyświetla niektóre narzędzia, które go używają.
- [Windows Azure Web Sites: Jak ciągi, aplikacji i pracy ciągów połączenia](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog Microsoft Azure). Przekształca alternatywę do pliku Web.config Jeśli środowisku docelowym jest system Windows Azure Web Sites i chcesz przekształcić `appSettings` lub `connectionStrings`.


<a id="webdeployparms"></a>


## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>Aby zmienić ustawienia aplikacji sieci web docelowym podczas wdrażania przy użyciu parametrów narzędzia Web Deploy

- [Porady: Użyj narzędzia Web Deploy parametrów w pakiecie wdrożeniowym sieci Web](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN).
- [MSDeploy: Jak zaktualizować ustawienia aplikacji na publikowanie oparte na profilu publikowania](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (blog Sayed Hashimi). Pokazuje sposób integracji narzędzia Web deploy parametrów do programu Visual Studio profilów publikowania.
- [Parametryzacja wdrażania w sieci Web](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (IIS.NET witryny sieci web).
- [Sieci Web wdrażanie parametryzacja w akcji](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (blog Vishal Joshi).
- [Vs parametryzacja wdrażania w sieci Web. Transformacji pliku Web.config](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (blog Vishal Joshi).
- [Windows Azure Web Sites: Jak ciągi, aplikacji i pracy ciągów połączenia](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog Microsoft Azure). Alternatywę dla sieci Web wdrażanie parametrów, jeśli środowisku docelowym jest system Windows Azure Web Sites i chcesz parametryzacja `appSettings` lub `connectionStrings`.


<a id="appoffline"></a>


## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Sprawdzanie, czy aplikacja jest offline podczas wdrażania

- [Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji kodu](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Zobacz sekcję **przejdź do trybu offline podczas wdrażania.**
- [Pobieranie aplikacji w trybie Offline przed opublikowaniem](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (IIS.net lokacji). Zawiera opis funkcji, wbudowane Deploy 3.0 w sieci Web, który zautomatyzuje Obsługa aplikacji\_offline.htm pliku. Ta funkcja nie działa z aplikacji niestandardowej\_offline.htm plików.
- [Sposób wykonania aplikacji sieci web w trybie offline podczas publikowania](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) (blog Sayed Hashimi). Jak zautomatyzować przy użyciu niestandardowych aplikacji\_offline.htm pliku.
- [Sieci Web publikowanie aktualizacji dla aplikacji w tryb offline i usechecksum](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (blog projektowanie witryn sieci Web firmy Microsoft). Inną opcją w przypadku automatyzacji korzystanie z aplikacji\_offline.htm pliku.
- [Sieci Web wdrażanie RTW 3.5](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (IIS.net lokacji). Nową funkcją w sieci Web wdrażanie 3.5 dla aplikacji niestandardowej\_offline.htm plików.


<a id="databasewithweb"></a>


## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>Wdrażanie bazy danych lub zmiany do bazy danych jako część wdrożenia aplikacji sieci web

- [Konfigurowanie wdrażania bazy danych w programie Visual Studio](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment) (MSDN). Omówienie opcji wdrażania bazy danych z projektu sieci web.
- [Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 części samouczka serii, pokazuje wdrażania bazy danych przy użyciu dostawcy dbDacFx narzędzia i migracje Code First Framework jednostki.
- [Porady: Wdrażanie sieci Web projektu za pomocą jednego kliknięcia publikowania w programie Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (MSDN).
- [Wdrażanie aplikacji bezpiecznego platformy ASP.NET MVC 5 z członkostwa, OAuth i bazy danych SQL do witryny sieci Web platformy Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Samouczek długi, które tworzy i wdraża aplikację, która używa pojedynczego serwera SQL bazy danych dla danych członkostwa i aplikacji.
- [Wdrażanie aplikacji sieci Web ASP.NET przy użyciu programu Visual Studio programu SQL Server Compact](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). 12 części samouczka serii, pokazano sposób migracji z programu SQL Server Compact do pełnej wersji programu SQL Server oraz sposób wdrażania baz danych programu SQL Server Compact.

Zobacz również wdrażanie aplikacji sieci web, tworząc i instalowanie pakietu wdrożeniowego sieci web i wdrażanie aplikacji sieci web za pomocą procesu ciągłej integracji (CI) we wcześniejszej części tej strony.


<a id="databaseseparate"></a>


## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Wdrożenie bazy danych niezależnie od wdrażania aplikacji sieci web

- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN).
- [Łącznie z danymi w projekcie bazy danych programu SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (blogu zespołu programu SQL Server Data Tools). Jak wdrożyć zarówno schematu, jak i dane podczas wdrażania bazy danych.
- [Jak wdrożyć bazę danych w systemie Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (witryny Microsoft Azure)
- [Migrowanie baz danych do bazy danych SQL Azure z systemem Windows (poprzednio Azure SQL)](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN).
- [Migrowanie bazy danych SQL Azure za pomocą narzędzi SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (blogu zespołu programu SQL Server Data Tools).
- [Migrowanie skoncentrowane na dane aplikacji w systemie Windows Azure](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN).
- [Migrowanie bazy danych programu SQL Server do systemu Windows Azure SQL Database](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).


<a id="aspnetmembership"></a>


## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Wdrażanie aplikacji sieci web, która używa aplikacji ASP.NET usług takich jak członkostwo i profilowania

- [Wdrażanie aplikacji bezpiecznego platformy ASP.NET MVC 5 z członkostwa, OAuth i bazy danych SQL do witryny sieci Web platformy Azure Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Samouczek długi, które tworzy i wdraża aplikację, która używa pojedynczego serwera SQL bazy danych dla danych członkostwa i aplikacji.
- [ASP.NET Identity](https://asp.net/identity/). Zasoby dla tożsamości ASP.NET.
- [Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 części samouczka serii, przedstawia sposób wdrażania bazy danych członkostwa ASP.NET.
- [Konfigurowanie witryny sieci Web, która korzysta z usługi aplikacji](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). Witryny sieci web projektów ale jest również zastosowanie w przypadku projektów aplikacji sieci web.
- [Użytkownicy i role w witrynie sieci Web produkcji](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). Witryny sieci web projektów ale jest również zastosowanie w przypadku projektów aplikacji sieci web.


<a id="precompiling"></a>


## <a name="precompiling-for-deployment"></a>Prekompilowanie dla wdrożenia

- [Omówienie wstępnej kompilacji projektu aplikacji sieci Web ASP.NET](https://msdn.microsoft.com/library/aa983464.aspx) (MSDN).
- [Karta Sieć Web pakowania/publikowania, właściwości projektu](https://msdn.microsoft.com/library/dd410108.aspx) (MSDN).
- [Zaawansowane Prekompilowanie okno dialogowe Ustawienia](https://msdn.microsoft.com/library/hh475319.aspx) (MSDN).


<a id="intranet"></a>


## <a name="deploying-an-intranet-web-application"></a>Wdrażanie aplikacji sieci web w sieci intranet

- [Użyj opcji uwierzytelniania organizacyjnego lokalnego (AD FS) z programem ASP.NET w programie Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (Blog przez Vittorio Bertocci.).
- [Sposób tworzenia witryny intranetowej przy użyciu platformy ASP.NET MVC](https://msdn.microsoft.com/library/gg703322(VS.98).aspx) (MSDN). Starsze writen wskazówki dla programu Visual Studio 2010, nie odzwierciedlają istotne zmiany w szablonach projektu intranet wprowadzone w programie Visual Studio 2013.


<a id="automating"></a>


## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Automatyzacji typowych zadań wdrażania, które nie są automatyzowane fabrycznej

- [Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Wdrażanie dodatkowych plików](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Ustawianie uprawnień folderów w sieci Web publikowanie](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (blog Sayed Hashimi).
- [Jak rozszerzyć pliku elementy docelowe, aby uwzględnić ustawienia rejestru pakietu projektu sieci web](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (blog narzędzi Web Development Tools).
- [Rozszerzanie transformacji XML (plik Web.config)](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (blog Sayed Hashimi). Przedstawia sposób tworzenia niestandardowych przekształceń XDT.
- [Sieci Web narzędzia wdrażania (MSDeploy) niestandardowego dostawcy zajmuje 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (blog Sayed Hashimi). Przedstawia sposób tworzenia niestandardowego dostawcy narzędzia Web Deploy.
- [Pakiet i wdrażanie składników COM](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (blog narzędzi Web Development Tools).
- [Jak pakiet zestawów platformy .NET](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) (blog narzędzi Web Development Tools). Jak wdrożyć zestawów w pamięci GAC.
- [Wszystko — zainicjować Your Azure maszyny Wirtualnej systemu Windows dla serwera sieci Web usług IIS, narzędzie Web Deploy i inne rzeczy skryptu](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (blog Tugberk Ugurlu).


<a id="configuringservers"></a>


## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Konfigurowanie serwerów sieci web, dzięki temu deweloperzy wdrażać je za pomocą narzędzia Web Deploy aplikacji sieci web

- [Instalowanie i konfigurowanie narzędzie Web Deploy w dla administratora i z systemem innym niż administratora wdrożenia](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (IIS.net lokacji).


<a id="hostingprovider"></a>


## <a name="configuring-servers-for-a-hosting-provider"></a>Konfigurowanie serwerów dla dostawcy hostingu

- [Przewodniku dotyczącym wdrażania hostingu platformy Microsoft ASP.NET 4](https://go.microsoft.com/fwlink/?LinkId=191365) (Centrum pobierania Microsoft).
- [Generowanie pliku profilu XML](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (IIS.net lokacji).


<a id="troubleshooting"></a>


## <a name="troubleshooting-deployment-problems"></a>Rozwiązywanie problemów z wdrażaniem

- [Rozwiązywanie problemów z systemu Windows Azure Web Sites w programie Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (witryny Microsoft Azure).
- [Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Rozwiązywanie problemów z](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md).
- [Rozwiązywania typowych problemów z sieci Web wdrażanie](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Kody błędów wdrażania w sieci Web](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (IIS.net lokacji).
- [Narzędzie Web Deployment — często zadawane pytania dotyczące programu Visual Studio i ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN).
- [Podstawowe różnice między usługami IIS a ASP.NET Development Server](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Typowych konfiguracji różnice między rozwoju i produkcji](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md).
- [Hosting aplikacji ASP.NET w trybie średniego zaufania](http://www.4guysfromrolla.com/articles/100307-1.aspx) (4 Guys z witryny Rolla).


<a id="gettinghelp"></a>


## <a name="getting-help-with-a-specific-deployment-question"></a>Uzyskiwanie pomocy dotyczącej zapytania określonego wdrożenia

- [Konfiguracja programu ASP.NET i wdrażania forum](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).


<a id="additional"></a>


## <a name="additional-resources"></a>Dodatkowe zasoby

Ta sekcja zawiera linki do dodatkowych zasobów, które są przydatne w przypadku dowiedzieć się więcej na temat używania narzędzia wdrażania usług IIS i Visual Studio.

Następujących blogach często zawierają informacje dotyczące wdrożenia sieci web programu Visual Studio:

- [W blogu firmy Microsoft w sieci Web narzędzia deweloperskie](https://blogs.msdn.com/b/webdevtools/).
- [Blog Sayed Hashimi](http://www.sedodream.com/).

Następujące zasoby zawierają dokumentację dotyczącą narzędzia Web Deploy, framework usług IIS, Visual Studio używane do wykonywania zadań wdrażania projektu aplikacji sieci web. Można zadawać pytania o narzędziu Web Deploy w [forum narzędzie Web Deployment](https://go.microsoft.com/fwlink/?LinkId=149411) w witrynie sieci web IIS.net.

- [Wprowadzenie do sieci Web wdrażanie](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Instalowanie i konfigurowanie witryny sieci Web wdrażanie](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [Skrypty programu PowerShell do automatyzacji Web wdrażają konfigurację](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Narzędzia wdrażania Web](https://go.microsoft.com/fwlink/?LinkId=151481). Tabeli najwyższego poziomu węzła zawartość dla narzędzia Web Deploy dokumentacji w witrynie TechNet. Zawiera przydatne informacje, ale większość TechNet strony nie zostały zaktualizowane przez lata.
- [Namespace składnika Microsoft.Web.Deployment](https://go.microsoft.com/fwlink/?LinkId=148630). Dokumentacja interfejsu API nie została zaktualizowana od wersji 1.0.
- [Blog zespołu wdrażania sieci Web Microsoft](https://blogs.iis.net/msdeploy/default.aspx).
- [Publikowanie kartę w witrynie sieci web IIS.net](https://www.iis.net/learn/publish).
