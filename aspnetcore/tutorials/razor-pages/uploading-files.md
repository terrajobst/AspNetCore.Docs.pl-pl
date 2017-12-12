---
title: "Przekazywanie plików do Razor strony platformy ASP.NET Core"
author: guardrex
description: "Dowiedz się, jak przekazać pliki do strony Razor."
keywords: "Platformy ASP.NET Core, Razor, stron Razor, IFormFile przekazywania pliku, przekazywaniem plików"
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 3b54bf0b40c396c8c141966219f65231fb362ca4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="22abd-104">Przekazywanie plików do Razor strony platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="22abd-104">Uploading files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="22abd-105">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="22abd-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="22abd-106">W tej sekcji przedstawiono przekazywania plików ze stroną Razor.</span><span class="sxs-lookup"><span data-stu-id="22abd-106">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="22abd-107">[Filmu stron Razor Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) w ten samouczek używa modelu prostego powiązania Aby przekazać pliki, które działa dobrze w przypadku przekazywania małych plików.</span><span class="sxs-lookup"><span data-stu-id="22abd-107">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="22abd-108">Aby uzyskać informacje na przesyłanie strumieniowe dużych plików, zobacz [przekazywania dużych plików z przesyłania strumieniowego](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="22abd-108">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="22abd-109">W poniższych krokach należy dodać do przykładowej aplikacji funkcji przekazywania pliku filmu harmonogramu.</span><span class="sxs-lookup"><span data-stu-id="22abd-109">In the steps below, you add a movie schedule file upload feature to the sample app.</span></span> <span data-ttu-id="22abd-110">Harmonogram film jest reprezentowana przez `Schedule` klasy.</span><span class="sxs-lookup"><span data-stu-id="22abd-110">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="22abd-111">Klasa zawiera dwie wersje harmonogramu.</span><span class="sxs-lookup"><span data-stu-id="22abd-111">The class includes two versions of the schedule.</span></span> <span data-ttu-id="22abd-112">Jednej wersji są przekazywane klientom, `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="22abd-112">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="22abd-113">Druga wersja jest używana dla pracowników firmy `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="22abd-113">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="22abd-114">Każda wersja jest przekazany jako oddzielny plik.</span><span class="sxs-lookup"><span data-stu-id="22abd-114">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="22abd-115">Samouczek pokazuje, jak wykonać dwa przekazywania plików ze strony z jednego wpisu na serwerze.</span><span class="sxs-lookup"><span data-stu-id="22abd-115">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="22abd-116">Dodaj klasę przekazywaniem plików</span><span class="sxs-lookup"><span data-stu-id="22abd-116">Add a FileUpload class</span></span>

<span data-ttu-id="22abd-117">Poniżej utworzysz stronę Razor do obsługi parę przekazywania plików.</span><span class="sxs-lookup"><span data-stu-id="22abd-117">Below, you create a Razor page to handle a pair of file uploads.</span></span> <span data-ttu-id="22abd-118">Dodaj `FileUpload` klasy, która jest powiązana ze stroną uzyskać dane harmonogramu.</span><span class="sxs-lookup"><span data-stu-id="22abd-118">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="22abd-119">Kliknij prawym przyciskiem myszy *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="22abd-119">Right click the *Models* folder.</span></span> <span data-ttu-id="22abd-120">Wybierz **dodać** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="22abd-120">Select **Add** > **Class**.</span></span> <span data-ttu-id="22abd-121">Nazwa klasy **przekazywaniem plików** i dodaj następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="22abd-121">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="22abd-122">Klasa ma właściwość tytułu harmonogramu i właściwości dla każdego z dwóch wersji harmonogramu.</span><span class="sxs-lookup"><span data-stu-id="22abd-122">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="22abd-123">Wszystkie trzy właściwości są wymagane, i tytuł musi wynosić 3 – 60 znaków.</span><span class="sxs-lookup"><span data-stu-id="22abd-123">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="22abd-124">Dodaj metodę pomocnika, aby przekazać pliki</span><span class="sxs-lookup"><span data-stu-id="22abd-124">Add a helper method to upload files</span></span>

<span data-ttu-id="22abd-125">Aby uniknąć zduplikowania kodu do przetwarzania plików przekazane harmonogramu, najpierw Dodaj metodę pomocnika statycznych.</span><span class="sxs-lookup"><span data-stu-id="22abd-125">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="22abd-126">Utwórz *narzędzia* folderu w aplikacji i Dodaj *FileHelpers.cs* pliku o następującej zawartości.</span><span class="sxs-lookup"><span data-stu-id="22abd-126">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="22abd-127">Metoda pomocnika `ProcessFormFile`, przyjmuje [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) i [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) i zwraca ciąg zawierający rozmiar pliku i jego zawartości.</span><span class="sxs-lookup"><span data-stu-id="22abd-127">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="22abd-128">Typ zawartości i długości są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="22abd-128">The content type and length are checked.</span></span> <span data-ttu-id="22abd-129">Jeśli plik nie przeszły sprawdzanie poprawności, błąd jest dodawany do `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="22abd-129">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

## <a name="add-the-schedule-class"></a><span data-ttu-id="22abd-130">Dodaj klasę harmonogramu</span><span class="sxs-lookup"><span data-stu-id="22abd-130">Add the Schedule class</span></span>

<span data-ttu-id="22abd-131">Kliknij prawym przyciskiem myszy *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="22abd-131">Right click the *Models* folder.</span></span> <span data-ttu-id="22abd-132">Wybierz **dodać** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="22abd-132">Select **Add** > **Class**.</span></span> <span data-ttu-id="22abd-133">Nazwa klasy **harmonogram** i dodaj następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="22abd-133">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="22abd-134">Używa klasy `Display` i `DisplayFormat` atrybuty, które przyjazną tytułów i formatowania podczas renderowania danych harmonogramu.</span><span class="sxs-lookup"><span data-stu-id="22abd-134">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="22abd-135">Aktualizacja MovieContext</span><span class="sxs-lookup"><span data-stu-id="22abd-135">Update the MovieContext</span></span>

<span data-ttu-id="22abd-136">Określ `DbSet` w `MovieContext` (*Models/MovieContext.cs*) dla harmonogramów:</span><span class="sxs-lookup"><span data-stu-id="22abd-136">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="22abd-137">Dodaj tabelę harmonogramu do bazy danych</span><span class="sxs-lookup"><span data-stu-id="22abd-137">Add the Schedule table to the database</span></span>

<span data-ttu-id="22abd-138">Otwórz konsolę Menedżera pakietów (PMC): **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="22abd-138">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![PMC menu](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="22abd-140">W kryterium wykonaj następujące polecenia.</span><span class="sxs-lookup"><span data-stu-id="22abd-140">In the PMC, execute the following commands.</span></span> <span data-ttu-id="22abd-141">Te polecenia powodują dodanie `Schedule` tabeli w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="22abd-141">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="22abd-142">Dodaj stronę Razor przekazywania pliku</span><span class="sxs-lookup"><span data-stu-id="22abd-142">Add a file upload Razor Page</span></span>

<span data-ttu-id="22abd-143">W *stron* folderu, Utwórz *harmonogramy* folderu.</span><span class="sxs-lookup"><span data-stu-id="22abd-143">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="22abd-144">W *harmonogramy* folderu, Utwórz stronę o nazwie *Index.cshtml* o przekazywaniu harmonogram o następującej treści:</span><span class="sxs-lookup"><span data-stu-id="22abd-144">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="22abd-145">Każda grupa formularz zawiera  **\<Etykieta >** który wyświetla nazwę każda właściwość klasy.</span><span class="sxs-lookup"><span data-stu-id="22abd-145">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="22abd-146">`Display` Atrybutów w `FileUpload` modelu Podaj wartości wyświetlania etykiet.</span><span class="sxs-lookup"><span data-stu-id="22abd-146">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="22abd-147">Na przykład `UploadPublicSchedule` nazwy wyświetlanej właściwości ustawiono `[Display(Name="Public Schedule")]` i w związku z tym Wyświetla "Harmonogram publiczny" w etykiecie podczas renderowania formularza.</span><span class="sxs-lookup"><span data-stu-id="22abd-147">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="22abd-148">Każda grupa formularz zawiera weryfikacji  **\<span >**.</span><span class="sxs-lookup"><span data-stu-id="22abd-148">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="22abd-149">Jeśli użytkownik wejściowych nie spełniają atrybuty właściwości ustawione `FileUpload` klasy lub jeśli któryś z `ProcessFormFile` sprawdzanie poprawności pliku metody kończyć się niepowodzeniem, model kończy się niepowodzeniem do sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="22abd-149">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="22abd-150">Podczas sprawdzania poprawności modelu nie powiedzie się, komunikat dotyczący sprawdzania poprawności pomocne jest renderowany do użytkownika.</span><span class="sxs-lookup"><span data-stu-id="22abd-150">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="22abd-151">Na przykład `Title` właściwość jest oznaczona przy `[Required]` i `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="22abd-151">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="22abd-152">Jeśli użytkownik nie może podać tytuł, otrzymają komunikat informujący, że wymagana jest wartość.</span><span class="sxs-lookup"><span data-stu-id="22abd-152">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="22abd-153">Jeśli użytkownik wprowadzi wartość mniej niż 3 znaków ani więcej niż 60 znaków, otrzymają komunikat informujący, że wartość ma nieprawidłową długość.</span><span class="sxs-lookup"><span data-stu-id="22abd-153">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="22abd-154">Jeśli plik jest pod warunkiem, że nie ma zawartości, zostanie wyświetlony komunikat, że plik jest pusty.</span><span class="sxs-lookup"><span data-stu-id="22abd-154">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-code-behind-file"></a><span data-ttu-id="22abd-155">Dodaj plik CodeBehind</span><span class="sxs-lookup"><span data-stu-id="22abd-155">Add the code-behind file</span></span>

<span data-ttu-id="22abd-156">Dodaj plik CodeBehind (*Index.cshtml.cs*) do *harmonogramy* folderu:</span><span class="sxs-lookup"><span data-stu-id="22abd-156">Add the code-behind file (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="22abd-157">Model strony (`IndexModel` w *Index.cshtml.cs*) wiąże `FileUpload` klasy:</span><span class="sxs-lookup"><span data-stu-id="22abd-157">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="22abd-158">Model korzysta również z listą harmonogramy (`IList<Schedule>`) do wyświetlenia przechowywanych w bazie danych na stronie harmonogramów:</span><span class="sxs-lookup"><span data-stu-id="22abd-158">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="22abd-159">Załadowanie strony z `OnGetAsync`, `Schedules` jest wypełnione z bazy danych i używane do generowania tabeli HTML załadować harmonogramów:</span><span class="sxs-lookup"><span data-stu-id="22abd-159">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="22abd-160">Gdy formularz jest przesyłana do serwera, `ModelState` jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="22abd-160">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="22abd-161">Jeśli jest to nieprawidłowa `Schedule` odbudowaniu i renderuje stronę z jednego lub więcej komunikatów dotyczących sprawdzania poprawności, podając, dlaczego nie można sprawdzić poprawności strony.</span><span class="sxs-lookup"><span data-stu-id="22abd-161">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="22abd-162">Jeśli jest prawidłowa, `FileUpload` właściwości są używane w *OnPostAsync* aby zakończyć przekazywanie plików dla obu wersji harmonogramu i utworzyć nową `Schedule` obiektu do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="22abd-162">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="22abd-163">Harmonogram jest następnie zapisywana w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="22abd-163">The schedule is then saved to the database:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="22abd-164">Link przekazywania pliku Razor strony</span><span class="sxs-lookup"><span data-stu-id="22abd-164">Link the file upload Razor Page</span></span>

<span data-ttu-id="22abd-165">Otwórz *_Layout.cshtml* i dodać łącze do paska nawigacyjnego, aby przejść do strony przekazywania plików:</span><span class="sxs-lookup"><span data-stu-id="22abd-165">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="22abd-166">Dodaj stronę, aby potwierdzić usunięcie harmonogramu</span><span class="sxs-lookup"><span data-stu-id="22abd-166">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="22abd-167">Gdy użytkownik kliknie przycisk, aby usunąć harmonogram, mają mieć możliwość anulowania operacji.</span><span class="sxs-lookup"><span data-stu-id="22abd-167">When the user clicks to delete a schedule, you want them to have a chance to cancel the operation.</span></span> <span data-ttu-id="22abd-168">Dodaj stronę potwierdzenia usunięcia (*Delete.cshtml*) do *harmonogramy* folderu:</span><span class="sxs-lookup"><span data-stu-id="22abd-168">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="22abd-169">Plik CodeBehind (*Delete.cshtml.cs*) ładuje jeden harmonogram identyfikowane przez `id` w danych trasy żądania.</span><span class="sxs-lookup"><span data-stu-id="22abd-169">The code-behind file (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="22abd-170">Dodaj *Delete.cshtml.cs* pliku *harmonogramy* folderu:</span><span class="sxs-lookup"><span data-stu-id="22abd-170">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="22abd-171">`OnPostAsync` Obsługuje metoda usuwania harmonogramu przez jego `id`:</span><span class="sxs-lookup"><span data-stu-id="22abd-171">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="22abd-172">Po pomyślnym usunięciu harmonogram, `RedirectToPage` wysyła użytkownika z powrotem do harmonogramów *Index.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="22abd-172">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="22abd-173">Pracy harmonogramy Razor strony</span><span class="sxs-lookup"><span data-stu-id="22abd-173">The working Schedules Razor Page</span></span>

<span data-ttu-id="22abd-174">Podczas ładowania strony, etykiety i dane wejściowe dla tytułu harmonogram harmonogram publicznego i prywatnego harmonogramu są renderowane z przycisk przesyłania:</span><span class="sxs-lookup"><span data-stu-id="22abd-174">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Planuje Razor strony, jak pokazano na ładowania początkowego bez błędów weryfikacji i puste pola](uploading-files/_static/browser1.png)

<span data-ttu-id="22abd-176">Wybieranie **przekazać** przycisk bez wypełniania pól narusza `[Required]` atrybutów w modelu.</span><span class="sxs-lookup"><span data-stu-id="22abd-176">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="22abd-177">`ModelState` Jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="22abd-177">The `ModelState` is invalid.</span></span> <span data-ttu-id="22abd-178">Komunikatów o błędach są wyświetlane dla użytkownika:</span><span class="sxs-lookup"><span data-stu-id="22abd-178">The validation error messages are displayed to the user:</span></span>

![Komunikatów o błędach są wyświetlane obok każdej kontrolki wprowadzania](uploading-files/_static/browser2.png)

<span data-ttu-id="22abd-180">Wpisz dwie litery w **tytuł** pola.</span><span class="sxs-lookup"><span data-stu-id="22abd-180">Type two letters into the **Title** field.</span></span> <span data-ttu-id="22abd-181">Sprawdzanie poprawności zmienia się na wskazują, czy tytuł musi należeć do zakresu od 3 do 60 znaków:</span><span class="sxs-lookup"><span data-stu-id="22abd-181">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Tytuł komunikatu weryfikacji zmienione](uploading-files/_static/browser3.png)

<span data-ttu-id="22abd-183">Po przekazaniu co najmniej jeden harmonogram **załadować harmonogramy** sekcji renderuje załadować harmonogramów:</span><span class="sxs-lookup"><span data-stu-id="22abd-183">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Daty UTC, rozmiar pliku publicznej wersji i rozmiar pliku w prywatnej wersji przekazać tabeli załadować harmonogramy, przedstawiający tytuł każdy z harmonogramów](uploading-files/_static/browser4.png)

<span data-ttu-id="22abd-185">Użytkownik może kliknąć **usunąć** łącza z tego miejsca do widoku potwierdzenie usunięcia, które mają możliwość potwierdzenia lub anulowania operacji usuwania.</span><span class="sxs-lookup"><span data-stu-id="22abd-185">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="22abd-186">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="22abd-186">Troubleshooting</span></span>

<span data-ttu-id="22abd-187">Aby uzyskać informacje o rozwiązywaniu problemów z `IFormFile` przekazywania, zobacz [przekazywania plików w ASP.NET Core: Rozwiązywanie problemów z](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="22abd-187">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="22abd-188">Dziękujemy za korzystanie z wprowadzenia do stron Razor.</span><span class="sxs-lookup"><span data-stu-id="22abd-188">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="22abd-189">Dziękujemy za wszelkie komentarze, które pozostaną.</span><span class="sxs-lookup"><span data-stu-id="22abd-189">We appreciate any comments you leave.</span></span> <span data-ttu-id="22abd-190">[Wprowadzenie do programu MVC i podstawowe EF](xref:data/ef-mvc/intro) jest doskonałym uzupełnianie w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="22abd-190">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="22abd-191">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="22abd-191">Additional resources</span></span>

* [<span data-ttu-id="22abd-192">W przypadku platformy ASP.NET Core przekazywania plików</span><span class="sxs-lookup"><span data-stu-id="22abd-192">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="22abd-193">IFormFile</span><span class="sxs-lookup"><span data-stu-id="22abd-193">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[<span data-ttu-id="22abd-194">Poprzednie: Sprawdzanie poprawności</span><span class="sxs-lookup"><span data-stu-id="22abd-194">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
