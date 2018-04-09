---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Część 7: Dodawanie funkcji | Dokumentacja firmy Microsoft'
author: JoeStagner
description: Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 7 dodaje dodatkowe funkcje, takie jak konto okienko...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 17f068155f6726047901e2f7d580d375a4e07c87
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-7-adding-features"></a>Część 7: Dodawanie funkcji
====================
przez [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks pokazano, jak bardzo proste jest tworzenie zaawansowanych, skalowalnych aplikacji dla platformy .NET. Przedstawia on poza jak nowe, fantastyczne funkcje programu ASP.NET 4 do tworzenia sklepu online, łącznie z zakupów, wyewidencjonowania i administracji.
> 
> Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 7 dodaje dodatkowe funkcje, takie jak konto przeglądu, recenzje produktów i "popularnych elementy" i kontrolek użytkownika "również zakupionych".


## <a id="_Toc260221673"></a>  Dodawanie funkcji

Chociaż użytkownicy mogą przeglądać naszego katalogu, należy umieścić elementów w ich koszyk i ukończyć proces wyewidencjonowania, Brak funkcji obsługi wielu możemy uwzględni zwiększające naszej witrynie.

1. Przegląd konta (Lista zleceń umieszczone oraz szczegóły).
2. Dodaj zawartość określonego kontekstu, pierwsza strona.
3. Dodawanie funkcji, aby umożliwić użytkownikom przeglądu produktów w katalogu.
4. Tworzenie formantu użytkownika do wyświetlania popularnych elementów i miejsce, które kontrolują na stronie witryny.
5. Tworzenie formantu użytkownika "Zakupu" i dodaj go do strony szczegółów produktu.
6. Dodawanie kontaktu strony.
7. Dodaj informacje o stronie.
8. Błąd globalne

## <a id="_Toc260221674"></a>  Przegląd konta

W folderze "Konto" Utwórz dwie strony .aspx jedną o nazwie OrderList.aspx i innych OrderDetails.aspx o nazwie

OrderList.aspx będzie korzystać kontrolki GridView i EntityDataSoure, ile mamy wcześniej.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

EntityDataSoure wybiera rekordy z tabeli zamówienia filtrowane wg nazwy użytkownika (zobacz WhereParameter) który mamy ustawiony w zmiennej sesji użytkownikowi logowanie użytkownika.

Należy też zauważyć tych parametrów w pole hiperłącza HyperlinkField GridView:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Określają one łącza do każdego produktu, określając pola OrderID jako parametr QueryString na stronie OrderDetails.aspx w widoku szczegółów zamówienia.

## <a id="_Toc260221675"></a>  OrderDetails.aspx

Używamy obiektu EntityDataSource kontroli dostępu do zamówienia i FormView wyświetlać dane kolejności i innego obiektu EntityDataSource z widoku GridView do wyświetlenia wszystkich kolejność pozycji.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

W pliku kodu za (OrderDetails.aspx.cs) mamy dwa małego bity celów.

Najpierw należy upewnić się, że SzczegółyZamówień zawsze pobiera OrderId.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

Musimy także obliczania i wyświetlania kolejności całkowita z pozycji wiersza.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  Strona główna

Na stronie Default.aspx Dodajmy niektórych zawartość statyczną.

Najpierw będzie utworzyć folderu "Content" i jego folderu obrazów (i będzie umieścić obraz ma być używany na stronie głównej.)

W zastępczym dolnej części strony Default.aspx Dodaj następujący kod znaczników.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  Recenzje produktów

Najpierw dodamy przycisk z łączem do formularza, który możemy użyć do wprowadzania przeglądu produktu.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Należy pamiętać, możemy przekazywanie ProductID w ciągu zapytania

Następny Dodajmy stronę o nazwie ReviewAdd.aspx

Ta strona będzie używać zestawu ASP.NET AJAX kontroli narzędzi. Jeśli użytkownik jeszcze nie, aby pobrać go z [DevExpress](http://devexpress.com/act) i wskazówki dotyczące konfigurowania zestawu narzędzi do użytku z programem Visual Studio tutaj [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

W trybie projektowania przeciągnij formanty i modułów weryfikacji z przybornika i kompilacji podobny do poniższego formularza.

![](tailspin-spyworks-part-7/_static/image2.jpg)

Kod znaczników będzie wyglądać następująco.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Teraz, możemy wprowadzić recenzji, służy do wyświetlania tych przeglądami na stronie produktu.

Na stronie ProductDetails.aspx, Dodaj ten kod znaczników.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

Uruchomienie teraz naszej aplikacji i przechodząc do produktu zawiera informacje o produkcie tym recenzje klientów.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  Formant popularnych elementów (Tworzenie kontrolki użytkownika)

W celu zwiększenia sprzedaży w witrynie sieci web dodamy kilka funkcji "sugerujących sprzedaje" produktów popularnych lub pokrewne.

Pierwszy te funkcje będą listę najpopularniejszych produktu w naszym katalogu produktów.

Utworzymy "Kontrola użytkownika" do wyświetlenia sprzedających się elementy na stronie głównej naszej aplikacji. Ponieważ będzie to formantu, możemy użyć go na każdej stronie, po prostu przeciągania i upuszczania formantu w projektancie programu Visual Studio na każdej stronie, które firma Microsoft, takich jak.

W Eksploratorze rozwiązań programu Visual Studio kliknij prawym przyciskiem myszy nazwę rozwiązania i Utwórz nowy katalog o nazwie "Formanty". Mimo że nie jest konieczne to zrobić, możemy pomóc, Zachowaj naszych projektu, tworząc naszych kontrolek użytkownika w katalogu "Formanty".

Kliknij prawym przyciskiem myszy w folderze formantów, a następnie wybierz pozycję "Nowy element":

![](tailspin-spyworks-part-7/_static/image4.jpg)

Określ nazwę dla naszych formantu "PopularItems". Należy pamiętać, że rozszerzenie pliku dla formantów użytkownika .ascx nie aspx.

Mamy kontroli popularnych użytkownika elementy zostaną zdefiniowane w następujący sposób.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

W tym miejscu używamy metody, które firma Microsoft nie zostały jeszcze użyte w tej aplikacji. Używamy kontrolce elementu powtarzanego i zamiast kontroli źródła danych możemy jest powiązanie kontrolce elementu powtarzanego z wyników zapytania składnika LINQ to Entities.

W kodzie mamy kontroli przejdziemy w następujący sposób.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Należy też zauważyć wierszem ważne w górnej części znaczników naszych formantu.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Ponieważ większość popularnych elementów nie zmiana na podstawie minutę na minutę dodamy bóle dyrektywy poprawić wydajność aplikacji. Ta dyrektywa spowoduje, że kod formanty można wykonywać tylko po wygaśnięciu pamięci podręcznej danych wyjściowych formantu. W przeciwnym razie będzie używać pamięci podręcznej wersji wyjście formantu.

Teraz wszystko, co mamy zrobić to objąć naszą stronę Default.aspc naszego nowego formantu.

Użyj przeciągnij i upuść można umieścić w kolumnie Otwórz naszych domyślnego formularza wystąpienia formantu.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Teraz możemy uruchamiania aplikacji na stronę główną Wyświetla najpopularniejszych elementy.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  "Zakupu" kontroli (kontrolek użytkownika z parametrami)

Drugi formant użytkownika, który utworzymy potrwa sugerujących, sprzedaży na następny poziom, dodając kontekstu szczegółowością.

Logika obliczyć pierwszych elementów "Zakupu" jest nieuproszczony.

Mamy kontroli "Zakupu" Wybierz rekordy SzczegółyZamówień (wcześniej kupił) dla aktualnie wybranego ProductID, a następnie pobrania OrderIDs dla każdego zlecenia unikatowy, która znajduje się.

Następnie firma Microsoft będzie wybierać al produktów z tych zleceń i Suma zakupione ilości. Firma Microsoft będzie sortować produkty Suma ilość i Wyświetl 5 pierwszych elementów.

Biorąc pod uwagę złożoność tę logikę, wprowadzimy tego algorytmu jako procedury składowanej.

T-SQL dla procedury składowanej ma następującą składnię.

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Należy pamiętać, że tej procedury składowanej (SelectPurchasedWithProducts) istniał w bazie danych, gdy firma Microsoft uwzględnione w naszej aplikacji i firma Microsoft wygenerowany modelu danych jednostki, możemy określić oprócz tabele i widoki, które firma Microsoft potrzeby modelu danych jednostki powinna zawierać tę procedurę składowaną.

Dostęp do procedury składowanej z modelu danych jednostki, należy zaimportować funkcji.

Kliknij dwukrotnie modelu danych jednostki w Eksploratorze rozwiązań, aby otworzyć go w Projektancie i Otwórz przeglądarkę z modelu, a następnie kliknij prawym przyciskiem myszy w Projektancie i wybierz opcję "Dodaj importu funkcji".

![](tailspin-spyworks-part-7/_static/image1.png)

W ten sposób spowoduje otwarcie okna dialogowego.

![](tailspin-spyworks-part-7/_static/image2.png)

Wypełnij pola, jak pokazano powyżej, wybierając "SelectPurchasedWithProducts" i użyj nazwy procedury nazwę naszych importowanych funkcji.

Kliknij przycisk "Ok".

To to, które firma Microsoft może po prostu program względem procedury składowanej jak firma Microsoft może innego elementu w modelu.

Tak w folderze naszych "Formanty" Utwórz kontrolkę użytkownika o nazwie AlsoPurchased.ascx.

Kod znaczników dla tego formantu wyglądają bardzo dostosowanym do sterowania PopularItems.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

Znacząca różnica jest który są nie pamięci podręcznej danych wyjściowych, ponieważ do renderowania elementu będą się różnić od produktu.

ProductId będzie "property" do formantu.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

W obsłudze zdarzeń PreRender formantu możemy bkość, aby wykonać trzy czynności.

1. Upewnij się, że ustawiono ProductID.
2. Zobacz, czy wszystkie produkty, które zostały zakupione z bieżącej.
3. Dane wyjściowe niektóre elementy, jak określono w #2.

Należy zwrócić uwagę, jak łatwo jest wywołać procedurę składowaną za pośrednictwem modelu.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Po ustaleniu, które są "również zakupione" możemy po prostu powiązać powtarzanego wyników zwróconych przez kwerendę.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

Jakby nie było żadnych elementów "zakupu" będzie wystarczy wyświetlić innych popularnych elementów z naszego katalogu.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Aby wyświetlić elementy "Zakupu", otwórz stronę ProductDetails.aspx i przeciągnij formant AlsoPurchased w Eksploratorze rozwiązań, aby był on wyświetlany w tym miejscu w znaczniku.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

W ten sposób utworzyć odwołanie do formantu w górnej części strony ProductDetails.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Ponieważ kontrola użytkownika AlsoPurchased wymaga podania liczby ProductId możemy ustawi właściwość ProductID naszych formantu przy użyciu instrukcji Eval względem bieżącego elementu modelu danych strony.

![](tailspin-spyworks-part-7/_static/image3.png)

Gdy firma Microsoft kompilacji i uruchomić teraz i przejdź do produktu widzimy elementów "Zakupu".

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Poprzednie](tailspin-spyworks-part-6.md)
> [dalej](tailspin-spyworks-part-8.md)
