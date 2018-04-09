---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
title: 'Iteracja #3 — Dodaj walidacji formularza (VB) | Dokumentacja firmy Microsoft'
author: microsoft
description: W trzecim iteracji dodamy podstawowej postaci weryfikacji. Firma Microsoft uniemożliwiać przesyłanie formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować emai...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 4805e75a-7911-46e3-b11b-229a6eed245e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 8e30e247bd31dfb800eea517d195025f9e881cd3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="iteration-3--add-form-validation-vb"></a>Iteracja #3 — Dodaj walidacji formularza (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz kod](iteration-3-add-form-validation-vb/_static/contactmanager_3_vb1.zip)

> W trzecim iteracji dodamy podstawowej postaci weryfikacji. Firma Microsoft uniemożliwiać przesyłanie formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować adresy e-mail i numerów telefonów.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Tworzenie aplikacji ASP.NET MVC kontaktu administracyjnego (VB)
  

W tej serii samouczków budujemy całej aplikacji zarządzania skontaktuj się z od początku do zakończenia. Aplikacji skontaktuj się z Menedżera umożliwia przechowywanie informacji kontaktowych - nazwy, numerów telefonów i adresów e-mail — lista osób.

Firma Microsoft kompilowania aplikacji za pośrednictwem przejść przez wiele iteracji. Przy każdej iteracji firma Microsoft stopniowego zwiększenia aplikacji. Celem tego podejścia wiele iteracji jest ułatwia zrozumienie przyczyn, dla każdej zmiany.

- Iteracja #1 — Tworzenie aplikacji. W pierwszej iteracji utworzymy menedżera kontaktu w najprostszym sposobem możliwe. Możemy dodać obsługę operacji podstawowej bazy danych: tworzenia, odczytu, aktualizacji i usuwania (CRUD).

- Iteracji #2 — należy Szukaj nieuprzywilejowany aplikacji. W tym iteracji możemy ulepszyć wyglądu aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku programu ASP.NET MVC i kaskadowych arkuszy stylów.

- Iteracja #3 — Dodawanie walidacji formularza. W trzecim iteracji dodamy podstawowej postaci weryfikacji. Firma Microsoft uniemożliwiać przesyłanie formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować adresy e-mail i numerów telefonów.

- Iteracji #4 — należy luźno powiązane z aplikacji. W tym trzeci iteracji możemy korzystać z kilku wzorce projektowe oprogramowania do ułatwiają obsługiwanie i modyfikowanie aplikacji Menedżera skontaktuj się z pomocą. Na przykład firma Microsoft Refaktoryzuj naszej aplikacji do korzystania z wzorca repozytorium i wzorzec iniekcji zależności.

- Iteracja #5 - tworzenia testów jednostkowych. W piątym iteracji możemy ułatwić naszej aplikacji obsługiwanie i modyfikowanie przez dodanie testów jednostkowych. Firma Microsoft mock naszej klasy modelu danych i tworzenie testów jednostkowych dla naszych kontrolerów i logiki sprawdzania poprawności.

- Iteracja #6 - użyj test-driven development. W tym szóstego iteracji dodania nowych funkcji do naszej aplikacji najpierw pisania testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji dodamy grup kontaktów.

- Iteracja #7 - dodawania funkcjonalności interfejsu Ajax. Siódmego iteracji możemy ulepszyć czas reakcji i wydajności aplikacji, dodając obsługę technologii Ajax.


## <a name="this-iteration"></a>Tej iteracji

W drugiej iteracji aplikacji kontaktów Menedżerze dodamy podstawowej postaci weryfikacji. Firma Microsoft uniemożliwiać przesyłanie kontaktu bez podawania wartości wymaganych pól formularza. Możemy zweryfikować numerów telefonów i adresów e-mail (zobacz rysunek 1).


[![Okno dialogowe nowego projektu](iteration-3-add-form-validation-vb/_static/image1.jpg)](iteration-3-add-form-validation-vb/_static/image1.png)

**Rysunek 01**: formularza z weryfikacją ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](iteration-3-add-form-validation-vb/_static/image2.png))


W tym iteracji możemy dodać logikę weryfikacji bezpośrednio do akcji kontrolera. Ogólnie rzecz biorąc to nie jest zalecanym sposobem Dodawanie walidacji do aplikacji platformy ASP.NET MVC. Lepszym rozwiązaniem jest umieszczenie logikę weryfikacji s aplikacji w oddzielnej [warstwy usług](http://martinfowler.com/eaaCatalog/serviceLayer.html). W następnej iteracji firma Microsoft Refaktoryzuj aplikacji Menedżera skontaktuj się z pomocą, aby aplikacja była bardziej łatwy w obsłudze.

W tym iteracji aby zapewnić prosty, możemy zapisanie wszystkich kodu walidacji ręcznie. Zamiast pisanie kodu sprawdzania poprawności, nad, firma Microsoft może korzystać z strukturze weryfikacji. Na przykład można użyć Microsoft Enterprise biblioteki weryfikacji aplikacji bloku (VAB) wdrożyć logikę weryfikacji dla aplikacji ASP.NET MVC. Aby dowiedzieć się więcej na temat sprawdzania poprawności bloku aplikacji, zobacz:

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a>Dodawanie walidacji do tworzenia widoku

Let s, Rozpocznij od dodania logikę weryfikacji do tworzenia widoku. Na szczęście ponieważ firma Microsoft wygenerowana Utwórz widok z programem Visual Studio, Utwórz widok już zawiera wszystkie logikę interfejsu użytkownika niezbędne do wyświetlania komunikatów dotyczących sprawdzania poprawności. Utwórz widok znajduje się w 1 wyświetlania.

**Wyświetlanie listy 1 - \Views\Contact\Create.aspx**

[!code-aspx[Main](iteration-3-add-form-validation-vb/samples/sample1.aspx)]

Klasa błędzie sprawdzania poprawności pola jest używana do określenia stylu danych wyjściowych przez pomocnika Html.ValidationMessage(). Klasa błędzie sprawdzania poprawności danych wejściowych jest używana do określania stylu pola tekstowego (dane wejściowe) renderowana przez pomocnika Html.TextBox(). Klasa błędy sprawdzania poprawności — podsumowanie jest używana do określania stylu nieuporządkowaną listę renderowana przez pomocnika Html.ValidationSummary().

> [!NOTE] 
> 
> Można zmodyfikować klasy arkusza stylów opisane w tej sekcji, aby dostosować wygląd komunikatów o błędach weryfikacji.


## <a name="adding-validation-logic-to-the-create-action"></a>Dodawanie logiki sprawdzania poprawności można utworzyć akcji

Teraz, ponieważ firma Microsoft nie zostały zapisane logiki, aby wygenerować jakiekolwiek komunikaty Utwórz widok nigdy nie wyświetla komunikaty o błędach weryfikacji. Aby wyświetlić komunikaty o błędach weryfikacji, należy dodać komunikaty o błędach do ModelState.

> [!NOTE] 
> 
> Metoda UpdateModel() dodaje komunikaty o błędach do ModelState automatycznie po błąd przypisanie do właściwości wartości pola formularza. Na przykład jeśli użytkownik spróbuje przypisać ciąg "apple" we właściwości data urodzenia akceptuje wartości daty/godziny, metody UpdateModel() dodaje błąd do ModelState.


Zmodyfikowane w wyświetlania 2 metody Create() zawiera nową sekcję, która weryfikuje właściwości klasy kontaktu, przed jego miejsce nowego kontaktu zostaną wstawione do bazy danych.

**Wyświetlanie listy 2 - Controllers\ContactController.vb (utworzenie z weryfikacją)**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample2.vb)]

W sekcji weryfikacji wymusza cztery różne sprawdzania poprawności reguły:

- Właściwość FirstName musi mieć długość większą niż zero (i jej nie może składać się z samych spacji)
- Właściwość LastName musi mieć długość większą niż zero (i jej nie może składać się z samych spacji)
- Jeśli właściwość telefonu ma wartość (ma długość większą niż 0), a następnie właściwość telefonu musi odpowiada wyrażeniu regularnemu.
- Jeśli właściwość poczty E-mail ma wartość (ma długość większą niż 0), a następnie właściwość poczty E-mail musi być zgodna wyrażenia regularnego.

W przypadku naruszenia reguły sprawdzania poprawności komunikat o błędzie jest dodawany do ModelState za pomocą metody AddModelError(). Po dodaniu wiadomość do ModelState, musisz podać nazwę właściwości, z tekstem komunikat o błędzie weryfikacji. Ten komunikat o błędzie jest wyświetlany w widoku przez metody pomocnicze Html.ValidationSummary() i Html.ValidationMessage().

Po wykonaniu są reguły sprawdzania poprawności, właściwość IsValid ModelState jest zaznaczone. Właściwość IsValid zwraca wartość false, jeśli komunikaty o błędach weryfikacji zostały dodane do ModelState. Jeśli weryfikacja zakończy się niepowodzeniem, Utwórz formularz zostanie wyświetlony ponownie z komunikatów o błędach.

> [!NOTE] 
> 
> Otrzymano wyrażeń regularnych do sprawdzania poprawności telefonu i adresu e-mail z repozytorium wyrażeń regularnych w [*http://regexlib.com*](http://regexlib.com)


## <a name="adding-validation-logic-to-the-edit-action"></a>Dodawanie logiki sprawdzania poprawności działania edycji

Akcja Edit() aktualizuje kontaktu. Akcja Edit() potrzebnych do wykonania jako akcja Create() dokładnie tego samego weryfikacji. Zamiast duplikowania tego samego kodu sprawdzania poprawności, firma Microsoft refaktoryzować skontaktuj się z kontrolerem, aby zarówno Create(), jak i Edit() akcje wywołanie tej samej metody sprawdzania poprawności.

Zmodyfikowane klasy kontrolera kontaktu znajduje się w 3 wyświetlania. Ta klasa ma nową metodę ValidateContact() jest wywoływana w ramach działań Create() i Edit().

**Wyświetlanie listy 3 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-3-add-form-validation-vb/samples/sample3.vb)]

## <a name="summary"></a>Podsumowanie

W tym iteracji dodano do naszej aplikacji Menedżera skontaktuj się z podstawowej postaci weryfikacji. Logiki sprawdzania poprawności uniemożliwia użytkownikom przesyłanie nowych kontaktów lub edycji istniejącego kontaktu bez podawania wartości dla właściwości imię i nazwisko. Ponadto użytkownicy musisz podać prawidłową phone liczb i adresami poczty e-mail.

W tym iteracji dodano logikę weryfikacji do naszej aplikacji menedżera kontaktu w najprostszym sposobem możliwe. Jednak mieszanie logiki sprawdzania poprawności do logiki kontrolera utworzy problemy firmie Microsoft w perspektywie długoterminowej. Naszej aplikacji będzie trudne do obsługiwanie i modyfikowanie wraz z upływem czasu.

W następnej iteracji firma Microsoft będzie Refaktoryzuj naszych logikę weryfikacji i logika dostępu do bazy danych naszych kontrolerami poza. Firma Microsoft będzie korzystać z kilku zasady projektowania oprogramowania umożliwia firmie Microsoft w celu utworzenia aplikacji luźno sprzężonego i bardziej łatwy w obsłudze.

> [!div class="step-by-step"]
> [Poprzednie](iteration-2-make-the-application-look-nice-vb.md)
> [dalej](iteration-4-make-the-application-loosely-coupled-vb.md)
