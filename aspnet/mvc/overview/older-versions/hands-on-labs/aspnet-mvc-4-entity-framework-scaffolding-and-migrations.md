---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: Platforma ASP.NET MVC 4 Entity Framework rusztowania i migracje | Dokumentacja firmy Microsoft
author: rick-anderson
description: Jeśli znasz metod kontrolera ASP.NET MVC 4 lub została ukończona &quot;pomocników, formularzy i sprawdzania poprawności&quot; laboratorium praktycznego, należy zwrócić uwagę...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 548afe1926eed49841251832d54dc213da0cb753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>Platforma ASP.NET MVC 4 Entity Framework rusztowania i migracji

Przez [obozów sieci Web Team](https://twitter.com/webcamps)

[Pobierz obozów sieci Web uczenie Kit](https://aka.ms/webcamps-training-kit)

Jeśli znasz metod kontrolera ASP.NET MVC 4 lub została ukończona &quot;pomocników, formularzy i sprawdzania poprawności&quot; laboratorium praktycznego, należy pamiętać, że wiele logiki do tworzenia, aktualizacji, listy i usuń wszelkie dane powtarza się wśród aplikacji. Aby nie mówią, że, jeśli model ma kilka klas do manipulowania, będzie prawdopodobnie poświęcić przez dłuższy czas zapisywania metod akcji POST i GET dla każdej operacji jednostki, a także wszystkich widoków.

W tym laboratorium dowiesz się, jak używać szkieletów ASP.NET MVC 4 mają być automatycznie generowane linia bazowa CRUD aplikacji (tworzenia, odczytu, aktualizacji i usuwania). Uruchamianie z klasy modelu prostego i bez napisania jakiegokolwiek wiersza kodu, utworzysz kontroler, który będzie zawierać wszystkich operacji CRUD, a także wszystkie widoki niezbędne. Po tworzenia i uruchamiania prostym rozwiązaniem, konieczne będzie generowany wraz z logikę MVC i widoków do manipulowania danymi bazy danych aplikacji.

Ponadto dowiesz się, jak łatwo jest Użyj Entity Framework migracji do przeprowadzania aktualizacji modelu w całej aplikacji. Entity Framework migracji będzie można zmodyfikować bazy danych po zmianie modelu prostych kroków. Wszystkie te na uwadze można do tworzenia i obsługi aplikacji sieci web wydajniej, korzystając z najnowszych funkcji platformy ASP.NET MVC 4.

> [!NOTE]
> Wszystkie przykładowy kod i fragmenty kodu są uwzględnione w sieci Web obozów zestaw szkoleniowy, pod [wersje Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Specyficzne dla tego laboratorium projektu jest dostępna na [migracja i ASP.NET MVC 4 Entity Framework szkieletów](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Cele

W tym laboratorium Hands-On przedstawiono sposób:

- Użyj szkieletów ASP.NET dla operacji CRUD w kontrolerów.
- Zmień model bazy danych przy użyciu Entity Framework migracji.

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

Poniższym ćwiczeniu tworzą tego laboratorium Hands-On:

1. [Przy użyciu funkcji szkieletów platformy ASP.NET MVC 4 z Entity Framework migracji](#Exercise1)

> [!NOTE]
> Towarzyszy tego ćwiczenia **zakończenia** folderu zawierającego wynikowy rozwiązanie, należy uzyskać po zakończeniu wykonywania. Jeśli potrzebujesz dodatkowej pomocy, Praca do wykonywania, można użyć tego rozwiązania jako przewodnika.


Szacowany czas trwania tego laboratorium: **30 minut**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Ćwiczenie 1: Przy użyciu funkcji szkieletów platformy ASP.NET MVC 4 z Entity Framework migracji

Szkieletów MVC ASP.NET zapewnia możliwość szybkiego generowania operacji CRUD w sposób Zestandaryzowany tworzenia logiki niezbędne, które umożliwia interakcję z warstwy bazy danych aplikacji.

W tym ćwiczeniu dowiesz się, jak używać szkieletów ASP.NET MVC 4 z kodem najpierw utworzyć metody CRUD. Następnie dowiesz jak zaktualizować model zastosowania zmian w bazie danych przy użyciu Entity Framework migracji.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Zadanie 1 — Tworzenie nowej technologii ASP.NET MVC 4 projektu przy użyciu funkcji szkieletów

1. Jeśli nie już otwarty, uruchom **programu Visual Studio 2012**.
2. Wybierz **pliku | Nowy projekt**. W nowy projekt okna dialogowego, w obszarze **Visual C# | Web** zaznacz **aplikacji sieci Web programu ASP.NET MVC 4**. Nazwij projekt do **MVC4andEFMigrations** i jako jego lokalizację ustaw **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** folder tego laboratorium. Ustaw **Nazwa rozwiązania** do **rozpocząć** i upewnij się, **Utwórz katalog rozwiązania** jest zaznaczony. Kliknij przycisk **OK**.

    ![Okno dialogowe nowego projektu platformy ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "okno dialogowe Nowy projekt ASP.NET MVC 4")

    *Okno dialogowe nowego projektu platformy ASP.NET MVC 4*
3. W **nowy projekt programu ASP.NET MVC 4** wybierz okno dialogowe **aplikacji internetowej** szablonu i upewnij się, że **Razor** jest wybrane **aparat widoku**. Kliknij przycisk **OK** Aby utworzyć projekt.

    ![Nowej aplikacji internetowej platformy ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nowej aplikacji internetowej platformy ASP.NET MVC 4")

    *Nowej aplikacji internetowej platformy ASP.NET MVC 4*
4. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modele** i wybierz **Dodaj | Klasa** utworzyć prostą klasę osoby (POCO). Nadaj mu nazwę **osoby** i kliknij przycisk **OK**.
5. Otwórz klasy osoby i Wstaw następujące właściwości.

    (Fragment - kodu *platformy ASP.NET MVC 4 oraz Entity Framework migracje - właściwości osoby Ex1*)


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
~~~
6. Kliknij przycisk **kompilacji | Tworzenie rozwiązania** Aby zapisać zmiany i skompilować projekt.

    ![Tworzenie aplikacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "tworzenie aplikacji")

    *Tworzenie aplikacji*
7. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy folder kontrolery, a następnie wybierz **Dodaj | Kontroler**.
8. Nazwa kontrolera *PersonController* i ukończyć **opcje szkieletów** z następującymi wartościami.

   1. W **szablonu** listy rozwijanej wybierz **kontroler MVC z akcjami odczytu/zapisu i widokami używający narzędzia Entity Framework** opcji.
   2. W **klasa modelu** listy rozwijanej wybierz **osoby** klasy.
   3. W **klasy kontekstu danych** listy, wybierz  **&lt;nowy kontekst danych... &gt;**. Wybierz dowolną nazwę, a następnie kliknij przycisk **OK**.
   4. W **widoków** rozwijania listy, upewnij się, że **Razor** jest zaznaczone.

      ![Dodawanie kontrolera osoby z szkieletów](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "dodawania kontrolera osoby z szkieletów")

      *Dodawanie kontrolera osoby z szkieletów*
9. Kliknij przycisk **Dodaj** do utworzenia nowego kontrolera dla osoby z szkieletów. Wygenerowane zostały akcji kontrolera, a także widoki.

    ![Po utworzeniu kontrolera osoby za pomocą szkieletów](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "po utworzeniu kontrolera osoby za pomocą szkieletów")

    *Po utworzeniu kontrolera osoby za pomocą szkieletów*
10. Otwórz **PersonController** klasy. Należy zauważyć, że pełna metod akcji CRUD został wygenerowany automatycznie.

   ![Wewnątrz kontrolera osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "kontrolera wewnątrz osoby")

   *Wewnątrz kontrolera osoby*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Zadanie 2 — uruchamianie aplikacji

W tym momencie bazy danych nie został jeszcze utworzony. W tym zadaniu zostanie Uruchom aplikację po raz pierwszy, a test operacji CRUD. Bazy danych zostanie utworzona na bieżąco, Code First.

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. W przeglądarce, Dodaj **/Person** do adresu URL, aby otworzyć stronę osoby.

    ![Najpierw uruchom aplikację](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "pierwszym uruchomieniu aplikacji")

    *Aplikację: najpierw uruchom*
3. Teraz zostanie Eksploruj stron osoby i przetestować operacji CRUD.

    1. Kliknij przycisk **Utwórz nowy** można dodać nowej osoby. Wprowadź imię i nazwisko, a następnie kliknij przycisk **Utwórz**.

        ![Dodawanie nowej osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Dodawanie nowej osoby")

        *Dodawanie nowej osoby*
    2. Na liście osoby możesz usunąć, Edytuj lub dodawania elementów.

        ![listy osób](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "listy osób")

        *Listy osób*
    3. Kliknij przycisk **szczegóły** można otworzyć szczegółów danej osoby.

        ![Szczegóły osoby](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "szczegóły osoby")

        *Szczegóły osoby*
4. Zamknij przeglądarkę i powrócić do programu Visual Studio. Zauważ, że utworzono całego CRUD dla obiekt osoby w całej aplikacji — model do widoków — bez konieczności pisania pojedynczy wiersz kodu!

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Zadanie 3 — aktualizowanie bazy danych używającej Entity Framework migracji

To zadanie zaktualizuje bazę danych przy użyciu Entity Framework migracji. Możesz zauważyć, jak łatwo jest można zmienić modelu na model i uwzględnia zmiany w bazach danych za pomocą funkcji Entity Framework migracji.

1. Otwórz konsolę Menedżera pakietów. Wybierz **narzędzia | Menedżer pakietów biblioteki | Konsola Menedżera pakietów**.
2. W konsoli Menedżera pakietów wprowadź następujące polecenie:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Włączanie migracje](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "włączenie migracji")

    *Włączanie migracji*

    Polecenie Enable-migracji tworzy **migracje** folder zawierający skrypt, aby zainicjować bazy danych.

    ![Migrations folder](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Migrations folder")

    *Migrations folder*
3. Otwórz **Configuration.cs** pliku w folderze migracji. Znajdź Konstruktor klasy i zmień **AutomaticMigrationsEnabled** do wartości *true*.


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
~~~
4. Otwórz klasy osoby i Dodaj atrybut drugie imię. Z tego nowego atrybutu zmieniasz modelu.


~~~
[!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
~~~
5. Wybierz **kompilacji | Tworzenie rozwiązania** menu do skompilowania aplikacji.

    ![Tworzenie aplikacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "tworzenie aplikacji")

    *Tworzenie aplikacji*
6. W konsoli Menedżera pakietów wprowadź następujące polecenie:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    To polecenie będzie wyglądać na zmiany w obiektach danych, a następnie dodaj niezbędne polecenia można zmodyfikować odpowiednio bazy danych.

    ![Dodawanie drugie imię](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Dodawanie drugie imię")

    *Dodawanie drugie imię*
7. (Opcjonalnie) Można uruchomić następujące polecenie, aby wygenerować skryptu SQL z aktualizacji różnicowych. Umożliwi to należy ręcznie zaktualizować bazę danych (w tym przypadku go nie jest to konieczne), lub Zastosuj zmiany w innych bazach danych:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Generowanie skryptu SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generowanie skryptu SQL")

    *Generowanie skryptu SQL*

    ![Aktualizacja skryptu SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "aktualizacji skryptu SQL")

    *Aktualizacja skryptu SQL*
8. W konsoli Menedżera pakietów wprowadź następujące polecenie, aby zaktualizować bazę danych:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Uaktualnienie bazy danych](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "aktualizowania bazy danych")

    *Uaktualnienie bazy danych*

    Spowoduje to dodanie **MiddleName** kolumny w **osób** tabeli, aby dopasować bieżącej definicji **osoby** klasy.
9. Po zaktualizowaniu bazy danych, kliknij prawym przyciskiem myszy folder kontrolera i wybierz **Dodaj | Kontroler** do dodania osoby kontrolera ponownie (razem z takich samych wartości). Ta operacja spowoduje zaktualizowanie istniejących metod i widoki, dodanie nowego atrybutu.

    ![Dodawanie aktualizacji kontrolera](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Dodawanie aktualizacji kontrolera")

    *Aktualizowanie kontrolera*
10. Kliknij przycisk **Dodaj**. Następnie wybierz wartości **zastąpić PersonController.cs** i **Zastąp skojarzone widoków** i kliknij przycisk **OK**.

   ![Dodawanie Zastąp kontrolera](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Aktualizowanie kontrolera*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 - uruchamiania aplikacji

1. Naciśnij klawisz **F5** do uruchomienia aplikacji.
2. Otwórz **/Person**. Zwróć uwagę, że dane została zachowana, podczas gdy drugie imię kolumna została dodana.

    ![Drugie imię dodane](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "drugie imię dodane")

    *Drugie imię dodane*
3. Jeśli klikniesz przycisk **Edytuj**, można dodać drugie imię do bieżącej osoby.

    ![Wydanie drugie imię](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "wydanie drugie imię")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Podsumowanie

W tym laboratorium praktycznego uzyskanych proste kroki umożliwiające utworzenie operacji CRUD z platformy ASP.NET MVC 4 rusztowania, używając dowolnej klasy modelu. Następnie uzyskanych przeprowadzania aktualizacji pełnego w aplikacji — z bazy danych do widoków — za pomocą Entity Framework migracji.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Dodatek A: Instalowanie programu Visual Studio Express 2012 for Web

Można zainstalować **Microsoft Visual Studio Express 2012 for Web** lub innym &quot;Express&quot; przy użyciu wersji **[Instalatora platformy sieci Web firmy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Poniższe instrukcje przedstawiono czynności wymagane do zainstalowania *programu Visual studio Express 2012 for Web* przy użyciu *Instalatora platformy sieci Web firmy Microsoft*.

1. Przejdź do [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Alternatywnie, jeśli została już zainstalowana Instalatora platformy sieci Web, można otworzyć go i Wyszukaj produkt &quot; <em>programu Visual Studio Express 2012 for Web z zestawem Windows Azure SDK</em>&quot;.
2. Polecenie **teraz zainstalować**. Jeśli nie masz **Instalatora platformy sieci Web** nastąpi przekierowanie do pobrania i zainstalowania go najpierw.
3. Raz **Instalatora platformy sieci Web** jest otwarty, kliknij przycisk **zainstalować** można uruchomić Instalatora.

    ![Instalowanie programu Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "instalacji programu Visual Studio Express")

    *Instalowanie programu Visual Studio Express*
4. Odczytywanie wszystkich produktów licencji i warunków, a następnie kliknij przycisk **akceptuję** aby kontynuować.

    ![Akceptowanie umowy licencyjnej](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Akceptowanie umowy licencyjnej*
5. Poczekaj na zakończenie procesu pobierania i instalacji.

    ![Postęp instalacji](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Postęp instalacji*
6. Po zakończeniu instalacji kliknij przycisk **Zakończ**.

    ![Instalacja została zakończona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Instalacja została zakończona*
7. Kliknij przycisk **zakończenia** aby zamknąć Instalatora platformy sieci Web.
8. Aby otworzyć program Visual Studio Express for Web, przejdź do **Start** ekranu i zacznij pisać &quot; **VS Express**&quot;, następnie kliknij polecenie **VS Express for Web** Kafelek.

    ![VS Express for Web kafelka](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *VS Express for Web kafelka*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Dodatek B: korzystania z wstawek kodu

Wstawki kodu zapewniają całego kodu, które są potrzebne w zasięgu ręki. Dokument laboratorium informuje o dokładnie po ich użycia, jak pokazano na poniższej ilustracji.

![Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "wstawki kodu za pomocą programu Visual Studio, aby wstawić kod do projektu")

*Aby wstawić kod do projektu przy użyciu wstawki kodu programu Visual Studio*

***Aby dodać fragment kodu za pomocą klawiatury (C# tylko)***

1. Umieść kursor, w którym chcesz wstawić kod.
2. Zacznij wpisywać nazwę fragment (bez spacji i łączniki).
3. Obejrzyj jako IntelliSense wyświetla zgodne z nazwami wstawki.
4. Wybierz prawidłowe fragment (lub zachować wpisywanie do momentu wybrania fragment całą nazwę).
5. Naciśnij klawisz Tab dwa razy, aby wstawić fragment kodu w lokalizacji kursora.

![Rozpocznij wpisywanie nazwy fragment](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "zacznij wpisywać nazwę wstawki programu")

*Rozpocznij wpisywanie nazwy fragment kodu*

![Naciśnij klawisz Tab, aby wybrać wyróżnione fragment](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu")

*Naciśnij klawisz Tab, aby wybrać wyróżnione fragment kodu*

![Ponownie naciśnij klawisz Tab i fragment rozwinie](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "rozwinie ponownie naciśnij klawisz Tab i wstawki kodu")

*Rozwinie ponownie naciśnij klawisz Tab i wstawki kodu*

***Aby dodać fragment kodu za pomocą myszy (C#, Visual Basic i XML)*** 1. Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu.

1. Wybierz **wstawić fragment** następuje **Moje wstawki kodu**.
2. Wybierz odpowiedni fragment z listy, klikając go.

![Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu")

*Kliknij prawym przyciskiem myszy, gdy chcesz wstawić fragment kodu i wybierz wstawić fragment kodu*

![Wybierz odpowiedni fragment z listy, klikając go](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "wybierz odpowiedni fragment z listy, klikając go")

*Wybierz odpowiedni fragment z listy, klikając go*
