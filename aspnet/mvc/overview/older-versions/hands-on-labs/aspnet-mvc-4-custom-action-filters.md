---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtry akcji niestandardowych platformy ASP.NET MVC 4 | Dokumentacja firmy Microsoft
author: rick-anderson
description: ASP.NET MVC zawiera filtry akcji do wykonania logikę filtrowania przed lub po nosi nazwę metody akcji. Filtry akcji są tha atrybuty niestandardowe...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: c435fef624d526ceb01dbc370c5df52e2a1e8350
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807938"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>Platforma ASP.NET MVC 4 — niestandardowe filtry akcji

Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)

[Pobierz Camp Web szkolenia Kit](https://aka.ms/webcamps-training-kit)

ASP.NET MVC zawiera filtry akcji do wykonania logikę filtrowania przed lub po nosi nazwę metody akcji. Filtry akcji są atrybutów niestandardowych, które zapewniają deklaracyjne oznacza, że można dodać zachowanie akcja przed i po działaniu do metody akcji kontrolera.

W tym warsztatów utworzysz atrybutu filtru akcji niestandardowej w rozwiązaniu MvcMusicStore dziennika aktywności lokację do tabeli bazy danych i przechwytywać żądań kontrolera. Można dodać filtr rejestrowania przez iniekcję do dowolnego kontrolera lub akcji. Na koniec zostanie wyświetlony widok dziennika, który wyświetla listę użytkowników.

W tym laboratorium praktyczne przyjęto założenie, masz podstawową wiedzę na temat **platformy ASP.NET MVC**. Jeśli nie używasz **platformy ASP.NET MVC** , zalecamy zapoznać się z **podstawowe informacje dotyczące programu ASP.NET MVC 4** warsztatów.

> [!NOTE]
> Wszystkie przykładowy kod i fragmenty są uwzględnione w sieci Web Camp zestaw szkoleniowy, pod [wersji Microsoft — w sieci Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specyficzne dla tego laboratorium znajduje się w temacie [filtry akcji programu ASP.NET MVC 4 niestandardowe](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym laboratorium praktyczne dowiesz się jak:

- Tworzenie akcji niestandardowej atrybutu filtru, aby rozszerzyć możliwości filtrowania
- Zastosuj atrybut niestandardowy filtr przez iniekcję do określonego poziomu
- Zarejestruj filtry akcji niestandardowej globalnie

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Należy dysponować następującymi elementami do przygotowania tego laboratorium:

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczyt [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

**Instalowanie fragmentów kodu**

Dla wygody większość kodu, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako fragmenty kodu programu Visual Studio. Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.

Jeśli nie jesteś zaznajomiony z fragmentów kodu w usłudze Visual Studio i chcesz dowiedzieć się, jak z nich korzystać, możesz zapoznać się z dodatku z tego dokumentu &quot; [dodatek C: za pomocą wstawek kodu programu](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

W tym laboratorium praktyczne składa się przez następujących czynnościach:

1. [Ćwiczenie 1: Rejestrowanie akcji](#Exercise1)
2. [Ćwiczenie 2: Zarządzanie wieloma filtry akcji](#Exercise2)

Szacowany czas do ukończenia tego laboratorium: **30 minut**.

> [!NOTE]
> Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązania, należy uzyskać po ukończeniu ćwiczenia. Jeśli potrzebujesz dodatkowej pomocy ćwiczeń opisanych w dalszej, można użyć tego rozwiązania jako wskazówki.


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Ćwiczenie 1: Rejestrowanie akcji

W tym ćwiczeniu dowiesz się, jak utworzyć filtr dziennika akcji niestandardowej za pomocą programu ASP.NET MVC 4 dostawców filtrów. W tym celu filtr rejestrowania będą dotyczyć witryny MusicStore, które będą rejestrowane wszystkie działania w wybrane kontrolerów.

Filtr będzie rozszerzać **ActionFilterAttributeClass** i zastąpić **OnActionExecuting** metodę, aby przechwycić każdego żądania, a następnie wykonać akcje rejestrowania. Informacje o kontekście żądania HTTP, wykonywanie metod, wyniki i parametry będzie świadczona przez platformy ASP.NET MVC **ActionExecutingContext** klasy **.**

> [!NOTE]
> ASP.NET MVC 4 ma również domyślnych dostawców filtrów, których można używać bez konieczności tworzenia niestandardowego filtru. ASP.NET MVC 4 zawiera następujące typy filtrów:
> 
> - **Autoryzacja** filtrowania, co sprawia, że decyzje dotyczące bezpieczeństwa o tym, czy można wykonać metody akcji, takich jak wykonanie uwierzytelniania lub sprawdzania poprawności właściwości żądania.
> - **Akcja** filtr, który opakowuje wykonanie metody akcji. Ten filtr, można wykonać dodatkowe przetwarzania, takich jak dostarcza dodatkowe dane do metody akcji, kontroli wartość zwracaną lub który anulował wykonanie metody akcji
> - **Wynik** filtr, który opakowuje wykonanie obiektu ActionResult. Ten filtr, można wykonać dodatkowego przetwarzania wyniku, takie jak modyfikowanie odpowiedzi HTTP.
> - **Wyjątek** filtr, który jest wykonywany, jeśli ma nieobsługiwany wyjątek spowodowany gdzieś w metodzie akcji, począwszy od filtry autoryzacji, a kończąc na wykonanie wynik. Filtry wyjątków może służyć do zadań, takich jak rejestrowanie lub wyświetlanie strony błędu.
> 
> Aby uzyskać więcej informacji o dostawcach filtrów można znaleźć ten link MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>Funkcja rejestrowania aplikacji programu MVC Music Store — informacje

To rozwiązanie Music Store ma nowej tabeli modelu danych do rejestrowania witryny **ActionLog**, przy użyciu następujących pól: Nazwa kontrolera, który odebrał żądanie, wywoływana akcja, adres IP klienta i sygnaturę czasową.

![Model danych. Tabela ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modelu danych. Tabela ActionLog.")

*Model danych — ActionLog tabeli*

Rozwiązanie udostępnia widok ASP.NET MVC dla dziennika akcji, która znajduje się w temacie **MvcMusicStores/widoków/ActionLog**:

![Widok dziennika akcji](aspnet-mvc-4-custom-action-filters/_static/image2.png "widok dziennika akcji")

*Widok dziennika akcji*

Dzięki temu podane struktury całą pracę koncentrować się na przerywania kontrolera żądań i wykonywania rejestrowanie za pomocą niestandardowych filtrowania.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Zadanie 1. Tworzenie niestandardowych filtr, aby przechwytywać żądania poziomu kontrolera

W tym zadaniu utworzysz klasę atrybutów niestandardowego filtru, która będzie zawierać logiki rejestrowania. W tym celu możesz rozszerzyć platformy ASP.NET MVC **ActionFilterAttribute** klasy i zaimplementuj interfejs **IActionFilter**.

> [!NOTE]
> **ActionFilterAttribute** jest klasą bazową dla wszystkich filtrów atrybutu. Oferuje ono następujące metody umożliwiające wykonywanie logiki określonych po, a także przed wykonaniem akcji kontrolera:
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): tuż przed akcji jest wywoływana metoda.
> - **OnActionExecuted**(ActionExecutedContext filterContext): po wywołaniu metody akcji i zanim wynik zostanie wykonany (przed renderowania widoku).
> - **OnResultExecuting**(ResultExecutingContext filterContext): tak, zanim wynik zostanie wykonany (przed renderowania widoku).
> - **OnResultExecuted**(ResultExecutedContext filterContext): po wykonaniu wyniku (po renderowania widoku).
> 
> Przez zastąpienie dowolnego z tych metod w klasie pochodnej, można wykonać filtrowania kodu.


1. Otwórz **rozpocząć** rozwiązania znajdujący się w **\Source\Ex01-LoggingActions\Begin** folderu.

   1. Należy pobrać niektóre brakujące pakiety NuGet, przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
      > 
      > Aby uzyskać więcej informacji znajduje się w artykule: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Dodaj nową klasę C# do **filtry** folder i nadaj mu nazwę *CustomActionFilter.cs*. Ten folder będzie przechowywać wszystkie filtry niestandardowe.
3. Otwórz **CustomActionFilter.cs** i Dodaj odwołanie do **System.Web.Mvc** i **MvcMusicStore.Models** przestrzeni nazw:

    (Code Snippet — *filtry akcji niestandardowej platformy ASP.NET MVC 4 — Ex1 CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Dziedziczenie **CustomActionFilter** klasy z **ActionFilterAttribute** , a następnie **CustomActionFilter** implementacji klasy **IActionFilter** interfejsu.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Wprowadź **CustomActionFilter** klasie zastąpić metodę **OnActionExecuting** i dodać logikę potrzebną do dziennika wykonywania filtru. Aby to zrobić, Dodaj następujący wyróżniony kod w ramach **CustomActionFilter** klasy.

    (Code Snippet — *filtry akcji niestandardowej platformy ASP.NET MVC 4 — Ex1 LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting** metoda używa **Entity Framework** można dodać nowego rejestru ActionLog. Tworzy i wypełnia nowe wystąpienie jednostki informacje o kontekście z **filterContext**.
    > 
    > Przeczytaj więcej o **kontekstem ControllerContext** klasy na [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Zadanie 2 — wprowadzanie Interceptor kod do klasy kontrolera Store

W tym zadaniu należy dodać filtr niestandardowy przez iniekcję go do wszystkich klas kontrolera i akcji kontrolera, które będą rejestrowane. Na potrzeby tego ćwiczenia klasy kontrolera Store będzie prowadzić dziennik.

Metoda **OnActionExecuting** z **ActionLogFilterAttribute** filtr niestandardowy jest uruchamiany, gdy element wprowadzonego jest wywoływana.

Istnieje również możliwość przechwytuje metodę określonego kontrolera.

1. Otwórz **StoreController** na **MvcMusicStore\Controllers** i Dodaj odwołanie do **filtry** przestrzeni nazw:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Filtr niestandardowy wstrzyknąć **CustomActionFilter** do **StoreController** klasy, dodając **[CustomActionFilter]** atrybutu przed deklaracją klasy.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Jeśli filtr są wstrzykiwane do klasy kontrolera, jego akcje również są wprowadzane. Jeśli chcesz zastosować filtr tylko dla działań, trzeba będzie wstawić **[CustomActionFilter]** do każdego z nich z nich:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3. uruchamianie aplikacji

W tym zadaniu przetestujesz działa filtr rejestrowania. Należy uruchomić aplikację i odwiedź Sklep, a następnie będzie sprawdzać zarejestrowanych działań.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Przejdź do **/ActionLog** aby zobaczyć stan początkowy widok dziennika:

    ![Rejestrowanie śledzenia stanu przed działaniem strony](aspnet-mvc-4-custom-action-filters/_static/image3.png "rejestrowania śledzenia stanu przed na pager")

    *Dziennika śledzenia stanu przed na pager*

   > [!NOTE]
   > Domyślnie będzie zawsze wyświetlany jeden element, który jest generowany podczas pobierania istniejących gatunki menu.
   > 
   > Dla uproszczenia czyścimy **ActionLog** tabeli przy każdym uruchomieniu aplikacji, widoczna będzie tylko dzienniki weryfikacji każdego określonego zadania.
   > 
   > Może być konieczne usunięcie następujący kod z **sesji\_Start** — metoda (w **Global.asax** klasy), aby zapisać dziennik historyczny akcje wykonywane w ramach Store Kontroler.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Kliknij jedną z **gatunki** z menu i wykonywać niektórych akcji, takich jak przeglądanie dostępnych albumu.
4. Przejdź do **/ActionLog** i jeśli dziennik jest pusty naciśnij **F5** Aby odświeżyć stronę. Sprawdź, czy Twoje odwiedziny były śledzone:

    ![Dziennik akcji za pomocą działania rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image4.png "dziennik akcji za pomocą działania rejestrowane")

    *Dziennik akcji za pomocą działania rejestrowane*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Ćwiczenie 2: Zarządzanie wieloma filtry akcji

W tym ćwiczeniu możesz dodać drugi niestandardowy filtr akcji do klasy StoreController i zdefiniuj określonej kolejności, w którym będą wykonywane obu filtrów. Następnie zmodyfikujemy kod, aby zarejestrować filtr globalny.

Istnieją różne opcje, aby wziąć pod uwagę podczas definiowania kolejność wykonywania filtrów. Na przykład właściwość kolejności i te filtry zakresu:

Można zdefiniować **zakres** dla każdego z filtrów, na przykład przykład możesz ograniczyć zakres wszystkie filtry akcji do działania w ramach **zakresem kontrolera**i wszystkie filtry autoryzacji do uruchamiania w **zakresie globalnym** . Zakresy mają kolejność wykonywania zdefiniowane.

Ponadto każdy filtr akcji ma właściwość zlecenia służy do określania kolejności wykonywania w zakresie filtru.

Więcej informacji na temat kolejność wykonywania filtrów akcji niestandardowych można znaleźć w artykule MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Zadanie 1: Tworzenie nowego niestandardowy filtr akcji

W tym zadaniu utworzysz nowy niestandardowy filtr akcji można wstawić do klasy StoreController, jak zarządzać kolejność wykonywania filtrów.

1. Otwórz **rozpocząć** rozwiązania znajdujący się w **\Source\Ex02-ManagingMultipleActionFilters\Begin** folderu. W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.

    1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
    2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
    3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

        > [!NOTE]
        > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
        > 
        > Aby uzyskać więcej informacji znajduje się w artykule: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Dodaj nową klasę C# do **filtry** folder i nadaj mu nazwę *MyNewCustomActionFilter.cs*
3. Otwórz **MyNewCustomActionFilter.cs** i Dodaj odwołanie do **System.Web.Mvc** i **MvcMusicStore.Models** przestrzeni nazw:

    (Code Snippet — *filtry akcji niestandardowej platformy ASP.NET MVC 4 — Ex2 MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Zastąp domyślną deklaracji klasy z następującym kodem.

    (Code Snippet — *filtry akcji niestandardowej platformy ASP.NET MVC 4 — Ex2 MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Ten niestandardowy filtr akcji jest prawie taki sam, niż ten, który został utworzony w poprzednim ćwiczeniu. Główną różnicą jest to, że ma on *&quot;rejestrowane przez&quot;* zaktualizowany o nazwie tej nowej klasy, aby zidentyfikować filtr wystąpiły atrybut zarejestrowany dziennika.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Zadanie 2: Wprowadzanie nowych Interceptor kod do klasy StoreController

W tym zadaniu zostanie dodaje nowy filtr niestandardowy do klasy StoreController i uruchomić rozwiązanie, aby sprawdzić, jak w przypadku obu filtrów współpracują ze sobą.

1. Otwórz **StoreController** klasy znajdujący się w **MvcMusicStore\Controllers** i wstawić nowy filtr niestandardowy **MyNewCustomActionFilter** do  **StoreController** klasy, jak przedstawiono w poniższym kodzie.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. Teraz uruchom aplikację, aby zobaczyć, jak działają te dwa filtry akcji niestandardowych. Aby to zrobić, naciśnij klawisz **F5** i zaczekaj, aż uruchamiania aplikacji.
3. Przejdź do **/ActionLog** aby zobaczyć stan początkowy widok dziennika.

    ![Rejestrowanie śledzenia stanu przed działaniem strony](aspnet-mvc-4-custom-action-filters/_static/image5.png "rejestrowania śledzenia stanu przed na pager")

    *Dziennika śledzenia stanu przed na pager*
4. Kliknij jedną z **gatunki** z menu i wykonywać niektórych akcji, takich jak przeglądanie dostępnych albumu.
5. Sprawdź, czy ten czas; wizyty były śledzone dwa razy: po każdej niestandardowe filtry akcji dodane w **kontroler magazynu** klasy.

    ![Dziennik akcji za pomocą działania rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image6.png "dziennik akcji za pomocą działania rejestrowane")

    *Dziennik akcji za pomocą działania rejestrowane*
6. Zamknij przeglądarkę.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Zadanie 3: Zarządzanie, kolejności filtru

W tym zadaniu dowiesz się, jak zarządzać kolejność wykonywania filtrów przy użyciu podawany zamówienia.

1. Otwórz **StoreController** klasy znajdujący się w **MvcMusicStore\Controllers** i określ **kolejności** właściwości w obu filtrów, takich jak pokazano poniżej.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. Teraz Sprawdź, jak filtry są wykonywane w zależności od wartości właściwości jego zamówienia. Będzie zauważysz, że filtr mający najmniejszą wartość kolejności (**CustomActionFilter**) jest to pierwsza, który jest wykonywany. Naciśnij klawisz **F5** i zaczekaj, aż uruchamiania aplikacji.
3. Przejdź do **/ActionLog** aby zobaczyć stan początkowy widok dziennika.

    ![Rejestrowanie śledzenia stanu przed działaniem strony](aspnet-mvc-4-custom-action-filters/_static/image7.png "rejestrowania śledzenia stanu przed na pager")

    *Dziennika śledzenia stanu przed na pager*
4. Kliknij jedną z **gatunki** z menu i wykonywać niektórych akcji, takich jak przeglądanie dostępnych albumu.
5. Sprawdź, czy ten czas wizyty były śledzone uporządkowane według wartości kolejności filtrów: **CustomActionFilter** dzienniki pierwszy.

    ![Dziennik akcji za pomocą działania rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image8.png "dziennik akcji za pomocą działania rejestrowane")

    *Dziennik akcji za pomocą działania rejestrowane*
6. Będzie teraz zaktualizować wartość kolejności filtrów i sprawdź, jak zmienia kolejność rejestrowania. W **StoreController** klasy, zaktualizuj wartość kolejności filtrów, jak pokazano poniżej.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Uruchom aplikację ponownie, naciskając klawisz **F5**.
8. Kliknij jedną z **gatunki** z menu i wykonywać niektórych akcji, takich jak przeglądanie dostępnych albumu.
9. Sprawdź, czy ten czas dzienniki utworzone w wyniku **MyNewCustomActionFilter** występuje jako pierwszy filtr.

    ![Dziennik akcji za pomocą działania rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image9.png "dziennik akcji za pomocą działania rejestrowane")

    *Dziennik akcji za pomocą działania rejestrowane*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Zadanie 4: Rejestrowanie filtry globalnie

To zadanie zaktualizuje rozwiązanie, aby zarejestrować nowy filtr (**MyNewCustomActionFilter**) jako filtrów globalnych. W ten sposób zostanie wyzwolone przez wszystkie zestawem akcji w aplikacji i nie tylko w StoreController z nich, tak jak w poprzednim zadaniu.

1. W **StoreController** klasy, należy usunąć **[MyNewCustomActionFilter]** atrybut i właściwość kolejności **[CustomActionFilter]**. Jego powinien wyglądać następująco:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Otwórz **Global.asax** pliku, a następnie zlokalizuj **aplikacji\_Start** metody. Zwróć uwagę, że każdorazowo aplikacja uruchamia go rejestruje filtry globalne, wywołując **RegisterGlobalFilters** metodę w ramach **FilterConfig** klasy.

    ![Rejestrowanie filtry globalne w pliku Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "rejestrowanie filtry globalne w pliku Global.asax")

    *Rejestrowanie filtry globalne w pliku Global.asax*
3. Otwórz **FilterConfig.cs** plików w ramach **aplikacji\_Start** folderu.
4. Dodaj odwołanie do przy użyciu System.Web.Mvc; za pomocą MvcMusicStore.Filters; przestrzeń nazw.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Aktualizacja **RegisterGlobalFilters** metody dodawania niestandardowego filtru. Aby to zrobić, Dodaj wyróżniony kod:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Uruchom aplikację, naciskając klawisz **F5**.
7. Kliknij jedną z **gatunki** z menu i wykonywać niektórych akcji, takich jak przeglądanie dostępnych albumu.
8. Sprawdź, że teraz **[MyNewCustomActionFilter]** jest on zbyt wprowadzony w HomeController i ActionLogController.

    ![Dziennik akcji za pomocą działania rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image11.png "dziennik akcji za pomocą działania rejestrowane")

    *Dziennik akcji za pomocą działania globalnego rejestrowane*

> [!NOTE]
> Ponadto można wdrożyć tę aplikację do witryny sieci Web systemu Windows Azure następujące [dodatek B: publikowania aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Przez ukończenie tego laboratorium praktyczne mają pokazaliśmy, jak rozszerzyć filtr akcji do wykonania akcji niestandardowej. Ma również pokazaliśmy, jak wprowadzić dowolny filtr z kontrolerami strony. Były używane następujące pojęcia:

- Jak utworzyć filtry akcji niestandardowej z klasy ASP.NET MVC ActionFilterAttribute
- Jak wstawić filtry do kontrolery ASP.NET MVC
- Jak zarządzać filtru kolejność przy użyciu właściwości Order
- Jak zarejestrować filtry globalnie

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Możesz zainstalować **programu Microsoft Visual Studio Express 2012 for Web** lub inne &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe wskazówki ułatwiają kroki wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Zainstaluj program Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "zainstalować program Visual Studio Express")

    *Zainstaluj program Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Akceptowanie umowy licencyjnej*
5. Zaczekaj, aż do zakończenia procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu, a następnie zacznij pisać &quot; **VS Express**&quot;, następnie kliknij pozycję **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    *VS Express for Web kafelka*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek B: publikowania aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy

Ten dodatek będzie pokazują, jak utworzyć nową witrynę sieci web w portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskany postępując zgodnie z laboratorium, korzystając z zalet funkcji publikowania narzędzia Web Deploy oferowanego przez system Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Zadanie 1. Tworzenie nowej witryny sieci Web z Windows Azure Portal

1. Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft, powiązaną z Twoją subskrypcją.

    > [!NOTE]
    > Platforma Windows Azure można bezpłatny hosting 10 witryn sieci Web platformy ASP.NET i skalowanie w miarę wzrostu ruchu. Możesz zarejestrować się [tutaj](http://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do portalu usługi Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "Zaloguj się do portalu usługi Windows Azure")

    *Zaloguj się do portalu zarządzania systemu Azure Windows*
2. Kliknij przycisk **New** na pasku poleceń.

    ![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "tworzenia nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij przycisk **obliczenia** | **witryny sieci Web**. Następnie wybierz pozycję **szybkie tworzenie** opcji. Podaj adres URL dostępny dla nowej witryny sieci web, a następnie kliknij przycisk **Utwórz witrynę sieci Web**.

    > [!NOTE]
    > Witryny sieci Web Windows Azure jest hostem dla aplikacji sieci web działające w chmurze, które można kontrolować i zarządzać nimi. Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do Windows Azure witryny sieci Web z poza portalem. Nie obejmuje kroki konfigurowania bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu szybkiego tworzenia](aspnet-mvc-4-custom-action-filters/_static/image19.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")

    *Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*
4. Poczekaj, aż nowe **witryny sieci Web** zostanie utworzony.
5. Po utworzeniu witryny sieci Web kliknij link w obszarze **adresu URL** kolumny. Sprawdź, czy działa nową witrynę sieci Web.

    ![Przejście do nowej witryny sieci web](aspnet-mvc-4-custom-action-filters/_static/image20.png "przejście do nowej witryny sieci web")

    *Przejście do nowej witryny sieci web*

    ![Witryna sieci Web działa](aspnet-mvc-4-custom-action-filters/_static/image21.png "witryna sieci Web działa")

    *Witryna sieci Web działa*
6. Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.

    ![Otwieranie strony zarządzania witryny sieci web](aspnet-mvc-4-custom-action-filters/_static/image22.png "otwieranie stron zarządzania witryny sieci web")

    *Otwieranie strony zarządzania witryny sieci Web*
7. W **pulpit nawigacyjny** w obszarze **Przegląd** kliknij **Pobierz profil publikowania** łącza.

    > [!NOTE]
    > *Profil publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web w witrynie sieci Web usługi Windows Azure dla poszczególnych metod włączone publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do łączenia się i uwierzytelnianie w odniesieniu do każdego z punktów końcowych, dla których włączono metoda publikacji. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** Obsługa odczytywania profili publikowania do automatyzowania konfiguracji tych programów dla Publikowanie aplikacji sieci web w usłudze Windows Azure websites.

    ![Pobieranie witryny sieci web profil publikowania](aspnet-mvc-4-custom-action-filters/_static/image23.png "pobierania witryny sieci web profilu publikowania")

    *Pobieranie witryny sieci Web profilu publikowania*
8. Pobierz plik profilu publikowania w znanej lokalizacji. Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do Windows Azure Web Sites z programu Visual Studio za pomocą tego pliku.

    ![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-custom-action-filters/_static/image24.png "zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z programu SQL Server baz danych, musisz utworzyć serwer bazy danych SQL. Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.

1. Należy serwera usługi SQL Database do przechowywania bazy danych aplikacji. Możesz wyświetlić serwery baz danych SQL z subskrypcji w portalu zarządzania Azure Windows pod **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**. Jeśli nie masz serwer, który został utworzony, można utworzyć ją przy użyciu **Dodaj** przycisk na pasku poleceń. Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, ponieważ będziesz ich używać w kolejne zadania podrzędne. Nie należy tworzyć bazy danych, ponieważ zostanie on utworzony w późniejszym terminie.

    ![Pulpit nawigacyjny z serwera bazy danych SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "pulpitu nawigacyjnego serwera bazy danych SQL")

    *Pulpit nawigacyjny z serwera bazy danych SQL*
2. W ramach następnego zadania spowoduje przetestowanie połączenia z bazą danych z programu Visual Studio, dlatego trzeba uwzględnić adres IP lokalnego serwera liście **dozwolone adresy IP**. Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżący adres IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) przycisku.

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Dodawanie adresu IP klienta*
3. Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP listy, kliknij pozycję **Zapisz** aby potwierdzić zmiany.

    ![Potwierdź zmiany](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Potwierdź zmiany*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 — Publikowanie aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **Publikuj**.

    ![Publikowanie aplikacji](aspnet-mvc-4-custom-action-filters/_static/image29.png "publikowania aplikacji")

    *Publikowanie witryny sieci web*
2. Importuj profil publikowania, zapisaną w pierwszym zadaniem.

    ![Trwa importowanie profilu publikowania](aspnet-mvc-4-custom-action-filters/_static/image30.png "importowania profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij przycisk **sprawdzanie poprawności połączenia**. Po zakończeniu sprawdzania kliknij **dalej**.

    > [!NOTE]
    > Sprawdzanie poprawności zostało ukończone, gdy zostanie wyświetlony z zielonym znacznikiem wyboru pojawiają się obok przycisku Waliduj połączenie.

    ![Sprawdzanie poprawności połączenia](aspnet-mvc-4-custom-action-filters/_static/image31.png "sprawdzanie poprawności połączenia")

    *Sprawdzanie poprawności połączenia*
4. W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk obok pola tekstowego połączenia bazy danych (czyli **DefaultConnection**).

    ![Konfiguracja narzędzia Web deploy](aspnet-mvc-4-custom-action-filters/_static/image32.png "Konfiguracja narzędzia Web deploy")

    *Konfiguracja narzędzia Web deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W **nazwy serwera** wpisz swoją bazę danych SQL server adresu URL przy użyciu *tcp:* prefiks.
   - W **nazwa_użytkownika** wpisz nazwę logowania administratora serwera.
   - W **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nazwę nowej bazy danych.

     ![Konfigurowanie parametrów połączenia z docelowym](aspnet-mvc-4-custom-action-filters/_static/image33.png "Konfigurowanie parametrów połączenia z docelowym")

     *Konfigurowanie parametrów połączenia z docelowym*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu, aby utworzyć kliknij bazę danych **tak**.

    ![Tworzenie bazy danych](aspnet-mvc-4-custom-action-filters/_static/image34.png "Tworzenie parametrów bazy danych")

    *Tworzenie bazy danych*
7. Parametry połączenia, które będą używane do łączenia z bazą danych SQL na platformie Windows Azure jest wyświetlana w polu tekstowym połączenia domyślne. Następnie kliknij przycisk **Dalej**.

    ![Ciąg połączenia wskazujący bazę danych SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "ciąg połączenia wskazujący bazę danych SQL")

    *Ciąg połączenia wskazujący bazę danych SQL*
8. W **Podgląd** kliknij **Publikuj**.

    ![Publikowanie aplikacji sieci web](aspnet-mvc-4-custom-action-filters/_static/image36.png "publikowania aplikacji sieci web")

    *Publikowanie aplikacji sieci web*
9. Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Dodatek C: korzystania z fragmentów kodu

Za pomocą wstawek kodu masz kod, czego potrzebujesz w zasięgu ręki. Dokument laboratorium informuje o dokładnie podczas korzystania z nich, jak pokazano na poniższej ilustracji.

![Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu](aspnet-mvc-4-custom-action-filters/_static/image37.png "fragmenty kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu*

***Aby dodać wstawki kodu za pomocą klawiatury (tylko język C#)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Rozpocznij wpisywanie nazwy fragmentu kodu (bez spacji i łączniki).
3. Obejrzyj, jak technologia IntelliSense wyświetla zgodne z nazwami fragmenty kodu.
4. Wybierz prawidłowy fragment kodu lub pisz dalej do momentu wybrania debugowanej nazwy całego fragmentu kodu.
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

![Rozpocznij wpisywanie nazwy fragmentu kodu](aspnet-mvc-4-custom-action-filters/_static/image38.png "Rozpocznij wpisywanie nazwy fragmentu kodu")

*Rozpocznij wpisywanie nazwy fragmentu kodu*

![Naciśnij klawisz Tab, aby zaznaczyć fragment wyróżnione](aspnet-mvc-4-custom-action-filters/_static/image39.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragmentu kodu")

*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

![Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte](aspnet-mvc-4-custom-action-filters/_static/image40.png "rozwinie ponownie naciśnij klawisz Tab i fragment kodu")

*Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte.*

***Aby dodać wstawki kodu za pomocą myszy (C#, Visual Basic i XML)*** 1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.

1. Wybierz **Wstaw fragment kodu** następuje **Moje fragmenty kodu**.
2. Wybierz odpowiedni fragment z listy, klikając go.

![Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu](aspnet-mvc-4-custom-action-filters/_static/image41.png "kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu")

*Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu*

![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-custom-action-filters/_static/image42.png "wybierz odpowiedni fragment z listy, klikając go")

*Wybierz odpowiedni fragment z listy, klikając go*
