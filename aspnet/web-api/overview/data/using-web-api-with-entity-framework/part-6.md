---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Tworzenie klienta JavaScript | Dokumentacja firmy Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: b397c5a413ae213c9b79da1c0e0626efe21c7e21
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="create-the-javascript-client"></a><span data-ttu-id="877ca-102">Tworzenie klienta JavaScript</span><span class="sxs-lookup"><span data-stu-id="877ca-102">Create the JavaScript Client</span></span>
====================
<span data-ttu-id="877ca-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="877ca-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="877ca-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="877ca-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="877ca-105">W tej sekcji utworzysz klienta dla aplikacji, przy użyciu języka HTML, JavaScript oraz [Knockout.js](http://knockoutjs.com/) biblioteki.</span><span class="sxs-lookup"><span data-stu-id="877ca-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="877ca-106">Firma Microsoft będzie kompilacji aplikacji klienckiej w etapach:</span><span class="sxs-lookup"><span data-stu-id="877ca-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="877ca-107">Wyświetlanie listy książek.</span><span class="sxs-lookup"><span data-stu-id="877ca-107">Showing a list of books.</span></span>
- <span data-ttu-id="877ca-108">Wyświetlanie szczegółów książki.</span><span class="sxs-lookup"><span data-stu-id="877ca-108">Showing a book detail.</span></span>
- <span data-ttu-id="877ca-109">Dodawanie nowej książki.</span><span class="sxs-lookup"><span data-stu-id="877ca-109">Adding a new book.</span></span>

<span data-ttu-id="877ca-110">Biblioteki Knockout korzysta ze wzorca Model-View-ViewModel (MVVM):</span><span class="sxs-lookup"><span data-stu-id="877ca-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="877ca-111">**Modelu** jest po stronie serwera reprezentację danych w domenie business (w naszym case, książki i autorów).</span><span class="sxs-lookup"><span data-stu-id="877ca-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="877ca-112">**Widoku** to warstwa prezentacji (HTML).</span><span class="sxs-lookup"><span data-stu-id="877ca-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="877ca-113">**Model widoku** jest obiektu JavaScript, która przechowuje modeli.</span><span class="sxs-lookup"><span data-stu-id="877ca-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="877ca-114">Model widoku jest abstrakcji kodu interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="877ca-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="877ca-115">Go nie ma informacji o reprezentacji w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="877ca-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="877ca-116">Zamiast tego reprezentuje funkcje abstrakcyjne widoku, takie jak &quot;listę książek&quot;.</span><span class="sxs-lookup"><span data-stu-id="877ca-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="877ca-117">Widok jest powiązany z danymi model widoku.</span><span class="sxs-lookup"><span data-stu-id="877ca-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="877ca-118">Aktualizacje na model widoku są automatycznie odzwierciedlane w widoku.</span><span class="sxs-lookup"><span data-stu-id="877ca-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="877ca-119">Model widoku również pobiera zdarzenia w widoku, takich jak kliknięcie przycisku.</span><span class="sxs-lookup"><span data-stu-id="877ca-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="877ca-120">Tej metody można łatwo zmienić układ i interfejsu użytkownika aplikacji, ponieważ powiązania, można zmienić bez ponownego tworzenia kodu.</span><span class="sxs-lookup"><span data-stu-id="877ca-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="877ca-121">Na przykład może wyświetlić listę elementów jako `<ul>`, następnie zmienić go później do tabeli.</span><span class="sxs-lookup"><span data-stu-id="877ca-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="877ca-122">Dodawanie biblioteki Knockout</span><span class="sxs-lookup"><span data-stu-id="877ca-122">Add the Knockout Library</span></span>

<span data-ttu-id="877ca-123">W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**.</span><span class="sxs-lookup"><span data-stu-id="877ca-123">In Visual Studio, from the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="877ca-124">Następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="877ca-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="877ca-125">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="877ca-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="877ca-126">To polecenie dodaje pliki odcinania w folderze skryptów.</span><span class="sxs-lookup"><span data-stu-id="877ca-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="877ca-127">Tworzenie modelu widoku</span><span class="sxs-lookup"><span data-stu-id="877ca-127">Create the View Model</span></span>

<span data-ttu-id="877ca-128">Dodaj plik JavaScript o nazwie app.js w folderze skryptów.</span><span class="sxs-lookup"><span data-stu-id="877ca-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="877ca-129">(W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder skryptów, wybierz **Dodaj**, a następnie wybierz pozycję **plik JavaScript**.) Wklej następujący kod:</span><span class="sxs-lookup"><span data-stu-id="877ca-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="877ca-130">W Knockout `observable` klasa umożliwia powiązanie danych.</span><span class="sxs-lookup"><span data-stu-id="877ca-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="877ca-131">Zmienić zawartość zauważalny, według powiadamia wszystkie formanty powiązane z danymi, więc one aktualizowane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="877ca-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="877ca-132">( `observableArray` Klasy jest wersją tablicy *według*.) Do uruchomienia z naszych model widoku zawiera dwa dostrzegalne elementy:</span><span class="sxs-lookup"><span data-stu-id="877ca-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="877ca-133">`books`zawiera listę książek.</span><span class="sxs-lookup"><span data-stu-id="877ca-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="877ca-134">`error`zawiera komunikat o błędzie, jeśli wywołanie AJAX nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="877ca-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="877ca-135">`getAllBooks` Metody sprawia, że wywołanie AJAX do pobrania listy książek.</span><span class="sxs-lookup"><span data-stu-id="877ca-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="877ca-136">Następnie wypchnięcia jej wyników na `books` tablicy.</span><span class="sxs-lookup"><span data-stu-id="877ca-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="877ca-137">`ko.applyBindings` Metoda jest częścią biblioteki Knockout.</span><span class="sxs-lookup"><span data-stu-id="877ca-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="877ca-138">Pobiera model widoku jako parametru, a konfiguruje wiązania danych.</span><span class="sxs-lookup"><span data-stu-id="877ca-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="877ca-139">Dodaj pakiet skryptu</span><span class="sxs-lookup"><span data-stu-id="877ca-139">Add a Script Bundle</span></span>

<span data-ttu-id="877ca-140">Tworzenie pakietów jest funkcją w programie ASP.NET 4.5, który ułatwia łączenie lub wielu plików pakietu w jednym pliku.</span><span class="sxs-lookup"><span data-stu-id="877ca-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="877ca-141">Tworzenie pakietów zmniejsza liczbę żądań do serwera, który można zwiększyć czas ładowania strony.</span><span class="sxs-lookup"><span data-stu-id="877ca-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="877ca-142">Otwórz plik aplikacji\_Start/BundleConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="877ca-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="877ca-143">Dodaj następujący kod do metody RegisterBundles.</span><span class="sxs-lookup"><span data-stu-id="877ca-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

>[!div class="step-by-step"]
<span data-ttu-id="877ca-144">[Poprzednie](part-5.md)
[dalej](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="877ca-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
