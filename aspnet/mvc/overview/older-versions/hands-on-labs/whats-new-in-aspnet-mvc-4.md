---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: Co to jest nowe w programie ASP.NET MVC 4 | Dokumentacja firmy Microsoft
author: rick-anderson
description: ASP.NET MVC 4 to struktura służąca do tworzenia aplikacji sieci web skalowalnych, opartych na standardach za pomocą często używanych wzorów projektów oraz funkcji platform ASP.NET i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 485d2ba7a1274bbb36cfbcbca9322cecc8c8d77c
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306900"
---
# <a name="whats-new-in-aspnet-mvc-4"></a>Co to jest nowe w programie ASP.NET MVC 4

Przez [obozów sieci Web Team](https://twitter.com/webcamps)

[Pobierz obozów sieci Web uczenie Kit](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 to struktura służąca do tworzenia aplikacji sieci web skalowalnych, opartych na standardach za pomocą często używanych wzorów projektów oraz funkcji platform ASP.NET i .NET framework. Nowy, czwarty wersji platformy koncentruje się na ułatwiając tworzenie aplikacji sieci web urządzeń przenośnych.

Najpierw utwórz nowy projekt ASP.NET MVC 4 ma teraz szablon projektu aplikacji mobilnej, który umożliwia tworzenie aplikacji autonomicznej specjalnie dla urządzeń przenośnych. Ponadto program ASP.NET MVC 4 integruje się z jQuery Mobile za pośrednictwem pakietu NuGet jQuery.Mobile.MVC. jQuery Mobile to struktura na podstawie HTML5 umożliwiający projektowanie aplikacji sieci web, które są zgodne z wszystkich platform popularnym urządzeniu przenośnym, w tym Windows Phone, iPhone, Android i tak dalej. Jednak specjalizacji, należy ASP.NET MVC 4 umożliwia również obsługiwać różne widoki dla różnych urządzeń i podaj optymalizacje specyficzne dla urządzenia.

W tym laboratorium praktycznego rozpocznie z platformy ASP.NET MVC 4 &quot;aplikacji internetowej&quot; szablon projektu do tworzenia aplikacji galerii fotografii. Stopniowo umożliwiających ulepszenie aplikacji przy użyciu technologii jQuery Mobile i nowe funkcje platformy ASP.NET MVC 4, aby był zgodny z różnych urządzeń przenośnych i przeglądarki sieci web. Możesz także informacje dotyczące nowych przepisami kodu dla generowania kodu i jak ASP.NET MVC 4 pozwala na łatwiejsze do pisania metod asynchronicznych akcji przez zadanie obsługi&lt;ActionResult&gt; zwracanych typów.

> [!NOTE]
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [wersje Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Specyficzne dla tego laboratorium projektu jest dostępna na [What's New in formularzy sieci Web w programie ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym laboratorium praktycznego przedstawiono sposób:

- Zawiera ulepszenia do platformy ASP.NET MVC projektu szablony tym szablonie projektu aplikacji mobilnej
- Za pomocą atrybutu okienka ekranu HTML5 i zapytaniami multimediów CSS zwiększyć wyświetlania na urządzeniach przenośnych
- Ulepszenia progresywnego oraz tworzenie zoptymalizowanych pod kątem touch interfejsu użytkownika sieci web mieć jQuery Mobile
- Utwórz widoki specyficzne dla mobile
- Korzystanie ze składnika tym przełącznikiem widoku do przełączania się między widokami aplikacji mobilnych i klasycznych
- Utwórz asynchroniczne kontrolerów przy użyciu zadań pomocy technicznej

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Musi mieć następujące elementy do przygotowania tego laboratorium:

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek B](#AppendixB) instrukcje dotyczące sposobu jego instalacji).
- [ASP.NET MVC 4](../../../mvc4.md) (dołączone do instalacji programu Microsoft Visual Studio 2012)
- Emulator Windows Phone (objęte [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- Opcjonalne - [programu WebMatrix 2](https://www.microsoft.com/web/webmatrix/) z **iPhone Electric Plum symulatora** (tylko w przypadku wykonywania 3 używane do przeglądania aplikacji sieci web przy użyciu symulatora telefonu iPhone)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

W dokumencie laboratorium należy poinstruować do wstawienia bloków kodu. Dla wygody większość tego kodu jest dostępna w Visual Studio wstawki kodu, którym można z poziomu programu Visual Studio Aby uniknąć konieczności Dodaj ją ręcznie.

Aby zainstalować wstawki kodu:

1. Otwórz okno Eksploratora Windows i przejdź do laboratorium **Source\Setup** folderu.
2. Kliknij dwukrotnie **Setup.cmd** plik w tym folderze, aby zainstalować wstawki kodu programu Visual Studio.

Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek A:](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To laboratorium praktycznego obejmuje następujących czynnościach:

1. [Nowe szablony projektów programu ASP.NET MVC 4](#Exercise1)
2. [Tworzenie aplikacji sieci Web galerii fotografii](#Exercise2)
3. [Dodawanie obsługi urządzeń przenośnych](#Exercise3)
4. [Za pomocą kontrolerów asynchroniczne](#Exercise4)

> [!NOTE]
> Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach. Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.


Szacowany czas trwania tego laboratorium: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Ćwiczenie 1: Nowe szablony projektów programu ASP.NET MVC 4

W tym ćwiczeniu zostanie Poznaj ulepszenia w szablonach projektu programu ASP.NET MVC 4. Oprócz szablonu aplikacji internetowej znajduje się już w MVC 3, ta wersja teraz obejmuje oddzielne szablonu dla aplikacji dla urządzeń przenośnych. Po pierwsze będzie wyglądać na niektóre istotne cechy każdego z szablonów. Następnie będzie działać na renderowanie strony prawidłowo na różnych platformach przy użyciu podejście.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Zadanie 1 - eksploracji szablon aplikacji internetowych

1. Otwórz **programu Visual Studio**.
2. Wybierz **pliku | Nowy | Projekt** polecenia menu. W **nowy projekt** okno dialogowe, wybierz opcję **Visual C# | Web** szablonu w lewym okienku drzewa, a następnie wybierz pozycję **aplikacji sieci Web programu ASP.NET MVC 4.** Nazwij projekt **PhotoGallery**, wybierz lokalizację (lub pozostaw wartość domyślną) i kliknij przycisk **OK**.

    > [!NOTE]
    > Później będzie można dostosować rozwiązania PhotoGallery ASP.NET MVC 4, które teraz tworzysz.

    ![Tworzenie nowego projektu](whats-new-in-aspnet-mvc-4/_static/image1.png "tworzenia nowego projektu")

    *Tworzenie nowego projektu*
3. W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej** szablonu projektu i kliknij przycisk **OK**. Upewnij się, że został wybrany aparat widoku Razor.

    ![Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC](whats-new-in-aspnet-mvc-4/_static/image2.png "Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC")

    *Tworzenie nowej aplikacji internetowych 4 ASP.NET MVC*

    > [!NOTE]
    > Składnia razor została wprowadzona w programie ASP.NET MVC 3. Jego celem jest zminimalizowanie liczby znaków i naciśnięcia klawiszy wymagane w pliku, umożliwiające szybkie oraz płynu kodowania przepływu pracy. Razor wykorzystuje istniejące C# / VB (lub innych) umiejętności język, a następnie dostarcza składnia znacznika szablonu, umożliwiającą świetny przepływu pracy konstrukcji kodu HTML.
4. Naciśnij klawisz **F5** uruchomić rozwiązanie i zobaczyć odnowionego szablonów. Można wyewidencjonować następujące funkcje:

    **Szablonów w stylu Modern**

    Szablony odnowienia, zapewniając więcej stylów nowoczesny wygląd.

    ![Szablony ASP.NET MVC 4 restyled](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled szablonów")

    *Szablony ASP.NET MVC 4 restyled*

    ![Nowa strona Skontaktuj się z](whats-new-in-aspnet-mvc-4/_static/image4.png "nowy kontakt strony")

    *Nowa strona kontaktu*

    **Renderowanie adaptacyjną**

    Zapoznaj się z zmiany rozmiaru okna przeglądarki i zwróć uwagę, jak dynamicznie dostosowuje układ strony do nowego rozmiaru okna. Te szablony używać techniki adaptacyjną renderowania mają być renderowane poprawnie w zarówno wersje desktop i mobile platform bez potrzeby dostosowywania.

    ![Szablon projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiary](whats-new-in-aspnet-mvc-4/_static/image5.png "szablonu projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiarach")

    *Szablon projektu programu ASP.NET MVC 4 w innej przeglądarce rozmiarach*

    **Bardziej zaawansowane funkcje interfejsu użytkownika w języku JavaScript**

    Inny rozszerzenie domyślnych szablonów projektu jest użycie JavaScript w celu zapewnienia większej liczby interaktywnych JavaScript. Łącza logowania i rejestrowanie, używane w szablonie spróbujemy sposób użycia jQuery operacji sprawdzania poprawności do sprawdzania poprawności pól wejściowych od klienta.

    ![Sprawdzanie poprawności jQuery](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *Sprawdzanie poprawności jQuery*

    > [!NOTE]
    > Powiadomienie, że dwa Zaloguj się w sekcji, w pierwszej sekcji można zalogować się przy użyciu konta SuperScript z lokacji i w drugiej sekcji można altenativelly logowanie się za pomocą innej usługi uwierzytelniania, takich jak google (domyślnie wyłączone).
5. Zamknij przeglądarkę, aby zatrzymać debuger i powrócić do programu Visual Studio.
6. Otwórz plik **AuthConfig.cs** znajduje się w obszarze **aplikacji\_Start** folderu.
7. Usuń komentarz z ostatniego wiersza, aby zarejestrować klienta Google *OAuth* uwierzytelniania.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Należy zauważyć, że można łatwo włączyć uwierzytelnianie przy użyciu dowolnej usługi OpenID lub OAuth, takich jak Facebook, Twitter, Microsoft itd.
8. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie i przejdź do strony logowania.
9. Wybierz **Google** usługi, aby się zalogować.

    ![Wybieranie dziennika w usłudze](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *Wybieranie dziennika w usłudze*
10. Zaloguj się za pomocą konta Google.
11. Zezwalaj na lokacji (localhost) pobrać informacje z konta Google.
12. Ponadto należy zarejestrować się w lokacji, aby skojarzyć konto Google.

   ![Skojarz konto Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Kojarzenie konto Google*
13. Zamknij przeglądarkę, aby zatrzymać debuger i powrócić do programu Visual Studio.
14. Teraz Poznaj rozwiązania do wyewidencjonowania niektórych innych nowych funkcji wprowadzonych przez program ASP.NET MVC 4 w szablonie projektu.

   ![Szablon projektu aplikacji internetowych platformy ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image9.png "szablonu projektu aplikacji internetowych platformy ASP.NET MVC 4")

   *Szablon projektu aplikacji internetowych platformy ASP.NET MVC 4*

   - **HTML 5 znaczników**

       Przeglądaj widoków szablonu, aby dowiedzieć się nowego znacznika motywu.

       ![Nowy szablon, za pomocą znacznika About.cshtml Razor i HTML5. ] (whats-new-in-aspnet-mvc-4/_static/image10.png "Nowego szablonu, za pomocą znacznika About.cshtml Razor i HTML5.")

       *Nowy szablon, przy użyciu znaczników Razor i HTML5 (About.cshtml).*
   - **Zaktualizowano biblioteki JavaScript**

       Domyślny szablon platformy ASP.NET MVC 4 zawiera teraz elementami KnockoutJS i platforma JavaScript MVVM, która umożliwia tworzenie zaawansowanych aplikacji szybkiego sieci web przy użyciu języka JavaScript i HTML. Podobnie jak w MVC3, jQuery i jQuery biblioteki interfejsu użytkownika znajdują się również w technologii ASP.NET MVC 4.

     > [!NOTE]
     > Możesz uzyskać więcej informacji na temat biblioteki elementami KnockOutJS w to łącze: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/). Ponadto informacje na temat jQuery i jQuery interfejsu użytkownika w [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Zadanie 2 — poznawanie szablonu aplikacji mobilnej

ASP.NET MVC 4 ułatwia projektowanie witryn sieci Web dla urządzeń przenośnych i przeglądarek typu tablet. Ten szablon ma taką samą strukturę aplikacji jako szablonu aplikację internetową (powiadomienie, że kod kontrolera jest praktycznie identyczny), ale jego styl został zmodyfikowany mają być renderowane poprawnie w urządzeń przenośnych z systemem touch.

1. Wybierz **pliku | Nowy | Projekt** polecenia menu. W **nowy projekt** okno dialogowe, wybierz opcję **Visual C# | Web** szablonu w lewym okienku drzewa, a następnie wybierz pozycję **aplikacji sieci Web programu ASP.NET MVC 4.** Nazwij projekt **PhotoGallery.Mobile**, wybierz lokalizację (lub pozostaw wartość domyślną), wybierz &quot;dodać do rozwiązania&quot; i kliknij przycisk **OK**.
2. W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji Mobile** szablonu projektu i kliknij przycisk **OK**. Upewnij się, że został wybrany aparat widoku Razor.

    ![Tworzenie nowej aplikacji mobilnej 4 ASP.NET MVC](whats-new-in-aspnet-mvc-4/_static/image11.png "Tworzenie nowej aplikacji mobilnej 4 ASP.NET MVC")

    *Tworzenie nowej aplikacji mobilnej 4 ASP.NET MVC*
3. Teraz masz możliwość Eksploruj rozwiązania i zapoznaj się z nowych funkcji wprowadzonych w szablonie rozwiązania ASP.NET MVC 4 dla urządzeń przenośnych:

    - **jQuery Mobile biblioteki**

        Szablon projektu aplikacji mobilnych zawiera przenośnych biblioteki jQuery, która jest biblioteki typu open source dla przenośnych przeglądarkami. jQuery Mobile dotyczy stopniowym rozszerzaniu przeglądarek urządzeń przenośnych, które obsługują CSS i JavaScript. Stopniowym rozszerzaniu włącza wszystkie przeglądarki wyświetlić podstawowe elementy strony sieci web podczas tylko umożliwia korzystanie z najbardziej zaawansowanych przeglądarki do wyświetlania zawartości sformatowanego. Pliki obsługi języka JavaScript i CSS, uwzględnione w jQuery przenośnych styl pomocy przeglądarek urządzeń przenośnych w celu dopasowania do zawartości na ekranie bez wprowadzania żadnych zmian w znaczniku strony.

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *Biblioteka przenośnych jQuery zawarte w szablonie*
    - **HTML5 na podstawie znaczników**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Szablon aplikacji mobilnej przy użyciu znaczników HTML5, (Login.cshtml i index.cshtml)*
4. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.
5. Otwórz **Windows Phone 7 Emulator**.
6. Na ekranie startowym telefonu Otwórz program Internet Explorer. Zapoznaj się z adresu URL, gdzie uruchomić aplikacji klasycznych i przejść do tego adresu URL z telefonu (np. `http://localhost:[PortNumber]/`).
7. Teraz możesz wprowadzić strony logowania lub zapoznaj się z o stronie. Należy zauważyć, że styl witryny sieci Web jest oparty na nową aplikację Metro dla urządzeń przenośnych. Szablon projektu programu ASP.NET MVC 4 jest prawidłowo wyświetlany na urządzeniach przenośnych, upewniając się, że wszystkie elementy na stronie są widoczne i włączone. Zwróć uwagę, czy wystarczająco duży, aby być kliknięte lub dotknięciu łącza w nagłówku.

    ![Projekt strony szablonu w urządzeniu przenośnym](whats-new-in-aspnet-mvc-4/_static/image14.png "projektu szablonu stron w urządzeniu przenośnym")

    *Strony szablonu projektu w urządzeniu przenośnym*
8. Nowy szablon używa również **okienka ekranu metatag**. Szerokość okna przeglądarki wirtualnego zdefiniuj przeglądarki dla większości urządzeń przenośnych lub &quot;okienka ekranu&quot;, które jest większe niż rzeczywista szerokość urządzenia przenośnego. Dzięki temu przeglądarek urządzeń przenośnych wyświetlić całą stronę sieci web wewnątrz wirtualnych wyświetlania. **Okienka ekranu metatag** umożliwia deweloperom sieci web ustawianie szerokości, wysokości i skali obszaru przeglądarki na urządzeniach przenośnych **.** Szablonu platformy ASP.NET MVC 4 dla aplikacji mobilnych Ustawia szerokość urządzenia okienka ekranu (&quot;szerokość = szerokość urządzenia&quot;) w szablonie układu (*Views\Shared\_Layout.cshtml*), dzięki czemu wszystkie strony mają ich okienka ekranu, Ustaw szerokość ekranu urządzenia. Zwróć uwagę, metatag okienka ekranu nie zmieni domyślny widok przeglądarki.
9. Otwórz  **\_Layout.cshtml**, który znajduje się w **widoków | Udostępnione** folder i komentarza metatag okienka ekranu. Uruchom aplikację, jeśli nie już otwarty i zapoznaj się z różnic.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![Lokacji po komentowania metatag okienka ekranu](whats-new-in-aspnet-mvc-4/_static/image15.png "lokacji po komentowania metatag okienka ekranu")

*Lokacji po komentowania metatag okienka ekranu*
10. W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** można zatrzymać debugowania aplikacji.
11. Usuń znaczniki komentarza metatag okienka ekranu.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Zadanie 3 - adaptacyjnych renderowaniem

W tym zadaniu dowiesz się innej metody do renderowania strony sieci Web prawidłowo na urządzeniach przenośnych i przeglądarki sieci Web w tym samym czasie, bez potrzeby dostosowywania. Używano już okienka ekranu metatag o podobnych celów. Teraz będzie spełniać innej metody zaawansowane: *adaptacyjną renderowania*.

Renderowanie adaptacyjną jest technika, która używa **zapytaniami multimediów CSS3** Aby dostosować styl stosowany do strony. Zapytaniami multimediów Zdefiniuj warunki wewnątrz arkusza stylów, grupowanie stylów CSS w obszarze określonego warunku. Tylko wtedy, gdy warunek jest spełniony, styl jest stosowany do obiektów zadeklarowane.

Elastyczności techniką adaptacyjną renderowania umożliwia dostosowywanie wyświetlania lokacji na różnych urządzeniach. Można określić dowolną liczbę style, w których chcesz na arkusz stylów pojedynczego bez pisania kodu logiki wybierz styl. W związku z tym jest bardzo przejrzysty sposób dostosowania strony style, ponieważ zmniejsza ilość zduplikowanych kodu i logikę do celów renderowania. Z drugiej strony zwiększyć będą zużycie przepustowości, wzrostem rozmiaru plików CSS mogą się nieznacznie.

Przy użyciu techniki adaptacyjną renderowania, zostanie wyświetlona witryna **wyświetlane prawidłowo, niezależnie od przeglądarki.** Jednak należy rozważyć, jest istotny, jeśli przepustowość dodatkowego obciążenia.

> [!NOTE]
> Jest podstawowy format zapytanie o multimedia: @media \[zakres: wszystkie | handheld | wydruku | projekcji | ekranu\] ([właściwość: wartość] i... [właściwość: wartość])


Przykłady z zapytaniami multimediów: &gt;  <strong>@media wszystkich i (szerokość maksymalna: 1000px) i (szerokość minimalna: 700px) {}:</strong> dla wszystkich rozdzielczości między 700px i 1000px.

> <strong>@media ekran i (szerokość minimalna: 400 piks.) i (szerokość maksymalna: 700px) {...}:</strong> tylko na ekranach. Rozdzielczość musi wynosić od 400 do 700px.
> 
> <strong>@media urządzenia przenośnego i (minimalną szerokość: 20em), ekranu i (szerokość minimalna: 20em) {...}:</strong> urządzenia podręczne (przenośne i urządzenia) i ekranów. Minimalna szerokość musi być większa niż 20em.
> 
> Więcej informacji na ten temat można znaleźć na [lokacji W3C](http://www.w3.org/TR/css3-mediaqueries/).


Może teraz zapoznać, jak działa adaptacyjną renderowania, poprawa czytelności platformy ASP.NET MVC 4 domyślny szablon witryny sieci Web.

1. Otwórz **PhotoGallery.sln** rozwiązania, które zostały utworzone w zadaniu 1 i wybierz **PhotoGallery** projektu. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.
2. Zmienia szerokość w przeglądarce, ustawienia systemu windows do połowę lub mniej niż kwartału oryginalnego rozmiaru. Zwróć uwagę, co się dzieje z elementami w nagłówku: niektóre elementy nie będą widoczne w widocznym obszarze nagłówka.
3. Otwórz <strong>Site.css</strong> plików w Eksploratorze rozwiązania Visual Studio, znajduje się w <strong>zawartości</strong> folderu projektu. Naciśnij klawisz <strong>CTRL + F</strong> Otwórz wyszukiwanie zintegrowane Visual Studio i zapisać <strong>@media</strong> zlokalizować <strong>zapytanie o multimedia CSS</strong>.

    Warunek kwerendy nośnika zdefiniowane w tym szablonie działa w ten sposób: gdy rozmiar okna przeglądarki jest poniżej **850 pikseli**, reguły CSS, stosowane są tymi zdefiniowany w tym bloku nośnika.

    ![Lokalizowanie zapytanie o multimedia](whats-new-in-aspnet-mvc-4/_static/image16.png "lokalizowanie zapytanie o multimedia")

    *Lokalizowanie zapytanie o multimedia*
4. Zastąp wartość atrybutu maksymalna szerokość 850 pikseli z **10px**, aby wyłączyć adaptacyjną renderowania i naciśnij klawisz **CTRL + S** Aby zapisać zmiany. Wróć do przeglądarki i naciśnij klawisz **CTRL + F5** odświeżenie strony z wprowadzone zmiany. Zwróć uwagę różnice w obu stron, zmieniając szerokości okna.

    ![W lewej strony jest stosowania @media pominięcia stylu w prawo, styl](whats-new-in-aspnet-mvc-4/_static/image17.png "w lewej strony jest stosowania @media stylu w prawo, styl zostanie pominięty")

    <em>W lewej strony jest stosowania @media stylu w prawo, styl zostanie pominięty.</em>

    Teraz załóżmy wyewidencjonować co się dzieje na urządzeniach przenośnych:

    ![W lewej strony jest stosowania @media pominięcia stylu w prawo, styl](whats-new-in-aspnet-mvc-4/_static/image18.png "w lewej strony jest stosowania @media stylu w prawo, styl zostanie pominięty")

    <em>W lewej strony jest stosowania @media stylu w prawo, styl zostanie pominięty.</em>

    Mimo że można zauważyć, że zmiany podczas renderowania strony w przeglądarce sieci Web nie są bardzo istotne, gdy na urządzeniu przenośnym, różnice staną się bardziej oczywistymi. Po lewej stronie obrazu możemy stwierdzić, czy styl niestandardowy poprawia czytelność.

    W wielu scenariuszach więcej, co ułatwia zastosować warunkowego stylów do witryny sieci Web i rozwiązywania typowych problemów styl porządek podejścia przy rozwiązywaniu służy adaptacyjną renderowania.

    Tag meta okienka ekranu i zapytaniami multimediów CSS nie są specyficzne dla platformy ASP.NET MVC 4, dzięki czemu można korzystać z tych funkcji w dowolnej aplikacji sieci web.
5. W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** można zatrzymać debugowania aplikacji.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Ćwiczenie 2: Tworzenie aplikacji sieci Web galerii fotografii

W tym ćwiczeniu będzie działać w Galerii fotografii aplikacji do wyświetlenia zdjęcia. Rozpocznie się przy użyciu szablonu projektu programu ASP.NET MVC 4, a następnie doda funkcji można pobrać zdjęcia z usługą i wyświetlać na stronie głównej.

W poniższym ćwiczeniu zostanie zaktualizowana to rozwiązanie w celu zwiększenia sposób wyświetlania na urządzeniach przenośnych.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Zadanie 1 — Tworzenie usługi zasymulować zdjęcia

W ramach tego zadania spowoduje utworzenie makiety usługi fotografii można pobrać zawartości, który będzie wyświetlany w galerii. Aby to zrobić, zostaną dodane nowego kontrolera, która po prostu zwróci pliku JSON z danymi każdej fotografii.

1. Otwórz **programu Visual Studio** Jeśli nie jest jeszcze otwarty.
2. Wybierz **pliku | Nowy | Projekt** polecenia menu. W **nowy projekt** okno dialogowe, wybierz opcję **Visual C# | Web** szablonu w lewym okienku drzewa, a następnie wybierz pozycję **aplikacji sieci Web programu ASP.NET MVC 4.** Nazwij projekt **PhotoGallery**, wybierz lokalizację (lub pozostaw wartość domyślną) i kliknij przycisk **OK**. Alternatywnie można kontynuować pracę z istniejących MVC 4 ASP.NET **aplikacji internetowej** rozwiązania z **ćwiczenie 1** i przejść do następnego kroku.
3. W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej** szablonu projektu i kliknij przycisk **OK**. Upewnij się, że masz wybrany jako aparat widoku Razor.
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **aplikacji\_danych** folderu projektu i wybierz **Dodaj | Istniejący element**. Przejdź do **Source\Assets\App\_danych** folder tego laboratorium i Dodaj **Photos.json** pliku.
5. Tworzenie nowego kontrolera o nazwie **PhotoController**. Aby to zrobić, kliknij prawym przyciskiem myszy **kontrolerów** folderu, przejdź do **Dodaj** i wybierz **kontrolera.** Pełna nazwa kontrolera, pozostaw **kontroler MVC pusty** szablon i kliknij przycisk **Dodaj**.

    ![Dodawanie PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Dodawanie PhotoController")

    *Dodawanie PhotoController*
6. Zastąp **indeksu** metodę z następującym **galerii** akcji i powrocie zawartość z pliku JSON ostatnio dodane do projektu.

    (Fragment - kodu *platformy ASP.NET MVC 4 - Ex02 - laboratorium galerii akcji*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie, a następnie przejdź do następującego adresu URL w celu przetestowania usługi mocked zdjęcie: `http://localhost:[port]/photo/gallery` (wartość [port] zależy od bieżącego portu, na którym aplikacja została uruchomiona). Żądanie do tego adresu URL powinien pobierać zawartość z **Photos.json** pliku.

    ![Testowanie usługi fotografii mocked](whats-new-in-aspnet-mvc-4/_static/image20.png "testowania usługi mocked zdjęcia")

    *Testowanie usługi mocked zdjęcia*

W implementacji rzeczywistych można użyć [ASP.NET Web API](../../../../web-api/index.md) do implementacji usługi galerii fotografii. Interfejs API sieci Web platformy ASP.NET to platforma, która ułatwia tworzenie usług HTTP, które są używane przez szeroki wachlarz klientów, w tym przeglądarki i urządzenia przenośne. Interfejs API sieci Web ASP.NET jest idealną platformą do tworzenia RESTful aplikacji w programie .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Zadanie 2 — Wyświetlanie galerii fotografii

To zadanie zaktualizuje strony głównej można wyświetlić za pomocą usługi mocked utworzonego w pierwszym zadaniu tego ćwiczenia galerii fotografii. Spowoduje dodanie plików modelu i zaktualizować widoki galerii.

1. W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** można zatrzymać debugowania aplikacji.
2. Utwórz **fotografii** klasy w **modele** folderu. Aby to zrobić, kliknij prawym przyciskiem myszy **modele** folderu, wybierz opcję **Dodaj** i kliknij przycisk **klasy**. Następnie ustaw nazwę **Photo.cs** i kliknij przycisk **Dodaj**.
3. Dodaj następujące elementy członkowskie do **fotografii** klasy.

    (Fragment - kodu *modelu fotografii ASP.NET MVC 4 laboratorium - Ex02 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Otwórz **HomeController.cs** plik z **kontrolerów** folderu.
5. Dodaj następujące instrukcje using.

    (Fragment - kodu *deklaracje Using HomeController laboratorium - Ex02 - platformy ASP.NET MVC 4*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Aktualizacja **indeksu** akcję do użycia **HttpClient** pobierania danych galerii, a następnie użyć **JavaScriptSerializer** do deserializacji do modelu widoku.

    (Fragment - kodu *akcji indeksu laboratorium - Ex02 - platformy ASP.NET MVC 4*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Otwórz **Index.cshtml** plik znajdujący się w obszarze **Views\Home** folderu i Zastąp całą zawartość następującym kodem.

    Ten kod w pętli wszystkich zdjęć, które są pobierane z usługi i wyświetla je do listy nieuporządkowanej.

    (Fragment - kodu *listy fotografii laboratorium - Ex02 - platformy ASP.NET MVC 4*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **zawartości** folderu projektu i wybierz **Dodaj | Istniejący element**. Przejdź do **Source\Assets\Content** folder tego laboratorium i Dodaj **Site.css** pliku. Należy potwierdzić jego zastąpienie. Jeśli masz **Site.css** plik otwarty, musisz potwierdzić również ponownie załadować plik.
9. Otwórz Eksplorator plików i skopiować całą **zdjęć** folder znajduje się w obszarze **Source\Assets** folder tego laboratorium do folderu głównego projektu w Eksploratorze rozwiązań.
10. Uruchom aplikację. Powinna zostać wyświetlona strona główna wyświetlanie zdjęć w galerii.

    ![Galeria fotografii](whats-new-in-aspnet-mvc-4/_static/image21.png "galerii fotografii")

    *Galeria fotografii*
11. W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** można zatrzymać debugowania aplikacji.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Ćwiczenie 3: Dodawanie obsługi urządzeń przenośnych

Jeden z kluczy aktualizacji w programie ASP.NET MVC 4 jest obsługiwane do tworzenia aplikacji mobilnych. W tym ćwiczeniu zostanie Poznaj nowe funkcje platformy ASP.NET MVC 4 dla aplikacji dla urządzeń przenośnych przez rozszerzenie rozwiązanie PhotoGallery, które zostały utworzone w poprzednim ćwiczeniu.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Zadanie 1 - instalowania jQuery Mobile w aplikacji ASP.NET MVC 4

1. Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex3-MobileSupport/Begin/** folderu. W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.

   1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz **Konsola Menedżera pakietów** klikając **narzędzia** &gt; **Menedżer pakietów biblioteki** &gt; **Menedżera pakietów Konsola** opcji menu.

    ![Otwieranie konsoli Menedżera pakietów NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "otwieranie konsoli Menedżera pakietów NuGet")

    *Otwieranie konsoli Menedżera pakietów NuGet*
3. W konsoli Menedżera pakietów, uruchom następujące polecenie, aby zainstalować **jQuery.Mobile.MVC** pakietu.

    jQuery Mobile to biblioteki typu open source do tworzenia zoptymalizowanych pod kątem touch interfejsu użytkownika sieci web. Pakiet NuGet jQuery.Mobile.MVC zawiera pomocników do użycia z aplikacją ASP.NET MVC 4 jQuery Mobile.

    > [!NOTE]
    > Uruchamiając następujące polecenie, będzie można pobierania biblioteki jQuery.Mobile.MVC z pakietu Nuget.

    PM.

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    To polecenie instaluje jQuery Mobile, a niektóre pliki pomocnicze, takie jak następujące:

    - **Widoki/Shared/\_Layout.Mobile.cshtml**: jest zoptymalizowana pod kątem mniejszego ekranu jQuery Mobile na podstawie układu. Gdy witryna sieci Web otrzymuje żądanie z przenośnymi przeglądarki, spowoduje zastąpienie oryginalnego układu (\_Layout.cshtml) z tym kontem.
    - Składnik tym przełącznikiem widoku: składa się z **widoków/Shared/\_ViewSwitcher.cshtml** widok częściowy i **ViewSwitcherController.cs** kontrolera. Ten składnik będą wyświetlane łącza na przeglądarek urządzeń przenośnych, aby umożliwić użytkownikom przełączyć się do tej wersji strony.  
        ![Galeria fotografii projektu z obsługą przenośnych](whats-new-in-aspnet-mvc-4/_static/image23.png "galerii fotografii projektu z obsługą przenośnych")

        *Galeria fotografii projektu z obsługą przenośnych*
4. Zarejestruj pakiety przenośnych. Aby to zrobić, otwórz **Global.asax.cs** pliku i Dodaj następujący wiersz.

    (Fragment - kodu *platformy ASP.NET MVC 4 - Ex03 - laboratorium rejestru przenośnych pakiety*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Uruchom aplikację za pomocą przeglądarki sieci web.
6. Otwórz **Windows Phone 7 Emulator,** znajduje się w **Start Menu | Wszystkie programy | Windows Phone SDK 7.1 | Emulator Windows Phone.**
7. Na ekranie startowym telefonu Otwórz program Internet Explorer. Zapoznaj się z adresu URL, w której aplikacja jest uruchomiona, a następnie przejdź do tego adresu URL za pośrednictwem przeglądarki telefonu (np. `http://localhost:[PortNumber]/`).

    Można zauważyć, że aplikacja będzie wyglądać inaczej w emulator Windows Phone jQuery.Mobile.MVC ma tworzenia nowych zasobów w projekcie Pokaż widoki zoptymalizowane dla urządzeń przenośnych.

    Zwróć uwagę, wiadomości w górnej części przez telefon, przedstawiający link zmienia się do widoku pulpitu. Ponadto  **\_Layout.Mobile.cshtml** układu, który został utworzony przez pakiet został zainstalowany inny układ jest w tym w aplikacji.

    > [!NOTE]
    > Do tej pory nie ma żadnego linku, aby wrócić do widokiem dla urządzeń przenośnych. Są uwzględniane w nowszych wersjach.

    ![Widok strony głównej Galeria fotografii dla urządzeń przenośnych](whats-new-in-aspnet-mvc-4/_static/image24.png "widok strony głównej Galeria fotografii dla urządzeń przenośnych")

    *Widok strony głównej Galeria fotografii dla urządzeń przenośnych*
8. W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** można zatrzymać debugowania aplikacji.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Zadanie 2 — Tworzenie widokach dla urządzeń przenośnych

W ramach tego zadania spowoduje utworzenie mobilnej wersji widoku indeksu z zawartością dostosowane do lepszego appareance na urządzeniach przenośnych.

1. Kopiuj **Views\Home\Index.cshtml** wyświetlanie i wklej go, aby utworzyć kopię, Zmień nazwę nowego pliku **Index.Mobile.cshtml**.
2. Otwórz utworzony nowy **Index.Mobile.cshtml** wyświetlanie i Zastąp istniejące &lt;ul&gt; tag o tym kodzie. Dzięki temu będzie aktualizowanie &lt;ul&gt; tag przy użyciu adnotacji danych mobilnych jQuery używać przenośnych motywów z jQuery.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Zwróć uwagę, że:
    > 
    > - **Danych roli** ustawić atrybutu **listview** spowoduje, że listy przy użyciu stylów elementu listview.
    > 
    > - **Danych wstawki** pokazuje listę zaokrąglony obramowania i margines atrybut ma wartość true.
    > 
    > - **Filtr danych** ustawić atrybutu **true** wygeneruje pola wyszukiwania.
    > 
    > Można poznać więcej informacji na temat Konwencji przenośnych jQuery w dokumentacji projektu: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.
4. Przełącz się do **Emulator Windows Phone** i Odśwież lokacji. Zwróć uwagę, nowe wygląd listy galerii, a także pole wyszukiwania znajduje się na górze. Następnie wpisz wyraz w polu wyszukiwania (na przykład **Tulipany**), aby rozpocząć wyszukiwanie w Galerii fotografii.

    ![Galeria przy użyciu stylu listview z filtrowania](whats-new-in-aspnet-mvc-4/_static/image25.png "galerii przy użyciu stylu listview z filtrowania")

    *Galeria przy użyciu stylu listview z filtrowania*

    Podsumowując, aby utworzyć kopię widoku indeksu z zostały użyte przepisu Mobilizer widoku &quot;przenośnych&quot; sufiks. Ten sufiks wskazuje ASP.NET MVC 4, że każde żądanie generowane z urządzenia przenośnego będzie używał kopii tego indeksu. Ponadto zaktualizowano wersja urządzenia przenośne widoku indeksu na jQuery Mobile wzmocnienia lokacji wygląd i działanie na urządzeniach przenośnych.
5. Wróć do programu Visual Studio i Otwórz **Site.Mobile.css** znajduje się w obszarze **zawartości** folderu.
6. Napraw pozycjonowanie tytuł fotografii, aby wyświetlić je po prawej stronie obrazu. Aby to zrobić, Dodaj następujący kod, aby **Site.Mobile.css** pliku.

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.
8. Przywraca **Emulator Windows Phone** i Odśwież lokacji. Należy zauważyć, że tytuł fotografii prawidłowo znajduje się teraz.

    ![Tytuł znajduje się po prawej stronie obrazu](whats-new-in-aspnet-mvc-4/_static/image26.png "tytuł znajduje się po prawej stronie obrazu")

    *Tytuł znajduje się po prawej stronie obrazu*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Zadanie 3 - jQuery Mobile motywów

Każdy układ i elementu widget w jQuery Mobile jest zaprojektowana dla nowego zorientowane obiektowo CSS platforma, która pozwala na zastosowanie motywu pełną ujednoliconego projekt visual do witryn i aplikacji.

jQuery Mobile domyślny motyw obejmuje 5 próbki, które są podane litery (, b, c, d, e) krótkimi opisami.

To zadanie zaktualizuje przenośnych układ na kompozycję inną niż domyślna.

1. Przełącz się do programu Visual Studio.
2. Otwórz  **\_Layout.Mobile.cshtml** plik znajdujący się w **Views\Shared**.
3. Znajdź div element z rolą danych ustawioną &quot;strony&quot; i zaktualizuj **data-theme** do &quot; **e**&quot;.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.
5. Odśwież lokację w **Emulator Windows Phone** i zwróć uwagę, nowy schemat kolorów.

    ![Przenośne układ z inny schemat kolorów](whats-new-in-aspnet-mvc-4/_static/image27.png "przenośnych układ z innego schematu kolorów")

    *Przenośne układ z innego schematu kolorów*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Zadanie 4 — przy użyciu składnika tym przełącznikiem widoku i zastępowanie funkcje przeglądarki

Konwencja dla stron sieci web zoptymalizowanych pod kątem mobile jest dodać łącze, którego tekst jest coś jak widok pulpitu lub tryb witrynę w trybie pełnym, który umożliwia użytkownikom pulpitu wersję strony. Pakiet jQuery.Mobile.MVC zawiera przykładowe **tym przełącznikiem widoku** składnika w tym celu używana w  **\_Layout.Mobile.cshtml** widoku.

![Łącze, aby przełączyć do widoku pulpitu](whats-new-in-aspnet-mvc-4/_static/image28.png "łącze, aby przełączyć do widoku pulpitu")

*Łącze, aby przełączyć do widoku pulpitu*

Korzysta z tym przełącznikiem widoku nową funkcję o nazwie **zastępowania przeglądarki**. Ta funkcja umożliwia aplikacji taką obsługę żądań, tak jakby pochodziły one z innej przeglądarki (agenta użytkownika) niż ten, który faktycznie pochodzą.

W tym zadaniu zostanie Poznaj Przykładowa implementacja dodane przez jQuery.Mobile.MVC i Nowa przeglądarka zastępowanie funkcji w programie ASP.NET MVC 4 przełącznik widoku.

1. Przełącz się do programu Visual Studio.
2. Otwórz  **\_Layout.Mobile.cshtml** widoku znajduje się w obszarze **Views\Shared** folderu i zwróć uwagę, składnik tym przełącznikiem widoku jest określany jako widok częściowy.

    ![Układ przenośnych za pomocą składnika z tym przełącznikiem widoku](whats-new-in-aspnet-mvc-4/_static/image29.png "układu przenośnych za pomocą składnika z tym przełącznikiem widoku")

    *Układ przenośnych za pomocą składnika z tym przełącznikiem widoku*
3. Otwórz  **\_ViewSwitcher.cshtml** widoku częściowego.

    Nowa metoda używa widoku częściowego **ViewContext.HttpContext.GetOverriddenBrowser()** do określenia pochodzenia żądania sieci web i wyświetlić odpowiednie łącze, aby przełączyć się do widoków Desktop lub Mobile.

    **GetOverridenBrowser** metoda zwraca **HttpBrowserCapabilitiesBase** wystąpienie, które odpowiada agenta użytkownika aktualnie ustawione na żądanie (rzeczywista lub przesłonięte). Możesz użyć tej wartości można pobrać właściwości, takie jak **IsMobileDevice**.

    ![Widok częściowy ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher widok częściowy")

    *Widok częściowy ViewSwitcher*
4. Otwórz **ViewSwitcherController.cs** klasa znajduje się w **kontrolerów** folderu. Zapoznaj się z tym SwitchView akcji jest wywoływana przez łącze w składniku ViewSwitcher i zwróć uwagę, nowych metod element HttpContext.

    - **HttpContext.ClearOverridenBrowser()** metoda usuwa wszelkich przesłoniętych agentów użytkownika dla bieżącego żądania.
    - **HttpContext.SetOverridenBrowser()** metody przesłania żądania rzeczywistą wartość agenta użytkownika przy użyciu określonego agenta użytkownika.  
        ![Kontroler ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher kontrolera")  
*Kontroler ViewSwitcher*

        Przesłanianie przeglądarki jest funkcją podstawowej platformy ASP.NET MVC 4, który jest również dostępny nawet wtedy, gdy pakiet jQuery.Mobile.MVC nie jest instalowany. Jednak ta funkcja dotyczy tylko widoku, układu i widoku częściowego, a nie dotyczy żadnych funkcji, które są zależne od obiektu Request.Browser.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Zadanie 5 dodawanie z tym przełącznikiem widoku w widoku pulpitu.

To zadanie zaktualizuje układ pulpitu do uwzględnienia w tym przełącznikiem widoku. Dzięki temu użytkowników mobilnych powrócić do widoku przenośnych podczas przeglądania widok pulpitu.

1. Odśwież lokację w **Emulator Windows Phone**.
2. Polecenie **widok pulpitu** łącze w górnej części galerii. Należy zauważyć, że istnieje widok przełącznik w widoku pulpitu do zezwalania powróć do widoku przenośnych.
3. Wróć do programu Visual Studio i Otwórz  **\_Layout.cshtml** widoku.
4. Znajdź sekcję logowania i wstawianie wywołania do renderowania  **\_ViewSwitcher** widoku częściowego poniżej  **\_LogOnPartial** widoku częściowego. Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.
6. Odśwież stronę w Emulator Windows Phone, a następnie kliknij dwukrotnie ikonę ekranu, aby powiększyć. Należy zauważyć, że strona główna zawiera obecnie **widokiem dla urządzeń przenośnych** łącze, które zmienia z przenośnymi na widok pulpitu.

    ![Wyświetl przełącznik renderowane w widoku pulpitu](whats-new-in-aspnet-mvc-4/_static/image32.png "przełącznik widok renderowany w widoku pulpitu")

    *Przełącznik widok renderowany w widoku pulpitu*
7. Przełącz do widoku przenośnych ponownie, a następnie przejdź do <strong>o</strong> strony (http://localhost[portu]/Home/About). Należy zauważyć, że nawet jeśli nie utworzono widok About.Mobile.cshtml informacje strona jest wyświetlana w układzie przenośnych (\_Layout.Mobile.cshtml).

    ![Strona informacje](whats-new-in-aspnet-mvc-4/_static/image33.png "strona — informacje")

    *Strona — informacje*
8. Na koniec Otwórz witrynę w przeglądarce sieci Web pulpitu. Należy zauważyć, że żaden z poprzednich aktualizacji ma wpływ widok pulpitu.

    ![Widok pulpitu PhotoGallery](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery widok pulpitu")

    *Widok pulpitu PhotoGallery*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Zadanie 6 — utworzenie nowego trybów wyświetlania

Nowa funkcja trybów wyświetlania umożliwia aplikacji wybierz widoki, w zależności od przeglądarki, która generuje żądanie. Na przykład jeśli przeglądarka pulpitu zażąda strony głównej, aplikacja zwróci **Views\Home\Index.cshtml** szablonu. Następnie, jeśli przeglądarkę dla telefonów zażąda strony głównej, aplikacja zwróci **Views\Home\Index.mobile.cshtml** szablonu.

W ramach tego zadania spowoduje utworzenie układu dostosowanego dla urządzenia iPhone i konieczne będzie symulować żądania z urządzenia iPhone. Aby to zrobić, można użyć albo iPhone emulatorze/symulatorze (takich jak [Electric symulatora Mobile](http://www.electricplum.com/)) lub przeglądarki z dodatkami, które modyfikują agenta użytkownika. Aby uzyskać instrukcje dotyczące ustawiania ciąg agenta użytkownika przeglądarki Safari emulować iPhone, zobacz [jak umożliwić Safari podawać jest IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) w blogu Alison Dominika.

**Zwróć uwagę, że to zadanie jest opcjonalne i można kontynuować bez jej wykonanie w laboratorium.**

1. W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** można zatrzymać debugowania aplikacji.
2. Otwórz **Global.asax.cs** i dodaj następującą instrukcję using.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Dodaj następujący wyróżniony kod do aplikacji\_Start — metoda.

    (Fragment - kodu *ASP.NET MVC 4 laboratorium - Ex03 — iPhone DisplayMode*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

Zarejestrowano nowe **DefaultDisplayMode o nazwie &quot;iPhone&quot;**, w ramach statycznych **DisplayModeProvider.Instance.Modes** listy statycznej pasujących przed każdego żądania przychodzącego. Jeśli żądania przychodzącego, jeśli zawiera ciąg &quot;iPhone&quot;, ASP.NET MVC zostanie ustalone, widoki, których nazwa zawiera &quot;iPhone&quot; sufiks. Parametr 0 wskazuje, jak określone jest nowy tryb; na przykład, w tym widoku jest bardziej szczegółowy niż ogólne &quot;.mobile&quot; regułę, która odpowiada na żądania z urządzeń przenośnych.

Po uruchomieniu tego kodu, jeśli iPhone przeglądarka generuje żądanie, aplikacja będzie korzystać z **Views\Shared\\_Layout.iPhone.cshtml** układu utworzysz w następnych krokach.

> [!NOTE]
> Dzięki temu testowania żądania dla telefonów iPhone, zostały uproszczone dla celów demonstracyjnych i może nie działać zgodnie z oczekiwaniami dotyczącymi każdego iPhone ciąg agenta użytkownika (na przykład testu jest uwzględniana wielkość liter).

4. Utwórz kopię  **\_Layout.Mobile.cshtml** w pliku **Views\Shared** folder i Zmień nazwę kopii do &quot; **\_Layout.iPhone.csthml**&quot;.
5. Otwórz  **\_Layout.iPhone.csthml** utworzony w poprzednim kroku.
6. Znajdź div element, używając atrybutu data-role ustawioną **strony** i zmienić **data-theme** atrybutu &quot; **a**&quot;.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Teraz masz 3 układów w aplikacji ASP.NET MVC 4:

1. **\_Layout.cshtml**: układ domyślny używany w przypadku przeglądarek komputerowych.
2. **\_Layout.Mobile.cshtml**: układ domyślny używany dla urządzeń przenośnych.
3. **\_Layout.iPhone.cshtml**: układ określonych dla urządzenia iPhone, przy użyciu innego schematu kolorów do odróżnienia od \_Layout.mobile.cshtml.
7. Naciśnij klawisz **F5** uruchomić aplikację, a następnie przejdź do lokacji w **Emulator Windows Phone**.
8. Otwórz **symulatora telefonu iPhone** (zobacz [dodatku C](#AppendixC) instrukcje na temat instalowania i konfigurowania symulatora telefonu iPhone) i przejdź do witryny za. Zwróć uwagę, że każdy telefon przy użyciu określonego szablonu.

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Przy użyciu różnych widoków dla każdego urządzenia przenośnego*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Ćwiczenie 4: Za pomocą kontrolerów asynchroniczne

Microsoft .NET Framework 4.5 wprowadza nowe funkcje języka C# i Visual Basic zapewnia nowy podstawę dla asynchrony w programowaniu .NET. Ten nowy foundation sprawia, że programowanie asynchroniczne podobne do - i około równie proste jak - synchroniczne programowania. Jesteś teraz możliwość zapisywania akcji asynchronicznej metody w technologii ASP.NET MVC 4 przy użyciu **AsyncController** klasy. Za pomocą metod asynchronicznych akcji dla długotrwałe, niezwiązane z procesorem powiązany żądania. Dzięki temu można uniknąć blokowanie serwera sieci Web z wykonywania pracy podczas przetwarzania żądania. Klasa AsyncController zazwyczaj jest używany dla wywołań usług sieci Web długotrwałe.

Tego ćwiczenia przedstawiono podstawowe operację asynchroniczną na platformie ASP.NET MVC 4. Jeśli chcesz bardziej zgłębić temat, można wyewidencjonować artykule: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Zadanie 1 - wdrażanie kontrolera asynchronicznego

1. Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex4-Async/Begin/** folderu. W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.

   1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
2. Otwórz **HomeController.cs** klasę z **kontrolerów** folderu.
3. Dodaj następującą instrukcję using.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Aktualizacja **HomeController** klasa dziedziczy po **AsyncController**. Kontrolery, które pochodzą z AsyncController włączyć platformę ASP.NET do przetwarzania żądań asynchronicznych i może nadal metod synchronicznych akcji usługi.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Dodaj **async** słowa kluczowego **indeksu** — metoda i przydzielić mu typ zwracany **zadań&lt;ActionResult&gt;**.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > **Async** — słowo kluczowe jest jedną z nowych słów kluczowych, .NET Framework 4.5 zapewnia; go informuje kompilator, że ta metoda zawiera kod asynchroniczny. A **zadań** obiekt reprezentuje operację asynchroniczną, która może zakończyć w pewnym momencie w przyszłości.
6. Zastąp **klienta. GetAsync()** wywołanie za pomocą wersji pełnej async — słowo kluczowe await, jak pokazano poniżej.

    (Fragment - kodu *platformy ASP.NET MVC 4 laboratorium - Ex04 - GetAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > W poprzedniej wersji, były używane **wynik** właściwość z **zadań** obiekt, aby zablokować wątek, dopóki wynik zostanie zwrócony (wersję usługi synchronizacji).
    > 
    > Dodawanie **await** — słowo kluczowe informuje kompilator, aby asynchronicznie poczekaj, aż zadanie zwrócone przez wywołanie metody. Oznacza to, że dalszej części kodu zostanie wykonany jako wywołanie zwrotne dopiero po zakończeniu Oczekiwano metody. Jest innym czynnikiem, który należy zauważyć, że nie trzeba zmienić z bloku try-catch, aby umożliwić użycie tych wartości: wyjątki, które się tak zdarzyć w tle lub na pierwszym planie nadal zostanie przechwycony bez konieczności wykonywania dodatkowych działań za pomocą obsługi dostarczane przez platformę.
7. Zmień kod, aby kontynuować z implementacją asynchroniczne przez zamianę wiersze nowy kod w sposób przedstawiony poniżej

    (Fragment - kodu *platformy ASP.NET MVC 4 laboratorium - Ex04 - ReadAsStringAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Uruchom aplikację. Można zauważyć nie większych zmian, ale kodu nie powoduje blokowania wątków z puli wątków wprowadzania czy lepsze wykorzystanie zasobów serwera i poprawia wydajność.

    > [!NOTE]
    > Dowiedz się więcej o nowe funkcje programowania asynchronicznego w laboratorium &quot; **programowania asynchronicznego w programie .NET 4.5 z C# i Visual Basic** &quot; zawarte w zestawie szkolenia programu Visual Studio.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Zadanie 2 — limity czasu obsługi o anulowanie tokenów

Metody asynchroniczne akcji, które zwracają wystąpienia obiektów Task może również obsługiwać przekroczeń limitu czasu. To zadanie zaktualizuje indeksu kodu metody do obsługi scenariusza limitu czasu przy użyciu token anulowania.

1. Wróć do programu Visual Studio i naciśnij klawisz **SHIFT + F5** można zatrzymać debugowania.
2. Dodaj następujący kod za pomocą instrukcji do **HomeController.cs** pliku.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Akcja indeksu do otrzymywania aktualizacji **CancellationToken** argumentu.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Aktualizacja **GetAsync** wywołania do przekazania token anulowania.

    (Fragment - kodu *SendAsync laboratorium - Ex04 - platformy ASP.NET MVC 4 z CancellationToken*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Dekoracji *indeksu* metody z **Wartość AsyncTimeout** atrybutu ustawić milisekund 500 i **HandleError** atrybutu skonfigurowane do obsługi  **TaskCanceledException** przez przekierowywanie do **upłynął limit czasu** widoku.

    (Fragment - kodu *atrybuty laboratorium - Ex04 - platformy ASP.NET MVC 4*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Otwórz **PhotoController** klasy i aktualizacji **galerii** metody opóźnienia miliseconds wykonywania 1000 (1 sekunda), aby symulować długotrwałych zadań.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Otwórz **Web.config** plików i włączyć błędów niestandardowych, dodając następujący element.

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Utwórz nowy widok w **Views\Shared** o nazwie **upłynął limit czasu** i używać domyślnego układu. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **Views\Shared** i wybierz polecenie **Dodaj | Widok**.

    ![Na każdym urządzeniu przenośnym, przy użyciu różnych widoków](whats-new-in-aspnet-mvc-4/_static/image36.png "przy użyciu różnych widoków dla każdego urządzenia przenośnego")

    *Przy użyciu różnych widoków dla każdego urządzenia przenośnego*
9. Aktualizacja **upłynął limit czasu** wyświetlania zawartości, jak pokazano poniżej.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Uruchom aplikację i przejdź do adresu URL katalogu głównego. Jak dodano **Thread.Sleep** 1000 milisekund, wystąpi błąd limitu czasu, generowane przez **Wartość AsyncTimeout** atrybutu i catch przez **HandleError** atrybut.

    ![Wyjątek limitu czasu obsługi](whats-new-in-aspnet-mvc-4/_static/image37.png "wyjątek limitu czasu obsługi")

    *Wyjątek limitu czasu obsługi*

> [!NOTE]
> Ponadto można wdrożyć tę aplikację systemu Windows Azure Web Sites następujących [dodatek D: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy](#AppendixD).


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym hands-na-laboratorium możesz już przestrzegane niektóre z nowych funkcji znajdują się w technologii ASP.NET MVC 4. Zostały poddane dyskusji następujące kwestie:

- Zawiera ulepszenia do platformy ASP.NET MVC projektu szablony tym szablonie projektu aplikacji mobilnej
- Za pomocą atrybutu okienka ekranu HTML5 i zapytaniami multimediów CSS zwiększyć wyświetlania na urządzeniach przenośnych
- Ulepszenia progresywnego oraz tworzenie zoptymalizowanych pod kątem touch interfejsu użytkownika sieci web mieć jQuery Mobile
- Utwórz widoki specyficzne dla mobile
- Korzystanie ze składnika tym przełącznikiem widoku do przełączania się między widokami aplikacji mobilnych i klasycznych
- Utwórz asynchroniczne kontrolerów przy użyciu zadań pomocy technicznej

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Dodatek A: korzystania z wstawek kodu

Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki. Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.

![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](whats-new-in-aspnet-mvc-4/_static/image38.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*

***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragment (bez spacji i łączniki).
3. Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.
4. Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

![Rozpocznij wpisywanie nazwy fragment](whats-new-in-aspnet-mvc-4/_static/image39.png "zacznij wpisywać nazwę wstawki programu")

*Rozpocznij wpisywanie nazwy fragment kodu*

![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](whats-new-in-aspnet-mvc-4/_static/image40.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")

*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

![Ponownie naciśnij klawisz Tab i fragment rozwinie](whats-new-in-aspnet-mvc-4/_static/image41.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")

*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*

***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)***

1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.
2. Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.
3. Wybierz odpowiedni fragment z listy, klikając go.

![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](whats-new-in-aspnet-mvc-4/_static/image42.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")

*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*

![Wybierz odpowiedni fragment z listy, klikając go](whats-new-in-aspnet-mvc-4/_static/image43.png "wybierz odpowiedni fragment z listy, klikając go")

*Wybierz odpowiedni fragment z listy, klikając go*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Dodatek B: Instalowanie programu Visual Studio Express 2012 for Web

Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; <em>programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.
2. Polecenie **teraz zainstalować**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Instalowanie programu Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "instalacji programu Visual Studio Express")

    *Instalowanie programu Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Akceptowanie umowy licencyjnej*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *VS Express for Web kafelka*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Dodatek C: Instalowanie programu WebMatrix 2 i iPhone symulatora

Działanie witryny na urządzeniu iPhone symulowane można użyć rozszerzenia programu WebMatrix &quot;Electric symulatora mobilnych dla telefonów iPhone&quot;. Ponadto można skonfigurować te same rozszerzeń do uruchamiania symulatora z programu Visual Studio 2012.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Zadanie 1 — Instalowanie programu WebMatrix 2

1. Przejdź do [ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; <em>programu WebMatrix 2</em>&quot;.
2. Polecenie **teraz zainstalować**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Zainstaluj program WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "zainstalować program WebMatrix 2")

    *Instalowanie programu WebMatrix 2*
4. Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](whats-new-in-aspnet-mvc-4/_static/image50.png "akceptowanie umowy licencyjnej")

    *Akceptowanie umowy licencyjnej*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](whats-new-in-aspnet-mvc-4/_static/image51.png "postęp instalacji")

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](whats-new-in-aspnet-mvc-4/_static/image52.png "instalacja została zakończona")

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Zadanie 2 — Instalowanie iPhone symulatora rozszerzenia

1. Uruchom **WebMatrix** i Otwórz istniejącą witrynę sieci Web lub Utwórz nową.
2. Kliknij przycisk **Uruchom** przycisk z **Home** Wstążki i wybierz **Dodaj nowe**.

    ![Dodawanie nowego rozszerzenia programu WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "dodanie nowego rozszerzenia programu WebMatrix")

    *Dodawanie nowego rozszerzenia programu WebMatrix*
3. Wybierz **iPhone symulatora** i kliknij przycisk **zainstalować**.

    ![Przeglądanie rozszerzenia programu WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "rozszerzenia przeglądania programu WebMatrix")

    *Przeglądanie rozszerzenia programu WebMatrix*
4. Informacje dotyczące pakietu, kliknij przycisk **zainstalować** do kontynuowania instalacji rozszerzenia.

    ![rozszerzenie symulatora telefonu iPhone](whats-new-in-aspnet-mvc-4/_static/image55.png "rozszerzenia symulatora telefonu iPhone")

    *rozszerzenie symulatora telefonu iPhone*
5. Przeczytaj i zaakceptuj rozszerzenia umowy licencyjnej.

    ![Rozszerzenia programu WebMatrix umowy licencyjnej](whats-new-in-aspnet-mvc-4/_static/image56.png "rozszerzenia programu WebMatrix umowy licencyjnej")

    *Rozszerzenia programu WebMatrix umowy licencyjnej*
6. Teraz można uruchomić witryny sieci Web z programu WebMatrix za pomocą iPhone opcja symulatora.

    ![Uruchom przy użyciu iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "uruchomić przy użyciu iPhone")

    *Uruchom przy użyciu iPhone*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Zadanie 3 — Konfigurowanie programu Visual Studio 2012 do uruchamiania symulatora telefonu iPhone

1. Otwórz **programu Visual Studio 2012** i otwarcie każdej witryny sieci Web lub Utwórz nowy projekt.
2. Kliknij strzałkę w dół od przycisk Uruchom, a następnie wybierz **przeglądania przy użyciu**.

    ![Przeglądaj z](whats-new-in-aspnet-mvc-4/_static/image58.png "przeglądania przy użyciu")

    *Przeglądaj z*
3. W &quot;przeglądanie za pomocą&quot; okna dialogowego, kliknij przycisk **Dodaj**.
4. W &quot;Dodaj Program&quot; okna dialogowego, użyj następujących wartości:

   - <strong>Program</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (zaktualizować odpowiednio ścieżkę)</em>
   - **Argumenty**: &quot;1&quot;
   - **Przyjazna nazwa**: symulatora telefonu iPhone

     ![Dodaj program](whats-new-in-aspnet-mvc-4/_static/image59.png "Dodaj program")

     *Dodaj program do przeglądania przy użyciu*
5. Kliknij przycisk **OK** i zamknąć okna dialogowe.
6. Jesteś teraz można uruchamiać aplikacje sieci Web w symulatorze iPhone z programu Visual Studio 2012.

    ![Przeglądaj z iPhone symulatora](whats-new-in-aspnet-mvc-4/_static/image60.png "przeglądania przy użyciu symulatora telefonu iPhone")

    *Przeglądania przy użyciu symulatora telefonu iPhone*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek D: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy

Ten dodatek opisano sposób tworzenia nowej witryny sieci web z portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskane wykonując laboratorium, korzystając z funkcji publikowania narzędzia Web Deploy dostarczane przez Windows Azure.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Zadanie 1 — Tworzenie nowej witryny sieci Web systemu Windows portalu Azure

1. Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń Microsoft skojarzonych z Twoją subskrypcją.

    > [!NOTE]
    > Z systemu Windows Azure można udostępniać 10 witryn sieci Web platformy ASP.NET bezpłatnie i następnie Skaluj w miarę zwiększania się ruchu. Możesz utworzyć konto [tutaj](http://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do portalu Windows Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "Zaloguj się do portalu Windows Azure")

    *Zaloguj się do portalu zarządzania platformy Azure z systemem Windows*
2. Kliknij przycisk **nowy** na pasku poleceń.

    ![Tworzenie nowej witryny sieci Web](whats-new-in-aspnet-mvc-4/_static/image62.png "tworzenia nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij przycisk **obliczeniowe** | **witryny sieci Web**. Następnie wybierz **szybkie tworzenie** opcji. Podaj dostępny adres URL dla nowej witryny sieci web, a następnie kliknij przycisk **tworzenie witryny sieci Web**.

    > [!NOTE]
    > Witryny sieci Web systemu Windows Azure jest hostem dla aplikacji sieci web w chmurze, które można kontrolować i zarządzanie nimi. Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do systemu Windows Azure witryny internetowej z spoza portalu. Nie obejmuje kroki konfigurowania bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie](whats-new-in-aspnet-mvc-4/_static/image63.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")

    *Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*
4. Poczekaj na nowe **witryny sieci Web** jest tworzony.
5. Po utworzeniu witryny sieci Web kliknij łącze w obszarze **adres URL** kolumny. Sprawdź, czy działa nowej witryny sieci Web.

    ![Przeglądanie do nowej witryny sieci web](whats-new-in-aspnet-mvc-4/_static/image64.png "przeglądanie do nowej witryny sieci web")

    *Przeglądanie do nowej witryny sieci web*

    ![Witryna sieci Web działa](whats-new-in-aspnet-mvc-4/_static/image65.png "uruchamiania witryny sieci Web")

    *Witryna sieci Web uruchomiona*
6. Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.

    ![Otwieranie stron witryny sieci web zarządzania](whats-new-in-aspnet-mvc-4/_static/image66.png "otwieranie stron zarządzania witryny sieci web")

    *Otwieranie stron zarządzania witryny sieci Web*
7. W **pulpitu nawigacyjnego** w obszarze **szybkiego dostępu** kliknij **pobieranie profilu publikowania** łącza.

    > [!NOTE]
    > *Profilu publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web do witryny sieci Web systemu Windows Azure dla każdej metody włączone publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego z punktów końcowych, dla których włączono metoda publikacji. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **programu Microsoft Visual Studio 2012** obsługują odczytywanie publikowanie profile do zautomatyzowania te programy Publikowanie aplikacji sieci web do witryn sieci Web systemu Windows Azure.

    ![Pobieranie witryny sieci web profilu publikowania](whats-new-in-aspnet-mvc-4/_static/image67.png "pobierania witryny sieci web profilu publikowania")

    *Pobieranie witryny sieci Web profilu publikowania*
8. Pobierz profil publikowania w znanej lokalizacji. Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do witryny sieci Web systemu Windows Azure w programie Visual Studio przy użyciu tego pliku.

    ![Zapisywanie pliku profilu publikowania](whats-new-in-aspnet-mvc-4/_static/image68.png "zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z programu SQL Server baz danych, należy utworzyć serwer bazy danych SQL. Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.

1. Będzie potrzebny serwer bazy danych SQL do przechowywania bazy danych aplikacji. Można wyświetlić serwery bazy danych SQL z subskrypcji usługi Windows Azure Management Portal pod adresem **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**. Jeśli nie masz serwer, który został utworzony, można utworzyć przy użyciu jednego **Dodaj** przycisk paska poleceń. Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, jak będą używane w następnego zadania. Nie należy tworzyć bazy danych jeszcze, jako zostaną utworzone w późniejszym terminie.

    ![Pulpit nawigacyjny serwera bazy danych SQL](whats-new-in-aspnet-mvc-4/_static/image69.png "pulpitu nawigacyjnego serwera bazy danych SQL")

    *Pulpit nawigacyjny serwera bazy danych SQL*
2. W następnym zadaniem Testuj połączenie z bazą danych z programu Visual Studio z tego powodu należy uwzględnić lokalny adres IP serwera liście **dozwolone adresy IP**. Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) przycisku.

    ![Dodawanie adresu IP klienta](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Dodawanie adresu IP klienta*
3. Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP kliknij na **zapisać** o potwierdzenie zmian.

    ![Potwierdzenie zmian](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Potwierdzenie zmian*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 - publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **publikowania**.

    ![Publikowanie aplikacji](whats-new-in-aspnet-mvc-4/_static/image73.png "publikowania aplikacji")

    *Publikowanie witryny sieci web*
2. Zaimportuj profil publikowania, zapisana w pierwszym zadaniu.

    ![Importowanie profilu publikowania](whats-new-in-aspnet-mvc-4/_static/image74.png "Importowanie profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij przycisk **Weryfikacja połączenia z**. Po zakończeniu sprawdzania kliknij **dalej**.

    > [!NOTE]
    > Zakończeniu sprawdzania poprawności, gdy zostanie wyświetlony zielony znacznik wyboru są wyświetlane obok przycisku sprawdzania poprawności połączenia.

    ![Sprawdzanie poprawności połączenia](whats-new-in-aspnet-mvc-4/_static/image75.png "sprawdzanie poprawności połączenia")

    *Sprawdzanie poprawności połączenia*
4. W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk Dalej, aby textbox połączenia bazy danych (tj. **połączenia DefaultConnection**).

    ![Konfiguracja narzędzia Web deploy](whats-new-in-aspnet-mvc-4/_static/image76.png "Konfiguracja narzędzia Web deploy")

    *Konfiguracja narzędzia Web deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W **nazwy serwera** wpisz swoją bazą danych SQL server adresu URL przy użyciu *tcp:* prefiks.
   - W **nazwy użytkownika** wpisz nazwę logowania administratora serwera.
   - W **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nazwę nowej bazy danych, na przykład: *MVC4SampleDB*.

     ![Konfigurowanie parametrów połączenia z lokalizacją docelową](whats-new-in-aspnet-mvc-4/_static/image77.png "Konfigurowanie parametrów połączenia z lokalizacją docelową")

     *Konfigurowanie parametrów połączenia z lokalizacją docelową*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu można utworzyć bazy danych kliknij **tak**.

    ![Tworzenie bazy danych](whats-new-in-aspnet-mvc-4/_static/image78.png "tworzenie parametry bazy danych")

    *Tworzenie bazy danych*
7. Ciągu połączenia używanego do łączenia z bazą danych SQL w systemie Windows Azure jest wyświetlany w pole tekstowe domyślne połączenie. Następnie kliknij przycisk **Dalej**.

    ![Parametry połączenia wskazujące bazę danych SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "ciąg połączenia wskazujące bazę danych SQL")

    *Parametry połączenia wskazujące bazę danych SQL*
8. W **Podgląd** kliknij przycisk **publikowania**.

    ![Publikowanie aplikacji sieci web](whats-new-in-aspnet-mvc-4/_static/image80.png "publikowania aplikacji sieci web")

    *Publikowanie aplikacji sieci web*
9. Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.

    ![Aplikacja opublikowana w systemie Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "aplikacji publikowanych w systemie Windows Azure")

    *Aplikacji publikowanych w systemie Windows Azure*
