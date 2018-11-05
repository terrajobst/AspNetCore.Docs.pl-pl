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
ms.openlocfilehash: b05f3780d7c4e4734b35c0d9377a89d6f3edb0f8
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021342"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Pobieranie i wyświetlanie danych za pomocą wiązania modelu i formularzy sieci web
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tej serii samouczków pokazano podstawowych aspektów projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązanie modelu sprawia, że dane interakcji prostszą niż rozwiązywania problemów związanych z danymi obiektów źródła (takich jak kontrolki ObjectDataSource lub SqlDataSource). Ta seria rozpoczyna się od wprowadzające informacje i przenosi do bardziej zaawansowanych pojęciach w kolejnych samouczkach.
> 
> Wzorca wiązania modelu współpracuje z dowolnym technologii dostępu do danych. W tym samouczku użyjesz programu Entity Framework, ale można użyć technologii dostępu do danych, który jest najbardziej znane. Z kontrolki powiązane z danymi serwera, na przykład kontrolki GridView, ListView, DetailsView lub FormView określane są nazwy metod na potrzeby wybierając, aktualizowanie, usuwanie i tworzenie danych. W tym samouczku należy określić wartość dla metody SelectMethod.
> 
> W ramach tej metody można przewidzieć logikę pobierania danych. W następnym samouczku ustawi wartości dla operacji UpdateMethod, DeleteMethod i operacji InsertMethod.
> 
> Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB. Kod do pobrania w programach Visual Studio 2012 lub Visual Studio 2013. Używa szablonu programu Visual Studio 2012, który różni się nieco od szablonu programu Visual Studio 2013, przedstawione w tym samouczku.
> 
> W tym samouczku możesz uruchomić aplikację w programie Visual Studio. Można także udostępnić aplikację za pośrednictwem Internetu przez wdrożenie jej dostawcy usług hostingowych. Firma Microsoft oferuje bezpłatny internetowy hostowanie do 10 witryn sieci web w  
>  [bezpłatne konto wersji próbnej platformy Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Aby uzyskać informacje o sposobie wdrażania projektu sieci web programu Visual Studio do usługi Azure App Service Web Apps, zobacz [wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) serii. Ten samouczek pokazuje również, jak użyć migracje Code First Framework jednostki do wdrożenia bazy danych programu SQL Server do usługi Azure SQL Database.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - Microsoft Visual Studio 2013 lub programu Microsoft Visual Studio Express 2013 for Web
>   
> 
> W tym samouczku współpracuje również z programu Visual Studio 2012, ale będzie istnieć pewne różnice w szablonie Projekt i interfejsu użytkownika.


## <a name="what-youll-build"></a>Będziesz tworzyć

W ramach tego samouczka należy:

1. Tworzenie obiektów danych, które odzwierciedlają uniwersytetu z studentów rejestrację na kursy
2. Tworzenie tabel bazy danych z obiektów
3. Wypełniania bazy danych przy użyciu danych testowych
4. Wyświetlanie danych w formularzu sieci web

## <a name="set-up-project"></a>Konfigurowanie projektu

W programie Visual Studio 2013, Utwórz nową **aplikacji sieci Web ASP.NET** o nazwie **ContosoUniversityModelBinding**.

![Tworzenie projektu](retrieving-data/_static/image2.png)

Wybierz szablon formularzy sieci Web i pozostaw opcje domyślne. Kliknij przycisk OK, aby skonfigurować projekt.

![Wybierz formularze sieci web](retrieving-data/_static/image3.png)

Najpierw należy będzie kilka niewielkich zmian, aby dostosować wygląd witryny. Otwórz **Site.Master** pliku i Zmień tytuł na obejmują University Contoso zamiast Moja aplikacja platformy ASP.NET.

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

Następnie zmień tekst nagłówka z **Nazwa aplikacji** do **Contoso University**.

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

Również w Site.Master, należy zmienić łącza nawigacji, które są wyświetlane w nagłówku, aby odzwierciedlić stron, które są istotne dla tej lokacji. Nie musisz albo **o** strony lub **skontaktuj się z pomocą** strony można usunąć tych łączy. Zamiast tego konieczne będzie łącze do strony o nazwie **studentów**. Ta strona nie został jeszcze utworzony.

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

Zapisz i zamknij Site.Master.

Teraz utworzysz formularz sieci web do wyświetlania danych dla uczniów. Kliknij prawym przyciskiem myszy projekt, a **Dodaj** **nowy element**. Wybierz **formularz sieci Web ze stroną wzorcową** szablonu i nadaj mu nazwę **Students.aspx**.

![Tworzenie strony](retrieving-data/_static/image5.png)

Wybierz **Site.Master** jako stronę wzorcową dla nowego formularza sieci web.

## <a name="create-the-data-models-and-database"></a>Tworzenie modeli danych i bazy danych

Migracje Code First użyje do tworzenia obiektów i odpowiednie tabele bazy danych. Te tabele zapisze informacje na temat uczniów i ich kursy.

W folderze modeli, Dodaj nową klasę o nazwie **UniversityModels.cs**.

![Tworzenie klasy modelu](retrieving-data/_static/image7.png)

W tym pliku zdefiniuj klasy SchoolContext, studentów, rejestracji i kurs w następujący sposób:

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

Klasa SchoolContext pochodzi od typu DbContext, który zarządza połączenia z bazą danych i zmiany w danych.

W klasie dla uczniów, zwróć uwagę, atrybuty, które zostały zastosowane do **FirstName**, **LastName**, i **roku** właściwości. Te atrybuty będą używane do sprawdzania poprawności danych w ramach tego samouczka. W celu uproszczenia kodu dla tego projektu demonstracji, tylko te właściwości, które zostały oznaczone za pomocą atrybutów sprawdzania poprawności danych. W projekcie rzeczywistych atrybutów sprawdzania poprawności będzie dotyczą wszystkich właściwości, które muszą zweryfikowanych danych, takich jak właściwości w klasach rejestracji i kursów.

Save UniversityModels.cs.

Narzędzia do migracji Code First użyje do skonfigurowania bazy danych na podstawie tych klas.

W **Konsola Menedżera pakietów**, uruchom polecenie:  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

Jeśli polecenie zakończy się pomyślnie, zostanie wyświetlony komunikat z informacją, że migracja zostanie włączona,

![Włączanie funkcji migracje](retrieving-data/_static/image8.png)

Należy zauważyć, że nowy plik o nazwie **Configuration.cs** została utworzona. W programie Visual Studio otworzyć tego pliku jest automatycznie po jego utworzeniu. Zawiera klasę konfiguracji **inicjatora** metody, która umożliwia wstępne wypełnianie tabel bazy danych z danymi.

Dodaj następujący kod do metody inicjatora. Musisz dodać **przy użyciu** poufności informacji dotyczące **ContosoUniversityModelBinding.Models** przestrzeni nazw.

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

Zapisz Configuration.cs.

W konsoli Menedżera pakietów, uruchom polecenie `add-migration initial`.

Następnie uruchom polecenie `update-database`.

Jeśli wystąpi wyjątek podczas uruchamiania tego polecenia, jest to możliwe, że wartości StudentID i CourseID mają różne wartości w metodzie inicjatora. Otwórz tych tabel w bazie danych i Znajdź istniejące wartości StudentID i CourseID. Dodaj te wartości w kodzie do wstępnego wypełniania tabeli rejestracji.

Plik bazy danych został dodany, ale jest on ukryty w projekcie. Kliknij przycisk **Pokaż wszystkie pliki** do wyświetlenia pliku.

![Pokaż wszystkie pliki](retrieving-data/_static/image10.png)

Zwróć uwagę, pojawi się w aplikacji pliku .mdf\_folderu danych.

![plik bazy danych](retrieving-data/_static/image12.png)

Kliknij dwukrotnie plik mdf, a następnie otworzyć Eksploratora serwera. Tabele teraz istnieją i są wypełniane przy użyciu danych.

![tabele bazy danych](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>Wyświetlanie danych z powiązanych tabel i studentów

Z danymi w bazie danych możesz teraz przystąpić do pobierania danych i wyświetlić go na stronie sieci web. Użyjesz **GridView** formantu, aby wyświetlić dane w kolumn i wierszy.

Otwórz Students.aspx i zlokalizuj **MainContent** symbol zastępczy. W ramach tego symbolu zastępczego, należy dodać **GridView** formant, który zawiera następujący kod.

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

Istnieje kilka ważnych koncepcji, w tym kodzie znaczników dla Ciebie należy zwrócić uwagę. Najpierw zwróć uwagę, czy wartość jest ustawiona dla **metody SelectMethod** właściwości w elemencie GridView. Ta wartość Określa nazwę metody, która służy do pobierania danych dla widoku GridView. Ta metoda utworzy w następnym kroku. Po drugie, zwróć uwagę, że **ItemType** właściwość jest ustawiona na klasę dla uczniów, która została utworzona wcześniej. Ustawiając tę wartość, może odnosić się do właściwości tej klasy w kodzie znaczników. Na przykład klasa uczniów zawiera kolekcję o nazwie rejestracji. Możesz użyć **Item.Enrollments** do pobrania tej kolekcji, a następnie pobrać sumy zarejestrowanych środki na korzystanie z dla każdego ucznia za pomocą składni LINQ.

W pliku związanym z kodem, należy dodać metodę, która jest określona dla **metody SelectMethod** wartość. Otwórz **Students.aspx.cs**i Dodaj **przy użyciu** instrukcji dla **ContosoUniversityModelBinding.Models** i **System.Data.Entity**przestrzeni nazw.

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

Następnie dodaj następującą metodę. Zwróć uwagę, czy nazwa ta metoda jest zgodna Nazwa parametru metody SelectMethod.

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

**Include** klauzuli poprawia wydajność kwerendy, ale nie jest istotne dla kwerendy do pracy. Bez klauzuli Include będzie można odzyskać danych przy użyciu ładowania z opóźnieniem, które obejmuje wysłanie oddzielnych zapytań do bazy danych zawsze związane z dane są pobierane. Dostarczając klauzuli Include, dane są pobierane za pomocą wczesne ładowanie, co oznacza, wszystkie powiązane dane są pobierane za pomocą jednego zapytania bazy danych. W przypadku większości powiązanych danych nie jest używane, eager ładowania może być mniej wydajne rozwiązanie, ponieważ jest pobierana większej ilości danych. Jednak w takim przypadku wczesne ładowanie zapewnia najlepszą wydajność, ponieważ powiązane dane jest wyświetlany dla każdego rekordu.

Aby dowiedzieć się więcej o aspekt dotyczący wydajności podczas ładowania powiązanych danych, zobacz sekcję pod tytułem **leniwy Eager i jawne ładowanie powiązanych danych** w [odczytywania danych powiązanych z platformą Entity Framework w ASP Aplikacja MVC platformy .NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tematu.

Domyślnie dane są sortowane według wartości właściwości oznaczonym jako klucz. Możesz dodać klauzulę OrderBy, aby określić inną wartość do sortowania. W tym przykładzie właściwość StudentID domyślnej jest używana do sortowania. W [sortowanie, stronicowanie i filtrowanie danych](sorting-paging-and-filtering-data.md) tematu spowoduje włączenie użytkownikowi na wybranie kolumny sortowania.

Uruchom aplikację sieci web i przejdź do strony studentów. Na stronie studentów są wyświetlane następujące informacje dla uczniów.

![Pokaż dane](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Automatyczne generowanie metody wiązania modelu

Podczas pracy za pośrednictwem tej serii samouczków, możesz po prostu skopiować kod z tego samouczka, do projektu. Jednak jedną wadą tego podejścia jest, że użytkownik może nie zostaną powiadomieni o funkcji dostarczane przez program Visual Studio, aby automatycznie wygenerować kod dla metody wiązania modelu. Podczas pracy z własnych projektów, automatyczne generowanie kodu można zaoszczędzić czas i pomoc, uzyskasz zorientować się, jak zaimplementować operacji. W tej sekcji opisano funkcję generowanie kodu automatyczne. Ta sekcja ma tylko charakter informacyjny i nie zawiera żadnego kodu, potrzebne do zaimplementowania w projekcie.

Podczas ustawiania wartości **metody SelectMethod**, **operacji UpdateMethod**, **operacji InsertMethod**, lub **DeleteMethod** właściwości w kodzie znaczników Możesz wybrać **Tworzenie nowej metody** opcji.

![Utwórz nową metodę](retrieving-data/_static/image18.png)

Program Visual Studio nie tylko tworzy metodę w kodem przy użyciu prawidłowego podpisu, ale również generuje kod implementacji pomocne podczas wykonywania operacji. Jeśli najpierw ustawić **ItemType** właściwości przed rozpoczęciem korzystania z funkcji generowania kodu automatyczne wygenerowany kod użyje tego typu dla operacji. Na przykład ustawiając właściwość operacji UpdateMethod, poniższy kod jest generowany automatycznie:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Ponownie powyższy kod nie musi być dodany do projektu. W następnym samouczku wdroży metod aktualizowania, usuwania i dodawania nowych danych.

## <a name="conclusion"></a>Wniosek

W tym samouczku utworzono klasy modelu danych i wygenerowany bazy danych na podstawie tych klas. Tabele bazy danych są wypełnione danych testowych. Używane wiązania modelu do pobierania danych z bazy danych, a następnie wyświetlane dane w widoku GridView.

W ciągu następnych [samouczek](updating-deleting-and-creating-data.md) w tej serii umożliwi aktualizowanie, usuwanie i tworzenie danych.

> [!div class="step-by-step"]
> [Next](updating-deleting-and-creating-data.md)
