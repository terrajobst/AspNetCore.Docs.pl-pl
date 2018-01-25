---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: "Iniekcji zależności platformy ASP.NET MVC 4 | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Uwaga: W tym laboratorium Hands-on przyjęto założenie, że masz podstawową wiedzę na temat filtrów platformy ASP.NET MVC i platformy ASP.NET MVC 4. Jeśli nie użyto filtrów platformy ASP.NET MVC 4 przed możemy rec..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 48a7d7fdb670aebb72450fc4eb12a364ef595c53
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-dependency-injection"></a>Iniekcji zależności platformy ASP.NET MVC 4
====================
przez [obozów sieci Web Team](https://twitter.com/webcamps)

> [!NOTE]
> W tym laboratorium Hands-on zakłada mieć podstawową wiedzę na temat **ASP.NET MVC** i **filtrów platformy ASP.NET MVC 4**. Jeśli nie używasz **filtrów platformy ASP.NET MVC 4** przed, zalecamy zapoznać się z **filtry akcji niestandardowych MVC ASP.NET** Hands-on laboratorium.
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).


W **programowanie zorientowane na obiekt** modelu, obiekty współdziałają ze sobą w modelu współpracy w przypadku, gdy istnieją współautorzy i konsumentów. Oczywiście ten model komunikacji generuje zależności między obiektami i składników, staje się trudno było zarządzać podczas zwiększa złożoności.

![Klasa zależności i modelu złożoności](aspnet-mvc-4-dependency-injection/_static/image1.png "klasy zależności i modelu złożoności")

*Klasa zależności i złożoność modelu*

Prawdopodobnie ma wiesz o **wzorzec fabryki** i rozdzielenie interfejs i wdrożenia przy użyciu usług, gdy obiekty klienta często są odpowiedzialne za lokalizacji usługi.

Wzorzec iniekcji zależności jest konkretnej implementacji Inwersja kontroli. **Inwersja kontroli (IoC)** oznacza, że obiektów należy tworzyć inne obiekty, które opierają się ich w pracy. Zamiast tego otrzymują obiektów, które potrzebują ze źródła zewnętrznego (na przykład plik konfiguracyjny xml).

**Zależności Iniekcja** oznacza, że jest to zrobić bez interwencji obiektu zwykle przez składnik framework, która przekazuje parametry konstruktora i ustaw właściwości.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>Wzorzec projektowy (Podpisane) iniekcji zależności

Na wysokim poziomie celem iniekcji zależności jest to, że klasa klienta (np. *golfer*) wymaga czegoś, która obsługuje interfejs (np. *IClub*). Nowe zależy, co to jest konkretnego typu (np. *WedgeClub WoodClub, IronClub,* lub *PutterClub*), ktoś inny do obsługi, który chce (np. jest dobrą *caddy*). Mechanizm rozpoznawania zależności na platformie ASP.NET MVC umożliwia zarejestrować gdzieś logiki zależności (np. kontener lub *zbioru klubów*).

![Diagram iniekcji zależności](aspnet-mvc-4-dependency-injection/_static/image2.png "ilustracji iniekcji zależności")

*Iniekcji zależności - odpowiednio pól*

Zalety korzystania z wzorca iniekcji zależności oraz Inwersja kontroli są następujące:

- Zmniejsza sprzężenia klas
- Zwiększa ponowne wykorzystywanie kodu
- Zwiększa utrzymanie kodu
- Zwiększa testowanie aplikacji

> [!NOTE]
> Iniekcji zależności czasami jest porównywany ze wzorca projektowego fabryki abstrakcyjny, ale istnieje niewielka różnica między obu podejść. Podpisane ma środowisko pracy za rozwiązać zależności w wywołaniu fabryk i zarejestrowane usługi.


Znasz wzorzec iniekcji zależności dowiesz w tym laboratorium sposób stosowania go w programie ASP.NET MVC 4. Zostanie uruchomiona, przy użyciu iniekcji zależności w **kontrolerów** uwzględnienie usługi dostępu do bazy danych. Następnie należy zastosować iniekcji zależności do **widoków** do wykorzystania usługi i zawierają informacje. Na koniec rozszerzenie Podpisane do platformy ASP.NET MVC 4 filtrów, zostanie wstrzyknięta filtr akcji niestandardowej w rozwiązaniu.

W tym laboratorium Hands-on przedstawiono sposób:

- Integracja programu ASP.NET MVC 4 z Unity iniekcji zależności za pomocą pakietów NuGet
- Iniekcji zależności w obrębie kontrolera ASP.NET MVC
- Iniekcji zależności w widoku programu ASP.NET MVC
- Iniekcji zależności wewnątrz filtr akcji platformy ASP.NET MVC

> [!NOTE]
> W tym laboratorium jest używany pakiet NuGet Unity.Mvc3 rozpoznawania zależności, ale istnieje możliwość dostosowania dowolnej architektury iniekcji zależności do pracy z platformy ASP.NET MVC 4.


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

Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek B:](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

W tym laboratorium Hands-On składa się przez następujących czynnościach:

1. [Ćwiczenie 1: Wstrzyknięcie kontrolera](#Exercise1)
2. [Ćwiczenie 2: Wstrzyknięcie widoku](#Exercise2)
3. [Ćwiczenie 3: Wstrzyknięcie filtry](#Exercise3)

> [!NOTE]
> Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach. Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.


Szacowany czas trwania tego laboratorium: **30 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Ćwiczenie 1: Wstrzyknięcie kontrolera

W tym ćwiczeniu dowiesz się, jak używać iniekcji zależności w kontrolery ASP.NET MVC, integrując Unity przy użyciu pakietu NuGet. Z tego powodu będzie dołączać usług do kontrolerów MvcMusicStore do oddzielania logiki z dostępu do danych. Usługi spowoduje utworzenie nowej zależności w Konstruktorze kontrolera, który zostanie rozwiązany przy użyciu iniekcji zależności za pomocą **Unity**.

Takie podejście opisano sposób generowania mniej sprzężonego aplikacji, które są bardziej elastyczne i łatwiejsze do utrzymywania i testowania. Zostanie również sposób integracji ASP.NET MVC z Unity.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>Usługa StoreManager informacje

Magazyn utworów muzycznych MVC podane w rozwiązaniu Rozpocznij teraz obejmują usługę, która zarządza danymi kontrolera magazynu o nazwie **StoreService**. Poniżej przedstawiono implementacji usługi magazynu. Należy pamiętać, że wszystkie metody zwraca jednostek w modelu.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** BEGIN teraz zużywa rozwiązania **StoreService**. Usunięto wszystkie odwołania do danych z **StoreController**i teraz można modyfikować bieżącego dostawcę dostępu do danych bez zmieniania dowolnej metody, który wykorzystuje **StoreService**.

Znajdują się poniżej **StoreController** implementacji ma zależność o **StoreService** wewnątrz konstruktora klasy.

> [!NOTE]
> Zależności wprowadzone w tym ćwiczeniu jest powiązany z **Inwersja kontroli** (IoC).
> 
> **StoreController** odbiera konstruktora klasy **IStoreService** parametr typu, które są niezbędne do wykonania wywołania usługi z wewnątrz klasy. Jednak **StoreController** nie implementuje konstruktora domyślnego (z żadnych parametrów), że każdy kontroler musi mieć do pracy z platformą ASP.NET MVC.
> 
> Aby rozwiązać zależności, kontrolera musi być utworzony przez fabrykę abstrakcyjny (Klasa zwraca dowolnego obiektu określonego typu).


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Wystąpił błąd wystąpi po klasie próbuje utworzyć StoreController bez wysyłania obiektu usługi, ponieważ nie istnieje żaden konstruktor bez parametrów zadeklarowany.


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Zadanie 1 - uruchamiania aplikacji

To zadanie uruchomi aplikacji Begin, która obejmuje usługę do kontrolera magazynu, która oddziela dostęp do danych z aplikacji logiki.

Podczas uruchamiania aplikacji, jak usługa kontrolera nie zostanie przekazany jako parametr domyślnie zostanie wyświetlony Wystąpił wyjątek:

1. Otwórz **rozpocząć** rozwiązania, znajdujących się w **wstrzyknięcie Source\Ex01 Controller\Begin**.

    1. Należy pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
    2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
    3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

    > [!NOTE]
    > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Naciśnij klawisz **Ctrl + F5** do uruchomienia aplikacji bez debugowania. Otrzymasz komunikat o błędzie &quot; **nie konstruktora bez parametrów zdefiniowanych dla tego obiektu**&quot;:

    ![Błąd podczas uruchamiania rozpocząć aplikacji programu ASP.NET MVC](aspnet-mvc-4-dependency-injection/_static/image3.png "błąd podczas uruchamiania aplikacji rozpocząć MVC ASP.NET")

    *Błąd podczas uruchamiania aplikacji rozpocząć MVC ASP.NET*
3. Zamknij przeglądarkę.

W poniższych krokach będą działać w ramach rozwiązania magazynu utworów muzycznych iniekcję zależności, potrzebne w tym kontrolerze.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Zadanie 2 — w tym Unity do rozwiązania MvcMusicStore

To zadanie będzie zawierać **Unity.Mvc3** pakietu NuGet w rozwiązaniu.

> [!NOTE]
> Pakiet Unity.Mvc3 był przeznaczony dla platformy ASP.NET MVC 3, ale jest w pełni zgodny z platformy ASP.NET MVC 4.
> 
> Unity jest na przykład kontener iniekcji zależności niewielka, rozszerzalne z obsługą opcjonalne i wpisz zatrzymania. Jest kontenerem ogólnego przeznaczenia do użycia w dowolnego typu aplikacji .NET. Zapewnia wspólne funkcje, w tym mechanizmów iniekcji zależności: Tworzenie obiektów, abstrakcji wymagania, określając zależności środowiska uruchomieniowego i elastyczność, przez odkładanie konfiguracji składnika do kontenera.


1. Zainstaluj **Unity.Mvc3** pakietu NuGet w **MvcMusicStore** projektu. Aby to zrobić, otwórz **Konsola Menedżera pakietów** z **widoku** | **inne okna**.
2. Uruchom następujące polecenie.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Installing Unity.Mvc3 NuGet Package](aspnet-mvc-4-dependency-injection/_static/image4.png "Installing Unity.Mvc3 NuGet Package")

    *Installing Unity.Mvc3 NuGet Package*
3. Raz **Unity.Mvc3** pakiet jest zainstalowany, Analizuj pliki i foldery, które automatycznie dodaje Aby uprościć konfigurację Unity.

    ![Zainstalowanym pakietem Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "zainstalowanym pakietem Unity.Mvc3")

    *Pakiet Unity.Mvc3 zainstalowany*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>Zadanie 3 — rejestrowanie Unity w aplikacji Global.asax.cs\_Start

To zadanie zaktualizuje **aplikacji\_Start** metoda znajduje się w **Global.asax.cs** można wywołać inicjatora programu inicjującego Unity, a następnie, zaktualizuj rejestrowanie pliku inicjującego Usługi i kontrolera użyjesz iniekcji zależności.

1. Teraz zostanie Podłączanie programu inicjującego, czyli plik, który inicjuje kontenera Unity i rozpoznawania zależności. Aby to zrobić, otwórz **Global.asax.cs** i Dodaj następujący wyróżniony kod w **aplikacji\_Start** metody.

    (Fragment - kodu *laboratorium iniekcji zależności ASP.NET - Ex01 - zainicjować Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Otwórz **Bootstrapper.cs** pliku.
3. Obejmują następujących przestrzeni nazw: **MvcMusicStore.Services** i **MusicStore.Controllers**.

    (Fragment - kodu *Dodawanie przestrzeni nazw programu ASP.NET zależności iniekcji laboratorium - Ex01 - inicjującego*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Zastąp **BuildUnityContainer** metoda jego zawartość następującym kodem, który rejestruje kontrolera magazynu i usługi magazynu.

    (Fragment - kodu *ASP.NET zależności iniekcji laboratorium - Ex01 - magazynu rejestru kontrolera i usługi*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — uruchamianie aplikacji

W ramach tego zadania należy uruchomić aplikację, aby sprawdzić, czy teraz może być załadowany po dołączeniu Unity.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji, aplikacja powinna teraz załadować bez wyświetlania wszelkie komunikaty o błędach.

    ![Aplikacja uruchamiana za pomocą iniekcji zależności](aspnet-mvc-4-dependency-injection/_static/image6.png "aplikacja uruchamiana za pomocą iniekcji zależności")

    *Aplikacja uruchamiana za pomocą iniekcji zależności*
2. Przejdź do **/magazynu**. To spowoduje wywołanie **StoreController**, który został utworzony przy użyciu **Unity**.

    ![Magazyn utworów muzycznych MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC utworów muzycznych magazynu")

    *Magazyn utworów muzycznych MVC*
3. Zamknij przeglądarkę.

W poniższych ćwiczeniach dowiesz się, jak rozszerzyć zakresu iniekcji zależności, aby użyć go w widoków MVC ASP.NET i filtrów akcji.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Ćwiczenie 2: Wstrzyknięcie widoku

W tym ćwiczeniu dowiesz się, jak używać iniekcji zależności w widoku z nowych funkcji programu ASP.NET MVC 4 integracji Unity. Aby to zrobić, spowodują wywołanie usługi niestandardowej wewnątrz widok Przeglądaj magazynu, który będzie widoczny komunikat i obraz poniżej.

Następnie zostanie integracji projektu z Unity i utworzyć niestandardowy mechanizm rozpoznawania zależności wstrzyknięcia zależności.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Zadanie 1 — Tworzenie widoku, który wykorzystuje usługę

W ramach tego zadania spowoduje utworzenie widoku, który wykonuje wywołanie usługi, aby wygenerować nową zależność. Usługa składa się proste usługą obsługi wiadomości zawartych w tym rozwiązaniu.

1. Otwórz **rozpocząć** rozwiązania, znajdujących się w **wstrzyknięcie Source\Ex02 View\Begin** folderu. W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.

    1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
    2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
    3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

    > [!NOTE]
    > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
    > 
    > Aby uzyskać więcej informacji, zobacz ten artykuł: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Obejmują **MessageService.cs** i **IMessageService.cs** klas znajdujących się w **źródła \Assets** folderu w **/usług**. Aby to zrobić, kliknij prawym przyciskiem myszy **usług** i wybierz polecenie **Dodaj istniejący element**. Przejdź do lokalizacji plików i włączyć je.

    ![Dodawanie usługi wiadomości i interfejs usługi](aspnet-mvc-4-dependency-injection/_static/image8.png "Dodawanie usługi wiadomości i interfejs usługi")

    *Dodawanie usługi wiadomości i interfejs usługi*

    > [!NOTE]
    > **IMessageService** interfejs definiuje dwie właściwości implementowane przez **MessageService** klasy. Te właściwości —**komunikat** i **ImageUrl**-przechowywać wiadomość i adres URL obrazu, który będzie wyświetlany.
3. Utwórz folder **/strony** w projekcie głównym folderu, a następnie dodaj istniejącej klasy **MyBasePage.cs** z **Source\Assets**. Strony podstawowej, który będzie dziedziczyć ma następującą strukturę.

    ![Folder stron](aspnet-mvc-4-dependency-injection/_static/image9.png "folder stron")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Otwórz **Browse.cshtml** wyświetlić z **/widoków/Store** folderu i przydzielić mu dziedziczyć **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. W **Przeglądaj** wyświetlić, dodaj wywołanie do **MessageService** do wyświetlania obrazu i wiadomości pobierane przez usługę.
(C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Zadanie 2 — w tym moduł rozpoznawania zależności niestandardowych i aktywatora strony widoku niestandardowego

W poprzednim zadaniu możesz wprowadzić nowe zależności w widoku do wykonywania w nim wywołanie usługi. Teraz, rozwiąże ten zależności dzięki implementacji interfejsów iniekcji zależności MVC ASP.NET **IViewPageActivator** i **elementu IDependencyResolver**. W rozwiązaniu będzie zawierać implementację **elementu IDependencyResolver** który zajmuje się pobieranie usługi przy użyciu platformy Unity. Następnie będzie zawierać innej implementacji niestandardowych **IViewPageActivator** interfejs, który rozwiąże tworzenia widoków.

> [!NOTE]
> Od platformy ASP.NET MVC 3 implementacja iniekcji zależności miał uproszczone interfejsy rejestrowania usług. **Elementu IDependencyResolver** i **IViewPageActivator** są częścią funkcje platformy ASP.NET MVC 3 iniekcji zależności.
> 
> **-Elementu IDependencyResolver** poprzedniej IMvcServiceLocator zastępuje interfejs. Implementacje elementu IDependencyResolver musi zwrócić wystąpienie usługi lub kolekcji usługi.
>
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator** interfejs zapewnia bardziej precyzyjną kontrolę nad jak utworzyć widok strony wystąpienia za pomocą iniekcji zależności. Klasy, które implementują **IViewPageActivator** interfejsu można utworzyć wystąpienia widoku przy użyciu informacji o kontekście.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. Utwórz /**fabryki** folderu w folderze głównym projektu.
2. Obejmują **CustomViewPageActivator.cs** do rozwiązania z **/źródeł/zasoby/** do **fabryki** folderu. W tym celu kliknij prawym przyciskiem myszy **/Factories** folderu, wybierz opcję **Dodaj | Istniejący element** , a następnie wybierz **CustomViewPageActivator.cs**. Ta klasa implementuje **IViewPageActivator** interfejs do przechowywania kontenera Unity.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** jest odpowiedzialny za zarządzanie tworzenia widoku przy użyciu kontenera Unity.
3. Obejmują **UnityDependencyResolver.cs** plik z **/źródeł/zasoby** do **/Factories** folderu. W tym celu kliknij prawym przyciskiem myszy **/Factories** folderu, wybierz opcję **Dodaj | Istniejący element** , a następnie wybierz **UnityDependencyResolver.cs** pliku.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver** klasy jest niestandardowej klasy DependencyResolver for Unity. Jeśli nie można znaleźć usługi kontenera platformy Unity, jest invocated podstawowego rozpoznawania nazw.

W poniższych zadań zarówno implementacje zostanie zarejestrowany umożliwi poznanie lokalizacji usług i widoki modelu.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Zadanie 3 — rejestrowanie iniekcji zależności w kontenerze Unity

To zadanie należy umieścić wszystkie poprzednie rzeczy, dzięki czemu będą działać iniekcji zależności.

Do chwili rozwiązania zawiera następujące elementy:

- A **Przeglądaj** widoku, który dziedziczy z **MyBaseClass** i wykorzystuje **MessageService**.
- Klasa pośrednicząca -**MyBaseClass**-mający iniekcji zależności zgłoszonych do interfejsu usługi.
- Usługa - **MessageService** - i jego interfejs **IMessageService**.
- Moduł rozpoznawania zależności niestandardowych dla Unity - **UnityDependencyResolver** — która zajmuje się usługa pobierania.
- Aktywatora strony widoku - **CustomViewPageActivator** -który tworzy stronę.

Iniekcję **Przeglądaj** widoku, możesz teraz zarejestruje mechanizmu rozpoznawania zależności niestandardowych w kontenerze Unity.

1. Otwórz **Bootstrapper.cs** pliku.
2. Zarejestrowanie wystąpienia **MessageService** w kontenerze Unity zainicjować usługi:

    (Fragment - kodu *usługi wiadomości rejestru laboratorium - Ex02 - iniekcji zależności ASP.NET*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Dodaj odwołanie do **MvcMusicStore.Factories** przestrzeni nazw.

    (Fragment - kodu *ASP.NET zależności iniekcji laboratorium - Ex02 - fabryki Namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Zarejestruj **CustomViewPageActivator** jako aktywatora strony widoku w kontenerze Unity:

    (Fragment - kodu *ASP.NET zależności iniekcji laboratorium - Ex02 - rejestru CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. Zamień na wystąpienie ASP.NET MVC 4 mechanizmem rozpoznawania zależności **UnityDependencyResolver**. Aby to zrobić, Zamień **Initialise** metody zawartość następującym kodem:

    (Fragment - kodu *rozpoznawania zależności aktualizacji laboratorium - Ex02 - iniekcji zależności ASP.NET*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC udostępnia klasę mechanizmu rozpoznawania zależności domyślne. Aby pracować z mechanizmów rozpoznawania zależności niestandardowego, który został utworzony dla unity, ten mechanizm rozpoznawania ma zostać zamieniona.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — uruchamianie aplikacji

To zadanie uruchomi aplikację, aby sprawdzić, czy przeglądarka magazynu wykorzystuje usługę i przedstawia obrazu i pobrać komunikat:

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Kliknij przycisk **skale** w Genres Menu i zobacz, jak **MessageService** zostało dodane do widoku i załadować komunikat powitalny i obrazu. W tym przykładzie mamy wprowadzania do &quot; **skale**&quot;:

    ![Magazynu utworów muzycznych MVC — Wyświetl iniekcji](aspnet-mvc-4-dependency-injection/_static/image10.png "magazynu utworów muzycznych MVC — Wyświetl iniekcji")

    *Magazynu utworów muzycznych MVC — Wyświetl iniekcji*
3. Zamknij przeglądarkę.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Ćwiczenie 3: Wstrzyknięcie filtry akcji

W poprzednich laboratorium praktycznego **filtry akcji niestandardowych** mający doświadczenie z filtrów, dostosowywanie i iniekcji. W tym ćwiczeniu dowiesz się, jak wstrzyknięcie filtrów z iniekcji zależności za pomocą kontenera Unity. W tym celu zostaną dodane do rozwiązania magazynu utworów muzycznych filtr akcji niestandardowej, który będzie śledzić działania witryny.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Zadanie 1 — w tym filtru śledzenia w rozwiązaniu

W tym zadaniu będą zawarte w magazynie utworów muzycznych filtr akcji niestandardowych w celu śledzenia zdarzeń. Jako filtr akcji niestandardowej pojęcia już są traktowane w poprzednim laboratorium &quot;filtry akcji niestandardowych&quot;, będą tylko zawiera klasy filtr z folderu zasoby tego laboratorium, a następnie utworzyć dostawcę filtru dla Unity:

1. Otwórz **rozpocząć** rozwiązania, znajdujących się w **Source\Ex03 - wstrzyknięcie Filter\Begin akcji** folderu. W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.

    1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
    2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
    3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

    > [!NOTE]
    > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
    > 
    > Aby uzyskać więcej informacji, zobacz ten artykuł: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Obejmują **TraceActionFilter.cs** plik z **/źródeł/zasoby** do **/filtry** folderu.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Ten filtr akcji niestandardowej wykonuje śledzenie na platformie ASP.NET. Możesz sprawdzić &quot;filtrów dynamicznych akcji i ASP.NET MVC 4 lokalne&quot; laboratorium więcej odwołania.
3. Dodaj pustą klasę **FilterProvider.cs** do projektu w folderze   **/filtry.**
4. Dodaj **System.Web.Mvc** i **Microsoft.Practices.Unity** przestrzeni nazw w **FilterProvider.cs**.

    (Fragment - kodu *ASP.NET zależności iniekcji laboratorium - Ex03 - Filtr dostawcy Dodawanie obszarów nazw*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Zmień klasę dziedziczyć **IFilterProvider** interfejsu.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Dodaj **IUnityContainer** właściwości w **FilterProvider** klasy, a następnie utwórz konstruktora klasy, aby przypisać kontenera.

    (Fragment - kodu *Konstruktor dostawcy filtru laboratorium - Ex03 - iniekcji zależności ASP.NET*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > Konstruktor klasy dostawcy filtru nie jest tworzone **nowe** obiektów wewnątrz. Kontener jest przekazywana jako parametr, a zależność jest obliczany przez Unity.
7. W **FilterProvider** klasy, zaimplementuj metodę **GetFilters** z **IFilterProvider** interfejsu.

    (Fragment - kodu *ASP.NET zależności iniekcji laboratorium - Ex03 - Filtr dostawcy GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Zadanie 2 — rejestrowania i włączanie filtru

To zadanie spowoduje włączenie śledzenia lokacji. Aby to zrobić, zarejestruj filtr w **Bootstrapper.cs BuildUnityContainer** metodę, aby rozpocząć śledzenie:

1. Otwórz **Web.config** znajdujący się w głównym projektu i Włącz śledzenie śledzenia w grupie System.Web.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Otwórz **Bootstrapper.cs** w katalogu głównym projektu.
3. Dodaj odwołanie do **MvcMusicStore.Filters** przestrzeni nazw.

    (Fragment - kodu *Dodawanie przestrzeni nazw programu ASP.NET zależności iniekcji laboratorium - Ex03 - inicjującego*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Wybierz **BuildUnityContainer** — metoda i rejestrowanie filtru w kontenerze Unity. Należy zarejestrować dostawcę filtru, a także filtru akcji.

    (Fragment - kodu *ASP.NET zależności iniekcji laboratorium - Ex03 - rejestru FilterProvider i ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3 — uruchamianie aplikacji

W tym zadaniu zostanie Uruchom aplikację i przetestować, czy filtr akcji niestandardowej jest śledzenie działania:

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Kliknij przycisk **skale** w Genres Menu. Jeśli chcesz, można przejść do genres więcej.

    ![Muzyka magazynu](aspnet-mvc-4-dependency-injection/_static/image11.png "magazynu utworów muzycznych")

    *Music Store*
3. Przejdź do **/Trace.axd** aby zobaczyć śledzenie aplikacji, a następnie kliknij przycisk **Wyświetl szczegóły**.

    ![Dziennik śledzenia aplikacji](aspnet-mvc-4-dependency-injection/_static/image12.png "dziennik śledzenia aplikacji")

    *Dziennik śledzenia aplikacji*

    ![Aplikacja śledzenia — informacje dotyczące żądania](aspnet-mvc-4-dependency-injection/_static/image13.png "śledzenia aplikacji — szczegóły żądania")

    *Aplikacja śledzenia — informacje dotyczące żądania*
4. Zamknij przeglądarkę.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Wykonując tego laboratorium Hands-On ma przedstawiono sposób używania iniekcji zależności w programie ASP.NET MVC 4 integrując Unity przy użyciu pakietu NuGet. Aby to osiągnąć, użyto iniekcji zależności wewnątrz kontrolerów, widoków i filtrów akcji.

Zostały uwzględnione następujące kwestie:

- Funkcje iniekcji zależności programu ASP.NET MVC 4
- Integracja platformy Unity za pośrednictwem pakietu NuGet Unity.Mvc3
- Iniekcji zależności w kontrolerów
- Iniekcji zależności w widokach
- Iniekcja zależności filtry akcji

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji  **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; *programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK*&quot;.
2. Polecenie **teraz zainstalować**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Instalowanie programu Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "instalacji programu Visual Studio Express")

    *Instalowanie programu Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Akceptowanie umowy licencyjnej*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *VS Express for Web kafelka*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Dodatek B: korzystania z wstawek kodu

Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki. Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.

![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](aspnet-mvc-4-dependency-injection/_static/image19.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*

***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragment (bez spacji i łączniki).
3. Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.
4. Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

![Rozpocznij wpisywanie nazwy fragment](aspnet-mvc-4-dependency-injection/_static/image20.png "zacznij wpisywać nazwę wstawki programu")

*Rozpocznij wpisywanie nazwy fragment kodu*

![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](aspnet-mvc-4-dependency-injection/_static/image21.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")

*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

![Ponownie naciśnij klawisz Tab i fragment rozwinie](aspnet-mvc-4-dependency-injection/_static/image22.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")

*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*

***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)*** 1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.

1. Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.
2. Wybierz odpowiedni fragment z listy, klikając go.

![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](aspnet-mvc-4-dependency-injection/_static/image23.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")

*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*

![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-dependency-injection/_static/image24.png "wybierz odpowiedni fragment z listy, klikając go")

*Wybierz odpowiedni fragment z listy, klikając go*
