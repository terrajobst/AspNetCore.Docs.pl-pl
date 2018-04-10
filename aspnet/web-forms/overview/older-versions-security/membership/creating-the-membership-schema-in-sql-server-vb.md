---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
title: Tworzenie schematu członkostwa w programie SQL Server (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku rozpoczyna badanie techniki Dodawanie schematu niezbędne do bazy danych, aby można było używać SqlMembershipProvider. Po tym, że firma Microsoft wi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 112a674d-716f-41a6-99b8-4074d65a54c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 44458e8022f1f0d52cf136ad7fbaa5dd1f546632
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-the-membership-schema-in-sql-server-vb"></a>Tworzenie schematu członkostwa w programie SQL Server (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_VB.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_vb.pdf)

> W tym samouczku rozpoczyna badanie techniki Dodawanie schematu niezbędne do bazy danych, aby można było używać SqlMembershipProvider. Po tym, że firma Microsoft zbada klucza tabel w schemacie i omówienia ich przeznaczenia i znaczenie. W tym samouczku kończy przyjrzeć się jak stwierdzić, który dostawca należy używać w ramach członkostwa aplikacji ASP.NET.


## <a name="introduction"></a>Wprowadzenie

Poprzednich dwóch samouczki zbadane, aby zidentyfikować osoby odwiedzające witrynę sieci Web przy użyciu uwierzytelniania formularzy. Framework uwierzytelniania formularzy ułatwia deweloperom logowania użytkownika do witryny sieci Web i zapamiętać między wizyt strony przy użyciu biletów uwierzytelniania. `FormsAuthentication` Klasa zawiera metody do generowania bilet i dodanie go do plików cookie odwiedzającego. `FormsAuthenticationModule` Sprawdza wszystkie żądania przychodzące, dla osób z prawidłowego biletu, tworzy i kojarzy `GenericPrincipal` i `FormsIdentity` obiektu z bieżącego żądania. Uwierzytelnianie formularzy jest jedynie mechanizm przyznanie bilet uwierzytelnienia dla obiekt odwiedzający podczas logowania w, a kolejne żądania analizowania tego biletu, aby określić tożsamości użytkownika. Dla aplikacji sieci web do obsługi kont użytkowników nadal należy zaimplementować magazynu użytkowników oraz dodawać funkcje do walidacji poświadczenia, rejestrować nowych użytkowników i różnych innych zadań związanych z kontem użytkownika.

Przed składnika ASP.NET 2.0 deweloperzy były na haku wykonywania wszystkich zadań związanych z kontem użytkownika. Na szczęście zespołu ASP.NET rozpoznaje tego braku i wprowadzone w ramach członkostwa programu ASP.NET 2.0. W ramach członkostwa to zestaw klas w programie .NET Framework, które udostępniają interfejs programowych dotyczących wykonywanie zadań związanych z kontem użytkownika core. Ta struktura została stworzona [modelu dostawcy](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), który umożliwia deweloperom Podłącz implementacji dostosowane do standardowych interfejsu API.

Zgodnie z opisem w <a id="Tutorial1"> </a> [ *podstawowe informacje o zabezpieczeniach i obsługę ASP.NET* ](../introduction/security-basics-and-asp-net-support-vb.md) samouczek, .NET Framework jest dostarczany z dwóch wbudowanych dostawców członkostwa: [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) i [ `SqlMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Jak jego nazwa wskazuje, `SqlMembershipProvider` korzysta z bazy danych programu Microsoft SQL Server do przechowywania użytkownika. Aby można było używać tego dostawcę w aplikacji, należy sprawdzić, jakie bazy danych do użycia jako magazynu dostawcy. Oczywiście, `SqlMembershipProvider` oczekuje bazy danych magazynu użytkownika do określonych tabel bazy danych, widoków i procedur składowanych. Musimy dodać tego Oczekiwano schematu w wybranej bazie danych.

W tym samouczku rozpoczyna się znaleźć, sprawdzając techniki dodawania schematu niezbędne do bazy danych, aby można było używać `SqlMembershipProvider`. Po tym, że firma Microsoft zbada klucza tabel w schemacie i omówienia ich przeznaczenia i znaczenie. W tym samouczku kończy przyjrzeć się jak stwierdzić, który dostawca należy używać w ramach członkostwa aplikacji ASP.NET.

Dzieła!

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Krok 1: Podejmowania decyzji o lokalizację magazynu użytkowników

Dane aplikacji ASP.NET jest często przechowywane w liczbę tabel w bazie danych. Podczas implementowania `SqlMembershipProvider` schematu bazy danych, możemy należy zdecydować, czy można umieścić schematu członkostwo w tej samej bazy danych jako dane aplikacji lub alternatywnej bazy danych.

Najlepiej Lokalizowanie schematu członkostwo w tej samej bazy danych jako dane aplikacji z następujących powodów:

- **Możliwość utrzymania** aplikacji, którego dane są umieszczane w jednej bazy danych jest łatwiej zrozumieć, obsługę i wdrażanie niż aplikacja, która ma dwa oddzielne bazy danych.
- **Relacyjna integralność** dzięki umieszczeniu tabele powiązane członkostwo w tej samej bazy danych jako aplikacja tabele on jest możliwe ustalenie [ograniczeń klucza obcego](http://en.wikipedia.org/wiki/Foreign_key) między kluczy podstawowych w Tabele dotyczące członkostwa i tabel powiązanych aplikacji.

Rozdzielenie użytkownika aplikacji i magazynu danych do oddzielnego baz danych ma sens tylko w przypadku wielu aplikacji każdego używać oddzielnego baz danych, ale muszą udostępniać wspólnej magazynu użytkowników.

### <a name="creating-a-database"></a>Tworzenie bazy danych

Aplikację, która ma możemy tworzenia od drugiego samouczka nie ma jeszcze potrzebne bazy danych. Potrzebujemy, jednak magazynu użytkowników. Umożliwia utworzenie, a następnie dodaj do niej schematu wymagane przez `SqlMembershipProvider` dostawcy (zobacz krok 2).

> [!NOTE]
> W tej serii samouczek użyjemy [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) bazy danych do przechowywania tabel naszej aplikacji i `SqlMembershipProvider` schematu. Ta decyzja została wprowadzona w dwóch powodów: najpierw, ze względu na jego koszt — darmowa - Express Edition jest najbardziej readably dostępna wersja programu SQL Server 2005; Po drugie, baz danych programu SQL Server 2005 Express Edition można umieścić bezpośrednio w aplikacji sieci web `App_Data` folderu, dzięki czemu można z łatwością pakietu bazy danych i aplikacji sieci web razem w jednym pliku ZIP oraz należy ponownie wdrożyć bez żadnych instrukcji ustawienia specjalne lub opcji konfiguracji. Jeśli wolisz odbiorze przy użyciu programu SQL Server w wersji z systemem innym niż Express Edition, możesz. Te kroki są niemal identyczne. `SqlMembershipProvider` Schematu zostanie współdziała z żadną wersją programu Microsoft SQL Server 2000 lub nowsze.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy `App_Data` folderu i wybierz opcję Dodaj nowy element. (Jeśli nie widzisz `App_Data` folder w projekcie, kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, wybierz opcję Dodaj Folder ASP.NET i wybierz `App_Data`.) W oknie dialogowym Dodawanie nowego elementu możliwość dodania nowej bazy danych SQL o nazwie `SecurityTutorials.mdf`. W tym samouczku dodamy `SqlMembershipProvider` schematu do tej bazy danych, w kolejnych samouczkach utworzymy dodatkowe tabele do przechwytywania danych aplikacji.


[![Dodaj nową bazę danych SQL o nazwie SecurityTutorials.mdf bazy danych w folderze App_Data](creating-the-membership-schema-in-sql-server-vb/_static/image2.png)](creating-the-membership-schema-in-sql-server-vb/_static/image1.png)

**Rysunek 1**: Dodawanie nowej bazy danych SQL, o nazwie `SecurityTutorials.mdf` bazy danych do `App_Data` Folder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-vb/_static/image3.png))


Dodawanie bazy danych do `App_Data` folderu automatycznie uwzględnia ona w widoku Eksploratora bazy danych. (W wersji z systemem innym niż Express Edition programu Visual Studio, Eksploratora bazy danych jest nazywany Eksploratora serwera). Przejdź do Eksploratora bazy danych i rozwiń po prostu dodane `SecurityTutorials` bazy danych. Jeśli nie ma pomocy Eksploratora bazy danych na ekranie, przejdź do menu Widok i wybierz Eksploratora bazy danych lub kliknij przycisk Ctrl + Alt + S. Jak pokazano na rysunku 2, `SecurityTutorials` baza danych jest pusta — zawiera on żadnych tabel, Brak widoków i nie procedur składowanych.


[![Baza danych SecurityTutorials jest obecnie pusta](creating-the-membership-schema-in-sql-server-vb/_static/image5.png)](creating-the-membership-schema-in-sql-server-vb/_static/image4.png)

**Rysunek 2**: `SecurityTutorials` bazy danych jest obecnie pusta ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-vb/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Krok 2: Dodawanie`SqlMembershipProvider`schematu do bazy danych

`SqlMembershipProvider` Wymaga określony zestaw tabel, widoków i procedur składowanych do zainstalowania w bazie danych magazynu użytkownika. Te obiekty wymagania bazy danych można dodać za pomocą [ `aspnet_regsql.exe` narzędzia](https://msdn.microsoft.com/library/ms229862.aspx). Ten plik znajduje się w `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\` folderu.

> [!NOTE]
> `aspnet_regsql.exe` Narzędzie udostępnia zarówno funkcje wiersza polecenia i graficzny interfejs użytkownika. Interfejs graficzny jest bardziej przyjazny dla użytkownika i jakie zostaną omówione w tym samouczku. Interfejs wiersza polecenia jest przydatne, gdy dodanie `SqlMembershipProvider` schemat musi zostać zautomatyzowane, taki jak kompilacji skrypty lub automatyczne scenariuszy testowych.

`aspnet_regsql.exe` Narzędzie służy do dodawania lub usuwania *usługami aplikacji ASP.NET* określonej bazy danych SQL Server. Usługi aplikacji ASP.NET obejmować schematów dla `SqlMembershipProvider` i `SqlRoleProvider`, wraz ze schematów dla dostawców SQL na podstawie dla innych platform, ASP.NET 2.0. Należy podać dwa bity informacji `aspnet_regsql.exe` narzędzie:

- Określa, czy chcemy dodać lub usunąć usługi aplikacji i
- Bazy danych, w którym można dodać ani usunąć schemat usług aplikacji

W monitowania dla bazy danych do użycia, `aspnet_regsql.exe` narzędzie monituje nam Podaj nazwę serwera bazy danych znajduje się na poświadczenia bezpieczeństwa dla łączenia z bazą danych i nazwa bazy danych. Jeśli korzystają z systemem innym niż - Express Edycja programu SQL Server, należy znasz już te informacje, jak te same informacje wymagane do ciągu połączenia podczas pracy z bazy danych za pośrednictwem strony sieci web platformy ASP.NET. Określanie nazwy serwera i bazy danych w przypadku korzystania z bazy danych programu SQL Server 2005 Express Edition w `App_Data` folderu, jest jednak nieco więcej wysiłku.

Poniższa sekcja sprawdza łatwe do określenia nazwy serwera i bazy danych dla bazy danych programu SQL Server 2005 Express Edition w `App_Data` folderu. Jeśli nie używasz programu SQL Server 2005 Express Edition działanie może przejść od razu do instalowania sekcji usługi aplikacji.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>Określanie serwera i nazwę bazy danych programu SQL Server 2005 Express Edition bazy danych w`App_Data`folderu

Aby można było używać `aspnet_regsql.exe` narzędzia należy znać nazwy serwera i bazy danych. Nazwa serwera jest `localhost\InstanceName`. Najbardziej prawdopodobną *InstanceName* jest `SQLExpress`. Jednak jeśli ręcznie zainstalować program SQL Server 2005 Express Edition (to znaczy, że nie zainstalowano go automatycznie podczas instalowania programu Visual Studio), wówczas jest to możliwe, że wybrano inną nazwę wystąpienia.

Nazwa bazy danych jest nieco trudniejszy do określenia. Bazy danych w `App_Data` folderu zwykle mają nazwę bazy danych, która zawiera [Unikatowy identyfikator globalny](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) wraz ze ścieżką do pliku bazy danych. Należy określić to nazwa bazy danych, aby można było dodać schematu usług aplikacji za pomocą `aspnet_regsql.exe`.

Najprostszym sposobem sprawdzenia Nazwa bazy danych jest zbadanie za pomocą programu SQL Server Management Studio. SQL Server Management Studio udostępnia interfejs graficzny do zarządzania baz danych programu SQL Server 2005, ale nie jest dostarczany z Express Edition z programu SQL Server 2005. Dobre wieści jest to, że [można pobrać](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) wolnego Express Edition programu SQL Server Management Studio.

> [!NOTE]
> Jeśli masz z systemem innym niż Express Edition wersji SQL Server 2005 zainstalowanego na pulpicie pełną wersję programu Management Studio prawdopodobnie jest zainstalowany. Pełną wersję służy do określenia nazwy bazy danych, wykonując czynności opisane poniżej dla wersji Express Edition.

Rozpocznij od zamknięcia programu Visual Studio, aby upewnić się, że wszystkie blokady narzucone przez program Visual Studio w pliku bazy danych są zamknięte. Następnie uruchom program SQL Server Management Studio i połącz się z `localhost\InstanceName` bazy danych programu SQL Server 2005 Express Edition. Jak wspomniano wcześniej, prawdopodobnie nazwa wystąpienia jest `SQLExpress`. Dla opcji uwierzytelniania wybierz Uwierzytelnianie systemu Windows.


[![Połącz z wystąpieniem programu SQL Server 2005 Express Edition](creating-the-membership-schema-in-sql-server-vb/_static/image8.png)](creating-the-membership-schema-in-sql-server-vb/_static/image7.png)

**Rysunek 3**: nawiązać połączenia z wystąpienia programu SQL Server 2005 Express Edition ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-vb/_static/image9.png))


Po nawiązaniu połączenia z wystąpieniem programu SQL Server 2005 Express Edition, Management Studio Wyświetla folderów dla baz danych, ustawienia zabezpieczeń, obiektów serwera i tak dalej. Po rozwinięciu karcie bazy danych zostanie wyświetlone `SecurityTutorials.mdf` baza danych jest *nie* zarejestrowany w wystąpieniu bazy danych — należy najpierw dołączyć bazy danych.

Kliknij prawym przyciskiem folder baz danych i wybierz z menu kontekstowego Attach. Spowoduje to wyświetlenie okna dialogowego dołączyć bazy danych. W tym miejscu, kliknij przycisk Dodaj, przejdź do `SecurityTutorials.mdf` bazę danych, a następnie kliknij przycisk OK. Na rysunku 4 przedstawiono okno dialogowe dołączanie bazy danych po `SecurityTutorials.mdf` baza danych została wybrana. Rysunek 5. Pokazuje Eksplorator obiektów programu Management Studio po pomyślnym dołączyć bazy danych.


[![Dołącz tę bazę danych SecurityTutorials.mdf](creating-the-membership-schema-in-sql-server-vb/_static/image11.png)](creating-the-membership-schema-in-sql-server-vb/_static/image10.png)

**Rysunek 4**: Dołącz `SecurityTutorials.mdf` bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-vb/_static/image12.png))


[![Baza danych SecurityTutorials.mdf znajduje się w folderze baz danych](creating-the-membership-schema-in-sql-server-vb/_static/image14.png)](creating-the-membership-schema-in-sql-server-vb/_static/image13.png)

**Rysunek 5**: `SecurityTutorials.mdf` bazy danych znajduje się w folderze bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-vb/_static/image15.png))


Jak pokazano na rysunku nr 5, `SecurityTutorials.mdf` bazy danych jest raczej abstruse nazwy. Umożliwia zmieniać, aby łatwiej zapamiętać (i łatwiejsze do typu) nazwa. Kliknij prawym przyciskiem myszy w bazie danych, z menu kontekstowego wybierz polecenie Zmień nazwę i zmień jego nazwę `SecurityTutorialsDatabase`. Nie zmienia nazwę pliku, używa tylko nazwy bazy danych do identyfikacji do programu SQL Server.


[![Zmień nazwę bazy danych na SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-vb/_static/image17.png)](creating-the-membership-schema-in-sql-server-vb/_static/image16.png)

**Rysunek 6**: Zmień nazwę bazy danych do `SecurityTutorialsDatabase`([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-vb/_static/image18.png))


W tym momencie wiemy, nazwy serwera i bazy danych dla `SecurityTutorials.mdf` pliku bazy danych: `localhost\InstanceName` i `SecurityTutorialsDatabase`odpowiednio. Firma Microsoft są teraz gotowe do zainstalowania usług aplikacji za pośrednictwem `aspnet_regsql.exe` narzędzia.

### <a name="installing-the-application-services"></a>Instalowanie usług aplikacji

Aby uruchomić `aspnet_regsql.exe` narzędzia, przejdź do start menu i wybierz polecenie Uruchom. Wprowadź `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` w polu tekstowym i kliknij przycisk OK. Alternatywnie można użyć Eksploratora Windows do następnie przejść do odpowiedniego folderu, a następnie kliknij dwukrotnie ikonę `aspnet_regsql.exe` pliku. Każda metoda będzie net takie same wyniki.

Uruchomiona `aspnet_regsql.exe` narzędzia bez żadnych argumentów wiersza polecenia uruchamia Kreator konfiguracji ASP.NET SQL Server graficznego interfejsu użytkownika. Kreator ułatwia dodawanie lub usuwanie określonej bazy danych usług aplikacji ASP.NET. Pierwszy ekran kreatora pokazano na rysunku 7 opisującą jego cel narzędzia.


[![Umożliwia dodawanie schematu członkostwa ułatwia Kreatora instalacji serwera SQL programu ASP.NET](creating-the-membership-schema-in-sql-server-vb/_static/image20.png)](creating-the-membership-schema-in-sql-server-vb/_static/image19.png)

**Rysunek 7**: Użyj ASP.NET SQL Server Instalator Kreatora powoduje, że można dodać schematu członkostwa ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-vb/_static/image21.png))


Drugim krokiem w Kreatorze zapyta nam czy chcemy usług aplikacji Dodaj lub usuń je. Ponieważ chcemy dodać tabel, widoków i procedur składowanych, które są niezbędne do `SqlMembershipProvider`, wybierz pozycję Konfiguruj serwer SQL dla opcji usług aplikacji. Później Jeśli chcesz usunąć ten schemat z bazy danych, uruchom ponownie tego kreatora, ale zamiast tego wybrać informacje o usługach aplikacji Usuń z istniejących opcji bazy danych.


[![Wybierz konfigurowania programu SQL Server dla opcji usługi aplikacji](creating-the-membership-schema-in-sql-server-vb/_static/image23.png)](creating-the-membership-schema-in-sql-server-vb/_static/image22.png)

**Rysunek 8**: wybierz pozycję Konfiguruj serwer SQL dla aplikacji usług opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-vb/_static/image24.png))


Trzeci krok monituje o informacje z bazy danych: Nazwa serwera, informacje dotyczące uwierzytelniania i nazwę bazy danych. Jeśli zostały następujące wraz z tego samouczka i dodano `SecurityTutorials.mdf` bazy danych do `App_Data`, dołączone do `localhost\InstanceName`, zmienić jego nazwę i `SecurityTutorialsDatabase`, następnie użyj następujących wartości:

- Serwer: `localhost\InstanceName`
- Uwierzytelnianie systemu Windows
- Baza danych: `SecurityTutorialsDatabase`


[![Wprowadź informacje o bazie danych](creating-the-membership-schema-in-sql-server-vb/_static/image26.png)](creating-the-membership-schema-in-sql-server-vb/_static/image25.png)

**Rysunek 9**: Wprowadź informacje o bazie danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-vb/_static/image27.png))


Po wprowadzeniu informacji bazy danych, kliknij przycisk Dalej. Ostatnim krokiem zawiera podsumowanie kroków, które zostaną wykonane. Kliknij przycisk Dalej, aby zainstalować usługi aplikacji i Zakończ, aby zakończyć pracę kreatora.

> [!NOTE]
> Jeśli używasz Management Studio można dołączyć bazy danych i Zmień nazwę pliku bazy danych, należy odłączyć bazy danych i zamknij program Management Studio przed ponownym programu Visual Studio. Aby odłączyć `SecurityTutorialsDatabase` bazy danych, kliknij prawym przyciskiem myszy nazwę bazy danych i wybierz polecenie Odłącz z menu zadania.

Po zakończeniu działania kreatora wróć do programu Visual Studio i przejdź do Eksploratora bazy danych. Rozwiń folder tabel. Powinny pojawić się serii tabel, których nazwy rozpoczynają się od prefiksu `aspnet_`. Podobnie różne widoki i procedury składowane można znaleźć w folderze widoków i procedur składowanych. Te obiekty bazy danych tworzą schemat usług aplikacji. Omówione obiektów bazy danych specyficznych dla członkostwa i ról w kroku 3.


[![Różnych tabel, widoków i procedur składowanych zostały dodane do bazy danych](creating-the-membership-schema-in-sql-server-vb/_static/image29.png)](creating-the-membership-schema-in-sql-server-vb/_static/image28.png)

**Na rysunku nr 10**: A różnych tabel, widoków i przechowywane procedury zostały dodane do bazy danych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-vb/_static/image30.png))


> [!NOTE]
> `aspnet_regsql.exe` Schemat usług całej aplikacji instaluje narzędzia graficznego interfejsu użytkownika. Jednak podczas wykonywania `aspnet_regsql.exe` z wiersza polecenia można określić, jakie określonej aplikacji usługi składników, aby zainstalować (lub usunąć). W związku z tym, jeśli chcesz dodać tylko tabele, widoki i przechowywane procedury niezbędne do `SqlMembershipProvider` i `SqlRoleProvider` dostawców, uruchamiania `aspnet_regsql.exe` z wiersza polecenia. Alternatywnie możesz ręcznie uruchomić odpowiednie podzbiór T-SQL tworzenia skryptów używanych przez `aspnet_regsql.exe`. Skrypty te znajdują się w `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` folder o nazwach, jak `InstallCommon.sql`, `InstallMembership.sql`, `InstallRoles.sql`, `InstallProfile.sql`, `InstallSqlState.sql`i tak dalej.

Na tym etapie firma Microsoft opracowała obiektów bazy danych wymaganych przez `SqlMembershipProvider`. Jednak nadal należy poinstruować framework członkostwa, czy należy używać `SqlMembershipProvider` (a, powiedz, `ActiveDirectoryMembershipProvider`) oraz że `SqlMembershipProvider` należy używać `SecurityTutorials` bazy danych. Wyjaśniono, jak określać, jakie dostawca do użycia i dostosowywanie ustawień wybranego dostawcy w kroku 4. Jednak najpierw Spójrzmy więcej danych na obiekty bazy danych, które zostały utworzone.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Krok 3: Przyjrzeć tabele podstawowe schematu

Podczas pracy z platform członkostwo i role w aplikacji ASP.NET, szczegóły implementacji są hermetyzowane przez dostawcę. W przyszłości samouczki firma Microsoft będzie łączyć się z tych platform za pomocą programu .NET Framework `Membership` i `Roles` klasy. Korzystając z tych ogólnych interfejsów API nie musimy dotyczą nad szczegóły niskiego poziomu, takie jak zapytania, jakie są wykonywane lub tabele są modyfikowane przez `SqlMembershipProvider` i `SqlRoleProvider`.

Biorąc pod uwagę to, można bezpiecznie używamy struktur członkostwo i role bez konieczności przedstawione schematu bazy danych utworzone w kroku 2. Jednak podczas tworzenia tabel do przechowywania danych aplikacji może należy utworzyć jednostki, które odnoszą się do użytkowników lub ról. Ułatwia znajomość `SqlMembershipProvider` i `SqlRoleProvider` schematy podczas ustanawiania obcego klucza ograniczenia między tabelami danych aplikacji i tych tabel utworzone w kroku 2. Ponadto w pewnych okolicznościach rzadkich firma Microsoft może być konieczne interfejsu użytkownika i roli przechowuje bezpośrednio na poziomie bazy danych (zamiast za pomocą `Membership` lub `Roles` klasy).

### <a name="partitioning-the-user-store-into-applications"></a>Partycjonowanie magazynie użytkownika do aplikacji

Struktury członkostwo i role są zaprojektowane tak, aby ma jeden magazyn użytkownika i roli mogą być współużytkowane przez wiele aplikacji. Aplikacji programu ASP.NET, która używa struktury członkostwa lub ról, należy określić partycji aplikacji do użycia. Krótko mówiąc wiele aplikacji sieci web można użyć tego samego użytkownika i roli magazynów. Rysunek 11 przedstawia użytkownika i roli magazynów, które są podzielone na trzy aplikacje: HRSite, CustomerSite i SalesSite. Te trzy aplikacje sieci web każdego mają własne unikatowych użytkowników i ról, ale wszystkie fizycznie przechowywania ich kont użytkowników i ról informacji w tej samej tabel bazy danych.


[![Konta użytkowników mogą być podzielone na partycje dla wielu aplikacji](creating-the-membership-schema-in-sql-server-vb/_static/image32.png)](creating-the-membership-schema-in-sql-server-vb/_static/image31.png)

**Rysunek 11**: użytkownika konta może być podzielona na partycje w wielu aplikacjach ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-the-membership-schema-in-sql-server-vb/_static/image33.png))


`aspnet_Applications` Tabela jest definiuje co tych partycji. Każda aplikacja, która przechowuje informacje o koncie użytkownika przy użyciu bazy danych jest reprezentowana przez wiersza w tej tabeli. `aspnet_Applications` Ma cztery kolumny: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`, i `Description`.`ApplicationId` Typ jest [ `uniqueidentifier` ](https://msdn.microsoft.com/library/ms187942.aspx) i klucz podstawowy tabeli; `ApplicationName` zapewnia unikatową nazwę przyjaznych dla człowieka dla każdej aplikacji.

Inne powiązane członkostwa i ról tabele link do `ApplicationId` w `aspnet_Applications`. Na przykład `aspnet_Users` tabeli, która zawiera rekord dla każdego konta użytkownika, ma `ApplicationId` pola klucza obcego; jw. dla `aspnet_Roles` tabeli. `ApplicationId` Pola w tych tabelach określa partycji aplikacji konto użytkownika lub rola.

### <a name="storing-user-account-information"></a>Zapisywanie informacji o koncie użytkownika

Informacje o koncie użytkownika są przechowywane w dwóch tabelach: `aspnet_Users` i `aspnet_Membership`. `aspnet_Users` Tabela zawiera pola, które są przechowywane informacje o koncie użytkownika podstawowych. Trzy kolumny najbardziej potrzebne są:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` jest to klucz podstawowy (i typu `uniqueidentifier`). `UserName` Typ jest `nvarchar(256)` i wraz z hasłem, stanowi poświadczenia użytkownika. (Hasło użytkownika są przechowywane w `aspnet_Membership` tabeli.) `ApplicationId` łączy konta użytkownika do konkretnej aplikacji w `aspnet_Applications`. Brak złożonym [ `UNIQUE` ograniczenia](https://msdn.microsoft.com/library/ms191166.aspx) na `UserName` i `ApplicationId` kolumn. Dzięki temu, że w danej aplikacji jest unikatowe nazwy użytkownika, ale umożliwia dla tego samego `UserName` do użycia w innych aplikacjach.

`aspnet_Membership` Tabela zawiera dodatkowe informacje, takie jak hasło użytkownika, adres e-mail, ostatniego logowania daty i godziny oraz itd. Brak odpowiednika między rekordów w `aspnet_Users` i `aspnet_Membership` tabel. Ta relacja jest zapewniana przez `UserId` w `aspnet_Membership`, która służy jako klucz podstawowy tabeli. Podobnie jak `aspnet_Users` tabeli `aspnet_Membership` obejmuje `ApplicationId` pola, które wiąże te informacje do określonej partycji aplikacji.

### <a name="securing-passwords"></a>Zabezpieczanie hasła

Informacje o hasło jest przechowywane w `aspnet_Membership` tabeli. `SqlMembershipProvider` Umożliwia hasła mają być przechowywane w bazie danych przy użyciu jednej z następujących trzech metod:

- **Wyczyść** — hasło jest przechowywane w bazie danych jako zwykłego tekstu. Można zdecydowanie zniechęcić przy użyciu tej opcji. Jeśli zostanie naruszony bazy danych — można go przez hakera, która znajdzie drzwi wstecz lub niezadowoleni pracownika, który ma dostęp do bazy danych — poświadczenia każdego pojedynczego użytkownika czy istnieją pobierania.
- **Wartość skrótu** -hasła mają formę skrótu przy użyciu jednokierunkowego algorytmu skrótu i generowanej losowo wartości zaburzającej. Ta wartość skrótu (wraz z soli) są przechowywane w bazie danych.
- **Szyfrowane** -zaszyfrowana wersja hasło jest przechowywane w bazie danych.

Zależy od hasła magazynu zastosowanych `SqlMembershipProvider` ustawień określonych w `Web.config`. Zajmiemy dostosowywanie `SqlMembershipProvider` ustawienia w kroku 4. Domyślnym zachowaniem jest przechowywanie skrótu hasła.

Są zobowiązani do przechowywania hasła kolumny `Password`, `PasswordFormat`, i `PasswordSalt`. `PasswordFormat` To pole typu `int` wskazuje, którego wartość zastosowanych do przechowywania hasła: 0 w przypadku zwykłego; 1 dla Hashed 2 dla zaszyfrowana. `PasswordSalt` przypisano losowo wygenerowany ciąg niezależnie od tego hasła magazynu zastosowanych; wartość `PasswordSalt` jest używana tylko podczas obliczania skrótu hasła. Na koniec `Password` kolumna zawiera dane rzeczywiste hasło, jest to hasło jako zwykły tekst, skrót hasła lub zaszyfrowane hasło.

Tabela 1 przedstawiono te trzy kolumny przykładową różnych technik magazynu w przypadku przechowywania haseł MySecret! .

| **Techniki magazynu&lt;\_o3a\_p /&gt;** | **Hasło&lt;\_o3a\_p /&gt;** | **PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| Wyczyść | MySecret! | 0 | tTnkPlesqissc2y2SMEygA== |
| Wartość skrótu | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1 | wFgjUfhdUFOCKQiI61vtiQ== |
| Szyfrowane | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**Tabela 1**: Przykładowe wartości w polach powiązane hasło podczas zapisywania MySecret hasło!

> [!NOTE]
> Określonego szyfrowania lub algorytmu wyznaczania wartości skrótu używanego przez `SqlMembershipProvider` zależy od ustawień w `<machineKey>` elementu. Omówiono ten element konfiguracji w kroku 3 <a id="Tutorial3"> </a> [ *konfiguracji uwierzytelniania formularzy i Tematy zaawansowane* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) samouczka.

### <a name="storing-roles-and-role-associations"></a>Przechowywanie ról i skojarzenia roli

W ramach ról umożliwia deweloperom zdefiniować zestaw ról i określ, jakie użytkownicy należą do role. Te informacje są przechwytywane w bazie danych za pośrednictwem dwóch tabel: `aspnet_Roles` i `aspnet_UsersInRoles`. Każdy rekord w `aspnet_Roles` tabeli reprezentuje roli dla określonej aplikacji. Podobnie jak `aspnet_Users` tabeli `aspnet_Roles` ma trzy kolumny właściwą naszych dyskusji:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` jest to klucz podstawowy (i typu `uniqueidentifier`). `RoleName` jest typu `nvarchar(256)`. I `ApplicationId` łączy konta użytkownika do konkretnej aplikacji w `aspnet_Applications`. Brak złożonym `UNIQUE` ograniczenie `RoleName` i `ApplicationId` kolumn, zapewnia, że nazwa każdej roli w danej aplikacji jest unikatowy.

`aspnet_UsersInRoles` Tabeli służy jako mapowania między użytkownikami i rolami. Istnieją tylko dwa kolumny - `UserId` i `RoleId` - i razem stanowią podstawowy klucz złożony.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Krok 4: Określanie dostawcy i dostosowując jego ustawienia

Struktury obsługujących modelu dostawcy — takich jak członkostwo i role struktury — Brak szczegóły implementacji, same i zamiast tego przekazać to zadanie do klasy dostawcy. W przypadku framework członkostwa `Membership` klasa definiuje interfejs API do zarządzania kontami użytkowników, ale nie nawiązuje interakcji bezpośrednio z dowolnego magazynu użytkownika. Zamiast `Membership` żądanie skonfigurowanego dostawcy - Dostarcz metody klasy użyjemy `SqlMembershipProvider`. Gdy firma Microsoft wywołania jednej z metod w `Membership` klasy, jak będzie framework członkostwa wiadomo, aby delegować wywołanie `SqlMembershipProvider`?

`Membership` Klasa ma [ `Providers` właściwości](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) zawiera odwołania do wszystkich klas zarejestrowanych dostawców dostępne do użycia przez platformę członkostwa. Każdy zarejestrowany dostawca ma skojarzonej z nazwy i typu. Nazwa daje możliwość przyjaznych dla człowieka odwołania określonego dostawcy w `Providers` kolekcji, gdy typ identyfikuje klasy dostawcy. Ponadto każdy zarejestrowany dostawca może uwzględniają ustawienia konfiguracji. Ustawienia konfiguracji dla framework członkostwa obejmują `PasswordFormat` i `requiresUniqueEmail`, wśród wielu innych. Zobacz pełną listę ustawień konfiguracyjnych używanych przez Tabela 2 `SqlMembershipProvider`.

`Providers` Zawartość właściwości są określone w ustawieniach konfiguracji aplikacji sieci web. Domyślnie wszystkie aplikacje sieci web ma dostawcy o nazwie `AspNetSqlMembershipProvider` typu `SqlMembershipProvider`. Domyślny dostawca członkostwa jest zarejestrowany w `machine.config` (znajdujący się w `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample1.xml)]

Jako kod znaczników powyżej przedstawiono [ `<membership>` elementu](https://msdn.microsoft.com/library/1b9hw62f.aspx) definiuje ustawienia konfiguracji dla framework członkostwa podczas [ `<providers>` element podrzędny](https://msdn.microsoft.com/library/6d4936ht.aspx) Określa zarejestrowaną dostawcy. Dostawcy mogą być dodane lub usunięte za pomocą [ `<add>` ](https://msdn.microsoft.com/library/whae3t94.aspx) lub [ `<remove>` ](https://msdn.microsoft.com/library/aykw9a6d.aspx) elementy; użyj [ `<clear>` ](https://msdn.microsoft.com/library/t062y6yc.aspx) elementu do usunięcia wszystkich obecnie zarejestrowanych dostawców. Jako kod znaczników powyżej przedstawiono `machine.config` Dodaje dostawcę o nazwie `AspNetSqlMembershipProvider` typu `SqlMembershipProvider`.

Oprócz `name` i `type` atrybuty, `<add>` element zawiera atrybuty definiujące wartości dla różnych konfigurowania ustawień. Tabela 2 zawiera listę dostępnych `SqlMembershipProvider`-określonych ustawień konfiguracyjnych, wraz z opisem.

> [!NOTE]
> Wartości domyślne wymienionych w tabeli 2 odwoływać się do wartości domyślnej zdefiniowanej w `SqlMembershipProvider` klasy. Należy pamiętać, że nie wszystkie ustawienia konfiguracji w `AspNetSqlMembershipProvider` odpowiada wartości domyślne `SqlMembershipProvider` klasy. Na przykład, jeśli nie określono dostawcy członkostwa `requiresUniqueEmail` ustawienie domyślne true. Jednak `AspNetSqlMembershipProvider` zastępuje wartość domyślną jawnie, określając wartość `false`.

| **Ustawienie&lt;\_o3a\_p /&gt;** | **Opis elementu&lt;\_o3a\_p /&gt;** |
| --- | --- |
| `ApplicationName` | Wycofanie członkostwa framework umożliwiający magazynu jednego użytkownika do partycjonowania dla wielu aplikacji. To ustawienie wskazuje nazwę partycji aplikacji, używane przez dostawcę członkostwa. Jeśli ta wartość nie jawnie określono, jest ustawiona, w czasie wykonywania wartość ścieżka wirtualnego katalogu głównego aplikacji. |
| `commandTimeout` | Określa wartość limitu czasu polecenia SQL (w sekundach). Wartość domyślna to 30. |
| `connectionStringName` | Nazwa parametrów połączenia w `<connectionStrings>` elementu na potrzeby połączenia z bazą danych magazynu użytkownika. Ta wartość jest wymagana. |
| `description` | Zawiera opis przyjaznych dla człowieka zarejestrowanego dostawcy. |
| `enablePasswordRetrieval` | Określa, czy użytkownicy mogą pobrać ich zapomniane hasło. Wartość domyślna to `false`. |
| `enablePasswordReset` | Wskazuje, czy użytkownicy mogą zresetować swoje hasło. Domyślnie `true`. |
| `maxInvalidPasswordAttempts` | Maksymalna liczba prób logowania nie powiedzie, które mogą wystąpić dla danego użytkownika w określonym `passwordAttemptWindow` przed zablokowaniem użytkownika. Wartość domyślna to 5. |
| `minRequiredNonalphanumericCharacters` | Minimalna liczba znaków innych niż alfanumeryczne, które muszą znajdować się w hasło użytkownika. Ta wartość musi wynosić od 0 do 128; Wartość domyślna to 1. |
| `minRequiredPasswordLength` | Minimalna liczba znaków w haśle. Ta wartość musi wynosić od 0 do 128; Wartość domyślna to 7. |
| `name` | Nazwa zarejestrowanego dostawcy. Ta wartość jest wymagana. |
| `passwordAttemptWindow` | Liczba minut, w którym nie są śledzone prób logowania. Jeśli użytkownik poda nieprawidłowych poświadczeń logowania `maxInvalidPasswordAttempts` określono czasy w ramach tego okna, są zablokowane. Wartość domyślna to 10. |
| `PasswordFormat` | Format przechowywania haseł: `Clear`, `Hashed`, lub `Encrypted`. Wartość domyślna to `Hashed`. |
| `passwordStrengthRegularExpression` | Jeśli zostanie podana, to wyrażenie regularne jest używany do oceny siły hasła wybranego użytkownika podczas tworzenia nowego konta lub, w przypadku zmiany hasła. Wartością domyślną jest ciąg pusty. |
| `requiresQuestionAndAnswer` | Określa, czy użytkownik musi odpowiedzieć na swoje pytanie zabezpieczeń podczas pobierania lub resetowania hasła. Wartość domyślna to `true`. |
| `requiresUniqueEmail` | Wskazuje, czy wszystkie konta użytkowników w danej aplikacji musi mieć unikatowy adres e-mail. Wartość domyślna to `true`. |
| `type` | Określa typ dostawcy. Ta wartość jest wymagana. |

**Tabela 2**: członkostwa i `SqlMembershipProvider` ustawienia konfiguracji

Oprócz `AspNetSqlMembershipProvider`, innych dostawców członkostwa mogą być rejestrowane przez dodanie znaczników podobny do poszczególnych aplikacji w aplikacji `Web.config` pliku.

> [!NOTE]
> W ramach ról działa w taki sam sposób: Brak domyślnego dostawcę roli zarejestrowanych w `machine.config` i zarejestrowanych dostawców można dostosować na podstawie aplikacji przez aplikację w `Web.config`. Zostaną omówione framework ról i jego znaczników konfiguracji szczegółowo w przyszłości samouczka.

### <a name="customizing-thesqlmembershipprovidersettings"></a>Dostosowywanie`SqlMembershipProvider`ustawienia

Wartość domyślna `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) ma jego `connectionStringName` ustawić atrybutu `LocalSqlServer`. Podobnie jak `AspNetSqlMembershipProvider` dostawcy, nazwa ciągu połączenia `LocalSqlServer` jest zdefiniowany w `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample2.xml)]

Jak widać, ten ciąg połączenia definiuje SQL 2005 Express Edition w lokalizacji bazy danych | DataDirectory|aspnetdb.mdf. Ciąg | DataDirectory | przetłumaczono w czasie wykonywania, aby wskazywał `~/App_Data/` katalogu, więc ścieżki bazy danych | Oznacza DataDirectory|aspnetdb.mdf `~/App_Data` / `aspnet.mdf`.

Jeśli firma Microsoft nie określa żadnych informacji o dostawcy członkostwa w naszej aplikacji `Web.config` plik, aplikacja używa dostawcę członkostwa w zarejestrowany domyślne `AspNetSqlMembershipProvider`. Jeśli `~/App_Data/aspnet.mdf` bazy danych nie istnieje, automatycznie go utworzyć i dodać schematu usług aplikacji środowiska wykonawczego programu ASP.NET. Jednak nie chcemy użyć `aspnet.mdf` bazy danych; zamiast chcemy użyć `SecurityTutorials.mdf` bazy danych utworzono w kroku 2. Ta modyfikacja można zrobić w jeden z dwóch sposobów:

- <strong>Określ wartość</strong><strong>`LocalSqlServer`</strong><strong>Nazwa ciągu połączenia w</strong><strong>`Web.config`</strong><strong>.</strong> Zastępując `LocalSqlServer` wartości nazwy parametrów połączenia w `Web.config`, możemy użyć zarejestrowany domyślnym dostawcą członkostwa (`AspNetSqlMembershipProvider`) i będzie on działać poprawnie z `SecurityTutorials.mdf` bazy danych. Ta metoda jest poprawnie, jeśli jesteś zawartości przy użyciu ustawień konfiguracyjnych, określony przez `AspNetSqlMembershipProvider`. Aby uzyskać więcej informacji na temat tej techniki, zobacz [Scott Guthrie](https://weblogs.asp.net/scottgu/)w blogu, [Konfigurowanie ASP.NET 2.0 usługi aplikacji, użyj programu SQL Server 2000 lub SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Dodaj nowego dostawcę zarejestrowanego typu</strong><strong>`SqlMembershipProvider`</strong><strong>i skonfigurować jej</strong><strong>`connectionStringName`</strong><strong>ustawienie, aby wskazywał</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>bazy danych.</strong> Takie rozwiązanie jest przydatne w scenariuszach, w których chcesz dostosować inne właściwości konfiguracji oprócz parametry połączenia bazy danych. W projektach I zawsze posłuż się tą metodą ze względu na jego elastyczność i możliwość odczytania.

Zanim można dodać nowego dostawcę zarejestrowanych, który odwołuje się do `SecurityTutorials.mdf` bazy danych, najpierw należy dodać wartość ciągu połączenia w `<connectionStrings>` sekcji `Web.config`. Następujący kod dodaje nowe parametry połączenia o nazwie `SecurityTutorialsConnectionString` który odwołuje się do programu SQL Server 2005 Express Edition `SecurityTutorials.mdf` bazy danych w `App_Data` folderu.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample3.xml)]

> [!NOTE]
> Jeśli korzystasz z plikiem alternatywnej bazy danych, należy zaktualizować parametry połączenia, zgodnie z potrzebami. Więcej informacji o tworzące prawidłowych parametrów połączenia, można znaleźć w [ConnectionStrings.com](http://www.connectionstrings.com/).

Następnie dodaj następujący kod konfiguracji członkostwa do `Web.config` pliku. Ten kod znaczników rejestruje nowego dostawcę o nazwie `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample4.xml)]

Oprócz rejestrowania `SecurityTutorialsSqlMembershipProvider` definiuje powyżej znaczników dostawcy, `SecurityTutorialsSqlMembershipProvider` jako domyślny dostawca (za pośrednictwem `defaultProvider` atrybutu w `<membership>` elementu). Odwołania, że w ramach członkostwa może mieć wiele zarejestrowanych dostawców. Ponieważ `AspNetSqlMembershipProvider` jest zarejestrowany jako pierwszy dostawca w `machine.config`, chyba że firma Microsoft nie wskazują inaczej służy jako domyślny dostawca.

Obecnie naszej aplikacji ma dwóch dostawców zarejestrowanych: `AspNetSqlMembershipProvider` i `SecurityTutorialsSqlMembershipProvider`. Jednak przed zarejestrowaniem `SecurityTutorialsSqlMembershipProvider` dostawcy firma Microsoft może wyczyszczony całe wcześniej zarejestrowanego dostawcę przez dodanie [ `<clear />` elementu](https://msdn.microsoft.com/library/t062y6yc.aspx) bezpośrednio przed naszych `<add>` elementu. Spowoduje to wyczyszczenie `AspNetSqlMembershipProvider` na liście zarejestrowanych dostawców, co oznacza, że `SecurityTutorialsSqlMembershipProvider` będzie to jedyny Dostawca członkostwa w zarejestrowany. Jeśli użyliśmy takie podejście, firma Microsoft może okazać się oznaczyć `SecurityTutorialsSqlMembershipProvider` jako domyślnego dostawcę, ponieważ będzie to jedyny Dostawca członkostwa w zarejestrowany. Aby uzyskać więcej informacji na temat używania `<clear />`, zobacz [Using `<clear />` podczas dodawania dostawcy](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Należy pamiętać, że `SecurityTutorialsSqlMembershipProvider`w `connectionStringName` ustawienie odwołania, po prostu dodane `SecurityTutorialsConnectionString` Nazwa ciągu połączenia, a jego `applicationName` ustawienie ustawiono wartość SecurityTutorials. Ponadto `requiresUniqueEmail` ustawienie została ustawiona jako `true`. Inne opcje konfiguracji są takie same jak wartości w `AspNetSqlMembershipProvider`. Możesz także zmienić wszelkie zmiany konfiguracji, w tym miejscu, w razie potrzeby. Na przykład można zwiększyć siły hasła, wymagając od dwóch znaków innych niż alfanumeryczne zamiast jedną lub przez odpowiednie zwiększenie długości hasła do ośmiu znaków zamiast 7.

> [!NOTE]
> Wycofanie członkostwa framework umożliwiający magazynu jednego użytkownika do partycjonowania dla wielu aplikacji. Dostawca członkostwa `applicationName` ustawienie wskazuje, jaki aplikacji, dostawca używa podczas pracy z magazynem użytkownika. Należy jawnie ustawić wartość `applicationName` ustawienie konfiguracji, ponieważ jeśli `applicationName` nie jest jawnie ustawiona, jest ona przypisana do ścieżka wirtualnego katalogu głównego aplikacji sieci web w czasie wykonywania. To działa prawidłowo, dopóki nie zmieniają się ścieżka wirtualnego katalogu głównego aplikacji, ale przenoszenia aplikacji na inną ścieżkę, `applicationName` zbyt zmieni ustawienie. W takim przypadku Dostawca członkostwa rozpoczęcia pracy z innej aplikacji, nie był wcześniej używany. Konta użytkowników utworzone przed przeniesienie będą znajdować się w innej aplikacji i tych użytkownicy nie będą mogli zalogować się do witryny. Aby uzyskać więcej informacji na temat omówione w tej sprawie, zobacz [zawsze wartość `applicationName` właściwości podczas konfigurowania programu ASP.NET 2.0 członkostwa i inni dostawcy](https://weblogs.asp.net/scottgu/443634).

## <a name="summary"></a>Podsumowanie

W tym momencie mamy bazy danych z usługami skonfigurowanej aplikacji (`SecurityTutorials.mdf`) i skonfigurowano naszej aplikacji sieci web, aby używał w ramach członkostwa `SecurityTutorialsSqlMembershipProvider` dostawca właśnie zarejestrowany. Ten dostawca zarejestrowanych jest typu `SqlMembershipProvider` i ma jego `connectionStringName` Ustaw ciąg połączenia (`SecurityTutorialsConnectionString`) i jego `applicationName` jawnie ustawiona wartość.

Firma Microsoft są teraz gotowe do użycia w ramach członkostwa z naszej aplikacji. W następnym samouczku omówione zostanie tworzenie nowych kont użytkowników. Po, że przeanalizujemy uwierzytelnianie użytkowników, wykonanie autoryzacji na podstawie użytkownika i przechowuje dodatkowe informacje dotyczące użytkowników w bazie danych.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Zawsze wartość `applicationName` właściwości podczas konfigurowania programu ASP.NET 2.0 członkostwa i innych dostawców](https://weblogs.asp.net/scottgu/443634)
- [Konfigurowanie programu ASP.NET 2.0 usług aplikacji, użyj programu SQL Server 2000 lub SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Pobierz program SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Badanie ASP.NET 2.0 członkostwa s, ról i profilu](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [`<add>` Elementu dla dostawców do członkostwa](https://msdn.microsoft.com/library/whae3t94.aspx)
- [`<membership>` — Element](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [`<providers>` Element członkostwa](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Przy użyciu `<clear />` podczas dodawania dostawcy](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Praca bezpośrednio z `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Szkolenie wideo na tematy zawarte w tym samouczku

- [Objaśnienie członkostwa platformy ASP.NET](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Konfigurowanie programu SQL do pracy ze schematami członkostwa](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Modyfikowanie ustawień członkostwa w domyślnym schemacie członkostwa](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>Informacje o autorze

Scott Bento, Utwórz wiele książek ASP/ASP.NET i twórcę 4GuysFromRolla.com, pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki  *[Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott jest osiągalny w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blogu w [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Alicja Maziarz. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](storing-additional-user-information-cs.md)
> [dalej](creating-user-accounts-vb.md)
