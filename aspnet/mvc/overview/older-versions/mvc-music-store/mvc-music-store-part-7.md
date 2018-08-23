---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Część 7: Członkostwo i autoryzacja | Dokumentacja firmy Microsoft'
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 7 obejmuje członkostwo i autoryzacja.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: 44205f0ef59e00ad9fb1c45fdc0ba8934b5804cc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753104"
---
<a name="part-7-membership-and-authorization"></a>Część 7: Członkostwo i autoryzacja
====================
przez [Galloway'em Jon](https://github.com/jongalloway)

> MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.  
>   
> MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.  
>   
> W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 7 obejmuje członkostwo i autoryzacja.


Nasze kontroler Menedżera Store jest obecnie dostępna dla każdego, kto odwiedzenie naszej witryny. Zmieńmy ten element, aby ograniczyć uprawnienia do administratorów witryny.

## <a name="adding-the-accountcontroller-and-views"></a>Dodawanie elementu AccountController i widoków

Jedną różnicą między pełnym szablonem znajdującym się aplikacja sieci Web programu ASP.NET MVC 3 i szablonu platformy ASP.NET MVC 3 pusta aplikacja sieci Web jest, że pusty szablon nie zawiera konta kontrolera. Dodamy kontroler kont przez skopiowanie kilka plików utworzenie nowej aplikacji platformy ASP.NET MVC z pełnym szablonem znajdującym się aplikacja sieci Web programu ASP.NET MVC 3.

Tworzenie nowej aplikacji platformy ASP.NET MVC przy użyciu pełnej szablonu aplikacji sieci Web programu ASP.NET MVC 3 i skopiuj następujące pliki w tych samych katalogach w projekcie:

1. Skopiuj AccountController.cs w katalogu kontrolerów
2. Skopiuj AccountModels w katalogu modeli
3. Tworzenie katalogu kont w katalogu Views i skopiuj wszystkie cztery widoki w

Zmień przestrzeń nazw dla klasy kontrolera i Model, aby ich zaczynają się od MvcMusicStore. Klasa elementu AccountController powinny używać przestrzeni nazw MvcMusicStore.Controllers i klasy AccountModels powinny używać przestrzeni nazw MvcMusicStore.Models.

*Uwaga: Te pliki są również dostępne do pobrania MvcMusicStore Assets.zip, z którego możemy skopiowane pliki projektu naszej witryny na początku tego samouczka. Pliki członkostwa znajdują się w katalogu kodu.*

Zaktualizowane rozwiązanie powinno wyglądać następująco:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Dodawanie użytkownika administracyjnego z lokacją konfiguracja platformy ASP.NET

Zanim wymagamy autoryzacji w naszej witryny sieci Web, musimy utworzyć użytkownika z dostępem. Najprostszym sposobem utworzenia użytkownika jest użyć wbudowanego witryny sieci Web Konfiguracja platformy ASP.NET.

Uruchom witrynę sieci Web Konfiguracja platformy ASP.NET, klikając następujące ikony w Eksploratorze rozwiązań.

![](mvc-music-store-part-7/_static/image2.png)

Spowoduje to uruchomienie konfiguracji witryny sieci Web. Kliknij kartę Zabezpieczenia na ekranie głównym, a następnie kliknij link "Włącz role" w środku ekranu.

![](mvc-music-store-part-7/_static/image3.png)

Kliknij link "Utwórz i Zarządzaj rolami".

![](mvc-music-store-part-7/_static/image4.png)

Wpisz "Administrator" jako nazwę roli, a następnie naciśnij przycisk Dodaj rolę.

![](mvc-music-store-part-7/_static/image5.png)

Kliknij przycisk Wstecz, a następnie kliknij łącze Utwórz użytkownika po lewej stronie.

![](mvc-music-store-part-7/_static/image6.png)

Wypełnij pola informacji po lewej stronie, korzystając z poniższych informacji:

| **Pole** | **Wartość** |
| --- | --- |
| **Nazwa użytkownika** | Administrator |
| **Hasło** | / password123! |
| **Potwierdź hasło** | / password123! |
| **Wiadomości e-mail** | (dowolny adres e-mail będzie działać) |
| **Pytanie zabezpieczające** | (dowolną) |
| **Odpowiedź zabezpieczeń** | (dowolną) |

*Uwaga: Oczywiście można użyć z dowolnym hasłem, które Twoim zdaniem. Domyślne ustawienia zabezpieczeń haseł wymagają hasła 7 znaków, która zawiera jeden znak inny niż alfanumeryczny.*

Wybierz rolę administratora dla tego użytkownika, a następnie kliknij przycisk Utwórz użytkownika.

![](mvc-music-store-part-7/_static/image7.png)

W tym momencie powinien zostać wyświetlony komunikat informujący, że użytkownik został pomyślnie utworzony.

![](mvc-music-store-part-7/_static/image8.png)

Możesz teraz zamknąć okna przeglądarki.

## <a name="role-based-authorization"></a>Autoryzacja oparta na rolach

Teraz możemy ograniczyć dostęp do StoreManagerController przy użyciu atrybutu [Authorize], określając, czy użytkownik musi być członkiem roli administratora do dostępu do żadnych akcji kontrolera w klasie.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Uwaga: Atrybut [Authorize] można umieścić w metodach określonej akcji, a także na poziomie klasy kontrolera.*

Teraz Przechodzenie do /StoreManager Wyświetla okno dialogowe logowania:

![](mvc-music-store-part-7/_static/image9.png)

Po zalogowaniu się za pomocą naszego nowego konta administratora, jesteśmy w stanie przejść do ekranu edycja albumu w formie przed.

> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-6.md)
> [dalej](mvc-music-store-part-8.md)
