---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: "Nowości w sieci Web formularzy w programie ASP.NET 4.5 | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Nowa wersja składnika ASP.NET Web Forms wprowadzono wiele ulepszeń skoncentrowane na ułatwieniu środowisko użytkownika podczas pracy z danymi. W poprzednich wersjach..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 560f949f79be8ba4936e4a6f8ee8ee32ef15acbf
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>Nowości w formularzach sieci Web w programie ASP.NET 4.5
====================
przez [obozów sieci Web Team](https://twitter.com/webcamps)

> Nowa wersja składnika ASP.NET Web Forms wprowadzono wiele ulepszeń skoncentrowane na ułatwieniu środowisko użytkownika podczas pracy z danymi.
> 
> W poprzednich wersjach formularzy sieci Web, gdy używanie powiązania danych, aby emitować wartość obiektu elementu członkowskiego użyto wyrażenia wiązania danych Bind() lub Eval(). W nowej wersji platformy ASP.NET jest możliwość zadeklarować, jakiego typu dane formantu ma być powiązane z przy użyciu nową właściwość ItemType. Ustawienie tej właściwości spowoduje włączenie użyć zmiennej jednoznacznie otrzymywać pełne korzyści środowisko programistyczne Visual Studio, takie jak IntelliSense, nawigacji elementu członkowskiego i sprawdzanie kompilacji.
> 
> Z formantów powiązanych z danymi można teraz również określić własne niestandardowe metody zaznaczenie, aktualizowanie, usuwanie i wstawianie danych, upraszczając interakcji między formantami strony i logiki aplikacji. Ponadto możliwości wiązania modelu zostały dodane do programu ASP.NET, co oznacza, że możesz mapować dane ze strony bezpośrednio do parametrów typu metody.
> 
> Walidacja danych wejściowych użytkownika również musi być łatwiejsze do najnowszej wersji formularzy sieci Web. Można teraz dodawać adnotacje klasach modeli z atrybutów sprawdzania poprawności z **System.ComponentModel.DataAnnotations** przestrzeń nazw i żądania, która kontroluje witryny sieci sprawdzania poprawności danych wprowadzonych za pomocą tych informacji przez użytkownika. Sprawdzanie poprawności po stronie klienta w formularzach sieci Web jest teraz zintegrowana z jQuery, zapewniając czyszcząca kod po stronie klienta i funkcjami dyskretnego kodu JavaScript.
> 
> W obszarze weryfikacji żądań wprowadzono ulepszenia ułatwiają selektywnie wyłączyć weryfikację żądań dla określonych części aplikacji lub odczytać danych żądania nieważne.
> 
> Niektóre ulepszenia do formularzy sieci Web kontrolki serwera mógł korzystać z nowych funkcji HTML5:
> 
> - Tryb tekstowy właściwości formantu TextBox została zaktualizowana do obsługi nowych typów wejściowych HTML5, takie jak wiadomości e-mail, daty i godziny i tak dalej.
> - Sterowanie przekazywaniem plików teraz obsługuje wiele przekazywania plików z przeglądarek, które obsługują tę funkcję HTML5.
> - Moduł sprawdzania poprawności steruje teraz obsługi sprawdzania poprawności HTML5 elementów wejściowych.
> - Nowych elementów HTML5, które mają atrybuty, które reprezentują adresu URL teraz obsługuje runat =&quot;serwera&quot;. W związku z tym można użyć programu ASP.NET Konwencji w ścieżki adresu URL, tak samo, jak ~ operatora, aby reprezentować katalog główny aplikacji (na przykład &lt;wideo runat =&quot;serwera&quot; src =&quot;~/myVideo.wmv&quot; &gt; &lt;/wideo&gt;).
> - Formantu UpdatePanel został rozwiązany do obsługi publikowanie HTML5 pól wejściowych.
> 
> W portalu oficjalnego ASP.NET można znaleźć więcej przykładów dotyczących nowych funkcji w programie ASP.NET 4.5 formularzy sieci Web: [What's New in ASP.NET 4.5 i programu Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym laboratorium praktycznego przedstawiono sposób:

- Użyj jednoznacznie wyrażenia wiązania danych
- Korzystać z nowych funkcji powiązania modelu w formularzach sieci Web
- Użyj dostawców wartości do mapowania danych strony metody związane z kodem
- Za pomocą adnotacji danych do sprawdzania poprawności danych wejściowych użytkownika
- Wykonaj advange unobstrusive weryfikacji po stronie klienta z jQuery w formularzach sieci Web
- Implementuje weryfikację żądań szczegółowego
- Implementuje stronę asynchronicznego przetwarzania w formularzach sieci Web

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Musi mieć następujące elementy do przygotowania tego laboratorium:

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

**Instalowanie wstawki kodu**

Dla wygody taki kod, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako wstawki kodu programu Visual Studio. Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.

Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek C:](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To laboratorium praktycznego obejmuje następujących czynnościach:

1. [Ćwiczenie 1: Model powiązania w formularzach sieci Web ASP.NET](#Exercise1)
2. [Ćwiczenie 2: Sprawdzanie poprawności danych](#Exercise2)
3. [Ćwiczenie 3: Strona asynchronicznego przetwarzania w sieci Web ASP.NET formularzy](#Exercise3)

> [!NOTE]
> Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach. Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.


Szacowany czas trwania tego laboratorium: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Ćwiczenie 1: Model powiązania w formularzach sieci Web ASP.NET

Nowa wersja składnika ASP.NET Web Forms wprowadzono wiele ulepszeń skoncentrowane na ułatwieniu środowisko użytkownika podczas pracy z danymi. W ramach tego ćwiczenia zostanie Dowiedz się więcej o jednoznacznie formantów danych i powiązania modelu.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Zadanie 1 - przy użyciu jednoznacznie powiązania danych

W tym zadaniu zostanie wykryty nowego powiązania jednoznacznie dostępne w programie ASP.NET 4.5.

1. Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex1-metodyModelBinding/Begin/** folderu.

    1. Należy pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
    2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
    3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

    > [!NOTE]
    > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz **Customers.aspx** strony. Umieść nienumerowanych listy w formancie głównego i obejmują kontrolce elementu powtarzanego wewnątrz do wyświetlania każdego klienta. Ustaw nazwę elementu powtarzanego **customersRepeater** jak pokazano w poniższym kodzie.

    W poprzednich wersjach formularzy sieci Web gdy używanie powiązania danych do wysyłania wartości elementu członkowskiego dla obiekt jest powiązanie danych w celu, należy użyć wyrażenia wiązania danych wraz z wywołanie do metody Eval, przekazywanie nazwy elementu członkowskiego jako ciąg.

    W czasie wykonywania tych wywołań Eval będzie odczytywanie wartość elementu członkowskiego o podanej nazwie przy użyciu odbicia względem obecnie powiązany obiekt i wyświetlić wyniki w formacie HTML. Takie podejście ułatwia bardzo do tworzenia powiązań danych z dowolnego, unshaped danych.

    Niestety utracisz wiele funkcji wspaniały czasie opracowywania w programie Visual Studio, w tym IntelliSense dla nazwy elementów członkowskich, Obsługa nawigacji (np. Przejdź do definicji) i sprawdzanie kompilacji.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Otwórz **Customers.aspx.cs** pliku.
4. Dodaj następującą instrukcję using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. W **strony\_obciążenia** metody, Dodaj kod, aby wypełnić elementu powtarzanego z listy odbiorców.

    (Fragment - kodu *laboratorium - Ex01 - formularze powiązanego źródła danych klientów w sieci Web*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    W tym rozwiązaniu zastosowano EntityFramework wraz z CodeFirst do tworzenia i dostęp do bazy danych. W poniższym kodzie customersRepeater jest powiązany z zmaterializowanego kwerendę, która zwraca wszystkich klientów z bazy danych.
6. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie i przejdź do **klientów** stronę, aby zobaczyć powtarzanego w akcji. Jako dane rozwiązanie korzysta z CodeFirst, bazy danych zostanie utworzona i wypełnione w lokalnym wystąpieniu programu SQL Express, podczas uruchamiania aplikacji.

    ![Wyświetlanie listy klientów z elementu powtarzanego](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "listę klientów z elementu powtarzanego")

    *Wyświetlanie listy klientów z elementu powtarzanego*

    > [!NOTE]
    > W programie Visual Studio 2012 usługi IIS Express to serwera wdrożeniowego sieci Web domyślne.
7. Zamknij przeglądarkę i przejdź wstecz do programu Visual Studio.
8. Teraz Zastąp implementację, aby użyć jednoznacznie powiązania. Otwórz **Customers.aspx** strony i użyj nowych **ItemType** atrybutu w elemencie powtarzanym można ustawić **klienta** typu powiązania.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    Właściwość ItemType można zadeklarować typu danych formantu jest, aby powiązać i pozwala na stosowanie jednoznacznie powiązania wewnątrz formantu powiązanego z danymi.
9. Zastąp zawartość następującym kodem ItemTemplate.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Wadą jednego interfejsu z metod powyżej to wywołań Eval() i Bind() późnym wiązaniem — oznacza, że w przypadku przekazania parametrów do reprezentowania nazwy właściwości. Oznacza to, że nie pobieraj Intellisense dla nazwy elementów członkowskich, Obsługa kodu nawigacji (np. Przejdź do definicji) ani obsługę sprawdzanie, czy w czasie kompilacji.

    Ustawienie właściwości ItemType powoduje, że dwa nowe zmienne typu ma być generowany w zakresie wyrażenia wiązania danych: **elementu** i **metody BindItem**. Możesz użyć tych zmiennych jednoznacznie w wyrażeniach wiązania danych i uzyskać pełne korzyści środowisko programistyczne Visual Studio.

    &quot; **:** &quot; Wyrażenia będzie automatycznie kodowanie HTML danych wyjściowych, aby uniknąć problemów z zabezpieczeniami (na przykład krzyżowych skryptów ataków). Ten element notation była dostępna od wersji .NET 4 dla zapisu odpowiedzi, ale teraz jest również dostępna w wyrażeniach wiązania z danymi.

    > [!NOTE]
    > Elementu członkowskiego działania dla powiązania jednokierunkowe. Jeśli chcesz wykonać powiązanie dwukierunkowe użyj **metody BindItem** elementu członkowskiego.

    ![Obsługę funkcji IntelliSense w powiązaniu jednoznacznie](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "obsługę funkcji IntelliSense w powiązaniu z silną kontrolą typów")

    *Obsługę funkcji IntelliSense w powiązaniu z silną kontrolą typów*
10. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie i przejdź do strony klientów, aby się upewnić, że zmiany działać zgodnie z oczekiwaniami.

    ![Wyświetlanie szczegółów odbiorcy](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "wyświetlania Szczegóły klienta")

    *Wyświetlanie szczegółów klienta*
11. Zamknij przeglądarkę i przejdź wstecz do programu Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Zadanie 2 — wprowadzenie modelu powiązania w formularzach sieci Web

W poprzednich wersjach programu ASP.NET Web Forms Jeśli chcesz wykonać dwukierunkowego wiązania danych, zarówno pobieranie i aktualizowanie danych, trzeba było użyć obiektu źródła danych. Może to być źródłem danych obiektów, źródło danych SQL, źródła danych LINQ i tak dalej. Jednak w razie potrzeby danego scenariusza niestandardowego kodu do obsługi danych potrzebne do użycia obiektu źródła danych i to wprowadzone niektóre wady. Na przykład wymagane w celu uniknięcia typy złożone i potrzebne do obsługi wyjątków podczas wykonywania logiki sprawdzania poprawności.

W nowej wersji składnika ASP.NET Web Forms formanty powiązane z danymi obsługuje wiązania modelu. Oznacza to, że można określić wybierz, zaktualizować, wstawiania lub usuwania metody bezpośrednio w formancie powiązane z danymi do wywołania logiki z pliku CodeBehind lub z innej klasy.

Aby dowiedzieć się więcej na ten temat, zostanie użyty element GridView do listy Kategorie produktów, przy użyciu nowego **SelectMethod** atrybutu. Ten atrybut można określić metodę pobierania danych widoku GridView.

1. Otwórz **Products.aspx** strony i obejmują **GridView**. Konfigurowanie widoku GridView, jak pokazano poniżej jednoznacznie powiązania oraz włączyć sortowanie i stronicowanie.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Użyj nowych **SelectMethod** atrybutu, aby skonfigurować widoku GridView do wywołania **GetCategories** metody, aby wybrać dane.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Otwórz **Products.aspx.cs** CodeBehind i dodaj następujące instrukcje using.

    (Fragment - kodu *sieci Web przestrzenie nazw laboratorium - Ex01 - formularze*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Dodaj prywatnego elementu członkowskiego w **produktów** klasy i przypisz nowe wystąpienie klasy **ProductsContext**. Ta właściwość będzie przechowywać kontekstu danych programu Entity Framework, które umożliwia połączenie z bazą danych.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Utwórz **GetCategories** metody do pobierania listy kategorii za pomocą LINQ. Kwerenda będzie zawierała **produktów** właściwości, więc ilość produktów dla każdej kategorii mogą być prezentowane w widoku GridView. Zwróć uwagę, że metoda zwraca obiekt IQueryable raw, który reprezentuje zapytanie, aby można wykonać na dalszym etapie cyklu życia strony.

    (Fragment - kodu *sieci Web formularzy laboratorium - Ex01 - GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > W poprzednich wersjach programu ASP.NET Web Forms Włączanie sortowania i stronicowania, za pomocą własnych logiki repozytorium w kontekście obiektu źródła danych, wymagane do zapisu niestandardowy kod i odbierać wszystkie niezbędne parametry. Teraz, jako metody wiązania danych może zwracać interfejs IQueryable, a ta pozycja reprezentuje zapytanie nadal ma być wykonywana, ASP.NET zająć się modyfikowanie zapytanie, aby dodać prawidłowe sortowanie i stronicowanie parametrów.
6. Naciśnij klawisz **F5** Rozpocznij debugowanie lokacji i przejdź do strony produktów. Powinna zostać wyświetlona widoku GridView jest wypełniana kategorii zwracany przez metodę GetCategories.

    ![Wypełnianie Element GridView przy użyciu wiązania modelu](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "wypełnianie Element GridView przy użyciu wiązania modelu")

    *Wypełnianie Element GridView przy użyciu wiązania modelu*
7. Naciśnij klawisz **SHIFT**+**F5** zatrzymać debugowanie.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Zadanie 3 - dostawców wartości w powiązaniu modelu

Powiązanie modelu nie tylko umożliwia określenie niestandardowych metod do pracy z danymi bezpośrednio w formancie powiązane z danymi, ale również umożliwia mapowanie danych ze strony do parametrów z tych metod. Dla parametru metody służy atrybuty dostawcy wartości do określenia tej wartości źródła danych. Na przykład:

- Formanty na stronie
- Wartości ciągu zapytania
- Wyświetlanie danych
- Stan sesji
- Pliki cookie
- Przesłanego formularza danych
- Stan widoku
- Dostawców wartości niestandardowe są obsługiwane również

Użycie programu ASP.NET MVC 4 Zauważ, że przypomina obsługi powiązania modelu. W rzeczywistości te funkcje zostały pobrane z platformy ASP.NET MVC i przenoszony do **System.Web** zestawu, aby można było używać ich również w formularzach sieci Web.

To zadanie spowoduje zaktualizowanie widoku GridView, aby filtrować wyniki według ilości produktów dla każdej kategorii na odbieranie parametr filtru z wiązania modelu.

1. Wróć do **Products.aspx** strony.
2. U góry widoku GridView, Dodaj **etykiety** i **ComboBox** wybierz liczbę produktów dla każdej kategorii, jak pokazano poniżej.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Dodaj **elementu EmptyDataTemplate** do widoku GridView, aby wyświetlić komunikat, jeśli istnieją żadnych kategorii z wybraną liczbę produktów.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Otwórz **Products.aspx.cs** CodeBehind i dodaj następującą instrukcję using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Modyfikowanie **GetCategories** metodę, aby odbierać całkowitą **minProductsCount** argumentu i filtrować wyniki zwracane. Aby to zrobić, Zastąp metodę z następującym kodem.

    (Fragment - kodu *sieci Web Forms GetCategories laboratorium - Ex01 - 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    Nowy **[kontrola]** atrybutu **minProductsCount** argument umożliwia platformie ASP.NET poznać jego wartość musi być wypełniona za pomocą formantu na stronie. ASP.NET będzie wyglądać dla każdego formantu pasującego do nazwy argumentu (minProductsCount) i wykonywać niezbędne mapowania i konwersji, aby wypełnić parametr wartości formantu.

    Alternatywnie atrybutu zawiera przeciążonego konstruktora, który umożliwia określenie kontroli, skąd pobrać wartości.

    > [!NOTE]
    > Jeden celem funkcji wiązania danych jest zmniejszenie ilości kodu, który musi być napisana dla strony interakcji. Oprócz [kontrola] dostawcy wartości można użyć innych dostawców wiązania modelu w parametry metody. Niektóre z nich są wymienione w wprowadzenie zadań.
6. Naciśnij klawisz **F5** Rozpocznij debugowanie lokacji i przejdź do strony produktów. Wybierz z listy rozwijanej wiele produktów i zwróć uwagę, jak widoku GridView jest już uaktualniona.

    ![Filtrowanie widoku GridView o wartości listy rozwijanej](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "filtrowanie widoku GridView o wartości listy rozwijanej")

    *Filtrowanie widoku GridView o wartości listy rozwijanej*
7. Zatrzymaj debugowanie.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Zadanie 4 — przy użyciu modelu powiązania w celu filtrowania

W ramach tego zadania spowoduje dodanie sekundy, podrzędny widoku GridView pokazanie produktów w ramach wybranej kategorii.

1. Otwórz **Products.aspx** strony i widoku GridView do automatycznego generowania przycisku Wybierz kategorie aktualizacji.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Dodaj drugi **GridView** o nazwie **productsGrid** u dołu. Ustaw **ItemType** do **WebFormsLab.Model.Product**, **DataKeyNames** do **ProductId** i **SelectMethod**  do **GetProducts**. Ustaw **właściwość AutoGenerateColumns miała** do **false** i Dodaj kolumny ProductId, ProductName, opis i UnitPrice.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Otwórz **Products.aspx.cs** pliku CodeBehind. Implementowanie **GetProducts** metody w celu odbierania identyfikator kategorii z kategorią widoku GridView i filtrować produktów. Wiązanie modelu spowoduje ustawienie wartości parametru za pomocą wybranego wiersza w **categoriesGrid**. Ponieważ nazwy argumentu i nazwa formantu nie są zgodne, należy określić nazwę formantu w dostawcy wartości formantu.

    (Fragment - kodu *sieci Web Forms laboratorium - Ex01 - GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Takie podejście ułatwia jednostki tych metod testowych. W kontekście testu jednostki, której formularzy sieci Web nie jest wykonywany, atrybut [kontrola] nie będzie wykonywać żadnych określonej akcji.
4. Otwórz **Products.aspx** strony i Znajdź produktów widoku GridView. Aktualizuj produkty GridView, aby wyświetlić łącza do edycji zaznaczonego produktu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Otwórz **ProductDetails.aspx** strony CodeBehind i Zastąp **SelectProduct** metodę z następującym kodem.

    (Fragment - kodu *sieci Web formularzy laboratorium - Ex01 - SelectProduct metody*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Zwróć uwagę, że **[QueryString]** atrybut jest używany do wypełnienia parametru metody z parametrem productId w ciągu zapytania.
6. Naciśnij klawisz **F5** Rozpocznij debugowanie lokacji i przejdź do strony produktów. Wybierz odpowiednią kategorię z kategorii w widoku GridView i zwróć uwagę, że produkty widoku GridView jest aktualizowany.

    ![Wyświetlanie produktów z wybranej kategorii](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "przedstawiający produkty z wybranej kategorii")

    *Wyświetlanie produktów z wybranej kategorii*
7. Kliknij przycisk **widoku** łącze produktu, aby otworzyć stronę ProductDetails.aspx.

    Zwróć uwagę, że strona jest pobierania produktu z SelectMethod za pomocą parametru productId z ciągu zapytania.

    ![Wyświetlanie szczegółów produktu](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "wyświetlanie szczegółów produktu")

    *Wyświetlanie szczegółów produktu*

    > [!NOTE]
    > Możliwość wpisz opis HTML będą realizowane w następnym ćwiczeniu.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Zadanie 5 - przy użyciu modelu powiązania dla operacji aktualizacji

W poprzednim zadaniu użyto wiązania modelu głównie do wybierania danych, w tym zadaniu zostanie Dowiedz się jak używać w operacjach aktualizowania wiązania modelu.

Kategorie GridView, aby umożliwić użytkownikowi aktualizacja kategorii zostanie zaktualizowana.

1. Otwórz **Products.aspx** strony i zaktualizuj kategorie GridView do automatycznego generowania przycisk Edytuj i korzystać z nowych **UpdateMethod** atrybutu, aby określić **UpdateCategory**metodę, aby zaktualizować wybranego elementu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    Atrybut DataKeyNames w widoku GridView zdefiniować, które są elementami, które identyfikują w obiekcie powiązanym ze modelu i w związku z tym, które są parametry metody aktualizacji co najmniej powinien zostać wyświetlony.
2. Otwórz **Products.aspx.cs** plik CodeBehind i wdrożenie **UpdateCategory** metody. Metoda powinien zostać wyświetlony identyfikator kategorii do ładowania bieżącej kategorii, wypełnić wartości z widoku GridView i następnie zaktualizować kategorii.

    (Fragment - kodu *sieci Web formularzy laboratorium - Ex01 - UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    Nowy **TryUpdateModel** metody w klasie strony odpowiada zapełnianie obiekt modelu, używając wartości z formantów strony. W takim przypadku zostanie zastąpiony przez zaktualizowane wartości z bieżącego wiersza GridView edytowany w **kategorii** obiektu.

    > [!NOTE]
    > Następnym ćwiczeniu objaśnia użycie ModelState.IsValid sprawdzania poprawności danych wprowadzonych przez użytkownika podczas edycji obiektu.
3. Uruchom witrynę i przejdź do strony produktów. Edytuj kategorię. Wpisz nową nazwę, a następnie kliknij przycisk **aktualizacji** aby utrwalić zmiany.

    ![Edytowanie kategorii](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "edycji kategorii")

    *Edytowanie kategorii*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Ćwiczenie 2: Sprawdzanie poprawności danych

W tym ćwiczeniu dowiesz się o nowych funkcjach sprawdzania poprawności danych w programie ASP.NET 4.5. Nowe funkcje sprawdzania poprawności dyskretnego kodu w formularzach sieci Web będzie wyewidencjonować. Do sprawdzania poprawności danych wejściowych użytkownika użyje adnotacji danych w klasach modelu aplikacji i na koniec dowiesz się jak włączyć lub wyłączyć weryfikację żądań do poszczególnych kontrolek na stronie.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Zadanie 1 - sprawdzania poprawności dyskretnego kodu

Formularze z złożonych dane w tym moduły weryfikacji zwykle prowadzi do generowania zbyt dużej ilości kodu JavaScript na stronie może reprezentować około 60% kodu. Z włączoną sprawdzania poprawności dyskretnego kodu kod HTML będzie wyglądać czyszczący i pisma odręcznego.

W tej sekcji zostaną włączone poprawności dyskretnego kodu w programie ASP.NET do porównania kod HTML wygenerowany przez obie konfiguracje.

1. Otwórz **programu Visual Studio 2012** , a następnie otwórz **rozpocząć** rozwiązania, znajdujących się w **Source\Ex2 Validation\Begin** folder tego laboratorium. Alternatywnie można kontynuować pracę na istniejące rozwiązania z poprzednim ćwiczeniu.

    1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. W tym celu w Eksploratorze rozwiązań kliknij **WebFormsLab** projektu **Zarządzaj pakietami NuGet**.
    2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
    3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

    > [!NOTE]
    > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Naciśnij klawisz **F5** można uruchomić aplikacji sieci web. Przejdź do klientów i kliknij przycisk **dodać nowego klienta** łącza.
3. Kliknij prawym przyciskiem myszy na stronie przeglądarki, a następnie wybierz **Wyświetl źródło** opcję, aby otworzyć kod HTML wygenerowany przez aplikację.

    ![Wyświetlana strona kod HTML](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "wyświetlana strona kod HTML")

    *Wyświetlanie kodu HTML strony*
4. Przewiń stronę kodu źródłowego i zwróć uwagę, że program ASP.NET ma wprowadzonym JavaScript kod i dane modułów sprawdzania poprawności na stronie Aby wykonywać operacje sprawdzania poprawności i wyświetlić listę błędów.

    ![Sprawdzanie poprawności kodu JavaScript na stronie CustomerDetails](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "kodu JavaScript sprawdzania poprawności na stronie CustomerDetails")

    *Sprawdzanie poprawności kodu JavaScript na stronie CustomerDetails*
5. Zamknij przeglądarkę i przejdź wstecz do programu Visual Studio.
6. Teraz spowoduje włączenie sprawdzania poprawności dyskretnego kodu. Otwórz **Web.Config** i Znajdź **ValidationSettings:UnobtrusiveValidationMode** klucza w **AppSettings** sekcji **.** Ustaw wartość klucza **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > Można też ustawić tę właściwość w &quot; **strony\_obciążenia** &quot; zdarzeń w przypadku chcesz włączyć tylko w przypadku niektórych stron sprawdzania poprawności dyskretnego kodu.
7. Otwórz **CustomerDetails.aspx** i naciśnij klawisz **F5** można uruchomić aplikacji sieci Web.
8. Naciśnij klawisz F12, aby otworzyć narzędzi deweloperskich programu Internet Explorer. Po otwarciu narzędzia deweloperskie, wybierz kartę skryptu. Wybierz **CustomerDetails.aspx** z menu i podejmij załadowano należy pamiętać, że skrypty wymagane do uruchomienia jQuery na stronie w przeglądarce z lokacji lokalnej.

    ![Ładowanie jQuery JavaScript pliki bezpośrednio z lokalnego serwera IIS](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "ładowania jQuery JavaScript pliki bezpośrednio z lokalnego serwera IIS")

    *Ładowanie plików JavaScript jQuery bezpośrednio z lokalnego serwera IIS*
9. Zamknij przeglądarkę, aby powrócić do programu Visual Studio. Otwórz **Site.Master** ponownie i Znajdź **ScriptManager**. Dodaj atrybut **EnableCdn** właściwość z wartością **True**. Spowoduje to wymuszenie jQuery do załadowania, adres URL online, ale nie z lokacji lokalnej adresu URL.
10. Otwórz **CustomerDetails.aspx** w programie Visual Studio. Naciśnij klawisz F5, aby uruchomić witryny. Po otwarciu programu Internet Explorer, naciśnij klawisz F12, aby otworzyć narzędzia deweloperskie. Wybierz **skryptu** karcie, a następnie spójrz na liście rozwijanej. Należy pamiętać, że pliki JavaScript jQuery już ładowanych z lokacją lokalną, ale raczej z online jQuery CDN.

    ![Ładowanie jQuery JavaScript pliki z sieci CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "ładowania jQuery JavaScript pliki z sieci CDN")

    *Ładowanie plików JavaScript jQuery z sieci CDN*
11. Otwórz ponownie, używając opcji widoku źródła w przeglądarce kod źródłowy HTML strony. Powiadomienie, że przez włączenie sprawdzania poprawności dyskretnego kodu ASP.NET został zastąpiony wprowadzony kod JavaScript danych - \*atrybutów.

    ![Kod sprawdzania poprawności dyskretnego kodu](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "kodu sprawdzania poprawności dyskretnego kodu")

    *Kod sprawdzania poprawności dyskretnego kodu*

    > [!NOTE]
    > W tym przykładzie widać, jak przy użyciu adnotacji danych podsumowania weryfikacji została uproszczona w celu tylko kilka HTML i JavaScript wierszy. Wcześniej bez sprawdzania poprawności dyskretnego kodu, więcej formanty walidacji, które można dodać, tym większy kod JavaScript weryfikacji będzie rosnąć.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Zadanie 2 — Sprawdzanie poprawności modelu przy użyciu adnotacji danych

Program ASP.NET 4.5 wprowadza Walidacja adnotacji danych w formularzach sieci Web. Zamiast formantu sprawdzania poprawności każdego danych wejściowych, można zdefiniować ograniczenia w klasach modeli i używaj ich we wszystkich aplikacji sieci web. W tej sekcji dowiesz się, jak weryfikowania formularza klienta nowy i edytować za pomocą adnotacji danych.

1. Otwórz **CustomerDetail.aspx** strony. Należy zauważyć, że klient imienia i drugie imię i nazwisko **EditItemTemplate** i **InsertItemTemplate** sekcje są weryfikowane za pomocą formantów RequiredFieldValidator. Każdy moduł sprawdzania poprawności jest skojarzony określony warunek, więc musi zawierać dowolną liczbę modułów sprawdzania poprawności jako warunki sprawdzające.
2. Dodawanie adnotacji danych można sprawdzić poprawności klasy modelu klienta. Otwórz **Customer.cs** klasy w **modelu** folderu i *dekoracji* każdej właściwości, za pomocą atrybutów adnotacji danych.

    (Fragment - kodu *sieci Web adnotacje danych na formularzach laboratorium - Ex02 -*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET framework 4.5 ma rozszerzone istniejącej kolekcji adnotacji danych. To niektóre adnotacji danych można użyć: [CreditCard] [Phone], [EmailAddress], [zakres], [porównać], [Url] [FileExtensions], [wymagane] [klucza], [wyrażenia regularnego].
    > 
    > Przykłady użycia:
    > 
    > [klucza]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > Istnieje również możliwość definiowania własnych komunikaty o błędach w ramach każdego atrybutu.
3. Otwórz **CustomerDetails.aspx** i Usuń wszystkie RequiredFieldvalidators dla pól Imię i nazwisko w sekcji w EditItemTemplate i InsertItemTemplate formantu FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Jedną z zalet za pomocą adnotacji danych jest logiki sprawdzania poprawności nie jest zduplikowany na stronach aplikacji. Zdefiniuj je raz w modelu i używać go na wszystkich stronach aplikacji, które manipulować danymi.
4. Otwórz **CustomerDetails.aspx** CodeBehind i lokalizowania metodę SaveCustomer. Ta metoda jest wywoływana podczas wstawiania nowego klienta i odbiera parametru klienta od wartości kontrolki FormView. Po mapowanie między formantów strony i przyszłą obiektu parametru, zostanie wykonany ASP.NET weryfikacji modelu względem wszystkich adnotacji danych atrybutów i wypełnienie słownika ModelState wystąpiły błędy. Jeśli istnieją.

    ModelState.IsValid tylko zwróci wartość true, jeśli wszystkie pola w modelu są prawidłowe po wykonaniu weryfikacji.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Dodaj **ValidationSummary** sterowania na końcu strony CustomerDetails, aby wyświetlić listę błędów modelu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    **ShowModelStateErrors** jest nową właściwość w podsumowaniu ValidationSummary kontrolować, czy wartość **true**, kontrolka będzie wyświetlać błędy ze słownika ModelState. Błędy te pochodzą z weryfikacji adnotacji danych.
6. Naciśnij klawisz **F5** do uruchomienia aplikacji sieci Web. Wypełnij formularz z niektórych błędne wartości, a następnie kliknij przycisk **zapisać** do wykonywania sprawdzania poprawności. Zwróć uwagę, błąd podsumowania u dołu.

    ![Sprawdzanie poprawności przy użyciu adnotacji danych](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "weryfikacji przy użyciu adnotacji danych")

    *Sprawdzanie poprawności przy użyciu adnotacji danych*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Zadanie 3 — Obsługa błędów w niestandardowej bazie danych z ModelState

W poprzedniej wersji formularzy sieci Web obsługi błędów bazy danych, takie jak zbyt długi ciąg lub naruszenie unikatowego klucza może obejmować zgłaszanie wyjątków w kodzie repozytorium, a następnie Obsługa wyjątków w Twojej kodem do wyświetlenia wystąpił błąd. Dużą ilość kodu jest wymagana do czymś stosunkowo proste.

W 4.5 formularzy sieci Web obiekt ModelState można wyświetlić błędy na stronie z modelu lub z bazy danych, w sposób ciągły.

W tym zadaniu dodasz kod, aby poprawnie Obsługa wyjątków bazy danych i wyświetlić odpowiedni komunikat do użytkownika przy użyciu obiektu ModelState.

1. Gdy aplikacja jest nadal uruchomiona, spróbuj zaktualizować nazwę kategorii przy użyciu zduplikowanych wartości.

    ![Aktualizowanie kategorii z nazwą zduplikowanych](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "aktualizowanie kategorii z nazwą zduplikowanych")

    *Aktualizowanie kategorii z nazwą zduplikowanych*

    Należy zauważyć, że wyjątek z powodu &quot;unikatowy&quot; ograniczenie **CategoryName** kolumny.

    ![Wyjątek dla nazwy kategorii zduplikowanych](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "wyjątek dla nazwy kategorii zduplikowanych")

    *Wyjątek dla nazwy kategorii zduplikowanych*
2. Zatrzymaj debugowanie. W **Products.aspx.cs** plik CodeBehind, aktualizacja **UpdateCategory** sposób obsługi wyjątków zgłaszanych przez bazę danych. Metoda SaveChanges() wywołania i dodać błąd **ModelState** obiektu.

    Nowy **TryUpdateModel** metody aktualizacji obiektu kategorii pobrane z bazy danych przy użyciu danych formularza przez użytkownika.

    (Fragment - kodu *sieci Web formularzy laboratorium - Ex02 - dojście UpdateCategory błędy*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > Najlepiej trzeba zidentyfikować przyczynę DbUpdateException i sprawdź, czy główną przyczyną jest naruszenie ograniczenia klucza unique.
3. Otwórz **Products.aspx** i Dodaj **ValidationSummary** kontroli poniżej kategorii GridView, aby wyświetlić listę błędów modelu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Uruchom witrynę i przejdź do strony produktów. Spróbuj zaktualizować nazwę kategorii przy użyciu zduplikowanych wartości.

    Należy zauważyć, że określony wyjątek został obsłużony i komunikat o błędzie pojawia się w **ValidationSummary** formantu.

    ![Zduplikowany błąd kategorii](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "zduplikowane błąd kategorii")

    *Błąd zduplikowanych kategorii*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Zadanie 4 — żądanie weryfikacji w formularzach sieci Web platformy ASP.NET 4.5

Funkcja sprawdzania poprawności żądania w programie ASP.NET zawiera pewien poziom domyślną ochronę przed atakami skryptów między witrynami (XSS). W poprzednich wersjach programu ASP.NET Weryfikacja żądania zostało włączone domyślnie i może być wyłączona dla całej strony. W nowej wersji składnika ASP.NET Web Forms możesz teraz wyłączyć weryfikację żądań dla pojedynczego kontrola, sprawdzania poprawności żądania opóźnieniem lub dostępu do danych żądania niesprawdzone (należy zachować ostrożność, jeśli tak zrobisz!).

1. Naciśnij klawisz **Ctrl + F5** uruchomić witrynę bez debugowania i przejdź do strony produktów. Wybierz kategorię, a następnie kliknij przycisk **Edytuj** łącze na każdy z produktów.
2. Podaj opis zawierający potencjalnie niebezpieczną zawartość, na przykład tym tagów HTML. Uwzględniać wyjątek z powodu weryfikacji żądań.

    ![Edytowanie produkt o zawartości potencjalnie niebezpiecznych](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "edycji produkt o potencjalnie niebezpieczną zawartość")

    *Edytowanie produktu z potencjalnie niebezpieczną zawartość.*

    ![Wyjątek z powodu żądania weryfikacji](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "wyjątek z powodu żądania weryfikacji")

    *Wyjątek z powodu żądania weryfikacji*
3. Zamknij stronę i w programie Visual Studio, naciśnij klawisz **SHIFT + F5** można zatrzymać debugowania.
4. Otwórz **ProductDetails.aspx** strony i Znajdź **opis** pola tekstowego.
5. Dodaj nowe **ValidateRequestMode** właściwości pole tekstowe i ustaw dla niego wartość **wyłączone**.

    Nowy **ValidateRequestMode** atrybutu umożliwia wyłączenie sprawdzania poprawności żądania częściami w każdej kontrolki. Jest to przydatne, jeśli chcesz użyć danych wejściowych, który może odbierać kodu HTML, ale chce zachować weryfikacji pracy pozostałej części strony.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Naciśnij klawisz **F5** do uruchomienia aplikacji sieci web. Otwórz ponownie Edytuj stronę produktu i ukończyć tagów HTML w tym opis produktu. Zwróć uwagę, można teraz dodać zawartość HTML w opisie.

    ![Żądanie weryfikacji wyłączone opisu produktu](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "żądania weryfikacji wyłączone dla opis produktu.")

    *Sprawdzanie poprawności żądań wyłączone dla opis produktu.*

    > [!NOTE]
    > W aplikacji produkcyjnej, powinien oczyszczenia kodu HTML, wprowadzony przez użytkownika, aby się upewnić, że tylko bezpiecznych znaczniki HTML są wprowadzane (na przykład istnieją nie &lt;skryptu&gt; tagów). Aby to zrobić, można użyć [ochrony sieci Web biblioteki](https://www.nuget.org/packages/AntiXSS).
7. Edytuj ponownie produkt. W polu nazwy wpisz kod HTML, a następnie kliknij przycisk **zapisać**. Należy zauważyć, że żądania weryfikacji tylko jest wyłączona dla pola Opis, a pozostałe pola re nadal weryfikowana pod kątem zawartości potencjalnie niebezpiecznych.

    ![Żądanie sprawdzania poprawności włączone w pozostałych polach](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "żądania weryfikacji w pozostałych polach włączone")

    *W pozostałych polach włączone sprawdzanie poprawności żądań*

    Formularze sieci Web platformy ASP.NET 4.5 obejmuje nowy tryb weryfikacji żądania do sprawdzania poprawności żądania opóźnieniem. Tryb weryfikacji żądania ustawiono na **4.5**, jeśli element uzyskuje dostęp do kodu *Request.Form [&quot;klucza&quot;]*, ASP.NET 4.5 żądania weryfikacji będzie tylko wyzwalacz żądania weryfikacji dla tego określonego elementu w kolekcji formularza.

    Ponadto ASP.NET 4.5 teraz obejmuje procedury kodowania core z v4.0 Anti-XSS biblioteki. Anti-XSS kodowania procedury są implementowane przez nowy *AntiXssEncoder* odnaleźć typu w nowym **System.Web.Security.AntiXss** przestrzeni nazw. Z **encoderType** skonfigurowana do używania parametru *AntiXssEncoder*, wszystkie dane wyjściowe kodowanie automatycznie w programie ASP.NET używa nowe procedury kodowania.
8. Program ASP.NET 4.5 żądania weryfikacji obsługuje również niesprawdzone dostęp do danych żądania. Program ASP.NET 4.5 dodaje nową właściwość kolekcji do **HttpRequest** obiektu o nazwie **Unvalidated**. Po przejściu do **HttpRequest.Unvalidated** masz dostęp do wszystkich typowych fragmentów danych żądania, w tym formularzy, QueryStrings, plików cookie, adresy URL i tak dalej.

    ![Obiekt Request.Unvalidated](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated obiektu")

    *Obiekt Request.Unvalidated*

    > [!NOTE]
    > **Należy zachować ostrożność przy Użyj właściwości HttpRequest.Unvalidated!** Upewnij się, że dokładnie wykonywania niestandardowego sprawdzania poprawności danych żądania raw, sprawdź, czy tekst niebezpiecznych nie wysyłać, zwracać i renderowania do klientów podejrzewający!

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Ćwiczenie 3: Strona asynchronicznego przetwarzania w sieci Web ASP.NET formularzy

W tym ćwiczeniu zostaną wprowadzone do nowej strony asynchronicznego przetwarzania funkcje w formularzach sieci Web ASP.NET.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Zadanie 1 - aktualizowania produktu szczegóły strony, aby przekazać i wyświetlanie obrazów

W tym zadaniu zostanie zaktualizowana na stronie Szczegóły produktu umożliwia użytkownikom, podaj adres URL obrazu, produktu i wyświetlić je w widoku tylko do odczytu. Utworzy kopię lokalną określonego obrazu pobierając synchronicznie. W następnym zadaniem spowoduje zaktualizowanie tej implementacji, aby pracować asynchronicznie.

1. Otwórz **programu Visual Studio 2012** i załadować **rozpocząć** rozwiązania, znajdujących się w **Source\Ex3 Async\Begin** z folderu tego laboratorium. Alternatywnie można kontynuować pracę na istniejące rozwiązania z poprzednich ćwiczeń.

    1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. W tym celu w Eksploratorze rozwiązań kliknij **WebFormsLab** projekt i wybierz **Zarządzaj pakietami NuGet**.
    2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
    3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

    > [!NOTE]
    > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz **ProductDetails.aspx** strony źródłowej i Dodaj pole w ItemTemplate FormView, aby wyświetlić obraz produktu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Dodaj pole, aby określić adres URL obrazu w FormView EditTemplate.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Otwórz **ProductDetails.aspx.cs** CodeBehind i dodaj następujące dyrektywy przestrzeni nazw.

    (Fragment - kodu *sieci Web przestrzenie nazw laboratorium - Ex03 - formularze*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Utwórz **UpdateProductImage** metody do przechowywania obrazów zdalnego lokalnej **obrazów** folderu i zaktualizuj jednostki produktu z nową wartość obrazu w lokalizacji.

    (Fragment - kodu *sieci Web formularzy laboratorium - Ex03 - UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Aktualizacja **UpdateProduct** metodę do wywołania **UpdateProductImage** metody.

    (Fragment - kodu *sieci Web formularzy laboratorium - Ex03 - UpdateProductImage wywołania*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Uruchom aplikację i spróbuj przekazać obraz produktu. Na przykład można użyć następującego adresu URL obrazu z Arts klip pakietu Office: [ [http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![Ustawienie obrazu produktu](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "ustawienie obrazu produktu")

    *Ustawienie obrazu produktu*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Zadanie 2 — Dodawanie asynchronicznego przetwarzania do strony szczegółów produktu

W tym zadaniu zostanie zaktualizowana na stronie Szczegóły produktu do własnych preferencji asynchronicznie. Zwiększy długotrwałych zadań — proces pobierania obrazu — przy użyciu przetwarzania asynchronicznego strony ASP.NET 4.5.

Metod asynchronicznych w aplikacjach sieci web można zoptymalizować sposób, używane są pule wątków programu ASP.NET. W programie ASP.NET są ograniczoną liczbę wątków w puli wątków dla uczestnictwa żądań, w związku z tym podczas wszystkie wątki są zajęte, ASP.NET uruchamia do odrzucania żądań nowe, wysyła komunikaty o błędach aplikacji i powoduje, że witryna.

Czas operacji w witrynie sieci web są obiekty doskonale nadające się programowania asynchronicznego, ponieważ zajmują przypisanej wątku przez długi czas. W tym długo wykonywanych żądań się wiele różnych elementów i stron, które wymagają operacji w trybie offline, takie kwerendy bazy danych lub uzyskiwania dostępu do zewnętrznego serwera sieci web. Zaletą jest to, że jeśli używasz metody asynchronicznej dla tych operacji podczas przetwarzania strony, wątek jest zwalniany i zwrócony do wątku puli i może służyć do obsługi nowego żądania strony. Oznacza to strona rozpocznie przetwarzanie w jednym wątku z puli wątków i może zostać ukończone przetwarzanie w innym, po zakończeniu przetwarzania asynchronicznego.

1. Otwórz **ProductDetails.aspx** strony. Dodaj **Async** atrybutu w **strony** element i ustaw ją na **true**. Ten atrybut informuje program ASP.NET do zaimplementowania interfejsu IHttpAsyncHandler.


    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Dodaj etykietę w dolnej części strony, aby wyświetlić szczegóły wątki uruchomione na stronie.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Otwórz **ProductDetails.aspx.cs** i dodaj następujące dyrektywy przestrzeni nazw.

    (Fragment - kodu *sieci Web formularzy nazw laboratorium - Ex03 - 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Modyfikowanie **UpdateProductImage** metody do pobierania obrazu o zadaniu asynchronicznym. Spowoduje zastąpienie **WebClient** **DownloadFile** metody z **DownloadFileTaskAsync** — metoda i obejmują **await** — słowo kluczowe.

    (Fragment - kodu *sieci Web formularzy laboratorium - Ex03 - UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    RegisterAsyncTask rejestruje nowe zadanie asynchroniczne strony ma być wykonywana w innym wątku. Odbiera wyrażenia lambda za pomocą zadania (t) do wykonania. **Await** — słowo kluczowe w **DownloadFileTaskAsync** metoda konwertuje pozostałe metody do wywołania zwrotnego, które jest wywoływane asynchronicznie po **DownloadFileTaskAsync** metody została ukończona. ASP.NET wznowi wykonywanie metody automatycznie zachowanie wszystkich HTTP żądania oryginalnych wartości. Nowy model programowania asynchronicznego w programie .NET 4.5 umożliwia pisanie asynchroniczne kodu, który wygląda bardzo podobnie synchroniczne kodu i pozwól kompilatora obsługi komplikacji funkcje wywołania zwrotnego lub kontynuacji kodu.
    > [!NOTE]
    > RegisterAsyncTask i PageAsyncTask były dostępne od wersji .NET 2.0. Słowo kluczowe await nowego z modelu programowania asynchronicznego .NET 4.5 i może być używany razem nowych metod TaskAsync z obiektu .NET WebClient.
5. Dodaj kod, aby wyświetlić wątków, na których kod rozpoczęcia i zakończenia, wykonywania. Aby to zrobić, należy zaktualizować **UpdateProductImage** metodę z następującym kodem.

    (Fragment - kodu *Web laboratorium formularze — Ex03 — Pokaż wątki*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Otwórz witrynę sieci web **Web.config** pliku. Dodaj następującą zmienną appSetting.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Naciśnij klawisz **F5** do uruchomienia aplikacji, a następnie Przekaż obraz dla produktu. Zwróć uwagę, identyfikator wątków, gdzie może być inny kod rozpoczęcia i zakończenia. Jest to spowodowane asynchroniczne zadania uruchomione w oddzielnym wątku z puli wątków programu ASP.NET. Po ukończeniu zadania ASP.NET umieszcza w kolejce zadania i przypisuje do dowolnej z dostępnych wątków.

    ![Pobieranie asynchroniczne obrazu](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "asynchronicznie pobierania obrazu")

    *Pobieranie asynchroniczne obrazu*

> [!NOTE]
> Ponadto można wdrożyć tę aplikację Azure następujących [dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym laboratorium praktycznego zostały skierowane i przedstawiono następujące kwestie:

- Użyj jednoznacznie wyrażenia wiązania danych
- Korzystać z nowych funkcji powiązania modelu w formularzach sieci Web
- Użyj dostawców wartości do mapowania danych strony metody związane z kodem
- Za pomocą adnotacji danych do sprawdzania poprawności danych wejściowych użytkownika
- Wykonaj advange unobstrusive weryfikacji po stronie klienta z jQuery w formularzach sieci Web
- Implementuje weryfikację żądań szczegółowego
- Implementuje stronę asynchronicznego przetwarzania w formularzach sieci Web

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji  **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; *programu Visual Studio Express 2012 for Web z zestawem Azure SDK*&quot;.
2. Polecenie **teraz zainstalować**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Instalowanie programu Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "instalacji programu Visual Studio Express")

    *Instalowanie programu Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Akceptowanie umowy licencyjnej*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *VS Express for Web kafelka*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy

Ten dodatek opisano sposób tworzenia nowej witryny sieci web z portalu Azure i opublikować aplikację, uzyskane wykonując laboratorium, korzystając z funkcji publikowania narzędzia Web Deploy dostarczany przez platformę Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Zadanie 1 — Tworzenie nowej witryny sieci Web w portalu Azure

1. Przejdź do [portalu zarządzania Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń Microsoft skojarzonych z Twoją subskrypcją.

    > [!NOTE]
    > Przy użyciu platformy Azure można udostępniać 10 witryn sieci Web platformy ASP.NET bezpłatnie i następnie Skaluj w miarę zwiększania się ruchu. Możesz utworzyć konto [tutaj](http://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do portalu Windows Azure](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Zaloguj się do portalu Windows Azure")

    *Zaloguj się do portalu*
2. Kliknij przycisk **nowy** na pasku poleceń.

    ![Tworzenie nowej witryny sieci Web](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "tworzenia nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij przycisk **obliczeniowe** | **witryny sieci Web**. Następnie wybierz **szybkie tworzenie** opcji. Podaj dostępny adres URL dla nowej witryny sieci web, a następnie kliknij przycisk **tworzenie witryny sieci Web**.

    > [!NOTE]
    > Azure jest hostem dla aplikacji sieci web w chmurze, które można kontrolować i zarządzanie nimi. Opcja szybkie tworzenie umożliwia wdrażanie aplikacji sieci web ukończone do platformy Azure z spoza portalu. Nie obejmuje kroki konfigurowania bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")

    *Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*
4. Poczekaj na nowe **witryny sieci Web** jest tworzony.
5. Po utworzeniu witryny sieci Web kliknij łącze w obszarze **adres URL** kolumny. Sprawdź, czy działa nowej witryny sieci Web.

    ![Przeglądanie do nowej witryny sieci web](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "przeglądanie do nowej witryny sieci web")

    *Przeglądanie do nowej witryny sieci web*

    ![Witryna sieci Web działa](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "uruchamiania witryny sieci Web")

    *Witryna sieci Web uruchomiona*
6. Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.

    ![Otwieranie stron witryny sieci web zarządzania](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "otwieranie stron zarządzania witryny sieci web")

    *Otwieranie stron zarządzania witryny sieci Web*
7. W **pulpitu nawigacyjnego** w obszarze **szybkiego dostępu** kliknij **pobieranie profilu publikowania** łącza.

    > [!NOTE]
    > *Profilu publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web na platformie Azure dla każdej metody włączone publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego z punktów końcowych, dla których włączono metoda publikacji. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **programu Microsoft Visual Studio 2012** obsługują odczytywanie publikowanie profile do zautomatyzowania te programy Publikowanie aplikacji sieci web na platformie Azure.

    ![Pobieranie witryny sieci web profilu publikowania](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "pobierania witryny sieci web profilu publikowania")

    *Pobieranie witryny sieci Web profilu publikowania*
8. Pobierz profil publikowania w znanej lokalizacji. Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web na platformie Azure w programie Visual Studio przy użyciu tego pliku.

    ![Zapisywanie pliku profilu publikowania](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z programu SQL Server baz danych, należy utworzyć serwer bazy danych SQL. Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.

1. Będzie potrzebny serwer bazy danych SQL do przechowywania bazy danych aplikacji. Można wyświetlić serwery bazy danych SQL z subskrypcji usługi Azure Management Portal pod adresem **baz danych Sql** | **serwerów** | **pulpitu nawigacyjnego serwera**. Jeśli nie masz serwer, który został utworzony, można utworzyć przy użyciu jednego **Dodaj** przycisk paska poleceń. Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, jak będą używane w następnego zadania. Nie należy tworzyć bazy danych jeszcze, jako zostaną utworzone w późniejszym terminie.

    ![Pulpit nawigacyjny serwera bazy danych SQL](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "pulpitu nawigacyjnego serwera bazy danych SQL")

    *Pulpit nawigacyjny serwera bazy danych SQL*
2. W następnym zadaniem Testuj połączenie z bazą danych z programu Visual Studio z tego powodu należy uwzględnić lokalny adres IP serwera liście **dozwolone adresy IP**. Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) przycisku.

    ![Dodawanie adresu IP klienta](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Dodawanie adresu IP klienta*
3. Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP kliknij na **zapisać** o potwierdzenie zmian.

    ![Potwierdzenie zmian](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Potwierdzenie zmian*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 - publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **publikowania**.

    ![Publikowanie aplikacji](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "publikowania aplikacji")

    *Publikowanie witryny sieci web*
2. Zaimportuj profil publikowania, zapisana w pierwszym zadaniu.

    ![Importowanie profilu publikowania](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "Importowanie profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij przycisk **Weryfikacja połączenia z**. Po zakończeniu sprawdzania kliknij **dalej**.

    > [!NOTE]
    > Zakończeniu sprawdzania poprawności, gdy zostanie wyświetlony zielony znacznik wyboru są wyświetlane obok przycisku sprawdzania poprawności połączenia.

    ![Sprawdzanie poprawności połączenia](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "sprawdzanie poprawności połączenia")

    *Sprawdzanie poprawności połączenia*
4. W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk Dalej, aby textbox połączenia bazy danych (tj. **połączenia DefaultConnection**).

    ![Konfiguracja narzędzia Web deploy](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Konfiguracja narzędzia Web deploy")

    *Konfiguracja narzędzia Web deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

    - W **nazwy serwera** wpisz swoją bazą danych SQL server adresu URL przy użyciu *tcp:* prefiks.
    - W **nazwy użytkownika** wpisz nazwę logowania administratora serwera.
    - W **hasło** wpisz hasło logowania administratora serwera.
    - Wpisz nazwę nowej bazy danych.

    ![Konfigurowanie parametrów połączenia z lokalizacją docelową](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "Konfigurowanie parametrów połączenia z lokalizacją docelową")

    *Konfigurowanie parametrów połączenia z lokalizacją docelową*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu można utworzyć bazy danych kliknij **tak**.

    ![Tworzenie bazy danych](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "tworzenie parametry bazy danych")

    *Tworzenie bazy danych*
7. Ciągu połączenia używanego do łączenia z bazą danych SQL platformy Azure jest wyświetlany w pole tekstowe domyślne połączenie. Następnie kliknij przycisk **Dalej**.

    ![Parametry połączenia wskazujące bazę danych SQL](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "ciąg połączenia wskazujące bazę danych SQL")

    *Parametry połączenia wskazujące bazę danych SQL*
8. W **Podgląd** kliknij przycisk **publikowania**.

    ![Publikowanie aplikacji sieci web](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "publikowania aplikacji sieci web")

    *Publikowanie aplikacji sieci web*
9. Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Dodatek C: korzystania z wstawek kodu

Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki. Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.

![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*

***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragment (bez spacji i łączniki).
3. Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.
4. Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

![Rozpocznij wpisywanie nazwy fragment](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "zacznij wpisywać nazwę wstawki programu")

*Rozpocznij wpisywanie nazwy fragment kodu*

![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")

*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

![Ponownie naciśnij klawisz Tab i fragment rozwinie](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")

*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*

***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)*** 1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.

1. Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.
2. Wybierz odpowiedni fragment z listy, klikając go.

![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")

*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*

![Wybierz odpowiedni fragment z listy, klikając go](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "wybierz odpowiedni fragment z listy, klikając go")

*Wybierz odpowiedni fragment z listy, klikając go*
