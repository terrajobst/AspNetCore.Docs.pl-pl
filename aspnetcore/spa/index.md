---
title: Szablony jednej strony aplikacji za pomocą platformy ASP.NET Core
author: SteveSandersonMS
description: Dowiedz się, jak zainstalować i rozpoczynanie pracy z szablonami projektu platformy ASP.NET Core jednej strony aplikacji JEDNOSTRONICOWEJ.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: f7bb23e9001c7606c3e622bf4575a4debec56ccd
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555563"
---
# <a name="use-the-single-page-application-templates-with-aspnet-core"></a><span data-ttu-id="becb0-103">Szablony jednej strony aplikacji za pomocą platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="becb0-103">Use the Single Page Application templates with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="becb0-104">Wydane oprogramowanie .NET Core 2.0.x zestaw SDK zawiera szablony projektów starsze dla kątową, platformy React i reagować z Redux.</span><span class="sxs-lookup"><span data-stu-id="becb0-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="becb0-105">W tej dokumentacji nie ma tych starszych szablony projektu — informacje.</span><span class="sxs-lookup"><span data-stu-id="becb0-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="becb0-106">W tej dokumentacji do najnowszej kątową, platformy React i reagują z Redux szablonów, które można zainstalować ręcznie do składnika ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="becb0-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="becb0-107">Szablony znajdują się domyślnie z platformy ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="becb0-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="becb0-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="becb0-108">Prerequisites</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="becb0-109">[Node.js](https://nodejs.org), w wersji 6 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="becb0-109">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="becb0-110">Instalacja</span><span class="sxs-lookup"><span data-stu-id="becb0-110">Installation</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="becb0-111">Szablony są już zainstalowane przy użyciu platformy ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="becb0-111">The templates are already installed with ASP.NET Core 2.1.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="becb0-112">Jeśli masz składnika ASP.NET 2.0 Core, uruchom następujące polecenie, aby zainstalować zaktualizowanych szablonów platformy ASP.NET Core dla kątową reagować i reagować z Redux:</span><span class="sxs-lookup"><span data-stu-id="becb0-112">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

::: moniker-end

## <a name="use-the-templates"></a><span data-ttu-id="becb0-113">Za pomocą szablonów</span><span class="sxs-lookup"><span data-stu-id="becb0-113">Use the templates</span></span>

* [<span data-ttu-id="becb0-114">Należy użyć szablonu projektu dyrektywy Angular</span><span class="sxs-lookup"><span data-stu-id="becb0-114">Use the Angular project template</span></span>](xref:spa/angular)
* [<span data-ttu-id="becb0-115">Należy użyć szablonu projektu w bibliotece React.</span><span class="sxs-lookup"><span data-stu-id="becb0-115">Use the React project template</span></span>](xref:spa/react)
* [<span data-ttu-id="becb0-116">Użyj platformy React z szablonem projektu Redux</span><span class="sxs-lookup"><span data-stu-id="becb0-116">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
