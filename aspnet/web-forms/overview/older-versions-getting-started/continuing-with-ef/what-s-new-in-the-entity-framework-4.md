---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: What's New in programu Entity Framework 4.0 | Dokumentacja firmy Microsoft
author: tdykstra
description: W tej serii samouczków opiera się na aplikacji sieci web firmy Contoso University, utworzony przez wprowadzenie do serii samouczków Entity Framework 4.0. CZY MOGĘ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 2a6d2ddd6fdfb418cbb948d0a3ffefc8452d6127
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363173"
---
<a name="whats-new-in-the-entity-framework-40"></a>What's New in programu Entity Framework 4.0
====================
przez [Tom Dykstra](https://github.com/tdykstra)

> W tej serii samouczków jest oparta na Contoso University aplikacji sieci web, który jest tworzony przez [rozpoczęcie korzystania z programu Entity Framework](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serii samouczków. Jeśli nie została ukończona wcześniej samouczki, jako punkt początkowy na potrzeby tego samouczka możesz [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) będzie utworzony. Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tworzone przez zakończenie serii samouczków. Jeśli masz pytania dotyczące samouczków, możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


W poprzednim samouczku pokazano niektóre metody maksymalizowanie wydajności aplikacji sieci web, który używa programu Entity Framework. Ten samouczek zawiera przegląd niektórych Najważniejsze nowe funkcje w wersji 4 programu Entity Framework, a następnie łączy on zasobów, które oferują bardziej szczegółowy wprowadzenie do wszystkich nowych funkcji. Następujące funkcje wyróżnione w ramach tego samouczka:

- Klucz obcy skojarzenia.
- Wykonywanie poleceń SQL zdefiniowane przez użytkownika.
- Pierwszy model programowania.
- Obsługa POCO.

Ponadto krótko wprowadzi samouczka *najpierw kod rozwoju*, funkcja, która zostanie wprowadzona w kolejnej wersji programu Entity Framework.

Uruchom samouczek, uruchom program Visual Studio i otwarcie aplikacji sieci web firmy Contoso University pracowano w poprzednim samouczku.

## <a name="foreign-key-associations"></a>Skojarzenia klucza obcego

Entity Framework w wersji 3.5 dołączone właściwości nawigacji, ale nie zawierało właściwości klucza obcego w modelu danych. Na przykład `CourseID` i `StudentID` kolumn `StudentGrade` tabeli zostaną pominięte z `StudentGrade` jednostki.

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

Przyczyna tego podejścia była, mówiąc ściślej, kluczy obcych są szczegółowo fizyczna implementacja i nie powinny znajdować się w modelu koncepcyjnego danych. Jednak w praktyce, jest często łatwiej jest pracować z jednostkami w kodzie, gdy masz bezpośredni dostęp do kluczy obcych.

Na przykład jak obcych kluczy w modelu danych można uprościć kod, należy wziąć pod uwagę sposób trzeba było kodowi *DepartmentsAdd.aspx* strony bez nich. W `Department` jednostki `Administrator` właściwość ma klucz obcy, który odpowiada `PersonID` w `Person` jednostki. Aby możliwe było nawiązanie skojarzenie między nowy dział i jego administratora, wszystkie trzeba było robić została ustawiona wartość `Administrator` właściwość `ItemInserting` program obsługi zdarzeń kontrolki z danymi:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Bez klucze obce w modelu danych, możesz obsługiwać `Inserting` zdarzeń do kontroli źródła danych zamiast `ItemInserting` zdarzeń kontrolki z danymi, aby pobrać odwołanie do jednostki, sama przed jednostka zostanie dodana do zestawu jednostek. W przypadku tego odwołania jest nawiązywane skojarzenia przy użyciu kodu, tak jak w następujących przykładach:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Jak widać w zespole platformy Entity Framework [wpis w blogu na klucza obcego skojarzenia](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), są inne przypadki, w którym różnica w złożoność kodu jest dużo większy. Aby zaspokoić potrzeby tych, którzy wolą przy użyciu szczegółów implementacji w model koncepcyjny danych w celu uproszczenia kodu, platformy Entity Framework teraz z opcją, w tym klucze obce w modelu danych.

W terminologii programu Entity Framework, jeśli zawiera klucze obce w modelu danych używasz *skojarzenia klucza obcego*, i jeśli wykluczasz klucze obce używasz *niezależnych skojarzenia*.

## <a name="executing-user-defined-sql-commands"></a>Wykonywanie poleceń SQL zdefiniowane przez użytkownika

We wcześniejszych wersjach programu Entity Framework nie było żadnych łatwy sposób tworzenia własnych poleceń SQL na bieżąco i uruchom je. Entity Framework generowana dynamicznie poleceń SQL dla Ciebie albo było konieczne tworzenie procedur składowanych i zaimportuj go jako funkcję. W wersji 4 dodaje `ExecuteStoreQuery` i `ExecuteStoreCommand` metody `ObjectContext` klasę, która ułatwić przekazywać dowolne zapytanie bezpośrednio do bazy danych.

Załóżmy, że administratorzy Contoso University chcesz móc wykonywać zbiorcze zmiany w bazie danych bez konieczności przechodzenia przez proces tworzenia procedury składowanej i importowanie go do modelu danych. Jest ich pierwsze żądanie dla strony, która umożliwi zmianę liczby kredytów na wszystkie kursy w bazie danych. Na stronie sieci web chcą mieć możliwość wprowadzania numer na potrzeby Mnoży wartość każdego `Course` wiersza `Credits` kolumny.

Utwórz nową stronę, która używa *Site.Master* strony wzorcowej i nadaj mu nazwę *UpdateCredits.aspx*. Następnie dodaj następujący kod do `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Tworzy ten kod znaczników `TextBox` kontrolki, w którym użytkownik może wprowadzić wartość mnożnika `Button` kontrolki kliknij, aby wykonać polecenie, a `Label` kontrola określającą liczbę uwzględnionych wierszy.

Otwórz *UpdateCredits.aspx.cs*i dodaj następującą `using` instrukcji i procedury obsługi dla przycisku `Click` zdarzeń:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Ten kod jest wykonywany SQL `Update` polecenia przy użyciu wartości w polu tekstowym i użyto etykiety, aby wyświetlić liczbę uwzględnionych wierszy. Przed uruchomieniem strony uruchamiania *Courses.aspx* strony w celu uzyskania obrazu "before" niektórych danych.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Uruchom *UpdateCredits.aspx*wprowadź "10" jako mnożnik, a następnie kliknij przycisk **Execute**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Uruchom *Courses.aspx* strony ponownie, aby zobaczyć zmienione dane.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Jeśli chcesz ustawić liczbę środki na korzystanie z powrotem do ich oryginalnych wartości w *UpdateCredits.aspx.cs* zmienić `Credits * {0}` do `Credits / {0}` i uruchom ponownie na stronie wprowadzenie 10 co dzielnik.)

Aby uzyskać więcej informacji na temat wykonywania zapytań, które definiujesz w kodzie, zobacz [instrukcje: bezpośrednie wykonywanie poleceń względem źródła danych](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Pierwszy model programowania

Te przewodniki służy do wcześniejszego utworzenia bazy danych i następnie generowane model danych oparty na strukturze bazy danych. W programie Entity Framework 4 możesz zamiast tego uruchomić przy użyciu modelu danych i wygenerować bazę danych na podstawie struktury modelu danych. Jeśli tworzysz aplikację, dla którego jeszcze nie istnieje baza danych podejście modelu pozwala na tworzenie jednostek i relacji, które mają sens koncepcyjnie dla aplikacji, podczas nie martwiąc się o szczegóły implementacji fizycznych . (To pozostaje prawdziwy wyłącznie za pośrednictwem początkowych etapów rozwoju, jednak. Po pewnym czasie baza danych zostanie utworzony i będzie mieć danych produkcyjnych w nim i odtworzenia go z modelu nie będzie już praktyczne; w tym momencie będziesz mieć do podejście bazy danych.)

W tej części samouczka możesz utworzyć model danych proste i wygenerować bazę danych z niego.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *DAL* i wybierz polecenie **Dodaj nowy element**. W **Dodaj nowy element** dialogowego **zainstalowane szablony** wybierz **danych** , a następnie wybierz **ADO.NET Entity Data Model** szablonu . Nadaj nowemu plikowi *AlumniAssociationModel.edmx* i kliknij przycisk **Dodaj**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Spowoduje to uruchomienie Kreatora modelu danych jednostki. W **wybierz zawartość modelu** kroku, wybierz pozycję **pusty Model** a następnie kliknij przycisk **Zakończ**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

**Projektancie Entity Data Model** zostanie otwarty przy użyciu powierzchni projektowej puste. Przeciągnij **jednostki** elementu z **przybornika** na powierzchnię projektu.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Zmień nazwę jednostki z `Entity1` do `Alumnus`, zmień `Id` nazwę właściwości, aby `AlumnusId`i Dodaj nową właściwość skalarna, o nazwie `Name`. Aby dodać nowe właściwości naciśnięciu klawisza Enter po zmianie nazwy `Id` kolumn, lub kliknij prawym przyciskiem myszy jednostkę i wybierz pozycję **Dodaj właściwość skalarną**. Domyślnym typem dla nowej właściwości jest `String`, co jest dobrym rozwiązaniem w tej prezentacji prostego, ale oczywiście zmienisz elementów, takich jak typ danych w **właściwości** okna.

Utwórz inną jednostkę w taki sam sposób i nadaj mu `Donation`. Zmiana `Id` właściwości `DonationId` i Dodaj właściwość skalarną o nazwie `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Dodaj skojarzenie między tymi dwoma jednostkami, kliknij prawym przyciskiem myszy `Alumnus` jednostki, wybierz opcję **Dodaj**, a następnie wybierz pozycję **skojarzenie**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Domyślne wartości w **Dodawanie skojarzenia** to okno dialogowe, co ma (jeden do wielu, zawierają właściwości nawigacji, obejmują klucze obce), więc wystarczy kliknąć **OK**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

Projektant dodaje linia skojarzenia i właściwości klucza obcego.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Teraz możesz przystąpić do tworzenia bazy danych. Kliknij prawym przyciskiem myszy projekt powierzchni i wybierz **Generuj z bazy danych z modelu**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Spowoduje to uruchomienie Kreatora generowanie bazy danych. (Jeśli wyświetlane są ostrzeżenia, które wskazują, że jednostki nie są mapowane, możesz zignorować te, które w chwili obecnej.)

W **wybierz połączenie danych** kroku, kliknij przycisk **nowe połączenie**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

W **właściwości połączenia** okna dialogowego Wybierz lokalne wystąpienie programu SQL Server Express, a nazwa bazy danych `AlumniAsssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Kliknij przycisk **tak** kiedy pojawi się prośba Jeśli chcesz utworzyć bazę danych. Gdy **wybierz połączenie danych** kroku zostanie ponownie wyświetlony, kliknij przycisk **dalej**.

W **Podsumowanie i ustawienia** kroku, kliknij przycisk **Zakończ**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

A *.sql* zostanie utworzony plik za pomocą poleceń (DDL) języka definicji danych, ale polecenia nie zostało jeszcze uruchomione.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Użyj narzędzia takiego jak **SQL Server Management Studio** do uruchamiania skryptu i tworzenia tabel, ponieważ może zrobisz podczas tworzenia `School` bazy danych dla [pierwszym samouczku tej serii samouczka Wprowadzenie ](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (Jeśli nie można pobrać bazy danych.)

Teraz możesz używać `AlumniAssociation` modelu danych w sieci web pages taki sam sposób, jak korzystasz `School` modelu. Aby wypróbować tę możliwość, Dodaj dane do tabel i utworzyć stronę sieci web, który wyświetla dane.

Za pomocą **Eksploratora serwera**, Dodaj następujące wiersze do `Alumnus` i `Donation` tabel.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Utwórz nową stronę sieci web o nazwie *Alumni.aspx* , który używa *Site.Master* strony wzorcowej. Dodaj następujący kod do `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Ten kod znaczników tworzy zagnieżdżonych `GridView` kontrolki zewnętrznej jeden do wyświetlania nazw absolwentów i wewnętrznego katalog w celu wyświetlania darowizn dat i kwot.

Otwórz *Alumni.aspx.cs*. Dodaj `using` poufności danych dostęp do warstwy i obsługa zewnętrzny `GridView` kontrolki `RowDataBound` zdarzeń:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Ten kod databinds wewnętrzny `GridView` sterować za pomocą `Donations` właściwość nawigacji bieżącego wiersza `Alumnus` jednostki.

Uruchom stronę.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Uwaga: na tej stronie znajduje się w projekcie do pobrania, ale aby umożliwić jej pracę, użytkownik musi utworzyć bazę danych w lokalnym wystąpieniu programu SQL Server Express; baza danych nie jest uwzględniona jako *.mdf* w pliku *aplikacji\_ Dane* folderu.)

Aby uzyskać więcej informacji o korzystaniu z funkcji modelu Entity Framework, zobacz [pierwszego modelu w programie Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>Obsługa obiektów POCO

Gdy używasz metodologię projektowania opartego na domenie, projektowania klas danych, które reprezentują dane i zachowania, która jest odpowiednia dla domeny biznesowej. Te klasy powinny być niezależne od wszelkich określonych technologii używany do przechowywania (utrwalanie) danych; innymi słowy powinny one być *trwałości zakresu*. Nieznajomości trwałości może również ułatwić klasy testu jednostkowego ponieważ projekt testów jednostkowych mogą używać niezależnie od technologii trwałości jest najwygodniejszym do testowania. Ograniczona obsługa nieznajomości trwałości jest oferowana w starszych wersjach programu Entity Framework, ponieważ musiały dziedziczyć z klas jednostek `EntityObject` klasy, a zatem się dużym stopniem Entity Framework funkcje.

Entity Framework 4 wprowadza możliwość używania klas jednostek, które nie dziedziczą `EntityObject` klasy, dlatego trwałości zakresu. W kontekście programu Entity Framework klasy takie są zwykle nazywane *obiektów CLR stary zwykły* (POCO lub POCOs). Można napisać klasy POCO ręcznie lub może automatycznie generować je na podstawie istniejącego modelu danych przy użyciu szablonów Toolkit przekształcania szablonu tekstu (T4) dostarczane przez program Entity Framework.

Aby uzyskać więcej informacji o używaniu POCOs platformy Entity Framework Zobacz następujące zasoby:

- [Praca z jednostkami POCO](https://msdn.microsoft.com/library/dd456853.aspx). To jest dokument MSDN, który zawiera omówienie POCOs, wraz z łączami do innych dokumentów, które zawierają bardziej szczegółowe informacje.
- [Wskazówki: Obiektów POCO szablonu programu Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) to wpis w blogu od zespołu deweloperów platformy Entity Framework i zawiera łącza do innych wpisów w blogu o POCOs.

## <a name="code-first-development"></a>Najpierw kod tworzenia

Obsługa POCO w programie Entity Framework 4 nadal wymaga Tworzenie modelu danych, a następnie połączyć z klasami jednostki do modelu danych. Nowa wersja programu Entity Framework będzie zawierać funkcję o nazwie *najpierw kod rozwoju*. Ta funkcja umożliwia za pomocą programu Entity Framework własnych klas POCO bez konieczności używania Projektant modelu danych lub plik XML modelu danych. (W związku z tym, ta opcja również została wywołana *tylko kod*; *najpierw kod* i *tylko kod* odnoszą się do tej samej funkcji programu Entity Framework.)

Aby uzyskać więcej informacji o korzystaniu z podejścia najpierw kod do rozwoju, zobacz następujące zasoby:

- [Tworzenie aplikacji przy użyciu platformy Entity Framework 4 najpierw kod](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). To jest wpis na blogu autorstwa Scotta Guthriego wprowadzenie do programowania najpierw kod.
- [Blog zespołu Entity Framework projektowania - publikuje oznakowane CodeOnly](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Entity Framework rozwoju zespołu - przeczytać wpisy na blogu oznakowane Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [Samouczek platformy MVC Music Store — część 4: modele i dostęp do danych](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Wprowadzenie do MVC 3 — część 4: Entity Framework najpierw kod tworzenia](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Ponadto nowe samouczek MVC najpierw kod, który tworzy aplikację, podobnie jak w aplikacji Contoso University jest rzutowany do opublikowania wiosną 2011 w [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Więcej informacji

Na tym kończy się przegląd, aby what's new in Entity Framework i ten proces jest kontynuowany z tej serii samouczka platformy Entity Framework. Aby uzyskać więcej informacji o nowych funkcjach w programie Entity Framework 4, które nie są omówione w tym miejscu zobacz następujące zasoby:

- [What's New in ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx) temacie w witrynie MSDN na temat nowych funkcji w wersji 4 programu Entity Framework.
- [Ogłoszenie wydania Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) wpis w blogu zespołu programistycznego platformy Entity Framework nowe funkcje w wersji 4.

> [!div class="step-by-step"]
> [Poprzednie](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
