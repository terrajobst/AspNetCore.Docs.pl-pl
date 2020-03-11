# <a name="visual-studio"></a>[<span data-ttu-id="c5e86-101">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c5e86-101">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="c5e86-102">Tworzenie nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="c5e86-102">Create a new project.</span></span>
1. <span data-ttu-id="c5e86-103">Wybierz pozycję **Usługa procesu roboczego**.</span><span class="sxs-lookup"><span data-stu-id="c5e86-103">Select **Worker Service**.</span></span> <span data-ttu-id="c5e86-104">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="c5e86-104">Select **Next**.</span></span>
1. <span data-ttu-id="c5e86-105">Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu.</span><span class="sxs-lookup"><span data-stu-id="c5e86-105">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="c5e86-106">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="c5e86-106">Select **Create**.</span></span>
1. <span data-ttu-id="c5e86-107">W oknie dialogowym **Tworzenie nowej usługi procesu roboczego** wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="c5e86-107">In the **Create a new Worker service** dialog, select **Create**.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="c5e86-108">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="c5e86-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="c5e86-109">Tworzenie nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="c5e86-109">Create a new project.</span></span>
1. <span data-ttu-id="c5e86-110">Na pasku bocznym wybierz pozycję **aplikacja** na **platformie .NET Core** .</span><span class="sxs-lookup"><span data-stu-id="c5e86-110">Select **App** under **.NET Core** in the sidebar.</span></span>
1. <span data-ttu-id="c5e86-111">Wybierz pozycję **proces roboczy** w obszarze **ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c5e86-111">Select **Worker** under **ASP.NET Core**.</span></span> <span data-ttu-id="c5e86-112">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="c5e86-112">Select **Next**.</span></span>
1. <span data-ttu-id="c5e86-113">Wybierz **platformę .NET Core 3,0** lub nowszą dla **platformy docelowej**.</span><span class="sxs-lookup"><span data-stu-id="c5e86-113">Select **.NET Core 3.0** or later for the **Target Framework**.</span></span> <span data-ttu-id="c5e86-114">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="c5e86-114">Select **Next**.</span></span>
1. <span data-ttu-id="c5e86-115">Podaj nazwę w polu **Nazwa projektu** .</span><span class="sxs-lookup"><span data-stu-id="c5e86-115">Provide a name in the **Project Name** field.</span></span> <span data-ttu-id="c5e86-116">Wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="c5e86-116">Select **Create**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="c5e86-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c5e86-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c5e86-118">Użyj szablonu usługi procesu roboczego (`worker`) z poleceniem [dotnet New](/dotnet/core/tools/dotnet-new) z powłoki poleceń.</span><span class="sxs-lookup"><span data-stu-id="c5e86-118">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="c5e86-119">W poniższym przykładzie jest tworzona aplikacja usługi Worker o nazwie `ContosoWorker`.</span><span class="sxs-lookup"><span data-stu-id="c5e86-119">In the following example, a Worker Service app is created named `ContosoWorker`.</span></span> <span data-ttu-id="c5e86-120">Folder dla aplikacji `ContosoWorker` jest tworzony automatycznie podczas wykonywania polecenia.</span><span class="sxs-lookup"><span data-stu-id="c5e86-120">A folder for the `ContosoWorker` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorker
```

---
