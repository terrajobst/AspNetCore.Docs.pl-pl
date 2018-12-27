---
title: Strony razor z programem EF Core w programie ASP.NET Core — Model danych — 5 8
author: rick-anderson
description: W tym samouczku należy dodać większą liczbę jednostek i relacji i Dostosuj model danych, określając formatowania i sprawdzania poprawności i reguł mapowania.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 930a6f2b860c71b6f499cff53e0d909a130f7948
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637901"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a>Strony razor z programem EF Core w programie ASP.NET Core — Model danych — 5 8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

Poprzednich samouczków pracy z modelem danych podstawowych, który został składające się z trzech jednostek. W tym samouczku:

* Więcej jednostek i relacji są dodawane.
* Model danych jest dostosowane, określając formatowania i sprawdzania poprawności i reguł mapowania bazy danych.

Na poniższej ilustracji pokazano klas jednostek dla modelu danych zakończone:

![Diagram jednostek](complex-data-model/_static/diagram.png)

Jeśli napotkasz problemy, nie można rozwiązać, Pobierz [ukończonej aplikacji](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).

## <a name="customize-the-data-model-with-attributes"></a>Dostosuj model danych za pomocą atrybutów

W tej sekcji model danych został dostosowany, za pomocą atrybutów.

### <a name="the-datatype-attribute"></a>Atrybut typu danych

Na stronach dla uczniów obecnie Wyświetla czas Data rejestracji. Zazwyczaj Data polach wskaźnika myszy wyświetlane tylko datę, a nie czasu.

Aktualizacja *Models/Student.cs* przy użyciu następujących wyróżniony kod:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

[DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) atrybut określa typ danych, który jest bardziej szczegółowe niż typ wewnętrznej bazy danych. W tym przypadku tylko data powinna być wyświetlana, nie daty i godziny. [Wyliczenie typu danych](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) udostępnia wiele typów danych, takich jak daty, godziny, numer telefonu, waluty, EmailAddress itp. `DataType` Atrybut można również włączyć automatyczne udostępnianie funkcji specyficznych dla typu aplikacji. Na przykład:

* `mailto:` Łącze jest tworzona automatycznie dla `DataType.EmailAddress`.
* Selektor daty towarzyszy `DataType.Date` w większości przeglądarek.

`DataType` Atrybut emituje HTML 5 `data-` atrybutów (Wymowa: dane dash), korzystających z przeglądarki HTML 5. `DataType` Atrybutów nie zapewniają weryfikacji.

`DataType.Date` nie określa format daty, która jest wyświetlana. Domyślnie pole daty są wyświetlane domyślne formaty oparte na tym serwerze [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).

`DisplayFormat` Atrybut jest używany jawnie określić format daty:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode` Ustawienie określa, że formatowanie powinien również będą stosowane do edycji interfejsu użytkownika. Niektóre pola nie używaj `ApplyFormatInEditMode`. Na przykład symbol waluty ogólnie nie powinien być wyświetlany w polu edycji tekstu.

`DisplayFormat` Atrybut może być używany przez siebie. Zazwyczaj jest to dobry pomysł, aby użyć `DataType` atrybutem `DisplayFormat` atrybutu. `DataType` Atrybutu powoduje semantykę dane, a nie jak renderować ją na ekranie. `DataType` Atrybut zapewnia następujące korzyści, które nie są dostępne w `DisplayFormat`:

* Przeglądarka można włączyć funkcje HTML5. Na przykład pokazać kontrolki kalendarza, symbol waluty odpowiednich ustawień regionalnych, przesyłanie pocztą e-mail łączy, sprawdzania poprawności danych wejściowych po stronie klienta, itp.
* Domyślnie przeglądarka Renderowanie danych przy użyciu poprawny format, w oparciu o ustawienia regionalne.

Aby uzyskać więcej informacji, zobacz [ \<wejściowych > dokumentacja Pomocnik tagu](xref:mvc/views/working-with-forms#the-input-tag-helper).

Uruchom aplikację. Przejdź do strony indeksu studentów. Nie są wyświetlane godziny. Każdy widok, który używa `Student` modelu umożliwia wyświetlenie daty bez godziny.

![Wyświetlanie daty bez godziny strony indeksu uczniów](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>Atrybut StringLength

Reguły sprawdzania poprawności danych i komunikatów o błędach weryfikacji można określić za pomocą atrybutów. [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) atrybut określa minimalną i maksymalną długość znaków, które są dozwolone w polu danych. `StringLength` Atrybut udostępnia również weryfikacji po stronie klienta i po stronie serwera. Wartość minimalna nie ma wpływu na schemat bazy danych.

Aktualizacja `Student` modelu z następującym kodem:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

Powyższy kod ogranicza nazwy do nie więcej niż 50 znaków. `StringLength` Atrybutu nie uniemożliwia użytkownikowi wprowadzanie białe miejsca dla nazwy. [Wyrażenia regularnego](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) atrybut jest używany, aby zastosować ograniczenia danych wejściowych. Na przykład poniższy kod wymaga pierwszy znak na wielkie litery, a pozostałe znaki, aby być znakiem alfabetycznym:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

Uruchom aplikację:

* Przejdź do strony studentów.
* Wybierz **Utwórz nowy**, a następnie wprowadź nazwę więcej niż 50 znaków.
* Wybierz **Utwórz**, weryfikacji po stronie klienta zawiera komunikat o błędzie.

![Studenci indeksu strona wyświetlająca błędy długość ciągu](complex-data-model/_static/string-length-errors.png)

W **Eksplorator obiektów SQL Server** (SSOX), otwórz projektanta tabel dla uczniów, klikając dwukrotnie **uczniów** tabeli.

![Tabela studentów w SSOX przed migracji](complex-data-model/_static/ssox-before-migration.png)

Na powyższej ilustracji pokazano schematu dla `Student` tabeli. Nazwa pola, które mają typ `nvarchar(MAX)` ponieważ migracji nie został uruchomiony dla bazy danych. Podczas migracji są uruchamiane w dalszej części tego samouczka, nazwy pola stają się `nvarchar(50)`.

### <a name="the-column-attribute"></a>Atrybut kolumny

Atrybuty można kontrolować, jak klasy i właściwości są mapowane do bazy danych. W tej sekcji `Column` atrybut jest używany do mapowania nazwy `FirstMidName` właściwość na "FirstName" w bazie danych.

Po utworzeniu bazy danych, nazwy właściwości w modelu są używane dla nazw kolumn (z wyjątkiem kiedy `Column` atrybut jest używany).

`Student` Model używa `FirstMidName` nazwę pierwszego pola, ponieważ pole może także zawierać imienia.

Aktualizacja *Student.cs* pliku następujący wyróżniony kod:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Column&highlight=4,14)]

Z powyższej zmiany `Student.FirstMidName` w aplikacji mapuje `FirstName` kolumny `Student` tabeli.

Dodanie `Column` atrybut zmieni się zapasowy modelu `SchoolContext`. Zapasowy modelu `SchoolContext` nie jest już zgodny z bazy danych. Jeśli aplikacja jest uruchamiana przed zastosowaniem migracje, generowany jest następujący wyjątek:

```SQL
SqlException: Invalid column name 'FirstName'.
```

Aby zaktualizować bazy danych:

* Skompiluj projekt.
* Otwórz okno polecenia w folderze projektu. Wprowadź następujące polecenia, aby utworzyć nową migrację i aktualizowanie bazy danych:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```PMC
Add-Migration ColumnFirstName
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet ef migrations add ColumnFirstName
dotnet ef database update
```

------

`migrations add ColumnFirstName` Polecenie generuje następujący komunikat ostrzegawczy:

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

Ostrzeżenia jest generowany, ponieważ nazwa pola, które są obecnie ograniczone do 50 znaków. Jeśli nazwa bazy danych — w więcej niż 50 znaków, 51 do ostatniego znaku zostałyby utracone.

* Testowanie aplikacji.

Otwórz Tabela Student SSOX:

![Tabela studentów w SSOX po migracji](complex-data-model/_static/ssox-after-migration.png)

Przed zastosowaniem migracji, nazwa kolumny były typu [nvarchar(MAX)](/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql). Nazwa kolumny są teraz `nvarchar(50)`. Nazwa kolumny został zmieniony z `FirstMidName` do `FirstName`.

> [!Note]
> W poniższej sekcji Kompilowanie aplikacji na niektórych etapach generuje błędy kompilatora. Instrukcje Określ, kiedy do skompilowania aplikacji.

## <a name="student-entity-update"></a>Aktualizacja jednostki dla uczniów

![Jednostki dla uczniów](complex-data-model/_static/student-entity.png)

Aktualizacja *Models/Student.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Wymagany atrybut

`Required` Atrybutu sprawia, że nazwa właściwości wymagane pola. `Required` Atrybut nie jest wymagane dla typów innych niż null, takich jak typy wartości (`DateTime`, `int`, `double`itp.). Typy, które nie może mieć wartości null są automatycznie traktowane jako wymagane pola.

`Required` Atrybut może zostać zastąpione minimalną długość parametru w `StringLength` atrybutu:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Atrybut wyświetlania

`Display` Atrybut określa, że podpis dla pól tekstowych powinien być "Imię", "Last Name", "Pełna nazwa" i "Data rejestracji". Podpisy domyślna ma już miejsca dzielenia wyrazów, na przykład "Lastname".

### <a name="the-fullname-calculated-property"></a>Właściwości obliczane imię i nazwisko

`FullName` jest właściwością obliczeniową, która zwraca wartość, która jest tworzona przez dołączenie dwóch innych właściwości. `FullName` Nie można ustawić, ma tylko akcesor pobierania. Nie `FullName` kolumna jest tworzona w bazie danych.

## <a name="create-the-instructor-entity"></a>Tworzenie jednostki przez instruktorów

![Jednostki przez instruktorów](complex-data-model/_static/instructor-entity.png)

Tworzenie *Models/Instructor.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Models/Instructor.cs)]

Wiele atrybutów może być w jednym wierszu. `HireDate` Atrybuty, które mogłyby być zapisywane w następujący sposób:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>Właściwości nawigacji CourseAssignments i OfficeAssignment

`CourseAssignments` i `OfficeAssignment` właściwości są właściwości nawigacji.

Pod kierunkiem instruktora nauczyć dowolnej liczby kursów, więc `CourseAssignments` jest zdefiniowany jako kolekcja.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Jeśli właściwość nawigacji posiada wiele jednostek:

* Musi być typu listy, gdzie wpisy mogą być dodawane, usuwane lub zaktualizować.

Typy właściwości nawigacji:

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

Jeśli `ICollection<T>` jest określona, tworzy programu EF Core `HashSet<T>` kolekcji domyślnie.

`CourseAssignment` Jednostki zostało wyjaśnione w sekcji w relacji wiele do wielu.

Firmy Contoso University reguł stanu, że pod kierunkiem instruktora może mieć co najwyżej jednego pakietu office. `OfficeAssignment` Właściwość przechowuje pojedynczej `OfficeAssignment` jednostki. `OfficeAssignment` ma wartość null, jeśli nie przypisano żadnych pakietu office.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Tworzenie jednostki OfficeAssignment

![OfficeAssignment jednostki](complex-data-model/_static/officeassignment-entity.png)

Tworzenie *Models/OfficeAssignment.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Atrybut klucza

`[Key]` Atrybutu służy do identyfikowania właściwość jako kluczowi podstawowemu (PK) gdy nazwa właściwości jest coś innego niż classnameID lub identyfikatora organizacji.

Brak relacji jeden do zero lub jeden między `Instructor` i `OfficeAssignment` jednostek. Biuro istnieje tylko w odniesieniu do przez instruktorów, do którego jest przypisany. `OfficeAssignment` Klucz podstawowy jest również jej klucz obcy (klucz OBCY) `Instructor` jednostki. EF Core automatycznie nie może rozpoznać `InstructorID` jako klucz podstawowy z `OfficeAssignment` ponieważ:

* `InstructorID` nie należy wykonać identyfikator lub classnameID konwencji nazewnictwa.

W związku z tym `Key` atrybut jest używany do identyfikowania `InstructorID` jako klucz podstawowy:

```csharp
[Key]
public int InstructorID { get; set; }
```

Domyślnie EF Core traktuje klucza jako inne niż wygenerowane bazy danych, ponieważ kolumna jest przeznaczona dla relacji identyfikującej.

### <a name="the-instructor-navigation-property"></a>Właściwość nawigacji przez instruktorów

`OfficeAssignment` Właściwości nawigacji dla `Instructor` jednostki ma wartość null ponieważ:

* Typów referencyjnych (takie jak klasy dopuszczają wartości null).
* Pod kierunkiem instruktora, może nie mieć biuro.


`OfficeAssignment` Jednostka ma wartość null `Instructor` właściwość nawigacji ponieważ:

* `InstructorID` nie dopuszcza wartości.
* Przypisanie pakietu office nie może istnieć bez pod kierunkiem instruktora.

Gdy `Instructor` jednostka ma powiązane `OfficeAssignment` jednostki, każdy obiekt odwołuje się do innych niż w jego właściwości nawigacji.

`[Required]` Można zastosować atrybutu do `Instructor` właściwość nawigacji:

```csharp
[Required]
public Instructor Instructor { get; set; }
```

Powyższy kod określa, że musi być powiązany instruktora. Powyższy kod nie jest konieczne ponieważ `InstructorID` nie dopuszczają wartości klucza obcego (jest to również PK).

## <a name="modify-the-course-entity"></a>Modyfikowanie jednostek kursu

![Kurs jednostki](complex-data-model/_static/course-entity.png)

Aktualizacja *Models/Course.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

`Course` Jednostka ma właściwość klucza obcego (klucz OBCY) `DepartmentID`. `DepartmentID` Wskazuje powiązane `Department` jednostki. `Course` Jednostka ma `Department` właściwości nawigacji.

EF Core nie wymaga właściwości klucza Obcego dla modelu danych, gdy model nie ma właściwości nawigacji dla powiązanej jednostki.

EF Core automatycznie tworzy FKs w bazie danych wszędzie tam, gdzie będą one potrzebne. Tworzy programu EF Core [w tle właściwości](/ef/core/modeling/shadow-properties) dla FKs utworzone automatycznie. Posiadanie klucza Obcego w modelu danych, że aktualizacje są prostszy i efektywniejszy. Na przykład, należy wziąć pod uwagę modelu gdzie właściwości klucza Obcego `DepartmentID` jest *nie* uwzględnione. Gdy jednostka kurs jest pobierana do edycji:

* `Department` Jednostki ma wartość null, jeśli nie zostały jawnie jest ładowany.
* Do zaktualizowania jednostki kurs `Department` należy najpierw pobrać jednostki.

Gdy właściwość klucza Obcego `DepartmentID` znajduje się w modelu danych nie ma potrzeby można pobrać `Department` jednostki przed aktualizacją.

### <a name="the-databasegenerated-attribute"></a>Atrybut DatabaseGenerated

`[DatabaseGenerated(DatabaseGeneratedOption.None)]` Atrybut określa, że klucz podstawowy jest udostępniany przez aplikację, a nie generowane przez bazę danych.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Domyślnie EF Core przyjęto założenie, że wartości klucza produktu są generowane przez bazy danych. DB wygenerowany klucz podstawowy wartościami ogólnie jest najlepszym rozwiązaniem. Aby uzyskać `Course` jednostek, użytkownik określa PK. Na przykład liczba kurs takich jak seria 1000 dla działu matematycznych, serię 2000 dla angielskiego działu.

`DatabaseGenerated` Atrybut może służyć także do generowania wartości domyślne. Na przykład baza danych może automatycznie generować pole daty, aby zarejestrować Data wiersz został utworzony lub zaktualizowany. Aby uzyskać więcej informacji, zobacz [wygenerowanych właściwości](/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Obcy właściwości klucza i nawigacja

Właściwości klucza obcego (klucz OBCY) i właściwości nawigacji w `Course` jednostki odzwierciedlają się następująco:

Kurs jest przypisany do jednego działu, więc ma `DepartmentID` klucza Obcego i `Department` właściwości nawigacji.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Kurs może mieć dowolną liczbę uczniów zarejestrowane, więc `Enrollments` właściwość nawigacji jest kolekcją:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Kurs może być prowadzone przez instruktorów wielu, więc `CourseAssignments` właściwość nawigacji jest kolekcją:

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment` objaśniono [później](#many-to-many-relationships).

## <a name="create-the-department-entity"></a>Tworzenie jednostki działu

![Dział jednostki](complex-data-model/_static/department-entity.png)

Tworzenie *Models/Department.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Atrybut kolumny

Wcześniej `Column` użyto atrybutu można zmienić mapowania nazwy kolumny. W kodzie dla `Department` jednostki `Column` atrybut jest używany, aby zmienić mapowanie typu danych SQL. `Budget` Kolumna jest definiowana za pomocą typu pieniądze programu SQL Server w bazie danych:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Mapowanie kolumn zazwyczaj nie jest wymagana. EF Core zazwyczaj wybiera się odpowiedni typ danych programu SQL Server, na podstawie typu CLR dla właściwości. Środowisko CLR `decimal` typu map do programu SQL Server `decimal` typu. `Budget` dotyczy waluty, a typ danych pieniądze są bardziej odpowiednie dla waluty.

### <a name="foreign-key-and-navigation-properties"></a>Obcy właściwości klucza i nawigacja

Właściwości klucza Obcego i nawigacyjny odzwierciedla następujące relacje:

* Dział mogą być lub może nie mieć uprawnienia administratora.
* Administrator jest zawsze pod kierunkiem instruktora. W związku z tym `InstructorID` właściwość jest uwzględniona jako klucza Obcego do `Instructor` jednostki.

Właściwość nawigacji jest o nazwie `Administrator` , ale przechowuje `Instructor` jednostki:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Znak zapytania (?) w poprzednim kodzie Określa, że właściwość ma wartość null.

Dział może mieć wielu kursów, więc ma właściwości nawigacji kursy:

```csharp
public ICollection<Course> Courses { get; set; }
```

Uwaga: Zgodnie z Konwencją programu EF Core umożliwia usuwanie kaskadowe dopuszcza FKs oraz relacji wiele do wielu. Usuwanie kaskadowe może spowodować cykliczne cascade delete reguły. Cykliczne usuwanie kaskadowe reguły powoduje, że wyjątek po dodaniu migracji.

Na przykład jeśli `Department.InstructorID` właściwości nie został zdefiniowany jako dopuszczający wartość null:

* EF Core konfiguruje regułę usuwanie kaskadowe można usunąć instruktora, po usunięciu działu.
* Usuwanie instruktora, po usunięciu działu nie jest to oczekiwane zachowanie.

W razie potrzeby reguły biznesowe `InstructorID` właściwości nie dopuszcza, użyj następującej instrukcji fluent API:

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

Powyższy kod wyłącza usuwanie kaskadowe relacji przez instruktorów działu.

## <a name="update-the-enrollment-entity"></a>Aktualizacja jednostki rejestracji

Rekord rejestracji dotyczy jednego kursu podjęte przez jeden dla uczniów.

![Jednostki rejestracji](complex-data-model/_static/enrollment-entity.png)

Aktualizacja *Models/Enrollment.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Obcy właściwości klucza i nawigacja

Właściwości klucza Obcego i właściwości nawigacji odzwierciedla się następująco:

Rekord rejestracji dotyczy jednego kursu, więc ma `CourseID` właściwości klucza Obcego i `Course` właściwość nawigacji:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Rekord rejestracji jest dla jednego uczniów lub studentów, więc ma `StudentID` właściwości klucza Obcego i `Student` właściwość nawigacji:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relacje wiele do wielu

Istnieje relacja wiele do wielu między `Student` i `Course` jednostek. `Enrollment` Jednostki działa jako tabelę sprzężenia wiele do wielu *z ładunkiem* w bazie danych. "Z ładunkiem" oznacza, że `Enrollment` tabela zawiera dane dodatkowe, oprócz FKs dla połączonych tabel (w tym przypadku PK i `Grade`).

Poniższa ilustracja przedstawia, jak wyglądają te relacje w diagramie jednostki. (Ten diagram został wygenerowany za pomocą [EF Power Tools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) na platformie EF 6.x. Tworzenie diagramów nie jest część samouczka).

![Kurs dla uczniów wiele do wielu relacji](complex-data-model/_static/student-course.png)

Każdy wiersz relacji ma 1 na jednym końcu i znak gwiazdki (*) na drugim, wskazując relacji jeden do wielu.

Jeśli `Enrollment` tabeli nie włączono informacji klasy korporacyjnej, czy tylko musi on zawierać dwa FKs (`CourseID` i `StudentID`). Tabelę sprzężenia wiele do wielu, bez ładunku jest czasami nazywane tabelę sprzężenia czystego (PJT).

`Instructor` i `Course` jednostki mają relacji wiele do wielu, przy użyciu czystego sprzężenie tabeli.

Uwaga: Niejawne sprzężenie tabel dla relacji wiele do wielu, ale programu EF Core nie obsługuje 6.x EF. Aby uzyskać więcej informacji, zobacz [wiele do wielu relacji w programie EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).

## <a name="the-courseassignment-entity"></a>Jednostka CourseAssignment

![CourseAssignment jednostki](complex-data-model/_static/courseassignment-entity.png)

Tworzenie *Models/CourseAssignment.cs* następującym kodem:

[!code-csharp[](intro/samples/cu21/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a>Przez instruktorów do kursów

![M:M przez instruktorów do kursów](complex-data-model/_static/courseassignment.png)

Relacja wiele do wielu przez instruktorów na kursy:

* Wymaga tabelę sprzężenia, która musi być reprezentowana przez zestaw jednostek.
* Jest czysty sprzężenie tabeli (tabeli bez ładunku).

Często jest nazwa jednostki sprzężenia `EntityName1EntityName2`. Na przykład tabela sprzężenia instruktora do kursów, za pomocą tego wzorca jest `CourseInstructor`. Firma Microsoft zaleca jednak przy użyciu nazwy, która opisuje relację.

Modele danych zaczynają prosty i rozwój. Ładunek nie sprzężenia (PJTs) często ewolucji obejmujący ładunku. Począwszy od jednostki opisową nazwę, nazwę nie trzeba zmienić w przypadku zmiany tabeli sprzężenia. W idealnym przypadku jednostki sprzężenia będzie mieć własną fizyczną nazwę (prawdopodobnie jednowyrazowego) w domenie firmy. Na przykład książek i klientów można połączyć z jednostką sprzężenia o nazwie klasyfikacji. Dla relacji wiele do wielu instruktora do kursów `CourseAssignment` jest preferowana nad `CourseInstructor`.

### <a name="composite-key"></a>Klucz złożony

Nie dopuszczają FKs. Dwa FKs w `CourseAssignment` (`InstructorID` i `CourseID`) razem jednoznacznie identyfikują poszczególne wiersze z `CourseAssignment` tabeli. `CourseAssignment` nie wymaga dedykowanego PK. `InstructorID` i `CourseID` właściwości pełnią PK. złożone Jedynym sposobem, aby określić złożonego PKs do programu EF Core jest *interfejsu API fluent*. W następnej sekcji pokazano, jak skonfigurować PK. złożonego

Klucz złożony zapewnia:

* Wiele wierszy są dozwolone dla jednego kursu.
* Wiele wierszy są dozwolone dla jednego instruktora.
* Wiele wierszy dla tego samego przez instruktorów i kurs jest niedozwolona.

`Enrollment` Jednostki sprzężenia definiuje swój własny klucz podstawowy, więc możliwe są duplikatami tego rodzaju. Aby uniknąć takich duplikaty:

* Dodaj unikatowy indeks pól klucza Obcego lub
* Konfigurowanie `Enrollment` przy użyciu podstawowego klucza złożonego podobne do `CourseAssignment`. Aby uzyskać więcej informacji, zobacz [indeksów](/ef/core/modeling/indexes).

## <a name="update-the-db-context"></a>Aktualizuj kontekst bazy danych

Dodaj następujący wyróżniony kod do *Data/SchoolContext.cs*:

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Poprzedzający kod dodaje nowe jednostki i konfiguruje `CourseAssignment` PK. złożonego jednostki

## <a name="fluent-api-alternative-to-attributes"></a>Zamiast interfejsu API Fluent atrybutów

`OnModelCreating` Metody w poprzednim kodzie używa *interfejsu API fluent* do konfigurowania zachowania programu EF Core. Interfejs API jest nazywany "fluent", ponieważ jest ona często używana przez centrali szereg wywołań metod w pojedynczej instrukcji. [Następujący kod](/ef/core/modeling/#methods-of-configuration) jest przykładem wygodnego interfejsu API:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

W tym samouczku interfejs fluent API jest używany tylko w przypadku mapowania bazy danych, który nie może odbywać się za pomocą atrybutów. Jednak interfejs fluent API można określić większość formatowania, sprawdzanie poprawności i reguł mapowania, które mogą realizować za pomocą atrybutów.

Niektóre atrybuty, takie jak `MinimumLength` nie można zastosować za pomocą interfejsu API fluent. `MinimumLength` nie powoduje zmiany schematu, ma zastosowanie tylko minimalną długość reguły weryfikacji.

Niektórzy deweloperzy wolą używać interfejsu API fluent, wyłącznie tak, aby ich zachowania ich klasami jednostki "Wyczyść". Atrybuty i wygodnego interfejsu API mogą być mieszane. Istnieją pewne konfiguracje, które mogą być przeprowadzone wyłącznie z interfejsu API fluent (określenia złożony klucz podstawowy). Istnieją pewne konfiguracje, które może być przeprowadzone wyłącznie z atrybutami (`MinimumLength`). Zalecana praktyka używania fluent API lub atrybutów:

* Wybierz jedną z tych dwóch metod.
* Użyć wybranej metody spójnie możliwie.

Niektóre atrybuty używane w tym samouczku są używane do:

* Sprawdzanie poprawności tylko (na przykład `MinimumLength`).
* EF Core tylko konfiguracji (na przykład `HasKey`).
* Sprawdzanie poprawności i programem EF Core konfiguracji (na przykład `[StringLength(50)]`).

Aby uzyskać więcej informacji na temat atrybutów, a interfejs fluent API zobacz [metod konfiguracji](/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Jednostki Diagram przedstawiający relacje

Poniższa ilustracja przedstawia diagram, który EF Power Tools Tworzenie modelu ukończone szkoły.

![Diagram jednostek](complex-data-model/_static/diagram.png)

Na powyższym diagramie przedstawiono:

* Kilka wierszy relacji jeden do wielu (1, aby \*).
* Wiersz relacji jeden do zero lub jeden (1 się od 0 do 1) między `Instructor` i `OfficeAssignment` jednostek.
* Wiersz relacji zero lub jeden do wielu (od 0 do 1 do *) między `Instructor` i `Department` jednostek.

## <a name="seed-the-db-with-test-data"></a>Inicjator bazy danych przy użyciu danych testowych

Aktualizowanie kodu w *Data/DbInitializer.cs*:

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Final)]

Powyższy kod udostępnia dane inicjatora dla nowych jednostek. Większość ten kod tworzy nowe obiekty jednostki i ładowania przykładowych danych. Dane przykładowe są używane do testowania. Zobacz `Enrollments` i `CourseAssignments` dla przykładów jak wiele do wielu Dołączanie tabel może zostać rozpoczęta.

## <a name="add-a-migration"></a>Dodaj migrację

Skompiluj projekt.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

```PMC
Add-Migration ComplexDataModel
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```console
dotnet ef migrations add ComplexDataModel
```

------

Poprzednie polecenie wyświetli ostrzeżenie o możliwej utracie danych.

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Jeśli `database update` polecenie jest wykonywane, generowany jest następujący błąd:

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

## <a name="apply-the-migration"></a>Zastosuj migracji

Teraz, gdy masz istniejącą bazę danych, należy wziąć pod uwagę sposób stosowania przyszłe zmiany do niego. W tym samouczku przedstawiono dwie metody:

* [Porzuć i ponownie utworzyć bazę danych](#drop)
* [Dotyczą migracji z istniejącej bazy danych](#applyexisting). Ta metoda jest bardziej złożony i czasochłonny proces, jest preferowanym podejściem w środowiskach produkcyjnych w rzeczywistych warunkach. **Uwaga**: Jest to opcjonalne części samouczka. Można zrobić listy i ponownie utwórz kroki i pominąć tę sekcję. Jeśli chcesz wykonać kroki zawarte w tej sekcji, nie są listy i ponownie utwórz kroki. 

<a name="drop"></a>

### <a name="drop-and-re-create-the-database"></a>Porzuć i ponownie utworzyć bazę danych

Kod w zaktualizowanej `DbInitializer` dodaje dane nowe jednostki. Aby wymusić programu EF Core do utworzenia nowej bazy danych, Porzuć i aktualizowanie bazy danych:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

W **Konsola Menedżera pakietów** (PMC), uruchom następujące polecenie:

```PMC
Drop-Database
Update-Database
```

Uruchom `Get-Help about_EntityFrameworkCore` z konsoli zarządzania Pakietami, aby uzyskać informacje pomocy.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Otwórz okno polecenia i przejdź do folderu projektu. Folder projektu zawiera *Startup.cs* pliku.

W oknie wiersza polecenia, należy wprowadzić następujące czynności:

 ```console
 dotnet ef database drop
dotnet ef database update
 ```

------

Uruchom aplikację. Uruchamianie uruchomienia aplikacji `DbInitializer.Initialize` metody. `DbInitializer.Initialize` Wypełnia nowej bazy danych.

Otwórz SSOX bazy danych:

* Jeśli SSOX był wcześniej otwarty, kliknij przycisk **Odśwież** przycisku.
* Rozwiń **tabel** węzła. Utworzone tabele są wyświetlane.

![Tabele w SSOX](complex-data-model/_static/ssox-tables.png)

Sprawdź **CourseAssignment** tabeli:

* Kliknij prawym przyciskiem myszy **CourseAssignment** tabeli, a następnie wybierz pozycję **dane widoku**.
* Sprawdź **CourseAssignment** tabela zawiera dane.

![Dane CourseAssignment SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="applyexisting"></a>

### <a name="apply-the-migration-to-the-existing-database"></a>Dotyczą migracji z istniejącej bazy danych

Ta sekcja jest opcjonalna. Następujące kroki działają tylko wtedy, gdy pominięto poprzednie [Usuń i ponownie utworzyć bazę danych](#drop) sekcji.

Podczas migracji są uruchamiane z istniejącymi danymi, mogą istnieć ograniczenia klucza Obcego, które nie są spełnione z istniejącymi danymi. Z danymi produkcyjnymi należy przedsięwziąć do migrowania istniejących danych. Ta sekcja zawiera przykład poprawiania naruszenia ograniczeń klucza Obcego. Nie wprowadzaj tych zmian kodu, bez kopii zapasowej. Nie należy wprowadzić te zmiany kodu, jeśli wykonano poprzedniej sekcji, a aktualizacji bazy danych.

*{Timestamp}_ComplexDataModel.cs* plik zawiera następujący kod:

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

Poprzedzający kod dodaje dopuszcza `DepartmentID` klucza Obcego do `Course` tabeli. Bazy danych z poprzedniego samouczka zawiera wiersze w `Course`, więc ta tabela nie może zaktualizować migracji.

Zapewnienie `ComplexDataModel` pracy migracji z istniejącymi danymi:

* Zmień kod, aby nadać nową kolumnę (`DepartmentID`) wartość domyślną.
* Tworzyć fałszywej dział o nazwie "Temp" jako domyślnego działu.

#### <a name="fix-the-foreign-key-constraints"></a>Rozwiązywanie ograniczeń klucza obcego

Aktualizacja `ComplexDataModel` klasy `Up` metody:

* Otwórz *{timestamp}_ComplexDataModel.cs* pliku.
* Komentarz wiersza kodu, który dodaje `DepartmentID` kolumny `Course` tabeli.

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

Dodaj następujący wyróżniony kod. Nowy kod przechodzi po `.CreateTable( name: "Department"` bloku: [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

Za pomocą zmian poprzednim, istniejące `Course` wiersze zostaną powiązane do działu "Temperatura", po `ComplexDataModel` `Up` metoda przebiegów.

Aplikacji produkcyjnej może:

* Uwzględnić w kodzie albo skryptach, aby dodać `Department` wiersze i powiązane `Course` wierszy do nowego `Department` wierszy.
* Używaj działu "Temp" lub wartość domyślną dla `Course.DepartmentID`.

Następny samouczek obejmuje powiązane dane.

::: moniker-end

> [!div class="step-by-step"]
> [Poprzednie](xref:data/ef-rp/migrations)
> [dalej](xref:data/ef-rp/read-related-data)
