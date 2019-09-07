---
title: Hostowanie i wdrażanie ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak hostować i wdrażać aplikacje Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 5a56bbda5bb7727c7dbeaed7f2a91d0dcb6e7e71
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773585"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a>Hostowanie i wdrażanie ASP.NET Core Blazor

[Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com)i [Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>Publikowanie aplikacji

Aplikacje są publikowane do wdrożenia w konfiguracji wydania.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Wybierz pozycję **kompilacja** > **Opublikuj aplikację {aplikacja}** na pasku nawigacyjnym.
1. Wybierz *element docelowy publikowania*. Aby opublikować lokalnie, wybierz pozycję **folder**.
1. Zaakceptuj lokalizację domyślną w polu **Wybierz folder** lub określ inną lokalizację. Wybierz przycisk **Publikuj** .

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Użyj [dotnet Publish](/dotnet/core/tools/dotnet-publish) polecenia, aby opublikować aplikację z konfiguracją wydania:

```console
dotnet publish -c Release
```

---

Opublikowanie aplikacji wyzwala [przywracanie](/dotnet/core/tools/dotnet-restore) zależności projektu i [kompiluje](/dotnet/core/tools/dotnet-build) projekt przed utworzeniem zasobów do wdrożenia. W ramach procesu kompilacji nieużywane metody i zestawy są usuwane w celu zmniejszenia rozmiaru pobierania aplikacji i czasów ładowania.

Aplikacja po stronie klienta Blazor jest publikowana w folderze */bin/Release/{Target Framework}/Publish/{Assembly Name}/dist* . Aplikacja po stronie serwera Blazor jest publikowana w folderze */bin/Release/{Target Framework}/Publish* .

Zasoby w folderze są wdrażane na serwerze sieci Web. Wdrożenie może być procesem ręcznym lub zautomatyzowanym w zależności od używanych narzędzi programistycznych.

## <a name="app-base-path"></a>Ścieżka podstawowa aplikacji

*Ścieżka podstawowa aplikacji* jest ścieżką głównego adresu URL aplikacji. Weź pod uwagę następujące aplikacje główne i Blazor:

* Główna aplikacja jest wywoływana `MyApp`:
  * Aplikacja znajduje się fizycznie w lokalizacji *\\d: MojaApl*.
  * Żądania są odbierane `https://www.contoso.com/{MYAPP RESOURCE}`o.
* Aplikacja Blazor o nazwie `CoolApp` jest podaplikacją z: `MyApp`
  * Aplikacja podrzędna fizycznie znajduje się w lokalizacji *d:\\MojaApl\\CoolApp*.
  * Żądania są odbierane `https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`o.

Bez określania dodatkowej konfiguracji `CoolApp`programu aplikacja podrzędna w tym scenariuszu nie ma informacji o tym, gdzie znajduje się na serwerze. Na przykład aplikacja nie może utworzyć prawidłowych względnych adresów URL do swoich zasobów, nie wiedząc, że znajduje się w względnej ścieżce `/CoolApp/`adresu URL.

`https://www.contoso.com/CoolApp/`Aby zapewnić konfigurację ścieżki podstawowej aplikacji Blazor `<base>` , `href` atrybut tagu jest ustawiany na ścieżkę względną root w pliku *wwwroot/index.html* :

```html
<base href="/CoolApp/">
```

Dostarczając względną ścieżkę adresu URL, składnik, który nie znajduje się w katalogu głównym, może konstruować adresy URL względem ścieżki katalogu głównego aplikacji. Składniki na różnych poziomach struktury katalogów mogą tworzyć linki do innych zasobów w lokalizacji w całej aplikacji. Ścieżka podstawowa aplikacji jest również używana do przechwytywania hiperłącze, gdzie `href` element docelowy linku znajduje się w przestrzeni&mdash;URI ścieżki bazowej aplikacji. router Blazor obsługuje nawigację wewnętrzną.

W wielu scenariuszach hostingu względna ścieżka URL do aplikacji jest katalogiem głównym aplikacji. W takich przypadkach Ścieżka bazowa względnego adresu URL aplikacji jest ukośnikiem (`<base href="/" />`), który jest domyślną konfiguracją dla aplikacji Blazor. W innych scenariuszach hostingu, takich jak strony GitHub i aplikacje podrzędne IIS, ścieżka podstawowa aplikacji musi być ustawiona na ścieżkę URL względną dla serwera do aplikacji.

Aby ustawić ścieżkę bazową aplikacji, zaktualizuj `<base>` tag `<head>` wewnątrz elementów tagów pliku *wwwroot/index.html* . Ustaw wartość `/{RELATIVE URL PATH}/`atrybutuna (wymagany jest końcowy ukośnik), gdzie `{RELATIVE URL PATH}` jest pełną względną ścieżkę adresu URL aplikacji. `href`

W przypadku aplikacji z ścieżką względną adresu URL niegłówną (na przykład `<base href="/CoolApp/">`) aplikacja nie będzie mogła znaleźć zasobów w *przypadku uruchamiania lokalnego*. Aby rozwiązać ten problem podczas lokalnego tworzenia i testowania, można podać podstawowy argument *ścieżki* , który jest zgodny `href` z wartością `<base>` tagu w czasie wykonywania. Aby przekazać argument podstawowy ścieżki podczas lokalnego uruchamiania aplikacji, należy wykonać `dotnet run` polecenie z katalogu aplikacji `--pathbase` z opcją:

```console
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

W przypadku aplikacji ze względną ścieżką `/CoolApp/` URL (`<base href="/CoolApp/">`) polecenie to:

```console
dotnet run --pathbase=/CoolApp
```

Aplikacja reaguje lokalnie o `http://localhost:port/CoolApp`.

## <a name="deployment"></a>wdrażania

Aby uzyskać wskazówki dotyczące wdrażania, zobacz następujące tematy:

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
