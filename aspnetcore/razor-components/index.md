---
title: Wprowadzenie do składników Razor
author: guardrex
description: Poznaj składniki Razor programu ASP.NET Core, .NET sieci web framework przy użyciu C#/Razor, jak i HTML.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/09/2019
uid: razor-components/index
ms.openlocfilehash: e8d75a0647704f1ff05e70a96abbb2e448868165
ms.sourcegitcommit: 5e3797a02ff3c48bb8cb9ad4320bfd169ebe8aba
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56102932"
---
# <a name="introduction-to-razor-components"></a>Wprowadzenie do składników Razor

Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

*Składniki razor* to nowy sposób tworzyć interaktywne po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET:

* Tworzyć zaawansowane interaktywne UI za pomocą C# zamiast JavaScript.
* Udostępnianie aplikacji po stronie serwera i klienta logiki napisane przy użyciu platformy .NET.
* Renderuj interfejsie użytkownika jako HTML i CSS obsługi szerokiego przeglądarki, w tym przeglądarki dla urządzeń przenośnych.

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

Składniki razor obsługują urządzenia podstawowe wymagane przez większość aplikacji, w tym:

* Parametry
* Obsługa zdarzeń
* Powiązanie danych
* Routing
* Wstrzykiwanie zależności
* Układy
* Tworzenie szablonów
* Kaskadowe wartości

## <a name="javascript-interop"></a>Międzyoperacyjność w języku JavaScript

W przypadku aplikacji wymagających bibliotek JavaScript innych firm i przeglądarce interfejsów API składniki Razor współdziałać z użyciem języka JavaScript. Składniki razor są w stanie korzystającego z dowolnej biblioteki lub interfejsu API języka JavaScript nie może korzystać. C#Kod może wywołać kod JavaScript i kodu w języku JavaScript można wywoływać C# kodu. Aby uzyskać więcej informacji, zobacz [międzyoperacyjnego JavaScript](xref:razor-components/javascript-interop).

## <a name="hosting-models"></a>Modele hostingu

### <a name="server-side-hosting-model"></a>Model hostingu po stronie serwera

Ponieważ składniki Razor oddzielenie logiki renderowania składnika w jaki sposób są stosowane aktualizacje interfejsu użytkownika, w jak Razor składniki mogą być hostowane jest elastyczność. Składniki Razor programu ASP.NET Core w środowisku .NET Core 3.0 dodaje obsługę hostowania Razor składników w aplikacji ASP.NET Core na serwerze. Aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem połączenia SignalR.

Środowisko uruchomieniowe:

* Obsługuje wysyłanie zdarzeń interfejsu użytkownika z przeglądarki do serwera.
* Zastosowanie interfejsu użytkownika aktualizacji wysłanych przez serwer z powrotem do przeglądarki, po uruchomieniu składników.

Połączenie używane przez składniki Razor do komunikowania się za pośrednictwem przeglądarki umożliwia również obsługę wywołań międzyoperacyjnych w języku JavaScript.

![Składniki razor uruchamiania kodu platformy .NET na serwerze i wchodzi w interakcję z Document Object Model na kliencie za pośrednictwem połączenia SignalR](index/_static/aspnet-core-razor-components.png)

Aby uzyskać więcej informacji, zobacz <xref:razor-components/hosting-models#server-side-hosting-model>.

### <a name="client-side-hosting-model"></a>Model hostingu po stronie klienta

*Blazor* jest eksperymentalna modelu hostingu po stronie klienta, składników Razor. Blazor działające na platformie .NET w przeglądarce, za pomocą standardów sieci web Otwórz bez transpilation wtyczki lub kodu. Blazor działa we wszystkich przeglądarkach nowoczesne rozwiązania sieci web, w tym przeglądarki dla urządzeń przenośnych.

Uruchamianie kodu platformy .NET w przeglądarkach sieci web jest możliwe, [format WebAssembly](http://webassembly.org) (skrócona *wasm*). Format WebAssembly to open sieci web, standard i obsługiwane w przeglądarkach sieci web bez wtyczek. Format WebAssembly to format compact kodu bajtowego zoptymalizowane pod kątem szybkiego pobierania i wykonywania maksymalną szybkość.

Format WebAssembly kod może uzyskać dostęp do pełnej funkcjonalności przeglądarki za pomocą międzyoperacyjności języka JavaScript. W tym samym czasie format WebAssembly kod jest uruchamiany w tym samym piaskownicy zaufanych jako język JavaScript, aby zapobiec złośliwe akcje na komputerze klienckim.

![Blazor uruchamia kod .NET w przeglądarce z format WebAssembly.](index/_static/blazor.png)

Przy aplikacji Blazor jest wbudowana i uruchomić w przeglądarce:

* C#pliki kodu i Razor są kompilowane do zestawów platformy .NET.
* Zestawy i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.
* Blazor używa języka JavaScript do uruchamiania środowiska uruchomieniowego .NET i konfiguruje środowiska uruchomieniowego należy załadować zestawy. Wywołania dokumentu Object Model (DOM) manipulacji i przeglądarka interfejsu API są obsługiwane przez środowisko uruchomieniowe Blazor za pomocą międzyoperacyjności języka JavaScript.

Aby zapewnić obsługę starszych przeglądarkach, które nie obsługują format WebAssembly, użyj [po stronie serwera modelu hostingu](#server-side-hosting-model).

Aby uzyskać więcej informacji, zobacz <xref:razor-components/hosting-models#client-side-hosting-model>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Format WebAssembly](http://webassembly.org/)
* [Przewodnik dla języka C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
