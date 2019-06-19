---
title: Wprowadzenie do platformy ASP.NET Core Blazor
author: guardrex
description: Rozpoczynanie pracy z usługą Blazor, tworząc aplikację Blazor, za pomocą narzędzi dostępnych w wybranym.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/18/2019
uid: blazor/get-started
ms.openlocfilehash: c614ff52600434158c75e288e0b15985c0eb8e68
ms.sourcegitcommit: a1283d486ac1dcedfc7ea302e1cc882833e2c515
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67207659"
---
# <a name="get-started-with-aspnet-core-blazor"></a>Wprowadzenie do platformy ASP.NET Core Blazor

Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

Rozpoczynanie pracy z usługą Blazor:

1. Zainstaluj najnowszą wersję [zestawu SDK programu .NET Core 3.0 w wersji zapoznawczej](https://dotnet.microsoft.com/download/dotnet-core/3.0) wydania.

1. Zainstaluj szablony Blazor, uruchamiając następujące polecenie w powłoce poleceń:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview6.19307.2
   ```

1. Zgodnie z wytycznymi z dowolnie wybranych narzędzi:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1\. Zainstaluj najnowszą wersję [programu Visual Studio preview](https://visualstudio.com/vs/preview) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia.

   2\. Zainstaluj najnowszą wersję [rozszerzenia Blazor](https://go.microsoft.com/fwlink/?linkid=870389) z witryny Marketplace programu Visual Studio. Ten krok udostępnia Blazor szablony programu Visual Studio.

   3\. Utwórz nowy projekt.

   4\. Wybierz **aplikacji sieci Web platformy ASP.NET Core**. Wybierz opcję **Dalej**.

   5\. Podaj nazwę projektu w **Nazwa projektu** pola lub zaakceptuj domyślną nazwę projektu. Upewnij się, **lokalizacji** wpis jest poprawny lub podaj lokalizację dla projektu. Wybierz pozycję **Utwórz**.

   6\. W **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okna dialogowego, upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** są zaznaczone.

   7\. Środowisko pracy klienta Blazor, wybierz **Blazor (po stronie klienta)** szablonu. Środowisko pracy Blazor po stronie serwera, wybierz **Blazor (po stronie serwera)** szablonu. Wybierz pozycję **Utwórz**. Aby uzyskać informacje dotyczące dwóch modelach hostingu Blazor, po stronie serwera i klienta, zobacz <xref:blazor/hosting-models>.

   8\. Naciśnij klawisz **F5** do uruchomienia aplikacji.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)
   
   1\. Zainstaluj [programu Visual Studio Code](https://code.visualstudio.com/).

   2\. Zainstaluj najnowszą wersję [ C# rozszerzenia programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3\. Środowisko pracy klienta Blazor wykonaj następujące polecenie z powłoki poleceń:

      ```console
      dotnet new blazor -o WebApplication1
      ```

      Środowisko pracy Blazor po stronie serwera wykonaj następujące polecenie z powłoki poleceń:

      ```console
      dotnet new blazorserverside -o WebApplication1
      ```

      Aby uzyskać informacje dotyczące dwóch modelach hostingu Blazor, po stronie serwera i klienta, zobacz <xref:blazor/hosting-models>.

   4\. Otwórz *WebApplication1* folderu w programie Visual Studio Code.

   5\. W projekcie po stronie serwera Blazor IDE żądań, dodać zasoby do tworzenia i debugowania projektu. Wybierz **tak**.

   6\. Jeśli używasz aplikacji po stronie serwera Blazor, uruchom aplikację za pomocą debugera programu Visual Studio Code. Jeśli używasz aplikacji po stronie klienta Blazor, wykonaj `dotnet run` z folderu projektu aplikacji.

   7\. W przeglądarce przejdź do `https://localhost:5001`.

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1\. Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2\. Select **File** > **New Solution** or **New Project**.

   3\. In the sidebar, select **.NET Core** > **App**.

   4\. For a Blazor server-side experience, select the **ASP.NET Core Blazor (server-side)** template. For a Blazor client-side experience, select the **ASP.NET Core Blazor (client-side)** template. Select **Next**. For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.

   5\. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6\. In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7\. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

   Środowisko pracy klienta Blazor wykonaj następujące polecenia z powłoki poleceń:

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Środowisko pracy Blazor po stronie serwera wykonaj następujące polecenia z powłoki poleceń:

   ```console
   dotnet new blazorserverside -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Aby uzyskać informacje dotyczące dwóch modelach hostingu Blazor, po stronie serwera i klienta, zobacz <xref:blazor/hosting-models>.

   W przeglądarce przejdź do `https://localhost:5001`.

   ---

Wiele stron są dostępne na kartach w pasku bocznym:

* Home
* Licznik
* Pobieranie danych

Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony. Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale Razor składniki zapewniają lepsze przy użyciu podejścia C#.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor?highlight=7,12-15)]

Żądanie dotyczące `/counter` w przeglądarce, określony przez `@page` dyrektywy u góry strony, powoduje, że składnik licznika do renderowania jego zawartości. Składniki renderowania do reprezentacji w pamięci drzewa renderowania, który następnie może służyć do aktualizowania interfejsu użytkownika w elastyczny i efektywny sposób.

Każdorazowo **kliknij mnie** wybrany przycisk:

* `onclick` Jest wyzwalane zdarzenie.
* `IncrementCount` Metoda jest wywoływana.
* `currentCount` Jest zwiększany.
* Składnik jest renderowany ponownie.

Środowisko uruchomieniowe porównuje nową zawartość do poprzedniego zawartości i ma zastosowanie tylko zmiany zawartości do modelu DOM (Document Object).

Dodaj składnik do innego składnika przy użyciu składni języka HTML. Na przykład dodać składnik licznika do strony głównej aplikacji, dodając `<Counter />` elementu składnik indeksu.

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Uruchom aplikację. Strona główna ma swój własny licznika, dostarczone przez składnik licznika.

Składnik parametry są określane za pomocą atrybutów lub [zawartość elementu podrzędnego](xref:blazor/components#child-content), co pozwala użytkownikowi na ustawianie właściwości w składniku podrzędnych. Aby dodać parametr do składnika licznika, należy zaktualizować składnika `@code` bloku:

* Dodaj właściwość `IncrementAmount` z `[Parameter]` atrybutu.
* Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=12-13,17)]

Określ `IncrementAmount` w składnik indeksu `<Counter>` elementu za pomocą atrybutu.

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor?highlight=7)]

Uruchom aplikację. Składnik indeksu ma swój własny licznik, który zwiększa przez dziesięć każdorazowo **kliknij mnie** przycisk jest zaznaczony. Składnik Counter (*Counter.razor*) na `/counter` w dalszym ciągu o jeden.

## <a name="next-steps"></a>Następne kroki

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:signalr/introduction>
