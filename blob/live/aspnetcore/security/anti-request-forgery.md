---
title: "Zapobieganie atakom sfałszowaniem (XSRF/CSRF) żądania Międzywitrynowego na platformie ASP.NET Core"
author: steve-smith
ms.author: riande
description: "Zapobieganie atakom sfałszowaniem (XSRF/CSRF) żądania Międzywitrynowego na platformie ASP.NET Core"
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/anti-request-forgery
ms.openlocfilehash: d7df8f91e88290509c8751a4b69804b60138846e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
# <a name="preventing-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="34893-103">Zapobieganie atakom sfałszowaniem (XSRF/CSRF) żądania Międzywitrynowego na platformie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="34893-103">Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core</span></span>

<span data-ttu-id="34893-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="34893-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-attack-does-anti-forgery-prevent"></a><span data-ttu-id="34893-105">Jakie ataku uniemożliwia zabezpieczający przed sfałszowaniem?</span><span class="sxs-lookup"><span data-stu-id="34893-105">What attack does anti-forgery prevent?</span></span>

<span data-ttu-id="34893-106">Fałszowanie żądań między witrynami (znanej także jako XSRF lub CSRF, Wymowa *surf zobacz*) jest atak wykorzystujący aplikacje obsługiwane w sieci web, zgodnie z którymi złośliwa witryna sieci web może mieć wpływ interakcji między przeglądarką klienta i witryny sieci web, które ufają Przeglądarka.</span><span class="sxs-lookup"><span data-stu-id="34893-106">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted applications whereby a malicious web site can influence the interaction between a client browser and a web site that trusts that browser.</span></span> <span data-ttu-id="34893-107">Tego rodzaju ataki są możliwe, ponieważ przeglądarki sieci web wysyłać niektóre typy tokenów uwierzytelniania automatycznie z każdego żądania do witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="34893-107">These attacks are made possible because web browsers send some types of authentication tokens automatically with every request to a web site.</span></span> <span data-ttu-id="34893-108">Ten formularz wykorzystać jest także znana jako *ataku jednym kliknięciem* lub jako *sesji jazda*, ponieważ wykorzystuje ataku użytkownika wcześniej uwierzytelniona sesji.</span><span class="sxs-lookup"><span data-stu-id="34893-108">This form of exploit is also known as a *one-click attack* or as *session riding*, because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="34893-109">Przykład atak CSRF:</span><span class="sxs-lookup"><span data-stu-id="34893-109">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="34893-110">Użytkownicy logujący się do `www.example.com`, uwierzytelnianie formularzy przy użyciu.</span><span class="sxs-lookup"><span data-stu-id="34893-110">A user logs into `www.example.com`, using forms authentication.</span></span>
2. <span data-ttu-id="34893-111">Serwer uwierzytelnia użytkownika i generuje odpowiedzi, który zawiera plik cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="34893-111">The server authenticates the user and issues a response that includes an authentication cookie.</span></span>
3. <span data-ttu-id="34893-112">Użytkownik odwiedzi złośliwą witrynę.</span><span class="sxs-lookup"><span data-stu-id="34893-112">The user visits a malicious site.</span></span>

   <span data-ttu-id="34893-113">Złośliwa witryna zawiera formularza HTML podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="34893-113">The malicious site contains an HTML form similar to the following:</span></span>

```html
   <h1>You Are a Winner!</h1>
     <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw" />
       <input type="hidden" name="Amount" value="1000000" />
     <input type="submit" value="Click Me"/>
   </form>
```

<span data-ttu-id="34893-114">Należy zauważyć, że akcja formularza zapisuje do lokacji narażony, nie niebezpiecznej witryny.</span><span class="sxs-lookup"><span data-stu-id="34893-114">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="34893-115">Jest to część CSRF "cross-site".</span><span class="sxs-lookup"><span data-stu-id="34893-115">This is the “cross-site” part of CSRF.</span></span>

4. <span data-ttu-id="34893-116">Użytkownik klika przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="34893-116">The user clicks the submit button.</span></span> <span data-ttu-id="34893-117">Przeglądarka automatycznie uwzględnia pliku cookie uwierzytelniania dla żądanej domeny (narażone lokacji w tym przypadku) z żądaniem.</span><span class="sxs-lookup"><span data-stu-id="34893-117">The browser automatically includes the authentication cookie for the requested domain (the vulnerable site in this case) with the request.</span></span>
5. <span data-ttu-id="34893-118">Żądanie jest uruchomiony na serwerze z kontekstem uwierzytelniania użytkownika i mogą wykonywać wszystkie uwierzytelniony użytkownik może wykonywać.</span><span class="sxs-lookup"><span data-stu-id="34893-118">The request runs on the server with the user’s authentication context and can do anything that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="34893-119">W tym przykładzie wymaga od użytkownika kliknij przycisk formularza.</span><span class="sxs-lookup"><span data-stu-id="34893-119">This example requires the user to click the form button.</span></span> <span data-ttu-id="34893-120">Strony złośliwych można:</span><span class="sxs-lookup"><span data-stu-id="34893-120">The malicious page could:</span></span>

* <span data-ttu-id="34893-121">Uruchom skrypt, który automatycznie wyśle formularz.</span><span class="sxs-lookup"><span data-stu-id="34893-121">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="34893-122">Wysyła żądanie AJAX przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="34893-122">Sends a form submission as an AJAX request.</span></span> 
* <span data-ttu-id="34893-123">Formularz ukryty z CSS.</span><span class="sxs-lookup"><span data-stu-id="34893-123">Use a hidden form with CSS.</span></span> 

<span data-ttu-id="34893-124">Przy użyciu protokołu SSL nie zapobiega atak CSRF, złośliwa witryna można wysyłać `https://` żądania.</span><span class="sxs-lookup"><span data-stu-id="34893-124">Using SSL does not prevent a CSRF attack, the malicious site can send an `https://` request.</span></span> 

<span data-ttu-id="34893-125">Niektóre ataki docelowych punktów końcowych witryny, które odpowiadają na `GET` żądania, w których przypadku tag obrazu może służyć do wykonywania akcji (Ta forma ataku jest typowe na forum witryn, które zezwala na obrazy, ale zablokowanie JavaScript).</span><span class="sxs-lookup"><span data-stu-id="34893-125">Some attacks  target site endpoints that respond to `GET` requests, in which case an image tag can be used to perform the action (this form of attack is common on forum sites that permit images but block JavaScript).</span></span> <span data-ttu-id="34893-126">Aplikacje, które spowodują zmianę stanu z `GET` żądania są narażone przed złośliwymi atakami.</span><span class="sxs-lookup"><span data-stu-id="34893-126">Applications that change state with `GET` requests are  vulnerable from malicious attacks.</span></span>

<span data-ttu-id="34893-127">Ataki CSRF są możliwe do przed witryn sieci web, które używają plików cookie do uwierzytelniania, ponieważ przeglądarki wysyłać wszystkich odpowiednich plików cookie do docelowej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="34893-127">CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="34893-128">Jednak ataków CSRF nie są ograniczone do wykorzystania plików cookie.</span><span class="sxs-lookup"><span data-stu-id="34893-128">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="34893-129">Na przykład uwierzytelnianie podstawowe i szyfrowane również są zagrożone.</span><span class="sxs-lookup"><span data-stu-id="34893-129">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="34893-130">Po zalogowaniu się użytkownika za pomocą uwierzytelnianie podstawowe lub szyfrowane, przeglądarka automatycznie wysyła poświadczenia zakończenia sesji.</span><span class="sxs-lookup"><span data-stu-id="34893-130">After a user logs in with Basic or Digest authentication, the browser automatically sends the credentials until the session ends.</span></span>

<span data-ttu-id="34893-131">Uwaga: W tym kontekście *sesji* odwołuje się do sesji po stronie klienta, w którym użytkownik jest uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="34893-131">Note: In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="34893-132">Jest związana z sesji po stronie serwera lub [oprogramowanie pośredniczące sesji](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="34893-132">It is unrelated to server-side sessions or [session middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="34893-133">Użytkownicy mogą ochronić CSRF luki w zabezpieczeniach poprzez:</span><span class="sxs-lookup"><span data-stu-id="34893-133">Users can guard against CSRF vulnerabilities by:</span></span>
* <span data-ttu-id="34893-134">Wylogowując się witryn sieci web, gdy zostało ukończone z nich korzystać.</span><span class="sxs-lookup"><span data-stu-id="34893-134">Logging off of web sites when they have finished using them.</span></span>
* <span data-ttu-id="34893-135">Okresowo wyczyszczenie plików cookie w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="34893-135">Clearing their browser's cookies periodically.</span></span>

<span data-ttu-id="34893-136">Jednak luk w zabezpieczeniach CSRF są zasadniczo problem z aplikacji sieci web, a nie użytkownika końcowego.</span><span class="sxs-lookup"><span data-stu-id="34893-136">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="how-does-aspnet-core-mvc-address-csrf"></a><span data-ttu-id="34893-137">W jaki sposób program ASP.NET Core MVC uwzględnia CSRF</span><span class="sxs-lookup"><span data-stu-id="34893-137">How does ASP.NET Core MVC address CSRF?</span></span>

> [!WARNING]
> <span data-ttu-id="34893-138">Implementuje platformy ASP.NET Core za pomocą przeciwko request forgery [stosu ochrony danych platformy ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="34893-138">ASP.NET Core implements anti-request-forgery using the [ASP.NET Core data protection stack](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="34893-139">Ochrona danych platformy ASP.NET Core musi być skonfigurowana do pracy w farmie serwerów.</span><span class="sxs-lookup"><span data-stu-id="34893-139">ASP.NET Core data protection must be configured to work in a server farm.</span></span> <span data-ttu-id="34893-140">Zobacz [konfigurowania ochrony danych](xref:security/data-protection/configuration/overview) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="34893-140">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="34893-141">Konfiguracja ochrony danych przeciwko request forgery domyślnego platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="34893-141">ASP.NET Core anti-request-forgery  default data protection configuration</span></span> 

<span data-ttu-id="34893-142">W programie ASP.NET 2.0 MVC Core [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects tokenów zabezpieczających przed sfałszowaniem elementów formularza HTML.</span><span class="sxs-lookup"><span data-stu-id="34893-142">In ASP.NET Core MVC 2.0 the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects anti-forgery tokens for HTML form elements.</span></span> <span data-ttu-id="34893-143">Na przykład następujący kod w pliku Razor automatycznie wygeneruje tokenów zabezpieczających przed sfałszowaniem:</span><span class="sxs-lookup"><span data-stu-id="34893-143">For example, the following markup in a Razor file will automatically generate anti-forgery tokens:</span></span>

```html
<form method="post">
  <!-- form markup -->
</form>
```

<span data-ttu-id="34893-144">Automatyczne generowanie tokenów zabezpieczających przed sfałszowaniem elementów formularza HTML się stanie, gdy:</span><span class="sxs-lookup"><span data-stu-id="34893-144">The automatic generation of anti-forgery tokens for HTML form elements happens when:</span></span>

* <span data-ttu-id="34893-145">`form` Tag zawiera `method="post"` atrybutu i</span><span class="sxs-lookup"><span data-stu-id="34893-145">The `form` tag contains the `method="post"` attribute AND</span></span>

  * <span data-ttu-id="34893-146">Atrybut akcji jest pusty.</span><span class="sxs-lookup"><span data-stu-id="34893-146">The action attribute is empty.</span></span> <span data-ttu-id="34893-147">( `action=""`) LUB</span><span class="sxs-lookup"><span data-stu-id="34893-147">( `action=""`) OR</span></span>
  * <span data-ttu-id="34893-148">Nie podano atrybutu akcji.</span><span class="sxs-lookup"><span data-stu-id="34893-148">The action attribute is not supplied.</span></span> <span data-ttu-id="34893-149">(`<form method="post">`)</span><span class="sxs-lookup"><span data-stu-id="34893-149">(`<form method="post">`)</span></span>

<span data-ttu-id="34893-150">Możesz wyłączyć automatyczne generowanie tokenów zabezpieczających przed sfałszowaniem elementów formularza HTML przez:</span><span class="sxs-lookup"><span data-stu-id="34893-150">You can disable automatic generation of anti-forgery tokens for HTML form elements by:</span></span>

* <span data-ttu-id="34893-151">Jawnie wyłącza `asp-antiforgery`.</span><span class="sxs-lookup"><span data-stu-id="34893-151">Explicitly disabling `asp-antiforgery`.</span></span> <span data-ttu-id="34893-152">Na przykład</span><span class="sxs-lookup"><span data-stu-id="34893-152">For example</span></span>

 ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* <span data-ttu-id="34893-153">OPT element form poza pomocników tagów przy użyciu Pomocnika tagów [! symbol Wypisz](xref:mvc/views/tag-helpers/intro#opt-out).</span><span class="sxs-lookup"><span data-stu-id="34893-153">Opt the form element out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out).</span></span>

 ```html
  <!form method="post">
  </!form>
  ```

* <span data-ttu-id="34893-154">Usuń `FormTagHelper` z widoku.</span><span class="sxs-lookup"><span data-stu-id="34893-154">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="34893-155">Możesz usunąć `FormTagHelper` z widoku przez dodanie do widoku Razor następujące dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="34893-155">You can remove the `FormTagHelper` from a view by adding the following directive to the Razor view:</span></span>

 ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="34893-156">[Stron razor](xref:mvc/razor-pages/index) są automatycznie chronione przed XSRF/CSRF.</span><span class="sxs-lookup"><span data-stu-id="34893-156">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="34893-157">Nie trzeba zapisać jakiegokolwiek dodatkowego kodu.</span><span class="sxs-lookup"><span data-stu-id="34893-157">You don't have to write any additional code.</span></span> <span data-ttu-id="34893-158">Zobacz [XSRF/CSRF i stron Razor](xref:mvc/razor-pages/index#xsrf) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="34893-158">See [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf) for more information.</span></span>

<span data-ttu-id="34893-159">Najbardziej typowym podejściem do obrony przed atakami CSRF jest wzorzec tokenu Synchronizatora (STP).</span><span class="sxs-lookup"><span data-stu-id="34893-159">The most common approach to defending against CSRF attacks is the synchronizer token pattern (STP).</span></span> <span data-ttu-id="34893-160">STP to technika używany, gdy użytkownik zażąda strony z danych formularza.</span><span class="sxs-lookup"><span data-stu-id="34893-160">STP is a technique used when the user requests a page with form data.</span></span> <span data-ttu-id="34893-161">Serwer wysyła token skojarzone z tożsamością bieżącego użytkownika do klienta.</span><span class="sxs-lookup"><span data-stu-id="34893-161">The server sends a token associated with the current user's identity to the client.</span></span> <span data-ttu-id="34893-162">Klient wysyła ponownie token do serwera w celu weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="34893-162">The client sends back the token to the server for verification.</span></span> <span data-ttu-id="34893-163">Jeśli serwer odbiera token, który nie odpowiada tożsamość uwierzytelnionego użytkownika, żądanie zostanie odrzucone.</span><span class="sxs-lookup"><span data-stu-id="34893-163">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span> <span data-ttu-id="34893-164">Token jest unikatowy i nieprzewidywalny.</span><span class="sxs-lookup"><span data-stu-id="34893-164">The token is unique and unpredictable.</span></span> <span data-ttu-id="34893-165">Token mogą służyć do zapewnienia prawidłowego sekwencjonowania szereg żądań (strona zapewnienie 1 poprzedza strona 2 poprzedza strony 3).</span><span class="sxs-lookup"><span data-stu-id="34893-165">The token can also be used to ensure proper sequencing of a series of requests (ensuring page 1 precedes page 2 which precedes page 3).</span></span> <span data-ttu-id="34893-166">Wszystkich formularzy w szablonach platformy ASP.NET Core MVC generowania antiforgery tokenów.</span><span class="sxs-lookup"><span data-stu-id="34893-166">All the forms in ASP.NET Core MVC templates generate antiforgery tokens.</span></span> <span data-ttu-id="34893-167">Dwa poniższe przykłady logiki widoku generowania tokenów antiforgery:</span><span class="sxs-lookup"><span data-stu-id="34893-167">The following two examples of view logic generate antiforgery tokens:</span></span>

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

<span data-ttu-id="34893-168">Należy jawnie dodać antiforgery token do ``<form>`` elementu bez użycia pomocników tagów z Pomocnika kodu HTML ``@Html.AntiForgeryToken``:</span><span class="sxs-lookup"><span data-stu-id="34893-168">You can explicitly add an antiforgery token to a ``<form>`` element without using tag helpers with the HTML helper ``@Html.AntiForgeryToken``:</span></span>


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="34893-169">W każdym z powyższych przypadków platformy ASP.NET Core doda ukryte pole formularza podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="34893-169">In each of the preceding cases, ASP.NET Core will add a hidden form field similar to the following:</span></span>
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw" />
```

<span data-ttu-id="34893-170">Platformy ASP.NET Core obejmuje trzy [filtry](xref:mvc/controllers/filters) do pracy z tokenami antiforgery: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, i ``IgnoreAntiforgeryToken``.</span><span class="sxs-lookup"><span data-stu-id="34893-170">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens: ``ValidateAntiForgeryToken``, ``AutoValidateAntiforgeryToken``, and ``IgnoreAntiforgeryToken``.</span></span>

<a name="vaft"></a>

### <a name="validateantiforgerytoken"></a><span data-ttu-id="34893-171">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="34893-171">ValidateAntiForgeryToken</span></span>

<span data-ttu-id="34893-172">``ValidateAntiForgeryToken`` Jest filtr akcji, który można zastosować do poszczególnych akcji kontrolera, lub globalnie.</span><span class="sxs-lookup"><span data-stu-id="34893-172">The ``ValidateAntiForgeryToken`` is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="34893-173">Żądania kierowane do akcji, które mają ten filtr zostanie zablokowane, jeśli żądanie zawiera prawidłowy token antiforgery.</span><span class="sxs-lookup"><span data-stu-id="34893-173">Requests made to actions that have this filter applied will be blocked unless the request includes a valid antiforgery token.</span></span>

```c#
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();
    if (user != null)
    {
        var result = await _userManager.RemoveLoginAsync(user, account.LoginProvider, account.ProviderKey);
        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }
    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="34893-174">``ValidateAntiForgeryToken`` Atrybut wymaga tokenu dla żądań kierowanych do metod akcji go decorates tym `HTTP GET` żądań.</span><span class="sxs-lookup"><span data-stu-id="34893-174">The ``ValidateAntiForgeryToken`` attribute requires a token for requests to action methods it decorates, including `HTTP GET` requests.</span></span> <span data-ttu-id="34893-175">W przypadku zastosowania go szeroko, można zastąpić go przy użyciu ``IgnoreAntiforgeryToken`` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="34893-175">If you apply it broadly, you can override it with the ``IgnoreAntiforgeryToken`` attribute.</span></span>

### <a name="autovalidateantiforgerytoken"></a><span data-ttu-id="34893-176">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="34893-176">AutoValidateAntiforgeryToken</span></span>

<span data-ttu-id="34893-177">Aplikacje platformy ASP.NET Core zazwyczaj nie generują antiforgery tokeny dla bezpieczne metody HTTP (GET, HEAD, opcje i śledzenia).</span><span class="sxs-lookup"><span data-stu-id="34893-177">ASP.NET Core apps generally do not generate antiforgery tokens for HTTP safe methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="34893-178">Zamiast stosowania szeroko ``ValidateAntiForgeryToken`` atrybutu, a następnie przesłanianie go z ``IgnoreAntiforgeryToken`` atrybutów, można użyć ``AutoValidateAntiforgeryToken`` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="34893-178">Instead of broadly applying the ``ValidateAntiForgeryToken`` attribute and then overriding it with ``IgnoreAntiforgeryToken`` attributes, you can use the ``AutoValidateAntiforgeryToken`` attribute.</span></span> <span data-ttu-id="34893-179">Ten atrybut działa identycznie do ``ValidateAntiForgeryToken`` atrybutów, z wyjątkiem tego, czy nie wymaga tokeny żądań zostało nawiązane przy użyciu następujących metod HTTP:</span><span class="sxs-lookup"><span data-stu-id="34893-179">This attribute works identically to the ``ValidateAntiForgeryToken`` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="34893-180">POBIERZ</span><span class="sxs-lookup"><span data-stu-id="34893-180">GET</span></span>
* <span data-ttu-id="34893-181">HEAD</span><span class="sxs-lookup"><span data-stu-id="34893-181">HEAD</span></span>
* <span data-ttu-id="34893-182">OPCJE</span><span class="sxs-lookup"><span data-stu-id="34893-182">OPTIONS</span></span>
* <span data-ttu-id="34893-183">TRACE</span><span class="sxs-lookup"><span data-stu-id="34893-183">TRACE</span></span>

<span data-ttu-id="34893-184">Zalecane jest użycie ``AutoValidateAntiforgeryToken`` szeroko dla scenariuszy-API.</span><span class="sxs-lookup"><span data-stu-id="34893-184">We recommend you use ``AutoValidateAntiforgeryToken`` broadly for non-API scenarios.</span></span> <span data-ttu-id="34893-185">Dzięki temu akcji POST są chronione przez domyślny.</span><span class="sxs-lookup"><span data-stu-id="34893-185">This ensures your POST actions are protected by default.</span></span> <span data-ttu-id="34893-186">Alternatywą jest Ignoruj antiforgery tokeny domyślnie, chyba że ``ValidateAntiForgeryToken`` jest stosowany do metody akcji indywidualnych.</span><span class="sxs-lookup"><span data-stu-id="34893-186">The alternative is to ignore antiforgery tokens by default, unless ``ValidateAntiForgeryToken`` is applied to the individual action method.</span></span> <span data-ttu-id="34893-187">Bardziej prawdopodobne, w tym scenariuszu dla metody akcji POST się po lewej niechronionej, pozostawiając narażony na ataki CSRF aplikacji.</span><span class="sxs-lookup"><span data-stu-id="34893-187">It's more likely in this scenario for a POST action method to be left unprotected, leaving your app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="34893-188">Nawet anonimowe WPISÓW, należy wysłać antiforgery tokenu.</span><span class="sxs-lookup"><span data-stu-id="34893-188">Even anonymous POSTS should send the antiforgery token.</span></span>

<span data-ttu-id="34893-189">Uwaga: Interfejsów API nie mają mechanizm automatycznego wysyłania-cookie część tokenu; prawdopodobnie implementacji zależy od implementacji kodu klienta.</span><span class="sxs-lookup"><span data-stu-id="34893-189">Note: APIs don't have an automatic mechanism for sending the non-cookie part of the token; your implementation will likely depend on your client code implementation.</span></span> <span data-ttu-id="34893-190">Poniżej przedstawiono kilka przykładów.</span><span class="sxs-lookup"><span data-stu-id="34893-190">Some examples are shown below.</span></span>


<span data-ttu-id="34893-191">Przykład (klasa poziom):</span><span class="sxs-lookup"><span data-stu-id="34893-191">Example (class level):</span></span>

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="34893-192">Przykład (globalne):</span><span class="sxs-lookup"><span data-stu-id="34893-192">Example (global):</span></span>

```c#
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a><span data-ttu-id="34893-193">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="34893-193">IgnoreAntiforgeryToken</span></span>

<span data-ttu-id="34893-194">``IgnoreAntiforgeryToken`` Użyć filtru, aby wyeliminować potrzebę antiforgery token obecności dla danej akcji (lub kontrolera).</span><span class="sxs-lookup"><span data-stu-id="34893-194">The ``IgnoreAntiforgeryToken`` filter is used to eliminate the need for an antiforgery token to be present for a given action (or controller).</span></span> <span data-ttu-id="34893-195">Po zastosowaniu filtru spowoduje zastąpienie ``ValidateAntiForgeryToken`` i/lub ``AutoValidateAntiforgeryToken`` filtry określone na wyższym poziomie (globalnie lub w kontrolerze).</span><span class="sxs-lookup"><span data-stu-id="34893-195">When applied, this filter will override ``ValidateAntiForgeryToken`` and/or ``AutoValidateAntiforgeryToken`` filters specified at a higher level (globally or on a controller).</span></span>

```c#
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
  [HttpPost]
  [IgnoreAntiforgeryToken]
  public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
  {
    // no antiforgery token required
  }
}
```

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="34893-196">JavaScript, AJAX i źródła</span><span class="sxs-lookup"><span data-stu-id="34893-196">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="34893-197">W aplikacjach opartych na języku HTML tradycyjnych antiforgery tokenów są przekazywane do serwera przy użyciu pól ukrytym.</span><span class="sxs-lookup"><span data-stu-id="34893-197">In traditional HTML-based applications, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="34893-198">Nowoczesne aplikacje oparte na języku JavaScript i aplikacje jednej strony (źródła) składają się programowo wiele żądań.</span><span class="sxs-lookup"><span data-stu-id="34893-198">In modern JavaScript-based apps and single page applications (SPAs), many requests are made programmatically.</span></span> <span data-ttu-id="34893-199">Te żądania AJAX mogą używać innych technik (np. nagłówki żądania lub pliki cookie) do wysyłania tokenu.</span><span class="sxs-lookup"><span data-stu-id="34893-199">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span> <span data-ttu-id="34893-200">Jeśli pliki cookie są używane do przechowywania tokeny uwierzytelniania i do uwierzytelniania żądań interfejsu API na serwerze, CSRF będzie potencjalny problem.</span><span class="sxs-lookup"><span data-stu-id="34893-200">If cookies are used to store authentication tokens and to authenticate API requests on the server, then CSRF will be a potential problem.</span></span> <span data-ttu-id="34893-201">Jednak jeśli magazyn lokalny jest używany do przechowywania token, CSRF luki w zabezpieczeniach mogą można zminimalizować, ponieważ wartości z magazynu lokalnego nie są automatycznie wysyłane do serwera z każdego nowego żądania.</span><span class="sxs-lookup"><span data-stu-id="34893-201">However, if local storage is used to store the token, CSRF vulnerability may be mitigated, since values from local storage are not sent automatically to the server with every new request.</span></span> <span data-ttu-id="34893-202">W związku z tym korzystanie z magazynu lokalnego do przechowywania antiforgery token na kliencie i wysyłania tokenu, ponieważ nagłówek żądania jest zalecane podejście.</span><span class="sxs-lookup"><span data-stu-id="34893-202">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="angularjs"></a><span data-ttu-id="34893-203">AngularJS</span><span class="sxs-lookup"><span data-stu-id="34893-203">AngularJS</span></span>

<span data-ttu-id="34893-204">AngularJS używa konwencji adres CSRF.</span><span class="sxs-lookup"><span data-stu-id="34893-204">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="34893-205">Jeśli serwer wysyła plik cookie o nazwie ``XSRF-TOKEN``, kątową ``$http`` usługi doda wartość z tego pliku cookie do nagłówka podczas wysyłania żądania do tego serwera.</span><span class="sxs-lookup"><span data-stu-id="34893-205">If the server sends a cookie with the name ``XSRF-TOKEN``, the Angular ``$http`` service will add the value from this cookie to a header when it sends a request to this server.</span></span> <span data-ttu-id="34893-206">Ten proces odbywa się automatycznie; nie musisz jawnie ustawić nagłówka.</span><span class="sxs-lookup"><span data-stu-id="34893-206">This process is automatic; you don't need to set the header explicitly.</span></span> <span data-ttu-id="34893-207">Nazwa nagłówka jest ``X-XSRF-TOKEN``.</span><span class="sxs-lookup"><span data-stu-id="34893-207">The header name is ``X-XSRF-TOKEN``.</span></span> <span data-ttu-id="34893-208">Serwer powinna wykryć ten nagłówek i zweryfikować jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="34893-208">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="34893-209">Dla interfejsu API platformy ASP.NET Core współpracują z tę Konwencję:</span><span class="sxs-lookup"><span data-stu-id="34893-209">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="34893-210">Skonfiguruj aplikację, aby dostarczyć token w pliku cookie o nazwie``XSRF-TOKEN``</span><span class="sxs-lookup"><span data-stu-id="34893-210">Configure your app to provide a token in a cookie called ``XSRF-TOKEN``</span></span>
* <span data-ttu-id="34893-211">Skonfiguruj usługę antiforgery do wyszukania nagłówek o nazwie``X-XSRF-TOKEN``</span><span class="sxs-lookup"><span data-stu-id="34893-211">Configure the antiforgery service to look for a header named ``X-XSRF-TOKEN``</span></span>

```c#
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="34893-212">[Przykładowy widok](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span><span class="sxs-lookup"><span data-stu-id="34893-212">[View sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span></span>

### <a name="javascript"></a><span data-ttu-id="34893-213">JavaScript</span><span class="sxs-lookup"><span data-stu-id="34893-213">JavaScript</span></span>

<span data-ttu-id="34893-214">Przy użyciu języka JavaScript z widokami, można utworzyć tokenu przy użyciu usługi z w danym widoku.</span><span class="sxs-lookup"><span data-stu-id="34893-214">Using JavaScript with views, you can create the token using a service from within your view.</span></span> <span data-ttu-id="34893-215">Aby to zrobić, Wstaw `Microsoft.AspNetCore.Antiforgery.IAntiforgery` usługi do widoku i wywołanie `GetAndStoreTokens`, jak pokazano:</span><span class="sxs-lookup"><span data-stu-id="34893-215">To do so, you inject the `Microsoft.AspNetCore.Antiforgery.IAntiforgery` service into the view and call `GetAndStoreTokens`, as shown:</span></span>

[!code-csharp[Main](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,24)]

<span data-ttu-id="34893-216">Takie podejście eliminuje potrzebę bezpośrednio przez ustawienia plików cookie z serwera lub odczytywania ich z klienta.</span><span class="sxs-lookup"><span data-stu-id="34893-216">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="34893-217">JavaScript można także uzyskać dostęp do tokenów podane w plikach cookie, a następnie użyj zawartość pliku cookie do tworzenia nagłówka o wartości tokenu, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="34893-217">JavaScript can also access tokens provided in cookies, and then use the cookie's contents to create a header with the token's value, as shown below.</span></span>

```c#
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="34893-218">Następnie, przy założeniu utworzenia skryptu żądania do wysyłania tokenu w nagłówku o nazwie ``X-CSRF-TOKEN``, skonfiguruj usługę antiforgery do wyszukania ``X-CSRF-TOKEN`` nagłówka:</span><span class="sxs-lookup"><span data-stu-id="34893-218">Then, assuming you construct your script requests to send the token in a header called ``X-CSRF-TOKEN``, configure the antiforgery service to look for the ``X-CSRF-TOKEN`` header:</span></span>

```c#
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="34893-219">W poniższym przykładzie użyto jQuery żądania AJAX z odpowiedni nagłówek:</span><span class="sxs-lookup"><span data-stu-id="34893-219">The following example uses jQuery to make an AJAX request with the appropriate header:</span></span>

```javascript
var csrfToken = $.cookie("CSRF-TOKEN");

$.ajax({
    url: "/api/password/changepassword",
    contentType: "application/json",
    data: JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }),
    type: "POST",
    headers: {
        "X-CSRF-TOKEN": csrfToken
    }
});
```

## <a name="configuring-antiforgery"></a><span data-ttu-id="34893-220">Konfigurowanie Antiforgery</span><span class="sxs-lookup"><span data-stu-id="34893-220">Configuring Antiforgery</span></span>

<span data-ttu-id="34893-221">`IAntiforgery`udostępnia interfejs API do skonfigurowania antiforgery systemu.</span><span class="sxs-lookup"><span data-stu-id="34893-221">`IAntiforgery` provides the API to configure the antiforgery system.</span></span> <span data-ttu-id="34893-222">Może być wymagane w `Configure` metody `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="34893-222">It can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="34893-223">W poniższym przykładzie użyto oprogramowanie pośredniczące ze strony głównej aplikacji do wygenerowania tokenu antiforgery i wysłać go w odpowiedzi jako plik cookie (przy użyciu domyślnego kątowego konwencji nazewnictwa opisane powyżej):</span><span class="sxs-lookup"><span data-stu-id="34893-223">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described above):</span></span>


```c#
public void Configure(IApplicationBuilder app, 
    IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;
        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // We can send the request token as a JavaScript-readable cookie, 
            // and Angular will use it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
    //
}
```

### <a name="options"></a><span data-ttu-id="34893-224">Opcje</span><span class="sxs-lookup"><span data-stu-id="34893-224">Options</span></span>

<span data-ttu-id="34893-225">Można dostosować [antiforgery opcje](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="34893-225">You can customize [antiforgery options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) in `ConfigureServices`:</span></span>

```c#
services.AddAntiforgery(options => 
{
  options.CookieDomain = "mydomain.com";
  options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
  options.CookiePath = "Path";
  options.FormFieldName = "AntiforgeryFieldname";
  options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
  options.RequireSsl = false;
  options.SuppressXFrameOptionsHeader = false;
});
```

<!-- QAfix fix table -->

|<span data-ttu-id="34893-226">Opcja</span><span class="sxs-lookup"><span data-stu-id="34893-226">Option</span></span>        | <span data-ttu-id="34893-227">Opis</span><span class="sxs-lookup"><span data-stu-id="34893-227">Description</span></span> |
|------------- | ----------- |
|<span data-ttu-id="34893-228">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="34893-228">CookieDomain</span></span>  | <span data-ttu-id="34893-229">Domena pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="34893-229">The domain of the cookie.</span></span> <span data-ttu-id="34893-230">Domyślnie `null`.</span><span class="sxs-lookup"><span data-stu-id="34893-230">Defaults to `null`.</span></span> |
|<span data-ttu-id="34893-231">Nazwę CookieName</span><span class="sxs-lookup"><span data-stu-id="34893-231">CookieName</span></span>    | <span data-ttu-id="34893-232">Nazwa pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="34893-232">The name of the cookie.</span></span> <span data-ttu-id="34893-233">Jeśli nie jest ustawiona, system wygeneruje unikatowa nazwa zaczyna się od `DefaultCookiePrefix` (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="34893-233">If not set, the system will generate a unique name beginning with the `DefaultCookiePrefix` (".AspNetCore.Antiforgery.").</span></span> |
|<span data-ttu-id="34893-234">CookiePath</span><span class="sxs-lookup"><span data-stu-id="34893-234">CookiePath</span></span>    | <span data-ttu-id="34893-235">Ścieżka ustawiona w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="34893-235">The path set on the cookie.</span></span> |
|<span data-ttu-id="34893-236">NazwaPolaFormularza</span><span class="sxs-lookup"><span data-stu-id="34893-236">FormFieldName</span></span> | <span data-ttu-id="34893-237">Nazwa ukryte pole formularza używany przez antiforgery system do renderowania antiforgery tokenów w widokach.</span><span class="sxs-lookup"><span data-stu-id="34893-237">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
|<span data-ttu-id="34893-238">HeaderName</span><span class="sxs-lookup"><span data-stu-id="34893-238">HeaderName</span></span>    | <span data-ttu-id="34893-239">Nazwa nagłówka używany przez antiforgery system.</span><span class="sxs-lookup"><span data-stu-id="34893-239">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="34893-240">Jeśli `null`, system będzie uwzględniać tylko dane formularza.</span><span class="sxs-lookup"><span data-stu-id="34893-240">If `null`, the system will consider only form data.</span></span> |
|<span data-ttu-id="34893-241">parametru requireSsl</span><span class="sxs-lookup"><span data-stu-id="34893-241">RequireSsl</span></span>    | <span data-ttu-id="34893-242">Określa, czy protokół SSL jest wymagany przez antiforgery system.</span><span class="sxs-lookup"><span data-stu-id="34893-242">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="34893-243">Domyślnie `false`.</span><span class="sxs-lookup"><span data-stu-id="34893-243">Defaults to `false`.</span></span> <span data-ttu-id="34893-244">Jeśli `true`, żądania bez użycia protokołu SSL zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="34893-244">If `true`, non-SSL requests will fail.</span></span> |
|<span data-ttu-id="34893-245">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="34893-245">SuppressXFrameOptionsHeader</span></span>  | <span data-ttu-id="34893-246">Określa, czy pominąć Generowanie `X-Frame-Options` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="34893-246">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="34893-247">Domyślnie nagłówek jest generowany z wartością "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="34893-247">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="34893-248">Domyślnie `false`.</span><span class="sxs-lookup"><span data-stu-id="34893-248">Defaults to `false`.</span></span> |

<span data-ttu-id="34893-249">Zobacz https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="34893-249">See https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions for more info.</span></span>

### <a name="extending-antiforgery"></a><span data-ttu-id="34893-250">Rozszerzanie Antiforgery</span><span class="sxs-lookup"><span data-stu-id="34893-250">Extending Antiforgery</span></span>

<span data-ttu-id="34893-251">[IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) typu umożliwia deweloperom rozszerzyć zachowanie systemu anti-XSRF przez dwustronną komunikację w każdym token dodatkowe dane.</span><span class="sxs-lookup"><span data-stu-id="34893-251">The [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-XSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="34893-252">[GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) metoda jest wywoływana za każdym razem zostanie wygenerowany token pola i wartość zwracana jest osadzony w wygenerowany token.</span><span class="sxs-lookup"><span data-stu-id="34893-252">The [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="34893-253">Realizator może zwracać sygnaturę czasową, identyfikatora jednorazowego lub wszelkie inne wartości, a następnie wywołać [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) do sprawdzania poprawności danych podczas weryfikowania tokenu.</span><span class="sxs-lookup"><span data-stu-id="34893-253">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) to validate this data when the token is validated.</span></span> <span data-ttu-id="34893-254">Nazwa użytkownika klienta już jest osadzony w generowanych tokenach, więc nie trzeba wpisywać te informacje.</span><span class="sxs-lookup"><span data-stu-id="34893-254">The client's username is already embedded in the generated tokens, so there is no need to include this information.</span></span> <span data-ttu-id="34893-255">Jeśli token zawiera dane dodatkowe, ale nie `IAntiForgeryAdditionalDataProvider` został skonfigurowany, dane dodatkowe nie jest weryfikowany.</span><span class="sxs-lookup"><span data-stu-id="34893-255">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` has been configured, the supplemental data is not validated.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="34893-256">Podstawowe założenia</span><span class="sxs-lookup"><span data-stu-id="34893-256">Fundamentals</span></span>

<span data-ttu-id="34893-257">CSRF ataki polegają na domyślne zachowanie przeglądarki przesyłać pliki cookie skojarzone z domeną z wszystkie żądania skierowane do tej domeny.</span><span class="sxs-lookup"><span data-stu-id="34893-257">CSRF attacks rely on the default browser behavior of sending cookies associated with a domain with every request made to that domain.</span></span> <span data-ttu-id="34893-258">Te pliki cookie są przechowywane w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="34893-258">These cookies are stored within the browser.</span></span> <span data-ttu-id="34893-259">Często zawierają one plików cookie sesji dla uwierzytelnionych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="34893-259">They frequently include session cookies for authenticated users.</span></span> <span data-ttu-id="34893-260">Plik cookie uwierzytelniania jest popularnych formy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="34893-260">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="34893-261">Systemami uwierzytelniania opartego na tokenie ma zostały rośnie w popularne, szczególnie w przypadku źródła i innych scenariuszy "inteligentne klienta".</span><span class="sxs-lookup"><span data-stu-id="34893-261">Token-based authentication systems have been growing in popularity, especially for SPAs and other "smart client" scenarios.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="34893-262">Uwierzytelnianie na podstawie plików cookie</span><span class="sxs-lookup"><span data-stu-id="34893-262">Cookie-based authentication</span></span>

<span data-ttu-id="34893-263">Gdy użytkownik został uwierzytelniony przy użyciu nazwy użytkownika i hasła, wystawiane token, który może służyć do ich identyfikacji i weryfikacji, czy zostały uwierzytelnione.</span><span class="sxs-lookup"><span data-stu-id="34893-263">Once a user has authenticated using their username and password, they are issued a token that can be used to identify them and validate that they have been authenticated.</span></span> <span data-ttu-id="34893-264">Token jest przechowywany jako sprawia, że plik cookie dołączona każde żądanie klienta.</span><span class="sxs-lookup"><span data-stu-id="34893-264">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="34893-265">Generowanie i sprawdzanie poprawności ten plik cookie odbywa się przez oprogramowanie pośredniczące uwierzytelniania plików cookie.</span><span class="sxs-lookup"><span data-stu-id="34893-265">Generating and validating this cookie is done by the cookie authentication middleware.</span></span> <span data-ttu-id="34893-266">Platformy ASP.NET Core zawiera plik cookie [oprogramowanie pośredniczące](../fundamentals/middleware.md) co serializuje głównej nazwy użytkownika do zaszyfrowanego pliku cookie i kolejne żądania sprawdza poprawność pliku cookie, odtwarza podmiot zabezpieczeń i przypisuje go do `User` właściwości `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="34893-266">ASP.NET Core provides cookie [middleware](../fundamentals/middleware.md) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal and assigns it to the `User` property on `HttpContext`.</span></span>

<span data-ttu-id="34893-267">Gdy używany jest plik cookie, plik cookie uwierzytelniania jest tylko kontenerem dla biletu uwierzytelniania formularzy.</span><span class="sxs-lookup"><span data-stu-id="34893-267">When a cookie is used, The authentication cookie is just a container for the forms authentication ticket.</span></span> <span data-ttu-id="34893-268">Bilet jest przekazywany jako wartość pliku cookie uwierzytelniania formularzy z każdym żądaniem i jest używany przez uwierzytelnianie formularzy, na serwerze, aby zidentyfikować uwierzytelnionego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="34893-268">The ticket is passed as the value of the forms authentication cookie with each request and is used by forms authentication, on the server, to identify an authenticated user.</span></span>

<span data-ttu-id="34893-269">Gdy użytkownik jest zalogowany do systemu, sesja użytkownika jest tworzony po stronie serwera i są przechowywane w bazie danych lub innych magazynu trwałego.</span><span class="sxs-lookup"><span data-stu-id="34893-269">When a user is logged in to a system, a user session is created on the server-side and is stored in a database or some other persistent store.</span></span> <span data-ttu-id="34893-270">System generuje klucz sesji wskazujące rzeczywiste sesji w magazynie danych i są wysyłane jako plik cookie po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="34893-270">The system generates a session key that points to the actual session in the data store and it is sent as a client side cookie.</span></span> <span data-ttu-id="34893-271">Serwer sieci web będzie sprawdzać ten klucz sesji żadnych żądaniem użytkownika z zasobem, który wymaga autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="34893-271">The web server will check this session key any time a user requests a resource that requires authorization.</span></span> <span data-ttu-id="34893-272">System sprawdza, czy sesja skojarzonego użytkownika ma uprawnienia dostępu do żądanego zasobu.</span><span class="sxs-lookup"><span data-stu-id="34893-272">The system checks whether the associated user session has the privilege to access the requested resource.</span></span> <span data-ttu-id="34893-273">Jeśli tak, nadal żądania.</span><span class="sxs-lookup"><span data-stu-id="34893-273">If so, the request continues.</span></span> <span data-ttu-id="34893-274">W przeciwnym razie żądania zwraca w postaci nie autoryzowanych.</span><span class="sxs-lookup"><span data-stu-id="34893-274">Otherwise, the request returns as not authorized.</span></span> <span data-ttu-id="34893-275">W takie podejście pliki cookie służą do tworzenia aplikacji jest stanowa, ponieważ jest w stanie "Pamiętaj" który użytkownik wcześniej uwierzytelnił się z serwerem.</span><span class="sxs-lookup"><span data-stu-id="34893-275">In this approach, cookies are used to make the application appear to be stateful, since it is able to "remember" that the user has previously authenticated with the server.</span></span>

### <a name="user-tokens"></a><span data-ttu-id="34893-276">Tokeny użytkownika</span><span class="sxs-lookup"><span data-stu-id="34893-276">User tokens</span></span>

<span data-ttu-id="34893-277">Uwierzytelnianie na podstawie tokenu nie przechowuje sesji na serwerze.</span><span class="sxs-lookup"><span data-stu-id="34893-277">Token-based authentication doesn’t store session on the server.</span></span> <span data-ttu-id="34893-278">Zamiast tego gdy użytkownik jest zalogowany są wydawane token (nie antiforgery token).</span><span class="sxs-lookup"><span data-stu-id="34893-278">Instead, when a user is logged in they are issued a token (not an antiforgery token).</span></span> <span data-ttu-id="34893-279">Token ten zawiera wszystkie dane, które jest wymagane do weryfikacji tokenu.</span><span class="sxs-lookup"><span data-stu-id="34893-279">This token holds all the data that is required to validate the token.</span></span> <span data-ttu-id="34893-280">Zawiera także informacje o użytkowniku w formie [oświadczeń](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span><span class="sxs-lookup"><span data-stu-id="34893-280">It also contains user information, in the form of [claims](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span></span> <span data-ttu-id="34893-281">Gdy użytkownik chce, aby uzyskać dostęp do zasobów serwera wymaga uwierzytelnienia, token jest wysyłany na serwer z nagłówkiem dodatkowe autoryzacji w formie {tokenu} elementu nośnego.</span><span class="sxs-lookup"><span data-stu-id="34893-281">When a user wants to access a server resource requiring authentication, the token is sent to the server with an additional authorization header in form of Bearer {token}.</span></span> <span data-ttu-id="34893-282">Dzięki temu aplikacji bezstanowych, ponieważ w kolejnych żądań token jest przekazywany w żądania do weryfikacji po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="34893-282">This makes the application stateless since in each subsequent request the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="34893-283">Ten token nie jest *zaszyfrowanych*; jest raczej *zakodowane*.</span><span class="sxs-lookup"><span data-stu-id="34893-283">This token is not *encrypted*; rather it is *encoded*.</span></span> <span data-ttu-id="34893-284">Po stronie serwera token może zostać odczytany na dostęp do nieprzetworzonej informacji w tokenie.</span><span class="sxs-lookup"><span data-stu-id="34893-284">On the server-side the token can be decoded to access the raw information within the token.</span></span> <span data-ttu-id="34893-285">Aby wysłać token w kolejnych żądań, można przechowywać go w magazynie lokalnym w przeglądarce lub w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="34893-285">To send the token in subsequent requests, you can either store it in browser’s local storage or in a cookie.</span></span> <span data-ttu-id="34893-286">Nie trzeba martwić XSRF luki w zabezpieczeniach, jeśli token jest przechowywany w magazynie lokalnym, ale jest to problem, jeśli token jest przechowywany w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="34893-286">You don’t have to worry about XSRF vulnerability if your token is stored in the local storage, but it is an issue if the token is stored in a cookie.</span></span>

### <a name="multiple-applications-are-hosted-in-one-domain"></a><span data-ttu-id="34893-287">Wiele aplikacji znajdują się w jednej domenie</span><span class="sxs-lookup"><span data-stu-id="34893-287">Multiple applications are hosted in one domain</span></span>

<span data-ttu-id="34893-288">Mimo że `example1.cloudapp.net` i `example2.cloudapp.net` są różnych hostach, istnieje relacja zaufania niejawne między wszystkie hosty w `*.cloudapp.net` domeny.</span><span class="sxs-lookup"><span data-stu-id="34893-288">Even though `example1.cloudapp.net` and `example2.cloudapp.net` are different hosts, there is an implicit trust relationship between all hosts under the `*.cloudapp.net` domain.</span></span> <span data-ttu-id="34893-289">Ta relacja zaufania niejawne umożliwia potencjalnie niezaufane hosty wpływa na siebie nawzajem pliki cookie (zasad tego samego źródła, które będą zarządzały sposobem żądania AJAX nie zawsze dotyczą plików cookie protokołu HTTP).</span><span class="sxs-lookup"><span data-stu-id="34893-289">This implicit trust relationship allows potentially untrusted hosts to affect each other’s cookies (the same-origin policies that govern AJAX requests do not necessarily apply to HTTP cookies).</span></span> <span data-ttu-id="34893-290">Środowisko uruchomieniowe platformy ASP.NET Core zawiera pewne środki zaradcze, nazwa użytkownika jest osadzony w token pola, tak więc nawet, jeśli złośliwe poddomeny jest w stanie zastąpić tokenu sesji będzie nie można wygenerować pola prawidłowy token dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="34893-290">The ASP.NET Core runtime provides some mitigation in that the username is embedded into the field token, so even if a malicious subdomain is able to overwrite a session token it will be unable to generate a valid field token for the user.</span></span> <span data-ttu-id="34893-291">Jednak w przypadku hostowania w takim środowisku procedury wbudowanych anti-XSRF nadal nie obrony przed przejęcie kontroli sesji lub logowania CSRF ataków.</span><span class="sxs-lookup"><span data-stu-id="34893-291">However, when hosted in such an environment the built-in anti-XSRF routines still cannot defend against session hijacking or login CSRF attacks.</span></span> <span data-ttu-id="34893-292">Udostępnionych środowiskach macierzystych są vunerable przejęcie kontroli sesji logowania CSRF i inne ataki.</span><span class="sxs-lookup"><span data-stu-id="34893-292">Shared hosting environments are vunerable to session hijacking, login CSRF, and other attacks.</span></span>


### <a name="additional-resources"></a><span data-ttu-id="34893-293">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="34893-293">Additional Resources</span></span>

* <span data-ttu-id="34893-294">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) na [Otwórz projekt zabezpieczeń aplikacji sieci Web](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="34893-294">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
