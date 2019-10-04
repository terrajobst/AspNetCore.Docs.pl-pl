---
title: Przekaż pliki w ASP.NET Core
author: guardrex
description: Jak używać powiązania modelu i przesyłania strumieniowego do przekazywania plików w ASP.NET Core MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/30/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: 46cdb517742431a7f2a93155564832d586233e68
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925116"
---
# <a name="upload-files-in-aspnet-core"></a><span data-ttu-id="f9027-103">Przekaż pliki w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f9027-103">Upload files in ASP.NET Core</span></span>

<span data-ttu-id="f9027-104">[Luke Latham](https://github.com/guardrex), [Steve Kowalski](https://ardalis.com/)i [Rutger burza](https://github.com/rutix)</span><span class="sxs-lookup"><span data-stu-id="f9027-104">By [Luke Latham](https://github.com/guardrex), [Steve Smith](https://ardalis.com/), and [Rutger Storm](https://github.com/rutix)</span></span>

<span data-ttu-id="f9027-105">ASP.NET Core obsługuje przekazywanie co najmniej jednego pliku przy użyciu powiązania z buforowanym modelem dla mniejszych plików i przesyłania strumieniowego z buforem dla większych plików.</span><span class="sxs-lookup"><span data-stu-id="f9027-105">ASP.NET Core supports uploading one or more files using buffered model binding for smaller files and unbuffered streaming for larger files.</span></span>

<span data-ttu-id="f9027-106">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([sposobu pobierania](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f9027-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="security-considerations"></a><span data-ttu-id="f9027-107">Zagadnienia dotyczące bezpieczeństwa</span><span class="sxs-lookup"><span data-stu-id="f9027-107">Security considerations</span></span>

<span data-ttu-id="f9027-108">Należy zachować ostrożność, zapewniając użytkownikom możliwość przekazywania plików na serwer.</span><span class="sxs-lookup"><span data-stu-id="f9027-108">Use caution when providing users with the ability to upload files to a server.</span></span> <span data-ttu-id="f9027-109">Osoby atakujące mogą próbować:</span><span class="sxs-lookup"><span data-stu-id="f9027-109">Attackers may attempt to:</span></span>

* <span data-ttu-id="f9027-110">Wykonywanie ataków [typu "odmowa usługi"](/windows-hardware/drivers/ifs/denial-of-service) .</span><span class="sxs-lookup"><span data-stu-id="f9027-110">Execute [denial of service](/windows-hardware/drivers/ifs/denial-of-service) attacks.</span></span>
* <span data-ttu-id="f9027-111">Przekazuj wirusy lub złośliwe oprogramowanie.</span><span class="sxs-lookup"><span data-stu-id="f9027-111">Upload viruses or malware.</span></span>
* <span data-ttu-id="f9027-112">Naruszyć bezpieczeństwo sieci i serwerów w inny sposób.</span><span class="sxs-lookup"><span data-stu-id="f9027-112">Compromise networks and servers in other ways.</span></span>

<span data-ttu-id="f9027-113">Kroki zabezpieczeń, które zmniejszają prawdopodobieństwo pomyślnego ataku:</span><span class="sxs-lookup"><span data-stu-id="f9027-113">Security steps that reduce the likelihood of a successful attack are:</span></span>

* <span data-ttu-id="f9027-114">Przekaż pliki do dedykowanego obszaru przekazywania plików w systemie, najlepiej do dysku niesystemowego.</span><span class="sxs-lookup"><span data-stu-id="f9027-114">Upload files to a dedicated file upload area on the system, preferably to a non-system drive.</span></span> <span data-ttu-id="f9027-115">Użycie dedykowanej lokalizacji ułatwia nakładanie ograniczeń zabezpieczeń na przekazane pliki.</span><span class="sxs-lookup"><span data-stu-id="f9027-115">Using a dedicated location makes it easier to impose security restrictions on uploaded files.</span></span> <span data-ttu-id="f9027-116">Wyłącz uprawnienia do wykonywania w lokalizacji przekazywania pliku. &dagger;</span><span class="sxs-lookup"><span data-stu-id="f9027-116">Disable execute permissions on the file upload location.&dagger;</span></span>
* <span data-ttu-id="f9027-117">Nigdy nie Utrwalaj przekazanych plików w tym samym drzewie katalogów co aplikacja. &dagger;</span><span class="sxs-lookup"><span data-stu-id="f9027-117">Never persist uploaded files in the same directory tree as the app.&dagger;</span></span>
* <span data-ttu-id="f9027-118">Użyj bezpiecznej nazwy pliku, która jest określana przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="f9027-118">Use a safe file name determined by the app.</span></span> <span data-ttu-id="f9027-119">Nie należy używać nazwy pliku dostarczonej przez użytkownika lub niezaufanej nazwy pliku przekazanego pliku. &dagger; Aby wyświetlić niezaufaną nazwę pliku w interfejsie użytkownika lub w wiadomości rejestrowania, w kodzie HTML należy zakodować wartość.</span><span class="sxs-lookup"><span data-stu-id="f9027-119">Don't use a file name provided by the user or the untrusted file name of the uploaded file.&dagger; To display an untrusted file name in a UI or in a logging message, HTML-encode the value.</span></span>
* <span data-ttu-id="f9027-120">Zezwala tylko na określony zestaw zatwierdzonych rozszerzeń plików. &dagger;</span><span class="sxs-lookup"><span data-stu-id="f9027-120">Only allow a specific set of approved file extensions.&dagger;</span></span>
* <span data-ttu-id="f9027-121">Sprawdź podpis formatu pliku, aby uniemożliwić użytkownikowi przekazywanie zamaskowanego pliku. &dagger; Na przykład nie Zezwalaj użytkownikowi na przekazywanie pliku *exe* z rozszerzeniem *. txt* .</span><span class="sxs-lookup"><span data-stu-id="f9027-121">Check the file format signature to prevent a user from uploading a masqueraded file.&dagger; For example, don't permit a user to upload an *.exe* file with a *.txt* extension.</span></span>
* <span data-ttu-id="f9027-122">Sprawdź, czy na serwerze są również wykonywane sprawdzenia po stronie klienta. &dagger; Sprawdzanie klienta są łatwe do obejścia.</span><span class="sxs-lookup"><span data-stu-id="f9027-122">Verify that client-side checks are also performed on the server.&dagger; Client-side checks are easy to circumvent.</span></span>
* <span data-ttu-id="f9027-123">Sprawdź rozmiar przekazanego pliku i Zapobiegaj przeciążom, które są większe niż oczekiwano. &dagger;</span><span class="sxs-lookup"><span data-stu-id="f9027-123">Check the size of an uploaded file and prevent uploads that are larger than expected.&dagger;</span></span>
* <span data-ttu-id="f9027-124">Jeśli pliki nie powinny być zastąpione przez przekazany plik o tej samej nazwie, przed przekazaniem pliku Sprawdź nazwę pliku względem bazy danych lub magazynu fizycznego.</span><span class="sxs-lookup"><span data-stu-id="f9027-124">When files shouldn't be overwritten by an uploaded file with the same name, check the file name against the database or physical storage before uploading the file.</span></span>
* <span data-ttu-id="f9027-125">**Przed zapisaniem pliku Uruchom skaner wirusów/złośliwego oprogramowania dla przekazanej zawartości.**</span><span class="sxs-lookup"><span data-stu-id="f9027-125">**Run a virus/malware scanner on uploaded content before the file is stored.**</span></span>

<span data-ttu-id="f9027-126">Przykładowa aplikacja &dagger;The ilustruje podejście, które spełnia kryteria.</span><span class="sxs-lookup"><span data-stu-id="f9027-126">&dagger;The sample app demonstrates an approach that meets the criteria.</span></span>

> [!WARNING]
> <span data-ttu-id="f9027-127">Przekazywanie złośliwego kodu do systemu często jest pierwszym krokiem do wykonywania kodu, która może być:</span><span class="sxs-lookup"><span data-stu-id="f9027-127">Uploading malicious code to a system is frequently the first step to executing code that can:</span></span>
>
> * <span data-ttu-id="f9027-128">Całkowicie przejąć kontrolę nad systemem.</span><span class="sxs-lookup"><span data-stu-id="f9027-128">Completely gain control of a system.</span></span>
> * <span data-ttu-id="f9027-129">Przeciąż system z wynikiem awarii systemu.</span><span class="sxs-lookup"><span data-stu-id="f9027-129">Overload a system with the result that the system crashes.</span></span>
> * <span data-ttu-id="f9027-130">Naruszyć bezpieczeństwo danych użytkownika lub systemu.</span><span class="sxs-lookup"><span data-stu-id="f9027-130">Compromise user or system data.</span></span>
> * <span data-ttu-id="f9027-131">Zastosuj graffiti do publicznego interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f9027-131">Apply graffiti to a public UI.</span></span>
>
> <span data-ttu-id="f9027-132">Instrukcje dotyczące zmniejszenie obszaru powierzchni ataku, akceptując pliki użytkowników zobacz następujące zasoby:</span><span class="sxs-lookup"><span data-stu-id="f9027-132">For information on reducing the attack surface area when accepting files from users, see the following resources:</span></span>
>
> * [<span data-ttu-id="f9027-133">Przekazywanie pliku bez ograniczeń</span><span class="sxs-lookup"><span data-stu-id="f9027-133">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * <span data-ttu-id="f9027-134">@no__t — zabezpieczenia 0Azure: Upewnij się, że podczas akceptowania plików od użytkowników jest stosowane odpowiednie kontrolki @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="f9027-134">[Azure Security: Ensure appropriate controls are in place when accepting files from users](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)</span></span>

<span data-ttu-id="f9027-135">Aby uzyskać więcej informacji na temat implementowania środków zabezpieczeń, w tym przykładów z przykładowej aplikacji, zobacz sekcję [Walidacja](#validation) .</span><span class="sxs-lookup"><span data-stu-id="f9027-135">For more information on implementing security measures, including examples from the sample app, see the [Validation](#validation) section.</span></span>

## <a name="storage-scenarios"></a><span data-ttu-id="f9027-136">Scenariusze magazynu</span><span class="sxs-lookup"><span data-stu-id="f9027-136">Storage scenarios</span></span>

<span data-ttu-id="f9027-137">Typowe opcje magazynu dla plików to:</span><span class="sxs-lookup"><span data-stu-id="f9027-137">Common storage options for files include:</span></span>

* <span data-ttu-id="f9027-138">Database (Baza danych)</span><span class="sxs-lookup"><span data-stu-id="f9027-138">Database</span></span>

  * <span data-ttu-id="f9027-139">W przypadku małych operacji przekazywania plików baza danych jest często szybsza niż opcje magazynu fizycznego (systemu plików lub udziału sieciowego).</span><span class="sxs-lookup"><span data-stu-id="f9027-139">For small file uploads, a database is often faster than physical storage (file system or network share) options.</span></span>
  * <span data-ttu-id="f9027-140">Baza danych jest często bardziej wygodna niż opcje magazynu fizycznego, ponieważ Pobieranie rekordu bazy danych dla danych użytkownika może jednocześnie dostarczyć zawartość pliku (na przykład obraz awatara).</span><span class="sxs-lookup"><span data-stu-id="f9027-140">A database is often more convenient than physical storage options because retrieval of a database record for user data can concurrently supply the file content (for example, an avatar image).</span></span>
  * <span data-ttu-id="f9027-141">Baza danych może być tańsza niż użycie usługi magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="f9027-141">A database is potentially less expensive than using a data storage service.</span></span>

* <span data-ttu-id="f9027-142">Magazyn fizyczny (system plików lub udział sieciowy)</span><span class="sxs-lookup"><span data-stu-id="f9027-142">Physical storage (file system or network share)</span></span>

  * <span data-ttu-id="f9027-143">W przypadku dużych operacji przekazywania plików:</span><span class="sxs-lookup"><span data-stu-id="f9027-143">For large file uploads:</span></span>
    * <span data-ttu-id="f9027-144">Limity bazy danych mogą ograniczać rozmiar przekazywania.</span><span class="sxs-lookup"><span data-stu-id="f9027-144">Database limits may restrict the size of the upload.</span></span>
    * <span data-ttu-id="f9027-145">Magazyn fizyczny jest często mniej ekonomiczny niż magazyn w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f9027-145">Physical storage is often less economical than storage in a database.</span></span>
  * <span data-ttu-id="f9027-146">Magazyn fizyczny jest prawdopodobnie tańszy niż użycie usługi magazynu danych.</span><span class="sxs-lookup"><span data-stu-id="f9027-146">Physical storage is potentially less expensive than using a data storage service.</span></span>
  * <span data-ttu-id="f9027-147">Proces aplikacji musi mieć uprawnienia do odczytu i zapisu w lokalizacji magazynu.</span><span class="sxs-lookup"><span data-stu-id="f9027-147">The app's process must have read and write permissions to the storage location.</span></span> <span data-ttu-id="f9027-148">**Nigdy nie udzielaj uprawnienia EXECUTE.**</span><span class="sxs-lookup"><span data-stu-id="f9027-148">**Never grant execute permission.**</span></span>

* <span data-ttu-id="f9027-149">Usługa magazynu danych (na przykład [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span><span class="sxs-lookup"><span data-stu-id="f9027-149">Data storage service (for example, [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/))</span></span>

  * <span data-ttu-id="f9027-150">Usługa zazwyczaj oferuje ulepszoną skalowalność i odporność w rozwiązaniach lokalnych, które zwykle podlegają pojedynczym punktom awarii.</span><span class="sxs-lookup"><span data-stu-id="f9027-150">Services usually offer improved scalability and resiliency over on-premises solutions that are usually subject to single points of failure.</span></span>
  * <span data-ttu-id="f9027-151">Usługi są potencjalnie tańsze w dużych scenariuszach infrastruktury magazynu.</span><span class="sxs-lookup"><span data-stu-id="f9027-151">Services are potentially lower cost in large storage infrastructure scenarios.</span></span>

  <span data-ttu-id="f9027-152">Aby uzyskać więcej informacji, [zobacz Szybki Start: Użyj programu .NET, aby utworzyć obiekt BLOB w magazynie obiektów @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="f9027-152">For more information, see [Quickstart: Use .NET to create a blob in object storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet).</span></span> <span data-ttu-id="f9027-153">W temacie przedstawiono <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, ale <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> można użyć do zapisania <xref:System.IO.FileStream> do magazynu obiektów BLOB podczas pracy z <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="f9027-153">The topic demonstrates <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromFileAsync*>, but <xref:Microsoft.Azure.Storage.File.CloudFile.UploadFromStreamAsync*> can be used to save a <xref:System.IO.FileStream> to blob storage when working with a <xref:System.IO.Stream>.</span></span>

## <a name="file-upload-scenarios"></a><span data-ttu-id="f9027-154">Scenariusze przekazywania plików</span><span class="sxs-lookup"><span data-stu-id="f9027-154">File upload scenarios</span></span>

<span data-ttu-id="f9027-155">Dwie ogólne podejścia do przekazywania plików to buforowanie i przesyłanie strumieniowe.</span><span class="sxs-lookup"><span data-stu-id="f9027-155">Two general approaches for uploading files are buffering and streaming.</span></span>

<span data-ttu-id="f9027-156">**Buforowania**</span><span class="sxs-lookup"><span data-stu-id="f9027-156">**Buffering**</span></span>

<span data-ttu-id="f9027-157">Cały plik jest odczytywany do <xref:Microsoft.AspNetCore.Http.IFormFile>, który jest C# reprezentacją pliku używanego do przetworzenia lub zapisania pliku.</span><span class="sxs-lookup"><span data-stu-id="f9027-157">The entire file is read into an <xref:Microsoft.AspNetCore.Http.IFormFile>, which is a C# representation of the file used to process or save the file.</span></span>

<span data-ttu-id="f9027-158">Zasoby (dysk, pamięć) używane przez operacje przekazywania plików zależą od liczby i rozmiaru współbieżnych przekazywania plików.</span><span class="sxs-lookup"><span data-stu-id="f9027-158">The resources (disk, memory) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="f9027-159">Jeśli aplikacja próbuje buforować zbyt wiele przeciążeń, lokacja uległa awarii, gdy zabraknie pamięci lub miejsca na dysku.</span><span class="sxs-lookup"><span data-stu-id="f9027-159">If an app attempts to buffer too many uploads, the site crashes when it runs out of memory or disk space.</span></span> <span data-ttu-id="f9027-160">Jeśli rozmiar lub częstotliwość przekazywania plików powoduje wyczerpanie zasobów aplikacji, należy użyć przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="f9027-160">If the size or frequency of file uploads is exhausting app resources, use streaming.</span></span>

> [!NOTE]
> <span data-ttu-id="f9027-161">Każdy pojedynczy buforowany plik przekraczający 64 KB jest przenoszony z pamięci do pliku tymczasowego na dysku.</span><span class="sxs-lookup"><span data-stu-id="f9027-161">Any single buffered file exceeding 64 KB is moved from memory to a temp file on disk.</span></span>

<span data-ttu-id="f9027-162">Buforowanie małych plików zostało omówione w następujących sekcjach tego tematu:</span><span class="sxs-lookup"><span data-stu-id="f9027-162">Buffering small files is covered in the following sections of this topic:</span></span>

* [<span data-ttu-id="f9027-163">Magazyn fizyczny</span><span class="sxs-lookup"><span data-stu-id="f9027-163">Physical storage</span></span>](#upload-small-files-with-buffered-model-binding-to-physical-storage)
* [<span data-ttu-id="f9027-164">Baza danych</span><span class="sxs-lookup"><span data-stu-id="f9027-164">Database</span></span>](#upload-small-files-with-buffered-model-binding-to-a-database)

<span data-ttu-id="f9027-165">**Przesyłanie strumieniowe**</span><span class="sxs-lookup"><span data-stu-id="f9027-165">**Streaming**</span></span>

<span data-ttu-id="f9027-166">Plik jest odbierany od żądania wieloczęściowego i bezpośrednio przetwarzany lub zapisywany przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="f9027-166">The file is received from a multipart request and directly processed or saved by the app.</span></span> <span data-ttu-id="f9027-167">Przesyłanie strumieniowe nie poprawia znacząco wydajności.</span><span class="sxs-lookup"><span data-stu-id="f9027-167">Streaming doesn't improve performance significantly.</span></span> <span data-ttu-id="f9027-168">Przesyłanie strumieniowe zmniejsza wymagania dotyczące pamięci lub miejsca na dysku podczas przekazywania plików.</span><span class="sxs-lookup"><span data-stu-id="f9027-168">Streaming reduces the demands for memory or disk space when uploading files.</span></span>

<span data-ttu-id="f9027-169">Przesyłanie strumieniowe dużych plików jest omówione w sekcji [przekazywanie dużych plików z przesyłaniem strumieniowym](#upload-large-files-with-streaming) .</span><span class="sxs-lookup"><span data-stu-id="f9027-169">Streaming large files is covered in the [Upload large files with streaming](#upload-large-files-with-streaming) section.</span></span>

### <a name="upload-small-files-with-buffered-model-binding-to-physical-storage"></a><span data-ttu-id="f9027-170">Przekazywanie małych plików z buforowanym powiązaniem modelu z magazynem fizycznym</span><span class="sxs-lookup"><span data-stu-id="f9027-170">Upload small files with buffered model binding to physical storage</span></span>

<span data-ttu-id="f9027-171">Aby przekazać małe pliki, użyj formularza wieloczęściowego lub Skonstruuj żądanie POST przy użyciu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f9027-171">To upload small files, use a multipart form or construct a POST request using JavaScript.</span></span>

<span data-ttu-id="f9027-172">Poniższy przykład ilustruje użycie formularza Razor Pages do przekazywania pojedynczego pliku (*Pages/BufferedSingleFileUploadPhysical. cshtml* w przykładowej aplikacji):</span><span class="sxs-lookup"><span data-stu-id="f9027-172">The following example demonstrates the use of a Razor Pages form to upload a single file (*Pages/BufferedSingleFileUploadPhysical.cshtml* in the sample app):</span></span>

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

<span data-ttu-id="f9027-173">Poniższy przykład jest analogiczny do poprzedniego przykładu, z wyjątkiem tego, że:</span><span class="sxs-lookup"><span data-stu-id="f9027-173">The following example is analogous to the prior example except that:</span></span>

* <span data-ttu-id="f9027-174">Kod JavaScript ([interfejs API pobierania](https://developer.mozilla.org/docs/Web/API/Fetch_API)) służy do przesyłania danych formularza.</span><span class="sxs-lookup"><span data-stu-id="f9027-174">JavaScript's ([Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)) is used to submit the form's data.</span></span>
* <span data-ttu-id="f9027-175">Nie istnieje weryfikacja.</span><span class="sxs-lookup"><span data-stu-id="f9027-175">There's no validation.</span></span>

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

<span data-ttu-id="f9027-176">Aby wykonać formularz POST w języku JavaScript dla klientów, którzy [nie obsługują interfejsu API pobierania](https://caniuse.com/#feat=fetch), należy użyć jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="f9027-176">To perform the form POST in JavaScript for clients that [don't support the Fetch API](https://caniuse.com/#feat=fetch), use one of the following approaches:</span></span>

* <span data-ttu-id="f9027-177">Użyj wypełniania pobierania (na przykład [window. Fetch Fill (GitHub/Fetch)](https://github.com/github/fetch)).</span><span class="sxs-lookup"><span data-stu-id="f9027-177">Use a Fetch Polyfill (for example, [window.fetch polyfill (github/fetch)](https://github.com/github/fetch)).</span></span>
* <span data-ttu-id="f9027-178">Użyj `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="f9027-178">Use `XMLHttpRequest`.</span></span> <span data-ttu-id="f9027-179">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f9027-179">For example:</span></span>

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

<span data-ttu-id="f9027-180">W celu obsługi przekazywania plików formularze HTML muszą określać typ kodowania (`enctype`) dla `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="f9027-180">In order to support file uploads, HTML forms must specify an encoding type (`enctype`) of `multipart/form-data`.</span></span>

<span data-ttu-id="f9027-181">Aby element wejściowy `files` obsługiwał przekazywanie wielu plików, podaj atrybut `multiple` dla elementu `<input>`:</span><span class="sxs-lookup"><span data-stu-id="f9027-181">For a `files` input element to support uploading multiple files provide the `multiple` attribute on the `<input>` element:</span></span>

```cshtml
<input asp-for="FileUpload.FormFiles" type="file" multiple>
```

<span data-ttu-id="f9027-182">Do poszczególnych plików przekazanych do serwera można uzyskać dostęp za pośrednictwem [powiązania modelu](xref:mvc/models/model-binding) przy użyciu <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="f9027-182">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span> <span data-ttu-id="f9027-183">Przykładowa aplikacja pokazuje wiele buforowanych operacji przekazywania plików dla scenariuszy bazy danych i magazynu fizycznego.</span><span class="sxs-lookup"><span data-stu-id="f9027-183">The sample app demonstrates multiple buffered file uploads for database and physical storage scenarios.</span></span>

> [!WARNING]
> <span data-ttu-id="f9027-184">Nie używaj ani nie ufaj właściwości `FileName` <xref:Microsoft.AspNetCore.Http.IFormFile> bez sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="f9027-184">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="f9027-185">Właściwość `FileName` powinna być używana tylko do celów wyświetlania i tylko po kodowaniu w kodzie HTML wartości.</span><span class="sxs-lookup"><span data-stu-id="f9027-185">The `FileName` property should only be used for display purposes and only after HTML encoding the value.</span></span>
>
> <span data-ttu-id="f9027-186">Przykłady udostępnione w ten sposób nie uwzględniają zagadnień związanych z bezpieczeństwem.</span><span class="sxs-lookup"><span data-stu-id="f9027-186">The examples provided thus far don't take into account security considerations.</span></span> <span data-ttu-id="f9027-187">Dodatkowe informacje są dostarczane przez następujące sekcje i [Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="f9027-187">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="f9027-188">Zagadnienia dotyczące bezpieczeństwa</span><span class="sxs-lookup"><span data-stu-id="f9027-188">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="f9027-189">Walidacja</span><span class="sxs-lookup"><span data-stu-id="f9027-189">Validation</span></span>](#validation)

<span data-ttu-id="f9027-190">Podczas przekazywania plików przy użyciu powiązania modelu i <xref:Microsoft.AspNetCore.Http.IFormFile> Metoda akcji może przyjmować następujące wartości:</span><span class="sxs-lookup"><span data-stu-id="f9027-190">When uploading files using model binding and <xref:Microsoft.AspNetCore.Http.IFormFile>, the action method can accept:</span></span>

* <span data-ttu-id="f9027-191">Jeden <xref:Microsoft.AspNetCore.Http.IFormFile>.</span><span class="sxs-lookup"><span data-stu-id="f9027-191">A single <xref:Microsoft.AspNetCore.Http.IFormFile>.</span></span>
* <span data-ttu-id="f9027-192">Dowolne z następujących kolekcji, które reprezentują kilka plików:</span><span class="sxs-lookup"><span data-stu-id="f9027-192">Any of the following collections that represent several files:</span></span>
  * <xref:Microsoft.AspNetCore.Http.IFormFileCollection>
  * <xref:System.Collections.IEnumerable>\<<xref:Microsoft.AspNetCore.Http.IFormFile>>
  * <span data-ttu-id="f9027-193">[Lista](xref:System.Collections.Generic.List`1)\< @ no__t-2 @ no__t-3</span><span class="sxs-lookup"><span data-stu-id="f9027-193">[List](xref:System.Collections.Generic.List`1)\<<xref:Microsoft.AspNetCore.Http.IFormFile>></span></span>

> [!NOTE]
> <span data-ttu-id="f9027-194">Powiązanie dopasowuje pliki formularza według nazwy.</span><span class="sxs-lookup"><span data-stu-id="f9027-194">Binding matches form files by name.</span></span> <span data-ttu-id="f9027-195">Na przykład wartość HTML `name` w `<input type="file" name="formFile">` musi być zgodna z C# wartością parametru/właściwości (`FormFile`).</span><span class="sxs-lookup"><span data-stu-id="f9027-195">For example, the HTML `name` value in `<input type="file" name="formFile">` must match the C# parameter/property bound (`FormFile`).</span></span> <span data-ttu-id="f9027-196">Aby uzyskać więcej informacji, zobacz sekcję [dopasowanie wartości atrybutu do nazwy parametru w sekcji Metoda post](#match-name-attribute-value-to-parameter-name-of-post-method) .</span><span class="sxs-lookup"><span data-stu-id="f9027-196">For more information, see the [Match name attribute value to parameter name of POST method](#match-name-attribute-value-to-parameter-name-of-post-method) section.</span></span>

<span data-ttu-id="f9027-197">Poniższy przykład:</span><span class="sxs-lookup"><span data-stu-id="f9027-197">The following example:</span></span>

* <span data-ttu-id="f9027-198">Pętle przez co najmniej jeden przekazany plik.</span><span class="sxs-lookup"><span data-stu-id="f9027-198">Loops through one or more uploaded files.</span></span>
* <span data-ttu-id="f9027-199">Używa funkcji [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) , aby zwrócić pełną ścieżkę do pliku, łącznie z nazwą pliku.</span><span class="sxs-lookup"><span data-stu-id="f9027-199">Uses [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to return a full path for a file, including the file name.</span></span> 
* <span data-ttu-id="f9027-200">Zapisuje pliki w lokalnym systemie plików przy użyciu nazwy pliku wygenerowanej przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="f9027-200">Saves the files to the local file system using a file name generated by the app.</span></span>
* <span data-ttu-id="f9027-201">Zwraca łączną liczbę i rozmiar przekazanych plików.</span><span class="sxs-lookup"><span data-stu-id="f9027-201">Returns the total number and size of files uploaded.</span></span>

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

<span data-ttu-id="f9027-202">Użyj `Path.GetRandomFileName` do wygenerowania nazwy pliku bez ścieżki.</span><span class="sxs-lookup"><span data-stu-id="f9027-202">Use `Path.GetRandomFileName` to generate a file name without a path.</span></span> <span data-ttu-id="f9027-203">W poniższym przykładzie ścieżka jest uzyskiwana z konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="f9027-203">In the following example, the path is obtained from configuration:</span></span>

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

<span data-ttu-id="f9027-204">Ścieżka przeniesiona do <xref:System.IO.FileStream> *musi* zawierać nazwę pliku.</span><span class="sxs-lookup"><span data-stu-id="f9027-204">The path passed to the <xref:System.IO.FileStream> *must* include the file name.</span></span> <span data-ttu-id="f9027-205">Jeśli nie podano nazwy pliku, <xref:System.UnauthorizedAccessException> jest generowany w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="f9027-205">If the file name isn't provided, an <xref:System.UnauthorizedAccessException> is thrown at runtime.</span></span>

<span data-ttu-id="f9027-206">Pliki przekazane przy użyciu techniki <xref:Microsoft.AspNetCore.Http.IFormFile> są buforowane w pamięci lub na dysku na serwerze przed przetworzeniem.</span><span class="sxs-lookup"><span data-stu-id="f9027-206">Files uploaded using the <xref:Microsoft.AspNetCore.Http.IFormFile> technique are buffered in memory or on disk on the server before processing.</span></span> <span data-ttu-id="f9027-207">Wewnątrz metody akcji zawartość <xref:Microsoft.AspNetCore.Http.IFormFile> jest dostępna jako <xref:System.IO.Stream>.</span><span class="sxs-lookup"><span data-stu-id="f9027-207">Inside the action method, the <xref:Microsoft.AspNetCore.Http.IFormFile> contents are accessible as a <xref:System.IO.Stream>.</span></span> <span data-ttu-id="f9027-208">Oprócz lokalnego systemu plików można zapisywać pliki w udziale sieciowym lub w usłudze magazynu plików, na przykład w [usłudze Azure Blob Storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span><span class="sxs-lookup"><span data-stu-id="f9027-208">In addition to the local file system, files can be saved to a network share or to a file storage service, such as [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs).</span></span>

<span data-ttu-id="f9027-209">Aby uzyskać inny przykład pętli dla wielu plików na potrzeby przekazywania i używania bezpiecznych nazw plików, zobacz *Pages/BufferedMultipleFileUploadPhysical. cshtml. cs* w przykładowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f9027-209">For another example that loops over multiple files for upload and uses safe file names, see *Pages/BufferedMultipleFileUploadPhysical.cshtml.cs* in the sample app.</span></span>

> [!WARNING]
> <span data-ttu-id="f9027-210">[Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) zgłasza <xref:System.IO.IOException>, jeśli utworzono więcej niż 65 535 plików bez usuwania poprzednich plików tymczasowych.</span><span class="sxs-lookup"><span data-stu-id="f9027-210">[Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) throws an <xref:System.IO.IOException> if more than 65,535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="f9027-211">Limit 65 535 plików jest limitem dla serwera.</span><span class="sxs-lookup"><span data-stu-id="f9027-211">The limit of 65,535 files is a per-server limit.</span></span> <span data-ttu-id="f9027-212">Aby uzyskać więcej informacji na temat tego limitu dla systemu operacyjnego Windows, zobacz uwagi w następujących tematach:</span><span class="sxs-lookup"><span data-stu-id="f9027-212">For more information on this limit on Windows OS, see the remarks in the following topics:</span></span>
>
> * [<span data-ttu-id="f9027-213">GetTempFileNameA, funkcja</span><span class="sxs-lookup"><span data-stu-id="f9027-213">GetTempFileNameA function</span></span>](/windows/desktop/api/fileapi/nf-fileapi-gettempfilenamea#remarks)
> * <xref:System.IO.Path.GetTempFileName*>

### <a name="upload-small-files-with-buffered-model-binding-to-a-database"></a><span data-ttu-id="f9027-214">Przekazywanie małych plików z buforowanym powiązaniem modelu z bazą danych</span><span class="sxs-lookup"><span data-stu-id="f9027-214">Upload small files with buffered model binding to a database</span></span>

<span data-ttu-id="f9027-215">Aby przechowywać dane binarne pliku w bazie danych przy użyciu [Entity Framework](/ef/core/index), zdefiniuj właściwość tablicy <xref:System.Byte> dla jednostki:</span><span class="sxs-lookup"><span data-stu-id="f9027-215">To store binary file data in a database using [Entity Framework](/ef/core/index), define a <xref:System.Byte> array property on the entity:</span></span>

```csharp
public class AppFile
{
    public int Id { get; set; }
    public byte[] Content { get; set; }
}
```

<span data-ttu-id="f9027-216">Określ właściwość modelu strony dla klasy, która zawiera <xref:Microsoft.AspNetCore.Http.IFormFile>:</span><span class="sxs-lookup"><span data-stu-id="f9027-216">Specify a page model property for the class that includes an <xref:Microsoft.AspNetCore.Http.IFormFile>:</span></span>

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
> <span data-ttu-id="f9027-217"><xref:Microsoft.AspNetCore.Http.IFormFile> może być używana bezpośrednio jako parametr metody akcji lub jako właściwość modelu powiązania.</span><span class="sxs-lookup"><span data-stu-id="f9027-217"><xref:Microsoft.AspNetCore.Http.IFormFile> can be used directly as an action method parameter or as a bound model property.</span></span> <span data-ttu-id="f9027-218">W poprzednim przykładzie użyto powiązanej właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="f9027-218">The prior example uses a bound model property.</span></span>

<span data-ttu-id="f9027-219">@No__t-0 jest używana w formularzu Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="f9027-219">The `FileUpload` is used in the Razor Pages form:</span></span>

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

<span data-ttu-id="f9027-220">Po opublikowaniu formularza na serwerze Skopiuj <xref:Microsoft.AspNetCore.Http.IFormFile> do strumienia i Zapisz go jako tablicę bajtową w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="f9027-220">When the form is POSTed to the server, copy the <xref:Microsoft.AspNetCore.Http.IFormFile> to a stream and save it as a byte array in the database.</span></span> <span data-ttu-id="f9027-221">W poniższym przykładzie `_dbContext` przechowuje kontekst bazy danych aplikacji:</span><span class="sxs-lookup"><span data-stu-id="f9027-221">In the following example, `_dbContext` stores the app's database context:</span></span>

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

<span data-ttu-id="f9027-222">Poprzedni przykład przypomina scenariusz przedstawiony w przykładowej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="f9027-222">The preceding example is similar to a scenario demonstrated in the sample app:</span></span>

* <span data-ttu-id="f9027-223">*Pages/BufferedSingleFileUploadDb. cshtml*</span><span class="sxs-lookup"><span data-stu-id="f9027-223">*Pages/BufferedSingleFileUploadDb.cshtml*</span></span>
* <span data-ttu-id="f9027-224">*Strony/BufferedSingleFileUploadDb. cshtml. cs*</span><span class="sxs-lookup"><span data-stu-id="f9027-224">*Pages/BufferedSingleFileUploadDb.cshtml.cs*</span></span>

> [!WARNING]
> <span data-ttu-id="f9027-225">Należy zachować ostrożność podczas przechowywania danych binarnych w relacyjnych bazach danych, ponieważ może to mieć negatywny wpływ na wydajność.</span><span class="sxs-lookup"><span data-stu-id="f9027-225">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>
>
> <span data-ttu-id="f9027-226">Nie używaj ani nie ufaj właściwości `FileName` <xref:Microsoft.AspNetCore.Http.IFormFile> bez sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="f9027-226">Don't rely on or trust the `FileName` property of <xref:Microsoft.AspNetCore.Http.IFormFile> without validation.</span></span> <span data-ttu-id="f9027-227">Właściwość `FileName` powinna być używana tylko na potrzeby wyświetlania i tylko po kodowaniu HTML.</span><span class="sxs-lookup"><span data-stu-id="f9027-227">The `FileName` property should only be used for display purposes and only after HTML encoding.</span></span>
>
> <span data-ttu-id="f9027-228">Podane przykłady nie uwzględniają zagadnień związanych z zabezpieczeniami.</span><span class="sxs-lookup"><span data-stu-id="f9027-228">The examples provided don't take into account security considerations.</span></span> <span data-ttu-id="f9027-229">Dodatkowe informacje są dostarczane przez następujące sekcje i [Przykładowa aplikacja](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span><span class="sxs-lookup"><span data-stu-id="f9027-229">Additional information is provided by the following sections and the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/file-uploads/samples/):</span></span>
>
> * [<span data-ttu-id="f9027-230">Zagadnienia dotyczące bezpieczeństwa</span><span class="sxs-lookup"><span data-stu-id="f9027-230">Security considerations</span></span>](#security-considerations)
> * [<span data-ttu-id="f9027-231">Walidacja</span><span class="sxs-lookup"><span data-stu-id="f9027-231">Validation</span></span>](#validation)

### <a name="upload-large-files-with-streaming"></a><span data-ttu-id="f9027-232">Przekazywanie dużych plików strumieniowo</span><span class="sxs-lookup"><span data-stu-id="f9027-232">Upload large files with streaming</span></span>

<span data-ttu-id="f9027-233">Poniższy przykład ilustruje sposób użycia języka JavaScript do przesyłania strumieniowego pliku do akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f9027-233">The following example demonstrates how to use JavaScript to stream a file to a controller action.</span></span> <span data-ttu-id="f9027-234">Token antysfałszowany pliku jest generowany przy użyciu niestandardowego atrybutu filtru i przekazywać do nagłówków HTTP klienta zamiast w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="f9027-234">The file's antiforgery token is generated using a custom filter attribute and passed to the client HTTP headers instead of in the request body.</span></span> <span data-ttu-id="f9027-235">Ponieważ metoda akcji przetwarza przekazane dane bezpośrednio, powiązanie modelu formularza jest wyłączone przez inny filtr niestandardowy.</span><span class="sxs-lookup"><span data-stu-id="f9027-235">Because the action method processes the uploaded data directly, form model binding is disabled by another custom filter.</span></span> <span data-ttu-id="f9027-236">W ramach akcji zawartość formularza jest odczytywana przy użyciu `MultipartReader`, który odczytuje każdy indywidualny `MultipartSection`, przetwarza plik lub przechowując zawartość odpowiednio do potrzeb.</span><span class="sxs-lookup"><span data-stu-id="f9027-236">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="f9027-237">Po odczytaniu sekcji wieloczęściowej akcja wykonuje własne powiązanie modelu.</span><span class="sxs-lookup"><span data-stu-id="f9027-237">After the multipart sections are read, the action performs its own model binding.</span></span>

<span data-ttu-id="f9027-238">Początkowa odpowiedź strony ładuje formularz i zapisuje w pliku cookie token antysfałszowany (za pośrednictwem atrybutu `GenerateAntiforgeryTokenCookieAttribute`).</span><span class="sxs-lookup"><span data-stu-id="f9027-238">The initial page response loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieAttribute` attribute).</span></span> <span data-ttu-id="f9027-239">Ten atrybut używa wbudowanej [obsługi przed fałszowaniem](xref:security/anti-request-forgery) ASP.NET Core, aby ustawić plik cookie z tokenem żądania:</span><span class="sxs-lookup"><span data-stu-id="f9027-239">The attribute uses ASP.NET Core's built-in [antiforgery support](xref:security/anti-request-forgery) to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/Antiforgery.cs?name=snippet_GenerateAntiforgeryTokenCookieAttribute)]

<span data-ttu-id="f9027-240">@No__t-0 służy do wyłączania powiązania modelu:</span><span class="sxs-lookup"><span data-stu-id="f9027-240">The `DisableFormValueModelBindingAttribute` is used to disable model binding:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Filters/ModelBinding.cs?name=snippet_DisableFormValueModelBindingAttribute)]

<span data-ttu-id="f9027-241">W przykładowej aplikacji `GenerateAntiforgeryTokenCookieAttribute` i `DisableFormValueModelBindingAttribute` są stosowane jako filtry do modeli aplikacji strony `/StreamedSingleFileUploadDb` i `/StreamedSingleFileUploadPhysical` w `Startup.ConfigureServices` przy użyciu [konwencji Razor Pages](xref:razor-pages/razor-pages-conventions):</span><span class="sxs-lookup"><span data-stu-id="f9027-241">In the sample app, `GenerateAntiforgeryTokenCookieAttribute` and `DisableFormValueModelBindingAttribute` are applied as filters to the page application models of `/StreamedSingleFileUploadDb` and `/StreamedSingleFileUploadPhysical` in `Startup.ConfigureServices` using [Razor Pages conventions](xref:razor-pages/razor-pages-conventions):</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Startup.cs?name=snippet_AddMvc&highlight=8-11,17-20)]

<span data-ttu-id="f9027-242">Ponieważ powiązanie modelu nie odczytuje formularza, parametry, które są powiązane z formularza nie są powiązane (zapytania, trasy i nagłówki nadal pracują).</span><span class="sxs-lookup"><span data-stu-id="f9027-242">Since model binding doesn't read the form, parameters that are bound from the form don't bind (query, route, and header continue to work).</span></span> <span data-ttu-id="f9027-243">Metoda akcji działa bezpośrednio z właściwością `Request`.</span><span class="sxs-lookup"><span data-stu-id="f9027-243">The action method works directly with the `Request` property.</span></span> <span data-ttu-id="f9027-244">@No__t-0 jest używany do odczytywania każdej sekcji.</span><span class="sxs-lookup"><span data-stu-id="f9027-244">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="f9027-245">Dane klucza/wartości są przechowywane w `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="f9027-245">Key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="f9027-246">Po odczytaniu sekcji wieloczęściowych zawartość `KeyValueAccumulator` służy do powiązania danych formularza z typem modelu.</span><span class="sxs-lookup"><span data-stu-id="f9027-246">After the multipart sections are read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="f9027-247">Pełna Metoda `StreamingController.UploadDatabase` do przesyłania strumieniowego do bazy danych z EF Core:</span><span class="sxs-lookup"><span data-stu-id="f9027-247">The complete `StreamingController.UploadDatabase` method for streaming to a database with EF Core:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadDatabase)]

<span data-ttu-id="f9027-248">Pełna Metoda `StreamingController.UploadPhysical` do przesyłania strumieniowego do lokalizacji fizycznej:</span><span class="sxs-lookup"><span data-stu-id="f9027-248">The complete `StreamingController.UploadPhysical` method for streaming to a physical location:</span></span>

[!code-csharp[](file-uploads/samples/2.x/SampleApp/Controllers/StreamingController.cs?name=snippet_UploadPhysical)]

<span data-ttu-id="f9027-249">W przykładowej aplikacji sprawdzanie poprawności jest obsługiwane przez `FileHelpers.ProcessStreamedFile`.</span><span class="sxs-lookup"><span data-stu-id="f9027-249">In the sample app, validation checks are handled by `FileHelpers.ProcessStreamedFile`.</span></span>

## <a name="validation"></a><span data-ttu-id="f9027-250">Walidacja</span><span class="sxs-lookup"><span data-stu-id="f9027-250">Validation</span></span>

<span data-ttu-id="f9027-251">Klasa `FileHelpers` aplikacji przykładowej pokazuje kilka testów dla buforowanych <xref:Microsoft.AspNetCore.Http.IFormFile> i przesyłanych strumieniowo przekazywania plików.</span><span class="sxs-lookup"><span data-stu-id="f9027-251">The sample app's `FileHelpers` class demonstrates a several checks for buffered <xref:Microsoft.AspNetCore.Http.IFormFile> and streamed file uploads.</span></span> <span data-ttu-id="f9027-252">Aby przetwarzać <xref:Microsoft.AspNetCore.Http.IFormFile> buforowane przekazywanie plików w przykładowej aplikacji, zobacz `ProcessFormFile` metody w pliku *Utilities/FileHelpers. cs* .</span><span class="sxs-lookup"><span data-stu-id="f9027-252">For processing <xref:Microsoft.AspNetCore.Http.IFormFile> buffered file uploads in the sample app, see the `ProcessFormFile` method in the *Utilities/FileHelpers.cs* file.</span></span> <span data-ttu-id="f9027-253">Aby można było przetwarzać pliki przesyłane strumieniowo, zobacz metodę `ProcessStreamedFile` w tym samym pliku.</span><span class="sxs-lookup"><span data-stu-id="f9027-253">For processing streamed files, see the `ProcessStreamedFile` method in the same file.</span></span>

> [!WARNING]
> <span data-ttu-id="f9027-254">Metody przetwarzania walidacji przedstawione w przykładowej aplikacji nie skanują zawartości przekazanych plików.</span><span class="sxs-lookup"><span data-stu-id="f9027-254">The validation processing methods demonstrated in the sample app don't scan the content of uploaded files.</span></span> <span data-ttu-id="f9027-255">W większości scenariuszy produkcyjnych do pliku jest używany interfejs API skanera wirusów/złośliwego oprogramowania przed udostępnieniem go użytkownikom lub innym systemom.</span><span class="sxs-lookup"><span data-stu-id="f9027-255">In most production scenarios, a virus/malware scanner API is used on the file before making the file available to users or other systems.</span></span>
>
> <span data-ttu-id="f9027-256">Chociaż przykład tematu zawiera przykładowy technikę sprawdzania poprawności, nie należy implementować klasy `FileHelpers` w aplikacji produkcyjnej, chyba że:</span><span class="sxs-lookup"><span data-stu-id="f9027-256">Although the topic sample provides a working example of validation techniques, don't implement the `FileHelpers` class in a production app unless you:</span></span>
>
> * <span data-ttu-id="f9027-257">W pełni zapoznaj się z implementacją.</span><span class="sxs-lookup"><span data-stu-id="f9027-257">Fully understand the implementation.</span></span>
> * <span data-ttu-id="f9027-258">Zmodyfikuj implementację odpowiednio do środowiska i specyfikacji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f9027-258">Modify the implementation as appropriate for the app's environment and specifications.</span></span>
>
> <span data-ttu-id="f9027-259">**Nigdy nie należy wdrażać kodu zabezpieczeń w aplikacji bez konieczności ich rozwiązywania.**</span><span class="sxs-lookup"><span data-stu-id="f9027-259">**Never indiscriminately implement security code in an app without addressing these requirements.**</span></span>

### <a name="content-validation"></a><span data-ttu-id="f9027-260">Weryfikacja zawartości</span><span class="sxs-lookup"><span data-stu-id="f9027-260">Content validation</span></span>

<span data-ttu-id="f9027-261">**Użyj interfejsu API skanowania wirusa/złośliwego oprogramowania innej firmy dla przekazanej zawartości.**</span><span class="sxs-lookup"><span data-stu-id="f9027-261">**Use a third party virus/malware scanning API on uploaded content.**</span></span>

<span data-ttu-id="f9027-262">Skanowanie plików wymaga użycia zasobów serwera w scenariuszach o dużych ilościach.</span><span class="sxs-lookup"><span data-stu-id="f9027-262">Scanning files is demanding on server resources in high volume scenarios.</span></span> <span data-ttu-id="f9027-263">Jeśli wydajność przetwarzania żądań jest zmniejszana ze względu na skanowanie plików, rozważ odciążenie pracy skanowania do [usługi w tle](xref:fundamentals/host/hosted-services), prawdopodobnie usługi uruchomionej na serwerze innym niż serwer aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f9027-263">If request processing performance is diminished due to file scanning, consider offloading the scanning work to a [background service](xref:fundamentals/host/hosted-services), possibly a service running on a server different from the app's server.</span></span> <span data-ttu-id="f9027-264">Zwykle przekazane pliki są przechowywane w obszarze poddany kwarantannie, dopóki skaner wirusów w tle nie sprawdzi ich.</span><span class="sxs-lookup"><span data-stu-id="f9027-264">Typically, uploaded files are held in a quarantined area until the background virus scanner checks them.</span></span> <span data-ttu-id="f9027-265">Gdy plik kończy się, plik zostanie przeniesiony do normalnej lokalizacji przechowywania plików.</span><span class="sxs-lookup"><span data-stu-id="f9027-265">When a file passes, the file is moved to the normal file storage location.</span></span> <span data-ttu-id="f9027-266">Te kroki są zwykle wykonywane w połączeniu z rekordem bazy danych, który wskazuje na stan skanowania pliku.</span><span class="sxs-lookup"><span data-stu-id="f9027-266">These steps are usually performed in conjunction with a database record that indicates the scanning status of a file.</span></span> <span data-ttu-id="f9027-267">Korzystając z takiego podejścia, aplikacja i serwer aplikacji pozostają skoncentrowane na odpowiedzi na żądania.</span><span class="sxs-lookup"><span data-stu-id="f9027-267">By using such an approach, the app and app server remain focused on responding to requests.</span></span>

### <a name="file-extension-validation"></a><span data-ttu-id="f9027-268">Weryfikacja rozszerzenia pliku</span><span class="sxs-lookup"><span data-stu-id="f9027-268">File extension validation</span></span>

<span data-ttu-id="f9027-269">Rozszerzenie przekazanego pliku powinno być sprawdzane względem listy dozwolonych rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="f9027-269">The uploaded file's extension should be checked against a list of permitted extensions.</span></span> <span data-ttu-id="f9027-270">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="f9027-270">For example:</span></span>

```csharp
private string[] permittedExtensions = { ".txt", ".pdf" };

var ext = Path.GetExtension(uploadedFileName).ToLowerInvariant();

if (string.IsNullOrEmpty(ext) || !permittedExtensions.Contains(ext))
{
    // The extension is invalid ... discontinue processing the file
}
```

### <a name="file-signature-validation"></a><span data-ttu-id="f9027-271">Walidacja podpisu pliku</span><span class="sxs-lookup"><span data-stu-id="f9027-271">File signature validation</span></span>

<span data-ttu-id="f9027-272">Sygnatura pliku jest określana przez pierwsze kilka bajtów na początku pliku.</span><span class="sxs-lookup"><span data-stu-id="f9027-272">A file's signature is determined by the first few bytes at the start of a file.</span></span> <span data-ttu-id="f9027-273">Te bajty mogą być używane do wskazywania, czy rozszerzenie jest zgodne z zawartością pliku.</span><span class="sxs-lookup"><span data-stu-id="f9027-273">These bytes can be used to indicate if the extension matches the content of the file.</span></span> <span data-ttu-id="f9027-274">Przykładowa aplikacja sprawdza podpisy plików dla kilku popularnych typów plików.</span><span class="sxs-lookup"><span data-stu-id="f9027-274">The sample app checks file signatures for a few common file types.</span></span> <span data-ttu-id="f9027-275">W poniższym przykładzie podpis pliku dla obrazu JPEG jest sprawdzany pod kątem pliku:</span><span class="sxs-lookup"><span data-stu-id="f9027-275">In the following example, the file signature for a JPEG image is checked against the file:</span></span>

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

<span data-ttu-id="f9027-276">Aby uzyskać dodatkowe podpisy plików, zapoznaj się z [bazami danych sygnatury plików](https://www.filesignatures.net/) i oficjalnymi specyfikacjami plików.</span><span class="sxs-lookup"><span data-stu-id="f9027-276">To obtain additional file signatures, see the [File Signatures Database](https://www.filesignatures.net/) and official file specifications.</span></span>

### <a name="file-name-security"></a><span data-ttu-id="f9027-277">Zabezpieczenia nazw plików</span><span class="sxs-lookup"><span data-stu-id="f9027-277">File name security</span></span>

<span data-ttu-id="f9027-278">Nigdy nie należy używać nazwy pliku dostarczonej przez klienta do zapisywania plików w magazynie fizycznym.</span><span class="sxs-lookup"><span data-stu-id="f9027-278">Never use a client-supplied file name for saving a file to physical storage.</span></span> <span data-ttu-id="f9027-279">Utwórz bezpieczną nazwę pliku dla pliku przy użyciu [ścieżki. GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) lub [Path. GetTempFileName](xref:System.IO.Path.GetTempFileName*) , aby utworzyć pełną ścieżkę (łącznie z nazwą pliku) dla magazynu tymczasowego.</span><span class="sxs-lookup"><span data-stu-id="f9027-279">Create a safe file name for the file using [Path.GetRandomFileName](xref:System.IO.Path.GetRandomFileName*) or [Path.GetTempFileName](xref:System.IO.Path.GetTempFileName*) to create a full path (including the file name) for temporary storage.</span></span>

<span data-ttu-id="f9027-280">KOD Razor automatycznie koduje wartości właściwości dla wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="f9027-280">Razor automatically HTML encodes property values for display.</span></span> <span data-ttu-id="f9027-281">Następujący kod jest bezpieczny w użyciu:</span><span class="sxs-lookup"><span data-stu-id="f9027-281">The following code is safe to use:</span></span>

```cshtml
@foreach (var file in Model.DatabaseFiles) {
    <tr>
        <td>
            @file.UntrustedName
        </td>
    </tr>
}
```

<span data-ttu-id="f9027-282">Poza Razor, zawsze <xref:System.Net.WebUtility.HtmlEncode*> zawartość nazwy pliku z żądania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f9027-282">Outside of Razor, always <xref:System.Net.WebUtility.HtmlEncode*> file name content from a user's request.</span></span>

<span data-ttu-id="f9027-283">Wiele implementacji musi zawierać sprawdzenie, czy plik istnieje; w przeciwnym razie plik zostanie zastąpiony przez plik o tej samej nazwie.</span><span class="sxs-lookup"><span data-stu-id="f9027-283">Many implementations must include a check that the file exists; otherwise, the file is overwritten by a file of the same name.</span></span> <span data-ttu-id="f9027-284">Podaj dodatkową logikę, aby spełnić wymagania dotyczące Twojej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f9027-284">Supply additional logic to meet your app's specifications.</span></span>

### <a name="size-validation"></a><span data-ttu-id="f9027-285">Sprawdzanie poprawności rozmiaru</span><span class="sxs-lookup"><span data-stu-id="f9027-285">Size validation</span></span>

<span data-ttu-id="f9027-286">Ogranicz rozmiar przekazanych plików.</span><span class="sxs-lookup"><span data-stu-id="f9027-286">Limit the size of uploaded files.</span></span>

<span data-ttu-id="f9027-287">W przykładowej aplikacji rozmiar pliku jest ograniczony do 2 MB (wyrażony w bajtach).</span><span class="sxs-lookup"><span data-stu-id="f9027-287">In the sample app, the size of the file is limited to 2 MB (indicated in bytes).</span></span> <span data-ttu-id="f9027-288">Limit jest dostarczany przez [konfigurację](xref:fundamentals/configuration/index) z pliku *appSettings. JSON* :</span><span class="sxs-lookup"><span data-stu-id="f9027-288">The limit is supplied via [Configuration](xref:fundamentals/configuration/index) from the *appsettings.json* file:</span></span>

```json
{
  "FileSizeLimit": 2097152
}
```

<span data-ttu-id="f9027-289">@No__t-0 jest wstrzykiwana do klas `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="f9027-289">The `FileSizeLimit` is injected into `PageModel` classes:</span></span>

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

<span data-ttu-id="f9027-290">Gdy rozmiar pliku przekracza limit, plik zostanie odrzucony:</span><span class="sxs-lookup"><span data-stu-id="f9027-290">When a file size exceeds the limit, the file is rejected:</span></span>

```csharp
if (formFile.Length > _fileSizeLimit)
{
    // The file is too large ... discontinue processing the file
}
```

### <a name="match-name-attribute-value-to-parameter-name-of-post-method"></a><span data-ttu-id="f9027-291">Dopasuj wartość atrybutu Name do nazwy parametru metody POST</span><span class="sxs-lookup"><span data-stu-id="f9027-291">Match name attribute value to parameter name of POST method</span></span>

<span data-ttu-id="f9027-292">W formularzach innych niż Razor, które PUBLIKUJą dane formularza lub używają kodu JavaScript `FormData` bezpośrednio, nazwa określona w elemencie formularza lub `FormData` musi być zgodna z nazwą parametru w akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="f9027-292">In non-Razor forms that POST form data or use JavaScript's `FormData` directly, the name specified in the form's element or `FormData` must match the name of the parameter in the controller's action.</span></span>

<span data-ttu-id="f9027-293">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="f9027-293">In the following example:</span></span>

* <span data-ttu-id="f9027-294">W przypadku używania elementu `<input>` atrybut `name` ma ustawioną wartość `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="f9027-294">When using an `<input>` element, the `name` attribute is set to the value `battlePlans`:</span></span>

  ```html
  <input type="file" name="battlePlans" multiple>
  ```

* <span data-ttu-id="f9027-295">W przypadku używania `FormData` w języku JavaScript nazwa jest ustawiona na wartość `battlePlans`:</span><span class="sxs-lookup"><span data-stu-id="f9027-295">When using `FormData` in JavaScript, the name is set to the value `battlePlans`:</span></span>

  ```javascript
  var formData = new FormData();

  for (var file in files) {
    formData.append("battlePlans", file, file.name);
  }
  ```

<span data-ttu-id="f9027-296">Użyj zgodnej nazwy dla parametru C# metody (`battlePlans`):</span><span class="sxs-lookup"><span data-stu-id="f9027-296">Use a matching name for the parameter of the C# method (`battlePlans`):</span></span>

* <span data-ttu-id="f9027-297">Dla metody obsługi strony Razor Pages o nazwie `Upload`:</span><span class="sxs-lookup"><span data-stu-id="f9027-297">For a Razor Pages page handler method named `Upload`:</span></span>

  ```csharp
  public async Task<IActionResult> OnPostUploadAsync(List<IFormFile> battlePlans)
  ```

* <span data-ttu-id="f9027-298">Dla metody akcji składnika MVC Controller:</span><span class="sxs-lookup"><span data-stu-id="f9027-298">For an MVC POST controller action method:</span></span>

  ```csharp
  public async Task<IActionResult> Post(List<IFormFile> battlePlans)
  ```

## <a name="server-and-app-configuration"></a><span data-ttu-id="f9027-299">Konfiguracja serwera i aplikacji</span><span class="sxs-lookup"><span data-stu-id="f9027-299">Server and app configuration</span></span>

### <a name="multipart-body-length-limit"></a><span data-ttu-id="f9027-300">Limit długości treści wieloczęściowej</span><span class="sxs-lookup"><span data-stu-id="f9027-300">Multipart body length limit</span></span>

<span data-ttu-id="f9027-301"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> ustawia limit długości każdej wieloczęściowej treści.</span><span class="sxs-lookup"><span data-stu-id="f9027-301"><xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> sets the limit for the length of each multipart body.</span></span> <span data-ttu-id="f9027-302">Sekcje formularza, które przekraczają ten limit, zgłaszają <xref:System.IO.InvalidDataException> podczas analizowania.</span><span class="sxs-lookup"><span data-stu-id="f9027-302">Form sections that exceed this limit throw an <xref:System.IO.InvalidDataException> when parsed.</span></span> <span data-ttu-id="f9027-303">Wartość domyślna to 134 217 728 (128 MB).</span><span class="sxs-lookup"><span data-stu-id="f9027-303">The default is 134,217,728 (128 MB).</span></span> <span data-ttu-id="f9027-304">Dostosuj limit przy użyciu ustawienia <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f9027-304">Customize the limit using the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> setting in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    services.Configure<FormOptions>(options =>
    {
        // Set the limit to 256 MB
        options.MultipartBodyLengthLimit = 268435456;
    });
}
```

<span data-ttu-id="f9027-305"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> służy do ustawiania <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> dla pojedynczej strony lub akcji.</span><span class="sxs-lookup"><span data-stu-id="f9027-305"><xref:Microsoft.AspNetCore.Mvc.RequestFormLimitsAttribute> is used to set the <xref:Microsoft.AspNetCore.Http.Features.FormOptions.MultipartBodyLengthLimit> for a single page or action.</span></span>

<span data-ttu-id="f9027-306">W aplikacji Razor Pages Zastosuj filtr z [Konwencją](xref:razor-pages/razor-pages-conventions) w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f9027-306">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="f9027-307">W aplikacji Razor Pages lub aplikacji MVC Zastosuj filtr do modelu strony lub metody akcji:</span><span class="sxs-lookup"><span data-stu-id="f9027-307">In a Razor Pages app or an MVC app, apply the filter to the page model or action method:</span></span>

```csharp
// Set the limit to 256 MB
[RequestFormLimits(MultipartBodyLengthLimit = 268435456)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="kestrel-maximum-request-body-size"></a><span data-ttu-id="f9027-308">Maksymalny rozmiar treści żądania Kestrel</span><span class="sxs-lookup"><span data-stu-id="f9027-308">Kestrel maximum request body size</span></span>

<span data-ttu-id="f9027-309">W przypadku aplikacji hostowanych przez Kestrel domyślny maksymalny rozmiar treści żądania to 30 000 000 bajtów, czyli około 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="f9027-309">For apps hosted by Kestrel, the default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span> <span data-ttu-id="f9027-310">Dostosuj limit przy użyciu opcji serwera [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel:</span><span class="sxs-lookup"><span data-stu-id="f9027-310">Customize the limit using the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) Kestrel server option:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        })
        .ConfigureKestrel((context, options) =>
        {
            // Handle requests up to 50 MB
            options.Limits.MaxRequestBodySize = 52428800;
        });
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

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

::: moniker-end

<span data-ttu-id="f9027-311"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> służy do ustawiania [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) dla pojedynczej strony lub akcji.</span><span class="sxs-lookup"><span data-stu-id="f9027-311"><xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> is used to set the [MaxRequestBodySize](xref:fundamentals/servers/kestrel#maximum-request-body-size) for a single page or action.</span></span>

<span data-ttu-id="f9027-312">W aplikacji Razor Pages Zastosuj filtr z [Konwencją](xref:razor-pages/razor-pages-conventions) w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f9027-312">In a Razor Pages app, apply the filter with a [convention](xref:razor-pages/razor-pages-conventions) in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="f9027-313">W aplikacji strony Razor lub aplikacji MVC Zastosuj filtr do klasy procedury obsługi stron lub metody akcji:</span><span class="sxs-lookup"><span data-stu-id="f9027-313">In a Razor pages app or an MVC app, apply the filter to the page handler class or action method:</span></span>

```csharp
// Handle requests up to 50 MB
[RequestSizeLimit(52428800)]
public class BufferedSingleFileUploadPhysicalModel : PageModel
{
    ...
}
```

### <a name="other-kestrel-limits"></a><span data-ttu-id="f9027-314">Inne limity Kestrel</span><span class="sxs-lookup"><span data-stu-id="f9027-314">Other Kestrel limits</span></span>

<span data-ttu-id="f9027-315">Inne limity Kestrel mogą dotyczyć aplikacji hostowanych przez Kestrel:</span><span class="sxs-lookup"><span data-stu-id="f9027-315">Other Kestrel limits may apply for apps hosted by Kestrel:</span></span>

* [<span data-ttu-id="f9027-316">Maksymalna liczba połączeń klienta</span><span class="sxs-lookup"><span data-stu-id="f9027-316">Maximum client connections</span></span>](xref:fundamentals/servers/kestrel#maximum-client-connections)
* [<span data-ttu-id="f9027-317">Stawki danych żądań i odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="f9027-317">Request and response data rates</span></span>](xref:fundamentals/servers/kestrel#minimum-request-body-data-rate)

### <a name="iis-content-length-limit"></a><span data-ttu-id="f9027-318">Limit długości zawartości usług IIS</span><span class="sxs-lookup"><span data-stu-id="f9027-318">IIS content length limit</span></span>

<span data-ttu-id="f9027-319">Domyślny limit żądań (`maxAllowedContentLength`) to 30 000 000 bajtów, czyli około 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="f9027-319">The default request limit (`maxAllowedContentLength`) is 30,000,000 bytes, which is approximately 28.6MB.</span></span> <span data-ttu-id="f9027-320">Dostosuj limit w pliku *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="f9027-320">Customize the limit in the *web.config* file:</span></span>

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

<span data-ttu-id="f9027-321">To ustawienie dotyczy tylko usług IIS.</span><span class="sxs-lookup"><span data-stu-id="f9027-321">This setting only applies to IIS.</span></span> <span data-ttu-id="f9027-322">Zachowanie nie występuje domyślnie podczas hostowania w Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f9027-322">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="f9027-323">Aby uzyskać więcej informacji, zobacz [limity żądań \<requestLimits >](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="f9027-323">For more information, see [Request Limits \<requestLimits>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

<span data-ttu-id="f9027-324">Ograniczenia w module ASP.NET Core lub obecność modułu filtrowania żądań usług IIS mogą ograniczyć przekazywanie do 2 lub 4 GB.</span><span class="sxs-lookup"><span data-stu-id="f9027-324">Limitations in the ASP.NET Core Module or presence of the IIS Request Filtering Module may limit uploads to either 2 or 4 GB.</span></span> <span data-ttu-id="f9027-325">Aby uzyskać więcej informacji, zobacz [nie można przekazać pliku o rozmiarze większym niż 2 GB (ASPNET/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span><span class="sxs-lookup"><span data-stu-id="f9027-325">For more information, see [Unable to upload file greater than 2GB in size (aspnet/AspNetCore #2711)](https://github.com/aspnet/AspNetCore/issues/2711).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="f9027-326">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="f9027-326">Troubleshoot</span></span>

<span data-ttu-id="f9027-327">Poniżej przedstawiono niektóre typowe problemy, które można napotkać podczas pracy z przekazywaniem plików i ich możliwymi rozwiązaniami.</span><span class="sxs-lookup"><span data-stu-id="f9027-327">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="not-found-error-when-deployed-to-an-iis-server"></a><span data-ttu-id="f9027-328">Nie znaleziono błędu w przypadku wdrożenia na serwerze usług IIS</span><span class="sxs-lookup"><span data-stu-id="f9027-328">Not Found error when deployed to an IIS server</span></span>

<span data-ttu-id="f9027-329">Następujący błąd wskazuje, że przekazany plik przekracza skonfigurowaną długość zawartości serwera:</span><span class="sxs-lookup"><span data-stu-id="f9027-329">The following error indicates that the uploaded file exceeds the server's configured content length:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="f9027-330">Aby uzyskać więcej informacji na temat zwiększania limitu, zobacz sekcję [limit długości zawartości usług IIS](#iis-content-length-limit) .</span><span class="sxs-lookup"><span data-stu-id="f9027-330">For more information on increasing the limit, see the [IIS content length limit](#iis-content-length-limit) section.</span></span>

### <a name="connection-failure"></a><span data-ttu-id="f9027-331">Błąd połączenia</span><span class="sxs-lookup"><span data-stu-id="f9027-331">Connection failure</span></span>

<span data-ttu-id="f9027-332">Wystąpił błąd połączenia i połączenie z serwerem resetowania prawdopodobnie wskazuje, że przekazany plik przekracza maksymalny rozmiar treści żądania Kestrel.</span><span class="sxs-lookup"><span data-stu-id="f9027-332">A connection error and a reset server connection probably indicates that the uploaded file exceeds Kestrel's maximum request body size.</span></span> <span data-ttu-id="f9027-333">Aby uzyskać więcej informacji, zobacz sekcję [Maksymalny rozmiar treści żądania Kestrel](#kestrel-maximum-request-body-size) .</span><span class="sxs-lookup"><span data-stu-id="f9027-333">For more information, see the [Kestrel maximum request body size](#kestrel-maximum-request-body-size) section.</span></span> <span data-ttu-id="f9027-334">Limity połączeń klienta Kestrel mogą również wymagać korekty.</span><span class="sxs-lookup"><span data-stu-id="f9027-334">Kestrel client connection limits may also require adjustment.</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="f9027-335">Wyjątek odwołania o wartości null z IFormFile</span><span class="sxs-lookup"><span data-stu-id="f9027-335">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="f9027-336">Jeśli kontroler akceptuje przekazane pliki przy użyciu <xref:Microsoft.AspNetCore.Http.IFormFile>, ale wartość jest `null`, upewnij się, że w formularzu HTML określono wartość `enctype` `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="f9027-336">If the controller is accepting uploaded files using <xref:Microsoft.AspNetCore.Http.IFormFile> but the value is `null`, confirm that the HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="f9027-337">Jeśli ten atrybut nie jest ustawiony dla elementu `<form>`, przekazywanie pliku nie wystąpi i wszystkie powiązane argumenty <xref:Microsoft.AspNetCore.Http.IFormFile> są `null`.</span><span class="sxs-lookup"><span data-stu-id="f9027-337">If this attribute isn't set on the `<form>` element, the file upload doesn't occur and any bound <xref:Microsoft.AspNetCore.Http.IFormFile> arguments are `null`.</span></span> <span data-ttu-id="f9027-338">Sprawdź również, czy [Nazwa przekazywania w postaci danych jest zgodna z nazewnictwem aplikacji](#match-name-attribute-value-to-parameter-name-of-post-method).</span><span class="sxs-lookup"><span data-stu-id="f9027-338">Also confirm that the [upload naming in form data matches the app's naming](#match-name-attribute-value-to-parameter-name-of-post-method).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f9027-339">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f9027-339">Additional resources</span></span>

* [<span data-ttu-id="f9027-340">Przekazywanie pliku bez ograniczeń</span><span class="sxs-lookup"><span data-stu-id="f9027-340">Unrestricted File Upload</span></span>](https://www.owasp.org/index.php/Unrestricted_File_Upload)
* <span data-ttu-id="f9027-341">@no__t — zabezpieczenia 0Azure: Ramka zabezpieczeń: Walidacja danych wejściowych | Środki zaradcze @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="f9027-341">[Azure Security: Security Frame: Input Validation | Mitigations](/azure/security/azure-security-threat-modeling-tool-input-validation)</span></span>
* <span data-ttu-id="f9027-342">Wzorce projektowe w chmurze @no__t 0Azure: Wzorzec klucza portiera @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="f9027-342">[Azure Cloud Design Patterns: Valet Key pattern](/azure/architecture/patterns/valet-key)</span></span>
