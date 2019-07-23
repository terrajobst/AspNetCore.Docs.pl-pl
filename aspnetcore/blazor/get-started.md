---
title: Wprowadzenie do ASP.NET Core Blazor
author: guardrex
description: Rozpocznij pracę z usługą Blazor, tworząc aplikację Blazor przy użyciu wybranych przez siebie narzędzi.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/19/2019
uid: blazor/get-started
ms.openlocfilehash: 7cc302216d14a6f1791ac3c0892d03ddb8abc974
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371791"
---
# <a name="get-started-with-aspnet-core-blazor"></a>Wprowadzenie do ASP.NET Core Blazor

Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

Rozpocznij pracę z usługą Blazor:

1. Zainstaluj najnowszą wersję [zestawu SDK programu .NET Core 3,0](https://dotnet.microsoft.com/download/dotnet-core/3.0) w wersji zapoznawczej.

1. Zainstaluj szablony Blazor, uruchamiając następujące polecenie w powłoce poleceń:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview6.19307.2
   ```

1. Postępuj zgodnie ze wskazówkami dotyczącymi wybranego narzędzia:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1\. Zainstaluj najnowszą wersję [zapoznawczą programu Visual Studio](https://visualstudio.com/vs/preview) , korzystając z obciążeń **ASP.NET i Web Development** .

   2\. Zainstaluj najnowsze [rozszerzenie Blazor](https://go.microsoft.com/fwlink/?linkid=870389) z Visual Studio Marketplace. Ten krok sprawia, że szablony Blazor są dostępne dla programu Visual Studio.

   3 \. Utwórz nowy projekt.

   4\. Wybierz **aplikacji sieci Web platformy ASP.NET Core**. Wybierz opcję **Dalej**.

   5 \. Podaj nazwę projektu w polu **Nazwa projektu** lub zaakceptuj nazwę domyślną projektu. Potwierdź, że wpis **lokalizacji** jest poprawny lub podaj lokalizację dla projektu. Wybierz pozycję **Utwórz**.

   6 \. W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** upewnij się, że wybrano opcję **.net Core** i **ASP.NET Core 3,0** .

   7 \. W przypadku środowiska po stronie klienta Blazor wybierz szablon **aplikacji Blazor webassembly** . W przypadku środowiska po stronie serwera Blazor wybierz szablon **aplikacji Blazor Server** . Wybierz pozycję **Utwórz**. Aby uzyskać informacje na temat dwóch Blazorch modeli hostingu, po stronie serwera i klienta, zobacz <xref:blazor/hosting-models>.

   8 \. Naciśnij klawisz **F5** , aby uruchomić aplikację.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

   1\. Zainstaluj [Visual Studio Code](https://code.visualstudio.com/).

   2\. Zainstaluj najnowsze [ C# rozszerzenie programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3 \. W przypadku środowiska po stronie klienta Blazor wykonaj następujące polecenie w powłoce poleceń:

      ```console
      dotnet new blazor -o WebApplication1
      ```

      W przypadku środowiska po stronie serwera Blazor wykonaj następujące polecenie w powłoce poleceń:

      ```console
      dotnet new blazorserverside -o WebApplication1
      ```

      Aby uzyskać informacje na temat dwóch Blazorch modeli hostingu, po stronie serwera i klienta, zobacz <xref:blazor/hosting-models>.

   4\. Otwórz folder *WebApplication1* w Visual Studio Code.

   5 \. W przypadku projektu po stronie serwera Blazor IDE żąda dodania zasobów do kompilowania i debugowania projektu. Wybierz pozycję **tak**.

   6 \. Jeśli używasz aplikacji po stronie serwera Blazor, uruchom aplikację przy użyciu debugera Visual Studio Code. Jeśli używasz aplikacji po stronie klienta Blazor, wykonaj `dotnet run` z folderu projektu aplikacji.

   7 \. W przeglądarce przejdź do `https://localhost:5001`.

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor server-side experience, select the **ASP.NET Core Blazor Server App** template. For a Blazor client-side experience, select the **ASP.NET Core Blazor WebAssembly App** template. Select **Next**. For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

   W przypadku środowiska po stronie klienta Blazor wykonaj następujące polecenia w powłoce poleceń:

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   W przypadku środowiska Blazor po stronie serwera wykonaj następujące polecenia w powłoce poleceń:

   ```console
   dotnet new blazorserverside -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Aby uzyskać informacje na temat dwóch Blazorch modeli hostingu, po stronie serwera i klienta, zobacz <xref:blazor/hosting-models>.

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

* Dodaj właściwość dla `IncrementAmount` `[Parameter]` atrybutu.
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
