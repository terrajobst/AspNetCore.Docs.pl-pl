---
title: Interfejs użytkownika Razor wielokrotnego użytku w bibliotekach klas z ASP.NET Core
author: Rick-Anderson
description: Wyjaśnia, jak utworzyć interfejs użytkownika Razor wielokrotnego użytku przy użyciu częściowych widoków w bibliotece klas w ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/08/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: dcd24f7dafd198f88cdf84d1ab67c84f45428a95
ms.sourcegitcommit: d81912782a8b0bd164f30a516ad80f8defb5d020
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/09/2019
ms.locfileid: "72179333"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Utwórz interfejs użytkownika wielokrotnego użytku przy użyciu projektu biblioteki klas Razor w ASP.NET Core

Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

Widoki Razor, strony, kontrolery, modele stron, [składniki Razor](xref:blazor/class-libraries), [składniki widoku](xref:mvc/views/view-components)i modele danych mogą być wbudowane w bibliotekę klas Razor (RCL). RCL można spakować i ponownie użyć. Aplikacje mogą zawierać RCL i przesłonić widoki oraz strony, które zawiera. Gdy widok, częściowy widok lub strona Razor zostaną znalezione zarówno w aplikacji sieci Web, jak i w RCL, pierwszeństwo ma znacznik Razor (plik *. cshtml* ) w aplikacji sieci Web.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Tworzenie biblioteki klas zawierającej interfejs użytkownika Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt**>.
* Wybierz **ASP.NET Core aplikacji sieci Web**.
* Nazwij bibliotekę (na przykład "RazorClassLib") > **OK**. Aby uniknąć kolizji nazw plików z wygenerowanej biblioteki widoków, upewnij się, że nazwa biblioteki nie kończy się `.Views`.
* Sprawdź, czy wybrano **ASP.NET Core 3,0** lub nowszą.
* Wybierz **bibliotekę klas Razor** > **OK**.

Szablon biblioteki klas Razor (RCL) domyślnie jest domyślnym programowaniem składników Razor. Opcja szablonu w programie Visual Studio zapewnia obsługę szablonów dla stron i widoków.

# <a name="net-core-clitabnetcore-cli"></a>[interfejs wiersza polecenia platformy .NET Core](#tab/netcore-cli)

W wierszu polecenia Uruchom `dotnet new razorclasslib`. Na przykład:

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

Szablon biblioteki klas Razor (RCL) domyślnie jest domyślnym programowaniem składników Razor. Aby zapewnić obsługę stron i widoków, należy przekazać opcję `--support-pages-and-views` (`dotnet new razorclasslib --support-pages-and-views`).

Aby uzyskać więcej informacji, zobacz [dotnet New](/dotnet/core/tools/dotnet-new). Aby uniknąć kolizji nazw plików z wygenerowanej biblioteki widoków, upewnij się, że nazwa biblioteki nie kończy się `.Views`.

---

Dodaj pliki Razor do RCL.

Szablony ASP.NET Core założono, że zawartość RCL znajduje się w folderze *obszarów* . Zobacz [układ stron RCL](#rcl-pages-layout) , aby utworzyć RCL, który uwidacznia zawartość w `~/Pages`, a nie `~/Areas/Pages`.

## <a name="reference-rcl-content"></a>Odwołanie do zawartości RCL

RCL może odwoływać się do:

* Pakiet NuGet. Zobacz [Tworzenie pakietów NuGet](/nuget/create-packages/creating-a-package) i [dotnet Dodawanie pakietu](/dotnet/core/tools/dotnet-add-package) oraz [Tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{ProjectName}. csproj*. Zobacz sekcję [dotnet — Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).

## <a name="override-views-partial-views-and-pages"></a>Zastąp widoki, częściowe widoki i strony

Gdy widok, częściowy widok lub strona Razor zostaną znalezione zarówno w aplikacji sieci Web, jak i w RCL, pierwszeństwo ma znacznik Razor (plik *. cshtml* ) w aplikacji sieci Web. Na przykład Dodaj *WebApp1/Areas/webfeature/Pages/Strona1. cshtml* do WebApp1, a element Strona1 w WebApp1 będzie miał pierwszeństwo przed STRONA1 w RCL.

W przykładowym pobieraniu Zmień nazwę *WebApp1/obszarów/MyFeature2* na *WebApp1/Areas/webfeature* , aby przetestować priorytet.

Skopiuj widok części *RazorUIClassLib/Areas/Webfeature/Pages/Shared/_Message. cshtml* do obszaru WebApp1/refeatures/ *Shared/_Message. cshtml*. Zaktualizuj adiustację, aby wskazać nową lokalizację. Skompiluj i uruchom aplikację, aby sprawdzić, czy jest używana częściowa wersja aplikacji.

### <a name="rcl-pages-layout"></a>Układ stron RCL

Aby odwoływać się do zawartości RCL, tak jakby była częścią folderu *stron* aplikacji sieci Web, Utwórz projekt RCL o następującej strukturze plików:

* *RazorUIClassLib/strony*
* *RazorUIClassLib/strony/udostępnione*

Załóżmy, że *RazorUIClassLib/Pages/Shared* zawiera dwa częściowe pliki: *_Header. cshtml* i *_Footer. cshtml*. Tagi `<partial>` można dodać do pliku *_Layout. cshtml* :

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

## <a name="create-an-rcl-with-static-assets"></a>Tworzenie RCL przy użyciu statycznych zasobów

RCL może wymagać pomocnika zasobów statycznych, do których może odwoływać się aplikacja zużywana przez RCL. ASP.NET Core umożliwia tworzenie RCLs zawierających statyczne zasoby, które są dostępne dla aplikacji zużywanej.

Aby dołączyć zasoby towarzyszące jako część RCL, Utwórz folder *wwwroot* w bibliotece klas i Dołącz wszystkie wymagane pliki w tym folderze.

Podczas pakowania RCL wszystkie zasoby towarzyszące w folderze *wwwroot* zostaną automatycznie dołączone do pakietu.

### <a name="exclude-static-assets"></a>Wyklucz zasoby statyczne

Aby wykluczyć statyczne elementy zawartości, Dodaj żądaną ścieżkę wykluczenia do grupy właściwości `$(DefaultItemExcludes)` w pliku projektu. Oddzielaj wpisy średnikami (`;`).

W poniższym przykładzie arkusz stylów *lib. css* w folderze *wwwroot* nie jest traktowany jako statyczny zasób i nie jest uwzględniony w opublikowanym RCL:

```xml
<PropertyGroup>
  <DefaultItemExcludes>$(DefaultItemExcludes);wwwroot\lib.css</DefaultItemExcludes>
</PropertyGroup>
```

### <a name="typescript-integration"></a>Integracja języka TypeScript

Aby dołączyć pliki TypeScript do RCL:

1. Umieść pliki TypeScript ( *. TS*) poza folderem *wwwroot* . Na przykład Umieść pliki w folderze *Client* .

1. Skonfiguruj dane wyjściowe kompilacji TypeScript dla folderu *wwwroot* . Ustaw właściwość `TypescriptOutDir` wewnątrz `PropertyGroup` w pliku projektu:

   ```xml
   <TypescriptOutDir>wwwroot</TypescriptOutDir>
   ```

1. Dołącz obiekt docelowy TypeScript jako zależność elementu docelowego `ResolveCurrentProjectStaticWebAssets`, dodając następujący element docelowy wewnątrz `PropertyGroup` w pliku projektu:

   ```xml
  <ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
    CompileTypeScript;
    $(ResolveCurrentProjectStaticWebAssetsInputs)
  </ResolveCurrentProjectStaticWebAssetsInputsDependsOn>
   ```

### <a name="consume-content-from-a-referenced-rcl"></a>Korzystanie z zawartości z RCL, do którego istnieje odwołanie

Pliki znajdujące się w folderze *WWWROOT* RCL są uwidocznione dla aplikacji zużywanej pod prefiksem `_content/{LIBRARY NAME}/`. Na przykład biblioteka o nazwie *Razor. Class. lib* skutkuje ścieżką do zawartości statycznej w `_content/Razor.Class.Lib/`.

Aplikacja, której używa, odwołuje się do statycznych zasobów udostępnianych przez bibliotekę z `<script>`, `<style>`, `<img>` i innymi tagami HTML. Aplikacja do konsumowania musi mieć włączoną [obsługę pliku statycznego](xref:fundamentals/static-files) w `Startup.Configure`:

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

W przypadku uruchamiania aplikacji zużywanej z danych wyjściowych kompilacji (`dotnet run`) statyczne zasoby sieci Web są domyślnie włączone w środowisku deweloperskim. Aby zapewnić obsługę zasobów w innych środowiskach podczas uruchamiania z danych wyjściowych kompilacji, wywołaj `UseStaticWebAssets` na konstruktorze hosta w *program.cs*:

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStaticWebAssets();
                webBuilder.UseStartup<Startup>();
            });
}
```

Wywołanie `UseStaticWebAssets` nie jest wymagane podczas uruchamiania aplikacji z opublikowanych danych wyjściowych (`dotnet publish`).

### <a name="multi-project-development-flow"></a>Przepływ programistyczny dla wieloprojektowych

Po uruchomieniu aplikacji zużywanej przez aplikację:

* Zasoby w RCL pozostają w swoich oryginalnych folderach. Elementy zawartości nie są przenoszone do aplikacji zużywanej.
* Wszelkie zmiany w folderze *WWWROOT* RCL są odzwierciedlone w aplikacji zużywanej po odtworzeniu RCL i niepomyślnym skompilowaniu aplikacji.

Po skompilowaniu RCL jest tworzony manifest, który opisuje lokalizacje statycznego elementu zawartości sieci Web. Aplikacja, która korzysta z aplikacji, odczytuje manifest w czasie wykonywania, aby wykorzystać zasoby z przywoływanych projektów i pakietów. Po dodaniu nowego elementu zawartości do RCL należy ponownie skompilować RCL, aby zaktualizować jego manifest, zanim aplikacja zużywa dostęp do nowego elementu zawartości.

### <a name="publish"></a>Publikuj

Po opublikowaniu aplikacji składniki towarzyszące ze wszystkich przywoływanych projektów i pakietów są kopiowane do folderu *wwwroot* opublikowanej aplikacji w obszarze `_content/{LIBRARY NAME}/`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Widoki Razor, strony, kontrolery, modele stron, [składniki Razor](xref:blazor/class-libraries), [składniki widoku](xref:mvc/views/view-components)i modele danych mogą być wbudowane w bibliotekę klas Razor (RCL). RCL można spakować i ponownie użyć. Aplikacje mogą zawierać RCL i przesłonić widoki oraz strony, które zawiera. Gdy widok, częściowy widok lub strona Razor zostaną znalezione zarówno w aplikacji sieci Web, jak i w RCL, pierwszeństwo ma znacznik Razor (plik *. cshtml* ) w aplikacji sieci Web.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([jak pobrać](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Tworzenie biblioteki klas zawierającej interfejs użytkownika Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt**>.
* Wybierz **ASP.NET Core aplikacji sieci Web**.
* Nazwij bibliotekę (na przykład "RazorClassLib") > **OK**. Aby uniknąć kolizji nazw plików z wygenerowanej biblioteki widoków, upewnij się, że nazwa biblioteki nie kończy się `.Views`.
* Sprawdź, czy wybrano **ASP.NET Core 2,1** lub nowszą.
* Wybierz **bibliotekę klas Razor** > **OK**.

RCL ma następujący plik projektu:

[!code-xml[](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[interfejs wiersza polecenia platformy .NET Core](#tab/netcore-cli)

W wierszu polecenia Uruchom `dotnet new razorclasslib`. Na przykład:

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
```

Aby uzyskać więcej informacji, zobacz [dotnet New](/dotnet/core/tools/dotnet-new). Aby uniknąć kolizji nazw plików z wygenerowanej biblioteki widoków, upewnij się, że nazwa biblioteki nie kończy się `.Views`.

---

Dodaj pliki Razor do RCL.

Szablony ASP.NET Core założono, że zawartość RCL znajduje się w folderze *obszarów* . Zobacz [układ stron RCL](#rcl-pages-layout) , aby utworzyć RCL, który uwidacznia zawartość w `~/Pages`, a nie `~/Areas/Pages`.

## <a name="reference-rcl-content"></a>Odwołanie do zawartości RCL

RCL może odwoływać się do:

* Pakiet NuGet. Zobacz [Tworzenie pakietów NuGet](/nuget/create-packages/creating-a-package) i [dotnet Dodawanie pakietu](/dotnet/core/tools/dotnet-add-package) oraz [Tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{ProjectName}. csproj*. Zobacz sekcję [dotnet — Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a>Przewodnik: Tworzenie projektu RCL i korzystanie z projektu Razor Pages

Możesz pobrać [kompletny projekt](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) i przetestować go zamiast go tworzyć. Pobranie próbek zawiera dodatkowy kod i linki, które ułatwiają przetestowanie projektu. Możesz wystawić opinię na temat [tego problemu](https://github.com/aspnet/AspNetCore.Docs/issues/6098) w usłudze GitHub, korzystając z komentarzy dotyczących pobierania próbek, a także instrukcje krok po kroku.

### <a name="test-the-download-app"></a>Testowanie aplikacji do pobrania

Jeśli nie pobrano ukończonej aplikacji i wolisz utworzyć projekt przewodnika, przejdź do [następnej sekcji](#create-an-rcl).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Otwórz plik *sln* w programie Visual Studio. Uruchom aplikację.

# <a name="net-core-clitabnetcore-cli"></a>[interfejs wiersza polecenia platformy .NET Core](#tab/netcore-cli)

Z wiersza polecenia w katalogu *CLI* Utwórz aplikację RCL i sieć Web.

```dotnetcli
dotnet build
```

Przejdź do katalogu *WebApp1* i uruchom aplikację:

```dotnetcli
dotnet run
```

---

Postępuj zgodnie z instrukcjami w temacie [test WebApp1](#test-webapp1)

## <a name="create-an-rcl"></a>Utwórz RCL

W tej sekcji jest tworzony RCL. Pliki Razor są dodawane do RCL.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Utwórz projekt RCL:

* Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** **projekt**>.
* Wybierz **ASP.NET Core aplikacji sieci Web**.
* Nazwij aplikację **RazorUIClassLib** > **OK**.
* Sprawdź, czy wybrano **ASP.NET Core 2,1** lub nowszą.
* Wybierz **bibliotekę klas Razor** > **OK**.
* Dodaj plik widoku częściowego Razor o nazwie *RazorUIClassLib/Areas/webfeature/Pages/Shared/_Message. cshtml*.

# <a name="net-core-clitabnetcore-cli"></a>[interfejs wiersza polecenia platformy .NET Core](#tab/netcore-cli)

W wierszu polecenia Uruchom następujące polecenie:

```dotnetcli
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

Poprzednie polecenia:

* Tworzy `RazorUIClassLib` RCL.
* Tworzy stronę _Message Razor i dodaje ją do RCL. Parametr `-np` tworzy stronę bez `PageModel`.
* Tworzy plik [_ViewStart. cshtml](xref:mvc/views/layout#running-code-before-each-view) i dodaje go do RCL.

Plik *_ViewStart. cshtml* jest wymagany do korzystania z układu projektu Razor Pages (który jest dodawany w następnej sekcji).

---

### <a name="add-razor-files-and-folders-to-the-project"></a>Dodawanie plików i folderów Razor do projektu

* Zastąp znaczniki w *RazorUIClassLib/Areas/webfeature/Pages/Shared/_Message. cshtml* następującym kodem:

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Zastąp znaczniki w *RazorUIClassLib/Areas/webfeature/Pages/Strona1. cshtml* następującym kodem:

  [!code-cshtml[](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

  `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` jest wymagany do korzystania z widoku częściowego (`<partial name="_Message" />`). Zamiast uwzględniać dyrektywę `@addTagHelper`, można dodać plik *_ViewImports. cshtml* . Na przykład:

  ```dotnetcli
  dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
  ```

  Aby uzyskać więcej informacji na temat *_ViewImports. cshtml*, zobacz [Importowanie wspólnych dyrektyw](xref:mvc/views/layout#importing-shared-directives)

* Kompiluj bibliotekę klas, aby upewnić się, że nie występują błędy kompilatora:

  ```dotnetcli
  dotnet build RazorUIClassLib
  ```

Dane wyjściowe kompilacji zawierają *RazorUIClassLib. dll* i *RazorUIClassLib. views. dll*. *RazorUIClassLib. views. dll* zawiera skompilowaną zawartość Razor.

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Korzystanie z biblioteki interfejsu użytkownika Razor z projektu Razor Pages

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Utwórz aplikację sieci Web Razor Pages:

* W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy rozwiązanie > **Dodaj** >  **Nowy projekt**.
* Wybierz **ASP.NET Core aplikacji sieci Web**.
* Nadaj aplikacji nazwę **WebApp1**.
* Sprawdź, czy wybrano **ASP.NET Core 2,1** lub nowszą.
* Wybierz **aplikację sieci Web** > **OK**.

* W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję **WebApp1** i wybierz pozycję **Ustaw jako projekt startowy**.
* W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję **WebApp1** , a następnie wybierz pozycję **zależności kompilacji** > **zależności projektu**.
* Sprawdź **RazorUIClassLib** jako zależność **WebApp1**.
* W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję **WebApp1** i wybierz pozycję **Dodaj** **odwołanie**>.
* W oknie dialogowym **Menedżer odwołań** sprawdź **RazorUIClassLib** > **OK**.

Uruchom aplikację.

# <a name="net-core-clitabnetcore-cli"></a>[interfejs wiersza polecenia platformy .NET Core](#tab/netcore-cli)

Utwórz Razor Pages aplikację sieci Web i plik rozwiązania zawierający aplikację Razor Pages i RCL:

```dotnetcli
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

Kompiluj i uruchom aplikację sieci Web:

```dotnetcli
cd WebApp1
dotnet run
```

---

### <a name="test-webapp1"></a>Test WebApp1

Przejdź do `/MyFeature/Page1`, aby sprawdzić, czy biblioteka klas interfejsu użytkownika Razor jest używana.

## <a name="override-views-partial-views-and-pages"></a>Zastąp widoki, częściowe widoki i strony

Gdy widok, częściowy widok lub strona Razor zostaną znalezione zarówno w aplikacji sieci Web, jak i w RCL, pierwszeństwo ma znacznik Razor (plik *. cshtml* ) w aplikacji sieci Web. Na przykład Dodaj *WebApp1/Areas/webfeature/Pages/Strona1. cshtml* do WebApp1, a element Strona1 w WebApp1 będzie miał pierwszeństwo przed STRONA1 w RCL.

W przykładowym pobieraniu Zmień nazwę *WebApp1/obszarów/MyFeature2* na *WebApp1/Areas/webfeature* , aby przetestować priorytet.

Skopiuj widok części *RazorUIClassLib/Areas/Webfeature/Pages/Shared/_Message. cshtml* do obszaru WebApp1/refeatures/ *Shared/_Message. cshtml*. Zaktualizuj adiustację, aby wskazać nową lokalizację. Skompiluj i uruchom aplikację, aby sprawdzić, czy jest używana częściowa wersja aplikacji.

### <a name="rcl-pages-layout"></a>Układ stron RCL

Aby odwoływać się do zawartości RCL, tak jakby była częścią folderu *stron* aplikacji sieci Web, Utwórz projekt RCL o następującej strukturze plików:

* *RazorUIClassLib/strony*
* *RazorUIClassLib/strony/udostępnione*

Załóżmy, że *RazorUIClassLib/Pages/Shared* zawiera dwa częściowe pliki: *_Header. cshtml* i *_Footer. cshtml*. Tagi `<partial>` można dodać do pliku *_Layout. cshtml* :

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker-end
