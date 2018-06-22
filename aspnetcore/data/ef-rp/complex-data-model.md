---
title: Stron razor podstawowych EF w platformy ASP.NET Core - Model danych — 5 8
author: rick-anderson
description: W tym samouczku Dodaj więcej jednostki i relacje i dostosować modelu danych, określając formatowania, sprawdzanie poprawności i mapowanie reguły.
ms.author: riande
ms.date: 10/25/2017
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: a885809205f13e1090a957496710cc0d9c7257c0
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274544"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a>Stron razor podstawowych EF w platformy ASP.NET Core - Model danych — 5 8

Przez [Dykstra Tomasz](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

Samouczki poprzedniej pracy z modelem danych podstawowych składającą się z trzech jednostek. W tym samouczku:

* Więcej jednostki i relacje są dodawane.
* Model danych jest dostosowane przez określenie formatowania, sprawdzanie poprawności i reguły mapowania bazy danych.

Na poniższej ilustracji pokazano klas jednostek w modelu danych zakończone:

![Diagram jednostek](complex-data-model/_static/diagram.png)

Jeśli wystąpiły problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji dla tego etapu](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).

## <a name="customize-the-data-model-with-attributes"></a>Dostosowywanie modelu danych z atrybutami

W tej sekcji modelu danych jest dostosowane przy użyciu atrybutów.

### <a name="the-datatype-attribute"></a>Atrybut typu danych

Na stronach uczniów obecnie Wyświetla czas Data rejestracji. Zazwyczaj Data zawierają tylko data i czas nie.

Aktualizacja *Models/Student.cs* z następującymi wyróżniony kod:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

[DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) atrybut określa typ danych, który jest bardziej szczegółowy niż typ wewnętrznej bazy danych. W tym przypadku powinien zostać wyświetlony tylko data nie daty i godziny. [Wyliczenie DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) zawiera wiele typów danych, takie jak data, czas, numer telefonu, waluty, EmailAddress itp. `DataType` Atrybut można również włączyć automatycznie udostępnić funkcji specyficznych dla typu aplikacji. Na przykład:

* `mailto:` Link jest tworzony automatycznie dla `DataType.EmailAddress`.
* Selektor Data jest dostępne w celu `DataType.Date` w większości przeglądarek.

`DataType` HTML 5 emituje atrybut `data-` atrybutów (dash wyraźnym danych), które korzystać z przeglądarki HTML 5. `DataType` Atrybutów nie mają funkcje sprawdzania poprawności.

`DataType.Date` nie określono format daty, która jest wyświetlana. Domyślnie pole daty są wyświetlane domyślne formaty oparte na tym serwerze [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).

`DisplayFormat` Atrybut służy do jawnie określić format daty:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode` Ustawienie określa, że formatowanie powinny również będą stosowane do edycji interfejsu użytkownika. Niektóre pola nie powinny używać `ApplyFormatInEditMode`. Na przykład symbol waluty zazwyczaj nie powinny być wyświetlane w polu edycji.

`DisplayFormat` Atrybut może być używany przez samego siebie. Zazwyczaj jest warto użyć `DataType` atrybutem `DisplayFormat` atrybutu. `DataType` Atrybut przekazuje semantykę danych zamiast sposób renderowania jej na ekranie. `DataType` Atrybut zapewnia następujące korzyści, które nie są dostępne w `DisplayFormat`:

* Przeglądarki, można włączyć funkcje HTML5. Na przykład wyświetlić formant kalendarza, symbol waluty odpowiednie ustawienia regionalne, przesyłanie pocztą e-mail łączy, sprawdzania poprawności danych wejściowych po stronie klienta,... itd.
* Domyślnie przeglądarki Renderowanie danych przy użyciu właściwego formatu oparte na ustawienia regionalne.

Aby uzyskać więcej informacji, zobacz [ \<wejściowych > pomocnika tagów dokumentacji](xref:mvc/views/working-with-forms#the-input-tag-helper).

Uruchom aplikację. Przejdź do strony indeksu studenta. Nie są wyświetlane godziny. Każdy widok, który używa `Student` modelu Wyświetla datę bez czasu.

![Wyświetlanie dat bez godzin strony indeksu uczniów lub studentów](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>Atrybut StringLength

Atrybuty można określić reguły sprawdzania poprawności danych i komunikatów o błędach weryfikacji. [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) atrybut określa minimalną i maksymalną długość znaki, które są dozwolone w polu danych. `StringLength` Atrybut udostępnia również weryfikację po stronie klienta i po stronie serwera. Wartość minimalna nie ma wpływu na schemat bazy danych.

Aktualizacja `Student` modelu z następującym kodem:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

Poprzedni kod ogranicza nazwy do nie więcej niż 50 znaków. `StringLength` Atrybutu nie uniemożliwić wprowadzanie biały znak dla nazwy użytkownika. [Wyrażenia regularnego](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) atrybut jest używany, aby zastosować ograniczenia do danych wejściowych. Na przykład następujący kod wymaga pierwszego znaku się wielkie litery i pozostałych znaków jako alfabetycznej:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

Uruchom aplikację:

* Przejdź do strony studenta.
* Wybierz **Utwórz nowy**, a następnie wprowadź nazwa jest dłuższa niż 50 znaków.
* Wybierz **Utwórz**, weryfikacji po stronie klienta zawiera komunikat o błędzie.

![Strona wyświetlająca ciąg błędy długość indeksu uczniów lub studentów](complex-data-model/_static/string-length-errors.png)

W **Eksplorator obiektów SQL Server** (SSOX), otwórz projektanta tabel dla użytkowników domowych, klikając dwukrotnie **uczniowie** tabeli.

![Tabela studentów w SSOX przed migracji](complex-data-model/_static/ssox-before-migration.png)

Na powyższej ilustracji przedstawiono schematu `Student` tabeli. Nazwa pola ma typ `nvarchar(MAX)` ponieważ migracji nie zostało uruchomione w bazie danych. Podczas migracji są uruchamiane w dalszej części tego samouczka, staje się nazwa pola `nvarchar(50)`.

### <a name="the-column-attribute"></a>Atrybut kolumny

Atrybuty można kontrolować, jak klas i właściwości są mapowane do bazy danych. W tej sekcji `Column` atrybut służy do mapowania nazwy `FirstMidName` dla właściwości "Imię" w bazie danych.

Po utworzeniu bazy danych, nazwy właściwości w modelu są używane dla nazw kolumn (z wyjątkiem sytuacji, gdy `Column` użyć atrybutu).

`Student` Model używa `FirstMidName` nazwę pierwszego pola, ponieważ pole może także zawierać drugie imię.

Aktualizacja *Student.cs* pliku następującym kodem wyróżnione:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

Z tej zmiany `Student.FirstMidName` w aplikacji mapowana `FirstName` kolumny `Student` tabeli.

Dodanie `Column` atrybutu zmieni model obsługujący `SchoolContext`. Model obsługujący `SchoolContext` nie jest już zgodny z bazy danych. Jeśli aplikacja jest uruchamiana przed zastosowaniem migracji, generowany jest następujący wyjątek:

```SQL
SqlException: Invalid column name 'FirstName'.
```
Aby zaktualizować bazę danych:

* Skompiluj projekt.
* Otwórz okno polecenia w folderze projektu. Wprowadź następujące polecenia do tworzenia nowych migracji i aktualizacji bazy danych:

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

`dotnet ef migrations add ColumnFirstName` Polecenie generuje następujący komunikat ostrzegawczy:

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

To ostrzeżenie jest generowany, ponieważ nazwa pola są teraz ograniczone do 50 znaków. Jeśli nazwy w bazie danych ma więcej niż 50 znaków, od 51 do ostatniego znaku może spowodować utratę.

* Testowanie aplikacji.

Otwórz tabelę uczniów w SSOX:

![Tabela studentów w SSOX po migracji](complex-data-model/_static/ssox-after-migration.png)

Przed zastosowaniem migracji, nazwa kolumny zostały typu [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql). Nazwa kolumny są teraz `nvarchar(50)`. Zmieniono nazwę kolumny z `FirstMidName` do `FirstName`.

> [!Note]
> W poniższej sekcji Tworzenie aplikacji na niektórych etapach generuje błędy kompilatora. Instrukcje Określ, kiedy do tworzenia aplikacji.

## <a name="student-entity-update"></a>Aktualizacja encji uczniów

![Jednostki dla użytkowników domowych](complex-data-model/_static/student-entity.png)

Aktualizacja *Models/Student.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Wymagany atrybut

`Required` Atrybut powoduje, że nazwa właściwości wymaganych pól. `Required` Atrybut nie jest wymagane dla typów wartości null, takich jak typy wartości (`DateTime`, `int`, `double`itp.). Typy, które nie może mieć wartości null są automatycznie traktowane jako wymagane pola.

`Required` Atrybutu mogły zostać zastąpione z minimalną długość parametru w `StringLength` atrybutu:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Atrybut wyświetlania

`Display` Atrybut określa, że podpis pola tekstowe powinny "Imię", "Nazwisko", "Pełna nazwa" i "Data rejestracji". Podpisy domyślne spacji, nie dzielenia wyrazów, na przykład "Lastname." było

### <a name="the-fullname-calculated-property"></a>Właściwość obliczona imię i nazwisko

`FullName` jest obliczonej właściwości, która zwraca wartość, która jest tworzona przez łączenie dwóch innych właściwości. `FullName` Nie można ustawiać, ma akcesora get. Nie `FullName` kolumny jest tworzony w bazie danych.

## <a name="create-the-instructor-entity"></a>Utwórz jednostkę instruktora

![Jednostka Instructor](complex-data-model/_static/instructor-entity.png)

Utwórz *Models/Instructor.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Zwróć uwagę, że kilka właściwości są takie same, w `Student` i `Instructor` jednostek. W samouczku wdrażanie dziedziczenia w dalszej części tej serii ten kod został zrefaktoryzowany wyeliminować nadmiarowości.

Wiele atrybutów może być w jednym wierszu. `HireDate` Atrybuty można zapisać w następujący sposób:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>Właściwości nawigacji CourseAssignments i OfficeAssignment

`CourseAssignments` i `OfficeAssignment` właściwości są właściwości nawigacji.

Instruktora nauczyć dowolną liczbę kursów, więc `CourseAssignments` jest zdefiniowany jako kolekcja.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Jeśli właściwość nawigacji posiada wiele jednostek:

* Musi być typu listy, w którym wpisy mogą być dodawane, usunąć lub zaktualizować.

Typy właściwości nawigacji:

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

Jeśli `ICollection<T>` określono tworzy EF Core `HashSet<T>` kolekcji domyślnie.

`CourseAssignment` Jednostki znajduje się w sekcji w relacji wiele do wielu.

Firm Contoso University zasady stanu, że instruktora może mieć co najwyżej jednego pakietu office. `OfficeAssignment` Właściwość przechowuje pojedynczy `OfficeAssignment` jednostki. `OfficeAssignment` ma wartość null, jeśli nie przypisano żadnych pakietu office.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Utwórz jednostkę OfficeAssignment

![OfficeAssignment jednostki](complex-data-model/_static/officeassignment-entity.png)

Utwórz *Models/OfficeAssignment.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Atrybut klucza

`[Key]` Atrybut służy do identyfikowania właściwości jako klucz podstawowy (PK) gdy nazwa właściwości coś innego niż classnameID lub identyfikator.

Brak relacji jeden do zero lub jeden między `Instructor` i `OfficeAssignment` jednostek. Przypisania office występuje tylko w odniesieniu do instruktora, który jest przypisany do. `OfficeAssignment` Klucz prywatny jest również jego klucz obcy (klucz OBCY) `Instructor` jednostki. Podstawowy EF nie może automatycznie rozpoznaje `InstructorID` jako klucz podstawowy z `OfficeAssignment` ponieważ:

* `InstructorID` nie będzie zgodna z konwencją nazewnictwa Identyfikatora lub classnameID.

W związku z tym `Key` atrybut służy do identyfikowania `InstructorID` jako klucz podstawowy:

```csharp
[Key]
public int InstructorID { get; set; }
```

Domyślnie EF Core traktuje klucz jako z systemem innym niż baza danych wygenerowała, ponieważ kolumna jest identyfikujące relacji.

### <a name="the-instructor-navigation-property"></a>Właściwość nawigacji instruktora

`OfficeAssignment` Właściwości nawigacji dla `Instructor` jednostka ma wartość null ponieważ:

* Odwołanie typy (takich jak klasy dopuszczają wartości null).
* Instruktor może nie mieć przypisania pakietu office.


`OfficeAssignment` Jednostka ma niedopuszczającą `Instructor` właściwość nawigacji ponieważ:

* `InstructorID` jest wartości null.
* Przypisania pakietu office nie może istnieć bez instruktora.

Gdy `Instructor` jednostka ma powiązanego `OfficeAssignment` jednostki, każdy obiekt ma odwołanie do jeden z nich w jej właściwości nawigacji.

`[Required]` Można zastosować atrybutu do `Instructor` właściwość nawigacji:

```csharp
[Required]
public Instructor Instructor { get; set; }
```

Poprzedni kod określa, że muszą być powiązane instruktora. Poprzedni kod nie jest konieczne ponieważ `InstructorID` klucz obcy (która jest również PK) jest wartości null.

## <a name="modify-the-course-entity"></a>Modyfikowanie jednostek kursu

![Jednostki ciągu](complex-data-model/_static/course-entity.png)

Aktualizacja *Models/Course.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

`Course` Jednostka ma właściwość klucza obcego (klucz OBCY) `DepartmentID`. `DepartmentID` Wskazuje pokrewny `Department` jednostki. `Course` Jednostka ma `Department` właściwości nawigacji.

Podstawowe EF nie wymaga właściwości klucza Obcego dla modelu danych, jeśli model ma właściwości nawigacji dla obiekt pokrewny.

Wszędzie tam, gdzie są potrzebne, EF Core automatycznie tworzy FKs w bazie danych. Tworzy EF Core [w tle właściwości](https://docs.microsoft.com/ef/core/modeling/shadow-properties) dla FKs automatycznie utworzone. Po klucz OBCY w modelu danych można uaktualniać prostszy i efektywniejszy. Rozważmy na przykład model którym właściwości klucza Obcego `DepartmentID` jest *nie* uwzględnione. Gdy jednostki ciągu jest pobierana do edycji:

* `Department` Jednostki ma wartość null, jeśli nie są jawnie został załadowany.
* Do zaktualizowania jednostki kursu `Department` jednostki najpierw musi zostać pobrana.

Gdy właściwość klucza Obcego `DepartmentID` znajduje się w modelu danych nie jest konieczne do pobierania `Department` jednostki przed aktualizacją.

### <a name="the-databasegenerated-attribute"></a>Atrybut DatabaseGenerated

`[DatabaseGenerated(DatabaseGeneratedOption.None)]` Atrybut określa, że klucz prywatny jest udostępniany przez aplikację, a nie wygenerowanych przez bazę danych.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Domyślnie EF Core zakłada, że wartości klucza podstawowego są generowane przez bazę danych. DB wygenerowany klucz podstawowy wartości jest zwykle najlepszym rozwiązaniem. Aby uzyskać `Course` PK. określa jednostki, na użytkownika Na przykład liczba kursu takich jak seria 1000 dla działu matematyczne, serii 2000 działu angielskiej wersji językowej.

`DatabaseGenerated` Atrybutu mogą służyć do generowania wartości domyślne. Na przykład bazy danych może automatycznie generować pole daty, aby zarejestrować Data wiersz został utworzony lub zaktualizowany. Aby uzyskać więcej informacji, zobacz [wygenerowane właściwości](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Właściwości obcego klucza i nawigacji

Właściwości klucza obcego (klucz OBCY) i właściwości nawigacji w `Course` jednostkę odzwierciedlić się następująco:

Kursu jest przypisany do jednego działu, więc ma `DepartmentID` klucza Obcego i `Department` właściwości nawigacji.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Kursu może mieć dowolną liczbę studentów zarejestrowane, więc `Enrollments` właściwość nawigacji jest kolekcją:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Kursu może organizowane jednocześnie przez wiele instruktorów, więc `CourseAssignments` właściwość nawigacji jest kolekcją:

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment` objaśniono [później](#many-to-many-relationships).

## <a name="create-the-department-entity"></a>Utwórz jednostkę działu

![Dział jednostki](complex-data-model/_static/department-entity.png)

Utwórz *Models/Department.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Atrybut kolumny

Wcześniej `Column` użyto atrybutu można zmienić mapowania nazw kolumn. W kodzie `Department` jednostki, `Column` atrybut służy do zmiany mapowanie typu danych SQL. `Budget` Zdefiniowana kolumna w bazie danych przy użyciu typu money SQL Server:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Mapowanie kolumny zwykle nie jest wymagane. Podstawowe EF zazwyczaj wybiera odpowiedni typ danych programu SQL Server, na podstawie typu CLR dla właściwości. Środowisko CLR `decimal` typu map do programu SQL Server `decimal` typu. `Budget` jest waluty, a typ danych money jest bardziej odpowiednie dla waluty.

### <a name="foreign-key-and-navigation-properties"></a>Właściwości obcego klucza i nawigacji

Właściwości klucza Obcego i nawigacja odzwierciedla się następująco:

* Dział może lub nie ma uprawnienia administratora.
* Administrator jest zawsze instruktora. W związku z tym `InstructorID` właściwość jest uwzględniona jako klucz OBCY do `Instructor` jednostki.

Właściwość nawigacji jest o nazwie `Administrator` , ale zawiera `Instructor` jednostki:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Znak zapytania (?) w powyższym kodzie Określa, czy właściwość dopuszcza wartość null.

Dział może mieć wiele kursów, więc ma właściwości nawigacji kursy:

```csharp
public ICollection<Course> Courses { get; set; }
```

Uwaga: W Konwencji, EF Core umożliwia usuwanie kaskadowe FKs wartości null i relacje wiele do wielu. Usuwanie kaskadowe może spowodować cykliczne cascade delete reguły. Cykliczne Kaskadowo usuń reguły powoduje, że wystąpił wyjątek podczas migracji jest dodawany.

Na przykład jeśli `Department.InstructorID` właściwości nie został zdefiniowany jako dopuszczający wartość null:

* Podstawowe EF konfiguruje reguły usuwania kaskadowego można usunąć instruktora po usunięciu działu.
* Usuwanie instruktora po usunięciu działu nie jest to oczekiwane zachowanie.

W razie potrzeby reguły biznesowe `InstructorID` właściwości dopuszczać wartości null, użyj następujących instrukcji interfejsu API fluent:

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

Poprzedni kod wyłącza usuwanie kaskadowe relacji instruktora działu.

## <a name="update-the-enrollment-entity"></a>Aktualizacja encji rejestracji

Rekord rejestracji jest jeden kursu wykonywaną przez jeden uczniów.

![Jednostki rejestracji](complex-data-model/_static/enrollment-entity.png)

Aktualizacja *Models/Enrollment.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Właściwości obcego klucza i nawigacji

Właściwości klucza Obcego i właściwości nawigacji odzwierciedla się następująco:

Rekord rejestracji dotyczy jednego ciągu, więc ma `CourseID` właściwości klucza Obcego i `Course` właściwości nawigacji:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Rekord rejestracji jest dla jednego studentów, więc ma `StudentID` właściwości klucza Obcego i `Student` właściwości nawigacji:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relacje wiele do wielu

Relację wiele do wielu między `Student` i `Course` jednostek. `Enrollment` Jednostki działa jako sprzężenia wiele do wielu tabela *z ładunku* w bazie danych. "Za pomocą ładunku" oznacza, że `Enrollment` tabela zawiera dodatkowe dane poza FKs dla połączonych tabel (w tym przypadku na PK i `Grade`).

Na poniższej ilustracji przedstawiono, jak wyglądają te relacje w diagramie jednostki. (Ten diagram został wygenerowany za pomocą narzędzia Power EF dla EF 6.x. Tworzenie diagramu nie jest częścią tego samouczka).

![Uczniów kursu wiele do wielu](complex-data-model/_static/student-course.png)

Każdy wiersz relacji ma 1 w jeden element end i znak gwiazdki (*) w innych, wskazując relacji jeden do wielu.

Jeśli `Enrollment` tabeli nie włączono informacji o kategorii, go tylko musi zawierać dwa FKs (`CourseID` i `StudentID`). Wiele do wielu sprzężenia tabeli bez ładunku jest niekiedy nazywany tabeli czysty sprzężenia (PJT).

`Instructor` i `Course` jednostek ma relację wiele do wielu, przy użyciu tabeli czysty sprzężenia.

Uwaga: Niejawna sprzężenia tabel dla relacji wiele do wielu, ale podstawowe EF nie obsługuje 6.x EF. Aby uzyskać więcej informacji, zobacz [wiele do wielu relacji w programie EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).

## <a name="the-courseassignment-entity"></a>Jednostka CourseAssignment

![CourseAssignment jednostki](complex-data-model/_static/courseassignment-entity.png)

Utwórz *Models/CourseAssignment.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a>Instruktora do szkolenia

![M:M instruktora do szkolenia](complex-data-model/_static/courseassignment.png)

Relacja wiele do wielu instruktora do kursów:

* Wymaga tabeli sprzężenia, który musi być reprezentowany przez zestaw jednostek.
* Znajduje się tabela czysty sprzężenia (tabeli bez ładunku).

Często jest nazwa jednostki sprzężenia `EntityName1EntityName2`. Na przykład tabela sprzężenia instruktora-kursy użycia tego wzorca jest `CourseInstructor`. Jednak zaleca się przy użyciu nazwy, który opisuje relację.

Modele danych uruchamiane prosty i powiększania. Sprzężenia nie ładunku (PJTs) często rozwijać, aby uwzględnić ładunku. Począwszy od nazwę opisową jednostki, nazwa nie trzeba zmienić po zmianie tabeli sprzężenia. W idealnym przypadku jednostki sprzężenia musi własną nazwę fizycznych (prawdopodobnie pojedynczego wyrazu) w domenie biznesowych. Na przykład książki i klientów można połączyć z jednostką sprzężenia o nazwie klasyfikacji. Dla tej relacji wiele do wielu instruktora do kursów `CourseAssignment` jest preferowana przez `CourseInstructor`.

### <a name="composite-key"></a>Klucz złożony

FKs nie są wartości null. Dwa FKs w `CourseAssignment` (`InstructorID` i `CourseID`) ze sobą jednoznacznie zidentyfikować każdy wiersz `CourseAssignment` tabeli. `CourseAssignment` nie wymaga dedykowanego PK. `InstructorID` i `CourseID` właściwości działać jako PK. złożone Jedynym sposobem Określ złożonego PKs na rdzeń EF jest z *interfejsu API fluent*. W następnej części pokazano sposób konfigurowania PK. złożone

Klucz złożony zapewnia:

* Wiele wierszy są dozwolone dla porach jeden.
* Wiele wierszy są dozwolone dla jednego instruktora.
* Wiele wierszy dla tego samego instruktora i kursu nie jest dozwolona.

`Enrollment` Jednostki sprzężenia definiuje własny klucz podstawowy, więc możliwe są duplikatami tego sortowania. Aby uniknąć tych duplikaty:

* Dodaj unikatowy indeks dla pól klucza Obcego lub
* Skonfiguruj `Enrollment` z głównej klucz złożony, podobnie jak `CourseAssignment`. Aby uzyskać więcej informacji, zobacz [indeksów](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-db-context"></a>Zaktualizuj kontekst bazy danych

Dodaj następujący wyróżniony kod, aby *Data/SchoolContext.cs*:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Poprzedni kod dodaje nowe jednostki i konfiguruje `CourseAssignment` PK. złożonego jednostki

## <a name="fluent-api-alternative-to-attributes"></a>Zamiast interfejsu API Fluent atrybutów

`OnModelCreating` Metody w poprzednim kod używa *interfejsu API fluent* na konfigurowanie zachowania EF Core. Interfejs API jest nazywany "fluent", ponieważ jest często używany przez instalowanie szereg wywołania metody w jednej instrukcji. [Następującego kodu](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) przykładem interfejsu API fluent:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

W tym samouczku interfejsu API fluent jest używany tylko w przypadku mapowania bazy danych, które nie może zostać wykonane z atrybutami. Jednak interfejsu API fluent można określić większość formatowania, sprawdzanie poprawności i reguły mapowania, które można wykonać za pomocą atrybutów.

Niektóre atrybuty, takie jak `MinimumLength` nie można zastosować z interfejsu API fluent. `MinimumLength` nie powoduje zmiany schematu, ma zastosowanie tylko reguły weryfikacji minimalnej długości.

Niektórzy deweloperzy wolą Użyj interfejsu API fluent wyłącznie tak, aby ich zachowanie ich klasami jednostki "Wyczyść". Atrybuty i wygodnego interfejsu API można łączyć. Istnieją pewne konfiguracje, które jest możliwe tylko z interfejsu API fluent (Określanie złożonego klucza podstawowego). Istnieją pewne konfiguracje, które można wykonać tylko z atrybutami (`MinimumLength`). Zalecana praktyka dla przy użyciu fluent API lub atrybuty:

* Wybierz jedną z tych dwóch metod.
* Użyć wybranej metody spójnie jak to możliwe.

Niektóre atrybuty używane w tym samouczku są używane do:

* Sprawdzanie poprawności tylko (na przykład `MinimumLength`).
* Tylko konfiguracja Core EF (na przykład `HasKey`).
* Sprawdzanie poprawności i podstawowe EF konfiguracji (na przykład `[StringLength(50)]`).

Aby uzyskać więcej informacji dotyczących atrybutów w porównaniu z interfejsu API fluent, zobacz [metody konfiguracji](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Jednostki Diagram przedstawiający relacje

Na poniższej ilustracji przedstawiono diagram, który EF zasilania narzędzia tworzenia dla modelu ukończone szkoły.

![Diagram jednostek](complex-data-model/_static/diagram.png)

Na powyższym diagramie przedstawiono:

* Kilka relacji jeden do wielu wierszy (od 1 do \*).
* Linię relacji jeden do zero lub jeden (1 do od 0 do 1) między `Instructor` i `OfficeAssignment` jednostek.
* Linię relacji zero lub jeden do wielu (od 0 do 1 do *) między `Instructor` i `Department` jednostek.

## <a name="seed-the-db-with-test-data"></a>Inicjatora bazy danych z danych testowych

Zaktualizuj kod w *Data/DbInitializer.cs*:

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

Poprzedni kod zawiera dane dla nowych jednostek. Większość ten kod tworzy nowe obiekty jednostki i ładuje przykładowych danych. Dane przykładowe są używane do testowania. Poprzedni kod tworzy następujące relacje wiele do wielu:

* `Enrollments`
* `CourseAssignment`

Uwaga: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) będzie obsługiwać [wstępne wypełnianie danych](https://github.com/aspnet/EntityFrameworkCore/issues/629).

## <a name="add-a-migration"></a>Dodaj migracji

Skompiluj projekt. Otwórz okno polecenia w folderze projektu i wprowadź następujące polecenie:

```console
dotnet ef migrations add ComplexDataModel
```

Poprzednie polecenie wyświetla ostrzeżenie o możliwości utraty danych.

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Jeśli `database update` polecenie jest uruchamiane, jest generowany następujący błąd:

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

Podczas migracji są uruchamiane z istniejącymi danymi, może to być ograniczenia klucza Obcego, które nie są spełnione przy użyciu istniejących danych. W tym samouczku tworzona jest nowej bazy danych, więc nie żadne naruszenia ograniczenia klucza Obcego. Zobacz [rozwiązywania ograniczeń klucza obcego z danymi starszych](#fk) instrukcje dotyczące sposobu rozwiązania naruszenia klucza Obcego dla bieżącej bazy danych.

## <a name="change-the-connection-string-and-update-the-db"></a>Zmień parametry połączenia i zaktualizować bazę danych

Kod w zaktualizowanego `DbInitializer` dodaje dane nowych jednostek. Aby wymusić EF Core, aby utworzyć nowy pusty bazy danych:

* Zmień nazwę ciągu połączenia bazy danych w *appsettings.json* do ContosoUniversity3. Nowa nazwa musi być nazwą, który nie został użyty na komputerze.

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* Można również usunąć przy użyciu bazy danych:

  * **Eksplorator obiektów SQL Server** (SSOX).
  * `database drop` Polecenia interfejsu wiersza polecenia:

    ```console
    dotnet ef database drop
    ```

Uruchom `database update` w oknie wiersza polecenia:

```console
dotnet ef database update
```

Poprzednie polecenie uruchamia wszystkich migracji.

Uruchom aplikację. Uruchomiona aplikacja działa `DbInitializer.Initialize` metody. `DbInitializer.Initialize` Wypełnia nowej bazy danych.

Otwórz SSOX bazy danych:

* Rozwiń węzeł **tabel** węzła. Utworzony tabele są wyświetlane.
* Jeśli SSOX został poprzednio otwarty, kliknij przycisk **Odśwież** przycisku.

![Tabele w SSOX](complex-data-model/_static/ssox-tables.png)

Sprawdź **CourseAssignment** tabeli:

* Kliknij prawym przyciskiem myszy **CourseAssignment** tabeli i wybierz **danych widoku**.
* Sprawdź **CourseAssignment** tabela zawiera dane.

![Dane CourseAssignment w SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a>Ustalenie ograniczeń klucza obcego starszych danych

Ta sekcja jest opcjonalna.

Podczas migracji są uruchamiane z istniejącymi danymi, może to być ograniczenia klucza Obcego, które nie są spełnione przy użyciu istniejących danych. Z danymi produkcyjnymi należy przedsięwziąć do migrowania istniejących danych. Ta sekcja zawiera przykład ustalania narusza ograniczenie klucza Obcego. Nie wprowadzić te zmiany kodu bez kopii zapasowej. Nie należy wprowadzić te zmiany kodu, gdy wypełnione poprzedniej sekcji i aktualizacji bazy danych.

*{Timestamp}_ComplexDataModel.cs* plik zawiera następujący kod:

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

Poprzedni kod dodaje niedopuszczającą `DepartmentID` klucza Obcego do `Course` tabeli. Bazy danych z poprzedniej samouczek zawiera wiersze w `Course`, więc nie można zaktualizować tabeli przez migracje.

Aby `ComplexDataModel` pracy migracji z istniejącymi danymi:

* Zmień kod, aby nadać nowej kolumny (`DepartmentID`) wartość domyślną.
* Utwórz fałszywych dział o nazwie "Temp" jako domyślnego działu.

### <a name="fix-the-foreign-key-constraints"></a>Usuń ograniczeń klucza obcego

Aktualizacja `ComplexDataModel` klasy `Up` metody:

* Otwórz *{timestamp}_ComplexDataModel.cs* pliku.
* Komentarz wiersz kodu, który dodaje `DepartmentID` kolumnę `Course` tabeli.

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

Dodaj następujący wyróżniony kod. Nowy kod przechodzi po `.CreateTable( name: "Department"` bloku: [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

Z poprzednim zmiany, istniejące `Course` wiersze zostaną powiązane do działu "Temp" po `ComplexDataModel` `Up` uruchamia metody.

Czy aplikacji produkcyjnej:

* Zawiera kod lub skryptów służących do dodawania `Department` wierszy i powiązanych `Course` wierszy do nowego `Department` wierszy.
* Używaj dział "Temp" lub wartość domyślną dla `Course.DepartmentID`.

Następny samouczek obejmuje dane dotyczące.

> [!div class="step-by-step"]
> [Poprzednie](xref:data/ef-rp/migrations)
> [dalej](xref:data/ef-rp/read-related-data)
