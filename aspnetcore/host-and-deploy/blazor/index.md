---
title: Hostowanie i wdrażanie ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikacje Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/index
ms.openlocfilehash: ddf70da29a82d462422c1bdf74ff45b92bb10b56
ms.sourcegitcommit: 5bdc54162d7dea8d9fa54ac3055678db23586af1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2020
ms.locfileid: "79434268"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a>Hostowanie i wdrażanie ASP.NET Core Blazor

[Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)i [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="publish-the-app"></a>Publikowanie aplikacji

Aplikacje są publikowane do wdrożenia w konfiguracji wydania.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Wybierz pozycję **kompilacja** > **Opublikuj aplikację {Application}** na pasku nawigacyjnym.
1. Wybierz *element docelowy publikowania*. Aby opublikować lokalnie, wybierz pozycję **folder**.
1. Zaakceptuj lokalizację domyślną w polu **Wybierz folder** lub określ inną lokalizację. Wybierz przycisk **Publikuj**.

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Użyj [dotnet Publish](/dotnet/core/tools/dotnet-publish) polecenia, aby opublikować aplikację z konfiguracją wydania:

```dotnetcli
dotnet publish -c Release
```

---

Opublikowanie aplikacji wyzwala [przywracanie](/dotnet/core/tools/dotnet-restore) zależności projektu i [kompiluje](/dotnet/core/tools/dotnet-build) projekt przed utworzeniem zasobów do wdrożenia. W ramach procesu kompilacji nieużywane metody i zestawy są usuwane w celu zmniejszenia rozmiaru pobierania aplikacji i czasów ładowania.

Lokalizacje publikowania:

* Blazor webassembly
  * Autonomiczna &ndash; aplikacja jest publikowana w folderze */bin/Release/{Target Framework}/Publish/wwwroot* . Aby wdrożyć aplikację jako lokację statyczną, skopiuj zawartość folderu *wwwroot* do hosta lokacji statycznej.
  * Hostowane &ndash; aplikacja webassembly Blazor Client jest publikowana w folderze */bin/Release/{Target Framework}/Publish/wwwroot* aplikacji serwera, wraz ze wszystkimi innymi statycznymi zasobami sieci Web aplikacji serwera. Wdróż zawartość folderu *publikowania* na hoście.
* Blazor Server &ndash; aplikacja zostanie opublikowana w folderze */bin/Release/{Target Framework}/Publish* . Wdróż zawartość folderu *publikowania* na hoście.

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

## <a name="deployment"></a>wdrażania

Aby uzyskać wskazówki dotyczące wdrażania, zobacz następujące tematy:

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
