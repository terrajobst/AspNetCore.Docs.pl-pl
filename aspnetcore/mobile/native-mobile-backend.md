---
title: Tworzenie usługi wewnętrznej bazy danych dla natywnych aplikacji mobilnych z platformy ASP.NET Core
author: ardalis
description: Dowiedz się, jak tworzyć przy użyciu platformy ASP.NET Core MVC do obsługi natywnych aplikacji mobilnych usługi wewnętrznej bazy danych.
ms.author: riande
ms.date: 10/14/2016
uid: mobile/native-mobile-backend
ms.openlocfilehash: 27051cd3c4e2c3aa1ebf6d5510db4645651120e8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276129"
---
# <a name="create-backend-services-for-native-mobile-apps-with-aspnet-core"></a><span data-ttu-id="7a902-103">Tworzenie usługi wewnętrznej bazy danych dla natywnych aplikacji mobilnych z platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7a902-103">Create backend services for native mobile apps with ASP.NET Core</span></span>

<span data-ttu-id="7a902-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="7a902-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="7a902-105">Aplikacje mobilne łatwo mogą komunikować się z usługami zaplecza ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7a902-105">Mobile apps can easily communicate with ASP.NET Core backend services.</span></span>

[<span data-ttu-id="7a902-106">Wyświetlić lub pobrać przykładowy kod usługi wewnętrznej bazy danych</span><span class="sxs-lookup"><span data-stu-id="7a902-106">View or download sample backend services code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a><span data-ttu-id="7a902-107">Przykładowe natywnych aplikacji mobilnej</span><span class="sxs-lookup"><span data-stu-id="7a902-107">The Sample Native Mobile App</span></span>

<span data-ttu-id="7a902-108">Ten samouczek przedstawia sposób tworzenia usługi wewnętrznej bazy danych przy użyciu platformy ASP.NET Core MVC do obsługi natywnych aplikacji mobilnych.</span><span class="sxs-lookup"><span data-stu-id="7a902-108">This tutorial demonstrates how to create backend services using ASP.NET Core MVC to support native mobile apps.</span></span> <span data-ttu-id="7a902-109">Używa [aplikacji platformy Xamarin Forms ToDoRest](/xamarin/xamarin-forms/data-cloud/consuming/rest) jako jego klientami, która obejmuje oddzielne klientach natywnych dla urządzeń systemu Android, iOS, uniwersalnych systemu Windows i Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="7a902-109">It uses the [Xamarin Forms ToDoRest app](/xamarin/xamarin-forms/data-cloud/consuming/rest) as its native client, which includes separate native clients for Android, iOS, Windows Universal, and Window Phone devices.</span></span> <span data-ttu-id="7a902-110">Można wykonaj połączonego samouczek do tworzenia aplikacji natywnej (i zainstalować niezbędne bezpłatnych narzędzi Xamarin), a także Pobierz przykładowe rozwiązanie Xamarin.</span><span class="sxs-lookup"><span data-stu-id="7a902-110">You can follow the linked tutorial to create the native app (and install the necessary free Xamarin tools), as well as download the Xamarin sample solution.</span></span> <span data-ttu-id="7a902-111">Przykładowe Xamarin zawiera projekt usługi ASP.NET Web API 2 aplikacji platformy ASP.NET Core w tym artykule zastępuje (bez zmian wymaganych przez klienta).</span><span class="sxs-lookup"><span data-stu-id="7a902-111">The Xamarin sample includes an ASP.NET Web API 2 services project, which this article's ASP.NET Core app replaces (with no changes required by the client).</span></span>

![Wykonaj pozostałe aplikacji uruchomionych na smartfonie systemu Android](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a><span data-ttu-id="7a902-113">Funkcje</span><span class="sxs-lookup"><span data-stu-id="7a902-113">Features</span></span>

<span data-ttu-id="7a902-114">Aplikacja ToDoRest obsługuje wyświetlanie, dodawanie i aktualizowanie elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="7a902-114">The ToDoRest app supports listing, adding, deleting, and updating To-Do items.</span></span> <span data-ttu-id="7a902-115">Każdy element ma identyfikator, nazwę uwagi i Właściwość wskazująca, czy jego jest zostało to jeszcze zrobione.</span><span class="sxs-lookup"><span data-stu-id="7a902-115">Each item has an ID, a Name, Notes, and a property indicating whether it's been Done yet.</span></span>

<span data-ttu-id="7a902-116">Głównym widok elementów, jak pokazano powyżej, wyświetla każdy element name i wskazuje, jeśli jest przeprowadzana ze znacznikiem wyboru.</span><span class="sxs-lookup"><span data-stu-id="7a902-116">The main view of the items, as shown above, lists each item's name and indicates if it's done with a checkmark.</span></span>

<span data-ttu-id="7a902-117">Naciskając `+` ikona otwiera okno dialogowe Dodawanie elementu:</span><span class="sxs-lookup"><span data-stu-id="7a902-117">Tapping the `+` icon opens an add item dialog:</span></span>

![Dodaj element okna dialogowego](native-mobile-backend/_static/todo-android-new-item.png)

<span data-ttu-id="7a902-119">Naciskając element na ekranie głównym listy otwiera okno dialogowe Edycja, gdzie można zmodyfikować nazwę, uwagi i gotowe ustawienia elementu, lub można usunąć elementu:</span><span class="sxs-lookup"><span data-stu-id="7a902-119">Tapping an item on the main list screen opens up an edit dialog where the item's Name, Notes, and Done settings can be modified, or the item can be deleted:</span></span>

![Element okno dialogowe Edycja](native-mobile-backend/_static/todo-android-edit-item.png)

<span data-ttu-id="7a902-121">Ten przykład jest domyślnie skonfigurowany do korzystania z usług wewnętrznej bazy danych hostowanej w lokalizacji developer.xamarin.com, które pozwalają operacji tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="7a902-121">This sample is configured by default to use backend services hosted at developer.xamarin.com, which allow read-only operations.</span></span> <span data-ttu-id="7a902-122">Do przetestowania go się samodzielnie aplikacji platformy ASP.NET Core utworzone w następnej sekcji, które są uruchomione na komputerze, musisz zaktualizować aplikację `RestUrl` stałej.</span><span class="sxs-lookup"><span data-stu-id="7a902-122">To test it out yourself against the ASP.NET Core app created in the next section running on your computer, you'll need to update the app's `RestUrl` constant.</span></span> <span data-ttu-id="7a902-123">Przejdź do `ToDoREST` projektu i Otwórz *Constants.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="7a902-123">Navigate to the `ToDoREST` project and open the *Constants.cs* file.</span></span> <span data-ttu-id="7a902-124">Zastąp `RestUrl` adres URL, który obejmuje IP tego komputera adresów (nie localhost ani niebędący adresem 127.0.0.1, ponieważ ten adres jest używany z emulatora urządzenia, nie z komputera).</span><span class="sxs-lookup"><span data-stu-id="7a902-124">Replace the `RestUrl` with a URL that includes your machine's IP address (not localhost or 127.0.0.1, since this address is used from the device emulator, not from your machine).</span></span> <span data-ttu-id="7a902-125">Zawierają również numer portu (5000).</span><span class="sxs-lookup"><span data-stu-id="7a902-125">Include the port number as well (5000).</span></span> <span data-ttu-id="7a902-126">Aby przetestować działanie usługi z urządzeniem, upewnij się, że masz aktywne Zapora blokuje dostęp do tego portu.</span><span class="sxs-lookup"><span data-stu-id="7a902-126">In order to test that your services work with a device, ensure you don't have an active firewall blocking access to this port.</span></span>

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a><span data-ttu-id="7a902-127">Tworzenie projektu platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7a902-127">Creating the ASP.NET Core Project</span></span>

<span data-ttu-id="7a902-128">Utwórz nową aplikację sieci Web platformy ASP.NET Core w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7a902-128">Create a new ASP.NET Core Web Application in Visual Studio.</span></span> <span data-ttu-id="7a902-129">Wybierz szablon interfejsu API sieci Web i bez uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7a902-129">Choose the Web API template and No Authentication.</span></span> <span data-ttu-id="7a902-130">Nazwij projekt *ToDoApi*.</span><span class="sxs-lookup"><span data-stu-id="7a902-130">Name the project *ToDoApi*.</span></span>

![Okno dialogowe nowego aplikacji sieci Web platformy ASP.NET z wybrany szablon projektu interfejsu API sieci Web](native-mobile-backend/_static/web-api-template.png)

<span data-ttu-id="7a902-132">Aplikacja powinno odpowiedzieć na wszystkie żądania kierowane do portu 5000.</span><span class="sxs-lookup"><span data-stu-id="7a902-132">The application should respond to all requests made to port 5000.</span></span> <span data-ttu-id="7a902-133">Aktualizacja *Program.cs* uwzględnienie `.UseUrls("http://*:5000")` w tym:</span><span class="sxs-lookup"><span data-stu-id="7a902-133">Update *Program.cs* to include `.UseUrls("http://*:5000")` to achieve this:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> <span data-ttu-id="7a902-134">Upewnij się, że uruchomienie aplikacji bezpośrednio, a nie za usług IIS Express, który ignoruje żądania innego niż lokalne domyślnie.</span><span class="sxs-lookup"><span data-stu-id="7a902-134">Make sure you run the application directly, rather than behind IIS Express, which ignores non-local requests by default.</span></span> <span data-ttu-id="7a902-135">Uruchom [dotnet Uruchom](/dotnet/core/tools/dotnet-run) z wiersza polecenia, lub wybierz profil Nazwa aplikacji z listy rozwijanej docelowego debugowania na pasku narzędzi programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7a902-135">Run [dotnet run](/dotnet/core/tools/dotnet-run) from a command prompt, or choose the application name profile from the Debug Target dropdown in the Visual Studio toolbar.</span></span>

<span data-ttu-id="7a902-136">Dodaj klasę modelu do reprezentowania elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="7a902-136">Add a model class to represent To-Do items.</span></span> <span data-ttu-id="7a902-137">Zaznacz wymagane pola przy użyciu `[Required]` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="7a902-137">Mark required fields using the `[Required]` attribute:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

<span data-ttu-id="7a902-138">Metody interfejsu API wymagają jakiś sposób pracy z danymi.</span><span class="sxs-lookup"><span data-stu-id="7a902-138">The API methods require some way to work with data.</span></span> <span data-ttu-id="7a902-139">Używać tego samego `IToDoRepository` interfejsu oryginalnego używa próbki Xamarin:</span><span class="sxs-lookup"><span data-stu-id="7a902-139">Use the same `IToDoRepository` interface the original Xamarin sample uses:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

<span data-ttu-id="7a902-140">Dla tego przykładu implementacja używa tylko prywatnej kolekcję elementów:</span><span class="sxs-lookup"><span data-stu-id="7a902-140">For this sample, the implementation just uses a private collection of items:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

<span data-ttu-id="7a902-141">Konfigurowanie wdrożenia w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7a902-141">Configure the implementation in *Startup.cs*:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

<span data-ttu-id="7a902-142">W tym momencie możesz przystąpić do tworzenia *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="7a902-142">At this point, you're ready to create the *ToDoItemsController*.</span></span>

> [!TIP]
> <span data-ttu-id="7a902-143">Dowiedz się więcej o tworzeniu interfejsów API w sieci web [kompilacji pierwszy interfejsu API sieci Web platformy ASP.NET Core MVC i Visual Studio](../tutorials/first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="7a902-143">Learn more about creating web APIs in [Build your first Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md).</span></span>

## <a name="creating-the-controller"></a><span data-ttu-id="7a902-144">Tworzenie kontrolera</span><span class="sxs-lookup"><span data-stu-id="7a902-144">Creating the Controller</span></span>

<span data-ttu-id="7a902-145">Dodaj nowy kontroler do projektu, *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="7a902-145">Add a new controller to the project, *ToDoItemsController*.</span></span> <span data-ttu-id="7a902-146">Powinien on dziedziczyć Microsoft.AspNetCore.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="7a902-146">It should inherit from Microsoft.AspNetCore.Mvc.Controller.</span></span> <span data-ttu-id="7a902-147">Dodaj `Route` atrybutu, aby wskazać, że kontroler będzie obsługiwać żądania kierowane do ścieżek rozpoczynających się od `api/todoitems`.</span><span class="sxs-lookup"><span data-stu-id="7a902-147">Add a `Route` attribute to indicate that the controller will handle requests made to paths starting with `api/todoitems`.</span></span> <span data-ttu-id="7a902-148">`[controller]` Token w trasie jest zastępowany przez nazwę kontrolera (pominięcie `Controller` sufiks) i jest szczególnie przydatne dla tras globalnego.</span><span class="sxs-lookup"><span data-stu-id="7a902-148">The `[controller]` token in the route is replaced by the name of the controller (omitting the `Controller` suffix), and is especially helpful for global routes.</span></span> <span data-ttu-id="7a902-149">Dowiedz się więcej o [routingu](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="7a902-149">Learn more about [routing](../fundamentals/routing.md).</span></span>

<span data-ttu-id="7a902-150">Wymaga kontrolera `IToDoRepository` do funkcji; żądania wystąpienia tego typu za pomocą konstruktora kontrolera.</span><span class="sxs-lookup"><span data-stu-id="7a902-150">The controller requires an `IToDoRepository` to function; request an instance of this type through the controller's constructor.</span></span> <span data-ttu-id="7a902-151">W czasie wykonywania, będą udostępniane tego wystąpienia przy użyciu platformy obsługę [iniekcji zależności](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="7a902-151">At runtime, this instance will be provided using the framework's support for [dependency injection](../fundamentals/dependency-injection.md).</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

<span data-ttu-id="7a902-152">Ten interfejs API obsługuje cztery różne zlecenia HTTP w celu wykonania operacji CRUD (tworzenia, odczytu, aktualizacji, usuwania) w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="7a902-152">This API supports four different HTTP verbs to perform CRUD (Create, Read, Update, Delete) operations on the data source.</span></span> <span data-ttu-id="7a902-153">Najprostszym z nich jest operacja odczytu, która odpowiada na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="7a902-153">The simplest of these is the Read operation, which corresponds to an HTTP GET request.</span></span>

### <a name="reading-items"></a><span data-ttu-id="7a902-154">Trwa odczyt elementów</span><span class="sxs-lookup"><span data-stu-id="7a902-154">Reading Items</span></span>

<span data-ttu-id="7a902-155">Żądania listy elementów wykonuje się za pomocą żądania GET do `List` metody.</span><span class="sxs-lookup"><span data-stu-id="7a902-155">Requesting a list of items is done with a GET request to the `List` method.</span></span> <span data-ttu-id="7a902-156">`[HttpGet]` Atrybutu `List` metoda wskazuje, że ta akcja powinna obsługiwać żądania GET.</span><span class="sxs-lookup"><span data-stu-id="7a902-156">The `[HttpGet]` attribute on the `List` method indicates that this action should only handle GET requests.</span></span> <span data-ttu-id="7a902-157">Trasy dla tej akcji jest trasy określonych w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="7a902-157">The route for this action is the route specified on the controller.</span></span> <span data-ttu-id="7a902-158">Nie jest konieczna użyć nazwy akcji w ramach trasy.</span><span class="sxs-lookup"><span data-stu-id="7a902-158">You don't necessarily need to use the action name as part of the route.</span></span> <span data-ttu-id="7a902-159">Wystarczy upewnić się, że każda akcja ma unikatowy i jednoznaczny trasy.</span><span class="sxs-lookup"><span data-stu-id="7a902-159">You just need to ensure each action has a unique and unambiguous route.</span></span> <span data-ttu-id="7a902-160">Atrybuty routingu mogą być stosowane na poziomach metody do zbudowania określonych tras i kontrolera.</span><span class="sxs-lookup"><span data-stu-id="7a902-160">Routing attributes can be applied at both the controller and method levels to build up specific routes.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

<span data-ttu-id="7a902-161">`List` Metoda zwraca kod odpowiedzi 200 OK i wszystkich elementów ToDo zserializowanym w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="7a902-161">The `List` method returns a 200 OK response code and all of the ToDo items, serialized as JSON.</span></span>

<span data-ttu-id="7a902-162">Można przetestować nowy metodę interfejsu API przy użyciu różnych narzędzi, takich jak [Postman](https://www.getpostman.com/docs/), pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="7a902-162">You can test your new API method using a variety of tools, such as [Postman](https://www.getpostman.com/docs/), shown here:</span></span>

![Postman Konsola pokazująca, że żądanie GET todoitems i treści odpowiedzi JSON dla trzech elementów zwróconych przedstawiający](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a><span data-ttu-id="7a902-164">Tworzenie elementów</span><span class="sxs-lookup"><span data-stu-id="7a902-164">Creating Items</span></span>

<span data-ttu-id="7a902-165">Konwencja tworzenie nowych elementów danych jest mapowany na zlecenie HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="7a902-165">By convention, creating new data items is mapped to the HTTP POST verb.</span></span> <span data-ttu-id="7a902-166">`Create` Metoda ma `[HttpPost]` zastosowano atrybut i akceptuje `ToDoItem` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="7a902-166">The `Create` method has an `[HttpPost]` attribute applied to it, and accepts a `ToDoItem` instance.</span></span> <span data-ttu-id="7a902-167">Ponieważ `item` argument zostanie przekazany w treści POST, ten parametr zostanie nadany `[FromBody]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="7a902-167">Since the `item` argument will be passed in the body of the POST, this parameter is decorated with the `[FromBody]` attribute.</span></span>

<span data-ttu-id="7a902-168">Wewnątrz metody element jest zaznaczony ważności i wcześniejszych istnienie w magazynie danych, a jeśli wystąpią żadne problemy, jest ona dodawana przy użyciu repozytorium.</span><span class="sxs-lookup"><span data-stu-id="7a902-168">Inside the method, the item is checked for validity and prior existence in the data store, and if no issues occur, it's added using the repository.</span></span> <span data-ttu-id="7a902-169">Sprawdzanie `ModelState.IsValid` wykonuje [modelu weryfikacji](../mvc/models/validation.md)i ma się odbywać w każdej metody interfejsu API, który akceptuje dane wejściowe użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7a902-169">Checking `ModelState.IsValid` performs [model validation](../mvc/models/validation.md), and should be done in every API method that accepts user input.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

<span data-ttu-id="7a902-170">W przykładzie użyto wyliczenia zawierający kody błędów, które są przekazywane do klientów urządzeń przenośnych:</span><span class="sxs-lookup"><span data-stu-id="7a902-170">The sample uses an enum containing error codes that are passed to the mobile client:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

<span data-ttu-id="7a902-171">Przetestuj dodawania nowych elementów przy użyciu Postman, wybierając zlecenie POST, podając nowy obiekt w formacie JSON w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="7a902-171">Test adding new items using Postman by choosing the POST verb providing the new object in JSON format in the Body of the request.</span></span> <span data-ttu-id="7a902-172">Należy również dodać nagłówka żądania określając `Content-Type` z `application/json`.</span><span class="sxs-lookup"><span data-stu-id="7a902-172">You should also add a request header specifying a `Content-Type` of `application/json`.</span></span>

![Konsola postman pokazująca, POST i odpowiedzi](native-mobile-backend/_static/postman-post.png)

<span data-ttu-id="7a902-174">Metoda zwraca nowo utworzonego elementu w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="7a902-174">The method returns the newly created item in the response.</span></span>

### <a name="updating-items"></a><span data-ttu-id="7a902-175">Aktualizowanie elementów</span><span class="sxs-lookup"><span data-stu-id="7a902-175">Updating Items</span></span>

<span data-ttu-id="7a902-176">Modyfikowanie rekordów jest wykonywane przy użyciu żądania HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="7a902-176">Modifying records is done using HTTP PUT requests.</span></span> <span data-ttu-id="7a902-177">Inne niż ta zmiana `Edit` metody jest niemal identyczny `Create`.</span><span class="sxs-lookup"><span data-stu-id="7a902-177">Other than this change, the `Edit` method is almost identical to `Create`.</span></span> <span data-ttu-id="7a902-178">Należy pamiętać, że jeśli rekord nie zostanie odnaleziony, `Edit` akcji, którą będzie zwracać `NotFound` odpowiedzi (404).</span><span class="sxs-lookup"><span data-stu-id="7a902-178">Note that if the record isn't found, the `Edit` action will return a `NotFound` (404) response.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

<span data-ttu-id="7a902-179">Aby przetestować z Postman, zmień zlecenie na PUT.</span><span class="sxs-lookup"><span data-stu-id="7a902-179">To test with Postman, change the verb to PUT.</span></span> <span data-ttu-id="7a902-180">Określ dane zaktualizowanego obiektu w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="7a902-180">Specify the updated object data in the Body of the request.</span></span>

![Konsola postman pokazująca, PUT i odpowiedzi](native-mobile-backend/_static/postman-put.png)

<span data-ttu-id="7a902-182">Ta metoda zwraca `NoContent` odpowiedzi (204) po pomyślnym spójności z istniejącym interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="7a902-182">This method returns a `NoContent` (204) response when successful, for consistency with the pre-existing API.</span></span>

### <a name="deleting-items"></a><span data-ttu-id="7a902-183">Usuwanie elementów</span><span class="sxs-lookup"><span data-stu-id="7a902-183">Deleting Items</span></span>

<span data-ttu-id="7a902-184">Usuwanie rekordów odbywa się tworzenie żądań DELETE służących do usługi i przekazywanie identyfikator elementu do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="7a902-184">Deleting records is accomplished by making DELETE requests to the service, and passing the ID of the item to be deleted.</span></span> <span data-ttu-id="7a902-185">Zgodnie z aktualizacjami, otrzymają żądań dla elementów, które nie istnieją `NotFound` odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="7a902-185">As with updates, requests for items that don't exist will receive `NotFound` responses.</span></span> <span data-ttu-id="7a902-186">W przeciwnym razie zostanie wyświetlony pomyślnego żądania `NoContent` odpowiedzi (204).</span><span class="sxs-lookup"><span data-stu-id="7a902-186">Otherwise, a successful request will get a `NoContent` (204) response.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

<span data-ttu-id="7a902-187">Należy pamiętać, że podczas testowania funkcji usuwania, nic nie jest wymagana w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="7a902-187">Note that when testing the delete functionality, nothing is required in the Body of the request.</span></span>

![Konsola postman pokazująca, usuwania i odpowiedzi](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a><span data-ttu-id="7a902-189">Typowych konwersji interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="7a902-189">Common Web API Conventions</span></span>

<span data-ttu-id="7a902-190">Podczas tworzenia usługi wewnętrznej bazy danych dla aplikacji można pozwoli uzyskać spójny zestaw konwencje lub zasady dotyczące postępowania z kompleksowymi problemy.</span><span class="sxs-lookup"><span data-stu-id="7a902-190">As you develop the backend services for your app, you will want to come up with a consistent set of conventions or policies for handling cross-cutting concerns.</span></span> <span data-ttu-id="7a902-191">Na przykład w usłudze przedstawionych powyżej, żądania dotyczące określonych rekordów, które nie zostały znalezione Odebrano `NotFound` odpowiedzi, a nie `BadRequest` odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="7a902-191">For example, in the service shown above, requests for specific records that weren't found received a `NotFound` response, rather than a `BadRequest` response.</span></span> <span data-ttu-id="7a902-192">Podobnie, polecenia do tej usługi, który przekazany w typach powiązana z modelem zawsze zaznaczone `ModelState.IsValid` i zwracany `BadRequest` dla typów nieprawidłowy model.</span><span class="sxs-lookup"><span data-stu-id="7a902-192">Similarly, commands made to this service that passed in model bound types always checked `ModelState.IsValid` and returned a `BadRequest` for invalid model types.</span></span>

<span data-ttu-id="7a902-193">Po wyłaniają wspólne zasady dla interfejsów API, możesz można zwykle Hermetyzowanie w [filtru](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="7a902-193">Once you've identified a common policy for your APIs, you can usually encapsulate it in a [filter](../mvc/controllers/filters.md).</span></span> <span data-ttu-id="7a902-194">Dowiedz się więcej o [jak hermetyzacji wspólnych zasad interfejsu API w aplikacjach ASP.NET Core MVC](https://msdn.microsoft.com/magazine/mt767699.aspx).</span><span class="sxs-lookup"><span data-stu-id="7a902-194">Learn more about [how to encapsulate common API policies in ASP.NET Core MVC applications](https://msdn.microsoft.com/magazine/mt767699.aspx).</span></span>
