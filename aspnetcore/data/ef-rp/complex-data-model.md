---
title: Razor Pages EF Core w ASP.NET Core — model danych-5 z 8
author: rick-anderson
description: W tym samouczku Dodaj więcej jednostek i relacje i Dostosuj model danych, określając formatowanie, walidację i reguły mapowania.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 411c0874d2b2c6ecadd1da9aff7a093f1e8e525a
ms.sourcegitcommit: d2ba66023884f0dca115ff010bd98d5ed6459283
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2020
ms.locfileid: "77213431"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a>Razor Pages EF Core w ASP.NET Core — model danych-5 z 8

Autorzy [Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

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

Poprzedni kod dodaje właściwość `FullName` i dodaje następujące atrybuty do istniejących właściwości:

* `[DataType]`
* `[DisplayFormat]`
* `[StringLength]`
* `[Column]`
* `[Required]`
* `[Display]`

### <a name="the-fullname-calculated-property"></a>Właściwość obliczeniowa FullName

`FullName` jest właściwością obliczaną, która zwraca wartość utworzoną przez połączenie dwóch innych właściwości. nie można ustawić `FullName`, więc ma tylko metodę dostępu get. Nie utworzono kolumny `FullName` w bazie danych.

### <a name="the-datatype-attribute"></a>Atrybut DataType

```csharp
[DataType(DataType.Date)]
```

W przypadku dat rejestracji uczniów wszystkie strony wyświetlają teraz godzinę i datę, chociaż tylko data jest ważna. Używając atrybutów adnotacji danych, można wprowadzić jedną zmianę kodu, która naprawi format wyświetlania na każdej stronie, która wyświetla dane. 

Atrybut [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) określa typ danych, który jest bardziej szczegółowy niż typ wewnętrzny bazy danych. W tym przypadku należy wyświetlić tylko datę, a nie datę i godzinę. [Wyliczenie DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) zawiera wiele typów danych, takich jak data, godzina, numer telefonu, waluta, EmailAddress itp. Atrybut `DataType` może również umożliwić aplikacji automatyczne udostępnianie funkcji specyficznych dla typu. Na przykład:

* `mailto:` link jest tworzony automatycznie dla `DataType.EmailAddress`.
* Selektor daty jest dostępny dla `DataType.Date` w większości przeglądarek.

Atrybut `DataType` emituje `data-` HTML 5 (wymawiane kreski danych). Atrybuty `DataType` nie zapewniają walidacji.

### <a name="the-displayformat-attribute"></a>Atrybut DisplayFormat

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`DataType.Date` nie określa formatu wyświetlanej daty. Domyślnie pole Date jest wyświetlane zgodnie z domyślnymi formatami opartymi na [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support)serwera.

Atrybut `DisplayFormat` jest używany do jawnego określania formatu daty. Ustawienie `ApplyFormatInEditMode` określa, że formatowanie powinno być również stosowane do interfejsu użytkownika edytowania. Niektóre pola nie powinny używać `ApplyFormatInEditMode`. Na przykład symbol waluty zazwyczaj nie powinien być wyświetlany w polu tekstowym Edycja.

Atrybut `DisplayFormat` może być używany przez siebie. Zwykle dobrym pomysłem jest użycie atrybutu `DataType` z atrybutem `DisplayFormat`. Atrybut `DataType` przekazuje semantykę danych w przeciwieństwie do sposobu renderowania na ekranie. Atrybut `DataType` zapewnia następujące korzyści, które nie są dostępne w `DisplayFormat`:

* Przeglądarka może włączyć funkcje HTML5. Na przykład Pokaż kontrolkę kalendarz, odpowiedni dla ustawień regionalnych symbol waluty, linki poczty e-mail i sprawdzanie poprawności danych po stronie klienta.
* Domyślnie przeglądarka renderuje dane przy użyciu poprawnego formatu na podstawie ustawień regionalnych.

Aby uzyskać więcej informacji, zobacz [dokumentację pomocnika tagów\<input >](xref:mvc/views/working-with-forms#the-input-tag-helper).

### <a name="the-stringlength-attribute"></a>Atrybut StringLength

```csharp
[StringLength(50, ErrorMessage = "First name cannot be longer than 50 characters.")]
```

Można określić reguły walidacji danych i komunikaty o błędach walidacji z atrybutami. Atrybut [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) określa minimalną i maksymalną długość znaków, które są dozwolone w polu danych. Kod pokazuje limity nazw do nie więcej niż 50 znaków. Przykład określający, że minimalna długość ciągu jest pokazywana w [dalszej](#the-required-attribute)części.

Atrybut `StringLength` zapewnia również weryfikację po stronie klienta i serwera. Wartość minimalna nie ma wpływu na schemat bazy danych.

Atrybut `StringLength` nie uniemożliwia użytkownikowi wprowadzania białych znaków w nazwie. Atrybut [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) może służyć do stosowania ograniczeń do danych wejściowych. Na przykład poniższy kod wymaga, aby pierwszy znak był wielką literą, a pozostałe znaki są alfabetyczne:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W **Eksplorator obiektów SQL Server** (SSOX) Otwórz projektanta tabeli uczniów, klikając dwukrotnie tabelę **uczniów** .

![Tabela studentów w SSOX przed migracją](complex-data-model/_static/ssox-before-migration.png)

Na powyższym obrazie przedstawiono schemat tabeli `Student`. Pola nazw mają typ `nvarchar(MAX)`. Gdy migracja zostanie utworzona i zastosowana w dalszej części tego samouczka, pola Nazwa stają się `nvarchar(50)` w wyniku atrybutów długości ciągu.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

W narzędziu SQLite Sprawdź definicje kolumn tabeli `Student`. Pola nazw mają typ `Text`. Zwróć uwagę, że pole first name ma nazwę `FirstMidName`. W następnej sekcji zmienisz nazwę tej kolumny na `FirstName`.

---

### <a name="the-column-attribute"></a>Atrybut Column

```csharp
[Column("FirstName")]
public string FirstMidName { get; set; }
```

Atrybuty mogą kontrolować sposób mapowania klas i właściwości do bazy danych. W modelu `Student` atrybut `Column` jest używany do mapowania nazwy właściwości `FirstMidName` na "FirstName" w bazie danych.

Po utworzeniu bazy danych nazwy właściwości w modelu są używane dla nazw kolumn (z wyjątkiem sytuacji, gdy atrybut `Column` jest używany). Model `Student` używa `FirstMidName` dla pola pierwszej nazwy, ponieważ pole może zawierać również nazwę środkową.

`[Column]` atrybutu `Student.FirstMidName` w modelu danych mapuje do kolumny `FirstName` tabeli `Student`. Dodanie atrybutu `Column` powoduje zmianę modelu z kopią zapasową `SchoolContext`. Model wykonujący kopię zapasową `SchoolContext` nie jest już zgodny z bazą danych. Niezgodność zostanie rozwiązany przez dodanie migracji w dalszej części tego samouczka.

### <a name="the-required-attribute"></a>Wymagany atrybut

```csharp
[Required]
```

Atrybut `Required` powoduje, że pola nazwy są wymagane. Atrybut `Required` nie jest wymagany dla typów niedopuszczających wartości null, takich jak typy wartości (na przykład `DateTime`, `int`i `double`). Typy, które nie mogą mieć wartości null, są automatycznie traktowane jako pola wymagane.

Atrybut `Required` musi być używany z `MinimumLength`, aby można było wymusić `MinimumLength`.

```csharp
[Display(Name = "Last Name")]
[Required]
[StringLength(50, MinimumLength=2)]
public string LastName { get; set; }
```

`MinimumLength` i `Required` Zezwalaj na odstępy, aby spełnić kryteria weryfikacji. Użyj atrybutu `RegularExpression`, aby uzyskać pełną kontrolę nad ciągiem.

### <a name="the-display-attribute"></a>Atrybut wyświetlania

```csharp
[Display(Name = "Last Name")]
```

Atrybut `Display` określa, że podpis pól tekstowych powinien mieć wartość "imię i nazwisko", "nazwisko", "imię i nazwisko" oraz "Data rejestracji". Domyślne podpisy nie zawierają spacji dzielących wyrazy, na przykład "LastName".

### <a name="create-a-migration"></a>Tworzenie migracji

Uruchom aplikację i przejdź do strony uczniów. Zgłaszany jest wyjątek. `[Column]` atrybutu powoduje, że Dr powinien znaleźć kolumnę o nazwie `FirstName`, ale nazwa kolumny w bazie danych jest nadal `FirstMidName`.

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

  Przed zainstalowaniem migracji kolumny nazw były typu [nvarchar (max)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql). Kolumny nazw są teraz `nvarchar(50)`. Nazwa kolumny została zmieniona z `FirstMidName` na `FirstName`.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Komunikat o błędzie jest podobny do poniższego przykładu:

```
SqliteException: SQLite Error 1: 'no such column: s.FirstName'.
```

* Otwórz okno polecenia w folderze projektu. Wprowadź następujące polecenia, aby utworzyć nową migrację i zaktualizować bazę danych:

  ```dotnetcli
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

  ```dotnetcli
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

Wiele atrybutów może znajdować się w jednym wierszu. Atrybuty `HireDate` mogą być zapisywane w następujący sposób:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="navigation-properties"></a>Właściwości nawigacji

Właściwości `CourseAssignments` i `OfficeAssignment` są właściwościami nawigacji.

Instruktor może uczyć się dowolnej liczby kursów, dlatego `CourseAssignments` jest zdefiniowany jako kolekcja.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Instruktor może mieć co najwyżej jeden pakiet Office, więc Właściwość `OfficeAssignment` zawiera jedną jednostkę `OfficeAssignment`. `OfficeAssignment` ma wartość null, jeśli nie przypisano pakietu Office.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="the-officeassignment-entity"></a>Jednostka OfficeAssignment

![Jednostka OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

Utwórz *modele/OfficeAssignment. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Atrybut klucza

Atrybut `[Key]` jest używany do identyfikowania właściwości jako klucza podstawowego (PK), gdy nazwa właściwości jest inna niż classnameID lub ID.

Istnieje relacja jeden do zera między jednostkami `Instructor` i `OfficeAssignment`. Przypisanie pakietu Office istnieje tylko w odniesieniu do instruktora, do którego jest przypisane. `OfficeAssignment` klucz podstawowy jest również jego kluczem obcym (FK) do jednostki `Instructor`.

EF Core nie może automatycznie rozpoznać `InstructorID` jako klucza podstawowego dla `OfficeAssignment`, ponieważ `InstructorID` nie jest zgodna z konwencją nazewnictwa ID lub classnameID. W związku z tym, atrybut `Key` jest używany do identyfikowania `InstructorID` jako klucz podstawowy:

```csharp
[Key]
public int InstructorID { get; set; }
```

Domyślnie EF Core traktuje klucz jako wygenerowane poza bazą danych, ponieważ kolumna służy do identyfikowania relacji.

### <a name="the-instructor-navigation-property"></a>Właściwość nawigacji instruktora

Właściwość nawigacji `Instructor.OfficeAssignment` może mieć wartość null, ponieważ nie może istnieć wiersz `OfficeAssignment` dla danego instruktora. Instruktor może nie mieć przypisania pakietu Office.

Właściwość nawigacji `OfficeAssignment.Instructor` będzie zawsze mieć jednostkę instruktora, ponieważ typ `InstructorID` klucza obcego to `int`, typ wartości niedopuszczający wartości null. Przypisanie pakietu Office nie może istnieć bez instruktora.

Gdy jednostka `Instructor` ma pokrewną jednostkę `OfficeAssignment`, każda jednostka ma odwołanie do drugiej z nich we właściwości nawigacji.

## <a name="the-course-entity"></a>Jednostka kursu

![Jednostka kursu](complex-data-model/_static/course-entity.png)

Aktualizuj *modele/kurs. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Models/Course.cs?highlight=2,10,13,16,19,21,23)]

Jednostka `Course` ma `DepartmentID`właściwość klucza obcego (obcy). `DepartmentID` punkty do powiązanej jednostki `Department`. Jednostka `Course` ma `Department` właściwość nawigacji.

EF Core nie wymaga właściwości klucza obcego dla modelu danych, gdy model ma właściwość nawigacji dla powiązanej jednostki. EF Core automatycznie tworzy FKs w bazie danych wszędzie tam, gdzie są one zbędne. EF Core tworzy [Właściwości cienia](/ef/core/modeling/shadow-properties) dla automatycznie utworzonych FKs. Jednak jawne dołączenie klucza obcego w modelu danych może spowodować uproszczenie i wydajniejsze aktualizacje. Rozważmy na przykład model, w którym Właściwość FK `DepartmentID` *nie* jest uwzględniona. Gdy jednostka kursu jest pobierana do edycji:

* Właściwość `Department` ma wartość null, jeśli nie została jawnie załadowana.
* Aby zaktualizować jednostkę kursu, najpierw należy pobrać jednostkę `Department`.

Gdy właściwość FK `DepartmentID` jest uwzględniona w modelu danych, nie ma potrzeby pobierania jednostki `Department` przed aktualizacją.

### <a name="the-databasegenerated-attribute"></a>Atrybut DatabaseGenerated

Atrybut `[DatabaseGenerated(DatabaseGeneratedOption.None)]` określa, że klucz podstawowy jest dostarczany przez aplikację, a nie generowany przez bazę danych.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Domyślnie EF Core zakłada, że wartości klucza PK są generowane przez bazę danych. Wygenerowana baza danych jest ogólnie najlepszym rozwiązaniem. W przypadku jednostek `Course` użytkownik określa klucz podstawowy. Na przykład numer kursu, taki jak seria 1000 dla działu Math, serii 2000 dla działu angielskiego.

Atrybut `DatabaseGenerated` może być również używany do generowania wartości domyślnych. Na przykład baza danych może automatycznie wygenerować pole daty, aby zarejestrować datę utworzenia lub zaktualizowania wiersza. Aby uzyskać więcej informacji, zobacz [wygenerowane właściwości](/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Właściwości klucza obcego i nawigacji

Właściwości klucza obcego (FK) i właściwości nawigacji w jednostce `Course` odzwierciedlają następujące relacje:

Kurs jest przypisywany do jednego działu, dlatego istnieje `DepartmentID` FK i `Department` właściwość nawigacji.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

W kursie może być zarejestrowana dowolna liczba uczniów, więc właściwość nawigacji `Enrollments` jest kolekcją:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Kurs może być nauczany przez wiele instruktorów, więc właściwość nawigacji `CourseAssignments` jest kolekcją:

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment` został wyjaśniony [później](#many-to-many-relationships).

## <a name="the-department-entity"></a>Jednostka działu

![Jednostka działu](complex-data-model/_static/department-entity.png)

Utwórz *modele/dział. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Models/Department1.cs)]

### <a name="the-column-attribute"></a>Atrybut Column

Wcześniej atrybut `Column` został użyty do zmiany mapowania nazw kolumn. W kodzie dla jednostki `Department` atrybut `Column` służy do zmiany mapowania typu danych SQL. Kolumna `Budget` jest definiowana przy użyciu SQL Server typie pieniędzy w bazie danych:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Mapowanie kolumn zazwyczaj nie jest wymagane. EF Core wybiera odpowiedni SQL Server typ danych na podstawie typu CLR dla właściwości. Typ `decimal` CLR mapuje do typu `decimal` SQL Server. `Budget` jest dla waluty, a typ danych walutowych jest bardziej odpowiedni dla waluty.

### <a name="foreign-key-and-navigation-properties"></a>Właściwości klucza obcego i nawigacji

Właściwości FK i nawigacji odzwierciedlają następujące relacje:

* Dział może lub nie ma uprawnienia administratora.
* Administrator jest zawsze instruktorem. W związku z tym Właściwość `InstructorID` jest dołączana jako obcy do jednostki `Instructor`.

Właściwość nawigacji ma nazwę `Administrator`, ale zawiera jednostkę `Instructor`:

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

Na przykład jeśli właściwość `Department.InstructorID` została zdefiniowana jako nie dopuszczający wartości null, EF Core skonfigurować regułę usuwania kaskadowego. W takim przypadku dział zostanie usunięty po usunięciu instruktora przypisanego jako administrator. W tym scenariuszu reguła ograniczania byłaby bardziej zrozumiała. Poniższy [interfejs API Fluent](#fluent-api-alternative-to-attributes) ustawi regułę ograniczenia i wyłącza usuwanie kaskadowe.

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

Rekord rejestracji jest przeznaczony dla jednego kursu, dlatego istnieje właściwość `CourseID` obcego i właściwość nawigacji `Course`:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Rekord rejestracji jest przeznaczony dla jednego ucznia, dlatego istnieje właściwość `StudentID` obcego i właściwość nawigacji `Student`:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relacje wiele do wielu

Istnieje relacja wiele-do-wielu między jednostkami `Student` i `Course`. Jednostka `Enrollment` działa jako tabela sprzężeń wiele-do-wielu *z ładunkiem* w bazie danych. "Z ładunkiem" oznacza, że tabela `Enrollment` zawiera dodatkowe dane oprócz FKs dla sprzężonych tabel (w tym przypadku klucz podstawowy i `Grade`).

Na poniższej ilustracji pokazano, jak wyglądają te relacje w diagramie jednostek. (Ten diagram został wygenerowany przy użyciu [narzędzi EF PowerShell](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) dla programu EF 6. x. Tworzenie diagramu nie jest częścią samouczka.

![Student-kurs wiele do wielu relacji](complex-data-model/_static/student-course.png)

Każda linia relacji ma 1 na jednym końcu i gwiazdkę (*) na drugim, wskazując relację jeden do wielu.

Jeśli tabela `Enrollment` nie zawiera informacji o klasie, musi zawierać tylko dwa FKs (`CourseID` i `StudentID`). Tabela sprzężenia wiele-do-wielu bez ładunku jest czasami nazywana tabelą sprzężenia czystego (PJT).

Jednostki `Instructor` i `Course` mają relację wiele-do-wielu przy użyciu czystej tabeli sprzężenia.

Uwaga: w odniesieniu do relacji wiele-do-wielu są obsługiwane niejawne tabele sprzężenia w programie Dr 6. x, ale nie EF Core. Aby uzyskać więcej informacji, zobacz [relacje wiele-do-wielu w EF Core 2,0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).

## <a name="the-courseassignment-entity"></a>Jednostka CourseAssignment

![Jednostka CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

Utwórz *modele/CourseAssignment. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Models/CourseAssignment.cs)]

Relacja "instruktora" do wielu kursów wymaga tabeli sprzężenia, a jednostka dla tej tabeli sprzężenia jest CourseAssignment.

![M:M instruktora do kursu](complex-data-model/_static/courseassignment.png)

Nazwa jednostki sprzężenia jest wspólna dla `EntityName1EntityName2`. Na przykład tabela sprzężenia "instruktor do kursu" przy użyciu tego wzorca byłaby `CourseInstructor`. Zalecamy jednak użycie nazwy opisującej relację.

Modele danych rozpoczynają się od siebie i rosną. Tabele sprzężenia bez ładunku (PJTs) często są rozwijane w celu uwzględnienia ładunku. Rozpoczynając od nazwy obiektu opisowego, nie trzeba zmieniać nazwy po zmianie tabeli sprzężeń. Najlepiej, aby jednostka sprzężenia miała własną nazwę naturalną (prawdopodobnie jednowyrazową) w domenie biznesowej. Na przykład książki i klienci mogą być połączone z jednostką sprzężenia o nazwie ratings. Dla instruktora "wiele do wielu", `CourseAssignment` jest preferowana przez `CourseInstructor`.

### <a name="composite-key"></a>Klucz złożony

Dwa FKs w `CourseAssignment` (`InstructorID` i `CourseID`) jednoznacznie identyfikują każdy wiersz tabeli `CourseAssignment`. `CourseAssignment` nie wymaga dedykowanego klucza podstawowego. Właściwości `InstructorID` i `CourseID` są funkcją złożonego klucza podstawowego. Jedynym sposobem określenia złożonego PKs do EF Core jest *interfejs API Fluent*. W następnej sekcji przedstawiono sposób konfigurowania złożonego klucza podstawowego.

Klucz złożony gwarantuje, że:

* W jednym kursie są dozwolone wiele wierszy.
* Dla jednego instruktora są dozwolone wiele wierszy.
* Wiele wierszy nie jest dozwolonych dla tego samego instruktora i kursu.

Jednostka `Enrollment` Join definiuje własny klucz podstawowy, więc możliwe jest duplikowanie tego sortowania. Aby uniknąć takich duplikatów:

* Dodaj unikatowy indeks do pól klucza obcego lub
* Skonfiguruj `Enrollment` przy użyciu podstawowego klucza złożonego podobnego do `CourseAssignment`. Aby uzyskać więcej informacji, zobacz [indeksy](/ef/core/modeling/indexes).

## <a name="update-the-database-context"></a>Aktualizowanie kontekstu bazy danych

Zaktualizuj *dane/SchoolContext. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu30/Data/SchoolContext.cs?highlight=15-18,25-31)]

Poprzedni kod dodaje nowe jednostki i konfiguruje złożonego klucza podstawowego jednostki `CourseAssignment`.

## <a name="fluent-api-alternative-to-attributes"></a>Alternatywny interfejs API Fluent dla atrybutów

Metoda `OnModelCreating` w poprzednim kodzie używa *interfejsu API Fluent* do konfigurowania zachowania EF Core. Interfejs API jest nazywany "Fluent", ponieważ jest często używany przez ciąg serii wywołań metod w jednej instrukcji. [Poniższy kod](/ef/core/modeling/#use-fluent-api-to-configure-a-model) jest przykładem interfejsu API Fluent:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

W tym samouczku interfejs API Fluent jest używany tylko na potrzeby mapowania bazy danych, której nie można wykonać przy użyciu atrybutów. Jednak interfejs API Fluent może określić większość reguł formatowania, walidacji i mapowania, które mogą być wykonywane przy użyciu atrybutów.

Niektóre atrybuty, takie jak `MinimumLength`, nie mogą być stosowane z interfejsem API Fluent. `MinimumLength` nie zmienia schematu, stosuje tylko regułę walidacji o minimalnej długości.

Niektórzy deweloperzy wolą korzystać z interfejsu API Fluent wyłącznie w taki sposób, aby mogli utrzymać czyste klasy jednostek. Atrybuty i interfejs API Fluent mogą być mieszane. Istnieją pewne konfiguracje, które można wykonać tylko przy użyciu interfejsu API Fluent (określenie złożonego klucza podstawowego). Istnieją pewne konfiguracje, które można wykonać tylko przy użyciu atrybutów (`MinimumLength`). Zalecane rozwiązanie dotyczące korzystania z interfejsu API Fluent lub atrybutów:

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

* Kilka linii relacji jeden-do-wielu (od 1 do \*).
* Linia relacji "jeden do zera" (od 1 do 0.. 1) między jednostkami `Instructor` i `OfficeAssignment`.
* Linia relacji zero-lub-jeden-do-wielu (0.. 1 do *) między jednostkami `Instructor` i `Department`.

## <a name="seed-the-database"></a>Inicjowanie bazy danych

Zaktualizuj kod w *danych/Dbinitializeer. cs*:

[!code-csharp[](intro/samples/cu30/Data/DbInitializer.cs)]

Poprzedni kod zawiera dane inicjatora dla nowych jednostek. Większość tego kodu tworzy nowe obiekty Entity i ładuje przykładowe dane. Przykładowe dane służą do testowania. Zobacz `Enrollments` i `CourseAssignments`, aby zapoznać się z przykładami tabel sprzężenia wiele-do-wielu.

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

Jeśli polecenie `database update` zostanie uruchomione, zostanie utworzony następujący błąd:

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

W następnej sekcji opisano, co należy zrobić, aby uzyskać informacje o tym błędzie.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

W przypadku dodania migracji i uruchomienia polecenia `database update` zostanie utworzony następujący błąd:

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

  ```dotnetcli
  dotnet ef database drop --force
  ```

* Usuń folder *migrations* , a następnie uruchom następujące polecenie:

  ```dotnetcli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

---

Uruchom aplikację. Uruchomienie aplikacji uruchamia metodę `DbInitializer.Initialize`. `DbInitializer.Initialize` wypełnia nową bazę danych.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Otwórz bazę danych w programie SSOX:

* Jeśli SSOX został wcześniej otwarty, kliknij przycisk **Odśwież** .
* Rozwiń węzeł **tabele** . Zostaną wyświetlone utworzone tabele.

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

Poprzedni kod dodaje `DepartmentID` obcy niedopuszczający wartości null do tabeli `Course`. Baza danych z poprzedniego samouczka zawiera wiersze w `Course`, w związku z czym nie można zaktualizować tabeli za pomocą migracji.

Aby migracja `ComplexDataModel` działała z istniejącymi danymi:

* Zmień kod, aby nadać nowej kolumnie (`DepartmentID`) wartość domyślną.
* Utwórz fałszywy Departament o nazwie "Temp", który ma pełnić rolę działu domyślnego.

#### <a name="fix-the-foreign-key-constraints"></a>Popraw ograniczenia klucza obcego

W klasie migracji `ComplexDataModel` zaktualizuj metodę `Up`:

* Otwórz plik *{timestamp} _ComplexDataModel. cs* .
* Dodaj komentarz do wiersza kodu, który dodaje kolumnę `DepartmentID` do tabeli `Course`.

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

Dodaj następujący wyróżniony kod. Nowy kod przechodzi po bloku `.CreateTable( name: "Department"`:

[!code-csharp[](intro/samples/cu30snapshots/5-complex/Migrations/ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=23-31)]

Po powyższym zmianach istniejące `Course` wiersze będą powiązane z działem "Temp" po uruchomieniu metody `ComplexDataModel.Up`.

Sposób obsługi pokazanej tutaj sytuacji jest uproszczony dla tego samouczka. Aplikacja produkcyjna:

* Dołącz kod lub skrypty, aby dodać `Department` wiersze i powiązane wiersze `Course` do nowych wierszy `Department`.
* Nie należy używać działu "Temp" ani wartości domyślnej dla `Course.DepartmentID`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W **konsoli Menedżera pakietów** (PMC) Uruchom następujące polecenie:

  ```powershell
  Update-Database
  ```

Ponieważ metoda `DbInitializer.Initialize` została zaprojektowana tak, aby działała tylko z pustą bazą danych, należy usunąć wszystkie wiersze z tabeli uczniów i kursów za pomocą SSOX. (Usuwanie kaskadowe zajmie tabelę rejestracji).

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Jeśli używasz SQL Server LocalDB z Visual Studio Code, uruchom następujące polecenie:

  ```dotnetcli
  dotnet ef database update
  ```

---

Uruchom aplikację. Uruchomienie aplikacji uruchamia metodę `DbInitializer.Initialize`. `DbInitializer.Initialize` wypełnia nową bazę danych.

## <a name="next-steps"></a>Następne kroki

W następnych dwóch samouczkach pokazano, jak odczytywać i aktualizować powiązane dane.

> [!div class="step-by-step"]
> [Poprzedni samouczek](xref:data/ef-rp/migrations)
> [następnego samouczka](xref:data/ef-rp/read-related-data)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Poprzednie samouczki pracowały z podstawowym modelem danych, który składa się z trzech jednostek. W tym samouczku:

* Dodano więcej jednostek i relacji.
* Model danych jest dostosowywany przez określenie formatowania, walidacji i reguł mapowania bazy danych.

Klasy jednostek dla kompletnego modelu danych przedstawiono na poniższej ilustracji:

![Diagram jednostek](complex-data-model/_static/diagram.png)

Jeśli wystąpią problemy, których nie można rozwiązać, Pobierz [ukończoną aplikację](
https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).

## <a name="customize-the-data-model-with-attributes"></a>Dostosowywanie modelu danych przy użyciu atrybutów

W tej sekcji model danych jest dostosowany przy użyciu atrybutów.

### <a name="the-datatype-attribute"></a>Atrybut DataType

Na stronach ucznia jest obecnie wyświetlana godzina daty rejestracji. Zwykle pola Date zawierają tylko datę, a nie czas.

Aktualizuj *modele/uczniów. cs* przy użyciu następującego wyróżnionego kodu:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

Atrybut [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) określa typ danych, który jest bardziej szczegółowy niż typ wewnętrzny bazy danych. W tym przypadku należy wyświetlić tylko datę, a nie datę i godzinę. [Wyliczenie DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) zawiera wiele typów danych, takich jak data, godzina, numer telefonu, waluta, EmailAddress itp. Atrybut `DataType` może również umożliwić aplikacji automatyczne udostępnianie funkcji specyficznych dla typu. Na przykład:

* `mailto:` link jest tworzony automatycznie dla `DataType.EmailAddress`.
* Selektor daty jest dostępny dla `DataType.Date` w większości przeglądarek.

Atrybut `DataType` emituje `data-` HTML 5 (wymawiane kreski danych), których używają przeglądarki HTML 5. Atrybuty `DataType` nie zapewniają walidacji.

`DataType.Date` nie określa formatu wyświetlanej daty. Domyślnie pole Date jest wyświetlane zgodnie z domyślnymi formatami opartymi na [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support)serwera.

Atrybut `DisplayFormat` jest używany do jawnego określania formatu daty:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

Ustawienie `ApplyFormatInEditMode` określa, że formatowanie powinno być również stosowane do interfejsu użytkownika edytowania. Niektóre pola nie powinny używać `ApplyFormatInEditMode`. Na przykład symbol waluty zazwyczaj nie powinien być wyświetlany w polu tekstowym Edycja.

Atrybut `DisplayFormat` może być używany przez siebie. Zwykle dobrym pomysłem jest użycie atrybutu `DataType` z atrybutem `DisplayFormat`. Atrybut `DataType` przekazuje semantykę danych w przeciwieństwie do sposobu renderowania na ekranie. Atrybut `DataType` zapewnia następujące korzyści, które nie są dostępne w `DisplayFormat`:

* Przeglądarka może włączyć funkcje HTML5. Na przykład Pokaż kontrolkę kalendarz, odpowiedni dla ustawień regionalnych symbol waluty, linki poczty e-mail, sprawdzanie poprawności danych po stronie klienta itd.
* Domyślnie przeglądarka renderuje dane przy użyciu poprawnego formatu na podstawie ustawień regionalnych.

Aby uzyskać więcej informacji, zobacz [dokumentację pomocnika tagów\<input >](xref:mvc/views/working-with-forms#the-input-tag-helper).

Uruchom aplikację. Przejdź do strony indeksu uczniów. Czasy nie są już wyświetlane. Każdy widok, który używa modelu `Student` wyświetla datę bez czasu.

![Strona indeksu uczniów pokazująca daty bez czasu](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>Atrybut StringLength

Można określić reguły walidacji danych i komunikaty o błędach walidacji z atrybutami. Atrybut [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) określa minimalną i maksymalną długość znaków, które są dozwolone w polu danych. Atrybut `StringLength` zapewnia również weryfikację po stronie klienta i serwera. Wartość minimalna nie ma wpływu na schemat bazy danych.

Zaktualizuj model `Student` przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

Poprzedni kod ogranicza nazwy do nie więcej niż 50 znaków. Atrybut `StringLength` nie uniemożliwia użytkownikowi wprowadzania białych znaków w nazwie. Atrybut [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) służy do stosowania ograniczeń do danych wejściowych. Na przykład poniższy kod wymaga, aby pierwszy znak był wielką literą, a pozostałe znaki są alfabetyczne:

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

Na powyższym obrazie przedstawiono schemat tabeli `Student`. Pola nazw mają `nvarchar(MAX)` typu, ponieważ nie uruchomiono migracji w bazie danych. Po uruchomieniu migracji w dalszej części tego samouczka pola nazwy stają się `nvarchar(50)`.

### <a name="the-column-attribute"></a>Atrybut Column

Atrybuty mogą kontrolować sposób mapowania klas i właściwości do bazy danych. W tej sekcji atrybut `Column` jest używany do mapowania nazwy właściwości `FirstMidName` na "FirstName" w bazie danych.

Po utworzeniu bazy danych nazwy właściwości w modelu są używane dla nazw kolumn (z wyjątkiem sytuacji, gdy atrybut `Column` jest używany).

Model `Student` używa `FirstMidName` dla pola pierwszej nazwy, ponieważ pole może zawierać również nazwę środkową.

Zaktualizuj plik *student.cs* za pomocą następującego wyróżnionego kodu:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

Po powyższej zmianie `Student.FirstMidName` w aplikacji jest mapowany do kolumny `FirstName` tabeli `Student`.

Dodanie atrybutu `Column` powoduje zmianę modelu z kopią zapasową `SchoolContext`. Model wykonujący kopię zapasową `SchoolContext` nie jest już zgodny z bazą danych. Jeśli aplikacja jest uruchamiana przed zastosowaniem migracji, generowany jest następujący wyjątek:

```
SqlException: Invalid column name 'FirstName'.
```

Aby zaktualizować bazę danych:

* Skompiluj projekt.
* Otwórz okno polecenia w folderze projektu. Wprowadź następujące polecenia, aby utworzyć nową migrację i zaktualizować bazę danych:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```powershell
Add-Migration ColumnFirstName
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```dotnetcli
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

---

Polecenie `migrations add ColumnFirstName` generuje następujący komunikat ostrzegawczy:

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

Ostrzeżenie jest generowane, ponieważ pola nazw są teraz ograniczone do 50 znaków. Jeśli nazwa w bazie danych ma więcej niż 50 znaków, zostanie utracony od 51 do ostatniego znaku.

* Testowanie aplikacji.

Otwórz tabelę uczniów w SSOX:

![Tabela studentów w SSOX po migracji](complex-data-model/_static/ssox-after-migration.png)

Przed zainstalowaniem migracji kolumny nazw były typu [nvarchar (max)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql). Kolumny nazw są teraz `nvarchar(50)`. Nazwa kolumny została zmieniona z `FirstMidName` na `FirstName`.

> [!Note]
> W poniższej sekcji Kompilowanie aplikacji na niektórych etapach powoduje wygenerowanie błędów kompilatora. Instrukcje określają, kiedy należy skompilować aplikację.

## <a name="student-entity-update"></a>Aktualizacja jednostki ucznia

![Jednostka ucznia](complex-data-model/_static/student-entity.png)

Aktualizuj *modele/uczniów. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Wymagany atrybut

Atrybut `Required` powoduje, że pola nazwy są wymagane. Atrybut `Required` nie jest wymagany dla typów niedopuszczających wartości null, takich jak typy wartości (`DateTime`, `int`, `double`itp.). Typy, które nie mogą mieć wartości null, są automatycznie traktowane jako pola wymagane.

Atrybut `Required` może zostać zastąpiony parametrem minimalnej długości w atrybucie `StringLength`:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Atrybut wyświetlania

Atrybut `Display` określa, że podpis pól tekstowych powinien mieć wartość "imię i nazwisko", "nazwisko", "imię i nazwisko" oraz "Data rejestracji". Domyślne podpisy nie zawierają spacji dzielących wyrazy, na przykład "LastName".

### <a name="the-fullname-calculated-property"></a>Właściwość obliczeniowa FullName

`FullName` jest właściwością obliczaną, która zwraca wartość utworzoną przez połączenie dwóch innych właściwości. nie można ustawić `FullName`, ma tylko metodę dostępu get. Nie utworzono kolumny `FullName` w bazie danych.

## <a name="create-the-instructor-entity"></a>Tworzenie jednostki instruktora

![Jednostka instruktora](complex-data-model/_static/instructor-entity.png)

Utwórz *modele/instruktor. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

Wiele atrybutów może znajdować się w jednym wierszu. Atrybuty `HireDate` mogą być zapisywane w następujący sposób:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>Właściwości nawigacji CourseAssignments i OfficeAssignment

Właściwości `CourseAssignments` i `OfficeAssignment` są właściwościami nawigacji.

Instruktor może uczyć się dowolnej liczby kursów, dlatego `CourseAssignments` jest zdefiniowany jako kolekcja.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Jeśli właściwość nawigacji zawiera wiele jednostek:

* Musi to być typ listy, w którym można dodawać, usuwać i aktualizować wpisy.

Typy właściwości nawigacji obejmują:

* `ICollection<T>`
* `List<T>`
* `HashSet<T>`

Jeśli `ICollection<T>` jest określony, EF Core domyślnie tworzy kolekcję `HashSet<T>`.

Jednostka `CourseAssignment` została omówiona w sekcji dotyczącej relacji wiele-do-wielu.

Firma Contoso University Rules States, że instruktor może mieć co najwyżej jednego biura. Właściwość `OfficeAssignment` zawiera jedną jednostkę `OfficeAssignment`. `OfficeAssignment` ma wartość null, jeśli nie przypisano pakietu Office.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Tworzenie jednostki OfficeAssignment

![Jednostka OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

Utwórz *modele/OfficeAssignment. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Atrybut klucza

Atrybut `[Key]` jest używany do identyfikowania właściwości jako klucza podstawowego (PK), gdy nazwa właściwości jest inna niż classnameID lub ID.

Istnieje relacja jeden do zera między jednostkami `Instructor` i `OfficeAssignment`. Przypisanie pakietu Office istnieje tylko w odniesieniu do instruktora, do którego jest przypisane. `OfficeAssignment` klucz podstawowy jest również jego kluczem obcym (FK) do jednostki `Instructor`. EF Core nie może automatycznie rozpoznać `InstructorID` jako klucz podstawowy `OfficeAssignment` ponieważ:

* `InstructorID` nie jest zgodna z konwencją nazewnictwa ID lub classnameID.

W związku z tym, atrybut `Key` jest używany do identyfikowania `InstructorID` jako klucz podstawowy:

```csharp
[Key]
public int InstructorID { get; set; }
```

Domyślnie EF Core traktuje klucz jako wygenerowane poza bazą danych, ponieważ kolumna służy do identyfikowania relacji.

### <a name="the-instructor-navigation-property"></a>Właściwość nawigacji instruktora

Właściwość nawigacji `OfficeAssignment` dla jednostki `Instructor` ma wartość null, ponieważ:

* Typy odwołań (takie jak klasy są dopuszczające wartość null).
* Instruktor może nie mieć przypisania pakietu Office.

Jednostka `OfficeAssignment` ma właściwość nawigacji `Instructor` niedopuszczające wartości null, ponieważ:

* `InstructorID` nie dopuszcza wartości null.
* Przypisanie pakietu Office nie może istnieć bez instruktora.

Gdy jednostka `Instructor` ma pokrewną jednostkę `OfficeAssignment`, każda jednostka ma odwołanie do drugiej z nich we właściwości nawigacji.

Atrybut `[Required]` można zastosować do właściwości nawigacji `Instructor`:

```csharp
[Required]
public Instructor Instructor { get; set; }
```

Poprzedni kod określa, że musi być pokrewnym instruktorem. Poprzedzający kod jest niezbędny, ponieważ `InstructorID` klucz obcy (który jest również kluczem PK) nie dopuszcza wartości null.

## <a name="modify-the-course-entity"></a>Modyfikowanie jednostki kursu

![Jednostka kursu](complex-data-model/_static/course-entity.png)

Aktualizuj *modele/kurs. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

Jednostka `Course` ma `DepartmentID`właściwość klucza obcego (obcy). `DepartmentID` punkty do powiązanej jednostki `Department`. Jednostka `Course` ma `Department` właściwość nawigacji.

EF Core nie wymaga właściwości FK dla modelu danych, gdy model ma właściwość nawigacji dla powiązanej jednostki.

EF Core automatycznie tworzy FKs w bazie danych wszędzie tam, gdzie są one zbędne. EF Core tworzy [Właściwości cienia](/ef/core/modeling/shadow-properties) dla automatycznie utworzonych FKs. Posiadanie klucza obcego w modelu danych może sprawiać, że aktualizacje są prostsze i wydajniejsze. Rozważmy na przykład model, w którym Właściwość FK `DepartmentID` *nie* jest uwzględniona. Gdy jednostka kursu jest pobierana do edycji:

* Jednostka `Department` ma wartość null, jeśli nie została jawnie załadowana.
* Aby zaktualizować jednostkę kursu, najpierw należy pobrać jednostkę `Department`.

Gdy właściwość FK `DepartmentID` jest uwzględniona w modelu danych, nie ma potrzeby pobierania jednostki `Department` przed aktualizacją.

### <a name="the-databasegenerated-attribute"></a>Atrybut DatabaseGenerated

Atrybut `[DatabaseGenerated(DatabaseGeneratedOption.None)]` określa, że klucz podstawowy jest dostarczany przez aplikację, a nie generowany przez bazę danych.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Domyślnie EF Core zakłada, że wartości klucza PK są generowane przez bazę danych. Wartość klucza podstawowego wygenerowanego przez bazę danych jest ogólnie najlepszym rozwiązaniem. W przypadku jednostek `Course` użytkownik określa klucz podstawowy. Na przykład numer kursu, taki jak seria 1000 dla działu Math, serii 2000 dla działu angielskiego.

Atrybut `DatabaseGenerated` może być również używany do generowania wartości domyślnych. Na przykład baza danych może automatycznie wygenerować pole daty, aby zarejestrować datę utworzenia lub zaktualizowania wiersza. Aby uzyskać więcej informacji, zobacz [wygenerowane właściwości](/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Właściwości klucza obcego i nawigacji

Właściwości klucza obcego (FK) i właściwości nawigacji w jednostce `Course` odzwierciedlają następujące relacje:

Kurs jest przypisywany do jednego działu, dlatego istnieje `DepartmentID` FK i `Department` właściwość nawigacji.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

W kursie może być zarejestrowana dowolna liczba uczniów, więc właściwość nawigacji `Enrollments` jest kolekcją:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Kurs może być nauczany przez wiele instruktorów, więc właściwość nawigacji `CourseAssignments` jest kolekcją:

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment` został wyjaśniony [później](#many-to-many-relationships).

## <a name="create-the-department-entity"></a>Tworzenie jednostki działu

![Jednostka działu](complex-data-model/_static/department-entity.png)

Utwórz *modele/dział. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Atrybut Column

Wcześniej atrybut `Column` został użyty do zmiany mapowania nazw kolumn. W kodzie dla jednostki `Department` atrybut `Column` służy do zmiany mapowania typu danych SQL. Kolumna `Budget` jest definiowana przy użyciu SQL Server typu pieniędzy w bazie danych:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Mapowanie kolumn zazwyczaj nie jest wymagane. EF Core zwykle wybiera odpowiedni SQL Server typ danych oparty na typie CLR właściwości. Typ `decimal` CLR mapuje do typu `decimal` SQL Server. `Budget` jest dla waluty, a typ danych walutowych jest bardziej odpowiedni dla waluty.

### <a name="foreign-key-and-navigation-properties"></a>Właściwości klucza obcego i nawigacji

Właściwości FK i nawigacji odzwierciedlają następujące relacje:

* Dział może lub nie ma uprawnienia administratora.
* Administrator jest zawsze instruktorem. W związku z tym Właściwość `InstructorID` jest dołączana jako obcy do jednostki `Instructor`.

Właściwość nawigacji ma nazwę `Administrator`, ale zawiera jednostkę `Instructor`:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Znak zapytania (?) w powyższym kodzie określa właściwość dopuszcza wartość null.

Dział może mieć wiele kursów, więc istnieje właściwość nawigacji kursów:

```csharp
public ICollection<Course> Courses { get; set; }
```

Uwaga: według Konwencji EF Core umożliwia usuwanie kaskadowe dla FKs niedopuszczających wartości null oraz relacji wiele-do-wielu. Kaskadowe usuwanie może spowodować cykliczne reguły usuwania kaskadowego. Cykliczne reguły usuwania kaskadowego powodują wyjątek podczas dodawania migracji.

Na przykład, jeśli właściwość `Department.InstructorID` została zdefiniowana jako niedopuszczający wartości null:

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

Rekord rejestracji jest przeznaczony dla jednego kursu, dlatego istnieje właściwość `CourseID` obcego i właściwość nawigacji `Course`:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Rekord rejestracji jest przeznaczony dla jednego ucznia, dlatego istnieje właściwość `StudentID` obcego i właściwość nawigacji `Student`:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relacje wiele do wielu

Istnieje relacja wiele-do-wielu między jednostkami `Student` i `Course`. Jednostka `Enrollment` działa jako tabela sprzężeń wiele-do-wielu *z ładunkiem* w bazie danych. "Z ładunkiem" oznacza, że tabela `Enrollment` zawiera dodatkowe dane oprócz FKs dla sprzężonych tabel (w tym przypadku klucz podstawowy i `Grade`).

Na poniższej ilustracji pokazano, jak wyglądają te relacje w diagramie jednostek. (Ten diagram został wygenerowany przy użyciu [narzędzi EF PowerShell](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) dla programu EF 6. x. Tworzenie diagramu nie jest częścią samouczka.

![Student-kurs wiele do wielu relacji](complex-data-model/_static/student-course.png)

Każda linia relacji ma 1 na jednym końcu i gwiazdkę (*) na drugim, wskazując relację jeden do wielu.

Jeśli tabela `Enrollment` nie zawiera informacji o klasie, musi zawierać tylko dwa FKs (`CourseID` i `StudentID`). Tabela sprzężenia wiele-do-wielu bez ładunku jest czasami nazywana tabelą sprzężenia czystego (PJT).

Jednostki `Instructor` i `Course` mają relację wiele-do-wielu przy użyciu czystej tabeli sprzężenia.

Uwaga: w odniesieniu do relacji wiele-do-wielu są obsługiwane niejawne tabele sprzężenia w programie Dr 6. x, ale nie EF Core. Aby uzyskać więcej informacji, zobacz [relacje wiele-do-wielu w EF Core 2,0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).

## <a name="the-courseassignment-entity"></a>Jednostka CourseAssignment

![Jednostka CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

Utwórz *modele/CourseAssignment. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a>Instruktorzy do kursów

![M:M instruktora do kursu](complex-data-model/_static/courseassignment.png)

"Instruktora" — relacja wiele do wielu:

* Wymaga tabeli sprzężenia, która musi być reprezentowana przez zestaw jednostek.
* Jest czystym tabelą sprzężenia (tabela bez ładunku).

Nazwa jednostki sprzężenia jest wspólna dla `EntityName1EntityName2`. Na przykład tabela sprzężenia "instruktor do kursu" przy użyciu tego wzorca jest `CourseInstructor`. Zalecamy jednak użycie nazwy opisującej relację.

Modele danych rozpoczynają się od siebie i rosną. Sprzężenia bez ładunku (PJTs) często są rozwijane w celu uwzględnienia ładunku. Rozpoczynając od nazwy obiektu opisowego, nie trzeba zmieniać nazwy po zmianie tabeli sprzężeń. Najlepiej, aby jednostka sprzężenia miała własną nazwę naturalną (prawdopodobnie jednowyrazową) w domenie biznesowej. Na przykład książki i klienci mogą być połączone z jednostką sprzężenia o nazwie ratings. Dla instruktora "wiele do wielu", `CourseAssignment` jest preferowana przez `CourseInstructor`.

### <a name="composite-key"></a>Klucz złożony

FKs nie dopuszcza wartości null. Dwa FKs w `CourseAssignment` (`InstructorID` i `CourseID`) jednoznacznie identyfikują każdy wiersz tabeli `CourseAssignment`. `CourseAssignment` nie wymaga dedykowanego klucza podstawowego. Właściwości `InstructorID` i `CourseID` są funkcją złożonego klucza podstawowego. Jedynym sposobem określenia złożonego PKs do EF Core jest *interfejs API Fluent*. W następnej sekcji przedstawiono sposób konfigurowania złożonego klucza podstawowego.

Klucz złożony gwarantuje:

* W jednym kursie są dozwolone wiele wierszy.
* Dla jednego instruktora są dozwolone wiele wierszy.
* Wiele wierszy dla tego samego instruktora i kursu jest niedozwolonych.

Jednostka `Enrollment` Join definiuje własny klucz podstawowy, więc możliwe jest duplikowanie tego sortowania. Aby uniknąć takich duplikatów:

* Dodaj unikatowy indeks do pól klucza obcego lub
* Skonfiguruj `Enrollment` przy użyciu podstawowego klucza złożonego podobnego do `CourseAssignment`. Aby uzyskać więcej informacji, zobacz [indeksy](/ef/core/modeling/indexes).

## <a name="update-the-db-context"></a>Aktualizowanie kontekstu bazy danych

Dodaj następujący wyróżniony kod do *danych/SchoolContext. cs*:

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Poprzedni kod dodaje nowe jednostki i konfiguruje złożonego klucza podstawowego jednostki `CourseAssignment`.

## <a name="fluent-api-alternative-to-attributes"></a>Alternatywny interfejs API Fluent dla atrybutów

Metoda `OnModelCreating` w poprzednim kodzie używa *interfejsu API Fluent* do konfigurowania zachowania EF Core. Interfejs API jest nazywany "Fluent", ponieważ jest często używany przez ciąg serii wywołań metod w jednej instrukcji. [Poniższy kod](/ef/core/modeling/#use-fluent-api-to-configure-a-model) jest przykładem interfejsu API Fluent:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

W tym samouczku interfejs API Fluent jest używany tylko w przypadku mapowania bazy danych, której nie można wykonać przy użyciu atrybutów. Jednak interfejs API Fluent może określić większość reguł formatowania, walidacji i mapowania, które mogą być wykonywane przy użyciu atrybutów.

Niektóre atrybuty, takie jak `MinimumLength`, nie mogą być stosowane z interfejsem API Fluent. `MinimumLength` nie zmienia schematu, stosuje tylko regułę walidacji o minimalnej długości.

Niektórzy deweloperzy wolą korzystać z interfejsu API Fluent wyłącznie w taki sposób, aby mogli utrzymać czyste klasy jednostek. Atrybuty i interfejs API Fluent mogą być mieszane. Istnieją pewne konfiguracje, które można wykonać tylko przy użyciu interfejsu API Fluent (określenie złożonego klucza podstawowego). Istnieją pewne konfiguracje, które można wykonać tylko przy użyciu atrybutów (`MinimumLength`). Zalecane rozwiązanie dotyczące korzystania z interfejsu API Fluent lub atrybutów:

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

* Kilka linii relacji jeden-do-wielu (od 1 do \*).
* Linia relacji "jeden do zera" (od 1 do 0.. 1) między jednostkami `Instructor` i `OfficeAssignment`.
* Linia relacji zero-lub-jeden-do-wielu (0.. 1 do *) między jednostkami `Instructor` i `Department`.

## <a name="seed-the-db-with-test-data"></a>Wypełnianie bazy danych danymi testowymi

Zaktualizuj kod w *danych/Dbinitializeer. cs*:

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

Poprzedni kod zawiera dane inicjatora dla nowych jednostek. Większość tego kodu tworzy nowe obiekty Entity i ładuje przykładowe dane. Przykładowe dane służą do testowania. Zobacz `Enrollments` i `CourseAssignments`, aby zapoznać się z przykładami tabel sprzężenia wiele-do-wielu.

## <a name="add-a-migration"></a>Dodawanie migracji

Skompiluj projekt.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```powershell
Add-Migration ComplexDataModel
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```dotnetcli
dotnet ef migrations add ComplexDataModel
```

---

Poprzednie polecenie wyświetla ostrzeżenie o możliwej utracie danych.

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Jeśli polecenie `database update` zostanie uruchomione, zostanie utworzony następujący błąd:

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a>Zastosuj migrację

Teraz, gdy masz już istniejącą bazę danych, musisz się zastanowić, jak zastosować w niej przyszłe zmiany. Ten samouczek przedstawia dwa podejścia:

* [Porzuć i ponownie utwórz bazę danych](#drop)
* [Zastosuj migrację do istniejącej bazy danych](#applyexisting). Chociaż ta metoda jest bardziej złożona i czasochłonna, jest preferowanym podejściem dla rzeczywistych środowisk produkcyjnych. **Uwaga**: jest to opcjonalna sekcja samouczka. Możesz wykonać kroki porzucenia i utworzyć ponownie i pominąć tę sekcję. Jeśli chcesz wykonać kroki opisane w tej sekcji, nie wykonuj kroków usuwania i ponownego tworzenia. 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a>Porzuć i ponownie utwórz bazę danych

Kod w zaktualizowanym `DbInitializer` dodaje dane inicjatora dla nowych jednostek. Aby wymusić EF Core tworzenia nowej bazy danych, Porzuć i zaktualizuj bazę danych:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W **konsoli Menedżera pakietów** (PMC) Uruchom następujące polecenie:

```powershell
Drop-Database
Update-Database
```

Uruchom `Get-Help about_EntityFrameworkCore` z PMC, aby uzyskać informacje pomocy.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Otwórz okno polecenia i przejdź do folderu projektu. Folder projektu zawiera plik *Startup.cs* .

Wprowadź następujące polecenie w oknie polecenia:

```dotnetcli
dotnet ef database drop
dotnet ef database update
```

---

Uruchom aplikację. Uruchomienie aplikacji uruchamia metodę `DbInitializer.Initialize`. `DbInitializer.Initialize` wypełnia nową bazę danych.

Otwórz bazę danych w programie SSOX:

* Jeśli SSOX został wcześniej otwarty, kliknij przycisk **Odśwież** .
* Rozwiń węzeł **tabele** . Zostaną wyświetlone utworzone tabele.

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

Poprzedni kod dodaje `DepartmentID` obcy niedopuszczający wartości null do tabeli `Course`. Baza danych z poprzedniego samouczka zawiera wiersze w `Course`, w związku z czym nie można zaktualizować tabeli za pomocą migracji.

Aby migracja `ComplexDataModel` działała z istniejącymi danymi:

* Zmień kod, aby nadać nowej kolumnie (`DepartmentID`) wartość domyślną.
* Utwórz fałszywy Departament o nazwie "Temp", który ma pełnić rolę działu domyślnego.

#### <a name="fix-the-foreign-key-constraints"></a>Popraw ograniczenia klucza obcego

Zaktualizuj klasy `ComplexDataModel` `Up` metody:

* Otwórz plik *{timestamp} _ComplexDataModel. cs* .
* Dodaj komentarz do wiersza kodu, który dodaje kolumnę `DepartmentID` do tabeli `Course`.

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

Dodaj następujący wyróżniony kod. Nowy kod przechodzi po bloku `.CreateTable( name: "Department"`:

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

Po powyższym zmianach istniejące `Course` wiersze będą powiązane z działem "Temp" po uruchomieniu metody `ComplexDataModel` `Up`.

Aplikacja produkcyjna:

* Dołącz kod lub skrypty, aby dodać `Department` wiersze i powiązane wiersze `Course` do nowych wierszy `Department`.
* Nie należy używać działu "Temp" ani wartości domyślnej dla `Course.DepartmentID`.

Następny samouczek obejmuje powiązane dane.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja usługi YouTube w tym samouczku (część 1)](https://www.youtube.com/watch?v=0n2f0ObgCoA)
* [Wersja usługi YouTube w tym samouczku (część 2)](https://www.youtube.com/watch?v=Je0Z5K1TNmY)

> [!div class="step-by-step"]
> [Poprzednie](xref:data/ef-rp/migrations)
> [dalej](xref:data/ef-rp/read-related-data)

::: moniker-end
