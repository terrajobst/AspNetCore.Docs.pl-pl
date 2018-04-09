---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: Wyświetlanie tabeli bazy danych (VB) | Dokumentacja firmy Microsoft
author: microsoft
description: W tym samouczku pokazują I wyświetlanie zestaw rekordów bazy danych na dwa sposoby. Wyświetlić dwóch metod formatowania zestaw rekordów bazy danych w formacie HTML ta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: dd871520f98aaae2d7b33d83b1646eb9eee88821
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="displaying-a-table-of-database-data-vb"></a>Wyświetlanie tabeli bazy danych (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> W tym samouczku pokazują I wyświetlanie zestaw rekordów bazy danych na dwa sposoby. Wyświetlić dwóch metod formatowania zestaw rekordów bazy danych w tabeli HTML. Najpierw należy wyświetlić sposób można sformatować rekordów bazy danych bezpośrednio w widoku. Następnie I pokazują, jak można korzystać z częściowe podczas formatowania rekordów bazy danych.


Celem tego samouczka jest wyjaśnienie, jak można wyświetlić tabeli HTML danych bazy danych w aplikacji platformy ASP.NET MVC. Najpierw zostanie przedstawiony sposób korzystania z narzędzi szkieletów uwzględnione w programie Visual Studio do generowania widoku, który automatycznie wyświetla zestaw rekordów. Następnie zostanie przedstawiony sposób użycia częściowym jako szablon podczas formatowania rekordów bazy danych.

## <a name="create-the-model-classes"></a>Tworzenie klasy modelu

Zamierzamy, aby wyświetlić zestaw rekordów z tabeli bazy danych filmów. W tabeli bazy danych filmy zawiera następujące kolumny:

<a id="0.4_table01"></a>


| **Nazwa kolumny** | **Typ danych** | **Dopuszcza wartości null** |
| --- | --- | --- |
| Id | int | False |
| Tytuł | Nvarchar(200) | False |
| Dyrektor | NVarchar(50) | False |
| DateReleased | DataGodzina | False |


Aby można było przedstawić tabeli filmów w naszej aplikacji ASP.NET MVC, należy utworzyć klasę modelu. W tym samouczku używamy Microsoft Entity Framework do tworzenia klas naszych modelu.

> [!NOTE] 
> 
> W tym samouczku używamy Microsoft Entity Framework. Jest jednak wziąć pod uwagę, że można użyć szereg różnych technologii wchodzić w interakcje z bazy danych z aplikacji platformy ASP.NET MVC, w tym składniku LINQ to SQL, NHibernate lub ADO.NET.


Wykonaj następujące kroki, aby uruchomić Kreatora modelu danych jednostki:

1. Kliknij prawym przyciskiem myszy folder modeli w oknie Eksploratora rozwiązań i wybierz opcję menu **Dodaj, nowy element**.
2. Wybierz **danych** kategorii i wybierz **modelu danych jednostki ADO.NET** szablonu.
3. Nadaj nazwę modelu danych *MoviesDBModel.edmx* i kliknij przycisk **Dodaj** przycisku.

Po kliknięciu przycisku Dodaj zostanie wyświetlony Kreator modelu danych jednostki (zobacz rysunek 1). Wykonaj następujące kroki, aby zakończyć działanie kreatora:

1. W **wybierz zawartość modelu** krok, wybierz opcję **generowania z bazy danych** opcji.
2. W **wybierz połączenie danych** krok, użyj *MoviesDB.mdf* połączenie danych i nazwy *MoviesDBEntities* dla ustawień połączenia. Kliknij przycisk **dalej** przycisku.
3. W **wybierz obiekty bazy danych użytkownika** krok, rozwiń węzeł tabel, zaznacz tabelę filmów. Wprowadź przestrzeń nazw *modele* i kliknij przycisk **Zakończ** przycisku.


[![Tworzenie LINQ w klasach SQL](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)

**Rysunek 01**: Tworzenie klasy LINQ do SQL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image2.png))


Po zakończeniu pracy Kreatora modelu danych jednostki, zostanie otwarty projektant modelu danych jednostki. Projektant powinien być wyświetlany jednostki filmów (patrz rysunek 2).


[![Projektant modelu danych jednostki](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)

**Rysunek 02**: Projektant modelu danych jednostki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image4.png))


Musimy upewnić jednej zmiany przed kontynuowaniem. Kreator danych jednostek generuje klasę modelu o nazwie *filmy* reprezentujący filmy tabeli bazy danych. Ponieważ użyjemy klasy filmów do reprezentowania określonego filmu, należy zmodyfikować nazwę klasy, która ma być *film* zamiast *filmy* (pojedynczej zamiast mnogiej).

Kliknij dwukrotnie nazwę klasy na powierzchni projektanta i Zmień nazwę klasy z filmów filmu. Po wprowadzeniu tej zmiany, kliknij przycisk **zapisać** przycisk (ikona dyskietki) do generowania klasy filmu.

## <a name="create-the-movies-controller"></a>Tworzenie kontrolera filmy

Teraz, gdy mamy sposób reprezentacji naszych danych bazy danych można utworzyć kontroler, który zwraca kolekcji filmów. W oknie programu Visual Studio Solution Explorer, kliknij prawym przyciskiem myszy folder kontrolery, a następnie wybierz opcję menu **Dodaj, kontrolera** (patrz rysunek 3).


[![Dodawanie Menu kontrolera](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)

**Rysunek 03**: Dodaj Menu kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image6.png))


Gdy **Dodaj kontroler** zostanie wyświetlone okno dialogowe, wprowadź nazwę kontrolera MovieController (patrz rysunek 4). Kliknij przycisk **Dodaj** przycisk, aby dodać nowy kontroler.


[![Okno dialogowe Dodawanie kontrolera](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)

**Rysunek 04**: okno dialogowe Dodaj kontroler ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image8.png))


Należy zmodyfikować akcję indeks() udostępnianych przez kontrolera film, tak aby zwracało zestaw rekordów bazy danych. Zmodyfikuj kontrolera wyglądającą jak kontrolera 1 wyświetlania.

**Wyświetlanie listy 1 – Controllers\MovieController.vb**

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

Wyświetlanie listy 1 klasa MoviesDBEntities jest używana do reprezentowania MoviesDB bazy danych. Wyrażenie *jednostek. MovieSet.ToList()* zwraca zestaw wszystkich filmów filmy tabeli bazy danych.

## <a name="create-the-view"></a>Tworzenie widoku

Najprostszym sposobem wyświetlenia zestaw rekordów bazy danych w tabeli HTML jest przeprowadzać szkieletów dostarczane przez program Visual Studio.

Tworzenie aplikacji przez wybranie opcji menu **kompilacji Kompiluj rozwiązanie**. Należy skompilować aplikację przed otwarciem **Dodaj widok** okna dialogowego lub klas danych nie będzie dłużej wyświetlane w oknie dialogowym.

Kliknij prawym przyciskiem myszy działanie indeks() i wybierz opcję menu **Dodaj widok** (patrz rysunek 5).


[![Dodawanie widoku](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)

**Rysunek 05**: Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image10.png))


W **Dodaj widok** okno dialogowe, zaznacz pole wyboru z etykietą **utworzyć widok jednoznacznie**. Wybierz klasę filmu jako **wyświetlić klasy danych**. Wybierz *listy* jako **wyświetlać zawartość** (patrz rysunek 6). Wybranie opcji wygeneruje silnie typizowanym widoku, który wyświetla listę filmów.


[![Okno dialogowe dodawania widoku](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)

**Rysunek 06**: okno dialogowe dodawania widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image12.png))


Po kliknięciu **Dodaj** przycisk, Widok wyświetlania 2 jest generowany automatycznie. Ten widok zawiera kod wymagany do iterowania po kolekcji filmów i wyświetlania każdej z właściwości filmu.

**Wyświetlanie listy 2 — Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

Można uruchomić aplikację, wybierając opcję menu **debugowania i Rozpocznij debugowanie** (lub naciśnięcie klawisza F5). Uruchamianie aplikacji uruchamia program Internet Explorer. Jeśli przejdziesz do adresu URL /Movie zostanie wyświetlone strony na rysunku 7.


[![Spis filmy](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)

**Rysunek 07**: Tabela filmów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-vb/_static/image14.png))


Jeśli nie lubisz niczego o wygląd siatki rekordów bazy danych na rysunku 7 można po prostu zmodyfikuj widok indeksu. Na przykład można zmienić *DateReleased* nagłówka do *Data wydania* przez zmodyfikowanie widoku indeksu.

## <a name="create-a-template-with-a-partial"></a>Utwórz szablon z częściowym

Jeśli widok pobiera zbyt skomplikowane, jest dobrym rozwiązaniem jest rozpocząć dzielenie widoku na częściowe. Przy użyciu częściowe umożliwia łatwiejsze do zrozumienia i obsługa widoków. Utworzymy częściowym który możemy użyć jako szablon do formatowania każdego filmu rekordów bazy danych.

Wykonaj następujące kroki, aby utworzyć częściowego:

1. Kliknij prawym przyciskiem myszy Views\Movie folder i wybierz opcję menu **Dodaj widok**.
2. Zaznacz pole wyboru z etykietą *tworzenia widoku częściowego (.ascx)*.
3. Nazwa częściowego *MovieTemplate*.
4. Zaznacz pole wyboru z etykietą **utworzyć widok jednoznacznie**.
5. Wybierz film jako *wyświetlić klasy danych*.
6. Wybierz opcję Pusta jako *wyświetlać zawartość*.
7. Kliknij przycisk **Dodaj** przycisk, aby dodać częściowego do projektu.

Po wykonaniu tych kroków należy zmodyfikować MovieTemplate częściowego, aby wyglądały jak lista 3.

**Wyświetlanie listy 3 — Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

Partial wyświetlania 3 zawiera szablon w pojedynczym wierszu rekordów.

Zmodyfikowany widok indeksu listę 4 korzysta z częściowa MovieTemplate.

**Wyświetlanie listy 4 — Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

Widok w listę 4 zawiera For każdej pętli, który iteruje po wszystkie filmy. Dla każdego filmu częściowe MovieTemplate jest używany do formatowania filmu. MovieTemplate jest renderowany przez wywołanie metody pomocnika RenderPartial().

Zmodyfikowany widok indeksu renderuje w tej samej tabeli HTML rekordów bazy danych. Jednak widoku znacznie uproszczono.


Metoda RenderPartial() jest inny niż w przypadku pozostałych metod pomocniczych, ponieważ nie zwraca ciąg. W związku z tym należy wywołać przy użyciu metody RenderPartial() &lt;Html.RenderPartial() %&gt; zamiast &lt;% = Html.RenderPartial() %&gt;.


## <a name="summary"></a>Podsumowanie

Celem tego samouczka jest wyjaśnienie, jak wyświetlić zestaw rekordów bazy danych w tabeli HTML. Najpierw przedstawiono sposób zwracania zestawu rekordów bazy danych z akcji kontrolera, korzystając z programu Entity Framework firmy Microsoft. Następnie przedstawiono sposób użycia szkieletów Visual Studio do generowania widoku, który automatycznie wyświetla kolekcję elementów. Ponadto przedstawiono sposób uproszczenia widoku dzięki wykorzystaniu częściowym. Przedstawiono sposób użycia częściowym jako szablon, dzięki czemu można sformatować każdego rekordu bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](creating-model-classes-with-linq-to-sql-vb.md)
> [dalej](performing-simple-validation-vb.md)
