---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: "Część 7: Członkostwa i autoryzacji | Dokumentacja firmy Microsoft"
author: jongalloway
description: "Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 7 obejmuje członkostwa i autoryzacji."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: db459de687db862be00a9b59ff5b1b238fa75061
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
<a name="part-7-membership-and-authorization"></a><span data-ttu-id="1da63-104">Część 7: Członkostwa i autoryzacji</span><span class="sxs-lookup"><span data-stu-id="1da63-104">Part 7: Membership and Authorization</span></span>
====================
<span data-ttu-id="1da63-105">przez [Galloway Jan](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="1da63-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="1da63-106">Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1da63-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="1da63-107">Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.</span><span class="sxs-lookup"><span data-stu-id="1da63-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="1da63-108">Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu.</span><span class="sxs-lookup"><span data-stu-id="1da63-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="1da63-109">Część 7 obejmuje członkostwa i autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="1da63-109">Part 7 covers Membership and Authorization.</span></span>


<span data-ttu-id="1da63-110">Menedżer magazynu kontrolera jest obecnie dostępny do każdy odwiedzający witryny firmy Microsoft.</span><span class="sxs-lookup"><span data-stu-id="1da63-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="1da63-111">Zmieńmy tutaj, aby ograniczyć uprawnienia do witryny administratorów.</span><span class="sxs-lookup"><span data-stu-id="1da63-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="1da63-112">Dodawanie elementu AccountController i widoków</span><span class="sxs-lookup"><span data-stu-id="1da63-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="1da63-113">Jeden różnica między pełną szablonu aplikacji sieci Web programu ASP.NET MVC 3 i ASP.NET MVC 3 pusty szablonu aplikacji sieci Web jest, że pustego szablonu nie zawiera konta kontrolera.</span><span class="sxs-lookup"><span data-stu-id="1da63-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="1da63-114">Dodamy konto kontrolera przez skopiowanie kilka plików z nowej aplikacji ASP.NET MVC utworzone na podstawie pełnego szablonu aplikacji sieci Web programu ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="1da63-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="1da63-115">Tworzenie nowej aplikacji ASP.NET MVC przy użyciu pełnego szablonu aplikacji sieci Web programu ASP.NET MVC 3 i skopiuj następujące pliki do tych samych katalogach w naszym projektu:</span><span class="sxs-lookup"><span data-stu-id="1da63-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="1da63-116">Skopiuj AccountController.cs w katalogu kontrolerów</span><span class="sxs-lookup"><span data-stu-id="1da63-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="1da63-117">Skopiuj AccountModels w katalogu modeli</span><span class="sxs-lookup"><span data-stu-id="1da63-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="1da63-118">Utwórz katalog konta w katalogu widoków i skopiuj wszystkie cztery widoki w</span><span class="sxs-lookup"><span data-stu-id="1da63-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="1da63-119">Zmień przestrzeń nazw dla klasy kontrolera i Model, aby ich rozpoczynać się od MvcMusicStore.</span><span class="sxs-lookup"><span data-stu-id="1da63-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="1da63-120">Klasa elementu AccountController należy używać MvcMusicStore.Controllers przestrzeni nazw i klasy AccountModels powinny używać MvcMusicStore.Models przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="1da63-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="1da63-121">*Uwaga: Te pliki są również dostępne w pobierania MvcMusicStore Assets.zip, z którego możemy skopiowane lokacji plików projektu na początku tego samouczka. Pliki członkostwa znajdują się w katalogu kodu.*</span><span class="sxs-lookup"><span data-stu-id="1da63-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="1da63-122">Zaktualizowane rozwiązanie powinno wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="1da63-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="1da63-123">Dodawanie użytkownika administracyjnego z lokacją konfiguracja platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="1da63-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="1da63-124">Przed wymagamy autoryzacji w naszej witrynie sieci Web, musimy utworzenia użytkownika z dostępem.</span><span class="sxs-lookup"><span data-stu-id="1da63-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="1da63-125">Najprostszym sposobem, aby utworzyć użytkownika jest wbudowane witrynę sieci Web Konfiguracja platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1da63-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="1da63-126">Uruchamianie witryny sieci Web Konfiguracja platformy ASP.NET, klikając następujące ikony w Eksploratorze rozwiązań.</span><span class="sxs-lookup"><span data-stu-id="1da63-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="1da63-127">Spowoduje to uruchomienie konfiguracji witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="1da63-127">This launches a configuration website.</span></span> <span data-ttu-id="1da63-128">Kliknij na karcie Zabezpieczenia ekranu głównego, a następnie kliknij łącze "Włącz role" w Centrum ekranu.</span><span class="sxs-lookup"><span data-stu-id="1da63-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="1da63-129">Kliknij łącze "Utwórz i Zarządzaj rolami".</span><span class="sxs-lookup"><span data-stu-id="1da63-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="1da63-130">Wprowadź "Administrator" jako nazwę roli, a następnie naciśnij przycisk Dodaj rolę.</span><span class="sxs-lookup"><span data-stu-id="1da63-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="1da63-131">Kliknij przycisk Wstecz, a następnie kliknięcie łącza tworzenia użytkownika po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="1da63-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="1da63-132">Wypełnij pola informacji po lewej stronie, używając następujących informacji:</span><span class="sxs-lookup"><span data-stu-id="1da63-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="1da63-133">**Pole**</span><span class="sxs-lookup"><span data-stu-id="1da63-133">**Field**</span></span> | <span data-ttu-id="1da63-134">**Wartość**</span><span class="sxs-lookup"><span data-stu-id="1da63-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="1da63-135">**Nazwa użytkownika**</span><span class="sxs-lookup"><span data-stu-id="1da63-135">**User Name**</span></span> | <span data-ttu-id="1da63-136">Administrator</span><span class="sxs-lookup"><span data-stu-id="1da63-136">Administrator</span></span> |
| <span data-ttu-id="1da63-137">**Hasło**</span><span class="sxs-lookup"><span data-stu-id="1da63-137">**Password**</span></span> | <span data-ttu-id="1da63-138">password123!</span><span class="sxs-lookup"><span data-stu-id="1da63-138">password123!</span></span> |
| <span data-ttu-id="1da63-139">**Potwierdź hasło**</span><span class="sxs-lookup"><span data-stu-id="1da63-139">**Confirm Password**</span></span> | <span data-ttu-id="1da63-140">password123!</span><span class="sxs-lookup"><span data-stu-id="1da63-140">password123!</span></span> |
| <span data-ttu-id="1da63-141">**Wiadomości e-mail**</span><span class="sxs-lookup"><span data-stu-id="1da63-141">**E-mail**</span></span> | <span data-ttu-id="1da63-142">(dowolnego adresu e-mail będzie działać)</span><span class="sxs-lookup"><span data-stu-id="1da63-142">(any email address will work)</span></span> |
| <span data-ttu-id="1da63-143">**Pytanie zabezpieczające**</span><span class="sxs-lookup"><span data-stu-id="1da63-143">**Security Question**</span></span> | <span data-ttu-id="1da63-144">(co chcesz)</span><span class="sxs-lookup"><span data-stu-id="1da63-144">(whatever you like)</span></span> |
| <span data-ttu-id="1da63-145">**Odpowiedź zabezpieczeń**</span><span class="sxs-lookup"><span data-stu-id="1da63-145">**Security Answer**</span></span> | <span data-ttu-id="1da63-146">(co chcesz)</span><span class="sxs-lookup"><span data-stu-id="1da63-146">(whatever you like)</span></span> |

<span data-ttu-id="1da63-147">*Uwaga: Oczywiście można użyć z dowolnym hasłem, którą chcesz. Ustawienia domyślne hasło zabezpieczeń wymagają podania hasła, 7 znaków, który zawiera jeden znak inny niż alfanumeryczny.*</span><span class="sxs-lookup"><span data-stu-id="1da63-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="1da63-148">Wybierz rolę administratora dla tego użytkownika, a następnie kliknij przycisk Utwórz użytkownika.</span><span class="sxs-lookup"><span data-stu-id="1da63-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="1da63-149">W tym momencie powinien zostać wyświetlony komunikat wskazujący, że użytkownik został pomyślnie utworzony.</span><span class="sxs-lookup"><span data-stu-id="1da63-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="1da63-150">Teraz można zamknąć okna przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="1da63-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="1da63-151">Autoryzacji opartej na rolach</span><span class="sxs-lookup"><span data-stu-id="1da63-151">Role-based Authorization</span></span>

<span data-ttu-id="1da63-152">Teraz możemy ograniczyć dostęp do StoreManagerController za pomocą atrybutu [Authorize], określając, że użytkownik musi być w roli administratora można uzyskać dostępu do żadnych akcji kontrolera w klasie.</span><span class="sxs-lookup"><span data-stu-id="1da63-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="1da63-153">*Uwaga: Atrybut [Authorize] można umieścić w metodach określonej akcji, a także na poziomie klasy kontrolera.*</span><span class="sxs-lookup"><span data-stu-id="1da63-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="1da63-154">Teraz przechodząc do /StoreManager Wyświetla okno dialogowe logowania:</span><span class="sxs-lookup"><span data-stu-id="1da63-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="1da63-155">Po zalogowaniu się przy użyciu naszego nowego konta administratora, możemy przejść do ekranu Edytuj Album jako przed.</span><span class="sxs-lookup"><span data-stu-id="1da63-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1da63-156">[Poprzednie](mvc-music-store-part-6.md)
[dalej](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="1da63-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
