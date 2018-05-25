---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Informacje o wersji dla platformy ASP.NET i narzędzia sieci Web 2013.1 dla programu Visual Studio 2012 | Dokumentacja firmy Microsoft
author: microsoft
description: Ten dokument zawiera opis wersji platformy ASP.NET i 2013.1 narzędzia sieci Web dla programu Visual Studio 2012.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: c11e2ef9c33b0cae1f196690533094ce1c342da5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="f6912-103">Informacje o wersji dla platformy ASP.NET i narzędzia sieci Web 2013.1 dla programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="f6912-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>
====================
<span data-ttu-id="f6912-104">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f6912-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f6912-105">Ten dokument zawiera opis wersji platformy ASP.NET i 2013.1 narzędzia sieci Web dla programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="f6912-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>


## <a name="contents"></a><span data-ttu-id="f6912-106">Spis treści</span><span class="sxs-lookup"><span data-stu-id="f6912-106">Contents</span></span>

- [<span data-ttu-id="f6912-107">Informacje o instalacji</span><span class="sxs-lookup"><span data-stu-id="f6912-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="f6912-108">Wymagania dotyczące oprogramowania</span><span class="sxs-lookup"><span data-stu-id="f6912-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="f6912-109">Nowe funkcje programu ASP.NET i narzędzia sieci Web 2013.1 dla programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="f6912-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="f6912-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="f6912-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="f6912-111">Szablony</span><span class="sxs-lookup"><span data-stu-id="f6912-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="f6912-112">Szablonu platformy ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="f6912-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="f6912-113">Szablon 2 interfejsu API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f6912-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="f6912-114">Szablony elementu</span><span class="sxs-lookup"><span data-stu-id="f6912-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="f6912-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="f6912-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="f6912-116">ASP.NET Scaffolding</span><span class="sxs-lookup"><span data-stu-id="f6912-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="f6912-117">Razor Editor</span><span class="sxs-lookup"><span data-stu-id="f6912-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="f6912-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="f6912-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="f6912-119">Znane problemy i fundamentalne zmiany</span><span class="sxs-lookup"><span data-stu-id="f6912-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="f6912-120">ASP.NET Scaffolding</span><span class="sxs-lookup"><span data-stu-id="f6912-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="f6912-121">MVC i Web API szkieletów - HTTP 404 błędu nie znaleziono</span><span class="sxs-lookup"><span data-stu-id="f6912-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="f6912-122">Visual Studio Express 2012 for Web przestaje działać po Dodawanie elementu szkieletu</span><span class="sxs-lookup"><span data-stu-id="f6912-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="f6912-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="f6912-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="f6912-124">Wyświetlanie pliku cshtml przeglądanie za pomocą lub F5 powoduje błąd serwera</span><span class="sxs-lookup"><span data-stu-id="f6912-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="f6912-125">Ponowne zapisywanie adresów URL i Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="f6912-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="f6912-126">Szablony</span><span class="sxs-lookup"><span data-stu-id="f6912-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="f6912-127">Informacje o instalacji</span><span class="sxs-lookup"><span data-stu-id="f6912-127">Installation Notes</span></span>

<span data-ttu-id="f6912-128">[Zainstaluj](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET i sieć Web narzędzi 2013.1 dla programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="f6912-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="f6912-129">Wymagania programowe</span><span class="sxs-lookup"><span data-stu-id="f6912-129">Software Requirements</span></span>

<span data-ttu-id="f6912-130">Musi mieć programu Visual Studio 2012 lub Visual Studio Express 2012 for Web.</span><span class="sxs-lookup"><span data-stu-id="f6912-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="f6912-131">Nowe funkcje programu ASP.NET i narzędzia sieci Web 2013.1 dla programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="f6912-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="f6912-132">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="f6912-132">Bootstrap</span></span>

<span data-ttu-id="f6912-133">Po utworzeniu szkieletu kontrolerów MVC 5 z widokami, korzysta z kodu znaczników dla widoków [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="f6912-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="f6912-134">Szablony</span><span class="sxs-lookup"><span data-stu-id="f6912-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="f6912-135">Szablonu platformy ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="f6912-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="f6912-136">Dodano nowy szablon MVC 5.</span><span class="sxs-lookup"><span data-stu-id="f6912-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="f6912-137">Odwołuje się do najnowszych pakietów MVC 5 NuGet i można dodać kontrolery i widoki szkieletów.</span><span class="sxs-lookup"><span data-stu-id="f6912-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="f6912-138">Szablon 2 interfejsu API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f6912-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="f6912-139">Dodano nowy szablon 2 interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f6912-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="f6912-140">Odwołuje się do najnowszych pakietów NuGet 2 interfejsu API sieci Web i można dodać kontrolery i widoki szkieletów.</span><span class="sxs-lookup"><span data-stu-id="f6912-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="f6912-141">Szablony elementu</span><span class="sxs-lookup"><span data-stu-id="f6912-141">Item Templates</span></span>

<span data-ttu-id="f6912-142">Dodaliśmy nowe szablony elementów dla widoków MVC 5, stron sieci Web (Razor 3) i sieci Web API 2 kontrolerów.</span><span class="sxs-lookup"><span data-stu-id="f6912-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="f6912-143">Instalacji powiązanych pakietów NuGet do projektu podczas dodawania nowych elementów.</span><span class="sxs-lookup"><span data-stu-id="f6912-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="f6912-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="f6912-144">Entity Framework 6</span></span>

<span data-ttu-id="f6912-145">Po utworzeniu szkieletu kontrolera MVC lub Web API przy użyciu programu Entity Framework, używamy Framework 6.</span><span class="sxs-lookup"><span data-stu-id="f6912-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="f6912-146">Aby uzyskać więcej informacji na temat programu Entity Framework, zobacz [Historia wersji programu Entity Framework](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="f6912-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="f6912-147">Można również pobrać i zainstalować narzędzia Entity Framework 6 dla programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="f6912-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="f6912-148">Zobacz [uzyskać programu Entity Framework](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="f6912-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="f6912-149">ASP.NET Scaffolding</span><span class="sxs-lookup"><span data-stu-id="f6912-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="f6912-150">Rusztowania ASP.NET to platforma generowania kodu dla aplikacji sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f6912-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="f6912-151">Ułatwia on dodać schematyczny kod służący do projektu, który współdziała z modelem danych.</span><span class="sxs-lookup"><span data-stu-id="f6912-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="f6912-152">W poprzednich wersjach programu Visual Studio szkieletów została ograniczona do projektów platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f6912-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="f6912-153">Dzięki tej aktualizacji można teraz używać szkieletów dla żadnego projektu ASP.NET, w tym formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="f6912-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="f6912-154">Ta aktualizacja nie obsługuje generowania stron dla projektu formularzy sieci Web, ale można nadal używać szkieletów z formularzy sieci Web, dodając zależności MVC do projektu.</span><span class="sxs-lookup"><span data-stu-id="f6912-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="f6912-155">Obsługa generowania strony formularzy sieci Web zostanie dodana w przyszłej aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="f6912-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="f6912-156">Jeśli przy użyciu funkcji szkieletów, Upewniamy się, że wszystkie wymagane zależności są zainstalowane w projekcie.</span><span class="sxs-lookup"><span data-stu-id="f6912-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="f6912-157">Na przykład uruchomienie z projektem formularzy sieci Web ASP.NET i następnie dodać Kontroler interfejsu API sieci Web za pomocą funkcja szkieletów, wymagane pakiety NuGet i odwołania są automatycznie dodawane do projektu.</span><span class="sxs-lookup"><span data-stu-id="f6912-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="f6912-158">Aby dodać szkieletów MVC do projektu formularzy sieci Web, Dodaj **nowy element szkieletu** i wybierz **zależności MVC 5** w oknie dialogowym.</span><span class="sxs-lookup"><span data-stu-id="f6912-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="f6912-159">Dostępne są dwie opcje do tworzenia szkieletu MVC; Minimalne i pełne.</span><span class="sxs-lookup"><span data-stu-id="f6912-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="f6912-160">W przypadku wybrania minimalny, tylko pakiety NuGet i odwołań dla platformy ASP.NET MVC zostaną dodane do projektu.</span><span class="sxs-lookup"><span data-stu-id="f6912-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="f6912-161">Jeśli wybierzesz opcję pełne, minimalnym zależności zostaną dodane, a także wymagane pliki zawartości projektu MVC.</span><span class="sxs-lookup"><span data-stu-id="f6912-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="f6912-162">Obsługę tworzenia szkieletu kontrolerów async korzysta z nowych funkcji asynchronicznych z programu Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="f6912-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="f6912-163">Aby uzyskać więcej informacji i samouczki, zobacz [omówienie szkieletów ASP.NET](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f6912-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="f6912-164">Te samouczki przedstawiają szkieletów z programu Visual Studio 2013, ale dotyczą również programu ASP.NET i 2013.1 narzędzia sieci Web dla programu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="f6912-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="f6912-165">Razor Editor</span><span class="sxs-lookup"><span data-stu-id="f6912-165">Razor Editor</span></span>

<span data-ttu-id="f6912-166">Dzięki tej aktualizacji programu Visual Studio 2012 obsługują teraz, narzędzi/edytowania Razor 3.</span><span class="sxs-lookup"><span data-stu-id="f6912-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="f6912-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="f6912-167">NuGet 2.7</span></span>

<span data-ttu-id="f6912-168">NuGet 2.7 zawiera bogaty zestaw nowych funkcji, które opisano szczegółowo w [informacje o wersji 2.7 NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="f6912-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="f6912-169">Ta wersja programu NuGet eliminuje to potrzebę dla użytkowników jawnie zezwolić na NuGet, aby przywrócić brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="f6912-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="f6912-170">Podczas instalowania NuGet 2.7, użytkownicy niejawnie wyrażenia zgody na automatyczne przywracanie brakujących pakietów.</span><span class="sxs-lookup"><span data-stu-id="f6912-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="f6912-171">Użytkownicy jawnie można zrezygnować z przywracania pakietu za pomocą ustawień NuGet w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f6912-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="f6912-172">Ta zmiana upraszcza sposób działania przywracania pakietu.</span><span class="sxs-lookup"><span data-stu-id="f6912-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="f6912-173">Znane problemy i fundamentalne zmiany</span><span class="sxs-lookup"><span data-stu-id="f6912-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="f6912-174">ASP.NET Scaffolding</span><span class="sxs-lookup"><span data-stu-id="f6912-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="f6912-175">MVC i Web API szkieletów - HTTP 404 błędu nie znaleziono</span><span class="sxs-lookup"><span data-stu-id="f6912-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="f6912-176">Jeśli wystąpi błąd podczas Dodawanie elementu szkieletu do projektu, jest to możliwe, projekt zostanie pozostawiony w stanie niespójnym.</span><span class="sxs-lookup"><span data-stu-id="f6912-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="f6912-177">Niektóre zmiany wprowadzone można szkieletów zostanie wycofana, ale inne zmiany, takie jak zainstalowane pakiety NuGet zostaną nie można wycofać.</span><span class="sxs-lookup"><span data-stu-id="f6912-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="f6912-178">Jeśli routingu zmiany konfiguracji zostaną przywrócone, użytkownicy będą otrzymywać błąd HTTP 404 podczas nawigowania do szkieletu elementów.</span><span class="sxs-lookup"><span data-stu-id="f6912-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="f6912-179">Aby naprawić ten błąd dla platformy MVC, Dodaj nowy element szkieletu i wybierz zależności MVC 5 (minimalnie lub pełna).</span><span class="sxs-lookup"><span data-stu-id="f6912-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="f6912-180">Ten proces, zostaną dodane wszystkie wymagane zmiany do projektu.</span><span class="sxs-lookup"><span data-stu-id="f6912-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="f6912-181">Aby naprawić ten błąd interfejsu API sieci Web:</span><span class="sxs-lookup"><span data-stu-id="f6912-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="f6912-182">Dodaj następujące klasy WebApiConfig do projektu.</span><span class="sxs-lookup"><span data-stu-id="f6912-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="f6912-183">Skonfiguruj WebApiConfig.Register w aplikacji\_Start — metoda w pliku Global.asax w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="f6912-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="f6912-184">Visual Studio Express 2012 for Web przestaje działać po Dodawanie elementu szkieletu</span><span class="sxs-lookup"><span data-stu-id="f6912-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="f6912-185">Jeśli program Visual Studio Express 2012 for Web przestaje działać po Dodawanie elementu szkieletu z programu Entity Framework (na przykład sieci Web Kontroler interfejsu API 2 z akcjami używający narzędzia Entity Framework), może się zdarzyć, że Visual Studio Express nie można załadować obrazu macierzystego zestawu w zależności od System.Web.Extensions.</span><span class="sxs-lookup"><span data-stu-id="f6912-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="f6912-186">Aby rozwiązać ten problem, skonfiguruj Visual Studio Express do pracy z obrazu MSIL System.Web.Extensions:</span><span class="sxs-lookup"><span data-stu-id="f6912-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="f6912-187">Otwórz wiersz polecenia w trybie administratora.</span><span class="sxs-lookup"><span data-stu-id="f6912-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="f6912-188">Przejdź do %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE lub % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (dla 64-bitowym systemem Windows).</span><span class="sxs-lookup"><span data-stu-id="f6912-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="f6912-189">Otwórz VWDExpress.exe.config w edytorze tekstów.</span><span class="sxs-lookup"><span data-stu-id="f6912-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="f6912-190">Dodaj następujący wiersz w obszarze &lt;konfiguracji&gt;/&lt;środowiska uruchomieniowego&gt; elementu:</span><span class="sxs-lookup"><span data-stu-id="f6912-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="f6912-191">Uruchom ponownie program Visual Studio Express 2012 for Web.</span><span class="sxs-lookup"><span data-stu-id="f6912-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="f6912-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="f6912-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a><span data-ttu-id="f6912-193">Wyświetlanie withBrowse plik cshtml WithorF5causes błąd serwera</span><span class="sxs-lookup"><span data-stu-id="f6912-193">Viewing cshtml file withBrowse WithorF5causes a server error</span></span>

<span data-ttu-id="f6912-194">Podczas tworzenia projektu MVC 5 w Visual Studio 2012 (lub Otwórz w programie Visual Studio 2012 MVC 5 projektu, który został utworzony w programie Visual Studio 2013) i próbujesz wyświetlić przy użyciu przeglądanie za pomocą lub F5 plik cshtml, zostanie wyświetlony błąd informujący - **błąd serwera w Aplikacja '/'**.</span><span class="sxs-lookup"><span data-stu-id="f6912-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="f6912-195">Próbuje przejdź do serwera`http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="f6912-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="f6912-196">Aby rozwiązać ten problem, zmień **Akcja uruchamiania** ustawienie w swoim projekcie **określonej strony**.</span><span class="sxs-lookup"><span data-stu-id="f6912-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="f6912-197">Nie trzeba podać wartość dla strony.</span><span class="sxs-lookup"><span data-stu-id="f6912-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="f6912-198">Po wprowadzeniu tej zmiany, wybierając F5 przechodzi do katalogu głównego aplikacji (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="f6912-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="f6912-199">To zachowanie nie jest tak samo, jak dla projektów MVC 5 w programie Visual Studio 2013, gdzie **bieżącej strony** ustawienie uruchamia Otwórz stronę.</span><span class="sxs-lookup"><span data-stu-id="f6912-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="f6912-200">Ponowne zapisywanie adresów URL i Tilde(~)</span><span class="sxs-lookup"><span data-stu-id="f6912-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="f6912-201">Po uaktualnieniu do programu ASP.NET Razor 3 i ASP.NET MVC 5 notacji tilde(~) może już nie działają prawidłowo, jeśli używasz ponownego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="f6912-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="f6912-202">Ponowne zapisywanie adresów URL wpływa notacji tilde(~) w elementów HTML, takich jak &lt;A /&gt;, &lt;skryptu /&gt;, &lt;łącze /&gt;, i w związku z tym tylda nie jest już mapowana do katalogu głównego.</span><span class="sxs-lookup"><span data-stu-id="f6912-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="f6912-203">Na przykład, jeśli przepisywania żądania **asp.net/content** do **asp.net**, atrybutu href w &lt;A href = "~/content/" /&gt; jest rozpoznawana jako **/content/ zawartość /** zamiast **/**.</span><span class="sxs-lookup"><span data-stu-id="f6912-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="f6912-204">Aby pominąć tę zmianę, można ustawić **IIS\_WasUrlRewritten** kontekstu false w każdej strony sieci Web lub w **aplikacji\_powstaniem zdarzenia BeginRequest** w pliku Global.asax.</span><span class="sxs-lookup"><span data-stu-id="f6912-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="f6912-205">Szablony</span><span class="sxs-lookup"><span data-stu-id="f6912-205">Templates</span></span>

<span data-ttu-id="f6912-206">Po utworzeniu ASP.NET MVC projektów programu Visual Studio 2012, Windows 8.1 lub Windows Server 2012 R2, programu Visual Studio z komunikatem o błędzie stwierdzający "Konfigurowanie sieci Web [url] dla platformy ASP.NET 4.5 nie powiodło się."</span><span class="sxs-lookup"><span data-stu-id="f6912-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![Błąd konfiguracji](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="f6912-208">Zostanie wyświetlony ten błąd, ponieważ program Visual Studio 2012 nie obsługuje funkcję ASP.NET 4.5 zainstalowanego w tych wersjach systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="f6912-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="f6912-209">Aby włączyć funkcję ASP.NET 4.5, wykonaj czynności opisane w [Włącz lub wyłącz funkcje systemu Windows](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span><span class="sxs-lookup"><span data-stu-id="f6912-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![Włącz lub wyłącz funkcje systemu Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="f6912-211">Alternatywnie można włączyć funkcję ASP.NET 4.5 za pomocą wiersza polecenia.</span><span class="sxs-lookup"><span data-stu-id="f6912-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="f6912-212">Otwórz wiersz polecenia w trybie administratora.</span><span class="sxs-lookup"><span data-stu-id="f6912-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="f6912-213">Uruchom następujące polecenie, aby włączyć funkcję ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="f6912-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
