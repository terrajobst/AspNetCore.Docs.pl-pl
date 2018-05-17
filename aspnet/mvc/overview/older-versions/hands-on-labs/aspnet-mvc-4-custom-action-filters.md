---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtry akcji niestandardowych platformy ASP.NET MVC 4 | Dokumentacja firmy Microsoft
author: rick-anderson
description: ASP.NET MVC udostępnia filtry akcji wykonywania logiki filtrowania przed lub po nosi nazwę metody akcji. Filtry akcji są określona atrybutów niestandardowych...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 8b135b23aea64b0c7c7d4368eef9ee80914159e4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>Filtry akcji niestandardowych platformy ASP.NET MVC 4

Przez [obozów sieci Web Team](https://twitter.com/webcamps)

[Pobierz obozów sieci Web uczenie Kit](https://aka.ms/webcamps-training-kit)

ASP.NET MVC udostępnia filtry akcji wykonywania logiki filtrowania przed lub po nosi nazwę metody akcji. Filtry akcji są atrybutów niestandardowych, zawierających deklaratywne oznacza dodanie akcji przed i po zachowania do metod akcji kontrolera.

W tym laboratorium Hands-on spowoduje utworzenie atrybutu filtru akcji niestandardowej do rozwiązania MvcMusicStore catch kontrolera żądań i rejestrowanie aktywności lokacji do tabeli bazy danych. Będzie można dodać filtr rejestrowania przez uruchomienie kontrolera lub akcji. Na koniec zostanie wyświetlony widok dziennika z listą użytkowników.

W tym laboratorium Hands-on zakłada mieć podstawową wiedzę na temat **ASP.NET MVC**. Jeśli nie używasz **ASP.NET MVC** przed, zalecamy zapoznać się z **podstawowe informacje na temat platformy ASP.NET MVC 4** Hands-on laboratorium.

> [!NOTE]
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, pod [wersje Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Specyficzne dla tego laboratorium projektu jest dostępna na [filtry akcji niestandardowych platformy ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym laboratorium Hands-On przedstawiono sposób:

- Tworzenie atrybutu filtru akcji niestandardowej, aby rozszerzyć możliwości filtrowania
- Zastosuj atrybut niestandardowy filtr przez wstrzyknięcie do określonego poziomu
- Zarejestruj globalnie filtry akcji niestandardowej

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Musi mieć następujące elementy do przygotowania tego laboratorium:

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek a.](#AppendixA) instrukcje dotyczące sposobu jego instalacji).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

**Instalowanie wstawki kodu**

Dla wygody taki kod, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako wstawki kodu programu Visual Studio. Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.

Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek C:](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

W tym laboratorium Hands-On składa się przez następujących czynnościach:

1. [Ćwiczenie 1: Rejestrowanie akcji](#Exercise1)
2. [Ćwiczenie 2: Zarządzanie wielu filtrów akcji](#Exercise2)

Szacowany czas trwania tego laboratorium: **30 minut**.

> [!NOTE]
> Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach. Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Ćwiczenie 1: Rejestrowanie akcji

W tym ćwiczeniu dowiesz się, jak utworzyć filtr dziennika akcji niestandardowej przy użyciu dostawców filtrów programu ASP.NET MVC 4. W tym celu filtr rejestrowania będą dotyczyć lokacji MusicStore, który będzie rejestrować wszystkie działania w wybranych kontrolerów.

Rozszerzenie filtr **ActionFilterAttributeClass** i zastąpić **OnActionExecuting** metodę przechwytuje każde żądanie, a następnie wykonać akcje rejestrowania. Informacje o kontekście dotyczące żądania HTTP, wykonywanie metod, wyniki i parametry będą udostępniane przez program ASP.NET MVC **ActionExecutingContext** klasy **.**

> [!NOTE]
> ASP.NET MVC 4 ma również domyślnych dostawców filtrów, których można używać bez tworzenia niestandardowego filtru. ASP.NET MVC 4 zawiera następujące typy filtrów:
> 
> - **Autoryzacji** filtrować, co czyni zabezpieczeń decyzji o tym, czy można wykonać metody akcji, takich jak uwierzytelniania lub sprawdzania poprawności właściwości żądania.
> - **Akcja** filtr, który opakowuje wykonanie metody akcji. Ten filtr można wykonać przetwarzania dodatkowe, takie jak dostarczanie dodatkowe dane do metody akcji, sprawdzania wartości zwracanej lub anulowanie wykonywania metody akcji
> - **Wynik** filtr, który opakowuje wykonanie obiektu ActionResult. Ten filtr może wykonywać dodatkowego przetwarzania wyniku, takie jak modyfikowanie odpowiedzi HTTP.
> - **Wyjątek** filtr, który wykonuje przypadku nieobsługiwany wyjątek gdzieś w metodzie akcji, począwszy od filtry autoryzacji i kończy wykonywanie wyniku. Filtry wyjątków może służyć do zadań, takich jak rejestrowanie lub wyświetlanie stronę błędu.
> 
> Aby uzyskać więcej informacji o dostawcach filtry odwiedź linkiem MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>Informacje o funkcji rejestrowania aplikacji programu MVC utworów muzycznych magazynu

To rozwiązanie magazynu utworów muzycznych ma nowej tabeli modelu danych do rejestrowania lokacji **ActionLog**, z następującymi polami: Nazwa kontrolera, który odebrał żądanie, wywoływanej akcji, IP klienta i sygnaturę czasową.

![Model danych. Tabela ActionLog. ](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modelu danych. Tabela ActionLog.")

*Model danych - ActionLog tabeli*

Rozwiązanie zawiera widok MVC ASP.NET do dziennika akcji, który znajduje się w temacie **ActionLog-MvcMusicStores/widoków**:

![Widok dziennika akcji](aspnet-mvc-4-custom-action-filters/_static/image2.png "widok dziennika akcji")

*Widok dziennika akcji*

Z tym podane struktury całą pracę koncentrować się na przerywania żądania kontrolera, a następnie wykonać rejestrowanie przy użyciu niestandardowych filtrowania.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Zadanie 1 — Tworzenie niestandardowego filtru do wychwytywania żądania kontrolera

W ramach tego zadania spowoduje utworzenie klasy atrybutu niestandardowego filtru, która będzie zawierać logikę rejestrowania. W tym celu można rozszerzyć ASP.NET MVC **ActionFilterAttribute** klasy i zaimplementuj interfejs **IActionFilter**.

> [!NOTE]
> **ActionFilterAttribute** jest klasa podstawowa dla wszystkich filtrów atrybutu. Udostępnia ona następujące metody umożliwiające wykonanie konkretnej logiki po i przed wykonaniem akcji kontrolera:
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): tuż przed akcji jest wywoływana metoda.
> - **OnActionExecuted**(ActionExecutedContext filterContext): po wywołaniu metody akcji i przed (przed renderowania widoku) jest wykonywany wynik.
> - **OnResultExecuting**(ResultExecutingContext filterContext): bezpośrednio przed (przed renderowania widoku) jest wykonywany wynik.
> - **OnResultExecuted**(ResultExecutedContext filterContext): po wykonaniu wyniku (po renderowania widoku).
> 
> Przez zastąpienie jednej z tych metod w klasie pochodnej, można wykonać filtrowania kodu.


1. Otwórz **rozpocząć** rozwiązania, znajdujących się na **\Source\Ex01-LoggingActions\Begin** folderu.

   1. Należy pobrać niektórych brakujących pakietów NuGet, przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
      > 
      > Aby uzyskać więcej informacji, zobacz ten artykuł: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Dodaj nową klasę C# do **filtry** folder i nadaj mu nazwę *CustomActionFilter.cs*. Ten folder będzie przechowywać wszystkie filtry niestandardowe.
3. Otwórz **CustomActionFilter.cs** i Dodaj odwołanie do **System.Web.Mvc** i **MvcMusicStore.Models** przestrzeni nazw:

    (Fragment - kodu *filtry akcji niestandardowych platformy ASP.NET MVC 4 - Ex1 CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Dziedzicz **CustomActionFilter** klasę z **ActionFilterAttribute** , a następnie wprowadź **CustomActionFilter** implementacji klasy **IActionFilter** interfejsu.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Wprowadź **CustomActionFilter** klasy przesłonić metodę **OnActionExecuting** i Dodaj logikę niezbędne do logowania się wykonanie filtru. Aby to zrobić, Dodaj następujący wyróżniony kod w **CustomActionFilter** klasy.

    (Fragment - kodu *filtry akcji niestandardowych platformy ASP.NET MVC 4 - Ex1 LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > **OnActionExecuting** używa metody **Entity Framework** można dodać nowego rejestru ActionLog. Tworzy i wypełnia nowe wystąpienie jednostki informacje o kontekście z **filterContext**.
    > 
    > Możesz przeczytać dodatkowe informacje **kontekstem ControllerContext** klasy w [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Zadanie 2 — wstrzykiwania interceptora kodu do klasy kontrolera magazynu

To zadanie spowoduje dodanie niestandardowego filtru przez wstrzykiwanie go do wszystkich klas kontrolera i akcji kontrolera, które będą rejestrowane. Na potrzeby tego ćwiczenia klasy kontrolera magazynu ma dziennika.

Metoda **OnActionExecuting** z **ActionLogFilterAttribute** niestandardowy filtr jest uruchamiany, gdy jest wywoływana wprowadzony element.

Istnieje również możliwość przechwycenia metody określonego kontrolera.

1. Otwórz **StoreController** w **MvcMusicStore\Controllers** i Dodaj odwołanie do **filtry** przestrzeni nazw:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Wstaw niestandardowy filtr **CustomActionFilter** do **StoreController** klasy, dodając **[CustomActionFilter]** atrybutu przed deklaracją klasy.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Gdy filtr jest wstrzykiwane do klasy kontrolera, również są wprowadzić swoje działania. Jeśli chcesz zastosować filtr tylko dla działań, konieczne będzie przeprowadzenie wstrzyknąć **[CustomActionFilter]** do każdej z nich z nich:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3 — uruchamianie aplikacji

W tym zadaniu zostanie testu działa filtr rejestrowania. Będzie uruchomić aplikację i przejdź do sklepu, a następnie będzie sprawdzać rejestrowane działania.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Przejdź do **/ActionLog** aby zobaczyć stan początkowy widok dziennika:

    ![Rejestrowanie śledzenia stanu przed działania strony](aspnet-mvc-4-custom-action-filters/_static/image3.png "rejestrowania śledzenia stanu przed działania strony")

    *Dziennik śledzenia stanu przed działania strony*

   > [!NOTE]
   > Domyślnie go będzie zawsze pokazuj elementu, który jest generowany podczas pobierania istniejących genres menu.
   > 
   > Dla uproszczenia możemy jest czyszczony **ActionLog** tabeli każdym uruchomieniu aplikacji, będzie wyświetlana tylko dzienniki weryfikacji każdego określonego zadania.
   > 
   > Konieczne może być usuń poniższy kod z **sesji\_Start** — metoda (w **Global.asax** klasy), aby zapisać dziennik historyczny dla wszystkich akcji, które są wykonywane w magazynie Kontroler.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Kliknij jeden z **Genres** z menu i wykonywania pewnych działań, takich jak przeglądanie dostępnych albumu.
4. Przejdź do **/ActionLog** i jeśli dziennik jest pusty naciśnij **F5** odświeżenie strony. Sprawdź, czy zostały śledzone wizyty:

    ![Dziennik akcji z działaniem rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image4.png "dziennika akcji z działaniem rejestrowane")

    *Dziennik akcji z działaniem rejestrowane*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Ćwiczenie 2: Zarządzanie wielu filtrów akcji

W tym ćwiczeniu zostanie dodać drugi niestandardowy filtr akcji do klasy StoreController i zdefiniuj określonej kolejności, w której zostanie wykonana obu filtrów. Następnie zostanie zaktualizuj kod, aby zarejestrować filtr globalny.

Istnieją różne opcje wziąć pod uwagę podczas definiowania kolejność wykonywania filtrów. Na przykład właściwość kolejności i zakres te filtry:

Można zdefiniować **zakres** dla każdego z filtrów, na przykład można zakresu wszystkie filtry akcji do uruchomienia w ramach **zakresu kontrolera**oraz wszystkie filtry autoryzacji do uruchamiania w **zakresie globalnym** . Zakresy mają kolejność wykonywania zdefiniowane.

Ponadto każdy filtr akcji ma właściwość kolejność, która służy do określania kolejności wykonywania w zakresie filtru.

Aby uzyskać więcej informacji na temat kolejność wykonywania filtrów akcji niestandardowej, odwiedź ten artykuł w witrynie MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Zadanie 1: Tworzenie nowego niestandardowy filtr akcji

W ramach tego zadania spowoduje utworzenie nowej niestandardowy filtr akcji do dodania do klasy StoreController dowiedzieć, jak zarządzać kolejność wykonywania filtrów.

1. Otwórz **rozpocząć** rozwiązania, znajdujących się na **\Source\Ex02-ManagingMultipleActionFilters\Begin** folderu. W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.

    1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
    2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
    3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

        > [!NOTE]
        > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
        > 
        > Aby uzyskać więcej informacji, zobacz ten artykuł: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Dodaj nową klasę C# do **filtry** folder i nadaj mu nazwę *MyNewCustomActionFilter.cs*
3. Otwórz **MyNewCustomActionFilter.cs** i Dodaj odwołanie do **System.Web.Mvc** i **MvcMusicStore.Models** przestrzeni nazw:

    (Fragment - kodu *filtry akcji niestandardowych platformy ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Zastąp domyślną deklaracji klasy następującym kodem.

    (Fragment - kodu *filtry akcji niestandardowych platformy ASP.NET MVC 4 - Ex2 MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Ten niestandardowy filtr akcji jest niemal taki sam, niż ten, który został utworzony w poprzednim ćwiczeniu. Główną różnicą jest to, że ma *&quot;rejestrowane przez&quot;* zaktualizowany o nazwie ta nowa klasa, aby zidentyfikować filtru upubliczniona atrybut zarejestrowany dziennika.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Zadanie 2: Wstrzykiwania nowe interceptora kodu do klasy StoreController

W tym zadaniu zostanie dodaje nowy filtr niestandardowy do klasy StoreController i uruchamianie rozwiązania, aby sprawdzić, jak obu filtrów współpracują ze sobą.

1. Otwórz **StoreController** klasa znajduje się w **MvcMusicStore\Controllers** i Wstaw nowy filtr niestandardowy **MyNewCustomActionFilter** do  **StoreController** klasy, jak przedstawiono w poniższym kodzie.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. Teraz uruchom aplikację, aby zobaczyć, jak działają te dwa filtry akcji niestandardowej. Aby to zrobić, naciśnij klawisz **F5** i poczekaj, aż uruchamiania aplikacji.
3. Przejdź do **/ActionLog** aby zobaczyć stan początkowy widok dziennika.

    ![Rejestrowanie śledzenia stanu przed działania strony](aspnet-mvc-4-custom-action-filters/_static/image5.png "rejestrowania śledzenia stanu przed działania strony")

    *Dziennik śledzenia stanu przed działania strony*
4. Kliknij jeden z **Genres** z menu i wykonywania pewnych działań, takich jak przeglądanie dostępnych albumu.
5. Sprawdź, czy ten czas; wizyty zostały śledzone dwa razy: po każdej filtry akcji niestandardowych dodaniu w **StorageController** klasy.

    ![Dziennik akcji z działaniem rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image6.png "dziennika akcji z działaniem rejestrowane")

    *Dziennik akcji z działaniem rejestrowane*
6. Zamknij przeglądarkę.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Zadanie 3: Zarządzanie kolejności filtru

W tym zadaniu dowiesz się, jak zarządzać kolejność wykonywania filtrów za pomocą właściwości kolejności.

1. Otwórz **StoreController** klasa znajduje się w **MvcMusicStore\Controllers** i określ **kolejności** właściwości w obu filtrów, takich jak pokazano poniżej.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. Sprawdź teraz, jak filtry są wykonywane w zależności od wartości jego właściwość kolejności. Zostanie stwierdzisz, że filtr mający najmniejszą wartość kolejności (**CustomActionFilter**) jest pierwszą, która jest wykonywana. Naciśnij klawisz **F5** i poczekaj, aż uruchamiania aplikacji.
3. Przejdź do **/ActionLog** aby zobaczyć stan początkowy widok dziennika.

    ![Rejestrowanie śledzenia stanu przed działania strony](aspnet-mvc-4-custom-action-filters/_static/image7.png "rejestrowania śledzenia stanu przed działania strony")

    *Dziennik śledzenia stanu przed działania strony*
4. Kliknij jeden z **Genres** z menu i wykonywania pewnych działań, takich jak przeglądanie dostępnych albumu.
5. Sprawdź, czy ten czas wizyty zostały śledzone uporządkowanych według wartości kolejności filtrów: **CustomActionFilter** dzienniki pierwszy.

    ![Dziennik akcji z działaniem rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image8.png "dziennika akcji z działaniem rejestrowane")

    *Dziennik akcji z działaniem rejestrowane*
6. Zostanie teraz zaktualizować wartość kolejności filtrów i sprawdź, jak zmienia kolejność rejestrowania. W **StoreController** klasy, zaktualizuj wartość kolejności filtrów, jak pokazano poniżej.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Uruchom aplikację ponownie, naciskając klawisz **F5**.
8. Kliknij jeden z **Genres** z menu i wykonywania pewnych działań, takich jak przeglądanie dostępnych albumu.
9. Sprawdź, czy ten czas dzienniki utworzony przez **MyNewCustomActionFilter** filtru występuje jako pierwszy.

    ![Dziennik akcji z działaniem rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image9.png "dziennika akcji z działaniem rejestrowane")

    *Dziennik akcji z działaniem rejestrowane*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Zadanie 4: Rejestrowanie globalnie filtry.

To zadanie zaktualizuje rozwiązanie, aby zarejestrować nowego filtru (**MyNewCustomActionFilter**) jako filtr globalny. W ten sposób zostanie wywołane przez wszystkie przeprowadzane akcje w aplikacji i nie tylko StoreController widocznych w poprzednim zadaniu.

1. W **StoreController** klasy, Usuń **[MyNewCustomActionFilter]** atrybutu, a właściwość kolejności z **[CustomActionFilter]**. Jego wygląd powinien być podobne do poniższych:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Otwórz **Global.asax** plików i Znajdź **aplikacji\_Start** metody. Powiadomienie, że każdy czas uruchamiania aplikacji go rejestruje filtry globalne wywołując **RegisterGlobalFilters** metodę **FilterConfig** klasy.

    ![Rejestrowanie w pliku Global.asax filtry globalne](aspnet-mvc-4-custom-action-filters/_static/image10.png "rejestrowanie filtry globalne w pliku Global.asax")

    *Rejestrowanie w pliku Global.asax filtry globalne*
3. Otwórz **FilterConfig.cs** pliku w ramach **aplikacji\_Start** folderu.
4. Dodaj odwołanie do przy użyciu System.Web.Mvc; przy użyciu MvcMusicStore.Filters; przestrzeń nazw.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Aktualizacja **RegisterGlobalFilters** metody dodawania filtru niestandardowego. Aby to zrobić, Dodaj wyróżniony kod:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Uruchom aplikację, naciskając klawisz **F5**.
7. Kliknij jeden z **Genres** z menu i wykonywania pewnych działań, takich jak przeglądanie dostępnych albumu.
8. Sprawdź, która teraz **[MyNewCustomActionFilter]** jest trwa zbyt wprowadzonym w HomeController i ActionLogController.

    ![Dziennik akcji z działaniem rejestrowane](aspnet-mvc-4-custom-action-filters/_static/image11.png "dziennika akcji z działaniem rejestrowane")

    *Dziennik akcji z globalnego zarejestrowanego działania*

> [!NOTE]
> Ponadto można wdrożyć tę aplikację systemu Windows Azure Web Sites następujących [dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Wykonując tego laboratorium Hands-On uzyskanych jak rozszerzyć filtr akcji do wykonania akcji niestandardowych. Ma również przedstawiono sposób wstrzyknąć filtr, który ma kontrolerów strony. Zostały użyte następujące kwestie:

- Jak utworzyć filtry akcji niestandardowych z klasy ASP.NET MVC ActionFilterAttribute
- Jak można wstawić filtrów do kontrolerów ASP.NET MVC
- Jak zarządzać filtru kolejność przy użyciu właściwości Order
- Jak zarejestrować filtry globalny

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; <em>programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.
2. Polecenie **teraz zainstalować**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Instalowanie programu Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "instalacji programu Visual Studio Express")

    *Instalowanie programu Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Akceptowanie umowy licencyjnej*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    *VS Express for Web kafelka*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek B: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy

Ten dodatek opisano sposób tworzenia nowej witryny sieci web z portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskane wykonując laboratorium, korzystając z funkcji publikowania narzędzia Web Deploy dostarczane przez Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Zadanie 1 — Tworzenie nowej witryny sieci Web systemu Windows portalu Azure

1. Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń Microsoft skojarzonych z Twoją subskrypcją.

    > [!NOTE]
    > Z systemu Windows Azure można udostępniać 10 witryn sieci Web platformy ASP.NET bezpłatnie i następnie Skaluj w miarę zwiększania się ruchu. Możesz utworzyć konto [tutaj](http://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do portalu Windows Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "Zaloguj się do portalu Windows Azure")

    *Zaloguj się do portalu zarządzania platformy Azure z systemem Windows*
2. Kliknij przycisk **nowy** na pasku poleceń.

    ![Tworzenie nowej witryny sieci Web](aspnet-mvc-4-custom-action-filters/_static/image18.png "tworzenia nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij przycisk **obliczeniowe** | **witryny sieci Web**. Następnie wybierz **szybkie tworzenie** opcji. Podaj dostępny adres URL dla nowej witryny sieci web, a następnie kliknij przycisk **tworzenie witryny sieci Web**.

    > [!NOTE]
    > Witryny sieci Web systemu Windows Azure jest hostem dla aplikacji sieci web w chmurze, które można kontrolować i zarządzanie nimi. Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do systemu Windows Azure witryny internetowej z spoza portalu. Nie obejmuje kroki konfigurowania bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie](aspnet-mvc-4-custom-action-filters/_static/image19.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")

    *Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*
4. Poczekaj na nowe **witryny sieci Web** jest tworzony.
5. Po utworzeniu witryny sieci Web kliknij łącze w obszarze **adres URL** kolumny. Sprawdź, czy działa nowej witryny sieci Web.

    ![Przeglądanie do nowej witryny sieci web](aspnet-mvc-4-custom-action-filters/_static/image20.png "przeglądanie do nowej witryny sieci web")

    *Przeglądanie do nowej witryny sieci web*

    ![Witryna sieci Web działa](aspnet-mvc-4-custom-action-filters/_static/image21.png "uruchamiania witryny sieci Web")

    *Witryna sieci Web uruchomiona*
6. Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.

    ![Otwieranie stron witryny sieci web zarządzania](aspnet-mvc-4-custom-action-filters/_static/image22.png "otwieranie stron zarządzania witryny sieci web")

    *Otwieranie stron zarządzania witryny sieci Web*
7. W **pulpitu nawigacyjnego** w obszarze **szybkiego dostępu** kliknij **pobieranie profilu publikowania** łącza.

    > [!NOTE]
    > *Profilu publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web do witryny sieci Web systemu Windows Azure dla każdej metody włączone publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego z punktów końcowych, dla których włączono metoda publikacji. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **programu Microsoft Visual Studio 2012** obsługują odczytywanie publikowanie profile do zautomatyzowania te programy Publikowanie aplikacji sieci web do witryn sieci Web systemu Windows Azure.

    ![Pobieranie witryny sieci web profilu publikowania](aspnet-mvc-4-custom-action-filters/_static/image23.png "pobierania witryny sieci web profilu publikowania")

    *Pobieranie witryny sieci Web profilu publikowania*
8. Pobierz profil publikowania w znanej lokalizacji. Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do witryny sieci Web systemu Windows Azure w programie Visual Studio przy użyciu tego pliku.

    ![Zapisywanie pliku profilu publikowania](aspnet-mvc-4-custom-action-filters/_static/image24.png "zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z programu SQL Server baz danych, należy utworzyć serwer bazy danych SQL. Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.

1. Będzie potrzebny serwer bazy danych SQL do przechowywania bazy danych aplikacji. Można wyświetlić serwery bazy danych SQL z subskrypcji usługi Windows Azure Management Portal pod adresem **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**. Jeśli nie masz serwer, który został utworzony, można utworzyć przy użyciu jednego **Dodaj** przycisk paska poleceń. Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, jak będą używane w następnego zadania. Nie należy tworzyć bazy danych jeszcze, jako zostaną utworzone w późniejszym terminie.

    ![Pulpit nawigacyjny serwera bazy danych SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "pulpitu nawigacyjnego serwera bazy danych SQL")

    *Pulpit nawigacyjny serwera bazy danych SQL*
2. W następnym zadaniem Testuj połączenie z bazą danych z programu Visual Studio z tego powodu należy uwzględnić lokalny adres IP serwera liście **dozwolone adresy IP**. Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) przycisku.

    ![Dodawanie adresu IP klienta](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Dodawanie adresu IP klienta*
3. Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP kliknij na **zapisać** o potwierdzenie zmian.

    ![Potwierdzenie zmian](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Potwierdzenie zmian*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 - publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **publikowania**.

    ![Publikowanie aplikacji](aspnet-mvc-4-custom-action-filters/_static/image29.png "publikowania aplikacji")

    *Publikowanie witryny sieci web*
2. Zaimportuj profil publikowania, zapisana w pierwszym zadaniu.

    ![Importowanie profilu publikowania](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importowanie profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij przycisk **Weryfikacja połączenia z**. Po zakończeniu sprawdzania kliknij **dalej**.

    > [!NOTE]
    > Zakończeniu sprawdzania poprawności, gdy zostanie wyświetlony zielony znacznik wyboru są wyświetlane obok przycisku sprawdzania poprawności połączenia.

    ![Sprawdzanie poprawności połączenia](aspnet-mvc-4-custom-action-filters/_static/image31.png "sprawdzanie poprawności połączenia")

    *Sprawdzanie poprawności połączenia*
4. W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk Dalej, aby textbox połączenia bazy danych (tj. **połączenia DefaultConnection**).

    ![Konfiguracja narzędzia Web deploy](aspnet-mvc-4-custom-action-filters/_static/image32.png "Konfiguracja narzędzia Web deploy")

    *Konfiguracja narzędzia Web deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W **nazwy serwera** wpisz swoją bazą danych SQL server adresu URL przy użyciu *tcp:* prefiks.
   - W **nazwy użytkownika** wpisz nazwę logowania administratora serwera.
   - W **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nazwę nowej bazy danych.

     ![Konfigurowanie parametrów połączenia z lokalizacją docelową](aspnet-mvc-4-custom-action-filters/_static/image33.png "Konfigurowanie parametrów połączenia z lokalizacją docelową")

     *Konfigurowanie parametrów połączenia z lokalizacją docelową*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu można utworzyć bazy danych kliknij **tak**.

    ![Tworzenie bazy danych](aspnet-mvc-4-custom-action-filters/_static/image34.png "tworzenie parametry bazy danych")

    *Tworzenie bazy danych*
7. Ciągu połączenia używanego do łączenia z bazą danych SQL w systemie Windows Azure jest wyświetlany w pole tekstowe domyślne połączenie. Następnie kliknij przycisk **Dalej**.

    ![Parametry połączenia wskazujące bazę danych SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "ciąg połączenia wskazujące bazę danych SQL")

    *Parametry połączenia wskazujące bazę danych SQL*
8. W **Podgląd** kliknij przycisk **publikowania**.

    ![Publikowanie aplikacji sieci web](aspnet-mvc-4-custom-action-filters/_static/image36.png "publikowania aplikacji sieci web")

    *Publikowanie aplikacji sieci web*
9. Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Dodatek C: korzystania z wstawek kodu

Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki. Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.

![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](aspnet-mvc-4-custom-action-filters/_static/image37.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*

***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragment (bez spacji i łączniki).
3. Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.
4. Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

![Rozpocznij wpisywanie nazwy fragment](aspnet-mvc-4-custom-action-filters/_static/image38.png "zacznij wpisywać nazwę wstawki programu")

*Rozpocznij wpisywanie nazwy fragment kodu*

![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](aspnet-mvc-4-custom-action-filters/_static/image39.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")

*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

![Ponownie naciśnij klawisz Tab i fragment rozwinie](aspnet-mvc-4-custom-action-filters/_static/image40.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")

*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*

***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)*** 1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.

1. Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.
2. Wybierz odpowiedni fragment z listy, klikając go.

![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](aspnet-mvc-4-custom-action-filters/_static/image41.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")

*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*

![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-custom-action-filters/_static/image42.png "wybierz odpowiedni fragment z listy, klikając go")

*Wybierz odpowiedni fragment z listy, klikając go*
