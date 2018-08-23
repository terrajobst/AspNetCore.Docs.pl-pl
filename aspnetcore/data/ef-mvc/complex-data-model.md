---
title: Platforma ASP.NET Core MVC z programem EF Core — Model danych — 5, 10
author: rick-anderson
description: W tym samouczku należy dodać większą liczbę jednostek i relacji i Dostosuj model danych, określając formatowania i sprawdzania poprawności i reguł mapowania.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: c97acd2b943e1d4c466c060247220b3b8bab6d6b
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756216"
---
# <a name="aspnet-core-mvc-with-ef-core---data-model---5-of-10"></a>Platforma ASP.NET Core MVC z programem EF Core — Model danych — 5, 10

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

Przez [Tom Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)

Przykładową aplikację sieci web firmy Contoso University pokazuje, jak tworzyć aplikacje sieci web platformy ASP.NET Core MVC za pomocą platformy Entity Framework Core i Visual Studio. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](intro.md).

W poprzednich samouczkach doświadczenie w pracy z modelu prostego danych, który został składające się z trzech jednostek. W tym samouczku dodasz więcej jednostek i relacji i będzie Dostosuj model danych, określając formatowania i sprawdzania poprawności i reguł mapowania bazy danych.

Gdy skończysz, klas jednostek tworzących model danych ukończone, który jest wyświetlany na poniższej ilustracji:

![Diagram jednostek](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Dostosuj Model danych przy użyciu atrybutów

W tej sekcji pokazano, jak dostosować model danych przy użyciu atrybutów, które określają, formatowanie, sprawdzanie poprawności i reguł mapowania bazy danych. Następnie w kilku z następujących sekcji, które zostaną utworzone pełnego modelu danych służbowych, dodając atrybutów do klasy została już utworzona oraz tworzenie nowych klas pozostałe typy jednostek w modelu.

### <a name="the-datatype-attribute"></a>Atrybut typu danych

Dat rejestracji dla uczniów wszystkie strony sieci web obecnie wyświetlić czas wraz z datą, mimo że wszystkich interesujących Cię dla tego pola jest datą. Za pomocą atrybutów adnotacji danych, można go utworzyć kod zmian, który naprawi format wyświetlania w każdym widoku, który zawiera dane. Aby zobaczyć przykład sposobu wykonania, należy dodać atrybut `EnrollmentDate` właściwość `Student` klasy.

W *Models/Student.cs*, Dodaj `using` poufności informacji dotyczące `System.ComponentModel.DataAnnotations` przestrzeni nazw i Dodaj `DataType` i `DisplayFormat` atrybuty do `EnrollmentDate` właściwości, jak pokazano w poniższym przykładzie:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

`DataType` Atrybut jest używany do określenia typu danych, który jest bardziej szczegółowe niż typ wewnętrznej bazy danych. W tym przypadku ma być uruchamiany tylko do śledzenia daty, nie daty i godziny. `DataType` Wyliczenie udostępnia wiele typów danych, takich jak daty, godziny, numer telefonu, waluty, EmailAddress i więcej. `DataType` Atrybut można również włączyć automatyczne udostępnianie funkcji specyficznych dla typu aplikacji. Na przykład `mailto:` łącza mogą być tworzone dla `DataType.EmailAddress`, i można podać selektora daty `DataType.Date` w przeglądarkach obsługujących HTML5. `DataType` Atrybut emituje HTML 5 `data-` atrybutów (Wymowa: dane dash), które może zrozumieć przeglądarki HTML 5. `DataType` Atrybuty nie zawierają żadnych sprawdzania poprawności.

`DataType.Date` nie określa format daty, która jest wyświetlana. Domyślnie pole danych są wyświetlane domyślne formaty oparte na serwerze CultureInfo.

`DisplayFormat` Atrybut jest używany jawnie określić format daty:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

`ApplyFormatInEditMode` Ustawienie określa, że formatowanie powinien również będą stosowane, gdy wartość jest wyświetlana w polu tekstowym do edycji. (Nie można w przypadku niektórych pól — na przykład dla wartości waluty, może nie przewidziane symbol waluty w polu tekstowym do edycji.)

Możesz użyć `DisplayFormat` atrybutu przez sam, ale zazwyczaj jest dobry pomysł, aby użyć `DataType` również atrybutu. `DataType` Atrybut umożliwia przekazywanie semantykę dane, a nie jak renderować ją na ekranie i zapewnia następujące korzyści, które nie można uzyskać za pomocą `DisplayFormat`:

* Przeglądarka można włączyć funkcje HTML5 (na przykład pokazać kontrolki kalendarza, symbol waluty odpowiednich ustawień regionalnych, przesyłanie pocztą e-mail łączy kilka po stronie klienta. dane wejściowe sprawdzania poprawności, itp.).

* Domyślnie przeglądarka wyświetli dane przy użyciu poprawny format, w oparciu o ustawienia regionalne.

Aby uzyskać więcej informacji, zobacz [ \<wejściowych > dokumentacja pomocnika tagów](../../mvc/views/working-with-forms.md#the-input-tag-helper).

Uruchom aplikację, przejdź do strony indeksu studentów i zwróć uwagę, nie są wyświetlane godziny dla daty rejestracji. Będzie taka sama wartość true dla dowolnego widoku, który używa modelu dla uczniów.

![Wyświetlanie daty bez godziny strony indeksu uczniów](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>Atrybut StringLength

Można również określić reguły sprawdzania poprawności danych i komunikatów o błędach weryfikacji przy użyciu atrybutów. `StringLength` Atrybut Ustawia maksymalną długość w bazie danych i zapewnia po stronie klienta i po stronie serwera sprawdzania poprawności dla platformy ASP.NET Core MVC. Minimalna długość ciągu można również określić, w tym atrybucie, ale wartość minimalna nie ma wpływu na schemat bazy danych.

Załóżmy, że chcesz upewnić się, że użytkownicy nie wprowadzić więcej niż 50 znaków dla nazwy. Aby dodać to ograniczenie `StringLength` atrybuty do `LastName` i `FirstMidName` właściwości, jak pokazano w poniższym przykładzie:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

`StringLength` Atrybutu nie uniemożliwić wprowadzanie białe miejsca dla nazwy użytkownika. Możesz użyć `RegularExpression` atrybutu, aby zastosować ograniczenia do danych wejściowych. Na przykład poniższy kod wymaga pierwszy znak na wielkie litery, a pozostałe znaki, aby być znakiem alfabetycznym:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

`MaxLength` Atrybut oferuje funkcje podobne do `StringLength` atrybutu, ale nie zapewnia po stronie klienta sprawdzania poprawności.

Model bazy danych zostanie zmieniona w taki sposób, że wymaga zmiany w schemacie bazy danych. Użyjesz migracji do zaktualizowania schematu bez utraty danych, która może być została dodana do bazy danych przy użyciu interfejsu użytkownika aplikacji.

Zapisz zmiany i skompiluj projekt. Następnie otwórz okno polecenia w folderze projektu i wprowadź następujące polecenia:

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

`migrations add` Polecenia wyświetli ostrzeżenie, że może dojść do utraty danych, ponieważ ta zmiana powoduje maksymalną długość krótszy dwie kolumny.  Migracje tworzy plik o nazwie  *\<sygnatura czasowa > _MaxLengthOnNames.cs*. Ten plik zawiera kod w `Up` metodę, która spowoduje zaktualizowanie bazy danych, aby dopasować bieżącym modelem danych. `database update` Polecenie zadziałało kod.

Sygnatura czasowa prefiks do nazwy pliku migracji umożliwia przez program Entity Framework kolejność migracji. Możesz utworzyć wiele migracji przed uruchomieniem polecenia update-database, a następnie wszystkie migracje są stosowane w kolejności, w którym zostały utworzone.

Uruchom aplikację, wybierz **studentów** kliknij pozycję **Utwórz nowy**, a następnie wprowadź nazwę, albo więcej niż 50 znaków. Po kliknięciu **Utwórz**, weryfikacji po stronie klienta zawiera komunikat o błędzie.

![Studenci indeksu strona wyświetlająca błędy długość ciągu](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a>Atrybut kolumny

Umożliwia także atrybuty kontrolować, jak klas i właściwości są mapowane do bazy danych. Załóżmy, że używasz nazwy `FirstMidName` nazwę pierwszego pola, ponieważ pole może także zawierać imienia. Jednak kolumna bazy danych, można go nazwać `FirstName`, ponieważ użytkownicy, którzy będą Pisanie zapytań ad hoc w bazie danych są przyzwyczajeni do tej nazwy. Aby to mapowanie, można użyć `Column` atrybutu.

`Column` Atrybut określa, że po utworzeniu bazy danych, kolumna `Student` tabelę, która mapuje do `FirstMidName` właściwość o nazwie `FirstName`. Innymi słowy, gdy kod odwołuje się do `Student.FirstMidName`, dane będą pochodzić z lub aktualizowane w `FirstName` kolumny `Student` tabeli. Jeśli nie określisz nazwy kolumn, mają one taką samą nazwę jak nazwa właściwości.

W *Student.cs* Dodaj `using` poufności informacji dotyczące `System.ComponentModel.DataAnnotations.Schema` i Dodaj atrybut nazwy kolumny, aby `FirstMidName` właściwości, jak pokazano na następujący wyróżniony kod:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

Dodanie `Column` atrybut zmieni się zapasowy modelu `SchoolContext`, dzięki czemu nie będzie ona zgodna z bazą danych.

Zapisz zmiany i skompiluj projekt. Następnie otwórz okno polecenia w folderze projektu i wprowadź następujące polecenia, aby utworzyć inną migracji:

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

W **Eksplorator obiektów SQL Server**, otwórz projektanta tabel dla uczniów, klikając dwukrotnie **uczniów** tabeli.

![Tabela studentów w SSOX po migracji](complex-data-model/_static/ssox-after-migration.png)

Przed zastosowaniem pierwsze dwie migracje nazwa kolumny były typu nvarchar(MAX). Są one teraz nvarchar(50) i nazwę kolumny został zmieniony z FirstMidName na imię.

> [!Note]
> Jeśli spróbujesz skompilować przed zakończeniem, tworzenie wszystkich klas jednostek w poniższych sekcjach, możesz otrzymać błędy kompilatora.

## <a name="final-changes-to-the-student-entity"></a>Ostateczne zmiany do jednostki dla uczniów

![Jednostki dla uczniów](complex-data-model/_static/student-entity.png)

W *Models/Student.cs*, Zastąp kod został dodany wcześniej następujący kod. Zmiany są wyróżnione.

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>Wymagany atrybut

`Required` Atrybutu sprawia, że nazwa właściwości wymagane pola. `Required` Atrybut nie jest wymagane dla typów innych niż null, takich jak typy wartości (daty/godziny, int, dwukrotnie, float, itp.). Typy, które nie może mieć wartości null są automatycznie traktowane jako wymagane pola.

Można usunąć `Required` atrybutu i zastąp go minimalną długość parametru `StringLength` atrybutu:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>Atrybut wyświetlania

`Display` Atrybut określa, czy podpis dla pól tekstowych powinny być "Imię", "Last Name", "Pełna nazwa" i "Data rejestracji" zamiast nazwy właściwości, w każdym wystąpieniu (który nie ma miejsca dzielenia wyrazów).

### <a name="the-fullname-calculated-property"></a>Właściwości obliczane imię i nazwisko

`FullName` jest właściwością obliczeniową, która zwraca wartość, która jest tworzona przez dołączenie dwóch innych właściwości. Dlatego ma tylko akcesor pobierania, a nie `FullName` kolumny zostanie wygenerowany w bazie danych.

## <a name="create-the-instructor-entity"></a>Tworzenie jednostki przez instruktorów

![Jednostki przez instruktorów](complex-data-model/_static/instructor-entity.png)

Tworzenie *Models/Instructor.cs*, zastępując kod szablonu poniższym kodem:

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Należy zauważyć, że kilka właściwości są takie same, w jednostkach studenta i programistę-instruktora. W [Implementowanie dziedziczenia](inheritance.md) samouczek w dalszej części tej serii, będzie Refaktoryzuj ten kod w celu wyeliminowania nadmiarowości.

Wiele atrybutów można umieścić w jednym wierszu, dzięki czemu można również napisać `HireDate` atrybutów w następujący sposób:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>Właściwości nawigacji CourseAssignments i OfficeAssignment

`CourseAssignments` i `OfficeAssignment` właściwości są właściwości nawigacji.

Pod kierunkiem instruktora nauczyć dowolnej liczby kursów, więc `CourseAssignments` jest zdefiniowany jako kolekcja.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Właściwość nawigacji może zawierać wiele jednostek, listy, w którym wpisy mogą być dodawane, usuwane lub zaktualizować musi być typu.  Można określić `ICollection<T>` lub typu, takie jak `List<T>` lub `HashSet<T>`. Jeśli określisz `ICollection<T>`, tworzy EF `HashSet<T>` kolekcji domyślnie.

Powód, dlaczego są `CourseAssignment` jednostek zostało wyjaśnione poniżej w sekcji o relacji wiele do wielu.

Reguły biznesowe firmy Contoso University stanu, że pod kierunkiem instruktora może mieć tylko co najwyżej jednego pakietu office, więc `OfficeAssignment` właściwość przechowuje pojedynczej jednostki OfficeAssignment, (które mogą być wartość null, jeśli nie przypisano żadnych pakietu office).

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Tworzenie jednostki OfficeAssignment

![OfficeAssignment jednostki](complex-data-model/_static/officeassignment-entity.png)

Tworzenie *Models/OfficeAssignment.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Atrybut klucza

Brak relacji jeden do zero lub jeden między instruktora oraz OfficeAssignment jednostek. Biuro istnieje tylko w odniesieniu do przez instruktorów, który jest przypisany do, a zatem jej klucz podstawowy również jest jego klucza obcego z jednostką instruktora. Ale Entity Framework automatycznie nie może rozpoznać InstructorID jako klucz podstawowy dla tej jednostki, ponieważ jego nazwa nie przestrzegać konwencji nazewnictwa identyfikator lub classnameID. W związku z tym `Key` atrybut jest używany do identyfikowania jej jako klucza:

```csharp
[Key]
public int InstructorID { get; set; }
```

Można również użyć `Key` atrybutu, jeśli jednostka ma swój własny klucz podstawowy, ale chcesz nazwać właściwość, coś innego niż classnameID lub identyfikatora organizacji.

Domyślnie program EF traktuje klucza jako inne niż wygenerowane bazy danych, ponieważ kolumna jest przeznaczona dla relacji identyfikującej.

### <a name="the-instructor-navigation-property"></a>Właściwość nawigacji przez instruktorów

Jednostka przez instruktorów ma wartość null `OfficeAssignment` właściwości nawigacji (ponieważ pod kierunkiem instruktora, może nie mieć biuro), i jednostka OfficeAssignment ma wartość null `Instructor` właściwości nawigacji (ponieważ nie przypisania pakietu office istnieje bez pod kierunkiem instruktora — `InstructorID` nie dopuszcza wartości null). Jednostka przez instruktorów ma powiązanej jednostki OfficeAssignment, każda jednostka ma odwołania do innych niż w jego właściwości nawigacji.

Możesz umieścić `[Required]` atrybutu właściwość nawigacji przez instruktorów, aby określić, że muszą być powiązane przez instruktorów, ale nie trzeba tego robić, ponieważ `InstructorID` nie dopuszczają wartości klucza obcego, (który również jest kluczem do tej tabeli).

## <a name="modify-the-course-entity"></a>Modyfikowanie jednostek kursu

![Kurs jednostki](complex-data-model/_static/course-entity.png)

W *Models/Course.cs*, Zastąp kod został dodany wcześniej następujący kod. Zmiany są wyróżnione.

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

Jednostka kurs ma właściwość klucza obcego `DepartmentID` który wskazuje na powiązanej jednostce działu i ma `Department` właściwości nawigacji.

Entity Framework nie wymaga dodanie właściwości klucza obcego w modelu danych mają właściwości nawigacji dla powiązanej jednostki.  EF automatycznie tworzy klucze obce w bazie danych wszędzie tam, gdzie będą one potrzebne i tworzy [w tle właściwości](https://docs.microsoft.com/ef/core/modeling/shadow-properties) dla nich. Ale o klucza obcego w modelu danych, że aktualizacje są prostszy i efektywniejszy. Na przykład, gdy fetch jednostkę kurs, aby edytować jednostki działu ma wartość null Jeśli nie zostanie załadowany, więc po zaktualizowaniu jednostki kurs, trzeba najpierw pobrać jednostki działu. Gdy właściwość klucza obcego `DepartmentID` znajduje się w modelu danych nie trzeba pobrać jednostki działu, przed uaktualnieniem.

### <a name="the-databasegenerated-attribute"></a>Atrybut DatabaseGenerated

`DatabaseGenerated` Atrybutem `None` parametru `CourseID` właściwość określa, że wartości klucza podstawowego są dostarczone przez użytkownika, a nie generowane przez bazę danych.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

Domyślnie program Entity Framework przyjęto założenie, że wartości klucza podstawowego są generowane przez bazę danych. To, co ma w większości scenariuszy. Jednak w przypadku jednostek kurs będzie numer kurs określonych przez użytkownika, takich jak serie 1000 służy do jednego działu, serię 2000 do innego działu i tak dalej.

`DatabaseGenerated` Atrybut może służyć także do generowania wartości domyślne, jak w przypadku kolumny bazy danych używane do rejestrowania Data wiersz został utworzony lub zaktualizowany.  Aby uzyskać więcej informacji, zobacz [wygenerowanych właściwości](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Obcy właściwości klucza i nawigacja

Właściwości klucza obcego i właściwości nawigacji w jednostce kurs odzwierciedla się następująco:

Kurs jest przypisany do jednego działu, więc ma `DepartmentID` klucza obcego i `Department` właściwość nawigacji z powodów wymienionych powyżej.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Kurs może mieć dowolną liczbę uczniów zarejestrowane, więc `Enrollments` właściwość nawigacji jest kolekcją:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Kurs może być prowadzone przez instruktorów wielu, więc `CourseAssignments` właściwość nawigacji jest kolekcją (typ `CourseAssignment` zostało wyjaśnione [później](#many-to-many-relationships)):

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a>Tworzenie jednostki działu

![Dział jednostki](complex-data-model/_static/department-entity.png)


Tworzenie *Models/Department.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>Atrybut kolumny

Wcześniej użyto `Column` atrybutu, aby zmienić mapowanie nazwy kolumny. W kodzie dla jednostki działu `Column` atrybut jest używany do zmienić mapowanie typu danych SQL, tak aby kolumna zostanie zdefiniowana przy użyciu typu pieniądze programu SQL Server w bazie danych:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Mapowanie kolumn zazwyczaj nie jest wymagana, ponieważ Entity Framework wybiera odpowiedni typ danych programu SQL Server, na podstawie typu CLR, który zdefiniujesz dla właściwości. Środowisko CLR `decimal` typu map do programu SQL Server `decimal` typu. Ale w takim przypadku wiesz, że kolumna będzie zawierający kwot i pieniędzy typ danych jest bardziej odpowiednia dla tego.

### <a name="foreign-key-and-navigation-properties"></a>Obcy właściwości klucza i nawigacja

Obcy właściwości klucza i nawigacji odzwierciedla się następująco:

Dział może lub nie ma uprawnienia administratora, a administrator zawsze ma pod kierunkiem instruktora. W związku z tym `InstructorID` właściwości jest dołączana jako kluczem obcym jednostki przez instruktorów, a znak zapytania są dodawane po `int` wpisz oznaczenia, aby oznaczyć właściwości jako dopuszczającego wartość null. Właściwość nawigacji jest o nazwie `Administrator` , ale zawiera jednostki przez instruktorów:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Dział może mieć wielu kursów, więc ma właściwości nawigacji kursy:

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> Zgodnie z Konwencją platformy Entity Framework umożliwia usuwanie kaskadowe nieprzyjmujące wartości kluczy obcych i w przypadku relacji wiele do wielu. Może to spowodować cykliczne cascade delete reguł, które spowoduje wyjątek podczas próby dodania do migracji. Na przykład jeśli właściwość Department.InstructorID nie zostały zdefiniowane jako dopuszczającego wartość null, EF konfigurowania cascade, Usuń regułę można usunąć instruktora, gdy usuniesz działu, która nie ma być możliwe. W razie potrzeby własne reguły biznesowe `InstructorID` właściwość jako wartość null, trzeba wyłączyć przy użyciu następującej instrukcji interfejsu API fluent usuwanie kaskadowe relacji:
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a>Modyfikowanie jednostek rejestracji

![Jednostki rejestracji](complex-data-model/_static/enrollment-entity.png)

W *Models/Enrollment.cs*, Zastąp kod został dodany wcześniej następujący kod:

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Obcy właściwości klucza i nawigacja

Właściwości klucza obcego i właściwości nawigacji odzwierciedla się następująco:

Rekord rejestracji jest jednego kursu, więc ma `CourseID` właściwość klucza obcego i `Course` właściwość nawigacji:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Rekord rejestracji jest dla uczniów lub studentów pojedynczego, więc ma `StudentID` właściwość klucza obcego i `Student` właściwość nawigacji:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relacje wiele do wielu

Istnieje relacja wiele do wielu między jednostkami studenta i programistę-kurs i jednostki rejestrowanie funkcji jako tabelę sprzężenia wiele do wielu *z ładunkiem* w bazie danych. "Z ładunkiem" oznacza, że tabela rejestracji zawiera dane dodatkowe, oprócz klucze obce w dołączonym do tabel (w tym przypadku klucz podstawowy i właściwości klasy korporacyjnej).

Poniższa ilustracja przedstawia, jak wyglądają te relacje w diagramie jednostki. (Ten diagram został wygenerowany za pomocą narzędzi Entity Framework Power Tools dla platformy EF 6.x; tworzenia diagramu nie należały do tego samouczka, po prostu jest on używany tutaj jako ilustrację.)

![Kurs dla uczniów wiele do wielu relacji](complex-data-model/_static/student-course.png)

Każdy wiersz relacji ma 1 na jednym końcu i znak gwiazdki (*) na drugim, wskazując relacji jeden do wielu.

Jeśli tabela rejestracji nie obejmują informacji klasy korporacyjnej, czy tylko musi on zawierać dwa klucze obce CourseID i StudentID. W takim przypadku byłoby tabelę sprzężenia wiele do wielu, bez ładunku (lub tabelę sprzężenia czystego) w bazie danych. Jednostki instruktora oraz przypisane kurs mają tego rodzaju relacji wiele do wielu i następnym krokiem jest utworzenie klasę jednostki działa jak sprzężenie tabeli bez ładunku.

(Tabele niejawne sprzężenia dla relacji wiele do wielu, ale programu EF Core nie obsługuje 6.x EF. Aby uzyskać więcej informacji, zobacz [dyskusji w repozytorium programu EF Core w usłudze GitHub](https://github.com/aspnet/EntityFramework/issues/1368).)

## <a name="the-courseassignment-entity"></a>Jednostka CourseAssignment

![CourseAssignment jednostki](complex-data-model/_static/courseassignment-entity.png)

Tworzenie *Models/CourseAssignment.cs* następującym kodem:

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a>Dołącz do nazwy podmiotu

Tabelę sprzężenia jest wymagana w bazie danych dla relacji wiele do wielu instruktora do kursów, a musi być reprezentowany przez zestaw jednostek. Często jest nazwa jednostki sprzężenia `EntityName1EntityName2`, który w tym przypadku wyniesie `CourseInstructor`. Jednak zaleca się, że wybierz nazwę, która opisuje relację. Modele danych zaczynają prosty i rozwój za pomocą sprzężeń ładunku nie często uzyskiwanie ładunków później. W przypadku uruchomienia pod nazwą opisową jednostki, nie musisz później zmienić nazwę. W idealnym przypadku jednostki sprzężenia będzie mieć własną fizyczną nazwę (prawdopodobnie jednowyrazowego) w domenie firmy. Na przykład książek i klientów można połączyć za pomocą klasyfikacji. Dla tej relacji `CourseAssignment` jest lepszym rozwiązaniem niż `CourseInstructor`.

### <a name="composite-key"></a>Klucz złożony

Ponieważ klucze obce nie dopuszcza wartości null i razem jednoznacznie identyfikują poszczególne wiersze tabeli, nie ma potrzeby oddzielnych klucza podstawowego. *InstructorID* i *CourseID* właściwości powinny działać jako złożony klucz podstawowy. Jedynym sposobem, aby zidentyfikować złożonego kluczy podstawowych do programów EF polega na użyciu *interfejsu API fluent* (go nie może odbywać się przy użyciu atrybutów). Pokazano, jak skonfigurować złożony klucz podstawowy w następnej sekcji.

Klucz złożony gwarantuje, że kiedy masz wiele wierszy do jednego kursu i wiele wierszy dla jednego przez instruktorów, nie może mieć wiele wierszy dla tego samego przez instruktorów i kurs. `Enrollment` Sprzężenia jednostka definiuje swój własny klucz podstawowy, więc możliwe są duplikatami tego rodzaju. Aby uniknąć takich duplikaty, możesz można dodać unikatowego indeksu na pola kluczy obcych, lub skonfigurować `Enrollment` przy użyciu podstawowego klucza złożonego podobne do `CourseAssignment`. Aby uzyskać więcej informacji, zobacz [indeksów](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-database-context"></a>Aktualizowanie kontekstu bazy danych

Dodaj następujący wyróżniony kod do *Data/SchoolContext.cs* pliku:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Ten kod dodaje nowe jednostki i konfiguruje złożony klucz podstawowy jednostki CourseAssignment.

## <a name="fluent-api-alternative-to-attributes"></a>Zamiast interfejsu API Fluent atrybutów

Kod w `OnModelCreating` metody `DbContext` klasy używa *interfejsu API fluent* do konfigurowania zachowania EF. Interfejs API jest nazywany "fluent", ponieważ jest ona często używana przez centrali szereg wywołań metod, które razem w pojedynczej instrukcji, jak w poniższym przykładzie z [dokumentacji programu EF Core](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

W tym samouczku używasz interfejsu API fluent tylko dla mapowania bazy danych, nie można wykonać za pomocą atrybutów. Jednak umożliwia także wygodnego interfejsu API do określenia większość formatowania, sprawdzanie poprawności i reguł mapowania, które można wykonać przy użyciu atrybutów. Niektóre atrybuty, takie jak `MinimumLength` nie można zastosować za pomocą interfejsu API fluent. Jak wspomniano wcześniej, `MinimumLength` nie zmienił się schemat, ma zastosowanie tylko reguły walidacji po stronie klienta i serwera.

Niektórzy deweloperzy wolą używać interfejsu API fluent, wyłącznie tak, aby ich zachowania ich klasami jednostki "Wyczyść". Jeśli chcesz, a kilka dostosowania, które jest możliwe tylko przy użyciu interfejsu API fluent można łączyć atrybutów i wygodnego interfejsu API, ale ogólnie rzecz biorąc zalecaną praktyką jest, wybierz jedną z tych dwóch metod i używać oznacza spójnie możliwie. Jeśli używasz obu, należy pamiętać, że wszędzie tam, gdzie występuje konflikt, interfejsu API Fluent zastępuje atrybuty.

Aby uzyskać więcej informacji na temat atrybutów, a interfejs fluent API zobacz [metod konfiguracji](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Jednostki Diagram przedstawiający relacje

Poniższa ilustracja przedstawia diagram, który narzędzi Entity Framework Power Tools Tworzenie modelu ukończone szkoły.

![Diagram jednostek](complex-data-model/_static/diagram.png)

Oprócz liniach relacji jeden do wielu (1, aby \*), możesz teraz zobaczyć wiersz relacji jeden do zero lub jeden (1 się od 0 do 1) między jednostkami instruktora oraz przypisane OfficeAssignment i wiersz relacji zero lub jeden do wielu (od 0 do 1 do *) między Jednostki przez instruktorów i działu.

## <a name="seed-the-database-with-test-data"></a>Inicjowanie bazy danych przy użyciu danych testowych

Zastąp kod w *Data/DbInitializer.cs* pliku następującym kodem w celu dostarczenia danych inicjatora dla nowych jednostek, które zostały utworzone.

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

Jak przedstawiono w pierwszym samouczku większość ten kod po prostu tworzy nowe obiekty jednostki i ładuje przykładowych danych do właściwości zgodnie z potrzebami testowania. Zwróć uwagę, jak są obsługiwane w relacji wiele do wielu: kod tworzy relacje, tworząc jednostek w `Enrollments` i `CourseAssignment` Dołącz zestawy jednostek.

## <a name="add-a-migration"></a>Dodaj migrację

Zapisz zmiany i skompiluj projekt. Następnie otwórz okno polecenia w folderze projektu i wprowadź `migrations add` polecenie (nie jeszcze nie robi polecenia update-database):

```console
dotnet ef migrations add ComplexDataModel
```

Zostanie wyświetlone ostrzeżenie o możliwej utracie danych.

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Jeśli próbowano uruchomić `database update` polecenia w tym momencie (nie rób tego jeszcze), czy występuje następujący błąd:

> Instrukcja ALTER TABLE konflikt z ograniczeniem klucza OBCEGO "FK_dbo. Course_dbo. Department_DepartmentID". Konflikt wystąpił w bazie danych "ContosoUniversity" table "dbo. Dział", kolumna"DepartmentID".

Czasami, podczas wykonywania migracji z istniejącymi danymi, należy wstawić dane do bazy danych, aby spełnić ograniczeń klucza obcego. Kod wygenerowany w `Up` metoda dodaje klucz obcy dopuszcza DepartmentID do tabeli kursu. Jeśli istnieją już wierszy w tabeli kursu po uruchomieniu kodu `AddColumn` operacja zakończy się niepowodzeniem, ponieważ program SQL Server nie może ustalić, jakie wartości do umieszczenia w kolumnie, która nie może mieć wartości null. W tym samouczku będą uruchamiane migracji w nowej bazy danych, ale w aplikacji produkcyjnej trzeba migracji, obsługę istniejących danych, dlatego poniższe instrukcje przedstawiają przykłady jak to zrobić.

Zapewnienie tej migracji działa z istniejącymi danymi, które trzeba zmienić kod, aby nadać nową kolumnę wartości domyślnej, a następnie utwórz działu szkieletu o nazwie "Temp" jako domyślnego działu. W rezultacie istniejące wiersze kurs zostaną wszystkie powiązane do działu "Temperatura", po `Up` metoda przebiegów.

* Otwórz *{timestamp}_ComplexDataModel.cs* pliku.

* Komentarz wiersza kodu, która dodaje kolumnę DepartmentID do tabeli kursu.

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* Dodaj następujący wyróżniony kod po kodzie, który tworzy tabelę działu:

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

W przypadku aplikacji produkcyjnych należy napisać kod lub skrypty do dodawania wierszy do działu i dotyczą wierszy kurs nowych wierszy działu. Nie jest już należałoby wtedy działu "Temp" lub wartość domyślna w kolumnie Course.DepartmentID.

Zapisz zmiany i skompiluj projekt.

## <a name="change-the-connection-string-and-update-the-database"></a>Zmień parametry połączenia i zaktualizować bazę danych

Masz teraz nowy kod w `DbInitializer` klasę, która dodaje dane nowe jednostki do pustej bazy danych. Aby EF, Utwórz nową, pustą bazę danych, Zmień nazwę bazy danych w parametrach połączenia w *appsettings.json* ContosoUniversity3 lub innej nazwy, które nie były używane na komputerze używasz.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

Zapisać zmiany do *appsettings.json*.

> [!NOTE]
> Alternatywnie można zmienić nazwy bazy danych można usunąć bazy danych. Użyj **Eksplorator obiektów SQL Server** (SSOX) lub `database drop` interfejsu wiersza polecenia:
> ```console
> dotnet ef database drop
> ```

Po zmianie nazwy bazy danych lub usunięte z bazy danych uruchom `database update` polecenia w oknie wiersza polecenia do wykonania migracji.

```console
dotnet ef database update
```

Uruchom aplikację, aby spowodować, że `DbInitializer.Initialize` metodę, aby uruchomić i wypełnić nową bazę danych.

Otwórz bazę danych w SSOX, tak jak wcześniej, a następnie rozwiń **tabel** węzeł, aby zobaczyć, czy wszystkie tabele zostały utworzone. (Jeśli nadal masz SSOX otworzyć z wcześniejszego stanu, kliknij przycisk **Odśwież** przycisku.)

![Tabele w SSOX](complex-data-model/_static/ssox-tables.png)

Uruchom aplikację, aby wyzwolić kod inicjatora, który inicjowania inicjuje bazy danych.

Kliknij prawym przyciskiem myszy **CourseAssignment** tabeli, a następnie wybierz pozycję **dane widoku** Aby sprawdzić, czy zawiera dane w nim.

![Dane CourseAssignment SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a>Podsumowanie

Masz teraz bardziej złożonego modelu danych i odpowiednią bazę danych. W następującego samouczka dowiesz się więcej o tym, jak uzyskać dostęp do powiązanych danych.
::: moniker-end

> [!div class="step-by-step"]
> [Poprzednie](migrations.md)
> [dalej](read-related-data.md)
