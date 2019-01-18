---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Pobieranie i wyświetlanie danych za pomocą modelu formularzy sieci web i powiązania | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji więcej proste —...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: c53c27f4852eab9813bd917315111e7cd3b04953
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396288"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Pobieranie i wyświetlanie danych za pomocą wiązania modelu i formularzy sieci web
====================

> W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji prostszą niż rozwiązywania problemów związanych z danymi obiektów źródła (takich jak kontrolki ObjectDataSource lub SqlDataSource). Ta seria rozpoczyna się od wprowadzające informacje i przenosi do bardziej zaawansowanych pojęciach w kolejnych samouczkach.
> 
>  Wzorca wiązania modelu współpracuje z dowolnym technologii dostępu do danych. W tym samouczku użyjesz programu Entity Framework, ale można użyć technologii dostępu do danych, który jest najbardziej znane. Z kontrolki powiązane z danymi serwera, na przykład kontrolki GridView, ListView, DetailsView lub FormView określane są nazwy metod na potrzeby wybierając, aktualizowanie, usuwanie i tworzenie danych. W tym samouczku należy określić wartość dla metody SelectMethod. 
> 
> W ramach tej metody można przewidzieć logikę pobierania danych. W następnym samouczku ustawi wartości dla operacji UpdateMethod, DeleteMethod i operacji InsertMethod.
>
> Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w C# lub Visual Basic. Do pobrania kod działa, za pomocą programu Visual Studio 2012 i nowszych. Używa szablonu programu Visual Studio 2012, który różni się nieco od szablonu programu Visual Studio 2017, przedstawione w tym samouczku.
> 
> W tym samouczku możesz uruchomić aplikację w programie Visual Studio. Można również wdrożyć aplikację na dostawcy usług hostingowych i był dostępny za pośrednictwem Internetu. Firma Microsoft oferuje bezpłatny internetowy hostowanie do 10 witryn sieci web w  
>  [bezpłatne konto wersji próbnej platformy Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Aby uzyskać informacje o sposobie wdrażania projektu sieci web programu Visual Studio do usługi Azure App Service Web Apps, zobacz [wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) serii. Ten samouczek pokazuje również, jak użyć migracje Code First Framework jednostki do wdrożenia bazy danych programu SQL Server do usługi Azure SQL Database.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> - Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017
>   
> W tym samouczku współpracuje również z programu Visual Studio 2012 i Visual Studio 2013, ale istnieją pewne różnice w szablonie Projekt i interfejsu użytkownika.


## <a name="what-youll-build"></a>Będziesz tworzyć

W ramach tego samouczka należy:

* Tworzenie obiektów danych, które odzwierciedlają uniwersytetu z studentów rejestrację na kursy
* Tworzenie tabel bazy danych z obiektów
* Wypełniania bazy danych przy użyciu danych testowych
* Wyświetlanie danych w formularzu sieci web

## <a name="create-the-project"></a>Utwórz projekt

1. W programie Visual Studio 2017, należy utworzyć **aplikacji sieci Web platformy ASP.NET (.NET Framework)** projekt o nazwie **ContosoUniversityModelBinding**.

   ![Tworzenie projektu](retrieving-data/_static/image19.png)

2. Kliknij przycisk **OK**. Zostanie wyświetlone okno dialogowe, aby wybrać szablon.

   ![Wybierz formularze sieci web](retrieving-data/_static/image3.png)

3. Wybierz **formularzy sieci Web** szablonu. 

4. W razie potrzeby zmień uwierzytelnianie, aby **indywidualne konta użytkowników**. 

5. Wybierz **OK** do tworzenia projektu.

## <a name="modify-site-appearance"></a>Modyfikowanie wyglądu witryny

   Wprowadzić kilka zmian, aby dostosować wygląd witryny. 
   
   1. Otwórz plik Site.Master.
   
   2. Zmienianie tytułu, aby wyświetlić **Contoso University** i nie **My ASP.NET Application**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. Zmień tekst nagłówka z **Nazwa aplikacji** do **Contoso University**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. Zmień łącza nagłówka nawigacji do lokacji odpowiednią z nich. 
   
      Usuń linki do **o** i **skontaktuj się z pomocą** i zamiast tego należy połączyć **studentów** strony, który zostanie utworzony.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. Zapisz Site.Master.

## <a name="add-a-web-form-to-display-student-data"></a>Dodaj formularz sieci web, aby wyświetlić dane dla uczniów

   1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt, wybierz **Dodaj** i następnie **nowy element**. 
   
   2. W **Dodaj nowy element** okno dialogowe, wybierz opcję **formularz sieci Web ze stroną wzorcową** szablonu i nadaj mu nazwę **Students.aspx**.

      ![Tworzenie strony](retrieving-data/_static/image5.png)

   3. Wybierz pozycję **Dodaj**.
   
   4. Formularz sieci web stronę wzorcową, można wybrać **Site.Master**.
   
   5. Kliknij przycisk **OK**.
   

## <a name="add-the-data-model"></a>Dodawanie modelu danych

W **modeli** folderu, Dodaj klasę o nazwie **UniversityModels.cs**.

   1. Kliknij prawym przyciskiem myszy **modeli**, wybierz opcję **Dodaj**, a następnie **nowy element**. **Dodaj nowy element** pojawi się okno dialogowe.

   2. W menu nawigacji po lewej stronie wybierz **kodu**, następnie **klasy**.

      ![Tworzenie klasy modelu](retrieving-data/_static/image20.png)

   3. Nazwa klasy **UniversityModels.cs** i wybierz **Dodaj**.

      W tym pliku, należy zdefiniować `SchoolContext`, `Student`, `Enrollment`, i `Course` klas w następujący sposób:

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      `SchoolContext` Klasa pochodzi od `DbContext`, która zarządza połączenia z bazą danych i zmiany w danych.

      W `Student` klasy, zwróć uwagę, atrybuty stosowane do `FirstName`, `LastName`, i `Year` właściwości. W tym samouczku korzysta z tych atrybutów sprawdzania poprawności danych. Aby uprościć kod, tylko te właściwości są oznaczone za pomocą atrybutów sprawdzania poprawności danych. W projekcie rzeczywistych atrybutów sprawdzania poprawności będzie dotyczą wszystkich właściwości wymagające weryfikacji.

   4. Save UniversityModels.cs.

## <a name="set-up-the-database-based-on-classes"></a>Konfigurowanie bazy danych oparte na klasach

W tym samouczku [migracje Code First](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) do tworzenia obiektów i tabel bazy danych programu. Te tabele są przechowywane informacje o uczniów i ich kursy.

   1. Wybierz **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.

   2. W **Konsola Menedżera pakietów**, uruchom następujące polecenie:  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      Jeśli polecenie zakończy się pomyślnie, zostanie wyświetlony komunikat z informacją, że włączono migracji.

      ![Włączanie funkcji migracje](retrieving-data/_static/image8.png)

      Należy zauważyć, że plik o nazwie *Configuration.cs* została utworzona. `Configuration` Klasa ma `Seed` metody, która wstępnie wypełnić tabele bazy danych z danymi.

## <a name="pre-populate-the-database"></a>Wstępne wypełnianie bazy danych

   1. Otwórz Configuration.cs.
   
   2. Dodaj następujący kod do `Seed` metody. Ponadto Dodaj `using` poufności informacji dotyczące `ContosoUniversityModelBinding. Models` przestrzeni nazw.

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. Zapisz Configuration.cs.

   4. W konsoli Menedżera pakietów, uruchom polecenie **początkowej migracji Dodaj**.

   5. Uruchom polecenie **update-database**.

      Jeśli wystąpi wyjątek podczas uruchamiania tego polecenia `StudentID` i `CourseID` wartości może się różnić od `Seed` wartości metody. Otwórz te tabele bazy danych i Znajdź istniejące wartości dla `StudentID` i `CourseID`. Dodaj te wartości do kodu rozmieszczania `Enrollments` tabeli.

## <a name="add-a-gridview-control"></a>Dodawanie kontrolki GridView

Z wypełniania bazy danych możesz teraz pobrać te dane i wyświetlania ich. 

1. Otwórz Students.aspx.

2. Znajdź `MainContent` symbol zastępczy. W ramach tego symbolu zastępczego, należy dodać **GridView** formant, który zawiera ten kod.

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   Kwestii, które należy zwrócić uwagę:
   * Zwróć uwagę, wartość ustawioną dla `SelectMethod` właściwości w elemencie GridView. Ta wartość określa metodę używaną do pobierania danych widoku GridView, które są tworzone w następnym kroku. 
   
   * `ItemType` Właściwość jest ustawiona na `Student` klasy utworzonej wcześniej. To ustawienie umożliwia odwoływać się do właściwości klasy w znaczniku. Na przykład `Student` klasa ma kolekcję o nazwie `Enrollments`. Można użyć `Item.Enrollments` pobrania tej kolekcji, a następnie użyć [składni LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) można pobrać każdy uczeń użytkownika zarejestrowane środki na korzystanie z sumą.
   
3. Zapisz Students.aspx.

## <a name="add-code-to-retrieve-data"></a>Dodaj kod, aby pobrać dane

   W pliku związanym z kodem Students.aspx, Dodaj metodę określone dla `SelectMethod` wartość. 
   
   1. Otwórz Students.aspx.cs.
   
   2. Dodaj `using` instrukcji dla `ContosoUniversityModelBinding. Models` i `System.Data.Entity` przestrzeni nazw.

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. Dodaj metodę określona dla `SelectMethod`:

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      `Include` Klauzuli poprawia wydajność zapytań, ale nie jest wymagana. Bez `Include` klauzuli, dane są pobierane przy użyciu [ *powolne ładowanie*](https://en.wikipedia.org/wiki/Lazy_loading), które obejmuje wysyłanie oddzielnego zapytania do bazy danych zawsze powiązane dane są pobierane. Za pomocą `Include` klauzuli, dane są pobierane przy użyciu *wczesne ładowanie*, co oznacza, że zapytanie pojedynczej bazy danych pobiera wszystkie powiązane dane. Powiązane dane nie będą używane, wczesne ładowanie jest mniej wydajne rozwiązanie, ponieważ dane są pobierane. Jednak w takim przypadku wczesne ładowanie zapewnia najlepszą wydajność, ponieważ powiązane dane jest wyświetlany dla każdego rekordu.

      Aby uzyskać więcej informacji na temat zagadnienia związane z wydajnością podczas ładowania powiązanych danych, zobacz **leniwy Eager i jawne ładowanie powiązanych danych** sekcji [odczytywania danych powiązanych z platformą Entity Framework na platformie ASP.NET Aplikacja MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) artykułu.

      Domyślnie dane są sortowane według wartości właściwości oznaczonym jako klucz. Możesz dodać `OrderBy` klauzulę, aby określić wartość sortowania. W tym przykładzie domyślne `StudentID` właściwość jest używana do sortowania. W [sortowanie, stronicowanie i filtrowanie danych](sorting-paging-and-filtering-data.md) artykułu, użytkownik jest włączona, aby wybrać kolumnę sortowania.
 
   4. Zapisz Students.aspx.cs.

## <a name="run-your-application"></a>Uruchamianie aplikacji 

Uruchom aplikację sieci web (**F5**) i przejdź do **studentów** stronę, która wyświetla następujące czynności:

   ![Pokaż dane](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Automatyczne generowanie metody wiązania modelu

Podczas pracy za pośrednictwem tej serii samouczków, możesz po prostu skopiować kod z tego samouczka, do projektu. Jednak jedną wadą tego podejścia jest, że użytkownik może nie zostaną powiadomieni o funkcji dostarczane przez program Visual Studio, aby automatycznie wygenerować kod dla metody wiązania modelu. Podczas pracy z własnych projektów, automatyczne generowanie kodu można zaoszczędzić czas i pomoc, uzyskasz zorientować się, jak zaimplementować operacji. W tej sekcji opisano funkcję generowanie kodu automatyczne. Ta sekcja ma tylko charakter informacyjny i nie zawiera żadnego kodu, potrzebne do zaimplementowania w projekcie. 

Podczas ustawiania wartości `SelectMethod`, `UpdateMethod`, `InsertMethod`, lub `DeleteMethod` właściwości w kodzie znaczników, możesz wybrać **Tworzenie nowej metody** opcji.

![Utwórz metodę](retrieving-data/_static/image18.png)

Program Visual Studio nie tylko tworzy metodę w kodem przy użyciu prawidłowego podpisu, ale również generuje kod implementacji do wykonania tej operacji. Jeśli najpierw ustawić `ItemType` właściwości przed użyciem automatyczne generowanie kodu funkcji, które należy wpisać używa wygenerowany kod dla operacji. Na przykład podczas ustawiania `UpdateMethod` właściwość, poniższy kod jest generowany automatycznie:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Ponownie ten kod nie musi być dodany do projektu. W następnym samouczku będziesz implementować metody do aktualizowania, usuwania i dodawania nowych danych.

## <a name="summary"></a>Podsumowanie

W tym samouczku utworzono klasy modelu danych i wygenerowany bazy danych na podstawie tych klas. Tabele bazy danych są wypełnione danych testowych. Używane wiązania modelu do pobierania danych z bazy danych, a następnie wyświetlane dane w widoku GridView.

W ciągu następnych [samouczek](updating-deleting-and-creating-data.md) w tej serii włączysz, aktualizowanie, usuwanie i tworzenie danych.

> [!div class="step-by-step"]
> [Next](updating-deleting-and-creating-data.md)
