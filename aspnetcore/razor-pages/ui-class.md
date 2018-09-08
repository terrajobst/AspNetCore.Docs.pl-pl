---
title: Wielokrotnego użytku UI Razor w bibliotekach klas z platformą ASP.NET Core
author: Rick-Anderson
description: Wyjaśnia sposób tworzenia wielokrotnego użytku Razor interfejsu użytkownika w bibliotece klas.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/07/2018
uid: razor-pages/ui-class
ms.openlocfilehash: 7e9ab07a9060b16c09afb1e88950f6a3e55b13cb
ms.sourcegitcommit: 8268cc67beb1bb1ca470abb0e28b15a7a71b8204
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44126751"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Tworzenie interfejsu użytkownika wielokrotnego użytku, używając projektu biblioteki klas Razor w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Widokami razor, strony, kontrolerów, modele strony [wyświetlanie składników](xref:mvc/views/view-components), a modeli danych może być kompilowany do biblioteki klas Razor (RCL). RCL może spakowane i ponownie używane. Aplikacje mogą zawierać RCL oraz zastąpić widoków i stron, które zawiera. Gdy widoku, widoku częściowego lub strona Razor znajduje się w RCL, znaczników Razor i aplikacji sieci web (*.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo.

Ta funkcja wymaga [!INCLUDE[](~/includes/2.1-SDK.md)]

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Tworzenie biblioteki klas zawierający Razor interfejsu użytkownika

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.
* Wybierz **aplikacji sieci Web platformy ASP.NET Core**.
* Nazwa biblioteki (na przykład "RazorClassLib") > **OK**. Aby uniknąć kolizji nazw plików, za pomocą biblioteki wygenerowany widok, upewnij się, nazwa biblioteki nie kończy się w `.Views`.
* Sprawdź **platformy ASP.NET Core 2.1** lub nowszej jest zaznaczone.
* Wybierz **biblioteki klas Razor** > **OK**.

Biblioteki klas Razor ma następujący plik projektu:

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

W wierszu polecenia Uruchom polecenie `dotnet new razorclasslib`. Na przykład:

```console
dotnet new razorclasslib -o RazorUIClassLib
```

Aby uzyskać więcej informacji, zobacz [dotnet nowe](/dotnet/core/tools/dotnet-new). Aby uniknąć kolizji nazw plików, za pomocą biblioteki wygenerowany widok, upewnij się, nazwa biblioteki nie kończy się w `.Views`.

------
Dodaj pliki Razor do RCL.

Szablony ASP.NET Core przyjęto założenie, zawartość RCL znajduje się w *obszarów* folderu. Zobacz [układ stron RCL](#afs) utworzyć RCL, który udostępnia zawartość `~/Pages` zamiast `~/Areas/Pages`.

## <a name="referencing-razor-class-library-content"></a>Odwoływanie się do zawartości biblioteki klas Razor

RCL mogą być przywoływane przez:

* Pakiet NuGet. Zobacz [pakiety NuGet tworzenia](/nuget/create-packages/creating-a-package) i [dotnet Dodaj pakiet](/dotnet/core/tools/dotnet-add-package) i [tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{Nazwa_projektu} .csproj*. Zobacz [dotnet-Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a>Przewodnik: Tworzenie projektu biblioteki klas Razor i korzystać z projektu stron Razor

Możesz pobrać [kompletnego projektu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) i przetestuj go, a nie jej tworzenia. Pobierz przykładowy zawiera dodatkowy kod i łącza, które ułatwiają Testowanie projektu. Możesz pozostawić opinię w [problem w usłudze GitHub](https://github.com/aspnet/Docs/issues/6098) z komentarzami pobierania próbek i instrukcje krok po kroku.

### <a name="test-the-download-app"></a>Testowanie aplikacji pobierania

Jeśli nie zostały pobrane ukończonej aplikacji i raczej utworzyć projekt wskazówki, przejdź do [następnej sekcji](#create-a-razor-class-library).

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

------

Postępuj zgodnie z instrukcjami w [WebApp1 testu](#test)

## <a name="create-a-razor-class-library"></a>Tworzenie biblioteki klas Razor

W tej sekcji jest tworzony biblioteki klas Razor (RCL). Pliki razor są dodawane do RCL.

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

* Tworzy `RazorUIClassLib` biblioteki klas Razor (RCL).
* Tworzy stronę _Message Razor i dodaje go do RCL. `-np` Parametr tworzy tę stronę bez `PageModel`.
* Tworzy [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) plików i dodaje go do RCL.

*_ViewStart.cshtml* pliku jest wymagana do używania układ stron Razor projektu, (który został dodany w następnej sekcji).

------

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

Tworzenie, aplikacja internetowa ze stronami Razor i plikiem rozwiązania zawierającego aplikację stron Razor i biblioteki klas Razor:

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

Kompilowanie i uruchamianie aplikacji sieci web:

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a>WebApp1 testu

Sprawdź, czy jest on używany biblioteki klas Razor interfejsu użytkownika.

* Przejdź do `/MyFeature/Page1`.

## <a name="override-views-partial-views-and-pages"></a>Zastąp widoki, widoki częściowe i strony

Gdy widoku, widoku częściowego lub strona Razor znajduje się w aplikacji sieci web i biblioteki klas Razor znaczników Razor (*.cshtml* pliku) w sieci web aplikacji ma pierwszeństwo. Na przykład dodać *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* do WebApp1, a strona 1 w WebApp1 mają wyższy priorytet niż strona 1 w bibliotece klas Razor.

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
