---
uid: web-forms/what-is-web-forms
title: Co to jest formularzy sieci Web | Dokumentacja firmy Microsoft
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: dcaba60a8640ce83f73460a5c8ee457257b9397e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="what-is-web-forms"></a>Co to jest formularzy sieci Web
====================
Formularze sieci Web ASP.NET jest częścią struktury aplikacji sieci web programu ASP.NET i jest dołączony do [programu Visual Studio](https://www.asp.net/downloads). Jest jednym z czterech modeli programowania, używanego do tworzenia aplikacji sieci web ASP.NET, są inne platformy ASP.NET MVC, ASP.NET Web Pages i aplikacji jednej strony ASP.NET.

Formularze sieci Web są stron, które użytkownicy żądania przy użyciu przeglądarki. Te strony można pisać przy użyciu kombinacji kodu HTML, skrypt klienta, serwera formanty i kod serwera. Gdy użytkownik zażąda strony, kompilacji i wykonywane na serwerze przez platformę, a następnie generuje kod znaczników HTML, umożliwiający renderowanie przeglądarki w ramach. Strony formularzy sieci Web ASP.NET wyświetlane informacje użytkownika w dowolnej przeglądarce lub urządzenie klienckie.

Za pomocą programu Visual Studio, można utworzyć formularzy sieci Web ASP.NET. Visual Studio programowanie środowiska IDE (Integrated) umożliwia przeciągania i upuszczania kontrolki serwera do tworzenia układu strony formularzy sieci Web. Następnie można łatwo ustawić właściwości, metod i zdarzeń dla formantów na stronie lub innych poleceń strony. Te właściwości, metody i zdarzenia są używane do definiowania zachowania strony sieci web, wyglądu i działania i tak dalej. Aby napisać kod serwera do obsługi logiki strony, można użyć języka .NET, takie jak Visual Basic lub C#.

> [!NOTE] 
> 
> Dokumentacja programu ASP.NET i Visual Studio obejmuje kilka wersji. Tematy dotyczące funkcji z poprzednich wersji mogą być przydatne dla bieżącego zadania i scenariusze korzystania z najnowszej wersji.


**Formularze sieci Web platformy ASP.NET są:** 

- Oparte na technologii Microsoft ASP.NET, w którym kod, który działa na serwerze dynamicznie generuje dane wyjściowe strony sieci Web na urządzeniu klienta lub dotyczących przeglądarki.
- Zgodny z dowolnej przeglądarki lub urządzeniu przenośnym. Na stronie sieci Web ASP.NET automatycznie renderuje poprawne zgodny z przeglądarki HTML dla funkcji, takich jak style, układu i tak dalej.
- Zgodny z dowolnym języków obsługiwanych przez .NET środowisko uruchomieniowe języka wspólnego, takich jak Microsoft Visual Basic i Microsoft Visual C#.
- Oparty na programie Microsoft .NET Framework. Zapewnia to wszystkie zalety platformy, łącznie z zarządzanego środowiska, zabezpieczeń i dziedziczenia.
- Elastyczne, ponieważ można dodać utworzonych przez użytkownika i innych firm kontrolki do nich.

**Oferta formularzy sieci Web programu ASP.NET:** 

- Rozdzielenie HTML i innego kodu interfejsu użytkownika aplikacji logiki.
- Bogaty zestaw kontrolek serwera dla typowych zadań, takich jak dostęp do danych.
- Powiązanie z obsługą doskonałe narzędzie zaawansowanych danych.
- Obsługa skryptów po stronie klienta, który wykonuje w przeglądarce.
- Obsługa różnych innych funkcji, łącznie z routing, bezpieczeństwo, wydajność, przystosowywanie do warunków międzynarodowych, testowania, debugowania, obsługa błędów, zarządzanie stanem.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>Formularze sieci Web ASP.NET pomaga rozwiązać problemy

Programowanie aplikacji sieci Web Wyświetla wyzwania, które zwykle nie pojawiać programowania tradycyjne aplikacje oparte na kliencie. Jednym z wyzwań związanych są:

- **Wdrażanie zaawansowanych interfejs użytkownika sieci Web** — może być trudne i niewygodne projektowania i implementacji użytkownika interfejsu za pomocą podstawowych funkcji HTML, zwłaszcza, jeśli strona ma złożonego układu dużą ilością zawartości dynamicznej i kompletne obiekty interakcji z użytkownikiem.
- **Rozdzielenie klienta i serwera** -aplikacji w sieci Web klienta (przeglądarki) i serwera są różnych programów często na różnych komputerach (a nawet w różnych systemach operacyjnych). W rezultacie dwie połowy aplikacji udostępnianie bardzo niewielka ilość informacji; one komunikować się, ale zazwyczaj wymiany tylko małych fragmentów informacji proste.
- **Wykonanie bezstanowych** — w przypadku serwera sieci Web otrzymuje żądanie strony, go znajduje stronę, procesy wysyła go do przeglądarki i odrzuca wszystkie informacje ze strony. Jeśli użytkownik ponownie żąda tej samej stronie, serwer powtarza całą sekwencję ponowne przetwarzanie strony, od początku. Innymi słowy, serwer ma Brak pamięci stron, które został on przetworzony — strony są bezstanowe. W związku z tym jeśli aplikacja musi zachować informacje o stronie, natury bezstanowych może stać się problemem.
- **Możliwości klienta nieznany** — w wielu przypadkach są dostępne dla wielu użytkowników za pomocą różnych przeglądarkach aplikacji sieci Web. Przeglądarki mają różne możliwości, utrudniając do tworzenia aplikacji, która będzie działać równie również na wszystkich z nich.
- **Komplikacji z dostępem do danych** -odczytu z oraz zapisywanie do źródła danych w tradycyjnych aplikacji sieci Web może być skomplikowane i użyciem zasobów.
- **Komplikacji z skalowalność** — w wielu przypadkach aplikacji sieci Web z istniejących metod, które nie spełniają skalowalności z powodu braku zgodności między różnymi składnikami aplikacji. Jest to często wspólnego punktu awarii dla aplikacji w cyklu wzrostu duże.

Spełniające te problemy dotyczące aplikacji sieci Web może wymagać sporo czasu i wysiłku. Formularze sieci Web ASP.NET i platforma ASP.NET te wyzwania w następujący sposób:

- **Model obiektów intuicyjny, spójne** -architekturę stron programu ASP.NET przedstawia model obiektów, umożliwiający traktować formularzy jako jednostki, a nie jako osobne fragmenty klienta i serwera. W tym modelu można programu strony w sposób bardziej intuicyjne, niż tradycyjne aplikacje sieci Web, łącznie z możliwością reagowania na zdarzenia i ustawić właściwości dla elementów strony. Ponadto kontrolek serwera ASP.NET są abstrakcję z fizycznego zawartość strony HTML i bezpośredniej interakcji między przeglądarką i serwerem. Ogólnie rzecz biorąc można użyć kontrolki serwera sposób może pracować z formantów w aplikacji klienckiej i nie muszą pamiętać o tworzeniu HTML w celu przedstawienia i przetworzyć formantów i ich zawartość.
- **Model programowania sterowane zdarzeniami** -formularzy sieci Web ASP.NET doprowadzić do aplikacji sieci Web znanego modelu zapisywania programy obsługi zdarzeń dla zdarzenia występujące na klienta lub serwera. Struktura strony ASP.NET abstracts tego modelu w taki sposób, że podstawowy mechanizm Przechwytywanie zdarzeń na kliencie, przesyłania ich do serwera i wywołanie metody odpowiednie wszystkich automatyczne i niewidoczne. Wynik jest strukturą Wyczyść, łatwo napisane kodu, która obsługuje programowanie sterowane zdarzeniami.
- **Zarządzanie stanem intuicyjne** — architekturę stron programu ASP.NET automatycznie obsługuje zadanie obsługi stan strony i jego formantów i umożliwia jawne sposoby zarządzania stanem informacje specyficzne dla aplikacji. To odbywa się bez duże wykorzystanie zasobów serwera i można ją wdrożyć z lub bez wysyłania plików cookie w przeglądarce.
- **Aplikacje przeglądarki niezależny** -architekturę stron programu ASP.NET można tworzyć całą logikę aplikacji na serwerze, co eliminuje konieczność jawnie kod różnice w przeglądarkach. Jednak nadal umożliwia korzystanie z funkcji specyficznych dla przeglądarki dzięki pisanie kodu po stronie klienta, aby zapewnić lepszą wydajność i bardziej rozbudowane środowisko klienta.
- **Obsługa środowiska uruchomieniowego CLR platformy .NET framework** -architekturę stron programu ASP.NET jest oparty na programie .NET Framework, dlatego cały framework jest dostępna dla wszystkich aplikacji ASP.NET. Aplikacje można pisać w dowolnym języku, który jest zgodny z środowiska uruchomieniowego. Ponadto dostęp do danych jest uproszczone, przy użyciu danych access infrastrukturę programu .NET Framework, w tym ADO.NET.
- **Wydajność serwera skalowalne .NET framework** -architekturę stron programu ASP.NET umożliwia skalowanie aplikacji sieci Web z jednego komputera z jednym procesorem do farmy sieci Web złożonym z wielu komputerów prawidłowo i bez skomplikowane zmian w aplikacji Logika.

## <a name="features-of-aspnet-web-forms"></a>Funkcje formularzy sieci Web ASP.NET

- **Formanty serwera**-kontrolki serwera sieci Web ASP.NET są obiektami stron ASP.NET Web pages systemem, gdy strona jest pobierana i że znaczników renderowania w przeglądarce. Wiele kontrolki serwera sieci Web są podobne do znanych elementów HTML, takie jak przycisków i pola tekstowe. Inne formanty obejmować zachowanie złożonych, takie jak kalendarz kontrolek i formanty używane do łączenia się ze źródłami danych i wyświetlania danych.
- **Strony wzorcowe**-stron wzorcowych ASP.NET umożliwiają tworzenie spójnego układu dla strony w aplikacji. Jednej strony wzorcowej definiuje wygląd i działanie i standardowe zachowanie dla wszystkich stron (lub grupę stron) w aplikacji. Następnie możesz utworzyć poszczególne strony zawartości, które zawierają zawartość, którą chcesz wyświetlić. Gdy użytkownik zażąda strony z zawartością, je połączyć ze stroną wzorcową do generowania danych wyjściowych, łączącą układ strony wzorcowej z zawartością ze strony zawartość.
- **Praca z danymi**— program ASP.NET udostępnia wiele opcji przechowywanie, pobieranie i wyświetlanie danych. W aplikacji formularzy sieci Web ASP.NET formanty powiązane z danymi służą do automatyzacji prezentacji lub wprowadzania danych w elementów interfejsu użytkownika strony sieci web, takich jak tabele i pola tekstowe i list rozwijanych.
- **Członkostwo**-tożsamości ASP.NET przechowuje poświadczenia użytkownika w bazie danych utworzonego przez aplikację. Gdy użytkownicy zalogują się, aplikacji sprawdza poprawność poświadczeń za Odczytywanie bazy danych. Projektu *konta* folder zawiera pliki, które implementuje różne części członkostwa: rejestracji, logowania, zmiana haseł i autoryzowania dostępu. Ponadto formularzy sieci Web ASP.NET obsługuje OAuth i OpenID. Te ulepszenia uwierzytelniania zezwolić użytkownikom na logowanie się do witryny przy użyciu istniejących poświadczeń z konta, takich jak Facebook, Twitter, Windows Live i Google. Domyślnie szablon tworzy bazę danych członkostwa przy użyciu domyślnej nazwy bazy danych w wystąpieniu programu SQL Server Express LocalDB, serwer bazy danych programowanie dołączoną do programu Visual Studio Express 2013 for Web.
- **Skrypt po stronie klienta i platform klienta**— funkcje na serwerze programu ASP.NET można zwiększyć, włączając funkcje skryptu klienta na stronach formularzy sieci Web ASP.NET. Skrypt po stronie klienta umożliwia udostępniają interfejs użytkownika bardziej rozbudowane, bardziej odpowiednie dla użytkowników. Umożliwia także skrypt po stronie klienta do asynchronicznego połączeń z serwerem sieci Web uruchomionej stronę w przeglądarce.
- **Routing**-routingu adresów URL umożliwia konfigurowanie aplikacji do akceptowania żądań adresów URL, które nie są mapowane na pliki fizyczne. Adres URL żądania jest po prostu adres URL, które użytkownik wprowadzi w przeglądarce, aby znaleźć strony w witrynie sieci web. Aby zdefiniować adresy URL, które są semantycznie zrozumiały dla użytkowników i które mogą pomóc z optymalizacji dla aparatów wyszukiwania (SEO) używać routingu.
- **Stan zarządzania**-formularzy sieci Web ASP.NET zawiera kilka opcji, które pomagają zachować dane na poziomie aplikacji i bazując na stronie.
- **Zabezpieczenia**-ważnym elementem projektowania aplikacji bezpieczniejsze jest zrozumienie zagrożeń do niego. Firma Microsoft opracowała sposobem kategoryzowanie zagrożeń: podszywanie się, naruszanie zawartości, możliwość wyparcia, ujawnienie informacji, odmowę usługi i podniesienie uprawnień (krok). W formularzach sieci Web ASP.NET można dodać punkty rozszerzeń i opcje konfiguracji, które umożliwiają dostosowanie różne zachowania zabezpieczeń w formularzach sieci Web ASP.NET.
- **Wydajność**-wydajność może mieć kluczowe znaczenie witrynę sieci Web lub projektu. Formularze sieci Web ASP.NET umożliwia modyfikowanie wydajności związanych z strony i przetwarzanie na serwerze kontroli, zarządzanie stanem, dostęp do danych konfiguracji aplikacji i ładowanie i wydajne rozwiązania w zakresie kodowania.
- **Przystosowywanie do warunków międzynarodowych**— formularzy sieci Web ASP.NET umożliwia tworzenie zawartości strony sieci web, które można uzyskać i innych danych na podstawie ustawienia preferowany język przeglądarki lub na podstawie użytkownika jawne wyboru języka. Zawartość i innych danych odnosi się do zasobów i takie dane mogą być przechowywane w plikach zasobu lub innych źródeł. Na stronie składnika ASP.NET Web Forms możesz skonfigurować kontrolki można pobrać wartości właściwości z zasobów. W czasie wykonywania wyrażenia zasobów zostały zastąpione przez zasoby z pliku odpowiednie zlokalizowanych zasobów.
- **Debugowanie i obsługi błędu**— program ASP.NET zawiera funkcje ułatwiające diagnozowanie problemów, które mogą wystąpić w aplikacji formularzy sieci Web. Debugowania i obsługa błędów również są obsługiwane w formularzach sieci Web ASP.NET, dzięki czemu aplikacje skompilować i uruchomić skutecznie.
- **Wdrażanie i hostingu**— Visual Studio, ASP.NET, Azure i usługi IIS zapewniają narzędzia, które zapewniają pomoc podczas procesu wdrażania i obsługę aplikacji formularzy sieci Web.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Podejmowanie decyzji o utworzeniu aplikacji formularzy sieci Web

Należy starannie rozważyć czy do wdrożenia aplikacji sieci Web za pomocą sieci Web platformy ASP.NET stanowi modelu lub inny model, takich jak platforma ASP.NET MVC. Struktura MVC nie zastępuje modelu formularzy sieci Web; można użyć obu struktur dla aplikacji sieci Web. Przed podjęciem decyzji użyć modelu formularzy sieci Web i struktura MVC dla określonej witryny sieci Web należy porównać zalety każdego z podejść.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Zalety aplikacji sieci Web opartych na formularzach sieci Web

Struktura oparta na formularzach sieci Web ma następujące zalety:

- Obsługuje ona model zdarzeń, która zachowuje stan za pośrednictwem protokołu HTTP, które korzysta z tworzenia aplikacji biznesowych z sieci Web. Aplikacji opartych na formularzach sieci Web udostępnia dziesiątki zdarzeń, które są obsługiwane przez setki kontrolek serwerowych.
- Używa wzorca kontrolera strony, który dodaje funkcje do pojedynczych stron. Aby uzyskać więcej informacji, zobacz [kontrolera strony](https://go.microsoft.com/fwlink/?LinkId=106359 "kontrolera strony") w witrynie MSDN.
- Używa stanu widoku ani formularzy opartych na serwer, co ułatwia zarządzanie informacjami o stanie.
- Sprawdza się w przypadku niewielkich zespołów deweloperów sieci Web i projektantów, którzy chcą czerpać korzyści z dużej liczby składników dostępnych do szybkiego opracowywania aplikacji.
- Ogólnie rzecz biorąc, jest mniej złożona przy projektowaniu aplikacji, ponieważ składniki ( **strony** klasy, kontrolki itd.) są ściśle powiązane i zwykle potrzebna jest mniejsza ilość kodu niż w modelu MVC.

### <a name="advantages-of-an-mvc-based-web-application"></a>Zalety aplikacji sieci Web opartych na MVC

Platforma ASP.NET MVC ma następujące zalety:

- Ułatwia ona zarządzanie złożonością, dzieląc aplikację na model, widok i kontroler.
- Nie używa stanu widoku ani formularzy opartych na serwerze. Dzięki temu struktura MVC idealna dla deweloperów, którzy mają pełną kontrolę nad działaniem aplikacji.
- Używa wzorca kontrolera Frontowego, który przetwarza żądania aplikacji sieci Web za pośrednictwem jednego kontrolera. Pozwala na projektowanie aplikacji, która obsługuje złożoną infrastrukturę routingu. Aby uzyskać więcej informacji, zobacz [kontroler Frontowy](https://go.microsoft.com/fwlink/?LinkId=106357 "kontroler Frontowy") w witrynie MSDN.
- Projektowanie oparte na (testach TDD) zapewnia lepszą obsługę.
- Sprawdza się w przypadku aplikacji sieci Web, które są obsługiwane przez duże zespoły deweloperów i projektantów witryn sieci Web, którzy potrzebują wysokiego stopnia kontroli nad działaniem aplikacji.
