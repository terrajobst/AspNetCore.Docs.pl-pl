---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
title: 'Iteracja #3 — Dodawanie weryfikacji formularza (C#) | Dokumentacja firmy Microsoft'
author: microsoft
description: W trzecim iteracji dodamy weryfikacji formularza podstawowego. Firma Microsoft ochronić przed przesłaniem formularza nie kończą działania wymaganych pól formularza. Możemy sprawdzić, czy emai...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 51a0d175-913b-43d8-95e3-840fb96ad1a9
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: a76e75f2d343764038142235c92e2db04e807778
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377009"
---
<a name="iteration-3--add-form-validation-c"></a>Iteracja #3 — Dodawanie weryfikacji formularza (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz program Code](iteration-3-add-form-validation-cs/_static/contactmanager_3_cs1.zip)

> W trzecim iteracji dodamy weryfikacji formularza podstawowego. Firma Microsoft ochronić przed przesłaniem formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować adresy e-mail oraz numerów telefonów.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Tworzenie aplikacji zarządzania kontaktami platformy ASP.NET MVC (C#)
  

W tej serii samouczków wbudowujemy całej aplikacji zarządzania skontaktuj się z od początku do zakończenia. Aplikacja Contact Manager umożliwia przechowywanie informacji kontaktowych — nazwy, numerów telefonów i adresów e-mail — lista osób.

Firma Microsoft tworzy aplikację za pośrednictwem wiele iteracji. Z każdą iteracją można stopniowo ulepszyć aplikację. Celem tego wielu podejścia iteracji jest, aby umożliwić Ci zrozumienie przyczyn wprowadzenia poszczególnych zmian.

- Iteracja #1 — Tworzenie aplikacji. W pierwszej iteracji utworzymy Contact Manager w najprostszym sposobem możliwe. Dodano obsługę operacji podstawowej bazy danych: tworzenia, odczytu, aktualizacji i usuwania (CRUD).

- Iteracja 2 # — należy wyglądu nieuprzywilejowany aplikacji. W tej iteracji możemy poprawić wygląd aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku platformy ASP.NET MVC i kaskadowych arkuszy stylów.

- Iteracja #3 — Dodawanie weryfikacji formularza. W trzecim iteracji dodamy weryfikacji formularza podstawowego. Firma Microsoft ochronić przed przesłaniem formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować adresy e-mail oraz numerów telefonów.

- Iteracja 4 # — należy luźne sprzężenie aplikacji. W tym trzeci iteracji możemy skorzystać z kilku wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Na przykład możemy refaktoryzować naszej aplikacji do korzystania z wzorca repozytorium i wzorzec iniekcji zależności.

- Iteracja #5 — Tworzenie testów jednostkowych. W piątej iteracji ułatwiamy naszej aplikacji ułatwia konserwację i modyfikowanie, dodając testów jednostkowych. Firma Microsoft testowanie naszych zajęć modelu danych i tworzenie testów jednostkowych dla naszych kontrolery i logikę weryfikacji.

- Iteracja #6 — korzystanie z projektowania opartego na testach. W tym szóstego iteracji dodamy nowe funkcje do naszej aplikacji, najpierw pisanie testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji dodamy grup kontaktów.

- Iteracja #7 — dodawanie funkcji Ajax. W siódmej iteracji można ulepszyć czas odpowiedzi i wydajności naszych aplikacji przez dodanie obsługi technologii AJAX.


## <a name="this-iteration"></a>Tej iteracji

W drugiej iteracji aplikacji Contact Manager dodamy weryfikacji formularza podstawowego. Firma Microsoft ochronić przed przesłaniem kontaktu bez podawania wartości wymaganych pól formularza. Możemy zweryfikować numerów telefonów i adresów e-mail (patrz rysunek 1).


[![Okno dialogowe Nowy projekt](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)

**Rysunek 01**: formularza z weryfikacją ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-3-add-form-validation-cs/_static/image2.png))


W tej iteracji możemy dodać logikę weryfikacji bezpośrednio do akcji kontrolera. Ogólnie rzecz biorąc to nie jest to zalecany sposób dodawania sprawdzania poprawności do aplikacji ASP.NET MVC. Lepszym rozwiązaniem jest umieszczenie logikę weryfikacji s aplikacji w osobnym [warstwy usług](http://martinfowler.com/eaaCatalog/serviceLayer.html). W następnej iteracji refaktoryzacji aplikacji Contact Manager, aby aplikacja będzie łatwiejszy w utrzymaniu.

W tym iteracji Aby zachować ich prostotę, napiszemy cały kod sprawdzania poprawności ręcznie. Zamiast pisać kod sprawdzania poprawności, osoby, firma Microsoft może korzystać z ram sprawdzania poprawności. Na przykład można użyć Microsoft Enterprise Library weryfikacji aplikacji bloku (VAB) Aby zaimplementować logikę weryfikacji dla aplikacji ASP.NET MVC. Aby dowiedzieć się więcej na temat weryfikacji blok aplikacji, zobacz:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Dodawanie walidacji do widoku Create

Pozwól, s, najpierw Dodaj logikę walidacji do tworzenia widoku. Na szczęście, ponieważ firma Microsoft wygenerowany widok Utwórz za pomocą programu Visual Studio, Utwórz widok zawiera już całą logikę interfejsu użytkownika niezbędne do wyświetlania komunikatów dotyczących sprawdzania poprawności. Utwórz widok znajduje się w ofercie 1.

**Wyświetlanie listy 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-cs/samples/sample1.aspx)]

Zwróć uwagę, wywołanie metody pomocnika Html.ValidationSummary(), pojawia się natychmiast powyżej formularza HTML. W przypadku komunikatów o błędach weryfikacji następnie ta metoda Wy wyświetla komunikatów dotyczących sprawdzania poprawności na liście punktowanej.

Zwróć uwagę, ponadto wywołania Html.ValidationMessage(), które pojawiają się obok każdego pola formularza. Pomocnik ValidationMessage() wyświetli komunikat o błędzie sprawdzania poprawności poszczególnych. W przypadku wyświetlania listy 1 gwiazdka jest wyświetlana, gdy występuje błąd weryfikacji.

Na koniec pomocnika Html.TextBox() automatycznie renderuje klas kaskadowych arkuszy stylów, gdy występuje błąd weryfikacji skojarzony z właściwością wyświetlane przez pomocnika. Pomocnik Html.TextBox() renderuje klasę o nazwie **błędzie sprawdzania poprawności danych wejściowych**.

Podczas tworzenia nowej aplikacji platformy ASP.NET MVC, arkusz stylów o nazwie pliku Site.css jest automatycznie tworzony w folderze zawartości. Ten arkusz stylów zawiera poniższe definicje klas CSS związane z wygląd komunikatów o błędach weryfikacji:

[!code-css[Main](iteration-3-add-form-validation-cs/samples/sample2.css)]

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

**Wyświetlanie listy 2 - Controllers\ContactController.cs (Tworzenie ze sprawdzaniem poprawności)**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample3.cs)]

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

**Wyświetlanie listy 3 - Controllers\ContactController.cs**

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample4.cs)]

## <a name="summary"></a>Podsumowanie

W tej iteracji dodaliśmy weryfikacji formularza podstawowego do naszej aplikacji Contact Manager. Nasze logikę weryfikacji uniemożliwia użytkownikom przesyłanie nowego kontaktu lub edycji istniejącego kontaktu bez podawania wartości dla właściwości imię i nazwisko. Ponadto użytkownicy muszą podać prawidłowy phone liczb i ich adresy e-mail.

W tej iteracji dodaliśmy logikę walidacji do naszej aplikacji Contact Manager w najprostszym sposobem możliwe. Jednak mieszanie naszych logikę walidacji do naszych logiką kontrolera utworzy problemy dla nas w perspektywie długoterminowej. Nasza aplikacja będzie trudniejsze do konserwację i modyfikowanie wraz z upływem czasu.

W następnej iteracji naszych kontrolerami poza będzie refaktoryzacji naszych logikę weryfikacji i logiką dostępu do bazy danych. Firma Microsoft będzie korzystać z kilku zasad projektowania oprogramowania, aby umożliwiają tworzenie luźno sprzężonych i będzie łatwiejszy w utrzymaniu, aplikacji.

> [!div class="step-by-step"]
> [Poprzednie](iteration-2-make-the-application-look-nice-cs.md)
> [dalej](iteration-4-make-the-application-loosely-coupled-cs.md)
