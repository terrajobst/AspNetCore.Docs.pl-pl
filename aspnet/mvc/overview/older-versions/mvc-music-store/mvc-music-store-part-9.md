---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
title: 'Część 9: Rejestracja i finalizacja zakupu | Dokumentacja firmy Microsoft'
author: jongalloway
description: W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 9 obejmuje Rejestracja i finalizacja zakupu.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: d65c5c2b-a039-463f-ad29-25cf9fb7a1ba
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-9
msc.type: authoredcontent
ms.openlocfilehash: 935729be0dce790c6fce2e2e982ee063318d64dd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367013"
---
<a name="part-9-registration-and-checkout"></a>Część 9: Rejestracja i finalizacja zakupu
====================
przez [Galloway'em Jon](https://github.com/jongalloway)

> MVC Music Store jest aplikacją z samouczka, który wprowadzono i opisano krok po kroku, jak używać platformy ASP.NET MVC i programu Visual Studio do tworzenia aplikacji internetowych.  
>   
> MVC Music Store jest uproszczone przykładową implementację magazynu sprzedaje utworów muzycznych albumy online, która implementuje podstawowej witryny administracji, logowania użytkownika i funkcje koszyka zakupów.  
>   
> W tej serii samouczków szczegółowo opisuje wszystkie etapy, tworzenie przykładowej aplikacji platformy ASP.NET MVC Music Store. Część 9 obejmuje Rejestracja i finalizacja zakupu.


W tej sekcji utworzymy CheckoutController, który będzie zbierać dobierana adresu i informacji o płatności. Firma Microsoft będzie wymagać od użytkowników do rejestracji w usłudze naszą witrynę przed wyewidencjonowywanie, więc ten kontroler wymaga autoryzacji.

Użytkownicy spowoduje przejście do rozpoczęcie procesu realizowania zamówienia z ich koszyka, klikając przycisk "Wyewidencjonowania".

![](mvc-music-store-part-9/_static/image1.jpg)

Jeśli użytkownik nie jest zalogowany, ich zostanie wyświetlony monit.

![](mvc-music-store-part-9/_static/image1.png)

Po pomyślnym logowaniu użytkownika będzie wyświetlana w widoku adresu i płatności.

![](mvc-music-store-part-9/_static/image2.png)

Po ich wprowadzeniu formularza i złożeniem zamówienia, będą one wyświetlane ekranie potwierdzenia zamówienia.

![](mvc-music-store-part-9/_static/image3.png)

Podjęto próbę wyświetlić kolejność nieistniejącej lub zamówienie, która nie należy do Ciebie pokaże widoku błędów.

![](mvc-music-store-part-9/_static/image4.png)

## <a name="migrating-the-shopping-cart"></a>Migrowanie koszyka

Chociaż proces zakupów anonimowe, gdy użytkownik kliknie przycisk wyewidencjonowania, ich będą musieli zarejestrować i zaloguj się. Użytkownicy oczekują, że firma Microsoft zachowa informacjami koszyka zakupów między wizyt, dzięki czemu firma Microsoft będzie należy skojarzyć je koszyka zakupów z użytkownikiem, po ich zakończeniu rejestracji lub logowania.

Jest to naprawdę bardzo proste, w celu klasy Nasze ShoppingCart ma już metody, która zostanie skojarzony z wszystkich elementów w koszyku bieżącego z nazwą użytkownika. Po prostu konieczne będzie wywołanie tej metody, gdy użytkownik kończy rejestracji lub logowania.

Otwórz **elementu AccountController** klasę, która dodaliśmy przygotowując możemy zostały członkostwo i autoryzacja. Dodaj następnie za pomocą instrukcji odwołujące się do MvcMusicStore.Models, dodaj następującą metodę MigrateShoppingCart:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample1.cs)]

Następnie zmodyfikuj akcji po logowania, aby wywołać MigrateShoppingCart po sprawdzeniu poprawności użytkownika, jak pokazano poniżej:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample2.cs)]

Należy tej samej zmiany do rejestru wpis akcji, natychmiast, po pomyślnym utworzeniu konta użytkownika:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample3.cs)]

To wszystko — teraz anonimowe koszyka będą automatycznie przekazywane do konta użytkownika po pomyślnej rejestracji lub logowania.

## <a name="creating-the-checkoutcontroller"></a>Tworzenie CheckoutController

Kliknij prawym przyciskiem myszy w folderze kontrolerów i Dodaj nowy kontroler do projektu o nazwie CheckoutController przy użyciu szablonu pusty kontroler.

![](mvc-music-store-part-9/_static/image5.png)

Najpierw dodaj atrybut Autoryzuj nad deklaracją klasy kontrolera, aby wymagać od użytkowników zarejestrowania przed wyewidencjonowania:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample4.cs)]

*Uwaga: Jest to podobne do zmiany, które wcześniej wprowadziliśmy StoreManagerController, ale w tym przypadku atrybut autoryzacji wymagane, aby użytkownik był w roli administratora. W kontrolerze wyewidencjonowania, firma Microsoft jest wymaganie, użytkownik jest zalogowany, ale nie wymaga, aby były one administratorów.*

Dla uproszczenia firma Microsoft nie będzie można zajmujących się informacje o płatności w ramach tego samouczka. Zamiast tego jest więcej użytkowników do wyewidencjonowania, przy użyciu kodu promocyjnego. Ten kod promocyjny przy użyciu stałą o nazwie PromoCode będą przechowywane.

Jak StoreController firma Microsoft będzie zadeklarować pole do przechowywania wystąpienia klasy MusicStoreEntities o nazwie storeDB. Aby można było wprowadzić korzystać z klasy MusicStoreEntities, firma Microsoft będzie należy dodać, przy użyciu instrukcji dla przestrzeni nazw MvcMusicStore.Models. Górnej kontrolera wyewidencjonowania pojawia się poniżej.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample5.cs)]

CheckoutController będzie miała następujące akcje kontrolera:

**AddressAndPayment (metoda GET)** spowoduje wyświetlenie formularza, aby umożliwić użytkownikowi wprowadzić informacje o ich.

**AddressAndPayment (metody POST)** będzie sprawdzanie poprawności danych wejściowych i przetworzenia zamówienia.

**Pełne** zostanie wyświetlone po użytkownik zostało zakończone pomyślnie rozpoczęcie procesu realizowania zamówienia. Ten widok będzie zawierać numer zamówienia przez użytkownika, jako potwierdzenie.

Po pierwsze możemy zmienić nazwy indeksu akcji kontrolera, (który został wygenerowany podczas tworzenia kontrolera) na AddressAndPayment. Ta akcja kontrolera po prostu wyświetla formularz wyewidencjonowania, więc nie wymaga żadnych informacji o modelu.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample6.cs)]

Nasze metody AddressAndPayment POST będzie zgodna z tego samego wzorca użytymi w StoreManagerController: spróbuje zaakceptować przesyłania formularza i wypełnij zamówienie i ponownie spowoduje wyświetlenie formularza, jeśli zakończy się niepowodzeniem.

Po weryfikacji danych wejściowych z formularza spełnia naszych wymagań w zakresie sprawdzania poprawności dla zamówienia, firma Microsoft będzie sprawdzać wartości formularza PromoCode bezpośrednio. Przy założeniu, że wszystko jest poprawna, firma Microsoft będzie zapisywać zaktualizowane informacje o kolejności, powiedzieć obiektowi ShoppingCart, aby ukończyć proces kolejności i Przekieruj do Zakończenie akcji.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample7.cs)]

Po pomyślnym zakończeniu procesu realizowania zamówienia nastąpi przekierowanie do akcji kontrolera pełną użytkowników. Ta akcja wykona prostą kontrolę, aby zweryfikować, że kolejność faktycznie należą do zalogowanego użytkownika przed wyświetleniem numer zamówienia jako potwierdzenie.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample8.cs)]

*Uwaga: Widok błąd został automatycznie utworzony dla nas w folderze /Views/Shared zaczynaliśmy korzystać z projektu.*

Kompletny kod CheckoutController jest następująca:

[!code-csharp[Main](mvc-music-store-part-9/samples/sample9.cs)]

## <a name="adding-the-addressandpayment-view"></a>Dodawanie widoku AddressAndPayment

Teraz Utwórzmy widoku AddressAndPayment. Kliknij prawym przyciskiem myszy na jednym z AddressAndPayment akcji kontrolera, a następnie Dodaj widok o nazwie AddressAndPayment jest silnie typizowane jako zamówienie, która używa tego szablonu edycji, jak pokazano poniżej.

![](mvc-music-store-part-9/_static/image6.png)

W tym widoku spowoduje, że korzystanie z dwóch technik przyjrzeliśmy się podczas tworzenia widoku StoreManagerEdit:

- Firma Microsoft użyje Html.EditorForModel() do wyświetlania pól formularza dla modelu zamówienia
- Firma Microsoft będzie korzystać z reguł sprawdzania poprawności, za pomocą klasy kolejności przy użyciu atrybutów sprawdzania poprawności

Zaczniemy, aktualizując kod formularza, aby użyć Html.EditorForModel(), następuje dodatkowe pole tekstowe dla kodu promocyjnego. Kompletny kod dla widoku AddressAndPayment znajdują się poniżej.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample10.cshtml)]

## <a name="defining-validation-rules-for-the-order"></a>Definiowanie reguł sprawdzania poprawności dla zamówienia

Teraz, gdy skonfigurowano naszych widoku firma Microsoft ustawi reguł sprawdzania poprawności dla modelu kolejności ile My mieliśmy wcześniej dla modelu albumu. Kliknij prawym przyciskiem myszy w folderze modeli i Dodaj klasę o nazwie zamówienia. Oprócz atrybutów sprawdzania poprawności, która była wcześniej używana albumu również użyjemy wyrażeń regularnych do sprawdzania poprawności adresu e-mail użytkownika.

[!code-csharp[Main](mvc-music-store-part-9/samples/sample11.cs)]

Podjęto próbę Prześlij formularz z brakującymi lub nieprawidłowe informacje będą teraz pokazywać komunikat o błędzie, za pomocą weryfikacji po stronie klienta.

![](mvc-music-store-part-9/_static/image7.png)

To wszystko firma Microsoft wykonane większość trudną pracę na rozpoczęcie procesu realizowania zamówienia; Wystarczy kilka kończy się i kolizję na zakończenie. Należy dodać dwa widoki proste i musimy zadbać o przekazywanie informacji koszyka w procesie logowania.

## <a name="adding-the-checkout-complete-view"></a>Dodawanie wyewidencjonowania pełny przegląd

Wyewidencjonowanie pełnego widoku jest całkiem proste, po prostu potrzeba do wyświetlenia identyfikatora zamówienia. Kliknij prawym przyciskiem myszy na akcję pełną kontrolera i dodać widok o nazwie zakończone, w którym zdecydowanie jest wpisane w formie typu int.

![](mvc-music-store-part-9/_static/image8.png)

Teraz zaktualizujemy kod widok, aby wyświetlić identyfikator zamówienia, jak pokazano poniżej.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample12.cshtml)]

## <a name="updating-the-error-view"></a>Trwa aktualizowanie widoku błędów

Szablon domyślny zawiera widoku błędów w folderze Widoki udostępnione, dzięki czemu może być ponownie używane gdzie indziej w lokacji. Ten widok błąd zawiera bardzo prosty błąd i nie używa naszą stronę układu, dlatego zaktualizujemy go.

Ponieważ jest to strona błąd rodzajowy, zawartość jest bardzo proste. Firma Microsoft będzie obejmują wiadomość i łącze, aby przejść do poprzedniej strony w historii, jeśli użytkownik chce ponowić próbę ich działania.

[!code-cshtml[Main](mvc-music-store-part-9/samples/sample13.cshtml)]


> [!div class="step-by-step"]
> [Poprzednie](mvc-music-store-part-8.md)
> [dalej](mvc-music-store-part-10.md)
