---
title: "W przypadku platformy ASP.NET Core przekazywania plików"
author: ardalis
description: "Jak używać wiązania modelu i przesyłania strumieniowego przekazywania plików w programie ASP.NET MVC Core."
keywords: "Platformy ASP.NET Core przekazywania pliku modelu powiązania IFormFile przesyłania strumieniowego"
ms.author: riande
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.assetid: ebc98159-a028-4a94-b06c-43981c79c6be
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/file-uploads
ms.openlocfilehash: e8608a46d6688df8da6c665a25b6f4db5f480461
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="file-uploads-in-aspnet-core"></a><span data-ttu-id="3978a-104">W przypadku platformy ASP.NET Core przekazywania plików</span><span class="sxs-lookup"><span data-stu-id="3978a-104">File uploads in ASP.NET Core</span></span>

<span data-ttu-id="3978a-105">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="3978a-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="3978a-106">Akcje ASP.NET MVC obsługuje przekazywanie jednego lub więcej plików za pomocą prostego modelu powiązanie mniejsze pliki lub przesyłanie strumieniowe do większych plików.</span><span class="sxs-lookup"><span data-stu-id="3978a-106">ASP.NET MVC actions support uploading of one or more files using simple model binding for smaller files or streaming for larger files.</span></span>

[<span data-ttu-id="3978a-107">Przykładowy widok lub pobrania z witryny GitHub</span><span class="sxs-lookup"><span data-stu-id="3978a-107">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a><span data-ttu-id="3978a-108">Przekazywanie małych plików z wiązania modelu</span><span class="sxs-lookup"><span data-stu-id="3978a-108">Uploading small files with model binding</span></span>

<span data-ttu-id="3978a-109">Aby przekazać małych plików, można użyć formularza HTML wieloczęściowej lub utworzenia żądania POST przy użyciu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3978a-109">To upload small files, you can use a multi-part HTML form or construct a POST request using JavaScript.</span></span> <span data-ttu-id="3978a-110">Formularz przykład przy użyciu Razor, który obsługuje wielu przekazanych plików, przedstawiono poniżej:</span><span class="sxs-lookup"><span data-stu-id="3978a-110">An example form using Razor, which supports multiple uploaded files, is shown below:</span></span>

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple />
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload" />
        </div>
    </div>
</form>
```

<span data-ttu-id="3978a-111">Aby zapewnić obsługę przekazywania plików, należy określić formularzy HTML `enctype` z `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="3978a-111">In order to support file uploads, HTML forms must specify an `enctype` of `multipart/form-data`.</span></span> <span data-ttu-id="3978a-112">`files` Elementu wejściowego pokazanym powyżej obsługuje przekazywania wielu plików.</span><span class="sxs-lookup"><span data-stu-id="3978a-112">The `files` input element shown above supports uploading multiple files.</span></span> <span data-ttu-id="3978a-113">Pomiń `multiple` atrybut w tym elemencie wejściowych, aby zezwolić tylko pojedynczy plik do przekazania.</span><span class="sxs-lookup"><span data-stu-id="3978a-113">Omit the `multiple` attribute on this input element to allow just a single file to be uploaded.</span></span> <span data-ttu-id="3978a-114">Renderuje kod znaczników powyżej w przeglądarce jako:</span><span class="sxs-lookup"><span data-stu-id="3978a-114">The above markup renders in a browser as:</span></span>

![Formularz przekazywania pliku](file-uploads/_static/upload-form.png)

<span data-ttu-id="3978a-116">Poszczególnych przekazany na serwer plików jest możliwy za pośrednictwem [powiązanie modelu](xref:mvc/models/model-binding) przy użyciu [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) interfejsu.</span><span class="sxs-lookup"><span data-stu-id="3978a-116">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using the [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) interface.</span></span> <span data-ttu-id="3978a-117">`IFormFile`ma następującą strukturę:</span><span class="sxs-lookup"><span data-stu-id="3978a-117">`IFormFile` has the following structure:</span></span>

```csharp
public interface IFormFile
{
    string ContentType { get; }
    string ContentDisposition { get; }
    IHeaderDictionary Headers { get; }
    long Length { get; }
    string Name { get; }
    string FileName { get; }
    Stream OpenReadStream();
    void CopyTo(Stream target);
    Task CopyToAsync(Stream target, CancellationToken cancellationToken = null);
}
```

> [!WARNING]
> <span data-ttu-id="3978a-118">Nie zależą od lub zaufania `FileName` właściwości bez sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="3978a-118">Don't rely on or trust the `FileName` property without validation.</span></span> <span data-ttu-id="3978a-119">`FileName` Właściwość powinna być używana tylko w celach wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="3978a-119">The `FileName` property should only be used for display purposes.</span></span>

<span data-ttu-id="3978a-120">Podczas przekazywania plików przy użyciu wiązania modelu i `IFormFile` interfejsu, metoda akcji może akceptować pojedyncze `IFormFile` lub `IEnumerable<IFormFile>` (lub `List<IFormFile>`) reprezentujący kilka plików.</span><span class="sxs-lookup"><span data-stu-id="3978a-120">When uploading files using model binding and the `IFormFile` interface, the action method can accept either a single `IFormFile` or an `IEnumerable<IFormFile>` (or `List<IFormFile>`) representing several files.</span></span> <span data-ttu-id="3978a-121">Poniższy przykład jeden lub więcej plików przekazanego w pętli, zapisuje je w lokalnym systemie plików, a następnie zwraca całkowitą liczbę i rozmiar przekazywanych plików.</span><span class="sxs-lookup"><span data-stu-id="3978a-121">The following example loops through one or more uploaded files, saves them to the local file system, and returns the total number and size of files uploaded.</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

<span data-ttu-id="3978a-122">Pliki przesłane za pomocą `IFormFile` technika są buforowane w pamięci lub na dysku na serwerze sieci web przed przetworzeniem.</span><span class="sxs-lookup"><span data-stu-id="3978a-122">Files uploaded using the `IFormFile` technique are buffered in memory or on disk on the web server before being processed.</span></span> <span data-ttu-id="3978a-123">Wewnątrz metody akcji `IFormFile` zawartość jest dostępna jako strumień.</span><span class="sxs-lookup"><span data-stu-id="3978a-123">Inside the action method, the `IFormFile` contents are accessible as a stream.</span></span> <span data-ttu-id="3978a-124">Oprócz lokalnego systemu plików, pliki mogą być przesyłane strumieniowo do [magazynu obiektów Blob Azure](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) lub [Entity Framework](https://docs.microsoft.com/ef/core/index).</span><span class="sxs-lookup"><span data-stu-id="3978a-124">In addition to the local file system, files can be streamed to [Azure Blob storage](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) or [Entity Framework](https://docs.microsoft.com/ef/core/index).</span></span>

<span data-ttu-id="3978a-125">Aby przechowywać dane pliku binarnego w bazie danych przy użyciu programu Entity Framework, Zdefiniuj właściwość typu `byte[]` jednostki:</span><span class="sxs-lookup"><span data-stu-id="3978a-125">To store binary file data in a database using Entity Framework, define a property of type `byte[]` on the entity:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

<span data-ttu-id="3978a-126">Określ właściwość viewmodel typu `IFormFile`:</span><span class="sxs-lookup"><span data-stu-id="3978a-126">Specify a viewmodel property of type `IFormFile`:</span></span>

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="3978a-127">`IFormFile`można bezpośrednio jako parametr metody akcji lub właściwością viewmodel, jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="3978a-127">`IFormFile` can be used directly as an action method parameter or as a viewmodel property, as shown above.</span></span>

<span data-ttu-id="3978a-128">Kopiuj `IFormFile` do strumienia i zapisać go do tablicy typu byte:</span><span class="sxs-lookup"><span data-stu-id="3978a-128">Copy the `IFormFile` to a stream and save it to the byte array:</span></span>

```csharp
// POST: /Account/Register
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Register(RegisterViewModel model)
{
    ViewData["ReturnUrl"] = returnUrl;
    if  (ModelState.IsValid)
    {
        var user = new ApplicationUser {
          UserName = model.Email,
          Email = model.Email
        };
        using (var memoryStream = new MemoryStream())
        {
            await model.AvatarImage.CopyToAsync(memoryStream);
            user.AvatarImage = memoryStream.ToArray();
        }
    // additional logic omitted
    
    // Don't rely on or trust the model.AvatarImage.FileName property 
    // without validation.
}
```

> [!NOTE]
> <span data-ttu-id="3978a-129">Należy zachować ostrożność podczas zapisywania danych binarnych w relacyjnych baz danych, zgodnie z jego może niekorzystnie wpłynąć na wydajność.</span><span class="sxs-lookup"><span data-stu-id="3978a-129">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>

## <a name="uploading-large-files-with-streaming"></a><span data-ttu-id="3978a-130">Przekazywanie dużych plików z przesyłania strumieniowego</span><span class="sxs-lookup"><span data-stu-id="3978a-130">Uploading large files with streaming</span></span>

<span data-ttu-id="3978a-131">Jeśli rozmiar i częstotliwość wysyłania plików powoduje problemy zasobów dla aplikacji, należy wziąć pod uwagę przesyłania strumieniowego przekazywania pliku, a nie buforowanie go w całości, tak jak w powyższym podejście powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="3978a-131">If the size or frequency of file uploads is causing resource problems for the app, consider streaming the file upload rather than buffering it in its entirety, as the model binding approach shown above does.</span></span> <span data-ttu-id="3978a-132">Podczas używania `IFormFile` i wiązania modelu jest dużo prostsze rozwiązania, przesyłanie strumieniowe wymaga kilku kroków, aby prawidłowo zaimplementować.</span><span class="sxs-lookup"><span data-stu-id="3978a-132">While using `IFormFile` and model binding is a much simpler solution, streaming requires a number of steps to implement properly.</span></span>

> [!NOTE]
> <span data-ttu-id="3978a-133">Pojedynczy plik buforowany przekraczającej 64KB zostaną przeniesione z pamięci RAM do tymczasowego pliku na dysku na serwerze.</span><span class="sxs-lookup"><span data-stu-id="3978a-133">Any single buffered file exceeding 64KB will be moved from RAM to a temp file on disk on the server.</span></span> <span data-ttu-id="3978a-134">Zasoby (dysk, pamięci RAM) używany przez przekazywania plików są zależne od liczby i rozmiaru przekazywania plików współbieżnych.</span><span class="sxs-lookup"><span data-stu-id="3978a-134">The resources (disk, RAM) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="3978a-135">Przesyłania strumieniowego nie jest tak wiele o wydajności, chodzi o skali.</span><span class="sxs-lookup"><span data-stu-id="3978a-135">Streaming is not so much about perf, it's about scale.</span></span> <span data-ttu-id="3978a-136">Jeśli spróbujesz buforu przekazywania zbyt wiele lokacji ulegnie awarii, gdy zabraknie mu pamięci lub miejsca na dysku.</span><span class="sxs-lookup"><span data-stu-id="3978a-136">If you try to buffer too many uploads, your site will crash when it runs out of memory or disk space.</span></span>

<span data-ttu-id="3978a-137">W poniższym przykładzie pokazano, przy użyciu języka JavaScript/kątową do strumienia do akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3978a-137">The following example demonstrates using JavaScript/Angular to stream to a controller action.</span></span> <span data-ttu-id="3978a-138">Token antiforgery pliku jest generowany przy użyciu atrybutu niestandardowego filtru i przekazywane w nagłówkach HTTP zamiast w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="3978a-138">The file's antiforgery token is generated using a custom filter attribute and passed in HTTP headers instead of in the request body.</span></span> <span data-ttu-id="3978a-139">Ponieważ metoda akcji przetwarza dane przekazane bezpośrednio, tworzenia powiązania modelu została wyłączona przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="3978a-139">Because the action method processes the uploaded data directly, model binding is disabled by another filter.</span></span> <span data-ttu-id="3978a-140">W ramach tej akcji, zawartość formularza są odczytywane przy użyciu `MultipartReader`, która odczytuje na poszczególnych `MultipartSection`, przetwarzanie pliku lub zapisywanie zawartości zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="3978a-140">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="3978a-141">Po przeczytaniu wszystkie sekcje akcję wykonuje wiązaniu modelu.</span><span class="sxs-lookup"><span data-stu-id="3978a-141">Once all sections have been read, the action performs its own model binding.</span></span>

<span data-ttu-id="3978a-142">Pierwsze załaduje formularz i antiforgery token jest zapisywany w pliku cookie (za pośrednictwem `GenerateAntiforgeryTokenCookieForAjax` atrybut):</span><span class="sxs-lookup"><span data-stu-id="3978a-142">The initial action loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieForAjax` attribute):</span></span>

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

<span data-ttu-id="3978a-143">Atrybut korzysta z wbudowanej platformy ASP.NET Core [Antiforgery](xref:security/anti-request-forgery) pomocy technicznej, aby ustawić pliku cookie z żądania tokenu:</span><span class="sxs-lookup"><span data-stu-id="3978a-143">The attribute uses ASP.NET Core's built-in [Antiforgery](xref:security/anti-request-forgery) support to set a cookie with a request token:</span></span>

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

<span data-ttu-id="3978a-144">Kątową automatycznie przekazuje antiforgery tokenu w nagłówku żądania o nazwie `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="3978a-144">Angular automatically passes an antiforgery token in a request header named `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="3978a-145">Aplikacji platformy ASP.NET Core MVC jest skonfigurowany do odwoływania się do tego nagłówka w jej konfiguracji w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3978a-145">The ASP.NET Core MVC app is configured to refer to this header in its configuration in *Startup.cs*:</span></span>

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

<span data-ttu-id="3978a-146">`DisableFormValueModelBinding` Atrybutu, pokazano poniżej, jest używana do wyłączenia wiązania modelu dla `Upload` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="3978a-146">The `DisableFormValueModelBinding` attribute, shown below, is used to disable model binding for the `Upload` action method.</span></span>

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

<span data-ttu-id="3978a-147">Wiązanie modelu jest wyłączona, `Upload` metody akcji nie akceptuje parametrów.</span><span class="sxs-lookup"><span data-stu-id="3978a-147">Since model binding is disabled, the `Upload` action method doesn't accept parameters.</span></span> <span data-ttu-id="3978a-148">Działa on bezpośrednio z `Request` właściwość `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="3978a-148">It works directly with the `Request` property of `ControllerBase`.</span></span> <span data-ttu-id="3978a-149">A `MultipartReader` jest używany do odczytu każdej sekcji.</span><span class="sxs-lookup"><span data-stu-id="3978a-149">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="3978a-150">Plik zostanie zapisany pod nazwą identyfikator GUID i dane klucza i wartości są przechowywane w `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="3978a-150">The file is saved with a GUID filename and the key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="3978a-151">Po przeczytaniu wszystkie sekcje zawartości `KeyValueAccumulator` są używane dla wiązania danych formularza typu modelu.</span><span class="sxs-lookup"><span data-stu-id="3978a-151">Once all sections have been read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="3978a-152">Pełną `Upload` metody jest pokazany poniżej:</span><span class="sxs-lookup"><span data-stu-id="3978a-152">The complete `Upload` method is shown below:</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a><span data-ttu-id="3978a-153">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="3978a-153">Troubleshooting</span></span>

<span data-ttu-id="3978a-154">Poniżej przedstawiono niektóre typowe problemy występujące podczas pracy z przekazywania plików i ich możliwe rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="3978a-154">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="unexpected-not-found-error-with-iis"></a><span data-ttu-id="3978a-155">Nieoczekiwany błąd nie znaleziono z usługami IIS</span><span class="sxs-lookup"><span data-stu-id="3978a-155">Unexpected Not Found error with IIS</span></span>

<span data-ttu-id="3978a-156">Następujący błąd wskazuje przekazania pliku przekracza serwera skonfigurowana `maxAllowedContentLength`:</span><span class="sxs-lookup"><span data-stu-id="3978a-156">The following error indicates your file upload exceeds the server's configured `maxAllowedContentLength`:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="3978a-157">Ustawieniem domyślnym jest `30000000`, czyli około 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="3978a-157">The default setting is `30000000`, which is approximately 28.6MB.</span></span> <span data-ttu-id="3978a-158">Wartość można dostosować, edytując *web.config*:</span><span class="sxs-lookup"><span data-stu-id="3978a-158">The value can be customized by editing *web.config*:</span></span>

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- This will handle requests up to 50MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

<span data-ttu-id="3978a-159">To ustawienie dotyczy tylko programu IIS.</span><span class="sxs-lookup"><span data-stu-id="3978a-159">This setting only applies to IIS.</span></span> <span data-ttu-id="3978a-160">To zachowanie nie występuje domyślnie odnośnie do hostowania na Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3978a-160">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="3978a-161">Aby uzyskać więcej informacji, zobacz [limity żądań \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="3978a-161">For more information, see [Request Limits \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="3978a-162">Wyjątek odwołania zerowego z IFormFile</span><span class="sxs-lookup"><span data-stu-id="3978a-162">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="3978a-163">Jeśli kontroler jest akceptują przekazać pliki przy użyciu `IFormFile` , ale możesz znaleźć wartość zawsze ma wartość null, upewnij się, że formularza HTML jest określenie `enctype` wartość `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="3978a-163">If your controller is accepting uploaded files using `IFormFile` but you find that the value is always null, confirm that your HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="3978a-164">Jeśli ten atrybut nie jest ustawiony na `<form>` elementu przekazywania plików nie zostanie przeprowadzona i wszelkie powiązane z `IFormFile` argumentów będzie mieć wartość null.</span><span class="sxs-lookup"><span data-stu-id="3978a-164">If this attribute is not set on the `<form>` element, the file upload will not occur and any bound `IFormFile` arguments will be null.</span></span>
