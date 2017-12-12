---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: "Część 5: Logika biznesowa | Dokumentacja firmy Microsoft"
author: JoeStagner
description: "Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 5 dodaje niektórych logiki biznesowej."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e205788e05a2ad94d86d4847c11c40898b1c3113
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="part-5-business-logic"></a>Część 5: Logika biznesowa
====================
przez [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks pokazano, jak bardzo proste jest tworzenie zaawansowanych, skalowalnych aplikacji dla platformy .NET. Przedstawia on poza jak nowe, fantastyczne funkcje programu ASP.NET 4 do tworzenia sklepu online, łącznie z zakupów, wyewidencjonowania i administracji.
> 
> Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 5 dodaje niektórych logiki biznesowej.


## <a id="_Toc260221671"></a>Dodanie niektórych logika biznesowa

Chcemy wiemy z doświadczenia zakupów mają być dostępne, gdy ktoś odwiedzi witryny sieci web. Osoby odwiedzające będą mogli przeglądania i Dodaj elementy do koszyka, nawet jeśli nie są zarejestrowane lub zalogowany. Gdy są one gotowe do wyewidencjonowania otrzymają oni możliwość uwierzytelnienia i jeśli nie są jeszcze elementy członkowskie będą oni mogli utworzyć konto.

Oznacza to, że musimy zaimplementować logiki można przekonwertować koszyka z anonimowego stanu na stan "Zarejestrowany użytkownik".

Teraz Utwórz katalog o nazwie "Klasy", a następnie kliknij prawym przyciskiem myszy w folderze i utworzyć nowe "Class" pliku o nazwie MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Jak wcześniej wspomniano, firma Microsoft będzie rozszerzenie klasy, która implementuje stronę MyShoppingCart.aspx i firma Microsoft będzie to zrobić przy użyciu. Konstrukcja zaawansowane "klasy częściowej" przez sieć.

Wywołanie wygenerowany dla naszych pliku MyShoppingCart.aspx.cf wygląda następująco.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Zwróć uwagę na użycie słowa kluczowego "częściowej".

Plik klasy, który mamy wygenerował wygląda następująco.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Firma Microsoft zostaną scalone naszych implementacje przez dodanie do tego pliku, a także partial — słowo kluczowe.

Naszego nowego pliku klasy teraz wygląda następująco.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

Pierwsza metoda, który zostanie dodany do naszej klasy jest metoda "AddItem". Jest to metoda, który ostatecznie zostanie wywołany, gdy użytkownik kliknie łącza "Dodaj do grafik" strony Lista produktów i szczegółowe informacje.

Dołącz do przy użyciu następujących instrukcji w górnej części strony.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

I Dodaj tę metodę do klasy MyShoppingCart.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Aby zobaczyć, czy element jest już koszyka korzystamy LINQ to Entities. Jeśli tak, możemy zaktualizować ilość zamówienia elementu, w przeciwnym razie utworzymy nowy wpis dla wybranego elementu

Aby można było wywołać tę metodę wprowadzimy strony AddToCart.aspx nie tylko klasy tej metody, ale następnie wyświetlane bieżące koszyka = po dodaniu elementu.

Kliknij prawym przyciskiem myszy nazwę rozwiązania w Eksploratorze rozwiązań i Dodaj i nową stronę o nazwie AddToCart.aspx, jak firma Microsoft wcześniej zrobione.

Gdy ta strona może służy do wyświetlania wyników pośrednich niski problemów standardowych itd., w naszym implementacji, strony rzeczywiście renderowania, ale raczej wywoła logiki "Dodaj" i przekierowania.

W tym celu dodamy poniższy kod do strony\_zdarzeń obciążenia.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Należy pamiętać, że trwa pobieranie produktu do dodania do koszyka parametr QueryString i wywołanie metody AddItem klasy Nasze.

Zakładając, że żadne błędy nie zostaną napotkane kontrola jest przekazywana do strony SHoppingCart.aspx, która pełni wprowadzimy w polu. Jeśli ma to być błąd możemy Zgłoś wyjątek.

Obecnie firma Microsoft ma nie zaimplementowano jeszcze obsługi błędów ogólnych tego wyjątku przejdzie nieobsługiwany przez naszą aplikację, ale firma Microsoft będzie wkrótce to rozwiązać.

Należy też zauważyć, użyj instrukcji Debug.Fail() (dostępne za pośrednictwem`using System.Diagnostics;)`

Jest aplikacja jest uruchomiona w debugerze, ta metoda wyświetli okno dialogowe szczegółowe informacje o stanie aplikacji oraz komunikat o błędzie, który jest określona.

Podczas pracy w środowisku produkcyjnym instrukcji Debug.Fail() jest ignorowana.

Można zauważyć w kodzie powyżej wywołanie do metody w naszym zakupów nazw klas koszyka "GetShoppingCartId".

Dodaj kod, aby zaimplementować metodę w następujący sposób.

Należy pamiętać, że również dodaliśmy przyciski aktualizacji i wyewidencjonowania i etykiety, której można wyświetlić koszyka "Suma".

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Można teraz dodać elementy do naszego koszyka, ale nie wdrożonych logikę do wyświetlenia koszyka, po dodaniu produktu.

Tak na stronie MyShoppingCart.aspx dodamy formantem obiektu EntityDataSource i GridVire w następujący sposób.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Wywołanie formularza w projektancie, dzięki czemu można dwukrotnie kliknij przycisk Aktualizuj koszyk i generowanie obsługi zdarzeń kliknięcia, który określono w deklaracji w znaczniku.

Firma Microsoft będzie później zaimplementować szczegóły, ale w ten sposób zostanie Daj nam kompilowanie i uruchamianie aplikacji bez błędów.

Po uruchomieniu aplikacji i Dodaj element do koszyka zobaczysz to.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Należy pamiętać, że firma Microsoft odpowiadają regułom z widoku siatki "domyślne" zaimplementowanie trzy kolumny niestandardowe.

Pierwsza to edytowalna, pole "Powiązane" ilość:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

Następne jest kolumnę "obliczeniową", która wyświetla całkowitą element wiersza (element koszt razy może zostać określona ilość):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Na koniec mamy niestandardowych kolumny, która zawiera formant wyboru, który użytkownik zostanie służy do wskazywania, czy można usunąć elementu z planu zakupów.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Jak widać, zamówienia, więc warto całkowita wiersz jest pusty Dodaj logikę można obliczyć całkowitą kolejności.

Firma Microsoft będzie najpierw implementacji metody "GetTotal" do naszej klasy MyShoppingCart.

W pliku MyShoppingCart.cs Dodaj poniższy kod.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Następnie na stronie\_można Zadzwonimy naszych GetTotal — metoda obsługi zdarzeń obciążenia. W tym samym czasie dodamy test, aby sprawdzić, czy koszyk jest pusty i odpowiednio wyświetlania, jeśli jest.

Obecnie Jeśli koszyk jest pusty uzyskujemy to:

![](tailspin-spyworks-part-5/_static/image4.jpg)

A jeśli nie, widzimy naszych razem.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Jednak ta strona nie jest jeszcze ukończone.

Potrzebujemy dodatkową logikę ponownego obliczenia koszyka przez usunięcie elementów, które zostały oznaczone do usunięcia i określając nowe wartości ilości jako część mogły zostać zmienione w siatce przez użytkownika.

Umożliwia dodawanie metody "RemoveItem" do klasy Nasze koszyka zakupów w MyShoppingCart.cs do obsługi w przypadku, gdy użytkownik oznacza element do usunięcia.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Teraz załóżmy ad metody obsługi sytuacji, gdy użytkownik po prostu zmienia jakości może zostać określona w widoku GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Z podstawowymi funkcjami aktualizacja i usuwanie w miejscu możemy wdrożyć logikę, która faktycznie aktualizuje koszyk w bazie danych. (W MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Będzie należy pamiętać, że ta metoda oczekuje dwóch parametrów. Jeden element jest koszyk Id, a drugi jest Tablica obiektów typu zdefiniowanego przez użytkownika.

Aby zminimalizować zależności logiki szczegółowych interfejsu użytkownika, firma Microsoft zdefiniowaniu struktury danych, które firma Microsoft może używać do przekazywania elementy koszyka zakupów do naszego kodu bez naszych metody konieczności bezpośredni dostęp do kontrolki widoku siatki.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

W naszym pliku MyShoppingCart.aspx.cs możemy użyć tej struktury w naszym obsługi zdarzeń kliknij przycisk aktualizacji w następujący sposób. Należy pamiętać, że oprócz aktualizowanie koszyka przeprowadzimy aktualizację do całości koszyka.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Należy pamiętać o szczególne znaczenie ten wiersz kodu:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() jest funkcja pomocnika specjalne wprowadzimy w MyShoppingCart.aspx.cs w następujący sposób.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

To zapewnia czystą sposób uzyskać dostęp do wartości elementów powiązania w naszym kontrolki widoku siatki. Ponieważ naszych formant wyboru "Usuń element" nie jest powiązany firma Microsoft będzie do niego dostęp za pomocą metody FindControl().

Na tym etapie tworzenia projektu przygotowujemy się do realizacji procesu realizacji transakcji.

Przed dokonaniem teraz użyć programu Visual Studio do generowania bazy danych członkostwa i dodać użytkownika do repozytorium członkostwa.

>[!div class="step-by-step"]
[Poprzednie](tailspin-spyworks-part-4.md)
[dalej](tailspin-spyworks-part-6.md)
