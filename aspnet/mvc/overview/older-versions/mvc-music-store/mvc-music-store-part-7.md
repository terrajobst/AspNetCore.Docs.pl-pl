---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Część 7: Członkostwo i autoryzacja | Dokumentacja firmy Microsoft'
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 7 obejmuje członkostwo i autoryzacja.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: 41b17315cbe1f6d93001a736bc24bf003df24061
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393173"
---
<a name="part-7-membership-and-authorization"></a><span data-ttu-id="8e61a-104">Część 7: Członkostwo i autoryzacja</span><span class="sxs-lookup"><span data-stu-id="8e61a-104">Part 7: Membership and Authorization</span></span>
====================
<span data-ttu-id="8e61a-105">przez [Galloway'em Jon](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="8e61a-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="8e61a-106">MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.</span><span class="sxs-lookup"><span data-stu-id="8e61a-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="8e61a-107">MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="8e61a-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="8e61a-108">W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="8e61a-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="8e61a-109">Część 7 obejmuje członkostwo i autoryzacja.</span><span class="sxs-lookup"><span data-stu-id="8e61a-109">Part 7 covers Membership and Authorization.</span></span>


<span data-ttu-id="8e61a-110">Nasze kontroler Menedżera Store jest obecnie dostępna dla każdego, kto odwiedzenie naszej witryny.</span><span class="sxs-lookup"><span data-stu-id="8e61a-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="8e61a-111">Zmieńmy ten element, aby ograniczyć uprawnienia do administratorów witryny.</span><span class="sxs-lookup"><span data-stu-id="8e61a-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="8e61a-112">Dodawanie elementu AccountController i widoków</span><span class="sxs-lookup"><span data-stu-id="8e61a-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="8e61a-113">Jedną różnicą między pełnym szablonem znajdującym się aplikacja sieci Web programu ASP.NET MVC 3 i szablonu platformy ASP.NET MVC 3 pusta aplikacja sieci Web jest, że pusty szablon nie zawiera konta kontrolera.</span><span class="sxs-lookup"><span data-stu-id="8e61a-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="8e61a-114">Dodamy kontroler kont przez skopiowanie kilka plików utworzenie nowej aplikacji platformy ASP.NET MVC z pełnym szablonem znajdującym się aplikacja sieci Web programu ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="8e61a-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="8e61a-115">Tworzenie nowej aplikacji platformy ASP.NET MVC przy użyciu pełnej szablonu aplikacji sieci Web programu ASP.NET MVC 3 i skopiuj następujące pliki w tych samych katalogach w projekcie:</span><span class="sxs-lookup"><span data-stu-id="8e61a-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="8e61a-116">Skopiuj AccountController.cs w katalogu kontrolerów</span><span class="sxs-lookup"><span data-stu-id="8e61a-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="8e61a-117">Skopiuj AccountModels w katalogu modeli</span><span class="sxs-lookup"><span data-stu-id="8e61a-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="8e61a-118">Tworzenie katalogu kont w katalogu Views i skopiuj wszystkie cztery widoki w</span><span class="sxs-lookup"><span data-stu-id="8e61a-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="8e61a-119">Zmień przestrzeń nazw dla klasy kontrolera i Model, aby ich zaczynają się od MvcMusicStore.</span><span class="sxs-lookup"><span data-stu-id="8e61a-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="8e61a-120">Klasa elementu AccountController powinny używać przestrzeni nazw MvcMusicStore.Controllers i klasy AccountModels powinny używać przestrzeni nazw MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="8e61a-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="8e61a-121">*Uwaga: Te pliki są również dostępne do pobrania MvcMusicStore Assets.zip, z którego możemy skopiowane pliki projektu naszej witryny na początku tego samouczka. Pliki członkostwa znajdują się w katalogu kodu.*</span><span class="sxs-lookup"><span data-stu-id="8e61a-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="8e61a-122">Zaktualizowane rozwiązanie powinno wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="8e61a-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="8e61a-123">Dodawanie użytkownika administracyjnego z lokacją konfiguracja platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8e61a-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="8e61a-124">Zanim wymagamy autoryzacji w naszej witryny sieci Web, musimy utworzyć użytkownika z dostępem.</span><span class="sxs-lookup"><span data-stu-id="8e61a-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="8e61a-125">Najprostszym sposobem utworzenia użytkownika jest użyć wbudowanego witryny sieci Web Konfiguracja platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8e61a-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="8e61a-126">Uruchom witrynę sieci Web Konfiguracja platformy ASP.NET, klikając następujące ikony w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="8e61a-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="8e61a-127">Spowoduje to uruchomienie konfiguracji witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="8e61a-127">This launches a configuration website.</span></span> <span data-ttu-id="8e61a-128">Kliknij kartę Zabezpieczenia na ekranie głównym, a następnie kliknij link "Włącz role" w środku ekranu.</span><span class="sxs-lookup"><span data-stu-id="8e61a-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="8e61a-129">Kliknij link "Utwórz i Zarządzaj rolami".</span><span class="sxs-lookup"><span data-stu-id="8e61a-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="8e61a-130">Wpisz "Administrator" jako nazwę roli, a następnie naciśnij przycisk Dodaj rolę.</span><span class="sxs-lookup"><span data-stu-id="8e61a-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="8e61a-131">Kliknij przycisk Wstecz, a następnie kliknij łącze Utwórz użytkownika po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="8e61a-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="8e61a-132">Wypełnij pola informacji po lewej stronie, korzystając z poniższych informacji:</span><span class="sxs-lookup"><span data-stu-id="8e61a-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="8e61a-133">**Pole**</span><span class="sxs-lookup"><span data-stu-id="8e61a-133">**Field**</span></span> | <span data-ttu-id="8e61a-134">**Wartość**</span><span class="sxs-lookup"><span data-stu-id="8e61a-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="8e61a-135">**Nazwa użytkownika**</span><span class="sxs-lookup"><span data-stu-id="8e61a-135">**User Name**</span></span> | <span data-ttu-id="8e61a-136">Administrator</span><span class="sxs-lookup"><span data-stu-id="8e61a-136">Administrator</span></span> |
| <span data-ttu-id="8e61a-137">**Hasło**</span><span class="sxs-lookup"><span data-stu-id="8e61a-137">**Password**</span></span> | <span data-ttu-id="8e61a-138">/ password123!</span><span class="sxs-lookup"><span data-stu-id="8e61a-138">password123!</span></span> |
| <span data-ttu-id="8e61a-139">**Potwierdź hasło**</span><span class="sxs-lookup"><span data-stu-id="8e61a-139">**Confirm Password**</span></span> | <span data-ttu-id="8e61a-140">/ password123!</span><span class="sxs-lookup"><span data-stu-id="8e61a-140">password123!</span></span> |
| <span data-ttu-id="8e61a-141">**Wiadomości e-mail**</span><span class="sxs-lookup"><span data-stu-id="8e61a-141">**E-mail**</span></span> | <span data-ttu-id="8e61a-142">(dowolny adres e-mail będzie działać)</span><span class="sxs-lookup"><span data-stu-id="8e61a-142">(any email address will work)</span></span> |
| <span data-ttu-id="8e61a-143">**Pytanie zabezpieczające**</span><span class="sxs-lookup"><span data-stu-id="8e61a-143">**Security Question**</span></span> | <span data-ttu-id="8e61a-144">(dowolną)</span><span class="sxs-lookup"><span data-stu-id="8e61a-144">(whatever you like)</span></span> |
| <span data-ttu-id="8e61a-145">**Odpowiedź zabezpieczeń**</span><span class="sxs-lookup"><span data-stu-id="8e61a-145">**Security Answer**</span></span> | <span data-ttu-id="8e61a-146">(dowolną)</span><span class="sxs-lookup"><span data-stu-id="8e61a-146">(whatever you like)</span></span> |

<span data-ttu-id="8e61a-147">*Uwaga: Oczywiście można użyć z dowolnym hasłem, które Twoim zdaniem. Domyślne ustawienia zabezpieczeń haseł wymagają hasła 7 znaków, która zawiera jeden znak inny niż alfanumeryczny.*</span><span class="sxs-lookup"><span data-stu-id="8e61a-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="8e61a-148">Wybierz rolę administratora dla tego użytkownika, a następnie kliknij przycisk Utwórz użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8e61a-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="8e61a-149">W tym momencie powinien zostać wyświetlony komunikat informujący, że użytkownik został pomyślnie utworzony.</span><span class="sxs-lookup"><span data-stu-id="8e61a-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="8e61a-150">Możesz teraz zamknąć okna przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8e61a-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="8e61a-151">Autoryzacja oparta na rolach</span><span class="sxs-lookup"><span data-stu-id="8e61a-151">Role-based Authorization</span></span>

<span data-ttu-id="8e61a-152">Teraz możemy ograniczyć dostęp do StoreManagerController przy użyciu atrybutu [Authorize], określając, czy użytkownik musi być członkiem roli administratora do dostępu do żadnych akcji kontrolera w klasie.</span><span class="sxs-lookup"><span data-stu-id="8e61a-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="8e61a-153">*Uwaga: Atrybut [Authorize] można umieścić w metodach określonej akcji, a także na poziomie klasy kontrolera.*</span><span class="sxs-lookup"><span data-stu-id="8e61a-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="8e61a-154">Teraz Przechodzenie do /StoreManager Wyświetla okno dialogowe logowania:</span><span class="sxs-lookup"><span data-stu-id="8e61a-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="8e61a-155">Po zalogowaniu się za pomocą naszego nowego konta administratora, jesteśmy w stanie przejść do ekranu edycja albumu w formie przed.</span><span class="sxs-lookup"><span data-stu-id="8e61a-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8e61a-156">[Poprzednie](mvc-music-store-part-6.md)
> [dalej](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="8e61a-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
