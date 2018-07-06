---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Tworzenie stron pomocy dla interfejsu API sieci Web programu ASP.NET | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 04/01/2013
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 3e8a0e6c4555e2b8d880118ca499b801ac8be63e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811947"
---
<a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="a822b-102">Tworzenie stron pomocy dla interfejsu API sieci Web platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a822b-102">Creating Help Pages for ASP.NET Web API</span></span>
====================
<span data-ttu-id="a822b-103">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a822b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a822b-104">Podczas tworzenia interfejsu API sieci web, często jest przydatne utworzyć stronę pomocy, aby inni deweloperzy powinni wiedzieć, jak wywołać interfejs API.</span><span class="sxs-lookup"><span data-stu-id="a822b-104">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="a822b-105">Można utworzyć całą dokumentację ręcznie, ale zaleca się automatyczne generowanie możliwie.</span><span class="sxs-lookup"><span data-stu-id="a822b-105">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span>

<span data-ttu-id="a822b-106">Aby ułatwić to zadanie, Web API platformy ASP.NET udostępnia bibliotekę do automatycznego wygenerowania stron pomocy w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="a822b-106">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="a822b-107">Tworzenie stron pomocy interfejsu API</span><span class="sxs-lookup"><span data-stu-id="a822b-107">Creating API Help Pages</span></span>

<span data-ttu-id="a822b-108">Zainstaluj [ASP.NET and Web Tools 2012.2 aktualizacji](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="a822b-108">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="a822b-109">Ta aktualizacja integruje stron pomocy szablonu projektu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a822b-109">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="a822b-110">Następnie utwórz nowy projekt ASP.NET MVC 4 i wybierz szablon projektu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="a822b-110">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="a822b-111">Szablon projektu umożliwia utworzenie kontrolera przykładowy interfejs API o nazwie `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="a822b-111">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="a822b-112">Ten szablon tworzy również stron pomocy interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="a822b-112">The template also creates the API help pages.</span></span> <span data-ttu-id="a822b-113">Wszystkie pliki kodu strony pomocy są umieszczane w folderze obszarów projektu.</span><span class="sxs-lookup"><span data-stu-id="a822b-113">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="a822b-114">Po uruchomieniu aplikacji, na stronie głównej zawiera łącze do strony pomocy interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="a822b-114">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="a822b-115">Na stronie głównej ścieżka względna jest/Help.</span><span class="sxs-lookup"><span data-stu-id="a822b-115">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="a822b-116">Ten link łączy do strony Podsumowanie interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="a822b-116">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="a822b-117">Widok MVC ta strona jest zdefiniowany w Areas/HelpPage/Views/Help/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="a822b-117">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="a822b-118">Możesz edytować tę stronę, aby modyfikować układ, wprowadzenie, tytuł, style i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="a822b-118">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="a822b-119">Główna część strony znajduje się tabela interfejsów API, pogrupowane według kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a822b-119">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="a822b-120">Wpisy tabeli są generowane dynamicznie przy użyciu **IApiExplorer** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="a822b-120">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="a822b-121">(Czy powiemy więcej informacji na temat tego interfejsu później.) Jeśli dodasz nowy kontroler interfejsu API, tabeli jest automatycznie aktualizowana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="a822b-121">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="a822b-122">Kolumna "Interfejs API" wymieniono metody HTTP i względnym identyfikatorem URI.</span><span class="sxs-lookup"><span data-stu-id="a822b-122">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="a822b-123">Kolumna "Opis" zawiera dokumentacja dla każdego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="a822b-123">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="a822b-124">Dokumentacja jest początkowo tylko tekst symbolu zastępczego.</span><span class="sxs-lookup"><span data-stu-id="a822b-124">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="a822b-125">W następnej sekcji I opisano sposób dodawania dokumentacji z komentarzy XML.</span><span class="sxs-lookup"><span data-stu-id="a822b-125">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="a822b-126">Każdy interfejs API zawiera link do strony zawierającej więcej szczegółowych informacji, w tym przykładzie treści żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="a822b-126">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="a822b-127">Dodawanie stron pomocy do istniejącego projektu</span><span class="sxs-lookup"><span data-stu-id="a822b-127">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="a822b-128">Strony pomocy można dodać do istniejącego projektu interfejsu API sieci Web przy użyciu Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="a822b-128">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="a822b-129">Ta opcja jest przydatna, Rozpocznij od szablonu projektu innego niż szablon "Interfejs API sieci Web".</span><span class="sxs-lookup"><span data-stu-id="a822b-129">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="a822b-130">Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="a822b-130">From the **Tools** menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="a822b-131">W [Konsola Menedżera pakietów](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) okna, wpisz jedno z następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="a822b-131">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="a822b-132">Aby uzyskać **C#** aplikacji: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="a822b-132">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="a822b-133">Aby uzyskać **języka Visual Basic** aplikacji: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="a822b-133">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="a822b-134">Istnieją dwa pakiety, jeden dla języka C# i jeden dla języka Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="a822b-134">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="a822b-135">Upewnij się, że używasz zgodną z projektu.</span><span class="sxs-lookup"><span data-stu-id="a822b-135">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="a822b-136">To polecenie instaluje konieczne zestawy, a także dodaje widoków MVC dla stron pomocy (znajdujący się w folderze obszarów/HelpPage).</span><span class="sxs-lookup"><span data-stu-id="a822b-136">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="a822b-137">Należy ręcznie dodać link do strony pomocy.</span><span class="sxs-lookup"><span data-stu-id="a822b-137">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="a822b-138">Identyfikator URI jest/Help.</span><span class="sxs-lookup"><span data-stu-id="a822b-138">The URI is /Help.</span></span> <span data-ttu-id="a822b-139">Aby utworzyć link w widoku razor, Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="a822b-139">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="a822b-140">Upewnij się również zarejestrować obszarów.</span><span class="sxs-lookup"><span data-stu-id="a822b-140">Also, make sure to register areas.</span></span> <span data-ttu-id="a822b-141">W pliku Global.asax, Dodaj następujący kod do **aplikacji\_Start** metody, jeśli go nie zostało to zrobione:</span><span class="sxs-lookup"><span data-stu-id="a822b-141">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="a822b-142">Dodawanie dokumentacji interfejsu API</span><span class="sxs-lookup"><span data-stu-id="a822b-142">Adding API Documentation</span></span>

<span data-ttu-id="a822b-143">Domyślnie pomocy strony zawierają symbol zastępczy parametrów dla dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="a822b-143">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="a822b-144">Możesz użyć [komentarze dokumentacji XML](https://msdn.microsoft.com/library/b2s063f7.aspx) do utworzenia w dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="a822b-144">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="a822b-145">Aby włączyć tę funkcję, otwórz plik obszarów/HelpPage/aplikacji\_Start/HelpPageConfig.cs i usuń znaczniki komentarza następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="a822b-145">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="a822b-146">Teraz Włącz dokumentacji XML.</span><span class="sxs-lookup"><span data-stu-id="a822b-146">Now enable XML documentation.</span></span> <span data-ttu-id="a822b-147">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="a822b-147">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="a822b-148">Wybierz **kompilacji** strony.</span><span class="sxs-lookup"><span data-stu-id="a822b-148">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="a822b-149">W obszarze **dane wyjściowe**, sprawdź **pliku dokumentacji XML**.</span><span class="sxs-lookup"><span data-stu-id="a822b-149">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="a822b-150">W polu edycji, wpisz "aplikacji\_Data/XmlDocument.xml".</span><span class="sxs-lookup"><span data-stu-id="a822b-150">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="a822b-151">Następnie otwórz kod `ValuesController` Kontroler interfejsu API, która jest zdefiniowana w /Controllers/ValuesControler.cs.</span><span class="sxs-lookup"><span data-stu-id="a822b-151">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesControler.cs.</span></span> <span data-ttu-id="a822b-152">Dodaj niektóre komentarze dokumentacji do metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a822b-152">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="a822b-153">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a822b-153">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="a822b-154">Porada: Jeśli położenie karetki na wiersz powyżej metody i typu trzy ukośniki, program Visual Studio automatycznie wstawia elementy XML.</span><span class="sxs-lookup"><span data-stu-id="a822b-154">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="a822b-155">Następnie można wypełnić puste wartości.</span><span class="sxs-lookup"><span data-stu-id="a822b-155">Then you can fill in the blanks.</span></span>


<span data-ttu-id="a822b-156">Teraz tworzenie i uruchomić ponownie aplikację i przejdź do strony pomocy.</span><span class="sxs-lookup"><span data-stu-id="a822b-156">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="a822b-157">Ciągi dokumentacji powinna pojawić się w tabeli interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="a822b-157">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="a822b-158">Strona pomocy odczytuje ciągi z pliku XML w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="a822b-158">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="a822b-159">(Podczas wdrażania aplikacji, upewnij się wdrożyć plik XML.)</span><span class="sxs-lookup"><span data-stu-id="a822b-159">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="a822b-160">Kulisy</span><span class="sxs-lookup"><span data-stu-id="a822b-160">Under the Hood</span></span>

<span data-ttu-id="a822b-161">Strony pomocy są wbudowane w górnej części **ApiExplorer** klasy, która jest częścią struktury Web API.</span><span class="sxs-lookup"><span data-stu-id="a822b-161">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="a822b-162">**ApiExplorer** klasy zawiera surowce do tworzenia strony pomocy.</span><span class="sxs-lookup"><span data-stu-id="a822b-162">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="a822b-163">Dla każdego interfejsu API **ApiExplorer** zawiera **ApiDescription** , który opisuje interfejs API.</span><span class="sxs-lookup"><span data-stu-id="a822b-163">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="a822b-164">W tym celu "API" jest zdefiniowany jako kombinacja metody HTTP i względnym identyfikatorem URI.</span><span class="sxs-lookup"><span data-stu-id="a822b-164">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="a822b-165">Na przykład poniżej przedstawiono niektóre różne interfejsy API:</span><span class="sxs-lookup"><span data-stu-id="a822b-165">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="a822b-166">Pobierz /api/Products</span><span class="sxs-lookup"><span data-stu-id="a822b-166">GET /api/Products</span></span>
- <span data-ttu-id="a822b-167">Pobierz/interfejs API/produkty / {id}</span><span class="sxs-lookup"><span data-stu-id="a822b-167">GET /api/Products/{id}</span></span>
- <span data-ttu-id="a822b-168">OPUBLIKUJ/api/produktów</span><span class="sxs-lookup"><span data-stu-id="a822b-168">POST /api/Products</span></span>

<span data-ttu-id="a822b-169">Jeśli akcja kontrolera obsługuje wiele metod HTTP, **ApiExplorer** traktuje każdą metodę jako odrębne interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="a822b-169">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="a822b-170">Spowoduje ukrycie interfejsu API z poziomu **ApiExplorer**, Dodaj **ApiExplorerSettings** atrybutu akcji i ustaw *IgnoreApi* na wartość true.</span><span class="sxs-lookup"><span data-stu-id="a822b-170">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="a822b-171">Ten atrybut można również dodać do kontrolera, które mają zostać wykluczone całego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="a822b-171">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="a822b-172">Klasa ApiExplorer otrzymuje ciągów dokumentacji z **IDocumentationProvider** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="a822b-172">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="a822b-173">Jak wcześniej, biblioteka strony pomocy zawiera **IDocumentationProvider** , pobiera z ciągów dokumentacji XML dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="a822b-173">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="a822b-174">Kod znajduje się w /Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="a822b-174">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="a822b-175">Możesz uzyskać dokumentację z innego źródła, pisania własnych **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="a822b-175">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="a822b-176">Aby powiązać go, należy wywołać **SetDocumentationProvider** zdefiniowany w metodzie rozszerzenia **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="a822b-176">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="a822b-177">**ApiExplorer** automatycznie wywołuje **IDocumentationProvider** interfejsu można pobrać ciągów dokumentacji dla każdego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="a822b-177">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="a822b-178">Przechowuje je w **dokumentacji** właściwość **ApiDescription** i **ApiParameterDescription** obiektów.</span><span class="sxs-lookup"><span data-stu-id="a822b-178">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a822b-179">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="a822b-179">Next Steps</span></span>

<span data-ttu-id="a822b-180">Nie są ograniczone do stron pomocy, pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="a822b-180">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="a822b-181">W rzeczywistości **ApiExplorer** nie jest ograniczony do tworzenia stron pomocy.</span><span class="sxs-lookup"><span data-stu-id="a822b-181">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="a822b-182">Połącz Huang Yao zapisane wpisy na kilka wspaniałych blogu pomagające w myśl gotowych:</span><span class="sxs-lookup"><span data-stu-id="a822b-182">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="a822b-183">Dodawanie prostego klienta testowego na stronie pomocy interfejsu API sieci Web platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a822b-183">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="a822b-184">Tworzenie ASP.NET Web stronie pomocy interfejsu API pracy nad własnym hostowanych usług</span><span class="sxs-lookup"><span data-stu-id="a822b-184">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="a822b-185">Generowanie czasu projektowania strony pomocy (lub klienta) interfejsu API sieci Web platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a822b-185">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="a822b-186">Zaawansowane dostosowania strona pomocy</span><span class="sxs-lookup"><span data-stu-id="a822b-186">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
