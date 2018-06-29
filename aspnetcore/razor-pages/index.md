---
title: Wprowadzenie do platformy ASP.NET Core stron Razor
author: Rick-Anderson
description: Dowiedz się, jak Razor strony platformy ASP.NET Core umożliwia kodowania scenariusze strony łatwiejsze i bardziej wydajnej pracy niż przy użyciu platformy MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/12/2018
uid: razor-pages/index
ms.openlocfilehash: 9d7d4d49dbb55e327a208df99a0e3ca744de8609
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077751"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="278ae-103">Wprowadzenie do platformy ASP.NET Core stron Razor</span><span class="sxs-lookup"><span data-stu-id="278ae-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="278ae-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="278ae-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="278ae-105">Stron razor jest elementem nowe platformy ASP.NET Core MVC umożliwia kodowanie strony scenariusze łatwiejsze i bardziej wydajnej pracy.</span><span class="sxs-lookup"><span data-stu-id="278ae-105">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="278ae-106">Jeśli szukasz samouczka, który korzysta z podejścia Model-View-Controller, zobacz [Rozpoczynanie pracy z platformą ASP.NET MVC Core](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="278ae-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="278ae-107">Ten dokument zawiera wprowadzenie do stron Razor.</span><span class="sxs-lookup"><span data-stu-id="278ae-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="278ae-108">Nie jest samouczek krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="278ae-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="278ae-109">Jeśli możesz znaleźć sekcje zbyt zaawansowanych, zobacz [wprowadzenie stron Razor](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="278ae-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="278ae-110">Omówienie platformy ASP.NET Core, zobacz [wprowadzenie do platformy ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="278ae-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="278ae-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="278ae-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="278ae-112">Tworzenie projektu stron Razor</span><span class="sxs-lookup"><span data-stu-id="278ae-112">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="278ae-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="278ae-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="278ae-114">Zobacz [wprowadzenie stron Razor](xref:tutorials/razor-pages/razor-pages-start) szczegółowe informacje dotyczące sposobu tworzenia stron Razor projektu za pomocą programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="278ae-114">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="278ae-115">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="278ae-115">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="278ae-116">Uruchom `dotnet new webapp` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="278ae-116">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="278ae-117">Uruchom `dotnet new razor` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="278ae-117">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

<span data-ttu-id="278ae-118">Otwórz wygenerowany *.csproj* plików z programu Visual Studio dla komputerów Mac.</span><span class="sxs-lookup"><span data-stu-id="278ae-118">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="278ae-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="278ae-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="278ae-120">Uruchom `dotnet new webapp` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="278ae-120">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="278ae-121">Uruchom `dotnet new razor` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="278ae-121">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="278ae-122">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="278ae-122">.NET Core CLI</span></span>](#tab/netcore-cli)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="278ae-123">Uruchom `dotnet new webapp` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="278ae-123">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="278ae-124">Uruchom `dotnet new razor` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="278ae-124">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

---

## <a name="razor-pages"></a><span data-ttu-id="278ae-125">Stron razor</span><span class="sxs-lookup"><span data-stu-id="278ae-125">Razor Pages</span></span>

<span data-ttu-id="278ae-126">Stron razor jest włączone w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="278ae-126">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="278ae-127">Należy wziąć pod uwagę strony podstawowej: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="278ae-127">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="278ae-128">Poprzedni kod znacznie wygląda jak plik widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="278ae-128">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="278ae-129">Jest to, co ułatwia różnych `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="278ae-129">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="278ae-130">`@page` powoduje, że plik na akcję MVC — czyli obsługi żądań bezpośrednio, bez przechodzenia przez kontroler.</span><span class="sxs-lookup"><span data-stu-id="278ae-130">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="278ae-131">`@page` musi być pierwszym dyrektywy Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="278ae-131">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="278ae-132">`@page` wpływa na działanie innych konstrukcji Razor.</span><span class="sxs-lookup"><span data-stu-id="278ae-132">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="278ae-133">Podobne strony przy użyciu `PageModel` klasa, jest wyświetlany w obszarze następujące dwa pliki.</span><span class="sxs-lookup"><span data-stu-id="278ae-133">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="278ae-134">*Pages/Index2.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="278ae-134">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="278ae-135">*Pages/Index2.cshtml.cs* modelu strony:</span><span class="sxs-lookup"><span data-stu-id="278ae-135">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="278ae-136">Według konwencji `PageModel` pliku klasy ma taką samą nazwę jak plik Razor strony z *.cs* dołączane.</span><span class="sxs-lookup"><span data-stu-id="278ae-136">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="278ae-137">Na przykład na poprzedniej stronie aparatu Razor jest *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="278ae-137">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="278ae-138">Plik zawierający `PageModel` nosi nazwę klasy *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="278ae-138">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="278ae-139">Skojarzenia ścieżki adresu URL do strony zależą od lokalizacji strony w systemie plików.</span><span class="sxs-lookup"><span data-stu-id="278ae-139">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="278ae-140">W poniższej tabeli przedstawiono ścieżki Razor strony i dopasowywania adresu URL:</span><span class="sxs-lookup"><span data-stu-id="278ae-140">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="278ae-141">Nazwa i ścieżka pliku</span><span class="sxs-lookup"><span data-stu-id="278ae-141">File name and path</span></span>               | <span data-ttu-id="278ae-142">Dopasowywanie adresu URL</span><span class="sxs-lookup"><span data-stu-id="278ae-142">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="278ae-143">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="278ae-143">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="278ae-144">`/` lub `/Index`</span><span class="sxs-lookup"><span data-stu-id="278ae-144">`/` or `/Index`</span></span> |
| <span data-ttu-id="278ae-145">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="278ae-145">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="278ae-146">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="278ae-146">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="278ae-147">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="278ae-147">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="278ae-148">`/Store` lub `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="278ae-148">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="278ae-149">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="278ae-149">Notes:</span></span>

* <span data-ttu-id="278ae-150">Środowisko uruchomieniowe wyszukuje pliki stron Razor w *stron* folderu domyślnie.</span><span class="sxs-lookup"><span data-stu-id="278ae-150">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="278ae-151">`Index` jest domyślną stronę, gdy adres URL nie zawiera strony.</span><span class="sxs-lookup"><span data-stu-id="278ae-151">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="278ae-152">Zapisywanie formularza podstawowego</span><span class="sxs-lookup"><span data-stu-id="278ae-152">Writing a basic form</span></span>

<span data-ttu-id="278ae-153">Uniemożliwia stron razor typowe wzorce używane w przeglądarkach łatwa do wdrożenia podczas kompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="278ae-153">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="278ae-154">[Model powiązania](xref:mvc/models/model-binding), [pomocników tagów](xref:mvc/views/tag-helpers/intro)i wszystkich pomocników HTML *tylko pracy* z właściwościami zdefiniowana w klasie Razor strony.</span><span class="sxs-lookup"><span data-stu-id="278ae-154">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="278ae-155">Należy wziąć pod uwagę strona, która implementuje podstawowego "Skontaktuj się z nami" tworzą dla `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="278ae-155">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="278ae-156">Aby wyświetlić przykłady w tym dokumencie `DbContext` został zainicjowany w [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) pliku.</span><span class="sxs-lookup"><span data-stu-id="278ae-156">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="278ae-157">Model danych:</span><span class="sxs-lookup"><span data-stu-id="278ae-157">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="278ae-158">Kontekst bazy danych:</span><span class="sxs-lookup"><span data-stu-id="278ae-158">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="278ae-159">*Pages/Create.cshtml* pliku widoku:</span><span class="sxs-lookup"><span data-stu-id="278ae-159">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="278ae-160">*Pages/Create.cshtml.cs* modelu strony:</span><span class="sxs-lookup"><span data-stu-id="278ae-160">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="278ae-161">Według konwencji `PageModel` nosi nazwę klasy `<PageName>Model` i znajduje się w tej samej przestrzeni nazw jako strony.</span><span class="sxs-lookup"><span data-stu-id="278ae-161">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="278ae-162">`PageModel` Klasa umożliwia oddzielenie logiki strony z jego prezentacji.</span><span class="sxs-lookup"><span data-stu-id="278ae-162">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="278ae-163">Definiuje stronę obsługi żądania wysyłane na stronie i dane używane do renderowania strony.</span><span class="sxs-lookup"><span data-stu-id="278ae-163">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="278ae-164">Ta separacja umożliwia zarządzanie zależności strony za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection) i [testu jednostkowego](xref:test/razor-pages-tests) stron.</span><span class="sxs-lookup"><span data-stu-id="278ae-164">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="278ae-165">Na stronie `OnPostAsync` *metoda obsługi*, która działa w `POST` żąda (gdy użytkownik zapisuje formularz).</span><span class="sxs-lookup"><span data-stu-id="278ae-165">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="278ae-166">Można dodać obsługi metod dla dowolnej zlecenie HTTP.</span><span class="sxs-lookup"><span data-stu-id="278ae-166">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="278ae-167">Są najczęściej programów obsługi:</span><span class="sxs-lookup"><span data-stu-id="278ae-167">The most common handlers are:</span></span>

* <span data-ttu-id="278ae-168">`OnGet` Aby zainicjować stanu potrzebne dla strony.</span><span class="sxs-lookup"><span data-stu-id="278ae-168">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="278ae-169">[OnGet](#OnGet) próbki.</span><span class="sxs-lookup"><span data-stu-id="278ae-169">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="278ae-170">`OnPost` do obsługi przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="278ae-170">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="278ae-171">`Async` Sufiks nazwy jest opcjonalne, ale często używane przez Konwencję dla funkcji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="278ae-171">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="278ae-172">`OnPostAsync` Kodu w poprzednim przykładzie wygląda podobnie do co zwykle piszesz w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="278ae-172">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="278ae-173">Poprzedni kod jest typowe dla stron Razor.</span><span class="sxs-lookup"><span data-stu-id="278ae-173">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="278ae-174">Większość podstawowych MVC, takich jak [modelu powiązania](xref:mvc/models/model-binding), [weryfikacji](xref:mvc/models/validation), i są współdzielone wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="278ae-174">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="278ae-175">Poprzedni `OnPostAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="278ae-175">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="278ae-176">Podstawowy przepływ `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="278ae-176">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="278ae-177">Sprawdź błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="278ae-177">Check for validation errors.</span></span>

*  <span data-ttu-id="278ae-178">Jeśli nie ma żadnych błędów, Zapisz dane i przekierowanie.</span><span class="sxs-lookup"><span data-stu-id="278ae-178">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="278ae-179">Jeśli wystąpią błędy, Wyświetl stronę ponownie, podając komunikatów dotyczących sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="278ae-179">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="278ae-180">Weryfikacja po stronie klienta jest identyczna jak tradycyjne aplikacje platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="278ae-180">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="278ae-181">W wielu przypadkach błędy sprawdzania poprawności może być wykryte na komputerze klienckim i nigdy nie zostały przekazane do serwera.</span><span class="sxs-lookup"><span data-stu-id="278ae-181">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="278ae-182">Po pomyślnym wprowadzeniu danych `OnPostAsync` wywołań metody obsługi `RedirectToPage` metody pomocnika do zwrócenia wystąpienia klasy `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="278ae-182">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="278ae-183">`RedirectToPage` jest nowy wynik akcji, podobnie jak `RedirectToAction` lub `RedirectToRoute`, ale dostosowanych stron.</span><span class="sxs-lookup"><span data-stu-id="278ae-183">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="278ae-184">W poprzednim przykładzie przekierowuje do strony indeksu głównego (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="278ae-184">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="278ae-185">`RedirectToPage` została szczegółowo opisana w [generowania adresu URL dla stron](#url_gen) sekcji.</span><span class="sxs-lookup"><span data-stu-id="278ae-185">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="278ae-186">Jeśli przesłanego formularza zawiera błędy sprawdzania poprawności (które są przekazywane do serwera),`OnPostAsync` wywołań metody obsługi `Page` metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="278ae-186">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="278ae-187">`Page` Zwraca wystąpienie klasy `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="278ae-187">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="278ae-188">Zwracanie `Page` jest podobny do sposób zwrócenia akcje w kontrolerach `View`.</span><span class="sxs-lookup"><span data-stu-id="278ae-188">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="278ae-189">`PageResult` Wartość domyślna to <!-- Review  --> zwracany typ metody obsługi.</span><span class="sxs-lookup"><span data-stu-id="278ae-189">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="278ae-190">Metoda obsługi, która zwraca `void` renderuje stronę.</span><span class="sxs-lookup"><span data-stu-id="278ae-190">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="278ae-191">`Customer` Używa właściwości `[BindProperty]` atrybutu korzystania z wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="278ae-191">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="278ae-192">Stron razor domyślnie powiązania właściwości tylko z innych niż GET zleceń.</span><span class="sxs-lookup"><span data-stu-id="278ae-192">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="278ae-193">Powiązanie właściwości może zmniejszyć ilość kodu, które trzeba zapisać.</span><span class="sxs-lookup"><span data-stu-id="278ae-193">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="278ae-194">Powiązanie zmniejsza kodu przy użyciu tej samej właściwości do renderowania pól formularza (`<input asp-for="Customer.Name" />`) i akceptuje dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="278ae-194">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="278ae-195">Ze względów bezpieczeństwa należy zgadzaj się na wiązanie danych żądania GET do strony właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="278ae-195">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="278ae-196">Sprawdź dane wejściowe użytkownika przed zamapowaniem ją do właściwości.</span><span class="sxs-lookup"><span data-stu-id="278ae-196">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="278ae-197">Zgody na korzystanie z to zachowanie jest przydatne, gdy adresowania scenariusze, które zależą od wartości ciągu lub trasy kwerendy.</span><span class="sxs-lookup"><span data-stu-id="278ae-197">Opting in to this behavior is useful when addressing scenarios which rely on query string or route values.</span></span>
>
> <span data-ttu-id="278ae-198">Aby powiązać właściwość na żądania GET, ustaw `[BindProperty]` atrybutu `SupportsGet` właściwości `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="278ae-198">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="278ae-199">Strona główna (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="278ae-199">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="278ae-200">Skojarzony `PageModel` klasy (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="278ae-200">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="278ae-201">*Index.cshtml* pliku zawiera następujące znaczniki, aby utworzyć link edycji dla każdego kontakt:</span><span class="sxs-lookup"><span data-stu-id="278ae-201">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="278ae-202">[Pomocnika Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) używane `asp-route-{value}` atrybut do generowania łącza do edycji strony.</span><span class="sxs-lookup"><span data-stu-id="278ae-202">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="278ae-203">Ten link zawiera dane trasy, o kontakt identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="278ae-203">The link contains route data with the contact ID.</span></span> <span data-ttu-id="278ae-204">Na przykład `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="278ae-204">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="278ae-205">*Pages/Edit.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="278ae-205">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="278ae-206">Pierwszy wiersz zawiera `@page "{id:int}"` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="278ae-206">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="278ae-207">Ograniczenie routingu`"{id:int}"` informuje strony do akceptowania żądań do strony, które zawierają `int` danych trasy.</span><span class="sxs-lookup"><span data-stu-id="278ae-207">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="278ae-208">Jeśli żądanie do strony nie zawiera danych trasy, który może zostać przekonwertowany na `int`, środowisko uruchomieniowe zwraca błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="278ae-208">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="278ae-209">Aby wprowadzić identyfikator opcjonalne, dołącz `?` ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="278ae-209">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="278ae-210">*Pages/Edit.cshtml.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="278ae-210">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="278ae-211">*Index.cshtml* plik zawiera także znaczników do utworzenia przycisk Usuń widoczny dla każdego kontakt klienta:</span><span class="sxs-lookup"><span data-stu-id="278ae-211">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="278ae-212">Przycisk usuwania jest renderowany w języku HTML, jego `formaction` zawiera parametry:</span><span class="sxs-lookup"><span data-stu-id="278ae-212">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="278ae-213">Określony przez identyfikator kontaktu klienta z `asp-route-id` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="278ae-213">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="278ae-214">`handler` Określonego przez `asp-page-handler` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="278ae-214">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="278ae-215">Poniżej przedstawiono przykładowy przycisk usuwania renderowany z klientem skontaktuj się z Identyfikatorem `1`:</span><span class="sxs-lookup"><span data-stu-id="278ae-215">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="278ae-216">Jeśli przycisk jest zaznaczony, formularz `POST` wysłaniu żądania do serwera.</span><span class="sxs-lookup"><span data-stu-id="278ae-216">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="278ae-217">Według Konwencji wybrano nazwę metody obsługi na podstawie wartości `handler` parametru zgodnie ze schematem `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="278ae-217">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="278ae-218">Ponieważ `handler` jest `delete` w tym przykładzie `OnPostDeleteAsync` metoda obsługi jest używany do procesu `POST` żądania.</span><span class="sxs-lookup"><span data-stu-id="278ae-218">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="278ae-219">Jeśli `asp-page-handler` ustawiono innej wartości, takich jak `remove`, strona metody obsługi o nazwie `OnPostRemoveAsync` jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="278ae-219">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="278ae-220">`OnPostDeleteAsync` Metody:</span><span class="sxs-lookup"><span data-stu-id="278ae-220">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="278ae-221">Akceptuje `id` z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="278ae-221">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="278ae-222">Wysyła zapytanie do bazy danych dla klienta kontakt z `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="278ae-222">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="278ae-223">Jeśli zostanie znaleziony kontaktowe klienta, są one usunięte z listy kontaktów klienta.</span><span class="sxs-lookup"><span data-stu-id="278ae-223">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="278ae-224">Baza danych jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="278ae-224">The database is updated.</span></span>
* <span data-ttu-id="278ae-225">Wywołania `RedirectToPage` ma nastąpić przekierowanie do strony indeksu głównego (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="278ae-225">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-required"></a><span data-ttu-id="278ae-226">Wymagane właściwości strony znaku</span><span class="sxs-lookup"><span data-stu-id="278ae-226">Mark page properties required</span></span>

<span data-ttu-id="278ae-227">Właściwości `PageModel` może korzystać z [wymagane](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) atrybutu:</span><span class="sxs-lookup"><span data-stu-id="278ae-227">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="278ae-228">Zobacz [modelu weryfikacji](xref:mvc/models/validation) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="278ae-228">See [Model validation](xref:mvc/models/validation) for more information.</span></span>

## <a name="manage-head-requests-with-the-onget-handler"></a><span data-ttu-id="278ae-229">Zarządzaj żądaniami HEAD z obsługą OnGet</span><span class="sxs-lookup"><span data-stu-id="278ae-229">Manage HEAD requests with the OnGet handler</span></span>

<span data-ttu-id="278ae-230">Zwykle HEAD program obsługi jest tworzony i wywołana dla żądania HEAD:</span><span class="sxs-lookup"><span data-stu-id="278ae-230">Ordinarily, a HEAD handler is created and called for HEAD requests:</span></span>

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="278ae-231">Jeśli bez obsługi HEAD (`OnHead`) jest zdefiniowany, stron Razor nastąpi powrót do wywoływania obsługi strony GET (`OnGet`) platformy ASP.NET Core 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="278ae-231">If no HEAD handler (`OnHead`) is defined, Razor Pages falls back to calling the GET page handler (`OnGet`) in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="278ae-232">Zgódź się na zachowanie w przypadku [metody SetCompatibilityVersion](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) w `Startup.Configure` dla platformy ASP.NET Core 2.1 2.x:</span><span class="sxs-lookup"><span data-stu-id="278ae-232">Opt in to this behavior with the [SetCompatibilityVersion method](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) in `Startup.Configure` for ASP.NET Core 2.1 to 2.x:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="278ae-233">`SetCompatibilityVersion` efektywnie ustawia opcję stron Razor `AllowMappingHeadRequestsToGetHandler` do `true`.</span><span class="sxs-lookup"><span data-stu-id="278ae-233">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="278ae-234">Zamiast Rezygnacja do wszystkich zachowań 2.1 z `SetCompatibilityVersion`, użytkownik może jawnie wyrazić zgodę na konkretne zachowania.</span><span class="sxs-lookup"><span data-stu-id="278ae-234">Rather than opting into all 2.1 behaviors with `SetCompatibilityVersion`, you can explicitly opt-in to specific behaviors.</span></span> <span data-ttu-id="278ae-235">Poniższy kod wybranych żądań HEAD mapowanie do programu obsługi pobierania.</span><span class="sxs-lookup"><span data-stu-id="278ae-235">The following code opts into the mapping HEAD requests to the GET handler.</span></span>


```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```
::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="278ae-236">XSRF/CSRF i stron Razor</span><span class="sxs-lookup"><span data-stu-id="278ae-236">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="278ae-237">Nie masz do pisania kodu dla [antiforgery weryfikacji](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="278ae-237">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="278ae-238">Antiforgery generowania tokenów i weryfikacja są automatycznie umieszczane w stron Razor.</span><span class="sxs-lookup"><span data-stu-id="278ae-238">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="278ae-239">Za pomocą układów, częściowe, szablonów i pomocników tagów Razor strony</span><span class="sxs-lookup"><span data-stu-id="278ae-239">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="278ae-240">Strony pracować ze wszystkimi funkcjami systemu aparatu widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="278ae-240">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="278ae-241">Układy, częściowe, szablony, pomocników tagów *_ViewStart.cshtml*, *_ViewImports.cshtml* pracy w taki sam sposób jak w przypadku konwencjonalnych widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="278ae-241">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="278ae-242">Dzięki wykorzystaniu niektóre z tych funkcji umożliwia declutter tej strony.</span><span class="sxs-lookup"><span data-stu-id="278ae-242">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="278ae-243">Dodaj [układ strony](xref:mvc/views/layout) do *Pages/Shared/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="278ae-243">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="278ae-244">Dodaj [układ strony](xref:mvc/views/layout) do *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="278ae-244">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

::: moniker-end

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="278ae-245">[Układu](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="278ae-245">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="278ae-246">Określa układ każdej strony (o ile nie zdecyduje strony poza układ).</span><span class="sxs-lookup"><span data-stu-id="278ae-246">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="278ae-247">Importuje struktury HTML, takich jak JavaScript i arkusze stylów.</span><span class="sxs-lookup"><span data-stu-id="278ae-247">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="278ae-248">Zobacz [układ strony](xref:mvc/views/layout) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="278ae-248">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="278ae-249">[Układu](xref:mvc/views/layout#specifying-a-layout) właściwość jest ustawiona *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="278ae-249">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="278ae-250">Układ jest *strony/udostępnione* folderu.</span><span class="sxs-lookup"><span data-stu-id="278ae-250">The layout is in the *Shared/Pages* folder.</span></span> <span data-ttu-id="278ae-251">Strony wyszukać innych widoków (układy, szablony, częściowe) hierarchicznie, uruchamianie w tym samym folderze co bieżąca strona.</span><span class="sxs-lookup"><span data-stu-id="278ae-251">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="278ae-252">Układ w *strony/udostępnione* folderu można używać z dowolnej strony Razor, w obszarze *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="278ae-252">A layout in the *Shared/Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="278ae-253">Plik układu powinien znajdować się *stron/Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="278ae-253">The layout file should go in the *Pages/Shared* folder.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="278ae-254">Układ jest *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="278ae-254">The layout is in the *Pages* folder.</span></span> <span data-ttu-id="278ae-255">Strony wyszukać innych widoków (układy, szablony, częściowe) hierarchicznie, uruchamianie w tym samym folderze co bieżąca strona.</span><span class="sxs-lookup"><span data-stu-id="278ae-255">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="278ae-256">Układ w *stron* folderu można używać z dowolnej strony Razor, w obszarze *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="278ae-256">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

::: moniker-end

<span data-ttu-id="278ae-257">Firma Microsoft zaleca **nie** umieścić plik układu w *widoków/Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="278ae-257">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="278ae-258">*Widoki/Shared* jest wzorzec widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="278ae-258">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="278ae-259">Stron razor są przeznaczone do zależą od hierarchii folderów, nie ścieżkę Konwencji.</span><span class="sxs-lookup"><span data-stu-id="278ae-259">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="278ae-260">Widok wyszukiwania ze strony Razor obejmuje *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="278ae-260">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="278ae-261">Układy, szablonów i częściowe jest używany z konwencjonalnej Razor widoków i kontrolerów MVC *tylko pracy*.</span><span class="sxs-lookup"><span data-stu-id="278ae-261">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="278ae-262">Dodaj *Pages/_ViewImports.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="278ae-262">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="278ae-263">`@namespace` znajduje się w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="278ae-263">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="278ae-264">`@addTagHelper` Dyrektywy dostarcza [wbudowanych pomocników tagów](xref:mvc/views/tag-helpers/builtin-th/Index) dla wszystkich stron w *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="278ae-264">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="278ae-265">Gdy `@namespace` dyrektywa jest używana jawnie na stronie:</span><span class="sxs-lookup"><span data-stu-id="278ae-265">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="278ae-266">Dyrektywa ustawia obszar nazw dla strony.</span><span class="sxs-lookup"><span data-stu-id="278ae-266">The directive sets the namespace for the page.</span></span> <span data-ttu-id="278ae-267">`@model` Dyrektywy nie musi obejmować przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="278ae-267">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="278ae-268">Gdy `@namespace` dyrektywy znajduje się w *_ViewImports.cshtml*, określonego obszaru nazw dostarcza prefiksu dla przestrzeni nazw wygenerowane na stronie, który importuje `@namespace` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="278ae-268">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="278ae-269">Pozostała część wygenerowanej (część sufiks) jest oddzielona kropkami ścieżki względnej między folder zawierający *_ViewImports.cshtml* i folderu zawierającego strony.</span><span class="sxs-lookup"><span data-stu-id="278ae-269">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="278ae-270">Na przykład `PageModel` klasy *Pages/Customers/Edit.cshtml.cs* jawnie Ustawia obszar nazw:</span><span class="sxs-lookup"><span data-stu-id="278ae-270">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="278ae-271">*Pages/_ViewImports.cshtml* pliku ustawia następujących nazw:</span><span class="sxs-lookup"><span data-stu-id="278ae-271">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="278ae-272">Wygenerowany obszar nazw dla *Pages/Customers/Edit.cshtml* Razor strony jest taka sama jak `PageModel` klasy.</span><span class="sxs-lookup"><span data-stu-id="278ae-272">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="278ae-273">`@namespace` *współdziała również z konwencjonalnej widokami Razor.*</span><span class="sxs-lookup"><span data-stu-id="278ae-273">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="278ae-274">Oryginalna *Pages/Create.cshtml* pliku widoku:</span><span class="sxs-lookup"><span data-stu-id="278ae-274">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="278ae-275">Zaktualizowany interfejs *Pages/Create.cshtml* pliku widoku:</span><span class="sxs-lookup"><span data-stu-id="278ae-275">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="278ae-276">[Projektu starter stron Razor](#rpvs17) zawiera *Pages/_ValidationScriptsPartial.cshtml*, który przechwytuje sprawdzania poprawności po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="278ae-276">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="278ae-277">Generowania adresu URL dla stron</span><span class="sxs-lookup"><span data-stu-id="278ae-277">URL generation for Pages</span></span>

<span data-ttu-id="278ae-278">`Create` Strony pokazana wcześniej, używa `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="278ae-278">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="278ae-279">Aplikacja ma następującą strukturę plik lub folder:</span><span class="sxs-lookup"><span data-stu-id="278ae-279">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="278ae-280">*/ Stron*</span><span class="sxs-lookup"><span data-stu-id="278ae-280">*/Pages*</span></span>

  * <span data-ttu-id="278ae-281">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="278ae-281">*Index.cshtml*</span></span>
  * <span data-ttu-id="278ae-282">*/ Klientów*</span><span class="sxs-lookup"><span data-stu-id="278ae-282">*/Customers*</span></span>

    * <span data-ttu-id="278ae-283">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="278ae-283">*Create.cshtml*</span></span>
    * <span data-ttu-id="278ae-284">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="278ae-284">*Edit.cshtml*</span></span>
    * <span data-ttu-id="278ae-285">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="278ae-285">*Index.cshtml*</span></span>

<span data-ttu-id="278ae-286">*Pages/Customers/Create.cshtml* i *Pages/Customers/Edit.cshtml* stron przekierowania do *Pages/Index.cshtml* po pomyślnym.</span><span class="sxs-lookup"><span data-stu-id="278ae-286">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="278ae-287">Ciąg `/Index` jest częścią identyfikatora URI uzyskać dostęp do poprzedniej strony.</span><span class="sxs-lookup"><span data-stu-id="278ae-287">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="278ae-288">Ciąg `/Index` może służyć do generowania identyfikatorów URI *Pages/Index.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="278ae-288">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="278ae-289">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="278ae-289">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="278ae-290">Nazwa strony jest ścieżka do strony z katalogu głównego */strony* folderu, w tym na początku `/` (na przykład `/Index`).</span><span class="sxs-lookup"><span data-stu-id="278ae-290">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="278ae-291">Poprzedniej próbki generowania adresu URL oferują rozszerzoną opcje i funkcji za pośrednictwem hardcoding adresu URL.</span><span class="sxs-lookup"><span data-stu-id="278ae-291">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="278ae-292">Używa generowania adresu URL [routingu](xref:mvc/controllers/routing) i generowanie i kodowanie parametry zgodnie z sposób definiowania trasy w ścieżce docelowej.</span><span class="sxs-lookup"><span data-stu-id="278ae-292">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="278ae-293">Generowania adresu URL dla stron obsługuje nazw względnych.</span><span class="sxs-lookup"><span data-stu-id="278ae-293">URL generation for pages supports relative names.</span></span> <span data-ttu-id="278ae-294">W poniższej tabeli przedstawiono stronę indeksu, która jest zaznaczone z różnych `RedirectToPage` parametry z *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="278ae-294">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="278ae-295">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="278ae-295">RedirectToPage(x)</span></span>| <span data-ttu-id="278ae-296">Strona</span><span class="sxs-lookup"><span data-stu-id="278ae-296">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="278ae-297">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="278ae-297">RedirectToPage("/Index")</span></span> | <span data-ttu-id="278ae-298">*Strony/indeksu*</span><span class="sxs-lookup"><span data-stu-id="278ae-298">*Pages/Index*</span></span> |
| <span data-ttu-id="278ae-299">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="278ae-299">RedirectToPage("./Index");</span></span> | <span data-ttu-id="278ae-300">*Strony/klientów/indeksu*</span><span class="sxs-lookup"><span data-stu-id="278ae-300">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="278ae-301">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="278ae-301">RedirectToPage("../Index")</span></span> | <span data-ttu-id="278ae-302">*Strony/indeksu*</span><span class="sxs-lookup"><span data-stu-id="278ae-302">*Pages/Index*</span></span> |
| <span data-ttu-id="278ae-303">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="278ae-303">RedirectToPage("Index")</span></span>  | <span data-ttu-id="278ae-304">*Strony/klientów/indeksu*</span><span class="sxs-lookup"><span data-stu-id="278ae-304">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="278ae-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, i `RedirectToPage("../Index")` są <em>względne nazwy</em>.</span><span class="sxs-lookup"><span data-stu-id="278ae-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are <em>relative names</em>.</span></span> <span data-ttu-id="278ae-306">`RedirectToPage` Parametr jest <em>łączyć</em> ze ścieżką bieżącej strony do obliczenia nazwę strony docelowej.</span><span class="sxs-lookup"><span data-stu-id="278ae-306">The `RedirectToPage` parameter is <em>combined</em> with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="278ae-307">Nazwa względna konsolidacji jest przydatne, gdy tworzenie witryn ze strukturą złożonych.</span><span class="sxs-lookup"><span data-stu-id="278ae-307">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="278ae-308">Jeśli używasz nazwy względne do połączenia między stronami w folderze, można zmienić nazwę tego folderu.</span><span class="sxs-lookup"><span data-stu-id="278ae-308">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="278ae-309">Wszystkie linki nadal działać (ponieważ one nie obejmować nazwę folderu).</span><span class="sxs-lookup"><span data-stu-id="278ae-309">All the links still work (because they didn't include the folder name).</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="viewdata-attribute"></a><span data-ttu-id="278ae-310">Atrybut viewData</span><span class="sxs-lookup"><span data-stu-id="278ae-310">ViewData attribute</span></span>

<span data-ttu-id="278ae-311">Dane mogą być przekazywane do strony z [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="278ae-311">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="278ae-312">Właściwości w kontrolerach ani w modelach Razor strony ozdobione `[ViewData]` ich wartości przechowywane i załadowane z [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="278ae-312">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="278ae-313">W poniższym przykładzie `AboutModel` zawiera `Title` ozdobione właściwości `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="278ae-313">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="278ae-314">`Title` Właściwość jest ustawiona na tytuł strony informacje:</span><span class="sxs-lookup"><span data-stu-id="278ae-314">The `Title` property is set to the title of the About page:</span></span>

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

<span data-ttu-id="278ae-315">Na stronie informacje dostępu `Title` właściwości jako właściwość modelu:</span><span class="sxs-lookup"><span data-stu-id="278ae-315">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="278ae-316">W układzie tytuł jest do odczytu ze słownika ViewData:</span><span class="sxs-lookup"><span data-stu-id="278ae-316">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```
::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="278ae-317">TempData</span><span class="sxs-lookup"><span data-stu-id="278ae-317">TempData</span></span>

<span data-ttu-id="278ae-318">Przedstawia platformy ASP.NET Core [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) właściwość [kontrolera](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="278ae-318">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="278ae-319">Ta właściwość przechowuje dane, dopóki nie jest do odczytu.</span><span class="sxs-lookup"><span data-stu-id="278ae-319">This property stores data until it's read.</span></span> <span data-ttu-id="278ae-320">`Keep` i `Peek` metod można użyć do sprawdzenia danych bez usuwania.</span><span class="sxs-lookup"><span data-stu-id="278ae-320">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="278ae-321">`TempData` jest przydatne w przypadku przekierowania, gdy dane są potrzebne dla więcej niż jednego żądania.</span><span class="sxs-lookup"><span data-stu-id="278ae-321">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="278ae-322">`[TempData]` Jest nowy w programie ASP.NET 2.0 Core i jest obsługiwane na stronach i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="278ae-322">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="278ae-323">Poniższy kod ustawia wartość `Message` przy użyciu `TempData`:</span><span class="sxs-lookup"><span data-stu-id="278ae-323">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="278ae-324">Następujący kod w *Pages/Customers/Index.cshtml* plik zawiera wartość `Message` przy użyciu `TempData`.</span><span class="sxs-lookup"><span data-stu-id="278ae-324">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="278ae-325">*Pages/Customers/Index.cshtml.cs* stosuje modelu strony `[TempData]` atrybutu `Message` właściwości.</span><span class="sxs-lookup"><span data-stu-id="278ae-325">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="278ae-326">Zobacz [TempData](xref:fundamentals/app-state#tempdata) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="278ae-326">See [TempData](xref:fundamentals/app-state#tempdata) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="278ae-327">Wielu obsług na stronie</span><span class="sxs-lookup"><span data-stu-id="278ae-327">Multiple handlers per page</span></span>

<span data-ttu-id="278ae-328">Następująca strona generuje kod znaczników dla dwie strony przy użyciu programów obsługi `asp-page-handler` pomocnika tagów:</span><span class="sxs-lookup"><span data-stu-id="278ae-328">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="278ae-329">Formularz w poprzednim przykładzie ma dwa przesłać przyciski, za pomocą każdej `FormActionTagHelper` do przesłania do innego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="278ae-329">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="278ae-330">`asp-page-handler` Atrybut jest dodatek do `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="278ae-330">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="278ae-331">`asp-page-handler` generuje adresów URL, które przesłania do każdej z metod obsługi zdefiniowane przez stronę.</span><span class="sxs-lookup"><span data-stu-id="278ae-331">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="278ae-332">`asp-page` nie jest określony, ponieważ próbki jest konsolidacja do bieżącej strony.</span><span class="sxs-lookup"><span data-stu-id="278ae-332">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="278ae-333">Model strony:</span><span class="sxs-lookup"><span data-stu-id="278ae-333">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="278ae-334">W poprzednim kodzie użyto *o nazwie metod obsługi*.</span><span class="sxs-lookup"><span data-stu-id="278ae-334">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="278ae-335">Metody o nazwie procedury obsługi są tworzone przez pobieranie tekstu w nazwie po `On<HTTP Verb>` i przed `Async` (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="278ae-335">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="278ae-336">W powyższym przykładzie metody strony są OnPost**JoinList**Async i OnPost**JoinListUC**asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="278ae-336">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="278ae-337">Z *OnPost* i *Async* usunięta, nazwy programu obsługi są `JoinList` i `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="278ae-337">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="278ae-338">Przy użyciu poprzedniego kodu ścieżkę adresu URL, który przesyła do `OnPostJoinListAsync` jest `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="278ae-338">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="278ae-339">Ścieżka adresu URL, który przesyła do `OnPostJoinListUCAsync` jest `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="278ae-339">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="278ae-340">Trasy niestandardowe</span><span class="sxs-lookup"><span data-stu-id="278ae-340">Custom routes</span></span>

<span data-ttu-id="278ae-341">Użyj `@page` dyrektywy do:</span><span class="sxs-lookup"><span data-stu-id="278ae-341">Use the `@page` directive to:</span></span>

* <span data-ttu-id="278ae-342">Określ niestandardowe trasy do strony.</span><span class="sxs-lookup"><span data-stu-id="278ae-342">Specify a custom route to a page.</span></span> <span data-ttu-id="278ae-343">Na przykład można ustawić trasy do strony informacje `/Some/Other/Path` z `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="278ae-343">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="278ae-344">Dołącz segmentów do trasy domyślnej strony.</span><span class="sxs-lookup"><span data-stu-id="278ae-344">Append segments to a page's default route.</span></span> <span data-ttu-id="278ae-345">Na przykład "item" segmentu można dodać do trasy domyślnej strony z `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="278ae-345">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="278ae-346">Dołącz parametry do trasy domyślnej strony.</span><span class="sxs-lookup"><span data-stu-id="278ae-346">Append parameters to a page's default route.</span></span> <span data-ttu-id="278ae-347">Na przykład parametr ID, `id`, może być wymagana dla strony z `@page "{id}"`.</span><span class="sxs-lookup"><span data-stu-id="278ae-347">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="278ae-348">Ścieżka względem katalogu głównego, wskazywany przez tyldy (`~`) na początku ścieżki jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="278ae-348">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="278ae-349">Na przykład `@page "~/Some/Other/Path"` jest taka sama jak `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="278ae-349">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="278ae-350">Ciąg zapytania można zmienić `?handler=JoinList` w adresie URL do segmentu trasy `/JoinList` , określając szablon trasy `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="278ae-350">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="278ae-351">Jeśli nie chcesz, ciąg zapytania `?handler=JoinList` w adresie URL, można zmienić trasy, aby umieścić nazwę programu obsługi w części ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="278ae-351">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="278ae-352">Trasy można dostosować, dodając szablonu trasy ująć w podwójny cudzysłów po `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="278ae-352">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="278ae-353">Przy użyciu poprzedniego kodu ścieżkę adresu URL, który przesyła do `OnPostJoinListAsync` jest `http://localhost:5000/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="278ae-353">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="278ae-354">Ścieżka adresu URL, który przesyła do `OnPostJoinListUCAsync` jest `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="278ae-354">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="278ae-355">`?` Następujące `handler` oznacza, że parametr trasy jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="278ae-355">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="278ae-356">Konfiguracja i ustawienia</span><span class="sxs-lookup"><span data-stu-id="278ae-356">Configuration and settings</span></span>

<span data-ttu-id="278ae-357">Aby skonfigurować opcje zaawansowane, użyj metody rozszerzenia `AddRazorPagesOptions` w Konstruktorze MVC:</span><span class="sxs-lookup"><span data-stu-id="278ae-357">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="278ae-358">Obecnie można użyć `RazorPagesOptions` Ustaw katalog główny strony lub Dodaj konwencje modelu aplikacji dla stron.</span><span class="sxs-lookup"><span data-stu-id="278ae-358">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="278ae-359">Firma Microsoft będzie włączyć więcej rozszerzeń w ten sposób w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="278ae-359">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="278ae-360">Wstępnej kompilacji widoków, zobacz [kompilacji widoku Razor](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="278ae-360">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="278ae-361">[Pobrania lub wyświetlenia przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="278ae-361">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="278ae-362">Zobacz [wprowadzenie stron Razor](xref:tutorials/razor-pages/razor-pages-start), która opiera się na to wprowadzenie.</span><span class="sxs-lookup"><span data-stu-id="278ae-362">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="278ae-363">Określ, czy stron Razor zawartości katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="278ae-363">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="278ae-364">Domyślnie stron Razor są umieszczone w */strony* katalogu.</span><span class="sxs-lookup"><span data-stu-id="278ae-364">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="278ae-365">Dodaj [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) stron Razor są zawartości katalogu głównego ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) aplikacji:</span><span class="sxs-lookup"><span data-stu-id="278ae-365">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="278ae-366">Określ, czy w katalogu głównym niestandardowych stron Razor</span><span class="sxs-lookup"><span data-stu-id="278ae-366">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="278ae-367">Dodaj [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) do określenia, czy stron Razor w katalogu głównym niestandardowych w aplikacji (podaj ścieżkę względną):</span><span class="sxs-lookup"><span data-stu-id="278ae-367">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="278ae-368">Zobacz także</span><span class="sxs-lookup"><span data-stu-id="278ae-368">See also</span></span>

* [<span data-ttu-id="278ae-369">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="278ae-369">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="278ae-370">Składnia Razor</span><span class="sxs-lookup"><span data-stu-id="278ae-370">Razor syntax</span></span>](xref:mvc/views/razor)
* [<span data-ttu-id="278ae-371">Wprowadzenie do korzystania ze stron Razor</span><span class="sxs-lookup"><span data-stu-id="278ae-371">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="278ae-372">Konwencje autoryzacji stron razor</span><span class="sxs-lookup"><span data-stu-id="278ae-372">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="278ae-373">Razor strony trasy i strony modelu dostawców niestandardowych</span><span class="sxs-lookup"><span data-stu-id="278ae-373">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* [<span data-ttu-id="278ae-374">Testy jednostkowe stron Razor</span><span class="sxs-lookup"><span data-stu-id="278ae-374">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)
