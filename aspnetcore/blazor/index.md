---
title: Wprowadzenie do Blazor w programie ASP.NET Core
author: guardrex
description: Zapoznaj się z platformy ASP.NET Core Blazor, umożliwia tworzenie interaktywnych po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 05/01/2019
uid: blazor/index
ms.openlocfilehash: d58115b17536cad0b3927e6d32b7dbe8db8e4b0f
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034423"
---
# <a name="introduction-to-blazor"></a>Wprowadzenie do Blazor

Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

*Blazor — Zapraszamy!*

Blazor to architektura służąca do tworzenia interaktywnego po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET:

* Tworzyć zaawansowane interaktywne interfejsy, za pomocą C# zamiast JavaScript.
* Udostępnij logiki po stronie serwera i klienta aplikacji napisanych przy użyciu platformy .NET.
* Renderuj interfejsie użytkownika jako HTML i CSS obsługi szerokiego przeglądarki, w tym przeglądarki dla urządzeń przenośnych.

Do tworzenia aplikacji internetowych po stronie klienta przy użyciu platformy .NET ma następujące zalety:

* Pisanie kodu w C# zamiast JavaScript.
* Wykorzystaj istniejące ekosystemu .NET bibliotek platformy .NET.
* Logika aplikacji może być współużytkowane przez serwer i klienta.
* Korzystaj z. Wydajność, niezawodność i bezpieczeństwo przez sieć.
* Utrzymanie produktywności przy użyciu programu Visual Studio w Windows, Linux i macOS.
* Twórz wspólny zbiór języków, struktur i narzędzi, które są stabilne, bogate i łatwy w użyciu.

## <a name="components"></a>Składniki

Aplikacje Blazor opierają się na *składniki*. Składnik Blazor jest element interfejsu użytkownika, takich jak strony, okno dialogowe lub dane formularza zgłoszenia. Składniki obsługi zdarzeń użytkownika i definiowanie elastyczne logika renderowania interfejsu użytkownika. Składniki może zagnieżdżone i ponownie używane.

Składniki są klasy .NET, wbudowane zestawy .NET, które mogą być udostępniane i dystrybuowanych jako [pakiety NuGet](/nuget/what-is-nuget). Klasa składników są zwykle zapisywane w postaci znaczników stronę Razor za pomocą *.razor* rozszerzenie pliku.

Składniki Blazor są czasami określane jako *składniki Razor*. [Razor](xref:mvc/views/razor) używa składni łączenia kod znaczników HTML za pomocą C# kodu zapewniające produktywność deweloperów. Razor pozwala na przełączanie między kod znaczników HTML i C# w tym samym pliku z [IntelliSense](/visualstudio/ide/using-intellisense) pomocy technicznej. Strony razor i MVC również używają Razor. W przeciwieństwie do stron Razor i MVC, które są zbudowane wokół modelu żądań/odpowiedzi, składniki są używane dla logika interfejsu użytkownika po stronie klienta i kompozycji.

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
        Console.WriteLine("Write to the console in C#!");
    }
}
```

Okno dialogowe treść (`ChildContent`) i tytuł (`Title`) są dostarczane przez składnik, który używa tego składnika w jego interfejsie użytkownika. `OnYes` jest C# metoda wyzwalane za pomocą przycisku `onclick` zdarzeń.

Blazor używa naturalnych znaczników HTML dla kompozycji interfejsu użytkownika. Elementy HTML określ składniki, a atrybuty znacznika przekazania wartości do właściwości tego składnika. `ChildContent` i `Title` ustawionych przez składnik, który używa składnika okna dialogowego (*Index.razor*):

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

Składniki renderowania do reprezentacji w pamięci w przeglądarce modelu DOM o nazwie *renderowania drzewa* służący do aktualizowania interfejsu użytkownika w elastyczny i efektywny sposób.

## <a name="blazor-client-side"></a>Blazor po stronie klienta

Blazor po stronie klienta jest aplikacja jednostronicowa umożliwiająca tworzenie aplikacji sieci web po stronie klienta interactive na platformie .NET. Blazor po stronie klienta używa standardów sieci web Otwórz bez transpilation wtyczki lub kodu i działa we wszystkich przeglądarkach nowoczesne rozwiązania sieci web, w tym przeglądarki dla urządzeń przenośnych.

Uruchamianie kodu platformy .NET w przeglądarkach sieci web jest możliwe, [format WebAssembly](http://webassembly.org) (skrócona *wasm*). Format WebAssembly to open sieci web, standard i obsługiwane w przeglądarkach sieci web bez wtyczek. Format WebAssembly to format compact kodu bajtowego zoptymalizowane pod kątem szybkiego pobierania i wykonywania maksymalną szybkość.

Format WebAssembly kod może uzyskiwać dostęp do pełnej funkcjonalności przeglądarki przy użyciu języka JavaScript, o nazwie *współdziałanie JavaScript* (lub *międzyoperacyjnego JavaScript*). Kod platformy .NET, wykonywane przy użyciu format WebAssembly w przeglądarce jest uruchamiany w tym samym piaskownicy zaufanych jako kodu JavaScript, który wirtualnie eliminuje możliwości dla aplikacji w celu wykonania złośliwych akcji na komputerze klienckim.

![Blazor po stronie klienta działa kod .NET w przeglądarce z format WebAssembly.](index/_static/blazor-client-side.png)

Przy aplikacji po stronie klienta Blazor jest wbudowana i uruchomić w przeglądarce:

* C#pliki kodu i Razor są kompilowane do zestawów platformy .NET.
* Zestawy i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.
* Blazor po stronie klienta używa do ładowania środowiska uruchomieniowego .NET i konfiguruje środowiska uruchomieniowego należy załadować zestawy dla aplikacji. Środowisko uruchomieniowe Blazor po stronie klienta używa międzyoperacyjnego JavaScript do obsługi wywołań interfejsu API manipulacji i przeglądarka modelu DOM (Document Object).

Rozmiar opublikowanej aplikacji, jego *rozmiar ładunku*, jest czynnikiem wydajność krytycznych dla użyteczności aplikacji. Aplikacja dużych względnie długi czas pobierania do przeglądarki, która dobrze jest środowisko użytkownika. Po stronie klienta Blazor optymalizuje rozmiar ładunku, aby zmniejszyć czas pobierania:

* Nieużywany kod jest usunięte z aplikacji, gdy zostanie opublikowany przez [konsolidatora języka pośredniego (IL)](xref:host-and-deploy/blazor/configure-linker).
* Odpowiedzi HTTP są kompresowane.
* Środowisko uruchomieniowe platformy .NET i zestawy są buforowane w przeglądarce.

Aby uzyskać więcej informacji i wskazówek na temat wybierania modelu hostingu, zobacz <xref:blazor/hosting-models>.

## <a name="blazor-server-side"></a>Blazor po stronie serwera

Blazor oddziela składnika renderowania logiki sposób stosowania aktualizacje interfejsu użytkownika. Po stronie serwera Blazor zapewnia obsługę hostującego składniki Razor w aplikacji ASP.NET Core na serwerze. Aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem [SignalR](xref:signalr/introduction) połączenia.

Środowisko wykonawcze obsługuje wysyłanie zdarzeń interfejsu użytkownika z przeglądarki do serwera i stosuje aktualizacje interfejsu użytkownika wysyłane przez serwer z powrotem do przeglądarki po uruchomieniu składników.

Połączenie używane przez Blazor po stronie serwera do komunikacji za pośrednictwem przeglądarki umożliwia również obsługę wywołań międzyoperacyjnych w języku JavaScript.

![Po stronie serwera Blazor uruchamiania kodu platformy .NET na serwerze i wchodzi w interakcję z Document Object Model na kliencie za pośrednictwem połączenia SignalR](index/_static/blazor-server-side.png)

Aby uzyskać więcej informacji i wskazówek na temat wybierania modelu hostingu, zobacz <xref:blazor/hosting-models>.

## <a name="javascript-interop"></a>Międzyoperacyjność w języku JavaScript

W przypadku aplikacji wymagających bibliotek JavaScript innych firm i przeglądarka interfejsów API składniki współdziałać z użyciem języka JavaScript. Składniki są w stanie korzystającego z dowolnej biblioteki lub interfejsu API języka JavaScript nie może korzystać. C#Kod może wywołać kod JavaScript i kodu w języku JavaScript można wywoływać C# kodu. Aby uzyskać więcej informacji, zobacz <xref:blazor/javascript-interop>.

## <a name="code-sharing-and-net-standard"></a>Udostępnianie kodu i .NET Standard

Implementuje Blazor [.NET Standard 2.0](/dotnet/standard/net-standard). .NET standard to formalną specyfikację interfejsów API platformy .NET, które są wspólne dla implementacji platformy .NET. Biblioteki klas .NET standard mogą być udostępniane na różnych platformach .NET, takich jak Blazor, .NET Framework, .NET Core, Xamarin, narzędzie Mono i Unity.

Throw interfejsów API, które nie są stosowane w przeglądarce sieci web (na przykład dostęp do systemu plików, gniazdo i wątkowość) <xref:System.PlatformNotSupportedException>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Format WebAssembly](http://webassembly.org/)
* [Przewodnik dla języka C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
