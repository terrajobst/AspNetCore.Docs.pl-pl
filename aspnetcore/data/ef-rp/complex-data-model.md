---
title: Razor Pages EF Core w ASP.NET Core — model danych-5 z 8
author: tdykstra
description: W tym samouczku Dodaj więcej jednostek i relacje i Dostosuj model danych, określając formatowanie, walidację i reguły mapowania.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: ab29cf687c80551d275cae69f28b7576016bfff6
ms.sourcegitcommit: e6bd2bbe5683e9a7dbbc2f2eab644986e6dc8a87
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/03/2019
ms.locfileid: "70238126"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a>Razor Pages EF Core w ASP.NET Core — model danych-5 z 8

Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

Poprzednie samouczki pracowały z podstawowym modelem danych, który składa się z trzech jednostek. W tym samouczku:

* Dodano więcej jednostek i relacji.
* Model danych jest dostosowywany przez określenie formatowania, walidacji i reguł mapowania bazy danych.

Ukończony model danych przedstawiono na poniższej ilustracji:

![Diagram jednostek](complex-data-model/_static/diagram.png)

## <a name="the-student-entity"></a>Jednostki dla uczniów

![Jednostka ucznia](complex-data-model/_static/student-entity.png)

Zastąp kod w *modelach/student. cs* następującym kodem:

[!code-csharp[](intro/samples/cu30/Models/Student.cs)]

Poprzedni kod dodaje `FullName` Właściwość i dodaje następujące atrybuty do istniejących właściwości:

* `[DataType]`
* `[DisplayFormat]`
* `[StringLength]`
* `[Column]`
* `[Required]`
* `[Display]`

### <a name="the-fullname-calculated-property"></a>Właściwość obliczeniowa FullName

`FullName`jest właściwością obliczaną, która zwraca wartość utworzoną przez połączenie dwóch innych właściwości. `FullName`nie można ustawić, dlatego ma tylko metodę dostępu get. Nie `FullName` utworzono żadnej kolumny w bazie danych.

### <a name="the-datatype-attribute"></a>Atrybut DataType

```csharp
[DataType(DataType.Date)]
```

W przypadku dat rejestracji uczniów wszystkie strony wyświetlają teraz godzinę i datę, chociaż tylko data jest ważna. Używając atrybutów adnotacji danych, można wprowadzić jedną zmianę kodu, która naprawi format wyświetlania na każdej stronie, która wyświetla dane. 

Atrybut [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) określa typ danych, który jest bardziej szczegółowy niż typ wewnętrzny bazy danych. W tym przypadku należy wyświetlić tylko datę, a nie datę i godzinę. [Wyliczenie DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) zawiera wiele typów danych, takich jak data, godzina, numer telefonu, waluta, EmailAddress itp. Ten `DataType` atrybut może również umożliwić aplikacji automatyczne udostępnianie funkcji specyficznych dla typu. Przykład:

* Łącze jest tworzone automatycznie dla `DataType.EmailAddress`. `mailto:`
* Selektor daty jest dostępny `DataType.Date` w większości przeglądarek.

Ten `DataType` atrybut emituje atrybuty HTML `data-` 5 (wymawiane kreski danych). `DataType` Atrybuty nie zapewniają walidacji.

### <a name="the-displayformat-attribute"></a>Atrybut DisplayFormat

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`DataType.Date`nie określa formatu wyświetlanej daty. Domyślnie pole Date jest wyświetlane zgodnie z domyślnymi formatami opartymi na [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support)serwera.

Ten `DisplayFormat` atrybut służy do jawnego określenia formatu daty. `ApplyFormatInEditMode` Ustawienie określa, że formatowanie ma być również stosowane do interfejsu użytkownika edytowania. Niektóre pola nie powinny `ApplyFormatInEditMode`być używane. Na przykład symbol waluty zazwyczaj nie powinien być wyświetlany w polu tekstowym Edycja.

Ten `DisplayFormat` atrybut może być używany przez siebie. Zwykle dobrym pomysłem jest użycie `DataType` atrybutu `DisplayFormat` z atrybutem. Ten `DataType` atrybut przekazuje semantykę danych w przeciwieństwie do sposobu renderowania na ekranie. Ten `DataType` atrybut zapewnia następujące korzyści, które nie są dostępne w `DisplayFormat`programie:

* Przeglądarka może włączyć funkcje HTML5. Na przykład Pokaż kontrolkę kalendarz, odpowiedni dla ustawień regionalnych symbol waluty, linki poczty e-mail i sprawdzanie poprawności danych po stronie klienta.
* Domyślnie przeglądarka renderuje dane przy użyciu poprawnego formatu na podstawie ustawień regionalnych.

Aby uzyskać więcej informacji, zobacz [ \<dokumentację pomocnika tagów > danych wejściowych](xref:mvc/views/working-with-forms#the-input-tag-helper).

### <a name="the-stringlength-attribute"></a>Atrybut StringLength

```csharp
[StringLength(50, ErrorMessage = "First name cannot be longer than 50 characters.")]
```

Można określić reguły walidacji danych i komunikaty o błędach walidacji z atrybutami. Atrybut [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) określa minimalną i maksymalną długość znaków, które są dozwolone w polu danych. Kod pokazuje limity nazw do nie więcej niż 50 znaków. Przykład określający, że minimalna długość ciągu jest pokazywana w [dalszej](#the-required-attribute)części.

Ten `StringLength` atrybut zapewnia również weryfikację po stronie klienta i serwera. Wartość minimalna nie ma wpływu na schemat bazy danych.

Ten `StringLength` atrybut nie uniemożliwia użytkownikowi wprowadzania białych znaków w nazwie. Atrybut [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) może służyć do stosowania ograniczeń do danych wejściowych. Na przykład poniższy kod wymaga, aby pierwszy znak był wielką literą, a pozostałe znaki są alfabetyczne:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W **Eksplorator obiektów SQL Server** (SSOX) Otwórz projektanta tabeli uczniów, klikając dwukrotnie tabelę **uczniów** .

![Tabela studentów w SSOX przed migracją](complex-data-model/_static/ssox-before-migration.png)

Na powyższym obrazie przedstawiono schemat `Student` tabeli. Pola nazw mają typ `nvarchar(MAX)`. Gdy migracja zostanie utworzona i zastosowana w dalszej części tego samouczka, nazwy `nvarchar(50)` pól staną się wynikiem atrybutów długości ciągu.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

W narzędziu SQLite Sprawdź definicje `Student` kolumn w tabeli. Pola nazw mają typ `Text`. Zwróć uwagę, że jest wywoływana `FirstMidName`nazwa pola imię. W następnej sekcji zmienisz nazwę tej kolumny na `FirstName`.

---

### <a name="the-column-attribute"></a>Atrybut Column

```csharp
[Column("FirstName")]
public string FirstMidName { get; set; }
```

Atrybuty mogą kontrolować sposób mapowania klas i właściwości do bazy danych. `Student` W modelu `Column` atrybut jest używany do `FirstMidName` mapowania nazwy właściwości na "FirstName" w bazie danych.

Po utworzeniu bazy danych nazwy właściwości w modelu są używane dla nazw kolumn (z wyjątkiem sytuacji, `Column` gdy atrybut jest używany). `Student` Model używa`FirstMidName` dla pola pierwszej nazwy, ponieważ pole może zawierać również nazwę środkową.

Atrybut w modelu danych jest mapowany`Student` do kolumnytabeli.`FirstName` `Student.FirstMidName` `[Column]` Dodanie `Column` atrybutu zmienia model z `SchoolContext`kopii zapasowej. Model, który wykonuje `SchoolContext` kopię zapasową, nie jest już zgodny z bazą danych. Niezgodność zostanie rozwiązany przez dodanie migracji w dalszej części tego samouczka.

### <a name="the-required-attribute"></a>Wymagany atrybut

```csharp
[Required]
```

`Required` Atrybut powoduje, że właściwości nazwy są wymagane. Ten `Required` atrybut nie jest wymagany dla typów niedopuszczających wartości null, takich jak typy wartości `DateTime`( `int`na przykład `double`,, i). Typy, które nie mogą mieć wartości null, są automatycznie traktowane jako pola wymagane.

Atrybut musi być używany z `MinimumLength` , `MinimumLength` aby można było wymusić. `Required`

```csharp
[Display(Name = "Last Name")]
[Required]
[StringLength(50, MinimumLength=2)]
public string LastName { get; set; }
```

`MinimumLength`i `Required` Zezwalaj na odstępy, aby spełnić kryteria weryfikacji. `RegularExpression` Użyj atrybutu w celu zapewnienia pełnej kontroli nad ciągiem.

### <a name="the-display-attribute"></a>Atrybut wyświetlania

```csharp
[Display(Name = "Last Name")]
```

Ten `Display` atrybut określa, że podpis pól tekstowych powinien mieć wartość "imię i nazwisko", "nazwisko", "imię i nazwisko" oraz "Data rejestracji". Domyślne podpisy nie zawierają spacji dzielących wyrazy, na przykład "LastName".

### <a name="create-a-migration"></a>Tworzenie migracji

Uruchom aplikację i przejdź do strony uczniów. Zgłaszany jest wyjątek. Ten `[Column]` atrybut powoduje, że Dr powinien znaleźć kolumnę o nazwie `FirstName`, ale nazwa kolumny w bazie danych jest nadal `FirstMidName`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Komunikat o błędzie jest podobny do poniższego przykładu:

```
SqlException: Invalid column name 'FirstName'.
```

* W obszarze PMC wprowadź następujące polecenia, aby utworzyć nową migrację i zaktualizować bazę danych:

  ```powershell
  Add-Migration ColumnFirstName
  Update-Database
  ```

  Pierwsze z tych poleceń generuje następujący komunikat ostrzegawczy:

  ```text
  An operation was scaffolded that may result in the loss of data.
  Please review the migration for accuracy.
  ```

  Ostrzeżenie jest generowane, ponieważ pola nazw są teraz ograniczone do 50 znaków. Jeśli nazwa w bazie danych ma więcej niż 50 znaków, zostanie utracony od 51 do ostatniego znaku.

* Otwórz tabelę uczniów w SSOX:

  ![Tabela studentów w SSOX po migracji](complex-data-model/_static/ssox-after-migration.png)

  Przed zainstalowaniem migracji kolumny nazw były typu [nvarchar (max)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql). Kolumny nazw są teraz `nvarchar(50)`. Nazwa kolumny została zmieniona z `FirstMidName` na. `FirstName`

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Komunikat o błędzie jest podobny do poniższego przykładu:

```
SqliteException: SQLite Error 1: 'no such column: s.FirstName'.
```

* Otwórz okno polecenia w folderze projektu. Wprowadź następujące polecenia, aby utworzyć nową migrację i zaktualizować bazę danych:

  ```console
  dotnet ef migrations add ColumnFirstName
  dotnet ef database update
  ```

  Polecenie aktualizacji bazy danych wyświetla błąd podobny do następującego przykładu:

  ```text
  SQLite does not support this migration operation ('AlterColumnOperation'). For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
  ```

W tym samouczku sposób, w jaki można to zrobić, jest usunięcie i ponowne utworzenie początkowej migracji. Aby uzyskać więcej informacji, zobacz uwagi dotyczące ostrzeżeń oprogramowania SQLite w górnej części [samouczka migracji](xref:data/ef-rp/migrations).

* Usuń folder *migracji* .
* Uruchom następujące polecenia, aby usunąć bazę danych, utworzyć nową migrację początkową i zastosować migrację:

  ```console
  dotnet ef database drop --force
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

* Sprawdź tabelę uczniów w narzędziu SQLite. Kolumna, która była FirstMidName, jest teraz FirstName.

---

* Uruchom aplikację i przejdź do strony uczniów.
* Należy zauważyć, że czasy nie są wprowadzane ani wyświetlane wraz z datami.
* Wybierz pozycję **Utwórz nową**, a następnie spróbuj wprowadzić nazwę dłuższą niż 50 znaków.

> [!Note]
> W poniższych sekcjach Kompilowanie aplikacji na niektórych etapach powoduje wygenerowanie błędów kompilatora. Instrukcje określają, kiedy należy skompilować aplikację.

## <a name="the-instructor-entity"></a>Jednostka instruktora

![Jednostka instruktora](complex-data-model/_static/instructor-entity.png)

Utwórz *modele/instruktor. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Models/Instructor.cs)]

Wiele atrybutów może znajdować się w jednym wierszu. `HireDate` Atrybuty mogą być zapisywane w następujący sposób:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="navigation-properties"></a>Właściwości nawigacji

Właściwości `CourseAssignments` i`OfficeAssignment` są właściwościami nawigacji.

Instruktor może nauczyć dowolną liczbę kursów, więc `CourseAssignments` jest zdefiniowany jako kolekcja.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Instruktor może mieć co najwyżej jednego biura, więc `OfficeAssignment` Właściwość zawiera jedną `OfficeAssignment` jednostkę. `OfficeAssignment`ma wartość null, jeśli nie przypisano pakietu Office.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="the-officeassignment-entity"></a>Jednostka OfficeAssignment

![Jednostka OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

Utwórz *modele/OfficeAssignment. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Atrybut klucza

Ten `[Key]` atrybut służy do identyfikowania właściwości jako klucza podstawowego (PK), gdy nazwa właściwości ma wartość inną niż classnameID lub ID.

Istnieje relacja jeden do zera między `Instructor` jednostkami i. `OfficeAssignment` Przypisanie pakietu Office istnieje tylko w odniesieniu do instruktora, do którego jest przypisane. Klucz podstawowy jest również jego kluczem obcym (obcy) `Instructor` do jednostki. `OfficeAssignment`

EF Core nie może automatycznie `InstructorID` rozpoznać jako `OfficeAssignment` klucza podstawowego, `InstructorID` ponieważ nie jest zgodna z konwencją nazewnictwa ID lub classnameID. W związku z tym `InstructorID` atrybutjestużywanydoidentyfikacjijakokluczpodstawowy:`Key`

```csharp
[Key]
public int InstructorID { get; set; }
```

Domyślnie EF Core traktuje klucz jako wygenerowane poza bazą danych, ponieważ kolumna służy do identyfikowania relacji.

### <a name="the-instructor-navigation-property"></a>Właściwość nawigacji instruktora

Właściwość `Instructor.OfficeAssignment` nawigacji może mieć wartość null, ponieważ nie może `OfficeAssignment` istnieć wiersz danego instruktora. Instruktor może nie mieć przypisania pakietu Office.

Właściwość nawigacji zawsze będzie mieć jednostkę instruktora, ponieważ typ klucza `InstructorID` obcego to `int`, typ wartości niedopuszczający wartości null. `OfficeAssignment.Instructor` Przypisanie pakietu Office nie może istnieć bez instruktora.

Gdy jednostka ma powiązaną `OfficeAssignment` jednostkę, każda jednostka ma odwołanie do drugiej z nich we właściwości nawigacji. `Instructor`

## <a name="the-course-entity"></a>Jednostka kursu

![Jednostka kursu](complex-data-model/_static/course-entity.png)

Aktualizuj *modele/kurs. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Models/Course.cs?highlight=2,10,13,16,19,21,23)]

Jednostka ma właściwość `DepartmentID`klucza obcego (obcy). `Course` `DepartmentID`wskazuje powiązaną `Department` jednostkę. `Course` Jednostka ma właściwość nawigacji. `Department`

EF Core nie wymaga właściwości klucza obcego dla modelu danych, gdy model ma właściwość nawigacji dla powiązanej jednostki. EF Core automatycznie tworzy FKs w bazie danych wszędzie tam, gdzie są one zbędne. EF Core tworzy [Właściwości cienia](/ef/core/modeling/shadow-properties) dla automatycznie utworzonych FKs. Jednak jawne dołączenie klucza obcego w modelu danych może spowodować uproszczenie i wydajniejsze aktualizacje. Rozważmy na przykład model, w którym Właściwość `DepartmentID` FK *nie* jest uwzględniona. Gdy jednostka kursu jest pobierana do edycji:

* Właściwość `Department` ma wartość null, jeśli nie jest jawnie załadowana.
* Aby zaktualizować jednostkę kursu, `Department` należy najpierw pobrać jednostkę.

Gdy właściwość `DepartmentID` FK jest uwzględniona w modelu danych, nie trzeba `Department` pobierać jednostki przed aktualizacją.

### <a name="the-databasegenerated-attribute"></a>Atrybut DatabaseGenerated

Ten `[DatabaseGenerated(DatabaseGeneratedOption.None)]` atrybut określa, że klucz podstawowy jest dostarczany przez aplikację, a nie generowany przez bazę danych.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Domyślnie EF Core zakłada, że wartości klucza PK są generowane przez bazę danych. Wygenerowana baza danych jest ogólnie najlepszym rozwiązaniem. W `Course` przypadku jednostek użytkownik określa klucz podstawowy. Na przykład numer kursu, taki jak seria 1000 dla działu Math, serii 2000 dla działu angielskiego.

Ten `DatabaseGenerated` atrybut może być również używany do generowania wartości domyślnych. Na przykład baza danych może automatycznie wygenerować pole daty, aby zarejestrować datę utworzenia lub zaktualizowania wiersza. Aby uzyskać więcej informacji, zobacz [wygenerowane właściwości](/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Właściwości klucza obcego i nawigacji

Właściwości klucza obcego (FK) i właściwości nawigacji w `Course` jednostce odzwierciedlają następujące relacje:

Kurs jest przypisywany do jednego działu, więc istnieje `DepartmentID` obcy `Department` i właściwość nawigacji.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Kurs może zawierać dowolną liczbę studentów, więc `Enrollments` właściwość nawigacji jest kolekcją:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Kurs może być nauczany przez wiele instruktorów, więc `CourseAssignments` właściwość nawigacji jest kolekcją:

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment`wyjaśniono [później](#many-to-many-relationships).

## <a name="the-department-entity"></a>Jednostka działu

![Jednostka działu](complex-data-model/_static/department-entity.png)

Utwórz *modele/dział. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Models/Department1.cs)]

### <a name="the-column-attribute"></a>Atrybut Column

`Column` Wcześniej atrybut został użyty do zmiany mapowania nazw kolumn. W kodzie dla `Department` jednostki `Column` atrybut jest używany do zmiany mapowania typu danych SQL. `Budget` Kolumna jest definiowana przy użyciu SQL Server typie pieniędzy w bazie danych:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Mapowanie kolumn zazwyczaj nie jest wymagane. EF Core wybiera odpowiedni SQL Server typ danych na podstawie typu CLR dla właściwości. Typ CLR `decimal` jest mapowany na typ SQL Server `decimal` . `Budget`jest dla waluty, a typ danych walutowych jest bardziej odpowiedni dla waluty.

### <a name="foreign-key-and-navigation-properties"></a>Właściwości klucza obcego i nawigacji

Właściwości FK i nawigacji odzwierciedlają następujące relacje:

* Dział może lub nie ma uprawnienia administratora.
* Administrator jest zawsze instruktorem. W związku z tym `Instructor` Właściwośćjestdołączanajakoobcydojednostki.`InstructorID`

Właściwość nawigacji ma nazwę `Administrator` , ale `Instructor` zawiera jednostkę:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Znak zapytania (?) w powyższym kodzie określa właściwość dopuszcza wartość null.

Dział może mieć wiele kursów, więc istnieje właściwość nawigacji kursów:

```csharp
public ICollection<Course> Courses { get; set; }
```

Zgodnie z Konwencją EF Core włącza kaskadowe usuwanie dla FKs niedopuszczających wartości null i dla relacji wiele-do-wielu. To domyślne zachowanie może spowodować cykliczne reguły usuwania kaskadowego. Cykliczne reguły usuwania kaskadowego powodują wyjątek podczas dodawania migracji.

Na przykład jeśli `Department.InstructorID` właściwość została zdefiniowana jako niedopuszczający wartości null, EF Core skonfigurować regułę usuwania kaskadowego. W takim przypadku dział zostanie usunięty po usunięciu instruktora przypisanego jako administrator. W tym scenariuszu reguła ograniczania byłaby bardziej zrozumiała. Poniższy [interfejs API Fluent](#fluent-api-alternative-to-attributes) ustawi regułę ograniczenia i wyłącza usuwanie kaskadowe.

  ```csharp
  modelBuilder.Entity<Department>()
     .HasOne(d => d.Administrator)
     .WithMany()
     .OnDelete(DeleteBehavior.Restrict)
  ```

## <a name="the-enrollment-entity"></a>Jednostki rejestracji

Rekord rejestracji jest wykonywany przez jednego ucznia.

![Jednostka rejestracji](complex-data-model/_static/enrollment-entity.png)

Aktualizuj *modele/rejestrację. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Models/Enrollment.cs?highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Właściwości klucza obcego i nawigacji

Właściwości klucza obcego i właściwości nawigacji odzwierciedlają następujące relacje:

Rekord rejestracji jest przeznaczony dla jednego kursu, dlatego istnieje `CourseID` Właściwość FK `Course` i właściwość nawigacji:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Rekord rejestracji jest przeznaczony dla jednego ucznia, dlatego istnieje `StudentID` właściwość klucza obcego `Student` i właściwość nawigacji:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relacje wiele do wielu

Istnieje relacja wiele-do-wielu między `Student` obiektami i. `Course` Jednostka działa jako tabela sprzężenia wiele-do-wielu *z ładunkiem* w bazie danych. `Enrollment` "Z ładunkiem" oznacza, że `Enrollment` tabela zawiera dodatkowe dane oprócz FKs dla sprzężonych tabel (w tym przypadku klucz podstawowy i `Grade`).

Na poniższej ilustracji pokazano, jak wyglądają te relacje w diagramie jednostek. (Ten diagram został wygenerowany przy użyciu [narzędzi EF PowerShell](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) dla programu EF 6. x. Tworzenie diagramu nie jest częścią samouczka.

![Student-kurs wiele do wielu relacji](complex-data-model/_static/student-course.png)

Każda linia relacji ma 1 na jednym końcu i gwiazdkę (*) na drugim, wskazując relację jeden do wielu.

Jeśli tabela nie zawiera informacji o klasie, musi zawierać tylko dwa FKs (`CourseID` i `StudentID`). `Enrollment` Tabela sprzężenia wiele-do-wielu bez ładunku jest czasami nazywana tabelą sprzężenia czystego (PJT).

Jednostki `Instructor` i`Course` mają relację wiele-do-wielu przy użyciu czystej tabeli sprzężenia.

Uwaga: Dr 6. x obsługuje niejawne tabele sprzężeń dla relacji wiele-do-wielu, ale EF Core nie. Aby uzyskać więcej informacji, zobacz [relacje wiele-do-wielu w EF Core 2,0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).

## <a name="the-courseassignment-entity"></a>Jednostka CourseAssignment

![Jednostka CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

Utwórz *modele/CourseAssignment. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Models/CourseAssignment.cs)]

Relacja "instruktora" do wielu kursów wymaga tabeli sprzężenia, a jednostka dla tej tabeli sprzężenia jest CourseAssignment.

![M:M instruktora do kursu](complex-data-model/_static/courseassignment.png)

Nazwa jednostki `EntityName1EntityName2`sprzężenia jest wspólna. Na przykład tabela sprzężenia "instruktora do kursu" używa tego wzorca `CourseInstructor`. Zalecamy jednak użycie nazwy opisującej relację.

Modele danych rozpoczynają się od siebie i rosną. Tabele sprzężenia bez ładunku (PJTs) często są rozwijane w celu uwzględnienia ładunku. Rozpoczynając od nazwy obiektu opisowego, nie trzeba zmieniać nazwy po zmianie tabeli sprzężeń. Najlepiej, aby jednostka sprzężenia miała własną nazwę naturalną (prawdopodobnie jednowyrazową) w domenie biznesowej. Na przykład książki i klienci mogą być połączone z jednostką sprzężenia o nazwie ratings. Dla instruktora "wiele do wielu", `CourseAssignment` `CourseInstructor`preferowana jest wartość.

### <a name="composite-key"></a>Klucz złożony

Dwa FKs w `CourseAssignment` (`InstructorID` `CourseID` i`CourseAssignment` ) jednoznacznie identyfikują każdy wiersz tabeli. `CourseAssignment`nie wymaga dedykowanego klucza podstawowego. Właściwości `InstructorID` i`CourseID` działają jako złożony klucz podstawowy. Jedynym sposobem określenia złożonego PKs do EF Core jest *interfejs API Fluent*. W następnej sekcji przedstawiono sposób konfigurowania złożonego klucza podstawowego.

Klucz złożony gwarantuje, że:

* W jednym kursie są dozwolone wiele wierszy.
* Dla jednego instruktora są dozwolone wiele wierszy.
* Wiele wierszy nie jest dozwolonych dla tego samego instruktora i kursu.

Jednostka `Enrollment` Join definiuje własny klucz podstawowy, więc możliwe jest duplikowanie tego sortowania. Aby uniknąć takich duplikatów:

* Dodaj unikatowy indeks do pól klucza obcego lub
* Skonfiguruj `Enrollment` przy użyciu podstawowego klucza złożonego podobnego `CourseAssignment`do. Aby uzyskać więcej informacji, zobacz [indeksy](/ef/core/modeling/indexes).

## <a name="update-the-database-context"></a>Aktualizowanie kontekstu bazy danych

Zaktualizuj *dane/SchoolContext. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Data/SchoolContext.cs?highlight=15-18,25-31)]

Poprzedni kod dodaje nowe jednostki i konfiguruje `CourseAssignment` złożonego klucza podstawowego jednostki.

## <a name="fluent-api-alternative-to-attributes"></a>Alternatywny interfejs API Fluent dla atrybutów

Metoda w poprzednim kodzie używa *interfejsu API Fluent* , aby skonfigurować zachowanie EF Core. `OnModelCreating` Interfejs API jest nazywany "Fluent", ponieważ jest często używany przez ciąg serii wywołań metod w jednej instrukcji. [Poniższy kod](/ef/core/modeling/#use-fluent-api-to-configure-a-model) jest przykładem interfejsu API Fluent:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

W tym samouczku interfejs API Fluent jest używany tylko na potrzeby mapowania bazy danych, której nie można wykonać przy użyciu atrybutów. Jednak interfejs API Fluent może określić większość reguł formatowania, walidacji i mapowania, które mogą być wykonywane przy użyciu atrybutów.

Niektórych atrybutów, takich `MinimumLength` jak nie można zastosować w interfejsie API Fluent. `MinimumLength`nie zmienia schematu, stosuje tylko regułę walidacji o minimalnej długości.

Niektórzy deweloperzy wolą korzystać z interfejsu API Fluent wyłącznie w taki sposób, aby mogli utrzymać czyste klasy jednostek. Atrybuty i interfejs API Fluent mogą być mieszane. Istnieją pewne konfiguracje, które można wykonać tylko przy użyciu interfejsu API Fluent (określenie złożonego klucza podstawowego). Istnieją pewne konfiguracje, które można wykonać tylko z atrybutami (`MinimumLength`). Zalecane rozwiązanie dotyczące korzystania z interfejsu API Fluent lub atrybutów:

* Wybierz jedną z tych dwóch metod.
* W miarę możliwości należy stosować wybrane podejście.

Niektóre atrybuty użyte w tym samouczku są używane w programie:

* Tylko Walidacja (na przykład `MinimumLength`).
* Tylko konfiguracja EF Core (na przykład `HasKey`).
* Sprawdzanie poprawności i konfiguracja EF Core (na przykład `[StringLength(50)]`).

Aby uzyskać więcej informacji na temat atrybutów a interfejs API Fluent, zobacz [metody konfiguracji](/ef/core/modeling/).

## <a name="entity-diagram"></a>Diagram jednostek

Na poniższej ilustracji przedstawiono diagram, który jest tworzony przez narzędzia Dr PowerShell dla gotowego modelu szkoły.

![Diagram jednostek](complex-data-model/_static/diagram.png)

Na powyższym diagramie przedstawiono:

* Kilka linii relacji jeden-do-wielu (od 1 \*do).
* Linia relacji "jeden do zera" (od 1 do 0.. 1) między `Instructor` jednostkami i. `OfficeAssignment`
* Linia relacji zero-lub-jeden-do-wielu (0.. 1 do *) między `Instructor` obiektami i. `Department`

## <a name="seed-the-database"></a>Inicjowanie bazy danych

Zaktualizuj kod w *danych/Dbinitializeer. cs*:

[!code-csharp[](intro/samples/cu30/Data/DbInitializer.cs)]

Poprzedni kod zawiera dane inicjatora dla nowych jednostek. Większość tego kodu tworzy nowe obiekty Entity i ładuje przykładowe dane. Przykładowe dane służą do testowania. Zobacz `Enrollments` i `CourseAssignments` , aby zapoznać się z przykładami tabel sprzężenia wiele-do-wielu.

## <a name="add-a-migration"></a>Dodawanie migracji

Skompiluj projekt.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W obszarze PMC Uruchom następujące polecenie.

```powershell
Add-Migration ComplexDataModel
```

Poprzednie polecenie wyświetla ostrzeżenie o możliwej utracie danych.

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
To undo this action, use 'ef migrations remove'
```

`database update` Jeśli polecenie zostanie uruchomione, zostanie utworzony następujący błąd:

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

W następnej sekcji opisano, co należy zrobić, aby uzyskać informacje o tym błędzie.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

W przypadku dodania migracji i uruchomienia `database update` polecenia zostanie utworzony następujący błąd:

```text
SQLite does not support this migration operation ('DropForeignKeyOperation').
For more information, see http://go.microsoft.com/fwlink/?LinkId=723262.
```

W następnej sekcji zobaczysz, jak uniknąć tego błędu.

---

## <a name="apply-the-migration-or-drop-and-re-create"></a>Zastosuj migrację lub Porzuć i Utwórz ponownie

Teraz, gdy masz już istniejącą bazę danych, musisz się zastanowić, jak zastosować do niej zmiany. Ten samouczek przedstawia dwie alternatywy:

* [Porzuć i ponownie utwórz bazę danych](#drop). Wybierz tę sekcję, jeśli używasz oprogramowania SQLite.
* [Zastosuj migrację do istniejącej bazy danych](#applyexisting). Instrukcje w tej sekcji działają tylko w przypadku SQL Server, a **nie dla oprogramowania SQLite**. 

Dowolny wybór działa dla SQL Server. Chociaż metoda Apply-Migration jest bardziej złożona i czasochłonna, jest to preferowane podejście do rzeczywistych środowisk produkcyjnych. 

<a name="drop"></a>

## <a name="drop-and-re-create-the-database"></a>Porzuć i ponownie utwórz bazę danych

[Pomiń tę sekcję](#apply-the-migration) , jeśli używasz SQL Server i chcesz wykonać podejście Apply-Migration w poniższej sekcji.

Aby wymusić EF Core tworzenia nowej bazy danych, Porzuć i zaktualizuj bazę danych:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W **konsoli Menedżera pakietów** (PMC) Uruchom następujące polecenie:

  ```powershell
  Drop-Database
  ```

* Usuń folder *migrations* , a następnie uruchom następujące polecenie:

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Otwórz okno polecenia i przejdź do folderu projektu. Folder projektu zawiera plik *ContosoUniversity. csproj* .

* Uruchom następujące polecenie:

  ```console
  dotnet ef database drop --force
  ```

* Usuń folder *migrations* , a następnie uruchom następujące polecenie:

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

---

Uruchom aplikację. Uruchomienie aplikacji uruchamia `DbInitializer.Initialize` metodę. `DbInitializer.Initialize` Wypełnia nową bazę danych.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Otwórz bazę danych w programie SSOX:

* Jeśli SSOX został wcześniej otwarty, kliknij przycisk **Odśwież** .
* Rozwiń **tabel** węzła. Zostaną wyświetlone utworzone tabele.

  ![Tabele w SSOX](complex-data-model/_static/ssox-tables.png)

* Zapoznaj się z tabelą **CourseAssignment** :

  * Kliknij prawym przyciskiem myszy tabelę **CourseAssignment** , a następnie wybierz polecenie **Wyświetl dane**.
  * Sprawdź, czy tabela **CourseAssignment** zawiera dane.

  ![CourseAssignment dane w SSOX](complex-data-model/_static/ssox-ci-data.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Sprawdź bazę danych za pomocą narzędzia SQLite:

* Nowe tabele i kolumny.
* Dane są umieszczane w tabelach, na przykład w tabeli **CourseAssignment** .

---

<a name="applyexisting"></a>

## <a name="apply-the-migration"></a>Zastosuj migrację

Ta sekcja jest opcjonalna. Te kroki działają tylko dla SQL Server LocalDB i tylko wtedy, gdy pominięto poprzednią [i ponownie utworzysz sekcję Database](#drop) .

Po uruchomieniu migracji z istniejącymi danymi mogą istnieć ograniczenia klucza obcego, które nie są spełnione w przypadku istniejących danych. W przypadku danych produkcyjnych należy wykonać kroki, aby przeprowadzić migrację istniejących danych. Ta sekcja zawiera przykład rozwiązywania naruszeń ograniczenia klucza obcego. Nie należy wprowadzać tych zmian w kodzie bez tworzenia kopii zapasowej. Nie wprowadzaj tych zmian w kodzie, jeśli wykonano poprzednie [upuszczenie i ponownie utworzysz sekcję bazy danych](#drop) .

Plik *{timestamp} _ComplexDataModel. cs* zawiera następujący kod:

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_DepartmentID)]

Poprzedzający kod dodaje `DepartmentID` `Course` do tabeli obcy niedopuszczający wartości null. Baza danych z poprzedniego samouczka zawiera wiersze w `Course`, dlatego nie można zaktualizować tabeli za pomocą migracji.

Aby `ComplexDataModel` migracja była współdziałać z istniejącymi danymi:

* Zmień kod, aby nadać nowej kolumnie (`DepartmentID`) wartość domyślną.
* Utwórz fałszywy Departament o nazwie "Temp", który ma pełnić rolę działu domyślnego.

#### <a name="fix-the-foreign-key-constraints"></a>Popraw ograniczenia klucza obcego

W klasie `Up` migracji należy zaktualizować metodę: `ComplexDataModel`

* Otwórz plik *{timestamp} _ComplexDataModel. cs* .
* Dodaj komentarz `DepartmentID` `Course` do wiersza kodu, który dodaje kolumnę do tabeli.

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

Dodaj następujący wyróżniony kod. Nowy kod przechodzi po `.CreateTable( name: "Department"` bloku:

[! code-CSharp [] (wprowadzenie/przykłady/cu30snapshots/5 — złożone/migracje/ComplexDataModel. cs? Name = snippet_CreateDefaultValue & Podświetl = 23-31)]

Po wykonaniu powyższych zmian istniejące `Course` wiersze będą powiązane z działem "Temp" po uruchomieniu metody.`ComplexDataModel.Up`

Sposób obsługi pokazanej tutaj sytuacji jest uproszczony dla tego samouczka. Aplikacja produkcyjna:

* Dołącz kod lub skrypty, aby `Department` dodać wiersze i `Course` powiązane wiersze do nowych `Department` wierszy.
* Nie używaj działu "Temp" ani wartości domyślnej dla `Course.DepartmentID`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W **konsoli Menedżera pakietów** (PMC) Uruchom następujące polecenie:

  ```powershell
  Update-Database
  ```

`DbInitializer.Initialize` Ponieważ metoda została zaprojektowana tak, aby działała tylko z pustą bazą danych, użyj SSOX, aby usunąć wszystkie wiersze z tabel uczniów i kursów. (Usuwanie kaskadowe zajmie tabelę rejestracji).

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Jeśli używasz SQL Server LocalDB z Visual Studio Code, uruchom następujące polecenie:

  ```console
  dotnet ef database update
  ```

---

Uruchom aplikację. Uruchomienie aplikacji uruchamia `DbInitializer.Initialize` metodę. `DbInitializer.Initialize` Wypełnia nową bazę danych.

## <a name="next-steps"></a>Następne kroki

W następnych dwóch samouczkach pokazano, jak odczytywać i aktualizować powiązane dane.

> [!div class="step-by-step"]
> [Poprzedni](xref:data/ef-rp/migrations)
> samouczek w[następnym](xref:data/ef-rp/read-related-data) samouczku

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Poprzednie samouczki pracowały z podstawowym modelem danych, który składa się z trzech jednostek. W tym samouczku:

* Dodano więcej jednostek i relacji.
* Model danych jest dostosowywany przez określenie formatowania, walidacji i reguł mapowania bazy danych.

Klasy jednostek dla kompletnego modelu danych przedstawiono na poniższej ilustracji:

![Diagram jednostek](complex-data-model/_static/diagram.png)

Jeśli wystąpią problemy, których nie można rozwiązać, Pobierz [ukończoną](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)aplikację.

## <a name="customize-the-data-model-with-attributes"></a>Dostosowywanie modelu danych przy użyciu atrybutów

W tej sekcji model danych jest dostosowany przy użyciu atrybutów.

### <a name="the-datatype-attribute"></a>Atrybut DataType

Na stronach ucznia jest obecnie wyświetlana godzina daty rejestracji. Zwykle pola Date zawierają tylko datę, a nie czas.

Aktualizuj *modele/uczniów. cs* przy użyciu następującego wyróżnionego kodu:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

Atrybut [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) określa typ danych, który jest bardziej szczegółowy niż typ wewnętrzny bazy danych. W tym przypadku należy wyświetlić tylko datę, a nie datę i godzinę. [Wyliczenie DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) zawiera wiele typów danych, takich jak data, godzina, numer telefonu, waluta, EmailAddress itp. Ten `DataType` atrybut może również umożliwić aplikacji automatyczne udostępnianie funkcji specyficznych dla typu. Na przykład:

* Łącze jest tworzone automatycznie dla `DataType.EmailAddress`. `mailto:`
* Selektor daty jest dostępny `DataType.Date` w większości przeglądarek.

Ten `DataType` atrybut emituje kod HTML `data-` 5 (wymawiane kreski danych), które wykorzystują przeglądarki HTML 5. `DataType` Atrybuty nie zapewniają walidacji.

`DataType.Date`nie określa formatu wyświetlanej daty. Domyślnie pole Date jest wyświetlane zgodnie z domyślnymi formatami opartymi na [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support)serwera.

Ten `DisplayFormat` atrybut służy do jawnego określenia formatu daty:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode` Ustawienie określa, że formatowanie ma być również stosowane do interfejsu użytkownika edytowania. Niektóre pola nie powinny `ApplyFormatInEditMode`być używane. Na przykład symbol waluty zazwyczaj nie powinien być wyświetlany w polu tekstowym Edycja.

Ten `DisplayFormat` atrybut może być używany przez siebie. Zwykle dobrym pomysłem jest użycie `DataType` atrybutu `DisplayFormat` z atrybutem. Ten `DataType` atrybut przekazuje semantykę danych w przeciwieństwie do sposobu renderowania na ekranie. Ten `DataType` atrybut zapewnia następujące korzyści, które nie są dostępne w `DisplayFormat`programie:

* Przeglądarka może włączyć funkcje HTML5. Na przykład Pokaż kontrolkę kalendarz, odpowiedni dla ustawień regionalnych symbol waluty, linki poczty e-mail, sprawdzanie poprawności danych po stronie klienta itd.
* Domyślnie przeglądarka renderuje dane przy użyciu poprawnego formatu na podstawie ustawień regionalnych.

Aby uzyskać więcej informacji, zobacz [ \<dokumentację pomocnika tagów > danych wejściowych](xref:mvc/views/working-with-forms#the-input-tag-helper).

Uruchom aplikację. Przejdź do strony indeksu uczniów. Czasy nie są już wyświetlane. Każdy widok korzystający z `Student` modelu wyświetla datę bez czasu.

![Strona indeksu uczniów pokazująca daty bez czasu](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>Atrybut StringLength

Można określić reguły walidacji danych i komunikaty o błędach walidacji z atrybutami. Atrybut [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) określa minimalną i maksymalną długość znaków, które są dozwolone w polu danych. Ten `StringLength` atrybut zapewnia również weryfikację po stronie klienta i serwera. Wartość minimalna nie ma wpływu na schemat bazy danych.

`Student` Zaktualizuj model przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

Poprzedni kod ogranicza nazwy do nie więcej niż 50 znaków. Ten `StringLength` atrybut nie uniemożliwia użytkownikowi wprowadzania białych znaków w nazwie. Atrybut [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) służy do stosowania ograniczeń do danych wejściowych. Na przykład poniższy kod wymaga, aby pierwszy znak był wielką literą, a pozostałe znaki są alfabetyczne:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

Uruchom aplikację:

* Przejdź do strony uczniów.
* Wybierz pozycję **Utwórz nową**, a następnie wprowadź nazwę o długości większej niż 50 znaków.
* Wybierz pozycję **Utwórz**, a po stronie klienta zostanie wyświetlony komunikat o błędzie.

![Strona indeksu studentów przedstawiająca błędy długości ciągu](complex-data-model/_static/string-length-errors.png)

W **Eksplorator obiektów SQL Server** (SSOX) Otwórz projektanta tabeli uczniów, klikając dwukrotnie tabelę **uczniów** .

![Tabela studentów w SSOX przed migracją](complex-data-model/_static/ssox-before-migration.png)

Na powyższym obrazie przedstawiono schemat `Student` tabeli. Pola nazw mają typ `nvarchar(MAX)` , ponieważ migracja nie została uruchomiona w bazie danych. Gdy migracja zostanie uruchomiona w dalszej części tego samouczka, pola nazwy `nvarchar(50)`stają się.

### <a name="the-column-attribute"></a>Atrybut Column

Atrybuty mogą kontrolować sposób mapowania klas i właściwości do bazy danych. W tej sekcji `Column` atrybut jest używany do mapowania nazwy `FirstMidName` właściwości na "FirstName" w bazie danych.

Po utworzeniu bazy danych nazwy właściwości w modelu są używane dla nazw kolumn (z wyjątkiem sytuacji, `Column` gdy atrybut jest używany).

`Student` Model używa`FirstMidName` dla pola pierwszej nazwy, ponieważ pole może zawierać również nazwę środkową.

Zaktualizuj plik *student.cs* za pomocą następującego wyróżnionego kodu:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

Z poprzednią zmianą `Student.FirstMidName` w aplikacji jest mapowany `FirstName` do kolumny `Student` tabeli.

Dodanie `Column` atrybutu zmienia model z `SchoolContext`kopii zapasowej. Model, który wykonuje `SchoolContext` kopię zapasową, nie jest już zgodny z bazą danych. Jeśli aplikacja jest uruchamiana przed zastosowaniem migracji, generowany jest następujący wyjątek:

```SQL
SqlException: Invalid column name 'FirstName'.
```

Aby zaktualizować bazę danych:

* Skompiluj projekt.
* Otwórz okno polecenia w folderze projektu. Wprowadź następujące polecenia, aby utworzyć nową migrację i zaktualizować bazę danych:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

---

`migrations add ColumnFirstName` Polecenie generuje następujący komunikat ostrzegawczy:

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

Ostrzeżenie jest generowane, ponieważ pola nazw są teraz ograniczone do 50 znaków. Jeśli nazwa w bazie danych ma więcej niż 50 znaków, zostanie utracony od 51 do ostatniego znaku.

* Przetestuj aplikację.

Otwórz tabelę uczniów w SSOX:

![Tabela studentów w SSOX po migracji](complex-data-model/_static/ssox-after-migration.png)

Przed zainstalowaniem migracji kolumny nazw były typu [nvarchar (max)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql). Kolumny nazw są teraz `nvarchar(50)`. Nazwa kolumny została zmieniona z `FirstMidName` na. `FirstName`

> [!Note]
> W poniższej sekcji Kompilowanie aplikacji na niektórych etapach powoduje wygenerowanie błędów kompilatora. Instrukcje określają, kiedy należy skompilować aplikację.

## <a name="student-entity-update"></a>Aktualizacja jednostki ucznia

![Jednostka ucznia](complex-data-model/_static/student-entity.png)

Aktualizuj *modele/uczniów. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Wymagany atrybut

`Required` Atrybut powoduje, że właściwości nazwy są wymagane. Ten `Required` atrybut nie jest wymagany dla typów niedopuszczających wartości null, takich jak `int`typy `double`wartości (`DateTime`,, itp.). Typy, które nie mogą mieć wartości null, są automatycznie traktowane jako pola wymagane.

Ten `Required` atrybut może być zastąpiony parametrem o minimalnej długości `StringLength` w atrybucie:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Atrybut wyświetlania

Ten `Display` atrybut określa, że podpis pól tekstowych powinien mieć wartość "imię i nazwisko", "nazwisko", "imię i nazwisko" oraz "Data rejestracji". Domyślne podpisy nie zawierają spacji dzielących wyrazy, na przykład "LastName".

### <a name="the-fullname-calculated-property"></a>Właściwość obliczeniowa FullName

`FullName`jest właściwością obliczaną, która zwraca wartość utworzoną przez połączenie dwóch innych właściwości. `FullName`nie można ustawić, ma tylko metodę dostępu get. Nie `FullName` utworzono żadnej kolumny w bazie danych.

## <a name="create-the-instructor-entity"></a>Tworzenie jednostki instruktora

![Jednostka instruktora](complex-data-model/_static/instructor-entity.png)

Utwórz *modele/instruktor. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

Wiele atrybutów może znajdować się w jednym wierszu. `HireDate` Atrybuty mogą być zapisywane w następujący sposób:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>Właściwości nawigacji CourseAssignments i OfficeAssignment

Właściwości `CourseAssignments` i`OfficeAssignment` są właściwościami nawigacji.

Instruktor może nauczyć dowolną liczbę kursów, więc `CourseAssignments` jest zdefiniowany jako kolekcja.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Jeśli właściwość nawigacji zawiera wiele jednostek:

* Musi to być typ listy, w którym można dodawać, usuwać i aktualizować wpisy.

Typy właściwości nawigacji obejmują:

* `ICollection<T>`
* `List<T>`
* `HashSet<T>`

Jeśli `ICollection<T>` jest określony, EF Core domyślnie `HashSet<T>` tworzy kolekcję.

`CourseAssignment` Jednostka została omówiona w sekcji dotyczącej relacji wiele-do-wielu.

Firma Contoso University Rules States, że instruktor może mieć co najwyżej jednego biura. Właściwość zawiera jedną `OfficeAssignment`jednostkę. `OfficeAssignment` `OfficeAssignment`ma wartość null, jeśli nie przypisano pakietu Office.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Tworzenie jednostki OfficeAssignment

![Jednostka OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

Utwórz *modele/OfficeAssignment. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Atrybut klucza

Ten `[Key]` atrybut służy do identyfikowania właściwości jako klucza podstawowego (PK), gdy nazwa właściwości ma wartość inną niż classnameID lub ID.

Istnieje relacja jeden do zera między `Instructor` jednostkami i. `OfficeAssignment` Przypisanie pakietu Office istnieje tylko w odniesieniu do instruktora, do którego jest przypisane. Klucz podstawowy jest również jego kluczem obcym (obcy) `Instructor` do jednostki. `OfficeAssignment` EF Core nie może automatycznie `InstructorID` rozpoznać jako klucz podstawowy `OfficeAssignment` dla:

* `InstructorID`nie jest zgodna z konwencją nazewnictwa ID lub classnameID.

W związku z tym `InstructorID` atrybutjestużywanydoidentyfikacjijakokluczpodstawowy:`Key`

```csharp
[Key]
public int InstructorID { get; set; }
```

Domyślnie EF Core traktuje klucz jako wygenerowane poza bazą danych, ponieważ kolumna służy do identyfikowania relacji.

### <a name="the-instructor-navigation-property"></a>Właściwość nawigacji instruktora

Właściwość `OfficeAssignment` nawigacji `Instructor` dla jednostki nie dopuszcza wartości null, ponieważ:

* Typy odwołań (takie jak klasy są dopuszczające wartość null).
* Instruktor może nie mieć przypisania pakietu Office.

Jednostka ma właściwość nawigacji, która nie `Instructor` dopuszcza wartości null, ponieważ: `OfficeAssignment`

* `InstructorID`nie dopuszcza wartości null.
* Przypisanie pakietu Office nie może istnieć bez instruktora.

Gdy jednostka ma powiązaną `OfficeAssignment` jednostkę, każda jednostka ma odwołanie do drugiej z nich we właściwości nawigacji. `Instructor`

Ten `[Required]` atrybut może zostać zastosowany `Instructor` do właściwości nawigacji:

```csharp
[Required]
public Instructor Instructor { get; set; }
```

Poprzedni kod określa, że musi być pokrewnym instruktorem. Poprzedni kod jest niezbędny, `InstructorID` ponieważ klucz obcy (który jest również kluczem PK) nie dopuszcza wartości null.

## <a name="modify-the-course-entity"></a>Modyfikowanie jednostki kursu

![Jednostka kursu](complex-data-model/_static/course-entity.png)

Aktualizuj *modele/kurs. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

Jednostka ma właściwość `DepartmentID`klucza obcego (obcy). `Course` `DepartmentID`wskazuje powiązaną `Department` jednostkę. `Course` Jednostka ma właściwość nawigacji. `Department`

EF Core nie wymaga właściwości FK dla modelu danych, gdy model ma właściwość nawigacji dla powiązanej jednostki.

EF Core automatycznie tworzy FKs w bazie danych wszędzie tam, gdzie są one zbędne. EF Core tworzy [Właściwości cienia](/ef/core/modeling/shadow-properties) dla automatycznie utworzonych FKs. Posiadanie klucza obcego w modelu danych może sprawiać, że aktualizacje są prostsze i wydajniejsze. Rozważmy na przykład model, w którym Właściwość `DepartmentID` FK *nie* jest uwzględniona. Gdy jednostka kursu jest pobierana do edycji:

* Jednostka `Department` ma wartość null, jeśli nie jest jawnie załadowana.
* Aby zaktualizować jednostkę kursu, `Department` należy najpierw pobrać jednostkę.

Gdy właściwość `DepartmentID` FK jest uwzględniona w modelu danych, nie trzeba `Department` pobierać jednostki przed aktualizacją.

### <a name="the-databasegenerated-attribute"></a>Atrybut DatabaseGenerated

Ten `[DatabaseGenerated(DatabaseGeneratedOption.None)]` atrybut określa, że klucz podstawowy jest dostarczany przez aplikację, a nie generowany przez bazę danych.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Domyślnie EF Core zakłada, że wartości klucza PK są generowane przez bazę danych. Wartość klucza podstawowego wygenerowanego przez bazę danych jest ogólnie najlepszym rozwiązaniem. W `Course` przypadku jednostek użytkownik określa klucz podstawowy. Na przykład numer kursu, taki jak seria 1000 dla działu Math, serii 2000 dla działu angielskiego.

Ten `DatabaseGenerated` atrybut może być również używany do generowania wartości domyślnych. Na przykład baza danych może automatycznie wygenerować pole daty, aby zarejestrować datę utworzenia lub zaktualizowania wiersza. Aby uzyskać więcej informacji, zobacz [wygenerowane właściwości](/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Właściwości klucza obcego i nawigacji

Właściwości klucza obcego (FK) i właściwości nawigacji w `Course` jednostce odzwierciedlają następujące relacje:

Kurs jest przypisywany do jednego działu, więc istnieje `DepartmentID` obcy `Department` i właściwość nawigacji.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Kurs może zawierać dowolną liczbę studentów, więc `Enrollments` właściwość nawigacji jest kolekcją:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Kurs może być nauczany przez wiele instruktorów, więc `CourseAssignments` właściwość nawigacji jest kolekcją:

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment`wyjaśniono [później](#many-to-many-relationships).

## <a name="create-the-department-entity"></a>Tworzenie jednostki działu

![Jednostka działu](complex-data-model/_static/department-entity.png)

Utwórz *modele/dział. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Atrybut Column

`Column` Wcześniej atrybut został użyty do zmiany mapowania nazw kolumn. W kodzie dla `Department` jednostki `Column` atrybut jest używany do zmiany mapowania typu danych SQL. `Budget` Kolumna jest definiowana przy użyciu SQL Server typie pieniędzy w bazie danych:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Mapowanie kolumn zazwyczaj nie jest wymagane. EF Core zwykle wybiera odpowiedni SQL Server typ danych oparty na typie CLR właściwości. Typ CLR `decimal` jest mapowany na typ SQL Server `decimal` . `Budget`jest dla waluty, a typ danych walutowych jest bardziej odpowiedni dla waluty.

### <a name="foreign-key-and-navigation-properties"></a>Właściwości klucza obcego i nawigacji

Właściwości FK i nawigacji odzwierciedlają następujące relacje:

* Dział może lub nie ma uprawnienia administratora.
* Administrator jest zawsze instruktorem. W związku z tym `Instructor` Właściwośćjestdołączanajakoobcydojednostki.`InstructorID`

Właściwość nawigacji ma nazwę `Administrator` , ale `Instructor` zawiera jednostkę:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Znak zapytania (?) w powyższym kodzie określa właściwość dopuszcza wartość null.

Dział może mieć wiele kursów, więc istnieje właściwość nawigacji kursów:

```csharp
public ICollection<Course> Courses { get; set; }
```

Uwaga: Zgodnie z Konwencją EF Core włącza kaskadowe usuwanie dla FKs niedopuszczających wartości null i dla relacji wiele-do-wielu. Kaskadowe usuwanie może spowodować cykliczne reguły usuwania kaskadowego. Cykliczne reguły usuwania kaskadowego powodują wyjątek podczas dodawania migracji.

Na przykład, jeśli `Department.InstructorID` właściwość została zdefiniowana jako niedopuszczający wartości null:

* EF Core konfiguruje regułę usuwania kaskadowego w celu usunięcia działu po usunięciu instruktora.
* Usunięcie działu po usunięciu instruktora nie jest zamierzonym zachowaniem.
* Poniższy [interfejs API Fluent](#fluent-api-alternative-to-attributes) ustawi regułę ograniczenia zamiast kaskadowo.

   ```csharp
   modelBuilder.Entity<Department>()
      .HasOne(d => d.Administrator)
      .WithMany()
      .OnDelete(DeleteBehavior.Restrict)
  ```

Poprzedni kod wyłącza usuwanie kaskadowe dla relacji instruktora działu.

## <a name="update-the-enrollment-entity"></a>Aktualizowanie jednostki rejestracji

Rekord rejestracji jest wykonywany przez jednego ucznia.

![Jednostka rejestracji](complex-data-model/_static/enrollment-entity.png)

Aktualizuj *modele/rejestrację. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Właściwości klucza obcego i nawigacji

Właściwości klucza obcego i właściwości nawigacji odzwierciedlają następujące relacje:

Rekord rejestracji jest przeznaczony dla jednego kursu, dlatego istnieje `CourseID` Właściwość FK `Course` i właściwość nawigacji:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Rekord rejestracji jest przeznaczony dla jednego ucznia, dlatego istnieje `StudentID` właściwość klucza obcego `Student` i właściwość nawigacji:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relacje wiele do wielu

Istnieje relacja wiele-do-wielu między `Student` obiektami i. `Course` Jednostka działa jako tabela sprzężenia wiele-do-wielu *z ładunkiem* w bazie danych. `Enrollment` "Z ładunkiem" oznacza, że `Enrollment` tabela zawiera dodatkowe dane oprócz FKs dla sprzężonych tabel (w tym przypadku klucz podstawowy i `Grade`).

Na poniższej ilustracji pokazano, jak wyglądają te relacje w diagramie jednostek. (Ten diagram został wygenerowany przy użyciu [narzędzi EF PowerShell](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) dla programu EF 6. x. Tworzenie diagramu nie jest częścią samouczka.

![Student-kurs wiele do wielu relacji](complex-data-model/_static/student-course.png)

Każda linia relacji ma 1 na jednym końcu i gwiazdkę (*) na drugim, wskazując relację jeden do wielu.

Jeśli tabela nie zawiera informacji o klasie, musi zawierać tylko dwa FKs (`CourseID` i `StudentID`). `Enrollment` Tabela sprzężenia wiele-do-wielu bez ładunku jest czasami nazywana tabelą sprzężenia czystego (PJT).

Jednostki `Instructor` i`Course` mają relację wiele-do-wielu przy użyciu czystej tabeli sprzężenia.

Uwaga: Dr 6. x obsługuje niejawne tabele sprzężeń dla relacji wiele-do-wielu, ale EF Core nie. Aby uzyskać więcej informacji, zobacz [relacje wiele-do-wielu w EF Core 2,0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).

## <a name="the-courseassignment-entity"></a>Jednostka CourseAssignment

![Jednostka CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

Utwórz *modele/CourseAssignment. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a>Instruktorzy do kursów

![M:M instruktora do kursu](complex-data-model/_static/courseassignment.png)

"Instruktora" — relacja wiele do wielu:

* Wymaga tabeli sprzężenia, która musi być reprezentowana przez zestaw jednostek.
* Jest czystym tabelą sprzężenia (tabela bez ładunku).

Nazwa jednostki `EntityName1EntityName2`sprzężenia jest wspólna. Na przykład tabela sprzężenia "instruktor-kurs" używa tego wzorca jest `CourseInstructor`. Zalecamy jednak użycie nazwy opisującej relację.

Modele danych rozpoczynają się od siebie i rosną. Sprzężenia bez ładunku (PJTs) często są rozwijane w celu uwzględnienia ładunku. Rozpoczynając od nazwy obiektu opisowego, nie trzeba zmieniać nazwy po zmianie tabeli sprzężeń. Najlepiej, aby jednostka sprzężenia miała własną nazwę naturalną (prawdopodobnie jednowyrazową) w domenie biznesowej. Na przykład książki i klienci mogą być połączone z jednostką sprzężenia o nazwie ratings. Dla instruktora "wiele do wielu", `CourseAssignment` `CourseInstructor`preferowana jest wartość.

### <a name="composite-key"></a>Klucz złożony

FKs nie dopuszcza wartości null. Dwa FKs w `CourseAssignment` (`InstructorID` `CourseID` i`CourseAssignment` ) jednoznacznie identyfikują każdy wiersz tabeli. `CourseAssignment`nie wymaga dedykowanego klucza podstawowego. Właściwości `InstructorID` i`CourseID` działają jako złożony klucz podstawowy. Jedynym sposobem określenia złożonego PKs do EF Core jest *interfejs API Fluent*. W następnej sekcji przedstawiono sposób konfigurowania złożonego klucza podstawowego.

Klucz złożony gwarantuje:

* W jednym kursie są dozwolone wiele wierszy.
* Dla jednego instruktora są dozwolone wiele wierszy.
* Wiele wierszy dla tego samego instruktora i kursu jest niedozwolonych.

Jednostka `Enrollment` Join definiuje własny klucz podstawowy, więc możliwe jest duplikowanie tego sortowania. Aby uniknąć takich duplikatów:

* Dodaj unikatowy indeks do pól klucza obcego lub
* Skonfiguruj `Enrollment` przy użyciu podstawowego klucza złożonego podobnego `CourseAssignment`do. Aby uzyskać więcej informacji, zobacz [indeksy](/ef/core/modeling/indexes).

## <a name="update-the-db-context"></a>Aktualizowanie kontekstu bazy danych

Dodaj następujący wyróżniony kod do *danych/SchoolContext. cs*:

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Poprzedni kod dodaje nowe jednostki i konfiguruje `CourseAssignment` złożonego klucza podstawowego jednostki.

## <a name="fluent-api-alternative-to-attributes"></a>Alternatywny interfejs API Fluent dla atrybutów

Metoda w poprzednim kodzie używa *interfejsu API Fluent* , aby skonfigurować zachowanie EF Core. `OnModelCreating` Interfejs API jest nazywany "Fluent", ponieważ jest często używany przez ciąg serii wywołań metod w jednej instrukcji. [Poniższy kod](/ef/core/modeling/#use-fluent-api-to-configure-a-model) jest przykładem interfejsu API Fluent:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

W tym samouczku interfejs API Fluent jest używany tylko w przypadku mapowania bazy danych, której nie można wykonać przy użyciu atrybutów. Jednak interfejs API Fluent może określić większość reguł formatowania, walidacji i mapowania, które mogą być wykonywane przy użyciu atrybutów.

Niektórych atrybutów, takich `MinimumLength` jak nie można zastosować w interfejsie API Fluent. `MinimumLength`nie zmienia schematu, stosuje tylko regułę walidacji o minimalnej długości.

Niektórzy deweloperzy wolą korzystać z interfejsu API Fluent wyłącznie w taki sposób, aby mogli utrzymać czyste klasy jednostek. Atrybuty i interfejs API Fluent mogą być mieszane. Istnieją pewne konfiguracje, które można wykonać tylko przy użyciu interfejsu API Fluent (określenie złożonego klucza podstawowego). Istnieją pewne konfiguracje, które można wykonać tylko z atrybutami (`MinimumLength`). Zalecane rozwiązanie dotyczące korzystania z interfejsu API Fluent lub atrybutów:

* Wybierz jedną z tych dwóch metod.
* W miarę możliwości należy stosować wybrane podejście.

Niektóre z atrybutów używanych w tym samouczku są używane w programie:

* Tylko Walidacja (na przykład `MinimumLength`).
* Tylko konfiguracja EF Core (na przykład `HasKey`).
* Sprawdzanie poprawności i konfiguracja EF Core (na przykład `[StringLength(50)]`).

Aby uzyskać więcej informacji na temat atrybutów a interfejs API Fluent, zobacz [metody konfiguracji](/ef/core/modeling/).

## <a name="entity-diagram-showing-relationships"></a>Diagram jednostek przedstawiający relacje

Na poniższej ilustracji przedstawiono diagram, który jest tworzony przez narzędzia Dr PowerShell dla gotowego modelu szkoły.

![Diagram jednostek](complex-data-model/_static/diagram.png)

Na powyższym diagramie przedstawiono:

* Kilka linii relacji jeden-do-wielu (od 1 \*do).
* Linia relacji "jeden do zera" (od 1 do 0.. 1) między `Instructor` jednostkami i. `OfficeAssignment`
* Linia relacji zero-lub-jeden-do-wielu (0.. 1 do *) między `Instructor` obiektami i. `Department`

## <a name="seed-the-db-with-test-data"></a>Wypełnianie bazy danych danymi testowymi

Zaktualizuj kod w *danych/Dbinitializeer. cs*:

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

Poprzedni kod zawiera dane inicjatora dla nowych jednostek. Większość tego kodu tworzy nowe obiekty Entity i ładuje przykładowe dane. Przykładowe dane służą do testowania. Zobacz `Enrollments` i `CourseAssignments` , aby zapoznać się z przykładami tabel sprzężenia wiele-do-wielu.

## <a name="add-a-migration"></a>Dodawanie migracji

Skompiluj projekt.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```console
dotnet ef migrations add ComplexDataModel
```

---

Poprzednie polecenie wyświetla ostrzeżenie o możliwej utracie danych.

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

`database update` Jeśli polecenie zostanie uruchomione, zostanie utworzony następujący błąd:

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a>Zastosuj migrację

Teraz, gdy masz już istniejącą bazę danych, musisz się zastanowić, jak zastosować w niej przyszłe zmiany. Ten samouczek przedstawia dwa podejścia:

* [Porzuć i ponownie utwórz bazę danych](#drop)
* [Zastosuj migrację do istniejącej bazy danych](#applyexisting). Chociaż ta metoda jest bardziej złożona i czasochłonna, jest preferowanym podejściem dla rzeczywistych środowisk produkcyjnych. **Uwaga**: Jest to opcjonalna sekcja samouczka. Możesz wykonać kroki porzucenia i utworzyć ponownie i pominąć tę sekcję. Jeśli chcesz wykonać kroki opisane w tej sekcji, nie wykonuj kroków usuwania i ponownego tworzenia. 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a>Porzuć i ponownie utwórz bazę danych

Kod w zaktualizowanych `DbInitializer` dodaje dane inicjatora dla nowych jednostek. Aby wymusić EF Core tworzenia nowej bazy danych, Porzuć i zaktualizuj bazę danych:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W **konsoli Menedżera pakietów** (PMC) Uruchom następujące polecenie:

```PMC
Drop-Database
Update-Database
```

Uruchom `Get-Help about_EntityFrameworkCore` z PMC, aby uzyskać informacje pomocy.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Otwórz okno polecenia i przejdź do folderu projektu. Folder projektu zawiera plik *Startup.cs* .

Wprowadź następujące polecenie w oknie polecenia:

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

---

Uruchom aplikację. Uruchomienie aplikacji uruchamia `DbInitializer.Initialize` metodę. `DbInitializer.Initialize` Wypełnia nową bazę danych.

Otwórz bazę danych w programie SSOX:

* Jeśli SSOX został wcześniej otwarty, kliknij przycisk **Odśwież** .
* Rozwiń **tabel** węzła. Zostaną wyświetlone utworzone tabele.

![Tabele w SSOX](complex-data-model/_static/ssox-tables.png)

Zapoznaj się z tabelą **CourseAssignment** :

* Kliknij prawym przyciskiem myszy tabelę **CourseAssignment** , a następnie wybierz polecenie **Wyświetl dane**.
* Sprawdź, czy tabela **CourseAssignment** zawiera dane.

![CourseAssignment dane w SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a>Zastosowanie migracji do istniejącej bazy danych

Ta sekcja jest opcjonalna. Kroki te działają tylko wtedy, gdy pominięto poprzednią [listę i ponownie utworzysz sekcję bazy danych](#drop) .

Po uruchomieniu migracji z istniejącymi danymi mogą istnieć ograniczenia klucza obcego, które nie są spełnione w przypadku istniejących danych. W przypadku danych produkcyjnych należy wykonać kroki, aby przeprowadzić migrację istniejących danych. Ta sekcja zawiera przykład rozwiązywania naruszeń ograniczenia klucza obcego. Nie należy wprowadzać tych zmian w kodzie bez tworzenia kopii zapasowej. Nie wprowadzaj tych zmian w kodzie, jeśli wykonano poprzednią sekcję i Zaktualizowano bazę danych.

Plik *{timestamp} _ComplexDataModel. cs* zawiera następujący kod:

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

Poprzedzający kod dodaje `DepartmentID` `Course` do tabeli obcy niedopuszczający wartości null. Baza danych z poprzedniego samouczka zawiera wiersze w `Course`, dlatego nie można zaktualizować tabeli za pomocą migracji.

Aby `ComplexDataModel` migracja była współdziałać z istniejącymi danymi:

* Zmień kod, aby nadać nowej kolumnie (`DepartmentID`) wartość domyślną.
* Utwórz fałszywy Departament o nazwie "Temp", który ma pełnić rolę działu domyślnego.

#### <a name="fix-the-foreign-key-constraints"></a>Popraw ograniczenia klucza obcego

Zaktualizuj metodę `Up`klasy: `ComplexDataModel`

* Otwórz plik *{timestamp} _ComplexDataModel. cs* .
* Dodaj komentarz `DepartmentID` `Course` do wiersza kodu, który dodaje kolumnę do tabeli.

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

Dodaj następujący wyróżniony kod. Nowy kod przechodzi po `.CreateTable( name: "Department"` bloku:

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

Po wykonaniu powyższych zmian istniejące `Course` wiersze będą powiązane z działem "Temp" po uruchomieniumetody.`Up` `ComplexDataModel`

Aplikacja produkcyjna:

* Dołącz kod lub skrypty, aby `Department` dodać wiersze i `Course` powiązane wiersze do nowych `Department` wierszy.
* Nie używaj działu "Temp" ani wartości domyślnej dla `Course.DepartmentID`.

Następny samouczek obejmuje powiązane dane.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja usługi YouTube w tym samouczku (część 1)](https://www.youtube.com/watch?v=0n2f0ObgCoA)
* [Wersja usługi YouTube w tym samouczku (część 2)](https://www.youtube.com/watch?v=Je0Z5K1TNmY)

> [!div class="step-by-step"]
> [Poprzedni](xref:data/ef-rp/migrations)Następny
> [](xref:data/ef-rp/read-related-data)

::: moniker-end
