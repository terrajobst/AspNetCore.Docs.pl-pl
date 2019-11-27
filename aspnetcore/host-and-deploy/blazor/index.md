---
title: Hostowanie i wdrażanie ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikacje Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/23/2019
no-loc:
- Blazor
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 5c37c3d9f424f4c4b814e1955880623fd95179f2
ms.sourcegitcommit: 918d7000b48a2892750264b852bad9e96a1165a7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/27/2019
ms.locfileid: "74550371"
---
# <a name="host-and-deploy-aspnet-core-opno-locblazor"></a>Hostowanie i wdrażanie ASP.NET Core Blazor

[Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)i [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="publish-the-app"></a>Publikowanie aplikacji

Aplikacje są publikowane do wdrożenia w konfiguracji wydania.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Wybierz pozycję **kompilacja** > **Opublikuj aplikację {Application}** na pasku nawigacyjnym.
1. Wybierz *element docelowy publikowania*. Aby opublikować lokalnie, wybierz pozycję **folder**.
1. Zaakceptuj lokalizację domyślną w polu **Wybierz folder** lub określ inną lokalizację. Wybierz przycisk **Publikuj** .

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Użyj [dotnet Publish](/dotnet/core/tools/dotnet-publish) polecenia, aby opublikować aplikację z konfiguracją wydania:

```dotnetcli
dotnet publish -c Release
```

---

Opublikowanie aplikacji wyzwala [przywracanie](/dotnet/core/tools/dotnet-restore) zależności projektu i [kompiluje](/dotnet/core/tools/dotnet-build) projekt przed utworzeniem zasobów do wdrożenia. W ramach procesu kompilacji nieużywane metody i zestawy są usuwane w celu zmniejszenia rozmiaru pobierania aplikacji i czasów ładowania.

Aplikacja webassembly Blazor jest publikowana w folderze */bin/Release/{Target Framework}/Publish/{Assembly Name}/dist* . Aplikacja serwera Blazor jest publikowana w folderze */bin/Release/{Target Framework}/Publish* .

Zasoby w folderze są wdrażane na serwerze sieci Web. Wdrożenie może być procesem ręcznym lub zautomatyzowanym w zależności od używanych narzędzi programistycznych.

## <a name="app-base-path"></a>Ścieżka podstawowa aplikacji

*Ścieżka podstawowa aplikacji* jest ścieżką głównego adresu URL aplikacji. Weź pod uwagę następujące ASP.NET Core aplikacji i Blazor aplikacji podrzędnej:

* Aplikacja ASP.NET Core ma nazwę `MyApp`:
  * Aplikacja fizycznie znajduje się na *d:/MojaApl*.
  * Żądania są odbierane w `https://www.contoso.com/{MYAPP RESOURCE}`.
* Aplikacja Blazor o nazwie `CoolApp` jest podaplikacją `MyApp`:
  * Aplikacja podrzędna fizycznie znajduje się na *d:/MojaApl/CoolApp*.
  * Żądania są odbierane w `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.

Bez określania dodatkowej konfiguracji `CoolApp`aplikacja podrzędna w tym scenariuszu nie ma informacji o tym, gdzie znajduje się na serwerze. Na przykład aplikacja nie może utworzyć prawidłowych względnych adresów URL do swoich zasobów, nie wiedząc, że znajduje się w względnej ścieżce adresu URL `/CoolApp/`.

Aby zapewnić konfigurację Blazor ścieżce podstawowej `https://www.contoso.com/CoolApp/`, atrybut `href` tagu `<base>` jest ustawiany na względną ścieżkę katalogu głównego w pliku *Pages/_Host. cshtml* (Blazor Server) lub pliku *wwwroot/index.html* (Blazor webassembly):

```html
<base href="/CoolApp/">
```

aplikacje serwera Blazor dodatkowo ustawiać ścieżkę bazową po stronie serwera, wywołując <xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*> w potoku żądania aplikacji `Startup.Configure`:

```csharp
app.UsePathBase("/CoolApp");
```

Dostarczając względną ścieżkę adresu URL, składnik, który nie znajduje się w katalogu głównym, może konstruować adresy URL względem ścieżki katalogu głównego aplikacji. Składniki na różnych poziomach struktury katalogów mogą tworzyć linki do innych zasobów w lokalizacji w całej aplikacji. Ścieżka podstawowa aplikacji służy również do przechwytywania wybranych hiperłączy, w których `href` miejsce docelowe łącza znajduje się w obszarze URI ścieżki podstawowej aplikacji. Router Blazor obsługuje nawigację wewnętrzną.

W wielu scenariuszach hostingu względna ścieżka URL do aplikacji jest katalogiem głównym aplikacji. W takich przypadkach ścieżką bazową względnego adresu URL aplikacji jest ukośnik (`<base href="/" />`), który jest domyślną konfiguracją aplikacji Blazor. W innych scenariuszach hostingu, takich jak strony GitHub i aplikacje podrzędne IIS, ścieżka podstawowa aplikacji musi być ustawiona na względną ścieżkę URL serwera aplikacji.

Aby ustawić ścieżkę bazową aplikacji, zaktualizuj tag `<base>` wewnątrz elementów tagów `<head>` pliku *Pages/_Host. cshtml* (serwerBlazor) lub plik *wwwroot/index.html* (Blazor webassembly). Ustaw wartość atrybutu `href` na `/{RELATIVE URL PATH}/` (wymagany jest końcowy ukośnik), gdzie `{RELATIVE URL PATH}` jest pełną względną ścieżką URL aplikacji.

W przypadku aplikacji Blazor webassembly z ścieżką względnego adresu URL niegłówną (na przykład `<base href="/CoolApp/">`) aplikacja nie będzie mogła znaleźć zasobów, *gdy są uruchamiane lokalnie*. Aby rozwiązać ten problem podczas lokalnego tworzenia i testowania, można podać podstawowy argument *ścieżki* , który odpowiada wartości `href` tagu `<base>` w czasie wykonywania. Nie dołączaj końcowego ukośnika. Aby przekazać argument podstawowy ścieżki podczas lokalnego uruchamiania aplikacji, uruchom `dotnet run` polecenie w katalogu aplikacji z opcją `--pathbase`:

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

W przypadku aplikacji Blazor webassembly ze względną ścieżką URL `/CoolApp/` (`<base href="/CoolApp/">`) polecenie to:

```dotnetcli
dotnet run --pathbase=/CoolApp
```

Aplikacja webassembly Blazor reaguje lokalnie na `http://localhost:port/CoolApp`.

## <a name="deployment"></a>Wdrażanie

Aby uzyskać wskazówki dotyczące wdrażania, zobacz następujące tematy:

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
