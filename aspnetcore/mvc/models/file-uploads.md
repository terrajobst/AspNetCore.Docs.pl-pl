---
title: Przekazywanie plików z platformy ASP.NET Core
author: ardalis
description: Jak używać wiązania modelu i przesyłania strumieniowego do przekazywania plików na platformie ASP.NET Core MVC.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: mvc/models/file-uploads
ms.openlocfilehash: 831f0c84f0ff062e9e24ccbf4ca81b7143c66e66
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59468556"
---
# <a name="file-uploads-in-aspnet-core"></a><span data-ttu-id="ff29e-103">Przekazywanie plików z platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ff29e-103">File uploads in ASP.NET Core</span></span>

<span data-ttu-id="ff29e-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="ff29e-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="ff29e-105">Akcje programu ASP.NET MVC obsługuje przekazywanie jeden lub więcej plików przy użyciu prostego modelu binding na mniejsze pliki lub przesyłania strumieniowego większe pliki.</span><span class="sxs-lookup"><span data-stu-id="ff29e-105">ASP.NET MVC actions support uploading of one or more files using simple model binding for smaller files or streaming for larger files.</span></span>

[<span data-ttu-id="ff29e-106">Wyświetlanie lub pobieranie przykładowy z serwisu GitHub</span><span class="sxs-lookup"><span data-stu-id="ff29e-106">View or download sample from GitHub</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a><span data-ttu-id="ff29e-107">Przekazywanie małych plików z wiązania modelu</span><span class="sxs-lookup"><span data-stu-id="ff29e-107">Uploading small files with model binding</span></span>

<span data-ttu-id="ff29e-108">Aby przekazać małych plików, możesz użyć formularza HTML wieloczęściowej lub utworzyć żądanie POST, przy użyciu języka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ff29e-108">To upload small files, you can use a multi-part HTML form or construct a POST request using JavaScript.</span></span> <span data-ttu-id="ff29e-109">Przykładowego formularza przy użyciu Razor, który obsługuje wiele plików przekazanych, znajdują się poniżej:</span><span class="sxs-lookup"><span data-stu-id="ff29e-109">An example form using Razor, which supports multiple uploaded files, is shown below:</span></span>

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple>
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload">
        </div>
    </div>
</form>
```

<span data-ttu-id="ff29e-110">Aby zapewnić obsługę przekazywania plików, należy określić formularzy HTML `enctype` z `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="ff29e-110">In order to support file uploads, HTML forms must specify an `enctype` of `multipart/form-data`.</span></span> <span data-ttu-id="ff29e-111">`files` Elementu wejściowego powyżej obsługuje przekazywania wielu plików.</span><span class="sxs-lookup"><span data-stu-id="ff29e-111">The `files` input element shown above supports uploading multiple files.</span></span> <span data-ttu-id="ff29e-112">Pomiń `multiple` atrybut ten element input, aby zezwolić na pojedynczy plik do przekazania.</span><span class="sxs-lookup"><span data-stu-id="ff29e-112">Omit the `multiple` attribute on this input element to allow just a single file to be uploaded.</span></span> <span data-ttu-id="ff29e-113">Powyższe znaczników powoduje wyświetlenie w przeglądarce jako:</span><span class="sxs-lookup"><span data-stu-id="ff29e-113">The above markup renders in a browser as:</span></span>

![Formularz przekazywania pliku](file-uploads/_static/upload-form.png)

<span data-ttu-id="ff29e-115">Poszczególnych plików, które są przekazywane do serwera jest możliwy za pośrednictwem [powiązań modelu](xref:mvc/models/model-binding) przy użyciu [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) interfejsu.</span><span class="sxs-lookup"><span data-stu-id="ff29e-115">The individual files uploaded to the server can be accessed through [Model Binding](xref:mvc/models/model-binding) using the [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) interface.</span></span> <span data-ttu-id="ff29e-116">`IFormFile` ma następującą strukturę:</span><span class="sxs-lookup"><span data-stu-id="ff29e-116">`IFormFile` has the following structure:</span></span>

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
> <span data-ttu-id="ff29e-117">Nie należy polegać na ani zaufania `FileName` właściwości bez sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="ff29e-117">Don't rely on or trust the `FileName` property without validation.</span></span> <span data-ttu-id="ff29e-118">`FileName` Właściwość powinna być używana tylko w celach wyświetlania.</span><span class="sxs-lookup"><span data-stu-id="ff29e-118">The `FileName` property should only be used for display purposes.</span></span>

<span data-ttu-id="ff29e-119">Podczas przekazywania plików przy użyciu wiązania modelu i `IFormFile` interfejsu, metoda akcji może akceptować może to być pojedynczy `IFormFile` lub `IEnumerable<IFormFile>` (lub `List<IFormFile>`) reprezentujący kilka plików.</span><span class="sxs-lookup"><span data-stu-id="ff29e-119">When uploading files using model binding and the `IFormFile` interface, the action method can accept either a single `IFormFile` or an `IEnumerable<IFormFile>` (or `List<IFormFile>`) representing several files.</span></span> <span data-ttu-id="ff29e-120">Poniższy przykład jeden lub więcej plików przekazanych w pętli, zapisuje je w lokalnym systemie plików i zwraca całkowitą liczbę i rozmiar przekazywanych plików.</span><span class="sxs-lookup"><span data-stu-id="ff29e-120">The following example loops through one or more uploaded files, saves them to the local file system, and returns the total number and size of files uploaded.</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

<span data-ttu-id="ff29e-121">Przekazane przy użyciu `IFormFile` technika są buforowane w pamięci lub na dysku na serwerze sieci web przed przetworzeniem.</span><span class="sxs-lookup"><span data-stu-id="ff29e-121">Files uploaded using the `IFormFile` technique are buffered in memory or on disk on the web server before being processed.</span></span> <span data-ttu-id="ff29e-122">Wewnątrz metody akcji `IFormFile` zawartość jest dostępna jako strumień.</span><span class="sxs-lookup"><span data-stu-id="ff29e-122">Inside the action method, the `IFormFile` contents are accessible as a stream.</span></span> <span data-ttu-id="ff29e-123">Oprócz lokalnego systemu plików, pliki mogą być przesłana strumieniowo do [usługi Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs) lub [Entity Framework](/ef/core/index).</span><span class="sxs-lookup"><span data-stu-id="ff29e-123">In addition to the local file system, files can be streamed to [Azure Blob storage](/azure/visual-studio/vs-storage-aspnet5-getting-started-blobs) or [Entity Framework](/ef/core/index).</span></span>

<span data-ttu-id="ff29e-124">Aby przechowywać dane plik binarny w bazie danych przy użyciu platformy Entity Framework, należy zdefiniować właściwości typu `byte[]` jednostki:</span><span class="sxs-lookup"><span data-stu-id="ff29e-124">To store binary file data in a database using Entity Framework, define a property of type `byte[]` on the entity:</span></span>

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

<span data-ttu-id="ff29e-125">Określ właściwość viewmodel typu `IFormFile`:</span><span class="sxs-lookup"><span data-stu-id="ff29e-125">Specify a viewmodel property of type `IFormFile`:</span></span>

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> <span data-ttu-id="ff29e-126">`IFormFile` można bezpośrednio jako parametru metody akcji lub właściwością viewmodel jak pokazano powyżej.</span><span class="sxs-lookup"><span data-stu-id="ff29e-126">`IFormFile` can be used directly as an action method parameter or as a viewmodel property, as shown above.</span></span>

<span data-ttu-id="ff29e-127">Kopiuj `IFormFile` do strumienia, a następnie zapisz go do tablicy typu byte:</span><span class="sxs-lookup"><span data-stu-id="ff29e-127">Copy the `IFormFile` to a stream and save it to the byte array:</span></span>

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
        var user = new ApplicationUser 
        {
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
> <span data-ttu-id="ff29e-128">Należy zachować ostrożność podczas zapisywania danych binarnych w relacyjnych baz danych, zgodnie z jego jest niekorzystnie wpłynąć na wydajność.</span><span class="sxs-lookup"><span data-stu-id="ff29e-128">Use caution when storing binary data in relational databases, as it can adversely impact performance.</span></span>

## <a name="uploading-large-files-with-streaming"></a><span data-ttu-id="ff29e-129">Przekazywanie dużych plików za pomocą przesyłania strumieniowego</span><span class="sxs-lookup"><span data-stu-id="ff29e-129">Uploading large files with streaming</span></span>

<span data-ttu-id="ff29e-130">Jeśli rozmiar lub częstotliwość wysyłania plików powoduje problemy z zasobami dla aplikacji, należy wziąć pod uwagę przesyłania strumieniowego przekazywania pliku, a nie buforowania go w całości, tak jak powyżej podejście powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="ff29e-130">If the size or frequency of file uploads is causing resource problems for the app, consider streaming the file upload rather than buffering it in its entirety, as the model binding approach shown above does.</span></span> <span data-ttu-id="ff29e-131">Podczas korzystania z `IFormFile` i wiązania modelu jest dużo prostsze rozwiązaniem, przesyłanie strumieniowe wymaga kilku kroków dotyczących implementacji prawidłowo.</span><span class="sxs-lookup"><span data-stu-id="ff29e-131">While using `IFormFile` and model binding is a much simpler solution, streaming requires a number of steps to implement properly.</span></span>

> [!NOTE]
> <span data-ttu-id="ff29e-132">Pojedynczy plik buforowany przekraczającym 64KB zostanie przeniesiony z pamięci RAM w pliku tymczasowym na dysku, na serwerze.</span><span class="sxs-lookup"><span data-stu-id="ff29e-132">Any single buffered file exceeding 64KB will be moved from RAM to a temp file on disk on the server.</span></span> <span data-ttu-id="ff29e-133">Zasoby (dysk, pamięci RAM) używany przez operacje przekazywania plików zależą od liczby i rozmiaru współbieżne przekazania plików.</span><span class="sxs-lookup"><span data-stu-id="ff29e-133">The resources (disk, RAM) used by file uploads depend on the number and size of concurrent file uploads.</span></span> <span data-ttu-id="ff29e-134">Przesyłanie strumieniowe nie jest tak wiele dotyczące wydajności, chodzi o skali.</span><span class="sxs-lookup"><span data-stu-id="ff29e-134">Streaming isn't so much about perf, it's about scale.</span></span> <span data-ttu-id="ff29e-135">Jeśli spróbujesz buforu przekazywania zbyt wiele lokacji ulegnie awarii, gdy zabraknie mu pamięci lub miejsca na dysku.</span><span class="sxs-lookup"><span data-stu-id="ff29e-135">If you try to buffer too many uploads, your site will crash when it runs out of memory or disk space.</span></span>

<span data-ttu-id="ff29e-136">Poniższy przykład pokazuje, przy użyciu języka JavaScript/Angular do przesyłania strumieniowego do akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="ff29e-136">The following example demonstrates using JavaScript/Angular to stream to a controller action.</span></span> <span data-ttu-id="ff29e-137">Token antiforgery pliku jest generowana za pomocą niestandardowego atrybutu filtru i przekazywane w nagłówkach HTTP, a nie w treści żądania.</span><span class="sxs-lookup"><span data-stu-id="ff29e-137">The file's antiforgery token is generated using a custom filter attribute and passed in HTTP headers instead of in the request body.</span></span> <span data-ttu-id="ff29e-138">Ponieważ metody akcji, które bezpośrednio przetwarza dane przekazane, wiązanie modelu została wyłączona przez inny filtr.</span><span class="sxs-lookup"><span data-stu-id="ff29e-138">Because the action method processes the uploaded data directly, model binding is disabled by another filter.</span></span> <span data-ttu-id="ff29e-139">W akcji, zawartość formularza są odczytywane przy użyciu `MultipartReader`, która odczytuje poszczególnym `MultipartSection`, przetwarzanie pliku lub zawartość jest przechowywana zgodnie z potrzebami.</span><span class="sxs-lookup"><span data-stu-id="ff29e-139">Within the action, the form's contents are read using a `MultipartReader`, which reads each individual `MultipartSection`, processing the file or storing the contents as appropriate.</span></span> <span data-ttu-id="ff29e-140">Po przeczytaniu wszystkie sekcje akcji wykonuje swoje własne powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="ff29e-140">Once all sections have been read, the action performs its own model binding.</span></span>

<span data-ttu-id="ff29e-141">Akcja początkowa ładuje formularz i tokenu antiforgery są zapisywane w pliku cookie (za pośrednictwem `GenerateAntiforgeryTokenCookieForAjax` atrybutu):</span><span class="sxs-lookup"><span data-stu-id="ff29e-141">The initial action loads the form and saves an antiforgery token in a cookie (via the `GenerateAntiforgeryTokenCookieForAjax` attribute):</span></span>

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

<span data-ttu-id="ff29e-142">Ten atrybut używa wbudowanego platformy ASP.NET Core [Antiforgery](xref:security/anti-request-forgery) pomocy technicznej można ustawić pliku cookie z token żądania:</span><span class="sxs-lookup"><span data-stu-id="ff29e-142">The attribute uses ASP.NET Core's built-in [Antiforgery](xref:security/anti-request-forgery) support to set a cookie with a request token:</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

<span data-ttu-id="ff29e-143">Platformy Angular automatycznie przekazuje antiforgery token w nagłówku żądania, o nazwie `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="ff29e-143">Angular automatically passes an antiforgery token in a request header named `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="ff29e-144">Aplikacja ASP.NET Core MVC jest skonfigurowana do odwoływania się do tego pliku nagłówkowego w jej konfiguracji w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ff29e-144">The ASP.NET Core MVC app is configured to refer to this header in its configuration in *Startup.cs*:</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

<span data-ttu-id="ff29e-145">`DisableFormValueModelBinding` Atrybutu, pokazano poniżej, jest używana do wyłączenia wiązania modelu dla `Upload` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="ff29e-145">The `DisableFormValueModelBinding` attribute, shown below, is used to disable model binding for the `Upload` action method.</span></span>

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

<span data-ttu-id="ff29e-146">Ponieważ wiązania modelu jest wyłączona, `Upload` metody akcji nie akceptuje parametrów.</span><span class="sxs-lookup"><span data-stu-id="ff29e-146">Since model binding is disabled, the `Upload` action method doesn't accept parameters.</span></span> <span data-ttu-id="ff29e-147">Współpracuje bezpośrednio z `Request` właściwość `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="ff29e-147">It works directly with the `Request` property of `ControllerBase`.</span></span> <span data-ttu-id="ff29e-148">A `MultipartReader` służy do odczytywania każdej sekcji.</span><span class="sxs-lookup"><span data-stu-id="ff29e-148">A `MultipartReader` is used to read each section.</span></span> <span data-ttu-id="ff29e-149">Plik zostanie zapisany pod nazwą identyfikator GUID i dane klucz wartość są przechowywane w `KeyValueAccumulator`.</span><span class="sxs-lookup"><span data-stu-id="ff29e-149">The file is saved with a GUID filename and the key/value data is stored in a `KeyValueAccumulator`.</span></span> <span data-ttu-id="ff29e-150">Po przeczytaniu wszystkie sekcje zawartości `KeyValueAccumulator` są używane do powiązania danych formularza typu modelu.</span><span class="sxs-lookup"><span data-stu-id="ff29e-150">Once all sections have been read, the contents of the `KeyValueAccumulator` are used to bind the form data to a model type.</span></span>

<span data-ttu-id="ff29e-151">Pełne `Upload` metoda znajdują się poniżej:</span><span class="sxs-lookup"><span data-stu-id="ff29e-151">The complete `Upload` method is shown below:</span></span>

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a><span data-ttu-id="ff29e-152">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="ff29e-152">Troubleshooting</span></span>

<span data-ttu-id="ff29e-153">Poniżej przedstawiono niektóre typowe problemy występujące podczas pracy z przekazywania plików i ich możliwe rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="ff29e-153">Below are some common problems encountered when working with uploading files and their possible solutions.</span></span>

### <a name="unexpected-not-found-error-with-iis"></a><span data-ttu-id="ff29e-154">Nieoczekiwany błąd nie znaleziono za pomocą programu IIS</span><span class="sxs-lookup"><span data-stu-id="ff29e-154">Unexpected Not Found error with IIS</span></span>

<span data-ttu-id="ff29e-155">Następujący błąd wskazuje, przekazywanie pliku przekracza serwera skonfigurowany przez `maxAllowedContentLength`:</span><span class="sxs-lookup"><span data-stu-id="ff29e-155">The following error indicates your file upload exceeds the server's configured `maxAllowedContentLength`:</span></span>

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

<span data-ttu-id="ff29e-156">Ustawieniem domyślnym jest `30000000`, czyli około 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="ff29e-156">The default setting is `30000000`, which is approximately 28.6MB.</span></span> <span data-ttu-id="ff29e-157">Wartość można dostosować, edytując *web.config*:</span><span class="sxs-lookup"><span data-stu-id="ff29e-157">The value can be customized by editing *web.config*:</span></span>

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

<span data-ttu-id="ff29e-158">To ustawienie dotyczy tylko usług IIS.</span><span class="sxs-lookup"><span data-stu-id="ff29e-158">This setting only applies to IIS.</span></span> <span data-ttu-id="ff29e-159">Zachowanie nie występuje domyślnie w przypadku hostowania w Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ff29e-159">The behavior doesn't occur by default when hosting on Kestrel.</span></span> <span data-ttu-id="ff29e-160">Aby uzyskać więcej informacji, zobacz [limity żądań \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span><span class="sxs-lookup"><span data-stu-id="ff29e-160">For more information, see [Request Limits \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).</span></span>

### <a name="null-reference-exception-with-iformfile"></a><span data-ttu-id="ff29e-161">Wyjątek odwołania o wartości null z IFormFile</span><span class="sxs-lookup"><span data-stu-id="ff29e-161">Null Reference Exception with IFormFile</span></span>

<span data-ttu-id="ff29e-162">Jeśli kontroler jest przyjmuje przekazane pliki przy użyciu `IFormFile` , ale możesz znaleźć, wartość jest zawsze wartość null, upewnij się, że formularza HTML jest określenie `enctype` wartość `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="ff29e-162">If your controller is accepting uploaded files using `IFormFile` but you find that the value is always null, confirm that your HTML form is specifying an `enctype` value of `multipart/form-data`.</span></span> <span data-ttu-id="ff29e-163">Jeśli ten atrybut nie jest ustawiony na `<form>` element nie zostanie wykonana, przekazanie pliku i wszystkie powiązane z `IFormFile` argumenty będą miały wartość null.</span><span class="sxs-lookup"><span data-stu-id="ff29e-163">If this attribute isn't set on the `<form>` element, the file upload won't occur and any bound `IFormFile` arguments will be null.</span></span>
