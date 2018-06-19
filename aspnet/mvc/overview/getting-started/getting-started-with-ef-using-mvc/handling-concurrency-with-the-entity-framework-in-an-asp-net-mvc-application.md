---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Obsługa współbieżności Entity Framework 6 w aplikacji platformy ASP.NET MVC 5 (10 12) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b92aded80ad6b435a2409a137bb96fe4d0a726f4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875659"
---
<a name="handling-concurrency-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-10-of-12"></a>Obsługa współbieżności Entity Framework 6 w aplikacji platformy ASP.NET MVC 5 (10 12)
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [pobierania plików PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio 2013. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


W starszych samouczkach przedstawiono sposób aktualizowania danych. Ten samouczek pokazuje sposób obsługi konfliktów w przypadku wielu użytkowników aktualizacji tej samej jednostki w tym samym czasie.

Użytkownik zmieni stron sieci web, które współpracują z `Department` jednostki tak, aby ich obsługi błędów współbieżności. Na poniższych ilustracjach przedstawiono strony indeksu i Delete, takie jak komunikaty, które są wyświetlane, gdy wystąpi konflikt współbieżności.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Konfliktom współbieżności

Występuje konflikt współbieżności, gdy jeden użytkownik wyświetla dane jednostki celu jego edycji, a następnie inny użytkownik aktualizuje dane tej samej jednostki przed zapisaniem zmian pierwszego użytkownika do bazy danych. Nie włączaj wykrywania takie konflikty, ostatnio kto aktualizuje bazę danych zastępuje zmiany wprowadzone przez użytkownika. W wielu aplikacjach to zagrożenie jest dopuszczalne: w przypadku kilku użytkowników lub kilka aktualizacji lub jeśli nie są naprawdę krytyczne, jeśli pewne zmiany zostaną zastąpione, kosztów programowania dla współbieżności może przeważają korzyści. W takim przypadku nie trzeba skonfigurować aplikację do obsługi konfliktom współbieżności.

### <a name="pessimistic-concurrency-locking"></a>Pesymistyczne współbieżności (blokowanie)

Jeśli aplikacja potrzeba uniknąć przypadkowej utraty danych w scenariuszach współbieżności, jeden sposób jest użycie blokady bazy danych. Ta metoda jest wywoływana *współbieżności pesymistyczne*. Na przykład przed przeczytaniem wiersz z bazy danych, możesz zażądać blokady dla tylko do odczytu lub aktualizacji dostępu. Jeśli zablokujesz wiersza dla dostępu do aktualizacji, inni użytkownicy nie mogą zablokować wiersza dla tylko do odczytu lub aktualizacji dostępu, ponieważ uzyskują kopii danych, która jest w trakcie zmieniane. Jeśli zablokujesz wiersz dostęp tylko do odczytu, inne można również zablokować go uzyskać dostęp tylko do odczytu, ale nie dla aktualizacji.

Zarządzanie blokad ma wady. Może być skomplikowane, aby program. Wymaga to znaczące bazy danych zarządzania zasobami i może spowodować problemy z wydajnością jako liczbę użytkowników aplikacji zwiększa. Z tego względu nie wszystkie systemy zarządzania bazy danych obsługują pesymistyczne współbieżności. Entity Framework nie zapewnia wbudowanej obsługi dla niego, a w tym samouczku nie pokazuje, jak ją wdrożyć.

### <a name="optimistic-concurrency"></a>Optymistycznej współbieżności

Alternatywą dla pesymistyczne współbieżności jest *optymistycznej współbieżności*. Optymistycznej współbieżności oznacza stosowanie konfliktom współbieżności mieć miejsce, a następnie reaguje odpowiednio Jeśli. Na przykład Jan uruchamia strony Edytuj działów zmiany **budżetu** kwotę dla angielskiej wersji językowej działu z 350,000.00 $ $ 0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Zanim Jan klika **zapisać**, Magdalena uruchamia te same strony i zmiany **Data rozpoczęcia** pola 1-9/2007 2013-8/8.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Jan klika **zapisać** pierwszy i widzi kliknie jego zmiany, gdy przeglądarka powróci do strony indeksu, a następnie Magdalena **zapisać**. Co dalej zależy od sposobu obsługi konfliktom współbieżności. Niektóre opcje są następujące:

- Można zachować informacje o właściwości, które zostało zmodyfikowane przez użytkownika i aktualizować tylko odpowiednie kolumny w bazie danych. W przykładowym scenariuszu żadne dane nie byłoby utracone, ponieważ inne właściwości zostały zaktualizowane przez użytkowników. Przy następnym ktoś przegląda w angielskiej wersji językowej działu, będzie zobaczą zmiany zarówno w Jan i Joanny — Data rozpoczęcia 8/8/2013 i budżetu dolarów Zero.

    Ta metoda aktualizacji może zmniejszyć liczbę konfliktów, które może spowodować utratę danych, ale nie można uniknąć utraty danych, jeśli konkurują zmian z tą samą właściwością jednostki. Czy programu Entity Framework działa w ten sposób zależy od sposobu implementacji kodu aktualizacji. Często nie jest praktyczne w aplikacji sieci web, ponieważ może wymagać, obsługa dużych ilości stanu celu śledzenia wszystkich oryginalnej wartości właściwości dla obiektu, a także nowe wartości. Obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji ponieważ go wymaga zasobów serwera lub muszą być zawarte w strony sieci web (na przykład w pola ukryte) lub w pliku cookie.
- Możesz pozwolić, aby zastąpić zmiany w Jan zmiany nazwy. Przy następnym ktoś przegląda w angielskiej wersji językowej działu, będzie zobaczy 8/8/2013 i przywrócone wartości $350,000.00. Ta metoda jest wywoływana *klienta Wins* lub *ostatniego w usłudze Wins* scenariusza. (Wszystkie wartości z klienta wyższy priorytet niż co znajduje się w magazynie danych). Zgodnie z opisem w wprowadzenie do tej sekcji, w przeciwnym razie pisania kodu do obsługi współbieżności, nastąpi to automatycznie.
- Aby uniemożliwić zmianę nazwy aktualizację w bazie danych. Zazwyczaj będzie wyświetlony komunikat o błędzie, wyświetlić jej bieżący stan danych i umożliwia jej Zastosuj ponownie swoje zmiany, jeśli chce nadal były. Ta metoda jest wywoływana *Wins magazynu* scenariusza. (Wartości magazynu danych mają priorytet nad wartości przesłany przez klienta). W tym samouczku będziesz wdrożyć scenariusz dla magazynu usługi Wins. Ta metoda gwarantuje, że żadne zmiany nie zostaną zastąpione bez użytkownika są alerty o tym, co dzieje.

### <a name="detecting-concurrency-conflicts"></a>Wykrywanie konfliktów współbieżności

Obsługa może rozwiązać konflikty [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) wyjątki, które generuje programu Entity Framework. Aby wiedzieć, kiedy throw te wyjątki, Entity Framework musi mieć możliwość wykrywania konfliktów. W związku z tym musisz skonfigurować bazę danych i modelu danych odpowiednio. Niektóre opcje umożliwiających wykrywanie konfliktów są następujące:

- W tabeli bazy danych należy dołączyć kolumny śledzenia, który może służyć do określania, kiedy wiersz został zmieniony. Następnie można skonfigurować programu Entity Framework w celu uwzględnienia tej kolumny `Where` klauzuli SQL `Update` lub `Delete` poleceń.

    Typ danych kolumny śledzenia jest zwykle [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) wartość jest liczbą sekwencyjnych, który jest zwiększany po każdej zaktualizować wiersza. W `Update` lub `Delete` polecenia `Where` klauzula zawiera oryginalnej wartości kolumny śledzenia (oryginalnej wersji wierszy). Jeśli aktualizacji wiersza został zmieniony przez innego użytkownika, wartość w `rowversion` kolumny różni się od oryginalnej wartości, więc `Update` lub `Delete` instrukcji nie można odnaleźć wiersza do aktualizacji z powodu `Where` klauzuli. Gdy programu Entity Framework stwierdzi, że żadne wiersze nie zostały zaktualizowane przez `Update` lub `Delete` polecenia (Jeśli liczba wierszy, których dotyczy to zero), interpretuje który jako konflikt współbieżności.
- Konfigurowanie programu Entity Framework, aby uwzględnić oryginalnych wartości wszystkich kolumn w tabeli w `Where` klauzuli `Update` i `Delete` poleceń.

    Jak pierwsza opcja, jeśli dowolny wiersz zmieniła się od najpierw odczytano wiersza `Where` klauzuli nie zwrócą wiersza do aktualizacji, które programu Entity Framework interpretowane jako konflikt współbieżności. Dla tabel bazy danych, które mają wiele kolumn, ta metoda może spowodować bardzo dużych `Where` klauzule i może wymagać, aby zachować stan dużych ilości. Jak wspomniano wcześniej, obsługa dużych ilości stan może mieć wpływ na wydajność aplikacji. W związku z tym tej metody zwykle nie jest zalecane, a nie jest ona metodę używaną w tym samouczku.

    Jeśli chcesz wdrożyć takie podejście do współbieżności, masz Oznacz wszystkie właściwości bez klucza podstawowego w jednostce chcesz śledzić concurrency przez dodanie [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) atrybutu do nich. Czy zmiana umożliwia programu Entity Framework uwzględnić wszystkie kolumny w SQL `WHERE` klauzuli `UPDATE` instrukcje.

W pozostałej części tego samouczka zostanie dodana [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) śledzenia dla właściwości `Department` jednostki, Tworzenie kontrolera i widoków i sprawdzenie, czy wszystko działa prawidłowo.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Dodaj właściwość optymistycznej współbieżności do działu jednostki

W *Models\Department.cs*, Dodaj właściwość śledzenia o nazwie `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

[Sygnatury czasowej](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) atrybut określa, czy mają być uwzględnieni w tej kolumnie w `Where` klauzuli `Update` i `Delete` polecenia wysyłane do bazy danych. Ten atrybut jest nazywany [sygnatury czasowej](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) ponieważ poprzednie wersje programu SQL Server SQL [sygnatury czasowej](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) — typ danych przed SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) on zastąpiony. Typ architektury .net dla [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) jest tablicą bajtów.

Jeśli wolisz korzystać z interfejsu API fluent, możesz użyć [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) metodę, aby określić właściwości śledzenia, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Przez dodanie właściwości po zmianie modelu bazy danych, więc należy przeprowadzić migrację z innej. W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenia:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-the-department-controller"></a>Modyfikowanie kontrolera działu

W *Controllers\DepartmentController.cs*, Dodaj `using` instrukcji:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

W *DepartmentController.cs* pliku, zmienić wszystkie cztery wystąpienia "Nazwisko" na "Pełna nazwa", tak aby list rozwijanych administratora działu będzie zawierać pełną nazwę instruktora, a nie tylko nazwisko.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Zastąp istniejący kod `HttpPost` `Edit` metodę z następującym kodem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Jeśli `FindAsync` metoda zwraca wartość null, dział został usunięty przez innego użytkownika. Pokazano kod używa wartości przesłanego formularza utworzyć jednostki działu, tak aby edycji strony mogą być wyświetlane ponownie z komunikatem o błędzie. Alternatywnie nie trzeba ponownie utworzyć jednostki działu, jeśli wyświetla komunikat o błędzie bez ponowne wyświetlanie pola działu.

Widok przechowuje oryginalnej `RowVersion` wartość w ukrytym polu i metody odbiera w `rowVersion` parametru. Przed wywołaniem `SaveChanges`, trzeba umieścić który oryginalnego `RowVersion` wartości właściwości w `OriginalValues` kolekcji jednostki. Gdy programu Entity Framework utworzy SQL `UPDATE` poleceń, że polecenie będzie zawierać `WHERE` klauzuli sprawdzający wiersza, który ma oryginalną `RowVersion` wartość.

Jeśli wpływają na żadne wiersze `UPDATE` polecenia (żadnych wierszy ma oryginalną `RowVersion` wartość), zgłasza programu Entity Framework `DbUpdateConcurrencyException` wyjątku i kodu w `catch` bloku pobiera dotkniętych `Department` jednostek z tego wyjątku obiekt.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Ten obiekt zawiera nowe wartości wprowadzonej przez użytkownika w jego `Entity` właściwości oraz można uzyskać wartości odczytane z bazy danych przez wywołanie metody `GetDatabaseValues` metody.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

`GetDatabaseValues` Metoda zwraca wartość null, jeśli ktoś usunął wiersz z bazy danych; w przeciwnym razie należy rzutować zwróconego obiektu na `Department` klasy w celu uzyskania dostępu do `Department` właściwości. (Ponieważ jest już zaznaczone do usunięcia, `databaseEntry` może mieć wartości null, tylko wtedy, gdy dział został usunięty po `FindAsync` wykonuje i przed `SaveChanges` wykonuje.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Następnie kod dodaje niestandardowy komunikat o błędzie dla każdej kolumny z wartościami bazy danych jest inny niż użytkownik wprowadził na stronie edycji:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Komunikat o błędzie dłużej wyjaśniono, co się stało i co należy zrobić informacji na ten temat:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Na koniec kod ustawia `RowVersion` wartość `Department` obiektu na nową wartość pobrane z bazy danych. Nowy `RowVersion` wartości będą przechowywane w ukrytym polu podczas edycji strony zostanie wyświetlony ponownie, a następne czasu użytkownik klika polecenie **zapisać**, tylko błędy współbieżności, które się zdarzyć, ponieważ ponowne wyświetlanie edycji strony zostanie przechwycony.

W *Views\Department\Edit.cshtml*, Dodaj ukryte pole, aby zapisać `RowVersion` wartość właściwości, natychmiast po ukryte pole umożliwiające `DepartmentID` właściwości:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="testing-optimistic-concurrency-handling"></a>Testowanie Obsługa optymistycznej współbieżności

Uruchom witrynę i kliknij przycisk **działów**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Kliknij prawym przyciskiem myszy **Edytuj** hiperłącze dla działu angielskiej wersji językowej i wybierz **Otwórz na nowej karcie** następnie kliknij przycisk **Edytuj** hyperlink działu angielskiej wersji językowej. Dwie karty wyświetlenia tych samych informacji.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Zmień pola w pierwszej karcie przeglądarki i kliknij **zapisać**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Przeglądarka wyświetla stronę indeksu o wartości zmienione.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Zmień pola w drugiej karty przeglądarki, a następnie kliknij przycisk **zapisać**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Kliknij przycisk **zapisać** w drugiej karty przeglądarki. Zostanie wyświetlony komunikat o błędzie:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Kliknij przycisk **zapisać** ponownie. Wartość wprowadzona w drugiej karty przeglądarki są zapisywane wraz z oryginalnej wartości danych, które można zmienić w przeglądarce pierwszy. Zostanie wyświetlony zapisanych wartości, gdy zostanie wyświetlona strona indeksu.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aktualizowanie strony usuwania

Na stronie usuwania programu Entity Framework wykrywa konfliktom współbieżności spowodowane przez kogoś else edycji dział w podobny sposób. Gdy `HttpGet` `Delete` metoda Wyświetla widok potwierdzenie, widok zawiera oryginalny `RowVersion` wartość w polu ukrytym. Wartość jest następnie udostępniana `HttpPost` `Delete` metodę, która jest wywoływana, gdy użytkownik potwierdza usunięcie. Gdy programu Entity Framework utworzy SQL `DELETE` polecenia zawiera `WHERE` klauzuli z oryginalnym `RowVersion` wartość. Jeśli wyniki poleceń w żadnych wierszy dotyczy (znaczenie wiersz został zmieniony po stronie potwierdzenia usunięcia została wyświetlona), zwracany jest wyjątek współbieżności i `HttpGet Delete` metoda jest wywoływana z flagą błąd ustawioną na `true` Aby ponownie wyświetlić Strona potwierdzenia z komunikatem o błędzie. Istnieje również możliwość, że zero zmienionych wierszy, ponieważ wiersz został usunięty przez innego użytkownika, więc w takim przypadku zostanie wyświetlony komunikat inny błąd.

W *DepartmentController.cs*, Zastąp `HttpGet` `Delete` metodę z następującym kodem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Metoda przyjmuje opcjonalny parametr, który wskazuje, czy strona jest są wyświetlane ponownie po błędzie współbieżności. Jeśli ta flaga jest `true`, komunikat o błędzie jest wysyłany do widoku przy użyciu `ViewBag` właściwości.

Zastąp kod w `HttpPost` `Delete` — metoda (o nazwie `DeleteConfirmed`) z następującym kodem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

W kodzie szkieletu po prostu zastąpić ta metoda zaakceptowane identyfikator rekordu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Ten parametr, aby po zmianie `Department` utworzonego przez obiekt wiążący modelu wystąpienia jednostki. Daje dostęp do `RowVersion` wartość właściwości oprócz klucza rekordu.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Również zmieniono nazwę metody akcji `DeleteConfirmed` do `Delete`. Kod z utworzonym szkieletem o nazwie `HttpPost` `Delete` metody `DeleteConfirmed` umożliwiają `HttpPost` metody unikatowego podpisu. (CLR wymaga przeciążonej metody mają parametry innej metody). Teraz, czy podpisy są unikatowe, możesz przestrzegaj Konwencji MVC i użyć takiej samej nazwy `HttpPost` i `HttpGet` metody zostaną usunięte.

Jeśli zostanie przechwycony błąd współbieżności, kod zostanie ponownie stronę potwierdzenia Delete i udostępnia Flaga, która wskazuje, że powinien być wyświetlany komunikat o błędzie współbieżności.

W *Views\Department\Delete.cshtml*, Zastąp następujący kod, który dodaje błąd pola wiadomości i ukryte pola dla właściwości DepartmentID i RowVersion szkieletu kodu. Zmiany zostały wyróżnione.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

Ten kod dodaje komunikat o błędzie między `h2` i `h3` nagłówki:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Zastępuje `LastName` z `FullName` w `Administrator` pola:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Na koniec dodaje ukryte pola dla `DepartmentID` i `RowVersion` właściwości po `Html.BeginForm` instrukcji:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

Uruchom strony indeksu działów. Kliknij prawym przyciskiem myszy **usunąć** hiperłącze dla działu angielskiej wersji językowej i wybierz **Otwórz na nowej karcie** następnie na karcie pierwszy kliknij **Edytuj** hyperlink działu angielskiej wersji językowej.

W pierwszym oknie zmienić jedną z wartości, a następnie kliknij przycisk **zapisać** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

Strona indeksu potwierdza zmianę.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Na karcie drugi kliknij **usunąć**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Zostanie wyświetlony komunikat o błędzie współbieżności, a dział wartości są odświeżane co to jest obecnie w bazie danych.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Jeśli klikniesz przycisk **usunąć** ponownie, że przekierowanie do strony indeksu, który pokazuje, czy dział została usunięta.

## <a name="summary"></a>Podsumowanie

Na tym kończy się wprowadzenie do obsługi konfliktom współbieżności. Aby uzyskać informacje na temat innych do obsługi różnych scenariuszy współbieżności, zobacz [optymistycznej współbieżności wzorce](https://msdn.microsoft.com/data/jj592904) i [Praca z wartościami właściwości](https://msdn.microsoft.com/data/jj592677) w witrynie MSDN. Następny samouczek pokazuje, jak zaimplementować tabeli na hierarchii dziedziczenia dla `Instructor` i `Student` jednostek.

Linki do innych zasobów programu Entity Framework, można znaleźć w [dostępu do danych programu ASP.NET - zalecane zasobów](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [dalej](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
