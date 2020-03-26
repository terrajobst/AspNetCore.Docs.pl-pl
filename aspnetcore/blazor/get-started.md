---
title: Wprowadzenie do ASP.NET Core Blazor
author: guardrex
description: Rozpocznij pracę z Blazor, tworząc aplikację Blazor za pomocą wybranego przez siebie narzędzia.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: abecb640930c1e5770c0fad45a1e9a6df31a20f4
ms.sourcegitcommit: 6ffb583991d6689326605a24565130083a28ef85
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/26/2020
ms.locfileid: "80306458"
---
# <a name="get-started-with-aspnet-core-blazor"></a>Wprowadzenie do ASP.NET Core Blazor

Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Aby rozpocząć pracę z usługą Blazor, postępuj zgodnie ze wskazówkami dotyczącymi wybranych narzędzi:

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Aby utworzyć aplikacje serwera Blazor, zainstaluj [program Visual Studio 2019 w wersji 16,4 lub nowszej](https://visualstudio.microsoft.com/vs/preview/) przy użyciu obciążenia **ASP.NET i sieci Web** .

   Aby utworzyć aplikacje Blazor Server i Blazor webassembly, zainstaluj program Visual Studio 2019 16,6 Preview 2 lub nowszy przy użyciu obciążeń programu **ASP.NET i sieci Web** .

   Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor webassembly* i *Blazor Server*, zobacz <xref:blazor/hosting-models>.

1. Tworzenie nowego projektu.

1. Wybierz pozycję **aplikacja Blazor**. Wybierz opcję **Dalej**.

1. Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu. Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu. Wybierz pozycję **Utwórz**.

1. W przypadku środowiska webassembly Blazor (Visual Studio 16,6 Preview 2 lub nowszego) wybierz szablon **aplikacji Blazor webassembly** . W przypadku środowiska serwera Blazor (Visual Studio 16,4 lub nowszego) wybierz szablon **aplikacji Blazor Server** . Wybierz pozycję **Utwórz**.

1. Naciśnij klawisz <kbd>Ctrl</kbd> ,+<kbd>F5</kbd> , aby uruchomić aplikację.

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Zainstaluj [zestaw SDK platformy .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).

1. Opcjonalnie można zainstalować szablon [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) Preview, uruchamiając następujące polecenie:

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > [Zestaw .NET Core SDK w wersji 3.1.201 lub nowszej](https://dotnet.microsoft.com/download/dotnet-core/3.1) jest **wymagana** , aby można było użyć szablonu zestawu webassembly 3,2 (wersja zapoznawcza 3 Blazor). Potwierdź zainstalowaną zestaw .NET Core SDK wersję, uruchamiając `dotnet --version` w powłoce poleceń.

1. Zainstaluj narzędzie [Visual Studio Code](https://code.visualstudio.com/).

1. Zainstaluj najnowsze [ C# rozszerzenie programu dla Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) i rozszerzenie [JavaScript Debugger (nocne)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) z `debug.javascript.usePreview` ustawionym na `true`.

1. W przypadku środowiska serwera Blazor wykonaj następujące polecenie w powłoce poleceń:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   ```

   W przypadku środowiska webassembly Blazor wykonaj następujące polecenie w powłoce poleceń:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   ```

   Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.

1. Otwórz folder *WebApplication1* w Visual Studio Code.

1. Żądania IDE służące do dodawania zasobów do kompilowania i debugowania projektu. Wybierz pozycję **Tak**.

1. TUN aplikację przy użyciu debugera Visual Studio Code.

1. W przeglądarce przejdź do `https://localhost:5001`.

# <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

Serwer Blazor jest obsługiwany w Visual Studio dla komputerów Mac. Zestaw webassembly Blazor nie jest obecnie obsługiwany. Aby kompilować Blazor aplikacje webassembly na macOS, postępuj zgodnie ze wskazówkami na karcie **interfejs wiersza polecenia platformy .NET Core** .

1. Zainstaluj [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac/).

1. Wybierz pozycję **plik** > **nowe rozwiązanie** lub Utwórz **Nowy projekt**.

1. Na pasku bocznym wybierz pozycję **aplikacja** **.NET Core** > .

1. Wybierz szablon **aplikacji Blazor Server** . Wybierz pozycję **Utwórz**.

   Aby uzyskać informacje na temat modelu hostingu serwera Blazor, zobacz <xref:blazor/hosting-models>.

1. Ustaw platformę **docelową** na **platformę .NET Core 3,1** i wybierz pozycję **dalej**.

1. W polu **Nazwa projektu** Nadaj nazwę aplikacji `WebApplication1`. Wybierz pozycję **Utwórz**.

1. Wybierz pozycję **uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację *bez debugera*. Uruchom aplikację przy użyciu **Rozpocznij debugowanie** , aby uruchomić aplikację *za pomocą debugera*.

Jeśli zostanie wyświetlony monit o zaufać certyfikatowi Deweloperskiemu, zaufaj certyfikatowi i Kontynuuj.

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

1. Zainstaluj [zestaw SDK platformy .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).

1. Opcjonalnie można zainstalować szablon [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) Preview, uruchamiając następujące polecenie:

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview3.20168.3
   ```

   > [!NOTE]
   > [Zestaw .NET Core SDK w wersji 3.1.201 lub nowszej](https://dotnet.microsoft.com/download/dotnet-core/3.1) jest **wymagana** , aby można było użyć szablonu zestawu webassembly 3,2 (wersja zapoznawcza 3 Blazor). Potwierdź zainstalowaną zestaw .NET Core SDK wersję, uruchamiając `dotnet --version` w powłoce poleceń.

1. W przypadku środowiska serwera Blazor należy wykonać następujące polecenia w powłoce poleceń:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   W przypadku środowiska webassembly Blazor wykonaj następujące polecenia w powłoce poleceń:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.

1. W przeglądarce przejdź do `https://localhost:5001`.

---

Na pasku bocznym są dostępne wiele stron:

* Domowy
* Licznik
* Pobieranie danych

Na stronie licznik wybierz przycisk **kliknij** , aby zwiększyć licznik bez odświeżania strony. Zwiększenie licznika na stronie sieci Web zwykle wymaga pisania kodu JavaScript, ale z Blazor można użyć C#.

*Pages/Counter. Razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

Żądanie `/counter` w przeglądarce, zgodnie z dyrektywą `@page` w górnej części, powoduje, że składnik `Counter` renderuje jego zawartość. Składniki są renderowane w postaci reprezentacji drzewa renderowania, która może być następnie używana do aktualizowania interfejsu użytkownika w elastyczny i wydajny sposób.

Za każdym razem, gdy zostanie wybrany przycisk **kliknij mnie** :

* Zdarzenie `onclick` jest wyzwalane.
* Metoda `IncrementCount` jest wywoływana.
* `currentCount` jest zwiększana.
* Składnik jest ponownie renderowany.

Środowisko uruchomieniowe porównuje nową zawartość z poprzednią zawartością i stosuje tylko zmienioną zawartość do Document Object Model (DOM).

Dodaj składnik do innego składnika przy użyciu składni języka HTML. Na przykład Dodaj składnik `Counter` do strony głównej aplikacji przez dodanie elementu `<Counter />` do składnika `Index`.

*Pages/index. Razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Uruchom aplikację. Strona główna ma swój własny licznik dostarczony przez składnik `Counter`.

Parametry składnika są określone przy użyciu atrybutów lub [zawartości podrzędnej](xref:blazor/components#child-content), które umożliwiają ustawianie właściwości składnika podrzędnego. Aby dodać parametr do składnika `Counter`, zaktualizuj blok `@code` składnika:

* Dodaj właściwość publiczną dla `IncrementAmount` z atrybutem `[Parameter]`.
* Zmień metodę `IncrementCount`, aby użyć `IncrementAmount` podczas zwiększania wartości `currentCount`.

*Pages/Counter. Razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

Określ `IncrementAmount` w elemencie `<Counter>` składnika `Index` przy użyciu atrybutu.

*Pages/index. Razor*:

[!code-razor[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Uruchom aplikację. Składnik `Index` ma swój własny licznik, który zwiększa się o dziesięć za każdym razem, gdy jest zaznaczony przycisk **kliknij mnie** . Składnik `Counter` (*Counter. Razor*) w `/counter` nadal zwiększa się o jeden.

## <a name="next-steps"></a>Następne kroki

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:blazor/templates>
* <xref:signalr/introduction>
