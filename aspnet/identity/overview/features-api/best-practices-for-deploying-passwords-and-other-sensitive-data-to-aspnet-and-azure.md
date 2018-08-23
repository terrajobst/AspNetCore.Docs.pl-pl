---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: Najlepsze rozwiązania dotyczące wdrażania haseł i innych danych poufnych na platformie ASP.NET i usłudze Azure App Service | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ten samouczek pokazuje, jak Twój kod może bezpieczne przechowywanie i dostęp do informacji poufnych. Najbardziej istotną kwestią jest, że nigdy nie przechowuj hasła lub inne dawcy...
ms.author: riande
ms.date: 05/21/2015
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: eda2277a4baad8f2a63aa2fdf6ab84f57f1eb0e0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755622"
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Najlepsze rozwiązania dotyczące wdrażania haseł i innych danych poufnych na platformie ASP.NET i usłudze Azure App Service
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Ten samouczek pokazuje, jak Twój kod może bezpieczne przechowywanie i dostęp do informacji poufnych. Najważniejsze jest nigdy nie przechowuj haseł i innych poufnych danych w kodzie źródłowym i wpisów tajnych w środowisku produkcyjnym nie należy używać w trybie projektowania i testowania.
> 
> Przykładowy kod jest prosta aplikacja konsoli zadań WebJob i aplikacji ASP.NET MVC, która wymaga dostępu do bazy danych połączenia ciąg hasła, Twilio, serwis Google czy SendGrid kluczy zabezpieczeń.
> 
> Lokalne ustawienia i PHP wymieniana jest także.


- [Przy użyciu haseł w środowisku programistycznym](#pwd)
- [Praca z parametrów połączenia w środowisku programistycznym](#con)
- [Aplikacje konsoli zadań Webjob](#wj)
- [Wdrażanie kluczy tajnych na platformie Azure](#da)
- [Informacje o lokalnych i PHP](#not)
- [Dodatkowe zasoby](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Przy użyciu haseł w środowisku programistycznym

W samouczkach pokazano często danych poufnych w kodzie źródłowym, miejmy nadzieję z ale należy pamiętać, że nigdy nie należy przechowywać poufnych danych w kodzie źródłowym. Na przykład Moje [aplikacji ASP.NET MVC 5 za pomocą wiadomości SMS i wiadomości e-mail funkcji 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) samouczek pokazuje, że w *web.config* pliku:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

*Web.config* plik jest kod źródłowy, więc tych kluczy tajnych nigdy nie powinny być przechowywane w tym pliku. Na szczęście `<appSettings>` element ma `file` atrybut, który pozwala na określenie zewnętrznego pliku, który zawiera ustawienia konfiguracji aplikacji poufnych. Można przenieść wszystkich wpisów tajnych do zewnętrznego pliku, tak długo, jak zewnętrzny plik nie zostanie zaznaczona w drzewie źródłowym. Na przykład w niej następujące znaczniki pliku *AppSettingsSecrets.config* zawiera wszystkie wpisy tajne aplikacji:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

Znaczniki w pliku zewnętrznym (*AppSettingsSecrets.config* w tym przykładzie), ten sam kod znaczników znajduje się w *web.config* pliku:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

Środowisko uruchomieniowe ASP.NET Scala zawartość pliku zewnętrznego ze znacznikami w &lt;appSettings&gt; elementu. Środowisko uruchomieniowe ignoruje atrybut pliku, jeśli nie można odnaleźć określonego pliku.

> [!WARNING]
> Zabezpieczenia — nie należy dodawać swoje *.config wpisów tajnych* plik do projektu lub sprawdź go do kontroli źródła. Domyślnie program Visual Studio ustawia `Build Action` do `Content`, co oznacza, że plik jest wdrożony. Aby uzyskać więcej informacji, zobacz [Dlaczego nie wszystkie pliki w folderze projektu wdrożony?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Chociaż można używać dowolnego rozszerzenia dla *.config wpisów tajnych* pliku, warto zachować *.config*, jak pliki konfiguracji nie są obsługiwane przez usługi IIS. Zauważ również, że *AppSettingsSecrets.config* plik znajduje się dwa poziomy katalogu w górę od *web.config* plików, dzięki czemu jest całkowicie poza katalog rozwiązania. Dzięki przeniesieniu pliku poza katalog rozwiązania &quot;git Dodaj \* &quot; nie będzie dodać go do repozytorium.


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Praca z parametrów połączenia w środowisku programistycznym

Program Visual Studio tworzy nowych projektach programu ASP.NET, które używają [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). LocalDB został utworzony specjalnie dla środowiska programistycznego. Nie wymaga hasła, więc nie trzeba nic robić, aby uniemożliwić wpisów tajnych sprawdzany w kodzie źródłowym. Niektóre zespoły deweloperów używanie pełnych wersji programu SQL Server (lub inne DBMS), wymagające hasła.

Możesz użyć `configSource` atrybutu, aby zastąpić całą `<connectionStrings>` znaczników. W odróżnieniu od `<appSettings>` `file` atrybut, który scala znaczników, `configSource` atrybut zastępuje znaczników. Ilustruje poniższy kod znaczników `configSource` atrybutu w *web.config* pliku:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Jeśli używasz `configSource` atrybutu, jak pokazano powyżej, aby przenieść parametry połączenia do pliku zewnętrznego i program Visual Studio Utwórz nową witrynę sieci web, nie będzie mógł wykryć korzystania z bazy danych i nie będzie dostępna opcja konfigurowania bazy danych po użytkownik pu Publikuj na platformie Azure z programu Visual Studio. Jeśli używasz `configSource` atrybut, można użyć programu PowerShell do tworzenia i wdrażania witryn sieci web i bazy danych lub można utworzyć witryny sieci web i bazy danych w portalu usługi przed opublikowaniem. [New AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) skrypt spowoduje utworzenie nowej witryny sieci web i bazy danych.


> [!WARNING]
> Zabezpieczenia — w przeciwieństwie do *AppSettingsSecrets.config* pliku, plik ciągów połączenia zewnętrznego musi być w tym samym katalogu głównym *web.config* plików, dlatego należy zachować środki ostrożności, aby zagwarantować, że nie zaznaczaj jej w repozytorium źródłowym.


> [!NOTE]
> **Ostrzeżenie o zabezpieczeniach w pliku wpisów tajnych:** najlepszym rozwiązaniem jest nieużywanie tajemnice produkcji w projektowania i testowania. Korzystanie z haseł produkcji w testowym lub deweloperskim przeciek tych kluczy tajnych.


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>Aplikacje konsoli zadań Webjob

*App.config* plik używany przez aplikację konsoli nie obsługuje ścieżek względnych, ale obsługuje ona ścieżek bezwzględnych. Klucze tajne wychodzenia z katalogu projektu, można użyć ścieżki bezwzględnej. Poniższy kod przedstawia wpisów tajnych w *C:\secrets\AppSettingsSecrets.config* plików i innych poufnych danych w *app.config* pliku.

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Wdrażanie kluczy tajnych na platformie Azure

W przypadku wdrażania aplikacji sieci web na platformie Azure, *AppSettingsSecrets.config* pliku nie zostanie wdrożony (jest to zamierzone). Można przejść do [portalu zarządzania systemu Azure](https://azure.microsoft.com/services/management-portal/) i ustawić je ręcznie, aby to zrobić:

1. Przejdź do [ https://portal.azure.com ](https://portal.azure.com)i zaloguj się przy użyciu swoich poświadczeń platformy Azure.
2. Kliknij przycisk **Przeglądaj &gt; aplikacji sieci Web**, następnie kliknij nazwę aplikacji sieci web.
3. Kliknij przycisk **wszystkie ustawienia &gt; ustawienia aplikacji**.

**Ustawienia aplikacji** i **parametry połączenia** wartości zastąpienie tych samych ustawień w *web.config* pliku. W tym przykładzie firma Microsoft nie zostały wdrożone te ustawienia na platformie Azure, ale jeśli te klucze znajdowały się w *web.config* pliku, ustawienia wyświetlane w portalu pierwszeństwo.

Najlepszym rozwiązaniem jest wykonanie czynności opisanych [przepływ pracy DevOps](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) i użyj [programu Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (lub innej platformy, takie jak [Chef](http://www.opscode.com/chef/) lub [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) do Automatyzowanie, ustawienia te wartości na platformie Azure. Poniższy skrypt programu PowerShell używa [CliXml eksportu](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) do wyeksportowania zaszyfrowane klucze tajne na dysku:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

W skrypcie powyżej "Name" jest nazwą klucza tajnego, takie jak "&quot;platformy FB\_AppSecret&quot; lub"TwitterSecret". Można wyświetlić pliku ".credential" utworzony przez skrypt w przeglądarce. Poniższy fragment kodu sprawdza każdy z plików poświadczeń i ustawia wpisów tajnych aplikacji sieci web o nazwie:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Zabezpieczenia — nie dołączaj hasła lub inne wpisy tajne w skrypcie programu PowerShell, wykonując tak zaprzeczenie celem wdrażania poufnych danych przy użyciu skryptu programu PowerShell. [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) polecenie cmdlet zapewnia mechanizm bezpiecznego można uzyskać hasło. Przy użyciu wiersza interfejsu użytkownika może uniemożliwić przeciek hasła.


### <a name="deploying-db-connection-strings"></a>Wdrażanie parametry połączenia bazy danych

Parametry połączenia bazy danych są obsługiwane podobnie do ustawień aplikacji. W przypadku wdrożenia aplikacji sieci web w programie Visual Studio, parametry połączenia zostaną skonfigurowane dla Ciebie. Można to sprawdzić, w portalu. Jest to zalecany sposób ustawimy parametry połączenia przy użyciu programu PowerShell. Przykładowy skrypt programu PowerShell utworzy witryny sieci Web i bazy danych, a następnie ustawia parametry połączenia w witrynie sieci Web, Pobierz [New AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) z [biblioteki skryptów Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Informacje o języku PHP

Ponieważ pary klucz wartość dla obu **ustawienia aplikacji** i **parametry połączenia** są przechowywane w zmiennych środowiskowych w usłudze Azure App Service deweloperów, którzy łatwo używać dowolnej może struktur (takich jak PHP) aplikacji sieci web Pobierz te wartości. Zobacz Stefan Schackow [Windows Azure Web Sites: How Application Strings and Connection Strings Work](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) wpis w blogu, zawierającym fragment PHP można odczytać ustawień aplikacji i parametry połączenia.

## <a name="notes-for-on-premises-servers"></a>Informacje o serwerach lokalnych

Jeśli wdrażasz do serwerów sieci web w środowisku lokalnym, możesz pomóc bezpieczne wpisy tajne przy [szyfrowania sekcji konfiguracyjnych w pliku konfiguracji](https://msdn.microsoft.com/library/ff647398.aspx). Jako alternatywę, można użyć tej samej metody, zalecane w przypadku usługi Azure Websites: Zachowaj ustawienia środowiska deweloperskiego w plikach konfiguracji i używać wartości zmiennych środowiskowych ustawień w środowisku produkcyjnym. W takim jednak trzeba napisać kod aplikacji dla funkcji, które odbywa się automatycznie w usłudze Azure Websites: pobrać ustawień ze zmiennych środowiskowych i użyć tych wartości zamiast ustawień pliku konfiguracji lub ustawień pliku konfiguracji podczas Nie znaleziono zmiennych środowiskowych.

<a id="addRes"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

Na przykład programu PowerShell skrypt, który tworzy aplikacja sieci web i bazy danych, ustawia parametry połączenia i ustawienia aplikacji, Pobierz [New AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) z [biblioteki skryptów Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Zobacz Stefan Schackow [Windows Azure Web Sites: sposób działania ciągów Application Strings and połączenia](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


Specjalne dzięki Barry Dorrans ( [ @blowdart ](https://twitter.com/blowdart) ) i Farre Carlos przeglądania.
