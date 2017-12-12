---
title: Wprowadzenie do platformy ASP.NET Core stron Razor
author: Rick-Anderson
description: "Ten dokument zawiera omówienie używanie stron Razor w platformy ASP.NET Core, aby ułatwić projektowanie strony scenariuszy."
keywords: Platformy ASP.NET Core stron Razor
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: a66b5ea32c2090b9944cd61f90f7fe011a823e82
ms.sourcegitcommit: 3511552becb081fb860a23d6c9b6c4efcab74577
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="c5136-104">Wprowadzenie do platformy ASP.NET Core stron Razor</span><span class="sxs-lookup"><span data-stu-id="c5136-104">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="c5136-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="c5136-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="c5136-106">Stron razor to nowa funkcja platformy ASP.NET Core MVC umożliwia kodowanie strony scenariusze łatwiejsze i bardziej wydajnej pracy.</span><span class="sxs-lookup"><span data-stu-id="c5136-106">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="c5136-107">Jeśli szukasz samouczka, który korzysta z podejścia Model-View-Controller, zobacz [wprowadzenie do platformy ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="c5136-107">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="c5136-108">Ten dokument zawiera wprowadzenie do stron Razor.</span><span class="sxs-lookup"><span data-stu-id="c5136-108">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="c5136-109">Nie jest samouczek krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="c5136-109">It's not a step by step tutorial.</span></span> <span data-ttu-id="c5136-110">Jeśli niektóre sekcje trudne do wykonania, zobacz [wprowadzenie stron Razor](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="c5136-110">If you find some of the sections difficult to follow, see [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="c5136-111">Wymagania wstępne platformy ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="c5136-111">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="c5136-112">Zainstaluj [.NET Core](https://www.microsoft.com/net/core) 2.0.0 lub nowszym.</span><span class="sxs-lookup"><span data-stu-id="c5136-112">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="c5136-113">Jeśli używasz programu Visual Studio, zainstaluj [programu Visual Studio](https://www.visualstudio.com/vs/) 2017 wersji 15.3 lub nowszym z następujące obciążenia:</span><span class="sxs-lookup"><span data-stu-id="c5136-113">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="c5136-114">**ASP.NET i sieć web development**</span><span class="sxs-lookup"><span data-stu-id="c5136-114">**ASP.NET and web development**</span></span>
* <span data-ttu-id="c5136-115">**Programowanie wieloplatformowych .NET core**</span><span class="sxs-lookup"><span data-stu-id="c5136-115">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="c5136-116">Tworzenie projektu stron Razor</span><span class="sxs-lookup"><span data-stu-id="c5136-116">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c5136-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5136-117">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="c5136-118">Zobacz [wprowadzenie stron Razor](xref:tutorials/razor-pages/razor-pages-start) szczegółowe informacje dotyczące sposobu tworzenia stron Razor projektu za pomocą programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5136-118">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c5136-119">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="c5136-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="c5136-120">Uruchom `dotnet new razor` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="c5136-120">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="c5136-121">Otwórz wygenerowany *.csproj* plików z programu Visual Studio dla komputerów Mac.</span><span class="sxs-lookup"><span data-stu-id="c5136-121">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c5136-122">Kod Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5136-122">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="c5136-123">Uruchom `dotnet new razor` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="c5136-123">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c5136-124">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c5136-124">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="c5136-125">Uruchom `dotnet new razor` z wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="c5136-125">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="c5136-126">Stron razor</span><span class="sxs-lookup"><span data-stu-id="c5136-126">Razor Pages</span></span>

<span data-ttu-id="c5136-127">Stron razor jest włączone w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c5136-127">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="c5136-128">Należy wziąć pod uwagę strony podstawowej:<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="c5136-128">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="c5136-129">Poprzedni kod znacznie wygląda jak plik widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="c5136-129">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="c5136-130">Jest to, co ułatwia różnych `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="c5136-130">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="c5136-131">`@page`powoduje, że plik na akcję MVC — czyli obsługi żądań bezpośrednio, bez przechodzenia przez kontroler.</span><span class="sxs-lookup"><span data-stu-id="c5136-131">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="c5136-132">`@page`musi być pierwszym dyrektywy Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="c5136-132">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="c5136-133">`@page`wpływa na działanie innych konstrukcji Razor.</span><span class="sxs-lookup"><span data-stu-id="c5136-133">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="c5136-134">Podobne strony przy użyciu `PageModel` klasa, jest wyświetlany w obszarze następujące dwa pliki.</span><span class="sxs-lookup"><span data-stu-id="c5136-134">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="c5136-135">*Pages/Index2.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="c5136-135">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="c5136-136">*Pages/Index2.cshtml.cs* "kodem" pliku:</span><span class="sxs-lookup"><span data-stu-id="c5136-136">The *Pages/Index2.cshtml.cs* "code-behind" file:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="c5136-137">Według konwencji `PageModel` pliku klasy ma taką samą nazwę jak plik Razor strony z *.cs* dołączane.</span><span class="sxs-lookup"><span data-stu-id="c5136-137">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="c5136-138">Na przykład na poprzedniej stronie aparatu Razor jest *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c5136-138">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="c5136-139">Plik zawierający `PageModel` nosi nazwę klasy *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="c5136-139">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="c5136-140">Skojarzenia ścieżki adresu URL do strony zależą od lokalizacji strony w systemie plików.</span><span class="sxs-lookup"><span data-stu-id="c5136-140">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="c5136-141">W poniższej tabeli przedstawiono ścieżki Razor strony i dopasowywania adresu URL:</span><span class="sxs-lookup"><span data-stu-id="c5136-141">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="c5136-142">Nazwa i ścieżka pliku</span><span class="sxs-lookup"><span data-stu-id="c5136-142">File name and path</span></span>               | <span data-ttu-id="c5136-143">Dopasowywanie adresu URL</span><span class="sxs-lookup"><span data-stu-id="c5136-143">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="c5136-144">*/Pages/index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c5136-144">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="c5136-145">`/`lub`/Index`</span><span class="sxs-lookup"><span data-stu-id="c5136-145">`/` or `/Index`</span></span> |
| <span data-ttu-id="c5136-146">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c5136-146">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="c5136-147">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c5136-147">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="c5136-148">*/Pages/Store/index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c5136-148">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="c5136-149">`/Store`lub`/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="c5136-149">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="c5136-150">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="c5136-150">Notes:</span></span>

* <span data-ttu-id="c5136-151">Środowisko uruchomieniowe wyszukuje pliki stron Razor w *stron* folderu domyślnie.</span><span class="sxs-lookup"><span data-stu-id="c5136-151">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="c5136-152">`Index`jest domyślną stronę, gdy adres URL nie zawiera strony.</span><span class="sxs-lookup"><span data-stu-id="c5136-152">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="c5136-153">Zapisywanie formularza podstawowego</span><span class="sxs-lookup"><span data-stu-id="c5136-153">Writing a basic form</span></span>

<span data-ttu-id="c5136-154">Funkcje stron razor ułatwiają typowe wzorce używane w przeglądarkach łatwe.</span><span class="sxs-lookup"><span data-stu-id="c5136-154">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="c5136-155">[Model powiązania](xref:mvc/models/model-binding), [pomocników tagów](xref:mvc/views/tag-helpers/intro)i wszystkich pomocników HTML *tylko pracy* z właściwościami zdefiniowana w klasie Razor strony.</span><span class="sxs-lookup"><span data-stu-id="c5136-155">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="c5136-156">Należy wziąć pod uwagę strona, która implementuje podstawowego "Skontaktuj się z nami" tworzą dla `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="c5136-156">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="c5136-157">Aby wyświetlić przykłady w tym dokumencie `DbContext` został zainicjowany w [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) pliku.</span><span class="sxs-lookup"><span data-stu-id="c5136-157">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="c5136-158">Model danych:</span><span class="sxs-lookup"><span data-stu-id="c5136-158">The data model:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="c5136-159">Kontekst bazy danych:</span><span class="sxs-lookup"><span data-stu-id="c5136-159">The db context:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="c5136-160">*Pages/Create.cshtml* pliku widoku:</span><span class="sxs-lookup"><span data-stu-id="c5136-160">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="c5136-161">*Pages/Create.cshtml.cs* pliku CodeBehind dla widoku:</span><span class="sxs-lookup"><span data-stu-id="c5136-161">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="c5136-162">Według konwencji `PageModel` nosi nazwę klasy `<PageName>Model` i znajduje się w tej samej przestrzeni nazw jako strony.</span><span class="sxs-lookup"><span data-stu-id="c5136-162">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="c5136-163">`PageModel` Klasa umożliwia oddzielenie logiki strony z jego prezentacji.</span><span class="sxs-lookup"><span data-stu-id="c5136-163">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="c5136-164">Definiuje stronę obsługi żądania wysyłane na stronie i dane używane do renderowania strony.</span><span class="sxs-lookup"><span data-stu-id="c5136-164">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="c5136-165">Ta separacja umożliwia zarządzanie zależności strony za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection) i [testu jednostkowego](xref:testing/razor-pages-testing) stron.</span><span class="sxs-lookup"><span data-stu-id="c5136-165">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="c5136-166">Na stronie `OnPostAsync` *metoda obsługi*, która działa w `POST` żąda (gdy użytkownik zapisuje formularz).</span><span class="sxs-lookup"><span data-stu-id="c5136-166">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="c5136-167">Można dodać obsługi metod dla dowolnej zlecenie HTTP.</span><span class="sxs-lookup"><span data-stu-id="c5136-167">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="c5136-168">Są najczęściej programów obsługi:</span><span class="sxs-lookup"><span data-stu-id="c5136-168">The most common handlers are:</span></span>

* <span data-ttu-id="c5136-169">`OnGet`Aby zainicjować stanu potrzebne dla strony.</span><span class="sxs-lookup"><span data-stu-id="c5136-169">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="c5136-170">[OnGet](#OnGet) próbki.</span><span class="sxs-lookup"><span data-stu-id="c5136-170">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="c5136-171">`OnPost`do obsługi przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="c5136-171">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="c5136-172">`Async` Sufiks nazwy jest opcjonalne, ale często używane przez Konwencję dla funkcji asynchronicznej.</span><span class="sxs-lookup"><span data-stu-id="c5136-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="c5136-173">`OnPostAsync` Kodu w poprzednim przykładzie wygląda podobnie do co zwykle piszesz w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="c5136-173">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="c5136-174">Poprzedni kod jest typowe dla stron Razor.</span><span class="sxs-lookup"><span data-stu-id="c5136-174">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="c5136-175">Większość podstawowych MVC, takich jak [modelu powiązania](xref:mvc/models/model-binding), [weryfikacji](xref:mvc/models/validation), i są współdzielone wyników akcji.</span><span class="sxs-lookup"><span data-stu-id="c5136-175">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="c5136-176">Poprzedni `OnPostAsync` metody:</span><span class="sxs-lookup"><span data-stu-id="c5136-176">The previous `OnPostAsync` method:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="c5136-177">Podstawowy przepływ `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="c5136-177">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="c5136-178">Sprawdź błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="c5136-178">Check for validation errors.</span></span>

*  <span data-ttu-id="c5136-179">Jeśli nie ma żadnych błędów, Zapisz dane i przekierowanie.</span><span class="sxs-lookup"><span data-stu-id="c5136-179">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="c5136-180">Jeśli wystąpią błędy, Wyświetl stronę ponownie, podając komunikatów dotyczących sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="c5136-180">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="c5136-181">Weryfikacja po stronie klienta jest identyczna jak tradycyjne aplikacje platformy ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="c5136-181">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="c5136-182">W wielu przypadkach błędy sprawdzania poprawności może być wykryte na komputerze klienckim i nigdy nie zostały przekazane do serwera.</span><span class="sxs-lookup"><span data-stu-id="c5136-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="c5136-183">Po pomyślnym wprowadzeniu danych `OnPostAsync` wywołań metody obsługi `RedirectToPage` metody pomocnika do zwrócenia wystąpienia klasy `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="c5136-183">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="c5136-184">`RedirectToPage`jest nowy wynik akcji, podobnie jak `RedirectToAction` lub `RedirectToRoute`, ale dostosowanych stron.</span><span class="sxs-lookup"><span data-stu-id="c5136-184">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="c5136-185">W poprzednim przykładzie przekierowuje do strony indeksu głównego (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="c5136-185">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="c5136-186">`RedirectToPage`została szczegółowo opisana w [generowania adresu URL dla stron](#url_gen) sekcji.</span><span class="sxs-lookup"><span data-stu-id="c5136-186">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="c5136-187">Jeśli przesłanego formularza zawiera błędy sprawdzania poprawności (które są przekazywane do serwera),`OnPostAsync` wywołań metody obsługi `Page` metody pomocnika.</span><span class="sxs-lookup"><span data-stu-id="c5136-187">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="c5136-188">`Page`Zwraca wystąpienie klasy `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="c5136-188">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="c5136-189">Zwracanie `Page` jest podobny do sposób zwrócenia akcje w kontrolerach `View`.</span><span class="sxs-lookup"><span data-stu-id="c5136-189">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="c5136-190">`PageResult`Wartość domyślna to <!-- Review  --> zwracany typ metody obsługi.</span><span class="sxs-lookup"><span data-stu-id="c5136-190">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="c5136-191">Metoda obsługi, która zwraca `void` renderuje stronę.</span><span class="sxs-lookup"><span data-stu-id="c5136-191">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="c5136-192">`Customer` Używa właściwości `[BindProperty]` atrybutu korzystania z wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="c5136-192">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="c5136-193">Stron razor domyślnie powiązania właściwości tylko z innych niż GET zleceń.</span><span class="sxs-lookup"><span data-stu-id="c5136-193">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="c5136-194">Powiązanie właściwości może zmniejszyć ilość kodu, które trzeba zapisać.</span><span class="sxs-lookup"><span data-stu-id="c5136-194">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="c5136-195">Powiązanie zmniejsza kodu przy użyciu tej samej właściwości do renderowania pól formularza (`<input asp-for="Customer.Name" />`) i akceptuje dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="c5136-195">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="c5136-196">Strona główna (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="c5136-196">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="c5136-197">Kod związany z *Index.cshtml.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="c5136-197">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="c5136-198">*Index.cshtml* pliku zawiera następujące znaczniki, aby utworzyć link edycji dla każdego kontakt:</span><span class="sxs-lookup"><span data-stu-id="c5136-198">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="c5136-199">[Pomocnika Tag kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) używane [asp - route - {wartość value}](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#route) atrybut do generowania łącza do edycji strony.</span><span class="sxs-lookup"><span data-stu-id="c5136-199">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#route) attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="c5136-200">Ten link zawiera dane trasy, o kontakt identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="c5136-200">The link contains route data with the contact ID.</span></span> <span data-ttu-id="c5136-201">Na przykład `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="c5136-201">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="c5136-202">*Pages/Edit.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="c5136-202">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="c5136-203">Pierwszy wiersz zawiera `@page "{id:int}"` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="c5136-203">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="c5136-204">Ograniczenie routingu`"{id:int}"` informuje strony do akceptowania żądań do strony, które zawierają `int` danych trasy.</span><span class="sxs-lookup"><span data-stu-id="c5136-204">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="c5136-205">Jeśli żądanie do strony nie zawiera danych trasy, który może zostać przekonwertowany na `int`, środowisko uruchomieniowe zwraca błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="c5136-205">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="c5136-206">*Pages/Edit.cshtml.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="c5136-206">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="c5136-207">*Index.cshtml* plik zawiera także znaczników do utworzenia przycisk Usuń widoczny dla każdego kontakt klienta:</span><span class="sxs-lookup"><span data-stu-id="c5136-207">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="c5136-208">Przycisk usuwania jest renderowany w języku HTML, jego `formaction` zawiera parametry:</span><span class="sxs-lookup"><span data-stu-id="c5136-208">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="c5136-209">Określony przez identyfikator kontaktu klienta z `asp-route-id` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="c5136-209">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="c5136-210">`handler` Określonego przez `asp-page-handler` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="c5136-210">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="c5136-211">Poniżej przedstawiono przykładowy przycisk usuwania renderowany z klientem skontaktuj się z Identyfikatorem `1`:</span><span class="sxs-lookup"><span data-stu-id="c5136-211">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="c5136-212">Jeśli przycisk jest zaznaczony, formularz `POST` wysłaniu żądania do serwera.</span><span class="sxs-lookup"><span data-stu-id="c5136-212">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="c5136-213">Według Konwencji wybrano nazwę metody obsługi na podstawie wartości `handler` parametru zgodnie ze schematem `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="c5136-213">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="c5136-214">Ponieważ `handler` jest `delete` w tym przykładzie `OnPostDeleteAsync` metoda obsługi jest używany do procesu `POST` żądania.</span><span class="sxs-lookup"><span data-stu-id="c5136-214">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="c5136-215">Jeśli `asp-page-handler` ustawiono innej wartości, takich jak `remove`, strona metody obsługi o nazwie `OnPostRemoveAsync` jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="c5136-215">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="c5136-216">`OnPostDeleteAsync` Metody:</span><span class="sxs-lookup"><span data-stu-id="c5136-216">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="c5136-217">Akceptuje `id` z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="c5136-217">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="c5136-218">Wysyła zapytanie do bazy danych dla klienta kontakt z `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="c5136-218">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="c5136-219">Jeśli zostanie znaleziony kontaktowe klienta, są one usunięte z listy kontaktów klienta.</span><span class="sxs-lookup"><span data-stu-id="c5136-219">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="c5136-220">Baza danych jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="c5136-220">The database is updated.</span></span>
* <span data-ttu-id="c5136-221">Wywołania `RedirectToPage` ma nastąpić przekierowanie do strony indeksu głównego (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="c5136-221">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="c5136-222">XSRF/CSRF i stron Razor</span><span class="sxs-lookup"><span data-stu-id="c5136-222">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="c5136-223">Nie masz do pisania kodu dla [antiforgery weryfikacji](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="c5136-223">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="c5136-224">Antiforgery generowania tokenów i weryfikacja są automatycznie umieszczane w stron Razor.</span><span class="sxs-lookup"><span data-stu-id="c5136-224">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="c5136-225">Za pomocą układów, częściowe, szablonów i pomocników tagów Razor strony</span><span class="sxs-lookup"><span data-stu-id="c5136-225">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="c5136-226">Strony działać z wszystkimi funkcjami aparatu widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="c5136-226">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="c5136-227">Układy, częściowe, szablony, pomocników tagów *_ViewStart.cshtml*, *_ViewImports.cshtml* pracy w taki sam sposób jak w przypadku konwencjonalnych widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="c5136-227">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="c5136-228">Korzystając z tych funkcji umożliwia declutter tej strony.</span><span class="sxs-lookup"><span data-stu-id="c5136-228">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="c5136-229">Dodaj [układ strony](xref:mvc/views/layout) do *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c5136-229">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="c5136-230">[Układu](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="c5136-230">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="c5136-231">Określa układ każdej strony (o ile nie zdecyduje strony poza układ).</span><span class="sxs-lookup"><span data-stu-id="c5136-231">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="c5136-232">Importuje struktury HTML, takich jak JavaScript i arkusze stylów.</span><span class="sxs-lookup"><span data-stu-id="c5136-232">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="c5136-233">Zobacz [układ strony](xref:mvc/views/layout) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="c5136-233">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="c5136-234">[Układu](xref:mvc/views/layout#specifying-a-layout) właściwość jest ustawiona *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c5136-234">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="c5136-235">**Uwaga:** układ jest *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="c5136-235">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="c5136-236">Strony wyszukać innych widoków (układy, szablony, częściowe) hierarchicznie, uruchamianie w tym samym folderze co bieżąca strona.</span><span class="sxs-lookup"><span data-stu-id="c5136-236">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="c5136-237">Układ w *stron* folderu można używać z dowolnej strony Razor, w obszarze *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="c5136-237">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="c5136-238">Firma Microsoft zaleca **nie** umieścić plik układu w *widoków/Shared* folderu.</span><span class="sxs-lookup"><span data-stu-id="c5136-238">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="c5136-239">*Widoki/Shared* jest wzorzec widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="c5136-239">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="c5136-240">Stron razor są przeznaczone do zależą od hierarchii folderów, nie ścieżkę Konwencji.</span><span class="sxs-lookup"><span data-stu-id="c5136-240">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="c5136-241">Widok wyszukiwania ze strony Razor obejmuje *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="c5136-241">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="c5136-242">Układy, szablonów i częściowe jest używany z konwencjonalnej Razor widoków i kontrolerów MVC *tylko pracy*.</span><span class="sxs-lookup"><span data-stu-id="c5136-242">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="c5136-243">Dodaj *Pages/_ViewImports.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="c5136-243">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="c5136-244">`@namespace`znajduje się w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="c5136-244">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="c5136-245">`@addTagHelper` Dyrektywy dostarcza [wbudowanych pomocników tagów](xref:mvc/views/tag-helpers/builtin-th/Index) dla wszystkich stron w *stron* folderu.</span><span class="sxs-lookup"><span data-stu-id="c5136-245">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="c5136-246">Gdy `@namespace` dyrektywa jest używana jawnie na stronie:</span><span class="sxs-lookup"><span data-stu-id="c5136-246">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="c5136-247">Dyrektywa ustawia obszar nazw dla strony.</span><span class="sxs-lookup"><span data-stu-id="c5136-247">The directive sets the namespace for the page.</span></span> <span data-ttu-id="c5136-248">`@model` Dyrektywy nie musi obejmować przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="c5136-248">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="c5136-249">Gdy `@namespace` dyrektywy znajduje się w *_ViewImports.cshtml*, określonego obszaru nazw dostarcza prefiksu dla przestrzeni nazw wygenerowane na stronie, który importuje `@namespace` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="c5136-249">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="c5136-250">Pozostała część wygenerowanej (część sufiks) jest oddzielona kropkami ścieżki względnej między folder zawierający *_ViewImports.cshtml* i folderu zawierającego strony.</span><span class="sxs-lookup"><span data-stu-id="c5136-250">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="c5136-251">Na przykład kodzie pliku *Pages/Customers/Edit.cshtml.cs* jawnie Ustawia obszar nazw:</span><span class="sxs-lookup"><span data-stu-id="c5136-251">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="c5136-252">*Pages/_ViewImports.cshtml* pliku ustawia następujących nazw:</span><span class="sxs-lookup"><span data-stu-id="c5136-252">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="c5136-253">Wygenerowany obszar nazw dla *Pages/Customers/Edit.cshtml* Razor strony jest taka sama jak pliku CodeBehind.</span><span class="sxs-lookup"><span data-stu-id="c5136-253">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="c5136-254">`@namespace` Dyrektywa została zaprojektowana tak klasy C# dodane do projektu i stron wygenerowany kod *tylko pracy* bez dodawania `@using` dyrektywy do pliku CodeBehind.</span><span class="sxs-lookup"><span data-stu-id="c5136-254">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="c5136-255">**Uwaga:** `@namespace` współdziała również z konwencjonalnej widokami Razor.</span><span class="sxs-lookup"><span data-stu-id="c5136-255">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="c5136-256">Oryginalna *Pages/Create.cshtml* pliku widoku:</span><span class="sxs-lookup"><span data-stu-id="c5136-256">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="c5136-257">Zaktualizowany interfejs *Pages/Create.cshtml* pliku widoku:</span><span class="sxs-lookup"><span data-stu-id="c5136-257">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="c5136-258">[Projektu starter stron Razor](#rpvs17) zawiera *Pages/_ValidationScriptsPartial.cshtml*, który przechwytuje sprawdzania poprawności po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="c5136-258">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="c5136-259">Generowania adresu URL dla stron</span><span class="sxs-lookup"><span data-stu-id="c5136-259">URL generation for Pages</span></span>

<span data-ttu-id="c5136-260">`Create` Strony pokazana wcześniej, używa `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="c5136-260">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="c5136-261">Aplikacja ma następującą strukturę plik lub folder:</span><span class="sxs-lookup"><span data-stu-id="c5136-261">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="c5136-262">*/ Stron*</span><span class="sxs-lookup"><span data-stu-id="c5136-262">*/Pages*</span></span>

  * <span data-ttu-id="c5136-263">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c5136-263">*Index.cshtml*</span></span>
  * <span data-ttu-id="c5136-264">*/ Klienta*</span><span class="sxs-lookup"><span data-stu-id="c5136-264">*/Customer*</span></span>

    * <span data-ttu-id="c5136-265">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c5136-265">*Create.cshtml*</span></span>
    * <span data-ttu-id="c5136-266">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c5136-266">*Edit.cshtml*</span></span>
    * <span data-ttu-id="c5136-267">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c5136-267">*Index.cshtml*</span></span>

<span data-ttu-id="c5136-268">*Pages/Customers/Create.cshtml* i *Pages/Customers/Edit.cshtml* stron przekierowania do *Pages/Index.cshtml* po pomyślnym.</span><span class="sxs-lookup"><span data-stu-id="c5136-268">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="c5136-269">Ciąg `/Index` jest częścią identyfikatora URI uzyskać dostęp do poprzedniej strony.</span><span class="sxs-lookup"><span data-stu-id="c5136-269">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="c5136-270">Ciąg `/Index` może służyć do generowania identyfikatorów URI *Pages/Index.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="c5136-270">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="c5136-271">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c5136-271">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="c5136-272">Nazwa strony jest ścieżka do strony z katalogu głównego */strony* folderze (w tym na początku `/`, na przykład `/Index`).</span><span class="sxs-lookup"><span data-stu-id="c5136-272">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="c5136-273">Poprzedniej próbki generowania adresu URL są bardziej zaawansowanej funkcji niż hardcoding tylko adres URL.</span><span class="sxs-lookup"><span data-stu-id="c5136-273">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="c5136-274">Używa generowania adresu URL [routingu](xref:mvc/controllers/routing) i generowanie i kodowanie parametry zgodnie z sposób definiowania trasy w ścieżce docelowej.</span><span class="sxs-lookup"><span data-stu-id="c5136-274">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="c5136-275">Generowania adresu URL dla stron obsługuje nazw względnych.</span><span class="sxs-lookup"><span data-stu-id="c5136-275">URL generation for pages supports relative names.</span></span> <span data-ttu-id="c5136-276">W poniższej tabeli przedstawiono stronę indeksu, która jest zaznaczone z różnych `RedirectToPage` parametry z *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c5136-276">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="c5136-277">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="c5136-277">RedirectToPage(x)</span></span>| <span data-ttu-id="c5136-278">Strona</span><span class="sxs-lookup"><span data-stu-id="c5136-278">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="c5136-279">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="c5136-279">RedirectToPage("/Index")</span></span> | <span data-ttu-id="c5136-280">*Strony/indeksu*</span><span class="sxs-lookup"><span data-stu-id="c5136-280">*Pages/Index*</span></span> |
| <span data-ttu-id="c5136-281">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="c5136-281">RedirectToPage("./Index");</span></span> | <span data-ttu-id="c5136-282">*Strony/klientów/indeksu*</span><span class="sxs-lookup"><span data-stu-id="c5136-282">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="c5136-283">RedirectToPage(".. / Indeks")</span><span class="sxs-lookup"><span data-stu-id="c5136-283">RedirectToPage("../Index")</span></span> | <span data-ttu-id="c5136-284">*Strony/indeksu*</span><span class="sxs-lookup"><span data-stu-id="c5136-284">*Pages/Index*</span></span> |
| <span data-ttu-id="c5136-285">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="c5136-285">RedirectToPage("Index")</span></span>  | <span data-ttu-id="c5136-286">*Strony/klientów/indeksu*</span><span class="sxs-lookup"><span data-stu-id="c5136-286">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="c5136-287">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, i `RedirectToPage("../Index")` są *względne nazwy*.</span><span class="sxs-lookup"><span data-stu-id="c5136-287">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="c5136-288">`RedirectToPage` Parametr jest *łączyć* ze ścieżką bieżącej strony do obliczenia nazwę strony docelowej.</span><span class="sxs-lookup"><span data-stu-id="c5136-288">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="c5136-289">Nazwa względna konsolidacji jest przydatne, gdy tworzenie witryn ze strukturą złożonych.</span><span class="sxs-lookup"><span data-stu-id="c5136-289">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="c5136-290">Jeśli używasz nazwy względne do połączenia między stronami w folderze, można zmienić nazwę tego folderu.</span><span class="sxs-lookup"><span data-stu-id="c5136-290">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="c5136-291">Wszystkie linki nadal działać (ponieważ one nie obejmować nazwę folderu).</span><span class="sxs-lookup"><span data-stu-id="c5136-291">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="c5136-292">TempData</span><span class="sxs-lookup"><span data-stu-id="c5136-292">TempData</span></span>

<span data-ttu-id="c5136-293">Przedstawia platformy ASP.NET Core [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) właściwość [kontrolera](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="c5136-293">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="c5136-294">Ta właściwość przechowuje dane, dopóki nie jest do odczytu.</span><span class="sxs-lookup"><span data-stu-id="c5136-294">This property stores data until it is read.</span></span> <span data-ttu-id="c5136-295">`Keep` i `Peek` metod można użyć do sprawdzenia danych bez usuwania.</span><span class="sxs-lookup"><span data-stu-id="c5136-295">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="c5136-296">`TempData`jest przydatne w przypadku przekierowania, gdy dane są potrzebne dla więcej niż jednego żądania.</span><span class="sxs-lookup"><span data-stu-id="c5136-296">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="c5136-297">`[TempData]` Jest nowy w programie ASP.NET 2.0 Core i jest obsługiwane na stronach i kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="c5136-297">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="c5136-298">Poniższy kod ustawia wartość `Message` przy użyciu `TempData`:</span><span class="sxs-lookup"><span data-stu-id="c5136-298">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="c5136-299">Następujący kod w *Pages/Customers/Index.cshtml* plik zawiera wartość `Message` przy użyciu `TempData`.</span><span class="sxs-lookup"><span data-stu-id="c5136-299">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="c5136-300">*Pages/Customers/Index.cshtml.cs* dotyczy pliku CodeBehind `[TempData]` atrybutu `Message` właściwości.</span><span class="sxs-lookup"><span data-stu-id="c5136-300">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="c5136-301">Zobacz [TempData](xref:fundamentals/app-state#temp) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="c5136-301">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="c5136-302">Wielu obsług na stronie</span><span class="sxs-lookup"><span data-stu-id="c5136-302">Multiple handlers per page</span></span>

<span data-ttu-id="c5136-303">Następująca strona generuje kod znaczników dla dwie strony przy użyciu programów obsługi `asp-page-handler` pomocnika tagów:</span><span class="sxs-lookup"><span data-stu-id="c5136-303">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

<span data-ttu-id="c5136-304">Formularz w poprzednim przykładzie ma dwa przesłać przyciski, za pomocą każdej `FormActionTagHelper` do przesłania do innego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="c5136-304">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="c5136-305">`asp-page-handler` Atrybut jest dodatek do `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="c5136-305">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="c5136-306">`asp-page-handler`generuje adresów URL, które przesłania do każdej z metod obsługi zdefiniowane przez stronę.</span><span class="sxs-lookup"><span data-stu-id="c5136-306">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="c5136-307">`asp-page`nie określono ponieważ próbki jest konsolidacja do bieżącej strony.</span><span class="sxs-lookup"><span data-stu-id="c5136-307">`asp-page` is not specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="c5136-308">Plik CodeBehind:</span><span class="sxs-lookup"><span data-stu-id="c5136-308">The code-behind file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="c5136-309">W poprzednim kodzie użyto *o nazwie metod obsługi*.</span><span class="sxs-lookup"><span data-stu-id="c5136-309">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="c5136-310">Metody o nazwie procedury obsługi są tworzone przez pobieranie tekstu w nazwie po `On<HTTP Verb>` i przed `Async` (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="c5136-310">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="c5136-311">W powyższym przykładzie metody strony są OnPost**JoinList**Async i OnPost**JoinListUC**asynchronicznego.</span><span class="sxs-lookup"><span data-stu-id="c5136-311">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="c5136-312">Z *OnPost* i *Async* usunięta, nazwy programu obsługi są `JoinList` i `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="c5136-312">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="c5136-313">Przy użyciu poprzedniego kodu ścieżkę adresu URL, który przesyła do `OnPostJoinListAsync` jest `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="c5136-313">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="c5136-314">Ścieżka adresu URL, który przesyła do `OnPostJoinListUCAsync` jest `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="c5136-314">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="c5136-315">Dostosowywanie routingu</span><span class="sxs-lookup"><span data-stu-id="c5136-315">Customizing Routing</span></span>

<span data-ttu-id="c5136-316">Jeśli nie chcesz, ciąg zapytania `?handler=JoinList` w adresie URL, można zmienić trasy, aby umieścić nazwę programu obsługi w części ścieżki adresu URL.</span><span class="sxs-lookup"><span data-stu-id="c5136-316">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="c5136-317">Trasy można dostosować, dodając szablonu trasy ująć w podwójny cudzysłów po `@page` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="c5136-317">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="c5136-318">Trasa poprzedniego umieszcza nazwa programu obsługi ścieżki adresu URL zamiast ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="c5136-318">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="c5136-319">`?` Następujące `handler` oznacza, że parametr trasy jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="c5136-319">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="c5136-320">Można użyć `@page` dodać dodatkowe segmenty i parametrów do strony trasy.</span><span class="sxs-lookup"><span data-stu-id="c5136-320">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="c5136-321">Niezależnie od istnieje już jest **dołączany** do trasy domyślnej strony.</span><span class="sxs-lookup"><span data-stu-id="c5136-321">Whatever's there is **appended** to the default route of the page.</span></span> <span data-ttu-id="c5136-322">Zmiana trasy strony przy użyciu ścieżki bezwzględnej lub wirtualnych (takie jak `"~/Some/Other/Path"`) nie jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="c5136-322">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) is not supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="c5136-323">Konfiguracja i ustawienia</span><span class="sxs-lookup"><span data-stu-id="c5136-323">Configuration and settings</span></span>

<span data-ttu-id="c5136-324">Aby skonfigurować opcje zaawansowane, użyj metody rozszerzenia `AddRazorPagesOptions` w Konstruktorze MVC:</span><span class="sxs-lookup"><span data-stu-id="c5136-324">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="c5136-325">Obecnie można użyć `RazorPagesOptions` Ustaw katalog główny strony lub Dodaj konwencje modelu aplikacji dla stron.</span><span class="sxs-lookup"><span data-stu-id="c5136-325">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="c5136-326">Firma Microsoft będzie włączyć więcej rozszerzeń w ten sposób w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="c5136-326">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="c5136-327">Wstępnej kompilacji widoków, zobacz [kompilacji widoku Razor](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="c5136-327">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="c5136-328">[Pobrania lub wyświetlenia przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="c5136-328">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="c5136-329">Zobacz [wprowadzenie Razor strony platformy ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), która opiera się na to wprowadzenie.</span><span class="sxs-lookup"><span data-stu-id="c5136-329">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="c5136-330">Określ, czy stron Razor zawartości katalogu głównego</span><span class="sxs-lookup"><span data-stu-id="c5136-330">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="c5136-331">Domyślnie stron Razor są umieszczone w */strony* katalogu.</span><span class="sxs-lookup"><span data-stu-id="c5136-331">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="c5136-332">Dodaj [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) stron Razor są zawartości katalogu głównego ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) aplikacji:</span><span class="sxs-lookup"><span data-stu-id="c5136-332">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="c5136-333">Określ, czy w katalogu głównym niestandardowych stron Razor</span><span class="sxs-lookup"><span data-stu-id="c5136-333">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="c5136-334">Dodaj [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) do określenia, czy stron Razor w katalogu głównym niestandardowych w aplikacji (podaj ścieżkę względną):</span><span class="sxs-lookup"><span data-stu-id="c5136-334">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="c5136-335">Zobacz także</span><span class="sxs-lookup"><span data-stu-id="c5136-335">See also</span></span>

* [<span data-ttu-id="c5136-336">Wprowadzenie do stron Razor</span><span class="sxs-lookup"><span data-stu-id="c5136-336">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="c5136-337">Konwencje autoryzacji stron razor</span><span class="sxs-lookup"><span data-stu-id="c5136-337">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="c5136-338">Razor strony trasy i strony modelu dostawców niestandardowych</span><span class="sxs-lookup"><span data-stu-id="c5136-338">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="c5136-339">Jednostka stron razor i integracji testowania</span><span class="sxs-lookup"><span data-stu-id="c5136-339">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
