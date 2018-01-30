---
title: "Migrowanie uwierzytelnianie i tożsamość"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/identity
ms.openlocfilehash: f02d9472ea0aa1dceae3f53c812776aab85ab54e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-authentication-and-identity"></a><span data-ttu-id="2d824-102">Migrowanie uwierzytelnianie i tożsamość</span><span class="sxs-lookup"><span data-stu-id="2d824-102">Migrating Authentication and Identity</span></span>

<a name="migration-identity"></a>

<span data-ttu-id="2d824-103">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="2d824-103">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="2d824-104">W poprzednim artykule firma Microsoft [migracji konfiguracji z projektu programu ASP.NET MVC do platformy ASP.NET Core MVC](configuration.md).</span><span class="sxs-lookup"><span data-stu-id="2d824-104">In the previous article we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](configuration.md).</span></span> <span data-ttu-id="2d824-105">W tym artykule będziemy migrować funkcji zarządzania rejestracji, logowania i użytkownika.</span><span class="sxs-lookup"><span data-stu-id="2d824-105">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-identity-and-membership"></a><span data-ttu-id="2d824-106">Konfiguruj tożsamość i członkostwa</span><span class="sxs-lookup"><span data-stu-id="2d824-106">Configure Identity and Membership</span></span>

<span data-ttu-id="2d824-107">Na platformie ASP.NET MVC uwierzytelnianie i tożsamość funkcje są skonfigurowane przy użyciu tożsamości ASP.NET Startup.Auth.cs i IdentityConfig.cs znajdujące się w folderze App_Start.</span><span class="sxs-lookup"><span data-stu-id="2d824-107">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in Startup.Auth.cs and IdentityConfig.cs, located in the App_Start folder.</span></span> <span data-ttu-id="2d824-108">W programie ASP.NET MVC Core, te funkcje są konfigurowane w *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="2d824-108">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="2d824-109">Zainstaluj `Microsoft.AspNetCore.Identity.EntityFrameworkCore` i `Microsoft.AspNetCore.Authentication.Cookies` pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="2d824-109">Install the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Authentication.Cookies` NuGet packages.</span></span>

<span data-ttu-id="2d824-110">Następnie należy otworzyć pliku Startup.cs i aktualizacji `ConfigureServices()` sposób korzystania z programu Entity Framework i tożsamość usług:</span><span class="sxs-lookup"><span data-stu-id="2d824-110">Then, open Startup.cs and update the `ConfigureServices()` method to use Entity Framework and Identity services:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
  // Add EF services to the services container.
  services.AddEntityFramework(Configuration)
    .AddSqlServer()
    .AddDbContext<ApplicationDbContext>();

  // Add Identity services to the services container.
  services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
    .AddEntityFrameworkStores<ApplicationDbContext>();

  services.AddMvc();
}
```

<span data-ttu-id="2d824-111">W tym momencie istnieją dwa typy, do którego odwołuje się powyżej kod, który firma Microsoft nie zostały jeszcze zmigrowane z projektu programu ASP.NET MVC: `ApplicationDbContext` i `ApplicationUser`.</span><span class="sxs-lookup"><span data-stu-id="2d824-111">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="2d824-112">Utwórz nową *modele* folderu w ASP.NET Core projektu, a następnie dodaj dwie klasy do niego odpowiednie do tych typów.</span><span class="sxs-lookup"><span data-stu-id="2d824-112">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="2d824-113">ASP.NET MVC znajdzie wersji tych klas w `/Models/IdentityModels.cs`, ale używamy jednego pliku na klasę w projekcie migrowane, ponieważ jest podejmowanie.</span><span class="sxs-lookup"><span data-stu-id="2d824-113">You will find the ASP.NET MVC versions of these classes in `/Models/IdentityModels.cs`, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="2d824-114">ApplicationUser.cs:</span><span class="sxs-lookup"><span data-stu-id="2d824-114">ApplicationUser.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvc6Project.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="2d824-115">ApplicationDbContext.cs:</span><span class="sxs-lookup"><span data-stu-id="2d824-115">ApplicationDbContext.cs:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFramework;
using Microsoft.Data.Entity;

namespace NewMvc6Project.Models
{
  public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
  {
    public ApplicationDbContext()
    {
      Database.EnsureCreated();
    }

    protected override void OnConfiguring(DbContextOptionsBuilder options)
    {
      options.UseSqlServer();
    }
  }
}
```

<span data-ttu-id="2d824-116">Projekt sieci Web platformy ASP.NET Core MVC Starter nie zawiera wiele dostosowanie użytkowników lub ApplicationDbContext.</span><span class="sxs-lookup"><span data-stu-id="2d824-116">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the ApplicationDbContext.</span></span> <span data-ttu-id="2d824-117">Podczas migrowania rzeczywistej aplikacji, należy również do migracji wszystkie niestandardowe właściwości i metod użytkownika aplikacji i klasy DbContext, jak również inne klasy modelu, który korzysta z aplikacji (na przykład, jeśli Twoje DbContext ma DbSet<Album>, oczywiście należy migrować albumów klasy).</span><span class="sxs-lookup"><span data-stu-id="2d824-117">When migrating a real application, you will also need to migrate all of the custom properties and methods of your application's user and DbContext classes, as well as any other Model classes your application utilizes (for example, if your DbContext has a DbSet<Album>, you will of course need to migrate the Album class).</span></span>

<span data-ttu-id="2d824-118">Z tych plików w miejscu pliku Startup.cs może się skompilować, aktualizując za jego pomocą instrukcje:</span><span class="sxs-lookup"><span data-stu-id="2d824-118">With these files in place, the Startup.cs file can be made to compile by updating its using statements:</span></span>

```csharp
using Microsoft.Framework.ConfigurationModel;
using Microsoft.AspNetCore.Hosting;
using NewMvc6Project.Models;
using Microsoft.AspNetCore.Identity;
```

<span data-ttu-id="2d824-119">Naszej aplikacji jest teraz gotowy do obsługi uwierzytelniania i tożsamość usług — tylko musi mieć te funkcje uwidoczniona dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="2d824-119">Our application is now ready to support authentication and identity services - it just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="2d824-120">Migrowanie rejestracji i logowania logiki</span><span class="sxs-lookup"><span data-stu-id="2d824-120">Migrate Registration and Login Logic</span></span>

<span data-ttu-id="2d824-121">Z usługi tożsamości aplikacji i dostępu do danych konfiguracji przy użyciu programu Entity Framework i programu SQL Server możemy teraz przystąpić do dodać obsługę rejestracji i logowania do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="2d824-121">With identity services configured for the application and data access configured using Entity Framework and SQL Server, we are now ready to add support for registration and login to the application.</span></span> <span data-ttu-id="2d824-122">Odwołania, który [wcześniej w procesie migracji](mvc.md#migrate-layout-file) możemy oznaczone jako komentarz odwołanie do _LoginPartial w _Layout.cshtml.</span><span class="sxs-lookup"><span data-stu-id="2d824-122">Recall that [earlier in the migration process](mvc.md#migrate-layout-file) we commented out a reference to _LoginPartial in _Layout.cshtml.</span></span> <span data-ttu-id="2d824-123">Teraz nadszedł czas na powrót do tego kodu, Usuń komentarz, a następnie dodaj wymagane kontrolery i widoki do obsługi funkcji logowania.</span><span class="sxs-lookup"><span data-stu-id="2d824-123">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="2d824-124">Aktualizacja _Layout.cshtml; Usuń znaczniki komentarza @Html.Partial wiersza:</span><span class="sxs-lookup"><span data-stu-id="2d824-124">Update _Layout.cshtml; uncomment the @Html.Partial line:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="2d824-125">Teraz Dodaj nowe strona widoku MVC wywołuje _LoginPartial do folderu widoki/Shared:</span><span class="sxs-lookup"><span data-stu-id="2d824-125">Now, add a new MVC View Page called _LoginPartial to the Views/Shared folder:</span></span>

<span data-ttu-id="2d824-126">Zaktualizuj _LoginPartial.cshtml następującym kodem (Zastąp całą jego zawartość):</span><span class="sxs-lookup"><span data-stu-id="2d824-126">Update _LoginPartial.cshtml with the following code (replace all of its contents):</span></span>

```cshtml
@inject SignInManager<User> SignInManager
@inject UserManager<User> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="LogOff" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log off</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

<span data-ttu-id="2d824-127">W tym momencie można odświeżyć witrynę w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="2d824-127">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="2d824-128">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="2d824-128">Summary</span></span>

<span data-ttu-id="2d824-129">Platformy ASP.NET Core wprowadzono zmiany do funkcji ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="2d824-129">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="2d824-130">W tym artykule jak już wspomniano sposób migracji uwierzytelniania i użytkownika funkcji do zarządzania tożsamością ASP.NET do platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2d824-130">In this article, you have seen how to migrate the authentication and user management features of an ASP.NET Identity to ASP.NET Core.</span></span>
