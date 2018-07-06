---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Wstrzykiwanie zależności platformy ASP.NET MVC 4 | Dokumentacja firmy Microsoft
author: rick-anderson
description: 'Uwaga: W tym laboratorium praktyczne przyjęto założenie, że masz podstawową wiedzę na temat filtrów platformy ASP.NET MVC i platformy ASP.NET MVC 4. Jeśli nie używasz filtrów platformy ASP.NET MVC 4 przed możemy "rec"...'
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 44c8f2055fb62d589e874683cbf43eed87a8c447
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812346"
---
# <a name="aspnet-mvc-4-dependency-injection"></a>Wstrzykiwanie zależności platformy ASP.NET MVC 4

Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)

[Pobierz Camp Web szkolenia Kit](https://aka.ms/webcamps-training-kit)

W tym laboratorium praktyczne przyjęto założenie, masz podstawową wiedzę na temat **platformy ASP.NET MVC** i **filtrów platformy ASP.NET MVC 4**. Jeśli nie używasz **filtrów platformy ASP.NET MVC 4** , zalecamy zapoznać się z **programu ASP.NET MVC niestandardowe filtry akcji** warsztatów.

> [!NOTE]
> Wszystkie przykładowy kod i fragmenty są uwzględnione w sieci Web Camp zestaw szkoleniowy, pod [wersji Microsoft — w sieci Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specyficzne dla tego laboratorium znajduje się w temacie [wstrzykiwanie zależności programu ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).

W **programowanie zorientowane na obiekt** paradigm, obiekty współpracują ze sobą w model współpracy w przypadku, gdy istnieją współautorów i konsumentów. Oczywiście ten model komunikacji generuje zależności między obiektami i składników, staje się trudne do zarządzania, gdy zwiększa złożonością.

![Klasa zależności i modelowanie złożoności](aspnet-mvc-4-dependency-injection/_static/image1.png "klasy zależności i modelowanie złożoności")

*Klasa zależności i złożoność modelu*

Prawdopodobnie wiesz o **wzorzec fabryki** i oddzielenie interfejs i wdrożenia przy użyciu usług, których obiekty klienta często są odpowiedzialne za lokalizacji usługi.

Wzorzec wstrzykiwanie zależności jest określonej implementacji Inwersja kontroli. **Inwersja kontroli (IoC)** oznacza, że obiektów nie należy tworzyć inne obiekty, które korzystają z do wykonania swojej pracy. Zamiast tego otrzymują obiektów, które są im niezbędne ze źródła zewnętrznego (na przykład plik konfiguracyjny xml).

**Wstrzykiwanie zależności (DI)** oznacza, że jest zazwyczaj wykonywane bez interwencji obiektu przez składnik framework, który przekazuje parametry konstruktora i ustaw właściwości.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>Wzorzec projektowy (DI) wstrzykiwania zależności

Na wysokim poziomie celem wstrzykiwanie zależności jest to, że klasa klienta (np. *golfer*) wymaga coś, co spełnia interfejsu (np. *IClub*). Zależy to konkretny typ (np. *WedgeClub WoodClub, IronClub,* lub *PutterClub*), ktoś inny do obsługi, który chce (np. jest dobrą *caddy*). Mechanizm rozpoznawania zależności na platformie ASP.NET MVC umożliwia rejestrowanie logikę zależności gdzie indziej (np. kontenerze lub *zbioru kluby*).

![Diagram iniekcji zależności](aspnet-mvc-4-dependency-injection/_static/image2.png "ilustracji wstrzykiwanie zależności")

*Wstrzykiwanie zależności - odpowiednio Golf*

Zalety korzystania z wzorca wstrzykiwanie zależności oraz Inwersja kontroli są następujące:

- Zmniejsza sprzężenia klas
- Zwiększa, ponowne użycie kodu
- Zwiększa łatwości utrzymania kodu
- Zwiększa testowania aplikacji

> [!NOTE]
> Wstrzykiwanie zależności czasem jest porównywana z abstrakcyjne wzorzec projektowy fabryki, ale istnieje niewielka różnica między oba podejścia. DI ma strukturę pracę za modułem rozwiązać zależności w wywołaniu fabryk i zarejestrowane usługi.


Teraz, gdy już rozumiesz wzorzec iniekcji zależności, nauczysz w całym tym środowisku laboratoryjnym ją zastosować w technologii ASP.NET MVC 4. Zostanie uruchomiona, przy użyciu iniekcji zależności w **kontrolerów** obejmujący usługi dostępu do bazy danych. Następnie zostaną zastosowane wstrzykiwanie zależności do **widoków** do korzystania z usługi i wyświetlania informacji. Na koniec rozszerzenie DI do platformy ASP.NET MVC 4 filtrów, wprowadzanie filtru akcji niestandardowej w rozwiązaniu.

W tym laboratorium praktyczne dowiesz się jak:

- Integracja platformy ASP.NET MVC 4 przy użyciu aparatu Unity do wstrzykiwania zależności za pomocą pakietów NuGet
- Iniekcji zależności wewnątrz kontroler składnika ASP.NET MVC
- Iniekcji zależności w widoku programu ASP.NET MVC
- Iniekcji zależności wewnątrz filtru akcji programu ASP.NET MVC

> [!NOTE]
> W tym laboratorium to przy użyciu pakietu NuGet Unity.Mvc3 do rozpoznawania zależności, ale istnieje możliwość dostosowania dowolnej architektury wstrzykiwanie zależności do pracy z platformy ASP.NET MVC 4.


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

Jeśli nie jesteś zaznajomiony z fragmentów kodu w usłudze Visual Studio i chcesz dowiedzieć się, jak z nich korzystać, możesz zapoznać się z dodatku z tego dokumentu &quot; [dodatek B: za pomocą wstawek kodu programu](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

W tym laboratorium praktyczne składa się przez następujących czynnościach:

1. [Ćwiczenie 1: Wstawianie kontrolera](#Exercise1)
2. [Ćwiczenie 2: Wstawianie widoku](#Exercise2)
3. [Ćwiczenie 3: Wstawianie filtry](#Exercise3)

> [!NOTE]
> Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązania, należy uzyskać po ukończeniu ćwiczenia. Jeśli potrzebujesz dodatkowej pomocy ćwiczeń opisanych w dalszej, można użyć tego rozwiązania jako wskazówki.


Szacowany czas do ukończenia tego laboratorium: **30 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Ćwiczenie 1: Wstawianie kontrolera

W tym ćwiczeniu dowiesz się, jak używać wstrzykiwanie zależności w kontrolery ASP.NET MVC, integrując Unity przy użyciu pakietu NuGet. Z tego powodu będzie zawierać usług na kontrolerach MvcMusicStore do oddzielania logika dostępu do danych. Usługi spowoduje utworzenie nowych zależności w Konstruktorze kontrolera, który zostanie rozwiązany przy użyciu iniekcji zależności za pomocą **Unity**.

To podejście pokazano sposób generowania mniej sprzężone aplikacje, które są bardziej elastyczne i ułatwia utrzymywanie i testowanie. Ponadto dowiesz sposobu integrowania programu ASP.NET MVC przy użyciu aparatu Unity.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>O usłudze StoreManager

MVC Music Store, podany w rozwiązaniu Rozpocznij teraz obejmuje usługi, która zarządza danymi Store kontroler o nazwie **StoreService**. Poniżej można znaleźć implementacji usługi Store. Należy pamiętać, że wszystkie metody zwracają modelowania jednostek.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** BEGIN teraz wykorzystuje rozwiązanie **StoreService**. Wszystkie odwołania zostały usunięte z **StoreController**, a teraz możliwość modyfikowania bieżącego dostawcy dostępu do danych bez wprowadzania zmian w dowolnej metody, która zużywa **StoreService**.

Można znaleźć poniżej **StoreController** implementacja ma zależność **StoreService** wewnątrz konstruktora klasy.

> [!NOTE]
> Zależność wprowadzone w tym ćwiczeniu jest powiązany z **Inwersja kontroli** (IoC).
> 
> **StoreController** odbiera konstruktora klasy **IStoreService** parametr typu, który jest niezbędne do wykonania wywołania usługi wewnątrz klasy. Jednak **StoreController** nie zawiera implementacji konstruktora domyślnego (z bez parametrów), że każdy kontroler musi mieć do pracy z platformą ASP.NET MVC.
> 
> Można rozpoznać zależności, kontroler ma zostać utworzona przez fabrykę abstrakcyjny (klasę, która zwraca dowolnego obiektu określonego typu).


[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Gdy klasa próbuje utworzyć StoreController bez wysyłania obiektu usługi, ponieważ nie ma żadnych konstruktora bez parametrów, które zostały zgłoszone, otrzymają komunikat o błędzie.


<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Zadanie 1. uruchamianie aplikacji

W tym zadaniu uruchomisz aplikację Begin, która obejmuje usługę do kontrolera Store, która oddziela dostęp do danych od logiki aplikacji.

Po uruchomieniu aplikacji, zostanie wyświetlony wyjątek, ponieważ usługa kontrolera nie jest przekazywany jako parametr domyślny:

1. Otwórz **rozpocząć** rozwiązanie znajduje się w **wprowadza Source\Ex01 Controller\Begin**.

   1. Musisz pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. Naciśnij klawisz **klawiszy Ctrl + F5** Aby uruchomić aplikację bez debugowania. Otrzymasz komunikat o błędzie &quot; **nie konstruktora bez parametrów, które zostały zdefiniowane dla tego obiektu**&quot;:

    ![Wystąpił błąd podczas uruchamiania aplikacji rozpocząć MVC ASP.NET](aspnet-mvc-4-dependency-injection/_static/image3.png "wystąpił błąd podczas uruchamiania aplikacji rozpocząć MVC ASP.NET")

    *Wystąpił błąd podczas uruchamiania aplikacji rozpocząć MVC ASP.NET*
3. Zamknij przeglądarkę.

W poniższych krokach będzie działać na rozwiązanie Store utworów muzycznych na iniekcji zależności, czego potrzebuje tego kontrolera.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Zadanie 2 — w tym aparatu Unity w rozwiązaniu MvcMusicStore

W ramach tego zadania będzie zawierać **Unity.Mvc3** pakietu NuGet z rozwiązaniem.

> [!NOTE]
> Pakiet Unity.Mvc3 był przeznaczony dla platformy ASP.NET MVC 3, ale jest w pełni zgodny z platformy ASP.NET MVC 4.
> 
> Unity to kontener iniekcji zależności niewielka, rozszerzalna opcjonalna pomoc techniczna dla wystąpienia i wpisz przejmowanie. Jest kontener ogólnego zastosowania do użycia w aplikacji .NET dowolnego typu. Umożliwia wspólne funkcje w mechanizmy iniekcji zależności, w tym: Tworzenie obiektów, abstrakcji wymagania za pośrednictwem zależności środowiska uruchomieniowego i elastyczność, poprzez opóźnienie Konfiguracja składnika, do kontenera.


1. Zainstaluj **Unity.Mvc3** pakietu NuGet w **MvcMusicStore** projektu. Aby to zrobić, otwórz **Konsola Menedżera pakietów** z **widoku** | **inne Windows**.
2. Uruchom następujące polecenie.

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Instalowanie pakietu NuGet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Instalowanie pakietu NuGet Unity.Mvc3")

    *Instalowanie pakietu NuGet Unity.Mvc3*
3. Gdy **Unity.Mvc3** pakiet jest zainstalowany, Eksploruj pliki i foldery, które automatycznie dodaje w celu uproszczenia konfiguracji aparatu Unity.

    ![Zainstalowany pakiet Unity.Mvc3](aspnet-mvc-4-dependency-injection/_static/image5.png "zainstalowany pakiet Unity.Mvc3")

    *Zainstalowany pakiet Unity.Mvc3*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-applicationstart"></a>Zadanie 3. rejestrowanie Unity w aplikacji Global.asax.cs\_Start

To zadanie zaktualizuje **aplikacji\_Start** metoda znajduje się w **Global.asax.cs** wywołać inicjatora programu inicjującego Unity, a następnie, zaktualizować, rejestrowanie pliku inicjującego Usługi i kontroler użyje do wstrzykiwania zależności.

1. Teraz będzie podpiąć program inicjujący, czyli pliku który inicjuje kontenera aparatu Unity i mechanizmu rozpoznawania zależności. Aby to zrobić, otwórz **Global.asax.cs** i Dodaj następujący wyróżniony kod w ramach **aplikacji\_Start** metody.

    (Code Snippet — *laboratorium iniekcji zależności platformy ASP.NET — Ex01 — inicjowanie aparatu Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Otwórz **Bootstrapper.cs** pliku.
3. Uwzględnić następujące przestrzenie nazw: **MvcMusicStore.Services** i **MusicStore.Controllers**.

    (Code Snippet — *ASP.NET zależności iniekcji laboratorium — Ex01 — program inicjujący Instalatora programu Dodawanie przestrzenie nazw*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Zastąp **BuildUnityContainer** metoda jego zawartości za pomocą następujący kod, który rejestruje Store kontrolera i Store Service.

    (Code Snippet — *ASP.NET zależności iniekcji laboratorium - Ex01 - Store Zarejestruj kontroler i usługa*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — uruchamianie aplikacji

To zadanie uruchomi aplikację, aby sprawdzić, czy teraz może być załadowany po tym aparatu Unity.

1. Naciśnij klawisz **F5** do uruchamiania aplikacji, aplikacja powinna teraz załadować bez pokazywania wszelkie komunikaty o błędach.

    ![Uruchomienie aplikacji przy użyciu iniekcji zależności](aspnet-mvc-4-dependency-injection/_static/image6.png "uruchamiania aplikacji przy użyciu iniekcji zależności")

    *Uruchomienie aplikacji przy użyciu iniekcji zależności*
2. Przejdź do **/Store**. Spowoduje to wywołanie **StoreController**, który został utworzony przy użyciu **Unity**.

    ![MVC Music Store](aspnet-mvc-4-dependency-injection/_static/image7.png "MVC Music Store")

    *MVC Music Store*
3. Zamknij przeglądarkę.

W poniższym ćwiczeniach dowiesz się, jak rozszerzyć zakres wstrzykiwanie zależności z niej korzystać w widoków MVC platformy ASP.NET i filtrów akcji.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Ćwiczenie 2: Wstawianie widoku

W tym ćwiczeniu dowiesz się, jak używać wstrzykiwanie zależności w widoku z nowych funkcji programu ASP.NET MVC 4 do integracji środowiska Unity. Aby to zrobić, będzie wywoływać to usługa niestandardowa wewnątrz Store Przeglądaj widoku, który wyświetli komunikat i obraz poniżej.

Następnie zostanie zintegrować przy użyciu aparatu Unity i utworzyć niestandardowy mechanizm rozpoznawania zależności iniekcji zależności.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Zadanie 1 — Tworzenie widoku, który korzysta z usługi

W tym zadaniu utworzysz widok, który wykonuje wywołanie usługi, aby wygenerować nowy zależności. Usługi polega na prostą usługę komunikatów zawartych w tym rozwiązaniu.

1. Otwórz **rozpocząć** rozwiązanie znajduje się w **wprowadza Source\Ex02 View\Begin** folderu. W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
      > 
      > Aby uzyskać więcej informacji znajduje się w artykule: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Obejmują **MessageService.cs** i **IMessageService.cs** klas znajdujących się w **źródła \Assets** folderu w **/usług**. Aby to zrobić, kliknij prawym przyciskiem myszy **usług** i wybierz polecenie **Dodaj istniejący element**. Przejdź do lokalizacji plików i je uwzględnić.

    ![Dodawanie wiadomości SMS i interfejsu usługi](aspnet-mvc-4-dependency-injection/_static/image8.png "dodawanie wiadomości SMS i interfejsu usługi")

    *Dodawanie usługi wiadomości i interfejsu usługi*

    > [!NOTE]
    > **IMessageService** interfejs definiuje dwie właściwości implementowane przez **MessageService** klasy. Te właściwości -**komunikat** i **ImageUrl**— przechowywanie komunikat i adres URL obrazu, który będzie wyświetlany.
3. Utwórz folder **/strony** w projekcie folder główny, a następnie Dodaj istniejącą klasę **MyBasePage.cs** z **Source\Assets**. Strony podstawowej, który będzie dziedziczyć ma następującą strukturę.

    ![Folder stron](aspnet-mvc-4-dependency-injection/_static/image9.png "folder stron")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Otwórz **Browse.cshtml** z widoku **/widoków/Store** folderu i przypisz ją dziedziczyć **MyBasePage.cs**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. W **Przeglądaj** wyświetlić, dodać wywołanie **MessageService** do wyświetlania obrazu i komunikat, który został pobrany przez usługę.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Zadanie 2 — w tym mechanizm rozpoznawania zależności niestandardowej i aktywatora strony widoku niestandardowego

W poprzednim zadaniu wprowadzony się nowych zależności wewnątrz widoku, aby wykonać wywołanie usługi wewnątrz niego. Teraz, zostanie rozwiązany tej zależności poprzez implementację interfejsów wstrzykiwanie zależności MVC ASP.NET **IViewPageActivator** i **elementu IDependencyResolver**. W rozwiązaniu będzie zawierać implementację **elementu IDependencyResolver** który poradzi sobie z pobierania usługi za pomocą aparatu Unity. Następnie będzie zawierać inny niestandardową implementację **IViewPageActivator** interfejs, który rozwiąże tworzenia widoków.

> [!NOTE]
> Od platformy ASP.NET MVC 3 wykonania na iniekcji zależności miał uproszczone interfejsów, które można zarejestrować usługi. **Elementu IDependencyResolver** i **IViewPageActivator** wchodzą w skład funkcji ASP.NET MVC 3 do wstrzykiwania zależności.
> 
> **-Elementu IDependencyResolver** interfejsu zastępuje IMvcServiceLocator poprzedniego. Implementacje elementu IDependencyResolver musi zwrócić wystąpienie usługi lub kolekcję usługi.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-IViewPageActivator** interfejs zapewnia bardziej precyzyjną kontrolę nad jak widoku strony są tworzone za pomocą iniekcji zależności. Klasy, które implementują **IViewPageActivator** interfejsu można utworzyć wystąpienia widoku przy użyciu informacji o kontekście.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]


1. Utwórz /**fabryk** folderu w folderze głównym projektu.
2. Obejmują **CustomViewPageActivator.cs** do rozwiązania z **/źródła/zasobów/** do **fabryk** folderu. Aby to zrobić, kliknij prawym przyciskiem myszy **/Factories** folderu, wybierz **Dodaj | Istniejący element** , a następnie wybierz **CustomViewPageActivator.cs**. Ta klasa implementuje **IViewPageActivator** interfejs do przechowywania kontenera aparatu Unity.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** jest odpowiedzialny za zarządzanie tworzenia widoku przy użyciu kontenerów aparatu Unity.
3. Obejmują **UnityDependencyResolver.cs** plik wchodzącej w skład **/źródła/zasoby** do **/Factories** folderu. Aby to zrobić, kliknij prawym przyciskiem myszy **/Factories** folderu, wybierz **Dodaj | Istniejący element** , a następnie wybierz **UnityDependencyResolver.cs** pliku.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > **UnityDependencyResolver** klasa jest niestandardowej klasy DependencyResolver dla aparatu Unity. Jeśli usługa nie zostanie znaleziony w kontenerze aparatu Unity, jest invocated podstawowego rozpoznawania nazw.

Następujące zadanie zostanie zarejestrowana obu implementacjach umożliwiające modelu znać lokalizację usługi i widoki.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Zadanie 3 — rejestrowanie na potrzeby wstrzykiwanie zależności w kontenerze aparatu Unity

W tym zadaniu należy umieścić wszystkie poprzednie elementy razem w celu zapewnienia wstrzykiwanie zależności w pracy.

Do chwili obecnej rozwiązanie ma następujące elementy:

- A **Przeglądaj** widoku, który dziedziczy z **MyBaseClass** i wykorzystuje **MessageService**.
- Klasa pośrednicząca -**MyBaseClass**-zawierający wstrzykiwanie zależności zadeklarowany dla interfejsu usługi.
- Usługa - **MessageService** — i jego interfejs **IMessageService**.
- Mechanizm rozpoznawania zależności niestandardowej dla aparatu Unity — **UnityDependencyResolver** — która zajmuje się pobieranie usługi.
- Aktywatora strony widoku - **CustomViewPageActivator** -tworząca stronę.

Aby wstawić **Przeglądaj** widoku teraz zarejestrujesz mechanizmu rozpoznawania zależności niestandardowej w kontenerze aparatu Unity.

1. Otwórz **Bootstrapper.cs** pliku.
2. Rejestrowanie wystąpienia **MessageService** w kontenerze aparatu Unity zainicjować usługi:

    (Code Snippet — *usługi wiadomości rejestru laboratorium — Ex02 — iniekcji zależności ASP.NET*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Dodaj odwołanie do **MvcMusicStore.Factories** przestrzeni nazw.

    (Code Snippet — *ASP.NET zależności iniekcji laboratorium - Ex02 - fabryk Namespace*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Zarejestruj **CustomViewPageActivator** jako aktywatora strony widoku w kontenerze aparatu Unity:

    (Code Snippet — *ASP.NET zależności iniekcji laboratorium — Ex02 — Zarejestruj CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. Zamień na wystąpienie platformy ASP.NET MVC 4 mechanizmem rozpoznawania zależności **UnityDependencyResolver**. Aby to zrobić, Zastąp **Initialise** metoda zawartość następującym kodem:

    (Code Snippet — *rozpoznawania zależności aktualizacji laboratorium — Ex02 — wstrzykiwanie zależności ASP.NET*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC udostępnia domyślną klasę mechanizmu rozpoznawania zależności. Aby pracować z mechanizmów rozpoznawania zależności niestandardowego, który został utworzony dla aparatu unity, ten mechanizm rozpoznawania ma zostać zastąpiony.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Zadanie 4 — uruchamianie aplikacji

To zadanie uruchomi aplikację, aby sprawdzić, czy przeglądarka Store wykorzystuje usługę i pokazuje obraz i komunikat pobrać:

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Kliknij przycisk **Rock** gatunki Menu i zobacz jak **MessageService** został wprowadzony w widoku, a następnie ładowane wiadomość powitalna i obraz. W tym przykładzie mamy wprowadzenie do &quot; **Rock**&quot;:

    ![MVC Music Store — widok iniekcji](aspnet-mvc-4-dependency-injection/_static/image10.png "MVC Music Store — iniekcji widoku")

    *MVC Music Store — iniekcji widoku*
3. Zamknij przeglądarkę.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Ćwiczenie 3: Wstawianie filtry akcji

W poprzednim laboratorium praktycznego **niestandardowe filtry akcji** mający doświadczenie z filtrów dostosowywania i iniekcji. W tym ćwiczeniu dowiesz się, jak wstawić filtry przy użyciu iniekcji zależności za pomocą kontenera aparatu Unity. Aby to zrobić, dodasz do rozwiązania Music Store filtr akcji niestandardowej, który będzie śledzić działania lokacji.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Zadanie 1 — w tym filtru śledzenia w rozwiązaniu

W tym zadaniu w Store utworów muzycznych będzie zawierać filtr akcję niestandardową do śledzenia zdarzeń. Jako filtr akcji niestandardowej pojęcia są już traktowane w poprzednim środowisku laboratoryjnym &quot;niestandardowe filtry akcji&quot;, będą po prostu Dołącz klasy filtr z folderu zasobów w tym laboratorium, a następnie utworzyć dostawcę filtru dla aparatu Unity:

1. Otwórz **rozpocząć** rozwiązanie znajduje się w **Source\Ex03 — wprowadzanie Filter\Begin akcji** folderu. W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
      > 
      > Aby uzyskać więcej informacji znajduje się w artykule: [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Obejmują **TraceActionFilter.cs** plik wchodzącej w skład **/źródła/zasoby** do **/filtry** folderu.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Ten filtr akcji niestandardowej wykonuje śledzenie na platformie ASP.NET. Możesz sprawdzić &quot;lokalnej platformy ASP.NET MVC 4 i filtrów dynamicznych akcji&quot; laboratorium na potrzeby więcej odwołań.
3. Dodać pustą klasę **FilterProvider.cs** do projektu w folderze   **/filtrów.**
4. Dodaj **System.Web.Mvc** i **Microsoft.Practices.Unity** przestrzeniach nazw **FilterProvider.cs**.

    (Code Snippet — *dostawcy filtru rozszerzeń ASP.NET zależności iniekcji laboratorium — Ex03 — Dodawanie przestrzenie nazw*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Wprowadź dziedziczyć z klasy **IFilterProvider** interfejsu.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Dodaj **IUnityContainer** właściwość **FilterProvider** klasy, a następnie utwórz konstruktora klasy, aby przypisać kontenera.

    (Code Snippet — *ASP.NET zależności iniekcji laboratorium — Ex03 — Filtr dostawcy Konstruktor*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > Konstruktora klasy dostawcy filtru są nietworzenie **nowe** wewnątrz. Kontener jest przekazywany jako parametr i zależność jest obliczany przez aparat Unity.
7. W **FilterProvider** klasy, implementować metody **GetFilters** z **IFilterProvider** interfejsu.

    (Code Snippet — *ASP.NET zależności iniekcji laboratorium — Ex03 — Filtr dostawcy GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Zadanie 2 — rejestrowania i włączanie filtru

W ramach tego zadania spowoduje włączenie śledzenia witryny. Aby to zrobić, zarejestruje filtru **Bootstrapper.cs BuildUnityContainer** metodę, aby rozpocząć śledzenie:

1. Otwórz **Web.config** znajduje się w katalogu głównym projektu i włączanie śledzenia śledzenia w grupie System.Web.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Otwórz **Bootstrapper.cs** w katalogu głównym projektu.
3. Dodaj odwołanie do **MvcMusicStore.Filters** przestrzeni nazw.

    (Code Snippet — *ASP.NET zależności iniekcji laboratorium — Ex03 — program inicjujący Instalatora programu Dodawanie przestrzenie nazw*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Wybierz **BuildUnityContainer** metody i rejestrowanie filtru w kontenerze aparatu Unity. Należy zarejestrować dostawcę filtru, a także filtru akcji.

    (Code Snippet — *ASP.NET zależności iniekcji laboratorium — Ex03 — Zarejestruj FilterProvider i ActionFilter*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Zadanie 3. uruchamianie aplikacji

W tym zadaniu zostanie Uruchom aplikację i przetestować, czy filtr akcji niestandardowej jest śledzenie aktywności:

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Kliknij przycisk **Rock** w Menu gatunki. Jeśli chcesz, można przeglądać gatunki więcej.

    ![Music Store](aspnet-mvc-4-dependency-injection/_static/image11.png "Music Store")

    *Music Store*
3. Przejdź do **/Trace.axd** aby zobaczyć śledzenie aplikacji strony, a następnie kliknij przycisk **Wyświetl szczegóły**.

    ![Dziennik śledzenia aplikacji](aspnet-mvc-4-dependency-injection/_static/image12.png "dziennika śledzenia aplikacji")

    *Dziennik śledzenia aplikacji*

    ![Śledzenie aplikacji — szczegóły żądania](aspnet-mvc-4-dependency-injection/_static/image13.png "śledzenia aplikacji — szczegóły żądania")

    *Śledzenie aplikacji — szczegóły żądania*
4. Zamknij przeglądarkę.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Przez ukończenie tego laboratorium praktyczne mają przedstawiono sposób korzystania wstrzykiwanie zależności w technologii ASP.NET MVC 4, integrując Unity przy użyciu pakietu NuGet. Aby to osiągnąć, użyto wstrzykiwanie zależności wewnątrz kontrolerów, widoki i filtrów akcji.

Następujące pojęcia zostały objęte pomocą techniczną:

- Funkcje programu ASP.NET MVC 4 wstrzykiwanie zależności
- Integracja aparatu Unity przy użyciu pakietu NuGet Unity.Mvc3
- Wstrzykiwanie zależności w kontrolerów
- Wstrzykiwanie zależności w widokach
- Wstrzykiwanie zależności filtrów akcji

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Możesz zainstalować **programu Microsoft Visual Studio Express 2012 for Web** lub inne &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe wskazówki ułatwiają kroki wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Zainstaluj program Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "zainstalować program Visual Studio Express")

    *Zainstaluj program Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Akceptowanie umowy licencyjnej*
5. Zaczekaj, aż do zakończenia procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu, a następnie zacznij pisać &quot; **VS Express**&quot;, następnie kliknij pozycję **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *VS Express for Web kafelka*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Dodatek B: korzystania z fragmentów kodu

Za pomocą wstawek kodu masz kod, czego potrzebujesz w zasięgu ręki. Dokument laboratorium informuje o dokładnie podczas korzystania z nich, jak pokazano na poniższej ilustracji.

![Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu](aspnet-mvc-4-dependency-injection/_static/image19.png "fragmenty kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu*

***Aby dodać wstawki kodu za pomocą klawiatury (tylko język C#)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Rozpocznij wpisywanie nazwy fragmentu kodu (bez spacji i łączniki).
3. Obejrzyj, jak technologia IntelliSense wyświetla zgodne z nazwami fragmenty kodu.
4. Wybierz prawidłowy fragment kodu lub pisz dalej do momentu wybrania debugowanej nazwy całego fragmentu kodu.
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

![Rozpocznij wpisywanie nazwy fragmentu kodu](aspnet-mvc-4-dependency-injection/_static/image20.png "Rozpocznij wpisywanie nazwy fragmentu kodu")

*Rozpocznij wpisywanie nazwy fragmentu kodu*

![Naciśnij klawisz Tab, aby zaznaczyć fragment wyróżnione](aspnet-mvc-4-dependency-injection/_static/image21.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragmentu kodu")

*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

![Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte](aspnet-mvc-4-dependency-injection/_static/image22.png "rozwinie ponownie naciśnij klawisz Tab i fragment kodu")

*Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte.*

***Aby dodać wstawki kodu za pomocą myszy (C#, Visual Basic i XML)*** 1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.

1. Wybierz **Wstaw fragment kodu** następuje **Moje fragmenty kodu**.
2. Wybierz odpowiedni fragment z listy, klikając go.

![Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu](aspnet-mvc-4-dependency-injection/_static/image23.png "kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu")

*Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu*

![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-dependency-injection/_static/image24.png "wybierz odpowiedni fragment z listy, klikając go")

*Wybierz odpowiedni fragment z listy, klikając go*
