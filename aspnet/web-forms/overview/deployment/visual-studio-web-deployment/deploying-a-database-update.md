---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: "Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji bazy danych | Dokumentacja firmy Microsoft"
author: tdykstra
description: "Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web przez używane..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 8e875a4282df78ec647579e74c3fbeabd2495fc2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Wdrażanie sieci Web ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji bazy danych
====================
przez [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobierz początkowego projektu](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Ta seria samouczek pokazuje, jak wdrożyć platformy ASP.NET (publikowanie) aplikacji do aplikacji sieci Web usługi aplikacji Azure lub innego dostawcy hostingu sieci web za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje dotyczące serii, zobacz [pierwszy samouczek z tej serii](introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku wprowadzić zmianę w bazie danych i zmiany kodu powiązanego przetestować zmiany w programie Visual Studio, a następnie wdrożyć aktualizację na środowiska testowego, tymczasową i produkcyjną.

Samouczek najpierw pokazano, jak zaktualizować bazę danych, który jest zarządzany przez migracje Code First, a następnie później go pokazano, jak zaktualizować bazę danych przy użyciu dostawcy dbDacFx narzędzia.

Przypomnienie: Jeśli coś nie działa podczas wykonywania kroków samouczka wyświetlony komunikat o błędzie, należy sprawdzić [Rozwiązywanie problemów z strony](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Wdrażanie aktualizacji bazy danych przy użyciu migracje Code First

W tej sekcji, Dodaj kolumnę daty urodzenia `Person` klasę podstawową dla `Student` i `Instructor` jednostek. Następnie należy zaktualizować strona, wyświetlająca instruktora danych, tak aby wyświetlone nowej kolumny. Ponadto można wdrożyć zmiany do testu, tymczasową i produkcyjną.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Dodaj kolumnę do tabeli w bazie danych aplikacji

1. W *ContosoUniversity.DAL* otwarciu projektu *Person.cs* i dodaj następującą właściwość na końcu `Person` klasy (powinny istnieć dwie zamykający nawias klamrowy następnego):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    Następnie zaktualizuj `Seed` metodę, którą zawiera wartość dla nowej kolumny. Otwórz *Migrations\Configuration.cs* i Zastąp blok kodu, który rozpoczyna się `var instructors = new List<Instructor>` z następujący blok kodu, który obejmuje informacje daty urodzenia:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna. Upewnij się, że ContosoUniversity.DAL jest nadal wybrany jako **domyślny projekt**.
3. W **Konsola Menedżera pakietów** wybierz **ContosoUniversity.DAL** jako **domyślny projekt**, a następnie wprowadź następujące polecenie:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Po zakończeniu działania tego polecenia programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMIgration` klasy, a następnie w `Up` — metoda zostanie wyświetlony kod, który tworzy nową kolumnę. `Up` Metoda tworzy kolumny, wprowadzając zmiany i `Down` — metoda usuwa kolumny, gdy użytkownik jest wycofywanie zmian.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Skompiluj rozwiązanie, a następnie wprowadź następujące polecenie w **Konsola Menedżera pakietów** okna (Upewnij się, nadal wybrano projektu ContosoUniversity.DAL):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Uruchamia programu Entity Framework `Up` metody, a następnie uruchamia `Seed` metody.

### <a name="display-the-new-column-in-the-instructors-page"></a>Wyświetlanie nowej kolumny, na stronie instruktorów

1. W projekcie ContosoUniversity Otwórz *Instructors.aspx* i Dodaj nowe pole do szablonu, aby wyświetlić datę urodzenia. Dodaj go między te przypisania zatrudnienia daty i pakietu office:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Jeśli wcięcia kodu jest niezsynchronizowana, możesz nacisnąć klawisze CTRL-K, a następnie klawisz CTRL-D automatycznie sformatuj ponownie plik.)
2. Uruchom aplikację, a następnie kliknij przycisk **instruktorów** łącza.

    Podczas ładowania strony widać, że ma nowy Data urodzenia.

    ![Strona instruktorów z Data urodzenia](deploying-a-database-update/_static/image2.png)
3. Zamknij przeglądarkę.

### <a name="deploy-the-database-update"></a>Wdrażanie aktualizacji bazy danych

1. W **Eksploratora rozwiązań** wybierz projekt ContosoUniversity.
2. W **jeden kliknij publikowania w sieci Web** narzędzi, kliknij przycisk **testu** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**. (Wyłączenie pasku narzędzi wybierz projekt ContosoUniversity **Eksploratora rozwiązań**.)

    Program Visual Studio wdroży zaktualizowaną aplikację, a następnie otwiera przeglądarkę na stronę główną.
3. Uruchom **instruktorów** stronę, aby sprawdzić, czy aktualizacja została pomyślnie wdrożona.

    Gdy aplikacja podejmie próbę dostępu do bazy danych dla tej strony, Code First aktualizacje schematu bazy danych i uruchamia `Seed` metody. Gdy zostanie wyświetlona strona, można znaleźć oczekiwanej **Data urodzenia** kolumny zawierającej daty w nim.
4. W **jeden kliknij publikowania w sieci Web** narzędzi, kliknij przycisk **przemieszczania** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**.
5. Uruchom **instruktorów** strony w przejściowym można zweryfikować, że aktualizacja została pomyślnie wdrożona.
6. W **jeden kliknij publikowania w sieci Web** narzędzi, kliknij przycisk **produkcji** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**.
7. Uruchom **instruktorów** strony w środowisku produkcyjnym, aby zweryfikować, że aktualizacja została pomyślnie wdrożona.

    Rzeczywistej produkcji aktualizacji aplikacji, która obejmuje zmianę w bazie danych przy użyciu również zwykle zajmie aplikacji w tryb offline podczas wdrażania *aplikacji\_offline.htm*, jak opisany w poprzednim samouczka.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Wdrażanie aktualizacji bazy danych przy użyciu dostawcy dbDacFx narzędzia

W tej sekcji możesz dodać *komentarze* kolumny *użytkownika* tabeli w bazie danych członkostwa i utworzyć stronę, która pozwala wyświetlić i edytować komentarze dla każdego użytkownika. Następnie możesz wdrożyć zmiany do testu, tymczasowych i produkcyjnych.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Dodaj kolumnę do tabeli w bazie danych członkostwa

1. W programie Visual Studio Otwórz **Eksplorator obiektów SQL Server**.
2. Rozwiń węzeł **(localdb) \v11.0**, rozwiń węzeł **baz danych**, rozwiń węzeł **aspnet ContosoUniversity** (nie **aspnet-ContosoUniversity-produkcyjną**) a następnie rozwiń węzeł **tabel**.

    Jeśli nie widzisz **(localdb) \v11.0** w obszarze **programu SQL Server** węzła, kliknij prawym przyciskiem myszy **programu SQL Server** węzeł i kliknij przycisk **dodawania serwera SQL**. W **Połącz z serwerem** wprowadź okno dialogowe *(localdb) \v11.0* jako **nazwy serwera**, a następnie kliknij przycisk **Connect**.

    Jeśli nie widzisz **aspnet ContosoUniversity**, uruchom projekt i zaloguj się za pomocą *admin* poświadczeń (hasło jest *devpwd*), a następnie Odśwież  **Eksplorator obiektów SQL Server** okna.
3. Kliknij prawym przyciskiem myszy **użytkowników** tabeli, a następnie kliknij przycisk **Widok projektanta**.

    ![Projektant widoków SSOX](deploying-a-database-update/_static/image3.png)
4. W projektancie, Dodaj *komentarze* kolumny i zapewnić ich *nvarchar(128)* i wartości null, a następnie kliknij przycisk **aktualizacji**.

    ![Dodawanie komentarzy kolumny](deploying-a-database-update/_static/image4.png)
5. W **aktualizacje bazy danych w wersji zapoznawczej** kliknij **aktualizacji bazy danych**.

    ![Aktualizacje bazy danych w wersji zapoznawczej](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Utwórz stronę, aby wyświetlić i edytować nowej kolumny.

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **konta** kliknij folder w projekcie ContosoUniversity **Dodaj**, a następnie kliknij przycisk **nowy element** .
2. Utwórz nową **strona wzorcowa przy użyciu formularza sieci Web** i nadaj mu nazwę *UserInfo.aspx*. Zaakceptuj wartość domyślną *Site.Master* pliku jako strony wzorcowej.
3. Skopiuj następujący kod do `MainContent` `Content` elementu (ostatni 3 `Content` elementy):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Kliknij prawym przyciskiem myszy *UserInfo.aspx* i kliknij przycisk **Wyświetl w przeglądarce**.
5. Zaloguj się przy użyciu programu *admin* poświadczenia użytkownika (hasło jest *devpwd*) i Dodaj niektóre komentarze do użytkownika w celu zweryfikowania, że strony działa prawidłowo.

    ![Strona informacje o użytkowniku](deploying-a-database-update/_static/image6.png)
6. Zamknij przeglądarkę.

## <a name="deploy-the-database-update"></a>Wdrażanie aktualizacji bazy danych

Aby wdrożyć przy użyciu dostawcy dbDacFx, wystarczy wybrać **aktualizacji bazy danych** opcji w profilu publikowania. Jednak dla pierwszego wdrożenia tej opcji należy również skonfigurowane niektóre dodatkowe na uruchamianie skryptów SQL: te są nadal w profilu i należy uniemożliwić ich ponowne uruchomienie.

1. Otwórz **publikowanie w sieci Web** Kreator prawym przyciskiem myszy projekt ContosoUniversity, a następnie klikając polecenie **publikowania**.
2. Wybierz **testu** profilu.
3. Kliknij przycisk **ustawienia** kartę.
4. W obszarze **połączenia DefaultConnection**, wybierz pozycję **aktualizacji bazy danych**.
5. Wyłącz dodatkowych skryptów, skonfigurowanych do uruchamiania dla pierwszego wdrożenia:

    1. Kliknij przycisk **skonfigurować aktualizacje bazy danych**.
    2. W **Konfigurowanie aktualizacji bazy danych** okno dialogowe, usuń zaznaczenie pola wyboru obok pozycji *Grant.sql* i *aspnet-data-dev.sql*.
    3. Kliknij przycisk **Zamknij**.
6. Kliknij przycisk **Podgląd** kartę.
7. W obszarze **baz danych** i po prawej stronie **połączenia DefaultConnection**, kliknij przycisk **bazy danych w wersji zapoznawczej** łącza.

    ![Podgląd bazy danych](deploying-a-database-update/_static/image7.png)

    Okno podglądu pokazuje skrypt, które będą uruchamiane w docelowej bazie danych, ten schemat bazy danych pasuje do schematu źródłowej bazy danych. Skrypt zawiera polecenia ALTER TABLE, który dodaje nową kolumnę.
8. Zamknij **Podgląd bazy danych** , a następnie kliknij przycisk **publikowania**.

    Program Visual Studio wdroży zaktualizowaną aplikację, a następnie otwiera przeglądarkę na stronę główną.
9. Uruchom na stronie informacje o użytkowniku (Dodaj *Account/UserInfo.aspx* na adres URL strony głównej) można zweryfikować, że aktualizacja została pomyślnie wdrożona. Należy się zalogować, wprowadzając *admin* i *devpwd*.

    Dane w tabelach nie została wdrożona domyślna, a nie skonfigurowała skryptu wdrażania danych do uruchomienia, więc nie zostaną odnalezione komentarz, który został dodany do rozwoju. Można teraz dodać nowy komentarz w przejściowym, aby sprawdzić, czy zmiana została wdrożona do bazy danych i strony działa prawidłowo.
10. Postępuj zgodnie z tą samą procedurą, aby wdrożyć tymczasową i produkcyjną.

    Należy pamiętać wyłączyć dodatkowych skryptów. Jedyną różnicą w porównaniu do profilu testu jest czy tylko jeden skrypt w tymczasowej spowoduje wyłączenie i produkcji profile, ponieważ zostały one skonfigurowane do działania tylko *aspnet produkcyjną data.sql*.

    Poświadczenia dla tymczasowych i produkcyjnych są administratora, jak i prodpwd.

    Rzeczywistej produkcji aktualizacji aplikacji, która obejmuje zmianę w bazie danych aplikacji w tryb offline podczas wdrażania będzie również zwykle zajmują przekazując *aplikacji\_offline.htm* przed publikowania i usuwania później, jak przedstawiono w [poprzedniego samouczek](deploying-a-code-update.md).

## <a name="summary"></a>Podsumowanie

Teraz wdrożeniu aktualizacji aplikacji, która obejmuje zmianę bazy danych przy użyciu zarówno migracje Code First i dostawcy dbDacFx narzędzia.

![Strona instruktorów z Data urodzenia](deploying-a-database-update/_static/image8.png)

![Strona informacje o użytkowniku](deploying-a-database-update/_static/image9.png)

Następny samouczek pokazuje, jak wykonać wdrożenia przy użyciu wiersza polecenia.

>[!div class="step-by-step"]
[Poprzednie](deploying-a-code-update.md)
[dalej](command-line-deployment.md)
