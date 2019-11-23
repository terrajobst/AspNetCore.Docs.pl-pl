---
title: Introduction to Razor Pages in ASP.NET Core
author: Rick-Anderson
description: Learn how Razor Pages in ASP.NET Core makes coding page-focused scenarios easier and more productive than using MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 10/07/2019
uid: razor-pages/index
ms.openlocfilehash: 67cc4f9b261372996d612f922c9f491f53948ece
ms.sourcegitcommit: ddc813f0f1fb293861a01597532919945b0e7fe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/23/2019
ms.locfileid: "74412073"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="1a44e-103">Introduction to Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a44e-103">Introduction to Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1a44e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="1a44e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="1a44e-105">Razor Pages can make coding page-focused scenarios easier and more productive than using controllers and views.</span><span class="sxs-lookup"><span data-stu-id="1a44e-105">Razor Pages can make coding page-focused scenarios easier and more productive than using controllers and views.</span></span>

<span data-ttu-id="1a44e-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="1a44e-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="1a44e-107">This document provides an introduction to Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1a44e-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="1a44e-108">It's not a step by step tutorial.</span><span class="sxs-lookup"><span data-stu-id="1a44e-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="1a44e-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="1a44e-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="1a44e-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="1a44e-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a44e-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1a44e-111">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a44e-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a44e-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1a44e-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1a44e-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1a44e-114">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1a44e-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="1a44e-115">Create a Razor Pages project</span><span class="sxs-lookup"><span data-stu-id="1a44e-115">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a44e-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a44e-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1a44e-117">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span><span class="sxs-lookup"><span data-stu-id="1a44e-117">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1a44e-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1a44e-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1a44e-119">Run `dotnet new webapp` from the command line.</span><span class="sxs-lookup"><span data-stu-id="1a44e-119">Run `dotnet new webapp` from the command line.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1a44e-120">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1a44e-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="1a44e-121">Run `dotnet new webapp` from the command line.</span><span class="sxs-lookup"><span data-stu-id="1a44e-121">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="1a44e-122">Open the generated *.csproj* file from Visual Studio for Mac.</span><span class="sxs-lookup"><span data-stu-id="1a44e-122">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="1a44e-123">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="1a44e-123">Razor Pages</span></span>

<span data-ttu-id="1a44e-124">Razor Pages is enabled in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="1a44e-124">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Startup.cs?name=snippet_Startup&highlight=12,36)]

<span data-ttu-id="1a44e-125">Consider a basic page: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="1a44e-125">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index.cshtml?highlight=1)]

<span data-ttu-id="1a44e-126">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span><span class="sxs-lookup"><span data-stu-id="1a44e-126">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="1a44e-127">What makes it different is the [@page](xref:mvc/views/razor#page) directive.</span><span class="sxs-lookup"><span data-stu-id="1a44e-127">What makes it different is the [@page](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="1a44e-128">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span><span class="sxs-lookup"><span data-stu-id="1a44e-128">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="1a44e-129">`@page` must be the first Razor directive on a page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-129">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="1a44e-130">`@page` affects the behavior of other [Razor](xref:mvc/views/razor) constructs.</span><span class="sxs-lookup"><span data-stu-id="1a44e-130">`@page` affects the behavior of other [Razor](xref:mvc/views/razor) constructs.</span></span> <span data-ttu-id="1a44e-131">Razor Pages file names have a *.cshtml* suffix.</span><span class="sxs-lookup"><span data-stu-id="1a44e-131">Razor Pages file names have a *.cshtml* suffix.</span></span>

<span data-ttu-id="1a44e-132">A similar page, using a `PageModel` class, is shown in the following two files.</span><span class="sxs-lookup"><span data-stu-id="1a44e-132">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="1a44e-133">The *Pages/Index2.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="1a44e-133">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="1a44e-134">The *Pages/Index2.cshtml.cs* page model:</span><span class="sxs-lookup"><span data-stu-id="1a44e-134">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="1a44e-135">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span><span class="sxs-lookup"><span data-stu-id="1a44e-135">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="1a44e-136">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1a44e-136">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="1a44e-137">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="1a44e-137">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="1a44e-138">The associations of URL paths to pages are determined by the page's location in the file system.</span><span class="sxs-lookup"><span data-stu-id="1a44e-138">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="1a44e-139">The following table shows a Razor Page path and the matching URL:</span><span class="sxs-lookup"><span data-stu-id="1a44e-139">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="1a44e-140">File name and path</span><span class="sxs-lookup"><span data-stu-id="1a44e-140">File name and path</span></span>               | <span data-ttu-id="1a44e-141">matching URL</span><span class="sxs-lookup"><span data-stu-id="1a44e-141">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="1a44e-142">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1a44e-142">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="1a44e-143">`/` or `/Index`</span><span class="sxs-lookup"><span data-stu-id="1a44e-143">`/` or `/Index`</span></span> |
| <span data-ttu-id="1a44e-144">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1a44e-144">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="1a44e-145">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1a44e-145">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="1a44e-146">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1a44e-146">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="1a44e-147">`/Store` or `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="1a44e-147">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="1a44e-148">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="1a44e-148">Notes:</span></span>

* <span data-ttu-id="1a44e-149">The runtime looks for Razor Pages files in the *Pages* folder by default.</span><span class="sxs-lookup"><span data-stu-id="1a44e-149">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="1a44e-150">`Index` is the default page when a URL doesn't include a page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-150">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="1a44e-151">Write a basic form</span><span class="sxs-lookup"><span data-stu-id="1a44e-151">Write a basic form</span></span>

<span data-ttu-id="1a44e-152">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span><span class="sxs-lookup"><span data-stu-id="1a44e-152">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="1a44e-153">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span><span class="sxs-lookup"><span data-stu-id="1a44e-153">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="1a44e-154">Consider a page that implements a basic "contact us" form for the `Contact` model:</span><span class="sxs-lookup"><span data-stu-id="1a44e-154">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="1a44e-155">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) file.</span><span class="sxs-lookup"><span data-stu-id="1a44e-155">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) file.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Startup.cs?name=snippet)]

<span data-ttu-id="1a44e-156">The data model:</span><span class="sxs-lookup"><span data-stu-id="1a44e-156">The data model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="1a44e-157">The db context:</span><span class="sxs-lookup"><span data-stu-id="1a44e-157">The db context:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Data/CustomerDbContext.cs)]

<span data-ttu-id="1a44e-158">The *Pages/Create.cshtml* view file:</span><span class="sxs-lookup"><span data-stu-id="1a44e-158">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="1a44e-159">The *Pages/Create.cshtml.cs* page model:</span><span class="sxs-lookup"><span data-stu-id="1a44e-159">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="1a44e-160">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-160">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="1a44e-161">The `PageModel` class allows separation of the logic of a page from its presentation.</span><span class="sxs-lookup"><span data-stu-id="1a44e-161">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="1a44e-162">It defines page handlers for requests sent to the page and the data used to render the page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-162">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="1a44e-163">This separation allows:</span><span class="sxs-lookup"><span data-stu-id="1a44e-163">This separation allows:</span></span>

* <span data-ttu-id="1a44e-164">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1a44e-164">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* [<span data-ttu-id="1a44e-165">Testowanie jednostek</span><span class="sxs-lookup"><span data-stu-id="1a44e-165">Unit testing</span></span>](xref:test/razor-pages-tests)

<span data-ttu-id="1a44e-166">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span><span class="sxs-lookup"><span data-stu-id="1a44e-166">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="1a44e-167">Handler methods for any HTTP verb can be added.</span><span class="sxs-lookup"><span data-stu-id="1a44e-167">Handler methods for any HTTP verb can be added.</span></span> <span data-ttu-id="1a44e-168">The most common handlers are:</span><span class="sxs-lookup"><span data-stu-id="1a44e-168">The most common handlers are:</span></span>

* <span data-ttu-id="1a44e-169">`OnGet` to initialize state needed for the page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-169">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="1a44e-170">In the preceding code, the `OnGet` method displays the *CreateModel.cshtml* Razor Page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-170">In the preceding code, the `OnGet` method displays the *CreateModel.cshtml* Razor Page.</span></span>
* <span data-ttu-id="1a44e-171">`OnPost` to handle form submissions.</span><span class="sxs-lookup"><span data-stu-id="1a44e-171">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="1a44e-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span><span class="sxs-lookup"><span data-stu-id="1a44e-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="1a44e-173">The preceding code is typical for Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1a44e-173">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="1a44e-174">If you're familiar with ASP.NET apps using controllers and views:</span><span class="sxs-lookup"><span data-stu-id="1a44e-174">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="1a44e-175">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span><span class="sxs-lookup"><span data-stu-id="1a44e-175">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="1a44e-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results work the same with Controllers and Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1a44e-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results work the same with Controllers and Razor Pages.</span></span> 

<span data-ttu-id="1a44e-177">The previous `OnPostAsync` method:</span><span class="sxs-lookup"><span data-stu-id="1a44e-177">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="1a44e-178">The basic flow of `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="1a44e-178">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="1a44e-179">Check for validation errors.</span><span class="sxs-lookup"><span data-stu-id="1a44e-179">Check for validation errors.</span></span>

* <span data-ttu-id="1a44e-180">If there are no errors, save the data and redirect.</span><span class="sxs-lookup"><span data-stu-id="1a44e-180">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="1a44e-181">If there are errors, show the page again with validation messages.</span><span class="sxs-lookup"><span data-stu-id="1a44e-181">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="1a44e-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span><span class="sxs-lookup"><span data-stu-id="1a44e-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="1a44e-183">The *Pages/Create.cshtml* view file:</span><span class="sxs-lookup"><span data-stu-id="1a44e-183">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="1a44e-184">The rendered HTML from *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1a44e-184">The rendered HTML from *Pages/Create.cshtml*:</span></span>

[!code-html[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.html)]

<span data-ttu-id="1a44e-185">In the previous code, posting the form:</span><span class="sxs-lookup"><span data-stu-id="1a44e-185">In the previous code, posting the form:</span></span>

* <span data-ttu-id="1a44e-186">With valid data:</span><span class="sxs-lookup"><span data-stu-id="1a44e-186">With valid data:</span></span>

  * <span data-ttu-id="1a44e-187">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> helper method.</span><span class="sxs-lookup"><span data-stu-id="1a44e-187">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> helper method.</span></span> <span data-ttu-id="1a44e-188">`RedirectToPage` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span><span class="sxs-lookup"><span data-stu-id="1a44e-188">`RedirectToPage` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span></span> <span data-ttu-id="1a44e-189">`RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="1a44e-189">`RedirectToPage`:</span></span>

    * <span data-ttu-id="1a44e-190">Is an action result.</span><span class="sxs-lookup"><span data-stu-id="1a44e-190">Is an action result.</span></span>
    * <span data-ttu-id="1a44e-191">Is similar to `RedirectToAction` or `RedirectToRoute` (used in controllers and views).</span><span class="sxs-lookup"><span data-stu-id="1a44e-191">Is similar to `RedirectToAction` or `RedirectToRoute` (used in controllers and views).</span></span>
    * <span data-ttu-id="1a44e-192">Is customized for pages.</span><span class="sxs-lookup"><span data-stu-id="1a44e-192">Is customized for pages.</span></span> <span data-ttu-id="1a44e-193">In the preceding sample, it redirects to the root Index page (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="1a44e-193">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="1a44e-194">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span><span class="sxs-lookup"><span data-stu-id="1a44e-194">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

* <span data-ttu-id="1a44e-195">With validation errors that are passed to the server:</span><span class="sxs-lookup"><span data-stu-id="1a44e-195">With validation errors that are passed to the server:</span></span>

  * <span data-ttu-id="1a44e-196">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> helper method.</span><span class="sxs-lookup"><span data-stu-id="1a44e-196">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> helper method.</span></span> <span data-ttu-id="1a44e-197">`Page` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>.</span><span class="sxs-lookup"><span data-stu-id="1a44e-197">`Page` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>.</span></span> <span data-ttu-id="1a44e-198">Returning `Page` is similar to how actions in controllers return `View`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-198">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="1a44e-199">`PageResult` is the default return type for a handler method.</span><span class="sxs-lookup"><span data-stu-id="1a44e-199">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="1a44e-200">A handler method that returns `void` renders the page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-200">A handler method that returns `void` renders the page.</span></span>
  * <span data-ttu-id="1a44e-201">In the preceding example, posting the form with no value results in [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) returning false.</span><span class="sxs-lookup"><span data-stu-id="1a44e-201">In the preceding example, posting the form with no value results in [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) returning false.</span></span> <span data-ttu-id="1a44e-202">In this sample, no validation errors are displayed on the client.</span><span class="sxs-lookup"><span data-stu-id="1a44e-202">In this sample, no validation errors are displayed on the client.</span></span> <span data-ttu-id="1a44e-203">Validation error handing is covered later in this document.</span><span class="sxs-lookup"><span data-stu-id="1a44e-203">Validation error handing is covered later in this document.</span></span>

  [!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

* <span data-ttu-id="1a44e-204">With validation errors detected by client side validation:</span><span class="sxs-lookup"><span data-stu-id="1a44e-204">With validation errors detected by client side validation:</span></span>

  * <span data-ttu-id="1a44e-205">Data is **not** posted to the server.</span><span class="sxs-lookup"><span data-stu-id="1a44e-205">Data is **not** posted to the server.</span></span>
  * <span data-ttu-id="1a44e-206">Client-side validation is explained later in this document.</span><span class="sxs-lookup"><span data-stu-id="1a44e-206">Client-side validation is explained later in this document.</span></span>

<span data-ttu-id="1a44e-207">The `Customer` property uses [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute to opt in to model binding:</span><span class="sxs-lookup"><span data-stu-id="1a44e-207">The `Customer` property uses [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute to opt in to model binding:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=15-16)]

<span data-ttu-id="1a44e-208">`[BindProperty]` should **not** be used on models containing properties that should not be changed by the client.</span><span class="sxs-lookup"><span data-stu-id="1a44e-208">`[BindProperty]` should **not** be used on models containing properties that should not be changed by the client.</span></span> <span data-ttu-id="1a44e-209">For more information, see [Overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="1a44e-209">For more information, see [Overposting](xref:data/ef-rp/crud#overposting).</span></span>

<span data-ttu-id="1a44e-210">Razor Pages, by default, bind properties only with non-`GET` verbs.</span><span class="sxs-lookup"><span data-stu-id="1a44e-210">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="1a44e-211">Binding to properties removes the need to writing code to convert HTTP data to the model type.</span><span class="sxs-lookup"><span data-stu-id="1a44e-211">Binding to properties removes the need to writing code to convert HTTP data to the model type.</span></span> <span data-ttu-id="1a44e-212">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span><span class="sxs-lookup"><span data-stu-id="1a44e-212">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="1a44e-213">Reviewing the *Pages/Create.cshtml* view file:</span><span class="sxs-lookup"><span data-stu-id="1a44e-213">Reviewing the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml?highlight=3,9)]

* <span data-ttu-id="1a44e-214">In the preceding code, the [input tag helper](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` binds the HTML `<input>` element to the `Customer.Name` model expression.</span><span class="sxs-lookup"><span data-stu-id="1a44e-214">In the preceding code, the [input tag helper](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` binds the HTML `<input>` element to the `Customer.Name` model expression.</span></span>
* <span data-ttu-id="1a44e-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) makes Tag Helpers available.</span><span class="sxs-lookup"><span data-stu-id="1a44e-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) makes Tag Helpers available.</span></span>

### <a name="the-home-page"></a><span data-ttu-id="1a44e-216">The home page</span><span class="sxs-lookup"><span data-stu-id="1a44e-216">The home page</span></span>

<span data-ttu-id="1a44e-217">*Index.cshtml* is the home page:</span><span class="sxs-lookup"><span data-stu-id="1a44e-217">*Index.cshtml* is the home page:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml)]

<span data-ttu-id="1a44e-218">The associated `PageModel` class (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1a44e-218">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="1a44e-219">The *Index.cshtml* file contains the following markup:</span><span class="sxs-lookup"><span data-stu-id="1a44e-219">The *Index.cshtml* file contains the following markup:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=21)]

<span data-ttu-id="1a44e-220">The `<a /a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-220">The `<a /a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="1a44e-221">The link contains route data with the contact ID.</span><span class="sxs-lookup"><span data-stu-id="1a44e-221">The link contains route data with the contact ID.</span></span> <span data-ttu-id="1a44e-222">For example, `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-222">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="1a44e-223">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="1a44e-223">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>

<span data-ttu-id="1a44e-224">The *Index.cshtml* file contains markup to create a delete button for each customer contact:</span><span class="sxs-lookup"><span data-stu-id="1a44e-224">The *Index.cshtml* file contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=22-23)]

<span data-ttu-id="1a44e-225">The rendered HTML:</span><span class="sxs-lookup"><span data-stu-id="1a44e-225">The rendered HTML:</span></span>

```HTML
<button type="submit" formaction="/Customers?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="1a44e-226">When the delete button is rendered in HTML, its [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) includes parameters for:</span><span class="sxs-lookup"><span data-stu-id="1a44e-226">When the delete button is rendered in HTML, its [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) includes parameters for:</span></span>

* <span data-ttu-id="1a44e-227">The customer contact ID, specified by the `asp-route-id` attribute.</span><span class="sxs-lookup"><span data-stu-id="1a44e-227">The customer contact ID, specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="1a44e-228">The `handler`, specified by the `asp-page-handler` attribute.</span><span class="sxs-lookup"><span data-stu-id="1a44e-228">The `handler`, specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="1a44e-229">When the button is selected, a form `POST` request is sent to the server.</span><span class="sxs-lookup"><span data-stu-id="1a44e-229">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="1a44e-230">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-230">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="1a44e-231">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span><span class="sxs-lookup"><span data-stu-id="1a44e-231">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="1a44e-232">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span><span class="sxs-lookup"><span data-stu-id="1a44e-232">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="1a44e-233">The `OnPostDeleteAsync` method:</span><span class="sxs-lookup"><span data-stu-id="1a44e-233">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="1a44e-234">Gets the `id` from the query string.</span><span class="sxs-lookup"><span data-stu-id="1a44e-234">Gets the `id` from the query string.</span></span>
* <span data-ttu-id="1a44e-235">Queries the database for the customer contact with `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-235">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="1a44e-236">If the customer contact is found, it's removed and the database is updated.</span><span class="sxs-lookup"><span data-stu-id="1a44e-236">If the customer contact is found, it's removed and the database is updated.</span></span>
* <span data-ttu-id="1a44e-237">Calls <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> to redirect to the root Index page (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="1a44e-237">Calls <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> to redirect to the root Index page (`/Index`).</span></span>

### <a name="the-editcshtml-file"></a><span data-ttu-id="1a44e-238">The Edit.cshtml file</span><span class="sxs-lookup"><span data-stu-id="1a44e-238">The Edit.cshtml file</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml?highlight=1)]

<span data-ttu-id="1a44e-239">The first line contains the `@page "{id:int}"` directive.</span><span class="sxs-lookup"><span data-stu-id="1a44e-239">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="1a44e-240">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span><span class="sxs-lookup"><span data-stu-id="1a44e-240">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="1a44e-241">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span><span class="sxs-lookup"><span data-stu-id="1a44e-241">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="1a44e-242">To make the ID optional, append `?` to the route constraint:</span><span class="sxs-lookup"><span data-stu-id="1a44e-242">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="1a44e-243">The *Edit.cshtml.cs* file:</span><span class="sxs-lookup"><span data-stu-id="1a44e-243">The *Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml.cs?name=snippet)]

## <a name="validation"></a><span data-ttu-id="1a44e-244">Walidacja</span><span class="sxs-lookup"><span data-stu-id="1a44e-244">Validation</span></span>

<span data-ttu-id="1a44e-245">Validation rules:</span><span class="sxs-lookup"><span data-stu-id="1a44e-245">Validation rules:</span></span>

* <span data-ttu-id="1a44e-246">Are declaratively specified in the model class.</span><span class="sxs-lookup"><span data-stu-id="1a44e-246">Are declaratively specified in the model class.</span></span>
* <span data-ttu-id="1a44e-247">Are enforced everywhere in the app.</span><span class="sxs-lookup"><span data-stu-id="1a44e-247">Are enforced everywhere in the app.</span></span>

<span data-ttu-id="1a44e-248">The <xref:System.ComponentModel.DataAnnotations> namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span><span class="sxs-lookup"><span data-stu-id="1a44e-248">The <xref:System.ComponentModel.DataAnnotations> namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="1a44e-249">DataAnnotations also contains formatting attributes like [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) that help with formatting and don't provide any validation.</span><span class="sxs-lookup"><span data-stu-id="1a44e-249">DataAnnotations also contains formatting attributes like [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) that help with formatting and don't provide any validation.</span></span>

<span data-ttu-id="1a44e-250">Consider the `Customer` model:</span><span class="sxs-lookup"><span data-stu-id="1a44e-250">Consider the `Customer` model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="1a44e-251">Using the following *Create.cshtml* view file:</span><span class="sxs-lookup"><span data-stu-id="1a44e-251">Using the following *Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=3,8-9,15-99)]

<span data-ttu-id="1a44e-252">The preceding code:</span><span class="sxs-lookup"><span data-stu-id="1a44e-252">The preceding code:</span></span>

* <span data-ttu-id="1a44e-253">Includes jQuery and jQuery validation scripts.</span><span class="sxs-lookup"><span data-stu-id="1a44e-253">Includes jQuery and jQuery validation scripts.</span></span>
* <span data-ttu-id="1a44e-254">Uses the `<div />` and `<span />` [Tag Helpers](xref:mvc/views/tag-helpers/intro) to enable:</span><span class="sxs-lookup"><span data-stu-id="1a44e-254">Uses the `<div />` and `<span />` [Tag Helpers](xref:mvc/views/tag-helpers/intro) to enable:</span></span>

  * <span data-ttu-id="1a44e-255">Client-side validation.</span><span class="sxs-lookup"><span data-stu-id="1a44e-255">Client-side validation.</span></span>
  * <span data-ttu-id="1a44e-256">Validation error rendering.</span><span class="sxs-lookup"><span data-stu-id="1a44e-256">Validation error rendering.</span></span>

* <span data-ttu-id="1a44e-257">Generates the following HTML:</span><span class="sxs-lookup"><span data-stu-id="1a44e-257">Generates the following HTML:</span></span>

  [!code-html[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create5.html)]

<span data-ttu-id="1a44e-258">Posting the Create form without a name value displays the error message "The Name field is required."</span><span class="sxs-lookup"><span data-stu-id="1a44e-258">Posting the Create form without a name value displays the error message "The Name field is required."</span></span> <span data-ttu-id="1a44e-259">on the form.</span><span class="sxs-lookup"><span data-stu-id="1a44e-259">on the form.</span></span> <span data-ttu-id="1a44e-260">If JavaScript is enabled on the client, the browser displays the error without posting to the server.</span><span class="sxs-lookup"><span data-stu-id="1a44e-260">If JavaScript is enabled on the client, the browser displays the error without posting to the server.</span></span>

<span data-ttu-id="1a44e-261">The `[StringLength(10)]` attribute generates `data-val-length-max="10"` on the rendered HTML.</span><span class="sxs-lookup"><span data-stu-id="1a44e-261">The `[StringLength(10)]` attribute generates `data-val-length-max="10"` on the rendered HTML.</span></span> <span data-ttu-id="1a44e-262">`data-val-length-max` prevents browsers from entering more than the maximum length specified.</span><span class="sxs-lookup"><span data-stu-id="1a44e-262">`data-val-length-max` prevents browsers from entering more than the maximum length specified.</span></span> <span data-ttu-id="1a44e-263">If a tool such as [Fiddler](https://www.telerik.com/fiddler) is used to edit and replay the post:</span><span class="sxs-lookup"><span data-stu-id="1a44e-263">If a tool such as [Fiddler](https://www.telerik.com/fiddler) is used to edit and replay the post:</span></span>

* <span data-ttu-id="1a44e-264">With the name longer than 10.</span><span class="sxs-lookup"><span data-stu-id="1a44e-264">With the name longer than 10.</span></span>
* <span data-ttu-id="1a44e-265">The error message "The field Name must be a string with a maximum length of 10."</span><span class="sxs-lookup"><span data-stu-id="1a44e-265">The error message "The field Name must be a string with a maximum length of 10."</span></span> <span data-ttu-id="1a44e-266">is returned.</span><span class="sxs-lookup"><span data-stu-id="1a44e-266">is returned.</span></span>

<span data-ttu-id="1a44e-267">Consider the following `Movie` model:</span><span class="sxs-lookup"><span data-stu-id="1a44e-267">Consider the following `Movie` model:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="1a44e-268">The validation attributes specify behavior to enforce on the model properties they're applied to:</span><span class="sxs-lookup"><span data-stu-id="1a44e-268">The validation attributes specify behavior to enforce on the model properties they're applied to:</span></span>

* <span data-ttu-id="1a44e-269">The `Required` and `MinimumLength` attributes indicate that a property must have a value, but nothing prevents a user from entering white space to satisfy this validation.</span><span class="sxs-lookup"><span data-stu-id="1a44e-269">The `Required` and `MinimumLength` attributes indicate that a property must have a value, but nothing prevents a user from entering white space to satisfy this validation.</span></span>
* <span data-ttu-id="1a44e-270">The `RegularExpression` attribute is used to limit what characters can be input.</span><span class="sxs-lookup"><span data-stu-id="1a44e-270">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="1a44e-271">In the preceding code, "Genre":</span><span class="sxs-lookup"><span data-stu-id="1a44e-271">In the preceding code, "Genre":</span></span>

  * <span data-ttu-id="1a44e-272">Must only use letters.</span><span class="sxs-lookup"><span data-stu-id="1a44e-272">Must only use letters.</span></span>
  * <span data-ttu-id="1a44e-273">The first letter is required to be uppercase.</span><span class="sxs-lookup"><span data-stu-id="1a44e-273">The first letter is required to be uppercase.</span></span> <span data-ttu-id="1a44e-274">White space, numbers, and special characters are not allowed.</span><span class="sxs-lookup"><span data-stu-id="1a44e-274">White space, numbers, and special characters are not allowed.</span></span>

* <span data-ttu-id="1a44e-275">The `RegularExpression` "Rating":</span><span class="sxs-lookup"><span data-stu-id="1a44e-275">The `RegularExpression` "Rating":</span></span>

  * <span data-ttu-id="1a44e-276">Requires that the first character be an uppercase letter.</span><span class="sxs-lookup"><span data-stu-id="1a44e-276">Requires that the first character be an uppercase letter.</span></span>
  * <span data-ttu-id="1a44e-277">Allows special characters and numbers in subsequent spaces.</span><span class="sxs-lookup"><span data-stu-id="1a44e-277">Allows special characters and numbers in subsequent spaces.</span></span> <span data-ttu-id="1a44e-278">"PG-13" is valid for a rating, but fails for a "Genre".</span><span class="sxs-lookup"><span data-stu-id="1a44e-278">"PG-13" is valid for a rating, but fails for a "Genre".</span></span>

* <span data-ttu-id="1a44e-279">The `Range` attribute constrains a value to within a specified range.</span><span class="sxs-lookup"><span data-stu-id="1a44e-279">The `Range` attribute constrains a value to within a specified range.</span></span>
* <span data-ttu-id="1a44e-280">The `StringLength` attribute sets the maximum length of a string property, and optionally its minimum length.</span><span class="sxs-lookup"><span data-stu-id="1a44e-280">The `StringLength` attribute sets the maximum length of a string property, and optionally its minimum length.</span></span>
* <span data-ttu-id="1a44e-281">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span><span class="sxs-lookup"><span data-stu-id="1a44e-281">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="1a44e-282">The Create page for the `Movie` model shows displays errors with invalid values:</span><span class="sxs-lookup"><span data-stu-id="1a44e-282">The Create page for the `Movie` model shows displays errors with invalid values:</span></span>

![Movie view form with multiple jQuery client-side validation errors](~/tutorials/razor-pages/validation/_static/val.png)

<span data-ttu-id="1a44e-284">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="1a44e-284">For more information, see:</span></span>

* [<span data-ttu-id="1a44e-285">Add validation to the Movie app</span><span class="sxs-lookup"><span data-stu-id="1a44e-285">Add validation to the Movie app</span></span>](xref:tutorials/razor-pages/validation)
* <span data-ttu-id="1a44e-286">[Model validation in ASP.NET Core](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="1a44e-286">[Model validation in ASP.NET Core](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="1a44e-287">Handle HEAD requests with an OnGet handler fallback</span><span class="sxs-lookup"><span data-stu-id="1a44e-287">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="1a44e-288">`HEAD` requests allow retrieving the headers for a specific resource.</span><span class="sxs-lookup"><span data-stu-id="1a44e-288">`HEAD` requests allow retrieving the headers for a specific resource.</span></span> <span data-ttu-id="1a44e-289">Unlike `GET` requests, `HEAD` requests don't return a response body.</span><span class="sxs-lookup"><span data-stu-id="1a44e-289">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="1a44e-290">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span><span class="sxs-lookup"><span data-stu-id="1a44e-290">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Privacy.cshtml.cs?name=snippet)]

<span data-ttu-id="1a44e-291">Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span><span class="sxs-lookup"><span data-stu-id="1a44e-291">Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="1a44e-292">XSRF/CSRF and Razor Pages</span><span class="sxs-lookup"><span data-stu-id="1a44e-292">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="1a44e-293">Razor Pages are protected by [Antiforgery validation](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="1a44e-293">Razor Pages are protected by [Antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="1a44e-294">The [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span><span class="sxs-lookup"><span data-stu-id="1a44e-294">The [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="1a44e-295">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span><span class="sxs-lookup"><span data-stu-id="1a44e-295">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="1a44e-296">Pages work with all the capabilities of the Razor view engine.</span><span class="sxs-lookup"><span data-stu-id="1a44e-296">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="1a44e-297">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, and *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span><span class="sxs-lookup"><span data-stu-id="1a44e-297">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, and *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="1a44e-298">Let's declutter this page by taking advantage of some of those capabilities.</span><span class="sxs-lookup"><span data-stu-id="1a44e-298">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="1a44e-299">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1a44e-299">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Shared/_Layout2.cshtml?hightlight=12)]

<span data-ttu-id="1a44e-300">The [Layout](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="1a44e-300">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="1a44e-301">Controls the layout of each page (unless the page opts out of layout).</span><span class="sxs-lookup"><span data-stu-id="1a44e-301">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="1a44e-302">Imports HTML structures such as JavaScript and stylesheets.</span><span class="sxs-lookup"><span data-stu-id="1a44e-302">Imports HTML structures such as JavaScript and stylesheets.</span></span>
* <span data-ttu-id="1a44e-303">The contents of the Razor page are rendered where `@RenderBody()` is called.</span><span class="sxs-lookup"><span data-stu-id="1a44e-303">The contents of the Razor page are rendered where `@RenderBody()` is called.</span></span>

<span data-ttu-id="1a44e-304">For more information, see [layout page](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="1a44e-304">For more information, see [layout page](xref:mvc/views/layout).</span></span>

<span data-ttu-id="1a44e-305">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1a44e-305">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="1a44e-306">The layout is in the *Pages/Shared* folder.</span><span class="sxs-lookup"><span data-stu-id="1a44e-306">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="1a44e-307">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-307">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="1a44e-308">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span><span class="sxs-lookup"><span data-stu-id="1a44e-308">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="1a44e-309">The layout file should go in the *Pages/Shared* folder.</span><span class="sxs-lookup"><span data-stu-id="1a44e-309">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="1a44e-310">We recommend you **not** put the layout file in the *Views/Shared* folder.</span><span class="sxs-lookup"><span data-stu-id="1a44e-310">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="1a44e-311">*Views/Shared* is an MVC views pattern.</span><span class="sxs-lookup"><span data-stu-id="1a44e-311">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="1a44e-312">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span><span class="sxs-lookup"><span data-stu-id="1a44e-312">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="1a44e-313">View search from a Razor Page includes the *Pages* folder.</span><span class="sxs-lookup"><span data-stu-id="1a44e-313">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="1a44e-314">The layouts, templates, and partials used with MVC controllers and conventional Razor views *just work*.</span><span class="sxs-lookup"><span data-stu-id="1a44e-314">The layouts, templates, and partials used with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="1a44e-315">Add a *Pages/_ViewImports.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="1a44e-315">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="1a44e-316">`@namespace` is explained later in the tutorial.</span><span class="sxs-lookup"><span data-stu-id="1a44e-316">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="1a44e-317">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span><span class="sxs-lookup"><span data-stu-id="1a44e-317">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="1a44e-318">The `@namespace` directive set on a page:</span><span class="sxs-lookup"><span data-stu-id="1a44e-318">The `@namespace` directive set on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="1a44e-319">The `@namespace` directive sets the namespace for the page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-319">The `@namespace` directive sets the namespace for the page.</span></span> <span data-ttu-id="1a44e-320">The `@model` directive doesn't need to include the namespace.</span><span class="sxs-lookup"><span data-stu-id="1a44e-320">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="1a44e-321">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span><span class="sxs-lookup"><span data-stu-id="1a44e-321">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="1a44e-322">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-322">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="1a44e-323">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span><span class="sxs-lookup"><span data-stu-id="1a44e-323">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="1a44e-324">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span><span class="sxs-lookup"><span data-stu-id="1a44e-324">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="1a44e-325">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span><span class="sxs-lookup"><span data-stu-id="1a44e-325">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="1a44e-326">`@namespace` *also works with conventional Razor views.*</span><span class="sxs-lookup"><span data-stu-id="1a44e-326">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="1a44e-327">Consider the *Pages/Create.cshtml* view file:</span><span class="sxs-lookup"><span data-stu-id="1a44e-327">Consider the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=2-3)]

<span data-ttu-id="1a44e-328">The updated *Pages/Create.cshtml* view file with *_ViewImports.cshtml* and the preceding layout file:</span><span class="sxs-lookup"><span data-stu-id="1a44e-328">The updated *Pages/Create.cshtml* view file with *_ViewImports.cshtml* and the preceding layout file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.cshtml?highlight=2)]

<span data-ttu-id="1a44e-329">In the preceding code, the *_ViewImports.cshtml* imported the namespace and Tag Helpers.</span><span class="sxs-lookup"><span data-stu-id="1a44e-329">In the preceding code, the *_ViewImports.cshtml* imported the namespace and Tag Helpers.</span></span> <span data-ttu-id="1a44e-330">The layout file imported the JavaScript files.</span><span class="sxs-lookup"><span data-stu-id="1a44e-330">The layout file imported the JavaScript files.</span></span>

<span data-ttu-id="1a44e-331">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span><span class="sxs-lookup"><span data-stu-id="1a44e-331">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="1a44e-332">For more information on partial views, see <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="1a44e-332">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="1a44e-333">URL generation for Pages</span><span class="sxs-lookup"><span data-stu-id="1a44e-333">URL generation for Pages</span></span>

<span data-ttu-id="1a44e-334">The `Create` page, shown previously, uses `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="1a44e-334">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=28)]

<span data-ttu-id="1a44e-335">The app has the following file/folder structure:</span><span class="sxs-lookup"><span data-stu-id="1a44e-335">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="1a44e-336">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="1a44e-336">*/Pages*</span></span>

  * <span data-ttu-id="1a44e-337">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1a44e-337">*Index.cshtml*</span></span>
  * <span data-ttu-id="1a44e-338">*Privacy.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1a44e-338">*Privacy.cshtml*</span></span>
  * <span data-ttu-id="1a44e-339">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="1a44e-339">*/Customers*</span></span>

    * <span data-ttu-id="1a44e-340">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1a44e-340">*Create.cshtml*</span></span>
    * <span data-ttu-id="1a44e-341">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1a44e-341">*Edit.cshtml*</span></span>
    * <span data-ttu-id="1a44e-342">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1a44e-342">*Index.cshtml*</span></span>

<span data-ttu-id="1a44e-343">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Customers/Index.cshtml* after success.</span><span class="sxs-lookup"><span data-stu-id="1a44e-343">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Customers/Index.cshtml* after success.</span></span> <span data-ttu-id="1a44e-344">The string `./Index` is a relative page name used to access the preceding page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-344">The string `./Index` is a relative page name used to access the preceding page.</span></span> <span data-ttu-id="1a44e-345">It is used to generate URLs to the *Pages/Customers/Index.cshtml* page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-345">It is used to generate URLs to the *Pages/Customers/Index.cshtml* page.</span></span> <span data-ttu-id="1a44e-346">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1a44e-346">For example:</span></span>

* `Url.Page("./Index", ...)`
* `<a asp-page="./Index">Customers Index Page</a>`
* `RedirectToPage("./Index")`

<span data-ttu-id="1a44e-347">The absolute page name `/Index` is used to generate URLs to the *Pages/Index.cshtml* page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-347">The absolute page name `/Index` is used to generate URLs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="1a44e-348">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1a44e-348">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">Home Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="1a44e-349">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span><span class="sxs-lookup"><span data-stu-id="1a44e-349">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="1a44e-350">The preceding URL generation samples offer enhanced options and functional capabilities over hard-coding a URL.</span><span class="sxs-lookup"><span data-stu-id="1a44e-350">The preceding URL generation samples offer enhanced options and functional capabilities over hard-coding a URL.</span></span> <span data-ttu-id="1a44e-351">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span><span class="sxs-lookup"><span data-stu-id="1a44e-351">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="1a44e-352">URL generation for pages supports relative names.</span><span class="sxs-lookup"><span data-stu-id="1a44e-352">URL generation for pages supports relative names.</span></span> <span data-ttu-id="1a44e-353">The following table shows which Index page is selected using different `RedirectToPage` parameters in *Pages/Customers/Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1a44e-353">The following table shows which Index page is selected using different `RedirectToPage` parameters in *Pages/Customers/Create.cshtml*.</span></span>

| <span data-ttu-id="1a44e-354">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="1a44e-354">RedirectToPage(x)</span></span>| <span data-ttu-id="1a44e-355">Page</span><span class="sxs-lookup"><span data-stu-id="1a44e-355">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="1a44e-356">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="1a44e-356">RedirectToPage("/Index")</span></span> | <span data-ttu-id="1a44e-357">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="1a44e-357">*Pages/Index*</span></span> |
| <span data-ttu-id="1a44e-358">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="1a44e-358">RedirectToPage("./Index");</span></span> | <span data-ttu-id="1a44e-359">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="1a44e-359">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="1a44e-360">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="1a44e-360">RedirectToPage("../Index")</span></span> | <span data-ttu-id="1a44e-361">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="1a44e-361">*Pages/Index*</span></span> |
| <span data-ttu-id="1a44e-362">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="1a44e-362">RedirectToPage("Index")</span></span>  | <span data-ttu-id="1a44e-363">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="1a44e-363">*Pages/Customers/Index*</span></span> |

<!-- Test via ~/razor-pages/index/3.0sample/RazorPagesContacts/Pages/Customers/Details.cshtml.cs -->

<span data-ttu-id="1a44e-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")` are *relative names*.</span><span class="sxs-lookup"><span data-stu-id="1a44e-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")` are *relative names*.</span></span> <span data-ttu-id="1a44e-365">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-365">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>

<span data-ttu-id="1a44e-366">Relative name linking is useful when building sites with a complex structure.</span><span class="sxs-lookup"><span data-stu-id="1a44e-366">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="1a44e-367">When relative names are used to link between pages in a folder:</span><span class="sxs-lookup"><span data-stu-id="1a44e-367">When relative names are used to link between pages in a folder:</span></span>

* <span data-ttu-id="1a44e-368">Renaming a folder doesn't break the relative links.</span><span class="sxs-lookup"><span data-stu-id="1a44e-368">Renaming a folder doesn't break the relative links.</span></span>
* <span data-ttu-id="1a44e-369">Links are not broken because they don't include the folder name.</span><span class="sxs-lookup"><span data-stu-id="1a44e-369">Links are not broken because they don't include the folder name.</span></span>

<span data-ttu-id="1a44e-370">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span><span class="sxs-lookup"><span data-stu-id="1a44e-370">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="1a44e-371">Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/areas> i <xref:razor-pages/razor-pages-conventions>.</span><span class="sxs-lookup"><span data-stu-id="1a44e-371">For more information, see <xref:mvc/controllers/areas> and <xref:razor-pages/razor-pages-conventions>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="1a44e-372">ViewData attribute</span><span class="sxs-lookup"><span data-stu-id="1a44e-372">ViewData attribute</span></span>

<span data-ttu-id="1a44e-373">Data can be passed to a page with <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>.</span><span class="sxs-lookup"><span data-stu-id="1a44e-373">Data can be passed to a page with <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>.</span></span> <span data-ttu-id="1a44e-374">Properties with the `[ViewData]` attribute have their values stored and loaded from the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>.</span><span class="sxs-lookup"><span data-stu-id="1a44e-374">Properties with the `[ViewData]` attribute have their values stored and loaded from the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>.</span></span>

<span data-ttu-id="1a44e-375">In the following example, the `AboutModel` applies the `[ViewData]` attribute to the `Title` property:</span><span class="sxs-lookup"><span data-stu-id="1a44e-375">In the following example, the `AboutModel` applies the `[ViewData]` attribute to the `Title` property:</span></span>

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

<span data-ttu-id="1a44e-376">In the About page, access the `Title` property as a model property:</span><span class="sxs-lookup"><span data-stu-id="1a44e-376">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="1a44e-377">In the layout, the title is read from the ViewData dictionary:</span><span class="sxs-lookup"><span data-stu-id="1a44e-377">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="1a44e-378">TempData</span><span class="sxs-lookup"><span data-stu-id="1a44e-378">TempData</span></span>

<span data-ttu-id="1a44e-379">ASP.NET Core exposes the <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>.</span><span class="sxs-lookup"><span data-stu-id="1a44e-379">ASP.NET Core exposes the <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>.</span></span> <span data-ttu-id="1a44e-380">This property stores data until it's read.</span><span class="sxs-lookup"><span data-stu-id="1a44e-380">This property stores data until it's read.</span></span> <span data-ttu-id="1a44e-381">The <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> and <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> methods can be used to examine the data without deletion.</span><span class="sxs-lookup"><span data-stu-id="1a44e-381">The <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> and <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="1a44e-382">`TempData` is useful for redirection, when data is needed for more than a single request.</span><span class="sxs-lookup"><span data-stu-id="1a44e-382">`TempData` is useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="1a44e-383">The following code sets the value of `Message` using `TempData`:</span><span class="sxs-lookup"><span data-stu-id="1a44e-383">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="1a44e-384">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-384">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="1a44e-385">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span><span class="sxs-lookup"><span data-stu-id="1a44e-385">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="1a44e-386">For more information, see [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="1a44e-386">For more information, see [TempData](xref:fundamentals/app-state#tempdata).</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="1a44e-387">Multiple handlers per page</span><span class="sxs-lookup"><span data-stu-id="1a44e-387">Multiple handlers per page</span></span>

<span data-ttu-id="1a44e-388">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span><span class="sxs-lookup"><span data-stu-id="1a44e-388">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<span data-ttu-id="1a44e-389">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span><span class="sxs-lookup"><span data-stu-id="1a44e-389">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="1a44e-390">The `asp-page-handler` attribute is a companion to `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-390">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="1a44e-391">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-391">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="1a44e-392">`asp-page` isn't specified because the sample is linking to the current page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-392">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="1a44e-393">The page model:</span><span class="sxs-lookup"><span data-stu-id="1a44e-393">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="1a44e-394">The preceding code uses *named handler methods*.</span><span class="sxs-lookup"><span data-stu-id="1a44e-394">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="1a44e-395">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span><span class="sxs-lookup"><span data-stu-id="1a44e-395">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="1a44e-396">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="1a44e-396">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="1a44e-397">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-397">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="1a44e-398">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-398">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="1a44e-399">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-399">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="1a44e-400">Custom routes</span><span class="sxs-lookup"><span data-stu-id="1a44e-400">Custom routes</span></span>

<span data-ttu-id="1a44e-401">Use the `@page` directive to:</span><span class="sxs-lookup"><span data-stu-id="1a44e-401">Use the `@page` directive to:</span></span>

* <span data-ttu-id="1a44e-402">Specify a custom route to a page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-402">Specify a custom route to a page.</span></span> <span data-ttu-id="1a44e-403">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-403">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="1a44e-404">Append segments to a page's default route.</span><span class="sxs-lookup"><span data-stu-id="1a44e-404">Append segments to a page's default route.</span></span> <span data-ttu-id="1a44e-405">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-405">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="1a44e-406">Append parameters to a page's default route.</span><span class="sxs-lookup"><span data-stu-id="1a44e-406">Append parameters to a page's default route.</span></span> <span data-ttu-id="1a44e-407">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-407">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="1a44e-408">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span><span class="sxs-lookup"><span data-stu-id="1a44e-408">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="1a44e-409">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-409">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="1a44e-410">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-410">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="1a44e-411">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span><span class="sxs-lookup"><span data-stu-id="1a44e-411">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="1a44e-412">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span><span class="sxs-lookup"><span data-stu-id="1a44e-412">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="1a44e-413">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-413">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="1a44e-414">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-414">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="1a44e-415">The `?` following `handler` means the route parameter is optional.</span><span class="sxs-lookup"><span data-stu-id="1a44e-415">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="advanced-configuration-and-settings"></a><span data-ttu-id="1a44e-416">Advanced configuration and settings</span><span class="sxs-lookup"><span data-stu-id="1a44e-416">Advanced configuration and settings</span></span>

<span data-ttu-id="1a44e-417">The configuration and settings in following sections is not required by most apps.</span><span class="sxs-lookup"><span data-stu-id="1a44e-417">The configuration and settings in following sections is not required by most apps.</span></span>

<span data-ttu-id="1a44e-418">To configure advanced options, use the extension method <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>:</span><span class="sxs-lookup"><span data-stu-id="1a44e-418">To configure advanced options, use the extension method <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupRPoptions.cs?name=snippet)]

<span data-ttu-id="1a44e-419">Use the <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> to set the root directory for pages, or add application model conventions for pages.</span><span class="sxs-lookup"><span data-stu-id="1a44e-419">Use the <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="1a44e-420">For more information on conventions, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="1a44e-420">For more information on conventions, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization).</span></span>

<span data-ttu-id="1a44e-421">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="1a44e-421">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation).</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="1a44e-422">Specify that Razor Pages are at the content root</span><span class="sxs-lookup"><span data-stu-id="1a44e-422">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="1a44e-423">By default, Razor Pages are rooted in the */Pages* directory.</span><span class="sxs-lookup"><span data-stu-id="1a44e-423">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="1a44e-424">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) of the app:</span><span class="sxs-lookup"><span data-stu-id="1a44e-424">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) of the app:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesAtContentRoot.cs?name=snippet)]

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="1a44e-425">Specify that Razor Pages are at a custom root directory</span><span class="sxs-lookup"><span data-stu-id="1a44e-425">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="1a44e-426">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> to specify that Razor Pages are at a custom root directory in the app (provide a relative path):</span><span class="sxs-lookup"><span data-stu-id="1a44e-426">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> to specify that Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesRoot.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="1a44e-427">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1a44e-427">Additional resources</span></span>

* <span data-ttu-id="1a44e-428">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction</span><span class="sxs-lookup"><span data-stu-id="1a44e-428">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction</span></span>
* [<span data-ttu-id="1a44e-429">Download or view sample code</span><span class="sxs-lookup"><span data-stu-id="1a44e-429">Download or view sample code</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/3.0sample)
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

<span data-ttu-id="1a44e-430">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="1a44e-430">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="1a44e-431">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span><span class="sxs-lookup"><span data-stu-id="1a44e-431">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="1a44e-432">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="1a44e-432">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="1a44e-433">This document provides an introduction to Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1a44e-433">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="1a44e-434">It's not a step by step tutorial.</span><span class="sxs-lookup"><span data-stu-id="1a44e-434">It's not a step by step tutorial.</span></span> <span data-ttu-id="1a44e-435">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="1a44e-435">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="1a44e-436">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="1a44e-436">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a44e-437">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="1a44e-437">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a44e-438">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a44e-438">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1a44e-439">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1a44e-439">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1a44e-440">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1a44e-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="1a44e-441">Create a Razor Pages project</span><span class="sxs-lookup"><span data-stu-id="1a44e-441">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a44e-442">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a44e-442">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1a44e-443">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span><span class="sxs-lookup"><span data-stu-id="1a44e-443">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1a44e-444">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1a44e-444">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="1a44e-445">Run `dotnet new webapp` from the command line.</span><span class="sxs-lookup"><span data-stu-id="1a44e-445">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="1a44e-446">Open the generated *.csproj* file from Visual Studio for Mac.</span><span class="sxs-lookup"><span data-stu-id="1a44e-446">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1a44e-447">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1a44e-447">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1a44e-448">Run `dotnet new webapp` from the command line.</span><span class="sxs-lookup"><span data-stu-id="1a44e-448">Run `dotnet new webapp` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="1a44e-449">Razor Pages</span><span class="sxs-lookup"><span data-stu-id="1a44e-449">Razor Pages</span></span>

<span data-ttu-id="1a44e-450">Razor Pages is enabled in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="1a44e-450">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="1a44e-451">Consider a basic page: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="1a44e-451">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="1a44e-452">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span><span class="sxs-lookup"><span data-stu-id="1a44e-452">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="1a44e-453">What makes it different is the `@page` directive.</span><span class="sxs-lookup"><span data-stu-id="1a44e-453">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="1a44e-454">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span><span class="sxs-lookup"><span data-stu-id="1a44e-454">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="1a44e-455">`@page` must be the first Razor directive on a page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-455">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="1a44e-456">`@page` affects the behavior of other Razor constructs.</span><span class="sxs-lookup"><span data-stu-id="1a44e-456">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="1a44e-457">A similar page, using a `PageModel` class, is shown in the following two files.</span><span class="sxs-lookup"><span data-stu-id="1a44e-457">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="1a44e-458">The *Pages/Index2.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="1a44e-458">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="1a44e-459">The *Pages/Index2.cshtml.cs* page model:</span><span class="sxs-lookup"><span data-stu-id="1a44e-459">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="1a44e-460">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span><span class="sxs-lookup"><span data-stu-id="1a44e-460">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="1a44e-461">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="1a44e-461">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="1a44e-462">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="1a44e-462">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="1a44e-463">The associations of URL paths to pages are determined by the page's location in the file system.</span><span class="sxs-lookup"><span data-stu-id="1a44e-463">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="1a44e-464">The following table shows a Razor Page path and the matching URL:</span><span class="sxs-lookup"><span data-stu-id="1a44e-464">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="1a44e-465">File name and path</span><span class="sxs-lookup"><span data-stu-id="1a44e-465">File name and path</span></span>               | <span data-ttu-id="1a44e-466">matching URL</span><span class="sxs-lookup"><span data-stu-id="1a44e-466">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="1a44e-467">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1a44e-467">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="1a44e-468">`/` or `/Index`</span><span class="sxs-lookup"><span data-stu-id="1a44e-468">`/` or `/Index`</span></span> |
| <span data-ttu-id="1a44e-469">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1a44e-469">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="1a44e-470">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1a44e-470">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="1a44e-471">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1a44e-471">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="1a44e-472">`/Store` or `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="1a44e-472">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="1a44e-473">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="1a44e-473">Notes:</span></span>

* <span data-ttu-id="1a44e-474">The runtime looks for Razor Pages files in the *Pages* folder by default.</span><span class="sxs-lookup"><span data-stu-id="1a44e-474">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="1a44e-475">`Index` is the default page when a URL doesn't include a page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-475">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="1a44e-476">Write a basic form</span><span class="sxs-lookup"><span data-stu-id="1a44e-476">Write a basic form</span></span>

<span data-ttu-id="1a44e-477">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span><span class="sxs-lookup"><span data-stu-id="1a44e-477">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="1a44e-478">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span><span class="sxs-lookup"><span data-stu-id="1a44e-478">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="1a44e-479">Consider a page that implements a basic "contact us" form for the `Contact` model:</span><span class="sxs-lookup"><span data-stu-id="1a44e-479">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="1a44e-480">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span><span class="sxs-lookup"><span data-stu-id="1a44e-480">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="1a44e-481">The data model:</span><span class="sxs-lookup"><span data-stu-id="1a44e-481">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="1a44e-482">The db context:</span><span class="sxs-lookup"><span data-stu-id="1a44e-482">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="1a44e-483">The *Pages/Create.cshtml* view file:</span><span class="sxs-lookup"><span data-stu-id="1a44e-483">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="1a44e-484">The *Pages/Create.cshtml.cs* page model:</span><span class="sxs-lookup"><span data-stu-id="1a44e-484">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="1a44e-485">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-485">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="1a44e-486">The `PageModel` class allows separation of the logic of a page from its presentation.</span><span class="sxs-lookup"><span data-stu-id="1a44e-486">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="1a44e-487">It defines page handlers for requests sent to the page and the data used to render the page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-487">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="1a44e-488">This separation allows:</span><span class="sxs-lookup"><span data-stu-id="1a44e-488">This separation allows:</span></span>

* <span data-ttu-id="1a44e-489">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1a44e-489">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="1a44e-490">[Unit testing](xref:test/razor-pages-tests) the pages.</span><span class="sxs-lookup"><span data-stu-id="1a44e-490">[Unit testing](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="1a44e-491">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span><span class="sxs-lookup"><span data-stu-id="1a44e-491">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="1a44e-492">You can add handler methods for any HTTP verb.</span><span class="sxs-lookup"><span data-stu-id="1a44e-492">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="1a44e-493">The most common handlers are:</span><span class="sxs-lookup"><span data-stu-id="1a44e-493">The most common handlers are:</span></span>

* <span data-ttu-id="1a44e-494">`OnGet` to initialize state needed for the page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-494">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="1a44e-495">[OnGet](#OnGet) sample.</span><span class="sxs-lookup"><span data-stu-id="1a44e-495">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="1a44e-496">`OnPost` to handle form submissions.</span><span class="sxs-lookup"><span data-stu-id="1a44e-496">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="1a44e-497">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span><span class="sxs-lookup"><span data-stu-id="1a44e-497">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="1a44e-498">The preceding code is typical for Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1a44e-498">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="1a44e-499">If you're familiar with ASP.NET apps using controllers and views:</span><span class="sxs-lookup"><span data-stu-id="1a44e-499">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="1a44e-500">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span><span class="sxs-lookup"><span data-stu-id="1a44e-500">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="1a44e-501">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), [Validation](xref:mvc/models/validation),  and action results are shared.</span><span class="sxs-lookup"><span data-stu-id="1a44e-501">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), [Validation](xref:mvc/models/validation),  and action results are shared.</span></span>

<span data-ttu-id="1a44e-502">The previous `OnPostAsync` method:</span><span class="sxs-lookup"><span data-stu-id="1a44e-502">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="1a44e-503">The basic flow of `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="1a44e-503">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="1a44e-504">Check for validation errors.</span><span class="sxs-lookup"><span data-stu-id="1a44e-504">Check for validation errors.</span></span>

* <span data-ttu-id="1a44e-505">If there are no errors, save the data and redirect.</span><span class="sxs-lookup"><span data-stu-id="1a44e-505">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="1a44e-506">If there are errors, show the page again with validation messages.</span><span class="sxs-lookup"><span data-stu-id="1a44e-506">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="1a44e-507">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span><span class="sxs-lookup"><span data-stu-id="1a44e-507">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="1a44e-508">In many cases, validation errors would be detected on the client, and never submitted to the server.</span><span class="sxs-lookup"><span data-stu-id="1a44e-508">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="1a44e-509">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-509">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="1a44e-510">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span><span class="sxs-lookup"><span data-stu-id="1a44e-510">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="1a44e-511">In the preceding sample, it redirects to the root Index page (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="1a44e-511">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="1a44e-512">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span><span class="sxs-lookup"><span data-stu-id="1a44e-512">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="1a44e-513">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span><span class="sxs-lookup"><span data-stu-id="1a44e-513">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="1a44e-514">`Page` returns an instance of `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-514">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="1a44e-515">Returning `Page` is similar to how actions in controllers return `View`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-515">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="1a44e-516">`PageResult` is the default return type for a handler method.</span><span class="sxs-lookup"><span data-stu-id="1a44e-516">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="1a44e-517">A handler method that returns `void` renders the page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-517">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="1a44e-518">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span><span class="sxs-lookup"><span data-stu-id="1a44e-518">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="1a44e-519">Razor Pages, by default, bind properties only with non-`GET` verbs.</span><span class="sxs-lookup"><span data-stu-id="1a44e-519">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="1a44e-520">Binding to properties can reduce the amount of code you have to write.</span><span class="sxs-lookup"><span data-stu-id="1a44e-520">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="1a44e-521">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span><span class="sxs-lookup"><span data-stu-id="1a44e-521">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="1a44e-522">The home page (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="1a44e-522">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="1a44e-523">The associated `PageModel` class (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1a44e-523">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="1a44e-524">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span><span class="sxs-lookup"><span data-stu-id="1a44e-524">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="1a44e-525">The `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-525">The `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="1a44e-526">The link contains route data with the contact ID.</span><span class="sxs-lookup"><span data-stu-id="1a44e-526">The link contains route data with the contact ID.</span></span> <span data-ttu-id="1a44e-527">For example, `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-527">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="1a44e-528">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) umożliwiają uczestniczenie kodu po stronie serwera w tworzeniu i renderowaniu elementów HTML w plikach Razor.</span><span class="sxs-lookup"><span data-stu-id="1a44e-528">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="1a44e-529">Tag Helpers are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="1a44e-529">Tag Helpers are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="1a44e-530">The *Pages/Edit.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="1a44e-530">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="1a44e-531">The first line contains the `@page "{id:int}"` directive.</span><span class="sxs-lookup"><span data-stu-id="1a44e-531">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="1a44e-532">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span><span class="sxs-lookup"><span data-stu-id="1a44e-532">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="1a44e-533">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span><span class="sxs-lookup"><span data-stu-id="1a44e-533">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="1a44e-534">To make the ID optional, append `?` to the route constraint:</span><span class="sxs-lookup"><span data-stu-id="1a44e-534">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="1a44e-535">The *Pages/Edit.cshtml.cs* file:</span><span class="sxs-lookup"><span data-stu-id="1a44e-535">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="1a44e-536">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span><span class="sxs-lookup"><span data-stu-id="1a44e-536">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="1a44e-537">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span><span class="sxs-lookup"><span data-stu-id="1a44e-537">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="1a44e-538">The customer contact ID specified by the `asp-route-id` attribute.</span><span class="sxs-lookup"><span data-stu-id="1a44e-538">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="1a44e-539">The `handler` specified by the `asp-page-handler` attribute.</span><span class="sxs-lookup"><span data-stu-id="1a44e-539">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="1a44e-540">Here is an example of a rendered delete button with a customer contact ID of `1`:</span><span class="sxs-lookup"><span data-stu-id="1a44e-540">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="1a44e-541">When the button is selected, a form `POST` request is sent to the server.</span><span class="sxs-lookup"><span data-stu-id="1a44e-541">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="1a44e-542">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-542">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="1a44e-543">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span><span class="sxs-lookup"><span data-stu-id="1a44e-543">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="1a44e-544">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span><span class="sxs-lookup"><span data-stu-id="1a44e-544">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span> <span data-ttu-id="1a44e-545">The following code shows the `OnPostDeleteAsync` handler:</span><span class="sxs-lookup"><span data-stu-id="1a44e-545">The following code shows the `OnPostDeleteAsync` handler:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="1a44e-546">The `OnPostDeleteAsync` method:</span><span class="sxs-lookup"><span data-stu-id="1a44e-546">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="1a44e-547">Accepts the `id` from the query string.</span><span class="sxs-lookup"><span data-stu-id="1a44e-547">Accepts the `id` from the query string.</span></span> <span data-ttu-id="1a44e-548">If the *Index.cshtml* page directive contained routing constraint `"{id:int?}"`, `id` would come from route data.</span><span class="sxs-lookup"><span data-stu-id="1a44e-548">If the *Index.cshtml* page directive contained routing constraint `"{id:int?}"`, `id` would come from route data.</span></span> <span data-ttu-id="1a44e-549">The route data for `id` is specified in the URI such as `https://localhost:5001/Customers/2`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-549">The route data for `id` is specified in the URI such as `https://localhost:5001/Customers/2`.</span></span>
* <span data-ttu-id="1a44e-550">Queries the database for the customer contact with `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-550">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="1a44e-551">If the customer contact is found, they're removed from the list of customer contacts.</span><span class="sxs-lookup"><span data-stu-id="1a44e-551">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="1a44e-552">The database is updated.</span><span class="sxs-lookup"><span data-stu-id="1a44e-552">The database is updated.</span></span>
* <span data-ttu-id="1a44e-553">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="1a44e-553">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="1a44e-554">Mark page properties as required</span><span class="sxs-lookup"><span data-stu-id="1a44e-554">Mark page properties as required</span></span>

<span data-ttu-id="1a44e-555">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span><span class="sxs-lookup"><span data-stu-id="1a44e-555">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="1a44e-556">For more information, see [Model validation](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="1a44e-556">For more information, see [Model validation](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="1a44e-557">Handle HEAD requests with an OnGet handler fallback</span><span class="sxs-lookup"><span data-stu-id="1a44e-557">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="1a44e-558">`HEAD` requests allow you to retrieve the headers for a specific resource.</span><span class="sxs-lookup"><span data-stu-id="1a44e-558">`HEAD` requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="1a44e-559">Unlike `GET` requests, `HEAD` requests don't return a response body.</span><span class="sxs-lookup"><span data-stu-id="1a44e-559">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="1a44e-560">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span><span class="sxs-lookup"><span data-stu-id="1a44e-560">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="1a44e-561">In ASP.NET Core 2.1 or later, Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span><span class="sxs-lookup"><span data-stu-id="1a44e-561">In ASP.NET Core 2.1 or later, Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span> <span data-ttu-id="1a44e-562">This behavior is enabled by the call to [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1a44e-562">This behavior is enabled by the call to [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="1a44e-563">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span><span class="sxs-lookup"><span data-stu-id="1a44e-563">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span> <span data-ttu-id="1a44e-564">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-564">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="1a44e-565">Rather than opting in to all behaviors with `SetCompatibilityVersion`, you can explicitly opt in to *specific* behaviors.</span><span class="sxs-lookup"><span data-stu-id="1a44e-565">Rather than opting in to all behaviors with `SetCompatibilityVersion`, you can explicitly opt in to *specific* behaviors.</span></span> <span data-ttu-id="1a44e-566">The following code opts in to allowing `HEAD` requests to be mapped to the `OnGet` handler:</span><span class="sxs-lookup"><span data-stu-id="1a44e-566">The following code opts in to allowing `HEAD` requests to be mapped to the `OnGet` handler:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="1a44e-567">XSRF/CSRF and Razor Pages</span><span class="sxs-lookup"><span data-stu-id="1a44e-567">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="1a44e-568">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="1a44e-568">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="1a44e-569">Antiforgery token generation and validation are automatically included in Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="1a44e-569">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="1a44e-570">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span><span class="sxs-lookup"><span data-stu-id="1a44e-570">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="1a44e-571">Pages work with all the capabilities of the Razor view engine.</span><span class="sxs-lookup"><span data-stu-id="1a44e-571">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="1a44e-572">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span><span class="sxs-lookup"><span data-stu-id="1a44e-572">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="1a44e-573">Let's declutter this page by taking advantage of some of those capabilities.</span><span class="sxs-lookup"><span data-stu-id="1a44e-573">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="1a44e-574">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1a44e-574">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="1a44e-575">The [Layout](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="1a44e-575">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="1a44e-576">Controls the layout of each page (unless the page opts out of layout).</span><span class="sxs-lookup"><span data-stu-id="1a44e-576">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="1a44e-577">Imports HTML structures such as JavaScript and stylesheets.</span><span class="sxs-lookup"><span data-stu-id="1a44e-577">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="1a44e-578">See [layout page](xref:mvc/views/layout) for more information.</span><span class="sxs-lookup"><span data-stu-id="1a44e-578">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="1a44e-579">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1a44e-579">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="1a44e-580">The layout is in the *Pages/Shared* folder.</span><span class="sxs-lookup"><span data-stu-id="1a44e-580">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="1a44e-581">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-581">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="1a44e-582">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span><span class="sxs-lookup"><span data-stu-id="1a44e-582">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="1a44e-583">The layout file should go in the *Pages/Shared* folder.</span><span class="sxs-lookup"><span data-stu-id="1a44e-583">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="1a44e-584">We recommend you **not** put the layout file in the *Views/Shared* folder.</span><span class="sxs-lookup"><span data-stu-id="1a44e-584">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="1a44e-585">*Views/Shared* is an MVC views pattern.</span><span class="sxs-lookup"><span data-stu-id="1a44e-585">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="1a44e-586">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span><span class="sxs-lookup"><span data-stu-id="1a44e-586">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="1a44e-587">View search from a Razor Page includes the *Pages* folder.</span><span class="sxs-lookup"><span data-stu-id="1a44e-587">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="1a44e-588">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span><span class="sxs-lookup"><span data-stu-id="1a44e-588">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="1a44e-589">Add a *Pages/_ViewImports.cshtml* file:</span><span class="sxs-lookup"><span data-stu-id="1a44e-589">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="1a44e-590">`@namespace` is explained later in the tutorial.</span><span class="sxs-lookup"><span data-stu-id="1a44e-590">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="1a44e-591">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span><span class="sxs-lookup"><span data-stu-id="1a44e-591">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="1a44e-592">When the `@namespace` directive is used explicitly on a page:</span><span class="sxs-lookup"><span data-stu-id="1a44e-592">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="1a44e-593">The directive sets the namespace for the page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-593">The directive sets the namespace for the page.</span></span> <span data-ttu-id="1a44e-594">The `@model` directive doesn't need to include the namespace.</span><span class="sxs-lookup"><span data-stu-id="1a44e-594">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="1a44e-595">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span><span class="sxs-lookup"><span data-stu-id="1a44e-595">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="1a44e-596">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-596">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="1a44e-597">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span><span class="sxs-lookup"><span data-stu-id="1a44e-597">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="1a44e-598">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span><span class="sxs-lookup"><span data-stu-id="1a44e-598">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="1a44e-599">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span><span class="sxs-lookup"><span data-stu-id="1a44e-599">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="1a44e-600">`@namespace` *also works with conventional Razor views.*</span><span class="sxs-lookup"><span data-stu-id="1a44e-600">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="1a44e-601">The original *Pages/Create.cshtml* view file:</span><span class="sxs-lookup"><span data-stu-id="1a44e-601">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="1a44e-602">The updated *Pages/Create.cshtml* view file:</span><span class="sxs-lookup"><span data-stu-id="1a44e-602">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="1a44e-603">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span><span class="sxs-lookup"><span data-stu-id="1a44e-603">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="1a44e-604">For more information on partial views, see <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="1a44e-604">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="1a44e-605">URL generation for Pages</span><span class="sxs-lookup"><span data-stu-id="1a44e-605">URL generation for Pages</span></span>

<span data-ttu-id="1a44e-606">The `Create` page, shown previously, uses `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="1a44e-606">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="1a44e-607">The app has the following file/folder structure:</span><span class="sxs-lookup"><span data-stu-id="1a44e-607">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="1a44e-608">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="1a44e-608">*/Pages*</span></span>

  * <span data-ttu-id="1a44e-609">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1a44e-609">*Index.cshtml*</span></span>
  * <span data-ttu-id="1a44e-610">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="1a44e-610">*/Customers*</span></span>

    * <span data-ttu-id="1a44e-611">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1a44e-611">*Create.cshtml*</span></span>
    * <span data-ttu-id="1a44e-612">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1a44e-612">*Edit.cshtml*</span></span>
    * <span data-ttu-id="1a44e-613">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="1a44e-613">*Index.cshtml*</span></span>

<span data-ttu-id="1a44e-614">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span><span class="sxs-lookup"><span data-stu-id="1a44e-614">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="1a44e-615">The string `/Index` is part of the URI to access the preceding page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-615">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="1a44e-616">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-616">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="1a44e-617">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="1a44e-617">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="1a44e-618">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span><span class="sxs-lookup"><span data-stu-id="1a44e-618">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="1a44e-619">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span><span class="sxs-lookup"><span data-stu-id="1a44e-619">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="1a44e-620">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span><span class="sxs-lookup"><span data-stu-id="1a44e-620">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="1a44e-621">URL generation for pages supports relative names.</span><span class="sxs-lookup"><span data-stu-id="1a44e-621">URL generation for pages supports relative names.</span></span> <span data-ttu-id="1a44e-622">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="1a44e-622">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="1a44e-623">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="1a44e-623">RedirectToPage(x)</span></span>| <span data-ttu-id="1a44e-624">Page</span><span class="sxs-lookup"><span data-stu-id="1a44e-624">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="1a44e-625">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="1a44e-625">RedirectToPage("/Index")</span></span> | <span data-ttu-id="1a44e-626">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="1a44e-626">*Pages/Index*</span></span> |
| <span data-ttu-id="1a44e-627">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="1a44e-627">RedirectToPage("./Index");</span></span> | <span data-ttu-id="1a44e-628">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="1a44e-628">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="1a44e-629">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="1a44e-629">RedirectToPage("../Index")</span></span> | <span data-ttu-id="1a44e-630">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="1a44e-630">*Pages/Index*</span></span> |
| <span data-ttu-id="1a44e-631">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="1a44e-631">RedirectToPage("Index")</span></span>  | <span data-ttu-id="1a44e-632">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="1a44e-632">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="1a44e-633">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span><span class="sxs-lookup"><span data-stu-id="1a44e-633">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="1a44e-634">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-634">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="1a44e-635">Relative name linking is useful when building sites with a complex structure.</span><span class="sxs-lookup"><span data-stu-id="1a44e-635">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="1a44e-636">If you use relative names to link between pages in a folder, you can rename that folder.</span><span class="sxs-lookup"><span data-stu-id="1a44e-636">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="1a44e-637">All the links still work (because they didn't include the folder name).</span><span class="sxs-lookup"><span data-stu-id="1a44e-637">All the links still work (because they didn't include the folder name).</span></span>

<span data-ttu-id="1a44e-638">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span><span class="sxs-lookup"><span data-stu-id="1a44e-638">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="1a44e-639">Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="1a44e-639">For more information, see <xref:mvc/controllers/areas>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="1a44e-640">ViewData attribute</span><span class="sxs-lookup"><span data-stu-id="1a44e-640">ViewData attribute</span></span>

<span data-ttu-id="1a44e-641">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="1a44e-641">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="1a44e-642">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="1a44e-642">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="1a44e-643">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-643">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="1a44e-644">The `Title` property is set to the title of the About page:</span><span class="sxs-lookup"><span data-stu-id="1a44e-644">The `Title` property is set to the title of the About page:</span></span>

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

<span data-ttu-id="1a44e-645">In the About page, access the `Title` property as a model property:</span><span class="sxs-lookup"><span data-stu-id="1a44e-645">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="1a44e-646">In the layout, the title is read from the ViewData dictionary:</span><span class="sxs-lookup"><span data-stu-id="1a44e-646">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="1a44e-647">TempData</span><span class="sxs-lookup"><span data-stu-id="1a44e-647">TempData</span></span>

<span data-ttu-id="1a44e-648">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="1a44e-648">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="1a44e-649">This property stores data until it's read.</span><span class="sxs-lookup"><span data-stu-id="1a44e-649">This property stores data until it's read.</span></span> <span data-ttu-id="1a44e-650">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span><span class="sxs-lookup"><span data-stu-id="1a44e-650">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="1a44e-651">`TempData` is  useful for redirection, when data is needed for more than a single request.</span><span class="sxs-lookup"><span data-stu-id="1a44e-651">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="1a44e-652">The following code sets the value of `Message` using `TempData`:</span><span class="sxs-lookup"><span data-stu-id="1a44e-652">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="1a44e-653">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-653">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="1a44e-654">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span><span class="sxs-lookup"><span data-stu-id="1a44e-654">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="1a44e-655">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span><span class="sxs-lookup"><span data-stu-id="1a44e-655">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="1a44e-656">Multiple handlers per page</span><span class="sxs-lookup"><span data-stu-id="1a44e-656">Multiple handlers per page</span></span>

<span data-ttu-id="1a44e-657">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span><span class="sxs-lookup"><span data-stu-id="1a44e-657">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="1a44e-658">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span><span class="sxs-lookup"><span data-stu-id="1a44e-658">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="1a44e-659">The `asp-page-handler` attribute is a companion to `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-659">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="1a44e-660">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-660">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="1a44e-661">`asp-page` isn't specified because the sample is linking to the current page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-661">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="1a44e-662">The page model:</span><span class="sxs-lookup"><span data-stu-id="1a44e-662">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="1a44e-663">The preceding code uses *named handler methods*.</span><span class="sxs-lookup"><span data-stu-id="1a44e-663">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="1a44e-664">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span><span class="sxs-lookup"><span data-stu-id="1a44e-664">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="1a44e-665">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="1a44e-665">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="1a44e-666">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-666">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="1a44e-667">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-667">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="1a44e-668">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-668">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="1a44e-669">Custom routes</span><span class="sxs-lookup"><span data-stu-id="1a44e-669">Custom routes</span></span>

<span data-ttu-id="1a44e-670">Use the `@page` directive to:</span><span class="sxs-lookup"><span data-stu-id="1a44e-670">Use the `@page` directive to:</span></span>

* <span data-ttu-id="1a44e-671">Specify a custom route to a page.</span><span class="sxs-lookup"><span data-stu-id="1a44e-671">Specify a custom route to a page.</span></span> <span data-ttu-id="1a44e-672">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-672">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="1a44e-673">Append segments to a page's default route.</span><span class="sxs-lookup"><span data-stu-id="1a44e-673">Append segments to a page's default route.</span></span> <span data-ttu-id="1a44e-674">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-674">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="1a44e-675">Append parameters to a page's default route.</span><span class="sxs-lookup"><span data-stu-id="1a44e-675">Append parameters to a page's default route.</span></span> <span data-ttu-id="1a44e-676">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-676">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="1a44e-677">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span><span class="sxs-lookup"><span data-stu-id="1a44e-677">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="1a44e-678">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-678">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="1a44e-679">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-679">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="1a44e-680">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span><span class="sxs-lookup"><span data-stu-id="1a44e-680">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="1a44e-681">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span><span class="sxs-lookup"><span data-stu-id="1a44e-681">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="1a44e-682">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-682">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="1a44e-683">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="1a44e-683">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="1a44e-684">The `?` following `handler` means the route parameter is optional.</span><span class="sxs-lookup"><span data-stu-id="1a44e-684">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="1a44e-685">Configuration and settings</span><span class="sxs-lookup"><span data-stu-id="1a44e-685">Configuration and settings</span></span>

<span data-ttu-id="1a44e-686">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span><span class="sxs-lookup"><span data-stu-id="1a44e-686">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="1a44e-687">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span><span class="sxs-lookup"><span data-stu-id="1a44e-687">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="1a44e-688">We'll enable more extensibility this way in the future.</span><span class="sxs-lookup"><span data-stu-id="1a44e-688">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="1a44e-689">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="1a44e-689">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="1a44e-690">[Download or view sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="1a44e-690">[Download or view sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="1a44e-691">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span><span class="sxs-lookup"><span data-stu-id="1a44e-691">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="1a44e-692">Specify that Razor Pages are at the content root</span><span class="sxs-lookup"><span data-stu-id="1a44e-692">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="1a44e-693">By default, Razor Pages are rooted in the */Pages* directory.</span><span class="sxs-lookup"><span data-stu-id="1a44e-693">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="1a44e-694">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span><span class="sxs-lookup"><span data-stu-id="1a44e-694">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="1a44e-695">Specify that Razor Pages are at a custom root directory</span><span class="sxs-lookup"><span data-stu-id="1a44e-695">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="1a44e-696">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span><span class="sxs-lookup"><span data-stu-id="1a44e-696">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="1a44e-697">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="1a44e-697">Additional resources</span></span>

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end
