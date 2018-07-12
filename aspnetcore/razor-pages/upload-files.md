---
title: Przekazywanie plików na stronę Razor programu ASP.NET Core
author: guardrex
description: Dowiedz się, jak przekazać pliki do strony Razor.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/11/2018
uid: razor-pages/upload-files
ms.openlocfilehash: 4b2f80cd5644cf21d5d8452aff6df4eb5591d41b
ms.sourcegitcommit: 19cbda409bdbbe42553dc385ea72d2a8e246509c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38993499"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="3f72e-103">Przekazywanie plików na stronę Razor programu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f72e-103">Upload files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="3f72e-104">Przez [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3f72e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="3f72e-105">Ten temat opiera się na [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) w <xref:tutorials/razor-pages/razor-pages-start>.</span><span class="sxs-lookup"><span data-stu-id="3f72e-105">This topic builds upon the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) in <xref:tutorials/razor-pages/razor-pages-start>.</span></span>

<span data-ttu-id="3f72e-106">W tym temacie pokazano, jak przekazać pliki, za pomocą prostego modelu powiązania, co sprawdza się dobrze w przypadku przekazywania plików na małe.</span><span class="sxs-lookup"><span data-stu-id="3f72e-106">This topic shows how to use simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="3f72e-107">Aby uzyskać informacje na przesyłanie strumieniowe dużych plików, zobacz [przekazywanie dużych plików z przesyłaniem strumieniowym](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="3f72e-107">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="3f72e-108">W poniższych krokach funkcji przekazywania plików harmonogram film zostanie dodany do przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3f72e-108">In the following steps, a movie schedule file upload feature is added to the sample app.</span></span> <span data-ttu-id="3f72e-109">Harmonogram film jest reprezentowany przez `Schedule` klasy.</span><span class="sxs-lookup"><span data-stu-id="3f72e-109">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="3f72e-110">Klasa zawiera dwie wersje harmonogramu.</span><span class="sxs-lookup"><span data-stu-id="3f72e-110">The class includes two versions of the schedule.</span></span> <span data-ttu-id="3f72e-111">Jedna wersja są przekazywane klientom, `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="3f72e-111">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="3f72e-112">Druga wersja jest używana w przypadku pracowników firmy `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="3f72e-112">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="3f72e-113">Każda wersja jest przekazywany jako oddzielny plik.</span><span class="sxs-lookup"><span data-stu-id="3f72e-113">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="3f72e-114">Samouczek pokazuje, jak wykonać dwie operacje przekazywania plików ze strony za pomocą pojedynczego wpisu do serwera.</span><span class="sxs-lookup"><span data-stu-id="3f72e-114">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="security-considerations"></a><span data-ttu-id="3f72e-115">Zagadnienia dotyczące bezpieczeństwa</span><span class="sxs-lookup"><span data-stu-id="3f72e-115">Security considerations</span></span>

<span data-ttu-id="3f72e-116">Należy zachować ostrożność przy zapewnieniu użytkownikom możliwość przekazywania plików na serwerze.</span><span class="sxs-lookup"><span data-stu-id="3f72e-116">Caution must be taken when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="3f72e-117">Może być wykonywane przez osoby atakujące ["odmowa usługi"](/windows-hardware/drivers/ifs/denial-of-service) i inne ataki na system.</span><span class="sxs-lookup"><span data-stu-id="3f72e-117">Attackers may execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) and other attacks on a system.</span></span> <span data-ttu-id="3f72e-118">Niektóre czynności zabezpieczeń, które zmniejszają prawdopodobieństwo udanego ataku są następujące:</span><span class="sxs-lookup"><span data-stu-id="3f72e-118">Some security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="3f72e-119">Przekazywanie plików do obszaru przekazywania pliku dedykowanych w systemie, dzięki czemu łatwiej można nałożyć środków bezpieczeństwa w systemach przekazana zawartość.</span><span class="sxs-lookup"><span data-stu-id="3f72e-119">Upload files to a dedicated file upload area on the system, which makes it easier to impose security measures on uploaded content.</span></span> <span data-ttu-id="3f72e-120">Podczas pozwalające przekazywania plików, upewnij się, że uprawnienia do wykonywania są wyłączone w lokalizacji przekazywania.</span><span class="sxs-lookup"><span data-stu-id="3f72e-120">When permitting file uploads, make sure that execute permissions are disabled on the upload location.</span></span>
* <span data-ttu-id="3f72e-121">Użyj nazwy plików bezpieczne określane przez aplikację, a nie z danych wejściowych użytkownika lub nazwę pliku przekazanego pliku.</span><span class="sxs-lookup"><span data-stu-id="3f72e-121">Use a safe file name determined by the app, not from user input or the file name of the uploaded file.</span></span>
* <span data-ttu-id="3f72e-122">Zezwalaj na tylko określony zestaw rozszerzeń plików zatwierdzone.</span><span class="sxs-lookup"><span data-stu-id="3f72e-122">Only allow a specific set of approved file extensions.</span></span>
* <span data-ttu-id="3f72e-123">Sprawdź, czy po stronie klienta są sprawdzane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="3f72e-123">Verify client-side checks are performed on the server.</span></span> <span data-ttu-id="3f72e-124">Sprawdzanie klienta są łatwe do obejścia.</span><span class="sxs-lookup"><span data-stu-id="3f72e-124">Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="3f72e-125">Sprawdź rozmiar przekazywania i uniemożliwić przekazywanie większych niż oczekiwano.</span><span class="sxs-lookup"><span data-stu-id="3f72e-125">Check the size of the upload and prevent larger uploads than expected.</span></span>
* <span data-ttu-id="3f72e-126">Uruchomić skanera przed wirusami i złośliwym oprogramowaniem na przekazana zawartość.</span><span class="sxs-lookup"><span data-stu-id="3f72e-126">Run a virus/malware scanner on uploaded content.</span></span>

> [!WARNING]
> <span data-ttu-id="3f72e-127">Przekazywanie złośliwego kodu do systemu często jest pierwszym krokiem do wykonywania kodu, która może być:</span><span class="sxs-lookup"><span data-stu-id="3f72e-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
> * <span data-ttu-id="3f72e-128">Całkowicie przejęcia w systemie.</span><span class="sxs-lookup"><span data-stu-id="3f72e-128">Completely takeover a system.</span></span>
> * <span data-ttu-id="3f72e-129">Przeciążenia systemu, w wyniku czego system całkowicie nie powiedzie się.</span><span class="sxs-lookup"><span data-stu-id="3f72e-129">Overload a system with the result that the system completely fails.</span></span>
> * <span data-ttu-id="3f72e-130">Naruszyć bezpieczeństwo danych użytkownika lub systemu.</span><span class="sxs-lookup"><span data-stu-id="3f72e-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="3f72e-131">Zastosowanie graffiti do interfejsu publicznego.</span><span class="sxs-lookup"><span data-stu-id="3f72e-131">Apply graffiti to a public interface.</span></span>

## <a name="add-a-fileupload-class"></a><span data-ttu-id="3f72e-132">Dodaj klasę FileUpload</span><span class="sxs-lookup"><span data-stu-id="3f72e-132">Add a FileUpload class</span></span>

<span data-ttu-id="3f72e-133">Utwórz stronę Razor do obsługi parę przekazywania plików.</span><span class="sxs-lookup"><span data-stu-id="3f72e-133">Create a Razor Page to handle a pair of file uploads.</span></span> <span data-ttu-id="3f72e-134">Dodaj `FileUpload` klasy, który jest powiązany do strony, aby uzyskać dane tabeli schedule.</span><span class="sxs-lookup"><span data-stu-id="3f72e-134">Add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="3f72e-135">Kliknij prawym przyciskiem myszy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="3f72e-135">Right click the *Models* folder.</span></span> <span data-ttu-id="3f72e-136">Wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="3f72e-136">Select **Add** > **Class**.</span></span> <span data-ttu-id="3f72e-137">Nazwa klasy **FileUpload** i dodaj następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="3f72e-137">Name the class **FileUpload** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

<span data-ttu-id="3f72e-138">Klasa ma właściwość Tytuł harmonogram i właściwości dla każdego z dwóch wersji harmonogramu.</span><span class="sxs-lookup"><span data-stu-id="3f72e-138">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="3f72e-139">Wymagane są wszystkie trzy właściwości, a tytuł musi zawierać 3-60 znaków.</span><span class="sxs-lookup"><span data-stu-id="3f72e-139">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="3f72e-140">Dodaj metodę pomocnika do przekazywania plików</span><span class="sxs-lookup"><span data-stu-id="3f72e-140">Add a helper method to upload files</span></span>

<span data-ttu-id="3f72e-141">Aby uniknąć zduplikowania kodu do przetwarzania plików przekazanych harmonogram, najpierw Dodaj metodę pomocnika statyczne.</span><span class="sxs-lookup"><span data-stu-id="3f72e-141">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="3f72e-142">Tworzenie *narzędzia* folderu w aplikacji i Dodaj *FileHelpers.cs* pliku o następującej zawartości.</span><span class="sxs-lookup"><span data-stu-id="3f72e-142">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="3f72e-143">Metoda pomocnika `ProcessFormFile`, przyjmuje [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) i [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) i zwraca ciąg zawierający rozmiar i zawartości pliku.</span><span class="sxs-lookup"><span data-stu-id="3f72e-143">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="3f72e-144">Typ zawartości i długość są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="3f72e-144">The content type and length are checked.</span></span> <span data-ttu-id="3f72e-145">Jeśli plik nie przeszły sprawdzanie poprawności, błąd jest dodawany do `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="3f72e-145">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

### <a name="save-the-file-to-disk"></a><span data-ttu-id="3f72e-146">Zapisz plik na dysku</span><span class="sxs-lookup"><span data-stu-id="3f72e-146">Save the file to disk</span></span>

<span data-ttu-id="3f72e-147">Przykładowa aplikacja zapisuje przekazywanych plików w polach bazy danych.</span><span class="sxs-lookup"><span data-stu-id="3f72e-147">The sample app saves uploaded files into database fields.</span></span> <span data-ttu-id="3f72e-148">Aby zapisać plik na dysku, należy użyć [FileStream](/dotnet/api/system.io.filestream).</span><span class="sxs-lookup"><span data-stu-id="3f72e-148">To save a file to disk, use a [FileStream](/dotnet/api/system.io.filestream).</span></span> <span data-ttu-id="3f72e-149">Poniższy przykład służy do kopiowania pliku w posiadaniu `FileUpload.UploadPublicSchedule` do `FileStream` w `OnPostAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="3f72e-149">The following example copies a file held by `FileUpload.UploadPublicSchedule` to a `FileStream` in an `OnPostAsync` method.</span></span> <span data-ttu-id="3f72e-150">`FileStream` Zapisuje plik na dysku w `<PATH-AND-FILE-NAME>` podane:</span><span class="sxs-lookup"><span data-stu-id="3f72e-150">The `FileStream` writes the file to disk at the `<PATH-AND-FILE-NAME>` provided:</span></span>

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

<span data-ttu-id="3f72e-151">Proces roboczy musi mieć uprawnienia do zapisu w lokalizacji określonej przez `filePath`.</span><span class="sxs-lookup"><span data-stu-id="3f72e-151">The worker process must have write permissions to the location specified by `filePath`.</span></span>

> [!NOTE]
> <span data-ttu-id="3f72e-152">`filePath` *Musi* można umieszczać nazwy pliku.</span><span class="sxs-lookup"><span data-stu-id="3f72e-152">The `filePath` *must* include the file name.</span></span> <span data-ttu-id="3f72e-153">Jeśli nie podasz nazwy pliku, [unauthorizedaccessexception —](/dotnet/api/system.unauthorizedaccessexception) jest generowany w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="3f72e-153">If the file name isn't provided, an [UnauthorizedAccessException](/dotnet/api/system.unauthorizedaccessexception) is thrown at runtime.</span></span>

> [!WARNING]
> <span data-ttu-id="3f72e-154">Nigdy nie są zachowywane przekazywanych plików w tym samym drzewie katalogu, co aplikacja.</span><span class="sxs-lookup"><span data-stu-id="3f72e-154">Never persist uploaded files in the same directory tree as the app.</span></span>
>
> <span data-ttu-id="3f72e-155">Przykładowy kod zawiera nie ochrony po stronie serwera przed przekazywania złośliwych plików.</span><span class="sxs-lookup"><span data-stu-id="3f72e-155">The code sample provides no server-side protection against malicious file uploads.</span></span> <span data-ttu-id="3f72e-156">Instrukcje dotyczące zmniejszenie obszaru powierzchni ataku, akceptując pliki użytkowników zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="3f72e-156">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="3f72e-157">Przekazywanie pliku bez ograniczeń</span><span class="sxs-lookup"><span data-stu-id="3f72e-157">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="3f72e-158">Zabezpieczenia platformy Azure: Upewnij się, że odpowiednie formanty są stosowane podczas akceptowania plików od użytkowników</span><span class="sxs-lookup"><span data-stu-id="3f72e-158">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a><span data-ttu-id="3f72e-159">Zapisz plik w usłudze Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="3f72e-159">Save the file to Azure Blob Storage</span></span>

<span data-ttu-id="3f72e-160">Aby przekazać zawartość pliku do usługi Azure Blob Storage, zobacz [Rozpoczynanie pracy z usługą Azure Blob Storage przy użyciu platformy .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="3f72e-160">To upload file content to Azure Blob Storage, see [Get started with Azure Blob Storage using .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs).</span></span> <span data-ttu-id="3f72e-161">Temat demonstruje sposób skorzystania [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) można zapisać [FileStream](/dotnet/api/system.io.filestream) do magazynu obiektów blob.</span><span class="sxs-lookup"><span data-stu-id="3f72e-161">The topic demonstrates how to use [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) to save a [FileStream](/dotnet/api/system.io.filestream) to blob storage.</span></span>

## <a name="add-the-schedule-class"></a><span data-ttu-id="3f72e-162">Dodaj klasę harmonogramu</span><span class="sxs-lookup"><span data-stu-id="3f72e-162">Add the Schedule class</span></span>

<span data-ttu-id="3f72e-163">Kliknij prawym przyciskiem myszy *modeli* folderu.</span><span class="sxs-lookup"><span data-stu-id="3f72e-163">Right click the *Models* folder.</span></span> <span data-ttu-id="3f72e-164">Wybierz **Dodaj** > **klasy**.</span><span class="sxs-lookup"><span data-stu-id="3f72e-164">Select **Add** > **Class**.</span></span> <span data-ttu-id="3f72e-165">Nazwa klasy **harmonogram** i dodaj następujące właściwości:</span><span class="sxs-lookup"><span data-stu-id="3f72e-165">Name the class **Schedule** and add the following properties:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

<span data-ttu-id="3f72e-166">Ta klasa używa `Display` i `DisplayFormat` atrybuty, które generuje przyjazna tytułów i formatowania podczas renderowania dane tabeli schedule.</span><span class="sxs-lookup"><span data-stu-id="3f72e-166">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="update-the-razorpagesmoviecontext"></a><span data-ttu-id="3f72e-167">Aktualizacja RazorPagesMovieContext</span><span class="sxs-lookup"><span data-stu-id="3f72e-167">Update the RazorPagesMovieContext</span></span>

<span data-ttu-id="3f72e-168">Określ `DbSet` w `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) dla harmonogramów:</span><span class="sxs-lookup"><span data-stu-id="3f72e-168">Specify a `DbSet` in the `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Data/RazorPagesMovieContext.cs?highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="update-the-moviecontext"></a><span data-ttu-id="3f72e-169">Aktualizacja MovieContext</span><span class="sxs-lookup"><span data-stu-id="3f72e-169">Update the MovieContext</span></span>

<span data-ttu-id="3f72e-170">Określ `DbSet` w `MovieContext` (*Models/MovieContext.cs*) dla harmonogramów:</span><span class="sxs-lookup"><span data-stu-id="3f72e-170">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

::: moniker-end

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="3f72e-171">Dodaj tabelę harmonogram do bazy danych</span><span class="sxs-lookup"><span data-stu-id="3f72e-171">Add the Schedule table to the database</span></span>

<span data-ttu-id="3f72e-172">Otwórz konsolę Menedżera pakietów (PMC): **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="3f72e-172">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![Menu konsoli zarządzania Pakietami](upload-files/_static/pmc.png)

<span data-ttu-id="3f72e-174">W konsoli zarządzania Pakietami wykonaj następujące polecenia.</span><span class="sxs-lookup"><span data-stu-id="3f72e-174">In the PMC, execute the following commands.</span></span> <span data-ttu-id="3f72e-175">Te polecenia Dodaj `Schedule` tabeli w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="3f72e-175">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="3f72e-176">Dodaj stronę Razor przekazywania pliku</span><span class="sxs-lookup"><span data-stu-id="3f72e-176">Add a file upload Razor Page</span></span>

<span data-ttu-id="3f72e-177">W *stron* folderze utwórz *harmonogramy* folderu.</span><span class="sxs-lookup"><span data-stu-id="3f72e-177">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="3f72e-178">W *harmonogramy* folderu, Utwórz stronę o nazwie *Index.cshtml* przekazywania harmonogram o następującej zawartości:</span><span class="sxs-lookup"><span data-stu-id="3f72e-178">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

<span data-ttu-id="3f72e-179">Każda grupa formularz zawiera  **\<Etykieta >** wyświetlającą nazwa każdej właściwości klasy.</span><span class="sxs-lookup"><span data-stu-id="3f72e-179">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="3f72e-180">`Display` Atrybutów w `FileUpload` modelu podanie wartości wyświetlania etykiet.</span><span class="sxs-lookup"><span data-stu-id="3f72e-180">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="3f72e-181">Na przykład `UploadPublicSchedule` nazwy wyświetlania właściwości została ustawiona za pomocą `[Display(Name="Public Schedule")]` i dlatego wyświetla "Harmonogram publiczny" w etykiecie, gdy powoduje wyświetlenie formularza.</span><span class="sxs-lookup"><span data-stu-id="3f72e-181">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="3f72e-182">Każda grupa formularz zawiera weryfikacji  **\<span >**.</span><span class="sxs-lookup"><span data-stu-id="3f72e-182">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="3f72e-183">Jeśli użytkownik wejściowych nie spełniają atrybuty właściwości ustawione w `FileUpload` klasy lub jeśli któryś z `ProcessFormFile` sprawdzanie poprawności pliku metoda kończyć się niepowodzeniem, modelu nie powiedzie się sprawdzić poprawność.</span><span class="sxs-lookup"><span data-stu-id="3f72e-183">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="3f72e-184">Podczas sprawdzania poprawności modelu nie powiedzie się, komunikat dotyczący sprawdzania poprawności pomocne jest renderowany do użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3f72e-184">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="3f72e-185">Na przykład `Title` właściwość jest oznaczona za pomocą `[Required]` i `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="3f72e-185">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="3f72e-186">Jeśli użytkownik nie może wpisać tytuł, otrzyma komunikat informujący, że wartość jest wymagana.</span><span class="sxs-lookup"><span data-stu-id="3f72e-186">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="3f72e-187">Jeśli użytkownik wprowadzi wartości, mniej niż trzy znaki lub więcej niż 60 znaków, otrzyma komunikat informujący, że wartość ma niepoprawną długość.</span><span class="sxs-lookup"><span data-stu-id="3f72e-187">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="3f72e-188">Jeśli plik jest pod warunkiem, że nie ma zawartości, zostanie wyświetlony komunikat, że plik jest pusty.</span><span class="sxs-lookup"><span data-stu-id="3f72e-188">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-page-model"></a><span data-ttu-id="3f72e-189">Dodawanie modelu strony</span><span class="sxs-lookup"><span data-stu-id="3f72e-189">Add the page model</span></span>

<span data-ttu-id="3f72e-190">Dodawanie modelu strony (*Index.cshtml.cs*) do *harmonogramy* folderu:</span><span class="sxs-lookup"><span data-stu-id="3f72e-190">Add the page model (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

<span data-ttu-id="3f72e-191">Model strony (`IndexModel` w *Index.cshtml.cs*) wiąże `FileUpload` klasy:</span><span class="sxs-lookup"><span data-stu-id="3f72e-191">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="3f72e-192">Model wykorzystuje także listę harmonogramy (`IList<Schedule>`) aby wyświetlić harmonogramy przechowywanych w bazie danych na stronie:</span><span class="sxs-lookup"><span data-stu-id="3f72e-192">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

<span data-ttu-id="3f72e-193">Jeśli strona ładuje się za pomocą `OnGetAsync`, `Schedules` jest wypełniana z bazy danych i używany do generowania tabeli HTML załadować harmonogramów:</span><span class="sxs-lookup"><span data-stu-id="3f72e-193">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="3f72e-194">Po opublikowaniu formularza z serwerem `ModelState` jest zaznaczone.</span><span class="sxs-lookup"><span data-stu-id="3f72e-194">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="3f72e-195">Jeśli jest to nieprawidłowa `Schedule` zostanie ponownie skompilowany, i strony, renderowanie przy użyciu jednego lub więcej komunikatów dotyczących sprawdzania poprawności informacją, dlaczego strony Weryfikacja nie powiodła się.</span><span class="sxs-lookup"><span data-stu-id="3f72e-195">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="3f72e-196">Jeśli są one prawidłowe, `FileUpload` właściwości są używane w *OnPostAsync* aby zakończyć przekazywanie pliku dla obu wersji harmonogram i Utwórz nową `Schedule` obiektu do przechowywania danych.</span><span class="sxs-lookup"><span data-stu-id="3f72e-196">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="3f72e-197">Harmonogram jest następnie zapisywany w bazie danych:</span><span class="sxs-lookup"><span data-stu-id="3f72e-197">The schedule is then saved to the database:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="3f72e-198">Link przekazywania pliku strony Razor</span><span class="sxs-lookup"><span data-stu-id="3f72e-198">Link the file upload Razor Page</span></span>

<span data-ttu-id="3f72e-199">Otwórz *Pages/Shared/_Layout.cshtml* i dodać link na pasku nawigacyjnym, aby dotrzeć do strony harmonogramów:</span><span class="sxs-lookup"><span data-stu-id="3f72e-199">Open *Pages/Shared/_Layout.cshtml* and add a link to the navigation bar to reach the Schedules page:</span></span>

```cshtml
<div class="navbar-collapse collapse">
    <ul class="nav navbar-nav">
        <li><a asp-page="/Index">Home</a></li>
        <li><a asp-page="/Schedules/Index">Schedules</a></li>
        <li><a asp-page="/About">About</a></li>
        <li><a asp-page="/Contact">Contact</a></li>
    </ul>
</div>
```

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="3f72e-200">Dodaj stronę, aby potwierdzić usunięcie harmonogramu</span><span class="sxs-lookup"><span data-stu-id="3f72e-200">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="3f72e-201">Gdy użytkownik kliknie przycisk, aby usunąć harmonogram, znajduje się szansę, aby anulować operację.</span><span class="sxs-lookup"><span data-stu-id="3f72e-201">When the user clicks to delete a schedule, a chance to cancel the operation is provided.</span></span> <span data-ttu-id="3f72e-202">Dodaj stronę potwierdzenia usunięcia (*Delete.cshtml*) do *harmonogramy* folderu:</span><span class="sxs-lookup"><span data-stu-id="3f72e-202">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

<span data-ttu-id="3f72e-203">Model strony (*Delete.cshtml.cs*) ładuje identyfikowane za pomocą jednego harmonogramu `id` w danych trasy żądania.</span><span class="sxs-lookup"><span data-stu-id="3f72e-203">The page model (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="3f72e-204">Dodaj *Delete.cshtml.cs* plik *harmonogramy* folderu:</span><span class="sxs-lookup"><span data-stu-id="3f72e-204">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

<span data-ttu-id="3f72e-205">`OnPostAsync` Obsługiwała usuwanie harmonogram, według jego `id`:</span><span class="sxs-lookup"><span data-stu-id="3f72e-205">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

<span data-ttu-id="3f72e-206">Po pomyślnym usunięciu harmonogramu, `RedirectToPage` wysyła użytkownika harmonogramy *Index.cshtml* strony.</span><span class="sxs-lookup"><span data-stu-id="3f72e-206">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="3f72e-207">Strona Razor harmonogramy dni roboczych</span><span class="sxs-lookup"><span data-stu-id="3f72e-207">The working Schedules Razor Page</span></span>

<span data-ttu-id="3f72e-208">Po załadowaniu strony, etykiety i dane wejściowe dla tytułu harmonogramu, harmonogram publicznych i prywatnych harmonogramu są renderowane przy użyciu przycisku Prześlij:</span><span class="sxs-lookup"><span data-stu-id="3f72e-208">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Planuje strony Razor, jak pokazano na ładowania początkowego bez błędów sprawdzania poprawności i puste pola](upload-files/_static/browser1.png)

<span data-ttu-id="3f72e-210">Wybieranie **przekazywanie** przycisk bez żadnego pola wypełnianie narusza `[Required]` atrybutów w modelu.</span><span class="sxs-lookup"><span data-stu-id="3f72e-210">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="3f72e-211">`ModelState` Jest nieprawidłowy.</span><span class="sxs-lookup"><span data-stu-id="3f72e-211">The `ModelState` is invalid.</span></span> <span data-ttu-id="3f72e-212">Komunikaty o błędach weryfikacji są wyświetlane dla użytkownika:</span><span class="sxs-lookup"><span data-stu-id="3f72e-212">The validation error messages are displayed to the user:</span></span>

![Komunikaty o błędach weryfikacji pojawiają się obok każdej kontrolki wprowadzania](upload-files/_static/browser2.png)

<span data-ttu-id="3f72e-214">Wpisz dwie litery w **tytuł** pola.</span><span class="sxs-lookup"><span data-stu-id="3f72e-214">Type two letters into the **Title** field.</span></span> <span data-ttu-id="3f72e-215">Komunikat sprawdzania poprawności zmienia się do wskazania, że tytuł musi mieć od 3 – 60 znaków:</span><span class="sxs-lookup"><span data-stu-id="3f72e-215">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Zmienić komunikat weryfikacji tytuł](upload-files/_static/browser3.png)

<span data-ttu-id="3f72e-217">Przekazywane co najmniej jeden harmonogram **załadowane harmonogramy** sekcji renderuje załadować harmonogramów:</span><span class="sxs-lookup"><span data-stu-id="3f72e-217">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Tabela załadować harmonogramy, przedstawiający tytuł każdy z harmonogramów przekazany daty UTC, rozmiar pliku w publicznej wersji i rozmiar pliku w prywatnej wersji](upload-files/_static/browser4.png)

<span data-ttu-id="3f72e-219">Użytkownik może kliknąć **Usuń** link z tego miejsca nawiązać widoku potwierdzenie usunięcia, które mają możliwość potwierdzenia lub anulować operację usuwania.</span><span class="sxs-lookup"><span data-stu-id="3f72e-219">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="3f72e-220">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="3f72e-220">Troubleshooting</span></span>

<span data-ttu-id="3f72e-221">Aby uzyskać informacje o rozwiązywaniu problemów z `IFormFile` przekazywania, zobacz [przekazywania plików z platformy ASP.NET Core: Rozwiązywanie problemów z](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="3f72e-221">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>
