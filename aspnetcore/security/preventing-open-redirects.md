---
title: Zapobieganie atakom na otwarte przekierowywanie w programie ASP.NET Core
author: ardalis
description: Pokazuje, jak zapobiegać atakom na otwarte przekierowywanie względem aplikacji ASP.NET Core
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 9d8cac8708fe9aeadba5af1287362a20df7f6bfe
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815499"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a><span data-ttu-id="edb95-103">Zapobieganie atakom na otwarte przekierowywanie w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="edb95-103">Prevent open redirect attacks in ASP.NET Core</span></span>

<span data-ttu-id="edb95-104">Aplikację internetową, która przekierowuje do adresu URL, który jest określony za pomocą żądania, takie jak dane formularza lub querystring może potencjalnie naruszone przekierowywać użytkowników zewnętrznych, złośliwy adres URL.</span><span class="sxs-lookup"><span data-stu-id="edb95-104">A web app that redirects to a URL that's specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="edb95-105">Tym naruszeniu nosi nazwę na otwarte przekierowywanie ataki.</span><span class="sxs-lookup"><span data-stu-id="edb95-105">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="edb95-106">Zawsze, gdy logika aplikacji przekierowuje pod określony adres URL, należy sprawdzić, czy adres URL przekierowania nie została zmieniona.</span><span class="sxs-lookup"><span data-stu-id="edb95-106">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="edb95-107">Platforma ASP.NET Core ma wbudowaną funkcję, aby chronić aplikacje przed atakom na otwarte przekierowywanie (znany także jako otwarte przekierowywanie).</span><span class="sxs-lookup"><span data-stu-id="edb95-107">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="edb95-108">Co to jest atak na otwarte przekierowywanie?</span><span class="sxs-lookup"><span data-stu-id="edb95-108">What is an open redirect attack?</span></span>

<span data-ttu-id="edb95-109">Aplikacje sieci Web często przekierowywać użytkowników do strony logowania w przypadku uzyskiwania dostępu do zasobów, które wymagają uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="edb95-109">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="edb95-110">Zwykle obejmują przekierowanie `returnUrl` parametr querystring tak, aby użytkownika mogą być zwrócone do pierwotnie żądanego adresu URL, po ich pomyślnym zalogowaniu.</span><span class="sxs-lookup"><span data-stu-id="edb95-110">The redirection typically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="edb95-111">Po użytkownik jest uwierzytelniany, są one przekierowanie do adresu URL, pierwszy poprosił.</span><span class="sxs-lookup"><span data-stu-id="edb95-111">After the user authenticates, they're redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="edb95-112">Ponieważ docelowy adres URL jest określona w zmiennej querystring żądania, złośliwy użytkownik może manipulować ciąg zapytania.</span><span class="sxs-lookup"><span data-stu-id="edb95-112">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="edb95-113">Zmodyfikowany querystring umożliwiającej witryny przekierować użytkownika do witryny zewnętrznej, złośliwy.</span><span class="sxs-lookup"><span data-stu-id="edb95-113">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="edb95-114">Technika ta jest zwana Otwórz atak przekierowania (lub przekierowania).</span><span class="sxs-lookup"><span data-stu-id="edb95-114">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="edb95-115">Na przykład ataki</span><span class="sxs-lookup"><span data-stu-id="edb95-115">An example attack</span></span>

<span data-ttu-id="edb95-116">Złośliwy użytkownik może tworzyć ataku, aby umożliwić złośliwemu użytkownikowi dostęp do poświadczeń użytkownika lub informacji poufnych.</span><span class="sxs-lookup"><span data-stu-id="edb95-116">A malicious user can develop an attack intended to allow the malicious user access to a user's credentials or sensitive information.</span></span> <span data-ttu-id="edb95-117">Aby rozpocząć atak, złośliwy użytkownik convinces użytkownikowi kliknij link do strony logowania w witrynie za pomocą `returnUrl` wartości querystring dodany do adresu URL.</span><span class="sxs-lookup"><span data-stu-id="edb95-117">To begin the attack, the malicious user convinces the user to click a link to your site's login page with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="edb95-118">Rozważmy na przykład aplikacji `contoso.com` zawierającą stronę logowania w `http://contoso.com/Account/LogOn?returnUrl=/Home/About`.</span><span class="sxs-lookup"><span data-stu-id="edb95-118">For example, consider an app at `contoso.com` that includes a login page at `http://contoso.com/Account/LogOn?returnUrl=/Home/About`.</span></span> <span data-ttu-id="edb95-119">Atak obejmuje następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="edb95-119">The attack follows these steps:</span></span>

1. <span data-ttu-id="edb95-120">Użytkownik kliknie to łącze do `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (drugi adres URL to "contoso**1**.com", nie "contoso.com").</span><span class="sxs-lookup"><span data-stu-id="edb95-120">The user clicks a malicious link to `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` (the second URL is "contoso**1**.com", not "contoso.com").</span></span>
2. <span data-ttu-id="edb95-121">Użytkownik, który loguje się pomyślnie.</span><span class="sxs-lookup"><span data-stu-id="edb95-121">The user logs in successfully.</span></span>
3. <span data-ttu-id="edb95-122">Użytkownik jest przekierowywany (przez witrynę) do `http://contoso1.com/Account/LogOn` (złośliwych witryn, który wygląda tak samo jak real lokacji).</span><span class="sxs-lookup"><span data-stu-id="edb95-122">The user is redirected (by the site) to `http://contoso1.com/Account/LogOn` (a malicious site that looks exactly like real site).</span></span>
4. <span data-ttu-id="edb95-123">Użytkownik loguje się ponownie (dzięki czemu złośliwego lokacji poświadczeń) i jest przekierowywany do rzeczywistego witryny.</span><span class="sxs-lookup"><span data-stu-id="edb95-123">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="edb95-124">Prawdopodobnie użytkownik uważa, że pierwsza próba logowania nie powiodło się i że ich druga próba zakończyła się powodzeniem.</span><span class="sxs-lookup"><span data-stu-id="edb95-124">The user likely believes that their first attempt to log in failed and that their second attempt is successful.</span></span> <span data-ttu-id="edb95-125">Prawdopodobnie użytkownik pozostaje świadomości, że ich poświadczenia są naruszone.</span><span class="sxs-lookup"><span data-stu-id="edb95-125">The user most likely remains unaware that their credentials are compromised.</span></span>

![Proces ataku otwarte przekierowywanie](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="edb95-127">Oprócz strony logowania niektóre witryny zapewniają przekierowania strony lub punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="edb95-127">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="edb95-128">Wyobraź sobie, Twoja aplikacja ma stronę z otwarte przekierowywanie `/Home/Redirect`.</span><span class="sxs-lookup"><span data-stu-id="edb95-128">Imagine your app has a page with an open redirect, `/Home/Redirect`.</span></span> <span data-ttu-id="edb95-129">Osoba atakująca może utworzyć na przykład łącze w wiadomości e-mail, który prowadzi do `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`.</span><span class="sxs-lookup"><span data-stu-id="edb95-129">An attacker could create, for example, a link in an email that goes to `[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`.</span></span> <span data-ttu-id="edb95-130">Typowy użytkownik odczyta adres URL i zobacz zaczyna się od nazwy lokacji.</span><span class="sxs-lookup"><span data-stu-id="edb95-130">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="edb95-131">Zaufanie, który, będzie kliknij łącze.</span><span class="sxs-lookup"><span data-stu-id="edb95-131">Trusting that, they will click the link.</span></span> <span data-ttu-id="edb95-132">Otwarte przekierowywanie będzie wysłać użytkownika do witryny wyłudzania informacji, który będzie taka sama jak należy do Ciebie, a użytkownik będzie prawdopodobnie Zaloguj się do będą uważali jest witryny.</span><span class="sxs-lookup"><span data-stu-id="edb95-132">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="edb95-133">Ochrona przed atakom na otwarte przekierowywanie</span><span class="sxs-lookup"><span data-stu-id="edb95-133">Protecting against open redirect attacks</span></span>

<span data-ttu-id="edb95-134">Podczas tworzenia aplikacji sieci web, wszystkie dane dostarczane przez użytkownika należy traktować jako niezaufana.</span><span class="sxs-lookup"><span data-stu-id="edb95-134">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="edb95-135">Jeśli aplikacja ma funkcji, która przekierowuje użytkownika, w oparciu o zawartość adresu URL, upewnij się, że takie przekierowuje tylko są wykonywane lokalnie w aplikacji (lub znany adres URL nie dowolnego adresu URL, które mogą być dostarczane w zmiennej querystring).</span><span class="sxs-lookup"><span data-stu-id="edb95-135">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="edb95-136">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="edb95-136">LocalRedirect</span></span>

<span data-ttu-id="edb95-137">Użyj `LocalRedirect` metody pomocnika od podstawy `Controller` klasy:</span><span class="sxs-lookup"><span data-stu-id="edb95-137">Use the `LocalRedirect` helper method from the base `Controller` class:</span></span>

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="edb95-138">`LocalRedirect` spowoduje zgłoszenie wyjątku, jeśli określono adres URL inną niż lokalna.</span><span class="sxs-lookup"><span data-stu-id="edb95-138">`LocalRedirect` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="edb95-139">W przeciwnym razie działa podobnie jak `Redirect` metody.</span><span class="sxs-lookup"><span data-stu-id="edb95-139">Otherwise, it behaves just like the `Redirect` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="edb95-140">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="edb95-140">IsLocalUrl</span></span>

<span data-ttu-id="edb95-141">Użyj [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper.islocalurl#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) metodę, aby przetestować, zanim zostaną przekierowywanie adresów URL:</span><span class="sxs-lookup"><span data-stu-id="edb95-141">Use the [IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper.islocalurl#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="edb95-142">Poniższy przykład pokazuje, jak sprawdzić, czy adres URL jest lokalny, przed przekierowaniem.</span><span class="sxs-lookup"><span data-stu-id="edb95-142">The following example shows how to check whether a URL is local before redirecting.</span></span>

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

<span data-ttu-id="edb95-143">`IsLocalUrl` Metoda ochronę przypadkowo nastąpi przekierowanie do złośliwych witryn przez użytkowników.</span><span class="sxs-lookup"><span data-stu-id="edb95-143">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="edb95-144">Można rejestrować szczegóły adresu URL, który został podany, gdy adres URL inną niż lokalna jest podany w sytuacji, gdy oczekiwany lokalny adres URL.</span><span class="sxs-lookup"><span data-stu-id="edb95-144">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="edb95-145">Rejestrowanie przekierowania adresów URL może pomóc w diagnozowaniu ataków przekierowania.</span><span class="sxs-lookup"><span data-stu-id="edb95-145">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>
