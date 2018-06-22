---
title: Iniekcji zależności w obsłudze wymaganie w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak dodanie obsługi wymagań autoryzacji do aplikacji platformy ASP.NET Core za pomocą iniekcji zależności.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: c6bb2589c6fef9f4586e6f4ddbb574866e6c48ab
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273725"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a><span data-ttu-id="47452-103">Iniekcji zależności w obsłudze wymaganie w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="47452-103">Dependency injection in requirement handlers in ASP.NET Core</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="47452-104">[Programy obsługi autoryzacji musi być zarejestrowana](xref:security/authorization/policies#handler-registration) w kolekcji usługi podczas konfigurowania (przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="47452-104">[Authorization handlers must be registered](xref:security/authorization/policies#handler-registration) in the service collection during configuration (using [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="47452-105">Załóżmy, że masz repozytorium reguł, który chcesz ocenić wewnątrz obsługi autoryzacji i tego repozytorium został zarejestrowany w kolekcji usługi.</span><span class="sxs-lookup"><span data-stu-id="47452-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="47452-106">Autoryzacja i rozpoznawania iniekcję który do Twojej konstruktora.</span><span class="sxs-lookup"><span data-stu-id="47452-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="47452-107">Na przykład, jeśli chcesz użyć ASP. NET do rejestrowania infrastruktury chcesz wstawić `ILoggerFactory` do programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="47452-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="47452-108">Program obsługi może wyglądać tak:</span><span class="sxs-lookup"><span data-stu-id="47452-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="47452-109">Może zarejestrować programu obsługi z `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="47452-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="47452-110">Wystąpienie zostanie obsługi można utworzyć podczas uruchamiania aplikacji i zostanie Podpisane wstrzyknąć zarejestrowaną `ILoggerFactory` do Twojej konstruktora.</span><span class="sxs-lookup"><span data-stu-id="47452-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="47452-111">Programy obsługi, które używają programu Entity Framework nie powinien być zarejestrowany jako pojedynczych wystąpień.</span><span class="sxs-lookup"><span data-stu-id="47452-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
