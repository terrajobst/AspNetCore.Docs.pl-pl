---
title: Wielokrotnego użytku UI Razor w bibliotekach klas z platformą ASP.NET Core
author: Rick-Anderson
description: Wyjaśnia, jak tworzyć wielokrotnego użytku Razor interfejsu użytkownika przy użyciu widoków częściowych w bibliotece klas, w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 08/20/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 468d961c291810ca4dfbe615acd972cfd6e7572a
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886397"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Utwórz interfejs użytkownika wielokrotnego użytku przy użyciu projektu biblioteki klas Razor w ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Widoki Razor, strony, kontrolery, modele stron, [składniki Razor](xref:blazor/class-libraries), [składniki widoku](xref:mvc/views/view-components)i modele danych mogą być wbudowane w bibliotekę klas Razor (RCL). RCL może spakowane i ponownie używane. Aplikacje mogą zawierać RCL oraz zastąpić widoków i stron, które zawiera. Gdy widoku, widoku częściowego lub strona Razor znajduje się w RCL, znaczników Razor i aplikacji sieci web ( *.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo.

Ta funkcja wymaga [!INCLUDE[](~/includes/2.1-SDK.md)]

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Tworzenie biblioteki klas zawierający Razor interfejsu użytkownika

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.
* Wybierz **aplikacji sieci Web platformy ASP.NET Core**.
* Nazwa biblioteki (na przykład "RazorClassLib") > **OK**. Aby uniknąć kolizji nazw plików, za pomocą biblioteki wygenerowany widok, upewnij się, nazwa biblioteki nie kończy się w `.Views`.
* Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.
* Wybierz **biblioteki klas Razor** > **OK**.

RCL ma następujący plik projektu:

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

W wierszu polecenia Uruchom polecenie `dotnet new razorclasslib`. Na przykład:

```console
dotnet new razorclasslib -o RazorUIClassLib
```

Aby uzyskać więcej informacji, zobacz [dotnet nowe](/dotnet/core/tools/dotnet-new). Aby uniknąć kolizji nazw plików, za pomocą biblioteki wygenerowany widok, upewnij się, nazwa biblioteki nie kończy się w `.Views`.

---

Dodaj pliki Razor do RCL.

Szablony ASP.NET Core przyjęto założenie, zawartość RCL znajduje się w *obszarów* folderu. Zobacz [układ stron RCL](#afs) , aby utworzyć RCL, który uwidacznia zawartość `~/Pages` zamiast. `~/Areas/Pages`

## <a name="referencing-rcl-content"></a>Odwołanie do zawartości RCL

RCL mogą być przywoływane przez:

* Pakiet NuGet. Zobacz [pakiety NuGet tworzenia](/nuget/create-packages/creating-a-package) i [dotnet Dodaj pakiet](/dotnet/core/tools/dotnet-add-package) i [tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{Nazwa_projektu} .csproj*. Zobacz [dotnet-Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a>Przewodnik: Tworzenie projektu RCL i korzystanie z projektu Razor Pages

Możesz pobrać [kompletnego projektu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) i przetestuj go, a nie jej tworzenia. Pobierz przykładowy zawiera dodatkowy kod i łącza, które ułatwiają Testowanie projektu. Możesz pozostawić opinię w [problem w usłudze GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6098) z komentarzami pobierania próbek i instrukcje krok po kroku.

### <a name="test-the-download-app"></a>Testowanie aplikacji pobierania

Jeśli nie zostały pobrane ukończonej aplikacji i raczej utworzyć projekt wskazówki, przejdź do [następnej sekcji](#create-an-rcl).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Otwórz *.sln* pliku w programie Visual Studio. Uruchom aplikację.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Z poziomu wiersza polecenia w *interfejsu wiersza polecenia* katalogu, tworzenie RCL i aplikacja sieci web.

```console
dotnet build
```

Przenieś do *WebApp1* katalogu i uruchom aplikację:

```console
dotnet run
```

---

Postępuj zgodnie z instrukcjami w [WebApp1 testu](#test)

## <a name="create-an-rcl"></a>Utwórz RCL

W tej sekcji jest tworzony RCL. Pliki razor są dodawane do RCL.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Utwórz projekt RCL:

* W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.
* Wybierz **aplikacji sieci Web platformy ASP.NET Core**.
* Określanie nazwy aplikacji **RazorUIClassLib** > **OK**.
* Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.
* Wybierz **biblioteki klas Razor** > **OK**.
* Dodaj plik do widoku częściowego Razor o nazwie *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

W wierszu polecenia Uruchom następujące polecenie:

```console
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

Poprzedniego polecenia:

* `RazorUIClassLib` Tworzy RCL.
* Tworzy stronę _Message Razor i dodaje go do RCL. `-np` Parametr tworzy tę stronę bez `PageModel`.
* Tworzy [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) plików i dodaje go do RCL.

*_ViewStart.cshtml* pliku jest wymagana do używania układ stron Razor projektu, (który został dodany w następnej sekcji).

---

### <a name="add-razor-files-and-folders-to-the-project"></a>Dodawanie Razor plików i folderów do projektu

* Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* następującym kodem:

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* następującym kodem:

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` wymagane jest wprowadzenie widoku częściowego (`<partial name="_Message" />`). Zamiast tym `@addTagHelper` dyrektywy, możesz dodać *_ViewImports.cshtml* pliku. Na przykład:

```console
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

Aby uzyskać więcej informacji na temat *_ViewImports.cshtml*, zobacz [importowania dyrektywy udostępnione](xref:mvc/views/layout#importing-shared-directives)

* Tworzenie biblioteki klas, aby sprawdzić, czy nie ma żadnych błędów kompilatora:

```console
dotnet build RazorUIClassLib
```

Zawiera dane wyjściowe kompilacji *RazorUIClassLib.dll* i *RazorUIClassLib.Views.dll*. *RazorUIClassLib.Views.dll* zawiera skompilowanej zawartości Razor.

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Korzystanie z biblioteki interfejsu użytkownika Razor z projektu stron Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Tworzenie aplikacji sieci web stron Razor:

* Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie > **Dodaj** >  **nowy projekt**.
* Wybierz **aplikacji sieci Web platformy ASP.NET Core**.
* Określanie nazwy aplikacji **WebApp1**.
* Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.
* Wybierz **aplikacji sieci Web** > **OK**.

* Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **Ustaw jako projekt startowy**.
* Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **zależności kompilacji** > **zależności projektu**.
* Sprawdź **RazorUIClassLib** jako zależność z **WebApp1**.
* Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **Dodaj** > **odwołania**.
* W **Menadżer odwołań** okno dialogowe, sprawdź **RazorUIClassLib** > **OK**.

Uruchom aplikację.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Utwórz Razor Pages aplikację sieci Web i plik rozwiązania zawierający aplikację Razor Pages i RCL:

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

Kompilowanie i uruchamianie aplikacji sieci web:

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a>WebApp1 testu

Sprawdź, czy biblioteka klas interfejsu użytkownika Razor jest używana:

* Przejdź do `/MyFeature/Page1`.

## <a name="override-views-partial-views-and-pages"></a>Zastąp widoki, widoki częściowe i strony

Gdy widoku, widoku częściowego lub strona Razor znajduje się w RCL, znaczników Razor i aplikacji sieci web ( *.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo. Na przykład Dodaj *WebApp1/Areas/webfeature/Pages/Strona1. cshtml* do WebApp1, a element Strona1 w WebApp1 będzie miał pierwszeństwo przed STRONA1 w RCL.

Do pobrania próbki, Zmień nazwę *WebApp1/obszarów/MyFeature2* do *WebApp1/obszarów/MyFeature* do testowania pierwszeństwo.

Kopiuj *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* widoku częściowego do *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Zaktualizuj znaczników, aby wskazać nowej lokalizacji. Skompiluj i uruchom aplikację, aby sprawdzić, czy jest używana wersja aplikacji częściowego.

<a name="afs"></a>

### <a name="rcl-pages-layout"></a>Układ stron RCL

Odwołanie RCL zawartości, jakby była częścią aplikacji sieci web *stron* folderu, Utwórz projekt RCL o następującej strukturze pliku:

* *RazorUIClassLib/stron*
* *RazorUIClassLib/stron/udostępnione*

Załóżmy, że *RazorUIClassLib/stron/Shared* zawiera dwa pliki częściowa: *_Header.cshtml* i *_Footer.cshtml*. `<partial>` Tagów może zostać dodany do *_Layout.cshtml* pliku:

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker range=">= aspnetcore-3.0"

## <a name="create-an-rcl-with-static-assets"></a>Tworzenie RCL przy użyciu statycznych zasobów

RCL może wymagać pomocnika zasobów statycznych, do których może odwoływać się aplikacja zużywana przez RCL. ASP.NET Core umożliwia tworzenie RCLs zawierających statyczne zasoby, które są dostępne dla aplikacji zużywanej.

Aby dołączyć zasoby towarzyszące jako część RCL, Utwórz folder *wwwroot* w bibliotece klas i Dołącz wszystkie wymagane pliki w tym folderze.

Podczas pakowania RCL wszystkie zasoby towarzyszące w folderze *wwwroot* zostaną automatycznie dołączone do pakietu.

### <a name="consume-content-from-a-referenced-rcl"></a>Korzystanie z zawartości z RCL, do którego istnieje odwołanie

Pliki znajdujące się w folderze *WWWROOT* RCL są uwidocznione dla aplikacji zużywanej pod prefiksem `_content/{LIBRARY NAME}/`. Na przykład biblioteka o nazwie *Razor. Class. lib* skutkuje ścieżką do zawartości statycznej pod adresem `_content/Razor.Class.Lib/`.

Aplikacja, która zużywa odwołania `<script>` `<img>`, odwołuje się do statycznych zasobów udostępnianych przez bibliotekę z, `<style>`, i innymi tagami HTML. Aplikacja zużywana musi mieć włączoną [obsługę pliku statycznego](xref:fundamentals/static-files) w `Startup.Configure`programie:

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseStaticFiles();

    ...
}
```

W przypadku uruchamiania aplikacji zużywającej dane wyjściowe kompilacji (`dotnet run`) statyczne zasoby sieci Web są domyślnie włączone w środowisku programistycznym. Aby zapewnić obsługę zasobów w innych środowiskach podczas uruchamiania z danych wyjściowych `UseStaticWebAssets` kompilacji, należy wywołać konstruktora hosta w *program.cs*:

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

Wywołanie `UseStaticWebAssets` nie jest wymagane w przypadku uruchamiania aplikacji z opublikowanych danych`dotnet publish`wyjściowych ().

### <a name="multi-project-development-flow"></a>Przepływ programistyczny dla wieloprojektowych

Po uruchomieniu aplikacji zużywanej przez aplikację:

* Zasoby w RCL pozostają w swoich oryginalnych folderach. Elementy zawartości nie są przenoszone do aplikacji zużywanej.
* Wszelkie zmiany w folderze *WWWROOT* RCL są odzwierciedlone w aplikacji zużywanej po odtworzeniu RCL i niepomyślnym skompilowaniu aplikacji.

Po skompilowaniu RCL jest tworzony manifest, który opisuje lokalizacje statycznego elementu zawartości sieci Web. Aplikacja, która korzysta z aplikacji, odczytuje manifest w czasie wykonywania, aby wykorzystać zasoby z przywoływanych projektów i pakietów. Po dodaniu nowego elementu zawartości do RCL należy ponownie skompilować RCL, aby zaktualizować jego manifest, zanim aplikacja zużywa dostęp do nowego elementu zawartości.

### <a name="publish"></a>Publikowanie

Po opublikowaniu aplikacji składniki towarzyszące ze wszystkich przywoływanych projektów i pakietów są kopiowane do folderu *wwwroot* opublikowanej aplikacji w obszarze `_content/{LIBRARY NAME}/`.

::: moniker-end
