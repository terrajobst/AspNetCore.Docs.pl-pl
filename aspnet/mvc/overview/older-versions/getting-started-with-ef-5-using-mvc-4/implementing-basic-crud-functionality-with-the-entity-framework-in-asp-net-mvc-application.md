---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementowanie funkcji Basic CRUD z programu Entity Framework w aplikacji ASP.NET MVC (2 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: "Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 425335d72643da39faee6e457d552c9faa6a1f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Implementowanie funkcji Basic CRUD z programu Entity Framework w aplikacji ASP.NET MVC (2 10)
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Informacje o samouczek serii, zobacz [pierwszy samouczek z tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Samouczek serii można uruchomić od początku lub [pobrać projekt starter w tym rozdziale](building-the-ef5-mvc4-chapter-downloads.md) i zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli napotkasz problem, nie można rozwiązać, [Pobieranie ukończone rozdział](building-the-ef5-mvc4-chapter-downloads.md) i próba odtworzenia problemu. Rozwiązanie tego problemu można znaleźć ogólnie porównując swój kod kompletny kod. Dla niektórych typowych błędów i sposobu rozwiązania tych problemów, zobacz [błędów i rozwiązania problemu.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


W poprzednich samouczek utworzono aplikację MVC, która przechowuje i wyświetla danych przy użyciu programu Entity Framework i bazy danych LocalDB programu SQL Server. W tym samouczku należy przejrzeć i dostosować CRUD (tworzenia, odczytu, aktualizowanie i usuwanie) kodu, który będzie szkieletów MVC automatycznie tworzy w kontrolery i widoki.

> [!NOTE]
> Jest typowym rozwiązaniem implementacji klienta wzorca repozytorium, aby można było utworzyć warstwę abstrakcji między kontrolerem a warstwa dostępu do danych. Aby zachować te samouczki proste, nie implementuje repozytorium do nowszej samouczku z tej serii.


W tym samouczku utworzysz następujących stron sieci web:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Tworzenie strony szczegółów

Kod z utworzonym szkieletem dla uczniów lub studentów `Index` po lewej stronie `Enrollments` właściwości, ponieważ kolekcja zawiera tej właściwości. W `Details` strony będzie wyświetlać zawartość kolekcji, w tabeli HTML.

 W *Controllers\StudentController.cs*, metoda akcji `Details` wyświetlić używa `Find` metoda pobierania pojedynczy `Student` jednostki. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 Wartość tego klucza jest przekazywany do metody jako `id` parametru i pochodzi z danych trasy w **szczegóły** hiperłącza na stronie indeksu. 

1. Otwórz *Views\Student\Details.cshtml*. Każde pole jest wyświetlane przy użyciu `DisplayFor` pomocnika, jak pokazano w poniższym przykładzie: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Po `EnrollmentDate` pól i bezpośrednio przed tagiem zamykającym `fieldset` tagów, Dodaj kod, aby wyświetlić listę rejestracji, jak pokazano w poniższym przykładzie:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Ten kod w pętli jednostek w `Enrollments` właściwości nawigacji. Dla każdego `Enrollment` jednostki we właściwości, Wyświetla tytuł kursu i kategorii. Tytuł kursu są pobierane z `Course` jednostki, która jest przechowywana w `Course` właściwość nawigacji `Enrollments` jednostki. Wszystkie te dane są pobierane z bazy danych automatycznie gdy jest to potrzebne. (Innymi słowy, jest używany podczas ładowania opóźnionego tutaj. Nie określono *wczesny ładowania* dla `Courses` właściwość nawigacji, aby po raz pierwszy, zostanie podjęta próba dostęp do tej właściwości, zapytanie jest wysyłane do pobierania danych do bazy danych. Więcej o opóźnionego ładowania i ładowanie wczesny w [dane dotyczące odczytywania](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczek później w tej serii.)
3. Uruchom strony, wybierając **studentów** kartę i klikając **szczegóły** łącze Alexander Carson. Zostanie wyświetlona lista kursów i klasy dla wybranego uczniów:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Aktualizowanie tworzenia strony

1. W *Controllers\StudentController.cs*, Zastąp `HttpPost``Create` metodę akcji za pomocą następujący kod, aby dodać `try-catch` bloku i [atrybutu Bind](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx) szkieletu metody: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Ten kod dodaje `Student` jednostki utworzone przez program ASP.NET MVC integratora modelu do `Students` jednostki ustawiona, a następnie zapisuje zmiany w bazie danych. (*Integratora modelu* odwołuje się do funkcjonalność platformy ASP.NET MVC, który sprawia, że jej ułatwiają pracę z danych przesyłanych przez formularz; integratora modelu formularza zaksięgowany konwertuje wartości do typów CLR i przekazuje je do metody akcji w parametrach. In this case, tworzy wystąpienie integratora modelu `Student` jednostki przy użyciu właściwości wartości z `Form` kolekcji.)

    `ValidateAntiForgeryToken` Atrybutu pomaga zapobiegać [sfałszowaniem żądania między lokacjami](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) ataków.

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/en-us/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/en-us/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.chstml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. Uruchom strony, wybierając **studentów** kartę i klikając **Utwórz nowy**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    Niektóre sprawdzanie poprawności danych działa domyślnie. Wprowadź nazwy i nieprawidłową datę, a następnie kliknij przycisk **Utwórz** Aby wyświetlić komunikat o błędzie.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    Następujący wyróżniony kod pokazuje sprawdzenie poprawności modelu.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Zmień datę na prawidłową wartość, takich jak 9/1/2005, a następnie kliknij przycisk **Utwórz** aby zobaczyć nowe student są wyświetlane w **indeksu** strony.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Aktualizowanie edycji strony POST

W *Controllers\StudentController.cs*, `HttpGet` `Edit` — metoda (jeden bez `HttpPost` atrybut) używa `Find` metoda pobierania wybranego `Student` jednostki, jako użytkownik był wyświetlany w `Details` metody. Nie trzeba będzie zmienić tę metodę.

Jednak zastąpić `HttpPost` `Edit` metodę akcji za pomocą następujący kod, aby dodać `try-catch` bloku i [atrybutu Bind](https://msdn.microsoft.com/en-us/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Ten kod jest podobny do opisany w `HttpPost` `Create` metody. Jednak zamiast opcji dodawania obiekt utworzony przez obiekt wiążący modelu do zestawu jednostek, ten kod ustawia flagę w jednostce wskazujący, że został on zmieniony. Gdy [SaveChanges](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metoda jest wywoływana, [zmodyfikowane](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx) flaga powoduje, że programu Entity Framework w celu tworzenia instrukcji SQL, aby zaktualizować wiersza bazy danych. Wszystkie kolumny wiersza bazy danych zostaną zaktualizowane, łącznie z tymi, które nie zmienił się użytkownik, a konfliktom współbieżności są ignorowane. (Dowiesz się, jak obsłużyć współbieżność w późniejszym samouczku z tej serii.)

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Stany jednostki i przełącznikami Attach i metody SaveChanges

Przechowuje informacje o kontekście bazy danych, czy jednostki w pamięci są zsynchronizowane z ich odpowiednich wierszy w bazie danych, a te informacje określa, co się stanie w przypadku wywołania `SaveChanges` metody. Na przykład podczas przekazywania nową jednostkę do [Dodaj](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset.add(v=vs.103).aspx) — metoda, która stanu jednostki jest ustawiona na `Added`. Następnie podczas wywoływania [SaveChanges](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metody wystawia kontekst bazy danych SQL `INSERT` polecenia.

Jednostka może działać w jednym z[następujące stany](https://msdn.microsoft.com/en-us/library/system.data.entitystate.aspx):

- `Added`. Jednostka nie istnieje jeszcze w bazie danych. `SaveChanges` Metody należy wygenerować `INSERT` instrukcji.
- `Unchanged`. Nie trzeba żadnego ustawienia można zrobić za pomocą tej jednostki przez `SaveChanges` metody. Podczas odczytu jednostki bazy danych, jednostka rozpoczyna się od tego stanu.
- `Modified`. Zmodyfikowano niektóre lub wszystkie wartości właściwości jednostki. `SaveChanges` Metody należy wygenerować `UPDATE` instrukcji.
- `Deleted`. Jednostka została oznaczona do usunięcia. `SaveChanges` Metody należy wygenerować `DELETE` instrukcji.
- `Detached`. Jednostka nie jest śledzony przez kontekst bazy danych.

W aplikacji pulpitu zmian stanu zwykle są ustawiane automatycznie. Pulpitu typu aplikacji służy do odczytu jednostki i wprowadzić zmiany w niektóre z jej wartości właściwości. Powoduje to, że jego stan jednostki automatycznie zmieniona na `Modified`. Następnie podczas wywoływania `SaveChanges`, Entity Framework generuje SQL `UPDATE` instrukcji, która aktualizuje tylko rzeczywiste właściwości, które można zmienić.

Dla tej ciągłej sekwencji nie zezwala na odłączonego rodzaju aplikacje sieci web. [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx) które odczytuje jednostki zostanie usunięty po renderowania strony. Gdy `HttpPost` `Edit` metoda akcji jest wywoływana, nowych żądań i masz nowe wystąpienie klasy [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx), dlatego należy ręcznie ustawić stan jednostki `Modified.` , a następnie podczas wywoływania `SaveChanges`, Entity Framework aktualizuje wszystkie kolumny wiersza bazy danych, ponieważ kontekst nie ma możliwości wiedzieć, właściwości, które można zmienić.

Jeśli chcesz, aby SQL `Update` instrukcji można zaktualizować tylko pola, które użytkownik faktycznie zmienił, oryginalne wartości można zapisać w jakiś sposób (np. pola ukryte), aby były dostępne podczas `HttpPost` `Edit` metoda jest wywoływana. Następnie można utworzyć `Student` jednostką przy użyciu oryginalnych wartości, wywołanie `Attach` metody z tej wersji oryginalnej jednostki, zaktualizuj wartości jednostki do nowych wartości, a następnie wywołać `SaveChanges.` uzyskać więcej informacji, zobacz [ Stany jednostki i metody SaveChanges](https://msdn.microsoft.com/en-us/data/jj592676) i [dane lokalne](https://msdn.microsoft.com/en-us/data/jj592872) w Centrum deweloperów MSDN danych.

Kod w *Views\Student\Edit.cshtml* jest podobny do opisany w *Create.cshtml*, a zmiany nie są wymagane.

Uruchom strony, wybierając **studentów** kartę, a następnie klikając pozycję **Edytuj** hiperłącza.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Niektóre dane i kliknij przycisk Zmień **zapisać**. Zostaną wyświetlone zmienione dane na stronie indeksu.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aktualizowanie strony usuwania

W *Controllers\StudentController.cs*, kod szablonu `HttpGet` `Delete` używa metody `Find` metoda pobierania wybranego `Student` jednostki, jak opisany w `Details` i `Edit` metody. Jednak do zaimplementowania niestandardowy komunikat o błędzie podczas wywołania `SaveChanges` nie powiedzie się, niektóre funkcje zostanie dodana do tej metody i jego odpowiedni widok.

Instrukcji dotyczących aktualizacji i tworzenia operacji operacji usuwania wymaga dwóch metod akcji. Metoda jest wywoływana w odpowiedzi na żądanie GET Wyświetla widok, który daje użytkownikowi możliwość zatwierdzenia lub anulować operację usuwania. Jeśli użytkownik zaakceptuje go, tworzona jest wysłanie żądania POST. W takim przypadku `HttpPost` `Delete` metoda jest wywoływana, a następnie metoda faktycznie wykonuje operację usuwania.

Należy dodać `try-catch` za pomocą bloku `HttpPost` `Delete` można obsłużyć wszystkie błędy, które mogą wystąpić, gdy baza danych jest aktualizowana. Jeśli wystąpi błąd, `HttpPost` `Delete` wywołania metody `HttpGet` `Delete` metody przekazanie jej przez parametr, który wskazuje, że wystąpił błąd. `HttpGet Delete` Metody następnie zostanie ponownie stronę potwierdzenia oraz komunikat o błędzie, która umożliwia użytkownikowi możliwość anulowania lub spróbuj ponownie.

1. Zastąp `HttpGet` `Delete` metodę akcji za pomocą następujący kod, który zarządza raportowanie błędów: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Ten kod akceptuje [opcjonalne](https://msdn.microsoft.com/en-us/library/dd264739.aspx) parametrów typu Boolean wskazującą, czy została wywołana po awarii, aby zapisać zmiany. Ten parametr jest `false` podczas `HttpGet` `Delete` metoda jest wywoływana bez poprzednim błędzie. Gdy jest wywoływana `HttpPost` `Delete` parametr metody w odpowiedzi na błąd aktualizacji bazy danych, jest `true` , a komunikat o błędzie jest przekazywana do widoku.
- Zastąp `HttpPost` `Delete` metody akcji (o nazwie `DeleteConfirmed`) z następującym kodem, wykonuje operację usuwania rzeczywistego oraz przechwytującą wszystkie błędy aktualizacji bazy danych.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

    Ten kod pobiera wybranej jednostki, następnie wywołuje [Usuń](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset.remove(v=vs.103).aspx) metody w celu ustawienia stanu jednostki `Deleted`. Gdy `SaveChanges` jest nazywany SQL `DELETE` wygenerowaniu polecenia. Również zmieniono nazwę metody akcji `DeleteConfirmed` do `Delete`. Kod z utworzonym szkieletem o nazwie `HttpPost` `Delete` metody `DeleteConfirmed` umożliwiają `HttpPost` metody unikatowego podpisu. (CLR wymaga przeciążonej metody mają parametry innej metody). Teraz, czy podpisy są unikatowe, możesz przestrzegaj Konwencji MVC i użyć takiej samej nazwy `HttpPost` i `HttpGet` metody zostaną usunięte.

    W przypadku zwiększania wydajności aplikacji dużych priorytet, można uniknąć niepotrzebnych zapytanie SQL, który można pobrać wiersza, zastępując wierszy kodu, które wywołują `Find` i `Remove` metody z następującym kodem, jak pokazano w żółty Wyróżnij:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

    Ten kod tworzy `Student` jednostką przy użyciu tylko wartość klucza podstawowego, a następnie ustawia stan jednostki `Deleted`. To wszystko, który programu Entity Framework wymaga, aby usunąć jednostkę.

    Jak wspomniano, `HttpGet` `Delete` — metoda nie powoduje usunięcia danych. Wykonywanie operacji usuwania w odpowiedzi na polecenie GET żądania (lub istotnego dla badania, wykonywanie żadnych operacji edycji Utwórz operację lub innej operacji, które zmienia dane) tworzy zagrożenie bezpieczeństwa. Aby uzyskać więcej informacji, zobacz [46 Porada # w programie ASP.NET MVC — nie używaj usunąć łącza, ponieważ mogą one tworzyć luk w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) na blogu Stephen Walther.
- W *Views\Student\Delete.cshtml*, Dodaj komunikat o błędzie między `h2` nagłówek i `h3` nagłówek, jak pokazano w poniższym przykładzie:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

    Uruchom strony, wybierając **studentów** kartę i klikając **usunąć** hiperłącze:

    ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
- Kliknij przycisk **usunąć**. Bez uczniów usuniętych zostanie wyświetlona strona indeksu. (Zobaczysz przykładowy kod w praktyce obsługi błędów [Obsługa współbieżności](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczek później w tej serii.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Zapewnienie połączenia bazy danych nie pozostaną otwarte

Aby upewnić się, że połączenia bazy danych zostały prawidłowo zamknięte i zasoby, które posiadają zwolnionych w górę, należy sprawdzić jej usunięciu wystąpienia kontekstu. Oznacza to, dlaczego szkieletu kodu zawiera [Dispose](https://msdn.microsoft.com/en-us/library/system.idisposable.dispose(v=vs.110).aspx) metody na końcu `StudentController` klasy w *StudentController.cs*, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

Podstawowym `Controller` klasy już implementuje `IDisposable` interfejsu, dlatego ten kod dodaje po prostu zastąpienia `Dispose(bool)` metodę jawne usuwanie wystąpienia kontekstu.

## <a name="summary"></a>Podsumowanie

Masz teraz kompletny zestaw stron, które wykonywać proste operacje CRUD na `Student` jednostek. Pomocnicy MVC jest używany do generowania elementy interfejsu użytkownika dla pola danych. Aby uzyskać więcej informacji na temat pomocników MVC, zobacz [renderowania pomocników HTML za pomocą formularza](https://msdn.microsoft.com/en-us/library/dd410596(v=VS.98).aspx) (strona jest dla platformy MVC 3, ale jest nadal istotne dla platformy MVC 4).

W następnym samouczku będzie rozwiń funkcji strony indeksu, dodając sortowania i stronicowania.

Linki do innych zasobów programu Entity Framework, można znaleźć w [Mapa zawartości dostępu do danych programu ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Poprzednie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
[dalej](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
