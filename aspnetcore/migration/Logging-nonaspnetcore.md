---
title: Migrowanie z Microsoft. Extensions. Logging 2,1 do 2,2 lub 3,0
author: pakrym
description: Dowiedz się, jak przeprowadzić migrację aplikacji non-ASP.NET Core, która korzysta z Microsoft. Extensions. rejestrowanie z 2,1 do 2,2 lub 3,0.
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659321"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a><span data-ttu-id="b18a2-103">Migrowanie z Microsoft. Extensions. Logging 2,1 do 2,2 lub 3,0</span><span class="sxs-lookup"><span data-stu-id="b18a2-103">Migrate from Microsoft.Extensions.Logging 2.1 to 2.2 or 3.0</span></span>

<span data-ttu-id="b18a2-104">W tym artykule opisano typowe kroki migrowania aplikacji non-ASP.NET Core, która używa `Microsoft.Extensions.Logging` od 2,1 do 2,2 lub 3,0.</span><span class="sxs-lookup"><span data-stu-id="b18a2-104">This article outlines the common steps for migrating a non-ASP.NET Core application that uses `Microsoft.Extensions.Logging` from 2.1 to 2.2 or 3.0.</span></span>

## <a name="21-to-22"></a><span data-ttu-id="b18a2-105">Z wersji 2.1 do 2.2</span><span class="sxs-lookup"><span data-stu-id="b18a2-105">2.1 to 2.2</span></span>

<span data-ttu-id="b18a2-106">Ręcznie Utwórz `ServiceCollection` i Wywołaj `AddLogging`.</span><span class="sxs-lookup"><span data-stu-id="b18a2-106">Manually create `ServiceCollection` and call `AddLogging`.</span></span>

<span data-ttu-id="b18a2-107">2,1 przykład:</span><span class="sxs-lookup"><span data-stu-id="b18a2-107">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="b18a2-108">2,2 przykład:</span><span class="sxs-lookup"><span data-stu-id="b18a2-108">2.2 example:</span></span>

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a><span data-ttu-id="b18a2-109">2,1 do 3,0</span><span class="sxs-lookup"><span data-stu-id="b18a2-109">2.1 to 3.0</span></span>

<span data-ttu-id="b18a2-110">W 3,0 użyj `LoggingFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="b18a2-110">In 3.0, use `LoggingFactory.Create`.</span></span>

<span data-ttu-id="b18a2-111">2,1 przykład:</span><span class="sxs-lookup"><span data-stu-id="b18a2-111">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="b18a2-112">3,0 przykład:</span><span class="sxs-lookup"><span data-stu-id="b18a2-112">3.0 example:</span></span>

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a><span data-ttu-id="b18a2-113">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="b18a2-113">Additional resources</span></span>

<xref:fundamentals/logging/index>