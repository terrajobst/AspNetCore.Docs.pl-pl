---
title: Wprowadzenie do stron Razor programu ASP.NET Core
author: Rick-Anderson
description: Dowiedz się, jak stron Razor w programie ASP.NET Core umożliwia kodowania scenariuszy skoncentrowane na stronie łatwiejsze i bardziej wydajne niż przy użyciu platformy MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 04/06/2019
uid: razor-pages/index
ms.openlocfilehash: 93796fa1edfa316790794d3775342147ea28ae2e
ms.sourcegitcommit: 5dd2ce9709c9e41142771e652d1a4bd0b5248cec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692544"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="aeaea-103">Wprowadzenie do stron Razor programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aeaea-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="aeaea-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="aeaea-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="aeaea-105">Strony razor jest nowy aspekt ASP.NET Core MVC, która sprawia, że kodowania skoncentrowane na stronie scenariuszy łatwiejsze i bardziej wydajne.</span><span class="sxs-lookup"><span data-stu-id="aeaea-105">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="aeaea-106">Jeśli szukasz samouczka, który korzysta z metody Model-View-Controller [Rozpoczynanie pracy z platformą ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="aeaea-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="aeaea-107">Ten dokument zawiera wprowadzenie do stron Razor.</span><span class="sxs-lookup"><span data-stu-id="aeaea-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="aeaea-108">Nie jest samouczek krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="aeaea-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="aeaea-109">Jeśli znajdziesz niektóre sekcje zbyt zaawansowany, zobacz [Rozpoczynanie pracy ze stronami Razor](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="aeaea-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="aeaea-110">Aby uzyskać omówienie platformy ASP.NET Core, zobacz [wprowadzenie do platformy ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="aeaea-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="aeaea-111">Utwórz projekt stron Razor</span><span class="sxs-lookup"><span data-stu-id="aeaea-111">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aeaea-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aeaea-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="aeaea-113">Zobacz [Rozpoczynanie pracy ze stronami Razor](xref:tutorials/razor-pages/razor-pages-start) szczegółowe instrukcje dotyczące sposobu tworzenia projektu stron Razor.</span><span class="sxs-lookup"><span data-stu-id="aeaea-113">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="aeaea-114">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="aeaea-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="aeaea-115">Uruchom `dotnet new webapp` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="aeaea-115">Run `dotnet new webapp` from the command line.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="aeaea-116">Uruchom `dotnet new razor` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="aeaea-116">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

<span data-ttu-id="aeaea-117">Otwórz wygenerowany *.csproj* pliku z programu Visual Studio dla komputerów Mac.</span><span class="sxs-lookup"><span data-stu-id="aeaea-117">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="aeaea-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="aeaea-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="aeaea-119">Uruchom `dotnet new webapp` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="aeaea-119">Run `dotnet new webapp` from the command line.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="aeaea-120">Uruchom `dotnet new razor` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="aeaea-120">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

---

## <a name="razor-pages"></a><span data-ttu-id="aeaea-121">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="aeaea-121">Razor Pages</span></span>

<span data-ttu-id="aeaea-122">Strony razor jest włączone w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="aeaea-122">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="aeaea-123">Należy wziąć pod uwagę strona podstawowa: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="aeaea-123">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="aeaea-124">Powyższy kod wygląda o wiele [plik widoku Razor](xref:tutorials/first-mvc-app/adding-view) używane w aplikacji ASP.NET Core za pomocą kontrolerów i widoków.</span><span class="sxs-lookup"><span data-stu-id="aeaea-124">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="aeaea-125">Czym różni się on jest `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="aeaea-125">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="aeaea-126">`@page` sprawia, że plik do Akcja kontrolera MVC — oznacza to, obsługi żądań bezpośrednio, bez przechodzenia przez kontrolera.</span><span class="sxs-lookup"><span data-stu-id="aeaea-126">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="aeaea-127">`@page` musi być pierwszym dyrektywy Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="aeaea-127">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="aeaea-128">`@page` wpływa na działanie innych konstrukcji Razor.</span><span class="sxs-lookup"><span data-stu-id="aeaea-128">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="aeaea-129">Podobny strony, `PageModel` klasy, jest wyświetlany w następujące dwa pliki.</span><span class="sxs-lookup"><span data-stu-id="aeaea-129">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="aeaea-130">*Pages/Index2.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="aeaea-130">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="aeaea-131">*Pages/Index2.cshtml.cs* modelu strony:</span><span class="sxs-lookup"><span data-stu-id="aeaea-131">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="aeaea-132">Zgodnie z Konwencją `PageModel` plik klasy ma taką samą nazwę jak plik strony Razor za pomocą *.cs* dołączane.</span><span class="sxs-lookup"><span data-stu-id="aeaea-132">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="aeaea-133">Na przykład jest poprzedniej strony Razor *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="aeaea-133">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="aeaea-134">Plik zawierający `PageModel` nosi nazwę klasy *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="aeaea-134">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="aeaea-135">Skojarzenia ścieżki adresu URL do strony są określane według lokalizacji strony w systemie plików.</span><span class="sxs-lookup"><span data-stu-id="aeaea-135">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="aeaea-136">W poniższej tabeli przedstawiono ścieżki strony Razor i dopasowywania adresu URL:</span><span class="sxs-lookup"><span data-stu-id="aeaea-136">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="aeaea-137">Nazwa pliku i ścieżka</span><span class="sxs-lookup"><span data-stu-id="aeaea-137">File name and path</span></span>               | <span data-ttu-id="aeaea-138">dopasowanie adresu URL</span><span class="sxs-lookup"><span data-stu-id="aeaea-138">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="aeaea-139">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="aeaea-139">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="aeaea-140">`/` lub `/Index`</span><span class="sxs-lookup"><span data-stu-id="aeaea-140">`/` or `/Index`</span></span> |
| <span data-ttu-id="aeaea-141">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="aeaea-141">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="aeaea-142">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="aeaea-142">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="aeaea-143">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="aeaea-143">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="aeaea-144">`/Store` lub `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="aeaea-144">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="aeaea-145">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="aeaea-145">Notes:</span></span>

* <span data-ttu-id="aeaea-146">Środowisko uruchomieniowe szuka plików stron Razor w *stron* folderu domyślnie.</span><span class="sxs-lookup"><span data-stu-id="aeaea-146">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="aeaea-147">`Index` jest domyślną stroną, podczas gdy adres URL nie zawiera strony.</span><span class="sxs-lookup"><span data-stu-id="aeaea-147">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="aeaea-148">Zapis formularza podstawowego</span><span class="sxs-lookup"><span data-stu-id="aeaea-148">Write a basic form</span></span>

<span data-ttu-id="aeaea-149">Strony razor zaprojektowaną do podejmowania typowe wzorce używane w przeglądarkach, łatwe do wdrożenia podczas kompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aeaea-149">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="aeaea-150">[Wiązanie modelu](xref:mvc/models/model-binding), [pomocników tagów](xref:mvc/views/tag-helpers/intro)i wszystkich pomocników HTML *po prostu działają* z właściwościami, zdefiniowanej w klasie strony Razor.</span><span class="sxs-lookup"><span data-stu-id="aeaea-150">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="aeaea-151">Należy wziąć pod uwagę strona, która implementuje podstawową "Skontaktuj się z nami" formularza dla `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="aeaea-151">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="aeaea-152">Aby wyświetlić przykłady w niniejszym dokumencie `DbContext` jest inicjowana w [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) pliku.</span><span class="sxs-lookup"><span data-stu-id="aeaea-152">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="aeaea-153">Model danych:</span><span class="sxs-lookup"><span data-stu-id="aeaea-153">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="aeaea-154">Kontekst bazy danych:</span><span class="sxs-lookup"><span data-stu-id="aeaea-154">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="aeaea-155">*Pages/Create.cshtml* Wyświetl plik:</span><span class="sxs-lookup"><span data-stu-id="aeaea-155">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="aeaea-156">*Pages/Create.cshtml.cs* modelu strony:</span><span class="sxs-lookup"><span data-stu-id="aeaea-156">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="aeaea-157">Zgodnie z Konwencją `PageModel` nosi nazwę klasy `<PageName>Model` i znajduje się w tej samej przestrzeni nazw, gdy ta strona.</span><span class="sxs-lookup"><span data-stu-id="aeaea-157">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="aeaea-158">`PageModel` Klasa umożliwia oddzielenie logiki strony z jej prezentacji.</span><span class="sxs-lookup"><span data-stu-id="aeaea-158">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="aeaea-159">Definiuje stronę obsługi żądań wysyłanych do strony i dane używane do renderowania strony.</span><span class="sxs-lookup"><span data-stu-id="aeaea-159">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="aeaea-160">Ta separacja pozwala na zarządzanie zależnościami strony za pomocą [wstrzykiwanie zależności](xref:fundamentals/dependency-injection) i [testu jednostkowego](xref:test/razor-pages-tests) stron.</span><span class="sxs-lookup"><span data-stu-id="aeaea-160">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="aeaea-161">Strona ta zawiera `OnPostAsync` *metody obsługi*, która działa w `POST` żądania (po użytkownik wpisy formularza).</span><span class="sxs-lookup"><span data-stu-id="aeaea-161">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="aeaea-162">Możesz dodać metody obsługi dla dowolnego zlecenie HTTP.</span><span class="sxs-lookup"><span data-stu-id="aeaea-162">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="aeaea-163">Najbardziej typowe procedury obsługi są:</span><span class="sxs-lookup"><span data-stu-id="aeaea-163">The most common handlers are:</span></span>

* <span data-ttu-id="aeaea-164">`OnGet` Aby zainicjować stanu potrzebne dla strony.</span><span class="sxs-lookup"><span data-stu-id="aeaea-164">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="aeaea-165">[OnGet](#OnGet) próbki.</span><span class="sxs-lookup"><span data-stu-id="aeaea-165">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="aeaea-166">`OnPost` do obsługi przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="aeaea-166">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="aeaea-167">`Async` Sufiks nazwy jest opcjonalne, ale jest często używana przez Konwencję w funkcji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="aeaea-167">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="aeaea-168">`OnPostAsync` Kodu w powyższym przykładzie jest podobny do będzie zwykle napisany w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="aeaea-168">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="aeaea-169">Powyższy kod jest typowe dla stron Razor.</span><span class="sxs-lookup"><span data-stu-id="aeaea-169">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="aeaea-170">Większość elementów podstawowych MVC, takie jak [wiązanie modelu](xref:mvc/models/model-binding), [weryfikacji](xref:mvc/models/validation), wyniki akcji są współdzielone.</span><span class="sxs-lookup"><span data-stu-id="aeaea-170">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="aeaea-171">Poprzedni `OnPostAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="aeaea-171">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="aeaea-172">Podstawowy przepływ `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="aeaea-172">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="aeaea-173">Sprawdź, czy błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="aeaea-173">Check for validation errors.</span></span>

* <span data-ttu-id="aeaea-174">Jeśli nie ma żadnych błędów, Zapisz dane i przekierować.</span><span class="sxs-lookup"><span data-stu-id="aeaea-174">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="aeaea-175">Jeśli występują błędy, wyświetlenie strony ponownie przy użyciu komunikatów dotyczących sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="aeaea-175">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="aeaea-176">Weryfikacja po stronie klienta jest identyczna z tradycyjnych aplikacji ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="aeaea-176">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="aeaea-177">W wielu przypadkach błędy sprawdzania poprawności będzie być wykryte na komputerze klienckim i nigdy nie zostały przekazane do serwera.</span><span class="sxs-lookup"><span data-stu-id="aeaea-177">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="aeaea-178">Po pomyślnym wprowadzeniu danych `OnPostAsync` wywołań metody obsługi `RedirectToPage` metody pomocnika do zwrócenia wystąpienia `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-178">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="aeaea-179">`RedirectToPage` to nowy wynik akcji, podobnie jak `RedirectToAction` lub `RedirectToRoute`, ale dostosowane dla stron.</span><span class="sxs-lookup"><span data-stu-id="aeaea-179">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="aeaea-180">W poprzednim przykładzie, zostanie przekierowany do strony indeksu głównego (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="aeaea-180">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="aeaea-181">`RedirectToPage` została szczegółowo opisana w [Generowanie adresu URL dla stron](#url_gen) sekcji.</span><span class="sxs-lookup"><span data-stu-id="aeaea-181">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="aeaea-182">Kiedy przesłanego formularza ma błędy sprawdzania poprawności, (które są przekazywane do serwera),`OnPostAsync` wywołań metody obsługi `Page` metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="aeaea-182">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="aeaea-183">`Page` Zwraca wystąpienie `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-183">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="aeaea-184">Zwracanie `Page` jest podobny do sposobu Zwróć akcje w kontrolerach `View`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-184">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="aeaea-185">`PageResult` jest wartością domyślną</span><span class="sxs-lookup"><span data-stu-id="aeaea-185">`PageResult` is the default</span></span> <!-- Review  --> <span data-ttu-id="aeaea-186">zwracany typ metody programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="aeaea-186">return type for a handler method.</span></span> <span data-ttu-id="aeaea-187">Metody obsługi, która zwraca `void` renderuje stronę.</span><span class="sxs-lookup"><span data-stu-id="aeaea-187">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="aeaea-188">`Customer` Używa właściwości `[BindProperty]` atrybutu zgadzaj się na wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="aeaea-188">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="aeaea-189">Strony razor domyślnie powiązania właściwości tylko zlecenia-GET.</span><span class="sxs-lookup"><span data-stu-id="aeaea-189">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="aeaea-190">Powiązanie z właściwościami może zmniejszyć ilość kodu, który trzeba napisać.</span><span class="sxs-lookup"><span data-stu-id="aeaea-190">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="aeaea-191">Powiązanie zmniejsza kodu przy użyciu tej samej właściwości do renderowania pól formularza (`<input asp-for="Customer.Name">`) i akceptuje dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="aeaea-191">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="aeaea-192">Strona główna (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="aeaea-192">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="aeaea-193">Skojarzone `PageModel` klasy (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="aeaea-193">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="aeaea-194">*Index.cshtml* plik zawiera następujące znaczniki, aby utworzyć link edycji dla każdego kontaktu:</span><span class="sxs-lookup"><span data-stu-id="aeaea-194">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="aeaea-195">[Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) używane `asp-route-{value}` atrybutu, aby wygenerować łącze do strony edytowania.</span><span class="sxs-lookup"><span data-stu-id="aeaea-195">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="aeaea-196">Ten link zawiera dane trasy z kontaktem identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="aeaea-196">The link contains route data with the contact ID.</span></span> <span data-ttu-id="aeaea-197">Na przykład `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-197">For example, `http://localhost:5000/Edit/1`.</span></span> <span data-ttu-id="aeaea-198">Użyj `asp-area` atrybutu, aby określić obszar.</span><span class="sxs-lookup"><span data-stu-id="aeaea-198">Use the `asp-area` attribute to specify an area.</span></span> <span data-ttu-id="aeaea-199">Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="aeaea-199">For more information, see <xref:mvc/controllers/areas>.</span></span>

<span data-ttu-id="aeaea-200">*Pages/Edit.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="aeaea-200">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="aeaea-201">Pierwszy wiersz zawiera `@page "{id:int}"` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="aeaea-201">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="aeaea-202">Ograniczenia routingu`"{id:int}"` informuje strony aby akceptował żądania do strony, które zawierają `int` przekierowywanie danych.</span><span class="sxs-lookup"><span data-stu-id="aeaea-202">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="aeaea-203">Jeśli żądanie strony nie zawiera danych trasy, który może zostać przekonwertowany na `int`, środowisko wykonawcze zwraca błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="aeaea-203">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="aeaea-204">Aby wprowadzić identyfikator jest opcjonalne, należy dołączyć `?` do ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="aeaea-204">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="aeaea-205">*Pages/Edit.cshtml.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="aeaea-205">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="aeaea-206">*Index.cshtml* plik zawiera także znaczników, aby utworzyć przycisk Usuń, dla każdego kontaktu klienta:</span><span class="sxs-lookup"><span data-stu-id="aeaea-206">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="aeaea-207">Gdy przycisk Usuń jest renderowany w języku HTML, jego `formaction` obejmuje parametry:</span><span class="sxs-lookup"><span data-stu-id="aeaea-207">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="aeaea-208">Określony przez identyfikator kontaktu klienta z `asp-route-id` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="aeaea-208">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="aeaea-209">`handler` Określony przez `asp-page-handler` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="aeaea-209">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="aeaea-210">Oto przykład przycisku Usuń renderowany przy użyciu klienta skontaktować się z Identyfikatora `1`:</span><span class="sxs-lookup"><span data-stu-id="aeaea-210">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="aeaea-211">Po wybraniu przycisku, formularz `POST` żądanie jest wysyłane do serwera.</span><span class="sxs-lookup"><span data-stu-id="aeaea-211">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="aeaea-212">Zgodnie z Konwencją Nazwa metody obsługi jest zaznaczona, na podstawie wartości z `handler` parametru, zgodnie ze schematem `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-212">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="aeaea-213">Ponieważ `handler` jest `delete` w tym przykładzie `OnPostDeleteAsync` metody obsługi jest używany do procesu `POST` żądania.</span><span class="sxs-lookup"><span data-stu-id="aeaea-213">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="aeaea-214">Jeśli `asp-page-handler` jest ustawiona na inną wartość, takich jak `remove`, metodę programu obsługi strony o nazwie `OnPostRemoveAsync` jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="aeaea-214">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="aeaea-215">`OnPostDeleteAsync` Metody:</span><span class="sxs-lookup"><span data-stu-id="aeaea-215">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="aeaea-216">Akceptuje `id` z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="aeaea-216">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="aeaea-217">Zapytania bazy danych dla kontaktu klienta z `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-217">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="aeaea-218">Jeśli zostanie znaleziony kontakt z klientem, zostali oni usunięci z listy kontaktów klienta.</span><span class="sxs-lookup"><span data-stu-id="aeaea-218">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="aeaea-219">Baza danych jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="aeaea-219">The database is updated.</span></span>
* <span data-ttu-id="aeaea-220">Wywołania `RedirectToPage` do przekierowania do strony indeksu głównego (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="aeaea-220">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="aeaea-221">Oznacz właściwości strony zgodnie z potrzebami</span><span class="sxs-lookup"><span data-stu-id="aeaea-221">Mark page properties as required</span></span>

<span data-ttu-id="aeaea-222">Właściwości `PageModel` być dekorowane za pomocą [wymagane](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) atrybutu:</span><span class="sxs-lookup"><span data-stu-id="aeaea-222">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="aeaea-223">Aby uzyskać więcej informacji, zobacz [Walidacja modelu](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="aeaea-223">For more information, see [Model validation](xref:mvc/models/validation).</span></span>

## <a name="manage-head-requests-with-the-onget-handler"></a><span data-ttu-id="aeaea-224">Zarządzanie żądaniami HEAD za pomocą obsługi OnGet</span><span class="sxs-lookup"><span data-stu-id="aeaea-224">Manage HEAD requests with the OnGet handler</span></span>

<span data-ttu-id="aeaea-225">Żądania HEAD umożliwiają pobieranie nagłówki dla określonego zasobu.</span><span class="sxs-lookup"><span data-stu-id="aeaea-225">HEAD requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="aeaea-226">Inaczej niż w przypadku żądania GET HEAD żądań nie zwracają treść odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="aeaea-226">Unlike GET requests, HEAD requests don't return a response body.</span></span>

<span data-ttu-id="aeaea-227">Zazwyczaj obsługi HEAD jest tworzony i wywołana dla żądania HEAD:</span><span class="sxs-lookup"><span data-stu-id="aeaea-227">Ordinarily, a HEAD handler is created and called for HEAD requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="aeaea-228">Jeśli żadna procedura obsługi HEAD (`OnHead`) jest zdefiniowany, stron Razor powraca do wywoływania procedury obsługi strony GET (`OnGet`) w programie ASP.NET Core 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="aeaea-228">If no HEAD handler (`OnHead`) is defined, Razor Pages falls back to calling the GET page handler (`OnGet`) in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="aeaea-229">W programie ASP.NET Core 2.1 i 2.2 to zachowanie dotyczy [SetCompatibilityVersion](xref:mvc/compatibility-version) w `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="aeaea-229">In ASP.NET Core 2.1 and 2.2, this behavior occurs with the [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.Configure`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="aeaea-230">Generowanie szablonów domyślnych `SetCompatibilityVersion` wywołania platformy ASP.NET Core 2.1 i 2.2.</span><span class="sxs-lookup"><span data-stu-id="aeaea-230">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span>

<span data-ttu-id="aeaea-231">`SetCompatibilityVersion` efektywnie ustawia opcję stron Razor `AllowMappingHeadRequestsToGetHandler` do `true`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-231">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="aeaea-232">Zamiast włączenie wszystkich 2.1 zachowania `SetCompatibilityVersion`, użytkownik może jawnie wyrazić zgodę na konkretne zachowania.</span><span class="sxs-lookup"><span data-stu-id="aeaea-232">Rather than opting into all 2.1 behaviors with `SetCompatibilityVersion`, you can explicitly opt-in to specific behaviors.</span></span> <span data-ttu-id="aeaea-233">Poniższy kod zdecyduje się na żądania HEAD mapowanie programu obsługi GET.</span><span class="sxs-lookup"><span data-stu-id="aeaea-233">The following code opts into the mapping HEAD requests to the GET handler.</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="aeaea-234">XSRF/CSRF i stron Razor</span><span class="sxs-lookup"><span data-stu-id="aeaea-234">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="aeaea-235">Nie trzeba pisać kodu dla [antiforgery weryfikacji](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="aeaea-235">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="aeaea-236">Antiforgery generowania tokenu i sprawdzanie poprawności są automatycznie uwzględniane stron Razor.</span><span class="sxs-lookup"><span data-stu-id="aeaea-236">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="aeaea-237">Za pomocą układów, częściowe, szablonów i pomocnicy tagów ze stron Razor</span><span class="sxs-lookup"><span data-stu-id="aeaea-237">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="aeaea-238">Strony pracować ze wszystkimi funkcjami aparatu widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="aeaea-238">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="aeaea-239">Układy, częściowe, szablony, pomocników tagów *_ViewStart.cshtml*, *_ViewImports.cshtml* działają w taki sam sposób jak w przypadku konwencjonalnych widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="aeaea-239">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="aeaea-240">Teraz declutter tę stronę, wykorzystując niektóre z tych możliwości.</span><span class="sxs-lookup"><span data-stu-id="aeaea-240">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="aeaea-241">Dodaj [stronę układu](xref:mvc/views/layout) do *Pages/Shared/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="aeaea-241">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="aeaea-242">Dodaj [stronę układu](xref:mvc/views/layout) do *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="aeaea-242">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

::: moniker-end

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="aeaea-243">[Układ](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="aeaea-243">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="aeaea-244">Kontroluje układ każdej strony (chyba że strona oznacza brak zgody na układ).</span><span class="sxs-lookup"><span data-stu-id="aeaea-244">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="aeaea-245">Importuje HTML struktur, takich jak JavaScript i arkusze stylów.</span><span class="sxs-lookup"><span data-stu-id="aeaea-245">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="aeaea-246">Zobacz [stronę układu](xref:mvc/views/layout) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="aeaea-246">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="aeaea-247">[Układ](xref:mvc/views/layout#specifying-a-layout) właściwość jest ustawiona *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="aeaea-247">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="aeaea-248">Układ jest *stron/Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="aeaea-248">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="aeaea-249">Strony wyszukać inne widoki (układów, szablony, częściowe) hierarchiczny, w tym samym folderze co bieżącej strony.</span><span class="sxs-lookup"><span data-stu-id="aeaea-249">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="aeaea-250">Układ w *stron/Shared* folderu można używać na dowolnej strony Razor, w obszarze *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="aeaea-250">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="aeaea-251">Plik układu powinny przejść *stron/Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="aeaea-251">The layout file should go in the *Pages/Shared* folder.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="aeaea-252">Układ jest *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="aeaea-252">The layout is in the *Pages* folder.</span></span> <span data-ttu-id="aeaea-253">Strony wyszukać inne widoki (układów, szablony, częściowe) hierarchiczny, w tym samym folderze co bieżącej strony.</span><span class="sxs-lookup"><span data-stu-id="aeaea-253">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="aeaea-254">Układ w *stron* folderu można używać na dowolnej strony Razor, w obszarze *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="aeaea-254">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

::: moniker-end

<span data-ttu-id="aeaea-255">Firma Microsoft zaleca **nie** umieścić ten plik układu w *widoków/Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="aeaea-255">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="aeaea-256">*Widoki/Shared* jest wzorzec widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="aeaea-256">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="aeaea-257">Strony razor są przeznaczone do zależą od hierarchii folderów, nie ścieżka Konwencji.</span><span class="sxs-lookup"><span data-stu-id="aeaea-257">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="aeaea-258">Wyszukiwanie w widoku ze strony Razor obejmuje *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="aeaea-258">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="aeaea-259">Układy, szablony i częściowych używasz konwencjonalnych Razor widoków i kontrolerów MVC *po prostu działają*.</span><span class="sxs-lookup"><span data-stu-id="aeaea-259">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="aeaea-260">Dodaj *Pages/_ViewImports.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="aeaea-260">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="aeaea-261">`@namespace` zostało wyjaśnione w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="aeaea-261">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="aeaea-262">`@addTagHelper` Wiąże dyrektywy [wbudowanych pomocników tagów](xref:mvc/views/tag-helpers/builtin-th/Index) do wszystkich stron w *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="aeaea-262">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="aeaea-263">Gdy `@namespace` dyrektywa jest używany jawnie na stronie:</span><span class="sxs-lookup"><span data-stu-id="aeaea-263">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="aeaea-264">Dyrektywa ustawia obszar nazw dla strony.</span><span class="sxs-lookup"><span data-stu-id="aeaea-264">The directive sets the namespace for the page.</span></span> <span data-ttu-id="aeaea-265">`@model` Dyrektywy nie musi zawierać przestrzeń nazw.</span><span class="sxs-lookup"><span data-stu-id="aeaea-265">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="aeaea-266">Gdy `@namespace` dyrektywy znajduje się w *_ViewImports.cshtml*, określonego obszaru nazw dostarcza prefiksu dla przestrzeni nazw wygenerowanej na stronie, który importuje `@namespace` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="aeaea-266">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="aeaea-267">Pozostała część wygenerowana przestrzeń nazw (sufiks fragment) jest oddzielona względnej ścieżki między folder zawierający *_ViewImports.cshtml* i folderu zawierającego strony.</span><span class="sxs-lookup"><span data-stu-id="aeaea-267">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="aeaea-268">Na przykład `PageModel` klasy *Pages/Customers/Edit.cshtml.cs* jawnie określa przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="aeaea-268">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="aeaea-269">*Pages/_ViewImports.cshtml* pliku ustawia następująca przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="aeaea-269">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="aeaea-270">Wygenerowany obszar nazw dla *Pages/Customers/Edit.cshtml* strona Razor jest taka sama jak `PageModel` klasy.</span><span class="sxs-lookup"><span data-stu-id="aeaea-270">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="aeaea-271">`@namespace` *współpracuje również z konwencjonalnych widokami Razor.*</span><span class="sxs-lookup"><span data-stu-id="aeaea-271">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="aeaea-272">Oryginalny *Pages/Create.cshtml* Wyświetl plik:</span><span class="sxs-lookup"><span data-stu-id="aeaea-272">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="aeaea-273">Zaktualizowany interfejs *Pages/Create.cshtml* Wyświetl plik:</span><span class="sxs-lookup"><span data-stu-id="aeaea-273">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="aeaea-274">[Projekt startowy stron Razor](#rpvs17) zawiera *Pages/_ValidationScriptsPartial.cshtml*, która przechwytuje sprawdzania poprawności po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="aeaea-274">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="aeaea-275">Aby uzyskać więcej informacji na temat widoki częściowe, zobacz <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="aeaea-275">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="aeaea-276">Generowanie adresu URL dla stron</span><span class="sxs-lookup"><span data-stu-id="aeaea-276">URL generation for Pages</span></span>

<span data-ttu-id="aeaea-277">`Create` Strona, pokazana wcześniej, używa `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="aeaea-277">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="aeaea-278">Aplikacja ma następującą strukturę pliku/folderu:</span><span class="sxs-lookup"><span data-stu-id="aeaea-278">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="aeaea-279">*/ Stron*</span><span class="sxs-lookup"><span data-stu-id="aeaea-279">*/Pages*</span></span>

  * <span data-ttu-id="aeaea-280">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="aeaea-280">*Index.cshtml*</span></span>
  * <span data-ttu-id="aeaea-281">*/ Klientów*</span><span class="sxs-lookup"><span data-stu-id="aeaea-281">*/Customers*</span></span>

    * <span data-ttu-id="aeaea-282">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="aeaea-282">*Create.cshtml*</span></span>
    * <span data-ttu-id="aeaea-283">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="aeaea-283">*Edit.cshtml*</span></span>
    * <span data-ttu-id="aeaea-284">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="aeaea-284">*Index.cshtml*</span></span>

<span data-ttu-id="aeaea-285">*Pages/Customers/Create.cshtml* i *Pages/Customers/Edit.cshtml* strony przekierowania do *Pages/Index.cshtml* po pomyślnym zakończeniu.</span><span class="sxs-lookup"><span data-stu-id="aeaea-285">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="aeaea-286">Ciąg `/Index` jest częścią identyfikatora URI, aby uzyskać dostęp do poprzedniej strony.</span><span class="sxs-lookup"><span data-stu-id="aeaea-286">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="aeaea-287">Ciąg `/Index` może służyć do generowania identyfikatorów URI *Pages/Index.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="aeaea-287">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="aeaea-288">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="aeaea-288">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="aeaea-289">Nazwa strony jest ścieżką do strony z katalogu głównego */strony* folder, w tym wiodący `/` (na przykład `/Index`).</span><span class="sxs-lookup"><span data-stu-id="aeaea-289">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="aeaea-290">Poprzednich przykładów generowania adresu URL oferują rozszerzone opcje i funkcji za pośrednictwem hardcoding adresu URL.</span><span class="sxs-lookup"><span data-stu-id="aeaea-290">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="aeaea-291">Generowanie adresu URL używa [routingu](xref:mvc/controllers/routing) można wygenerować i kodowanie parametrów, zgodnie z jak trasy jest zdefiniowany w ścieżce docelowej.</span><span class="sxs-lookup"><span data-stu-id="aeaea-291">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="aeaea-292">Generowanie adresu URL dla stron obsługuje nazwy względnej.</span><span class="sxs-lookup"><span data-stu-id="aeaea-292">URL generation for pages supports relative names.</span></span> <span data-ttu-id="aeaea-293">W poniższej tabeli przedstawiono strony indeksu, który wybrano z różnymi `RedirectToPage` parametry z *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="aeaea-293">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="aeaea-294">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="aeaea-294">RedirectToPage(x)</span></span>| <span data-ttu-id="aeaea-295">Strona</span><span class="sxs-lookup"><span data-stu-id="aeaea-295">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="aeaea-296">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="aeaea-296">RedirectToPage("/Index")</span></span> | <span data-ttu-id="aeaea-297">*Indeks strony*</span><span class="sxs-lookup"><span data-stu-id="aeaea-297">*Pages/Index*</span></span> |
| <span data-ttu-id="aeaea-298">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="aeaea-298">RedirectToPage("./Index");</span></span> | <span data-ttu-id="aeaea-299">*Strony/klientów/indeksu*</span><span class="sxs-lookup"><span data-stu-id="aeaea-299">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="aeaea-300">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="aeaea-300">RedirectToPage("../Index")</span></span> | <span data-ttu-id="aeaea-301">*Indeks strony*</span><span class="sxs-lookup"><span data-stu-id="aeaea-301">*Pages/Index*</span></span> |
| <span data-ttu-id="aeaea-302">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="aeaea-302">RedirectToPage("Index")</span></span>  | <span data-ttu-id="aeaea-303">*Strony/klientów/indeksu*</span><span class="sxs-lookup"><span data-stu-id="aeaea-303">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="aeaea-304">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, i `RedirectToPage("../Index")` są *względne nazwy*.</span><span class="sxs-lookup"><span data-stu-id="aeaea-304">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="aeaea-305">`RedirectToPage` Parametr jest *połączone* ze ścieżką bieżącej strony, aby obliczyć nazwę strony docelowej.</span><span class="sxs-lookup"><span data-stu-id="aeaea-305">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="aeaea-306">Nazwa względna łączenia jest przydatne w przypadku, gdy tworzenia witryn z złożonej strukturze.</span><span class="sxs-lookup"><span data-stu-id="aeaea-306">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="aeaea-307">Jeśli używasz względne nazwy do połączenia między stronami w folderze, można zmienić nazwę tego folderu.</span><span class="sxs-lookup"><span data-stu-id="aeaea-307">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="aeaea-308">Wszystkie łącza nadal działać (ponieważ nie obejmują one nazwę folderu).</span><span class="sxs-lookup"><span data-stu-id="aeaea-308">All the links still work (because they didn't include the folder name).</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="aeaea-309">Przekierowywanie do strony w innej [obszaru](xref:mvc/controllers/areas), określ obszar:</span><span class="sxs-lookup"><span data-stu-id="aeaea-309">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="aeaea-310">Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="aeaea-310">For more information, see <xref:mvc/controllers/areas>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="aeaea-311">Atrybut viewData</span><span class="sxs-lookup"><span data-stu-id="aeaea-311">ViewData attribute</span></span>

<span data-ttu-id="aeaea-312">Dane mogą być przekazywane do stron z [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="aeaea-312">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="aeaea-313">Właściwości na kontrolerach ani w modelach strony Razor ozdobione `[ViewData]` ich wartości przechowywane i ładowane z [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="aeaea-313">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="aeaea-314">W poniższym przykładzie `AboutModel` zawiera `Title` ozdobione właściwość `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-314">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="aeaea-315">`Title` Właściwość jest ustawiona na tytuł strony informacje:</span><span class="sxs-lookup"><span data-stu-id="aeaea-315">The `Title` property is set to the title of the About page:</span></span>

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

<span data-ttu-id="aeaea-316">Na stronie informacje dostępu `Title` właściwość jako właściwość modelu:</span><span class="sxs-lookup"><span data-stu-id="aeaea-316">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="aeaea-317">W układzie tytuł jest do odczytu ze słownika ViewData:</span><span class="sxs-lookup"><span data-stu-id="aeaea-317">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="aeaea-318">TempData</span><span class="sxs-lookup"><span data-stu-id="aeaea-318">TempData</span></span>

<span data-ttu-id="aeaea-319">Udostępnia platformy ASP.NET Core [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) właściwość [kontrolera](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="aeaea-319">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="aeaea-320">Ta właściwość przechowuje dane, dopóki nie jest do odczytu.</span><span class="sxs-lookup"><span data-stu-id="aeaea-320">This property stores data until it's read.</span></span> <span data-ttu-id="aeaea-321">`Keep` i `Peek` metody może służyć do sprawdzenia danych bez usuwania.</span><span class="sxs-lookup"><span data-stu-id="aeaea-321">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="aeaea-322">`TempData` jest przydatne w przypadku przekierowania, gdy dane są potrzebne dla więcej niż jedno żądanie.</span><span class="sxs-lookup"><span data-stu-id="aeaea-322">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="aeaea-323">`[TempData]` Atrybut jest nowego w programie ASP.NET Core 2.0 i jest obsługiwane na stronach i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="aeaea-323">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="aeaea-324">Poniższy kod ustawia wartość `Message` przy użyciu `TempData`:</span><span class="sxs-lookup"><span data-stu-id="aeaea-324">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="aeaea-325">Następujące znaczniki w *Pages/Customers/Index.cshtml* pliku wyświetlana jest wartość `Message` przy użyciu `TempData`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-325">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="aeaea-326">*Pages/Customers/Index.cshtml.cs* stosowany model strony `[TempData]` atrybutu `Message` właściwości.</span><span class="sxs-lookup"><span data-stu-id="aeaea-326">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="aeaea-327">Aby uzyskać więcej informacji, zobacz [TempData](xref:fundamentals/app-state#tempdata) .</span><span class="sxs-lookup"><span data-stu-id="aeaea-327">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="aeaea-328">Procedury obsługi wielu na stronę</span><span class="sxs-lookup"><span data-stu-id="aeaea-328">Multiple handlers per page</span></span>

<span data-ttu-id="aeaea-329">Następująca strona generuje kod znaczników dla dwóch stron przy użyciu programów obsługi `asp-page-handler` Pomocnik tagu:</span><span class="sxs-lookup"><span data-stu-id="aeaea-329">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="aeaea-330">Formularz w poprzednim przykładzie ma dwa przesłać przyciski, za pomocą każdego `FormActionTagHelper` do przesłania do innego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="aeaea-330">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="aeaea-331">`asp-page-handler` Atrybutu jest uzupełnieniem do `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-331">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="aeaea-332">`asp-page-handler` generuje adresy URL, którzy przesłali do każdej metody obsługi zdefiniowane przez stronę.</span><span class="sxs-lookup"><span data-stu-id="aeaea-332">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="aeaea-333">`asp-page` nie jest określona, ponieważ plik jest tworzenie łączy do bieżącej strony.</span><span class="sxs-lookup"><span data-stu-id="aeaea-333">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="aeaea-334">Model strony:</span><span class="sxs-lookup"><span data-stu-id="aeaea-334">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="aeaea-335">W poprzednim kodzie użyto *o nazwie metody obsługi*.</span><span class="sxs-lookup"><span data-stu-id="aeaea-335">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="aeaea-336">Metody obsługi o nazwie są tworzone przez przyjęcie tekst nazwę po `On<HTTP Verb>` i przed `Async` (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="aeaea-336">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="aeaea-337">W poprzednim przykładzie, metody strony są OnPost**JoinList**Async i OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="aeaea-337">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="aeaea-338">Za pomocą *OnPost* i *Async* usunięty, są nazwy programów obsługi `JoinList` i `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-338">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="aeaea-339">Za pomocą poprzedniego kodu Ścieżka adresu URL, które są przesyłane `OnPostJoinListAsync` jest `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-339">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="aeaea-340">Ścieżka adresu URL, które są przesyłane `OnPostJoinListUCAsync` jest `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-340">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="aeaea-341">Trasy niestandardowe</span><span class="sxs-lookup"><span data-stu-id="aeaea-341">Custom routes</span></span>

<span data-ttu-id="aeaea-342">Użyj `@page` dyrektywę:</span><span class="sxs-lookup"><span data-stu-id="aeaea-342">Use the `@page` directive to:</span></span>

* <span data-ttu-id="aeaea-343">Określ niestandardowe trasy do strony.</span><span class="sxs-lookup"><span data-stu-id="aeaea-343">Specify a custom route to a page.</span></span> <span data-ttu-id="aeaea-344">Na przykład można ustawić trasy do strony informacje `/Some/Other/Path` z `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-344">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="aeaea-345">Dołącz segmentów do trasy domyślnej strony.</span><span class="sxs-lookup"><span data-stu-id="aeaea-345">Append segments to a page's default route.</span></span> <span data-ttu-id="aeaea-346">Na przykład "item" segmentu można dodać do trasy domyślnej strony z `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-346">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="aeaea-347">Dodaj parametry do trasy domyślnej strony.</span><span class="sxs-lookup"><span data-stu-id="aeaea-347">Append parameters to a page's default route.</span></span> <span data-ttu-id="aeaea-348">Na przykład parametru ID, `id`, może być wymagane dla strony z `@page "{id}"`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-348">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="aeaea-349">Ścieżka względem katalogu głównego, w wyznaczonym przez tyldy (`~`) na początku ścieżki jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="aeaea-349">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="aeaea-350">Na przykład `@page "~/Some/Other/Path"` jest taka sama jak `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-350">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="aeaea-351">Możesz zmienić ciągu zapytania `?handler=JoinList` w adresie URL do segmentu trasy `/JoinList` , określając szablon trasy `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-351">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="aeaea-352">Jeśli nie chcesz, aby ciąg zapytania `?handler=JoinList` w adresie URL, możesz zmienić trasy, aby umieścić nazwę programu obsługi w część ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="aeaea-352">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="aeaea-353">Trasy można dostosować, dodając szablon trasy, ujęte w podwójny cudzysłów po `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="aeaea-353">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="aeaea-354">Za pomocą poprzedniego kodu Ścieżka adresu URL, które są przesyłane `OnPostJoinListAsync` jest `http://localhost:5000/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-354">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="aeaea-355">Ścieżka adresu URL, które są przesyłane `OnPostJoinListUCAsync` jest `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="aeaea-355">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="aeaea-356">`?` Następujące `handler` oznacza, że parametr trasy jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="aeaea-356">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="aeaea-357">Konfiguracja i ustawienia</span><span class="sxs-lookup"><span data-stu-id="aeaea-357">Configuration and settings</span></span>

<span data-ttu-id="aeaea-358">Aby skonfigurować opcje zaawansowane, należy użyć metody rozszerzenia `AddRazorPagesOptions` w Konstruktorze MVC:</span><span class="sxs-lookup"><span data-stu-id="aeaea-358">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="aeaea-359">Obecnie możesz używać `RazorPagesOptions` katalog główny strony lub Dodaj konwencje modelu aplikacji dla stron.</span><span class="sxs-lookup"><span data-stu-id="aeaea-359">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="aeaea-360">Firma Microsoft będzie włączyć więcej rozszerzeń w ten sposób w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="aeaea-360">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="aeaea-361">Przeprowadzać prekompilowanie widoków, zobacz [kompilacja widoku Razor](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="aeaea-361">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="aeaea-362">[Pobieranie i wyświetlanie przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="aeaea-362">[Download or view sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="aeaea-363">Zobacz [Rozpoczynanie pracy ze stronami Razor](xref:tutorials/razor-pages/razor-pages-start), która jest oparta na wprowadzeniu.</span><span class="sxs-lookup"><span data-stu-id="aeaea-363">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="aeaea-364">Określ, czy w katalogu głównym zawartości stron Razor</span><span class="sxs-lookup"><span data-stu-id="aeaea-364">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="aeaea-365">Domyślnie strony Razor są umieszczone w */strony* katalogu.</span><span class="sxs-lookup"><span data-stu-id="aeaea-365">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="aeaea-366">Dodaj [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) do określenia, czy strony Razor zawartości katalogu głównego ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) aplikacji:</span><span class="sxs-lookup"><span data-stu-id="aeaea-366">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="aeaea-367">Określ, czy w katalogu głównym niestandardowych stron Razor</span><span class="sxs-lookup"><span data-stu-id="aeaea-367">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="aeaea-368">Dodaj [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) do określenia, czy strony Razor w katalogu głównym niestandardowego w aplikacji (podaj ścieżkę względną):</span><span class="sxs-lookup"><span data-stu-id="aeaea-368">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="aeaea-369">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="aeaea-369">Additional resources</span></span>

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>
