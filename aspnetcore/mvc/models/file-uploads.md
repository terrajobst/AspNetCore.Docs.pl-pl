---
title: "W przypadku platformy ASP.NET Core przekazywania plików"
author: ardalis
description: "Jak używać wiązania modelu i przesyłania strumieniowego przekazywania plików w programie ASP.NET MVC Core."
ms.author: riande
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/file-uploads
ms.openlocfilehash: 3c5abe84a5c7cc399e0586e680a414fab7a26c1d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="file-uploads-in-aspnet-core"></a>W przypadku platformy ASP.NET Core przekazywania plików

Przez [Steve Smith](https://ardalis.com/)

Akcje ASP.NET MVC obsługuje przekazywanie jednego lub więcej plików za pomocą prostego modelu powiązanie mniejsze pliki lub przesyłanie strumieniowe do większych plików.

[Przykładowy widok lub pobrania z witryny GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a>Przekazywanie małych plików z wiązania modelu

Aby przekazać małych plików, można użyć formularza HTML wieloczęściowej lub utworzenia żądania POST przy użyciu języka JavaScript. Formularz przykład przy użyciu Razor, który obsługuje wielu przekazanych plików, przedstawiono poniżej:

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

Aby zapewnić obsługę przekazywania plików, należy określić formularzy HTML `enctype` z `multipart/form-data`. `files` Elementu wejściowego pokazanym powyżej obsługuje przekazywania wielu plików. Pomiń `multiple` atrybut w tym elemencie wejściowych, aby zezwolić tylko pojedynczy plik do przekazania. Renderuje kod znaczników powyżej w przeglądarce jako:

![Formularz przekazywania pliku](file-uploads/_static/upload-form.png)

Poszczególnych przekazany na serwer plików jest możliwy za pośrednictwem [powiązanie modelu](xref:mvc/models/model-binding) przy użyciu [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) interfejsu. `IFormFile`ma następującą strukturę:

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
> Nie zależą od lub zaufania `FileName` właściwości bez sprawdzania poprawności. `FileName` Właściwość powinna być używana tylko w celach wyświetlania.

Podczas przekazywania plików przy użyciu wiązania modelu i `IFormFile` interfejsu, metoda akcji może akceptować pojedyncze `IFormFile` lub `IEnumerable<IFormFile>` (lub `List<IFormFile>`) reprezentujący kilka plików. Poniższy przykład jeden lub więcej plików przekazanego w pętli, zapisuje je w lokalnym systemie plików, a następnie zwraca całkowitą liczbę i rozmiar przekazywanych plików.

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

Pliki przesłane za pomocą `IFormFile` technika są buforowane w pamięci lub na dysku na serwerze sieci web przed przetworzeniem. Wewnątrz metody akcji `IFormFile` zawartość jest dostępna jako strumień. Oprócz lokalnego systemu plików, pliki mogą być przesyłane strumieniowo do [magazynu obiektów Blob Azure](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) lub [Entity Framework](https://docs.microsoft.com/ef/core/index).

Aby przechowywać dane pliku binarnego w bazie danych przy użyciu programu Entity Framework, Zdefiniuj właściwość typu `byte[]` jednostki:

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

Określ właściwość viewmodel typu `IFormFile`:

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> `IFormFile`można bezpośrednio jako parametr metody akcji lub właściwością viewmodel, jak pokazano powyżej.

Kopiuj `IFormFile` do strumienia i zapisać go do tablicy typu byte:

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
> Należy zachować ostrożność podczas zapisywania danych binarnych w relacyjnych baz danych, zgodnie z jego może niekorzystnie wpłynąć na wydajność.

## <a name="uploading-large-files-with-streaming"></a>Przekazywanie dużych plików z przesyłania strumieniowego

Jeśli rozmiar i częstotliwość wysyłania plików powoduje problemy zasobów dla aplikacji, należy wziąć pod uwagę przesyłania strumieniowego przekazywania pliku, a nie buforowanie go w całości, tak jak w powyższym podejście powiązania modelu. Podczas używania `IFormFile` i wiązania modelu jest dużo prostsze rozwiązania, przesyłanie strumieniowe wymaga kilku kroków, aby prawidłowo zaimplementować.

> [!NOTE]
> Pojedynczy plik buforowany przekraczającej 64KB zostaną przeniesione z pamięci RAM do tymczasowego pliku na dysku na serwerze. Zasoby (dysk, pamięci RAM) używany przez przekazywania plików są zależne od liczby i rozmiaru przekazywania plików współbieżnych. Przesyłania strumieniowego nie jest tak wiele o wydajności, chodzi o skali. Jeśli spróbujesz buforu przekazywania zbyt wiele lokacji ulegnie awarii, gdy zabraknie mu pamięci lub miejsca na dysku.

W poniższym przykładzie pokazano, przy użyciu języka JavaScript/kątową do strumienia do akcji kontrolera. Token antiforgery pliku jest generowany przy użyciu atrybutu niestandardowego filtru i przekazywane w nagłówkach HTTP zamiast w treści żądania. Ponieważ metoda akcji przetwarza dane przekazane bezpośrednio, tworzenia powiązania modelu została wyłączona przez inny filtr. W ramach tej akcji, zawartość formularza są odczytywane przy użyciu `MultipartReader`, która odczytuje na poszczególnych `MultipartSection`, przetwarzanie pliku lub zapisywanie zawartości zgodnie z potrzebami. Po przeczytaniu wszystkie sekcje akcję wykonuje wiązaniu modelu.

Pierwsze załaduje formularz i antiforgery token jest zapisywany w pliku cookie (za pośrednictwem `GenerateAntiforgeryTokenCookieForAjax` atrybut):

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

Atrybut korzysta z wbudowanej platformy ASP.NET Core [Antiforgery](xref:security/anti-request-forgery) pomocy technicznej, aby ustawić pliku cookie z żądania tokenu:

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

Kątową automatycznie przekazuje antiforgery tokenu w nagłówku żądania o nazwie `X-XSRF-TOKEN`. Aplikacji platformy ASP.NET Core MVC jest skonfigurowany do odwoływania się do tego nagłówka w jej konfiguracji w *Startup.cs*:

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

`DisableFormValueModelBinding` Atrybutu, pokazano poniżej, jest używana do wyłączenia wiązania modelu dla `Upload` metody akcji.

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

Wiązanie modelu jest wyłączona, `Upload` metody akcji nie akceptuje parametrów. Działa on bezpośrednio z `Request` właściwość `ControllerBase`. A `MultipartReader` jest używany do odczytu każdej sekcji. Plik zostanie zapisany pod nazwą identyfikator GUID i dane klucza i wartości są przechowywane w `KeyValueAccumulator`. Po przeczytaniu wszystkie sekcje zawartości `KeyValueAccumulator` są używane dla wiązania danych formularza typu modelu.

Pełną `Upload` metody jest pokazany poniżej:

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Poniżej przedstawiono niektóre typowe problemy występujące podczas pracy z przekazywania plików i ich możliwe rozwiązania.

### <a name="unexpected-not-found-error-with-iis"></a>Nieoczekiwany błąd nie znaleziono z usługami IIS

Następujący błąd wskazuje przekazania pliku przekracza serwera skonfigurowana `maxAllowedContentLength`:

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

Ustawieniem domyślnym jest `30000000`, czyli około 28.6 MB. Wartość można dostosować, edytując *web.config*:

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

To ustawienie dotyczy tylko programu IIS. To zachowanie nie występuje domyślnie odnośnie do hostowania na Kestrel. Aby uzyskać więcej informacji, zobacz [limity żądań \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).

### <a name="null-reference-exception-with-iformfile"></a>Wyjątek odwołania zerowego z IFormFile

Jeśli kontroler jest akceptują przekazać pliki przy użyciu `IFormFile` , ale możesz znaleźć wartość zawsze ma wartość null, upewnij się, że formularza HTML jest określenie `enctype` wartość `multipart/form-data`. Jeśli ten atrybut nie jest ustawiony na `<form>` elementu przekazywania plików nie zostanie przeprowadzona i wszelkie powiązane z `IFormFile` argumentów będzie mieć wartość null.
