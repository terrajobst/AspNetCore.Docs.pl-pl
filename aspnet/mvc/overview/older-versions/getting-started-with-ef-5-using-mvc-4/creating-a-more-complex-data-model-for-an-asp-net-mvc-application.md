---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: Tworzenie bardziej złożonych modelu danych dla aplikacji platformy ASP.NET MVC (4 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f81f3d80-3674-4d8e-a9b1-87feed1a93c9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: c8f01b33c18ce77d91ee2f0db5e561b047c1891c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application-4-of-10"></a>Tworzenie bardziej złożonych modelu danych dla aplikacji platformy ASP.NET MVC (4 10)
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Samouczek serii można uruchomić od początku lub [pobrać projekt starter w tym rozdziale](building-the-ef5-mvc4-chapter-downloads.md) i zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli napotkasz problem, nie można rozwiązać, [Pobieranie ukończone rozdział](building-the-ef5-mvc4-chapter-downloads.md) i próba odtworzenia problemu. Rozwiązanie tego problemu można znaleźć ogólnie porównując swój kod kompletny kod. Dla niektórych typowych błędów i sposobu rozwiązania tych problemów, zobacz [błędów i rozwiązania problemu.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


W poprzednich samouczki pracy z modelem danych proste składającą się z trzech jednostek. W tym samouczku można dodać więcej jednostki i relacje i model danych będzie dostosować, określając formatowania, sprawdzanie poprawności i reguły mapowania bazy danych. Zobaczysz Dostosowywanie modelu danych na dwa sposoby: poprzez dodanie atrybutów do klas jednostek oraz dodawanie kodu do klasy kontekstu bazy danych.

Po zakończeniu klas jednostek spowoduje model pełne dane, które przedstawiono na poniższej ilustracji:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Dostosowywanie modelu danych za pomocą atrybutów

W tej sekcji pojawi się, jak dostosować za pomocą atrybutów określających formatowania, sprawdzanie poprawności i reguły mapowania bazy danych modelu danych. Następnie w kilka z następujących sekcji utworzysz pełną `School` modelu danych, dodając atrybuty dla klasy już utworzony i tworzenia nowych klas dla pozostałych typów jednostek w modelu.

### <a name="the-datatype-attribute"></a>Atrybut typu danych

Dat rejestracji dla użytkowników domowych wszystkie strony sieci web obecnie Wyświetl czas wraz z datą, mimo że wszystkie potrzebne informacje dla tego pola to data. Za pomocą atrybutów adnotacji danych, można go utworzyć kod zmian, który naprawi format wyświetlania w każdym widoku, który zawiera dane. Aby wyświetlić przykład sposobu wykonywania, czy zostanie dodana do atrybutu `EnrollmentDate` właściwości w `Student` klasy.

W *Models\Student.cs*, Dodaj `using` instrukcji dla `System.ComponentModel.DataAnnotations` przestrzeni nazw i Dodaj `DataType` i `DisplayFormat` atrybuty do `EnrollmentDate` właściwości, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,13-14)]

[DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybut służy do określania typu danych, który jest bardziej szczegółowy niż typ wewnętrznej bazy danych. W takim przypadku tylko chcemy śledzić data nie Data i godzina. [Wyliczenie DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) zawiera wiele typów danych, takich jak *dat, czasu, numer telefonu, waluty, EmailAddress* i inne. `DataType` Atrybut można również włączyć aplikacji w celu umożliwienia automatycznie funkcji specyficznych dla typu. Na przykład `mailto:` można tworzyć łącza [DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx), i może zostać dostarczony selektora daty [DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) w przeglądarkach obsługujących [HTML5](http://html5.org/). [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybuty emituje HTML 5 [danych -](http://ejohn.org/blog/html-5-data-attributes/) (Wymowa *kreska danych*) atrybutów, które byłyby zrozumiałe dla przeglądarki HTML 5. [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybutów nie dostarcza żadnych sprawdzania poprawności.

`DataType.Date` Określa format daty, która jest wyświetlana. Domyślnie pole danych są wyświetlane domyślne formaty oparte na tym serwerze [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

`DisplayFormat` Atrybut służy do jawnie określić format daty:


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


`ApplyFormatInEditMode` Ustawienie określa, czy określony sposób formatowania powinny również będą stosowane, gdy wartość jest wyświetlana w polu tekstowym do edycji. (Nie może być który dla niektórych pól — na przykład dla wartości waluty, może nie ma symbolu waluty w polu tekstowym do edycji.)

Można użyć [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atrybutu przez sam, ale jest zwykle warto użyć [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) również atrybutu. `DataType` Przekazuje atrybutu *semantyki* danych w przeciwieństwie do jak renderować ją na ekranie i zapewnia następujące korzyści, które nie można uzyskać z `DisplayFormat`:

- Przeglądarki, można włączyć funkcje HTML5 (na przykład pokazać formant kalendarza, symbol waluty odpowiednie ustawienia regionalne, przesyłanie pocztą e-mail łączy, itp.).
- Domyślnie, przeglądarka wyświetli danych przy użyciu właściwego formatu na podstawie Twojej [ustawień regionalnych](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atrybut można włączyć MVC wybrać szablon pola prawo do renderowania danych ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) Jeśli używany przez samego używa szablonu ciągu). Aby uzyskać więcej informacji, zobacz Brad Wilson [ASP.NET MVC 2 szablony](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Chociaż przeznaczone dla platformy MVC 2, w tym artykule nadal dotyczy bieżącej wersji programu ASP.NET MVC.)

Jeśli używasz `DataType` atrybutu z polem daty należy określić `DisplayFormat` atrybutu również w celu zapewnienia, że pole poprawnie renderuje przeglądarki Chrome. Aby uzyskać więcej informacji, zobacz [tego wątku StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Ponownie uruchom strony indeksu dla użytkowników domowych i zwróć uwagę, nie są wyświetlane godziny dla daty rejestracji. Będzie taka sama wartość true dla dowolnego widoku, który używa `Student` modelu.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>Element StringLengthAttribute

Można również określić reguły sprawdzania poprawności danych i komunikatów za pomocą atrybutów. Załóżmy, że chcesz upewnić się, że użytkownicy nie wprowadzić więcej niż 50 znaków dla nazwy. Aby dodać to ograniczenie, Dodaj [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atrybuty do `LastName` i `FirstMidName` właściwości, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atrybutu nie uniemożliwić wprowadzanie biały znak dla nazwy użytkownika. Można użyć [wyrażenia regularnego](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atrybutu, aby zastosować ograniczenia do danych wejściowych. Na przykład następujący kod wymaga pierwszego znaku się wielkie litery i pozostałych znaków jako alfabetycznej:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

[MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) atrybutu zapewnia funkcje podobne do [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atrybutu, ale nie zapewnia po stronie klienta sprawdzania poprawności.

Uruchom aplikację, a następnie kliknij przycisk **studentów** kartę. Otrzymasz następujący błąd:

*Model kopii kontekstu "SchoolContext" została zmieniona od czasu utworzenia bazy danych. Należy rozważyć użycie migracje Code First aktualizacji bazy danych ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

Model bazy danych został zmieniony w taki sposób, który wymaga zmiany w schemacie bazy danych, a Entity Framework wykrył, że. Użyjesz migracji do aktualizacji schematu bez utraty danych, które są dodawane do bazy danych przy użyciu interfejsu użytkownika. Zmiana danych, która została utworzona przez `Seed` — metoda, która ma zostać zmieniona wstecz do oryginalnego stanu z powodu [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) metody, której używasz w `Seed` metody. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) jest odpowiednikiem operacji "upsert" z bazy danych terminologii.)

W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenia:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

`add-migration MaxLengthOnNames` Polecenie tworzy plik o nazwie  *&lt;sygnatury czasowej&gt;\_MaxLengthOnNames.cs*. Ten plik zawiera kod, który spowoduje zaktualizowanie bazy danych zgodnie z bieżącym modelem danych. Sygnatura czasowa dołączany na początku nazwy pliku migracji jest używany przez programu Entity Framework porządkowania migracji. Po utworzeniu wiele migracji, jeśli porzucenia bazy danych lub w przypadku wdrażania projektu za pomocą migracji, wszystkie migracja są stosowane w kolejności, w której zostały utworzone.

Uruchom **Utwórz** strony, a następnie wprowadź nazwę, albo więcej niż 50 znaków. Jak przekracza 50 znaków, weryfikacji po stronie klienta natychmiast zawiera komunikat o błędzie.

![Błąd val po stronie klienta](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>Atrybut kolumny

Atrybuty umożliwia także kontrolować sposób z klas i właściwości są mapowane do bazy danych. Załóżmy, że używasz nazwy `FirstMidName` nazwę pierwszego pola, ponieważ pole może także zawierać drugie imię. Ale ma być nazwane kolumna bazy danych `FirstName`, ponieważ użytkownicy, którzy będą Pisanie zapytań ad hoc w bazie danych są zapoznanie tej nazwy. Aby to mapowanie, można użyć `Column` atrybutu.

`Column` Atrybut określa, że po utworzeniu bazy danych, w kolumnie `Student` tabeli, który jest mapowany na `FirstMidName` właściwość o nazwie `FirstName`. Innymi słowy, gdy kod odwołuje się do `Student.FirstMidName`, dane będą pobierane z lub zaktualizowane w `FirstName` kolumny `Student` tabeli. Jeśli nie określisz nazwy kolumn mających taką samą nazwę jak nazwa właściwości.

Dodaj using instrukcji dla [System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) i atrybut nazwy kolumny do `FirstMidName` właściwości, jak pokazano w poniższym kodzie wyróżnione:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

Dodanie [atrybut kolumny](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) zmiany modelu tworzenia kopii SchoolContext, więc nie będzie zgodny z bazą danych. Wpisz następujące polecenia w PMC, aby utworzyć inną migracji:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

W **Eksploratora serwera** (**Eksploratora bazy danych** Jeśli używasz Express for Web), kliknij dwukrotnie *uczniowie* tabeli.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

Na poniższej ilustracji przedstawiono oryginalna nazwa kolumny, jak przed zastosowaniem dwóch pierwszych migracji. Oprócz nazwy kolumny zmiana z `FirstMidName` do `FirstName`, dwóch kolumn będących nazwy zostały zmienione od `MAX` długości do 50 znaków.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Można również zmienić bazy danych mapowania za pomocą [interfejsu API Fluent](https://msdn.microsoft.com/data/jj591617), jak można zauważyć w dalszej części tego samouczka.

> [!NOTE]
> Jeśli spróbujesz skompilować przed zakończeniem Tworzenie wszystkich tych klas jednostek, może wystąpić błędy kompilatora.


## <a name="create-the-instructor-entity"></a>Utwórz jednostkę instruktora

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Utwórz *Models\Instructor.cs*, zastępując kod szablonu z następującym kodem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Zwróć uwagę, że kilka właściwości są takie same, w `Student` i `Instructor` jednostek. W [wdrażanie dziedziczenia](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczek później w tej serii, użytkownik będzie Refaktoryzuj używanie dziedziczenia, aby wyeliminować dublowanie.

### <a name="the-required-and-display-attributes"></a>Wymagane i wyświetlanie atrybutów

Atrybuty w `LastName` właściwości określić, że jest polem wymaganym podpis pola tekstowego powinien być "Nazwisko" (zamiast nazwy właściwości, które byłyby "Nazwisko" bez spacji) i czy wartość nie może być dłuższa niż 50 znaków.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

[Atrybutu StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) Ustawia maksymalną długość w bazie danych i zapewnia po stronie klienta i po stronie serwera weryfikacji dla platformy ASP.NET MVC. Minimalna długość ciągu można również określić, w tym atrybucie, ale wartość minimalna nie ma wpływu na schemat bazy danych. [Wymaganego atrybutu](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) jest zbędny dla typów wartości, takie jak DateTime, int, double i float. Typy wartości nie można przypisać wartość null, dzięki czemu są one z założenia wymagane. Można usunąć [wymaganego atrybutu](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) i zamień ją na minimalną długość parametru `StringLength` atrybutu:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs?highlight=2)]

Wiele atrybutów można umieścić w jednym wierszu, więc można także zapisać klasy instruktora w następujący sposób:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-fullname-calculated-property"></a>Imię i nazwisko obliczona właściwość

`FullName` jest obliczonej właściwości, która zwraca wartość, która jest tworzona przez łączenie dwóch innych właściwości. W związku z tym ma tylko `get` metody dostępu i nie `FullName` kolumny zostanie wygenerowany w bazie danych.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Właściwości nawigacji OfficeAssignment i szkolenia

`Courses` i `OfficeAssignment` właściwości są właściwości nawigacji. Jak wyjaśniono wcześniej, są zazwyczaj definiowane jako [wirtualnego](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) tak, aby można było korzystać funkcję programu Entity Framework [opóźnionego ładowania](https://msdn.microsoft.com/magazine/hh205756.aspx). Ponadto, jeśli właściwość nawigacji może zawierać wiele jednostek, jego typ musi implementować [ICollection&lt;T&gt; ](https://msdn.microsoft.com/library/92t2ye13.aspx) interfejsu. (Na przykład [IList&lt;T&gt; ](https://msdn.microsoft.com/library/5y536ey6.aspx) nie kwalifikuje się [IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/library/9eekhta0.aspx) ponieważ `IEnumerable<T>` nie implementuje [Dodaj ](https://msdn.microsoft.com/library/63ywd54z.aspx).

Instruktora nauczyć dowolną liczbę kursów, więc `Courses` jest zdefiniowany jako kolekcja `Course` jednostek. Stan naszej reguły biznesowe instruktora tylko może mieć co najwyżej jeden pakiet office, tak `OfficeAssignment` jest zdefiniowany jako pojedynczy `OfficeAssignment` jednostki (które mogą być `null` , jeśli nie przypisano żadnych pakietu office).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>Utwórz jednostkę OfficeAssignment

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Utwórz *Models\OfficeAssignment.cs* następującym kodem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Skompiluj projekt, który zapisuje zmiany i sprawdza, czy nie utworzono żadnej kopii i Wklej błędów, które kompilator może catch.

### <a name="the-key-attribute"></a>Atrybut klucza

Brak relacji jeden do zero lub jeden między `Instructor` i `OfficeAssignment` jednostek. Przypisania office występuje tylko w odniesieniu do instruktora jest przypisany do, a więc jego klucz podstawowy również jest jego klucza obcego do `Instructor` jednostki. Ale programu Entity Framework automatycznie nie może rozpoznać `InstructorID` jako serwera podstawowego klucza tej jednostki, ponieważ jego nazwa nie `ID` lub *classname* `ID` konwencji nazewnictwa. W związku z tym `Key` atrybut służy do identyfikacji jako klucz:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

Można również użyć `Key` atrybut, jeśli jednostka ma własny klucz podstawowy, ale ma inny niż nazwa właściwości `classnameID` lub `ID`. Domyślnie EF traktuje klucz jako z systemem innym niż baza danych wygenerowała, ponieważ kolumna jest identyfikujące relacji.

### <a name="the-foreignkey-attribute"></a>Atrybut klucza obcego

Po relacji jeden do zero lub jeden lub relacją między dwoma obiektami (takich jak między `OfficeAssignment` i `Instructor`), nie może działać EF, które punkt końcowy relacji jest podmiot zabezpieczeń i zakończenia, która jest zależna. Relacje jeden do jednego ma właściwości nawigacji odwołania w każdej klasie do innej klasy. [Atrybutu klucza obcego](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) może odnosić się do klasy zależne ustanowienie relacji. W przypadku pominięcia [atrybutu klucza obcego](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), otrzymasz następujący błąd podczas próby utworzenia migracji:

Nie można ustalić głównego końca skojarzenia między typami "ContosoUniversity.Models.OfficeAssignment" i "ContosoUniversity.Models.Instructor". Główny koniec skojarzenia należy jawnie skonfigurować przy użyciu interfejsu API fluent relacji lub adnotacji danych.

Później w samouczku poniżej opisano sposób konfigurowania tej relacji z interfejsu API fluent.

### <a name="the-instructor-navigation-property"></a>Właściwość nawigacji instruktora

`Instructor` Jednostka ma wartość null `OfficeAssignment` właściwości nawigacji (ponieważ instruktora nie może być przypisanie pakietu office) i `OfficeAssignment` jednostka ma niedopuszczającą `Instructor` właściwości nawigacji (ponieważ przypisania office nie istnieje bez instruktora — `InstructorID` nie dopuszcza wartości null). Gdy `Instructor` jednostka ma powiązanego `OfficeAssignment` jednostki, każdy obiekt ma odwołanie do jeden z nich w jej właściwości nawigacji.

Można umieścić `[Required]` atrybutu właściwości nawigacji instruktora w celu określenia muszą być powiązane instruktora, że nie trzeba to zrobić, ponieważ klucz obcy InstructorID (który jest także klucz do tej tabeli) jest wartości null.

## <a name="modify-the-course-entity"></a>Modyfikowanie jednostek kursu

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

W *Models\Course.cs*, Zastąp kod dodane wcześniej następujący kod:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

Jednostka kursu ma właściwości klucza obcego `DepartmentID` wskazujących pokrewny `Department` jednostki i będzie miał `Department` właściwości nawigacji. Nie wymaga programu Entity Framework, można dodać właściwości klucza obcego do modelu danych w przypadku właściwości nawigacji dla obiekt pokrewny. Wszędzie tam, gdzie są one potrzebne EF automatycznie tworzy klucze obce w bazie danych. Jednak po klucz obcy w modelu danych może uaktualniać prostszy i efektywniejszy. Na przykład podczas pobierania jednostki kursu, aby edytować, `Department` jednostka ma wartość null, jeśli nie zostanie załadowany, więc podczas aktualizacji obiektu kursu, należy najpierw pobrać `Department` jednostki. Gdy właściwość klucza obcego `DepartmentID` znajduje się w modelu danych, nie trzeba było pobrać `Department` jednostki przed uaktualnieniem.

### <a name="the-databasegenerated-attribute"></a>Atrybut DatabaseGenerated

[Atrybutu DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) z [Brak](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) parametru `CourseID` właściwość określa, że wartości klucza podstawowego są dostarczane przez użytkownika zamiast wygenerowanych przez bazę danych.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

Domyślnie program Entity Framework przyjęto założenie, że wartości klucza podstawowego są generowane przez bazę danych. To już ma w większości przypadków. Niemniej jednak w przypadku `Course` jednostki, będzie używać numer kursu określone przez użytkownika, takie jak seria 1000 działu jednej serii 2000 do innego działu i tak dalej.

### <a name="foreign-key-and-navigation-properties"></a>Klucz obcy i właściwości nawigacji

Właściwości klucza obcego i właściwości nawigacji w `Course` jednostkę odzwierciedlić się następująco:

- Kursu jest przypisany do jednego działu, więc ma `DepartmentID` klucza obcego i `Department` właściwość nawigacji z powodów wymienionych powyżej. 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Kursu może mieć dowolną liczbę studentów zarejestrowane, więc `Enrollments` właściwość nawigacji jest kolekcją: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Kursu może organizowane jednocześnie przez wiele instruktorów, więc `Instructors` właściwość nawigacji jest kolekcją: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="creating-the-department-entity"></a>Tworzenie jednostki działu

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Utwórz *Models\Department.cs* następującym kodem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Atrybut kolumny

Wcześniej używane [atrybut kolumny](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) zmienić mapowanie nazwy kolumny. W kodzie `Department` jednostki, `Column` atrybutów jest używane do zmienić mapowanie typu danych SQL, tak aby kolumna zostanie zdefiniowana przy użyciu programu SQL Server [pieniędzy](https://msdn.microsoft.com/library/ms179882.aspx) typu w bazie danych:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Mapowanie kolumny zwykle nie jest wymagane, ponieważ programu Entity Framework zazwyczaj wybiera odpowiedni typ danych programu SQL Server, na podstawie typu CLR dla właściwości. Środowisko CLR `decimal` typu map do programu SQL Server `decimal` typu. Ale w takim przypadku wiesz, że kolumna zostanie zawierający kwot i [pieniędzy](https://msdn.microsoft.com/library/ms179882.aspx) jest bardziej odpowiednie dla danego typu danych.

### <a name="foreign-key-and-navigation-properties"></a>Klucz obcy i właściwości nawigacji

Właściwości klucza i nawigacja obcego odzwierciedla się następująco:

- Dział może lub nie ma uprawnienia administratora, a administrator jest zawsze instruktora. W związku z tym `InstructorID` właściwość jest uwzględniona jako klucz obcy do `Instructor` jednostki i znak zapytania jest dodawany po `int` określenie można oznaczyć jako wartości null właściwość typu. Właściwość nawigacji jest o nazwie `Administrator` , ale zawiera `Instructor` jednostki: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Dział może mieć wiele kursów, więc ma `Courses` właściwość nawigacji: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Według Konwencji programu Entity Framework umożliwia usuwanie kaskadowe dla klucza obcego nie dopuszcza wartości null i relacje wiele do wielu. Może to spowodować cykliczne cascade delete reguł, które spowoduje, że wystąpił wyjątek podczas wykonywania kodu inicjatora. Na przykład, jeśli nie zostały zdefiniowane `Department.InstructorID` właściwość jako wartości null, jak następujący komunikat o wyjątku podczas uruchamiania inicjatora: "więzy relacji spowoduje odwołanie cykliczne, które nie są dozwolone." W razie potrzeby reguł biznesowych `InstructorID` właściwość jako wartości null, czy należy użyć następujących interfejsu API fluent wyłączyć usuwanie kaskadowe w relacji: 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modifying-the-student-entity"></a>Modyfikowanie jednostek dla użytkowników domowych

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

W *Models\Student.cs*, Zastąp kod dodane wcześniej następujący kod. Zmiany zostały wyróżnione.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=12,15,24-27)]

## <a name="the-enrollment-entity"></a>Jednostka rejestracji

 W *Models\Enrollment.cs*, Zastąp kod dodane wcześniej następujący kod

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs?highlight=16)]

### <a name="foreign-key-and-navigation-properties"></a>Klucz obcy i właściwości nawigacji

Właściwości klucza obcego i właściwości nawigacji odzwierciedla się następująco:

- Rekord rejestracji dotyczy jednego ciągu, więc ma `CourseID` właściwości kluczy obcych i `Course` właściwości nawigacji: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]
- Rekord rejestracji jest dla pojedynczego studentów, więc ma `StudentID` właściwości kluczy obcych i `Student` właściwości nawigacji: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

### <a name="many-to-many-relationships"></a>Relacje wiele do wielu

Relację wiele do wielu między `Student` i `Course` jednostek i `Enrollment` jednostki działa jako sprzężenia wiele do wielu tabela *z ładunku* w bazie danych. Oznacza to, że `Enrollment` tabela zawiera dodatkowe dane poza kluczy obcych dla połączonych tabel (w tym przypadku klucza podstawowego i `Grade` właściwości).

Na poniższej ilustracji przedstawiono, jak wyglądają te relacje w diagramie jednostki. (Ten diagram został wygenerowany za pomocą [Entity Framework zaawansowanych narzędzi](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); Tworzenie diagramu nie stanowi części samouczka, po prostu jest on używany jako ilustrację.)

![Student-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

Każdy wiersz relacji ma 1 w jeden element end i znak gwiazdki (\*) na drugim, wskazując relacji jeden do wielu.

Jeśli `Enrollment` tabeli nie włączono informacji o kategorii, czy tylko musi zawierać dwa klucze obce `CourseID` i `StudentID`. W takim przypadku odpowiada on tabeli sprzężenia wiele do wielu *bez ładunku* (lub *czysty sprzężenia tabeli*) w bazie danych i nie należy utworzyć klasę modelu dla niej w ogóle. `Instructor` i `Course` mają takie relacji wiele do wielu, i jak widać, nie żadna z klas jednostek między nimi:

![Instructor-Course_many-to-many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Tabela sprzężenia jest wymagana w bazie danych, jednak, jak pokazano na poniższym diagramie bazy danych:

![Instructor-Course_many-to-many_relationship_tables](https://asp.net/media/2577802/Windows-Live-Writer_Creating-a.NET-MVC-Application-4-of-10h1_B662_Instructor-Course_many-to-many_relationship_tables_03e042cf-db89-4b4c-985a-e458351ada76.png)

Entity Framework automatycznie tworzy `CourseInstructor` tabeli i odczytu i zaktualizować go pośrednio za odczytywanie i aktualizowanie `Instructor.Courses` i `Course.Instructors` właściwości nawigacji.

## <a name="entity-diagram-showing-relationships"></a>Jednostki Diagram przedstawiający relacje

Na poniższej ilustracji przedstawiono diagram do narzędzia Entity Framework zasilania utworzyć dla ukończonych modelu służbowe.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Oprócz linii relacji wiele do wielu (\* do \*) i linii relacji jeden do wielu (od 1 do \*), można widoczną w tym miejscu linię relacji jeden do zero lub jeden (1 do od 0 do 1) między `Instructor` i `OfficeAssignment` jednostki i linię relacji zero lub jeden do wielu (od 0 do 1, aby \*) między jednostkami instruktora i działu.

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Dostosowywanie modelu danych przez dodanie kodu do kontekstu bazy danych

Następnie należy dodać nowe jednostki do `SchoolContext` klasy i dostosowywać niektóre mapowania za pomocą [interfejsu API fluent](https://msdn.microsoft.com/data/jj591617) wywołania. (Interfejs API jest "fluent", ponieważ jest często używany przez instalowanie szereg wywołania metody w jednej instrukcji).

W tym samouczku użyjesz interfejsu API fluent tylko w przypadku mapowanie bazy danych, które nie są z atrybutami. Jednak umożliwia także interfejsu API fluent określić większość formatowania, sprawdzanie poprawności i reguły mapowania, które możesz wykonać za pomocą atrybutów. Niektóre atrybuty, takie jak `MinimumLength` nie można zastosować z interfejsu API fluent. Jak wspomniano wcześniej, `MinimumLength` nie powoduje zmiany schematu, ma zastosowanie tylko reguły sprawdzania poprawności po stronie klienta i serwera

Niektórzy deweloperzy wolą Użyj interfejsu API fluent wyłącznie tak, aby ich zachowanie ich klasami jednostki "Wyczyść". Jeśli chcesz, i istnieje kilka dostosowania, które jest możliwe tylko przy użyciu interfejsu API fluent można mieszać atrybutów i wygodnego interfejsu API, ale na ogół zalecaną praktyką jest wybierz jedną z tych dwóch metod i użyj to spójnie możliwie.

Aby dodać nowe jednostki w danym modelu i wykonać mapowanie bazy danych, które nie wykonać za pomocą atrybutów, Zastąp kod w *DAL\SchoolContext.cs* następującym kodem:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

Nowy raport w [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) sprzężenia wiele do wielu tabela konfiguruje metody:

- Dla relacji wiele do wielu między `Instructor` i `Course` jednostek, kod określa nazwy tabel i kolumn dla tabeli sprzężenia. Kod najpierw skonfigurować relacji wiele do wielu można bez tego kodu, ale nie można wywołać, otrzymasz domyślnych nazw takich jak `InstructorInstructorID` dla `InstructorID` kolumny.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

Następujący kod stanowi przykład użytkownik może mieć użycia interfejsu API fluent zamiast atrybutów Określ relację między `Instructor` i `OfficeAssignment` jednostek:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Informacje o instrukcji "interfejsu API fluent" robią w tle, zobacz [interfejsu API Fluent](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) wpis w blogu.

## <a name="seed-the-database-with-test-data"></a>Inicjatora bazy danych z danych testowych

Zastąp kod w *Migrations\Configuration.cs* pliku następującym kodem w celu zapewnienia danych inicjatora nowe jednostki po utworzeniu.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Jak przedstawiono w pierwszym samouczku większość ten kod po prostu aktualizuje lub tworzy nowe obiekty jednostki i ładuje przykładowych danych do właściwości wymaganych do testowania. Zwróć jednak uwagę, jak `Course` jednostki, która ma relację wiele do wielu z `Instructor` jednostki, jest obsługiwane:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Po utworzeniu `Course` obiektu, należy zainicjować `Instructors` właściwość nawigacji jako pustą kolekcję przy użyciu kodu `Instructors = new List<Instructor>()`. Dzięki temu można dodać `Instructor` jednostek, które są powiązane z tym `Course` przy użyciu `Instructors.Add` metody. Utworzył lista jest pusta, nie można dodać te relacje ponieważ `Instructors` właściwości może mieć wartości null i nie mają `Add` metody. Inicjalizacja listy można również dodać do konstruktora.

## <a name="add-a-migration-and-update-the-database"></a>Dodaj migracji i aktualizacji bazy danych

Z PMC, wprowadź `add-migration` polecenia:

`PM> add-Migration Chap4`

Próba aktualizacji bazy danych w tym momencie zostanie wyświetlony następujący błąd:

*Instrukcja ALTER TABLE jest w konflikcie z ograniczenie FOREIGN KEY "klucz OBCY\_dbo. Kursu\_dbo. Dział\_DepartmentID ". Konflikt wystąpił w bazie danych "ContosoUniversity" tabeli "dbo. Dział", kolumna"DepartmentID".*

Edytuj &lt; *sygnatury czasowej&gt;\_Chap4.cs* pliku, a następnie wprowadź następujące zmiany kodu (należy dodać instrukcję SQL i zmodyfikować `AddColumn` instrukcji):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

(Upewnij się, że komentarz, lub usuń istniejącą `AddColumn` wiersz po dodaniu nowego lub zostanie wyświetlony błąd, po wprowadzeniu `update-database` polecenia.)

Czasami podczas wykonywania migracji z istniejącymi danymi, należy wstawić dane do bazy danych do zaspokojenia ograniczeń klucza obcego, a to wykonywanych czynności teraz. Wygenerowany kod dodaje niedopuszczającą `DepartmentID` klucza obcego do `Course` tabeli. Jeśli istnieje już wierszy w `Course` tabeli, gdy kod działa, `AddColumn` operacja nie powiedzie się, ponieważ program SQL Server nie może ustalić, jaka wartość w kolumnie, która nie może mieć wartości null. W związku z tym zmieniono kod, aby podać nową kolumnę wartości domyślnej, a po utworzeniu szkieletu dział o nazwie "Temp" jako domyślnego działu. Dzięki temu, jeśli istnieją `Course` wierszy, gdy ten kod jest uruchamiany, ich zostaną wszystkie powiązane do działu "Temp".

Gdy `Seed` uruchamia — metoda powoduje wstawienie wierszy w `Department` tabeli i będą dotyczyły istniejących `Course` wierszy do tych nowych `Department` wierszy. Jeśli nie dodano żadnych kursów w interfejsie użytkownika, będzie następnie nie jest już potrzebny dział "Temp" lub wartość domyślną w `Course.DepartmentID` kolumny. Aby zezwolić na możliwość, że ktoś może dodano kursy za pomocą aplikacji, będą również chcesz zaktualizować `Seed` kod metody, aby upewnić się, że wszystkie `Course` wierszy (nie tylko te, które zostały wstawione przez starszych uruchomień `Seed` metoda) ma Nieprawidłowa `DepartmentID` wartości przed usunięciem domyślna wartość z kolumny i Usuń dział "Temp".

Po zakończeniu edycji &lt; *sygnatury czasowej&gt;\_Chap4.cs* pliku, wprowadź `update-database` w PMC do wykonania migracji.

> [!NOTE]
> Istnieje możliwość pobrania inne błędy podczas migracji danych i wprowadzania zmian schematu. Jeśli występują błędy migracji nie można rozwiązać, to można zmienić w ciągu połączenia *Web.config* pliku lub Usuń bazę danych. Najprostsza metoda jest można zmienić nazwy bazy danych w *Web.config* pliku. Na przykład zmienić nazwę bazy danych do aktualizacji zbiorczej\_testu, jak pokazano poniżej:
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.xml?highlight=1-2)]
> 
>  Z nową bazę danych, nie ma żadnych danych, aby przeprowadzić migrację oraz `update-database` polecenie jest znacznie bardziej prawdopodobne zakończyć bez błędów. Aby uzyskać instrukcje dotyczące sposobu usuwania bazy danych, zobacz [jak usunąć bazę danych z programu Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).


Otworzyć bazy danych w **Eksploratora serwera** została wcześniej, a rozwiń **tabel** węzeł, aby zobaczyć, czy wszystkie tabele zostały utworzone. (Jeśli nadal masz **Eksploratora serwera** otworzyć z wcześniejszego stanu, kliknij przycisk **Odśwież** przycisku.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

Nie utworzono klasę modelu dla `CourseInstructor` tabeli. Jak opisano wcześniej, jest to tabela sprzężenia dla relacji wiele do wielu między `Instructor` i `Course` jednostek.

Kliknij prawym przyciskiem myszy `CourseInstructor` tabeli i wybierz **Pokaż dane tabeli** Aby sprawdzić, czy zawiera dane w nim w `Instructor` jednostek dodane do `Course.Instructors` właściwości nawigacji.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="summary"></a>Podsumowanie

Masz teraz bardziej złożonych modelu danych i odpowiednią bazę danych. W samouczku następujące dowiesz się więcej na temat sposobów dane dotyczące dostępu.

Linki do innych zasobów programu Entity Framework, można znaleźć w [Mapa zawartości dostępu do danych programu ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
