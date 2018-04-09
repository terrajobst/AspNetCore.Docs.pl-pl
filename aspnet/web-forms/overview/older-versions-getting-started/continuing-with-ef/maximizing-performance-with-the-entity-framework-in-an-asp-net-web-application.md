---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Maksymalizacja wydajności przy użyciu programu Entity Framework 4.0 w aplikacji ASP.NET 4 Web | Dokumentacja firmy Microsoft
author: tdykstra
description: Ten samouczek serii opiera się na aplikację sieci web Contoso University jest tworzony przez wprowadzenie do samouczka serii Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: b85645eebf2822b33df944692736ea9d9b69b9aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Maksymalizacja wydajności przy użyciu programu Entity Framework 4.0 w aplikacji ASP.NET 4 sieci Web
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

> Ten samouczek serii opiera się na aplikację sieci web Contoso University jest tworzony przez [wprowadzenie do korzystania z programu Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) samouczka serii. Jeśli nie została ukończona wcześniejszych samouczki, jako punkt początkowy dla tego samouczka możesz [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) będzie utworzony. Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tworzone przez zakończenie samouczka serii. Jeśli masz pytania dotyczące samouczków, możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


W poprzednim samouczku pokazano, jak obsłużyć konfliktom współbieżności. W tym samouczku przedstawiono opcje zwiększanie wydajności aplikacji sieci web ASP.NET, która używa programu Entity Framework. Dowiesz się kilka metod maksymalizacja wydajności lub do diagnozowania problemów z wydajnością.

Informacje przedstawione w poniższych sekcjach jest mogą być przydatne w szerokiej gamy scenariuszy:

- Efektywnie załadowania powiązane dane.
- Zarządzanie stanem widoku.

Informacje przedstawione w poniższych sekcjach mogą być przydatne w przypadku poszczególnych kwerend tego obecne problemy z wydajnością:

- Użyj `NoTracking` opcja scalania.
- Wstępnie skompilować zapytań LINQ.
- Sprawdź, czy w poleceniach zapytań wysłanych do bazy danych.

Informacje przedstawione w poniższej sekcji potencjalnie przydaje się do aplikacji, które mają bardzo dużej ilości danych modeli:

- Wstępnego wygenerowania widoków.

> [!NOTE]
> Przez wiele czynników, takich jak np. rozmiar danych żądania i odpowiedzi, szybkość kwerend bazy danych, jak wiele żądań, serwer można dodać do kolejki i jak szybko może obsłużyć, a nawet wydajności dowolnego jest wpływ na wydajność aplikacji sieci Web biblioteki skryptu klienta, który może być używany. Jeśli wydajność jest szczególnie ważne w aplikacji lub testowania i obsługi pokazuje, że wydajność aplikacji nie jest zadowalającym, należy wykonać normalnego protokołu dostrajania wydajności. Miary w celu określenia, gdzie występują wąskich gardeł wydajności, a następnie adresować obszarów, które będą miały największy wpływ na ogólną wydajnością.
> 
> W tym temacie skupiono się głównie na sposób, w którym może potencjalnie podnieść wydajność w szczególności Entity Framework w programie ASP.NET. Sugestie, w tym miejscu są przydatne, jeśli okaże się, że dostęp do danych jest jednym z wąskich gardeł wydajności w aplikacji. Z wyjątkiem jak wspomniano, metody omówione nie powinny być uznawane za &quot;najlepsze rozwiązania&quot; ogólnie rzecz biorąc — wiele z nich jest odpowiednie tylko w sytuacjach wyjątkowych lub adres bardzo określonych rodzajów wąskich gardeł wydajności.


Aby uruchomić samouczek, uruchom program Visual Studio i Otwórz aplikację sieci web Contoso University pracy z poprzednich samouczka.

## <a name="efficiently-loading-related-data"></a>Efektywne ładowania powiązane dane

Istnieje kilka sposobów programu Entity Framework może ładować powiązanych danych do właściwości nawigacji jednostki:

- *Powolne ładowanie*. Gdy obiekt jest najpierw przeczytać artykuł, pobrać nie jest powiązane dane. Jednak podczas pierwszej próby dostępu do właściwości nawigacji wymagane dane dla tej właściwości nawigacji jest automatycznie pobierany. Powoduje to wiele zapytań wysłanych do bazy danych — jeden dla sam podmiot i jeden musi zostać pobrany za każdym razem powiązane dane dla tej jednostki. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Ładowanie wczesny*. Podczas odczytywania jednostki powiązane dane są pobierane wraz z jej. Powoduje to zwykle w zapytaniu sprzężenia jednej, która pobiera wszystkie dane potrzebne. Określ wczesny ładowania przy użyciu `Include` metody, jak przedstawiono już w tych samouczkach.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Jawne ładowania*. Jest to podobne do opóźnionego ładowania, z wyjątkiem tego, że jawnie pobrać powiązanych danych w kodzie; go nie jest realizowane automatycznie podczas dostępu do właściwości nawigacji. Ładowanie powiązanych danych ręcznie przy użyciu `Load` metoda właściwości nawigacji kolekcji, lub użyj `Load` metody właściwości odwołania dla właściwości, które przechowywania pojedynczego obiektu. (Na przykład wywołać `PersonReference.Load` metodę, aby załadować `Person` właściwość nawigacji `Department` jednostki.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Ponieważ nie są natychmiast pobrać wartości właściwości, opóźnionego ładowania i jawnego ładowania również zarówno nazywamy *odroczonego ładowania*.

Powolne ładowanie jest to domyślne zachowanie dla kontekstu obiektu, który został wygenerowany przez projektanta. Po otwarciu *SchoolModel.Designer.cs* pliku definiuje klasę obiektu kontekstu, są dostępne trzy metody konstruktora i każdego z nich zawiera następująca instrukcja:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Ogólnie rzecz biorąc Jeśli znasz potrzebne dane dotyczące dla każdy obiekt, do którego pobrane, wczesny ładowania zapewnia najlepszą wydajność, ponieważ pojedynczego zapytania wysyłane do bazy danych jest zazwyczaj bardziej efektywne niż oddzielne zapytania dla każdej jednostki pobrane. Z drugiej strony, aby uzyskać dostęp do właściwości nawigacji jednostki tylko rzadko lub tylko dla małych zestawu jednostek, opóźnionego ładowania lub jawnego ładowania może być bardziej wydajne, ponieważ ładowanie wczesny czy pobrać więcej danych niż trzeba.

W aplikacji sieci web opóźnionego ładowania może być stosunkowo mały wartości mimo wszystko, ponieważ akcje użytkownika, które mają wpływ na potrzeby powiązanych danych została wykonana w przeglądarce, która nie jest połączona w kontekście obiektu, który renderowania strony. Z drugiej strony, gdy użytkownik databind formantu, zwykle wiesz, jakie dane należy i dlatego jest zazwyczaj najlepiej wybrać opcję ładowania wczesny czy ładowanie odłożone na podstawie co to jest właściwy dla każdego scenariusza.

Ponadto formantu z danymi może używać do obiektu jednostki, po usunięciu kontekstu obiektów. W takim przypadku próba opóźnionego ładowania właściwości nawigacji nie powiedzie się. Zostanie wyświetlony komunikat o błędzie jest wyczyszczone: &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

`EntityDataSource` Opóźnionego ładowania domyślnie wyłącza formant. Dla `ObjectDataSource` kontroli używasz bieżący samouczek (lub jeśli kontekst możesz uzyskać dostęp z kodu strony), istnieje kilka sposobów, możesz wprowadzić opóźnionego ładowania domyślnie wyłączone. Można ją wyłączyć, gdy wystąpienia kontekstu obiektów. Na przykład, Dodaj następujący wiersz do metody konstruktora `SchoolRepository` klasy:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Dla aplikacji Contoso University należy podjąć kontekst automatycznie wyłączyć opóźnionego ładowania tak, aby ta właściwość nie ma ustawienie zawsze, gdy kontekst jest tworzone.

Otwórz *SchoolModel.edmx* modelu danych, kliknij powierzchnię projektu, a następnie w okienku właściwości ustaw **opóźnionego ładowania włączone** właściwości `False`. Zapisz i zamknij modelu danych.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Wyświetl stan zarządzania

Aby umożliwić korzystanie z funkcji aktualizacji, strona sieci web programu ASP.NET muszą być przechowywane oryginalnej wartości właściwości jednostki podczas renderowania strony. Podczas przetwarzania formantu ogłaszania zwrotnego można ponownie utworzyć pierwotnego stanu jednostki i Wywołaj jednostki `Attach` metody przed zastosowania zmian i wywoływania `SaveChanges` metody. Domyślnie formantów danych formularzy sieci Web ASP.NET służy widok stanu do przechowywania oryginalnych wartości. Jednak stan widoku może wpłynąć na wydajność, ponieważ jest ona przechowywana w ukryte pola, które może znacznie zwiększyć rozmiar strony, która jest wysyłane do i z przeglądarki.

Techniki zarządzania widoku stanu lub rozwiązań alternatywnych, takich jak stan sesji nie są unikatowe dla programu Entity Framework, w tym samouczku nie działa w tym temacie szczegółowo. Aby uzyskać więcej informacji zobacz łącza na końcu samouczka.

Jednak programu ASP.NET w wersji 4 udostępnia nowy sposób pracy z stan widoku, który każdy Deweloper ASP.NET w aplikacji formularzy sieci Web należy zwrócić uwagę: `ViewStateMode` właściwości. Ta nowa właściwość można ustawić na poziomie strona lub kontrolka i pozwala na Wyłącz stan widoku domyślnie dla strony i włącz ją tylko w przypadku kontrolek, które go potrzebują.

Dla aplikacji, w którym ma kluczowe znaczenie dobrym rozwiązaniem jest zawsze Wyłącz stan widoku na poziomie strony i włącz ją tylko w przypadku kontrolek, które tego wymagają. Rozmiar stan widoku na stronach Contoso University nie znacznie zmniejszyć przez tę metodę, ale aby zobaczyć, jak to działa, należy ją wykonać *Instructors.aspx* strony. Ta strona zawiera wiele formantów, w tym `Label` formantu, który ma stan widoku wyłączone. Brak kontrolek na tej stronie faktycznie musi być włączony stan widoku. ( `DataKeyNames` Właściwość `GridView` kontroli określa stan, które muszą zostać zachowane między odświeżeniami, ale te wartości są przechowywane w stanie formant, który nie ma wpływu na `ViewStateMode` właściwości.)

`Page` Dyrektywy i `Label` znacznika kontrolki obecnie podobnego do następującego:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Wprowadź następujące zmiany:

- Dodaj `ViewStateMode="Disabled"` do `Page` dyrektywy.
- Usuń `ViewStateMode="Disabled"` z `Label` formantu.

Kod znaczników teraz podobnego do następującego:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Stan widoku jest teraz wyłączona dla wszystkich kontrolek. Jeśli później dodać kontroli, które są potrzebne do korzystania ze stanu widoku, wszystko co należy zrobić to obejmują `ViewStateMode="Enabled"` atrybutu dla tego formantu.

## <a name="using-the-notracking-merge-option"></a>Przy użyciu opcji scalania NoTracking

Kontekst pobiera wierszami baz danych i tworzy obiekty jednostki, które reprezentują je, domyślnie on również śledzi te obiekty jednostki przy użyciu jego Menedżer stanu obiektu. Te dane śledzenia działa jako pamięci podręcznej i jest używany podczas aktualizacji jednostki. Ponieważ aplikacji sieci web ma zwykle krótkim okresie obiektów kontekstu, zapytania często zwrócić dane, które nie muszą być śledzone, ponieważ kontekst odczytujący ich zostanie usunięty, zanim dowolne jednostki, które odczytuje są używane ponownie lub aktualizowane.

W ramach jednostki, można określić, czy kontekst śledzi obiekty obiektów przez ustawienie *merge — opcja*. Można ustawić opcji scalania dla poszczególnych zapytań lub zestawów jednostek. Jeśli zostanie ustawiona dla zestawu jednostek, oznacza to, że jest ustawienie domyślne opcji scalania dla wszystkich zapytań, które są tworzone dla tego zestawu jednostek.

Dla aplikacji Contoso University śledzenia nie jest wymagane dla każdego z zestawów jednostek, do których dostęp do repozytorium, umożliwiając ustawienie opcji scalania `NoTracking` dla tych zestawów jednostek, gdy wystąpienia kontekstu obiektów w klasie repozytorium. (Należy pamiętać, że w tym samouczku ustawienie opcji scalania nie będziesz mieć znaczącego wpływu na wydajność aplikacji. `NoTracking` Opcja jest mogą spowodować wzrost wydajności zauważalne tylko w niektórych scenariuszach duże ilości danych.)

Otwórz w folderze DAL *SchoolRepository.cs* plików i Dodawanie metody konstruktora, który ustawia opcję scalania dla obiekt Ustawia, czy repozytorium uzyskuje dostęp do:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Wstępne kompilowanie zapytań LINQ

Po raz pierwszy programu Entity Framework wykonuje zapytanie SQL jednostki w ciągu trwania danego `ObjectContext` wystąpienia, dopiero po pewnym czasie skompilować zapytania. Wynik kompilacji są buforowane, co oznacza, że podczas kolejnych wykonań kodu zapytania jest znacznie szybsza, bo. Zapytania LINQ wykonaj podobnego wzorca z tą różnicą, że część pracy wymaganej do kompilowania zapytania odbywa się za każdym razem, gdy zapytanie jest wykonywane. Innymi słowy do kwerend LINQ, domyślnie nie wszystkie wyniki kompilacji są buforowane.

Jeśli masz zapytania LINQ, która ma być cyklicznie uruchamiany w życie kontekstu obiektów można napisać kod, który powoduje, że wszystkie wyniki kompilacji w pamięci podręcznej podczas pierwszego uruchomienia zapytania LINQ.

Ilustracją, możesz to zrobić dla dwóch `Get` metod w `SchoolRepository` klasy, z których jedna nie przyjmuje żadnych parametrów ( `GetInstructorNames` — metoda) i jedną, która wymaga parametru ( `GetDepartmentsByAdministrator` metody). Te metody są w niezmienionej formie teraz faktycznie nie trzeba skompilowany, ponieważ nie są one zapytań LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Jednak dzięki czemu można wypróbować skompilowane zapytania, można będzie kontynuować tak, jakby była został zapisany jako następujących zapytań LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Można zmienić kod w tych metod, co ma pokazanym powyżej i uruchomić aplikację, aby sprawdzić, czy działa przed kontynuowaniem. Jednak poniższe instrukcje od razu do tworzenia wstępnie skompilowanym ich wersje.

Utwórz plik klasy w *DAL* folderu, nadaj jej nazwę *SchoolEntities.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Ten kod tworzy częściowej klasy, która rozszerza klasę kontekstu automatycznie wygenerowanego obiektu. Klasy częściowej zawiera dwa zapytania LINQ skompilowanych przy użyciu `Compile` metody `CompiledQuery` klasy. Tworzy również metody używane do wywoływania zapytania. Zapisz i zamknij ten plik.

Następnie w *SchoolRepository.cs*, zmień istniejącą `GetInstructorNames` i `GetDepartmentsByAdministrator` klasy metod w repozytorium, aby wywołują skompilowane zapytania:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Uruchom *Departments.aspx* stronę, aby sprawdzić, czy działa tak jak poprzednio. `GetInstructorNames` Metoda jest wywoływana w celu wypełnienia listy rozwijanej administratora i `GetDepartmentsByAdministrator` metoda jest wywoływana po kliknięciu **aktualizacji** celu sprawdź, czy nie instruktora jest więcej niż jednego administratora Dział.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

W aplikacji Contoso University tylko po to, aby zobaczyć jak to zrobić, ponieważ widoczny poprawią wydajność, które zostały wstępnie skompilowanego zapytania. Wstępne kompilowanie zapytań LINQ dodać poziom złożoności kodu, upewnij się, że można wykonać tylko dla zapytania, które faktycznie stanowi wąskie gardła wydajności w aplikacji.

## <a name="examining-queries-sent-to-the-database"></a>Badanie zapytań wysłanych do bazy danych

Podczas analizowania problemów z wydajnością, czasami jest wiedzieć, dokładne poleceń SQL, które wysyła do bazy danych programu Entity Framework. Jeśli pracujesz z `IQueryable` obiektu, jest jednym ze sposobów używania `ToTraceString` metody.

W *SchoolRepository.cs*, Zmień kod w `GetDepartmentsByName` metody do dopasowania w poniższym przykładzie:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

`departments` Zmiennej musi być rzutowane na `ObjectQuery` typu tylko ponieważ `Where` metoda na końcu poprzedni wiersz tworzy `IQueryable` obiekt; bez `Where` metody Rzutowanie nie powinno być konieczne.

Ustaw punkt przerwania na `return` wiersza, a następnie uruchom *Departments.aspx* strony w debugerze. W przypadku trafiony punkt przerwania, sprawdzić `commandText` zmiennej w **zmiennych lokalnych** okno i użyj narzędzia text visualizer (Lupa w **wartość** kolumny) do wyświetlania wartości w **Narzędzia text Visualizer** okna. Można wyświetlić całej polecenie SQL, które wynika z tego kodu:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Alternatywnie funkcji IntelliTrace w programie Visual Studio Ultimate umożliwia wyświetlanie poleceń SQL generowane przez program Entity Framework, które nie wymagają zmiany kodu lub nawet Ustaw punkt przerwania.

> [!NOTE]
> Poniższych procedur można wykonywać tylko wtedy, gdy program Visual Studio Ultimate.


Przywrócić oryginalny kod w `GetDepartmentsByName` metody, a następnie uruchom *Departments.aspx* strony w debugerze.

W programie Visual Studio, wybierz **debugowania** menu, a następnie **IntelliTrace**, a następnie **zdarzeń funkcji IntelliTrace**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

W **IntelliTrace** okna, kliknij przycisk **Przerwij wszystkie**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

**IntelliTrace** okno wyświetla listę ostatnich zdarzeń:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Kliknij przycisk **ADO.NET** wiersza. Zostanie on rozszerzony do pokazania tekstu polecenia:

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Ciąg tekstowy całego polecenia można skopiować do Schowka z **zmiennych lokalnych** okna.

Załóżmy, że pracy z bazą danych z więcej tabel, kolumn niż prosty i relacje `School` bazy danych. Może się okazać, że kwerendę, która zbiera wszystkie informacje wymagane w jednej `Select` instrukcji zawierającej wiele `Join` klauzule staje się zbyt złożona, aby pracować wydajnie. W takim przypadku można przełączyć eager ładowania do jawnego ładowania uprościć zapytanie.

Na przykład, spróbuj zmienić kod w `GetDepartmentsByName` metody w *SchoolRepository.cs*. Obecnie w tej metody ma zapytanie obiektu, który ma `Include` metody `Person` i `Courses` właściwości nawigacji. Zastąp `return` instrukcji z kodem, który wykonuje jawnego ładowania, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Uruchom *Departments.aspx* strony w debugerze i sprawdź **IntelliTrace** okna ponownie jako jak przed. Teraz w przypadku, gdy było pojedynczego zapytania przed, zobacz długo sekwencja je.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Kliknij pierwszy **ADO.NET** wiersza, aby zobaczyć, jakie miało miejsce do złożonego zapytania można wyświetlić wcześniej.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

Zapytanie z działów stał się prostą `Select` zapytań bez `Join` klauzuli, ale następuje oddzielne zapytania, które pobrać Kursy pokrewne i administratora, przy użyciu zestawu dwa zapytania dla każdego działu zwrócony przez oryginalną Zapytanie.

> [!NOTE]
> Pozostawienie opóźnionego ładowania włączone, wzorzec widocznej w tym miejscu, z tego samego zapytania powtarzać, mogą być wynikiem opóźnionego ładowania. Wzorzec, który zwykle można uniknąć jest powolne ładowanie powiązanych danych dla każdego wiersza w tabeli podstawowej. O ile upewnieniu się, że pojedynczy kwerendy jest zbyt złożone, aby były skuteczne, zazwyczaj będzie mógł poprawić wydajność w takich przypadkach, zmieniając głównej zapytanie do użycia wczesny ładowania.


## <a name="pre-generating-views"></a>Wstępnego wygenerowania widoków

Gdy `ObjectContext` utworzenia obiektu w nowej domenie aplikacji, Entity Framework generuje zestaw klas używane do dostępu do bazy danych. Te klasy są nazywane *widoków*, jeśli model bardzo dużej ilości danych, generowanie tych widoków może opóźnić witryny sieci web odpowiedzi na pierwsze żądanie strony po zainicjowaniu nowej domeny aplikacji. Tworząc widoki w czasie kompilacji, a nie w czasie wykonywania, można zmniejszyć to opóźnienie pierwszego żądania.

> [!NOTE]
> Jeśli aplikacja nie ma modelu bardzo dużej ilości danych lub mieć modelu dużej ilości danych, ale nie ma danych dotyczących wydajności problemu, który dotyczy tylko pierwszego żądania strony po recyklingu usług IIS, możesz pominąć tę sekcję. Tworzenie nie jest realizowane za każdym razem, można utworzyć wystąpienia widoku `ObjectContext` obiektu, ponieważ widoki są buforowane w domenie aplikacji. W związku z tym chyba że jest często odtwarzanie aplikacji w usługach IIS, bardzo mało żądań strony będzie korzystać ze wstępnie wygenerowanych widoków.


Można wstępnie wygenerować widoków przy użyciu *EdmGen.exe* narzędzia wiersza polecenia lub przy użyciu *Toolkit transformacji szablonu tekstowego* szablonu (T4). W tym samouczku użyjesz szablonu T4.

W *DAL* folderu, Dodaj plików przy użyciu **szablonu tekstowego** szablonu (znajduje się w **ogólne** w węźle **zainstalowane szablony** listy), i nadaj mu nazwę *SchoolModel.Views.tt*. Zastąp istniejący kod w pliku następującym kodem:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Ten kod generuje widoków dla *edmx* pliku, który znajduje się w tym samym folderze co szablon i który ma taką samą nazwę jak plik szablonu. Na przykład, jeśli w nazwie pliku szablonu *SchoolModel.Views.tt*, będzie szukał pliku modelu danych o nazwie *SchoolModel.edmx*.

Zapisz plik, a następnie kliknij prawym przyciskiem myszy plik w **Eksploratora rozwiązań** i wybierz **Uruchom narzędzie niestandardowe**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio generuje kod plik, który tworzy widoki, które nosi nazwę *SchoolModel.Views.cs* na podstawie szablonu. (Zwróć uwagę, że plik kod jest generowany nawet przed wybraniem **Uruchom narzędzie niestandardowe**, jak zapisany plik szablonu.)

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Można teraz uruchomić aplikację i sprawdzić, czy działa tak jak poprzednio.

Aby uzyskać więcej informacji na temat wstępnie wygenerowanych widoków zobacz następujące zasoby:

- [Porady: wstępnego wygenerowania widoków, aby poprawić wydajność kwerend](https://msdn.microsoft.com/library/bb896240.aspx) w witrynie MSDN. Wyjaśniono, jak używać `EdmGen.exe` narzędzia wiersza polecenia do wstępnego wygenerowania widoków.
- [Izolowanie wydajności przy użyciu Prekompilowanego/wstępnie przygotowany-generated widoki programu Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) na blogu zespół doradczy klientów systemu Windows Server AppFabric.

Na tym kończy się wprowadzenie do poprawy wydajności w aplikacji sieci web ASP.NET, która korzysta z programu Entity Framework. Aby uzyskać więcej informacji, zobacz następujące zasoby:

- [Zagadnienia dotyczące wydajności (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) w witrynie MSDN.
- [Związane z wydajnością wpisach w blogu zespołu Framework jednostki](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [Scal EF opcje i skompilowane zapytania](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Wyjaśniający nieoczekiwane zachowania skompilowane zapytania i merge w blogu opcje, takie jak `NoTracking`. Jeśli planujesz użyć skompilowane zapytania lub modyfikowania ustawień opcji scalania w aplikacji, tym najpierw odczytu.
- [Zapisuje Entity Framework związane z na blogu dane i modelowanie zespół doradczy klientów](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Zawiera wpisy na skompilowane zapytania i przy użyciu programu Visual Studio 2010 profilującego do wykrywania problemów z wydajnością.
- [Entity Framework forum wątku z porady na temat zwiększania wydajności bardzo złożonych zapytań](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [Zalecenia dotyczące zarządzania stanu ASP.NET](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [Przy użyciu programu Entity Framework i ObjectDataSource: stronicowania niestandardowego](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Wpis w blogu bazującą na aplikacji ContosoUniversity utworzone w tych samouczkach wyjaśnić sposób zaimplementowania stronicowania w *Departments.aspx* strony.

Następny samouczek przegląda niektóre istotne ulepszenia do narzędzia Entity Framework, które stanowią nowość w wersji 4.

> [!div class="step-by-step"]
> [Poprzednie](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [dalej](what-s-new-in-the-entity-framework-4.md)
