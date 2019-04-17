---
title: Wprowadzenie do Blazor w programie ASP.NET Core
author: guardrex
description: Zapoznaj się z platformy ASP.NET Core Blazor, umożliwia tworzenie interaktywnych po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET w aplikacji ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: seoapril2019
ms.date: 04/15/2019
uid: blazor/index
ms.openlocfilehash: a5b12a5c5c10a74ab192d0d67a2ba9a5617232c7
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614882"
---
# <a name="introduction-to-blazor"></a>Wprowadzenie do Blazor

Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

## <a name="razor-components"></a>Składniki Razor

Model hostingu po stronie serwera Blazor, *składniki Razor*, jest sposób tworzenia interaktywnego po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET:

* Tworzyć zaawansowane interaktywne UI za pomocą C# zamiast JavaScript.
* Udostępnianie aplikacji po stronie serwera i klienta logiki napisane przy użyciu platformy .NET.
* Renderuj interfejsie użytkownika jako HTML i CSS obsługi szerokiego przeglądarki, w tym przeglądarki dla urządzeń przenośnych.

Składniki razor obsługuje urządzenia podstawowe wymagane przez większość aplikacji, w tym:

* Parametry
* Obsługa zdarzeń
* Powiązanie danych
* Routing
* Wstrzykiwanie zależności
* Układy
* Tworzenie szablonów
* Kaskadowe wartości

Składniki razor oddziela składnika renderowania logiki sposób stosowania aktualizacje interfejsu użytkownika. Składniki Razor programu ASP.NET Core w środowisku .NET Core 3.0 dodaje obsługę hostowania Razor składników w aplikacji ASP.NET Core na serwerze. Aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem połączenia SignalR.

Środowisko uruchomieniowe:

* Obsługuje wysyłanie zdarzeń interfejsu użytkownika z przeglądarki do serwera.
* Zastosowanie interfejsu użytkownika aktualizacji wysłanych przez serwer z powrotem do przeglądarki, po uruchomieniu składników.

Połączenie używane przez składniki Razor do komunikowania się za pośrednictwem przeglądarki umożliwia również obsługę wywołań międzyoperacyjnych w języku JavaScript.

![Składniki razor uruchamiania kodu platformy .NET na serwerze i wchodzi w interakcję z Document Object Model na kliencie za pośrednictwem połączenia SignalR](index/_static/aspnet-core-razor-components.png)

Aby uzyskać więcej informacji, zobacz <xref:blazor/hosting-models#server-side-hosting-model>.

## <a name="components"></a>Składniki

A *składnika Razor* jest element interfejsu użytkownika, takich jak strony, okno dialogowe lub dane formularza zgłoszenia. Składniki obsługi zdarzeń użytkownika i definiowanie elastyczne logika renderowania interfejsu użytkownika. Składniki może zagnieżdżone i ponownie używane.

Składniki są klasy .NET, wbudowane zestawy .NET, które mogą być udostępniane i dystrybuowanych jako pakiety NuGet. Klasy zwykle są zapisywane w postaci znaczników stronę Razor za pomocą *.razor* rozszerzenie pliku (Razor składniki) lub strona znaczników Razor z *.cshtml* rozszerzenie (Blazor).

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

Gdy ten składnik jest używany w innym miejscu w aplikacji, funkcja IntelliSense przyspiesza tworzenie aplikacji przy użyciu uzupełnianie składni i parametrów.

Składniki renderowania do reprezentacji w pamięci w przeglądarce modelu DOM o nazwie *renderowania drzewa* , następnie można zaktualizować interfejs użytkownika w sposób, elastyczna i wydajna.

## <a name="javascript-interop"></a>Międzyoperacyjność w języku JavaScript

W przypadku aplikacji wymagających bibliotek JavaScript innych firm i przeglądarka interfejsów API składniki współdziałać z użyciem języka JavaScript. Składniki są w stanie korzystającego z dowolnej biblioteki lub interfejsu API języka JavaScript nie może korzystać. C#Kod może wywołać kod JavaScript i kodu w języku JavaScript można wywoływać C# kodu. Aby uzyskać więcej informacji, zobacz [międzyoperacyjnego JavaScript](xref:blazor/javascript-interop).

## <a name="code-sharing-and-net-standard"></a>Udostępnianie kodu i .NET Standard

Aplikacje można odwołać się i używać istniejącej [.NET Standard](/dotnet/standard/net-standard) bibliotek. .NET standard to formalną specyfikację interfejsów API platformy .NET, które są wspólne dla implementacji platformy .NET. Blazor implementuje .NET Standard 2.0. Interfejsy API, które nie są stosowane w przeglądarce sieci web (na przykład dostęp do systemu plików, gniazdo, wątki i inne funkcje) throw <xref:System.PlatformNotSupportedException>. Biblioteki klas .NET standard mogą być udostępniane na różnych platformach .NET, takich jak Blazor, .NET Framework, .NET Core, Xamarin, narzędzie Mono i Unity.

## <a name="blazor"></a>Blazor

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

Blazor to aplikacja jednostronicowa umożliwiająca tworzenie interaktywnych aplikacji sieci Web po stronie klienta przy użyciu platformy .NET. Blazor używa standardów sieci web Otwórz bez transpilation wtyczki lub kodu. Blazor działa we wszystkich przeglądarkach nowoczesne rozwiązania sieci web, w tym przeglądarki dla urządzeń przenośnych.

Przy użyciu platformy .NET w przeglądarce do tworzenia aplikacji internetowych po stronie klienta ma wiele zalet:

* **C#język**: Pisanie kodu w C# zamiast JavaScript.
* **Ekosystemu .NET**: Wykorzystaj istniejące ekosystemu bibliotek platformy .NET.
* **Tworzenie pełnych**: Udostępnij logiki po stronie klienta i serwera.
* **Szybkość i skalowalność**: .NET została skompilowana wydajność, niezawodność i bezpieczeństwo.
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

Aby zmniejszyć rozmiar pobranej aplikacji, nieużywany kod jest usuwany poza aplikacją, gdy zostanie opublikowany przez [konsolidatora języka pośredniego (IL)](xref:host-and-deploy/blazor/configure-linker).

Blazor po stronie klienta jest modelu hostingu po stronie klienta. Ponieważ Blazor oddziela składnika renderowania logiki, w jaki sposób są stosowane aktualizacje interfejsu użytkownika, w jaki może być hostowana Blazor jest elastyczność. Używanie platformy ASP.NET Core [składniki Razor](#razor-components) hosta Razor składników w aplikacji ASP.NET Core, w którym aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem połączenia SignalR na serwerze. Aby uzyskać więcej informacji, zobacz <xref:blazor/hosting-models#server-side-hosting-model>. 

Rozmiar ładunku jest czynnikiem wydajność krytycznych dla użyteczności aplikacji. Blazor optymalizuje rozmiar ładunku, aby zmniejszyć czas pobierania:

* Nieużywane części zestawów .NET są usuwane podczas procesu kompilacji.
* Odpowiedzi HTTP są kompresowane.
* Środowisko uruchomieniowe platformy .NET i zestawy są buforowane w przeglądarce.

[Składniki razor](#razor-components) zapewnia mniejszy rozmiar ładunku niż Blazor przy zachowaniu zestawy .NET, zestawu aplikacji i po stronie serwera środowisko uruchomieniowe. Razor składniki aplikacji służyć tylko plikami znaczników i statyczne elementy zawartości do klientów.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Format WebAssembly](http://webassembly.org/)
* [Przewodnik dla języka C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
