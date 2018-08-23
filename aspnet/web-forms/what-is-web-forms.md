---
uid: web-forms/what-is-web-forms
title: Co to jest formularzy sieci Web | Dokumentacja firmy Microsoft
author: rick-anderson
description: ''
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: ccb0e6096b0281e5ef8af63bfd84b172e7f209b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754235"
---
<a name="what-is-web-forms"></a>Co to jest formularzy sieci Web
====================
ASP.NET Web Forms jest częścią struktury aplikacji sieci web programu ASP.NET i jest dołączany [programu Visual Studio](https://www.asp.net/downloads). Jest to jeden z czterech modeli programowania służących do tworzenia aplikacji sieci web ASP.NET, pozostałe wersje to ASP.NET MVC, ASP.NET Web Pages i aplikacje jednostronicowe ASP.NET.

Formularze sieci Web są stron żądających użytkowników za pomocą przeglądarki. Te strony mogą być napisane przy użyciu kombinacji HTML, skrypt klienta, formanty serwera i kod serwera. Użytkownicy żądania strony, jest skompilowany i wykonany na serwerze przez platformę, a następnie generuje kod znaczników HTML, przeglądarka umożliwiający renderowanie w ramach. Na stronie ASP.NET Web Forms przedstawia informacje użytkownika w dowolnej przeglądarce lub urządzenie klienckie.

W programie Visual Studio można utworzyć formularzy sieci Web ASP.NET. Visual Studio rozwoju środowiska IDE (Integrated) umożliwia przeciąganie i upuszczanie formantów serwera do tworzenia układu strony formularzy sieci Web. Następnie można łatwo ustawić właściwości, metody i zdarzenia dla formantów na stronie lub dane strony. Te właściwości, metody i zdarzenia są używane do definiowania zachowania strony sieci web, wygląd i działanie i tak dalej. Aby napisać kod serwera do obsługi logiki dla strony, można użyć języka .NET, takich jak Visual Basic lub C#.

> [!NOTE] 
> 
> Dokumentacja platformy ASP.NET i programu Visual Studio obejmuje różne wersje. Tematy, przedstawiających funkcje z poprzednich wersji może być przydatne dla bieżącego zadania i scenariuszy przy użyciu najnowszej wersji.


**ASP.NET Web Forms są:** 

- Oparte na technologii Microsoft ASP.NET, w którym kod, który działa na serwerze dynamicznie generuje danych wyjściowych strony sieci Web do przeglądarki lub klienta urządzenia.
- Zgodny z dowolnej przeglądarki lub urządzenia przenośnego. Na stronie sieci Web platformy ASP.NET automatycznie renderuje poprawne zgodne z przeglądarki HTML, dla funkcji, takich jak style, układ i tak dalej.
- Zgodne z dowolnym językiem, obsługiwane przez .NET środowisko uruchomieniowe języka wspólnego, takich jak Microsoft Visual Basic i Microsoft Visual C#.
- Oparta na Microsoft .NET Framework. Dzięki temu wszystkie zalety framework, w tym środowisku zarządzanym, bezpieczeństwo typów i dziedziczenia.
- Elastyczne, ponieważ można dodać utworzone przez użytkownika, a do ich formanty innych firm.

**ASP.NET Web Forms oferty:** 

- Rozdzielenie kodu HTML i innego kodu interfejsu użytkownika z aplikacji logiki.
- Bogaty zestaw formantów serwera do wykonywania typowych zadań, w tym dostęp do danych.
- Zaawansowane wiązania danych, z obsługą doskonałe narzędzie.
- Obsługa skryptów po stronie klienta, który jest wykonywany w przeglądarce.
- Obsługa różnych inne możliwości, takie jak routing, bezpieczeństwo, wydajność, internacjonalizacji, testowanie, debugowanie, obsługa błędów, zarządzania stanem.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>ASP.NET Web Forms umożliwia pokonanie trudności

Programowanie aplikacji sieci Web Wyświetla wyzwania, które zwykle nie wystąpić podczas programowania tradycyjne aplikacje oparte na kliencie. Jednym z wyzwań związanych są:

- **Implementowanie bogaty interfejs użytkownika sieci Web** — może być trudne i żmudnym do projektowania i implementacji użytkownika interfejs, za pomocą podstawowe ułatwienia HTML, zwłaszcza, jeśli strona ma układ złożony dużej ilości zawartości dynamicznej i w pełni funkcjonalne obiekty interakcji z użytkownikiem.
- **Rozdzielenie klienta i serwera** -aplikacji w sieci Web, klienta (przeglądarki) i serwera są różne programy, często uruchomione na różnych komputerach (a nawet w różnych systemach operacyjnych). W związku z tym dwie części aplikacji udostępnianie bardzo mało informacji. one komunikować się, ale zazwyczaj programu exchange tylko małych fragmentów informacji proste.
- **Wykonanie o bezstanowa** — w przypadku serwera sieci Web odbiera żądanie dla strony sieci go znajdzie strony, przetwarza je, wysyła go do przeglądarki i odrzuca wszystkie informacje ze strony. Jeśli użytkownik zażąda tej samej stronie ponownie, serwer powtarza całą sekwencję, ponownego przetworzenia strony od podstaw. Innymi słowy, serwer ma Brak pamięci stron, które zostało przetworzone — strony są bezstanowe. W związku z tym jeśli aplikacja musi przechowywać informacje o stronie, natury bezstanowe może stać się problemem.
- **Możliwości klienta nieznany** — w wielu przypadkach aplikacje sieci Web są dostępne dla wielu użytkowników za pomocą różnych przeglądarek. Przeglądarki mają różne możliwości, co utrudnia do tworzenia aplikacji, która będzie działać równie dobrze na wszystkich z nich.
- **Kompilacji z dostępem do danych** -odczytywanie ich z niego i pisanie ze źródłem danych, w tradycyjnej aplikacji sieci Web może być skomplikowane i intensywnie korzystających z zasobów.
- **Kompilacji ze skalowalnością** — w wielu przypadkach aplikacje sieci Web z istniejących metod nie można zrealizować cele skalowalności ze względu na brak zgodności między składnikami aplikacji. Jest to często wspólnego punktu awarii dla aplikacji w ramach cyklu rozwoju duże.

Spełniające te wyzwania dla aplikacji sieci Web może wymagać sporo czasu i wysiłku. ASP.NET Web Forms i ASP.NET framework te wyzwania w następujący sposób:

- **Model obiektowy intuicyjne, spójne** — architektura strony ASP.NET przedstawia model obiektów, umożliwiające myśleć o formularzach jako jednostki, a nie jako osobne fragmenty klienta i serwera. W tym modelu można programować strony w bardziej intuicyjny sposób niż w tradycyjnej aplikacji sieci Web, w tym możliwość ustawiania właściwości dla elementów strony i reagowania na zdarzenia. Ponadto formanty serwera ASP.NET są abstrakcję fizycznego zawartość strony HTML i możliwości bezpośredniej interakcji między przeglądarką i serwerem. Ogólnie rzecz biorąc można użyć kontrolek serwera może Praca z kontrolkami w aplikacji klienckiej i nie trzeba myśleć o sposobie tworzenia kodu HTML, umożliwiające zaprezentowanie i przetworzyć kontrolki i ich zawartość.
- **Model programowania sterowanego** — ASP.NET Web Forms doprowadzić do aplikacji sieci Web, znanego modelu zapisu programy obsługi zdarzeń dla zdarzenia występujące na kliencie ani serwerze. Architektura strony ASP.NET przenosi tego modelu w taki sposób, że podstawowy mechanizm przechwytywania zdarzeń na komputerze klienckim, przesyłania ich do serwera i wywołanie odpowiedniej metody jest wszystkie automatyczne i niewidoczne dla użytkownika. Wynik jest to struktura wyraźne i łatwe napisane kodu, która obsługuje programowanie oparte na zdarzeniach.
- **Zarządzanie stanem intuicyjne** — architektura strony ASP.NET automatycznie obsługuje zadanie obsługi stanu strony i jego formantów i umożliwia jawne sposoby zarządzania stanem informacji specyficznych dla aplikacji. To odbywa się bez intensywnie korzysta z zasobów serwera i można ją wdrożyć z lub bez wysyłania plików cookie w przeglądarce.
- **Aplikacje niezależnie od przeglądarki** — architektura strony ASP.NET umożliwia utworzenie całą logikę aplikacji na serwerze, co eliminuje konieczność jawnego kodu pod względem różnic w przeglądarkach. Jednak nadal umożliwia korzystanie z zalet funkcji specyficznych dla przeglądarki przez napisanie kodu po stronie klienta, aby zapewnić lepszą wydajność i bardziej zaawansowane środowisko klienta.
- **Obsługa środowiska uruchomieniowego języka wspólnego .NET framework** — architektura strony ASP.NET działa w oparciu programu .NET Framework, dzięki czemu całej struktury framework jest dostępna dla dowolnej aplikacji platformy ASP.NET. Można napisać swoje aplikacje w dowolnym języku, który jest zgodny, który jest w środowisku uruchomieniowym. Ponadto dostęp do danych jest uproszczone, przy użyciu infrastruktury dostępu do danych dostarczone przez .NET Framework, w tym ADO.NET.
- **.NET framework skalowalnego serwera wydajności** — architektura strony ASP.NET umożliwia skalowanie aplikacji sieci Web z jednego komputera z jednym procesorem do farmy internetowej wielu komputerów nie pozostawia żadnych śladów i bez skomplikowane zmian w aplikacji Logika.

## <a name="features-of-aspnet-web-forms"></a>Funkcje wzorca ASP.NET Web Forms

- **Formanty serwera**— formanty serwera sieci Web platformy ASP.NET to obiekty stron ASP.NET Web pages, uruchamiane po żądaniu strony i że znaczników renderowania w przeglądarce. Wiele formantów serwera sieci Web są podobne do znanych elementów HTML, takie jak przyciski i pola tekstowe. Inne formanty obejmują zachowanie złożonych, takie jak kalendarz kontrolki i formantów, które można użyć do nawiązywania połączeń ze źródłami danych i wyświetlania danych.
- **Strony wzorcowe**-stron wzorcowych platformy ASP.NET umożliwiają tworzenie spójnego układu dla strony w aplikacji. Jednej strony wzorcowej definiuje wygląd i zachowanie standardowe dla wszystkich stron (lub grupy stron) w aplikacji. Następnie można utworzyć poszczególnych stron zawartości, które zawierają zawartość, którą chcesz wyświetlić. Jeśli użytkownicy zażądają zawartości stron, scalają ze stroną wzorcową w celu wygenerowania danych wyjściowych, który łączy układ strony wzorcowej z zawartości z poziomu strony zawartości.
- **Praca z danymi**— ASP.NET udostępnia wiele opcji przechowywanie, pobieranie i wyświetlanie danych. W aplikacji formularzy sieci Web ASP.NET umożliwia formantów powiązanych z danymi zautomatyzować prezentacji lub wprowadzania danych w elementach interfejsu użytkownika strony sieci web, takie jak tabele i pola tekstowe i list rozwijanych.
- **Członkostwo**-produktu ASP.NET Identity przechowuje poświadczenia użytkownika w bazie danych utworzonych przez aplikację. Podczas logowania się użytkowników aplikacji sprawdza poprawność poświadczeń, zapoznając się bazy danych. Projektu *konta* folder zawiera pliki, które implementują różne części członkostwa: rejestracji, logowania, zmiana haseł i autoryzowanie dostępu. Ponadto formularzy sieci Web programu ASP.NET obsługuje OAuth i OpenID. Te ulepszenia uwierzytelniania zezwolić użytkownikom na logowanie się do witryny przy użyciu bieżących poświadczeń z konta, takie jak Facebook, Twitter, Windows Live i Google. Domyślnie ten szablon tworzy bazy danych członkostwa przy użyciu domyślnej nazwy bazy danych w wystąpieniu programu SQL Server Express LocalDB, serwer bazy danych rozwoju, dostarczanego z Visual Studio Express 2013 for Web.
- **Skrypt po stronie klienta i struktur klienta**— funkcje oparte na serwerze programu ASP.NET można zwiększyć przez dołączenie funkcji skryptu klienta na stronach formularz sieci Web ASP.NET. Skrypt po stronie klienta można użyć w celu zapewnienia użytkownikom bardziej rozbudowane, zwiększyć szybkość reakcji interfejs użytkownika. Skrypt po stronie klienta umożliwia również wywołań asynchronicznych do serwera sieci Web uruchomionej strony w przeglądarce.
- **Routing**-routingu adresów URL pozwala skonfigurować aplikację tak, aby akceptował żądania z adresów URL, które nie są mapowane na pliki fizyczne. Adres URL żądania jest po prostu adres URL, które są podawane w przeglądarce, aby znaleźć stronę w witrynie sieci web. Używasz routingu do definiowania adresów URL, które są semantycznie zrozumiałe dla użytkowników i które może pomóc w optymalizacji dla aparatów wyszukiwania (SEO).
- **Stan zarządzania**-formularzy sieci Web ASP.NET zawiera kilka opcji, które pomagają zachować dane na podstawie na stronę i całej aplikacji.
- **Zabezpieczenia**-ważnym elementem projektowania bardziej bezpiecznych aplikacji jest zrozumienie zagrożenia do niego. Firma Microsoft opracowała sposób automatycznej klasyfikacji zagrożeń: fałszowania, modyfikowaniu, możliwość wyparcia, ujawnienie informacji, odmowę usługi i podniesienie uprawnień (krok). W formularzach sieci Web platformy ASP.NET możesz dodać punkty rozszerzeń i opcje konfiguracji, które umożliwiają dostosowanie różne zachowania zabezpieczeń w formularzach sieci Web platformy ASP.NET.
- **Wydajność**— wydajność może odbiegać kluczowym czynnikiem w witrynę sieci Web lub projektu. ASP.NET Web Forms umożliwia modyfikowanie wydajności związanych z strony i przetwarzanie na serwerze kontroli, zarządzanie stanem, dostęp do danych, konfigurację aplikacji i ładowanie i wydajny, kodowania.
- **Internacjonalizacja**— formularzy sieci Web ASP.NET umożliwia utworzenie strony sieci web, które mogą uzyskać zawartości i inne dane na podstawie ustawienia preferowany język przeglądarki lub oparte na jawne wybrany przez użytkownika języka. Zawartość i innych danych jest określany jako zasoby, i takie dane mogą być przechowywane w plikach zasobów lub innych źródeł. Na stronie ASP.NET Web Forms jednak konfigurujesz kontrolki, można pobrać wartości właściwości z zasobów. W czasie wykonywania wyrażenia zasobów są zastępowane przez zasoby z pliku odpowiednią zlokalizowanych zasobów.
- **Debugowanie i obsługa błędów**— ASP.NET zawiera funkcje, dzięki którym możesz diagnozować problemy, które mogą wystąpić w aplikacji formularzy sieci Web. Debugowanie i obsługa błędów są również obsługiwane w ramach formularzy sieci Web ASP.NET tak, aby Twoje aplikacje kompilacji i pracować wydajnie.
- **Wdrażanie i Hosting**— Visual Studio, platformy ASP.NET, Azure i usług IIS zapewnia narzędzia pomagające w ramach procesu wdrażania i hostowanie aplikacji formularzy sieci Web.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Podejmowanie decyzji o utworzeniu aplikacji formularzy sieci Web

Należy starannie rozważyć czy wdrożyć aplikację sieci Web za pomocą sieci Web platformy ASP.NET stanowi modelu lub innego modelu, na przykład platforma ASP.NET MVC. Struktura MVC nie zastępuje modelu formularzy sieci Web; dla aplikacji sieci Web, można użyć obu struktur. Zanim zdecydujesz się użyć modelu formularzy sieci Web lub struktura MVC dla określonej witryny sieci Web, należy rozważyć zalety każde podejście.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Zalety aplikacji sieci Web opartych na formularzach sieci Web

Struktura oparta na formularzach sieci Web ma następujące zalety:

- Obsługuje model zdarzeń zachowujący stan przez HTTP, co opracowywanie aplikacji sieci Web line-of-business. Aplikacja oparta na formularzach sieci Web udostępnia dziesiątki zdarzeń, które są obsługiwane przez setki kontrolek serwerowych.
- Używa wzorca kontrolera strony, który dodaje funkcje do pojedynczych stron. Aby uzyskać więcej informacji, zobacz [kontrolera strony](https://go.microsoft.com/fwlink/?LinkId=106359 "kontrolera strony") w witrynie MSDN w sieci Web.
- Używa stanu widoku ani formularzy opartych na serwer, które ułatwia zarządzanie informacjami o stanie.
- Sprawdza się w przypadku małych zespołów deweloperów i projektantów, którzy chcą korzystać z zalet dużej liczby składników dostępnych do szybkiego opracowywania aplikacji internetowych.
- Ogólnie rzecz biorąc, jest mniej złożona przy projektowaniu aplikacji, ponieważ składniki ( **strony** klasy, formantów i tak dalej) są ściśle powiązane i zwykle wymagają mniejszej ilości kodu niż w modelu MVC.

### <a name="advantages-of-an-mvc-based-web-application"></a>Zalety aplikacji sieci Web opartej na MVC

Platforma ASP.NET MVC ma następujące zalety:

- Ułatwia ona zarządzanie złożonością, dzieląc aplikację na model, widok i kontroler.
- Nie używa stanu widoku ani formularzy opartych na serwerze. To sprawia, że struktura MVC idealne rozwiązanie dla deweloperów, którzy mają pełną kontrolę nad działaniem aplikacji.
- Używa wzorca kontrolera Frontowego, który przetwarza żądania aplikacji sieci Web za pośrednictwem jednego kontrolera. Dzięki temu można zaprojektować aplikację, która obsługuje złożoną infrastrukturę routingu. Aby uzyskać więcej informacji, zobacz [kontroler Frontowy](https://go.microsoft.com/fwlink/?LinkId=106357 "kontroler Frontowy") w witrynie MSDN w sieci Web.
- Zapewnia lepszą obsługę projektowania opartego na testach (TDD).
- Sprawdza się w przypadku aplikacji sieci Web, które są obsługiwane przez duże zespoły deweloperów i projektantów witryn sieci Web, którzy potrzebują wysokiego stopnia kontroli nad działaniem aplikacji.
