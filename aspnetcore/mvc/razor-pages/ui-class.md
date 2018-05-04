---
title: Wielokrotnego użytku UI Razor w bibliotekach klas podstawowych ASP.NET
author: Rick-Anderson
description: Wyjaśnia sposób tworzenia wielokrotnego użytku Razor interfejsu użytkownika w bibliotece klas.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: 731d37a8f4983b18ded114f05470f8a408deb7cd
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Utwórz wielokrotnego użytku interfejsu użytkownika przy użyciu projektu biblioteki klas Razor w ASP.NET Core.

przez [Rick Anderson](https://twitter.com/RickAndMSFT)

Widokami razor, strony, kontrolerów, strona modeli i modeli danych mogą być wbudowane w Library(RCL) klasy Razor. RCL może być spakowane i użyć ponownie. Aplikacje można obejmują RCL i zastąpić widoków i stron, które zawiera. Gdy widok, widok częściowy lub Razor strony nie został znaleziony w aplikacji sieci web i RCL, znaczników Razor (*.cshtml* pliku) w sieci web, pierwszeństwo ma aplikacja.

Ta funkcja wymaga [!INCLUDE[](~/includes/2.1-SDK.md)]

[!INCLUDE[](~/includes/2.1.md)]

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Utwórz bibliotekę klasy zawierające Razor interfejsu użytkownika

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.
* Wybierz **aplikacji sieci Web platformy ASP.NET Core**.
* Sprawdź **platformy ASP.NET Core 2.1** lub nowszego jest zaznaczone.
* Wybierz **biblioteki klas Razor** > **OK**.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

W wierszu polecenia, uruchom `dotnet new razorclasslib`. Na przykład:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

Aby uzyskać więcej informacji, zobacz [dotnet nowe](/dotnet/core/tools/dotnet-new).

------
Dodaj pliki Razor do RCL.

Firma Microsoft zaleca RCL zawartości Przejdź w *obszarów* folderu. 


## <a name="referencing-razor-class-library-content"></a>Odwołanie do biblioteki klas Razor zawartości

RCL może odwoływać się:

* Pakiet NuGet. Zobacz [pakietów NuGet tworzenie](/nuget/create-packages/creating-a-package) i [dotnet Dodaj pakiet](/dotnet/core/tools/dotnet-add-package) i [tworzenie i publikowanie pakietu NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{Nazwa_projektu} .csproj*. Zobacz [dotnet — Dodaj odwołanie](/dotnet/core/tools/dotnet-add-reference).

### <a name="partial-files-access-in-the-rcl"></a>Dostęp do plików z częściowa w RCL

Zawartości poza RCL środowisko uruchomieniowe platformy ASP.NET Core nie wyszukiwania plików częściowe w RCL.

Na przykład w pobieranie próbki *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* można widoku częściowego **nie** odwoływać się do *WebApp1\Pages\About.cshtml* . Jednak stron w RCL ( *RazorUIClassLib /* **można** dostępu *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a>Wskazówki: Tworzenie projektu biblioteki klas Razor i korzystać z projektu stron Razor

Możesz pobrać [kompletnego projektu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) i przetestować go zamiast go utworzyć. Pobieranie próbki zawiera dodatkowy kod i linki, które umożliwiają łatwe testowanie projektu. Możesz pozostawić opinii w [ten problem GitHub](https://github.com/aspnet/Docs/issues/6098) z komentarzami na pobieranie próbek i instrukcje krok po kroku.

### <a name="test-the-download-app"></a>Testowanie aplikacji pobierania

Jeśli nie zostały pobrane ukończonej aplikacji, a raczej spowodowałoby utworzenie projektu wskazówki, przejdź do [następnej sekcji](#create-a-razor-class-library).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Otwórz *.sln* pliku w programie Visual Studio. Uruchom aplikację.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Z wiersza polecenia w *cli* katalogu kompilacji RCL i sieci web aplikacji.

``` CLI
dotnet build
```

Przenieś do *WebApp1* katalogu i uruchom aplikację:

``` CLI
dotnet run
```
------

Postępuj zgodnie z instrukcjami [WebApp1 testu](#test)

## <a name="create-a-razor-class-library"></a>Utwórz bibliotekę klas Razor

W tej sekcji jest tworzony biblioteki klasy Razor (RCL). Pliki razor zostaną dodane do RCL.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Utwórz projekt RCL:

* W programie Visual Studio **pliku** menu, wybierz opcję **nowy** > **projektu**.
* Wybierz **aplikacji sieci Web platformy ASP.NET Core**.
* Nazwa aplikacji **RazorUIClassLib**.
* Sprawdź **platformy ASP.NET Core 2.1** lub nowszego jest zaznaczone.
* Wybierz **biblioteki klas Razor** > **OK**.

Tworzenie aplikacji sieci web Razor strony:

* Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy rozwiązanie > **Dodaj** >  **nowy projekt**.
* Wybierz **aplikacji sieci Web platformy ASP.NET Core**.
* Nazwa aplikacji **WebApp1**.
* Sprawdź **platformy ASP.NET Core 2.1** lub nowszego jest zaznaczone.
* Wybierz **aplikacji sieci Web** > **OK**.

### <a name="add-razor-files-and-folders-to-the-project"></a>Dodaj Razor pliki i foldery do projektu.

* Dodaj widok częściowy Razor plik o nazwie *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.
* Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* następującym kodem:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Kopiuj *_ViewStart.cshtml* plik z projektu WebApp1 *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.

  [Viewstart](xref:mvc/views/layout#running-code-before-each-view) plik jest wymagany do użycia układ projektu stron Razor.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

W wierszu polecenia Uruchom następujące polecenie:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

Poprzednie polecenia:

* Tworzy `RazorUIClassLib` Razor biblioteki klas (RCL).
* Tworzy stronę _Message Razor i dodaje go do RCL. `-np` Parametr tworzy tę stronę bez `PageModel`.
* Tworzy [viewstart](xref:mvc/views/layout#running-code-before-each-view) plików i dodaje go do RCL.

Plik viewstart jest wymagany do użycia układ projektu stron Razor (który został dodany w następnej sekcji).

Aktualizacja stron Razor:

* Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* następującym kodem:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Zastąp kod znaczników w *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* następującym kodem:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` Musisz wybrać widok częściowy (`<partial name="_Message" />`). Zamiast tym `@addTagHelper` dyrektywy, możesz dodać *_ViewImports.cshtml* pliku. Na przykład:

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

Aby uzyskać więcej informacji o viewimports, zobacz [importowanie dyrektywy udostępnionych](xref:mvc/views/layout#importing-shared-directives)

* Tworzenie biblioteki klas, aby sprawdzić, czy nie ma żadnych błędów kompilatora:

``` CLI
dotnet build RazorUIClassLib
```

Zawiera dane wyjściowe kompilacji *RazorUIClassLib.dll* i *RazorUIClassLib.Views.dll*. *RazorUIClassLib.Views.dll* skompilowanych zawartość Razor.

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Korzystanie z biblioteki interfejsu użytkownika Razor z projektu stron Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **Ustaw jako projekt startowy**.
* Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **zależności kompilacji** > **zależności projektu**.
* Sprawdź **RazorUIClassLib** jako zależność z **WebApp1**.
* Z **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **WebApp1** i wybierz **Dodaj** > **odwołania**.
* W **Menedżera odwołań** dialogowym wyboru **RazorUIClassLib** > **OK**.

Uruchom aplikację.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Tworzenie aplikacji sieci web Razor strony i plik rozwiązania zawierający stron Razor aplikacji i biblioteki klas Razor:

``` CLI
dotnet new razor -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

Tworzenie i uruchamianie aplikacji sieci web:

``` CLI
cd WebApp1
dotnet run
```

------

<a name="test"></a>

### <a name="test-webapp1"></a>WebApp1 testu

Sprawdź, czy jest używana biblioteka klas Razor interfejsu użytkownika.

* Przejdź do `/MyFeature/Page1`.

## <a name="override-views-partial-views-and-pages"></a>Zastąpienie widoki, widoki częściowe i strony

Gdy widoku, widok częściowy lub Razor strony znajduje się zarówno w aplikacji sieci web, jak i w bibliotece klas programu Razor, znaczników Razor (*.cshtml* pliku) w sieci web, pierwszeństwo ma aplikacja. Na przykład dodać *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* do WebApp1, a strona 1 w WebApp1 mają wyższy priorytet niż Page1in biblioteki klas Razor.

Do pobrania na stronie próbki, należy zmienić *WebApp1/obszarów/MyFeature2* do *MyFeature-WebApp1/obszarów* do testowania pierwszeństwo.

Kopiuj *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* widoku częściowego do *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Zaktualizuj kod znaczników, aby wskazać, do nowej lokalizacji. Skompiluj i uruchom aplikację, aby sprawdzić, czy jest używana wersja aplikacji częściowego.
