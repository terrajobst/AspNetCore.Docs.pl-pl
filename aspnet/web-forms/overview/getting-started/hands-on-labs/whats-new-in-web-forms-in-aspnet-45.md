---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: What's New in sieci Web formularzy w programie ASP.NET 4.5 | Dokumentacja firmy Microsoft
author: rick-anderson
description: Nowa wersja ASP.NET Web Forms wprowadzono szereg ulepszeń skoncentrowane na ułatwieniu środowisko użytkownika podczas pracy z danymi. W poprzednich wersjach...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: f36c50b64ed2363ba648297a1424b638bf9d4af5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830415"
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>What's New in formularzy sieci Web na platformie ASP.NET 4.5
====================
Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)

> Nowa wersja ASP.NET Web Forms wprowadzono szereg ulepszeń skoncentrowane na ułatwieniu środowisko użytkownika podczas pracy z danymi.
> 
> W poprzednich wersjach programu formularzy sieci Web, emisji wartość elementu członkowskiego obiektu za pomocą powiązania danych użyto wyrażenia wiązania danych Bind() lub Eval(). W nowej wersji programu ASP.NET jest możliwe do deklarowania, jakiego typu dane formant będzie powiązać za pomocą nowych właściwości typu ItemType. Ustawienie tej właściwości spowoduje włączenie użyć zmiennej silnie typizowane uzyskania pełnych korzyści programu Visual Studio środowisko programistyczne, takie jak IntelliSense, nawigowanie elementu członkowskiego i sprawdzanie w czasie kompilacji.
> 
> Za pomocą kontrolek powiązanych z danymi można teraz również określić własne niestandardowe metody wybierając, aktualizowanie, usuwanie i wstawianie danych, upraszczanie interakcji między kontrolkami strony i logika aplikacji. Ponadto zostały dodane możliwości wiązania modelu programu ASP.NET, co oznacza, że dane ze strony można mapować bezpośrednio do parametrów typu w metodzie.
> 
> Walidacja danych wejściowych użytkownika należy również łatwiej z najnowszą wersją formularzy sieci Web. Teraz możesz dodawać adnotacje do klas modelu za pomocą atrybutów sprawdzania poprawności z **System.ComponentModel.DataAnnotations** przestrzeni nazw i żądania, który kontroluje witryny sieci Sprawdź poprawność danych wejściowych przy użyciu tych informacji użytkownika. Weryfikacja po stronie klienta w formularzach sieci Web jest teraz zintegrowana z jQuery, zapewniając oczyszczarkę plików kodu po stronie klienta i funkcje dyskretnego kodu JavaScript.
> 
> W obszarze żądania weryfikacji wprowadzono ulepszenia ułatwiają selektywnie wyłączyć sprawdzanie poprawności żądań dla poszczególnych części aplikacji lub odczytać danych unieważniany żądania.
> 
> Niektóre udoskonalenia formularzy sieci Web formanty serwera, aby móc korzystać z nowych funkcji języków HTML5:
> 
> - Tryb tekstowy własności formant pola tekstowego został zaktualizowany do obsługi nowych typów wejściowych HTML5, takie jak wiadomości e-mail, daty/godziny i tak dalej.
> - Sterowanie FileUpload teraz obsługuje wiele przekazywania plików z przeglądarki, które obsługują tę funkcję HTML5.
> - Moduł sprawdzania poprawności steruje teraz obsługi sprawdzania poprawności HTML5 elementów wejściowych.
> - Nowe elementy HTML5, które mają atrybuty, które reprezentują adresu URL teraz obsługuje runat =&quot;serwera&quot;. Co w efekcie można używać konwencji platformy ASP.NET w ścieżki adresu URL, takie jak ~ operatora do reprezentowania katalog główny aplikacji (na przykład &lt;wideo runat =&quot;serwera&quot; src =&quot;~/myVideo.wmv&quot; &gt; &lt;/video&gt;).
> - Kontrolki UpdatePanel został rozwiązany do obsługi pól wejściowych ogłaszania HTML5.
> 
> W portalu oficjalne platformy ASP.NET można znaleźć więcej przykładów dotyczących nowych funkcji w programie ASP.NET 4.5 formularzy sieci Web: [What's New in ASP.NET 4.5 i programu Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym praktyczne laboratorium dowiesz się jak:

- Użyj wyrażenia wiązania danych silnie typizowane
- Korzystać z nowych funkcji wiązania modelu w formularzach sieci Web
- Korzystanie z dostawców wartości do mapowania danych ze strony metody związanym z kodem
- Adnotacje danych na użytek walidacji danych wejściowych użytkownika
- Wykonaj advange unobstrusive weryfikacji po stronie klienta przy użyciu jQuery w formularzach sieci Web
- Implementowanie weryfikacji żądań szczegółowe
- Implementowanie strony asynchronicznego przetwarzania w formularzach sieci Web

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Należy dysponować następującymi elementami do przygotowania tego laboratorium:

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczyt [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

**Instalowanie fragmentów kodu**

Dla wygody większość kodu, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako fragmenty kodu programu Visual Studio. Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.

Jeśli nie jesteś zaznajomiony z fragmentów kodu w usłudze Visual Studio i chcesz dowiedzieć się, jak z nich korzystać, możesz zapoznać się z dodatku z tego dokumentu &quot; [dodatek C: za pomocą wstawek kodu programu](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To ćwiczenie praktyczne obejmuje następujących czynnościach:

1. [Ćwiczenie 1: Model powiązania w formularzach sieci Web platformy ASP.NET](#Exercise1)
2. [Ćwiczenie 2: Walidacja danych](#Exercise2)
3. [Ćwiczenie 3: Strona asynchroniczne przetwarzanie we wzorcu ASP.NET Web Forms](#Exercise3)

> [!NOTE]
> Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązania, należy uzyskać po ukończeniu ćwiczenia. Jeśli potrzebujesz dodatkowej pomocy ćwiczeń opisanych w dalszej, można użyć tego rozwiązania jako wskazówki.


Szacowany czas do ukończenia tego laboratorium: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Ćwiczenie 1: Model powiązania w formularzach sieci Web platformy ASP.NET

Nowa wersja ASP.NET Web Forms wprowadzono wiele ulepszeń skoncentrowane na ułatwieniu doświadczenia podczas pracy z danymi. W tym ćwiczeniu będzie Dowiedz się więcej o silnie typizowane kontrolki danych i wiązanie modelu.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Zadanie 1. Używanie powiązania danych silnie Typizowane

W tym zadaniu wykryje nowe powiązania silnie typizowane dostępne w programie ASP.NET 4.5.

1. Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex1 — metodyModelBinding/rozpoczęcia/** folderu.

   1. Musisz pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. Otwórz **Customers.aspx** strony. Umieść nienumerowane listy w główną kontrolką i zawierać kontrolką elementu powtarzanego wewnątrz do wyświetlania listy każdego klienta. Ustaw nazwę elementu powtarzanego na **customersRepeater** jak pokazano w poniższym kodzie.

    W poprzednich wersjach programu formularzy sieci Web po używanie powiązania danych do emitowania wartość elementu członkowskiego w obiekcie masz powiązanie danych w celu, należy użyć wyrażenia wiązania danych oraz wywołanie metody Eval, przekazywanie nazwę elementu członkowskiego jako ciąg.

    W czasie wykonywania te wywołania Eval będzie używać odbicia względem aktualnie powiązany obiekt ma zostać odczytana wartość elementu członkowskiego o podanej nazwie i wyświetlić wyniki w formacie HTML. To podejście sprawia, że bardzo proste powiązanie danych względem dowolnego unshaped danych.

    Niestety tracisz wiele funkcji wspaniałego środowiska czasu projektowania w programie Visual Studio, w tym funkcji IntelliSense dla nazwy elementów członkowskich, obsługę nawigacji (np. Przejdź do definicji) i sprawdzanie w czasie kompilacji.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Otwórz **Customers.aspx.cs** pliku.
4. Dodaj następującą instrukcję using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. W **strony\_obciążenia** metody, Dodaj kod, aby wypełnić elementu powtarzanego z listą klientów.

    (Code Snippet — *sieci Web źródła danych klientów powiązania laboratorium — Ex01 — formularze*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    Rozwiązanie używa platformy EntityFramework, wraz z CodeFirst do tworzenia i dostęp do bazy danych. W poniższym kodzie customersRepeater jest powiązany z zmaterializowany zapytania, które zwraca wszystkich klientów z bazy danych.
6. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie, a następnie przejdź do **klientów** strony, aby zobaczyć powtarzanego w działaniu. Jako rozwiązanie korzysta z CodeFirst, baza danych zostanie utworzona i wypełnione w lokalnym wystąpieniu programu SQL Express, po uruchomieniu aplikacji.

    ![Listę klientów z repeater](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "listę klientów z repeater")

    *Listę klientów z repeater*

    > [!NOTE]
    > W programie Visual Studio 2012 usług IIS Express jest domyślny serwer programowanie sieci Web.
7. Zamknij przeglądarkę i przejdź wstecz do programu Visual Studio.
8. Teraz Zastąp wdrożenia do użycia w silnie typizowany powiązania. Otwórz **Customers.aspx** strony i użyj nowych **ItemType** atrybutu w elemencie powtarzanym można ustawić **klienta** typ jako typ powiązania.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    Właściwość ItemType można zadeklarować typy danych wkrótce może być powiązane z formantem i pozwala na używanie silnie typizowaną powiązania wewnątrz kontrolki powiązania danych.
9. Zastąp zawartość następującym kodem ItemTemplate.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Wadą jednej z powyższych podejść jest możliwość wywołania Eval() i Bind() z późnym wiązaniem - co oznacza, że przekazujesz ciągów do reprezentowania nazwy właściwości. Oznacza to, że nie korzystania z funkcji Intellisense dla nazwy elementów członkowskich, obsługa nawigowanie po kodzie (na przykład przejdź do definicji) ani obsługi sprawdzanie, czy w czasie kompilacji.

    Ustawienie właściwości ItemType powoduje, że dwie nowe wpisane zmienne do wygenerowania w zakresie wyrażenia wiązania danych: **elementu** i **BindItem**. Można użyć tych silnie typizowaną zmiennych w wyrażeniach wiązania danych i korzyści pełne środowisko programistyczne programu Visual Studio.

    &quot; **:** &quot; Wyrażenia będzie automatycznie kodowanie HTML dane wyjściowe, aby uniknąć problemów z zabezpieczeniami (na przykład między lokacjami ataki z użyciem skryptów). Ten zapis była dostępna od wersji .NET 4 Zapisywanie odpowiedzi, ale teraz jest również dostępna w wyrażeniach wiązania danych.

    > [!NOTE]
    > Element członkowski elementu działa w przypadku powiązania jednokierunkowe. Jeśli chcesz przeprowadzić użycia określają powiązanie dwukierunkowe **BindItem** elementu członkowskiego.

    ![Obsługa funkcji IntelliSense w powiązaniu silnie typizowane](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "obsługę funkcji IntelliSense w powiązaniu silnie typizowane")

    *Obsługa funkcji IntelliSense w powiązaniu silnie typizowane*
10. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie, a następnie przejdź do strony klientów, aby upewnić się, czy zmiany działają zgodnie z oczekiwaniami.

    ![Wyświetlanie szczegółów odbiorcy](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "ofercie szczegóły klienta")

    *Wyświetlanie szczegółów klienta*
11. Zamknij przeglądarkę i przejdź wstecz do programu Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Zadanie 2 — wprowadzenie do modelu powiązania w formularzach sieci Web

W poprzednich wersjach programu ASP.NET Web Forms gdy chcesz wykonać dwukierunkowego wiązania danych, pobieranie i aktualizowanie danych, trzeba było używać obiektu źródła danych. Może to być źródło danych obiektu, źródło danych SQL, źródła danych LINQ i tak dalej. Jednak w razie potrzeby danego scenariusza niestandardowego kodu do obsługi danych potrzebnych do korzystania z obiektu źródła danych, a to wprowadzone pewne wady. Na przykład konieczne unikanie typów złożonych i potrzebny do obsługi wyjątków, podczas wykonywania logiki weryfikacji.

W nowej wersji wzorca ASP.NET Web Forms, formanty powiązane z danymi obsługują wiązanie modelu. Oznacza to, że możesz określić wybierz, aktualizacji, wstawiania i usunięcie metod bezpośrednio w kontrolce powiązanych z danymi do wywołania logiki z pliku związanego z kodem lub z innej klasy.

Aby dowiedzieć się więcej na ten temat, użyjesz GridView, aby wyświetlić listę kategorii produktów, za pomocą nowego **metody SelectMethod** atrybutu. Ten atrybut można określić metodę pobierania danych widoku GridView.

1. Otwórz **Products.aspx** strony i obejmują **GridView**. Skonfiguruj widoku GridView, jak pokazano poniżej silnie typizowane powiązań oraz włączyć sortowanie i stronicowanie.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Użyj nowego **metody SelectMethod** atrybutu, aby skonfigurować GridView wywołać **GetCategories** metodę, aby wybrać dane.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Otwórz **Products.aspx.cs** związanym z kodem i dodaj następujące za pomocą instrukcji.

    (Code Snippet — *Web Forms laboratorium — Ex01 — w przestrzeni nazw*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Dodaj od prywatnej składowej w **produktów** klasy i przypisać nowe wystąpienie klasy **ProductsContext**. Ta właściwość będzie przechowywać kontekstu danych narzędzia Entity Framework, która umożliwia nawiązanie połączenia z bazą danych.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Tworzenie **GetCategories** metody do pobierania listy kategorii za pomocą LINQ. Kwerenda będzie zawierała **produktów** właściwości, więc widoku GridView można wyświetlić ilość produktów dla każdej kategorii. Należy zauważyć, że metoda zwraca obiekt IQueryable raw, który reprezentuje zapytanie, aby być wykonywane później cyklu życia strony.

    (Code Snippet — *Web Forms laboratorium - Ex01 - GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > W poprzednich wersjach programu ASP.NET Web Forms Włączanie sortowanie i stronicowanie za pomocą własnej logiki repozytorium w kontekście źródło danych obiektu musieli napisać własny kod niestandardowy i otrzymywać wszystkie niezbędne parametry. Teraz, jako metody powiązanie danych może zwrócić IQueryable i reprezentuje zapytanie nadal do wykonania, ASP.NET zająć się modyfikowanie zapytanie, aby dodać prawidłowe sortowanie i stronicowanie parametrów.
6. Naciśnij klawisz **F5** aby rozpocząć debugowanie witryny i przejdź do strony produktów. Powinien zostać wyświetlony, widoku GridView jest wypełniana kategorie zwracany przez metodę GetCategories.

    ![Wypełnianie GridView przy użyciu wiązania modelu](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "wypełnianie GridView przy użyciu wiązania modelu")

    *Wypełnianie GridView przy użyciu wiązania modelu*
7. Naciśnij klawisz **SHIFT**+**F5** zatrzymać debugowanie.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Zadanie 3 - dostawców wartości w wiązanie modelu

Wiązanie modelu nie tylko umożliwia określenie niestandardowych metod do pracy z danymi bezpośrednio w kontrolce powiązanych z danymi, ale również pozwala na mapowanie danych ze strony do parametrów przy użyciu tych metod. Dla parametru metody można użyć atrybuty dostawcy wartości, aby określić źródło danych tej wartości. Na przykład:

- Formanty na stronie
- Wartości ciągu zapytania
- Wyświetlanie danych
- Stan sesji
- Pliki cookie
- Przesłanego formularza danych
- Wyświetl stan
- Wartość niestandardowa obsługiwani są także

Jeśli używasz platformy ASP.NET MVC 4, zauważymy, że przypomina obsługi powiązania modelu. W rzeczywistości te funkcje zostały pobrane z platformy ASP.NET MVC i przenoszony do **System.Web** zestawu, aby móc używać ich również w formularzach sieci Web.

W tym zadaniu zostanie zaktualizowana GridView, aby filtrować wyniki według ilości produktów dla każdej kategorii, odbieranie parametru filtru za wiązania modelu.

1. Wróć do **Products.aspx** strony.
2. W górnej części widoku GridView Dodaj **etykiety** i **ComboBox** wybierz liczbę produktów dla każdej kategorii, jak pokazano poniżej.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Dodaj **EmptyDataTemplate** do kontrolki GridView, aby wyświetlić komunikat, gdy nie ma kategorii z wybraną liczbę produktów.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Otwórz **Products.aspx.cs** związanym z kodem i dodaj następującą instrukcję using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Modyfikowanie **GetCategories** metodę, aby otrzymywać całkowitą **minProductsCount** argumentu i filtrowanie zwróconych wyników. Aby to zrobić, należy zastąpić metodę z następującym kodem.

    (Code Snippet — *Web Forms laboratorium - Ex01 - GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    Nowy **[kontrola]** atrybutu na **minProductsCount** argument umożliwi wiedzieć, jego wartość muszą być wypełnione za pomocą kontrolki na stronie ASP.NET. ASP.NET będzie szukać dowolną kontrolkę pasujące do nazwy argumentu (minProductsCount) i wykonywać niezbędne mapowania i konwersji, aby wypełnić parametr wartości kontrolki.

    Alternatywnie ten atrybut zapewnia przeciążonego konstruktora, który pozwala na określenie kontroli, skąd pobrać wartości.

    > [!NOTE]
    > Pierwszym celem funkcji wiązania danych jest skrócenie kod, który ma zostać zapisany do interakcji z strony. Oprócz [kontrola] dostawcy wartości można użyć innych dostawców wiązania modelu w parametry metody. Niektóre z nich są wymienione w wprowadzenie zadań.
6. Naciśnij klawisz **F5** aby rozpocząć debugowanie witryny i przejdź do strony produktów. Wybierz z listy rozwijanej wiele produktów i zwróć uwagę, jak widoku GridView został zaktualizowany.

    ![Filtrowanie widoku GridView o wartości listy rozwijanej](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "filtrowania widoku GridView o wartości listy rozwijanej")

    *Filtrowanie widoku GridView o wartości listy rozwijanej*
7. Zatrzymaj debugowanie.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Zadanie 4 — przy użyciu modelu powiązania w celu filtrowania

W tym zadaniu należy dodać chwilę podrzędnych GridView, aby pokazać produktów należących do wybranej kategorii.

1. Otwórz **Products.aspx** strony i GridView, aby automatycznie wygenerować przycisku Wybierz kategorie aktualizacji.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Dodaj drugą **GridView** o nazwie **productsGrid** u dołu. Ustaw **ItemType** do **WebFormsLab.Model.Product**, **DataKeyNames** do **ProductId** i **metody SelectMethod**  do **GetProducts**. Ustaw **właściwość AutoGenerateColumns miała** do **false** i Dodaj kolumny ProductId, ProductName, opis i cena jednostkowa.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Otwórz **Products.aspx.cs** pliku CodeBehind. Implementowanie **GetProducts** metodę, aby otrzymywać identyfikator kategorii z kategorii GridView i filtrowania produktów. Wiązanie modelu spowoduje ustawienie wartości parametru za pomocą wybranego wiersza w **categoriesGrid**. Ponieważ nazwy argumentu i nazwa formantu nie są zgodne, należy określić nazwę formantu w dostawcy wartości kontrolki.

    (Code Snippet — *Web Forms laboratorium - Ex01 - GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Takie podejście ułatwia to jednostka tych metod testowych. W kontekście testu jednostki, której formularzy sieci Web nie jest wykonywany, atrybut [nazwa kontrolki] nie wykonuje żadnych określonych działań.
4. Otwórz **Products.aspx** strony, a następnie zlokalizuj produktów GridView. Aktualizuj produkty GridView, aby wyświetlić łącza do edytowania wybranego produktu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Otwórz **ProductDetails.aspx** strony związanym z kodem, a następnie zastąp **SelectProduct** metoda następującym kodem.

    (Code Snippet — *metodę SelectProduct laboratorium - Ex01 - formularzy sieci Web*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Należy zauważyć, że **[QueryString]** atrybut jest używany do wypełniania parametru metody z parametrem productId w ciągu zapytania.
6. Naciśnij klawisz **F5** aby rozpocząć debugowanie witryny i przejdź do strony produktów. Wybierz odpowiednią kategorię z listy kategorii GridView i zwróć uwagę, że produkty GridView został zaktualizowany.

    ![Wyświetlanie produktów z wybranej kategorii](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "przedstawiający produkty z wybranej kategorii")

    *Wyświetlanie produktów z wybranej kategorii*
7. Kliknij przycisk **widoku** łącze na produkt, aby otworzyć stronę ProductDetails.aspx.

    Należy zauważyć, że strony pobierania produktu, za pomocą metody SelectMethod za pomocą parametru productId z ciągu zapytania.

    ![Wyświetlanie szczegółów produktu](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "wyświetlanie szczegółów produktu")

    *Wyświetlanie szczegółów produktu*

    > [!NOTE]
    > W następnym ćwiczeniu zostanie wprowadzone możliwość wpisz opis w formacie HTML.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Zadanie 5. za pomocą modelu powiązania dla operacji aktualizacji

W poprzednim zadaniu użyto wiązania modelu głównie do wybierania danych, w ramach tego zadania zostanie dowiesz się, jak używać wiązania modelu w operacji aktualizowania.

Zaktualizuje kategorie GridView, aby zezwolić użytkownikom na aktualizowanie kategorii.

1. Otwórz **Products.aspx** strony i Aktualizuj kategorie GridView do automatycznego generowania przycisk Edytuj i korzystać z nowych **operacji UpdateMethod** atrybutu, aby określić **UpdateCategory**metodę, aby zaktualizować wybranego elementu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    Atrybut DataKeyNames w widoku GridView zdefiniować służą do elementów członkowskich, które identyfikują w obiekcie powiązanym ze modelu i w związku z tym, które są parametrami metody aktualizacji co najmniej powinien zostać wyświetlony.
2. Otwórz **Products.aspx.cs** pliku związanego z kodem i implementowanie **UpdateCategory** metody. Metoda powinna zostać dostarczona identyfikator kategorii w bieżącej kategorii obciążenia, wypełnij wartości z kontrolki GridView, a następnie zaktualizować kategorii.

    (Code Snippet — *Web Forms laboratorium - Ex01 - UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    Nowy **TryUpdateModel** metody w klasie strony odpowiada zapełnianie obiekt modelu, używając wartości z kontrolek na stronie. W tym przypadku zostanie zastąpiony zaktualizowanymi wartościami z bieżącego wiersza GridView edytowany w **kategorii** obiektu.

    > [!NOTE]
    > Następnym ćwiczeniu będzie zawierał wyjaśnienie użycie ModelState.IsValid sprawdzania poprawności danych wprowadzonych przez użytkownika podczas edycji obiektu.
3. Uruchamianie witryny, a następnie przejdź do strony produktów. Edytuj kategorię. Wpisz nową nazwę, a następnie kliknij przycisk **aktualizacji** aby utrwalić zmiany.

    ![Edytowanie kategorii](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "edytowanie kategorii")

    *Edytowanie kategorii*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Ćwiczenie 2: Walidacja danych

W tym ćwiczeniu dowiesz się o nowych funkcjach sprawdzania poprawności danych w programie ASP.NET 4.5. Wyewidencjonuje nowe funkcje sprawdzania poprawności dyskretnego kodu w formularzach sieci Web. Do sprawdzania poprawności danych wejściowych użytkownika użyje adnotacje danych w klasach modelu aplikacji i na koniec będzie dowiesz się, jak włączyć lub wyłączyć weryfikację żądań do poszczególnych formantów na stronie.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Zadanie 1 — sprawdzania poprawności dyskretnego kodu

Zwykle formularzy za pomocą złożonych danych, w tym moduły weryfikacji do generowania zbyt dużej ilości kodu JavaScript na stronie, które mogą reprezentować około 60% kodu. Za pomocą włączone sprawdzania poprawności dyskretnego kodu kod HTML będzie wyglądać bardziej przejrzyste i pisma odręcznego.

W tej sekcji spowoduje włączenie poprawności dyskretnego kodu na platformie ASP.NET do porównywania kodu HTML, generowane przez obie konfiguracje.

1. Otwórz **programu Visual Studio 2012** , a następnie otwórz **rozpocząć** rozwiązanie znajduje się w **Source\Ex2 Validation\Begin** folder w tym laboratorium. Alternatywnie można kontynuować pracę na istniejące rozwiązanie z poprzednim ćwiczeniu.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. W tym celu w Eksploratorze rozwiązań kliknij **WebFormsLab** projektu **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. Naciśnij klawisz **F5** do uruchomienia aplikacji sieci web. Przejdź do klientów strony, a następnie kliknij przycisk **dodać nowego klienta** łącza.
3. Kliknij prawym przyciskiem myszy na stronie przeglądarki, a następnie wybierz pozycję **Wyświetl źródło** opcję, aby otworzyć kod HTML wygenerowanymi przez aplikację.

    ![Wyświetlanie strony kod HTML](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "przedstawiający stronę kodu HTML")

    *Wyświetlanie strony kod HTML*
4. Przewiń stronę kodu źródłowego i zwróć uwagę, czy program ASP.NET ma wprowadzony JavaScript kod i dane modułów sprawdzania poprawności na stronie można wykonywać operacje sprawdzania poprawności i Pokaż listę błędów.

    ![Sprawdzanie poprawności kodu JavaScript na stronie CustomerDetails](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "kod JavaScript sprawdzania poprawności na stronie CustomerDetails")

    *Sprawdzanie poprawności kodu JavaScript na stronie CustomerDetails*
5. Zamknij przeglądarkę i przejdź wstecz do programu Visual Studio.
6. Teraz spowoduje włączenie sprawdzania poprawności dyskretnego kodu. Otwórz **Web.Config** i Znajdź **ValidationSettings:UnobtrusiveValidationMode** w **AppSettings** sekcji **.** Ustaw wartość klucza **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > Można również ustawić tę właściwość &quot; **strony\_obciążenia** &quot; zdarzeń w przypadku chcesz włączyć sprawdzania poprawności dyskretnego kodu tylko w przypadku niektórych stron.
7. Otwórz **CustomerDetails.aspx** i naciśnij klawisz **F5** do uruchomienia aplikacji sieci Web.
8. Naciśnij klawisz F12, aby otworzyć narzędzi deweloperskich programu Internet Explorer. Po otwarciu narzędzia deweloperskie, wybierz kartę skryptu. Wybierz **CustomerDetails.aspx** menu i wypełnij należy pamiętać, że skrypty wymagane do uruchomienia jQuery na stronie zostały załadowane do przeglądarki z lokacją lokalną.

    ![Trwa ładowanie jQuery JavaScript pliki bezpośrednio z lokalnego serwera IIS](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "ładowania jQuery JavaScript pliki bezpośrednio z lokalnego serwera IIS")

    *Ładowanie plików JavaScript jQuery bezpośrednio z lokalnego serwera IIS*
9. Zamknij przeglądarkę, aby powrócić do programu Visual Studio. Otwórz **Site.Master** ponownie plik i Znajdź **ScriptManager**. Dodaj atrybut **EnableCdn** właściwość z wartością **True**. Spowoduje to wymuszenie jQuery zostać załadowane z adresu URL w trybie online, a nie z lokacji lokalnej adresu URL.
10. Otwórz **CustomerDetails.aspx** w programie Visual Studio. Naciśnij klawisz F5, aby uruchomić witryny. Po otwarciu programu Internet Explorer, naciśnij klawisz F12, aby otworzyć narzędzia dla deweloperów. Wybierz **skryptu** karcie, a następnie zapoznaj się z listy rozwijanej. Należy zauważyć, że pliki JavaScript jQuery już ładowanych z lokacji lokalnej, ale raczej z jQuery online usługi CDN.

    ![Trwa ładowanie jQuery JavaScript pliki z usługi CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "jQuery JavaScript ładowanie plików z usługi CDN")

    *Ładowanie plików JavaScript jQuery z sieci CDN*
11. Otwórz kod źródłowy strony HTML, ponownie, używając opcji źródła widoku w przeglądarce. Zwróć uwagę, że przez włączenie sprawdzania poprawności dyskretnego kodu ASP.NET została wymieniona wprowadzonego kodu JavaScript przy użyciu danych — \*atrybutów.

    ![Kod sprawdzania poprawności dyskretnego kodu](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "kod sprawdzania poprawności dyskretnego kodu")

    *Kod sprawdzania poprawności dyskretnego kodu*

    > [!NOTE]
    > W tym przykładzie pokazano, jak podsumowania przy użyciu adnotacji danych weryfikacji została uproszczona do tylko kilka języków HTML i JavaScript wierszy. Wcześniej bez sprawdzania poprawności dyskretnego kodu, więcej kontrolek weryfikacji, który dodajesz, tym większe kod sprawdzania poprawności języka JavaScript rośnie.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Zadanie 2 — Sprawdzanie poprawności modelu przy użyciu adnotacji danych

Program ASP.NET 4.5 wprowadza weryfikacji adnotacji danych w formularzach sieci Web. Zamiast sprawdzania poprawności formantu w poszczególnych danych wejściowych, można zdefiniować ograniczenia w klasach modeli i używać ich w aplikacji sieci web. W tej sekcji dowiesz się, jak używać adnotacji danych sprawdzania poprawności formularz nowy/Edycja klienta.

1. Otwórz **CustomerDetail.aspx** strony. Należy zauważyć, że klient nazwij pierwszy i drugi w **EditItemTemplate** i **InsertItemTemplate** sekcje są weryfikowane przy użyciu kontrolki RequiredFieldValidator. Każdy moduł sprawdzania poprawności jest skojarzona z określonego warunku, dlatego należy dołączyć dowolną liczbę modułów weryfikacji jako warunki sprawdzające.
2. Dodaj adnotacje danych można zweryfikować klasy modelu klienta. Otwórz **Customer.cs** klasy w **modelu** folder i *dekoracji* każdej właściwości, za pomocą atrybutów adnotacji danych.

    (Code Snippet — *Web adnotacje danych na formularzach laboratorium - Ex02 -*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET framework 4.5 ma rozszerzyć istniejącą kolekcję adnotacji danych. Poniżej przedstawiono niektóre adnotacje danych, możesz użyć: [CreditCard], [Phone] [EmailAddress], [zakres], [porównać], [Url] [FileExtensions], [wymagane] [Key], [wyrażenia regularnego].
    > 
    > Niektóre przykłady użycia:
    > 
    > [Key]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > Można również definiować własne komunikaty o błędach w ramach każdego atrybutu.
3. Otwórz **CustomerDetails.aspx** i Usuń wszystkie RequiredFieldvalidators dla pól Imię i nazwisko w w sekcjach EditItemTemplate i InsertItemTemplate kontroli FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Jedną z zalet przy użyciu adnotacji danych jest logikę weryfikacji nie są zduplikowane na stronach aplikacji. Zdefiniuj go jeden raz w modelu i użyć go na wszystkich stronach aplikacji, które manipulowanie danymi.
4. Otwórz **CustomerDetails.aspx** związanym z kodem i zlokalizuj metodę SaveCustomer. Ta metoda jest wywoływana podczas wstawiania nowego klienta i odbiera parametru klienta od wartości kontrolki FormView. Podczas mapowania między kontrolkami strony i odmówić obiektu parametr ASP.NET będą wykonywane sprawdzanie poprawności modelu względem wszystkich adnotacji danych atrybutów, a wypełnienie słownika ModelState wystąpiły błędy. Jeśli istnieją.

    ModelState.IsValid tylko zwróci wartość true, jeśli wszystkie pola w modelu są prawidłowe po przeprowadzeniu weryfikacji.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Dodaj **podsumowania walidacji** kontroli na końcu strony CustomerDetails, aby wyświetlić listę błędów modelu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    **ShowModelStateErrors** jest nową właściwość na podsumowania walidacji kontrolkę, która po ustawieniu **true**, kontrolka będzie wyświetlać błędy ze słownika ModelState. Te błędy pochodzą z weryfikacji adnotacji danych.
6. Naciśnij klawisz **F5** do uruchamiania aplikacji sieci Web. Wypełnij formularz z niektóre błędne wartości, a następnie kliknij przycisk **Zapisz** do wykonywania sprawdzania poprawności. Zwróć uwagę, podsumowanie u dołu błędów.

    ![Weryfikacja przy użyciu adnotacji danych](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "weryfikacji przy użyciu adnotacji danych")

    *Weryfikacja przy użyciu adnotacji danych*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Zadanie 3 — Obsługa błędów niestandardowej bazy danych, przy użyciu ModelState

W poprzednich wersjach formularzy sieci Web Obsługa błędów bazy danych, takie jak zbyt długi ciąg lub naruszenie unikatowego klucza może obejmować zgłaszanie wyjątków w kodzie repozytorium, a następnie obsługi wyjątków na usługi związane z kodem, aby wyświetlić błąd. Dużą ilość kodu, co jest wymagane do czymś stosunkowo proste.

W 4.5 formularzy sieci Web obiekt ModelState może służyć do wyświetlania błędów na stronie z modelu lub bazy danych i w sposób ciągły.

To zadanie dodasz kod, aby prawidłowo obsługiwać wyjątki bazy danych i wyświetlić odpowiedni komunikat dla użytkownika za pomocą obiektu ModelState.

1. Gdy aplikacja jest nadal uruchomione, spróbuj zaktualizować nazwę kategorii, używając zduplikowane wartości.

    ![Aktualizowanie kategorię o nazwie zduplikowane](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "aktualizowanie zduplikowana nazwa kategorii")

    *Aktualizowanie zduplikowana nazwa kategorii*

    Należy zauważyć, że wyjątek jest generowany ze względu na &quot;unikatowy&quot; ograniczenie **CategoryName** kolumny.

    ![Wyjątek dla nazwy kategorii zduplikowane](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "wyjątek dla kategorii zduplikowane nazwy")

    *Wyjątek dla kategorii zduplikowane nazwy*
2. Zatrzymaj debugowanie. W **Products.aspx.cs** pliku związanego z kodem, aktualizacji **UpdateCategory** metody, aby obsłużyć wyjątki generowane przez bazy danych. Metoda SaveChanges() wywołania i błędu, aby dodać **ModelState** obiektu.

    Nowy **TryUpdateModel** metoda aktualizuje obiekt kategorii, pobierane z bazy danych, przy użyciu danych formularza, dostarczone przez użytkownika.

    (Code Snippet — *sieci Web formularzy laboratorium - Ex02 - uchwyt UpdateCategory błędy*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > W idealnym przypadku trzeba zidentyfikować przyczynę DbUpdateException i sprawdź, czy głównej przyczyny naruszenia unikatowego ograniczenia klucza.
3. Otwórz **Products.aspx** i Dodaj **podsumowania walidacji** kontroli poniżej kategorie GridView, aby wyświetlić listę błędów modelu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Uruchamianie witryny, a następnie przejdź do strony produktów. Spróbuj zaktualizować nazwę kategorii, używając zduplikowane wartości.

    Należy zauważyć, że wyjątek został obsłużony i komunikat o błędzie pojawia się w **podsumowania walidacji** kontroli.

    ![Zduplikowane kategorii błędów](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "zduplikowane kategorii błędów")

    *Błąd zduplikowanych kategorii*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Zadanie 4. żądanie weryfikacji w formularzach sieci Web platformy ASP.NET 4.5

Funkcję weryfikacji żądania w programie ASP.NET zapewnia pewien stopień domyślną ochronę przed atakami skryptów między witrynami (XSS). W poprzednich wersjach programu ASP.NET Weryfikacja żądania zostało włączone domyślnie i może być wyłączona dla całej strony. W nowej wersji wzorca ASP.NET Web Forms można teraz wyłączono weryfikację żądań dla pojedynczego kontrola, wykonać sprawdzanie poprawności żądań z opóźnieniem lub uzyskać dostęp do danych niesprawdzone żądania (należy zachować ostrożność, jeśli możesz to zrobić!).

1. Naciśnij klawisz **kombinację klawiszy Ctrl + F5** uruchomić witrynę bez debugowania i przejdź do strony produktów. Wybierz kategorię, a następnie kliknij przycisk **Edytuj** łącze na każdy z produktów.
2. Podaj opis zawierający potencjalnie niebezpieczną treść, na przykład włączając tagi HTML. Zwróć uwagę na wyjątek z powodu weryfikacji żądań.

    ![Edytowanie produkt o potencjalnie niebezpieczną treść](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "edycji produkt o potencjalnie niebezpieczną treść")

    *Edytowanie produkt o potencjalnie niebezpieczną treść*

    ![Wyjątek zgłoszony z powodu weryfikacji żądań](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "wyjątek z powodu weryfikacji żądań")

    *Wyjątek zgłoszony z powodu weryfikacji żądań*
3. Zamknij stronę, a w programie Visual Studio, naciśnij klawisz **SHIFT + F5** Aby zatrzymać debugowanie.
4. Otwórz **ProductDetails.aspx** strony, a następnie zlokalizuj **opis** pola tekstowego.
5. Dodaj nowy **ValidateRequestMode** właściwości pola tekstowego i ustawić jej wartość na **wyłączone**.

    Nowy **ValidateRequestMode** atrybut pozwala wyłączyć sprawdzanie poprawności żądań dokładnością na każdy formant. Jest to przydatne, gdy użytkownik chce użyć danych wejściowych, które mogą odbierać kodu HTML, ale chce zachować weryfikacji pracy pozostałej części strony.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Naciśnij klawisz **F5** do uruchamiania aplikacji sieci web. Ponownie otwórz stronę produktu edycji i wypełnij opis produktu, włączając tagi HTML. Należy zauważyć, że możesz teraz dodać zawartość HTML w opisie.

    ![Żądanie weryfikacji wyłączone opisu produktu](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "żądanie weryfikacji wyłączone opis produktu")

    *Żądanie weryfikacji wyłączone opis produktu*

    > [!NOTE]
    > W przypadku aplikacji produkcyjnych należy oczyszczenia kodu HTML, wprowadzonej przez użytkownika, aby upewnić się, są wprowadzane tylko bezpiecznych tagów HTML (na przykład istnieją nie &lt;skryptu&gt; tagów). Aby to zrobić, można użyć [Biblioteka ochrony sieci Web firmy Microsoft](https://www.nuget.org/packages/AntiXSS).
7. Edytuj ponownie produkt. W polu nazwy wpisz kod HTML, a następnie kliknij przycisk **Zapisz**. Zauważ, że żądanie weryfikacji tylko jest wyłączona dla pola Opis, a pozostałe pola re nadal weryfikowany pod kątem potencjalnie niebezpieczną treść.

    ![Żądanie weryfikacji włączone w pozostałych polach](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "żądanie weryfikacji w pozostałych polach włączone")

    *Żądanie weryfikacji w pozostałych polach włączone*

    Formularze sieci Web platformy ASP.NET 4.5 zawiera nowy tryb weryfikacji żądania opóźnieniem przeprowadzić weryfikacji żądań. Tryb weryfikacji żądania ustawiono na **4.5**, jeśli fragment kodu uzyskuje dostęp do *Request.Form [&quot;klucz&quot;]*, wyzwalacza tylko ASP.NET 4.5 żądania weryfikacji będzie żądanie weryfikacji dla tego określonego elementu w kolekcji formularza.

    Ponadto ASP.NET 4.5 zawiera teraz podstawowe procedury kodowania z biblioteki Anti-XSS firmy Microsoft w wersji 4.0. XSS chroniących kodowanie procedury są wykonywane przez nowy *AntiXssEncoder* odnaleźć typu w nowym **System.Web.Security.AntiXss** przestrzeni nazw. Za pomocą **encoderType** skonfigurowany do używania parametru *AntiXssEncoder*, wszystkie dane wyjściowe kodowania w programie ASP.NET automatycznie używa nowej procedury kodowania.
8. Program ASP.NET 4.5 żądania weryfikacji obsługuje również niesprawdzone dostęp do danych żądania. Program ASP.NET 4.5 dodaje nową właściwość kolekcji do **HttpRequest** obiektu o nazwie **Unvalidated**. Po przejściu do **HttpRequest.Unvalidated** mają dostęp do wszystkich typowych rodzajów danych żądanie, w tym formularzy, ciągami zapytań, pliki cookie, adresy URL i tak dalej.

    ![Obiekt Request.Unvalidated](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated obiektu")

    *Obiekt Request.Unvalidated*

    > [!NOTE]
    > **Użyj właściwości HttpRequest.Unvalidated ostrożnie!** Upewnij się, że dokładnie wykonywania niestandardowego sprawdzania poprawności na danych pierwotnych żądania, upewnij się, że tekst niebezpiecznych jest nie-zwrotnego renderowania do klientów podejrzewający!

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Ćwiczenie 3: Strona asynchroniczne przetwarzanie we wzorcu ASP.NET Web Forms

W tym ćwiczeniu zostaną wprowadzone do nowej strony asynchronicznego przetwarzania funkcji w formularzach sieci Web platformy ASP.NET.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Zadanie 1 — aktualizowanie produktu szczegóły strony, aby przekazać i wyświetlanie obrazów

To zadanie zaktualizuje stronę szczegółów produktu, aby umożliwić użytkownikowi określić adres URL obrazu dla produktu i wyświetlania ich w widoku tylko do odczytu. Utworzysz lokalną kopię określonego obrazu, pobierając synchronicznie. W ramach następnego zadania spowoduje zaktualizowanie tej implementacji, aby umożliwić jej pracę asynchronicznie.

1. Otwórz **programu Visual Studio 2012** i załadować **rozpocząć** rozwiązanie znajduje się w **Source\Ex3 Async\Begin** z folderu w tym laboratorium. Alternatywnie można kontynuować pracę na istniejące rozwiązanie z poprzedniego ćwiczenia.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. W tym celu w Eksploratorze rozwiązań kliknij **WebFormsLab** projektu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. Otwórz **ProductDetails.aspx** strony źródła, a następnie dodaj pole w ItemTemplate FormView, aby wyświetlić obraz produktu.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Dodaj pole, aby określić adres URL obrazu w EditTemplate FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Otwórz **ProductDetails.aspx.cs** związanym z kodem i dodaj następujące dyrektywy przestrzeni nazw.

    (Code Snippet — *Web Forms laboratorium — Ex03 — w przestrzeni nazw*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Tworzenie **UpdateProductImage** metodę, aby przechowywać obrazy w zdalnym w lokalnym **obrazów** folderu i zaktualizuj jednostkę produktu z nową wartość obrazu w lokalizacji.

    (Code Snippet — *Web Forms laboratorium - Ex03 - UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Aktualizacja **UpdateProduct** metodę do wywołania **UpdateProductImage** metody.

    (Code Snippet — *sieci Web formularzy laboratorium - Ex03 - UpdateProductImage wywołanie*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Uruchom aplikację i spróbuj przekazać obraz dla produktu. Na przykład można użyć następującego adresu URL obrazu z sztuka klipu pakietu Office: [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![Ustawienie obrazu dla produktu](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "ustawienie obrazu dla produktu")

    *Ustawienie obrazu dla produktu*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Zadanie 2 — Dodawanie asynchroniczne, przetwarzanie na stronie szczegółów produktu

To zadanie zaktualizuje stronę szczegółów produktu, aby umożliwić jej pracę asynchronicznie. Długotrwałe zadanie — proces pobierania obrazu - ulepszenie przy użyciu przetwarzania asynchronicznego strony ASP.NET 4.5.

Metody asynchroniczne w aplikacjach sieci web można zoptymalizować sposób, w jaki są używane w puli wątków programu ASP.NET. W programie ASP.NET: są wnioski o ograniczonej liczbie wątków w puli wątków dla uczestnictwa, w związku z tym, gdy wszystkie wątki są zajęte, ASP.NET zaczyna odrzucać żądania nowego, wysyła komunikaty o błędach aplikacji i sprawia, że witryna jest niedostępny.

Czas operacji w witrynie sieci web są doskonałymi kandydatami dla programowania asynchronicznego, ponieważ zajmują przypisany wątek przez długi czas. Obejmuje to długo wykonywanych żądań z dużą liczbą różnych elementów i stron, które wymagają operacji w trybie offline, takich zapytań bazy danych lub uzyskiwania dostępu do zewnętrznego serwera sieci web. Zaletą jest to, że jeśli dla tych operacji można używać metod asynchronicznych, podczas przetwarzania strony wątek jest zwolniony i zwrócony do wątku puli i może służyć do udziału na nowe żądanie strony. Oznacza to strona rozpocznie przetwarzanie w jednym wątku z puli wątków i może zakończyć przetwarzanie w innym, po zakończeniu przetwarzania asynchronicznego.

1. Otwórz **ProductDetails.aspx** strony. Dodaj **Async** atrybutu w **strony** element i ustaw ją na **true**. Ten atrybut informuje platformę ASP.NET w celu implementacji interfejsu IHttpAsyncHandler.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Dodaj etykietę w dolnej części strony Aby wyświetlić jego szczegóły wątki uruchomione na stronie.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Otwórz **ProductDetails.aspx.cs** i dodaj następujące dyrektywy przestrzeni nazw.

    (Code Snippet — *sieci Web formularzy laboratorium - Ex03 - nazw 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Modyfikowanie **UpdateProductImage** metody do pobierania obrazu o zadaniu asynchronicznym. Spowoduje zastąpienie **WebClient** **DownloadFile** metody z **DownloadFileTaskAsync** metody i obejmują **await** — słowo kluczowe.

    (Code Snippet — *sieci Web formularzy laboratorium - Ex03 - UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    RegisterAsyncTask rejestruje nowe zadanie asynchroniczne strony do wykonania w innym wątku. Wyrażenie lambda z zadań (t) do wykonania, odbiera. **Await** — słowo kluczowe w **DownloadFileTaskAsync** metoda konwertuje pozostałą część metody wywołania zwrotnego, które jest wywoływane asynchronicznie po **DownloadFileTaskAsync** metoda została zakończona. ASP.NET zostanie wznowiona wykonywanie metody automatycznie utrzymując wszystkie żądania oryginalnej wartości HTTP. Nowy model programowania asynchronicznego w .NET 4.5 można napisać kod asynchroniczny, który wygląda bardzo podobnie synchroniczny kod i pozwolić kompilatorowi obsługi kompilacji funkcji wywołania zwrotnego lub kod kontynuacji.
    > [!NOTE]
    > RegisterAsyncTask i PageAsyncTask były dostępne już od .NET 2.0. Await — słowo kluczowe jest nowa w .NET 4.5 asynchronicznego modelu programowania i może być używane razem z nowych metod TaskAsync z obiektu .NET WebClient.
5. Dodaj kod, aby wyświetlić wątki, na których kod uruchomienia i zakończenia. Aby to zrobić, należy zaktualizować **UpdateProductImage** metoda następującym kodem.

    (Code Snippet — *wątków Web Forms Lab - Ex03 - Show*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Otwórz witrynę sieci web **Web.config** pliku. Dodaj następującą zmienną appSetting.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Naciśnij klawisz **F5** do uruchamiania aplikacji i przekazać obraz dla produktu. Zwróć uwagę na identyfikator wątków, w których kod rozpoczęcia i zakończenia mogą być różne. Jest to spowodowane asynchronicznej zadania są uruchamiane w oddzielnym wątku z puli wątków programu ASP.NET. Po zakończeniu zadania, ASP.NET umieszcza zadania w kolejce i przypisuje żadnego z dostępnych wątków.

    ![Pobieranie obrazu asynchronicznie](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "asynchronicznie pobieranie obrazu")

    *Pobieranie obrazu asynchronicznie*

> [!NOTE]
> Ponadto można wdrożyć tę aplikację do platformy Azure następujące [dodatek B: publikowania aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym laboratorium praktycznego zostały rozwiązane i przedstawiono następujące pojęcia:

- Użyj wyrażenia wiązania danych silnie typizowane
- Korzystać z nowych funkcji wiązania modelu w formularzach sieci Web
- Korzystanie z dostawców wartości do mapowania danych ze strony metody związanym z kodem
- Adnotacje danych na użytek walidacji danych wejściowych użytkownika
- Wykonaj advange unobstrusive weryfikacji po stronie klienta przy użyciu jQuery w formularzach sieci Web
- Implementowanie weryfikacji żądań szczegółowe
- Implementowanie strony asynchronicznego przetwarzania w formularzach sieci Web

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Możesz zainstalować **programu Microsoft Visual Studio Express 2012 for Web** lub inne &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe wskazówki ułatwiają kroki wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>Visual Studio Express 2012 for Web z zestawem Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Zainstaluj program Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "zainstalować program Visual Studio Express")

    *Zainstaluj program Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Akceptowanie umowy licencyjnej*
5. Zaczekaj, aż do zakończenia procesu pobierania i instalacji.

    ![Postęp instalacji](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu, a następnie zacznij pisać &quot; **VS Express**&quot;, następnie kliknij pozycję **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *VS Express for Web kafelka*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek B: publikowania aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy

Ten dodatek będzie pokazują, jak utworzyć nową witrynę sieci web w witrynie Azure Portal i publikowanie aplikacji, uzyskany postępując zgodnie z laboratorium, korzystając z zalet funkcji publikowania narzędzia Web Deploy udostępnianych przez platformę Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Zadanie 1. Tworzenie nowej witryny sieci Web w witrynie Azure Portal

1. Przejdź do [portalu zarządzania systemu Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft, powiązaną z Twoją subskrypcją.

    > [!NOTE]
    > Za pomocą platformy Azure można bezpłatny hosting 10 witryn sieci Web platformy ASP.NET i skalowanie w miarę wzrostu ruchu. Możesz zarejestrować się [tutaj](http://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do portalu usługi Windows Azure](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Zaloguj się do portalu usługi Windows Azure")

    *Zaloguj się do portalu*
2. Kliknij przycisk **New** na pasku poleceń.

    ![Tworzenie nowej witryny sieci Web](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "tworzenia nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij przycisk **obliczenia** | **witryny sieci Web**. Następnie wybierz pozycję **szybkie tworzenie** opcji. Podaj adres URL dostępny dla nowej witryny sieci web, a następnie kliknij przycisk **Utwórz witrynę sieci Web**.

    > [!NOTE]
    > Platforma Azure to hosta dla aplikacji sieci web działające w chmurze, które można kontrolować i zarządzać nimi. Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web na platformie Azure z poza portalem. Nie obejmuje kroki konfigurowania bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu szybkiego tworzenia](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")

    *Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*
4. Poczekaj, aż nowe **witryny sieci Web** zostanie utworzony.
5. Po utworzeniu witryny sieci Web kliknij link w obszarze **adresu URL** kolumny. Sprawdź, czy działa nową witrynę sieci Web.

    ![Przejście do nowej witryny sieci web](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "przejście do nowej witryny sieci web")

    *Przejście do nowej witryny sieci web*

    ![Witryna sieci Web działa](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "witryna sieci Web działa")

    *Witryna sieci Web działa*
6. Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.

    ![Otwieranie strony zarządzania witryny sieci web](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "otwieranie stron zarządzania witryny sieci web")

    *Otwieranie strony zarządzania witryny sieci Web*
7. W **pulpit nawigacyjny** w obszarze **Przegląd** kliknij **Pobierz profil publikowania** łącza.

    > [!NOTE]
    > *Profil publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web na platformie Azure dla poszczególnych metod włączone publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do łączenia się i uwierzytelnianie w odniesieniu do każdego z punktów końcowych, dla których włączono metoda publikacji. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** Obsługa odczytywania profili publikowania do automatyzowania konfiguracji tych programów dla Publikowanie aplikacji sieci web na platformie Azure.

    ![Pobieranie witryny sieci web profil publikowania](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "pobierania witryny sieci web profilu publikowania")

    *Pobieranie witryny sieci Web profilu publikowania*
8. Pobierz plik profilu publikowania w znanej lokalizacji. Dodatkowo w tym ćwiczeniu zostanie zobaczysz, jak publikować aplikację sieci web na platformie Azure z programu Visual Studio za pomocą tego pliku.

    ![Zapisywanie pliku profilu publikowania](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z programu SQL Server baz danych, musisz utworzyć serwer bazy danych SQL. Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.

1. Należy serwera usługi SQL Database do przechowywania bazy danych aplikacji. Możesz wyświetlić serwery baz danych SQL z subskrypcji w portalu zarządzania platformy Azure w **baz danych Sql** | **serwerów** | **pulpitu nawigacyjnego serwera**. Jeśli nie masz serwer, który został utworzony, można utworzyć ją przy użyciu **Dodaj** przycisk na pasku poleceń. Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, ponieważ będziesz ich używać w kolejne zadania podrzędne. Nie należy tworzyć bazy danych, ponieważ zostanie on utworzony w późniejszym terminie.

    ![Pulpit nawigacyjny z serwera bazy danych SQL](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "pulpitu nawigacyjnego serwera bazy danych SQL")

    *Pulpit nawigacyjny z serwera bazy danych SQL*
2. W ramach następnego zadania spowoduje przetestowanie połączenia z bazą danych z programu Visual Studio, dlatego trzeba uwzględnić adres IP lokalnego serwera liście **dozwolone adresy IP**. Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżący adres IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) przycisku.

    ![Dodawanie adresu IP klienta](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Dodawanie adresu IP klienta*
3. Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP listy, kliknij pozycję **Zapisz** aby potwierdzić zmiany.

    ![Potwierdź zmiany](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Potwierdź zmiany*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 — Publikowanie aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **Publikuj**.

    ![Publikowanie aplikacji](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "publikowania aplikacji")

    *Publikowanie witryny sieci web*
2. Importuj profil publikowania, zapisaną w pierwszym zadaniem.

    ![Trwa importowanie profilu publikowania](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "importowania profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij przycisk **sprawdzanie poprawności połączenia**. Po zakończeniu sprawdzania kliknij **dalej**.

    > [!NOTE]
    > Sprawdzanie poprawności zostało ukończone, gdy zostanie wyświetlony z zielonym znacznikiem wyboru pojawiają się obok przycisku Waliduj połączenie.

    ![Sprawdzanie poprawności połączenia](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "sprawdzanie poprawności połączenia")

    *Sprawdzanie poprawności połączenia*
4. W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk obok pola tekstowego połączenia bazy danych (czyli **DefaultConnection**).

    ![Konfiguracja narzędzia Web deploy](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Konfiguracja narzędzia Web deploy")

    *Konfiguracja narzędzia Web deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W **nazwy serwera** wpisz swoją bazę danych SQL server adresu URL przy użyciu *tcp:* prefiks.
   - W **nazwa_użytkownika** wpisz nazwę logowania administratora serwera.
   - W **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nazwę nowej bazy danych.

     ![Konfigurowanie parametrów połączenia z docelowym](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "Konfigurowanie parametrów połączenia z docelowym")

     *Konfigurowanie parametrów połączenia z docelowym*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu, aby utworzyć kliknij bazę danych **tak**.

    ![Tworzenie bazy danych](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "Tworzenie parametrów bazy danych")

    *Tworzenie bazy danych*
7. Parametry połączenia, które będą używane do łączenia z bazą danych SQL na platformie Azure jest wyświetlana w polu tekstowym połączenia domyślne. Następnie kliknij przycisk **Dalej**.

    ![Ciąg połączenia wskazujący bazę danych SQL](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "ciąg połączenia wskazujący bazę danych SQL")

    *Ciąg połączenia wskazujący bazę danych SQL*
8. W **Podgląd** kliknij **Publikuj**.

    ![Publikowanie aplikacji sieci web](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "publikowania aplikacji sieci web")

    *Publikowanie aplikacji sieci web*
9. Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Dodatek C: korzystania z fragmentów kodu

Za pomocą wstawek kodu masz kod, czego potrzebujesz w zasięgu ręki. Dokument laboratorium informuje o dokładnie podczas korzystania z nich, jak pokazano na poniższej ilustracji.

![Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "fragmenty kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu*

***Aby dodać wstawki kodu za pomocą klawiatury (tylko język C#)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Rozpocznij wpisywanie nazwy fragmentu kodu (bez spacji i łączniki).
3. Obejrzyj, jak technologia IntelliSense wyświetla zgodne z nazwami fragmenty kodu.
4. Wybierz prawidłowy fragment kodu lub pisz dalej do momentu wybrania debugowanej nazwy całego fragmentu kodu.
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

![Rozpocznij wpisywanie nazwy fragmentu kodu](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "Rozpocznij wpisywanie nazwy fragmentu kodu")

*Rozpocznij wpisywanie nazwy fragmentu kodu*

![Naciśnij klawisz Tab, aby zaznaczyć fragment wyróżnione](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragmentu kodu")

*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

![Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "rozwinie ponownie naciśnij klawisz Tab i fragment kodu")

*Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte.*

***Aby dodać wstawki kodu za pomocą myszy (C#, Visual Basic i XML)*** 1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.

1. Wybierz **Wstaw fragment kodu** następuje **Moje fragmenty kodu**.
2. Wybierz odpowiedni fragment z listy, klikając go.

![Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu")

*Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu*

![Wybierz odpowiedni fragment z listy, klikając go](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "wybierz odpowiedni fragment z listy, klikając go")

*Wybierz odpowiedni fragment z listy, klikając go*
