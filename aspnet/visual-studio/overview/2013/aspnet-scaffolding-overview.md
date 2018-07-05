---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Funkcja tworzenia szkieletu ASP.NET w programie Visual Studio 2013 | Dokumentacja firmy Microsoft
author: tfitzmac
description: Funkcja tworzenia szkieletu ASP.NET jest nową funkcję, która znajduje się w programie Visual Studio 2013.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: f014ef1439376349f49f10f1f4063945ab81e7c1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369095"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="b3375-103">Funkcja tworzenia szkieletu ASP.NET w programie Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b3375-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>
====================
<span data-ttu-id="b3375-104">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b3375-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b3375-105">Funkcja tworzenia szkieletu ASP.NET jest nową funkcję, która znajduje się w programie Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b3375-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>


## <a name="overview"></a><span data-ttu-id="b3375-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="b3375-106">Overview</span></span>

<span data-ttu-id="b3375-107">Funkcja tworzenia szkieletu ASP.NET jest struktura generowania kodu dla aplikacji sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b3375-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="b3375-108">Visual Studio 2013 obejmuje generatorów kodu wstępnie zainstalowane dla projektów MVC i interfejs API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b3375-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="b3375-109">Dodasz szkieletu do projektu, jeśli chcesz szybko dodać kod, który wchodzi w interakcję z modelami danych.</span><span class="sxs-lookup"><span data-stu-id="b3375-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="b3375-110">Szkieletów może skrócić czas do opracowywania działań standardowych danych w projekcie.</span><span class="sxs-lookup"><span data-stu-id="b3375-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="b3375-111">Domyślnie program Visual Studio 2013 nie obsługuje generowania kodu dla projektu formularzy sieci Web, ale za pomocą tworzenia szkieletów formularzy sieci Web przez dodanie zależności MVC do projektu lub Instalowanie rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="b3375-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="b3375-112">Poniżej przedstawiono oba podejścia.</span><span class="sxs-lookup"><span data-stu-id="b3375-112">Both approaches are shown below.</span></span>

<span data-ttu-id="b3375-113">Visual Studio 2013 Update 2 (obecnie RC) zapewnia możliwość rozszerzania funkcja tworzenia szkieletu ASP.NET w celu spełnienia wymagań scenariusza.</span><span class="sxs-lookup"><span data-stu-id="b3375-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="b3375-114">Dzięki tej funkcji można utworzyć szablon dostosowane szkieletu i dodać go do okna dialogowego Dodawanie szkieletu nowe.</span><span class="sxs-lookup"><span data-stu-id="b3375-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="b3375-115">W ramach dostosowanego szablonu możesz określić kod, który jest generowany, gdy Dodawanie elementu szkieletu.</span><span class="sxs-lookup"><span data-stu-id="b3375-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="b3375-116">Aby uzyskać więcej informacji, zobacz [tworzenia niestandardowego Generator szkieletu dla programu Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="b3375-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b3375-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="b3375-117">Prerequisites</span></span>

<span data-ttu-id="b3375-118">Aby korzystać z ASP.NET tworzenie szkieletów, musisz mieć:</span><span class="sxs-lookup"><span data-stu-id="b3375-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="b3375-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b3375-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="b3375-120">Narzędzia dla deweloperów sieci Web (część domyślnej instalacji programu Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="b3375-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="b3375-121">Struktury sieci Web platformy ASP.NET i Tools 2013 (część domyślnej instalacji programu Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="b3375-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="b3375-122">Dodawanie elementu szkieletu do MVC lub interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="b3375-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="b3375-123">W celu dodania szkieletu, kliknij prawym przyciskiem myszy projekt lub folder w projekcie, a następnie wybierz **Dodaj** — **nowy element szkieletu**, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="b3375-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![Dodawanie elementu szkieletu](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="b3375-125">Z **Dodawanie szkieletu** okna, wybierz typ szkieletu do dodania.</span><span class="sxs-lookup"><span data-stu-id="b3375-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![Wybierz typ szkieletu](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="b3375-127">**Dodaj kontroler** okno pozwala wybrać opcje generowania kontrolera, w tym, czy chcesz korzystać z nowych funkcji asynchronicznej z platformy Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="b3375-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![Dodawanie kontrolera](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="b3375-129">Odpowiednich klas i strony są tworzone dla danego scenariusza.</span><span class="sxs-lookup"><span data-stu-id="b3375-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="b3375-130">Na przykład na poniższej ilustracji przedstawiono kontroler MVC i widoki, które zostały utworzone za pomocą funkcją szkieletów dla klasę modelu o nazwie filmów.</span><span class="sxs-lookup"><span data-stu-id="b3375-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![Utworzone pliki](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="b3375-132">Dodawanie elementu szkieletu do formularzy sieci Web</span><span class="sxs-lookup"><span data-stu-id="b3375-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="b3375-133">Aby dodać tworzenia szkieletów, które generuje kod formularzy sieci Web, możesz zainstalować rozszerzenie programu Visual Studio lub Dodaj zależności MVC.</span><span class="sxs-lookup"><span data-stu-id="b3375-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="b3375-134">Poniżej przedstawiono oba podejścia, ale należy wykonać jedną z tych metod.</span><span class="sxs-lookup"><span data-stu-id="b3375-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="b3375-135">Formularze sieci Web, tworzenia szkieletów rozszerzenia</span><span class="sxs-lookup"><span data-stu-id="b3375-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="b3375-136">Można zainstalować rozszerzenia programu Visual Studio, które umożliwiają tworzenie szkieletu za pomocą projektu formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b3375-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="b3375-137">W programie Visual Studio, wybierz **narzędzia** i następnie **rozszerzenia i aktualizacje**.</span><span class="sxs-lookup"><span data-stu-id="b3375-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="b3375-138">W tym oknie dialogowym wyszukiwania galerii Visual Studio dla **szkieletu formularzy sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="b3375-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![Zainstaluj formularzy sieci web, tworzenia szkieletów](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="b3375-140">Aby uzyskać więcej informacji, zobacz [szkieletu formularzy sieci Web](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span><span class="sxs-lookup"><span data-stu-id="b3375-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="b3375-141">Zależności MVC</span><span class="sxs-lookup"><span data-stu-id="b3375-141">MVC Dependencies</span></span>

<span data-ttu-id="b3375-142">Aby dodać zależności MVC, wybierz **Dodaj** - **nowy element szkieletu**.</span><span class="sxs-lookup"><span data-stu-id="b3375-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="b3375-143">W oknie Dodawanie szkieletu wybierz **zależności MVC**, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="b3375-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![Dodaj zależności MVC](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="b3375-145">Dostępne są dwie opcje na potrzeby tworzenia szkieletów MVC; Minimalna i pełne.</span><span class="sxs-lookup"><span data-stu-id="b3375-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="b3375-146">Jeśli wybierzesz przycisku minimalnych tylko pakiety NuGet i odwołania dla platformy ASP.NET MVC są dodawane do projektu.</span><span class="sxs-lookup"><span data-stu-id="b3375-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="b3375-147">Jeśli wybierzesz opcję pełnej, minimalnym zależności zostaną dodane, a także wymagane pliki zawartości projektu MVC.</span><span class="sxs-lookup"><span data-stu-id="b3375-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="b3375-148">Aby łatwo korzystać z tworzenia szkieletów, wybierz pełną zależności.</span><span class="sxs-lookup"><span data-stu-id="b3375-148">To easily use scaffolding, select Full dependencies.</span></span>

![Wybierz opcję pełnej zależności](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="b3375-150">Po dodaniu zależności, zobaczysz **readme.txt** pliku.</span><span class="sxs-lookup"><span data-stu-id="b3375-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="b3375-151">Dokładnie postępuj zgodnie z instrukcjami w tym pliku, aby upewnić się, że projekt działa prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="b3375-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="b3375-152">Po zakończeniu kroków z pliku readme.txt można dodać nowy element szkieletowy, jak pokazano w poprzedniej sekcji o MVC i interfejs API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="b3375-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="b3375-153">Widoki generowane automatycznie i kontroler będzie działać prawidłowo w obrębie projektu.</span><span class="sxs-lookup"><span data-stu-id="b3375-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="b3375-154">Samouczki</span><span class="sxs-lookup"><span data-stu-id="b3375-154">Tutorials</span></span>

<span data-ttu-id="b3375-155">Aby utworzyć niestandardowe Generator szkieletu, zobacz [tworzenia niestandardowego Generator szkieletu dla programu Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="b3375-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="b3375-156">Aby dostosować wygenerowanych plików, zobacz [jak dostosować wygenerowane pliki w oknie dialogowym Nowy element szkieletu](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span><span class="sxs-lookup"><span data-stu-id="b3375-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="b3375-157">Na przykład szkieletów z **rozwoju Database First**, zobacz [EF Database First ze wzorca ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="b3375-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="b3375-158">Na przykład szkieletów w **MVC** projektu, zobacz [wprowadzenie do ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="b3375-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="b3375-159">Na przykład szkieletów w **interfejsu API sieci Web** projektu, zobacz [Tworzenie interfejsu API REST przy użyciu atrybutu routingu w sieci Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="b3375-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
