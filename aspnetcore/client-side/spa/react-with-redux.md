---
title: Szablon projektu platformy React z Redux za pomocą platformy ASP.NET Core
author: SteveSandersonMS
description: Dowiedz się, jak rozpocząć pracę przy użyciu szablonu projektu ASP.NET Core jednej strony aplikacji (SPA) dla platformy React z kontenera Redux i utworzyć react aplikacji.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: c1aedcf1e14a9e7b339b60dd02c4267cd5945a49
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341623"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="a2efb-103">Szablon projektu platformy React z Redux za pomocą platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2efb-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="a2efb-104">Ta dokumentacja nie odnoszą się do szablonu projektu platformy React z Redux, zawarte w ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a2efb-104">This documentation doesn't pertain to the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="a2efb-105">Odnoszą się do nowszej platformy React z Redux szablonu, który można aktualizować ręcznie.</span><span class="sxs-lookup"><span data-stu-id="a2efb-105">It pertains to the newer React-with-Redux template that you can update manually.</span></span> <span data-ttu-id="a2efb-106">Szablon jest dostępna w programie ASP.NET Core 2.1 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="a2efb-106">The template is available in ASP.NET Core 2.1 or later.</span></span>

::: moniker-end

<span data-ttu-id="a2efb-107">Zaktualizowany szablon projektu platformy React z kontenera Redux udostępnia dogodny punkt początkowy dla aplikacji platformy ASP.NET Core przy użyciu React i kontenera Redux, i [tworzenie platformy react aplikacji](https://github.com/facebookincubator/create-react-app) konwencje (CRA), aby zaimplementować interfejs rozbudowane, po stronie klienta użytkownika (UI).</span><span class="sxs-lookup"><span data-stu-id="a2efb-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="a2efb-108">Z wyjątkiem polecenia tworzenia projektu wszystkie informacje dotyczące platformy React z Redux szablonu jest taka sama jak w szablonie platformy React.</span><span class="sxs-lookup"><span data-stu-id="a2efb-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="a2efb-109">Aby utworzyć ten typ projektu, uruchom `dotnet new reactredux` zamiast `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="a2efb-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="a2efb-110">Aby uzyskać więcej informacji na temat funkcji, które są wspólne dla obu szablonów opartych na platformy React, zobacz [React dokumentacji dotyczącej szablonu](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="a2efb-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>

<span data-ttu-id="a2efb-111">Aby uzyskać informacji o konfigurowaniu platformy React z Redux aplikacji podrzędnych w usługach IIS, zobacz [2.1 szablonu reactredux dla platformy: Nie można użyć SPA BIURA w usługach IIS (aspnet/szablonów &num;555)](https://github.com/aspnet/Templating/issues/555).</span><span class="sxs-lookup"><span data-stu-id="a2efb-111">For information on configuring a React-with-Redux sub-application in IIS, see [ReactRedux Template 2.1: Unable to use SPA on IIS (aspnet/Templating &num;555)](https://github.com/aspnet/Templating/issues/555).</span></span>
