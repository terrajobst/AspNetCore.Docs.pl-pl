---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: "Porady: Dodawanie stron dla urządzeń przenośnych do formularzy sieci Web ASP.NET / aplikacji MVC | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Ten sposób można opisano różne sposoby do obsługi stron zoptymalizowane dla urządzeń przenośnych z formularzy sieci Web ASP.NET / aplikacji MVC, a także sugeruje architektury i..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2011
ms.topic: article
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: aac359b26c508784793a67260dc2e65c30db687a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Porady: Dodawanie stron dla urządzeń przenośnych do formularzy sieci Web ASP.NET / aplikacji MVC
====================
> **Dotyczy**
> 
> - Formularze sieci Web ASP.NET w wersji 4.0
> - ASP.NET MVC w wersji 3.0
> 
> **Podsumowanie**
> 
> Ten sposób można opisano różne sposoby do obsługi stron zoptymalizowane dla urządzeń przenośnych z formularzy sieci Web ASP.NET / aplikacji MVC i sugeruje architektury i projektu zagadnień, które należy wziąć pod uwagę podczas określania wartości docelowej dla wielu urządzeń. W tym dokumencie opisano również Dlaczego formantów przenośnych ASP.NET z programu ASP.NET 2.0 3.5 są teraz przestarzałe i omówiono niektóre nowoczesnych rozwiązań alternatywnych.


## <a name="contents"></a>Spis treści

- Omówienie
- Opcje architektury
- Wykrywanie przeglądarki i urządzenia
- Jak aplikacji formularzy sieci Web programu ASP.NET może ona powodować problemy specyficzne dla mobilnych stron
- Jak aplikacje programu ASP.NET MVC może ona powodować problemy specyficzne dla mobilnych stron
- Dodatkowe zasoby

Aby uzyskać przykłady kodu do pobrania prezentacja techniki to oficjalny dokument dla platformy MVC i formularzy sieci Web ASP.NET, zobacz [Mobile Apps & witryny ASP.NET](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Omówienie

Urządzenia przenośne — smartfonów, funkcja telefony i tablety — nadal zwiększa się popularne w celu dostępu do sieci Web. Dla wielu deweloperów sieci web i zorientowane na sieci web firmy oznacza to, że jest coraz bardziej ważne zapewnić doskonałe środowisko przeglądania sieci Web dla użytkowników używających tych urządzeń.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Jak w starszych wersjach programu ASP.NET obsługiwane przeglądarki dla urządzeń przenośnych

Wersje programu ASP.NET 2.0 do 3.5 uwzględnione *formantów przenośnych ASP.NET*: zestaw kontrolek serwera dla urządzeń przenośnych w *System.Web.Mobile.dll* zestawu i  *System.Web.UI.MobileControls* przestrzeni nazw. Zestaw jest dostępny w ASP.NET 4, ale jest przestarzały. Deweloperzy zalecana jest migracja do bardziej nowoczesnych metod, takich jak opisane w tym dokumencie.

Dlaczego formantów przenośnych ASP.NET została oznaczona jako przestarzała dzieje się tak aby ich projektu jest ukierunkowane wokół telefonów komórkowych, które zostały wspólnej wokół 2005 i wcześniejszych. Formanty głównie są przeznaczone do renderowania kodu znaczników WML lub cHTML (zamiast regularne HTML) w przypadku przeglądarek WAP z tym ery. Ale WAP, WML i cHTML nie są już odpowiednie dla większości projektów, ponieważ HTML stał się teraz język znaczników powszechny dla mobilnych i klasycznych przeglądarki podobne.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Wyzwania dzisiaj obsługi urządzeń przenośnych

Mimo że przeglądarki dla urządzeń przenośnych powszechnie obsługują teraz HTML, nadal będą występować wiele wyzwań, jeżeli celem tworzenia niezwykłych przenośnych przeglądania środowiska:

- ***Rozmiar ekranu*** — urządzeń przenośnych różnią się znacznie w formularzu, a ich ekrany są często znacznie mniejszy niż monitory pulpitu. Tak należy zaprojektować układ strony w dwóch zupełnie różnych dla nich.
- ***Dane wejściowe metody*** — niektóre urządzenia klawiatury, niektóre mają styluses, inne użyć touch. Konieczne może być należy wziąć pod uwagę nawigacji wiele mechanizmów metody i danych wejściowych.
- ***Zgodność ze standardami*** — wiele przeglądarek urządzeń przenośnych nie obsługują najnowszych standardów HTML, CSS i JavaScript.
- ***Przepustowość*** — wydajność sieci danych komórkowych zmienia się bardzo i niektórych użytkowników końcowych znajdują się na taryfy, które naliczają opłaty za megabajtów.

Istnieje rozwiązanie pasującego; aplikacja będzie musiał wyglądać i zachowywać się inaczej w zależności od urządzenia uzyskują do niej dostęp. W zależności od tego, jakiego poziomu wsparcia przenośnych, należy to być większe wyzwanie dla deweloperów sieci web niż kiedykolwiek było pulpitu "związany z konfliktami przeglądarki".

Deweloperzy zbliża się przeglądarkę dla telefonów pomocy technicznej, po raz pierwszy często początkowo wydaje się, że należy tylko do obsługi smartfonów najnowsze i najbardziej rozbudowane (np. Windows Phone 7, iPhone lub Android), prawdopodobnie ponieważ takie właścicielem często osobiście deweloperów urządzenia. Jednak tańsze telefony nadal są bardzo popularna, a właścicieli używać ich do przeglądania sieci web — szczególnie w krajach, w których ułatwiające wydobycie niż połączenie szerokopasmowe telefonów komórkowych. Należy wybrać zakres urządzeń do obsługi przy uwzględnieniu jego prawdopodobnie klientów firmy. Jeśli tworzysz online innej spa kondycji możliwość zaprojektowania kątem zabezpieczeń może być decyzji biznesowych celem zaawansowane smartfonów, natomiast jeśli tworzysz ticket system rezerwacji kinowych prawdopodobnie należy uwzględnić dla użytkowników zewnętrznych z funkcją słabszy telefony.

## <a name="architectural-options"></a>Opcje architektury

Przed uzyskujemy do szczegółowych informacji technicznych formularzy sieci Web ASP.NET lub MVC, należy zauważyć, że Deweloperzy sieci web zwykle trzy główne sposoby, aby obsługa przeglądarek urządzeń przenośnych:

1. ***Nic nie rób —*** można po prostu utworzyć aplikacji sieci web standardowe, zorientowane na pulpicie i zależne od przeglądarki dla urządzeń przenośnych do renderowania on prawidłowo. 

    - **Zaletą**: jest najtańszej opcji wdrożenia i obsługa — bez żadnych dodatkowych pracy
    - **Wadą**: zapewnia najgorszy środowisko użytkownika końcowego: 

        - Najnowsze smartfonów może spowodować HTML równie dobrze jako przeglądarka pulpitu, ale użytkownicy nadal będą zmuszeni do powiększania i przewijania w poziomie i w pionie do korzystania z zawartości na małego ekranu. Jest to daleko od optymalne.
        - Starsze urządzenia i telefony funkcja może zakończyć się niepowodzeniem do renderowania z kodu znaczników w zadowalający sposób.
        - Nawet w przypadku najnowszej urządzenia typu tablet (których ekranach może być tak duży jak ekranów komputerów przenośnych) mają zastosowanie różne interakcji reguły. Touch na podstawie danych wejściowych działa najlepiej z przyciskami większych i łączy rozpowszechniania dalsze od siebie, a nie istnieje sposób na wskaźnik myszy nad menu wysuwanego.
2. ***Rozwiązuje problem, na kliencie* —** z użyciem dokładne CSS i [stopniowym rozszerzaniu](http://en.wikipedia.org/wiki/Progressive_enhancement) można utworzyć kod znaczników, style i skrypty dostosowania do dowolnej przeglądarki zostały uruchomione. Na przykład z [zapytaniami multimediów CSS 3](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), można utworzyć układ wielokolumnowego zmieni się w układzie pojedynczej kolumny na urządzeniach, na których ekrany są węższe niż wybrany próg. 

    - **Zaletą**: optymalizuje renderowania dla konkretnego urządzenia w użyciu, nawet w przypadku nieznany przyszłych urządzeń zgodnie z dowolnego ekranu i właściwości wejściowe mają
    - **Zaletą**: pozwala łatwo udostępniać logiki po stronie serwera wszystkich typów urządzeń — minimalnego dublowania kodu lub nakładu pracy
    - **Wadą**: urządzenia przenośne tak różnią się od urządzeń komputerowych może pewno chcesz stronach przenośnych całkowicie różni się od strony pulpitu przedstawiający różne informacje. Te różnice mogą być mało wydajne i niemożliwe do osiągnięcia niezawodnie za pośrednictwem CSS samodzielnie, szczególnie biorąc pod uwagę sposób niespójnie starszych urządzeń interpretowania reguły CSS. Jest to szczególnie istotne atrybutów CSS 3.
    - **Wadą**: nie zapewnia obsługi dla różnych logikę po stronie serwera i przepływy pracy dla różnych urządzeń. Nie można na przykład wprowadzić uproszczony zakupów koszyka wyewidencjonowania przepływu pracy dla użytkowników urządzeń przenośnych za pomocą samodzielnie CSS.
    - **Wadą**: wykorzystanie przepustowości nieefektywne. Serwer może być przesyłać znaczniki i style, które są stosowane do wszystkich możliwych urządzeń, nawet jeśli urządzenie będzie używać tylko podzbiór tych informacji.
3. ***Rozwiązuje problem na serwerze* —** Jeśli serwer wie, jakiego urządzenia jest dostępu do niego — lub w co najmniej właściwości danego urządzenia, takie jak jego rozmiaru ekranu i Metoda wejściowa i czy jest to urządzenie przenośne — można je uruchomić inny logiki i dane wyjściowe różny kod znaczników HTML. 

    - **Zaleta:** maksymalną elastyczność. Nie ma żadnego limitu ilości różnią się logiki po stronie serwera dla przenośne lub zoptymalizować z kodu znaczników dla układu żądaną, specyficzne dla urządzenia.
    - **Zaleta:** wykorzystanie wydajnych przepustowości. Należy przesyłać znaczniki i elementy docelowe urządzenie będzie używać.
    - **Wadą:** czasami wymusza powtarzania nakładu pracy lub kodu (np., co możesz utworzyć podobne, lecz nieco inne kopii stron formularzy sieci Web lub widoków MVC). Gdzie to możliwe, którego można będzie uwzględnić limitu wspólnej logiki warstwy podstawowej lub usługi, ale nadal, niektóre elementy interfejsu użytkownika kodu lub znaczników może być zduplikowany, a następnie aktualizować równolegle.
    - **Wadą:** wykrywania urządzeń nie jest prosta. Wymaga listy lub bazy danych typów znanym urządzeniem i ich właściwości (które mogą nie być idealne aktualne), a nie jest gwarantowana Aby dokładnie dopasować każdego żądania przychodzącego. W tym dokumencie opisano niektóre opcje i pułapki związane z ich później.

Aby uzyskać najlepsze wyniki, większość deweloperów find muszą łączyć opcji (2) i (3). Niewielkie różnice stylistycznych najlepiej są obsługiwane na kliencie przy użyciu CSS i JavaScript nawet główne różnice w danych przepływu pracy i znaczników najbardziej efektywne są wdrażane w kodzie po stronie serwera.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Ten dokument koncentruje się na temat metod po stronie serwera

Ponieważ MVC i formularzy sieci Web ASP.NET obu tych technologii głównie po stronie serwera, ten dokument koncentruje się na temat metod po stronie serwera, które pozwalają na różnych znaczników i logikę przeglądarki dla urządzeń przenośnych. Oczywiście można także połączyć to z żadnych technika po stronie klienta (np. CSS 3 zapytaniami multimediów, zwiększające progresywnego JavaScript), ale jest to bardziej kwestią projekt sieci web niż projektowanie programu ASP.NET.

## <a name="browser-and-device-detection"></a>Wykrywanie przeglądarki i urządzenia

Klucza wstępnym dla wszystkich metod po stronie serwera do obsługi urządzeń przenośnych jest wiedzieć, jaki urządzenie korzysta z obiektu odwiedzającego. W rzeczywistości nawet lepszym rozwiązaniem niż wiedząc uprzedniego uzyskania informacji o numer producenta i modelu urządzenia *właściwości* urządzenia. Właściwości mogą obejmować:

- Jest on urządzenia przenośnego?
- Metoda wejściowa (myszy/klawiatury, touch, klawiaturę, joystick,...)
- Rozmiar ekranu (fizycznie lub w pikselach)
- Obsługiwane formaty nośników i danych
- Etc.

Zaleca się podjęcie decyzji, na podstawie charakterystyk niż numer modelu, ponieważ, a następnie będzie lepiej przystosowany do obsługi urządzeń w przyszłości.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Za pomocą stron ASP. Obsługa wykrywania przeglądarki wbudowanej na NET

Deweloperzy i formularzy sieci Web platformy ASP.NET MVC można natychmiast odnajdywanie najważniejsze czynniki zaproszonych przeglądarki przez sprawdzenie właściwości *Request.Browser* obiektu. Na przykład zobacz

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- .. .a wielu innych

W tle, platforma ASP.NET jest zgodna z przychodzącego *agenta użytkownika* nagłówka HTTP (UC) przed wyrażeń regularnych w zestawie plików XML definicji przeglądarki. Domyślnie platforma zawiera definicje dla wielu typowych urządzeń przenośnych, a można dodać niestandardowe pliki definicji przeglądarki dla innych osób, które chcesz rozpoznać. Aby uzyskać więcej informacji, zobacz stronę MSDN [kontrolki serwera sieci Web ASP.NET i możliwości przeglądarki](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Korzystanie z bazy danych urządzenia WURFL za pośrednictwem 51Degrees.mobi Foundation

Podczas ASP. Obsługa wykrywania przeglądarki wbudowanej na NET są wystarczające dla wielu aplikacji istnieją dwa główne przypadki gdy może nie być wystarczającej ilości:

- ***Aby rozpoznać najnowsze urządzeń***(bez ręcznego tworzenia plików definicji przeglądarki dla nich). Należy pamiętać, że .NET 4 definicji przeglądarki pliki nie są wystarczająco aktualna, aby rozpoznać Windows Phone 7, telefony z systemem Android, Opera Mobile przeglądarki lub Ipad firmy Apple.
- ***Potrzebujesz bardziej szczegółowe informacje dotyczące możliwości urządzenia***. Musisz wiedzieć o urządzenia metody wprowadzania (np. touch vs klawiaturze numerycznej) lub jakie audio formatuje przeglądarka obsługuje. Te informacje są niedostępne w standardowe pliki definicji przeglądarki.

[ *Bezprzewodowej uniwersalnych pliku zasobu* projektu (WURFL)](http://wurfl.sourceforge.net/) obsługuje większą aktualnych i szczegółowe informacje dotyczące urządzeń przenośnych w użyciu dzisiaj.

Dobra wiadomość dla deweloperów platformy .NET jest ASP. Funkcji wykrywania sieci w przeglądarce jest otwarty, dzięki czemu można zwiększyć, aby rozwiązać te problemy. Na przykład można dodać typu open source [ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection) biblioteki do projektu. Jest to ASP.NET IHttpModule lub przeglądarki możliwości dostawcy (można go używać w przypadku aplikacji MVC i formularzy sieci Web), który bezpośrednio odczytywać dane WURFL i przechwytuje go w ASP. Mechanizm wykrywania przeglądarki wbudowanej w sieci. Po zainstalowaniu modułu, *Request.Browser* nagle będzie zawierać znacznie bardziej dokładne i szczegółowe informacje: zostanie poprawnie rozpoznać wiele urządzeń wspomnianych wcześniej i wyświetlać ich możliwości (w tym dodatkowe funkcje takie jak metoda wejściowa). Zapoznaj się dokumentacją projektu, aby uzyskać więcej informacji.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Jak aplikacji formularzy sieci Web może ona powodować problemy specyficzne dla mobilnych stron

Domyślnie poniżej przedstawiono sposób wyświetlania zupełnie nowej aplikacji formularzy sieci Web na urządzeniach przenośnych wspólne:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Wyraźnie widać ani układu wygląda bardzo przyjaznych dla urządzeń przenośnych — ta strona została opracowana w przypadku dużych, orientacji poziomej monitora nie dla małego ekranu orientacji pionowej. Dlatego co można zrobić informacji na ten temat?

Zgodnie z opisem we wcześniejszej części tego dokumentu, istnieje wiele sposobów, aby dostosować strony dla urządzeń przenośnych. Niektóre metody są oparte na serwerze, innym uruchomienia na kliencie.

### <a name="creating-a-mobile-specific-master-page"></a>Tworzenie mobile specyficzne dla strony wzorcowej

W zależności od wymagań, można używać tego samego formularzy sieci Web dla wszystkich odwiedzających, ale ma dwa oddzielne strony wzorcowe: jeden dla użytkowników pulpitu, jedno dla gości przenośne. Zapewnia to elastyczność zmiany arkusza stylów CSS lub użytkownika najwyższego poziomu kod znaczników HTML do własnych urządzeń przenośnych, bez wymuszania można zduplikować wszelka logika strony.

Jest to łatwo zrobić. Na przykład można dodać obsługi PreInit, takie jak wymienione poniżej do formularza sieci Web:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Teraz Utwórz strony wzorcowej o nazwie Mobile.Master w folderze najwyższego poziomu w aplikacji i będą używane po wykryciu urządzenia przenośnego. W razie potrzeby z strony wzorcowej może się odwoływać arkusz stylów CSS specyficzne dla mobilnych. Odwiedzający pulpitu, będą również widzieć domyślnej strony głównej, co przenośnych.

### <a name="creating-independent-mobile-specific-web-forms"></a>Tworzenie niezależnych formularzy sieci Web specyficzne dla mobile

Maksymalna elastyczność można przejść znacznie więcej niż tylko osobnych stron wzorcowych dla różnych typów urządzeń. Można zaimplementować dwa *całkowicie oddzielić zestawów stron formularzy sieci Web* — jeden ustawiony w przypadku przeglądarek komputerowych, innego zestawu dla przenośne. Ten sposób działa najlepiej, jeśli chcesz istnieje bardzo różne informacje lub przepływów pracy dla osób odwiedzających przenośnych. Pozostałej części tej sekcji opisano takie podejście szczegółowo.

Zakładając, że masz już aplikację formularzy sieci Web przeznaczone dla przeglądarek komputerowych, najprostszym sposobem, aby kontynuować jest utworzenie podfolder o nazwie "Mobile" w projekcie i kompilacji swoich stronach przenośnych. Można utworzyć całej lokacji podrzędnych, z własnym strony wzorcowe, arkusze stylów i stron, przy użyciu tych samych metod, co w przypadku innych aplikacji formularzy sieci Web. Nie jest konieczna utworzyć przenośnych odpowiadający *co* strony w witrynie pulpitu; można wybrać podzbiór funkcji ma sens dla użytkowników mobilnych.

Twoje stron dla urządzeń przenośnych może udostępniać zasoby statyczne wspólnej (takich jak obrazy, JavaScript i CSS pliki) ze stronami sieci regularne w razie potrzeby. Ponieważ folder "Mobile" będzie *nie* można oznaczyć jako osobne aplikacji podczas udostępniania w usługach IIS (jest proste podfolder stron formularzy sieci Web), jego również udostępni te same konfiguracji, dane sesji i pozostałą infrastrukturą jako użytkownika strony pulpitu.

> [!NOTE]
> Ponieważ takie podejście zwykle obejmuje niektóre dublowania kodu (stron dla urządzeń przenośnych prawdopodobnie może udostępniać pewnych podobieństw stron pulpitu), ważne jest, aby współczynnik wszystkie wspólne firm logiki lub dane dostępu kodu do udostępnionego warstwy podstawowej lub usługi. W przeciwnym razie należy dokładnie nakładu pracy tworzenia i obsługi aplikacji.


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Przekierowywanie przenośnych odwiedzin strony przenośnych

Często jest to wygodny przekierować gości przenośne do stron dla urządzeń przenośnych tylko na *pierwszy* żądania w ich sesji przeglądania (i nie na każde żądanie w ich sesji), ponieważ:

- Następnie umożliwiają łatwe odwiedzających przenośnych uzyskać dostęp do swoich stronach pulpitu, jeśli chcą — wystarczy umieścić łącze na stronie głównej prowadzącym do "Pulpitu version". Obiekt odwiedzający nie będzie przekierowany do strony przenośnej, ponieważ nie jest już to pierwsze żądanie w sesji.
- Takie rozwiązanie pomaga uniknąć ryzyka związanego z zakłócać żądania dla wszystkich dynamicznych zasobów udostępnionych między częściami wersje desktop i mobile witryny (np. w przypadku typowych formularza sieci Web wyświetlanych w części zarówno wersje desktop i mobile witryny w elementu IFRAME lub niektórych obsługi technologii Ajax)

Aby to zrobić, możesz umieścić logiki przekierowania w **sesji\_Start** metody. Na przykład dodaj następującą metodę do pliku Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Konfigurowanie uwierzytelniania formularzy do przestrzegania Twojej stron dla urządzeń przenośnych

Należy pamiętać, że uwierzytelnianie formularzy zapewnia pewne założenia, o której go można przekierowywać odwiedzających podczas i po procesie uwierzytelniania:

- Gdy użytkownik musi zostać uwierzytelniony, uwierzytelnianie formularzy będzie przekierowanie do strony logowania, niezależnie od tego, czy są one użytkownika desktop lub mobile (ponieważ ma ona tylko koncepcji *jeden* adres URL logowania). Zakładając, że chcesz styl stronę logowania przenośnych inaczej, należy zwiększyć ze strony logowania, aby przekierowania do strony logowania przenośnych oddzielne użytkowników mobilnych. Na przykład, Dodaj następujący kod do Twojej **pulpitu** kodem strony logowania: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Po pomyślnym zalogowaniu użytkownika, uwierzytelnianie formularzy zostanie domyślnie przekierowanie do strony głównej pulpitu (ponieważ ma ona tylko koncepcji *jeden* domyślnej strony). Należy poprawić stronę logowania przenośne tak, aby wykonuje przekierowanie do strony głównej mobile po pomyślnym zalogowaniu się w. Na przykład, Dodaj następujący kod do Twojej **przenośnych** kodem strony logowania: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
 Ten kod przyjęto założenie, że strony jest formant serwera logowania nazywany odwoływały, jak w domyślnym szablonie projektu.

### <a name="working-with-output-caching"></a>Praca z buforowanie danych wyjściowych

Jeśli używasz buforowanie danych wyjściowych, należy pamiętać, że domyślnie istnieje możliwość pulpitu użytkownika do odwiedzenia niektórych adresu URL (co powoduje jego dane wyjściowe pamięci podręcznej), a następnie przenośnych użytkownika, który otrzyma zbuforowanej wyjściowej pulpitu. Dotyczy to ostrzeżenie jest tylko zróżnicowanie stronę wzorcową według typu urządzenia lub wykonania całkowicie oddzielnych formularzach sieci Web według typu urządzenia.

Aby uniknąć problemu, możesz wydać ASP.NET się różnić w zależności od tego, czy użytkownik jest na urządzeniu przenośnym, wpisu pamięci podręcznej. Dodaj parametr Element VaryByCustom do strony swojego OutputCache deklaracji w następujący sposób:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

Następnie określ *isMobileDevice* jako niestandardowego pamięci podręcznej parametru, dodając następujące metody zastąpienie pliku Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Daje to pewność, że przenośnych odwiedzin strony nie otrzyma dane wyjściowe wcześniej umieszczane w pamięci podręcznej przez obiekt odwiedzający pulpitu.

### <a name="a-working-example"></a>Przykład pracy

Aby wyświetlić te techniki w akcji, Pobierz [przykłady kodu w tym oficjalnym dokumencie](https://docs.microsoft.com/aspnet/mobile/overview). Przykładowa aplikacja formularzy sieci Web automatycznie przekierowuje użytkowników mobilnych z zestawem mobile określonych stron w podfolderze o nazwie Mobile. Znaczniki i style tych stron jest zoptymalizowana dla przeglądarki dla urządzeń przenośnych, jak widać w poniższe zrzuty ekranu:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Aby uzyskać więcej porad dotyczących optymalizacji sieci znaczników i CSS dla przeglądarki dla urządzeń przenośnych zobacz sekcję "Style stron dla urządzeń przenośnych dla przeglądarki dla urządzeń przenośnych" w dalszej części tego dokumentu.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Jak aplikacje programu ASP.NET MVC może ona powodować problemy specyficzne dla mobilnych stron

Ponieważ wzorca Model-View-Controller zapewnia oddzielenie logiki aplikacji (w kontrolerach) z logiki prezentacji (w widokach), można za pomocą dowolnego z poniższych metod przenośnych obsługi w kodzie po stronie serwera:

1. ***Użyj tego samego kontrolery i widoki dla przeglądarek, które komputery i przenośnych, ale renderowania widoków z różnymi układami Razor, w zależności od typu urządzenia *.** Ta opcja działa najlepiej, jeśli można wyświetlić identyczne dane na wszystkich urządzeniach, ale po prostu chcesz podać inną arkusze stylów CSS lub zmienić kilka elementów HTML najwyższego poziomu dla przenośne.
2. ***Użyj tego samego kontrolerów w przypadku przeglądarek zarówno wersje desktop i mobile, ale renderowania różne widoki w zależności od typu urządzenia***. Ta opcja działa najlepiej, jeśli jest około wyświetlanie tych samych danych i zapewnienie tego samego przepływy pracy dla użytkowników końcowych, ale aby renderować bardzo różny kod znaczników HTML do własnych urządzenie używane.
3. ***Utwórz oddzielne obszary dla komputerów stacjonarnych i przenośnych przeglądarek, implementacja niezależne kontrolery i widoki dla każdego *.** Ta opcja działa najlepiej, gdy są wyświetlane ekrany bardzo różnych, zawierających różne informacje i początkowe użytkownika za pomocą różnych przepływów pracy, zoptymalizowana pod kątem ich typ urządzenia. Może to oznaczać niektórych powtarzania kodu, ale który można zminimalizować przez factoring limitu wspólnej logiki do warstwy podstawowej lub usługi.

Jeśli chcesz wykonać **pierwszy** opcji i różnią się jedynie układu Razor według typu urządzenia, jest bardzo proste. Po prostu zmodyfikuj Twojej \_ViewStart.cshtml plików w następujący sposób:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Teraz możesz utworzyć układ specyficzne dla mobile o nazwie \_LayoutMobile.cshtml ze strukturą strony i CSS zasady, zoptymalizowane dla urządzeń przenośnych.

Jeśli chcesz wykonać **drugi** opcji renderowania widoków całkiem zgodnie z zewnętrznego typu urządzenia, zobacz [Scott Hanselman wpis w blogu](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

Pozostała część tego dokumentu skupiono się na **trzeci** opcję — Tworzenie oddzielnych kontrolerów *i* widoków dla urządzeń przenośnych — tak można kontrolować dokładnie podzbiór funkcji jest dostępna w przypadku gości przenośne.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Konfigurowanie przenośnych obszaru w aplikacji ASP.NET MVC

Można dodać obszaru o nazwie "Mobile" do istniejącej aplikacji ASP.NET MVC w normalnym trybie: kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, a następnie wybierz pozycję Dodaj a obszaru. Następnie można dodać widoków i kontrolerów jak w przypadku innych obszaru w aplikacji ASP.NET MVC. Na przykład dodać obszaru przenośnych nowego kontrolera o nazwie HomeController do działania jako stronę główną dla gości przenośne roboczej.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Zapewnienie /Mobile adres URL osiągnie przenośnych strony głównej

Chcąc /Mobile adres URL do osiągnięcia akcji indeksu na HomeController wewnątrz obszaru przenośnych, konieczne będzie wprowadź dwie niewielkich zmian w konfiguracji usługi routingu. Najpierw należy zaktualizować klasy MobileAreaRegistration tak, aby HomeController domyślnego kontrolera w Twoim rejonie przenośnych, jak pokazano w poniższym kodzie:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

Oznacza to, przenośne strony głównej teraz powinien znajdować się w /Mobile zamiast/Mobile domowych, ponieważ "Home" jest teraz niejawnie domyślną nazwę kontrolera dla stron dla urządzeń przenośnych.

Następnie należy pamiętać, że dodanie drugiego HomeController do aplikacji (tj. przenośnych jeden, oprócz istniejącego pulpitu jeden), będzie Przerwano regularne strona główna pulpitu. Powiedzie się z powodu błędu "*spełniających kontrolera o nazwie"Home"znaleziono wiele typów*". Aby rozwiązać ten problem, zaktualizuj konfigurację routingu najwyższego poziomu (w Global.asax.cs) do określenia, czy istnieje niejednoznaczność użytkownika pulpitu HomeController powinien mają większy priorytet:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Teraz błąd przejdzie zadań i adresu URL http://*yoursite*/ osiągną strony głównej pulpitu i http://*yoursite*/mobile/ osiągną przenośnych strony głównej.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Przekierowywanie gości przenośne obszaru przenośnych

Istnieje wiele punktów rozszerzalności różnych na platformie ASP.NET MVC, więc istnieje wiele sposobów możliwych do dodania logiki przekierowania. Jedną z opcji porządek jest można utworzyć atrybutu filtru, [RedirectMobileDevicesToMobileArea], który wykonuje przekierowanie, jeśli są spełnione następujące warunki:

1. Jest to pierwsze żądanie w sesji użytkownika (tj. Session.IsNewSession jest równa true)
2. Żądanie pochodzi z przenośnymi przeglądarki (np. Request.Browser.IsMobileDevice jest równa true)
3. Użytkownik nie jest już żąda zasobu w obszarze przenośnych (np. *ścieżki* część adresu URL nie rozpoczyna się od /Mobile)

Do pobrania próbki uwzględnionych w tym oficjalnym dokumencie zawiera implementację tego logiki. Są one zaimplementowane jako filtr autoryzacji pochodzi od klasy AuthorizeAttribute, co oznacza, że jej poprawnego działania nawet wtedy, gdy używasz buforowanie danych wyjściowych (w przeciwnym razie jeśli może być w pamięci podręcznej i następnie udostępniane pulpitu odwiedzający pierwszy uzyskuje dostęp do niektórych adresu URL, pulpitu dane wyjściowe kolejne gości przenośne)

Jak filtr, można wybrać jedną do określonych kontrolerów i akcji, np.

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… zastosowanie do wszystkich kontrolerów i akcji jako MVC 3 lub *filtrów globalnych* w pliku Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

Do pobrania przykładzie pokazano także, jak utworzyć podklasy tego atrybutu, które przekierowują do określonych lokalizacji w Twoim rejonie przenośnych. Oznacza to, na przykład można wykonywać następujące czynności:

- Zarejestrowanie filtru globalnego jak pokazano powyżej wysyłanej przenośnych odwiedzających przenośnych strony głównej domyślnie.
- Dotyczą także specjalne filtru [RedirectMobileDevicesToMobileProductPage] akcję "Wyświetl produkt", która przyjmuje gości przenośne do mobilnej wersji niezależnie od ich była żądana strona produktu.
- Mają zastosowanie również w innych specjalnych podklasy filtru określonych czynności, przekierowywanie gości przenośne do odpowiedniej strony przenośnych

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Konfigurowanie uwierzytelniania formularzy do przestrzegania Twojej stron dla urządzeń przenośnych

Jeśli używane jest uwierzytelnianie formularzy, należy zauważyć, że gdy użytkownik musi się zalogować, go automatycznie przekierowuje użytkownika do jeden określonych "Logowanie" adres URL, czyli domyślnie **/Account/logowania**. Oznacza to, że użytkowników mobilnych mogą być przekierowywane na akcję pulpitu "Logowanie".

Aby uniknąć tego problemu, Dodaj logikę do pulpitu akcji "Logowanie", aby ponownie przekierowuje użytkowników urządzeń przenośnych do przenośnych akcji "Logowanie". Jeśli używasz domyślnego szablonu aplikacji platformy ASP.NET MVC, zaktualizuj działań logowania tego elementu AccountController w następujący sposób:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… a następnie wdrożyć odpowiednich akcji "Logowanie" mobile określonych w kontrolerze o nazwie elementu AccountController w Twoim rejonie przenośnych.

### <a name="working-with-output-caching"></a>Praca z buforowanie danych wyjściowych

Jeśli używasz filtru [OutputCache], należy wymusić wpisu pamięci podręcznej do zależy od typu urządzenia. Na przykład zapisu:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Następnie dodaj następującą metodę do pliku Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Daje to pewność, że przenośnych odwiedzin strony nie otrzyma dane wyjściowe wcześniej umieszczane w pamięci podręcznej przez obiekt odwiedzający pulpitu.

### <a name="a-working-example"></a>Przykład pracy

Aby wyświetlić te techniki w akcji, Pobierz [kodu w tym oficjalnym dokumencie skojarzone przykłady](https://docs.microsoft.com/aspnet/mobile/overview). Przykład obejmuje aplikacji ASP.NET MVC 3 (Release Candidate) rozszerzony do obsługi urządzeń przenośnych za pomocą metod opisanych powyżej.

## <a name="further-guidance-and-suggestions"></a>Dalsze wskazówki i sugestie

Następujące dyskusji odnosi się zarówno do formularzy sieci Web i MVC deweloperów, którzy korzystają z techniki omówione w tym dokumencie.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Udoskonalanie logiki przekierowania przy użyciu 51Degrees.mobi Foundation

Logiki przekierowania opisane w tym dokumencie może być całkowicie wystarczający dla aplikacji, ale nie będzie działać, jeśli trzeba wyłączyć sesji lub z przeglądarek urządzeń przenośnych, które odrzucić pliki cookie (te nie mogą mieć sesje), ponieważ nie będzie wiadomo, czy danego żądania jest pierwsza od tej osoby odwiedzającej.

Wiesz już, jak poprawić dokładność ASP 51Degrees.mobi typu open source Foundation. Wykrywanie przeglądarki w sieci. Ma ona wbudowana możliwość przekierowania gości przenośne do określonych lokalizacji skonfigurowany w pliku Web.config. Może działać bez w zależności od sesji platformy ASP.NET (i w związku z tym pliki cookie) przez zapisanie tymczasowego dziennika wartości skrótów odwiedzających nagłówków HTTP i adresów IP, dlatego bez informacji czy każdego żądania jest pierwsza od danego vistor.

Następujący element dodawać do sekcji fiftyOne pliku web.config przekieruje pierwsze żądanie z wykrytego urządzenia przenośnego do strony ~ / Mobile/Default.aspx. Wszystkie żądania dotyczące stron w folderze przenośnych będą *nie* przekierowanie, niezależnie od typu urządzenia. Jeśli urządzenie przenośne jest nieaktywny przez okres 20 minut lub więcej urządzeń będzie można zapomnienia hasła i kolejne żądania będą traktowane jako nowe na potrzeby przekierowania.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Aby uzyskać więcej informacji, zobacz [51degrees.mobi dokumentacji Foundation](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> Możesz *można* Foundation 51Degrees.mobi Użyj funkcji Przekierowanie na aplikacje programu ASP.NET MVC, ale należy zdefiniować konfiguracji przekierowania w postaci zwykłego adresów URL, nie pod względem routingu parametrów lub poprzez umieszczenie filtrów platformy MVC na działania. Jest to spowodowane (w czasie zapisywania) nie może rozpoznać 51Degrees.mobi Foundation filtry lub routingu.


### <a name="disabling-transcoders-and-proxy-servers"></a>Wyłączenie Transkodery i serwerów Proxy

Operatory sieci komórkowej ma dwa cele szerokie w ich podejście do przenośnych internet:

1. Podaj jako znacznie odpowiedniej zawartości, jak to możliwe
2. Zwiększenie liczby klientów, którzy mogą udostępniać opcji ograniczona przepustowość sieci

Ponieważ większość stron sieci web zostały zaprojektowane dla dużych ekranów o rozmiarze pulpitu i połączenia szerokopasmowego fast stałej wierszy, użyj wielu operatorów *transkodery* lub *serwerów proxy* który dynamicznie zmieniać zawartość sieci web. Mogą oni modyfikować z kodu znaczników HTML lub CSS do własnych mniejszych ekranach (szczególnie w przypadku "Funkcja telefony" braku mocy obliczeniowej do obsługi złożonych układów), a ich może ponownie kompresować obrazów (ograniczaniu ich jakości) do zwiększenia szybkości dostarczania strony.

Ale jeśli zajęło starań, aby utworzyć zoptymalizowanych pod kątem mobile wersji lokacji, nie będzie prawdopodobnie operatorem sieci, aby zakłócać jego dowolne dodatkowe. Na stronie można Dodaj następujący wiersz\_zdarzeń obciążenia w jakimkolwiek formularzu sieci Web platformy ASP.NET:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

Lub kontrolera ASP.NET MVC, można dodać następujące metodę zastępującą, że jest ona stosowana do wszystkich akcji na tym kontrolerze:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

Wynikowy komunikat HTTP informuje W3C transkodery zgodne i serwerów proxy, aby nie zmieniać zawartość. Oczywiście nie ma żadnej gwarancji, że operatorów sieci komórkowej szanuje tego komunikatu.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Ustawianie stylów stron dla przeglądarki dla urządzeń przenośnych dla urządzeń przenośnych

Wykracza poza zakres tego dokumentu opisano szczegółowo jakiego rodzaju pracy kod znaczników HTML poprawnie lub które technik projektu sieci web zmaksymalizować użyteczność na poszczególnych urządzeń. Się użytkownikowi w celu znalezienia wystarczająco prostym układzie, optymalizacji dla ekranu o rozmiarze mobile, bez użycia zawodnych HTML i CSS pozycjonowanie wskazówki. Co ważne technika, warto zauważyć, jednak jest *okienka ekranu metatag*.

Niektóre nowoczesnych przeglądarkach dla urządzeń przenośnych, na stronach sieci web wyświetlaną nakładu przeznaczone dla monitorów pulpitu renderowania strony na kanwie wirtualne, nazywane również "okienka ekranu" (np. wirtualnych okienka ekranu jest 980 pikseli szerokości na telefonie iPhone i 850 pikseli szerokości w programie Opera Mobile domyślnie), a następnie Skalowanie wynik w dół do mieszczą się w pikselach fizycznych. Użytkownik może następnie powiększanie i przesuwanie tego okienka ekranu. To ma tę zaletę, że umożliwia przeglądarka wyświetli tej strony w jej układu zamierzone, ale jest również ma wada to wymusza powiększanie i przesuwanie, czyli niewygodne dla użytkownika. Projektując dla urządzeń przenośnych, lepiej jest do projektowania dla wąskie ekranu tak, że niezbędne jest nie powiększanie i przewijanie w poziomie.

Sposób mówić przenośnych przeglądarki, jak szeroka powinna być okienko ekranu jest za pomocą niestandardowych *okienka ekranu* meta tag. Na przykład dodać następujące sekcji HEAD ze strony

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… następnie Obsługa przeglądarek smartphone będzie układ strony na kanwie wielu wirtualnych 480 pikseli. Oznacza to, że elementów HTML zdefiniować ich szerokość procentowo, wartości procentowe będą interpretowane względem tego 480 pikseli szerokości, nie domyślnej szerokości okienka ekranu. W związku z tym użytkownik jest mniej prawdopodobne do powiększanie i przesuwanie poziomo — znacznie poprawy przeglądania na urządzeniach przenośnych.

Ma szerokość okienka ekranu do dopasowania w pikselach fizycznych urządzenia można określić następujące czynności:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Dla tego działał prawidłowo, należy nie jawnie wymusić elementy przekroczenie tego szerokości (np. przy użyciu *szerokość* atrybutu lub właściwości CSS), wymuszają przeglądarki do użycia niezależnie od większych okienka ekranu. Zobacz też: [więcej szczegółów na temat znacznika okienka ekranu niestandardowe](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

Obsługuje większość nowoczesnych smartfonów *podwójną orientacji*: może być przechowywany w trybie pionowa lub pozioma. Dlatego ważne jest nie dokonywać założenia dotyczące szerokość ekranu w pikselach. Nie nawet założono, że jest stała szerokość ekranu, ponieważ użytkownika ponownie można ustawić orientację swojego urządzenia, gdy są one na stronie.

Starsze urządzenia Windows Mobile i Blackberry również może przyjmować następujące tagi meta w nagłówku strony, aby poinformować zawartości został zoptymalizowany dla urządzeń przenośnych i dlatego nie powinny zostać przekształcone.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Dodatkowe zasoby

Listę urządzeń przenośnych emulatorów i symulatorów, można użyć do testowania aplikacji mobilnej sieci web ASP.NET, zobacz stronę [symulować popularnych urządzeń przenośnych na potrzeby testowania](../mobile/device-simulators.md).

## <a name="credits"></a>Środki na korzystanie z

- Główny Autor: Steven Sanderson
- Osoby dokonujące przeglądu / dodatkowe składniki zapisywania zawartości: Rosewell Kuba, Mikael Söderström, Scott Hanselman, Scott myśliwego
