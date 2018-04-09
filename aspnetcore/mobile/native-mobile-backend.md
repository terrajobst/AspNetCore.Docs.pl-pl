---
title: Tworzenie usługi wewnętrznej bazy danych dla natywnych aplikacji mobilnych z platformy ASP.NET Core
author: ardalis
description: Dowiedz się, jak tworzyć przy użyciu platformy ASP.NET Core MVC do obsługi natywnych aplikacji mobilnych usługi wewnętrznej bazy danych.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mobile/native-mobile-backend
ms.openlocfilehash: 18aecea00eb9cda3462ede7e478616a99cf302f8
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="create-backend-services-for-native-mobile-apps-with-aspnet-core"></a>Tworzenie usługi wewnętrznej bazy danych dla natywnych aplikacji mobilnych z platformy ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Aplikacje mobilne łatwo mogą komunikować się z usługami zaplecza ASP.NET Core.

[Wyświetlić lub pobrać przykładowy kod usługi wewnętrznej bazy danych](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a>Przykładowe natywnych aplikacji mobilnej

Ten samouczek przedstawia sposób tworzenia usługi wewnętrznej bazy danych przy użyciu platformy ASP.NET Core MVC do obsługi natywnych aplikacji mobilnych. Używa [aplikacji platformy Xamarin Forms ToDoRest](/xamarin/xamarin-forms/data-cloud/consuming/rest) jako jego klientami, która obejmuje oddzielne klientach natywnych dla urządzeń systemu Android, iOS, uniwersalnych systemu Windows i Windows Phone. Można wykonaj połączonego samouczek do tworzenia aplikacji natywnej (i zainstalować niezbędne bezpłatnych narzędzi Xamarin), a także Pobierz przykładowe rozwiązanie Xamarin. Przykładowe Xamarin zawiera projekt usługi ASP.NET Web API 2 aplikacji platformy ASP.NET Core w tym artykule zastępuje (bez zmian wymaganych przez klienta).

![Wykonaj pozostałe aplikacji uruchomionych na smartfonie systemu Android](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a>Funkcje

Aplikacja ToDoRest obsługuje wyświetlanie, dodawanie i aktualizowanie elementów do wykonania. Każdy element ma identyfikator, nazwę uwagi i Właściwość wskazująca, czy jego jest zostało to jeszcze zrobione.

Głównym widok elementów, jak pokazano powyżej, wyświetla każdy element name i wskazuje, jeśli jest przeprowadzana ze znacznikiem wyboru.

Naciskając `+` ikona otwiera okno dialogowe Dodawanie elementu:

![Dodaj element okna dialogowego](native-mobile-backend/_static/todo-android-new-item.png)

Naciskając element na ekranie głównym listy otwiera okno dialogowe Edycja, gdzie można zmodyfikować nazwę, uwagi i gotowe ustawienia elementu, lub można usunąć elementu:

![Element okno dialogowe Edycja](native-mobile-backend/_static/todo-android-edit-item.png)

Ten przykład jest domyślnie skonfigurowany do korzystania z usług wewnętrznej bazy danych hostowanej w lokalizacji developer.xamarin.com, które pozwalają operacji tylko do odczytu. Do przetestowania go się samodzielnie aplikacji platformy ASP.NET Core utworzone w następnej sekcji, które są uruchomione na komputerze, musisz zaktualizować aplikację `RestUrl` stałej. Przejdź do `ToDoREST` projektu i Otwórz *Constants.cs* pliku. Zastąp `RestUrl` adres URL, który obejmuje IP tego komputera adresów (nie localhost ani niebędący adresem 127.0.0.1, ponieważ ten adres jest używany z emulatora urządzenia, nie z komputera). Zawierają również numer portu (5000). Aby przetestować działanie usługi z urządzeniem, upewnij się, że masz aktywne Zapora blokuje dostęp do tego portu.

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a>Tworzenie projektu platformy ASP.NET Core

Utwórz nową aplikację sieci Web platformy ASP.NET Core w programie Visual Studio. Wybierz szablon interfejsu API sieci Web i bez uwierzytelniania. Nazwij projekt *ToDoApi*.

![Okno dialogowe nowego aplikacji sieci Web platformy ASP.NET z wybrany szablon projektu interfejsu API sieci Web](native-mobile-backend/_static/web-api-template.png)

Aplikacja powinno odpowiedzieć na wszystkie żądania kierowane do portu 5000. Aktualizacja *Program.cs* uwzględnienie `.UseUrls("http://*:5000")` w tym:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> Upewnij się, że uruchomienie aplikacji bezpośrednio, a nie za usług IIS Express, który ignoruje żądania innego niż lokalne domyślnie. Uruchom [dotnet Uruchom](/dotnet/core/tools/dotnet-run) z wiersza polecenia, lub wybierz profil Nazwa aplikacji z listy rozwijanej docelowego debugowania na pasku narzędzi programu Visual Studio.

Dodaj klasę modelu do reprezentowania elementów do wykonania. Zaznacz wymagane pola przy użyciu `[Required]` atrybutu:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

Metody interfejsu API wymagają jakiś sposób pracy z danymi. Używać tego samego `IToDoRepository` interfejsu oryginalnego używa próbki Xamarin:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

Dla tego przykładu implementacja używa tylko prywatnej kolekcję elementów:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

Konfigurowanie wdrożenia w *Startup.cs*:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

W tym momencie możesz przystąpić do tworzenia *ToDoItemsController*.

> [!TIP]
> Dowiedz się więcej o tworzeniu interfejsów API w sieci web [kompilacji pierwszy interfejsu API sieci Web platformy ASP.NET Core MVC i Visual Studio](../tutorials/first-web-api.md).

## <a name="creating-the-controller"></a>Tworzenie kontrolera

Dodaj nowy kontroler do projektu, *ToDoItemsController*. Powinien on dziedziczyć Microsoft.AspNetCore.Mvc.Controller. Dodaj `Route` atrybutu, aby wskazać, że kontroler będzie obsługiwać żądania kierowane do ścieżek rozpoczynających się od `api/todoitems`. `[controller]` Token w trasie jest zastępowany przez nazwę kontrolera (pominięcie `Controller` sufiks) i jest szczególnie przydatne dla tras globalnego. Dowiedz się więcej o [routingu](../fundamentals/routing.md).

Wymaga kontrolera `IToDoRepository` do funkcji; żądania wystąpienia tego typu za pomocą konstruktora kontrolera. W czasie wykonywania, będą udostępniane tego wystąpienia przy użyciu platformy obsługę [iniekcji zależności](../fundamentals/dependency-injection.md).

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

Ten interfejs API obsługuje cztery różne zlecenia HTTP w celu wykonania operacji CRUD (tworzenia, odczytu, aktualizacji, usuwania) w źródle danych. Najprostszym z nich jest operacja odczytu, która odpowiada na żądania HTTP GET.

### <a name="reading-items"></a>Trwa odczyt elementów

Żądania listy elementów wykonuje się za pomocą żądania GET do `List` metody. `[HttpGet]` Atrybutu `List` metoda wskazuje, że ta akcja powinna obsługiwać żądania GET. Trasy dla tej akcji jest trasy określonych w kontrolerze. Nie jest konieczna użyć nazwy akcji w ramach trasy. Wystarczy upewnić się, że każda akcja ma unikatowy i jednoznaczny trasy. Atrybuty routingu mogą być stosowane na poziomach metody do zbudowania określonych tras i kontrolera.

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

`List` Metoda zwraca kod odpowiedzi 200 OK i wszystkich elementów ToDo zserializowanym w formacie JSON.

Można przetestować nowy metodę interfejsu API przy użyciu różnych narzędzi, takich jak [Postman](https://www.getpostman.com/docs/), pokazano poniżej:

![Postman Konsola pokazująca, że żądanie GET todoitems i treści odpowiedzi JSON dla trzech elementów zwróconych przedstawiający](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a>Tworzenie elementów

Konwencja tworzenie nowych elementów danych jest mapowany na zlecenie HTTP POST. `Create` Metoda ma `[HttpPost]` zastosowano atrybut i akceptuje `ToDoItem` wystąpienia. Ponieważ `item` argument zostanie przekazany w treści POST, ten parametr zostanie nadany `[FromBody]` atrybutu.

Wewnątrz metody element jest zaznaczony ważności i wcześniejszych istnienie w magazynie danych, a jeśli wystąpią żadne problemy, jest ona dodawana przy użyciu repozytorium. Sprawdzanie `ModelState.IsValid` wykonuje [modelu weryfikacji](../mvc/models/validation.md)i ma się odbywać w każdej metody interfejsu API, który akceptuje dane wejściowe użytkownika.

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

W przykładzie użyto wyliczenia zawierający kody błędów, które są przekazywane do klientów urządzeń przenośnych:

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

Przetestuj dodawania nowych elementów przy użyciu Postman, wybierając zlecenie POST, podając nowy obiekt w formacie JSON w treści żądania. Należy również dodać nagłówka żądania określając `Content-Type` z `application/json`.

![Konsola postman pokazująca, POST i odpowiedzi](native-mobile-backend/_static/postman-post.png)

Metoda zwraca nowo utworzonego elementu w odpowiedzi.

### <a name="updating-items"></a>Aktualizowanie elementów

Modyfikowanie rekordów jest wykonywane przy użyciu żądania HTTP PUT. Inne niż ta zmiana `Edit` metody jest niemal identyczny `Create`. Należy pamiętać, że jeśli rekord nie zostanie odnaleziony, `Edit` akcji, którą będzie zwracać `NotFound` odpowiedzi (404).

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

Aby przetestować z Postman, zmień zlecenie na PUT. Określ dane zaktualizowanego obiektu w treści żądania.

![Konsola postman pokazująca, PUT i odpowiedzi](native-mobile-backend/_static/postman-put.png)

Ta metoda zwraca `NoContent` odpowiedzi (204) po pomyślnym spójności z istniejącym interfejsu API.

### <a name="deleting-items"></a>Usuwanie elementów

Usuwanie rekordów odbywa się tworzenie żądań DELETE służących do usługi i przekazywanie identyfikator elementu do usunięcia. Zgodnie z aktualizacjami, otrzymają żądań dla elementów, które nie istnieją `NotFound` odpowiedzi. W przeciwnym razie zostanie wyświetlony pomyślnego żądania `NoContent` odpowiedzi (204).

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

Należy pamiętać, że podczas testowania funkcji usuwania, nic nie jest wymagana w treści żądania.

![Konsola postman pokazująca, usuwania i odpowiedzi](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a>Typowych konwersji interfejsu API sieci Web

Podczas tworzenia usługi wewnętrznej bazy danych dla aplikacji można pozwoli uzyskać spójny zestaw konwencje lub zasady dotyczące postępowania z kompleksowymi problemy. Na przykład w usłudze przedstawionych powyżej, żądania dotyczące określonych rekordów, które nie zostały znalezione Odebrano `NotFound` odpowiedzi, a nie `BadRequest` odpowiedzi. Podobnie, polecenia do tej usługi, który przekazany w typach powiązana z modelem zawsze zaznaczone `ModelState.IsValid` i zwracany `BadRequest` dla typów nieprawidłowy model.

Po wyłaniają wspólne zasady dla interfejsów API, możesz można zwykle Hermetyzowanie w [filtru](../mvc/controllers/filters.md). Dowiedz się więcej o [jak hermetyzacji wspólnych zasad interfejsu API w aplikacjach ASP.NET Core MVC](https://msdn.microsoft.com/magazine/mt767699.aspx).
