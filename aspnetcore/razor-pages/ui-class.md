---
title: Wielokrotnego użytku UI Razor w bibliotekach klas z platformą ASP.NET Core
author: Rick-Anderson
description: Wyjaśnia, jak tworzyć wielokrotnego użytku Razor interfejsu użytkownika przy użyciu widoków częściowych w bibliotece klas, w programie ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 06/24/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: 96ef8fc055a6b92cd0808d02031d917b8446f305
ms.sourcegitcommit: 763af2cbdab0da62d1f1cfef4bcf787f251dfb5c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394745"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Tworzenie interfejsu użytkownika do wielokrotnego użytku, przy użyciu projektu biblioteki klas Razor w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Widokami razor, strony, kontrolerów, modele strony [składniki Razor](xref:blazor/class-libraries), [wyświetlanie składników](xref:mvc/views/view-components), i modeli danych może zostać wbudowana w bibliotekę klas Razor (RCL). RCL może spakowane i ponownie używane. Aplikacje mogą zawierać RCL oraz zastąpić widoków i stron, które zawiera. Gdy widoku, widoku częściowego lub strona Razor znajduje się w RCL, znaczników Razor i aplikacji sieci web ( *.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo.

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

Szablony ASP.NET Core przyjęto założenie, zawartość RCL znajduje się w *obszarów* folderu. Zobacz [układ stron RCL](#afs) utworzyć RCL, który udostępnia zawartość `~/Pages` zamiast `~/Areas/Pages`.

## <a name="referencing-rcl-content"></a>Odwoływanie się do zawartości RCL

RCL mogą być przywoływane przez:

* Pakiet NuGet. Zobacz [pakiety NuGet tworzenia](/nuget/create-packages/creating-a-package) i [dotnet Dodaj pakiet](/dotnet/core/tools/dotnet-add-package) i [tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{Nazwa_projektu} .csproj*. Zobacz [dotnet-Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a>Przewodnik: Utwórz projekt RCL i korzystać z projektu stron Razor

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

W tej sekcji RCL jest tworzony. Pliki razor są dodawane do RCL.

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

* Tworzy `RazorUIClassLib` RCL.
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

Tworzenie, aplikacja internetowa ze stronami Razor i plikiem rozwiązania zawierającego aplikację stron Razor i RCL:

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

Sprawdź, czy biblioteki klas Razor interfejsu użytkownika jest w użyciu:

* Przejdź do `/MyFeature/Page1`.

## <a name="override-views-partial-views-and-pages"></a>Zastąp widoki, widoki częściowe i strony

Gdy widoku, widoku częściowego lub strona Razor znajduje się w RCL, znaczników Razor i aplikacji sieci web ( *.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo. Na przykład dodać *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* do WebApp1, a strona 1 w WebApp1 mają wyższy priorytet niż strona 1 w RCL.

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

## <a name="create-an-rcl-with-static-assets"></a>Tworzenie RCL przy użyciu statycznych zasobów

RCL może wymagać pomocnika statycznych zasobów, które mogą być przywoływane przez aplikację odbierającą RCL. Platforma ASP.NET Core umożliwia tworzenie RCLs, które obejmują zasoby statyczne, które są dostępne dla aplikacji.

Aby dołączyć zasoby pomocnika jako część RCL, utworzyć *wwwroot* folder w ułatwieniach i zawierać wszystkie wymagane pliki w tym folderze.

Podczas pakowania RCL, wszystkie pomocnika zasobów w *wwwroot* folderów są automatycznie uwzględnione w pakiecie i są dostępne dla aplikacji, które odwołuje się do pakietu.

### <a name="consume-content-from-a-referenced-rcl"></a>Używać zawartości od RCL odwołania

Pliki zawarte w *wwwroot* folderu RCL są widoczne dla aplikacji w ramach prefiksu `_content/{LIBRARY NAME}/`. `{LIBRARY NAME}` Nazwa projektu biblioteki jest konwertowany na małe litery z kropek (`.`) usunięte. Na przykład, biblioteka o nazwie *Razor.Class.Lib* wyników w ścieżce do zawartości statycznej w `_content/razorclasslib/`.

Aplikacja odbierająca komunikaty odwołuje się do statycznych zasobów dostarczone przez bibliotekę z `<script>`, `<style>`, `<img>`, a inne tagi HTML. Aplikacja odbierająca komunikaty musi mieć [obsługi plików statycznych](xref:fundamentals/static-files) włączone.

### <a name="multi-project-development-flow"></a>Przepływ rozwoju wielu projektów

Podczas wykonywania aplikacji:

* Zasoby w pobytu RCL w ich oryginalnych folderów. Zasoby nie są przenoszone do aplikacji.
* Wszelkie zmiany w ramach RCL *wwwroot* folderu znajduje odzwierciedlenie w aplikacji po odbudowaniu RCL i bez odbudowywania odbierającą aplikację.

Podczas kompilowania RCL manifest jest generowany, opisujący lokalizacje zasobów statyczną sieci web. Aplikacja odbierająca komunikaty odczytuje manifest w czasie wykonywania, korzystanie z zasobów w projektach odwołania i pakietów. Po dodaniu nowego elementu zawartości RCL RCL musi zostać zrekompilowany, aby zaktualizować manifest, zanim aplikacja odbierająca komunikaty mogą uzyskiwać dostęp do nowego elementu zawartości.

### <a name="publish"></a>Publikowanie

Po opublikowaniu aplikacji zasoby pomocnika z wszystkie przywoływane projekty i pakiety są kopiowane do *wwwroot* folderu opublikowanej aplikacji, w obszarze `_content/{LIBRARY NAME}/`.
