---
title: Zapobieganie atakom przekierowania otwartych w aplikacji platformy ASP.NET Core | Dokumentacja firmy Microsoft
author: ardalis
description: "Pokazuje, jak zapobiegać atakom Otwórz przekierowania dla aplikacji platformy ASP.NET Core"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/preventing-open-redirects
ms.openlocfilehash: e57ae429e9af54ade74485361ba591cb75c16752
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="preventing-open-redirect-attacks-in-an-aspnet-core-app"></a><span data-ttu-id="52591-103">Zapobieganie atakom przekierowania otwartych w aplikacji platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="52591-103">Preventing Open Redirect Attacks in an ASP.NET Core app</span></span>

<span data-ttu-id="52591-104">Aplikacja sieci web, który przekierowuje do adresu URL, który został określony przy użyciu żądania, takich jak dane formularza lub ciągu kwerendy może potencjalnie niepowołane przekierowuje użytkowników zewnętrznych, złośliwy adresu URL.</span><span class="sxs-lookup"><span data-stu-id="52591-104">A web app that redirects to a URL that is specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="52591-105">Naruszeniu ten nosi nazwę atak przekierowania otwarte.</span><span class="sxs-lookup"><span data-stu-id="52591-105">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="52591-106">Zawsze, gdy logiki aplikacji przekierowuje pod określony adres URL, należy sprawdzić, czy adres URL przekierowania nie została zmieniona.</span><span class="sxs-lookup"><span data-stu-id="52591-106">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="52591-107">Platformy ASP.NET Core ma wbudowaną funkcję do ochrony aplikacji przed atakami Otwórz przekierowania (znanej także jako Otwórz przekierowania).</span><span class="sxs-lookup"><span data-stu-id="52591-107">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="52591-108">Co to jest atak przekierowania Otwórz?</span><span class="sxs-lookup"><span data-stu-id="52591-108">What is an open redirect attack?</span></span>

<span data-ttu-id="52591-109">Podczas uzyskiwania dostępu do zasobów, które wymagają uwierzytelniania aplikacji sieci Web często przekierować użytkowników do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="52591-109">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="52591-110">Obejmuje typlically przekierowania `returnUrl` parametr querystring, dzięki czemu użytkownik może być zwracany do pierwotnie żądanego adresu URL po ich pomyślnym zalogowaniu.</span><span class="sxs-lookup"><span data-stu-id="52591-110">The redirection typlically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="52591-111">Po uwierzytelnia użytkownika, zostanie przekierowany do były one pierwotnie żądanego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="52591-111">After the user authenticates, they are redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="52591-112">Docelowy adres URL, ponieważ określono w ciąg zapytania żądania złośliwy użytkownik może manipulować ciąg zapytania.</span><span class="sxs-lookup"><span data-stu-id="52591-112">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="52591-113">Zmodyfikowany querystring umożliwia witrynie przekierowanie użytkownika do witryny zewnętrznej, złośliwe.</span><span class="sxs-lookup"><span data-stu-id="52591-113">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="52591-114">Ta metoda jest wywoływana Otwórz atak przekierowania (lub przekierowania).</span><span class="sxs-lookup"><span data-stu-id="52591-114">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="52591-115">Na przykład ataki</span><span class="sxs-lookup"><span data-stu-id="52591-115">An example attack</span></span>

<span data-ttu-id="52591-116">Złośliwy użytkownik może utworzyć atak ma umożliwić złośliwemu użytkownikowi dostęp do poświadczeń użytkownika lub poufnych informacji w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="52591-116">A malicious user could develop an attack intended to allow the malicious user access to a user's credentials or sensitive information on your app.</span></span> <span data-ttu-id="52591-117">Aby rozpocząć atak, ich przekonać użytkownika do kliknij łącze do strony logowania w witrynie, z `returnUrl` wartości querystring dodany do adresu URL.</span><span class="sxs-lookup"><span data-stu-id="52591-117">To begin the attack, they convince the user to click a link to your site's login page, with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="52591-118">Na przykład [NerdDinner.com](http://nerddinner.com) przykładowej aplikacji (przeznaczone dla platformy ASP.NET MVC) zawiera takie strony logowania tutaj: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span><span class="sxs-lookup"><span data-stu-id="52591-118">For example, the [NerdDinner.com](http://nerddinner.com) sample application (written for ASP.NET MVC) includes such a login page here: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span></span> <span data-ttu-id="52591-119">Atak następnie obejmuje następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="52591-119">The attack then follows these steps:</span></span>

1. <span data-ttu-id="52591-120">Użytkownik kliknie łącze do ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (należy pamiętać, drugi adres URL jest nerddi**n**era, nie nerddi**nn**ERA).</span><span class="sxs-lookup"><span data-stu-id="52591-120">User clicks a link to ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (note, second URL is nerddi**n**er, not nerddi**nn**er).</span></span>
2. <span data-ttu-id="52591-121">Użytkownik, który loguje się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="52591-121">The user logs in successfully.</span></span>
3. <span data-ttu-id="52591-122">Użytkownik zostaje przekierowany (przez witrynę) do ``http://nerddiner.com/Account/LogOn`` (złośliwa witryna przypominającą rzeczywistej lokacji).</span><span class="sxs-lookup"><span data-stu-id="52591-122">The user is redirected (by the site) to ``http://nerddiner.com/Account/LogOn`` (malicious site that looks like real site).</span></span>
4. <span data-ttu-id="52591-123">Użytkownik loguje się ponownie (podając złośliwego lokacji poświadczeń) i jest przekierowywany do rzeczywistego witryny.</span><span class="sxs-lookup"><span data-stu-id="52591-123">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="52591-124">Użytkownik prawdopodobnie będą sądziły pierwsza próba logowania nie powiodła się, a ich drugi zakończyło się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="52591-124">The user will likely believe their first attempt to log in failed, and their second one was successful.</span></span> <span data-ttu-id="52591-125">Prawdopodobnie będziesz pozostają bez "świadomości" swoje poświadczenia zostały naruszone.</span><span class="sxs-lookup"><span data-stu-id="52591-125">They'll most likely remain unaware their credentials have been compromised.</span></span>

![Otwórz przekierowania ataku procesu](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="52591-127">Oprócz strony logowania niektóre Lokacje przekierowania strony lub punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="52591-127">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="52591-128">Wyobraź sobie aplikacja ma stronę Otwórz przekierowania ``/Home/Redirect``.</span><span class="sxs-lookup"><span data-stu-id="52591-128">Imagine your app has a page with an open redirect, ``/Home/Redirect``.</span></span> <span data-ttu-id="52591-129">Osoba atakująca może utworzyć na przykład łącze w wiadomości e-mail, który jest przesyłany do ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span><span class="sxs-lookup"><span data-stu-id="52591-129">An attacker could create, for example, a link in an email that goes to ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span></span> <span data-ttu-id="52591-130">Typowy użytkownik będzie wyglądać pod adresem URL i znaleźć zaczyna się od nazwy lokacji.</span><span class="sxs-lookup"><span data-stu-id="52591-130">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="52591-131">Który ufające, będzie kliknij łącze.</span><span class="sxs-lookup"><span data-stu-id="52591-131">Trusting that, they will click the link.</span></span> <span data-ttu-id="52591-132">Otwórz przekierowania następnie może wysłać do użytkownika do witryny wyłudzaniem informacji, która jest taka sama jak należy do Ciebie, a użytkownik będzie prawdopodobnie będą uważali logowanie jest witryną.</span><span class="sxs-lookup"><span data-stu-id="52591-132">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="52591-133">Ochrona przed atakami Otwórz przekierowania</span><span class="sxs-lookup"><span data-stu-id="52591-133">Protecting against open redirect attacks</span></span>

<span data-ttu-id="52591-134">Podczas tworzenia aplikacji sieci web, należy traktować wszystkie dane dostarczone przez użytkownika jako niezaufane.</span><span class="sxs-lookup"><span data-stu-id="52591-134">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="52591-135">Jeśli aplikacja ma funkcji, która przekierowuje użytkownika na podstawie zawartości adresu URL, upewnij się, że takie przekierowania tylko są wykonywane lokalnie w Twojej aplikacji (lub znany adres URL nie każdy adres URL, które mogą być dostarczane w zmiennej querystring).</span><span class="sxs-lookup"><span data-stu-id="52591-135">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="52591-136">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="52591-136">LocalRedirect</span></span>

<span data-ttu-id="52591-137">Użyj ``LocalRedirect`` metody pomocnika od podstawy `Controller` klasy:</span><span class="sxs-lookup"><span data-stu-id="52591-137">Use the ``LocalRedirect`` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="52591-138">``LocalRedirect``spowoduje zgłoszenie wyjątku, jeśli określono adres URL nie lokalnego.</span><span class="sxs-lookup"><span data-stu-id="52591-138">``LocalRedirect`` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="52591-139">W przeciwnym razie wartość działa tak samo jak ``Redirect`` metody.</span><span class="sxs-lookup"><span data-stu-id="52591-139">Otherwise, it behaves just like the ``Redirect`` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="52591-140">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="52591-140">IsLocalUrl</span></span>

<span data-ttu-id="52591-141">Użyj [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) metody do testowania przekierowywał adresów URL:</span><span class="sxs-lookup"><span data-stu-id="52591-141">Use the [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="52591-142">Poniższy przykład przedstawia sposób sprawdzania, czy adres URL jest lokalny, przed przekierowaniem.</span><span class="sxs-lookup"><span data-stu-id="52591-142">The following example shows how to check whether a URL is local before redirecting.</span></span>

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

<span data-ttu-id="52591-143">`IsLocalUrl` — Metoda uniemożliwia przypadkowo nastąpi przekierowanie do witryny złośliwych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="52591-143">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="52591-144">Możesz zalogować się szczegóły adres URL, który został podany, gdy adres URL-local znajduje się w sytuacji, gdy oczekiwano lokalny adres URL.</span><span class="sxs-lookup"><span data-stu-id="52591-144">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="52591-145">Rejestrowanie przekierowania adresów URL może pomóc w zdiagnozowaniu ataków przekierowania.</span><span class="sxs-lookup"><span data-stu-id="52591-145">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
