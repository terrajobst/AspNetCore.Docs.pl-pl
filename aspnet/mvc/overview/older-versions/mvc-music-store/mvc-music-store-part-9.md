---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Część 9: Rejestracja i wyewidencjonowania | Dokumentacja firmy Microsoft'
author: jongalloway
description: Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 9 obejmuje rejestracji i płatności.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: e7e83b70f2508b6dfc0c078b992747a76e4d0ff2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870118"
---
<a name="part-9-registration-and-checkout"></a>Część 9: Rejestracja i wyewidencjonowania
====================
przez [Galloway Jan](https://github.com/jongalloway)

> Magazyn utworów muzycznych MVC jest samouczek aplikacji, które wprowadzono i opisano krok po kroku, jak używać do tworzenia aplikacji sieci web platformy ASP.NET MVC i Visual Studio.  
>   
> Magazyn utworów muzycznych MVC jest implementacja magazynu lekkie próbki, co sprzedaje albumów muzycznych w trybie online i implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.  
>   
> Ten samouczek serii zawiera szczegóły dotyczące wszystkich kroków tworzenia przykładowej aplikacji ASP.NET MVC utworów muzycznych magazynu. Część 9 obejmuje rejestracji i płatności.


W tej sekcji możemy zostanie utworzony CheckoutController, który będzie zbierać dana osoba adresu i informacji o płatności. Firma Microsoft będzie wymagać od użytkowników do Zarejestruj się w naszej witrynie przed wyewidencjonowania, aby ten kontroler wymaga autoryzacji.

Użytkownicy będą przejdź do realizacji z ich koszyk przez kliknięcie przycisku "Wyewidencjonowania".

![](mvc-music-store-part-9/_static/image1.jpg)

Jeśli użytkownik nie jest zalogowany, zostanie wyświetlony monit do.

![](mvc-music-store-part-9/_static/image1.png)

Po pomyślnym logowaniu użytkownika będzie wyświetlana widoku adres i płatności.

![](mvc-music-store-part-9/_static/image2.png)

Po ich wprowadzeniu formularza i przesłać kolejności, będą one wyświetlane ekran potwierdzenia zamówienia.

![](mvc-music-store-part-9/_static/image3.png)

Próba wyświetlenia kolejności nieistniejącą lub kolejność, która nie należy do Ciebie zostaną wyświetlone w widoku błędów.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Migrowanie koszyka

Podczas procesu zakupów anonimowe, gdy użytkownik kliknie przycisk wyewidencjonowania, one będą musieli zarejestrować i logowania. Użytkownicy będą oczekują zachowamy ich zakupów informacji koszyka między wizyt, więc musimy kojarzy informacje koszyka zakupów z użytkownikiem, po ich zakończeniu rejestracji lub logowania.

Jest to rzeczywiście bardzo proste, w celu, ponieważ klasa nasze ShoppingCart już ma metodę, która zostanie skojarzony wszystkich elementów na bieżącej koszyka z nazwą użytkownika. Po prostu musimy ta metoda jest wywoływana po zakończeniu użytkownika rejestracji lub logowania.

Otwórz **elementu AccountController** klasy, która dodaliśmy możemy zostały konfigurowania członkostwa i autoryzacji. Dodaj następnie za pomocą instrukcji odwołujące się do MvcMusicStore.Models, dodaj następującą metodę MigrateShoppingCart:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Następnie należy zmodyfikować akcji po logowaniu do wywołania MigrateShoppingCart po zweryfikowaniu użytkownika, jak pokazano poniżej:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Wprowadzić do rejestru post akcji, natychmiast po pomyślnym utworzeniu konta użytkownika:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

To wszystko — teraz anonimowe koszyk zostanie automatycznie przeniesiona do konta użytkownika po pomyślnej rejestracji lub logowania.

## <a name="creating-the-checkoutcontroller"></a>Tworzenie CheckoutController

Kliknij prawym przyciskiem myszy folder kontrolery i dodania nowego kontrolera do projektu o nazwie CheckoutController przy użyciu szablonu pusty kontroler.

![](mvc-music-store-part-9/_static/image5.png)

Najpierw dodaj atrybut autoryzacji powyżej deklaracji klasy kontrolera, aby wymagać od użytkowników zarejestrować przed wyewidencjonowania:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Uwaga: Jest to podobne do zmiany, które wprowadziliśmy wcześniej do StoreManagerController, ale w takim przypadku wymagany atrybut autoryzacji, czy użytkownik jest w roli administratora. W kontrolerze wyewidencjonowania, firma Microsoft jest wymaganie, użytkownik jest zalogowany, ale nie są wymagające, aby były Administratorzy.*

Dla uproszczenia firma Microsoft nie będzie można zajmujących się informacje o płatności w tym samouczku. Zamiast tego jest więcej użytkowników do wyewidencjonowania, przy użyciu kod promocyjny. Ten kod promocyjny przy użyciu stałej o nazwie PromoCode będzie przechowywane.

Jak StoreController firma Microsoft będzie zadeklarować pole do przechowywania wystąpienia klasy MusicStoreEntities o nazwie storeDB. Aby używać klasy MusicStoreEntities, musimy dodać za pomocą instrukcji MvcMusicStore.Models przestrzeni nazw. Na początku kontrolera wyewidencjonowania pojawia się poniżej.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

CheckoutController mają następujące akcje kontrolera:

**AddressAndPayment (metoda GET)** spowoduje wyświetlenie formularza, aby umożliwić użytkownikom wprowadzanie informacji o ich.

**AddressAndPayment (metody POST)** będzie sprawdzanie poprawności danych wejściowych i przetwarzania zamówienia.

**Zakończenie** będą wyświetlane, gdy użytkownik pomyślnie zakończy proces realizacji transakcji. Ten widok będzie zawierać liczbę porządkową użytkownika, jako potwierdzenia.

Po pierwsze teraz Zmień nazwę akcji kontrolera indeksu (który został wygenerowany, gdy utworzono kontrolera) na AddressAndPayment. Ta akcja kontrolera wyświetli formularz wyewidencjonowania, więc nie wymaga żadnych informacji o modelu.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Nasze metody AddressAndPayment POST będzie wykonują te same czynności, które zostały użyte podczas StoreManagerController: spróbuje zaakceptować przesyłania formularza i ukończyć kolejność i ponownie spowoduje wyświetlenie formularza, jeśli działanie nie powiodło się.

Po sprawdzanie poprawności danych wejściowych z formularza spełnia wymagania weryfikacji naszych zamówienia, firma Microsoft będzie sprawdzać PromoCode wartości formularza bezpośrednio. Zakładając, że wszystkie ustawienia są poprawne, możemy zaktualizowane informacje zostaną zapisane w kolejności, poinformuj obiektu ShoppingCart, aby ukończyć proces kolejności i Przekierowanie do ukończenia akcji.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Po pomyślnym zakończeniu procesu realizacji transakcji użytkowników zostanie przekierowany do akcji kontrolera ukończone. Ta akcja wykona prostego wyboru, aby zweryfikować, że kolejność w rzeczywistości należy do zalogowanego użytkownika przed pokazaniem numer zamówienia jako potwierdzenie.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Uwaga: Wystąpił błąd podczas widoku został utworzony automatycznie firmie Microsoft w folderze /Views/Shared momencie mamy rozpoczęcia projektu.*

Kompletny kod CheckoutController wygląda następująco:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Dodawanie widoku AddressAndPayment

Teraz Utwórzmy AddressAndPayment widoku. Kliknij prawym przyciskiem myszy na jednym z AddressAndPayment akcji kontrolera, a następnie Dodaj widok o nazwie AddressAndPayment jest silnie typizowane jako kolejność, która używa Edytuj szablon, jak pokazano poniżej.

![](mvc-music-store-part-9/_static/image6.png)

Ten widok spowoduje, że użycie dwóch metod analizujemy podczas tworzenia widoku StoreManagerEdit:

- Używamy Html.EditorForModel() do wyświetlania pól formularza dla modelu kolejności
- Firma Microsoft będzie korzystać z atrybutów sprawdzania poprawności przy użyciu klasy kolejność reguł sprawdzania poprawności

Zaczniemy aktualizując kod formularza, aby użyć Html.EditorForModel() następuje dodatkowe pole tekstowe dla ten kod promocyjny. Kompletny kod dla widoku AddressAndPayment przedstawiono poniżej.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Definiowanie reguł sprawdzania poprawności dla zlecenia

Teraz, kiedy naszych widoku jest skonfigurowane, możemy ustawi reguł sprawdzania poprawności dla modelu kolejności jak robiliśmy wcześniej dla modelu albumu. Kliknij prawym przyciskiem folder modeli i dodać klasę o nazwie kolejności. Oprócz atrybuty weryfikacji, które wcześniej użyliśmy albumu firma Microsoft będzie także przy użyciu wyrażenia regularnego Sprawdź poprawność adresu e-mail użytkownika.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Podjęto próbę przesłać formularza z brakującym lub nieprawidłowe informacje teraz wyświetli komunikat o błędzie przy użyciu weryfikacji po stronie klienta.

![](mvc-music-store-part-9/_static/image7.png)

Zgoda zostało wykonane większość pracy twardych dla realizacji; po prostu mamy kilka prawdopodobieństwo i kończy na zakończenie. Należy dodać dwa widoki proste i musimy zajmie się oddanie informacji koszyka w procesie logowania.

## <a name="adding-the-checkout-complete-view"></a>Dodawanie wyewidencjonowania pełnego widoku

Wyewidencjonowanie pełnego widoku jest dość proste, ponieważ wymaga ona jedynie do wyświetlenia identyfikatora kolejności. Kliknij prawym przyciskiem akcji kontrolera pełne i Dodaj widok o nazwie Complete, który jest silnie typizowane jako int.

![](mvc-music-store-part-9/_static/image8.png)

Teraz modyfikacjom kod widoku do wyświetlenia Identyfikatora kolejności, jak pokazano poniżej.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Trwa aktualizowanie widoku błędów

Szablon domyślny zawiera widoku błędów w folderze Widoki udostępnione, dzięki czemu może być ponownie używane w innym miejscu w lokacji. Ten widok błąd zawiera błąd bardzo proste i nie używa naszej witrynie układu, więc będziemy informować go.

Ponieważ jest to strona rodzajowy komunikat o błędzie, zawartość jest bardzo proste. Firma Microsoft będzie zawierać wiadomości, a łącze, aby przejść do poprzedniej strony w historii, jeśli użytkownik chce ponownie spróbuj wykonać akcję ich.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-8.md)
> [dalej](mvc-music-store-part-10.md)
