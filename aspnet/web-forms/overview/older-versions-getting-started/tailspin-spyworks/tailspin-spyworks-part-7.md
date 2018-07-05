---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Część 7: Dodawanie funkcji | Dokumentacja firmy Microsoft'
author: JoeStagner
description: W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 7 dodaje dodatkowe funkcje, takie jak okienko konta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 8cdde10981835877e5ac2f65860010920a68d0a2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389179"
---
<a name="part-7-adding-features"></a>Część 7: Dodawanie funkcji
====================
przez [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks pokazuje, jak bardzo łatwo jest tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET. Przedstawia on poza sposób użycia wspaniałych nowych funkcjach w ASP.NET 4 do tworzenia sklep online, m.in. zakupy wyewidencjonowanie i Administracja.
> 
> W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 7 dodaje dodatkowe funkcje, takie jak konta przeglądu, recenzje produktów i "Najpopularniejsze pozycje" i formanty użytkownika "również zakupione".


## <a id="_Toc260221673"></a>  Dodawanie funkcji

Chociaż użytkownicy mogą przeglądać naszego wykazu, umieść elementy w swoich koszyka zakupów i ukończenie procesu realizowania zamówienia, istnieje szereg obsługi funkcji, że firma Microsoft dołącza do udoskonalać naszą witrynę.

1. Przegląd konta (Lista zamówień umieszczone i wyświetlić szczegółowe informacje).
2. Dodaj część zawartości określonego kontekstu do pierwszej strony.
3. Dodaj funkcję, aby umożliwić użytkownikom przeglądanie produktów, w katalogu.
4. Tworzenie kontrolki użytkownika do wyświetlania na pierwszej stronie Najpopularniejsze pozycje i miejsce, które kontrolują.
5. Tworzenie kontrolki użytkownika "Również zakupione" i dodaj go do strony szczegółów produktu.
6. Dodaj kontakt strony.
7. Dodaj informacje o stronie.
8. Błąd globalne

## <a id="_Toc260221674"></a>  Przegląd konta

W folderze "Konto" Utwórz dwie strony .aspx, jeden o nazwie OrderList.aspx i nazwanych OrderDetails.aspx

OrderList.aspx będzie korzystać z kontrolki GridView i EntityDataSoure, ile mamy wcześniej.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

EntityDataSoure wybiera rekordy z tabeli Orders filtrowane według nazwy użytkownika (zobacz WhereParameter), które możemy ustawić w zmiennej sesji użytkownikowi logowanie użytkownika.

Należy pamiętać, również tych parametrów w pole hiperłącza HyperlinkField GridView:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Określają one łącze do widoku szczegółów zamówienia dla każdego produktu, określając pole OrderID jako parametr QueryString na stronie OrderDetails.aspx.

## <a id="_Toc260221675"></a>  OrderDetails.aspx

Użyjemy EntityDataSource kontroli dostępu do zamówienia i FormView wyświetlane dane zamówień i innym EntityDataSource przy użyciu GridView do wyświetlenia wszystkich kolejności pozycji.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

W pliku kodu za (OrderDetails.aspx.cs) mamy dwa bity nieco celów.

Najpierw musimy upewnić się, że OrderDetails zawsze pobiera OrderId.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

Musimy również obliczają i wyświetlają kolejności całkowitej z elementów wiersza.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  Strona główna

Dodajmy zawartości statycznej do strony Default.aspx.

Najpierw będzie utworzyć folderu "Zawartość" i wewnątrz tego folderu obrazów (i będzie umieścić obraz ma być używany na stronie głównej)

Do symbolu zastępczego dolnej części strony Default.aspx Dodaj następujący kod znaczników.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  Recenzje produktów

Najpierw dodamy przycisk za pomocą łącza do formularza, które firma Microsoft może służyć do wprowadzania Recenzja produktu.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Należy pamiętać, firma Microsoft przekazywanie ProductID w ciągu zapytania

Dalej Dodajmy stronę o nazwie ReviewAdd.aspx

Ta strona będzie używać ASP.NET AJAX Control Toolkit. Jeśli użytkownik jeszcze tego nie zrobiono, aby pobrać go z [DevExpress](http://devexpress.com/act) i wskazówki dotyczące konfigurowania zestawu narzędzi do użytku z programem Visual Studio tutaj [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

W trybie projektowania przeciągnij formanty i modułów weryfikacji z przybornika i budowania formularza podobny do poniższego.

![](tailspin-spyworks-part-7/_static/image2.jpg)

Znaczniki będą wyglądać mniej więcej tak.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Teraz, że możemy wprowadzić recenzje, pozwala wyświetlać te przeglądy na stronie produktu.

Na stronie ProductDetails.aspx, Dodaj ten kod znaczników.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

Uruchomione obecnie nasza aplikacja i przechodząc do produktu zawiera informacje o produkcie, łącznie z przeglądy wykonywane przez klientów.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  Najpopularniejsze pozycje kontroli (Tworzenie kontrolki użytkownika)

Aby zwiększyć sprzedaż w witrynie sieci web dodamy kilka funkcji "dwuznaczne sell" popularnych lub powiązanych produktów.

Pierwsza z tych funkcji będzie lista więcej popularnych produktów w naszym katalogu produktów.

Utworzymy "Kontrolkę użytkownika" Aby wyświetlić najlepiej sprzedające elementów na stronie głównej naszej aplikacji. Ponieważ są to kontrolki, możemy użyć go na dowolnej stronie usługi, po prostu przeciąganie i upuszczanie formantu w projektancie programu Visual Studio na każdej stronie, które firma Microsoft, takich jak.

W Eksploratorze rozwiązań programu Visual Studio kliknij prawym przyciskiem myszy nazwę rozwiązania, a następnie utwórz nowy katalog o nazwie "Controls". Mimo że nie jest to konieczne to zrobić, firma Microsoft ułatwi naszego projektu, tworząc naszych kontrolki użytkownika w katalogu "Controls".

Kliknij prawym przyciskiem myszy w folderze kontroli i wybierz pozycję "Nowy element":

![](tailspin-spyworks-part-7/_static/image4.jpg)

Określ nazwę dla naszych formantu "PopularItems". Należy pamiętać, że rozszerzenie pliku, w przypadku kontrolek użytkownika .ascx nie .aspx.

Nasze kontrolki popularne użytkownika elementy będą zdefiniowane w następujący sposób.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

Tutaj używamy metody, które firma Microsoft nie ma jeszcze używane w tej aplikacji. Używamy kontrolce elementu powtarzanego i zamiast korzystać z kontroli źródła danych możemy konieczności utworzenia powiązania w kontrolce elementu powtarzanego z wynikami zapytaniu składnika LINQ to Entities.

W kodzie naszej kontroli czynność tę możemy wykonać w następujący sposób.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Należy też zauważyć ten wiersz ważne w górnej części naszych kontroli znaczników.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Ponieważ najpopularniejszych elementów, nie będzie zmiana na podstawie minutę na minutę możemy dodać dyrektywę bóle, aby poprawić wydajność aplikacji. Ta dyrektywa spowoduje, że kod formantów, aby wykonać tylko po wygaśnięciu pamięci podręcznej danych wyjściowych formantu. W przeciwnym razie będą używane w pamięci podręcznej wersji wyjście formantu.

Teraz musimy to zrobić będzie zawierać naszej nowej kontrolki na naszej stronie Default.aspc.

Użyj przeciągnij i upuść do umieszczenia wystąpienie formantu w kolumnie Otwórz naszych domyślnego formularza.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Firma Microsoft uruchamiania naszej aplikacji na stronie głównej wyświetli teraz najpopularniejszych elementów.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  "Również zakupione" sterowania (kontrolek użytkownika za pomocą parametrów)

Drugi formant użytkownika, który utworzymy potrwa dwuznaczne sprzedaży na wyższy poziom, dodając kontekstu szczegółowością.

Logiki można obliczyć pierwszych elementów "Również zakupione" jest trywialny.

Mamy kontroli "Również zakupione" będzie wybrać rekordy OrderDetails (wcześniej kupił) dla aktualnie wybranego elementu ProductID i Pobierz OrderIDs dla każdego zamówienia unikatowy, która znajduje się.

Następnie firma Microsoft będzie wybrać al produkty z tych zamówień i sumę ilości zakupu. Utworzymy posortować produkty według tej sumy liczby i wyświetlić pięć pierwszych elementów.

Biorąc pod uwagę złożoność tę logikę, wprowadzimy ten algorytm jako procedurę przechowywaną.

T-SQL dla procedury składowanej jest w następujący sposób.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Należy pamiętać, że tej procedury składowanej (SelectPurchasedWithProducts) istniał w bazie danych podczas dołączyliśmy w naszej aplikacji i możemy generowane określonej, oprócz tabele i widoki, które Musieliśmy, modelu Entity Data Model modelu Entity Data Model powinien zawierać tę procedurę składowaną.

Dostęp do procedury składowanej z modelu Entity Data Model należy zaimportować funkcji.

Kliknij dwukrotnie modelu Entity Data Model w Eksploratorze rozwiązań, aby otworzyć go w Projektancie i Otwórz przeglądarkę modelu, a następnie prawym przyciskiem myszy projektanta i wybierz pozycję "Dodaj funkcję Import".

![](tailspin-spyworks-part-7/_static/image1.png)

To spowoduje otwarcie tego okna dialogowego.

![](tailspin-spyworks-part-7/_static/image2.png)

Wypełnij pola jak pokazano powyżej, wybierając "SelectPurchasedWithProducts", a następnie użyj nazwy procedury nazwa naszej funkcji zaimportowane.

Kliknij przycisk "Ok".

To to, którą firma Microsoft może po prostu programować przy użyciu procedury składowanej, ponieważ firma Microsoft może być inny element w modelu.

Tak w folderze naszych "Controls" Utwórz nową kontrolkę użytkownika o nazwie AlsoPurchased.ascx.

Kod znaczników dla tej kontrolki będzie wyglądać bardzo podobnie do kontroli PopularItems.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

Godne uwagi różnica polega na tym, które są nie pamięci podręcznej danych wyjściowych od czasu do renderowania elementu różnią się według produktu.

Identyfikator produktu będzie "property" do formantu.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

W obsłudze zdarzenie PreRender kontrolki firma Microsoft eed, aby wykonać trzy czynności.

1. Upewnij się, że ustawiono ProductID.
2. Zobacz, czy wszystkie produkty, w których została zakupiona z bieżącej subskrypcji.
3. Dane wyjściowe niektóre elementy, jak określono w #2.

Należy pamiętać o tym, jak łatwo jest wywołanie procedury składowanej za pomocą modelu.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Po ustaleniu, które są "również zakupione" możemy po prostu powiązać powtarzanego wyników zwróconych przez zapytanie.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

Jakby nie było żadnych elementów "również zakupione" będzie wyświetlana po prostu innych popularnych elementów z naszego wykazu.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Aby wyświetlić elementy "Również zakupione", otwórz stronę ProductDetails.aspx, a następnie przeciągnij formant AlsoPurchased z poziomu Eksploratora rozwiązań, aby była ona wyświetlana w tym miejscu w znaczniku.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

Ten sposób utworzyć odwołanie do formantu w górnej części strony ProductDetails.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Ponieważ AlsoPurchased kontrolki użytkownika musi zawierać cyfry ProductId, firma Microsoft ustawi właściwości ProductID naszych formantu przy użyciu instrukcji Eval względem bieżącego elementu modelu danych strony.

![](tailspin-spyworks-part-7/_static/image3.png)

Gdy do skompilowania i uruchom teraz i przejdź do produktu widzimy elementów "Również zakupione".

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Poprzednie](tailspin-spyworks-part-6.md)
> [dalej](tailspin-spyworks-part-8.md)
