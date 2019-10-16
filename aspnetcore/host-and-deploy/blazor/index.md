---
title: Hostowanie i wdrażanie ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikacje Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 271135a0ebe67d31fd8e2bcf672e723814727147
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391342"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a>Hostowanie i wdrażanie ASP.NET Core Blazor

[Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)i [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="publish-the-app"></a>Publikowanie aplikacji

Aplikacje są publikowane do wdrożenia w konfiguracji wydania.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Wybierz pozycję **kompilacja** > **publikowanie aplikacji {Application}** na pasku nawigacyjnym.
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

*Ścieżka podstawowa aplikacji* jest ścieżką głównego adresu URL aplikacji. Weź pod uwagę następujące aplikacje główne i Blazor:

* Główna aplikacja jest nazywana `MyApp`:
  * Aplikacja fizycznie znajduje się w lokalizacji *d: \\MyApp*.
  * Żądania są odbierane w `https://www.contoso.com/{MYAPP RESOURCE}`.
* Aplikacja Blazor o nazwie `CoolApp` jest aplikacją podrzędną `MyApp`:
  * Aplikacja podrzędna fizycznie znajduje się w lokalizacji *d: \\MyApp @ no__t-2CoolApp*.
  * Żądania są odbierane w `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`.

Bez określania dodatkowej konfiguracji dla `CoolApp` Aplikacja podrzędna w tym scenariuszu nie ma informacji o tym, gdzie znajduje się na serwerze. Na przykład aplikacja nie może utworzyć prawidłowych względnych adresów URL do swoich zasobów, nie wiedząc, że znajduje się w względnej ścieżce URL `/CoolApp/`.

Aby zapewnić konfigurację @no__t ścieżki podstawowej aplikacji Blazor-0, atrybut `href` znacznika `<base>` jest ustawiony jako względna ścieżka katalogu głównego w pliku *wwwroot/index.html* :

```html
<base href="/CoolApp/">
```

Dostarczając względną ścieżkę adresu URL, składnik, który nie znajduje się w katalogu głównym, może konstruować adresy URL względem ścieżki katalogu głównego aplikacji. Składniki na różnych poziomach struktury katalogów mogą tworzyć linki do innych zasobów w lokalizacji w całej aplikacji. Ścieżka podstawowa aplikacji jest również używana do przechwytywania hiperłącze, gdzie element docelowy `href` łącza znajduje się w obszarze URI ścieżki podstawowej aplikacji przestrzeń @ no__t-1The Blazor router obsługuje nawigację wewnętrzną.

W wielu scenariuszach hostingu względna ścieżka URL do aplikacji jest katalogiem głównym aplikacji. W takich przypadkach ścieżką bazową względnego adresu URL aplikacji jest ukośnik (`<base href="/" />`), który jest domyślną konfiguracją dla aplikacji Blazor. W innych scenariuszach hostingu, takich jak strony GitHub i aplikacje podrzędne IIS, ścieżka podstawowa aplikacji musi być ustawiona na ścieżkę URL względną dla serwera do aplikacji.

Aby ustawić ścieżkę bazową aplikacji, zaktualizuj tag `<base>` w obrębie elementów tagów `<head>` pliku *wwwroot/index.html* . Ustaw wartość atrybutu `href` na `/{RELATIVE URL PATH}/` (wymagany jest końcowy ukośnik), gdzie `{RELATIVE URL PATH}` jest pełną względną ścieżką URL aplikacji.

W przypadku aplikacji z ścieżką względną adresu URL niegłówną (na przykład `<base href="/CoolApp/">`) aplikacja nie będzie mogła znaleźć zasobów w *przypadku uruchamiania lokalnego*. Aby rozwiązać ten problem podczas lokalnego tworzenia i testowania, można podać podstawowy argument *ścieżki* , który pasuje do `href` znacznika `<base>` w czasie wykonywania. Aby przekazać argument podstawowy ścieżki podczas lokalnego uruchamiania aplikacji, należy wykonać polecenie `dotnet run` z katalogu aplikacji z opcją `--pathbase`:

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

W przypadku aplikacji ze względną ścieżką URL `/CoolApp/` (`<base href="/CoolApp/">`) polecenie to:

```dotnetcli
dotnet run --pathbase=/CoolApp
```

Aplikacja reaguje lokalnie na `http://localhost:port/CoolApp`.

## <a name="deployment"></a>wdrażania

Aby uzyskać wskazówki dotyczące wdrażania, zobacz następujące tematy:

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
