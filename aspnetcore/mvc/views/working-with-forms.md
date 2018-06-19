---
title: Pomocników tagów w formularzy w programie ASP.NET Core
author: rick-anderson
description: W tym artykule opisano wbudowane pomocników tagów używane w formularzach.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/working-with-forms
ms.openlocfilehash: 9155bd54bc211c8be0678065e857f73d8a139365
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/03/2018
ms.locfileid: "32741131"
---
# <a name="tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="9ef17-103">Pomocników tagów w formularzy w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9ef17-103">Tag Helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="9ef17-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), i [Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="9ef17-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="9ef17-105">W tym dokumencie przedstawiono pracy z formularzami i elementy HTML często używane w formularzu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-105">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="9ef17-106">Kod HTML [formularza](https://www.w3.org/TR/html401/interact/forms.html) element udostępnia użycia aplikacji sieci web podstawowego mechanizmu publikowania dane do serwera.</span><span class="sxs-lookup"><span data-stu-id="9ef17-106">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="9ef17-107">Większość ten dokument zawiera opis [pomocników tagów](tag-helpers/intro.md) i jak ich można efektywnie tworzyć niezawodne formularzy HTML.</span><span class="sxs-lookup"><span data-stu-id="9ef17-107">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="9ef17-108">Zalecamy przeczytanie [wprowadzenie do pomocników tagów](tag-helpers/intro.md) przed przeczytaniem tego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-108">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="9ef17-109">W wielu przypadkach pomocników HTML Podaj informacje o innym podejściu do określonych pomocniczego znacznika, ale ważne jest, aby rozpoznać pomocników tagów nie Zastąp pomocników HTML, a nie ma tagu pomocnika dla każdego pomocnika kodu HTML.</span><span class="sxs-lookup"><span data-stu-id="9ef17-109">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="9ef17-110">Jeśli istnieje alternatywna pomocnika kodu HTML, to wymienione.</span><span class="sxs-lookup"><span data-stu-id="9ef17-110">When an HTML Helper alternative exists, it's mentioned.</span></span>

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="9ef17-111">Pomocnik Tag formularza</span><span class="sxs-lookup"><span data-stu-id="9ef17-111">The Form Tag Helper</span></span>

<span data-ttu-id="9ef17-112">[Formularza](https://www.w3.org/TR/html401/interact/forms.html) pomocnika tagów:</span><span class="sxs-lookup"><span data-stu-id="9ef17-112">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="9ef17-113">Generuje kod HTML [ \<formularza >](https://www.w3.org/TR/html401/interact/forms.html) `action` wartości atrybutu akcji kontrolera MVC lub nazwanej trasy</span><span class="sxs-lookup"><span data-stu-id="9ef17-113">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="9ef17-114">Generuje ukryty [żądania weryfikacji tokenu](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) celu zapobiegania fałszowaniu żądania między witrynami (w przypadku użycia z `[ValidateAntiForgeryToken]` atrybutu w metodzie akcji HTTP Post)</span><span class="sxs-lookup"><span data-stu-id="9ef17-114">Generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="9ef17-115">Udostępnia `asp-route-<Parameter Name>` atrybutu, gdzie `<Parameter Name>` jest dodawany do wartości trasy.</span><span class="sxs-lookup"><span data-stu-id="9ef17-115">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="9ef17-116">`routeValues` Parametry `Html.BeginForm` i `Html.BeginRouteForm` zapewniają podobne funkcje.</span><span class="sxs-lookup"><span data-stu-id="9ef17-116">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="9ef17-117">Jest to alternatywa pomocnika kodu HTML `Html.BeginForm` i `Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="9ef17-117">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="9ef17-118">Przykład:</span><span class="sxs-lookup"><span data-stu-id="9ef17-118">Sample:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="9ef17-119">Pomocnik Tag formularza powyżej generuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="9ef17-119">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
    <!-- Input and Submit elements -->
    <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

<span data-ttu-id="9ef17-120">Środowisko uruchomieniowe MVC generuje `action` wartość atrybutu z atrybutów pomocnika Tag formularza `asp-controller` i `asp-action`.</span><span class="sxs-lookup"><span data-stu-id="9ef17-120">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="9ef17-121">Pomocnik Tag formularza również generuje ukryty [żądania weryfikacji tokenu](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) celu zapobiegania fałszowaniu żądania między witrynami (w przypadku użycia z `[ValidateAntiForgeryToken]` atrybutu w metodzie akcji Post protokołu HTTP).</span><span class="sxs-lookup"><span data-stu-id="9ef17-121">The Form Tag Helper also generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="9ef17-122">Ochrona czysty formularza HTML przed sfałszowaniem żądań cross-site jest trudne, Pomocnik Tag formularza zapewnia tej usługi można.</span><span class="sxs-lookup"><span data-stu-id="9ef17-122">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="9ef17-123">Za pomocą nazwanej trasy</span><span class="sxs-lookup"><span data-stu-id="9ef17-123">Using a named route</span></span>

<span data-ttu-id="9ef17-124">`asp-route` Atrybut pomocnika tagów można również generować kod znaczników dla kodu HTML `action` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-124">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="9ef17-125">Aplikację z [trasy](../../fundamentals/routing.md) o nazwie `register` można użyć następujących znaczników dla strony rejestracji:</span><span class="sxs-lookup"><span data-stu-id="9ef17-125">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="9ef17-126">Wiele widoków w *widoków/konta* folder (generowane podczas tworzenia nowej aplikacji sieci web z *indywidualnych kont użytkowników*) zawierać [asp trasy returnurl](xref:mvc/views/working-with-forms) atrybutu:</span><span class="sxs-lookup"><span data-stu-id="9ef17-126">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](xref:mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="9ef17-127">Wbudowane szablony `returnUrl` tylko jest wypełniane automatycznie, gdy próbują uzyskać dostęp do autoryzowanych zasobów, ale nie ma uwierzytelniony ani autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="9ef17-127">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="9ef17-128">Podczas próby nieautoryzowanego dostępu, oprogramowanie pośredniczące zabezpieczeń przekieruje Cię do strony logowania z `returnUrl` ustawiony.</span><span class="sxs-lookup"><span data-stu-id="9ef17-128">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="9ef17-129">Pomocnik tagu wejściowego</span><span class="sxs-lookup"><span data-stu-id="9ef17-129">The Input Tag Helper</span></span>

<span data-ttu-id="9ef17-130">Wejściowy pomocnika tagów wiąże HTML [ \<wejściowych >](https://www.w3.org/wiki/HTML/Elements/input) element na wyrażenie modelu w widoku razor.</span><span class="sxs-lookup"><span data-stu-id="9ef17-130">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="9ef17-131">Składnia:</span><span class="sxs-lookup"><span data-stu-id="9ef17-131">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="9ef17-132">Pomocnik tagu wejściowego:</span><span class="sxs-lookup"><span data-stu-id="9ef17-132">The Input Tag Helper:</span></span>

* <span data-ttu-id="9ef17-133">Generuje `id` i `name` atrybutów HTML dla podanej nazwy wyrażenia w `asp-for` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-133">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="9ef17-134">`asp-for="Property1.Property2"` jest odpowiednikiem `m => m.Property1.Property2`.</span><span class="sxs-lookup"><span data-stu-id="9ef17-134">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="9ef17-135">Nazwa wyrażenia jest, do czego służy `asp-for` wartość atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-135">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="9ef17-136">Zobacz [nazwy wyrażeń](#expression-names) sekcji, aby uzyskać dodatkowe informacje.</span><span class="sxs-lookup"><span data-stu-id="9ef17-136">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="9ef17-137">Ustawia kod HTML `type` atrybutu wartości na podstawie typu modelu i [adnotacji danych elementu](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atrybuty zastosowane do właściwości modelu</span><span class="sxs-lookup"><span data-stu-id="9ef17-137">Sets the HTML `type` attribute value based on the model type and  [data annotation](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="9ef17-138">Nie zastępować HTML `type` wartość atrybutu, gdy został określony</span><span class="sxs-lookup"><span data-stu-id="9ef17-138">Won't overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="9ef17-139">Generuje [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) sprawdzania poprawności atrybutów z [adnotacji danych elementu](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atrybuty stosowane do właściwości modelu</span><span class="sxs-lookup"><span data-stu-id="9ef17-139">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="9ef17-140">Funkcję pomocnika kodu HTML, które nakładają się na `Html.TextBoxFor` i `Html.EditorFor`.</span><span class="sxs-lookup"><span data-stu-id="9ef17-140">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="9ef17-141">Zobacz **alternatyw pomocnika kodu HTML do pomocniczego Tag danych wejściowych** sekcji, aby uzyskać szczegółowe informacje.</span><span class="sxs-lookup"><span data-stu-id="9ef17-141">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="9ef17-142">Umożliwia wpisanie silne.</span><span class="sxs-lookup"><span data-stu-id="9ef17-142">Provides strong typing.</span></span> <span data-ttu-id="9ef17-143">Jeśli nazwa zmiany właściwości, a nie Aktualizuj pomocnika tagów pojawi błąd podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="9ef17-143">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="9ef17-144">`Input` Pomocnika tagów ustawia HTML `type` atrybut oparty na typie .NET.</span><span class="sxs-lookup"><span data-stu-id="9ef17-144">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="9ef17-145">W poniższej tabeli wymieniono niektóre typowe typów .NET i wygenerowanego typu HTML (nie każdy typ architektury .NET to wymienione).</span><span class="sxs-lookup"><span data-stu-id="9ef17-145">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="9ef17-146">Typ architektury .NET</span><span class="sxs-lookup"><span data-stu-id="9ef17-146">.NET type</span></span>|<span data-ttu-id="9ef17-147">Typ danych wejściowych</span><span class="sxs-lookup"><span data-stu-id="9ef17-147">Input Type</span></span>|
|---|---|
|<span data-ttu-id="9ef17-148">wartość logiczna</span><span class="sxs-lookup"><span data-stu-id="9ef17-148">Bool</span></span>|<span data-ttu-id="9ef17-149">type=”checkbox”</span><span class="sxs-lookup"><span data-stu-id="9ef17-149">type=”checkbox”</span></span>|
|<span data-ttu-id="9ef17-150">String</span><span class="sxs-lookup"><span data-stu-id="9ef17-150">String</span></span>|<span data-ttu-id="9ef17-151">Typ = "text"</span><span class="sxs-lookup"><span data-stu-id="9ef17-151">type=”text”</span></span>|
|<span data-ttu-id="9ef17-152">DataGodzina</span><span class="sxs-lookup"><span data-stu-id="9ef17-152">DateTime</span></span>|<span data-ttu-id="9ef17-153">Typ "datetime" =</span><span class="sxs-lookup"><span data-stu-id="9ef17-153">type=”datetime”</span></span>|
|<span data-ttu-id="9ef17-154">Byte</span><span class="sxs-lookup"><span data-stu-id="9ef17-154">Byte</span></span>|<span data-ttu-id="9ef17-155">Typ = "number"</span><span class="sxs-lookup"><span data-stu-id="9ef17-155">type=”number”</span></span>|
|<span data-ttu-id="9ef17-156">int</span><span class="sxs-lookup"><span data-stu-id="9ef17-156">Int</span></span>|<span data-ttu-id="9ef17-157">Typ = "number"</span><span class="sxs-lookup"><span data-stu-id="9ef17-157">type=”number”</span></span>|
|<span data-ttu-id="9ef17-158">Single, Double</span><span class="sxs-lookup"><span data-stu-id="9ef17-158">Single, Double</span></span>|<span data-ttu-id="9ef17-159">Typ = "number"</span><span class="sxs-lookup"><span data-stu-id="9ef17-159">type=”number”</span></span>|


<span data-ttu-id="9ef17-160">W poniższej tabeli przedstawiono niektóre typowe [adnotacji danych](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) atrybuty, które pomocnika tagu wejściowego przypisze do określonych typów wejściowych (nie każdy atrybut weryfikacji znajduje się):</span><span class="sxs-lookup"><span data-stu-id="9ef17-160">The following table shows some common [data annotations](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="9ef17-161">Atrybut</span><span class="sxs-lookup"><span data-stu-id="9ef17-161">Attribute</span></span>|<span data-ttu-id="9ef17-162">Typ danych wejściowych</span><span class="sxs-lookup"><span data-stu-id="9ef17-162">Input Type</span></span>|
|---|---|
|<span data-ttu-id="9ef17-163">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="9ef17-163">[EmailAddress]</span></span>|<span data-ttu-id="9ef17-164">type=”email”</span><span class="sxs-lookup"><span data-stu-id="9ef17-164">type=”email”</span></span>|
|<span data-ttu-id="9ef17-165">[Url]</span><span class="sxs-lookup"><span data-stu-id="9ef17-165">[Url]</span></span>|<span data-ttu-id="9ef17-166">type=”url”</span><span class="sxs-lookup"><span data-stu-id="9ef17-166">type=”url”</span></span>|
|<span data-ttu-id="9ef17-167">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="9ef17-167">[HiddenInput]</span></span>|<span data-ttu-id="9ef17-168">type=”hidden”</span><span class="sxs-lookup"><span data-stu-id="9ef17-168">type=”hidden”</span></span>|
|<span data-ttu-id="9ef17-169">[Phone]</span><span class="sxs-lookup"><span data-stu-id="9ef17-169">[Phone]</span></span>|<span data-ttu-id="9ef17-170">type=”tel”</span><span class="sxs-lookup"><span data-stu-id="9ef17-170">type=”tel”</span></span>|
|<span data-ttu-id="9ef17-171">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="9ef17-171">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="9ef17-172">Typ = 'password'</span><span class="sxs-lookup"><span data-stu-id="9ef17-172">type=”password”</span></span>|
|<span data-ttu-id="9ef17-173">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="9ef17-173">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="9ef17-174">Typ "date" =</span><span class="sxs-lookup"><span data-stu-id="9ef17-174">type=”date”</span></span>|
|<span data-ttu-id="9ef17-175">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="9ef17-175">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="9ef17-176">Typ = "razem"</span><span class="sxs-lookup"><span data-stu-id="9ef17-176">type=”time”</span></span>|


<span data-ttu-id="9ef17-177">Przykład:</span><span class="sxs-lookup"><span data-stu-id="9ef17-177">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="9ef17-178">Powyższy kod generuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="9ef17-178">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid email address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

<span data-ttu-id="9ef17-179">Adnotacje danych dotyczy `Email` i `Password` właściwości Generowanie metadanych dla modelu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-179">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="9ef17-180">Pomocnik Tag danych wejściowych zużywa metadanych modelu i tworzy [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` atrybutów (zobacz [sprawdzania poprawności modelu](../models/validation.md)).</span><span class="sxs-lookup"><span data-stu-id="9ef17-180">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="9ef17-181">Te atrybuty opisują moduły weryfikacji, aby dołączyć do pól wejściowych.</span><span class="sxs-lookup"><span data-stu-id="9ef17-181">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="9ef17-182">Zapewnia to dyskretnego kodu HTML5 i [jQuery](https://jquery.com/) sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="9ef17-182">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="9ef17-183">Atrybuty dyskretnego kodu mają format `data-val-rule="Error Message"`, gdzie reguła jest nazwa reguły sprawdzania poprawności (takich jak `data-val-required`, `data-val-email`, `data-val-maxlength`itp.) Jeśli komunikat o błędzie jest podana w atrybucie, jest wyświetlany jako wartość `data-val-rule` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-183">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it's displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="9ef17-184">Istnieją również atrybuty formularza `data-val-ruleName-argumentName="argumentValue"` dodatkowe szczegóły dotyczące reguły, które zapewniają na przykład `data-val-maxlength-max="1024"` .</span><span class="sxs-lookup"><span data-stu-id="9ef17-184">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="9ef17-185">Alternatywy pomocnika kodu HTML dla danych wejściowych pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="9ef17-185">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="9ef17-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` i `Html.EditorFor` mają pokrywające się funkcji przy użyciu Pomocnika Tag danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="9ef17-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="9ef17-187">Automatycznie ustawi pomocnika Tag danych wejściowych `type` atrybutu; `Html.TextBox` i `Html.TextBoxFor` nie.</span><span class="sxs-lookup"><span data-stu-id="9ef17-187">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` won't.</span></span> <span data-ttu-id="9ef17-188">`Html.Editor` i `Html.EditorFor` obsługiwać kolekcje obiektów złożonych i szablonów; nie pomocnika Tag danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="9ef17-188">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper doesn't.</span></span> <span data-ttu-id="9ef17-189">Pomocnik Tag danych wejściowych, `Html.EditorFor` i `Html.TextBoxFor` są silnie typizowane (one użycie wyrażeń lambda); `Html.TextBox` i `Html.Editor` nie są (używają nazwy wyrażeń).</span><span class="sxs-lookup"><span data-stu-id="9ef17-189">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="9ef17-190">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="9ef17-190">HtmlAttributes</span></span>

<span data-ttu-id="9ef17-191">`@Html.Editor()` i `@Html.EditorFor()` użycia specjalnego `ViewDataDictionary` wpis o nazwie `htmlAttributes` podczas wykonywania ich domyślnych szablonów.</span><span class="sxs-lookup"><span data-stu-id="9ef17-191">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="9ef17-192">To zachowanie jest opcjonalnie rozszerzone przy użyciu `additionalViewData` parametrów.</span><span class="sxs-lookup"><span data-stu-id="9ef17-192">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="9ef17-193">Klucz "htmlAttributes" jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="9ef17-193">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="9ef17-194">Klucz "htmlAttributes" odbywa się podobnie do `htmlAttributes` obiekt przekazany do danych wejściowych pomocników, takich jak `@Html.TextBox()`.</span><span class="sxs-lookup"><span data-stu-id="9ef17-194">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="9ef17-195">Nazwy wyrażeń</span><span class="sxs-lookup"><span data-stu-id="9ef17-195">Expression names</span></span>

<span data-ttu-id="9ef17-196">`asp-for` Wartość atrybutu jest `ModelExpression` i po prawej stronie wyrażenia lambda.</span><span class="sxs-lookup"><span data-stu-id="9ef17-196">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="9ef17-197">W związku z tym `asp-for="Property1"` staje się `m => m.Property1` w wygenerowanym kodzie, który, dlatego nie trzeba prefiks z `Model`.</span><span class="sxs-lookup"><span data-stu-id="9ef17-197">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="9ef17-198">Można użyć "@" znak do uruchamiania wyrażenia wbudowanego i przenieść przed `m.`:</span><span class="sxs-lookup"><span data-stu-id="9ef17-198">You can use the "@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="9ef17-199">Generuje następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="9ef17-199">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="9ef17-200">Z właściwościami kolekcji `asp-for="CollectionProperty[23].Member"` generuje taką samą nazwę jak `asp-for="CollectionProperty[i].Member"` podczas `i` ma wartość `23`.</span><span class="sxs-lookup"><span data-stu-id="9ef17-200">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

<span data-ttu-id="9ef17-201">Gdy platformy ASP.NET Core MVC oblicza wartość `ModelExpression`, sprawdza on kilka źródeł, takich jak `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="9ef17-201">When ASP.NET Core MVC calculates the value of `ModelExpression`, it inspects several sources, including `ModelState`.</span></span> <span data-ttu-id="9ef17-202">Należy wziąć pod uwagę `<input type="text" asp-for="@Name" />`.</span><span class="sxs-lookup"><span data-stu-id="9ef17-202">Consider `<input type="text" asp-for="@Name" />`.</span></span> <span data-ttu-id="9ef17-203">Obliczony `value` atrybut jest pierwsza wartość inną niż null z:</span><span class="sxs-lookup"><span data-stu-id="9ef17-203">The calculated `value` attribute is the first non-null value from:</span></span>

* <span data-ttu-id="9ef17-204">`ModelState` wpis z kluczem "Name".</span><span class="sxs-lookup"><span data-stu-id="9ef17-204">`ModelState` entry with key "Name".</span></span>
* <span data-ttu-id="9ef17-205">Wynik wyrażenia `Model.Name`.</span><span class="sxs-lookup"><span data-stu-id="9ef17-205">Result of the expression `Model.Name`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="9ef17-206">Nawigowanie po właściwości podrzędnej</span><span class="sxs-lookup"><span data-stu-id="9ef17-206">Navigating child properties</span></span>

<span data-ttu-id="9ef17-207">Można także przejść do właściwości podrzędnej przy użyciu ścieżki właściwości modelu widoku.</span><span class="sxs-lookup"><span data-stu-id="9ef17-207">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="9ef17-208">Należy wziąć pod uwagę bardziej złożonych klasy modelu, który zawiera element podrzędny `Address` właściwości.</span><span class="sxs-lookup"><span data-stu-id="9ef17-208">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="9ef17-209">W widoku, możemy powiązać `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="9ef17-209">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="9ef17-210">Poniższy kod HTML jest generowane dla `Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="9ef17-210">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="9ef17-211">Wyrażenie nazwy i kolekcji</span><span class="sxs-lookup"><span data-stu-id="9ef17-211">Expression names and Collections</span></span>

<span data-ttu-id="9ef17-212">Przykładowy model zawierający tablicę `Colors`:</span><span class="sxs-lookup"><span data-stu-id="9ef17-212">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="9ef17-213">Metody akcji:</span><span class="sxs-lookup"><span data-stu-id="9ef17-213">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="9ef17-214">Następujące Razor pokazuje, jak uzyskać dostępu do określonego `Color` elementu:</span><span class="sxs-lookup"><span data-stu-id="9ef17-214">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="9ef17-215">*Views/Shared/EditorTemplates/String.cshtml* szablonu:</span><span class="sxs-lookup"><span data-stu-id="9ef17-215">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="9ef17-216">Przykładowe przy użyciu `List<T>`:</span><span class="sxs-lookup"><span data-stu-id="9ef17-216">Sample using `List<T>`:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="9ef17-217">Następujące Razor pokazano, jak Iterowanie przez kolekcję:</span><span class="sxs-lookup"><span data-stu-id="9ef17-217">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="9ef17-218">*Views/Shared/EditorTemplates/ToDoItem.cshtml* szablonu:</span><span class="sxs-lookup"><span data-stu-id="9ef17-218">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
><span data-ttu-id="9ef17-219">Zawsze używaj `for` (i *nie* `foreach`) do wykonywania iteracji listy.</span><span class="sxs-lookup"><span data-stu-id="9ef17-219">Always use `for` (and *not* `foreach`) to iterate over a list.</span></span> <span data-ttu-id="9ef17-220">Ocena indeksatora w składniku LINQ wyrażenie może być kosztowne i powinien być minimalny.</span><span class="sxs-lookup"><span data-stu-id="9ef17-220">Evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="9ef17-221">Powyższym kodzie przykładowym komentarze pokazuje, jak należy zastąpić wyrażenia lambda za pomocą `@` operatora, aby uzyskać dostęp każdy `ToDoItem` na liście.</span><span class="sxs-lookup"><span data-stu-id="9ef17-221">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="9ef17-222">Pomocnik Textarea Tag</span><span class="sxs-lookup"><span data-stu-id="9ef17-222">The Textarea Tag Helper</span></span>

<span data-ttu-id="9ef17-223">`Textarea Tag Helper` Pomocnika tagów jest podobny do pomocniczego Tag danych wejściowych.</span><span class="sxs-lookup"><span data-stu-id="9ef17-223">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="9ef17-224">Generuje `id` i `name` atrybuty oraz atrybuty weryfikacji danych z modelu dla [ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea) elementu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-224">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="9ef17-225">Umożliwia wpisanie silne.</span><span class="sxs-lookup"><span data-stu-id="9ef17-225">Provides strong typing.</span></span>

* <span data-ttu-id="9ef17-226">Alternatywa pomocnika kodu HTML: `Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="9ef17-226">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="9ef17-227">Przykład:</span><span class="sxs-lookup"><span data-stu-id="9ef17-227">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="9ef17-228">Poniższy kod HTML jest generowany:</span><span class="sxs-lookup"><span data-stu-id="9ef17-228">The following HTML is generated:</span></span>

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a><span data-ttu-id="9ef17-229">Etykieta pomocnika tagów</span><span class="sxs-lookup"><span data-stu-id="9ef17-229">The Label Tag Helper</span></span>

* <span data-ttu-id="9ef17-230">Generuje podpis etykiety i `for` atrybutu [ <label> ](https://www.w3.org/wiki/HTML/Elements/label) elementu nazwa wyrażenia</span><span class="sxs-lookup"><span data-stu-id="9ef17-230">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="9ef17-231">Alternatywne pomocnika kodu HTML: `Html.LabelFor`.</span><span class="sxs-lookup"><span data-stu-id="9ef17-231">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="9ef17-232">`Label Tag Helper` Zapewnia następujące korzyści przez pure element label kodu HTML:</span><span class="sxs-lookup"><span data-stu-id="9ef17-232">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="9ef17-233">Automatycznie pobrana wartość opisową etykietę `Display` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-233">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="9ef17-234">Nazwa wyświetlana danego może zmieniać wraz z upływem czasu i kombinacja `Display` będą miały zastosowanie atrybutu i pomocnika tagów etykiety `Display` everywhere jest używany.</span><span class="sxs-lookup"><span data-stu-id="9ef17-234">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="9ef17-235">Mniej znacznika w kodzie źródłowym</span><span class="sxs-lookup"><span data-stu-id="9ef17-235">Less markup in source code</span></span>

* <span data-ttu-id="9ef17-236">Silne wpisywanie razem z właściwością modelu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-236">Strong typing with the model property.</span></span>

<span data-ttu-id="9ef17-237">Przykład:</span><span class="sxs-lookup"><span data-stu-id="9ef17-237">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="9ef17-238">Poniższy kod HTML jest generowane dla `<label>` elementu:</span><span class="sxs-lookup"><span data-stu-id="9ef17-238">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="9ef17-239">Pomocnika tagów etykiety wygenerowany `for` wartość atrybutu "E-mail", który jest identyfikator skojarzony z `<input>` elementu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-239">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="9ef17-240">Generowanie spójne pomocników tagów `id` i `for` elementy, tak aby były prawidłowo skojarzony.</span><span class="sxs-lookup"><span data-stu-id="9ef17-240">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="9ef17-241">Podpis w tym przykładzie pochodzi z `Display` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-241">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="9ef17-242">Jeśli model nie zawiera `Display` atrybut podpisu może być nazwą właściwości wyrażenia.</span><span class="sxs-lookup"><span data-stu-id="9ef17-242">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="9ef17-243">Sprawdzanie poprawności pomocników tagów</span><span class="sxs-lookup"><span data-stu-id="9ef17-243">The Validation Tag Helpers</span></span>

<span data-ttu-id="9ef17-244">Istnieją dwa pomocników tagów sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="9ef17-244">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="9ef17-245">`Validation Message Tag Helper` (Która wyświetla komunikat dotyczący sprawdzania poprawności dla jednej właściwości w modelu) i `Validation Summary Tag Helper` (która wyświetla podsumowanie błędy sprawdzania poprawności).</span><span class="sxs-lookup"><span data-stu-id="9ef17-245">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="9ef17-246">`Input Tag Helper` Dodaje atrybuty weryfikacji po stronie klienta HTML5 do wprowadzania elementów na podstawie danych atrybuty adnotacji w klasach modeli.</span><span class="sxs-lookup"><span data-stu-id="9ef17-246">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="9ef17-247">Sprawdzanie poprawności również jest wykonywane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="9ef17-247">Validation is also performed on the server.</span></span> <span data-ttu-id="9ef17-248">Pomocnika tagów weryfikacji wyświetla te komunikaty o błędach, gdy wystąpi błąd sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="9ef17-248">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="9ef17-249">Pomocnik Tag komunikatu weryfikacji</span><span class="sxs-lookup"><span data-stu-id="9ef17-249">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="9ef17-250">Dodaje [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"` atrybutu [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, który dołącza komunikatów o błędach weryfikacji na pole wejściowe właściwości określonego modelu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-250">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="9ef17-251">Gdy wystąpi błąd weryfikacji po stronie klienta, [jQuery](https://jquery.com/) wyświetla komunikat o błędzie w `<span>` elementu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-251">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="9ef17-252">Sprawdzanie poprawności również odbywa się na serwerze.</span><span class="sxs-lookup"><span data-stu-id="9ef17-252">Validation also takes place on the server.</span></span> <span data-ttu-id="9ef17-253">Klienci mogą mieć JavaScript wyłączona i niektóre sprawdzania poprawności jest możliwe tylko po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="9ef17-253">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="9ef17-254">Alternatywa pomocnika kodu HTML: `Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="9ef17-254">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="9ef17-255">`Validation Message Tag Helper` Jest używany z `asp-validation-for` atrybutu HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) elementu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-255">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="9ef17-256">Poniższy kod HTML generuje pomocnika tagów komunikatu sprawdzania poprawności:</span><span class="sxs-lookup"><span data-stu-id="9ef17-256">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="9ef17-257">Na ogół służy `Validation Message Tag Helper` po `Input` pomocnika tagów dla tej samej właściwości.</span><span class="sxs-lookup"><span data-stu-id="9ef17-257">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="9ef17-258">Dzięki temu wyświetla komunikaty o błędach weryfikacji w pobliżu danych wejściowych, który spowodował błąd.</span><span class="sxs-lookup"><span data-stu-id="9ef17-258">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="9ef17-259">Musi mieć widoku z poprawną JavaScript i [jQuery](https://jquery.com/) skryptu odwołań w celu weryfikacji po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="9ef17-259">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="9ef17-260">Zobacz [sprawdzania poprawności modelu](../models/validation.md) Aby uzyskać więcej informacji.</span><span class="sxs-lookup"><span data-stu-id="9ef17-260">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="9ef17-261">W przypadku wystąpienia błędu weryfikacji po stronie serwera (na przykład po masz weryfikacji po stronie serwera niestandardowych lub weryfikacji po stronie klienta jest wyłączony), MVC umieszcza ten komunikat o błędzie jako treść `<span>` elementu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-261">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="9ef17-262">Pomocnik weryfikacji Summary — Tag</span><span class="sxs-lookup"><span data-stu-id="9ef17-262">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="9ef17-263">Obiekty docelowe `<div>` elementy z `asp-validation-summary` atrybutu</span><span class="sxs-lookup"><span data-stu-id="9ef17-263">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="9ef17-264">Alternatywa pomocnika kodu HTML: `@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="9ef17-264">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="9ef17-265">`Validation Summary Tag Helper` Służy do wyświetlania podsumowania komunikatów dotyczących sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="9ef17-265">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="9ef17-266">`asp-validation-summary` Wartość atrybutu może być dowolną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="9ef17-266">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="9ef17-267">Podsumowanie ASP-sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="9ef17-267">asp-validation-summary</span></span>|<span data-ttu-id="9ef17-268">Wyświetlane komunikatów dotyczących sprawdzania poprawności</span><span class="sxs-lookup"><span data-stu-id="9ef17-268">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="9ef17-269">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="9ef17-269">ValidationSummary.All</span></span>|<span data-ttu-id="9ef17-270">Właściwości i modelu</span><span class="sxs-lookup"><span data-stu-id="9ef17-270">Property and model level</span></span>|
|<span data-ttu-id="9ef17-271">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="9ef17-271">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="9ef17-272">Model</span><span class="sxs-lookup"><span data-stu-id="9ef17-272">Model</span></span>|
|<span data-ttu-id="9ef17-273">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="9ef17-273">ValidationSummary.None</span></span>|<span data-ttu-id="9ef17-274">Brak</span><span class="sxs-lookup"><span data-stu-id="9ef17-274">None</span></span>|

### <a name="sample"></a><span data-ttu-id="9ef17-275">Przykład</span><span class="sxs-lookup"><span data-stu-id="9ef17-275">Sample</span></span>

<span data-ttu-id="9ef17-276">W poniższym przykładzie zostanie nadany modelu danych `DataAnnotation` atrybuty, które generuje komunikaty o błędach weryfikacji na `<input>` elementu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-276">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="9ef17-277">Gdy wystąpi błąd sprawdzania poprawności, weryfikacji pomocnika tagów wyświetla komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="9ef17-277">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="9ef17-278">Wygenerowany kod HTML, (Jeśli model jest nieprawidłowy):</span><span class="sxs-lookup"><span data-stu-id="9ef17-278">The generated HTML (when the model is valid):</span></span>

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid email address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a><span data-ttu-id="9ef17-279">Pomocnik tagu Select</span><span class="sxs-lookup"><span data-stu-id="9ef17-279">The Select Tag Helper</span></span>

* <span data-ttu-id="9ef17-280">Generuje [wybierz](https://www.w3.org/wiki/HTML/Elements/select) i skojarzone [opcji](https://www.w3.org/wiki/HTML/Elements/option) elementy dla właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="9ef17-280">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="9ef17-281">Jest to alternatywa pomocnika kodu HTML `Html.DropDownListFor` i `Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="9ef17-281">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="9ef17-282">`Select Tag Helper` `asp-for` Określa nazwę właściwości modelu [wybierz](https://www.w3.org/wiki/HTML/Elements/select) elementu i `asp-items` Określa [opcji](https://www.w3.org/wiki/HTML/Elements/option) elementów.</span><span class="sxs-lookup"><span data-stu-id="9ef17-282">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="9ef17-283">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="9ef17-283">For example:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="9ef17-284">Przykład:</span><span class="sxs-lookup"><span data-stu-id="9ef17-284">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="9ef17-285">`Index` Inicjuje metody `CountryViewModel`, ustawia wybranego kraju i przekazuje je do `Index` widoku.</span><span class="sxs-lookup"><span data-stu-id="9ef17-285">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

<span data-ttu-id="9ef17-286">HTTP POST `Index` metoda Wyświetla zaznaczenie:</span><span class="sxs-lookup"><span data-stu-id="9ef17-286">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="9ef17-287">`Index` Widoku:</span><span class="sxs-lookup"><span data-stu-id="9ef17-287">The `Index` view:</span></span>

[!code-cshtml[](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="9ef17-288">Generująca poniższy kod HTML (za pomocą "CA" wybrane):</span><span class="sxs-lookup"><span data-stu-id="9ef17-288">Which generates the following HTML (with "CA" selected):</span></span>

```html
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
```

> [!NOTE]
> <span data-ttu-id="9ef17-289">Nie zaleca się przy użyciu `ViewBag` lub `ViewData` z Pomocnika tagów wybierz.</span><span class="sxs-lookup"><span data-stu-id="9ef17-289">We don't recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="9ef17-290">Model widoku jest bardziej niezawodne zapewnienie metadanych MVC i zazwyczaj mniej powodować problemy.</span><span class="sxs-lookup"><span data-stu-id="9ef17-290">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="9ef17-291">`asp-for` Wartość atrybutu jest szczególnych przypadkach i nie wymaga `Model` prefiksu, inne są atrybuty pomocnika tagów (takie jak `asp-items`)</span><span class="sxs-lookup"><span data-stu-id="9ef17-291">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="9ef17-292">Powiązanie wyliczenia</span><span class="sxs-lookup"><span data-stu-id="9ef17-292">Enum binding</span></span>

<span data-ttu-id="9ef17-293">Często jest to łatwe w użyciu `<select>` z `enum` właściwości i generować `SelectListItem` elementy z `enum` wartości.</span><span class="sxs-lookup"><span data-stu-id="9ef17-293">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="9ef17-294">Przykład:</span><span class="sxs-lookup"><span data-stu-id="9ef17-294">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="9ef17-295">`GetEnumSelectList` Metoda generuje `SelectList` obiektu dla wyliczenia.</span><span class="sxs-lookup"><span data-stu-id="9ef17-295">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="9ef17-296">Można dekoracji lista modułu wyliczającego `Display` atrybutu, aby uzyskać bardziej zaawansowane funkcje interfejsu użytkownika:</span><span class="sxs-lookup"><span data-stu-id="9ef17-296">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="9ef17-297">Poniższy kod HTML jest generowany:</span><span class="sxs-lookup"><span data-stu-id="9ef17-297">The following HTML is generated:</span></span>

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

### <a name="option-group"></a><span data-ttu-id="9ef17-298">Opcja grupy</span><span class="sxs-lookup"><span data-stu-id="9ef17-298">Option Group</span></span>

<span data-ttu-id="9ef17-299">Kod HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup) element jest generowany, gdy model widoku zawiera jeden lub więcej `SelectListGroup` obiektów.</span><span class="sxs-lookup"><span data-stu-id="9ef17-299">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="9ef17-300">`CountryViewModelGroup` Grup `SelectListItem` elementy w grupach "Europy" i "Ameryki Północnej":</span><span class="sxs-lookup"><span data-stu-id="9ef17-300">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="9ef17-301">Poniżej przedstawiono dwie grupy:</span><span class="sxs-lookup"><span data-stu-id="9ef17-301">The two groups are shown below:</span></span>

![przykład grupy opcji](working-with-forms/_static/grp.png)

<span data-ttu-id="9ef17-303">Wygenerowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="9ef17-303">The generated HTML:</span></span>

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a><span data-ttu-id="9ef17-304">Wielokrotny wybór</span><span class="sxs-lookup"><span data-stu-id="9ef17-304">Multiple select</span></span>

<span data-ttu-id="9ef17-305">Automatycznie wygeneruje pomocnika tagów wybierz [wielu = "wielu"](http://w3c.github.io/html-reference/select.html) atrybut, jeśli określona właściwość w `asp-for` atrybutu `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="9ef17-305">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="9ef17-306">Na przykład podane następującego modelu:</span><span class="sxs-lookup"><span data-stu-id="9ef17-306">For example, given the following model:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="9ef17-307">Z następującego widoku:</span><span class="sxs-lookup"><span data-stu-id="9ef17-307">With the following view:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="9ef17-308">Generuje poniższy kod HTML:</span><span class="sxs-lookup"><span data-stu-id="9ef17-308">Generates the following HTML:</span></span>

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a><span data-ttu-id="9ef17-309">Brak zaznaczenia</span><span class="sxs-lookup"><span data-stu-id="9ef17-309">No selection</span></span>

<span data-ttu-id="9ef17-310">Okaże się przy użyciu opcji "nie został określony" na wielu stronach, po utworzeniu szablonu celu wyeliminowania powtarzających HTML:</span><span class="sxs-lookup"><span data-stu-id="9ef17-310">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="9ef17-311">*Views/Shared/EditorTemplates/CountryViewModel.cshtml* szablonu:</span><span class="sxs-lookup"><span data-stu-id="9ef17-311">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="9ef17-312">Dodawanie HTML [ \<opcji >](https://www.w3.org/wiki/HTML/Elements/option) elementów nie jest ograniczona do *Brak zaznaczenia* case.</span><span class="sxs-lookup"><span data-stu-id="9ef17-312">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements isn't limited to the *No selection* case.</span></span> <span data-ttu-id="9ef17-313">Na przykład następujące metody akcji i widoku wygeneruje HTML, podobnie jak w powyższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="9ef17-313">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="9ef17-314">Poprawny `<option>` będzie można wybrać elementu (zawierać `selected="selected"` atrybut) w zależności od bieżącej `Country` wartość.</span><span class="sxs-lookup"><span data-stu-id="9ef17-314">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a><span data-ttu-id="9ef17-315">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9ef17-315">Additional resources</span></span>

* [<span data-ttu-id="9ef17-316">Pomocnicy tagów</span><span class="sxs-lookup"><span data-stu-id="9ef17-316">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="9ef17-317">Element formularza HTML</span><span class="sxs-lookup"><span data-stu-id="9ef17-317">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)
* [<span data-ttu-id="9ef17-318">Token weryfikacji żądania</span><span class="sxs-lookup"><span data-stu-id="9ef17-318">Request Verification Token</span></span>](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)
* [<span data-ttu-id="9ef17-319">Wiązanie modelu</span><span class="sxs-lookup"><span data-stu-id="9ef17-319">Model Binding</span></span>](xref:mvc/models/model-binding)
* [<span data-ttu-id="9ef17-320">Weryfikacja modelu</span><span class="sxs-lookup"><span data-stu-id="9ef17-320">Model Validation</span></span>](xref:mvc/models/validation)
* [<span data-ttu-id="9ef17-321">Interfejs IAttributeAdapter</span><span class="sxs-lookup"><span data-stu-id="9ef17-321">IAttributeAdapter Interface</span></span>](/dotnet/api/Microsoft.AspNetCore.Mvc.DataAnnotations.IAttributeAdapter)
* [<span data-ttu-id="9ef17-322">Wstawki kodu dla tego dokumentu</span><span class="sxs-lookup"><span data-stu-id="9ef17-322">Code snippets for this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/working-with-forms/sample/final)
