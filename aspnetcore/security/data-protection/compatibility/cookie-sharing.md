---
title: "Udostępnianie plików cookie między aplikacjami"
author: rick-anderson
description: "W tym dokumencie opisano, jak stosu ochrony danych obsługuje udostępnianie plików cookie uwierzytelniania między ASP.NET 4.x i aplikacje platformy ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/cookie-sharing
ms.openlocfilehash: 0cbf5a3e9dfe8f99433800ac5c10ed36b4de6527
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="sharing-cookies-between-applications"></a>Udostępnianie plików cookie między aplikacjami

Witryny sieci Web często zawierają wiele aplikacji sieci web poszczególnych wszystkie pracy ze sobą harmonijnego. Jeśli Deweloper aplikacji chce zapewnić dobrą środowisko single-sign-on, będzie często muszą wszystkie aplikacje innej witryny sieci web w witrynie udostępnianie biletów uwierzytelniania między sobą.

Z tym scenariuszem stosu ochrony danych umożliwia udostępnianie Katana pliku cookie uwierzytelniania i biletów uwierzytelniania pliku cookie platformy ASP.NET Core.

## <a name="sharing-authentication-cookies-between-applications"></a>Udostępnianie plików cookie uwierzytelniania między aplikacjami

Aby udostępnić pliki cookie uwierzytelniania między dwóch różnych aplikacji platformy ASP.NET Core, skonfiguruj każdej aplikacji, które należy udostępnić pliki cookie w następujący sposób.

W Twojej skonfigurować metodę, użycie CookieAuthenticationOptions do skonfigurowania usługi ochrony danych dla plików cookie i AuthenticationScheme odpowiadające ASP.NET 4.x.

Jeśli używasz tożsamości:

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = "ApplicationCookie";
    var protectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"));
    options.Cookies.ApplicationCookie.DataProtectionProvider = protectionProvider;
    options.Cookies.ApplicationCookie.TicketDataFormat = new TicketDataFormat(protectionProvider.CreateProtector("Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", "Cookies", "v2"));
});
```

Jeśli używasz plików cookie bezpośrednio:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(new DirectoryInfo(@"c:\shared-auth-ticket-keys\"))
});
```
   
`DataProtectionProvider` Wymaga `Microsoft.AspNetCore.DataProtection.Extensions` pakietu NuGet.

Gdy są używane w ten sposób, DirectoryInfo powinny wskazywać lokalizacji magazynu kluczy, w szczególności przeznaczoną dla plików cookie uwierzytelniania. Oprogramowanie pośredniczące uwierzytelniania plików cookie użyje jawnie podana wykonania DataProtectionProvider, która jest teraz odizolowanych od systemu ochrony danych, które są używane przez inne części aplikacji. Nazwa aplikacji jest ignorowany (celowo tak, ponieważ próbujesz uzyskać wiele aplikacji do udostępniania ładunków).

>[!WARNING]
>Należy rozważyć skonfigurowanie DataProtectionProvider w taki sposób, że klucze są szyfrowane, gdy, podobnie jak w poniższym przykładzie.
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

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-applications"></a>Udostępnianie plików cookie uwierzytelniania między ASP.NET 4.x i aplikacje platformy ASP.NET Core

Aplikacje ASP.NET 4.x, korzystających z oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana można skonfigurować do generowania plików cookie uwierzytelniania, które są zgodne z oprogramowaniem pośredniczącym uwierzytelniania pliku cookie platformy ASP.NET Core. Dzięki temu uaktualniania dużej witryny poszczególnych aplikacji wyrywkowo zapewniając smooth rejestracji jednokrotnej w środowisku całej witryny.

>[!TIP]
> Można określić, czy istniejąca aplikacja używa oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana istnienie wywołanie UseCookieAuthentication w Startup.Auth.cs Twojego projektu. Projekty aplikacji sieci web programu ASP.NET 4.x utworzone za pomocą programu Visual Studio 2013, a następnie użyć oprogramowania pośredniczącego uwierzytelniania pliku cookie Katana domyślnie.

> [!NOTE]
> Aplikacja ASP.NET 4.x musi wskazywać na .NET Framework 4.5.1 lub nowszego, w przeciwnym razie niezbędne pakiety NuGet nie zostanie zainstalowana.

Aby udostępnić pliki cookie uwierzytelniania między aplikacje ASP.NET 4.x i aplikacje platformy ASP.NET Core, konfigurowania aplikacji platformy ASP.NET Core, jak wspomniano powyżej, a następnie konfigurowania aplikacji ASP.NET 4.x, wykonując poniższe kroki.

1.  Zainstaluj pakiet Microsoft.Owin.Security.Interop do poszczególnych aplikacji ASP.NET 4.x.

2.   W Startup.Auth.cs Znajdź wywołanie UseCookieAuthentication, które zazwyczaj będzie mieć następującą postać.

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        // ...
    });
    ```
    
3.  Zmodyfikuj wywołanie UseCookieAuthentication w następujący sposób, zmiana nazwę CookieName, aby dopasować nazwę używaną przez platformy ASP.NET Core pliku cookie oprogramowanie pośredniczące uwierzytelniania, zapewniając wystąpienia DataProtectionProvider, który został zainicjowany do lokalizacji magazynu kluczy i wartości CookieManager międzyoperacyjnego element ChunkingCookieManager co format segmentu jest zgodny.

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
    DirectoryInfo musi wskazać tej samej lokalizacji magazynu, która odnosi się do aplikacji platformy ASP.NET Core i powinna być skonfigurowana przy użyciu tych samych ustawień.

ASP.NET 4.x i aplikacje platformy ASP.NET Core są teraz skonfigurowane do udziału plików cookie uwierzytelniania.

> [!NOTE]
> Należy się upewnić, że w tej samej bazy danych użytkownika jest wskazywana systemu tożsamości dla każdej aplikacji. W przeciwnym razie systemu tożsamości powodują błędy w czasie wykonywania, przy próbie zgodny z informacjami w pliku cookie uwierzytelniania względem informacji w bazie danych.
