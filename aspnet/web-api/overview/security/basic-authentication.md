---
uid: web-api/overview/security/basic-authentication
title: Uwierzytelnianie podstawowe w składniku ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym artykule opisano, w interfejsie API sieci Web ASP.NET przy użyciu uwierzytelniania podstawowego.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4b8e6410668b2db289488bb4b6cd26d881e70f4c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566744"
---
<a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="e0c1b-103">Uwierzytelnianie podstawowe w składniku ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="e0c1b-103">Basic Authentication in ASP.NET Web API</span></span>
====================
<span data-ttu-id="e0c1b-104">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e0c1b-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e0c1b-105">Uwierzytelnianie podstawowe jest zdefiniowany w [RFC 2617, uwierzytelnianie HTTP: Basic i uwierzytelniania szyfrowanego dostępu](http://www.ietf.org/rfc/rfc2617.txt).</span><span class="sxs-lookup"><span data-stu-id="e0c1b-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="e0c1b-106">Wady</span><span class="sxs-lookup"><span data-stu-id="e0c1b-106">Disadvantages</span></span>

- <span data-ttu-id="e0c1b-107">Poświadczenia użytkownika są wysyłane w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="e0c1b-108">Poświadczenia są wysyłane w postaci zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="e0c1b-109">Poświadczenia są wysyłane z każdym żądaniem.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="e0c1b-110">Nie można wylogować się, chyba że przez kończenie sesji przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="e0c1b-111">Narażone na krzyżowych żądania międzywitrynowego (CSRF); wymaga anti-CSRF środków.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="e0c1b-112">Zalety</span><span class="sxs-lookup"><span data-stu-id="e0c1b-112">Advantages</span></span>

- <span data-ttu-id="e0c1b-113">Standardy internetowe.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-113">Internet standard.</span></span>
- <span data-ttu-id="e0c1b-114">Obsługuje wszystkie główne przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="e0c1b-115">Protokół stosunkowo proste.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-115">Relatively simple protocol.</span></span>

<span data-ttu-id="e0c1b-116">Uwierzytelnianie podstawowe działa w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="e0c1b-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="e0c1b-117">Jeśli żądanie wymaga uwierzytelnienia, serwer zwraca 401 (bez autoryzacji).</span><span class="sxs-lookup"><span data-stu-id="e0c1b-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="e0c1b-118">Odpowiedź zawiera nagłówek WWW-Authenticate, wskazujący, że serwer obsługuje uwierzytelnianie podstawowe.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="e0c1b-119">Klient wysyła żądanie innego, przy użyciu poświadczeń klienta w nagłówku autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="e0c1b-120">Poświadczenia są sformatowane jako ciąg "Nazwa: hasło", algorytmem Base64.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="e0c1b-121">Poświadczenia nie są szyfrowane.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="e0c1b-122">Uwierzytelnianie podstawowe jest wykonywane w kontekście "obszar".</span><span class="sxs-lookup"><span data-stu-id="e0c1b-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="e0c1b-123">Serwer zawiera nazwę obszaru nagłówka WWW-Authenticate.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="e0c1b-124">Poświadczenia użytkownika są prawidłowe w ramach tego obszaru.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="e0c1b-125">Dokładne zakresu obszaru jest zdefiniowany przez serwer.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="e0c1b-126">Na przykład można zdefiniować kilku obszarów w celu zasobów partycji.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="e0c1b-127">Ponieważ poświadczenia są wysyłane niezaszyfrowane, uwierzytelnianie podstawowe jest tylko bezpieczne za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="e0c1b-128">Zobacz [Praca z protokołem SSL w składniku Web API](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e0c1b-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="e0c1b-129">Uwierzytelnianie podstawowe jest również narażony na ataki CSRF.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="e0c1b-130">Po użytkownik wprowadza poświadczenia, przeglądarka automatycznie wysyła je na kolejnych żądań do tej samej domeny, czas trwania sesji.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="e0c1b-131">W tym żądania AJAX.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-131">This includes AJAX requests.</span></span> <span data-ttu-id="e0c1b-132">Zobacz [zapobieganie Fałszerstwie żądania Międzywitrynowego (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="e0c1b-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="e0c1b-133">Uwierzytelnianie podstawowe w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="e0c1b-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="e0c1b-134">Usługi IIS obsługują uwierzytelnianie podstawowe, ale istnieje Ostrzeżenie: użytkownik jest uwierzytelniany na podstawie swoich poświadczeń systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="e0c1b-135">Oznacza to, że użytkownik musi mieć konto w domenie serwera.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="e0c1b-136">Witryny sieci web publicznych zwykle można uwierzytelniać na dostawcy członkostwa ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="e0c1b-137">Aby włączyć uwierzytelnianie podstawowe, za pomocą usług IIS, ustaw tryb uwierzytelniania "Windows" w pliku Web.config projektu programu ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="e0c1b-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="e0c1b-138">W tym trybie usługi IIS używają poświadczeń systemu Windows do uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="e0c1b-139">Ponadto należy włączyć uwierzytelnianie podstawowe w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="e0c1b-140">W Menedżerze usług IIS przejdź do widoku funkcji, wybierz uwierzytelnianie, a następnie włączyć uwierzytelnianie podstawowe.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="e0c1b-141">W projekcie interfejsu API sieci Web, Dodaj `[Authorize]` atrybutu dla każdej akcji kontrolera, które wymagają uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="e0c1b-142">Klient samodzielnie przeprowadza uwierzytelnianie przez ustawienie nagłówek autoryzacji w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="e0c1b-143">Klientów w przeglądarkach automatycznego wykonania tego kroku.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="e0c1b-144">Klienci nonbrowser należy ustawić nagłówka.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="e0c1b-145">Uwierzytelnianie podstawowe z członkostwem niestandardowych</span><span class="sxs-lookup"><span data-stu-id="e0c1b-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="e0c1b-146">Jak wspomniano, uwierzytelnianie podstawowe zapewnia wbudowaną usług IIS korzysta z poświadczeń systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="e0c1b-147">Oznacza to, że należy utworzyć konta dla użytkowników na serwerze hostingu.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="e0c1b-148">Ale aplikację internetową kont użytkowników zwykle są przechowywane w zewnętrznej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="e0c1b-149">Poniższy kod, w jaki sposób moduł protokołu HTTP, który przeprowadza uwierzytelnianie podstawowe.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="e0c1b-150">Dostawcy członkostwa ASP.NET mogą łatwo dodatku, zastępując `CheckPassword` metodę, która jest atrapa metoda w tym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="e0c1b-151">W sieci Web API 2, należy rozważyć zapisu [filtr uwierzytelniania](authentication-filters.md) lub [oprogramowanie pośredniczące OWIN](../../../aspnet/overview/owin-and-katana/index.md), zamiast modułu HTTP.</span><span class="sxs-lookup"><span data-stu-id="e0c1b-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="e0c1b-152">Aby włączyć moduł protokołu HTTP, Dodaj następujący plik web.config w **system.webServer** sekcji:</span><span class="sxs-lookup"><span data-stu-id="e0c1b-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="e0c1b-153">Zamień na nazwę zestawu (nie z rozszerzeniem "dll") "YourAssemblyName".</span><span class="sxs-lookup"><span data-stu-id="e0c1b-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="e0c1b-154">Należy wyłączyć inne schematy uwierzytelniania, takich jak uwierzytelniania formularzy lub systemu Windows</span><span class="sxs-lookup"><span data-stu-id="e0c1b-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
