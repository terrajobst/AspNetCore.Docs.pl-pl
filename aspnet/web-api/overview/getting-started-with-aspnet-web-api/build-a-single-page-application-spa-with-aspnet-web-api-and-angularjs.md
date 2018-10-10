---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'Ćwiczenia praktyczne: Tworzenie aplikacji jednostronicowej (SPA) przy użyciu wzorca ASP.NET Web API i platformy Angular.js | Dokumentacja firmy Microsoft'
author: rick-anderson
description: W aplikacji sieci web tradycyjnych klienta (przeglądarki) inicjuje komunikację z serwerem, żądając strony. Serwer następnie przetwarza żądania...
ms.author: riande
ms.date: 09/30/2015
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 3c3557bb2be2807b11874937fcc629b5b773e463
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912257"
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Ćwiczenia praktyczne: Tworzenie aplikacji jednostronicowej (SPA) przy użyciu wzorca ASP.NET Web API i platformy Angular.js
====================
Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)

[Pobierz Camp Web szkolenia Kit](http://aka.ms/webcamps-training-kit)

> W aplikacji sieci web tradycyjnych klienta (przeglądarki) inicjuje komunikację z serwerem, żądając strony. Serwer następnie przetwarza żądanie i wysyła do klienta, HTML strony. W kolejnych interakcji ze stroną — np. użytkownik przechodzi do łącza lub przesyła formularz z danymi — nowe wezwanie jest wysyłane do serwera, a przepływ uruchamia ponownie: serwer przetwarza żądanie i wysyła nową stronę w przeglądarce w odpowiedzi na nowe żądanie akcji ED przez klienta.
> 
> W aplikacji jednostronicowej (źródła) całej strony jest ładowany w przeglądarce po wykonaniu początkowego żądania, ale kolejne interakcje odbywać się za pośrednictwem żądań Ajax. Oznacza to, że przeglądarka musi zaktualizować tylko części strony, który został zmieniony; nie ma potrzeby ponowne załadowanie całej strony. Podejście SPA ogranicza czas przez aplikację, aby odpowiedzieć na działania użytkownika, skutkuje bardziej płynne środowisko.
> 
> Architektura SPA obejmuje niektóre wyzwania, które nie są obecne w aplikacjach sieci web tradycyjnych. Jednak rozwijających się technologii, takich jak Web API platformy ASP.NET, JavaScript struktur, takich jak AngularJS, nowych funkcji stylów, dostarczonych przez CSS3 co tak naprawdę łatwe projektowanie i tworzenie aplikacji jednostronicowych.
> 
> W tym praktycznym laboratorium będą korzystać z tych technologii, aby zaimplementować maniaków komputerowych Quiz, witryny sieci Web elementy towarzyszące składni oparty na koncepcji SPA. Najpierw wdroży warstwy usług przy użyciu interfejsu API sieci Web platformy ASP.NET do udostępnienia wymagane punkty końcowe do pobierania na pytania quizu i przechowywania odpowiedzi. Następnie utworzysz bogaty i elastyczny interfejs użytkownika przy użyciu efektów przekształcania AngularJS i CSS3.
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


## <a name="overview"></a>Omówienie

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym praktyczne laboratorium dowiesz się jak:

- Tworzenie usługi ASP.NET Web API do wysyłania i odbierania danych JSON
- Tworzenie dynamicznego interfejsu użytkownika za pomocą biblioteki AngularJS
- Zwiększ możliwości interfejsu użytkownika za pomocą przekształcenia CSS3

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego laboratorium praktycznego niezbędne jest, następujące elementy:

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) lub nowszej

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

Aby można było uruchomić ćwiczeń opisanych w tym praktyczne laboratorium, należy najpierw skonfigurować swoje środowisko.

1. Otwórz Eksploratora Windows i przejdź do laboratorium **źródła** folderu.
2. Kliknij prawym przyciskiem myszy **plik Setup.cmd** i wybierz **Uruchom jako administrator** do uruchamiania procesu instalacji, który będzie skonfigurować środowisko i zainstalować fragmenty kodu programu Visual Studio, w tym środowisku laboratoryjnym.
3. Jeśli zostanie wyświetlone okno dialogowe kontroli konta użytkownika, upewnij się, działania, aby kontynuować.

> [!NOTE]
> Upewnij się, że wszystkie zależności w tym środowisku laboratoryjnym sprawdzeniu przed uruchomieniem Instalatora.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Za pomocą fragmentów kodu

W dokumencie laboratorium należy poinstruować można wstawiać bloki kodu. Dla wygody większość ten kod jest dostarczany jako Visual Studio fragmenty kodu, które są dostępne w Visual Studio 2013, aby uniknąć konieczności Dodaj ją ręcznie.

> [!NOTE]
> Każdy wykonywania towarzyszy początkowy rozwiązanie znajduje się w **rozpocząć** folderu ćwiczeniu, która umożliwia wykonanie każdego wykonywania niezależnie od innych. Należy pamiętać, że fragmenty kodu, które są dodawane podczas wykonywania brakuje te uruchamianie rozwiązań i może nie działać, dopóki nie zakończysz wykonywania. Wewnątrz kodu źródłowego dla ćwiczenia, można również znaleźć **zakończenia** folderu zawierającego rozwiązania programu Visual Studio z kodem, który powstały na skutek wykonaniu kroków w odpowiedniej wykonywania. Jeśli potrzebujesz dodatkowej pomocy, gdy pracujesz za pośrednictwem tego laboratorium praktycznego, można użyć jako wskazówki dotyczące tych rozwiązań.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To ćwiczenie praktyczne obejmuje następujących czynnościach:

1. [Tworzenie internetowego interfejsu API](#Exercise1)
2. [Tworzenie interfejsu SPA](#Exercise2)

Szacowany czas do ukończenia tego laboratorium: **60 minut**

> [!NOTE]
> Przy pierwszym uruchomieniu programu Visual Studio, należy wybrać jedną z kolekcji wstępnie zdefiniowanych ustawień. Każda kolekcja wstępnie zdefiniowanych służy do dopasowywania style rozwoju i określa układy okna, zachowanie edytora, fragmenty kodu IntelliSense i opcje w oknach dialogowych. Procedury przedstawione w tym środowisku laboratoryjnym opisano czynności niezbędnych do wykonywania danego zadania w programie Visual Studio, korzystając z **ogólnych ustawieniach projektowych** kolekcji. Jeśli wybierzesz kolekcji różne ustawienia dla swojego środowiska programowania, może być różnice w krokach, które należy wziąć pod uwagę.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Ćwiczenie 1: Tworzenie składnika Web API

Jednym z kluczowych części SPA jest warstwy usług. Jest odpowiedzialna za przetwarzanie wywołań Ajax, wysyłane przez interfejs użytkownika i zwracanie danych w odpowiedzi na to wywołanie. Dane pobrane powinno zostać wyświetlone w formacie czytelnym celu przeanalizowano i używane przez klienta.

W ramach interfejsu API sieci Web jest częścią stosu ASP.NET i jest przeznaczony do ułatwiają łatwe do wdrożenia usług HTTP, zwykle wysyłania i odbierania danych w formacie JSON lub XML za pomocą interfejsu API RESTful. W tym ćwiczeniu utworzysz witryny sieci Web do hostowania aplikacji maniaków komputerowych Quiz, a następnie wdrożyć usługi zaplecza do prezentowania i utrwalenia danych quiz, przy użyciu interfejsu API sieci Web platformy ASP.NET.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Zadanie 1 — Tworzenie początkowej projektu dla maniaków komputerowych Quiz

W ramach tego zadania będzie Tworzenie nowego projektu ASP.NET MVC z obsługą interfejsu API sieci Web platformy ASP.NET na podstawie **One ASP.NET** projektu typu, który jest dostarczany z programem Visual Studio. **One ASP.NET** łączy wszystkie technologie ASP.NET i daje możliwość mieszać i dopasowywać je zgodnie z potrzebami. Następnie dodasz klasy modelu Entity Framework i initializator bazy danych, aby wstawić pytania quizu.

1. Otwórz **Visual Studio Express 2013 for Web** i wybierz **pliku | Nowy projekt...**  można uruchomić nowego rozwiązania.

    ![Tworzenie nowego projektu](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "tworzenia nowego projektu")

    *Tworzenie nowego projektu*
2. W **nowy projekt** okno dialogowe, wybierz opcję **aplikacji sieci Web ASP.NET** w obszarze **Visual C# | Web** kartę. Upewnij się, że **.NET Framework 4.5** jest zaznaczone, nadaj jej nazwę *GeekQuiz*, wybierz **lokalizacji** i kliknij przycisk **OK**.

    ![Tworzenie nowego projektu aplikacji sieci Web ASP.NET](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "tworzenia nowego projektu aplikacji sieci Web ASP.NET")

    *Tworzenie nowego projektu aplikacji sieci Web ASP.NET*
3. W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **MVC** szablonu, a następnie wybierz **interfejsu API sieci Web** opcji. Ponadto upewnij się, że **uwierzytelniania** ustawiono opcję **indywidualne konta użytkowników**. Kliknij przycisk **OK** aby kontynuować.

    ![Tworzenie nowego projektu za pomocą szablonu MVC, w tym składników interfejsu API sieci Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Tworzenie nowego projektu za pomocą szablonu MVC, w tym składników interfejsu API sieci Web*
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **modeli** folderu **GeekQuiz** projektu, a następnie wybierz **Dodaj | Istniejący element...** .

    ![Dodawanie istniejącego elementu](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "Dodawanie istniejącego elementu")

    *Dodawanie istniejącego elementu*
5. W **Dodaj istniejący element** okno dialogowe, przejdź do **modele-źródłowe/zasoby** folder i wybierz pozycję wszystkie pliki. Kliknij przycisk **Dodaj**.

    ![Dodawanie zasobów modelu](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "Dodawanie zasobów modelu")

    *Dodawanie zasobów modelu*

    > [!NOTE]
    > Dodanie tych plików, dodajesz modelu danych, kontekst bazy danych programu Entity Framework i inicjatora bazy danych dla aplikacji Quiz maniaków komputerowych.
    > 
    > **Entity Framework (EF)** to maper obiektowo relacyjny (ORM), który pozwala na tworzenie aplikacji dostęp do danych przez programowania, korzystając z modelu koncepcyjnego aplikacji zamiast programowanie bezpośrednio przy użyciu schematu magazyn relacyjny. Dowiedz się więcej na temat platformy Entity Framework [tutaj](../../../entity-framework.md).
    > 
    > Poniżej przedstawiono opis klas, który właśnie został dodany:
    > 
    > - **TriviaOption:** reprezentuje pojedynczą opcję skojarzone z pytania quizu
    > - **TriviaQuestion:** reprezentuje pytania quizu i skojarzone opcje za pośrednictwem **opcje** właściwości
    > - **TriviaAnswer:** reprezentuje z opcją wybraną przez użytkownika w odpowiedzi na pytania quizu
    > - **TriviaContext:** reprezentuje kontekst bazy danych programu Entity Framework aplikacji Quiz maniaków komputerowych. Ta klasa jest pochodną **DContext** i udostępnia **DbSet** właściwości, które reprezentują kolekcjami jednostek opisanych powyżej.
    > - **TriviaDatabaseInitializer:** implementacji inicjator Entity Framework **TriviaContext** klasy, która dziedziczy z **CreateDatabaseIfNotExists**. Domyślnym zachowaniem tej klasy jest utworzyć bazę danych, tylko wtedy, gdy nie istnieje, wstawianie jednostek określona w **inicjatora** metody.
6. Otwórz **Global.asax.cs** pliku i dodaj następującą instrukcję using.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Dodaj następujący kod na początku **aplikacji\_Start** metodę, aby ustawić **TriviaDatabaseInitializer** jako inicjator bazy danych.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Modyfikowanie **Home** kontrolera, aby ograniczyć dostęp do uwierzytelnionych użytkowników. Aby to zrobić, otwórz **HomeController.cs** pliku wewnątrz **kontrolerów** folderze i Dodaj **Autoryzuj** atrybutu **HomeController**definicji klasy.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > **Autoryzuj** filtrować testy, aby zobaczyć, jeśli użytkownik jest uwierzytelniony. Jeśli użytkownik nie jest uwierzytelniony, zwracany jest kod stanu HTTP 401 (bez autoryzacji) bez wywoływania akcji. Można zastosować filtr, na całym świecie, na poziomie kontrolera lub na poziomie poszczególnych działań.
9. Będzie teraz dostosować układ stron sieci web i znakowanie. Aby to zrobić, otwórz  **\_Layout.cshtml** pliku wewnątrz **widoków | Udostępnione** folder i aktualizowanie zawartości **&lt;tytuł&gt;** elementu, zastępując *My ASP.NET Application* z *Quiz maniaków komputerowych* .

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. W tym samym pliku, należy zaktualizować na pasku nawigacyjnym, usuwając *o* i *skontaktuj się z pomocą* łącza i zmiana nazwy *Home* połączyć *Odtwórz*. Ponadto, Zmień nazwę *Nazwa aplikacji* połączyć *Quiz maniaków komputerowych*. Kod HTML dla paska nawigacyjnego powinien wyglądać podobnie do poniższego kodu.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Aktualizacja stopki strony układu, zastępując *My ASP.NET Application* z *Quiz maniaków komputerowych*. Aby to zrobić, Zastąp zawartość **&lt;stopki&gt;** element z następujący wyróżniony kod.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Zadanie 2 — Tworzenie interfejsu API sieci Web TriviaController

W poprzednim zadaniu został utworzony początkową strukturę aplikacji sieci web Quiz maniaków komputerowych. Teraz utworzysz prostą usługę interfejsu API sieci Web, korzysta z modelu danych quiz, która udostępnia następujące akcje:

- **GET/api/elementy towarzyszące składni**: pobiera następne pytanie, na liście quiz na udzielenie odpowiedzi przez uwierzytelnionego użytkownika.
- **POST/api/elementy towarzyszące składni**: przechowuje odpowiedzi quiz, określony przez uwierzytelnionego użytkownika.

Użyjesz narzędzia ASP.NET tworzenie szkieletów udostępniane przez program Visual Studio do utworzenia linii bazowej do klasy kontrolera interfejsu API sieci Web.

1. Otwórz **WebApiConfig.cs** pliku wewnątrz **aplikacji\_Start** folderu. Ten plik definiuje konfigurację usługi interfejsu API sieci Web, takich jak jak trasy są mapowane do akcji kontrolera interfejsu API sieci Web.
2. Dodaj następującą instrukcję using na początku pliku.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Dodaj następujący wyróżniony kod do **zarejestrować** metoda globalnie konfiguracji program formatujący dla danych JSON pobranych za pomocą metod akcji interfejsu API sieci Web.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > **CamelCasePropertyNamesContractResolver** automatycznie konwertuje nazw właściwości *pisane* przypadkach jest do Konwencja ogólne dla nazw właściwości w języku JavaScript.
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **kontrolerów** folderu **GeekQuiz** projektu, a następnie wybierz **Dodaj | Nowy element szkieletowy...** .

    ![Tworząc nowy element szkieletowy](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "tworząc nowy element szkieletowy")

    *Tworząc nowy element szkieletowy*
5. W **Dodawanie szkieletu** okna dialogowego pole, upewnij się, że **wspólnej** węzeł wybrano w okienku po lewej stronie. Następnie wybierz **kontroler internetowego interfejsu API 2 — pusty** szablonów w środkowym okienku i kliknij **Dodaj**.

    ![Wybór szablonu sieci Web API 2 kontrolera pusty](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "wybierając Web API 2 kontrolera pusty szablon")

    *Wybór szablonu pusty kontroler 2 interfejsu API sieci Web*

    > [!NOTE]
    > **Funkcja tworzenia szkieletu ASP.NET** jest struktura generowania kodu dla aplikacji sieci Web ASP.NET. Visual Studio 2013 obejmuje generatorów kodu wstępnie zainstalowane dla projektów MVC i interfejs API sieci Web. Tworzenie szkieletu należy używać w projekcie szybko dodać kod, który współdziała z modeli danych, aby zmniejszyć ilość czasu wymagane do rozwoju operacje na danych w warstwie standardowa.
    > 
    > Proces tworzenia szkieletów gwarantuje również, że wszystkie wymagane zależności są zainstalowane w projekcie. Na przykład jeśli rozpoczęcie pusty projekt programu ASP.NET, a następnie dodać Kontroler interfejsu API sieci Web za pomocą tworzenia szkieletu, wymagane pakiety NuGet interfejsu API sieci Web i odwołania są automatycznie dodawane do projektu.
6. W **Dodaj kontroler** okno dialogowe, typ *TriviaController* w **nazwy kontrolera** polu tekstowym i kliknij **Dodaj**.

    ![Dodawanie kontrolera elementy towarzyszące składni](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "dodawania kontrolera elementy towarzyszące składni")

    *Dodawanie kontrolera elementy towarzyszące składni*
7. **TriviaController.cs** plik zostanie następnie dodany do **kontrolerów** folderu **GeekQuiz** projektu, zawierającego pustą **TriviaController** klasy. Dodaj następujące instrukcje using na początku pliku.

    (Code Snippet — *TriviaControllerUsings AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Dodaj następujący kod na początku **TriviaController** klasy, aby zdefiniować, inicjowanie i usuwania **TriviaContext** wystąpienia w kontrolerze.

    (Code Snippet — *TriviaControllerContext AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > **Dispose** metody **TriviaController** wywołuje **Dispose** metody **TriviaContext** wystąpienia, co zapewnia, że wszystkie zasoby używane przez obiekt kontekstu są zwalniane, gdy **TriviaContext** wystąpienia jest usunięty lub zebranych elementów bezużytecznych. W tym zamyka wszystkie otwarte przez program Entity Framework połączenia z bazą danych.
9. Dodaj następującą metodę pomocnika na końcu **TriviaController** klasy. Ta metoda pobiera następujące pytania quizu z bazy danych na udzielenie odpowiedzi przez określonego użytkownika.

    (Code Snippet — *TriviaControllerNextQuestion AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Dodaj następujący kod **uzyskać** metody akcji, aby **TriviaController** klasy. Wywołuje się tą metodą akcji **NextQuestionAsync** metody pomocnika zdefiniowane w poprzednim kroku, aby pobrać następne pytanie dla tego uwierzytelnionego użytkownika.

    (Code Snippet — *TriviaControllerGetAction AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Dodaj następującą metodę pomocnika na końcu **TriviaController** klasy. Ta metoda określonej odpowiedzi są przechowywane w bazie danych i zwraca wartość logiczną wskazującą, czy odpowiedź jest poprawna.

    (Code Snippet — *TriviaControllerStoreAsync AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Dodaj następujący kod **wpis** metody akcji, aby **TriviaController** klasy. Tą metodą akcji kojarzy odpowiedzi użytkownika uwierzytelnionego i wywołania **StoreAsync** metody pomocnika. Następnie wysyła odpowiedź z wartością logiczną, zwracany przez metodę pomocnika.

    (Code Snippet — *TriviaControllerPostAction AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Modyfikowanie kontrolera interfejsu API sieci Web, aby ograniczyć dostęp do uwierzytelnionych użytkowników, dodając **Autoryzuj** atrybutu **TriviaController** definicji klasy.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Zadanie 3 — uruchamianie rozwiązania

W tym zadaniu zostanie zweryfikowana, czy usługi interfejsu API sieci Web, którą utworzoną w poprzednim zadaniu działa zgodnie z oczekiwaniami. Użyjesz programu Internet Explorer **narzędzi deweloperskich F12** do przechwytywania ruchu sieciowego i sprawdzić pełną odpowiedź z usługi interfejsu API sieci Web.

> [!NOTE]
> Upewnij się, że **programu Internet Explorer** wybrano **Start** znajdujący się na pasku narzędzi programu Visual Studio.
> 
> ![Opcja program Internet Explorer](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie. **Zaloguj** strony powinna zostać wyświetlona w przeglądarce.

    > [!NOTE]
    > Podczas uruchamiania aplikacji, jest wyzwalana domyślną trasę MVC, która domyślnie jest mapowany na **indeksu** akcji **HomeController** klasy. Ponieważ **HomeController** jest ograniczony do użytkowników uwierzytelnionych (należy pamiętać, że dekorowane tej klasy przy użyciu **Autoryzuj** atrybutu w ćwiczeniu 1) i brak uwierzytelnionych użytkowników, ale aplikacja Przekierowuje oryginalne żądanie do strony logowania.

    ![Uruchamianie rozwiązania](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "uruchamianie rozwiązania")

    *Uruchamianie rozwiązania*
2. Kliknij przycisk **zarejestrować** do utworzenia nowego użytkownika.

    ![Rejestrowanie nowego użytkownika](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "rejestrowanie nowego użytkownika")

    *Rejestrowanie nowego użytkownika*
3. W **zarejestrować** wpisz **nazwa_użytkownika** i **hasło**, a następnie kliknij przycisk **zarejestrować**.

    ![Strona rejestracji](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "strony rejestru")

    *Strona rejestracji*
4. Aplikacja rejestruje nowego konta i użytkownik jest uwierzytelniony i przekierowanie z powrotem do strony głównej.

    ![Użytkownik jest uwierzytelniany](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "użytkownik uwierzytelniony")

    *Użytkownik jest uwierzytelniony.*
5. W przeglądarce naciśnij **F12** otworzyć **narzędzi deweloperskich** panelu. Naciśnij klawisz **CTRL + 4** lub kliknij przycisk **sieci** ikonę, a następnie kliknij przycisk zieloną strzałkę, aby rozpocząć przechwytywanie ruchu sieciowego.

    ![Inicjowanie interfejsu API sieci Web, przechwytywanie sieci](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "przechwytywania sieci Inicjowanie interfejsu API sieci Web")

    *Inicjuje Przechwytywanie sieci interfejsu API sieci Web*
6. Dołącz **interfejsu api/elementy towarzyszące składni** do adresu URL w pasku adresu przeglądarki. Teraz Sprawdź szczegóły odpowiedzi z **uzyskać** metody akcji w **TriviaController**.

    ![Pobieranie następnego danych zapytania za pomocą interfejsu API sieci Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "podczas pobierania następnego danych zapytania za pomocą interfejsu API sieci Web")

    *Pobieranie następnego danych zapytania za pomocą interfejsu API sieci Web*

    > [!NOTE]
    > Po zakończeniu pobierania, pojawi się akcję przy użyciu pobranego pliku. Pozostaw to okno dialogowe otwarte, aby móc obejrzeć zawartość odpowiedzi za pomocą okna narzędzia dla deweloperów.
7. Teraz Sprawdź treść odpowiedzi. Aby to zrobić, kliknij przycisk **szczegóły** kartę, a następnie kliknij przycisk **treść odpowiedzi**. Można sprawdzić, czy pobrane dane jest obiekt z właściwościami **opcje** (czyli listę **TriviaOption** obiektów), **identyfikator** i **tytuł** odpowiadające **TriviaQuestion** klasy.

    ![Wyświetlanie treści odpowiedzi interfejsu API sieci Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "wyświetlanie treści odpowiedzi interfejsu API sieci Web")

    *Treść odpowiedzi interfejsu API sieci Web wyświetlania*
8. Wróć do programu Visual Studio, a następnie naciśnij klawisz **SHIFT + F5** Aby zatrzymać debugowanie.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Ćwiczenie 2: Tworzenie interfejsu SPA

W tym ćwiczeniu najpierw utworzysz sieci web frontonu część maniaków komputerowych Quiz, skupiając się na interakcję aplikacji jednostronicowej przy użyciu **AngularJS**. Zostanie następnie rozszerzyć środowisko użytkownika za pomocą CSS3, aby wykonać animacji i zapewniają efekt wizualny podczas przenoszenia z jedno pytanie do następnego do przełączania kontekstu.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Zadanie 1 — Tworzenie interfejsu SPA, za pomocą biblioteki AngularJS

W tym zadaniu użyjesz **AngularJS** do zaimplementowania Quiz maniaków komputerowych aplikacji po stronie klienta. **Moduł AngularJS** to platforma JavaScript typu open source, która rozszerza aplikacji opartych na przeglądarce *Model-View-Controller* możliwości (MVC), ułatwienia zarówno programowania i testowania.

Rozpocznie się od zainstalowania moduł AngularJS z konsoli Menedżera pakietów programu Visual Studio. Następnie utworzysz kontrolera, aby zapewnić działanie aplikacji Quiz maniaków komputerowych i widok do renderowania quiz pytań i odpowiedzi za pomocą aparatu szablonów AngularJS.

> [!NOTE]
> Aby uzyskać więcej informacji na temat AngularJS dotyczą [ [ http://angularjs.org/ ](http://angularjs.org/) ](http://angularjs.org/).


1. Otwórz **Visual Studio Express 2013 for Web** , a następnie otwórz **GeekQuiz.sln** rozwiązanie znajduje się w **/Ex2-CreatingASPAInterface/początkowy w źródle** folderu. Alternatywnie można kontynuować z rozwiązaniem uzyskanym w poprzednim ćwiczeniu.
2. Otwórz **Konsola Menedżera pakietów** z **narzędzia** > **Menedżera pakietów NuGet**. Wpisz następujące polecenie, aby zainstalować **AngularJS.Core** pakietu NuGet.

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **skrypty** folderu **GeekQuiz** projektu, a następnie wybierz **Dodaj | Nowy Folder**. Nazwa folderu **aplikacji** i naciśnij klawisz **Enter**.
4. Kliknij prawym przyciskiem myszy **aplikacji** właśnie utworzonego folderu i wybierz **Dodaj | Plik JavaScript**.

    ![Tworzenie nowego pliku JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Tworzenie nowego pliku JavaScript*
5. W **Określ nazwę dla elementu** okno dialogowe, typ *test controller* w **nazwa elementu** polu tekstowym i kliknij **OK**.

    ![Nazewnictwo nowy plik JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Nazewnictwo nowy plik JavaScript*
6. W **quiz controller.js** Dodaj następujący kod, aby zadeklarować i zainicjować AngularJS **QuizCtrl** kontrolera.

    (Code Snippet — *AngularQuizController AspNetWebApiSpa - Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > Funkcja konstruktora **QuizCtrl** kontrolera oczekuje, że do parametru o nazwie **$scope**. Stan początkowy zakresu należy skonfigurować w funkcji konstruktora, dołączając właściwości **$scope** obiektu. Zawiera właściwości **model widoku**i będzie dostępna do szablonu, po zarejestrowaniu kontrolera.
    > 
    > **QuizCtrl** kontroler jest zdefiniowany w moduł o nazwie **QuizApp**. Moduły są jednostek pracy, dzięki którym możesz przerwać osobne składniki aplikacji. Główne zalety korzystania z modułów jest, że kod to łatwiejsze do zrozumienia i ułatwia tworzenie testów jednostkowych, możliwość ponownego wykorzystania i łatwość konserwacji.
7. Teraz należy dodać zachowanie do zakresu w celu reagowania na zdarzenia wywoływane z poziomu tego widoku. Dodaj następujący kod na końcu **QuizCtrl** kontrolera w celu zdefiniowania **nextQuestion** działa w programach **$scope** obiektu.

    (Code Snippet — *AngularQuizControllerNextQuestion AspNetWebApiSpa - Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Ta funkcja pobiera następne pytanie z **elementy towarzyszące składni** internetowy interfejs API utworzony w poprzednim ćwiczeniu i dołącza dane zapytania do **$scope** obiektu.
8. Wstaw następujący kod na końcu **QuizCtrl** kontrolera w celu zdefiniowania **sendAnswer** działa w programach **$scope** obiektu.

    (Code Snippet — *AngularQuizControllerSendAnswer AspNetWebApiSpa - Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Ta funkcja wysyła odpowiedź wybrane przez użytkownika w celu **elementy towarzyszące składni** interfejsu API sieci Web i zapisuje wynik — np. w przypadku odpowiedź jest poprawna, czy nie — w **$scope** obiektu.
    > 
    > **NextQuestion** i **sendAnswer** funkcje z powyższych używają AngularJS **$http** obiektu komunikacji przy użyciu interfejsu API sieci Web za pośrednictwem element XMLHttpRequest warstwę abstrakcji Obiekt JavaScript w przeglądarce. Moduł AngularJS obsługuje innej usługi, która zapewnia wyższy poziom abstrakcji do wykonywania operacji CRUD względem zasobu za pośrednictwem interfejsów API RESTful. Moduł AngularJS **$resource** obiekt ma metody akcji, które zapewniają wysokiego poziomu zachowania bez konieczności interakcji z **$http** obiektu. Należy rozważyć użycie **$resource** obiektu w scenariuszach, który wymaga modelu CRUD (fore informacji, zobacz [dokumentacji $resource](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. Następnym krokiem jest do utworzenia szablonu AngularJS, definiujący widok, w którym quizu. Aby to zrobić, otwórz **Index.cshtml** pliku wewnątrz **widoków | Strona główna** folder i Zastąp zawartość następującym kodem.

    (Code Snippet — *GeekQuizView AspNetWebApiSpa - Ex2 -*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > Szablon AngularJS jest deklaratywne specyfikacja, która używa informacji o modelu i kontrolera do przekształcania znaczników statycznego na dynamiczny widok, który użytkownik zobaczy w przeglądarce. Poniżej przedstawiono przykłady AngularJS elementów i atrybutów elementów, których można użyć w szablonie:
    > 
    > - **Aplikacji ng** dyrektywy informuje AngularJS elementu DOM, który reprezentuje element główny aplikacji.
    > - **Ng-controller** dyrektywy dołącza kontrolera do modelu DOM w punkcie, w którym zadeklarowano dyrektywy.
    > - Notacja nawias klamrowy **{{}}** oznacza powiązania z właściwościami zakresu określonych w kontrolerze.
    > - **Kliknij ng** dyrektywy służy do wywoływania funkcji zdefiniowanych w zakresie w odpowiedzi użytkownik klika polecenie.
10. Otwórz **Site.css** pliku wewnątrz **zawartości** folderze i Dodaj następujący wyróżniony style na końcu pliku, aby zapewnić wygląd i działanie dla widoku quizu.

    (Code Snippet — *GeekQuizStyles AspNetWebApiSpa - Ex2 -*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Zadanie 2 — uruchamianie rozwiązania

Rozwiązanie za pomocą nowego użytkownika w ramach tego zadania, które będą wykonywane interfejs został utworzony przy użyciu narzędzia AngularJS do uzyskania odpowiedzi na niektóre pytania quizu.

1. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.
2. Rejestrowanie nowego konta użytkownika. Aby to zrobić, wykonaj kroki rejestracji opisano w ćwiczeniu 1, zadanie 3.

    > [!NOTE]
    > Jeśli używasz rozwiązania z poprzednim ćwiczeniu, możesz zalogować się przy użyciu konta użytkownika utworzonego wcześniej.
3. **Home** strony powinna zostać wyświetlona, przedstawiający na pierwsze pytanie quizu. Odpowiedz na pytanie, klikając jedną z opcji. Spowoduje to wyzwolenie **sendAnswer** funkcja zdefiniowana wcześniej, ułatwiającego wybranej opcji do **elementy towarzyszące składni** interfejsu API sieci Web.

    ![Odpowiadanie na pytania](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "odpowiadanie na pytania")

    *Odpowiadanie na pytania*
4. Po kliknięciu jednego z przycisków, powinna zostać wyświetlona odpowiedź. Kliknij przycisk **następne pytanie** pokazanie następujące pytanie. Spowoduje to wyzwolenie **nextQuestion** funkcję zdefiniowaną w kontrolerze.

    ![Żądanie następne pytanie](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "żądanie następne pytanie")

    *Żądanie następne pytanie*
5. Powinien pojawić się następne pytanie. Kontynuuj odpowiadanie na pytania dowolną liczbę razy. Po ukończeniu wszystkich pytań powinien zwrócić na pierwsze pytanie.

    ![Kolejnym pytaniem](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "następne pytanie")

    *Następne pytanie*
6. Wróć do programu Visual Studio, a następnie naciśnij klawisz **SHIFT + F5** Aby zatrzymać debugowanie.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Zadanie 3 — Tworzenie animacji przerzucania, za pomocą CSS3

W tym zadaniu CSS3, właściwości użyje do wykonywania animacji, dodając przerzucania efektu, gdy zostanie udzielona, a po pobraniu następne pytanie.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **zawartości** folderu **GeekQuiz** projektu, a następnie wybierz **Dodaj | Istniejący element...** .

    ![Dodawanie istniejącego elementu do folderu zawartości](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "Dodawanie istniejącego elementu do folderu zawartości")

    *Dodawanie istniejącego elementu do folderu zawartości*
2. W **Dodaj istniejący element** okno dialogowe, przejdź do **źródło/zasoby** i wybierz polecenie **Flip.css**. Kliknij przycisk **Dodaj**.

    ![Dodawanie pliku Flip.css z zasobów](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "Dodawanie pliku Flip.css z zasobów")

    *Dodawanie pliku Flip.css z zasobów*
3. Otwórz **Flip.css** właśnie został dodany plik i sprawdzić jego zawartości.
4. Znajdź **przerzucić przekształcania** komentarz. Wykorzystanie arkuszy CSS, style pod tym komentarzem **perspektywy** i **rotateY** przekształceń w celu wygenerowania &quot;karty przerzucanie&quot; efekt.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Znajdź **Ukryj tylnej stronie okienka podczas przerzucania** komentarz. Styl pod tym komentarzem ukrywa wstecz boku powierzchnie, gdy są one skierowane w przeciwną stronę przeglądarkę przez ustawienie **widoczność odrzucanie tylnych** właściwości CSS *ukryte*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Otwórz **BundleConfig.cs** pliku wewnątrz **aplikacji\_Start** folderze i Dodaj odwołanie do **Flip.css** w pliku **&quot;~/Content/css&quot;** styl pakietu

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Naciśnij klawisz **F5** uruchamiać rozwiązania i zaloguj się przy użyciu poświadczeń.
8. Udzielenia odpowiedzi na pytanie, klikając jedną z opcji. Podczas przenoszenia między widokami, zwróć uwagę wpływ przerzucania.

    ![Odpowiadanie na pytania z mocą przerzucania](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "udzielenie odpowiedzi na pytania za pomocą przerzucania efektu")

    *Odpowiadanie na pytania za pomocą przerzucania efektu*
9. Kliknij przycisk **następne pytanie** można pobrać następujące pytanie. Przerzuć efekt powinna pojawić się ponownie.

    ![Trwa pobieranie następujące pytanie, w tym celu przerzucania](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "pobierania następujące pytanie, w tym celu przerzucania")

    *Trwa pobieranie następujące pytanie, w tym celu przerzucania*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Przez ukończenie tego laboratorium praktycznego wiesz już, jak:

- Utworzyć kontroler Web API platformy ASP.NET za pomocą programu ASP.NET tworzenie szkieletów
- Implementowanie akcji Pobierz interfejsu API sieci Web do pobierania następnego pytania quizu
- Wykonania akcji po interfejsu API sieci Web do przechowywania odpowiedzi do quizu
- Zainstaluj moduł AngularJS z konsoli Menedżera pakietów Visual Studio
- Implementowanie AngularJS szablonów i kontrolerów
- Użyj CSS3 przejścia do wykonania efektów animacji
