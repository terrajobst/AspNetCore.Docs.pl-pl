# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1278d-101">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1278d-101">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1278d-102">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="1278d-102">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="1278d-103">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="1278d-103">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="1278d-104">Na pasku adresu są wyświetlane `localhost:port#` a nie elementy, takie jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="1278d-104">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="1278d-105">Wynika to z faktu, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="1278d-105">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="1278d-106">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="1278d-106">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="1278d-107">Podczas tworzenia projektu internetowego w programie Visual Studio dla serwera internetowego jest używany losowy port.</span><span class="sxs-lookup"><span data-stu-id="1278d-107">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1278d-108">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1278d-108">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="1278d-109">Naciśnij **klawisze CTRL + F5** , aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="1278d-109">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="1278d-110">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="1278d-110">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="1278d-111">Na pasku adresu są wyświetlane `localhost:port#` a nie elementy, takie jak `example.com`.</span><span class="sxs-lookup"><span data-stu-id="1278d-111">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="1278d-112">Wynika to z faktu, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="1278d-112">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="1278d-113">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="1278d-113">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1278d-114">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="1278d-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="1278d-115">W programie Visual Studio naciśnij pozycję **opt-cmd-Return** , aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="1278d-115">From Visual Studio, press **Opt-Cmd-Return** to run without the debugger.</span></span> <span data-ttu-id="1278d-116">Alternatywnie przejdź do paska menu i przejdź do pozycji **uruchom > Uruchom bez debugowania**.</span><span class="sxs-lookup"><span data-stu-id="1278d-116">Alternatively, navigate to the menu bar and go to **Run>Start Without Debugging**.</span></span>

  <span data-ttu-id="1278d-117">Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="1278d-117">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---