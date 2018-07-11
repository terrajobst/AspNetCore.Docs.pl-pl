---
title: Przekazywanie plików z platformy ASP.NET Core
author: ardalis
description: Jak używać wiązania modelu i przesyłania strumieniowego do przekazywania plików na platformie ASP.NET Core MVC.
ms.author: riande
ms.date: 07/05/2017
uid: mvc/models/file-uploads
ms.openlocfilehash: 771e22ca01c67f2b6bbee780324d9d08759b3279
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38201735"
---
# <a name="file-uploads-in-aspnet-core"></a>Przekazywanie plików z platformy ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Akcje programu ASP.NET MVC obsługuje przekazywanie jeden lub więcej plików przy użyciu prostego modelu binding na mniejsze pliki lub przesyłania strumieniowego większe pliki.

[Wyświetlanie lub pobieranie przykładowy z serwisu GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a>Przekazywanie małych plików z wiązania modelu

Aby przekazać małych plików, możesz użyć formularza HTML wieloczęściowej lub utworzyć żądanie POST, przy użyciu języka JavaScript. Przykładowego formularza przy użyciu Razor, który obsługuje wiele plików przekazanych, znajdują się poniżej:

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

Aby zapewnić obsługę przekazywania plików, należy określić formularzy HTML `enctype` z `multipart/form-data`. `files` Elementu wejściowego powyżej obsługuje przekazywania wielu plików. Pomiń `multiple` atrybut ten element input, aby zezwolić na pojedynczy plik do przekazania. Powyższe znaczników powoduje wyświetlenie w przeglądarce jako:

![Formularz przekazywania pliku](file-uploads/_static/upload-form.png)

Poszczególnych plików, które są przekazywane do serwera jest możliwy za pośrednictwem [powiązań modelu](xref:mvc/models/model-binding) przy użyciu [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) interfejsu. `IFormFile` ma następującą strukturę:

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
> Nie należy polegać na ani zaufania `FileName` właściwości bez sprawdzania poprawności. `FileName` Właściwość powinna być używana tylko w celach wyświetlania.

Podczas przekazywania plików przy użyciu wiązania modelu i `IFormFile` interfejsu, metoda akcji może akceptować może to być pojedynczy `IFormFile` lub `IEnumerable<IFormFile>` (lub `List<IFormFile>`) reprezentujący kilka plików. Poniższy przykład jeden lub więcej plików przekazanych w pętli, zapisuje je w lokalnym systemie plików i zwraca całkowitą liczbę i rozmiar przekazywanych plików.

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

Przekazane przy użyciu `IFormFile` technika są buforowane w pamięci lub na dysku na serwerze sieci web przed przetworzeniem. Wewnątrz metody akcji `IFormFile` zawartość jest dostępna jako strumień. Oprócz lokalnego systemu plików, pliki mogą być przesłana strumieniowo do [usługi Azure Blob storage](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) lub [Entity Framework](https://docs.microsoft.com/ef/core/index).

Aby przechowywać dane plik binarny w bazie danych przy użyciu platformy Entity Framework, należy zdefiniować właściwości typu `byte[]` jednostki:

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
> `IFormFile` można bezpośrednio jako parametru metody akcji lub właściwością viewmodel jak pokazano powyżej.

Kopiuj `IFormFile` do strumienia, a następnie zapisz go do tablicy typu byte:

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
> Należy zachować ostrożność podczas zapisywania danych binarnych w relacyjnych baz danych, zgodnie z jego jest niekorzystnie wpłynąć na wydajność.

## <a name="uploading-large-files-with-streaming"></a>Przekazywanie dużych plików za pomocą przesyłania strumieniowego

Jeśli rozmiar lub częstotliwość wysyłania plików powoduje problemy z zasobami dla aplikacji, należy wziąć pod uwagę przesyłania strumieniowego przekazywania pliku, a nie buforowania go w całości, tak jak powyżej podejście powiązania modelu. Podczas korzystania z `IFormFile` i wiązania modelu jest dużo prostsze rozwiązaniem, przesyłanie strumieniowe wymaga kilku kroków dotyczących implementacji prawidłowo.

> [!NOTE]
> Pojedynczy plik buforowany przekraczającym 64KB zostanie przeniesiony z pamięci RAM w pliku tymczasowym na dysku, na serwerze. Zasoby (dysk, pamięci RAM) używany przez operacje przekazywania plików zależą od liczby i rozmiaru współbieżne przekazania plików. Przesyłanie strumieniowe nie jest tak wiele dotyczące wydajności, chodzi o skali. Jeśli spróbujesz buforu przekazywania zbyt wiele lokacji ulegnie awarii, gdy zabraknie mu pamięci lub miejsca na dysku.

Poniższy przykład pokazuje, przy użyciu języka JavaScript/Angular do przesyłania strumieniowego do akcji kontrolera. Token antiforgery pliku jest generowana za pomocą niestandardowego atrybutu filtru i przekazywane w nagłówkach HTTP, a nie w treści żądania. Ponieważ metody akcji, które bezpośrednio przetwarza dane przekazane, wiązanie modelu została wyłączona przez inny filtr. W akcji, zawartość formularza są odczytywane przy użyciu `MultipartReader`, która odczytuje poszczególnym `MultipartSection`, przetwarzanie pliku lub zawartość jest przechowywana zgodnie z potrzebami. Po przeczytaniu wszystkie sekcje akcji wykonuje swoje własne powiązania modelu.

Akcja początkowa ładuje formularz i tokenu antiforgery są zapisywane w pliku cookie (za pośrednictwem `GenerateAntiforgeryTokenCookieForAjax` atrybutu):

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

Ten atrybut używa wbudowanego platformy ASP.NET Core [Antiforgery](xref:security/anti-request-forgery) pomocy technicznej można ustawić pliku cookie z token żądania:

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

Platformy Angular automatycznie przekazuje antiforgery token w nagłówku żądania, o nazwie `X-XSRF-TOKEN`. Aplikacja ASP.NET Core MVC jest skonfigurowana do odwoływania się do tego pliku nagłówkowego w jej konfiguracji w *Startup.cs*:

[!code-csharp[](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

`DisableFormValueModelBinding` Atrybutu, pokazano poniżej, jest używana do wyłączenia wiązania modelu dla `Upload` metody akcji.

[!code-csharp[](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

Ponieważ wiązania modelu jest wyłączona, `Upload` metody akcji nie akceptuje parametrów. Współpracuje bezpośrednio z `Request` właściwość `ControllerBase`. A `MultipartReader` służy do odczytywania każdej sekcji. Plik zostanie zapisany pod nazwą identyfikator GUID i dane klucz wartość są przechowywane w `KeyValueAccumulator`. Po przeczytaniu wszystkie sekcje zawartości `KeyValueAccumulator` są używane do powiązania danych formularza typu modelu.

Pełne `Upload` metoda znajdują się poniżej:

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Poniżej przedstawiono niektóre typowe problemy występujące podczas pracy z przekazywania plików i ich możliwe rozwiązania.

### <a name="unexpected-not-found-error-with-iis"></a>Nieoczekiwany błąd nie znaleziono za pomocą programu IIS

Następujący błąd wskazuje, przekazywanie pliku przekracza serwera skonfigurowany przez `maxAllowedContentLength`:

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

To ustawienie dotyczy tylko usług IIS. Zachowanie nie występuje domyślnie w przypadku hostowania w Kestrel. Aby uzyskać więcej informacji, zobacz [limity żądań \<requestLimits\>](/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).

### <a name="null-reference-exception-with-iformfile"></a>Wyjątek odwołania o wartości null z IFormFile

Jeśli kontroler jest przyjmuje przekazane pliki przy użyciu `IFormFile` , ale możesz znaleźć, wartość jest zawsze wartość null, upewnij się, że formularza HTML jest określenie `enctype` wartość `multipart/form-data`. Jeśli ten atrybut nie jest ustawiony na `<form>` element nie zostanie wykonana, przekazanie pliku i wszystkie powiązane z `IFormFile` argumenty będą miały wartość null.
