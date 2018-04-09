---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Tworzenie interfejsów API RESTful za pomocą interfejsu API sieci Web programu ASP.NET | Dokumentacja firmy Microsoft
author: rick-anderson
description: W ostatnich latach okazało HTTP jest nie tylko dla obsługująca stron HTML. Istnieje również zaawansowane platformę do tworzenia interfejsów API sieci Web, przy użyciu o kilku...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 320409cd395384a608a07307a56d18105d45de14
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="build-restful-apis-with-aspnet-web-api"></a>Tworzenie interfejsów API RESTful za pomocą interfejsu API sieci Web ASP.NET
====================
Przez [obozów sieci Web Team](https://twitter.com/webcamps)

> W ostatnich latach okazało HTTP jest nie tylko dla obsługująca stron HTML. Jest również zaawansowane platformę do tworzenia interfejsów API sieci Web, przy użyciu kilku poleceń (GET, POST i tak dalej) oraz kilka pojęć proste takich jak *identyfikatorów URI* i *nagłówki*. Interfejs API sieci Web platformy ASP.NET to zestaw składników, które upraszczają Programowanie HTTP. Ponieważ jest on oparty na środowiska uruchomieniowego ASP.NET MVC, interfejsu API sieci Web obsługuje automatycznie szczegółów niskiego poziomu transportu HTTP. W tym samym czasie interfejsu API sieci Web udostępnia naturalnie modelu programowania protokołu HTTP. W rzeczywistości celem jednego interfejsu API sieci Web jest *nie* abstrakcyjnej optymalizacji stanu HTTP. W związku z tym interfejs API sieci Web jest zarówno elastyczne i łatwe do rozszerzenia. W tym laboratorium praktycznego użyjesz interfejsu API sieci Web do tworzenia prostego interfejsu API REST kontaktów Menedżerze aplikacji. Utworzysz także klientowi korzystanie z interfejsu API. Styl architektury REST okazał się efektywny sposób korzystać z protokołu HTTP -, chociaż na pewno nie jest ważny tylko metody HTTP. Powoduje to udostępnienie kontaktów Menedżerze RESTful do wyświetlania, dodawania i usuwania kontaktów, między innymi. W tym laboratorium wymaga podstawową wiedzę na temat protokołu HTTP, REST, przyjęto założenie, że masz podstawową wiedzę na temat pracy z kodu HTML, JavaScript i jQuery.
> 
> > [!NOTE]
> > Witryny sieci Web ASP.NET jest przeznaczona do framework interfejsu API sieci Web platformy ASP.NET w [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api). Ta lokacja będzie dostarczać, najnowsze informacje, przykłady i informacje związane z interfejsu API sieci Web, więc spróbuj on często Jeśli chcesz delve więcej danych w kompozycji tworzenia niestandardowych interfejsów API sieci Web dostępnych praktycznie dowolne urządzenie lub programowanie Framework.
> > 
> > ASP.NET Web API, podobnie jak ASP.NET MVC 4 ma dużą elastyczność w zakresie oddzielanie warstwy usług z kontrolerów, co umożliwia używanie kilku dostępnych platform iniekcji zależności dość proste. W witrynie MSDN, która przedstawia sposób użycia Ninject iniekcji zależności w projekcie interfejsu API sieci Web platformy ASP.NET, który można pobrać z znajduje się przykład dobrej [tutaj](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).
> 
> 
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, dostępne pod adresem [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym laboratorium praktycznego przedstawiono sposób:

- Implementowanie RESTful interfejsu API sieci Web
- Wywołanie interfejsu API z klienta HTML

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Wymagania wstępne

Poniżej jest wymagany do ukończenia tego laboratorium praktycznego:

- [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) lub wyższego poziomu (odczytu [dodatek B](#AppendixB) instrukcje dotyczące sposobu jego instalacji).

<a id="Setup"></a>
### <a name="setup"></a>Konfiguracja

**Instalowanie wstawki kodu**

Dla wygody taki kod, który będzie zarządzany wzdłuż tego laboratorium jest dostępna jako wstawki kodu programu Visual Studio. Aby zainstalować fragmenty kodu, uruchom **.\Source\Setup\CodeSnippets.vsi** pliku.

Jeśli nie masz doświadczenia z wstawki programu Visual Studio i chcesz dowiedzieć się, jak ich używać, można odwołać się do dodatku z tego dokumentu &quot; [wstawki kodu za pomocą programu dodatek A:](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Ćwiczenia

To laboratorium praktycznego obejmuje wykonywanie następujących:

1. [Ćwiczenie 1: Tworzenie tylko do odczytu interfejsu API sieci Web](#Exercise1)
2. [Ćwiczenie 2: Tworzenie składnika Web API odczytu/zapisu](#Exercise2)
3. [Ćwiczenie 3: Korzystanie z interfejsu API sieci Web z klienta HTML](#Exercise3)

> [!NOTE]
> Towarzyszy każdego wykonywania **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po wykonaniu ćwiczeniach. Jeśli potrzebujesz dodatkowej pomocy pracuje nad ćwiczeniami, można użyć tego rozwiązania jako przewodnika.


Szacowany czas trwania tego laboratorium: **60 minut**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Ćwiczenie 1: Tworzenie tylko do odczytu interfejsu API sieci Web

W tym ćwiczeniu zostanie implementuje metody GET trybie tylko do odczytu do kontaktów Menedżerze.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Zadanie 1 — Tworzenie projektu interfejsu API

Nowe szablony projektów sieci web platformy ASP.NET to zadanie umożliwia tworzenie aplikacji sieci web interfejsu API sieci Web.

1. Uruchom **programu Visual Studio 2012 Express for Web**, aby zrobić to przejdź do **Start** i typ **VS Express for Web** naciśnij **Enter**.
2. Z **pliku** menu, wybierz opcję **nowy projekt**. Wybierz **Visual C# | Web** typ z widoku drzewa projektu typ projektu, a następnie wybierz **aplikacji sieci Web programu ASP.NET MVC 4** typu projektu. Ustaw projekt **nazwa** do *ContactManager* i **Nazwa rozwiązania** do *rozpocząć*, następnie kliknij przycisk **OK**.

    ![Tworzenie nowego programu ASP.NET MVC 4.0 projektu sieci Web aplikacji](build-restful-apis-with-aspnet-web-api/_static/image1.png "tworzenie nowych ASP.NET MVC 4.0 projektu sieci Web aplikacji")

    *Tworzenie nowego programu ASP.NET MVC 4.0 projektu sieci Web aplikacji*
3. W oknie dialogowym Typ projektu programu ASP.NET MVC 4, wybierz **interfejsu API sieci Web** typu projektu. Kliknij przycisk **OK**.

    ![Określanie typu projektu interfejsu API sieci Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "określenie typu projektu interfejsu API sieci Web")

    *Określanie typu projektu interfejsu API sieci Web*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Zadanie 2 — Tworzenie kontrolerów interfejsu API kontaktów Menedżerze

W ramach tego zadania spowoduje utworzenie klasy kontrolera, w których będą znajdować się metody interfejsu API.

1. Usuń plik o nazwie **ValuesController.cs** w **kontrolerów** folder z projektu.
2. Kliknij prawym przyciskiem myszy **kontrolerów** folderu projektu i wybierz **Dodaj | Kontroler** z menu kontekstowego.

    ![Dodawanie nowego kontrolera do projektu](build-restful-apis-with-aspnet-web-api/_static/image3.png "dodawania nowego kontrolera do projektu")

    *Dodawanie nowego kontrolera do projektu*
3. W **Dodaj kontroler** okno dialogowe zostanie wyświetlone, wybierz **pusty Kontroler interfejsu API** z menu szablon. Nazwa klasy kontrolera **ContactController**. Następnie kliknij przycisk **Dodaj.**

    ![Tworzenie nowego kontrolera interfejsu API sieci Web przy użyciu okna dialogowego Dodawanie kontrolera](build-restful-apis-with-aspnet-web-api/_static/image4.png "za pomocą okna dialogowego Dodawanie kontrolera do utworzenia nowego kontrolera interfejsu API sieci Web")

    *Tworzenie nowego kontrolera interfejsu API sieci Web przy użyciu okna dialogowego Dodawanie kontrolera*
4. Dodaj następujący kod do **ContactController**.

    (Fragment - kodu *laboratorium interfejsu API sieci Web - Ex01 - Get, metoda interfejsu API*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Naciśnij klawisz **F5** do debugowania aplikacji. Domyślną stronę główną dla projektu interfejsu API sieci Web powinny być wyświetlane.

    ![Domyślna strona główna aplikacji interfejsu API sieci Web platformy ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image5.png "domyślną stronę główną aplikacji interfejsu API sieci Web ASP.NET")

    *Domyślna strona główna aplikacji interfejsu API sieci Web platformy ASP.NET*
6. W oknie programu Internet Explorer, naciśnij klawisz **F12** klawisz, aby otworzyć **Developer Tools** okna. Kliknij przycisk **sieci** , a następnie kliknij pozycję **Start Przechwytywanie** przycisk, aby rozpocząć przechwytywanie ruchu sieciowego do okna.

    ![Otwarcie karty sieciowej i Inicjowanie sieci przechwytywania](build-restful-apis-with-aspnet-web-api/_static/image6.png "otwarcie karty sieciowej i zainicjowaniu sieciowej przechwytywania")

    *Otwarcie karty sieciowej i zainicjowaniu przechwytywania ruchu sieciowego*
7. Dołącz adres URL na pasku adresu przeglądarki z **/api/kontaktu** i naciśnij klawisz enter. W oknie Przechwytywanie sieci są wyświetlane szczegóły transmisji. Należy zauważyć, że typ MIME odpowiedzi **application/json**. Oznacza to, jak domyślny format danych wyjściowych jest JSON.

    ![Wynik żądania interfejsu API sieci Web jest wyświetlany w widoku sieci](build-restful-apis-with-aspnet-web-api/_static/image7.png "wynik żądania interfejsu API sieci Web jest wyświetlany w widoku sieci")

    *Wynik żądania interfejsu API sieci Web jest wyświetlany w widoku sieci*

    > [!NOTE]
    > Internet Explorer 10 domyślne zachowanie będzie w tym momencie do żądania, jeśli użytkownik chce zapisać lub otworzyć strumienia, w wyniku wywołania interfejsu API sieci Web. Dane wyjściowe będą pliku tekstowego zawierającego JSON wynikiem wywołania adres URL interfejsu API sieci Web. Okno dialogowe nie Anuluj aby można było oglądać zawartości odpowiedzi, za pomocą okna narzędzia dla deweloperów.
8. Kliknij przycisk **przejdź do widoku szczegółowe** przycisk, aby zobaczyć bardziej szczegółowe informacje o odpowiedzi to wywołanie interfejsu API.

    ![Przełącz na widok szczegółowy](build-restful-apis-with-aspnet-web-api/_static/image8.png "Przełącz do widoku szczegółów")

    *Przełącz na widok szczegółowy*
9. Kliknij przycisk **treść odpowiedzi** kartę, aby wyświetlić rzeczywiste tekstu JSON w odpowiedzi.

    ![Wyświetlanie kodu JSON output tekstu w Monitorze sieci](build-restful-apis-with-aspnet-web-api/_static/image9.png "wyświetlanie JSON output tekstu w Monitorze sieci")

    *Wyświetlanie tekstu wyjściowego JSON w Monitorze sieci*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Zadanie 3. tworzenie modeli kontaktów i rozszerzyć kontrolera kontaktów

W ramach tego zadania spowoduje utworzenie klasy kontrolera, w których będą znajdować się metody interfejsu API.

1. Kliknij prawym przyciskiem myszy **modele** i wybierz polecenie **Dodaj | Klasy...**  z menu kontekstowego.

    ![Dodawanie nowego modelu aplikacji sieci web](build-restful-apis-with-aspnet-web-api/_static/image10.png "dodanie nowego modelu aplikacji sieci web")

    *Dodawanie nowego modelu aplikacji sieci web*
2. W **Dodaj nowy element** okna dialogowego, nazwę nowego pliku **Contact.cs** i kliknij przycisk **Dodaj.**

    ![Tworzenie nowego pliku klasy skontaktuj się z](build-restful-apis-with-aspnet-web-api/_static/image11.png "Tworzenie nowego pliku klasy kontaktu")

    *Tworzenie nowego pliku klasy kontaktu*
3. Dodaj następujący wyróżniony kod, aby **skontaktuj się z** klasy.

    (Fragment - kodu *sieci Web, skontaktuj się z pomocą klasy interfejsu API laboratorium - Ex01 -*)


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
~~~
4. W **ContactController** klasy, zaznacz słowo **ciąg** w definicji metody **uzyskać** metody i typu wyrazu *skontaktuj się z*. Po słowie jest wpisany, wskaźnik będą wyświetlane na początku słowo **skontaktuj się z**. Albo przytrzymaj **Ctrl** klucza i naciśnij klawisz kropki (.) lub kliknij ikonę, aby otworzyć okno dialogowe Pomoc w edytorze kodu, aby automatycznie wypełnić przy użyciu myszy **przy użyciu** dyrektywy dla modeli przestrzeń nazw.

    ![Przy użyciu Pomocy Intellisense dla deklaracji przestrzeni nazw](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Przy użyciu Pomocy Intellisense dla deklaracji przestrzeni nazw*
5. Zmodyfikuj kod **uzyskać** metodę, tak że zwraca tablicę wystąpień modelu kontaktu.

    (Fragment - kodu *laboratorium interfejsu API sieci Web - Ex01 - zwracanie listy kontaktów*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Naciśnij klawisz **F5** do debugowania aplikacji sieci web w przeglądarce. Aby wyświetlić zmiany wprowadzone w danych wyjściowych odpowiedzi interfejsu API, wykonaj następujące kroki.

   1. Po otwarciu w przeglądarce naciśnij **F12** Jeśli narzędzi deweloperskich nie są jeszcze dostępne.
   2. Kliknij przycisk **sieci** kartę.
   3. Naciśnij klawisz **Start Przechwytywanie** przycisku.
   4. Dodaj sufiks adresu URL **/api/kontaktu** do adresu URL na pasku adresu i naciśnij klawisz **Enter** klucza.
   5. Naciśnij klawisz **przejdź do widoku szczegółowe** przycisku.
   6. Wybierz **treść odpowiedzi** kartę. Powinny pojawić się reprezentujący Zserializowany formę tablicę wystąpień skontaktuj się z ciągu JSON.

      ![Dane wyjściowe złożonych wywołanie metody interfejsu API sieci Web serializacji JSON](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serializowane dane wyjściowe złożonych wywołanie metody interfejsu API sieci Web")

      *Dane wyjściowe JSON serializować złożona wywołanie metody interfejsu API sieci Web*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Zadanie 4 — wyodrębnianie funkcji do warstwy usług

To zadanie będzie pokazują, jak można wyodrębnić funkcji do warstwy usług, aby ułatwić deweloperom oddzielić ich funkcji usługi z warstwy kontrolera, umożliwiając możliwość ponownego wykorzystania usług, które faktycznie pracę.

1. Utwórz nowy folder w katalogu głównym rozwiązania i nadaj jej nazwę **usług**. Aby to zrobić, kliknij prawym przyciskiem myszy **ContactManager** projektu, zaznacz **Dodaj** | **nowy Folder**, nadaj jej nazwę *usług*.

    ![Tworzenie folderu usługi](build-restful-apis-with-aspnet-web-api/_static/image14.png "usługi tworzenia folderu")

    *Tworzenie folderu usługi*
2. Kliknij prawym przyciskiem myszy **usług** i wybierz polecenie **Dodaj | Klasy...**  z menu kontekstowego.

    ![Dodawanie nowej klasy do folderu usługi](build-restful-apis-with-aspnet-web-api/_static/image15.png "Dodawanie nowej klasy do folderu usługi")

    *Dodawanie nowej klasy do folderu usługi*
3. Gdy **Dodaj nowy element** zostanie wyświetlone okno dialogowe nowej klasie nazwę **ContactRepository** i kliknij przycisk **Dodaj**.

    ![Tworzenie pliku klasy zawiera kod dla warstwy usług skontaktuj się z repozytorium](build-restful-apis-with-aspnet-web-api/_static/image16.png "tworzenia pliku klasy zawiera kod dla warstwy usług skontaktuj się z repozytorium")

    *Tworzenie pliku klasy zawiera kod dla warstwy usług skontaktuj się z repozytorium*
4. Dodaj using dyrektywy do **ContactRepository.cs** pliku, aby uwzględnić modeli przestrzeni nazw.


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
~~~
5. Dodaj następujący wyróżniony kod, aby **ContactRepository.cs** plik, aby zaimplementować metodę GetAllContacts.

    (Fragment - kodu *sieci Web interfejsu API laboratorium - Ex01 - kontaktu repozytorium*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Otwórz **ContactController.cs** plik, jeśli nie jest jeszcze otwarty.
7. Dodaj następującą instrukcję using do sekcji deklaracji przestrzeni nazw w pliku.


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
~~~
8. Dodaj następujący wyróżniony kod, aby **ContactController.cs** klasa do dodania pole prywatne reprezentujący wystąpienie repozytorium, dzięki czemu rest klasy członkowie mogą wprowadzać implementacji usługi.

    (Fragment - kodu *kontaktu Kontroler interfejsu API laboratorium - Ex01 - Web*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Zmiany **uzyskać** metodę, tak że sprawia, że korzystanie z usługi kontaktów repozytorium.

    (Fragment - kodu *laboratorium interfejsu API sieci Web - Ex01 - zwracanie listy kontaktów za pośrednictwem repozytorium*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Umieść punkt przerwania **ContactController**w **uzyskać** definicję metody.

   ![Dodawanie punktów przerwania do kontaktów kontrolera](build-restful-apis-with-aspnet-web-api/_static/image17.png "Dodawanie punktów przerwania do kontrolera kontaktów")

   *Dodawanie punktów przerwania do kontrolera kontaktów*
11. Naciśnij klawisz **F5** do uruchomienia aplikacji.
12. Po otwarciu przeglądarki, naciśnij klawisz **F12** można otworzyć narzędzia deweloperskie.
13. Kliknij przycisk **sieci** kartę.
14. Kliknij przycisk **Start Przechwytywanie** przycisku.
15. Dołącz adres URL na pasku adresu z sufiksem **/api/kontaktu** i naciśnij klawisz **Enter** załadować Kontroler interfejsu API.
16. Program Visual Studio 2012 należy przerywaj raz **uzyskać** metody rozpoczyna się wykonanie.

   ![Przerwanie w obrębie metody Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "krytyczne w obrębie metody Get")

   *Podziału wewnątrz metody Get*
17. Naciśnij klawisz **F5** aby kontynuować.
18. Wróć do programu Internet Explorer, jeśli nie jest już fokusu. Należy pamiętać, okno przechwytywania sieci.

    ![Sieci widoku w programie Internet Explorer wyświetlanie wyników wywołania interfejsu API sieci Web](build-restful-apis-with-aspnet-web-api/_static/image19.png "sieci widoku w programie Internet Explorer wyświetlanie wyników wywołania interfejsu API sieci Web")

    *Widok sieci w programie Internet Explorer wyświetlanie wyników wywołania interfejsu API sieci Web*
19. Kliknij przycisk **przejdź do widoku szczegółowe** przycisku.
20. Kliknij przycisk **treść odpowiedzi** kartę. Należy pamiętać, dane wyjściowe JSON wywołania interfejsu API i jak reprezentuje dwa kontakty pobierane przez warstwę usług.

    ![Wyświetlanie danych wyjściowych JSON z interfejsu API sieci Web w oknie narzędzia developer](build-restful-apis-with-aspnet-web-api/_static/image20.png "wyświetlania danych wyjściowych JSON z interfejsu API sieci Web w oknie narzędzia Projektant")

    *Wyświetlanie danych wyjściowych JSON z interfejsu API sieci Web w oknie narzędzia Projektant*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Ćwiczenie 2: Tworzenie składnika Web API odczytu/zapisu

W tym ćwiczeniu będzie implementowany POST i PUT metody kontaktu Menedżera ją włączyć funkcje edycji danych.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Zadanie 1 - otwierania projektu interfejsu API sieci Web

W tym zadaniu przygotujesz się do zwiększenia projektu interfejsu API sieci Web utworzony w ćwiczenie 1, aby umożliwić akceptowanie danych wejściowych użytkownika.

1. Uruchom **programu Visual Studio 2012 Express for Web**, aby zrobić to przejdź do **Start** i typ **VS Express for Web** naciśnij **Enter**.
2. Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex02-ReadWriteWebAPI/Begin/** folderu. W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.

   1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
3. Otwórz **Services/ContactRepository.cs** pliku.

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Zadanie 2 — Dodawanie funkcji trwałości danych do implementacji repozytorium kontaktów

W tym zadaniu zostanie rozszerzyć klasy ContactRepository projektu interfejsu API sieci Web utworzone w 1 wykonywania tak, aby można utrwalić i akceptuje dane wejściowe użytkownika i nowe wystąpienia kontaktu.

1. Dodaj następujące stała się **ContactRepository** klasy do reprezentowania nazwy sieci web serwera pamięci podręcznej elementu nazwę klucza później w tym ćwiczeniu.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Dodaj Konstruktor do **ContactRepository** zawierający następujący kod.

    (Fragment - kodu *sieci Web interfejsu API laboratorium - Ex02 - kontaktu repozytorium konstruktora*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Zmodyfikuj kod **GetAllContacts** — metoda, jak pokazano poniżej.

    (Fragment - kodu *laboratorium interfejsu API sieci Web - Ex02 - pobrania wszystkich kontaktów*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > W tym przykładzie jest dla celów demonstracyjnych i użyje pamięci podręcznej serwera sieci web jako nośniku, dzięki czemu wartości będzie być dostępne dla wielu klientów jednocześnie, a nie za pomocą mechanizmu magazynowania sesji lub istnienia magazynu żądania. Co można użyć programu Entity Framework, magazynu XML lub innych różnych zamiast pamięci podręcznej serwera sieci web.
4. Zaimplementuj nową metodę o nazwie **SaveContact** do **ContactRepository** klasy pracy zapisywania kontaktu. **SaveContact** metoda powinna przyjmować jeden **skontaktuj się z** parametr i zwracany Boolean wartość wskazuje powodzenie lub niepowodzenie.

    (Fragment - kodu *sieci Web interfejsu API laboratorium - Ex02 - implementacja metody SaveContact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Ćwiczenie 3: Korzystanie z interfejsu API sieci Web z klienta HTML

W tym ćwiczeniu utworzysz klienta HTML do wywołania interfejsu API sieci Web. Ten klient ułatwi wymiana danych z interfejsu API sieci Web przy użyciu języka JavaScript i wyświetli wyniki w przeglądarce sieci web przy użyciu znaczników HTML.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Zadanie 1 - zmodyfikowanie widoku indeksu zapewnienie graficznego interfejsu użytkownika do wyświetlania kontaktów

To zadanie należy zmodyfikować domyślny widok indeksu aplikacji sieci web do obsługi wymagań wyświetlania listy istniejących w przeglądarce HTML.

1. Otwórz **programu Visual Studio 2012 Express for Web** Jeśli nie jest już otwarty.
2. Otwórz **rozpocząć** rozwiązania, znajdujących się na **źródło/Ex03-ConsumingWebAPI/Begin/** folderu. W przeciwnym razie możesz nadal korzystać **zakończenia** uzyskane rozwiązanie, wykonując poprzednim ćwiczeniu.

   1. Po otwarciu dostarczonych **rozpocząć** rozwiązania, musisz pobrać niektórych brakujących pakietów NuGet aby kontynuować. Aby to zrobić, kliknij przycisk **projektu** menu i wybierz **Zarządzaj pakietami NuGet**.
   2. W **Zarządzaj pakietami NuGet** okna dialogowego, kliknij przycisk **przywrócić** celu pobieranie brakujących pakietów.
   3. Na koniec Skompiluj rozwiązanie, klikając **kompilacji** | **Kompiluj rozwiązanie**.

      > [!NOTE]
      > Jedną z zalet przy użyciu narzędzia NuGet jest, że nie masz do wysłania wszystkich bibliotek w projekcie, zmniejszenie jego rozmiar projektu. Narzędzia Power NuGet określając wersje pakietów w pliku Packages.config, będzie można pobrać wymaganych bibliotek podczas pierwszego uruchomienia projektu. Jest to, dlaczego konieczne będzie wykonanie tych kroków, po otwarciu istniejącego rozwiązania z tego laboratorium.
3. Otwórz **Index.cshtml** znajdującym się w **widoków domowych** folderu.
4. Zastąp kod HTML w elemencie div o identyfikatorze **treści** tak, aby wygląda podobnie do następującego kodu.


~~~
[!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
~~~
5. Dodaj następujący kod Javascript w dolnej części pliku do wykonania żądania HTTP do interfejsu API sieci Web.


~~~
[!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
~~~
6. Otwórz **ContactController.cs** plik, jeśli nie jest jeszcze otwarty.
7. Umieść punkt przerwania na **uzyskać** metody **ContactController** klasy.

    ![Umieszczenie punkt przerwania w metodzie Get kontrolera interfejsu API](build-restful-apis-with-aspnet-web-api/_static/image21.png "umieszczenie punkt przerwania w metodzie Get kontrolera interfejsu API")

    *Umieszczenie punkt przerwania w metodzie Get kontrolera interfejsu API*
8. Naciśnij klawisz **F5** uruchomić projekt. Przeglądarka pobierze dokumentu HTML.

    > [!NOTE]
    > Upewnij się, że przeglądania do głównego adresu URL aplikacji.
9. Po pobraniu strony i wykonuje kod JavaScript, punkt przerwania zostanie uruchomiona i wykonywanie kodu zostanie wstrzymane w kontrolerze.

    ![Debugowanie w wywołaniach interfejsu API sieci Web programu VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "debugowania do wywołania interfejsu API sieci Web programu VS Express for Web")

    *Debugowanie w wywołaniu interfejsu API sieci Web programu Visual Studio 2012 Express for Web*
10. Usuń punkt przerwania i naciśnij klawisz **F5** lub narzędzi debugowania **Kontynuuj** przycisk, aby kontynuować ładowanie widoku w przeglądarce. Po zakończeniu wywołania interfejsu API sieci Web powinna zostać wyświetlona kontaktów zwrócony z interfejsu API sieci Web wywołać wyświetlane jako elementy listy w przeglądarce.

    ![Wyniki wywołania interfejsu API w przeglądarce było wyświetlane jako elementy listy](build-restful-apis-with-aspnet-web-api/_static/image23.png "wyników wywołania interfejsu API w przeglądarce było wyświetlane jako elementy listy")

    *Wyniki wywołania interfejsu API w przeglądarce było wyświetlane jako elementy listy*
11. Zatrzymaj debugowanie.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Zadanie 2 - zmodyfikowanie widoku indeksu zapewnienie graficzny interfejs użytkownika służący do tworzenia kontaktów

To zadanie będzie do modyfikowania widoku indeksu aplikacji MVC. Formularz zostanie dodany do strony HTML, który będzie przechwytywanie danych wejściowych użytkownika, a następnie wysłać je do interfejsu API sieci Web, aby utworzyć nowy kontakt i nowa metoda kontrolera interfejsu API sieci Web zostanie utworzone w celu zbierania daty z graficznego interfejsu użytkownika.

1. Otwórz **ContactController.cs** pliku.
2. Dodaj nową metodę do klasy kontrolera o nazwie **Post** jak pokazano w poniższym kodzie.

    (Fragment - kodu *sieci Web interfejsu API laboratorium - Ex03 - metody Post*)


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
~~~
3. Otwórz **Index.cshtml** plik w programie Visual Studio, jeśli nie jest jeszcze otwarty.
4. Dodaj poniższy kod HTML do pliku zaraz po nieuporządkowaną listę, dodanym w poprzednim zadaniu.


~~~
[!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
~~~
5. W elemencie skryptu w dolnej części dokumentu Dodaj następujący kod wyróżnione do obsługi zdarzeń kliknięcia przycisków, których zostanie wysłany danych do interfejsu API sieci Web przy użyciu wywołania protokołu HTTP POST.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. W **ContactController.cs**, umieść punkt przerwania na **Post** metody.
7. Naciśnij klawisz **F5** Aby uruchomić aplikację w przeglądarce.
8. Po załadowaniu w przeglądarce stronę wpisz nową nazwę kontaktu i identyfikator i kliknij przycisk **zapisać** przycisku.

    ![Klient HTML dokument załadowaniu w przeglądarce](build-restful-apis-with-aspnet-web-api/_static/image24.png "dokument klienta HTML załadowaniu w przeglądarce")

    *Dokument HTML klienta załadowaniu w przeglądarce*
9. Gdy Dzielenie okna debugera w **Post** metody, podejmij Zobacz we właściwościach **skontaktuj się z** parametru. Wartości powinna być zgodna wprowadzonych w formularzu.

    ![Skontaktuj się z obiektu są wysyłane do interfejsu API sieci Web z klienta](build-restful-apis-with-aspnet-web-api/_static/image25.png "obiektu kontaktu są wysyłane do interfejsu API sieci Web z klienta")

    *Skontaktuj się z obiektu są wysyłane do interfejsu API sieci Web z klienta*
10. Krok za pomocą metody w debugerze do **odpowiedzi** zmiennej został utworzony. W trakcie kontroli w **zmiennych lokalnych** okna w debugerze, zobaczysz, że wszystkie właściwości ustawione.

   ![Po utworzeniu w debugerze odpowiedzi](build-restful-apis-with-aspnet-web-api/_static/image26.png "odpowiedzi po utworzeniu w debugerze")

   *Odpowiedź po utworzeniu w debugerze*
11. Jeśli naciśniesz **F5** lub kliknij przycisk **Kontynuuj** w debugerze żądanie zostanie ukończone. Po przełączeniu do przeglądarki, dodano nowy kontakt do listy kontaktów przechowywanych przez **ContactRepository** implementacji.

   ![Przeglądarka odzwierciedla pomyślne utworzenie nowego wystąpienia skontaktuj się z pomocą](build-restful-apis-with-aspnet-web-api/_static/image27.png "przeglądarki odzwierciedla pomyślnym utworzeniu nowego wystąpienia kontaktów")

   *Przeglądarka odzwierciedla pomyślnym utworzeniu nowego wystąpienia kontaktów*

> [!NOTE]
> Ponadto można wdrożyć tę aplikację Azure następujących [dodatek C: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy](#AppendixC).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

To laboratorium ma wprowadzone do nowej struktury ASP.NET Web API i wykonania RESTful interfejsów API sieci Web za pomocą środowiska. W tym miejscu możesz utworzyć nowe repozytorium, który ułatwia trwałości danych przy użyciu dowolnej liczby mechanizmów i okablować usługi zapasową zamiast istnieje tylko jedna użyte jako przykłady w tym laboratorium. Interfejs API sieci Web obsługuje szereg dodatkowych funkcji, takich jak umożliwiające komunikację z klientami z systemem innym niż HTML zapisane w dowolnym języku obsługującego protokół HTTP i JSON lub XML. Może obsługiwać interfejs API sieci Web poza aplikacją sieci web typowe możliwe jest również, jak również jest możliwość tworzenia własnych formatów serializacji.

Witryny sieci Web ASP.NET jest przeznaczona do framework interfejsu API sieci Web platformy ASP.NET w [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api). Ta lokacja będzie dostarczać, najnowsze informacje, przykłady i informacje związane z interfejsu API sieci Web, więc spróbuj on często Jeśli chcesz delve więcej danych w kompozycji tworzenia niestandardowych interfejsów API sieci Web dostępnych praktycznie dowolne urządzenie lub programowanie Framework.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Dodatek A: korzystania z wstawek kodu

Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki. Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.

![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](build-restful-apis-with-aspnet-web-api/_static/image28.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Aby dodać fragment kodu za pomocą klawiatury (C# tylko)

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragment (bez spacji i łączniki).
3. Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.
4. Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

    ![Rozpocznij wpisywanie nazwy fragment](build-restful-apis-with-aspnet-web-api/_static/image29.png "zacznij wpisywać nazwę wstawki programu")

    *Rozpocznij wpisywanie nazwy fragment kodu*

    ![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](build-restful-apis-with-aspnet-web-api/_static/image30.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")

    *Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

    ![Ponownie naciśnij klawisz Tab i fragment rozwinie](build-restful-apis-with-aspnet-web-api/_static/image31.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")

    *Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)

1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.
2. Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.
3. Wybierz odpowiedni fragment z listy, klikając go.

    ![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](build-restful-apis-with-aspnet-web-api/_static/image32.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")

    *Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*

    ![Wybierz odpowiedni fragment z listy, klikając go](build-restful-apis-with-aspnet-web-api/_static/image33.png "wybierz odpowiedni fragment z listy, klikając go")

    *Wybierz odpowiedni fragment z listy, klikając go*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Dodatek B: Instalowanie programu Visual Studio Express 2012 for Web

Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; <em>programu Visual Studio Express 2012 for Web z zestawem Azure SDK</em>&quot;.
2. Polecenie **teraz zainstalować**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Instalowanie programu Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "instalacji programu Visual Studio Express")

    *Instalowanie programu Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Akceptowanie umowy licencyjnej*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *VS Express for Web kafelka*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Dodatek C: publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy

Ten dodatek opisano sposób tworzenia nowej witryny sieci web z portalu Azure i opublikować aplikację, uzyskane wykonując laboratorium, korzystając z funkcji publikowania narzędzia Web Deploy dostarczany przez platformę Azure.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Zadanie 1 — Tworzenie nowej witryny sieci Web w portalu Azure

1. Przejdź do [portalu zarządzania Azure](https://manage.windowsazure.com/) i zaloguj się przy użyciu poświadczeń Microsoft skojarzonych z Twoją subskrypcją.

    > [!NOTE]
    > Przy użyciu platformy Azure można udostępniać 10 witryn sieci Web platformy ASP.NET bezpłatnie i następnie Skaluj w miarę zwiększania się ruchu. Możesz utworzyć konto [tutaj](http://aka.ms/aspnet-hol-azure).

    ![Zaloguj się do portalu Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "Zaloguj się do portalu Windows Azure")

    *Zaloguj się do portalu*
2. Kliknij przycisk **nowy** na pasku poleceń.

    ![Tworzenie nowej witryny sieci Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "tworzenia nowej witryny sieci Web")

    *Tworzenie nowej witryny sieci Web*
3. Kliknij przycisk **obliczeniowe** | **witryny sieci Web**. Następnie wybierz **szybkie tworzenie** opcji. Podaj dostępny adres URL dla nowej witryny sieci web, a następnie kliknij przycisk **tworzenie witryny sieci Web**.

    > [!NOTE]
    > Azure jest hostem dla aplikacji sieci web w chmurze, które można kontrolować i zarządzanie nimi. Opcja szybkie tworzenie umożliwia wdrażanie aplikacji sieci web ukończone do platformy Azure z spoza portalu. Nie obejmuje kroki konfigurowania bazy danych.

    ![Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie](build-restful-apis-with-aspnet-web-api/_static/image41.png "tworzenia nowej witryny sieci Web przy użyciu szybkie tworzenie")

    *Tworzenie nowej witryny sieci Web przy użyciu szybkie tworzenie*
4. Poczekaj na nowe **witryny sieci Web** jest tworzony.
5. Po utworzeniu witryny sieci Web kliknij łącze w obszarze **adres URL** kolumny. Sprawdź, czy działa nowej witryny sieci Web.

    ![Przeglądanie do nowej witryny sieci web](build-restful-apis-with-aspnet-web-api/_static/image42.png "przeglądanie do nowej witryny sieci web")

    *Przeglądanie do nowej witryny sieci web*

    ![Witryna sieci Web działa](build-restful-apis-with-aspnet-web-api/_static/image43.png "uruchamiania witryny sieci Web")

    *Witryna sieci Web uruchomiona*
6. Wróć do portalu i kliknij nazwę witryny sieci web w obszarze **nazwa** kolumny do wyświetlenia strony zarządzania.

    ![Otwieranie stron witryny sieci web zarządzania](build-restful-apis-with-aspnet-web-api/_static/image44.png "otwieranie stron zarządzania witryny sieci web")

    *Otwieranie stron zarządzania witryny sieci Web*
7. W **pulpitu nawigacyjnego** w obszarze **szybkiego dostępu** kliknij **pobieranie profilu publikowania** łącza.

    > [!NOTE]
    > *Profilu publikowania* zawiera wszystkie informacje wymagane do publikowania aplikacji sieci web Azure dla każdej metody włączone publikacji. Profil publikowania zawiera adresy URL, poświadczenia użytkownika i parametry bazy danych wymagane do nawiązania połączenia i uwierzytelniania dla każdego z punktów końcowych, dla których włączono metoda publikacji. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** i **programu Microsoft Visual Studio 2012** obsługują odczytywanie publikowanie profile do zautomatyzowania te programy Publikowanie aplikacji sieci web na platformie Azure.

    ![Pobieranie witryny sieci web profilu publikowania](build-restful-apis-with-aspnet-web-api/_static/image45.png "pobierania witryny sieci web profilu publikowania")

    *Pobieranie witryny sieci Web profilu publikowania*
8. Pobierz profil publikowania w znanej lokalizacji. Dodatkowo w tym ćwiczeniu zobaczysz jak opublikować aplikację sieci web na platformie Azure w programie Visual Studio przy użyciu tego pliku.

    ![Zapisywanie pliku profilu publikowania](build-restful-apis-with-aspnet-web-api/_static/image46.png "zapisywanie profilu publikowania")

    *Zapisywanie pliku profilu publikowania*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Zadanie 2 — Konfigurowanie serwera bazy danych

Jeśli aplikacja korzysta z programu SQL Server baz danych, należy utworzyć serwer bazy danych SQL. Jeśli chcesz wdrożyć prostą aplikację, która nie korzysta z programu SQL Server może pominąć to zadanie.

1. Będzie potrzebny serwer bazy danych SQL do przechowywania bazy danych aplikacji. Można wyświetlić serwery bazy danych SQL z subskrypcji usługi Azure Management Portal pod adresem **baz danych Sql** | **serwerów** | **pulpitu nawigacyjnego serwera**. Jeśli nie masz serwer, który został utworzony, można utworzyć przy użyciu jednego **Dodaj** przycisk paska poleceń. Zwróć uwagę na **nazwę serwera i adres URL, nazwę logowania administratora i hasła**, jak będą używane w następnego zadania. Nie należy tworzyć bazy danych jeszcze, jako zostaną utworzone w późniejszym terminie.

    ![Pulpit nawigacyjny serwera bazy danych SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "pulpitu nawigacyjnego serwera bazy danych SQL")

    *Pulpit nawigacyjny serwera bazy danych SQL*
2. W następnym zadaniem Testuj połączenie z bazą danych z programu Visual Studio z tego powodu należy uwzględnić lokalny adres IP serwera liście **dozwolone adresy IP**. Aby to zrobić, kliknij przycisk **Konfiguruj**, wybierz adres IP z **bieżącego adresu IP klienta** i wklej go na **początkowy adres IP** i **końcowy adres IP** pola tekstowe i kliknij przycisk ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) przycisku.

    ![Dodawanie adresu IP klienta](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *Dodawanie adresu IP klienta*
3. Raz **adres IP klienta** jest dodawany do dozwolonych adresów IP kliknij na **zapisać** o potwierdzenie zmian.

    ![Potwierdzenie zmian](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Potwierdzenie zmian*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Zadanie 3 - publikowania aplikacji ASP.NET MVC 4 przy użyciu narzędzia Web Deploy

1. Wróć do rozwiązania ASP.NET MVC 4. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt witryny sieci web i wybierz **publikowania**.

    ![Publikowanie aplikacji](build-restful-apis-with-aspnet-web-api/_static/image51.png "publikowania aplikacji")

    *Publikowanie witryny sieci web*
2. Zaimportuj profil publikowania, zapisana w pierwszym zadaniu.

    ![Importowanie profilu publikowania](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importowanie profilu publikowania")

    *Importowanie profilu publikowania*
3. Kliknij przycisk **Weryfikacja połączenia z**. Po zakończeniu sprawdzania kliknij **dalej**.

    > [!NOTE]
    > Zakończeniu sprawdzania poprawności, gdy zostanie wyświetlony zielony znacznik wyboru są wyświetlane obok przycisku sprawdzania poprawności połączenia.

    ![Sprawdzanie poprawności połączenia](build-restful-apis-with-aspnet-web-api/_static/image53.png "sprawdzanie poprawności połączenia")

    *Sprawdzanie poprawności połączenia*
4. W **ustawienia** w obszarze **baz danych** sekcji, kliknij przycisk Dalej, aby textbox połączenia bazy danych (tj. **połączenia DefaultConnection**).

    ![Konfiguracja narzędzia Web deploy](build-restful-apis-with-aspnet-web-api/_static/image54.png "Konfiguracja narzędzia Web deploy")

    *Konfiguracja narzędzia Web deploy*
5. Skonfiguruj połączenie z bazą danych w następujący sposób:

   - W **nazwy serwera** wpisz swoją bazą danych SQL server adresu URL przy użyciu *tcp:* prefiks.
   - W **nazwy użytkownika** wpisz nazwę logowania administratora serwera.
   - W **hasło** wpisz hasło logowania administratora serwera.
   - Wpisz nazwę nowej bazy danych, na przykład: *MVC4SampleDB*.

     ![Konfigurowanie parametrów połączenia z lokalizacją docelową](build-restful-apis-with-aspnet-web-api/_static/image55.png "Konfigurowanie parametrów połączenia z lokalizacją docelową")

     *Konfigurowanie parametrów połączenia z lokalizacją docelową*
6. Następnie kliknij przycisk **OK**. Po wyświetleniu monitu można utworzyć bazy danych kliknij **tak**.

    ![Tworzenie bazy danych](build-restful-apis-with-aspnet-web-api/_static/image56.png "tworzenie parametry bazy danych")

    *Tworzenie bazy danych*
7. Ciągu połączenia używanego do łączenia z bazą danych SQL w systemie Windows Azure jest wyświetlany w pole tekstowe domyślne połączenie. Następnie kliknij przycisk **Dalej**.

    ![Parametry połączenia wskazujące bazę danych SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "ciąg połączenia wskazujące bazę danych SQL")

    *Parametry połączenia wskazujące bazę danych SQL*
8. W **Podgląd** kliknij przycisk **publikowania**.

    ![Publikowanie aplikacji sieci web](build-restful-apis-with-aspnet-web-api/_static/image58.png "publikowania aplikacji sieci web")

    *Publikowanie aplikacji sieci web*
9. Po zakończeniu procesu publikowania domyślnej przeglądarce otworzy opublikowanej witryny sieci web.

    ![Aplikacja opublikowana w systemie Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "aplikacji publikowanych w systemie Windows Azure")

    *Aplikacja opublikowana na platformie Azure*
