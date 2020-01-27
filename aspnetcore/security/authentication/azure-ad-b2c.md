---
title: Uwierzytelnianie w chmurze za pomocą Azure Active Directory B2C w ASP.NET Core
author: camsoper
description: Dowiedz się, jak skonfigurować uwierzytelnianie Azure Active Directory B2C przy użyciu ASP.NET Core.
ms.author: casoper
ms.custom: mvc
ms.date: 01/21/2019
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 136fa47788456492a9a7fe6d9d9e5996c13e8c20
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727274"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a><span data-ttu-id="70e72-103">Uwierzytelnianie w chmurze za pomocą Azure Active Directory B2C w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70e72-103">Cloud authentication with Azure Active Directory B2C in ASP.NET Core</span></span>

<span data-ttu-id="70e72-104">Przez [Soper kamery](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="70e72-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="70e72-105">[Usługa Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) to rozwiązanie zarządzania tożsamością w chmurze dla aplikacji internetowych i mobilnych.</span><span class="sxs-lookup"><span data-stu-id="70e72-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) is a cloud identity management solution for web and mobile apps.</span></span> <span data-ttu-id="70e72-106">Usługa zapewnia uwierzytelnianie dla aplikacji hostowanych w chmurze i lokalnych.</span><span class="sxs-lookup"><span data-stu-id="70e72-106">The service provides authentication for apps hosted in the cloud and on-premises.</span></span> <span data-ttu-id="70e72-107">Typy uwierzytelniania obejmują indywidualnych kont, kont sieci społecznościowych i federacyjnych konta przedsiębiorstwa.</span><span class="sxs-lookup"><span data-stu-id="70e72-107">Authentication types include individual accounts, social network accounts, and federated enterprise accounts.</span></span> <span data-ttu-id="70e72-108">Ponadto Azure AD B2C może zapewnić uwierzytelnianie wieloskładnikowe z minimalną konfiguracją.</span><span class="sxs-lookup"><span data-stu-id="70e72-108">Additionally, Azure AD B2C can provide multi-factor authentication with minimal configuration.</span></span>

> [!TIP]
> <span data-ttu-id="70e72-109">Usługa Azure Active Directory (Azure AD) i Azure AD B2C są osobne oferty.</span><span class="sxs-lookup"><span data-stu-id="70e72-109">Azure Active Directory (Azure AD) and Azure AD B2C are separate product offerings.</span></span> <span data-ttu-id="70e72-110">Dzierżawy usługi Azure AD organizacja, podczas gdy dzierżawy usługi Azure AD B2C reprezentuje kolekcję tożsamości do użycia z aplikacjami danej firmy.</span><span class="sxs-lookup"><span data-stu-id="70e72-110">An Azure AD tenant represents an organization, while an Azure AD B2C tenant represents a collection of identities to be used with relying party applications.</span></span> <span data-ttu-id="70e72-111">Aby dowiedzieć się więcej, zobacz [usługi Azure AD B2C: często zadawane pytania (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span><span class="sxs-lookup"><span data-stu-id="70e72-111">To learn more, see [Azure AD B2C: Frequently asked questions (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span></span>

<span data-ttu-id="70e72-112">W tym samouczku pokazano, jak:</span><span class="sxs-lookup"><span data-stu-id="70e72-112">In this tutorial, learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="70e72-113">Tworzenie dzierżawy Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="70e72-113">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="70e72-114">Rejestrowanie aplikacji w Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="70e72-114">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="70e72-115">Użyj programu Visual Studio, aby utworzyć aplikację sieci Web ASP.NET Core skonfigurowaną do korzystania z dzierżawy Azure AD B2C na potrzeby uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="70e72-115">Use Visual Studio to create an ASP.NET Core web app configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="70e72-116">Konfigurowanie zasad kontrolujących zachowanie dzierżawy Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="70e72-116">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

## <a name="prerequisites"></a><span data-ttu-id="70e72-117">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="70e72-117">Prerequisites</span></span>

<span data-ttu-id="70e72-118">Wymagane jest spełnienie następujących w ramach tego przewodnika:</span><span class="sxs-lookup"><span data-stu-id="70e72-118">The following are required for this walkthrough:</span></span>

* [<span data-ttu-id="70e72-119">Subskrypcja Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="70e72-119">Microsoft Azure subscription</span></span>](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [<span data-ttu-id="70e72-120">Visual Studio 2019</span><span class="sxs-lookup"><span data-stu-id="70e72-120">Visual Studio 2019</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

## <a name="create-the-azure-active-directory-b2c-tenant"></a><span data-ttu-id="70e72-121">Tworzenie dzierżawy usługi Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="70e72-121">Create the Azure Active Directory B2C tenant</span></span>

<span data-ttu-id="70e72-122">Utwórz dzierżawę Azure Active Directory B2C [, zgodnie z opisem w dokumentacji](/azure/active-directory-b2c/active-directory-b2c-get-started).</span><span class="sxs-lookup"><span data-stu-id="70e72-122">Create an Azure Active Directory B2C tenant [as described in the documentation](/azure/active-directory-b2c/active-directory-b2c-get-started).</span></span> <span data-ttu-id="70e72-123">Po wyświetleniu monitu skojarzenie dzierżawcy z subskrypcją platformy Azure jest opcjonalny w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="70e72-123">When prompted, associating the tenant with an Azure subscription is optional for this tutorial.</span></span>

## <a name="register-the-app-in-azure-ad-b2c"></a><span data-ttu-id="70e72-124">Zarejestruj aplikację w Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="70e72-124">Register the app in Azure AD B2C</span></span>

<span data-ttu-id="70e72-125">W nowo utworzonym dzierżawie Azure AD B2C Zarejestruj swoją aplikację, korzystając [z procedury opisanej w dokumentacji](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application) w sekcji **Rejestrowanie aplikacji sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="70e72-125">In the newly created Azure AD B2C tenant, register your app using [the steps in the documentation](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application) under the **Register a web app** section.</span></span> <span data-ttu-id="70e72-126">Zatrzyma **Tworzenie klucza tajnego klienta aplikacji sieci web** sekcji.</span><span class="sxs-lookup"><span data-stu-id="70e72-126">Stop at the **Create a web app client secret** section.</span></span> <span data-ttu-id="70e72-127">Klucz tajny klienta nie jest wymagana na potrzeby tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="70e72-127">A client secret isn't required for this tutorial.</span></span> 

<span data-ttu-id="70e72-128">Użyj następujących wartości:</span><span class="sxs-lookup"><span data-stu-id="70e72-128">Use the following values:</span></span>

| <span data-ttu-id="70e72-129">Ustawienie</span><span class="sxs-lookup"><span data-stu-id="70e72-129">Setting</span></span>                       | <span data-ttu-id="70e72-130">Wartość</span><span class="sxs-lookup"><span data-stu-id="70e72-130">Value</span></span>                     | <span data-ttu-id="70e72-131">Uwagi</span><span class="sxs-lookup"><span data-stu-id="70e72-131">Notes</span></span>                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="70e72-132">**Nazwa**</span><span class="sxs-lookup"><span data-stu-id="70e72-132">**Name**</span></span>                      | <span data-ttu-id="70e72-133">*Nazwa &lt;aplikacji&gt;*</span><span class="sxs-lookup"><span data-stu-id="70e72-133">*&lt;app name&gt;*</span></span>        | <span data-ttu-id="70e72-134">Wprowadź **nazwa** dla aplikacji, który opisuje aplikację dla użytkowników.</span><span class="sxs-lookup"><span data-stu-id="70e72-134">Enter a **Name** for the app that describes your app to consumers.</span></span>                                                                                                                                 |
| <span data-ttu-id="70e72-135">**Uwzględnij aplikację sieci web / interfejs API sieci web**</span><span class="sxs-lookup"><span data-stu-id="70e72-135">**Include web app / web API**</span></span> | <span data-ttu-id="70e72-136">Tak</span><span class="sxs-lookup"><span data-stu-id="70e72-136">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="70e72-137">**Zezwalaj na niejawny przepływ**</span><span class="sxs-lookup"><span data-stu-id="70e72-137">**Allow implicit flow**</span></span>       | <span data-ttu-id="70e72-138">Tak</span><span class="sxs-lookup"><span data-stu-id="70e72-138">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="70e72-139">**Adres URL odpowiedzi**</span><span class="sxs-lookup"><span data-stu-id="70e72-139">**Reply URL**</span></span>                 | `https://localhost:44300/signin-oidc` | <span data-ttu-id="70e72-140">Adresy URL odpowiedzi to punkty końcowe, w którym usługi Azure AD B2C zwraca wszelkie tokeny żądane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="70e72-140">Reply URLs are endpoints where Azure AD B2C returns any tokens that your app requests.</span></span> <span data-ttu-id="70e72-141">Program Visual Studio udostępnia adres URL odpowiedzi, który ma być używany.</span><span class="sxs-lookup"><span data-stu-id="70e72-141">Visual Studio provides the Reply URL to use.</span></span> <span data-ttu-id="70e72-142">Na razie wprowadź `https://localhost:44300/signin-oidc`, aby zakończyć formularz.</span><span class="sxs-lookup"><span data-stu-id="70e72-142">For now, enter `https://localhost:44300/signin-oidc` to complete the form.</span></span> |
| <span data-ttu-id="70e72-143">**Identyfikator URI Identyfikatora aplikacji**</span><span class="sxs-lookup"><span data-stu-id="70e72-143">**App ID URI**</span></span>                | <span data-ttu-id="70e72-144">Pozostaw puste</span><span class="sxs-lookup"><span data-stu-id="70e72-144">Leave blank</span></span>               | <span data-ttu-id="70e72-145">Nie jest wymagane na potrzeby tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="70e72-145">Not required for this tutorial.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="70e72-146">**Dołącz klienta natywnego**</span><span class="sxs-lookup"><span data-stu-id="70e72-146">**Include native client**</span></span>     | <span data-ttu-id="70e72-147">Nie</span><span class="sxs-lookup"><span data-stu-id="70e72-147">No</span></span>                        |                                                                                                                                                                                                    |

> [!WARNING]
> <span data-ttu-id="70e72-148">Jeśli konfigurujesz adres URL odpowiedzi bez hosta lokalnego, weź pod uwagę [ograniczenia dotyczące tego, co jest dozwolone na liście adresów URL odpowiedzi](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application).</span><span class="sxs-lookup"><span data-stu-id="70e72-148">If setting up a non-localhost Reply URL, be aware of the [constraints on what is allowed in the Reply URL list](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application).</span></span> 

<span data-ttu-id="70e72-149">Po zarejestrowaniu aplikacji zostanie wyświetlona lista aplikacji w dzierżawie.</span><span class="sxs-lookup"><span data-stu-id="70e72-149">After the app is registered, the list of apps in the tenant is displayed.</span></span> <span data-ttu-id="70e72-150">Wybierz aplikację, która została właśnie zarejestrowana.</span><span class="sxs-lookup"><span data-stu-id="70e72-150">Select the app that was just registered.</span></span> <span data-ttu-id="70e72-151">Wybierz **kopiowania** ikony po prawej stronie **identyfikator aplikacji** pola, aby skopiować go do Schowka.</span><span class="sxs-lookup"><span data-stu-id="70e72-151">Select the **Copy** icon to the right of the **Application ID** field to copy it to the clipboard.</span></span>

<span data-ttu-id="70e72-152">W tej chwili nie można skonfigurować niczego w dzierżawie Azure AD B2C, ale pozostawić otwarte okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="70e72-152">Nothing more can be configured in the Azure AD B2C tenant at this time, but leave the browser window open.</span></span> <span data-ttu-id="70e72-153">Po utworzeniu aplikacji ASP.NET Core jest dostępna większa konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="70e72-153">There is more configuration after the ASP.NET Core app is created.</span></span>

## <a name="create-an-aspnet-core-app-in-visual-studio"></a><span data-ttu-id="70e72-154">Tworzenie aplikacji ASP.NET Core w programie Visual Studio</span><span class="sxs-lookup"><span data-stu-id="70e72-154">Create an ASP.NET Core app in Visual Studio</span></span>

<span data-ttu-id="70e72-155">Szablon aplikacji sieci Web w usłudze Visual Studio można skonfigurować do korzystania z dzierżawy usługi Azure AD B2C do uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="70e72-155">The Visual Studio Web Application template can be configured to use the Azure AD B2C tenant for authentication.</span></span>

<span data-ttu-id="70e72-156">W programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="70e72-156">In Visual Studio:</span></span>

1. <span data-ttu-id="70e72-157">Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="70e72-157">Create a new ASP.NET Core Web Application.</span></span> 
2. <span data-ttu-id="70e72-158">Z listy szablonów wybierz pozycję **aplikacja sieci Web** .</span><span class="sxs-lookup"><span data-stu-id="70e72-158">Select **Web Application** from the list of templates.</span></span>
3. <span data-ttu-id="70e72-159">Wybierz **Zmień uwierzytelnianie** przycisku.</span><span class="sxs-lookup"><span data-stu-id="70e72-159">Select the **Change Authentication** button.</span></span>
    
    ![Zmień przycisk uwierzytelniania](./azure-ad-b2c/_static/changeauth.png)

4. <span data-ttu-id="70e72-161">W oknie dialogowym **Zmienianie uwierzytelniania** wybierz pozycję **indywidualne konta użytkowników**, a następnie wybierz pozycję **Połącz z istniejącym magazynem użytkowników w chmurze** na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="70e72-161">In the **Change Authentication** dialog, select **Individual User Accounts**, and then select **Connect to an existing user store in the cloud** in the dropdown.</span></span> 
    
    ![Okno dialogowe uwierzytelniania zmiany](./azure-ad-b2c/_static/changeauthdialog.png)

5. <span data-ttu-id="70e72-163">Wypełnij formularz następującymi wartościami:</span><span class="sxs-lookup"><span data-stu-id="70e72-163">Complete the form with the following values:</span></span>
    
    | <span data-ttu-id="70e72-164">Ustawienie</span><span class="sxs-lookup"><span data-stu-id="70e72-164">Setting</span></span>                       | <span data-ttu-id="70e72-165">Wartość</span><span class="sxs-lookup"><span data-stu-id="70e72-165">Value</span></span>                                                 |
    |-------------------------------|-------------------------------------------------------|
    | <span data-ttu-id="70e72-166">**Nazwa domeny**</span><span class="sxs-lookup"><span data-stu-id="70e72-166">**Domain Name**</span></span>               | <span data-ttu-id="70e72-167">*&lt;nazwę domeny dzierżawy B2C&gt;*</span><span class="sxs-lookup"><span data-stu-id="70e72-167">*&lt;the domain name of your B2C tenant&gt;*</span></span>          |
    | <span data-ttu-id="70e72-168">**Identyfikator aplikacji**</span><span class="sxs-lookup"><span data-stu-id="70e72-168">**Application ID**</span></span>            | <span data-ttu-id="70e72-169">*&lt;wkleić identyfikator aplikacji ze schowka&gt;*</span><span class="sxs-lookup"><span data-stu-id="70e72-169">*&lt;paste the Application ID from the clipboard&gt;*</span></span> |
    | <span data-ttu-id="70e72-170">**Ścieżka wywołania zwrotnego**</span><span class="sxs-lookup"><span data-stu-id="70e72-170">**Callback Path**</span></span>             | <span data-ttu-id="70e72-171">*&lt;użyć wartości domyślnej&gt;*</span><span class="sxs-lookup"><span data-stu-id="70e72-171">*&lt;use the default value&gt;*</span></span>                       |
    | <span data-ttu-id="70e72-172">**Zasady tworzenia konta lub logowania**</span><span class="sxs-lookup"><span data-stu-id="70e72-172">**Sign-up or sign-in policy**</span></span> | `B2C_1_SiUpIn`                                        |
    | <span data-ttu-id="70e72-173">**Zasady resetowania haseł**</span><span class="sxs-lookup"><span data-stu-id="70e72-173">**Reset password policy**</span></span>     | `B2C_1_SSPR`                                          |
    | <span data-ttu-id="70e72-174">**Edytowanie zasad profilu**</span><span class="sxs-lookup"><span data-stu-id="70e72-174">**Edit profile policy**</span></span>       | <span data-ttu-id="70e72-175">*&lt;pozostawić puste&gt;*</span><span class="sxs-lookup"><span data-stu-id="70e72-175">*&lt;leave blank&gt;*</span></span>                                 |
    
    <span data-ttu-id="70e72-176">Wybierz link **Kopiuj** obok **identyfikatora URI odpowiedzi** , aby skopiować identyfikator URI odpowiedzi do Schowka.</span><span class="sxs-lookup"><span data-stu-id="70e72-176">Select the **Copy** link next to **Reply URI** to copy the Reply URI to the clipboard.</span></span> <span data-ttu-id="70e72-177">Wybierz **OK** zamknąć **Zmień uwierzytelnianie** okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="70e72-177">Select **OK** to close the **Change Authentication** dialog.</span></span> <span data-ttu-id="70e72-178">Wybierz **OK** umożliwiające utworzenie aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="70e72-178">Select **OK** to create the web app.</span></span>

## <a name="finish-the-b2c-app-registration"></a><span data-ttu-id="70e72-179">Kończenie rejestracji aplikacji B2C</span><span class="sxs-lookup"><span data-stu-id="70e72-179">Finish the B2C app registration</span></span>

<span data-ttu-id="70e72-180">Wróć do okna przeglądarki z wciąż otwartymi właściwościami aplikacji B2C.</span><span class="sxs-lookup"><span data-stu-id="70e72-180">Return to the browser window with the B2C app properties still open.</span></span> <span data-ttu-id="70e72-181">Zmień tymczasowy **adres URL odpowiedzi** określony wcześniej na wartość skopiowaną z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="70e72-181">Change the temporary **Reply URL** specified earlier to the value copied from Visual Studio.</span></span> <span data-ttu-id="70e72-182">Wybierz pozycję **Zapisz** w górnej części okna.</span><span class="sxs-lookup"><span data-stu-id="70e72-182">Select **Save** at the top of the window.</span></span>

> [!TIP]
> <span data-ttu-id="70e72-183">Jeśli adres URL odpowiedzi nie został skopiowany, użyj adresu HTTPS z karty debugowanie we właściwościach projektu sieci Web i Dołącz wartość **CallbackPath** z pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="70e72-183">If you didn't copy the Reply URL, use the HTTPS address from the Debug tab in the web project properties, and append the **CallbackPath** value from *appsettings.json*.</span></span>

## <a name="configure-policies"></a><span data-ttu-id="70e72-184">Konfigurowanie zasad</span><span class="sxs-lookup"><span data-stu-id="70e72-184">Configure policies</span></span>

<span data-ttu-id="70e72-185">Wykonaj kroki opisane w dokumentacji Azure AD B2C, aby [utworzyć zasady tworzenia konta lub logowania](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions), a następnie [Utwórz zasady resetowania hasła](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions).</span><span class="sxs-lookup"><span data-stu-id="70e72-185">Use the steps in the Azure AD B2C documentation to [create a sign-up or sign-in policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions), and then [create a password reset policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions).</span></span> <span data-ttu-id="70e72-186">Użyj przykładowych wartości podane w dokumentacji dotyczącej **dostawców tożsamości**, **atrybuty tworzenia konta**, i **oświadczeń aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="70e72-186">Use the example values provided in the documentation for **Identity providers**, **Sign-up attributes**, and **Application claims**.</span></span> <span data-ttu-id="70e72-187">Użycie przycisku **Uruchom teraz** w celu przetestowania zasad zgodnie z opisem w dokumentacji jest opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="70e72-187">Using the **Run now** button to test the policies as described in the documentation is optional.</span></span>

> [!WARNING]
> <span data-ttu-id="70e72-188">Upewnij się, że nazwy zasad są dokładnie zgodnie z opisem w dokumentacji, ponieważ te zasady były używane w oknie dialogowym **Zmienianie uwierzytelniania** w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="70e72-188">Ensure the policy names are exactly as described in the documentation, as those policies were used in the **Change Authentication** dialog in Visual Studio.</span></span> <span data-ttu-id="70e72-189">Nazwy zasad można weryfikować w pliku *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="70e72-189">The policy names can be verified in *appsettings.json*.</span></span>

## <a name="configure-the-underlying-openidconnectoptionsjwtbearercookie-options"></a><span data-ttu-id="70e72-190">Konfigurowanie podstawowych opcji OpenIdConnectOptions/JwtBearer/cookie</span><span class="sxs-lookup"><span data-stu-id="70e72-190">Configure the underlying OpenIdConnectOptions/JwtBearer/Cookie options</span></span>

<span data-ttu-id="70e72-191">Aby bezpośrednio skonfigurować podstawowe opcje, użyj odpowiedniej stałej schematu w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="70e72-191">To configure the underlying options directly, use the appropriate scheme constant in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<OpenIdConnectOptions>(
    AzureAD[B2C]Defaults.OpenIdScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<CookieAuthenticationOptions>(
    AzureAD[B2C]Defaults.CookieScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<JwtBearerOptions>(
    AzureAD[B2C]Defaults.JwtBearerAuthenticationScheme, options => 
    {
        // Omitted for brevity
    });
```

## <a name="run-the-app"></a><span data-ttu-id="70e72-192">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="70e72-192">Run the app</span></span>

<span data-ttu-id="70e72-193">W programie Visual Studio naciśnij klawisz **F5** , aby skompilować i uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="70e72-193">In Visual Studio, press **F5** to build and run the app.</span></span> <span data-ttu-id="70e72-194">Po uruchomieniu aplikacji sieci Web wybierz pozycję **Akceptuj** , aby zaakceptować użycie plików cookie (Jeśli zostanie wyświetlony monit), a następnie wybierz pozycję **Zaloguj się**.</span><span class="sxs-lookup"><span data-stu-id="70e72-194">After the web app launches, select **Accept** to accept the use of cookies (if prompted), and then select **Sign in**.</span></span>

![Zaloguj się do aplikacji](./azure-ad-b2c/_static/signin.png)

<span data-ttu-id="70e72-196">Przeglądarka przekieruje do dzierżawy Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="70e72-196">The browser redirects to the Azure AD B2C tenant.</span></span> <span data-ttu-id="70e72-197">Zaloguj się przy użyciu istniejącego konta, (Jeśli utworzono jedną testowania zasad) lub wybierz **Zarejestruj się teraz** do utworzenia nowego konta.</span><span class="sxs-lookup"><span data-stu-id="70e72-197">Sign in with an existing account (if one was created testing the policies) or select **Sign up now** to create a new account.</span></span> <span data-ttu-id="70e72-198">**Nie pamiętasz hasła?** łącze służy do zresetować zapomniane hasło.</span><span class="sxs-lookup"><span data-stu-id="70e72-198">The **Forgot your password?** link is used to reset a forgotten password.</span></span>

![Azure AD B2C logowanie](./azure-ad-b2c/_static/b2csts.png)

<span data-ttu-id="70e72-200">Po pomyślnym zalogowaniu przeglądarka przekieruje ją do aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="70e72-200">After successfully signing in, the browser redirects to the web app.</span></span>

![Powodzenie](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a><span data-ttu-id="70e72-202">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="70e72-202">Next steps</span></span>

<span data-ttu-id="70e72-203">W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="70e72-203">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="70e72-204">Tworzenie dzierżawy Azure Active Directory B2C</span><span class="sxs-lookup"><span data-stu-id="70e72-204">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="70e72-205">Rejestrowanie aplikacji w Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="70e72-205">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="70e72-206">Użyj programu Visual Studio, aby utworzyć aplikację sieci Web ASP.NET Core skonfigurowaną do korzystania z dzierżawy Azure AD B2C na potrzeby uwierzytelniania</span><span class="sxs-lookup"><span data-stu-id="70e72-206">Use Visual Studio to create an ASP.NET Core Web Application configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="70e72-207">Konfigurowanie zasad kontrolujących zachowanie dzierżawy Azure AD B2C</span><span class="sxs-lookup"><span data-stu-id="70e72-207">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

<span data-ttu-id="70e72-208">Teraz, gdy aplikacja ASP.NET Core jest skonfigurowana do używania Azure AD B2C do uwierzytelniania, do zabezpieczenia aplikacji można użyć [atrybutu Autoryzuj](xref:security/authorization/simple) .</span><span class="sxs-lookup"><span data-stu-id="70e72-208">Now that the ASP.NET Core app is configured to use Azure AD B2C for authentication, the [Authorize attribute](xref:security/authorization/simple) can be used to secure your app.</span></span> <span data-ttu-id="70e72-209">Kontynuuj opracowywanie aplikacji, przepoznając się z:</span><span class="sxs-lookup"><span data-stu-id="70e72-209">Continue developing your app by learning to:</span></span>

* <span data-ttu-id="70e72-210">[Dostosowywanie interfejsu użytkownika usługi Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span><span class="sxs-lookup"><span data-stu-id="70e72-210">[Customize the Azure AD B2C user interface](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span></span>
* <span data-ttu-id="70e72-211">[Skonfiguruj wymagania dotyczące złożoności hasła](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span><span class="sxs-lookup"><span data-stu-id="70e72-211">[Configure password complexity requirements](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span></span>
* <span data-ttu-id="70e72-212">[Włącz uwierzytelnianie wieloskładnikowe](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span><span class="sxs-lookup"><span data-stu-id="70e72-212">[Enable multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span></span>
* <span data-ttu-id="70e72-213">Konfigurowanie dostawców tożsamości dodatkowe, takie jak [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [w usłudze Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)i innym osobom.</span><span class="sxs-lookup"><span data-stu-id="70e72-213">Configure additional identity providers, such as [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), and others.</span></span>
* <span data-ttu-id="70e72-214">[Za pomocą interfejsu API programu Graph usługi Azure AD](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) do pobierania dodatkowych informacji dotyczących użytkowników, takich jak członkostwo w grupie z dzierżawy usługi Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="70e72-214">[Use the Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) to retrieve additional user information, such as group membership, from the Azure AD B2C tenant.</span></span>
* <span data-ttu-id="70e72-215">[Zabezpiecz internetowy interfejs API ASP.NET Core przy użyciu Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapi/).</span><span class="sxs-lookup"><span data-stu-id="70e72-215">[Secure an ASP.NET Core web API using Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapi/).</span></span>
* <span data-ttu-id="70e72-216">[Wywoływanie interfejsu web API platformy .NET z aplikacji sieci web platformy .NET przy użyciu usługi Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).</span><span class="sxs-lookup"><span data-stu-id="70e72-216">[Call a .NET web API from a .NET web app using Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).</span></span>