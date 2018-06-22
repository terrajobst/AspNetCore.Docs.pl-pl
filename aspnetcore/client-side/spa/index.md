---
title: Szablony jednej strony aplikacji za pomocą platformy ASP.NET Core
author: SteveSandersonMS
description: Dowiedz się, jak zainstalować i rozpoczynanie pracy z szablonami projektu platformy ASP.NET Core jednej strony aplikacji JEDNOSTRONICOWEJ.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/index
ms.openlocfilehash: ab164ae5d2df47739ec04b32cd21835ffdf9f87f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291532"
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a>Szablony jednej strony aplikacji za pomocą platformy ASP.NET Core

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Wydane oprogramowanie .NET Core 2.0.x zestaw SDK zawiera szablony projektów starsze dla kątową, platformy React i reagować z Redux. W tej dokumentacji nie ma tych starszych szablony projektu — informacje. W tej dokumentacji do najnowszej kątową, platformy React i reagują z Redux szablonów, które można zainstalować ręcznie do składnika ASP.NET 2.0 Core. Szablony znajdują się domyślnie z platformy ASP.NET Core 2.1.

::: moniker-end

## <a name="prerequisites"></a>Wymagania wstępne

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* [Node.js](https://nodejs.org), w wersji 6 lub nowszej

## <a name="installation"></a>Instalacja

::: moniker range=">= aspnetcore-2.1"

Szablony są już zainstalowane przy użyciu platformy ASP.NET Core 2.1.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Jeśli masz składnika ASP.NET 2.0 Core, uruchom następujące polecenie, aby zainstalować zaktualizowanych szablonów platformy ASP.NET Core dla kątową reagować i reagować z Redux:

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a>Za pomocą szablonów

* [Należy użyć szablonu projektu dyrektywy Angular](xref:spa/angular)
* [Należy użyć szablonu projektu w bibliotece React.](xref:spa/react)
* [Użyj platformy React z szablonem projektu Redux](xref:spa/react-with-redux)
