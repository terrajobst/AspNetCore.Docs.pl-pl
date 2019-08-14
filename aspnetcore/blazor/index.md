---
title: Wprowadzenie do Blazor w ASP.NET Core
author: guardrex
description: Poznaj ASP.NET Core Blazor, aby utworzyć interaktywny interfejs użytkownika sieci Web po stronie klienta przy użyciu platformy .NET w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 08/13/2019
uid: blazor/index
ms.openlocfilehash: b13446651603fe23c4595028272ba19ed7bbd5fd
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68993381"
---
# <a name="introduction-to-blazor"></a>Wprowadzenie do Blazor

Autorzy [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

*Witamy w Blazor!*

Blazor to struktura służąca do kompilowania interakcyjnego interfejsu użytkownika sieci Web po stronie klienta przy użyciu platformy .NET:

* Utwórz rozbudowane interaktywne C# interfejsów użytkownika przy użyciu zamiast języka JavaScript.
* Udostępnianie logiki aplikacji po stronie serwera i klienta zapisaną w środowisku .NET.
* Renderuj interfejs użytkownika jako HTML i CSS, aby obsługiwał szeroką przeglądarkę, w tym przeglądarki dla urządzeń przenośnych.

Korzystanie z programu .NET do tworzenia aplikacji sieci Web po stronie klienta zapewnia następujące korzyści:

* Napisz kod C# zamiast języka JavaScript.
* Skorzystaj z istniejącego ekosystemu .NET bibliotek platformy .NET.
* Udostępnianie logiki aplikacji między serwerem i klientem.
* Skorzystaj z programu. Wydajność, niezawodność i bezpieczeństwo sieci.
* Bądź na bieżąco z programem Visual Studio w systemach Windows, Linux i macOS.
* Twórz w oparciu o wspólny zestaw języków, struktur i narzędzi, które są stabilne, bogate w funkcje i łatwe w użyciu.

## <a name="components"></a>Składniki

Aplikacje Blazor są oparte na *składnikach*. Składnik w Blazor jest elementem interfejsu użytkownika, takim jak strona, okno dialogowe lub formularz wprowadzania danych.

Składniki to klasy .NET wbudowane w zestawy .NET, które:

* Definiowanie elastycznej logiki renderowania interfejsu użytkownika.
* Obsługa zdarzeń użytkownika.
* Mogą być zagnieżdżane i ponownie używane.
* Mogą być udostępniane i dystrybuowane jako [biblioteki klas Razor](xref:razor-pages/ui-class) lub [pakiety NuGet](/nuget/what-is-nuget).

Klasa składnika jest zwykle zapisywana w formie strony znaczników [Razor](xref:mvc/views/razor) z rozszerzeniem pliku *Razor* . Składniki w Blazor są formalnie określane jako *składniki Razor*. Razor to Składnia służąca do łączenia znaczników HTML C# z kodem zaprojektowanym na potrzeby produktywności dla deweloperów. Razor pozwala przełączać się między znakami C# HTML i w tym samym pliku z obsługą [IntelliSense](/visualstudio/ide/using-intellisense) . Razor Pages i MVC również używają Razor. W przeciwieństwie do Razor Pages i MVC, które są zbudowane wokół modelu żądania/odpowiedzi, składniki są używane specjalnie dla logiki interfejsu użytkownika po stronie klienta.

Poniższy znacznik Razor ilustruje składnik (*dialog. Razor*), który może być zagnieżdżony w innym składniku:

```cshtml
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    public string Title { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#! 'Yes' button was selected.");
    }
}
```

Zawartość (`ChildContent`) i tytuł (`Title`) okna dialogowego są udostępniane przez składnik, który używa tego składnika w interfejsie użytkownika. `OnYes`jest C# metodą wyzwalaną przez `onclick` zdarzenie przycisku.

Blazor używa naturalnych tagów HTML dla kompozycji interfejsu użytkownika. Elementy HTML określają składniki, a atrybuty znacznika przechodzą wartości do właściwości składnika.

W poniższym przykładzie `Index` składnik `Dialog` używa składnika. `ChildContent`i `Title` są ustawiane przez atrybuty i zawartość `<Dialog>` elementu.

*Index. Razor*:

```cshtml
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

Okno dialogowe jest renderowane po uzyskaniu dostępu do elementu nadrzędnego (*index. Razor*) w przeglądarce:

![Składnik okna dialogowego renderowany w przeglądarce](index/_static/dialog.png)

Gdy ten składnik jest używany w aplikacji, funkcja IntelliSense w programie [Visual Studio](/visualstudio/ide/using-intellisense) i [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) przyspiesza Programowanie przy użyciu składni i uzupełniania parametrów.

Składniki są renderowane w pamięci podręcznej Document Object Model (DOM) przeglądarki o nazwie *drzewo renderowania*, która jest używana do aktualizowania interfejsu użytkownika w elastyczny i wydajny sposób.

## <a name="blazor-client-side"></a>Blazor po stronie klienta

Blazor po stronie klienta jest platformą aplikacji jednostronicowej do tworzenia interaktywnych aplikacji sieci Web po stronie klienta przy użyciu platformy .NET. Blazor po stronie klienta używa otwartych standardów sieci Web bez wtyczek lub kodu transpilation i działa we wszystkich nowoczesnych przeglądarkach sieci Web, w tym w przeglądarkach dla urządzeń przenośnych.

Uruchamianie kodu platformy .NET wewnątrz przeglądarek sieci Web jest możliwe przez [zestaw webassembly](https://webassembly.org) (skrócony *wasm*). Webassembly to kompaktowy format kodu bajtowego zoptymalizowany pod kątem szybkiego pobierania i maksymalnej szybkości wykonywania. Webassembly to otwarty standard sieci Web, który jest obsługiwany w przeglądarkach sieci Web bez wtyczek.

Kod webassembly może uzyskać dostęp do pełnej funkcjonalności przeglądarki za pośrednictwem języka JavaScript, nazywanego współdziałaniem *JavaScript* (lub międzyoperacyjną *JavaScript*). Kod .NET wykonywany za pośrednictwem webassembly w przeglądarce jest uruchamiany w piaskownicy języka JavaScript przeglądarki z ochroną, którą piaskownica zapewnia przed złośliwymi działaniami na komputerze klienckim.

![Blazor kod .NET uruchamiany po stronie klienta w przeglądarce z zestawem webassembly.](index/_static/blazor-client-side.png)

Po skompilowaniu i uruchomieniu aplikacji po stronie klienta Blazor w przeglądarce:

* C#pliki kodu i pliki Razor są kompilowane do zestawów .NET.
* Zestawy i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.
* Blazor po stronie klienta środowisko uruchomieniowe platformy .NET i konfiguruje środowisko uruchomieniowe w celu załadowania zestawów dla aplikacji. Blazor środowisko uruchomieniowe po stronie klienta używa kodu JavaScript Interop do obsługi manipulowania DOM i wywołań interfejsu API przeglądarki.

Rozmiar opublikowanej aplikacji, jej *rozmiaru ładunku*, jest krytycznym czynnikiem wydajności dla useability aplikacji. Pobieranie dużej aplikacji do przeglądarki zajmuje stosunkowo dużo czasu, co zmniejsza środowisko użytkownika. Blazor po stronie klienta optymalizuje rozmiar ładunku, aby skrócić czas pobierania:

* Nieużywany kod jest usuwany z aplikacji, gdy jest publikowany przez [konsolidator języka pośredniego (IL)](xref:host-and-deploy/blazor/configure-linker).
* Odpowiedzi HTTP są kompresowane.
* Środowisko uruchomieniowe platformy .NET i zestawy są buforowane w przeglądarce.

## <a name="blazor-server-side"></a>Blazor po stronie serwera

Blazor oddziela logikę renderowania składników od sposobu stosowania aktualizacji interfejsu użytkownika. Po stronie serwera Blazor zapewnia obsługę hostowania składników Razor na serwerze w aplikacji ASP.NET Core. Aktualizacje interfejsu użytkownika są obsługiwane przez [](xref:signalr/introduction) połączenie sygnalizujące.

Środowisko uruchomieniowe obsługuje wysyłanie zdarzeń interfejsu użytkownika z przeglądarki do serwera i zastosowanie aktualizacji interfejsu użytkownika wysyłanych przez serwer z powrotem do przeglądarki po uruchomieniu składników programu.

Połączenie używane przez serwer Blazor do komunikacji z przeglądarką jest również używane do obsługi wywołań międzyoperacyjnych języka JavaScript.

![Serwer Blazor uruchamia kod .NET na serwerze i współdziała z Document Object Model na kliencie za pośrednictwem połączenia sygnalizującego](index/_static/blazor-server-side.png)

## <a name="javascript-interop"></a>Międzyoperacyjność w języku JavaScript

W przypadku aplikacji, które wymagają bibliotek JavaScript innych firm i dostępu do interfejsów API przeglądarki, składniki współdziałają z JavaScript. Składniki mogą korzystać z dowolnej biblioteki lub interfejsu API, który może być używany przez język JavaScript. C#kod może wywołać kod JavaScript, a kod JavaScript może wywołać C# kod. Aby uzyskać więcej informacji, zobacz <xref:blazor/javascript-interop>.

## <a name="code-sharing-and-net-standard"></a>Udostępnianie kodu i .NET Standard

Blazor implementuje [.NET Standard 2,0](/dotnet/standard/net-standard). .NET Standard jest formalną specyfikacją interfejsów API platformy .NET, które są wspólne dla implementacji platformy .NET. Biblioteki klas .NET Standard mogą być współużytkowane przez różne platformy .NET, takie jak Blazor, .NET Framework, .NET Core, Xamarin, mono i Unity.

Interfejsy API, które nie są stosowane w przeglądarce sieci Web (na przykład dostęp do systemu plików, otwieranie gniazda i wątkowość) throw <xref:System.PlatformNotSupportedException>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Zestaw webassembly](https://webassembly.org/)
* <xref:blazor/hosting-models>
* [Przewodnik dla języka C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
