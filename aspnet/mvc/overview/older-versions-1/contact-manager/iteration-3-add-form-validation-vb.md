---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: 'Iteracja #3 — Dodawanie weryfikacji formularza (VB) | Dokumentacja firmy Microsoft'
author: microsoft
description: W trzecim iteracji dodamy weryfikacji formularza podstawowego. Firma Microsoft ochronić przed przesłaniem formularza nie kończą działania wymaganych pól formularza. Możemy sprawdzić, czy emai...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: a02178bfb662f180343ad32a6453b910e8e70471
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396211"
---
<a name="iteration-3--add-form-validation-vb"></a>Iteracja #3 — Dodawanie weryfikacji formularza (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz program Code](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> W trzecim iteracji dodamy weryfikacji formularza podstawowego. Firma Microsoft ochronić przed przesłaniem formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować adresy e-mail oraz numerów telefonów.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Tworzenie aplikacji zarządzania kontaktami platformy ASP.NET MVC (VB)
  

W tej serii samouczków wbudowujemy całej aplikacji zarządzania skontaktuj się z od początku do zakończenia. Aplikacja Contact Manager umożliwia przechowywanie informacji kontaktowych — nazwy, numerów telefonów i adresów e-mail — lista osób.

Firma Microsoft tworzy aplikację za pośrednictwem wiele iteracji. Z każdą iteracją można stopniowo ulepszyć aplikację. Celem tego wielu podejścia iteracji jest, aby umożliwić Ci zrozumienie przyczyn wprowadzenia poszczególnych zmian.

- Iteracja #1 — Tworzenie aplikacji. W pierwszej iteracji utworzymy Contact Manager w najprostszym sposobem możliwe. Dodano obsługę operacji podstawowej bazy danych: tworzenia, odczytu, aktualizacji i usuwania (CRUD).

- Iteracja 2 # — należy wyglądu nieuprzywilejowany aplikacji. W tej iteracji możemy poprawić wygląd aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku platformy ASP.NET MVC i kaskadowych arkuszy stylów.

- Iteracja #3 — Dodawanie weryfikacji formularza. W trzecim iteracji dodamy weryfikacji formularza podstawowego. Firma Microsoft ochronić przed przesłaniem formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować adresy e-mail oraz numerów telefonów.

- Iteracja 4 # — należy luźne sprzężenie aplikacji. W tym czwarty iteracji możemy korzystać z kilku wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Na przykład możemy refaktoryzować naszej aplikacji do korzystania z wzorca repozytorium i wzorzec iniekcji zależności.

- Iteracja #5 — Tworzenie testów jednostkowych. W piątej iteracji ułatwiamy naszej aplikacji ułatwia konserwację i modyfikowanie, dodając testów jednostkowych. Firma Microsoft testowanie naszych zajęć modelu danych i tworzenie testów jednostkowych dla naszych kontrolery i logikę weryfikacji.

- Iteracja #6 — korzystanie z projektowania opartego na testach. W tym szóstego iteracji dodamy nowe funkcje do naszej aplikacji, najpierw pisanie testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji dodamy grup kontaktów.

- Iteracja #7 — dodawanie funkcji Ajax. W siódmej iteracji można ulepszyć czas odpowiedzi i wydajności naszych aplikacji przez dodanie obsługi technologii AJAX.


## <a name="this-iteration"></a>Tej iteracji

W drugiej iteracji aplikacji Contact Manager dodamy weryfikacji formularza podstawowego. Firma Microsoft ochronić przed przesłaniem kontaktu bez podawania wartości wymaganych pól formularza. Możemy zweryfikować numerów telefonów i adresów e-mail (patrz rysunek 1).


[![Okno dialogowe Nowy projekt](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**Rysunek 01**: formularza z weryfikacją ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-3-add-form-validation-vb/_static/image2.png))


W tej iteracji możemy dodać logikę weryfikacji bezpośrednio do akcji kontrolera. Ogólnie rzecz biorąc to nie jest to zalecany sposób dodawania sprawdzania poprawności do aplikacji ASP.NET MVC. Lepszym rozwiązaniem jest umieszczenie logikę weryfikacji s aplikacji w osobnym [warstwy usług](http://martinfowler.com/eaaCatalog/serviceLayer.html). W następnej iteracji refaktoryzacji aplikacji Contact Manager, aby aplikacja będzie łatwiejszy w utrzymaniu.

W tym iteracji Aby zachować ich prostotę, napiszemy cały kod sprawdzania poprawności ręcznie. Zamiast pisać kod sprawdzania poprawności, osoby, firma Microsoft może korzystać z ram sprawdzania poprawności. Na przykład można użyć Microsoft Enterprise Library weryfikacji aplikacji bloku (VAB) Aby zaimplementować logikę weryfikacji dla aplikacji ASP.NET MVC. Aby dowiedzieć się więcej na temat weryfikacji blok aplikacji, zobacz:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Dodawanie walidacji do widoku Create

Pozwól, s, najpierw Dodaj logikę walidacji do tworzenia widoku. Na szczęście, ponieważ firma Microsoft wygenerowany widok Utwórz za pomocą programu Visual Studio, Utwórz widok zawiera już całą logikę interfejsu użytkownika niezbędne do wyświetlania komunikatów dotyczących sprawdzania poprawności. Utwórz widok znajduje się w ofercie 1.

**Wyświetlanie listy 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

Klasa błędzie sprawdzania poprawności pola jest używana do określenia stylu danych wyjściowych renderowany przez pomocnika Html.ValidationMessage(). Klasa błędzie sprawdzania poprawności danych wejściowych jest używana do określania stylu pola tekstowego (dane wejściowe) renderowana przez pomocnika Html.TextBox(). Klasa błędy sprawdzania poprawności — podsumowanie jest używana do określania stylu listę nieuporządkowaną renderowany przez pomocnika Html.ValidationSummary().

> [!NOTE] 
> 
> Można zmodyfikować klasy arkusza stylów, które są opisane w tej sekcji, aby dostosować wygląd komunikatów o błędach weryfikacji.


## <a name="adding-validation-logic-to-the-create-action"></a>Dodawanie logikę walidacji do utworzenia akcji

W tej chwili, widok Utwórz nigdy nie wyświetla komunikatów o błędach weryfikacji, ponieważ możemy niezapisania logiki można wygenerować wszystkie komunikaty. Aby wyświetlić komunikaty o błędach weryfikacji, należy dodać komunikaty o błędach do ModelState.

> [!NOTE] 
> 
> Metoda UpdateModel() komunikaty o błędach do ModelState automatycznie dodaje po błąd przypisywania wartości pola formularza do właściwości. Na przykład jeśli użytkownik spróbuje przypisać ciąg "apple" do właściwości daty urodzenia, która akceptuje wartości daty/godziny, metoda UpdateModel() dodaje błąd do ModelState.


Modyfikowana Metoda Create() w ofercie 2 zawiera nową sekcję, która weryfikuje właściwości klasy skontaktuj się z przed jego miejsce nowego kontaktu są wstawiane do bazy danych.

**Wyświetlanie listy 2 - Controllers\ContactController.vb (Tworzenie ze sprawdzaniem poprawności)**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

W sekcji sprawdzania poprawności wymusza cztery różne sprawdzania poprawności reguły:

- Właściwość FirstName musi mieć długość większą niż zero (i nie może ona składać się wyłącznie ze spacji)
- Właściwość nazwisko musi mieć długość większą niż zero (i nie może ona składać się wyłącznie ze spacji)
- Jeśli wartość właściwości telefonu (ma długość większą niż 0), a następnie właściwość telefonu musi być zgodna wyrażenia regularnego.
- Jeśli wartość właściwości wiadomości E-mail (ma długość większą niż 0), a następnie właściwość poczty E-mail musi być zgodna wyrażenia regularnego.

Po naruszenie reguły sprawdzania poprawności, komunikat o błędzie jest dodawany do ModelState za pomocą metody AddModelError(). Po dodaniu komunikatu do ModelState, należy podać nazwę właściwości i tekst komunikat o błędzie weryfikacji. Ten komunikat o błędzie jest wyświetlany w widoku przez metody pomocnika Html.ValidationSummary() i Html.ValidationMessage().

Po reguł sprawdzania poprawności są wykonywane, właściwość IsValid ModelState jest sprawdzana. Właściwość IsValid zwraca wartość false, gdy do ModelState zostały dodane jakiekolwiek komunikaty o błędach weryfikacji. Jeśli weryfikacja zakończy się niepowodzeniem, Utwórz formularz zostanie wyświetlony ponownie z komunikatami o błędach.

> [!NOTE] 
> 
> Stało się wyrażeń regularnych do weryfikacji telefonu i adresu e-mail z repozytorium wyrażeń regularnych w [*http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>Dodawanie logikę walidacji do akcji, Edytuj

Akcja Edit() aktualizuje kontakt. Akcja Edit() musi wykonać dokładnie tych samych sprawdzania poprawności akcji Create(). Zamiast duplikowania ten sam kod sprawdzania poprawności, firma Microsoft refaktoryzować skontaktuj się z kontrolerem, dzięki czemu akcje Create() i Edit() wywołać tę samą metodę sprawdzania poprawności.

Zmodyfikowane klasy kontrolera kontaktu znajduje się w ofercie 3. Ta klasa ma nową metodę ValidateContact(), która jest wywoływana w ramach Create() i Edit() akcji.

**Wyświetlanie listy 3 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>Podsumowanie

W tej iteracji dodaliśmy weryfikacji formularza podstawowego do naszej aplikacji Contact Manager. Nasze logikę weryfikacji uniemożliwia użytkownikom przesyłanie nowego kontaktu lub edycji istniejącego kontaktu bez podawania wartości dla właściwości imię i nazwisko. Ponadto użytkownicy muszą podać prawidłowy phone liczb i ich adresy e-mail.

W tej iteracji dodaliśmy logikę walidacji do naszej aplikacji Contact Manager w najprostszym sposobem możliwe. Jednak mieszanie naszych logikę walidacji do naszych logiką kontrolera utworzy problemy dla nas w perspektywie długoterminowej. Nasza aplikacja będzie trudniejsze do konserwację i modyfikowanie wraz z upływem czasu.

W następnej iteracji naszych kontrolerami poza będzie refaktoryzacji naszych logikę weryfikacji i logiką dostępu do bazy danych. Firma Microsoft będzie korzystać z kilku zasad projektowania oprogramowania, aby umożliwiają tworzenie luźno sprzężonych i będzie łatwiejszy w utrzymaniu, aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](iteration-2-make-the-application-look-nice-vb.md)
> [dalej](iteration-4-make-the-application-loosely-coupled-vb.md)
