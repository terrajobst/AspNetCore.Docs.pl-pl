---
title: Wprowadzenie do Blazor
author: guardrex
description: Zapoznaj się z platformy ASP.NET Core Blazor, nowy sposób twórz interaktywne aplikacje po stronie klienta przy użyciu platformy .NET, które są uruchamiane w przeglądarce z format WebAssembly.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/11/2019
uid: spa/blazor/index
ms.openlocfilehash: 0d22365701a4fc1857582c13459280e50d59c858
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159628"
---
# <a name="introduction-to-blazor"></a>Wprowadzenie do Blazor

Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Blazor to aplikacja jednostronicowa umożliwiająca tworzenie interaktywnych aplikacji sieci Web po stronie klienta przy użyciu platformy .NET. Blazor używa standardów sieci web Otwórz bez transpilation wtyczki lub kodu. Blazor działa we wszystkich przeglądarkach nowoczesne rozwiązania sieci web, w tym przeglądarki dla urządzeń przenośnych.

Przy użyciu platformy .NET w przeglądarce do tworzenia aplikacji internetowych po stronie klienta ma wiele zalet:

* **C#język**: Pisanie kodu w C# zamiast JavaScript.
* **Ekosystemu .NET**: Wykorzystaj istniejące ekosystemu bibliotek platformy .NET.
* **Tworzenie pełnych**: Udostępnij logiki po stronie klienta i serwera.
* **Szybkość i skalowalność**: Sieć została stworzona z myślą wydajność, niezawodność i bezpieczeństwo.
* **Wiodące w branży narzędzi**: Utrzymanie produktywności przy użyciu programu Visual Studio w Windows, Linux i macOS.
* **Stabilność i spójność**:  Twórz commonset liczby języków, struktur i narzędzi, które są stabilne, bogate i łatwy w użyciu.

Uruchamianie kodu platformy .NET w przeglądarkach sieci web jest możliwe, [format WebAssembly](http://webassembly.org) (skrócona *wasm*). Format WebAssembly to open sieci web, standard i obsługiwane w przeglądarkach sieci web bez wtyczek. Format WebAssembly to format compact kodu bajtowego zoptymalizowane pod kątem szybkiego pobierania i wykonywania maksymalną szybkość.

Format WebAssembly kod może uzyskać dostęp do pełnej funkcjonalności przeglądarki za pomocą międzyoperacyjności języka JavaScript. W tym samym czasie kodu platformy .NET, wykonywane przy użyciu format WebAssembly działa w tym samym piaskownicy zaufanych jako język JavaScript, aby zapobiec złośliwe akcje na komputerze klienckim.

![Blazor uruchamia kod .NET w przeglądarce z format WebAssembly.](index/_static/blazor.png)

Przy aplikacji Blazor jest wbudowana i uruchomić w przeglądarce:

* C#pliki kodu i Razor są kompilowane do zestawów platformy .NET.
* Zestawy i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.
* Blazor używa do ładowania środowiska uruchomieniowego .NET i konfiguruje środowiska uruchomieniowego należy załadować zestawy dla aplikacji. Wywołania dokumentu Object Model (DOM) manipulacji i przeglądarka interfejsu API są obsługiwane przez środowisko uruchomieniowe Blazor za pomocą międzyoperacyjności języka JavaScript.

Blazor obsługuje urządzenia podstawowe wymagane przez większość aplikacji, w tym:

* Parametry
* Obsługa zdarzeń
* Powiązanie danych
* Routing
* Wstrzykiwanie zależności
* Układy
* Tworzenie szablonów
* Kaskadowe wartości

Aby zmniejszyć rozmiar aplikacji pobranych nieużywany kod usunięte z aplikacji publikowanych przez [konsolidatora języka pośredniego (IL)](xref:host-and-deploy/razor-components/configure-linker).

Blazor jest modelu hostingu po stronie klienta dla składników Razor. Ponieważ składniki Razor oddzielenie logiki renderowania składnika w jaki sposób są stosowane aktualizacje interfejsu użytkownika, w jak Razor składniki mogą być hostowane jest elastyczność. Umożliwia składniki programu ASP.NET Core Razor hosta Razor składników w aplikacji ASP.NET Core na serwerze gdzie aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem połączenia SignalR. Aby uzyskać więcej informacji, zobacz <xref:razor-components/hosting-models#server-side-hosting-model>. 

## <a name="components"></a>Składniki

A *składnika Razor* jest element interfejsu użytkownika, takich jak strony, okno dialogowe lub dane formularza zgłoszenia. Składniki obsługi zdarzeń użytkownika i definiowanie elastyczne logika renderowania interfejsu użytkownika. Składniki może zagnieżdżone i ponownie używane.

Składniki są klasy .NET, wbudowane zestawy .NET, które mogą być udostępniane i dystrybuowanych jako pakiety NuGet. Klasa, albo mogą być napisane w postaci znaczników strony Razor (*.cshtml*) lub jako C# klasy (*.cs*).

[Razor](xref:mvc/views/razor) używa składni łączenia kod znaczników HTML za pomocą C# kodu. Razor jest przeznaczony do pracy deweloperskiej, dzięki czemu dla deweloperów przełączać się między znaczników i C# w tym samym pliku z [IntelliSense](/visualstudio/ide/using-intellisense) pomocy technicznej. Strony razor i MVC widoki również używają Razor. W przeciwieństwie do stron Razor i widoków MVC, które są zbudowane wokół modelu żądań/odpowiedzi, składniki są używane dla obsługi kompozycji interfejsu użytkownika. Składniki razor mogą służyć specjalnie dla logika interfejsu użytkownika po stronie klienta i kompozycji.

Następujący kod jest przykładem składnika niestandardowy dialog w pliku Razor (*DialogComponent.cshtml*):

```cshtml
<div>
    <h2>@Title</h2>
    @BodyContent
    <button onclick=@OnOK>OK</button>
</div>

@functions {
    public string Title { get; set; }
    public RenderFragment BodyContent { get; set; }
    public Action OnOK { get; set; }
}
```

Gdy ten składnik jest używany w innym miejscu w aplikacji, funkcja IntelliSense przyspiesza tworzenie aplikacji przy użyciu uzupełnianie składni i parametrów.

Składniki renderowania do reprezentacji w pamięci w przeglądarce modelu DOM o nazwie *renderowania drzewa* , następnie można zaktualizować interfejs użytkownika w sposób, elastyczna i wydajna.

## <a name="javascript-interop"></a>Międzyoperacyjność w języku JavaScript

W przypadku aplikacji wymagających bibliotek JavaScript innych firm i przeglądarka interfejsów API Blazor współdziała z użyciem języka JavaScript. Składniki są w stanie korzystającego z dowolnej biblioteki lub interfejsu API języka JavaScript nie może korzystać. C#Kod może wywołać kod JavaScript i kodu w języku JavaScript można wywoływać C# kodu. Aby uzyskać więcej informacji, zobacz [międzyoperacyjnego JavaScript](xref:razor-components/javascript-interop).

## <a name="code-sharing-and-net-standard"></a>Udostępnianie kodu i .NET Standard

Aplikacje można odwołać się i używać istniejącej [.NET Standard](/dotnet/standard/net-standard) bibliotek. .NET standard to formalną specyfikację interfejsów API platformy .NET, które są wspólne dla implementacji platformy .NET. .NET standard 2.0 lub nowszej jest obsługiwana. Interfejsy API, które nie są stosowane w przeglądarce sieci web (na przykład dostęp do systemu plików, gniazdo, wątki i inne funkcje) throw <xref:System.PlatformNotSupportedException>. Biblioteki klas .NET standard mogą być udostępniane w kodzie serwera oraz w aplikacjach opartych na przeglądarce.

## <a name="optimization"></a>Optymalizacja

Rozmiar ładunku jest czynnikiem wydajność krytycznych dla użyteczności aplikacji. Blazor optymalizuje rozmiar ładunku, aby zmniejszyć czas pobierania:

* Nieużywane części zestawów .NET są usuwane podczas procesu kompilacji.
* Odpowiedzi HTTP są kompresowane.
* Środowisko uruchomieniowe platformy .NET i zestawy są buforowane w przeglądarce.

[Składniki Razor serwerowe](xref:razor-components/index) zapewnia jeszcze mniejszy rozmiar ładunku niż Blazor przy zachowaniu zestawy .NET, zestawu aplikacji i po stronie serwera środowisko uruchomieniowe. Razor składniki aplikacji służyć tylko plikami znaczników i statyczne elementy zawartości do klientów.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:razor-components/index>
* [Format WebAssembly](http://webassembly.org/)
* [Przewodnik dla języka C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
