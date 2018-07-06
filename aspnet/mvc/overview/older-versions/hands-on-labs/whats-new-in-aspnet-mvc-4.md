---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: Co nowego we wzorcu ASP.NET MVC 4 | Dokumentacja firmy Microsoft
author: rick-anderson
description: ASP.NET MVC 4 to architektura służąca do tworzenia skalowalnych, oparte na standardach aplikacji sieci web przy użyciu wzorców projektowych sprawdzone oraz dzięki możliwościom platformy ASP.NET i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 8862c4da0d881a6f1084317e08697354c0ae6d48
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374107"
---
# <a name="whats-new-in-aspnet-mvc-4"></a>Co nowego we wzorcu ASP.NET MVC 4

Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)

[Pobierz Camp Web szkolenia Kit](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 to architektura służąca do tworzenia skalowalnych, oparte na standardach aplikacji sieci web przy użyciu wzorców projektowych sprawdzone oraz dzięki możliwościom platformy ASP.NET i .NET framework. Ta nowa, czwarty wersji framework koncentruje się na ułatwiając opracowywanie aplikacji mobilnych w sieci web.

Najpierw podczas tworzenia nowego projektu ASP.NET MVC 4 jest teraz szablon projektu aplikacji mobilnych służących do tworzenia aplikacji autonomicznej specjalnie dla urządzeń przenośnych. Ponadto ASP.NET MVC 4 integruje się z jQuery Mobile za pośrednictwem pakietu NuGet jQuery.Mobile.MVC. jQuery Mobile to struktura opartego na języku HTML5 do tworzenia aplikacji sieci web, które są zgodne z wszystkich platform popularnych urządzeń przenośnych, w tym Windows Phone, iPhone, Android i tak dalej. Jednak specjalizacji, należy platformy ASP.NET MVC 4 umożliwia także obsługiwać różne widoki dla różnych urządzeń i podać optymalizacje specyficznych dla urządzenia.

W tym laboratorium praktycznego rozpocznie za pomocą platformy ASP.NET MVC 4 &quot;aplikacji internetowej&quot; szablon projektu, aby utworzyć aplikację Galeria fotografii. Będzie stopniowo ulepszanie aplikacji przy użyciu jQuery Mobile i nowe funkcje platformy ASP.NET MVC 4, aby był zgodny z innego urządzenia przenośne i przeglądarki sieci web. Możesz także informacje dotyczące nowego rozwiązania kodu na potrzeby generowania kodu i jak ASP.NET MVC 4 ułatwia pisanie metod asynchronicznych akcji dzięki obsłudze zadań&lt;ActionResult&gt; zwracanych typów.

> [!NOTE]
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [wersji Microsoft — w sieci Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specyficzne dla tego laboratorium znajduje się w temacie [What's New in formularzy sieci Web w programie ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym praktyczne laboratorium dowiesz się jak:

- Zawiera ulepszenia do platformy ASP.NET MVC projektu szablonów — w tym szablonie projektu aplikacji mobilnej
- Użyj atrybutu okienka ekranu HTML5 i CSS zapytaniami multimediów w celu wyświetlania na urządzeniach przenośnych
- Użyj jQuery Mobile, do ulepszenia stopniowego oraz do tworzenia zoptymalizowanych pod kątem touch interfejsu użytkownika sieci web
- Utwórz widoki specyficzne dla mobile
- Umożliwia przełączanie się między widokami mobilnych i klasycznych aplikacji składnika o przełącznikiem widoku
- Tworzenie kontrolerów asynchroniczne przy użyciu obsługi zadań

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Należy dysponować następującymi elementami do przygotowania tego laboratorium:

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczyt [dodatek B](#AppendixB) instrukcje dotyczące sposobu jego instalacji).
- [ASP.NET MVC 4](../../../mvc4.md) (uwzględniony w instalacji programu Microsoft Visual Studio 2012)
- Emulator Windows Phone (zawarte w [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- Opcjonalnie — [programu WebMatrix 2](https://www.microsoft.com/web/webmatrix/) z **iPhone Electric Plum symulator** rozszerzenia (tylko w przypadku 3 ćwiczenia używany do przeglądania aplikacji sieci web przy użyciu symulatora telefonu iPhone)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

W dokumencie laboratorium należy poinstruować można wstawiać bloki kodu. Dla wygody większość kodu jest dostarczany jako Visual Studio fragmenty kodu, którego można użyć z poziomu programu Visual Studio pozwala uniknąć Dodaj ją ręcznie.

Aby zainstalować fragmenty kodu:

1. Otwórz okno Eksploratora Windows i przejdź do laboratorium **Source\Setup** folderu.
2. Kliknij dwukrotnie **plik Setup.cmd** plik w tym folderze, do zainstalowania fragmenty kodu programu Visual Studio.

Jeśli nie jesteś zaznajomiony z fragmentów kodu w usłudze Visual Studio i chcesz dowiedzieć się, jak z nich korzystać, możesz zapoznać się z dodatku z tego dokumentu &quot; [dodatek A: za pomocą wstawek kodu programu](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To ćwiczenie praktyczne obejmuje następujących czynnościach:

1. [Nowe szablony projektów programu ASP.NET MVC 4](#Exercise1)
2. [Tworzenie aplikacji sieci Web galerii fotografii](#Exercise2)
3. [Dodano obsługę urządzeń przenośnych](#Exercise3)
4. [Za pomocą kontrolerów asynchroniczne](#Exercise4)

> [!NOTE]
> Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązania, należy uzyskać po ukończeniu ćwiczenia. Jeśli potrzebujesz dodatkowej pomocy ćwiczeń opisanych w dalszej, można użyć tego rozwiązania jako wskazówki.


Szacowany czas do ukończenia tego laboratorium: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Ćwiczenie 1: Nowe szablony projektów programu ASP.NET MVC 4

W tym ćwiczeniu przedstawimy rozszerzenia w szablonach projektu programu ASP.NET MVC 4. Oprócz szablonów aplikacji internetowych już istnieje w MVC 3, ta wersja zawiera teraz oddzielne szablonu dla aplikacji mobilnych. Najpierw przedstawimy niektóre istotne funkcje każdego z szablonów. Następnie będzie działać na renderowanie strony poprawnie na różnych platformach przy użyciu właściwej metody postępowania.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Zadanie 1 — poznawanie szablon aplikacji internetowych

1. Otwórz **programu Visual Studio**.
2. Wybierz **pliku | Nowe | Projekt** polecenia menu. W **nowy projekt** okno dialogowe, wybierz opcję **Visual C# | Web** szablonu w okienku po lewej stronie drzewa i wybierz polecenie **aplikacji sieci Web programu ASP.NET MVC 4.** Nadaj projektowi nazwę **PhotoGallery**, wybierz lokalizację (lub pozostaw wartość domyślną) i kliknij przycisk **OK**.

    > [!NOTE]
    > Później będzie dostosować rozwiązanie PhotoGallery platformy ASP.NET MVC 4, które są teraz tworzenie.

    ![Tworzenie nowego projektu](whats-new-in-aspnet-mvc-4/_static/image1.png "tworzenia nowego projektu")

    *Tworzenie nowego projektu*
3. W **nowego projektu programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej** szablonu projektu i kliknij przycisk **OK**. Upewnij się, że został wybrany aparat widoku Razor.

    ![Tworzenie nowej platformy ASP.NET MVC 4 Internet aplikacji](whats-new-in-aspnet-mvc-4/_static/image2.png "Tworzenie nowej aplikacji internetowych 4 platformy ASP.NET MVC")

    *Tworzenie nowej aplikacji internetowych 4 platformy ASP.NET MVC*

    > [!NOTE]
    > Składnia razor został wprowadzony w ASP.NET MVC 3. Jej celem jest zminimalizowanie liczby znaków i naciśnięć klawiszy wymagane w pliku, umożliwiając szybkie i płynów, przepływ pracy kodowania. Razor wykorzystuje istniejące C# / VB (lub innych) umiejętności językowe i oferuje składni znaczników szablonu, która włącza awesome przepływ pracy tworzenia kodu HTML.
4. Naciśnij klawisz **F5** do uruchamiania rozwiązania i szablony w nim odnowione. Możesz sprawdzić następujące funkcje:

    **Szablony aplikacji w stylu Modern**

    Szablony Odnowiono, podając więcej styli nowoczesnym wyglądzie.

    ![Szablony ASP.NET MVC 4 restyled](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled szablonów")

    *Szablony ASP.NET MVC 4 restyled*

    ![Nowa strona Skontaktuj się z pomocą](whats-new-in-aspnet-mvc-4/_static/image4.png "strony nowy kontakt")

    *Nowa strona kontaktu*

    **Funkcje adaptacyjnego sterowania renderowania**

    Zapoznaj się z zmiany rozmiaru okna przeglądarki i zwróć uwagę, jak układ strony dynamicznie dostosowuje się do nowego rozmiaru okna. Te szablony umożliwiają technika adaptacyjne renderowania są prawidłowo renderowane na platformach zarówno komputerów i urządzeń przenośnych, bez potrzeby dostosowywania.

    ![Szablon projektu platformy ASP.NET MVC 4 w innej przeglądarce rozmiarach](whats-new-in-aspnet-mvc-4/_static/image5.png "szablon projektu platformy ASP.NET MVC 4 w rozmiarach innej przeglądarki")

    *Szablon projektu platformy ASP.NET MVC 4 w rozmiarach innej przeglądarki*

    **Bardziej rozbudowane interfejsu użytkownika za pomocą języka JavaScript**

    Innym usprawnieniem domyślne szablony projektów polega na użyciu języka JavaScript, aby zapewnić bardziej interaktywny język JavaScript. Logowanie i rejestrowanie linków używany w szablonie spróbujemy jak sprawdzanie poprawności danych wejściowych pola po stronie klienta za pomocą jQuery operacji sprawdzania poprawności.

    ![jQuery sprawdzania poprawności](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *jQuery sprawdzania poprawności*

    > [!NOTE]
    > Zwróć uwagę, że dwa Zaloguj się w sekcji, w pierwszej sekcji można Zaloguj się przy użyciu konta zarejestrowany z lokacji i w drugiej sekcji, możesz altenativelly logowanie się za pomocą innej usługi uwierzytelniania, takich jak google (domyślnie wyłączone).
5. Zamknij przeglądarkę, aby zatrzymać debuger i powrócić do programu Visual Studio.
6. Otwórz plik **AuthConfig.cs** znajdujący się w folderze **aplikacji\_Start** folderu.
7. Usuń komentarz z ostatniego wiersza, aby zarejestrować klienta Google *OAuth* uwierzytelniania.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Zauważ, że możesz łatwo włączyć uwierzytelnianie przy użyciu dowolnej usługi OpenID lub OAuth, takich jak Facebook, Twitter, Microsoft, itp.
8. Naciśnij klawisz **F5** uruchamianie rozwiązania i przejdź do strony logowania.
9. Wybierz **Google** usługi, aby zalogować się.

    ![Zaznaczanie w dzienniku w usłudze](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *Zaznaczanie w dzienniku w usłudze*
10. Zaloguj się przy użyciu swojego konta Google.
11. Zezwalaj na lokacji (localhost) pobrać informacje z konta Google.
12. Na koniec należy zarejestrować się w lokacji, aby skojarzyć konto Google.

   ![Skojarz konto Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Kojarzenie konta Google*
13. Zamknij przeglądarkę, aby zatrzymać debuger i powrócić do programu Visual Studio.
14. Teraz możesz eksplorować rozwiązanie, zapoznaj się z niektórych innych nowych funkcji wprowadzonych przez program ASP.NET MVC 4 w szablonie projektu.

   ![Szablon projektu aplikacji internetowych platformy ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image9.png "szablon projektu aplikacji internetowych platformy ASP.NET MVC 4")

   *Szablon projektu aplikacji internetowych platformy ASP.NET MVC 4*

   - **HTML 5 znaczników**

       Przeglądaj widoków szablonu, aby dowiedzieć się, nowych znaczników motywu.

       ![Nowy szablon, za pomocą znacznika About.cshtml Razor i HTML5. ](whats-new-in-aspnet-mvc-4/_static/image10.png "Nowego szablonu, za pomocą znacznika About.cshtml Razor i HTML5.")

       *Nowy szablon przy użyciu znaczników Razor i HTML5 (About.cshtml).*
   - **Zaktualizowano bibliotek JavaScript**

       Domyślny szablon platformy ASP.NET MVC 4 zawiera teraz KnockoutJS i struktura JavaScript MVVM, która umożliwia tworzenie rozbudowanych aplikacji internetowych o wysokiej dynamice za pomocą języków JavaScript i HTML. Podobnie jak w MVC3, jQuery i biblioteki interfejsu użytkownika jQuery znajdują się również w ASP.NET MVC 4.

     > [!NOTE]
     > Można uzyskać więcej informacji na temat biblioteki KnockOutJS w to łącze: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/). Ponadto, możesz dowiedzieć się jQuery i interfejs użytkownika jQuery w [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Zadanie 2 — Eksplorowanie szablonu aplikacji mobilnej

ASP.NET MVC 4 ułatwia tworzenie witryn sieci Web dla urządzeń przenośnych i tabletów przeglądarki. Ten szablon ma tę samą strukturę aplikacji jako szablon aplikacji internetowych (Zwróć uwagę, że kod kontrolera jest praktycznie identyczny), ale jego styl został zmodyfikowany i są prawidłowo renderowane na urządzeniach przenośnych oparte na dotyku.

1. Wybierz **pliku | Nowe | Projekt** polecenia menu. W **nowy projekt** okno dialogowe, wybierz opcję **Visual C# | Web** szablonu w okienku po lewej stronie drzewa i wybierz polecenie **aplikacji sieci Web programu ASP.NET MVC 4.** Nadaj projektowi nazwę **PhotoGallery.Mobile**, wybierz lokalizację (lub pozostaw wartość domyślną) wybierz &quot;Dodaj do rozwiązania&quot; i kliknij przycisk **OK**.
2. W **nowego projektu programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji Mobile** szablonu projektu i kliknij przycisk **OK**. Upewnij się, że został wybrany aparat widoku Razor.

    ![Tworzenie nowej aplikacji mobilnej 4 platformy ASP.NET MVC](whats-new-in-aspnet-mvc-4/_static/image11.png "Tworzenie nowej aplikacji mobilnej 4 platformy ASP.NET MVC")

    *Tworzenie nowej aplikacji mobilnej 4 platformy ASP.NET MVC*
3. Teraz jesteś w stanie rozwiązania i zapoznaj się z nowych funkcji wprowadzonych przez szablon rozwiązania platformy ASP.NET MVC 4 dla urządzeń przenośnych:

    - **jQuery Mobile biblioteki**

        Szablon projektu aplikacji mobilnych zawiera przenośne biblioteki jQuery, która to biblioteka typu open source dla zgodności w przeglądarce dla urządzeń przenośnych. jQuery Mobile dotyczy stopniowym rozszerzaniu funkcji przeglądarki dla urządzeń przenośnych, które obsługują CSS i JavaScript. Stopniowym rozszerzaniu funkcji włącza wszystkie przeglądarki wyświetlić podstawowe elementy strony sieci web podczas włącza to tylko najbardziej zaawansowane przeglądarek do wyświetlenia sformatowanej zawartości. Pliki JavaScript i CSS uwzględnione w jQuery przenośnych styl pomóc przeglądarek urządzeń przenośnych w celu dopasowania do zawartości na ekranie bez wprowadzania żadnych zmian w znaczniku strony.

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *przenośne biblioteki jQuery, zawarte w szablonie*
    - **HTML5, na podstawie znaczników**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Szablon aplikacji mobilnych za pomocą znaczników języka HTML5, (Login.cshtml i index.cshtml)*
4. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.
5. Otwórz **Windows Phone 7 Emulator**.
6. Na ekranie początkowym telefonu Otwórz program Internet Explorer. Zapoznaj się z adresu URL, gdzie uruchomienia aplikacji pulpitu, a następnie przejdź do tego adresu URL w telefonie (np. `http://localhost:[PortNumber]/`).
7. Teraz można wprowadzić na stronie logowania, lub sprawdź informacje o stronie. Należy zauważyć, że stylu witryny sieci Web jest oparty na nową aplikację Metro dla urządzeń przenośnych. Szablon projektu platformy ASP.NET MVC 4 jest prawidłowo wyświetlany na urządzeniach przenośnych, upewniając się, że wszystkie elementy strony są widoczne i włączone. Należy zauważyć, że linki w nagłówku są wystarczająco duże, aby być kliknięte lub naciśnięte.

    ![Projekt szablonu strony w urządzeniu przenośnym](whats-new-in-aspnet-mvc-4/_static/image14.png "projektu szablonu strony w urządzeniu przenośnym")

    *Projekt szablonu strony w urządzeniu przenośnym*
8. Nowy szablon używa również **okienka ekranu metatag**. Przeglądarki dla większości urządzeń przenośnych zdefiniować szerokość okna przeglądarki wirtualnej lub &quot;okienka ekranu&quot;, którego rozmiar jest większy niż rzeczywista szerokość urządzenia przenośnego. Dzięki temu przeglądarek urządzeń przenośnych wyświetlić całą stronę sieci web wewnątrz wirtualnych wyświetlania. **Okienka ekranu metatag** umożliwia deweloperom sieci web ustawić szerokość, wysokość i skalowania obszarze przeglądarki na urządzeniach przenośnych **.** Szablon platformy ASP.NET MVC 4 dla aplikacji mobilnych Ustawia szerokość urządzenia okienka ekranu (&quot;szerokość = szerokość urządzenia&quot;) w szablonie układu (*Views\Shared\_Layout.cshtml*), dzięki czemu wszystkie strony będą mieć ich okienka ekranu, Ustaw szerokość ekranu urządzenia. Zwróć uwagę, czy okienko ekranu metatag nie zmieni się domyślny widok przeglądarki.
9. Otwórz  **\_Layout.cshtml**, który znajduje się w **widoków | Udostępnione** folder i komentarzy metatagów okienka ekranu. Uruchomienia aplikacji, w przeciwnym razie już otwarty i zapoznaj się z różnic.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![Witryna po komentowania metatag okienka ekranu](whats-new-in-aspnet-mvc-4/_static/image15.png "lokacji po komentowania metatag okienka ekranu")

*Witryna po komentowania metatag okienka ekranu*
10. W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** Aby zatrzymać debugowanie aplikacji.
11. Usuń znaczniki komentarza metatag okienka ekranu.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Zadanie 3 — Adaptacyjna renderowaniem

W tym zadaniu dowiesz się, innej metody do renderowania strony sieci Web prawidłowo na urządzeniach przenośnych i przeglądarki sieci Web, w tym samym czasie, bez potrzeby dostosowywania. O podobnej tematyce została już użyta metatag okienka ekranu. Teraz będzie spełniać inną zaawansowaną metodą: *adaptacyjne renderowania*.

Adaptacyjne renderowania jest techniką, która używa **zapytaniami multimediów CSS3** Aby dostosować styl stosowany do strony. Zapytaniami multimediów Zdefiniuj warunki wewnątrz arkusza stylów, grupowanie style CSS w ramach określonego warunku. Tylko wtedy, gdy warunek ma wartość true, zastosowano ten styl do obiektów zadeklarowane.

Elastyczności, jaką technika adaptacyjne renderowania umożliwia wszelkie dostosowania do wyświetlania witryny na różnych urządzeniach. Można zdefiniować dowolną liczbę style jak, w arkuszu stylów pojedynczego bez konieczności pisania kodu logiki, aby wybrać styl. Dlatego jest bardzo przejrzysty sposób dostosowania strony stylów, ponieważ zmniejsza to ilość zduplikowany kodem i logikę do celów renderowania. Z drugiej strony zużycie przepustowości wzrosną, gdy rozmiar plików CSS może nieznacznie wzrosną.

Za pomocą techniki adaptacyjne renderowania, zostanie wyświetlona witryna **wyświetlane prawidłowo, niezależnie od tego, w przeglądarce.** Jednak należy rozważyć, czy przepustowość dodatkowego obciążenia jest istotna.

> [!NOTE]
> Jest podstawowy format zapytanie o multimedia: @media \[zakres: wszystkie | handheld | drukowania | projekcji | ekranu\] ([właściwość: wartość] i... [właściwość: wartość])


Przykłady z zapytaniami multimediów: &gt;  <strong>@media wszystkie i (maksymalna szerokość: 1000px) i (minimalna szerokość: 700px) {}:</strong> dla wszystkich rozdzielczościach między 700px i 1000px.

> <strong>@media ekran i (minimalna szerokość: 400 piks.) i (maksymalna szerokość: 700px) {…}:</strong> tylko dla ekranów. Rozwiązanie musi wynosić od 400 do 700px.
> 
> <strong>@media urządzenia przenośnego i (minimalna szerokość: 20em), ekran i (minimalna szerokość: 20em) {…}:</strong> ekranu i urządzenia (mobile i urządzenia). Minimalna szerokość musi być większa niż 20em.
> 
> Więcej informacji na ten temat można znaleźć na [lokacji W3C](http://www.w3.org/TR/css3-mediaqueries/).


Możesz teraz przedstawimy działania adaptacyjne renderowania, poprawa czytelności platformy ASP.NET MVC 4 domyślny szablon witryny sieci Web.

1. Otwórz **PhotoGallery.sln** rozwiązania, które zostały utworzone w zadaniu 1 i wybierz **PhotoGallery** projektu. Naciśnij klawisz **F5** Aby uruchomić rozwiązanie.
2. Zmienia szerokość w przeglądarce, ustawienia systemu windows o połowę lub mniej niż czwarta oryginalnego rozmiaru. Zwróć uwagę, co się dzieje z elementami w nagłówku: niektóre elementy nie będą widoczne w obszaru nagłówka.
3. Otwórz <strong>Site.css</strong> plik z Eksploratora rozwiązania Visual Studio, na terenie <strong>zawartości</strong> folderu projektu. Naciśnij klawisz <strong>CTRL + F</strong> aby otworzyć wyszukiwanie zintegrowane z Visual Studio i zapisać <strong>@media</strong> zlokalizować <strong>CSS media query</strong>.

    Warunek kwerendy nośnika, które są zdefiniowane w tym szablonie działa w ten sposób: gdy rozmiar okna przeglądarki jest poniżej **850 px**, reguły CSS stosowane są zdefiniowane w tym bloku nośnika.

    ![Lokalizowanie zapytanie o multimedia](whats-new-in-aspnet-mvc-4/_static/image16.png "lokalizowanie zapytanie o multimedia")

    *Lokalizowanie zapytanie o multimedia*
4. Zastąp wartość atrybutu maksymalna szerokość 850 pikseli z **10px**, aby wyłączyć funkcje adaptacyjnego sterowania renderowanie i naciśnij klawisz **CTRL + S** Aby zapisać zmiany. Wróć do przeglądarki, a następnie naciśnij klawisz **klawiszy CTRL + F5** Aby odświeżyć stronę za pomocą wprowadzone zmiany. Zwróć uwagę na różnice w obie strony, zmieniając szerokości okna.

    ![Po lewej stronie, strony są stosowane @media styl w prawo, styl zostanie pominięty](whats-new-in-aspnet-mvc-4/_static/image17.png "po lewej stronie, strony są stosowane @media styl w prawo, styl zostanie pominięty.")

    <em>W lewej stronie są stosowane @media styl w prawo, styl zostanie pominięty.</em>

    Teraz sprawdźmy co się dzieje na urządzeniach przenośnych:

    ![Po lewej stronie, strony są stosowane @media styl w prawo, styl zostanie pominięty](whats-new-in-aspnet-mvc-4/_static/image18.png "po lewej stronie, strony są stosowane @media styl w prawo, styl zostanie pominięty.")

    <em>W lewej stronie są stosowane @media styl w prawo, styl zostanie pominięty.</em>

    Mimo że można zauważyć, że zmiany, gdy strona jest wyświetlana w przeglądarce sieci Web nie są bardzo istotne, gdy na urządzeniu przenośnym, różnice stają się bardziej oczywiste. Po lewej stronie obrazu widać, że styl niestandardowy czytelne.

    Funkcje adaptacyjnego sterowania renderowanie może służyć w wielu scenariuszach więcej, ułatwiając stosować warunkowego Ustawianie stylów w witrynie sieci Web i rozwiązywania typowych problemów stylu dzięki podejściu ładnie.

    Tag meta okienka ekranu i zapytaniami multimediów CSS nie są specyficzne dla platformy ASP.NET MVC 4, więc w dowolnej aplikacji sieci web mogą korzystać z tych funkcji.
5. W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** Aby zatrzymać debugowanie aplikacji.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Ćwiczenie 2: Tworzenie aplikacji sieci Web galerii fotografii

W tym ćwiczeniu będzie działać w aplikacji Galeria fotografii, aby wyświetlić zdjęcia. Rozpocznie się za pomocą szablonu projektu ASP.NET MVC 4, a następnie dodasz funkcję do pobrania zdjęcia z usługi i wyświetlać je na stronie głównej.

W poniższym ćwiczeniu zostanie zaktualizowana tego rozwiązania, aby ulepszyć sposób wyświetlania na urządzeniach przenośnych.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Zadanie 1 — Tworzenie usługi makiety zdjęć

W tym zadaniu utworzysz projekt usługi zdjęcie można pobrać zawartości, która będzie wyświetlana w galerii. Aby to zrobić, dodasz nowy kontroler, który po prostu zwraca plik w formacie JSON przy użyciu danych każdego zdjęcia.

1. Otwórz **programu Visual Studio** Jeśli nie jest jeszcze otwarty.
2. Wybierz **pliku | Nowe | Projekt** polecenia menu. W **nowy projekt** okno dialogowe, wybierz opcję **Visual C# | Web** szablonu w okienku po lewej stronie drzewa i wybierz polecenie **aplikacji sieci Web programu ASP.NET MVC 4.** Nadaj projektowi nazwę **PhotoGallery**, wybierz lokalizację (lub pozostaw wartość domyślną) i kliknij przycisk **OK**. Alternatywnie, można kontynuować pracę ze swojej istniejącej platformy ASP.NET MVC 4 **aplikacji internetowej** rozwiązania z **ćwiczenie 1** , a następnie przejdź do następnego kroku.
3. W **nowego projektu programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej** szablonu projektu i kliknij przycisk **OK**. Upewnij się, że masz wybrane jako aparat widoku Razor.
4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **aplikacji\_danych** folderu projektu i wybierz **Dodaj | Istniejący element**. Przejdź do **Source\Assets\App\_danych** folder tego laboratorium i Dodaj **Photos.json** pliku.
5. Utwórz nowy kontroler o nazwie **PhotoController**. Aby to zrobić, kliknij prawym przyciskiem myszy **kontrolerów** przejdź do folderu **Dodaj** i wybierz **kontrolera.** Pełna nazwa kontrolera, pozostaw **pusty kontroler MVC** szablon i kliknij przycisk **Dodaj**.

    ![Dodawanie PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Dodawanie PhotoController")

    *Dodawanie PhotoController*
6. Zastąp **indeksu** metody za pomocą następujących **galerii** akcji i powrocie zawartość z pliku JSON, ostatnio dodane do projektu.

    (Code Snippet — *platformy ASP.NET MVC 4 laboratorium — Ex02 — Galeria akcji*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Naciśnij klawisz **F5** uruchomić rozwiązania, a następnie przejdź do następującego adresu URL w celu przetestowania usługi pozorowane zdjęć: `http://localhost:[port]/photo/gallery` (wartość [port] zależy od bieżącego portu, w którym aplikacja została uruchomiona). Pobierz zawartość żądania do tego adresu URL **Photos.json** pliku.

    ![Testowanie usługi pozorowane zdjęcie](whats-new-in-aspnet-mvc-4/_static/image20.png "testowania usługi pozorowane zdjęć")

    *Testowanie usługi pozorowane zdjęć*

W rzeczywistej implementacji może używać [ASP.NET Web API](../../../../web-api/index.md) do implementacji usługi Galeria fotografii. Web API platformy ASP.NET to platforma, która ułatwia tworzenie usług HTTP, docierających do szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych. Web API platformy ASP.NET jest idealną platformą do tworzenia aplikacji typu RESTful na .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Zadanie 2 — wyświetlanie Galeria fotografii

To zadanie zaktualizuje stronę główną, aby pokazać Galeria fotografii przy użyciu usługi pozorowane, utworzonego w pierwszym zadaniu, w tym ćwiczeniu. Spowoduje dodanie plików modelu i aktualizowanie widoków galerii.

1. W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** Aby zatrzymać debugowanie aplikacji.
2. Tworzenie **zdjęcie** klasy w **modeli** folderu. Aby to zrobić, kliknij prawym przyciskiem myszy **modeli** folderu, wybierz **Dodaj** i kliknij przycisk **klasy**. Następnie ustaw nazwę na **Photo.cs** i kliknij przycisk **Dodaj**.
3. Dodaj następujące elementy członkowskie do **zdjęcie** klasy.

    (Code Snippet — *modelu zdjęcie platformy ASP.NET MVC 4 laboratorium - Ex02 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Otwórz **HomeController.cs** plik wchodzącej w skład **kontrolerów** folderu.
5. Dodaj następujące instrukcje using.

    (Code Snippet — *deklaracje Using HomeController laboratorium - Ex02 - platformy ASP.NET MVC 4*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Aktualizacja **indeksu** akcję do użycia **HttpClient** do pobierania danych galerii, a następnie użyj **JavaScriptSerializer** do deserializacji do modelu widoku.

    (Code Snippet — *akcji indeksu laboratorium - Ex02 - platformy ASP.NET MVC 4*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Otwórz **Index.cshtml** plik znajdujący się w obszarze **Views\Home** folder i Zastąp całą zawartość następującym kodem.

    Ten kod w pętli wszystkich zdjęć, które są pobierane z usługi i wyświetla je do listy nieuporządkowanej.

    (Code Snippet — *platformy ASP.NET MVC 4 Lab - Ex02 - zdjęcie List*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **zawartości** folderu projektu i wybierz **Dodaj | Istniejący element**. Przejdź do **Source\Assets\Content** folder tego laboratorium i Dodaj **Site.css** pliku. Należy potwierdzić jego zastąpienie. Jeśli masz **Site.css** plik jest otwarty, musisz potwierdzić również ponowne załadowanie pliku.
9. Otwórz Eksplorator plików i skopiować całą **zdjęcia** folder znajdujący się w **Source\Assets** folder tego laboratorium do głównego folderu projektu w Eksploratorze rozwiązań.
10. Uruchom aplikację. Powinny pojawić na stronie głównej wyświetlania zdjęcia w galerii.

    ![Galeria fotografii](whats-new-in-aspnet-mvc-4/_static/image21.png "Galeria fotografii")

    *Galeria fotografii*
11. W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** Aby zatrzymać debugowanie aplikacji.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Ćwiczenie 3: Dodano obsługę urządzeń przenośnych

Jednym z kluczowych aktualizacji w programie ASP.NET MVC 4 jest obsługa opracowywania aplikacji mobilnych. W tym ćwiczeniu zostanie Eksploruj nowe funkcje platformy ASP.NET MVC 4 dla aplikacji mobilnych, rozszerzając rozwiązanie PhotoGallery, które zostały utworzone w poprzednim ćwiczeniu.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Zadanie 1 — Instalowanie jQuery Mobile w aplikacji ASP.NET MVC 4

1. Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex3-MobileSupport/rozpoczęcia/** folderu. W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. Otwórz **Konsola Menedżera pakietów** , klikając **narzędzia** &gt; **Menedżer pakietów biblioteki** &gt; **Menedżera pakietów Konsola** opcji menu.

    ![Otwieranie konsoli Menedżera pakietów NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "otwierania konsoli Menedżera pakietów NuGet")

    *Otwieranie konsoli Menedżera pakietów NuGet*
3. W konsoli Menedżera pakietów, uruchom następujące polecenie, aby zainstalować **jQuery.Mobile.MVC** pakietu.

    jQuery Mobile to biblioteka typu open source do tworzenia zoptymalizowanych pod kątem touch interfejsu użytkownika sieci web. Pakiet NuGet jQuery.Mobile.MVC obejmuje pomocników jQuery Mobile za pomocą aplikacji platformy ASP.NET MVC 4.

    > [!NOTE]
    > Za pomocą następującego polecenia, będzie odbywać się pobieranie biblioteki jQuery.Mobile.MVC z pakietów Nuget.

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    To polecenie powoduje zainstalowanie jQuery Mobile, a niektóre pliki pomocnicze, takie jak następujące:

    - **Widoki/Shared/\_Layout.Mobile.cshtml**: jest zoptymalizowane pod kątem mniejszego ekranu jQuery Mobile na podstawie układu. Gdy usługa witryny sieci Web odbiera żądanie, w przeglądarce dla urządzeń przenośnych, spowoduje zastąpienie oryginalnego układu (\_Layout.cshtml) z tym kontem.
    - Składnik przełącznikiem widoku: składa się z **widoków/Shared/\_ViewSwitcher.cshtml** widok częściowy i **ViewSwitcherController.cs** kontrolera. Ten składnik będzie wyświetlany link w przeglądarkach dla urządzeń przenośnych umożliwiający użytkownikom przełączyć się do klasycznej wersji strony.  
        ![Galeria fotografii projektu za pomocą obsługę operacji mobilnych](whats-new-in-aspnet-mvc-4/_static/image23.png "Galeria fotografii projektu za pomocą Obsługa technologii mobilnych")

        *Galeria fotografii projektu za pomocą Obsługa technologii mobilnych*
4. Zarejestruj pakiety mobilnych. Aby to zrobić, otwórz **Global.asax.cs** pliku i Dodaj następujący wiersz.

    (Code Snippet — *platformy ASP.NET MVC 4 laboratorium — Ex03 — Zarejestruj przenośnych pakiety*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Uruchom aplikację za pomocą przeglądarki sieci web.
6. Otwórz **Windows Phone 7 Emulator,** na terenie **Start Menu | Wszystkie programy | Windows Phone SDK 7.1 | Emulator Windows Phone.**
7. Na ekranie początkowym telefonu Otwórz program Internet Explorer. Zapoznaj się z adresu URL, w którym aplikacja jest uruchomiona, a następnie przejdź do tego adresu URL w przeglądarce telefonu (np. `http://localhost:[PortNumber]/`).

    Zauważysz, że aplikacja będzie wyglądać inne w przypadku emulator Windows Phone utworzonemu jQuery.Mobile.MVC ma nowe zasoby w projekcie, który Pokaż widoki zoptymalizowane pod kątem urządzeń przenośnych.

    Zwróć uwagę, komunikat w górnej części przez telefon, przedstawiający link przełącza do widoku pulpitu. Ponadto  **\_Layout.Mobile.cshtml** układ, który został utworzony przez pakiet zainstalowano obejmują inny układ w aplikacji.

    > [!NOTE]
    > Do tej pory nie ma żadnego połączenia, aby wrócić do widoku dla urządzeń przenośnych. Zostanie uwzględniony w nowszych wersjach.

    ![Widok strony głównej galerii fotografii dla urządzeń przenośnych](whats-new-in-aspnet-mvc-4/_static/image24.png "widoku strony głównej galerii fotografii dla urządzeń przenośnych")

    *Widok strony głównej galerii fotografii dla urządzeń przenośnych*
8. W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** Aby zatrzymać debugowanie aplikacji.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Zadanie 2 — Tworzenie przenośnych widoków

W tym zadaniu utworzysz mobilnej wersji widoku indeksu zawartości zaadaptować pod kątem lepszych appareance na urządzeniach przenośnych.

1. Kopiuj **Views\Home\Index.cshtml** wyświetlanie i wklej go, aby utworzyć kopię, Zmień nazwę nowego pliku do **Index.Mobile.cshtml**.
2. Otwórz utworzony nowy **Index.Mobile.cshtml** wyświetlanie i Zastąp istniejące &lt;ul&gt; tagu przy użyciu tego kodu. W ten sposób zostanie zaktualizowany &lt;ul&gt; tagu przy użyciu adnotacji danych mobilnych jQuery używać przenośnych motywy z jQuery.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Należy zauważyć, że:
    > 
    > - **Roli danych** ustawioną wartość atrybutu **listview** spowoduje, że listy za pomocą ListView — style.
    > 
    > - **Wstawki danych** pokazuje listę z obramowaniem zaokrąglone i margines atrybut ma wartość true.
    > 
    > - **Filtr danych** ustawioną wartość atrybutu **true** wygeneruje pola wyszukiwania.
    > 
    > Można poznać więcej informacji na temat Konwencji przenośnych jQuery w dokumentacji projektu: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.
4. Przełącz się do **Emulator Windows Phone** i Odśwież witrynę. Zwróć uwagę, nowy wygląd listy galerii, a także nowe pole wyszukiwania znajduje się w górnej części. Następnie wpisz wyraz w polu wyszukiwania (na przykład **tulipanów**) można uruchomić wyszukiwanie w Galerii fotografii.

    ![Galeria przy użyciu stylu listview przy użyciu filtrowania](whats-new-in-aspnet-mvc-4/_static/image25.png "galerii przy użyciu stylu listview przy użyciu filtrowania")

    *Galeria przy użyciu stylu listview przy użyciu filtrowania*

    Aby podsumować, wykorzystano przepisu Mobilizer widok, aby utworzyć kopię widoku indeksu za pomocą &quot;przenośnych&quot; sufiks. Ten sufiks wskazuje platformy ASP.NET MVC 4, że każde żądanie generowane z urządzenia przenośnego będzie używał kopii tego indeksu. Ponadto zaktualizowano mobilnej wersji widoku indeksu, aby użyć jQuery Mobile związane z poprawianiem lokacji wygląd i działanie na urządzeniach przenośnych.
5. Wróć do programu Visual Studio i Otwórz **Site.Mobile.css** znajdujący się w folderze **zawartości** folderu.
6. Napraw pozycjonowanie tytuł zdjęcie, aby wyświetlić je po prawej stronie obrazu. Aby to zrobić, Dodaj następujący kod do **Site.Mobile.css** pliku.

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.
8. Przejdź z powrotem do **Emulator Windows Phone** i Odśwież witrynę. Zwróć uwagę, że tytuł fotografii prawidłowo znajduje się teraz.

    ![Tytuł umieszczony po prawej stronie obrazu](whats-new-in-aspnet-mvc-4/_static/image26.png "tytuł umieszczony po prawej stronie obrazu")

    *Tytuł umieszczony po prawej stronie obrazu*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Zadanie 3 - jQuery Mobile motywów

Każdy układ i elemencie widget jQuery Mobile został opracowany na nowe zorientowane obiektowo CSS strukturę, która pozwala na zastosowanie motywu pełną ujednoliconego projektowania wizualnego do witryn i aplikacji.

jQuery Mobile domyślny motyw obejmuje 5 próbki, które są określone litery (, b, c, d, e) krótki.

W tym zadaniu zostanie zaktualizowana przenośnych układ, aby użyć innego motywu niż domyślna.

1. Przełącz się do programu Visual Studio.
2. Otwórz  **\_Layout.Mobile.cshtml** plik znajdujący się w **Views\Shared**.
3. Znajdź div element z roli danych równa &quot;strony&quot; i zaktualizuj **data-theme** do &quot; **e**&quot;.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.
5. Odśwież witrynę w **Emulator Windows Phone** i zwróć uwagę, nowy schemat kolorów.

    ![Układu dla urządzeń przenośnych z systemem innym kolorze](whats-new-in-aspnet-mvc-4/_static/image27.png "układu dla urządzeń przenośnych za pomocą innego schematu kolorów")

    *Układu dla urządzeń przenośnych za pomocą innego schematu kolorów*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Zadanie 4 — za pomocą składnika o przełącznikiem widoku i przeglądarki zastępowanie funkcji

Konwencja zoptymalizowane pod kątem mobile stron sieci web jest dodać łącze, którego tekst jest coś, takich jak widok pulpitu lub w trybie witrynę w trybie pełnym, który umożliwia użytkownikom, przełącz się do klasycznej wersji strony. Pakiet jQuery.Mobile.MVC zawiera przykładowe **przełącznikiem widoku** składnika w tym celu używane w  **\_Layout.Mobile.cshtml** widoku.

![Łącze, aby przełączyć do widoku pulpitu](whats-new-in-aspnet-mvc-4/_static/image28.png "łącze, aby przełączyć do widoku pulpitu")

*Link, aby przełączyć do widoku pulpitu*

Korzysta z tym przełącznikiem widoku nową funkcję o nazwie **zastępowania przeglądarki**. Ta funkcja umożliwia aplikacji taką obsługę żądań, tak jakby pochodziły one z innej przeglądarki (agenta użytkownika) niż ten, który w rzeczywistości pochodzą one z.

W tym zadaniu przedstawimy przykład implementacji dodane przez jQuery.Mobile.MVC i Nowa przeglądarka zastępowanie funkcji na platformie ASP.NET MVC 4 przełącznikiem widoku.

1. Przełącz się do programu Visual Studio.
2. Otwórz  **\_Layout.Mobile.cshtml** widoku znajdujący się w folderze **Views\Shared** folderu i zwróć uwagę, składnik przełącznikiem widoku, do którego nastąpiło odwołanie jako widok częściowy.

    ![Układu dla urządzeń przenośnych za pomocą składnika o przełącznikiem widoku](whats-new-in-aspnet-mvc-4/_static/image29.png "układu dla urządzeń przenośnych za pomocą składnika o przełącznikiem widoku")

    *Za pomocą składnika o przełącznikiem widoku układu dla urządzeń przenośnych*
3. Otwórz  **\_ViewSwitcher.cshtml** widoku częściowego.

    Widok częściowy używa nowej metody **ViewContext.HttpContext.GetOverriddenBrowser()** do ustalenia źródła pochodzenia żądania sieci web i wyświetlić odpowiednie łącze, aby przełączyć się do widoków Desktop lub Mobile.

    **GetOverridenBrowser** metoda zwraca **HttpBrowserCapabilitiesBase** wystąpienie, które odnosi się do agenta użytkownika aktualnie ustawiona dla żądania (rzeczywistych lub zastąpiona). Tej wartości można użyć do pobrania właściwości, takie jak **IsMobileDevice**.

    ![Widok częściowy ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher widoku częściowego")

    *Widok częściowy ViewSwitcher*
4. Otwórz **ViewSwitcherController.cs** klasa znajduje się w **kontrolerów** folderu. Zapoznaj się z tym SwitchView akcji jest wywoływana przez łącze w składniku ViewSwitcher i zwróć uwagę, nowych metod HttpContext.

    - **HttpContext.ClearOverridenBrowser()** metoda usuwa wszelkich przesłoniętych agentów użytkownika dla bieżącego żądania.
    - **HttpContext.SetOverridenBrowser()** metoda zastępuje żądania rzeczywistą wartość agenta użytkownika przy użyciu określonego agenta użytkownika.  
        ![Kontroler ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher kontrolera")  
*Kontroler ViewSwitcher*

        Przesłanianie przeglądarki jest podstawowych funkcji programu ASP.NET MVC 4, który jest również dostępny nawet wtedy, gdy nie zostanie zainstalowany pakiet jQuery.Mobile.MVC. Jednak ta funkcja dotyczy tylko widok, układ i widoku częściowego, a nie dotyczy żadnych funkcji, które są zależne od obiektu Request.Browser.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Zadanie 5 Dodawanie przełącznikiem widoku w widoku pulpitu.

W tym zadaniu zostanie zaktualizowana układ pulpitu, aby uwzględnić przełącznikiem widoku. Zezwalaj na użytkowników urządzeń przenośnych powrócić do widoku dla urządzeń przenośnych podczas przeglądania widoku pulpitu.

1. Odśwież witrynę w **Emulator Windows Phone**.
2. Kliknij pozycję **widok pulpitu** link u góry galerii. Należy zauważyć, że ma nie przełącznikiem widoku w widoku pulpitu do zezwalania, można wrócić do widoku dla urządzeń przenośnych.
3. Wróć do programu Visual Studio i Otwórz  **\_Layout.cshtml** widoku.
4. Znajdź sekcję logowania i Wstaw wywołanie do renderowania  **\_ViewSwitcher** widoku częściowego poniżej  **\_LogOnPartial** widoku częściowego. Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Naciśnij klawisz **CTRL + S** Aby zapisać zmiany.
6. Odśwież stronę w Emulator Windows Phone, a następnie kliknij dwukrotnie ekranu, aby powiększyć. Należy zauważyć, że na stronie głównej znajdują się teraz **widoku dla urządzeń przenośnych** łącze, które zmienia się od urządzeń przenośnych na widok pulpitu.

    ![Wyświetl przełącznik renderowane w widoku pulpitu](whats-new-in-aspnet-mvc-4/_static/image32.png "przełącznikiem widoku renderowane w widoku pulpitu")

    *Wyświetl przełącznik renderowane w widoku pulpitu*
7. Przełącz do widoku przenośnych ponownie, a następnie przejdź do <strong>o</strong> strony (http://localhost[portu]/Home/About). Należy zauważyć, że nawet, jeśli jeszcze nie utworzono widok About.Mobile.cshtml, na stronie informacje jest wyświetlana przy użyciu układu dla urządzeń przenośnych (\_Layout.Mobile.cshtml).

    ![Informacje o stronie](whats-new-in-aspnet-mvc-4/_static/image33.png "o stronie")

    *Informacje o stronie*
8. Wreszcie Otwórz witrynę w przeglądarce sieci Web pulpitu. Należy zauważyć, że żaden z poprzednich aktualizacji ma wpływ na widok pulpitu.

    ![Widok pulpitu PhotoGallery](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery widok pulpitu")

    *Widok pulpitu PhotoGallery*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Zadanie 6 — Tworzenie nowych trybów wyświetlania

Nowa funkcja trybów wyświetlania umożliwia aplikacji wybierz widoki, w zależności od przeglądarki, która generuje żądanie. Na przykład na stronie głównej na żądanie przeglądarki na komputerze aplikacji zwróci **Views\Home\Index.cshtml** szablonu. Następnie w przeglądarce dla urządzeń przenośnych na żądanie strony głównej aplikacji zwróci **Views\Home\Index.mobile.cshtml** szablonu.

W tym zadaniu utworzysz niestandardowych układów dla urządzeń iPhone, a trzeba będzie symulować żądania z urządzenia iPhone. Aby to zrobić, można użyć albo iPhone emulatorze/symulatorze (takich jak [Electric symulator Mobile](http://www.electricplum.com/)) lub w przeglądarce, za pomocą dodatków, które modyfikują agenta użytkownika. Aby uzyskać instrukcje dotyczące sposobu ustawiania ciąg agenta użytkownika w przeglądarce Safari emulować dla telefonu iPhone, zobacz [jak umożliwić Safari poudawać jest IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) w blogu David Alison.

**Należy zauważyć, że to zadanie jest opcjonalne i można kontynuować bez jej wykonanie w całym środowisku laboratoryjnym.**

1. W programie Visual Studio, naciśnij klawisz **SHIFT** + **F5** Aby zatrzymać debugowanie aplikacji.
2. Otwórz **Global.asax.cs** i dodaj następującą instrukcję using.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Dodaj następujący wyróżniony kod do aplikacji\_Uruchom metodę.

    (Code Snippet — *platformy ASP.NET MVC 4 laboratorium — Ex03 — iPhone DisplayMode*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

Zarejestrowano nowe **DefaultDisplayMode o nazwie &quot;iPhone&quot;**, w ramach statycznej **DisplayModeProvider.Instance.Modes** statycznej listy, który dopasowywane każdego żądania przychodzącego. Jeśli żądanie przychodzące zawiera ciąg &quot;iPhone&quot;, ASP.NET MVC zawiera widoki, których nazwy zawierają &quot;iPhone&quot; sufiks. Parametr 0 wskazuje, jak określone jest nowy tryb; na przykład, ten widok jest bardziej szczegółowe niż ogólne &quot;.mobile&quot; regułę, która pasuje do żądania z urządzeń przenośnych.

Po uruchomieniu tego kodu, gdy przeglądarka iPhone generuje żądanie, aplikacja będzie używać **Views\Shared\\_Layout.iPhone.cshtml** układ utworzysz w następnych krokach.

> [!NOTE]
> Dzięki temu testowania żądanie dla urządzeń iPhone został uproszczony dla celów demonstracyjnych i może nie działać zgodnie z oczekiwaniami dla każdego ciąg agenta użytkownika dla telefonu iPhone, (na przykład test jest uwzględniana wielkość liter).

4. Utwórz kopię  **\_Layout.Mobile.cshtml** w pliku **Views\Shared** folder i zmień jego kopię, &quot; **\_Layout.iPhone.csthml**&quot;.
5. Otwórz  **\_Layout.iPhone.csthml** utworzonego w poprzednim kroku.
6. Znajdź div element z atrybutem roli danych równa **strony** i zmień **data-theme** atrybutu &quot; **a**&quot;.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Masz teraz układy 3 w aplikacji ASP.NET MVC 4:

1. **\_Layout.cshtml**: układ domyślny, używany w przypadku przeglądarek komputerowych.
2. **\_Layout.Mobile.cshtml**: domyślny układ używane dla urządzeń przenośnych.
3. **\_Layout.iPhone.cshtml**: określonego układu dla urządzeń iPhone, przy użyciu innego schematu kolorów do odróżnienia od \_Layout.mobile.cshtml.
7. Naciśnij klawisz **F5** do uruchamiania aplikacji, a następnie przejdź do lokacji w **Emulator Windows Phone**.
8. Otwórz **symulatora telefonu iPhone** (zobacz [dodatku C](#AppendixC) instrukcje na temat instalowania i konfigurowania symulatora telefonu iPhone), a następnie przejdź do witryny za. Należy zauważyć, że każdy telefon przy użyciu określonego szablonu.

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Przy użyciu różnych widoków dla każdego urządzenia przenośnego*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Ćwiczenie 4: Przy użyciu kontrolerów asynchroniczne

Microsoft .NET Framework 4.5 wprowadza nowe funkcje języka C# i Visual Basic do stanowią podstawę nowe asynchronii w programowaniu .NET. Takim fundamencie nowe sprawia, że programowanie asynchroniczne podobne do — i około tak proste, jak - synchroniczne programowania. Jesteś teraz możliwość zapisywania metod asynchronicznych akcji na platformie ASP.NET MVC 4 przy użyciu **AsyncController** klasy. Można użyć metod asynchronicznych akcji dla długotrwałych, innego niż procesor CPU powiązane żądania. Umożliwia to uniknięcie blokowania serwera sieci Web z wykonywania pracy, podczas przetwarzania żądania. Klasa AsyncController jest zwykle używany dla wywołania usługi sieci Web długoterminowych.

To ćwiczenie objaśnia podstawy operację asynchroniczną na platformie ASP.NET MVC 4. Jeśli chcesz bardziej zgłębić temat, możesz zapoznać się z następującym artykułem: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Zadanie 1 — wdrażanie kontrolera asynchronicznego

1. Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex4-Async/rozpoczęcia/** folderu. W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
2. Otwórz **HomeController.cs** klasy z **kontrolerów** folderu.
3. Dodaj następującą instrukcję using.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Aktualizacja **HomeController** dziedziczyć od **AsyncController**. Kontrolery, które wynikają z AsyncController włączyć platformę ASP.NET do przetwarzania żądań asynchronicznych, a wciąż mogą metody synchronicznej działania usługi.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Dodaj **async** słowa kluczowego **indeksu** metody i przypisz ją do zwrotu typu **zadań&lt;ActionResult&gt;**.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > **Async** — słowo kluczowe jest jedną z nowych słów kluczowych, .NET Framework 4.5 zapewnia; go informuje kompilator, że ta metoda zawiera kod asynchroniczny. A **zadań** obiekt reprezentuje operację asynchroniczną, która może zostać zakończone w pewnym momencie w przyszłości.
6. Zastąp **klienta. GetAsync()** wywołań przy użyciu async pełna wersja await — słowo kluczowe, jak pokazano poniżej.

    (Code Snippet — *platformy ASP.NET MVC 4 laboratorium - Ex04 - GetAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > W poprzedniej wersji były używane **wynik** właściwość **zadań** obiekt, aby zablokować wątek, dopóki nie wynik zostanie zwrócony (wersja synchronizacji).
    > 
    > Dodawanie **await** — słowo kluczowe informuje kompilator, aby asynchronicznie poczekaj, aż zadanie zwrócone z wywołania metody. Oznacza to, że reszta kodu zostanie wykonany jako wywołanie zwrotne dopiero po zakończeniu metody oczekiwane. Inną rzeczą, należy zwrócić uwagę jest, że nie trzeba zmienić swoje bloku try / catch, aby umożliwić: wyjątki, które odbywa się w tle lub na pierwszym planie nadal zostanie przechwycony bez konieczności wykonywania dodatkowej pracy przy użyciu programu obsługi dostarczanych przez szablon.
7. Zmień kod, aby kontynuować z implementacji asynchronicznego, zastępując wierszy za pomocą nowego kodu, jak pokazano poniżej

    (Code Snippet — *platformy ASP.NET MVC 4 laboratorium - Ex04 - ReadAsStringAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Uruchom aplikację. Można zauważyć nie zmienią, ale Twój kod nie będzie blokować wątek z puli wątków, dzięki czemu lepsze wykorzystanie zasobów serwera i zwiększanie wydajności.

    > [!NOTE]
    > Dowiedz się więcej na temat nowych funkcji programowania asynchronicznego w środowisku laboratoryjnym &quot; **programowania asynchronicznego w .NET 4.5 w języku C# i Visual Basic** &quot; zawarte w Visual Studio SDK szkolenia.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Zadanie 2 — Obsługa przekroczeń limitu czasu przy użyciu tokenów anulowania

Metody asynchroniczne akcji, które zwracają wystąpień zadań może również obsługiwać błędy przekroczenia limitu czasu. To zadanie zaktualizuje kod metody indeksu do obsługi scenariusza limitu czasu, korzystając z tokena odwołania.

1. Wróć do programu Visual Studio, a następnie naciśnij klawisz **SHIFT + F5** Aby zatrzymać debugowanie.
2. Dodaj następującą instrukcję using **HomeController.cs** pliku.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Akcja indeksu, aby otrzymywać aktualizacji **CancellationToken** argumentu.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Aktualizacja **GetAsync** wywołanie, aby przekazać token anulowania.

    (Code Snippet — *SendAsync laboratorium - Ex04 - platformy ASP.NET MVC 4 przy użyciu CancellationToken*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Dekoracji *indeksu* metody z **Wartość AsyncTimeout** atrybut ustawiony na 500 milisekund i **HandleError** atrybutu, który skonfigurowano do obsługi  **TaskCanceledException** przez przekierowywanie do **przekroczenie limitu czasu** widoku.

    (Code Snippet — *platformy ASP.NET MVC 4 laboratorium — Ex04 — atrybuty*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Otwórz **PhotoController** klasy i zaktualizuj **galerii** metoda opóźnienie wykonania 1000 milisekund, spędzonych (1 sekunda) do symulacji długo działające zadania.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Otwórz **Web.config** pliku i włącz błędy niestandardowe, dodając następujący element.

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Tworzenie nowego widoku w **Views\Shared** o nazwie **przekroczenie limitu czasu** i układ domyślny. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **Views\Shared** i wybierz polecenie **Dodaj | Widok**.

    ![Przy użyciu różnych widoków dla każdego urządzenia przenośnego](whats-new-in-aspnet-mvc-4/_static/image36.png "przy użyciu różnych widoków dla każdego urządzenia przenośnego")

    *Przy użyciu różnych widoków dla każdego urządzenia przenośnego*
9. Aktualizacja **przekroczenie limitu czasu** wyświetlanie zawartości, jak pokazano poniżej.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Uruchom aplikację i przejdź do adresu URL katalogu głównego. Ponieważ dodano **Thread.Sleep** 1000 milisekund, wystąpi błąd limitu czasu, generowane przez **Wartość AsyncTimeout** atrybutu i przechwytywać przez **HandleError** atrybut.

    ![Wyjątek limitu czasu obsługiwane](whats-new-in-aspnet-mvc-4/_static/image37.png "wyjątek limitu czasu obsługi")

    *Wyjątek limitu czasu obsługi*

> [!NOTE]
> Ponadto można wdrożyć tę aplikację do witryny sieci Web systemu Windows Azure następujące [dodatek D: publikowania aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy](#AppendixD).


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym hands-na-laboratorium został zaobserwowany niektóre z nowych funkcji w programie ASP.NET MVC 4. Zostały omówione następujące pojęcia:

- Zawiera ulepszenia do platformy ASP.NET MVC projektu szablonów — w tym szablonie projektu aplikacji mobilnej
- Użyj atrybutu okienka ekranu HTML5 i CSS zapytaniami multimediów w celu wyświetlania na urządzeniach przenośnych
- Użyj jQuery Mobile, do ulepszenia stopniowego oraz do tworzenia zoptymalizowanych pod kątem touch interfejsu użytkownika sieci web
- Utwórz widoki specyficzne dla mobile
- Umożliwia przełączanie się między widokami mobilnych i klasycznych aplikacji składnika o przełącznikiem widoku
- Tworzenie kontrolerów asynchroniczne przy użyciu obsługi zadań

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Dodatek A: korzystania z fragmentów kodu

Za pomocą wstawek kodu masz kod, czego potrzebujesz w zasięgu ręki. Dokument laboratorium informuje o dokładnie podczas korzystania z nich, jak pokazano na poniższej ilustracji.

![Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu](whats-new-in-aspnet-mvc-4/_static/image38.png "fragmenty kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu*

***Aby dodać wstawki kodu za pomocą klawiatury (tylko język C#)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Rozpocznij wpisywanie nazwy fragmentu kodu (bez spacji i łączniki).
3. Obejrzyj, jak technologia IntelliSense wyświetla zgodne z nazwami fragmenty kodu.
4. Wybierz prawidłowy fragment kodu lub pisz dalej do momentu wybrania debugowanej nazwy całego fragmentu kodu.
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

![Rozpocznij wpisywanie nazwy fragmentu kodu](whats-new-in-aspnet-mvc-4/_static/image39.png "Rozpocznij wpisywanie nazwy fragmentu kodu")

*Rozpocznij wpisywanie nazwy fragmentu kodu*

![Naciśnij klawisz Tab, aby zaznaczyć fragment wyróżnione](whats-new-in-aspnet-mvc-4/_static/image40.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragmentu kodu")

*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

![Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte](whats-new-in-aspnet-mvc-4/_static/image41.png "rozwinie ponownie naciśnij klawisz Tab i fragment kodu")

*Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte.*

***Aby dodać wstawki kodu za pomocą myszy (C#, Visual Basic i XML)***

1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.
2. Wybierz **Wstaw fragment kodu** następuje **Moje fragmenty kodu**.
3. Wybierz odpowiedni fragment z listy, klikając go.

![Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu](whats-new-in-aspnet-mvc-4/_static/image42.png "kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu")

*Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu*

![Wybierz odpowiedni fragment z listy, klikając go](whats-new-in-aspnet-mvc-4/_static/image43.png "wybierz odpowiedni fragment z listy, klikając go")

*Wybierz odpowiedni fragment z listy, klikając go*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Dodatek B: Instalowanie programu Visual Studio Express 2012 for Web

Możesz zainstalować **programu Microsoft Visual Studio Express 2012 for Web** lub inne &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe wskazówki ułatwiają kroki wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Zainstaluj program Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "zainstalować program Visual Studio Express")

    *Zainstaluj program Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Akceptowanie umowy licencyjnej*
5. Zaczekaj, aż do zakończenia procesu pobierania i instalacji.

    ![Postęp instalacji](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu, a następnie zacznij pisać &quot; **VS Express**&quot;, następnie kliknij pozycję **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *VS Express for Web kafelka*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Dodatek C: Instalowanie programu WebMatrix 2 i symulatora telefonu iPhone

Działanie witryny oraz na urządzeniu iPhone symulowane można użyć rozszerzenia programu WebMatrix &quot;Electric symulator Mobile dla telefonu iPhone&quot;. Ponadto można skonfigurować to samo rozszerzenie, aby uruchomić symulator z programu Visual Studio 2012.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Zadanie 1 — Instalowanie programu WebMatrix 2

1. Przejdź do [ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>programu WebMatrix 2</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Zainstaluj program WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Zainstaluj program WebMatrix 2")

    *Zainstaluj program WebMatrix 2*
4. Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](whats-new-in-aspnet-mvc-4/_static/image50.png "akceptowanie umowy licencyjnej")

    *Akceptowanie umowy licencyjnej*
5. Zaczekaj, aż do zakończenia procesu pobierania i instalacji.

    ![Postęp instalacji](whats-new-in-aspnet-mvc-4/_static/image51.png "postęp instalacji")

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](whats-new-in-aspnet-mvc-4/_static/image52.png "instalacja została zakończona")

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Zadanie 2 — Instalowanie rozszerzenia symulatora telefonu iPhone

1. Uruchom **WebMatrix** i Otwórz istniejącą witrynę sieci Web, lub Utwórz nową.
2. Kliknij przycisk **Uruchom** przycisk **Home** wstążki, a następnie wybierz pozycję **Dodaj nowe**.

    ![Dodawanie nowego rozszerzenia programu WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "dodanie nowego rozszerzenia programu WebMatrix")

    *Dodawanie nowego rozszerzenia programu WebMatrix*
3. Wybierz **symulatora telefonu iPhone** i kliknij przycisk **zainstalować**.

    ![Przeglądanie rozszerzenia programu WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "rozszerzenia przeglądania programu WebMatrix")

    *Przeglądanie rozszerzenia programu WebMatrix*
4. Szczegóły pakietu kliknij **zainstalować** aby kontynuować instalację rozszerzenia.

    ![rozszerzenie symulatora telefonu iPhone](whats-new-in-aspnet-mvc-4/_static/image55.png "rozszerzenia symulatora telefonu iPhone")

    *rozszerzenie symulatora telefonu iPhone*
5. Przeczytaj i zaakceptuj rozszerzenia umowy licencyjnej.

    ![Rozszerzenia programu WebMatrix Umowa licencyjna EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "rozszerzenia programu WebMatrix umowy licencyjnej")

    *Rozszerzenia programu WebMatrix umowy licencyjnej*
6. Teraz można uruchomić witryny sieci Web z programu WebMatrix za pomocą symulatora opcji dla telefonu iPhone.

    ![Uruchamianie przy użyciu telefonu iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "uruchamiane za pomocą telefonu iPhone")

    *Uruchamianie przy użyciu telefonu iPhone*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Zadanie 3 — Konfigurowanie programu Visual Studio 2012 do uruchamiania symulatora telefonu iPhone

1. Otwórz **programu Visual Studio 2012** i jest otwarcie każdej witryny sieci Web, lub Utwórz nowy projekt.
2. Kliknij strzałkę w dół od przycisk Uruchom, a następnie wybierz pozycję **przeglądania przy użyciu**.

    ![Przeglądanie przy użyciu](whats-new-in-aspnet-mvc-4/_static/image58.png "przeglądania przy użyciu")

    *Przeglądaj w*
3. W &quot;przeglądanie za pomocą&quot; okno dialogowe, kliknij przycisk **Dodaj**.
4. W &quot;Dodaj Program&quot; okno dialogowe, użyj następujących wartości:

   - <strong>Program</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (odpowiednio zaktualizować ścieżkę)</em>
   - **Argumenty**: &quot;1&quot;
   - **Przyjazna nazwa**: symulatora telefonu iPhone

     ![Dodaj program](whats-new-in-aspnet-mvc-4/_static/image59.png "Dodaj program")

     *Dodaj program do przeglądania przy użyciu*
5. Kliknij przycisk **OK** i zamknąć okna dialogowe.
6. Jesteś teraz można uruchamiać aplikacje sieci Web w symulatora telefonu iPhone z programu Visual Studio 2012.

    ![Przeglądaj z symulatora telefonu iPhone](whats-new-in-aspnet-mvc-4/_static/image60.png "przeglądania przy użyciu symulatora telefonu iPhone")

    *Przeglądaj z symulatora telefonu iPhone*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek D: publikowania aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy

Ten dodatek będzie pokazują, jak utworzyć nową witrynę sieci web w portalu zarządzania pakietu Windows Azure i publikowanie aplikacji, uzyskany postępując zgodnie z laboratorium, korzystając z zalet funkcji publikowania narzędzia Web Deploy oferowanego przez system Windows Azure.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Zadanie 1. Tworzenie nowej witryny sieci Web z Windows Azure Portal

1. Przejdź do [portalu zarządzania pakietu Windows Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft, powiązaną z Twoją subskrypcją.

    > [!NOTE]
    > Platforma Windows Azure można bezpłatny hosting 10 witryn sieci Web platformy ASP.NET i skalowanie w miarę wzrostu ruchu. Możesz zarejestrować się [tutaj](http://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do portalu usługi Windows Azure](whats-new-in-aspnet-mvc-4/_static/image61.png "Zaloguj się do portalu usługi Windows Azure")

    *Zaloguj się do portalu zarządzania systemu Azure Windows*
2. Kliknij przycisk **New** na pasku poleceń.

    ![Tworzenie nowej witryny sieci Web](whats-new-in-aspnet-mvc-4/_static/image62.png "tworzenia nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij przycisk **obliczenia** | **witryny sieci Web**. Następnie wybierz pozycję **szybkie tworzenie** opcji. Podaj adres URL dostępny dla nowej witryny sieci web, a następnie kliknij przycisk **Utwórz witrynę sieci Web**.

    > [!NOTE]
    > Witryny sieci Web Windows Azure jest hostem dla aplikacji sieci web działające w chmurze, które można kontrolować i zarządzać nimi. Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web do Windows Azure witryny sieci Web z poza portalem. Nie obejmuje kroki konfigurowania bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu szybkiego tworzenia](whats-new-in-aspnet-mvc-4/_static/image63.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")

    *Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*
4. Poczekaj, aż nowe **witryny sieci Web** zostanie utworzony.
5. Po utworzeniu witryny sieci Web kliknij link w obszarze **adresu URL** kolumny. Sprawdź, czy działa nową witrynę sieci Web.

    ![Przejście do nowej witryny sieci web](whats-new-in-aspnet-mvc-4/_static/image64.png "przejście do nowej witryny sieci web")

    *Przejście do nowej witryny sieci web*

    ![Witryna sieci Web działa](whats-new-in-aspnet-mvc-4/_static/image65.png "witryna sieci Web działa")

    *Witryna sieci Web działa*
6. Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.

    ![Otwieranie strony zarządzania witryny sieci web](whats-new-in-aspnet-mvc-4/_static/image66.png "otwieranie stron zarządzania witryny sieci web")

    *Otwieranie strony zarządzania witryny sieci Web*
7. W **pulpit nawigacyjny** w obszarze **Przegląd** kliknij **Pobierz profil publikowania** łącza.

    > [!NOTE]
    > *Profil publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web w witrynie sieci Web usługi Windows Azure dla poszczególnych metod włączone publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do łączenia się i uwierzytelnianie w odniesieniu do każdego z punktów końcowych, dla których włączono metoda publikacji. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** Obsługa odczytywania profili publikowania do automatyzowania konfiguracji tych programów dla Publikowanie aplikacji sieci web w usłudze Windows Azure websites.

    ![Pobieranie witryny sieci web profil publikowania](whats-new-in-aspnet-mvc-4/_static/image67.png "pobierania witryny sieci web profilu publikowania")

    *Pobieranie witryny sieci Web profilu publikowania*
8. Pobierz plik profilu publikowania w znanej lokalizacji. Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web do Windows Azure Web Sites z programu Visual Studio za pomocą tego pliku.

    ![Zapisywanie pliku profilu publikowania](whats-new-in-aspnet-mvc-4/_static/image68.png "zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z programu SQL Server baz danych, musisz utworzyć serwer bazy danych SQL. Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.

1. Należy serwera usługi SQL Database do przechowywania bazy danych aplikacji. Możesz wyświetlić serwery baz danych SQL z subskrypcji w portalu zarządzania Azure Windows pod **baz danych Sql** | **serwerów** | **serwera Pulpit nawigacyjny**. Jeśli nie masz serwer, który został utworzony, można utworzyć ją przy użyciu **Dodaj** przycisk na pasku poleceń. Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, ponieważ będziesz ich używać w kolejne zadania podrzędne. Nie należy tworzyć bazy danych, ponieważ zostanie on utworzony w późniejszym terminie.

    ![Pulpit nawigacyjny z serwera bazy danych SQL](whats-new-in-aspnet-mvc-4/_static/image69.png "pulpitu nawigacyjnego serwera bazy danych SQL")

    *Pulpit nawigacyjny z serwera bazy danych SQL*
2. W ramach następnego zadania spowoduje przetestowanie połączenia z bazą danych z programu Visual Studio, dlatego trzeba uwzględnić adres IP lokalnego serwera liście **dozwolone adresy IP**. Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżący adres IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) przycisku.

    ![Dodawanie adresu IP klienta](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Dodawanie adresu IP klienta*
3. Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP listy, kliknij pozycję **Zapisz** aby potwierdzić zmiany.

    ![Potwierdź zmiany](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Potwierdź zmiany*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 — Publikowanie aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **Publikuj**.

    ![Publikowanie aplikacji](whats-new-in-aspnet-mvc-4/_static/image73.png "publikowania aplikacji")

    *Publikowanie witryny sieci web*
2. Importuj profil publikowania, zapisaną w pierwszym zadaniem.

    ![Trwa importowanie profilu publikowania](whats-new-in-aspnet-mvc-4/_static/image74.png "importowania profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij przycisk **sprawdzanie poprawności połączenia**. Po zakończeniu sprawdzania kliknij **dalej**.

    > [!NOTE]
    > Sprawdzanie poprawności zostało ukończone, gdy zostanie wyświetlony z zielonym znacznikiem wyboru pojawiają się obok przycisku Waliduj połączenie.

    ![Sprawdzanie poprawności połączenia](whats-new-in-aspnet-mvc-4/_static/image75.png "sprawdzanie poprawności połączenia")

    *Sprawdzanie poprawności połączenia*
4. W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk obok pola tekstowego połączenia bazy danych (czyli **DefaultConnection**).

    ![Konfiguracja narzędzia Web deploy](whats-new-in-aspnet-mvc-4/_static/image76.png "Konfiguracja narzędzia Web deploy")

    *Konfiguracja narzędzia Web deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W **nazwy serwera** wpisz swoją bazę danych SQL server adresu URL przy użyciu *tcp:* prefiks.
   - W **nazwa_użytkownika** wpisz nazwę logowania administratora serwera.
   - W **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nazwę nowej bazy danych, na przykład: *MVC4SampleDB*.

     ![Konfigurowanie parametrów połączenia z docelowym](whats-new-in-aspnet-mvc-4/_static/image77.png "Konfigurowanie parametrów połączenia z docelowym")

     *Konfigurowanie parametrów połączenia z docelowym*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu, aby utworzyć kliknij bazę danych **tak**.

    ![Tworzenie bazy danych](whats-new-in-aspnet-mvc-4/_static/image78.png "Tworzenie parametrów bazy danych")

    *Tworzenie bazy danych*
7. Parametry połączenia, które będą używane do łączenia z bazą danych SQL na platformie Windows Azure jest wyświetlana w polu tekstowym połączenia domyślne. Następnie kliknij przycisk **Dalej**.

    ![Ciąg połączenia wskazujący bazę danych SQL](whats-new-in-aspnet-mvc-4/_static/image79.png "ciąg połączenia wskazujący bazę danych SQL")

    *Ciąg połączenia wskazujący bazę danych SQL*
8. W **Podgląd** kliknij **Publikuj**.

    ![Publikowanie aplikacji sieci web](whats-new-in-aspnet-mvc-4/_static/image80.png "publikowania aplikacji sieci web")

    *Publikowanie aplikacji sieci web*
9. Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.

    ![Aplikacja została opublikowana na platformie Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "aplikacja została opublikowana na platformie Windows Azure")

    *Aplikacja opublikowana na platformie Windows Azure*
