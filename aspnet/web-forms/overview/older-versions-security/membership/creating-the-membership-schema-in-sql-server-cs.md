---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
title: Tworzenie schematu członkostwa w programie SQL Server (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten samouczek rozpoczyna się przez badanie metod dodawania niezbędne schematu do bazy danych, aby można było używać SqlMembershipProvider. Poniżej firma Microsoft wi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: b4ac129d-1b8e-41ca-a38f-9b19d7c7bb0e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
msc.type: authoredcontent
ms.openlocfilehash: e1aec1ffbb052901fe34389943b5bb1db58348a9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400045"
---
<a name="creating-the-membership-schema-in-sql-server-c"></a>Tworzenie schematu członkostwa w programie SQL Server (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_CS.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_cs.pdf)

> Ten samouczek rozpoczyna się przez badanie metod dodawania niezbędne schematu do bazy danych, aby można było używać SqlMembershipProvider. Poniżej firma Microsoft zbadać klucza tabel w schemacie i omówić, ich przeznaczenia i ważności. W tym samouczku kończy się jak sprawdzić, który dostawca należy używać w ramach członkostwa aplikacji ASP.NET.


## <a name="introduction"></a>Wprowadzenie

Poprzednich samouczkach dwa badania, aby zidentyfikować osoby odwiedzające witrynę sieci Web za pomocą uwierzytelniania formularzy. Środowiska uwierzytelniania formularzy ułatwia deweloperom do zalogowania się użytkownika do witryny sieci Web i zapamiętasz między odwiedzin strony za pomocą biletów uwierzytelniania. `FormsAuthentication` Klasa zawiera metody do generowania--ticket i dodanie go do plików cookie przez obiekt odwiedzający. `FormsAuthenticationModule` Sprawdza wszystkie żądania przychodzące, a osobom prawidłowego biletu, tworzy i kojarzy `GenericPrincipal` i `FormsIdentity` obiekt z bieżącego żądania. Uwierzytelnianie formularzy jest jedynie mechanizm do przyznania biletu uwierzytelniania obiekt odwiedzający, podczas logowania się w, a kolejne żądania, analizowania tego biletu, aby ustalić tożsamość użytkownika. Dla aplikacji sieci web do obsługi kont użytkowników nadal musimy zaimplementować magazyn użytkownika i dodawania funkcjonalności do walidacji poświadczenia, rejestrowanie nowych użytkowników i wiele innych zadań związanych z kontem użytkownika.

Przed ASP.NET 2.0 deweloperzy były na punkt zaczepienia wykonywania wszystkich zadań związanych z kontem użytkownika. Na szczęście zespół programu ASP.NET rozpoznaje tego braku i wprowadzone w ramach członkostwa ASP.NET 2.0. W ramach członkostwa jest zestaw klas .NET Framework, które zapewniają interfejs programistyczny dla wykonywania zadań związanych z kontem użytkownika core. Ta struktura jest kompilowanych [modelu dostawca](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), które umożliwia deweloperom Podłącz implementacji dostosowane do standardowych interfejsów API.

Zgodnie z opisem w <a id="Tutorial1"> </a> [ *podstawy zabezpieczeń i Obsługa platformy ASP.NET* ](../introduction/security-basics-and-asp-net-support-cs.md) samouczek, .NET Framework jest dostarczany z dwóch wbudowanych dostawców członkostwa: [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) i [ `SqlMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Jak sugeruje jej nazwa, `SqlMembershipProvider` korzysta z bazy danych programu Microsoft SQL Server do przechowywania użytkownika. Aby korzystać z tego dostawcę w aplikacji, dlatego trzeba poinformować dostawcę z jakiej bazy danych do użycia jako magazyn. Oczywiście, `SqlMembershipProvider` oczekuje, że bazy danych magazynu użytkownika, aby niektórych tabel bazy danych, widoków i procedur składowanych. Musimy dodać tego Oczekiwano schematu z wybraną bazą danych.

Ten samouczek rozpoczyna od zbadania techniki Dodawanie niezbędne schematu do bazy danych, aby można było używać `SqlMembershipProvider`. Poniżej firma Microsoft zbadać klucza tabel w schemacie i omówić, ich przeznaczenia i ważności. W tym samouczku kończy się jak sprawdzić, który dostawca należy używać w ramach członkostwa aplikacji ASP.NET.

Zaczynajmy!

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Krok 1: Decydowanie o miejscu Store użytkownika

Dane aplikacji ASP.NET często są przechowywane w wielu tabel w bazie danych. Podczas implementowania `SqlMembershipProvider` schemat bazy danych, które firma Microsoft należy zdecydować, czy umieszczać schematu członkostwa w tej samej bazy danych jako dane aplikacji lub alternatywnej bazy danych.

Zalecam Lokalizowanie schematu członkostwa w tej samej bazy danych jako dane aplikacji z następujących powodów:

- **Łatwość konserwacji** aplikację, której dane są hermetyzowane w jednej bazie danych jest łatwiejsze do zrozumienia, obsługa i wdrażania niż aplikacja, która ma dwie osobne bazy danych.
- **Relacyjna integralność** , znajdując tabele powiązane członkostwa w tej samej bazy danych, jak aplikacja tabele go jest możliwe ustalenie [ograniczenia klucza obcego](http://en.wikipedia.org/wiki/Foreign_key) między kluczy podstawowych w Tabele powiązane członkostwa i tabel powiązanych aplikacji.

Oddzielenie danych użytkownika sklepu i aplikacji do osobnych baz danych tylko sens, jeśli masz wiele aplikacji każdego użycia osobnych baz danych, ale musisz udostępniać wspólnym magazynie użytkownika.

### <a name="creating-a-database"></a>Tworzenie bazy danych

Aplikacji, które tworzymy w drugim samouczku — od bazy danych nie ma jeszcze potrzebne. Potrzebujemy teraz jednak magazynu użytkowników. Teraz utwórz ją, a następnie dodaj do niego schemat wymagany przez `SqlMembershipProvider` dostawcy (zobacz krok 2).

> [!NOTE]
> W tej serii samouczków użyjemy [programu Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) bazy danych do przechowywania tabel naszej aplikacji i `SqlMembershipProvider` schematu. Tę decyzję podjęliśmy dwóch powodów: po pierwsze, ze względu na jej koszt — wersja bezpłatna — Express Edition jest najbardziej readably dostępną wersję programu SQL Server 2005; Po drugie, baz danych programu SQL Server 2005 Express Edition, można umieścić bezpośrednio w aplikacji sieci web `App_Data` folderu, dzięki czemu z łatwością pakietu bazy danych i aplikacji sieci web ze sobą w jednym pliku ZIP i wdrażając go ponownie bez wszelkie instrukcje specjalne ustawienia lub opcje konfiguracji. Jeśli wolisz samodzielnie, przy użyciu innego niż Express Edition w wersji programu SQL Server, możesz ją. Kroki są prawie identyczne. `SqlMembershipProvider` Schematu będzie pracować z dowolnej wersji programu Microsoft SQL Server 2000 i konfigurowanie.


W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy `App_Data` folder i wybierz opcję Dodaj nowy element. (Jeśli nie widzisz `App_Data` folder w projekcie, kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, wybierz opcję Dodaj Folder programu ASP.NET i wybierz `App_Data`.) W oknie dialogowym Dodaj nowy element, wybierz polecenie do dodania nowej bazy danych SQL o nazwie `SecurityTutorials.mdf`. W tym samouczku dodamy `SqlMembershipProvider` schematu do tej bazy danych; w kolejnych samouczkach zostaną utworzone dodatkowe tabele do przechwytywania danych aplikacji.


[![Dodawanie nowej bazy danych SQL o nazwie SecurityTutorials.mdf bazy danych w folderze App_Data](creating-the-membership-schema-in-sql-server-cs/_static/image2.png)](creating-the-membership-schema-in-sql-server-cs/_static/image1.png)

**Rysunek 1**: Dodawanie nowej bazy danych SQL, o nazwie `SecurityTutorials.mdf` bazy danych do `App_Data` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image3.png))


Dodawanie bazy danych do `App_Data` folderu automatycznie dołącza go w widoku Eksplorator bazy danych. (W wersji niż Express Edition programu Visual Studio, Eksplorator bazy danych jest nazywane Eksploratora serwera). Przejdź do Eksploratora bazy danych i rozwiń, po prostu dodane `SecurityTutorials` bazy danych. Jeśli nie widzisz Eksplorator bazy danych na ekranie, przejdź do menu Widok wybierz Eksplorator bazy danych i naciśnij klawisze Ctrl + Alt + S. Jak pokazano na rysunku 2, `SecurityTutorials` baza danych jest pusta — zawiera on żadnych tabel, nie widoków i nie procedur składowanych.


[![Baza danych SecurityTutorials jest obecnie pusta](creating-the-membership-schema-in-sql-server-cs/_static/image5.png)](creating-the-membership-schema-in-sql-server-cs/_static/image4.png)

**Rysunek 2**: `SecurityTutorials` baza danych jest obecnie pusta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Krok 2. Dodawanie`SqlMembershipProvider`schematu do bazy danych

`SqlMembershipProvider` Wymaga określonego zestawu tabel, widoków i procedur składowanych, ma być zainstalowany w bazie danych magazynu użytkownika. Te obiekty wymagania bazy danych można dodać za pomocą [ `aspnet_regsql.exe` narzędzie](https://msdn.microsoft.com/library/ms229862.aspx). Ten plik znajduje się w `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\` folderu.

> [!NOTE]
> `aspnet_regsql.exe` Narzędzie oferuje funkcje wiersza polecenia i graficzny interfejs użytkownika. Interfejs graficzny jest bardziej przyjazny dla użytkownika i co zostanie omówiony w tym samouczku. Interfejs wiersza polecenia jest przydatne, gdy dodanie `SqlMembershipProvider` schemat musi być zautomatyzowane, takich jak kompilacja skrypty lub zautomatyzowane scenariuszy testowania.


`aspnet_regsql.exe` Narzędzie służy do dodawania lub usuwania *usług aplikacji ASP.NET* określonej bazy danych SQL Server. Usługi aplikacji ASP.NET uwzględniający schematów dla `SqlMembershipProvider` i `SqlRoleProvider`, wraz z schematów dla dostawcy oparci na usłudze SQL dla innych platform ASP.NET 2.0. Należy podać dwa bity informacji `aspnet_regsql.exe` narzędzie:

- Czy chcesz dodać lub usunąć usługi aplikacji i
- Bazy danych, z którego można dodawać lub usuwać schemat usług aplikacji

W monitowania dla bazy danych do użycia, `aspnet_regsql.exe` narzędzie prosi nam podać nazwę serwera bazy danych znajduje się na poświadczenia bezpieczeństwa dla łączenia z bazą danych i nazwę bazy danych. Jeśli używasz innych niż Express Edition programu SQL Server, użytkownik powinien już wiedzieć tych informacji, ponieważ te same informacje, które należy podać przy użyciu parametrów połączenia podczas pracy z bazą danych za pośrednictwem strony sieci web ASP.NET. Określanie nazwy serwera i bazy danych, korzystając z bazy danych programu SQL Server 2005 Express Edition w `App_Data` folderu, jest jednak nieco bardziej skomplikowane.

Następujące części są rozpatrywane prosty sposób określania nazwy serwera i bazy danych dla bazy danych programu SQL Server 2005 Express Edition w `App_Data` folderu. Jeśli nie używasz programu SQL Server 2005 Express Edition możesz przejść od razu do Instalowanie sekcji usługi aplikacji.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>Określanie serwera i nazwę bazy danych programu SQL Server 2005 Express Edition bazy danych w`App_Data`folderu

Aby można było używać `aspnet_regsql.exe` narzędzie, które należy znać nazwy serwera i bazy danych. Nazwa serwera jest `localhost\InstanceName`. Najbardziej prawdopodobną *InstanceName* jest `SQLExpress`. Jednakże jeśli został zainstalowany ręcznie program SQL Server 2005 Express Edition (oznacza to, że nie zainstalowano go automatycznie podczas instalowania programu Visual Studio), jest możliwe, że wybrana nazwa innego wystąpienia.

Nazwa bazy danych jest nieco trudniejszy do określenia. Bazy danych w `App_Data` folderu zwykle mają nazwę bazy danych, która obejmuje [Unikatowy identyfikator globalny](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) oraz ścieżkę do pliku bazy danych. Należy określić to nazwa bazy danych, aby można było dodać schematu usług aplikacji za pomocą `aspnet_regsql.exe`.

Najprostszym sposobem ustalenia, nazwa bazy danych jest zbadanie programu SQL Server Management Studio. SQL Server Management Studio udostępnia interfejs graficzny do zarządzania bazami danych programu SQL Server 2005, ale nie jest dostarczany z Express Edition dla programu SQL Server 2005. Dobra wiadomość jest fakt, że [można pobrać](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) bezpłatne Express Edition programu SQL Server Management Studio.

> [!NOTE]
> Jeśli masz inne niż Express Edition wersję SQL Server 2005 zainstalowanego na pulpicie pełnej wersji programu Management Studio prawdopodobnie zostanie zainstalowana. Można ustalić nazwy bazy danych, wykonując czynności przedstawione poniżej Express Edition, można użyć pełnej wersji.


Rozpocznij od zamknięcia programu Visual Studio, aby upewnić się, że wszystkie blokady nałożone przez program Visual Studio w pliku bazy danych są zamknięte. Następnie uruchom program SQL Server Management Studio i połącz się z `localhost\InstanceName` bazy danych dla programu SQL Server 2005 Express Edition. Jak wspomniano wcześniej, jest szansa, nazwa wystąpienia jest `SQLExpress`. Dla opcji uwierzytelniania wybierz uwierzytelnianie Windows.


[![Nawiązać połączenie z wystąpieniem programu SQL Server 2005 Express Edition](creating-the-membership-schema-in-sql-server-cs/_static/image8.png)](creating-the-membership-schema-in-sql-server-cs/_static/image7.png)

**Rysunek 3**: nawiązać połączenie z wystąpieniem programu SQL Server 2005 Express Edition ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image9.png))


Po nawiązaniu połączenia z wystąpieniem programu SQL Server 2005 Express Edition Management Studio Wyświetla folderów dla baz danych, ustawienia zabezpieczeń, obiekty serwera i tak dalej. Po rozwinięciu pola na karcie bazy danych będzie zobaczysz, że `SecurityTutorials.mdf` baza danych jest *nie* zarejestrowana w wystąpieniu bazy danych — musimy najpierw dołączyć bazy danych.

Kliknij prawym przyciskiem myszy na folder baz danych, a następnie z menu kontekstowego wybierz polecenie Attach. Spowoduje to wyświetlenie okna dialogowego Dołączanie bazy danych. W tym miejscu, kliknij przycisk Dodaj, przejdź do `SecurityTutorials.mdf` bazy danych, a następnie kliknij przycisk OK. Rysunek 4 przedstawia okno dialogowe dołączanie bazy danych po `SecurityTutorials.mdf` baza danych została wybrana. Rysunek 5. pokazuje Eksploratora obiektów programu Management Studio po pomyślnie dołączyć bazy danych.


[![Dołącz SecurityTutorials.mdf bazę danych](creating-the-membership-schema-in-sql-server-cs/_static/image11.png)](creating-the-membership-schema-in-sql-server-cs/_static/image10.png)

**Rysunek 4**: Dołącz `SecurityTutorials.mdf` bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image12.png))


[![Baza danych SecurityTutorials.mdf pojawia się w folderze baz danych](creating-the-membership-schema-in-sql-server-cs/_static/image14.png)](creating-the-membership-schema-in-sql-server-cs/_static/image13.png)

**Rysunek 5**: `SecurityTutorials.mdf` baza danych pojawia się w folderze bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image15.png))


Jak pokazano na rysunku 5, `SecurityTutorials.mdf` bazy danych o nazwie zamiast abstruse. Teraz zmieniać, aby łatwiej zapamiętać (i łatwiejsze do typu) nazwa. Kliknij prawym przyciskiem myszy w bazie danych, z menu kontekstowego wybierz polecenie Zmień nazwę i zmień jego nazwę `SecurityTutorialsDatabase`. Nie ma to wpływu na nazwę pliku, po prostu nazwę bazy danych używa do identyfikacji do programu SQL Server.


[![Zmień nazwę bazy danych na SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-cs/_static/image17.png)](creating-the-membership-schema-in-sql-server-cs/_static/image16.png)

**Rysunek 6**: zmiana nazwy bazy danych do `SecurityTutorialsDatabase`([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image18.png))


W tym momencie wiemy, że nazwy serwera i bazy danych dla `SecurityTutorials.mdf` plik bazy danych: `localhost\InstanceName` i `SecurityTutorialsDatabase`, odpowiednio. Firma Microsoft są teraz gotowe do zainstalowania usług aplikacji za pośrednictwem `aspnet_regsql.exe` narzędzia.

### <a name="installing-the-application-services"></a>Instalowanie usługi aplikacji

Aby uruchomić `aspnet_regsql.exe` narzędzie, przejdź do start menu i wybierz polecenie Uruchom. Wprowadź `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` w polu tekstowym i kliknij przycisk OK. Alternatywnie można użyć Eksploratora Windows można przejść do odpowiedniego folderu, a następnie kliknij dwukrotnie ikonę `aspnet_regsql.exe` pliku. Każda z tych metod będzie net takie same wyniki.

Uruchamianie `aspnet_regsql.exe` narzędzia bez żadnych argumentów wiersza polecenia uruchamia Kreatora instalacji serwera SQL programu ASP.NET graficznego interfejsu użytkownika. Kreator ułatwia dodawanie lub usuwanie usług aplikacji ASP.NET w określonej bazie danych. Na pierwszym ekranie kreatora, pokazano na rysunku 7 opisuje przeznaczenie tego narzędzia.


[![Użyj udostępnia Kreatora instalacji serwera SQL programu ASP.NET, aby dodać schematu członkostwa](creating-the-membership-schema-in-sql-server-cs/_static/image20.png)](creating-the-membership-schema-in-sql-server-cs/_static/image19.png)

**Rysunek 7**: Użyj ASP.NET programu SQL Server Instalator Kreatora sprawia, że można dodać schematu członkostwa ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image21.png))


Drugi etap w Kreatorze pyta, czy nam czy chcemy dodać usługi aplikacji lub usuń je. Ponieważ chcemy dodać tabel, widoków i procedur składowanych, które są niezbędne do `SqlMembershipProvider`, wybierz pozycję Konfiguruj serwer SQL dla opcji usługi aplikacji. Później Jeśli chcesz usunąć ten schemat z bazy danych, uruchom ponownie tego kreatora, ale zamiast tego wybierz informacje o usługach aplikacji Usuń z istniejących opcji bazy danych.


[![Wybierz opcję konfigurowania programu SQL Server dla opcji usługi aplikacji](creating-the-membership-schema-in-sql-server-cs/_static/image23.png)](creating-the-membership-schema-in-sql-server-cs/_static/image22.png)

**Rysunek 8**: dla opcji usługi aplikacji wybierz pozycję Konfiguruj serwer SQL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image24.png))


Trzeci krok monituje o podanie informacji o bazie danych: Nazwa serwera, informacje o uwierzytelnianiu i nazwę bazy danych. Jeśli zostały następujące wraz z tego samouczka i dodano `SecurityTutorials.mdf` bazy danych do `App_Data`, dołączone do `localhost\InstanceName`, zmienić jego nazwę i `SecurityTutorialsDatabase`, następnie użyj następujących wartości:

- Serwer: `localhost\InstanceName`
- Uwierzytelnianie systemu Windows
- Baza danych: `SecurityTutorialsDatabase`


[![Wprowadź informacje o bazie danych](creating-the-membership-schema-in-sql-server-cs/_static/image26.png)](creating-the-membership-schema-in-sql-server-cs/_static/image25.png)

**Rysunek 9**: Wprowadź informacje o bazie danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image27.png))


Po wprowadzeniu informacji o bazie danych, kliknij przycisk Dalej. Ostatnim krokiem zawiera podsumowanie czynności, które zostaną wykonane. Kliknij przycisk Dalej, aby zainstalować usługi aplikacji, a następnie Zakończ, aby zakończyć działanie kreatora.

> [!NOTE]
> Jeśli używasz programu Management Studio można dołączyć bazy danych, a następnie zmień nazwę pliku bazy danych, pamiętaj odłączyć bazę danych, a następnie zamknij program Management Studio przed ponownym otwarciu programu Visual Studio. Aby odłączyć `SecurityTutorialsDatabase` bazy danych, kliknij prawym przyciskiem myszy nazwę bazy danych i z menu zadań wybierz odłączania.


Po zakończeniu działania kreatora wróć do programu Visual Studio, a następnie przejdź do Eksploratora bazy danych. Rozwiń folder tabel. Powinien zostać wyświetlony serii tabel, których nazwy rozpoczynają się prefiksem `aspnet_`. Podobnie można znaleźć różnych widoków i procedur składowanych w folderach widoków i procedur składowanych. Te obiekty bazy danych tworzą schemat usług aplikacji. Będziemy sprawdzać obiekty bazy danych członkostwa i ról określonych w kroku 3.


[![Dodano różnych tabel, widoków i procedur składowanych w bazie danych](creating-the-membership-schema-in-sql-server-cs/_static/image29.png)](creating-the-membership-schema-in-sql-server-cs/_static/image28.png)

**Na rysunku nr 10**: wiele tabel, widoków i przechowywane procedury zostały dodane do bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image30.png))


> [!NOTE]
> `aspnet_regsql.exe` Narzędzia graficznego interfejsu użytkownika instaluje schemat usług całej aplikacji. Jednak podczas wykonywania `aspnet_regsql.exe` z wiersza polecenia można określić, jakie określonej aplikacji usług składników, aby zainstalować (lub usunąć). W związku z tym, jeśli chcesz dodać tylko tabele, widoki i przechowywane procedury niezbędne dla `SqlMembershipProvider` i `SqlRoleProvider` dostawców, uruchamianie `aspnet_regsql.exe` z wiersza polecenia. Alternatywnie, możesz ręcznie uruchomić odpowiednie podzbiór języka T-SQL tworzenia skryptów używanych przez `aspnet_regsql.exe`. Skrypty te znajdują się w `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` folderu z nazwami, takich jak `InstallCommon.sql`,`InstallMembership.sql`,`InstallRoles.sql`, `InstallProfile.sql`,`InstallSqlState.sql`i tak dalej.


W tym momencie zostały utworzone obiekty bazy danych wymagane przez `SqlMembershipProvider`. Jednak nadal trzeba wydać polecenie framework członkostwa, należy użyć `SqlMembershipProvider` (versus, powiedz, `ActiveDirectoryMembershipProvider`) oraz że `SqlMembershipProvider` należy używać `SecurityTutorials` bazy danych. Omówimy sposób określić, jakie dostawca do użycia oraz dostosować ustawienia wybranego dostawcy w kroku 4. Jednak najpierw Przyjrzyjmy się bardziej na obiekty bazy danych, które właśnie zostały utworzone.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Krok 3: Przyjrzeć się tabele podstawowe schematu

Podczas pracy z platformami członkostwo i role w aplikacji ASP.NET, szczegóły implementacji są hermetyzowane przez dostawcę. W przyszłości samouczków firma Microsoft będzie współpracować z tych platform za pomocą programu .NET Framework `Membership` i `Roles` klasy. Korzystając z tych interfejsy API wysokiego poziomu nie musimy dotyczą osoby za pomocą szczegóły niskiego poziomu, takich jak zapytania, które są wykonywane lub tabele są modyfikowane przez `SqlMembershipProvider` i `SqlRoleProvider`.

Biorąc pod uwagę to, można bezpiecznie używamy struktury członkostwa i ról bez konieczności zbadano schemat bazy danych utworzone w kroku 2. Jednak podczas tworzenia tabel do przechowywania danych aplikacji firma Microsoft może być konieczne utworzenie jednostek, które odnoszą się do użytkowników lub ról. Warto mieć znajomość `SqlMembershipProvider` i `SqlRoleProvider` schematów podczas ustanawiania obcego klucza ograniczenia między tabelami danych aplikacji i tych tabel, utworzony w kroku 2. Ponadto w pewnych sytuacjach szczególnych firma Microsoft może być konieczne interfejsu użytkownika i roli są przechowywane bezpośrednio na poziomie bazy danych (a nie poprzez `Membership` lub `Roles` klasy).

### <a name="partitioning-the-user-store-into-applications"></a>Partycjonowanie Store użytkownika do aplikacji

Struktury członkostwa i ról zostały zaprojektowane w taki sposób, że jeden magazyn użytkownika i roli mogą być współużytkowane przez wiele aplikacji. Aplikacji ASP.NET, który używa struktury członkostwa lub ról, należy określić partycji aplikacji do użycia. Krótko mówiąc wielu aplikacji sieci web można użyć tego samego magazynów użytkownika i roli. Rysunek 11 przedstawia magazynów użytkownika i roli, które są partycjonowane na trzy aplikacje: HRSite CustomerSite i SalesSite. Te trzy aplikacje sieci web każdy mają własne unikatowych użytkowników i ról, ale wszystkie fizycznie przechowują swoje konto i roli informacje o użytkowniku w tych samych tabelach bazy danych.


[![Konta użytkowników mogą być podzielone na partycje dla wielu aplikacji](creating-the-membership-schema-in-sql-server-cs/_static/image32.png)](creating-the-membership-schema-in-sql-server-cs/_static/image31.png)

**Rysunek 11**: użytkownika konta może być podzielona na partycje w wielu aplikacjach ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-cs/_static/image33.png))


`aspnet_Applications` Tabela jest definiuje tych partycji. Każdej aplikacji korzystającej z bazy danych do przechowywania informacji o koncie użytkownika jest reprezentowany przez wiersza w tej tabeli. `aspnet_Applications` Tabela ma cztery kolumny: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`, i `Description`. `ApplicationId` Typ jest [ `uniqueidentifier` ](https://msdn.microsoft.com/library/ms187942.aspx) i klucz podstawowy tabeli; `ApplicationName` zapewnia unikatową nazwę przyjaznego dla człowieka dla każdej aplikacji.

Inne tabele powiązane członkostwa i ról prowadzą z powrotem do `ApplicationId` pole `aspnet_Applications`. Na przykład `aspnet_Users` tabeli, która zawiera rekord dla każdego konta użytkownika, ma `ApplicationId` pola klucza obcego; jw. Aby uzyskać `aspnet_Roles` tabeli. `ApplicationId` Partycji aplikacji Określa pole w tych tabelach, konto użytkownika lub rola.

### <a name="storing-user-account-information"></a>Przechowywanie informacji o koncie użytkownika

Informacje o koncie użytkownika są przechowywane w dwóch tabelach: `aspnet_Users` i `aspnet_Membership`. `aspnet_Users` Tabela zawiera pola, których przechowywane są informacje o kontach użytkowników podstawowych. Trzy kolumny najbardziej potrzebne są:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` jest to klucz podstawowy (i typu `uniqueidentifier`). `UserName` Typ jest `nvarchar(256)` i wraz z hasłem, tworzy poświadczenia użytkownika. (Hasła użytkownika są przechowywane w `aspnet_Membership` tabeli.) `ApplicationId` łączy konto użytkownika do konkretnej aplikacji w `aspnet_Applications`. Brak złożonego [ `UNIQUE` ograniczenie](https://msdn.microsoft.com/library/ms191166.aspx) na `UserName` i `ApplicationId` kolumn. Daje to gwarancję, że w danej aplikacji jest unikatowa nazwa każdego użytkownika, ale daje takie same `UserName` ma być używany w różnych aplikacjach.

`aspnet_Membership` Tabela zawiera dodatkowe informacje, takie jak hasło użytkownika, adres e-mail, ostatnie logowania daty i godziny oraz itd. Ma relację między rekordami w `aspnet_Users` i `aspnet_Membership` tabel. Ta relacja jest zapewniana przez `UserId` pole `aspnet_Membership`, który służy jako klucz podstawowy tabeli. Podobnie jak `aspnet_Users` tabeli `aspnet_Membership` obejmuje `ApplicationId` pola, która tych informacji do określonej partycji aplikacji.

### <a name="securing-passwords"></a>Zabezpieczanie haseł

Informacje dotyczące hasła są przechowywane w `aspnet_Membership` tabeli. `SqlMembershipProvider` Umożliwia haseł, które mają być przechowywane w bazie danych przy użyciu jednej z trzech następujących technik:

- **Wyczyść** — hasło jest przechowywane w bazie danych w postaci zwykłego tekstu. I zdecydowanie odradzamy przy użyciu tej opcji. Jeśli baza danych zostanie naruszony — są to przez hakera, który umożliwia znalezienie drzwi wstecz lub niezadowoleni pracownika, który ma dostęp do bazy danych — poświadczenia każdego pojedynczego użytkownika czy istnieją pobierania.
- **Mieszana** -hasła mają formę skrótu, za pomocą jednokierunkowego algorytmu skrótu i generowanej losowo wartości zaburzającej. Ta wartość skrótu (wraz z ziarna) są przechowywane w bazie danych.
- **Szyfrowane** -zaszyfrowana wersja hasła są przechowywane w bazie danych.

Hasło magazynu zastosowanych jest zależna od `SqlMembershipProvider` ustawienia określone w `Web.config`. Przyjrzymy się dostosowywanie `SqlMembershipProvider` ustawień w kroku 4. Zachowanie domyślne jest do przechowywania skrótów haseł.

Są zobowiązani do przechowywania hasła kolumny `Password`, `PasswordFormat`, i `PasswordSalt`. `PasswordFormat` To pole typu `int` techniki używane do przechowywania hasła wskazuje, którego wartość: 0 w przypadku zwykłego; 1 Hashed 2-zaszyfrowane. `PasswordSalt` przypisano losowo generowany ciągu, niezależnie od tego hasła magazynu zastosowanych; wartość `PasswordSalt` jest używana tylko podczas obliczania skrótu hasła. Na koniec `Password` kolumna zawiera dane rzeczywiste hasło, należy go hasła jako zwykły tekst, skrót hasła lub zaszyfrowane hasło.

Tabela 1 przedstawiono te trzy kolumny jak może wyglądać dla różnych technik przetwarzania magazynu podczas zapisywania haseł MySecret! .

| **Technika magazynu&lt;\_o3a\_p /&gt;** | **Hasło&lt;\_o3a\_p /&gt;** | **PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| Usuń zaznaczenie | MySecret! | 0 | tTnkPlesqissc2y2SMEygA== |
| Mieszana | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1 | wFgjUfhdUFOCKQiI61vtiQ== |
| Szyfrowane | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**Tabela 1**: Przykładowe wartości pól skojarzone z hasłem w przypadku przechowywania MySecret hasło!

> [!NOTE]
> Algorytmu wyznaczania wartości skrótu używanego przez serwer lub określonej szyfrowania `SqlMembershipProvider` zależy od ustawienia w `<machineKey>` elementu. Omówiliśmy ten element konfiguracji w kroku 3 <a id="Tutorial3"> </a> [ *Konfiguracja uwierzytelniania formularzy i Tematy zaawansowane* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) samouczka.


### <a name="storing-roles-and-role-associations"></a>Przechowywanie ról i skojarzenia roli

W ramach ról umożliwia deweloperom Definiowanie zestawu ról i określić, jakie użytkownicy należą do jakich ról. Te informacje są przechwytywane w bazie danych za pośrednictwem dwóch tabel: `aspnet_Roles` i `aspnet_UsersInRoles`. Każdy rekord w `aspnet_Roles` tabeli reprezentuje rolę dla określonej aplikacji. Podobnie jak `aspnet_Users` tabeli `aspnet_Roles` tabela ma trzy kolumny dotyczących naszych dyskusji:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` jest to klucz podstawowy (i typu `uniqueidentifier`). `RoleName` Typ jest `nvarchar(256)`. I `ApplicationId` łączy konto użytkownika do konkretnej aplikacji w `aspnet_Applications`. Brak złożonego `UNIQUE` ograniczenie `RoleName` i `ApplicationId` kolumn, zapewnia, że każda nazwa roli w danej aplikacji jest unikatowy.

`aspnet_UsersInRoles` Tabeli służy jako mapowania między użytkownikami i rolami. Istnieją tylko dwie kolumny — `UserId` i `RoleId` — a razem tworzą one złożony klucz podstawowy.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Krok 4: Określenie dostawcy i dostosowywanie jej ustawienia

Wszystkie strukturami, które obsługują modelu dostawca — takie jak struktury członkostwa i ról — Brak szczegółów implementacji, samodzielnie i zamiast tego przekazać to zadanie do klasy dostawcy. W przypadku członkostwa w ramach `Membership` klasa definiuje interfejs API do zarządzania kontami użytkowników, ale go nie bezpośrednią interakcję z dowolnego Sklepu użytkownika. Zamiast `Membership` żądanie skonfigurowany dostawca — Dostarcz metody tej klasy użyjemy `SqlMembershipProvider`. Gdy firma Microsoft wywołania jednej z metod w `Membership` klasy, jak framework członkostwa, aby delegować wywołanie `SqlMembershipProvider`?

`Membership` Klasa ma [ `Providers` właściwość](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) zawiera odwołanie do wszystkich klas zarejestrowanego dostawcy dostępne do użycia przez platformę, członkostwo. Każdy zarejestrowany dostawca ma skojarzone nazwy i typu. Nazwa oferuje sposób przyjaznego dla człowieka odwoływać się do określonego dostawcy w `Providers` kolekcji, gdy typ identyfikuje klasy dostawcy. Ponadto każdego zarejestrowanego dostawcy mogą obejmować ustawienia konfiguracji. Ustawienia konfiguracji dla struktury członkostwa obejmują `passwordFormat` i `requiresUniqueEmail`, wiele innych. Patrz tabela 2, aby uzyskać pełną listę ustawień konfiguracji używane przez `SqlMembershipProvider`.

`Providers` Zawartość właściwości są określone w ustawieniach konfiguracji aplikacji sieci web. Domyślnie wszystkie aplikacje sieci web mają dostawcę o nazwie `AspNetSqlMembershipProvider` typu `SqlMembershipProvider`. Domyślny dostawca członkostwa jest zarejestrowany w `machine.config` (znajdujący się w `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG)`:

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample1.xml)]

Jako kod znaczników powyżej przedstawiono [ `<membership>` elementu](https://msdn.microsoft.com/library/1b9hw62f.aspx) definiuje ustawienia konfiguracji dla struktury członkostwa, podczas [ `<providers>` element podrzędny](https://msdn.microsoft.com/library/6d4936ht.aspx) Określa zarejestrowaną dostawcy. Dostawcy mogą zostać dodane lub usunięte za pomocą [ `<add>` ](https://msdn.microsoft.com/library/whae3t94.aspx) lub [ `<remove>` ](https://msdn.microsoft.com/library/aykw9a6d.aspx) elementów; użyj [ `<clear>` ](https://msdn.microsoft.com/library/t062y6yc.aspx) elementu do usunięcia wszystkich aktualnie zarejestrowanych dostawców. Jako kod znaczników powyżej przedstawiono `machine.config` Dodaje dostawcę o nazwie `AspNetSqlMembershipProvider` typu `SqlMembershipProvider`.

Oprócz `name` i `type` atrybutów, `<add>` element zawiera atrybuty, które określają wartości dla różnych konfigurowania ustawień. Tabela 2 zawiera listę dostępnych `SqlMembershipProvider`-określonych ustawień konfiguracji, wraz z opisem każdego z nich.

> [!NOTE]
> Domyślne wartości podane w tabeli 2 odnoszą się do wartości domyślnych, zdefiniowane w `SqlMembershipProvider` klasy. Należy pamiętać, że nie wszystkie ustawienia konfiguracji w `AspNetSqlMembershipProvider` zgodne z wartościami domyślnymi `SqlMembershipProvider` klasy. Na przykład, jeśli nie określono dostawcy członkostwa `requiresUniqueEmail` ustawień domyślnych na wartość true. Jednak `AspNetSqlMembershipProvider` zastępuje tę wartość domyślną, jawnie ustawiając wartość `false`.


| **Ustawienie&lt;\_o3a\_p /&gt;** | **Opis&lt;\_o3a\_p /&gt;** |
| --- | --- |
| `ApplicationName` | Pamiętaj, że framework członkostwa pozwala magazynu jednego użytkownika do podzielenia na partycje dla wielu aplikacji. To ustawienie wskazuje nazwę partycji aplikacji używanej przez dostawcę członkostwa. Jeśli ta wartość nie jawnie określono, jest on ustawiony, w czasie wykonywania wartość ścieżka wirtualnego katalogu głównego aplikacji. |
| `commandTimeout` | Określa wartość limitu czasu polecenia SQL (w sekundach). Wartość domyślna to 30. |
| `connectionStringName` | Nazwa parametrów połączenia w `<connectionStrings>` elementu używanego do łączenia z bazą danych magazynu użytkownika. Ta wartość jest wymagana. |
| `description` | Zawiera opis przyjaznego dla człowieka zarejestrowanego dostawcy. |
| `enablePasswordRetrieval` | Określa, czy użytkownicy mogą pobrać zapomnianego hasła. Wartość domyślna to `false`. |
| `enablePasswordReset` | Wskazuje, czy użytkownicy mogą zresetować swoje hasło. Wartość domyślna to `true`. |
| `maxInvalidPasswordAttempts` | Maksymalną liczbę prób Nieudane logowanie, które mogą wystąpić dla danego użytkownika podczas określonego `passwordAttemptWindow` przed zablokowaniem użytkownika. Wartość domyślna to 5. |
| `minRequiredNonalphanumericCharacters` | Minimalna liczba znaków innych niż alfanumeryczne, które muszą być ujęte w hasła użytkownika. Ta wartość musi być z zakresu od 0 do 128; Wartość domyślna to 1. |
| `minRequiredPasswordLength` | Minimalna liczba znaków w haśle. Ta wartość musi być z zakresu od 0 do 128; Wartość domyślna to 7. |
| `name` | Nazwa zarejestrowanego dostawcy. Ta wartość jest wymagana. |
| `passwordAttemptWindow` | Liczba minut, podczas którego nieudanych prób są śledzone. Jeśli użytkownik poda nieprawidłowych poświadczeń logowania `maxInvalidPasswordAttempts` razy w ramach tej określony okna, są zablokowane. Wartość domyślna to 10. |
| `PasswordFormat` | Format przechowywania haseł: `Clear`, `Hashed`, lub `Encrypted`. Wartość domyślna to `Hashed`. |
| `passwordStrengthRegularExpression` | Jeśli nie dostarczono, to wyrażenie regularne jest używane do oceny siły hasła wybranego użytkownika podczas tworzenia nowego konta lub podczas zmiany hasła. Wartością domyślną jest ciąg pusty. |
| `requiresQuestionAndAnswer` | Określa, czy użytkownik musi odpowiedzieć na pytanie zabezpieczające, jego podczas pobierania lub resetowanie własnego hasła. Wartość domyślna to `true`. |
| `requiresUniqueEmail` | Wskazuje, czy wszystkie konta użytkowników w danej aplikacji partycji musi mieć unikatowego adresu e-mail. Wartość domyślna to `true`. |
| `type` | Określa typ dostawcy. Ta wartość jest wymagana. |

**Tabela 2**: członkostwo i `SqlMembershipProvider` ustawień konfiguracji

Oprócz `AspNetSqlMembershipProvider`, innych dostawców członkostwa mogą być zarejestrowane na podstawie aplikacji, aplikacji, dodając znaczników podobny do `Web.config` pliku.

> [!NOTE]
> W ramach ról działa w taki sam sposób: Brak domyślnego dostawcę roli zarejestrowanego w `machine.config` i zarejestrowanych dostawców można dostosować na podstawie aplikacji przez aplikację w `Web.config`. Będziemy sprawdzać framework ról i jego znaczników konfiguracji szczegółowo w przyszłości zapoznać się z samouczkiem.


### <a name="customizing-thesqlmembershipprovidersettings"></a>Dostosowywanie`SqlMembershipProvider`ustawienia

Wartość domyślna `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) ma jego `connectionStringName` ustawioną wartość atrybutu `LocalSqlServer`. Podobnie jak `AspNetSqlMembershipProvider` dostawcy, nazwa parametrów połączenia `LocalSqlServer` jest zdefiniowany w `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample2.xml)]

Jak widać, te parametry połączenia definiuje SQL 2005 Express Edition bazy danych znajdujące się u | DataDirectory|aspnetdb.mdf. Ciąg | DataDirectory | jest tłumaczony w czasie wykonywania, aby wskazywał `~/App_Data/` katalogu, więc ścieżka bazy danych | DataDirectory|aspnetdb.mdf"przekłada się na `~/App_Data` / `aspnet.mdf`.

Jeśli firma Microsoft nie określono żadnych informacji o dostawcy członkostwa w naszej aplikacji `Web.config` plik, aplikacja używa dostawcy członkostwa domyślna zarejestrowana, `AspNetSqlMembershipProvider`. Jeśli `~/App_Data/aspnet.mdf` bazy danych nie istnieje, środowisko uruchomieniowe ASP.NET utworzyć automatycznie i Dodaj schemat usług aplikacji. Jednak firma Microsoft nie chcesz używać `aspnet.mdf` bazy danych; zamiast chcemy do użycia `SecurityTutorials.mdf` bazy danych utworzonej w kroku 2. Ta modyfikacja można zrobić na dwa sposoby:

- <strong>Określ wartość dla</strong><strong>`LocalSqlServer`</strong><strong>nazwa parametrów połączenia w</strong><strong>`Web.config`</strong><strong>.</strong> Przez zastąpienie `LocalSqlServer` wartość Nazwa parametrów połączenia w `Web.config`, możemy użyć zarejestrowanych domyślnym dostawcą członkostwa (`AspNetSqlMembershipProvider`) i jest poprawnie pracować `SecurityTutorials.mdf` bazy danych. To podejście jest odpowiednie w przypadku zawartości przy użyciu ustawień konfiguracyjnych, określony przez `AspNetSqlMembershipProvider`. Aby uzyskać więcej informacji na temat tej techniki, zobacz [Scott Guthrie](https://weblogs.asp.net/scottgu/)firmy wpis w blogu [Konfigurowanie platformy ASP.NET 2.0 usługi aplikacji, użyj programu SQL Server 2000 lub SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Dodaj nowego dostawcę zarejestrowanego typu</strong><strong>`SqlMembershipProvider`</strong><strong>i konfigurowania jej</strong><strong>`connectionStringName`</strong><strong>ustawienie, aby wskazywał</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>bazy danych.</strong> Takie podejście jest przydatne w scenariuszach, w którym chcesz dostosować inne właściwości konfiguracji, oprócz parametry połączenia bazy danych. W projektach I zawsze używaj tego podejścia ze względu na swoją elastyczność i czytelności.

Zanim dodamy nowy zarejestrowanego dostawcy, który odwołuje się do `SecurityTutorials.mdf` bazy danych, najpierw musimy dodać wartość ciągu połączenia w `<connectionStrings>` sekcji `Web.config`. Następujący kod znaczników umożliwia dodanie nowych parametrów połączenia o nazwie `SecurityTutorialsConnectionString` odwołujący się program SQL Server 2005 Express Edition `SecurityTutorials.mdf` bazy danych w `App_Data` folderu.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample3.xml)]

> [!NOTE]
> Jeśli używasz pliku alternatywnej bazy danych, należy zaktualizować parametry połączenia, zgodnie z potrzebami. Aby uzyskać więcej informacji na temat stanowiący prawidłowych parametrów połączenia, zobacz [ConnectionStrings.com](http://www.connectionstrings.com/).

Następnie dodaj poniższe znaczników członkostwa w konfiguracji w celu `Web.config` pliku. Ten kod znaczników rejestruje nowego dostawcę o nazwie `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample4.xml)]

Oprócz rejestrowania `SecurityTutorialsSqlMembershipProvider` definiuje powyżej znaczników dostawcy `SecurityTutorialsSqlMembershipProvider` jako domyślnego dostawcę (za pośrednictwem `defaultProvider` atrybutu w `<membership>` elementu). Pamiętaj, że w ramach członkostwa mogą mieć wiele zarejestrowanych dostawców. Ponieważ `AspNetSqlMembershipProvider` jest zarejestrowany jako pierwszy dostawca w `machine.config`, chyba że wskazaliśmy, w przeciwnym razie służy jako domyślnego dostawcę.

Obecnie nasza aplikacja ma dwóch dostawców zarejestrowanych: `AspNetSqlMembershipProvider` i `SecurityTutorialsSqlMembershipProvider`. Jednak przed zarejestrowaniem `SecurityTutorialsSqlMembershipProvider` dostawcą, firma Microsoft może mieć wyczyszczone wszystkie wcześniej zarejestrowanego dostawcę, dodając [ `<clear />` elementu](https://msdn.microsoft.com/library/t062y6yc.aspx) bezpośrednio przed naszych `<add>` elementu. Spowoduje to wyczyszczenie `AspNetSqlMembershipProvider` z listy zarejestrowanych dostawców, co oznacza, że `SecurityTutorialsSqlMembershipProvider` będzie tylko zarejestrowanego dostawcy członkostwa. Jeśli użyto tego podejścia, a następnie firma Microsoft nie jest wymagane oznaczyć `SecurityTutorialsSqlMembershipProvider` jako domyślnego dostawcę, ponieważ jest tylko zarejestrowanego dostawcy członkostwa. Aby uzyskać więcej informacji na temat korzystania z `<clear />`, zobacz [Using `<clear />` podczas dodawania dostawcy](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Należy pamiętać, że `SecurityTutorialsSqlMembershipProvider`firmy `connectionStringName` ustawienie odwołania, po prostu dodane `SecurityTutorialsConnectionString` nazwa parametrów połączenia, a jego `applicationName` ustawieniu nadano wartość SecurityTutorials. Ponadto `requiresUniqueEmail` ustawiono ustawienie `true`. Inne opcje konfiguracji są identyczne do wartości w `AspNetSqlMembershipProvider`. Możesz wprowadzać żadnych zmian konfiguracji, w tym miejscu, w razie potrzeby. Na przykład można zwiększyć siły hasła, wymagając dwa znaki inne niż alfanumeryczne, zamiast jednego lub przez zwiększenie długości hasła do ośmiu znaków zamiast siedem.

> [!NOTE]
> Pamiętaj, że framework członkostwa pozwala magazynu jednego użytkownika do podzielenia na partycje dla wielu aplikacji. Dostawca członkostwa `applicationName` ustawienie wskazuje, dostawca używa podczas pracy z magazynu użytkowników aplikacji. Należy jawnie ustawić wartość `applicationName` ustawienia konfiguracji, ponieważ jeśli `applicationName` nie jest jawnie ustawiona, jest ona przypisana do ścieżka wirtualnego katalogu głównego aplikacji sieci web w czasie wykonywania. Działa to prawidłowo tak długo, jak ścieżka wirtualnego katalogu głównego aplikacji nie powoduje zmiany, ale przenieść aplikację na inną ścieżkę `applicationName` zbyt zmieni ustawienie. W takim przypadku Dostawca członkostwa rozpocznie się praca z partycji innej aplikacji, niż był wcześniej używany. Konta użytkowników utworzone przed przeniesienie będą znajdować się na partycji innej aplikacji i Ci użytkownicy już nie będzie można zalogować się do witryny. Aby uzyskać szczegółowe omówienie w tej sprawie, zobacz [zawsze wartość `applicationName` właściwości podczas konfigurowania programu ASP.NET 2.0 członkostwa i innych dostawców](https://weblogs.asp.net/scottgu/443634).


## <a name="summary"></a>Podsumowanie

W tym momencie mamy bazę danych przy użyciu usług aplikacji skonfigurowany (`SecurityTutorials.mdf`) i skonfigurowano naszej aplikacji sieci web, aby używał w ramach członkostwa `SecurityTutorialsSqlMembershipProvider` dostawcy, które właśnie zostało zarejestrowane. Jest to zarejestrowanego dostawcy typu `SqlMembershipProvider` i ma jego `connectionStringName` ustawiony na ciąg połączenia (`SecurityTutorialsConnectionString`) i jego `applicationName` jawnie ustawić wartość.

Firma Microsoft są teraz gotowe do użycia w ramach członkostwa z naszej aplikacji. W następnym samouczku zostanie omówiony sposób tworzenia nowych kont użytkowników. Po, że przeanalizujemy, uwierzytelnianie użytkowników, wykonywania autoryzacji opartej na użytkownika i przechowuje dodatkowe informacje dotyczące użytkowników w bazie danych.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Zawsze wartość `applicationName` właściwości podczas konfigurowania programu ASP.NET 2.0 członkostwa i innych dostawców](https://weblogs.asp.net/scottgu/443634)
- [Konfigurowanie programu ASP.NET 2.0 usługi aplikacji, użyj programu SQL Server 2000 lub SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Pobieranie programu SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Badanie programu ASP.NET 2.0 s członkostwo, role i profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [`<add>` Elementu dla dostawców członkostwa](https://msdn.microsoft.com/library/whae3t94.aspx)
- [`<membership>` — Element](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [`<providers>` Element członkostwa](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Za pomocą `<clear />` podczas dodawania dostawców](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Praca bezpośrednio z `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Szkolenie wideo na tematy zawarte w tym samouczku

- [Objaśnienie członkostwa platformy ASP.NET](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Konfigurowanie programu SQL do pracy ze schematami członkostwa](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Modyfikowanie ustawień członkostwa w domyślnym schemacie członkostwa](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>Informacje o autorze

Pracował nad Bento Scottem, autor wiele książek ASP/ASP.NET i założyciel 4GuysFromRolla.com, przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena  *[Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott można z Tobą skontaktować w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Alicja Maziarz. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Next](creating-user-accounts-cs.md)
