---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Implementowanie repozytorium i jednostki pracy w aplikacji platformy ASP.NET MVC (9, 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1f870b61658686769304a7809bde62e66da3bd0c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Implementowanie repozytorium i jednostki pracy w aplikacji platformy ASP.NET MVC (9, 10)
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Samouczek serii można uruchomić od początku lub [pobrać projekt starter w tym rozdziale](building-the-ef5-mvc4-chapter-downloads.md) i zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli napotkasz problem, nie można rozwiązać, [Pobieranie ukończone rozdział](building-the-ef5-mvc4-chapter-downloads.md) i próba odtworzenia problemu. Rozwiązanie tego problemu można znaleźć ogólnie porównując swój kod kompletny kod. Dla niektórych typowych błędów i sposobu rozwiązania tych problemów, zobacz [błędów i rozwiązania problemu.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


W poprzednich instrukcji używane dziedziczenie Aby zmniejszyć nadmiarowy kod w `Student` i `Instructor` klas jednostek. W tym samouczku zobaczysz niektóre sposoby korzystania z repozytorium i jednostki pracy dla operacji CRUD. Jak w poprzednim samouczek w tym jednym będzie zmienić sposób kod działa ze stronami możesz już utworzone, a nie tworzenie nowych stron.

## <a name="the-repository-and-unit-of-work-patterns"></a>Repozytorium i jednostki pracy

Repozytorium i jednostki pracy są przeznaczone do tworzenia warstwy abstrakcji między Warstwa dostępu do danych i warstwy logiki biznesowej aplikacji. Implementacja tych wzorców może pomóc zabezpieczyć aplikacji od zmian w magazynie danych i może ułatwić automatyczne testy jednostkowe lub projektowanie oparte na (testach TDD).

W tym samouczku będziesz zaimplementować klasę repozytorium dla poszczególnych typów jednostek. Aby uzyskać `Student` utworzysz interfejsu repozytorium i klasę repozytorium typu jednostki. Gdy w kontrolerze tworzenia wystąpienia repozytorium, będzie używać interfejsu tak, aby kontroler przyjmuje odwołanie do każdego obiektu, który implementuje interfejs repozytorium. Po uruchomieniu na serwerze sieci web kontroler odbierze repozytorium, który działa z programu Entity Framework. Po uruchomieniu kontroler w obszarze klasę testów jednostkowych odbiera repozytorium, który działa z danych przechowywanych w taki sposób, aby łatwo można manipulować do testowania, takie jak pamięci w pamięci.

W dalszej części tego samouczka użyjesz wielu repozytoriów i jednostki pracy klasy dla `Course` i `Department` typy jednostek w `Course` kontrolera. Klasa pracy jednostki koordynuje pracy wielu repozytoriów za tworzenie współużytkowane przez wszystkie klasy kontekstu pojedynczej bazy danych. Jeśli chcesz mogą wykonywać testy jednostkowe automatycznych, czy tworzenie i używać interfejsów dla tych klas w taki sam sposób jak w `Student` repozytorium. Jednak aby samouczka proste, będzie tworzenie i używanie tych klas bez interfejsów.

Na poniższej ilustracji przedstawiono jeden ze sposobów conceptualize relacje między kontrolerem i klasy kontekst, w porównaniu do nie używa repozytorium lub jednostki pracy wzorca w ogóle.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

W tym samouczku nie tworzenia testów jednostkowych. Aby obejrzeć wprowadzenie do TDD przy użyciu aplikacji MVC, który korzysta ze wzorca repozytorium, zobacz [wskazówki: przy użyciu TDD z platformą ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Aby uzyskać więcej informacji na temat wzorca repozytorium zobacz następujące zasoby:

- [Wzorzec repozytorium](https://msdn.microsoft.com/library/ff649690.aspx) w witrynie MSDN.
- [Przy użyciu wzorców repozytorium i jednostki pracy z programu Entity Framework 4.0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) na blogu zespołu programu Entity Framework.
- [Elastyczne Entity Framework 4 repozytorium](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) serii wpisach w blogu Julie Lerman.
- [Tworzenie konta w aplikacji HTML5/jQuery oka](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) na blogu Dan Wahlin.

> [!NOTE]
> Istnieje wiele sposobów, aby zaimplementować repozytorium i jednostki pracy. Można użyć klasy repozytorium, z lub bez jednostki pracy klasy. Można zaimplementować jednym repozytorium dla wszystkich typów jednostek lub po jednej dla każdego typu. W przypadku zastosowania po jednej dla każdego typu, można użyć osobnych klas, ogólny klasy podstawowej i klasy pochodne lub abstrakcyjna klasa podstawowa i klas pochodnych. Można uwzględnić logiki biznesowej w repozytorium lub ograniczyć jego logika dostępu do danych. Można także utworzyć warstwę abstrakcji do własnej klasy kontekstu bazy danych, za pomocą [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) interfejsy zamiast [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) typy z zestawów jednostek. Podejście do implementowania warstwy abstrakcji przedstawiona w tym samouczku jest jedną z opcji należy rozważyć, nie zalecamy dla wszystkich scenariuszach i środowiskach.


## <a name="creating-the-student-repository-class"></a>Tworzenie klasy repozytorium dla użytkowników domowych

W *DAL* folderu, Utwórz plik klasy o nazwie *IStudentRepository.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Ten kod deklaruje typowy zestaw metod CRUD, w tym dwie metody odczytu —, który zwraca wszystkie `Student` jednostek i który znajduje się jeden `Student` jednostki według identyfikatora.

W *DAL* folderu, Utwórz plik klasy o nazwie *StudentRepository.cs* pliku. Zamień istniejący kod następujący kod, który implementuje `IStudentRepository` interfejsu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

Kontekst bazy danych jest zdefiniowany w zmiennej klasy i konstruktora oczekuje obiektu wywołującego umożliwia przekazywanie wystąpienia kontekstu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Można utworzyć wystąpienia nowy kontekst w repozytorium, ale następnie użycie wielu repozytoriów w jeden kontroler każdego pojawiłyby się oddzielny kontekst. Później będzie używać wielu repozytoriów w `Course` kontrolera, pojawi się jak jednostki pracy klasy można zagwarantować, że wszystkie repozytoria używać tego samego kontekstu.

Implementuje repozytorium [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) i usuwa kontekst bazy danych, jak widać wcześniej w kontrolerze, a jego metody CRUD wykonywania wywołań do kontekstu bazy danych w taki sam sposób, który był wyświetlany poprzednio.

## <a name="change-the-student-controller-to-use-the-repository"></a>Zmień kontroler uczniów do użytku w repozytorium

W *StudentController.cs*, Zastąp kod w klasie z następującym kodem. Zmiany zostały wyróżnione.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Kontroler teraz deklaruje zmienną klasy dla obiekt, który implementuje `IStudentRepository` interfejsu zamiast klasy kontekstu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

Domyślnego (bezparametrowego) konstruktora tworzy nowe wystąpienie kontekstu i opcjonalnie konstruktor umożliwia obiekt wywołujący, aby przekazać wystąpienia kontekstu.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Jeśli były używane *iniekcji zależności*, lub Podpisane, nie potrzebujesz konstruktora domyślnego ponieważ oprogramowania Podpisane czy upewnij się, że zawsze można dostarczyć obiektu poprawnego repozytorium.)

W metodach CRUD nazywane są teraz repozytorium zamiast kontekstu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

I `Dispose` metoda usuwa teraz repozytorium zamiast kontekstu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Uruchom witrynę i kliknij przycisk **studentów** kartę.

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

Strona wygląda i działa tak samo jak przed zmiany kodu do repozytorium i innych stron uczniów również działać w identyczny sposób. Istnieje jednak istotną różnicą w ten sposób `Index` kontrolera jest filtrowanie i kolejność. Z oryginalną wersją tej metody zawiera następujący kod:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

Zaktualizowany interfejs `Index` metoda zawiera następujący kod:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Wyróżniony kod został zmieniony.

W wersji oryginalnej kodu `students` jest typu `IQueryable` obiektu. Zapytanie nie zostanie wysłany do bazy danych, aż zostanie przekonwertowany do kolekcji przy użyciu metody, takie jak `ToList`, który nie występuje aż widok indeksu uzyskuje dostęp do modelu studenta. `Where` Staje się w powyższym kodzie oryginalnej metody `WHERE` klauzuli w zapytanie SQL, które są wysyłane do bazy danych. Z kolei oznacza to, że tylko wybranych obiektów są zwracane przez bazę danych. Jednak w wyniku zmiany `context.Students` do `studentRepository.GetStudents()`, `students` zmiennej po tej instrukcji `IEnumerable` kolekcję zawierającą wszystkie studentów w bazie danych. W rezultacie stosowania `Where` metody jest taki sam, ale teraz praca jest wykonywana w pamięci na serwerze sieci web, a nie przez bazę danych. Dla zapytań, które zwracają dużych ilości danych może to być mało wydajne.

> [!TIP]
> 
> **IQueryable vs. IEnumerable**
> 
> Po zaimplementowaniu repozytorium w sposób pokazany poniżej, nawet jeśli coś po wprowadzeniu **wyszukiwania** pole zapytań wysyłanych do serwera SQL zwraca wszystkie wiersze dla użytkowników domowych, ponieważ nie ma wśród nich kryteria wyszukiwania:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Ta kwerenda zwraca wszystkie dane dla użytkowników domowych, ponieważ repozytorium wykonane zapytanie bez wiedzy kryteria wyszukiwania. Proces sortowania, zastosowanie kryteriów wyszukiwania i wybierając podzbiór danych dla stronicowania (w tym przypadku przedstawiający tylko 3 wiersze) odbywa się w pamięci w przypadku nowszych `ToPagedList` wywoływana jest metoda `IEnumerable` kolekcji.
> 
> W poprzedniej wersji kodu (przed zastosowaniem repozytorium), zapytania nie są wysyłane do bazy danych dopiero po zastosowaniu kryteriów wyszukiwania podczas `ToPagedList` jest wywoływana na `IQueryable` obiektu.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Gdy wywołano ToPagedList `IQueryable` obiekt zapytanie wysyłane do programu SQL Server określa ciąg wyszukiwania i w związku z tym są zwracane tylko wiersze spełniające kryteria wyszukiwania, a nie filtrowania niezbędne w pamięci.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (Następujące samouczek wyjaśnia, jak zbadać zapytań wysyłanych do serwera SQL).


Poniższa sekcja przedstawia sposób implementować metody repozytorium, które pozwalają określić, że praca ta ma się odbywać przez bazę danych.

Teraz po utworzeniu warstwy abstrakcji między kontrolerem i kontekst bazy danych programu Entity Framework. Jeśli zostały będą przeprowadzać automatyczne testy jednostkowe za pomocą tej aplikacji, można utworzyć klasy alternatywnych repozytorium w jednostkowy projekt testowy, który implementuje `IStudentRepository` *.* Zamiast kontaktować się z kontekstem do odczytywania i zapisywania danych, ta klasa zasymulować repozytorium można manipulować kolekcje w pamięci w celu przetestowania funkcje kontrolera.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Implementowanie ogólnego repozytorium i jednostki pracy — klasa

Tworzenie klasy repozytorium dla poszczególnych typów jednostek może skutkować dużą nadmiarowy kod i może skutkować częściowej aktualizacji. Załóżmy na przykład, że trzeba zaktualizować dwa typy inną jednostkę w ramach tej samej transakcji. Jeśli każda używa wystąpienia kontekstu oddzielnej bazy danych, co może się powieść i innych może zakończyć się niepowodzeniem. Jednym ze sposobów zminimalizować nadmiarowy kod jest użycie ogólnego repozytorium i jednym ze sposobów upewnij się, że wszystkie repozytoria używać tego samego kontekstu bazy danych (i w związku z tym koordynować aktualizacje wszystkich) jest użycie jednostki pracy klasy.

W tej części samouczka utworzysz `GenericRepository` klasy i `UnitOfWork` klasy, a następnie używać ich w `Course` kontrolera dostęp zarówno do `Department` i `Course` zestawów jednostek. Zgodnie z opisem, aby zachować tę część samouczka prosty, możesz nie są Tworzenie interfejsów dla tych klas. Ale jeśli zostały będą z nich korzystać w celu ułatwienia TDD, czy zwykle ich wdrożeniem z interfejsami tak samo jak `Student` repozytorium.

### <a name="create-a-generic-repository"></a>Tworzenie ogólnych repozytorium

W *DAL* folderu, Utwórz *GenericRepository.cs* i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Zmienne klasy są deklarowane jako kontekst bazy danych i zestaw jednostek, który jest skonkretyzowany repozytorium:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

Konstruktor akceptuje wystąpienia kontekstu bazy danych i inicjuje jednostki zmiennej zestawu:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

`Get` Metoda używa wyrażenia lambda do wywołującego kodu do określania warunku filtru i kolejność wyniki według kolumny, a parametr typu string umożliwia wywołującego Podaj rozdzielana przecinkami lista właściwości nawigacji dla ładowania wczesny:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

Kod `Expression<Func<TEntity, bool>> filter` oznacza element wywołujący zapewnia wyrażenia lambda na podstawie `TEntity` typu i to wyrażenie będzie zwracać wartość logiczną. Na przykład, jeśli repozytorium jest tworzone dla `Student` typu jednostki, kod w metodzie wywołujący może określać `student => student.LastName == "Smith` &quot; dla `filter` parametru.

Kod `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` również oznacza element wywołujący zapewnia wyrażenia lambda. Ale w takim przypadku danych wejściowych w wyrażeniu `IQueryable` obiekt do `TEntity` typu. Wyrażenie będzie zwracać uporządkowanej wersji tego `IQueryable` obiektu. Na przykład, jeśli repozytorium jest tworzone dla `Student` typu jednostki, kod w metodzie wywołujący może określać `q => q.OrderBy(s => s.LastName)` dla `orderBy` parametru.

Kod w `Get` metoda tworzy `IQueryable` obiekt, a następnie stosuje wyrażenie filtru, jeśli istnieje:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

Następnie stosuje wyrażenia eager ładowania po analizie rozdzielana przecinkami lista:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Na koniec stosuje `orderBy` wyrażenia, jeśli istnieje i zwraca wyniki; w przeciwnym razie zwraca wyniki z nieuporządkowaną kwerendy:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Podczas wywoływania `Get` metody może wykonać filtrowanie i sortowanie `IEnumerable` kolekcji zwracany przez metodę zamiast podając parametrami dla tych funkcji. Ale sortowania i filtrowania pracy następnie będą wykonywane w pamięci na serwerze sieci web. Korzystając z tych parametrów, sprawdź, czy praca jest wykonywana w bazie danych, a nie na serwerze sieci web. Alternatywą jest tworzenie klas pochodnych typów jednostek określonych i dodaj specjalne `Get` metod, takich jak `GetStudentsInNameOrder` lub `GetStudentsByName`. Jednak w aplikacji złożonych, może to spowodować dużej liczby takich klas pochodnych i specjalne metody, które może być więcej pracy do obsługi.

Kod w `GetByID`, `Insert`, i `Update` metody jest podobny do był wyświetlany w repozytorium nierodzajową. (Nie udostępnia parametru wczesny ładowania w `GetByID` podpisu, ponieważ nie można wykonać ładowania wczesny z `Find` metody.)

Dwa przeciążenia są dostępne dla `Delete` metody:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Jeden z tych pozwala przekazać tylko identyfikator jednostki do usunięcia, a wyższy wystąpienia jednostki. Jak przedstawiono w [Obsługa współbieżności](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczka możesz Obsługa współbieżności wymaga `Delete` metody pobierającej wystąpienia jednostki, która obejmuje oryginalnej wartości właściwości śledzenia.

To repozytorium ogólnego obsłuży typowe wymagania CRUD. Gdy typem danego podmiotu ma specjalne wymagania, na przykład bardziej złożonych filtrowania lub kolejności, można utworzyć klasy pochodnej, która ma dodatkowe metody dla tego typu.

## <a name="creating-the-unit-of-work-class"></a>Tworzenie jednostki pracy — klasa

Jednostka pracy klasy służy jednemu celowi: aby upewnij się, że gdy używasz wielu repozytoriów, mają kontekstu pojedynczej bazy danych. W ten sposób po zakończeniu jednostka pracy można wywołać `SaveChanges` metody w tym wystąpieniu kontekstu i mieć pewność, wszystkie powiązane zmiany zostaną skoordynowany sposób. Wszystkie opcje, które jest potrzeb klasy `Save` metody i właściwości dla każdego repozytorium. Każda właściwość repozytorium Zwraca wystąpienie repozytorium, które zostały utworzone przy użyciu tego samego wystąpienia kontekstu bazy danych, co inne wystąpienia repozytorium.

W *DAL* folderu, Utwórz plik klasy o nazwie *UnitOfWork.cs* i Zastąp kod szablonu z następującym kodem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

Kod tworzy zmienne klasy dla kontekstu bazy danych i każdego repozytorium. Aby uzyskać `context` zmiennej, zostanie uruchomiony nowy kontekst:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Każda właściwość repozytorium sprawdza, czy repozytorium już istnieje. Jeśli nie, metoda tworzy repozytorium, przekazując wystąpienia kontekstu. W związku z tym wszystkie repozytoria współużytkują to samo wystąpienie kontekstu.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

`Save` Wywołania metody `SaveChanges` w kontekście bazy danych.

Wszystkie klasy, która tworzy kontekst bazy danych w zmiennej klasy, takich jak `UnitOfWork` klasa implementuje `IDisposable` i usuwa kontekstu.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Zmienianie kontrolera kursu do korzystania z klasy UnitOfWork i repozytoriów

Zastąp kod aktualnie zainstalowana w *CourseController.cs* następującym kodem:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Ten kod dodaje zmienną klasy `UnitOfWork` klasy. (Jeśli były używane interfejsów w tym miejscu, nie należy zainicjować zmiennej tutaj; zamiast tego, czy implementuje wzorzec dwa konstruktory tak samo, jak została `Student` repozytorium.)

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

W pozostałej części klasy, wszystkie odwołania do kontekstu bazy danych są zastępowane przez odwołania do odpowiednich repozytorium przy użyciu `UnitOfWork` właściwości, które mają dostęp do repozytorium. `Dispose` Metoda usuwa `UnitOfWork` wystąpienia.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Uruchom witrynę i kliknij przycisk **kursów** kartę.

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

Strona wygląda i działa tak samo, jak poprzednio wprowadzone zmiany i innych stron kursu również działać w identyczny sposób.

## <a name="summary"></a>Podsumowanie

Teraz zaimplementowano repozytorium i jednostki pracy. Użyto wyrażenia lambda jako parametry metody w ogólnym repozytorium. Aby uzyskać więcej informacji o sposobie używania tych wyrażeń z `IQueryable` obiektów, zobacz [IQueryable(T) interfejsu (System.Linq)](https://msdn.microsoft.com/library/bb351562.aspx) w bibliotece MSDN. W następnej samouczka, dowiesz się, jak obsługiwać niektórych zaawansowanych scenariuszy.

Linki do innych zasobów programu Entity Framework, można znaleźć w [Mapa zawartości dostępu do danych programu ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
