---
title: Wprowadzenie do ASP.NET Core Blazor
author: guardrex
description: Rozpocznij pracę z Blazor, tworząc aplikację Blazor za pomocą wybranego przez siebie narzędzia.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/09/2019
no-loc:
- Blazor
uid: blazor/get-started
ms.openlocfilehash: 554f4daff92a0839ee7679287a4618e9b51e0fe5
ms.sourcegitcommit: 925cdbd94613243f33bc7613a62ea34006219931
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/13/2020
ms.locfileid: "75921306"
---
# <a name="get-started-with-aspnet-core-opno-locblazor"></a>Wprowadzenie do ASP.NET Core Blazor

Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Rozpocznij pracę z Blazor:

::: moniker range=">= aspnetcore-3.1"

1. Zainstaluj [zestaw SDK platformy .NET Core 3,1](https://dotnet.microsoft.com/download/dotnet-core/3.1).

1. Opcjonalnie zainstaluj szablon [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) :
   * Zainstaluj [zestaw SDK platformy .NET Core 3,1 lub nowszego (wersja zapoznawcza)](https://dotnet.microsoft.com/download/dotnet-core/3.1).
   * Uruchom następujące polecenie w powłoce poleceń. [Microsoft.AspNetCore.Blazor.Pakiet szablonów](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) ma wersję zapoznawczą, a Blazor webassembly jest w wersji zapoznawczej.


   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1\. Zainstaluj [program Visual Studio 16,4 lub nowszy](https://visualstudio.microsoft.com/vs/preview/) przy użyciu obciążeń **ASP.NET i Web Development** .

   2\. Utwórz nowy projekt.

   3 \. Wybierz pozycję **Blazor aplikacji**. Wybierz pozycję **Dalej**.

   4\. Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu. Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu. Wybierz przycisk **Utwórz**.

   5 \. W przypadku Blazor środowiska webassembly wybierz szablon **aplikacjiBlazor webassembly** . W przypadku środowiska Blazor Server wybierz szablon **aplikacjaBlazor Server** . Wybierz przycisk **Utwórz**. Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.

   6 \. Naciśnij klawisz **Ctrl** ,+**F5** , aby uruchomić aplikację.

   > [!NOTE]
   > Jeśli zainstalowano rozszerzenie programu Blazor Visual Studio dla starszej wersji zapoznawczej ASP.NET Core Blazor (wersja zapoznawcza 6 lub wcześniejsza), można odinstalować rozszerzenie. Instalowanie szablonów Blazor w powłoce poleceń jest teraz wystarczające do poszycia szablonów w programie Visual Studio.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1\. Zainstalowanie programu [Visual Studio Code](https://code.visualstudio.com/).

   2\. Zainstaluj najnowsze [ C# rozszerzenie programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3 \. W przypadku Blazor środowiska webassembly wykonaj następujące polecenie w powłoce poleceń:

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      W przypadku środowiska Blazor Server wykonaj następujące polecenie w powłoce poleceń:

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.

   4\. Otwórz folder *WebApplication1* w Visual Studio Code.

   5 \. Dla projektu serwera Blazor, żądania IDE, które umożliwiają dodanie zasobów do kompilowania i debugowania projektu. Wybierz pozycję **Yes**.

   6 \. Jeśli używasz aplikacji serwera Blazor, uruchom aplikację przy użyciu debugera Visual Studio Code. Jeśli używasz aplikacji Blazor webassembly, wykonaj `dotnet run` z folderu projektu aplikacji.

   7 \. W przeglądarce przejdź do `https://localhost:5001`.

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

   1\. Zainstaluj [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac/).

   2\. Wybierz pozycję **plik** > **nowe rozwiązanie** lub Utwórz **Nowy projekt**.

   3 \. Na pasku bocznym wybierz pozycję **aplikacja** **.NET Core** > .

   4\. Wybierz szablon **aplikacjiBlazor Server** . W tej chwili w Visual Studio dla komputerów Mac jest dostępny tylko szablon Blazor Server. W przypadku Blazor środowiska webassembly postępuj zgodnie z instrukcjami na karcie **interfejs wiersza polecenia platformy .NET Core** . Po wybraniu szablonu serwera Blazor wybierz pozycję **dalej**. Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   5 \. Ustaw platformę **docelową** na **platformę .NET Core 3,1** i wybierz pozycję **dalej**.

   6 \. W polu **Nazwa projektu** Nadaj nazwę aplikacji `WebApplication1`. Wybierz przycisk **Utwórz**.

   7 \. Wybierz pozycję **uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację *bez debugera*. Uruchom aplikację przy użyciu **Rozpocznij debugowanie** , aby uruchomić aplikację *za pomocą debugera*.

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

   W przypadku Blazor środowiska webassembly wykonaj następujące polecenia w powłoce poleceń:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   W przypadku środowiska Blazor Server wykonaj następujące polecenia w powłoce poleceń:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.

   W przeglądarce przejdź do `https://localhost:5001`.

   ---

::: moniker-end

::: moniker range="< aspnetcore-3.1"

1. Zainstaluj najnowszą wersję [zestawu SDK platformy .NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0).

1. Opcjonalnie zainstaluj szablon [Blazor webassembly](xref:blazor/hosting-models#blazor-webassembly) :
   * Zainstaluj [zestaw SDK platformy .NET Core 3,1 lub nowszego (wersja zapoznawcza)](https://dotnet.microsoft.com/download/dotnet-core/3.1).
   * Uruchom następujące polecenie w powłoce poleceń. [Microsoft.AspNetCore.Blazor.Pakiet szablonów](https://www.nuget.org/packages/Microsoft.AspNetCore.Blazor.Templates/) ma wersję zapoznawczą, a Blazor webassembly jest w wersji zapoznawczej.

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2
   ```

1. Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1\. Zainstaluj najnowszą wersję [programu Visual Studio](https://visualstudio.com/vs/) , korzystając z obciążeń **ASP.NET i Web Development** .

   2\. Opcjonalnie zainstaluj [program Visual Studio 16,4 w wersji zapoznawczej 2 lub nowszej](https://visualstudio.microsoft.com/vs/preview/) przy użyciu obciążeń **ASP.NET i Web Development** Blazor dla tworzenia aplikacji webassembly.

   3 \. Utwórz nowy projekt.

   4\. Wybierz pozycję **Blazor aplikacji**. Wybierz pozycję **Dalej**.

   5 \. Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu. Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu. Wybierz przycisk **Utwórz**.

   6 \. W przypadku Blazor środowiska webassembly wybierz szablon **aplikacjiBlazor webassembly** . W przypadku środowiska Blazor Server wybierz szablon **aplikacjaBlazor Server** . Wybierz przycisk **Utwórz**. Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.

   7 \. Naciśnij klawisz **F5** , aby uruchomić aplikację.

   > [!NOTE]
   > Jeśli zainstalowano rozszerzenie programu Blazor Visual Studio dla starszej wersji zapoznawczej ASP.NET Core Blazor (wersja zapoznawcza 6 lub wcześniejsza), można odinstalować rozszerzenie. Instalowanie szablonów Blazor w powłoce poleceń jest teraz wystarczające do poszycia szablonów w programie Visual Studio.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1\. Zainstalowanie programu [Visual Studio Code](https://code.visualstudio.com/).

   2\. Zainstaluj najnowsze [ C# rozszerzenie programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3 \. W przypadku Blazor środowiska webassembly wykonaj następujące polecenie w powłoce poleceń:

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      W przypadku środowiska Blazor Server wykonaj następujące polecenie w powłoce poleceń:

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.

   4\. Otwórz folder *WebApplication1* w Visual Studio Code.

   5 \. Dla projektu serwera Blazor, żądania IDE, które umożliwiają dodanie zasobów do kompilowania i debugowania projektu. Wybierz pozycję **Yes**.

   6 \. Jeśli używasz aplikacji serwera Blazor, uruchom aplikację przy użyciu debugera Visual Studio Code. Jeśli używasz aplikacji Blazor webassembly, wykonaj `dotnet run` z folderu projektu aplikacji.

   7 \. W przeglądarce przejdź do `https://localhost:5001`.

   # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio dla komputerów Mac](#tab/visual-studio-mac)

   1\. Zainstaluj [Visual Studio dla komputerów Mac](https://visualstudio.microsoft.com/vs/mac/). Przełącz [kanał aktualizacji na wersję zapoznawczą](/visualstudio/mac/install-preview).

   2\. Wybierz pozycję **plik** > **nowe rozwiązanie** lub Utwórz **Nowy projekt**.

   3 \. Na pasku bocznym wybierz pozycję **aplikacja** **.NET Core** > .

   4\. Wybierz szablon **aplikacjiBlazor Server** . W tej chwili w Visual Studio dla komputerów Mac jest dostępny tylko szablon Blazor Server. W przypadku Blazor środowiska webassembly postępuj zgodnie z instrukcjami na karcie **interfejs wiersza polecenia platformy .NET Core** . Po wybraniu szablonu serwera Blazor wybierz pozycję **dalej**. Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.

   <!-- For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. -->

   5 \. Ustaw platformę **docelową** na **platformę .NET Core 3,0** i wybierz pozycję **dalej**.

   6 \. W polu **Nazwa projektu** Nadaj nazwę aplikacji `WebApplication1`. Wybierz przycisk **Utwórz**.

   7 \. Wybierz pozycję **uruchom** > **Uruchom bez debugowania** , aby uruchomić aplikację *bez debugera*. Uruchom aplikację przy użyciu **Rozpocznij debugowanie** , aby uruchomić aplikację *za pomocą debugera*.

       If a prompt appears to trust the development certificate, trust the certificate and continue.

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

   W przypadku Blazor środowiska webassembly wykonaj następujące polecenia w powłoce poleceń:

   ```dotnetcli
   dotnet new blazorwasm -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   W przypadku środowiska Blazor Server wykonaj następujące polecenia w powłoce poleceń:

   ```dotnetcli
   dotnet new blazorserver -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Aby uzyskać informacje na temat dwóch Blazor modeli hostingu, *Blazor Server* i *Blazor webassembly*, zobacz <xref:blazor/hosting-models>.

   W przeglądarce przejdź do `https://localhost:5001`.

   ---

::: moniker-end

Na pasku bocznym są dostępne wiele stron:

* Strona główna programu
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
