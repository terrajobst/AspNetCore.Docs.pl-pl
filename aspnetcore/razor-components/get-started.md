---
title: Wprowadzenie do składników Razor
author: guardrex
description: Dowiedz się, jak rozpocząć pracę z architekturą składniki Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/03/2019
uid: razor-components/get-started
ms.openlocfilehash: 83f8bbcd415c6776ef14e1ce789f3f0a8cecc464
ms.sourcegitcommit: a91e8dd2f4b788114c8bc834507277f4b5e8d6c5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55712229"
---
# <a name="get-started-with-razor-components"></a>Wprowadzenie do składników Razor

Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Wymagania wstępne:

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

Aby utworzyć swój pierwszy projekt Razor składników w programie Visual Studio:

1. Wybierz **pliku** > **nowy projekt** > **Web** > **aplikacji sieci Web platformy ASP.NET Core**.
1. Upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 3.0** wybrano u góry.
1. Wybierz **składniki Razor** szablonu, a następnie wybierz **OK**.

   ![Nowe okno dialogowe aplikacji](https://msdnshared.blob.core.windows.net/media/2019/01/razor-components-template.png)

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.

Gratulacje! Po prostu uruchomieniu pierwszej aplikacji składniki Razor!

<!--

# [Visual Studio Code](#tab/visual-studio-code)

Prerequisites:

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

To create your first Razor Components project in Visual Studio Code:

1. Execute the following command from a command shell:

   ```console
   dotnet new razorcomponents -o WebApplication1
   ```

1. Open the *WebApplication1* folder in Visual Studio Code.

1. Add a *.vscode* folder.

1. Add a *tasks.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/tasks.json)]

1. Add a *launch.json* file to the *.vscode* folder with the following content:

   [!code-json[](get-started/samples_snapshot/3.x/launch.json)]

1. Execute the app using the Visual Studio Code debugger.

1. In a browser, navigate to `https://localhost:5001`.

Congrats! You just ran your first Razor Components app!

# [Visual Studio for Mac](#tab/visual-studio-mac)

.NET Core 3.0 will be supported with Visual Studio for Mac version 8.0 or later. Visual Studio for Mac version 8.0 Preview isn't available at this time.

Use the [.NET Core CLI version of this topic](xref:razor-components/get-started?tabs=netcore-cli) on macOS.


[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

To create your first project Razor Components project in Visual Studio for Mac:

1. Select **File** > **New Solution** or **New Project**.
1. In the sidebar, select **.NET Core** > **App**.
1. Select **ASP.NET Core Razor Components** and select **Next**.
1. The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.
1. In the **Project Name** field, enter `WebApplication1`. Select **Create**.
1. Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

Congratulations! You just ran your first Razor Components app!
-->

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

Wymagania wstępne:

* [.NET core SDK 3.0 w wersji zapoznawczej](https://dotnet.microsoft.com/download/dotnet-core/3.0)

Aby utworzyć swój pierwszy projekt składniki Razor z powłoki poleceń:

```console
dotnet new razorcomponents -o WebApplication1
cd WebApplication1
dotnet run
```

Gratulacje! Po prostu uruchomieniu pierwszej aplikacji składniki Razor!

---

## <a name="razor-components-project"></a>Razor składników projektów

Rozwiązanie utworzone przez szablon Razor składników zawiera dwa projekty:

* *WebApplication1.Server* &ndash; Projekt serwera jest projektem platformy ASP.NET Core, ustawić, aby hostować aplikację składniki Razor.
* *WebApplication1.App* &ndash; projektu interfejsu użytkownika sieci web po stronie klienta, który używa składników Razor.

Logika interfejsu użytkownika w *WebApplication1.App* projektu jest oddzielony od pozostałej części aplikacji ze względu na ograniczenia techniczne w ASP.NET Core 3.0 w wersji zapoznawczej 2. Rozszerzenie pliku Razor (*.cshtml*) używany dla składników Razor jest również używany widoków stron Razor i MVC. Obecnie składniki Razor i stron Razor/MVC mają modeli różnych kompilacji, dlatego plikach Razor składniki Razor są oddzielone. W przyszłej wersji zapoznawczej, planujemy wprowadzenie nowe rozszerzenie pliku dla składników Razor (*.razor*). Składniki, strony i widoki będzie obsługiwana *w tym samym projekcie*.

Gdy aplikacja jest uruchamiana, wiele stron są dostępne na kartach w pasku bocznym:

* Home
* Licznik
* Pobieranie danych

Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony. Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale składniki Razor zapewnia lepsze przy użyciu podejścia C#.

*WebApplication1.App/Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.cshtml)]

Żądanie dotyczące `/counter` w przeglądarce, określony przez `@page` dyrektywy u góry strony, powoduje, że składnik licznika do renderowania jego zawartości. Składniki renderowania do reprezentacji w pamięci drzewa renderowania, który następnie może służyć do aktualizowania interfejsu użytkownika w elastyczny i efektywny sposób.

Każdorazowo **kliknij mnie** wybrany przycisk:

* `onclick` Jest wyzwalane zdarzenie.
* `IncrementCount` Metoda jest wywoływana.
* `currentCount` Jest zwiększany.
* Składnik jest renderowany ponownie.

Środowisko uruchomieniowe porównuje nową zawartość do poprzedniego zawartości i ma zastosowanie tylko zmiany zawartości do modelu DOM (Document Object).

Dodaj składnik do innego składnika przy użyciu składni notacji HTML. Składnik parametry są określane, za pomocą atrybutów i zawartość elementu podrzędnego. Na przykład można dodać składnik licznika do strony głównej aplikacji, dodając `<Counter />` elementu składnik indeksu.

*WebApplication1.App/Pages/Index.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.cshtml?highlight=7)]

Uruchom aplikację. Strona główna ma swój własny licznika.

Aby dodać parametr do składnika licznika, należy zaktualizować składnika `@functions` bloku:

* Dodaj właściwość `IncrementAmount` ozdobione `[Parameter]` atrybutu.
* Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.

*WebApplication1.App/Pages/Counter.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.cshtml?highlight=4,8)]

Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu.

*WebApplication1.App/Pages/Index.cshtml*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.cshtml)]

Uruchom aplikację. Strona główna ma swój własny licznik, który zwiększa przez dziesięć każdorazowo **kliknij mnie** przycisk jest zaznaczony.

## <a name="next-steps"></a>Następne kroki

<xref:tutorials/first-blazor-app>
