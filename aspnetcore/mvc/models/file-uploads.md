---
title: Przekaż pliki w ASP.NET Core
author: guardrex
description: Jak używać powiązania modelu i przesyłania strumieniowego do przekazywania plików w ASP.NET Core MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/04/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: 20e58660185a3055e06e92d9136e80e2394a470d
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881069"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="3ba59-103">Przekaż pliki w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ba59-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="3ba59-104">[Luke Latham](https://github.com/guardrex), [Steve Kowalski](https://ardalis.com/)i [Rutger burza](https://github.com/rutix)</span><span class="sxs-lookup"><span data-stu-id="3ba59-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3ba59-105">ASP.NET Core obsługuje przekazywanie co najmniej jednego pliku przy użyciu powiązania z buforowanym modelem dla mniejszych plików i przesyłania strumieniowego z buforem dla większych plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="3ba59-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3ba59-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="3ba59-107">Zagadnienia dotyczące bezpieczeństwa</span><span class="sxs-lookup"><span data-stu-id="3ba59-107">Security considerations</span></span>

<span data-ttu-id="3ba59-108">Należy zachować ostrożność, zapewniając użytkownikom możliwość przekazywania plików na serwer.</span><span class="sxs-lookup"><span data-stu-id="3ba59-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="3ba59-109">Osoby atakujące mogą próbować:</span><span class="sxs-lookup"><span data-stu-id="3ba59-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="3ba59-110">Wykonywanie ataków [typu "odmowa usługi"](/windows-hardware/drivers/ifs/denial-of-service) .</span><span class="sxs-lookup"><span data-stu-id="3ba59-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="3ba59-111">Przekazuj wirusy lub złośliwe oprogramowanie.</span><span class="sxs-lookup"><span data-stu-id="3ba59-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="3ba59-112">Naruszyć bezpieczeństwo sieci i serwerów w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="3ba59-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="3ba59-113">Kroki zabezpieczeń, które zmniejszają prawdopodobieństwo pomyślnego ataku:</span><span class="sxs-lookup"><span data-stu-id="3ba59-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="3ba59-114">Przekaż pliki do dedykowanego obszaru przekazywania plików, najlepiej do dysku niesystemowego.</span><span class="sxs-lookup"><span data-stu-id="3ba59-114">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="3ba59-115">Dedykowana lokalizacja ułatwia nakładanie ograniczeń zabezpieczeń na przekazane pliki.</span><span class="sxs-lookup"><span data-stu-id="3ba59-115">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="3ba59-116">Wyłącz uprawnienia do wykonywania w lokalizacji przekazywania pliku.&dagger;</span><span class="sxs-lookup"><span data-stu-id="3ba59-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="3ba59-117">**Nie** Utrwalaj przekazanych plików w tym samym drzewie katalogów co aplikacja.&dagger;</span><span class="sxs-lookup"><span data-stu-id="3ba59-117">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="3ba59-118">Użyj bezpiecznej nazwy pliku, która jest określana przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="3ba59-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="3ba59-119">Nie należy używać nazwy pliku dostarczonej przez użytkownika lub niezaufanej nazwy pliku przekazanego pliku.&dagger; kod HTML zakodować niezaufaną nazwę pliku podczas jego wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="3ba59-120">Na przykład podczas rejestrowania nazwy pliku lub wyświetlania w interfejsie użytkownika (Razor automatyczne kodowanie HTML kodu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-120">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="3ba59-121">Zezwalaj tylko na zatwierdzone rozszerzenia plików dla specyfikacji projektu aplikacji.&dagger;</span><span class="sxs-lookup"><span data-stu-id="3ba59-121">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="3ba59-122">Sprawdź, czy testy po stronie klienta są wykonywane na serwerze.&dagger; sprawdzenia po stronie klienta można łatwo obejść.</span><span class="sxs-lookup"><span data-stu-id="3ba59-122">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="3ba59-123">Sprawdź rozmiar przekazanego pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-123">Check the size of an uploaded file.</span></span> <span data-ttu-id="3ba59-124">Ustaw maksymalny limit rozmiaru, aby zapobiec dużej ilości operacji przekazywania.&dagger;</span><span class="sxs-lookup"><span data-stu-id="3ba59-124">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="3ba59-125">Jeśli pliki nie powinny być zastąpione przez przekazany plik o tej samej nazwie, przed przekazaniem pliku Sprawdź nazwę pliku względem bazy danych lub magazynu fizycznego.</span><span class="sxs-lookup"><span data-stu-id="3ba59-125">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="3ba59-126">**Przed zapisaniem pliku Uruchom skaner wirusów/złośliwego oprogramowania dla przekazanej zawartości.**</span><span class="sxs-lookup"><span data-stu-id="3ba59-126">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="3ba59-127">&dagger;przykładowej aplikacji przedstawiono podejście, które spełnia kryteria.</span><span class="sxs-lookup"><span data-stu-id="3ba59-127">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="3ba59-128">Przekazywanie złośliwego kodu do systemu często jest pierwszym krokiem do wykonywania kodu, która może być:</span><span class="sxs-lookup"><span data-stu-id="3ba59-128">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="3ba59-129">Całkowicie przejąć kontrolę nad systemem.</span><span class="sxs-lookup"><span data-stu-id="3ba59-129">Completely gain control of a system.</span></span>
> * <span data-ttu-id="3ba59-130">Przeciąż system z wynikiem awarii systemu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-130">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="3ba59-131">Naruszyć bezpieczeństwo danych użytkownika lub systemu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-131">Compromise user or system data.</span></span>
> * <span data-ttu-id="3ba59-132">Zastosuj graffiti do publicznego interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3ba59-132">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="3ba59-133">Instrukcje dotyczące zmniejszenie obszaru powierzchni ataku, akceptując pliki użytkowników zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="3ba59-133">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="3ba59-134">Przekazywanie pliku bez ograniczeń</span><span class="sxs-lookup"><span data-stu-id="3ba59-134">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="3ba59-135">Zabezpieczenia platformy Azure: Upewnij się, że odpowiednie formanty są stosowane podczas akceptowania plików od użytkowników</span><span class="sxs-lookup"><span data-stu-id="3ba59-135">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="3ba59-136">Aby uzyskać więcej informacji na temat implementowania środków zabezpieczeń, w tym przykładów z przykładowej aplikacji, zobacz sekcję [Walidacja](#validation) .</span><span class="sxs-lookup"><span data-stu-id="3ba59-136">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="3ba59-137">Scenariusze magazynu</span><span class="sxs-lookup"><span data-stu-id="3ba59-137">Storage scenarios</span></span>

<span data-ttu-id="3ba59-138">Typowe opcje magazynu dla plików to:</span><span class="sxs-lookup"><span data-stu-id="3ba59-138">Common storage options for files include:</span></span>

* <span data-ttu-id="3ba59-139">Baza danych programu</span><span class="sxs-lookup"><span data-stu-id="3ba59-139">Database</span></span>

  * <span data-ttu-id="3ba59-140">W przypadku małych operacji przekazywania plików baza danych jest często szybsza niż opcje magazynu fizycznego (systemu plików lub udziału sieciowego).</span><span class="sxs-lookup"><span data-stu-id="3ba59-140">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="3ba59-141">Baza danych jest często bardziej wygodna niż opcje magazynu fizycznego, ponieważ Pobieranie rekordu bazy danych dla danych użytkownika może jednocześnie dostarczyć zawartość pliku (na przykład obraz awatara).</span><span class="sxs-lookup"><span data-stu-id="3ba59-141">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="3ba59-142">Baza danych może być tańsza niż użycie usługi magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="3ba59-142">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="3ba59-143">Magazyn fizyczny (system plików lub udział sieciowy)</span><span class="sxs-lookup"><span data-stu-id="3ba59-143">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="3ba59-144">W przypadku dużych operacji przekazywania plików:</span><span class="sxs-lookup"><span data-stu-id="3ba59-144">For large file uploads:</span></span>
    * <span data-ttu-id="3ba59-145">Limity bazy danych mogą ograniczać rozmiar przekazywania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-145">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="3ba59-146">Magazyn fizyczny jest często mniej ekonomiczny niż magazyn w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3ba59-146">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="3ba59-147">Magazyn fizyczny jest prawdopodobnie tańszy niż użycie usługi magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="3ba59-147">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="3ba59-148">Proces aplikacji musi mieć uprawnienia do odczytu i zapisu w lokalizacji magazynu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-148">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="3ba59-149">**Nigdy nie udzielaj uprawnienia EXECUTE.**</span><span class="sxs-lookup"><span data-stu-id="3ba59-149">**Never grant execute permission.**</span></span>

* <span data-ttu-id="3ba59-150">Usługa magazynu danych (na przykład [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="3ba59-150">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="3ba59-151">Usługa zazwyczaj oferuje ulepszoną skalowalność i odporność w rozwiązaniach lokalnych, które zwykle podlegają pojedynczym punktom awarii.</span><span class="sxs-lookup"><span data-stu-id="3ba59-151">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="3ba59-152">Usługi są potencjalnie tańsze w dużych scenariuszach infrastruktury magazynu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-152">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="3ba59-153">Aby uzyskać więcej informacji, zobacz [Szybki Start: korzystanie z platformy .NET do tworzenia obiektów BLOB w magazynie obiektów](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="3ba59-153">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="3ba59-154">Temat ilustruje <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, ale <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> może służyć do zapisywania <xref:System.IO.FileStream> do magazynu obiektów BLOB podczas pracy z <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="3ba59-154">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="3ba59-155">Scenariusze przekazywania plików</span><span class="sxs-lookup"><span data-stu-id="3ba59-155">File upload scenarios</span></span>

<span data-ttu-id="3ba59-156">Dwie ogólne podejścia do przekazywania plików to buforowanie i przesyłanie strumieniowe.</span><span class="sxs-lookup"><span data-stu-id="3ba59-156">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="3ba59-157">**Buforowania**</span><span class="sxs-lookup"><span data-stu-id="3ba59-157">**Buffering**</span></span>

<span data-ttu-id="3ba59-158">Cały plik jest odczytywany do <xref:Microsoft.AspNetCore.Http.IFormFile>, który jest C# reprezentacją pliku używanego do przetworzenia lub zapisania pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-158">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="3ba59-159">Zasoby (dysk, pamięć) używane przez operacje przekazywania plików zależą od liczby i rozmiaru współbieżnych przekazywania plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-159">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="3ba59-160">Jeśli aplikacja próbuje buforować zbyt wiele przeciążeń, lokacja uległa awarii, gdy zabraknie pamięci lub miejsca na dysku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-160">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="3ba59-161">Jeśli rozmiar lub częstotliwość przekazywania plików powoduje wyczerpanie zasobów aplikacji, należy użyć przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="3ba59-161">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="3ba59-162">Każdy pojedynczy buforowany plik przekraczający 64 KB jest przenoszony z pamięci do pliku tymczasowego na dysku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-162">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="3ba59-163">Buforowanie małych plików zostało omówione w następujących sekcjach tego tematu:</span><span class="sxs-lookup"><span data-stu-id="3ba59-163">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="3ba59-164">Magazyn fizyczny</span><span class="sxs-lookup"><span data-stu-id="3ba59-164">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="3ba59-165">Baza danych</span><span class="sxs-lookup"><span data-stu-id="3ba59-165">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="3ba59-166">**Przesyłanie strumieniowe**</span><span class="sxs-lookup"><span data-stu-id="3ba59-166">**Streaming**</span></span>

<span data-ttu-id="3ba59-167">Plik jest odbierany od żądania wieloczęściowego i bezpośrednio przetwarzany lub zapisywany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="3ba59-167">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="3ba59-168">Przesyłanie strumieniowe nie poprawia znacząco wydajności.</span><span class="sxs-lookup"><span data-stu-id="3ba59-168">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="3ba59-169">Przesyłanie strumieniowe zmniejsza wymagania dotyczące pamięci lub miejsca na dysku podczas przekazywania plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-169">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="3ba59-170">Przesyłanie strumieniowe dużych plików jest omówione w sekcji [przekazywanie dużych plików z przesyłaniem strumieniowym](#upload-large-files-with-streaming) .</span><span class="sxs-lookup"><span data-stu-id="3ba59-170">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="3ba59-171">Przekazywanie małych plików z buforowanym powiązaniem modelu z magazynem fizycznym</span><span class="sxs-lookup"><span data-stu-id="3ba59-171">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="3ba59-172">Aby przekazać małe pliki, użyj formularza wieloczęściowego lub Skonstruuj żądanie POST przy użyciu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3ba59-172">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="3ba59-173">Poniższy przykład ilustruje użycie formularza Razor Pages do przekazywania pojedynczego pliku (*Pages/BufferedSingleFileUploadPhysical. cshtml* w przykładowej aplikacji):</span><span class="sxs-lookup"><span data-stu-id="3ba59-173">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
            <span asp-validation-for="FileUpload.FormFile"></span>
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload" />
</form>
```

<span data-ttu-id="3ba59-174">Poniższy przykład jest analogiczny do poprzedniego przykładu, z wyjątkiem tego, że:</span><span class="sxs-lookup"><span data-stu-id="3ba59-174">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="3ba59-175">Kod JavaScript ([interfejs API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API)) służy do przesyłania danych formularza.</span><span class="sxs-lookup"><span data-stu-id="3ba59-175">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="3ba59-176">Nie istnieje weryfikacja.</span><span class="sxs-lookup"><span data-stu-id="3ba59-176">There's no validation.</span></span>

```cshtml
<form action="BufferedSingleFileUploadPhysical/?handler=Upload" 
      enctype="multipart/form-data" onsubmit="AJAXSubmit(this);return false;" 
      method="post">
    <dl>
        <dt>
            <label for="FileUpload_FormFile">File</label>
        </dt>
        <dd>
            <input id="FileUpload_FormFile" type="file" 
                name="FileUpload.FormFile" />
        </dd>
    </dl>

    <input class="btn" type="submit" value="Upload" />

    <div style="margin-top:15px">
        <output name="result"></output>
    </div>
</form>

<script>
  async function AJAXSubmit (oFormElement) {
    var resultElement = oFormElement.elements.namedItem("result");
    const formData = new FormData(oFormElement);

    try {
    const response = await fetch(oFormElement.action, {
      method: 'POST',
      body: formData
    });

    if (response.ok) {
      window.location.href = '/';
    }

    resultElement.value = 'Result: ' + response.status + ' ' + 
      response.statusText;
    } catch (error) {
      console.error('Error:', error);
    }
  }
</script>
```

<span data-ttu-id="3ba59-177">Aby wykonać formularz POST w języku JavaScript dla klientów, którzy [nie obsługują interfejsu API pobierania](https://caniuse.com/#feat=fetch), należy użyć jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="3ba59-177">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="3ba59-178">Użyj wypełniania pobierania (na przykład [window. Fetch Fill (GitHub/Fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="3ba59-178">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="3ba59-179">Użyj `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="3ba59-179">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="3ba59-180">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3ba59-180">For example:</span></span>

  ```javascript
  <script>
    "use strict";

    function AJAXSubmit (oFormElement) {
      var oReq = new XMLHttpRequest();
      oReq.onload = function(e) { 
      oFormElement.elements.namedItem("result").value = 
        'Result: ' + this.status + ' ' + this.statusText;
      };
      oReq.open("post", oFormElement.action);
      oReq.send(new FormData(oFormElement));
    }
  </script>
  ```

<span data-ttu-id="3ba59-181">W celu obsługi przekazywania plików formularze HTML muszą określać typ kodowania (`enctype`) `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="3ba59-181">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="3ba59-182">Aby `files` element wejściowy obsługujący przekazywanie wielu plików, podaj atrybut `multiple` dla elementu `<input>`:</span><span class="sxs-lookup"><span data-stu-id="3ba59-182">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="3ba59-183">Do poszczególnych plików przekazanych do serwera można uzyskać dostęp za pośrednictwem [powiązania modelu](xref:mvc/models/model-binding) przy użyciu <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="3ba59-183">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="3ba59-184">Przykładowa aplikacja pokazuje wiele buforowanych operacji przekazywania plików dla scenariuszy bazy danych i magazynu fizycznego.</span><span class="sxs-lookup"><span data-stu-id="3ba59-184">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename"></a>

> [!WARNING]
> <span data-ttu-id="3ba59-185">**Nie** należy używać właściwości `FileName` <xref:Microsoft.AspNetCore.Http.IFormFile> innej niż na potrzeby wyświetlania i rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-185">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="3ba59-186">Podczas wyświetlania lub rejestrowania, kod HTML koduje nazwę pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-186">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="3ba59-187">Osoba atakująca może dostarczyć złośliwą nazwę pliku, w tym pełne ścieżki lub ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="3ba59-187">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="3ba59-188">Aplikacje powinny:</span><span class="sxs-lookup"><span data-stu-id="3ba59-188">Applications should:</span></span>
>
> * <span data-ttu-id="3ba59-189">Usuń ścieżkę z nazwy pliku dostarczonej przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3ba59-189">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="3ba59-190">Zapisz plik w formacie HTML, który został usunięty z ścieżką dla interfejsu użytkownika lub rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-190">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="3ba59-191">Wygeneruj nową losową nazwę pliku dla magazynu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-191">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="3ba59-192">Poniższy kod usuwa ścieżkę z nazwy pliku:</span><span class="sxs-lookup"><span data-stu-id="3ba59-192">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="3ba59-193">Przykłady udostępnione w ten sposób nie uwzględniają zagadnień związanych z bezpieczeństwem.</span><span class="sxs-lookup"><span data-stu-id="3ba59-193">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="3ba59-194">Dodatkowe informacje są dostarczane przez następujące sekcje i [Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="3ba59-194">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="3ba59-195">Zagadnienia dotyczące bezpieczeństwa</span><span class="sxs-lookup"><span data-stu-id="3ba59-195">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="3ba59-196">Walidacja</span><span class="sxs-lookup"><span data-stu-id="3ba59-196">Validation</span></span>](#validation)

<span data-ttu-id="3ba59-197">Podczas przekazywania plików przy użyciu powiązania modelu i <xref:Microsoft.AspNetCore.Http.IFormFile>, Metoda akcji może przyjmować:</span><span class="sxs-lookup"><span data-stu-id="3ba59-197">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="3ba59-198">Jeden <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="3ba59-198">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="3ba59-199">Dowolne z następujących kolekcji, które reprezentują kilka plików:</span><span class="sxs-lookup"><span data-stu-id="3ba59-199">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="3ba59-200">\<<xref:Microsoft.AspNetCore.Http.IFormFile>[listy](xref:System.Collections.Generic.List`1) ></span><span class="sxs-lookup"><span data-stu-id="3ba59-200">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="3ba59-201">Powiązanie dopasowuje pliki formularza według nazwy.</span><span class="sxs-lookup"><span data-stu-id="3ba59-201">Binding matches form files by name.</span></span> <span data-ttu-id="3ba59-202">Na przykład wartość `name` HTML w `<input type="file" name="formFile">` musi być zgodna z C# wartością parametru/właściwością (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="3ba59-202">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="3ba59-203">Aby uzyskać więcej informacji, zobacz sekcję [dopasowanie wartości atrybutu do nazwy parametru w sekcji Metoda post](#match-name-attribute-value-to-parameter-name-of-post-method) .</span><span class="sxs-lookup"><span data-stu-id="3ba59-203">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="3ba59-204">Poniższy przykład:</span><span class="sxs-lookup"><span data-stu-id="3ba59-204">The following example:</span></span>

* <span data-ttu-id="3ba59-205">Pętle przez co najmniej jeden przekazany plik.</span><span class="sxs-lookup"><span data-stu-id="3ba59-205">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="3ba59-206">Używa funkcji [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) , aby zwrócić pełną ścieżkę do pliku, łącznie z nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-206">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="3ba59-207">Zapisuje pliki w lokalnym systemie plików przy użyciu nazwy pliku wygenerowanej przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="3ba59-207">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="3ba59-208">Zwraca łączną liczbę i rozmiar przekazanych plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-208">Returns the total number and size of files uploaded.</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> files)
{
    long size = files.Sum(f => f.Length);

    foreach (var formFile in files)
    {
        if (formFile.Length > 0)
        {
            var filePath = Path.GetTempFileName();

            using (var stream = System.IO.File.Create(filePath))
            {
                await formFile.CopyToAsync(stream);
            }
        }
    }

    // Process uploaded files
    // Don't rely on or trust the FileName property without validation.

    return Ok(new { count = files.Count, size, filePath });
}
```

<span data-ttu-id="3ba59-209">Użyj `Path.GetRandomFileName` do wygenerowania nazwy pliku bez ścieżki.</span><span class="sxs-lookup"><span data-stu-id="3ba59-209">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="3ba59-210">W poniższym przykładzie ścieżka jest uzyskiwana z konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="3ba59-210">In the following example, the path is obtained from configuration:</span></span>

```csharp
foreach (var formFile in files)
{
    if (formFile.Length > 0)
    {
        var filePath = Path.Combine(_config["StoredFilesPath"], 
            Path.GetRandomFileName());

        using (var stream = System.IO.File.Create(filePath))
        {
            await formFile.CopyToAsync(stream);
        }
    }
}
```

<span data-ttu-id="3ba59-211">Ścieżka przeniesiona do <xref:System.IO.FileStream> *musi* zawierać nazwę pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-211">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="3ba59-212">Jeśli nie podano nazwy pliku, <xref:System.UnauthorizedAccessException> jest generowany w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-212">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="3ba59-213">Pliki przekazane przy użyciu techniki <xref:Microsoft.AspNetCore.Http.IFormFile> są buforowane w pamięci lub na dysku na serwerze przed przetworzeniem.</span><span class="sxs-lookup"><span data-stu-id="3ba59-213">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="3ba59-214">Wewnątrz metody akcji <xref:Microsoft.AspNetCore.Http.IFormFile> zawartość jest dostępna jako <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="3ba59-214">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="3ba59-215">Oprócz lokalnego systemu plików można zapisywać pliki w udziale sieciowym lub w usłudze magazynu plików, na przykład w [usłudze Azure Blob Storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="3ba59-215">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="3ba59-216">Aby uzyskać inny przykład pętli dla wielu plików na potrzeby przekazywania i używania bezpiecznych nazw plików, zobacz *Pages/BufferedMultipleFileUploadPhysical. cshtml. cs* w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba59-216">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="3ba59-217">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) zgłasza <xref:System.IO.IOException>, jeśli utworzono więcej niż 65 535 plików bez usuwania poprzednich plików tymczasowych.</span><span class="sxs-lookup"><span data-stu-id="3ba59-217">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="3ba59-218">Limit 65 535 plików jest limitem dla serwera.</span><span class="sxs-lookup"><span data-stu-id="3ba59-218">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="3ba59-219">Aby uzyskać więcej informacji na temat tego limitu dla systemu operacyjnego Windows, zobacz uwagi w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="3ba59-219">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="3ba59-220">GetTempFileNameA, funkcja</span><span class="sxs-lookup"><span data-stu-id="3ba59-220">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="3ba59-221">Przekazywanie małych plików z buforowanym powiązaniem modelu z bazą danych</span><span class="sxs-lookup"><span data-stu-id="3ba59-221">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="3ba59-222">Aby przechowywać dane binarne pliku w bazie danych przy użyciu [Entity Framework](/ef/core/index), zdefiniuj Właściwość <xref:System.Byte> tablicy dla jednostki:</span><span class="sxs-lookup"><span data-stu-id="3ba59-222">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="3ba59-223">Określ właściwość modelu strony dla klasy, która zawiera <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="3ba59-223">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

```csharp
public class BufferedSingleFileUploadDbModel : PageModel
{
    ...

    [BindProperty]
    public BufferedSingleFileUploadDb FileUpload { get; set; }

    ...
}

public class BufferedSingleFileUploadDb
{
    [Required]
    [Display(Name="File")]
    public IFormFile FormFile { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="3ba59-224"><xref:Microsoft.AspNetCore.Http.IFormFile> można używać bezpośrednio jako parametru metody akcji lub jako powiązanej właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-224"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="3ba59-225">W poprzednim przykładzie użyto powiązanej właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-225">The prior example uses a bound model property.</span></span>

<span data-ttu-id="3ba59-226">`FileUpload` jest używana w formularzu Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="3ba59-226">The `FileUpload` is used in the Razor Pages form:</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload">
</form>
```

<span data-ttu-id="3ba59-227">Po opublikowaniu formularza na serwerze Skopiuj <xref:Microsoft.AspNetCore.Http.IFormFile> do strumienia i Zapisz go jako tablicę bajtową w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3ba59-227">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="3ba59-228">W poniższym przykładzie `_dbContext` przechowuje kontekst bazy danych aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3ba59-228">In the following example, `_dbContext` stores the app's database context:</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync()
{
    using (var memoryStream = new MemoryStream())
    {
        await FileUpload.FormFile.CopyToAsync(memoryStream);

        // Upload the file if less than 2 MB
        if (memoryStream.Length < 2097152)
        {
            var file = new AppFile()
            {
                Content = memoryStream.ToArray()
            };

            _dbContext.File.Add(file);

            await _dbContext.SaveChangesAsync();
        }
        else
        {
            ModelState.AddModelError("File", "The file is too large.");
        }
    }

    return Page();
}
```

<span data-ttu-id="3ba59-229">Poprzedni przykład przypomina scenariusz przedstawiony w przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3ba59-229">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="3ba59-230">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="3ba59-230">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="3ba59-231">*Strony/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="3ba59-231">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="3ba59-232">Należy zachować ostrożność podczas przechowywania danych binarnych w relacyjnych bazach danych, ponieważ może to mieć negatywny wpływ na wydajność.</span><span class="sxs-lookup"><span data-stu-id="3ba59-232">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="3ba59-233">Nie używaj ani nie ufaj właściwości `FileName` <xref:Microsoft.AspNetCore.Http.IFormFile> bez weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba59-233">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="3ba59-234">Właściwość `FileName` powinna być używana tylko do celów wyświetlania i tylko po kodowaniu kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="3ba59-234">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="3ba59-235">Podane przykłady nie uwzględniają zagadnień związanych z zabezpieczeniami.</span><span class="sxs-lookup"><span data-stu-id="3ba59-235">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="3ba59-236">Dodatkowe informacje są dostarczane przez następujące sekcje i [Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="3ba59-236">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="3ba59-237">Zagadnienia dotyczące bezpieczeństwa</span><span class="sxs-lookup"><span data-stu-id="3ba59-237">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="3ba59-238">Walidacja</span><span class="sxs-lookup"><span data-stu-id="3ba59-238">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="3ba59-239">Przekazywanie dużych plików strumieniowo</span><span class="sxs-lookup"><span data-stu-id="3ba59-239">Upload large files with streaming</span></span>

<span data-ttu-id="3ba59-240">Poniższy przykład ilustruje sposób użycia języka JavaScript do przesyłania strumieniowego pliku do akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3ba59-240">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="3ba59-241">Token antysfałszowany pliku jest generowany przy użyciu niestandardowego atrybutu filtru i przekazywać do nagłówków HTTP klienta zamiast w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-241">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="3ba59-242">Ponieważ metoda akcji przetwarza przekazane dane bezpośrednio, powiązanie modelu formularza jest wyłączone przez inny filtr niestandardowy.</span><span class="sxs-lookup"><span data-stu-id="3ba59-242">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="3ba59-243">W ramach akcji zawartość formularza jest odczytywana przy użyciu `MultipartReader`, który odczytuje poszczególne `MultipartSection`, przetwarza plik lub przechowując zawartość odpowiednio do potrzeb.</span><span class="sxs-lookup"><span data-stu-id="3ba59-243">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="3ba59-244">Po odczytaniu sekcji wieloczęściowej akcja wykonuje własne powiązanie modelu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-244">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="3ba59-245">Początkowa odpowiedź strony ładuje formularz i zapisuje w pliku cookie token antysfałszowany (za pośrednictwem atrybutu `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="3ba59-245">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="3ba59-246">Ten atrybut używa wbudowanej [obsługi przed fałszowaniem](xref:security/anti-request-forgery) ASP.NET Core, aby ustawić plik cookie z tokenem żądania:</span><span class="sxs-lookup"><span data-stu-id="3ba59-246">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="3ba59-247">`DisableFormValueModelBindingAttribute` jest używany do wyłączania powiązania modelu:</span><span class="sxs-lookup"><span data-stu-id="3ba59-247">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="3ba59-248">W przykładowej aplikacji `GenerateAntiforgeryTokenCookieAttribute` i `DisableFormValueModelBindingAttribute` są stosowane jako filtry do modeli aplikacji strony `/StreamedSingleFileUploadDb` i `/StreamedSingleFileUploadPhysical` w `Startup.ConfigureServices` przy użyciu [konwencji Razor Pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="3ba59-248">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Startup.cs?name=snippet_AddRazorPages&highlight=8-11,17-20)]

<span data-ttu-id="3ba59-249">Ponieważ powiązanie modelu nie odczytuje formularza, parametry, które są powiązane z formularza nie są powiązane (zapytania, trasy i nagłówki nadal pracują).</span><span class="sxs-lookup"><span data-stu-id="3ba59-249">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="3ba59-250">Metoda akcji działa bezpośrednio z właściwością `Request`.</span><span class="sxs-lookup"><span data-stu-id="3ba59-250">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="3ba59-251">`MultipartReader` jest używany do odczytywania każdej sekcji.</span><span class="sxs-lookup"><span data-stu-id="3ba59-251">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="3ba59-252">Dane klucza/wartości są przechowywane w `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="3ba59-252">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="3ba59-253">Po odczytaniu sekcji wieloczęściowych zawartość `KeyValueAccumulator` są używane do powiązania danych formularza z typem modelu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-253">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="3ba59-254">Pełna `StreamingController.UploadDatabase` Metoda przesyłania strumieniowego do bazy danych z EF Core:</span><span class="sxs-lookup"><span data-stu-id="3ba59-254">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="3ba59-255">`MultipartRequestHelper` (*Narzędzia/MultipartRequestHelper. cs*):</span><span class="sxs-lookup"><span data-stu-id="3ba59-255">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="3ba59-256">Pełna `StreamingController.UploadPhysical` Metoda przesyłania strumieniowego do lokalizacji fizycznej:</span><span class="sxs-lookup"><span data-stu-id="3ba59-256">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/3.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="3ba59-257">W przykładowej aplikacji sprawdzanie poprawności jest obsługiwane przez `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="3ba59-257">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="3ba59-258">Walidacja</span><span class="sxs-lookup"><span data-stu-id="3ba59-258">Validation</span></span>

<span data-ttu-id="3ba59-259">Klasa `FileHelpers` aplikacji przykładowej pokazuje kilka testów dla buforowanych <xref:Microsoft.AspNetCore.Http.IFormFile> i przesyłania strumieniowego plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-259">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="3ba59-260">Aby przetwarzać <xref:Microsoft.AspNetCore.Http.IFormFile> buforowane operacje przekazywania plików w aplikacji przykładowej, zobacz `ProcessFormFile` metody w pliku *Utilities/FileHelpers. cs* .</span><span class="sxs-lookup"><span data-stu-id="3ba59-260">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="3ba59-261">Aby przetwarzać pliki przesyłane strumieniowo, zobacz metodę `ProcessStreamedFile` w tym samym pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-261">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="3ba59-262">Metody przetwarzania walidacji przedstawione w przykładowej aplikacji nie skanują zawartości przekazanych plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-262">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="3ba59-263">W większości scenariuszy produkcyjnych do pliku jest używany interfejs API skanera wirusów/złośliwego oprogramowania przed udostępnieniem go użytkownikom lub innym systemom.</span><span class="sxs-lookup"><span data-stu-id="3ba59-263">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="3ba59-264">Chociaż przykład tematu zawiera przykładowy technikę sprawdzania poprawności, nie należy implementować klasy `FileHelpers` w aplikacji produkcyjnej, chyba że:</span><span class="sxs-lookup"><span data-stu-id="3ba59-264">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="3ba59-265">W pełni zapoznaj się z implementacją.</span><span class="sxs-lookup"><span data-stu-id="3ba59-265">Fully understand the implementation.</span></span>
> * <span data-ttu-id="3ba59-266">Zmodyfikuj implementację odpowiednio do środowiska i specyfikacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba59-266">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="3ba59-267">**Nigdy nie należy wdrażać kodu zabezpieczeń w aplikacji bez konieczności ich rozwiązywania.**</span><span class="sxs-lookup"><span data-stu-id="3ba59-267">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="3ba59-268">Weryfikacja zawartości</span><span class="sxs-lookup"><span data-stu-id="3ba59-268">Content validation</span></span>

<span data-ttu-id="3ba59-269">**Użyj interfejsu API skanowania wirusa/złośliwego oprogramowania innej firmy dla przekazanej zawartości.**</span><span class="sxs-lookup"><span data-stu-id="3ba59-269">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="3ba59-270">Skanowanie plików wymaga użycia zasobów serwera w scenariuszach o dużych ilościach.</span><span class="sxs-lookup"><span data-stu-id="3ba59-270">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="3ba59-271">Jeśli wydajność przetwarzania żądań jest zmniejszana ze względu na skanowanie plików, rozważ odciążenie pracy skanowania do [usługi w tle](xref:fundamentals/host/hosted-services), prawdopodobnie usługi uruchomionej na serwerze innym niż serwer aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba59-271">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="3ba59-272">Zwykle przekazane pliki są przechowywane w obszarze poddany kwarantannie, dopóki skaner wirusów w tle nie sprawdzi ich.</span><span class="sxs-lookup"><span data-stu-id="3ba59-272">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="3ba59-273">Gdy plik kończy się, plik zostanie przeniesiony do normalnej lokalizacji przechowywania plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-273">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="3ba59-274">Te kroki są zwykle wykonywane w połączeniu z rekordem bazy danych, który wskazuje na stan skanowania pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-274">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="3ba59-275">Korzystając z takiego podejścia, aplikacja i serwer aplikacji pozostają skoncentrowane na odpowiedzi na żądania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-275">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="3ba59-276">Weryfikacja rozszerzenia pliku</span><span class="sxs-lookup"><span data-stu-id="3ba59-276">File extension validation</span></span>

<span data-ttu-id="3ba59-277">Rozszerzenie przekazanego pliku powinno być sprawdzane względem listy dozwolonych rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="3ba59-277">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="3ba59-278">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3ba59-278">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="3ba59-279">Walidacja podpisu pliku</span><span class="sxs-lookup"><span data-stu-id="3ba59-279">File signature validation</span></span>

<span data-ttu-id="3ba59-280">Sygnatura pliku jest określana przez pierwsze kilka bajtów na początku pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-280">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="3ba59-281">Te bajty mogą być używane do wskazywania, czy rozszerzenie jest zgodne z zawartością pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-281">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="3ba59-282">Przykładowa aplikacja sprawdza podpisy plików dla kilku popularnych typów plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-282">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="3ba59-283">W poniższym przykładzie podpis pliku dla obrazu JPEG jest sprawdzany pod kątem pliku:</span><span class="sxs-lookup"><span data-stu-id="3ba59-283">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

```csharp
private static readonly Dictionary<string, List<byte[]>> _fileSignature = 
    new Dictionary<string, List<byte[]>>
{
    { ".jpeg", new List<byte[]>
        {
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 },
        }
    },
};

using (var reader = new BinaryReader(uploadedFileData))
{
    var signatures = _fileSignature[ext];
    var headerBytes = reader.ReadBytes(signatures.Max(m => m.Length));
    
    return signatures.Any(signature => 
        headerBytes.Take(signature.Length).SequenceEqual(signature));
}
```

<span data-ttu-id="3ba59-284">Aby uzyskać dodatkowe podpisy plików, zapoznaj się z [bazami danych sygnatury plików](https://www.filesignatures.net/) i oficjalnymi specyfikacjami plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-284">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="3ba59-285">Zabezpieczenia nazw plików</span><span class="sxs-lookup"><span data-stu-id="3ba59-285">File name security</span></span>

<span data-ttu-id="3ba59-286">Nigdy nie należy używać nazwy pliku dostarczonej przez klienta do zapisywania plików w magazynie fizycznym.</span><span class="sxs-lookup"><span data-stu-id="3ba59-286">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="3ba59-287">Utwórz bezpieczną nazwę pliku dla pliku przy użyciu [ścieżki. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) lub [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) , aby utworzyć pełną ścieżkę (łącznie z nazwą pliku) dla magazynu tymczasowego.</span><span class="sxs-lookup"><span data-stu-id="3ba59-287">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="3ba59-288">KOD Razor automatycznie koduje wartości właściwości dla wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-288">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="3ba59-289">Następujący kod jest bezpieczny w użyciu:</span><span class="sxs-lookup"><span data-stu-id="3ba59-289">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="3ba59-290">Poza Razor, zawsze <xref:System.Net.WebUtility.HtmlEncode*> zawartość nazwy pliku z żądania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3ba59-290">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="3ba59-291">Wiele implementacji musi zawierać sprawdzenie, czy plik istnieje; w przeciwnym razie plik zostanie zastąpiony przez plik o tej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="3ba59-291">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="3ba59-292">Podaj dodatkową logikę, aby spełnić wymagania dotyczące Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba59-292">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="3ba59-293">Sprawdzanie poprawności rozmiaru</span><span class="sxs-lookup"><span data-stu-id="3ba59-293">Size validation</span></span>

<span data-ttu-id="3ba59-294">Ogranicz rozmiar przekazanych plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-294">Limit the size of uploaded files.</span></span>

<span data-ttu-id="3ba59-295">W przykładowej aplikacji rozmiar pliku jest ograniczony do 2 MB (wyrażony w bajtach).</span><span class="sxs-lookup"><span data-stu-id="3ba59-295">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="3ba59-296">Limit jest dostarczany przez [konfigurację](xref:fundamentals/configuration/index) z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="3ba59-296">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="3ba59-297">`FileSizeLimit` jest wstrzykiwana do klas `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="3ba59-297">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

```csharp
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    private readonly long _fileSizeLimit;

    public BufferedSingleFileUploadPhysicalModel(IConfiguration config)
    {
        _fileSizeLimit = config.GetValue<long>("FileSizeLimit");
    }

    ...
}
```

<span data-ttu-id="3ba59-298">Gdy rozmiar pliku przekracza limit, plik zostanie odrzucony:</span><span class="sxs-lookup"><span data-stu-id="3ba59-298">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="3ba59-299">Dopasuj wartość atrybutu Name do nazwy parametru metody POST</span><span class="sxs-lookup"><span data-stu-id="3ba59-299">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="3ba59-300">W formularzach innych niż Razor, które PUBLIKUJą dane formularza lub używają `FormData` języka JavaScript bezpośrednio, nazwa określona w elemencie formularza lub `FormData` musi być zgodna z nazwą parametru w akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3ba59-300">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="3ba59-301">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="3ba59-301">In the following example:</span></span>

* <span data-ttu-id="3ba59-302">W przypadku używania elementu `<input>` atrybut `name` ma ustawioną wartość `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="3ba59-302">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="3ba59-303">W przypadku używania `FormData` w języku JavaScript nazwa jest ustawiana na wartość `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="3ba59-303">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="3ba59-304">Użyj zgodnej nazwy dla parametru C# metody (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="3ba59-304">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="3ba59-305">Dla metody obsługi strony Razor Pages o nazwie `Upload`:</span><span class="sxs-lookup"><span data-stu-id="3ba59-305">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="3ba59-306">Dla metody akcji składnika MVC Controller:</span><span class="sxs-lookup"><span data-stu-id="3ba59-306">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="3ba59-307">Konfiguracja serwera i aplikacji</span><span class="sxs-lookup"><span data-stu-id="3ba59-307">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="3ba59-308">Limit długości treści wieloczęściowej</span><span class="sxs-lookup"><span data-stu-id="3ba59-308">Multipart body length limit</span></span>

<span data-ttu-id="3ba59-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> ustawia limit długości każdej wieloczęściowej treści.</span><span class="sxs-lookup"><span data-stu-id="3ba59-309"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="3ba59-310">Sekcje formularzy, które przekraczają ten limit, generują <xref:System.IO.InvalidDataException> podczas analizowania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-310">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="3ba59-311">Wartość domyślna to 134 217 728 (128 MB).</span><span class="sxs-lookup"><span data-stu-id="3ba59-311">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="3ba59-312">Dostosuj limit przy użyciu ustawienia <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3ba59-312">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<span data-ttu-id="3ba59-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> służy do ustawiania <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> dla pojedynczej strony lub akcji.</span><span class="sxs-lookup"><span data-stu-id="3ba59-313"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="3ba59-314">W aplikacji Razor Pages Zastosuj filtr z [Konwencją](xref:razor-pages/razor-pages-conventions) w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3ba59-314">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRazorPages()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model.Filters.Add(
                    new RequestFormLimitsAttribute()
                    {
                        // Set the limit to 256 MB
                        MultipartBodyLengthLimit = 268435456
                    });
    });
```

<span data-ttu-id="3ba59-315">W aplikacji Razor Pages lub aplikacji MVC Zastosuj filtr do modelu strony lub metody akcji:</span><span class="sxs-lookup"><span data-stu-id="3ba59-315">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="3ba59-316">Maksymalny rozmiar treści żądania Kestrel</span><span class="sxs-lookup"><span data-stu-id="3ba59-316">Kestrel maximum request body size</span></span>

<span data-ttu-id="3ba59-317">W przypadku aplikacji hostowanych przez Kestrel domyślny maksymalny rozmiar treści żądania to 30 000 000 bajtów, czyli około 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="3ba59-317">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="3ba59-318">Dostosuj limit przy użyciu opcji serwera [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel:</span><span class="sxs-lookup"><span data-stu-id="3ba59-318">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="3ba59-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> służy do ustawiania [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) dla pojedynczej strony lub akcji.</span><span class="sxs-lookup"><span data-stu-id="3ba59-319"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="3ba59-320">W aplikacji Razor Pages Zastosuj filtr z [Konwencją](xref:razor-pages/razor-pages-conventions) w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3ba59-320">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddRazorPages()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model =>
                {
                    // Handle requests up to 50 MB
                    model.Filters.Add(
                        new RequestSizeLimitAttribute(52428800));
                });
    });
```

<span data-ttu-id="3ba59-321">W aplikacji strony Razor lub aplikacji MVC Zastosuj filtr do klasy procedury obsługi stron lub metody akcji:</span><span class="sxs-lookup"><span data-stu-id="3ba59-321">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

<span data-ttu-id="3ba59-322">`RequestSizeLimitAttribute` można również zastosować przy użyciu dyrektywy Razor [`@attribute`](xref:mvc/views/razor#attribute) :</span><span class="sxs-lookup"><span data-stu-id="3ba59-322">The `RequestSizeLimitAttribute` can also be applied using the [`@attribute`](xref:mvc/views/razor#attribute) Razor directive:</span></span>

```cshtml
@attribute [RequestSizeLimitAttribute(52428800)]
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="3ba59-323">Inne limity Kestrel</span><span class="sxs-lookup"><span data-stu-id="3ba59-323">Other Kestrel limits</span></span>

<span data-ttu-id="3ba59-324">Inne limity Kestrel mogą dotyczyć aplikacji hostowanych przez Kestrel:</span><span class="sxs-lookup"><span data-stu-id="3ba59-324">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="3ba59-325">Maksymalna liczba połączeń klienta</span><span class="sxs-lookup"><span data-stu-id="3ba59-325">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="3ba59-326">Stawki danych żądań i odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="3ba59-326">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="3ba59-327">Limit długości zawartości usług IIS</span><span class="sxs-lookup"><span data-stu-id="3ba59-327">IIS content length limit</span></span>

<span data-ttu-id="3ba59-328">Domyślny limit żądań (`maxAllowedContentLength`) to 30 000 000 bajtów, czyli około 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="3ba59-328">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="3ba59-329">Dostosuj limit w pliku *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="3ba59-329">Customize the limit in the *web.config* file:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- Handle requests up to 50 MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="3ba59-330">To ustawienie dotyczy tylko usług IIS.</span><span class="sxs-lookup"><span data-stu-id="3ba59-330">This setting only applies to IIS.</span></span> <span data-ttu-id="3ba59-331">Zachowanie nie występuje domyślnie podczas hostowania w Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3ba59-331">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="3ba59-332">Aby uzyskać więcej informacji, zobacz [limity żądań \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="3ba59-332">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="3ba59-333">Ograniczenia w module ASP.NET Core lub obecność modułu filtrowania żądań usług IIS mogą ograniczyć przekazywanie do 2 lub 4 GB.</span><span class="sxs-lookup"><span data-stu-id="3ba59-333">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="3ba59-334">Aby uzyskać więcej informacji, zobacz [nie można przekazać pliku o rozmiarze większym niż 2 GB (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="3ba59-334">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="3ba59-335">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="3ba59-335">Troubleshoot</span></span>

<span data-ttu-id="3ba59-336">Poniżej przedstawiono niektóre typowe problemy, które można napotkać podczas pracy z przekazywaniem plików i ich możliwymi rozwiązaniami.</span><span class="sxs-lookup"><span data-stu-id="3ba59-336">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="3ba59-337">Nie znaleziono błędu w przypadku wdrożenia na serwerze usług IIS</span><span class="sxs-lookup"><span data-stu-id="3ba59-337">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="3ba59-338">Następujący błąd wskazuje, że przekazany plik przekracza skonfigurowaną długość zawartości serwera:</span><span class="sxs-lookup"><span data-stu-id="3ba59-338">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="3ba59-339">Aby uzyskać więcej informacji na temat zwiększania limitu, zobacz sekcję [limit długości zawartości usług IIS](#iis-content-length-limit) .</span><span class="sxs-lookup"><span data-stu-id="3ba59-339">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="3ba59-340">Błąd połączenia</span><span class="sxs-lookup"><span data-stu-id="3ba59-340">Connection failure</span></span>

<span data-ttu-id="3ba59-341">Wystąpił błąd połączenia i połączenie z serwerem resetowania prawdopodobnie wskazuje, że przekazany plik przekracza maksymalny rozmiar treści żądania Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3ba59-341">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="3ba59-342">Aby uzyskać więcej informacji, zobacz sekcję [Maksymalny rozmiar treści żądania Kestrel](#kestrel-maximum-request-body-size) .</span><span class="sxs-lookup"><span data-stu-id="3ba59-342">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="3ba59-343">Limity połączeń klienta Kestrel mogą również wymagać korekty.</span><span class="sxs-lookup"><span data-stu-id="3ba59-343">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="3ba59-344">Wyjątek odwołania o wartości null z IFormFile</span><span class="sxs-lookup"><span data-stu-id="3ba59-344">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="3ba59-345">Jeśli kontroler akceptuje przekazane pliki przy użyciu <xref:Microsoft.AspNetCore.Http.IFormFile> ale wartość jest `null`, upewnij się, że w formularzu HTML określono wartość `enctype` `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="3ba59-345">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="3ba59-346">Jeśli ten atrybut nie jest ustawiony dla elementu `<form>`, przekazywanie pliku nie wystąpi i wszystkie powiązane argumenty <xref:Microsoft.AspNetCore.Http.IFormFile> są `null`.</span><span class="sxs-lookup"><span data-stu-id="3ba59-346">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="3ba59-347">Sprawdź również, czy [Nazwa przekazywania w postaci danych jest zgodna z nazewnictwem aplikacji](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="3ba59-347">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

### <a name="stream-was-too-long"></a><span data-ttu-id="3ba59-348">Strumień był zbyt długi</span><span class="sxs-lookup"><span data-stu-id="3ba59-348">Stream was too long</span></span>

<span data-ttu-id="3ba59-349">Przykłady w tym temacie polegają na <xref:System.IO.MemoryStream> do przechowywania zawartości przekazanego pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-349">The examples in this topic rely upon <xref:System.IO.MemoryStream> to hold the uploaded file's content.</span></span> <span data-ttu-id="3ba59-350">Limit rozmiaru `MemoryStream` jest `int.MaxValue`.</span><span class="sxs-lookup"><span data-stu-id="3ba59-350">The size limit of a `MemoryStream` is `int.MaxValue`.</span></span> <span data-ttu-id="3ba59-351">Jeśli scenariusz przekazywania plików aplikacji wymaga przechowywania zawartości pliku o rozmiarze większym niż 50 MB, użyj alternatywnego podejścia, które nie polega na pojedynczym `MemoryStream` do przechowywania zawartości przekazanego pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-351">If the app's file upload scenario requires holding file content larger than 50 MB, use an alternative approach that doesn't rely upon a single `MemoryStream` for holding an uploaded file's content.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3ba59-352">ASP.NET Core obsługuje przekazywanie co najmniej jednego pliku przy użyciu powiązania z buforowanym modelem dla mniejszych plików i przesyłania strumieniowego z buforem dla większych plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-352">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="3ba59-353">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3ba59-353">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="3ba59-354">Zagadnienia dotyczące bezpieczeństwa</span><span class="sxs-lookup"><span data-stu-id="3ba59-354">Security considerations</span></span>

<span data-ttu-id="3ba59-355">Należy zachować ostrożność, zapewniając użytkownikom możliwość przekazywania plików na serwer.</span><span class="sxs-lookup"><span data-stu-id="3ba59-355">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="3ba59-356">Osoby atakujące mogą próbować:</span><span class="sxs-lookup"><span data-stu-id="3ba59-356">Attackers may attempt to:</span></span>

* <span data-ttu-id="3ba59-357">Wykonywanie ataków [typu "odmowa usługi"](/windows-hardware/drivers/ifs/denial-of-service) .</span><span class="sxs-lookup"><span data-stu-id="3ba59-357">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="3ba59-358">Przekazuj wirusy lub złośliwe oprogramowanie.</span><span class="sxs-lookup"><span data-stu-id="3ba59-358">Upload viruses or malware.</span></span>
* <span data-ttu-id="3ba59-359">Naruszyć bezpieczeństwo sieci i serwerów w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="3ba59-359">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="3ba59-360">Kroki zabezpieczeń, które zmniejszają prawdopodobieństwo pomyślnego ataku:</span><span class="sxs-lookup"><span data-stu-id="3ba59-360">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="3ba59-361">Przekaż pliki do dedykowanego obszaru przekazywania plików, najlepiej do dysku niesystemowego.</span><span class="sxs-lookup"><span data-stu-id="3ba59-361">Upload files to a dedicated file upload area, preferably to a non-system drive.</span></span> <span data-ttu-id="3ba59-362">Dedykowana lokalizacja ułatwia nakładanie ograniczeń zabezpieczeń na przekazane pliki.</span><span class="sxs-lookup"><span data-stu-id="3ba59-362">A dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="3ba59-363">Wyłącz uprawnienia do wykonywania w lokalizacji przekazywania pliku.&dagger;</span><span class="sxs-lookup"><span data-stu-id="3ba59-363">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="3ba59-364">**Nie** Utrwalaj przekazanych plików w tym samym drzewie katalogów co aplikacja.&dagger;</span><span class="sxs-lookup"><span data-stu-id="3ba59-364">Do **not** persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="3ba59-365">Użyj bezpiecznej nazwy pliku, która jest określana przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="3ba59-365">Use a safe file name determined by the app.</span></span> <span data-ttu-id="3ba59-366">Nie należy używać nazwy pliku dostarczonej przez użytkownika lub niezaufanej nazwy pliku przekazanego pliku.&dagger; kod HTML zakodować niezaufaną nazwę pliku podczas jego wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-366">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; HTML encode the untrusted file name when displaying it.</span></span> <span data-ttu-id="3ba59-367">Na przykład podczas rejestrowania nazwy pliku lub wyświetlania w interfejsie użytkownika (Razor automatyczne kodowanie HTML kodu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-367">For example, logging the file name or displaying in UI (Razor automatically HTML encodes output).</span></span>
* <span data-ttu-id="3ba59-368">Zezwalaj tylko na zatwierdzone rozszerzenia plików dla specyfikacji projektu aplikacji.&dagger;</span><span class="sxs-lookup"><span data-stu-id="3ba59-368">Allow only approved file extensions for the app's design specification.&dagger;</span></span> <!-- * Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension. Add this back when we get instructions how to do this.  -->
* <span data-ttu-id="3ba59-369">Sprawdź, czy testy po stronie klienta są wykonywane na serwerze.&dagger; sprawdzenia po stronie klienta można łatwo obejść.</span><span class="sxs-lookup"><span data-stu-id="3ba59-369">Verify that client-side checks are performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="3ba59-370">Sprawdź rozmiar przekazanego pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-370">Check the size of an uploaded file.</span></span> <span data-ttu-id="3ba59-371">Ustaw maksymalny limit rozmiaru, aby zapobiec dużej ilości operacji przekazywania.&dagger;</span><span class="sxs-lookup"><span data-stu-id="3ba59-371">Set a maximum size limit to prevent large uploads.&dagger;</span></span>
* <span data-ttu-id="3ba59-372">Jeśli pliki nie powinny być zastąpione przez przekazany plik o tej samej nazwie, przed przekazaniem pliku Sprawdź nazwę pliku względem bazy danych lub magazynu fizycznego.</span><span class="sxs-lookup"><span data-stu-id="3ba59-372">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="3ba59-373">**Przed zapisaniem pliku Uruchom skaner wirusów/złośliwego oprogramowania dla przekazanej zawartości.**</span><span class="sxs-lookup"><span data-stu-id="3ba59-373">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="3ba59-374">&dagger;przykładowej aplikacji przedstawiono podejście, które spełnia kryteria.</span><span class="sxs-lookup"><span data-stu-id="3ba59-374">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="3ba59-375">Przekazywanie złośliwego kodu do systemu często jest pierwszym krokiem do wykonywania kodu, która może być:</span><span class="sxs-lookup"><span data-stu-id="3ba59-375">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="3ba59-376">Całkowicie przejąć kontrolę nad systemem.</span><span class="sxs-lookup"><span data-stu-id="3ba59-376">Completely gain control of a system.</span></span>
> * <span data-ttu-id="3ba59-377">Przeciąż system z wynikiem awarii systemu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-377">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="3ba59-378">Naruszyć bezpieczeństwo danych użytkownika lub systemu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-378">Compromise user or system data.</span></span>
> * <span data-ttu-id="3ba59-379">Zastosuj graffiti do publicznego interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3ba59-379">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="3ba59-380">Instrukcje dotyczące zmniejszenie obszaru powierzchni ataku, akceptując pliki użytkowników zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="3ba59-380">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="3ba59-381">Przekazywanie pliku bez ograniczeń</span><span class="sxs-lookup"><span data-stu-id="3ba59-381">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [<span data-ttu-id="3ba59-382">Zabezpieczenia platformy Azure: Upewnij się, że odpowiednie formanty są stosowane podczas akceptowania plików od użytkowników</span><span class="sxs-lookup"><span data-stu-id="3ba59-382">Azure Security: Ensure appropriate controls are in place when accepting files from users</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

<span data-ttu-id="3ba59-383">Aby uzyskać więcej informacji na temat implementowania środków zabezpieczeń, w tym przykładów z przykładowej aplikacji, zobacz sekcję [Walidacja](#validation) .</span><span class="sxs-lookup"><span data-stu-id="3ba59-383">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="3ba59-384">Scenariusze magazynu</span><span class="sxs-lookup"><span data-stu-id="3ba59-384">Storage scenarios</span></span>

<span data-ttu-id="3ba59-385">Typowe opcje magazynu dla plików to:</span><span class="sxs-lookup"><span data-stu-id="3ba59-385">Common storage options for files include:</span></span>

* <span data-ttu-id="3ba59-386">Baza danych programu</span><span class="sxs-lookup"><span data-stu-id="3ba59-386">Database</span></span>

  * <span data-ttu-id="3ba59-387">W przypadku małych operacji przekazywania plików baza danych jest często szybsza niż opcje magazynu fizycznego (systemu plików lub udziału sieciowego).</span><span class="sxs-lookup"><span data-stu-id="3ba59-387">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="3ba59-388">Baza danych jest często bardziej wygodna niż opcje magazynu fizycznego, ponieważ Pobieranie rekordu bazy danych dla danych użytkownika może jednocześnie dostarczyć zawartość pliku (na przykład obraz awatara).</span><span class="sxs-lookup"><span data-stu-id="3ba59-388">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="3ba59-389">Baza danych może być tańsza niż użycie usługi magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="3ba59-389">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="3ba59-390">Magazyn fizyczny (system plików lub udział sieciowy)</span><span class="sxs-lookup"><span data-stu-id="3ba59-390">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="3ba59-391">W przypadku dużych operacji przekazywania plików:</span><span class="sxs-lookup"><span data-stu-id="3ba59-391">For large file uploads:</span></span>
    * <span data-ttu-id="3ba59-392">Limity bazy danych mogą ograniczać rozmiar przekazywania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-392">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="3ba59-393">Magazyn fizyczny jest często mniej ekonomiczny niż magazyn w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3ba59-393">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="3ba59-394">Magazyn fizyczny jest prawdopodobnie tańszy niż użycie usługi magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="3ba59-394">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="3ba59-395">Proces aplikacji musi mieć uprawnienia do odczytu i zapisu w lokalizacji magazynu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-395">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="3ba59-396">**Nigdy nie udzielaj uprawnienia EXECUTE.**</span><span class="sxs-lookup"><span data-stu-id="3ba59-396">**Never grant execute permission.**</span></span>

* <span data-ttu-id="3ba59-397">Usługa magazynu danych (na przykład [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="3ba59-397">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="3ba59-398">Usługa zazwyczaj oferuje ulepszoną skalowalność i odporność w rozwiązaniach lokalnych, które zwykle podlegają pojedynczym punktom awarii.</span><span class="sxs-lookup"><span data-stu-id="3ba59-398">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="3ba59-399">Usługi są potencjalnie tańsze w dużych scenariuszach infrastruktury magazynu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-399">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="3ba59-400">Aby uzyskać więcej informacji, zobacz [Szybki Start: korzystanie z platformy .NET do tworzenia obiektów BLOB w magazynie obiektów](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span><span class="sxs-lookup"><span data-stu-id="3ba59-400">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="3ba59-401">Temat ilustruje <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, ale <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> może służyć do zapisywania <xref:System.IO.FileStream> do magazynu obiektów BLOB podczas pracy z <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="3ba59-401">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="3ba59-402">Scenariusze przekazywania plików</span><span class="sxs-lookup"><span data-stu-id="3ba59-402">File upload scenarios</span></span>

<span data-ttu-id="3ba59-403">Dwie ogólne podejścia do przekazywania plików to buforowanie i przesyłanie strumieniowe.</span><span class="sxs-lookup"><span data-stu-id="3ba59-403">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="3ba59-404">**Buforowania**</span><span class="sxs-lookup"><span data-stu-id="3ba59-404">**Buffering**</span></span>

<span data-ttu-id="3ba59-405">Cały plik jest odczytywany do <xref:Microsoft.AspNetCore.Http.IFormFile>, który jest C# reprezentacją pliku używanego do przetworzenia lub zapisania pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-405">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="3ba59-406">Zasoby (dysk, pamięć) używane przez operacje przekazywania plików zależą od liczby i rozmiaru współbieżnych przekazywania plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-406">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="3ba59-407">Jeśli aplikacja próbuje buforować zbyt wiele przeciążeń, lokacja uległa awarii, gdy zabraknie pamięci lub miejsca na dysku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-407">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="3ba59-408">Jeśli rozmiar lub częstotliwość przekazywania plików powoduje wyczerpanie zasobów aplikacji, należy użyć przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="3ba59-408">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="3ba59-409">Każdy pojedynczy buforowany plik przekraczający 64 KB jest przenoszony z pamięci do pliku tymczasowego na dysku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-409">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="3ba59-410">Buforowanie małych plików zostało omówione w następujących sekcjach tego tematu:</span><span class="sxs-lookup"><span data-stu-id="3ba59-410">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="3ba59-411">Magazyn fizyczny</span><span class="sxs-lookup"><span data-stu-id="3ba59-411">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="3ba59-412">Baza danych</span><span class="sxs-lookup"><span data-stu-id="3ba59-412">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="3ba59-413">**Przesyłanie strumieniowe**</span><span class="sxs-lookup"><span data-stu-id="3ba59-413">**Streaming**</span></span>

<span data-ttu-id="3ba59-414">Plik jest odbierany od żądania wieloczęściowego i bezpośrednio przetwarzany lub zapisywany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="3ba59-414">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="3ba59-415">Przesyłanie strumieniowe nie poprawia znacząco wydajności.</span><span class="sxs-lookup"><span data-stu-id="3ba59-415">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="3ba59-416">Przesyłanie strumieniowe zmniejsza wymagania dotyczące pamięci lub miejsca na dysku podczas przekazywania plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-416">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="3ba59-417">Przesyłanie strumieniowe dużych plików jest omówione w sekcji [przekazywanie dużych plików z przesyłaniem strumieniowym](#upload-large-files-with-streaming) .</span><span class="sxs-lookup"><span data-stu-id="3ba59-417">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="3ba59-418">Przekazywanie małych plików z buforowanym powiązaniem modelu z magazynem fizycznym</span><span class="sxs-lookup"><span data-stu-id="3ba59-418">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="3ba59-419">Aby przekazać małe pliki, użyj formularza wieloczęściowego lub Skonstruuj żądanie POST przy użyciu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3ba59-419">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="3ba59-420">Poniższy przykład ilustruje użycie formularza Razor Pages do przekazywania pojedynczego pliku (*Pages/BufferedSingleFileUploadPhysical. cshtml* w przykładowej aplikacji):</span><span class="sxs-lookup"><span data-stu-id="3ba59-420">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
            <span asp-validation-for="FileUpload.FormFile"></span>
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload" />
</form>
```

<span data-ttu-id="3ba59-421">Poniższy przykład jest analogiczny do poprzedniego przykładu, z wyjątkiem tego, że:</span><span class="sxs-lookup"><span data-stu-id="3ba59-421">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="3ba59-422">Kod JavaScript ([interfejs API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API)) służy do przesyłania danych formularza.</span><span class="sxs-lookup"><span data-stu-id="3ba59-422">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="3ba59-423">Nie istnieje weryfikacja.</span><span class="sxs-lookup"><span data-stu-id="3ba59-423">There's no validation.</span></span>

```cshtml
<form action="BufferedSingleFileUploadPhysical/?handler=Upload" 
      enctype="multipart/form-data" onsubmit="AJAXSubmit(this);return false;" 
      method="post">
    <dl>
        <dt>
            <label for="FileUpload_FormFile">File</label>
        </dt>
        <dd>
            <input id="FileUpload_FormFile" type="file" 
                name="FileUpload.FormFile" />
        </dd>
    </dl>

    <input class="btn" type="submit" value="Upload" />

    <div style="margin-top:15px">
        <output name="result"></output>
    </div>
</form>

<script>
  async function AJAXSubmit (oFormElement) {
    var resultElement = oFormElement.elements.namedItem("result");
    const formData = new FormData(oFormElement);

    try {
    const response = await fetch(oFormElement.action, {
      method: 'POST',
      body: formData
    });

    if (response.ok) {
      window.location.href = '/';
    }

    resultElement.value = 'Result: ' + response.status + ' ' + 
      response.statusText;
    } catch (error) {
      console.error('Error:', error);
    }
  }
</script>
```

<span data-ttu-id="3ba59-424">Aby wykonać formularz POST w języku JavaScript dla klientów, którzy [nie obsługują interfejsu API pobierania](https://caniuse.com/#feat=fetch), należy użyć jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="3ba59-424">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="3ba59-425">Użyj wypełniania pobierania (na przykład [window. Fetch Fill (GitHub/Fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="3ba59-425">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="3ba59-426">Użyj `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="3ba59-426">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="3ba59-427">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3ba59-427">For example:</span></span>

  ```javascript
  <script>
    "use strict";

    function AJAXSubmit (oFormElement) {
      var oReq = new XMLHttpRequest();
      oReq.onload = function(e) { 
      oFormElement.elements.namedItem("result").value = 
        'Result: ' + this.status + ' ' + this.statusText;
      };
      oReq.open("post", oFormElement.action);
      oReq.send(new FormData(oFormElement));
    }
  </script>
  ```

<span data-ttu-id="3ba59-428">W celu obsługi przekazywania plików formularze HTML muszą określać typ kodowania (`enctype`) `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="3ba59-428">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="3ba59-429">Aby `files` element wejściowy obsługujący przekazywanie wielu plików, podaj atrybut `multiple` dla elementu `<input>`:</span><span class="sxs-lookup"><span data-stu-id="3ba59-429">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="3ba59-430">Do poszczególnych plików przekazanych do serwera można uzyskać dostęp za pośrednictwem [powiązania modelu](xref:mvc/models/model-binding) przy użyciu <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="3ba59-430">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="3ba59-431">Przykładowa aplikacja pokazuje wiele buforowanych operacji przekazywania plików dla scenariuszy bazy danych i magazynu fizycznego.</span><span class="sxs-lookup"><span data-stu-id="3ba59-431">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

<a name="filename2"></a>

> [!WARNING]
> <span data-ttu-id="3ba59-432">**Nie** należy używać właściwości `FileName` <xref:Microsoft.AspNetCore.Http.IFormFile> innej niż na potrzeby wyświetlania i rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-432">Do **not** use the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> other than for display and logging.</span></span> <span data-ttu-id="3ba59-433">Podczas wyświetlania lub rejestrowania, kod HTML koduje nazwę pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-433">When displaying or logging, HTML encode the file name.</span></span> <span data-ttu-id="3ba59-434">Osoba atakująca może dostarczyć złośliwą nazwę pliku, w tym pełne ścieżki lub ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="3ba59-434">An attacker can provide a malicious filename, including full paths or relative paths.</span></span> <span data-ttu-id="3ba59-435">Aplikacje powinny:</span><span class="sxs-lookup"><span data-stu-id="3ba59-435">Applications should:</span></span>
>
> * <span data-ttu-id="3ba59-436">Usuń ścieżkę z nazwy pliku dostarczonej przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3ba59-436">Remove the path from the user-supplied filename.</span></span>
> * <span data-ttu-id="3ba59-437">Zapisz plik w formacie HTML, który został usunięty z ścieżką dla interfejsu użytkownika lub rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-437">Save the HTML-encoded, path-removed filename for UI or logging.</span></span>
> * <span data-ttu-id="3ba59-438">Wygeneruj nową losową nazwę pliku dla magazynu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-438">Generate a new random filename for storage.</span></span>
>
> <span data-ttu-id="3ba59-439">Poniższy kod usuwa ścieżkę z nazwy pliku:</span><span class="sxs-lookup"><span data-stu-id="3ba59-439">The following code removes the path from the file name:</span></span>
>
> ```csharp
> string untrustedFileName = Path.GetFileName(pathName);
> ```
>
> <span data-ttu-id="3ba59-440">Przykłady udostępnione w ten sposób nie uwzględniają zagadnień związanych z bezpieczeństwem.</span><span class="sxs-lookup"><span data-stu-id="3ba59-440">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="3ba59-441">Dodatkowe informacje są dostarczane przez następujące sekcje i [Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="3ba59-441">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="3ba59-442">Zagadnienia dotyczące bezpieczeństwa</span><span class="sxs-lookup"><span data-stu-id="3ba59-442">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="3ba59-443">Walidacja</span><span class="sxs-lookup"><span data-stu-id="3ba59-443">Validation</span></span>](#validation)

<span data-ttu-id="3ba59-444">Podczas przekazywania plików przy użyciu powiązania modelu i <xref:Microsoft.AspNetCore.Http.IFormFile>, Metoda akcji może przyjmować:</span><span class="sxs-lookup"><span data-stu-id="3ba59-444">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="3ba59-445">Jeden <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="3ba59-445">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="3ba59-446">Dowolne z następujących kolekcji, które reprezentują kilka plików:</span><span class="sxs-lookup"><span data-stu-id="3ba59-446">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="3ba59-447">\<<xref:Microsoft.AspNetCore.Http.IFormFile>[listy](xref:System.Collections.Generic.List`1) ></span><span class="sxs-lookup"><span data-stu-id="3ba59-447">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="3ba59-448">Powiązanie dopasowuje pliki formularza według nazwy.</span><span class="sxs-lookup"><span data-stu-id="3ba59-448">Binding matches form files by name.</span></span> <span data-ttu-id="3ba59-449">Na przykład wartość `name` HTML w `<input type="file" name="formFile">` musi być zgodna z C# wartością parametru/właściwością (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="3ba59-449">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="3ba59-450">Aby uzyskać więcej informacji, zobacz sekcję [dopasowanie wartości atrybutu do nazwy parametru w sekcji Metoda post](#match-name-attribute-value-to-parameter-name-of-post-method) .</span><span class="sxs-lookup"><span data-stu-id="3ba59-450">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="3ba59-451">Poniższy przykład:</span><span class="sxs-lookup"><span data-stu-id="3ba59-451">The following example:</span></span>

* <span data-ttu-id="3ba59-452">Pętle przez co najmniej jeden przekazany plik.</span><span class="sxs-lookup"><span data-stu-id="3ba59-452">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="3ba59-453">Używa funkcji [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) , aby zwrócić pełną ścieżkę do pliku, łącznie z nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-453">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="3ba59-454">Zapisuje pliki w lokalnym systemie plików przy użyciu nazwy pliku wygenerowanej przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="3ba59-454">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="3ba59-455">Zwraca łączną liczbę i rozmiar przekazanych plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-455">Returns the total number and size of files uploaded.</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> files)
{
    long size = files.Sum(f => f.Length);

    foreach (var formFile in files)
    {
        if (formFile.Length > 0)
        {
            var filePath = Path.GetTempFileName();

            using (var stream = System.IO.File.Create(filePath))
            {
                await formFile.CopyToAsync(stream);
            }
        }
    }

    // Process uploaded files
    // Don't rely on or trust the FileName property without validation.

    return Ok(new { count = files.Count, size, filePath });
}
```

<span data-ttu-id="3ba59-456">Użyj `Path.GetRandomFileName` do wygenerowania nazwy pliku bez ścieżki.</span><span class="sxs-lookup"><span data-stu-id="3ba59-456">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="3ba59-457">W poniższym przykładzie ścieżka jest uzyskiwana z konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="3ba59-457">In the following example, the path is obtained from configuration:</span></span>

```csharp
foreach (var formFile in files)
{
    if (formFile.Length > 0)
    {
        var filePath = Path.Combine(_config["StoredFilesPath"], 
            Path.GetRandomFileName());

        using (var stream = System.IO.File.Create(filePath))
        {
            await formFile.CopyToAsync(stream);
        }
    }
}
```

<span data-ttu-id="3ba59-458">Ścieżka przeniesiona do <xref:System.IO.FileStream> *musi* zawierać nazwę pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-458">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="3ba59-459">Jeśli nie podano nazwy pliku, <xref:System.UnauthorizedAccessException> jest generowany w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-459">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="3ba59-460">Pliki przekazane przy użyciu techniki <xref:Microsoft.AspNetCore.Http.IFormFile> są buforowane w pamięci lub na dysku na serwerze przed przetworzeniem.</span><span class="sxs-lookup"><span data-stu-id="3ba59-460">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="3ba59-461">Wewnątrz metody akcji <xref:Microsoft.AspNetCore.Http.IFormFile> zawartość jest dostępna jako <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="3ba59-461">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="3ba59-462">Oprócz lokalnego systemu plików można zapisywać pliki w udziale sieciowym lub w usłudze magazynu plików, na przykład w [usłudze Azure Blob Storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="3ba59-462">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="3ba59-463">Aby uzyskać inny przykład pętli dla wielu plików na potrzeby przekazywania i używania bezpiecznych nazw plików, zobacz *Pages/BufferedMultipleFileUploadPhysical. cshtml. cs* w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba59-463">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="3ba59-464">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) zgłasza <xref:System.IO.IOException>, jeśli utworzono więcej niż 65 535 plików bez usuwania poprzednich plików tymczasowych.</span><span class="sxs-lookup"><span data-stu-id="3ba59-464">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="3ba59-465">Limit 65 535 plików jest limitem dla serwera.</span><span class="sxs-lookup"><span data-stu-id="3ba59-465">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="3ba59-466">Aby uzyskać więcej informacji na temat tego limitu dla systemu operacyjnego Windows, zobacz uwagi w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="3ba59-466">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="3ba59-467">GetTempFileNameA, funkcja</span><span class="sxs-lookup"><span data-stu-id="3ba59-467">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="3ba59-468">Przekazywanie małych plików z buforowanym powiązaniem modelu z bazą danych</span><span class="sxs-lookup"><span data-stu-id="3ba59-468">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="3ba59-469">Aby przechowywać dane binarne pliku w bazie danych przy użyciu [Entity Framework](/ef/core/index), zdefiniuj Właściwość <xref:System.Byte> tablicy dla jednostki:</span><span class="sxs-lookup"><span data-stu-id="3ba59-469">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="3ba59-470">Określ właściwość modelu strony dla klasy, która zawiera <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="3ba59-470">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

```csharp
public class BufferedSingleFileUploadDbModel : PageModel
{
    ...

    [BindProperty]
    public BufferedSingleFileUploadDb FileUpload { get; set; }

    ...
}

public class BufferedSingleFileUploadDb
{
    [Required]
    [Display(Name="File")]
    public IFormFile FormFile { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="3ba59-471"><xref:Microsoft.AspNetCore.Http.IFormFile> można używać bezpośrednio jako parametru metody akcji lub jako powiązanej właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-471"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="3ba59-472">W poprzednim przykładzie użyto powiązanej właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-472">The prior example uses a bound model property.</span></span>

<span data-ttu-id="3ba59-473">`FileUpload` jest używana w formularzu Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="3ba59-473">The `FileUpload` is used in the Razor Pages form:</span></span>

```cshtml
<form enctype="multipart/form-data" method="post">
    <dl>
        <dt>
            <label asp-for="FileUpload.FormFile"></label>
        </dt>
        <dd>
            <input asp-for="FileUpload.FormFile" type="file">
        </dd>
    </dl>
    <input asp-page-handler="Upload" class="btn" type="submit" value="Upload">
</form>
```

<span data-ttu-id="3ba59-474">Po opublikowaniu formularza na serwerze Skopiuj <xref:Microsoft.AspNetCore.Http.IFormFile> do strumienia i Zapisz go jako tablicę bajtową w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="3ba59-474">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="3ba59-475">W poniższym przykładzie `_dbContext` przechowuje kontekst bazy danych aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3ba59-475">In the following example, `_dbContext` stores the app's database context:</span></span>

```csharp
public async Task<IActionResult> OnPostUploadAsync()
{
    using (var memoryStream = new MemoryStream())
    {
        await FileUpload.FormFile.CopyToAsync(memoryStream);

        // Upload the file if less than 2 MB
        if (memoryStream.Length < 2097152)
        {
            var file = new AppFile()
            {
                Content = memoryStream.ToArray()
            };

            _dbContext.File.Add(file);

            await _dbContext.SaveChangesAsync();
        }
        else
        {
            ModelState.AddModelError("File", "The file is too large.");
        }
    }

    return Page();
}
```

<span data-ttu-id="3ba59-476">Poprzedni przykład przypomina scenariusz przedstawiony w przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="3ba59-476">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="3ba59-477">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="3ba59-477">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="3ba59-478">*Strony/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="3ba59-478">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="3ba59-479">Należy zachować ostrożność podczas przechowywania danych binarnych w relacyjnych bazach danych, ponieważ może to mieć negatywny wpływ na wydajność.</span><span class="sxs-lookup"><span data-stu-id="3ba59-479">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="3ba59-480">Nie używaj ani nie ufaj właściwości `FileName` <xref:Microsoft.AspNetCore.Http.IFormFile> bez weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba59-480">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="3ba59-481">Właściwość `FileName` powinna być używana tylko do celów wyświetlania i tylko po kodowaniu kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="3ba59-481">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="3ba59-482">Podane przykłady nie uwzględniają zagadnień związanych z zabezpieczeniami.</span><span class="sxs-lookup"><span data-stu-id="3ba59-482">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="3ba59-483">Dodatkowe informacje są dostarczane przez następujące sekcje i [Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="3ba59-483">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="3ba59-484">Zagadnienia dotyczące bezpieczeństwa</span><span class="sxs-lookup"><span data-stu-id="3ba59-484">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="3ba59-485">Walidacja</span><span class="sxs-lookup"><span data-stu-id="3ba59-485">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="3ba59-486">Przekazywanie dużych plików strumieniowo</span><span class="sxs-lookup"><span data-stu-id="3ba59-486">Upload large files with streaming</span></span>

<span data-ttu-id="3ba59-487">Poniższy przykład ilustruje sposób użycia języka JavaScript do przesyłania strumieniowego pliku do akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3ba59-487">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="3ba59-488">Token antysfałszowany pliku jest generowany przy użyciu niestandardowego atrybutu filtru i przekazywać do nagłówków HTTP klienta zamiast w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-488">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="3ba59-489">Ponieważ metoda akcji przetwarza przekazane dane bezpośrednio, powiązanie modelu formularza jest wyłączone przez inny filtr niestandardowy.</span><span class="sxs-lookup"><span data-stu-id="3ba59-489">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="3ba59-490">W ramach akcji zawartość formularza jest odczytywana przy użyciu `MultipartReader`, który odczytuje poszczególne `MultipartSection`, przetwarza plik lub przechowując zawartość odpowiednio do potrzeb.</span><span class="sxs-lookup"><span data-stu-id="3ba59-490">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="3ba59-491">Po odczytaniu sekcji wieloczęściowej akcja wykonuje własne powiązanie modelu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-491">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="3ba59-492">Początkowa odpowiedź strony ładuje formularz i zapisuje w pliku cookie token antysfałszowany (za pośrednictwem atrybutu `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="3ba59-492">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="3ba59-493">Ten atrybut używa wbudowanej [obsługi przed fałszowaniem](xref:security/anti-request-forgery) ASP.NET Core, aby ustawić plik cookie z tokenem żądania:</span><span class="sxs-lookup"><span data-stu-id="3ba59-493">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="3ba59-494">`DisableFormValueModelBindingAttribute` jest używany do wyłączania powiązania modelu:</span><span class="sxs-lookup"><span data-stu-id="3ba59-494">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="3ba59-495">W przykładowej aplikacji `GenerateAntiforgeryTokenCookieAttribute` i `DisableFormValueModelBindingAttribute` są stosowane jako filtry do modeli aplikacji strony `/StreamedSingleFileUploadDb` i `/StreamedSingleFileUploadPhysical` w `Startup.ConfigureServices` przy użyciu [konwencji Razor Pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="3ba59-495">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="3ba59-496">Ponieważ powiązanie modelu nie odczytuje formularza, parametry, które są powiązane z formularza nie są powiązane (zapytania, trasy i nagłówki nadal pracują).</span><span class="sxs-lookup"><span data-stu-id="3ba59-496">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="3ba59-497">Metoda akcji działa bezpośrednio z właściwością `Request`.</span><span class="sxs-lookup"><span data-stu-id="3ba59-497">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="3ba59-498">`MultipartReader` jest używany do odczytywania każdej sekcji.</span><span class="sxs-lookup"><span data-stu-id="3ba59-498">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="3ba59-499">Dane klucza/wartości są przechowywane w `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="3ba59-499">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="3ba59-500">Po odczytaniu sekcji wieloczęściowych zawartość `KeyValueAccumulator` są używane do powiązania danych formularza z typem modelu.</span><span class="sxs-lookup"><span data-stu-id="3ba59-500">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="3ba59-501">Pełna `StreamingController.UploadDatabase` Metoda przesyłania strumieniowego do bazy danych z EF Core:</span><span class="sxs-lookup"><span data-stu-id="3ba59-501">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="3ba59-502">`MultipartRequestHelper` (*Narzędzia/MultipartRequestHelper. cs*):</span><span class="sxs-lookup"><span data-stu-id="3ba59-502">`MultipartRequestHelper` (*Utilities/MultipartRequestHelper.cs*):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Utilities/MultipartRequestHelper.cs)]

<span data-ttu-id="3ba59-503">Pełna `StreamingController.UploadPhysical` Metoda przesyłania strumieniowego do lokalizacji fizycznej:</span><span class="sxs-lookup"><span data-stu-id="3ba59-503">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="3ba59-504">W przykładowej aplikacji sprawdzanie poprawności jest obsługiwane przez `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="3ba59-504">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="3ba59-505">Walidacja</span><span class="sxs-lookup"><span data-stu-id="3ba59-505">Validation</span></span>

<span data-ttu-id="3ba59-506">Klasa `FileHelpers` aplikacji przykładowej pokazuje kilka testów dla buforowanych <xref:Microsoft.AspNetCore.Http.IFormFile> i przesyłania strumieniowego plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-506">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="3ba59-507">Aby przetwarzać <xref:Microsoft.AspNetCore.Http.IFormFile> buforowane operacje przekazywania plików w aplikacji przykładowej, zobacz `ProcessFormFile` metody w pliku *Utilities/FileHelpers. cs* .</span><span class="sxs-lookup"><span data-stu-id="3ba59-507">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="3ba59-508">Aby przetwarzać pliki przesyłane strumieniowo, zobacz metodę `ProcessStreamedFile` w tym samym pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-508">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="3ba59-509">Metody przetwarzania walidacji przedstawione w przykładowej aplikacji nie skanują zawartości przekazanych plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-509">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="3ba59-510">W większości scenariuszy produkcyjnych do pliku jest używany interfejs API skanera wirusów/złośliwego oprogramowania przed udostępnieniem go użytkownikom lub innym systemom.</span><span class="sxs-lookup"><span data-stu-id="3ba59-510">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="3ba59-511">Chociaż przykład tematu zawiera przykładowy technikę sprawdzania poprawności, nie należy implementować klasy `FileHelpers` w aplikacji produkcyjnej, chyba że:</span><span class="sxs-lookup"><span data-stu-id="3ba59-511">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="3ba59-512">W pełni zapoznaj się z implementacją.</span><span class="sxs-lookup"><span data-stu-id="3ba59-512">Fully understand the implementation.</span></span>
> * <span data-ttu-id="3ba59-513">Zmodyfikuj implementację odpowiednio do środowiska i specyfikacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba59-513">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="3ba59-514">**Nigdy nie należy wdrażać kodu zabezpieczeń w aplikacji bez konieczności ich rozwiązywania.**</span><span class="sxs-lookup"><span data-stu-id="3ba59-514">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="3ba59-515">Weryfikacja zawartości</span><span class="sxs-lookup"><span data-stu-id="3ba59-515">Content validation</span></span>

<span data-ttu-id="3ba59-516">**Użyj interfejsu API skanowania wirusa/złośliwego oprogramowania innej firmy dla przekazanej zawartości.**</span><span class="sxs-lookup"><span data-stu-id="3ba59-516">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="3ba59-517">Skanowanie plików wymaga użycia zasobów serwera w scenariuszach o dużych ilościach.</span><span class="sxs-lookup"><span data-stu-id="3ba59-517">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="3ba59-518">Jeśli wydajność przetwarzania żądań jest zmniejszana ze względu na skanowanie plików, rozważ odciążenie pracy skanowania do [usługi w tle](xref:fundamentals/host/hosted-services), prawdopodobnie usługi uruchomionej na serwerze innym niż serwer aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba59-518">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="3ba59-519">Zwykle przekazane pliki są przechowywane w obszarze poddany kwarantannie, dopóki skaner wirusów w tle nie sprawdzi ich.</span><span class="sxs-lookup"><span data-stu-id="3ba59-519">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="3ba59-520">Gdy plik kończy się, plik zostanie przeniesiony do normalnej lokalizacji przechowywania plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-520">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="3ba59-521">Te kroki są zwykle wykonywane w połączeniu z rekordem bazy danych, który wskazuje na stan skanowania pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-521">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="3ba59-522">Korzystając z takiego podejścia, aplikacja i serwer aplikacji pozostają skoncentrowane na odpowiedzi na żądania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-522">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="3ba59-523">Weryfikacja rozszerzenia pliku</span><span class="sxs-lookup"><span data-stu-id="3ba59-523">File extension validation</span></span>

<span data-ttu-id="3ba59-524">Rozszerzenie przekazanego pliku powinno być sprawdzane względem listy dozwolonych rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="3ba59-524">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="3ba59-525">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3ba59-525">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="3ba59-526">Walidacja podpisu pliku</span><span class="sxs-lookup"><span data-stu-id="3ba59-526">File signature validation</span></span>

<span data-ttu-id="3ba59-527">Sygnatura pliku jest określana przez pierwsze kilka bajtów na początku pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-527">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="3ba59-528">Te bajty mogą być używane do wskazywania, czy rozszerzenie jest zgodne z zawartością pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-528">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="3ba59-529">Przykładowa aplikacja sprawdza podpisy plików dla kilku popularnych typów plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-529">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="3ba59-530">W poniższym przykładzie podpis pliku dla obrazu JPEG jest sprawdzany pod kątem pliku:</span><span class="sxs-lookup"><span data-stu-id="3ba59-530">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

```csharp
private static readonly Dictionary<string, List<byte[]>> _fileSignature = 
    new Dictionary<string, List<byte[]>>
{
    { ".jpeg", new List<byte[]>
        {
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE0 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE2 },
            new byte[] { 0xFF, 0xD8, 0xFF, 0xE3 },
        }
    },
};

using (var reader = new BinaryReader(uploadedFileData))
{
    var signatures = _fileSignature[ext];
    var headerBytes = reader.ReadBytes(signatures.Max(m => m.Length));
    
    return signatures.Any(signature => 
        headerBytes.Take(signature.Length).SequenceEqual(signature));
}
```

<span data-ttu-id="3ba59-531">Aby uzyskać dodatkowe podpisy plików, zapoznaj się z [bazami danych sygnatury plików](https://www.filesignatures.net/) i oficjalnymi specyfikacjami plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-531">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="3ba59-532">Zabezpieczenia nazw plików</span><span class="sxs-lookup"><span data-stu-id="3ba59-532">File name security</span></span>

<span data-ttu-id="3ba59-533">Nigdy nie należy używać nazwy pliku dostarczonej przez klienta do zapisywania plików w magazynie fizycznym.</span><span class="sxs-lookup"><span data-stu-id="3ba59-533">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="3ba59-534">Utwórz bezpieczną nazwę pliku dla pliku przy użyciu [ścieżki. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) lub [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) , aby utworzyć pełną ścieżkę (łącznie z nazwą pliku) dla magazynu tymczasowego.</span><span class="sxs-lookup"><span data-stu-id="3ba59-534">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="3ba59-535">KOD Razor automatycznie koduje wartości właściwości dla wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-535">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="3ba59-536">Następujący kod jest bezpieczny w użyciu:</span><span class="sxs-lookup"><span data-stu-id="3ba59-536">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="3ba59-537">Poza Razor, zawsze <xref:System.Net.WebUtility.HtmlEncode*> zawartość nazwy pliku z żądania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3ba59-537">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="3ba59-538">Wiele implementacji musi zawierać sprawdzenie, czy plik istnieje; w przeciwnym razie plik zostanie zastąpiony przez plik o tej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="3ba59-538">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="3ba59-539">Podaj dodatkową logikę, aby spełnić wymagania dotyczące Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3ba59-539">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="3ba59-540">Sprawdzanie poprawności rozmiaru</span><span class="sxs-lookup"><span data-stu-id="3ba59-540">Size validation</span></span>

<span data-ttu-id="3ba59-541">Ogranicz rozmiar przekazanych plików.</span><span class="sxs-lookup"><span data-stu-id="3ba59-541">Limit the size of uploaded files.</span></span>

<span data-ttu-id="3ba59-542">W przykładowej aplikacji rozmiar pliku jest ograniczony do 2 MB (wyrażony w bajtach).</span><span class="sxs-lookup"><span data-stu-id="3ba59-542">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="3ba59-543">Limit jest dostarczany przez [konfigurację](xref:fundamentals/configuration/index) z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="3ba59-543">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="3ba59-544">`FileSizeLimit` jest wstrzykiwana do klas `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="3ba59-544">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

```csharp
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    private readonly long _fileSizeLimit;

    public BufferedSingleFileUploadPhysicalModel(IConfiguration config)
    {
        _fileSizeLimit = config.GetValue<long>("FileSizeLimit");
    }

    ...
}
```

<span data-ttu-id="3ba59-545">Gdy rozmiar pliku przekracza limit, plik zostanie odrzucony:</span><span class="sxs-lookup"><span data-stu-id="3ba59-545">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="3ba59-546">Dopasuj wartość atrybutu Name do nazwy parametru metody POST</span><span class="sxs-lookup"><span data-stu-id="3ba59-546">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="3ba59-547">W formularzach innych niż Razor, które PUBLIKUJą dane formularza lub używają `FormData` języka JavaScript bezpośrednio, nazwa określona w elemencie formularza lub `FormData` musi być zgodna z nazwą parametru w akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3ba59-547">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="3ba59-548">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="3ba59-548">In the following example:</span></span>

* <span data-ttu-id="3ba59-549">W przypadku używania elementu `<input>` atrybut `name` ma ustawioną wartość `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="3ba59-549">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="3ba59-550">W przypadku używania `FormData` w języku JavaScript nazwa jest ustawiana na wartość `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="3ba59-550">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="3ba59-551">Użyj zgodnej nazwy dla parametru C# metody (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="3ba59-551">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="3ba59-552">Dla metody obsługi strony Razor Pages o nazwie `Upload`:</span><span class="sxs-lookup"><span data-stu-id="3ba59-552">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="3ba59-553">Dla metody akcji składnika MVC Controller:</span><span class="sxs-lookup"><span data-stu-id="3ba59-553">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="3ba59-554">Konfiguracja serwera i aplikacji</span><span class="sxs-lookup"><span data-stu-id="3ba59-554">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="3ba59-555">Limit długości treści wieloczęściowej</span><span class="sxs-lookup"><span data-stu-id="3ba59-555">Multipart body length limit</span></span>

<span data-ttu-id="3ba59-556"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> ustawia limit długości każdej wieloczęściowej treści.</span><span class="sxs-lookup"><span data-stu-id="3ba59-556"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="3ba59-557">Sekcje formularzy, które przekraczają ten limit, generują <xref:System.IO.InvalidDataException> podczas analizowania.</span><span class="sxs-lookup"><span data-stu-id="3ba59-557">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="3ba59-558">Wartość domyślna to 134 217 728 (128 MB).</span><span class="sxs-lookup"><span data-stu-id="3ba59-558">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="3ba59-559">Dostosuj limit przy użyciu ustawienia <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3ba59-559">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<span data-ttu-id="3ba59-560"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> służy do ustawiania <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> dla pojedynczej strony lub akcji.</span><span class="sxs-lookup"><span data-stu-id="3ba59-560"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="3ba59-561">W aplikacji Razor Pages Zastosuj filtr z [Konwencją](xref:razor-pages/razor-pages-conventions) w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3ba59-561">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model.Filters.Add(
                    new RequestFormLimitsAttribute()
                    {
                        // Set the limit to 256 MB
                        MultipartBodyLengthLimit = 268435456
                    });
    })
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="3ba59-562">W aplikacji Razor Pages lub aplikacji MVC Zastosuj filtr do modelu strony lub metody akcji:</span><span class="sxs-lookup"><span data-stu-id="3ba59-562">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="3ba59-563">Maksymalny rozmiar treści żądania Kestrel</span><span class="sxs-lookup"><span data-stu-id="3ba59-563">Kestrel maximum request body size</span></span>

<span data-ttu-id="3ba59-564">W przypadku aplikacji hostowanych przez Kestrel domyślny maksymalny rozmiar treści żądania to 30 000 000 bajtów, czyli około 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="3ba59-564">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="3ba59-565">Dostosuj limit przy użyciu opcji serwera [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel:</span><span class="sxs-lookup"><span data-stu-id="3ba59-565">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        });
```

<span data-ttu-id="3ba59-566"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> służy do ustawiania [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) dla pojedynczej strony lub akcji.</span><span class="sxs-lookup"><span data-stu-id="3ba59-566"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="3ba59-567">W aplikacji Razor Pages Zastosuj filtr z [Konwencją](xref:razor-pages/razor-pages-conventions) w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3ba59-567">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.Conventions
            .AddPageApplicationModelConvention("/FileUploadPage",
                model =>
                {
                    // Handle requests up to 50 MB
                    model.Filters.Add(
                        new RequestSizeLimitAttribute(52428800));
                });
    })
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
```

<span data-ttu-id="3ba59-568">W aplikacji strony Razor lub aplikacji MVC Zastosuj filtr do klasy procedury obsługi stron lub metody akcji:</span><span class="sxs-lookup"><span data-stu-id="3ba59-568">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="3ba59-569">Inne limity Kestrel</span><span class="sxs-lookup"><span data-stu-id="3ba59-569">Other Kestrel limits</span></span>

<span data-ttu-id="3ba59-570">Inne limity Kestrel mogą dotyczyć aplikacji hostowanych przez Kestrel:</span><span class="sxs-lookup"><span data-stu-id="3ba59-570">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="3ba59-571">Maksymalna liczba połączeń klienta</span><span class="sxs-lookup"><span data-stu-id="3ba59-571">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="3ba59-572">Stawki danych żądań i odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="3ba59-572">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="3ba59-573">Limit długości zawartości usług IIS</span><span class="sxs-lookup"><span data-stu-id="3ba59-573">IIS content length limit</span></span>

<span data-ttu-id="3ba59-574">Domyślny limit żądań (`maxAllowedContentLength`) to 30 000 000 bajtów, czyli około 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="3ba59-574">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="3ba59-575">Dostosuj limit w pliku *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="3ba59-575">Customize the limit in the *web.config* file:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- Handle requests up to 50 MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="3ba59-576">To ustawienie dotyczy tylko usług IIS.</span><span class="sxs-lookup"><span data-stu-id="3ba59-576">This setting only applies to IIS.</span></span> <span data-ttu-id="3ba59-577">Zachowanie nie występuje domyślnie podczas hostowania w Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3ba59-577">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="3ba59-578">Aby uzyskać więcej informacji, zobacz [limity żądań \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="3ba59-578">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="3ba59-579">Ograniczenia w module ASP.NET Core lub obecność modułu filtrowania żądań usług IIS mogą ograniczyć przekazywanie do 2 lub 4 GB.</span><span class="sxs-lookup"><span data-stu-id="3ba59-579">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="3ba59-580">Aby uzyskać więcej informacji, zobacz [nie można przekazać pliku o rozmiarze większym niż 2 GB (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="3ba59-580">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="3ba59-581">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="3ba59-581">Troubleshoot</span></span>

<span data-ttu-id="3ba59-582">Poniżej przedstawiono niektóre typowe problemy, które można napotkać podczas pracy z przekazywaniem plików i ich możliwymi rozwiązaniami.</span><span class="sxs-lookup"><span data-stu-id="3ba59-582">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="3ba59-583">Nie znaleziono błędu w przypadku wdrożenia na serwerze usług IIS</span><span class="sxs-lookup"><span data-stu-id="3ba59-583">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="3ba59-584">Następujący błąd wskazuje, że przekazany plik przekracza skonfigurowaną długość zawartości serwera:</span><span class="sxs-lookup"><span data-stu-id="3ba59-584">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="3ba59-585">Aby uzyskać więcej informacji na temat zwiększania limitu, zobacz sekcję [limit długości zawartości usług IIS](#iis-content-length-limit) .</span><span class="sxs-lookup"><span data-stu-id="3ba59-585">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="3ba59-586">Błąd połączenia</span><span class="sxs-lookup"><span data-stu-id="3ba59-586">Connection failure</span></span>

<span data-ttu-id="3ba59-587">Wystąpił błąd połączenia i połączenie z serwerem resetowania prawdopodobnie wskazuje, że przekazany plik przekracza maksymalny rozmiar treści żądania Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3ba59-587">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="3ba59-588">Aby uzyskać więcej informacji, zobacz sekcję [Maksymalny rozmiar treści żądania Kestrel](#kestrel-maximum-request-body-size) .</span><span class="sxs-lookup"><span data-stu-id="3ba59-588">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="3ba59-589">Limity połączeń klienta Kestrel mogą również wymagać korekty.</span><span class="sxs-lookup"><span data-stu-id="3ba59-589">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="3ba59-590">Wyjątek odwołania o wartości null z IFormFile</span><span class="sxs-lookup"><span data-stu-id="3ba59-590">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="3ba59-591">Jeśli kontroler akceptuje przekazane pliki przy użyciu <xref:Microsoft.AspNetCore.Http.IFormFile> ale wartość jest `null`, upewnij się, że w formularzu HTML określono wartość `enctype` `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="3ba59-591">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="3ba59-592">Jeśli ten atrybut nie jest ustawiony dla elementu `<form>`, przekazywanie pliku nie wystąpi i wszystkie powiązane argumenty <xref:Microsoft.AspNetCore.Http.IFormFile> są `null`.</span><span class="sxs-lookup"><span data-stu-id="3ba59-592">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="3ba59-593">Sprawdź również, czy [Nazwa przekazywania w postaci danych jest zgodna z nazewnictwem aplikacji](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="3ba59-593">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

### <a name="stream-was-too-long"></a><span data-ttu-id="3ba59-594">Strumień był zbyt długi</span><span class="sxs-lookup"><span data-stu-id="3ba59-594">Stream was too long</span></span>

<span data-ttu-id="3ba59-595">Przykłady w tym temacie polegają na <xref:System.IO.MemoryStream> do przechowywania zawartości przekazanego pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-595">The examples in this topic rely upon <xref:System.IO.MemoryStream> to hold the uploaded file's content.</span></span> <span data-ttu-id="3ba59-596">Limit rozmiaru `MemoryStream` jest `int.MaxValue`.</span><span class="sxs-lookup"><span data-stu-id="3ba59-596">The size limit of a `MemoryStream` is `int.MaxValue`.</span></span> <span data-ttu-id="3ba59-597">Jeśli scenariusz przekazywania plików aplikacji wymaga przechowywania zawartości pliku o rozmiarze większym niż 50 MB, użyj alternatywnego podejścia, które nie polega na pojedynczym `MemoryStream` do przechowywania zawartości przekazanego pliku.</span><span class="sxs-lookup"><span data-stu-id="3ba59-597">If the app's file upload scenario requires holding file content larger than 50 MB, use an alternative approach that doesn't rely upon a single `MemoryStream` for holding an uploaded file's content.</span></span>

::: moniker-end


## <a name="additional-resources"></a><span data-ttu-id="3ba59-598">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3ba59-598">Additional resources</span></span>

* [<span data-ttu-id="3ba59-599">Przekazywanie pliku bez ograniczeń</span><span class="sxs-lookup"><span data-stu-id="3ba59-599">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
* [<span data-ttu-id="3ba59-600">Zabezpieczenia platformy Azure: ramka zabezpieczeń: sprawdzanie poprawności danych wejściowych | Środki zaradcze</span><span class="sxs-lookup"><span data-stu-id="3ba59-600">Azure Security: Security Frame: Input Validation | Mitigations</span></span>](/azure/security/azure-security-threat-modeling-tool-input-validation)
* [<span data-ttu-id="3ba59-601">Wzorce projektowe chmury platformy Azure: wzorzec klucza portiera</span><span class="sxs-lookup"><span data-stu-id="3ba59-601">Azure Cloud Design Patterns: Valet Key pattern</span></span>](/azure/architecture/patterns/valet-key)
