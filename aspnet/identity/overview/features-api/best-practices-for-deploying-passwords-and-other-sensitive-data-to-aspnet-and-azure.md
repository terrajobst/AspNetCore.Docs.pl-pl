---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: Najlepsze rozwiązania dotyczące wdrażania haseł i innych poufnych danych do platformy ASP.NET i usługi Azure App Service | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ten samouczek pokazuje, jak kod może bezpiecznie przechowywać i dostęp do informacji o. Najważniejsze punkt znajduje się, że nigdy nie należy przechowywać haseł i innych Wyślij...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2015
ms.topic: article
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 995d9a088e3095f36a01d2adb19ec08e6a6d1b3e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "28033024"
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Najlepsze rozwiązania dotyczące wdrażania haseł i innych poufnych danych do platformy ASP.NET i usługi Azure App Service
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ten samouczek pokazuje, jak kod może bezpiecznie przechowywać i dostęp do informacji o. Najważniejsze punkt znajduje się w kodzie źródłowym nigdy nie należy przechowywać haseł i innych poufnych danych i kluczy tajnych w środowisku produkcyjnym nie można używać w trybie projektowania i testowania.
> 
> Przykładowy kod jest prosta aplikacja konsoli zadania WebJob i aplikacji ASP.NET MVC, która potrzebuje dostępu do bazy danych połączenia ciąg hasła, usługi Twilio, Google i SendGrid kluczy zabezpieczeń.
> 
> Lokalnie również wspomniano ustawienia i PHP.


- [Praca z hasła w środowisku programistycznym](#pwd)
- [Praca z parametrów połączenia w środowisku programistycznym](#con)
- [Aplikacje konsoli zadań Webjob](#wj)
- [Wdrażanie kluczy tajnych na platformie Azure](#da)
- [Informacje o lokalnych i PHP](#not)
- [Dodatkowe zasoby](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Praca z hasła w środowisku programistycznym

Samouczki przedstawiają często poufne dane w kodzie źródłowym miejmy nadzieję, że z ostrzeżenie, że nigdy nie należy przechowywać poufnych danych w kodzie źródłowym. Na przykład Mój [aplikacji ASP.NET MVC 5 z 2FA SMS i wiadomości e-mail](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) samouczek zawiera następujące opcje w *web.config* pliku:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

*Web.config* plik jest kod źródłowy, więc te klucze tajne nigdy nie powinny być przechowywane w tym pliku. Na szczęście `<appSettings>` element ma `file` atrybut, który pozwala na określenie pliku zewnętrznego, który zawiera ustawienia konfiguracji aplikacji poufnych. Można przenieść wszystkich kluczy tajnych do pliku zewnętrznego, tak długo, jak zewnętrzny plik nie jest zaznaczone pole do Twojego źródła drzewa. Na przykład w następujących znaczników, plik *AppSettingsSecrets.config* zawiera wszystkie klucze tajne aplikacji:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

Znacznika w pliku zewnętrznym (*AppSettingsSecrets.config* w tym przykładzie), znaczników tej samej znajduje się w *web.config* pliku:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

Zawartość zewnętrznego pliku z kodu znaczników w scala środowiska uruchomieniowego ASP.NET &lt;appSettings&gt; elementu. Środowisko uruchomieniowe ignoruje atrybut pliku, jeśli nie można odnaleźć określonego pliku.

> [!WARNING]
> Zabezpieczenia — nie należy dodawać Twojej *.config kluczy tajnych* plików do projektu lub sprawdź go do kontroli źródła. Domyślnie program Visual Studio ustawia `Build Action` do `Content`, co oznacza, że plik jest wdrożona. Aby uzyskać więcej informacji, zobacz [Dlaczego nie wszystkie pliki w folderze projektu wdrożony?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Mimo że można użyć dowolnego rozszerzenia dla *.config kluczy tajnych* pliku, warto zachować *.config*, jak pliki konfiguracji nie są obsługiwane przez usługi IIS. Zauważ również, że *AppSettingsSecrets.config* plik jest dwa poziomy katalogu się z *web.config* pliku, tak aby zawierała całkowicie poza katalog rozwiązania. Przez przenoszenie pliku poza katalog rozwiązania &quot;dodać git \* &quot; nie dodać go do repozytorium.


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Praca z parametrów połączenia w środowisku programistycznym

Program Visual Studio tworzy nowe projekty programu ASP.NET korzystających z [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). LocalDB został utworzony specjalnie z myślą o środowisku programistycznym. Nie wymaga hasła, dlatego nie trzeba wykonywać żadnych czynności, aby zapobiec kluczy tajnych ze sprawdzania do kodu źródłowego. Niektóre zespoły deweloperów, użyj pełnej wersji programu SQL Server (lub innych DBMS) wymagające hasła.

Można użyć `configSource` atrybutu Zastąp całą `<connectionStrings>` znaczników. W odróżnieniu od `<appSettings>` `file` atrybut, który scala znaczników, `configSource` atrybutu zastępuje znaczników. Poniżej przedstawiono znaczników `configSource` atrybutu w *web.config* pliku:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Jeśli używasz `configSource` atrybutu, jak pokazano powyżej, aby przenieść parametry połączenia do pliku zewnętrznego i mieć Visual Studio Utwórz nową witrynę sieci web, nie będzie mógł wykryć korzystania z bazy danych i nie jest dostępna opcja konfigurowania bazy danych podczas należy zakupić Publikuj na platformie Azure w programie Visual Studio. Jeśli używasz `configSource` atrybutu, można użyć programu PowerShell do tworzenia i wdrażania witryn sieci web i bazy danych lub można utworzyć witrynę sieci web i bazy danych w portalu przed opublikowaniem. [AzureWebsitewithDB.ps1 nowy](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) skryptu spowoduje utworzenie nowej witryny sieci web i bazy danych.


> [!WARNING]
> Zabezpieczenia — w odróżnieniu od *AppSettingsSecrets.config* pliku, plik ciągów połączenia zewnętrznego musi być w tym samym katalogu głównym *web.config* plików, dlatego należy zachować środki ostrożności, aby zagwarantować, że Nie wyszukuj go do repozytorium źródła.


> [!NOTE]
> **Ostrzeżenie o zabezpieczeniach w pliku kluczy tajnych:** najlepszym rozwiązaniem jest, aby nie używać hasła produkcji testowych i programistycznych. Przy użyciu haseł produkcji w testowych lub przecieków tych kluczy tajnych.


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>Aplikacje konsoli zadań Webjob

*App.config* pliku używany przez aplikację konsoli nie obsługuje ścieżek względnych, ale obsługuje ścieżek bezwzględnych. Ścieżka bezwzględna służy do przenoszenia tajnych poza katalogiem projektu. Następujący kod przedstawia kluczy tajnych w *C:\secrets\AppSettingsSecrets.config* plików i z systemem innym niż poufne dane zawarte w *app.config* pliku.

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Wdrażanie kluczy tajnych na platformie Azure

Podczas wdrażania aplikacji sieci web na platformie Azure, *AppSettingsSecrets.config* pliku nie zostaną wdrożone (do którego ma). Można przejść do [portalu zarządzania Azure](https://azure.microsoft.com/services/management-portal/) i ustaw je ręcznie, w tym:

1. Przejdź do [ https://portal.azure.com ](https://portal.azure.com)i zaloguj się przy użyciu poświadczeń platformy Azure.
2. Kliknij przycisk **Przeglądaj &gt; aplikacje sieci Web**, następnie kliknij nazwę aplikacji sieci web.
3. Kliknij przycisk **wszystkie ustawienia &gt; ustawienia aplikacji**.

**Ustawień aplikacji** i **ciąg połączenia** wartości zastępują tych samych ustawień w *web.config* pliku. W tym przykładzie firma Microsoft nie zostały wdrożone te ustawienia na platformie Azure, ale jeśli klucze te były w *web.config* pliku, ustawienia wyświetlane w portalu będzie pierwszeństwo.

Najlepszym rozwiązaniem jest wykonaj [DevOps przepływu pracy](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) i użyj [programu Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (lub innej platformy takie jak [Chef](http://www.opscode.com/chef/) lub [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) do zautomatyzować, ustawianie tych wartości na platformie Azure. Poniższy skrypt programu PowerShell używa [CliXml eksportu](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) można wyeksportować zaszyfrowanych kluczy tajnych na dysku:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

W powyższym skryptu "Name" jest nazwą klucz tajny, takich jak "&quot;FB\_AppSecret&quot; lub"TwitterSecret". Można wyświetlić plik ".credential" utworzony przez skrypt w przeglądarce. Poniższy fragment testów każdy z plików poświadczeń i ustawia klucze tajne dla aplikacji sieci web o nazwie:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Zabezpieczenia — nie zawiera hasła lub innych informacji poufnych skrypt programu PowerShell, w sposób tak stanowi zaprzeczenie celu do wdrażania danych poufnych za pomocą skryptu programu PowerShell. [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) polecenia cmdlet zapewnia mechanizm bezpiecznego uzyskanie hasła. Użycie wiersza interfejsu użytkownika może uniemożliwić przeciek hasła.


### <a name="deploying-db-connection-strings"></a>Wdrażanie parametry połączenia bazy danych

Parametry połączenia bazy danych są obsługiwane podobnie do ustawień aplikacji. W przypadku wdrożenia aplikacji sieci web w programie Visual Studio, parametry połączenia zostaną skonfigurowane automatycznie. Można to sprawdzić, w portalu. Zalecanym sposobem Ustaw ciąg połączenia jest przy użyciu programu PowerShell. Na przykład skrypt programu PowerShell tworzy witryny sieci Web i bazy danych i ustawia parametry połączenia w witrynie internetowej, Pobierz [AzureWebsitewithDB.ps1 nowy](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) z [biblioteki Azure skryptu](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Uwagi dla PHP

Ponieważ pary klucz wartość dla obu **ustawień aplikacji** i **parametry połączenia** są przechowywane w zmiennych środowiskowych w usłudze Azure App Service, deweloperów, które łatwo używać żadnych może struktur (takich jak PHP) aplikacji sieci web pobierania tych wartości. Zobacz Stefan Schackow [Windows Azure Web Sites: jak ciągi aplikacji i pracy ciągów połączenia](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) blogu, który pokazuje fragment PHP na odczytywanie ustawień aplikacji i parametry połączenia.

## <a name="notes-for-on-premises-servers"></a>Informacje o lokalnych serwerów

Jeśli wdrażasz do lokalnych serwerów sieci web, możesz pomóc bezpiecznego hasła przez [szyfrowanie sekcji konfiguracyjnych plików konfiguracji](https://msdn.microsoft.com/library/ff647398.aspx). Alternatywnie, można użyć tej samej metody zalecane w przypadku witryn sieci Web platformy Azure: Zachowaj ustawienia środowiska deweloperskiego w plikach konfiguracji i używać wartości zmiennych środowiskowych dla ustawień produkcji. W takim przypadku jednak trzeba napisać kod aplikacji dla funkcji, które są wykonywane automatycznie w witrynach sieci Web platformy Azure: pobrać ustawienia zmiennych środowiskowych i użyć tych wartości, zamiast ustawień pliku konfiguracji lub ustawień pliku konfiguracji po zmienne środowiskowe nie został znaleziony.

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

Na przykład programu PowerShell skrypt, który tworzy aplikacja sieci web i bazy danych, ustawia parametry połączenia + ustawienia aplikacji, pobierania [AzureWebsitewithDB.ps1 nowy](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) z [biblioteki Azure skryptu](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Zobacz Stefan Schackow [Windows Azure Web Sites: jak połączenia i ciągi aplikacji ciągi pracy](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


Specjalne dzięki użyciu Dorrans Marcin ( [ @blowdart ](https://twitter.com/blowdart) ) i Farre Artur weryfikacji.
