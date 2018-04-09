---
title: Globalizacja i lokalizacja w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak platformy ASP.NET Core udostępnia usługi i oprogramowanie pośredniczące dla lokalizacji zawartości do innych języków i kultur.
manager: wpickett
ms.author: riande
ms.date: 01/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/localization
ms.openlocfilehash: 3ae73cb40b4db492883f302aeb559b9606aa8ee7
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="globalization-and-localization-in-aspnet-core"></a>Globalizacja i lokalizacja w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [linkach Damien](https://twitter.com/damien_bod), [Calixto Roman](https://twitter.com/bartmax), [Nadeem Afana](https://twitter.com/NadeemAfana), i [Ateya Hisham Bin](https://twitter.com/hishambinateya)

Tworzenie wielu języków witryny sieci Web za pomocą platformy ASP.NET Core umożliwi witrynę tak, aby uzyskać dostęp do większej liczby osób. Platformy ASP.NET Core udostępnia usługi i oprogramowanie pośredniczące dla lokalizowanie do różnych języków i kultur.

Przystosowywanie do warunków międzynarodowych obejmuje [globalizacji](https://docs.microsoft.com/dotnet/api/system.globalization) i [lokalizacja](https://docs.microsoft.com/dotnet/standard/globalization-localization/localization). Globalizacja to proces projektowania aplikacji, które obsługują innych kultur. Globalizacja dodaje obsługę danych wejściowych, wyświetlania i dane wyjściowe ze zdefiniowanym zestawem język skryptów, które odnoszą się do określonych obszarach geograficznych.

Lokalizacja jest procesem dostosowywania uniwersalnych aplikacji, która już przeprowadzono dla możliwości zlokalizowania do określonej kultury/ustawień regionalnych. Aby uzyskać więcej informacji, zobacz **warunki lokalizacja i globalizacja** pod koniec tego dokumentu.

Lokalizacja aplikacji obejmuje następujące czynności:

1. Wprowadź zlokalizowania zawartości aplikacji

2. Podaj zlokalizowanych zasobów dla języków i kultur, która jest obsługiwana

3. Wdrożenie strategii wybierz języka/kultury dla każdego żądania

## <a name="make-the-apps-content-localizable"></a>Wprowadź zlokalizowania zawartości aplikacji

Wprowadzone w ASP.NET Core `IStringLocalizer` i `IStringLocalizer<T>` zostały zaprojektowane pod poprawia wydajność podczas opracowywania zlokalizowane aplikacji. `IStringLocalizer` używa [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager) i [ResourceReader](https://docs.microsoft.com/dotnet/api/system.resources.resourcereader) zapewnienie zasobów określonej kultury w czasie wykonywania. Indeksator ma prosty interfejs i `IEnumerable` dla zwracania zlokalizowanych ciągów. `IStringLocalizer` nie wymaga przechowywania ciągów języka domyślnego pliku zasobu. Mogą tworzyć aplikacji przeznaczony dla lokalizacji i nie trzeba tworzyć pliki zasobów na początku programowanie. Poniższy kod przedstawia sposób zawijania ciąg "Title o" do lokalizacji.

[!code-csharp[](localization/sample/Localization/Controllers/AboutController.cs)]

W powyższym kodzie `IStringLocalizer<T>` implementacji pochodzi z [iniekcji zależności](dependency-injection.md). Jeśli nie zostanie odnaleziony zlokalizowaną wartość tytułu"temat", a następnie z kluczem indeksatora, jest zwracany, czyli ciąg "Title o". Możesz pozostaw wartość domyślną literału ciągi języka w aplikacji i zawijanie je w lokalizatora, dzięki czemu można skupić się na temat tworzenia aplikacji. Opracuj swoją aplikację z językiem domyślnym i przygotowania kroku lokalizacji bez tworzenia domyślnego pliku zasobów. Alternatywnie można użyć tradycyjne podejście i podaj klucz, aby pobrać domyślny ciąg języka. Dla wielu deweloperów nowy przepływ pracy nie ma domyślny język *.resx* plików i po prostu zawijania Literały ciągu można zmniejszyć koszty lokalizowanie aplikacji. Inni deweloperzy preferowane przepływu pracy tradycyjnych zgodnie z jego może ułatwić pracę z dłużej literały ciągów i ułatwić aktualizowanie zlokalizowanych ciągów.

Użyj `IHtmlLocalizer<T>` implementacji dla zasobów, które zawierają HTML. `IHtmlLocalizer` Argumenty, które są sformatowane w ciągu zasobu koduje HTML, ale nie sam ciąg zasobu kodowanie HTML. W przykładzie wyróżnione poniżej tylko wartości `name` parametr ma kodowania HTML.

[!code-csharp[](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

**Uwaga:** ma zazwyczaj do zlokalizowania tylko tekst i HTML nie.

Na najniższym poziomie, można uzyskać `IStringLocalizerFactory` z [iniekcji zależności](dependency-injection.md):

[!code-csharp[](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

Powyższy kod pokazuje każdej fabryki dwóch metod tworzenia.

Możesz partycji z zlokalizowanych ciągów przez kontroler, obszar, lub mieć tylko jeden kontener. W przykładowej aplikacji, o nazwie klasy fikcyjny `SharedResource` służy do udostępnionych zasobów.

[!code-csharp[](localization/sample/Localization/Resources/SharedResource.cs)]

Niektórzy deweloperzy wykorzystują `Startup` klasa może zawierać ciągów globalnej lub udostępniony. W przykładzie poniżej `InfoController` i `SharedResource` służą wiadomość dla lokalizatorów:

[!code-csharp[](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a>Lokalizacja widoku

`IViewLocalizer` Usługa zapewnia zlokalizowanych ciągów dla [widoku](https://docs.microsoft.com/aspnet/core). `ViewLocalizer` Klasa implementuje ten interfejs i wyszukuje lokalizacji zasobów ze ścieżki plików widoku. Poniższy kod przedstawia sposób użycia Domyślna implementacja `IViewLocalizer`:

[!code-cshtml[](localization/sample/Localization/Views/Home/About.cshtml)]

Domyślna implementacja `IViewLocalizer` umożliwia znalezienie pliku zasobów na podstawie nazwy pliku dla widoku. Nie jest dostępna opcja używania pliku globalnego zasobu udostępnionego. `ViewLocalizer` implementuje za pomocą lokalizatora `IHtmlLocalizer`, więc Razor nie HTML zlokalizowany ciąg do zakodowania. Można parametryzacja ciągów zasobów i `IViewLocalizer` będzie kodowanie HTML parametrów, ale nie do ciągu zasobu. Należy wziąć pod uwagę następujące znaczników Razor:

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

Plik zasobów francuskim może zawierać następujące informacje:

| Key | Wartość |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b> ` |

Widoku renderowanym zawierałoby kod znaczników HTML z pliku zasobów.

**Uwaga:** ma zazwyczaj do zlokalizowania tylko tekst i HTML nie.

Aby użyć pliku zasobu udostępnionego w widoku, wstrzyknąć `IHtmlLocalizer<T>`:

[!code-cshtml[](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a>Lokalizacja DataAnnotations

Komunikaty o błędach DataAnnotations są zlokalizowane z `IStringLocalizer<T>`. Przy użyciu opcji `ResourcesPath = "Resources"`, komunikaty o błędach w `RegisterViewModel` mogą być przechowywane w jednym z następujących ścieżek:

* *Resources/ViewModels.Account.RegisterViewModel.fr.resx*
* *Resources/ViewModels/Account/RegisterViewModel.fr.resx*

[!code-csharp[](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

ASP.NET Core MVC 1.1.0 i wyższe, bez sprawdzania poprawności atrybutów są zlokalizowane. Platformy ASP.NET Core MVC 1.0 jest **nie** wyszukiwania zlokalizowanych ciągów dla atrybutów bez sprawdzania poprawności.

<a name="one-resource-string-multiple-classes"></a>
### <a name="using-one-resource-string-for-multiple-classes"></a>Przy użyciu jednego ciągu zasobu dla wielu klas

Poniższy kod przedstawia sposób użycia jednego ciągu zasobu dla atrybutów sprawdzania poprawności z wielu klas:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddDataAnnotationsLocalization(options => {
            options.DataAnnotationLocalizerProvider = (type, factory) =>
                factory.Create(typeof(SharedResource));
        });
}
```

W kodzie poprzedzających `SharedResource` jest klasa odpowiadający resx przechowywania wiadomości sprawdzania poprawności. Takie podejście, DataAnnotations tylko za pomocą `SharedResource`, zamiast zasobów dla każdej klasy.

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a>Podaj zlokalizowanych zasobów dla języków i kultur, która jest obsługiwana

### <a name="supportedcultures-and-supporteduicultures"></a>SupportedCultures i SupportedUICultures

Platformy ASP.NET Core służy do określania dwóch wartości kultury, `SupportedCultures` i `SupportedUICultures`. [CultureInfo](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo) obiekt do `SupportedCultures` określa wyniki funkcje zależne od kultury, takie jak data, czas, numer i formatowanie waluty. `SupportedCultures` Określa również kolejność sortowania tekstu, konwencje wielkość liter i porównywania ciągów. Zobacz [wartość CultureInfo.CurrentCulture](https://docs.microsoft.com/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) uzyskać więcej informacji dotyczących sposobu serwera pobiera kultury. `SupportedUICultures` Określa, który tłumaczy ciągi (z *.resx* pliki) są wyszukiwane przez [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager). `ResourceManager` Po prostu wyszukuje specyficzne dla kultury ciągów, które jest określane przez `CurrentUICulture`. Każdy wątek .NET ma `CurrentCulture` i `CurrentUICulture` obiektów. Platformy ASP.NET Core sprawdzi te wartości podczas renderowania funkcje zależne od kultury. Na przykład, jeśli kultury bieżącej wątku ma ustawioną wartość "en US" (angielski, Stany Zjednoczone) `DateTime.Now.ToLongDateString()` Wyświetla "Czwartek, 18 luty 2016 r.", ale jeśli `CurrentCulture` jest ustawiona na "es-ES" (wersja hiszpańska, Hiszpania) dane wyjściowe będą "jueves, de febrero 18 de 2016".

## <a name="resource-files"></a>Pliki zasobów

Plik zasobów jest mechanizm przydatne do oddzielania Lokalizowalny ciągów z kodu. Przetłumaczone ciągi dla języków innych niż domyślne są izolowane *.resx* plików zasobów. Na przykład można utworzyć plik hiszpańskim zasobów o nazwie *Welcome.es.resx* zawierający przetłumaczone ciągi. "es" jest kod języka hiszpańskiego. Aby utworzyć ten plik zasobu w programie Visual Studio:

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy folder, w którym będzie zawierać plik zasobu > **Dodaj** > **nowy element**.

    ![Menu kontekstowe zagnieżdżone: W Eksploratorze rozwiązań menu kontekstowe jest otwarte dla zasobów. Drugi menu kontekstowe jest otwarty dla Dodaj przedstawiający wyróżnione polecenie Nowy element.](localization/_static/newi.png)

2. W **wyszukiwania zainstalowane szablony** polu wprowadź "zasobu" i nazwę pliku.

    ![Dodaj nowy element okna dialogowego](localization/_static/res.png)

3. Wprowadź wartość klucza (ciąg natywnego) w **nazwa** kolumny i przetłumaczonego ciąg w **wartość** kolumny.

    ![Welcome.ES.resx pliku (plik zasobów-Zapraszamy hiszpański) z programu word Hello kolumny z nazwami i word Hola (Hello w języku hiszpańskim) w kolumnie wartość](localization/_static/hola.png)

    Visual Studio zawiera *Welcome.es.resx* pliku.

    ![Eksploratora rozwiązań przedstawiający plik zasobów-Zapraszamy hiszpański (es)](localization/_static/se.png)

<a name="error"></a>

Jeśli używasz wersji Preview 2017 r w usłudze Visual Studio 15 ustęp 3, zostanie wyświetlony wskaźnik błędów w edytorze zasobów. Usuń *ResXFileCodeGenerator* wartość z *niestandardowego narzędzia* siatki właściwości, aby uniknąć tego komunikatu o błędzie:

![Edytor resx](localization/_static/err.png)

Alternatywnie możesz zignorować ten błąd. Mamy nadzieję rozwiązać ten problem w następnej wersji.

## <a name="resource-file-naming"></a>Nazywanie pliku zasobów

Zasoby są nazywane dla Pełna nazwa typu ich klasy minus nazwy zestawu. Na przykład francuskim zasobów w projekcie, których główny zestaw jest `LocalizationWebsite.Web.dll` dla klasy `LocalizationWebsite.Web.Startup` będą miały postać *Startup.fr.resx*. Zasób klasy `LocalizationWebsite.Web.Controllers.HomeController` będą miały postać *Controllers.HomeController.fr.resx*. Jeśli klasę docelową przestrzeń nazw nie jest taka sama jak nazwa zestawu należy Pełna nazwa typu. Na przykład w próbce projektu zasobów dla typu `ExtraNamespace.Tools` będą miały postać *ExtraNamespace.Tools.fr.resx*.

W projekcie próbki `ConfigureServices` zestawy metody `ResourcesPath` "Zasoby", więc projekt względna ścieżka pliku zasobu francuskim macierzystego kontrolera jest *Resources/Controllers.HomeController.fr.resx*. Alternatywnie można używać folderów można organizować pliki zasobów. Macierzysty kontrolera byłaby to ścieżka *Resources/Controllers/HomeController.fr.resx*. Jeśli nie używasz `ResourcesPath` opcji *.resx* przejdzie pliku w katalogu podstawowego projektu. Plik zasobów dla `HomeController` będą miały postać *Controllers.HomeController.fr.resx*. Wybór przy użyciu konwencji nazewnictwa kropką lub ścieżka zależy od tego, sposób organizowania zasobów plików.

| Nazwa zasobu | Kropek ani nazwy ścieżki |
| ------------   | ------------- |
| Resources/Controllers.HomeController.fr.resx | Kropka  |
| Resources/Controllers/HomeController.fr.resx  | Ścieżka |
|    |     |

Pliki zasobów przy użyciu `@inject IViewLocalizer` w widokach Razor wykonaj podobnego wzorca. Może mieć nazwę pliku zasobów dla widoku przy użyciu nazw kropką lub nazwy ścieżki. Pliki zasobów widoku razor naśladować ścieżkę pliku ich skojarzonego widoku. Zakładając, że możemy ustawić `ResourcesPath` do "Zasoby", plik zasobu francuskim skojarzone z *Views/Home/About.cshtml* widok może być jedną z następujących czynności:

* Resources/Views/Home/About.fr.resx

* Resources/Views.Home.About.fr.resx

Jeśli nie używasz `ResourcesPath` opcji *.resx* pliku dla widoku będzie znajdować się w tym samym folderze co widoku.

## <a name="culture-fallback-behavior"></a>Działanie rezerwowe kultury

Podczas wyszukiwania zasobów, lokalizacja uczestniczy w "kultury rezerwowej". Począwszy od żądaną kulturę, jeśli nie można odnaleźć, przywraca Kultura nadrzędna tej kultury. Jako Zarezerwuj [CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) właściwość reprezentuje Kultura nadrzędna. Oznacza to, zwykle (ale nie zawsze) usuwanie national signifier z obrazem ISO. Na przykład dialekt hiszpański używany w Meksyk jest "es-MX". Ma element nadrzędny "es"&mdash;hiszpańskim nieokreślonym w dowolnym kraju.

Załóżmy, że witryny odbiera żądanie dla zasobu "-Zapraszamy!" przy użyciu kultury "fr-CA". Lokalizacja systemu sprawdza następujące zasoby w kolejności i wybiera pierwszego dopasowania:

* *Welcome.fr-CA.resx*
* *Welcome.fr.resx*
* *Welcome.resx* (Jeśli `NeutralResourcesLanguage` jest "fr-CA")

Na przykład jeśli usuniesz określenia kultury ".fr" i ma kultury ustawione na język francuski, domyślny plik zasobu jest do odczytu i są zlokalizowane ciągi. Menedżer zasobów wyznacza domyślny lub rezerwowej zasobu dla Jeśli nic nie spełnia Twoje żądaną kulturę. Jeśli chcesz przywrócić klucz tylko, gdy nie ma zasobu dla żądanego kultury nie może mieć domyślnego pliku zasobów.

### <a name="generate-resource-files-with-visual-studio"></a>Generowanie plików zasobów z programem Visual Studio

Jeśli utworzysz plik zasobów w programie Visual Studio bez kultury w nazwie pliku (na przykład *Welcome.resx*), programu Visual Studio utworzy klasę C# z właściwością dla każdego ciągu. To zwykle nie chcesz z platformy ASP.NET Core. Zwykle nie ma wartości domyślnej *.resx* pliku zasobów ( *.resx* pliku bez nazwy kultury). Zaleca się tworzenia *.resx* plik o nazwie kultury (na przykład *Welcome.fr.resx*). Po utworzeniu *.resx* plik o nazwie kultury, Visual Studio nie będzie Generowanie pliku klasy. Przewidujemy, że wielu deweloperów nie utworzy plik domyślny język zasobu.

### <a name="add-other-cultures"></a>Dodawanie innych kultur

Każda kombinacja języka i kultury (innego niż domyślny język) wymaga pliku unikatowy zasób. Tworzenie plików zasobów dla innych kultur i ustawień regionalnych przez tworzenie nowych plików zasobów, w których kodów języka ISO należą do nazwy pliku (na przykład **en-us**, **fr-ca**, i  **pl pl.**). Te kody ISO umieszcza się pomiędzy nazwą i *.resx* pliku rozszerzenie, podobnie jak w *Welcome.es MX.resx* Hiszpański/Meksyk ().

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a>Wdrożenie strategii wybierz języka/kultury dla każdego żądania

### <a name="configure-localization"></a>Konfigurowanie lokalizacji

Lokalizacja jest skonfigurowana w `ConfigureServices` metody:

[!code-csharp[](localization/sample/Localization/Program.cs?name=snippet1)]

* `AddLocalization` Dodaje usługi lokalizacji do kontenera usług. Powyższy kod także ustawia ścieżkę zasobów do "Zasoby".

* `AddViewLocalization` Dodaje obsługę wyświetlanie zlokalizowanych plików. W tym widoku próbki lokalizacja jest oparta na sufiks widoku pliku. Na przykład "fr" w *Index.fr.cshtml* pliku.

* `AddDataAnnotationsLocalization` Dodaje wsparcie dla zlokalizowane `DataAnnotations` weryfikacji komunikaty za pośrednictwem `IStringLocalizer` obiektów abstrakcyjnych.

### <a name="localization-middleware"></a>Lokalizacja oprogramowania pośredniczącego

Bieżąca kultura na żądanie znajduje się w lokalizacji [oprogramowanie pośredniczące](xref:fundamentals/middleware/index). Oprogramowanie pośredniczące lokalizacji jest włączone w `Configure` metody. Oprogramowanie pośredniczące lokalizacja musi być skonfigurowana przed wszelkie oprogramowanie pośredniczące, które może sprawdzić kultury żądania (na przykład `app.UseMvcWithDefaultRoute()`).

[!code-csharp[](localization/sample/Localization/Program.cs?name=snippet2)]

`UseRequestLocalization` Inicjuje `RequestLocalizationOptions` obiektu. Na każde żądanie listy z `RequestCultureProvider` w `RequestLocalizationOptions` wyliczeniu i pierwszy dostawca, który pomyślnie można określić kulturę żądania jest używany. Domyślnych dostawców pochodzą z `RequestLocalizationOptions` klasy:

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

Na domyślnej liście przechodzi od najbardziej do najmniej specyficznych. W dalszej części tego artykułu możemy Zobacz jak zmienić kolejność i nawet dodać dostawcę kultura niestandardowa. Jeśli żaden z dostawców można określić kulturę żądania `DefaultRequestCulture` jest używany.

### <a name="querystringrequestcultureprovider"></a>QueryStringRequestCultureProvider

Niektóre aplikacje użyje ciąg zapytania można ustawić [kultury i kultury interfejsu użytkownika](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx). W przypadku aplikacji korzystających z pliku cookie lub podejście nagłówek Accept-Language dodanie ciągu zapytania do adresu URL jest przydatne w przypadku debugowania i testowania kodu. Domyślnie `QueryStringRequestCultureProvider` jest zarejestrowany jako pierwszy dostawca lokalizacji w `RequestCultureProvider` listy. Przekazywanie parametrów ciągu zapytania `culture` i `ui-culture`. Poniższy przykład przedstawia określoną kulturę (region i język) Hiszpański/Meksyk:

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

Jeśli możesz przekazać tylko w jednym z dwóch (`culture` lub `ui-culture`), dostawca ciąg zapytania zostanie ustawiony obie wartości przy użyciu jednego przekazany w. Na przykład ustawienie kultury właśnie ustawi zarówno `Culture` i `UICulture`:

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a>CookieRequestCultureProvider

Aplikacje w środowisku produkcyjnym zostanie często udostępniają mechanizm można ustawić kultury z plikiem cookie kultury platformy ASP.NET Core. Użyj `MakeCookieValue` metodę w celu utworzenia pliku cookie.

`CookieRequestCultureProvider` `DefaultCookieName` Informacji kultury zwraca preferowana przez domyślnej nazwy pliku cookie używane w celu śledzenia użytkowników. Domyślna nazwa pliku cookie jest `.AspNetCore.Culture`.

Format pliku cookie jest `c=%LANGCODE%|uic=%LANGCODE%`, gdzie `c` jest `Culture` i `uic` jest `UICulture`, na przykład:

    c=en-UK|uic=en-US

Jeśli określono tylko jedna z informacji kultury lub kultury interfejsu użytkownika, określonej kultury będzie używany dla kultury informacji i kultury interfejsu użytkownika.

### <a name="the-accept-language-http-header"></a>Nagłówek Accept-Language HTTP

[Nagłówka Accept-Language](https://www.w3.org/International/questions/qa-accept-lang-locales) można ustawić w większości przeglądarek i pierwotnie był przeznaczony do Określ język użytkownika. To ustawienie wskazuje co przeglądarki została ustawiona do wysłania lub odziedziczył system operacyjny. Nagłówek Accept-Language HTTP z żądanie przeglądarki nie jest infallible sposób, aby wykryć preferowanego języka użytkownika (zobacz [ustawienie Preferencje językowe w przeglądarce](https://www.w3.org/International/questions/qa-lang-priorities.en.php)). Aplikacji produkcyjnej powinny uwzględniać sposób, aby użytkownik mógł dostosować wybór kultury.

### <a name="set-the-accept-language-http-header-in-ie"></a>Ustaw nagłówek Accept-Language HTTP w programie Internet Explorer

1. Na koło zębate ikonę, naciśnij **Opcje internetowe**.

2. Wybierz **języków**.

    ![Opcje internetowe](localization/_static/lang.png)

3. Wybierz **Ustaw preferencje językowe**.

4. Wybierz **dodać język**.

5. Dodaj język.

6. Wybierz język, a następnie wybierz **Przenieś w górę**.

### <a name="use-a-custom-provider"></a>Użyj niestandardowego dostawcy

Załóżmy, że chcesz umożliwić klientom przechowywanie ich języka i kultury w bazach danych. Można zapisać dostawcy wyszukiwania te wartości dla użytkownika. Poniższy kod przedstawia sposób dodawania niestandardowego dostawcy:

```csharp
private const string enUSCulture = "en-US";

services.Configure<RequestLocalizationOptions>(options =>
{
    var supportedCultures = new[]
    {
        new CultureInfo(enUSCulture),
        new CultureInfo("fr")
    };

    options.DefaultRequestCulture = new RequestCulture(culture: enUSCulture, uiCulture: enUSCulture);
    options.SupportedCultures = supportedCultures;
    options.SupportedUICultures = supportedCultures;

    options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
});
```

Użyj `RequestLocalizationOptions` Aby dodać lub usunąć lokalizacji dostawcy.

### <a name="set-the-culture-programmatically"></a>Ustaw programowo kultury

W tym przykładzie **Localization.StarterWeb** projektu na [GitHub](https://github.com/aspnet/entropy) zawiera interfejsu użytkownika można ustawić `Culture`. *Views/Shared/_SelectLanguagePartial.cshtml* pliku można wybrać z listy obsługiwanych kultur kultura:


[!code-cshtml[](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

*Views/Shared/_SelectLanguagePartial.cshtml* plik zostanie dodany do `footer` sekcji pliku układu, będą dostępne dla wszystkich widoków:

[!code-cshtml[](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

`SetLanguage` Metoda ustawia plik cookie kultury.

[!code-csharp[](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

Nie można dołączyć *_SelectLanguagePartial.cshtml* do przykładowy kod dla tego projektu. **Localization.StarterWeb** projektu na [GitHub](https://github.com/aspnet/entropy) ma kod przepływ `RequestLocalizationOptions` do częściowego za pośrednictwem Razor [iniekcji zależności](dependency-injection.md) kontenera.

## <a name="globalization-and-localization-terms"></a>Globalizacja i lokalizacja

Proces lokalizowania aplikacji wymaga również podstawową wiedzę na temat zestawów znaków odpowiednich często używane w nowoczesnych programowania i zrozumienia problemów związanych z nimi. Mimo że wszystkie komputery przechowywanie tekstu w postaci liczb (kody), różnych systemów przechowywania tego samego tekstu przy użyciu innej liczby. Proces lokalizacji odwołuje się do tłumaczenia aplikacji interfejsu użytkownika (UI) dla określonej kultury/ustawień regionalnych.

[Możliwość lokalizacji](https://docs.microsoft.com/dotnet/standard/globalization-localization/localizability-review) pośredniego proces weryfikacji, że uniwersalnych aplikacji jest gotowy do lokalizacji.

[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) format nazwy kultury jest `<languagecode2>-<country/regioncode2>`, gdzie `<languagecode2>` jest kod języka i `<country/regioncode2>` przeszczepiania kodu. Na przykład `es-CL` dla języka hiszpańskiego (podrzędnej lokacji), `en-US` angielski (Stany Zjednoczone), a `en-AU` dla języka angielskiego (Australia). [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) jest kombinacją ISO 639 Kod kultury małe dwuliterowych skojarzone z językiem normy ISO 3166, kod przeszczepiania wielkie dwuliterowych skojarzone z kraju lub regionu. Zobacz [nazwa kultury języka](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).

Przystosowywanie do warunków międzynarodowych jest często skracana do "I18N". Skrót przyjmuje litery imię i nazwisko, a liczba liter między nimi tak 18 oznacza liczbę litery od pierwszego "I" i ostatniego "N". To samo dotyczy (G11N), lokalizacja i globalizacja (L10N).

Warunki:

* Globalizacja (G11N): Proces polegający na wprowadzaniu aplikacji obsługi różnych języków i regionów.
* Lokalizacja (L10N): Proces dostosowywania aplikacji dla danego języka i regionu.
* Przystosowywanie do warunków międzynarodowych (I18N): w tym artykule opisano lokalizacja i globalizacja.
* Kultura: Jest język i, opcjonalnie, region.
* Określa kulturę neutralną: kultura, która ma określony język, ale nie regionu. (na przykład "en", "es")
* Określonych kultura: kultura, która ma określony język i region. (na przykład "en US", "pl pl.", "es-CL")
* Element nadrzędny kultury: kultury neutralnej, który zawiera określoną kulturę. (na przykład "en" to Kultura nadrzędna "en US" i "pl pl.")
* Ustawienia regionalne: Ustawień regionalnych jest taka sama jak kultury.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Projekt Localization.StarterWeb](https://github.com/aspnet/entropy) używane w artykule.
* [Pliki zasobów w programie Visual Studio](https://docs.microsoft.com/cpp/windows/resource-files-visual-studio)
* [Zasoby w pliki .resx](https://docs.microsoft.com/dotnet/framework/resources/working-with-resx-files-programmatically)
* [Zestaw narzędzi firmy Microsoft wielojęzyczny aplikacji](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
