---
title: Platformy ASP.NET Core MVC podstawowych EF — Model danych — 5 10
author: rick-anderson
description: W tym samouczku Dodaj więcej jednostki i relacje i dostosować modelu danych, określając formatowania, sprawdzanie poprawności i mapowanie reguły.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: d89ca44917fac57febc2f8b0d632ae004ca7216c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277390"
---
# <a name="aspnet-core-mvc-with-ef-core---data-model---5-of-10"></a>Platformy ASP.NET Core MVC podstawowych EF — Model danych — 5 10

Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET Core MVC przy użyciu programu Entity Framework Core i Visual Studio. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](intro.md).

W poprzednich samouczki pracy z modelem danych proste składającą się z trzech jednostek. W tym samouczku dodasz więcej jednostki i relacje i model danych będzie dostosować, określając formatowania, sprawdzanie poprawności i reguły mapowania bazy danych.

Po zakończeniu klas jednostek spowoduje model pełne dane, które przedstawiono na poniższej ilustracji:

![Diagram jednostek](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Dostosowywanie modelu danych za pomocą atrybutów

W tej sekcji pojawi się, jak dostosować za pomocą atrybutów określających formatowania, sprawdzanie poprawności i reguły mapowania bazy danych modelu danych. Następnie w kilka z następujących sekcji, które należy utworzyć pełną modelu danych służbowych, dodając atrybuty dla klasy została już utworzona i tworzenia nowych klas dla pozostałych typów jednostek w modelu.

### <a name="the-datatype-attribute"></a>Atrybut typu danych

Dat rejestracji dla użytkowników domowych wszystkie strony sieci web obecnie Wyświetl czas wraz z datą, mimo że wszystkie potrzebne informacje dla tego pola to data. Za pomocą atrybutów adnotacji danych, można go utworzyć kod zmian, który naprawi format wyświetlania w każdym widoku, który zawiera dane. Aby wyświetlić przykład sposobu wykonywania, czy zostanie dodana do atrybutu `EnrollmentDate` właściwości w `Student` klasy.

W *Models/Student.cs*, Dodaj `using` instrukcji dla `System.ComponentModel.DataAnnotations` przestrzeni nazw i Dodaj `DataType` i `DisplayFormat` atrybuty do `EnrollmentDate` właściwości, jak pokazano w poniższym przykładzie:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

`DataType` Atrybut służy do określania typu danych, który jest bardziej szczegółowy niż typ wewnętrznej bazy danych. W takim przypadku tylko chcemy śledzić data nie Data i godzina. `DataType` Wyliczenie zawiera wiele typów danych, takich jak daty, godziny, numer telefonu, waluty, EmailAddress i więcej. `DataType` Atrybut można również włączyć aplikacji w celu umożliwienia automatycznie funkcji specyficznych dla typu. Na przykład `mailto:` można tworzyć łącza `DataType.EmailAddress`, i może zostać dostarczony selektora daty `DataType.Date` w przeglądarkach obsługujących HTML5. `DataType` HTML 5 emituje atrybut `data-` atrybutów (wyraźnym danych dash), które byłyby zrozumiałe dla przeglądarki HTML 5. `DataType` Atrybutów nie oferują żadnych sprawdzania poprawności.

`DataType.Date` nie określono format daty, która jest wyświetlana. Domyślnie są wyświetlane w polu danych domyślny format oparte na obiekt CultureInfo serwera.

`DisplayFormat` Atrybut służy do jawnie określić format daty:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode` Ustawienie określa, że formatowanie powinny również będą stosowane, gdy wartość jest wyświetlana w polu tekstowym do edycji. (Nie można że w przypadku niektórych pól — na przykład w przypadku wartości walutowych nie można symbol waluty w polu tekstowym do edycji.)

Można użyć `DisplayFormat` atrybutu przez sam, ale jest zwykle warto użyć `DataType` również atrybutu. `DataType` Atrybut przekazuje semantykę danych zamiast sposób renderowania jej na ekranie i zapewnia następujące korzyści, które nie można uzyskać z `DisplayFormat`:

* Przeglądarki, można włączyć funkcje HTML5 (na przykład do wyświetlenia w formancie kalendarza, symbol waluty odpowiednie ustawienia regionalne, przesyłanie pocztą e-mail łączy, niektóre klienta wejściowe weryfikacji itp.).

* Domyślnie przeglądarka wyświetli danych przy użyciu właściwego formatu oparte na ustawienia regionalne.

Aby uzyskać więcej informacji, zobacz [ \<wejściowych > tagów dokumentacji Pomocnika](../../mvc/views/working-with-forms.md#the-input-tag-helper).

Uruchom aplikację, przejdź do strony indeksu studentów i zwróć uwagę, nie są wyświetlane godziny dla daty rejestracji. Będzie taka sama wartość true dla dowolnego widoku, który używa modelu uczniów.

![Wyświetlanie dat bez godzin strony indeksu uczniów lub studentów](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>Atrybut StringLength

Można również określić reguły sprawdzania poprawności danych i komunikatów o błędach weryfikacji przy użyciu atrybutów. `StringLength` Atrybut Ustawia maksymalną długość w bazie danych i zapewnia po stronie klienta i po stronie serwera weryfikacji dla platformy ASP.NET MVC. Minimalna długość ciągu można również określić, w tym atrybucie, ale wartość minimalna nie ma wpływu na schemat bazy danych.

Załóżmy, że chcesz upewnić się, że użytkownicy nie wprowadzić więcej niż 50 znaków dla nazwy. Aby dodać to ograniczenie, Dodaj `StringLength` atrybuty do `LastName` i `FirstMidName` właściwości, jak pokazano w poniższym przykładzie:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

`StringLength` Atrybutu nie uniemożliwić wprowadzanie biały znak dla nazwy użytkownika. Można użyć `RegularExpression` atrybutu, aby zastosować ograniczenia do danych wejściowych. Na przykład następujący kod wymaga pierwszego znaku się wielkie litery i pozostałych znaków jako alfabetycznej:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

`MaxLength` Atrybutu zapewnia funkcje podobne do `StringLength` atrybutu, ale nie zapewnia po stronie klienta sprawdzania poprawności.

Model bazy danych zostanie zmieniona w taki sposób, który wymaga zmiany w schemacie bazy danych. Aby zaktualizować schemat bez utraty danych, które zostały dodane do bazy danych przy użyciu interfejsu użytkownika aplikacji użyjesz migracji.

Zapisz zmiany i skompilować projekt. Następnie otwórz okno polecenia w folderze projektu i wprowadź następujące polecenia:

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

`migrations add` Polecenie ostrzega, że może dojść do utraty danych, ponieważ zmiana powoduje, że maksymalna długość dla dwóch kolumn.  Migracje tworzy plik o nazwie  *\<sygnatury czasowej > _MaxLengthOnNames.cs*. Ten plik zawiera kod w `Up` metodę, która spowoduje zaktualizowanie bazy danych zgodnie z bieżącym modelem danych. `database update` Polecenie zadziałało kodu.

Sygnatura czasowa prefiksem nazwy pliku migracji jest używany przez programu Entity Framework porządkowania migracji. Przed uruchomieniem polecenia update-database można utworzyć wiele migracji, a następnie wszystkie migracja są stosowane w kolejności, w której zostały utworzone.

Uruchom aplikację, wybierz **studentów** , kliknij pozycję **Utwórz nowy**i wprowadź nazwę, albo więcej niż 50 znaków. Po kliknięciu **Utwórz**, weryfikacji po stronie klienta zawiera komunikat o błędzie.

![Strona wyświetlająca ciąg błędy długość indeksu uczniów lub studentów](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a>Atrybut kolumny

Atrybuty umożliwia także kontrolować sposób z klas i właściwości są mapowane do bazy danych. Załóżmy, że używasz nazwy `FirstMidName` nazwę pierwszego pola, ponieważ pole może także zawierać drugie imię. Ale ma być nazwane kolumna bazy danych `FirstName`, ponieważ użytkownicy, którzy będą Pisanie zapytań ad hoc w bazie danych są zapoznanie tej nazwy. Aby to mapowanie, można użyć `Column` atrybutu.

`Column` Atrybut określa, że po utworzeniu bazy danych, w kolumnie `Student` tabeli, który jest mapowany na `FirstMidName` właściwość o nazwie `FirstName`. Innymi słowy, gdy kod odwołuje się do `Student.FirstMidName`, dane będą pobierane z lub zaktualizowane w `FirstName` kolumny `Student` tabeli. Jeśli nie określisz nazwy kolumn, ich otrzymuje taką samą nazwę jak nazwa właściwości.

W *Student.cs* plików, dodawanie `using` instrukcji dla `System.ComponentModel.DataAnnotations.Schema` i dodać atrybut nazwy kolumny do `FirstMidName` właściwości, jak pokazano w poniższym kodzie wyróżnione:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

Dodanie `Column` atrybutu zmieni model obsługujący `SchoolContext`, więc nie będzie zgodny z bazą danych.

Zapisz zmiany i skompilować projekt. Następnie otwórz okno polecenia w folderze projektu i wprowadź następujące polecenia, aby utworzyć inną migracji:

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

W **Eksplorator obiektów SQL Server**, otwórz projektanta tabel dla użytkowników domowych, klikając dwukrotnie **uczniowie** tabeli.

![Tabela studentów w SSOX po migracji](complex-data-model/_static/ssox-after-migration.png)

Przed zastosowaniem dwóch pierwszych migracji, nazwa kolumny były typu nvarchar(MAX). Są one obecnie nvarchar(50) i nazwę kolumny zmieniła się z FirstMidName na imię.

> [!Note]
> Jeśli spróbujesz skompilować przed zakończeniem Tworzenie wszystkich klas jednostek w poniższych sekcjach można otrzymać błędy kompilatora.

## <a name="final-changes-to-the-student-entity"></a>Końcowe zmiany do jednostki dla użytkowników domowych

![Jednostki dla użytkowników domowych](complex-data-model/_static/student-entity.png)

W *Models/Student.cs*, Zastąp kod dodane wcześniej następujący kod. Zmiany zostały wyróżnione.

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Wymagany atrybut

`Required` Atrybut powoduje, że nazwa właściwości wymaganych pól. `Required` Atrybut nie jest wymagane dla typów wartości null, takich jak typy wartości (DateTime, int, kliknij dwukrotnie, float, itp.). Typy, które nie może mieć wartości null są automatycznie traktowane jako wymagane pola.

Można usunąć `Required` atrybutu i zamień ją na minimalną długość parametru `StringLength` atrybutu:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Atrybut wyświetlania

`Display` Atrybut określa, że podpis pola tekstowe powinny być "Imię", "Nazwisko", "Pełna nazwa" i "Data rejestracji" zamiast nazwy właściwości w każdym wystąpieniu (który nie ma miejsca dzielenia wyrazów).

### <a name="the-fullname-calculated-property"></a>Właściwość obliczona imię i nazwisko

`FullName` jest obliczonej właściwości, która zwraca wartość, która jest tworzona przez łączenie dwóch innych właściwości. W związku z tym ma metodę dostępu get i nie `FullName` kolumny zostanie wygenerowany w bazie danych.

## <a name="create-the-instructor-entity"></a>Utwórz jednostkę instruktora

![Jednostka Instructor](complex-data-model/_static/instructor-entity.png)

Utwórz *Models/Instructor.cs*, zastępując kod szablonu z następującym kodem:

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Zwróć uwagę, że kilka właściwości są takie same, w jednostkach dla użytkowników domowych i instruktora. W [wdrażanie dziedziczenia](inheritance.md) później w tym samouczku będziesz Refaktoryzuj ten kod, aby wyeliminować nadmiarowości.

Możesz też zaznaczyć wiele atrybutów w jednym wierszu, można także zapisać `HireDate` atrybutów w następujący sposób:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>Właściwości nawigacji CourseAssignments i OfficeAssignment

`CourseAssignments` i `OfficeAssignment` właściwości są właściwości nawigacji.

Instruktora nauczyć dowolną liczbę kursów, więc `CourseAssignments` jest zdefiniowany jako kolekcja.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Właściwość nawigacji może zawierać wiele jednostek, w którym wpisów można być dodane, usunięte i zaktualizowane listy musi być typu.  Można określić `ICollection<T>` lub typu, takich jak `List<T>` lub `HashSet<T>`. Jeśli określisz `ICollection<T>`, tworzy EF `HashSet<T>` kolekcji domyślnie.

Przyczyny, dlaczego są `CourseAssignment` jednostek opisanej poniżej w sekcji o relacje wiele do wielu.

Reguły biznesowe contoso University stanu, że instruktora tylko może mieć co najwyżej jeden pakiet office, więc `OfficeAssignment` właściwość przechowuje pojedynczą jednostką OfficeAssignment (co może mieć wartości zerowej, jeśli nie przypisano żadnych pakietu office).

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Utwórz jednostkę OfficeAssignment

![OfficeAssignment jednostki](complex-data-model/_static/officeassignment-entity.png)

Utwórz *Models/OfficeAssignment.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Atrybut klucza

Brak relacji jeden do zero lub jeden między instruktora i jednostkami, OfficeAssignment. Przypisania office występuje tylko w odniesieniu do instruktora, który jest przypisany do, a więc jego klucz podstawowy również jest jego klucza obcego do jednostki instruktora. Ale programu Entity Framework automatycznie nie może rozpoznać InstructorID jako klucz podstawowy tej jednostki, ponieważ jego nazwa nie będzie zgodna z konwencją nazewnictwa Identyfikatora lub classnameID. W związku z tym `Key` atrybut służy do identyfikacji jako klucz:

```csharp
[Key]
public int InstructorID { get; set; }
```

Można również użyć `Key` atrybut, jeśli jednostka ma własny klucz podstawowy, ale ma nazwę właściwości, coś innego niż classnameID lub identyfikator.

Domyślnie EF traktuje klucz jako z systemem innym niż baza danych wygenerowała, ponieważ kolumna jest identyfikujące relacji.

### <a name="the-instructor-navigation-property"></a>Właściwość nawigacji instruktora

Jednostka Instructor ma wartość null `OfficeAssignment` właściwości nawigacji (ponieważ instruktora nie może być przypisanie pakietu office), i jednostki OfficeAssignment ma niedopuszczającą `Instructor` właściwości nawigacji (ponieważ przypisania office nie istnieje bez instruktora — `InstructorID` nie dopuszcza wartości null). Jednostka Instructor ma powiązanej jednostki OfficeAssignment, każdej jednostki ma odwołanie do jeden z nich w jej właściwości nawigacji.

Można umieścić `[Required]` atrybutu we właściwości nawigacji instruktora, aby określić, że muszą być powiązane instruktora, ale nie trzeba to zrobić, ponieważ `InstructorID` wartości null jest klucz obcy (który jest także klucz do tej tabeli).

## <a name="modify-the-course-entity"></a>Modyfikowanie jednostek kursu

![Jednostki ciągu](complex-data-model/_static/course-entity.png)

W *Models/Course.cs*, Zastąp kod dodane wcześniej następujący kod. Zmiany zostały wyróżnione.

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

Jednostka kursu ma właściwości klucza obcego `DepartmentID` wskazujących powiązanej jednostki działu i ma `Department` właściwości nawigacji.

Nie wymaga programu Entity Framework, można dodać właściwości klucza obcego do modelu danych w przypadku właściwości nawigacji dla obiekt pokrewny.  EF automatycznie tworzy klucze obce w bazie danych, wszędzie tam, gdzie są potrzebne i tworzy [w tle właściwości](https://docs.microsoft.com/ef/core/modeling/shadow-properties) dla nich. Jednak po klucz obcy w modelu danych może uaktualniać prostszy i efektywniejszy. Na przykład podczas pobierania jednostki kursu, aby edytować, jednostki działu ma wartość null, jeśli nie zostanie załadowany, więc po zaktualizowaniu jednostki kursu należy najpierw pobrać jednostki działu. Gdy właściwość klucza obcego `DepartmentID` znajduje się w modelu danych, nie trzeba było pobrać jednostki działu, przed uaktualnieniem.

### <a name="the-databasegenerated-attribute"></a>Atrybut DatabaseGenerated

`DatabaseGenerated` Atrybutem `None` parametru `CourseID` właściwość określa, że wartości klucza podstawowego są dostarczane przez użytkownika zamiast wygenerowanych przez bazę danych.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Domyślnie program Entity Framework przyjęto założenie, że wartości klucza podstawowego są generowane przez bazę danych. To już ma w większości przypadków. Jednak dla porach jednostek, będzie używać numer kursu określone przez użytkownika, takie jak seria 1000 działu jednej serii 2000 do innego działu i tak dalej.

`DatabaseGenerated` Atrybutu mogą służyć do generowania wartości domyślne, jak w przypadku kolumn bazy danych używane do rejestrowania daty wiersz został utworzony lub zaktualizowany.  Aby uzyskać więcej informacji, zobacz [wygenerowane właściwości](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Właściwości obcego klucza i nawigacji

Właściwości klucza obcego i właściwości nawigacji w jednostce kursu odzwierciedla się następująco:

Kursu jest przypisany do jednego działu, więc ma `DepartmentID` klucza obcego i `Department` właściwość nawigacji z powodów wymienionych powyżej.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Kursu może mieć dowolną liczbę studentów zarejestrowane, więc `Enrollments` właściwość nawigacji jest kolekcją:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Kursu może organizowane jednocześnie przez wiele instruktorów, więc `CourseAssignments` właściwość nawigacji jest kolekcją (typ `CourseAssignment` objaśniono [później](#many-to-many-relationships)):

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a>Utwórz jednostkę działu

![Dział jednostki](complex-data-model/_static/department-entity.png)


Utwórz *Models/Department.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Atrybut kolumny

Wcześniej używane `Column` atrybutu można zmienić mapowania nazw kolumn. W kodzie dla obiekt działu `Column` atrybutów jest używane do zmienić mapowanie typu danych SQL, tak aby kolumna zostanie zdefiniowana przy użyciu typu money serwera SQL w bazie danych:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Mapowanie kolumny zwykle nie jest wymagane, ponieważ odpowiedni typ danych programu SQL Server, na podstawie typu CLR dla właściwości wybierze programu Entity Framework. Środowisko CLR `decimal` typu map do programu SQL Server `decimal` typu. Ale w takim przypadku wiesz kolumna zostanie zawierający kwot, czy typ danych money jest bardziej odpowiednie dla danego.

### <a name="foreign-key-and-navigation-properties"></a>Właściwości obcego klucza i nawigacji

Właściwości klucza i nawigacja obcego odzwierciedla się następująco:

Dział może lub nie ma uprawnienia administratora, a administrator jest zawsze instruktora. W związku z tym `InstructorID` właściwość jest uwzględniona jako klucza obcego do jednostki instruktora, a znak zapytania są dodawane po `int` określenie można oznaczyć jako wartości null właściwość typu. Właściwość nawigacji jest o nazwie `Administrator` , ale zawiera jednostki instruktora:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Dział może mieć wiele kursów, więc ma właściwości nawigacji kursy:

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> Według Konwencji programu Entity Framework umożliwia usuwanie kaskadowe dla klucza obcego nie dopuszcza wartości null i relacje wiele do wielu. Może to spowodować cykliczne cascade delete reguł, które spowoduje, że wystąpił wyjątek podczas próby dodania migracji. Na przykład jeśli właściwość Department.InstructorID nie zostały zdefiniowane jako wartości null, EF skonfigurować reguły usuwania kaskadowego można usunąć instruktora po usunięciu działu, które nie mają mieć wystąpić. W razie potrzeby reguł biznesowych `InstructorID` właściwości dopuszczać wartości null, użyj następującej instrukcji interfejsu API fluent, aby wyłączyć usuwanie kaskadowe w relacji musi:
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a>Modyfikowanie jednostek rejestracji

![Jednostki rejestracji](complex-data-model/_static/enrollment-entity.png)

W *Models/Enrollment.cs*, Zastąp kod dodane wcześniej następujący kod:

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Właściwości obcego klucza i nawigacji

Właściwości klucza obcego i właściwości nawigacji odzwierciedla się następująco:

Rekord rejestracji dotyczy jednego ciągu, więc ma `CourseID` właściwości kluczy obcych i `Course` właściwości nawigacji:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Rekord rejestracji jest dla pojedynczego studentów, więc ma `StudentID` właściwości kluczy obcych i `Student` właściwości nawigacji:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relacje wiele do wielu

Brak relacji wiele do wielu między jednostkami dla użytkowników domowych i kursu i jednostki rejestracji działa jako tabelę sprzężenia wiele do wielu *z ładunku* w bazie danych. "Za pomocą ładunku" oznacza, że w tabeli rejestracji zawiera dodatkowe dane poza kluczy obcych, dołączonego do tabel (w tym przypadku klucza podstawowego i właściwości klasy).

Na poniższej ilustracji przedstawiono, jak wyglądają te relacje w diagramie jednostki. (Ten diagram został wygenerowany za pomocą narzędzia Entity Framework zasilania dla EF 6.x; Tworzenie diagramu nie stanowi części samouczka, po prostu jest on używany jako ilustrację.)

![Uczniów kursu wiele do wielu](complex-data-model/_static/student-course.png)

Każdy wiersz relacji ma 1 w jeden element end i znak gwiazdki (*) w innych, wskazując relacji jeden do wielu.

Jeśli w tabeli rejestracji nie włączono informacji o kategorii, tylko będzie konieczne zawiera dwa klucze obce CourseID i StudentID. W takim przypadku byłoby wiele do wielu sprzężenia tabeli bez ładunku (lub tabeli czysty sprzężenia) w bazie danych. Jednostki instruktora i kursu mieć typu relacji wiele do wielu, a następnym krokiem jest utworzenie klasę jednostki do działania jako tabelę sprzężenia bez ładunku.

(Niejawne sprzężenie tabel dla relacji wiele do wielu, ale podstawowe EF nie obsługuje 6.x EF. Aby uzyskać więcej informacji, zobacz [dyskusji w repozytorium EF Core GitHub](https://github.com/aspnet/EntityFramework/issues/1368).) 

## <a name="the-courseassignment-entity"></a>Jednostka CourseAssignment

![CourseAssignment jednostki](complex-data-model/_static/courseassignment-entity.png)

Utwórz *Models/CourseAssignment.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a>Dołącz do nazwy podmiotu

Tabela sprzężenia jest wymagana w bazie danych dla relacji wiele do wielu instruktora do szkolenia, a musi być reprezentowane przez zestaw jednostek. Często jest nazwa jednostki sprzężenia `EntityName1EntityName2`, w tym przypadku byłoby `CourseInstructor`. Jednak zaleca się, że wybierz nazwę, która opisuje relację. Modeli danych uruchamiane prosty i wzrostu za pomocą sprzężeń ładunku nie często pobierania ładunków później. Rozpoczyna nazwę opisową jednostki, nie musisz zmienić nazwę. W idealnym przypadku jednostki sprzężenia musi własną nazwę fizycznych (prawdopodobnie pojedynczego wyrazu) w domenie biznesowych. Na przykład książki i klientów można połączyć za pośrednictwem klasyfikacji. Dla tej relacji `CourseAssignment` jest lepszym rozwiązaniem niż `CourseInstructor`.

### <a name="composite-key"></a>Klucz złożony

Ponieważ klucze obce nie dopuszcza wartości null i razem jednoznacznie zidentyfikować każdego wiersza tabeli, nie istnieje potrzeba oddzielne klucza podstawowego. *InstructorID* i *CourseID* właściwości powinny działać jako klucz podstawowy złożonego. Jedynym sposobem, aby zidentyfikować złożonych kluczy podstawowych, aby EF polega na użyciu *interfejsu API fluent* (go nie można wykonać przy użyciu atrybutów). Jak skonfigurować złożonego klucza podstawowego w następnej sekcji zostanie wyświetlone.

Klucz złożony gwarantuje, że natomiast może mieć wiele wierszy dla przebiegu jednego i wielu wierszy dla jednego instruktora, nie może mieć wiele wierszy dla tego samego instruktora oraz przebiegu. `Enrollment` Jednostki sprzężenia definiuje własny klucz podstawowy, więc możliwe są duplikatami tego sortowania. Aby zapobiec takiej duplikatów, możesz można dodać unikatowego indeksu na pola klucza obcego, lub skonfigurować `Enrollment` z głównej klucz złożony, podobnie jak `CourseAssignment`. Aby uzyskać więcej informacji, zobacz [indeksów](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-database-context"></a>Aktualizacja kontekst bazy danych

Dodaj następujący wyróżniony kod, aby *Data/SchoolContext.cs* pliku:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Ten kod dodaje nowe jednostki i konfiguruje jednostki CourseAssignment złożonego klucza podstawowego.

## <a name="fluent-api-alternative-to-attributes"></a>Zamiast interfejsu API Fluent atrybutów

Kod w `OnModelCreating` metody `DbContext` klasy używa *interfejsu API fluent* można skonfigurować zachowanie EF. Interfejs API jest nazywany "fluent", ponieważ jest często używany przez instalowanie szereg wywołania metody razem w jednej instrukcji, jak w poniższym przykładzie z [EF podstawowej dokumentacji](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

W tym samouczku używasz interfejsu API fluent tylko w przypadku mapowanie bazy danych, które nie są z atrybutami. Jednak umożliwia także interfejsu API fluent określić większość formatowania, sprawdzanie poprawności i reguły mapowania, które możesz wykonać za pomocą atrybutów. Niektóre atrybuty, takie jak `MinimumLength` nie można zastosować z interfejsu API fluent. Jak wspomniano wcześniej, `MinimumLength` nie powoduje zmiany schematu, ma zastosowanie tylko reguły sprawdzania poprawności po stronie klienta i serwera.

Niektórzy deweloperzy wolą Użyj interfejsu API fluent wyłącznie tak, aby ich zachowanie ich klasami jednostki "Wyczyść". Jeśli chcesz, i istnieje kilka dostosowania, które jest możliwe tylko przy użyciu interfejsu API fluent można mieszać atrybutów i wygodnego interfejsu API, ale na ogół zalecaną praktyką jest wybierz jedną z tych dwóch metod i użyj to spójnie możliwie. Jeśli używasz zarówno, należy pamiętać, że zawsze, gdy wystąpi konflikt, interfejsu API Fluent zastępuje atrybutów.

Aby uzyskać więcej informacji dotyczących atrybutów w porównaniu z interfejsu API fluent, zobacz [metody konfiguracji](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Jednostki Diagram przedstawiający relacje

Na poniższej ilustracji przedstawiono diagram do narzędzia Entity Framework zasilania utworzyć dla ukończonych modelu służbowe.

![Diagram jednostek](complex-data-model/_static/diagram.png)

Oprócz linii relacji jeden do wielu (od 1 do \*), można widoczną w tym miejscu wiersza relacji jeden do zero lub jeden (1 do od 0 do 1) między jednostkami instruktora i OfficeAssignment i linię relacji zero lub jeden do wielu (od 0 do 1 do *) między Jednostki instruktora i działu.

## <a name="seed-the-database-with-test-data"></a>Inicjatora bazy danych z danych testowych

Zastąp kod w *Data/DbInitializer.cs* pliku następującym kodem w celu zapewnienia danych inicjatora nowe jednostki po utworzeniu.

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

Jak przedstawiono w pierwszym samouczku większość ten kod po prostu tworzy nowe obiekty jednostki i ładuje przykładowych danych do właściwości wymaganych do testowania. Zwróć uwagę, jak relacje wiele do wielu są obsługiwane: kod tworzy relacje przez tworzenie jednostek w `Enrollments` i `CourseAssignment` join zestawów jednostek.

## <a name="add-a-migration"></a>Dodaj migracji

Zapisz zmiany i skompilować projekt. Następnie otwórz okno polecenia w folderze projektu i wprowadź `migrations add` polecenie (nie wykonuj polecenia update-database jeszcze):

```console
dotnet ef migrations add ComplexDataModel
```

Zostanie wyświetlone ostrzeżenie o możliwości utraty danych.

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Jeśli próbujesz uruchomić `database update` polecenia w tym momencie (nie należy go jeszcze) jak następujący błąd:

> Instrukcja ALTER TABLE jest w konflikcie z ograniczenie FOREIGN KEY "FK_dbo. Course_dbo. Department_DepartmentID". Konflikt wystąpił w bazie danych "ContosoUniversity" tabeli "dbo. Dział", kolumna"DepartmentID".

Czasami podczas wykonywania migracji z istniejącymi danymi, należy wstawić dane do bazy danych do zaspokojenia ograniczeń klucza obcego. Wygenerowany kod w `Up` metody dodaje klucz obcy DepartmentID wartości null do tabeli kursu. Jeśli istnieje już wierszy w tabeli kursu po uruchomieniu kodu `AddColumn` operacja nie powiedzie się, ponieważ program SQL Server nie może ustalić, jaka wartość w kolumnie, która nie może mieć wartości null. W tym samouczku uruchomisz migracji na nową bazę danych, ale w aplikacji produkcyjnej trzeba będzie dokonanie migracji obsługuje istniejących danych, dlatego poniższe instrukcje przedstawiają przykłady, jak to zrobić.

Aby migracja pracy z istniejącymi danymi, trzeba zmienić kod, aby podać nową kolumnę wartości domyślnej, a następnie utwórz działu stub o nazwie "Temp" jako domyślnego działu. W związku z tym istniejących wierszy kursu zostaną wszystkie powiązane do działu "Temp" po `Up` uruchamia metody.

* Otwórz *{timestamp}_ComplexDataModel.cs* pliku. 

* Komentarz wiersz kodu, który dodaje kolumnę DepartmentID tabeli kursu.

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* Dodaj następujący wyróżniony kod po kod, który tworzy tabelę działu:

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

W aplikacji produkcyjnej możesz zapisać kodu lub skryptów służących do dodawania wierszy działu i dotyczą wierszy kursu nowych wierszy działu. Następnie już nie musisz dział "Temp" lub wartość domyślną w kolumnie Course.DepartmentID.

Zapisz zmiany i skompilować projekt.

## <a name="change-the-connection-string-and-update-the-database"></a>Zmień parametry połączenia i zaktualizować bazę danych

Masz teraz nowy kod `DbInitializer` klasy, która dodaje nowe jednostki dane do pustej bazy danych. Aby EF, Utwórz nową, pustą bazę danych, Zmień nazwę bazy danych w parametrach połączenia w *appsettings.json* ContosoUniversity3 lub inna nazwa, który nie był używany na komputerze korzystasz.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

Zapisz zmiany w *appsettings.json*.

> [!NOTE]
> Alternatywą wobec zmiana nazwy bazy danych można usunąć bazy danych. Użyj **Eksplorator obiektów SQL Server** (SSOX) lub `database drop` polecenia interfejsu wiersza polecenia:
> ```console
> dotnet ef database drop
> ```

Po zmianie nazwy bazy danych lub usunąć bazy danych uruchom `database update` polecenie w oknie wiersza polecenia do wykonania migracji.

```console
dotnet ef database update
```

Uruchom aplikację, aby spowodować `DbInitializer.Initialize` metodę, aby uruchomić i wypełnić nowej bazy danych.

Otwórz bazę danych w SSOX tak samo jak wcześniej, a następnie rozwiń **tabel** węzeł, aby zobaczyć, czy wszystkie tabele zostały utworzone. (Jeśli nadal masz SSOX otworzyć z wcześniejszego stanu, kliknij przycisk **Odśwież** przycisku.)

![Tabele w SSOX](complex-data-model/_static/ssox-tables.png)

Uruchom aplikację do wyzwolenia kodu inicjatora określającej wartość początkową bazy danych.

Kliknij prawym przyciskiem myszy **CourseAssignment** tabeli i wybierz **danych widoku** Aby sprawdzić, czy zawiera dane w nim.

![Dane CourseAssignment w SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a>Podsumowanie

Masz teraz bardziej złożonych modelu danych i odpowiednią bazę danych. Następujące samouczka dowiesz się więcej na temat dostępu do danych powiązanych.

> [!div class="step-by-step"]
> [Poprzednie](migrations.md)
> [dalej](read-related-data.md)  
