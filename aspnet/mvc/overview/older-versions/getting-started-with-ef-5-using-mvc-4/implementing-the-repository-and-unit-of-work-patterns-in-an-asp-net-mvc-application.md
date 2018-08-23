---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Wdrażanie z repozytorium i jednostki pracy w aplikacji ASP.NET MVC (9, 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 20f82744582faddaffdc5b4785bf208ecf0955a7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755619"
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Wdrażanie z repozytorium i jednostki pracy w aplikacji ASP.NET MVC (9, 10)
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Można uruchomić tej serii samouczka od początku lub [pobrać projekt startowy w tym rozdziale](building-the-ef5-mvc4-chapter-downloads.md) i zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli napotkasz problem, nie można rozpoznać [Pobieranie ukończone rozdział](building-the-ef5-mvc4-chapter-downloads.md) i spróbuj odtworzyć problem. Rozwiązanie tego problemu można znaleźć zwykle porównując swój kod, aby kompletny kod. Niektóre typowe błędy i sposobu rozwiązania tych problemów można znaleźć [błędów i rozwiązania problemu.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


W poprzednim samouczku użyto dziedziczenia zmniejszyć nadmiarowy kod w `Student` i `Instructor` klas jednostek. W tym samouczku zobaczysz niektóre sposoby korzystania z repozytorium i jednostki pracy wzorce dla operacji CRUD. Tak jak w poprzednim samouczku w tym jednym poznasz, jak zmienić sposób działania swojego kodu za pomocą stron można już utworzyć zamiast tworzenia nowych stron.

## <a name="the-repository-and-unit-of-work-patterns"></a>Repozytorium i jednostki pracy

Repozytorium i jednostki pracy wzorce są przeznaczone do tworzenia warstwę abstrakcji od warstwy dostępu do danych i warstwy logiki biznesowej aplikacji. Implementacji tych wzorców może pomóc ochronić aplikację przed zmianami w magazynie danych i może ułatwić automatyczne testy jednostkowe i Programowanie oparte na testach (TDD).

W tym samouczku będziesz implementuje klasę repozytorium dla każdego typu jednostki. Aby uzyskać `Student` typu jednostki, utworzysz interfejs repozytorium i klasę repozytorium. Podczas tworzenia wystąpienia repozytorium w kontrolerze użyjesz interfejsu tak, aby kontroler będzie akceptować odwołania do dowolnego obiektu, który implementuje interfejs repozytorium. Po uruchomieniu na serwerze sieci web kontrolera odbiera repozytorium, która współdziała z programu Entity Framework. Po uruchomieniu kontroler w obszarze klasę testu jednostkowego odbiera repozytorium, który działa z danymi przechowywanymi w taki sposób, że możesz łatwo manipulować do testowania, takie jak pamięci w pamięci.

W dalszej części tego samouczka użyjesz wielu repozytoriów i jednostki pracy klasa `Course` i `Department` wpisuje jednostki `Course` kontrolera. Klasa pracy jednostki koordynuje pracy z wieloma repozytoriami, tworząc pojedynczą bazę danych klasy kontekstu, współużytkowane przez wszystkie z nich. Jeśli chcesz mieć możliwość wykonania automatycznych testów jednostkowych, można będzie tworzenie i używanie interfejsów dla tych klas w taki sam sposób jak w `Student` repozytorium. Jednak w celu uproszczenia tego samouczka można tworzyć i używać tych klas bez interfejsów.

Poniższa ilustracja przedstawia sposób wyobrażenie relacje między kontrolerem i klasy kontekst, w porównaniu do nie używa repozytorium lub jednostki wzorzec pracy na wszystkich.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

Testy jednostkowe nie utworzy w tej serii samouczków. Aby zapoznać się z TDD z aplikacją MVC, która używa wzorca repozytorium, zobacz [Instruktaż: przy użyciu TDD ze wzorca ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Aby uzyskać więcej informacji na temat wzorca repozytorium zobacz następujące zasoby:

- [Wzorzec repozytorium](https://msdn.microsoft.com/library/ff649690.aspx) w witrynie MSDN.
- [Przy użyciu wzorców repozytorium i jednostki pracy za pomocą programu Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) na blogu zespołu programu Entity Framework.
- [Agile Entity Framework 4 repozytorium](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) serię wpisów w blogu Julie Lerman.
- [Tworzenie konta w aplikacji HTML5/jQuery pierwszy rzut oka](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) na blogu Dan Wahlin.

> [!NOTE]
> Istnieje wiele sposobów implementowania repozytorium i jednostki pracy. Można użyć klasy repozytorium, z lub bez niego jednostkę pracy klasy. Możesz zaimplementować pojedynczego repozytorium na potrzeby wszystkie typy jednostek i jeden dla każdego typu. W przypadku zastosowania dla każdego typu można użyć osobnych klas, rodzajowego klasy podstawowej i klasy pochodne lub abstrakcyjną klasę bazową i klasach pochodnych. Można uwzględnić logikę biznesową w repozytorium lub ograniczyć jego logiką dostępu do danych. Można również wbudować warstwę abstrakcji do klasy kontekstu bazy danych, za pomocą [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) interfejsy istnieje zamiast [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) typów dla zestawów jednostek. Podejścia do wdrażania warstwę abstrakcji przedstawiona w tym samouczku jest jedną z opcji należy rozważyć nie zalecenie dla wszystkich scenariuszach i środowiskach.


## <a name="creating-the-student-repository-class"></a>Tworzenie klasy repozytorium dla uczniów

W *DAL* folderze utwórz plik klasy o nazwie *IStudentRepository.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Ten kod deklaruje typowy zestaw metod CRUD, w tym dwie metody odczytu — taki, który zwraca wszystkie `Student` jednostki a, który umożliwia znalezienie jednej `Student` jednostki według identyfikatora.

W *DAL* folderze utwórz plik klasy o nazwie *StudentRepository.cs* pliku. Zastąp istniejący kod poniższym kodem, który implementuje `IStudentRepository` interfejsu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

Kontekst bazy danych jest zdefiniowany w zmiennej klasy i konstruktora oczekuje, że obiekt wywołujący, aby przekazać wystąpienia kontekstu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Można utworzyć wystąpienia nowy kontekst w repozytorium, ale następnie użycie wielu repozytoriów w jednym kontrolerze każdego pojawiłyby się oddzielny kontekst. Później użyjesz wielu repozytoriów w `Course` kontrolera, a zobaczysz, jak jednostki pracy klasy można zagwarantować, że wszystkie repozytoria przy użyciu ten sam kontekst.

Implementuje repozytorium [interfejsu IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) i usuwa kontekst bazy danych, jak zanotowane wcześniej z kontrolera, a jego metod CRUD wykonywać wywołania kontekst bazy danych w taki sam sposób, który wcześniej.

## <a name="change-the-student-controller-to-use-the-repository"></a>Zmień kontroler uczniów do użycia w repozytorium

W *StudentController.cs*, Zastąp kod aktualnie w klasie, z następującym kodem. Zmiany są wyróżnione.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Kontroler teraz deklaruje zmienną klasy dla obiektu, który implementuje `IStudentRepository` interfejsu, a nie z klasy kontekstu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

Domyślnego (bezparametrowego) konstruktora tworzy nowe wystąpienie kontekstu i opcjonalnie konstruktor umożliwia obiektowi wywołującemu Przekaż wystąpienie kontekstu.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Jeśli używano *wstrzykiwanie zależności*, lub DI, nie należy konstruktora domyślnego, ponieważ oprogramowanie DI będzie upewnij się, obiekt poprawnego repozytorium zawsze będą dostarczane.)

W metodach CRUD repozytorium jest teraz nazywana zamiast kontekstu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

I `Dispose` metoda teraz usuwa repozytorium zamiast kontekstu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Uruchamianie witryny, a następnie kliknij przycisk **studentów** kartę.

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

Strona wygląda i działa tak samo jak przed zmieniony kod, aby użyć repozytorium i innych stron dla uczniów również działać tak samo. Dostępna jest ważna różnica w sposobie `Index` metody kontrolera jest filtrowanie i kolejności. Oryginalną wersję tej metody zawiera następujący kod:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

Zaktualizowany interfejs `Index` metoda zawiera następujący kod:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Wyróżniony kod został zmieniony.

W pierwotnej wersji kodu `students` jest `IQueryable` obiektu. Zapytanie nie jest wysyłana do bazy danych, aż zostanie przekonwertowany do kolekcji przy użyciu metody, takie jak `ToList`, która nie występuje aż widok indeksu uzyskuje dostęp do modelu dla uczniów. `Where` Staje się metody w powyższym kodzie oryginalnym `WHERE` klauzuli w kwerendzie SQL, które są wysyłane do bazy danych. Z kolei oznacza, że tylko wybrane jednostki są zwracane przez bazę danych. Jednak w wyniku zmiany `context.Students` do `studentRepository.GetStudents()`, `students` zmiennej po tej instrukcji `IEnumerable` kolekcję zawierającą wszystkich studentów w bazie danych. Efekt zastosowania `Where` metoda jest taka sama, ale teraz praca odbywa się w pamięci na serwerze sieci web, a nie przez bazę danych. Dla zapytań zwracających duże ilości danych może to być mało wydajne.

> [!TIP]
> 
> **Element IQueryable programu vs. IEnumerable**
> 
> Po zaimplementowaniu repozytorium jak pokazano poniżej, nawet wtedy, gdy element wprowadź **wyszukiwania** okno zapytania wysyłane do programu SQL Server zwraca wszystkie wiersze dla uczniów, ponieważ nie zawiera kryteria wyszukiwania:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> To zapytanie zwraca wszystkie dane dla uczniów, ponieważ repozytorium wykonywane zapytanie, nie wiedząc o tym dotyczącą kryteriów wyszukiwania. Proces sortowania, stosując kryteria wyszukiwania i wybierając podzbiór danych dla stronicowania (w tym przypadku wyświetlanie tylko 3 wiersze) odbywa się w pamięci po późniejszym `ToPagedList` wywoływana jest metoda `IEnumerable` kolekcji.
> 
> W poprzedniej wersji kodu (przed zastosowaniem repozytorium), zapytania nie są wysyłane do bazy danych, aż po zastosowaniu kryteriów wyszukiwania podczas `ToPagedList` jest wywoływana w `IQueryable` obiektu.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Gdy wywoływana jest ToPagedList `IQueryable` obiekt zapytania wysyłane do programu SQL Server określa ciąg wyszukiwania, w wyniku zwracane są tylko wiersze, które spełniają kryteria wyszukiwania i filtrowanie nie musi odbywać się w pamięci.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (Następującego samouczka wyjaśniono, jak sprawdzić zapytania wysyłane do programu SQL Server).


W poniższej sekcji pokazano, jak implementować metody repozytorium, które umożliwiają określenie, że ta praca ma się odbywać przez bazę danych.

Utworzono warstwę abstrakcji między kontrolerem i kontekst bazy danych programu Entity Framework. Jeśli zostały zamierza przeprowadzić zautomatyzowane testy jednostkowe za pomocą tej aplikacji, można utworzyć klasę alternatywnych repozytorium w projekcie testu jednostki, która implementuje `IStudentRepository` *.* Zamiast wywoływania kontekst do odczytu i zapisu danych, ta klasa makiety repozytorium można manipulować kolekcji w pamięci w celu przetestowania funkcje kontrolera.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Implementowanie ogólnego repozytorium i jednostki pracy, klasa

Tworzenie klasy repozytorium dla każdego typu jednostki może spowodować wiele nadmiarowy kod, a może to spowodować aktualizacje częściowe. Na przykład załóżmy, że musisz zaktualizować dwa typy różne jednostki w ramach tej samej transakcji. Jeśli każda używa wystąpienie kontekstu oddzielnej bazy danych, jeden może się powieść, a druga może zakończyć się niepowodzeniem. Jednym ze sposobów, aby zminimalizować nadmiarowy kod jest użycie ogólnego repozytorium i jednym ze sposobów, aby upewnić się, że wszystkie repozytoria używać ten sam kontekst bazy danych (a zatem skoordynować wszystkie aktualizacje) jest użycie jednostki pracy klasy.

W tej części samouczka utworzysz `GenericRepository` klasy i `UnitOfWork` klasy i używać ich w `Course` kontrolera dostęp zarówno do `Department` i `Course` zestawy jednostek. Jak wyjaśniono wcześniej, w celu uproszczenia tej części samouczka, możesz nie są Tworzenie interfejsów dla tych klas. Ale były ma być używane do ułatwienia TDD, można będzie zazwyczaj wdrożyć je przy użyciu interfejsów taki sam sposób jak `Student` repozytorium.

### <a name="create-a-generic-repository"></a>Tworzenie ogólnego repozytorium

W *DAL* folderze utwórz *GenericRepository.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Zmienne klasy są deklarowane w kontekście bazy danych, a dla zestawu jednostek, który jest skonkretyzowany repozytorium:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

Konstruktor akceptuje wystąpienie kontekstu bazy danych i inicjuje zmienną zestaw jednostek:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

`Get` Metoda używa wyrażenia lambda, aby umożliwić kod wywołujący, aby określić warunek filtru i kolumny, aby uporządkować wyniki według, a parametr ciągu umożliwia obiekt wywołujący, podaj rozdzielana przecinkami lista właściwości nawigacji dla wczesne ładowanie:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

Kod `Expression<Func<TEntity, bool>> filter` oznacza, że obiekt wywołujący zapewni Wyrażenie lambda, na podstawie `TEntity` typu, a to wyrażenie zwróci wartość logiczną. Na przykład, jeśli repozytorium jest skonkretyzowany `Student` typu jednostki, kod w metodzie wywołujący może określić `student => student.LastName == "Smith` &quot; dla `filter` parametru.

Kod `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` oznacza również, że obiekt wywołujący zapewni wyrażenia lambda. Ale w tym przypadku dane wejściowe używając wyrażenia `IQueryable` dla obiektu `TEntity` typu. Wyrażenie zwróci uporządkowane wersję tego `IQueryable` obiektu. Na przykład, jeśli repozytorium jest skonkretyzowany `Student` typu jednostki, kod w metodzie wywołujący może określić `q => q.OrderBy(s => s.LastName)` dla `orderBy` parametru.

Kod w `Get` metoda tworzy `IQueryable` obiektu, a następnie stosuje wyrażenie filtru, jeśli istnieje:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Następnie stosuje wyrażeń eager ładowania po analizie rozdzielana przecinkami lista:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Na koniec stosuje `orderBy` wyrażenia, jeśli istnieje i zwraca wyniki; w przeciwnym razie zwraca wyniki z nieuporządkowaną kwerendy:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Gdy wywołujesz `Get` metody, można było przeprowadzać filtrowanie i sortowanie zawartości `IEnumerable` kolekcji zwróconej przez metodę zamiast podawać parametrów dla tych funkcji. Ale sortowanie i filtrowanie następnie będą wykonywane w pamięci na serwerze sieci web. Za pomocą tych parametrów, należy upewnić się, prace wykonywane przez bazy danych, a nie na serwerze sieci web. Alternatywą jest tworzenie klas pochodnych dla typów w określonej jednostce i dodaj specjalne `Get` metod, takich jak `GetStudentsInNameOrder` lub `GetStudentsByName`. Jednak w złożonych aplikacji, może to spowodować dużej liczby takich klas pochodnych i specjalne metody, które może być więcej pracy do obsługi.

Kod w `GetByID`, `Insert`, i `Update` metody jest podobny do przedstawionego w repozytorium nieogólnego. (Nie są podając parametr wczesne ładowanie w `GetByID` podpisu, ponieważ nie można tego zrobić wczesne ładowanie z `Find` metody.)

Dwa przeciążenia są dostarczane dla `Delete` metody:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Jedną z tych umożliwia przekazanej tylko identyfikator obiektu do usunięcia, a jedno wykorzystuje wystąpienia jednostki. Jak widać w [Obsługa współbieżności](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczek, należy możesz Obsługa współbieżności `Delete` metody, która przyjmuje wystąpienie jednostki, która obejmuje oryginalnej wartości właściwości śledzenia.

Ten ogólny repozytorium będzie obsługiwać typowych wymagań CRUD. Gdy typ określonego obiektu ma specjalne wymagania, takie jak bardziej złożone filtrowanie lub kolejności, można utworzyć klasy pochodnej, która ma dodatkowe metody dla tego typu.

## <a name="creating-the-unit-of-work-class"></a>Tworzenie jednostki pracy klasy

Klasa pracy jednostki służy jeden cel: aby upewnić się, korzystając z wieloma repozytoriami, współużytkują one kontekstu pojedynczej bazy danych. Po zakończeniu jednostka pracy w ten sposób można wywołać `SaveChanges` metody, w tym wystąpieniu kontekstu i mieć pewność, wszystkie powiązane zmiany zostaną skoordynowany. Wszystko to jest klasa potrzeb `Save` metod i właściwości dla każdego repozytorium. Każda właściwość repozytorium Zwraca wystąpienie repozytorium, które zostały utworzone przy użyciu tego samego wystąpienia bazy danych w kontekście co inne wystąpienia repozytorium.

W *DAL* folderze utwórz plik klasy o nazwie *UnitOfWork.cs* i Zastąp kod szablonu poniższym kodem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

Ten kod tworzy zmienne klasy kontekstu bazy danych i każdego repozytorium. Aby uzyskać `context` zmiennej, zostanie uruchomiony nowy kontekst:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Każda właściwość repozytorium sprawdza, czy repozytorium już istnieje. W przeciwnym razie metoda tworzy repozytorium, przekazując to wystąpienie kontekstu. W rezultacie wszystkie repozytoria współużytkują to samo wystąpienie kontekstu.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

`Save` Wywołania metody `SaveChanges` w kontekście bazy danych.

Wszystkie klasy, która tworzy w zmiennej klasy kontekstu bazy danych, takich jak `UnitOfWork` klasy implementuje `IDisposable` i usuwa kontekst.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Zmiana kontroler kurs klasy UnitOfWork i repozytoriów

Zastąp kod w *CourseController.cs* następującym kodem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Ten kod dodaje zmienną klasy `UnitOfWork` klasy. (Jeśli była używana interfejsów w tym miejscu, w takich sytuacjach przydałaby zainicjować zmienną, w tym miejscu; zamiast tego możesz również zaimplementować wzorzec dwa konstruktory tak, jak została ona `Student` repozytorium.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

W pozostałej części klasy, wszystkie odwołania do kontekstu bazy danych są zastępowane przez odwołania do odpowiedniego repozytorium przy użyciu `UnitOfWork` właściwości, aby uzyskać dostępu do repozytorium. `Dispose` Usuwa metoda `UnitOfWork` wystąpienia.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Uruchamianie witryny, a następnie kliknij przycisk **kursów** kartę.

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

Strona wygląda i działa tak samo jak wcześniej zmiany i innych stron kurs również działać tak samo.

## <a name="summary"></a>Podsumowanie

Udało Ci się teraz wdrożyć repozytorium i jednostki pracy. Wyrażenia lambda używanych jako parametry metody w ogólnego repozytorium. Aby uzyskać więcej informacji o sposobie używania tych wyrażeń z `IQueryable` obiektu, zobacz [IQueryable(T) interfejsu (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) w bibliotece MSDN. W następnym samouczku dowiesz się, jak obsługiwać niektórych zaawansowanych scenariuszy.

Linki do innych zasobów platformy Entity Framework można znaleźć w [Mapa zawartości dostępu do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
