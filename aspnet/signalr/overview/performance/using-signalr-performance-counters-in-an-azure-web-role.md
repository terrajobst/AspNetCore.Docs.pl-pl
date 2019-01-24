---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Korzystanie z liczników wydajności SignalR w roli sieci Web platformy Azure | Dokumentacja firmy Microsoft
author: guardrex
description: Jak zainstalować i korzystać z liczników wydajności SignalR w roli sieci Web platformy Azure.
keywords: Licznik ASP.NET,signalr,Performance, roli sieci web platformy azure
ms.author: bradyg
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 8e17e945bc144731dd149bd7ddfc9e29160eaf0b
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836491"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="6e934-104">Korzystanie z liczników wydajności SignalR w roli sieci Web platformy Azure</span><span class="sxs-lookup"><span data-stu-id="6e934-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="6e934-105">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6e934-105">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="6e934-106">Liczniki wydajności SignalR są używane do monitorowania wydajności aplikacji w roli sieci Web platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="6e934-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="6e934-107">Liczniki są przechwytywane przez Microsoft Azure Diagnostics.</span><span class="sxs-lookup"><span data-stu-id="6e934-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="6e934-108">Instalowanie liczników wydajności SignalR na platformie Azure dzięki *signalr.exe*, tego samego narzędzia, które są używane dla aplikacji autonomicznych lub w środowisku lokalnym.</span><span class="sxs-lookup"><span data-stu-id="6e934-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="6e934-109">Ponieważ przejściowy ról platformy Azure, możesz skonfigurować aplikację tak, aby zainstalować i zarejestrować liczników wydajności SignalR podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="6e934-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6e934-110">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="6e934-110">Prerequisites</span></span>

* <span data-ttu-id="6e934-111">Program Visual Studio 2015 lub [2017 r.](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span><span class="sxs-lookup"><span data-stu-id="6e934-111">Visual Studio 2015 or [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span></span>
* <span data-ttu-id="6e934-112">[Zestaw Microsoft Azure SDK dla programu Visual Studio](https://azure.microsoft.com/downloads/) **Uwaga: Ponowne uruchomienie komputera po zainstalowaniu zestawu SDK.**</span><span class="sxs-lookup"><span data-stu-id="6e934-112">[Microsoft Azure SDK for Visual Studio](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="6e934-113">Subskrypcja Microsoft Azure: Aby utworzyć bezpłatne konto wersji próbnej platformy Azure, zobacz [bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="6e934-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="6e934-114">Tworzenie aplikacji w roli sieci Web platformy Azure, który udostępnia liczników wydajności SignalR</span><span class="sxs-lookup"><span data-stu-id="6e934-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="6e934-115">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6e934-115">Open Visual Studio.</span></span>

2. <span data-ttu-id="6e934-116">W programie Visual Studio, wybierz **pliku** > **New** > **projektu**.</span><span class="sxs-lookup"><span data-stu-id="6e934-116">In Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="6e934-117">W **nowy projekt** okno dialogowe, wybierz opcję **Visual C#** > **chmury** kategorii po lewej stronie, a następnie wybierz pozycję **usługi w chmurze Azure** szablonu.</span><span class="sxs-lookup"><span data-stu-id="6e934-117">In the **New Project** dialog box, select the **Visual C#** > **Cloud** category on the left, and then select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="6e934-118">Określanie nazwy aplikacji **SignalRPerfCounters** i wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="6e934-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Nowa aplikacja w chmurze](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > <span data-ttu-id="6e934-120">Jeśli nie widzisz **chmury** kategorii szablonu lub **usługa w chmurze** szablonu, musisz zainstalować **programowanie na platformie Azure** obciążenia w programie Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="6e934-120">If you don't see the **Cloud** template category or the **Azure Cloud Service** template, you need to install the **Azure development** workload for Visual Studio 2017.</span></span> <span data-ttu-id="6e934-121">Wybierz **Otwórz Instalator programu Visual Studio** łącze w lewym dolnym rogu **nowy projekt** Instalatora programu Visual Studio Otwórz okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="6e934-121">Choose the **Open Visual Studio Installer** link on the bottom-left side of the **New Project** dialog to open Visual Studio Installer.</span></span> <span data-ttu-id="6e934-122">Wybierz **programowanie na platformie Azure** obciążenia, a następnie wybierz **Modyfikuj** do uruchomienia obciążenia.</span><span class="sxs-lookup"><span data-stu-id="6e934-122">Select the **Azure development** workload, and then choose **Modify** to start installing the workload.</span></span>
   >
   > ![Obciążenie programistyczne platformy Azure w Instalatorze programu Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. <span data-ttu-id="6e934-124">W **nową usługę w chmurze Azure Microsoft** okno dialogowe, wybierz opcję **Role sieci Web ASP.NET** i wybierz > przycisk, aby dodać rolę do projektu.</span><span class="sxs-lookup"><span data-stu-id="6e934-124">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="6e934-125">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6e934-125">Select **OK**.</span></span>

   ![Dodaj rolę sieci Web platformy ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. <span data-ttu-id="6e934-127">W **nowej aplikacji sieci Web ASP.NET - WebRole1** okno dialogowe, wybierz opcję **MVC** szablonu, a następnie wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="6e934-127">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![Dodawanie MVC i Web API](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. <span data-ttu-id="6e934-129">W **Eksploratora rozwiązań**, otwórz *diagnostics.wadcfgx* plik **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="6e934-129">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Diagnostics.wadcfgx Eksploratora rozwiązań](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. <span data-ttu-id="6e934-131">Zastąp zawartość pliku o następującej konfiguracji, a następnie zapisz plik:</span><span class="sxs-lookup"><span data-stu-id="6e934-131">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. <span data-ttu-id="6e934-132">Otwórz **Konsola Menedżera pakietów** z **narzędzia** > **Menedżera pakietów NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6e934-132">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="6e934-133">Wprowadź następujące polecenia, aby zainstalować najnowszą wersję biblioteki SignalR i pakietu Narzędzia SignalR:</span><span class="sxs-lookup"><span data-stu-id="6e934-133">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. <span data-ttu-id="6e934-134">Skonfigurować aplikację można zainstalować liczników wydajności SignalR w wystąpieniu roli, podczas uruchamiania lub odtwarzania.</span><span class="sxs-lookup"><span data-stu-id="6e934-134">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="6e934-135">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebRole1** projektu, a następnie wybierz **Dodaj** > **nowy Folder**.</span><span class="sxs-lookup"><span data-stu-id="6e934-135">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="6e934-136">Nadaj nazwę nowego folderu *uruchamiania*.</span><span class="sxs-lookup"><span data-stu-id="6e934-136">Name the new folder *Startup*.</span></span>

   ![Dodaj Folder początkowy](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. <span data-ttu-id="6e934-138">Kopiuj *signalr.exe* pliku (dodane za pomocą **Microsoft.AspNet.SignalR.Utils** pakietu) z \<folderu projektu > / SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\< w wersji > / narzędzi do *uruchamiania* folderu utworzonego w poprzednim kroku.</span><span class="sxs-lookup"><span data-stu-id="6e934-138">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="6e934-139">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *uruchamiania* i wybierz polecenie **Dodaj** > **istniejący element**.</span><span class="sxs-lookup"><span data-stu-id="6e934-139">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="6e934-140">W wyświetlonym oknie dialogowym wybierz *signalr.exe* i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6e934-140">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Dodaj signalr.exe do projektu](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. <span data-ttu-id="6e934-142">Kliknij prawym przyciskiem myszy *uruchamiania* utworzony folder.</span><span class="sxs-lookup"><span data-stu-id="6e934-142">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="6e934-143">Wybierz **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="6e934-143">Select **Add** > **New Item**.</span></span> <span data-ttu-id="6e934-144">Wybierz **ogólne** węzeł **plik tekstowy**i nadaj nazwę nowego elementu *SignalRPerfCounterInstall.cmd*.</span><span class="sxs-lookup"><span data-stu-id="6e934-144">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="6e934-145">Ten plik polecenia zainstaluje liczników wydajności SignalR w roli sieci web.</span><span class="sxs-lookup"><span data-stu-id="6e934-145">This command file will install the SignalR performance counters into the web role.</span></span>

    ![Utwórz plik wsadowy instalacji liczników wydajności SignalR](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. <span data-ttu-id="6e934-147">Gdy program Visual Studio tworzy *SignalRPerfCounterInstall.cmd* pliku, zostanie automatycznie otwarty w głównym oknie.</span><span class="sxs-lookup"><span data-stu-id="6e934-147">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="6e934-148">Zastąp zawartość pliku następującym skryptem, a następnie zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="6e934-148">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="6e934-149">Ten skrypt jest wykonywany *signalr.exe*, która dodaje liczników wydajności SignalR do wystąpienia roli.</span><span class="sxs-lookup"><span data-stu-id="6e934-149">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. <span data-ttu-id="6e934-150">Wybierz *signalr.exe* w pliku **Eksploratora rozwiązań**.</span><span class="sxs-lookup"><span data-stu-id="6e934-150">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="6e934-151">W pliku **właściwości**ustaw **Kopiuj do katalogu wyjściowego** do **zawsze Kopiuj**.</span><span class="sxs-lookup"><span data-stu-id="6e934-151">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Ustaw Kopiuj do katalogu wyjściowego, aby zawsze Kopiuj](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. <span data-ttu-id="6e934-153">Powtórz poprzedni krok dla *SignalRPerfCounterInstall.cmd* pliku.</span><span class="sxs-lookup"><span data-stu-id="6e934-153">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>


16. <span data-ttu-id="6e934-154">Kliknij prawym przyciskiem myszy *SignalRPerfCounterInstall.cmd* plik i wybierz **Otwórz za pomocą**.</span><span class="sxs-lookup"><span data-stu-id="6e934-154">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="6e934-155">W wyświetlonym oknie dialogowym wybierz **Edytor plików binarnych** i wybierz **OK**.</span><span class="sxs-lookup"><span data-stu-id="6e934-155">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![Otwórz za pomocą edytora pliku binarnego](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. <span data-ttu-id="6e934-157">W edytorze binarnym zaznacz wszystkie wiodące bajty w pliku i usuń je.</span><span class="sxs-lookup"><span data-stu-id="6e934-157">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="6e934-158">Zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="6e934-158">Save and close the file.</span></span>

    ![Usuń bajty wiodące](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. <span data-ttu-id="6e934-160">Otwórz *ServiceDefinition.csdef* i Dodaj zadanie uruchamiania, który jest wykonywany *SignalrPerfCounterInstall.cmd* pliku podczas uruchamiania usługi:</span><span class="sxs-lookup"><span data-stu-id="6e934-160">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. <span data-ttu-id="6e934-161">Otwórz `Views/Shared/_Layout.cshtml` i Usuń skrypt pakietu jQuery od końca pliku.</span><span class="sxs-lookup"><span data-stu-id="6e934-161">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. <span data-ttu-id="6e934-162">Dodaj klienta JavaScript, która stale wywołuje `increment` metody na serwerze.</span><span class="sxs-lookup"><span data-stu-id="6e934-162">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="6e934-163">Otwórz `Views/Home/Index.cshtml` i zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6e934-163">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. <span data-ttu-id="6e934-164">Utwórz nowy folder w **WebRole1** projektu o nazwie *koncentratory*.</span><span class="sxs-lookup"><span data-stu-id="6e934-164">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="6e934-165">Kliknij prawym przyciskiem myszy *Hubs* folderu w **Eksploratora rozwiązań** i wybierz **Dodaj** > **nowy element**.</span><span class="sxs-lookup"><span data-stu-id="6e934-165">Right-click the *Hubs* folder in **Solution Explorer** and select **Add** > **New Item**.</span></span> <span data-ttu-id="6e934-166">W **Dodaj nowy element** okno dialogowe, wybierz opcję **Web** > **SignalR** kategorii, a następnie wybierz **klasa Centrum SignalR (v2)** szablon elementu.</span><span class="sxs-lookup"><span data-stu-id="6e934-166">In the **Add New Item** dialog box, select the **Web** > **SignalR** category, and then select the **SignalR Hub Class (v2)** item template.</span></span> <span data-ttu-id="6e934-167">Nazwa nowego centrum *MyHub.cs* i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6e934-167">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Dodawanie klasy koncentratora SignalR do folderu koncentratory, w oknie dialogowym Dodaj nowy element](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="6e934-169">*MyHub.cs* zostanie automatycznie otwarte w głównym oknie.</span><span class="sxs-lookup"><span data-stu-id="6e934-169">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="6e934-170">Zastąp zawartość następującym kodem, a następnie zapisz i zamknij plik:</span><span class="sxs-lookup"><span data-stu-id="6e934-170">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. <span data-ttu-id="6e934-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)*  zagęszczenie połączenia testowania narzędzie dołączonym SignalR codebase.</span><span class="sxs-lookup"><span data-stu-id="6e934-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="6e934-172">Ponieważ węzłówką wymaga trwałego połączenia, możesz dodać je do witryny do użycia podczas testowania.</span><span class="sxs-lookup"><span data-stu-id="6e934-172">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="6e934-173">Dodaj nowy folder w celu **WebRole1** projekt o nazwie *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="6e934-173">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="6e934-174">Kliknij prawym przyciskiem myszy ten folder, a następnie wybierz pozycję **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="6e934-174">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="6e934-175">Nadaj nowemu plikowi klasy *MyPersistentConnections.cs* i wybierz **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="6e934-175">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="6e934-176">Zostanie otwarty program Visual Studio *MyPersistentConnections.cs* pliku w oknie głównym.</span><span class="sxs-lookup"><span data-stu-id="6e934-176">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="6e934-177">Zastąp zawartość następującym kodem, a następnie zapisz i zamknij plik:</span><span class="sxs-lookup"><span data-stu-id="6e934-177">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. <span data-ttu-id="6e934-178">Za pomocą `Startup` klasy obiektów SignalR rozpoczyna się w momencie uruchamiania OWIN.</span><span class="sxs-lookup"><span data-stu-id="6e934-178">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="6e934-179">Otwórz lub Utwórz *Startup.cs* i zastąp jego zawartość następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="6e934-179">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    <span data-ttu-id="6e934-180">W powyższym kodzie `OwinStartup` atrybut oznacza tej klasy, aby uruchomić OWIN.</span><span class="sxs-lookup"><span data-stu-id="6e934-180">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="6e934-181">`Configuration` Metoda uruchamia SignalR.</span><span class="sxs-lookup"><span data-stu-id="6e934-181">The `Configuration` method starts SignalR.</span></span>

26. <span data-ttu-id="6e934-182">Przetestuj aplikację w emulatorze Microsoft Azure, naciskając klawisz **F5**.</span><span class="sxs-lookup"><span data-stu-id="6e934-182">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6e934-183">Jeśli napotkasz **fileloadexception —** na **MapSignalR**, zmień przekierowania powiązań w *web.config* do następującego:</span><span class="sxs-lookup"><span data-stu-id="6e934-183">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. <span data-ttu-id="6e934-184">Poczekaj około jednej minuty.</span><span class="sxs-lookup"><span data-stu-id="6e934-184">Wait about one minute.</span></span> <span data-ttu-id="6e934-185">Otwórz okno narzędzia programu Cloud Explorer w programie Visual Studio (**widoku** > **programu Cloud Explorer**) i Rozwiń ścieżkę `(Local)/Storage Accounts/(Development)/Tables`.</span><span class="sxs-lookup"><span data-stu-id="6e934-185">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="6e934-186">Kliknij dwukrotnie **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="6e934-186">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="6e934-187">Powinien zostać wyświetlony liczników SignalR w danych tabeli.</span><span class="sxs-lookup"><span data-stu-id="6e934-187">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="6e934-188">Jeśli nie widzisz tabeli może być konieczne ponownie wprowadzić swoje poświadczenia usługi Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6e934-188">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="6e934-189">Musisz wybrać **Odśwież** przycisk, aby wyświetlić tabelę w **programu Cloud Explorer** lub wybierz **Odśwież** przycisk w oknie Otwórz tabelę, aby wyświetlić dane w tabeli.</span><span class="sxs-lookup"><span data-stu-id="6e934-189">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Wybieranie tabeli WAD liczników wydajności w programie Cloud Explorer programu Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Pokazywanie liczników zbieranych w tabeli liczniki wydajności funkcji WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. <span data-ttu-id="6e934-192">Aby przetestować aplikację w chmurze, należy zaktualizować **ServiceConfiguration.Cloud.cscfg** plik i ustaw `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` na prawidłowe parametry połączenia konta usługi Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6e934-192">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="6e934-193">Wdrażanie aplikacji do subskrypcji platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="6e934-193">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="6e934-194">Aby uzyskać więcej informacji na temat sposobu wdrażania aplikacji na platformie Azure, zobacz [jak utworzyć i wdrożyć usługę w chmurze](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="6e934-194">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="6e934-195">Poczekaj kilka minut.</span><span class="sxs-lookup"><span data-stu-id="6e934-195">Wait a few minutes.</span></span> <span data-ttu-id="6e934-196">W **programu Cloud Explorer**Znajdź konto magazynu skonfigurowanych powyżej i Znajdź `WADPerformanceCountersTable` tabeli w nim.</span><span class="sxs-lookup"><span data-stu-id="6e934-196">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="6e934-197">Powinien zostać wyświetlony liczników SignalR w danych tabeli.</span><span class="sxs-lookup"><span data-stu-id="6e934-197">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="6e934-198">Jeśli nie widzisz tabeli może być konieczne ponownie wprowadzić swoje poświadczenia usługi Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="6e934-198">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="6e934-199">Musisz wybrać **Odśwież** przycisk, aby wyświetlić tabelę w **programu Cloud Explorer** lub wybierz **Odśwież** przycisk w oknie Otwórz tabelę, aby wyświetlić dane w tabeli.</span><span class="sxs-lookup"><span data-stu-id="6e934-199">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="6e934-200">Specjalne podziękowania dla [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) dla oryginalnej zawartości, które są używane w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="6e934-200">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
