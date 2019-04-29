---
title: Przekazywanie plików na stronę Razor programu ASP.NET Core
author: guardrex
description: Dowiedz się, jak przekazać pliki na stronę Razor programu ASP.NET Core przy użyciu klasy FileUpload.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 11/10/2018
ms.custom: mvc, seodec18
uid: razor-pages/upload-files
ms.openlocfilehash: a12653e5dc99ad731de63e151af6dbdbe38ffefc
ms.sourcegitcommit: 8a84ce880b4c40d6694ba6423038f18fc2eb5746
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60165225"
---
# <a name="upload-files-to-a-razor-page-in-aspnet-core"></a>Przekazywanie plików na stronę Razor programu ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Ten temat opiera się na [przykładową aplikację](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) w <xref:tutorials/razor-pages/razor-pages-start>.

W tym temacie pokazano, jak przekazać pliki, za pomocą prostego modelu powiązania, co sprawdza się dobrze w przypadku przekazywania plików na małe. Aby uzyskać informacje na przesyłanie strumieniowe dużych plików, zobacz [przekazywanie dużych plików z przesyłaniem strumieniowym](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).

W poniższych krokach funkcji przekazywania plików harmonogram film zostanie dodany do przykładowej aplikacji. Harmonogram film jest reprezentowany przez `Schedule` klasy. Klasa zawiera dwie wersje harmonogramu. Jedna wersja są przekazywane klientom, `PublicSchedule`. Druga wersja jest używana w przypadku pracowników firmy `PrivateSchedule`. Każda wersja jest przekazywany jako oddzielny plik. Samouczek pokazuje, jak wykonać dwie operacje przekazywania plików ze strony za pomocą pojedynczego wpisu do serwera.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/upload-files/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="security-considerations"></a>Zagadnienia dotyczące bezpieczeństwa

Należy zachować ostrożność przy zapewnieniu użytkownikom możliwość przekazywania plików na serwerze. Może być wykonywane przez osoby atakujące ["odmowa usługi"](/windows-hardware/drivers/ifs/denial-of-service) i inne ataki na system. Niektóre czynności zabezpieczeń, które zmniejszają prawdopodobieństwo udanego ataku są następujące:

* Przekazywanie plików do obszaru przekazywania pliku dedykowanych w systemie, dzięki czemu łatwiej można nałożyć środków bezpieczeństwa w systemach przekazana zawartość. Podczas pozwalające przekazywania plików, upewnij się, że uprawnienia do wykonywania są wyłączone w lokalizacji przekazywania.
* Użyj nazwy plików bezpieczne określane przez aplikację, a nie z danych wejściowych użytkownika lub nazwę pliku przekazanego pliku.
* Zezwalaj na tylko określony zestaw rozszerzeń plików zatwierdzone.
* Sprawdź, czy po stronie klienta są sprawdzane na serwerze. Sprawdzanie klienta są łatwe do obejścia.
* Sprawdź rozmiar przekazywania i uniemożliwić przekazywanie większych niż oczekiwano.
* Uruchomić skanera przed wirusami i złośliwym oprogramowaniem na przekazana zawartość.

> [!WARNING]
> Przekazywanie złośliwego kodu do systemu często jest pierwszym krokiem do wykonywania kodu, która może być:
> * Całkowicie przejęcia w systemie.
> * Przeciążenia systemu, w wyniku czego system całkowicie nie powiedzie się.
> * Naruszyć bezpieczeństwo danych użytkownika lub systemu.
> * Zastosowanie graffiti do interfejsu publicznego.

## <a name="add-a-fileupload-class"></a>Dodaj klasę FileUpload

Utwórz stronę Razor do obsługi parę przekazywania plików. Dodaj `FileUpload` klasy, który jest powiązany do strony, aby uzyskać dane tabeli schedule. Kliknij prawym przyciskiem myszy *modeli* folderu. Wybierz **Dodaj** > **klasy**. Nazwa klasy **FileUpload** i dodaj następujące właściwości:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/FileUpload.cs)]

::: moniker-end

Klasa ma właściwość Tytuł harmonogram i właściwości dla każdego z dwóch wersji harmonogramu. Wymagane są wszystkie trzy właściwości, a tytuł musi zawierać 3-60 znaków.

## <a name="add-a-helper-method-to-upload-files"></a>Dodaj metodę pomocnika do przekazywania plików

Aby uniknąć zduplikowania kodu do przetwarzania plików przekazanych harmonogram, najpierw Dodaj metodę pomocnika statyczne. Tworzenie *narzędzia* folderu w aplikacji i Dodaj *FileHelpers.cs* pliku o następującej zawartości. Metoda pomocnika `ProcessFormFile`, przyjmuje [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) i [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) i zwraca ciąg zawierający rozmiar i zawartości pliku. Typ zawartości i długość są sprawdzane. Jeśli plik nie przeszły sprawdzanie poprawności, błąd jest dodawany do `ModelState`.

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Utilities/FileHelpers.cs)]

::: moniker-end

### <a name="save-the-file-to-disk"></a>Zapisz plik na dysku

Przykładowa aplikacja zapisuje przekazywanych plików w polach bazy danych. Aby zapisać plik na dysku, należy użyć [FileStream](/dotnet/api/system.io.filestream). Poniższy przykład służy do kopiowania pliku w posiadaniu `FileUpload.UploadPublicSchedule` do `FileStream` w `OnPostAsync` metody. `FileStream` Zapisuje plik na dysku w `<PATH-AND-FILE-NAME>` podane:

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

Proces roboczy musi mieć uprawnienia do zapisu w lokalizacji określonej przez `filePath`.

> [!NOTE]
> `filePath` *Musi* można umieszczać nazwy pliku. Jeśli nie podasz nazwy pliku, [unauthorizedaccessexception —](/dotnet/api/system.unauthorizedaccessexception) jest generowany w czasie wykonywania.

> [!WARNING]
> Nigdy nie są zachowywane przekazywanych plików w tym samym drzewie katalogu, co aplikacja.
>
> Przykładowy kod zawiera nie ochrony po stronie serwera przed przekazywania złośliwych plików. Instrukcje dotyczące zmniejszenie obszaru powierzchni ataku, akceptując pliki użytkowników zobacz następujące zasoby:
>
> * [Przekazywanie pliku bez ograniczeń](https://www.owasp.org/index.php/Unrestricted_File_Upload)
> * [Zabezpieczenia platformy Azure: Upewnij się, że odpowiednie formanty są stosowane podczas akceptowania plików od użytkowników](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

### <a name="save-the-file-to-azure-blob-storage"></a>Zapisz plik w usłudze Azure Blob Storage

Aby przekazać zawartość pliku do usługi Azure Blob Storage, zobacz [Rozpoczynanie pracy z usługą Azure Blob Storage przy użyciu platformy .NET](/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Temat demonstruje sposób skorzystania [UploadFromStream](/dotnet/api/microsoft.windowsazure.storage.file.cloudfile.uploadfromstreamasync) można zapisać [FileStream](/dotnet/api/system.io.filestream) do magazynu obiektów blob.

## <a name="add-the-schedule-class"></a>Dodaj klasę harmonogramu

Kliknij prawym przyciskiem myszy *modeli* folderu. Wybierz **Dodaj** > **klasy**. Nazwa klasy **harmonogram** i dodaj następujące właściwości:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/Schedule.cs)]

::: moniker-end

Ta klasa używa `Display` i `DisplayFormat` atrybuty, które generuje przyjazna tytułów i formatowania podczas renderowania dane tabeli schedule.

::: moniker range=">= aspnetcore-2.1"

## <a name="update-the-razorpagesmoviecontext"></a>Aktualizacja RazorPagesMovieContext

Określ `DbSet` w `RazorPagesMovieContext` (*Data/RazorPagesMovieContext.cs*) dla harmonogramów:

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Data/RazorPagesMovieContext.cs?highlight=17)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="update-the-moviecontext"></a>Aktualizacja MovieContext

Określ `DbSet` w `MovieContext` (*Models/MovieContext.cs*) dla harmonogramów:

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

::: moniker-end

## <a name="add-the-schedule-table-to-the-database"></a>Dodaj tabelę harmonogram do bazy danych

Otwórz konsolę Menedżera pakietów (PMC): **Narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.

![Menu konsoli zarządzania Pakietami](upload-files/_static/pmc.png)

W konsoli zarządzania Pakietami wykonaj następujące polecenia. Te polecenia Dodaj `Schedule` tabeli w bazie danych:

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a>Dodaj stronę Razor przekazywania pliku

W *stron* folderze utwórz *harmonogramy* folderu. W *harmonogramy* folderu, Utwórz stronę o nazwie *Index.cshtml* przekazywania harmonogram o następującej zawartości:

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

::: moniker-end

Każda grupa formularz zawiera  **\<Etykieta >** wyświetlającą nazwa każdej właściwości klasy. `Display` Atrybutów w `FileUpload` modelu podanie wartości wyświetlania etykiet. Na przykład `UploadPublicSchedule` nazwy wyświetlania właściwości została ustawiona za pomocą `[Display(Name="Public Schedule")]` i dlatego wyświetla "Harmonogram publiczny" w etykiecie, gdy powoduje wyświetlenie formularza.

Każda grupa formularz zawiera weryfikacji  **\<span >**. Jeśli użytkownik wejściowych nie spełniają atrybuty właściwości ustawione w `FileUpload` klasy lub jeśli któryś z `ProcessFormFile` sprawdzanie poprawności pliku metoda kończyć się niepowodzeniem, modelu nie powiedzie się sprawdzić poprawność. Podczas sprawdzania poprawności modelu nie powiedzie się, komunikat dotyczący sprawdzania poprawności pomocne jest renderowany do użytkownika. Na przykład `Title` właściwość jest oznaczona za pomocą `[Required]` i `[StringLength(60, MinimumLength = 3)]`. Jeśli użytkownik nie może wpisać tytuł, otrzyma komunikat informujący, że wartość jest wymagana. Jeśli użytkownik wprowadzi wartości, mniej niż trzy znaki lub więcej niż 60 znaków, otrzyma komunikat informujący, że wartość ma niepoprawną długość. Jeśli plik jest pod warunkiem, że nie ma zawartości, zostanie wyświetlony komunikat, że plik jest pusty.

## <a name="add-the-page-model"></a>Dodawanie modelu strony

Dodawanie modelu strony (*Index.cshtml.cs*) do *harmonogramy* folderu:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

::: moniker-end

Model strony (`IndexModel` w *Index.cshtml.cs*) wiąże `FileUpload` klasy:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

::: moniker-end

Model wykorzystuje także listę harmonogramy (`IList<Schedule>`) aby wyświetlić harmonogramy przechowywanych w bazie danych na stronie:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

::: moniker-end

Jeśli strona ładuje się za pomocą `OnGetAsync`, `Schedules` jest wypełniana z bazy danych i używany do generowania tabeli HTML załadować harmonogramów:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

::: moniker-end

Po opublikowaniu formularza z serwerem `ModelState` jest zaznaczone. Jeśli jest to nieprawidłowa `Schedule` zostanie ponownie skompilowany, i strony, renderowanie przy użyciu jednego lub więcej komunikatów dotyczących sprawdzania poprawności informacją, dlaczego strony Weryfikacja nie powiodła się. Jeśli są one prawidłowe, `FileUpload` właściwości są używane w *OnPostAsync* aby zakończyć przekazywanie pliku dla obu wersji harmonogram i Utwórz nową `Schedule` obiektu do przechowywania danych. Harmonogram jest następnie zapisywany w bazie danych:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

::: moniker-end

## <a name="link-the-file-upload-razor-page"></a>Link przekazywania pliku strony Razor

Otwórz *Pages/Shared/_Layout.cshtml* i dodać link na pasku nawigacyjnym, aby dotrzeć do strony harmonogramów:

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

## <a name="add-a-page-to-confirm-schedule-deletion"></a>Dodaj stronę, aby potwierdzić usunięcie harmonogramu

Gdy użytkownik kliknie przycisk, aby usunąć harmonogram, znajduje się szansę, aby anulować operację. Dodaj stronę potwierdzenia usunięcia (*Delete.cshtml*) do *harmonogramy* folderu:

::: moniker range=">= aspnetcore-2.1"

[!code-cshtml[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-cshtml[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

::: moniker-end

Model strony (*Delete.cshtml.cs*) ładuje identyfikowane za pomocą jednego harmonogramu `id` w danych trasy żądania. Dodaj *Delete.cshtml.cs* plik *harmonogramy* folderu:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

::: moniker-end

`OnPostAsync` Obsługiwała usuwanie harmonogram, według jego `id`:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](upload-files/snapshot_samples/2.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](upload-files/snapshot_samples/1.x/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

::: moniker-end

Po pomyślnym usunięciu harmonogramu, `RedirectToPage` wysyła użytkownika harmonogramy *Index.cshtml* strony.

## <a name="the-working-schedules-razor-page"></a>Strona Razor harmonogramy dni roboczych

Po załadowaniu strony, etykiety i dane wejściowe dla tytułu harmonogramu, harmonogram publicznych i prywatnych harmonogramu są renderowane przy użyciu przycisku Prześlij:

![Planuje strony Razor, jak pokazano na ładowania początkowego bez błędów sprawdzania poprawności i puste pola](upload-files/_static/browser1.png)

Wybieranie **przekazywanie** przycisk bez żadnego pola wypełnianie narusza `[Required]` atrybutów w modelu. `ModelState` Jest nieprawidłowy. Komunikaty o błędach weryfikacji są wyświetlane dla użytkownika:

![Komunikaty o błędach weryfikacji pojawiają się obok każdej kontrolki wprowadzania](upload-files/_static/browser2.png)

Wpisz dwie litery w **tytuł** pola. Komunikat sprawdzania poprawności zmienia się do wskazania, że tytuł musi mieć od 3 – 60 znaków:

![Zmienić komunikat weryfikacji tytuł](upload-files/_static/browser3.png)

Przekazywane co najmniej jeden harmonogram **załadowane harmonogramy** sekcji renderuje załadować harmonogramów:

![Tabela załadować harmonogramy, przedstawiający tytuł każdy z harmonogramów przekazany daty UTC, rozmiar pliku w publicznej wersji i rozmiar pliku w prywatnej wersji](upload-files/_static/browser4.png)

Użytkownik może kliknąć **Usuń** link z tego miejsca nawiązać widoku potwierdzenie usunięcia, które mają możliwość potwierdzenia lub anulować operację usuwania.

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Aby uzyskać informacje o rozwiązywaniu problemów z `IFormFile` przekazywania, zobacz [przekazywania plików z platformy ASP.NET Core: Rozwiązywanie problemów z](xref:mvc/models/file-uploads#troubleshooting).
