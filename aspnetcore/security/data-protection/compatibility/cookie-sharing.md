---
title: "Udostępnianie plików cookie między aplikacjami"
author: rick-anderson
description: "W tym dokumencie opisano, jak stosu ochrony danych obsługuje udostępnianie plików cookie uwierzytelniania między ASP.NET 4.x i aplikacje platformy ASP.NET Core."
keywords: "Udostępnianie Core,ASP.NET,cookies,Interop,cookie ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 9a7aae45-8e21-4c54-950c-3c29df6c1082
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: e92cc81f9362787b7b4bfb44ba26b82182826a59
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="sharing-cookies-between-applications"></a><span data-ttu-id="975f9-104">Udostępnianie plików cookie między aplikacjami</span><span class="sxs-lookup"><span data-stu-id="975f9-104">Sharing cookies between applications</span></span>

<span data-ttu-id="975f9-105">Witryny sieci Web często zawierają wiele aplikacji sieci web poszczególnych wszystkie pracy ze sobą harmonijnego.</span><span class="sxs-lookup"><span data-stu-id="975f9-105">Web sites commonly consist of many individual web applications, all working together harmoniously.</span></span> <span data-ttu-id="975f9-106">Jeśli Deweloper aplikacji chce zapewnić dobrą środowisko single-sign-on, będzie często muszą wszystkie aplikacje innej witryny sieci web w witrynie udostępnianie biletów uwierzytelniania między sobą.</span><span class="sxs-lookup"><span data-stu-id="975f9-106">If an application developer wants to provide a good single-sign-on experience, they'll often need all of the different web applications within the site to share authentication tickets between each other.</span></span>

<span data-ttu-id="975f9-107">Z tym scenariuszem stosu ochrony danych umożliwia udostępnianie Katana pliku cookie uwierzytelniania i biletów uwierzytelniania pliku cookie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="975f9-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

## <a name="sharing-authentication-cookies-between-applications"></a><span data-ttu-id="975f9-108">Udostępnianie plików cookie uwierzytelniania między aplikacjami</span><span class="sxs-lookup"><span data-stu-id="975f9-108">Sharing authentication cookies between applications</span></span>

<span data-ttu-id="975f9-109">Aby udostępnić pliki cookie uwierzytelniania między dwóch różnych aplikacji platformy ASP.NET Core, skonfiguruj każdej aplikacji, które należy udostępnić pliki cookie w następujący sposób.</span><span class="sxs-lookup"><span data-stu-id="975f9-109">To share authentication cookies between two different ASP.NET Core applications, configure each application that should share cookies as follows.</span></span>

<span data-ttu-id="975f9-110">W Twojej skonfigurować metodę, użycie CookieAuthenticationOptions do skonfigurowania usługi ochrony danych dla plików cookie i AuthenticationScheme odpowiadające ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="975f9-110">In your configure method, use the CookieAuthenticationOptions to set up the data protection service for cookies and the AuthenticationScheme to match ASP.NET 4.x.</span></span>

<span data-ttu-id="975f9-111">Jeśli używasz tożsamości:</span><span class="sxs-lookup"><span data-stu-id="975f9-111">If you're using identity:</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
    var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
    options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
    options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
});
```

<span data-ttu-id="975f9-112">Jeśli używasz plików cookie bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="975f9-112">If you're using cookies directly:</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
});
```
   
<span data-ttu-id="975f9-113">`DataProtectionProvider` Wymaga `Microsoft.AspNetCore.DataProtection.Extensions` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="975f9-113">The `DataProtectionProvider` requires the `Microsoft.AspNetCore.DataProtection.Extensions` NuGet package.</span></span>

<span data-ttu-id="975f9-114">Gdy są używane w ten sposób, DirectoryInfo powinny wskazywać lokalizacji magazynu kluczy, w szczególności przeznaczoną dla plików cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="975f9-114">When used in this manner, the DirectoryInfo should point to a key storage location specifically set aside for authentication cookies.</span></span> <span data-ttu-id="975f9-115">Oprogramowanie pośredniczące uwierzytelniania plików cookie użyje jawnie podana wykonania DataProtectionProvider, która jest teraz odizolowanych od systemu ochrony danych, które są używane przez inne części aplikacji.</span><span class="sxs-lookup"><span data-stu-id="975f9-115">The cookie authentication middleware will use the explicitly provided implementation of the DataProtectionProvider, which is now isolated from the data protection system used by other parts of the application.</span></span> <span data-ttu-id="975f9-116">Nazwa aplikacji jest ignorowany (celowo tak, ponieważ próbujesz uzyskać wiele aplikacji do udostępniania ładunków).</span><span class="sxs-lookup"><span data-stu-id="975f9-116">The application name is ignored (intentionally so, since you're trying to get multiple applications to share payloads).</span></span>

>[!WARNING]
><span data-ttu-id="975f9-117">Należy rozważyć skonfigurowanie DataProtectionProvider w taki sposób, że klucze są szyfrowane, gdy, podobnie jak w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="975f9-117">You should consider configuring the DataProtectionProvider such that keys are encrypted at rest, as in the below example.</span></span>
>
>
>  ```csharp
>  app.UseCookieAuthentication(new CookieAuthenticationOptions
>  {
>      DataProtectionProvider = DataProtectionProvider.Create(
>          new DirectoryInfo(@"c:\shared-auth-ticket-keys\"),
>          configure =>
>          {
>              configure.ProtectKeysWithCertificate("thumbprint");
>          })
>  });
>  ```

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a><span data-ttu-id="975f9-118">Udostępnianie plików cookie uwierzytelniania między ASP.NET 4.x i aplikacje platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="975f9-118">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core applications</span></span>

<span data-ttu-id="975f9-119">Aplikacje ASP.NET 4.x, korzystających z oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana można skonfigurować do generowania plików cookie uwierzytelniania, które są zgodne z oprogramowaniem pośredniczącym uwierzytelniania pliku cookie platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="975f9-119">ASP.NET 4.x applications which use Katana cookie authentication middleware can be configured to generate authentication cookies which are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="975f9-120">Dzięki temu uaktualniania dużej witryny poszczególnych aplikacji wyrywkowo zapewniając smooth rejestracji jednokrotnej w środowisku całej witryny.</span><span class="sxs-lookup"><span data-stu-id="975f9-120">This allows upgrading a large site's individual applications piecemeal while still providing a smooth single sign on experience across the site.</span></span>

>[!TIP]
> <span data-ttu-id="975f9-121">Można określić, czy istniejąca aplikacja używa oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana istnienie wywołanie UseCookieAuthentication w Startup.Auth.cs Twojego projektu.</span><span class="sxs-lookup"><span data-stu-id="975f9-121">You can tell if your existing application uses Katana cookie authentication middleware by the existence of a call to UseCookieAuthentication in your project's Startup.Auth.cs.</span></span> <span data-ttu-id="975f9-122">Projekty aplikacji sieci web programu ASP.NET 4.x utworzone za pomocą programu Visual Studio 2013, a następnie użyć oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana domyślnie.</span><span class="sxs-lookup"><span data-stu-id="975f9-122">ASP.NET 4.x web application projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="975f9-123">Aplikacja ASP.NET 4.x musi wskazywać na .NET Framework 4.5.1 lub nowszego, w przeciwnym razie niezbędne pakiety NuGet nie zostanie zainstalowana.</span><span class="sxs-lookup"><span data-stu-id="975f9-123">Your ASP.NET 4.x application must target .NET Framework 4.5.1 or higher, otherwise the necessary NuGet packages will fail to install.</span></span>

<span data-ttu-id="975f9-124">Aby udostępnić pliki cookie uwierzytelniania między aplikacje ASP.NET 4.x i aplikacje platformy ASP.NET Core, konfigurowania aplikacji platformy ASP.NET Core, jak wspomniano powyżej, a następnie konfigurowania aplikacji ASP.NET 4.x, wykonując poniższe kroki.</span><span class="sxs-lookup"><span data-stu-id="975f9-124">To share authentication cookies between your ASP.NET 4.x applications and your ASP.NET Core applications, configure the ASP.NET Core application as stated above, then configure your ASP.NET 4.x applications by following the steps below.</span></span>

1.  <span data-ttu-id="975f9-125">Zainstaluj pakiet Microsoft.Owin.Security.Interop do poszczególnych aplikacji ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="975f9-125">Install the package Microsoft.Owin.Security.Interop into each of your ASP.NET 4.x applications.</span></span>

2.   <span data-ttu-id="975f9-126">W Startup.Auth.cs Znajdź wywołanie UseCookieAuthentication, które zazwyczaj będzie mieć następującą postać.</span><span class="sxs-lookup"><span data-stu-id="975f9-126">In Startup.Auth.cs, locate the call to UseCookieAuthentication, which will generally look like the following.</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  <span data-ttu-id="975f9-127">Zmodyfikuj wywołanie UseCookieAuthentication w następujący sposób, zmiana nazwę CookieName, aby dopasować nazwę używaną przez platformy ASP.NET Core pliku cookie oprogramowanie pośredniczące uwierzytelniania, zapewniając wystąpienia DataProtectionProvider, który został zainicjowany do lokalizacji magazynu kluczy i wartości CookieManager międzyoperacyjnego element ChunkingCookieManager co format segmentu jest zgodny.</span><span class="sxs-lookup"><span data-stu-id="975f9-127">Modify the call to UseCookieAuthentication as follows, changing the CookieName to match the name used by the ASP.NET Core cookie authentication middleware, providing an instance of a DataProtectionProvider that has been initialized to a key storage location, and set CookieManager to interop ChunkingCookieManager so the chunking format is compatible.</span></span>

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
        CookieName = ".AspNetCore.Cookies",
        // CookieName = ".AspNetCore.ApplicationCookie", (if you're using identity)
        // CookiePath = "...", (if necessary)
        // ...
        TicketDataFormat = new AspNetTicketDataFormat(
            new DataProtectorShim(
                DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
                .CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", "v2"))),
        CookieManager = new ChunkingCookieManager()
    });
    ```
    <span data-ttu-id="975f9-128">DirectoryInfo musi wskazać tej samej lokalizacji magazynu, która odnosi się do aplikacji platformy ASP.NET Core i powinna być skonfigurowana przy użyciu tych samych ustawień.</span><span class="sxs-lookup"><span data-stu-id="975f9-128">The DirectoryInfo has to point to the same storage location that you pointed your ASP.NET Core application to and should be configured using the same settings.</span></span>

<span data-ttu-id="975f9-129">ASP.NET 4.x i aplikacje platformy ASP.NET Core są teraz skonfigurowane do udziału plików cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="975f9-129">The ASP.NET 4.x and ASP.NET Core applications are now configured to share authentication cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="975f9-130">Należy się upewnić, że w tej samej bazy danych użytkownika jest wskazywana systemu tożsamości dla każdej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="975f9-130">You'll need to make sure that the identity system for each application is pointed at the same user database.</span></span> <span data-ttu-id="975f9-131">W przeciwnym razie systemu tożsamości powodują błędy w czasie wykonywania, przy próbie zgodny z informacjami w pliku cookie uwierzytelniania względem informacji w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="975f9-131">Otherwise the identity system will produce failures at runtime when it tries to match the information in the authentication cookie against the information in its database.</span></span>
