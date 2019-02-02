---
title: Wprowadzenie do składników Razor
author: guardrex
description: Zapoznaj się z Blazor, używając framework sieci web platformy .NET C#/Razor, jak i HTML, który działa w przeglądarce, za pomocą format WebAssembly.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/index
ms.openlocfilehash: 0f7a4b2ec404b6ceec2e643fea9e0635de6ad716
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668136"
---
# <a name="introduction-to-razor-components"></a>Wprowadzenie do składników Razor

Przez [Steve sanderson o](http://blog.stevensanderson.com), [Daniel Roth](https://github.com/danroth27), i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

*Składniki programu ASP.NET Core Razor* wykonuje po stronie serwera w programie ASP.NET Core podczas *Blazor* (składniki Razor wykonywane po stronie klienta) to eksperymentalne struktura sieci web platformy .NET przy użyciu C#/Razor, jak i HTML, która działa w Format WebAssembly w przeglądarce. Blazor udostępnia wszystkie korzyści zapewnianych przez strukturę interfejsu użytkownika sieci web po stronie klienta przy użyciu platformy .NET na komputerze klienckim.

Tworzenie aplikacji sieci Web ulepszono na wiele sposobów, w ciągu lat, ale nadal tworzenia nowoczesnych aplikacji internetowych stwarza trudności. Przy użyciu platformy .NET w przeglądarce oferuje wiele korzyści, które może sprawić, tworzenie aplikacji sieci web łatwiejsze i bardziej wydajne:

* **Stabilność i spójność**: .NET zapewnia standardowych środowisk programowania na platformach, które są stabilne, bogate i łatwy w użyciu.
* **Nowoczesnych języków innowacyjne**: nieustannie poprawiamy języków .NET za pomocą innowacyjnych nowe funkcje języka.
* **Wiodące w branży narzędzi**: Rodziny produktów Visual Studio zapewnia doskonałego środowiska pracy programowania .NET na platformach Windows, Linux i macOS.
* **Szybkość i skalowalność**: .NET w przeszłości silne wydajność, niezawodność i bezpieczeństwo, do tworzenia aplikacji. Przy użyciu platformy .NET, jako rozwiązanie w pełnym stosie ułatwia tworzenie aplikacji szybki, niezawodny i bezpieczny.
* **Rozwoju pełnym stosie, który korzysta ze znanych Ci**: C#/ Razor deweloperom korzystanie z istniejącej C#/Razor umiejętności, aby zapisać po stronie klienta kodu i udostępniaj serwera i logiki po stronie klienta między aplikacjami.
* **Obsługa przeglądarek szerokiego**: Składniki razor renderowania interfejsu użytkownika, jako kod znaczników zwykłego i JavaScript. Blazor działające na platformie .NET w przeglądarce przy użyciu standardów sieci web Otwórz za pomocą nie dodatków plug-in i transpilation nie kodu. Blazor działa we wszystkich przeglądarkach nowoczesne rozwiązania sieci web, w tym przeglądarki dla urządzeń przenośnych.

## <a name="hosting-models"></a>Modelach hostingu

### <a name="server-side-hosting-model"></a>Model hostingu po stronie serwera

Ponieważ składniki Razor oddzielenie logiki renderowania składnika w jaki sposób są stosowane aktualizacje interfejsu użytkownika, w jak Razor składniki mogą być hostowane jest elastyczność. Składniki Razor programu ASP.NET Core w środowisku .NET Core 3.0 dodaje obsługę hostowania Razor składników w aplikacji ASP.NET Core, w którym wszystkie aktualizacje interfejsu użytkownika są obsługiwane za pośrednictwem połączenia SignalR na serwerze. Środowisko wykonawcze obsługuje wysyłanie zdarzeń interfejsu użytkownika z przeglądarki do serwera, a następnie stosuje aktualizacje interfejsu użytkownika wysyłane przez serwer z powrotem do przeglądarki po uruchomieniu składników. To samo połączenie umożliwia również obsługę wywołań międzyoperacyjnych w języku JavaScript.

![Składniki razor uruchamiania kodu platformy .NET na serwerze i wchodzi w interakcję z Document Object Model na kliencie za pośrednictwem połączenia SignalR](index/_static/aspnet-core-razor-components.png)

Aby uzyskać więcej informacji, zobacz <xref:razor-components/hosting-models#server-side-hosting-model>.

### <a name="client-side-hosting-model"></a>Model hostingu po stronie klienta

Uruchamianie kodu platformy .NET w przeglądarkach sieci web jest możliwe dzięki stosunkowo nowe technologie, [format WebAssembly](http://webassembly.org) (skrócona *wasm*). Format WebAssembly jest open web standardowych i jest obsługiwany w przeglądarkach sieci web bez wtyczek. Format WebAssembly to format compact kodu bajtowego zoptymalizowane pod kątem szybkiego pobierania i wykonywania maksymalną szybkość.

Format WebAssembly kod może uzyskać dostęp do pełnej funkcjonalności przeglądarki za pomocą międzyoperacyjności języka JavaScript. W tym samym czasie format WebAssembly kod jest uruchamiany w tym samym piaskownicy zaufanych jako język JavaScript, aby zapobiec złośliwe akcje na komputerze klienckim.

![Blazor uruchamia kod .NET w przeglądarce z format WebAssembly.](index/_static/blazor.png)

Przy aplikacji Blazor jest wbudowana i uruchomić w przeglądarce:

1. C#pliki kodu i Razor są kompilowane do zestawów platformy .NET.
1. Zestawy i środowisko uruchomieniowe platformy .NET są pobierane do przeglądarki.
1. Blazor używa języka JavaScript do uruchamiania środowiska uruchomieniowego .NET i konfiguruje środowiska uruchomieniowego należy załadować wymagane odwołania do zestawu. Dokument object model (DOM) manipulacji i przeglądarki wywołań interfejsu API są obsługiwane przez środowisko uruchomieniowe Blazor za pośrednictwem interoperacyjności języka JavaScript.

Aby zapewnić obsługę starszych przeglądarkach, które nie obsługują format WebAssembly, można użyć składników platformy ASP.NET Core Razor [po stronie serwera modelu hostingu](#server-side-hosting-model).

Aby uzyskać więcej informacji, zobacz <xref:razor-components/hosting-models#client-side-hosting-model>.

## <a name="components"></a>Składniki

Aplikacje są tworzone za pomocą *składniki*. Składnik jest element interfejsu użytkownika, takich jak strony, okno dialogowe lub dane formularza zgłoszenia. Składniki mogą zagnieżdżony, ponownie i współużytkowane między projektami.

A *składnika* jest klasą .NET. Klasa albo mogą być zapisywane bezpośrednio, jako C# klasy (*\*.cs*), lub częściej w postaci strony Razor znaczników (*\*.cshtml*).

[Razor](/aspnet/core/mvc/views/razor) używa składni łączenia kod znaczników HTML za pomocą C# kodu. Razor jest przeznaczony do pracy deweloperskiej, dzięki czemu dla deweloperów przełączać się między znaczników i C# w tym samym pliku z [IntelliSense](/visualstudio/ide/using-intellisense) pomocy technicznej. Następujący kod jest przykładem składnika podstawowe niestandardowy dialog w pliku Razor (*DialogComponent.cshtml*):

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

Składniki mogą być:

* Nested.
* Utworzony ze składnią Razor (*\*.cshtml*) lub C# (*\*.cs*) kod.
* Udostępnione za pośrednictwem biblioteki klas.
* Jednostka przetestowane bez konieczności przeglądarka modelu DOM.

## <a name="infrastructure"></a>Infrastruktura

Składniki razor oferują funkcje podstawowe, które wymagają większość aplikacji, w tym:

* Układy
* Routing
* Wstrzykiwanie zależności

Wszystkie te funkcje są opcjonalne. Gdy jeden z tych funkcji nie jest używany w aplikacji, implementacja jest usuwany z aplikacji po opublikowaniu przez [konsolidatora języka pośredniego (IL)](xref:host-and-deploy/razor-components/configure-linker).

## <a name="code-sharing-and-net-standard"></a>Udostępnianie kodu i .NET Standard

Aplikacje można odwołać się i używać istniejącej [.NET Standard](/dotnet/standard/net-standard) bibliotek. .NET standard to formalną specyfikację interfejsów API platformy .NET, które są wspólne dla implementacji platformy .NET. .NET standard 2.0 lub nowszej jest obsługiwana. Interfejsy API, które nie są stosowane w przeglądarce sieci web (na przykład dostęp do systemu plików, gniazdo, wątki i inne funkcje) throw [PlatformNotSupportedException](/dotnet/api/system.platformnotsupportedexception). Biblioteki klas .NET standard mogą być udostępniane w kodzie serwera oraz w aplikacjach opartych na przeglądarce.

## <a name="javascript-interop"></a>JavaScript interop

W przypadku aplikacji wymagających bibliotek JavaScript innych firm i przeglądarka interfejsów API format WebAssembly zaprojektowano pod kątem współdziałania z użyciem języka JavaScript. Składniki razor są w stanie korzystającego z dowolnej biblioteki lub interfejsu API języka JavaScript nie może korzystać. C#Kod może wywołać kod JavaScript i kodu w języku JavaScript można wywoływać C# kodu. Aby uzyskać więcej informacji, zobacz [międzyoperacyjnego JavaScript](xref:razor-components/javascript-interop).

## <a name="optimization"></a>Optymalizacja

W przypadku aplikacji po stronie klienta rozmiar ładunku jest krytyczny. Blazor optymalizuje rozmiar ładunku, aby zmniejszyć czas pobierania. Na przykład nieużywanych części zestawów .NET są usuwane podczas procesu kompilacji, są skompresowane odpowiedzi HTTP i środowisko uruchomieniowe platformy .NET i zestawy są buforowane w przeglądarce.

Składniki razor zapewnia jeszcze mniejszy rozmiar ładunku niż Blazor przy zachowaniu zestawy .NET, zestawu aplikacji i środowiska uruchomieniowego — po stronie serwera. Razor składników aplikacji jednocześnie znaczników, skrypty i arkusze stylów klientów.

## <a name="deployment"></a>wdrażania

Do tworzenia aplikacji po stronie klienta czystego autonomicznej lub pełnych aplikacji ASP.NET Core, który zawiera aplikacje serwera i klienta, należy użyć Blazor:

* W **autonomiczną aplikacją po stronie klienta**, aplikacja Blazor jest skompilowany w *dist* folder, który zawiera tylko pliki statyczne. Pliki mogą być hostowane w usłudze Azure App Service, strony GitHub, usług IIS (skonfigurowany jako serwer plików statycznych), serwery środowiska Node.js i wiele innych serwerów i usług. .NET nie jest wymagane na serwerze w środowisku produkcyjnym.
* W **aplikacji ASP.NET Core pełnych**, kod może być współużytkowana serwera i aplikacji klienckich. Wynikowa aplikacja ASP.NET Core Razor składników obsługuje interfejs użytkownika po stronie klienta i inne punkty końcowe interfejsu API po stronie serwera, można skompilować i wdrożyć do dowolnego hosta w chmurze lub lokalnie, obsługiwana przez platformy ASP.NET Core.

## <a name="suggest-a-feature-or-file-a-bug-report"></a>Zasugerować funkcję lub plik raport o usterce

Aby zapewnić sugestie i raporty o usterkach, [Otwórz problem](https://github.com/aspnet/AspNetCore/issues/new). Aby uzyskać ogólną Pomoc i Uzyskaj odpowiedzi od społeczności, Dołącz do konwersacji na [dotyczącym oprogramowania Gitter](https://gitter.im/aspnet/Blazor).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Format WebAssembly](http://webassembly.org/)
* [Przewodnik dla języka C#](/dotnet/csharp/)
* [Razor](/aspnet/core/mvc/views/razor)
* [HTML](https://www.w3.org/html/)
