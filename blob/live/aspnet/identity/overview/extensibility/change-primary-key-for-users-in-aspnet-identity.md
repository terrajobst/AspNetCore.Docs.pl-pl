---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: "Należy zmienić wartość klucza podstawowego dla użytkowników w produkcie ASP.NET Identity | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W programie Visual Studio 2013 domyślnej aplikacji sieci web używa wartości ciągu klucza dla konta użytkownika. ASP.NET Identity umożliwia zmianę typu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="4a0f9-104">Należy zmienić wartość klucza podstawowego dla użytkowników w produkcie ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="4a0f9-104">Change Primary Key for Users in ASP.NET Identity</span></span>
====================
<span data-ttu-id="4a0f9-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4a0f9-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4a0f9-106">W programie Visual Studio 2013 domyślnej aplikacji sieci web używa wartości ciągu klucza dla konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="4a0f9-107">ASP.NET Identity umożliwia zmianę typu klucza zgodnie z wymaganiami danych.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="4a0f9-108">Na przykład można zmienić typu klucza z ciągu na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="4a0f9-109">W tym temacie przedstawiono, jak zacząć domyślnej aplikacji sieci web i zmienić wartość klucza konta użytkownika na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="4a0f9-110">Te same zmiany można użyć do wykonania dowolnego typu klucza w projekcie.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="4a0f9-111">Demonstracja do wprowadzenia tych zmian w domyślnej aplikacji sieci web, ale można zastosować zmian podobne do niestandardowych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="4a0f9-112">Przedstawia on zmiany podczas pracy z MVC i formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4a0f9-113">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="4a0f9-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="4a0f9-114">Visual Studio 2013 Update 2 (lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="4a0f9-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="4a0f9-115">ASP.NET Identity 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="4a0f9-115">ASP.NET Identity 2.1 or later</span></span>


<span data-ttu-id="4a0f9-116">Aby wykonać kroki opisane w tym samouczku, musi mieć Visual Studio 2013 Update 2 (lub nowszego) i aplikacji sieci web utworzone na podstawie szablonu aplikacji sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="4a0f9-117">Szablon zmienione w aktualizacji Update 3.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-117">The template changed in Update 3.</span></span> <span data-ttu-id="4a0f9-118">W tym temacie pokazano, jak zmienić szablonu w Update 2 i Update 3.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="4a0f9-119">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="4a0f9-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="4a0f9-120">Zmień typ klucza w klasie tożsamości użytkownika</span><span class="sxs-lookup"><span data-stu-id="4a0f9-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="4a0f9-121">Dodania niestandardowych klas tożsamości, korzystających z typu klucza</span><span class="sxs-lookup"><span data-stu-id="4a0f9-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="4a0f9-122">Zmienianie menedżera użytkownika oraz klasy kontekstu do użycia z typem klucza</span><span class="sxs-lookup"><span data-stu-id="4a0f9-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="4a0f9-123">Zmień konfigurację uruchamiania, aby użyć klucza typu</span><span class="sxs-lookup"><span data-stu-id="4a0f9-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="4a0f9-124">Dla platformy MVC z Update 2 można zmienić elementu AccountController przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="4a0f9-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="4a0f9-125">Dla platformy MVC z aktualizacją Update 3 Zmień elementu AccountController oraz ManageController przekazywanie typu klucza</span><span class="sxs-lookup"><span data-stu-id="4a0f9-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="4a0f9-126">Dla formularzy sieci Web z Update 2 Zmień konto strony do przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="4a0f9-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="4a0f9-127">Dla formularzy sieci Web z aktualizacją Update 3 Zmień konto strony do przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="4a0f9-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="4a0f9-128">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4a0f9-128">Run application</span></span>](#run)
- [<span data-ttu-id="4a0f9-129">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="4a0f9-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="4a0f9-130">Zmień typ klucza w klasie tożsamości użytkownika</span><span class="sxs-lookup"><span data-stu-id="4a0f9-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="4a0f9-131">W projekcie utworzone na podstawie szablonu aplikacji sieci Web platformy ASP.NET należy określić, że klasy ApplicationUser używa całkowitą klucza dla konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="4a0f9-132">W IdentityModels.cs, zmień klasy ApplicationUser odziedziczone IdentityUser, który ma typ **int** dla parametru ogólnego TKey.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="4a0f9-133">Możesz również przekazać nazwy trzy dostosowane klasy, które nie zostało jeszcze zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="4a0f9-134">Zmieniono typ klucza, ale domyślnie reszty aplikacji nadal przyjęto założenie, że klucz jest ciąg.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="4a0f9-135">Jawnie musi wskazywać typ klucza w kodzie, który przyjmuje ciąg.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="4a0f9-136">W **ApplicationUser** klasy, zmień **GenerateUserIdentityAsync** metodę w celu uwzględnienia int, jak pokazano w poniższym kodzie zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="4a0f9-137">Ta zmiana nie jest niezbędna dla projektów formularzy sieci Web przy użyciu szablonu Update 3.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="4a0f9-138">Dodania niestandardowych klas tożsamości, korzystających z typu klucza</span><span class="sxs-lookup"><span data-stu-id="4a0f9-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="4a0f9-139">Inne klasy tożsamości, takie jak IdentityUserRole IdentityUserClaim, IdentityUserLogin, IdentityRole, magazynie UserStore, elemencie RoleStore, są nadal skonfigurowane do użycia klucza ciągu.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="4a0f9-140">Utwórz nowe wersje tych klas, które określają całkowitą dla klucza.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="4a0f9-141">Nie trzeba podać kod wiele implementacji w tych klas, przede wszystkim tylko ustawieniu int jako klucz.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="4a0f9-142">Dodaj następujące klasy w pliku IdentityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="4a0f9-143">Zmienianie menedżera użytkownika oraz klasy kontekstu do użycia z typem klucza</span><span class="sxs-lookup"><span data-stu-id="4a0f9-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="4a0f9-144">W IdentityModels.cs, Zmień definicję **ApplicationDbContext** klasę, aby użyć nowego dostosowane klas i **int** klucza, jak pokazano w wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="4a0f9-145">Parametr ThrowIfV1Schema nie jest już prawidłowy w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="4a0f9-146">Zmień konstruktora, aby nie zostały spełnione wartości ThrowIfV1Schema.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="4a0f9-147">Otwórz IdentityConfig.cs i zmień **ApplicationUserManger** klasę, aby użyć nowego użytkownika przechowywania klasy trwałych danych i **int** dla klucza.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="4a0f9-148">W szablonie Update 3 możesz zmienić klasy ApplicationSignInManager.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="4a0f9-149">Zmień konfigurację uruchamiania, aby użyć klucza typu</span><span class="sxs-lookup"><span data-stu-id="4a0f9-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="4a0f9-150">W Startup.Auth.cs Zastąp kod element OnValidateIdentity, jak wyróżniono poniżej.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="4a0f9-151">Zwróć uwagę, że definicja getUserIdCallback analizuje wartość ciągu na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="4a0f9-152">Jeśli projekt nie rozpoznaje ogólną implementację **metodę GetUserId** metody, konieczne może być pakietu ASP.NET Identity NuGet aktualizacji do wersji 2.1</span><span class="sxs-lookup"><span data-stu-id="4a0f9-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="4a0f9-153">Wprowadzono wiele zmian dla klasy infrastruktury używane przez program ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="4a0f9-154">Podczas kompilowania projektu można zauważyć wiele błędów.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="4a0f9-155">Na szczęście pozostałe błędy są podobne.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="4a0f9-156">Klasa tożsamości oczekuje liczby całkowitej dla klucza, ale kontrolera (lub formularza sieci Web) jest przekazanie wartości ciągu.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="4a0f9-157">W każdym przypadku należy przekonwertować ciąg i liczby całkowitej przez wywołanie metody **metodę GetUserId&lt;int&gt;**.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="4a0f9-158">Możesz skorzystać z listy błędów z kompilacji lub wykonaj poniższe zmiany.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="4a0f9-159">Pozostałe zmiany są zależne od typu projektu tworzenia i aktualizacji, które zainstalowano w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="4a0f9-160">Można przejść bezpośrednio do odpowiedniej sekcji za pomocą poniższych łączy</span><span class="sxs-lookup"><span data-stu-id="4a0f9-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="4a0f9-161">Dla platformy MVC z Update 2 można zmienić elementu AccountController przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="4a0f9-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="4a0f9-162">Dla platformy MVC z aktualizacją Update 3 Zmień elementu AccountController oraz ManageController przekazywanie typu klucza</span><span class="sxs-lookup"><span data-stu-id="4a0f9-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="4a0f9-163">Dla formularzy sieci Web z Update 2 Zmień konto strony do przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="4a0f9-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="4a0f9-164">Dla formularzy sieci Web z aktualizacją Update 3 Zmień konto strony do przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="4a0f9-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="4a0f9-165">Dla platformy MVC z Update 2 można zmienić elementu AccountController przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="4a0f9-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="4a0f9-166">Otwórz plik AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="4a0f9-167">Musisz zmienić z następujących metod.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-167">You need to change the following methods.</span></span>

<span data-ttu-id="4a0f9-168">**ConfirmEmail** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="4a0f9-169">**Usuń skojarzenie** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="4a0f9-170">**Manage(ManageUserViewModel)** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="4a0f9-171">**LinkLoginCallback** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="4a0f9-172">**RemoveAccountList** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="4a0f9-173">**HasPassword** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="4a0f9-174">Możesz teraz [uruchomić aplikację](#run) i zarejestrować nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="4a0f9-175">Dla platformy MVC z aktualizacją Update 3 Zmień elementu AccountController oraz ManageController przekazywanie typu klucza</span><span class="sxs-lookup"><span data-stu-id="4a0f9-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="4a0f9-176">Otwórz plik AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="4a0f9-177">Musisz zmienić następującą metodę.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-177">You need to change the following method.</span></span>

<span data-ttu-id="4a0f9-178">**ConfirmEmail** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="4a0f9-179">**SendCode** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="4a0f9-180">Otwórz plik ManageController.cs.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="4a0f9-181">Musisz zmienić z następujących metod.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-181">You need to change the following methods.</span></span>

<span data-ttu-id="4a0f9-182">**Indeks** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="4a0f9-183">**RemoveLogin** metody</span><span class="sxs-lookup"><span data-stu-id="4a0f9-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="4a0f9-184">**AddPhoneNumber** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="4a0f9-185">**EnableTwoFactorAuthentication** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="4a0f9-186">**DisableTwoFactorAuthentication** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="4a0f9-187">**VerifyPhoneNumber** metody</span><span class="sxs-lookup"><span data-stu-id="4a0f9-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="4a0f9-188">**RemovePhoneNumber** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="4a0f9-189">**Element ChangePassword** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="4a0f9-190">**UstawianieHasła** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="4a0f9-191">**ManageLogins** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="4a0f9-192">**LinkLoginCallback** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="4a0f9-193">**HasPassword** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="4a0f9-194">**HasPhoneNumber** — metoda</span><span class="sxs-lookup"><span data-stu-id="4a0f9-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="4a0f9-195">Możesz teraz [uruchomić aplikację](#run) i zarejestrować nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="4a0f9-196">Dla formularzy sieci Web z Update 2 Zmień konto strony do przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="4a0f9-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="4a0f9-197">Formularze sieci Web z Update 2 należy zmienić następujące strony.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="4a0f9-198">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="4a0f9-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="4a0f9-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="4a0f9-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="4a0f9-200">**Manage.aspx.CS**</span><span class="sxs-lookup"><span data-stu-id="4a0f9-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="4a0f9-201">Możesz teraz [uruchomić aplikację](#run) i zarejestrować nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="4a0f9-202">Dla formularzy sieci Web z aktualizacją Update 3 Zmień konto strony do przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="4a0f9-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="4a0f9-203">W przypadku formularzy sieci Web z aktualizacją Update 3 musisz zmienić następujących stron.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="4a0f9-204">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="4a0f9-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="4a0f9-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="4a0f9-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="4a0f9-206">**Manage.aspx.CS**</span><span class="sxs-lookup"><span data-stu-id="4a0f9-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="4a0f9-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="4a0f9-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="4a0f9-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="4a0f9-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="4a0f9-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="4a0f9-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="4a0f9-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="4a0f9-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="4a0f9-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="4a0f9-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="4a0f9-212">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="4a0f9-212">Run application</span></span>

<span data-ttu-id="4a0f9-213">Zakończono wszystkie wymagane zmiany domyślnego szablonu aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="4a0f9-214">Uruchom aplikację i zarejestrować nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-214">Run the application and register a new user.</span></span> <span data-ttu-id="4a0f9-215">Po zarejestrowaniu użytkownika, można zauważyć, że tabela AspNetUsers zawiera identyfikator kolumny, która jest liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![nowy klucz podstawowy](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="4a0f9-217">Jeśli wcześniej utworzono ASP.NET Identity tabel za pomocą innego klucza podstawowego, należy wprowadzić kilka dodatkowych zmian.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="4a0f9-218">Jeśli to możliwe można usunąć istniejącą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="4a0f9-219">Bazy danych zostanie ponownie utworzony z poprawny projekt, po uruchomieniu aplikacji sieci web i dodać nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="4a0f9-220">Jeśli usunięcia nie jest możliwe, uruchom migracje code first, aby zmienić tabel.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="4a0f9-221">Jednakże nowy klucz podstawowy całkowitą nie zostanie ustawiona jako właściwość SQL IDENTITY w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="4a0f9-222">W kolumnie Identyfikator musi ręcznie Ustaw jako tożsamości.</span><span class="sxs-lookup"><span data-stu-id="4a0f9-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="4a0f9-223">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="4a0f9-223">Other resources</span></span>

- [<span data-ttu-id="4a0f9-224">Przegląd dostawców magazynu niestandardowego dla tożsamości ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4a0f9-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="4a0f9-225">Migrowanie istniejącej witryny sieci Web z członkostwa SQL do tożsamości platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4a0f9-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="4a0f9-226">Migrowanie danych uniwersalnych dostawcy członkostwa i profilów użytkownika dla tożsamości ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4a0f9-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="4a0f9-227">[Przykładowa aplikacja](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) z zmienione klucza podstawowego</span><span class="sxs-lookup"><span data-stu-id="4a0f9-227">[Sample application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) with changed primary key</span></span>
