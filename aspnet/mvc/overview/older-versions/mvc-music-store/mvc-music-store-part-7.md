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
ms.openlocfilehash: 064f2d6eca087fae8c796d1dde78d5079d3803ca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="part-7-membership-and-authorization"></a>Część 7: Członkostwa i autoryzacji
====================
przez [Galloway Jan](https://github.com/jongalloway)

> Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.  
>   
> Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.  
>   
> Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 7 obejmuje członkostwa i autoryzacji.


Menedżer magazynu kontrolera jest obecnie dostępny do każdy odwiedzający witryny firmy Microsoft. Zmieńmy tutaj, aby ograniczyć uprawnienia do witryny administratorów.

## <a name="adding-the-accountcontroller-and-views"></a>Dodawanie elementu AccountController i widoków

Jeden różnica między pełną szablonu aplikacji sieci Web programu ASP.NET MVC 3 i ASP.NET MVC 3 pusty szablonu aplikacji sieci Web jest, że pustego szablonu nie zawiera konta kontrolera. Dodamy konto kontrolera przez skopiowanie kilka plików z nowej aplikacji ASP.NET MVC utworzone na podstawie pełnego szablonu aplikacji sieci Web programu ASP.NET MVC 3.

Tworzenie nowej aplikacji ASP.NET MVC przy użyciu pełnego szablonu aplikacji sieci Web programu ASP.NET MVC 3 i skopiuj następujące pliki do tych samych katalogach w naszym projektu:

1. Skopiuj AccountController.cs w katalogu kontrolerów
2. Skopiuj AccountModels w katalogu modeli
3. Utwórz katalog konta w katalogu widoków i skopiuj wszystkie cztery widoki w

Zmień przestrzeń nazw dla klasy kontrolera i Model, aby ich rozpoczynać się od MvcMusicStore. Klasa elementu AccountController należy używać MvcMusicStore.Controllers przestrzeni nazw i klasy AccountModels powinny używać MvcMusicStore.Models przestrzeni nazw.

*Uwaga: Te pliki są również dostępne w pobierania MvcMusicStore Assets.zip, z którego możemy skopiowane lokacji plików projektu na początku tego samouczka. Pliki członkostwa znajdują się w katalogu kodu.*

Zaktualizowane rozwiązanie powinno wyglądać następująco:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Dodawanie użytkownika administracyjnego z lokacją konfiguracja platformy ASP.NET

Przed wymagamy autoryzacji w naszej witrynie sieci Web, musimy utworzenia użytkownika z dostępem. Najprostszym sposobem, aby utworzyć użytkownika jest wbudowane witrynę sieci Web Konfiguracja platformy ASP.NET.

Uruchamianie witryny sieci Web Konfiguracja platformy ASP.NET, klikając następujące ikony w Eksploratorze rozwiązań.

![](mvc-music-store-part-7/_static/image2.png)

Spowoduje to uruchomienie konfiguracji witryny sieci Web. Kliknij na karcie Zabezpieczenia ekranu głównego, a następnie kliknij łącze "Włącz role" w Centrum ekranu.

![](mvc-music-store-part-7/_static/image3.png)

Kliknij łącze "Utwórz i Zarządzaj rolami".

![](mvc-music-store-part-7/_static/image4.png)

Wprowadź "Administrator" jako nazwę roli, a następnie naciśnij przycisk Dodaj rolę.

![](mvc-music-store-part-7/_static/image5.png)

Kliknij przycisk Wstecz, a następnie kliknięcie łącza tworzenia użytkownika po lewej stronie.

![](mvc-music-store-part-7/_static/image6.png)

Wypełnij pola informacji po lewej stronie, używając następujących informacji:

| **Pole** | **Wartość** |
| --- | --- |
| **Nazwa użytkownika** | Administrator |
| **Hasło** | password123! |
| **Potwierdź hasło** | password123! |
| **Wiadomości e-mail** | (dowolny adres e-mail będzie działać) |
| **Pytanie zabezpieczające** | (co chcesz) |
| **Odpowiedź zabezpieczeń** | (co chcesz) |

*Uwaga: Oczywiście można użyć z dowolnym hasłem, którą chcesz. Ustawienia domyślne hasło zabezpieczeń wymagają podania hasła, 7 znaków, który zawiera jeden znak inny niż alfanumeryczny.*

Wybierz rolę administratora dla tego użytkownika, a następnie kliknij przycisk Utwórz użytkownika.

![](mvc-music-store-part-7/_static/image7.png)

W tym momencie powinien zostać wyświetlony komunikat wskazujący, że użytkownik został pomyślnie utworzony.

![](mvc-music-store-part-7/_static/image8.png)

Teraz można zamknąć okna przeglądarki.

## <a name="role-based-authorization"></a>Autoryzacji opartej na rolach

Teraz możemy ograniczyć dostęp do StoreManagerController za pomocą atrybutu [Authorize], określając, że użytkownik musi być w roli administratora można uzyskać dostępu do żadnych akcji kontrolera w klasie.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Uwaga: Atrybut [Authorize] można umieścić w metodach określonej akcji, a także na poziomie klasy kontrolera.*

Teraz przechodząc do /StoreManager Wyświetla okno dialogowe logowania:

![](mvc-music-store-part-7/_static/image9.png)

Po zalogowaniu się przy użyciu naszego nowego konta administratora, możemy przejść do ekranu Edytuj Album jako przed.

>[!div class="step-by-step"]
[Poprzednie](mvc-music-store-part-6.md)
[dalej](mvc-music-store-part-8.md)
