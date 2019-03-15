---
title: Wprowadzenie do składników Razor
author: guardrex
description: 'Poznaj składniki programu ASP.NET Core Razor, umożliwia tworzenie interaktywnych po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET w aplikacji ASP.NET Core.'
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/13/2019
uid: razor-components/index
---
# <a name="introduction-to-razor-components"></a>Wprowadzenie do składników Razor

Przez [Daniel Roth](https://github.com/danroth27) i [Luke Latham](https://github.com/guardrex)

*Składniki razor* to sposób na tworzenie interaktywnych po stronie klienta interfejsu użytkownika sieci web przy użyciu platformy .NET:

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

Aby uzyskać więcej informacji, zobacz <xref:razor-components/hosting-models#server-side-hosting-model>.

*Blazor* jest eksperymentalna modelu hostingu po stronie klienta, składników Razor. Blazor działające na platformie .NET w przeglądarce, za pomocą standardów sieci web Otwórz bez transpilation wtyczki lub kodu. Aby uzyskać więcej informacji, zobacz <xref:razor-components/hosting-models#client-side-hosting-model>.

## <a name="components"></a>Składniki

A *składnika Razor* jest element interfejsu użytkownika, takich jak strony, okno dialogowe lub dane formularza zgłoszenia. Składniki obsługi zdarzeń użytkownika i definiowanie elastyczne logika renderowania interfejsu użytkownika. Składniki może zagnieżdżone i ponownie używane.

Składniki są klasy .NET, wbudowane zestawy .NET, które mogą być udostępniane i dystrybuowanych jako pakiety NuGet. Klasy zwykle są zapisywane w postaci znaczników stronę Razor za pomocą *.razor* rozszerzenie pliku.

[Razor](xref:mvc/views/razor) używa składni łączenia kod znaczników HTML za pomocą C# kodu. Razor jest przeznaczony do pracy deweloperskiej, dzięki czemu dla deweloperów przełączać się między znaczników i C# w tym samym pliku z [IntelliSense](/visualstudio/ide/using-intellisense) pomocy technicznej. Strony razor i MVC widoki również używają Razor. W przeciwieństwie do stron Razor i widoków MVC, które są zbudowane wokół modelu żądań/odpowiedzi, składniki są używane dla obsługi kompozycji interfejsu użytkownika. Składniki razor mogą służyć specjalnie dla logika interfejsu użytkownika po stronie klienta i kompozycji.

Następujący kod jest przykładem składnika niestandardowy dialog w pliku Razor (*DialogComponent.razor*):

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

W przypadku aplikacji wymagających bibliotek JavaScript innych firm i przeglądarka interfejsów API składniki współdziałać z użyciem języka JavaScript. Składniki są w stanie korzystającego z dowolnej biblioteki lub interfejsu API języka JavaScript nie może korzystać. C#Kod może wywołać kod JavaScript i kodu w języku JavaScript można wywoływać C# kodu. Aby uzyskać więcej informacji, zobacz [międzyoperacyjnego JavaScript](xref:razor-components/javascript-interop).

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:spa/blazor/index>
* [Format WebAssembly](http://webassembly.org/)
* [Przewodnik dla języka C#](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
