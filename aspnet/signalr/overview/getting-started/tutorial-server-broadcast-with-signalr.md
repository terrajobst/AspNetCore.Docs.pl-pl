---
uid: signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
title: 'Samouczek: Emisje serwera z użyciem SignalR 2 | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web korzystającą z signalr2 na platformie ASP.NET w celu zapewnienia funkcji emisji serwera.
ms.author: riande
ms.date: 01/02/2019
ms.topic: tutorial
ms.assetid: 1568247f-60b5-4eca-96e0-e661fbb2b273
msc.legacyurl: /signalr/overview/getting-started/tutorial-server-broadcast-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: a6014e604613492db91b2dc6f846c3c73d938d99
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099302"
---
# <a name="tutorial-server-broadcast-with-signalr-2"></a><span data-ttu-id="09f34-103">Samouczek: Serwer emisji z SignalR 2</span><span class="sxs-lookup"><span data-stu-id="09f34-103">Tutorial: Server broadcast with SignalR 2</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="09f34-104">W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web korzystającą z signalr2 na platformie ASP.NET w celu zapewnienia funkcji emisji serwera.</span><span class="sxs-lookup"><span data-stu-id="09f34-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span> <span data-ttu-id="09f34-105">Emisji serwera oznacza, że serwer jest uruchamiany komunikacji klientów.</span><span class="sxs-lookup"><span data-stu-id="09f34-105">Server broadcast means that the server starts the communications sent to clients.</span></span>

<span data-ttu-id="09f34-106">Aplikacja, którą utworzysz w tym samouczku symuluje giełdowej typowy scenariusz emisji funkcje serwera.</span><span class="sxs-lookup"><span data-stu-id="09f34-106">The application that you'll create in this tutorial simulates a stock ticker, a typical scenario for server broadcast functionality.</span></span> <span data-ttu-id="09f34-107">Okresowo serwer losowo aktualizuje giełdowych i aktualizacji można rozgłaszać do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="09f34-107">Periodically, the server randomly updates stock prices and broadcast the updates to all connected clients.</span></span> <span data-ttu-id="09f34-108">W przeglądarce, cyfry i symbole w **zmienić** i **%** kolumn zmieniać dynamicznie w odpowiedzi na powiadomienia z serwera.</span><span class="sxs-lookup"><span data-stu-id="09f34-108">In the browser, the numbers and symbols in the **Change** and **%** columns dynamically change in response to notifications from the server.</span></span> <span data-ttu-id="09f34-109">Jeśli otworzysz dodatkowe przeglądarki pod kątem tego samego adresu URL, wszystkie one pokazywane te same dane i te same zmiany w danych jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="09f34-109">If you open additional browsers to the same URL, they all show the same data and the same changes to the data simultaneously.</span></span>

![Tworzenie sieci web](tutorial-server-broadcast-with-signalr/_static/image1.png)

<span data-ttu-id="09f34-111">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="09f34-111">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="09f34-112">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="09f34-112">Create the project</span></span>
> * <span data-ttu-id="09f34-113">Konfigurowanie kodu serwera</span><span class="sxs-lookup"><span data-stu-id="09f34-113">Set up the server code</span></span>
> * <span data-ttu-id="09f34-114">Badanie kodu serwera</span><span class="sxs-lookup"><span data-stu-id="09f34-114">Examine the server code</span></span>
> * <span data-ttu-id="09f34-115">Ustaw kod klienta</span><span class="sxs-lookup"><span data-stu-id="09f34-115">Set up the client code</span></span>
> * <span data-ttu-id="09f34-116">Badanie kodu klienta</span><span class="sxs-lookup"><span data-stu-id="09f34-116">Examine the client code</span></span>
> * <span data-ttu-id="09f34-117">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="09f34-117">Test the application</span></span>
> * <span data-ttu-id="09f34-118">Włącz rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="09f34-118">Enable logging</span></span>

> [!IMPORTANT]
> <span data-ttu-id="09f34-119">Jeśli nie chcesz pracować, kolejne kroki tworzenia aplikacji, można zainstalować pakietu SignalR.Sample w nowym projekcie pusta aplikacja sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="09f34-119">If you don't want to work through the steps of building the application, you can install the SignalR.Sample package in a new Empty ASP.NET Web Application project.</span></span> <span data-ttu-id="09f34-120">Po zainstalowaniu pakietu NuGet bez wykonywania czynności w ramach tego samouczka należy postępuj zgodnie z instrukcjami w *readme.txt* pliku.</span><span class="sxs-lookup"><span data-stu-id="09f34-120">If you install the NuGet package without performing the steps in this tutorial, you must follow the instructions in the *readme.txt* file.</span></span> <span data-ttu-id="09f34-121">Aby uruchomić pakiet, należy dodać początkowa OWIN klasy która wywołuje metodę `ConfigureSignalR` metody w zainstalowanym pakietem.</span><span class="sxs-lookup"><span data-stu-id="09f34-121">To run the package you need to add an OWIN startup class which calls the `ConfigureSignalR` method in the installed package.</span></span> <span data-ttu-id="09f34-122">Zostanie wyświetlony błąd, jeśli nie dodasz klasy początkowej OWIN.</span><span class="sxs-lookup"><span data-stu-id="09f34-122">You will receive an error if you do not add the OWIN startup class.</span></span> <span data-ttu-id="09f34-123">Zobacz [zainstalować przykład StockTicker](#install-the-stockticker-sample) dalszej części tego artykułu.</span><span class="sxs-lookup"><span data-stu-id="09f34-123">See the [Install the StockTicker sample](#install-the-stockticker-sample) section of this article.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="09f34-124">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="09f34-124">Prerequisites</span></span>

 * <span data-ttu-id="09f34-125">[Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="09f34-125">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="09f34-126">Utwórz projekt</span><span class="sxs-lookup"><span data-stu-id="09f34-126">Create the project</span></span>

<span data-ttu-id="09f34-127">W tej sekcji pokazano, jak utworzyć pustą aplikację sieci Web ASP.NET za pomocą programu Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="09f34-127">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application.</span></span>

1. <span data-ttu-id="09f34-128">W programie Visual Studio należy utworzyć aplikację sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="09f34-128">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Tworzenie sieci web](tutorial-server-broadcast-with-signalr/_static/image2.png)

1. <span data-ttu-id="09f34-130">W **nowej aplikacji sieci Web ASP.NET - SignalR.StockTicker** okna, pozostaw **pusty** zaznaczone, a następnie wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="09f34-130">In the **New ASP.NET Web Application - SignalR.StockTicker** window, leave **Empty** selected and select **OK**.</span></span>

## <a name="set-up-the-server-code"></a><span data-ttu-id="09f34-131">Konfigurowanie kodu serwera</span><span class="sxs-lookup"><span data-stu-id="09f34-131">Set up the server code</span></span>

<span data-ttu-id="09f34-132">W tej sekcji służy do konfigurowania kodu, który działa na serwerze.</span><span class="sxs-lookup"><span data-stu-id="09f34-132">In this section, you set up the code that runs on the server.</span></span>

### <a name="create-the-stock-class"></a><span data-ttu-id="09f34-133">Tworzenie klasy zasobów</span><span class="sxs-lookup"><span data-stu-id="09f34-133">Create the Stock class</span></span>

<span data-ttu-id="09f34-134">Rozpocznij od utworzenia *Stock* modelu klasy, które będzie używane do przechowywania i przesyłania informacji dotyczących danego zasobu.</span><span class="sxs-lookup"><span data-stu-id="09f34-134">You begin by creating the *Stock* model class that you'll use to store and transmit information about a stock.</span></span>

1. <span data-ttu-id="09f34-135">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="09f34-135">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="09f34-136">Nazwa klasy *Stock* i dodaj go do projektu.</span><span class="sxs-lookup"><span data-stu-id="09f34-136">Name the class *Stock* and add it to the project.</span></span>

1. <span data-ttu-id="09f34-137">Zastąp kod w *Stock.cs* pliku przy użyciu tego kodu:</span><span class="sxs-lookup"><span data-stu-id="09f34-137">Replace the code in the *Stock.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample1.cs)]

    <span data-ttu-id="09f34-138">Dwie właściwości, które zostaną ustawione podczas tworzenia zasobów `Symbol` (na przykład MSFT dla firmy Microsoft) i `Price`.</span><span class="sxs-lookup"><span data-stu-id="09f34-138">The two properties that you'll set when you create stocks are `Symbol` (for example, MSFT for Microsoft) and `Price`.</span></span> <span data-ttu-id="09f34-139">Inne właściwości zależą od tego, jak i kiedy ustawisz `Price`.</span><span class="sxs-lookup"><span data-stu-id="09f34-139">The other properties depend on how and when you set `Price`.</span></span> <span data-ttu-id="09f34-140">Możesz ustawić po raz pierwszy `Price`, wartość pobiera propagowane do `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="09f34-140">The first time you set `Price`, the value gets propagated to `DayOpen`.</span></span> <span data-ttu-id="09f34-141">Po tym, gdy ustawisz `Price`, aplikacja oblicza `Change` i `PercentChange` wartości właściwości na podstawie różnicy między `Price` i `DayOpen`.</span><span class="sxs-lookup"><span data-stu-id="09f34-141">After that, when you set `Price`, the app calculates the `Change` and `PercentChange` property values based on the difference between `Price` and `DayOpen`.</span></span>

### <a name="create-the-stocktickerhub-and-stockticker-classes"></a><span data-ttu-id="09f34-142">Tworzenie klasy StockTickerHub i StockTicker</span><span class="sxs-lookup"><span data-stu-id="09f34-142">Create the StockTickerHub and StockTicker classes</span></span>

<span data-ttu-id="09f34-143">Za pomocą interfejsu API Centrum SignalR będzie obsługiwać interakcji z serwera do klienta.</span><span class="sxs-lookup"><span data-stu-id="09f34-143">You'll use the SignalR Hub API to handle server-to-client interaction.</span></span> <span data-ttu-id="09f34-144">A `StockTickerHub` klasę pochodzącą od `SignalRHub` klasy będzie obsługiwać odbieranie wywołań metod i połączenia od klientów.</span><span class="sxs-lookup"><span data-stu-id="09f34-144">A `StockTickerHub` class that derives from the `SignalRHub` class will handle receiving connections and method calls from clients.</span></span> <span data-ttu-id="09f34-145">Należy również utrzymania danych giełdowych i uruchom `Timer` obiektu.</span><span class="sxs-lookup"><span data-stu-id="09f34-145">You also need to maintain stock data and run a `Timer` object.</span></span> <span data-ttu-id="09f34-146">`Timer` Obiektu okresowo spowoduje wyzwolenie aktualizacji cen niezależne od połączeń klientów.</span><span class="sxs-lookup"><span data-stu-id="09f34-146">The `Timer` object will periodically trigger price updates independent of client connections.</span></span> <span data-ttu-id="09f34-147">Nie można umieścić te funkcje w `Hub` klasy, ponieważ przejściowy koncentratorów.</span><span class="sxs-lookup"><span data-stu-id="09f34-147">You can't put these functions in a `Hub` class, because Hubs are transient.</span></span> <span data-ttu-id="09f34-148">Aplikacja tworzy `Hub` wystąpienia klasy dla każdego zadania w Centrum, takich jak połączenia i wywołania od klienta do serwera.</span><span class="sxs-lookup"><span data-stu-id="09f34-148">The app creates a `Hub` class instance for each task on the hub, like connections and calls from the client to the server.</span></span> <span data-ttu-id="09f34-149">Dlatego mechanizm, który przechowuje dane zapasów, aktualizacji cen i emituje aktualizacji cen musi działać w osobnej klasy.</span><span class="sxs-lookup"><span data-stu-id="09f34-149">So the mechanism that keeps stock data, updates prices, and broadcasts the price updates has to run in a separate class.</span></span> <span data-ttu-id="09f34-150">Będzie nazwa klasy `StockTicker`.</span><span class="sxs-lookup"><span data-stu-id="09f34-150">You'll name the class `StockTicker`.</span></span>

![Emisja z StockTicker](tutorial-server-broadcast-with-signalr/_static/image3.png)

<span data-ttu-id="09f34-152">Chcesz tylko jedno wystąpienie `StockTicker` klasy są uruchamiane na serwerze, więc musisz skonfigurować odwołanie z każdej `StockTickerHub` wystąpienia do wzorca singleton `StockTicker` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="09f34-152">You only want one instance of the `StockTicker` class to run on the server, so you'll need to set up a reference from each `StockTickerHub` instance to the singleton `StockTicker` instance.</span></span> <span data-ttu-id="09f34-153">`StockTicker` Klasa ma wysyłać do klientów, ponieważ ma danych podstawowych i wyzwala aktualizacje, ale `StockTicker` nie jest `Hub` klasy.</span><span class="sxs-lookup"><span data-stu-id="09f34-153">The `StockTicker` class has to broadcast to clients because it has the stock data and triggers updates, but `StockTicker` isn't a `Hub` class.</span></span> <span data-ttu-id="09f34-154">`StockTicker` Klasy musi uzyskać odwołanie do obiektu kontekstu połączenia koncentratora SignalR.</span><span class="sxs-lookup"><span data-stu-id="09f34-154">The `StockTicker` class has to get a reference to the SignalR Hub connection context object.</span></span> <span data-ttu-id="09f34-155">Można następnie użyć obiektu context połączenia SignalR do emisji przeznaczonych dla klientów.</span><span class="sxs-lookup"><span data-stu-id="09f34-155">It can then use the SignalR connection context object to broadcast to clients.</span></span>

#### <a name="create-stocktickerhubcs"></a><span data-ttu-id="09f34-156">Utwórz StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="09f34-156">Create StockTickerHub.cs</span></span>

1. <span data-ttu-id="09f34-157">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="09f34-157">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="09f34-158">W **Dodaj nowy element - SignalR.StockTicker**, wybierz opcję **zainstalowane** > **Visual C#**   >  **Web**  >  **SignalR** , a następnie wybierz **klasa Centrum SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="09f34-158">In **Add New Item - SignalR.StockTicker**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="09f34-159">Nazwa klasy *StockTickerHub* i dodaj go do projektu.</span><span class="sxs-lookup"><span data-stu-id="09f34-159">Name the class *StockTickerHub* and add it to the project.</span></span>

    <span data-ttu-id="09f34-160">Spowoduje to utworzenie *StockTickerHub.cs* pliku klasy.</span><span class="sxs-lookup"><span data-stu-id="09f34-160">This step creates the *StockTickerHub.cs* class file.</span></span> <span data-ttu-id="09f34-161">Jednocześnie dodaje zestaw pliki skryptów i odwołania do zestawu, który obsługuje SignalR do projektu.</span><span class="sxs-lookup"><span data-stu-id="09f34-161">Simultaneously, it adds a set of script files and assembly references that supports SignalR to the project.</span></span>

1. <span data-ttu-id="09f34-162">Zastąp kod w *StockTickerHub.cs* pliku przy użyciu tego kodu:</span><span class="sxs-lookup"><span data-stu-id="09f34-162">Replace the code in the *StockTickerHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="09f34-163">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="09f34-163">Save the file.</span></span>

<span data-ttu-id="09f34-164">Ta aplikacja używa [Centrum](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) klasy do definiowania metod klientów można wywołać na serwerze.</span><span class="sxs-lookup"><span data-stu-id="09f34-164">The app uses the [Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx) class to define methods the clients can call on the server.</span></span> <span data-ttu-id="09f34-165">Możesz zdefiniować jedną z metod: `GetAllStocks()`.</span><span class="sxs-lookup"><span data-stu-id="09f34-165">You're defining one method: `GetAllStocks()`.</span></span> <span data-ttu-id="09f34-166">Gdy klient początkowo łączy się z serwerem, wywoła tę metodę, aby uzyskać listę wszystkich zasobów z ich bieżącym ceny.</span><span class="sxs-lookup"><span data-stu-id="09f34-166">When a client initially connects to the server, it will call this method to get a list of all of the stocks with their current prices.</span></span> <span data-ttu-id="09f34-167">Metoda mogą być uruchamiane synchronicznie i zwracać `IEnumerable<Stock>` ponieważ zwraca dane z pamięci.</span><span class="sxs-lookup"><span data-stu-id="09f34-167">The method can run synchronously and return `IEnumerable<Stock>` because it's returning data from memory.</span></span>

<span data-ttu-id="09f34-168">Jeśli metoda musiały uzyskać danych, wykonując coś, co wymagałoby oczekiwania, takich jak wyszukiwania w bazie danych lub wywołanie usługi sieci web należy określić `Task<IEnumerable<Stock>>` jako wartości zwracanej, aby umożliwić przetwarzanie asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="09f34-168">If the method had to get the data by doing something that would involve waiting, like a database lookup or a web service call, you would specify `Task<IEnumerable<Stock>>` as the return value to enable asynchronous processing.</span></span> <span data-ttu-id="09f34-169">Aby uzyskać więcej informacji, zobacz [ASP.NET SignalR Podręcznik interfejsu API centrów — serwer — kiedy są wykonywane asynchronicznie](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span><span class="sxs-lookup"><span data-stu-id="09f34-169">For more information, see [ASP.NET SignalR Hubs API Guide - Server - When to execute asynchronously](../guide-to-the-api/hubs-api-guide-server.md#asyncmethods).</span></span>

<span data-ttu-id="09f34-170">`HubName` Atrybut określa, jak aplikacja będzie odwoływać się w Centrum w kodzie JavaScript na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="09f34-170">The `HubName` attribute specifies how the app will reference the Hub in JavaScript code on the client.</span></span> <span data-ttu-id="09f34-171">Domyślna nazwa na kliencie, jeśli nie korzystasz z tego atrybutu to camelCase wersję nazwy klasy, która w tym przypadku wyniesie `stockTickerHub`.</span><span class="sxs-lookup"><span data-stu-id="09f34-171">The default name on the client if you don't use this attribute, is a camelCase version of the class name, which in this case would be `stockTickerHub`.</span></span>

<span data-ttu-id="09f34-172">Jak zobaczysz później podczas tworzenia `StockTicker` klasy, aplikacja tworzy pojedyncze wystąpienie tej klasy w jego statyczny `Instance` właściwości.</span><span class="sxs-lookup"><span data-stu-id="09f34-172">As you'll see later when you create the `StockTicker` class, the app creates a singleton instance of that class in its static `Instance` property.</span></span> <span data-ttu-id="09f34-173">To wystąpienie singleton `StockTicker` znajduje się w pamięci, niezależnie od tego, ilu klientów łączyć i rozłączać.</span><span class="sxs-lookup"><span data-stu-id="09f34-173">That singleton instance of `StockTicker` is in memory no matter how many clients connect or disconnect.</span></span> <span data-ttu-id="09f34-174">To wystąpienie jest co `GetAllStocks()` metoda używa do zwracania bieżących informacji podstawowych.</span><span class="sxs-lookup"><span data-stu-id="09f34-174">That instance is what the `GetAllStocks()` method uses to return current stock information.</span></span>

#### <a name="create-stocktickercs"></a><span data-ttu-id="09f34-175">Utwórz StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="09f34-175">Create StockTicker.cs</span></span>

1. <span data-ttu-id="09f34-176">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="09f34-176">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="09f34-177">Nazwa klasy *StockTicker* i dodaj go do projektu.</span><span class="sxs-lookup"><span data-stu-id="09f34-177">Name the class *StockTicker* and add it to the project.</span></span>

1. <span data-ttu-id="09f34-178">Zastąp kod w *StockTicker.cs* pliku przy użyciu tego kodu:</span><span class="sxs-lookup"><span data-stu-id="09f34-178">Replace the code in the *StockTicker.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample3.cs)]

<span data-ttu-id="09f34-179">Ponieważ wszystkie wątki będą uruchomione to samo wystąpienie elementu StockTicker kodu, klasa StockTicker musi być metodą o bezpiecznych wątkach.</span><span class="sxs-lookup"><span data-stu-id="09f34-179">Since all threads will be running the same instance of StockTicker code, the StockTicker class has to be thread-safe.</span></span>

### <a name="examine-the-server-code"></a><span data-ttu-id="09f34-180">Badanie kodu serwera</span><span class="sxs-lookup"><span data-stu-id="09f34-180">Examine the server code</span></span>

<span data-ttu-id="09f34-181">Kod serwera podczas badania, pomoże Ci zrozumieć, jak działa aplikacja.</span><span class="sxs-lookup"><span data-stu-id="09f34-181">If you examine the server code, it will help you understand how the app works.</span></span>

#### <a name="storing-the-singleton-instance-in-a-static-field"></a><span data-ttu-id="09f34-182">Przechowywanie pojedyncze wystąpienie, w polu statycznym</span><span class="sxs-lookup"><span data-stu-id="09f34-182">Storing the singleton instance in a static field</span></span>

<span data-ttu-id="09f34-183">Ten kod inicjalizuje statycznej `_instance` pola, która będzie tworzyć kopię `Instance` właściwości przy użyciu wystąpienia klasy.</span><span class="sxs-lookup"><span data-stu-id="09f34-183">The code initializes the static `_instance` field that backs the `Instance` property with an instance of the class.</span></span> <span data-ttu-id="09f34-184">Ponieważ Konstruktor jest prywatny, jest tylko wystąpienia klasy, które można utworzyć aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09f34-184">Because the constructor is private, it's the only instance of the class that the app can create.</span></span> <span data-ttu-id="09f34-185">Ta aplikacja używa [inicjowania z opóźnieniem](/dotnet/framework/performance/lazy-initialization) dla `_instance` pola.</span><span class="sxs-lookup"><span data-stu-id="09f34-185">The app uses [Lazy initialization](/dotnet/framework/performance/lazy-initialization) for the `_instance` field.</span></span> <span data-ttu-id="09f34-186">Nie jest ze względu na wydajność.</span><span class="sxs-lookup"><span data-stu-id="09f34-186">It's not for performance reasons.</span></span> <span data-ttu-id="09f34-187">Jest upewnij się, że tworzenie wystąpienia jest bezpieczna dla wątków.</span><span class="sxs-lookup"><span data-stu-id="09f34-187">It's to make sure the instance creation is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample4.cs)]

<span data-ttu-id="09f34-188">Każdorazowo, klient nawiąże połączenie z serwerem, nowe wystąpienie klasy StockTickerHub działające w oddzielnym wątku pobiera StockTicker pojedyncze wystąpienie z `StockTicker.Instance` właściwość statyczna jak pokazano wcześniej w `StockTickerHub` klasy.</span><span class="sxs-lookup"><span data-stu-id="09f34-188">Each time a client connects to the server, a new instance of the StockTickerHub class running in a separate thread gets the StockTicker singleton instance from the `StockTicker.Instance` static property, as you saw earlier in the `StockTickerHub` class.</span></span>

#### <a name="storing-stock-data-in-a-concurrentdictionary"></a><span data-ttu-id="09f34-189">Przechowywanie danych giełdowych w ConcurrentDictionary</span><span class="sxs-lookup"><span data-stu-id="09f34-189">Storing stock data in a ConcurrentDictionary</span></span>

<span data-ttu-id="09f34-190">Konstruktor inicjuje `_stocks` kolekcji z pewnymi przykładowymi danymi zapasów i `GetAllStocks` zwraca zasobów.</span><span class="sxs-lookup"><span data-stu-id="09f34-190">The constructor initializes the `_stocks` collection with some sample stock data, and `GetAllStocks` returns the stocks.</span></span> <span data-ttu-id="09f34-191">Jak wcześniej, to zbiór zasobów jest zwracany przez `StockTickerHub.GetAllStocks`, czyli metody serwera w `Hub` klasę, która może wywołać klientów.</span><span class="sxs-lookup"><span data-stu-id="09f34-191">As you saw earlier, this collection of stocks is returned by `StockTickerHub.GetAllStocks`, which is a server method in the `Hub` class that clients can call.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample5.cs)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample6.cs)]

<span data-ttu-id="09f34-192">Kolekcja zasobów jest zdefiniowana jako [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) typu pod kątem bezpieczeństwa wątków.</span><span class="sxs-lookup"><span data-stu-id="09f34-192">The stocks collection is defined as a [ConcurrentDictionary](https://msdn.microsoft.com/library/dd287191.aspx) type for thread safety.</span></span> <span data-ttu-id="09f34-193">Alternatywnie, można użyć [słownika](https://msdn.microsoft.com/library/xfhwa508.aspx) obiektu i jawnie zablokować słownika, po wprowadzeniu zmian do niego.</span><span class="sxs-lookup"><span data-stu-id="09f34-193">As an alternative, you could use a [Dictionary](https://msdn.microsoft.com/library/xfhwa508.aspx) object and explicitly lock the dictionary when you make changes to it.</span></span>

<span data-ttu-id="09f34-194">Ta przykładowa aplikacja OK do przechowywania danych aplikacji w pamięci i jest do utraty danych, gdy aplikacja usuwa `StockTicker` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="09f34-194">For this sample application, it's OK to store application data in memory and to lose the data when the app disposes of the  `StockTicker` instance.</span></span> <span data-ttu-id="09f34-195">W rzeczywistej aplikacji będzie działać z magazynem danych zaplecza, takich jak bazy danych.</span><span class="sxs-lookup"><span data-stu-id="09f34-195">In a real application, you would work with a back-end data store like a database.</span></span>

#### <a name="periodically-updating-stock-prices"></a><span data-ttu-id="09f34-196">Okresowo aktualizowanie cen akcji</span><span class="sxs-lookup"><span data-stu-id="09f34-196">Periodically updating stock prices</span></span>

<span data-ttu-id="09f34-197">Konstruktor jest uruchamiany `Timer` obiekt, który okresowo wywołuje metody, które aktualizują cen akcji na podstawie losowej.</span><span class="sxs-lookup"><span data-stu-id="09f34-197">The constructor starts up a `Timer` object that periodically calls methods that update stock prices on a random basis.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample7.cs)]

 <span data-ttu-id="09f34-198">`Timer` wywołania `UpdateStockPrices`, które przechodzą w wartości null w parametrze state.</span><span class="sxs-lookup"><span data-stu-id="09f34-198">`Timer` calls `UpdateStockPrices`, which passes in null in the state parameter.</span></span> <span data-ttu-id="09f34-199">Przed zaktualizowaniem ceny, aplikacja przejmuje blokadę `_updateStockPricesLock` obiektu.</span><span class="sxs-lookup"><span data-stu-id="09f34-199">Before updating prices, the app takes a lock on the `_updateStockPricesLock` object.</span></span> <span data-ttu-id="09f34-200">Sprawdza kod, jeśli inny wątek już trwa aktualizowanie cen, a następnie wywołuje `TryUpdateStockPrice` na każdej akcji, na liście.</span><span class="sxs-lookup"><span data-stu-id="09f34-200">The code checks if another thread is already updating prices, and then it calls `TryUpdateStockPrice` on each stock in the list.</span></span> <span data-ttu-id="09f34-201">`TryUpdateStockPrice` Metoda określa, czy należy zmienić najniższej ceny i ilości go zmienić.</span><span class="sxs-lookup"><span data-stu-id="09f34-201">The `TryUpdateStockPrice` method decides whether to change the stock price, and how much to change it.</span></span> <span data-ttu-id="09f34-202">Jeśli zmieni się cena akcji w aplikacji jest nazywana `BroadcastStockPrice` do emisji przeznaczonych jest zmiana najniższej ceny akcji dla wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="09f34-202">If the stock price changes, the app calls `BroadcastStockPrice` to broadcast the stock price change to all connected clients.</span></span>

<span data-ttu-id="09f34-203">`_updatingStockPrices` Flagi wyznaczony [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) aby upewnić się, jest metodą o bezpiecznych wątkach.</span><span class="sxs-lookup"><span data-stu-id="09f34-203">The `_updatingStockPrices` flag designated [volatile](https://msdn.microsoft.com/library/x13ttww7.aspx) to make sure it is thread-safe.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample8.cs)]

<span data-ttu-id="09f34-204">W rzeczywistej aplikacji `TryUpdateStockPrice` metoda będzie wywołać usługę sieci web, aby wyszukać cena.</span><span class="sxs-lookup"><span data-stu-id="09f34-204">In a real application, the `TryUpdateStockPrice` method would call a web service to look up the price.</span></span> <span data-ttu-id="09f34-205">W tym kodzie aplikacja używa generator liczb losowych, aby wprowadzić zmiany w losowo.</span><span class="sxs-lookup"><span data-stu-id="09f34-205">In this code, the app uses a random number generator to make changes randomly.</span></span>

#### <a name="getting-the-signalr-context-so-that-the-stockticker-class-can-broadcast-to-clients"></a><span data-ttu-id="09f34-206">Wprowadzenie kontekstu SignalR, tak aby klasy StockTicker można rozgłaszać do klientów</span><span class="sxs-lookup"><span data-stu-id="09f34-206">Getting the SignalR context so that the StockTicker class can broadcast to clients</span></span>

<span data-ttu-id="09f34-207">Ponieważ zmiany cen poniżej pochodzą z `StockTicker` obiektu, jest to obiekt, który musi wywołać `updateStockPrice` metody na wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="09f34-207">Because the price changes originate here in the `StockTicker` object, it's the object that needs to call an `updateStockPrice` method on all connected clients.</span></span> <span data-ttu-id="09f34-208">W `Hub` klasy, masz interfejs API do wywoływania metody klientów, ale `StockTicker` nie pochodzi od `Hub` klasy, a nie ma odniesienia do dowolnych `Hub` obiektu.</span><span class="sxs-lookup"><span data-stu-id="09f34-208">In a `Hub` class, you have an API for calling client methods, but `StockTicker` doesn't derive from the `Hub` class and doesn't have a reference to any `Hub` object.</span></span> <span data-ttu-id="09f34-209">Do emisji przeznaczonych do połączonych klientów `StockTicker` klasa ma wystąpienia kontekstu SignalR dla `StockTickerHub` klasy i używać go do wywoływania metod na komputerach klienckich.</span><span class="sxs-lookup"><span data-stu-id="09f34-209">To broadcast to connected clients, the `StockTicker` class has to get the SignalR context instance for the `StockTickerHub` class and use that to call methods on clients.</span></span>

<span data-ttu-id="09f34-210">Ten kod pobiera odwołanie do kontekstu SignalR, podczas tworzenia wystąpienia klasy pojedyncze, przebiegi, które odwołują się do konstruktora, i umieszcza go w Konstruktorze `Clients` właściwości.</span><span class="sxs-lookup"><span data-stu-id="09f34-210">The code gets a reference to the SignalR context when it creates the singleton class instance, passes that reference to the constructor, and the constructor puts it in the `Clients` property.</span></span>

<span data-ttu-id="09f34-211">Istnieją dwie przyczyny, dlaczego chcesz uzyskać kontekst tylko raz: Pobieranie kontekstu jest kosztowne zadania oraz pobierania ich po sprawia, że się, że aplikacja zachowuje zalecanej kolejności wiadomości wysyłane do klientów.</span><span class="sxs-lookup"><span data-stu-id="09f34-211">There are two reasons why you want to get the context only once: getting the context is an expensive task, and getting it once makes sure the app preserves the intended order of messages sent to the clients.</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample9.cs)]

<span data-ttu-id="09f34-212">Wprowadzenie `Clients` właściwość kontekstu i umieszczenie go w `StockTickerClient` właściwość umożliwia pisanie kodu w celu wywołania metody klienta, który wygląda tak samo jak w `Hub` klasy.</span><span class="sxs-lookup"><span data-stu-id="09f34-212">Getting the `Clients` property of the context and putting it in the `StockTickerClient` property lets you write code to call client methods that looks the same as it would in a `Hub` class.</span></span> <span data-ttu-id="09f34-213">Na przykład napisać rozgłaszać do wszystkich klientów `Clients.All.updateStockPrice(stock)`.</span><span class="sxs-lookup"><span data-stu-id="09f34-213">For instance, to broadcast to all clients you can write `Clients.All.updateStockPrice(stock)`.</span></span>

<span data-ttu-id="09f34-214">`updateStockPrice` Metodę, która jest wywoływana w `BroadcastStockPrice` jeszcze nie istnieje.</span><span class="sxs-lookup"><span data-stu-id="09f34-214">The `updateStockPrice` method that you're calling in `BroadcastStockPrice` doesn't exist yet.</span></span> <span data-ttu-id="09f34-215">Należy dodać go później podczas pisania kodu, który jest uruchamiany na kliencie.</span><span class="sxs-lookup"><span data-stu-id="09f34-215">You'll add it later when you write code that runs on the client.</span></span> <span data-ttu-id="09f34-216">Możesz zapoznać się z `updateStockPrice` tutaj ponieważ `Clients.All` jest dynamiczną, która oznacza, że aplikacja będzie oceniać wyrażenia w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="09f34-216">You can refer to `updateStockPrice` here because `Clients.All` is dynamic, which means the app will evaluate the expression at runtime.</span></span> <span data-ttu-id="09f34-217">Po wykonaniu wywołanie tej metody, SignalR wyśle nazwy metody i wartość parametru do klienta, a jeśli klient ma metodę o nazwie `updateStockPrice`, aplikacja wywoła tę metodę i przekaż wartość parametru do niego.</span><span class="sxs-lookup"><span data-stu-id="09f34-217">When this method call executes, SignalR will send the method name and the parameter value to the client, and if the client has a method named `updateStockPrice`, the app will call that method and pass the parameter value to it.</span></span>

<span data-ttu-id="09f34-218">`Clients.All` oznacza, że wysyłać do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="09f34-218">`Clients.All` means send to all clients.</span></span> <span data-ttu-id="09f34-219">SignalR udostępnia innych opcji, aby określić, które klientów lub grup klientów, aby wysyłać.</span><span class="sxs-lookup"><span data-stu-id="09f34-219">SignalR gives you other options to specify which clients or groups of clients to send to.</span></span> <span data-ttu-id="09f34-220">Aby uzyskać więcej informacji, zobacz [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span><span class="sxs-lookup"><span data-stu-id="09f34-220">For more information, see [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx).</span></span>

### <a name="register-the-signalr-route"></a><span data-ttu-id="09f34-221">Zarejestruj trasy SignalR</span><span class="sxs-lookup"><span data-stu-id="09f34-221">Register the SignalR route</span></span>

<span data-ttu-id="09f34-222">Serwer musi znać adres URL, który można przechwycić i kierują je bezpośrednio z SignalR.</span><span class="sxs-lookup"><span data-stu-id="09f34-222">The server needs to know which URL to intercept and direct to SignalR.</span></span> <span data-ttu-id="09f34-223">Aby to zrobić, Dodaj klasę początkową OWIN:</span><span class="sxs-lookup"><span data-stu-id="09f34-223">To do that, add an OWIN startup class:</span></span>

1. <span data-ttu-id="09f34-224">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="09f34-224">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="09f34-225">W **Dodaj nowy element - SignalR.StockTicker** wybierz **zainstalowane** > **Visual C#**   >  **Web** i następnie wybierz pozycję **klasy początkowej OWIN**.</span><span class="sxs-lookup"><span data-stu-id="09f34-225">In **Add New Item - SignalR.StockTicker** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="09f34-226">Nazwa klasy *uruchamiania* i wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="09f34-226">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="09f34-227">Zastąp kod domyślne *Startup.cs* pliku przy użyciu tego kodu:</span><span class="sxs-lookup"><span data-stu-id="09f34-227">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample10.cs)]

<span data-ttu-id="09f34-228">Konfigurowanie kodu serwera zostało zakończone.</span><span class="sxs-lookup"><span data-stu-id="09f34-228">You have now finished setting up the server code.</span></span> <span data-ttu-id="09f34-229">W następnej sekcji należy skonfigurować klienta.</span><span class="sxs-lookup"><span data-stu-id="09f34-229">In the next section, you'll set up the client.</span></span>

## <a name="set-up-the-client-code"></a><span data-ttu-id="09f34-230">Ustaw kod klienta</span><span class="sxs-lookup"><span data-stu-id="09f34-230">Set up the client code</span></span>

<span data-ttu-id="09f34-231">W tej sekcji służy do konfigurowania kod, który jest uruchamiany na kliencie.</span><span class="sxs-lookup"><span data-stu-id="09f34-231">In this section, you set up the code that runs on the client.</span></span>

### <a name="create-the-html-page-and-javascript-file"></a><span data-ttu-id="09f34-232">Utwórz stronę HTML i plik JavaScript</span><span class="sxs-lookup"><span data-stu-id="09f34-232">Create the HTML page and JavaScript file</span></span>

<span data-ttu-id="09f34-233">Strony HTML będą wyświetlane dane i plik JavaScript będzie organizowania danych.</span><span class="sxs-lookup"><span data-stu-id="09f34-233">The HTML page will display the data and the JavaScript file will organize the data.</span></span>

#### <a name="create-stocktickerhtml"></a><span data-ttu-id="09f34-234">Utwórz StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="09f34-234">Create StockTicker.html</span></span>

<span data-ttu-id="09f34-235">Najpierw należy dodać klienta HTML.</span><span class="sxs-lookup"><span data-stu-id="09f34-235">First, you'll add the HTML client.</span></span>

1. <span data-ttu-id="09f34-236">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **strony HTML**.</span><span class="sxs-lookup"><span data-stu-id="09f34-236">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="09f34-237">Nadaj plikowi nazwę *StockTicker* i wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="09f34-237">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="09f34-238">Zastąp kod domyślne *StockTicker.html* pliku przy użyciu tego kodu:</span><span class="sxs-lookup"><span data-stu-id="09f34-238">Replace the default code in the *StockTicker.html* file with this code:</span></span>

    [!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample11.html?highlight=40-43)]

    <span data-ttu-id="09f34-239">Kod HTML tworzy tabelę z pięciu kolumn, wiersz nagłówka i wiersz danych z pojedynczą komórkę, która obejmuje wszystkie kolumny pięć.</span><span class="sxs-lookup"><span data-stu-id="09f34-239">The HTML creates a table with five columns, a header row, and a data row with a single cell that spans all five columns.</span></span> <span data-ttu-id="09f34-240">Wiersz danych zawiera "Trwa ładowanie..." chwilowo, po uruchomieniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09f34-240">The data row shows "loading..." momentarily when the app starts.</span></span> <span data-ttu-id="09f34-241">Kod JavaScript spowoduje usunięcie tego wiersza i dodać w jej miejscu wiersze z danych giełdowych pobrany z serwera.</span><span class="sxs-lookup"><span data-stu-id="09f34-241">JavaScript code will remove that row and add in its place rows with stock data retrieved from the server.</span></span>

    <span data-ttu-id="09f34-242">Określ tagi skryptu:</span><span class="sxs-lookup"><span data-stu-id="09f34-242">The script tags specify:</span></span>

    * <span data-ttu-id="09f34-243">Plik skryptu jQuery.</span><span class="sxs-lookup"><span data-stu-id="09f34-243">The jQuery script file.</span></span>

    * <span data-ttu-id="09f34-244">Plik skryptu core SignalR.</span><span class="sxs-lookup"><span data-stu-id="09f34-244">The SignalR core script file.</span></span>

    * <span data-ttu-id="09f34-245">Plik skryptu proxy SignalR.</span><span class="sxs-lookup"><span data-stu-id="09f34-245">The SignalR proxies script file.</span></span>

    * <span data-ttu-id="09f34-246">Plik skryptu StockTicker, którą utworzysz później.</span><span class="sxs-lookup"><span data-stu-id="09f34-246">A StockTicker script file that you'll create later.</span></span>

    <span data-ttu-id="09f34-247">Aplikacja dynamicznie generuje plik skryptu proxy SignalR.</span><span class="sxs-lookup"><span data-stu-id="09f34-247">The app dynamically generates the SignalR proxies script file.</span></span> <span data-ttu-id="09f34-248">Określa adres URL "/ signalr/centra" i definiuje metody serwera proxy dla metody na klasy koncentratora, w tym przypadku `StockTickerHub.GetAllStocks`.</span><span class="sxs-lookup"><span data-stu-id="09f34-248">It specifies the "/signalr/hubs" URL and defines proxy methods for the methods on the Hub class, in this case, for `StockTickerHub.GetAllStocks`.</span></span> <span data-ttu-id="09f34-249">Jeśli wolisz, możesz wygenerować ten plik JavaScript ręcznie przy użyciu [narzędzia SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span><span class="sxs-lookup"><span data-stu-id="09f34-249">If you prefer, you can generate this JavaScript file manually by using [SignalR Utilities](http://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/).</span></span> <span data-ttu-id="09f34-250">Nie należy zapominać wyłączyć tworzenie dynamicznych plików w `MapHubs` wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="09f34-250">Don't forget to disable dynamic file creation in the `MapHubs` method call.</span></span>

1. <span data-ttu-id="09f34-251">W **Eksploratora rozwiązań**, rozwiń węzeł **skrypty**.</span><span class="sxs-lookup"><span data-stu-id="09f34-251">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="09f34-252">Biblioteki skryptów, jQuery i SignalR są widoczne w projekcie.</span><span class="sxs-lookup"><span data-stu-id="09f34-252">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="09f34-253">Menedżer pakietów zainstaluje późniejszą wersję skrypty SignalR.</span><span class="sxs-lookup"><span data-stu-id="09f34-253">The package manager will install a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="09f34-254">Aktualizuj odwołania do skryptu w bloku kodu, aby odpowiadać wersji plików skrypt w projekcie.</span><span class="sxs-lookup"><span data-stu-id="09f34-254">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

1. <span data-ttu-id="09f34-255">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *StockTicker.html*, a następnie wybierz pozycję **Ustaw jako strona startowa**.</span><span class="sxs-lookup"><span data-stu-id="09f34-255">In **Solution Explorer**, right-click *StockTicker.html*, and then select **Set as Start Page**.</span></span>

#### <a name="create-stocktickerjs"></a><span data-ttu-id="09f34-256">Utwórz StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="09f34-256">Create StockTicker.js</span></span>

<span data-ttu-id="09f34-257">Teraz można utworzyć pliku JavaScript.</span><span class="sxs-lookup"><span data-stu-id="09f34-257">Now create the JavaScript file.</span></span>

1. <span data-ttu-id="09f34-258">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **plik JavaScript**.</span><span class="sxs-lookup"><span data-stu-id="09f34-258">In **Solution Explorer**, right-click the project and select **Add** > **JavaScript File**.</span></span>

1. <span data-ttu-id="09f34-259">Nadaj plikowi nazwę *StockTicker* i wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="09f34-259">Name the file *StockTicker* and select **OK**.</span></span>

1. <span data-ttu-id="09f34-260">Dodaj następujący kod do *StockTicker.js* pliku:</span><span class="sxs-lookup"><span data-stu-id="09f34-260">Add this code to the *StockTicker.js* file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample12.js)]

### <a name="examine-the-client-code"></a><span data-ttu-id="09f34-261">Badanie kodu klienta</span><span class="sxs-lookup"><span data-stu-id="09f34-261">Examine the client code</span></span>

<span data-ttu-id="09f34-262">Jeśli możesz zbadać kod klienta, pomoże Ci dowiedzieć się, jak kod klienta współdziała z kod serwera, aby aplikacja pracuje.</span><span class="sxs-lookup"><span data-stu-id="09f34-262">If you examine the client code, it will help you learn how the client code interacts with the server code to make the app work.</span></span>

#### <a name="starting-the-connection"></a><span data-ttu-id="09f34-263">Zainicjowanie połączenia</span><span class="sxs-lookup"><span data-stu-id="09f34-263">Starting the connection</span></span>

<span data-ttu-id="09f34-264">`$.connection` odnosi się do serwerów proxy SignalR.</span><span class="sxs-lookup"><span data-stu-id="09f34-264">`$.connection` refers to the SignalR proxies.</span></span> <span data-ttu-id="09f34-265">Ten kod pobiera odwołanie do serwera proxy dla `StockTickerHub` klasy i umieszcza go w `ticker` zmiennej.</span><span class="sxs-lookup"><span data-stu-id="09f34-265">The code gets a reference to the proxy for the `StockTickerHub` class and puts it in the `ticker` variable.</span></span> <span data-ttu-id="09f34-266">Nazwa serwera proxy jest nazwa, która została ustawiona przez `HubName` atrybutu:</span><span class="sxs-lookup"><span data-stu-id="09f34-266">The proxy name is the name that was set by the `HubName` attribute:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample13.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample14.cs)]

<span data-ttu-id="09f34-267">Po zdefiniowaniu wszystkich zmiennych i funkcji, ostatni wiersz kodu w pliku inicjuje połączenia SignalR, wywołując SignalR `start` funkcji.</span><span class="sxs-lookup"><span data-stu-id="09f34-267">After you define all the variables and functions, the last line of code in the file initializes the SignalR connection by calling the SignalR `start` function.</span></span> <span data-ttu-id="09f34-268">`start` Funkcja wykonuje asynchronicznie i zwraca [jQuery opóźnionych obiektu](http://api.jquery.com/category/deferred-object/).</span><span class="sxs-lookup"><span data-stu-id="09f34-268">The `start` function executes asynchronously and returns a [jQuery Deferred object](http://api.jquery.com/category/deferred-object/).</span></span> <span data-ttu-id="09f34-269">Można wywołać funkcję gotowe do określenia funkcji do wywołania po jej zakończeniu akcję asynchroniczną.</span><span class="sxs-lookup"><span data-stu-id="09f34-269">You can call the done function to specify the function to call when the app finishes the asynchronous action.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample15.js)]

#### <a name="getting-all-the-stocks"></a><span data-ttu-id="09f34-270">Wprowadzenie do zasobów</span><span class="sxs-lookup"><span data-stu-id="09f34-270">Getting all the stocks</span></span>

<span data-ttu-id="09f34-271">`init` Wywołaniach funkcji `getAllStocks` funkcji na serwerze i używa tych informacji, serwer zwraca można zaktualizować podstawowego tabeli.</span><span class="sxs-lookup"><span data-stu-id="09f34-271">The `init` function calls the `getAllStocks` function on the server and uses the information that the server returns to update the stock table.</span></span> <span data-ttu-id="09f34-272">Należy zauważyć, że domyślnie, trzeba użyć camelCasing na kliencie, nawet jeśli nazwa metody jest pascal — z uwzględnieniem wielkości liter na serwerze.</span><span class="sxs-lookup"><span data-stu-id="09f34-272">Notice that, by default, you have to use camelCasing on the client even though the method name is pascal-cased on the server.</span></span> <span data-ttu-id="09f34-273">Reguła camelCasing dotyczy tylko metody, a nie obiektów.</span><span class="sxs-lookup"><span data-stu-id="09f34-273">The camelCasing rule only applies to methods, not objects.</span></span> <span data-ttu-id="09f34-274">Na przykład, możesz odwołać się do `stock.Symbol` i `stock.Price`, a nie `stock.symbol` lub `stock.price`.</span><span class="sxs-lookup"><span data-stu-id="09f34-274">For example, you refer to `stock.Symbol` and `stock.Price`, not `stock.symbol` or `stock.price`.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample16.js)]

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample17.cs)]

<span data-ttu-id="09f34-275">W `init` metody, aplikacja tworzy HTML dla wiersza tabeli, dla każdego obiektu podstawowego otrzymany z serwera, wywołując `formatStock` format właściwości `stock` obiektu, a następnie przez wywołanie `supplant` zastąpić symbole zastępcze w wywołaniach `rowTemplate` zmiennej za pomocą `stock` wartości właściwości obiektu.</span><span class="sxs-lookup"><span data-stu-id="09f34-275">In the `init` method, the app creates HTML for a table row for each stock object received from the server by calling `formatStock` to format properties of the `stock` object, and then by calling `supplant` to replace placeholders in the `rowTemplate` variable with the `stock` object property values.</span></span> <span data-ttu-id="09f34-276">Wynikowy kod HTML, następnie jest dołączany do podstawowego tabeli.</span><span class="sxs-lookup"><span data-stu-id="09f34-276">The resulting HTML is then appended to the stock table.</span></span>

> [!NOTE]
> <span data-ttu-id="09f34-277">Należy wywołać `init` przez przekazanie jej jako `callback` funkcji, który jest wykonywany po asynchroniczną `start` funkcji zostanie zakończone.</span><span class="sxs-lookup"><span data-stu-id="09f34-277">You call `init` by passing it in as a `callback` function that executes after the asynchronous `start` function finishes.</span></span> <span data-ttu-id="09f34-278">Jeśli wywołujesz `init` jako osobne instrukcja kodu JavaScript po wywołaniu `start`, funkcja może zakończyć się niepowodzeniem, ponieważ aplikacja może działać natychmiast bez oczekiwania na funkcję start, aby zakończyć ustanawiania połączenia.</span><span class="sxs-lookup"><span data-stu-id="09f34-278">If you called `init` as a separate JavaScript statement after calling `start`, the function would fail because it would run immediately without waiting for the start function to finish establishing the connection.</span></span> <span data-ttu-id="09f34-279">W takim przypadku `init` próbowała funkcja do wywołania `getAllStocks` działać, zanim aplikacja nawiązuje połączenie z serwerem.</span><span class="sxs-lookup"><span data-stu-id="09f34-279">In that case, the `init` function would try to call the `getAllStocks` function before the app establishes a server connection.</span></span>

#### <a name="getting-updated-stock-prices"></a><span data-ttu-id="09f34-280">Pobieranie zaktualizowanego cen akcji</span><span class="sxs-lookup"><span data-stu-id="09f34-280">Getting updated stock prices</span></span>

<span data-ttu-id="09f34-281">Po zmianie cen zasobów serwera wywoływanych przez nią `updateStockPrice` na połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="09f34-281">When the server changes a stock's price, it calls the `updateStockPrice` on connected clients.</span></span> <span data-ttu-id="09f34-282">Aplikacja dodaje funkcję do właściwości klienta `stockTicker` serwera proxy, aby udostępnić ją do wywołań z serwera.</span><span class="sxs-lookup"><span data-stu-id="09f34-282">The app adds the function to the client property of the `stockTicker` proxy to make it available to calls from the server.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample18.js)]

<span data-ttu-id="09f34-283">`updateStockPrice` Formaty funkcja obiektem magazynowym otrzymany z serwera do tabeli Wiersz taki sam sposób jak w `init` funkcji.</span><span class="sxs-lookup"><span data-stu-id="09f34-283">The `updateStockPrice` function formats a stock object received from the server into a table row the same way as in the `init` function.</span></span> <span data-ttu-id="09f34-284">Zamiast dołączać wiersz do tabeli, wyszukuje stock bieżącego wiersza w tabeli i zamienia nowego wiersza.</span><span class="sxs-lookup"><span data-stu-id="09f34-284">Instead of appending the row to the table, it finds the stock's current row in the table and replaces that row with the new one.</span></span>

## <a name="test-the-application"></a><span data-ttu-id="09f34-285">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="09f34-285">Test the application</span></span>

<span data-ttu-id="09f34-286">Można przetestować aplikację, aby upewnić się, że działa.</span><span class="sxs-lookup"><span data-stu-id="09f34-286">You can test the app to make sure it's working.</span></span> <span data-ttu-id="09f34-287">Zostaną wyświetlone wszystkie okna przeglądarki, Wyświetl tabeli podstawowe na żywo z cen akcji zmienne.</span><span class="sxs-lookup"><span data-stu-id="09f34-287">You'll see all browser windows display the live stock table with stock prices fluctuating.</span></span>

1. <span data-ttu-id="09f34-288">Na pasku narzędzi, Włącz **debugowanie skryptu** a następnie wybierz przycisk Odtwórz, aby uruchomić aplikację w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="09f34-288">In the toolbar, turn on **Script Debugging** and then select the play button to run the app in Debug mode.</span></span>

    ![Zrzut ekranu przedstawiający użytkownika, włączając tryb debugowania i wybierając polecenie play.](tutorial-server-broadcast-with-signalr/_static/image4.png)

    <span data-ttu-id="09f34-290">Zostanie otwarte okno przeglądarki, wyświetlanie **Live tabeli Stock**.</span><span class="sxs-lookup"><span data-stu-id="09f34-290">A browser window will open displaying the **Live Stock Table**.</span></span> <span data-ttu-id="09f34-291">Tabela podstawowych początkowo jest widocznych wiersza "Trwa ładowanie...", następnie po pewnym czasie aplikacja będzie wyświetlana początkowej danych podstawowych i zacznij cen akcji można zmienić.</span><span class="sxs-lookup"><span data-stu-id="09f34-291">The stock table initially shows the "loading..." line, then, after a short time, the app shows the initial stock data, and then the stock prices start to change.</span></span>

1. <span data-ttu-id="09f34-292">Skopiuj adres URL w przeglądarce otwórz dwóch innych przeglądarek i wklejać adresy URL paski adresu.</span><span class="sxs-lookup"><span data-stu-id="09f34-292">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

    <span data-ttu-id="09f34-293">Początkowe wyświetlanie podstawowych jest taki sam jak pierwszy przeglądarki i jednocześnie wprowadzenia zmiany.</span><span class="sxs-lookup"><span data-stu-id="09f34-293">The initial stock display is the same as the first browser and changes happen simultaneously.</span></span>

1. <span data-ttu-id="09f34-294">Zamknij wszystkie przeglądarki, Otwórz w nowym oknie przeglądarki i przejdź do tego samego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="09f34-294">Close all browsers, open a new browser, and go to the same URL.</span></span>

    <span data-ttu-id="09f34-295">Nadal StockTicker pojedynczego obiektu do uruchomienia na serwerze.</span><span class="sxs-lookup"><span data-stu-id="09f34-295">The StockTicker singleton object continued to run in the server.</span></span> <span data-ttu-id="09f34-296">**Live tabeli Stock** pokazuje, które nadal zasobów można zmienić.</span><span class="sxs-lookup"><span data-stu-id="09f34-296">The **Live Stock Table** shows that the stocks have continued to change.</span></span> <span data-ttu-id="09f34-297">Nie widzisz początkowa tabela o wartości zero, zmień wartości.</span><span class="sxs-lookup"><span data-stu-id="09f34-297">You don't see the initial table with zero change figures.</span></span>

1. <span data-ttu-id="09f34-298">Zamknij przeglądarkę.</span><span class="sxs-lookup"><span data-stu-id="09f34-298">Close the browser.</span></span>

## <a name="enable-logging"></a><span data-ttu-id="09f34-299">Włącz rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="09f34-299">Enable logging</span></span>

<span data-ttu-id="09f34-300">SignalR ma funkcję wbudowane funkcje rejestrowania, który można włączyć na kliencie, aby ułatwić rozwiązywanie problemów.</span><span class="sxs-lookup"><span data-stu-id="09f34-300">SignalR has a built-in logging function that you can enable on the client to aid in troubleshooting.</span></span> <span data-ttu-id="09f34-301">W tej sekcji, Włącz rejestrowanie i zapoznaj się z przykładami, które pokazują, jak dzienniki informujące, który z poniższych metod transportu używa SignalR:</span><span class="sxs-lookup"><span data-stu-id="09f34-301">In this section, you enable logging and see examples that show how logs tell you which of the following transport methods SignalR is using:</span></span>

* <span data-ttu-id="09f34-302">[Gniazda Websocket](http://en.wikipedia.org/wiki/WebSocket)obsługiwany przez usługi IIS 8 i bieżącej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="09f34-302">[WebSockets](http://en.wikipedia.org/wiki/WebSocket), supported by IIS 8 and current browsers.</span></span>

* <span data-ttu-id="09f34-303">[Serwer wysłał zdarzenia](http://en.wikipedia.org/wiki/Server-sent_events), obsługiwane przez przeglądarki innej niż Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="09f34-303">[Server-sent events](http://en.wikipedia.org/wiki/Server-sent_events), supported by browsers other than Internet Explorer.</span></span>

* <span data-ttu-id="09f34-304">[Nieskończona ramki](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe)obsługiwany przez program Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="09f34-304">[Forever frame](http://en.wikipedia.org/wiki/Comet_(programming)#Hidden_iframe), supported by Internet Explorer.</span></span>

* <span data-ttu-id="09f34-305">[AJAX długim sondowaniem](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling)obsługiwany przez wszystkie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="09f34-305">[Ajax long polling](http://en.wikipedia.org/wiki/Comet_(programming)#Ajax_with_long_polling), supported by all browsers.</span></span>

<span data-ttu-id="09f34-306">Dla dowolnego danego połączenia SignalR wybiera najlepszą metodą transportu, który obsługuje zarówno na serwerze, jak i klienta.</span><span class="sxs-lookup"><span data-stu-id="09f34-306">For any given connection, SignalR chooses the best transport method that both the server and the client support.</span></span>

1. <span data-ttu-id="09f34-307">Otwórz *StockTicker.js*.</span><span class="sxs-lookup"><span data-stu-id="09f34-307">Open *StockTicker.js*.</span></span>

1. <span data-ttu-id="09f34-308">Dodaj ten wyróżniony wiersz kodu, aby włączyć rejestrowanie bezpośrednio przed kod, który inicjuje połączenie z końcem pliku:</span><span class="sxs-lookup"><span data-stu-id="09f34-308">Add this highlighted line of code to enable logging immediately before the code that initializes the connection at the end of the file:</span></span>

    [!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample19.js?highlight=2)]

1. <span data-ttu-id="09f34-309">Naciśnij klawisz **F5** Aby uruchomić projekt.</span><span class="sxs-lookup"><span data-stu-id="09f34-309">Press **F5** to run the project.</span></span>

1. <span data-ttu-id="09f34-310">Otwórz okno narzędzi programistycznych w przeglądarce, a następnie wybierz konsolę, aby wyświetlić dzienniki.</span><span class="sxs-lookup"><span data-stu-id="09f34-310">Open your browser's developer tools window, and select the Console to see the logs.</span></span> <span data-ttu-id="09f34-311">Trzeba będzie odświeżyć stronę, aby wyświetlić dzienniki negocjowania metodę transportu do nowego połączenia SignalR.</span><span class="sxs-lookup"><span data-stu-id="09f34-311">You might have to refresh the page to see the logs of SignalR negotiating the transport method for a new connection.</span></span>

    * <span data-ttu-id="09f34-312">Jeśli korzystasz z programu Internet Explorer 10 w systemie Windows 8 (IIS 8), jest metodą transportu **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="09f34-312">If you're running Internet Explorer 10 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

    * <span data-ttu-id="09f34-313">Jeśli korzystasz z programu Internet Explorer 10, Windows 7 (usługi IIS 7.5), jest metodą transportu **iframe**.</span><span class="sxs-lookup"><span data-stu-id="09f34-313">If you're running Internet Explorer 10 on Windows 7 (IIS 7.5), the transport method is **iframe**.</span></span>

    * <span data-ttu-id="09f34-314">Jeśli korzystasz z 19 przeglądarki Firefox w systemie Windows 8 (IIS 8), jest metodą transportu **WebSockets**.</span><span class="sxs-lookup"><span data-stu-id="09f34-314">If you're running Firefox 19 on Windows 8 (IIS 8), the transport method is **WebSockets**.</span></span>

        > [!TIP]
        > <span data-ttu-id="09f34-315">W przeglądarce Firefox Zainstaluj dodatek Firebug można pobrać z okna konsoli.</span><span class="sxs-lookup"><span data-stu-id="09f34-315">In Firefox, install the Firebug add-in to get a Console window.</span></span>

    * <span data-ttu-id="09f34-316">Jeśli korzystasz z Firefox 19, Windows 7 (usługi IIS 7.5), jest metodą transportu **serwer wysłał** zdarzenia.</span><span class="sxs-lookup"><span data-stu-id="09f34-316">If you're running Firefox 19 on Windows 7 (IIS 7.5), the transport method is **server-sent** events.</span></span>

## <a name="install-the-stockticker-sample"></a><span data-ttu-id="09f34-317">Instalowanie przykładowej StockTicker</span><span class="sxs-lookup"><span data-stu-id="09f34-317">Install the StockTicker sample</span></span>

<span data-ttu-id="09f34-318">[Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) instaluje aplikację StockTicker.</span><span class="sxs-lookup"><span data-stu-id="09f34-318">The [Microsoft.AspNet.SignalR.Sample](http://nuget.org/packages/microsoft.aspnet.signalr.sample) installs the StockTicker application.</span></span> <span data-ttu-id="09f34-319">Pakiet NuGet zawiera więcej funkcji niż uproszczonej wersji, który został utworzony od zera.</span><span class="sxs-lookup"><span data-stu-id="09f34-319">The NuGet package includes more features than the simplified version that you created from scratch.</span></span> <span data-ttu-id="09f34-320">W tej części samouczka zainstaluj pakiet NuGet i Przejrzyj nowe funkcje i kod, który implementuje je.</span><span class="sxs-lookup"><span data-stu-id="09f34-320">In this section of the tutorial, you install the NuGet package and review the new features and the code that implements them.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="09f34-321">Po zainstalowaniu pakietu bez przeprowadzania wcześniejsze kroki tego samouczka, należy dodać klasę początkową OWIN do projektu.</span><span class="sxs-lookup"><span data-stu-id="09f34-321">If you install the package without performing the earlier steps of this tutorial, you must add an OWIN startup class to your project.</span></span> <span data-ttu-id="09f34-322">Ten plik readme.txt dla pakietu NuGet wyjaśnia, w tym kroku.</span><span class="sxs-lookup"><span data-stu-id="09f34-322">This readme.txt file for the NuGet package explains this step.</span></span>

### <a name="install-the-signalrsample-nuget-package"></a><span data-ttu-id="09f34-323">Zainstaluj pakiet SignalR.Sample NuGet</span><span class="sxs-lookup"><span data-stu-id="09f34-323">Install the SignalR.Sample NuGet package</span></span>

1. <span data-ttu-id="09f34-324">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Zarządzaj pakietami NuGet**.</span><span class="sxs-lookup"><span data-stu-id="09f34-324">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="09f34-325">W **Menedżera pakietów NuGet: SignalR.StockTicker**, wybierz opcję **Przeglądaj**.</span><span class="sxs-lookup"><span data-stu-id="09f34-325">In **NuGet Package manager: SignalR.StockTicker**, select **Browse**.</span></span>

1. <span data-ttu-id="09f34-326">Z **źródła pakietu**, wybierz opcję **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="09f34-326">From **Package source**, select **nuget.org**.</span></span>

1. <span data-ttu-id="09f34-327">Wprowadź *SignalR.Sample* w polu wyszukiwania i wybierz pozycję **Microsoft.AspNet.SignalR.Sample** > **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="09f34-327">Enter *SignalR.Sample* in the search box and select **Microsoft.AspNet.SignalR.Sample** > **Install**.</span></span>

1. <span data-ttu-id="09f34-328">W **Eksploratora rozwiązań**, rozwiń węzeł *SignalR.Sample* folderu.</span><span class="sxs-lookup"><span data-stu-id="09f34-328">In **Solution Explorer**, expand the *SignalR.Sample* folder.</span></span>

    <span data-ttu-id="09f34-329">Instalowanie pakietu SignalR.Sample utworzony folder i jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="09f34-329">Installing the SignalR.Sample package created the folder and its contents.</span></span>

1. <span data-ttu-id="09f34-330">W *SignalR.Sample* folderu, kliknij prawym przyciskiem myszy *StockTicker.html*, a następnie wybierz pozycję **Ustaw jako stronę startową**.</span><span class="sxs-lookup"><span data-stu-id="09f34-330">In the *SignalR.Sample* folder, right-click *StockTicker.html*, and then select **Set As Start Page**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="09f34-331">Instalowanie SignalR.Sample NuGet pakietu może zmienić wersję jQuery, które mają w swojej *skrypty* folderu.</span><span class="sxs-lookup"><span data-stu-id="09f34-331">Installing The SignalR.Sample NuGet package might change the version of jQuery that you have in your *Scripts* folder.</span></span> <span data-ttu-id="09f34-332">Nowy *StockTicker.html* pliku, który pakiet instaluje w *SignalR.Sample* folder będzie zsynchronizowany z wersją jQuery instalująca pakiet, ale jeśli chcesz uruchomić oryginalny *StockTicker.html* ponownie plik, trzeba najpierw zaktualizuj odwołanie jQuery w tagu.</span><span class="sxs-lookup"><span data-stu-id="09f34-332">The new *StockTicker.html* file that the package installs in the *SignalR.Sample* folder will be in sync with the jQuery version that the package installs, but if you want to run your original *StockTicker.html* file again, you might have to update the jQuery reference in the script tag first.</span></span>

### <a name="run-the-application"></a><span data-ttu-id="09f34-333">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="09f34-333">Run the application</span></span>

 <span data-ttu-id="09f34-334">Tabelę, która działa w pierwszej aplikacji ma użytecznych funkcji.</span><span class="sxs-lookup"><span data-stu-id="09f34-334">The table that you saw in the first app had useful features.</span></span> <span data-ttu-id="09f34-335">Aplikacja pełnej giełdowej przedstawiono nowe funkcje: okna przewijania w poziomie, pokazujący danych giełdowych i zasobów, które zmieniają kolor, jak długo będą rosnąć i dzielą się.</span><span class="sxs-lookup"><span data-stu-id="09f34-335">The full stock ticker application shows new features: a horizontally scrolling window that shows the stock data and stocks that change color as they rise and fall.</span></span>

1. <span data-ttu-id="09f34-336">Naciśnij klawisz **F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="09f34-336">Press **F5** to run the app.</span></span>

     <span data-ttu-id="09f34-337">Po uruchomieniu aplikacji po raz pierwszy "na rynku" to "zamknięte", a zobaczysz tabelę statyczną i okna znacznika, który nie jest przewijanie.</span><span class="sxs-lookup"><span data-stu-id="09f34-337">When you run the app for the first time, the "market" is "closed" and you see a static table and a ticker window that isn't scrolling.</span></span>

1. <span data-ttu-id="09f34-338">Wybierz **wolnym rynku**.</span><span class="sxs-lookup"><span data-stu-id="09f34-338">Select **Open Market**.</span></span>

    ![Zrzut ekranu przedstawiający znacznika na żywo.](tutorial-server-broadcast-with-signalr/_static/image5.png)

    * <span data-ttu-id="09f34-340">**Live znacznika Stock** pole zaczyna być przewijane w poziomie, a serwer jest uruchamiany okresowo wysyłać zmian cen akcji na podstawie losowej.</span><span class="sxs-lookup"><span data-stu-id="09f34-340">The **Live Stock Ticker** box starts to scroll horizontally, and the server starts to periodically broadcast stock price changes on a random basis.</span></span>

    * <span data-ttu-id="09f34-341">Każdorazowo cena akcji zmiany, aplikacja aktualizuje zarówno **Live tabeli Stock** i **Live znacznika Stock**.</span><span class="sxs-lookup"><span data-stu-id="09f34-341">Each time a stock price changes, the app updates both the **Live Stock Table** and the **Live Stock Ticker**.</span></span>

    * <span data-ttu-id="09f34-342">Gdy zmian cen zasobów jest dodatnia, aplikacja będzie wyświetlana cena akcji z zielonym tłem.</span><span class="sxs-lookup"><span data-stu-id="09f34-342">When a stock's price change is positive, the app shows the stock with a green background.</span></span>

    * <span data-ttu-id="09f34-343">Gdy ta zmiana jest ujemna, stock czerwonym tle wyświetlany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="09f34-343">When the change is negative, the app shows the stock with a red background.</span></span>

1. <span data-ttu-id="09f34-344">Wybierz **Zamknij rynku**.</span><span class="sxs-lookup"><span data-stu-id="09f34-344">Select **Close Market**.</span></span>

    * <span data-ttu-id="09f34-345">Tabela aktualizacji zatrzymania.</span><span class="sxs-lookup"><span data-stu-id="09f34-345">The table updates stop.</span></span>

    * <span data-ttu-id="09f34-346">Znacznika zatrzymuje przewijania.</span><span class="sxs-lookup"><span data-stu-id="09f34-346">The ticker stops scrolling.</span></span>

1. <span data-ttu-id="09f34-347">Wybierz **resetowania**.</span><span class="sxs-lookup"><span data-stu-id="09f34-347">Select **Reset**.</span></span>

    * <span data-ttu-id="09f34-348">Wszystkie dane podstawowe jest resetowany.</span><span class="sxs-lookup"><span data-stu-id="09f34-348">All stock data is reset.</span></span>

    * <span data-ttu-id="09f34-349">Aplikacja przywraca stan początkowy przed wprowadzeniem zmian cen pracę.</span><span class="sxs-lookup"><span data-stu-id="09f34-349">The app restores the initial state before price changes started.</span></span>

1. <span data-ttu-id="09f34-350">Skopiuj adres URL w przeglądarce otwórz dwóch innych przeglądarek i wklejać adresy URL paski adresu.</span><span class="sxs-lookup"><span data-stu-id="09f34-350">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="09f34-351">Możesz zobaczyć te same dane, które są aktualizowane dynamicznie w tym samym czasie w każdej przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="09f34-351">You see the same data dynamically updated at the same time in each browser.</span></span>

1. <span data-ttu-id="09f34-352">Po wybraniu dowolnej kontrolki wszystkie przeglądarki odpowiedzieć ten sam sposób, w tym samym czasie.</span><span class="sxs-lookup"><span data-stu-id="09f34-352">When you select any of the controls, all browsers respond the same way at the same time.</span></span>

### <a name="live-stock-ticker-display"></a><span data-ttu-id="09f34-353">Na żywo wyświetlanie znacznika zapasów</span><span class="sxs-lookup"><span data-stu-id="09f34-353">Live Stock Ticker display</span></span>

<span data-ttu-id="09f34-354">**Live znacznika Stock** wyświetlana jest Lista nieuporządkowana w `<div>` element sformatowane w jednej linii za style CSS.</span><span class="sxs-lookup"><span data-stu-id="09f34-354">The **Live Stock Ticker** display is an unordered list in a `<div>` element formatted into a single line by CSS styles.</span></span> <span data-ttu-id="09f34-355">Inicjuje i aktualizuje znacznika taki sam sposób jak tabela aplikacji:, zastępując symbole zastępcze w wywołaniach `<li>` ciąg szablonu i dynamiczne dodawanie `<li>` elementów `<ul>` elementu.</span><span class="sxs-lookup"><span data-stu-id="09f34-355">The app initializes and updates the ticker the same way as the table: by replacing placeholders in an `<li>` template string and dynamically adding the `<li>` elements to the `<ul>` element.</span></span> <span data-ttu-id="09f34-356">Aplikacja zawiera przewijanie przy użyciu jQuery `animate` funkcji różnią się w lewej margines nieuporządkowaną listę w obrębie `<div>`.</span><span class="sxs-lookup"><span data-stu-id="09f34-356">The app includes  scrolling by using the jQuery `animate` function to vary the margin-left of the unordered list within the `<div>`.</span></span>

#### <a name="signalrsample-stocktickerhtml"></a><span data-ttu-id="09f34-357">SignalR.Sample StockTicker.html</span><span class="sxs-lookup"><span data-stu-id="09f34-357">SignalR.Sample StockTicker.html</span></span>

<span data-ttu-id="09f34-358">Giełdowej kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="09f34-358">The stock ticker HTML code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample20.html)]

#### <a name="signalrsample-stocktickercss"></a><span data-ttu-id="09f34-359">SignalR.Sample StockTicker.css</span><span class="sxs-lookup"><span data-stu-id="09f34-359">SignalR.Sample StockTicker.css</span></span>

<span data-ttu-id="09f34-360">Giełdowej kod CSS:</span><span class="sxs-lookup"><span data-stu-id="09f34-360">The stock ticker CSS code:</span></span>

[!code-html[Main](tutorial-server-broadcast-with-signalr/samples/sample21.html)]

#### <a name="signalrsample-signalrstocktickerjs"></a><span data-ttu-id="09f34-361">SignalR.Sample SignalR.StockTicker.js</span><span class="sxs-lookup"><span data-stu-id="09f34-361">SignalR.Sample SignalR.StockTicker.js</span></span>

<span data-ttu-id="09f34-362">Przewiń kodu jQuery, który sprawia, że:</span><span class="sxs-lookup"><span data-stu-id="09f34-362">The jQuery code that makes it scroll:</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample22.js)]

### <a name="additional-methods-on-the-server-that-the-client-can-call"></a><span data-ttu-id="09f34-363">Dodatkowe metody na serwerze, który klient może wywołać</span><span class="sxs-lookup"><span data-stu-id="09f34-363">Additional methods on the server that the client can call</span></span>

<span data-ttu-id="09f34-364">Aby dodać elastyczność do aplikacji, istnieją dodatkowe metody, które aplikacja może wywołać.</span><span class="sxs-lookup"><span data-stu-id="09f34-364">To add flexibility to the app, there are additional methods the app can call.</span></span>

#### <a name="signalrsample-stocktickerhubcs"></a><span data-ttu-id="09f34-365">SignalR.Sample StockTickerHub.cs</span><span class="sxs-lookup"><span data-stu-id="09f34-365">SignalR.Sample StockTickerHub.cs</span></span>

<span data-ttu-id="09f34-366">`StockTickerHub` Klasa definiuje cztery dodatkowe metody, które klient może wywoływać:</span><span class="sxs-lookup"><span data-stu-id="09f34-366">The `StockTickerHub` class defines four additional methods that the client can call:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample23.cs)]

<span data-ttu-id="09f34-367">Wywołania aplikacji `OpenMarket`, `CloseMarket`, i `Reset` w odpowiedzi na przycisków w górnej części strony.</span><span class="sxs-lookup"><span data-stu-id="09f34-367">The app calls `OpenMarket`, `CloseMarket`, and `Reset` in response to the buttons at the top of the page.</span></span> <span data-ttu-id="09f34-368">Pokazują one wzorca jednego klienta, wyzwalając zmianę stanu natychmiast propagowane do wszystkich klientów.</span><span class="sxs-lookup"><span data-stu-id="09f34-368">They demonstrate the pattern of one client triggering a change in state immediately propagated to all clients.</span></span> <span data-ttu-id="09f34-369">Każda z tych metod wywołuje metodę `StockTicker` klasę, która powoduje, że zmiana stanu na rynku, a następnie emituje nowy stan.</span><span class="sxs-lookup"><span data-stu-id="09f34-369">Each of these methods calls a method in the `StockTicker` class that causes the market state change and then broadcasts the new state.</span></span>

#### <a name="signalrsample-stocktickercs"></a><span data-ttu-id="09f34-370">SignalR.Sample StockTicker.cs</span><span class="sxs-lookup"><span data-stu-id="09f34-370">SignalR.Sample StockTicker.cs</span></span>

<span data-ttu-id="09f34-371">W `StockTicker` klasy, aplikacja przechowuje stan rynku dzięki `MarketState` właściwość, która zwraca `MarketState` wartość wyliczenia:</span><span class="sxs-lookup"><span data-stu-id="09f34-371">In the `StockTicker` class, the app maintains the state of the market with a `MarketState` property that returns a `MarketState` enum value:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample24.cs)]

<span data-ttu-id="09f34-372">Każdej z metod, które zmieniają stan rynku zrobić wewnątrz bloku blokady ponieważ `StockTicker` klasy musi być metodą o bezpiecznych wątkach:</span><span class="sxs-lookup"><span data-stu-id="09f34-372">Each of the methods that change the market state do so inside a lock block because the `StockTicker` class has to be thread-safe:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample25.cs)]

<span data-ttu-id="09f34-373">Aby upewnić się, że ten kod jest bezpieczna dla wątków, `_marketState` pola, która będzie tworzyć kopię `MarketState` wyznaczona właściwość `volatile`:</span><span class="sxs-lookup"><span data-stu-id="09f34-373">To make sure this code is thread-safe, the `_marketState` field that backs the `MarketState` property designated `volatile`:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample26.cs)]

<span data-ttu-id="09f34-374">`BroadcastMarketStateChange` i `BroadcastMarketReset` metody są podobne do metody BroadcastStockPrice, która już działa, z wyjątkiem wywołują różnych metod, zdefiniowanych na komputerze klienckim:</span><span class="sxs-lookup"><span data-stu-id="09f34-374">The `BroadcastMarketStateChange` and `BroadcastMarketReset` methods are similar to the BroadcastStockPrice method that you already saw, except they call different methods defined at the client:</span></span>

[!code-csharp[Main](tutorial-server-broadcast-with-signalr/samples/sample27.cs)]

### <a name="additional-functions-on-the-client-that-the-server-can-call"></a><span data-ttu-id="09f34-375">Dodatkowe funkcje, na komputerze klienckim, który można wywoływać serwera</span><span class="sxs-lookup"><span data-stu-id="09f34-375">Additional functions on the client that the server can call</span></span>

<span data-ttu-id="09f34-376">`updateStockPrice` Funkcja obsługuje teraz zarówno w tabeli, jak i wyświetlanie znacznika, przy czym `jQuery.Color` do flash czerwonego i zielonego kolorów.</span><span class="sxs-lookup"><span data-stu-id="09f34-376">The `updateStockPrice` function now handles both the table and the ticker display, and it uses `jQuery.Color` to flash red and green colors.</span></span>

<span data-ttu-id="09f34-377">Nowe funkcje w *SignalR.StockTicker.js* Włączanie i wyłączanie przycisków na podstawie stanu rynku.</span><span class="sxs-lookup"><span data-stu-id="09f34-377">New functions in *SignalR.StockTicker.js* enable and disable the buttons based on market state.</span></span> <span data-ttu-id="09f34-378">Mogą również zatrzymać lub rozpocząć **Live znacznika Stock** przewijanie w poziomie.</span><span class="sxs-lookup"><span data-stu-id="09f34-378">They also stop or start the **Live Stock Ticker** horizontal scrolling.</span></span> <span data-ttu-id="09f34-379">Ponieważ wiele funkcji są dodawane do `ticker.client`, ta aplikacja używa [jQuery rozszerzanie funkcji](http://api.jquery.com/jQuery.extend/) je dodać.</span><span class="sxs-lookup"><span data-stu-id="09f34-379">Since many functions are being added to `ticker.client`, the app uses the [jQuery extend function](http://api.jquery.com/jQuery.extend/) to add them.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample28.js)]

### <a name="additional-client-setup-after-establishing-the-connection"></a><span data-ttu-id="09f34-380">Instalacja klienta dodatkowe po ustanowieniu połączenia</span><span class="sxs-lookup"><span data-stu-id="09f34-380">Additional client setup after establishing the connection</span></span>

<span data-ttu-id="09f34-381">Po nawiązaniu połączenia przez klienta ma pewne dodatkowe zadania do wykonania:</span><span class="sxs-lookup"><span data-stu-id="09f34-381">After the client establishes the connection, it has some additional work to do:</span></span>

* <span data-ttu-id="09f34-382">Dowiedz się, czy rynku jest otwarte lub zamknięte do wywołania odpowiednie `marketOpened` lub `marketClosed` funkcji.</span><span class="sxs-lookup"><span data-stu-id="09f34-382">Find out if the market is open or closed to call the appropriate `marketOpened` or `marketClosed` function.</span></span>

* <span data-ttu-id="09f34-383">Dołącz wywołania metody serwera do przycisków.</span><span class="sxs-lookup"><span data-stu-id="09f34-383">Attach the server method calls to the buttons.</span></span>

[!code-javascript[Main](tutorial-server-broadcast-with-signalr/samples/sample29.js)]

<span data-ttu-id="09f34-384">Metody serwera nie są powiązaną przycisków do momentu, po Aplikacja nawiązuje połączenie.</span><span class="sxs-lookup"><span data-stu-id="09f34-384">The server methods aren't wired up to the buttons until after the app establishes the connection.</span></span> <span data-ttu-id="09f34-385">To dlatego kod nie można wywołać metody serwera, zanim staną się dostępne.</span><span class="sxs-lookup"><span data-stu-id="09f34-385">It's so the code can't call the server methods before they're available.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="09f34-386">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="09f34-386">Additional resources</span></span>

<span data-ttu-id="09f34-387">W tym samouczku wyjaśniono sposób programowania aplikacji SignalR, który emituje komunikaty z serwera do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="09f34-387">In this tutorial you've learned how to program a SignalR application that broadcasts messages from the server to all connected clients.</span></span> <span data-ttu-id="09f34-388">Teraz może emitować wiadomości w regularnych odstępach czasu i odpowiedzi na powiadomienia za pomocą dowolnego klienta.</span><span class="sxs-lookup"><span data-stu-id="09f34-388">Now you can broadcast messages on a periodic basis and in response to notifications from any client.</span></span> <span data-ttu-id="09f34-389">Pojęcie wielowątkowych pojedyncze wystąpienie służy do zarządzania stanem serwera w trybie online scenariuszach gier wielu graczy.</span><span class="sxs-lookup"><span data-stu-id="09f34-389">You can use the concept of multi-threaded singleton instance to maintain server state in multi-player online game scenarios.</span></span> <span data-ttu-id="09f34-390">Aby uzyskać przykład, zobacz [ShootR gry oparte na SignalR](https://github.com/NTaylorMullen/ShootR).</span><span class="sxs-lookup"><span data-stu-id="09f34-390">For an example, see [the ShootR game based on SignalR](https://github.com/NTaylorMullen/ShootR).</span></span>

<span data-ttu-id="09f34-391">Samouczki, które pokazują scenariuszy komunikacji między peer-to-peer, zobacz [wprowadzenie do SignalR](introduction-to-signalr.md) i [aktualizacji w czasie rzeczywistym, przy użyciu SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="09f34-391">For tutorials that show peer-to-peer communication scenarios, see [Getting Started with SignalR](introduction-to-signalr.md) and [Real-Time Updating with SignalR](tutorial-high-frequency-realtime-with-signalr.md).</span></span>

<span data-ttu-id="09f34-392">Aby uzyskać więcej informacji na temat biblioteki SignalR zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="09f34-392">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="09f34-393">Biblioteki SignalR platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="09f34-393">ASP.NET SignalR</span></span>](../../index.md)
* [<span data-ttu-id="09f34-394">Projekt SignalR</span><span class="sxs-lookup"><span data-stu-id="09f34-394">SignalR Project</span></span>](http://signalr.net/)
* [<span data-ttu-id="09f34-395">SignalR GitHub i przykłady</span><span class="sxs-lookup"><span data-stu-id="09f34-395">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)
* [<span data-ttu-id="09f34-396">Witryny typu Wiki biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="09f34-396">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="09f34-397">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="09f34-397">Next steps</span></span>

<span data-ttu-id="09f34-398">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="09f34-398">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="09f34-399">Tworzenia projektu</span><span class="sxs-lookup"><span data-stu-id="09f34-399">Created the project</span></span>
> * <span data-ttu-id="09f34-400">Konfigurowanie kodu serwera</span><span class="sxs-lookup"><span data-stu-id="09f34-400">Set up the server code</span></span>
> * <span data-ttu-id="09f34-401">Zbadanie kodu serwera</span><span class="sxs-lookup"><span data-stu-id="09f34-401">Examined the server code</span></span>
> * <span data-ttu-id="09f34-402">Ustaw kod klienta</span><span class="sxs-lookup"><span data-stu-id="09f34-402">Set up the client code</span></span>
> * <span data-ttu-id="09f34-403">Zbadanie kodu klienta</span><span class="sxs-lookup"><span data-stu-id="09f34-403">Examined the client code</span></span>
> * <span data-ttu-id="09f34-404">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="09f34-404">Tested the application</span></span>
> * <span data-ttu-id="09f34-405">Rejestrowanie włączone</span><span class="sxs-lookup"><span data-stu-id="09f34-405">Enabled logging</span></span>

<span data-ttu-id="09f34-406">Przejdź do następnego artykułu, aby dowiedzieć się, jak utworzyć aplikację sieci web w czasie rzeczywistym, korzystającą z signalr2 na platformie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="09f34-406">Advance to the next article to learn how to create a real-time web application that uses ASP.NET SignalR 2.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="09f34-407">Tworzenie aplikacji sieci web w czasie rzeczywistym przy użyciu biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="09f34-407">Create real-time web app with SignalR</span></span>](real-time-web-applications-with-signalr.md)