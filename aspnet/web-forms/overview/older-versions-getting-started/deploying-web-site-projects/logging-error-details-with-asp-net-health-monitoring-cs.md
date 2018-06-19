---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
title: Rejestrowanie szczegóły błędu z kondycji ASP.NET monitorowania (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: System monitorowania kondycji firmy Microsoft umożliwia łatwe i dostosowywanych do rejestrowania różnych zdarzeń sieci web, łącznie z nieobsługiwanych wyjątków. Ten samouczek przedstawia t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: b1abb452-642a-4ff3-8504-37b85590ff79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
msc.type: authoredcontent
ms.openlocfilehash: 370f19b36628a9811a31e263e468453897cb7d92
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888526"
---
<a name="logging-error-details-with-aspnet-health-monitoring-c"></a>Rejestrowanie szczegóły błędu z kondycji ASP.NET monitorowania (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_CS.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_cs.pdf)

> System monitorowania kondycji firmy Microsoft umożliwia łatwe i dostosowywanych do rejestrowania różnych zdarzeń sieci web, łącznie z nieobsługiwanych wyjątków. Ten samouczek przedstawia przy konfigurowaniu monitorowania systemu kondycji do rejestrowania nieobsługiwanych wyjątków do bazy danych i powiadamiania deweloperzy za pośrednictwem wiadomości e-mail.


## <a name="introduction"></a>Wprowadzenie

Rejestrowanie jest przydatne narzędzie monitorowania kondycji wdrożonej aplikacji i diagnozowanie problemów, które mogą wystąpić. Jest szczególnie ważne do dziennika błędów występujących w wdrożonej aplikacji, dzięki czemu może zostać usunięta. `Error` Zdarzenie jest wywoływane zawsze, gdy wystąpi nieobsługiwany wyjątek w aplikacji ASP.NET; [poprzedniego samouczek](processing-unhandled-exceptions-cs.md) pokazano, jak powiadamiać dewelopera błąd i dziennika jego szczegóły przez utworzenie programu obsługi zdarzeń dla `Error` zdarzeń. Jednak tworzenie `Error` program obsługi zdarzeń do dziennika szczegóły błędu i powiadom dewelopera jest wykorzystywana, ponieważ to zadanie może zostać wykonana przez ASP. W sieci *kondycji systemu monitorującego*.

System monitorowania kondycji została wprowadzona w programie ASP.NET 2.0 i służy do monitorowania prawidłowości wdrożonej aplikacji ASP.NET przez rejestrowania zdarzeń, występujących podczas okresu istnienia żądania lub aplikacji. Zdarzenia zarejestrowane przez system monitorowania kondycji są określane jako *zdarzenia monitorowania kondycji* lub *sieci Web zdarzenia*i obejmują:

- Zdarzenia okresu istnienia aplikacji, na przykład jeśli aplikacja uruchomienia lub zatrzymania
- Zdarzenia zabezpieczeń, w tym nieudanych prób logowania i nieudane żądania autoryzacji adresu URL
- Błędy aplikacji, w tym nieobsługiwane wyjątki, stan widoku analizowania wyjątków, wyjątki poprawności żądań i błędów kompilacji, między innymi typy błędów.

Gdy kondycję monitorowania zdarzenia mogą być rejestrowane do dowolnej liczby określonego *dziennika źródeł*. System monitorowania kondycji jest dostarczany z dziennika źródeł, które rejestrowania zdarzeń sieci Web do bazy danych programu Microsoft SQL Server, w dzienniku zdarzeń systemu Windows lub za pośrednictwem wiadomości e-mail, między innymi. Istnieje również możliwość utworzenia własnych źródeł dziennika.

Zdarzenia monitorowania systemu kondycji dzienniki, wraz z dziennika źródła danych, są zdefiniowane w `Web.config`. Przy użyciu kilku wierszy kodu znaczników konfiguracji służy do rejestrowania wszystkich nieobsługiwanych wyjątków do bazy danych i aby powiadamiać wyjątek za pośrednictwem poczty e-mail monitorowania kondycji.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Eksploracja monitorowania konfiguracji systemu kondycji

Monitorowanie zachowania systemu kondycji jest definiowana za pomocą jej informacje o konfiguracji, który znajduje się w [ `<healthMonitoring>` elementu](https://msdn.microsoft.com/library/2fwh2ss9.aspx) w `Web.config`. Ta sekcja konfiguracji definiuje między innymi następujące trzy ważne informacje:

1. Zdarzenia monitorowania kondycji, gdy zgłoszone, powinny być rejestrowane,
2. Źródła dziennika i
3. Sposób monitorowania zdarzeń zdefiniowanych w (1) każdej kondycji jest mapowany na źródła dziennika zdefiniowane w (2).

Tych informacji jest określany za pośrednictwem trzy elementy podrzędne elementy konfiguracji: [ `<eventMappings>` ](https://msdn.microsoft.com/library/yc5yk01w.aspx), [ `<providers>` ](https://msdn.microsoft.com/library/zaa41kz1.aspx), i [ `<rules>` ](https://msdn.microsoft.com/library/fe5wyxa0.aspx)odpowiednio.

Kondycja domyślne monitorowanie informacji o konfiguracji systemu znajdują się w `Web.config` w pliku `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` folderu. Te informacje konfiguracji domyślnej, z niektórych znaczników usunięta, jednak jest pokazany poniżej:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample1.xml)]

Kondycja monitorowania zdarzeń są definiowane w `<eventMappings>` element, który nadaje nazwę przyjaznych dla człowieka do klasy zdarzenia monitorowania kondycji. W znaczniku powyżej `<eventMappings>` element przypisuje nazwy przyjaznego dla człowieka "Wszystkie błędy" do monitorowania zdarzeń typu kondycji `WebBaseErrorEvent` i nazwie "niepowodzeń" do monitorowania zdarzeń typu kondycji `WebFailureAuditEvent`.

`<providers>` Element definiuje źródeł dziennika, zapewniając im nazw przyjaznych dla człowieka i określając żadnych informacji konfiguracyjnych źródła dziennika. Pierwszy `<add>` element definiuje dostawcy "EventLogProvider", który rejestruje określony monitorowania zdarzeń za pomocą kondycji `EventLogWebEventProvider` klasy. `EventLogWebEventProvider` Klasy rejestruje zdarzenie w dzienniku zdarzeń systemu Windows. Drugi `<add>` element definiuje dostawcy "SqlWebEventProvider", który zapisuje zdarzenia do bazy danych programu Microsoft SQL Server za pośrednictwem `SqlWebEventProvider` klasy. Konfiguracja "SqlWebEventProvider" Określa parametry połączenia bazy danych (`connectionStringName`) wśród innych opcji konfiguracji.

`<rules>` Element mapy zdarzeń określony w `<eventMappings>` element do rejestrowania źródeł `<providers>` elementu. Domyślnie aplikacje sieci web ASP.NET rejestrowania wszystkich nieobsługiwanych wyjątków i niepowodzeń w dzienniku zdarzeń systemu Windows.

## <a name="logging-events-to-a-database"></a>Rejestrowania zdarzeń do bazy danych

Monitorowania systemu domyślna konfiguracja kondycji można dostosować na podstawie aplikacji sieci web aplikacji sieci web, dodając `<healthMonitoring>` sekcji z aplikacją `Web.config` pliku. Możesz podać dodatkowe elementy w `<eventMappings>`, `<providers>`, i `<rules>` sekcje przy użyciu `<add>` elementu. Aby usunąć ustawienie z użyciem konfiguracji domyślnej `<remove>` elementu, lub użyj `<clear />` Aby usunąć wszystkie wartości domyślne z jednej z tych sekcji. Umożliwia konfigurowanie aplikacji sieci web przeglądami książki w celu rejestrowania wszystkich nieobsługiwanych wyjątków do bazy danych programu Microsoft SQL Server przy użyciu `SqlWebEventProvider` klasy.

`SqlWebEventProvider` Klasy jest częścią systemu monitorowania kondycji i rejestruje kondycję monitorowania zdarzeń do określonej bazy danych programu SQL Server. `SqlWebEventProvider` Klasy oczekuje, że określona baza danych zawiera procedury składowanej o nazwie `aspnet_WebEvent_LogEvent`. Tej procedury składowanej jest przekazywana szczegóły zdarzenia, a jest zlecił mu przechowywania szczegóły zdarzenia. Dobre wieści jest, że nie należy utworzyć to przechowywane procedury ani tabelę do przechowywania szczegóły zdarzenia. Te obiekty można dodać przy użyciu bazy danych `aspnet_regsql.exe` narzędzia.

> [!NOTE]
> `aspnet_regsql.exe` Narzędzie została omówiona w [ *Konfigurowanie witryny sieci Web czy korzysta z usługi aplikacji* samouczek](configuring-a-website-that-uses-application-services-cs.md) po Dodaliśmy obsługę ASP. Usługi aplikacji w sieci. W rezultacie witryny przeglądami książki baza danych już zawiera `aspnet_WebEvent_LogEvent` przechowywane procedury, która przechowuje informacje dotyczące zdarzenia do tabeli o nazwie `aspnet_WebEvent_Events`.


Po określeniu niezbędnych procedur składowanych i tabela dodawane do bazy danych liście nie zostanie nakazać kondycji monitorowania do rejestrowania wszystkich nieobsługiwanych wyjątków w bazie danych. W tym celu, dodając następujący kod do witryny sieci Web `Web.config` pliku:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample2.xml)]

Monitorowania konfiguracji znacznika powyżej używa kondycji `<clear />` elementy, aby wyczyścić kondycji wstępnie zdefiniowane informacje o konfiguracji z monitorowania `<eventMappings>`, `<providers>`, i `<rules>` sekcje. Następnie dodaje pojedynczy wpis do każdego z tych sekcji.

- `<eventMappings>` Element definiuje pojedynczego kondycji, monitorowanie zdarzeń odsetek o nazwie "Wszystkie błędy", który jest zgłaszany, gdy wystąpi nieobsługiwany wyjątek.
- `<providers>` Element definiuje źródło pojedynczego dziennika o nazwie "SqlWebEventProvider", która używa `SqlWebEventProvider` klasy. `connectionStringName` Atrybutu została ustawiona na "ReviewsConnectionString", czyli nazwę naszych połączenia zdefiniowany w `<connectionStrings>` sekcji.
- Na koniec &lt;reguły&gt; element wskazuje, że gdy zdarzenie "Wszystkie błędy" wynika, że jej powinny być rejestrowane za pomocą dostawcy "SqlWebEventProvider".

Informacje o konfiguracji powoduje, że kondycji systemu do rejestrowania wszystkich nieobsługiwanych wyjątków w bazie danych przeglądami książki monitorowania.

> [!NOTE]
> `WebBaseErrorEvent` Jest tylko zdarzenia błędów serwera; nie jest wywoływane dla błędów HTTP, np. żądań dotyczących zasobów programu ASP.NET, że nie można odnaleźć. To różni się od zachowania `HttpApplication` klasy `Error` zdarzenie, które jest wywoływane dla serwera i komunikaty o błędach HTTP.


Aby wyświetlić kondycję systemu działania monitorowania, odwiedź witrynę sieci Web i generować błąd w czasie wykonywania, odwiedzając `Genre.aspx?ID=foo`. Powinna zostać wyświetlona strona błędu odpowiednie — wyjątek szczegóły żółty ekranem śmierci (podczas odwiedzania lokalnie) lub niestandardowej strony błędu (podczas odwiedzania witryn w środowisku produkcyjnym). W tle kondycji systemu monitorującego rejestrowane informacje o błędzie do bazy danych. Powinien istnieć jeden rekord w `aspnet_WebEvent_Events` tabeli (zobacz **rysunek 1**); ten rekord zawiera informacje o właśnie wystąpił błąd środowiska uruchomieniowego.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image1.png)

**Rysunek 1**: szczegóły błędu zarejestrowano `aspnet_WebEvent_Events` tabeli  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](logging-error-details-with-asp-net-health-monitoring-cs/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Wyświetlanie dziennik błędów na stronie sieci Web

Z bieżącej konfiguracji witryny sieci Web kondycji systemu monitorującego rejestruje wszystkie nieobsługiwane wyjątki bazy danych. Monitorowanie kondycji nie zapewnia jednak dowolny mechanizm, aby wyświetlić dziennik błędów za pomocą strony sieci web. Można jednak utworzyć strony ASP.NET, który wyświetla te informacje z bazy danych. (Jak zajmiemy się na chwilę, można wybrać opcję ma szczegóły błędu wysłany do Ciebie w wiadomości e-mail).

Jeśli tworzysz takiej strony, upewnij się, że należy wykonać kroki, aby zezwolić tylko autoryzowani użytkownicy wyświetlić szczegóły błędu. Jeśli witryna już wykorzystuje kont użytkowników można używać reguł autoryzacji adresów URL do ograniczenia dostępu do strony do określonych użytkowników lub ról. Aby uzyskać więcej informacji na temat sposobu udzielania lub ograniczanie dostępu do stron sieci web na podstawie zalogowanego użytkownika, zapoznaj się Moje [samouczki dotyczące zabezpieczeń witryny sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> Samouczek kolejnych Eksploruje alternatywnych Błąd rejestrowania i powiadomień systemu o nazwie ELMAH. ELMAH zawiera wbudowany mechanizm Wyświetl dziennik błędów z obu stron sieci web, a jako źródła danych RSS.


## <a name="logging-events-to-email"></a>Rejestrowanie zdarzeń do poczty E-mail

System monitorowania kondycji zawiera dostawcy źródła dziennika "dzienniki" zdarzenia do wiadomości e-mail. Źródło dziennika zawiera te same informacje, które są rejestrowane w bazie danych w treści wiadomości e-mail. To źródło dziennika umożliwia Powiadamiaj dewelopera, gdy wystąpi określone zdarzenie monitorowania kondycji.

Teraz zaktualizuj przeglądy książki konfiguracji witryny sieci Web, dzięki czemu możemy otrzymasz wiadomość e-mail, gdy wyjątek występuje. W tym celu należy wykonać trzy zadania:

1. Konfigurowanie aplikacji sieci web ASP.NET do wysyłania wiadomości e-mail. Jest to osiągane przez określenie sposobu wysyłania wiadomości e-mail za pośrednictwem `<system.net>` element konfiguracji. Więcej informacji na temat wysyłania wiadomości w aplikacji ASP.NET można znaleźć [wysyłania poczty E-mail w programie ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) i [System.Net.Mail — często zadawane pytania](http://systemnetmail.com/).
2. Zarejestruj dostawcę poczty e-mail dziennika źródła w `<providers>` elementu, a
3. Dodawanie wpisu do `<rules>` element, który mapuje zdarzeń "Wszystkie błędy" dostawca źródło dziennika dodanej w kroku (2).

System monitorowania kondycji zawiera dwie klasy dostawcy źródła dziennika poczty e-mail: `SimpleMailWebEventProvider` i `TemplatedMailWebEventProvider`. [ `SimpleMailWebEventProvider` Klasy](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx) wysyła wiadomości e-mail w formacie zwykłego tekstu, która zawiera zdarzenia szczegółowe informacje i zapewnia małego dostosowania treść wiadomości e-mail. Z [ `TemplatedMailWebEventProvider` klasy](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) Określ strony platformy ASP.NET, w których renderowanego kodu znaczników jest używana jako treść wiadomości e-mail. [ `TemplatedMailWebEventProvider` Klasy](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) zapewnia znacznie większą kontrolę nad zawartość i format wiadomości e-mail, ale wymaga nieco więcej wysiłku góry jako trzeba tworzyć strony ASP.NET, który generuje treść wiadomości e-mail. Ten samouczek koncentruje się na temat używania `SimpleMailWebEventProvider` klasy.

Zaktualizuj monitorowania systemu kondycji `<providers>` element `Web.config` pliku, aby uwzględnić źródło dziennika `SimpleMailWebEventProvider` klasy:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample3.xml)]

Używa znacznika powyżej `SimpleMailWebEventProvider` klasy jako dostawcę dziennika źródła i przypisuje go przyjazna nazwa "EmailWebEventProvider". Ponadto `<add>` atrybutu zawiera dodatkowe opcje konfiguracji, takie jak do i z adresów wiadomości e-mail.

Z źródło dziennika wiadomości e-mail zdefiniowanych liście nie zostanie nakazać programowi monitorowania systemu, aby używać tego źródła "rejestrowania" nieobsługiwanych wyjątków kondycji. Jest to osiągane przez dodanie nowej reguły w `<rules>` sekcji:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample4.xml)]

`<rules>` Sekcja zawiera teraz dwie reguły. Pierwsza z nich, o nazwie "Wszystkie błędy do wiadomości E-mail", wysyła wszystkie nieobsługiwane wyjątki źródło dziennika "EmailWebEventProvider". Ta zasada powoduje wysyłanie informacji o błędach w witrynie sieci Web do określonego adres. Reguła "Wszystkie błędy w bazie danych" rejestruje szczegóły błędu bazy danych lokacji. W związku z tym po zmianie nieobsługiwany wyjątek w witrynie jego szczegóły są oba rejestrowane w bazie danych i wysyłane do określony adres e-mail.

**Rysunek 2** pokazuje wiadomości e-mail generowanych przez `SimpleMailWebEventProvider` klasy podczas odwiedzania `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image4.png)

**Rysunek 2**: szczegóły błędu są wysyłane w wiadomości E-mail  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](logging-error-details-with-asp-net-health-monitoring-cs/_static/image6.png))

## <a name="summary"></a>Podsumowanie

System monitorowania kondycji programu ASP.NET umożliwia administratorom monitorowanie kondycji wdrożonej aplikacji sieci web. Zdarzenia monitorowania kondycji są wywoływane, gdy ujawniać pewne akcje, takie jak po zatrzymaniu aplikacji, gdy pomyślnie zalogował się użytkownik lokacji lub gdy wystąpi nieobsługiwany wyjątek. Zdarzenia te mogą być rejestrowane do dowolnej liczby dziennika źródła. W tym samouczku pokazano, jak rejestrować szczegółowe informacje o nieobsługiwanych wyjątków do bazy danych i za pośrednictwem wiadomości e-mail.

Ten samouczek koncentruje się na korzystanie z programu health monitorowania do rejestrowania nieobsługiwanych wyjątków, ale należy pamiętać, że monitorowanie kondycji jest przeznaczona do mierzenia ogólną kondycję wdrożonej aplikacji ASP.NET i zawiera wiele zdarzeń monitorowania kondycji i nie dziennika źródła przedstawione tutaj. Ponadto można tworzyć własne kondycji monitorowanie zdarzeń i dzienników źródła, jeśli będzie to potrzebne wystąpić. Jeśli chcesz dowiedzieć się więcej na temat monitorowania kondycji, dobrym pierwszym krokiem jest zapoznaj się z artykułem [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)w [często zadawane pytania dotyczące monitorowania kondycji](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). Następujące, zapoznaj się [jak: Użyj monitorowanie kondycji w programie ASP.NET 2.0](https://msdn.microsoft.com/library/ms998306.aspx).

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Omówienie monitorowania kondycji programu ASP.NET](https://msdn.microsoft.com/library/bb398933.aspx)
- [Konfigurowanie i dostosowywanie kondycji systemu programu ASP.NET monitorowania](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [Często zadawane pytania — monitorowania w programie ASP.NET 2.0 kondycji](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Porady: Wysyłanie wiadomości E-Mail dla powiadomień dotyczących monitorowania kondycji](https://msdn.microsoft.com/library/ms227553.aspx)
- [Porady: Użyj monitorowanie kondycji w programie ASP.NET](https://msdn.microsoft.com/library/ms998306.aspx)
- [Kondycja monitorowania w programie ASP.NET](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [Poprzednie](processing-unhandled-exceptions-cs.md)
> [dalej](logging-error-details-with-elmah-cs.md)
