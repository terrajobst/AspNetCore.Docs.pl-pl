---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
title: Rejestrowanie szczegółów błędu za pomocą (VB) do monitorowania kondycji ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: System monitorowania kondycji przez firmę Microsoft umożliwia łatwe i możliwe do dostosowania do rejestrowania różnych zdarzeń w sieci web, łącznie z nieobsługiwanych wyjątków. W tym samouczku przedstawiono t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 09a6c74e-936a-4c04-8547-5bb313a4e4a3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
msc.type: authoredcontent
ms.openlocfilehash: 1ed7b63989dc6ea7e46210a45612e2672a662177
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382475"
---
<a name="logging-error-details-with-aspnet-health-monitoring-vb"></a>Rejestrowanie szczegółów błędów za pomocą programu Health ASP.NET Monitoring (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_VB.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_vb.pdf)

> System monitorowania kondycji przez firmę Microsoft umożliwia łatwe i możliwe do dostosowania do rejestrowania różnych zdarzeń w sieci web, łącznie z nieobsługiwanych wyjątków. W tym samouczku przedstawiono konfigurowanie kondycji systemu monitorowania, rejestrowania nieobsługiwanych wyjątków z bazą danych i deweloperom za pośrednictwem wiadomości e-mail powiadamiania.


## <a name="introduction"></a>Wprowadzenie

Rejestrowanie jest użytecznym narzędziem do monitorowania kondycji wdrożoną aplikację i diagnozowania wszelkich problemów, które mogą wystąpić. Jest to szczególnie ważne rejestrować błędy występujące we wdrożonej aplikacji, dzięki czemu można naprawić. `Error` Zdarzenie jest wywoływane zawsze wtedy, gdy wystąpił nieobsługiwany wyjątek występuje w aplikacji ASP.NET; [poprzedni Samouczek](processing-unhandled-exceptions-vb.md) pokazano, jak dziennika jego szczegóły, tworząc program obsługi zdarzeń dla ipowiadamiadeweloperabłąd`Error` zdarzeń. Jednak tworzenie `Error` program obsługi zdarzeń, aby rejestrować szczegóły błędu i powiadamia dewelopera jest zbędne, ponieważ to zadanie może zostać wykonana przez ASP. NET firmy *system monitorowania kondycji*.

Kondycja systemu monitorującego została wprowadzona w programie ASP.NET 2.0 i służy do monitorowania prawidłowości wdrożonej aplikacji ASP.NET przez rejestrowania zdarzeń, które występują podczas okresu istnienia żądania lub aplikacji. Zdarzenia zarejestrowane przez kondycji systemu monitorowania są określane jako *zdarzeń monitorowania kondycji* lub *Web zdarzenia*i obejmują:

- Zdarzenia okresu istnienia aplikacji, takich jak podczas uruchomienia lub zatrzymania w aplikacji
- Zdarzenia zabezpieczeń, w tym nieudanych prób zalogowania i adres URL autoryzacji żądania zakończone niepowodzeniem
- Błędy aplikacji, w tym nieobsługiwane wyjątki, stan widoku analizowania wyjątki, żądania weryfikacji wyjątki i błędy kompilacji, wśród innych typów błędów.

Gdy jest wywoływane zdarzenie monitorowania kondycji mogą być rejestrowane do dowolnej liczby określonej *dziennika źródła*. Kondycja systemu monitorowania jest dostarczany z źródeł dziennika, które rejestrowania zdarzeń w sieci Web do bazy danych programu Microsoft SQL Server, w dzienniku zdarzeń Windows lub za pośrednictwem wiadomości e-mail, między innymi. Można również utworzyć własne źródła dzienników.

Zdarzenia kondycji systemu monitorowania dzienników wraz z dziennika źródła danych, są definiowane w `Web.config`. Przy użyciu kilku wierszy kodu znaczników konfiguracji można użyć monitorowania logowania wszystkie nieobsłużone wyjątki bazy danych i powiadamiać Cię o wyjątku za pośrednictwem poczty e-mail kondycji.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Eksplorowanie monitorowania konfiguracji systemu kondycji

Kondycja monitorowanie zachowania systemu jest definiowany przez jej informacje o konfiguracji, który znajduje się w [ `<healthMonitoring>` elementu](https://msdn.microsoft.com/library/2fwh2ss9.aspx) w `Web.config`. Ta sekcja konfiguracji zdefiniowano, między innymi następujące trzy ważne informacje:

1. Zdarzeń monitorowania kondycji, gdy wywoływane, powinny być rejestrowane,
2. Źródła dzienników i
3. Jak monitorowania zdarzeń zdefiniowanych w (1) każdego kondycji jest mapowany do źródła dzienników zdefiniowane w (2).

Informacja ta jest określona przez trzy elementy podrzędne elementy konfiguracji: [ `<eventMappings>` ](https://msdn.microsoft.com/library/yc5yk01w.aspx), [ `<providers>` ](https://msdn.microsoft.com/library/zaa41kz1.aspx), i [ `<rules>` ](https://msdn.microsoft.com/library/fe5wyxa0.aspx), odpowiednio.

Kondycja domyślne monitorowanie informacji o konfiguracji systemu znajdują się w `Web.config` w pliku `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` folderu. Domyślne informacje konfiguracyjne z niektórych znaczników usunięte w celu skrócenia programu, znajdują się poniżej:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample1.xml)]

Kondycja interesujących Cię wydarzeń monitorowania są zdefiniowane w `<eventMappings>` element, który umożliwia nadanie nazwy przyjaznego dla człowieka klasy zdarzeń monitorowania kondycji. W powyższym znaczników `<eventMappings>` element przypisuje nazwę przyjaznego dla człowieka "Wszystkie błędy" do monitorowania zdarzeń typu kondycji `WebBaseErrorEvent` i nazwie "Błąd audytów" zdarzeń typu monitorowania kondycji `WebFailureAuditEvent`.

`<providers>` Element definiuje źródła dzienników, dając im nazw przyjaznych dla człowieka i określając dziennika specyficznymi dla źródła informacji o konfiguracji. Pierwszy `<add>` element definiuje dostawcę "EventLogProvider", która tworzy dzienniki monitorowania zdarzeń za pomocą kondycji określony `EventLogWebEventProvider` klasy. `EventLogWebEventProvider` Klasy rejestruje zdarzenie w dzienniku zdarzeń Windows. Drugi `<add>` element definiuje dostawcę "SqlWebEventProvider", która tworzy dzienniki zdarzeń do bazy danych programu Microsoft SQL Server za pośrednictwem `SqlWebEventProvider` klasy. Konfiguracja "SqlWebEventProvider" Określa parametry połączenia bazy danych (`connectionStringName`) wśród innych opcji konfiguracji.

`<rules>` Element mapy zdarzeń określony w `<eventMappings>` elementu, aby zalogować się źródeł `<providers>` elementu. Domyślnie aplikacji sieci web ASP.NET rejestrowania wszystkich nieobsługiwanych wyjątków i niepowodzeń w dzienniku zdarzeń Windows.

## <a name="logging-events-to-a-database"></a>Rejestrowanie zdarzeń do bazy danych

Kondycja monitorowania konfiguracji domyślnego systemu można dostosować na podstawie aplikacji sieci web aplikacji sieci web, dodając `<healthMonitoring>` sekcji z aplikacją `Web.config` pliku. Może zawierać dodatkowe elementy w `<eventMappings>`, `<providers>`, i `<rules>` sekcje przy użyciu `<add>` elementu. Aby usunąć ustawienie z domyślnie używają konfiguracji `<remove>` elementu lub użyj `<clear />` do usuwania wszystkich wartości domyślnych jedna z nich. Skonfigurujmy aplikacji sieci web przeglądy książki do logowania się wszystkie nieobsłużone wyjątki bazy danych programu Microsoft SQL Server przy użyciu `SqlWebEventProvider` klasy.

`SqlWebEventProvider` Klasa jest częścią systemu monitorowania zdrowia i dzienniki zdarzeń na określonej bazy danych SQL Server do monitorowania kondycji. `SqlWebEventProvider` Klasy oczekuje, że określona baza danych zawiera procedury składowanej o nazwie `aspnet_WebEvent_LogEvent`. Tę procedurę składowaną jest przekazywany szczegóły zdarzenia i podobne zadanie zostanie przypisane przechowywanie szczegółów zdarzenia. Dobra wiadomość jest konieczne do utworzenia tego przechowywane procedury, ani tabelę do przechowywania szczegóły zdarzenia. Te obiekty można dodać do bazy danych za pomocą `aspnet_regsql.exe` narzędzia.

> [!NOTE]
> `aspnet_regsql.exe` Narzędzie została omówiona w [ *Konfigurowanie witryny sieci Web, korzysta z usługi aplikacji* samouczek](configuring-a-website-that-uses-application-services-vb.md) po Dodaliśmy obsługę dla stron ASP. Usługi aplikacji w sieci. W związku z tym, przeglądy książki witryny internetowej baza danych zawiera już `aspnet_WebEvent_LogEvent` procedurą składowaną, która przechowuje informacje o zdarzeniu do tabeli o nazwie `aspnet_WebEvent_Events`.


Po utworzeniu niezbędnych procedur składowanych i tabela dodawane do bazy danych pozostaje nakazać kondycji monitorowania, aby rejestrować wszystkie nieobsłużone wyjątki w bazie danych. To osiągnąć, dodając następujący kod do witryny sieci Web `Web.config` pliku:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample2.xml)]

Monitorowania znaczników konfiguracji powyżej używa kondycji `<clear />` elementy, aby wyczyścić monitorowania informacji o konfiguracji z kondycji wstępnie zdefiniowanych `<eventMappings>`, `<providers>`, i `<rules>` sekcje. Następnie dodaje pojedynczy wpis dla każdego z tych sekcji.

- `<eventMappings>` Element definiuje pojedynczy kondycji monitorowania zdarzenia o nazwie "Wszystkie błędy", które jest wywoływane po każdym wystąpieniu nieobsługiwanego wyjątku.
- `<providers>` Element definiuje pojedynczy dziennik źródła o nazwie "SqlWebEventProvider", który używa `SqlWebEventProvider` klasy. `connectionStringName` Atrybut został ustawiony na "ReviewsConnectionString", która jest nazwą połączenia naszej parametrów zdefiniowanych w `<connectionStrings>` sekcji.
- Na koniec &lt;reguły&gt; element wskazuje, że gdy zdarzenie "Wszystkie błędy" wynika, że jej powinny być rejestrowane przy użyciu dostawcy "SqlWebEventProvider".

Te informacje o konfiguracji powoduje, że monitorowania systemu, aby rejestrować wszystkie nieobsłużone wyjątki w bazie danych przeglądy książki kondycji.

> [!NOTE]
> `WebBaseErrorEvent` Zdarzenie jest zgłaszane tylko w błędów serwera; nie jest inicjowane dla błędów HTTP, takich jak żądania dla zasobu ASP.NET, które nie zostało odnalezione. To różni się od zachowania `HttpApplication` klasy `Error` zdarzenie, które jest wywoływane dla serwera i błędów HTTP.


Aby wyświetlić kondycję systemu w działaniu monitorowania, odwiedź witrynę internetową i wygenerować błąd w czasie wykonywania, odwiedzając stronę `Genre.aspx?ID=foo`. Zobaczyć stronę odpowiedni komunikat o błędzie — wyjątek szczegóły żółty ekranem śmierci (gdy użytkownik odwiedzi lokalnie) lub niestandardowej strony błędu (podczas odwiedzania witryn w środowisku produkcyjnym). Za kulisami kondycji systemu monitorującego rejestrowane informacje o błędzie do bazy danych. Należy do jednego rekordu w `aspnet_WebEvent_Events` tabeli (zobacz **rys.1**); ten rekord zawiera informacje o błąd w czasie wykonywania, który właśnie wykonana.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image1.png)

**Rysunek 1**: szczegóły błędu zostały zarejestrowane do `aspnet_WebEvent_Events` tabeli  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](logging-error-details-with-asp-net-health-monitoring-vb/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Wyświetlanie dziennika błędów na stronie sieci Web

Za pomocą bieżącej konfiguracji witryny sieci Web kondycji systemu monitorującego rejestruje wszystkie nieobsłużone wyjątki bazy danych. Monitorowanie kondycji zapewnia jednak każdy mechanizm, aby wyświetlić dziennik błędów, za pośrednictwem strony sieci web. Jednak można utworzyć strony ASP.NET, która wyświetla te informacje z bazy danych. (Ponieważ zajmiemy się tym chwilowo, możesz zdecydować się na szczegóły błędu wysłana do Ciebie w wiadomości e-mail.)

Jeśli tworzysz takiej strony, upewnij się, że należy wykonać czynności, aby zezwolić tylko autoryzowani użytkownicy wyświetlić szczegóły błędu. Jeśli witryna już wykorzystuje kont użytkowników, możesz użyć reguł autoryzacji adresów URL, aby ograniczyć dostęp do strony, aby niektórych użytkowników lub ról. Aby uzyskać więcej informacji na temat sposobu przyznawanie lub ograniczanie dostępu do stron sieci web na podstawie zalogowanego użytkownika, zobacz mój [samouczki dotyczące zabezpieczeń witryny sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> W tym samouczku kolejnych przedstawiono alternatywnych Błąd rejestrowania i powiadomień systemu o nazwie ELMAH. ELMAH zawiera wbudowany mechanizm, aby wyświetlić dziennik błędów, ze strony sieci web oraz jako źródła danych RSS.


## <a name="logging-events-to-email"></a>Rejestrowanie zdarzeń do poczty E-mail

Kondycja systemu monitorowania obejmuje dzienników dostawcy źródła "rejestruje" zdarzenie do wiadomości e-mail. Źródło dziennika zawiera te same informacje, które są rejestrowane w bazie danych w treści wiadomości e-mail. Możesz używać tego źródła dziennika, aby powiadomić deweloperem, gdy wystąpi określone zdarzenie monitorowania kondycji.

Zaktualizujmy przeglądy książki konfiguracji witryny sieci Web tak, aby firma Microsoft otrzyma wiadomość e-mail, gdy wyjątek występuje. W tym celu należy wykonać trzy zadania:

1. Konfigurowanie aplikacji sieci web ASP.NET do wysyłania wiadomości e-mail. Jest to realizowane przez określenie, jak wiadomości e-mail są wysyłane za pośrednictwem `<system.net>` element konfiguracji. Więcej informacji na temat wysyłania wiadomości w aplikacji ASP.NET można znaleźć [wysyłania wiadomości E-mail w programie ASP.NET:](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) i [System.Net.Mail — często zadawane pytania](http://systemnetmail.com/).
2. Zarejestruj dostawcę poczty e-mail dziennika źródła w `<providers>` elementu, a
3. Dodaj wpis do `<rules>` element, który mapuje zdarzeń "Wszystkie błędy" dostawca źródło dziennika dodanej w kroku (2).

Kondycji systemu monitorowania zawiera dwie klasy dostawcy źródła wiadomości e-mail dziennika: `SimpleMailWebEventProvider` i `TemplatedMailWebEventProvider`. [ `SimpleMailWebEventProvider` Klasy](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx) wysyła wiadomości e-mail w formacie zwykłego tekstu, która zawiera zdarzenia szczegółowych informacji i umożliwia dostosowanie mało treść wiadomości e-mail. Za pomocą [ `TemplatedMailWebEventProvider` klasy](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) określić strony ASP.NET, w których renderowanego kodu znaczników jest używana jako treść wiadomości e-mail. [ `TemplatedMailWebEventProvider` Klasy](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) zapewnia znacznie większą kontrolę nad zawartość i format wiadomości e-mail, ale wymaga nieco więcej pracy ponoszonych z góry, jak należy utworzyć strony ASP.NET, która generuje treść wiadomości e-mail. Ten samouczek koncentruje się na temat korzystania z `SimpleMailWebEventProvider` klasy.

Aktualizuj monitorowania systemu kondycji `<providers>` element `Web.config` pliku, aby uwzględnić źródło dziennika `SimpleMailWebEventProvider` klasy:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample3.xml)]

Używa znaczników powyżej `SimpleMailWebEventProvider` klasy jako dostawcę dziennika źródła i przypisuje mu przyjazna nazwa "EmailWebEventProvider". Ponadto `<add>` atrybut zawiera dodatkowe opcje konfiguracji, takich jak na i z adresów e-mail.

Przy użyciu źródła dziennika wiadomości e-mail zdefiniowane pozostaje nakazać kondycji monitorowania systemu, aby używać tego źródła "rejestrowania" nieobsługiwanych wyjątków. Jest to osiągane przez dodanie nowej reguły w `<rules>` sekcji:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample4.xml)]

`<rules>` Sekcja zawiera teraz dwie reguły. Pierwszy z nich, o nazwie "Wszystkie błędy do wiadomości E-mail", wysyła wszystkie nieobsłużone wyjątki źródło dziennika "EmailWebEventProvider". Ta zasada obowiązuje wysłać szczegółowe informacje o błędach w witrynie internetowej do określonego adresu. Zasada "Wszystkie błędy w bazie danych" rejestruje szczegóły błędu w bazie danych lokacji. W związku z tym po zmianie nieobsługiwany wyjątek w witrynie jego szczegóły są obie rejestrowane w bazie danych i wysłany na określony adres e-mail.

**Rysunek 2** pokazuje wiadomości e-mail generowanych przez `SimpleMailWebEventProvider` klasy podczas odwiedzania `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image4.png)

**Rysunek 2**: szczegóły błędu są wysyłane w wiadomościach E-mail  
([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](logging-error-details-with-asp-net-health-monitoring-vb/_static/image6.png))

## <a name="summary"></a>Podsumowanie

System monitorowania kondycji ASP.NET została zaprojektowana do umożliwiają administratorom monitorowanie kondycji wdrożoną aplikacją internetową. Zdarzenia monitorowania kondycji są wywoływane, gdy ujawniać pewnych działań, np. po zatrzymaniu aplikacji, jeśli pomyślnie zalogował się użytkownik witryny lub po wystąpieniu nieobsługiwanego wyjątku. Te zdarzenia mogą być rejestrowane do dowolnej liczby źródła dzienników. W tym samouczku pokazano, jak rejestrować szczegóły nieobsługiwanych wyjątków z bazą danych i za pośrednictwem wiadomości e-mail.

Ten samouczek koncentruje się na korzystanie z programu health monitorowania do rejestrowania nieobsługiwanych wyjątków, ale należy pamiętać, że monitorowanie kondycji służy do mierzenia ogólną kondycję wdrożonej aplikacji ASP.NET i zawiera wiele zdarzeń monitorowania kondycji i nie rejestrować źródła przedstawione tutaj. Co to jest więcej, możesz utworzyć własne kondycji, monitorowanie zdarzeń i dziennika źródła, jeśli będzie to potrzebne wystąpić. Jeśli chcesz dowiedzieć się więcej informacji na temat monitorowania kondycji, dobrze jest najpierw do przeczytania przez czytelników [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)firmy [często zadawane pytania dotyczące monitorowania kondycji](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). Poniżej, zapoznaj się z [How to: Użyj monitorowanie kondycji w programie ASP.NET 2.0](https://msdn.microsoft.com/library/ms998306.aspx).

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Omówienie monitorowania kondycji ASP.NET](https://msdn.microsoft.com/library/bb398933.aspx)
- [Konfigurowanie i dostosowywanie monitorowania systemu ASP.NET kondycji](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [Często zadawane pytania — monitorowanie kondycji w programie ASP.NET 2.0](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Instrukcje: Wysyłanie wiadomości E-Mail dla powiadomień dotyczących monitorowania kondycji](https://msdn.microsoft.com/library/ms227553.aspx)
- [Instrukcje: Używanie monitorowanie kondycji w programie ASP.NET:](https://msdn.microsoft.com/library/ms998306.aspx)
- [Monitorowanie kondycji w programie ASP.NET](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [Poprzednie](processing-unhandled-exceptions-vb.md)
> [dalej](logging-error-details-with-elmah-vb.md)
