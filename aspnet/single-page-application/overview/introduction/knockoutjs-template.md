---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'Jednej strony aplikacji: Szablon elementami KnockoutJS | Dokumentacja firmy Microsoft'
author: MikeWasson
description: Szablon odcinania
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 6e84dcc16345e33fcd3a3f83c4b35bc993c03ca6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="single-page-application-knockoutjs-template"></a>Jednej strony aplikacji: Szablon elementami KnockoutJS
====================
przez [Wasson Jan](https://github.com/MikeWasson)

> Szablon MVC odcinania jest częścią programu ASP.NET i 2012.2 narzędzi sieci Web
> 
> [Pobierz program ASP.NET i narzędzia sieci Web 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)


Aktualizacja programu ASP.NET i 2012.2 narzędzia sieci Web zawiera szablon aplikacji jednostronicowej (SPA) dla platformy ASP.NET MVC 4. Ten szablon jest przeznaczony do ułatwiające rozpoczęcie pracy szybkie opracowywanie aplikacji sieci web po stronie klienta interactive.

"Jednostronicowej aplikacji" (SPA) jest ogólny termin dla aplikacji sieci web, który ładuje pojedynczej strony HTML, a następnie aktualizuje stronę dynamicznie, zamiast ładowania nowych stron. Po załadowaniu stronę początkową aplikacja JEDNOSTRONICOWA komunikuje się z serwerem za pośrednictwem żądania AJAX.

![](knockoutjs-template/_static/image1.png)

AJAX ma nowych danych, ale obecnie istnieją struktury języka JavaScript, ułatwiające tworzenie i obsługa dużej aplikacji JEDNOSTRONICOWEJ zaawansowane. Ponadto HTML 5 i CSS3 są ułatwiając tworzenie rozbudowanych UI.

Ułatwiające rozpoczęcie pracy, szablon SPA tworzenia przykładowej aplikacji "Lista zadań do wykonania". W tym samouczku zostaną wykonane samouczek szablonu. Najpierw firma Microsoft będzie przyjrzeć się samej aplikacji listy zadań do wykonania, a następnie przejrzyj elementy technologia, dzięki któremu można pracować.

## <a name="create-a-new-spa-template-project"></a>Utwórz nowy projekt szablonu SPA

Wymagania:

- Program Visual Studio 2012 lub Visual Studio Express 2012 for Web
- Narzędzia sieci Web ASP.NET 2012.2 aktualizacji. Zostanie zainstalowana aktualizacja [tutaj](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Uruchom program Visual Studio i wybierz **nowy projekt** ze strony początkowej. Lub z **pliku** menu, wybierz opcję **nowy** , a następnie **projektu**.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz pozycję **Web**. Na liście szablony projektów, wybierz **aplikacji sieci Web programu ASP.NET MVC 4**. Nazwij projekt i kliknij przycisk **OK**.

![](knockoutjs-template/_static/image2.png)

W **nowy projekt** kreatora wybierz **jednej strony aplikacji**.

![](knockoutjs-template/_static/image3.png)

Naciśnij klawisz F5, aby skompilować i uruchomić aplikację. Po pierwszym uruchomieniu aplikacji wyświetla ekran logowania.

![](knockoutjs-template/_static/image4.png)

Kliknij przycisk &quot;Zarejestruj&quot; link i utworzenie nowego użytkownika.

![](knockoutjs-template/_static/image5.png)

Po zalogowaniu, aplikacja tworzy domyślnej listy Todo z dwoma elementami. Kliknij przycisk "Dodaj lista czynności do wykonania" Aby dodać nową listę.

![](knockoutjs-template/_static/image6.png)

Zmień nazwę listy, Dodaj elementy do listy i je odznaczyć. Można również usunąć elementy lub usunąć całą listę. Zmiany są automatycznie zachowywane w bazie danych na serwerze (faktycznie LocalDB w tym momencie, ponieważ aplikacja jest uruchomiony lokalnie).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Architektura szablonu SPA

Ten diagram przedstawia główne bloków konstrukcyjnych dla aplikacji.

![](knockoutjs-template/_static/image8.png)

Po stronie serwera ASP.NET MVC obsługuje HTML i również obsługuje uwierzytelnianie oparte na formularzach.

Interfejs API sieci Web platformy ASP.NET obsługuje wszystkie żądania, które odnoszą się do ToDoLists i ToDoItems, w tym pobieranie, tworzenie, aktualizowanie i usuwanie. Klient wymienia danych za pomocą interfejsu API sieci Web w formacie JSON.

Entity Framework (EF) to warstwa O/Menedżera zasobów. Przekazuje on między world zorientowane obiektowo programu ASP.NET i podstawowej bazy danych. Baza danych używa LocalDB, ale można go zmienić w pliku Web.config. Zazwyczaj będzie używać LocalDB dla rozwoju lokalnych, a następnie wdrożyć do bazy danych SQL na serwerze, za pomocą migracji pierwszy kod EF.

Po stronie klienta biblioteki Knockout.js obsługuje aktualizacje z żądania AJAX. Odcinania używa powiązanie danych w celu synchronizowania strony przy użyciu najnowszych danych. Dzięki temu nie trzeba pisać kod, który przeprowadzi Cię przez dane JSON i aktualizuje modelu DOM. Zamiast tego należy umieścić deklaratywne atrybuty w kodzie HTML, który poinformuje odcinania jak przedstawiają dane.

Duży zaletą tej architektury jest to oddziela warstwę prezentacji, z logiki aplikacji. Można utworzyć części interfejsu API sieci Web bez znajomości wygląd strony sieci web. Po stronie klienta utworzyć model"Widok" do reprezentowania danych, a model widoku używa odcinania powiązać HTML. Która pozwala łatwo zmienić kod HTML bez zmiany modelu widoku. (Przyjrzymy odcinania nieco później.)

## <a name="models"></a>Modele

W projekcie programu Visual Studio folderu modeli zawiera modeli, które są używane po stronie serwera. (Dostępne są również modele po stronie klienta; możemy uzyskać tych).

![](knockoutjs-template/_static/image9.png)

**TodoItem, listy zadań**

Są to bazy danych modeli Entity Framework Code First. Zwróć uwagę, że te modele mają właściwości, odnoszące się do siebie. `ToDoList`zawiera kolekcję ToDoItems i każde `ToDoItem` zawiera odwołanie do nadrzędnej listy zadań. Te właściwości są nazywane właściwości nawigacji, i reprezentują relacji jeden do wielu, listy zadań do wykonania i jego elementów do wykonania.

`ToDoItem` Klasy również używa **[ForeignKey]** atrybutu, aby określić, że `ToDoListId` jest kluczem obcym do `ToDoList` tabeli. Ta wartość informuje EF można dodać ograniczenia klucza obcego do bazy danych.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Te klasy definiują dane, które zostaną wysłane do klienta. "DTO" oznacza "obiekt transferu danych." DTO definiuje sposób jednostek zostaną zserializowane w formacie JSON. Ogólnie rzecz biorąc istnieje kilka przyczyn, aby użyć DTOs:

- Aby sterować właściwości, które są serializowane. DTO może zawierać podzbiór właściwości z modelu domeny. Można to zrobić, ze względów bezpieczeństwa (ukryć dane poufne) lub po prostu Aby zmniejszyć ilość danych, który można wysłać.
- Aby zmienić kształt danych — np. do spłaszczenia bardziej złożonych struktury danych.
- Aby zachować wszelka logika biznesowa poza DTO (separacji).
- Jeśli z jakiegoś powodu nie można szeregować do modeli domeny. Na przykład odwołania cykliczne może spowodować problemy podczas szeregowania obiektu istnieją sposoby obsługi tego problemu w składniku Web API (zobacz [obsługi odwołań cyklicznych obiektu](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); ale przy użyciu DTO po prostu pozwala uniknąć tego problemu całkowicie.

W szablonie SPA DTOs zawiera te same dane jako modeli domeny. Jednak są nadal przydatne ponieważ eliminują odwołań cyklicznych z właściwości nawigacji, a wykazują ogólne wzorzec DTO.

**AccountModels.cs**

Ten plik zawiera modele przynależność do lokacji. `UserProfile` Klasa definiuje schemat profilów użytkowników w członkostwie bazy danych. (W takim przypadku tylko informacje jest identyfikator użytkownika i nazwę użytkownika). Inne klasy modelu w tym pliku są używane do tworzenia formularzy rejestracji i logowania użytkownika.

## <a name="entity-framework"></a>Entity Framework

Szablon SPA używa EF Code First. W rozwoju Code First najpierw zdefiniować modele do uwzględnienia w kodzie, a następnie EF używa modelu do utworzenia bazy danych. Można również użyć EF z istniejącej bazy danych ([Database First](https://msdn.microsoft.com/en-us/data/jj206878.aspx)).

`TodoItemContext` Pochodną klasy w folderze modele **DbContext**. Ta klasa udostępnia "sklejki" między modelami i EF. `TodoItemContext` Przechowuje `ToDoItem` kolekcji i `TodoList` kolekcji. Aby w bazie danych, po prostu zapisu kwerenda LINQ na kolekcjach. Na przykład Oto jak można wybrać wszystkie listy zadań do wykonania dla użytkownika "Alicja":

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

Można również dodawania nowych elementów do kolekcji, zaktualizować elementy, lub usuń elementy z kolekcji i utrwalania zmian w bazie danych.

## <a name="aspnet-web-api-controllers"></a>Kontrolery ASP.NET Web API

W interfejsie API sieci Web ASP.NET kontrolery są obiekty, które obsługują żądania HTTP. Jak wspomniano, szablon SPA używa interfejsu API sieci Web, aby umożliwić operacje CRUD na `ToDoList` i `ToDoItem` wystąpień. Kontrolery znajdują się w folderze kontrolery rozwiązania.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: Obsługuje żądania HTTP dla elementów do wykonania
- `TodoListController`: Obsługuje żądania HTTP do listy zadań do wykonania.

Te nazwy są istotne, ponieważ ścieżka identyfikatora URI, nazwy kontrolera jest zgodna z interfejsu API sieci Web. (Aby dowiedzieć się, jak interfejsu API sieci Web kieruje żądania HTTP do kontrolerów, zobacz [routingu na platformie ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).)

Przyjrzyjmy się `ToDoListController` klasy. Zawiera ona członka danych jednego:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

`TodoItemContext` Jest używany do komunikacji z EF, zgodnie z wcześniejszym opisem. Metody na kontrolerze implementuje operacji CRUD. Interfejs API sieci Web mapy żądania HTTP od klienta do metod kontrolera, w następujący sposób:

| Żądania HTTP | Kontroler — metoda | Opis |
| --- | --- | --- |
| Pobierz /api/todo | `GetTodoLists` | Pobiera kolekcję listy zadań do wykonania. |
| GET/api/zadania/*id* | `GetTodoList` | Pobiera listę zadań do wykonania według Identyfikatora |
| Umieść/api/zadania/*id* | `PutTodoList` | Aktualizuje listy zadań do wykonania. |
| POST/api/todo | `PostTodoList` | Tworzy nową listę zadań do wykonania. |
| Usuń/api/zadania/*id* | `DeleteTodoList` | Usuwa lista czynności do wykonania. |

Powiadomienie, że identyfikator URI dla niektórych operacji zawierają symbole zastępcze wartości Identyfikatora. Na przykład, aby usunąć do listy o identyfikatorze 42, identyfikator URI jest `/api/todo/42`.

Aby dowiedzieć się więcej o korzystaniu z interfejsu API sieci Web dla operacji CRUD, zobacz [Tworzenie składnika Web API ten obsługuje operacje CRUD](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). Kod dla tego kontrolera jest bardzo prosta. Oto niektóre ciekawe:

- `GetTodoLists` Metoda używa zapytania LINQ mają być filtrowane wyniki według Identyfikatora zalogowanego użytkownika. Dzięki temu użytkownik widzi tylko dane, które należy do niej. Zauważ również, że instrukcja Select służy do konwertowania `ToDoList` wystąpień do `TodoListDto` wystąpień.
- Metody PUT i POST Sprawdź stan modelu przed zmodyfikowaniem bazy danych. Jeśli **ModelState.IsValid** ma wartość false, te metody zwracają HTTP 400 Niewłaściwe żądanie. Dowiedz się więcej o weryfikacji modelu w interfejsie API sieci Web w [sprawdzania poprawności modelu](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- Klasa kontrolera jest również ozdobione **[Authorize]** atrybutu. Ten atrybut umożliwia sprawdzenie, czy żądanie HTTP jest uwierzytelniane. Jeśli żądanie nie jest uwierzytelniony, klient odbierze HTTP 401 Unauthorized. Przeczytaj więcej na temat uwierzytelniania na [uwierzytelnianie i autoryzację w ASP.NET Web API](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

`TodoController` Klasy jest bardzo podobny do `TodoListController`. Największych różnica polega na tym że nie definiuje żadnych metod GET, ponieważ klient pobierze wykonania wraz z każdym listy zadań do wykonania.

## <a name="mvc-controllers-and-views"></a>Widoków i kontrolerów MVC

Kontrolerów MVC również znajdują się w folderze kontrolery rozwiązania. `HomeController`renderuje głównego HTML dla aplikacji. Widok dla głównej kontroler jest zdefiniowany w Views/Home/Index.cshtml. Widok głównej renderuje zawartość różne w zależności od tego, czy użytkownik jest zalogowany:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Gdy użytkownicy są zalogowani, zostanie wyświetlona głównego interfejsu użytkownika. W przeciwnym razie zostanie wyświetlona na panelu logowania. Należy zwrócić uwagę na to, że ta warunkowego renderowania ma być wykonywana po stronie serwera. Nigdy nie próbuje ukryć poufnej zawartości po stronie klienta & 8212anything #, który możesz wysłać w odpowiedzi HTTP jest widoczny dla osoby, która obserwuje raw wiadomości HTTP.

## <a name="client-side-javascript-and-knockoutjs"></a>JavaScript po stronie klienta i Knockout.js

Teraz możemy zacząć od strony serwera aplikacji do klienta. Szablon SPA używa kombinacji jQuery i Knockout.js można utworzyć smooth, interaktywnego interfejsu użytkownika. Knockout.js jest biblioteki JavaScript, która ułatwia powiązanie HTML z danymi. Knockout.js korzysta ze wzorca o nazwie "Model-View-ViewModel."

- Model jest danych domeny (ToDo list i zadań do wykonania).
- Widok nie zawiera dokumentu HTML.
- Model widoku jest obiekt JavaScript, która przechowuje dane modelu. Model widoku jest abstrakcji kodu interfejsu użytkownika. Go nie ma informacji o reprezentacji w formacie HTML. Zamiast tego reprezentuje funkcje abstrakcyjne widoku, takie jak "Lista zadań do wykonania".

Widok jest powiązany z danymi model widoku. Aktualizacje na model widoku są automatycznie odzwierciedlane w widoku. Powiązania działają inne kierunek również. Zdarzenia w modelu DOM (takie jak kliknięcie) są powiązane z danymi do funkcji na model widoku podlegających wywołania AJAX.

Szablon SPA organizuje JavaScript po stronie klienta w trzech warstw:

- TODO.DataContext.js: wysyła żądania AJAX.
- TODO.model.js: definiuje modeli.
- TODO.ViewModel.js: definiuje model widoku.

![](knockoutjs-template/_static/image11.png)

Te pliki skryptów znajdują się w folderze skryptów/app rozwiązania.

![](knockoutjs-template/_static/image12.png)

**TODO.DataContext** obsługuje wszystkie wywołania AJAX do kontrolerów interfejsu API sieci Web. (Wywołania AJAX dla logowania są zdefiniowane w innym miejscu w ajaxlogin.js.)

**TODO.model.js** definiuje modeli po stronie klienta (przeglądarki) do listy zadań do wykonania. Istnieją dwie klasy modelu: todoItem i listy zadań.

Wiele właściwości w modelu klasy jest typu "ko.observable". Dostrzegalne elementy są, jak jego magic odcinania. Z [dokumentacji odcinania](http://knockoutjs.com/documentation/introduction.html): zauważalny jest "JavaScript obiekt, który może powiadomić subskrybentów o zmianach." Po zmianie wartości zauważalny odcinania aktualizuje żadne elementy HTML, które są powiązane z tymi dostrzegalne elementy. Na przykład todoItem ma dostrzegalne elementy dla właściwości title i isDone:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

Można również subskrybować zauważalny w kodzie. Na przykład klasa todoItem subskrybuje zmian właściwości "isDone" i "title":

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Model widoku**

Model widoku jest zdefiniowany w todo.viewmodel.js. Model widoku jest punkt centralny, gdzie aplikacja wiąże elementy na stronie HTML danych domeny. W szablonie SPA model widoku zawiera tablicę zauważalne todoLists. Następujący kod w modelu widoku informuje odcinania dotyczyć powiązania:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML i powiązanie danych

Główne HTML na stronie jest zdefiniowany w Views/Home/Index.cshtml. Ponieważ używamy wiązania danych, HTML jest tylko szablon co faktycznie pobiera renderowane. Używa odcinania *deklaratywne* powiązania. Elementy na stronie jest powiązany z danymi przez dodanie atrybutu "data-bind" do elementu. W tym miejscu jest bardzo prosty przykład, z dokumentacją odcinania:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

W tym przykładzie odcinania aktualizuje zawartość  **&lt;span&gt;**  elementu o wartości `myItems.count()`. Przy każdej zmianie tej wartości odcinania aktualizuje dokument.

Odcinania zawiera wiele typów inne powiązanie. Oto niektóre powiązania używane w szablonie SPA:

- **Instrukcja foreach**: umożliwia iteracji pętli i zastosowania tego samego kodu znaczników do każdego elementu listy. To jest używany do renderowania listy zadań do wykonania i elementów do wykonania. W ramach **foreach**, powiązania są stosowane do elementów listy.
- **widoczne**: pozwala włączyć widoczność. Ukryj znaczników, gdy kolekcja jest pusta lub wyświetlić komunikat o błędzie.
- **wartość**: używany do wypełnienia wartości formularza.
- **Kliknij przycisk**: wiąże zdarzenie click funkcja na model widoku.

## <a name="anti-csrf-protection"></a>Ochrona przed CSRF

Sfałszowaniem żądania między witrynami (CSRF) jest atak, gdzie niebezpiecznej witryny wysyła żądanie do narażone lokacji, w którym użytkownik jest aktualnie zalogowany. Aby zapobiec atakom CSRF, korzysta z platformy ASP.NET MVC *tokenów zabezpieczających przed sfałszowaniem*, nazywany również żądania weryfikacji tokenów. Pomysł to, że serwer umieszcza losowo wygenerowany token strony sieci web. Gdy klient przesyła dane do serwera, ta wartość musi zawierać w komunikacie żądania.

Tokenów zabezpieczających przed sfałszowaniem działać, ponieważ złośliwy strony nie może odczytać tokenów użytkownika, z powodu zasad tego samego źródła. (Tego samego źródła zasady uniemożliwiają dokumenty hostowanej w dwóch różnych witrynach dostęp do siebie nawzajem zawartości).

ASP.NET MVC udostępnia wbudowaną obsługę tokenów zabezpieczających przed sfałszowaniem za pośrednictwem [AntiForgery](https://msdn.microsoft.com/en-us/library/system.web.helpers.antiforgery.aspx) klasy i [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute.aspx) atrybutu. Ta funkcja nie jest obecnie wbudowane w interfejsu API sieci Web. Jednak szablon SPA zawiera niestandardowej implementacji interfejsu API sieci Web. Ten kod jest zdefiniowany w `ValidateHttpAntiForgeryTokenAttribute` klasy, która znajduje się w folderze filtry rozwiązania. Aby dowiedzieć się więcej na temat anti-CSRF w składniku Web API, zobacz [ataków interfejsu uniemożliwia Cross-Site żądania Międzywitrynowego (CSRF)](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Wniosek

Szablon SPA został zaprojektowany ułatwiających rozpoczęcie pracy szybko pisanie aplikacji sieci web nowoczesny, interaktywnego. Biblioteka Knockout.js używa do oddzielnej prezentacji (kod znaczników HTML) z danych i aplikacji logiki. Ale odcinania nie jest tylko biblioteka języka JavaScript, których można używać do tworzenia SPA. Jeśli chcesz zapoznać się z innymi opcjami, Przyjrzyjmy się [szablony utworzone społeczności SPA](../templates/index.md).
