---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: "Trwa pobieranie i wyświetlanie danych z modelu formularzy sieci web i powiązanie | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązania modelu sprawia, że dane interakcji więcej proste-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: e750250285fcb0088da284588d721ac21e73820c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Trwa pobieranie i wyświetlanie danych z wiązania modelu i formularzy sieci web
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Ten samouczek serii przedstawiono podstawowe aspekty projektu formularzy sieci Web ASP.NET przy użyciu wiązania modelu. Wiązania modelu sprawia, że dane interakcji więcej proste niż dotyczących danych obiekty źródła (na przykład element ObjectDataSource lub SqlDataSource). Ta seria rozpoczyna się od wprowadzające informacje i przechodzi do bardziej zaawansowanych pojęcia w kolejnych samouczkach.
> 
> Wzorca wiązania modelu działa w przypadku innych technologii dostępu do danych. W tym samouczku użyjesz programu Entity Framework, ale można użyć technologii dostępu do danych, która jest najbardziej znane. Z formantu powiązanego z danymi serwera, takich jak kontrola GridView ListView, widoku DetailsView albo FormView możesz określić nazwy metody zaznaczenie, aktualizowanie i tworzenia danych. W tym samouczku będzie określić wartość dla metody SelectMethod.
> 
> W ramach tej metody musisz podać logikę do odzyskiwania danych. W następnym samouczku spowoduje ustawienie wartości dla właściwości UpdateMethod i DeleteMethod InsertMethod.
> 
> Możesz [Pobierz](https://go.microsoft.com/fwlink/?LinkId=286116) kompletnego projektu w języku C# lub VB. Kod do pobrania współpracuje z programu Visual Studio 2012 lub Visual Studio 2013. Używa szablonu programu Visual Studio 2012, który jest nieco inne niż szablon programu Visual Studio 2013 przedstawiona w tym samouczku.
> 
> W samouczku uruchomieniu aplikacji w programie Visual Studio. Można również udostępnić aplikacji w Internecie przez wdrożenie jej do dostawcy hostingu. Firma Microsoft oferuje usługi hostingu sieci web wolnego maksymalnie 10 witryn sieci web w  
>  [bezpłatne konto wersji próbnej platformy Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Aby uzyskać informacje dotyczące wdrażania projektu sieci web programu Visual Studio do aplikacji sieci Web usługi aplikacji Azure, zobacz [wdrożenia sieci Web ASP.NET przy użyciu programu Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) serii. Ten samouczek również przedstawia sposób użycia migracje Code First Framework jednostki do wdrażania bazy danych SQL Server z bazą danych SQL Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Microsoft Visual Studio 2013 lub programu Microsoft Visual Studio Express 2013 for Web
>   
> 
> W tym samouczku współdziała również z programu Visual Studio 2012, ale będzie pewne różnice w szablonie użytkownika interfejsu i projektu.


## <a name="what-youll-build"></a>Będzie kompilacji

W tym samouczku będziesz:

1. Tworzenie obiektów danych, które odzwierciedlać uniwersytetu z zarejestrowanych w kursy uczniów lub studentów
2. Tworzenie tabel bazy danych z obiektów
3. Wypełnianie bazy danych z danych testowych
4. Wyświetlanie danych w formularzu sieci web

## <a name="set-up-project"></a>Konfigurowanie projektu

W programie Visual Studio 2013, Utwórz nową **aplikacji sieci Web ASP.NET** o nazwie **ContosoUniversityModelBinding**.

![Tworzenie projektu](retrieving-data/_static/image2.png)

Wybierz szablon formularzy sieci Web i pozostaw domyślne opcje. Kliknij przycisk OK, aby skonfigurować projekt.

![Wybierz formularze sieci web](retrieving-data/_static/image3.png)

Najpierw należy będzie kilka niewielkich zmian w celu dostosowania wyglądu witryny. Otwórz **Site.Master** pliku, a następnie zmień tytuł, aby dołączyć University Contoso zamiast Moja aplikacja platformy ASP.NET.

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

Następnie należy zmienić tekst nagłówka z **Nazwa aplikacji** do **Contoso University**.

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

Również w Site.Master, należy zmienić łącza nawigacji wyświetlane w nagłówku, aby odzwierciedlić stron, które mają zastosowanie do tej witryny. Nie musisz albo **o** strony lub **skontaktuj się z** strony, można usunąć te łącza. Zamiast tego należy łącze do strony o nazwie **studentów**. Ta strona nie został jeszcze utworzony.

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

Zapisz i zamknij Site.Master.

Teraz utworzysz formularza sieci web do wyświetlania danych studenta. Kliknij prawym przyciskiem myszy projekt, a **Dodaj** **nowy element**. Wybierz **formularza sieci Web ze stroną wzorcową** szablonu i nadaj mu nazwę **Students.aspx**.

![Tworzenie strony](retrieving-data/_static/image5.png)

Wybierz **Site.Master** jako stronę wzorcową dla nowego formularza sieci web.

## <a name="create-the-data-models-and-database"></a>Tworzenie modeli danych i bazy danych

Migracje Code First użyje do tworzenia obiektów i odpowiadających jej tabel bazy danych. Te tabele zapisze informacje na temat studentów i ich kursy.

W folderze modeli, Dodaj nową klasę o nazwie **UniversityModels.cs**.

![Utwórz klasę modelu](retrieving-data/_static/image7.png)

W tym pliku zdefiniuj klasy SchoolContext, uczniów rejestracji i kursu w następujący sposób:

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

Klasa SchoolContext pochodzi od typu DbContext, która zarządza połączenia z bazą danych i zmiany w danych.

W klasie uczniów zauważyć atrybuty, które zostały zastosowane do **imię**, **nazwisko**, i **roku** właściwości. Te atrybuty będą używane do sprawdzania poprawności danych w tym samouczku. Aby uprościć kod dla tego projektu wykazanie, tylko te właściwości zostały oznaczone atrybutów sprawdzania poprawności danych. W projekcie prawdziwe będą miały zastosowania atrybutów sprawdzania poprawności na wszystkie właściwości wymagające zatwierdzonych dane, takie jak właściwości w klasach rejestracji oraz przebiegu.

Zapisz UniversityModels.cs.

Narzędzia dla migracje Code First użyje do konfigurowania bazy danych na podstawie tych klas.

W **Konsola Menedżera pakietów**, uruchom polecenie:  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

Jeśli polecenie zostanie wykonane pomyślnie zostanie wyświetlony komunikat informujący, że migracje zostały włączone,

![Włącz migracji](retrieving-data/_static/image8.png)

Należy zauważyć, że nowy plik o nazwie **Configuration.cs** został utworzony. W programie Visual Studio ten plik jest otwarty automatycznie po jego utworzeniu. Zawiera klasę konfiguracji **inicjatora** metody, dzięki czemu można wstępnie wypełnić tabel bazy danych z danych testowych.

Dodaj następujący kod do metody inicjatora. Musisz dodać **przy użyciu** instrukcji dla **ContosoUniversityModelBinding.Models** przestrzeni nazw.

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

Zapisz Configuration.cs.

W konsoli Menedżera pakietów, uruchom polecenie `add-migration initial`.

Następnie uruchom polecenie `update-database`.

Jeśli wystąpi wyjątek podczas uruchamiania tego polecenia, jest to możliwe, że wartości StudentID i CourseID mają różne wartości w metodzie inicjatora. Otwórz tych tabel w bazie danych i Znajdź istniejące wartości StudentID i CourseID. Dodaj te wartości w kodzie do wstępnego wypełniania tabeli rejestracji.

Plik bazy danych został dodany, ale jest on ukryty w projekcie. Kliknij przycisk **Pokaż wszystkie pliki** Aby wyświetlić plik.

![Pokaż wszystkie pliki](retrieving-data/_static/image10.png)

Zwróć uwagę, plików .mdf jest teraz wyświetlany w aplikacji\_folderem danych.

![plik bazy danych](retrieving-data/_static/image12.png)

Kliknij dwukrotnie plik .mdf i Otwórz Eksploratora serwera. Tabele teraz istnieją i są wypełniane przy użyciu danych.

![tabele bazy danych](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>Wyświetlanie danych z powiązanych tabel i uczniów lub studentów

Z danymi w bazie danych możesz teraz przystąpić do pobierania danych i wyświetl ją na stronie sieci web. Użyjesz **GridView** formantu, aby wyświetlić dane w kolumn i wierszy.

Otwórz Students.aspx i Znajdź **znacznika** symbolu zastępczego. W tym symbolu zastępczego, Dodaj **GridView** formant, który zawiera następujący kod.

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

Istnieje kilka ważnych pojęć w ten kod znaczników, można zauważyć. Najpierw należy zauważyć, że wybrano wartość dla **SelectMethod** właściwości w elemencie widoku GridView. Ta wartość Określa nazwę metody, która służy do pobierania danych widoku GridView. Ta metoda zostanie utworzona w następnym kroku. Po drugie, zwróć uwagę, że **ItemType** właściwość jest ustawiona na klasie uczniów utworzony wcześniej. Ustawiając tę wartość, mogą odwoływać się do właściwości tej klasy w kodzie znaczników. Na przykład klasa uczniów zawiera kolekcję o nazwie rejestracji. Można użyć **Item.Enrollments** pobrać tej kolekcji, a następnie użyć składni LINQ można pobrać sumy środków zarejestrowanych dla użytkowników.

W pliku związanym z kodem, konieczne jest dodanie metody, która jest określona dla **SelectMethod** wartość. Otwórz **Students.aspx.cs**i Dodaj **przy użyciu** instrukcje dla **ContosoUniversityModelBinding.Models** i **System.Data.Entity**przestrzeni nazw.

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

Następnie dodaj następującą metodę. Należy zauważyć, że nazwa ta metoda odpowiada nazwie podane dla metody SelectMethod.

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

**Include** klauzuli poprawia wydajność tej kwerendy, ale nie jest istotne dla kwerendy do pracy. Bez klauzuli Dołącz dane będą pobierane przy użyciu opóźnionego ładowania, który obejmuje wysyłania oddzielne zapytania do bazy danych zawsze powiązane się, że dane są pobierane. Zapewniając klauzuli Include, dane są pobierane przy użyciu ładowania wczesny, co oznacza wszystkie dotyczące go dane są pobierane za pomocą pojedynczego zapytania bazy danych. W przypadku większości powiązanych danych nie jest używany, wczesny ładowania może być mniej wydajne, ponieważ dane są pobierane. Jednak w takim przypadku wczesny ładowania zapewnia najlepszą wydajność, ponieważ powiązane dane są wyświetlane dla każdego rekordu.

Więcej informacji na temat wydajności brany pod uwagę podczas ładowania związanych z danych, zobacz sekcję **Lazy, Eager i jawnego ładowania powiązanych danych** w [odczytu danych powiązanych z programu Entity Framework w ASP Aplikacji .NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tematu.

Domyślnie dane są sortowane według wartości właściwości oznaczone jako klucz. Możesz dodać klauzula OrderBy, aby określić inną wartość dla sortowania. W tym przykładzie właściwość StudentID domyślnej jest używana do sortowania. W [sortowanie, stronicowania i filtrowanie danych](sorting-paging-and-filtering-data.md) tematu, umożliwia użytkownikowi wybranie kolumny sortowania.

Uruchom aplikację sieci web i przejdź do strony studentów. Na stronie studentów zawiera następujące informacje dla użytkowników domowych.

![Wyświetlanie danych](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Automatyczne generowanie metody wiązania modelu

Podczas pracy za pośrednictwem tego samouczka serii, możesz po prostu skopiuj kod z tego samouczka do projektu. Jeden wadą tego podejścia jest jednak, że użytkownik może nie zostaną powiadomieni o funkcji dostarczonych przez Visual Studio, aby automatycznie wygenerować kod dla metody wiązania modelu. Podczas pracy z własnych projektów, automatyczne generowanie kodu mogą pomóc zaoszczędzić czasu i uzyskania zorientować się, jak do wykonania operacji. W tej sekcji opisano funkcję generowania kodu automatycznego. Ta sekcja ma tylko charakter informacyjny i nie zawiera żadnego kodu, które należy wdrożyć w projekcie.

Podczas ustawiania wartości **SelectMethod**, **UpdateMethod**, **InsertMethod**, lub **DeleteMethod** właściwości w kodzie znaczników Możesz wybrać **Tworzenie nowej metody** opcji.

![Tworzenie nowej metody](retrieving-data/_static/image18.png)

Visual Studio nie tylko tworzy metodę w kodem o właściwej sygnaturze, ale także generuje kod implementacji pomocne podczas wykonywania operacji. Jeśli najpierw ustawić **ItemType** właściwości przed rozpoczęciem korzystania z funkcji generowania kodu automatyczne wygenerowany kod użyje tego typu dla operacji. Na przykład podczas ustawiania właściwości UpdateMethod, jest automatycznie generowany następujący kod:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Powyższy kod nie musi ponownie, można dodać do projektu. W następnym samouczku zaimplementowaniem metod aktualizowania, usuwania i dodawanie nowych danych.

## <a name="conclusion"></a>Wniosek

W tym samouczku utworzyć klasy modelu danych i wygenerować bazę danych z tych klas. Tabele bazy danych są wypełnione danych testowych. Używane powiązanie modelu do pobierania danych z bazy danych, a następnie wyświetlane dane w widoku GridView.

W następnej [samouczek](updating-deleting-and-creating-data.md) w tej serii spowoduje włączenie aktualizowania, usuwania i tworzenia danych.

>[!div class="step-by-step"]
[Dalej](updating-deleting-and-creating-data.md)
