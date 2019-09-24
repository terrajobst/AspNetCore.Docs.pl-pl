---
title: Wprowadzenie do Razor Pages w ASP.NET Core
author: Rick-Anderson
description: Dowiedz się, jak stron Razor w programie ASP.NET Core umożliwia kodowania scenariuszy skoncentrowane na stronie łatwiejsze i bardziej wydajne niż przy użyciu platformy MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/19/2019
uid: razor-pages/index
ms.openlocfilehash: 7e27bbc84816bd282b2b8de936772eb5c597d74f
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/24/2019
ms.locfileid: "71207370"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="20583-103">Wprowadzenie do Razor Pages w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="20583-103">Introduction to Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="20583-104">[Rick Anderson](https://twitter.com/RickAndMSFT) i [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="20583-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="20583-105">Razor Pages może sprawiać, że kodowanie scenariuszy ukierunkowanych na strony jest łatwiejsze i wydajniejsze niż używanie kontrolerów i widoków.</span><span class="sxs-lookup"><span data-stu-id="20583-105">Razor Pages can make coding page-focused scenarios easier and more productive than using controllers and views.</span></span>

<span data-ttu-id="20583-106">Jeśli szukasz samouczka korzystającego z podejścia Model-View-Controller, zobacz Wprowadzenie do [ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="20583-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="20583-107">Ten dokument zawiera wprowadzenie do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="20583-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="20583-108">Nie jest to samouczek krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="20583-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="20583-109">Jeśli okaże się, że niektóre sekcje są zbyt zaawansowane, zobacz [wprowadzenie do Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="20583-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="20583-110">Aby zapoznać się z omówieniem ASP.NET Core, zobacz [wprowadzenie do ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="20583-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="20583-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="20583-111">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20583-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20583-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="20583-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="20583-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="20583-114">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="20583-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="20583-115">Tworzenie projektu Razor Pages</span><span class="sxs-lookup"><span data-stu-id="20583-115">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20583-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20583-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="20583-117">Aby uzyskać szczegółowe instrukcje dotyczące sposobu tworzenia projektu Razor Pages, zobacz Wprowadzenie do [Razor Pages](xref:tutorials/razor-pages/razor-pages-start) .</span><span class="sxs-lookup"><span data-stu-id="20583-117">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="20583-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="20583-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="20583-119">Uruchom `dotnet new webapp` polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="20583-119">Run `dotnet new webapp` from the command line.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="20583-120">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="20583-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="20583-121">Uruchom `dotnet new webapp` polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="20583-121">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="20583-122">Otwórz wygenerowany plik *csproj* z Visual Studio dla komputerów Mac.</span><span class="sxs-lookup"><span data-stu-id="20583-122">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="20583-123">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="20583-123">Razor Pages</span></span>

<span data-ttu-id="20583-124">Razor Pages jest włączona w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="20583-124">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Startup.cs?name=snippet_Startup&highlight=12)]

<span data-ttu-id="20583-125">Weź pod uwagę podstawową stronę:<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="20583-125">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index.cshtml?highlight=1)]

<span data-ttu-id="20583-126">Poprzedni kod wygląda podobnie jak [plik widoku Razor](xref:tutorials/first-mvc-app/adding-view) używany w aplikacji ASP.NET Core z kontrolerami i widokami.</span><span class="sxs-lookup"><span data-stu-id="20583-126">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="20583-127">Co sprawia, [@page](xref:mvc/views/razor#page) że jest to dyrektywa.</span><span class="sxs-lookup"><span data-stu-id="20583-127">What makes it different is the [@page](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="20583-128">`@page`sprawia, że plik jest akcją MVC, co oznacza, że obsługuje żądania bezpośrednio, bez przechodzenia przez kontroler.</span><span class="sxs-lookup"><span data-stu-id="20583-128">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="20583-129">`@page`musi być pierwszą dyrektywą Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="20583-129">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="20583-130">`@page`wpływa na zachowanie innych konstrukcji [Razor](xref:mvc/views/razor) .</span><span class="sxs-lookup"><span data-stu-id="20583-130">`@page` affects the behavior of other [Razor](xref:mvc/views/razor) constructs.</span></span> <span data-ttu-id="20583-131">Nazwy plików Razor Pages mają sufiks *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="20583-131">Razor Pages file names have a *.cshtml* suffix.</span></span>

<span data-ttu-id="20583-132">Podobna Strona, za pomocą `PageModel` klasy, jest pokazana w następujących dwóch plikach.</span><span class="sxs-lookup"><span data-stu-id="20583-132">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="20583-133">Plik *Pages/index2. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="20583-133">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="20583-134">Model strony *stron/index2. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="20583-134">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="20583-135">Zgodnie z Konwencją `PageModel` plik klasy ma taką samą nazwę jak plik stronicowania Razor z dołączonym rozszerzeniem *. cs* .</span><span class="sxs-lookup"><span data-stu-id="20583-135">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="20583-136">Na przykład Poprzednia strona Razor to *Pages/index2. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="20583-136">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="20583-137">Plik zawierający `PageModel` klasę ma nazwę Pages */index2. cshtml. cs*.</span><span class="sxs-lookup"><span data-stu-id="20583-137">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="20583-138">Skojarzenia ścieżek adresów URL ze stronami są określane przez lokalizację strony w systemie plików.</span><span class="sxs-lookup"><span data-stu-id="20583-138">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="20583-139">W poniższej tabeli przedstawiono ścieżkę strony Razor i pasujący adres URL:</span><span class="sxs-lookup"><span data-stu-id="20583-139">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="20583-140">Nazwa i ścieżka pliku</span><span class="sxs-lookup"><span data-stu-id="20583-140">File name and path</span></span>               | <span data-ttu-id="20583-141">pasujący adres URL</span><span class="sxs-lookup"><span data-stu-id="20583-141">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="20583-142">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="20583-142">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="20583-143">`/` lub `/Index`</span><span class="sxs-lookup"><span data-stu-id="20583-143">`/` or `/Index`</span></span> |
| <span data-ttu-id="20583-144">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="20583-144">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="20583-145">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="20583-145">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="20583-146">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="20583-146">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="20583-147">`/Store` lub `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="20583-147">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="20583-148">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="20583-148">Notes:</span></span>

* <span data-ttu-id="20583-149">Środowisko wykonawcze domyślnie wyszukuje pliki Razor Pages w folderze Pages.</span><span class="sxs-lookup"><span data-stu-id="20583-149">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="20583-150">`Index`jest stroną domyślną, gdy adres URL nie zawiera strony.</span><span class="sxs-lookup"><span data-stu-id="20583-150">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="20583-151">Napisz podstawowy formularz</span><span class="sxs-lookup"><span data-stu-id="20583-151">Write a basic form</span></span>

<span data-ttu-id="20583-152">Razor Pages zaprojektowano tak, aby wspólne wzorce używane z przeglądarkami sieci Web były łatwe do wdrożenia podczas kompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="20583-152">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="20583-153">[Powiązania modelu](xref:mvc/models/model-binding), [pomocników tagów](xref:mvc/views/tag-helpers/intro)i pomocników HTML same *działają* z właściwościami zdefiniowanymi w klasie strony Razor.</span><span class="sxs-lookup"><span data-stu-id="20583-153">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="20583-154">Weź pod uwagę stronę implementującą podstawowy formularz "contact us" (kontakt `Contact` z nami) dla modelu:</span><span class="sxs-lookup"><span data-stu-id="20583-154">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="20583-155">Przykłady w tym dokumencie `DbContext` są inicjowane w pliku [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) .</span><span class="sxs-lookup"><span data-stu-id="20583-155">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) file.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Startup.cs?name=snippet)]

<span data-ttu-id="20583-156">Model danych:</span><span class="sxs-lookup"><span data-stu-id="20583-156">The data model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="20583-157">Kontekst bazy danych:</span><span class="sxs-lookup"><span data-stu-id="20583-157">The db context:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Data/CustomerDbContext.cs)]

<span data-ttu-id="20583-158">Plik widoku *Pages/Create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="20583-158">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="20583-159">Model strony *Pages/Create. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="20583-159">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="20583-160">Zgodnie z Konwencją `PageModel` Klasa jest wywoływana `<PageName>Model` i znajduje się w tej samej przestrzeni nazw co strona.</span><span class="sxs-lookup"><span data-stu-id="20583-160">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="20583-161">`PageModel` Klasa umożliwia rozdzielenie logiki strony od jej prezentacji.</span><span class="sxs-lookup"><span data-stu-id="20583-161">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="20583-162">Definiuje procedury obsługi stron dla żądań wysyłanych do strony oraz dane używane do renderowania strony.</span><span class="sxs-lookup"><span data-stu-id="20583-162">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="20583-163">Ta separacja umożliwia:</span><span class="sxs-lookup"><span data-stu-id="20583-163">This separation allows:</span></span>

* <span data-ttu-id="20583-164">Zarządzanie zależnościami stron przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="20583-164">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* [<span data-ttu-id="20583-165">Testowanie jednostek</span><span class="sxs-lookup"><span data-stu-id="20583-165">Unit testing</span></span>](xref:test/razor-pages-tests)

<span data-ttu-id="20583-166">Strona ma `OnPostAsync` *metodę obsługi*, która jest uruchamiana na `POST` żądaniach (gdy użytkownik księguje formularz).</span><span class="sxs-lookup"><span data-stu-id="20583-166">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="20583-167">Można dodać metody obsługi dla dowolnego zlecenia HTTP.</span><span class="sxs-lookup"><span data-stu-id="20583-167">Handler methods for any HTTP verb can be added.</span></span> <span data-ttu-id="20583-168">Najczęstszymi obsłudze są:</span><span class="sxs-lookup"><span data-stu-id="20583-168">The most common handlers are:</span></span>

* <span data-ttu-id="20583-169">`OnGet`do zainicjowania stanu wymaganego dla strony.</span><span class="sxs-lookup"><span data-stu-id="20583-169">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="20583-170">W powyższym kodzie `OnGet` Metoda wyświetla stronę Razor *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="20583-170">In the preceding code, the `OnGet` method displays the *CreateModel.cshtml* Razor Page.</span></span>
* <span data-ttu-id="20583-171">`OnPost`do obsługi przesłanych formularzy.</span><span class="sxs-lookup"><span data-stu-id="20583-171">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="20583-172">Sufiks `Async` nazewnictwa jest opcjonalny, ale jest często używany przez Konwencję dla funkcji asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="20583-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="20583-173">Poprzedni kod jest typowy dla Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="20583-173">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="20583-174">Jeśli znasz już aplikacje ASP.NET przy użyciu kontrolerów i widoków:</span><span class="sxs-lookup"><span data-stu-id="20583-174">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="20583-175">`OnPostAsync` Kod w poprzednim przykładzie wygląda podobnie do typowego kodu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="20583-175">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="20583-176">Większość podstawowych elementów MVC, takich jak [powiązania modelu](xref:mvc/models/model-binding), [Walidacja](xref:mvc/models/validation)i akcje, działa tak samo z kontrolerami i Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="20583-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results work the same with Controllers and Razor Pages.</span></span> 

<span data-ttu-id="20583-177">Poprzednia `OnPostAsync` Metoda:</span><span class="sxs-lookup"><span data-stu-id="20583-177">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="20583-178">Podstawowy przepływ `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="20583-178">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="20583-179">Sprawdź, czy występują błędy walidacji.</span><span class="sxs-lookup"><span data-stu-id="20583-179">Check for validation errors.</span></span>

* <span data-ttu-id="20583-180">Jeśli nie ma żadnych błędów, Zapisz dane i Przekieruj.</span><span class="sxs-lookup"><span data-stu-id="20583-180">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="20583-181">W przypadku wystąpienia błędów ponownie Wyświetl stronę z komunikatami walidacji.</span><span class="sxs-lookup"><span data-stu-id="20583-181">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="20583-182">W wielu przypadkach błędy sprawdzania poprawności zostaną wykryte na kliencie i nigdy nie przesłano ich do serwera.</span><span class="sxs-lookup"><span data-stu-id="20583-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="20583-183">Plik widoku *Pages/Create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="20583-183">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="20583-184">Renderowany kod HTML ze *stron/Create. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="20583-184">The rendered HTML from *Pages/Create.cshtml*:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.html)]

<span data-ttu-id="20583-185">W poprzednim kodzie, ogłaszanie formularza:</span><span class="sxs-lookup"><span data-stu-id="20583-185">In the previous code, posting the form:</span></span>

* <span data-ttu-id="20583-186">Z prawidłowymi danymi:</span><span class="sxs-lookup"><span data-stu-id="20583-186">With valid data:</span></span>

  * <span data-ttu-id="20583-187">Metoda `OnPostAsync` obsługi <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> wywołuje metodę pomocnika.</span><span class="sxs-lookup"><span data-stu-id="20583-187">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> helper method.</span></span> <span data-ttu-id="20583-188">`RedirectToPage`Zwraca wystąpienie elementu <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span><span class="sxs-lookup"><span data-stu-id="20583-188">`RedirectToPage` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span></span> <span data-ttu-id="20583-189">`RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="20583-189">`RedirectToPage`:</span></span>

    * <span data-ttu-id="20583-190">Jest wynikiem akcji.</span><span class="sxs-lookup"><span data-stu-id="20583-190">Is an action result.</span></span>
    * <span data-ttu-id="20583-191">Jest podobny do `RedirectToAction` lub `RedirectToRoute` (używany w kontrolerach i widokach).</span><span class="sxs-lookup"><span data-stu-id="20583-191">Is similar to `RedirectToAction` or `RedirectToRoute` (used in controllers and views).</span></span>
    * <span data-ttu-id="20583-192">Jest dostosowywany dla stron.</span><span class="sxs-lookup"><span data-stu-id="20583-192">Is customized for pages.</span></span> <span data-ttu-id="20583-193">W powyższym przykładzie przekierowuje do strony indeksu głównego (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="20583-193">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="20583-194">`RedirectToPage`szczegółowo znajduje się w sekcji [generowanie adresów URL dla stron](#url_gen) .</span><span class="sxs-lookup"><span data-stu-id="20583-194">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

* <span data-ttu-id="20583-195">Z błędami walidacji, które są przesyłane do serwera:</span><span class="sxs-lookup"><span data-stu-id="20583-195">With validation errors that are passed to the server:</span></span>

  * <span data-ttu-id="20583-196">Metoda `OnPostAsync` obsługi <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> wywołuje metodę pomocnika.</span><span class="sxs-lookup"><span data-stu-id="20583-196">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> helper method.</span></span> <span data-ttu-id="20583-197">`Page`Zwraca wystąpienie elementu <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>.</span><span class="sxs-lookup"><span data-stu-id="20583-197">`Page` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>.</span></span> <span data-ttu-id="20583-198">Zwracanie `Page` jest podobne do sposobu, w jaki akcje `View`w kontrolerach zwracają.</span><span class="sxs-lookup"><span data-stu-id="20583-198">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="20583-199">`PageResult`jest domyślnym typem zwracanym dla metody obsługi.</span><span class="sxs-lookup"><span data-stu-id="20583-199">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="20583-200">Metoda obsługi, która zwraca `void` renderowanie strony.</span><span class="sxs-lookup"><span data-stu-id="20583-200">A handler method that returns `void` renders the page.</span></span>
  * <span data-ttu-id="20583-201">W poprzednim przykładzie, księgowanie formularza bez wartości powoduje, że [ModelState. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) zwraca wartość false.</span><span class="sxs-lookup"><span data-stu-id="20583-201">In the preceding example, posting the form with no value results in [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) returning false.</span></span> <span data-ttu-id="20583-202">W tym przykładzie na kliencie nie są wyświetlane błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="20583-202">In this sample, no validation errors are displayed on the client.</span></span> <span data-ttu-id="20583-203">Przekazywanie błędów sprawdzania poprawności jest omówione w dalszej części tego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="20583-203">Validation error handing is covered later in this document.</span></span>

  [!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

* <span data-ttu-id="20583-204">Z błędami walidacji wykrytymi przez weryfikację po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="20583-204">With validation errors detected by client side validation:</span></span>

  * <span data-ttu-id="20583-205">Dane **nie** są ogłaszane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="20583-205">Data is **not** posted to the server.</span></span>
  * <span data-ttu-id="20583-206">Sprawdzanie poprawności po stronie klienta zostało wyjaśnione w dalszej części tego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="20583-206">Client-side validation is explained later in this document.</span></span>

<span data-ttu-id="20583-207">`Customer` Właściwość [używa`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) atrybutu, aby zrezygnować z powiązania modelu:</span><span class="sxs-lookup"><span data-stu-id="20583-207">The `Customer` property uses [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute to opt in to model binding:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=15-16)]

<span data-ttu-id="20583-208">`[BindProperty]`**nie** powinien być używany dla modeli zawierających właściwości, które nie powinny być zmieniane przez klienta.</span><span class="sxs-lookup"><span data-stu-id="20583-208">`[BindProperty]` should **not** be used on models containing properties that should not be changed by the client.</span></span> <span data-ttu-id="20583-209">Aby uzyskać więcej informacji, zobacz temat [przefinalizowanie](xref:data/ef-rp/crud#overposting)</span><span class="sxs-lookup"><span data-stu-id="20583-209">For more information, see [Overposting](xref:data/ef-rp/crud#overposting)</span></span>

<span data-ttu-id="20583-210">Razor Pages domyślnie Powiąż właściwości tylko z`GET` niezleceniami.</span><span class="sxs-lookup"><span data-stu-id="20583-210">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="20583-211">Powiązanie z właściwościami eliminuje konieczność pisania kodu w celu przekonwertowania danych HTTP na typ modelu.</span><span class="sxs-lookup"><span data-stu-id="20583-211">Binding to properties removes the need to writing code to convert HTTP data to the model type.</span></span> <span data-ttu-id="20583-212">Powiązanie zmniejsza kod, używając tej samej właściwości do renderowania pól formularza (`<input asp-for="Customer.Name">`) i akceptuję dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="20583-212">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="20583-213">Przeglądanie pliku widoku *stron/Create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="20583-213">Reviewing the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml?highlight=3,9)]

* <span data-ttu-id="20583-214">W poprzednim kodzie `<input asp-for="Customer.Name" />` [pomocnik tagów wejściowych](xref:mvc/views/working-with-forms#the-input-tag-helper) wiąże element `Customer.Name` HTML `<input>` z wyrażeniem modelu.</span><span class="sxs-lookup"><span data-stu-id="20583-214">In the preceding code, the [input tag helper](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` binds the HTML `<input>` element to the `Customer.Name` model expression.</span></span>
* <span data-ttu-id="20583-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available)udostępnia pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="20583-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) makes Tag Helpers available.</span></span>

### <a name="the-home-page"></a><span data-ttu-id="20583-216">Strona główna</span><span class="sxs-lookup"><span data-stu-id="20583-216">The home page</span></span>

<span data-ttu-id="20583-217">*Index. cshtml* to Strona główna:</span><span class="sxs-lookup"><span data-stu-id="20583-217">*Index.cshtml* is the home page:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml)]

<span data-ttu-id="20583-218">Skojarzona `PageModel` Klasa (*index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="20583-218">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="20583-219">Plik *index. cshtml* zawiera następujące znaczniki:</span><span class="sxs-lookup"><span data-stu-id="20583-219">The *Index.cshtml* file contains the following markup:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=21)]

<span data-ttu-id="20583-220">[Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) użył atrybutuwceluwygenerowanialinkudostronyedycji.`asp-route-{value}` `<a /a>`</span><span class="sxs-lookup"><span data-stu-id="20583-220">The `<a /a>`[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="20583-221">Link zawiera dane trasy z IDENTYFIKATORem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="20583-221">The link contains route data with the contact ID.</span></span> <span data-ttu-id="20583-222">Na przykład `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="20583-222">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="20583-223">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="20583-223">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>

<span data-ttu-id="20583-224">Plik *index. cshtml* zawiera znaczniki umożliwiające utworzenie przycisku usuwania dla każdego kontaktu z klientem:</span><span class="sxs-lookup"><span data-stu-id="20583-224">The *Index.cshtml* file contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=22-23)]

<span data-ttu-id="20583-225">Renderowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="20583-225">The rendered HTML:</span></span>

```HTML
<button type="submit" formaction="/Customers?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="20583-226">Gdy przycisk Usuń jest renderowany w języku HTML, jego [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) zawiera parametry dla:</span><span class="sxs-lookup"><span data-stu-id="20583-226">When the delete button is rendered in HTML, its [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) includes parameters for:</span></span>

* <span data-ttu-id="20583-227">Identyfikator osoby kontaktowej klienta określony przez `asp-route-id` atrybut.</span><span class="sxs-lookup"><span data-stu-id="20583-227">The customer contact ID, specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="20583-228">`handler`, Określony`asp-page-handler` przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="20583-228">The `handler`, specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="20583-229">Po wybraniu przycisku do serwera zostanie wysłane `POST` żądanie formularza.</span><span class="sxs-lookup"><span data-stu-id="20583-229">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="20583-230">Według Konwencji, nazwa metody obsługi jest wybierana na podstawie wartości `handler` parametru zgodnie z schematem. `OnPost[handler]Async`</span><span class="sxs-lookup"><span data-stu-id="20583-230">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="20583-231">`handler` Ponieważ jest `delete` w`OnPostDeleteAsync` tym przykładzie, metoda obsługi jest używana do przetwarzania `POST` żądania.</span><span class="sxs-lookup"><span data-stu-id="20583-231">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="20583-232">Jeśli jest ustawiona na inną wartość, na przykład `remove`, jest wybierana metoda obsługi o nazwie `OnPostRemoveAsync`. `asp-page-handler`</span><span class="sxs-lookup"><span data-stu-id="20583-232">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="20583-233">`OnPostDeleteAsync` Metody:</span><span class="sxs-lookup"><span data-stu-id="20583-233">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="20583-234">`id` Pobiera z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="20583-234">Gets the `id` from the query string.</span></span>
* <span data-ttu-id="20583-235">Wysyła zapytanie do bazy danych w celu skontaktowania się z klientem za pomocą `FindAsync`programu.</span><span class="sxs-lookup"><span data-stu-id="20583-235">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="20583-236">Jeśli kontakt z klientem zostanie znaleziony, zostanie on usunięty, a baza danych zostanie zaktualizowana.</span><span class="sxs-lookup"><span data-stu-id="20583-236">If the customer contact is found, it's removed and the database is updated.</span></span>
* <span data-ttu-id="20583-237">Wywołania <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> do przekierowania na stronę indeksu głównego (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="20583-237">Calls <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> to redirect to the root Index page (`/Index`).</span></span>

### <a name="the-editcshtml-file"></a><span data-ttu-id="20583-238">Plik Edit. cshtml</span><span class="sxs-lookup"><span data-stu-id="20583-238">The Edit.cshtml file</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml?highlight=1)]

<span data-ttu-id="20583-239">Pierwszy wiersz zawiera `@page "{id:int}"` dyrektywę.</span><span class="sxs-lookup"><span data-stu-id="20583-239">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="20583-240">Ograniczenie`"{id:int}"` routingu instruuje stronę, aby akceptowała żądania do strony `int` zawierającej dane trasy.</span><span class="sxs-lookup"><span data-stu-id="20583-240">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="20583-241">Jeśli żądanie do strony nie zawiera danych trasy, które można przekonwertować na obiekt `int`, środowisko uruchomieniowe zwróci błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="20583-241">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="20583-242">Aby identyfikator był opcjonalny, Dołącz `?` do ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="20583-242">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="20583-243">Plik *Edit.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="20583-243">The *Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml.cs?name=snippet)]

## <a name="validation"></a><span data-ttu-id="20583-244">Walidacja</span><span class="sxs-lookup"><span data-stu-id="20583-244">Validation</span></span>

<span data-ttu-id="20583-245">Reguły walidacji:</span><span class="sxs-lookup"><span data-stu-id="20583-245">Validation rules:</span></span>

* <span data-ttu-id="20583-246">Są deklaratywnie określone w klasie modelu.</span><span class="sxs-lookup"><span data-stu-id="20583-246">Are declaratively specified in the model class.</span></span>
* <span data-ttu-id="20583-247">Są wymuszane wszędzie w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="20583-247">Are enforced everywhere in the app.</span></span>

<span data-ttu-id="20583-248"><xref:System.ComponentModel.DataAnnotations> Przestrzeń nazw zawiera zestaw wbudowanych atrybutów walidacji, które są stosowane deklaratywnie do klasy lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="20583-248">The <xref:System.ComponentModel.DataAnnotations> namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="20583-249">Adnotacje DataAnnotation zawierają również atrybuty [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) formatowania, takie jak pomoc dotycząca formatowania i nie zapewniają weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="20583-249">DataAnnotations also contains formatting attributes like [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) that help with formatting and don't provide any validation.</span></span>

<span data-ttu-id="20583-250">Rozważmy `Customer` model:</span><span class="sxs-lookup"><span data-stu-id="20583-250">Consider the `Customer` model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="20583-251">Za pomocą następującego pliku widoku *Create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="20583-251">Using the following *Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=3,8-9,15-99)]

<span data-ttu-id="20583-252">Powyższy kod:</span><span class="sxs-lookup"><span data-stu-id="20583-252">The preceding code:</span></span>

* <span data-ttu-id="20583-253">Obejmuje skrypty sprawdzania poprawności jQuery i jQuery.</span><span class="sxs-lookup"><span data-stu-id="20583-253">Includes jQuery and jQuery validation scripts.</span></span>
* <span data-ttu-id="20583-254">Używa pomocników `<span />` [tagów](xref:mvc/views/tag-helpers/intro) i do włączania `<div />` :</span><span class="sxs-lookup"><span data-stu-id="20583-254">Uses the `<div />` and `<span />` [Tag Helpers](xref:mvc/views/tag-helpers/intro) to enable:</span></span>

  * <span data-ttu-id="20583-255">Sprawdzanie poprawności po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="20583-255">Client-side validation.</span></span>
  * <span data-ttu-id="20583-256">Renderowanie błędów walidacji.</span><span class="sxs-lookup"><span data-stu-id="20583-256">Validation error rendering.</span></span>

* <span data-ttu-id="20583-257">Generuje następujący kod HTML:[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create5.html)]</span><span class="sxs-lookup"><span data-stu-id="20583-257">Generates the following HTML: [!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create5.html)]</span></span>

<span data-ttu-id="20583-258">Opublikowanie formularza tworzenia bez wartości nazwa powoduje wyświetlenie komunikatu o błędzie "Nazwa pola jest wymagana."</span><span class="sxs-lookup"><span data-stu-id="20583-258">Posting the Create form without a name value displays the error message "The Name field is required."</span></span> <span data-ttu-id="20583-259">w formularzu.</span><span class="sxs-lookup"><span data-stu-id="20583-259">on the form.</span></span> <span data-ttu-id="20583-260">Jeśli na kliencie jest włączona obsługa języka JavaScript, przeglądarka wyświetli komunikat o błędzie bez publikowania na serwerze.</span><span class="sxs-lookup"><span data-stu-id="20583-260">If JavaScript is enabled on the client, the browser displays the error without posting to the server.</span></span>

<span data-ttu-id="20583-261">Ten `[StringLength(10)]` atrybut jest `data-val-length-max="10"` generowany na renderowanym kodzie HTML.</span><span class="sxs-lookup"><span data-stu-id="20583-261">The `[StringLength(10)]` attribute generates `data-val-length-max="10"` on the rendered HTML.</span></span> <span data-ttu-id="20583-262">`data-val-length-max`zapobiega wprowadzaniu przez przeglądarki więcej niż określoną maksymalną długość.</span><span class="sxs-lookup"><span data-stu-id="20583-262">`data-val-length-max` prevents browsers from entering more than the maximum length specified.</span></span> <span data-ttu-id="20583-263">Jeśli jest używane narzędzie, takie jak [programu Fiddler](https://www.telerik.com/fiddler) , do edytowania i powtarzania wpisu:</span><span class="sxs-lookup"><span data-stu-id="20583-263">If a tool such as [Fiddler](https://www.telerik.com/fiddler) is used to edit and replay the post:</span></span>

* <span data-ttu-id="20583-264">O nazwie dłuższej niż 10.</span><span class="sxs-lookup"><span data-stu-id="20583-264">With the name longer than 10.</span></span>
* <span data-ttu-id="20583-265">Komunikat o błędzie "Nazwa pola musi być ciągiem o maksymalnej długości 10".</span><span class="sxs-lookup"><span data-stu-id="20583-265">The error message "The field Name must be a string with a maximum length of 10."</span></span> <span data-ttu-id="20583-266">jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="20583-266">is returned.</span></span>

<span data-ttu-id="20583-267">Rozważmy następujący `Movie` model:</span><span class="sxs-lookup"><span data-stu-id="20583-267">Consider the following `Movie` model:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="20583-268">Atrybuty walidacji określają zachowanie do wymuszania na właściwościach modelu, do których są stosowane:</span><span class="sxs-lookup"><span data-stu-id="20583-268">The validation attributes specify behavior to enforce on the model properties they're applied to:</span></span>

* <span data-ttu-id="20583-269">Atrybuty `Required` i`MinimumLength` wskazują, że właściwość musi mieć wartość, ale nic nie zapobiega wprowadzaniu przez użytkownika odstępu w celu zaspokojenia tej walidacji.</span><span class="sxs-lookup"><span data-stu-id="20583-269">The `Required` and `MinimumLength` attributes indicate that a property must have a value; but nothing prevents a user from entering white space to satisfy this validation.</span></span>
* <span data-ttu-id="20583-270">Ten `RegularExpression` atrybut służy do ograniczania, jakie znaki mogą być wprowadzane.</span><span class="sxs-lookup"><span data-stu-id="20583-270">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="20583-271">W poprzednim kodzie "gatunek":</span><span class="sxs-lookup"><span data-stu-id="20583-271">In the preceding code, "Genre":</span></span>

  * <span data-ttu-id="20583-272">Należy używać tylko liter.</span><span class="sxs-lookup"><span data-stu-id="20583-272">Must only use letters.</span></span>
  * <span data-ttu-id="20583-273">Pierwsza litera musi być wielką literą.</span><span class="sxs-lookup"><span data-stu-id="20583-273">The first letter is required to be uppercase.</span></span> <span data-ttu-id="20583-274">Odstępy, cyfry i znaki specjalne są niedozwolone.</span><span class="sxs-lookup"><span data-stu-id="20583-274">White space, numbers, and special characters are not allowed.</span></span>

* <span data-ttu-id="20583-275">`RegularExpression` "Ocena":</span><span class="sxs-lookup"><span data-stu-id="20583-275">The `RegularExpression` "Rating":</span></span>

  * <span data-ttu-id="20583-276">Wymaga, aby pierwszy znak był wielką literą.</span><span class="sxs-lookup"><span data-stu-id="20583-276">Requires that the first character be an uppercase letter.</span></span>
  * <span data-ttu-id="20583-277">Zezwala na znaki specjalne i cyfry w kolejnych odstępach.</span><span class="sxs-lookup"><span data-stu-id="20583-277">Allows special characters and numbers in subsequent spaces.</span></span> <span data-ttu-id="20583-278">"PG-13" jest prawidłowy dla oceny, ale kończy się niepowodzeniem dla "gatunku".</span><span class="sxs-lookup"><span data-stu-id="20583-278">"PG-13" is valid for a rating, but fails for a "Genre".</span></span>

* <span data-ttu-id="20583-279">`Range` Atrybut ogranicza wartość do określonego zakresu.</span><span class="sxs-lookup"><span data-stu-id="20583-279">The `Range` attribute constrains a value to within a specified range.</span></span>
* <span data-ttu-id="20583-280">`StringLength` Atrybut ustawia maksymalną długość właściwości ciągu i opcjonalnie jej długość minimalną.</span><span class="sxs-lookup"><span data-stu-id="20583-280">The `StringLength` attribute sets the maximum length of a string property, and optionally its minimum length.</span></span>
* <span data-ttu-id="20583-281">Typy wartości (takie jak `decimal` `float`, `int` `DateTime`,,) są z założenia wymagane i nie wymagają `[Required]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="20583-281">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="20583-282">Na stronie Tworzenie dla `Movie` modelu są wyświetlane błędy z nieprawidłowymi wartościami:</span><span class="sxs-lookup"><span data-stu-id="20583-282">The Create page for the `Movie` model shows displays errors with invalid values:</span></span>

![Formularz widoku filmu z wieloma błędami walidacji po stronie klienta jQuery](~/tutorials/razor-pages/validation/_static/val.png)

<span data-ttu-id="20583-284">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="20583-284">For more information, see:</span></span>

* [<span data-ttu-id="20583-285">Dodawanie walidacji do aplikacji filmowej</span><span class="sxs-lookup"><span data-stu-id="20583-285">Add validation to the Movie app</span></span>](xref:tutorials/razor-pages/validation)
* <span data-ttu-id="20583-286">[Walidacja modelu w ASP.NET Core](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="20583-286">[Model validation in ASP.NET Core](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="20583-287">Obsługa żądań głównych przy użyciu rezerwy procedury obsługi OnGet</span><span class="sxs-lookup"><span data-stu-id="20583-287">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="20583-288">`HEAD`żądania umożliwiają pobranie nagłówków dla określonego zasobu.</span><span class="sxs-lookup"><span data-stu-id="20583-288">`HEAD` requests allow retrieving the headers for a specific resource.</span></span> <span data-ttu-id="20583-289">W przeciwieństwie `GET` do `HEAD` żądań, żądania nie zwracają treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="20583-289">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="20583-290">Zwykle program obsługi jest tworzony i wywoływany dla `HEAD` żądań: `OnHead`</span><span class="sxs-lookup"><span data-stu-id="20583-290">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Privacy.cshtml.cs?name=snippet)]

<span data-ttu-id="20583-291">Razor Pages powracać do wywoływania `OnGet` procedury obsługi, `OnHead` Jeśli nie zdefiniowano procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="20583-291">Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="20583-292">XSRF/CSRF i Razor Pages</span><span class="sxs-lookup"><span data-stu-id="20583-292">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="20583-293">Razor Pages są chronione przez[weryfikację przed fałszerstwem](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="20583-293">Razor Pages are protected by[Antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="20583-294">[FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) wprowadza do elementów formularza HTML tokeny zabezpieczające przed fałszerstwem.</span><span class="sxs-lookup"><span data-stu-id="20583-294">The [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="20583-295">Używanie układów, częściowych, szablonów i pomocników tagów z Razor Pages</span><span class="sxs-lookup"><span data-stu-id="20583-295">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="20583-296">Strony współpracują ze wszystkimi funkcjami aparatu widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="20583-296">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="20583-297">Układy, części, szablony, pomocników tagów, *_ViewStart. cshtml*, *_ViewImports. cshtml* działają w taki sam sposób, jak w przypadku konwencjonalnych widoków Razor.</span><span class="sxs-lookup"><span data-stu-id="20583-297">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="20583-298">Zanotujmy Tę stronę, korzystając z zalet niektórych z tych funkcji.</span><span class="sxs-lookup"><span data-stu-id="20583-298">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="20583-299">Dodaj [stronę układu](xref:mvc/views/layout) do *stron/Shared/_Layout. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="20583-299">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Shared/_Layout2.cshtml?hightlight=12)]

<span data-ttu-id="20583-300">[Układ](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="20583-300">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="20583-301">Steruje układem każdej strony (chyba że strona nie jest częścią układu).</span><span class="sxs-lookup"><span data-stu-id="20583-301">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="20583-302">Importuje struktury HTML, takie jak JavaScript i stylesheets.</span><span class="sxs-lookup"><span data-stu-id="20583-302">Imports HTML structures such as JavaScript and stylesheets.</span></span>
* <span data-ttu-id="20583-303">Zawartość strony Razor jest renderowana, gdzie `@RenderBody()` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="20583-303">The contents of the Razor page are rendered where `@RenderBody()` is called.</span></span>

<span data-ttu-id="20583-304">Aby uzyskać więcej informacji, zobacz [stronę układu](xref:mvc/views/layout)...</span><span class="sxs-lookup"><span data-stu-id="20583-304">For more information, see [layout page](xref:mvc/views/layout)..</span></span>

<span data-ttu-id="20583-305">Właściwość [układu](xref:mvc/views/layout#specifying-a-layout) jest ustawiana na *stronie/_ViewStart. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="20583-305">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="20583-306">Układ znajduje się w *stronie/* w folderze udostępnionym.</span><span class="sxs-lookup"><span data-stu-id="20583-306">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="20583-307">Strony szukają innych widoków (układów, szablonów, częściowych) hierarchicznie, rozpoczynając w tym samym folderze, w którym znajduje się bieżąca strona.</span><span class="sxs-lookup"><span data-stu-id="20583-307">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="20583-308">Układ na *stronach/* w folderze udostępnionym może być używany z dowolnej strony Razor w folderze *Pages* .</span><span class="sxs-lookup"><span data-stu-id="20583-308">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="20583-309">Plik układu powinien przejść do *stron/folderu udostępnionego* .</span><span class="sxs-lookup"><span data-stu-id="20583-309">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="20583-310">Zalecamy **umieszczenie pliku** układu w widokach */* folderze udostępnionym.</span><span class="sxs-lookup"><span data-stu-id="20583-310">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="20583-311">*Widoki/udostępnione* są wzorcem widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="20583-311">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="20583-312">Razor Pages są przeznaczone do korzystania z hierarchii folderów, a nie Konwencji ścieżek.</span><span class="sxs-lookup"><span data-stu-id="20583-312">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="20583-313">Widok wyszukiwania na stronie Razor zawiera folder *strony* .</span><span class="sxs-lookup"><span data-stu-id="20583-313">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="20583-314">Układy, szablony i częściowe używane z kontrolerami MVC i konwencjonalnymi widokami Razor *same działają*.</span><span class="sxs-lookup"><span data-stu-id="20583-314">The layouts, templates, and partials used with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="20583-315">Dodaj plik *Pages/_ViewImports. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="20583-315">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="20583-316">`@namespace`wyjaśniono w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="20583-316">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="20583-317">Dyrektywa `@addTagHelper` znajduje się w [wbudowanych pomocników tagów](xref:mvc/views/tag-helpers/builtin-th/Index) do wszystkich stron w folderze *strony* .</span><span class="sxs-lookup"><span data-stu-id="20583-317">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="20583-318">`@namespace` Dyrektywa ustawiona na stronie:</span><span class="sxs-lookup"><span data-stu-id="20583-318">The `@namespace` directive set on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="20583-319">`@namespace` Dyrektywa ustawia przestrzeń nazw dla strony.</span><span class="sxs-lookup"><span data-stu-id="20583-319">The `@namespace` directive sets the namespace for the page.</span></span> <span data-ttu-id="20583-320">`@model` Dyrektywa nie musi zawierać przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="20583-320">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="20583-321">Gdy dyrektywa jest zawarta w *_ViewImports. cshtml*, określona przestrzeń nazw udostępnia prefiks dla wygenerowanej przestrzeni nazw na `@namespace` stronie, która importuje dyrektywę. `@namespace`</span><span class="sxs-lookup"><span data-stu-id="20583-321">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="20583-322">Pozostała część wygenerowanej przestrzeni nazw (część sufiksu) jest ścieżką względną oddzieloną kropką między folderem zawierającym *_ViewImports. cshtml* i folderem zawierającym stronę.</span><span class="sxs-lookup"><span data-stu-id="20583-322">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="20583-323">Na przykład `PageModel` Klasa Pages */Customers/Edit. cshtml. cs* jawnie ustawia przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="20583-323">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="20583-324">Plik *Pages/_ViewImports. cshtml* ustawia następującą przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="20583-324">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="20583-325">Wygenerowana przestrzeń nazw dla strony */Customers/Edit. cshtml* Razor jest taka sama jak `PageModel` Klasa.</span><span class="sxs-lookup"><span data-stu-id="20583-325">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="20583-326">`@namespace`*działa również z konwencjonalnymi widokami Razor.*</span><span class="sxs-lookup"><span data-stu-id="20583-326">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="20583-327">Rozważ użycie pliku widoku *Pages/Create. cshtml* :[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=2-3)]</span><span class="sxs-lookup"><span data-stu-id="20583-327">Consider the *Pages/Create.cshtml* view file: [!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=2-3)]</span></span>

<span data-ttu-id="20583-328">Zaktualizowane *strony/Utwórz plik widoku. cshtml* z *_ViewImports. cshtml* i poprzedni plik układu:</span><span class="sxs-lookup"><span data-stu-id="20583-328">The updated *Pages/Create.cshtml* view file with *_ViewImports.cshtml* and the preceding layout file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.cshtml?highlight=2)]

<span data-ttu-id="20583-329">W poprzednim kodzie *_ViewImports. cshtml* zaimportował przestrzeń nazw i pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="20583-329">In the preceding code, the *_ViewImports.cshtml* imported the namespace and Tag Helpers.</span></span> <span data-ttu-id="20583-330">Plik układu zaimportował pliki JavaScript.</span><span class="sxs-lookup"><span data-stu-id="20583-330">The layout file imported the JavaScript files.</span></span>

<span data-ttu-id="20583-331">[Razor Pages początkowy projekt](#rpvs17) zawiera *stronę/_ValidationScriptsPartial. cshtml*, który łączy weryfikację po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="20583-331">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="20583-332">Aby uzyskać więcej informacji o widokach częściowych, zobacz <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="20583-332">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="20583-333">Generowanie adresu URL dla stron</span><span class="sxs-lookup"><span data-stu-id="20583-333">URL generation for Pages</span></span>

<span data-ttu-id="20583-334">Strona, pokazana wcześniej, używa `RedirectToPage`: `Create`</span><span class="sxs-lookup"><span data-stu-id="20583-334">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=28)]

<span data-ttu-id="20583-335">Aplikacja ma następującą strukturę plików/folderów:</span><span class="sxs-lookup"><span data-stu-id="20583-335">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="20583-336">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="20583-336">*/Pages*</span></span>

  * <span data-ttu-id="20583-337">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="20583-337">*Index.cshtml*</span></span>
  * <span data-ttu-id="20583-338">*Privacy. cshtml*</span><span class="sxs-lookup"><span data-stu-id="20583-338">*Privacy.cshtml*</span></span>
  * <span data-ttu-id="20583-339">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="20583-339">*/Customers*</span></span>

    * <span data-ttu-id="20583-340">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="20583-340">*Create.cshtml*</span></span>
    * <span data-ttu-id="20583-341">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="20583-341">*Edit.cshtml*</span></span>
    * <span data-ttu-id="20583-342">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="20583-342">*Index.cshtml*</span></span>

<span data-ttu-id="20583-343">Strony */Customers/Create. cshtml* i *Pages/Customers/Edit. cshtml* przekierowywać do *stron/Customers/index. cshtml* po powodzeniu.</span><span class="sxs-lookup"><span data-stu-id="20583-343">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Customers/Index.cshtml* after success.</span></span> <span data-ttu-id="20583-344">Ciąg `./Index` jest względną nazwą strony używaną w celu uzyskania dostępu do poprzedniej strony.</span><span class="sxs-lookup"><span data-stu-id="20583-344">The string `./Index` is a relative page name used to access the preceding page.</span></span> <span data-ttu-id="20583-345">Służy do generowania adresów URL na stronie *strony/klienci/index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="20583-345">It is used to generate URLs to the *Pages/Customers/Index.cshtml* page.</span></span> <span data-ttu-id="20583-346">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="20583-346">For example:</span></span>

* `Url.Page("./Index", ...)`
* `<a asp-page="./Index">Customers Index Page</a>`
* `RedirectToPage("./Index")`

<span data-ttu-id="20583-347">Bezwzględna nazwa `/Index` strony służy do generowania adresów URL na stronie *stron/index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="20583-347">The absolute page name `/Index` is used to generate URLs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="20583-348">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="20583-348">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">Home Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="20583-349">Nazwa strony jest ścieżką do strony z folderu głównego */Pages* , włącznie z wiodącym `/` `/Index`(na przykład).</span><span class="sxs-lookup"><span data-stu-id="20583-349">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="20583-350">Powyższe przykłady generowania adresów URL oferują udoskonalone Opcje i możliwości funkcjonalne w porównaniu z zakodowanym adresem URL.</span><span class="sxs-lookup"><span data-stu-id="20583-350">The preceding URL generation samples offer enhanced options and functional capabilities over hard-coding a URL.</span></span> <span data-ttu-id="20583-351">Generowanie adresów URL używa [routingu](xref:mvc/controllers/routing) i może generować i kodować parametry zgodnie ze sposobem zdefiniowania trasy w ścieżce docelowej.</span><span class="sxs-lookup"><span data-stu-id="20583-351">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="20583-352">Generowanie adresów URL dla stron obsługuje nazwy względne.</span><span class="sxs-lookup"><span data-stu-id="20583-352">URL generation for pages supports relative names.</span></span> <span data-ttu-id="20583-353">W poniższej tabeli przedstawiono, która strona indeksu została wybrana przy `RedirectToPage` użyciu różnych parametrów na stronie */Customers/Create. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="20583-353">The following table shows which Index page is selected using different `RedirectToPage` parameters in *Pages/Customers/Create.cshtml*.</span></span>

| <span data-ttu-id="20583-354">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="20583-354">RedirectToPage(x)</span></span>| <span data-ttu-id="20583-355">Stronic</span><span class="sxs-lookup"><span data-stu-id="20583-355">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="20583-356">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="20583-356">RedirectToPage("/Index")</span></span> | <span data-ttu-id="20583-357">*Strony/indeks*</span><span class="sxs-lookup"><span data-stu-id="20583-357">*Pages/Index*</span></span> |
| <span data-ttu-id="20583-358">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="20583-358">RedirectToPage("./Index");</span></span> | <span data-ttu-id="20583-359">*Strony/klienci/indeks*</span><span class="sxs-lookup"><span data-stu-id="20583-359">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="20583-360">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="20583-360">RedirectToPage("../Index")</span></span> | <span data-ttu-id="20583-361">*Strony/indeks*</span><span class="sxs-lookup"><span data-stu-id="20583-361">*Pages/Index*</span></span> |
| <span data-ttu-id="20583-362">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="20583-362">RedirectToPage("Index")</span></span>  | <span data-ttu-id="20583-363">*Strony/klienci/indeks*</span><span class="sxs-lookup"><span data-stu-id="20583-363">*Pages/Customers/Index*</span></span> |

<!-- Test via ~/razor-pages/index/3.0sample/RazorPagesContacts/Pages/Customers/Details.cshtml.cs -->

<span data-ttu-id="20583-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, i `RedirectToPage("../Index")` są *nazwami względnymi*.</span><span class="sxs-lookup"><span data-stu-id="20583-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")` are *relative names*.</span></span> <span data-ttu-id="20583-365">Parametr jest połączony ze ścieżką bieżącej strony, aby obliczyć nazwę strony docelowej. `RedirectToPage`</span><span class="sxs-lookup"><span data-stu-id="20583-365">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>

<span data-ttu-id="20583-366">Łączenie nazw względnych jest przydatne podczas kompilowania lokacji ze złożoną strukturą.</span><span class="sxs-lookup"><span data-stu-id="20583-366">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="20583-367">Gdy nazwy względne są używane do łączenia między stronami w folderze:</span><span class="sxs-lookup"><span data-stu-id="20583-367">When relative names are used to link between pages in a folder:</span></span>

* <span data-ttu-id="20583-368">Zmiana nazwy folderu nie powoduje zerwania linków względnych.</span><span class="sxs-lookup"><span data-stu-id="20583-368">Renaming a folder doesn't break the relative links.</span></span>
* <span data-ttu-id="20583-369">Linki nie są przerwane, ponieważ nie zawierają nazwy folderu.</span><span class="sxs-lookup"><span data-stu-id="20583-369">Links are not broken because they don't include the folder name.</span></span>

<span data-ttu-id="20583-370">Aby przekierować do strony w innym [obszarze](xref:mvc/controllers/areas), określ obszar:</span><span class="sxs-lookup"><span data-stu-id="20583-370">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="20583-371">Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/areas> i <xref:razor-pages/razor-pages-conventions>.</span><span class="sxs-lookup"><span data-stu-id="20583-371">For more information, see <xref:mvc/controllers/areas> and <xref:razor-pages/razor-pages-conventions>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="20583-372">ViewData — atrybut</span><span class="sxs-lookup"><span data-stu-id="20583-372">ViewData attribute</span></span>

<span data-ttu-id="20583-373">Dane można przekazywać do strony za pomocą <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>.</span><span class="sxs-lookup"><span data-stu-id="20583-373">Data can be passed to a page with <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>.</span></span> <span data-ttu-id="20583-374">Właściwości z atrybutem [ViewData] mają przechowywane i ładowane wartości z <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>.</span><span class="sxs-lookup"><span data-stu-id="20583-374">Properties with the [ViewData] attribute have their values stored and loaded from the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>.</span></span>

<span data-ttu-id="20583-375">W poniższym przykładzie `AboutModel` `[ViewData]` stosuje atrybut do `Title` właściwości:</span><span class="sxs-lookup"><span data-stu-id="20583-375">In the following example, the `AboutModel` applies the `[ViewData]` attribute to the `Title` property:</span></span>

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

<span data-ttu-id="20583-376">Na stronie informacje uzyskaj dostęp do `Title` właściwości jako właściwości modelu:</span><span class="sxs-lookup"><span data-stu-id="20583-376">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="20583-377">W układzie tytuł jest odczytywany ze słownika ViewData:</span><span class="sxs-lookup"><span data-stu-id="20583-377">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="20583-378">TempData</span><span class="sxs-lookup"><span data-stu-id="20583-378">TempData</span></span>

<span data-ttu-id="20583-379">ASP.NET Core uwidacznia <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>.</span><span class="sxs-lookup"><span data-stu-id="20583-379">ASP.NET Core exposes the <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>.</span></span> <span data-ttu-id="20583-380">Ta właściwość przechowuje dane, dopóki nie zostanie odczytana.</span><span class="sxs-lookup"><span data-stu-id="20583-380">This property stores data until it's read.</span></span> <span data-ttu-id="20583-381">Metody <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> i<xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> mogą służyć do badania danych bez usuwania.</span><span class="sxs-lookup"><span data-stu-id="20583-381">The <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> and <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="20583-382">`TempData`jest przydatne w przypadku przekierowania, gdy dane są potrzebne dla więcej niż jednego żądania.</span><span class="sxs-lookup"><span data-stu-id="20583-382">`TempData` is useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="20583-383">Poniższy kod ustawia wartość `Message` użycia: `TempData`</span><span class="sxs-lookup"><span data-stu-id="20583-383">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="20583-384">W poniższym znaczniku w pliku *Pages/Customers/index. cshtml* jest wyświetlana wartość `Message` using `TempData`.</span><span class="sxs-lookup"><span data-stu-id="20583-384">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="20583-385">Model strony *Pages/Customers/index. cshtml. cs* stosuje `[TempData]` atrybut do `Message` właściwości.</span><span class="sxs-lookup"><span data-stu-id="20583-385">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="20583-386">Aby uzyskać więcej informacji, zobacz [TempData](xref:fundamentals/app-state#tempdata) .</span><span class="sxs-lookup"><span data-stu-id="20583-386">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="20583-387">Wiele programów obsługi na stronie</span><span class="sxs-lookup"><span data-stu-id="20583-387">Multiple handlers per page</span></span>

<span data-ttu-id="20583-388">Poniższa Strona generuje znaczniki dla dwóch programów obsługi przy użyciu `asp-page-handler` pomocnika tagów:</span><span class="sxs-lookup"><span data-stu-id="20583-388">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<span data-ttu-id="20583-389">Formularz w poprzednim przykładzie ma dwa przyciski przesyłania, z których `FormActionTagHelper` każdy używa do przesłania do innego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="20583-389">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="20583-390">Ten `asp-page-handler` atrybut jest towarzyszący do `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="20583-390">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="20583-391">`asp-page-handler`generuje adresy URL, które przesyłają do każdej metody obsługi zdefiniowanej przez stronę.</span><span class="sxs-lookup"><span data-stu-id="20583-391">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="20583-392">`asp-page`nie została określona, ponieważ próbka jest łączona z bieżącą stroną.</span><span class="sxs-lookup"><span data-stu-id="20583-392">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="20583-393">Model strony:</span><span class="sxs-lookup"><span data-stu-id="20583-393">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="20583-394">Poprzedni kod używa *nazwanych metod obsługi*.</span><span class="sxs-lookup"><span data-stu-id="20583-394">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="20583-395">Nazwane metody obsługi są tworzone przez pobranie tekstu w nazwie po `On<HTTP Verb>` i przed `Async` (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="20583-395">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="20583-396">W poprzednim przykładzie metody strony są onpost**JoinList**Async i Onpost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="20583-396">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="20583-397">Po usunięciu funkcji onpost i *Async* nazwy programów obsługi `JoinList` są `JoinListUC`i.</span><span class="sxs-lookup"><span data-stu-id="20583-397">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="20583-398">Korzystając z powyższego kodu, ścieżka URL, która jest `OnPostJoinListAsync` przesyłana do usługi, to `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="20583-398">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="20583-399">Ścieżka URL, która jest przesyłana `OnPostJoinListUCAsync` do `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`programu, to.</span><span class="sxs-lookup"><span data-stu-id="20583-399">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="20583-400">Trasy niestandardowe</span><span class="sxs-lookup"><span data-stu-id="20583-400">Custom routes</span></span>

<span data-ttu-id="20583-401">Użyj dyrektywy `@page` , aby:</span><span class="sxs-lookup"><span data-stu-id="20583-401">Use the `@page` directive to:</span></span>

* <span data-ttu-id="20583-402">Określ trasę niestandardową dla strony.</span><span class="sxs-lookup"><span data-stu-id="20583-402">Specify a custom route to a page.</span></span> <span data-ttu-id="20583-403">Na przykład trasy do strony informacje można ustawić na `/Some/Other/Path` wartość z. `@page "/Some/Other/Path"`</span><span class="sxs-lookup"><span data-stu-id="20583-403">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="20583-404">Dołącz segmenty do domyślnej trasy strony.</span><span class="sxs-lookup"><span data-stu-id="20583-404">Append segments to a page's default route.</span></span> <span data-ttu-id="20583-405">Na przykład segment "Item" można dodać do domyślnej trasy strony przy użyciu `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="20583-405">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="20583-406">Dołącz parametry do domyślnej trasy strony.</span><span class="sxs-lookup"><span data-stu-id="20583-406">Append parameters to a page's default route.</span></span> <span data-ttu-id="20583-407">Na przykład parametr ID, `id`, może być wymagany dla strony z. `@page "{id}"`</span><span class="sxs-lookup"><span data-stu-id="20583-407">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="20583-408">Ścieżka względna elementu głównego wypisana przez tyldę`~`() na początku ścieżki jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="20583-408">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="20583-409">Na przykład `@page "~/Some/Other/Path"` jest taka sama jak `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="20583-409">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="20583-410">Można zmienić ciąg `?handler=JoinList` zapytania w adresie URL na segment `/JoinList` trasy, określając szablon `@page "{handler?}"`trasy.</span><span class="sxs-lookup"><span data-stu-id="20583-410">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="20583-411">Jeśli nie podoba Ci się ciąg `?handler=JoinList` zapytania w adresie URL, możesz zmienić trasy, aby umieścić nazwę programu obsługi w części adresu URL.</span><span class="sxs-lookup"><span data-stu-id="20583-411">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="20583-412">Możesz dostosować trasę, dodając szablon trasy ujęty w podwójne cudzysłowy po `@page` dyrektywie.</span><span class="sxs-lookup"><span data-stu-id="20583-412">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="20583-413">Korzystając z powyższego kodu, ścieżka URL, która jest `OnPostJoinListAsync` przesyłana do usługi, to `https://localhost:5001/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="20583-413">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="20583-414">Ścieżka URL, która jest przesyłana `OnPostJoinListUCAsync` do `https://localhost:5001/Customers/CreateFATH/JoinListUC`programu, to.</span><span class="sxs-lookup"><span data-stu-id="20583-414">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="20583-415">Poniżej przedstawiono `handler` , że parametr trasy jest opcjonalny. `?`</span><span class="sxs-lookup"><span data-stu-id="20583-415">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="advanced-configuration-and-settings"></a><span data-ttu-id="20583-416">Zaawansowana konfiguracja i ustawienia</span><span class="sxs-lookup"><span data-stu-id="20583-416">Advanced configuration and settings</span></span>

<span data-ttu-id="20583-417">Konfiguracja i ustawienia w poniższych sekcjach nie są wymagane przez większość aplikacji.</span><span class="sxs-lookup"><span data-stu-id="20583-417">The configuration and settings in following sections is not required by most apps.</span></span>

<span data-ttu-id="20583-418">Aby skonfigurować opcje zaawansowane, użyj metody <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>rozszerzenia:</span><span class="sxs-lookup"><span data-stu-id="20583-418">To configure advanced options, use the extension method <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupRPoptions.cs?name=snippet)]

<span data-ttu-id="20583-419">Użyj, <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> aby ustawić katalog główny dla stron lub dodać konwencje modelu aplikacji dla stron.</span><span class="sxs-lookup"><span data-stu-id="20583-419">Use the <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="20583-420">Aby uzyskać więcej informacji na temat Konwencji, zobacz [Razor Pages Konwencji autoryzacji](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="20583-420">For more information on conventions, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization).</span></span>

<span data-ttu-id="20583-421">Aby wstępnie skompilować widoki, zobacz [kompilacja widoku Razor](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="20583-421">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="20583-422">Określ, że Razor Pages znajdują się w katalogu głównym zawartości</span><span class="sxs-lookup"><span data-stu-id="20583-422">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="20583-423">Domyślnie Razor Pages są umieszczane w katalogu */Pages* .</span><span class="sxs-lookup"><span data-stu-id="20583-423">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="20583-424">Dodaj <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> , aby określić, że Twoje Razor Pages znajdują się w<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>katalogu głównym zawartości aplikacji:</span><span class="sxs-lookup"><span data-stu-id="20583-424">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> to specify that your Razor Pages are at the content root (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) of the app:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesAtContentRoot.cs?name=snippet)]

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="20583-425">Określ, że Razor Pages znajdują się w niestandardowym katalogu głównym</span><span class="sxs-lookup"><span data-stu-id="20583-425">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="20583-426">Dodaj <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> , aby określić, że Razor Pages znajdują się w niestandardowym katalogu głównym w aplikacji (podaj ścieżkę względną):</span><span class="sxs-lookup"><span data-stu-id="20583-426">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> to specify that Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesRoot.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="20583-427">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="20583-427">Additional resources</span></span>

* <span data-ttu-id="20583-428">Zobacz Rozpoczynanie [pracy z usługą Razor Pages](xref:tutorials/razor-pages/razor-pages-start), która kompiluje się w tym wprowadzeniu.</span><span class="sxs-lookup"><span data-stu-id="20583-428">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>
* <span data-ttu-id="20583-429">[Pobierz lub Wyświetl przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/3.0sample).</span><span class="sxs-lookup"><span data-stu-id="20583-429">[Download or view sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/3.0sample).</span></span>
* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="20583-430">[Rick Anderson](https://twitter.com/RickAndMSFT) i [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="20583-430">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="20583-431">Razor Pages jest nowym aspektem ASP.NET Core MVC, który sprawia, że kodowanie scenariuszy ukierunkowanych na strony jest łatwiejsze i bardziej produktywne.</span><span class="sxs-lookup"><span data-stu-id="20583-431">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="20583-432">Jeśli szukasz samouczka korzystającego z podejścia Model-View-Controller, zobacz Wprowadzenie do [ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="20583-432">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="20583-433">Ten dokument zawiera wprowadzenie do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="20583-433">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="20583-434">Nie jest to samouczek krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="20583-434">It's not a step by step tutorial.</span></span> <span data-ttu-id="20583-435">Jeśli okaże się, że niektóre sekcje są zbyt zaawansowane, zobacz [wprowadzenie do Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="20583-435">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="20583-436">Aby zapoznać się z omówieniem ASP.NET Core, zobacz [wprowadzenie do ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="20583-436">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="20583-437">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="20583-437">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20583-438">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20583-438">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="20583-439">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="20583-439">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="20583-440">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="20583-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="20583-441">Tworzenie projektu Razor Pages</span><span class="sxs-lookup"><span data-stu-id="20583-441">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="20583-442">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20583-442">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="20583-443">Aby uzyskać szczegółowe instrukcje dotyczące sposobu tworzenia projektu Razor Pages, zobacz Wprowadzenie do [Razor Pages](xref:tutorials/razor-pages/razor-pages-start) .</span><span class="sxs-lookup"><span data-stu-id="20583-443">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="20583-444">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="20583-444">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="20583-445">Uruchom `dotnet new webapp` polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="20583-445">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="20583-446">Otwórz wygenerowany plik *csproj* z Visual Studio dla komputerów Mac.</span><span class="sxs-lookup"><span data-stu-id="20583-446">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="20583-447">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="20583-447">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="20583-448">Uruchom `dotnet new webapp` polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="20583-448">Run `dotnet new webapp` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="20583-449">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="20583-449">Razor Pages</span></span>

<span data-ttu-id="20583-450">Razor Pages jest włączona w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="20583-450">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="20583-451">Weź pod uwagę podstawową stronę:<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="20583-451">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="20583-452">Poprzedni kod wygląda podobnie jak [plik widoku Razor](xref:tutorials/first-mvc-app/adding-view) używany w aplikacji ASP.NET Core z kontrolerami i widokami.</span><span class="sxs-lookup"><span data-stu-id="20583-452">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="20583-453">Co sprawia, `@page` że jest to dyrektywa.</span><span class="sxs-lookup"><span data-stu-id="20583-453">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="20583-454">`@page`sprawia, że plik jest akcją MVC, co oznacza, że obsługuje żądania bezpośrednio, bez przechodzenia przez kontroler.</span><span class="sxs-lookup"><span data-stu-id="20583-454">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="20583-455">`@page`musi być pierwszą dyrektywą Razor na stronie.</span><span class="sxs-lookup"><span data-stu-id="20583-455">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="20583-456">`@page`wpływa na zachowanie innych konstrukcji Razor.</span><span class="sxs-lookup"><span data-stu-id="20583-456">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="20583-457">Podobna Strona, za pomocą `PageModel` klasy, jest pokazana w następujących dwóch plikach.</span><span class="sxs-lookup"><span data-stu-id="20583-457">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="20583-458">Plik *Pages/index2. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="20583-458">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="20583-459">Model strony *stron/index2. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="20583-459">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="20583-460">Zgodnie z Konwencją `PageModel` plik klasy ma taką samą nazwę jak plik stronicowania Razor z dołączonym rozszerzeniem *. cs* .</span><span class="sxs-lookup"><span data-stu-id="20583-460">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="20583-461">Na przykład Poprzednia strona Razor to *Pages/index2. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="20583-461">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="20583-462">Plik zawierający `PageModel` klasę ma nazwę Pages */index2. cshtml. cs*.</span><span class="sxs-lookup"><span data-stu-id="20583-462">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="20583-463">Skojarzenia ścieżek adresów URL ze stronami są określane przez lokalizację strony w systemie plików.</span><span class="sxs-lookup"><span data-stu-id="20583-463">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="20583-464">W poniższej tabeli przedstawiono ścieżkę strony Razor i pasujący adres URL:</span><span class="sxs-lookup"><span data-stu-id="20583-464">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="20583-465">Nazwa i ścieżka pliku</span><span class="sxs-lookup"><span data-stu-id="20583-465">File name and path</span></span>               | <span data-ttu-id="20583-466">pasujący adres URL</span><span class="sxs-lookup"><span data-stu-id="20583-466">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="20583-467">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="20583-467">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="20583-468">`/` lub `/Index`</span><span class="sxs-lookup"><span data-stu-id="20583-468">`/` or `/Index`</span></span> |
| <span data-ttu-id="20583-469">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="20583-469">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="20583-470">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="20583-470">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="20583-471">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="20583-471">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="20583-472">`/Store` lub `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="20583-472">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="20583-473">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="20583-473">Notes:</span></span>

* <span data-ttu-id="20583-474">Środowisko wykonawcze domyślnie wyszukuje pliki Razor Pages w folderze Pages.</span><span class="sxs-lookup"><span data-stu-id="20583-474">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="20583-475">`Index`jest stroną domyślną, gdy adres URL nie zawiera strony.</span><span class="sxs-lookup"><span data-stu-id="20583-475">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="20583-476">Napisz podstawowy formularz</span><span class="sxs-lookup"><span data-stu-id="20583-476">Write a basic form</span></span>

<span data-ttu-id="20583-477">Razor Pages zaprojektowano tak, aby wspólne wzorce używane z przeglądarkami sieci Web były łatwe do wdrożenia podczas kompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="20583-477">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="20583-478">[Powiązania modelu](xref:mvc/models/model-binding), [pomocników tagów](xref:mvc/views/tag-helpers/intro)i pomocników HTML same *działają* z właściwościami zdefiniowanymi w klasie strony Razor.</span><span class="sxs-lookup"><span data-stu-id="20583-478">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="20583-479">Weź pod uwagę stronę implementującą podstawowy formularz "contact us" (kontakt `Contact` z nami) dla modelu:</span><span class="sxs-lookup"><span data-stu-id="20583-479">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="20583-480">Przykłady w tym dokumencie `DbContext` są inicjowane w pliku [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) .</span><span class="sxs-lookup"><span data-stu-id="20583-480">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="20583-481">Model danych:</span><span class="sxs-lookup"><span data-stu-id="20583-481">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="20583-482">Kontekst bazy danych:</span><span class="sxs-lookup"><span data-stu-id="20583-482">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="20583-483">Plik widoku *Pages/Create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="20583-483">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="20583-484">Model strony *Pages/Create. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="20583-484">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="20583-485">Zgodnie z Konwencją `PageModel` Klasa jest wywoływana `<PageName>Model` i znajduje się w tej samej przestrzeni nazw co strona.</span><span class="sxs-lookup"><span data-stu-id="20583-485">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="20583-486">`PageModel` Klasa umożliwia rozdzielenie logiki strony od jej prezentacji.</span><span class="sxs-lookup"><span data-stu-id="20583-486">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="20583-487">Definiuje procedury obsługi stron dla żądań wysyłanych do strony oraz dane używane do renderowania strony.</span><span class="sxs-lookup"><span data-stu-id="20583-487">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="20583-488">Ta separacja umożliwia:</span><span class="sxs-lookup"><span data-stu-id="20583-488">This separation allows:</span></span>

* <span data-ttu-id="20583-489">Zarządzanie zależnościami stron przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="20583-489">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="20583-490">[Testowanie jednostkowe](xref:test/razor-pages-tests) stron.</span><span class="sxs-lookup"><span data-stu-id="20583-490">[Unit testing](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="20583-491">Strona ma `OnPostAsync` *metodę obsługi*, która jest uruchamiana na `POST` żądaniach (gdy użytkownik księguje formularz).</span><span class="sxs-lookup"><span data-stu-id="20583-491">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="20583-492">Można dodać metody obsługi dla dowolnego zlecenia HTTP.</span><span class="sxs-lookup"><span data-stu-id="20583-492">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="20583-493">Najczęstszymi obsłudze są:</span><span class="sxs-lookup"><span data-stu-id="20583-493">The most common handlers are:</span></span>

* <span data-ttu-id="20583-494">`OnGet`do zainicjowania stanu wymaganego dla strony.</span><span class="sxs-lookup"><span data-stu-id="20583-494">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="20583-495">Przykład [OnGet](#OnGet) .</span><span class="sxs-lookup"><span data-stu-id="20583-495">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="20583-496">`OnPost`do obsługi przesłanych formularzy.</span><span class="sxs-lookup"><span data-stu-id="20583-496">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="20583-497">Sufiks `Async` nazewnictwa jest opcjonalny, ale jest często używany przez Konwencję dla funkcji asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="20583-497">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="20583-498">Poprzedni kod jest typowy dla Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="20583-498">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="20583-499">Jeśli znasz już aplikacje ASP.NET przy użyciu kontrolerów i widoków:</span><span class="sxs-lookup"><span data-stu-id="20583-499">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="20583-500">`OnPostAsync` Kod w poprzednim przykładzie wygląda podobnie do typowego kodu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="20583-500">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="20583-501">Większość elementów podstawowych MVC, takich jak [powiązanie modelu](xref:mvc/models/model-binding), [Walidacja](xref:mvc/models/validation), [Walidacja](xref:mvc/models/validation)i wyniki akcji, jest udostępniana.</span><span class="sxs-lookup"><span data-stu-id="20583-501">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), [Validation](xref:mvc/models/validation),  and action results are shared.</span></span>

<span data-ttu-id="20583-502">Poprzednia `OnPostAsync` Metoda:</span><span class="sxs-lookup"><span data-stu-id="20583-502">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="20583-503">Podstawowy przepływ `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="20583-503">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="20583-504">Sprawdź, czy występują błędy walidacji.</span><span class="sxs-lookup"><span data-stu-id="20583-504">Check for validation errors.</span></span>

* <span data-ttu-id="20583-505">Jeśli nie ma żadnych błędów, Zapisz dane i Przekieruj.</span><span class="sxs-lookup"><span data-stu-id="20583-505">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="20583-506">W przypadku wystąpienia błędów ponownie Wyświetl stronę z komunikatami walidacji.</span><span class="sxs-lookup"><span data-stu-id="20583-506">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="20583-507">Walidacja po stronie klienta jest taka sama jak w przypadku tradycyjnych ASP.NET Core aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="20583-507">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="20583-508">W wielu przypadkach błędy sprawdzania poprawności zostaną wykryte na kliencie i nigdy nie przesłano ich do serwera.</span><span class="sxs-lookup"><span data-stu-id="20583-508">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="20583-509">Po pomyślnym `OnPostAsync` wprowadzeniu danych metoda obsługi `RedirectToPage` wywołuje metodę pomocnika zwracającą wystąpienie elementu `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="20583-509">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="20583-510">`RedirectToPage`to nowy wynik akcji, podobny do `RedirectToAction` lub `RedirectToRoute`, ale dostosowany do stron.</span><span class="sxs-lookup"><span data-stu-id="20583-510">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="20583-511">W powyższym przykładzie przekierowuje do strony indeksu głównego (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="20583-511">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="20583-512">`RedirectToPage`szczegółowo znajduje się w sekcji [generowanie adresów URL dla stron](#url_gen) .</span><span class="sxs-lookup"><span data-stu-id="20583-512">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="20583-513">Gdy przesłany formularz ma błędy walidacji (które są przekazywane do serwera),`OnPostAsync` metoda obsługi `Page` wywołuje metodę pomocnika.</span><span class="sxs-lookup"><span data-stu-id="20583-513">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="20583-514">`Page`Zwraca wystąpienie elementu `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="20583-514">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="20583-515">Zwracanie `Page` jest podobne do sposobu, w jaki akcje `View`w kontrolerach zwracają.</span><span class="sxs-lookup"><span data-stu-id="20583-515">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="20583-516">`PageResult`jest domyślnym typem zwracanym dla metody obsługi.</span><span class="sxs-lookup"><span data-stu-id="20583-516">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="20583-517">Metoda obsługi, która zwraca `void` renderowanie strony.</span><span class="sxs-lookup"><span data-stu-id="20583-517">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="20583-518">`Customer` Właściwość używa`[BindProperty]` atrybutu, aby zrezygnować z powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="20583-518">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="20583-519">Razor Pages domyślnie Powiąż właściwości tylko z`GET` niezleceniami.</span><span class="sxs-lookup"><span data-stu-id="20583-519">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="20583-520">Powiązanie z właściwościami może zmniejszyć ilość kodu, który trzeba napisać.</span><span class="sxs-lookup"><span data-stu-id="20583-520">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="20583-521">Powiązanie zmniejsza kod, używając tej samej właściwości do renderowania pól formularza (`<input asp-for="Customer.Name">`) i akceptuję dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="20583-521">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="20583-522">Strona główna (*index. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="20583-522">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="20583-523">Skojarzona `PageModel` Klasa (*index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="20583-523">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="20583-524">Plik *index. cshtml* zawiera następujące znaczniki, aby utworzyć łącze do edycji dla każdego kontaktu:</span><span class="sxs-lookup"><span data-stu-id="20583-524">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="20583-525">[Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) użył atrybutuwceluwygenerowanialinkudostronyedycji.`asp-route-{value}` `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>`</span><span class="sxs-lookup"><span data-stu-id="20583-525">The `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="20583-526">Link zawiera dane trasy z IDENTYFIKATORem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="20583-526">The link contains route data with the contact ID.</span></span> <span data-ttu-id="20583-527">Na przykład `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="20583-527">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="20583-528">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="20583-528">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="20583-529">Pomocnicy tagów są włączani przez`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="20583-529">Tag Helpers are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="20583-530">Plik *Pages/Edit. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="20583-530">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="20583-531">Pierwszy wiersz zawiera `@page "{id:int}"` dyrektywę.</span><span class="sxs-lookup"><span data-stu-id="20583-531">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="20583-532">Ograniczenie`"{id:int}"` routingu instruuje stronę, aby akceptowała żądania do strony `int` zawierającej dane trasy.</span><span class="sxs-lookup"><span data-stu-id="20583-532">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="20583-533">Jeśli żądanie do strony nie zawiera danych trasy, które można przekonwertować na obiekt `int`, środowisko uruchomieniowe zwróci błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="20583-533">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="20583-534">Aby identyfikator był opcjonalny, Dołącz `?` do ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="20583-534">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="20583-535">Plik *stron/Edytuj. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="20583-535">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="20583-536">Plik *index. cshtml* zawiera również znaczniki umożliwiające utworzenie przycisku usuwania dla każdego kontaktu z klientem:</span><span class="sxs-lookup"><span data-stu-id="20583-536">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="20583-537">Gdy przycisk Usuń jest renderowany w języku HTML, jego `formaction` parametry obejmują:</span><span class="sxs-lookup"><span data-stu-id="20583-537">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="20583-538">Identyfikator osoby kontaktowej klienta określony przez `asp-route-id` atrybut.</span><span class="sxs-lookup"><span data-stu-id="20583-538">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="20583-539">`handler` Określony`asp-page-handler` przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="20583-539">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="20583-540">Oto przykład renderowanego przycisku usuwania z IDENTYFIKATORem `1`kontaktu klienta:</span><span class="sxs-lookup"><span data-stu-id="20583-540">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="20583-541">Po wybraniu przycisku do serwera zostanie wysłane `POST` żądanie formularza.</span><span class="sxs-lookup"><span data-stu-id="20583-541">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="20583-542">Według Konwencji, nazwa metody obsługi jest wybierana na podstawie wartości `handler` parametru zgodnie z schematem. `OnPost[handler]Async`</span><span class="sxs-lookup"><span data-stu-id="20583-542">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="20583-543">`handler` Ponieważ jest `delete` w`OnPostDeleteAsync` tym przykładzie, metoda obsługi jest używana do przetwarzania `POST` żądania.</span><span class="sxs-lookup"><span data-stu-id="20583-543">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="20583-544">Jeśli jest ustawiona na inną wartość, na przykład `remove`, jest wybierana metoda obsługi o nazwie `OnPostRemoveAsync`. `asp-page-handler`</span><span class="sxs-lookup"><span data-stu-id="20583-544">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span> <span data-ttu-id="20583-545">Poniższy kod ilustruje `OnPostDeleteAsync` procedurę obsługi:</span><span class="sxs-lookup"><span data-stu-id="20583-545">The following code shows the `OnPostDeleteAsync` handler:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="20583-546">`OnPostDeleteAsync` Metody:</span><span class="sxs-lookup"><span data-stu-id="20583-546">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="20583-547">`id` Akceptuje z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="20583-547">Accepts the `id` from the query string.</span></span> <span data-ttu-id="20583-548">Jeśli na stronie *index. cshtml* znajduje się ograniczenie `"{id:int?}"`routingu, `id` będą pochodzić z danych tras.</span><span class="sxs-lookup"><span data-stu-id="20583-548">If the *Index.cshtml* page directive contained routing constraint `"{id:int?}"`, `id` would come from route data.</span></span> <span data-ttu-id="20583-549">Dane trasy dla programu `id` są określone w identyfikatorze URI, `https://localhost:5001/Customers/2`takich jak.</span><span class="sxs-lookup"><span data-stu-id="20583-549">The route data for `id` is specified in the URI such as `https://localhost:5001/Customers/2`.</span></span>
* <span data-ttu-id="20583-550">Wysyła zapytanie do bazy danych w celu skontaktowania się z klientem za pomocą `FindAsync`programu.</span><span class="sxs-lookup"><span data-stu-id="20583-550">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="20583-551">Jeśli kontakt z klientem zostanie znaleziony, zostanie on usunięty z listy kontaktów klientów.</span><span class="sxs-lookup"><span data-stu-id="20583-551">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="20583-552">Baza danych jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="20583-552">The database is updated.</span></span>
* <span data-ttu-id="20583-553">Wywołania `RedirectToPage` do przekierowania na stronę indeksu głównego (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="20583-553">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="20583-554">Oznacz właściwości strony jako wymagane</span><span class="sxs-lookup"><span data-stu-id="20583-554">Mark page properties as required</span></span>

<span data-ttu-id="20583-555">Właściwości na serwerze `PageModel` mogą być dekoracyjne z [wymaganym](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) atrybutem:</span><span class="sxs-lookup"><span data-stu-id="20583-555">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="20583-556">Aby uzyskać więcej informacji, zobacz [Walidacja modelu](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="20583-556">For more information, see [Model validation](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="20583-557">Obsługa żądań głównych przy użyciu rezerwy procedury obsługi OnGet</span><span class="sxs-lookup"><span data-stu-id="20583-557">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="20583-558">`HEAD`żądania umożliwiają pobranie nagłówków dla określonego zasobu.</span><span class="sxs-lookup"><span data-stu-id="20583-558">`HEAD` requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="20583-559">W przeciwieństwie `GET` do `HEAD` żądań, żądania nie zwracają treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="20583-559">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="20583-560">Zwykle program obsługi jest tworzony i wywoływany dla `HEAD` żądań: `OnHead`</span><span class="sxs-lookup"><span data-stu-id="20583-560">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="20583-561">W ASP.NET Core 2,1 lub nowszej Razor Pages powracać do wywoływania procedury `OnGet` obsługi, jeśli `OnHead` nie zdefiniowano procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="20583-561">In ASP.NET Core 2.1 or later, Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span> <span data-ttu-id="20583-562">To zachowanie jest włączane przez wywołanie [SetCompatibilityVersion](xref:mvc/compatibility-version) w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="20583-562">This behavior is enabled by the call to [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="20583-563">Szablony domyślne generują `SetCompatibilityVersion` wywołanie w ASP.NET Core 2,1 i 2,2.</span><span class="sxs-lookup"><span data-stu-id="20583-563">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span> <span data-ttu-id="20583-564">`SetCompatibilityVersion`efektywnie ustawia opcję `AllowMappingHeadRequestsToGetHandler` Razor Pages na `true`.</span><span class="sxs-lookup"><span data-stu-id="20583-564">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="20583-565">Zamiast korzystać z wszystkich zachowań w programie `SetCompatibilityVersion`, można jawnie zrezygnować z *określonych* zachowań.</span><span class="sxs-lookup"><span data-stu-id="20583-565">Rather than opting in to all behaviors with `SetCompatibilityVersion`, you can explicitly opt in to *specific* behaviors.</span></span> <span data-ttu-id="20583-566">Poniższy kod pozwala na umożliwienie `HEAD` mapowania żądań `OnGet` do programu obsługi:</span><span class="sxs-lookup"><span data-stu-id="20583-566">The following code opts in to allowing `HEAD` requests to be mapped to the `OnGet` handler:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="20583-567">XSRF/CSRF i Razor Pages</span><span class="sxs-lookup"><span data-stu-id="20583-567">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="20583-568">Nie trzeba pisać kodu do [weryfikacji](xref:security/anti-request-forgery)przed fałszerstwem.</span><span class="sxs-lookup"><span data-stu-id="20583-568">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="20583-569">Generowanie i sprawdzanie poprawności tokenów antysfałszowanych są automatycznie dołączane do Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="20583-569">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="20583-570">Używanie układów, częściowych, szablonów i pomocników tagów z Razor Pages</span><span class="sxs-lookup"><span data-stu-id="20583-570">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="20583-571">Strony współpracują ze wszystkimi funkcjami aparatu widoku Razor.</span><span class="sxs-lookup"><span data-stu-id="20583-571">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="20583-572">Układy, części, szablony, pomocników tagów, *_ViewStart. cshtml*, *_ViewImports. cshtml* działają w taki sam sposób, jak w przypadku konwencjonalnych widoków Razor.</span><span class="sxs-lookup"><span data-stu-id="20583-572">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="20583-573">Zanotujmy Tę stronę, korzystając z zalet niektórych z tych funkcji.</span><span class="sxs-lookup"><span data-stu-id="20583-573">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="20583-574">Dodaj [stronę układu](xref:mvc/views/layout) do *stron/Shared/_Layout. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="20583-574">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="20583-575">[Układ](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="20583-575">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="20583-576">Steruje układem każdej strony (chyba że strona nie jest częścią układu).</span><span class="sxs-lookup"><span data-stu-id="20583-576">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="20583-577">Importuje struktury HTML, takie jak JavaScript i stylesheets.</span><span class="sxs-lookup"><span data-stu-id="20583-577">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="20583-578">Aby uzyskać więcej informacji, zobacz [stronę układu](xref:mvc/views/layout) .</span><span class="sxs-lookup"><span data-stu-id="20583-578">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="20583-579">Właściwość [układu](xref:mvc/views/layout#specifying-a-layout) jest ustawiana na *stronie/_ViewStart. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="20583-579">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="20583-580">Układ znajduje się w *stronie/* w folderze udostępnionym.</span><span class="sxs-lookup"><span data-stu-id="20583-580">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="20583-581">Strony szukają innych widoków (układów, szablonów, częściowych) hierarchicznie, rozpoczynając w tym samym folderze, w którym znajduje się bieżąca strona.</span><span class="sxs-lookup"><span data-stu-id="20583-581">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="20583-582">Układ na *stronach/* w folderze udostępnionym może być używany z dowolnej strony Razor w folderze *Pages* .</span><span class="sxs-lookup"><span data-stu-id="20583-582">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="20583-583">Plik układu powinien przejść do *stron/folderu udostępnionego* .</span><span class="sxs-lookup"><span data-stu-id="20583-583">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="20583-584">Zalecamy **umieszczenie pliku** układu w widokach */* folderze udostępnionym.</span><span class="sxs-lookup"><span data-stu-id="20583-584">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="20583-585">*Widoki/udostępnione* są wzorcem widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="20583-585">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="20583-586">Razor Pages są przeznaczone do korzystania z hierarchii folderów, a nie Konwencji ścieżek.</span><span class="sxs-lookup"><span data-stu-id="20583-586">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="20583-587">Widok wyszukiwania na stronie Razor zawiera folder *strony* .</span><span class="sxs-lookup"><span data-stu-id="20583-587">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="20583-588">Układy, szablony i częściowe, które są używane z kontrolerami MVC i konwencjonalnymi widokami Razor, *działają tylko*.</span><span class="sxs-lookup"><span data-stu-id="20583-588">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="20583-589">Dodaj plik *Pages/_ViewImports. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="20583-589">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="20583-590">`@namespace`wyjaśniono w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="20583-590">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="20583-591">Dyrektywa `@addTagHelper` znajduje się w [wbudowanych pomocników tagów](xref:mvc/views/tag-helpers/builtin-th/Index) do wszystkich stron w folderze *strony* .</span><span class="sxs-lookup"><span data-stu-id="20583-591">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="20583-592">`@namespace` Gdy dyrektywa jest używana jawnie na stronie:</span><span class="sxs-lookup"><span data-stu-id="20583-592">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="20583-593">Dyrektywa ustawia przestrzeń nazw dla strony.</span><span class="sxs-lookup"><span data-stu-id="20583-593">The directive sets the namespace for the page.</span></span> <span data-ttu-id="20583-594">`@model` Dyrektywa nie musi zawierać przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="20583-594">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="20583-595">Gdy dyrektywa jest zawarta w *_ViewImports. cshtml*, określona przestrzeń nazw udostępnia prefiks dla wygenerowanej przestrzeni nazw na `@namespace` stronie, która importuje dyrektywę. `@namespace`</span><span class="sxs-lookup"><span data-stu-id="20583-595">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="20583-596">Pozostała część wygenerowanej przestrzeni nazw (część sufiksu) jest ścieżką względną oddzieloną kropką między folderem zawierającym *_ViewImports. cshtml* i folderem zawierającym stronę.</span><span class="sxs-lookup"><span data-stu-id="20583-596">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="20583-597">Na przykład `PageModel` Klasa Pages */Customers/Edit. cshtml. cs* jawnie ustawia przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="20583-597">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="20583-598">Plik *Pages/_ViewImports. cshtml* ustawia następującą przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="20583-598">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="20583-599">Wygenerowana przestrzeń nazw dla strony */Customers/Edit. cshtml* Razor jest taka sama jak `PageModel` Klasa.</span><span class="sxs-lookup"><span data-stu-id="20583-599">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="20583-600">`@namespace`*działa również z konwencjonalnymi widokami Razor.*</span><span class="sxs-lookup"><span data-stu-id="20583-600">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="20583-601">Oryginalne *strony/Utwórz plik widoku. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="20583-601">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="20583-602">Zaktualizowane *strony/Utwórz plik widoku. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="20583-602">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="20583-603">[Razor Pages początkowy projekt](#rpvs17) zawiera *stronę/_ValidationScriptsPartial. cshtml*, który łączy weryfikację po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="20583-603">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="20583-604">Aby uzyskać więcej informacji o widokach częściowych, zobacz <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="20583-604">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="20583-605">Generowanie adresu URL dla stron</span><span class="sxs-lookup"><span data-stu-id="20583-605">URL generation for Pages</span></span>

<span data-ttu-id="20583-606">Strona, pokazana wcześniej, używa `RedirectToPage`: `Create`</span><span class="sxs-lookup"><span data-stu-id="20583-606">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="20583-607">Aplikacja ma następującą strukturę plików/folderów:</span><span class="sxs-lookup"><span data-stu-id="20583-607">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="20583-608">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="20583-608">*/Pages*</span></span>

  * <span data-ttu-id="20583-609">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="20583-609">*Index.cshtml*</span></span>
  * <span data-ttu-id="20583-610">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="20583-610">*/Customers*</span></span>

    * <span data-ttu-id="20583-611">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="20583-611">*Create.cshtml*</span></span>
    * <span data-ttu-id="20583-612">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="20583-612">*Edit.cshtml*</span></span>
    * <span data-ttu-id="20583-613">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="20583-613">*Index.cshtml*</span></span>

<span data-ttu-id="20583-614">Strony */Customers/Create. cshtml* i Pages/ *Customers/Edit. cshtml* przekierują do *stron/index. cshtml* po powodzeniu.</span><span class="sxs-lookup"><span data-stu-id="20583-614">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="20583-615">Ciąg `/Index` jest częścią identyfikatora URI, aby uzyskać dostęp do poprzedniej strony.</span><span class="sxs-lookup"><span data-stu-id="20583-615">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="20583-616">Ten ciąg `/Index` może służyć do generowania identyfikatorów URI na stronie *stron/index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="20583-616">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="20583-617">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="20583-617">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="20583-618">Nazwa strony jest ścieżką do strony z folderu głównego */Pages* , włącznie z wiodącym `/` `/Index`(na przykład).</span><span class="sxs-lookup"><span data-stu-id="20583-618">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="20583-619">Powyższe przykłady generowania adresów URL oferują ulepszone opcje i możliwości funkcjonalne w porównaniu z zakodowana adresem URL.</span><span class="sxs-lookup"><span data-stu-id="20583-619">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="20583-620">Generowanie adresów URL używa [routingu](xref:mvc/controllers/routing) i może generować i kodować parametry zgodnie ze sposobem zdefiniowania trasy w ścieżce docelowej.</span><span class="sxs-lookup"><span data-stu-id="20583-620">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="20583-621">Generowanie adresów URL dla stron obsługuje nazwy względne.</span><span class="sxs-lookup"><span data-stu-id="20583-621">URL generation for pages supports relative names.</span></span> <span data-ttu-id="20583-622">W poniższej tabeli przedstawiono, która strona indeksu została wybrana z `RedirectToPage` różnymi parametrami *stron/Customers/Create. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="20583-622">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="20583-623">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="20583-623">RedirectToPage(x)</span></span>| <span data-ttu-id="20583-624">Stronic</span><span class="sxs-lookup"><span data-stu-id="20583-624">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="20583-625">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="20583-625">RedirectToPage("/Index")</span></span> | <span data-ttu-id="20583-626">*Strony/indeks*</span><span class="sxs-lookup"><span data-stu-id="20583-626">*Pages/Index*</span></span> |
| <span data-ttu-id="20583-627">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="20583-627">RedirectToPage("./Index");</span></span> | <span data-ttu-id="20583-628">*Strony/klienci/indeks*</span><span class="sxs-lookup"><span data-stu-id="20583-628">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="20583-629">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="20583-629">RedirectToPage("../Index")</span></span> | <span data-ttu-id="20583-630">*Strony/indeks*</span><span class="sxs-lookup"><span data-stu-id="20583-630">*Pages/Index*</span></span> |
| <span data-ttu-id="20583-631">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="20583-631">RedirectToPage("Index")</span></span>  | <span data-ttu-id="20583-632">*Strony/klienci/indeks*</span><span class="sxs-lookup"><span data-stu-id="20583-632">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="20583-633">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, i `RedirectToPage("../Index")` są *nazwami względnymi*.</span><span class="sxs-lookup"><span data-stu-id="20583-633">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="20583-634">Parametr jest połączony ze ścieżką bieżącej strony, aby obliczyć nazwę strony docelowej. `RedirectToPage`</span><span class="sxs-lookup"><span data-stu-id="20583-634">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="20583-635">Łączenie nazw względnych jest przydatne podczas kompilowania lokacji ze złożoną strukturą.</span><span class="sxs-lookup"><span data-stu-id="20583-635">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="20583-636">Jeśli używasz nazw względnych do łączenia między stronami w folderze, możesz zmienić nazwę tego folderu.</span><span class="sxs-lookup"><span data-stu-id="20583-636">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="20583-637">Wszystkie linki nadal działają (ponieważ nie zawierają nazwy folderu).</span><span class="sxs-lookup"><span data-stu-id="20583-637">All the links still work (because they didn't include the folder name).</span></span>

<span data-ttu-id="20583-638">Aby przekierować do strony w innym [obszarze](xref:mvc/controllers/areas), określ obszar:</span><span class="sxs-lookup"><span data-stu-id="20583-638">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="20583-639">Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="20583-639">For more information, see <xref:mvc/controllers/areas>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="20583-640">ViewData — atrybut</span><span class="sxs-lookup"><span data-stu-id="20583-640">ViewData attribute</span></span>

<span data-ttu-id="20583-641">Dane można przekazywać do strony z [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="20583-641">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="20583-642">Właściwości na kontrolerach lub modelach stron Razor, `[ViewData]` których wartości są przechowywane i ładowane z [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="20583-642">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="20583-643">W poniższym przykładzie `AboutModel` `Title` zawiera właściwość z `[ViewData]`właściwością.</span><span class="sxs-lookup"><span data-stu-id="20583-643">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="20583-644">`Title` Właściwość jest ustawiona na tytuł strony informacje:</span><span class="sxs-lookup"><span data-stu-id="20583-644">The `Title` property is set to the title of the About page:</span></span>

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

<span data-ttu-id="20583-645">Na stronie informacje uzyskaj dostęp do `Title` właściwości jako właściwości modelu:</span><span class="sxs-lookup"><span data-stu-id="20583-645">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="20583-646">W układzie tytuł jest odczytywany ze słownika ViewData:</span><span class="sxs-lookup"><span data-stu-id="20583-646">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="20583-647">TempData</span><span class="sxs-lookup"><span data-stu-id="20583-647">TempData</span></span>

<span data-ttu-id="20583-648">ASP.NET Core uwidacznia Właściwość [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) na [kontrolerze](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="20583-648">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="20583-649">Ta właściwość przechowuje dane, dopóki nie zostanie odczytana.</span><span class="sxs-lookup"><span data-stu-id="20583-649">This property stores data until it's read.</span></span> <span data-ttu-id="20583-650">Metody `Keep` i`Peek` mogą służyć do badania danych bez usuwania.</span><span class="sxs-lookup"><span data-stu-id="20583-650">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="20583-651">`TempData`jest przydatne w przypadku przekierowania, gdy dane są potrzebne dla więcej niż jednego żądania.</span><span class="sxs-lookup"><span data-stu-id="20583-651">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="20583-652">Poniższy kod ustawia wartość `Message` użycia: `TempData`</span><span class="sxs-lookup"><span data-stu-id="20583-652">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="20583-653">W poniższym znaczniku w pliku *Pages/Customers/index. cshtml* jest wyświetlana wartość `Message` using `TempData`.</span><span class="sxs-lookup"><span data-stu-id="20583-653">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="20583-654">Model strony *Pages/Customers/index. cshtml. cs* stosuje `[TempData]` atrybut do `Message` właściwości.</span><span class="sxs-lookup"><span data-stu-id="20583-654">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="20583-655">Aby uzyskać więcej informacji, zobacz [TempData](xref:fundamentals/app-state#tempdata) .</span><span class="sxs-lookup"><span data-stu-id="20583-655">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="20583-656">Wiele programów obsługi na stronie</span><span class="sxs-lookup"><span data-stu-id="20583-656">Multiple handlers per page</span></span>

<span data-ttu-id="20583-657">Poniższa Strona generuje znaczniki dla dwóch programów obsługi przy użyciu `asp-page-handler` pomocnika tagów:</span><span class="sxs-lookup"><span data-stu-id="20583-657">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="20583-658">Formularz w poprzednim przykładzie ma dwa przyciski przesyłania, z których `FormActionTagHelper` każdy używa do przesłania do innego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="20583-658">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="20583-659">Ten `asp-page-handler` atrybut jest towarzyszący do `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="20583-659">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="20583-660">`asp-page-handler`generuje adresy URL, które przesyłają do każdej metody obsługi zdefiniowanej przez stronę.</span><span class="sxs-lookup"><span data-stu-id="20583-660">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="20583-661">`asp-page`nie została określona, ponieważ próbka jest łączona z bieżącą stroną.</span><span class="sxs-lookup"><span data-stu-id="20583-661">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="20583-662">Model strony:</span><span class="sxs-lookup"><span data-stu-id="20583-662">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="20583-663">Poprzedni kod używa *nazwanych metod obsługi*.</span><span class="sxs-lookup"><span data-stu-id="20583-663">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="20583-664">Nazwane metody obsługi są tworzone przez pobranie tekstu w nazwie po `On<HTTP Verb>` i przed `Async` (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="20583-664">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="20583-665">W poprzednim przykładzie metody strony są onpost**JoinList**Async i Onpost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="20583-665">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="20583-666">Po usunięciu funkcji onpost i *Async* nazwy programów obsługi `JoinList` są `JoinListUC`i.</span><span class="sxs-lookup"><span data-stu-id="20583-666">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="20583-667">Korzystając z powyższego kodu, ścieżka URL, która jest `OnPostJoinListAsync` przesyłana do usługi, to `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="20583-667">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="20583-668">Ścieżka URL, która jest przesyłana `OnPostJoinListUCAsync` do `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`programu, to.</span><span class="sxs-lookup"><span data-stu-id="20583-668">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="20583-669">Trasy niestandardowe</span><span class="sxs-lookup"><span data-stu-id="20583-669">Custom routes</span></span>

<span data-ttu-id="20583-670">Użyj dyrektywy `@page` , aby:</span><span class="sxs-lookup"><span data-stu-id="20583-670">Use the `@page` directive to:</span></span>

* <span data-ttu-id="20583-671">Określ trasę niestandardową dla strony.</span><span class="sxs-lookup"><span data-stu-id="20583-671">Specify a custom route to a page.</span></span> <span data-ttu-id="20583-672">Na przykład trasy do strony informacje można ustawić na `/Some/Other/Path` wartość z. `@page "/Some/Other/Path"`</span><span class="sxs-lookup"><span data-stu-id="20583-672">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="20583-673">Dołącz segmenty do domyślnej trasy strony.</span><span class="sxs-lookup"><span data-stu-id="20583-673">Append segments to a page's default route.</span></span> <span data-ttu-id="20583-674">Na przykład segment "Item" można dodać do domyślnej trasy strony przy użyciu `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="20583-674">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="20583-675">Dołącz parametry do domyślnej trasy strony.</span><span class="sxs-lookup"><span data-stu-id="20583-675">Append parameters to a page's default route.</span></span> <span data-ttu-id="20583-676">Na przykład parametr ID, `id`, może być wymagany dla strony z. `@page "{id}"`</span><span class="sxs-lookup"><span data-stu-id="20583-676">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="20583-677">Ścieżka względna elementu głównego wypisana przez tyldę`~`() na początku ścieżki jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="20583-677">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="20583-678">Na przykład `@page "~/Some/Other/Path"` jest taka sama jak `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="20583-678">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="20583-679">Można zmienić ciąg `?handler=JoinList` zapytania w adresie URL na segment `/JoinList` trasy, określając szablon `@page "{handler?}"`trasy.</span><span class="sxs-lookup"><span data-stu-id="20583-679">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="20583-680">Jeśli nie podoba Ci się ciąg `?handler=JoinList` zapytania w adresie URL, możesz zmienić trasy, aby umieścić nazwę programu obsługi w części adresu URL.</span><span class="sxs-lookup"><span data-stu-id="20583-680">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="20583-681">Możesz dostosować trasę, dodając szablon trasy ujęty w podwójne cudzysłowy po `@page` dyrektywie.</span><span class="sxs-lookup"><span data-stu-id="20583-681">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="20583-682">Korzystając z powyższego kodu, ścieżka URL, która jest `OnPostJoinListAsync` przesyłana do usługi, to `https://localhost:5001/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="20583-682">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="20583-683">Ścieżka URL, która jest przesyłana `OnPostJoinListUCAsync` do `https://localhost:5001/Customers/CreateFATH/JoinListUC`programu, to.</span><span class="sxs-lookup"><span data-stu-id="20583-683">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="20583-684">Poniżej przedstawiono `handler` , że parametr trasy jest opcjonalny. `?`</span><span class="sxs-lookup"><span data-stu-id="20583-684">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="20583-685">Konfiguracja i ustawienia</span><span class="sxs-lookup"><span data-stu-id="20583-685">Configuration and settings</span></span>

<span data-ttu-id="20583-686">Aby skonfigurować opcje zaawansowane, użyj metody `AddRazorPagesOptions` rozszerzenia w konstruktorze MVC:</span><span class="sxs-lookup"><span data-stu-id="20583-686">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="20583-687">Obecnie można użyć, `RazorPagesOptions` aby ustawić katalog główny dla stron lub dodać konwencje modelu aplikacji dla stron.</span><span class="sxs-lookup"><span data-stu-id="20583-687">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="20583-688">W przyszłości włączysz więcej rozszerzeń w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="20583-688">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="20583-689">Aby wstępnie skompilować widoki, zobacz [kompilacja widoku Razor](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="20583-689">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="20583-690">[Pobierz lub Wyświetl przykładowy kod](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="20583-690">[Download or view sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="20583-691">Zobacz Rozpoczynanie [pracy z usługą Razor Pages](xref:tutorials/razor-pages/razor-pages-start), która kompiluje się w tym wprowadzeniu.</span><span class="sxs-lookup"><span data-stu-id="20583-691">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="20583-692">Określ, że Razor Pages znajdują się w katalogu głównym zawartości</span><span class="sxs-lookup"><span data-stu-id="20583-692">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="20583-693">Domyślnie Razor Pages są umieszczane w katalogu */Pages* .</span><span class="sxs-lookup"><span data-stu-id="20583-693">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="20583-694">Dodaj [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) , aby określić, że Razor Pages znajdują się w katalogu głównym zawartości ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) aplikacji:</span><span class="sxs-lookup"><span data-stu-id="20583-694">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="20583-695">Określ, że Razor Pages znajdują się w niestandardowym katalogu głównym</span><span class="sxs-lookup"><span data-stu-id="20583-695">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="20583-696">Dodaj [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) , aby określić, że Razor Pages znajdują się w niestandardowym katalogu głównym w aplikacji (podaj ścieżkę względną):</span><span class="sxs-lookup"><span data-stu-id="20583-696">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="20583-697">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="20583-697">Additional resources</span></span>

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end
