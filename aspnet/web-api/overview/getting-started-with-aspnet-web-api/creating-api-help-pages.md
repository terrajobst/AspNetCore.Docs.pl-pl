---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Tworzenie stron pomocy dla interfejsu API sieci Web programu ASP.NET | Dokumentacja firmy Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 37fd26ebaea192cb540c443eff8a07343ab8c15b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="ef5ec-102">Tworzenie stron pomocy dla interfejsu API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ef5ec-102">Creating Help Pages for ASP.NET Web API</span></span>
====================
<span data-ttu-id="ef5ec-103">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ef5ec-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ef5ec-104">Podczas tworzenia składnika web API często jest przydatne do tworzenia strony pomocy, aby inni deweloperzy będą wiedzieli, sposób wywołania interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-104">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="ef5ec-105">Można utworzyć cała dokumentacja ręcznie, ale zaleca się generowanie możliwie.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-105">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span>

<span data-ttu-id="ef5ec-106">Aby ułatwić to zadanie, Web API platformy ASP.NET udostępnia bibliotekę do automatycznego generowania strony pomocy w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-106">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="ef5ec-107">Tworzenie stron pomocy interfejsu API</span><span class="sxs-lookup"><span data-stu-id="ef5ec-107">Creating API Help Pages</span></span>

<span data-ttu-id="ef5ec-108">Zainstaluj [ASP.NET i sieć Web narzędzi aktualizacji 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="ef5ec-108">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="ef5ec-109">Ta aktualizacja integruje strony pomocy szablonu projektu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-109">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="ef5ec-110">Następnie utwórz nowy projekt ASP.NET MVC 4 i wybierz szablon projektu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-110">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="ef5ec-111">Szablon projektu tworzy kontroler przykładowy interfejs API o nazwie `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-111">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="ef5ec-112">Szablon tworzy także interfejsu API strony pomocy.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-112">The template also creates the API help pages.</span></span> <span data-ttu-id="ef5ec-113">Wszystkie pliki kodu na stronie pomocy są umieszczane w folderze obszarów projektu.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-113">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="ef5ec-114">Po uruchomieniu aplikacji, strony głównej zawiera łącze do strony pomocy interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-114">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="ef5ec-115">Na stronie głównej ścieżka względna jest/Help.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-115">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="ef5ec-116">To łącze powoduje przeniesienie na stronie Podsumowanie interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-116">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="ef5ec-117">Widok MVC dla tej strony jest zdefiniowany w Areas/HelpPage/Views/Help/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-117">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="ef5ec-118">Możesz edytować tę stronę, aby zmodyfikować układ, wprowadzenie, title, style i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-118">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="ef5ec-119">Główna część strony znajduje się tabela interfejsów API, które są pogrupowane według kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-119">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="ef5ec-120">Wpisy tabeli są generowane dynamicznie, przy użyciu **IApiExplorer** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-120">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="ef5ec-121">(I będzie komunikował więcej informacji na temat tego interfejsu później.) Po dodaniu nowego kontrolera interfejsu API tabeli jest automatycznie aktualizowany w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-121">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="ef5ec-122">Kolumna "Interfejsu API" zawiera metody HTTP oraz względnego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-122">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="ef5ec-123">Kolumna "Opis" zawiera dokumentacja dla każdego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-123">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="ef5ec-124">Dokumentacja jest początkowo tylko tekst zastępczy.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-124">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="ef5ec-125">W następnej sekcji I opisano sposób dodawania dokumentacji z komentarze XML.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-125">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="ef5ec-126">Każdy interfejs API zawiera łącze do strony z bardziej szczegółowe informacje, w tym przykładzie treści żądania i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-126">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="ef5ec-127">Dodawanie strony pomocy do istniejącego projektu</span><span class="sxs-lookup"><span data-stu-id="ef5ec-127">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="ef5ec-128">Można dodać strony pomocy do istniejącego projektu interfejsu API sieci Web za pomocą Menedżera pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-128">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="ef5ec-129">Ta opcja jest przydatna w przypadku uruchomienia szablonu projektu innego niż szablon "Interfejsu API sieci Web".</span><span class="sxs-lookup"><span data-stu-id="ef5ec-129">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="ef5ec-130">Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-130">From the **Tools** menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="ef5ec-131">W [Konsola Menedżera pakietów](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) okna, wpisz jedno z następujących poleceń:</span><span class="sxs-lookup"><span data-stu-id="ef5ec-131">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="ef5ec-132">Aby uzyskać **C#** aplikacji:`Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="ef5ec-132">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="ef5ec-133">Aby uzyskać **Visual Basic** aplikacji:`Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="ef5ec-133">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="ef5ec-134">Istnieją dwa pakiety, dla C# i jeden dla języka Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-134">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="ef5ec-135">Upewnij się użyć zgodną z projektu.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-135">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="ef5ec-136">To polecenie instaluje konieczne zestawy i dodaje widoków MVC dla strony pomocy (znajdujące się w folderze obszarów/HelpPage).</span><span class="sxs-lookup"><span data-stu-id="ef5ec-136">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="ef5ec-137">Należy ręcznie dodać łącze do strony pomocy.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-137">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="ef5ec-138">Identyfikator URI jest/Help.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-138">The URI is /Help.</span></span> <span data-ttu-id="ef5ec-139">Aby utworzyć łącze w widoku razor, Dodaj następujące informacje:</span><span class="sxs-lookup"><span data-stu-id="ef5ec-139">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="ef5ec-140">Upewnij się również zarejestrować obszarów.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-140">Also, make sure to register areas.</span></span> <span data-ttu-id="ef5ec-141">W pliku Global.asax, Dodaj następujący kod do **aplikacji\_Start** metody, jeśli nie jest już:</span><span class="sxs-lookup"><span data-stu-id="ef5ec-141">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="ef5ec-142">Dodawanie dokumentacji interfejsu API</span><span class="sxs-lookup"><span data-stu-id="ef5ec-142">Adding API Documentation</span></span>

<span data-ttu-id="ef5ec-143">Domyślnie pomocy strony mają ciągów symbolu zastępczego dla dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-143">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="ef5ec-144">Można użyć [komentarze dokumentacji XML](https://msdn.microsoft.com/library/b2s063f7.aspx) można utworzyć w dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-144">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="ef5ec-145">Aby włączyć tę funkcję, otwórz plik obszarów/HelpPage/aplikacji\_Start/HelpPageConfig.cs i Usuń komentarz następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="ef5ec-145">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="ef5ec-146">Teraz Włącz dokumentacji XML.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-146">Now enable XML documentation.</span></span> <span data-ttu-id="ef5ec-147">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-147">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="ef5ec-148">Wybierz **kompilacji** strony.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-148">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="ef5ec-149">W obszarze **dane wyjściowe**, sprawdź **pliku dokumentacji XML**.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-149">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="ef5ec-150">W polu edycji wpisz "App\_Data/XmlDocument.xml".</span><span class="sxs-lookup"><span data-stu-id="ef5ec-150">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="ef5ec-151">Następnie otwórz folder kod `ValuesController` kontrolera interfejsu API, który jest zdefiniowany w /Controllers/ValuesControler.cs.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-151">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesControler.cs.</span></span> <span data-ttu-id="ef5ec-152">Dodaj niektórych komentarzy do dokumentacji do metody kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-152">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="ef5ec-153">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ef5ec-153">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="ef5ec-154">Porada: Jeśli pozycji karetki w wierszu powyżej metody i typu trzy ukośniki, Visual Studio automatycznie wstawia elementy XML.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-154">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="ef5ec-155">Można następnie Wypełnij puste wartości.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-155">Then you can fill in the blanks.</span></span>


<span data-ttu-id="ef5ec-156">Teraz utworzyć i uruchomić ponownie aplikację i przejdź do strony pomocy.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-156">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="ef5ec-157">Ciągi dokumentacji powinny być wyświetlane w tabeli interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-157">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="ef5ec-158">Strona pomocy odczytuje ciągi z pliku XML w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-158">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="ef5ec-159">(Podczas wdrażania aplikacji, upewnij się, że wdrażanie pliku XML.)</span><span class="sxs-lookup"><span data-stu-id="ef5ec-159">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="ef5ec-160">Kulisy</span><span class="sxs-lookup"><span data-stu-id="ef5ec-160">Under the Hood</span></span>

<span data-ttu-id="ef5ec-161">Strony pomocy są wbudowane nad **ApiExplorer** klasy, która jest częścią struktury interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-161">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="ef5ec-162">**ApiExplorer** klasy zawiera materiały raw do tworzenia strony pomocy.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-162">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="ef5ec-163">Dla każdego interfejsu API **ApiExplorer** zawiera **ApiDescription** , który opisuje interfejs API.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-163">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="ef5ec-164">W tym celu "interfejsu API" jest zdefiniowany jako kombinacja metody HTTP i względnym identyfikatorem URI.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-164">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="ef5ec-165">Na przykład poniżej przedstawiono niektóre różne funkcje interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="ef5ec-165">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="ef5ec-166">Pobierz /api/Products</span><span class="sxs-lookup"><span data-stu-id="ef5ec-166">GET /api/Products</span></span>
- <span data-ttu-id="ef5ec-167">Pobierz /api/produkty / {id}</span><span class="sxs-lookup"><span data-stu-id="ef5ec-167">GET /api/Products/{id}</span></span>
- <span data-ttu-id="ef5ec-168">POST/api/produktów</span><span class="sxs-lookup"><span data-stu-id="ef5ec-168">POST /api/Products</span></span>

<span data-ttu-id="ef5ec-169">Jeśli akcja kontroler obsługuje wiele metod HTTP, **ApiExplorer** traktuje każdej metody jako odrębne interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-169">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="ef5ec-170">Aby ukryć interfejsu API z **ApiExplorer**, Dodaj **ApiExplorerSettings** atrybutu akcji i zestawu *IgnoreApi* na wartość true.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-170">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="ef5ec-171">Można również dodać ten atrybut z kontrolerem, aby wykluczyć całego kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-171">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="ef5ec-172">Klasa ApiExplorer pobiera ciągi dokumentacji **IDocumentationProvider** interfejsu.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-172">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="ef5ec-173">Jak przedstawiono wcześniej, biblioteka strony pomocy zawiera **IDocumentationProvider** który pobiera dokumentację z ciągów dokumentacji XML.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-173">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="ef5ec-174">Kod znajduje się w /Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-174">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="ef5ec-175">Można uzyskać dokumentację z innego źródła pisanie własnych **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-175">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="ef5ec-176">Aby połączenie go w górę, należy wywołać **SetDocumentationProvider** zdefiniowany w metodzie rozszerzenia **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="ef5ec-176">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="ef5ec-177">**ApiExplorer** automatycznie wywołuje **IDocumentationProvider** interfejsu można pobrać ciągów dokumentacji dla każdego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-177">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="ef5ec-178">Przechowuje je w **dokumentacji** właściwość **ApiDescription** i **ApiParameterDescription** obiektów.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-178">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ef5ec-179">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="ef5ec-179">Next Steps</span></span>

<span data-ttu-id="ef5ec-180">Nie są ograniczone do strony pomocy pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-180">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="ef5ec-181">W rzeczywistości **ApiExplorer** nie jest ograniczona do tworzenia strony pomocy.</span><span class="sxs-lookup"><span data-stu-id="ef5ec-181">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="ef5ec-182">Łącz Huang Yao został zapisany się, że niektóre dużą blogu ogłoszenia ułatwiających myśląc fabrycznej:</span><span class="sxs-lookup"><span data-stu-id="ef5ec-182">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="ef5ec-183">Dodawanie prostego klienta testowego do strona pomocy interfejsu API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ef5ec-183">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="ef5ec-184">Tworzenie stronę sieci Web ASP.NET interfejsu API pomocy pracować nad własnym obsługiwane usługi</span><span class="sxs-lookup"><span data-stu-id="ef5ec-184">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="ef5ec-185">Generowanie czasu projektowania strony pomocy (lub klienta) interfejsu API sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ef5ec-185">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="ef5ec-186">Zaawansowane dostosowania strona pomocy</span><span class="sxs-lookup"><span data-stu-id="ef5ec-186">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
