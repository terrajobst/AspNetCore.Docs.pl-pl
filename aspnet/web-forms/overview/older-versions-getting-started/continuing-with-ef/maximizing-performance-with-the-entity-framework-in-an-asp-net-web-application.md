---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Maksymalizacja wydajności przy użyciu programu Entity Framework 4.0 w aplikacji ASP.NET 4 Web | Dokumentacja firmy Microsoft
author: tdykstra
description: W tej serii samouczków opiera się na aplikacji sieci web firmy Contoso University, utworzony przez wprowadzenie do serii samouczków Entity Framework 4.0. CZY MOGĘ...
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 2c5aa6c8a512ccd2ff4b94f5a8a60b31aaa39fc5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828283"
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Maksymalizacja wydajności przy użyciu programu Entity Framework 4.0 w aplikacji ASP.NET sieci Web 4
====================
przez [Tom Dykstra](https://github.com/tdykstra)

> W tej serii samouczków jest oparta na Contoso University aplikacji sieci web, który jest tworzony przez [rozpoczęcie korzystania z programu Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) serii samouczków. Jeśli nie została ukończona wcześniej samouczki, jako punkt początkowy na potrzeby tego samouczka możesz [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) będzie utworzony. Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tworzone przez zakończenie serii samouczków. Jeśli masz pytania dotyczące samouczków, możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


W poprzednim samouczku pokazano, jak obsługa konfliktów współbieżności. Ten samouczek przedstawia opcje poprawę wydajności aplikacji sieci web platformy ASP.NET, która używa programu Entity Framework. Poznasz kilka metod przeznaczonych do maksymalizacji wydajności, lub do diagnozowania problemów z wydajnością.

Informacje przedstawione w poniższych sekcjach prawdopodobnie może być przydatne do szerokiej gamy scenariuszy:

- Wydajne obciążenia powiązanych danych.
- Zarządzanie stanem widoku.

Informacje przedstawione w poniższych sekcjach mogą być przydatne w przypadku pojedynczych zapytań tego się problemy z wydajnością:

- Użyj `NoTracking` opcja scalania.
- Wstępnie skompilować zapytań LINQ.
- Sprawdź polecenia zapytania wysyłane do bazy danych.

Informacje przedstawione w poniższej sekcji jest potencjalnie przydatne w przypadku aplikacji, które mają bardzo dużych modeli danych:

- Wstępnie wygenerować widoków.

> [!NOTE]
> Wydajność aplikacji sieci Web wpływa wiele czynników, takich jak elementów, takich jak rozmiar danych żądań i odpowiedzi, szybkość zapytań do bazy danych, jak wiele żądań, że serwer można kolejkować i jak szybko może obsłużyć, a nawet efektywności dowolnego biblioteki skryptu klienta, który może być używany. Jeśli wydajność ma kluczowe znaczenie dla aplikacji lub testowania lub obsługi pokazuje, że wydajność aplikacji nie jest zadowalające, należy przestrzegać normalne protokołu dotyczące dostosowywania wydajności. Miar do określenia, gdzie występują wąskie gardła wydajności, a następnie adresować obszary, które mają największy wpływ na wydajność aplikacji ogólnej.
> 
> Ten temat koncentruje się głównie na sposób, w którym może potencjalnie podnieść wydajność specjalnie programu Entity Framework na platformie ASP.NET. Sugestie, w tym miejscu są przydatne, jeśli okaże się, że dostęp do danych jest jednym z wąskich gardeł wydajności w aplikacji. Z tym, jak wspomniano, metody opisane w tym miejscu nie należy uznać za &quot;najlepsze praktyki&quot; ogólnie rzecz biorąc — wiele z nich jest odpowiednie tylko w sytuacjach wyjątkowych lub adres bardzo konkretnych rodzajów wąskich gardeł wydajności.


Uruchom samouczek, uruchom program Visual Studio i otwarcie aplikacji sieci web firmy Contoso University pracowano w poprzednim samouczku.

## <a name="efficiently-loading-related-data"></a>Efektywne ładowanie powiązanych danych

Istnieje kilka sposobów, platformy Entity Framework można załadować powiązane dane do właściwości nawigacji jednostki:

- *Powolne ładowanie*. Podczas odczytywania jednostki powiązane dane nie są pobierane. Jednak użytkownik podejmie próbę dostępu do właściwości nawigacji po raz pierwszy wymagane dane dla tej właściwości nawigacji jest automatycznie pobierany. Skutkiem wielu zapytań wysyłanych do bazy danych — jedną dla sam podmiot, a co za każdym razem, powiązane dane dla jednostki musi zostać pobrany. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Wczesne ładowanie*. Podczas odczytywania jednostki powiązane dane są pobierane wraz z jej. Powoduje to zwykle w zapytaniu sprzężenia jednego, który pobiera wszystkie dane potrzebne. Należy określić wczesne ładowanie za pomocą `Include` metody, jak w tym samouczku już w tych samouczkach.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Jawne ładowanie*. Jest to podobne do ładowania z opóźnieniem, chyba że jawnie pobrać powiązanych danych w kodzie nie będzie automatycznie gdy uzyskujesz dostęp do właściwości nawigacji. Ładowanie powiązanych danych ręcznie za pomocą `Load` metoda właściwości nawigacji kolekcji lub możesz użyć `Load` metoda referencyjna właściwość dla właściwości, używane do przechowywania pojedynczego obiektu. (Na przykład wywołać `PersonReference.Load` metodę, aby załadować `Person` właściwość nawigacji `Department` jednostki.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Ponieważ nie są natychmiast pobrać wartości właściwości, powolne ładowanie i jawne ładowanie są również zarówno określane jako *odroczone ładowanie*.

Powolne ładowanie to domyślne zachowanie dla kontekstu obiektu, który został wygenerowany przez projektanta. Jeśli otworzysz *SchoolModel.Designer.cs* plik definiuje klasę obiektu kontekstu, znajdują się trzy konstruktory i każdy z nich zawiera następującą instrukcję:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Ogólnie rzecz biorąc Jeśli znasz należy powiązanych danych dla każdej jednostki, które pobrane, eager ładowanie zapewnia najlepszą wydajność, ponieważ pojedynczego zapytania wysłanego do bazy danych jest zazwyczaj bardziej efektywne niż oddzielne zapytania, dla każdej jednostki pobierane. Z drugiej strony, jeśli chcesz uzyskać dostęp tylko rzadko właściwości nawigacji jednostki lub tylko dla małych i zestawu jednostek, powolne ładowanie lub jawne ładowanie może być bardziej efektywne, ponieważ wczesne ładowanie pobiera większej ilości danych niż jest Ci potrzebne.

W aplikacji sieci web powolne ładowanie może być stosunkowo mały wartości mimo to, ponieważ akcje użytkownika, wpływających na potrzeby powiązanych danych została wykonana w przeglądarce nie połączona z kontekstu obiektów, które renderowania strony. Z drugiej strony, gdy można powiązać z danymi kontrolkę, zwykle wiadomo, jakie potrzebne dane i dlatego zazwyczaj jest najlepiej wybrać wczesne ładowanie lub odroczonego ładowania na podstawie co to jest właściwy dla każdego scenariusza.

Ponadto kontrolkę powiązaną z danymi mogą używać obiektu jednostki po usunięciu kontekstu obiektów. W tym przypadku próba z opóźnieniem obciążenia właściwości nawigacji może zakończyć się niepowodzeniem. Wyczyść jest otrzymany komunikat o błędzie: &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

`EntityDataSource` Kontroli domyślnie wyłącza powolne ładowanie. Dla `ObjectDataSource` kontroli używasz bieżący samouczek (lub możesz przejść do kontekstu obiektów z kodu strony), istnieje kilka sposobów, możesz wprowadzić z opóźnieniem ładowania domyślnie wyłączone. Można ją wyłączyć, podczas tworzenia wystąpienia kontekstu obiektów. Na przykład można dodać następujący wiersz do metody konstruktora `SchoolRepository` klasy:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Dla aplikacji Contoso University wprowadzisz kontekst automatycznie wyłączyć powolne ładowanie tak, aby ta właściwość nie muszą być ustawione w każdym przypadku, gdy zostanie uruchomiony w kontekście.

Otwórz *SchoolModel.edmx* modelu danych, kliknij na powierzchnię projektową, a następnie w okienku właściwości ustaw **powolne ładowanie włączone** właściwość `False`. Zapisz i zamknij modelu danych.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Wyświetl stan zarządzania

Aby zapewnić funkcji aktualizacji, na stronie sieci web platformy ASP.NET muszą być przechowywane oryginalnej wartości właściwości jednostki Po wyrenderowaniu strony. Podczas przetwarzania kontrolki zwrotu można ponownie utworzyć oryginalnej jednostki i wywołać jednostki `Attach` metody przed stosowanie zmian i wywoływać metodę `SaveChanges` metody. Domyślnie przez kontrolki danych wzorca ASP.NET Web Forms umożliwiają wyświetlanie stanu przechowywanie oryginalnych wartości. Jednak stan widoku może wpłynąć na wydajność, ponieważ jest ona przechowywana w ukrytych polach, które może znacznie zwiększyć rozmiar strony, które są przesyłane do i z przeglądarki.

Techniki zarządzania stanu widoku ani rozwiązań alternatywnych, takich jak stan sesji nie są unikatowe dla platformy Entity Framework, więc w tym samouczku nie jest akceptowana w tym temacie szczegółowo. Aby uzyskać więcej informacji zobacz linki na końcu tego samouczka.

Jednak w wersji 4 programu ASP.NET oferuje nowy sposób pracy z stan widoku, które każdy Deweloper platformy ASP.NET Web Forms aplikacji należy pamiętać o: `ViewStateMode` właściwości. Tej nowej właściwości można ustawić na poziomie strony lub kontroli i pozwala wyłączyć stan widoku domyślnie dla strony, a następnie ją włączyć tylko w przypadku kontrolek, które go potrzebują.

W przypadku aplikacji, gdzie wydajność ma kluczowe znaczenie dobrym rozwiązaniem jest zawsze wyłączyć stanu widoku na poziomie strony i ją włączyć tylko w przypadku kontrolek, które tego wymagają. Rozmiar stanu widoku na stronach University firmy Contoso w takich sytuacjach przydałaby znacznie zmniejszyć przez tę metodę, ale aby zobaczyć, jak to działa, należy to zrobić go *Instructors.aspx* strony. Ta strona zawiera wiele kontrolek, w tym `Label` formantu, który ma stan widoku wyłączone. Brak kontrolek na tej stronie rzeczywiście konieczne Wyświetl stan włączony. ( `DataKeyNames` Właściwość `GridView` kontroli określa stan, który utrzymuje się między ogłoszeniami wstecznymi, ale te wartości są przechowywane w stan formantu, który nie ma wpływu na `ViewStateMode` właściwości.)

`Page` Dyrektywy i `Label` kodu znaczników kontrolki obecnie podobnego do następującego:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Wprowadź następujące zmiany:

- Dodaj `ViewStateMode="Disabled"` do `Page` dyrektywy.
- Usuń `ViewStateMode="Disabled"` z `Label` kontroli.

Znaczniki są teraz podobne w poniższym przykładzie:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Wyświetl stan została wyłączona dla wszystkich kontrolek. Jeśli później dodasz formant, który trzeba użyć stanu widoku, wszystko, czego potrzebujesz, aby zrobić to obejmują `ViewStateMode="Enabled"` atrybutu dla tej kontrolki.

## <a name="using-the-notracking-merge-option"></a>Przy użyciu opcji scalania NoTracking

Kontekst pobiera wiersze z bazy danych i tworzy obiekty jednostki, które reprezentują je, domyślnie on również śledzi te obiekty jednostki przy użyciu swojego menedżera stanu obiektu. Te dane śledzenia działa jak pamięć podręczna i jest używany podczas aktualizowania jednostki. Ponieważ aplikacja sieci web ma zwykle wystąpienia kontekstu krótkotrwałych obiektów, zapytania często zwracają dane, które nie muszą być śledzone, ponieważ kontekst obiektu, który odczytuje je, zostaną usunięte przed dowolnej jednostki, które odczytuje są ponownie używane lub aktualizowane.

Platformy Entity Framework można określić, czy kontekst śledzi obiekty jednostki, ustawiając *merge — opcja*. Można ustawić opcji scalania dla pojedynczych zapytań lub zestawy jednostek. Jeśli ustawisz dla zestawu jednostek oznacza to, że ustawiasz domyślna opcja scalania dla wszystkich zapytań, które zostały utworzone dla tego zestawu jednostek.

Dla aplikacji Contoso University śledzenia nie jest wymagane dla żadnego z zestawów jednostek, do których dostęp z repozytorium, dzięki czemu można ustawić opcji scalania `NoTracking` dla tych zestawów jednostki podczas tworzenia wystąpienia kontekstu obiektów w klasie repozytorium. (Zwróć uwagę, że w tym samouczku ustawienie opcji scalania nie będziesz mieć znaczącego wpływu na wydajność aplikacji. `NoTracking` Opcja jest mogą spowodować poprawę wydajności zauważalne, tylko w niektórych scenariuszach dotyczących dużych ilości danych.)

W folderze DAL Otwórz *SchoolRepository.cs* pliku i Dodaj metody konstruktora, która ustawia opcję scalania dla jednostki zestawów, że repozytorium ma dostęp do:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Wstępne kompilowanie zapytań LINQ

Przy pierwszym Entity Framework wykonuje zapytanie SQL jednostki w ramach cyklu życia danego `ObjectContext` wystąpienia, dopiero po pewnym czasie można skompilować zapytania. Wynik kompilacji są buforowane, co oznacza, że kolejne wykonania zapytania są znacznie szybciej. Zapytania LINQ wykonaj podobny wzorzec, z tą różnicą, że niektóre ilości pracy wymaganej do kompilowania zapytanie jest wykonywane za każdym razem, gdy zapytanie jest wykonywane. Innymi słowy do kwerend LINQ, domyślnie nie wszystkie wyniki kompilacji są buforowane.

Jeśli zapytanie LINQ, która ma być cyklicznie uruchamiany z uprawnieniami obiektu życia, można napisać kod, który powoduje, że wszystkie wyniki kompilacji w pamięci podręcznej podczas pierwszego zapytania LINQ.

Ilustracją, możesz to zrobić na dwa `Get` metod w `SchoolRepository` klasy, z których jedna nie przyjmuje żadnych parametrów ( `GetInstructorNames` metoda) oraz jedną, która wymaga parametru ( `GetDepartmentsByAdministrator` metody). Te metody są w niezmienionej formie teraz faktycznie nie ma potrzeby kompilowana, ponieważ nie są one zapytań LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Jednak tak, aby wypróbować zapytania skompilowane, można będzie kontynuować tak, jakby było został zapisany jako następujące zapytania LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Można zmienić kod w tych metodach, co ma powyżej i uruchomić aplikację, aby sprawdzić, czy to działa, przed kontynuowaniem. Jednak zgodnie z poniższymi instrukcjami przejść bezpośrednio do procesu tworzenia wstępnie skompilowanym ich wersje.

Utwórz plik klasy w *DAL* folderu, nadaj jej nazwę *SchoolEntities.cs*i Zastąp istniejący kod następującym kodem:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Ten kod tworzy częściową klasą, która rozszerza klasę kontekstu automatycznie generowanych obiektów. Częściowa klasa zawiera dwa zapytania LINQ skompilowane przy użyciu `Compile` metody `CompiledQuery` klasy. Tworzy również metody używane do wywoływania zapytania. Zapisz i zamknij ten plik.

Następnie w *SchoolRepository.cs*, zmień istniejące `GetInstructorNames` i `GetDepartmentsByAdministrator` klasy metod w repozytorium, dzięki czemu mogą wywołać zapytania skompilowane:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Uruchom *Departments.aspx* strony, aby sprawdzić, czy działa tak jak poprzednio. `GetInstructorNames` Metoda jest wywoływana w celu wypełnienia listy rozwijanej administratora, a `GetDepartmentsByAdministrator` metoda jest wywoływana po kliknięciu **aktualizacji** w celu sprawdzenia, czy instruktora nie jest administratorem więcej niż jeden Dział.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

W aplikacji Contoso University tylko po to, aby dowiedzieć się, jak to zrobić, nie, ponieważ nie będzie widoczny poprawić wydajność, które zostały wstępnie skompilowanym zapytania. Prekompilowanie zapytań LINQ Dodaj poziom złożoności kodu, więc upewnij się, że można wykonać tylko w przypadku zapytań, które faktycznie stanowi wąskie gardła wydajności w aplikacji.

## <a name="examining-queries-sent-to-the-database"></a>Badanie zapytań wysłanych do bazy danych

Jeśli badasz problemy z wydajnością, czasami jest przydatne dokładnie poleceń SQL, które wysyła do bazy danych programu Entity Framework. Jeśli pracujesz z `IQueryable` obiekt o jeden ze sposobów jest użycie `ToTraceString` metody.

W *SchoolRepository.cs*, Zmień kod w `GetDepartmentsByName` metody zgodnie z poniższym przykładem:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

`departments` Musi być rzutowany zmienną `ObjectQuery` typu tylko w przypadku, ponieważ `Where` metoda na końcu poprzedni wiersz tworzy `IQueryable` obiektu; bez `Where` metody Rzutowanie nie jest konieczne.

Ustaw punkt przerwania na `return` wiersza, a następnie uruchom *Departments.aspx* strony w debugerze. Po osiągnięciu punktu przerwania zbadać `commandText` zmienną **lokalne** okno i użyj Wizualizator tekstu (ikonę lupy w **wartość** kolumny) do wyświetlania wartości w **Wizualizator tekstu** okna. Możesz zobaczyć całą polecenia SQL, która wynika z tego kodu:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Jako alternatywę funkcji IntelliTrace w programie Visual Studio Ultimate zapewnia sposób wyświetlania poleceń SQL, generowane przez program Entity Framework, która nie wymaga zmian w kodzie lub nawet Ustaw punkt przerwania.

> [!NOTE]
> Poniższych procedur można wykonywać tylko wtedy, gdy masz programu Visual Studio Ultimate.


Przywrócić oryginalny kod w `GetDepartmentsByName` metody, a następnie uruchom *Departments.aspx* strony w debugerze.

W programie Visual Studio, wybierz **debugowania** menu, a następnie **IntelliTrace**, a następnie **zdarzenia IntelliTrace**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

W **IntelliTrace** okna, kliknij przycisk **Przerwij wszystko**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

**IntelliTrace** okna wyświetla listę ostatnie zdarzenia:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Kliknij przycisk **ADO.NET** wiersza. Rozszerza on dowiesz się, tekst polecenia:

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Możesz skopiować polecenie cały ciąg tekstowy do Schowka z **lokalne** okna.

Załóżmy, że pracujesz z bazą danych przy użyciu więcej tabel, relacje i kolumn niż prostą `School` bazy danych. Może się okazać, że kwerendę, która zbiera wszystkie informacje wymagane w ramach pojedynczej `Select` instrukcji zawierającej wiele `Join` klauzule staje się zbyt złożona, aby pracować wydajnie. W takim przypadku możesz przełączyć eager ładowania na potrzeby jawnego ładowania, aby uprościć zapytanie.

Na przykład, spróbuj zmienić kod w `GetDepartmentsByName` method in Class metoda *SchoolRepository.cs*. Obecnie w tym metoda ma zapytań obiekt, który ma `Include` metody `Person` i `Courses` właściwości nawigacji. Zastąp `return` instrukcji z kodem, który wykonuje jawne ładowanie, jak pokazano w poniższym przykładzie:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Uruchom *Departments.aspx* strony w debugerze, a następnie sprawdź **IntelliTrace** oknie ponownie poprzednio. Teraz w przypadku, gdy było pojedyncze zapytanie przed, zobaczysz długo sekwencja.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Kliknij pierwszy **ADO.NET** wiersza, aby zobaczyć, co się stało złożonego zapytania można wyświetlić wcześniej.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

Zapytanie z działów stał się prostej `Select` zapytania bez `Join` klauzuli, ale następuje oddzielne zapytania, które pobierają Kursy pokrewne i administrator, przy użyciu zestawu dwóch zapytań dla każdego działu zwrócony przez oryginalny Zapytanie.

> [!NOTE]
> Pozostawienie powolne ładowanie włączone, wzorzec, widocznej w tym miejscu, za pomocą tego samego zapytania powtarzać wiele razy, mogą być wynikiem powolne ładowanie. Wzorzec, który chcesz uniknąć zwykle jest powolne ładowanie powiązanych danych dla każdego wiersza w tabeli podstawowej. Chyba, że gdy masz pewność, że pojedynczy kwerendy jest zbyt złożona, aby były skuteczne, zazwyczaj będzie można poprawić wydajność w takich sytuacjach, zmieniając podstawowego zapytania, aby użyć wczesne ładowanie.


## <a name="pre-generating-views"></a>Wstępnie generowanie widoków

Gdy `ObjectContext` najpierw tworzony jest obiekt w nowej domeny aplikacji, platformy Entity Framework generuje zestaw klas używanych do dostępu do bazy danych. Te klasy są nazywane *widoków*, i w przypadku modelu bardzo dużej ilości danych generowanie tych widoków można opóźnić witryny sieci web odpowiedzi na pierwsze żądanie strony po zainicjowaniu nowej domeny aplikacji. Tworząc widoki, w czasie kompilacji, a nie w czasie wykonywania, można zmniejszyć to opóźnienie pierwszego żądania.

> [!NOTE]
> Jeśli aplikacja nie ma modelu bardzo dużych ilości danych lub jeśli niesie ze sobą modelu dużych ilości danych, ale nie ma zajmującym się problem z wydajnością, który wpływa na tylko pierwsze żądanie strony po recyklingu usług IIS, można pominąć tę sekcję. Wyświetl tworzenia nie jest realizowane za każdym razem, gdy tworzenia wystąpienia `ObjectContext` obiektu, ponieważ opinie są buforowane w domenie aplikacji. W związku z tym chyba że są często odtwarzanie aplikacji w usługach IIS, bardzo mało żądań strony używającym wstępnie wygenerowanych widoków.


Można wstępnie wygenerować widoków przy użyciu *EdmGen.exe* narzędzia wiersza polecenia lub przy użyciu *Toolkit przekształcania szablonu tekstu* szablonu (T4). W tym samouczku użyjesz szablon T4.

W *DAL* folderu, Dodaj plik przy użyciu **szablon tekstowy** szablonu (znajduje się pod **ogólne** w węźle **zainstalowane szablony** listy), i nadaj mu nazwę *SchoolModel.Views.tt*. Zastąp istniejący kod w pliku następującym kodem:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Ten kod generuje widoków *edmx* pliku, który znajduje się w tym samym folderze co szablon i który ma taką samą nazwę jak plik szablonu. Na przykład, jeśli w nazwie pliku szablonu *SchoolModel.Views.tt*, będzie szukać pliku modelu danych o nazwie *SchoolModel.edmx*.

Zapisz plik, a następnie kliknij prawym przyciskiem myszy plik w **Eksploratora rozwiązań** i wybierz **Uruchom narzędzie niestandardowe**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Program Visual Studio generuje kod pliku, który tworzy widoków, które nosi *SchoolModel.Views.cs* na podstawie szablonu. (Zwróć uwagę, że generowany jest plik kodu, nawet w przypadku, przed wybraniem **Uruchom narzędzie niestandardowe**, zaraz po zapisaniu pliku szablonu.)

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Możesz teraz uruchomić aplikację i sprawdzić, czy działa tak jak poprzednio.

Aby uzyskać więcej informacji na temat wstępnie wygenerowanych widoków zobacz następujące zasoby:

- [Porady: generowanie wstępnie widoków, aby poprawić wydajność zapytań](https://msdn.microsoft.com/library/bb896240.aspx) w witrynie MSDN. Opis sposobu użycia `EdmGen.exe` narzędzie wiersza polecenia można wstępnie wygenerować widoków.
- [Izolowanie wydajności za pomocą prekompilowany/wstępnej-generated widoków w programie Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) na blogu zespołu doradczego klientów systemu Windows Server AppFabric.

Na tym kończy się wprowadzenie do zwiększania wydajności w aplikacji sieci web ASP.NET, który używa programu Entity Framework. Aby uzyskać więcej informacji, zobacz następujące zasoby:

- [Zagadnienia dotyczące wydajności (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) w witrynie MSDN.
- [Związane z wydajnością wpisy na blogu zespołu Framework jednostki](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [Scal EF, opcji i zapytania skompilowane](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Wpis w blogu, który objaśnia, nieoczekiwane wyniki zapytania skompilowane i scalanie opcje, takie jak `NoTracking`. Jeśli planujesz użyć zapytania skompilowane i modyfikować ustawienia opcji scalania w aplikacji, zapoznaj się z pierwszej.
- [Entity Framework, powiązane wpisy w blogu dane i modelowanie zespół doradczy klientów](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Zawiera wpisy na zapytania skompilowane i przy użyciu Profiler 2010 usługi Visual Studio, aby wykryć problemy z wydajnością.
- [Entity Framework forum wątku z porady dotyczące poprawy wydajności zapytań bardzo złożone](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [Zalecenia dotyczące zarządzania stanu ASP.NET](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [Za pomocą programu Entity Framework i kontrolki ObjectDataSource: niestandardowe stronicowania](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Wpis w blogu, który tworzy aplikacji ContosoUniversity utworzone w tych samouczkach, aby wyjaśnić sposób zaimplementowania stronicowania w *Departments.aspx* strony.

Następnego samouczka, przegląd niektórych ważnych ulepszeń w programie Entity Framework, które stanowią nowość w wersji 4.

> [!div class="step-by-step"]
> [Poprzednie](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [dalej](what-s-new-in-the-entity-framework-4.md)
