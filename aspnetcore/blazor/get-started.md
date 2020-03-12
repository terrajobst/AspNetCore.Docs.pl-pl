---
title: Wprowadzenie do ASP.NET Core Blazor
author: guardrex
description: Rozpocznij pracę z Blazor, tworząc aplikację Blazor za pomocą wybranego przez siebie narzędzia.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/10/2020
no-loc:
- Blazor
- SignalR
uid: blazor/get-started
ms.openlocfilehash: 89c7529d2b8ec97db731f7c7268e19937c398115
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083238"
---
# <a name="get-started-with-aspnet-core-blazor"></a>Wprowadzenie do ASP.NET Core Blazor

Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Rozpocznij pracę z usługą Blazor:

1. Zainstaluj [zestaw SDK platformy .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).

1. Opcjonalnie zainstaluj szablon [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) :
   * Zainstaluj [zestaw SDK platformy .NET Core 3.1.102 lub nowszego (wersja zapoznawcza)](https://dotnet.microsoft.com/download/dotnet-core/3.1).
   * Uruchom następujące polecenie w powłoce poleceń. Pakiet [Microsoft. AspNetCore. Components. webassembly. Templates](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Templates/) ma wersję zapoznawczą, a Blazor webassembly jest w wersji zapoznawczej.

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Components.WebAssembly.Templates::3.2.0-preview2.20160.5
   ```

   > [!NOTE]
   > Aby można było użyć szablonu zestawu webassembly 3,2 w wersji 2 Blazor, **wymagany** jest zestaw .NET Core SDK wersja 3.1.102 lub nowsza. Potwierdź zainstalowaną zestaw .NET Core SDK wersję, uruchamiając `dotnet --version` w powłoce poleceń.

1. Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:

   # <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

   1\. Zainstaluj [program Visual Studio 2019 w wersji 16,4 lub nowszej](https://visualstudio.microsoft.com/vs/preview/) przy użyciu obciążeń **ASP.NET i Web Development** .

   2\. Utwórz nowy projekt.

   3 \. Wybierz pozycję **aplikacja Blazor**. Wybierz opcję **Dalej**.

   4\. Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu. Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu. Wybierz pozycję **Utwórz**.

   5 \. Aby zapoznać się z Blazor webassembly, wybierz szablon **aplikacji Blazor webassembly** . Dla środowiska serwera Blazor wybierz szablon **aplikacji Blazor Server** . Wybierz pozycję **Utwórz**. Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>. Jeśli szablon Blazor webassembly nie istnieje, Wróć do poprzedniego kroku i ponownie zainstaluj szablon.

   6 \. Naciśnij klawisz **Ctrl** ,+**F5** , aby uruchomić aplikację.

   > [!NOTE]
   > Jeśli zainstalowano rozszerzenie Blazor programu Visual Studio dla starszej wersji zapoznawczej programu ASP.NET Core Blazor (wersja zapoznawcza 6 lub wcześniejsza), można odinstalować rozszerzenie. Instalowanie szablonów Blazor w powłoce poleceń jest teraz wystarczające do poszycia szablonów w programie Visual Studio.

   # <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1\. Zainstaluj narzędzie [Visual Studio Code](https://code.visualstudio.com/).

   2\. Zainstaluj najnowsze [ C# rozszerzenie programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).

   3 \. W przypadku środowiska webassembly Blazor wykonaj następujące polecenie w powłoce poleceń:

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      W przypadku środowiska serwera Blazor wykonaj następujące polecenie w powłoce poleceń:

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.

   4\. Otwórz folder *WebApplication1* w Visual Studio Code.

   5 \. W przypadku projektu serwera Blazor, IDE żąda dodania zasobów do kompilowania i debugowania projektu. Wybierz pozycję **Tak**.

   6 \. W przypadku korzystania z aplikacji serwera Blazor należy uruchomić aplikację przy użyciu debugera Visual Studio Code. Jeśli używasz aplikacji Blazor webassembly, wykonaj `dotnet run` z folderu projektu aplikacji.

   7 \. W przeglądarce przejdź do `https://localhost:5001`.

   # <a name="visual-studio-for-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

   1\. Zainstaluj [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac/).

   2\. Wybierz pozycję **plik** > **nowe rozwiązanie** lub Utwórz **Nowy projekt**.

   3 \. Na pasku bocznym wybierz pozycję **aplikacja** **.NET Core** > .

   4\. Wybierz szablon **aplikacji Blazor Server** . W Visual Studio dla komputerów Mac jest dostępny tylko szablon serwera Blazor. W przypadku środowiska webassembly Blazor postępuj zgodnie z instrukcjami na karcie **interfejs wiersza polecenia platformy .NET Core** . Po wybraniu szablonu serwera Blazor wybierz pozycję **dalej**. Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   5 \. Ustaw platformę **docelową** na **platformę .NET Core 3,1** i wybierz pozycję **dalej**.

   6 \. W polu **Nazwa projektu** Nadaj nazwę aplikacji `WebApplication1`. Wybierz pozycję **Utwórz**.

   7 \. Wybierz pozycję **uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację *bez debugera*. Uruchom aplikację przy użyciu **Rozpocznij debugowanie** , aby uruchomić aplikację *za pomocą debugera*.

   Jeśli zostanie wyświetlony monit o zaufać certyfikatowi Deweloperskiemu, zaufaj certyfikatowi i Kontynuuj.

   # <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

   W przypadku środowiska webassembly Blazor wykonaj następujące polecenia w powłoce poleceń:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   W przypadku środowiska serwera Blazor należy wykonać następujące polecenia w powłoce poleceń:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.

   W przeglądarce przejdź do `https://localhost:5001`.

   ---

Na pasku bocznym są dostępne wiele stron:

* Home
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
