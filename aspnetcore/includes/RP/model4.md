<a name="codegenerator"></a><span data-ttu-id="8bf02-101">W poniższej tabeli przedstawiono szczegóły ASP.NET Core parametrów generatora kodu:</span><span class="sxs-lookup"><span data-stu-id="8bf02-101">The following table details the ASP.NET Core code generator parameters:</span></span>

| <span data-ttu-id="8bf02-102">Parametr</span><span class="sxs-lookup"><span data-stu-id="8bf02-102">Parameter</span></span>               | <span data-ttu-id="8bf02-103">Opis</span><span class="sxs-lookup"><span data-stu-id="8bf02-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="8bf02-104">-m</span><span class="sxs-lookup"><span data-stu-id="8bf02-104">-m</span></span>  | <span data-ttu-id="8bf02-105">Nazwa modelu.</span><span class="sxs-lookup"><span data-stu-id="8bf02-105">The name of the model.</span></span> |
| <span data-ttu-id="8bf02-106">-DC</span><span class="sxs-lookup"><span data-stu-id="8bf02-106">-dc</span></span>  | <span data-ttu-id="8bf02-107">Klasa `DbContext` , która ma zostać użyta.</span><span class="sxs-lookup"><span data-stu-id="8bf02-107">The `DbContext` class to use.</span></span> |
| <span data-ttu-id="8bf02-108">-UDL</span><span class="sxs-lookup"><span data-stu-id="8bf02-108">-udl</span></span> | <span data-ttu-id="8bf02-109">Użyj układu domyślnego.</span><span class="sxs-lookup"><span data-stu-id="8bf02-109">Use the default layout.</span></span> |
| <span data-ttu-id="8bf02-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="8bf02-110">-outDir</span></span> | <span data-ttu-id="8bf02-111">Ścieżka względna folderu wyjściowego do tworzenia widoków.</span><span class="sxs-lookup"><span data-stu-id="8bf02-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="8bf02-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="8bf02-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="8bf02-113">Dodaje `_ValidationScriptsPartial` do edycji i tworzenia stron</span><span class="sxs-lookup"><span data-stu-id="8bf02-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="8bf02-114">Użyj przełącznika `h` , aby uzyskać pomoc `aspnet-codegenerator razorpage` dotyczącą polecenia:</span><span class="sxs-lookup"><span data-stu-id="8bf02-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```

<span data-ttu-id="8bf02-115">Aby uzyskać więcej informacji, zobacz [dotnet ASPNET-CodeGenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator)</span><span class="sxs-lookup"><span data-stu-id="8bf02-115">For more information, see [dotnet aspnet-codegenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator)</span></span> 