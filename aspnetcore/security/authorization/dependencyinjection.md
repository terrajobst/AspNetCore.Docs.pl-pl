---
title: "Iniekcji zależności w obsłudze wymaganie"
author: rick-anderson
description: "W tym dokumencie przedstawiono sposób dodanie obsługi wymagań autoryzacji do aplikacji platformy ASP.NET Core za pomocą iniekcji zależności."
keywords: "Platformy ASP.NET Core iniekcji zależności, program obsługi autoryzacji"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5fb6625c-173a-4feb-8380-73c9844dc23c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: b5e590cc63387553af7385b611cdf8cd6b255db7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="dependency-injection-in-requirement-handlers"></a><span data-ttu-id="ddb90-104">Iniekcji zależności w obsłudze wymaganie</span><span class="sxs-lookup"><span data-stu-id="ddb90-104">Dependency Injection in requirement handlers</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="ddb90-105">[Programy obsługi autoryzacji musi być zarejestrowana](policies.md#handler-registration) w kolekcji usługi podczas konfigurowania (przy użyciu [iniekcji zależności](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="ddb90-105">[Authorization handlers must be registered](policies.md#handler-registration) in the service collection during configuration (using [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="ddb90-106">Załóżmy, że masz repozytorium reguł, który chcesz ocenić wewnątrz obsługi autoryzacji i tego repozytorium został zarejestrowany w kolekcji usługi.</span><span class="sxs-lookup"><span data-stu-id="ddb90-106">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="ddb90-107">Autoryzacja i rozpoznawania iniekcję który do Twojej konstruktora.</span><span class="sxs-lookup"><span data-stu-id="ddb90-107">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="ddb90-108">Na przykład, jeśli chcesz użyć ASP. NET do rejestrowania infrastruktury chcesz wstawić `ILoggerFactory` do programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="ddb90-108">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="ddb90-109">Program obsługi może wyglądać tak:</span><span class="sxs-lookup"><span data-stu-id="ddb90-109">Such a handler might look like:</span></span>

```csharp
public class LoggingAuthorizationHandler : AuthorizationHandler<MyRequirement>
   {
       ILogger _logger;

       public LoggingAuthorizationHandler(ILoggerFactory loggerFactory)
       {
           _logger = loggerFactory.CreateLogger(this.GetType().FullName);
       }

       protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MyRequirement requirement)
       {
           _logger.LogInformation("Inside my handler");
           // Check if the requirement is fulfilled.
           return Task.CompletedTask;
       }
   }
   ```

<span data-ttu-id="ddb90-110">Może zarejestrować programu obsługi z `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="ddb90-110">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="ddb90-111">Wystąpienie zostanie obsługi można utworzyć podczas uruchamiania aplikacji i zostanie Podpisane wstrzyknąć zarejestrowaną `ILoggerFactory` do Twojej konstruktora.</span><span class="sxs-lookup"><span data-stu-id="ddb90-111">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="ddb90-112">Programy obsługi, które używają programu Entity Framework nie powinien być zarejestrowany jako pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="ddb90-112">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
