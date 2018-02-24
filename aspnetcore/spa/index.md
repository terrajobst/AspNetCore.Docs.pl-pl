---
title: "Za pomocą szablonów jednej strony aplikacji"
author: SteveSandersonMS
description: "Dowiedz się, jak zainstalować i rozpoczynanie pracy z szablonami projektu platformy ASP.NET Core jednej strony aplikacji JEDNOSTRONICOWEJ."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/index
ms.openlocfilehash: 63b56de101199e9ea0d66d89d2dd7288e47902f6
ms.sourcegitcommit: 49fb3b7669b504d35edad34db8285e56b958a9fc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/23/2018
---
# <a name="use-the-single-page-application-templates"></a><span data-ttu-id="5eb34-103">Za pomocą szablonów jednej strony aplikacji</span><span class="sxs-lookup"><span data-stu-id="5eb34-103">Use the Single Page Application templates</span></span>

> [!NOTE]
> <span data-ttu-id="5eb34-104">Wydane oprogramowanie .NET Core 2.0.x zestaw SDK zawiera szablony projektów starsze dla kątową, platformy React i reagować z Redux.</span><span class="sxs-lookup"><span data-stu-id="5eb34-104">The released .NET Core 2.0.x SDK includes older project templates for Angular, React, and React with Redux.</span></span> <span data-ttu-id="5eb34-105">W tej dokumentacji nie ma tych starszych szablony projektu — informacje.</span><span class="sxs-lookup"><span data-stu-id="5eb34-105">This documentation isn't about those older project templates.</span></span> <span data-ttu-id="5eb34-106">W tej dokumentacji do najnowszej kątową, platformy React i reagują z Redux szablonów, które można zainstalować ręcznie do składnika ASP.NET 2.0 Core.</span><span class="sxs-lookup"><span data-stu-id="5eb34-106">This documentation is for the latest Angular, React, and React with Redux templates, which can be installed manually into ASP.NET Core 2.0.</span></span> <span data-ttu-id="5eb34-107">Szablony znajdują się domyślnie z platformy ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="5eb34-107">The templates are included by default with ASP.NET Core 2.1.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5eb34-108">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="5eb34-108">Prerequisites</span></span>

* <span data-ttu-id="5eb34-109">[Zestaw SDK programu .NET core](https://www.microsoft.com/net/download), wersji 2.0.0 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="5eb34-109">[.NET Core SDK](https://www.microsoft.com/net/download), version 2.0.0 or later</span></span>
* <span data-ttu-id="5eb34-110">[Node.js](https://nodejs.org), w wersji 6 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="5eb34-110">[Node.js](https://nodejs.org), version 6 or later</span></span>

## <a name="installation"></a><span data-ttu-id="5eb34-111">Instalacja</span><span class="sxs-lookup"><span data-stu-id="5eb34-111">Installation</span></span>

<span data-ttu-id="5eb34-112">Jeśli masz składnika ASP.NET 2.0 Core, uruchom następujące polecenie, aby zainstalować zaktualizowanych szablonów platformy ASP.NET Core dla kątową reagować i reagować z Redux:</span><span class="sxs-lookup"><span data-stu-id="5eb34-112">If you have ASP.NET Core 2.0, run the following command to install the updated ASP.NET Core templates for Angular, React, and React with Redux:</span></span>

```console
dotnet new --install Microsoft.DotNet.Web.Spa.ProjectTemplates::2.0.0
```

## <a name="use-the-templates"></a><span data-ttu-id="5eb34-113">Za pomocą szablonów</span><span class="sxs-lookup"><span data-stu-id="5eb34-113">Use the templates</span></span>

- [<span data-ttu-id="5eb34-114">Należy użyć szablonu projektu dyrektywy Angular</span><span class="sxs-lookup"><span data-stu-id="5eb34-114">Use the Angular project template</span></span>](xref:spa/angular)
- [<span data-ttu-id="5eb34-115">Należy użyć szablonu projektu w bibliotece React.</span><span class="sxs-lookup"><span data-stu-id="5eb34-115">Use the React project template</span></span>](xref:spa/react)
- [<span data-ttu-id="5eb34-116">Użyj platformy React z szablonem projektu Redux</span><span class="sxs-lookup"><span data-stu-id="5eb34-116">Use the React with Redux project template</span></span>](xref:spa/react-with-redux)
