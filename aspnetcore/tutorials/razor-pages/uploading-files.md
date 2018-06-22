---
title: Przekazywanie plików do Razor strony platformy ASP.NET Core
author: guardrex
description: Dowiedz się, jak przekazać pliki do strony Razor.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/12/2017
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 43268e24b67279b57c990a6289922ae38d883221
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275960"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="3dfb0-103">Przekazywanie plików do Razor strony platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3dfb0-103">Upload files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="3dfb0-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3dfb0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3dfb0-105">W tej sekcji przedstawiono przekazywania plików ze stroną Razor.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-105">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="3dfb0-106">[Filmu stron Razor Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) w ten samouczek używa modelu prostego powiązania Aby przekazać pliki, które działa dobrze w przypadku przekazywania małych plików.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-106">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="3dfb0-107">Aby uzyskać informacje na przesyłanie strumieniowe dużych plików, zobacz [przekazywania dużych plików z przesyłania strumieniowego](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="3dfb0-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="3dfb0-108">W poniższych krokach funkcji przekazywania pliku harmonogramu film jest dodawany do przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="3dfb0-109">Harmonogram film jest reprezentowana przez `Schedule` klasy.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="3dfb0-110">Klasa zawiera dwie wersje harmonogramu.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="3dfb0-111">Jednej wersji są przekazywane klientom, `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="3dfb0-112">Druga wersja jest używana dla pracowników firmy `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="3dfb0-113">Każda wersja jest przekazany jako oddzielny plik.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="3dfb0-114">Samouczek pokazuje, jak wykonać dwa przekazywania plików ze strony z jednego wpisu na serwerze.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="3dfb0-115">Zagadnienia dotyczące bezpieczeństwa</span><span class="sxs-lookup"><span data-stu-id="3dfb0-115">Security considerations</span></span>

<span data-ttu-id="3dfb0-116">Należy zachować ostrożność podczas zapewniając użytkownikom możliwość przekazywania plików do serwera.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-116">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="3dfb0-117">Osoby atakujące mogą wykonywać ["odmowa usługi"](/windows-hardware/drivers/ifs/denial-of-service) i inne ataki w systemie.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-117">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="3dfb0-118">Niektóre kroki zabezpieczeń, które zmniejszyć prawdopodobieństwo udanego ataku są:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-118">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="3dfb0-119">Przekazywanie plików do obszaru przekazywania dedykowanych plików w systemie, co ułatwia nałożyć środków bezpieczeństwa w przekazanym zawartości.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-119">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="3dfb0-120">Podczas umożliwiający przekazywania plików, upewnij się, że uprawnienia do wykonywania są wyłączone w lokalizacji przekazywania.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-120">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="3dfb0-121">Użyj nazwy plików bezpieczne określane przez aplikację, a nie z danych wejściowych użytkownika lub nazwę pliku przekazanego pliku.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-121">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="3dfb0-122">Zezwalaj tylko na określonych rozszerzeń plików zatwierdzone.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-122">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="3dfb0-123">Sprawdź, czy po stronie klienta są sprawdzane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-123">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="3dfb0-124">Testy po stronie klienta są łatwe do obejścia.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-124">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="3dfb0-125">Sprawdź rozmiar przekazywania i uniemożliwić przekazywanie większych niż oczekiwano.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-125">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="3dfb0-126">Uruchom skanera przed wirusami i złośliwym oprogramowaniem w przekazanym zawartości.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-126">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="3dfb0-127">Przekazywanie złośliwego kodu do systemu jest często pierwszy krok w celu wykonywania kodu, który można:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="3dfb0-128">Całkowicie przejęcia pamięci.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-128">Completely takeover a system.</span></span>
> * <span data-ttu-id="3dfb0-129">Przeciążenia systemu, w wyniku czego system całkowicie zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-129">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="3dfb0-130">Naruszenia danych użytkownika lub systemu.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="3dfb0-131">Zastosowanie graffiti do interfejsu publicznego.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-131">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="3dfb0-132">Dodaj klasę przekazywaniem plików</span><span class="sxs-lookup"><span data-stu-id="3dfb0-132">Add a FileUpload class</span></span>

<span data-ttu-id="3dfb0-133">Utwórz stronę Razor do obsługi parę przekazywania plików.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-133">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="3dfb0-134">Dodaj `FileUpload` klasy, która jest powiązana ze stroną uzyskać dane harmonogramu.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-134">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="3dfb0-135">Kliknij prawym przyciskiem myszy *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-135">Right click the *Models* folder.</span></span> <span data-ttu-id="3dfb0-136">Wybierz **dodać** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-136">Select **Add** > **Class**.</span></span> <span data-ttu-id="3dfb0-137">Nazwa klasy **przekazywaniem plików** i dodaj następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-137">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="3dfb0-138">Klasa ma właściwość tytułu harmonogramu i właściwości dla każdego z dwóch wersji harmonogramu.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-138">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="3dfb0-139">Wszystkie trzy właściwości są wymagane, i tytuł musi wynosić 3 – 60 znaków.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-139">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="3dfb0-140">Dodaj metodę pomocnika, aby przekazać pliki</span><span class="sxs-lookup"><span data-stu-id="3dfb0-140">Add a helper method to upload files</span></span>

<span data-ttu-id="3dfb0-141">Aby uniknąć zduplikowania kodu do przetwarzania plików przekazane harmonogramu, najpierw Dodaj metodę pomocnika statycznych.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-141">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="3dfb0-142">Utwórz *narzędzia* folderu w aplikacji i Dodaj *FileHelpers.cs* pliku o następującej zawartości.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-142">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="3dfb0-143">Metoda pomocnika `ProcessFormFile`, przyjmuje [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) i [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) i zwraca ciąg zawierający rozmiar pliku i jego zawartości.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-143">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="3dfb0-144">Typ zawartości i długości są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-144">The content type and length are checked.</span></span> <span data-ttu-id="3dfb0-145">Jeśli plik nie przeszły sprawdzanie poprawności, błąd jest dodawany do `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-145">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

### <a name="save-the-file-to-disk"></a><span data-ttu-id="3dfb0-146">Zapisz plik na dysku</span><span class="sxs-lookup"><span data-stu-id="3dfb0-146">Save the file to disk</span></span>

<span data-ttu-id="3dfb0-147">Przykładowa aplikacja zapisuje pliki przekazana do pola bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-147">The sample app saves uploaded files into database fields.</span></span> <span data-ttu-id="3dfb0-148">Aby zapisać plik na dysku, należy użyć [FileStream](/dotnet/api/system.io.filestream).</span><span class="sxs-lookup"><span data-stu-id="3dfb0-148">To save a file to disk, use a [FileStream](/dotnet/api/system.io.filestream).</span></span> <span data-ttu-id="3dfb0-149">Poniższy przykładowy kod kopiuje plik posiadaniu `FileUpload.UploadPublicSchedule` do `FileStream` w `OnPostAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-149">The following example copies a file held by `FileUpload.UploadPublicSchedule` to a `FileStream` in an `OnPostAsync` method.</span></span> <span data-ttu-id="3dfb0-150">`FileStream` Zapisuje plik na dysku na `<PATH-AND-FILE-NAME>` podane:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-150">The `FileStream` writes the file to disk at the `<PATH-AND-FILE-NAME>` provided:</span></span>

```csharp
public async Task<IActionResult> OnPostAsync()
{
    // Perform an initial check to catch FileUpload class attribute violations.
    if (!ModelState.IsValid)
    {
        return Page();
    }

    var filePath = "<PATH-AND-FILE-NAME>";

    using (var fileStream = new FileStream(filePath, FileMode.Create))
    {
        await FileUpload.UploadPublicSchedule.CopyToAsync(fileStream);
    }

    return RedirectToPage("./Index");
}
```

<span data-ttu-id="3dfb0-151">Proces roboczy musi mieć uprawnienia do zapisu w lokalizacji określonej przez `filePath`.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-151">The worker process must have write permissions to the location specified by `filePath`.</span></span>

> [!NOTE]
> <span data-ttu-id="3dfb0-152">`filePath` *Musi* obejmują nazwę pliku.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-152">The `filePath` *must* include the file name.</span></span> <span data-ttu-id="3dfb0-153">Jeśli nazwa pliku nie został podany, [unauthorizedaccessexception —](/dotnet/api/system.unauthorizedaccessexception) jest zgłaszany w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-153">If the file name isn't provided, an [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) is thrown at runtime.</span></span>

> [!WARNING]
> <span data-ttu-id="3dfb0-154">Nigdy nie będą się powtarzać przekazane pliki w tym samym drzewie katalogu co aplikacja.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-154">Never persist uploaded files in the same directory tree as the app.</span></span>
>
> <span data-ttu-id="3dfb0-155">Przykładowy kod nie zapewnia po stronie serwera ochrony przed przekazywania złośliwych plików.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-155">The code sample provides no server-side protection against malicious file uploads.</span></span> <span data-ttu-id="3dfb0-156">Aby uzyskać informacje na zmniejszenie powierzchni ataku, akceptując pliki od użytkowników zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-156">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="3dfb0-157">Przekazywanie pliku bez ograniczeń</span><span class="sxs-lookup"><span data-stu-id="3dfb0-157">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="3dfb0-158">Zabezpieczeń platformy Azure: Upewnij się, że odpowiednie formanty w miejscu akceptując pliki użytkowników</span><span class="sxs-lookup"><span data-stu-id="3dfb0-158">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="3dfb0-159">Zapisz plik do magazynu obiektów Blob Azure</span><span class="sxs-lookup"><span data-stu-id="3dfb0-159">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="3dfb0-160">Aby przekazać zawartość pliku do magazynu obiektów Blob Azure, zobacz [Rozpoczynanie pracy z magazynem obiektów Blob Azure przy użyciu platformy .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="3dfb0-160">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="3dfb0-161">Temacie przedstawiono sposób użycia [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) zapisać [FileStream](/dotnet/api/system.io.filestream) do magazynu obiektów blob.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-161">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="3dfb0-162">Dodaj klasę harmonogramu</span><span class="sxs-lookup"><span data-stu-id="3dfb0-162">Add the Schedule class</span></span>

<span data-ttu-id="3dfb0-163">Kliknij prawym przyciskiem myszy *modele* folderu.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-163">Right click the *Models* folder.</span></span> <span data-ttu-id="3dfb0-164">Wybierz **dodać** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-164">Select **Add** > **Class**.</span></span> <span data-ttu-id="3dfb0-165">Nazwa klasy **harmonogram** i dodaj następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-165">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="3dfb0-166">Używa klasy `Display` i `DisplayFormat` atrybuty, które przyjazną tytułów i formatowania podczas renderowania danych harmonogramu.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-166">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="3dfb0-167">Aktualizacja MovieContext</span><span class="sxs-lookup"><span data-stu-id="3dfb0-167">Update the MovieContext</span></span>

<span data-ttu-id="3dfb0-168">Określ `DbSet` w `MovieContext` (*Models/MovieContext.cs*) dla harmonogramów:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-168">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="3dfb0-169">Dodaj tabelę harmonogramu do bazy danych</span><span class="sxs-lookup"><span data-stu-id="3dfb0-169">Add the Schedule table to the database</span></span>

<span data-ttu-id="3dfb0-170">Otwórz konsolę Menedżera pakietów (PMC): **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-170">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![PMC menu](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="3dfb0-172">W kryterium wykonaj następujące polecenia.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-172">In the PMC, execute the following commands.</span></span> <span data-ttu-id="3dfb0-173">Te polecenia powodują dodanie `Schedule` tabeli w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-173">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="3dfb0-174">Dodaj stronę Razor przekazywania pliku</span><span class="sxs-lookup"><span data-stu-id="3dfb0-174">Add a file upload Razor Page</span></span>

<span data-ttu-id="3dfb0-175">W *stron* folderu, Utwórz *harmonogramy* folderu.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-175">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="3dfb0-176">W *harmonogramy* folderu, Utwórz stronę o nazwie *Index.cshtml* o przekazywaniu harmonogram o następującej treści:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-176">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="3dfb0-177">Każda grupa formularz zawiera  **\<Etykieta >** który wyświetla nazwę każda właściwość klasy.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-177">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="3dfb0-178">`Display` Atrybutów w `FileUpload` modelu Podaj wartości wyświetlania etykiet.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-178">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="3dfb0-179">Na przykład `UploadPublicSchedule` nazwy wyświetlanej właściwości ustawiono `[Display(Name="Public Schedule")]` i w związku z tym Wyświetla "Harmonogram publiczny" w etykiecie podczas renderowania formularza.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-179">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="3dfb0-180">Każda grupa formularz zawiera weryfikacji  **\<span >**.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-180">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="3dfb0-181">Jeśli użytkownik wejściowych nie spełniają atrybuty właściwości ustawione `FileUpload` klasy lub jeśli któryś z `ProcessFormFile` sprawdzanie poprawności pliku metody kończyć się niepowodzeniem, model kończy się niepowodzeniem do sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-181">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="3dfb0-182">Podczas sprawdzania poprawności modelu nie powiedzie się, komunikat dotyczący sprawdzania poprawności pomocne jest renderowany do użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-182">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="3dfb0-183">Na przykład `Title` właściwość jest oznaczona przy `[Required]` i `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-183">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="3dfb0-184">Jeśli użytkownik nie może podać tytuł, otrzymają komunikat informujący, że wymagana jest wartość.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-184">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="3dfb0-185">Jeśli użytkownik wprowadzi wartość mniej niż 3 znaków ani więcej niż 60 znaków, otrzymają komunikat informujący, że wartość ma nieprawidłową długość.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-185">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="3dfb0-186">Jeśli plik jest pod warunkiem, że nie ma zawartości, zostanie wyświetlony komunikat, że plik jest pusty.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-186">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="3dfb0-187">Dodawanie modelu strony</span><span class="sxs-lookup"><span data-stu-id="3dfb0-187">Add the page model</span></span>

<span data-ttu-id="3dfb0-188">Dodawanie modelu strony (*Index.cshtml.cs*) do *harmonogramy* folderu:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-188">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="3dfb0-189">Model strony (`IndexModel` w *Index.cshtml.cs*) wiąże `FileUpload` klasy:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-189">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="3dfb0-190">Model korzysta również z listą harmonogramy (`IList<Schedule>`) do wyświetlenia przechowywanych w bazie danych na stronie harmonogramów:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-190">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="3dfb0-191">Załadowanie strony z `OnGetAsync`, `Schedules` jest wypełnione z bazy danych i używane do generowania tabeli HTML załadować harmonogramów:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-191">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="3dfb0-192">Gdy formularz jest przesyłana do serwera, `ModelState` jest zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-192">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="3dfb0-193">Jeśli jest to nieprawidłowa `Schedule` odbudowaniu i renderuje stronę z jednego lub więcej komunikatów dotyczących sprawdzania poprawności, podając, dlaczego nie można sprawdzić poprawności strony.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-193">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="3dfb0-194">Jeśli jest prawidłowa, `FileUpload` właściwości są używane w *OnPostAsync* aby zakończyć przekazywanie plików dla obu wersji harmonogramu i utworzyć nową `Schedule` obiektu do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-194">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="3dfb0-195">Harmonogram jest następnie zapisywana w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-195">The schedule is then saved to the database:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="3dfb0-196">Link przekazywania pliku Razor strony</span><span class="sxs-lookup"><span data-stu-id="3dfb0-196">Link the file upload Razor Page</span></span>

<span data-ttu-id="3dfb0-197">Otwórz *_Layout.cshtml* i dodać łącze do paska nawigacyjnego, aby przejść do strony przekazywania plików:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-197">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="3dfb0-198">Dodaj stronę, aby potwierdzić usunięcie harmonogramu</span><span class="sxs-lookup"><span data-stu-id="3dfb0-198">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="3dfb0-199">Gdy użytkownik kliknie przycisk, aby usunąć harmonogram, znajduje się możliwość anulowania operacji.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-199">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="3dfb0-200">Dodaj stronę potwierdzenia usunięcia (*Delete.cshtml*) do *harmonogramy* folderu:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-200">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="3dfb0-201">Model strony (*Delete.cshtml.cs*) ładuje jeden harmonogram identyfikowane przez `id` w danych trasy żądania.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-201">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="3dfb0-202">Dodaj *Delete.cshtml.cs* pliku *harmonogramy* folderu:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-202">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="3dfb0-203">`OnPostAsync` Obsługuje metoda usuwania harmonogramu przez jego `id`:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-203">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="3dfb0-204">Po pomyślnym usunięciu harmonogram, `RedirectToPage` wysyła użytkownika z powrotem do harmonogramów *Index.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-204">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="3dfb0-205">Pracy harmonogramy Razor strony</span><span class="sxs-lookup"><span data-stu-id="3dfb0-205">The working Schedules Razor Page</span></span>

<span data-ttu-id="3dfb0-206">Podczas ładowania strony, etykiety i dane wejściowe dla tytułu harmonogram harmonogram publicznego i prywatnego harmonogramu są renderowane z przycisk przesyłania:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-206">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Planuje Razor strony, jak pokazano na ładowania początkowego bez błędów weryfikacji i puste pola](uploading-files/_static/browser1.png)

<span data-ttu-id="3dfb0-208">Wybieranie **przekazać** przycisk bez wypełniania pól narusza `[Required]` atrybutów w modelu.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-208">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="3dfb0-209">`ModelState` Jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-209">The `ModelState` is invalid.</span></span> <span data-ttu-id="3dfb0-210">Komunikatów o błędach są wyświetlane dla użytkownika:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-210">The validation error messages are displayed to the user:</span></span>

![Komunikatów o błędach są wyświetlane obok każdej kontrolki wprowadzania](uploading-files/_static/browser2.png)

<span data-ttu-id="3dfb0-212">Wpisz dwie litery w **tytuł** pola.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-212">Type two letters into the **Title** field.</span></span> <span data-ttu-id="3dfb0-213">Sprawdzanie poprawności zmienia się na wskazują, czy tytuł musi należeć do zakresu od 3 do 60 znaków:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-213">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Tytuł komunikatu weryfikacji zmienione](uploading-files/_static/browser3.png)

<span data-ttu-id="3dfb0-215">Po przekazaniu co najmniej jeden harmonogram **załadować harmonogramy** sekcji renderuje załadować harmonogramów:</span><span class="sxs-lookup"><span data-stu-id="3dfb0-215">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Daty UTC, rozmiar pliku publicznej wersji i rozmiar pliku w prywatnej wersji przekazać tabeli załadować harmonogramy, przedstawiający tytuł każdy z harmonogramów](uploading-files/_static/browser4.png)

<span data-ttu-id="3dfb0-217">Użytkownik może kliknąć **usunąć** łącza z tego miejsca do widoku potwierdzenie usunięcia, które mają możliwość potwierdzenia lub anulowania operacji usuwania.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-217">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="3dfb0-218">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="3dfb0-218">Troubleshooting</span></span>

<span data-ttu-id="3dfb0-219">Aby uzyskać informacje o rozwiązywaniu problemów z `IFormFile` przekazywania, zobacz [przekazywania plików w ASP.NET Core: Rozwiązywanie problemów z](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="3dfb0-219">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="3dfb0-220">Dziękujemy za korzystanie z wprowadzenia do stron Razor.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-220">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="3dfb0-221">Dziękujemy za opinię.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-221">We appreciate feedback.</span></span> <span data-ttu-id="3dfb0-222">[Rozpoczynanie pracy z MVC i podstawowe EF](xref:data/ef-mvc/intro) jest doskonałym uzupełnianie w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="3dfb0-222">[Get started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3dfb0-223">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3dfb0-223">Additional resources</span></span>

* [<span data-ttu-id="3dfb0-224">W przypadku platformy ASP.NET Core przekazywania plików</span><span class="sxs-lookup"><span data-stu-id="3dfb0-224">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="3dfb0-225">IFormFile</span><span class="sxs-lookup"><span data-stu-id="3dfb0-225">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

> [!div class="step-by-step"]
> [<span data-ttu-id="3dfb0-226">Poprzednie: Sprawdzanie poprawności</span><span class="sxs-lookup"><span data-stu-id="3dfb0-226">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
