<span data-ttu-id="6822d-101">Poniższe szczegóły tabeli platformy ASP.NET Core kodu generatorów parametry:</span><span class="sxs-lookup"><span data-stu-id="6822d-101">The following table details the ASP.NET Core code generators\` parameters:</span></span>

| <span data-ttu-id="6822d-102">Parametr</span><span class="sxs-lookup"><span data-stu-id="6822d-102">Parameter</span></span>               | <span data-ttu-id="6822d-103">Opis</span><span class="sxs-lookup"><span data-stu-id="6822d-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="6822d-104">-m</span><span class="sxs-lookup"><span data-stu-id="6822d-104">-m</span></span>  | <span data-ttu-id="6822d-105">Nazwa modelu.</span><span class="sxs-lookup"><span data-stu-id="6822d-105">The name of the model.</span></span> |
| <span data-ttu-id="6822d-106">-dc</span><span class="sxs-lookup"><span data-stu-id="6822d-106">-dc</span></span>  | <span data-ttu-id="6822d-107">Kontekst danych.</span><span class="sxs-lookup"><span data-stu-id="6822d-107">The data context.</span></span> |
| <span data-ttu-id="6822d-108">-udl</span><span class="sxs-lookup"><span data-stu-id="6822d-108">-udl</span></span> | <span data-ttu-id="6822d-109">Użyj domyślnego układu.</span><span class="sxs-lookup"><span data-stu-id="6822d-109">Use the default layout.</span></span> |
| <span data-ttu-id="6822d-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="6822d-110">-outDir</span></span> | <span data-ttu-id="6822d-111">Ścieżka folderu wyjściowego względne do tworzenia widoków.</span><span class="sxs-lookup"><span data-stu-id="6822d-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="6822d-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="6822d-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="6822d-113">Dodaje `_ValidationScriptsPartial` to edytowanie i tworzenie stron</span><span class="sxs-lookup"><span data-stu-id="6822d-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="6822d-114">Użyj `h` przełącznika, aby uzyskać pomoc dotyczącą `aspnet-codegenerator razorpage` polecenia:</span><span class="sxs-lookup"><span data-stu-id="6822d-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="6822d-115">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="6822d-115">Test the app</span></span>

* <span data-ttu-id="6822d-116">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="6822d-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="6822d-117">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="6822d-117">Test the **Create** link.</span></span>

  ![Tworzenie strony](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="6822d-119">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="6822d-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="6822d-120">Jeśli komunikat o błędzie podobny do poniższego, sprawdź mają Uruchom migracje i aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="6822d-120">If you get the error similar to the following, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
'no such table: Movie'.
```
