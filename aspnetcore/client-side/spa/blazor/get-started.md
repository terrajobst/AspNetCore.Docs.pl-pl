---
title: Rozpoczynanie pracy z usługą Blazor
author: guardrex
description: Dowiedz się, jak rozpocząć pracę z Blazor, tworząc i modyfikując projekt Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: spa/blazor/get-started
ms.openlocfilehash: b3928c2812be6f34cdf2f17295a1251106f651e5
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068238"
---
# <a name="get-started-with-blazor"></a>Rozpoczynanie pracy z usługą Blazor

Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# [<a name="visual-studio"></a>Visual Studio](#tab/visual-studio)

Wymagania wstępne:

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

Aby utworzyć swój pierwszy projekt Blazor w programie Visual Studio:

1. Zainstaluj najnowszą wersję [zestawu SDK programu .NET Core 3.0 w wersji zapoznawczej](https://dotnet.microsoft.com/download/dotnet-core/3.0) wydania.
1. Włącz Visual Studio ma używać zestawów SDK w wersji zapoznawczej:
   1. Otwórz **narzędzia** > **opcje** na pasku menu.
   1. Otwórz **projekty i rozwiązania** węzła. Otwórz **platformy .NET Core** kartę.
   1. Pole wyboru dla **Użyj wersji zapoznawczych programu .NET Core SDK**. Kliknij przycisk **OK**.
1. Zainstaluj najnowszą wersję [rozszerzenia Blazor](https://go.microsoft.com/fwlink/?linkid=870389) z witryny Marketplace programu Visual Studio. Ten krok udostępnia Blazor szablony programu Visual Studio.
1. Udostępnia szablony Blazor do użytku z programem .NET Core interfejsu wiersza polecenia, uruchamiając następujące polecenie w powłoce poleceń:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```
1. Utwórz nowy projekt.
1. Wybierz **aplikacji sieci Web platformy ASP.NET Core**. Wybierz opcję **Dalej**.
1. Podaj nazwę w **Nazwa projektu** pola. Upewnij się, **lokalizacji** wpis jest poprawny lub podaj lokalizację dla projektu. Wybierz pozycję **Utwórz**.
1. Upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** wybrano u góry.
1. Wybierz **Blazor** szablonu, a następnie wybierz **Utwórz**.
1. Naciśnij klawisz **F5** do uruchomienia aplikacji.

Gratulacje! Po prostu uruchomiono swoją pierwszą aplikację Blazor!

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Blazor project in Visual Studio Code:

1. Execute the following command in a command shell:

   ```console
   dotnet new blazor -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Visual Studio code offers to create assets to build and debug the app, which includes the *tasks.json* and *launch.json* files. Select **Yes** to add the assets.

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congratulations! You just ran your first Blazor app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Blazor project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **Blazor** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Blazor app!
-->

# [<a name="net-core-cli"></a>.NET core interfejsu wiersza polecenia](#tab/netcore-cli/)

Wymagania wstępne:

* [.NET core SDK 3.0 w wersji zapoznawczej](https://dotnet.microsoft.com/download/dotnet-core/3.0)

1. Dodaj do niego szablony Blazor, uruchamiając następujące polecenie w powłoce poleceń:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::0.9.0-preview3-19154-02
   ```

1. Utwórz swój pierwszy projekt Blazor w powłoce poleceń:

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

1. W przeglądarce przejdź do `https://localhost:5001`.

Gratulacje! Po prostu uruchomiono swoją pierwszą aplikację Blazor!

---

## <a name="blazor-project"></a>Blazor projektu

Gdy aplikacja jest uruchamiana, wiele stron są dostępne na kartach w pasku bocznym:

* Home
* Licznik
* Pobieranie danych

Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony. Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale Blazor zapewnia lepsze przy użyciu podejścia C#.

*Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

Żądanie dotyczące `/counter` w przeglądarce, określony przez `@page` dyrektywy u góry strony, powoduje, że składnik licznika do renderowania jego zawartości. Składniki renderowania do reprezentacji w pamięci drzewa renderowania, który następnie może służyć do aktualizowania interfejsu użytkownika w elastyczny i efektywny sposób.

Każdorazowo **kliknij mnie** wybrany przycisk:

* `onclick` Jest wyzwalane zdarzenie.
* `IncrementCount` Metoda jest wywoływana.
* `currentCount` Jest zwiększany.
* Składnik jest renderowany ponownie.

Środowisko uruchomieniowe porównuje nową zawartość do poprzedniego zawartości i ma zastosowanie tylko zmiany zawartości do modelu DOM (Document Object).

Dodaj składnik do innego składnika przy użyciu składni notacji HTML. Składnik parametry są określane, za pomocą atrybutów i zawartość elementu podrzędnego. Na przykład można dodać składnik licznika do strony głównej aplikacji, dodając `<Counter />` elementu składnik indeksu.

W *Pages/Index.cshtml*, Zastąp ankiety monitu składnika ze składnikiem licznika:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

Uruchom aplikację. Strona główna ma swój własny licznika.

Aby dodać parametr do składnika licznika, należy zaktualizować składnika `@functions` bloku:

* Dodaj właściwość `IncrementAmount` ozdobione `[Parameter]` atrybutu.
* Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.

*Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu.

*Pages/Index.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

Uruchom aplikację. Strona główna ma swój własny licznik, który zwiększa przez dziesięć każdorazowo **kliknij mnie** przycisk jest zaznaczony.

## <a name="next-steps"></a>Następne kroki

<xref:tutorials/first-razor-components-app>
