---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 'Porady: Dodawanie stron dla urządzeń przenośnych do aplikacji ASP.NET Web Forms / MVC aplikacji | Dokumentacja firmy Microsoft'
author: rick-anderson
description: Ten sposób można w tym artykule opisano różne sposoby, aby obsługiwać strony zoptymalizowane pod kątem urządzeń przenośnych z formularzy sieci Web platformy ASP.NET / aplikacji MVC, a także sugeruje architektury i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2011
ms.topic: article
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
ms.technology: ''
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: 075329087cb5e07d85bba0c546538e7cc55ac463
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366760"
---
<a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Porady: Dodawanie stron dla urządzeń przenośnych do aplikacji ASP.NET Web Forms / MVC aplikacji
====================
> **Dotyczy**
> 
> - Formularze sieci Web platformy ASP.NET w wersji 4.0
> - ASP.NET MVC w wersji 3.0 lub nowszej
> 
> **Podsumowanie**
> 
> Ten sposób można w tym artykule opisano różne sposoby, aby obsługiwać strony zoptymalizowane pod kątem urządzeń przenośnych z formularzy sieci Web platformy ASP.NET / aplikacji MVC i sugeruje architektury i projektowania kwestie do rozważenia przy przeznaczeniu szerokiej gamy urządzeń. W tym dokumencie wyjaśniono również, dlaczego formantów mobilnych ASP.NET z programu ASP.NET 2.0 3.5 są już nieaktualne, a w tym artykule omówiono niektóre nowoczesnych rozwiązań alternatywnych.


## <a name="contents"></a>Spis treści

- Omówienie
- Opcje architektury
- Wykrywanie przeglądarki i urządzenia
- Jak aplikacji formularzy sieci Web programu ASP.NET może powodować mobile specyficznych stron
- Jak aplikacje programu ASP.NET MVC można przedstawić mobile specyficznych stron
- Dodatkowe zasoby

Aby uzyskać przykłady kodu do pobrania, prezentacja technik ten oficjalny dokument dla platformy MVC i formularzy sieci Web ASP.NET, zobacz [Mobile Apps & witryny ASP.NET](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Omówienie

Urządzenia przenośne — smartfony, funkcja telefony i tablety — w dalszym ciągu Zwiększanie popularności jako sposób dostępu do sieci Web. W przypadku wielu programistów sieci web i zorientowane na sieci web firmy oznacza to, że jest coraz ważniejsze zapewnić doskonałe środowisko przeglądania dla gości korzystających z tych urządzeń.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Jak wcześniejszych wersjach programu ASP.NET obsługiwanych przeglądarek dla urządzeń przenośnych

Program ASP.NET w wersji 2.0 do 3.5 uwzględnione *formantów mobilnych ASP.NET*: zestaw formantów serwera dla urządzeń przenośnych w *System.Web.Mobile.dll* zestawu i  *System.Web.UI.MobileControls* przestrzeni nazw. Zestaw jest dostępny w ASP.NET 4, ale jest przestarzały. Deweloperom doradza się przeprowadzić migrację do bardziej nowoczesnego podejścia, takich jak opisane w tym dokumencie.

Powód, dlaczego formantów mobilnych ASP.NET została oznaczona jako przestarzała jest, że ich projektu jest zorientowany na wokół telefony komórkowe, które były wspólnego wokół 2005 i wcześniejszych. Formanty przede wszystkim są przeznaczone do renderowania kodu znaczników WML lub cHTML (zamiast regularnego HTML) dla przeglądarek WAP, że ery. Ale WAP, WML i cHTML nie są już odpowiednie dla większości projektów, ponieważ kodu HTML teraz stał się wszechobecne znaczników języka dla przeglądarek mobilnych i klasycznych podobne.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Wyzwania związane z obecnie obsługi urządzeń przenośnych

Mimo że teraz przeglądarki dla urządzeń przenośnych obsługują powszechnie HTML, nadal będą występować wiele wyzwań podczas mające na celu tworzenie wspaniałych środowisk przeglądania mobilnych:

- ***Rozmiar ekranu*** — urządzeń przenośnych różnią się znacznie w formularzu, a ich ekrany często są znacznie mniejsze niż monitory pulpitu. Dlatego należy do projektowania układów całkowicie różne strony dla nich.
- ***Dane wejściowe metody*** — niektóre urządzenia mają dominują, niektóre z nich mają styluses, inne osoby za pomocą dotyku. Może być konieczne należy wziąć pod uwagę nawigacji wiele mechanizmów metody i danych wejściowych.
- ***Zgodność ze standardami*** — wiele przeglądarek dla urządzeń przenośnych nie obsługują najnowszych standardów HTML, CSS i JavaScript.
- ***Przepustowość*** — wydajność sieci danych komórkowych zmienia się bardzo popularny i niektórzy użytkownicy końcowi znajdują się na taryf, które opłaty wg megabajtów.

Istnieje rozwiązanie opracowanie; Aplikacja musi wyglądały i zachowywały się różnie zgodnie z urządzeń, uzyskiwanie do niej dostępu. W zależności od stopnia obsługę operacji mobilnych, należy to być większe wyzwanie dla deweloperów sieci web niż kiedykolwiek było pulpitu "gwiezdnych przeglądarki".

Deweloperzy często początkowo zbliża się obsługa przeglądarek mobilnych po raz pierwszy wydaje się, że jest tylko ważne do obsługi smartfonów najnowsze i najbardziej zaawansowane (np. Windows Phone 7, telefonu iPhone lub Android), może być, ponieważ deweloperzy często osobiście takie właścicielem urządzenia. Telefony tańsze nadal są bardzo popularne i ich właścicieli ich używać do przeglądania sieci web — w szczególności w krajach, w których łatwiej niż połączenie szerokopasmowe telefonów komórkowych. Należy wybrać zakres urządzeń na potrzeby obsługi, biorąc pod uwagę jej klientom prawdopodobnie Twojej firmy. Jeśli tworzysz online innej, spa kondycji luksusowe można wprowadzać decyzji biznesowych tylko do docelowych zaawansowane smartfonów, dlatego jeśli tworzysz system rezerwacji biletów dla kin prawdopodobnie trzeba będzie konta dla użytkowników zewnętrznych z mniej zaawansowanych funkcji telefony.

## <a name="architectural-options"></a>Opcje architektury

Przed przejściem do szczegółowych informacji technicznych formularzy sieci Web ASP.NET lub MVC, należy zauważyć, że Deweloperzy sieci web w zasadzie trzy główne sposoby, aby obsługa przeglądarek urządzeń przenośnych:

1. ***Nic nie rób —*** można po prostu utworzyć aplikację sieci web standard, zorientowanych na pulpicie i zależą od przeglądarki dla urządzeń przenośnych do jej akceptowalny sposób renderowania. 

    - **Zaletą**: jest najtańszej opcji wdrożenia i obsługa — działają bez żadnych dodatkowych
    - **Wadą**: zapewnia najgorszy środowisko użytkownika końcowego: 

        - Najnowsze smartfonów może spowodować HTML równie dobrze jak przeglądarki na komputerze, ale użytkownicy nadal będą zmuszeni do powiększania i być przewijane w poziomie i w pionie do korzystania z zawartości na małym ekranie. To nie jest optymalne.
        - Starsze urządzenia i telefony funkcji może zakończyć się niepowodzeniem do renderowania znaczników w zadowalający sposób.
        - Nawet na najnowszych urządzeniach tablet (których ekranach może być tak dużego jak ekranów komputerów przenośnych) interakcji z innej reguły. Oparte na dotyku dane wejściowe sprawdza się najlepiej w przypadku większych przycisków i łączy rozprzestrzeniania się dalej od siebie, a nie istnieje sposób, aby umieść kursor myszy nad menu wysuwanego.
2. ***Rozwiązuje problem na komputerze klienckim* —** przy użyciu dokładnej CSS i [stopniowym rozszerzaniu funkcji](http://en.wikipedia.org/wiki/Progressive_enhancement) możesz utworzyć kod znaczników, stylów i skrypty, które się do dowolnych przeglądarki je uruchomiono. Na przykład za pomocą [zapytaniami multimediów CSS 3](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), można utworzyć układ wielokolumnowych, który jest przekształcany układ jedną kolumnę na urządzeniach, na których ekrany są węższe niż wybrany próg. 

    - **Zaletą**: optymalizuje renderowania dla określonego urządzenia na użytek, jeszcze nieznanych przyszłych urządzeń zgodnie z dowolnego ekranu i właściwości danych wejściowych mają
    - **Zaletą**: pozwala łatwo udostępniać logikę po stronie serwera w przypadku wszystkich typów urządzeń — minimalny powielania kodu lub nakładu pracy
    - **Wadą**: urządzenia przenośne tak różnią się od urządzeń komputerowych, że może być naprawdę potrzebujesz swoje stron dla urządzeń przenośnych całkowicie różni się od strony pulpitu przedstawiający różne informacje. Te różnice mogą być mało wydajne i niemożliwe do osiągnięcia niezawodnie za pomocą CSS samodzielnie, szczególnie biorąc pod uwagę sposób niespójnie starszych urządzeń interpretowania reguły CSS. Jest to szczególnie istotne, atrybutów CSS 3.
    - **Wadą**: nie obsługuje różnych logiki po stronie serwera i przepływów pracy dla różnych urządzeń. Uproszczone zakupy koszyka wyewidencjonowania przepływ pracy dla użytkowników urządzeń przenośnych nie na przykład zaimplementować przy użyciu CSS samodzielnie.
    - **Wadą**: wykorzystanie przepustowości nieefektywne. Serwer może być konieczne przesyłać znaczników i style, które są stosowane do wszystkich możliwych urządzeniach, nawet jeśli urządzenie będzie używać tylko podzbiór tych informacji.
3. ***Rozwiązuje problem na serwerze* —** Jeśli serwer wie, jakiego urządzenia uzyskuje dostęp do — lub w co najmniej właściwości danego urządzenia, takie jak rozmiar ekranu i metodę wprowadzania i czy jest to urządzenie przenośne — można uruchomić w niej inną logikę i dane wyjściowe różny kod znaczników HTML. 

    - **Zaletą:** maksymalną elastyczność. Nie ma żadnego limitu, ile różnią się w logice po stronie serwera dla przenośne lub zoptymalizować znaczników dla układu żądaną, specyficzne dla urządzenia.
    - **Zaletą:** wydajne przepustowości. Należy przesyłać znaczników i informacje dotyczące stylu, który zamierza użyć urządzenia docelowego.
    - **Wadą:** czasami wymusza powtórzenia nakład pracy lub kodu (np. dzięki czemu można tworzyć kopie podobnym, lecz nieco stron formularzy sieci Web lub widoków MVC). Gdzie to możliwe, który będzie wyodrębnić wspólnej logiki do odpowiedniej warstwy lub usługi, ale nadal niektóre części kodu interfejsu użytkownika lub znaczników może mieć zduplikowanych i następnie przechowywane w sposób równoległy.
    - **Wadą:** wykrywania urządzeń nie jest proste. On wymaga listy lub bazy danych typów znanych urządzeń i ich właściwości (które nie zawsze są całkowicie aktualne) i nie jest gwarantowana, aby dokładnie dopasować każdego żądania przychodzącego. W tym dokumencie opisano niektóre opcje i ich przeszkody w dalszej części.

Aby uzyskać najlepsze wyniki, większość deweloperów Znajdź potrzebne im łączyć opcji (2) i (3). Niewielkie różnice stylistyczne najlepiej są obsługiwane na komputerze klienckim przy użyciu CSS lub nawet języku JavaScript, istotne różnice w danych, przepływ pracy lub znaczników najbardziej efektywne są wdrażane w kodzie po stronie serwera.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Ten dokument zawiera omówienie metod po stronie serwera

Ponieważ MVC i formularzy sieci Web platformy ASP.NET są obie technologie przede wszystkim po stronie serwera, ten dokument koncentruje się na technik po stronie serwera, które pozwalają tworzyć różny kod znaczników i logikę przeglądarki dla urządzeń przenośnych. Oczywiście można także połączyć to przy użyciu dowolnej techniki po stronie klienta (np. CSS 3 zapytaniami multimediów, rozszerzenie progresywnego JavaScript), ale jest bardziej kwestią projekt sieci web niż projektowanie programu ASP.NET.

## <a name="browser-and-device-detection"></a>Wykrywanie przeglądarki i urządzenia

Kluczowe wstępnym dla wszystkich metod po stronie serwera do obsługi urządzeń przenośnych jest wiedzieć, jakiego urządzenia, które korzysta z obiektu odwiedzającego. W rzeczywistości jeszcze lepsze niż wiedza, producent i model liczbę tego urządzenia jest uświadomienie *charakterystyki* urządzenia. Właściwości mogą obejmować:

- Czy urządzenie przenośne?
- Metoda wprowadzania (mysz i klawiatura, dotykowe, klawiatury, joystick,...)
- Rozmiar ekranu (fizycznie i w pikselach)
- Obsługiwane formaty mediów i danych
- Etc.

Zaleca się decyzje na podstawie charakterystyki niż numer modelu, ponieważ, a następnie będzie lepiej wyposażone w celu obsługi przyszłych urządzeń.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Za pomocą stron ASP. Obsługa wykrywania przeglądarki wbudowanej w sieci

Deweloperzy i formularzy sieci Web platformy ASP.NET MVC można natychmiastowego poznania istotne cechy zaproszonych przeglądarki, sprawdzając właściwości *Request.Browser* obiektu. Na przykład zobacz

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ...a wielu innych

W tle, platforma ASP.NET jest zgodna z przychodzącego *User-Agent* nagłówka HTTP (UA) dla wyrażenia regularne w zestawie plików XML definicji przeglądarki. Domyślnie ta platforma obejmuje definicje wiele wspólnych urządzeń przenośnych, a następnie można dodać niestandardowe pliki definicji przeglądarki dla innych osób, które mają aby rozpoznawał. Aby uzyskać więcej informacji, zobacz stronę MSDN [formanty serwera sieci Web ASP.NET i możliwości przeglądarki](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Przy użyciu bazy danych urządzenia WURFL za pośrednictwem 51Degrees.mobi Foundation

Podczas gdy ASP. Wsparcie w zakresie wykrywania przeglądarki wbudowanej na NET będą wystarczające dla wielu aplikacji istnieją dwa główne przypadki podczas może nie być wystarczająca:

- ***Aby rozpoznać najnowszych urządzeniach***(bez ręcznego tworzenia plików definicji przeglądarki dla nich). Należy pamiętać, że pliki definicji przeglądarki .NET 4 nie są wystarczająco aktualne, rozpoznawał Windows Phone 7, telefony z systemem Android, programie Opera Mobile przeglądarki lub urządzenia Ipad firmy Apple.
- ***Potrzebujesz bardziej szczegółowe informacje dotyczące możliwości urządzenia***. Musisz wiedzieć o metodę wprowadzania urządzenia (np. dotyk vs klawiatury numerycznej) lub jakie audio formatuje przeglądarka obsługuje. Te informacje są niedostępne w standardowych plikach definicji przeglądarki.

[ *Uniwersalny plik zasobów sieci bezprzewodowej* projektu (WURFL)](http://wurfl.sourceforge.net/) obsługuje znacznie więcej aktualności i szczegółowych informacji dotyczących urządzeń przenośnych w użyciu już dziś.

Dobra wiadomość dla deweloperów platformy .NET jest ASP. Funkcji wykrywania przeglądarki firmy NET jest rozszerzalny, więc istnieje możliwość rozbudowy, aby wyeliminować te problemy. Na przykład można dodać typu open source [ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection) biblioteki do projektu. Jest ASP.NET IHttpModule lub przeglądarki możliwości dostawcy (można go używać w aplikacji MVC i formularzy sieci Web), który bezpośrednio odczytuje dane WURFL i przyczepia go do ASP. Mechanizm wykrywania przeglądarki wbudowanej w sieci. Po zainstalowaniu modułu, *Request.Browser* nagle będzie zawierać znacznie bardziej dokładne i szczegółowe informacje: będzie poprawnie rozpoznać wieloma typami urządzeń wymienionych wcześniej i wyświetlanie listy ich możliwości (w tym dodatkowe funkcje takie jak metoda wprowadzania). W dokumentacji projektu Aby uzyskać więcej informacji.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Jak aplikacji formularzy sieci Web może powodować mobile specyficznych stron

Domyślnie poniżej przedstawiono sposób wyświetlania zupełnie nowa aplikacja formularzy sieci Web na urządzeniach przenośnych wspólne:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Wyraźnie widać ani układu wygląda bardzo przyjazne dla urządzeń przenośnych — ta strona została opracowana dla dużych, zorientowanych na pozioma monitora nie na małym ekranie orientacji pionowej. Dlatego co można zrobić na jego temat?

Jak wspomniano wcześniej w tym dokumencie, istnieje wiele sposobów, aby dostosować strony dla urządzeń przenośnych. Kilka technik są oparte na serwerze, innym uruchomienia na kliencie.

### <a name="creating-a-mobile-specific-master-page"></a>Tworzenie strony wzorcowej specyficzne dla mobile

W zależności od wymagań można użyć tego samego formularzy sieci Web dla wszystkich użytkowników zewnętrznych, ale masz dwa oddzielne strony wzorcowe: jeden dla użytkowników pulpitu, drugie odwiedziny na urządzeniach mobilnych. Zapewnia to elastyczność zmiany stylów CSS lub najwyższego poziomu znaczników HTML do własnych urządzeń przenośnych bez zduplikowane wszelka logika strony.

Jest to proste. Na przykład można dodać program obsługi PreInit podobny do następującego formularza sieci Web:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Teraz Utwórz stronę wzorcową, o nazwie Mobile.Master w folderze najwyższego poziomu w aplikacji i będzie on używany w przypadku wykrycia urządzenia przenośnego. Stronę wzorcową mobilnych można odwoływać arkusz stylów CSS specyficzne dla mobilnych, w razie potrzeby. Goście pulpitu będą również widzieć domyślnej strony głównej, nie przenośnych jeden.

### <a name="creating-independent-mobile-specific-web-forms"></a>Tworzenie niezależnych formularzy sieci Web specyficzne dla mobile

Aby zapewnić maksymalną elastyczność możesz znacznie więcej niż tylko posiadanie oddzielnych stron wzorcowych dla różnych typów urządzeń. Można zaimplementować dwa *całkowicie oddzielić zestawów stron formularzy sieci Web* — jednego zestawu, w przypadku przeglądarek komputerowych, innego zestawu dla przenośne. Ten sposób działa najlepiej, jeśli mają być przedstawiane bardzo różne informacje i przepływów pracy do odwiedziny na urządzeniach mobilnych. W pozostałej części tej sekcji opisano tego podejścia szczegółowo.

Przy założeniu, że masz już aplikację formularzy sieci Web, przeznaczony dla przeglądarek komputerowych, aby kontynuować najłatwiej Utwórz podfolder o nazwie "wyraz Mobile" w obrębie projektu i twórz swoje stron dla urządzeń przenośnych. Można skonstruować całej lokacji podrzędnych, za pomocą własnej strony wzorcowe, arkusze stylów i strony, przy użyciu tych samych technik, które są używane dla innych aplikacji formularzy sieci Web. Nie jest konieczna utworzyć przenośnych równorzędne do *co* strony w witrynie pulpitu; można wybrać podzbiór funkcji ma sens dla odwiedziny na urządzeniach mobilnych.

Twoje stron dla urządzeń przenośnych mogą udostępniać typowe zasoby statyczne (takich jak obrazy, JavaScript i CSS plików) za pomocą regularnego strony w razie potrzeby. Ponieważ folderu "wyraz Mobile" będzie *nie* można oznaczyć jako oddzielną aplikację w przypadku hostowania w usługach IIS (jest to prosty podfolder stron formularzy sieci Web), jego również udostępniać te same konfiguracji, dane sesji i innych infrastruktury jako usługi strony pulpitu.

> [!NOTE]
> Ponieważ takie podejście zazwyczaj polega na niektórych duplikatów kodu (stron dla urządzeń przenośnych prawdopodobnie może udostępnić pewne podobieństwa stron pulpitu), ważne jest, aby współczynnik żadnych wspólnych firm logiki lub dane kod dostępu do udostępnionej warstwy podstawowej lub usługi. W przeciwnym razie będziesz double nakład pracy podczas tworzenia i obsługi aplikacji.


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Przekierowywanie odwiedziny na urządzeniach mobilnych do usługi stron dla urządzeń przenośnych

Często jest to wygodne przekierować odwiedziny na urządzeniach mobilnych do stron dla urządzeń przenośnych tylko na *pierwszy* żądania w ich sesji przeglądania (a nie na każde żądanie w ich sesji), ponieważ:

- Następnie umożliwia łatwe odwiedziny na urządzeniach mobilnych dostępu stron pulpitu, sieci, gdy mają być — po prostu umieść łącze na stronie głównej, która przechodzi do "Wersja pulpitu". Nie można przekierować odwiedzający powrót do strony aplikacji mobilnej, ponieważ nie jest już pierwszego żądania w ich sesji.
- Takie rozwiązanie pomaga uniknąć ryzyka nie zakłócają żądania wszelkie dynamiczne zasobów udostępnionych między częściami komputerów i urządzeń przenośnych w lokacji (np. w przypadku typowych formularz sieci Web, zarówno komputerów i urządzeń przenośnych części witryny do wyświetlania w IFRAME lub niektórych obsługi technologii Ajax)

Aby to zrobić, możesz umieścić logikę przekierowania w **sesji\_Start** metody. Na przykład dodaj następującą metodę do pliku Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Konfigurowanie uwierzytelniania formularzy do przestrzegania swoje stron dla urządzeń przenośnych

Należy zwrócić uwagę na to, że uwierzytelnianie formularzy sprawia, że pewne założenia, o której go można skierować użytkowników podczas i po zakończeniu procesu uwierzytelniania:

- Po użytkownik wymaga uwierzytelniania, uwierzytelnianie formularzy spowoduje przekierowanie do strony logowania pulpitu, niezależnie od tego, czy są one użytkownikiem komputerze lub urządzeniu przenośnym (ponieważ ma on tylko koncepcji *jeden* adres URL logowania). Przy założeniu, że chcesz do nadawania stylu Twoja strona logowania przenośnych inaczej, należy zwiększyć Twoja strona logowania pulpitu tak, aby użytkownicy urządzeń przenośnych zostanie przekierowany na stronę logowania przenośnych oddzielne. Na przykład, Dodaj następujący kod, aby Twoje **pulpitu** kodem strony logowania: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Po pomyślnym zalogowaniu użytkownika, uwierzytelnianie formularzy będzie domyślnie przekierowanie do strony głównej pulpitu (ponieważ ma on tylko koncepcji *jeden* domyślna strona). Należy poprawić Twoja strona logowania mobilnych tak, aby zostanie przekierowany do strony głównej telefon komórkowy po pomyślnym zalogowaniu się w. Na przykład, Dodaj następujący kod, aby Twoje **przenośnych** kodem strony logowania: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  Ten kod zakłada, że Twoja strona zawiera formant serwera logowania o nazwie odwoływały, tak jak w domyślnym szablonie projektu.

### <a name="working-with-output-caching"></a>Praca z buforowania danych wyjściowych

Jeśli używasz, buforowanie danych wyjściowych, należy pamiętać, że domyślnie, możliwe, że pulpitu użytkownika, aby znaleźć niektórych adresu URL (co powoduje jego dane wyjściowe pamięci podręcznej), zostały wykonane przez użytkownika mobilnego, który odbiera pamięci podręcznej danych wyjściowych z pulpitu. To ostrzeżenie dotyczy zarówno one po prostu zmieniającego się Strona wzorcowa według typu urządzenia lub implementacji całkowicie oddzielnych formularzach sieci Web na typ urządzenia.

Aby uniknąć tego problemu, możesz wydać polecenie ASP.NET różnicującej wpisu pamięci podręcznej według tego, czy obiekt odwiedzający jest na urządzeniu przenośnym. Dodaj parametr Element VaryByCustom do strony swojego OutputCache deklaracji w następujący sposób:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

Następnie zdefiniuj *isMobileDevice* jako niestandardowy pamięć podręczną parametr, dodając następującą metodę zastąpienie pliku Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Pozwoli to zagwarantować, że odwiedziny na urządzeniach mobilnych do strony nie pojawić się dane wyjściowe wcześniej umieścić w pamięci podręcznej przez obiekt odwiedzający pulpitu.

### <a name="a-working-example"></a>Praktyczny przykład

Aby wyświetlić te techniki w działaniu, Pobierz [przykładów kodu w tym oficjalnym dokumencie](https://docs.microsoft.com/aspnet/mobile/overview). Przykładowa aplikacja formularzy sieci Web automatycznie przekierowuje użytkowników urządzeń przenośnych z zestawem mobile specyficznych stron w podfolderze o nazwie Mobile. Znaczniki i stylów tych stron jest lepiej zoptymalizowany pod kątem przeglądarek dla urządzeń przenośnych, jak widać na poniższych zrzutach ekranu:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Aby uzyskać więcej porad na temat optymalizowania swoje znaczników i arkusze CSS dla przeglądarki dla urządzeń przenośnych zobacz sekcję "Stylu stron dla urządzeń przenośnych dla przeglądarki dla urządzeń przenośnych" w dalszej części tego dokumentu.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Jak aplikacje programu ASP.NET MVC można przedstawić mobile specyficznych stron

Ponieważ wzorzec Model-View-Controller oddziela logiki aplikacji (w kontrolerach) od logiki prezentacji (w widokach), można za pomocą dowolnego z następujących metod do obsługi obsługę operacji mobilnych w kodzie po stronie serwera:

1. ***Przy użyciu tego samego kontrolerów i widoków dla przeglądarek, zarówno komputerów i urządzeń przenośnych, ale renderowania widoków przy użyciu różnych układów Razor, w zależności od typu urządzenia *.** Ta opcja sprawdza się najlepiej, jeśli jest wyświetlanie danych taka sama na wszystkich urządzeniach, ale po prostu chcesz podać inną arkuszy stylów CSS lub zmienić kilka elementów HTML najwyższego poziomu dla przenośne.
2. ***Użyj tego samego kontrolerów dla przeglądarek, zarówno komputerów i urządzeń przenośnych, ale renderowania różnych widoków w zależności od typu urządzenia***. Ta opcja działa najlepiej, jeśli jesteś około wyświetlania tych samych danych i zapewniając tym samym przepływów pracy dla użytkowników końcowych, ale ma być renderowany bardzo różny kod znaczników HTML do własnych urządzenie używane.
3. ***Utwórz oddzielne obszary dla przeglądarek komputerów i urządzeń przenośnych, wdrażanie niezależny kontrolery i widoki dla każdego *.** Ta opcja sprawdza się najlepiej, jeśli wyświetlanie bardzo różnych ekranach, zawierające różne informacje i początkowe użytkownika za pomocą różnych przepływów pracy, zoptymalizowane pod kątem ich typ urządzenia. Może oznaczać, że niektóre powtórzenia kodu, ale można zminimalizować, który, uwzględniając limitu wspólnej logiki do warstwy podstawowej lub usługi.

Jeśli chcesz móc **pierwszy** opcji i różnią się tylko układ Razor na typ urządzenia jest bardzo proste. Po prostu zmodyfikuj swoje \_ViewStart.cshtml pliku w następujący sposób:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Teraz możesz utworzyć układ specyficzne dla mobile, o nazwie \_LayoutMobile.cshtml ze strukturą strony i arkusze CSS reguł, zoptymalizowane pod kątem urządzeń przenośnych.

Jeśli chcesz móc **drugi** opcji renderowania widoków zupełnie różne zależnie od typu urządzenia zewnętrznego, zobacz [wpis w blogu Scotta Hanselmana](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

Pozostała część ten dokument koncentruje się na **trzeci** opcja — tworzenie oddzielnych kontrolerów *i* widoków dla urządzeń przenośnych — dzięki czemu można kontrolować, dokładnie podzbiór funkcji jest oferowana w przypadku odwiedziny na urządzeniach mobilnych.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Konfigurowanie mobilnej obszaru w aplikacji ASP.NET MVC

Można dodać obszaru o nazwie wyraz "Mobile" do istniejącej aplikacji platformy ASP.NET MVC w normalny sposób: kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, a następnie wybierz pozycję Dodaj, a obszar. Następnie można dodać widoków i kontrolerów podobnie jak w przypadku innych obszarów, w ramach aplikacji ASP.NET MVC. Na przykład dodać do usługi mobilnej obszaru nowy kontroler o nazwie HomeController ma działać jako stronę główną dla odwiedziny na urządzeniach mobilnych roboczej.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Zapewnienie /Mobile adresu URL osiągnie przenośnych strony głównej

Chcąc /Mobile adres URL do osiągnięcia akcji indeksu na HomeController wewnątrz obszaru mobilnych, konieczne będzie wprowadź dwie niewielkich zmian w konfiguracji usługi routingu. Najpierw zaktualizuj klasy MobileAreaRegistration tak, aby HomeController domyślny kontroler w Twojej okolicy przenośne, jak pokazano w poniższym kodzie:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

Oznacza to, przenośne strony głównej teraz powinien znajdować się w /Mobile zamiast/Mobile domowych, ponieważ "Home" jest teraz niejawnie domyślna nazwa kontrolera dla stron dla urządzeń przenośnych.

Następnie należy pamiętać, że dodając HomeController drugi do aplikacji (czyli mobilnych, oprócz istniejącego pulpitu jeden), będzie Przerwano regularne strona główna pulpitu. Zakończy się niepowodzeniem z powodu błędu "*znaleziono wiele typów zgodnych kontroler o nazwie"Home"*". Aby rozwiązać ten problem, zaktualizuj konfigurację routingu najwyższego poziomu (w Global.asax.cs) do określenia Twojej pulpitu HomeController powinno zająć priorytet po niejednoznaczności:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Teraz błąd zaczną away i adresu URL http://<em>twoja_witryna</em>/ będzie korzystał z strony głównej pulpitu i http://<em>twoja_witryna</em>/mobile/ będzie korzystał z przenośnymi strony głównej.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Przekierowywanie odwiedziny na urządzeniach mobilnych do usługi mobilnej obszaru

Istnieje wiele punktów rozszerzeń innej we wzorcu ASP.NET MVC, więc istnieje wiele sposobów możliwych do dodania logiki przekierowania. Jedną z opcji ładnie jest utworzyć atrybutu filtru, [RedirectMobileDevicesToMobileArea], który wykonuje przekierowanie, jeśli są spełnione następujące warunki:

1. Pierwsze żądanie jest w sesji użytkownika (czyli Session.IsNewSession jest równa true)
2. Żądanie pochodzi z przeglądarce dla urządzeń przenośnych (czyli Request.Browser.IsMobileDevice jest równa true)
3. Użytkownik już nie żąda zasobu, w obszarze przenośnych (czyli *ścieżki* w adresie URL nie rozpoczyna się od /Mobile)

Do pobrania próbki, dołączone ten dokument zawiera implementację tę logikę. Są one zaimplementowane jako filtru autoryzacji, pochodzi od klasy AuthorizeAttribute, co oznacza, że jej poprawnego działania nawet wtedy, gdy używasz buforowania danych wyjściowych (w przeciwnym razie, jeśli pulpitu odwiedzający pierwszy uzyskuje dostęp do niektórych adresu URL, pulpitu dane wyjściowe mogą być buforowane i następnie obsłużonych kolejne odwiedziny na urządzeniach mobilnych).

Ponieważ jest filtr, możesz wybrać dowolną stosować je do określonych kontrolerów i akcji, np.

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… lub można zastosować do wszystkich kontrolerów i akcji jako MVC 3 *filtrów globalnych* w pliku Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

Przykład pliku do pobrania ilustruje też sposób tworzenia podklasy tego atrybutu, które przekierowują do określonych lokalizacji w Twojej okolicy mobilnych. Oznacza to, na przykład, możesz:

- Zarejestrowanie filtru globalnego jak pokazano powyżej wysyłający odwiedziny na urządzeniach mobilnych na stronie głównej przenośnych domyślnie.
- Dotyczą również specjalne [RedirectMobileDevicesToMobileProductPage] filtru akcji "Wyświetl produkt", która przyjmuje odwiedziny na urządzeniach mobilnych do mobilnej wersji niezależnie od produktu one miały żądanej strony.
- Dotyczą również innych specjalnych podklasy filtr konkretne akcje, przekierowywanie odwiedziny na urządzeniach mobilnych do odpowiedniej strony mobilnych

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Konfigurowanie uwierzytelniania formularzy do przestrzegania swoje stron dla urządzeń przenośnych

Jeśli używasz uwierzytelniania formularzy, należy pamiętać, że gdy użytkownik zechce się zalogować, go automatycznie przekierowuje użytkownika do pojedynczego określonego "Zaloguj" adresu URL, czyli domyślnie **/konta/logowania**. Oznacza to, że użytkownicy urządzeń przenośnych może zostać przekierowany do akcji pulpitu "Zaloguj".

Aby uniknąć tego problemu, należy dodać logikę do klasycznych akcji "Zaloguj", aby użytkownicy urządzeń przenośnych zostanie przekierowany ponownie do mobilnych akcji "Zaloguj". Jeśli używasz domyślnego szablonu aplikacji platformy ASP.NET MVC, zaktualizuj działań logowania tego elementu AccountController w następujący sposób:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… a następnie wdrożyć odpowiedniej akcji "Zaloguj" mobile określonych w kontrolerze o nazwie elementu AccountController w Twojej okolicy mobilnych.

### <a name="working-with-output-caching"></a>Praca z buforowania danych wyjściowych

Jeśli używasz filtru [OutputCache], należy wymusić wpisu pamięci podręcznej będzie się różnić od typu urządzenia. Na przykład napisać:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Następnie dodaj następującą metodę do pliku Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Pozwoli to zagwarantować, że odwiedziny na urządzeniach mobilnych do strony nie pojawić się dane wyjściowe wcześniej umieścić w pamięci podręcznej przez obiekt odwiedzający pulpitu.

### <a name="a-working-example"></a>Praktyczny przykład

Aby wyświetlić te techniki w działaniu, Pobierz [kod ten oficjalny dokument skojarzony przykłady](https://docs.microsoft.com/aspnet/mobile/overview). Przykład obejmuje aplikację ASP.NET MVC 3 (Wersja Release Candidate) ulepszony na potrzeby obsługi urządzeń przenośnych za pomocą metod opisanych powyżej.

## <a name="further-guidance-and-suggestions"></a>Dalsze wskazówki i sugestie

Następujące dyskusji mają zastosowanie zarówno do deweloperów MVC, którzy korzystają z technik w niniejszym dokumencie i formularzy sieci Web.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Udoskonalanie logikę przekierowania za pomocą 51Degrees.mobi Foundation

Logika przekierowania opisane w tym dokumencie może być całkowicie wystarczający dla aplikacji, ale nie będzie działać, jeśli chcesz wyłączyć sesje lub za pomocą przeglądarki dla urządzeń przenośnych, które odrzucić pliki cookie (muszą one nie może być sesji), ponieważ nie będzie wiadomo, czy danego żądania jest pierwsza od tej osoby odwiedzającej.

Wiesz już, jak poprawić dokładność ASP 51Degrees.mobi "open source" Foundation. Wykrywanie przeglądarki przez sieć. Ma wbudowaną funkcję, aby skierować użytkowników mobilnych na określonych lokalizacjach skonfigurowany w pliku Web.config. Jest w stanie działać bez zależności od tego, w sesji programu ASP.NET (i w związku z tym pliki cookie), przechowując dziennika tymczasowych skrótów odwiedzających nagłówków HTTP i adresów IP, dlatego wie czy każdego żądania jest to pierwsza z danym vistor.

Następujący element dodany do sekcji fiftyOne pliku web.config będzie przekierowywać pierwsze żądanie wykrytego urządzenia przenośnego do strony ~ / Mobile/Default.aspx. Wszystkie żądania do stron w folderze przenośnych będą *nie* przekierowanie, niezależnie od typu urządzenia. Jeśli urządzenie przenośne jest nieaktywny przez okres 20 minut lub większej liczby urządzeń spowoduje usunięcie i kolejnych żądań będzie traktowane jako nowe na potrzeby przekierowywania.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Aby uzyskać więcej informacji, zobacz [51degrees.mobi dokumentacji Foundation](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> Możesz *można* Foundation 51Degrees.mobi Użyj funkcji Przekierowanie na aplikacje programu ASP.NET MVC, ale należy zdefiniować konfigurację przekierowania pod względem zwykły adresów URL, nie w zakresie routingu parametrów lub poprzez umieszczenie filtrów platformy MVC w akcji. Jest to spowodowane (w momencie pisania) nie może rozpoznać 51Degrees.mobi Foundation filtry lub routingu.


### <a name="disabling-transcoders-and-proxy-servers"></a>Wyłączanie Transkoderów i serwerów Proxy

Operatory sieci komórkowej mają dwa cele szerokiego w ich podejście do mobilnych internet.

1. Podaj jako znacznie odpowiedniej zawartości, jak to możliwe
2. Zwiększenie liczby klientów, którzy mogą udostępniać radio ograniczonej przepustowości sieci

Ponieważ większość stron sieci web zostały zaprojektowane dla dużych ekranów o rozmiarze pulpitu i połączenia szerokopasmowego szybko stała wierszy, użyć wielu operatorów *transkoderów* lub *serwerów proxy* , dynamicznie zmieniać zawartość sieci web. Mogą oni modyfikować swoje kod znaczników HTML i CSS, aby odpowiadały na mniejszych ekranach (szczególnie w przypadku "Funkcja telefony" braku mocy obliczeniowej do obsługi złożonych układów), a ich może ponownie kompresować obrazów (znacznemu ograniczeniu ich jakość) w celu zwiększenia szybkości dostarczania strony.

Ale jeśli wykorzystaliście nakład pracy w celu wygenerowania zoptymalizowanych pod kątem mobile wersji lokacji prawdopodobnie nie chcesz, operatorem sieci kolidować z niego dowolny dalsze. Można dodać następujący wiersz do strony\_załadowane zdarzenie w dowolnej formie sieci Web platformy ASP.NET:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

Lub dla kontroler składnika ASP.NET MVC można dodać następujące zastąpienie metody, tak aby dotyczyła wszystkich akcji na tym kontrolerze:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

Wynikowy komunikatu HTTP informuje transkoderów zgodne W3C i serwery proxy, aby nie zmieniać zawartość. Oczywiście nie ma żadnej gwarancji, Operatorzy sieci komórkowej zachowuje tę wiadomość.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Ustawianie stylów stron dla urządzeń przenośnych dla przeglądarki dla urządzeń przenośnych

Jest to poza zakres tego dokumentu opisano szczegółowo jakiego rodzaju pracę kod znaczników HTML poprawnie lub które technik projektu sieci web zmaksymalizować użyteczności na poszczególnych urządzeń. Się użytkownikowi w celu znalezienia wystarczająco prostym układzie, optymalizacji dla ekranu o rozmiarze mobile, bez używania zawodnych HTML i CSS, pozycjonowanie wskazówki. Jest jednak jeden ważną technikę warte wskazujące, *okienka ekranu metatag*.

Niektóre nowoczesne przeglądarki dla urządzeń przenośnych, na stronach sieci web wyświetlaną nakład pracy przeznaczone dla pulpitu monitorów renderowania strony na kanwie wirtualnych również o nazwie "okienka ekranu" (np. wirtualnych okienko ekranu jest 980 pikseli na urządzeniu iPhone oraz 850 pikseli w programie Opera Mobile domyślnie) i następnie Skaluj wynik w dół w celu dopasowania go do pikselach fizycznych. Użytkownik może, a następnie powiększanie i przesuwanie tego okienka ekranu. Ma tę zaletę, że umożliwia ona przeglądarki wyświetlenia strony w jego zamierzone układ, ale jest również ma wadą wymusza powiększanie i przesuwanie, który jest wygodne, dla użytkownika. Projektując aplikacje mobilne dla zaleca się projektowanie wąskie ekranu tak, aby bez powiększanie i przewijanie w poziomie jest niezbędne.

Umożliwia informowanie przeglądarce dla urządzeń przenośnych, jak szeroki powinien być okienka ekranu jest za pomocą niestandardowych *okienka ekranu* meta tag. Na przykład, jeśli Dodaj następujący kod do sekcji HEAD na stronie

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… następnie Obsługa przeglądarek smartphone będzie układ strony na kanwie szeroki wirtualnego 480 pikseli. Oznacza to, że elementów HTML zdefiniować ich szerokości w ujęciu procentowym, wartości procentowe będą interpretowane względem szerokość piksela 480 ta nie domyślnej szerokości okienka ekranu. W rezultacie użytkownik jest mniej prawdopodobne na powiększanie i przesuwanie poziomo — znacznie ulepszanie mobilnymi przeglądania.

Chcąc szerokość okienka ekranu, aby dopasować pikselach fizycznych urządzeń, można określić następujące czynności:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Aby to działało poprawnie, należy nie jawnie wymusić elementów przekracza tej szerokości (np. przy użyciu *szerokość* atrybutu lub właściwość CSS), wymuszają przeglądarki do użycia niezależnie od tego, większy okienka ekranu. Zobacz również: [więcej szczegółów na temat tagów niestandardowych okienka ekranu](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

Obsługa większości współczesnych smartfonów *podwójną orientacji*: mogą być przechowywane w trybie pionowej lub poziomej. Jest tak, nie należy wprowadzać założeń dotyczących szerokość ekranu w pikselach. Nie nawet przyjęto założenie, że jest stała szerokość ekranu, ponieważ użytkownik może ponownie poznaniu swojego urządzenia, bez przerywania ich na stronie.

Starsze urządzenia Windows Mobile i Blackberry może również przyjmować następujące tagi meta w nagłówku strony, aby poinformować zawartości zoptymalizowany dla urządzeń przenośnych i dlatego nie powinny zostać przekształcone.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Dodatkowe zasoby

Aby uzyskać listę urządzeń przenośnych emulatorów i symulatorów, można użyć do testowania mobilnych aplikacji sieci web ASP.NET, zobacz stronę [symulowanie testowanie popularnych urządzeń przenośnych](../mobile/device-simulators.md).

## <a name="credits"></a>Środki na korzystanie z

- Główny Autor: sanderson o Steven
- Recenzenci / dodatkowej zawartości autorzy: James Rosewell, Mikael Söderström, autorstwa Scotta Hanselmana i Scotta Huntera
