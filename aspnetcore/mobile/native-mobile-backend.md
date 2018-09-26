---
title: Tworzenie usług zaplecza dla natywnych aplikacji mobilnych za pomocą programu ASP.NET Core
author: ardalis
description: Dowiedz się, jak tworzenie usług zaplecza za pomocą platformy ASP.NET Core MVC do obsługi natywnych aplikacji mobilnych.
ms.author: riande
ms.date: 10/14/2016
uid: mobile/native-mobile-backend
ms.openlocfilehash: 3ebd30ad1ffbd66b256e7f3954a07d682f76a754
ms.sourcegitcommit: 517bb1366da2a28b0014e384fa379755c21b47d8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47230181"
---
# <a name="create-backend-services-for-native-mobile-apps-with-aspnet-core"></a><span data-ttu-id="c99db-103">Tworzenie usług zaplecza dla natywnych aplikacji mobilnych za pomocą programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c99db-103">Create backend services for native mobile apps with ASP.NET Core</span></span>

<span data-ttu-id="c99db-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="c99db-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="c99db-105">Aplikacje mobilne może łatwo komunikować się z usług zaplecza programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c99db-105">Mobile apps can easily communicate with ASP.NET Core backend services.</span></span>

[<span data-ttu-id="c99db-106">Wyświetlanie lub pobieranie przykładowego kodu usługi wewnętrznej bazy danych</span><span class="sxs-lookup"><span data-stu-id="c99db-106">View or download sample backend services code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a><span data-ttu-id="c99db-107">Przykładowe natywnej aplikacji mobilnej</span><span class="sxs-lookup"><span data-stu-id="c99db-107">The Sample Native Mobile App</span></span>

<span data-ttu-id="c99db-108">W tym samouczku pokazano, jak tworzenie usług zaplecza za pomocą platformy ASP.NET Core MVC do obsługi natywnych aplikacji mobilnych.</span><span class="sxs-lookup"><span data-stu-id="c99db-108">This tutorial demonstrates how to create backend services using ASP.NET Core MVC to support native mobile apps.</span></span> <span data-ttu-id="c99db-109">Używa ona [aplikacji Xamarin Forms ToDoRest](/xamarin/xamarin-forms/data-cloud/consuming/rest) jako jego klient natywny, która obejmuje oddzielne klientów natywnych dla systemów Android, iOS, Windows Universal i Windows Phone urządzeń.</span><span class="sxs-lookup"><span data-stu-id="c99db-109">It uses the [Xamarin Forms ToDoRest app](/xamarin/xamarin-forms/data-cloud/consuming/rest) as its native client, which includes separate native clients for Android, iOS, Windows Universal, and Window Phone devices.</span></span> <span data-ttu-id="c99db-110">Można wykonać kroki samouczka połączone do tworzenia aplikacji natywnej (i zainstaluj wymagane bezpłatne narzędzia Xamarin), a także Pobierz przykładowe rozwiązanie platformy Xamarin.</span><span class="sxs-lookup"><span data-stu-id="c99db-110">You can follow the linked tutorial to create the native app (and install the necessary free Xamarin tools), as well as download the Xamarin sample solution.</span></span> <span data-ttu-id="c99db-111">Przykład Xamarin obejmuje projekt usług ASP.NET Web API 2 aplikacji ASP.NET Core w tym artykule są zastępowane (bez zmian, wymagane przez klienta).</span><span class="sxs-lookup"><span data-stu-id="c99db-111">The Xamarin sample includes an ASP.NET Web API 2 services project, which this article's ASP.NET Core app replaces (with no changes required by the client).</span></span>

![Wykonaj pozostałe aplikacji uruchomionych na smartfonie dla systemu Android](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a><span data-ttu-id="c99db-113">Funkcje</span><span class="sxs-lookup"><span data-stu-id="c99db-113">Features</span></span>

<span data-ttu-id="c99db-114">Aplikacja ToDoRest obsługuje wyświetlanie, dodawanie i aktualizowania elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="c99db-114">The ToDoRest app supports listing, adding, deleting, and updating To-Do items.</span></span> <span data-ttu-id="c99db-115">Każdy element ma identyfikator, nazwę, uwagi i właściwości wskazującą, czy go jest zostało to jeszcze zrobione.</span><span class="sxs-lookup"><span data-stu-id="c99db-115">Each item has an ID, a Name, Notes, and a property indicating whether it's been Done yet.</span></span>

<span data-ttu-id="c99db-116">Widok główny elementów, jak pokazano powyżej, wyświetla nazwę każdego elementu i wskazuje, jeśli jest wykonywane ze znacznikiem wyboru.</span><span class="sxs-lookup"><span data-stu-id="c99db-116">The main view of the items, as shown above, lists each item's name and indicates if it's done with a checkmark.</span></span>

<span data-ttu-id="c99db-117">Naciskając `+` ikona otwiera okno dialogowe Dodawanie elementu:</span><span class="sxs-lookup"><span data-stu-id="c99db-117">Tapping the `+` icon opens an add item dialog:</span></span>

![Okno dialogowe elementu Dodawanie](native-mobile-backend/_static/todo-android-new-item.png)

<span data-ttu-id="c99db-119">Naciśnięcie elementu na ekranie głównym listy zostanie otwarte okno dialogowe Edycja, gdzie można zmodyfikować nazwę, uwagi i gotowe ustawienia elementu, lub można usunąć elementu:</span><span class="sxs-lookup"><span data-stu-id="c99db-119">Tapping an item on the main list screen opens up an edit dialog where the item's Name, Notes, and Done settings can be modified, or the item can be deleted:</span></span>

![Edytuj okno dialogowe elementu](native-mobile-backend/_static/todo-android-edit-item.png)

<span data-ttu-id="c99db-121">W tym przykładzie jest domyślnie skonfigurowany do użycia usług zaplecza hostowanych w developer.xamarin.com, dzięki czemu operacji tylko do odczytu.</span><span class="sxs-lookup"><span data-stu-id="c99db-121">This sample is configured by default to use backend services hosted at developer.xamarin.com, which allow read-only operations.</span></span> <span data-ttu-id="c99db-122">Aby przetestować go samodzielnie względem aplikacji ASP.NET Core utworzona w następnej sekcji, które są uruchomione na komputerze, musisz zaktualizować aplikację `RestUrl` stałej.</span><span class="sxs-lookup"><span data-stu-id="c99db-122">To test it out yourself against the ASP.NET Core app created in the next section running on your computer, you'll need to update the app's `RestUrl` constant.</span></span> <span data-ttu-id="c99db-123">Przejdź do `ToDoREST` projektu, a następnie otwórz *Constants.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="c99db-123">Navigate to the `ToDoREST` project and open the *Constants.cs* file.</span></span> <span data-ttu-id="c99db-124">Zastąp `RestUrl` adres URL, który zawiera IP Twojego komputera adresów (nie localhost ani niebędący adresem 127.0.0.1, ponieważ ten adres jest używany z emulatora urządzeń, nie z Twojego komputera).</span><span class="sxs-lookup"><span data-stu-id="c99db-124">Replace the `RestUrl` with a URL that includes your machine's IP address (not localhost or 127.0.0.1, since this address is used from the device emulator, not from your machine).</span></span> <span data-ttu-id="c99db-125">Zawierają również numer portu (5000).</span><span class="sxs-lookup"><span data-stu-id="c99db-125">Include the port number as well (5000).</span></span> <span data-ttu-id="c99db-126">Aby przetestować działanie usługi z urządzeniem, upewnij się, że nie masz aktywnych Zapora blokuje dostęp do tego portu.</span><span class="sxs-lookup"><span data-stu-id="c99db-126">In order to test that your services work with a device, ensure you don't have an active firewall blocking access to this port.</span></span>

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a><span data-ttu-id="c99db-127">Tworzenie projektu platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c99db-127">Creating the ASP.NET Core Project</span></span>

<span data-ttu-id="c99db-128">Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c99db-128">Create a new ASP.NET Core Web Application in Visual Studio.</span></span> <span data-ttu-id="c99db-129">Wybierz szablon interfejsu API sieci Web i bez uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="c99db-129">Choose the Web API template and No Authentication.</span></span> <span data-ttu-id="c99db-130">Nadaj projektowi nazwę *ToDoApi*.</span><span class="sxs-lookup"><span data-stu-id="c99db-130">Name the project *ToDoApi*.</span></span>

![Okno dialogowe Nowy aplikacji sieci Web ASP.NET z wybraniu szablonu projektu interfejsu API sieci Web](native-mobile-backend/_static/web-api-template.png)

<span data-ttu-id="c99db-132">Aplikacja powinien odpowiedzieć na wszystkie żądania na porcie 5000.</span><span class="sxs-lookup"><span data-stu-id="c99db-132">The application should respond to all requests made to port 5000.</span></span> <span data-ttu-id="c99db-133">Aktualizacja *Program.cs* obejmujący `.UseUrls("http://*:5000")` do osiągnięcia tego:</span><span class="sxs-lookup"><span data-stu-id="c99db-133">Update *Program.cs* to include `.UseUrls("http://*:5000")` to achieve this:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> <span data-ttu-id="c99db-134">Sprawdź, czy uruchomić aplikację bezpośrednio, a nie za usług IIS Express, który ignoruje żądania-local domyślnie.</span><span class="sxs-lookup"><span data-stu-id="c99db-134">Make sure you run the application directly, rather than behind IIS Express, which ignores non-local requests by default.</span></span> <span data-ttu-id="c99db-135">Uruchom [dotnet, uruchom](/dotnet/core/tools/dotnet-run) z wiersza polecenia, lub wybierz nazwę profilu aplikacji z listy rozwijanej Debuguj element docelowy, na pasku narzędzi programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c99db-135">Run [dotnet run](/dotnet/core/tools/dotnet-run) from a command prompt, or choose the application name profile from the Debug Target dropdown in the Visual Studio toolbar.</span></span>

<span data-ttu-id="c99db-136">Dodaj klasę modelu do reprezentowania elementów do wykonania.</span><span class="sxs-lookup"><span data-stu-id="c99db-136">Add a model class to represent To-Do items.</span></span> <span data-ttu-id="c99db-137">Oznacz wymagane pola za pomocą `[Required]` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="c99db-137">Mark required fields using the `[Required]` attribute:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

<span data-ttu-id="c99db-138">Metody interfejsu API wymagają jakiś sposób, aby pracować z danymi.</span><span class="sxs-lookup"><span data-stu-id="c99db-138">The API methods require some way to work with data.</span></span> <span data-ttu-id="c99db-139">Użyto tych samych `IToDoRepository` interfejsu oryginalnego używa platformy Xamarin w przykładowej:</span><span class="sxs-lookup"><span data-stu-id="c99db-139">Use the same `IToDoRepository` interface the original Xamarin sample uses:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

<span data-ttu-id="c99db-140">W tym przykładzie wdrożenia po prostu używa prywatnego kolekcję elementów:</span><span class="sxs-lookup"><span data-stu-id="c99db-140">For this sample, the implementation just uses a private collection of items:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

<span data-ttu-id="c99db-141">Konfigurowanie wdrożenia w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c99db-141">Configure the implementation in *Startup.cs*:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

<span data-ttu-id="c99db-142">W tym momencie możesz przystąpić do tworzenia *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="c99db-142">At this point, you're ready to create the *ToDoItemsController*.</span></span>

> [!TIP]
> <span data-ttu-id="c99db-143">Dowiedz się więcej na temat tworzenia internetowych interfejsów API w [Tworzenie pierwszego interfejsu API sieci Web za pomocą platformy ASP.NET Core MVC i programu Visual Studio](../tutorials/first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c99db-143">Learn more about creating web APIs in [Build your first Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md).</span></span>

## <a name="creating-the-controller"></a><span data-ttu-id="c99db-144">Tworzenie kontrolera</span><span class="sxs-lookup"><span data-stu-id="c99db-144">Creating the Controller</span></span>

<span data-ttu-id="c99db-145">Dodaj nowy kontroler do projektu, *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="c99db-145">Add a new controller to the project, *ToDoItemsController*.</span></span> <span data-ttu-id="c99db-146">Powinien on dziedziczyć Microsoft.AspNetCore.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="c99db-146">It should inherit from Microsoft.AspNetCore.Mvc.Controller.</span></span> <span data-ttu-id="c99db-147">Dodaj `Route` atrybutu, aby wskazać, że kontroler będzie obsługiwać żądania kierowane do ścieżki, począwszy od `api/todoitems`.</span><span class="sxs-lookup"><span data-stu-id="c99db-147">Add a `Route` attribute to indicate that the controller will handle requests made to paths starting with `api/todoitems`.</span></span> <span data-ttu-id="c99db-148">`[controller]` Token dla trasy jest zastępowany przez nazwę kontrolera (z pominięciem `Controller` sufiks) i jest szczególnie przydatne w przypadku globalnych trasy.</span><span class="sxs-lookup"><span data-stu-id="c99db-148">The `[controller]` token in the route is replaced by the name of the controller (omitting the `Controller` suffix), and is especially helpful for global routes.</span></span> <span data-ttu-id="c99db-149">Dowiedz się więcej o [routingu](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="c99db-149">Learn more about [routing](../fundamentals/routing.md).</span></span>

<span data-ttu-id="c99db-150">Wymaga kontrolera `IToDoRepository` do funkcji; żądania wystąpienia tego typu za pośrednictwem konstruktora kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c99db-150">The controller requires an `IToDoRepository` to function; request an instance of this type through the controller's constructor.</span></span> <span data-ttu-id="c99db-151">W czasie wykonywania, to wystąpienie uwarunkowane przestrzeganiem dzięki obsłudze struktury [wstrzykiwanie zależności](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="c99db-151">At runtime, this instance will be provided using the framework's support for [dependency injection](../fundamentals/dependency-injection.md).</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

<span data-ttu-id="c99db-152">Ten interfejs API obsługuje czterech różnych czasowników HTTP do wykonywania operacji CRUD (tworzenia, odczytu, Update, Delete) w źródle danych.</span><span class="sxs-lookup"><span data-stu-id="c99db-152">This API supports four different HTTP verbs to perform CRUD (Create, Read, Update, Delete) operations on the data source.</span></span> <span data-ttu-id="c99db-153">Najprostsza to jest operacja odczytu, która odpowiada na żądania HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="c99db-153">The simplest of these is the Read operation, which corresponds to an HTTP GET request.</span></span>

### <a name="reading-items"></a><span data-ttu-id="c99db-154">Trwa odczyt elementów</span><span class="sxs-lookup"><span data-stu-id="c99db-154">Reading Items</span></span>

<span data-ttu-id="c99db-155">Żądanie listy elementów odbywa się za pomocą żądanie GET w celu `List` metody.</span><span class="sxs-lookup"><span data-stu-id="c99db-155">Requesting a list of items is done with a GET request to the `List` method.</span></span> <span data-ttu-id="c99db-156">`[HttpGet]` Atrybutu na `List` metoda wskazuje, czy ta akcja tylko powinna obsługiwać żądania GET.</span><span class="sxs-lookup"><span data-stu-id="c99db-156">The `[HttpGet]` attribute on the `List` method indicates that this action should only handle GET requests.</span></span> <span data-ttu-id="c99db-157">Trasy dla tej akcji jest trasy określonych w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="c99db-157">The route for this action is the route specified on the controller.</span></span> <span data-ttu-id="c99db-158">Nie jest konieczna użyć nazwy akcji w ramach trasy.</span><span class="sxs-lookup"><span data-stu-id="c99db-158">You don't necessarily need to use the action name as part of the route.</span></span> <span data-ttu-id="c99db-159">Wystarczy upewnić się, że każde działanie ma unikatowy i jednoznacznie trasy.</span><span class="sxs-lookup"><span data-stu-id="c99db-159">You just need to ensure each action has a unique and unambiguous route.</span></span> <span data-ttu-id="c99db-160">Atrybuty routingu mogą być stosowane na poziomach metody do zbudowania określonej trasy i kontrolera.</span><span class="sxs-lookup"><span data-stu-id="c99db-160">Routing attributes can be applied at both the controller and method levels to build up specific routes.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

<span data-ttu-id="c99db-161">`List` Metoda zwraca kod odpowiedzi 200 OK i wszystkie elementy zadań do wykonania, zserializowanym w formacie JSON.</span><span class="sxs-lookup"><span data-stu-id="c99db-161">The `List` method returns a 200 OK response code and all of the ToDo items, serialized as JSON.</span></span>

<span data-ttu-id="c99db-162">Można przetestować nową metodę interfejsu API przy użyciu różnych narzędzi, takich jak [Postman](https://www.getpostman.com/docs/), pokazano poniżej:</span><span class="sxs-lookup"><span data-stu-id="c99db-162">You can test your new API method using a variety of tools, such as [Postman](https://www.getpostman.com/docs/), shown here:</span></span>

![Konsola narzędzia postman, pokazująca, że żądanie GET todoitems i treści odpowiedzi, wyświetlanie kod JSON dla trzech elementów zwróconych](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a><span data-ttu-id="c99db-164">Tworzenie elementów</span><span class="sxs-lookup"><span data-stu-id="c99db-164">Creating Items</span></span>

<span data-ttu-id="c99db-165">Zgodnie z Konwencją tworzenie nowych elementów danych jest zamapowana na zlecenie HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="c99db-165">By convention, creating new data items is mapped to the HTTP POST verb.</span></span> <span data-ttu-id="c99db-166">`Create` Metoda ma `[HttpPost]` zastosowano atrybut i akceptuje `ToDoItem` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="c99db-166">The `Create` method has an `[HttpPost]` attribute applied to it, and accepts a `ToDoItem` instance.</span></span> <span data-ttu-id="c99db-167">Ponieważ `item` argumentów, które zostaną przekazane w treści wpisu, ten parametr zostanie nadany `[FromBody]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="c99db-167">Since the `item` argument will be passed in the body of the POST, this parameter is decorated with the `[FromBody]` attribute.</span></span>

<span data-ttu-id="c99db-168">Wewnątrz metody element jest zaznaczony dla ważności i wcześniejsze istnienie w magazynie danych, a jeśli wystąpią żadne problemy, jest ona dodawana przy użyciu repozytorium.</span><span class="sxs-lookup"><span data-stu-id="c99db-168">Inside the method, the item is checked for validity and prior existence in the data store, and if no issues occur, it's added using the repository.</span></span> <span data-ttu-id="c99db-169">Sprawdzanie `ModelState.IsValid` wykonuje [Walidacja modelu](../mvc/models/validation.md)i musi być wykonywane w każdej metody interfejsu API, który akceptuje dane wejściowe użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c99db-169">Checking `ModelState.IsValid` performs [model validation](../mvc/models/validation.md), and should be done in every API method that accepts user input.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

<span data-ttu-id="c99db-170">W przykładzie użyto wyliczenia zawierający kody błędów, które są przekazywane do klientów urządzeń przenośnych:</span><span class="sxs-lookup"><span data-stu-id="c99db-170">The sample uses an enum containing error codes that are passed to the mobile client:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

<span data-ttu-id="c99db-171">Przetestuj dodawania nowych elementów przy użyciu narzędzia Postman, wybierając zlecenie WPIS, podając nowy obiekt w formacie JSON w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="c99db-171">Test adding new items using Postman by choosing the POST verb providing the new object in JSON format in the Body of the request.</span></span> <span data-ttu-id="c99db-172">Należy również dodać nagłówek żądania określając `Content-Type` z `application/json`.</span><span class="sxs-lookup"><span data-stu-id="c99db-172">You should also add a request header specifying a `Content-Type` of `application/json`.</span></span>

![Konsola postman pokazująca, że WPIS i odpowiedzi](native-mobile-backend/_static/postman-post.png)

<span data-ttu-id="c99db-174">Metoda zwraca nowo utworzonego elementu w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="c99db-174">The method returns the newly created item in the response.</span></span>

### <a name="updating-items"></a><span data-ttu-id="c99db-175">Aktualizowanie elementów</span><span class="sxs-lookup"><span data-stu-id="c99db-175">Updating Items</span></span>

<span data-ttu-id="c99db-176">Modyfikowanie rekordów jest wykonywane za pomocą żądania HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="c99db-176">Modifying records is done using HTTP PUT requests.</span></span> <span data-ttu-id="c99db-177">Inne niż ta zmiana `Edit` metody jest niemal identyczny `Create`.</span><span class="sxs-lookup"><span data-stu-id="c99db-177">Other than this change, the `Edit` method is almost identical to `Create`.</span></span> <span data-ttu-id="c99db-178">Należy pamiętać, że jeśli nie odnaleziono rekordu, `Edit` zwróci akcji `NotFound` odpowiedzi (404).</span><span class="sxs-lookup"><span data-stu-id="c99db-178">Note that if the record isn't found, the `Edit` action will return a `NotFound` (404) response.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

<span data-ttu-id="c99db-179">Aby przetestować za pomocą narzędzia Postman, zmień zlecenie na PUT.</span><span class="sxs-lookup"><span data-stu-id="c99db-179">To test with Postman, change the verb to PUT.</span></span> <span data-ttu-id="c99db-180">Określ dane zaktualizowanego obiektu w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="c99db-180">Specify the updated object data in the Body of the request.</span></span>

![Konsola postman przedstawiające PUT i odpowiedzi](native-mobile-backend/_static/postman-put.png)

<span data-ttu-id="c99db-182">Ta metoda zwraca `NoContent` odpowiedzi (204), jeśli operacja się powiedzie, w celu zachowania spójności z istniejącym interfejsem API.</span><span class="sxs-lookup"><span data-stu-id="c99db-182">This method returns a `NoContent` (204) response when successful, for consistency with the pre-existing API.</span></span>

### <a name="deleting-items"></a><span data-ttu-id="c99db-183">Usuwanie elementów</span><span class="sxs-lookup"><span data-stu-id="c99db-183">Deleting Items</span></span>

<span data-ttu-id="c99db-184">Usuwanie rekordów odbywa się przez wprowadzania żądań DELETE służących do usługi i przekazanie identyfikator elementu do usunięcia.</span><span class="sxs-lookup"><span data-stu-id="c99db-184">Deleting records is accomplished by making DELETE requests to the service, and passing the ID of the item to be deleted.</span></span> <span data-ttu-id="c99db-185">Zgodnie z aktualizacjami, otrzyma żądań dla elementów, które nie istnieją `NotFound` odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="c99db-185">As with updates, requests for items that don't exist will receive `NotFound` responses.</span></span> <span data-ttu-id="c99db-186">W przeciwnym razie otrzyma żądania zakończonego powodzeniem `NoContent` odpowiedzi (204).</span><span class="sxs-lookup"><span data-stu-id="c99db-186">Otherwise, a successful request will get a `NoContent` (204) response.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

<span data-ttu-id="c99db-187">Należy pamiętać, że po testowania funkcji usuwania, nic nie musi w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="c99db-187">Note that when testing the delete functionality, nothing is required in the Body of the request.</span></span>

![Konsola postman pokazująca, że DELETE i odpowiedzi](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a><span data-ttu-id="c99db-189">Typowych Konwencji interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="c99db-189">Common Web API Conventions</span></span>

<span data-ttu-id="c99db-190">Podczas opracowywania usług zaplecza dla aplikacji, należy stworzyć zestaw spójne konwencje lub zasady dotyczące postępowania odciąż przekrojowe zagadnienia.</span><span class="sxs-lookup"><span data-stu-id="c99db-190">As you develop the backend services for your app, you will want to come up with a consistent set of conventions or policies for handling cross-cutting concerns.</span></span> <span data-ttu-id="c99db-191">Na przykład w usłudze powyżej żądań dla określonych rekordów, które nie zostały odnalezione otrzymywanych `NotFound` odpowiedzi, a nie `BadRequest` odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="c99db-191">For example, in the service shown above, requests for specific records that weren't found received a `NotFound` response, rather than a `BadRequest` response.</span></span> <span data-ttu-id="c99db-192">Podobnie, polecenia wprowadzone do tej usługi, które są przekazywane w typach powiązana z modelem zawsze zaznaczone `ModelState.IsValid` i zwrócony `BadRequest` dla typów nieprawidłowy model.</span><span class="sxs-lookup"><span data-stu-id="c99db-192">Similarly, commands made to this service that passed in model bound types always checked `ModelState.IsValid` and returned a `BadRequest` for invalid model types.</span></span>

<span data-ttu-id="c99db-193">Po zidentyfikowaniu wspólne zasady dla interfejsów API, można zwykle hermetyzację go w [filtru](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="c99db-193">Once you've identified a common policy for your APIs, you can usually encapsulate it in a [filter](../mvc/controllers/filters.md).</span></span> <span data-ttu-id="c99db-194">Dowiedz się więcej o [jak hermetyzacji wspólnych zasad interfejsu API w aplikacji ASP.NET Core MVC](https://msdn.microsoft.com/magazine/mt767699.aspx).</span><span class="sxs-lookup"><span data-stu-id="c99db-194">Learn more about [how to encapsulate common API policies in ASP.NET Core MVC applications](https://msdn.microsoft.com/magazine/mt767699.aspx).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c99db-195">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="c99db-195">Additional resources</span></span>

* [<span data-ttu-id="c99db-196">Uwierzytelnianie i autoryzacja</span><span class="sxs-lookup"><span data-stu-id="c99db-196">Authentication and Authorization</span></span>](/xamarin/xamarin-forms/enterprise-application-patterns/authentication-and-authorization)
