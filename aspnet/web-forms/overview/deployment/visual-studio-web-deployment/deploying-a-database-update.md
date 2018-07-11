---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji bazy danych | Dokumentacja firmy Microsoft'
author: tdykstra
description: W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub dostawcy hostingu w innych firm, używane...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 88652a33d5367241fec4d442e9deb15d88a096c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817626"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Wdrażanie aplikacji internetowych ASP.NET przy użyciu programu Visual Studio: Wdrażanie aktualizacji bazy danych
====================
przez [Tom Dykstra](https://github.com/tdykstra)

[Pobieranie projektu startowego](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków dowiesz się, jak wdrożyć (opublikować) platformy ASP.NET sieci web aplikacji do usługi Azure App Service Web Apps lub innych firm dostawcy hostingu za pomocą programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje na temat serii, zobacz [pierwszym samouczku tej serii](introduction.md).


## <a name="overview"></a>Omówienie

W tym samouczku, wprowadzić zmianę w bazie danych i zmian kodu powiązanego przetestować zmiany w programie Visual Studio, a następnie wdrożyć aktualizację w środowiskach testowych, przejściowych i produkcyjnych.

Samouczek najpierw pokazano, jak można zaktualizować bazy danych, który jest zarządzany przez migracje Code First, a następnie później pokazano, jak zaktualizować bazę danych przy użyciu dostawcy dbDacFx.

Przypomnienie: Jeśli otrzymujesz komunikat o błędzie lub coś nie działa podczas wykonywania kroków samouczka, upewnij się sprawdzić [strona rozwiązywania problemów](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Wdrażanie aktualizacji bazy danych przy użyciu migracje Code First

W tej sekcji Dodaj kolumnę daty urodzenia `Person` klasa podstawowa dla `Student` i `Instructor` jednostek. Następnie należy zaktualizować Strona która wyświetla dane przez instruktorów, tak, aby wyświetlał nowej kolumny. Na koniec wdrożysz zmiany do testowania, przejściowego i produkcji.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Dodawanie kolumny do tabeli w bazie danych aplikacji

1. W *ContosoUniversity.DAL* otwarty projekt *osoba.cs* i dodaj następującą właściwość na końcu `Person` klasy (powinny istnieć dwie zamykający nawias klamrowy następujący):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    Następnie zaktualizuj `Seed` metodę, tak że zawiera wartość dla nowej kolumny. Otwórz *Migrations\Configuration.cs* i Zastąp blok kodu, który rozpoczyna się `var instructors = new List<Instructor>` z poniższy blok kodu, który zawiera informacje o dacie urodzenia:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Skompiluj rozwiązanie, a następnie otwórz **Konsola Menedżera pakietów** okna. Upewnij się, że ContosoUniversity.DAL jest nadal wybrany jako **projekt domyślny**.
3. W **Konsola Menedżera pakietów** wybierz **ContosoUniversity.DAL** jako **projekt domyślny**, a następnie wprowadź następujące polecenie:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Po zakończeniu tego polecenia, programu Visual Studio otwiera plik klasy, który definiuje nowy `DbMIgration` klasy, a następnie w `Up` metody zostanie wyświetlony kod, który tworzy nową kolumnę. `Up` Metoda tworzy kolumny, wprowadzając zmiany, a `Down` metoda usuwa kolumny, gdy użytkownik jest wycofywanie zmian.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Skompiluj rozwiązanie, a następnie wprowadź następujące polecenie w **Konsola Menedżera pakietów** okna (Upewnij się, projekt ContosoUniversity.DAL została ona zaznaczona):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Uruchamia programu Entity Framework `Up` metody, a następnie uruchamia `Seed` metody.

### <a name="display-the-new-column-in-the-instructors-page"></a>Wyświetlanie nowych kolumn na stronie instruktorów

1. W projekcie ContosoUniversity Otwórz *Instructors.aspx* i dodać nowe pole do szablonu, aby wyświetlić datę urodzenia. Dodać go między tymi zatrudnienia przypisania daty i pakietu office:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Jeśli wcięcia kodu o szerokość jest niezsynchronizowana, można nacisnąć klawisze CTRL-K, a następnie CTRL-D do automatycznego ponownego sformatowania pliku.)
2. Uruchom aplikację, a następnie kliknij przycisk **Instruktorzy** łącza.

    Po załadowaniu strony zobaczysz, że ma nowe pole daty urodzenia.

    ![Strona Instruktorzy z datę urodzenia](deploying-a-database-update/_static/image2.png)
3. Zamknij przeglądarkę.

### <a name="deploy-the-database-update"></a>Wdrażanie aktualizacji bazy danych

1. W **Eksploratora rozwiązań** wybierz projekt ContosoUniversity.
2. W **Web publikacja w pojedynczym kliknięciem** narzędzi, kliknij przycisk **testu** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**. (Jeśli pasek narzędzi jest wyłączona, wybierz projekt ContosoUniversity **Eksploratora rozwiązań**.)

    Program Visual Studio wdroży zaktualizowaną aplikację i otwarciu przeglądarki do strony głównej.
3. Uruchom **Instruktorzy** stronę, aby sprawdzić, czy aktualizacja została wdrożona pomyślnie.

    Gdy aplikacja podejmie próbę dostępu do bazy danych dla tej strony, Code First aktualizuje schemat bazy danych i uruchamia `Seed` metody. Jeśli zostanie wyświetlona strona, zobaczysz oczekiwany **Data urodzenia** kolumnie znajdują się daty w nim.
4. W **Web publikacja w pojedynczym kliknięciem** narzędzi, kliknij przycisk **przemieszczania** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**.
5. Uruchom **Instruktorzy** strony na etapie wdrażania przejściowego, aby sprawdzić, czy aktualizacja została wdrożona pomyślnie.
6. W **Web publikacja w pojedynczym kliknięciem** narzędzi, kliknij przycisk **produkcji** profil publikowania, a następnie kliknij przycisk **publikowanie w sieci Web**.
7. Uruchom **Instruktorzy** strony w środowisku produkcyjnym, aby sprawdzić, czy aktualizacja została wdrożona pomyślnie.

    Do rzeczywistej produkcji aktualizacji aplikacji, która obejmuje zmianę w bazie danych zwykle także wykonywane w aplikacji w trybie offline podczas wdrażania za pomocą *aplikacji\_offline.htm*, jak pokazano w poprzednim samouczku.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Wdrażanie aktualizacji bazy danych przy użyciu dostawcy dbDacFx

W tej sekcji dodasz *komentarze* kolumny *użytkownika* tabeli w bazie danych członkostwa i utworzyć stronę, która umożliwia wyświetlanie i edytowanie komentarzy dla każdego użytkownika. Następnie możesz wdrożyć zmiany do testowania, przejściowego i produkcji.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Dodawanie kolumny do tabeli w bazie danych członkostwa

1. W programie Visual Studio, otwórz **Eksplorator obiektów SQL Server**.
2. Rozwiń **(localdb) \v11.0**, rozwiń węzeł **baz danych**, rozwiń węzeł **aspnet ContosoUniversity** (nie **aspnet-ContosoUniversity-Prod**) a następnie rozwiń węzeł **tabel**.

    Jeśli nie widzisz **(localdb) \v11.0** w obszarze **programu SQL Server** węzła, kliknij prawym przyciskiem myszy **programu SQL Server** węzła i kliknij przycisk **dodawania serwera SQL**. W **Połącz z serwerem** wprowadź okno dialogowe *(localdb) \v11.0* jako **nazwy serwera**, a następnie kliknij przycisk **Connect**.

    Jeśli nie widzisz **aspnet ContosoUniversity**, uruchom projekt i zaloguj się przy użyciu *administratora* poświadczenia (hasło jest *devpwd*), a następnie Odśwież  **Eksplorator obiektów SQL Server** okna.
3. Kliknij prawym przyciskiem myszy **użytkowników** tabeli, a następnie kliknij przycisk **Projektant widoków**.

    ![Projektant widoków SSOX](deploying-a-database-update/_static/image3.png)
4. W projektancie, Dodaj *komentarze* kolumny i przypisz ją *nvarchar(128)* i dopuszcza wartości null, a następnie kliknij przycisk **aktualizacji**.

    ![Dodawanie kolumny Comments](deploying-a-database-update/_static/image4.png)
5. W **Podgląd aktualizacji bazy danych** kliknij **Aktualizuj bazę danych**.

    ![Podgląd aktualizacji bazy danych](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Utwórz stronę do wyświetlania i edytowania nowej kolumny

1. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **konta** kliknij folder w projekcie ContosoUniversity **Dodaj**, a następnie kliknij przycisk **nowy element** .
2. Utwórz nową **strona wzorcowa za pomocą formularza sieci Web** i nadaj mu nazwę *UserInfo.aspx*. Zaakceptuj wartość domyślną *Site.Master* pliku strony wzorcowej.
3. Skopiuj następujący kod do `MainContent` `Content` — element (ostatnie 3 `Content` elementy):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Kliknij prawym przyciskiem myszy *UserInfo.aspx* strony, a następnie kliknij przycisk **Pokaż w przeglądarce**.
5. Zaloguj się przy użyciu usługi *administratora* poświadczeń użytkownika (hasło jest *devpwd*) i Dodaj niektóre komentarze do użytkownika, aby sprawdzić, czy strona działa poprawnie.

    ![Strona informacje o użytkowniku](deploying-a-database-update/_static/image6.png)
6. Zamknij przeglądarkę.

## <a name="deploy-the-database-update"></a>Wdrażanie aktualizacji bazy danych

Aby wdrożyć przy użyciu dostawcy dbDacFx, wystarczy wybrać **Aktualizuj bazę danych** opcję w profilu publikowania. Jednak do początkowego wdrożenia przypadku użycia tej opcji możesz również skonfigurowana tak, niektóre dodatkowe skrypty SQL, aby uruchomić: są nadal w profilu i należy uniemożliwić ich ponowne uruchomienie.

1. Otwórz **publikowanie w sieci Web** kreatora, kliknij prawym przyciskiem myszy projekt ContosoUniversity i klikając **Publikuj**.
2. Wybierz **testu** profilu.
3. Kliknij przycisk **ustawienia** kartę.
4. W obszarze **DefaultConnection**, wybierz opcję **Aktualizuj bazę danych**.
5. Wyłącz dodatkowe skrypty, które skonfigurowano do uruchamiania do początkowego wdrożenia:

    1. Kliknij przycisk **Konfiguruj aktualizacje bazy danych**.
    2. W **Konfiguruj aktualizacje bazy danych** okno dialogowe, usuń zaznaczenie pola wyboru obok pozycji *Grant.sql* i *aspnet-data-dev.sql*.
    3. Kliknij przycisk **Zamknij**.
6. Kliknij przycisk **Podgląd** kartę.
7. W obszarze **baz danych** i po prawej stronie **DefaultConnection**, kliknij przycisk **bazy danych w wersji zapoznawczej** łącza.

    ![Podgląd bazy danych](deploying-a-database-update/_static/image7.png)

    Okno podglądu zawiera skrypt, który zostanie wykonany w docelowej bazie danych, że schemat bazy danych, które pasuje do schematu źródłowej bazy danych. Skrypt zawiera polecenia ALTER TABLE, który dodaje nową kolumnę.
8. Zamknij **bazy danych w wersji zapoznawczej** okno dialogowe, a następnie kliknij przycisk **Publikuj**.

    Program Visual Studio wdroży zaktualizowaną aplikację i otwarciu przeglądarki do strony głównej.
9. Uruchom na stronie informacje o użytkowniku (Dodaj *Account/UserInfo.aspx* na adres URL strony głównej) aby sprawdzić, czy aktualizacja została wdrożona pomyślnie. Musisz zalogować się, wprowadzając *administratora* i *devpwd*.

    Dane w tabelach nie są wdrażane domyślnie i nie skonfigurowała skryptu wdrażania danych, aby uruchomić, dzięki czemu nie odnajdzie komentarz, który dodaje w trakcie opracowywania. Możesz teraz dodać nowy komentarz na etapie wdrażania przejściowego, aby sprawdzić, czy zmiany zostały wdrożone względem bazy danych i strony działa prawidłowo.
10. Wykonaj tę samą procedurę, aby wdrożyć przejściowym i produkcyjnym.

    Należy pamiętać wyłączyć dodatkowe skrypty. Jedyną różnicą w porównaniu do profilu testowego jest czy spowoduje wyłączenie tylko jeden skrypt w tymczasowej i profile produkcyjnych, ponieważ zostały skonfigurowane do uruchamiania tylko *aspnet-prod-data.sql*.

    Poświadczenia dla środowisk przejściowych i produkcyjnych są prodpwd i administratora.

    Do rzeczywistej produkcji aktualizacji aplikacji, która obejmuje zmianę w bazie danych, przekazując także zazwyczaj zajęłoby aplikacji w trybie offline podczas wdrażania *aplikacji\_offline.htm* przed opublikowaniem i usuwając je później, jak przedstawiono w [poprzedniego samouczka](deploying-a-code-update.md).

## <a name="summary"></a>Podsumowanie

Teraz udało Ci się wdrożyć aktualizację aplikacji, który się przy użyciu zarówno migracje Code First, jak i dostawcy dbDacFx zmianę w bazie danych.

![Strona Instruktorzy z datę urodzenia](deploying-a-database-update/_static/image8.png)

![Strona informacje o użytkowniku](deploying-a-database-update/_static/image9.png)

Następnym samouczku dowiesz się, jak wykonać wdrożenia przy użyciu wiersza polecenia.

> [!div class="step-by-step"]
> [Poprzednie](deploying-a-code-update.md)
> [dalej](command-line-deployment.md)
