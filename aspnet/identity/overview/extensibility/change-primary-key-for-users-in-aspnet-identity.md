---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Zmiana klucza podstawowego dla użytkowników w produkcie ASP.NET Identity | Dokumentacja firmy Microsoft
author: tfitzmac
description: W programie Visual Studio 2013 domyślnej aplikacji sieci web używa wartość ciągu dla klucza dla kont użytkowników. ASP.NET Identity umożliwia zmianę typu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: ''
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 20e6b86f50a6ea62f188ae592e0b302c7ef77177
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373331"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="8e462-104">Zmiana klucza podstawowego dla użytkowników w produkcie ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="8e462-104">Change Primary Key for Users in ASP.NET Identity</span></span>
====================
<span data-ttu-id="8e462-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8e462-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8e462-106">W programie Visual Studio 2013 domyślnej aplikacji sieci web używa wartość ciągu dla klucza dla kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="8e462-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="8e462-107">ASP.NET Identity umożliwia zmianę typu klucza, aby spełnić wymagania dotyczące danych.</span><span class="sxs-lookup"><span data-stu-id="8e462-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="8e462-108">Na przykład można zmienić typ klucza z ciągu na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="8e462-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="8e462-109">W tym temacie pokazano, jak zacząć domyślną aplikację sieci web i zmienić klucz konta użytkownika na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="8e462-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="8e462-110">Te same modyfikacje można użyć do wdrożenia dowolnego typu klucza w projekcie.</span><span class="sxs-lookup"><span data-stu-id="8e462-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="8e462-111">Pokazuje sposób wprowadzania tych zmian w domyślnej aplikacji sieci web, ale można zastosować zmiany podobne do niestandardowych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8e462-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="8e462-112">Pokazuje zmiany wymagane podczas pracy z MVC i formularzy sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8e462-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="8e462-113">Wersje oprogramowania używanego w tym samouczku</span><span class="sxs-lookup"><span data-stu-id="8e462-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="8e462-114">Visual Studio 2013 z aktualizacją Update 2 (lub nowszym)</span><span class="sxs-lookup"><span data-stu-id="8e462-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="8e462-115">ASP.NET Identity 2.1 lub nowszej</span><span class="sxs-lookup"><span data-stu-id="8e462-115">ASP.NET Identity 2.1 or later</span></span>


<span data-ttu-id="8e462-116">Aby wykonać kroki opisane w tym samouczku, konieczne jest posiadanie programu Visual Studio 2013 Update 2 (lub nowszego) i aplikacji sieci web utworzone za pomocą szablonu aplikacji sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8e462-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="8e462-117">Szablon zmienione w wersji Update 3.</span><span class="sxs-lookup"><span data-stu-id="8e462-117">The template changed in Update 3.</span></span> <span data-ttu-id="8e462-118">W tym temacie pokazano, jak zmienić szablon w aktualizacji 2 i Update 3.</span><span class="sxs-lookup"><span data-stu-id="8e462-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="8e462-119">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="8e462-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="8e462-120">Zmień typ klucza w tożsamości klasy użytkownika</span><span class="sxs-lookup"><span data-stu-id="8e462-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="8e462-121">Dodaj niestandardowe klasy tożsamości, które używają typu klucza</span><span class="sxs-lookup"><span data-stu-id="8e462-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="8e462-122">Zmienianie menedżera użytkownika oraz klasy kontekstu do użycia typ klucza</span><span class="sxs-lookup"><span data-stu-id="8e462-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="8e462-123">Zmień konfigurację uruchamiania, aby używać kluczy typu</span><span class="sxs-lookup"><span data-stu-id="8e462-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="8e462-124">Dla platformy MVC z aktualizacją Update 2 można zmienić elementu AccountController przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="8e462-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="8e462-125">Dla platformy MVC z aktualizacją Update 3 Zmień elementu AccountController i ManageController, aby przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="8e462-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="8e462-126">W przypadku formularzy sieci Web z aktualizacją Update 2 zmienić konto strony do przekazania z typem klucza</span><span class="sxs-lookup"><span data-stu-id="8e462-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="8e462-127">W przypadku formularzy sieci Web z aktualizacją Update 3 zmienić konto strony do przekazania z typem klucza</span><span class="sxs-lookup"><span data-stu-id="8e462-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="8e462-128">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="8e462-128">Run application</span></span>](#run)
- [<span data-ttu-id="8e462-129">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="8e462-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="8e462-130">Zmień typ klucza w tożsamości klasy użytkownika</span><span class="sxs-lookup"><span data-stu-id="8e462-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="8e462-131">W projekcie, utworzone na podstawie szablonu aplikacji sieci Web ASP.NET należy określić, że klasy ApplicationUser używa całkowitą dla klucza dla kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="8e462-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="8e462-132">W IdentityModels.cs, zmień klasy ApplicationUser odziedziczone IdentityUser, który ma typ **int** dla parametru generycznego TKey.</span><span class="sxs-lookup"><span data-stu-id="8e462-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="8e462-133">Możesz też przekazać nazwy trzy niestandardowe klasy, która nie zostało jeszcze zaimplementowane.</span><span class="sxs-lookup"><span data-stu-id="8e462-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="8e462-134">Zmieniono typ klucza, ale domyślnie pozostałe części aplikacji nadal przyjęto założenie, że klucz jest ciąg.</span><span class="sxs-lookup"><span data-stu-id="8e462-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="8e462-135">Należy jawnie wskazać typ klucza w kodzie, który przyjmuje ciąg.</span><span class="sxs-lookup"><span data-stu-id="8e462-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="8e462-136">W **ApplicationUser** klasy, zmienić **GenerateUserIdentityAsync** metodę, aby uwzględnić int, jak pokazano w poniższym kodzie wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="8e462-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="8e462-137">Ta zmiana nie jest konieczne w przypadku projektów formularzy sieci Web za pomocą szablonu aktualizacji 3.</span><span class="sxs-lookup"><span data-stu-id="8e462-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="8e462-138">Dodaj niestandardowe klasy tożsamości, które używają typu klucza</span><span class="sxs-lookup"><span data-stu-id="8e462-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="8e462-139">Inne klasy tożsamości, takich jak IdentityUserRole IdentityUserClaim, IdentityUserLogin, IdentityRole, magazynie UserStore, elemencie RoleStore, nadal są konfigurowane do użycia klucza typu ciąg.</span><span class="sxs-lookup"><span data-stu-id="8e462-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="8e462-140">Utwórz nowe wersje tych klas, które określają całkowitą dla klucza.</span><span class="sxs-lookup"><span data-stu-id="8e462-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="8e462-141">Nie należy podać wiele kod implementacji w ramach tych zajęć, przede wszystkim właśnie przeprowadzasz int jako klucz.</span><span class="sxs-lookup"><span data-stu-id="8e462-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="8e462-142">Dodaj następujące klasy do pliku IdentityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="8e462-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="8e462-143">Zmienianie menedżera użytkownika oraz klasy kontekstu do użycia typ klucza</span><span class="sxs-lookup"><span data-stu-id="8e462-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="8e462-144">W IdentityModels.cs, należy zmienić definicję **ApplicationDbContext** nowej klasy dostosowane klasy i **int** klucza, jak pokazano na wyróżniony kod.</span><span class="sxs-lookup"><span data-stu-id="8e462-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="8e462-145">Parametr ThrowIfV1Schema nie jest już prawidłowy w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="8e462-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="8e462-146">Zmień konstruktora, aby nie mogła przekazywać wartość ThrowIfV1Schema.</span><span class="sxs-lookup"><span data-stu-id="8e462-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="8e462-147">Otwórz IdentityConfig.cs i zmień **ApplicationUserManger** klasy nowy użytkownik przechowywania klasę utrwalanie danych i **int** dla klucza.</span><span class="sxs-lookup"><span data-stu-id="8e462-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="8e462-148">W szablonie Update 3 możesz zmienić klasy ApplicationSignInManager.</span><span class="sxs-lookup"><span data-stu-id="8e462-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="8e462-149">Zmień konfigurację uruchamiania, aby używać kluczy typu</span><span class="sxs-lookup"><span data-stu-id="8e462-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="8e462-150">W Startup.Auth.cs Zastąp kod element OnValidateIdentity, jak wyróżniono poniżej.</span><span class="sxs-lookup"><span data-stu-id="8e462-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="8e462-151">Należy zauważyć, że definicja getUserIdCallback analizuje wartości ciągu na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="8e462-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="8e462-152">Jeśli projekt nie może rozpoznać ogólną implementację **metodę GetUserId** metody, konieczne może być aktualizacja pakietu NuGet tożsamości ASP.NET do wersji 2.1</span><span class="sxs-lookup"><span data-stu-id="8e462-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="8e462-153">Wprowadzono wiele zmian do klas infrastruktury posługują się produktu ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="8e462-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="8e462-154">Jeśli spróbujesz kompilowania projektu, można zauważyć wiele błędów.</span><span class="sxs-lookup"><span data-stu-id="8e462-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="8e462-155">Na szczęście pozostałe błędy są podobne.</span><span class="sxs-lookup"><span data-stu-id="8e462-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="8e462-156">Klasa tożsamości oczekuje całkowitą dla klucza, ale kontroler (lub formularza sieci Web) przekazuje wartość ciągu.</span><span class="sxs-lookup"><span data-stu-id="8e462-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="8e462-157">W obu przypadkach należy przekonwertować ciąg i liczba całkowita, wywołując **metodę GetUserId&lt;int&gt;**.</span><span class="sxs-lookup"><span data-stu-id="8e462-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="8e462-158">Możesz pracować za pośrednictwem listy błędów z kompilacji lub wykonaj poniższe zmiany.</span><span class="sxs-lookup"><span data-stu-id="8e462-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="8e462-159">Pozostałe zmiany są zależne od typu projektu, tworzenia i aktualizacji, które zostały zainstalowane w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8e462-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="8e462-160">Możesz przejść bezpośrednio do odpowiedniej sekcji za pomocą poniższych łączy</span><span class="sxs-lookup"><span data-stu-id="8e462-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="8e462-161">Dla platformy MVC z aktualizacją Update 2 można zmienić elementu AccountController przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="8e462-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="8e462-162">Dla platformy MVC z aktualizacją Update 3 Zmień elementu AccountController i ManageController, aby przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="8e462-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="8e462-163">W przypadku formularzy sieci Web z aktualizacją Update 2 zmienić konto strony do przekazania z typem klucza</span><span class="sxs-lookup"><span data-stu-id="8e462-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="8e462-164">W przypadku formularzy sieci Web z aktualizacją Update 3 zmienić konto strony do przekazania z typem klucza</span><span class="sxs-lookup"><span data-stu-id="8e462-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="8e462-165">Dla platformy MVC z aktualizacją Update 2 można zmienić elementu AccountController przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="8e462-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="8e462-166">Otwórz plik AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="8e462-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="8e462-167">Musisz zmienić następujących metod.</span><span class="sxs-lookup"><span data-stu-id="8e462-167">You need to change the following methods.</span></span>

<span data-ttu-id="8e462-168">**ConfirmEmail** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="8e462-169">**Usuń skojarzenie** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="8e462-170">**Manage(ManageUserViewModel)** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="8e462-171">**LinkLoginCallback** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="8e462-172">**RemoveAccountList** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="8e462-173">**HasPassword** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="8e462-174">Możesz teraz [uruchomić aplikację](#run) i rejestrowanie nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8e462-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="8e462-175">Dla platformy MVC z aktualizacją Update 3 Zmień elementu AccountController i ManageController, aby przekazać typ klucza</span><span class="sxs-lookup"><span data-stu-id="8e462-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="8e462-176">Otwórz plik AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="8e462-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="8e462-177">Musisz zmienić następującą metodę.</span><span class="sxs-lookup"><span data-stu-id="8e462-177">You need to change the following method.</span></span>

<span data-ttu-id="8e462-178">**ConfirmEmail** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="8e462-179">**SendCode** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="8e462-180">Otwórz plik ManageController.cs.</span><span class="sxs-lookup"><span data-stu-id="8e462-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="8e462-181">Musisz zmienić następujących metod.</span><span class="sxs-lookup"><span data-stu-id="8e462-181">You need to change the following methods.</span></span>

<span data-ttu-id="8e462-182">**Indeks** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="8e462-183">**RemoveLogin** metody</span><span class="sxs-lookup"><span data-stu-id="8e462-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="8e462-184">**AddPhoneNumber** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="8e462-185">**EnableTwoFactorAuthentication** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="8e462-186">**DisableTwoFactorAuthentication** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="8e462-187">**VerifyPhoneNumber** metody</span><span class="sxs-lookup"><span data-stu-id="8e462-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="8e462-188">**RemovePhoneNumber** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="8e462-189">**ChangePassword** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="8e462-190">**SetPassword** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="8e462-191">**ManageLogins** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="8e462-192">**LinkLoginCallback** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="8e462-193">**HasPassword** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="8e462-194">**HasPhoneNumber** — metoda</span><span class="sxs-lookup"><span data-stu-id="8e462-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="8e462-195">Możesz teraz [uruchomić aplikację](#run) i rejestrowanie nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8e462-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="8e462-196">W przypadku formularzy sieci Web z aktualizacją Update 2 zmienić konto strony do przekazania z typem klucza</span><span class="sxs-lookup"><span data-stu-id="8e462-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="8e462-197">Formularze sieci Web z aktualizacją Update 2 należy zmienić na następujących stronach.</span><span class="sxs-lookup"><span data-stu-id="8e462-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="8e462-198">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="8e462-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="8e462-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="8e462-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="8e462-200">**Manage.aspx.CS**</span><span class="sxs-lookup"><span data-stu-id="8e462-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="8e462-201">Możesz teraz [uruchomić aplikację](#run) i rejestrowanie nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8e462-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="8e462-202">W przypadku formularzy sieci Web z aktualizacją Update 3 zmienić konto strony do przekazania z typem klucza</span><span class="sxs-lookup"><span data-stu-id="8e462-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="8e462-203">Formularze sieci Web z aktualizacją Update 3 należy zmienić na następujących stronach.</span><span class="sxs-lookup"><span data-stu-id="8e462-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="8e462-204">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="8e462-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="8e462-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="8e462-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="8e462-206">**Manage.aspx.CS**</span><span class="sxs-lookup"><span data-stu-id="8e462-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="8e462-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="8e462-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="8e462-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="8e462-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="8e462-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="8e462-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="8e462-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="8e462-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="8e462-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="8e462-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="8e462-212">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="8e462-212">Run application</span></span>

<span data-ttu-id="8e462-213">Zakończono wszystkie zmiany wymagane w celu domyślnego szablonu aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8e462-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="8e462-214">Uruchom aplikację i zarejestrować nowego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8e462-214">Run the application and register a new user.</span></span> <span data-ttu-id="8e462-215">Po zarejestrowaniu użytkownika, zauważysz, że tabela AspNetUsers zawiera kolumny identyfikatora, który jest liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="8e462-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![nowy klucz podstawowy](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="8e462-217">Jeśli wcześniej utworzono produktu ASP.NET Identity tabel przy użyciu innego klucza podstawowego, należy wprowadzić pewne dodatkowe zmiany.</span><span class="sxs-lookup"><span data-stu-id="8e462-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="8e462-218">Jeśli to możliwe można usunąć istniejącej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8e462-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="8e462-219">Po uruchomieniu aplikacji sieci web i dodać nowego użytkownika będzie ponownie utworzony przy użyciu poprawny projekt bazy danych.</span><span class="sxs-lookup"><span data-stu-id="8e462-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="8e462-220">Jeśli usuwanie nie jest możliwe, uruchom migracje code first, aby zmienić w tabelach.</span><span class="sxs-lookup"><span data-stu-id="8e462-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="8e462-221">Jednakże nowy klucz podstawowy dla liczby całkowitej nie zostanie ustawiona jako właściwość SQL IDENTITY w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="8e462-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="8e462-222">Jako tożsamość, należy ręcznie ustawić kolumny identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="8e462-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="8e462-223">Inne zasoby</span><span class="sxs-lookup"><span data-stu-id="8e462-223">Other resources</span></span>

- [<span data-ttu-id="8e462-224">Omówienie niestandardowych dostawców magazynu dla produktu ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="8e462-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="8e462-225">Migrowanie istniejącej witryny internetowej z członkostwa SQL do produktu ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="8e462-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="8e462-226">Migrowanie danych uniwersalnego dostawcy dotyczących członkostwa i profilów użytkowników do produktu ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="8e462-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="8e462-227">[Przykładowa aplikacja](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) przy użyciu zmienionego klucz podstawowy</span><span class="sxs-lookup"><span data-stu-id="8e462-227">[Sample application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) with changed primary key</span></span>
