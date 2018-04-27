---
title: Wprowadzenie do platformy ASP.NET Core stron Razor
author: Rick-Anderson
description: Dowiedz się, jak Razor strony platformy ASP.NET Core umożliwia kodowania scenariusze strony łatwiejsze i bardziej wydajnej pracy niż przy użyciu platformy MVC.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: mvc/razor-pages/index
ms.openlocfilehash: 5e2b53a4771a97b0a4091f593720b9c0e4e345bf
ms.sourcegitcommit: c4a31aaf902f2e84aaf4a9d882ca980fdf6488c0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="12b6f-103">Wprowadzenie do platformy ASP.NET Core stron Razor</span><span class="sxs-lookup"><span data-stu-id="12b6f-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="12b6f-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="12b6f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="12b6f-105">Stron razor to nowa funkcja platformy ASP.NET Core MVC umożliwia kodowanie strony scenariusze łatwiejsze i bardziej wydajnej pracy.</span><span class="sxs-lookup"><span data-stu-id="12b6f-105">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="12b6f-106">Jeśli szukasz samouczka, który korzysta z podejścia Model-View-Controller, zobacz [Rozpoczynanie pracy z platformą ASP.NET MVC Core](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="12b6f-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="12b6f-107">Ten dokument zawiera wprowadzenie do stron Razor.</span><span class="sxs-lookup"><span data-stu-id="12b6f-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="12b6f-108">Nie jest samouczek krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="12b6f-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="12b6f-109">Jeśli możesz znaleźć sekcje zbyt zaawansowanych, zobacz [wprowadzenie stron Razor](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="12b6f-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="12b6f-110">Omówienie platformy ASP.NET Core, zobacz [wprowadzenie do platformy ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="12b6f-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12b6f-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="12b6f-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="12b6f-112">Tworzenie projektu stron Razor</span><span class="sxs-lookup"><span data-stu-id="12b6f-112">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="12b6f-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="12b6f-113">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="12b6f-114">Zobacz [wprowadzenie stron Razor](xref:tutorials/razor-pages/razor-pages-start) szczegółowe informacje dotyczące sposobu tworzenia stron Razor projektu za pomocą programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="12b6f-114">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="12b6f-115">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="12b6f-115">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="12b6f-116">Uruchom `dotnet new razor` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="12b6f-116">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="12b6f-117">Otwórz wygenerowany *.csproj* plików z programu Visual Studio dla komputerów Mac.</span><span class="sxs-lookup"><span data-stu-id="12b6f-117">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="12b6f-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="12b6f-118">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="12b6f-119">Uruchom `dotnet new razor` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="12b6f-119">Run `dotnet new razor` from the command line.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="12b6f-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="12b6f-120">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="12b6f-121">Uruchom `dotnet new razor` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="12b6f-121">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="12b6f-122">Stron razor</span><span class="sxs-lookup"><span data-stu-id="12b6f-122">Razor Pages</span></span>

<span data-ttu-id="12b6f-123">Stron razor jest włączone w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="12b6f-123">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="12b6f-124">Należy wziąć pod uwagę strony podstawowej: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="12b6f-124">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="12b6f-125">Poprzedni kod znacznie wygląda jak plik widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="12b6f-125">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="12b6f-126">Jest to, co ułatwia różnych `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="12b6f-126">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="12b6f-127">`@page` powoduje, że plik na akcję MVC — czyli obsługi żądań bezpośrednio, bez przechodzenia przez kontroler.</span><span class="sxs-lookup"><span data-stu-id="12b6f-127">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="12b6f-128">`@page` musi być pierwszym dyrektywy Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="12b6f-128">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="12b6f-129">`@page` wpływa na działanie innych konstrukcji Razor.</span><span class="sxs-lookup"><span data-stu-id="12b6f-129">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="12b6f-130">Podobne strony przy użyciu `PageModel` klasa, jest wyświetlany w obszarze następujące dwa pliki.</span><span class="sxs-lookup"><span data-stu-id="12b6f-130">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="12b6f-131">*Pages/Index2.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="12b6f-131">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="12b6f-132">*Pages/Index2.cshtml.cs* modelu strony:</span><span class="sxs-lookup"><span data-stu-id="12b6f-132">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="12b6f-133">Według konwencji `PageModel` pliku klasy ma taką samą nazwę jak plik Razor strony z *.cs* dołączane.</span><span class="sxs-lookup"><span data-stu-id="12b6f-133">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="12b6f-134">Na przykład na poprzedniej stronie aparatu Razor jest *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="12b6f-134">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="12b6f-135">Plik zawierający `PageModel` nosi nazwę klasy *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="12b6f-135">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="12b6f-136">Skojarzenia ścieżki adresu URL do strony zależą od lokalizacji strony w systemie plików.</span><span class="sxs-lookup"><span data-stu-id="12b6f-136">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="12b6f-137">W poniższej tabeli przedstawiono ścieżki Razor strony i dopasowywania adresu URL:</span><span class="sxs-lookup"><span data-stu-id="12b6f-137">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="12b6f-138">Nazwa i ścieżka pliku</span><span class="sxs-lookup"><span data-stu-id="12b6f-138">File name and path</span></span>               | <span data-ttu-id="12b6f-139">Dopasowywanie adresu URL</span><span class="sxs-lookup"><span data-stu-id="12b6f-139">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="12b6f-140">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="12b6f-140">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="12b6f-141">`/` lub `/Index`</span><span class="sxs-lookup"><span data-stu-id="12b6f-141">`/` or `/Index`</span></span> |
| <span data-ttu-id="12b6f-142">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="12b6f-142">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="12b6f-143">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="12b6f-143">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="12b6f-144">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="12b6f-144">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="12b6f-145">`/Store` lub `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="12b6f-145">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="12b6f-146">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="12b6f-146">Notes:</span></span>

* <span data-ttu-id="12b6f-147">Środowisko uruchomieniowe wyszukuje pliki stron Razor w *stron* folderu domyślnie.</span><span class="sxs-lookup"><span data-stu-id="12b6f-147">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="12b6f-148">`Index` jest domyślną stronę, gdy adres URL nie zawiera strony.</span><span class="sxs-lookup"><span data-stu-id="12b6f-148">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="12b6f-149">Zapisywanie formularza podstawowego</span><span class="sxs-lookup"><span data-stu-id="12b6f-149">Writing a basic form</span></span>

<span data-ttu-id="12b6f-150">Funkcje stron razor ułatwiają typowe wzorce używane w przeglądarkach łatwe.</span><span class="sxs-lookup"><span data-stu-id="12b6f-150">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="12b6f-151">[Model powiązania](xref:mvc/models/model-binding), [pomocników tagów](xref:mvc/views/tag-helpers/intro)i wszystkich pomocników HTML *tylko pracy* z właściwościami zdefiniowana w klasie Razor strony.</span><span class="sxs-lookup"><span data-stu-id="12b6f-151">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="12b6f-152">Należy wziąć pod uwagę strona, która implementuje podstawowego "Skontaktuj się z nami" tworzą dla `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="12b6f-152">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="12b6f-153">Aby wyświetlić przykłady w tym dokumencie `DbContext` został zainicjowany w [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) pliku.</span><span class="sxs-lookup"><span data-stu-id="12b6f-153">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="12b6f-154">Model danych:</span><span class="sxs-lookup"><span data-stu-id="12b6f-154">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="12b6f-155">Kontekst bazy danych:</span><span class="sxs-lookup"><span data-stu-id="12b6f-155">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="12b6f-156">*Pages/Create.cshtml* pliku widoku:</span><span class="sxs-lookup"><span data-stu-id="12b6f-156">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="12b6f-157">*Pages/Create.cshtml.cs* modelu strony:</span><span class="sxs-lookup"><span data-stu-id="12b6f-157">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="12b6f-158">Według konwencji `PageModel` nosi nazwę klasy `<PageName>Model` i znajduje się w tej samej przestrzeni nazw jako strony.</span><span class="sxs-lookup"><span data-stu-id="12b6f-158">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="12b6f-159">`PageModel` Klasa umożliwia oddzielenie logiki strony z jego prezentacji.</span><span class="sxs-lookup"><span data-stu-id="12b6f-159">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="12b6f-160">Definiuje stronę obsługi żądania wysyłane na stronie i dane używane do renderowania strony.</span><span class="sxs-lookup"><span data-stu-id="12b6f-160">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="12b6f-161">Ta separacja umożliwia zarządzanie zależności strony za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection) i [testu jednostkowego](xref:testing/razor-pages-testing) stron.</span><span class="sxs-lookup"><span data-stu-id="12b6f-161">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="12b6f-162">Na stronie `OnPostAsync` *metoda obsługi*, która działa w `POST` żąda (gdy użytkownik zapisuje formularz).</span><span class="sxs-lookup"><span data-stu-id="12b6f-162">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="12b6f-163">Można dodać obsługi metod dla dowolnej zlecenie HTTP.</span><span class="sxs-lookup"><span data-stu-id="12b6f-163">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="12b6f-164">Są najczęściej programów obsługi:</span><span class="sxs-lookup"><span data-stu-id="12b6f-164">The most common handlers are:</span></span>

* <span data-ttu-id="12b6f-165">`OnGet` Aby zainicjować stanu potrzebne dla strony.</span><span class="sxs-lookup"><span data-stu-id="12b6f-165">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="12b6f-166">[OnGet](#OnGet) próbki.</span><span class="sxs-lookup"><span data-stu-id="12b6f-166">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="12b6f-167">`OnPost` do obsługi przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="12b6f-167">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="12b6f-168">`Async` Sufiks nazwy jest opcjonalne, ale często używane przez Konwencję dla funkcji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="12b6f-168">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="12b6f-169">`OnPostAsync` Kodu w poprzednim przykładzie wygląda podobnie do co zwykle piszesz w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="12b6f-169">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="12b6f-170">Poprzedni kod jest typowe dla stron Razor.</span><span class="sxs-lookup"><span data-stu-id="12b6f-170">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="12b6f-171">Większość podstawowych MVC, takich jak [modelu powiązania](xref:mvc/models/model-binding), [weryfikacji](xref:mvc/models/validation), i są współdzielone wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="12b6f-171">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="12b6f-172">Poprzedni `OnPostAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="12b6f-172">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="12b6f-173">Podstawowy przepływ `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="12b6f-173">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="12b6f-174">Sprawdź błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="12b6f-174">Check for validation errors.</span></span>

*  <span data-ttu-id="12b6f-175">Jeśli nie ma żadnych błędów, Zapisz dane i przekierowanie.</span><span class="sxs-lookup"><span data-stu-id="12b6f-175">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="12b6f-176">Jeśli wystąpią błędy, Wyświetl stronę ponownie, podając komunikatów dotyczących sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="12b6f-176">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="12b6f-177">Weryfikacja po stronie klienta jest identyczna jak tradycyjne aplikacje platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="12b6f-177">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="12b6f-178">W wielu przypadkach błędy sprawdzania poprawności może być wykryte na komputerze klienckim i nigdy nie zostały przekazane do serwera.</span><span class="sxs-lookup"><span data-stu-id="12b6f-178">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="12b6f-179">Po pomyślnym wprowadzeniu danych `OnPostAsync` wywołań metody obsługi `RedirectToPage` metody pomocnika do zwrócenia wystąpienia klasy `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="12b6f-179">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="12b6f-180">`RedirectToPage` jest nowy wynik akcji, podobnie jak `RedirectToAction` lub `RedirectToRoute`, ale dostosowanych stron.</span><span class="sxs-lookup"><span data-stu-id="12b6f-180">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="12b6f-181">W poprzednim przykładzie przekierowuje do strony indeksu głównego (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="12b6f-181">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="12b6f-182">`RedirectToPage` została szczegółowo opisana w [generowania adresu URL dla stron](#url_gen) sekcji.</span><span class="sxs-lookup"><span data-stu-id="12b6f-182">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="12b6f-183">Jeśli przesłanego formularza zawiera błędy sprawdzania poprawności (które są przekazywane do serwera),`OnPostAsync` wywołań metody obsługi `Page` metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="12b6f-183">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="12b6f-184">`Page` Zwraca wystąpienie klasy `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="12b6f-184">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="12b6f-185">Zwracanie `Page` jest podobny do sposób zwrócenia akcje w kontrolerach `View`.</span><span class="sxs-lookup"><span data-stu-id="12b6f-185">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="12b6f-186">`PageResult` Wartość domyślna to <!-- Review  --> zwracany typ metody obsługi.</span><span class="sxs-lookup"><span data-stu-id="12b6f-186">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="12b6f-187">Metoda obsługi, która zwraca `void` renderuje stronę.</span><span class="sxs-lookup"><span data-stu-id="12b6f-187">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="12b6f-188">`Customer` Używa właściwości `[BindProperty]` atrybutu korzystania z wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="12b6f-188">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="12b6f-189">Stron razor domyślnie powiązania właściwości tylko z innych niż GET zleceń.</span><span class="sxs-lookup"><span data-stu-id="12b6f-189">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="12b6f-190">Powiązanie właściwości może zmniejszyć ilość kodu, które trzeba zapisać.</span><span class="sxs-lookup"><span data-stu-id="12b6f-190">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="12b6f-191">Powiązanie zmniejsza kodu przy użyciu tej samej właściwości do renderowania pól formularza (`<input asp-for="Customer.Name" />`) i akceptuje dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="12b6f-191">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="12b6f-192">Ze względów bezpieczeństwa należy zgadzaj się na wiązanie danych żądania GET do strony właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="12b6f-192">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="12b6f-193">Sprawdź dane wejściowe użytkownika przed zamapowaniem ją do właściwości.</span><span class="sxs-lookup"><span data-stu-id="12b6f-193">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="12b6f-194">Zgody na korzystanie z to zachowanie jest przydatne podczas kompilowania funkcji, które zależą od wartości ciągu lub trasy kwerendy.</span><span class="sxs-lookup"><span data-stu-id="12b6f-194">Opting in to this behavior is useful when building features which rely on query string or route values.</span></span>
>
> <span data-ttu-id="12b6f-195">Aby powiązać właściwość na żądania GET, ustaw `[BindProperty]` atrybutu `SupportsGet` właściwości `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="12b6f-195">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="12b6f-196">Strona główna (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="12b6f-196">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="12b6f-197">Kod związany z *Index.cshtml.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="12b6f-197">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="12b6f-198">*Index.cshtml* pliku zawiera następujące znaczniki, aby utworzyć link edycji dla każdego kontakt:</span><span class="sxs-lookup"><span data-stu-id="12b6f-198">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="12b6f-199">[Pomocnika Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) używane `asp-route-{value}` atrybut do generowania łącza do edycji strony.</span><span class="sxs-lookup"><span data-stu-id="12b6f-199">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="12b6f-200">Ten link zawiera dane trasy, o kontakt identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="12b6f-200">The link contains route data with the contact ID.</span></span> <span data-ttu-id="12b6f-201">Na przykład `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="12b6f-201">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="12b6f-202">*Pages/Edit.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="12b6f-202">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="12b6f-203">Pierwszy wiersz zawiera `@page "{id:int}"` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="12b6f-203">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="12b6f-204">Ograniczenie routingu`"{id:int}"` informuje strony do akceptowania żądań do strony, które zawierają `int` danych trasy.</span><span class="sxs-lookup"><span data-stu-id="12b6f-204">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="12b6f-205">Jeśli żądanie do strony nie zawiera danych trasy, który może zostać przekonwertowany na `int`, środowisko uruchomieniowe zwraca błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="12b6f-205">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="12b6f-206">Aby wprowadzić identyfikator opcjonalne, dołącz `?` ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="12b6f-206">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="12b6f-207">*Pages/Edit.cshtml.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="12b6f-207">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="12b6f-208">*Index.cshtml* plik zawiera także znaczników do utworzenia przycisk Usuń widoczny dla każdego kontakt klienta:</span><span class="sxs-lookup"><span data-stu-id="12b6f-208">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="12b6f-209">Przycisk usuwania jest renderowany w języku HTML, jego `formaction` zawiera parametry:</span><span class="sxs-lookup"><span data-stu-id="12b6f-209">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="12b6f-210">Określony przez identyfikator kontaktu klienta z `asp-route-id` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="12b6f-210">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="12b6f-211">`handler` Określonego przez `asp-page-handler` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="12b6f-211">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="12b6f-212">Poniżej przedstawiono przykładowy przycisk usuwania renderowany z klientem skontaktuj się z Identyfikatorem `1`:</span><span class="sxs-lookup"><span data-stu-id="12b6f-212">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="12b6f-213">Jeśli przycisk jest zaznaczony, formularz `POST` wysłaniu żądania do serwera.</span><span class="sxs-lookup"><span data-stu-id="12b6f-213">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="12b6f-214">Według Konwencji wybrano nazwę metody obsługi na podstawie wartości `handler` parametru zgodnie ze schematem `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="12b6f-214">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="12b6f-215">Ponieważ `handler` jest `delete` w tym przykładzie `OnPostDeleteAsync` metoda obsługi jest używany do procesu `POST` żądania.</span><span class="sxs-lookup"><span data-stu-id="12b6f-215">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="12b6f-216">Jeśli `asp-page-handler` ustawiono innej wartości, takich jak `remove`, strona metody obsługi o nazwie `OnPostRemoveAsync` jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="12b6f-216">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="12b6f-217">`OnPostDeleteAsync` Metody:</span><span class="sxs-lookup"><span data-stu-id="12b6f-217">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="12b6f-218">Akceptuje `id` z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="12b6f-218">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="12b6f-219">Wysyła zapytanie do bazy danych dla klienta kontakt z `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="12b6f-219">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="12b6f-220">Jeśli zostanie znaleziony kontaktowe klienta, są one usunięte z listy kontaktów klienta.</span><span class="sxs-lookup"><span data-stu-id="12b6f-220">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="12b6f-221">Baza danych jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="12b6f-221">The database is updated.</span></span>
* <span data-ttu-id="12b6f-222">Wywołania `RedirectToPage` ma nastąpić przekierowanie do strony indeksu głównego (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="12b6f-222">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="12b6f-223">XSRF/CSRF i stron Razor</span><span class="sxs-lookup"><span data-stu-id="12b6f-223">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="12b6f-224">Nie masz do pisania kodu dla [antiforgery weryfikacji](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="12b6f-224">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="12b6f-225">Antiforgery generowania tokenów i weryfikacja są automatycznie umieszczane w stron Razor.</span><span class="sxs-lookup"><span data-stu-id="12b6f-225">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="12b6f-226">Za pomocą układów, częściowe, szablonów i pomocników tagów Razor strony</span><span class="sxs-lookup"><span data-stu-id="12b6f-226">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="12b6f-227">Strony działać z wszystkimi funkcjami aparatu widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="12b6f-227">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="12b6f-228">Układy, częściowe, szablony, pomocników tagów *_ViewStart.cshtml*, *_ViewImports.cshtml* pracy w taki sam sposób jak w przypadku konwencjonalnych widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="12b6f-228">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="12b6f-229">Korzystając z tych funkcji umożliwia declutter tej strony.</span><span class="sxs-lookup"><span data-stu-id="12b6f-229">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="12b6f-230">Dodaj [układ strony](xref:mvc/views/layout) do *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="12b6f-230">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="12b6f-231">[Układu](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="12b6f-231">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="12b6f-232">Określa układ każdej strony (o ile nie zdecyduje strony poza układ).</span><span class="sxs-lookup"><span data-stu-id="12b6f-232">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="12b6f-233">Importuje struktury HTML, takich jak JavaScript i arkusze stylów.</span><span class="sxs-lookup"><span data-stu-id="12b6f-233">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="12b6f-234">Zobacz [układ strony](xref:mvc/views/layout) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="12b6f-234">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="12b6f-235">[Układu](xref:mvc/views/layout#specifying-a-layout) właściwość jest ustawiona *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="12b6f-235">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="12b6f-236">**Uwaga:** układ jest *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="12b6f-236">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="12b6f-237">Strony wyszukać innych widoków (układy, szablony, częściowe) hierarchicznie, uruchamianie w tym samym folderze co bieżąca strona.</span><span class="sxs-lookup"><span data-stu-id="12b6f-237">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="12b6f-238">Układ w *stron* folderu można używać z dowolnej strony Razor, w obszarze *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="12b6f-238">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="12b6f-239">Firma Microsoft zaleca **nie** umieścić plik układu w *widoków/Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="12b6f-239">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="12b6f-240">*Widoki/Shared* jest wzorzec widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="12b6f-240">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="12b6f-241">Stron razor są przeznaczone do zależą od hierarchii folderów, nie ścieżkę Konwencji.</span><span class="sxs-lookup"><span data-stu-id="12b6f-241">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="12b6f-242">Widok wyszukiwania ze strony Razor obejmuje *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="12b6f-242">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="12b6f-243">Układy, szablonów i częściowe jest używany z konwencjonalnej Razor widoków i kontrolerów MVC *tylko pracy*.</span><span class="sxs-lookup"><span data-stu-id="12b6f-243">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="12b6f-244">Dodaj *Pages/_ViewImports.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="12b6f-244">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="12b6f-245">`@namespace` znajduje się w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="12b6f-245">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="12b6f-246">`@addTagHelper` Dyrektywy dostarcza [wbudowanych pomocników tagów](xref:mvc/views/tag-helpers/builtin-th/Index) dla wszystkich stron w *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="12b6f-246">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="12b6f-247">Gdy `@namespace` dyrektywa jest używana jawnie na stronie:</span><span class="sxs-lookup"><span data-stu-id="12b6f-247">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="12b6f-248">Dyrektywa ustawia obszar nazw dla strony.</span><span class="sxs-lookup"><span data-stu-id="12b6f-248">The directive sets the namespace for the page.</span></span> <span data-ttu-id="12b6f-249">`@model` Dyrektywy nie musi obejmować przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="12b6f-249">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="12b6f-250">Gdy `@namespace` dyrektywy znajduje się w *_ViewImports.cshtml*, określonego obszaru nazw dostarcza prefiksu dla przestrzeni nazw wygenerowane na stronie, który importuje `@namespace` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="12b6f-250">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="12b6f-251">Pozostała część wygenerowanej (część sufiks) jest oddzielona kropkami ścieżki względnej między folder zawierający *_ViewImports.cshtml* i folderu zawierającego strony.</span><span class="sxs-lookup"><span data-stu-id="12b6f-251">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="12b6f-252">Na przykład kodzie pliku *Pages/Customers/Edit.cshtml.cs* jawnie Ustawia obszar nazw:</span><span class="sxs-lookup"><span data-stu-id="12b6f-252">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="12b6f-253">*Pages/_ViewImports.cshtml* pliku ustawia następujących nazw:</span><span class="sxs-lookup"><span data-stu-id="12b6f-253">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="12b6f-254">Wygenerowany obszar nazw dla *Pages/Customers/Edit.cshtml* Razor strony jest taka sama jak pliku CodeBehind.</span><span class="sxs-lookup"><span data-stu-id="12b6f-254">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="12b6f-255">`@namespace` Dyrektywa została zaprojektowana tak klasy C# dodane do projektu i stron wygenerowany kod *tylko pracy* bez dodawania `@using` dyrektywy do pliku CodeBehind.</span><span class="sxs-lookup"><span data-stu-id="12b6f-255">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="12b6f-256">**Uwaga:** `@namespace` współdziała również z konwencjonalnej widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="12b6f-256">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="12b6f-257">Oryginalna *Pages/Create.cshtml* pliku widoku:</span><span class="sxs-lookup"><span data-stu-id="12b6f-257">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="12b6f-258">Zaktualizowany interfejs *Pages/Create.cshtml* pliku widoku:</span><span class="sxs-lookup"><span data-stu-id="12b6f-258">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="12b6f-259">[Projektu starter stron Razor](#rpvs17) zawiera *Pages/_ValidationScriptsPartial.cshtml*, który przechwytuje sprawdzania poprawności po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="12b6f-259">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="12b6f-260">Generowania adresu URL dla stron</span><span class="sxs-lookup"><span data-stu-id="12b6f-260">URL generation for Pages</span></span>

<span data-ttu-id="12b6f-261">`Create` Strony pokazana wcześniej, używa `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="12b6f-261">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="12b6f-262">Aplikacja ma następującą strukturę plik lub folder:</span><span class="sxs-lookup"><span data-stu-id="12b6f-262">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="12b6f-263">*/ Stron*</span><span class="sxs-lookup"><span data-stu-id="12b6f-263">*/Pages*</span></span>

  * <span data-ttu-id="12b6f-264">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="12b6f-264">*Index.cshtml*</span></span>
  * <span data-ttu-id="12b6f-265">*/ Klientów*</span><span class="sxs-lookup"><span data-stu-id="12b6f-265">*/Customers*</span></span>

    * <span data-ttu-id="12b6f-266">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="12b6f-266">*Create.cshtml*</span></span>
    * <span data-ttu-id="12b6f-267">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="12b6f-267">*Edit.cshtml*</span></span>
    * <span data-ttu-id="12b6f-268">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="12b6f-268">*Index.cshtml*</span></span>

<span data-ttu-id="12b6f-269">*Pages/Customers/Create.cshtml* i *Pages/Customers/Edit.cshtml* stron przekierowania do *Pages/Index.cshtml* po pomyślnym.</span><span class="sxs-lookup"><span data-stu-id="12b6f-269">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="12b6f-270">Ciąg `/Index` jest częścią identyfikatora URI uzyskać dostęp do poprzedniej strony.</span><span class="sxs-lookup"><span data-stu-id="12b6f-270">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="12b6f-271">Ciąg `/Index` może służyć do generowania identyfikatorów URI *Pages/Index.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="12b6f-271">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="12b6f-272">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="12b6f-272">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="12b6f-273">Nazwa strony jest ścieżka do strony z katalogu głównego */strony* folderze (w tym na początku `/`, na przykład `/Index`).</span><span class="sxs-lookup"><span data-stu-id="12b6f-273">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="12b6f-274">Poprzedniej próbki generowania adresu URL są bardziej zaawansowanej funkcji niż hardcoding tylko adres URL.</span><span class="sxs-lookup"><span data-stu-id="12b6f-274">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="12b6f-275">Używa generowania adresu URL [routingu](xref:mvc/controllers/routing) i generowanie i kodowanie parametry zgodnie z sposób definiowania trasy w ścieżce docelowej.</span><span class="sxs-lookup"><span data-stu-id="12b6f-275">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="12b6f-276">Generowania adresu URL dla stron obsługuje nazw względnych.</span><span class="sxs-lookup"><span data-stu-id="12b6f-276">URL generation for pages supports relative names.</span></span> <span data-ttu-id="12b6f-277">W poniższej tabeli przedstawiono stronę indeksu, która jest zaznaczone z różnych `RedirectToPage` parametry z *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="12b6f-277">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="12b6f-278">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="12b6f-278">RedirectToPage(x)</span></span>| <span data-ttu-id="12b6f-279">Strona</span><span class="sxs-lookup"><span data-stu-id="12b6f-279">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="12b6f-280">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="12b6f-280">RedirectToPage("/Index")</span></span> | <span data-ttu-id="12b6f-281">*Strony/indeksu*</span><span class="sxs-lookup"><span data-stu-id="12b6f-281">*Pages/Index*</span></span> |
| <span data-ttu-id="12b6f-282">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="12b6f-282">RedirectToPage("./Index");</span></span> | <span data-ttu-id="12b6f-283">*Strony/klientów/indeksu*</span><span class="sxs-lookup"><span data-stu-id="12b6f-283">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="12b6f-284">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="12b6f-284">RedirectToPage("../Index")</span></span> | <span data-ttu-id="12b6f-285">*Strony/indeksu*</span><span class="sxs-lookup"><span data-stu-id="12b6f-285">*Pages/Index*</span></span> |
| <span data-ttu-id="12b6f-286">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="12b6f-286">RedirectToPage("Index")</span></span>  | <span data-ttu-id="12b6f-287">*Strony/klientów/indeksu*</span><span class="sxs-lookup"><span data-stu-id="12b6f-287">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="12b6f-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, i `RedirectToPage("../Index")` są <em>względne nazwy</em>.</span><span class="sxs-lookup"><span data-stu-id="12b6f-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are <em>relative names</em>.</span></span> <span data-ttu-id="12b6f-289">`RedirectToPage` Parametr jest <em>łączyć</em> ze ścieżką bieżącej strony do obliczenia nazwę strony docelowej.</span><span class="sxs-lookup"><span data-stu-id="12b6f-289">The `RedirectToPage` parameter is <em>combined</em> with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="12b6f-290">Nazwa względna konsolidacji jest przydatne, gdy tworzenie witryn ze strukturą złożonych.</span><span class="sxs-lookup"><span data-stu-id="12b6f-290">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="12b6f-291">Jeśli używasz nazwy względne do połączenia między stronami w folderze, można zmienić nazwę tego folderu.</span><span class="sxs-lookup"><span data-stu-id="12b6f-291">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="12b6f-292">Wszystkie linki nadal działać (ponieważ one nie obejmować nazwę folderu).</span><span class="sxs-lookup"><span data-stu-id="12b6f-292">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="12b6f-293">TempData</span><span class="sxs-lookup"><span data-stu-id="12b6f-293">TempData</span></span>

<span data-ttu-id="12b6f-294">Przedstawia platformy ASP.NET Core [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) właściwość [kontrolera](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="12b6f-294">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="12b6f-295">Ta właściwość przechowuje dane, dopóki nie jest do odczytu.</span><span class="sxs-lookup"><span data-stu-id="12b6f-295">This property stores data until it's read.</span></span> <span data-ttu-id="12b6f-296">`Keep` i `Peek` metod można użyć do sprawdzenia danych bez usuwania.</span><span class="sxs-lookup"><span data-stu-id="12b6f-296">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="12b6f-297">`TempData` jest przydatne w przypadku przekierowania, gdy dane są potrzebne dla więcej niż jednego żądania.</span><span class="sxs-lookup"><span data-stu-id="12b6f-297">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="12b6f-298">`[TempData]` Jest nowy w programie ASP.NET 2.0 Core i jest obsługiwane na stronach i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="12b6f-298">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="12b6f-299">Poniższy kod ustawia wartość `Message` przy użyciu `TempData`:</span><span class="sxs-lookup"><span data-stu-id="12b6f-299">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="12b6f-300">Następujący kod w *Pages/Customers/Index.cshtml* plik zawiera wartość `Message` przy użyciu `TempData`.</span><span class="sxs-lookup"><span data-stu-id="12b6f-300">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="12b6f-301">*Pages/Customers/Index.cshtml.cs* stosuje modelu strony `[TempData]` atrybutu `Message` właściwości.</span><span class="sxs-lookup"><span data-stu-id="12b6f-301">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="12b6f-302">Zobacz [TempData](xref:fundamentals/app-state#temp) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="12b6f-302">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="12b6f-303">Wielu obsług na stronie</span><span class="sxs-lookup"><span data-stu-id="12b6f-303">Multiple handlers per page</span></span>

<span data-ttu-id="12b6f-304">Następująca strona generuje kod znaczników dla dwie strony przy użyciu programów obsługi `asp-page-handler` pomocnika tagów:</span><span class="sxs-lookup"><span data-stu-id="12b6f-304">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="12b6f-305">Formularz w poprzednim przykładzie ma dwa przesłać przyciski, za pomocą każdej `FormActionTagHelper` do przesłania do innego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="12b6f-305">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="12b6f-306">`asp-page-handler` Atrybut jest dodatek do `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="12b6f-306">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="12b6f-307">`asp-page-handler` generuje adresów URL, które przesłania do każdej z metod obsługi zdefiniowane przez stronę.</span><span class="sxs-lookup"><span data-stu-id="12b6f-307">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="12b6f-308">`asp-page` nie jest określony, ponieważ próbki jest konsolidacja do bieżącej strony.</span><span class="sxs-lookup"><span data-stu-id="12b6f-308">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="12b6f-309">Model strony:</span><span class="sxs-lookup"><span data-stu-id="12b6f-309">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="12b6f-310">W poprzednim kodzie użyto *o nazwie metod obsługi*.</span><span class="sxs-lookup"><span data-stu-id="12b6f-310">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="12b6f-311">Metody o nazwie procedury obsługi są tworzone przez pobieranie tekstu w nazwie po `On<HTTP Verb>` i przed `Async` (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="12b6f-311">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="12b6f-312">W powyższym przykładzie metody strony są OnPost**JoinList**Async i OnPost**JoinListUC**asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="12b6f-312">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="12b6f-313">Z *OnPost* i *Async* usunięta, nazwy programu obsługi są `JoinList` i `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="12b6f-313">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="12b6f-314">Przy użyciu poprzedniego kodu ścieżkę adresu URL, który przesyła do `OnPostJoinListAsync` jest `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="12b6f-314">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="12b6f-315">Ścieżka adresu URL, który przesyła do `OnPostJoinListUCAsync` jest `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="12b6f-315">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="12b6f-316">Dostosowywanie routingu</span><span class="sxs-lookup"><span data-stu-id="12b6f-316">Customizing Routing</span></span>

<span data-ttu-id="12b6f-317">Jeśli nie chcesz, ciąg zapytania `?handler=JoinList` w adresie URL, można zmienić trasy, aby umieścić nazwę programu obsługi w części ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="12b6f-317">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="12b6f-318">Trasy można dostosować, dodając szablonu trasy ująć w podwójny cudzysłów po `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="12b6f-318">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="12b6f-319">Trasa poprzedniego umieszcza nazwa programu obsługi ścieżki adresu URL zamiast ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="12b6f-319">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="12b6f-320">`?` Następujące `handler` oznacza, że parametr trasy jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="12b6f-320">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="12b6f-321">Można użyć `@page` dodać dodatkowe segmenty i parametrów do strony trasy.</span><span class="sxs-lookup"><span data-stu-id="12b6f-321">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="12b6f-322">Niezależnie od istnieje już ma **dołączany** do trasy domyślnej strony.</span><span class="sxs-lookup"><span data-stu-id="12b6f-322">Whatever's there's **appended** to the default route of the page.</span></span> <span data-ttu-id="12b6f-323">Zmiana trasy strony przy użyciu ścieżki bezwzględnej lub wirtualnych (takie jak `"~/Some/Other/Path"`) nie jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="12b6f-323">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) isn't supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="12b6f-324">Konfiguracja i ustawienia</span><span class="sxs-lookup"><span data-stu-id="12b6f-324">Configuration and settings</span></span>

<span data-ttu-id="12b6f-325">Aby skonfigurować opcje zaawansowane, użyj metody rozszerzenia `AddRazorPagesOptions` w Konstruktorze MVC:</span><span class="sxs-lookup"><span data-stu-id="12b6f-325">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="12b6f-326">Obecnie można użyć `RazorPagesOptions` Ustaw katalog główny strony lub Dodaj konwencje modelu aplikacji dla stron.</span><span class="sxs-lookup"><span data-stu-id="12b6f-326">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="12b6f-327">Firma Microsoft będzie włączyć więcej rozszerzeń w ten sposób w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="12b6f-327">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="12b6f-328">Wstępnej kompilacji widoków, zobacz [kompilacji widoku Razor](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="12b6f-328">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="12b6f-329">[Pobrania lub wyświetlenia przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="12b6f-329">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="12b6f-330">Zobacz [wprowadzenie stron Razor](xref:tutorials/razor-pages/razor-pages-start), która opiera się na to wprowadzenie.</span><span class="sxs-lookup"><span data-stu-id="12b6f-330">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="12b6f-331">Określ, czy stron Razor zawartości katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="12b6f-331">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="12b6f-332">Domyślnie stron Razor są umieszczone w */strony* katalogu.</span><span class="sxs-lookup"><span data-stu-id="12b6f-332">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="12b6f-333">Dodaj [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) stron Razor są zawartości katalogu głównego ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) aplikacji:</span><span class="sxs-lookup"><span data-stu-id="12b6f-333">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="12b6f-334">Określ, czy w katalogu głównym niestandardowych stron Razor</span><span class="sxs-lookup"><span data-stu-id="12b6f-334">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="12b6f-335">Dodaj [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) do określenia, czy stron Razor w katalogu głównym niestandardowych w aplikacji (podaj ścieżkę względną):</span><span class="sxs-lookup"><span data-stu-id="12b6f-335">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="12b6f-336">Zobacz także</span><span class="sxs-lookup"><span data-stu-id="12b6f-336">See also</span></span>

* [<span data-ttu-id="12b6f-337">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="12b6f-337">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="12b6f-338">Składnia Razor</span><span class="sxs-lookup"><span data-stu-id="12b6f-338">Razor syntax</span></span>](xref:mvc/views/razor)
* [<span data-ttu-id="12b6f-339">Wprowadzenie do korzystania ze stron Razor</span><span class="sxs-lookup"><span data-stu-id="12b6f-339">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="12b6f-340">Konwencje autoryzacji stron razor</span><span class="sxs-lookup"><span data-stu-id="12b6f-340">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="12b6f-341">Razor strony trasy i strony modelu dostawców niestandardowych</span><span class="sxs-lookup"><span data-stu-id="12b6f-341">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="12b6f-342">Testy jednostkowe i integracja z stron razor</span><span class="sxs-lookup"><span data-stu-id="12b6f-342">Razor Pages unit and integration tests</span></span>](xref:testing/razor-pages-testing)
