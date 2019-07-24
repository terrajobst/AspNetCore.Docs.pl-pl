---
title: Wprowadzenie do Razor Pages w ASP.NET Core
author: Rick-Anderson
description: Dowiedz się, jak stron Razor w programie ASP.NET Core umożliwia kodowania scenariuszy skoncentrowane na stronie łatwiejsze i bardziej wydajne niż przy użyciu platformy MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 04/06/2019
uid: razor-pages/index
ms.openlocfilehash: 406e89c96ea63493091d0287077e244faee5f730
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308006"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="bb09a-103">Wprowadzenie do Razor Pages w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bb09a-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="bb09a-104">[Rick Anderson](https://twitter.com/RickAndMSFT) i [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="bb09a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="bb09a-105">Razor Pages jest nowym aspektem ASP.NET Core MVC, który sprawia, że kodowanie scenariuszy ukierunkowanych na strony jest łatwiejsze i bardziej produktywne.</span><span class="sxs-lookup"><span data-stu-id="bb09a-105">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="bb09a-106">Jeśli szukasz samouczka korzystającego z podejścia Model-View-Controller, zobacz Wprowadzenie do [ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="bb09a-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="bb09a-107">Ten dokument zawiera wprowadzenie do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="bb09a-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="bb09a-108">Nie jest to samouczek krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="bb09a-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="bb09a-109">Jeśli okaże się, że niektóre sekcje są zbyt zaawansowane, zobacz [wprowadzenie do Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="bb09a-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="bb09a-110">Aby zapoznać się z omówieniem ASP.NET Core, zobacz [wprowadzenie do ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="bb09a-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bb09a-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="bb09a-111">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bb09a-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb09a-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bb09a-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bb09a-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bb09a-114">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="bb09a-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="bb09a-115">Tworzenie projektu Razor Pages</span><span class="sxs-lookup"><span data-stu-id="bb09a-115">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bb09a-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bb09a-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bb09a-117">Aby uzyskać szczegółowe instrukcje dotyczące sposobu tworzenia projektu Razor Pages, zobacz Wprowadzenie do [Razor Pages](xref:tutorials/razor-pages/razor-pages-start) .</span><span class="sxs-lookup"><span data-stu-id="bb09a-117">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bb09a-118">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="bb09a-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bb09a-119">Uruchom `dotnet new webapp` polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="bb09a-119">Run `dotnet new webapp` from the command line.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bb09a-120">Uruchom `dotnet new razor` polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="bb09a-120">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

<span data-ttu-id="bb09a-121">Otwórz wygenerowany plik *csproj* z Visual Studio dla komputerów Mac.</span><span class="sxs-lookup"><span data-stu-id="bb09a-121">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bb09a-122">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bb09a-122">Visual Studio Code</span></span>](#tab/visual-studio-code)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bb09a-123">Uruchom `dotnet new webapp` polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="bb09a-123">Run `dotnet new webapp` from the command line.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bb09a-124">Uruchom `dotnet new razor` polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="bb09a-124">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

---

## <a name="razor-pages"></a><span data-ttu-id="bb09a-125">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="bb09a-125">Razor Pages</span></span>

<span data-ttu-id="bb09a-126">Razor Pages jest włączona w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="bb09a-126">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="bb09a-127">Weź pod uwagę podstawową stronę:<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="bb09a-127">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="bb09a-128">Poprzedni kod wygląda podobnie jak [plik widoku Razor](xref:tutorials/first-mvc-app/adding-view) używany w aplikacji ASP.NET Core z kontrolerami i widokami.</span><span class="sxs-lookup"><span data-stu-id="bb09a-128">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="bb09a-129">Co sprawia, `@page` że jest to dyrektywa.</span><span class="sxs-lookup"><span data-stu-id="bb09a-129">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="bb09a-130">`@page`sprawia, że plik jest akcją MVC, co oznacza, że obsługuje żądania bezpośrednio, bez przechodzenia przez kontroler.</span><span class="sxs-lookup"><span data-stu-id="bb09a-130">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="bb09a-131">`@page`musi być pierwszą dyrektywą Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="bb09a-131">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="bb09a-132">`@page`wpływa na zachowanie innych konstrukcji Razor.</span><span class="sxs-lookup"><span data-stu-id="bb09a-132">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="bb09a-133">Podobna Strona, za pomocą `PageModel` klasy, jest pokazana w następujących dwóch plikach.</span><span class="sxs-lookup"><span data-stu-id="bb09a-133">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="bb09a-134">Plik *Pages/index2. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="bb09a-134">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="bb09a-135">Model strony *stron/index2. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="bb09a-135">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="bb09a-136">Zgodnie z Konwencją `PageModel` plik klasy ma taką samą nazwę jak plik stronicowania Razor z dołączonym rozszerzeniem *. cs* .</span><span class="sxs-lookup"><span data-stu-id="bb09a-136">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="bb09a-137">Na przykład Poprzednia strona Razor to *Pages/index2. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="bb09a-137">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="bb09a-138">Plik zawierający `PageModel` klasę ma nazwę Pages */index2. cshtml. cs*.</span><span class="sxs-lookup"><span data-stu-id="bb09a-138">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="bb09a-139">Skojarzenia ścieżek adresów URL ze stronami są określane przez lokalizację strony w systemie plików.</span><span class="sxs-lookup"><span data-stu-id="bb09a-139">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="bb09a-140">W poniższej tabeli przedstawiono ścieżkę strony Razor i pasujący adres URL:</span><span class="sxs-lookup"><span data-stu-id="bb09a-140">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="bb09a-141">Nazwa i ścieżka pliku</span><span class="sxs-lookup"><span data-stu-id="bb09a-141">File name and path</span></span>               | <span data-ttu-id="bb09a-142">pasujący adres URL</span><span class="sxs-lookup"><span data-stu-id="bb09a-142">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="bb09a-143">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb09a-143">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="bb09a-144">`/` lub `/Index`</span><span class="sxs-lookup"><span data-stu-id="bb09a-144">`/` or `/Index`</span></span> |
| <span data-ttu-id="bb09a-145">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb09a-145">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="bb09a-146">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb09a-146">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="bb09a-147">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb09a-147">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="bb09a-148">`/Store` lub `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="bb09a-148">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="bb09a-149">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="bb09a-149">Notes:</span></span>

* <span data-ttu-id="bb09a-150">Środowisko wykonawcze domyślnie wyszukuje pliki Razor Pages  w folderze Pages.</span><span class="sxs-lookup"><span data-stu-id="bb09a-150">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="bb09a-151">`Index`jest stroną domyślną, gdy adres URL nie zawiera strony.</span><span class="sxs-lookup"><span data-stu-id="bb09a-151">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="bb09a-152">Napisz podstawowy formularz</span><span class="sxs-lookup"><span data-stu-id="bb09a-152">Write a basic form</span></span>

<span data-ttu-id="bb09a-153">Razor Pages zaprojektowano tak, aby wspólne wzorce używane z przeglądarkami sieci Web były łatwe do wdrożenia podczas kompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bb09a-153">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="bb09a-154">[Powiązania modelu](xref:mvc/models/model-binding), [pomocników tagów](xref:mvc/views/tag-helpers/intro)i pomocników HTML same *działają* z właściwościami zdefiniowanymi w klasie strony Razor.</span><span class="sxs-lookup"><span data-stu-id="bb09a-154">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="bb09a-155">Weź pod uwagę stronę implementującą podstawowy formularz "contact us" (kontakt `Contact` z nami) dla modelu:</span><span class="sxs-lookup"><span data-stu-id="bb09a-155">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="bb09a-156">Przykłady w tym dokumencie `DbContext` są inicjowane w pliku [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) .</span><span class="sxs-lookup"><span data-stu-id="bb09a-156">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="bb09a-157">Model danych:</span><span class="sxs-lookup"><span data-stu-id="bb09a-157">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="bb09a-158">Kontekst bazy danych:</span><span class="sxs-lookup"><span data-stu-id="bb09a-158">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="bb09a-159">Plik widoku *Pages/Create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="bb09a-159">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="bb09a-160">Model strony *Pages/Create. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="bb09a-160">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="bb09a-161">Zgodnie z Konwencją `PageModel` Klasa jest wywoływana `<PageName>Model` i znajduje się w tej samej przestrzeni nazw co strona.</span><span class="sxs-lookup"><span data-stu-id="bb09a-161">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="bb09a-162">`PageModel` Klasa umożliwia rozdzielenie logiki strony od jej prezentacji.</span><span class="sxs-lookup"><span data-stu-id="bb09a-162">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="bb09a-163">Definiuje procedury obsługi stron dla żądań wysyłanych do strony oraz dane używane do renderowania strony.</span><span class="sxs-lookup"><span data-stu-id="bb09a-163">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="bb09a-164">To Separacja umożliwia zarządzanie zależnościami stron za pomocą [iniekcji zależności](xref:fundamentals/dependency-injection) oraz do [testowania jednostkowego](xref:test/razor-pages-tests) stron.</span><span class="sxs-lookup"><span data-stu-id="bb09a-164">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="bb09a-165">Strona ma `OnPostAsync` *metodę obsługi*, która jest uruchamiana na `POST` żądaniach (gdy użytkownik księguje formularz).</span><span class="sxs-lookup"><span data-stu-id="bb09a-165">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="bb09a-166">Można dodać metody obsługi dla dowolnego zlecenia HTTP.</span><span class="sxs-lookup"><span data-stu-id="bb09a-166">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="bb09a-167">Najczęstszymi obsłudze są:</span><span class="sxs-lookup"><span data-stu-id="bb09a-167">The most common handlers are:</span></span>

* <span data-ttu-id="bb09a-168">`OnGet`do zainicjowania stanu wymaganego dla strony.</span><span class="sxs-lookup"><span data-stu-id="bb09a-168">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="bb09a-169">Przykład [OnGet](#OnGet) .</span><span class="sxs-lookup"><span data-stu-id="bb09a-169">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="bb09a-170">`OnPost`do obsługi przesłanych formularzy.</span><span class="sxs-lookup"><span data-stu-id="bb09a-170">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="bb09a-171">Sufiks `Async` nazewnictwa jest opcjonalny, ale jest często używany przez Konwencję dla funkcji asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="bb09a-171">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="bb09a-172">`OnPostAsync` Kod w poprzednim przykładzie wygląda podobnie jak w przypadku normalnego zapisu w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="bb09a-172">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="bb09a-173">Poprzedni kod jest typowy dla Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="bb09a-173">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="bb09a-174">Większość elementów podstawowych MVC, takich jak [powiązanie modelu](xref:mvc/models/model-binding), [Walidacja](xref:mvc/models/validation)i wyniki akcji, jest udostępniana.</span><span class="sxs-lookup"><span data-stu-id="bb09a-174">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="bb09a-175">Poprzednia `OnPostAsync` Metoda:</span><span class="sxs-lookup"><span data-stu-id="bb09a-175">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="bb09a-176">Podstawowy przepływ `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="bb09a-176">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="bb09a-177">Sprawdź, czy występują błędy walidacji.</span><span class="sxs-lookup"><span data-stu-id="bb09a-177">Check for validation errors.</span></span>

* <span data-ttu-id="bb09a-178">Jeśli nie ma żadnych błędów, Zapisz dane i Przekieruj.</span><span class="sxs-lookup"><span data-stu-id="bb09a-178">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="bb09a-179">W przypadku wystąpienia błędów ponownie Wyświetl stronę z komunikatami walidacji.</span><span class="sxs-lookup"><span data-stu-id="bb09a-179">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="bb09a-180">Walidacja po stronie klienta jest taka sama jak w przypadku tradycyjnych ASP.NET Core aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="bb09a-180">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="bb09a-181">W wielu przypadkach błędy sprawdzania poprawności zostaną wykryte na kliencie i nigdy nie przesłano ich do serwera.</span><span class="sxs-lookup"><span data-stu-id="bb09a-181">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="bb09a-182">Po pomyślnym `OnPostAsync` wprowadzeniu danych metoda obsługi `RedirectToPage` wywołuje metodę pomocnika zwracającą wystąpienie elementu `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="bb09a-182">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="bb09a-183">`RedirectToPage`to nowy wynik akcji, podobny do `RedirectToAction` lub `RedirectToRoute`, ale dostosowany do stron.</span><span class="sxs-lookup"><span data-stu-id="bb09a-183">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="bb09a-184">W powyższym przykładzie przekierowuje do strony indeksu głównego (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="bb09a-184">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="bb09a-185">`RedirectToPage`szczegółowo znajduje się w sekcji [generowanie adresów URL dla stron](#url_gen) .</span><span class="sxs-lookup"><span data-stu-id="bb09a-185">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="bb09a-186">Gdy przesłany formularz ma błędy walidacji (które są przekazywane do serwera),`OnPostAsync` metoda obsługi `Page` wywołuje metodę pomocnika.</span><span class="sxs-lookup"><span data-stu-id="bb09a-186">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="bb09a-187">`Page`Zwraca wystąpienie elementu `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="bb09a-187">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="bb09a-188">Zwracanie `Page` jest podobne do sposobu, w jaki akcje `View`w kontrolerach zwracają.</span><span class="sxs-lookup"><span data-stu-id="bb09a-188">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="bb09a-189">`PageResult`jest wartością domyślną</span><span class="sxs-lookup"><span data-stu-id="bb09a-189">`PageResult` is the default</span></span> <!-- Review  --> <span data-ttu-id="bb09a-190">zwracany typ dla metody obsługi.</span><span class="sxs-lookup"><span data-stu-id="bb09a-190">return type for a handler method.</span></span> <span data-ttu-id="bb09a-191">Metoda obsługi, która zwraca `void` renderowanie strony.</span><span class="sxs-lookup"><span data-stu-id="bb09a-191">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="bb09a-192">`Customer` Właściwość używa`[BindProperty]` atrybutu, aby zrezygnować z powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="bb09a-192">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="bb09a-193">Razor Pages domyślnie Powiąż właściwości tylko z`GET` niezleceniami.</span><span class="sxs-lookup"><span data-stu-id="bb09a-193">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="bb09a-194">Powiązanie z właściwościami może zmniejszyć ilość kodu, który trzeba napisać.</span><span class="sxs-lookup"><span data-stu-id="bb09a-194">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="bb09a-195">Powiązanie zmniejsza kod, używając tej samej właściwości do renderowania pól formularza (`<input asp-for="Customer.Name">`) i akceptuję dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="bb09a-195">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="bb09a-196">Strona główna (*index. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="bb09a-196">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="bb09a-197">Skojarzona `PageModel` Klasa (*index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="bb09a-197">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="bb09a-198">Plik *index. cshtml* zawiera następujące znaczniki, aby utworzyć łącze do edycji dla każdego kontaktu:</span><span class="sxs-lookup"><span data-stu-id="bb09a-198">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="bb09a-199">[Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) użył `asp-route-{value}` atrybutu w celu wygenerowania linku do strony edycji.</span><span class="sxs-lookup"><span data-stu-id="bb09a-199">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="bb09a-200">Link zawiera dane trasy z IDENTYFIKATORem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="bb09a-200">The link contains route data with the contact ID.</span></span> <span data-ttu-id="bb09a-201">Na przykład `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="bb09a-201">For example, `http://localhost:5000/Edit/1`.</span></span> <span data-ttu-id="bb09a-202">Użyj atrybutu `asp-area` , aby określić obszar.</span><span class="sxs-lookup"><span data-stu-id="bb09a-202">Use the `asp-area` attribute to specify an area.</span></span> <span data-ttu-id="bb09a-203">Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="bb09a-203">For more information, see <xref:mvc/controllers/areas>.</span></span>

<span data-ttu-id="bb09a-204">Plik *Pages/Edit. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="bb09a-204">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="bb09a-205">Pierwszy wiersz zawiera `@page "{id:int}"` dyrektywę.</span><span class="sxs-lookup"><span data-stu-id="bb09a-205">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="bb09a-206">Ograniczenie`"{id:int}"` routingu instruuje stronę, aby akceptowała żądania do strony `int` zawierającej dane trasy.</span><span class="sxs-lookup"><span data-stu-id="bb09a-206">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="bb09a-207">Jeśli żądanie do strony nie zawiera danych trasy, które można przekonwertować na obiekt `int`, środowisko uruchomieniowe zwróci błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="bb09a-207">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="bb09a-208">Aby identyfikator był opcjonalny, Dołącz `?` do ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="bb09a-208">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="bb09a-209">Plik *stron/Edytuj. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="bb09a-209">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="bb09a-210">Plik *index. cshtml* zawiera również znaczniki umożliwiające utworzenie przycisku usuwania dla każdego kontaktu z klientem:</span><span class="sxs-lookup"><span data-stu-id="bb09a-210">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="bb09a-211">Gdy przycisk Usuń jest renderowany w języku HTML, jego `formaction` parametry obejmują:</span><span class="sxs-lookup"><span data-stu-id="bb09a-211">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="bb09a-212">Identyfikator osoby kontaktowej klienta określony przez `asp-route-id` atrybut.</span><span class="sxs-lookup"><span data-stu-id="bb09a-212">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="bb09a-213">`handler` Określony`asp-page-handler` przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="bb09a-213">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="bb09a-214">Oto przykład renderowanego przycisku usuwania z IDENTYFIKATORem `1`kontaktu klienta:</span><span class="sxs-lookup"><span data-stu-id="bb09a-214">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="bb09a-215">Po wybraniu przycisku do serwera zostanie wysłane `POST` żądanie formularza.</span><span class="sxs-lookup"><span data-stu-id="bb09a-215">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="bb09a-216">Według Konwencji, nazwa metody obsługi jest wybierana na podstawie wartości `handler` parametru zgodnie z schematem. `OnPost[handler]Async`</span><span class="sxs-lookup"><span data-stu-id="bb09a-216">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="bb09a-217">`handler` Ponieważ jest `delete` w`OnPostDeleteAsync` tym przykładzie, metoda obsługi jest używana do przetwarzania `POST` żądania.</span><span class="sxs-lookup"><span data-stu-id="bb09a-217">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="bb09a-218">Jeśli jest ustawiona na inną wartość, na przykład `remove`, jest wybierana metoda obsługi o nazwie `OnPostRemoveAsync`. `asp-page-handler`</span><span class="sxs-lookup"><span data-stu-id="bb09a-218">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="bb09a-219">`OnPostDeleteAsync` Metody:</span><span class="sxs-lookup"><span data-stu-id="bb09a-219">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="bb09a-220">`id` Akceptuje z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="bb09a-220">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="bb09a-221">Wysyła zapytanie do bazy danych w celu skontaktowania się z klientem za pomocą `FindAsync`programu.</span><span class="sxs-lookup"><span data-stu-id="bb09a-221">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="bb09a-222">Jeśli kontakt z klientem zostanie znaleziony, zostanie on usunięty z listy kontaktów klientów.</span><span class="sxs-lookup"><span data-stu-id="bb09a-222">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="bb09a-223">Baza danych jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="bb09a-223">The database is updated.</span></span>
* <span data-ttu-id="bb09a-224">Wywołania `RedirectToPage` do przekierowania na stronę indeksu głównego (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="bb09a-224">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="bb09a-225">Oznacz właściwości strony jako wymagane</span><span class="sxs-lookup"><span data-stu-id="bb09a-225">Mark page properties as required</span></span>

<span data-ttu-id="bb09a-226">Właściwości na serwerze `PageModel` mogą być dekoracyjne z [wymaganym](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) atrybutem:</span><span class="sxs-lookup"><span data-stu-id="bb09a-226">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="bb09a-227">Aby uzyskać więcej informacji, zobacz [Walidacja modelu](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="bb09a-227">For more information, see [Model validation](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="bb09a-228">Obsługa żądań głównych przy użyciu rezerwy procedury obsługi OnGet</span><span class="sxs-lookup"><span data-stu-id="bb09a-228">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="bb09a-229">`HEAD`żądania umożliwiają pobranie nagłówków dla określonego zasobu.</span><span class="sxs-lookup"><span data-stu-id="bb09a-229">`HEAD` requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="bb09a-230">W przeciwieństwie `GET` do `HEAD` żądań, żądania nie zwracają treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="bb09a-230">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="bb09a-231">Zwykle program obsługi jest tworzony i wywoływany dla `HEAD` żądań: `OnHead`</span><span class="sxs-lookup"><span data-stu-id="bb09a-231">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="bb09a-232">W ASP.NET Core 2,1 lub nowszej Razor Pages powracać do wywoływania procedury `OnGet` obsługi, jeśli `OnHead` nie zdefiniowano procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="bb09a-232">In ASP.NET Core 2.1 or later, Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span> <span data-ttu-id="bb09a-233">To zachowanie jest włączane przez wywołanie [SetCompatibilityVersion](xref:mvc/compatibility-version) w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="bb09a-233">This behavior is enabled by the call to [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="bb09a-234">Szablony domyślne generują `SetCompatibilityVersion` wywołanie w ASP.NET Core 2,1 i 2,2.</span><span class="sxs-lookup"><span data-stu-id="bb09a-234">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span> <span data-ttu-id="bb09a-235">`SetCompatibilityVersion`efektywnie ustawia opcję `AllowMappingHeadRequestsToGetHandler` Razor Pages na `true`.</span><span class="sxs-lookup"><span data-stu-id="bb09a-235">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="bb09a-236">Zamiast korzystać z wszystkich zachowań w programie `SetCompatibilityVersion`, można jawnie zrezygnować z *określonych* zachowań.</span><span class="sxs-lookup"><span data-stu-id="bb09a-236">Rather than opting in to all behaviors with `SetCompatibilityVersion`, you can explicitly opt in to *specific* behaviors.</span></span> <span data-ttu-id="bb09a-237">Poniższy kod pozwala na umożliwienie `HEAD` mapowania żądań `OnGet` do programu obsługi:</span><span class="sxs-lookup"><span data-stu-id="bb09a-237">The following code opts in to allowing `HEAD` requests to be mapped to the `OnGet` handler:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="bb09a-238">XSRF/CSRF i Razor Pages</span><span class="sxs-lookup"><span data-stu-id="bb09a-238">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="bb09a-239">Nie trzeba pisać kodu do [weryfikacji](xref:security/anti-request-forgery)przed fałszerstwem.</span><span class="sxs-lookup"><span data-stu-id="bb09a-239">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="bb09a-240">Generowanie i sprawdzanie poprawności tokenów antysfałszowanych są automatycznie dołączane do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="bb09a-240">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="bb09a-241">Używanie układów, częściowych, szablonów i pomocników tagów z Razor Pages</span><span class="sxs-lookup"><span data-stu-id="bb09a-241">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="bb09a-242">Strony współpracują ze wszystkimi funkcjami aparatu widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="bb09a-242">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="bb09a-243">Układy, części, szablony, pomocników tagów, *_ViewStart. cshtml*, *_ViewImports. cshtml* działają w taki sam sposób, jak w przypadku konwencjonalnych widoków Razor.</span><span class="sxs-lookup"><span data-stu-id="bb09a-243">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="bb09a-244">Zanotujmy Tę stronę, korzystając z zalet niektórych z tych funkcji.</span><span class="sxs-lookup"><span data-stu-id="bb09a-244">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bb09a-245">Dodaj [stronę układu](xref:mvc/views/layout) do *stron/Shared/_Layout. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="bb09a-245">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bb09a-246">Dodaj [stronę układu](xref:mvc/views/layout) do *stron/_Layout. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="bb09a-246">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

::: moniker-end

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="bb09a-247">[Układ](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="bb09a-247">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="bb09a-248">Steruje układem każdej strony (chyba że strona nie jest częścią układu).</span><span class="sxs-lookup"><span data-stu-id="bb09a-248">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="bb09a-249">Importuje struktury HTML, takie jak JavaScript i stylesheets.</span><span class="sxs-lookup"><span data-stu-id="bb09a-249">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="bb09a-250">Aby uzyskać więcej informacji, zobacz [stronę układu](xref:mvc/views/layout) .</span><span class="sxs-lookup"><span data-stu-id="bb09a-250">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="bb09a-251">Właściwość [układu](xref:mvc/views/layout#specifying-a-layout) jest ustawiana na *stronie/_ViewStart. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="bb09a-251">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bb09a-252">Układ znajduje się w *stronie/* w folderze udostępnionym.</span><span class="sxs-lookup"><span data-stu-id="bb09a-252">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="bb09a-253">Strony szukają innych widoków (układów, szablonów, częściowych) hierarchicznie, rozpoczynając w tym samym folderze, w którym znajduje się bieżąca strona.</span><span class="sxs-lookup"><span data-stu-id="bb09a-253">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="bb09a-254">Układ na *stronach/* w folderze udostępnionym może być używany z dowolnej strony Razor w folderze *Pages* .</span><span class="sxs-lookup"><span data-stu-id="bb09a-254">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="bb09a-255">Plik układu powinien przejść do *stron/folderu udostępnionego* .</span><span class="sxs-lookup"><span data-stu-id="bb09a-255">The layout file should go in the *Pages/Shared* folder.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bb09a-256">Układ znajduje się w folderze *strony* .</span><span class="sxs-lookup"><span data-stu-id="bb09a-256">The layout is in the *Pages* folder.</span></span> <span data-ttu-id="bb09a-257">Strony szukają innych widoków (układów, szablonów, częściowych) hierarchicznie, rozpoczynając w tym samym folderze, w którym znajduje się bieżąca strona.</span><span class="sxs-lookup"><span data-stu-id="bb09a-257">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="bb09a-258">Układ w folderze *Pages* może być używany z dowolnej strony Razor w folderze *Pages* .</span><span class="sxs-lookup"><span data-stu-id="bb09a-258">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

::: moniker-end

<span data-ttu-id="bb09a-259">Zalecamy **umieszczenie pliku** układu w widokach */* folderze udostępnionym.</span><span class="sxs-lookup"><span data-stu-id="bb09a-259">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="bb09a-260">*Widoki/udostępnione* są wzorcem widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="bb09a-260">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="bb09a-261">Razor Pages są przeznaczone do korzystania z hierarchii folderów, a nie Konwencji ścieżek.</span><span class="sxs-lookup"><span data-stu-id="bb09a-261">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="bb09a-262">Widok wyszukiwania na stronie Razor zawiera folder *strony* .</span><span class="sxs-lookup"><span data-stu-id="bb09a-262">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="bb09a-263">Układy, szablony i częściowe, które są używane z kontrolerami MVC i konwencjonalnymi widokami Razor, *działają tylko*.</span><span class="sxs-lookup"><span data-stu-id="bb09a-263">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="bb09a-264">Dodaj plik *Pages/_ViewImports. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="bb09a-264">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="bb09a-265">`@namespace`wyjaśniono w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="bb09a-265">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="bb09a-266">Dyrektywa `@addTagHelper` znajduje się w [wbudowanych pomocników tagów](xref:mvc/views/tag-helpers/builtin-th/Index) do wszystkich stron w folderze *strony* .</span><span class="sxs-lookup"><span data-stu-id="bb09a-266">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="bb09a-267">`@namespace` Gdy dyrektywa jest używana jawnie na stronie:</span><span class="sxs-lookup"><span data-stu-id="bb09a-267">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="bb09a-268">Dyrektywa ustawia przestrzeń nazw dla strony.</span><span class="sxs-lookup"><span data-stu-id="bb09a-268">The directive sets the namespace for the page.</span></span> <span data-ttu-id="bb09a-269">`@model` Dyrektywa nie musi zawierać przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="bb09a-269">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="bb09a-270">Gdy dyrektywa jest zawarta w *_ViewImports. cshtml*, określona przestrzeń nazw udostępnia prefiks dla wygenerowanej przestrzeni nazw na `@namespace` stronie, która importuje dyrektywę. `@namespace`</span><span class="sxs-lookup"><span data-stu-id="bb09a-270">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="bb09a-271">Pozostała część wygenerowanej przestrzeni nazw (część sufiksu) jest ścieżką względną oddzieloną kropką między folderem zawierającym *_ViewImports. cshtml* i folderem zawierającym stronę.</span><span class="sxs-lookup"><span data-stu-id="bb09a-271">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="bb09a-272">Na przykład `PageModel` Klasa Pages */Customers/Edit. cshtml. cs* jawnie ustawia przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="bb09a-272">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="bb09a-273">Plik *Pages/_ViewImports. cshtml* ustawia następującą przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="bb09a-273">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="bb09a-274">Wygenerowana przestrzeń nazw dla strony */Customers/Edit. cshtml* Razor jest taka sama jak `PageModel` Klasa.</span><span class="sxs-lookup"><span data-stu-id="bb09a-274">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="bb09a-275">`@namespace`*działa również z konwencjonalnymi widokami Razor.*</span><span class="sxs-lookup"><span data-stu-id="bb09a-275">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="bb09a-276">Oryginalne *strony/Utwórz plik widoku. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="bb09a-276">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="bb09a-277">Zaktualizowane *strony/Utwórz plik widoku. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="bb09a-277">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="bb09a-278">[Razor Pages początkowy projekt](#rpvs17) zawiera *stronę/_ValidationScriptsPartial. cshtml*, który łączy weryfikację po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="bb09a-278">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="bb09a-279">Aby uzyskać więcej informacji o widokach częściowych, zobacz <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="bb09a-279">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="bb09a-280">Generowanie adresu URL dla stron</span><span class="sxs-lookup"><span data-stu-id="bb09a-280">URL generation for Pages</span></span>

<span data-ttu-id="bb09a-281">Strona, pokazana wcześniej, używa `RedirectToPage`: `Create`</span><span class="sxs-lookup"><span data-stu-id="bb09a-281">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="bb09a-282">Aplikacja ma następującą strukturę plików/folderów:</span><span class="sxs-lookup"><span data-stu-id="bb09a-282">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="bb09a-283">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="bb09a-283">*/Pages*</span></span>

  * <span data-ttu-id="bb09a-284">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb09a-284">*Index.cshtml*</span></span>
  * <span data-ttu-id="bb09a-285">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="bb09a-285">*/Customers*</span></span>

    * <span data-ttu-id="bb09a-286">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb09a-286">*Create.cshtml*</span></span>
    * <span data-ttu-id="bb09a-287">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb09a-287">*Edit.cshtml*</span></span>
    * <span data-ttu-id="bb09a-288">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bb09a-288">*Index.cshtml*</span></span>

<span data-ttu-id="bb09a-289">Strony */Customers/Create. cshtml* i Pages/ *Customers/Edit. cshtml* przekierują do *stron/index. cshtml* po powodzeniu.</span><span class="sxs-lookup"><span data-stu-id="bb09a-289">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="bb09a-290">Ciąg `/Index` jest częścią identyfikatora URI, aby uzyskać dostęp do poprzedniej strony.</span><span class="sxs-lookup"><span data-stu-id="bb09a-290">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="bb09a-291">Ten ciąg `/Index` może służyć do generowania identyfikatorów URI na stronie *stron/index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="bb09a-291">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="bb09a-292">Przykład:</span><span class="sxs-lookup"><span data-stu-id="bb09a-292">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="bb09a-293">Nazwa strony jest ścieżką do strony z folderu głównego */Pages* , włącznie z wiodącym `/` `/Index`(na przykład).</span><span class="sxs-lookup"><span data-stu-id="bb09a-293">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="bb09a-294">Powyższe przykłady generowania adresów URL oferują ulepszone opcje i możliwości funkcjonalne w porównaniu z zakodowana adresem URL.</span><span class="sxs-lookup"><span data-stu-id="bb09a-294">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="bb09a-295">Generowanie adresów URL używa [routingu](xref:mvc/controllers/routing) i może generować i kodować parametry zgodnie ze sposobem zdefiniowania trasy w ścieżce docelowej.</span><span class="sxs-lookup"><span data-stu-id="bb09a-295">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="bb09a-296">Generowanie adresów URL dla stron obsługuje nazwy względne.</span><span class="sxs-lookup"><span data-stu-id="bb09a-296">URL generation for pages supports relative names.</span></span> <span data-ttu-id="bb09a-297">W poniższej tabeli przedstawiono, która strona indeksu została wybrana z `RedirectToPage` różnymi parametrami *stron/Customers/Create. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="bb09a-297">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="bb09a-298">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="bb09a-298">RedirectToPage(x)</span></span>| <span data-ttu-id="bb09a-299">Stronic</span><span class="sxs-lookup"><span data-stu-id="bb09a-299">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="bb09a-300">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="bb09a-300">RedirectToPage("/Index")</span></span> | <span data-ttu-id="bb09a-301">*Strony/indeks*</span><span class="sxs-lookup"><span data-stu-id="bb09a-301">*Pages/Index*</span></span> |
| <span data-ttu-id="bb09a-302">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="bb09a-302">RedirectToPage("./Index");</span></span> | <span data-ttu-id="bb09a-303">*Strony/klienci/indeks*</span><span class="sxs-lookup"><span data-stu-id="bb09a-303">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="bb09a-304">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="bb09a-304">RedirectToPage("../Index")</span></span> | <span data-ttu-id="bb09a-305">*Strony/indeks*</span><span class="sxs-lookup"><span data-stu-id="bb09a-305">*Pages/Index*</span></span> |
| <span data-ttu-id="bb09a-306">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="bb09a-306">RedirectToPage("Index")</span></span>  | <span data-ttu-id="bb09a-307">*Strony/klienci/indeks*</span><span class="sxs-lookup"><span data-stu-id="bb09a-307">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="bb09a-308">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, i `RedirectToPage("../Index")` są *nazwami względnymi*.</span><span class="sxs-lookup"><span data-stu-id="bb09a-308">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="bb09a-309">Parametr jest połączony ze ścieżką bieżącej strony, aby obliczyć nazwę strony docelowej.  `RedirectToPage`</span><span class="sxs-lookup"><span data-stu-id="bb09a-309">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="bb09a-310">Łączenie nazw względnych jest przydatne podczas kompilowania lokacji ze złożoną strukturą.</span><span class="sxs-lookup"><span data-stu-id="bb09a-310">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="bb09a-311">Jeśli używasz nazw względnych do łączenia między stronami w folderze, możesz zmienić nazwę tego folderu.</span><span class="sxs-lookup"><span data-stu-id="bb09a-311">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="bb09a-312">Wszystkie linki nadal działają (ponieważ nie zawierają nazwy folderu).</span><span class="sxs-lookup"><span data-stu-id="bb09a-312">All the links still work (because they didn't include the folder name).</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bb09a-313">Aby przekierować do strony w innym [obszarze](xref:mvc/controllers/areas), określ obszar:</span><span class="sxs-lookup"><span data-stu-id="bb09a-313">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="bb09a-314">Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="bb09a-314">For more information, see <xref:mvc/controllers/areas>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="bb09a-315">ViewData — atrybut</span><span class="sxs-lookup"><span data-stu-id="bb09a-315">ViewData attribute</span></span>

<span data-ttu-id="bb09a-316">Dane można przekazywać do strony z [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="bb09a-316">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="bb09a-317">Właściwości na kontrolerach lub modelach stron Razor, `[ViewData]` których wartości są przechowywane i ładowane z [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="bb09a-317">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="bb09a-318">W poniższym przykładzie `AboutModel` `Title` zawiera właściwość z `[ViewData]`właściwością.</span><span class="sxs-lookup"><span data-stu-id="bb09a-318">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="bb09a-319">`Title` Właściwość jest ustawiona na tytuł strony informacje:</span><span class="sxs-lookup"><span data-stu-id="bb09a-319">The `Title` property is set to the title of the About page:</span></span>

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

<span data-ttu-id="bb09a-320">Na stronie informacje uzyskaj dostęp do `Title` właściwości jako właściwości modelu:</span><span class="sxs-lookup"><span data-stu-id="bb09a-320">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="bb09a-321">W układzie tytuł jest odczytywany ze słownika ViewData:</span><span class="sxs-lookup"><span data-stu-id="bb09a-321">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="bb09a-322">TempData</span><span class="sxs-lookup"><span data-stu-id="bb09a-322">TempData</span></span>

<span data-ttu-id="bb09a-323">ASP.NET Core uwidacznia Właściwość [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) na [kontrolerze](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="bb09a-323">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="bb09a-324">Ta właściwość przechowuje dane, dopóki nie zostanie odczytana.</span><span class="sxs-lookup"><span data-stu-id="bb09a-324">This property stores data until it's read.</span></span> <span data-ttu-id="bb09a-325">Metody `Keep` i`Peek` mogą służyć do badania danych bez usuwania.</span><span class="sxs-lookup"><span data-stu-id="bb09a-325">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="bb09a-326">`TempData`jest przydatne w przypadku przekierowania, gdy dane są potrzebne dla więcej niż jednego żądania.</span><span class="sxs-lookup"><span data-stu-id="bb09a-326">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="bb09a-327">Ten `[TempData]` atrybut jest nowy w ASP.NET Core 2,0 i jest obsługiwany na kontrolerach i stronach.</span><span class="sxs-lookup"><span data-stu-id="bb09a-327">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="bb09a-328">Poniższy kod ustawia wartość `Message` użycia: `TempData`</span><span class="sxs-lookup"><span data-stu-id="bb09a-328">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="bb09a-329">W poniższym znaczniku w pliku *Pages/Customers/index. cshtml* jest wyświetlana wartość `Message` using `TempData`.</span><span class="sxs-lookup"><span data-stu-id="bb09a-329">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="bb09a-330">Model strony *Pages/Customers/index. cshtml. cs* stosuje `[TempData]` atrybut do `Message` właściwości.</span><span class="sxs-lookup"><span data-stu-id="bb09a-330">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="bb09a-331">Aby uzyskać więcej informacji, zobacz [TempData](xref:fundamentals/app-state#tempdata) .</span><span class="sxs-lookup"><span data-stu-id="bb09a-331">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="bb09a-332">Wiele programów obsługi na stronie</span><span class="sxs-lookup"><span data-stu-id="bb09a-332">Multiple handlers per page</span></span>

<span data-ttu-id="bb09a-333">Poniższa Strona generuje znaczniki dla dwóch programów obsługi przy użyciu `asp-page-handler` pomocnika tagów:</span><span class="sxs-lookup"><span data-stu-id="bb09a-333">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="bb09a-334">Formularz w poprzednim przykładzie ma dwa przyciski przesyłania, z których `FormActionTagHelper` każdy używa do przesłania do innego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="bb09a-334">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="bb09a-335">Ten `asp-page-handler` atrybut jest towarzyszący do `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="bb09a-335">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="bb09a-336">`asp-page-handler`generuje adresy URL, które przesyłają do każdej metody obsługi zdefiniowanej przez stronę.</span><span class="sxs-lookup"><span data-stu-id="bb09a-336">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="bb09a-337">`asp-page`nie została określona, ponieważ próbka jest łączona z bieżącą stroną.</span><span class="sxs-lookup"><span data-stu-id="bb09a-337">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="bb09a-338">Model strony:</span><span class="sxs-lookup"><span data-stu-id="bb09a-338">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="bb09a-339">Poprzedni kod używa *nazwanych metod obsługi*.</span><span class="sxs-lookup"><span data-stu-id="bb09a-339">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="bb09a-340">Nazwane metody obsługi są tworzone przez pobranie tekstu w nazwie po `On<HTTP Verb>` i przed `Async` (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="bb09a-340">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="bb09a-341">W poprzednim przykładzie metody strony są onpost**JoinList**Async i Onpost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="bb09a-341">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="bb09a-342">Po  usunięciu funkcji onpost i *Async* nazwy programów obsługi `JoinList` są `JoinListUC`i.</span><span class="sxs-lookup"><span data-stu-id="bb09a-342">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="bb09a-343">Korzystając z powyższego kodu, ścieżka URL, która jest `OnPostJoinListAsync` przesyłana do usługi, to `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="bb09a-343">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="bb09a-344">Ścieżka URL, która jest przesyłana `OnPostJoinListUCAsync` do `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`programu, to.</span><span class="sxs-lookup"><span data-stu-id="bb09a-344">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="bb09a-345">Trasy niestandardowe</span><span class="sxs-lookup"><span data-stu-id="bb09a-345">Custom routes</span></span>

<span data-ttu-id="bb09a-346">Użyj dyrektywy `@page` , aby:</span><span class="sxs-lookup"><span data-stu-id="bb09a-346">Use the `@page` directive to:</span></span>

* <span data-ttu-id="bb09a-347">Określ trasę niestandardową dla strony.</span><span class="sxs-lookup"><span data-stu-id="bb09a-347">Specify a custom route to a page.</span></span> <span data-ttu-id="bb09a-348">Na przykład trasy do strony informacje można ustawić na `/Some/Other/Path` wartość z. `@page "/Some/Other/Path"`</span><span class="sxs-lookup"><span data-stu-id="bb09a-348">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="bb09a-349">Dołącz segmenty do domyślnej trasy strony.</span><span class="sxs-lookup"><span data-stu-id="bb09a-349">Append segments to a page's default route.</span></span> <span data-ttu-id="bb09a-350">Na przykład segment "Item" można dodać do domyślnej trasy strony przy użyciu `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="bb09a-350">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="bb09a-351">Dołącz parametry do domyślnej trasy strony.</span><span class="sxs-lookup"><span data-stu-id="bb09a-351">Append parameters to a page's default route.</span></span> <span data-ttu-id="bb09a-352">Na przykład parametr ID, `id`, może być wymagany dla strony z. `@page "{id}"`</span><span class="sxs-lookup"><span data-stu-id="bb09a-352">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="bb09a-353">Ścieżka względna elementu głównego wypisana przez tyldę`~`() na początku ścieżki jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="bb09a-353">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="bb09a-354">Na przykład `@page "~/Some/Other/Path"` jest taka sama jak `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="bb09a-354">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="bb09a-355">Można zmienić ciąg `?handler=JoinList` zapytania w adresie URL na segment `/JoinList` trasy, określając szablon `@page "{handler?}"`trasy.</span><span class="sxs-lookup"><span data-stu-id="bb09a-355">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="bb09a-356">Jeśli nie podoba Ci się ciąg `?handler=JoinList` zapytania w adresie URL, możesz zmienić trasy, aby umieścić nazwę programu obsługi w części adresu URL.</span><span class="sxs-lookup"><span data-stu-id="bb09a-356">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="bb09a-357">Możesz dostosować trasę, dodając szablon trasy ujęty w podwójne cudzysłowy po `@page` dyrektywie.</span><span class="sxs-lookup"><span data-stu-id="bb09a-357">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="bb09a-358">Korzystając z powyższego kodu, ścieżka URL, która jest `OnPostJoinListAsync` przesyłana do usługi, to `http://localhost:5000/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="bb09a-358">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="bb09a-359">Ścieżka URL, która jest przesyłana `OnPostJoinListUCAsync` do `http://localhost:5000/Customers/CreateFATH/JoinListUC`programu, to.</span><span class="sxs-lookup"><span data-stu-id="bb09a-359">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="bb09a-360">Poniżej przedstawiono `handler` , że parametr trasy jest opcjonalny. `?`</span><span class="sxs-lookup"><span data-stu-id="bb09a-360">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="bb09a-361">Konfiguracja i ustawienia</span><span class="sxs-lookup"><span data-stu-id="bb09a-361">Configuration and settings</span></span>

<span data-ttu-id="bb09a-362">Aby skonfigurować opcje zaawansowane, użyj metody `AddRazorPagesOptions` rozszerzenia w konstruktorze MVC:</span><span class="sxs-lookup"><span data-stu-id="bb09a-362">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="bb09a-363">Obecnie można użyć, `RazorPagesOptions` aby ustawić katalog główny dla stron lub dodać konwencje modelu aplikacji dla stron.</span><span class="sxs-lookup"><span data-stu-id="bb09a-363">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="bb09a-364">W przyszłości włączysz więcej rozszerzeń w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="bb09a-364">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="bb09a-365">Aby wstępnie skompilować widoki, zobacz [kompilacja widoku Razor](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="bb09a-365">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="bb09a-366">[Pobierz lub Wyświetl przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="bb09a-366">[Download or view sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="bb09a-367">Zobacz Rozpoczynanie [pracy z usługą Razor Pages](xref:tutorials/razor-pages/razor-pages-start), która kompiluje się w tym wprowadzeniu.</span><span class="sxs-lookup"><span data-stu-id="bb09a-367">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="bb09a-368">Określ, że Razor Pages znajdują się w katalogu głównym zawartości</span><span class="sxs-lookup"><span data-stu-id="bb09a-368">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="bb09a-369">Domyślnie Razor Pages są umieszczane w katalogu */Pages* .</span><span class="sxs-lookup"><span data-stu-id="bb09a-369">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="bb09a-370">Dodaj [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) , aby określić, że Razor Pages znajdują się w katalogu głównym zawartości ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) aplikacji:</span><span class="sxs-lookup"><span data-stu-id="bb09a-370">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="bb09a-371">Określ, że Razor Pages znajdują się w niestandardowym katalogu głównym</span><span class="sxs-lookup"><span data-stu-id="bb09a-371">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="bb09a-372">Dodaj [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) , aby określić, że Razor Pages znajdują się w niestandardowym katalogu głównym w aplikacji (podaj ścieżkę względną):</span><span class="sxs-lookup"><span data-stu-id="bb09a-372">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="bb09a-373">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="bb09a-373">Additional resources</span></span>

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>
