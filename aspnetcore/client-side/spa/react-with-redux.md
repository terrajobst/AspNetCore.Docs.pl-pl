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
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a>Szablon projektu platformy React z Redux za pomocą platformy ASP.NET Core

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Ta dokumentacja nie odnoszą się do szablonu projektu platformy React z Redux, zawarte w ASP.NET Core 2.0. Odnoszą się do nowszej platformy React z Redux szablonu, który można aktualizować ręcznie. Szablon jest dostępna w programie ASP.NET Core 2.1 lub nowszej.

::: moniker-end

Zaktualizowany szablon projektu platformy React z kontenera Redux udostępnia dogodny punkt początkowy dla aplikacji platformy ASP.NET Core przy użyciu React i kontenera Redux, i [tworzenie platformy react aplikacji](https://github.com/facebookincubator/create-react-app) konwencje (CRA), aby zaimplementować interfejs rozbudowane, po stronie klienta użytkownika (UI).

Z wyjątkiem polecenia tworzenia projektu wszystkie informacje dotyczące platformy React z Redux szablonu jest taka sama jak w szablonie platformy React. Aby utworzyć ten typ projektu, uruchom `dotnet new reactredux` zamiast `dotnet new react`. Aby uzyskać więcej informacji na temat funkcji, które są wspólne dla obu szablonów opartych na platformy React, zobacz [React dokumentacji dotyczącej szablonu](xref:spa/react).

Aby uzyskać informacji o konfigurowaniu platformy React z Redux aplikacji podrzędnych w usługach IIS, zobacz [2.1 szablonu reactredux dla platformy: Nie można użyć SPA BIURA w usługach IIS (aspnet/szablonów &num;555)](https://github.com/aspnet/Templating/issues/555).
