---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Testowanie aplikacji SignalR jednostek | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym artykule opisano, jak używać funkcji testów jednostkowych SignalR w wersji 2.0.
ms.author: riande
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: ba8f5d4577403fe9765641d7ee5d88bde045680a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910827"
---
<a name="unit-testing-signalr-applications"></a><span data-ttu-id="1289b-103">Testowanie jednostkowe aplikacji SignalR</span><span class="sxs-lookup"><span data-stu-id="1289b-103">Unit Testing SignalR Applications</span></span>
====================
<span data-ttu-id="1289b-104">przez [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="1289b-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="1289b-105">W tym artykule opisano korzystanie z funkcji testów jednostkowych SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="1289b-105">This article describes using the Unit Testing features of SignalR 2.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="1289b-106">Wersje oprogramowania używaną w tym temacie</span><span class="sxs-lookup"><span data-stu-id="1289b-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="1289b-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="1289b-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="1289b-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="1289b-108">.NET 4.5</span></span>
> - <span data-ttu-id="1289b-109">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="1289b-109">SignalR version 2</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="1289b-110">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="1289b-110">Questions and comments</span></span>
>
> <span data-ttu-id="1289b-111">Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="1289b-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="1289b-112">Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="1289b-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="1289b-113">Testowanie aplikacji SignalR jednostek</span><span class="sxs-lookup"><span data-stu-id="1289b-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="1289b-114">Funkcji testów jednostkowych w SignalR 2 umożliwia tworzenie testów jednostkowych dla aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="1289b-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="1289b-115">SignalR 2 zawiera [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interfejs, który może służyć do tworzenia obiektu makiety symulowanie testowanie metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="1289b-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="1289b-116">W tej sekcji dodasz testy jednostkowe dla aplikacji utworzonych w [samouczka Wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu [XUnit.net](https://github.com/xunit/xunit) i [Moq](https://github.com/Moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="1289b-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="1289b-117">XUnit.net będzie służyć do kontrolowania badania; Moq będzie służyć do tworzenia [testowanie](http://en.wikipedia.org/wiki/Mock_object) obiektu do testowania.</span><span class="sxs-lookup"><span data-stu-id="1289b-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="1289b-118">Inne struktury pozorowania może służyć w razie potrzeby; [NSubstitute](http://nsubstitute.github.io/) jest również dobrym wyborem.</span><span class="sxs-lookup"><span data-stu-id="1289b-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="1289b-119">W tym samouczku pokazano, jak skonfigurować makiety obiektu na dwa sposoby: po pierwsze, za pomocą `dynamic` obiektu (zostanie wprowadzony w programie .NET Framework 4), a drugi, przy użyciu interfejsu.</span><span class="sxs-lookup"><span data-stu-id="1289b-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="1289b-120">Spis treści</span><span class="sxs-lookup"><span data-stu-id="1289b-120">Contents</span></span>

<span data-ttu-id="1289b-121">Ten samouczek zawiera następujące sekcje.</span><span class="sxs-lookup"><span data-stu-id="1289b-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="1289b-122">Testowanie jednostek za pomocą dynamiczny</span><span class="sxs-lookup"><span data-stu-id="1289b-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="1289b-123">Testowanie jednostek, typ</span><span class="sxs-lookup"><span data-stu-id="1289b-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="1289b-124">Testowanie jednostek za pomocą dynamiczny</span><span class="sxs-lookup"><span data-stu-id="1289b-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="1289b-125">W tej sekcji dodasz testu jednostkowego dla aplikacji utworzonych w [samouczka Wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu obiekt dynamiczny.</span><span class="sxs-lookup"><span data-stu-id="1289b-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="1289b-126">Zainstaluj [rozszerzenie modułu uruchamiającego XUnit](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) dla programu Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="1289b-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="1289b-127">Z założenia wykonać [samouczka Wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md), lub pobrać gotową aplikację z [galerii kodu MSDN](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="1289b-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="1289b-128">Jeśli używasz wersji pobierania wprowadzenie do aplikacji, otwórz **Konsola Menedżera pakietów** i kliknij przycisk **przywrócić** Dodaj pakiet SignalR do projektu.</span><span class="sxs-lookup"><span data-stu-id="1289b-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![Przywracanie pakietów](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="1289b-130">Dodaj projekt do rozwiązania dla testu jednostkowego.</span><span class="sxs-lookup"><span data-stu-id="1289b-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="1289b-131">Kliknij prawym przyciskiem myszy rozwiązania w **Eksploratora rozwiązań** i wybierz **Dodaj**, **nowy projekt...** . W obszarze **C#** węzeł **Windows** węzła.</span><span class="sxs-lookup"><span data-stu-id="1289b-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="1289b-132">Wybierz **Biblioteka klas**.</span><span class="sxs-lookup"><span data-stu-id="1289b-132">Select **Class Library**.</span></span> <span data-ttu-id="1289b-133">Nazwa nowego projektu **TestLibrary** i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="1289b-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![Tworzenie biblioteki testu](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="1289b-135">Dodaj odwołanie w projekcie biblioteki testów w projekcie SignalRChat.</span><span class="sxs-lookup"><span data-stu-id="1289b-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="1289b-136">Kliknij prawym przyciskiem myszy **TestLibrary** projektu, a następnie wybierz **Dodaj**, **odwołanie...** . Wybierz **projektów** węźle **rozwiązania** węzła i sprawdź **SignalRChat**.</span><span class="sxs-lookup"><span data-stu-id="1289b-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="1289b-137">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="1289b-137">Click **OK**.</span></span>

    ![Dodaj odwołanie do projektu](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="1289b-139">Dodawanie pakietów SignalR, Moq i struktury XUnit **TestLibrary** projektu.</span><span class="sxs-lookup"><span data-stu-id="1289b-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="1289b-140">W **Konsola Menedżera pakietów**ustaw **domyślny projekt** listę rozwijaną, aby **TestLibrary**.</span><span class="sxs-lookup"><span data-stu-id="1289b-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="1289b-141">W oknie konsoli, uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="1289b-141">Run the following commands in the console window:</span></span>

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Instalowanie pakietów](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="1289b-143">Utwórz plik testu.</span><span class="sxs-lookup"><span data-stu-id="1289b-143">Create the test file.</span></span> <span data-ttu-id="1289b-144">Kliknij prawym przyciskiem myszy **TestLibrary** projektu, a następnie kliknij przycisk **Dodaj...** , **Klasy**.</span><span class="sxs-lookup"><span data-stu-id="1289b-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="1289b-145">Nadaj nowej klasie **Tests.cs**.</span><span class="sxs-lookup"><span data-stu-id="1289b-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="1289b-146">Zastąp zawartość Tests.cs następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="1289b-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="1289b-147">W powyższym kodzie klienta testowego jest tworzony przy użyciu `Mock` obiektu z [Moq](https://github.com/Moq/moq4) biblioteki typu [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (w SignalR 2.1, należy przypisać `dynamic` dla typu Parametr). `IHubCallerConnectionContext` Interfejs jest obiekt serwera proxy, za pomocą którego można wywołać metody na kliencie.</span><span class="sxs-lookup"><span data-stu-id="1289b-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="1289b-148">`broadcastMessage` Funkcja następnie jest zdefiniowana makiety klienta, dzięki czemu mogą być wywoływane przez `ChatHub` klasy.</span><span class="sxs-lookup"><span data-stu-id="1289b-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="1289b-149">Następnie wywołuje aparat testów `Send` metody `ChatHub` klasy, która z kolei wywołuje pozorowane `broadcastMessage` funkcji.</span><span class="sxs-lookup"><span data-stu-id="1289b-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="1289b-150">Skompiluj rozwiązanie, naciskając klawisz **F6**.</span><span class="sxs-lookup"><span data-stu-id="1289b-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="1289b-151">Uruchom test jednostkowy.</span><span class="sxs-lookup"><span data-stu-id="1289b-151">Run the unit test.</span></span> <span data-ttu-id="1289b-152">W programie Visual Studio, wybierz **testu**, **Windows**, **Eksplorator testów**.</span><span class="sxs-lookup"><span data-stu-id="1289b-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="1289b-153">W oknie Eksploratora testów, kliknij prawym przyciskiem myszy **HubsAreMockableViaDynamic** i wybierz **Uruchom wybrane testy**.</span><span class="sxs-lookup"><span data-stu-id="1289b-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Eksplorator testów](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="1289b-155">Upewnij się, że test zakończył się powodzeniem, sprawdzając w dolnym okienku w oknie Eksploratora testów.</span><span class="sxs-lookup"><span data-stu-id="1289b-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="1289b-156">W oknie będą wyświetlane, aby test zakończył się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="1289b-156">The window will show that the test passed.</span></span>

    ![Test zakończony pomyślnie](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="1289b-158">Testowanie jednostek, typ</span><span class="sxs-lookup"><span data-stu-id="1289b-158">Unit testing by type</span></span>

<span data-ttu-id="1289b-159">W tej sekcji dodasz testu dla aplikacji utworzonych w [samouczka Wprowadzenie](../getting-started/tutorial-getting-started-with-signalr.md) przy użyciu interfejsu, który zawiera metodę, która ma zostać przetestowana.</span><span class="sxs-lookup"><span data-stu-id="1289b-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="1289b-160">Wykonaj kroki 1 – 7 w [testowanie jednostek za pomocą dynamiczne](#dynamic) samouczek powyżej.</span><span class="sxs-lookup"><span data-stu-id="1289b-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="1289b-161">Zastąp zawartość Tests.cs następującym kodem.</span><span class="sxs-lookup"><span data-stu-id="1289b-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="1289b-162">W powyższym kodzie jest tworzony interfejs definiujący podpis `broadcastMessage` metody, dla którego aparat testów utworzy makiety klienta.</span><span class="sxs-lookup"><span data-stu-id="1289b-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="1289b-163">Makiety klienta jest tworzony przy użyciu `Mock` obiektu typu [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (w SignalR 2.1, należy przypisać `dynamic` dla parametru typu.) `IHubCallerConnectionContext` Interfejs jest obiekt serwera proxy, za pomocą którego można wywołać metody na kliencie.</span><span class="sxs-lookup"><span data-stu-id="1289b-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="1289b-164">Test następnie tworzy wystąpienie `ChatHub`, a następnie tworzy wersję makiety obiektu `broadcastMessage` metody, która z kolei jest wywoływany przez wywołanie metody `Send` metody koncentratora.</span><span class="sxs-lookup"><span data-stu-id="1289b-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="1289b-165">Skompiluj rozwiązanie, naciskając klawisz **F6**.</span><span class="sxs-lookup"><span data-stu-id="1289b-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="1289b-166">Uruchom test jednostkowy.</span><span class="sxs-lookup"><span data-stu-id="1289b-166">Run the unit test.</span></span> <span data-ttu-id="1289b-167">W programie Visual Studio, wybierz **testu**, **Windows**, **Eksplorator testów**.</span><span class="sxs-lookup"><span data-stu-id="1289b-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="1289b-168">W oknie Eksploratora testów, kliknij prawym przyciskiem myszy **HubsAreMockableViaDynamic** i wybierz **Uruchom wybrane testy**.</span><span class="sxs-lookup"><span data-stu-id="1289b-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Eksplorator testów](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="1289b-170">Upewnij się, że test zakończył się powodzeniem, sprawdzając w dolnym okienku w oknie Eksploratora testów.</span><span class="sxs-lookup"><span data-stu-id="1289b-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="1289b-171">W oknie będą wyświetlane, aby test zakończył się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="1289b-171">The window will show that the test passed.</span></span>

    ![Test zakończony pomyślnie](unit-testing-signalr-applications/_static/image8.png)
