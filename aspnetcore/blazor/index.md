---
title: Wprowadzenie do Blazor w programie ASP.NET Core
author: guardrex
description: Zapoznaj się z platformy ASP.NET Core Blazor, umożliwia tworzenie interaktywnych po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 07/01/2019
uid: blazor/index
ms.openlocfilehash: e0af0f27d79973f10493251c3f6c6daebe1b99a8
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855770"
---
# <a name="introduction-to-blazor"></a>Wprowadzenie do Blazor

Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

*Blazor — Zapraszamy!*

Blazor to architektura służąca do tworzenia interaktywnego po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET:

* Tworzyć zaawansowane interaktywne interfejsy, za pomocą C# zamiast JavaScript.
* Udostępnij logiki aplikacji po stronie serwera i klienta, napisane w języku .NET.
* Renderuj interfejsie użytkownika jako HTML i CSS obsługi szerokiego przeglądarki, w tym przeglądarki dla urządzeń przenośnych.

Do tworzenia aplikacji internetowych po stronie klienta przy użyciu platformy .NET ma następujące zalety:

* Pisanie kodu w C# zamiast JavaScript.
* Wykorzystaj istniejące ekosystemu .NET bibliotek platformy .NET.
* Logika aplikacji może być współużytkowane przez klienta i serwera.
* Korzystaj z. Wydajność, niezawodność i bezpieczeństwo przez sieć.
* Utrzymanie produktywności przy użyciu programu Visual Studio w Windows, Linux i macOS.
* Twórz wspólny zbiór języków, struktur i narzędzi, które są stabilne, bogate i łatwy w użyciu.

## <a name="components"></a>Składniki

Aplikacje Blazor opierają się na *składniki*. Składnik Blazor jest element interfejsu użytkownika, takich jak strony, okno dialogowe lub dane formularza zgłoszenia.

Składniki są klasy .NET, wbudowane zestawy .NET który:

* Zdefiniuj elastyczne logika renderowania interfejsu użytkownika.
* Obsługa zdarzeń użytkownika.
* Zagnieżdżone i ponownie używane.
* Może być udostępniane i dystrybuowanych jako [biblioteki klas Razor](xref:razor-pages/ui-class) lub [pakiety NuGet](/nuget/what-is-nuget).

Klasa składników są zwykle zapisywane w postaci [Razor](xref:mvc/views/razor) znaczników strony *.razor* rozszerzenie pliku. Składniki Blazor formalnie są określane jako *składniki Razor*. Razor jest składni łączenia kod znaczników HTML za pomocą C# kodu zapewniające produktywność deweloperów. Razor pozwala na przełączanie między kod znaczników HTML i C# w tym samym pliku z [IntelliSense](/visualstudio/ide/using-intellisense) pomocy technicznej. Strony razor i MVC również używają Razor. W przeciwieństwie do stron Razor i MVC, które są zbudowane wokół modelu żądań/odpowiedzi, składniki są używane dla logika interfejsu użytkownika po stronie klienta i kompozycji.

Następujący kod Razor pokazuje składnika (*Dialog.razor*), który może być zagnieżdżona w innym składniku:

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="@OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    private string Title { get; set; }

    [Parameter]
    private RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#! 'Yes' button was selected.");
    }
}
```

Okno dialogowe treść (`ChildContent`) i tytuł (`Title`) są dostarczane przez składnik, który używa tego składnika w jego interfejsie użytkownika. `OnYes` jest C# metoda wyzwalane za pomocą przycisku `onclick` zdarzeń.

Blazor używa naturalnych znaczników HTML dla kompozycji interfejsu użytkownika. Elementy HTML określ składniki, a atrybuty znacznika przekazania wartości do właściwości tego składnika.

W poniższym przykładzie `Index` składnik używa `Dialog` składnika. `ChildContent` i `Title` są ustawiane przez atrybuty i zawartości `<Dialog>` elementu.

*Index.razor*:

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

Okno dialogowe jest renderowana, gdy element nadrzędny (*Index.razor*) jest dostępny w przeglądarce:

![Okno dialogowe składnika w przeglądarce](index/_static/dialog.png)

Gdy ten składnik jest używany w aplikacji, funkcji IntelliSense w [programu Visual Studio](/visualstudio/ide/using-intellisense) i [programu Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) przyspiesza tworzenie aplikacji przy użyciu uzupełnianie składni i parametrów.

Składniki renderowania do reprezentacji w pamięci z przeglądarki w modelu DOM (Document Object) o nazwie *renderowania drzewa*, które jest używane do aktualizowania interfejsu użytkownika w sposób, elastyczna i wydajna.

## <a name="blazor-client-side"></a>Blazor po stronie klienta

Blazor po stronie klienta jest aplikacja jednostronicowa umożliwiająca tworzenie aplikacji sieci web po stronie klienta interactive na platformie .NET. Blazor po stronie klienta używa standardów sieci web Otwórz bez transpilation wtyczki lub kodu i działa we wszystkich przeglądarkach nowoczesne rozwiązania sieci web, w tym przeglądarki dla urządzeń przenośnych.

Uruchamianie kodu platformy .NET w przeglądarkach sieci web jest możliwe, [format WebAssembly](https://webassembly.org) (skrócona *wasm*). Format WebAssembly to format compact kodu bajtowego zoptymalizowane pod kątem szybkiego pobierania i wykonywania maksymalną szybkość. Format WebAssembly to open sieci web, standard i obsługiwane w przeglądarkach sieci web bez wtyczek.

Format WebAssembly kod może uzyskiwać dostęp do pełnej funkcjonalności przeglądarki przy użyciu języka JavaScript, o nazwie *współdziałanie JavaScript* (lub *międzyoperacyjnego JavaScript*). Kod .NET, wykonywane przy użyciu format WebAssembly w przeglądarce jest uruchamiany w piaskownicy JavaScript w przeglądarce, za pomocą zabezpieczeń zapewnianych przez piaskownicy przed złośliwymi działaniami na komputerze klienckim.

![Blazor po stronie klienta działa kod .NET w przeglądarce z format WebAssembly.](index/_static/blazor-client-side.png)

Przy aplikacji po stronie klienta Blazor jest wbudowana i uruchomić w przeglądarce:

* C#pliki kodu i Razor są kompilowane do zestawów platformy .NET.
* Zestawy i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.
* Blazor po stronie klienta używa do ładowania środowiska uruchomieniowego .NET i konfiguruje środowiska uruchomieniowego należy załadować zestawy dla aplikacji. Środowisko uruchomieniowe Blazor po stronie klienta używa międzyoperacyjnego JavaScript do obsługi DOM manipulacji i przeglądarki wywołań interfejsu API.

Rozmiar opublikowanej aplikacji, jego *rozmiar ładunku*, jest czynnikiem wydajność krytycznych dla użyteczności aplikacji. Aplikacji sieci dużych względnie długi czas pobierania do przeglądarki, która zmniejsza się środowisko użytkownika. Po stronie klienta Blazor optymalizuje rozmiar ładunku, aby zmniejszyć czas pobierania:

* Nieużywany kod jest usunięte z aplikacji, gdy zostanie opublikowany przez [konsolidatora języka pośredniego (IL)](xref:host-and-deploy/blazor/configure-linker).
* Odpowiedzi HTTP są kompresowane.
* Środowisko uruchomieniowe platformy .NET i zestawy są buforowane w przeglądarce.

## <a name="blazor-server-side"></a>Blazor po stronie serwera

Blazor oddziela składnika renderowania logiki sposób stosowania aktualizacje interfejsu użytkownika. Po stronie serwera Blazor zapewnia obsługę hostującego składniki Razor w aplikacji ASP.NET Core na serwerze. Aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem [SignalR](xref:signalr/introduction) połączenia.

Środowisko wykonawcze obsługuje wysyłanie zdarzeń interfejsu użytkownika z przeglądarki do serwera i stosuje aktualizacje interfejsu użytkownika wysyłane przez serwer z powrotem do przeglądarki po uruchomieniu składników.

Połączenie używane przez Blazor po stronie serwera do komunikacji za pośrednictwem przeglądarki umożliwia również obsługę wywołań międzyoperacyjnych w języku JavaScript.

![Po stronie serwera Blazor uruchamiania kodu platformy .NET na serwerze i wchodzi w interakcję z Document Object Model na kliencie za pośrednictwem połączenia SignalR](index/_static/blazor-server-side.png)

## <a name="javascript-interop"></a>Międzyoperacyjność w języku JavaScript

W przypadku aplikacji wymagających bibliotek JavaScript innych firm i dostępu w przeglądarce interfejsów API składniki współdziałać z użyciem języka JavaScript. Składniki są w stanie korzystającego z dowolnej biblioteki lub interfejsu API języka JavaScript nie może korzystać. C#Kod może wywołać kod JavaScript i kodu w języku JavaScript można wywoływać C# kodu. Aby uzyskać więcej informacji, zobacz <xref:blazor/javascript-interop>.

## <a name="code-sharing-and-net-standard"></a>Udostępnianie kodu i .NET Standard

Implementuje Blazor [.NET Standard 2.0](/dotnet/standard/net-standard). .NET standard to formalną specyfikację interfejsów API platformy .NET, które są wspólne dla implementacji platformy .NET. Biblioteki klas .NET standard mogą być udostępniane na różnych platformach .NET, takich jak Blazor, .NET Framework, .NET Core, Xamarin, narzędzie Mono i Unity.

Throw interfejsów API, które nie są stosowane w przeglądarce sieci web (na przykład dostęp do systemu plików, gniazdo i wątkowość) <xref:System.PlatformNotSupportedException>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Format WebAssembly](https://webassembly.org/)
* <xref:blazor/hosting-models>
* [Przewodnik dla języka C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
