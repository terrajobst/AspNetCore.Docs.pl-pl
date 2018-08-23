---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: 'Część 5: Logika biznesowa | Dokumentacja firmy Microsoft'
author: JoeStagner
description: W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 5 dodaje niektóre logiki biznesowej.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e18acb66dbdb3bd3e0dfa21193f617dad82afc74
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755659"
---
<a name="part-5-business-logic"></a>Część 5: Logika biznesowa
====================
przez [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks pokazuje, jak bardzo łatwo jest tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET. Przedstawia on poza sposób użycia wspaniałych nowych funkcjach w ASP.NET 4 do tworzenia sklep online, m.in. zakupy wyewidencjonowanie i Administracja.
> 
> W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 5 dodaje niektóre logiki biznesowej.


## <a id="_Toc260221671"></a>  Dodawanie logiki biznesowej

Chcemy, aby nasze środowisko zakupów były dostępne w każdym przypadku, gdy ktoś odwiedza witryny sieci web. Osoby odwiedzające będzie przeglądanie i dodawanie elementów do koszyka, nawet jeśli nie są zarejestrowane lub zalogowany. Gdy są one gotowe do wyewidencjonowania otrzymają oni możliwość uwierzytelniania i jeśli nie są jeszcze Członkowie będą mogli utworzyć konto.

Oznacza to, że firma Microsoft będzie należy zaimplementować logikę do przekonwertowania koszyk sklepowy z anonimowych stanu do stanu "Zarejestrowany użytkownik".

Teraz Utwórz katalog o nazwie "Klasy", a następnie kliknij prawym przyciskiem myszy w folderze i utworzyć nowy plik "Class" o nazwie MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Jak wcześniej wspomniano, będziemy rozszerzać klasy, która implementuje stronę MyShoppingCart.aspx i zrobimy to za pomocą. Konstrukcja zaawansowane "klasy częściowej" NET firmy.

Wygenerowany wywołania dla naszego pliku MyShoppingCart.aspx.cf wygląda następująco.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Zwróć uwagę na użycie "partial" — słowo kluczowe.

Plik klasy, które właśnie wygenerowaną wygląda następująco.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Firma Microsoft spowoduje scalenie naszej implementacji, dodając partial-słowo kluczowe do tego pliku.

Nasz nowy plik klasy wygląda teraz następująco.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

Pierwsza metoda, która zostanie dodany do naszych klasy jest metodą "AddItem". Jest to metoda, który ostatecznie zostanie wywołana, gdy użytkownik kliknie w łączach "Dodaj do grafikę" na stronach listy produktów oraz szczegóły produktów.

Dołącz następujący ciąg do przy użyciu instrukcji w górnej części strony.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

I Dodaj tę metodę do klasy MyShoppingCart.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Używamy składnik LINQ to Entities, aby zobaczyć, czy element jest już w koszyku. Jeśli tak, możemy zaktualizować ilość zamówienia elementu, w przeciwnym razie firma Microsoft tworzy nowy wpis dla wybranego elementu

Aby wywołać tę metodę wdrażamy strony AddToCart.aspx, który nie tylko metody tej klasy, ale następnie wyświetlone bieżące koszyk = po dodaniu elementu.

Kliknij prawym przyciskiem myszy nazwę rozwiązania w Eksploratorze rozwiązań i Dodaj i nową stronę o nazwie AddToCart.aspx, jak firma Microsoft wcześniej robione.

Gdy firma Microsoft może używać tej strony do wyświetlenia wyników pośrednich, takich jak niska podstawowych problemów itp., w naszej implementacji, strony bez faktycznego renderowania, ale raczej wywoła logiki "Add" i przekierowania.

W tym celu dodamy poniższy kod do strony\_załadowane zdarzenie.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Należy pamiętać, że trwa pobieranie produktu do dodania do koszyka parametr QueryString i wywołanie metody AddItem klasy Nasze.

Zakładając, że żadne błędy nie zostaną napotkane kontrola jest przekazywana do strony SHoppingCart.aspx, który będzie w pełni wdrażamy obok. Jeśli ma to być błąd możemy zgłosić wyjątek.

Obecnie mamy jeszcze nie zaimplementowano globalna procedura obsługi błędów, więc będzie nieobsłużony wyjątek przez naszą aplikację, ale firma Microsoft będzie rozwiązać ten problem wkrótce.

Ponadto Zwróć uwagę na użycie instrukcji Debug.Fail() (dostępne za pośrednictwem `using System.Diagnostics;)`

To aplikacja jest uruchomiona w debugerze, ta metoda spowoduje wyświetlenie okna dialogowego szczegółowe informacje na temat stanu aplikacji oraz komunikat o błędzie, który określamy.

Podczas uruchamiania w środowisku produkcyjnym instrukcji Debug.Fail() jest ignorowany.

Można zauważyć w kodzie powyżej wywołanie metody w naszym zakupów nazw klas koszyka "GetShoppingCartId".

Dodaj kod, aby wdrożyć metodę w następujący sposób.

Należy pamiętać, że dodaliśmy również aktualizacji i wyewidencjonowywania przycisków i etykiet, gdzie możemy wyświetlić koszyk "łącznie".

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Teraz możemy dodać elementy do naszego koszyka, ale nie zaimplementowano rozwiązania tlp logikę do wyświetlenia w koszyku po dodaniu produktu.

Tak na stronie MyShoppingCart.aspx dodamy kontrolkę EntityDataSource i kontroli GridVire w następujący sposób.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Wywołaj formularza w projektancie, dzięki czemu można dwukrotnie kliknij przycisk Aktualizuj koszyk i wygenerować program obsługi zdarzeń kliknięcie, który jest określony w deklaracji w znaczniku.

Szczegółowe informacje będą później wdrażamy, ale w ten sposób będzie nam skompilować i uruchomić naszą aplikację bez błędów.

Po uruchomieniu aplikacji i Dodaj element do koszyka zobaczysz następujący.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Należy pamiętać, że firma Microsoft odpowiadają regułom z widoku siatki "domyślna" poprzez implementację trzy kolumny niestandardowej.

Pierwszy jest edytowalna, pole "Związane" ilość:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

Następne jest kolumnę "obliczeniową", która powoduje wyświetlenie elementu wiersz całkowitej (element kosztów razy ilość do zamówienia):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Na koniec mamy kolumnę niestandardową, która zawiera formant pola wyboru, który użytkownik będzie używany do wskazania, że element powinny zostać usunięte z wykresu zakupów.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Jak widać, zamówienia, więc wiersz podsumowania jest pusty dodać logikę do obliczania całkowitego zamówienia.

Firma Microsoft będzie najpierw zaimplementować metodę "GetTotal" do naszych MyShoppingCart klasy.

W pliku MyShoppingCart.cs Dodaj następujący kod.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Następnie na stronie\_firma Microsoft będzie metodę można wywołać nasz GetTotal program obsługi zdarzeń obciążenia. W tym samym czasie dodamy test, aby sprawdzić, czy koszyka jest pusta i odpowiednio dostosować ekran, jeśli jest.

Teraz Jeśli koszyka jest pusta uzyskujemy to:

![](tailspin-spyworks-part-5/_static/image4.jpg)

A jeśli nie, widzimy naszych łącznie.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Jednak ta strona nie jest jeszcze ukończone.

Musimy dodatkowej logiki, aby ponownie obliczyć koszyka, usuwając elementy, które zostały oznaczone do usunięcia i określając nowe wartości ilości, ponieważ niektóre mogły zostać zmienione w siatce przez użytkownika.

Umożliwia, Dodaj metodę "RemoveItem" do klasy Nasze koszyka zakupów w MyShoppingCart.cs, aby obsłużyć przypadek, gdy użytkownik oznaczenie elementu do usunięcia.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Teraz sklonujemy ad metodę, aby obsłużyć sytuacji, gdy użytkownik po prostu zmienia jakości do zamówienia w widoku GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Za pomocą podstawowych funkcji Remove i aktualizację w miejscu można zaimplementować logikę, która faktycznie aktualizuje koszyka zakupów w bazie danych. (In MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Należy zauważyć, że że ta metoda oczekuje dwóch parametrów. Jeden jest koszyka Id, a drugi jest Tablica obiektów typu zdefiniowanego przez użytkownika.

Aby zminimalizować zależności naszych logiki szczegółowych interfejsu użytkownika, zdefiniowaliśmy struktury danych, które możemy użyć, aby przekazać elementy koszyka zakupów do naszego kodu bez naszych metoda konieczności uzyskania bezpośredniego dostępu do kontrolki GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

W naszym pliku MyShoppingCart.aspx.cs firma Microsoft może używać tej struktury w naszej obsługi zdarzeń kliknij przycisk aktualizacji w następujący sposób. Należy pamiętać, że oprócz aktualizowanie koszyka zaktualizujemy również Suma koszyka.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Należy zwrócić uwagę przy użyciu szczególne znaczenie w odniesieniu ten wiersz kodu:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() to funkcja pomocnika specjalne, który wprowadzimy w MyShoppingCart.aspx.cs w następujący sposób.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Zapewnia to czyste możliwość dostępu do wartości elementów powiązania w naszym kontrolki GridView. Ponieważ naszych formant pola wyboru "Usuń element" nie jest powiązana firma Microsoft będzie do niego dostęp za pomocą metody FindControl().

Na tym etapie, podczas tworzenia projektu przygotowujemy się do zaimplementowania rozpoczęcie procesu realizowania zamówienia.

Przed ten sposób możemy należy użyć programu Visual Studio, aby wygenerować członkowskiej bazie danych, a następnie dodanie użytkownika do repozytorium członkostwa.

> [!div class="step-by-step"]
> [Poprzednie](tailspin-spyworks-part-4.md)
> [dalej](tailspin-spyworks-part-6.md)
