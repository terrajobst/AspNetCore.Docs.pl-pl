---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Tworzenie interfejsów API RESTful za pomocą interfejsu API sieci Web programu ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: W ostatnich latach stało się jasne, czy protokół HTTP nie jest po prostu potrzeby stron HTML. Warto również to zaawansowana platforma do tworzenia interfejsów API sieci Web, za pomocą o kilku...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 88ac5102a1cf14050412abc336e7a8260a9fa80d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363523"
---
<a name="build-restful-apis-with-aspnet-web-api"></a>Tworzenie interfejsów API RESTful za pomocą wzorca ASP.NET Web API
====================
Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)

> W ostatnich latach stało się jasne, czy protokół HTTP nie jest po prostu potrzeby stron HTML. Ponadto jest to zaawansowana platforma do tworzenia interfejsów API sieci Web, przy użyciu aspekty czasowniki (GET, POST i tak dalej) oraz kilka proste pojęcia — takich jak *identyfikatorów URI* i *nagłówki*. Web API platformy ASP.NET to zestaw składników, które upraszczają Programowanie HTTP. Ponieważ jest on oparty na podstawie środowiska uruchomieniowego platformy ASP.NET MVC, interfejs API sieci Web automatycznie obsługuje szczegóły niskiego poziomu transportu HTTP. W tym samym czasie internetowy interfejs API udostępnia naturalnie modelu programowania protokołu HTTP. W rzeczywistości ma jeden cel interfejsu API sieci Web *nie* abstrakcji natychmiast rzeczywistości HTTP. W rezultacie internetowy interfejs API jest elastyczny i łatwy sposób rozszerzyć. W tym laboratorium praktycznego użyje interfejsu API sieci Web do tworzenia prostego interfejsu API REST dla aplikacji menedżera kontaktu. Zostanie też utworzony klienta do korzystania z interfejsu API. Styl architektury REST okazał się efektywny sposób wykorzystać HTTP -, mimo że na pewno nie jest prawidłowa tylko metody HTTP. Skontaktuj się z pomocą Menedżer udostępni zgodne ze specyfikacją REST na wyświetlanie, dodawanie i usuwanie kontaktów, między innymi. W tym laboratorium wymaga podstawową wiedzę na temat protokołu HTTP, REST, a przyjęto założenie, że masz podstawową wiedzę na temat pracy z kodu HTML, JavaScript i jQuery.
> 
> > [!NOTE]
> > Witryny sieci Web ASP.NET jest dedykowany do interfejsu API sieci Web platformy ASP.NET framework na [ https://asp.net/web-api ](https://asp.net/web-api). Ta lokacja będzie w dalszym ciągu zapewnia najnowsze informacje, przykłady i informacje związane z internetowego interfejsu API, dlatego należy sprawdzić ją często czy chcesz delve głębiej do sztukę tworzenia niestandardowych interfejsów API sieci Web dostępne praktycznie dowolne urządzenie lub rozwoju Framework.
> > 
> > ASP.NET Web API, podobne do ASP.NET MVC 4, ma dużą elastyczność w zakresie oddzielenie warstwy usług z kontrolerów, co pozwala korzystać z kilku dostępnych platform wstrzykiwanie zależności dość proste. W witrynie MSDN, który ilustruje sposób używania Ninject do wstrzykiwania zależności w projekcie interfejsu API sieci Web platformy ASP.NET, którą można pobrać go z znajduje się dobrej próbki [tutaj](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).
> 
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web Camp zestaw szkoleniowy, dostępne pod adresem [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym praktyczne laboratorium dowiesz się jak:

- Implementowanie RESTful interfejsu API sieci Web
- Wywoływanie interfejsu API z klienta HTML

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego laboratorium praktycznego niezbędne jest, następujące elementy:

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczyt [dodatek B](#AppendixB) instrukcje dotyczące sposobu jego instalacji).

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

**Instalowanie fragmentów kodu**

Dla wygody większość kodu, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako fragmenty kodu programu Visual Studio. Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.

Jeśli nie jesteś zaznajomiony z fragmentów kodu w usłudze Visual Studio i chcesz dowiedzieć się, jak z nich korzystać, możesz zapoznać się z dodatku z tego dokumentu &quot; [dodatek A: za pomocą wstawek kodu programu](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To ćwiczenie praktyczne obejmuje wykonywanie następujących:

1. [Ćwiczenie 1: Tworzenie tylko do odczytu internetowego interfejsu API](#Exercise1)
2. [Ćwiczenie 2: Tworzenie internetowego interfejsu API odczytu/zapisu](#Exercise2)
3. [Ćwiczenie 3: Korzystanie z interfejsu API sieci Web z klienta HTML](#Exercise3)

> [!NOTE]
> Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązania, należy uzyskać po ukończeniu ćwiczenia. Jeśli potrzebujesz dodatkowej pomocy ćwiczeń opisanych w dalszej, można użyć tego rozwiązania jako wskazówki.


Szacowany czas do ukończenia tego laboratorium: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Ćwiczenie 1: Tworzenie tylko do odczytu internetowego interfejsu API

W tym ćwiczeniu zostanie implementować metody GET tylko do odczytu, skontaktuj się z Menedżera.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Zadanie 1. Tworzenie projektu interfejsu API

W tym zadaniu użyjesz nowe szablony projektów sieci web platformy ASP.NET do tworzenia aplikacji sieci web interfejsu API sieci Web.

1. Uruchom **Visual Studio 2012 Express for Web**, aby to zrobić, przejdź do **Start** i typ **VS Express for Web** naciśnij **Enter**.
2. Z **pliku** menu, wybierz opcję **nowy projekt**. Wybierz **Visual C# | Web** projektu typu w widoku drzewa typ projektu, a następnie wybierz **aplikacji sieci Web programu ASP.NET MVC 4** typ projektu. Ustaw projekt **nazwa** do *ContactManager* i **Nazwa rozwiązania** do *rozpocząć*, następnie kliknij przycisk **OK**.

    ![Tworzenie nowego projektu ASP.NET MVC 4.0 sieci Web aplikacji](build-restful-apis-with-aspnet-web-api/_static/image1.png "tworzenia nowego projektu ASP.NET MVC 4.0 sieci Web aplikacji")

    *Tworzenie nowego projektu ASP.NET MVC 4.0 sieci Web aplikacji*
3. W oknie dialogowym Typ projektu ASP.NET MVC 4, wybierz **interfejsu API sieci Web** typ projektu. Kliknij przycisk **OK**.

    ![Określanie typu projektu interfejsu API sieci Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "określający typ projektu interfejsu API sieci Web")

    *Określanie typu projektu interfejsu API sieci Web*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Zadanie 2 — Tworzenie kontrolerów interfejsu API Contact Manager

W tym zadaniu utworzysz klasy kontrolera, w których będą znajdować się metody interfejsu API.

1. Usuń plik o nazwie **ValuesController.cs** w ramach **kontrolerów** folder z projektu.
2. Kliknij prawym przyciskiem myszy **kontrolerów** folderu w projekcie i wybierz **Dodaj | Kontroler** z menu kontekstowego.

    ![Dodawanie nowego kontrolera do projektu](build-restful-apis-with-aspnet-web-api/_static/image3.png "dodawania nowego kontrolera do projektu")

    *Dodawanie nowego kontrolera do projektu*
3. W **Dodaj kontroler** wyświetlonym oknie dialogowym wybierz **pusty Kontroler interfejsu API** menu szablonu. Nazwa klasy kontrolera **ContactController**. Następnie kliknij przycisk **Dodaj.**

    ![Aby utworzyć nowy kontroler interfejsu API sieci Web za pomocą okna dialogowego Dodawanie kontrolera](build-restful-apis-with-aspnet-web-api/_static/image4.png "do utworzenia nowego kontrolera interfejsu API sieci Web za pomocą okna dialogowego Dodawanie kontrolera")

    *Aby utworzyć nowy kontroler interfejsu API sieci Web za pomocą okna dialogowego Dodawanie kontrolera*
4. Dodaj następujący kod do **ContactController**.

    (Code Snippet — *laboratorium interfejsu API sieci Web - Ex01 - Get, metoda interfejsu API*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Naciśnij klawisz **F5** debugowania aplikacji. Domyślną stronę główną dla projektu interfejsu API sieci Web powinna zostać wyświetlona.

    ![Domyślną stronę główną w aplikacji ASP.NET Web API](build-restful-apis-with-aspnet-web-api/_static/image5.png "domyślną stronę główną w aplikacji interfejsu API sieci Web platformy ASP.NET")

    *Domyślna strona główna aplikacji interfejsu API sieci Web platformy ASP.NET*
6. W oknie programu Internet Explorer, naciśnij klawisz **F12** klawisz, aby otworzyć **narzędzi deweloperskich** okna. Kliknij przycisk **sieci** kartę, a następnie kliknij przycisk **Rozpocznij przechwytywanie** przycisk, aby rozpocząć, Przechwytywanie ruchu sieciowego do okna.

    ![Otwierając karty sieciowej i Inicjowanie sieci przechwytywania](build-restful-apis-with-aspnet-web-api/_static/image6.png "otwierając karty sieciowej i Inicjowanie sieci przechwytywania")

    *Otwierając karty sieciowej i Inicjowanie sieciowej przechwytywania*
7. Dołącz adres URL w pasku adresu przeglądarki z **/api/contact** i naciśnij klawisz enter. W oknie Przechwytywanie sieci są wyświetlane szczegóły przekazywania. Należy zauważyć, że typ MIME odpowiedzi **application/json**. W tym przykładzie pokazano, jak dane JSON są domyślnym formatem danych wyjściowych.

    ![Wyświetlanie danych wyjściowych żądania interfejsu API sieci Web w widoku sieci](build-restful-apis-with-aspnet-web-api/_static/image7.png "wyświetlania danych wyjściowych żądania interfejsu API sieci Web w widoku sieci")

    *Wyświetlanie danych wyjściowych żądania interfejsu API sieci Web w widoku sieci*

    > [!NOTE]
    > Zachowanie domyślne programu Internet Explorer 10 na tym etapie należy zadać, jeśli użytkownik chce zapisać lub otworzyć strumienia wynikające z wywołania interfejsu API sieci Web. Dane wyjściowe będą plik tekstowy zawierający wynik wywołania adresu URL interfejsu API sieci Web JSON. Nie Anuluj okno dialogowe Aby móc obejrzeć zawartość odpowiedzi za pomocą okna narzędzi dla deweloperów.
8. Kliknij przycisk **przejdź do widoku szczegółowym** przycisk, aby wyświetlić więcej szczegółów na temat odpowiedzi to wywołanie interfejsu API.

    ![Przełącz na widok szczegółowy](build-restful-apis-with-aspnet-web-api/_static/image8.png "Przełącz do widoku szczegółów")

    *Przełącz na widok szczegółowy*
9. Kliknij przycisk **treść odpowiedzi** kartę, aby wyświetlić rzeczywiste tekst JSON w odpowiedzi.

    ![Przeglądanie za pomocą pliku JSON tekst danych wyjściowych polecenia w Monitorze sieci](build-restful-apis-with-aspnet-web-api/_static/image9.png "przeglądanie za pomocą pliku JSON tekst danych wyjściowych polecenia w Monitorze sieci")

    *Wyświetlanie tekstu dane wyjściowe JSON w Monitorze sieci*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Zadanie 3. tworzenie modeli skontaktuj się z pomocą i rozszerzyć nazwę Contactcontroller

W tym zadaniu utworzysz klasy kontrolera, w których będą znajdować się metody interfejsu API.

1. Kliknij prawym przyciskiem myszy **modeli** i wybierz polecenie **Dodaj | Klasa...**  z menu kontekstowego.

    ![Dodawanie nowego modelu do aplikacji sieci web](build-restful-apis-with-aspnet-web-api/_static/image10.png "dodanie nowego modelu aplikacji sieci web")

    *Dodawanie nowego modelu do aplikacji sieci web*
2. W **Dodaj nowy element** okno dialogowe, Nazwa nowego pliku **Contact.cs** i kliknij przycisk **Dodaj.**

    ![Tworzenie nowego pliku klasy skontaktuj się z pomocą](build-restful-apis-with-aspnet-web-api/_static/image11.png "Tworzenie nowego pliku klasy kontaktu")

    *Tworzenie nowego pliku klasy kontaktu*
3. Dodaj następujący wyróżniony kod do **skontaktuj się z pomocą** klasy.

    (Code Snippet — *sieci Web interfejsu API laboratorium — Ex01 — skontaktuj się z klasy*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. W **ContactController** klasy, zaznacz wyraz **ciąg** w definicji metody **uzyskać** metody i wpisz wyraz *skontaktuj się z pomocą*. Po słowo jest wpisany, wskaźnik pojawi się na początku słowo **skontaktuj się z pomocą**. Albo naciśnij i przytrzymaj **Ctrl** klucz i naciśnij klawisz kropki (.) lub kliknij ikonę, aby otworzyć okno dialogowe pomocy w edytorze kodu, aby automatycznie wypełnić przy użyciu myszy **przy użyciu** dyrektywy dla modeli przestrzeń nazw.

    ![Przy użyciu technologii Intellisense pomocy dla deklaracji przestrzeni nazw](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Przy użyciu technologii Intellisense pomocy dla deklaracji przestrzeni nazw*
5. Zmodyfikuj kod **uzyskać** metodę, tak że zwraca tablicę, skontaktuj się z modelu wystąpień.

    (Code Snippet — *laboratorium interfejsu API sieci Web — Ex01 — zwraca listę kontaktów*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Naciśnij klawisz **F5** do debugowania aplikacji sieci web w przeglądarce. Aby wyświetlić zmiany wprowadzone dane wyjściowe odpowiedzi interfejsu API, wykonaj następujące czynności.

   1. Po otwarciu przeglądarki naciśnij **F12** Jeśli narzędzia programistyczne nie są jeszcze otwarte.
   2. Kliknij przycisk **sieci** kartę.
   3. Naciśnij klawisz **Rozpocznij przechwytywanie** przycisku.
   4. Dodaj sufiks adresu URL **/api/contact** do adresu URL w pasku adresu i naciśnij klawisz **Enter** klucza.
   5. Naciśnij klawisz **przejdź do widoku szczegółowym** przycisku.
   6. Wybierz **treść odpowiedzi** kartę. Powinien zostać wyświetlony ciąg JSON reprezentujący serializowane postaci tablicy, skontaktuj się z wystąpień.

      ![JSON serializowane dane wyjściowe złożonych wywołania metody interfejsu API sieci Web](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serializowane dane wyjściowe złożonych wywołania metody interfejsu API sieci Web")

      *Dane wyjściowe Zserializowany do ciągu JSON złożonych wywołania metody interfejsu API sieci Web*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Zadanie 4 — wyodrębnianie funkcji do warstwy usług

To zadanie pokażemy, jak można wyodrębnić funkcji do warstwy usług, aby ułatwić deweloperom rozdzielić ich funkcjonalności usług z warstwy kontrolera, co pozwoli na możliwość ponownego wykorzystania usług, które rzeczywiście wykonują pracę.

1. Utwórz nowy folder w katalogu głównym rozwiązania i nadaj mu nazwę **usług**. Aby to zrobić, kliknij prawym przyciskiem myszy **ContactManager** projektu, wybierz opcję **Dodaj** | **nowy Folder**, nadaj jej nazwę *usług*.

    ![Tworzenie folderu usługi](build-restful-apis-with-aspnet-web-api/_static/image14.png "folder Tworzenie usługi")

    *Tworzenie folderu usługi*
2. Kliknij prawym przyciskiem myszy **usług** i wybierz polecenie **Dodaj | Klasa...**  z menu kontekstowego.

    ![Dodawanie nowej klasy do folderu usługi](build-restful-apis-with-aspnet-web-api/_static/image15.png "Dodawanie nowej klasy do folderu usługi")

    *Dodawanie nowej klasy do folderu usługi*
3. Gdy **Dodaj nowy element** zostanie wyświetlone okno dialogowe, nadaj nowej klasie **ContactRepository** i kliknij przycisk **Dodaj**.

    ![Tworzenie pliku klasy w celu uwzględnienia kodu dla warstwy usług skontaktuj się z repozytorium](build-restful-apis-with-aspnet-web-api/_static/image16.png "tworzenia plik klasy w celu uwzględnienia kodu dla warstwy usług skontaktuj się z repozytorium")

    *Tworzenie pliku klasy w celu uwzględnienia kodu dla warstwy usług skontaktuj się z repozytorium*
4. Dodaj instrukcję using dyrektywę **ContactRepository.cs** plik, aby uwzględnić przestrzeń nazw modeli.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. Dodaj następujący wyróżniony kod do **ContactRepository.cs** plik, aby zaimplementować metodę GetAllContacts.

    (Code Snippet — *sieci Web interfejsu API laboratorium — Ex01 — skontaktuj się z repozytorium*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Otwórz **ContactController.cs** plik, jeśli nie jest jeszcze otwarty.
7. Dodaj następującą instrukcję using do sekcji deklaracji przestrzeni nazw w pliku.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. Dodaj następujący wyróżniony kod do **ContactController.cs** klasy, aby dodać pole prywatne reprezentujący wystąpienie repozytorium, pozostała część klasy członkowie mogą wprowadzać użytkowania implementacji usługi.

    (Code Snippet — *laboratorium interfejsu API — Ex01 — skontaktuj się z pomocą kontroler Web*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Zmiana **uzyskać** metodę, tak że sprawia, że korzystanie z usługi skontaktuj się z repozytorium.

    (Code Snippet — *laboratorium interfejsu API sieci Web — Ex01 — zwraca listę kontaktów za pośrednictwem repozytorium*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Umieść punkt przerwania **ContactController**firmy **uzyskać** definicję metody.

   ![Dodawanie punktów przerwania, skontaktuj się z kontrolerem](build-restful-apis-with-aspnet-web-api/_static/image17.png "Dodawanie punktów przerwania, skontaktuj się z kontrolerem")

   *Dodawanie punktów przerwania, skontaktuj się z kontrolerem.*
11. Naciśnij klawisz **F5** do uruchomienia aplikacji.
12. Po otwarciu przeglądarki, naciśnij klawisz **F12** można otworzyć narzędzia dla deweloperów.
13. Kliknij przycisk **sieci** kartę.
14. Kliknij przycisk **Rozpocznij przechwytywanie** przycisku.
15. Dołącz adres URL w pasku adresu z sufiksem **/api/contact** i naciśnij klawisz **Enter** załadować Kontroler interfejsu API.
16. Program Visual Studio 2012 należy przerywaj raz **uzyskać** metoda rozpoczyna wykonywanie.

   ![Przerwanie wewnątrz metody Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "istotne wewnątrz metody Get")

   *Przerwanie wewnątrz metody Get*
17. Naciśnij klawisz **F5** aby kontynuować.
18. Wróć do przeglądarki Internet Explorer Jeśli nie jest już w trybie koncentracji uwagi. Należy pamiętać, okno przechwytywania sieci.

    ![Widok w programie Internet Explorer, wyświetlanie wyników wywołań interfejsu API sieci Web sieci](build-restful-apis-with-aspnet-web-api/_static/image19.png "sieci widok w programie Internet Explorer, wyświetlanie wyników wywołań interfejsu API sieci Web")

    *Widok sieci w programie Internet Explorer, wyświetlanie wyników wywołań interfejsu API sieci Web*
19. Kliknij przycisk **przejdź do widoku szczegółowym** przycisku.
20. Kliknij przycisk **treść odpowiedzi** kartę. Należy pamiętać, dane wyjściowe JSON z wywołania interfejsu API i jak reprezentuje dwa kontakty pobierane przez warstwę usług.

    ![Wyświetlanie danych wyjściowych JSON z interfejsu API sieci Web w oknie narzędzia dla deweloperów](build-restful-apis-with-aspnet-web-api/_static/image20.png "wyświetlania danych wyjściowych JSON z interfejsu API sieci Web w oknie narzędzia dla deweloperów")

    *Wyświetlanie danych wyjściowych JSON z interfejsu API sieci Web w oknie narzędzia dla deweloperów*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Ćwiczenie 2: Tworzenie internetowego interfejsu API odczytu/zapisu

W tym ćwiczeniu zostaną zaimplementowane POST i PUT metody skontaktuj się z pomocą manager ją włączyć za pomocą funkcji edycji danych.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Zadanie 1 — otwierania projektu interfejsu API sieci Web

W tym zadaniu przygotujesz się do zwiększenia projekt internetowy interfejs API utworzony w ćwiczeniu 1, dzięki czemu może akceptować dane wejściowe użytkownika.

1. Uruchom **Visual Studio 2012 Express for Web**, aby to zrobić, przejdź do **Start** i typ **VS Express for Web** naciśnij **Enter**.
2. Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex02-ReadWriteWebAPI/rozpoczęcia/** folderu. W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
3. Otwórz **Services/ContactRepository.cs** pliku.

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Zadanie 2 — Dodawanie funkcji trwałości danych do implementacji skontaktuj się z repozytorium

W tym zadaniu zostanie rozszerzyć klasę ContactRepository projektu składnika Web API utworzony w ćwiczeniu 1 tak, aby go zostaną zachowane, a akceptują dane wejściowe użytkownika i nowe wystąpienia skontaktuj się z pomocą.

1. Dodaj następującą stałą na **ContactRepository** klasy do reprezentowania nazwy nazwę serwera sieci web pamięci podręcznej elementu klucza w dalszej części tego ćwiczenia.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Dodaj Konstruktor do **ContactRepository** zawierający poniższy kod.

    (Code Snippet — *sieci Web interfejsu API laboratorium — Ex02 — skontaktuj się z repozytorium Konstruktor*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Zmodyfikuj kod **GetAllContacts** metody, jak pokazano poniżej.

    (Code Snippet — *laboratorium interfejsu API sieci Web - Ex02 - pobrania wszystkich kontaktów*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > W tym przykładzie jest dla celów demonstracyjnych i będzie używać pamięci podręcznej na serwerze sieci web jako nośnika magazynowania, tak, aby wartości będzie być dostępne dla wielu klientów jednocześnie, a nie za pomocą mechanizmu magazynowania sesji lub okres istnienia magazynu żądania. Jeden wystarczą środki platformy Entity Framework, magazynu XML lub inne różne zamiast pamięci podręcznej na serwerze sieci web.
4. Zaimplementuj nową metodę o nazwie **SaveContact** do **ContactRepository** klasy, które wykonają tę pracę zapisywać kontaktu. **SaveContact** metoda powinna podjąć jeden **skontaktuj się z pomocą** parametrów i zwrotu atrybut typu wartość logiczna. wartość oznaczający powodzenie lub niepowodzenie.

    (Code Snippet — *sieci Web interfejsu API laboratorium — Ex02 — implementacja metody SaveContact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Ćwiczenie 3: Korzystanie z interfejsu API sieci Web z klienta HTML

W tym ćwiczeniu utworzysz klienta HTML do wywołania interfejsu API sieci Web. Ten klient ułatwi wymiany danych z interfejsu API sieci Web przy użyciu języka JavaScript i wyświetli wyniki w przeglądarce sieci web przy użyciu znaczników HTML.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Zadanie 1 — Modyfikowanie widoku indeksu do zapewnienia wyświetlanie kontaktów graficznego interfejsu użytkownika

W tym zadaniu należy zmodyfikować domyślny widok indeksu w aplikacji sieci web do obsługi wymagań wyświetlania listy istniejących kontaktów w przeglądarce HTML.

1. Otwórz **Visual Studio 2012 Express for Web** Jeśli nie jest już otwarty.
2. Otwórz **rozpocząć** rozwiązania znajdujący się w **źródło/Ex03-ConsumingWebAPI/rozpoczęcia/** folderu. W przeciwnym razie będziesz nadal korzystać z **zakończenia** rozwiązania uzyskać, wykonując poprzednim ćwiczeniu.

   1. Jeśli został otwarty dołączonym **rozpocząć** rozwiązania, należy pobrać niektóre brakujące pakiety NuGet przed kontynuowaniem. Aby to zrobić, kliknij przycisk **projektu** menu, a następnie wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okno dialogowe, kliknij przycisk **przywrócić** Aby pobrać brakujące pakiety.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet za pomocą narzędzia NuGet jest, że nie masz do wysłania wszystkie biblioteki w projekcie, zmniejszenie rozmiaru projektu. Za pomocą narzędzi NuGet Power Tools, określając wersji pakietu w pliku Packages.config można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego należy uruchomić następujące kroki, po otwarciu istniejącego rozwiązania, w tym środowisku laboratoryjnym.
3. Otwórz **Index.cshtml** plik znajdujący się w **widoków domowych** folderu.
4. Zastąp kod HTML wewnątrz elementu div z identyfikatorem **treści** tak, aby wyglądał podobnie do poniższego kodu.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. Dodaj następujący kod Javascript w dolnej części pliku do wykonania żądania HTTP do interfejsu API sieci Web.

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. Otwórz **ContactController.cs** plik, jeśli nie jest jeszcze otwarty.
7. Umieść punkt przerwania na **uzyskać** metody **ContactController** klasy.

    ![Wprowadzenie do punktu przerwania na metody Get kontrolera interfejsu API](build-restful-apis-with-aspnet-web-api/_static/image21.png "wprowadzania punkt przerwania na metody Get kontrolera interfejsu API")

    *Wprowadzenie do punktu przerwania na metody Get kontrolera interfejsu API*
8. Naciśnij klawisz **F5** Aby uruchomić projekt. Przeglądarka zostanie załadowany dokumentu HTML.

    > [!NOTE]
    > Upewnij się, że po przejściu do adresu URL katalogu głównego aplikacji.
9. Gdy strona ładuje się i wykonuje kod JavaScript, punkt przerwania zostanie osiągnięty i wykonywanie kodu zostanie wstrzymany w kontrolerze.

    ![Debugowanie do wywołań interfejsu API sieci Web programu VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "debugowania do wywołań interfejsu API sieci Web programu VS Express for Web")

    *Debugowanie do wywołania interfejsu API sieci Web programu Visual Studio 2012 Express for Web*
10. Usuń punkt przerwania, a następnie naciśnij klawisz **F5** lub narzędzi debugowania **Kontynuuj** przycisk, aby kontynuować ładowania widoku w przeglądarce. Po ukończeniu wywołania interfejsu API sieci Web powinna być widoczna kontakty zwrócony z interfejsu API sieci Web, wywołaj wyświetlane jako elementy listy w przeglądarce.

    ![Wyniki wywołania interfejsu API, wyświetlana w przeglądarce jako elementy listy](build-restful-apis-with-aspnet-web-api/_static/image23.png "wyniki wywołania interfejsu API, wyświetlana w przeglądarce jako elementy listy")

    *Wyniki wywołania interfejsu API, wyświetlana w przeglądarce jako elementy listy*
11. Zatrzymaj debugowanie.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Zadanie 2 — Modyfikowanie widoku indeksu, aby zapewnić graficzny interfejs użytkownika pozwalający kontaktów

W ramach tego zadania będziesz zmodyfikuj widok indeksu w aplikacji MVC. Formularz zostanie dodany do strony HTML, który będzie przechwytywać dane wejściowe użytkownika i wysyłać je do interfejsu API sieci Web, aby utworzyć nowy kontakt, a nowa metoda kontrolera interfejsu API sieci Web zostanie utworzone w celu zbierania daty z graficznego interfejsu użytkownika.

1. Otwórz **ContactController.cs** pliku.
2. Dodaj nową metodę do klasy kontrolera, o nazwie **wpis** jak pokazano w poniższym kodzie.

    (Code Snippet — *sieci Web interfejsu API laboratorium — Ex03 — metody Post*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. Otwórz **Index.cshtml** plik w programie Visual Studio, jeśli nie jest jeszcze otwarty.
4. Dodaj poniższy kod HTML do pliku, zaraz po listę nieuporządkowaną, które zostały dodane w poprzednim zadaniu.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. W elemencie skrypt w dolnej części dokumentu Dodaj następujący wyróżniony kod do obsługi zdarzeń kliknięcia przycisku, które będą wysyłania danych do interfejsu API sieci Web przy użyciu wywołania HTTP POST.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. W **ContactController.cs**, umieść punkt przerwania na **wpis** metody.
7. Naciśnij klawisz **F5** do uruchamiania aplikacji w przeglądarce.
8. Po załadowaniu w przeglądarce stronę wpisz nazwę nowego kontaktu i identyfikator i kliknij przycisk **Zapisz** przycisku.

    ![Klient HTML dokument załadowaniu w przeglądarce](build-restful-apis-with-aspnet-web-api/_static/image24.png "dokument klienta HTML załadowaniu w przeglądarce")

    *Dokument HTML klienta załadowaniu w przeglądarce*
9. Gdy podziały okna debugera w **wpis** metody take a przyjrzeć się właściwości **skontaktuj się z pomocą** parametru. Wartość powinna być zgodna dane, które zostały wprowadzone w formularzu.

    ![Skontaktuj się z obiektu są wysyłane do interfejsu API sieci Web z klienta](build-restful-apis-with-aspnet-web-api/_static/image25.png "Contact obiektu są wysyłane do interfejsu API sieci Web z klienta")

    *Skontaktuj się z obiektu są wysyłane do interfejsu API sieci Web z klienta*
10. Krok za pośrednictwem metody w debugerze, dopóki **odpowiedzi** zmiennej została utworzona. W trakcie kontroli w **lokalne** okna debugera, zobaczysz, że wszystkie właściwości ustawione.

   ![Odpowiedzi, po utworzeniu w debugerze](build-restful-apis-with-aspnet-web-api/_static/image26.png "odpowiedzi, po utworzeniu w debugerze")

   *Odpowiedzi, po utworzeniu w debugerze*
11. Jeśli użytkownik naciśnie klawisz **F5** lub kliknij przycisk **Kontynuuj** w debugerze żądanie zostanie ukończone. Kiedy przełączysz się do przeglądarki, dodano nowy kontakt do listy kontaktów przechowywane przez **ContactRepository** implementacji.

   ![Przeglądarka odzwierciedla pomyślne utworzenie nowego wystąpienia skontaktuj się z pomocą](build-restful-apis-with-aspnet-web-api/_static/image27.png "przeglądarki odzwierciedla pomyślne utworzenie nowego wystąpienia kontaktu")

   *Przeglądarka odzwierciedla pomyślne utworzenie nowego wystąpienia kontaktu*

> [!NOTE]
> Ponadto można wdrożyć tę aplikację do platformy Azure następujące [dodatek C: publikowania aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy](#AppendixC).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

Do nowej struktury Web API platformy ASP.NET i implementacji RESTful interfejsów API sieci Web przy użyciu framework została wprowadzona w tym laboratorium. W tym miejscu możesz utworzyć nowe repozytorium, które ułatwia funkcji trwałości danych przy użyciu dowolnej liczby mechanizmów i Podłączanie usługi zamiast prostych podano w przykładzie w tym laboratorium. Internetowy interfejs API obsługuje szereg dodatkowych funkcji, takich jak umożliwianie komunikację od klientów innych niż HTML napisane w dowolnym języku, który obsługuje HTTP i JSON lub XML. Możliwość hostowania interfejsu Web API poza aplikacji sieci web typowe możliwe jest również, jak również jest możliwość tworzenia własnych formatów serializacji.

Witryny sieci Web ASP.NET jest dedykowany do interfejsu API sieci Web platformy ASP.NET framework na [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api). Ta lokacja będzie w dalszym ciągu zapewnia najnowsze informacje, przykłady i informacje związane z internetowego interfejsu API, dlatego należy sprawdzić ją często czy chcesz delve głębiej do sztukę tworzenia niestandardowych interfejsów API sieci Web dostępne praktycznie dowolne urządzenie lub rozwoju Framework.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Dodatek A: korzystania z fragmentów kodu

Za pomocą wstawek kodu masz kod, czego potrzebujesz w zasięgu ręki. Dokument laboratorium informuje o dokładnie podczas korzystania z nich, jak pokazano na poniższej ilustracji.

![Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu](build-restful-apis-with-aspnet-web-api/_static/image28.png "fragmenty kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Aby dodać wstawki kodu za pomocą klawiatury (tylko język C#)

1. Umieść kursor, w którym chcesz wstawić kod.
2. Rozpocznij wpisywanie nazwy fragmentu kodu (bez spacji i łączniki).
3. Obejrzyj, jak technologia IntelliSense wyświetla zgodne z nazwami fragmenty kodu.
4. Wybierz prawidłowy fragment kodu lub pisz dalej do momentu wybrania debugowanej nazwy całego fragmentu kodu.
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

    ![Rozpocznij wpisywanie nazwy fragmentu kodu](build-restful-apis-with-aspnet-web-api/_static/image29.png "Rozpocznij wpisywanie nazwy fragmentu kodu")

    *Rozpocznij wpisywanie nazwy fragmentu kodu*

    ![Naciśnij klawisz Tab, aby zaznaczyć fragment wyróżnione](build-restful-apis-with-aspnet-web-api/_static/image30.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragmentu kodu")

    *Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

    ![Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte](build-restful-apis-with-aspnet-web-api/_static/image31.png "rozwinie ponownie naciśnij klawisz Tab i fragment kodu")

    *Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte.*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>Aby dodać wstawki kodu za pomocą myszy (C#, Visual Basic i XML)

1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.
2. Wybierz **Wstaw fragment kodu** następuje **Moje fragmenty kodu**.
3. Wybierz odpowiedni fragment z listy, klikając go.

    ![Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu](build-restful-apis-with-aspnet-web-api/_static/image32.png "kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu")

    *Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu*

    ![Wybierz odpowiedni fragment z listy, klikając go](build-restful-apis-with-aspnet-web-api/_static/image33.png "wybierz odpowiedni fragment z listy, klikając go")

    *Wybierz odpowiedni fragment z listy, klikając go*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Dodatek B: Instalowanie programu Visual Studio Express 2012 for Web

Możesz zainstalować **programu Microsoft Visual Studio Express 2012 for Web** lub inne &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe wskazówki ułatwiają kroki wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>Visual Studio Express 2012 for Web z zestawem Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Zainstaluj program Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "zainstalować program Visual Studio Express")

    *Zainstaluj program Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Akceptowanie umowy licencyjnej*
5. Zaczekaj, aż do zakończenia procesu pobierania i instalacji.

    ![Postęp instalacji](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu, a następnie zacznij pisać &quot; **VS Express**&quot;, następnie kliknij pozycję **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *VS Express for Web kafelka*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek C: publikowania aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy

Ten dodatek będzie pokazują, jak utworzyć nową witrynę sieci web w witrynie Azure Portal i publikowanie aplikacji, uzyskany postępując zgodnie z laboratorium, korzystając z zalet funkcji publikowania narzędzia Web Deploy udostępnianych przez platformę Azure.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Zadanie 1. Tworzenie nowej witryny sieci Web w witrynie Azure Portal

1. Przejdź do [portalu zarządzania systemu Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń firmy Microsoft, powiązaną z Twoją subskrypcją.

    > [!NOTE]
    > Za pomocą platformy Azure można bezpłatny hosting 10 witryn sieci Web platformy ASP.NET i skalowanie w miarę wzrostu ruchu. Możesz zarejestrować się [tutaj](http://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do portalu usługi Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "Zaloguj się do portalu usługi Windows Azure")

    *Zaloguj się do portalu*
2. Kliknij przycisk **New** na pasku poleceń.

    ![Tworzenie nowej witryny sieci Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "tworzenia nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij przycisk **obliczenia** | **witryny sieci Web**. Następnie wybierz pozycję **szybkie tworzenie** opcji. Podaj adres URL dostępny dla nowej witryny sieci web, a następnie kliknij przycisk **Utwórz witrynę sieci Web**.

    > [!NOTE]
    > Platforma Azure to hosta dla aplikacji sieci web działające w chmurze, które można kontrolować i zarządzać nimi. Opcja szybkie tworzenie umożliwia wdrażanie ukończonej aplikacji sieci web na platformie Azure z poza portalem. Nie obejmuje kroki konfigurowania bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu szybkiego tworzenia](build-restful-apis-with-aspnet-web-api/_static/image41.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")

    *Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*
4. Poczekaj, aż nowe **witryny sieci Web** zostanie utworzony.
5. Po utworzeniu witryny sieci Web kliknij link w obszarze **adresu URL** kolumny. Sprawdź, czy działa nową witrynę sieci Web.

    ![Przejście do nowej witryny sieci web](build-restful-apis-with-aspnet-web-api/_static/image42.png "przejście do nowej witryny sieci web")

    *Przejście do nowej witryny sieci web*

    ![Witryna sieci Web działa](build-restful-apis-with-aspnet-web-api/_static/image43.png "witryna sieci Web działa")

    *Witryna sieci Web działa*
6. Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.

    ![Otwieranie strony zarządzania witryny sieci web](build-restful-apis-with-aspnet-web-api/_static/image44.png "otwieranie stron zarządzania witryny sieci web")

    *Otwieranie strony zarządzania witryny sieci Web*
7. W **pulpit nawigacyjny** w obszarze **Przegląd** kliknij **Pobierz profil publikowania** łącza.

    > [!NOTE]
    > *Profil publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web Azure dla poszczególnych metod włączone publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do łączenia się i uwierzytelnianie w odniesieniu do każdego z punktów końcowych, dla których włączono metoda publikacji. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **Microsoft Visual Studio 2012** Obsługa odczytywania profili publikowania do automatyzowania konfiguracji tych programów dla Publikowanie aplikacji sieci web na platformie Azure.

    ![Pobieranie witryny sieci web profil publikowania](build-restful-apis-with-aspnet-web-api/_static/image45.png "pobierania witryny sieci web profilu publikowania")

    *Pobieranie witryny sieci Web profilu publikowania*
8. Pobierz plik profilu publikowania w znanej lokalizacji. Dodatkowo w tym ćwiczeniu zostanie zobaczysz, jak publikować aplikację sieci web na platformie Azure z programu Visual Studio za pomocą tego pliku.

    ![Zapisywanie pliku profilu publikowania](build-restful-apis-with-aspnet-web-api/_static/image46.png "zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z programu SQL Server baz danych, musisz utworzyć serwer bazy danych SQL. Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.

1. Należy serwera usługi SQL Database do przechowywania bazy danych aplikacji. Możesz wyświetlić serwery baz danych SQL z subskrypcji w portalu zarządzania platformy Azure w **baz danych Sql** | **serwerów** | **pulpitu nawigacyjnego serwera**. Jeśli nie masz serwer, który został utworzony, można utworzyć ją przy użyciu **Dodaj** przycisk na pasku poleceń. Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, ponieważ będziesz ich używać w kolejne zadania podrzędne. Nie należy tworzyć bazy danych, ponieważ zostanie on utworzony w późniejszym terminie.

    ![Pulpit nawigacyjny z serwera bazy danych SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "pulpitu nawigacyjnego serwera bazy danych SQL")

    *Pulpit nawigacyjny z serwera bazy danych SQL*
2. W ramach następnego zadania spowoduje przetestowanie połączenia z bazą danych z programu Visual Studio, dlatego trzeba uwzględnić adres IP lokalnego serwera liście **dozwolone adresy IP**. Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżący adres IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) przycisku.

    ![Dodawanie adresu IP klienta](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *Dodawanie adresu IP klienta*
3. Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP listy, kliknij pozycję **Zapisz** aby potwierdzić zmiany.

    ![Potwierdź zmiany](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Potwierdź zmiany*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 — Publikowanie aplikacji ASP.NET MVC 4 za pomocą narzędzia Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **Publikuj**.

    ![Publikowanie aplikacji](build-restful-apis-with-aspnet-web-api/_static/image51.png "publikowania aplikacji")

    *Publikowanie witryny sieci web*
2. Importuj profil publikowania, zapisaną w pierwszym zadaniem.

    ![Trwa importowanie profilu publikowania](build-restful-apis-with-aspnet-web-api/_static/image52.png "importowania profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij przycisk **sprawdzanie poprawności połączenia**. Po zakończeniu sprawdzania kliknij **dalej**.

    > [!NOTE]
    > Sprawdzanie poprawności zostało ukończone, gdy zostanie wyświetlony z zielonym znacznikiem wyboru pojawiają się obok przycisku Waliduj połączenie.

    ![Sprawdzanie poprawności połączenia](build-restful-apis-with-aspnet-web-api/_static/image53.png "sprawdzanie poprawności połączenia")

    *Sprawdzanie poprawności połączenia*
4. W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk obok pola tekstowego połączenia bazy danych (czyli **DefaultConnection**).

    ![Konfiguracja narzędzia Web deploy](build-restful-apis-with-aspnet-web-api/_static/image54.png "Konfiguracja narzędzia Web deploy")

    *Konfiguracja narzędzia Web deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W **nazwy serwera** wpisz swoją bazę danych SQL server adresu URL przy użyciu *tcp:* prefiks.
   - W **nazwa_użytkownika** wpisz nazwę logowania administratora serwera.
   - W **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nazwę nowej bazy danych, na przykład: *MVC4SampleDB*.

     ![Konfigurowanie parametrów połączenia z docelowym](build-restful-apis-with-aspnet-web-api/_static/image55.png "Konfigurowanie parametrów połączenia z docelowym")

     *Konfigurowanie parametrów połączenia z docelowym*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu, aby utworzyć kliknij bazę danych **tak**.

    ![Tworzenie bazy danych](build-restful-apis-with-aspnet-web-api/_static/image56.png "Tworzenie parametrów bazy danych")

    *Tworzenie bazy danych*
7. Parametry połączenia, które będą używane do łączenia z bazą danych SQL na platformie Windows Azure jest wyświetlana w polu tekstowym połączenia domyślne. Następnie kliknij przycisk **Dalej**.

    ![Ciąg połączenia wskazujący bazę danych SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "ciąg połączenia wskazujący bazę danych SQL")

    *Ciąg połączenia wskazujący bazę danych SQL*
8. W **Podgląd** kliknij **Publikuj**.

    ![Publikowanie aplikacji sieci web](build-restful-apis-with-aspnet-web-api/_static/image58.png "publikowania aplikacji sieci web")

    *Publikowanie aplikacji sieci web*
9. Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.

    ![Aplikacja została opublikowana na platformie Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "aplikacja została opublikowana na platformie Windows Azure")

    *Aplikacja opublikowana na platformie Azure*
