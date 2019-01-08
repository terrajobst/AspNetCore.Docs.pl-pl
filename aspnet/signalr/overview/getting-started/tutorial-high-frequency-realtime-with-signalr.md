---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Samouczek: Tworzenie aplikacji w czasie rzeczywistym z wysoką częstotliwością przy użyciu SignalR 2 | Dokumentacja firmy Microsoft'
author: pfletcher
description: W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web, która używa biblioteki SignalR platformy ASP.NET w celu zapewnienia wysokiej częstotliwości funkcje obsługi komunikatów.
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 85503db0b41be6f87136627667d6dd71f0d4f609
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098593"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a><span data-ttu-id="8fa4a-103">Samouczek: Tworzenie aplikacji w czasie rzeczywistym z wysoką częstotliwością przy użyciu SignalR 2</span><span class="sxs-lookup"><span data-stu-id="8fa4a-103">Tutorial: Create high-frequency real-time app with SignalR 2</span></span>

<span data-ttu-id="8fa4a-104">W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web korzystającą z signalr2 na platformie ASP.NET w celu zapewnienia funkcji obsługi wiadomości o wysokiej częstotliwości.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="8fa4a-105">W tym przypadku "komunikaty o wysokiej częstotliwości" oznacza, że serwer wysyła aktualizacje według stałej stawki.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-105">In this case, "high-frequency messaging" means the server sends updates at a fixed rate.</span></span> <span data-ttu-id="8fa4a-106">Możesz wysyłać maksymalnie 10 komunikatów na sekundę.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-106">You send up to 10 messages a second.</span></span>

<span data-ttu-id="8fa4a-107">Aplikacji, którą utworzysz Wyświetla kształtu, w którym można przeciągać użytkowników.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-107">The application you create displays a shape that users can drag.</span></span> <span data-ttu-id="8fa4a-108">Serwer aktualizacji położenie kształtu we wszystkich przeglądarkach połączonych, aby dopasować położenie przeciąganego kształtu przy użyciu Przekroczono limit czasu aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-108">The server updates the position of the shape in all connected browsers to match the position of the dragged shape using timed updates.</span></span>

<span data-ttu-id="8fa4a-109">Pojęciami opisanymi w tym samouczku mają aplikacji w czasie rzeczywistym gry i inne aplikacje symulacji.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-109">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>

<span data-ttu-id="8fa4a-110">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="8fa4a-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8fa4a-111">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="8fa4a-111">Set up the project</span></span>
> * <span data-ttu-id="8fa4a-112">Tworzenie podstawowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="8fa4a-112">Create the base application</span></span>
> * <span data-ttu-id="8fa4a-113">Mapowanie do koncentratora, po uruchomieniu aplikacji</span><span class="sxs-lookup"><span data-stu-id="8fa4a-113">Map to the hub when app starts</span></span>
> * <span data-ttu-id="8fa4a-114">Dodaj klienta</span><span class="sxs-lookup"><span data-stu-id="8fa4a-114">Add the client</span></span>
> * <span data-ttu-id="8fa4a-115">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="8fa4a-115">Run the app</span></span>
> * <span data-ttu-id="8fa4a-116">Dodaj pętlę klienta</span><span class="sxs-lookup"><span data-stu-id="8fa4a-116">Add the client loop</span></span>
> * <span data-ttu-id="8fa4a-117">Dodaj pętlę serwera</span><span class="sxs-lookup"><span data-stu-id="8fa4a-117">Add the server loop</span></span>
> * <span data-ttu-id="8fa4a-118">Dodaj płynne animacje</span><span class="sxs-lookup"><span data-stu-id="8fa4a-118">Add smooth animation</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="8fa4a-119">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="8fa4a-119">Prerequisites</span></span>

* <span data-ttu-id="8fa4a-120">[Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="8fa4a-121">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="8fa4a-121">Set up the project</span></span>

<span data-ttu-id="8fa4a-122">W tej sekcji utworzysz projekt programu Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-122">In this section, you create the project in Visual Studio 2017.</span></span>

<span data-ttu-id="8fa4a-123">W tej sekcji pokazano, jak utworzyć pustą aplikację sieci Web platformy ASP.NET i dodać biblioteki SignalR i jQuery.UI przy użyciu programu Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-123">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application and add the SignalR and jQuery.UI libraries.</span></span>

1. <span data-ttu-id="8fa4a-124">W programie Visual Studio należy utworzyć aplikację sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-124">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Tworzenie sieci web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. <span data-ttu-id="8fa4a-126">W **nowej aplikacji sieci Web ASP.NET - MoveShapeDemo** okna, pozostaw **pusty** zaznaczone, a następnie wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-126">In the **New ASP.NET Web Application - MoveShapeDemo** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="8fa4a-127">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-127">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="8fa4a-128">W **Dodaj nowy element - MoveShapeDemo**, wybierz opcję **zainstalowane** > **Visual C#**   >  **Web**  >  **SignalR** , a następnie wybierz **klasa Centrum SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-128">In **Add New Item - MoveShapeDemo**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="8fa4a-129">Nazwa klasy *MoveShapeHub* i dodaj go do projektu.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-129">Name the class *MoveShapeHub* and add it to the project.</span></span>

    <span data-ttu-id="8fa4a-130">Spowoduje to utworzenie *MoveShapeHub.cs* pliku klasy.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-130">This step creates the *MoveShapeHub.cs* class file.</span></span> <span data-ttu-id="8fa4a-131">Jednocześnie dodaje zestaw plików skryptów i odwołania do zestawów, obsługujące bibliotekę SignalR do projektu.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-131">Simultaneously, it adds  a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="8fa4a-132">Wybierz **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-132">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

1. <span data-ttu-id="8fa4a-133">W **Konsola Menedżera pakietów**, uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="8fa4a-133">In **Package Manager Console**, run this command:</span></span>

    ```console
    Install-Package jQuery.UI.Combined
    ```

    <span data-ttu-id="8fa4a-134">Polecenie instaluje biblioteki interfejsu użytkownika jQuery.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-134">The command installs the jQuery UI library.</span></span> <span data-ttu-id="8fa4a-135">Umożliwia ona animować kształt.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-135">You use it to animate the shape.</span></span>

1. <span data-ttu-id="8fa4a-136">W **Eksploratora rozwiązań**, rozwiń węzeł skryptów.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-136">In **Solution Explorer**, expand the Scripts node.</span></span>

    ![Odwołania do biblioteki skryptu](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    <span data-ttu-id="8fa4a-138">Biblioteki skryptów, jQuery, jQueryUI i SignalR są widoczne w projekcie.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-138">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

## <a name="create-the-base-application"></a><span data-ttu-id="8fa4a-139">Tworzenie podstawowej aplikacji</span><span class="sxs-lookup"><span data-stu-id="8fa4a-139">Create the base application</span></span>

<span data-ttu-id="8fa4a-140">W tej sekcji utworzysz aplikację przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-140">In this section, you create a browser application.</span></span> <span data-ttu-id="8fa4a-141">Aplikacja wysyła położenie kształtu do serwera podczas każdego zdarzenie przesunięcia kursora myszy.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-141">The app sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="8fa4a-142">Serwer wysyła te informacje do wszystkich innych połączonych klientów w czasie rzeczywistym.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-142">The server broadcasts this information to all other connected clients in real time.</span></span> <span data-ttu-id="8fa4a-143">Dowiesz się więcej na temat tej aplikacji w kolejnych sekcjach.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-143">You learn more about this application in later sections.</span></span>

1. <span data-ttu-id="8fa4a-144">Otwórz *MoveShapeHub.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-144">Open the *MoveShapeHub.cs* file.</span></span>

1. <span data-ttu-id="8fa4a-145">Zastąp kod w *MoveShapeHub.cs* pliku przy użyciu tego kodu:</span><span class="sxs-lookup"><span data-stu-id="8fa4a-145">Replace the code in the *MoveShapeHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="8fa4a-146">Zapisz plik.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-146">Save the file.</span></span>

<span data-ttu-id="8fa4a-147">`MoveShapeHub` Klasa jest implementacją Centrum SignalR.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-147">The `MoveShapeHub` class is an implementation of a SignalR hub.</span></span> <span data-ttu-id="8fa4a-148">Podobnie jak w [wprowadzenie do SignalR](tutorial-getting-started-with-signalr.md) samouczku Centrum ma metodę, która bezpośrednio wywoływać klientów.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-148">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients call directly.</span></span> <span data-ttu-id="8fa4a-149">W tym przypadku klient przesyła obiektu za pomocą nowego X i Y koordynuje kształtu do serwera.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-149">In this case, the client sends an object with the new X and Y coordinates of the shape to the server.</span></span> <span data-ttu-id="8fa4a-150">Tych współrzędnych Pobierz wysłano do wszystkich innych połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-150">Those coordinates get broadcasted to all other connected clients.</span></span> <span data-ttu-id="8fa4a-151">SignalR serializuje automatycznie tego obiektu przy użyciu formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-151">SignalR automatically serializes this object using JSON.</span></span>

<span data-ttu-id="8fa4a-152">Wysyła aplikacji `ShapeModel` obiektu do klienta.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-152">The app sends the `ShapeModel` object to the client.</span></span> <span data-ttu-id="8fa4a-153">Zawiera elementy członkowskie do przechowywania położenie kształtu.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-153">It has members to store the position of the shape.</span></span> <span data-ttu-id="8fa4a-154">Wersja obiektu na serwerze ma również członkiem, aby śledzić przechowywanych danych których klienta.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-154">The version of the object on the server also has a member to track which client's data is being stored.</span></span> <span data-ttu-id="8fa4a-155">Ten obiekt zapobiega wysłaniem dane klienta z powrotem do samego serwera.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-155">This object prevents the server from sending a client's data back to itself.</span></span> <span data-ttu-id="8fa4a-156">Używa tego elementu członkowskiego `JsonIgnore` atrybutu, aby uniemożliwić aplikacji serializacji danych i wysłanie go do klienta.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-156">This member uses the `JsonIgnore` attribute to keep the application from serializing the data and sending it back to the client.</span></span>

## <a name="map-to-the-hub-when-app-starts"></a><span data-ttu-id="8fa4a-157">Mapowanie do koncentratora, po uruchomieniu aplikacji</span><span class="sxs-lookup"><span data-stu-id="8fa4a-157">Map to the hub when app starts</span></span>

<span data-ttu-id="8fa4a-158">Następnie należy skonfigurować mapowanie do Centrum podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-158">Next, you set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="8fa4a-159">W SignalR 2 Dodawanie klasy początkowej OWIN tworzy mapowanie.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-159">In SignalR 2, adding an OWIN startup class creates the mapping.</span></span>

1. <span data-ttu-id="8fa4a-160">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-160">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="8fa4a-161">W **Dodaj nowy element - MoveShapeDemo** wybierz **zainstalowane** > **Visual C#**   >  **Web** a Wybierz **klasy początkowej OWIN**.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-161">In **Add New Item - MoveShapeDemo** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="8fa4a-162">Nazwa klasy *uruchamiania* i wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-162">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="8fa4a-163">Zastąp kod domyślne *Startup.cs* pliku przy użyciu tego kodu:</span><span class="sxs-lookup"><span data-stu-id="8fa4a-163">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<span data-ttu-id="8fa4a-164">Wywołuje klasę uruchamiania OWIN `MapSignalR` gdy aplikacja wykonuje `Configuration` metody.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-164">The OWIN startup class calls `MapSignalR` when the app executes the `Configuration` method.</span></span> <span data-ttu-id="8fa4a-165">Aplikacja dodaje Klasa początkowa OWIN dla przetwarzania przy użyciu `OwinStartup` atrybutu zestawu.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-165">The app adds the class to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

## <a name="add-the-client"></a><span data-ttu-id="8fa4a-166">Dodaj klienta</span><span class="sxs-lookup"><span data-stu-id="8fa4a-166">Add the client</span></span>

<span data-ttu-id="8fa4a-167">Dodaj stronę HTML dla klienta.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-167">Add the HTML page for the client.</span></span>

1. <span data-ttu-id="8fa4a-168">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj** > **strony HTML**.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-168">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="8fa4a-169">Nazwij stronę **domyślne** i wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-169">Name the page **Default** and select **OK**.</span></span>

1. <span data-ttu-id="8fa4a-170">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *Default.html* i wybierz **Ustaw jako strona startowa**.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-170">In **Solution Explorer**, right-click *Default.html* and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="8fa4a-171">Zastąp kod domyślne *Default.html* pliku przy użyciu tego kodu:</span><span class="sxs-lookup"><span data-stu-id="8fa4a-171">Replace the default code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. <span data-ttu-id="8fa4a-172">W **Eksploratora rozwiązań**, rozwiń węzeł **skrypty**.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-172">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="8fa4a-173">Biblioteki skryptów, jQuery i SignalR są widoczne w projekcie.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-173">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="8fa4a-174">Menedżer pakietów instaluje późniejszą wersję skrypty SignalR.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-174">The package manager installs a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="8fa4a-175">Aktualizuj odwołania do skryptu w bloku kodu, aby odpowiadać wersji plików skrypt w projekcie.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-175">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

<span data-ttu-id="8fa4a-176">Ten kod HTML i JavaScript tworzy czerwonego `div` o nazwie `shape`.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-176">This HTML and JavaScript code creates a red `div` called `shape`.</span></span> <span data-ttu-id="8fa4a-177">Włącza zachowanie przeciągania kształtu, za pomocą biblioteki jQuery i używa `drag` zdarzenia do wysłania położenie kształtu do serwera.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-177">It enables the shape's dragging behavior using the jQuery library and uses the `drag` event to send the shape's position to the server.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="8fa4a-178">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="8fa4a-178">Run the app</span></span>

<span data-ttu-id="8fa4a-179">Aplikację można uruchomić na se'e pracy.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-179">You can run the app to se\`e it work.</span></span> <span data-ttu-id="8fa4a-180">Podczas przeciągania kształtu wokół okna przeglądarki kształt zbyt przenosi w innych przeglądarkach.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-180">When you drag the shape around a browser window, the shape moves in the other browsers too.</span></span>

1. <span data-ttu-id="8fa4a-181">Na pasku narzędzi, Włącz **debugowanie skryptu** a następnie wybierz przycisk Odtwórz, aby uruchomić aplikację w trybie debugowania.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-181">In the toolbar, turn on **Script Debugging** and then select the play button to run the application in Debug mode.</span></span>

    ![Zrzut ekranu przedstawiający użytkownika, włączając tryb debugowania i wybierając polecenie play.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    <span data-ttu-id="8fa4a-183">Zostanie otwarte okno przeglądarki z czerwonym kształtu w prawym górnym rogu.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-183">A browser window opens with the red shape in the upper-right corner.</span></span>

1. <span data-ttu-id="8fa4a-184">Skopiuj adres URL strony.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-184">Copy the page's URL.</span></span>

1. <span data-ttu-id="8fa4a-185">Otwórz innej przeglądarki i wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-185">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="8fa4a-186">Przeciągnij kształt w jednym z okna przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-186">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="8fa4a-187">Następuje kształt w oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-187">The shape in the other browser window follows.</span></span>

<span data-ttu-id="8fa4a-188">Podczas gdy aplikacji funkcji przy użyciu tej metody, nie jest zalecany model programowania.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-188">While the application functions using this method, it's not a recommended programming model.</span></span> <span data-ttu-id="8fa4a-189">Nie ma żadnego górnego limitu liczby wiadomości wysyłanych wprowadzenie.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-189">There's no upper limit to the number of messages getting sent.</span></span> <span data-ttu-id="8fa4a-190">W rezultacie klientami a serwerem Pobierz przeciążeniu z wiadomościami i spadku wydajności.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-190">As a result, the clients and server get overwhelmed with messages and performance degrades.</span></span> <span data-ttu-id="8fa4a-191">Ponadto aplikacja wyświetla rozłączonych animacji na komputerze klienckim.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-191">Also, the app displays a disjointed animation on the client.</span></span> <span data-ttu-id="8fa4a-192">Ta animacja jerky wynika kształt natychmiast przechodzi przez każdą z tych metod.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-192">This jerky animation happens because the shape moves instantly by each method.</span></span> <span data-ttu-id="8fa4a-193">Zaleca kształt bezproblemową do każdej nowej lokalizacji.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-193">It's better if the shape moves smoothly to each new location.</span></span> <span data-ttu-id="8fa4a-194">Następnie dowiesz się, jak rozwiązać te problemy.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-194">Next, you learn how to fix those issues.</span></span>

## <a name="add-the-client-loop"></a><span data-ttu-id="8fa4a-195">Dodaj pętlę klienta</span><span class="sxs-lookup"><span data-stu-id="8fa4a-195">Add the client loop</span></span>

<span data-ttu-id="8fa4a-196">Wysyłanie położenie kształtu na każdym zdarzenie przesunięcia kursora myszy tworzy niepotrzebne ilości ruchu sieciowego.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-196">Sending the location of the shape on every mouse move event creates an unnecessary amount of network traffic.</span></span> <span data-ttu-id="8fa4a-197">Aplikacja musi ograniczania wiadomości od klienta.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-197">The app needs to throttle the messages from the client.</span></span>

<span data-ttu-id="8fa4a-198">Użyj javascript `setInterval` funkcję, aby skonfigurować pętlę, która wysyła do serwera według stałej stawki ustalanej nowe informacje pozycji.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-198">Use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="8fa4a-199">Ta pętla jest reprezentacją podstawowe "pętli gry".</span><span class="sxs-lookup"><span data-stu-id="8fa4a-199">This loop is a basic representation of a "game loop."</span></span> <span data-ttu-id="8fa4a-200">Jest wielokrotnie funkcji obsługujące funkcje gier.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-200">It's a repeatedly called function that drives all the functionality of a game.</span></span>

1. <span data-ttu-id="8fa4a-201">Zastąp kod klienta w *Default.html* pliku przy użyciu tego kodu:</span><span class="sxs-lookup"><span data-stu-id="8fa4a-201">Replace the client code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > <span data-ttu-id="8fa4a-202">Musisz ponownie Zamień odwołania do skryptu.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-202">You have to replace the script references again.</span></span> <span data-ttu-id="8fa4a-203">Muszą one być zgodne wersje skrypty w projekcie.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-203">They must match the versions of the scripts in the project.</span></span>

    <span data-ttu-id="8fa4a-204">Ten nowy kod dodaje `updateServerModel` funkcji.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-204">This new code adds the `updateServerModel` function.</span></span> <span data-ttu-id="8fa4a-205">Pobiera ona wywoływana z częstotliwością, stały.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-205">It gets called on a fixed frequency.</span></span> <span data-ttu-id="8fa4a-206">Funkcja wysyła umieszczanie danych do serwera przy każdym `moved` flagi oznacza, że są nowe dane pozycji do wysłania.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-206">The function sends the position data to the server whenever the `moved` flag indicates that there's new position data to send.</span></span>

1. <span data-ttu-id="8fa4a-207">Wybierz przycisk Odtwórz, aby uruchomić aplikację</span><span class="sxs-lookup"><span data-stu-id="8fa4a-207">Select the play button to start the application</span></span>

1. <span data-ttu-id="8fa4a-208">Skopiuj adres URL strony.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-208">Copy the page's URL.</span></span>

1. <span data-ttu-id="8fa4a-209">Otwórz innej przeglądarki i wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-209">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="8fa4a-210">Przeciągnij kształt w jednym z okna przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-210">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="8fa4a-211">Następuje kształt w oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-211">The shape in the other browser window follows.</span></span>

<span data-ttu-id="8fa4a-212">Ponieważ aplikacja ogranicza liczbę wiadomości, które są wysyłane do serwera, animacja nie będzie wyświetlane tak dobre, czy na początku.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-212">Since the app throttles the number of messages that get sent to the server, the animation won't appear as smooth did at first.</span></span>

## <a name="add-the-server-loop"></a><span data-ttu-id="8fa4a-213">Dodaj pętlę serwera</span><span class="sxs-lookup"><span data-stu-id="8fa4a-213">Add the server loop</span></span>

<span data-ttu-id="8fa4a-214">W bieżącej aplikacji komunikatów wysyłanych z serwera do klienta wysyłana tak często, jak ich otrzymaniu.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-214">In the current application, messages sent from the server to the client go out as often as they're received.</span></span> <span data-ttu-id="8fa4a-215">Jak widać na komputerze klienckim, ten ruch sieciowy przedstawia podobny problem.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-215">This network traffic presents a similar problem as we see on the client.</span></span>

<span data-ttu-id="8fa4a-216">Aplikacja może wysyłać komunikaty częściej niż zajdzie taka potrzeba.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-216">The app can send messages more often than they're needed.</span></span> <span data-ttu-id="8fa4a-217">Połączenie może być propagowane w wyniku.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-217">The connection can become flooded as a result.</span></span> <span data-ttu-id="8fa4a-218">Ta sekcja opisuje sposób aktualizacji server, aby dodać czasomierz, co ogranicza współczynnik wiadomości wychodzących.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-218">This section describes how to update the server to add a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="8fa4a-219">Zastąp zawartość `MoveShapeHub.cs` przy użyciu tego kodu:</span><span class="sxs-lookup"><span data-stu-id="8fa4a-219">Replace the contents of `MoveShapeHub.cs` with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. <span data-ttu-id="8fa4a-220">Wybierz przycisk Odtwórz, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-220">Select the play button to start the application.</span></span>

1. <span data-ttu-id="8fa4a-221">Skopiuj adres URL strony.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-221">Copy the page's URL.</span></span>

1. <span data-ttu-id="8fa4a-222">Otwórz innej przeglądarki i wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-222">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="8fa4a-223">Przeciągnij kształt w jednym z okna przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-223">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="8fa4a-224">Ten kod rozwija klienta, aby dodać `Broadcaster` klasy.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-224">This code expands the client to add the `Broadcaster` class.</span></span> <span data-ttu-id="8fa4a-225">Nowa klasa ogranicza wiadomości wychodzących za pomocą `Timer` klasy z programu .NET framework.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-225">The new class throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

<span data-ttu-id="8fa4a-226">Zaleca się dowiedzieć się, że sam Centrum przejściowe.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-226">It's good to learn that the hub itself is transitory.</span></span> <span data-ttu-id="8fa4a-227">Jest tworzony za każdym razem, gdy jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-227">It's created every time it's needed.</span></span> <span data-ttu-id="8fa4a-228">Aby aplikacja tworzy `Broadcaster` jako pojedynczą.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-228">So the app creates the `Broadcaster` as a singleton.</span></span> <span data-ttu-id="8fa4a-229">Używa inicjowania z opóźnieniem mają być odroczone `Broadcaster`jego tworzenia, dopóki nie jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-229">It uses lazy initialization to defer the `Broadcaster`'s creation until it's needed.</span></span> <span data-ttu-id="8fa4a-230">Pozwala to zagwarantować, że aplikacja tworzy pierwsze wystąpienie koncentratora całkowicie przed rozpoczęciem czasomierza.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-230">That guarantees that the app creates the first hub instance completely before starting the timer.</span></span>

<span data-ttu-id="8fa4a-231">Wywołania na klientach `UpdateShape` funkcji są przenoszone poza Centrum `UpdateModel` metody.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-231">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method.</span></span> <span data-ttu-id="8fa4a-232">Nie jest już nazywa się natychmiast, gdy aplikacja odbiera wiadomości przychodzących.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-232">It's no longer called immediately whenever the app receives incoming messages.</span></span> <span data-ttu-id="8fa4a-233">Zamiast tego aplikacja wysyła komunikaty do klientów w wysokości 25 wywołania na sekundę.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-233">Instead, the app sends the messages to the clients at a rate of 25 calls per second.</span></span> <span data-ttu-id="8fa4a-234">Ten proces odbywa się przez `_broadcastLoop` czasomierza z poziomu `Broadcaster` klasy.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-234">The process is  managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

<span data-ttu-id="8fa4a-235">Na koniec, a nie bezpośrednio, wywołanie metody klienta z Centrum `Broadcaster` klasa musi uzyskać odwołanie do aktualnie działających `_hubContext` koncentratora.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-235">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to get a reference to the currently operating `_hubContext` hub.</span></span> <span data-ttu-id="8fa4a-236">Pobiera odwołanie z `GlobalHost`.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-236">It gets the reference with the `GlobalHost`.</span></span>

## <a name="add-smooth-animation"></a><span data-ttu-id="8fa4a-237">Dodaj płynne animacje</span><span class="sxs-lookup"><span data-stu-id="8fa4a-237">Add smooth animation</span></span>

<span data-ttu-id="8fa4a-238">Aplikacja jest prawie gotowy, ale firma Microsoft może spowodować, że jeden więcej poprawy jakości obsługi.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-238">The application is almost finished, but we could make one more improvement.</span></span> <span data-ttu-id="8fa4a-239">Aplikacja spowoduje przesunięcie kształtu na komputerze klienckim w odpowiedzi na komunikaty serwera.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-239">The app moves the shape on the client in response to server messages.</span></span> <span data-ttu-id="8fa4a-240">Zamiast ustawiać położenie kształtu do nowej lokalizacji określonej przez serwer, użyj biblioteki interfejsu użytkownika JQuery `animate` funkcji.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-240">Instead of setting the position of the shape to the new location given by the server, use the JQuery UI library's `animate` function.</span></span> <span data-ttu-id="8fa4a-241">To płynnie przeniesienie kształt od jego bieżących i nowych pozycji.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-241">It can move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="8fa4a-242">Aktualizacja klienta programu `updateShape` method in Class metoda *Default.html* pliku, aby wyglądał jak wyróżniony kod:</span><span class="sxs-lookup"><span data-stu-id="8fa4a-242">Update the client's `updateShape` method in the *Default.html* file to look like the highlighted code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. <span data-ttu-id="8fa4a-243">Wybierz przycisk Odtwórz, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-243">Select the play button to start the application.</span></span>

1. <span data-ttu-id="8fa4a-244">Skopiuj adres URL strony.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-244">Copy the page's URL.</span></span>

1. <span data-ttu-id="8fa4a-245">Otwórz innej przeglądarki i wklej adres URL na pasku adresu.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-245">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="8fa4a-246">Przeciągnij kształt w jednym z okna przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-246">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="8fa4a-247">Przemieszczanie kształtu w innym oknie zostanie wyświetlona mniej jerky.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-247">The movement of the shape in the other window appears less jerky.</span></span> <span data-ttu-id="8fa4a-248">Aplikacja argumentu ruchu za pośrednictwem czasu, a nie raz ustawiany na wiadomości przychodzącej.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-248">The app interpolates its movement over time rather than being set once per incoming message.</span></span>

<span data-ttu-id="8fa4a-249">Ten kod przenosi kształt z poprzedniej lokalizacji na nową.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-249">This code moves the shape from the old location to the new one.</span></span> <span data-ttu-id="8fa4a-250">Serwer udostępnia położenie kształtu w miarę upływu interwał animacji.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-250">The server gives the position of the shape over the course of the animation interval.</span></span> <span data-ttu-id="8fa4a-251">W tym przypadku to 100 milisekund.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-251">In this case, that's 100 milliseconds.</span></span> <span data-ttu-id="8fa4a-252">Aplikacja czyści wszystkie poprzednie animacje systemem kształtu, przed rozpoczęciem nowej animacji.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-252">The app clears any previous animation running on the shape before the new animation starts.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8fa4a-253">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8fa4a-253">Additional resources</span></span>

<span data-ttu-id="8fa4a-254">Komunikat przedstawia informacje na temat przyjęła przydatne do tworzenia gier online i innych symulacje, takie jak [ShootR gry utworzone przy użyciu SignalR](https://shootr.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="8fa4a-254">The communication paradigm you just learned about is useful for developing online games and other simulations, like [the ShootR game created with SignalR](https://shootr.azurewebsites.net/).</span></span>

<span data-ttu-id="8fa4a-255">Aby uzyskać więcej informacji na temat biblioteki SignalR zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="8fa4a-255">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="8fa4a-256">Projekt SignalR</span><span class="sxs-lookup"><span data-stu-id="8fa4a-256">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="8fa4a-257">SignalR GitHub i przykłady</span><span class="sxs-lookup"><span data-stu-id="8fa4a-257">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="8fa4a-258">Witryny typu Wiki biblioteki SignalR</span><span class="sxs-lookup"><span data-stu-id="8fa4a-258">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="8fa4a-259">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="8fa4a-259">Next steps</span></span>

<span data-ttu-id="8fa4a-260">W ramach tego samouczka możesz:</span><span class="sxs-lookup"><span data-stu-id="8fa4a-260">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="8fa4a-261">Konfigurowanie projektu</span><span class="sxs-lookup"><span data-stu-id="8fa4a-261">Set up the project</span></span>
> * <span data-ttu-id="8fa4a-262">Podstawowa aplikacja</span><span class="sxs-lookup"><span data-stu-id="8fa4a-262">Created the base application</span></span>
> * <span data-ttu-id="8fa4a-263">Mapowany do koncentratora, po uruchomieniu aplikacji</span><span class="sxs-lookup"><span data-stu-id="8fa4a-263">Mapped to the hub when app starts</span></span>
> * <span data-ttu-id="8fa4a-264">Dodaje klienta</span><span class="sxs-lookup"><span data-stu-id="8fa4a-264">Added the client</span></span>
> * <span data-ttu-id="8fa4a-265">Uruchomienia aplikacji</span><span class="sxs-lookup"><span data-stu-id="8fa4a-265">Ran the app</span></span>
> * <span data-ttu-id="8fa4a-266">Dodano pętlę klienta</span><span class="sxs-lookup"><span data-stu-id="8fa4a-266">Added the client loop</span></span>
> * <span data-ttu-id="8fa4a-267">Dodano pętlę serwera</span><span class="sxs-lookup"><span data-stu-id="8fa4a-267">Added the server loop</span></span>
> * <span data-ttu-id="8fa4a-268">Dodano płynne animacje</span><span class="sxs-lookup"><span data-stu-id="8fa4a-268">Added smooth animation</span></span>

<span data-ttu-id="8fa4a-269">Przejdź do następnego artykułu, aby dowiedzieć się, jak utworzyć aplikację sieci web korzystającą z signalr2 na platformie ASP.NET w celu zapewnienia funkcji emisji serwera.</span><span class="sxs-lookup"><span data-stu-id="8fa4a-269">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="8fa4a-270">SignalR 2 i emisji serwera</span><span class="sxs-lookup"><span data-stu-id="8fa4a-270">SignalR 2 and server broadcasting</span></span>](tutorial-server-broadcast-with-signalr.md)