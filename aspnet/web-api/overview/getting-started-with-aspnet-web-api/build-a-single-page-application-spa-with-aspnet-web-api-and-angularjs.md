---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: "Wskazówki laboratorium: Tworzenie aplikacji jednej strony (SPA) z interfejsu API sieci Web ASP.NET i Angular.js | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W aplikacjach sieci web tradycyjnych klienta (przeglądarka) inicjuje komunikację z serwerem przez żądanie strony. Serwer następnie przetwarza żądanie..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2015
ms.topic: article
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 9a748628d53878be380869ac5327de0111d2284d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Wskazówki laboratorium: Tworzenie aplikacji jednej strony (SPA) z interfejsu API sieci Web ASP.NET i Angular.js
====================
przez [obozów sieci Web Team](https://twitter.com/webcamps)

[Pobierz obozów sieci Web uczenie Kit](http://aka.ms/webcamps-training-kit)

> W aplikacjach sieci web tradycyjnych klienta (przeglądarka) inicjuje komunikację z serwerem przez żądanie strony. Następnie serwer przetwarza żądanie i wysyła do klienta HTML strony. W kolejnych interakcji ze strony — np. użytkownik przechodzi do łącza lub przesłaniu formularza z danymi — nowe żądanie jest wysyłane do serwera, a przepływ uruchamia ponownie: serwer przetwarza żądanie i wysyła nowej strony do przeglądarki w odpowiedzi na żądanie nowej akcji ED przez klienta.
> 
> W aplikacji jednej strony (źródła) jest ładowany całą stronę w przeglądarce po odebraniu żądania początkowego, ale kolejnych interakcjach została wykonana za pomocą żądania Ajax. Oznacza to, że przeglądarka musi zaktualizować tylko części strony, który został zmieniony; nie istnieje potrzeba aby ponownie załadować dla całej strony. Podejście SPA zmniejsza czas aplikacji odpowiada na działania użytkownika, co zapewnia bardziej płynne środowisko.
> 
> Architektura SPA obejmuje niektóre wyzwania, które nie są obecne w aplikacji sieci web tradycyjnych. Jednak rozwijających się technologii, takich jak Web API platformy ASP.NET, takich jak JavaScript struktur AngularJS i nowe funkcje stylów oferowane przez CSS3 ułatwiają naprawdę projektowanie i kompilowanie źródła.
> 
> W tym strony w laboratorium będą korzystać z tych technologii do zaimplementowania Geek quizu oparty na koncepcji SPA witryny sieci Web elementy towarzyszące składni. Najpierw będzie implementowany warstwy usług z interfejsu API sieci Web ASP.NET do udostępnienia wymagane punkty końcowe można pobrać pytania kwizu i przechowywać odpowiedzi. Następnie utworzysz bogaty i elastyczny interfejsu użytkownika przy użyciu efekty przekształcania AngularJS i CSS3.
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).


## <a name="overview"></a>Omówienie

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym laboratorium praktycznego przedstawiono sposób:

- Tworzenie usługi interfejsu API sieci Web platformy ASP.NET do wysyłania i odbierania danych JSON
- Utwórz reakcje interfejsu użytkownika, przy użyciu AngularJS
- Przekształceń CSS3 udoskonalenie interfejsu użytkownika

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Poniżej jest wymagany do ukończenia tego laboratorium praktycznego:

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) lub nowszej

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

Aby można było uruchomić ćwiczeń opisanych w tym laboratorium praktycznego, należy najpierw skonfigurować środowiska.

1. Otwórz Eksploratora Windows i przejdź do laboratorium **źródła** folderu.
2. Kliknij prawym przyciskiem myszy **Setup.cmd** i wybierz **Uruchom jako administrator** do uruchamiania procesu instalacji, który skonfigurujesz środowisko i zainstalować wstawki kodu programu Visual Studio dla tego laboratorium.
3. Jeśli wyświetlane jest okno dialogowe kontroli konta użytkownika, potwierdź tę akcję, aby kontynuować.

> [!NOTE]
> Upewnij się, że zaznaczono wszystkie zależności dla tego laboratorium przed uruchomieniem Instalatora.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Korzystania z wstawek kodu

W dokumencie laboratorium należy poinstruować do wstawienia bloków kodu. Dla wygody większość tego kodu jest dostępna w Visual Studio wstawki kodu, które są dostępne w Visual Studio 2013, aby uniknąć konieczności Dodaj ją ręcznie.

> [!NOTE]
> Każdy wykonywania towarzyszy początkowy rozwiązania, znajdujących się w **rozpocząć** folderu ćwiczeniu, która umożliwia wykonanie każdej wykonywania niezależnie od innych. Należy pamiętać, że fragmenty kodu, które są dodawane podczas wykonywania brakuje te uruchamianie rozwiązań i może nie działać do czasu wykonywania. W kodzie źródłowym wykonywania można również znaleźć **zakończenia** folderu zawierającego rozwiązanie Visual Studio z kodem, który powoduje wykonanie czynności opisane w odpowiedniej wykonywania. Jeśli potrzebujesz dodatkowej pomocy w pracy za pośrednictwem tego laboratorium praktycznego, można użyć tych rozwiązań jako wskazówki.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To laboratorium praktycznego obejmuje następujących czynnościach:

1. [Tworzenie interfejsu API sieci Web](#Exercise1)
2. [Tworzenie interfejsu SPA](#Exercise2)

Szacowany czas trwania tego laboratorium: **60 minut**

> [!NOTE]
> Przy pierwszym uruchomieniu programu Visual Studio, musisz wybrać jeden z wstępnie zdefiniowanych ustawień kolekcji. Każda kolekcja wstępnie zdefiniowanych zaprojektowano w celu stylem rozwoju i określa układów okien, zachowanie edytora wstawki kodu IntelliSense i opcje w oknach dialogowych. Procedury przedstawione w tym laboratorium opisano czynności niezbędnych do wykonywania danego zadania w programie Visual Studio, korzystając z **ogólne ustawienia środowiska deweloperskiego** kolekcji. Jeśli wybierzesz kolekcji różne ustawienia dla swojego środowiska programowania, może być różnice w krokach, które należy wziąć pod uwagę.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Ćwiczenie 1: Tworzenie składnika Web API

Jednym z kluczowych części SPA jest warstwy usług. Jest odpowiedzialna za przetwarzanie wywołania Ajax wysyłany przez interfejs użytkownika i zwracająca dane w odpowiedzi dla tego wywołania. Dane pobrane powinny być prezentowane w formacie czytelną przeanalizowano i używane przez klienta w celu.

Strukturę interfejsu API sieci Web jest częścią stosu ASP.NET i jest przeznaczony do ułatwia wdrożenie usług HTTP, zazwyczaj wysyłania i odbierania danych w formacie JSON i XML za pomocą interfejsu API RESTful. W tym ćwiczeniu utworzysz witrynę sieci Web do hostowania aplikacji Geek testu, a następnie wdrożyć usługi zaplecza ujawnia do utrwalenia danych kwizu przy użyciu interfejsu API sieci Web platformy ASP.NET.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Zadanie 1 — Tworzenie początkowej projektu dla Geek kwizu

W tym zadaniu rozpoczęcia tworzenia nowego projektu platformy ASP.NET MVC z obsługą interfejsu API sieci Web platformy ASP.NET na podstawie **ASP.NET jeden** projektu typu, który jest dostarczany z programem Visual Studio. **Jeden ASP.NET** łączy wszystkie technologii ASP.NET i daje możliwość mieszać i dopasowywać je zgodnie z potrzebami. Następnie dodasz klasy modelu programu Entity Framework i initializator bazy danych, aby wstawić pytania testu.

1. Otwórz **programu Visual Studio Express 2013 for Web** i wybierz **pliku | Nowy projekt...**  można uruchomić nowego rozwiązania.

    ![Tworzenie nowego projektu](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "tworzenia nowego projektu")

    *Tworzenie nowego projektu*
2. W **nowy projekt** okno dialogowe, wybierz opcję **aplikacji sieci Web ASP.NET** w obszarze **Visual C# | Web** kartę. Upewnij się, że **.NET Framework 4.5** jest zaznaczone, nadaj jej nazwę *GeekQuiz*, wybierz **lokalizacji** i kliknij przycisk **OK**.

    ![Tworzenie nowego projektu aplikacji sieci Web ASP.NET](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "tworzenia nowego projektu aplikacji sieci Web ASP.NET")

    *Tworzenie nowego projektu aplikacji sieci Web ASP.NET*
3. W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **MVC** szablonu, a następnie wybierz **interfejsu API sieci Web** opcji. Ponadto upewnij się, że **uwierzytelniania** ustawiono opcję **indywidualnych kont użytkowników**. Kliknij przycisk **OK** aby kontynuować.

    ![Tworzenie nowego projektu z szablonu MVC, w tym składniki interfejsu API sieci Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Tworzenie nowego projektu z szablonu MVC, w tym składniki interfejsu API sieci Web*
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **modele** folderu **GeekQuiz** projekt i wybierz **Dodaj | Istniejący element...** .

    ![Dodawanie istniejącego elementu](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "Dodawanie istniejącego elementu")

    *Dodawanie istniejącego elementu*
5. W **Dodaj istniejący element** oknie dialogowym wybierz opcję **modele-źródłowego/zasoby** i wybierz polecenie wszystkie pliki. Kliknij przycisk **Dodaj**.

    ![Dodawanie zasobów modelu](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "Dodawanie zasobów modelu")

    *Dodawanie zasobów modelu*

    > [!NOTE]
    > Dodanie tych plików, są Dodawanie modelu danych, kontekst bazy danych programu Entity Framework i inicjatora bazy danych dla aplikacji quizu Geek.
    > 
    > **Entity Framework (EF)** jest mapowania obiektów relacyjnych (ORM) umożliwiający tworzenie aplikacji dostęp do danych przez programowania za pomocą modelu koncepcyjnego aplikacji zamiast programowanie bezpośrednio przy użyciu schematu magazyn relacyjny. Dowiedz się więcej na temat programu Entity Framework [tutaj](../../../entity-framework.md).
    > 
    > Poniżej znajduje się opis klas, który właśnie został dodany:
    > 
    > - **TriviaOption:** reprezentuje jednej opcji powiązanej z pytaniem kwizu
    > - **TriviaQuestion:** reprezentuje pytanie kwizu i udostępnia opcje skojarzone za pośrednictwem **opcje** właściwości
    > - **TriviaAnswer:** reprezentuje z opcją wybraną przez użytkownika w odpowiedzi na pytanie kwizu
    > - **TriviaContext:** reprezentuje kontekst bazy danych programu Entity Framework quizu Geek aplikacji. Ta klasa pochodzi od **DContext** i udostępnia **DbSet** właściwości, które reprezentują kolekcje jednostek opisane powyżej.
    > - **TriviaDatabaseInitializer:** implementacja inicjator Entity Framework dla **TriviaContext** klasy, która dziedziczy od **CreateDatabaseIfNotExists**. Domyślne zachowanie tej klasy można utworzyć bazy danych tylko wtedy, gdy nie istnieje, wstawianie jednostek w określono **inicjatora** metody.
6. Otwórz **Global.asax.cs** pliku i dodaj następującą instrukcję using.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Dodaj następujący kod na początku **aplikacji\_Start** metodę, aby ustawić **TriviaDatabaseInitializer** jako inicjatora bazy danych.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Modyfikowanie **Home** kontrolera, aby ograniczyć dostęp do uwierzytelnionych użytkowników. Aby to zrobić, otwórz **HomeController.cs** pliku wewnątrz **kontrolerów** folderu i Dodaj **autoryzacji** atrybutu **HomeController**definicji klasy.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > **Autoryzacji** filtrować sprawdza, czy użytkownik jest uwierzytelniony. Jeśli użytkownik nie jest uwierzytelniony, zwraca kod stanu HTTP 401 (bez autoryzacji) bez wywoływania akcji. Można zastosować filtr globalny, na poziomie kontrolera lub na poziomie poszczególnych działań.
9. Układ strony sieci web i znakowanie zostanie teraz dostosować. Aby to zrobić, otwórz  **\_Layout.cshtml** pliku wewnątrz **widoków | Udostępnione** folderu i zaktualizowania zawartości  **&lt;tytuł&gt;**  element zastępując *Moja aplikacja platformy ASP.NET* z *Geek quizu* .

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. W tym samym pliku, należy zaktualizować pasek nawigacyjny przez usunięcie *o* i *skontaktuj się z* łącza i zmiana nazwy *Home* połączyć *odtwarzanie*. Ponadto, Zmień nazwę *Nazwa aplikacji* połączyć *quizu Geek*. HTML paska nawigacyjnego powinien wyglądać podobnie do następującego kodu.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Zaktualizuj stopki strony układu zastępując *Moja aplikacja platformy ASP.NET* z *quizu Geek*. Aby to zrobić, zastąpi zawartość  **&lt;stopki&gt;**  elementu o następujący wyróżniony kod.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Zadanie 2 — Tworzenie interfejsu API sieci Web TriviaController

W poprzednim zadaniu utworzono początkowego struktury quizu Geek aplikacji sieci web. Teraz utworzysz prosty usługi interfejsu API sieci Web, która współdziała z modelem danych kwizu i udostępnia następujące akcje:

- **GET/api/elementy towarzyszące składni**: pobiera następne pytanie na liście testu musi być podana przez użytkownika uwierzytelnionego.
- **POST/api/elementy towarzyszące składni**: przechowuje odpowiedzi kwizu określony przez uwierzytelnionego użytkownika.

Aby utworzyć linię bazową dla klasy kontrolera interfejsu API sieci Web użyjesz narzędzi szkieletów ASP.NET w programie Visual Studio.

1. Otwórz **WebApiConfig.cs** pliku wewnątrz **aplikacji\_Start** folderu. Ten plik definiuje konfigurację usługi interfejsu API sieci Web, takich jak odwzorowania trasy do akcji kontrolera interfejsu API sieci Web.
2. Dodaj następującą instrukcję using na początku pliku.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Dodaj następujący wyróżniony kod, aby **zarejestrować** metoda globalnie konfiguracji program formatujący dla danych JSON pobierane przez metody akcji interfejsu API sieci Web.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > **CamelCasePropertyNamesContractResolver** powoduje automatyczną konwersję nazw właściwości do *formatu* przypadku ogólne Konwencji dla nazwy właściwości w języku JavaScript.
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **kontrolerów** folderu **GeekQuiz** projekt i wybierz **Dodaj | Nowy element szkieletu...** .

    ![Tworzenie nowy element szkieletu](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "tworzenie nowy element szkieletu")

    *Tworzenie nowy element szkieletu*
5. W **Dodawanie szkieletu** okna dialogowego upewnij się, że **wspólnej** jest wybrany węzeł w lewym okienku. Następnie wybierz opcję **kontrolera 2 interfejsu API sieci Web — pusty** szablonu w środkowym okienku i kliknij **Dodaj**.

    ![Wybór szablonu sieci Web API 2 kontrolera pusty](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "wybór sieci Web API 2 kontrolera pustego szablonu")

    *Wybór szablonu pusty kontroler 2 interfejsu API sieci Web*

    > [!NOTE]
    > **Funkcja szkieletów ASP.NET** to struktura generowania kodu dla aplikacji sieci Web ASP.NET. Visual Studio 2013 obejmuje generatory kodu wstępnie zainstalowane dla projektów MVC i interfejsu API sieci Web. Jeśli chcesz szybko dodać kod, który wchodzi w interakcję z modelami danych Aby skrócić czas wymagany do opracowywania działań standardowych danych należy używać szkieletów w projekcie.
    > 
    > Proces tworzenia szkieletów gwarantuje również, że wszystkie wymagane zależności są zainstalowane w projekcie. Na przykład jeśli rozpoczynać pusty projekt ASP.NET i następnie dodać Kontroler interfejsu API sieci Web za pomocą funkcja szkieletów, wymagane pakiety NuGet interfejsu API sieci Web i odwołania są automatycznie dodawane do projektu.
6. W **Dodaj kontroler** okno dialogowe, typ *TriviaController* w **nazwy kontrolera** polu tekstowym i kliknij przycisk **Dodaj**.

    ![Dodawanie kontrolera elementy towarzyszące składni](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "dodawania kontrolera elementy towarzyszące składni")

    *Dodawanie kontrolera elementy towarzyszące składni*
7. **TriviaController.cs** pliku jest dodawane do **kontrolerów** folderu **GeekQuiz** projektu zawierającego pustą **TriviaController** klasy. Dodaj następujące instrukcje using na początku pliku.

    (Fragment - kodu *TriviaControllerUsings AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Dodaj następujący kod na początku **TriviaController** klasę, aby zdefiniować, zainicjować i usuwania **TriviaContext** wystąpienia w kontrolerze.

    (Fragment - kodu *TriviaControllerContext AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > **Dispose** metody **TriviaController** wywołuje **Dispose** metody **TriviaContext** wystąpienia, która zapewnia, że wszystkie zasoby używane przez obiekt kontekstu są wydawane podczas **TriviaContext** wystąpienia jest usunięty lub zbierane z pamięci. W tym zamknięcie wszystkich połączeń bazy danych otwarty przez Entity Framework.
9. Dodaj następującą metodę pomocnika na końcu **TriviaController** klasy. Ta metoda pobiera na poniższe pytanie testu z bazy danych musi być podana przez określonego użytkownika.

    (Fragment - kodu *TriviaControllerNextQuestion AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Dodaj następujące **uzyskać** metody akcji, aby **TriviaController** klasy. Ta metoda akcji wywołuje **NextQuestionAsync** metody pomocnika zdefiniowanych w poprzednim kroku, aby pobrać następne pytanie dla tego uwierzytelnionego użytkownika.

    (Fragment - kodu *TriviaControllerGetAction AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Dodaj następującą metodę pomocnika na końcu **TriviaController** klasy. Ta metoda przechowuje odpowiedzi określonego w bazie danych i zwraca wartość Boolean wskazującą, czy odpowiedź jest poprawna.

    (Fragment - kodu *TriviaControllerStoreAsync AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Dodaj następujące **Post** metody akcji, aby **TriviaController** klasy. Ta metoda akcji kojarzy odpowiedzi uwierzytelnionego użytkownika i wywołania **StoreAsync** metody pomocnika. Następnie wysyła odpowiedź na wartość logiczną zwracany przez metodę pomocnika.

    (Fragment - kodu *TriviaControllerPostAction AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Modyfikowanie kontrolera interfejsu API sieci Web, aby ograniczyć dostęp dla użytkowników uwierzytelnionych przez dodanie **autoryzacji** atrybutu **TriviaController** definicji klasy.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Zadanie 3 — uruchomiona rozwiązania

W tym zadaniu zostanie Sprawdź, czy Usługa interfejsu API sieci Web, które zostały utworzone w poprzednim zadaniu działa zgodnie z oczekiwaniami. Korzystasz z programu Internet Explorer **F12 Developer Tools** do przechwytywania ruchu sieciowego i sprawdzić całą odpowiedź z usługi interfejsu API sieci Web.

> [!NOTE]
> Upewnij się, że **programu Internet Explorer** wybrano **Start** przycisk znajdujący się na pasku narzędzi programu Visual Studio.
> 
> ![Opcji programu Internet Explorer](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie. **Zaloguj** strony powinna zostać wyświetlona w przeglądarce.

    > [!NOTE]
    > Po uruchomieniu aplikacji MVC trasy domyślnej zostanie wywołany, która domyślnie jest mapowana na **indeksu** akcji **HomeController** klasy. Ponieważ **HomeController** jest ograniczony do użytkowników, uwierzytelnionym (należy pamiętać, że dekorowane tej klasy z **autoryzacji** atrybutu w ćwiczenie 1) i ma nie użytkownik uwierzytelniony, ale aplikacji Przekierowuje oryginalne żądanie do strony logowania.

    ![Uruchomiona rozwiązania](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "systemem rozwiązania")

    *Uruchomiona rozwiązania*
2. Kliknij przycisk **zarejestrować** do utworzenia nowego użytkownika.

    ![Rejestrowanie nowego użytkownika](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "rejestrowanie nowego użytkownika")

    *Rejestrowanie nowego użytkownika*
3. W **zarejestrować** wprowadź **nazwy użytkownika** i **hasło**, a następnie kliknij przycisk **zarejestrować**.

    ![Zarejestruj strony](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "strony rejestru")

    *Zarejestruj strony*
4. Aplikacja rejestruje nowego konta i użytkownik jest uwierzytelniony i przekierowany z powrotem do strony głównej.

    ![Użytkownik jest uwierzytelniany](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "użytkownik uwierzytelniony")

    *Użytkownik jest uwierzytelniony.*
5. W przeglądarce, naciśnij klawisz **F12** otworzyć **Developer Tools** panelu. Naciśnij klawisz **CTRL + 4** lub kliknij przycisk **sieci** ikonę, a następnie kliknij przycisk zieloną strzałkę, aby rozpocząć przechwytywanie ruchu sieciowego.

    ![Inicjowanie przechwytywania ruchu sieciowego interfejsu API sieci Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "przechwytywania ruchu sieciowego Inicjowanie interfejsu API sieci Web")

    *Inicjowanie przechwytywania ruchu sieciowego interfejsu API sieci Web*
6. Dołącz **interfejsu api/elementy towarzyszące składni** do adresu URL na pasku adresu przeglądarki. Teraz będzie przeprowadzał inspekcję szczegóły odpowiedzi z **uzyskać** metody akcji w **TriviaController**.

    ![Pobieranie następnej danych za pośrednictwem interfejsu API sieci Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "pobierania dalej danych za pośrednictwem interfejsu API sieci Web")

    *Pobieranie następnej danych za pośrednictwem interfejsu API sieci Web*

    > [!NOTE]
    > Po zakończeniu pobierania pojawi się monit dokonanie akcji z pobranego pliku. Pozostaw to okno dialogowe otwarte, aby można było oglądać zawartości odpowiedzi, za pomocą okna narzędzia dla deweloperów.
7. Teraz będzie przeprowadzał inspekcję treść odpowiedzi. Aby to zrobić, kliknij przycisk **szczegóły** a następnie kliknij pozycję **treść odpowiedzi**. Można sprawdzić, czy pobrane dane mają obiekt o właściwości **opcje** (czyli listę **TriviaOption** obiektów), **identyfikator** i **tytuł** odpowiadające **TriviaQuestion** klasy.

    ![Wyświetlanie treści odpowiedzi interfejsu API sieci Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "wyświetlanie treść odpowiedzi interfejsu API sieci Web")

    *Treść odpowiedzi interfejsu API sieci Web wyświetlania*
8. Wróć do programu Visual Studio i naciśnij klawisz **SHIFT + F5** można zatrzymać debugowania.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Ćwiczenie 2: Tworzenie interfejsu SPA

W tym ćwiczeniu najpierw utworzysz sieci web frontonu część Geek quizu, koncentrujących się na interakcji jednostronicowej aplikacji przy użyciu **AngularJS**. Następnie zostanie ulepszanie środowiska użytkownika z CSS3 wykonywania zaawansowanych animacji i podaj efektem przełączania podczas przejścia z poprzedniego pytania do następnego kontekstu.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Zadanie 1 — Tworzenie interfejsu SPA przy użyciu AngularJS

W tym zadaniu użyjesz **AngularJS** do zaimplementowania quizu Geek aplikacji po stronie klienta. **AngularJS** jest architektury JavaScript open source, który wspomaga aplikacji opartych na przeglądarce *Model-View-Controller* możliwości (MVC), ułatwiające zarówno programowanie i testowanie.

Rozpoczęcia, instalując AngularJS w konsoli Menedżera pakietów programu Visual Studio. Następnie można utworzyć kontrolera, aby zapewnić zachowanie aplikacji kwizu Geek i widoku do renderowania kwizu pytania i odpowiedzi przy użyciu aparatu szablonu AngularJS.

> [!NOTE]
> Aby uzyskać więcej informacji na temat AngularJS dotyczą [ [http://angularjs.org/](http://angularjs.org/)](http://angularjs.org/).


1. Otwórz **programu Visual Studio Express 2013 for Web** , a następnie otwórz **GeekQuiz.sln** rozwiązania, znajdujących się w **źródło/Ex2-CreatingASPAInterface/Begin** folderu. Alternatywnie możesz kontynuować z rozwiązaniem uzyskanymi w poprzednim ćwiczeniu.
2. Otwórz **Konsola Menedżera pakietów** z **narzędzia** | **Menedżer pakietów biblioteki**. Wpisz następujące polecenie, aby zainstalować **AngularJS.Core** pakietu NuGet.

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **skryptów** folderu **GeekQuiz** projekt i wybierz **Dodaj | Nowy Folder**. Nazwa folderu **aplikacji** i naciśnij klawisz **Enter**.
4. Kliknij prawym przyciskiem myszy **aplikacji** folderu właśnie utworzony i wybierz **Dodaj | Plik JavaScript**.

    ![Tworzenie nowego pliku JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Tworzenie nowego pliku JavaScript*
5. W **Określ nazwę dla elementu** okno dialogowe, typ *kontrolera testu* w **nazwa elementu** polu tekstowym i kliknij przycisk **OK**.

    ![Nazewnictwo nowy plik JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Nazewnictwo nowy plik JavaScript*
6. W **controller.js kwizu** pliku, Dodaj następujący kod, aby zadeklarować i zainicjuj AngularJS **QuizCtrl** kontrolera.

    (Fragment - kodu *AngularQuizController AspNetWebApiSpa - Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > Funkcji konstruktora **QuizCtrl** kontrolera oczekuje do parametru o nazwie **$scope**. Stan początkowy zakresu powinny zostać skonfigurowane w funkcji konstruktora, dołączając właściwości **$scope** obiektu. Właściwości zawierają **model widoku**i będą dostępne do szablonu, po zarejestrowaniu kontrolera.
    > 
    > **QuizCtrl** kontroler jest zdefiniowany w moduł o nazwie **QuizApp**. Moduły są jednostki pracy, które umożliwiają podzielić składniki aplikacji. Główne zalety używania modułów to, czy kod jest łatwiejsze do zrozumienia i ułatwia testowanie jednostkowe, możliwość ponownego wykorzystania i łatwości konserwacji.
7. Zachowanie będzie teraz dodać do zakresu, w celu reagowania na zdarzenia wyzwolone z widoku. Dodaj następujący kod na końcu **QuizCtrl** kontrolera do definiowania **nextQuestion** działać w **$scope** obiektu.

    (Fragment - kodu *AngularQuizControllerNextQuestion AspNetWebApiSpa - Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Ta funkcja pobiera następne pytanie z **elementy towarzyszące składni** interfejsu API sieci Web utworzony w poprzednim ćwiczeniu i dołącza dane pytanie **$scope** obiektu.
8. Wstaw następujący kod na końcu **QuizCtrl** kontrolera do definiowania **sendAnswer** działać w **$scope** obiektu.

    (Fragment - kodu *AngularQuizControllerSendAnswer AspNetWebApiSpa - Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Ta funkcja wysyła odpowiedzi wybranej przez użytkownika w celu **elementy towarzyszące składni** interfejsu API sieci Web i zapisuje wynik — oznacza, że jeśli odpowiedź jest poprawna, czy nie — w **$scope** obiektu.
    > 
    > **NextQuestion** i **sendAnswer** funkcje z powyższych używają AngularJS **$http** obiektu abstrakcyjnej komunikacji przy użyciu interfejsu API sieci Web za pośrednictwem XMLHttpRequest Obiekt JavaScript w przeglądarce. AngularJS obsługuje innej usługi, która zapewnia wyższy poziom abstrakcji do wykonywania operacji CRUD względem zasobów za pośrednictwem interfejsów API RESTful. AngularJS **$resource** obiekt ma metody akcji, które zapewniają wysokiego poziomu zachowania bez konieczności interakcji z **$http** obiektu. Należy rozważyć użycie **$resource** obiektu w scenariuszach, który wymaga modelu CRUD (fore informacji, zobacz [dokumentacji $resource](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. Następnym krokiem jest tworzenia szablonu AngularJS, która określa widok kwizu. Aby to zrobić, otwórz **Index.cshtml** pliku wewnątrz **widoków | Strona główna** folderu i Zastąp zawartość następującym kodem.

    (Fragment - kodu *GeekQuizView AspNetWebApiSpa - Ex2 -*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > Szablon AngularJS jest specyfikację deklaratywne na podstawie informacji z modelu i kontrolera do przekształcania znaczników statycznego na dynamiczny widok, który użytkownik zobaczy w przeglądarce. Poniżej przedstawiono przykłady AngularJS elementów i atrybutów elementów, których można użyć w szablonie:
    > 
    > - **Ng aplikacji** dyrektywy informuje AngularJS element DOM, który reprezentuje element główny aplikacji.
    > - **Ng kontrolera** dyrektywy dołącza kontrolera do modelu DOM w momencie, gdy zadeklarowano dyrektywy.
    > - Notacja nawias klamrowy **{{}}** oznacza powiązania właściwości zakresu określonych w kontrolerze.
    > - **Ng kliknięciem** dyrektywy służy do wywoływania funkcji zdefiniowanych w zakresie w odpowiedzi na użytkownik klika polecenie.
10. Otwórz **Site.css** pliku wewnątrz **zawartości** folderu i dodaj następujące style wyróżnione na końcu pliku zapewnienie wyglądu i działania dla widoku testu.

    (Fragment - kodu *GeekQuizStyles AspNetWebApiSpa - Ex2 -*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Zadanie 2 — rozwiązanie uruchomiona

Rozwiązanie przy użyciu nowego użytkownika w ramach tego zadania, które będą wykonywane interfejs zostanie utworzona za pomocą AngularJS odpowiedzi na niektóre pytania kwizu.

1. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.
2. Zarejestruj nowe konto użytkownika. Aby to zrobić, wykonaj kroki rejestracji opisanego w ćwiczenie 1, 3 zadań.

    > [!NOTE]
    > Jeśli korzystasz z rozwiązania z poprzednim ćwiczeniu, możesz zalogować się przy użyciu konta użytkownika, który utworzono przed.
3. **Home** strony powinien być wyświetlane na pierwsze pytanie kwizu. Odpowiedz na pytanie, klikając jedną z opcji. To wyzwoli **sendAnswer** funkcji zdefiniowanego wcześniej, który wysyła opcji do **elementy towarzyszące składni** interfejsu API sieci Web.

    ![Udzielenie odpowiedzi na pytanie](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "udzielenie odpowiedzi na pytania")

    *Udzielenie odpowiedzi na pytania*
4. Po kliknięciu przycisku jeden z przycisków, powinna zostać wyświetlona odpowiedź. Kliknij przycisk **następne pytanie** do wyświetlenia na poniższe pytanie. To wyzwoli **nextQuestion** funkcji zdefiniowanej w kontrolerze.

    ![Żąda następne pytanie](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "żąda następne pytanie")

    *Następne pytanie żądania*
5. Następne pytanie powinny być wyświetlane. Nadal odpowiadanie na pytania dowolną liczbę razy. Po wypełnieniu wszystkie pytania należy zwraca pierwsze pytanie.

    ![Następne pytanie](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "innego zapytania")

    *Następne pytanie*
6. Wróć do programu Visual Studio i naciśnij klawisz **SHIFT + F5** można zatrzymać debugowania.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Zadanie 3 — Tworzenie przerzucania animacji, przy użyciu CSS3

W tym zadaniu właściwości CSS3 użyje do wykonywania zaawansowanych animacji przez dodanie przerzucania po odpowiedzi na pytanie, a po pobraniu następne pytanie.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **zawartości** folderu **GeekQuiz** projekt i wybierz **Dodaj | Istniejący element...** .

    ![Dodawanie istniejącego elementu do folderu zawartości](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "Dodawanie istniejącego elementu do folderu zawartości")

    *Dodawanie istniejącego elementu do folderu zawartości*
2. W **Dodaj istniejący element** oknie dialogowym wybierz opcję **źródło/zasoby** i wybierz polecenie **Flip.css**. Kliknij przycisk **Dodaj**.

    ![Dodawanie pliku Flip.css z zasobów](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "dodawania pliku Flip.css z zasobów")

    *Dodawanie pliku Flip.css z zasobów*
3. Otwórz **Flip.css** pliku zostanie dodany i przejrzyj jego zawartość.
4. Zlokalizuj **Przerzuć przekształcania** komentarza. Style pod tym komentarzem Użyj CSS **perspektywy** i **rotateY** przekształceń do generowania &quot;kart przerzucanie&quot; efekt.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Zlokalizuj **Ukryj obu okienku podczas przerzucania** komentarza. Styl pod tym komentarzem ukrywa stronie powierzchni Wstecz, gdy są one skierowane w przeciwną Podgląd przez ustawienie **widoczność wszelkie niepożądane obiekty za** właściwości CSS do *ukryte*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Otwórz **BundleConfig.cs** pliku wewnątrz **aplikacji\_Start** folderu i Dodaj odwołanie do **Flip.css** w pliku  **&quot;~/Content/css&quot;**  styl pakietu

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Naciśnij klawisz **F5** do uruchamiania rozwiązań i zaloguj się przy użyciu poświadczeń.
8. Udzielenia odpowiedzi na pytanie, klikając jedną z opcji. Zwróć uwagę wpływ przerzucania podczas przejścia między widokami.

    ![Udzielenie odpowiedzi na pytania z mocą przerzucania](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "udzielenie odpowiedzi na pytania z mocą przerzucania")

    *Udzielenie odpowiedzi na pytania z mocą przerzucania*
9. Kliknij przycisk **następne pytanie** do pobrania na poniższe pytanie. Przerzuć efekt powinna pojawić się ponownie.

    ![Pobieranie na poniższe pytanie począwszy przerzucania](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "pobierania na poniższe pytanie począwszy przerzucania")

    *Pobieranie na poniższe pytanie począwszy przerzucania*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Wykonując tego laboratorium praktycznego wiesz już, jak:

- Tworzenie kontrolera interfejsu API sieci Web ASP.NET przy użyciu funkcji szkieletów ASP.NET
- Zastosuj akcję Get interfejsu API sieci Web do pobierania następne pytanie kwizu
- Implementowanie akcji po interfejsu API sieci Web do przechowywania odpowiedzi kwizu
- Zainstaluj AngularJS z konsoli Menedżera pakietów programu Visual Studio
- Implementowanie AngularJS szablonów i kontrolerów
- Aby przeprowadzić efekty animacji, użyj przejścia CSS3
