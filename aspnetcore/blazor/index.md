---
title: Wprowadzenie do Blazor w programie ASP.NET Core
author: guardrex
description: Zapoznaj się z platformy ASP.NET Core Blazor, umożliwia tworzenie interaktywnych po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/18/2019
uid: blazor/index
ms.openlocfilehash: 74eeb003c249fc9ff8267ac855455f875806ccd9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983000"
---
# <a name="introduction-to-blazor"></a>Wprowadzenie do Blazor

Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

Blazor — Zapraszamy!

Twórz interaktywne po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET:

* Tworzyć zaawansowane interaktywne UI za pomocą C# zamiast JavaScript.
* Udostępnij logiki po stronie serwera i klienta aplikacji napisanych przy użyciu platformy .NET.
* Renderuj interfejsie użytkownika jako HTML i CSS obsługi szerokiego przeglądarki, w tym przeglądarki dla urządzeń przenośnych.

Blazor obsługuje podstawowe scenariusze wymagane przez większość aplikacji:

* Parametry
* Obsługa zdarzeń
* Powiązanie danych
* Routing
* Wstrzykiwanie zależności
* Układy
* Szablony
* Kaskadowe wartości

## <a name="components"></a>Składniki

A *składnika* Blazor jest element interfejsu użytkownika, takich jak strony, okno dialogowe lub dane formularza zgłoszenia. Składniki obsługi zdarzeń użytkownika i definiowanie elastyczne logika renderowania interfejsu użytkownika. Składniki może zagnieżdżone i ponownie używane.

Składniki są klasy .NET, wbudowane zestawy .NET, które mogą być udostępniane i dystrybuowanych jako pakiety NuGet. Klasa składników są zwykle zapisywane w postaci znaczników stronę Razor za pomocą *.razor* rozszerzenie pliku. Składniki Blazor są czasami określane jako składniki Razor.

[Razor](xref:mvc/views/razor) używa składni łączenia kod znaczników HTML za pomocą C# kodu. Razor jest przeznaczony do pracy deweloperskiej, dzięki czemu dla deweloperów przełączać się między znaczników i C# w tym samym pliku z [IntelliSense](/visualstudio/ide/using-intellisense) pomocy technicznej. Strony razor i MVC widoki również używają Razor. W przeciwieństwie do stron Razor i widoków MVC, które są zbudowane wokół modelu żądań/odpowiedzi, składniki są używane dla obsługi kompozycji interfejsu użytkownika. Składniki razor mogą służyć specjalnie dla logika interfejsu użytkownika po stronie klienta i kompozycji.

Następujący kod jest przykładem składnika niestandardowe okno dialogowe:

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick="@OnOK">OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

Podczas tego składnika jest używany w aplikacji, funkcji IntelliSense w [programu Visual Studio](https://visualstudio.microsoft.com/vs/) przyspiesza tworzenie aplikacji przy użyciu uzupełnianie składni i parametrów.

Składniki renderowania do reprezentacji w pamięci w przeglądarce modelu DOM o nazwie *renderowania drzewa* , następnie można zaktualizować interfejs użytkownika w sposób, elastyczna i wydajna.

## <a name="blazor-server-side"></a>Blazor po stronie serwera

Blazor oddziela składnika renderowania logiki sposób stosowania aktualizacje interfejsu użytkownika. Po stronie serwera Blazor zapewnia obsługę hostującego składniki Razor w aplikacji ASP.NET Core na serwerze. Aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem połączenia SignalR.

Środowisko uruchomieniowe:

* Obsługuje wysyłanie zdarzeń interfejsu użytkownika z przeglądarki do serwera.
* Zastosowanie interfejsu użytkownika aktualizacji wysłanych przez serwer z powrotem do przeglądarki, po uruchomieniu składników.

Połączenie używane przez Blazor po stronie serwera do komunikacji za pośrednictwem przeglądarki umożliwia również obsługę wywołań międzyoperacyjnych w języku JavaScript.

![Po stronie serwera Blazor uruchamiania kodu platformy .NET na serwerze i wchodzi w interakcję z Document Object Model na kliencie za pośrednictwem połączenia SignalR](index/_static/blazor-server-side.png)

Aby uzyskać więcej informacji, zobacz <xref:blazor/hosting-models#server-side>.

## <a name="blazor-client-side"></a>Blazor po stronie klienta

Blazor po stronie klienta jest aplikacja jednostronicowa umożliwiająca tworzenie interaktywnych aplikacji sieci Web po stronie klienta przy użyciu platformy .NET. Blazor po stronie klienta używa standardów sieci web Otwórz bez transpilation wtyczki lub kodu. Blazor po stronie klienta działa we wszystkich przeglądarkach nowoczesne rozwiązania sieci web, w tym przeglądarki dla urządzeń przenośnych.

Przy użyciu platformy .NET w przeglądarce do tworzenia aplikacji internetowych po stronie klienta ma wiele zalet:

* **C#język**: Pisanie kodu w C# zamiast JavaScript.
* **Ekosystemu .NET**: Wykorzystaj istniejące ekosystemu bibliotek platformy .NET.
* **Tworzenie pełnych**: Udostępnij logiki po stronie klienta i serwera.
* **Szybkość i skalowalność**: .NET została skompilowana wydajność, niezawodność i bezpieczeństwo.
* **Wiodące w branży narzędzi**: Utrzymanie produktywności przy użyciu programu Visual Studio w Windows, Linux i macOS.
* **Stabilność i spójność**:  Twórz wspólny zbiór języków, struktur i narzędzi, które są stabilne, bogate i łatwy w użyciu.

Uruchamianie kodu platformy .NET w przeglądarkach sieci web jest możliwe, [format WebAssembly](http://webassembly.org) (skrócona *wasm*). Format WebAssembly to open sieci web, standard i obsługiwane w przeglądarkach sieci web bez wtyczek. Format WebAssembly to format compact kodu bajtowego zoptymalizowane pod kątem szybkiego pobierania i wykonywania maksymalną szybkość.

Format WebAssembly kod może uzyskać dostęp do pełnej funkcjonalności przeglądarki za pomocą międzyoperacyjności języka JavaScript. W tym samym czasie kodu platformy .NET, wykonywane przy użyciu format WebAssembly działa w tym samym piaskownicy zaufanych jako język JavaScript, aby zapobiec złośliwe akcje na komputerze klienckim.

![Blazor po stronie klienta działa kod .NET w przeglądarce z format WebAssembly.](index/_static/blazor-client-side.png)

Przy aplikacji po stronie klienta Blazor jest wbudowana i uruchomić w przeglądarce:

* C#pliki kodu i Razor są kompilowane do zestawów platformy .NET.
* Zestawy i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.
* Blazor po stronie klienta używa do ładowania środowiska uruchomieniowego .NET i konfiguruje środowiska uruchomieniowego należy załadować zestawy dla aplikacji. Wywołania dokumentu Object Model (DOM) manipulacji i przeglądarka interfejsu API są obsługiwane przez środowisko uruchomieniowe po stronie klienta Blazor za pomocą międzyoperacyjności języka JavaScript.

Aby zmniejszyć rozmiar pobranej aplikacji, nieużywany kod jest usuwany poza aplikacją, gdy zostanie opublikowany przez [konsolidatora języka pośredniego (IL)](xref:host-and-deploy/blazor/configure-linker).

Blazor po stronie klienta jest modelu hostingu po stronie klienta. Ponieważ Blazor oddziela składnika renderowania logiki, w jaki sposób są stosowane aktualizacje interfejsu użytkownika, w jaki może być hostowana Blazor jest elastyczność. Użyj [Blazor po stronie serwera](#blazor-server-side) hosta Blazor na serwerze w aplikacji ASP.NET Core, w którym aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem połączenia SignalR. Aby uzyskać więcej informacji, zobacz <xref:blazor/hosting-models#server-side>. 

Rozmiar ładunku jest czynnikiem wydajność krytycznych dla użyteczności aplikacji. Po stronie klienta Blazor optymalizuje rozmiar ładunku, aby zmniejszyć czas pobierania:

* Nieużywane części zestawów .NET są usuwane podczas procesu kompilacji.
* Odpowiedzi HTTP są kompresowane.
* Środowisko uruchomieniowe platformy .NET i zestawy są buforowane w przeglądarce.

[Po stronie serwera Blazor](#blazor-server-side) zapewnia mniejszy rozmiar ładunku niż Blazor po stronie klienta przy zachowaniu zestawy .NET, zestawu aplikacji i po stronie serwera środowisko uruchomieniowe. Blazor po stronie serwera aplikacji służyć tylko pliki znaczników i statyczne elementy zawartości do klientów.

## <a name="javascript-interop"></a>Międzyoperacyjność w języku JavaScript

W przypadku aplikacji wymagających bibliotek JavaScript innych firm i przeglądarka interfejsów API składniki współdziałać z użyciem języka JavaScript. Składniki są w stanie korzystającego z dowolnej biblioteki lub interfejsu API języka JavaScript nie może korzystać. C#Kod może wywołać kod JavaScript i kodu w języku JavaScript można wywoływać C# kodu. Aby uzyskać więcej informacji, zobacz [międzyoperacyjnego JavaScript](xref:blazor/javascript-interop).

## <a name="code-sharing-and-net-standard"></a>Udostępnianie kodu i .NET Standard

Aplikacje można odwołać się i używać istniejącej [.NET Standard](/dotnet/standard/net-standard) bibliotek. .NET standard to formalną specyfikację interfejsów API platformy .NET, które są wspólne dla implementacji platformy .NET. Blazor implementuje .NET Standard 2.0. Interfejsy API, które nie są stosowane w przeglądarce sieci web (na przykład dostęp do systemu plików, gniazdo, wątki i inne funkcje) throw <xref:System.PlatformNotSupportedException>. Biblioteki klas .NET standard mogą być udostępniane na różnych platformach .NET, takich jak Blazor, .NET Framework, .NET Core, Xamarin, narzędzie Mono i Unity.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Format WebAssembly](http://webassembly.org/)
* [Przewodnik dla języka C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
