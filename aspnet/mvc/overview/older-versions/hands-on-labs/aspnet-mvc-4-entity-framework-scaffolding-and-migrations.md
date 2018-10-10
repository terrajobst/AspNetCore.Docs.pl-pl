---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: Platforma ASP.NET MVC 4 Entity Framework szkieletu i migracje | Dokumentacja firmy Microsoft
author: rick-anderson
description: Jeśli dopiero zaczynasz z metodami kontrolera platformy ASP.NET MVC 4 lub została ukończona &quot;pomocnicy, formularze i Walidacja&quot; praktycznym laboratorium, należy zwrócić uwagę...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: bfb1edfcb756706e44126e7e96803bd2e9ce99fb
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911022"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>Platforma ASP.NET MVC 4 Entity Framework szkieletu i migracje

Przez [Camp w sieci Web zespołu](https://twitter.com/webcamps)

[Pobierz Camp Web szkolenia Kit](https://aka.ms/webcamps-training-kit)

Jeśli dopiero zaczynasz z metodami kontrolera platformy ASP.NET MVC 4 lub została ukończona &quot;pomocnicy, formularze i Walidacja&quot; praktycznym laboratorium, należy pamiętać, że wiele logiki, aby utworzyć, zaktualizować, listy i Usuń dowolnej jednostki danych powtarza się wśród aplikacji. Aby mówią, że, jeśli model zawiera kilka klas do manipulowania, nie będzie prawdopodobnie poświęcać wiele czasu Pisanie metod akcji POST i GET dla każdej operacji podmiotu, a także każdego z widoków.

W tym laboratorium dowiesz się, jak używać szkieletu ASP.NET MVC 4 do automatycznego generowania linię bazową aplikacji CRUD (tworzenia, odczytu, aktualizowania lub usuwania). Uruchamianie z klasy prosty model i bez konieczności pisania nawet jednego wiersza kodu, utworzysz kontroler, który będzie zawierać wszystkich operacji CRUD, a także wszystkie potrzeby widoków. Po kompilowania i uruchamiania prostego rozwiązania, będziesz mieć wygenerowane, wraz z logikę MVC i widoków manipulowanie danymi bazy danych aplikacji.

Ponadto dowiesz się, jak łatwe jest korzystanie z migracją architektury jednostek do przeprowadzania aktualizacji modelu w całej aplikacji. Migracją architektury jednostek będzie można zmodyfikować bazy danych, po zmianie modelu przy użyciu prostych krokach. Wszystkie te należy pamiętać można do tworzenia i konserwowania aplikacji sieci web bardziej wydajnie, korzystając z zalet najnowszych funkcji platformy ASP.NET MVC 4.

> [!NOTE]
> Wszystkie przykładowy kod i fragmenty są uwzględnione w sieci Web Camp zestaw szkoleniowy, pod [wersji Microsoft — w sieci Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Projekt specyficzne dla tego laboratorium znajduje się w temacie [platformy ASP.NET MVC 4 Entity Framework szkieletu i migracje](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym laboratorium praktyczne dowiesz się jak:

- Funkcja tworzenia szkieletu ASP.NET na użytek operacji CRUD w kontrolerów.
- Zmień model bazy danych przy użyciu migracją architektury jednostek.

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

Poniższym ćwiczeniu tworzą tego laboratorium praktyczne:

1. [Za pomocą platformy ASP.NET MVC 4 szkieletu z migracją architektury jednostek](#Exercise1)

> [!NOTE]
> Dołączone w tym ćwiczeniu **zakończenia** folderu zawierającego wynikowy rozwiązania, należy uzyskać po zakończeniu wykonywania. Jeśli potrzebujesz dodatkowej pomocy, w tym ćwiczeniu, można użyć tego rozwiązania jako wskazówki.


Szacowany czas do ukończenia tego laboratorium: **30 minut**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Ćwiczenie 1: Szkieletów platformy ASP.NET MVC 4 z migracją architektury jednostek

Funkcja tworzenia szkieletu ASP.NET MVC umożliwia szybkie generowanie operacji CRUD w sposób Zestandaryzowany tworzenia niezbędne logikę, która umożliwia interakcję z warstwą bazy danych aplikacji.

W tym ćwiczeniu dowiesz się, jak za pomocą tworzenia szkieletu ASP.NET MVC 4 przy użyciu kodu najpierw utworzyć metody CRUD. Następnie zostanie dowiesz się, jak zaktualizować model zastosowania zmian w bazie danych, korzystając z migracją architektury jednostek.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Zadanie 1 — Tworzenie nowej platformy ASP.NET MVC 4 projektu szkieletów

1. Jeśli nie już jest otwarty, uruchom **programu Visual Studio 2012**.
2. Wybierz **pliku | Nowy projekt**. W nowym projekcie okno dialogowe, w obszarze **Visual C# | Web** zaznacz **aplikacji sieci Web programu ASP.NET MVC 4**. Nazwij projekt, aby **MVC4andEFMigrations** i Ustaw lokalizację na **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** folder w tym laboratorium. Ustaw **Nazwa rozwiązania** do **rozpocząć** i upewnij się, **Utwórz katalog rozwiązania** jest zaznaczone. Kliknij przycisk **OK**.

    ![Okno dialogowe Nowy projekt ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "okno dialogowe Nowy projekt ASP.NET MVC 4")

    *Okno dialogowe Nowy projekt ASP.NET MVC 4*
3. W **nowego projektu programu ASP.NET MVC 4** wybierz okno dialogowe **aplikacji internetowej** szablonu i upewnij się, że **Razor** jest wybrane **aparat widoku**. Kliknij przycisk **OK** do tworzenia projektu.

    ![Nowej aplikacji internetowej platformy ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nowej aplikacji internetowej platformy ASP.NET MVC 4")

    *Nowej aplikacji internetowej platformy ASP.NET MVC 4*
4. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modeli** i wybierz **Dodaj | Klasa** utworzyć prostą klasę osoby (POCO). Nadaj mu nazwę **osoby** i kliknij przycisk **OK**.
5. Otwórz klasę osoby i Wstaw następujące właściwości.

    (Code Snippet — *platformy ASP.NET MVC 4 i migracją architektury jednostek — właściwości osoby Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Kliknij przycisk **kompilacji | Tworzenie rozwiązania** Aby zapisać zmiany i skompiluj projekt.

    ![Tworzenie aplikacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Kompilowanie aplikacji")

    *Tworzenie aplikacji*
7. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolerów, a następnie wybierz **Dodaj | Kontroler**.
8. Nazwa kontrolera *PersonController* i ukończyć **opcji tworzenia szkieletu** z następującymi wartościami.

   1. W **szablonu** listy rozwijanej wybierz **kontroler MVC z akcjami odczytu/zapisu i widokami używający narzędzia Entity Framework** opcji.
   2. W **klasa modelu** listy rozwijanej wybierz **osoby** klasy.
   3. W **klasy kontekstu danych** listy wybierz  **&lt;nowy kontekst danych... &gt;**. Wybierz dowolną nazwę, a następnie kliknij przycisk **OK**.
   4. W **widoków** listy rozwijanej liście, upewnij się, że **Razor** jest zaznaczone.

      ![Dodawanie kontrolera osoby za pomocą tworzenia szkieletów](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "dodawania kontrolera osoby za pomocą tworzenia szkieletów")

      *Dodawanie kontrolera osoby za pomocą tworzenia szkieletów*
9. Kliknij przycisk **Dodaj** do utworzenia nowego kontrolera dla osoby za pomocą tworzenia szkieletów. Masz teraz generowane akcji kontrolera, a także widoki.

    ![Po utworzeniu kontroler osoby za pomocą tworzenia szkieletów](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "po utworzeniu kontroler osoby za pomocą tworzenia szkieletów")

    *Po utworzeniu kontroler osoby za pomocą tworzenia szkieletów*
10. Otwórz **PersonController** klasy. Należy zauważyć, że pełna metod akcji CRUD został wygenerowany automatycznie.

   ![Wewnątrz kontrolera osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "kontrolera wewnątrz osoby")

   *Wewnątrz kontrolera osoby*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Zadanie 2 — uruchamianie aplikacji

W tym momencie baza danych nie został jeszcze utworzony. W ramach tego zadania możesz uruchomić aplikację po raz pierwszy i testowania operacji CRUD. Baza danych zostanie utworzona na bieżąco Code First.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. W przeglądarce, należy dodać **/Person** do adresu URL, aby otworzyć stronę osoby.

    ![Pierwsze uruchomienie aplikacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "pierwszego uruchomienia aplikacji")

    *Aplikacji: pierwszym uruchomieniu*
3. Teraz będzie przeglądanie stron osoby i testowania operacji CRUD.

    1. Kliknij przycisk **Utwórz nową** na dodawanie nowej osoby. Wprowadź imię i nazwisko, a następnie kliknij przycisk **Utwórz**.

        ![Dodawanie nowej osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Dodawanie nowej osoby")

        *Dodawanie nowej osoby*
    2. Na liście osoby możesz usunąć, Edytuj lub Dodaj elementy.

        ![listy osób](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "listy osób")

        *Listy osób*
    3. Kliknij przycisk **szczegóły** można otworzyć szczegóły tej osoby.

        ![Szczegóły osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "szczegółów osoby")

        *Szczegóły tej osoby*
4. Zamknij przeglądarkę i wróć do programu Visual Studio. Zwróć uwagę, czy utworzono całej operacji CRUD dla jednostki osoby w całej aplikacji — od modelu do widoków — bez konieczności pisania nawet wiersza kodu!

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Zadanie 3 — aktualizowanie bazy danych, korzystając z migracją architektury jednostek

To zadanie zaktualizuje bazy danych, korzystając z migracją architektury jednostek. Wykryje to, jak łatwo zmienić model i uwzględnia zmiany w bazach danych przy użyciu funkcji migracją architektury jednostek.

1. Otwórz konsolę Menedżera pakietów. Wybierz **narzędzia** > **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.
2. W konsoli Menedżera pakietów wpisz następujące polecenie:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Włączanie migracji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Włączanie migracji")

    *Włączanie migracji*

    Polecenie Włączanie migracji tworzy **migracje** folder, który zawiera skrypt, aby zainicjować bazy danych.

    ![Folder migracje](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "migracji folderu")

    *Folder migracji*
3. Otwórz **Configuration.cs** pliku w folderze migracji. Znajdź Konstruktor klasy i zmień **AutomaticMigrationsEnabled** wartość *true*.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Otwórz klasę osoby, a następnie dodaj atrybut drugie imię. Przy użyciu tego nowego atrybutu w przypadku zmiany modelu.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Wybierz **kompilacji | Tworzenie rozwiązania** menu, aby skompilować aplikację.

    ![Tworzenie aplikacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Kompilowanie aplikacji")

    *Tworzenie aplikacji*
6. W konsoli Menedżera pakietów wpisz następujące polecenie:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    To polecenie będzie szukał zmiany w obiektach danych, a następnie dodaj wymagane polecenia, aby odpowiednio zmodyfikować bazy danych.

    ![Dodawanie drugiego imienia](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Dodawanie drugie imię")

    *Dodawanie drugie imię*
7. (Opcjonalnie) Można uruchomić następujące polecenie, aby wygenerować skrypt SQL z zastosowaniem aktualizacji różnicowych. Dzięki temu możesz ręcznie zaktualizować bazę danych (w tym przypadku go nie jest to konieczne), lub Zastosuj zmiany w innych bazach danych:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Generowanie skryptu SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generowanie skryptu SQL")

    *Generowanie skryptu SQL*

    ![Skrypt SQL aktualizacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "aktualizacji skrypt SQL")

    *Aktualizacja skrypt SQL*
8. W konsoli Menedżera pakietów wpisz następujące polecenie, aby zaktualizować bazę danych:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Aktualizowanie bazy danych](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "zaktualizowanie bazy danych")

    *Aktualizowanie bazy danych*

    Spowoduje to dodanie **MiddleName** kolumny w **osób** tabelę do dopasowania bieżącej definicji **osoby** klasy.
9. Po zaktualizowaniu bazy danych, kliknij prawym przyciskiem myszy folder kontrolera i wybierz **Dodaj | Kontroler** Aby dodać osobę kontrolera ponownie (Zakończ z wartości). Spowoduje to zaktualizowanie istniejących metod i widoki, dodawanie nowego atrybutu.

    ![Dodawanie aktualizacji kontrolera](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "dodanie aktualizacji kontrolera")

    *Aktualizacja kontrolera*
10. Kliknij przycisk **Dodaj**. Następnie wybierz wartości **zastąpić PersonController.cs** i **Zastąp skojarzone widoków** i kliknij przycisk **OK**.

   ![Dodawanie Zastąp kontrolera](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Aktualizacja kontrolera*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 — uruchamianie aplikacji

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Otwórz **/Person**. Należy zauważyć, że dane została zachowana, gdy została dodana kolumna drugie imię.

    ![Drugie imię dodano](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "drugie imię dodane")

    *Drugie imię dodane*
3. Jeśli klikniesz **Edytuj**, można dodać nazwę środka bieżącego osobie.

    ![Wydanie drugie imię](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "wydanie drugie imię")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym laboratorium praktycznego wiesz proste kroki, aby utworzyć operacji CRUD, za pomocą platformy ASP.NET MVC 4 tworzenia szkieletów przy użyciu dowolnej klasy modelu. Następnie mają przedstawiono sposób przeprowadzenia aktualizacji typu end to end w aplikacji — od bazy danych do widoków — korzystając z migracją architektury jednostek.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Możesz zainstalować **programu Microsoft Visual Studio Express 2012 for Web** lub inne &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe wskazówki ułatwiają kroki wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli już zainstalowano Instalatora platformy sieci Web, możesz otworzyć go i Wyszukaj produkt &quot; <em>Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.
2. Kliknij pozycję **Zainstaluj teraz**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Zainstaluj program Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "zainstalować program Visual Studio Express")

    *Zainstaluj program Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji oraz warunki, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Akceptowanie umowy licencyjnej*
5. Zaczekaj, aż do zakończenia procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu, a następnie zacznij pisać &quot; **VS Express**&quot;, następnie kliknij pozycję **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *VS Express for Web kafelka*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Dodatek B: korzystania z fragmentów kodu

Za pomocą wstawek kodu masz kod, czego potrzebujesz w zasięgu ręki. Dokument laboratorium informuje o dokładnie podczas korzystania z nich, jak pokazano na poniższej ilustracji.

![Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "fragmenty kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Za pomocą wstawek kodu programu Visual Studio, aby wstawić kod do projektu*

***Aby dodać wstawki kodu za pomocą klawiatury (tylko język C#)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Rozpocznij wpisywanie nazwy fragmentu kodu (bez spacji i łączniki).
3. Obejrzyj, jak technologia IntelliSense wyświetla zgodne z nazwami fragmenty kodu.
4. Wybierz prawidłowy fragment kodu lub pisz dalej do momentu wybrania debugowanej nazwy całego fragmentu kodu.
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

![Rozpocznij wpisywanie nazwy fragmentu kodu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Rozpocznij wpisywanie nazwy fragmentu kodu")

*Rozpocznij wpisywanie nazwy fragmentu kodu*

![Naciśnij klawisz Tab, aby zaznaczyć fragment wyróżnione](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragmentu kodu")

*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

![Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "rozwinie ponownie naciśnij klawisz Tab i fragment kodu")

*Ponownie naciśnij klawisz Tab i fragment kodu zostanie rozwinięte.*

***Aby dodać wstawki kodu za pomocą myszy (C#, Visual Basic i XML)*** 1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.

1. Wybierz **Wstaw fragment kodu** następuje **Moje fragmenty kodu**.
2. Wybierz odpowiedni fragment z listy, klikając go.

![Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu")

*Kliknij prawym przyciskiem myszy, które chcesz wstawić fragment kodu i wybierz opcję Wstaw fragment kodu*

![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "wybierz odpowiedni fragment z listy, klikając go")

*Wybierz odpowiedni fragment z listy, klikając go*
