---
title: Tworzenie usług zaplecza dla natywnych aplikacji mobilnych za pomocą programu ASP.NET Core
author: ardalis
description: Dowiedz się, jak tworzenie usług zaplecza za pomocą platformy ASP.NET Core MVC do obsługi natywnych aplikacji mobilnych.
ms.author: riande
ms.date: 10/14/2016
uid: mobile/native-mobile-backend
ms.openlocfilehash: 13149dd4b877b8c17d33d428779ad31d8c51ae9e
ms.sourcegitcommit: 687ffb15ebe65379f75c84739ea851d5a0d788b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488731"
---
# <a name="create-backend-services-for-native-mobile-apps-with-aspnet-core"></a>Tworzenie usług zaplecza dla natywnych aplikacji mobilnych za pomocą programu ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Aplikacje mobilne mogą komunikować się z usługami zaplecza programu ASP.NET Core. Aby uzyskać instrukcje dotyczące łączenia usługi sieci web w lokalnych z symulatorach systemu iOS i emulatory systemu Android, zobacz [Połącz z lokalnych usług sieci Web z poziomu systemu iOS symulatorach i emulatory systemu Android](/xamarin/cross-platform/deploy-test/connect-to-local-web-services).

[Wyświetlanie lub pobieranie przykładowego kodu usługi wewnętrznej bazy danych](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a>Przykładowe natywnej aplikacji mobilnej

W tym samouczku pokazano, jak tworzenie usług zaplecza za pomocą platformy ASP.NET Core MVC do obsługi natywnych aplikacji mobilnych. Używa ona [aplikacji Xamarin Forms ToDoRest](/xamarin/xamarin-forms/data-cloud/consuming/rest) jako jego klient natywny, która obejmuje oddzielne klientów natywnych dla systemów Android, iOS, Windows Universal i Windows Phone urządzeń. Można wykonać kroki samouczka połączone do tworzenia aplikacji natywnej (i zainstaluj wymagane bezpłatne narzędzia Xamarin), a także Pobierz przykładowe rozwiązanie platformy Xamarin. Przykład Xamarin obejmuje projekt usług ASP.NET Web API 2 aplikacji ASP.NET Core w tym artykule są zastępowane (bez zmian, wymagane przez klienta).

![Wykonaj pozostałe aplikacji uruchomionych na smartfonie dla systemu Android](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a>Funkcje

Aplikacja ToDoRest obsługuje wyświetlanie, dodawanie i aktualizowania elementów do wykonania. Każdy element ma identyfikator, nazwę, uwagi i właściwości wskazującą, czy go jest zostało to jeszcze zrobione.

Widok główny elementów, jak pokazano powyżej, wyświetla nazwę każdego elementu i wskazuje, jeśli jest wykonywane ze znacznikiem wyboru.

Naciskając `+` ikona otwiera okno dialogowe Dodawanie elementu:

![Okno dialogowe elementu Dodawanie](native-mobile-backend/_static/todo-android-new-item.png)

Naciśnięcie elementu na ekranie głównym listy zostanie otwarte okno dialogowe Edycja, gdzie można zmodyfikować nazwę, uwagi i gotowe ustawienia elementu, lub można usunąć elementu:

![Edytuj okno dialogowe elementu](native-mobile-backend/_static/todo-android-edit-item.png)

W tym przykładzie jest domyślnie skonfigurowany do użycia usług zaplecza hostowanych w developer.xamarin.com, dzięki czemu operacji tylko do odczytu. Aby przetestować go samodzielnie względem aplikacji ASP.NET Core utworzona w następnej sekcji, które są uruchomione na komputerze, musisz zaktualizować aplikację `RestUrl` stałej. Przejdź do `ToDoREST` projektu, a następnie otwórz *Constants.cs* pliku. Zastąp `RestUrl` adres URL, który zawiera IP Twojego komputera adresów (nie localhost ani niebędący adresem 127.0.0.1, ponieważ ten adres jest używany z emulatora urządzeń, nie z Twojego komputera). Zawierają również numer portu (5000). Aby przetestować działanie usługi z urządzeniem, upewnij się, że nie masz aktywnych Zapora blokuje dostęp do tego portu.

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a>Tworzenie projektu platformy ASP.NET Core

Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core w programie Visual Studio. Wybierz szablon interfejsu API sieci Web i bez uwierzytelniania. Nadaj projektowi nazwę *ToDoApi*.

![Okno dialogowe Nowy aplikacji sieci Web ASP.NET z wybraniu szablonu projektu interfejsu API sieci Web](native-mobile-backend/_static/web-api-template.png)

Aplikacja powinien odpowiedzieć na wszystkie żądania na porcie 5000. Aktualizacja *Program.cs* obejmujący `.UseUrls("http://*:5000")` do osiągnięcia tego:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> Sprawdź, czy uruchomić aplikację bezpośrednio, a nie za usług IIS Express, który ignoruje żądania-local domyślnie. Uruchom [dotnet, uruchom](/dotnet/core/tools/dotnet-run) z wiersza polecenia, lub wybierz nazwę profilu aplikacji z listy rozwijanej Debuguj element docelowy, na pasku narzędzi programu Visual Studio.

Dodaj klasę modelu do reprezentowania elementów do wykonania. Oznacz wymagane pola za pomocą `[Required]` atrybutu:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

Metody interfejsu API wymagają jakiś sposób, aby pracować z danymi. Użyto tych samych `IToDoRepository` interfejsu oryginalnego używa platformy Xamarin w przykładowej:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

W tym przykładzie wdrożenia po prostu używa prywatnego kolekcję elementów:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

Konfigurowanie wdrożenia w *Startup.cs*:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

W tym momencie możesz przystąpić do tworzenia *ToDoItemsController*.

> [!TIP]
> Dowiedz się więcej na temat tworzenia internetowych interfejsów API w [Tworzenie pierwszego interfejsu API sieci Web za pomocą platformy ASP.NET Core MVC i programu Visual Studio](../tutorials/first-web-api.md).

## <a name="creating-the-controller"></a>Tworzenie kontrolera

Dodaj nowy kontroler do projektu, *ToDoItemsController*. It should inherit from Microsoft.AspNetCore.Mvc.Controller. Dodaj `Route` atrybutu, aby wskazać, że kontroler będzie obsługiwać żądania kierowane do ścieżki, począwszy od `api/todoitems`. `[controller]` Token dla trasy jest zastępowany przez nazwę kontrolera (z pominięciem `Controller` sufiks) i jest szczególnie przydatne w przypadku globalnych trasy. Dowiedz się więcej o [routingu](../fundamentals/routing.md).

Wymaga kontrolera `IToDoRepository` do funkcji; żądania wystąpienia tego typu za pośrednictwem konstruktora kontrolera. W czasie wykonywania, to wystąpienie uwarunkowane przestrzeganiem dzięki obsłudze struktury [wstrzykiwanie zależności](../fundamentals/dependency-injection.md).

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

Ten interfejs API obsługuje czterech różnych czasowników HTTP do wykonywania operacji CRUD (tworzenia, odczytu, Update, Delete) w źródle danych. Najprostsza to jest operacja odczytu, która odpowiada na żądania HTTP GET.

### <a name="reading-items"></a>Trwa odczyt elementów

Żądanie listy elementów odbywa się za pomocą żądanie GET w celu `List` metody. `[HttpGet]` Atrybutu na `List` metoda wskazuje, czy ta akcja tylko powinna obsługiwać żądania GET. Trasy dla tej akcji jest trasy określonych w kontrolerze. Nie jest konieczna użyć nazwy akcji w ramach trasy. Wystarczy upewnić się, że każde działanie ma unikatowy i jednoznacznie trasy. Atrybuty routingu mogą być stosowane na poziomach metody do zbudowania określonej trasy i kontrolera.

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

`List` Metoda zwraca kod odpowiedzi 200 OK i wszystkie elementy zadań do wykonania, zserializowanym w formacie JSON.

Można przetestować nową metodę interfejsu API przy użyciu różnych narzędzi, takich jak [Postman](https://www.getpostman.com/docs/), pokazano poniżej:

![Konsola narzędzia postman, pokazująca, że żądanie GET todoitems i treści odpowiedzi, wyświetlanie kod JSON dla trzech elementów zwróconych](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a>Tworzenie elementów

Zgodnie z Konwencją tworzenie nowych elementów danych jest zamapowana na zlecenie HTTP POST. `Create` Metoda ma `[HttpPost]` zastosowano atrybut i akceptuje `ToDoItem` wystąpienia. Ponieważ `item` argumentów, które zostaną przekazane w treści wpisu, ten parametr zostanie nadany `[FromBody]` atrybutu.

Wewnątrz metody element jest zaznaczony dla ważności i wcześniejsze istnienie w magazynie danych, a jeśli wystąpią żadne problemy, jest ona dodawana przy użyciu repozytorium. Sprawdzanie `ModelState.IsValid` wykonuje [Walidacja modelu](../mvc/models/validation.md)i musi być wykonywane w każdej metody interfejsu API, który akceptuje dane wejściowe użytkownika.

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

W przykładzie użyto wyliczenia zawierający kody błędów, które są przekazywane do klientów urządzeń przenośnych:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

Przetestuj dodawania nowych elementów przy użyciu narzędzia Postman, wybierając zlecenie WPIS, podając nowy obiekt w formacie JSON w treści żądania. Należy również dodać nagłówek żądania określając `Content-Type` z `application/json`.

![Konsola postman pokazująca, że WPIS i odpowiedzi](native-mobile-backend/_static/postman-post.png)

Metoda zwraca nowo utworzonego elementu w odpowiedzi.

### <a name="updating-items"></a>Aktualizowanie elementów

Modyfikowanie rekordów jest wykonywane za pomocą żądania HTTP PUT. Inne niż ta zmiana `Edit` metody jest niemal identyczny `Create`. Należy pamiętać, że jeśli nie odnaleziono rekordu, `Edit` zwróci akcji `NotFound` odpowiedzi (404).

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

Aby przetestować za pomocą narzędzia Postman, zmień zlecenie na PUT. Określ dane zaktualizowanego obiektu w treści żądania.

![Konsola postman przedstawiające PUT i odpowiedzi](native-mobile-backend/_static/postman-put.png)

Ta metoda zwraca `NoContent` odpowiedzi (204), jeśli operacja się powiedzie, w celu zachowania spójności z istniejącym interfejsem API.

### <a name="deleting-items"></a>Usuwanie elementów

Usuwanie rekordów odbywa się przez wprowadzania żądań DELETE służących do usługi i przekazanie identyfikator elementu do usunięcia. Zgodnie z aktualizacjami, otrzyma żądań dla elementów, które nie istnieją `NotFound` odpowiedzi. W przeciwnym razie otrzyma żądania zakończonego powodzeniem `NoContent` odpowiedzi (204).

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

Należy pamiętać, że po testowania funkcji usuwania, nic nie musi w treści żądania.

![Konsola postman pokazująca, że DELETE i odpowiedzi](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a>Typowych Konwencji interfejsu API sieci Web

Podczas opracowywania usług zaplecza dla aplikacji, należy stworzyć zestaw spójne konwencje lub zasady dotyczące postępowania odciąż przekrojowe zagadnienia. Na przykład w usłudze powyżej żądań dla określonych rekordów, które nie zostały odnalezione otrzymywanych `NotFound` odpowiedzi, a nie `BadRequest` odpowiedzi. Podobnie, polecenia wprowadzone do tej usługi, które są przekazywane w typach powiązana z modelem zawsze zaznaczone `ModelState.IsValid` i zwrócony `BadRequest` dla typów nieprawidłowy model.

Po zidentyfikowaniu wspólne zasady dla interfejsów API, można zwykle hermetyzację go w [filtru](../mvc/controllers/filters.md). Dowiedz się więcej o [jak hermetyzacji wspólnych zasad interfejsu API w aplikacji ASP.NET Core MVC](https://msdn.microsoft.com/magazine/mt767699.aspx).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Uwierzytelnianie i autoryzacja](/xamarin/xamarin-forms/enterprise-application-patterns/authentication-and-authorization)
