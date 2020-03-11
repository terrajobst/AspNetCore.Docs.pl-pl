---
title: Wstrzykiwanie zależności w programach obsługi wymagań w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak wstrzyknąć programy obsługi wymagań autoryzacji do aplikacji ASP.NET Core przy użyciu iniekcji zależności.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666090"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a><span data-ttu-id="6480f-103">Wstrzykiwanie zależności w programach obsługi wymagań w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6480f-103">Dependency injection in requirement handlers in ASP.NET Core</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="6480f-104">[Procedury obsługi autoryzacji muszą być zarejestrowane](xref:security/authorization/policies#handler-registration) w kolekcji usług podczas konfiguracji (przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="6480f-104">[Authorization handlers must be registered](xref:security/authorization/policies#handler-registration) in the service collection during configuration (using [dependency injection](xref:fundamentals/dependency-injection)).</span></span>

<span data-ttu-id="6480f-105">Załóżmy, że masz repozytorium reguł, które chcesz oszacować wewnątrz procedury obsługi autoryzacji i że repozytorium zostało zarejestrowane w kolekcji usług.</span><span class="sxs-lookup"><span data-stu-id="6480f-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="6480f-106">Autoryzacja zostanie rozwiązany i wstrzyknąć do konstruktora.</span><span class="sxs-lookup"><span data-stu-id="6480f-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="6480f-107">Na przykład jeśli chciałeś użyć ASP. Infrastruktura rejestrowania w sieci, którą chcesz wstrzyknąć `ILoggerFactory` do procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="6480f-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="6480f-108">Taka procedura obsługi może wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="6480f-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="6480f-109">Procedurę obsługi należy zarejestrować w `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="6480f-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="6480f-110">Wystąpienie programu obsługi zostanie utworzone, gdy aplikacja zostanie uruchomiona, a funkcja DI sprawdzi zarejestrowane `ILoggerFactory` w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="6480f-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="6480f-111">Programy obsługi, które używają Entity Framework nie powinny być rejestrowane jako pojedyncze.</span><span class="sxs-lookup"><span data-stu-id="6480f-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
