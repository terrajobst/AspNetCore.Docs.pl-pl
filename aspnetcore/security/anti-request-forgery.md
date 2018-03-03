---
title: "Zapobiegaj Cross-Site żądania (XSRF/CSRF) fałszerstwie w ASP.NET Core"
author: steve-smith
description: "Wykryj jak nie dopuścić do ataków na aplikacje sieci web, w którym złośliwą witrynę sieci Web może mieć wpływ interakcji między przeglądarką klienta i aplikacji."
manager: wpickett
ms.author: riande
ms.date: 7/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: 80651a3c3e4c722e0cb96d7cc07de366819f8d1d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="8292f-103">Zapobiegaj Cross-Site żądania (XSRF/CSRF) fałszerstwie w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8292f-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="8292f-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8292f-104">[Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-attack-does-anti-forgery-prevent"></a><span data-ttu-id="8292f-105">Jakie ataku uniemożliwia zabezpieczający przed sfałszowaniem?</span><span class="sxs-lookup"><span data-stu-id="8292f-105">What attack does anti-forgery prevent?</span></span>

<span data-ttu-id="8292f-106">Fałszowanie żądań między witrynami (znanej także jako XSRF lub CSRF, Wymowa *surf zobacz*) jest atak wykorzystujący aplikacje obsługiwane w sieci web, zgodnie z którymi złośliwa witryna sieci web może mieć wpływ interakcji między przeglądarką klienta i witryny sieci web, które ufają Przeglądarka.</span><span class="sxs-lookup"><span data-stu-id="8292f-106">Cross-site request forgery (also known as XSRF or CSRF, pronounced *see-surf*) is an attack against web-hosted applications whereby a malicious web site can influence the interaction between a client browser and a web site that trusts that browser.</span></span> <span data-ttu-id="8292f-107">Tego rodzaju ataki są możliwe, ponieważ przeglądarki sieci web wysyłać niektóre typy tokenów uwierzytelniania automatycznie z każdego żądania do witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="8292f-107">These attacks are made possible because web browsers send some types of authentication tokens automatically with every request to a web site.</span></span> <span data-ttu-id="8292f-108">Ten formularz wykorzystać do znanej także jako *ataku jednym kliknięciem* lub jako *sesji jazda*, ponieważ wykorzystuje ataku użytkownika wcześniej uwierzytelniona sesji.</span><span class="sxs-lookup"><span data-stu-id="8292f-108">This form of exploit's also known as a *one-click attack* or as *session riding*, because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="8292f-109">Przykład atak CSRF:</span><span class="sxs-lookup"><span data-stu-id="8292f-109">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="8292f-110">Użytkownicy logujący się do `www.example.com`, uwierzytelnianie formularzy przy użyciu.</span><span class="sxs-lookup"><span data-stu-id="8292f-110">A user logs into `www.example.com`, using forms authentication.</span></span>
2. <span data-ttu-id="8292f-111">Serwer uwierzytelnia użytkownika i generuje odpowiedzi, który zawiera plik cookie uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="8292f-111">The server authenticates the user and issues a response that includes an authentication cookie.</span></span>
3. <span data-ttu-id="8292f-112">Użytkownik odwiedzi złośliwą witrynę.</span><span class="sxs-lookup"><span data-stu-id="8292f-112">The user visits a malicious site.</span></span>

   <span data-ttu-id="8292f-113">Złośliwa witryna zawiera formularza HTML podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="8292f-113">The malicious site contains an HTML form similar to the following:</span></span>

   ```html
   <h1>You Are a Winner!</h1>
   <form action="http://example.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click Me">
   </form>
   ```

<span data-ttu-id="8292f-114">Należy zauważyć, że akcja formularza zapisuje do lokacji narażony, nie niebezpiecznej witryny.</span><span class="sxs-lookup"><span data-stu-id="8292f-114">Notice that the form action posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="8292f-115">Jest to część CSRF "cross-site".</span><span class="sxs-lookup"><span data-stu-id="8292f-115">This is the “cross-site” part of CSRF.</span></span>

4. <span data-ttu-id="8292f-116">Użytkownik klika przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="8292f-116">The user clicks the submit button.</span></span> <span data-ttu-id="8292f-117">Przeglądarka automatycznie uwzględnia pliku cookie uwierzytelniania dla żądanej domeny (narażone lokacji w tym przypadku) z żądaniem.</span><span class="sxs-lookup"><span data-stu-id="8292f-117">The browser automatically includes the authentication cookie for the requested domain (the vulnerable site in this case) with the request.</span></span>
5. <span data-ttu-id="8292f-118">Żądanie działa na serwerze z kontekstem uwierzytelniania użytkownika i mogą wykonywać żadnych czynności, że uwierzytelniony użytkownik może wykonywać.</span><span class="sxs-lookup"><span data-stu-id="8292f-118">The request runs on the server with the user's authentication context and can perform any action that an authenticated user is allowed to do.</span></span>

<span data-ttu-id="8292f-119">W tym przykładzie wymaga od użytkownika kliknij przycisk formularza.</span><span class="sxs-lookup"><span data-stu-id="8292f-119">This example requires the user to click the form button.</span></span> <span data-ttu-id="8292f-120">Strony złośliwych można:</span><span class="sxs-lookup"><span data-stu-id="8292f-120">The malicious page could:</span></span>

* <span data-ttu-id="8292f-121">Uruchom skrypt, który automatycznie wyśle formularz.</span><span class="sxs-lookup"><span data-stu-id="8292f-121">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="8292f-122">Wysyła żądanie AJAX przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="8292f-122">Sends a form submission as an AJAX request.</span></span> 
* <span data-ttu-id="8292f-123">Formularz ukryty z CSS.</span><span class="sxs-lookup"><span data-stu-id="8292f-123">Use a hidden form with CSS.</span></span> 

<span data-ttu-id="8292f-124">Przy użyciu protokołu SSL nie uniemożliwia atak CSRF, złośliwa witryna można wysyłać `https://` żądania.</span><span class="sxs-lookup"><span data-stu-id="8292f-124">Using SSL doesn't prevent a CSRF attack, the malicious site can send an `https://` request.</span></span> 

<span data-ttu-id="8292f-125">Niektóre ataki docelowych punktów końcowych witryny, które odpowiadają na `GET` żądania, w których przypadku tag obrazu może służyć do wykonywania akcji (Ta forma ataku jest typowe na forum witryn, które zezwala na obrazy, ale zablokowanie JavaScript).</span><span class="sxs-lookup"><span data-stu-id="8292f-125">Some attacks target site endpoints that respond to `GET` requests, in which case an image tag can be used to perform the action (this form of attack is common on forum sites that permit images but block JavaScript).</span></span> <span data-ttu-id="8292f-126">Aplikacje, które spowodują zmianę stanu z `GET` żądania są narażone przed złośliwymi atakami.</span><span class="sxs-lookup"><span data-stu-id="8292f-126">Applications that change state with `GET` requests are vulnerable from malicious attacks.</span></span>

<span data-ttu-id="8292f-127">Ataki CSRF są możliwe do przed witryn sieci web, które używają plików cookie do uwierzytelniania, ponieważ przeglądarki wysyłać wszystkich odpowiednich plików cookie do docelowej witryny sieci web.</span><span class="sxs-lookup"><span data-stu-id="8292f-127">CSRF attacks are possible against web sites that use cookies for authentication, because browsers send all relevant cookies to the destination web site.</span></span> <span data-ttu-id="8292f-128">Jednak ataków CSRF nie są ograniczone do wykorzystania plików cookie.</span><span class="sxs-lookup"><span data-stu-id="8292f-128">However, CSRF attacks are not limited to exploiting cookies.</span></span> <span data-ttu-id="8292f-129">Na przykład uwierzytelnianie podstawowe i szyfrowane również są zagrożone.</span><span class="sxs-lookup"><span data-stu-id="8292f-129">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="8292f-130">Po zalogowaniu się użytkownika za pomocą uwierzytelnianie podstawowe lub szyfrowane, przeglądarka automatycznie wysyła poświadczenia zakończenia sesji.</span><span class="sxs-lookup"><span data-stu-id="8292f-130">After a user logs in with Basic or Digest authentication, the browser automatically sends the credentials until the session ends.</span></span>

<span data-ttu-id="8292f-131">Uwaga: W tym kontekście *sesji* odwołuje się do sesji po stronie klienta, w którym użytkownik jest uwierzytelniony.</span><span class="sxs-lookup"><span data-stu-id="8292f-131">Note: In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="8292f-132">Jest niezwiązanych ze sobą do sesji po stronie serwera lub [oprogramowanie pośredniczące sesji](xref:fundamentals/app-state).</span><span class="sxs-lookup"><span data-stu-id="8292f-132">It's unrelated to server-side sessions or [session middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="8292f-133">Użytkownicy mogą ochronić CSRF luki w zabezpieczeniach poprzez:</span><span class="sxs-lookup"><span data-stu-id="8292f-133">Users can guard against CSRF vulnerabilities by:</span></span>
* <span data-ttu-id="8292f-134">Wylogowując się witryn sieci web, gdy zostało ukończone z nich korzystać.</span><span class="sxs-lookup"><span data-stu-id="8292f-134">Logging off of web sites when they have finished using them.</span></span>
* <span data-ttu-id="8292f-135">Okresowo wyczyszczenie plików cookie w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="8292f-135">Clearing their browser's cookies periodically.</span></span>

<span data-ttu-id="8292f-136">Jednak luk w zabezpieczeniach CSRF są zasadniczo problem z aplikacji sieci web, a nie użytkownika końcowego.</span><span class="sxs-lookup"><span data-stu-id="8292f-136">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="how-does-aspnet-core-mvc-address-csrf"></a><span data-ttu-id="8292f-137">W jaki sposób program ASP.NET Core MVC uwzględnia CSRF</span><span class="sxs-lookup"><span data-stu-id="8292f-137">How does ASP.NET Core MVC address CSRF?</span></span>

> [!WARNING]
> <span data-ttu-id="8292f-138">Implementuje platformy ASP.NET Core za pomocą przeciwko request forgery [stosu ochrony danych platformy ASP.NET Core](xref:security/data-protection/introduction).</span><span class="sxs-lookup"><span data-stu-id="8292f-138">ASP.NET Core implements anti-request-forgery using the [ASP.NET Core data protection stack](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="8292f-139">Ochrona danych platformy ASP.NET Core musi być skonfigurowana do pracy w farmie serwerów.</span><span class="sxs-lookup"><span data-stu-id="8292f-139">ASP.NET Core data protection must be configured to work in a server farm.</span></span> <span data-ttu-id="8292f-140">Zobacz [konfigurowania ochrony danych](xref:security/data-protection/configuration/overview) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="8292f-140">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

<span data-ttu-id="8292f-141">Konfiguracja ochrony danych przeciwko request forgery domyślnego platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8292f-141">ASP.NET Core anti-request-forgery default data protection configuration</span></span> 

<span data-ttu-id="8292f-142">W programie ASP.NET 2.0 MVC Core [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects tokenów zabezpieczających przed sfałszowaniem elementów formularza HTML.</span><span class="sxs-lookup"><span data-stu-id="8292f-142">In ASP.NET Core MVC 2.0 the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects anti-forgery tokens for HTML form elements.</span></span> <span data-ttu-id="8292f-143">Na przykład następujący kod w pliku Razor automatycznie wygeneruje tokenów zabezpieczających przed sfałszowaniem:</span><span class="sxs-lookup"><span data-stu-id="8292f-143">For example, the following markup in a Razor file will automatically generate anti-forgery tokens:</span></span>

```html
<form method="post">
  <!-- form markup -->
</form>
```

<span data-ttu-id="8292f-144">Automatyczne generowanie tokenów zabezpieczających przed sfałszowaniem elementów formularza HTML się stanie, gdy:</span><span class="sxs-lookup"><span data-stu-id="8292f-144">The automatic generation of anti-forgery tokens for HTML form elements happens when:</span></span>

* <span data-ttu-id="8292f-145">`form` Tag zawiera `method="post"` atrybutu i</span><span class="sxs-lookup"><span data-stu-id="8292f-145">The `form` tag contains the `method="post"` attribute AND</span></span>

  * <span data-ttu-id="8292f-146">Atrybut akcji jest pusty.</span><span class="sxs-lookup"><span data-stu-id="8292f-146">The action attribute is empty.</span></span> <span data-ttu-id="8292f-147">( `action=""`) OR</span><span class="sxs-lookup"><span data-stu-id="8292f-147">( `action=""`) OR</span></span>
  * <span data-ttu-id="8292f-148">Nie jest podany w atrybucie akcji.</span><span class="sxs-lookup"><span data-stu-id="8292f-148">The action attribute isn't supplied.</span></span> <span data-ttu-id="8292f-149">(`<form method="post">`)</span><span class="sxs-lookup"><span data-stu-id="8292f-149">(`<form method="post">`)</span></span>

<span data-ttu-id="8292f-150">Możesz wyłączyć automatyczne generowanie tokenów zabezpieczających przed sfałszowaniem elementów formularza HTML przez:</span><span class="sxs-lookup"><span data-stu-id="8292f-150">You can disable automatic generation of anti-forgery tokens for HTML form elements by:</span></span>

* <span data-ttu-id="8292f-151">Jawnie wyłącza `asp-antiforgery`.</span><span class="sxs-lookup"><span data-stu-id="8292f-151">Explicitly disabling `asp-antiforgery`.</span></span> <span data-ttu-id="8292f-152">Na przykład</span><span class="sxs-lookup"><span data-stu-id="8292f-152">For example</span></span>

  ```html
  <form method="post" asp-antiforgery="false">
  </form>
  ```

* <span data-ttu-id="8292f-153">OPT element form poza pomocników tagów przy użyciu Pomocnika tagów [! symbol Wypisz](xref:mvc/views/tag-helpers/intro#opt-out).</span><span class="sxs-lookup"><span data-stu-id="8292f-153">Opt the form element out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out).</span></span>

  ```html
  <!form method="post">
  </!form>
  ```

* <span data-ttu-id="8292f-154">Usuń `FormTagHelper` z widoku.</span><span class="sxs-lookup"><span data-stu-id="8292f-154">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="8292f-155">Możesz usunąć `FormTagHelper` z widoku przez dodanie do widoku Razor następujące dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="8292f-155">You can remove the `FormTagHelper` from a view by adding the following directive to the Razor view:</span></span>

  ```html
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="8292f-156">[Stron razor](xref:mvc/razor-pages/index) są automatycznie chronione przed XSRF/CSRF.</span><span class="sxs-lookup"><span data-stu-id="8292f-156">[Razor Pages](xref:mvc/razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="8292f-157">Nie trzeba zapisać jakiegokolwiek dodatkowego kodu.</span><span class="sxs-lookup"><span data-stu-id="8292f-157">You don't have to write any additional code.</span></span> <span data-ttu-id="8292f-158">Zobacz [XSRF/CSRF i stron Razor](xref:mvc/razor-pages/index#xsrf) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="8292f-158">See [XSRF/CSRF and Razor Pages](xref:mvc/razor-pages/index#xsrf) for more information.</span></span>

<span data-ttu-id="8292f-159">Najbardziej typowym podejściem do obrony przed atakami CSRF jest wzorzec tokenu Synchronizatora (STP).</span><span class="sxs-lookup"><span data-stu-id="8292f-159">The most common approach to defending against CSRF attacks is the synchronizer token pattern (STP).</span></span> <span data-ttu-id="8292f-160">STP to technika używany, gdy użytkownik zażąda strony z danych formularza.</span><span class="sxs-lookup"><span data-stu-id="8292f-160">STP is a technique used when the user requests a page with form data.</span></span> <span data-ttu-id="8292f-161">Serwer wysyła token skojarzone z tożsamością bieżącego użytkownika do klienta.</span><span class="sxs-lookup"><span data-stu-id="8292f-161">The server sends a token associated with the current user's identity to the client.</span></span> <span data-ttu-id="8292f-162">Klient wysyła ponownie token do serwera w celu weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="8292f-162">The client sends back the token to the server for verification.</span></span> <span data-ttu-id="8292f-163">Jeśli serwer odbiera token, który nie odpowiada tożsamość uwierzytelnionego użytkownika, żądanie zostanie odrzucone.</span><span class="sxs-lookup"><span data-stu-id="8292f-163">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span> <span data-ttu-id="8292f-164">Token jest unikatowy i nieprzewidywalny.</span><span class="sxs-lookup"><span data-stu-id="8292f-164">The token is unique and unpredictable.</span></span> <span data-ttu-id="8292f-165">Token mogą służyć do zapewnienia prawidłowego sekwencjonowania szereg żądań (strona zapewnienie 1 poprzedza strona 2 poprzedza strony 3).</span><span class="sxs-lookup"><span data-stu-id="8292f-165">The token can also be used to ensure proper sequencing of a series of requests (ensuring page 1 precedes page 2 which precedes page 3).</span></span> <span data-ttu-id="8292f-166">Wszystkich formularzy w szablonach platformy ASP.NET Core MVC generowania antiforgery tokenów.</span><span class="sxs-lookup"><span data-stu-id="8292f-166">All the forms in ASP.NET Core MVC templates generate antiforgery tokens.</span></span> <span data-ttu-id="8292f-167">Dwa poniższe przykłady logiki widoku generowania tokenów antiforgery:</span><span class="sxs-lookup"><span data-stu-id="8292f-167">The following two examples of view logic generate antiforgery tokens:</span></span>

```html
<form asp-controller="Manage" asp-action="ChangePassword" method="post">

</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    
}
```

<span data-ttu-id="8292f-168">Należy jawnie dodać antiforgery token do `<form>` elementu bez użycia pomocników tagów z Pomocnika kodu HTML `@Html.AntiForgeryToken`:</span><span class="sxs-lookup"><span data-stu-id="8292f-168">You can explicitly add an antiforgery token to a `<form>` element without using tag helpers with the HTML helper `@Html.AntiForgeryToken`:</span></span>


```html
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="8292f-169">W każdym z powyższych przypadków platformy ASP.NET Core doda ukryte pole formularza podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="8292f-169">In each of the preceding cases, ASP.NET Core will add a hidden form field similar to the following:</span></span>
```html
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkSldwD9CpLRyOtm6FiJB1Jr_F3FQJQDvhlHoLNJJrLA6zaMUmhjMsisu2D2tFkAiYgyWQawJk9vNm36sYP1esHOtamBEPvSk1_x--Sg8Ey2a-d9CV2zHVWIN9MVhvKHOSyKqdZFlYDVd69XYx-rOWPw3ilHGLN6K0Km-1p83jZzF0E4WU5OGg5ns2-m9Yw">
```

<span data-ttu-id="8292f-170">Platformy ASP.NET Core obejmuje trzy [filtry](xref:mvc/controllers/filters) do pracy z tokenami antiforgery: `ValidateAntiForgeryToken`, `AutoValidateAntiforgeryToken`, i `IgnoreAntiforgeryToken`.</span><span class="sxs-lookup"><span data-stu-id="8292f-170">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens: `ValidateAntiForgeryToken`, `AutoValidateAntiforgeryToken`, and `IgnoreAntiforgeryToken`.</span></span>

### <a name="validateantiforgerytoken"></a><span data-ttu-id="8292f-171">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="8292f-171">ValidateAntiForgeryToken</span></span>

<span data-ttu-id="8292f-172">`ValidateAntiForgeryToken` Jest filtr akcji, który można zastosować do poszczególnych akcji kontrolera, lub globalnie.</span><span class="sxs-lookup"><span data-stu-id="8292f-172">The `ValidateAntiForgeryToken` is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="8292f-173">Żądania kierowane do akcji, które mają ten filtr zostanie zablokowane, jeśli żądanie zawiera prawidłowy token antiforgery.</span><span class="sxs-lookup"><span data-stu-id="8292f-173">Requests made to actions that have this filter applied will be blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
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

<span data-ttu-id="8292f-174">`ValidateAntiForgeryToken` Atrybut wymaga tokenu dla żądań kierowanych do metod akcji go decorates tym `HTTP GET` żądań.</span><span class="sxs-lookup"><span data-stu-id="8292f-174">The `ValidateAntiForgeryToken` attribute requires a token for requests to action methods it decorates, including `HTTP GET` requests.</span></span> <span data-ttu-id="8292f-175">W przypadku zastosowania go szeroko, można zastąpić go przy użyciu `IgnoreAntiforgeryToken` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="8292f-175">If you apply it broadly, you can override it with the `IgnoreAntiforgeryToken` attribute.</span></span>

### <a name="autovalidateantiforgerytoken"></a><span data-ttu-id="8292f-176">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="8292f-176">AutoValidateAntiforgeryToken</span></span>

<span data-ttu-id="8292f-177">Ogólnie rzecz biorąc aplikacji platformy ASP.NET Core nie generować antiforgery tokeny dla bezpieczne metody HTTP (GET, HEAD, opcje i śledzenia).</span><span class="sxs-lookup"><span data-stu-id="8292f-177">ASP.NET Core apps generally don't generate antiforgery tokens for HTTP safe methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="8292f-178">Zamiast stosowania szeroko `ValidateAntiForgeryToken` atrybutu, a następnie przesłanianie go z `IgnoreAntiforgeryToken` atrybutów, można użyć ``AutoValidateAntiforgeryToken`` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="8292f-178">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, you can use the ``AutoValidateAntiforgeryToken`` attribute.</span></span> <span data-ttu-id="8292f-179">Ten atrybut działa identycznie do `ValidateAntiForgeryToken` atrybutów, z wyjątkiem tego, czy nie wymaga tokeny żądań zostało nawiązane przy użyciu następujących metod HTTP:</span><span class="sxs-lookup"><span data-stu-id="8292f-179">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="8292f-180">GET</span><span class="sxs-lookup"><span data-stu-id="8292f-180">GET</span></span>
* <span data-ttu-id="8292f-181">HEAD</span><span class="sxs-lookup"><span data-stu-id="8292f-181">HEAD</span></span>
* <span data-ttu-id="8292f-182">OPCJE</span><span class="sxs-lookup"><span data-stu-id="8292f-182">OPTIONS</span></span>
* <span data-ttu-id="8292f-183">TRACE</span><span class="sxs-lookup"><span data-stu-id="8292f-183">TRACE</span></span>

<span data-ttu-id="8292f-184">Zalecane jest użycie `AutoValidateAntiforgeryToken` szeroko dla scenariuszy-API.</span><span class="sxs-lookup"><span data-stu-id="8292f-184">We recommend you use `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="8292f-185">Dzięki temu akcji POST są chronione przez domyślny.</span><span class="sxs-lookup"><span data-stu-id="8292f-185">This ensures your POST actions are protected by default.</span></span> <span data-ttu-id="8292f-186">Alternatywą jest Ignoruj antiforgery tokeny domyślnie, chyba że `ValidateAntiForgeryToken` jest stosowany do metody akcji indywidualnych.</span><span class="sxs-lookup"><span data-stu-id="8292f-186">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to the individual action method.</span></span> <span data-ttu-id="8292f-187">Bardziej prawdopodobne, w tym scenariuszu dla metody akcji POST się po lewej niechronionej, pozostawiając narażony na ataki CSRF aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8292f-187">It's more likely in this scenario for a POST action method to be left unprotected, leaving your app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="8292f-188">Nawet anonimowe WPISÓW, należy wysłać antiforgery tokenu.</span><span class="sxs-lookup"><span data-stu-id="8292f-188">Even anonymous POSTS should send the antiforgery token.</span></span>

<span data-ttu-id="8292f-189">Uwaga: Interfejsów API nie mają mechanizm automatycznego wysyłania-cookie część tokenu; prawdopodobnie implementacji zależy od implementacji kodu klienta.</span><span class="sxs-lookup"><span data-stu-id="8292f-189">Note: APIs don't have an automatic mechanism for sending the non-cookie part of the token; your implementation will likely depend on your client code implementation.</span></span> <span data-ttu-id="8292f-190">Poniżej przedstawiono kilka przykładów.</span><span class="sxs-lookup"><span data-stu-id="8292f-190">Some examples are shown below.</span></span>

<span data-ttu-id="8292f-191">Przykład (klasa poziom):</span><span class="sxs-lookup"><span data-stu-id="8292f-191">Example (class level):</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="8292f-192">Przykład (globalne):</span><span class="sxs-lookup"><span data-stu-id="8292f-192">Example (global):</span></span>

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

<a name="iaft"></a>

### <a name="ignoreantiforgerytoken"></a><span data-ttu-id="8292f-193">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="8292f-193">IgnoreAntiforgeryToken</span></span>

<span data-ttu-id="8292f-194">`IgnoreAntiforgeryToken` Użyć filtru, aby wyeliminować potrzebę antiforgery token obecności dla danej akcji (lub kontrolera).</span><span class="sxs-lookup"><span data-stu-id="8292f-194">The `IgnoreAntiforgeryToken` filter is used to eliminate the need for an antiforgery token to be present for a given action (or controller).</span></span> <span data-ttu-id="8292f-195">Po zastosowaniu filtru spowoduje zastąpienie `ValidateAntiForgeryToken` i/lub `AutoValidateAntiforgeryToken` filtry określone na wyższym poziomie (globalnie lub w kontrolerze).</span><span class="sxs-lookup"><span data-stu-id="8292f-195">When applied, this filter will override `ValidateAntiForgeryToken` and/or `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

```csharp
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

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="8292f-196">JavaScript, AJAX i źródła</span><span class="sxs-lookup"><span data-stu-id="8292f-196">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="8292f-197">W aplikacjach opartych na języku HTML tradycyjnych antiforgery tokenów są przekazywane do serwera przy użyciu pól ukrytym.</span><span class="sxs-lookup"><span data-stu-id="8292f-197">In traditional HTML-based applications, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="8292f-198">Nowoczesne aplikacje oparte na języku JavaScript i aplikacje jednej strony (źródła) składają się programowo wiele żądań.</span><span class="sxs-lookup"><span data-stu-id="8292f-198">In modern JavaScript-based apps and single page applications (SPAs), many requests are made programmatically.</span></span> <span data-ttu-id="8292f-199">Te żądania AJAX mogą używać innych technik (np. nagłówki żądania lub pliki cookie) do wysyłania tokenu.</span><span class="sxs-lookup"><span data-stu-id="8292f-199">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span> <span data-ttu-id="8292f-200">Jeśli pliki cookie są używane do przechowywania tokeny uwierzytelniania i do uwierzytelniania żądań interfejsu API na serwerze, CSRF będzie potencjalny problem.</span><span class="sxs-lookup"><span data-stu-id="8292f-200">If cookies are used to store authentication tokens and to authenticate API requests on the server, then CSRF will be a potential problem.</span></span> <span data-ttu-id="8292f-201">Jednak jeśli magazyn lokalny jest używany do przechowywania token, CSRF luki w zabezpieczeniach mogą można zminimalizować, ponieważ wartości z magazynu lokalnego nie są automatycznie wysyłane do serwera z każdego nowego żądania.</span><span class="sxs-lookup"><span data-stu-id="8292f-201">However, if local storage is used to store the token, CSRF vulnerability may be mitigated, since values from local storage are not sent automatically to the server with every new request.</span></span> <span data-ttu-id="8292f-202">W związku z tym korzystanie z magazynu lokalnego do przechowywania antiforgery token na kliencie i wysyłania tokenu, ponieważ nagłówek żądania jest zalecane podejście.</span><span class="sxs-lookup"><span data-stu-id="8292f-202">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="angularjs"></a><span data-ttu-id="8292f-203">AngularJS</span><span class="sxs-lookup"><span data-stu-id="8292f-203">AngularJS</span></span>

<span data-ttu-id="8292f-204">AngularJS używa konwencji adres CSRF.</span><span class="sxs-lookup"><span data-stu-id="8292f-204">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="8292f-205">Jeśli serwer wysyła plik cookie o nazwie `XSRF-TOKEN`, kątową `$http` usługi doda wartość z tego pliku cookie do nagłówka podczas wysyłania żądania do tego serwera.</span><span class="sxs-lookup"><span data-stu-id="8292f-205">If the server sends a cookie with the name `XSRF-TOKEN`, the Angular `$http` service will add the value from this cookie to a header when it sends a request to this server.</span></span> <span data-ttu-id="8292f-206">Ten proces odbywa się automatycznie; nie musisz jawnie ustawić nagłówka.</span><span class="sxs-lookup"><span data-stu-id="8292f-206">This process is automatic; you don't need to set the header explicitly.</span></span> <span data-ttu-id="8292f-207">Nazwa nagłówka jest `X-XSRF-TOKEN`.</span><span class="sxs-lookup"><span data-stu-id="8292f-207">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="8292f-208">Serwer powinna wykryć ten nagłówek i zweryfikować jego zawartość.</span><span class="sxs-lookup"><span data-stu-id="8292f-208">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="8292f-209">Dla interfejsu API platformy ASP.NET Core współpracują z tę Konwencję:</span><span class="sxs-lookup"><span data-stu-id="8292f-209">For ASP.NET Core API work with this convention:</span></span>

* <span data-ttu-id="8292f-210">Skonfiguruj aplikację, aby dostarczyć token w pliku cookie o nazwie `XSRF-TOKEN`</span><span class="sxs-lookup"><span data-stu-id="8292f-210">Configure your app to provide a token in a cookie called `XSRF-TOKEN`</span></span>
* <span data-ttu-id="8292f-211">Skonfiguruj usługę antiforgery do wyszukania nagłówek o nazwie `X-XSRF-TOKEN`</span><span class="sxs-lookup"><span data-stu-id="8292f-211">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

<span data-ttu-id="8292f-212">[Przykładowy widok](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span><span class="sxs-lookup"><span data-stu-id="8292f-212">[View sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample).</span></span>

### <a name="javascript"></a><span data-ttu-id="8292f-213">JavaScript</span><span class="sxs-lookup"><span data-stu-id="8292f-213">JavaScript</span></span>

<span data-ttu-id="8292f-214">Przy użyciu języka JavaScript z widokami, można utworzyć tokenu przy użyciu usługi z w danym widoku.</span><span class="sxs-lookup"><span data-stu-id="8292f-214">Using JavaScript with views, you can create the token using a service from within your view.</span></span> <span data-ttu-id="8292f-215">Aby to zrobić, Wstaw `Microsoft.AspNetCore.Antiforgery.IAntiforgery` usługi do widoku i wywołanie `GetAndStoreTokens`, jak pokazano:</span><span class="sxs-lookup"><span data-stu-id="8292f-215">To do so, you inject the `Microsoft.AspNetCore.Antiforgery.IAntiforgery` service into the view and call `GetAndStoreTokens`, as shown:</span></span>

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,28)]

<span data-ttu-id="8292f-216">Takie podejście eliminuje potrzebę bezpośrednio przez ustawienia plików cookie z serwera lub odczytywania ich z klienta.</span><span class="sxs-lookup"><span data-stu-id="8292f-216">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="8292f-217">Powyższy przykład używa jQuery można odczytać wartości pola ukrytego nagłówka AJAX POST.</span><span class="sxs-lookup"><span data-stu-id="8292f-217">The preceding example uses jQuery to read the hidden field value for the AJAX POST header.</span></span> <span data-ttu-id="8292f-218">Aby uzyskać wartość tokenu przy użyciu języka JavaScript, użyj `document.getElementById('RequestVerificationToken').value`.</span><span class="sxs-lookup"><span data-stu-id="8292f-218">To use JavaScript to obtain the token's value, use `document.getElementById('RequestVerificationToken').value`.</span></span>

<span data-ttu-id="8292f-219">JavaScript można także uzyskać dostęp do tokenów podane w plikach cookie, a następnie użyj zawartość pliku cookie do tworzenia nagłówka o wartości tokenu, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="8292f-219">JavaScript can also access tokens provided in cookies, and then use the cookie's contents to create a header with the token's value, as shown below.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
  new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="8292f-220">Następnie, przy założeniu utworzenia skryptu żądania do wysyłania tokenu w nagłówku o nazwie `X-CSRF-TOKEN`, skonfiguruj usługę antiforgery do wyszukania `X-CSRF-TOKEN` nagłówka:</span><span class="sxs-lookup"><span data-stu-id="8292f-220">Then, assuming you construct your script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="8292f-221">W poniższym przykładzie użyto jQuery żądania AJAX z odpowiedni nagłówek:</span><span class="sxs-lookup"><span data-stu-id="8292f-221">The following example uses jQuery to make an AJAX request with the appropriate header:</span></span>

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

## <a name="configuring-antiforgery"></a><span data-ttu-id="8292f-222">Konfigurowanie Antiforgery</span><span class="sxs-lookup"><span data-stu-id="8292f-222">Configuring Antiforgery</span></span>

<span data-ttu-id="8292f-223">`IAntiforgery` udostępnia interfejs API do skonfigurowania antiforgery systemu.</span><span class="sxs-lookup"><span data-stu-id="8292f-223">`IAntiforgery` provides the API to configure the antiforgery system.</span></span> <span data-ttu-id="8292f-224">Może być wymagane w `Configure` metody `Startup` klasy.</span><span class="sxs-lookup"><span data-stu-id="8292f-224">It can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="8292f-225">W poniższym przykładzie użyto oprogramowanie pośredniczące ze strony głównej aplikacji do wygenerowania tokenu antiforgery i wysłać go w odpowiedzi jako plik cookie (przy użyciu domyślnego kątowego konwencji nazewnictwa opisane powyżej):</span><span class="sxs-lookup"><span data-stu-id="8292f-225">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described above):</span></span>


```csharp
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

### <a name="options"></a><span data-ttu-id="8292f-226">Opcje</span><span class="sxs-lookup"><span data-stu-id="8292f-226">Options</span></span>

<span data-ttu-id="8292f-227">Można dostosować [antiforgery opcje](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) w `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8292f-227">You can customize [antiforgery options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions#fields_summary) in `ConfigureServices`:</span></span>

```csharp
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

|<span data-ttu-id="8292f-228">Opcja</span><span class="sxs-lookup"><span data-stu-id="8292f-228">Option</span></span>        | <span data-ttu-id="8292f-229">Opis</span><span class="sxs-lookup"><span data-stu-id="8292f-229">Description</span></span> |
|------------- | ----------- |
|<span data-ttu-id="8292f-230">CookieDomain</span><span class="sxs-lookup"><span data-stu-id="8292f-230">CookieDomain</span></span>  | <span data-ttu-id="8292f-231">Domena pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8292f-231">The domain of the cookie.</span></span> <span data-ttu-id="8292f-232">Domyślnie `null`.</span><span class="sxs-lookup"><span data-stu-id="8292f-232">Defaults to `null`.</span></span> |
|<span data-ttu-id="8292f-233">CookieName</span><span class="sxs-lookup"><span data-stu-id="8292f-233">CookieName</span></span>    | <span data-ttu-id="8292f-234">Nazwa pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8292f-234">The name of the cookie.</span></span> <span data-ttu-id="8292f-235">Jeśli nie jest ustawiona, system wygeneruje unikatowa nazwa zaczyna się od `DefaultCookiePrefix` (". AspNetCore.Antiforgery.").</span><span class="sxs-lookup"><span data-stu-id="8292f-235">If not set, the system will generate a unique name beginning with the `DefaultCookiePrefix` (".AspNetCore.Antiforgery.").</span></span> |
|<span data-ttu-id="8292f-236">CookiePath</span><span class="sxs-lookup"><span data-stu-id="8292f-236">CookiePath</span></span>    | <span data-ttu-id="8292f-237">Ścieżka ustawiona w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8292f-237">The path set on the cookie.</span></span> |
|<span data-ttu-id="8292f-238">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="8292f-238">FormFieldName</span></span> | <span data-ttu-id="8292f-239">Nazwa ukryte pole formularza używany przez antiforgery system do renderowania antiforgery tokenów w widokach.</span><span class="sxs-lookup"><span data-stu-id="8292f-239">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
|<span data-ttu-id="8292f-240">HeaderName</span><span class="sxs-lookup"><span data-stu-id="8292f-240">HeaderName</span></span>    | <span data-ttu-id="8292f-241">Nazwa nagłówka używany przez antiforgery system.</span><span class="sxs-lookup"><span data-stu-id="8292f-241">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="8292f-242">Jeśli `null`, system będzie uwzględniać tylko dane formularza.</span><span class="sxs-lookup"><span data-stu-id="8292f-242">If `null`, the system will consider only form data.</span></span> |
|<span data-ttu-id="8292f-243">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="8292f-243">RequireSsl</span></span>    | <span data-ttu-id="8292f-244">Określa, czy protokół SSL jest wymagany przez antiforgery system.</span><span class="sxs-lookup"><span data-stu-id="8292f-244">Specifies whether SSL is required by the antiforgery system.</span></span> <span data-ttu-id="8292f-245">Domyślnie `false`.</span><span class="sxs-lookup"><span data-stu-id="8292f-245">Defaults to `false`.</span></span> <span data-ttu-id="8292f-246">Jeśli `true`, żądania bez użycia protokołu SSL zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="8292f-246">If `true`, non-SSL requests will fail.</span></span> |
|<span data-ttu-id="8292f-247">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="8292f-247">SuppressXFrameOptionsHeader</span></span> | <span data-ttu-id="8292f-248">Określa, czy pominąć Generowanie `X-Frame-Options` nagłówka.</span><span class="sxs-lookup"><span data-stu-id="8292f-248">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="8292f-249">Domyślnie nagłówek jest generowany z wartością "SAMEORIGIN".</span><span class="sxs-lookup"><span data-stu-id="8292f-249">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="8292f-250">Domyślnie `false`.</span><span class="sxs-lookup"><span data-stu-id="8292f-250">Defaults to `false`.</span></span> |

<span data-ttu-id="8292f-251">Zobacz https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="8292f-251">See https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions for more info.</span></span>

### <a name="extending-antiforgery"></a><span data-ttu-id="8292f-252">Rozszerzanie Antiforgery</span><span class="sxs-lookup"><span data-stu-id="8292f-252">Extending Antiforgery</span></span>

<span data-ttu-id="8292f-253">[IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) typu umożliwia deweloperom rozszerzyć zachowanie systemu anti-XSRF przez dwustronną komunikację w każdym token dodatkowe dane.</span><span class="sxs-lookup"><span data-stu-id="8292f-253">The [IAntiForgeryAdditionalDataProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-XSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="8292f-254">[GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) metoda jest wywoływana za każdym razem zostanie wygenerowany token pola i wartość zwracana jest osadzony w wygenerowany token.</span><span class="sxs-lookup"><span data-stu-id="8292f-254">The [GetAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_GetAdditionalData_Microsoft_AspNetCore_Http_HttpContext_) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="8292f-255">Realizator może zwracać sygnaturę czasową, identyfikatora jednorazowego lub wszelkie inne wartości, a następnie wywołać [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) do sprawdzania poprawności danych podczas weryfikowania tokenu.</span><span class="sxs-lookup"><span data-stu-id="8292f-255">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider#Microsoft_AspNetCore_Antiforgery_IAntiforgeryAdditionalDataProvider_ValidateAdditionalData_Microsoft_AspNetCore_Http_HttpContext_System_String_) to validate this data when the token is validated.</span></span> <span data-ttu-id="8292f-256">Nazwa użytkownika klienta już jest osadzony w generowanych tokenach, więc nie trzeba wpisywać te informacje.</span><span class="sxs-lookup"><span data-stu-id="8292f-256">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="8292f-257">Jeśli token zawiera dane dodatkowe, ale nie `IAntiForgeryAdditionalDataProvider` został skonfigurowany, dane dodatkowe nie jest zweryfikowany.</span><span class="sxs-lookup"><span data-stu-id="8292f-257">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` has been configured, the supplemental data isn't validated.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="8292f-258">Podstawy</span><span class="sxs-lookup"><span data-stu-id="8292f-258">Fundamentals</span></span>

<span data-ttu-id="8292f-259">CSRF ataki polegają na domyślne zachowanie przeglądarki przesyłać pliki cookie skojarzone z domeną z wszystkie żądania skierowane do tej domeny.</span><span class="sxs-lookup"><span data-stu-id="8292f-259">CSRF attacks rely on the default browser behavior of sending cookies associated with a domain with every request made to that domain.</span></span> <span data-ttu-id="8292f-260">Te pliki cookie są przechowywane w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="8292f-260">These cookies are stored within the browser.</span></span> <span data-ttu-id="8292f-261">Często zawierają one plików cookie sesji dla uwierzytelnionych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="8292f-261">They frequently include session cookies for authenticated users.</span></span> <span data-ttu-id="8292f-262">Plik cookie uwierzytelniania jest popularnych formy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="8292f-262">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="8292f-263">Systemami uwierzytelniania opartego na tokenie ma zostały rośnie w popularne, szczególnie w przypadku źródła i innych scenariuszy "inteligentne klienta".</span><span class="sxs-lookup"><span data-stu-id="8292f-263">Token-based authentication systems have been growing in popularity, especially for SPAs and other "smart client" scenarios.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="8292f-264">Uwierzytelnianie na podstawie plików cookie</span><span class="sxs-lookup"><span data-stu-id="8292f-264">Cookie-based authentication</span></span>

<span data-ttu-id="8292f-265">Gdy użytkownik został uwierzytelniony przy użyciu nazwy użytkownika i hasła, ich jest wystawiony token, który może służyć do ich identyfikacji i weryfikacji, czy zostały uwierzytelnione.</span><span class="sxs-lookup"><span data-stu-id="8292f-265">Once a user has authenticated using their username and password, they're issued a token that can be used to identify them and validate that they have been authenticated.</span></span> <span data-ttu-id="8292f-266">Token jest przechowywany jako sprawia, że plik cookie dołączona każde żądanie klienta.</span><span class="sxs-lookup"><span data-stu-id="8292f-266">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="8292f-267">Generowanie i sprawdzanie poprawności ten plik cookie odbywa się przez oprogramowanie pośredniczące uwierzytelniania plików cookie.</span><span class="sxs-lookup"><span data-stu-id="8292f-267">Generating and validating this cookie is done by the cookie authentication middleware.</span></span> <span data-ttu-id="8292f-268">Platformy ASP.NET Core zawiera plik cookie [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) co serializuje głównej nazwy użytkownika do zaszyfrowanego pliku cookie i kolejne żądania sprawdza poprawność pliku cookie, odtwarza podmiot zabezpieczeń i przypisuje go do `User` właściwości `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="8292f-268">ASP.NET Core provides cookie [middleware](xref:fundamentals/middleware/index) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal and assigns it to the `User` property on `HttpContext`.</span></span>

<span data-ttu-id="8292f-269">Gdy używany jest plik cookie, plik cookie uwierzytelniania jest tylko kontenerem dla biletu uwierzytelniania formularzy.</span><span class="sxs-lookup"><span data-stu-id="8292f-269">When a cookie is used, The authentication cookie is just a container for the forms authentication ticket.</span></span> <span data-ttu-id="8292f-270">Bilet jest przekazywany jako wartość pliku cookie uwierzytelniania formularzy z każdym żądaniem i jest używany przez uwierzytelnianie formularzy, na serwerze, aby zidentyfikować uwierzytelnionego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8292f-270">The ticket is passed as the value of the forms authentication cookie with each request and is used by forms authentication, on the server, to identify an authenticated user.</span></span>

<span data-ttu-id="8292f-271">Gdy użytkownik jest zalogowany do systemu, sesja użytkownika jest tworzony po stronie serwera i są przechowywane w bazie danych lub innych magazynu trwałego.</span><span class="sxs-lookup"><span data-stu-id="8292f-271">When a user is logged in to a system, a user session is created on the server-side and is stored in a database or some other persistent store.</span></span> <span data-ttu-id="8292f-272">System generuje klucz sesji wskazujące rzeczywiste sesji w magazynie danych i są wysyłane jako plik cookie po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="8292f-272">The system generates a session key that points to the actual session in the data store and it's sent as a client side cookie.</span></span> <span data-ttu-id="8292f-273">Serwer sieci web będzie sprawdzać ten klucz sesji żadnych żądaniem użytkownika z zasobem, który wymaga autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="8292f-273">The web server will check this session key any time a user requests a resource that requires authorization.</span></span> <span data-ttu-id="8292f-274">System sprawdza, czy sesja skojarzonego użytkownika ma uprawnienia dostępu do żądanego zasobu.</span><span class="sxs-lookup"><span data-stu-id="8292f-274">The system checks whether the associated user session has the privilege to access the requested resource.</span></span> <span data-ttu-id="8292f-275">Jeśli tak, nadal żądania.</span><span class="sxs-lookup"><span data-stu-id="8292f-275">If so, the request continues.</span></span> <span data-ttu-id="8292f-276">W przeciwnym razie żądania zwraca w postaci nie autoryzowanych.</span><span class="sxs-lookup"><span data-stu-id="8292f-276">Otherwise, the request returns as not authorized.</span></span> <span data-ttu-id="8292f-277">W takie podejście pliki cookie służą do tworzenia aplikacji jest stanowa, ponieważ jest w stanie "Pamiętaj" który użytkownik wcześniej uwierzytelnił się z serwerem.</span><span class="sxs-lookup"><span data-stu-id="8292f-277">In this approach, cookies are used to make the application appear to be stateful, since it's able to "remember" that the user has previously authenticated with the server.</span></span>

### <a name="user-tokens"></a><span data-ttu-id="8292f-278">Tokeny użytkownika</span><span class="sxs-lookup"><span data-stu-id="8292f-278">User tokens</span></span>

<span data-ttu-id="8292f-279">Uwierzytelnianie na podstawie tokenu nie przechowuje sesji na serwerze.</span><span class="sxs-lookup"><span data-stu-id="8292f-279">Token-based authentication doesn't store session on the server.</span></span> <span data-ttu-id="8292f-280">Gdy użytkownik jest zalogowany, są one wystawiony token (nie antiforgery token).</span><span class="sxs-lookup"><span data-stu-id="8292f-280">When a user is logged in, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="8292f-281">Token ten zawiera dane, które są wymagane do weryfikacji tokenu.</span><span class="sxs-lookup"><span data-stu-id="8292f-281">This token holds the data that's required to validate the token.</span></span> <span data-ttu-id="8292f-282">Zawiera także informacje o użytkowniku w formie [oświadczeń](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span><span class="sxs-lookup"><span data-stu-id="8292f-282">It also contains user information in the form of [claims](https://docs.microsoft.com/dotnet/framework/security/claims-based-identity-model).</span></span> <span data-ttu-id="8292f-283">Gdy użytkownik chce, aby uzyskać dostęp do zasobów serwera wymaga uwierzytelnienia, token jest wysyłany na serwer z nagłówkiem dodatkowe autoryzacji w formie {tokenu} elementu nośnego.</span><span class="sxs-lookup"><span data-stu-id="8292f-283">When a user wants to access a server resource requiring authentication, the token is sent to the server with an additional authorization header in form of Bearer {token}.</span></span> <span data-ttu-id="8292f-284">Dzięki temu aplikacji bezstanowych, ponieważ w kolejnych żądań token jest przekazywany w żądania do weryfikacji po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="8292f-284">This makes the application stateless since in each subsequent request the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="8292f-285">Ten token nie jest *zaszyfrowanych*; jest raczej *zakodowane*.</span><span class="sxs-lookup"><span data-stu-id="8292f-285">This token isn't *encrypted*; rather it's *encoded*.</span></span> <span data-ttu-id="8292f-286">Po stronie serwera token może zostać odczytany na dostęp do nieprzetworzonej informacji w tokenie.</span><span class="sxs-lookup"><span data-stu-id="8292f-286">On the server-side, the token can be decoded to access the raw information within the token.</span></span> <span data-ttu-id="8292f-287">Aby wysłać token w kolejnych żądań, albo zapisz go w magazynie lokalnym w przeglądarce lub w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8292f-287">To send the token in subsequent requests, either store it in the browser's local storage or in a cookie.</span></span> <span data-ttu-id="8292f-288">Nie martw się o XSRF luki w zabezpieczeniach, jeśli token jest przechowywany w magazynie lokalnym, ale jest to problem, jeśli token jest przechowywany w pliku cookie.</span><span class="sxs-lookup"><span data-stu-id="8292f-288">Don't worry about XSRF vulnerability if the token is stored in the local storage, but it's an issue if the token is stored in a cookie.</span></span>

### <a name="multiple-applications-are-hosted-in-one-domain"></a><span data-ttu-id="8292f-289">Wiele aplikacji znajdują się w jednej domenie</span><span class="sxs-lookup"><span data-stu-id="8292f-289">Multiple applications are hosted in one domain</span></span>

<span data-ttu-id="8292f-290">Mimo że `example1.cloudapp.net` i `example2.cloudapp.net` są różnych hostach, ma zależności nawiązywanie niejawnych relacji zaufania między hostami w obszarze `*.cloudapp.net` domeny.</span><span class="sxs-lookup"><span data-stu-id="8292f-290">Although `example1.cloudapp.net` and `example2.cloudapp.net` are different hosts, there's an implicit trust relationship between hosts under the `*.cloudapp.net` domain.</span></span> <span data-ttu-id="8292f-291">Ta relacja zaufania niejawne umożliwia potencjalnie niezaufane hosty wpływa na siebie nawzajem pliki cookie (zasad tego samego źródła, które będą zarządzały sposobem żądania AJAX nie zawsze dotyczą plików cookie protokołu HTTP).</span><span class="sxs-lookup"><span data-stu-id="8292f-291">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span> <span data-ttu-id="8292f-292">Środowisko uruchomieniowe platformy ASP.NET Core zawiera pewne środki zaradcze, nazwa użytkownika jest osadzony w tokenie pola.</span><span class="sxs-lookup"><span data-stu-id="8292f-292">The ASP.NET Core runtime provides some mitigation in that the username is embedded into the field token.</span></span> <span data-ttu-id="8292f-293">Nawet jeśli poddomeny złośliwego może zastąpić tokenu sesji, nie może generować prawidłowe pole token dla użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8292f-293">Even if a malicious subdomain is able to overwrite a session token, it can't generate a valid field token for the user.</span></span> <span data-ttu-id="8292f-294">W przypadku hostowania w takim środowisku, procedury wbudowanych anti-XSRF nadal nie obrony przed przejęcie kontroli sesji lub logowania CSRF ataków.</span><span class="sxs-lookup"><span data-stu-id="8292f-294">When hosted in such an environment, the built-in anti-XSRF routines still can't defend against session hijacking or login CSRF attacks.</span></span> <span data-ttu-id="8292f-295">Udostępnionych środowiskach macierzystych są vunerable przejęcie kontroli sesji logowania CSRF i inne ataki.</span><span class="sxs-lookup"><span data-stu-id="8292f-295">Shared hosting environments are vunerable to session hijacking, login CSRF, and other attacks.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="8292f-296">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8292f-296">Additional resources</span></span>

* <span data-ttu-id="8292f-297">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) na [Otwórz projekt zabezpieczeń aplikacji sieci Web](https://www.owasp.org/index.php/Main_Page) (OWASP).</span><span class="sxs-lookup"><span data-stu-id="8292f-297">[XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
