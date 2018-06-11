---
title: Wprowadzenie do platformy ASP.NET Core stron Razor
author: Rick-Anderson
description: Dowiedz się, jak Razor strony platformy ASP.NET Core umożliwia kodowania scenariusze strony łatwiejsze i bardziej wydajnej pracy niż przy użyciu platformy MVC.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 5/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: mvc/razor-pages/index
ms.openlocfilehash: d515e1354546e95553a010fa7a2143f0e529d68b
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252454"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="86088-103">Wprowadzenie do platformy ASP.NET Core stron Razor</span><span class="sxs-lookup"><span data-stu-id="86088-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="86088-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="86088-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="86088-105">Stron razor jest elementem nowe platformy ASP.NET Core MVC umożliwia kodowanie strony scenariusze łatwiejsze i bardziej wydajnej pracy.</span><span class="sxs-lookup"><span data-stu-id="86088-105">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="86088-106">Jeśli szukasz samouczka, który korzysta z podejścia Model-View-Controller, zobacz [Rozpoczynanie pracy z platformą ASP.NET MVC Core](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="86088-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="86088-107">Ten dokument zawiera wprowadzenie do stron Razor.</span><span class="sxs-lookup"><span data-stu-id="86088-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="86088-108">Nie jest samouczek krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="86088-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="86088-109">Jeśli możesz znaleźć sekcje zbyt zaawansowanych, zobacz [wprowadzenie stron Razor](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="86088-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="86088-110">Omówienie platformy ASP.NET Core, zobacz [wprowadzenie do platformy ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="86088-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="86088-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="86088-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="86088-112">Tworzenie projektu stron Razor</span><span class="sxs-lookup"><span data-stu-id="86088-112">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="86088-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="86088-113">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="86088-114">Zobacz [wprowadzenie stron Razor](xref:tutorials/razor-pages/razor-pages-start) szczegółowe informacje dotyczące sposobu tworzenia stron Razor projektu za pomocą programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="86088-114">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="86088-115">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="86088-115">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="86088-116">Uruchom `dotnet new webapp` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="86088-116">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="86088-118">Uruchom `dotnet new razor` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="86088-118">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

<span data-ttu-id="86088-119">Otwórz wygenerowany *.csproj* plików z programu Visual Studio dla komputerów Mac.</span><span class="sxs-lookup"><span data-stu-id="86088-119">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="86088-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="86088-120">Visual Studio Code</span></span>](#tab/visual-studio-code) 

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="86088-121">Uruchom `dotnet new webapp` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="86088-121">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="86088-123">Uruchom `dotnet new razor` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="86088-123">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="86088-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="86088-124">.NET Core CLI</span></span>](#tab/netcore-cli) 

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="86088-125">Uruchom `dotnet new webapp` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="86088-125">Run `dotnet new webapp` from the command line.</span></span>

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="86088-127">Uruchom `dotnet new razor` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="86088-127">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

---

## <a name="razor-pages"></a><span data-ttu-id="86088-128">Stron razor</span><span class="sxs-lookup"><span data-stu-id="86088-128">Razor Pages</span></span>

<span data-ttu-id="86088-129">Stron razor jest włączone w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="86088-129">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="86088-130">Należy wziąć pod uwagę strony podstawowej: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="86088-130">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="86088-131">Poprzedni kod znacznie wygląda jak plik widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="86088-131">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="86088-132">Jest to, co ułatwia różnych `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="86088-132">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="86088-133">`@page` powoduje, że plik na akcję MVC — czyli obsługi żądań bezpośrednio, bez przechodzenia przez kontroler.</span><span class="sxs-lookup"><span data-stu-id="86088-133">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="86088-134">`@page` musi być pierwszym dyrektywy Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="86088-134">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="86088-135">`@page` wpływa na działanie innych konstrukcji Razor.</span><span class="sxs-lookup"><span data-stu-id="86088-135">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="86088-136">Podobne strony przy użyciu `PageModel` klasa, jest wyświetlany w obszarze następujące dwa pliki.</span><span class="sxs-lookup"><span data-stu-id="86088-136">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="86088-137">*Pages/Index2.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="86088-137">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="86088-138">*Pages/Index2.cshtml.cs* modelu strony:</span><span class="sxs-lookup"><span data-stu-id="86088-138">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="86088-139">Według konwencji `PageModel` pliku klasy ma taką samą nazwę jak plik Razor strony z *.cs* dołączane.</span><span class="sxs-lookup"><span data-stu-id="86088-139">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="86088-140">Na przykład na poprzedniej stronie aparatu Razor jest *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="86088-140">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="86088-141">Plik zawierający `PageModel` nosi nazwę klasy *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="86088-141">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="86088-142">Skojarzenia ścieżki adresu URL do strony zależą od lokalizacji strony w systemie plików.</span><span class="sxs-lookup"><span data-stu-id="86088-142">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="86088-143">W poniższej tabeli przedstawiono ścieżki Razor strony i dopasowywania adresu URL:</span><span class="sxs-lookup"><span data-stu-id="86088-143">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="86088-144">Nazwa i ścieżka pliku</span><span class="sxs-lookup"><span data-stu-id="86088-144">File name and path</span></span>               | <span data-ttu-id="86088-145">Dopasowywanie adresu URL</span><span class="sxs-lookup"><span data-stu-id="86088-145">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="86088-146">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="86088-146">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="86088-147">`/` lub `/Index`</span><span class="sxs-lookup"><span data-stu-id="86088-147">`/` or `/Index`</span></span> |
| <span data-ttu-id="86088-148">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="86088-148">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="86088-149">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="86088-149">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="86088-150">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="86088-150">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="86088-151">`/Store` lub `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="86088-151">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="86088-152">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="86088-152">Notes:</span></span>

* <span data-ttu-id="86088-153">Środowisko uruchomieniowe wyszukuje pliki stron Razor w *stron* folderu domyślnie.</span><span class="sxs-lookup"><span data-stu-id="86088-153">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="86088-154">`Index` jest domyślną stronę, gdy adres URL nie zawiera strony.</span><span class="sxs-lookup"><span data-stu-id="86088-154">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="86088-155">Zapisywanie formularza podstawowego</span><span class="sxs-lookup"><span data-stu-id="86088-155">Writing a basic form</span></span>

<span data-ttu-id="86088-156">Uniemożliwia stron razor typowe wzorce używane w przeglądarkach łatwa do wdrożenia podczas kompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="86088-156">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="86088-157">[Model powiązania](xref:mvc/models/model-binding), [pomocników tagów](xref:mvc/views/tag-helpers/intro)i wszystkich pomocników HTML *tylko pracy* z właściwościami zdefiniowana w klasie Razor strony.</span><span class="sxs-lookup"><span data-stu-id="86088-157">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="86088-158">Należy wziąć pod uwagę strona, która implementuje podstawowego "Skontaktuj się z nami" tworzą dla `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="86088-158">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="86088-159">Aby wyświetlić przykłady w tym dokumencie `DbContext` został zainicjowany w [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) pliku.</span><span class="sxs-lookup"><span data-stu-id="86088-159">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="86088-160">Model danych:</span><span class="sxs-lookup"><span data-stu-id="86088-160">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="86088-161">Kontekst bazy danych:</span><span class="sxs-lookup"><span data-stu-id="86088-161">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="86088-162">*Pages/Create.cshtml* pliku widoku:</span><span class="sxs-lookup"><span data-stu-id="86088-162">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="86088-163">*Pages/Create.cshtml.cs* modelu strony:</span><span class="sxs-lookup"><span data-stu-id="86088-163">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="86088-164">Według konwencji `PageModel` nosi nazwę klasy `<PageName>Model` i znajduje się w tej samej przestrzeni nazw jako strony.</span><span class="sxs-lookup"><span data-stu-id="86088-164">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="86088-165">`PageModel` Klasa umożliwia oddzielenie logiki strony z jego prezentacji.</span><span class="sxs-lookup"><span data-stu-id="86088-165">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="86088-166">Definiuje stronę obsługi żądania wysyłane na stronie i dane używane do renderowania strony.</span><span class="sxs-lookup"><span data-stu-id="86088-166">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="86088-167">Ta separacja umożliwia zarządzanie zależności strony za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection) i [testu jednostkowego](xref:test/razor-pages-tests) stron.</span><span class="sxs-lookup"><span data-stu-id="86088-167">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="86088-168">Na stronie `OnPostAsync` *metoda obsługi*, która działa w `POST` żąda (gdy użytkownik zapisuje formularz).</span><span class="sxs-lookup"><span data-stu-id="86088-168">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="86088-169">Można dodać obsługi metod dla dowolnej zlecenie HTTP.</span><span class="sxs-lookup"><span data-stu-id="86088-169">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="86088-170">Są najczęściej programów obsługi:</span><span class="sxs-lookup"><span data-stu-id="86088-170">The most common handlers are:</span></span>

* <span data-ttu-id="86088-171">`OnGet` Aby zainicjować stanu potrzebne dla strony.</span><span class="sxs-lookup"><span data-stu-id="86088-171">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="86088-172">[OnGet](#OnGet) próbki.</span><span class="sxs-lookup"><span data-stu-id="86088-172">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="86088-173">`OnPost` do obsługi przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="86088-173">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="86088-174">`Async` Sufiks nazwy jest opcjonalne, ale często używane przez Konwencję dla funkcji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="86088-174">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="86088-175">`OnPostAsync` Kodu w poprzednim przykładzie wygląda podobnie do co zwykle piszesz w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="86088-175">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="86088-176">Poprzedni kod jest typowe dla stron Razor.</span><span class="sxs-lookup"><span data-stu-id="86088-176">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="86088-177">Większość podstawowych MVC, takich jak [modelu powiązania](xref:mvc/models/model-binding), [weryfikacji](xref:mvc/models/validation), i są współdzielone wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="86088-177">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="86088-178">Poprzedni `OnPostAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="86088-178">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="86088-179">Podstawowy przepływ `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="86088-179">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="86088-180">Sprawdź błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="86088-180">Check for validation errors.</span></span>

*  <span data-ttu-id="86088-181">Jeśli nie ma żadnych błędów, Zapisz dane i przekierowanie.</span><span class="sxs-lookup"><span data-stu-id="86088-181">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="86088-182">Jeśli wystąpią błędy, Wyświetl stronę ponownie, podając komunikatów dotyczących sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="86088-182">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="86088-183">Weryfikacja po stronie klienta jest identyczna jak tradycyjne aplikacje platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="86088-183">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="86088-184">W wielu przypadkach błędy sprawdzania poprawności może być wykryte na komputerze klienckim i nigdy nie zostały przekazane do serwera.</span><span class="sxs-lookup"><span data-stu-id="86088-184">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="86088-185">Po pomyślnym wprowadzeniu danych `OnPostAsync` wywołań metody obsługi `RedirectToPage` metody pomocnika do zwrócenia wystąpienia klasy `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="86088-185">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="86088-186">`RedirectToPage` jest nowy wynik akcji, podobnie jak `RedirectToAction` lub `RedirectToRoute`, ale dostosowanych stron.</span><span class="sxs-lookup"><span data-stu-id="86088-186">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="86088-187">W poprzednim przykładzie przekierowuje do strony indeksu głównego (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="86088-187">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="86088-188">`RedirectToPage` została szczegółowo opisana w [generowania adresu URL dla stron](#url_gen) sekcji.</span><span class="sxs-lookup"><span data-stu-id="86088-188">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="86088-189">Jeśli przesłanego formularza zawiera błędy sprawdzania poprawności (które są przekazywane do serwera),`OnPostAsync` wywołań metody obsługi `Page` metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="86088-189">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="86088-190">`Page` Zwraca wystąpienie klasy `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="86088-190">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="86088-191">Zwracanie `Page` jest podobny do sposób zwrócenia akcje w kontrolerach `View`.</span><span class="sxs-lookup"><span data-stu-id="86088-191">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="86088-192">`PageResult` Wartość domyślna to <!-- Review  --> zwracany typ metody obsługi.</span><span class="sxs-lookup"><span data-stu-id="86088-192">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="86088-193">Metoda obsługi, która zwraca `void` renderuje stronę.</span><span class="sxs-lookup"><span data-stu-id="86088-193">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="86088-194">`Customer` Używa właściwości `[BindProperty]` atrybutu korzystania z wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="86088-194">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="86088-195">Stron razor domyślnie powiązania właściwości tylko z innych niż GET zleceń.</span><span class="sxs-lookup"><span data-stu-id="86088-195">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="86088-196">Powiązanie właściwości może zmniejszyć ilość kodu, które trzeba zapisać.</span><span class="sxs-lookup"><span data-stu-id="86088-196">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="86088-197">Powiązanie zmniejsza kodu przy użyciu tej samej właściwości do renderowania pól formularza (`<input asp-for="Customer.Name" />`) i akceptuje dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="86088-197">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="86088-198">Ze względów bezpieczeństwa należy zgadzaj się na wiązanie danych żądania GET do strony właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="86088-198">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="86088-199">Sprawdź dane wejściowe użytkownika przed zamapowaniem ją do właściwości.</span><span class="sxs-lookup"><span data-stu-id="86088-199">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="86088-200">Zgody na korzystanie z to zachowanie jest przydatne, gdy adresowania scenariusze, które zależą od wartości ciągu lub trasy kwerendy.</span><span class="sxs-lookup"><span data-stu-id="86088-200">Opting in to this behavior is useful when addressing scenarios which rely on query string or route values.</span></span>
>
> <span data-ttu-id="86088-201">Aby powiązać właściwość na żądania GET, ustaw `[BindProperty]` atrybutu `SupportsGet` właściwości `true`: `[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="86088-201">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="86088-202">Strona główna (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="86088-202">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="86088-203">Kod związany z *Index.cshtml.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="86088-203">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="86088-204">*Index.cshtml* pliku zawiera następujące znaczniki, aby utworzyć link edycji dla każdego kontakt:</span><span class="sxs-lookup"><span data-stu-id="86088-204">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="86088-205">[Pomocnika Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) używane `asp-route-{value}` atrybut do generowania łącza do edycji strony.</span><span class="sxs-lookup"><span data-stu-id="86088-205">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="86088-206">Ten link zawiera dane trasy, o kontakt identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="86088-206">The link contains route data with the contact ID.</span></span> <span data-ttu-id="86088-207">Na przykład `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="86088-207">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="86088-208">*Pages/Edit.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="86088-208">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="86088-209">Pierwszy wiersz zawiera `@page "{id:int}"` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="86088-209">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="86088-210">Ograniczenie routingu`"{id:int}"` informuje strony do akceptowania żądań do strony, które zawierają `int` danych trasy.</span><span class="sxs-lookup"><span data-stu-id="86088-210">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="86088-211">Jeśli żądanie do strony nie zawiera danych trasy, który może zostać przekonwertowany na `int`, środowisko uruchomieniowe zwraca błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="86088-211">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="86088-212">Aby wprowadzić identyfikator opcjonalne, dołącz `?` ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="86088-212">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="86088-213">*Pages/Edit.cshtml.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="86088-213">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="86088-214">*Index.cshtml* plik zawiera także znaczników do utworzenia przycisk Usuń widoczny dla każdego kontakt klienta:</span><span class="sxs-lookup"><span data-stu-id="86088-214">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="86088-215">Przycisk usuwania jest renderowany w języku HTML, jego `formaction` zawiera parametry:</span><span class="sxs-lookup"><span data-stu-id="86088-215">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="86088-216">Określony przez identyfikator kontaktu klienta z `asp-route-id` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="86088-216">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="86088-217">`handler` Określonego przez `asp-page-handler` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="86088-217">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="86088-218">Poniżej przedstawiono przykładowy przycisk usuwania renderowany z klientem skontaktuj się z Identyfikatorem `1`:</span><span class="sxs-lookup"><span data-stu-id="86088-218">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="86088-219">Jeśli przycisk jest zaznaczony, formularz `POST` wysłaniu żądania do serwera.</span><span class="sxs-lookup"><span data-stu-id="86088-219">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="86088-220">Według Konwencji wybrano nazwę metody obsługi na podstawie wartości `handler` parametru zgodnie ze schematem `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="86088-220">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="86088-221">Ponieważ `handler` jest `delete` w tym przykładzie `OnPostDeleteAsync` metoda obsługi jest używany do procesu `POST` żądania.</span><span class="sxs-lookup"><span data-stu-id="86088-221">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="86088-222">Jeśli `asp-page-handler` ustawiono innej wartości, takich jak `remove`, strona metody obsługi o nazwie `OnPostRemoveAsync` jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="86088-222">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="86088-223">`OnPostDeleteAsync` Metody:</span><span class="sxs-lookup"><span data-stu-id="86088-223">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="86088-224">Akceptuje `id` z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="86088-224">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="86088-225">Wysyła zapytanie do bazy danych dla klienta kontakt z `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="86088-225">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="86088-226">Jeśli zostanie znaleziony kontaktowe klienta, są one usunięte z listy kontaktów klienta.</span><span class="sxs-lookup"><span data-stu-id="86088-226">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="86088-227">Baza danych jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="86088-227">The database is updated.</span></span>
* <span data-ttu-id="86088-228">Wywołania `RedirectToPage` ma nastąpić przekierowanie do strony indeksu głównego (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="86088-228">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-required"></a><span data-ttu-id="86088-229">Wymagane właściwości strony znaku</span><span class="sxs-lookup"><span data-stu-id="86088-229">Mark page properties required</span></span>

<span data-ttu-id="86088-230">Właściwości `PageModel` może korzystać z [wymagane](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) atrybutu:</span><span class="sxs-lookup"><span data-stu-id="86088-230">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

<span data-ttu-id="86088-231">[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]</span><span class="sxs-lookup"><span data-stu-id="86088-231">[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]</span></span>

<span data-ttu-id="86088-232">Zobacz [modelu weryfikacji](xref:mvc/models/validation) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="86088-232">See [Model validation](xref:mvc/models/validation) for more information.</span></span>

## <a name="manage-head-requests-with-the-onget-handler"></a><span data-ttu-id="86088-233">Zarządzaj żądaniami HEAD z obsługą OnGet</span><span class="sxs-lookup"><span data-stu-id="86088-233">Manage HEAD requests with the OnGet handler</span></span>

<span data-ttu-id="86088-234">Zwykle HEAD program obsługi jest tworzony i wywołana dla żądania HEAD:</span><span class="sxs-lookup"><span data-stu-id="86088-234">Ordinarily, a HEAD handler is created and called for HEAD requests:</span></span>

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="86088-235">Jeśli bez obsługi HEAD (`OnHead`) jest zdefiniowany, stron Razor nastąpi powrót do wywoływania obsługi strony GET (`OnGet`) platformy ASP.NET Core 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="86088-235">If no HEAD handler (`OnHead`) is defined, Razor Pages falls back to calling the GET page handler (`OnGet`) in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="86088-236">Zgódź się na zachowanie w przypadku [metody SetCompatibilityVersion](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) w `Startup.Configure` dla platformy ASP.NET Core 2.1 2.x:</span><span class="sxs-lookup"><span data-stu-id="86088-236">Opt in to this behavior with the [SetCompatibilityVersion method](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) in `Startup.Configure` for ASP.NET Core 2.1 to 2.x:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="86088-237">`SetCompatibilityVersion` efektywnie ustawia opcję stron Razor `AllowMappingHeadRequestsToGetHandler` do `true`.</span><span class="sxs-lookup"><span data-stu-id="86088-237">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="86088-238">Zamiast Rezygnacja do wszystkich zachowań 2.1 z `SetCompatibilityVersion`, użytkownik może jawnie wyrazić zgodę na konkretne zachowania.</span><span class="sxs-lookup"><span data-stu-id="86088-238">Rather than opting into all 2.1 behaviors with `SetCompatibilityVersion`, you can explicitly opt-in to specific behaviors.</span></span> <span data-ttu-id="86088-239">Poniższy kod wybranych żądań HEAD mapowanie do programu obsługi pobierania.</span><span class="sxs-lookup"><span data-stu-id="86088-239">The following code opts into the mapping HEAD requests to the GET handler.</span></span>


```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```
::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="86088-240">XSRF/CSRF i stron Razor</span><span class="sxs-lookup"><span data-stu-id="86088-240">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="86088-241">Nie masz do pisania kodu dla [antiforgery weryfikacji](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="86088-241">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="86088-242">Antiforgery generowania tokenów i weryfikacja są automatycznie umieszczane w stron Razor.</span><span class="sxs-lookup"><span data-stu-id="86088-242">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="86088-243">Za pomocą układów, częściowe, szablonów i pomocników tagów Razor strony</span><span class="sxs-lookup"><span data-stu-id="86088-243">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="86088-244">Strony pracować ze wszystkimi funkcjami systemu aparatu widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="86088-244">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="86088-245">Układy, częściowe, szablony, pomocników tagów *_ViewStart.cshtml*, *_ViewImports.cshtml* pracy w taki sam sposób jak w przypadku konwencjonalnych widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="86088-245">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="86088-246">Dzięki wykorzystaniu niektóre z tych funkcji umożliwia declutter tej strony.</span><span class="sxs-lookup"><span data-stu-id="86088-246">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="86088-247">Dodaj [układ strony](xref:mvc/views/layout) do *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="86088-247">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="86088-248">[Układu](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="86088-248">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="86088-249">Określa układ każdej strony (o ile nie zdecyduje strony poza układ).</span><span class="sxs-lookup"><span data-stu-id="86088-249">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="86088-250">Importuje struktury HTML, takich jak JavaScript i arkusze stylów.</span><span class="sxs-lookup"><span data-stu-id="86088-250">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="86088-251">Zobacz [układ strony](xref:mvc/views/layout) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="86088-251">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="86088-252">[Układu](xref:mvc/views/layout#specifying-a-layout) właściwość jest ustawiona *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="86088-252">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="86088-253">Układ jest *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="86088-253">The layout is in the *Pages* folder.</span></span> <span data-ttu-id="86088-254">Strony wyszukać innych widoków (układy, szablony, częściowe) hierarchicznie, uruchamianie w tym samym folderze co bieżąca strona.</span><span class="sxs-lookup"><span data-stu-id="86088-254">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="86088-255">Układ w *stron* folderu można używać z dowolnej strony Razor, w obszarze *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="86088-255">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="86088-256">Firma Microsoft zaleca **nie** umieścić plik układu w *widoków/Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="86088-256">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="86088-257">*Widoki/Shared* jest wzorzec widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="86088-257">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="86088-258">Stron razor są przeznaczone do zależą od hierarchii folderów, nie ścieżkę Konwencji.</span><span class="sxs-lookup"><span data-stu-id="86088-258">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="86088-259">Widok wyszukiwania ze strony Razor obejmuje *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="86088-259">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="86088-260">Układy, szablonów i częściowe jest używany z konwencjonalnej Razor widoków i kontrolerów MVC *tylko pracy*.</span><span class="sxs-lookup"><span data-stu-id="86088-260">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="86088-261">Dodaj *Pages/_ViewImports.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="86088-261">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="86088-262">`@namespace` znajduje się w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="86088-262">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="86088-263">`@addTagHelper` Dyrektywy dostarcza [wbudowanych pomocników tagów](xref:mvc/views/tag-helpers/builtin-th/Index) dla wszystkich stron w *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="86088-263">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="86088-264">Gdy `@namespace` dyrektywa jest używana jawnie na stronie:</span><span class="sxs-lookup"><span data-stu-id="86088-264">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="86088-265">Dyrektywa ustawia obszar nazw dla strony.</span><span class="sxs-lookup"><span data-stu-id="86088-265">The directive sets the namespace for the page.</span></span> <span data-ttu-id="86088-266">`@model` Dyrektywy nie musi obejmować przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="86088-266">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="86088-267">Gdy `@namespace` dyrektywy znajduje się w *_ViewImports.cshtml*, określonego obszaru nazw dostarcza prefiksu dla przestrzeni nazw wygenerowane na stronie, który importuje `@namespace` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="86088-267">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="86088-268">Pozostała część wygenerowanej (część sufiks) jest oddzielona kropkami ścieżki względnej między folder zawierający *_ViewImports.cshtml* i folderu zawierającego strony.</span><span class="sxs-lookup"><span data-stu-id="86088-268">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="86088-269">Na przykład kodzie pliku *Pages/Customers/Edit.cshtml.cs* jawnie Ustawia obszar nazw:</span><span class="sxs-lookup"><span data-stu-id="86088-269">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="86088-270">*Pages/_ViewImports.cshtml* pliku ustawia następujących nazw:</span><span class="sxs-lookup"><span data-stu-id="86088-270">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="86088-271">Wygenerowany obszar nazw dla *Pages/Customers/Edit.cshtml* Razor strony jest taka sama jak pliku CodeBehind.</span><span class="sxs-lookup"><span data-stu-id="86088-271">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="86088-272">`@namespace` Dyrektywa została zaprojektowana tak klasy C# dodane do projektu i stron wygenerowany kod *tylko pracy* bez dodawania `@using` dyrektywy do pliku CodeBehind.</span><span class="sxs-lookup"><span data-stu-id="86088-272">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="86088-273">`@namespace` *współdziała również z konwencjonalnej widokami Razor.*</span><span class="sxs-lookup"><span data-stu-id="86088-273">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="86088-274">Oryginalna *Pages/Create.cshtml* pliku widoku:</span><span class="sxs-lookup"><span data-stu-id="86088-274">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="86088-275">Zaktualizowany interfejs *Pages/Create.cshtml* pliku widoku:</span><span class="sxs-lookup"><span data-stu-id="86088-275">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="86088-276">[Projektu starter stron Razor](#rpvs17) zawiera *Pages/_ValidationScriptsPartial.cshtml*, który przechwytuje sprawdzania poprawności po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="86088-276">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="86088-277">Generowania adresu URL dla stron</span><span class="sxs-lookup"><span data-stu-id="86088-277">URL generation for Pages</span></span>

<span data-ttu-id="86088-278">`Create` Strony pokazana wcześniej, używa `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="86088-278">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="86088-279">Aplikacja ma następującą strukturę plik lub folder:</span><span class="sxs-lookup"><span data-stu-id="86088-279">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="86088-280">*/ Stron*</span><span class="sxs-lookup"><span data-stu-id="86088-280">*/Pages*</span></span>

  * <span data-ttu-id="86088-281">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="86088-281">*Index.cshtml*</span></span>
  * <span data-ttu-id="86088-282">*/ Klientów*</span><span class="sxs-lookup"><span data-stu-id="86088-282">*/Customers*</span></span>

    * <span data-ttu-id="86088-283">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="86088-283">*Create.cshtml*</span></span>
    * <span data-ttu-id="86088-284">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="86088-284">*Edit.cshtml*</span></span>
    * <span data-ttu-id="86088-285">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="86088-285">*Index.cshtml*</span></span>

<span data-ttu-id="86088-286">*Pages/Customers/Create.cshtml* i *Pages/Customers/Edit.cshtml* stron przekierowania do *Pages/Index.cshtml* po pomyślnym.</span><span class="sxs-lookup"><span data-stu-id="86088-286">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="86088-287">Ciąg `/Index` jest częścią identyfikatora URI uzyskać dostęp do poprzedniej strony.</span><span class="sxs-lookup"><span data-stu-id="86088-287">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="86088-288">Ciąg `/Index` może służyć do generowania identyfikatorów URI *Pages/Index.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="86088-288">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="86088-289">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="86088-289">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="86088-290">Nazwa strony jest ścieżka do strony z katalogu głównego */strony* folderu, w tym na początku `/` (na przykład `/Index`).</span><span class="sxs-lookup"><span data-stu-id="86088-290">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="86088-291">Poprzedniej próbki generowania adresu URL oferują rozszerzoną opcje i funkcji za pośrednictwem hardcoding adresu URL.</span><span class="sxs-lookup"><span data-stu-id="86088-291">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="86088-292">Używa generowania adresu URL [routingu](xref:mvc/controllers/routing) i generowanie i kodowanie parametry zgodnie z sposób definiowania trasy w ścieżce docelowej.</span><span class="sxs-lookup"><span data-stu-id="86088-292">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="86088-293">Generowania adresu URL dla stron obsługuje nazw względnych.</span><span class="sxs-lookup"><span data-stu-id="86088-293">URL generation for pages supports relative names.</span></span> <span data-ttu-id="86088-294">W poniższej tabeli przedstawiono stronę indeksu, która jest zaznaczone z różnych `RedirectToPage` parametry z *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="86088-294">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="86088-295">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="86088-295">RedirectToPage(x)</span></span>| <span data-ttu-id="86088-296">Strona</span><span class="sxs-lookup"><span data-stu-id="86088-296">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="86088-297">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="86088-297">RedirectToPage("/Index")</span></span> | <span data-ttu-id="86088-298">*Strony/indeksu*</span><span class="sxs-lookup"><span data-stu-id="86088-298">*Pages/Index*</span></span> |
| <span data-ttu-id="86088-299">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="86088-299">RedirectToPage("./Index");</span></span> | <span data-ttu-id="86088-300">*Strony/klientów/indeksu*</span><span class="sxs-lookup"><span data-stu-id="86088-300">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="86088-301">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="86088-301">RedirectToPage("../Index")</span></span> | <span data-ttu-id="86088-302">*Strony/indeksu*</span><span class="sxs-lookup"><span data-stu-id="86088-302">*Pages/Index*</span></span> |
| <span data-ttu-id="86088-303">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="86088-303">RedirectToPage("Index")</span></span>  | <span data-ttu-id="86088-304">*Strony/klientów/indeksu*</span><span class="sxs-lookup"><span data-stu-id="86088-304">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="86088-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, i `RedirectToPage("../Index")` są <em>względne nazwy</em>.</span><span class="sxs-lookup"><span data-stu-id="86088-305">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are <em>relative names</em>.</span></span> <span data-ttu-id="86088-306">`RedirectToPage` Parametr jest <em>łączyć</em> ze ścieżką bieżącej strony do obliczenia nazwę strony docelowej.</span><span class="sxs-lookup"><span data-stu-id="86088-306">The `RedirectToPage` parameter is <em>combined</em> with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="86088-307">Nazwa względna konsolidacji jest przydatne, gdy tworzenie witryn ze strukturą złożonych.</span><span class="sxs-lookup"><span data-stu-id="86088-307">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="86088-308">Jeśli używasz nazwy względne do połączenia między stronami w folderze, można zmienić nazwę tego folderu.</span><span class="sxs-lookup"><span data-stu-id="86088-308">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="86088-309">Wszystkie linki nadal działać (ponieważ one nie obejmować nazwę folderu).</span><span class="sxs-lookup"><span data-stu-id="86088-309">All the links still work (because they didn't include the folder name).</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="viewdata-attribute"></a><span data-ttu-id="86088-310">Atrybut viewData</span><span class="sxs-lookup"><span data-stu-id="86088-310">ViewData attribute</span></span>

<span data-ttu-id="86088-311">Dane mogą być przekazywane do strony z [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="86088-311">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="86088-312">Właściwości w kontrolerach ani w modelach Razor strony ozdobione `[ViewData]` ich wartości przechowywane i załadowane z [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="86088-312">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="86088-313">W poniższym przykładzie `AboutModel` zawiera `Title` ozdobione właściwości `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="86088-313">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="86088-314">`Title` Właściwość jest ustawiona na tytuł strony informacje:</span><span class="sxs-lookup"><span data-stu-id="86088-314">The `Title` property is set to the title of the About page:</span></span>

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

<span data-ttu-id="86088-315">Na stronie informacje dostępu `Title` właściwości jako właściwość modelu:</span><span class="sxs-lookup"><span data-stu-id="86088-315">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="86088-316">W układzie tytuł jest do odczytu ze słownika ViewData:</span><span class="sxs-lookup"><span data-stu-id="86088-316">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```
::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="86088-317">TempData</span><span class="sxs-lookup"><span data-stu-id="86088-317">TempData</span></span>

<span data-ttu-id="86088-318">Przedstawia platformy ASP.NET Core [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) właściwość [kontrolera](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="86088-318">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="86088-319">Ta właściwość przechowuje dane, dopóki nie jest do odczytu.</span><span class="sxs-lookup"><span data-stu-id="86088-319">This property stores data until it's read.</span></span> <span data-ttu-id="86088-320">`Keep` i `Peek` metod można użyć do sprawdzenia danych bez usuwania.</span><span class="sxs-lookup"><span data-stu-id="86088-320">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="86088-321">`TempData` jest przydatne w przypadku przekierowania, gdy dane są potrzebne dla więcej niż jednego żądania.</span><span class="sxs-lookup"><span data-stu-id="86088-321">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="86088-322">`[TempData]` Jest nowy w programie ASP.NET 2.0 Core i jest obsługiwane na stronach i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="86088-322">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="86088-323">Poniższy kod ustawia wartość `Message` przy użyciu `TempData`:</span><span class="sxs-lookup"><span data-stu-id="86088-323">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="86088-324">Następujący kod w *Pages/Customers/Index.cshtml* plik zawiera wartość `Message` przy użyciu `TempData`.</span><span class="sxs-lookup"><span data-stu-id="86088-324">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="86088-325">*Pages/Customers/Index.cshtml.cs* stosuje modelu strony `[TempData]` atrybutu `Message` właściwości.</span><span class="sxs-lookup"><span data-stu-id="86088-325">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="86088-326">Zobacz [TempData](xref:fundamentals/app-state#tempdata) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="86088-326">See [TempData](xref:fundamentals/app-state#tempdata) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="86088-327">Wielu obsług na stronie</span><span class="sxs-lookup"><span data-stu-id="86088-327">Multiple handlers per page</span></span>

<span data-ttu-id="86088-328">Następująca strona generuje kod znaczników dla dwie strony przy użyciu programów obsługi `asp-page-handler` pomocnika tagów:</span><span class="sxs-lookup"><span data-stu-id="86088-328">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="86088-329">Formularz w poprzednim przykładzie ma dwa przesłać przyciski, za pomocą każdej `FormActionTagHelper` do przesłania do innego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="86088-329">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="86088-330">`asp-page-handler` Atrybut jest dodatek do `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="86088-330">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="86088-331">`asp-page-handler` generuje adresów URL, które przesłania do każdej z metod obsługi zdefiniowane przez stronę.</span><span class="sxs-lookup"><span data-stu-id="86088-331">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="86088-332">`asp-page` nie jest określony, ponieważ próbki jest konsolidacja do bieżącej strony.</span><span class="sxs-lookup"><span data-stu-id="86088-332">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="86088-333">Model strony:</span><span class="sxs-lookup"><span data-stu-id="86088-333">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="86088-334">W poprzednim kodzie użyto *o nazwie metod obsługi*.</span><span class="sxs-lookup"><span data-stu-id="86088-334">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="86088-335">Metody o nazwie procedury obsługi są tworzone przez pobieranie tekstu w nazwie po `On<HTTP Verb>` i przed `Async` (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="86088-335">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="86088-336">W powyższym przykładzie metody strony są OnPost**JoinList**Async i OnPost**JoinListUC**asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="86088-336">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="86088-337">Z *OnPost* i *Async* usunięta, nazwy programu obsługi są `JoinList` i `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="86088-337">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="86088-338">Przy użyciu poprzedniego kodu ścieżkę adresu URL, który przesyła do `OnPostJoinListAsync` jest `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="86088-338">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="86088-339">Ścieżka adresu URL, który przesyła do `OnPostJoinListUCAsync` jest `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="86088-339">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="86088-340">Dostosowywanie routingu</span><span class="sxs-lookup"><span data-stu-id="86088-340">Customizing Routing</span></span>

<span data-ttu-id="86088-341">Ciąg zapytania można zmienić `?handler=JoinList` w adresie URL do segmentu trasy `/JoinList` , określając szablon trasy `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="86088-341">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="86088-342">Jeśli nie chcesz, ciąg zapytania `?handler=JoinList` w adresie URL, można zmienić trasy, aby umieścić nazwę programu obsługi w części ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="86088-342">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="86088-343">Trasy można dostosować, dodając szablonu trasy ująć w podwójny cudzysłów po `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="86088-343">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="86088-344">Przy użyciu poprzedniego kodu ścieżkę adresu URL, który przesyła do `OnPostJoinListAsync` jest `http://localhost:5000/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="86088-344">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="86088-345">Ścieżka adresu URL, który przesyła do `OnPostJoinListUCAsync` jest `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="86088-345">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="86088-346">`?` Następujące `handler` oznacza, że parametr trasy jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="86088-346">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="86088-347">Można użyć `@page` dołączyć segmentów i parametrów do trasy domyślnej strony.</span><span class="sxs-lookup"><span data-stu-id="86088-347">You can use `@page` to append segments and parameters to a page's default route.</span></span> <span data-ttu-id="86088-348">Zmiana trasy strony przy użyciu ścieżki bezwzględnej lub wirtualnych (takie jak `"~/Some/Other/Path"`) nie jest obsługiwany.</span><span class="sxs-lookup"><span data-stu-id="86088-348">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) isn't supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="86088-349">Konfiguracja i ustawienia</span><span class="sxs-lookup"><span data-stu-id="86088-349">Configuration and settings</span></span>

<span data-ttu-id="86088-350">Aby skonfigurować opcje zaawansowane, użyj metody rozszerzenia `AddRazorPagesOptions` w Konstruktorze MVC:</span><span class="sxs-lookup"><span data-stu-id="86088-350">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="86088-351">Obecnie można użyć `RazorPagesOptions` Ustaw katalog główny strony lub Dodaj konwencje modelu aplikacji dla stron.</span><span class="sxs-lookup"><span data-stu-id="86088-351">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="86088-352">Firma Microsoft będzie włączyć więcej rozszerzeń w ten sposób w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="86088-352">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="86088-353">Wstępnej kompilacji widoków, zobacz [kompilacji widoku Razor](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="86088-353">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="86088-354">[Pobrania lub wyświetlenia przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="86088-354">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="86088-355">Zobacz [wprowadzenie stron Razor](xref:tutorials/razor-pages/razor-pages-start), która opiera się na to wprowadzenie.</span><span class="sxs-lookup"><span data-stu-id="86088-355">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="86088-356">Określ, czy stron Razor zawartości katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="86088-356">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="86088-357">Domyślnie stron Razor są umieszczone w */strony* katalogu.</span><span class="sxs-lookup"><span data-stu-id="86088-357">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="86088-358">Dodaj [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) stron Razor są zawartości katalogu głównego ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) aplikacji:</span><span class="sxs-lookup"><span data-stu-id="86088-358">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="86088-359">Określ, czy w katalogu głównym niestandardowych stron Razor</span><span class="sxs-lookup"><span data-stu-id="86088-359">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="86088-360">Dodaj [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) do określenia, czy stron Razor w katalogu głównym niestandardowych w aplikacji (podaj ścieżkę względną):</span><span class="sxs-lookup"><span data-stu-id="86088-360">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="86088-361">Zobacz także</span><span class="sxs-lookup"><span data-stu-id="86088-361">See also</span></span>

* [<span data-ttu-id="86088-362">Wprowadzenie do platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="86088-362">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="86088-363">Składnia Razor</span><span class="sxs-lookup"><span data-stu-id="86088-363">Razor syntax</span></span>](xref:mvc/views/razor)
* [<span data-ttu-id="86088-364">Wprowadzenie do korzystania ze stron Razor</span><span class="sxs-lookup"><span data-stu-id="86088-364">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="86088-365">Konwencje autoryzacji stron razor</span><span class="sxs-lookup"><span data-stu-id="86088-365">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="86088-366">Razor strony trasy i strony modelu dostawców niestandardowych</span><span class="sxs-lookup"><span data-stu-id="86088-366">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-conventions)
* [<span data-ttu-id="86088-367">Testy jednostkowe stron razor</span><span class="sxs-lookup"><span data-stu-id="86088-367">Razor Pages unit tests</span></span>](xref:test/razor-pages-tests)
