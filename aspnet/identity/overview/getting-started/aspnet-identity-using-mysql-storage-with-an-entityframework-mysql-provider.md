---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: "Tożsamość platformy ASP.NET: Przy użyciu magazynu MySQL przy użyciu platformy EntityFramework MySQL dostawcy (C#) | Dokumentacja firmy Microsoft"
author: maumar
description: "Ten samouczek pokazuje, jak zastąpić domyślny mechanizm magazynu danych ASP.NET Identity EntityFramework (Dostawca klienta SQL) z dostarczyć MySQL..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 82341724286a53f7883df324a391beeae3a9e2bd
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>Tożsamość platformy ASP.NET: Przy użyciu magazynu MySQL przy użyciu dostawcy EntityFramework MySQL (C#)
====================
przez [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Roberta Mcmurraya](https://github.com/rmcmurray)

> Ten samouczek pokazuje, jak zastąpić domyślny mechanizm magazynu danych dla [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) z EntityFramework (Dostawca klienta SQL) z dostawcą MySQL.


Poniższe tematy zostaną omówione w tym samouczku:

- Tworzenie bazy danych MySQL na platformie Azure
- Tworzenie aplikacji MVC przy użyciu szablonu programu Visual Studio 2013 MVC
- Konfigurowanie EntityFramework do pracy z dostawcy bazy danych MySQL
- Uruchomienie aplikacji, aby zweryfikować wyniki

Na końcu tego samouczka będziesz mieć aplikacji MVC z zastosowaniem ASP.NET Identity przechowywania korzystającej z bazy danych MySQL hostowanej na platformie Azure.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Tworzenie wystąpienia bazy danych MySQL na platformie Azure

1. Zaloguj się do [portalu Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Kliknij przycisk **nowy** w dolnej części strony, a następnie wybierz **MAGAZYNU**:  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. W **wybierz i dodatek** kreatora wybierz **baza danych ClearDB MySQL**, a następnie kliknij przycisk **dalej** strzałki w dolnej części ramki:  
  
 [Kliknij poniższy obraz, aby go rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Zachowaj ustawienie domyślne **wolne** planowanie, zmień **nazwa** do **IdentityMySQLDatabase**, wybierz region, który znajduje się najbliżej możesz, a następnie kliknij przycisk **dalej** strzałki w dolnej części ramki:  
  
 [Kliknij poniższy obraz, aby go rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Kliknij przycisk **zakupu** znacznik wyboru, aby zakończyć tworzenie bazy danych.  
  
 [Kliknij poniższy obraz, aby go rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Po utworzeniu bazy danych, można zarządzać nim z **dodatki** kartę w portalu zarządzania. Aby uzyskać informacje o połączeniu dla bazy danych, kliknij przycisk **informacje o połączeniu** w dolnej części strony:  
  
 [Kliknij poniższy obraz, aby go rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Skopiuj parametry połączenia, klikając przycisk Kopiuj przez **CONNECTIONSTRING** pola i zapisz go; użyje tych informacji w dalszej części tego samouczka aplikacji MVC:  
  
 [Kliknij poniższy obraz, aby go rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Tworzenie projektu aplikacji MVC

Aby wykonać kroki opisane w tej części samouczka, najpierw należy zainstalować [programu Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Po zainstalowaniu programu Visual Studio, aby utworzyć nowy projekt aplikacji MVC Użyj następujące czynności:

1. Otwórz program Visual Studio 2103.
2. Kliknij przycisk **nowy projekt** z **Start** strony, lub kliknij przycisk **pliku** menu, a następnie **nowy projekt**:  
  
 [Kliknij poniższy obraz, aby go rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Gdy **nowy projekt** zostanie wyświetlone okno dialogowe, rozwiń **Visual C#** na liście szablonów, następnie kliknij przycisk **sieci Web**i wybierz **aplikacji sieci Web ASP.NET**. Nazwij swój projekt **IdentityMySQLDemo** , a następnie kliknij przycisk **OK**:  
  
 [Kliknij poniższy obraz, aby go rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **MVC** templatewith domyślne opcje; spowoduje to skonfigurować **indywidualnych kont użytkowników** jako metody uwierzytelniania. Kliknij przycisk **OK**:  
  
 [Kliknij poniższy obraz, aby go rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Skonfiguruj EntityFramework do pracy z bazy danych MySQL

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Aktualizacja zestawu programu Entity Framework dla projektu

Aplikacji MVC, który został utworzony na podstawie szablonu Visual Studio 2013 zawiera odwołanie do [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) pakietu, ale ma zostały do tego zestawu, ponieważ jego wersja aktualizacji, które zawierają istotne ulepszenia wydajności. Aby można było używać tych najnowsze aktualizacje w aplikacji, wykonaj następujące kroki.

1. Otwórz projekt MVC programu Visual Studio 2013.
2. Kliknij przycisk **narzędzia**, następnie kliknij przycisk **Menedżer pakietów biblioteki**, a następnie kliknij przycisk **Konsola Menedżera pakietów**:  
  
 [Kliknij poniższy obraz, aby go rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. **Konsola Menedżera pakietów** będą wyświetlane w dolnej części programu Visual Studio. Typ &quot; **EntityFramework pakiet aktualizacji** &quot; i naciśnij klawisz Enter:  
  
 [Kliknij poniższy obraz, aby go rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Zainstaluj dostawcę MySQL na platformie EntityFramework

EntityFramework do łączenia z bazą danych MySQL, należy zainstalować dostawcę MySQL. Aby to zrobić, otwórz **Konsola Menedżera pakietów** i typ &quot; **Pre - Install-Package MySql.Data.Entity**&quot;, a następnie naciśnij klawisz Enter.

> [!NOTE]
> To jest wstępna wersja zestawu i jako taki może on zawierać błędy. Wykryto wstępną wersję dostawcy nie należy używać w środowisku produkcyjnym.


[Kliknij poniższy obraz, aby go rozwinąć.]  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Wprowadzania zmian w konfiguracji projektu do pliku Web.config aplikacji

W tej sekcji skonfigurujesz Entity Framework do używania dostawcy MySQL, który został właśnie zainstalowany Zarejestruj fabryki dostawcy MySQL, a następnie dodaj parametry połączenia z platformy Azure.

> [!NOTE]
> Poniższe przykłady zawiera MySql.Data.dll wersji określonego zestawu. Wersja zestawu zmian, należy zmodyfikować ustawienia prawidłowej konfiguracji w odpowiedniej wersji.


1. Otwórz plik Web.config dla projektu programu Visual Studio 2013.
2. Znajdź następujące ustawienia konfiguracji, które określają domyślny dostawca bazy danych i fabryki Entity Framework:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Te ustawienia konfiguracji należy zastąpić poniższe polecenie, które służy do konfigurowania programu Entity Framework do używania dostawcy MySQL: 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Zlokalizuj &lt;connectionStrings&gt; sekcji i zastąp go następującym kodem, który będzie definiował ciąg połączenia dla bazy danych MySQL hostowanej na platformie Azure (należy pamiętać, że wartość providerName również została zmieniona z oryginalne):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Dodawanie niestandardowych kontekstu MigrationHistory

Korzysta z programu Entity Framework Code First **MigrationHistory** tabeli, aby śledzić zmiany modelu i zapewnienie spójności schematu bazy danych i schematu koncepcyjnego. Jednak ta tabela nie działa dla programu MySQL domyślnie ponieważ klucz podstawowy jest za duży. Aby rozwiązać ten problem, należy zmniejszyć rozmiar klucza dla tej tabeli. Aby to zrobić, wykonaj następujące kroki:

1. Gromadzenia informacji schematu dla tej tabeli **HistoryContext**, które mogą być zmodyfikowane, jak każdy inny **DbContext**. Aby to zrobić, należy dodać nowy plik klasy o nazwie **MySqlHistoryContext.cs** do projektu i zastąp jego zawartość następującym kodem:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Obok będzie konieczne skonfigurowanie programu Entity Framework, aby użyć zmodyfikowanego **HistoryContext**, zamiast domyślnego. Można to zrobić przy użyciu funkcji konfiguracji opartej na kodzie. Aby to zrobić, należy dodać nowy plik klasy o nazwie **MySqlConfiguration.cs** do projektu i zastąp jego zawartość z:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Tworzenie niestandardowych inicjator EntityFramework ApplicationDbContext

Dostawca MySQL, która jest dostępna w tym samouczku nie obsługuje obecnie w przypadku migracji z programu Entity Framework, musisz użyć inicjatory modelu w celu połączenia z bazą danych. Ponieważ w tym samouczku korzysta z wystąpienia programu MySQL na platformie Azure, należy utworzyć niestandardowe inicjatora programu Entity Framework.

> [!NOTE]
> Ten krok nie jest wymagane, jeśli łączysz się z wystąpieniem programu SQL Server na platformie Azure lub jeśli używasz bazy danych, która jest hostowana na lokalnym.


Aby utworzyć niestandardowe inicjatora programu Entity Framework dla programu MySQL, użyj następujących kroków:

1. Dodaj nowy plik klasy o nazwie **MySqlInitializer.cs** do projektu i Zastąp jest zawartość następującym kodem: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Otwórz **IdentityModels.cs** pliku projektu, który znajduje się w **modele** katalogu i zastąp jego zawartości z następujących czynności: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Uruchamianie aplikacji i weryfikowanie bazy danych

Po wykonaniu czynności opisane w poprzednich sekcjach, należy przetestować bazę danych. Aby to zrobić, wykonaj następujące kroki:

1. Naciśnij klawisz **Ctrl + F5** Aby skompilować i uruchomić aplikację sieci web.
2. Kliknij przycisk **zarejestrować** kartę w górnej części strony:  
  
 [Kliknij poniższy obraz, aby go rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Wprowadź nową nazwę użytkownika i hasło, a następnie kliknij przycisk **zarejestrować**:  
  
 [Kliknij poniższy obraz, aby go rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. W tym momencie ASP.NET Identity tabele są tworzone w bazie danych MySQL, a użytkownik jest zarejestrowany, a zalogowany do aplikacji:  
  
 [Kliknij poniższy obraz, aby go rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Instalowanie narzędzia MySQL Workbench do sprawdzania danych

1. Zainstaluj **MySQL Workbench** narzędzia z [MySQL pobiera strony](http://dev.mysql.com/downloads/windows/installer/)
2. W Kreatorze instalacji: **wybór funkcji** wybierz opcję **MySQL Workbench** w obszarze **aplikacji** sekcji.
3. Uruchom aplikację i Dodaj nowe połączenie przy użyciu ciągu połączenia danych z bazy danych MySQL na platformie Azure utworzonych na żebranie tego samouczka.
4. Po ustanowieniu połączenia, sprawdź **ASP.NET Identity** tabele utworzone na **IdentityMySQLDatabase.**
5. Zobaczysz, że wszystkie tożsamości ASP.NET wymagane tabele są tworzone, jak pokazano na poniższej ilustracji:  
  
 [Kliknij poniższy obraz, aby go rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Sprawdź **aspnetusers** tabeli, na przykład aby wyszukać wpisy, jak zarejestrować nowych użytkowników.  
  
 [Kliknij poniższy obraz, aby go rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
