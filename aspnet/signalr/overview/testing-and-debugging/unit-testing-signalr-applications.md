---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Testy jednostkowe aplikacji SignalR | Dokumentacja firmy Microsoft
author: pfletcher
description: "W tym artykule opisano sposób użycia funkcji testów jednostkowych 2.0 SignalR."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: d767e1a9d27670387133e5a48a8f92f5bdd39d9e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-signalr-applications"></a><span data-ttu-id="30fd7-103">Jednostka testowania aplikacji SignalR</span><span class="sxs-lookup"><span data-stu-id="30fd7-103">Unit Testing SignalR Applications</span></span>
====================
<span data-ttu-id="30fd7-104">przez [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="30fd7-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="30fd7-105">W tym artykule opisano, za pomocą funkcji testów jednostkowych 2 SignalR.</span><span class="sxs-lookup"><span data-stu-id="30fd7-105">This article describes using the Unit Testing features of SignalR 2.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="30fd7-106">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="30fd7-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="30fd7-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="30fd7-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="30fd7-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="30fd7-108">.NET 4.5</span></span>
> - <span data-ttu-id="30fd7-109">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="30fd7-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="30fd7-110">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="30fd7-110">Questions and comments</span></span>
> 
> <span data-ttu-id="30fd7-111">Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony.</span><span class="sxs-lookup"><span data-stu-id="30fd7-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="30fd7-112">Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="30fd7-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="30fd7-113">Testy jednostkowe aplikacji SignalR</span><span class="sxs-lookup"><span data-stu-id="30fd7-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="30fd7-114">Funkcje testu jednostki w SignalR 2 służy do tworzenia testów jednostkowych dla aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="30fd7-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="30fd7-115">SignalR 2 zawiera [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interfejs, który może służyć do tworzenia obiektu zasymulować symulowanie testowanie metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="30fd7-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="30fd7-116">W tej sekcji dodasz testów jednostkowych dla aplikacji utworzonych w [Wprowadzenie — samouczek](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu [XUnit.net](https://github.com/xunit/xunit) i [Moq](https://github.com/Moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="30fd7-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="30fd7-117">XUnit.net będzie można użyć do kontrolowania testu; Moq będzie używane do tworzenia [mock](http://en.wikipedia.org/wiki/Mock_object) obiektu do testowania.</span><span class="sxs-lookup"><span data-stu-id="30fd7-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="30fd7-118">Inne struktury mocking można w razie potrzeby; [NSubstitute](http://nsubstitute.github.io/) jest również dobrym rozwiązaniem.</span><span class="sxs-lookup"><span data-stu-id="30fd7-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="30fd7-119">W tym samouczku pokazano, jak skonfigurować zasymulować obiektu na dwa sposoby: najpierw przy użyciu `dynamic` obiektu (wprowadzona w programie .NET Framework 4), a druga Strona, przy użyciu interfejsu.</span><span class="sxs-lookup"><span data-stu-id="30fd7-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="30fd7-120">Spis treści</span><span class="sxs-lookup"><span data-stu-id="30fd7-120">Contents</span></span>

<span data-ttu-id="30fd7-121">Ten samouczek zawiera następujące sekcje.</span><span class="sxs-lookup"><span data-stu-id="30fd7-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="30fd7-122">Testowanie jednostkowe na platformie dynamiczne</span><span class="sxs-lookup"><span data-stu-id="30fd7-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="30fd7-123">Jednostka testowania według typu</span><span class="sxs-lookup"><span data-stu-id="30fd7-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="30fd7-124">Testowanie jednostkowe na platformie dynamiczne</span><span class="sxs-lookup"><span data-stu-id="30fd7-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="30fd7-125">W tej sekcji dodasz testu jednostkowego dla aplikacji utworzonych w [Wprowadzenie — samouczek](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu obiekt dynamiczny.</span><span class="sxs-lookup"><span data-stu-id="30fd7-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="30fd7-126">Zainstaluj [rozszerzenie modułu uruchamiającego XUnit](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) dla programu Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="30fd7-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="30fd7-127">Wykonać [Wprowadzenie — samouczek](../getting-started/tutorial-getting-started-with-signalr.md), lub pobrać ukończona aplikacja z [galerii kodu MSDN](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="30fd7-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="30fd7-128">Jeśli używasz wersji pobierania aplikacji wprowadzenie, otwórz **Konsola Menedżera pakietów** i kliknij przycisk **przywrócić** Aby dodać pakiet SignalR do projektu.</span><span class="sxs-lookup"><span data-stu-id="30fd7-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![Przywracanie pakietów](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="30fd7-130">Dodaj projekt do rozwiązania dla testu jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="30fd7-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="30fd7-131">Kliknij prawym przyciskiem myszy rozwiązanie w **Eksploratora rozwiązań** i wybierz **Dodaj**, **nowy projekt...** . W obszarze **C#** węzła, wybierz opcję **Windows** węzła.</span><span class="sxs-lookup"><span data-stu-id="30fd7-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="30fd7-132">Wybierz **Biblioteka klas**.</span><span class="sxs-lookup"><span data-stu-id="30fd7-132">Select **Class Library**.</span></span> <span data-ttu-id="30fd7-133">Nazwa nowego projektu **TestLibrary** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="30fd7-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![Utwórz bibliotekę testu](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="30fd7-135">Dodaj odwołanie do projektu biblioteki testów w projekcie SignalRChat.</span><span class="sxs-lookup"><span data-stu-id="30fd7-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="30fd7-136">Kliknij prawym przyciskiem myszy **TestLibrary** projekt i wybierz **Dodaj**, **odwołania...** . Wybierz **projekty** węźle **rozwiązania** węzeł, a także sprawdź **SignalRChat**.</span><span class="sxs-lookup"><span data-stu-id="30fd7-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="30fd7-137">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="30fd7-137">Click **OK**.</span></span>

    ![Dodaj odwołanie do projektu](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="30fd7-139">Dodawanie pakietów SignalR, Moq i XUnit do **TestLibrary** projektu.</span><span class="sxs-lookup"><span data-stu-id="30fd7-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="30fd7-140">W **Konsola Menedżera pakietów**ustaw **domyślny projekt** listy rozwijanej, aby **TestLibrary**.</span><span class="sxs-lookup"><span data-stu-id="30fd7-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="30fd7-141">Uruchom następujące polecenia w oknie konsoli:</span><span class="sxs-lookup"><span data-stu-id="30fd7-141">Run the following commands in the console window:</span></span>

    - `Install-Package Microsoft.AspNet.SignalR`
    - `Install-Package Moq`
    - `Install-Package XUnit`

    ![Instalowanie pakietów](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="30fd7-143">Utwórz plik testu.</span><span class="sxs-lookup"><span data-stu-id="30fd7-143">Create the test file.</span></span> <span data-ttu-id="30fd7-144">Kliknij prawym przyciskiem myszy **TestLibrary** projekt i kliknij przycisk **Dodaj...** , **Klasy**.</span><span class="sxs-lookup"><span data-stu-id="30fd7-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="30fd7-145">Nazwa nowej klasy **Tests.cs**.</span><span class="sxs-lookup"><span data-stu-id="30fd7-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="30fd7-146">Zastąp zawartość Tests.cs z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="30fd7-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="30fd7-147">W powyższym kodzie klienta testowego jest tworzony przy użyciu `Mock` obiekt z [Moq](https://github.com/Moq/moq4) biblioteki typu [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1, przypisywanie `dynamic` dla typu Parametr). `IHubCallerConnectionContext` Interfejs jest obiekt serwera proxy, z którym wywołania metody na kliencie.</span><span class="sxs-lookup"><span data-stu-id="30fd7-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="30fd7-148">`broadcastMessage` Funkcja następnie jest zdefiniowany dla zasymulować klienta, dzięki czemu może być wywoływany przez `ChatHub` klasy.</span><span class="sxs-lookup"><span data-stu-id="30fd7-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="30fd7-149">Następnie wywołuje aparat testów `Send` metody `ChatHub` klasy, która z kolei wywołuje mocked `broadcastMessage` funkcji.</span><span class="sxs-lookup"><span data-stu-id="30fd7-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="30fd7-150">Skompiluj rozwiązanie, naciskając klawisz **F6**.</span><span class="sxs-lookup"><span data-stu-id="30fd7-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="30fd7-151">Uruchamianie testu jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="30fd7-151">Run the unit test.</span></span> <span data-ttu-id="30fd7-152">W programie Visual Studio, wybierz **testu**, **Windows**, **Eksploratora testów**.</span><span class="sxs-lookup"><span data-stu-id="30fd7-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="30fd7-153">W oknie Eksploratora testów, kliknij prawym przyciskiem myszy **HubsAreMockableViaDynamic** i wybierz **Uruchom wybrane testy**.</span><span class="sxs-lookup"><span data-stu-id="30fd7-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Eksplorator testów](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="30fd7-155">Upewnij się, że test przekazany przez sprawdzanie dolnym okienku w oknie Eksploratora testów.</span><span class="sxs-lookup"><span data-stu-id="30fd7-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="30fd7-156">W oknie będą wyświetlane, że przekazany testu.</span><span class="sxs-lookup"><span data-stu-id="30fd7-156">The window will show that the test passed.</span></span>

    ![Test zakończył się powodzeniem](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="30fd7-158">Jednostka testowania według typu</span><span class="sxs-lookup"><span data-stu-id="30fd7-158">Unit testing by type</span></span>

<span data-ttu-id="30fd7-159">W tej sekcji dodasz testu dla aplikacji utworzonych w [Wprowadzenie — samouczek](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu interfejsu, który zawiera metody do sprawdzenia.</span><span class="sxs-lookup"><span data-stu-id="30fd7-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="30fd7-160">Wykonać kroki od 1 do 7 w [testowanie jednostkowe na platformie dynamicznie](#dynamic) samouczek powyżej.</span><span class="sxs-lookup"><span data-stu-id="30fd7-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="30fd7-161">Zastąp zawartość Tests.cs z następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="30fd7-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="30fd7-162">W powyższym kodzie utworzono interfejs definiujący podpis `broadcastMessage` metody, dla której aparat testów utworzy zasymulować klienta.</span><span class="sxs-lookup"><span data-stu-id="30fd7-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="30fd7-163">Zasymulować klienta jest tworzony przy użyciu `Mock` obiektu typu [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1, przypisywanie `dynamic` dla parametru typu.) `IHubCallerConnectionContext` Interfejs jest obiekt serwera proxy, z którym wywołania metody na kliencie.</span><span class="sxs-lookup"><span data-stu-id="30fd7-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="30fd7-164">Testu następnie tworzy wystąpienie `ChatHub`, a następnie tworzy wersję makiety `broadcastMessage` metodę, która z kolei jest wywoływany przez wywołanie metody `Send` metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="30fd7-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="30fd7-165">Skompiluj rozwiązanie, naciskając klawisz **F6**.</span><span class="sxs-lookup"><span data-stu-id="30fd7-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="30fd7-166">Uruchamianie testu jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="30fd7-166">Run the unit test.</span></span> <span data-ttu-id="30fd7-167">W programie Visual Studio, wybierz **testu**, **Windows**, **Eksploratora testów**.</span><span class="sxs-lookup"><span data-stu-id="30fd7-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="30fd7-168">W oknie Eksploratora testów, kliknij prawym przyciskiem myszy **HubsAreMockableViaDynamic** i wybierz **Uruchom wybrane testy**.</span><span class="sxs-lookup"><span data-stu-id="30fd7-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Eksplorator testów](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="30fd7-170">Upewnij się, że test przekazany przez sprawdzanie dolnym okienku w oknie Eksploratora testów.</span><span class="sxs-lookup"><span data-stu-id="30fd7-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="30fd7-171">W oknie będą wyświetlane, że przekazany testu.</span><span class="sxs-lookup"><span data-stu-id="30fd7-171">The window will show that the test passed.</span></span>

    ![Test zakończył się powodzeniem](unit-testing-signalr-applications/_static/image8.png)
