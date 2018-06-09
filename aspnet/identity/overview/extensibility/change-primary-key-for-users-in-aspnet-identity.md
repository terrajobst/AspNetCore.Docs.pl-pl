---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Należy zmienić wartość klucza podstawowego dla użytkowników w produkcie ASP.NET Identity | Dokumentacja firmy Microsoft
author: tfitzmac
description: W programie Visual Studio 2013 domyślnej aplikacji sieci web używa wartości ciągu klucza dla konta użytkownika. ASP.NET Identity umożliwia zmianę typu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "26563774"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="7152a-104">Należy zmienić wartość klucza podstawowego dla użytkowników w produkcie ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="7152a-104">Change Primary Key for Users in ASP.NET Identity</span></span>
====================
<span data-ttu-id="7152a-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7152a-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7152a-106">W programie Visual Studio 2013 domyślnej aplikacji sieci web używa wartości ciągu klucza dla konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7152a-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="7152a-107">ASP.NET Identity umożliwia zmianę typu klucza zgodnie z wymaganiami danych.</span><span class="sxs-lookup"><span data-stu-id="7152a-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="7152a-108">Na przykład można zmienić typu klucza z ciągu na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="7152a-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="7152a-109">W tym temacie przedstawiono, jak zacząć domyślnej aplikacji sieci web i zmienić wartość klucza konta użytkownika na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="7152a-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="7152a-110">Te same zmiany można użyć do wykonania dowolnego typu klucza w projekcie.</span><span class="sxs-lookup"><span data-stu-id="7152a-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="7152a-111">Demonstracja do wprowadzenia tych zmian w domyślnej aplikacji sieci web, ale można zastosować zmian podobne do niestandardowych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7152a-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="7152a-112">Przedstawia on zmiany podczas pracy z MVC i formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7152a-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7152a-113">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="7152a-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7152a-114">Visual Studio 2013 Update 2 (lub nowszy)</span><span class="sxs-lookup"><span data-stu-id="7152a-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="7152a-115">ASP.NET Identity 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="7152a-115">ASP.NET Identity 2.1 or later</span></span>


<span data-ttu-id="7152a-116">Aby wykonać kroki opisane w tym samouczku, musi mieć Visual Studio 2013 Update 2 (lub nowszego) i aplikacji sieci web utworzone na podstawie szablonu aplikacji sieci Web platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7152a-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="7152a-117">Szablon zmienione w aktualizacji Update 3.</span><span class="sxs-lookup"><span data-stu-id="7152a-117">The template changed in Update 3.</span></span> <span data-ttu-id="7152a-118">W tym temacie pokazano, jak zmienić szablonu w Update 2 i Update 3.</span><span class="sxs-lookup"><span data-stu-id="7152a-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="7152a-119">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="7152a-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="7152a-120">Zmień typ klucza w klasie tożsamości użytkownika</span><span class="sxs-lookup"><span data-stu-id="7152a-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="7152a-121">Dodania niestandardowych klas tożsamości, korzystających z typu klucza</span><span class="sxs-lookup"><span data-stu-id="7152a-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="7152a-122">Zmienianie menedżera użytkownika oraz klasy kontekstu do użycia z typem klucza</span><span class="sxs-lookup"><span data-stu-id="7152a-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="7152a-123">Zmień konfigurację uruchamiania, aby użyć klucza typu</span><span class="sxs-lookup"><span data-stu-id="7152a-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="7152a-124">Dla platformy MVC z Update 2 można zmienić elementu AccountController przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="7152a-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="7152a-125">Dla platformy MVC z aktualizacją Update 3 Zmień elementu AccountController oraz ManageController przekazywanie typu klucza</span><span class="sxs-lookup"><span data-stu-id="7152a-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="7152a-126">Dla formularzy sieci Web z Update 2 Zmień konto strony do przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="7152a-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="7152a-127">Dla formularzy sieci Web z aktualizacją Update 3 Zmień konto strony do przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="7152a-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="7152a-128">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="7152a-128">Run application</span></span>](#run)
- [<span data-ttu-id="7152a-129">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="7152a-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="7152a-130">Zmień typ klucza w klasie tożsamości użytkownika</span><span class="sxs-lookup"><span data-stu-id="7152a-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="7152a-131">W projekcie utworzone na podstawie szablonu aplikacji sieci Web platformy ASP.NET należy określić, że klasy ApplicationUser używa całkowitą klucza dla konta użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7152a-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="7152a-132">W IdentityModels.cs, zmień klasy ApplicationUser odziedziczone IdentityUser, który ma typ **int** dla parametru ogólnego TKey.</span><span class="sxs-lookup"><span data-stu-id="7152a-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="7152a-133">Możesz również przekazać nazwy trzy dostosowane klasy, które nie zostało jeszcze zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="7152a-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="7152a-134">Zmieniono typ klucza, ale domyślnie reszty aplikacji nadal przyjęto założenie, że klucz jest ciąg.</span><span class="sxs-lookup"><span data-stu-id="7152a-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="7152a-135">Jawnie musi wskazywać typ klucza w kodzie, który przyjmuje ciąg.</span><span class="sxs-lookup"><span data-stu-id="7152a-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="7152a-136">W **ApplicationUser** klasy, zmień **GenerateUserIdentityAsync** metodę w celu uwzględnienia int, jak pokazano w poniższym kodzie zaznaczony.</span><span class="sxs-lookup"><span data-stu-id="7152a-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="7152a-137">Ta zmiana nie jest niezbędna dla projektów formularzy sieci Web przy użyciu szablonu Update 3.</span><span class="sxs-lookup"><span data-stu-id="7152a-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="7152a-138">Dodania niestandardowych klas tożsamości, korzystających z typu klucza</span><span class="sxs-lookup"><span data-stu-id="7152a-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="7152a-139">Inne klasy tożsamości, takie jak IdentityUserRole IdentityUserClaim, IdentityUserLogin, IdentityRole, magazynie UserStore, elemencie RoleStore, są nadal skonfigurowane do użycia klucza ciągu.</span><span class="sxs-lookup"><span data-stu-id="7152a-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="7152a-140">Utwórz nowe wersje tych klas, które określają całkowitą dla klucza.</span><span class="sxs-lookup"><span data-stu-id="7152a-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="7152a-141">Nie trzeba podać kod wiele implementacji w tych klas, przede wszystkim tylko ustawieniu int jako klucz.</span><span class="sxs-lookup"><span data-stu-id="7152a-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="7152a-142">Dodaj następujące klasy w pliku IdentityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="7152a-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="7152a-143">Zmienianie menedżera użytkownika oraz klasy kontekstu do użycia z typem klucza</span><span class="sxs-lookup"><span data-stu-id="7152a-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="7152a-144">W IdentityModels.cs, Zmień definicję **ApplicationDbContext** klasę, aby użyć nowego dostosowane klas i **int** klucza, jak pokazano w wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="7152a-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="7152a-145">Parametr ThrowIfV1Schema nie jest już prawidłowy w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="7152a-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="7152a-146">Zmień konstruktora, aby nie zostały spełnione wartości ThrowIfV1Schema.</span><span class="sxs-lookup"><span data-stu-id="7152a-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="7152a-147">Otwórz IdentityConfig.cs i zmień **ApplicationUserManger** klasę, aby użyć nowego użytkownika przechowywania klasy trwałych danych i **int** dla klucza.</span><span class="sxs-lookup"><span data-stu-id="7152a-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="7152a-148">W szablonie Update 3 możesz zmienić klasy ApplicationSignInManager.</span><span class="sxs-lookup"><span data-stu-id="7152a-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="7152a-149">Zmień konfigurację uruchamiania, aby użyć klucza typu</span><span class="sxs-lookup"><span data-stu-id="7152a-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="7152a-150">W Startup.Auth.cs Zastąp kod element OnValidateIdentity, jak wyróżniono poniżej.</span><span class="sxs-lookup"><span data-stu-id="7152a-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="7152a-151">Zwróć uwagę, że definicja getUserIdCallback analizuje wartość ciągu na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="7152a-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="7152a-152">Jeśli projekt nie rozpoznaje ogólną implementację **metodę GetUserId** metody, konieczne może być pakietu ASP.NET Identity NuGet aktualizacji do wersji 2.1</span><span class="sxs-lookup"><span data-stu-id="7152a-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="7152a-153">Wprowadzono wiele zmian dla klasy infrastruktury używane przez program ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="7152a-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="7152a-154">Podczas kompilowania projektu można zauważyć wiele błędów.</span><span class="sxs-lookup"><span data-stu-id="7152a-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="7152a-155">Na szczęście pozostałe błędy są podobne.</span><span class="sxs-lookup"><span data-stu-id="7152a-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="7152a-156">Klasa tożsamości oczekuje liczby całkowitej dla klucza, ale kontrolera (lub formularza sieci Web) jest przekazanie wartości ciągu.</span><span class="sxs-lookup"><span data-stu-id="7152a-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="7152a-157">W każdym przypadku należy przekonwertować ciąg i liczby całkowitej przez wywołanie metody **metodę GetUserId&lt;int&gt;**.</span><span class="sxs-lookup"><span data-stu-id="7152a-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="7152a-158">Możesz skorzystać z listy błędów z kompilacji lub wykonaj poniższe zmiany.</span><span class="sxs-lookup"><span data-stu-id="7152a-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="7152a-159">Pozostałe zmiany są zależne od typu projektu tworzenia i aktualizacji, które zainstalowano w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7152a-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="7152a-160">Można przejść bezpośrednio do odpowiedniej sekcji za pomocą poniższych łączy</span><span class="sxs-lookup"><span data-stu-id="7152a-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="7152a-161">Dla platformy MVC z Update 2 można zmienić elementu AccountController przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="7152a-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="7152a-162">Dla platformy MVC z aktualizacją Update 3 Zmień elementu AccountController oraz ManageController przekazywanie typu klucza</span><span class="sxs-lookup"><span data-stu-id="7152a-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="7152a-163">Dla formularzy sieci Web z Update 2 Zmień konto strony do przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="7152a-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="7152a-164">Dla formularzy sieci Web z aktualizacją Update 3 Zmień konto strony do przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="7152a-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="7152a-165">Dla platformy MVC z Update 2 można zmienić elementu AccountController przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="7152a-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="7152a-166">Otwórz plik AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="7152a-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="7152a-167">Musisz zmienić z następujących metod.</span><span class="sxs-lookup"><span data-stu-id="7152a-167">You need to change the following methods.</span></span>

<span data-ttu-id="7152a-168">**ConfirmEmail** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="7152a-169">**Usuń skojarzenie** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="7152a-170">**Manage(ManageUserViewModel)** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="7152a-171">**LinkLoginCallback** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="7152a-172">**RemoveAccountList** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="7152a-173">**HasPassword** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="7152a-174">Możesz teraz [uruchomić aplikację](#run) i zarejestrować nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7152a-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="7152a-175">Dla platformy MVC z aktualizacją Update 3 Zmień elementu AccountController oraz ManageController przekazywanie typu klucza</span><span class="sxs-lookup"><span data-stu-id="7152a-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="7152a-176">Otwórz plik AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="7152a-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="7152a-177">Musisz zmienić następującą metodę.</span><span class="sxs-lookup"><span data-stu-id="7152a-177">You need to change the following method.</span></span>

<span data-ttu-id="7152a-178">**ConfirmEmail** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="7152a-179">**SendCode** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="7152a-180">Otwórz plik ManageController.cs.</span><span class="sxs-lookup"><span data-stu-id="7152a-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="7152a-181">Musisz zmienić z następujących metod.</span><span class="sxs-lookup"><span data-stu-id="7152a-181">You need to change the following methods.</span></span>

<span data-ttu-id="7152a-182">**Indeks** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="7152a-183">**RemoveLogin** metody</span><span class="sxs-lookup"><span data-stu-id="7152a-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="7152a-184">**AddPhoneNumber** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="7152a-185">**EnableTwoFactorAuthentication** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="7152a-186">**DisableTwoFactorAuthentication** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="7152a-187">**VerifyPhoneNumber** metody</span><span class="sxs-lookup"><span data-stu-id="7152a-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="7152a-188">**RemovePhoneNumber** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="7152a-189">**Element ChangePassword** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="7152a-190">**UstawianieHasła** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="7152a-191">**ManageLogins** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="7152a-192">**LinkLoginCallback** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="7152a-193">**HasPassword** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="7152a-194">**HasPhoneNumber** — metoda</span><span class="sxs-lookup"><span data-stu-id="7152a-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="7152a-195">Możesz teraz [uruchomić aplikację](#run) i zarejestrować nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7152a-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="7152a-196">Dla formularzy sieci Web z Update 2 Zmień konto strony do przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="7152a-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="7152a-197">Formularze sieci Web z Update 2 należy zmienić następujące strony.</span><span class="sxs-lookup"><span data-stu-id="7152a-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="7152a-198">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="7152a-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="7152a-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="7152a-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="7152a-200">**Manage.aspx.CS**</span><span class="sxs-lookup"><span data-stu-id="7152a-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="7152a-201">Możesz teraz [uruchomić aplikację](#run) i zarejestrować nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7152a-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="7152a-202">Dla formularzy sieci Web z aktualizacją Update 3 Zmień konto strony do przekazania typu klucza</span><span class="sxs-lookup"><span data-stu-id="7152a-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="7152a-203">W przypadku formularzy sieci Web z aktualizacją Update 3 musisz zmienić następujących stron.</span><span class="sxs-lookup"><span data-stu-id="7152a-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="7152a-204">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="7152a-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="7152a-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="7152a-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="7152a-206">**Manage.aspx.CS**</span><span class="sxs-lookup"><span data-stu-id="7152a-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="7152a-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="7152a-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="7152a-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="7152a-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="7152a-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="7152a-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="7152a-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="7152a-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="7152a-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="7152a-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="7152a-212">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="7152a-212">Run application</span></span>

<span data-ttu-id="7152a-213">Zakończono wszystkie wymagane zmiany domyślnego szablonu aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7152a-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="7152a-214">Uruchom aplikację i zarejestrować nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7152a-214">Run the application and register a new user.</span></span> <span data-ttu-id="7152a-215">Po zarejestrowaniu użytkownika, można zauważyć, że tabela AspNetUsers zawiera identyfikator kolumny, która jest liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="7152a-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![nowy klucz podstawowy](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="7152a-217">Jeśli wcześniej utworzono ASP.NET Identity tabel za pomocą innego klucza podstawowego, należy wprowadzić kilka dodatkowych zmian.</span><span class="sxs-lookup"><span data-stu-id="7152a-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="7152a-218">Jeśli to możliwe można usunąć istniejącą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="7152a-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="7152a-219">Bazy danych zostanie ponownie utworzony z poprawny projekt, po uruchomieniu aplikacji sieci web i dodać nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="7152a-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="7152a-220">Jeśli usunięcia nie jest możliwe, uruchom migracje code first, aby zmienić tabel.</span><span class="sxs-lookup"><span data-stu-id="7152a-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="7152a-221">Jednakże nowy klucz podstawowy całkowitą nie zostanie ustawiona jako właściwość SQL IDENTITY w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7152a-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="7152a-222">W kolumnie Identyfikator musi ręcznie Ustaw jako tożsamości.</span><span class="sxs-lookup"><span data-stu-id="7152a-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="7152a-223">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="7152a-223">Other resources</span></span>

- [<span data-ttu-id="7152a-224">Omówienie niestandardowych dostawców magazynu dla produktu ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="7152a-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="7152a-225">Migrowanie istniejącej witryny internetowej z członkostwa SQL do produktu ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="7152a-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="7152a-226">Migrowanie danych uniwersalnych dostawcy członkostwa i profilów użytkownika dla tożsamości ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7152a-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="7152a-227">[Przykładowa aplikacja](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) z zmienione klucza podstawowego</span><span class="sxs-lookup"><span data-stu-id="7152a-227">[Sample application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) with changed primary key</span></span>
