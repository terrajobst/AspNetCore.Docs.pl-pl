---
title: Rozpoczynanie pracy z usługą Blazor
author: guardrex
description: Dowiedz się, jak rozpocząć pracę z Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/19/2019
uid: blazor/get-started
ms.openlocfilehash: 45ae0acc6aaee433cce4eddb2fe9c59c306581d7
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982670"
---
# <a name="get-started-with-blazor"></a>Rozpoczynanie pracy z usługą Blazor

Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

W kilku krokach wprowadzenie Blazor:

1. Zainstaluj najnowszą wersję [zestawu SDK programu .NET Core 3.0 w wersji zapoznawczej](https://dotnet.microsoft.com/download/dotnet-core/3.0) wydania.

1. Zainstaluj szablony Blazor, uruchamiając następujące polecenie w powłoce poleceń:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates::3.0.0-preview4-19216-03
   ```

1. Zgodnie z wytycznymi z dowolnie wybranych narzędzi:

   # <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

   1.&nbsp;zainstalowania najnowszej wersji zapoznawczej [Visual Studio 2019](https://visualstudio.com/preview) z **ASP.NET i tworzenie aplikacji internetowych** obciążenia.

   2.&nbsp;zainstaluj najnowszą wersję [rozszerzenia Blazor](https://go.microsoft.com/fwlink/?linkid=870389) z witryny Marketplace programu Visual Studio. Ten krok udostępnia Blazor szablony programu Visual Studio.

   3.&nbsp;Włącz programu Visual Studio, aby użyć zestawów SDK w wersji zapoznawczej: Otwórz **narzędzia** > **opcje** na pasku menu. Otwórz **projekty i rozwiązania** węzła. Otwórz **platformy .NET Core** kartę. Pole wyboru dla **Użyj wersji zapoznawczych programu .NET Core SDK**. Kliknij przycisk **OK**.

   4.&nbsp;Utwórz nowy projekt.

   5.&nbsp;wybierz **aplikacji sieci Web platformy ASP.NET Core**. Wybierz opcję **Dalej**.

   6.&nbsp;Podaj nazwę w **Nazwa projektu** pola. Upewnij się, **lokalizacji** wpis jest poprawny lub podaj lokalizację dla projektu. Wybierz pozycję **Utwórz**.

   7.&nbsp;upewnij się, **platformy .NET Core** i **platformy ASP.NET Core 3.0** wybrano u góry.

   8.&nbsp;doświadczenia z Blazor po stronie klienta, można wybrać **Blazor (po stronie klienta)** szablonu. To środowisko z Blazor po stronie serwera wybierz **Blazor (po stronie serwera)** szablonu. Wybierz pozycję **Utwórz**.

   9.&nbsp;naciśnij **F5** do uruchomienia aplikacji.

   # <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)
   
   1.&nbsp;zainstalować [programu Visual Studio Code](https://code.visualstudio.com/).

   2.&nbsp;zainstaluj najnowszą wersję [ C# rozszerzenia programu Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

   3.&nbsp;środowisko pracy z Blazor po stronie klienta, wykonaj następujące polecenie z powłoki poleceń:

      ```console
      dotnet new blazor -o WebApplication1
      ```

      Środowisko pracy z Blazor po stronie serwera wykonaj następujące polecenie z powłoki poleceń:

      ```console
      dotnet new blazorserverside -o WebApplication1
      ```

      > [!NOTE]
      > Tylko Blazor po stronie klienta jest obsługiwana w systemie macOS w ASP.NET Core 3.0 w wersji zapoznawczej 4. Aby uzyskać więcej informacji, zobacz [po stronie serwera Blazor: dotnet, uruchom kończy się niepowodzeniem z InvalidOperationException w systemie MacOS](https://github.com/aspnet/AspNetCore/issues/9402).

   4.&nbsp;Otwórz *WebApplication1* folderu w programie Visual Studio Code.

   5.&nbsp;po wyświetleniu monitu przez Visual Studio Code dla projektu Blazor po stronie serwera można dodać zasoby wymagane do kompilowania i debugowania projektu, wybierz **tak**.

   6.&nbsp;Jeśli za pomocą Blazor aplikacji po stronie serwera, uruchom aplikację za pomocą debugera programu Visual Studio Code. Jeśli używasz aplikacji po stronie klienta Blazor, wykonaj `dotnet run` z folderu projektu aplikacji.

   <!--

   # [Visual Studio for Mac](#tab/visual-studio-mac)

   1.&nbsp;Install [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/). Switch the [Update channel to Preview](/visualstudio/mac/install-preview).

   2.&nbsp;Select **File** > **New Solution** or **New Project**.

   3.&nbsp;In the sidebar, select **.NET Core** > **App**.

   4.&nbsp;For an experience with Blazor server-side, select the **ASP.NET Core Blazor (server-side)** template. For an experience with Blazor server-side, select the **ASP.NET Core Blazor (client-side)** template. Select **Next**.

   5.&nbsp;The **Target Framework** defaults to **.NET Core 3.0**. Select **Next**.

   6.&nbsp;In the **Project Name** field, enter `WebApplication1`. Select **Create**.

   7.&nbsp;Select **Run** > **Run Without Debugging** to run the app *without the debugger*. Running with the debugger isn't supported at this time.

   -->

   # <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

   Środowisko pracy z Blazor po stronie klienta wykonaj następujące polecenia z powłoki poleceń:

   ```console
   dotnet new blazor -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   Środowisko pracy z Blazor po stronie serwera wykonaj następujące polecenia z powłoki poleceń:

   ```console
   dotnet new blazorserverside -o WebApplication1
   cd WebApplication1
   dotnet run
   ```

   > [!NOTE]
   > W systemie macOS należy użyć Blazor aplikacji po stronie klienta. Blazor po stronie serwera nie jest obsługiwana dla systemu macOS na platformy ASP.NET Core 3.0 w wersji zapoznawczej 4. Aby uzyskać więcej informacji, zobacz [po stronie serwera Blazor: dotnet, uruchom kończy się niepowodzeniem z InvalidOperationException w systemie MacOS](https://github.com/aspnet/AspNetCore/issues/9402).

   ---

W przeglądarce przejdź do `https://localhost:5001`.

Wiele stron są dostępne na kartach w pasku bocznym:

* Home
* Licznik
* Pobieranie danych

Na stronie licznika wybierz **kliknij mnie** przycisk, aby zwiększyć licznik bez odświeżania strony. Zwiększenie licznika, na stronie sieci Web zwykle wtedy konieczne napisanie kodu JavaScript, ale Razor składniki zapewniają lepsze przy użyciu podejścia C#.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter1.razor)]

Żądanie dotyczące `/counter` w przeglądarce, określony przez `@page` dyrektywy u góry strony, powoduje, że składnik licznika do renderowania jego zawartości. Składniki renderowania do reprezentacji w pamięci drzewa renderowania, który następnie może służyć do aktualizowania interfejsu użytkownika w elastyczny i efektywny sposób.

Każdorazowo **kliknij mnie** wybrany przycisk:

* `onclick` Jest wyzwalane zdarzenie.
* `IncrementCount` Metoda jest wywoływana.
* `currentCount` Jest zwiększany.
* Składnik jest renderowany ponownie.

Środowisko uruchomieniowe porównuje nową zawartość do poprzedniego zawartości i ma zastosowanie tylko zmiany zawartości do modelu DOM (Document Object).

Dodaj składnik do innego składnika przy użyciu składni notacji HTML. Składnik parametry są określane, za pomocą atrybutów i zawartość elementu podrzędnego. Na przykład można dodać składnik licznika do strony głównej aplikacji, dodając `<Counter />` elementu składnik indeksu.

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index1.razor?highlight=7)]

Uruchom aplikację. Strona główna ma swój własny licznika.

Aby dodać parametr do składnika licznika, należy zaktualizować składnika `@functions` bloku:

* Dodaj właściwość `IncrementAmount` ozdobione `[Parameter]` atrybutu.
* Zmiana `IncrementCount` metodę `IncrementAmount` podczas zwiększenie wartości `currentCount`.

*Pages/Counter.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Counter2.razor?highlight=4-5,9)]

Określ `IncrementAmount` parametru w części głównej `<Counter>` elementu za pomocą atrybutu.

*Pages/Index.razor*:

[!code-cshtml[](get-started/samples_snapshot/3.x/Index2.razor)]

Uruchom aplikację. Strona główna ma swój własny licznik, który zwiększa przez dziesięć każdorazowo **kliknij mnie** przycisk jest zaznaczony.

## <a name="next-steps"></a>Następne kroki

<xref:tutorials/first-blazor-app>

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:signalr/introduction>
