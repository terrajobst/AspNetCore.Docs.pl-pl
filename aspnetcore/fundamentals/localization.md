---
title: Globalizacja i lokalizacja w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak platforma ASP.NET Core oferuje usługi oraz oprogramowanie pośredniczące lokalizacja zawartości do różnych języków i kultur.
ms.author: riande
ms.date: 01/14/2017
uid: fundamentals/localization
ms.openlocfilehash: 6e8c4723ab0105b8c756221d3e3c5eebba6cc4e2
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912040"
---
# <a name="globalization-and-localization-in-aspnet-core"></a>Globalizacja i lokalizacja w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [linkach Damien](https://twitter.com/damien_bod), [Calixto Roman](https://twitter.com/bartmax), [Nadeem Afana](https://twitter.com/NadeemAfana), i [Ateya Hisham pojemnika](https://twitter.com/hishambinateya)

Tworzenie wielojęzycznych witryny sieci Web za pomocą platformy ASP.NET Core Twoja witryna będzie dotrzeć do większej liczby osób. ASP.NET Core oferuje usługi oraz oprogramowanie pośredniczące lokalizowanie w różnych językach i kultur.

Obejmuje internacjonalizacji [globalizacji](/dotnet/api/system.globalization) i [lokalizacji](/dotnet/standard/globalization-localization/localization). Globalizacja to proces projektowania aplikacji, które obsługują różne kultury. Globalizacja dodaje obsługę danych wejściowych, wyświetlają i danych wyjściowych ze zdefiniowanym zestawem skrypty języka, które odnoszą się do określonych obszarów geograficznych.

Lokalizacja jest proces dostosowywania uniwersalnych aplikacji, która już przeprowadzono dla przeglądu możliwości lokalizacji określonej kultury/ustawień regionalnych. Aby uzyskać więcej informacji, zobacz **warunki lokalizacja i globalizacja** pod koniec tego dokumentu.

Lokalizacja aplikacji obejmuje następujące czynności:

1. Wprowadź nazwę zawartości aplikacji możliwych do zlokalizowania

2. Podaj zlokalizowane zasoby dla języków i kultur, która jest obsługiwana

3. Wdrożenie strategii, aby wybrać język/kulturę dla każdego żądania

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/localization/sample/Localization) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="make-the-apps-content-localizable"></a>Wprowadź nazwę zawartości aplikacji możliwych do zlokalizowania

Wprowadzona w programie ASP.NET Core `IStringLocalizer` i `IStringLocalizer<T>` zostały architektura zaprojektowana pod kątem poprawiania produktywności podczas tworzenia zlokalizowanych aplikacji. `IStringLocalizer` używa [ResourceManager](/dotnet/api/system.resources.resourcemanager) i [ResourceReader](/dotnet/api/system.resources.resourcereader) zapewnienie specyficznych dla kultury zasobów w czasie wykonywania. Prosty interfejs ma indeksatora i `IEnumerable` dla zwracania zlokalizowane ciągi. `IStringLocalizer` nie wymaga przechowywania ciągów języka domyślnego w pliku zasobów. Można programowanie aplikacji przeznaczone dla lokalizacji i nie trzeba tworzyć pliki zasobów na wczesnym etapie projektowania. Poniższy kod pokazuje, jak opakowywać ciąg "Title o" do lokalizacji.

[!code-csharp[](localization/sample/Localization/Controllers/AboutController.cs)]

W powyższym kodzie `IStringLocalizer<T>` implementacji pochodzi z [wstrzykiwanie zależności](dependency-injection.md). Jeśli nie zostanie znaleziona zlokalizowaną wartość "Title o", a następnie klucz indeksatora, zostaną zwrócone, oznacza to, ciąg "Title o". Można pozostaw wartość domyślną ciągi literałowe języka w aplikacji i opakowywanie ich lokalizatorowi, dzięki czemu można skupić się na temat tworzenia aplikacji. Opracuj swoją aplikację z językiem domyślnym i przygotować je do kroku lokalizacji bez tworzenia domyślnego pliku zasobów. Alternatywnie można użyć tradycyjne podejście i podaj klucz, aby pobrać domyślny ciąg języka. Wielu deweloperów na nowy przepływ pracy nie ma domyślnego języka *resx* pliku i po prostu opakowywanie literały ciągów znaków oraz może zmniejszyć obciążenie lokalizowania aplikacji. Inni deweloperzy preferowała przepływu pracy tradycyjnych, zgodnie z jego może ułatwić pracę z dłużej literały ciągów i ułatwić aktualizowanie zlokalizowane ciągi.

Użyj `IHtmlLocalizer<T>` implementację dla zasobów, które zawierają HTML. `IHtmlLocalizer` HTML koduje argumenty, które są sformatowane w ciągu zasobu, ale nie sam ciąg zasobu kodowanie HTML. W przykładzie, które przedstawiono poniżej, tylko wartość `name` parametr ma kodowania HTML.

[!code-csharp[](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

**Uwaga:** zazwyczaj chcesz tylko lokalizowanie tekst i HTML nie.

Na najniższym poziomie, możesz uzyskać `IStringLocalizerFactory` poza [wstrzykiwanie zależności](dependency-injection.md):

[!code-csharp[](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

Powyższy kod demonstruje każdej fabryki dwie metody tworzenia.

Można podzielić zlokalizowanych ciągów przez kontroler, obszaru, lub mieć tylko jeden kontener. W przykładowej aplikacji o nazwie fikcyjną klasy `SharedResource` służy do udostępnionych zasobów.

[!code-csharp[](localization/sample/Localization/Resources/SharedResource.cs)]

Niektórzy deweloperzy użyj `Startup` klasa może zawierać ciągi globalne lub współdzielonej. W poniższym przykładzie `InfoController` i `SharedResource` lokalizatorzy służą:

[!code-csharp[](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a>Lokalizacja widoku

`IViewLocalizer` Usługi zawiera ciągi zlokalizowane dla [widoku](xref:mvc/views/overview). `ViewLocalizer` Klasa implementuje ten interfejs i znajduje się lokalizacja zasobu ze ścieżki plików widoku. Poniższy kod przedstawia sposób użycia domyślna Implementacja klasy `IViewLocalizer`:

[!code-cshtml[](localization/sample/Localization/Views/Home/About.cshtml)]

Domyślna implementacja klasy `IViewLocalizer` umożliwia znalezienie pliku zasobów na podstawie nazwy pliku dla widoku. Nie ma możliwości używania pliku globalnego zasobu udostępnionego. `ViewLocalizer` implementuje lokalizatorowi przy użyciu `IHtmlLocalizer`, więc Razor nie HTML zlokalizowany ciąg do zakodowania. Możecie ciągi zasobów i `IViewLocalizer` będzie kodowanie HTML parametrów, ale nie ciągu zasobu. Należy wziąć pod uwagę następujące znaczniki Razor:

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

Plik francuska zasobów może zawierać następujące informacje:

| Key | Wartość |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b> ` |

W widoku renderowanym zawierałoby kod znaczników HTML z pliku zasobów.

**Uwaga:** zazwyczaj chcesz tylko lokalizowanie tekst i HTML nie.

Aby użyć pliku udostępnionego zasobu w widoku, należy wstrzyknąć `IHtmlLocalizer<T>`:

[!code-cshtml[](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a>Lokalizacja DataAnnotations

Komunikaty o błędach DataAnnotations są lokalizowane za pomocą `IStringLocalizer<T>`. Za pomocą opcji `ResourcesPath = "Resources"`, komunikaty o błędach w `RegisterViewModel` mogą być przechowywane w jednej z następujących ścieżek:

* *Resources/ViewModels.Account.RegisterViewModel.fr.resx*
* *Resources/ViewModels/Account/RegisterViewModel.fr.resx*

[!code-csharp[](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

ASP.NET Core MVC 1.1.0 i nowszej, krzyżowa Weryfikacja atrybuty są zlokalizowane. Platforma ASP.NET Core MVC 1.0 jest **nie** wyszukiwania zlokalizowanych ciągów dla atrybutów bez sprawdzania poprawności.

<a name="one-resource-string-multiple-classes"></a>
### <a name="using-one-resource-string-for-multiple-classes"></a>Przy użyciu jednego ciągu zasobu dla wielu klas

Poniższy kod przedstawia sposób używania jednego ciągu zasobu dla atrybutów sprawdzania poprawności z wieloma klasami:

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

W kodzie poprzedzającej `SharedResource` jest klasa odpowiadający resx przechowywania wiadomości sprawdzania poprawności. W tym podejściu DataAnnotations będzie używać tylko `SharedResource`, zamiast zasobów dla każdej klasy.

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a>Podaj zlokalizowane zasoby dla języków i kultur, która jest obsługiwana

### <a name="supportedcultures-and-supporteduicultures"></a>SupportedCultures i SupportedUICultures

Platforma ASP.NET Core pozwala określić dwie wartości kultury, `SupportedCultures` i `SupportedUICultures`. [CultureInfo](/dotnet/api/system.globalization.cultureinfo) dla obiektu `SupportedCultures` określi wyniki funkcje zależne od kultury, takie jak daty, godziny, numer i formatowania waluty. `SupportedCultures` Określa również kolejność sortowania tekstu, konwencje obudowy i porównywania ciągów. Zobacz [wartość CultureInfo.CurrentCulture](/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) uzyskać więcej informacji dotyczących sposobu serwera pobiera kultury. `SupportedUICultures` Określa, która przekształca ciągi (z *resx* pliki) są wyszukiwane przez [ResourceManager](/dotnet/api/system.resources.resourcemanager). `ResourceManager` Po prostu wyszukuje ciągów specyficzne dla kultury, które jest określany przez `CurrentUICulture`. Każdy wątek na platformie .NET ma `CurrentCulture` i `CurrentUICulture` obiektów. Platforma ASP.NET Core bada tych wartości podczas renderowania funkcje zależne od kultury. Na przykład, jeśli kultury bieżącego wątku jest ustawiona na "en US" (angielski, Stany Zjednoczone) `DateTime.Now.ToLongDateString()` Wyświetla "Czwartek, 18 luty 2016", ale jeśli `CurrentCulture` jest ustawiona na "es-ES" (hiszpański, Hiszpania) dane wyjściowe będą "jueves, de febrero 18 de 2016".

## <a name="resource-files"></a>Pliki zasobów

Plik zasobów jest przydatny mechanizm do oddzielania możliwych do zlokalizowania ciągi z kodu. Przetłumaczone ciągi dla języków innych niż domyślne są izolowane *resx* plików zasobów. Na przykład możesz chcieć utworzyć plik hiszpański zasobów o nazwie *Welcome.es.resx* zawierający przetłumaczone ciągi. "es" jest kod języka hiszpańskiego. Aby utworzyć ten plik zasobów w programie Visual Studio:

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy folder, który będzie zawierać plik zasobu > **Dodaj** > **nowy element**.

    ![Menu kontekstowe zagnieżdżonych: W Eksploratorze rozwiązań menu kontekstowe jest otwarte dla zasobów. Drugi menu kontekstowe jest otwarty dla Dodaj pokazujący wyróżnione polecenie Nowy element.](localization/_static/newi.png)

2. W **Wyszukaj zainstalowane szablony** pole, wprowadź "resource" i nazwij plik.

    ![Dodaj okno dialogowe Nowy element](localization/_static/res.png)

3. Wprowadź wartość klucza (ciąg natywny) w **nazwa** kolumny i przetłumaczonego ciągu w **wartość** kolumny.

    ![Plik Welcome.ES.resx (plik zasobów-Zapraszamy hiszpański) z wyraz Hello w kolumnie Nazwa i word Hola (Hello w języku hiszpańskim) w kolumnie wartości](localization/_static/hola.png)

    Visual Studio Wyświetla *Welcome.es.resx* pliku.

    ![Plik zasobów-Zapraszamy hiszpański (es) Eksplorator rozwiązań](localization/_static/se.png)

## <a name="resource-file-naming"></a>Nazywanie pliku zasobu

Zasoby są nazywane dla Pełna nazwa typu klasy, ich minus nazwę zestawu. Na przykład francuski zasobów w projekcie, w których główny zestaw jest `LocalizationWebsite.Web.dll` dla klasy `LocalizationWebsite.Web.Startup` będą miały postać *Startup.fr.resx*. Zasób klasy `LocalizationWebsite.Web.Controllers.HomeController` będą miały postać *Controllers.HomeController.fr.resx*. Jeśli klasa docelowa przestrzeń nazw nie jest taka sama jak nazwa zestawu konieczne będzie pełna nazwa typu. Na przykład w próbce projektu zasobów dla typu `ExtraNamespace.Tools` będą miały postać *ExtraNamespace.Tools.fr.resx*.

W przykładowym projekcie `ConfigureServices` metody ustawia `ResourcesPath` "Zasoby", więc ścieżka względna projektu pliku zasobów francuska głównego kontrolera jest *Resources/Controllers.HomeController.fr.resx*. Alternatywnie można użyć folderów do organizowania plików zasobów. Dla głównego kontrolera byłaby to ścieżka *Resources/Controllers/HomeController.fr.resx*. Jeśli nie używasz `ResourcesPath` opcji *resx* przejdzie pliku w katalogu podstawowego projektu. Plik zasobów dla `HomeController` będą miały postać *Controllers.HomeController.fr.resx*. Do wyboru używania konwencji nazewnictwa kropka lub ścieżka zależy od tego, sposobu organizowania plików zasobów.

| Nazwa zasobu | Kropka lub nazwy ścieżki |
| ------------   | ------------- |
| Resources/Controllers.HomeController.fr.resx | Kropka  |
| Resources/Controllers/HomeController.fr.resx  | Ścieżka |
|    |     |

Pliki zasobów przy użyciu `@inject IViewLocalizer` widokami Razor wykonaj podobny wzorzec. Plik zasobów w celu wyświetlenia może być nazwany przy użyciu nazwy ścieżki lub nazewnictwa z dot. Pliki zasobów widoku razor naśladować ścieżkę pliku ich skojarzonego widoku. Zakładając, że ustawiliśmy `ResourcesPath` "Zasoby", plik zasobów francuska skojarzona z *Views/Home/About.cshtml* widok może być jedną z następujących czynności:

* Resources/Views/Home/About.fr.resx

* Resources/Views.Home.About.fr.resx

Jeśli nie używasz `ResourcesPath` opcji *resx* plik widoku będzie znajdować się w tym samym folderze co widoku.

### <a name="rootnamespaceattribute"></a>RootNamespaceAttribute 

[RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1) atrybut zawiera główna przestrzeń nazw zestawu, gdy główna przestrzeń nazw zestawu jest inny niż nazwa zestawu. 

Jeśli główna przestrzeń nazw zestawu jest inna niż nazwa zestawu:

* Lokalizacja nie obsługuje domyślnie.
* Lokalizacja kończy się niepowodzeniem ze względu na sposób zasoby są wyszukiwane w obrębie zestawu. `RootNamespace` jest to wartość czas kompilacji, który nie jest dostępny do wykonywania procesu. 

Jeśli `RootNamespace` różni się od `AssemblyName`, obejmują następujące elementy w *AssemblyInfo.cs* (z wartościami parametru zastąpione rzeczywistymi wartościami):

```Csharp
using System.Reflection;
using Microsoft.Extensions.Localization;

[assembly: ResourceLocation("Resource Folder Name")]
[assembly: RootNamespace("App Root Namespace")]
```

Powyższy kod umożliwia pomyślnego rozwiązania pliki resx.

## <a name="culture-fallback-behavior"></a>Działanie procedury rezerwowej kultury

Podczas wyszukiwania zasobu, lokalizacja uczestniczy w "kultury rezerwową". Począwszy od żądaną kulturę, jeśli nie można odnaleźć, przywraca kultury nadrzędnej tej kultury. Jako aside [CultureInfo.Parent](/dotnet/api/system.globalization.cultureinfo.parent) właściwość reprezentuje kulturę nadrzędnej. Oznacza to, zwykle (ale nie zawsze) usuwanie national signifier ISO. Na przykład dialekt hiszpański używany w Meksyku jest "es-" MX". Ma element nadrzędny "es"&mdash;hiszpański nieokreślonym w dowolnym kraju.

Załóżmy, że Twoja witryna odbiera żądanie dotyczące zasobów "Witaj", przy użyciu kultury "fr-CA". System lokalizacji wyglądu dla następujących zasobów, w kolejności i wybiera pierwsze dopasowanie:

* *Welcome.fr-CA.resx*
* *Welcome.fr.resx*
* *Welcome.resx* (Jeśli `NeutralResourcesLanguage` jest "fr-CA")

Na przykład jeśli masz kultury ustawione na język francuski Usuń oznaczenie kultury ".fr", domyślny plik zasobów jest do odczytu i są zlokalizowane ciągi. Usługi Resource manager Określa domyślne lub bazoey zasoby dla, jeśli nic nie spełnia Twoje żądaną kulturę. Jeśli chcesz po prostu zwraca klucz, jeśli brakuje zasobu dla żądanej kulturze nie może mieć domyślnego pliku zasobów.

### <a name="generate-resource-files-with-visual-studio"></a>Generowanie plików zasobów z programem Visual Studio

Jeśli utworzysz plik zasobów w programie Visual Studio bez kultury w nazwie pliku (na przykład *Welcome.resx*), Visual Studio utworzy klasy C# za pomocą właściwości dla każdego ciągu. To zwykle nie chcemy za pomocą programu ASP.NET Core. Zwykle nie mają domyślnie *resx* pliku zasobów ( *resx* plik bez nazwy kultury). Sugerujemy utworzenie *resx* plik o nazwie kultury (na przykład *Welcome.fr.resx*). Po utworzeniu *resx* plik o nazwie kultury, Visual Studio nie będzie Generowanie pliku klasy. Przewidujemy, że wielu programistów nie utworzy plikowi zasobów języka domyślnego.

### <a name="add-other-cultures"></a>Dodawanie innych kultur

Każda kombinacja języka i kultury (innego niż domyślny język) wymaga pliku unikatowy zasób. Tworzenie plików zasobów dla różnych kultur i ustawień regionalnych, tworząc nowe pliki zasobów, w których kodów ISO języka są częścią nazwy pliku (na przykład **en-us**, **fr-ca**, i  **en-gb**). Te kody ISO są umieszczane między nazwę pliku i *resx* pliku rozszerzenie, podobnie jak w *Welcome.es MX.resx* (Hiszpański/Meksyk).

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a>Wdrożenie strategii, aby wybrać język/kulturę dla każdego żądania

### <a name="configure-localization"></a>Konfigurowanie lokalizacji

Lokalizacja jest skonfigurowana w `ConfigureServices` metody:

[!code-csharp[](localization/sample/Localization/Program.cs?name=snippet1)]

* `AddLocalization` Dodaje usługi lokalizacji do kontenera usług. Powyższy kod ustawia również ścieżkę zasobów "Zasoby".

* `AddViewLocalization` Dodaje obsługę zlokalizowany widok plików. W tym widoku przykładowe lokalizacji opiera się na widok sufiks pliku. Na przykład "fr" w *Index.fr.cshtml* pliku.

* `AddDataAnnotationsLocalization` Dodaje obsługę lokalizowanej `DataAnnotations` weryfikacji komunikaty za pośrednictwem `IStringLocalizer` abstrakcje.

### <a name="localization-middleware"></a>Oprogramowanie pośredniczące lokalizacji

Bieżąca kultura na żądanie znajduje się w lokalizacji [oprogramowania pośredniczącego](xref:fundamentals/middleware/index). Oprogramowanie pośredniczące lokalizacji jest włączona w `Configure` metody. Oprogramowanie pośredniczące lokalizacja musi być skonfigurowana przed wszelkie oprogramowanie pośredniczące, które może również obejmować kontrolę kultury żądania (na przykład `app.UseMvcWithDefaultRoute()`).

[!code-csharp[](localization/sample/Localization/Program.cs?name=snippet2)]

`UseRequestLocalization` Inicjuje `RequestLocalizationOptions` obiektu. Na każde żądanie listy z `RequestCultureProvider` w `RequestLocalizationOptions` wyliczenia i pierwszym dostawcą, który pomyślnie można określić kulturę żądania jest używana. Pochodzą domyślnych dostawców `RequestLocalizationOptions` klasy:

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

Domyślna lista przechodzi od najbardziej do najmniej specyficznych. W dalszej części tego artykułu zobaczymy, jak zmienić kolejność i nawet dodać dostawcę kultury niestandardowej. Jeśli żaden z dostawców można określić kulturę żądania `DefaultRequestCulture` jest używany.

### <a name="querystringrequestcultureprovider"></a>QueryStringRequestCultureProvider

Niektóre aplikacje użyje ciąg zapytania do zestawu [kultury i kultury UI](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx). Aplikacje, które używają plików cookie lub podejście nagłówek Accept-Language dodając ciąg zapytania do adresu URL jest przydatne w przypadku debugowania i testowania kodu. Domyślnie `QueryStringRequestCultureProvider` jest zarejestrowany jako pierwszy dostawca lokalizacji w `RequestCultureProvider` listy. Przekazywanie parametrów ciągu zapytania `culture` i `ui-culture`. W poniższym przykładzie ustawiono określonej kultury (region i język), hiszpański/Meksyku:

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

Jeśli przekażesz w jednym z dwóch (`culture` lub `ui-culture`), dostawcy ciągu zapytania używanego ustawi obie wartości przekazywane w użyciu. Na przykład ustawienie kultury po prostu ustawi zarówno `Culture` i `UICulture`:

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a>CookieRequestCultureProvider

Aplikacje produkcyjne będą często udostępniają mechanizm ustawienie kultury z plikiem cookie kultury platformy ASP.NET Core. Użyj `MakeCookieValue` metodę w celu utworzenia pliku cookie.

`CookieRequestCultureProvider` `DefaultCookieName` Zwraca preferowanego domyślnej nazwy pliku cookie używane do śledzenia użytkownika kulturze informacji. Domyślną nazwą pliku cookie jest `.AspNetCore.Culture`.

Format pliku cookie jest `c=%LANGCODE%|uic=%LANGCODE%`, gdzie `c` jest `Culture` i `uic` jest `UICulture`, na przykład:

    c=en-UK|uic=en-US

Jeśli określono tylko jeden z informacje o kulturze i kultury UI, określonej kultury będzie służyć zarówno informacje o kulturze, jak i kultura interfejsu użytkownika.

### <a name="the-accept-language-http-header"></a>Nagłówek Accept-Language HTTP

[Nagłówek Accept-Language](https://www.w3.org/International/questions/qa-accept-lang-locales) można ustawić w większości przeglądarek i była pierwotnie przeznaczona do określenia języka użytkownika. To ustawienie wskazuje, jakie przeglądarki został ustawiony na wysyłanie lub odziedziczył zasadniczego systemu operacyjnego. Nagłówek Accept-Language HTTP z żądanie przeglądarki nie jest infallible sposób, aby wykryć język preferowany przez użytkownika (zobacz [ustawienia preferencji językowych w przeglądarce](https://www.w3.org/International/questions/qa-lang-priorities.en.php)). Aplikacji produkcyjnej powinno obejmować sposób na dostosowywanie wybranych przez nich kultury użytkownika.

### <a name="set-the-accept-language-http-header-in-ie"></a>Ustaw nagłówek Accept-Language HTTP w programie Internet Explorer

1. Na ikonę koła zębatego, naciśnij **Opcje internetowe**.

2. Naciśnij pozycję **języków**.

    ![Opcje internetowe](localization/_static/lang.png)

3. Naciśnij pozycję **Ustaw preferencje językowe**.

4. Naciśnij pozycję **Dodaj język**.

5. Dodaj język.

6. Wybierz język, a następnie naciśnij **Przenieś w górę**.

### <a name="use-a-custom-provider"></a>Użyj niestandardowego dostawcy

Załóżmy, że chcesz umożliwić klientom przechowywanie ich języka i kultury w bazach danych. Można zapisać dostawcy, aby wyszukać te wartości dla użytkownika. Poniższy kod przedstawia sposób dodawania niestandardowego dostawcy:

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

Użyj `RequestLocalizationOptions` do dodawania lub usuwania lokalizacji dostawcy.

### <a name="set-the-culture-programmatically"></a>Programowe Ustawianie kultury

W tym przykładzie **Localization.StarterWeb** project [GitHub](https://github.com/aspnet/entropy) zawiera interfejs użytkownika, aby ustawić `Culture`. *Views/Shared/_SelectLanguagePartial.cshtml* pliku służy do wybierania kultury z listy obsługiwanych kultur:


[!code-cshtml[](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

*Views/Shared/_SelectLanguagePartial.cshtml* plik zostanie dodany do `footer` sekcji pliku układu, dzięki czemu będzie on dostępny do wszystkich widoków:

[!code-cshtml[](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

`SetLanguage` Metoda ustawia plik cookie kultury.

[!code-csharp[](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

Nie można podłączyć *_SelectLanguagePartial.cshtml* do przykładowego kodu dla tego projektu. **Localization.StarterWeb** project [GitHub](https://github.com/aspnet/entropy) zawiera kod służący do przepływu `RequestLocalizationOptions` do częściowego za pośrednictwem Razor [wstrzykiwanie zależności](dependency-injection.md) kontenera.

## <a name="globalization-and-localization-terms"></a>Globalizacja i lokalizacja warunki

Proces lokalizowania aplikacji wymaga również podstawową wiedzę na temat zestawów znaków istotne, często używane podczas tworzenia nowoczesnych oprogramowania i zrozumieniu zagadnień związanych z nimi. Mimo że wszystkie komputery przechowywanie tekstu jako liczby (kody), różnych systemów przechowywania ten sam tekst przy użyciu różnych liczb. Proces lokalizacji odnosi się do tłumaczenia aplikacji interfejsu użytkownika (UI) dla określonej kultury/ustawień regionalnych.

[Możliwości zlokalizowania](/dotnet/standard/globalization-localization/localizability-review) jest procesem pośrednie w celu sprawdzenia, czy aplikacji zglobalizowanej jest gotowy do lokalizacji.

[RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) formatowania jest nazwa kultury `<languagecode2>-<country/regioncode2>`, gdzie `<languagecode2>` jest kod języka i `<country/regioncode2>` znajduje się kod przeszczepiania. Na przykład `es-CL` dla hiszpański (Chile) `en-US` dla języka angielskiego (Stany Zjednoczone) i `en-AU` dla języka angielskiego (Australia). [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) jest kombinacją ISO 639 kodu kultury małe dwuliterowych skojarzone z językiem i ISO 3166 przeszczepiania wielkie dwuliterowych kodu skojarzone z kraju lub regionu. Zobacz [nazwa kultury języka](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).

Internacjonalizacja jest często skracana do "I18N". Skrót przyjmuje litery imię i nazwisko, a liczba litery między nimi tak 18 oznacza liczbę litery od pierwszego "I" i ostatnie "N". To samo dotyczy (G11N), lokalizacja i globalizacja (L10N).

Warunki:

* Globalizacja (G11N): Proces wprowadzania aplikacji pomocy technicznej w różnych językach i regionach.
* Lokalizacja (L10N): Proces dostosowywania aplikacji dla danego języka i regionu.
* Internacjonalizacja (I18N): w tym artykule opisano lokalizacja i globalizacja.
* Kultura: Jest język i, opcjonalnie, region.
* Kultury neutralnej: kultury, który ma określony język, ale nie w regionie. (na przykład "en", "es")
* Określonej kultury: kultura, który ma określony język i region. (na przykład "en US", "en-GB", "es-CL")
* Nadrzędny kultury: kultury neutralnej, który zawiera określoną kulturę. (na przykład "en" jest Kultura nadrzędna "en US" i "en-GB")
* Ustawienia regionalne: Ustawienia regionalne jest taka sama jak kultury.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Projekt Localization.StarterWeb](https://github.com/aspnet/Entropy/tree/master/samples/Localization.StarterWeb) używane w artykule.
* [Globalizacja i lokalizacja aplikacji .NET](/dotnet/standard/globalization-localization/index)
* [Zasoby w plikach resx](/dotnet/framework/resources/working-with-resx-files-programmatically)
* [Microsoft wielojęzyczny zestaw narzędzi aplikacji](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
