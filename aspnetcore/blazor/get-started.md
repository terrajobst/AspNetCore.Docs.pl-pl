---
title: Rozpoczynanie pracy z usługą Blazor
author: guardrex
description: Rozpoczynanie pracy z usługą Blazor, tworząc aplikację Blazor, za pomocą narzędzi dostępnych w wybranym.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/06/2019
uid: blazor/get-started
ms.openlocfilehash: 09613f5d8a4d130f7dca53f31bdd33de527fc776
ms.sourcegitcommit: b4ef2b00f3e1eb287138f8b43c811cb35a100d3e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969865"
---
# <a name="get-started-with-blazor"></a>Rozpoczynanie pracy z usługą Blazor

Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

Rozpoczynanie pracy z usługą Blazor:

1. Zainstaluj najnowszą wersję [zestawu SDK programu .NET Core 3.0 w wersji zapoznawczej](https://dotnet.microsoft.com/download/dotnet-core/3.0) wydania.

1. Zainstaluj szablony Blazor, uruchamiając następujące polecenie w powłoce poleceń:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview5-19227-01
   ```

1. Zgodnie z wytycznymi z dowolnie wybranych narzędzi:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1.&nbsp;zainstaluj najnowszą wersję [programu Visual Studio w wersji zapoznawczej](https://visualstudio.com/preview) za pomocą **ASP.NET i tworzenie aplikacji internetowych** obciążenia.

   2.&nbsp;zainstaluj najnowszą wersję [rozszerzenia Blazor](https://go.microsoft.com/fwlink/?linkid=870389) z witryny Marketplace programu Visual Studio. Ten krok udostępnia Blazor szablony programu Visual Studio.

   3.&nbsp;używania stabilne najnowszą wersję programu Visual Studio (nie wersja zapoznawcza), włączyć Visual Studio ma używać zestawów SDK w wersji zapoznawczej: Otwórz **narzędzia** > **opcje** na pasku menu. Otwórz **projekty i rozwiązania** węzła. Otwórz **platformy .NET Core** kartę. Pole wyboru dla **Użyj wersji zapoznawczych programu .NET Core SDK**. Kliknij przycisk **OK**.

   4.&nbsp;Utwórz nowy projekt.

   5.&nbsp;wybierz **aplikacji sieci Web platformy ASP.NET Core**. Wybierz opcję **Dalej**.

   6.&nbsp;Podaj nazwę projektu w **Nazwa projektu** pola lub zaakceptuj domyślną nazwę projektu. Upewnij się, **lokalizacji** wpis jest poprawny lub podaj lokalizację dla projektu. Wybierz pozycję **Utwórz**.

   7.&nbsp;w **Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core** okna dialogowego, upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** są zaznaczone.

   8.&nbsp;Blazor środowisko pracy klienta, wybierz **Blazor (po stronie klienta)** szablonu. Środowisko pracy Blazor po stronie serwera, wybierz **Blazor (po stronie serwera)** szablonu. Wybierz pozycję **Utwórz**. Aby uzyskać informacje dotyczące dwóch modelach hostingu Blazor, po stronie serwera i klienta, zobacz <xref:blazor/hosting-models>.

   9.&nbsp;naciśnij **F5** do uruchomienia aplikacji.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)
   
   1.&nbsp;zainstalować [programu Visual Studio Code](https://code.visualstudio.com/).

   2.&nbsp;zainstaluj najnowszą wersję [ C# rozszerzenia programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3.&nbsp;Blazor środowisko pracy klienta, wykonaj następujące polecenie z powłoki poleceń:

      ```console
      dotnet new blazor -o WebApplication1
      ```

      Środowisko pracy Blazor po stronie serwera wykonaj następujące polecenie z powłoki poleceń:

      ```console
      dotnet new blazorserverside -o WebApplication1
      ```

      Aby uzyskać informacje dotyczące dwóch modelach hostingu Blazor, po stronie serwera i klienta, zobacz <xref:blazor/hosting-models>.

   4.&nbsp;Otwórz *WebApplication1* folderu w programie Visual Studio Code.

   5.&nbsp;projektu po stronie serwera dla Blazor, IDE żądań, dodać zasoby do tworzenia i debugowania projektu. Wybierz **tak**.

   6.&nbsp;Jeśli za pomocą Blazor aplikacji po stronie serwera, uruchom aplikację za pomocą debugera programu Visual Studio Code. Jeśli używasz aplikacji po stronie klienta Blazor, wykonaj `dotnet run` z folderu projektu aplikacji.

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1.&nbsp;Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2.&nbsp;Select **File** > **New Solution** or **New Project**.

   3.&nbsp;In the sidebar, select **.NET Core** > **App**.

   4.&nbsp;For a Blazor server-side experience, select the **ASP.NET Core Blazor (server-side)** template. For a Blazor client-side experience, select the **ASP.NET Core Blazor (client-side)** template. Select **Next**. For information on the two Blazor hosting models, server-side and client-side, see <xref:blazor/hosting-models>.

   5.&nbsp;The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6.&nbsp;In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7.&nbsp;Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

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

   ---

W przeglądarce przejdź do `https://localhost:5001`.

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

Dodaj składnik do innego składnika przy użyciu składni HTML. Składnik parametry są określane, za pomocą atrybutów i zawartość elementu podrzędnego. Na przykład można dodać składnik licznika do strony głównej aplikacji, dodając `<Counter />` elementu składnik indeksu.

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Uruchom aplikację. Strona główna ma swój własny licznika, dostarczone przez składnik licznika.

Aby dodać parametr do składnika licznika, należy zaktualizować składnika `@functions` bloku:

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
