---
title: Wprowadzenie do ASP.NET Core Blazor
author: guardrex
description: Rozpocznij pracę z usługą Blazor, tworząc aplikację Blazor przy użyciu wybranych przez siebie narzędzi.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/25/2019
uid: blazor/get-started
ms.openlocfilehash: 5aec91eff7de0732a47fec1aafa5e094c89c37a4
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/26/2019
ms.locfileid: "71295434"
---
# <a name="get-started-with-aspnet-core-blazor"></a>Wprowadzenie do ASP.NET Core Blazor

Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Rozpocznij pracę z usługą Blazor:

1. Zainstaluj najnowszą wersję [zestawu SDK platformy .NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0) .

1. Zainstaluj szablon [Webassembly Blazor](xref:blazor/hosting-models#blazor-webassembly) , uruchamiając następujące polecenie w powłoce poleceń:

   ```dotnetcli
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview9.19465.2
   ```

1. Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1\. Zainstaluj najnowszą wersję [programu Visual Studio](https://visualstudio.com/vs/) , korzystając z obciążeń **ASP.NET i Web Development** .

   2\. Utwórz nowy projekt.

   3 \. Wybierz pozycję **aplikacja Blazor**. Wybierz opcję **Dalej**.

   4\. Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu. Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu. Wybierz pozycję **Utwórz**.

   5 \. Aby zapoznać się z Blazor webassembly, wybierz szablon **aplikacji Blazor webassembly** . Dla środowiska serwera Blazor wybierz szablon **aplikacji Blazor Server** . Wybierz pozycję **Utwórz**. Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, <xref:blazor/hosting-models>Zobacz.

   6 \. Naciśnij klawisz **F5** , aby uruchomić aplikację.

   > [!NOTE]
   > Jeśli zainstalowano rozszerzenie Blazor programu Visual Studio dla starszej wersji zapoznawczej programu ASP.NET Core Blazor (wersja zapoznawcza 6 lub wcześniejsza), można odinstalować rozszerzenie. Instalowanie szablonów Blazor w powłoce poleceń jest teraz wystarczające do poszycia szablonów w programie Visual Studio.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1\. Zainstaluj [Visual Studio Code](https://code.visualstudio.com/).

   2\. Zainstaluj najnowsze [ C# rozszerzenie programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3 \. W przypadku środowiska webassembly Blazor wykonaj następujące polecenie w powłoce poleceń:

      ```dotnetcli
      dotnet new blazorwasm -o WebApplication1
      ```

      W przypadku środowiska serwera Blazor wykonaj następujące polecenie w powłoce poleceń:

      ```dotnetcli
      dotnet new blazorserver -o WebApplication1
      ```

      Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, <xref:blazor/hosting-models>Zobacz.

   4\. Otwórz folder *WebApplication1* w Visual Studio Code.

   5 \. W przypadku projektu serwera Blazor, IDE żąda dodania zasobów do kompilowania i debugowania projektu. Wybierz pozycję **tak**.

   6 \. W przypadku korzystania z aplikacji serwera Blazor należy uruchomić aplikację przy użyciu debugera Visual Studio Code. Jeśli używasz aplikacji Blazor webassembly, wykonaj `dotnet run` z folderu projektu aplikacji.

   7 \. W przeglądarce przejdź do `https://localhost:5001`.

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor Server experience, select the **Blazor Server App** template. For a Blazor WebAssembly experience, select the **Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, *Blazor Server* and *Blazor WebAssembly*, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

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

   Aby uzyskać informacje na temat dwóch modeli hostingu Blazor, *Blazor Server* i *Blazor webassembly*, <xref:blazor/hosting-models>Zobacz.

   W przeglądarce przejdź do `https://localhost:5001`.

   ---

Na pasku bocznym są dostępne wiele stron:

* Home
* Licznik
* Pobieranie danych

Na stronie licznik wybierz przycisk **kliknij** , aby zwiększyć licznik bez odświeżania strony. Zwiększenie licznika na stronie sieci Web zwykle wymaga pisania kodu JavaScript, ale składniki Razor zapewniają lepsze podejście przy użyciu C#.

*Pages/Counter. Razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

Żądanie `/counter` w przeglądarce, zgodnie z definicją `@page` w dyrektywie u góry, powoduje, że `Counter` składnik renderuje jego zawartość. Składniki są renderowane w postaci reprezentacji drzewa renderowania, która może być następnie używana do aktualizowania interfejsu użytkownika w elastyczny i wydajny sposób.

Za każdym razem, gdy zostanie wybrany przycisk **kliknij mnie** :

* `onclick` Zdarzenie jest wyzwalane.
* `IncrementCount` Metoda jest wywoływana.
* Wartość `currentCount` jest zwiększana.
* Składnik jest ponownie renderowany.

Środowisko uruchomieniowe porównuje nową zawartość z poprzednią zawartością i stosuje tylko zmienioną zawartość do Document Object Model (DOM).

Dodaj składnik do innego składnika przy użyciu składni języka HTML. Na przykład Dodaj `Counter` składnik do strony głównej aplikacji przez `<Counter />` dodanie elementu do `Index` składnika.

*Pages/index. Razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Uruchom aplikację. Strona główna ma swój własny licznik dostarczony przez `Counter` składnik.

Parametry składnika są określone przy użyciu atrybutów lub [zawartości podrzędnej](xref:blazor/components#child-content), które umożliwiają ustawianie właściwości składnika podrzędnego. Aby dodać parametr do `Counter` składnika, zaktualizuj `@code` blok składnika:

* Dodaj właściwość publiczną dla `IncrementAmount` `[Parameter]` atrybutu.
* Zmień metodę, aby `IncrementAmount` użyć `currentCount`podczas zwiększania wartości. `IncrementCount`

*Pages/Counter. Razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

`<Counter>` Określ element `IncrementAmount` w elemencie `Index` składnika przy użyciu atrybutu.

*Pages/index. Razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Uruchom aplikację. Składnik ma swój własny licznik, który zwiększa się o dziesięć za każdym razem, gdy jest zaznaczony przycisk **kliknij mnie.** `Index` Składnik (*Counter. Razor*) w `/counter` dalszym ciągu zwiększa się o jeden. `Counter`

## <a name="next-steps"></a>Następne kroki

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:signalr/introduction>
