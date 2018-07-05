---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementowanie podstawowych funkcji CRUD z platformą Entity Framework w aplikacji ASP.NET MVC (2 z 10) | Dokumentacja firmy Microsoft
author: tdykstra
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 7535a72afaa79260303e3094cbaf97f97132c1d5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399263"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Implementowanie podstawowych funkcji CRUD z platformą Entity Framework w aplikacji ASP.NET MVC (2 z 10)
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Można uruchomić tej serii samouczka od początku lub [pobrać projekt startowy w tym rozdziale](building-the-ef5-mvc4-chapter-downloads.md) i zacznij tutaj.
> 
> > [!NOTE] 
> > 
> > Jeśli napotkasz problem, nie można rozpoznać [Pobieranie ukończone rozdział](building-the-ef5-mvc4-chapter-downloads.md) i spróbuj odtworzyć problem. Rozwiązanie tego problemu można znaleźć zwykle porównując swój kod, aby kompletny kod. Niektóre typowe błędy i sposobu rozwiązania tych problemów można znaleźć [błędów i rozwiązania problemu.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


W poprzednim samouczku utworzono aplikację MVC, która przechowuje i wyświetla dane przy użyciu platformy Entity Framework i programu SQL Server LocalDB. W tym samouczku będziesz przejrzenie i dostosowanie CRUD (Tworzenie, odczytywanie, aktualizowanie, usuwanie) kod, który MVC scaffolding automatycznie utworzy dla Ciebie w widoków i kontrolerów.

> [!NOTE]
> Jest to powszechną praktyką w celu zaimplementowania wzorca repozytorium, aby można było utworzyć warstwę abstrakcji między kontrolerem i warstwy dostępu do danych. W celu uproszczenia w tych samouczkach nie implementuje repozytorium do dalszych samouczków w tej serii.


W tym samouczku utworzysz następujących stron sieci web:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Tworzenie strony szczegółów

Utworzony szkielet kodu dla uczniów lub studentów `Index` strony pozostawiono `Enrollments` właściwość, ponieważ ta właściwość zawiera kolekcję. W `Details` strony zawartości kolekcji będą wyświetlane w tabeli HTML.

 W *Controllers\StudentController.cs*, metodę akcji dla `Details` wyświetlić używa `Find` metodę, aby pobrać jeden `Student` jednostki. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 Wartość klucza jest przekazywany do metody jako `id` parametru i pochodzi z danych trasy w **szczegóły** hiperłącze na stronę indeksu. 

1. Otwórz *Views\Student\Details.cshtml*. Każde pole jest wyświetlana przy użyciu `DisplayFor` pomocnika, jak pokazano w poniższym przykładzie: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Po `EnrollmentDate` pól i bezpośrednio przed tagiem zamykającym `fieldset` tagów, należy dodać kod, aby wyświetlić listę rejestracji, jak pokazano w poniższym przykładzie:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Ten kod w pętli jednostek w `Enrollments` właściwości nawigacji. Dla każdego `Enrollment` jednostki we właściwości Wyświetla tytuł kurs i klasy korporacyjnej. Tytuł kurs jest pobierana z `Course` jednostki, która jest przechowywana w `Course` właściwość nawigacji `Enrollments` jednostki. Wszystkie te dane są pobierane z bazy danych automatycznie, gdy jest to konieczne. (Innymi słowy, używane są powolne ładowanie tutaj. Nie określono *wczesne ładowanie* dla `Courses` właściwość nawigacji, aby po raz pierwszy próby uzyskania dostępu do tej właściwości, zapytanie jest wysyłane do pobierania danych do bazy danych. Możesz dowiedzieć się więcej o powolne ładowanie i wczesne ładowanie w [odczytywanie powiązanych danych](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczków w dalszej części tej serii.)
3. Uruchomienia strony, wybierając **studentów** kartę i klikając **szczegóły** linku, aby użytkownik Alexander Carson. Zostanie wyświetlona lista kursów i ocen dla wybranych uczniów lub studentów:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Aktualizowanie strona tworzenia

1. W *Controllers\StudentController.cs*, Zastąp `HttpPost``Create` metody akcji, używając następującego kodu, aby dodać `try-catch` bloku i [atrybut wiązania](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) szkieletu metody: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Ten kod dodaje `Student` jednostki utworzone przez integrator modelu programu ASP.NET MVC do `Students` jednostki zestawu, a następnie zapisuje zmiany w bazie danych. (*Integratora modelu* odwołuje się do funkcjonalność platformy ASP.NET MVC, która pozwala je łatwiej jest pracować z danych przesyłanych przez formularz; integratora modelu w postaci konwertuje opublikowane wartości na typy CLR i przekazuje je do metody akcji w parametrach. In this case, tworzy wystąpienie integratora modelu `Student` jednostki przy użyciu właściwości wartości z kolekcji `Form` kolekcji.)

    `ValidateAntiForgeryToken` Atrybutów pomaga zapobiec [fałszerstwo żądania międzywitrynowego](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) ataków.

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.chstml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. Uruchom stronę, wybierając **studentów** kartę i klikając **Utwórz nowy**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    Sprawdzanie poprawności niektórych danych działa domyślnie. Wprowadź nazwy i nieprawidłową datę, a następnie kliknij przycisk **Utwórz** aby zobaczyć komunikat o błędzie.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    Następujący wyróżniony kod pokazuje sprawdzenie poprawności modelu.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Zmień datę na prawidłową wartość, np. 1/9/2005, a następnie kliknij przycisk **Utwórz** do nowego studenta pojawiają się w temacie **indeksu** strony.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Aktualizowanie wpisu strony edytowania

W *Controllers\StudentController.cs*, `HttpGet` `Edit` — metoda (jeden bez `HttpPost` atrybutu) używa `Find` metody do pobierania wybranej `Student` jednostki, jak pokazano w `Details` metody. Nie trzeba zmienić tę metodę.

Jednak zastąpić `HttpPost` `Edit` metody akcji, używając następującego kodu, aby dodać `try-catch` bloku i [atrybut wiązania](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Ten kod jest podobny do przedstawionego w `HttpPost` `Create` metody. Jednak zamiast opcji dodawania jednostki utworzone przez integratora modelu do zestawu jednostek, ten kod ustawia flagę w jednostce, wskazujący, że została ona zmieniona. Gdy [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metoda jest wywoływana, [zmodyfikowane](https://msdn.microsoft.com/library/system.data.entitystate.aspx) flaga powoduje, że platforma Entity Framework do tworzenia instrukcji SQL, aby zaktualizować wiersz bazy danych. Wszystkie kolumny wiersza bazy danych zostaną zaktualizowane, łącznie z tymi, które zmieniły się użytkownik, i konfliktów współbieżności są ignorowane. (Dowiesz się, jak obsłużyć współbieżności później w samouczku z tej serii.)

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Stany jednostki i dołączania i metod SaveChanges

Przechowuje informacje o kontekst bazy danych, czy jednostki w pamięci są zsynchronizowane z ich odpowiednie wiersze w bazie danych, a te informacje określa, co się stanie, gdy wywołujesz `SaveChanges` metody. Na przykład podczas przekazywania do nowej jednostki [Dodaj](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) metody, która stan obiektu jest ustawiony na `Added`. Następnie, gdy zostanie wywołana [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metody wystawia kontekst bazy danych SQL `INSERT` polecenia.

Jednostki mogą być w jednym z[następujące stany](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`. Jednostka nie istnieje jeszcze w bazie danych. `SaveChanges` Metody należy wygenerować `INSERT` instrukcji.
- `Unchanged`. Niczego nie trzeba wykonać za pomocą tej jednostki przy `SaveChanges` metody. Podczas odczytywania jednostki z bazy danych, jednostka rozpoczyna się w tym stanie.
- `Modified`. Niektóre lub wszystkie wartości właściwości jednostki zostały zmodyfikowane. `SaveChanges` Metody należy wygenerować `UPDATE` instrukcji.
- `Deleted`. Jednostka została oznaczona do usunięcia. `SaveChanges` Metody należy wygenerować `DELETE` instrukcji.
- `Detached`. Jednostka nie jest śledzony przez kontekst bazy danych.

W przypadku aplikacji komputerowych zmiany stanu są zazwyczaj ustawiane automatycznie. Pulpitu typu aplikacji służy do odczytu jednostki i wprowadzić zmiany do niektórych wartości właściwości. Powoduje to, że jego stan jednostki automatycznie był zmieniany na `Modified`. Następnie, gdy zostanie wywołana `SaveChanges`, platformy Entity Framework generuje SQL `UPDATE` instrukcję, która aktualizuje tylko rzeczywiste właściwości, które zostały zmienione.

Odłączony rodzaj aplikacji sieci web nie pozwala na to ciągła sekwencja. [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) , odczytuje jednostki zostanie usunięty po wyrenderowaniu strony. Gdy `HttpPost` `Edit` metody akcji jest wywoływana, nowe żądania i masz nowe wystąpienie klasy [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), więc trzeba ręcznie ustawić stan jednostki `Modified.` , a następnie podczas wywoływania `SaveChanges`, Entity Framework aktualizuje wszystkie kolumny wiersza bazy danych, ponieważ kontekst nie ma możliwości wiedzieć, właściwości, które można zmienić.

Jeśli chcesz, aby SQL `Update` instrukcję, aby zaktualizować tylko pola, które użytkownik rzeczywiste zmiany, można zapisać oryginalnych wartości w jakiś sposób (np. ukryte pola), tak aby były dostępne podczas `HttpPost` `Edit` metoda jest wywoływana. Możesz utworzyć `Student` jednostki przy użyciu oryginalnych wartości, wywołanie `Attach` metody z tej wersji oryginalnego obiektu, zaktualizuj wartości jednostki z nowymi wartościami miar, a następnie wywołaj `SaveChanges.` Aby uzyskać więcej informacji, zobacz [ Stany jednostki i SaveChanges](https://msdn.microsoft.com/data/jj592676) i [dane lokalne](https://msdn.microsoft.com/data/jj592872) w Centrum deweloperów danych w witrynie MSDN.

Kod w *Views\Student\Edit.cshtml* jest podobny do przedstawionego w *Create.cshtml*, i nie są wymagane żadne zmiany.

Uruchomienia strony, wybierając **studentów** kartę, a następnie klikając polecenie **Edytuj** hiperłącze.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Niektóre dane i kliknij przycisk Zmień **Zapisz**. Zobaczysz dane zmienione w stronę indeksu.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aktualizowanie strony Delete

W *Controllers\StudentController.cs*, kod szablonu dla `HttpGet` `Delete` metoda używa `Find` metody do pobierania wybranej `Student` jednostki, jak opisany w `Details` i `Edit` metody. Jednak aby zaimplementować niestandardowy komunikat o błędzie podczas wywołania `SaveChanges` zakończy się niepowodzeniem, pewne funkcje należy dodać do tej metody i jego odpowiedniego widoku.

Widoczny dla aktualizacji i operacje tworzenia, operacje usuwania wymagają dwóch metod akcji. Metoda, która jest wywoływana w odpowiedzi na żądanie GET Wyświetla widok, który zapewnia możliwość zatwierdzania lub anulować operację usuwania. Jeśli użytkownik zatwierdza, tworzona jest żądaniem POST. Jeśli tak się stanie, `HttpPost` `Delete` metoda jest wywoływana, a następnie ta metoda faktycznie wykonuje operację usuwania.

Następnie dodasz `try-catch` za pomocą bloku `HttpPost` `Delete` metody, aby obsłużyć wszystkie błędy, które mogą wystąpić po zaktualizowaniu bazy danych. Jeśli wystąpi błąd, `HttpPost` `Delete` wywołania metody `HttpGet` `Delete` metody, podając mu parametr, który wskazuje, że wystąpił błąd. `HttpGet Delete` Metoda następnie zostanie ponownie strona potwierdzenia oraz komunikat o błędzie, dając użytkownikowi możliwość anulowania lub spróbuj ponownie.

1. Zastąp `HttpGet` `Delete` metodę akcji za pomocą następujący kod, który zarządza raportowanie błędów: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Ten kod akceptuje [opcjonalne](https://msdn.microsoft.com/library/dd264739.aspx) parametr logiczny, który wskazuje, czy został wywołany po awarii, aby zapisać zmiany. Ten parametr jest `false` podczas `HttpGet` `Delete` metoda jest wywoływana bez poprzednim błędzie. Gdy jest wywoływana `HttpPost` `Delete` metody w odpowiedzi na błąd aktualizacji bazy danych, parametr jest `true` i komunikat o błędzie jest przekazywane do widoku.
2. Zastąp `HttpPost` `Delete` metody akcji (o nazwie `DeleteConfirmed`) z następującym kodem, wykonuje operację usuwania rzeczywiste i przechwytującą wszystkie błędy aktualizacji bazy danych.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Ten kod pobiera wybranej jednostki, następnie wywołuje [Usuń](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) metodę, aby ustawić stan jednostki `Deleted`. Gdy `SaveChanges` nosi SQL `DELETE` polecenia jest generowany. Również zostały zmienione nazwy metody akcji z `DeleteConfirmed` do `Delete`. Utworzony szkielet kodu o nazwie `HttpPost` `Delete` metoda `DeleteConfirmed` zapewnienie `HttpPost` unikatowy podpis metody. (Środowisko CLR wymaga przeciążone metody, aby miała parametry do innej metody). Skoro podpisy są unikatowe, możesz przestrzegaj Konwencji MVC i użyj takiej samej nazwie `HttpPost` i `HttpGet` usunięcie metod.

     Jeśli poprawę wydajności w aplikacji mocno obciążające ma najwyższy priorytet, można uniknąć niepotrzebnych zapytanie SQL, aby pobrać wiersza, zastępując wierszy kodu, które wywołują `Find` i `Remove` metody przy użyciu następującego kodu, jak pokazano na żółty Wyróżnij:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Ten kod tworzy `Student` jednostki przy użyciu tylko wartość klucza podstawowego, a następnie ustawia stan jednostki `Deleted`. To wszystko, który potrzebuje platformy Entity Framework, aby usunąć jednostkę.

     Jak wspomniano, `HttpGet` `Delete` metoda nie powoduje usunięcia danych. Wykonywanie operacji usuwania w odpowiedzi na polecenie GET żądania (lub służącego wykonaniem każdej operacji edycji Utwórz operacji lub innej operacji, które zmieniają dane) tworzy to zagrożenie bezpieczeństwa. Aby uzyskać więcej informacji, zobacz [46 Porada # w programie ASP.NET MVC — nie używaj usunąć łącza, ponieważ mogą tworzyć luki w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) na blogu Autor: Stephen Walther.
3. W *Views\Student\Delete.cshtml*, Dodaj komunikat o błędzie między `h2` nagłówek i `h3` nagłówka, jak pokazano w poniższym przykładzie:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Uruchomienia strony, wybierając **studentów** kartę i klikając **Usuń** hiperłącze:

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Kliknij przycisk **Usuń**. Nie usunięto uczniów zostanie wyświetlona strona indeksu. (Przedstawimy przykład błędu kodu w akcji obsługi [Obsługa współbieżności](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) samouczków w dalszej części tej serii.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Zapewnienie, że połączenia z bazą danych nie są pozostawione otwarte

Aby upewnić się, że połączenia z bazą danych zostały prawidłowo zamknięte i zasoby, które przechowują zwolnione w górę, powinien zostać wyświetlony jej usunięciu wystąpienia kontekstu. Oznacza to, dlaczego utworzony szkielet kodu udostępnia [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) metody na końcu `StudentController` klasy w *StudentController.cs*, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

Podstawowy `Controller` już klasy implementuje `IDisposable` interfejsu, dlatego ten kod po prostu dodaje zastąpienie `Dispose(bool)` metodę, aby jawnie Usuń wystąpienie kontekstu.

## <a name="summary"></a>Podsumowanie

Masz teraz kompletny zestaw stron, które wykonywać proste operacje CRUD dla `Student` jednostek. Pomocnicy MVC jest używany do generowania elementów interfejsu użytkownika dla pól danych. Aby uzyskać więcej informacji na temat pomocników MVC zobacz [renderowania pomocników HTML za pomocą formularza](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (strona jest dla platformy MVC 3, ale jest nadal istotne dla MVC 4).

W następnym samouczku będziesz rozwiń funkcje strony indeksu, dodając sortowanie i stronicowanie.

Linki do innych zasobów platformy Entity Framework można znaleźć w [Mapa zawartości dostępu do danych ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Poprzednie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [dalej](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
