---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'Aplikacja jednostronicowa: Szablon KnockoutJS | Dokumentacja firmy Microsoft'
author: MikeWasson
description: Szablon knockout
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
ms.technology: ''
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 75169836d1d9a647bdd4ac3742cb1dc1f31f2a32
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377905"
---
<a name="single-page-application-knockoutjs-template"></a>Aplikacja jednostronicowa: Szablon KnockoutJS
====================
przez [Mike Wasson](https://github.com/MikeWasson)

> Szablon MVC Knockout jest częścią platformy ASP.NET i Web Tools 2012.2
> 
> [Pobierz ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)


Aktualizacja programu ASP.NET i Web Tools 2012.2 zawiera szablon aplikacji jednostronicowej (SPA) dla platformy ASP.NET MVC 4. Ten szablon jest przeznaczony do ułatwiające rozpoczęcie pracy szybkie tworzenie aplikacji sieci web interactive po stronie klienta.

"Aplikacja jednostronicowa" (SPA) jest ogólnym terminem dla aplikacji sieci web, która ładuje z pojedynczą stroną HTML, a następnie aktualizuje stronę dynamicznie, zamiast ładowanie nowych stron. Po załadowaniu strony początkowej SPA komunikuje się z serwerem za pośrednictwem żądań AJAX.

![](knockoutjs-template/_static/image1.png)

AJAX to nic nowego, ale obecnie ma platformy JavaScript, które ułatwiają tworzenie i zarządzanie nimi dużej zaawansowanych aplikacji SPA. Ponadto HTML 5 i CSS3 są ułatwia tworzenie rozbudowanych interfejsów użytkownika.

Aby ułatwić rozpoczęcie pracy, ten szablon SPA tworzy przykładową aplikację "Lista zadań do wykonania". W tym samouczku zostaną wykonane Przewodnik po szablonie. Najpierw firma Microsoft będzie Przyjrzyj się sama aplikacja listy zadań do wykonania, a następnie sprawdź elementy technologii, dzięki którym działa.

## <a name="create-a-new-spa-template-project"></a>Utwórz nowy projekt szablonu SPA

Wymagania:

- Program Visual Studio 2012 lub Visual Studio Express 2012 for Web
- Narzędzia sieci Web ASP.NET 2012.2 aktualizacji. Aktualizację można zainstalować [tutaj](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Uruchom program Visual Studio i wybierz **nowy projekt** ze strony początkowej. Lub z **pliku** menu, wybierz opcję **New** i następnie **projektu**.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz opcję **Web**. Na liście szablonów projektu wybierz **aplikacji sieci Web programu ASP.NET MVC 4**. Nadaj projektowi nazwę, a następnie kliknij przycisk **OK**.

![](knockoutjs-template/_static/image2.png)

W **nowy projekt** kreatora wybierz **aplikacji jednostronicowej**.

![](knockoutjs-template/_static/image3.png)

Naciśnij klawisz F5, aby skompilować i uruchomić aplikację. Po pierwszym uruchomieniu aplikacji wyświetli ekran logowania.

![](knockoutjs-template/_static/image4.png)

Kliknij przycisk &quot;Zarejestruj&quot; połączyć, a następnie Utwórz nowego użytkownika.

![](knockoutjs-template/_static/image5.png)

Po zalogowaniu, aplikacja tworzy domyślnej listy zadań do wykonania z dwoma elementami. Kliknij przycisk "Dodaj listy zadań do wykonania", aby dodać nową listę.

![](knockoutjs-template/_static/image6.png)

Lista zmiany nazwy, dodać elementy do listy i sprawdź je. Możesz również usunąć elementy lub usunąć całą listę. Zmiany są automatycznie zachowywane do bazy danych na serwerze (faktycznie LocalDB w tym momencie, ponieważ aplikacja jest uruchomiony lokalnie).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Architektura szablonu SPA

Ten diagram przedstawia główne bloki konstrukcyjne aplikacji.

![](knockoutjs-template/_static/image8.png)

Po stronie serwera platformy ASP.NET MVC obsługuje kod HTML i obsługuje również uwierzytelnianie oparte na formularzach.

ASP.NET Web API obsługuje wszystkie żądania, które odnoszą się do ToDoLists i ToDoItems, w tym pobieranie, tworzenie, aktualizowanie i usuwanie. Klient wymienia dane z interfejsu API sieci Web w formacie JSON.

Entity Framework (EF) to warstwa Obiektowo. Jego pośredniczy między świecie zorientowane obiektowo, platformy ASP.NET i podstawowej bazy danych. Baza danych używa LocalDB, ale można to zmienić w pliku Web.config. Zwykle będzie użyć programu LocalDB do tworzenia aplikacji lokalnej, a następnie wdrożyć do bazy danych SQL na serwerze za pomocą migracji najpierw kod programem EF.

Po stronie klienta biblioteki struktura Knockout.js obsługuje aktualizacje z wysyłanie żądań AJAX. Knockout używa powiązanie danych w celu synchronizowania strony przy użyciu najnowszych danych. Dzięki temu nie trzeba pisać kodu, który przeprowadzi dane JSON i aktualizuje DOM. Zamiast tego należy umieścić deklaratywne atrybuty w formacie HTML, informacje Knockout sposobu prezentowania danych.

Dużą zaletą tej architektury jest oddziela warstwy prezentacji, od logiki aplikacji. Można utworzyć części interfejsu API sieci Web bez znajomości wygląd strony sieci web. Po stronie klienta, utworzyć model"Widok" do reprezentowania danych, a model widoku używa Knockout Aby powiązać kod HTML. Która pozwala łatwo zmienić kod HTML bez wprowadzania zmian w modelu widoku. (Przyjrzymy Knockout nieco później.)

## <a name="models"></a>Modele

W projekcie programu Visual Studio folder modeli zawiera modeli, które są używane po stronie serwera. (Dostępne są także modeli po stronie klienta; przejdziemy do tych).

![](knockoutjs-template/_static/image9.png)

**Czynność do wykonania, listy zadań**

Są to modele bazy danych dla programu Entity Framework Code First. Należy zauważyć, że te modele mają właściwości, które wskazują na siebie nawzajem. `ToDoList` zawiera kolekcję ToDoItems, a każdy `ToDoItem` zawiera odwołanie do nadrzędnej listy zadań. Te właściwości są nazywane właściwości nawigacji, a reprezentują one relacji jeden do wielu, listę zadań do wykonania i jego elementów do wykonania.

`ToDoItem` Klasy również używa **[klucza obcego]** atrybutu, aby określić, że `ToDoListId` to klucz obcy do `ToDoList` tabeli. Informuje EF, można dodać ograniczenia klucza obcego w bazie danych.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Te klasy definiują dane, które zostaną wysłane do klienta. "DTO" oznacza "obiekt transferu danych." Obiekt DTO definiuje, jak jednostki będą wykonywane szeregowo w formacie JSON. Ogólnie rzecz biorąc istnieje kilka powodów, aby użyć dto:

- Aby kontrolować, właściwości, które są serializowane. Obiekt DTO może zawierać podzbiór właściwości modelu domeny. Można to zrobić, ze względów bezpieczeństwa (w celu ukrywać dane wrażliwe) lub po prostu Aby zmniejszyć ilość danych przesyłanych.
- Aby zmienić kształt danych, np. do spłaszczenia bardziej złożone struktury danych.
- Aby zachować wszelka logika biznesowa poza obiekt DTO (separacji).
- Jeśli z jakiegoś powodu, nie można serializować modeli domeny. Na przykład odwołania cykliczne może powodować problemy podczas serializacji obiektu istnieją sposoby obsługi tego problemu w interfejsie API sieci Web (zobacz [obsługi odwołań cyklicznych obiektu](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); ale przy użyciu obiekt DTO po prostu pozwala uniknąć tego problemu całkowicie.

W szablonie SPA dto zawiera te same dane, co modeli domeny. Jednak są one nadal przydatne ponieważ one uniknąć odwołania cykliczne z właściwości nawigacji, a wykazują ogólny wzorzec DTO.

**AccountModels.cs**

Ten plik zawiera modele członkostwem w witrynie. `UserProfile` Klasa definiuje schemat dla profilów użytkowników w członkostwie bazy danych. (W tym przypadku tylko informacji jest identyfikator użytkownika i nazwę użytkownika). Inne klasy modelu, w tym pliku są używane do tworzenia formularzy rejestracji i logowania użytkownika.

## <a name="entity-framework"></a>Entity Framework

Szablon SPA używa EF Code First. W rozwiązania deweloperskiego Code First należy najpierw zdefiniować te modele w kodzie i następnie EF używa modelu, aby utworzyć bazę danych. Można również użyć EF z istniejącej bazy danych ([Database First](https://msdn.microsoft.com/data/jj206878.aspx)).

`TodoItemContext` Pochodną klasy w folderze modeli **DbContext**. Ta klasa udostępnia "skleić" między modelami i EF. `TodoItemContext` Przechowuje `ToDoItem` kolekcji i `TodoList` kolekcji. Aby wysłać zapytanie bazy danych, po prostu napisać zapytanie LINQ do tych kolekcji. Na przykład Oto jak można wybrać wszystkie listy zadań do wykonania dla użytkownik "Alicja":

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

Można również dodawania nowych elementów do kolekcji, aktualizować elementy, lub usunąć elementy z kolekcji i utrwalenia zmiany w bazie danych.

## <a name="aspnet-web-api-controllers"></a>Kontrolery ASP.NET Web API

W programie ASP.NET Web API kontrolery są obiekty, które obsługują żądania HTTP. Jak wspomniano wcześniej, szablon SPA używa interfejsu API sieci Web, aby umożliwić operacji CRUD na `ToDoList` i `ToDoItem` wystąpień. Kontrolerów znajdują się w folderze kontrolery rozwiązania.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: Obsługuje żądania HTTP na potrzeby wykonania
- `TodoListController`: Obsługuje żądania HTTP dla listy zadań do wykonania.

Te nazwy są istotne, ponieważ ścieżka identyfikatora URI, aby nazwa kontrolera jest zgodna z interfejsu API sieci Web. (Aby dowiedzieć się, jak internetowy interfejs API przekazuje żądania HTTP do kontrolerów, zobacz [routingu ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Przyjrzyjmy się `ToDoListController` klasy. Zawiera on element członkowski danych pojedynczego:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

`TodoItemContext` Jest używany do komunikowania się z programem EF, zgodnie z wcześniejszym opisem. Metody na kontrolerze implementowania operacji CRUD. Interfejs API sieci Web mapy żądania HTTP od klienta do metod kontrolera w następujący sposób:

| Żądanie HTTP | Metoda kontrolera | Opis |
| --- | --- | --- |
| Pobierz /api/todo | `GetTodoLists` | Pobiera kolekcję listy zadań do wykonania. |
| GET/interfejsAPI/zadania/*identyfikator* | `GetTodoList` | Pobiera listę zadań do wykonania według Identyfikatora |
| PUT/interfejsAPI/zadania/*identyfikator* | `PutTodoList` | Aktualizuje listę zadań do wykonania. |
| OPUBLIKUJ/api/zadań do wykonania | `PostTodoList` | Tworzy nową listę zadań do wykonania. |
| Usuń/interfejsAPI/zadania/*identyfikator* | `DeleteTodoList` | Usuwa listy zadań do wykonania. |

Zwróć uwagę, że identyfikatory URI w przypadku niektórych operacji zawierają symbole zastępcze wartości Identyfikatora. Na przykład, aby usunąć do listy o identyfikatorze 42, identyfikator URI jest `/api/todo/42`.

Aby dowiedzieć się więcej o korzystaniu z interfejsu API sieci Web dla operacji CRUD, zobacz [Tworzenie internetowego interfejsu API tej operacji CRUD obsługuje](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). Kod ten kontroler jest dość prosta. Poniżej przedstawiono kilka interesujących punktów:

- `GetTodoLists` Metoda używa zapytania LINQ do filtrowania wyników w identyfikatorze zalogowanego użytkownika. Dzięki temu użytkownik widzi tylko dane, które należy do niej. Zauważ również, że instrukcja Select jest używana do konwersji `ToDoList` wystąpień do `TodoListDto` wystąpień.
- Metody PUT i WPIS Sprawdź stan modelu przed zmodyfikowaniem bazy danych. Jeśli **ModelState.IsValid** ma wartość false, te metody zwracają HTTP 400 Niewłaściwe żądanie. Dowiedz się więcej o weryfikacji modelu w interfejsie API sieci Web w [sprawdzania poprawności modelu](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- Klasa kontrolera jest również ozdobione **[Authorize]** atrybutu. Ten atrybut umożliwia sprawdzenie, czy żądanie HTTP jest uwierzytelnione. Jeśli żądanie nie jest uwierzytelniony, klient odbierze HTTP 401 Brak autoryzacji. Przeczytaj więcej na temat uwierzytelniania na etapie [uwierzytelnianie i autoryzacja w interfejsie API sieci Web platformy ASP.NET](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

`TodoController` Klasy jest bardzo podobny do `TodoListController`. Największa różnica dotyczy, czy też nie definiuje żadnych metod GET, ponieważ klient otrzyma elementów do wykonania, wraz z każdej listy zadań do wykonania.

## <a name="mvc-controllers-and-views"></a>Widoków i kontrolerów MVC

Kontrolerów MVC również znajdują się w folderze kontrolery rozwiązania. `HomeController` renderuje głównego pliku HTML dla aplikacji. Widok dla kontrolera głównego jest zdefiniowany w Views/Home/Index.cshtml. Widok głównej renderuje różną zawartość w zależności od tego, czy użytkownik jest zalogowany:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Po zalogowaniu użytkownicy widzą głównego interfejsu użytkownika. W przeciwnym razie użytkownik zobaczy na panelu logowania. Należy pamiętać, że to warunkowe renderowania się dzieje po stronie serwera. Nigdy nie próbuje ukryć poufnej zawartości po stronie klienta i 8212anything #, która wysyłać w odpowiedzi HTTP jest widoczny dla kogoś, kto ogląda nieprzetworzone komunikaty HTTP.

## <a name="client-side-javascript-and-knockoutjs"></a>JavaScript po stronie klienta i użyciem Knockout.js

Teraz możemy włączyć po stronie serwera aplikacji do klienta. Szablon SPA używa kombinacji jQuery i struktura Knockout.js do tworzenia smooth, interaktywnego interfejsu użytkownika. Struktura Knockout.js jest bibliotekę JavaScript, który można łatwo powiązać HTML z danymi. Struktura Knockout.js korzysta ze wzorca, o nazwie "Model-View-ViewModel."

- Model jest danych domeny (listy zadań do wykonania i zadania do wykonania).
- Widok jest dokument HTML.
- Model widoku jest obiekt JavaScript, która przechowuje dane modelu. Model widoku jest abstrakcji kodu interfejsu użytkownika. Go nie zna reprezentacji w formacie HTML. Zamiast tego reprezentuje abstrakcyjnej funkcji widoku, takie jak "Lista zadań do wykonania".

Widok jest powiązany z danymi model widoku. Model widoku aktualizacje są automatycznie odzwierciedlane w widoku. Powiązania działa odwrotnie także. Zdarzeń w modelu DOM (takie jak kliknięcie) są powiązane z danymi do funkcji na model widoku, które wyzwalają wywołania AJAX.

Szablon SPA organizuje JavaScript po stronie klienta w trzech warstwach:

- TODO.DataContext.js: wysyła żądania AJAX.
- TODO.model.js: definiuje modeli.
- TODO.ViewModel.js: definiuje model widoku.

![](knockoutjs-template/_static/image11.png)

Te pliki skryptów znajdują się w folderze Skrypty/aplikacji, rozwiązania.

![](knockoutjs-template/_static/image12.png)

**TODO.DataContext** obsługuje wszystkie wywołania AJAX do kontrolerów internetowych interfejsów API. (Wywołania AJAX do zalogowania się są definiowane w innych miejscach, w ajaxlogin.js.)

**TODO.model.js** definiuje modeli po stronie klienta (przeglądarka) do listy zadań do wykonania. Istnieją dwie klasy modelu: todoItem i listy zadań.

Wiele właściwości w klasach modeli są typu "ko.observable". Dostrzegalne elementy są na tym, jakie Knockout jego magic. Z [dokumentacji Knockout](http://knockoutjs.com/documentation/introduction.html): zauważalny jest "JavaScript obiekt, który może generować powiadomienia subskrybentów o zmianach." Po zmianie wartości zauważalny Knockout aktualizacji żadne elementy HTML, które są powiązane z tymi dostrzegalne elementy. Na przykład todoItem ma dostrzegalne elementy właściwości tytułu i isDone:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

Istnieje również możliwość subskrybowania zauważalny w kodzie. Na przykład klasę todoItem subskrybuje zmian właściwości "isDone" i "title":

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Model widoku**

Model widoku jest zdefiniowany w todo.viewmodel.js. Model widoku jest centralnym miejscem, gdzie powiązana aplikacja elementy strony HTML dane domeny. W szablonie SPA modelu widoku zawiera tablicę dostrzegalnych todoLists. Poniższy kod w modelu widoku nakazuje Knockout do zastosowania powiązania:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML i powiązania danych

W Views/Home/Index.cshtml zdefiniowano głównego pliku HTML dla strony. Ponieważ używamy powiązania danych HTML jest tylko szablon co faktycznie pobiera renderowane. Używa knockout *deklaratywne* powiązania. Elementy strony jest powiązany z danymi przez dodanie atrybutu "data-bind" do elementu. Poniżej przedstawiono bardzo prosty przykład, pobierane z dokumentacji odcinania:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

W tym przykładzie Knockout aktualizuje **&lt;span&gt;** element z wartością `myItems.count()`. Zawsze, gdy ta wartość zmienia, Knockout aktualizuje dokument.

Knockout udostępnia wiele typów inne powiązanie. Oto niektóre powiązania, używany w szablonie SPA:

- **Instrukcja foreach**: pozwala wykonać iterację pętli i zastosować ten sam kod znaczników do każdego elementu na liście. Służy to do renderowania list zadań do wykonania i elementów do wykonania. W ramach **foreach**, powiązania są stosowane do elementów listy.
- **widoczne**: umożliwia przełączyć widoczność. Ukryj znaczników, gdy kolekcja jest pusta lub wyświetlić komunikat o błędzie.
- **wartość**: używany do wypełnienia wartości formularza.
- **Kliknij przycisk**: wiąże Zdarzenie kliknięcia do funkcji na model widoku.

## <a name="anti-csrf-protection"></a>Ochrony chroniących CSRF

Cross-Site fałszowaniu żądań Międzywitrynowych to atak, w którym złośliwych witryn wysyła żądanie do lokacji narażony, gdzie użytkownik jest aktualnie zalogowany. Aby zapobiec atakom CSRF, używa platformy ASP.NET MVC *tokeny zabezpieczające przed fałszerstwem*, nazywany również żądania weryfikacji tokenów. Chodzi o to, że serwer polega na spakowaniu losowo generowany token do strony sieci web. Gdy klient przesyła dane na serwerze, musi on zawierać tę wartość w komunikacie żądania.

Tokeny zabezpieczające przed fałszerstwem działać, ponieważ złośliwy strony nie może odczytać tokenów użytkownika, ze względu na zasady tego samego źródła. (Zasady tego samego źródła uniemożliwiają hostowanych w dwóch różnych witrynach dostęp do siebie nawzajem zawartości dokumentów).

ASP.NET MVC udostępnia wbudowaną obsługę tokenów zabezpieczających przed sfałszowaniem za pośrednictwem [AntiForgery](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx) klasy i [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx) atrybutu. Obecnie ta funkcja nie jest wbudowana do interfejsu API sieci Web. Jednak szablonu SPA zawiera implementację niestandardową dla interfejsu API sieci Web. Ten kod jest zdefiniowany w `ValidateHttpAntiForgeryTokenAttribute` klasy, która znajduje się w folderze filtry rozwiązania. Aby dowiedzieć się więcej na temat anti-CSRF w interfejsie API sieci Web, zobacz [ataków zapobieganie Cross-Site żądania Międzywitrynowego Międzywitrynowych](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Wniosek

Szablon SPA został zaprojektowany ułatwią Ci rozpoczęcie pracy szybko pisania nowoczesnych, interaktywnych aplikacji sieci web. Używa biblioteki struktura Knockout.js do oddzielania prezentację (kod znaczników HTML) z danych i aplikacji logiki. Ale separowania na ostro nie tylko biblioteki JavaScript, których można używać do tworzenia SPA. Jeśli chcesz poznać kilka innych opcji, zapoznaj się z [szablonów utworzonych przez społeczność aplikacji JEDNOSTRONICOWYCH](../templates/index.md).
