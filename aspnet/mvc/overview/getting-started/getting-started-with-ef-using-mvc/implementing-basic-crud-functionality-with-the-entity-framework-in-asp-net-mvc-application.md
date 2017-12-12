---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementowanie funkcji Basic CRUD z programu Entity Framework w aplikacji ASP.NET MVC | Dokumentacja firmy Microsoft
author: tdykstra
description: "Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/09/2015
ms.topic: article
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: c63b8f591023b68720c523d1c9184a527a34e9cc
ms.sourcegitcommit: e4fb6b13be56a0fb2f2778623740a047d6489227
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application"></a>Implementowanie funkcji Basic CRUD z programu Entity Framework w aplikacji ASP.NET MVC
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) lub [pobierania plików PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 5 za pomocą Entity Framework 6 Code First i Visual Studio 2013. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


W poprzednich samouczek utworzono aplikację MVC, która przechowuje i wyświetla danych przy użyciu programu Entity Framework i bazy danych LocalDB programu SQL Server. W tym samouczku należy przejrzeć i dostosować CRUD (tworzenia, odczytu, aktualizowanie i usuwanie) kodu, który będzie szkieletów MVC automatycznie tworzy w kontrolery i widoki.

> [!NOTE]
> Jest typowym rozwiązaniem implementacji klienta wzorca repozytorium, aby można było utworzyć warstwę abstrakcji między kontrolerem a warstwa dostępu do danych. Aby zachować te samouczki proste i skupiają się na nauczania sposobu korzystania z programu Entity Framework samej siebie, nie używają repozytoriów. Informacje o sposobie wdrożenia repozytoriów, zobacz [Mapa zawartości dostępu do danych programu ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).


W tym samouczku utworzysz następujących stron sieci web:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

## <a name="create-a-details-page"></a>Tworzenie strony szczegółów

Kod z utworzonym szkieletem dla uczniów lub studentów `Index` po lewej stronie `Enrollments` właściwości, ponieważ kolekcja zawiera tej właściwości. W `Details` strony będzie wyświetlać zawartość kolekcji, w tabeli HTML.

 W *Controllers\StudentController.cs*, metoda akcji `Details` wyświetlić używa [znaleźć](https://msdn.microsoft.com/en-us/library/gg696418(v=VS.103).aspx) metoda pobierania pojedynczy `Student` jednostki. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

Wartość tego klucza jest przekazywany do metody jako `id` parametru i pochodzi z *przekazywanie danych* w **szczegóły** hiperłącza na stronie indeksu.

### <a name="tip-route-data"></a>Porada: **przekazywanie danych**

Dane trasy to dane integratora modelu znaleziono w segment adresu URL określonego w tabeli routingu. Na przykład określa domyślną trasę `controller`, `action`, i `id` segmentów:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

Następujący adres URL, mapuje trasy domyślnej `Instructor` jako `controller`, `Index` jako `action` a 1 jako `id`; są wartości danych trasy.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

"? courseID = 2021" jest wartość ciągu zapytania. Integrator modelu również będzie działać w przypadku przekazania `id` jako wartość ciągu kwerendy:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Adresy URL są tworzone przez `ActionLink` instrukcje w widoku Razor. W poniższym kodzie `id` parametru odpowiada trasy domyślnej tak `id` jest dodawany do danych trasy.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

W poniższym kodzie `courseID` nie jest zgodny z parametrem w trasy domyślnej, jest dodawany jako ciąg zapytania.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]


1. Otwórz *Views\Student\Details.cshtml*. Każde pole jest wyświetlane przy użyciu `DisplayFor` pomocnika, jak pokazano w poniższym przykładzie:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]
2. Po `EnrollmentDate` pól i bezpośrednio przed tagiem zamykającym `</dl>` tagów, Dodaj wyróżniony kod, aby wyświetlić listę rejestracji, jak pokazano w poniższym przykładzie:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Jeśli po wklejeniu kod wcięcia kodu o nieprawidłowej naciśnij kombinację klawiszy CTRL-K-D, aby naprawić błąd.

    Ten kod w pętli jednostek w `Enrollments` właściwości nawigacji. Dla każdego `Enrollment` jednostki we właściwości, Wyświetla tytuł kursu i kategorii. Tytuł kursu są pobierane z `Course` jednostki, która jest przechowywana w `Course` właściwość nawigacji `Enrollments` jednostki. Wszystkie te dane są pobierane z bazy danych automatycznie gdy jest to potrzebne. (Innymi słowy, jest używany podczas ładowania opóźnionego tutaj. Nie określono *wczesny ładowania* dla `Courses` właściwość nawigacji, dzięki rejestracji nie zostały pobrane w jednym zapytaniu, który uzyskano studenta. Zamiast tego podczas pierwszej próby uzyskania dostępu do `Enrollments` właściwość nawigacji, nowe zapytanie jest wysyłane do pobierania danych do bazy danych. Więcej o opóźnionego ładowania i ładowanie wczesny w [dane dotyczące odczytywania](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczek później w tej serii.)
3. Uruchom strony, wybierając **studentów** kartę i klikając **szczegóły** łącze Alexander Carson. (Po naciśnięciu klawiszy CTRL + F5 otwarciu pliku Details.cshtml, zostanie wyświetlony błąd HTTP 400 ponieważ próbuje uruchomić na stronie Szczegóły programu Visual Studio, ale nie osiągnął z łącza określający student do wyświetlenia. In that Case, po prostu usuń "Studentów/szczegóły" z adresu URL i spróbuj ponownie, lub zamknij przeglądarkę, kliknij prawym przyciskiem myszy projekt i kliknij **widoku**, a następnie kliknij przycisk **Wyświetl w przeglądarce**.)

    Zostanie wyświetlona lista kursów i klasy dla wybranego uczniów:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="update-the-create-page"></a>Utwórz stronę aktualizacji

1. W *Controllers\StudentController.cs*, Zastąp `HttpPost``Create` metodę akcji za pomocą następujący kod, aby dodać `try-catch` blokować i Usuń `ID` z [atrybutu Bind](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx) dla Metoda szkieletu:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Ten kod dodaje `Student` jednostki utworzone przez program ASP.NET MVC integratora modelu do `Students` jednostki ustawiona, a następnie zapisuje zmiany w bazie danych. (*Integratora modelu* odwołuje się do funkcjonalność platformy ASP.NET MVC, który sprawia, że jej ułatwiają pracę z danych przesyłanych przez formularz; integratora modelu formularza zaksięgowany konwertuje wartości do typów CLR i przekazuje je do metody akcji w parametrach. In this case, tworzy wystąpienie integratora modelu `Student` jednostki przy użyciu właściwości wartości z `Form` kolekcji.)

    Możesz usunąć `ID` z powiązanego atrybutu, ponieważ `ID` jest wartość klucza podstawowego, której program SQL Server ustawi automatycznie, gdy zostanie wstawiona. Dane wejściowe użytkownika nie ustawia `ID` wartość.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Ostrzeżenie o zabezpieczeniach - `ValidateAntiForgeryToken` atrybutu pomaga zapobiegać [sfałszowaniem żądania między lokacjami](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) ataków. Wymaga to odpowiedniego `Html.AntiForgeryToken()` instrukcji w widoku, który pojawi się później.

    `Bind` Atrybut jest jednym ze sposobów ochrony przed *zbyt księgowej* w tworzenie scenariuszy. Załóżmy na przykład, `Student` zawiera jednostki `Secret` właściwości, których nie chcesz, aby ta strona sieci web, aby ustawić.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Nawet jeśli nie masz `Secret` pola na stronie sieci web, haker można za pomocą narzędzia, takie jak [fiddler](http://fiddler2.com/home), lub zapisu fragmentów kodu JavaScript można opublikować `Secret` tworzą wartość. Bez [powiązać](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx) ograniczanie pól używanych przez integrator modelu podczas tworzenia atrybutu `Student` wystąpienia*,* integratora modelu czy odebrania który `Secret` stanowią wartość i użyć go do Utwórz `Student` wystąpienia jednostki. Następnie niezależnie od wartości haker określony dla `Secret` pola formularza, czy zaktualizowane w bazie danych. Na poniższej ilustracji przedstawiono fiddler Dodawanie narzędzia `Secret` pola (o wartości "OverPost") do wartości przesłanego formularza.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)  

    Wartość "OverPost" może następnie pomyślnie dodane do `Secret` właściwości wstawionego wiersza, chociaż nie powinny strony sieci web i można ustawić tej właściwości.

    Jest najlepszym rozwiązaniem bezpieczeństwa używać `Include` parametr o `Bind` atrybutu *dozwolonych* pola. Istnieje również możliwość użycia `Exclude` parametr *blacklist* pola do wykluczenia. Powód `Include` jest bardziej bezpieczne jest, że po dodaniu nowych właściwości do jednostki, nowe pole nie jest automatycznie chroniony `Exclude` listy.

    Można zapobiec overposting w scenariuszach edycji jest najpierw odczytu jednostki bazy danych, a następnie podczas wywoływania `TryUpdateModel`, przekazując Jawne dozwolonych właściwości listy. To metodę użytą w tych samouczkach.

    Alternatywny sposób, aby zapobiec overposting, który jest wybierany przez wielu deweloperów jest użycie modeli widoku zamiast klas jednostek dla wiązania modelu. Zawierać tylko właściwości, które chcesz zaktualizować w modelu widoku. Po zakończeniu integratora modelu MVC, skopiuj Wyświetl właściwości modelu do wystąpienia jednostki, opcjonalnie przy użyciu narzędzia takie jak [AutoMapper](http://automapper.org/). Użyj bazy danych. Wpis na wystąpienie jednostki do zestawu stanie Unchanged, a następnie ustaw Property("PropertyName"). IsModified na wartość true dla każdej właściwości jednostki, który znajduje się w modelu widoku. Ta metoda działa zarówno w edytować i tworzyć scenariuszy.

    Inne niż `Bind` atrybutu `try-catch` blok jest tylko zmiany wprowadzone do szkieletu kodu. Jeśli wyjątek, która jest pochodną [DataException](https://msdn.microsoft.com/en-us/library/system.data.dataexception.aspx) jest zgłoszony, gdy trwa zapisywanie zmian, zostanie wyświetlony komunikat ogólny błąd. [DataException](https://msdn.microsoft.com/en-us/library/system.data.dataexception.aspx) wyjątki są czasami przerwany zewnętrzne do aplikacji, a nie błąd programistyczny, dlatego zalecane jest użytkownik, aby spróbować ponownie. Chociaż nie jest zaimplementowana w tym przykładzie, jakości aplikacji produkcyjnej może zarejestruje wyjątek. Aby uzyskać więcej informacji, zobacz **dziennik, aby uzyskać szczegółowe informacje o** sekcji [monitorowanie i dane telemetryczne (tworzenia rzeczywistych aplikacji w chmurze platformy Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    Kod w *Views\Student\Create.cshtml* jest podobny do opisany w *Details.cshtml*, ale `EditorFor` i `ValidationMessageFor` pomocników są używane dla każdego pola zamiast `DisplayFor`. W tym miejscu jest odpowiedni kod:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create.chstml* obejmuje również `@Html.AntiForgeryToken()`, który współpracuje z `ValidateAntiForgeryToken` atrybutu w kontrolerze, aby zapobiec [sfałszowaniem żądania między lokacjami](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) ataków.

    Nie są konieczne nie zmiany w *Create.cshtml*.
2. Uruchom strony, wybierając **studentów** kartę i klikając **Utwórz nowy**.
3. Wprowadź nazwy i nieprawidłową datę, a następnie kliknij przycisk **Utwórz** Aby wyświetlić komunikat o błędzie.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)

    Jest to weryfikacji po stronie serwera, który można pobrać domyślnie; w samouczku nowsze zobaczysz jak dodać atrybuty, które również wygeneruje kod weryfikacji po stronie klienta. Następujący wyróżniony kod pokazuje sprawdzenie poprawności modelu w **Utwórz** metody.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]
4. Zmień wartość na prawidłową datę, a następnie kliknij przycisk **Utwórz** aby zobaczyć nowe student są wyświetlane w **indeksu** strony.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

## <a name="update-the-edit-httppost-method"></a>Metoda HttpPost edycji aktualizacji

W *Controllers\StudentController.cs*, `HttpGet` `Edit` — metoda (jeden bez `HttpPost` atrybut) używa `Find` metoda pobierania wybranego `Student` jednostki, jako użytkownik był wyświetlany w `Details` metody. Nie trzeba będzie zmienić tę metodę.

Jednak zastąpić `HttpPost` `Edit` metody akcji z następującym kodem:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

Ze względów bezpieczeństwa zapobiegające zaimplementować te zmiany [overposting](#overpost), tworzenia szkieletu, generowane `Bind` atrybutu i dodać obiekt utworzony przez obiekt wiążący modelu do zestawu z flagą zmodyfikowane jednostek. Czy kod nie jest zalecane, ponieważ `Bind` atrybutu usuwa wszystkie istniejące dane w polach niewymienionych w `Include` parametru. W przyszłości zostaną zaktualizowane tworzenia szkieletu kontrolera MVC, aby go nie generuje `Bind` atrybuty dla metod edycji.

Nowy kod odczytuje istniejącej jednostki i wywołania [TryUpdateModel](https://msdn.microsoft.com/en-us/library/system.web.mvc.controller.tryupdatemodel(v=vs.118).aspx) można zaktualizować pola z danych wejściowych użytkownika w danych przesłanego formularza. Automatyczna zmiana Entity Framework śledzenia zestawy [zmodyfikowane](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx) flagi jednostki. Gdy [SaveChanges](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metoda jest wywoływana, `Modified` flaga powoduje, że programu Entity Framework w celu tworzenia instrukcji SQL, aby zaktualizować wiersza bazy danych. [Konfliktom współbieżności](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) są ignorowane, a wszystkie kolumny wiersza bazy danych są aktualizowane, w tym te, które nie zmienił się użytkownika. (Nowsze samouczek pokazuje, jak obsługiwać konfliktom współbieżności, a jeśli chcesz tylko poszczególnych pól zostać zaktualizowane w bazie danych, możesz ustawić jednostki Unchanged i ustawić poszczególnych pól do zmodyfikowane.)

Najlepszym rozwiązaniem zapobiegające overposting pola, które mają być aktualizuje edycji strony są białej w `TryUpdateModel` parametrów. Obecnie nie ma żadnych dodatkowych pól, które chronisz, ale lista pól, które mają integratora modelu do powiązania gwarantuje, że jeśli dodajesz pola do modelu danych w przyszłości, są one automatycznie chronione aż dodasz je tutaj.

W wyniku tych zmian podpis metody metody HttpPost edycji jest taka sama jak metoda edycji HttpGet; w związku z tym została zmieniona metoda EditPost.

> [!TIP] 
> 
> **Stany jednostki i przełącznikami Attach i metody SaveChanges**
> 
> Przechowuje informacje o kontekście bazy danych, czy jednostki w pamięci są zsynchronizowane z ich odpowiednich wierszy w bazie danych, a te informacje określa, co się stanie w przypadku wywołania `SaveChanges` metody. Na przykład podczas przekazywania nową jednostkę do [Dodaj](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset.add(v=vs.103).aspx) — metoda, która stanu jednostki jest ustawiona na `Added`. Następnie podczas wywoływania [SaveChanges](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metody wystawia kontekst bazy danych SQL `INSERT` polecenia.
> 
> Jednostka może działać w jednym z[następujące stany](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx):
> 
> - `Added`. Jednostka nie istnieje jeszcze w bazie danych. `SaveChanges` Metody należy wygenerować `INSERT` instrukcji.
> - `Unchanged`. Nie trzeba żadnego ustawienia można zrobić za pomocą tej jednostki przez `SaveChanges` metody. Podczas odczytu jednostki bazy danych, jednostka rozpoczyna się od tego stanu.
> - `Modified`. Zmodyfikowano niektóre lub wszystkie wartości właściwości jednostki. `SaveChanges` Metody należy wygenerować `UPDATE` instrukcji.
> - `Deleted`. Jednostka została oznaczona do usunięcia. `SaveChanges` Metody należy wygenerować `DELETE` instrukcji.
> - `Detached`. Jednostka nie jest śledzony przez kontekst bazy danych.
> 
> W aplikacji pulpitu zmian stanu zwykle są ustawiane automatycznie. Pulpitu typu aplikacji służy do odczytu jednostki i wprowadzić zmiany w niektóre z jej wartości właściwości. Powoduje to, że jego stan jednostki automatycznie zmieniona na `Modified`. Następnie podczas wywoływania `SaveChanges`, Entity Framework generuje SQL `UPDATE` instrukcji, która aktualizuje tylko rzeczywiste właściwości, które można zmienić.
> 
> Dla tej ciągłej sekwencji nie zezwala na odłączonego rodzaju aplikacje sieci web. [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx) które odczytuje jednostki zostanie usunięty po renderowania strony. Gdy `HttpPost` `Edit` metoda akcji jest wywoływana, nowych żądań i masz nowe wystąpienie klasy [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx), dlatego należy ręcznie ustawić stan jednostki `Modified.` , a następnie podczas wywoływania `SaveChanges`, Entity Framework aktualizuje wszystkie kolumny wiersza bazy danych, ponieważ kontekst nie ma możliwości wiedzieć, właściwości, które można zmienić.
> 
> Jeśli chcesz, aby SQL `Update` instrukcji można zaktualizować tylko pola, które użytkownik faktycznie zmienił, oryginalne wartości można zapisać w jakiś sposób (np. pola ukryte), aby były dostępne podczas `HttpPost` `Edit` metoda jest wywoływana. Następnie można utworzyć `Student` jednostką przy użyciu oryginalnych wartości, wywołanie `Attach` metody z tej wersji oryginalnej jednostki, zaktualizuj wartości jednostki do nowych wartości, a następnie wywołać `SaveChanges.` uzyskać więcej informacji, zobacz [ Stany jednostki i metody SaveChanges](https://msdn.microsoft.com/en-us/data/jj592676) i [dane lokalne](https://msdn.microsoft.com/en-us/data/jj592872) w Centrum deweloperów MSDN danych.


Kod HTML i Razor w *Views\Student\Edit.cshtml* jest podobny do opisany w *Create.cshtml*, a zmiany nie są wymagane.

Uruchom strony, wybierając **studentów** kartę, a następnie klikając pozycję **Edytuj** hiperłącza.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

Niektóre dane i kliknij przycisk Zmień **zapisać**. Zostaną wyświetlone zmienione dane na stronie indeksu.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-delete-page"></a>Aktualizowanie strony usuwania

W *Controllers\StudentController.cs*, kod szablonu `HttpGet` `Delete` używa metody `Find` metoda pobierania wybranego `Student` jednostki, jak opisany w `Details` i `Edit` metody. Jednak do zaimplementowania niestandardowy komunikat o błędzie podczas wywołania `SaveChanges` nie powiedzie się, niektóre funkcje zostanie dodana do tej metody i jego odpowiedni widok.

Instrukcji dotyczących aktualizacji i tworzenia operacji operacji usuwania wymaga dwóch metod akcji. Metoda jest wywoływana w odpowiedzi na żądanie GET Wyświetla widok, który daje użytkownikowi możliwość zatwierdzenia lub anulować operację usuwania. Jeśli użytkownik zaakceptuje go, tworzona jest wysłanie żądania POST. W takim przypadku `HttpPost` `Delete` metoda jest wywoływana, a następnie metoda faktycznie wykonuje operację usuwania.

Należy dodać `try-catch` za pomocą bloku `HttpPost` `Delete` można obsłużyć wszystkie błędy, które mogą wystąpić, gdy baza danych jest aktualizowana. Jeśli wystąpi błąd, `HttpPost` `Delete` wywołania metody `HttpGet` `Delete` metody przekazanie jej przez parametr, który wskazuje, że wystąpił błąd. `HttpGet Delete` Metody następnie zostanie ponownie stronę potwierdzenia oraz komunikat o błędzie, która umożliwia użytkownikowi możliwość anulowania lub spróbuj ponownie.

1. Zastąp `HttpGet` `Delete` metodę akcji za pomocą następujący kod, który zarządza raportowanie błędów: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Ten kod akceptuje [opcjonalny parametr](https://msdn.microsoft.com/en-us/library/dd264739.aspx) wskazująca, czy metoda została wywołana po awarii, aby zapisać zmiany. Ten parametr jest `false` podczas `HttpGet` `Delete` metoda jest wywoływana bez poprzednim błędzie. Gdy jest wywoływana `HttpPost` `Delete` parametr metody w odpowiedzi na błąd aktualizacji bazy danych, jest `true` , a komunikat o błędzie jest przekazywana do widoku.
- Zastąp `HttpPost` `Delete` metody akcji (o nazwie `DeleteConfirmed`) z następującym kodem, wykonuje operację usuwania rzeczywistego oraz przechwytującą wszystkie błędy aktualizacji bazy danych.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    Ten kod pobiera wybranej jednostki, następnie wywołuje [Usuń](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset.remove(v=vs.103).aspx) metody w celu ustawienia stanu jednostki `Deleted`. Gdy `SaveChanges` jest nazywany SQL `DELETE` wygenerowaniu polecenia. Również zmieniono nazwę metody akcji `DeleteConfirmed` do `Delete`. Kod z utworzonym szkieletem o nazwie `HttpPost` `Delete` metody `DeleteConfirmed` umożliwiają `HttpPost` metody unikatowego podpisu. (CLR wymaga przeciążonej metody mają parametry innej metody). Teraz, czy podpisy są unikatowe, możesz przestrzegaj Konwencji MVC i użyć takiej samej nazwy `HttpPost` i `HttpGet` metody zostaną usunięte.

    W przypadku zwiększania wydajności aplikacji dużych priorytet, można uniknąć niepotrzebnych zapytanie SQL, który można pobrać wiersza, zastępując wierszy kodu, które wywołują `Find` i `Remove` metody z następującym kodem:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    Ten kod tworzy `Student` jednostką przy użyciu tylko wartość klucza podstawowego, a następnie ustawia stan jednostki `Deleted`. To wszystko, który programu Entity Framework wymaga, aby usunąć jednostkę.

    Jak wspomniano, `HttpGet` `Delete` — metoda nie powoduje usunięcia danych. Wykonywanie operacji usuwania w odpowiedzi na polecenie GET żądania (lub istotnego dla badania, wykonywanie żadnych operacji edycji Utwórz operację lub innej operacji, które zmienia dane) tworzy zagrożenie bezpieczeństwa. Aby uzyskać więcej informacji, zobacz [46 Porada # w programie ASP.NET MVC — nie używaj usunąć łącza, ponieważ mogą one tworzyć luk w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) na blogu Stephen Walther.
- W *Views\Student\Delete.cshtml*, Dodaj komunikat o błędzie między `h2` nagłówek i `h3` nagłówek, jak pokazano w poniższym przykładzie:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

    Uruchom strony, wybierając **studentów** kartę i klikając **usunąć** hiperłącze:

    ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)
- Kliknij przycisk **usunąć**. Bez uczniów usuniętych zostanie wyświetlona strona indeksu. (Zobaczysz przykładowy kod w praktyce obsługi błędów [samouczek współbieżności](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="closing-database-connections"></a>Zamykanie połączenia bazy danych

Aby zamknąć połączenia bazy danych i zwolnić zasoby, które posiadają tak szybko, jak to możliwe, należy dysponować wystąpienia kontekstu po zakończeniu z nim. Oznacza to, dlaczego szkieletu kodu zawiera [Dispose](https://msdn.microsoft.com/en-us/library/system.idisposable.dispose(v=vs.110).aspx) metody na końcu `StudentController` klasy w *StudentController.cs*, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

Podstawowym `Controller` klasy już implementuje `IDisposable` interfejsu, dlatego ten kod dodaje po prostu zastąpienia `Dispose(bool)` metodę jawne usuwanie wystąpienia kontekstu.

<a id="transactions"></a>
## <a name="handling-transactions"></a>Obsługa transakcji

Domyślnie programu Entity Framework niejawnie implementuje transakcji. W scenariuszach, gdzie zmianę wielu wierszy lub tabel, a następnie wywołać `SaveChanges`, Entity Framework automatycznie upewnia się, że wszystkie zmiany powiedzie się lub nie powiedzie się. Jeśli niektóre zmiany są najpierw wykonywane, a następnie błąd wystąpi, zmiany te są automatycznie wycofana. Scenariusze, w którym należy więcej kontrolujesz — na przykład, jeśli chcesz dołączyć operacje wykonywane poza Entity Framework w transakcji — można znaleźć [Praca z transakcji](https://msdn.microsoft.com/en-US/data/dn456843) w witrynie MSDN.

## <a name="summary"></a>Podsumowanie

Masz teraz kompletny zestaw stron, które wykonywać proste operacje CRUD na `Student` jednostek. Pomocnicy MVC jest używany do generowania elementy interfejsu użytkownika dla pola danych. Aby uzyskać więcej informacji na temat pomocników MVC, zobacz [renderowania pomocników HTML za pomocą formularza](https://msdn.microsoft.com/en-us/library/dd410596(v=VS.98).aspx) (strona jest dla platformy MVC 3, ale jest nadal istotne dla MVC 5).

W następnym samouczku będzie rozwiń funkcji strony indeksu, dodając sortowania i stronicowania.

Wystaw opinię na jak zbędne tego samouczka i co można możemy ulepszyć. Możesz również poprosić o nowe tematy w [Pokaż mnie jak z kodu](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Linki do innych zasobów programu Entity Framework, można znaleźć w [dostępu do danych programu ASP.NET - zalecane zasobów](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Poprzednie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
[dalej](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
