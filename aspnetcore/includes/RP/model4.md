<span data-ttu-id="ae0cd-101">W poniższej tabeli przedstawiono parametry generator kodu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="ae0cd-101">The following table details the ASP.NET Core code generator parameters:</span></span>

| <span data-ttu-id="ae0cd-102">Parametr</span><span class="sxs-lookup"><span data-stu-id="ae0cd-102">Parameter</span></span>               | <span data-ttu-id="ae0cd-103">Opis</span><span class="sxs-lookup"><span data-stu-id="ae0cd-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="ae0cd-104">-m</span><span class="sxs-lookup"><span data-stu-id="ae0cd-104">-m</span></span>  | <span data-ttu-id="ae0cd-105">Nazwa modelu.</span><span class="sxs-lookup"><span data-stu-id="ae0cd-105">The name of the model.</span></span> |
| <span data-ttu-id="ae0cd-106">-dc</span><span class="sxs-lookup"><span data-stu-id="ae0cd-106">-dc</span></span>  | <span data-ttu-id="ae0cd-107">Kontekst danych.</span><span class="sxs-lookup"><span data-stu-id="ae0cd-107">The data context.</span></span> |
| <span data-ttu-id="ae0cd-108">-udl</span><span class="sxs-lookup"><span data-stu-id="ae0cd-108">-udl</span></span> | <span data-ttu-id="ae0cd-109">Użyj domyślnego układu.</span><span class="sxs-lookup"><span data-stu-id="ae0cd-109">Use the default layout.</span></span> |
| <span data-ttu-id="ae0cd-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="ae0cd-110">-outDir</span></span> | <span data-ttu-id="ae0cd-111">Ścieżka folderu wyjściowego względne do tworzenia widoków.</span><span class="sxs-lookup"><span data-stu-id="ae0cd-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="ae0cd-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="ae0cd-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="ae0cd-113">Dodaje `_ValidationScriptsPartial` to edytowanie i tworzenie stron</span><span class="sxs-lookup"><span data-stu-id="ae0cd-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="ae0cd-114">Użyj `h` przełącznika, aby uzyskać pomoc dotyczącą `aspnet-codegenerator razorpage` polecenia:</span><span class="sxs-lookup"><span data-stu-id="ae0cd-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="ae0cd-115">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ae0cd-115">Test the app</span></span>

* <span data-ttu-id="ae0cd-116">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/Movies`).</span><span class="sxs-lookup"><span data-stu-id="ae0cd-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/Movies`).</span></span>
* <span data-ttu-id="ae0cd-117">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="ae0cd-117">Test the **Create** link.</span></span>

  ![Tworzenie strony](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="ae0cd-119">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="ae0cd-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="ae0cd-120">Jeśli komunikat o błędzie podobny do poniższego, sprawdź mają Uruchom migracje i aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="ae0cd-120">If you get the error similar to the following, verify you have run migrations and updated the database:</span></span>

`An unhandled exception occurred while processing the request. 'no such table: Movie'.`
