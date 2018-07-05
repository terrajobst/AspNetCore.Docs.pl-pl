---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Tworzenie klienta JavaScript | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: b0b8ef9bd44bbce5102f2b12717e330f72a9e0c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400937"
---
<a name="create-the-javascript-client"></a><span data-ttu-id="99baf-102">Tworzenie klienta JavaScript</span><span class="sxs-lookup"><span data-stu-id="99baf-102">Create the JavaScript Client</span></span>
====================
<span data-ttu-id="99baf-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="99baf-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="99baf-104">Pobieranie ukończone projektu</span><span class="sxs-lookup"><span data-stu-id="99baf-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="99baf-105">W tej sekcji utworzysz klienta dla aplikacji, za pomocą kodu HTML, JavaScript oraz [struktura Knockout.js](http://knockoutjs.com/) biblioteki.</span><span class="sxs-lookup"><span data-stu-id="99baf-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="99baf-106">Utworzymy aplikację kliencką w etapach:</span><span class="sxs-lookup"><span data-stu-id="99baf-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="99baf-107">Wyświetlanie listy książek.</span><span class="sxs-lookup"><span data-stu-id="99baf-107">Showing a list of books.</span></span>
- <span data-ttu-id="99baf-108">Wyświetlanie szczegółów książki.</span><span class="sxs-lookup"><span data-stu-id="99baf-108">Showing a book detail.</span></span>
- <span data-ttu-id="99baf-109">Dodawanie nowej książki.</span><span class="sxs-lookup"><span data-stu-id="99baf-109">Adding a new book.</span></span>

<span data-ttu-id="99baf-110">Bibliotekę Knockout używa wzorca Model-View-ViewModel (MVVM):</span><span class="sxs-lookup"><span data-stu-id="99baf-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="99baf-111">**Modelu** jest po stronie serwera reprezentacja danych w domenie firmy (w naszym przypadku, książek i autorzy).</span><span class="sxs-lookup"><span data-stu-id="99baf-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="99baf-112">**Widoku** to warstwa prezentacji (HTML).</span><span class="sxs-lookup"><span data-stu-id="99baf-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="99baf-113">**Model widoku** jest obiekt JavaScript, który przechowuje modeli.</span><span class="sxs-lookup"><span data-stu-id="99baf-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="99baf-114">Model widoku jest abstrakcji kodu interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="99baf-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="99baf-115">Go nie zna reprezentacji w formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="99baf-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="99baf-116">Zamiast tego, reprezentuje abstrakcyjnej funkcji widoku, takie jak &quot;listy książek&quot;.</span><span class="sxs-lookup"><span data-stu-id="99baf-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="99baf-117">Widok jest powiązany z danymi model widoku.</span><span class="sxs-lookup"><span data-stu-id="99baf-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="99baf-118">Model widoku aktualizacje są automatycznie odzwierciedlane w widoku.</span><span class="sxs-lookup"><span data-stu-id="99baf-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="99baf-119">Model widoku również pobiera zdarzenia w widoku, takich jak kliknięcie przycisku.</span><span class="sxs-lookup"><span data-stu-id="99baf-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="99baf-120">Tej metody można łatwo zmienić układ i interfejsu użytkownika aplikacji, ponieważ powiązania, można zmienić bez konieczności ponownego zapisu żadnego kodu.</span><span class="sxs-lookup"><span data-stu-id="99baf-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="99baf-121">Na przykład może wyświetlać listę elementów jako `<ul>`, zmienić ją później do tabeli.</span><span class="sxs-lookup"><span data-stu-id="99baf-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="99baf-122">Dodaj bibliotekę Knockout</span><span class="sxs-lookup"><span data-stu-id="99baf-122">Add the Knockout Library</span></span>

<span data-ttu-id="99baf-123">W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**.</span><span class="sxs-lookup"><span data-stu-id="99baf-123">In Visual Studio, from the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="99baf-124">Następnie wybierz pozycję **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="99baf-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="99baf-125">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="99baf-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="99baf-126">To polecenie dodaje pliki Knockout na folder skryptów.</span><span class="sxs-lookup"><span data-stu-id="99baf-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="99baf-127">Tworzenie modelu widoku</span><span class="sxs-lookup"><span data-stu-id="99baf-127">Create the View Model</span></span>

<span data-ttu-id="99baf-128">Dodaj plik języka JavaScript o nazwie app.js na folder skryptów.</span><span class="sxs-lookup"><span data-stu-id="99baf-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="99baf-129">(W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder skryptów, wybierz **Dodaj**, a następnie wybierz **plik JavaScript**.) Wklej następujący kod:</span><span class="sxs-lookup"><span data-stu-id="99baf-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="99baf-130">W Knockout `observable` klasa umożliwia powiązanie danych.</span><span class="sxs-lookup"><span data-stu-id="99baf-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="99baf-131">Zawartość możliwość obserwowania zmiany, możliwość obserwowania powiadamia wszystkie formanty powiązane z danymi, dzięki czemu mogą aktualizować same.</span><span class="sxs-lookup"><span data-stu-id="99baf-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="99baf-132">( `observableArray` Klasy jest tablica wersją *obserwowalnymi*.) Na początek z nasz model widoku ma dwa dostrzegalne elementy:</span><span class="sxs-lookup"><span data-stu-id="99baf-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="99baf-133">`books` zawiera listy książek.</span><span class="sxs-lookup"><span data-stu-id="99baf-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="99baf-134">`error` zawiera komunikat o błędzie w przypadku niepowodzenia wywołania AJAX.</span><span class="sxs-lookup"><span data-stu-id="99baf-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="99baf-135">`getAllBooks` Metoda przeprowadza wywołanie AJAX w celu uzyskania listy książek.</span><span class="sxs-lookup"><span data-stu-id="99baf-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="99baf-136">A następnie umieszcza ono wynik na `books` tablicy.</span><span class="sxs-lookup"><span data-stu-id="99baf-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="99baf-137">`ko.applyBindings` Metodą jest częścią biblioteki Knockout.</span><span class="sxs-lookup"><span data-stu-id="99baf-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="99baf-138">Jej modelu widoku jako parametr przyjmuje i konfiguruje powiązanie danych.</span><span class="sxs-lookup"><span data-stu-id="99baf-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="99baf-139">Dodaj pakiet skryptu</span><span class="sxs-lookup"><span data-stu-id="99baf-139">Add a Script Bundle</span></span>

<span data-ttu-id="99baf-140">Tworzenie pakietów to funkcja w programie ASP.NET 4.5, który można łatwo połączyć lub wiele plików pakietu w jednym pliku.</span><span class="sxs-lookup"><span data-stu-id="99baf-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="99baf-141">Tworzenie pakietów pozwala zmniejszyć liczbę żądań do serwera, co może poprawić czas ładowania strony.</span><span class="sxs-lookup"><span data-stu-id="99baf-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="99baf-142">Otwórz plik App\_Start/BundleConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="99baf-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="99baf-143">Dodaj następujący kod do metody RegisterBundles.</span><span class="sxs-lookup"><span data-stu-id="99baf-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> <span data-ttu-id="99baf-144">[Poprzednie](part-5.md)
> [dalej](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="99baf-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
