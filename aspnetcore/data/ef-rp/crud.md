---
title: Razor Pages z EF Core w ASP.NET Core-CRUD-2 z 8
author: rick-anderson
description: Pokazuje, jak tworzyć, odczytywać, aktualizować i usuwać EF Core.
ms.author: riande
ms.date: 07/22/2019
uid: data/ef-rp/crud
ms.openlocfilehash: 05519852fab22bd3ad5b77e3494b49191448286f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665649"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a>Razor Pages z EF Core w ASP.NET Core-CRUD-2 z 8

Autorzy [Dykstra](https://github.com/tdykstra), [Jan P Kowalski](https://twitter.com/thereformedprog)i [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

W tym samouczku kod szkieletu CRUD (tworzenie, odczytywanie, aktualizowanie, usuwanie) zostanie sprawdzony i dostosowany.

## <a name="no-repository"></a>Brak repozytorium

Niektórzy Deweloperzy używają warstwy usług lub wzorca repozytorium do tworzenia warstwy abstrakcji między interfejsem użytkownika (Razor Pages) i warstwą dostępu do danych. Ten samouczek nie działa. Aby zminimalizować złożoność i utrzymywać samouczek na EF Core, kod EF Core zostanie dodany bezpośrednio do klas modelu strony. 

## <a name="update-the-details-page"></a>Aktualizowanie strony szczegółów

Kod szkieletowy dla stron uczniów nie obejmuje danych rejestracji. W tej sekcji dowiesz się, jak dodać rejestracje do strony szczegółów.

### <a name="read-enrollments"></a>Odczytaj rejestracje

Aby wyświetlić na stronie dane rejestracyjne ucznia, należy je przeczytać. Kod szkieletowy na *stronach/studentów/szczegóły. cshtml. cs* odczytuje tylko dane uczniów, bez danych rejestracji:

[!code-csharp[Main](intro/samples/cu30snapshots/2-crud/Pages/Students/Details1.cshtml.cs?name=snippet_OnGetAsync&highlight=8)]

Zastąp metodę `OnGetAsync` poniższym kodem, aby odczytywać dane rejestracyjne dla wybranego ucznia. Zmiany są wyróżnione.

[!code-csharp[Main](intro/samples/cu30/Pages/Students/Details.cshtml.cs?name=snippet_OnGetAsync&highlight=8-12)]

Metody [include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) i [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) powodują, że kontekst załaduje właściwość nawigacji `Student.Enrollments` i w każdej rejestracji `Enrollment.Course` właściwość nawigacji. Te metody są szczegółowo opisane w samouczku [odczytywanie powiązanych danych](xref:data/ef-rp/read-related-data) .

Metoda [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) zwiększa wydajność w scenariuszach, w których zwrócone jednostki nie są aktualizowane w bieżącym kontekście. `AsNoTracking` omówiono w dalszej części tego samouczka.

### <a name="display-enrollments"></a>Wyświetl rejestracje

Zastąp kod w obszarze *Pages/Students/details. cshtml* następującym kodem, aby wyświetlić listę rejestracji. Zmiany są wyróżnione.

[!code-cshtml[Main](intro/samples/cu30/Pages/Students/Details.cshtml?highlight=32-53)]

Poprzedni kod pętle za pomocą jednostek we właściwości nawigacji `Enrollments`. Dla każdej rejestracji jest wyświetlany tytuł kursu i Klasa. Tytuł kursu jest pobierany z jednostki kursu, która jest przechowywana we właściwości nawigacji `Course` jednostki rejestracji.

Uruchom aplikację, wybierz kartę **studenci** i kliknij link **szczegóły** dla ucznia. Zostanie wyświetlona lista kursów i ocen dla wybranego ucznia.

### <a name="ways-to-read-one-entity"></a>Sposoby odczytywania jednej jednostki

Wygenerowany kod używa [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) do odczytu jednej jednostki. Ta metoda zwraca wartość null, jeśli nic nie zostanie znalezione; w przeciwnym razie zwraca pierwszy znaleziony wiersz, który spełnia kryteria filtru zapytania. `FirstOrDefaultAsync` jest ogólnie lepszym rozwiązaniem niż następujące alternatywy:

* [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) — zgłasza wyjątek, jeśli istnieje więcej niż jedna jednostka, która spełnia kryteria filtru zapytań. Aby określić, czy zapytanie może zwrócić więcej niż jeden wiersz, `SingleOrDefaultAsync` próbuje pobrać wiele wierszy. Ta dodatkowa operacja jest niezbędna, jeśli zapytanie może zwrócić tylko jedną jednostkę, tak jak podczas wyszukiwania unikatowego klucza.
* [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) — umożliwia znalezienie jednostki z kluczem podstawowym (PK). Jeśli jednostka z PK jest śledzona przez kontekst, jest zwracana bez żądania do bazy danych. Ta metoda jest zoptymalizowana pod kątem wyszukiwania pojedynczej jednostki, ale nie można wywoływać `Include` z `FindAsync`.  Dlatego w przypadku konieczności pokrewnych danych `FirstOrDefaultAsync` jest lepszym wyborem.

### <a name="route-data-vs-query-string"></a>Dane trasy a ciąg zapytania

Adres URL strony szczegółów jest `https://localhost:<port>/Students/Details?id=1`. Wartość klucza podstawowego jednostki znajduje się w ciągu zapytania. Niektórzy deweloperzy wolą przekazać wartość klucza w polu dane trasy: `https://localhost:<port>/Students/Details/1`. Aby uzyskać więcej informacji, zobacz temat [Aktualizowanie wygenerowanego kodu](xref:tutorials/razor-pages/da1#update-the-generated-code).

## <a name="update-the-create-page"></a>Aktualizowanie strony tworzenia

Kod `OnPostAsync` szkieletowej dla strony tworzenia jest narażony na [nadpublikowanie](#overposting). Zastąp metodę `OnPostAsync` w obszarze *Pages/Students/Create. cshtml. cs* poniższym kodem.

[!code-csharp[Main](intro/samples/cu30/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>

### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

Poprzedni kod tworzy obiekt studenta, a następnie używa opublikowanych pól formularza do aktualizowania właściwości obiektu ucznia. Metoda [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) :

* Używa opublikowanych wartości formularza z właściwości [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) w [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).
* Aktualizuje tylko wymienione właściwości (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).
* Szuka pól formularza z prefiksem "Student". Na przykład `Student.FirstMidName`. Wielkość liter nie jest uwzględniana.
* Używa systemu [powiązań modelu](xref:mvc/models/model-binding) do konwersji wartości formularza z ciągów do typów w modelu `Student`. Na przykład `EnrollmentDate` musi być konwertowana na typ DateTime.

Uruchom aplikację i Utwórz jednostkę ucznia, aby przetestować stronę tworzenie.

## <a name="overposting"></a>Przefinalizowanie

Używanie `TryUpdateModel` do aktualizowania pól z opublikowanymi wartościami jest najlepszym rozwiązaniem w zakresie zabezpieczeń, ponieważ uniemożliwia przekazanie. Załóżmy na przykład, że jednostka ucznia zawiera właściwość `Secret`, którą ta strona sieci Web nie powinna aktualizować ani dodawać:

[!code-csharp[Main](intro/samples/cu30snapshots/2-crud/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

Nawet jeśli aplikacja nie ma pola `Secret` na stronie Tworzenie lub aktualizowanie Razor, haker może ustawić wartość `Secret` przez przepełnianie. Haker może użyć narzędzia, takiego jak programu Fiddler lub napisać kod JavaScript, aby opublikować wartość formularza `Secret`. Oryginalny kod nie ogranicza pól używanych przez spinacz modelu podczas tworzenia wystąpienia ucznia.

Niezależnie od wartości, która hakera określona dla pola formularza `Secret` zostanie zaktualizowana w bazie danych. Na poniższej ilustracji przedstawiono Narzędzie programu Fiddler, które dodaje pole `Secret` (z wartością "naddawaj") do wartości zaksięgowanych formularzy.

![Programu Fiddler Dodawanie pola tajnego](../ef-mvc/crud/_static/fiddler.png)

Wartość "Zaksięguj" została pomyślnie dodana do właściwości `Secret` wstawionego wiersza. Dzieje się tak, mimo że projektant aplikacji nigdy nie zamierzy właściwości `Secret` do ustawienia za pomocą strony Tworzenie.

### <a name="view-model"></a>Wyświetl model

Modele widoków zapewniają alternatywny sposób zapobiegania przepisywaniu.

Model aplikacji jest często nazywany modelem domeny. Model domeny zwykle zawiera wszystkie właściwości wymagane przez odpowiednią jednostkę w bazie danych. Model widoku zawiera tylko właściwości, które są niezbędne dla interfejsu użytkownika, który jest używany do (na przykład Strona tworzenia).

Poza modelem widoku niektóre aplikacje wykorzystują model powiązania lub model wejściowy do przekazywania danych między klasą Razor Pages modelu strony a przeglądarką. 

Rozważmy następujący `Student` model widoku:

[!code-csharp[Main](intro/samples/cu30snapshots/2-crud/Models/StudentVM.cs)]

Poniższy kod używa modelu widoku `StudentVM`, aby utworzyć nowego ucznia:

[!code-csharp[Main](intro/samples/cu30snapshots/2-crud/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

Metoda [setValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) ustawia wartości tego obiektu, odczytując wartości z innego obiektu [propertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) . `SetValues` używa dopasowywania nazw właściwości. Typ modelu widoku nie musi być powiązany z typem modelu, dlatego musi mieć już pasujące właściwości.

Za pomocą `StudentVM` należy zaktualizować [Create. cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/2-crud/Pages/Students/CreateVM.cshtml) do użycia `StudentVM` zamiast `Student`.

## <a name="update-the-edit-page"></a>Zaktualizuj strony edytowania

Na *stronie/Students/Edit. cshtml. cs*zastąp `OnGetAsync` i `OnPostAsync` metodami poniższym kodem.

[!code-csharp[Main](intro/samples/cu30/Pages/Students/Edit.cshtml.cs?name=snippet_OnGetPost)]

Zmiany w kodzie są podobne do strony tworzenie z kilkoma wyjątkami:

* `FirstOrDefaultAsync` został zastąpiony [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync). Gdy nie musisz dołączać powiązanych danych, `FindAsync` jest bardziej wydajne.
* `OnPostAsync` ma parametr `id`.
* Bieżący student jest pobierany z bazy danych, a nie od tworzenia pustego ucznia.

Uruchom aplikację i przetestuj ją, tworząc i edytując ucznia.

## <a name="entity-states"></a>Stany jednostek

Kontekst bazy danych śledzi, czy jednostki w pamięci są zsynchronizowane z odpowiadającymi im wierszami w bazie danych. Te informacje śledzenia decydują o tym, co się stanie w przypadku wywołania [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) . Na przykład gdy nowa jednostka jest przenoszona do metody [addasync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) , stan tej jednostki jest ustawiany na wartość [dodana](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added). Gdy `SaveChangesAsync` jest wywoływana, kontekst bazy danych wystawia polecenie SQL INSERT.

Jednostka może być w jednym z [następujących stanów](/dotnet/api/microsoft.entityframeworkcore.entitystate):

* `Added`: jednostka jeszcze nie istnieje w bazie danych. Metoda `SaveChanges` wystawia instrukcję INSERT.

* `Unchanged`: nie trzeba zapisywać zmian w tej jednostce. Jednostka ma ten stan, gdy jest odczytywany z bazy danych.

* `Modified`: zmodyfikowano niektóre lub wszystkie wartości właściwości jednostki. Metoda `SaveChanges` wystawia instrukcję UPDATE.

* `Deleted`: jednostka została oznaczona do usunięcia. Metoda `SaveChanges` wystawia instrukcję DELETE.

* `Detached`: jednostka nie jest śledzona przez kontekst bazy danych.

W aplikacji klasycznej zmiany stanu są zazwyczaj ustawiane automatycznie. Zostanie odczytana jednostka, wprowadzono zmiany, a stan jednostki zostanie automatycznie zmieniony na `Modified`. Wywołanie `SaveChanges` generuje instrukcję SQL UPDATE, która aktualizuje tylko zmienione właściwości.

W aplikacji sieci Web `DbContext`, która odczytuje jednostkę i wyświetla dane, są usuwane po wyrenderowaniu strony. Gdy wywoływana jest metoda `OnPostAsync` strony, tworzone jest nowe żądanie sieci Web i nowe wystąpienie `DbContext`. Odczytanie jednostki w tym nowym kontekście symuluje przetwarzanie pulpitu.

## <a name="update-the-delete-page"></a>Aktualizuj stronę Delete

W tej sekcji zaimplementowano niestandardowy komunikat o błędzie w przypadku niepowodzenia wywołania `SaveChanges`.

Zastąp kod w obszarze *Pages/Students/Delete. cshtml. cs* poniższym kodem. Zmiany są wyróżnione (inne niż oczyszczanie instrukcji `using`).

[!code-csharp[Main](intro/samples/cu30/Pages/Students/Delete.cshtml.cs?name=snippet_All&highlight=20,22,30,38-41,53-71)]

Poprzedni kod dodaje opcjonalny parametr `saveChangesError` do sygnatury metody `OnGetAsync`. `saveChangesError` wskazuje, czy metoda została wywołana po niepowodzeniu usunięcia obiektu ucznia. Operacja usuwania może zakończyć się niepowodzeniem z powodu przejściowych problemów z siecią. Przejściowe błędy sieciowe są bardziej prawdopodobnie, gdy baza danych znajduje się w chmurze. Parametr `saveChangesError` ma wartość false, gdy zostanie wywołana `OnGetAsync` usuwania strony z interfejsu użytkownika. Gdy `OnGetAsync` jest wywoływana przez `OnPostAsync` (ponieważ operacja usuwania nie powiodła się), parametr `saveChangesError` ma wartość true.

Metoda `OnPostAsync` pobiera wybraną jednostkę, a następnie wywołuje metodę [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) w celu ustawienia stanu jednostki na `Deleted`. Po wywołaniu `SaveChanges` zostanie wygenerowane polecenie SQL DELETE. Jeśli `Remove` nie powiedzie się:

* Przechwycono wyjątek bazy danych.
* Metoda `OnGetAsync` usuwania stron jest wywoływana z `saveChangesError=true`.

Dodaj komunikat o błędzie do strony usuwania Razor (*Pages/Students/Delete. cshtml*):

[!code-cshtml[Main](intro/samples/cu30/Pages/Students/Delete.cshtml?highlight=10)]

Uruchom aplikację i Usuń uczniów, aby przetestować stronę usuwania.

## <a name="next-steps"></a>Następne kroki

> [!div class="step-by-step"]
> [Poprzedni samouczek](xref:data/ef-rp/intro)
> [następnego samouczka](xref:data/ef-rp/sort-filter-page)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

W tym samouczku kod szkieletu CRUD (tworzenie, odczytywanie, aktualizowanie, usuwanie) zostanie sprawdzony i dostosowany.

Aby zminimalizować złożoność i utrzymywać te samouczki na EF Core, EF Core kod jest używany w modelach stron. Niektórzy Deweloperzy używają warstwy usług lub wzorca repozytorium w programie, aby utworzyć warstwę abstrakcji między interfejsem użytkownika (Razor Pages) i warstwą dostępu do danych.

W tym samouczku są badane Razor Pages tworzenia, edytowania, usuwania i szczegółów w folderze *uczniów* .

Kod szkieletowy używa następującego wzorca do tworzenia, edytowania i usuwania stron:

* Pobierz i Wyświetl żądane dane za pomocą metody HTTP GET `OnGetAsync`.
* Zapisz zmiany w danych za pomocą metody POST protokołu HTTP `OnPostAsync`.

Strony indeks i szczegóły pobierają i wyświetlają żądane dane przy użyciu metody HTTP GET `OnGetAsync`

## <a name="singleordefaultasync-vs-firstordefaultasync"></a>SingleOrDefaultAsync a FirstOrDefaultAsync

Wygenerowany kod używa [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), który jest ogólnie preferowany względem [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).

 `FirstOrDefaultAsync` jest bardziej wydajne niż `SingleOrDefaultAsync` przy pobieraniu jednej jednostki:

* Chyba że kod musi sprawdzić, czy nie ma więcej niż jednej jednostki zwróconej przez zapytanie.
* `SingleOrDefaultAsync` pobiera więcej danych i wykonuje niepotrzebne działania.
* `SingleOrDefaultAsync` zgłasza wyjątek, jeśli istnieje więcej niż jedna jednostka, która pasuje do części filtru.
* `FirstOrDefaultAsync` nie zostanie zgłoszony, jeśli istnieje więcej niż jedna jednostka, która pasuje do części filtru.

<a name="FindAsync"></a>

### <a name="findasync"></a>FindAsync

W większości kodu szkieletowego można używać [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) zamiast `FirstOrDefaultAsync`.

`FindAsync`:

* Znajduje jednostkę z kluczem podstawowym (PK). Jeśli jednostka z PK jest śledzona przez kontekst, jest zwracana bez żądania do bazy danych.
* Jest proste i zwięzłe.
* Jest zoptymalizowany pod kątem wyszukiwania pojedynczej jednostki.
* Mogą mieć w niektórych sytuacjach korzyści z wydajności, ale rzadko zdarzają się w przypadku typowych aplikacji sieci Web.
* Niejawnie używa [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) zamiast [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).

Jeśli jednak chcesz `Include` inne jednostki, `FindAsync` nie jest już odpowiednie. Oznacza to, że może być konieczne porzucenie `FindAsync` i przechodzenie do zapytania w miarę postępu aplikacji.

## <a name="customize-the-details-page"></a>Dostosuj stronę szczegółów

Przejdź do strony `Pages/Students`. Linki **Edytuj**, **szczegóły**i **Usuń** są generowane przez [pomocnika tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) w pliku *Pages/Students/index. cshtml* .

[!code-cshtml[](intro/samples/cu21/Pages/Students/Index1.cshtml?name=snippet)]

Uruchom aplikację i wybierz łącze **szczegóły** . Adres URL ma postać `http://localhost:5000/Students/Details?id=2`. Identyfikator ucznia jest przenoszona przy użyciu ciągu zapytania (`?id=2`).

Zaktualizuj Razor Pages Edytuj, szczegóły i Usuń, aby użyć szablonu trasy `"{id:int}"`. Zmień dyrektywę Page dla każdej z tych stron z `@page` na `@page "{id:int}"`.

Żądanie do strony z szablonem trasy "{ID: int}", który **nie** zawiera wartości trasy całkowitej zwraca błąd HTTP 404 (nie znaleziono). Na przykład `http://localhost:5000/Students/Details` zwraca błąd 404. Aby identyfikator był opcjonalny, Dołącz `?` do ograniczenia trasy:

 ```cshtml
@page "{id:int?}"
```

Uruchom aplikację, kliknij link szczegóły i sprawdź, czy adres URL przekazuje identyfikator jako dane trasy (`http://localhost:5000/Students/Details/2`).

Nie zmieniaj globalnie `@page` na `@page "{id:int}"`, ponieważ spowoduje to przerwanie linków do domu i tworzenie stron.

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>Dodaj powiązane dane

Kod szkieletu dla strony indeksu uczniów nie zawiera właściwości `Enrollments`. W tej sekcji zawartość kolekcji `Enrollments` zostanie wyświetlona na stronie szczegółów.

Metoda `OnGetAsync` *Pages/Students/details. cshtml. cs* używa metody `FirstOrDefaultAsync` do pobrania pojedynczej jednostki `Student`. Dodaj następujący wyróżniony kod:

[!code-csharp[](intro/samples/cu21/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

Metody [include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) i [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) powodują, że kontekst załaduje właściwość nawigacji `Student.Enrollments` i w każdej rejestracji `Enrollment.Course` właściwość nawigacji. Te metody są szczegółowo opisane w samouczku dotyczącym odczytywania danych.

Metoda [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) zwiększa wydajność w scenariuszach, gdy zwrócone jednostki nie są aktualizowane w bieżącym kontekście. `AsNoTracking` omówiono w dalszej części tego samouczka.

### <a name="display-related-enrollments-on-the-details-page"></a>Wyświetl powiązane rejestracje na stronie szczegółów

Otwórz *stronę/uczniów/szczegóły. cshtml*. Dodaj następujący wyróżniony kod, aby wyświetlić listę rejestracji:

[!code-cshtml[](intro/samples/cu21/Pages/Students/Details.cshtml?highlight=32-53)]

Jeśli Wcięcie kodu jest nieprawidłowe po wklejeniu kodu, naciśnij klawisze CTRL-K-D, aby je poprawić.

Poprzedni kod pętle za pomocą jednostek we właściwości nawigacji `Enrollments`. Dla każdej rejestracji jest wyświetlany tytuł kursu i Klasa. Tytuł kursu jest pobierany z jednostki kursu, która jest przechowywana we właściwości nawigacji `Course` jednostki rejestracji.

Uruchom aplikację, wybierz kartę **studenci** i kliknij link **szczegóły** dla ucznia. Zostanie wyświetlona lista kursów i ocen dla wybranego ucznia.

## <a name="update-the-create-page"></a>Aktualizowanie strony tworzenia

Zaktualizuj metodę `OnPostAsync` na *stronach/Students/Create. cshtml. cs* przy użyciu następującego kodu:

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>

### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

Obejrzyj kod [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) :

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

W poprzednim kodzie `TryUpdateModelAsync<Student>` próbuje zaktualizować obiekt `emptyStudent` przy użyciu wartości ogłoszonych formularza z właściwości [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) w [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel). `TryUpdateModelAsync` aktualizuje tylko wymienione właściwości (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).

W poprzednim przykładzie:

* Drugi argument (`"student", // Prefix`) jest prefiksem używanym do wyszukania wartości. Wielkość liter nie jest uwzględniana.
* Wartości opublikowanych formularzy są konwertowane na typy w modelu `Student` przy użyciu [powiązania modelu](xref:mvc/models/model-binding).

<a id="overpost"></a>

### <a name="overposting"></a>Przefinalizowanie

Używanie `TryUpdateModel` do aktualizowania pól z opublikowanymi wartościami jest najlepszym rozwiązaniem w zakresie zabezpieczeń, ponieważ uniemożliwia przekazanie. Załóżmy na przykład, że jednostka ucznia zawiera właściwość `Secret`, którą ta strona sieci Web nie powinna aktualizować ani dodawać:

[!code-csharp[](intro/samples/cu21/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

Nawet jeśli aplikacja nie ma pola `Secret` na stronie Utwórz/zaktualizuj Razor, haker może ustawić wartość `Secret` przez przepełnianie. Haker może użyć narzędzia, takiego jak programu Fiddler lub napisać kod JavaScript, aby opublikować wartość formularza `Secret`. Oryginalny kod nie ogranicza pól używanych przez spinacz modelu podczas tworzenia wystąpienia ucznia.

Niezależnie od wartości, haker określony dla pola formularza `Secret` zostanie zaktualizowany w bazie danych. Na poniższej ilustracji przedstawiono Narzędzie programu Fiddler, które dodaje pole `Secret` (z wartością "naddawaj") do wartości zaksięgowanych formularzy.

![Programu Fiddler Dodawanie pola tajnego](../ef-mvc/crud/_static/fiddler.png)

Wartość "Zaksięguj" została pomyślnie dodana do właściwości `Secret` wstawionego wiersza. Projektant aplikacji nigdy nie zamierzeniu właściwości `Secret` do ustawienia za pomocą strony Tworzenie.

<a name="vm"></a>

### <a name="view-model"></a>Wyświetl model

Model widoku zwykle zawiera podzbiór właściwości zawartych w modelu używanym przez aplikację. Model aplikacji jest często nazywany modelem domeny. Model domeny zwykle zawiera wszystkie właściwości wymagane przez odpowiednią jednostkę w bazie danych. Model widoku zawiera tylko właściwości, które są odpowiednie dla warstwy interfejsu użytkownika (na przykład Strona tworzenia). Poza modelem widoku niektóre aplikacje wykorzystują model powiązania lub model wejściowy do przekazywania danych między klasą Razor Pages modelu strony a przeglądarką. Rozważmy następujący `Student` model widoku:

[!code-csharp[](intro/samples/cu21/Models/StudentVM.cs)]

Modele widoków zapewniają alternatywny sposób zapobiegania przepisywaniu. Model widoku zawiera tylko właściwości do wyświetlenia (Display) lub Update.

Poniższy kod używa modelu widoku `StudentVM`, aby utworzyć nowego ucznia:

[!code-csharp[](intro/samples/cu21/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

Metoda [setValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) ustawia wartości tego obiektu, odczytując wartości z innego obiektu [propertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) . `SetValues` używa dopasowywania nazw właściwości. Typ modelu widoku nie musi być powiązany z typem modelu, dlatego musi mieć już pasujące właściwości.

Używanie `StudentVM` wymaga aktualizacji [CreateVM. cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) do użycia `StudentVM` zamiast `Student`.

W Razor Pages Klasa pochodna `PageModel` jest modelem widoku.

## <a name="update-the-edit-page"></a>Zaktualizuj strony edytowania

Zaktualizuj model strony dla strony Edycja. Najważniejsze zmiany są wyróżnione:

[!code-csharp[](intro/samples/cu21/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

Zmiany w kodzie są podobne do strony tworzenie z kilkoma wyjątkami:

* `OnPostAsync` ma opcjonalny parametr `id`.
* Bieżący student jest pobierany z bazy danych, a nie od tworzenia pustego ucznia.
* `FirstOrDefaultAsync` został zastąpiony [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync). `FindAsync` jest dobrym wyborem podczas wybierania jednostki z klucza podstawowego. Aby uzyskać więcej informacji, zobacz [FindAsync](#FindAsync) .

### <a name="test-the-edit-and-create-pages"></a>Testowanie stron Edycja i tworzenie

Utwórz i edytuj kilka jednostek ucznia.

## <a name="entity-states"></a>Stany jednostek

Kontekst bazy danych śledzi, czy jednostki w pamięci są zsynchronizowane z odpowiadającymi im wierszami w bazie danych. Informacje o synchronizacji kontekstu bazy danych określają, co się stanie w przypadku wywołania [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) . Na przykład gdy nowa jednostka jest przenoszona do metody [addasync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) , stan tej jednostki jest ustawiany na wartość [dodana](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added). Gdy `SaveChangesAsync` jest wywoływana, kontekst bazy danych wystawia polecenie SQL INSERT.

Jednostka może być w jednym z [następujących stanów](/dotnet/api/microsoft.entityframeworkcore.entitystate):

* `Added`: jednostka jeszcze nie istnieje w bazie danych. Metoda `SaveChanges` wystawia instrukcję INSERT.

* `Unchanged`: nie trzeba zapisywać zmian w tej jednostce. Jednostka ma ten stan, gdy jest odczytywany z bazy danych.

* `Modified`: zmodyfikowano niektóre lub wszystkie wartości właściwości jednostki. Metoda `SaveChanges` wystawia instrukcję UPDATE.

* `Deleted`: jednostka została oznaczona do usunięcia. Metoda `SaveChanges` wystawia instrukcję DELETE.

* `Detached`: jednostka nie jest śledzona przez kontekst bazy danych.

W aplikacji klasycznej zmiany stanu są zazwyczaj ustawiane automatycznie. Odczytano jednostkę, wprowadzono zmiany i stan jednostki do automatycznego zmiany na `Modified`. Wywołanie `SaveChanges` generuje instrukcję SQL UPDATE, która aktualizuje tylko zmienione właściwości.

W aplikacji sieci Web `DbContext`, która odczytuje jednostkę i wyświetla dane, są usuwane po wyrenderowaniu strony. Gdy wywoływana jest metoda `OnPostAsync` strony, tworzone jest nowe żądanie sieci Web i nowe wystąpienie `DbContext`. Odczytanie jednostki w tym nowym kontekście symuluje przetwarzanie pulpitu.

## <a name="update-the-delete-page"></a>Aktualizuj stronę Delete

W tej sekcji kod został dodany w celu zaimplementowania niestandardowego komunikatu o błędzie w przypadku niepowodzenia wywołania `SaveChanges`. Dodaj ciąg, aby zawierał możliwe komunikaty o błędach:

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

Zastąp metodę `OnGetAsync` poniższym kodem:

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

Poprzedni kod zawiera opcjonalny parametr `saveChangesError`. `saveChangesError` wskazuje, czy metoda została wywołana po niepowodzeniu usunięcia obiektu ucznia. Operacja usuwania może zakończyć się niepowodzeniem z powodu przejściowych problemów z siecią. Przejściowe błędy sieciowe są bardziej podobne do chmury. `saveChangesError`ma wartość false, gdy zostanie wywołana `OnGetAsync` usuwania strony z interfejsu użytkownika. Gdy `OnGetAsync` jest wywoływana przez `OnPostAsync` (ponieważ operacja usuwania nie powiodła się), parametr `saveChangesError` ma wartość true.

### <a name="the-delete-pages-onpostasync-method"></a>Metoda Delete Pages OnPostAsync

Zastąp `OnPostAsync` następującym kodem:

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

Powyższy kod pobiera wybraną jednostkę, a następnie wywołuje metodę [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) w celu ustawienia stanu jednostki na `Deleted`. Po wywołaniu `SaveChanges` zostanie wygenerowane polecenie SQL DELETE. Jeśli `Remove` nie powiedzie się:

* Przechwycono wyjątek bazy danych.
* Metoda `OnGetAsync` usuwania stron jest wywoływana z `saveChangesError=true`.

### <a name="update-the-delete-razor-page"></a>Aktualizowanie strony usuwania Razor

Dodaj następujący wyróżniony komunikat o błędzie na stronie Usuwanie Razor.
<!--
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?name=snippet&highlight=11)]
-->
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

Test Delete.

## <a name="common-errors"></a>Typowe błędy

Studenci/indeks lub inne linki nie działają:

Sprawdź, czy strona Razor zawiera poprawną dyrektywę `@page`. Na przykład strona "uczniowie/index Razor" **nie** powinna zawierać szablonu trasy:

```cshtml
@page "{id:int}"
```

Każda Strona Razor musi zawierać dyrektywę `@page`.



## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wersja tego samouczka usługi YouTube](https://www.youtube.com/watch?v=K4X1MT2jt6o)

> [!div class="step-by-step"]
> [Poprzednie](xref:data/ef-rp/intro)
> [dalej](xref:data/ef-rp/sort-filter-page)

::: moniker-end
