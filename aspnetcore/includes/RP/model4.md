<span data-ttu-id="d8c2a-101">Poniższe szczegóły tabeli platformy ASP.NET Core code generatory parametry:</span><span class="sxs-lookup"><span data-stu-id="d8c2a-101">The following table details the ASP.NET Core code generators\` parameters:</span></span>

| <span data-ttu-id="d8c2a-102">Parametr</span><span class="sxs-lookup"><span data-stu-id="d8c2a-102">Parameter</span></span>               | <span data-ttu-id="d8c2a-103">Opis</span><span class="sxs-lookup"><span data-stu-id="d8c2a-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="d8c2a-104">-m</span><span class="sxs-lookup"><span data-stu-id="d8c2a-104">-m</span></span>  | <span data-ttu-id="d8c2a-105">Nazwa modelu.</span><span class="sxs-lookup"><span data-stu-id="d8c2a-105">The name of the model.</span></span> |
| <span data-ttu-id="d8c2a-106">-dc</span><span class="sxs-lookup"><span data-stu-id="d8c2a-106">-dc</span></span>  | <span data-ttu-id="d8c2a-107">Kontekst danych.</span><span class="sxs-lookup"><span data-stu-id="d8c2a-107">The data context.</span></span> |
| <span data-ttu-id="d8c2a-108">-udl</span><span class="sxs-lookup"><span data-stu-id="d8c2a-108">-udl</span></span> | <span data-ttu-id="d8c2a-109">Użyj domyślnego układu.</span><span class="sxs-lookup"><span data-stu-id="d8c2a-109">Use the default layout.</span></span> |
| <span data-ttu-id="d8c2a-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="d8c2a-110">-outDir</span></span> | <span data-ttu-id="d8c2a-111">Ścieżka folderu względna dane wyjściowe do tworzenia widoków.</span><span class="sxs-lookup"><span data-stu-id="d8c2a-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="d8c2a-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="d8c2a-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="d8c2a-113">Dodaje `_ValidationScriptsPartial` można edytować i tworzyć strony</span><span class="sxs-lookup"><span data-stu-id="d8c2a-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="d8c2a-114">Użyj `h` przełącznik, aby uzyskać pomoc dotyczącą `aspnet-codegenerator razorpage` polecenia:</span><span class="sxs-lookup"><span data-stu-id="d8c2a-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="d8c2a-115">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="d8c2a-115">Test the app</span></span>

* <span data-ttu-id="d8c2a-116">Uruchom aplikację i Dołącz `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="d8c2a-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="d8c2a-117">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="d8c2a-117">Test the **Create** link.</span></span>

  ![Tworzenie strony](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="d8c2a-119">Test **Edytuj**, **szczegóły**, i **usunąć** łącza.</span><span class="sxs-lookup"><span data-stu-id="d8c2a-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="d8c2a-120">Jeśli komunikat o błędzie podobny do następującego, sprawdź zostało uruchomione migracji i aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="d8c2a-120">If you get the error similar to the following, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
'no such table: Movie'.
```
