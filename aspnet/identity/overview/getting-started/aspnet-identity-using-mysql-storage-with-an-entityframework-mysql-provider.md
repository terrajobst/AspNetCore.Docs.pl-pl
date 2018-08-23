---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: Używanie usługi MySQL Storage przy użyciu dostawcy MySQL programu EntityFramework (C#) | Dokumentacja firmy Microsoft'
author: maumar
description: W tym samouczku dowiesz się, jak zastąpić domyślny mechanizm magazynu danych dla produktu ASP.NET Identity dostarczyć MySQL za pomocą platformy EntityFramework (Dostawca klienta SQL)...
ms.author: riande
ms.date: 12/10/2013
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 3856b40b31a3deb6ad1c6c5d2cd678e183f012d7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755652"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity: Używanie usługi MySQL Storage z dostawcą MySQL programu EntityFramework (C#)
====================
przez [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)

> W tym samouczku dowiesz się, jak zastąpić domyślny mechanizm magazynu danych dla [ **produktu ASP.NET Identity** ](introduction-to-aspnet-identity.md) za pomocą platformy EntityFramework (Dostawca klienta SQL) przy użyciu dostawcy bazy danych MySQL.


Poniższe tematy zostały omówione w tym samouczku:

- Tworzenie bazy danych MySQL na platformie Azure
- Tworzenie aplikacji MVC przy użyciu szablonu programu Visual Studio 2013 MVC
- Konfigurowanie platformy EntityFramework, aby pracować z dostawcą bazy danych MySQL
- Uruchamianie aplikacji, aby zweryfikować wyniki

Na końcu tego samouczka konieczne będzie aplikacji MVC w produkcie ASP.NET Identity przechowywania korzystającej z bazy danych MySQL, która jest hostowana na platformie Azure.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Tworzenie wystąpienia bazy danych MySQL na platformie Azure

1. Zaloguj się do [witryny Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Kliknij przycisk **NEW** w dolnej części strony, a następnie wybierz **MAGAZYNU**:  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. W **wybierz i dodatek** kreatora wybierz **bazy danych MySQL ClearDB**, a następnie kliknij przycisk **dalej** strzałkę na końcu ramki:  
  
   [Kliknij poniższy obraz, aby ją rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Zachowaj wartość domyślną **bezpłatna** planowanie, zmień **nazwa** do **IdentityMySQLDatabase**, wybierz region, który jest najbliżej Twojej lokalizacji, a następnie kliknij przycisk **dalej** strzałkę na końcu ramki:  
  
   [Kliknij poniższy obraz, aby ją rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Kliknij przycisk **zakupu** znacznik wyboru, aby zakończyć tworzenie bazy danych.  
  
   [Kliknij poniższy obraz, aby ją rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Po utworzeniu bazy danych można zarządzać nim z **dodatki** karta w portalu zarządzania. Aby uzyskać informacje o połączeniu dla bazy danych, kliknij przycisk **informacje o połączeniu** w dolnej części strony:  
  
   [Kliknij poniższy obraz, aby ją rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Skopiuj parametry połączenia, klikając przycisk kopiowania, **CONNECTIONSTRING** pola, a następnie zapisz go; użyje tych informacji w dalszej części tego samouczka dla aplikacji MVC:  
  
   [Kliknij poniższy obraz, aby ją rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Tworzenie projektu aplikacji MVC

Aby wykonać kroki opisane w tej części samouczka, najpierw należy zainstalować [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Po zainstalowaniu programu Visual Studio umożliwia utworzenie nowego projektu aplikacji MVC następujące czynności:

1. Otwórz program Visual Studio 2103.
2. Kliknij przycisk **nowy projekt** z **Start** strony, lub kliknij przycisk **pliku** menu i następnie **nowy projekt**:  
  
   [Kliknij poniższy obraz, aby ją rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Gdy **nowy projekt** zostanie wyświetlone okno dialogowe, rozwiń **Visual C#** na liście szablonów, następnie kliknij przycisk **sieci Web**i wybierz **aplikacji sieci Web ASP.NET**. Nazwij swój projekt **IdentityMySQLDemo** a następnie kliknij przycisk **OK**:  
  
   [Kliknij poniższy obraz, aby ją rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **MVC** templatewith domyślne opcje; spowoduje skonfigurować **indywidualne konta użytkowników** jako metodę uwierzytelniania. Kliknij przycisk **OK**:  
  
   [Kliknij poniższy obraz, aby ją rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Konfigurowanie platformy EntityFramework do pracy z bazą danych MySQL

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Aktualizacja zestawu platformy Entity Framework, dla Twojego projektu

Aplikacja MVC, który został utworzony z szablonu programu Visual Studio 2013 zawiera odwołanie do [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) pakietu, ale ma zostało do tego zestawu, ponieważ jego wersja aktualizacji, które zawierają istotne poprawa wydajności. Aby można było używać tych najnowsze aktualizacje w aplikacji, wykonaj następujące kroki.

1. Otwórz swój projekt MVC w programie Visual Studio 2013.
2. Kliknij przycisk **narzędzia**, następnie kliknij przycisk **Menedżer pakietów biblioteki**, a następnie kliknij przycisk **Konsola Menedżera pakietów**:  
  
   [Kliknij poniższy obraz, aby ją rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. **Konsola Menedżera pakietów** pojawi się w dolnej części programu Visual Studio. Typ &quot; **pakietu aktualizacji EntityFramework** &quot; i naciśnij klawisz Enter:  
  
   [Kliknij poniższy obraz, aby ją rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Instalowanie dostawcy bazy danych MySQL na platformie EntityFramework

EntityFramework do łączenia z bazą danych MySQL, należy zainstalować dostawcy bazy danych MySQL. Aby to zrobić, otwórz **Konsola Menedżera pakietów** i typ &quot; **MySql.Data.Entity Install-Package - Pre**&quot;, a następnie naciśnij klawisz Enter.

> [!NOTE]
> Jest to wstępna wersja zestawu i jako takie mogą zawierać błędy. Wstępna wersja dostawcy nie należy używać w środowisku produkcyjnym.


[Kliknij poniższy obraz, aby ją rozwinąć.]  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Wprowadzanie zmian konfiguracji projektu do pliku Web.config aplikacji

W tej sekcji skonfigurujesz Entity Framework do użycia dostawcy bazy danych MySQL, który został zainstalowany należy zarejestrować fabryki dostawców MySQL i Dodaj parametry połączenia z platformy Azure.

> [!NOTE]
> Poniższe przykłady zawierają określony zestaw wersji do MySql.Data.dll. Wersja zestawu zmienia się, należy zmodyfikować ustawienia odpowiednia konfiguracja odpowiedniej wersji.


1. Otwórz plik Web.config dla projektu w programie Visual Studio 2013.
2. Znajdź poniższe ustawienia konfiguracji, które definiują domyślny dostawca bazy danych i fabryki dla programu Entity Framework:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Zastąp te ustawienia konfiguracji poniższych składników, które umożliwia skonfigurowanie programu Entity Framework do użycia dostawcy bazy danych MySQL: 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Znajdź &lt;connectionStrings&gt; sekcji i zastąp go następującym kodem, który określa parametry połączenia dla bazy danych MySQL hostowanej na platformie Azure (należy pamiętać, że wartość providerName również została zmieniona z oryginalne):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Dodawanie niestandardowych MigrationHistory kontekstu

Korzysta z programu Entity Framework Code First **MigrationHistory** tabeli, aby śledzić zmiany modelu i w celu zapewnienia spójności między schemat bazy danych i schematu koncepcyjnego. Jednak ta tabela nie działa dla MySQL domyślnie klucz podstawowy jest za duży. Aby rozwiązać ten problem, należy zmniejszyć rozmiar klucza dla tej tabeli. Aby to zrobić, wykonaj następujące kroki:

1. Informacje o schemacie dla tej tabeli jest przechwytywane w **HistoryContext**, który może być zmodyfikowane, jak każdy inny **DbContext**. Aby to zrobić, Dodaj plik klasy o nazwie **MySqlHistoryContext.cs** do projektu i zastąp jego zawartość następującym kodem:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Następnie konieczne będzie Konfigurowanie platformy Entity Framework, aby użyć zmodyfikowanego **HistoryContext**, zamiast domyślna. Można to zrobić dzięki wykorzystaniu funkcji konfiguracji na podstawie kodu. Aby to zrobić, Dodaj nowy plik klasy o nazwie **MySqlConfiguration.cs** do projektu i zastąp jego zawartość, za pomocą:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Tworzenie niestandardowych inicjator EntityFramework ApplicationDbContext

Dostawcy bazy danych MySQL, która jest dostępna w ramach tego samouczka nie obsługuje obecnie migracje platformy Entity Framework, dlatego należy używać inicjatory modelu w celu połączenia z bazą danych. Ponieważ w tym samouczku korzysta z wystąpienia programu MySQL na platformie Azure, należy utworzyć niestandardowe inicjatora programu Entity Framework.

> [!NOTE]
> Ten krok nie jest wymagane, jeśli łączysz się z wystąpieniem programu SQL Server na platformie Azure lub jeśli używasz bazy danych, która jest zainstalowana lokalnie.


Aby utworzyć niestandardowe inicjatora programu Entity Framework for MySQL, użyj następujących kroków:

1. Dodaj plik klasy o nazwie **MySqlInitializer.cs** i Zastąp projektu jest zawartość następującym kodem: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Otwórz **IdentityModels.cs** w pliku projektu, który znajduje się w **modeli** katalogu i zastąp jego zawartość następujących czynności: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Uruchamianie aplikacji i weryfikowanie bazy danych

Po zakończeniu czynności opisane w poprzednich sekcjach, należy przetestować bazy danych. Aby to zrobić, wykonaj następujące kroki:

1. Naciśnij klawisz **klawiszy Ctrl + F5** do kompilowania i uruchamiania aplikacji sieci web.
2. Kliknij przycisk **zarejestrować** kartę w górnej części strony:  
  
   [Kliknij poniższy obraz, aby ją rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Wprowadź nową nazwę użytkownika i hasło, a następnie kliknij przycisk **zarejestrować**:  
  
   [Kliknij poniższy obraz, aby ją rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. W tym momencie tabele produktu ASP.NET Identity zostały utworzone w bazie danych MySQL, a użytkownik jest zarejestrowany i zalogowaniu się do aplikacji:  
  
   [Kliknij poniższy obraz, aby ją rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Instalowanie aplikacji MySQL Workbench narzędzia do sprawdzania danych

1. Zainstaluj **połączenia aplikacji MySQL Workbench** narzędzia z [strony pobierania MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. W Kreatorze instalacji: **wybór funkcji** zaznacz **połączenia aplikacji MySQL Workbench** w obszarze **aplikacje** sekcji.
3. Uruchom aplikację i dodać nowe połączenie przy użyciu połączenia danych ciągu z bazy danych Azure MySQL utworzonej na żebranie w tym samouczku.
4. Po ustanowieniu połączenia, sprawdź **produktu ASP.NET Identity** tabele utworzone na **IdentityMySQLDatabase.**
5. Zobaczysz, że wszystkie tożsamości ASP.NET wymagane tabele zostały utworzone, jak pokazano na poniższej ilustracji:  
  
   [Kliknij poniższy obraz, aby ją rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Sprawdzanie **aspnetusers** tabeli dla wystąpienia pod kątem zapisów i rejestrowania nowych użytkowników.  
  
   [Kliknij poniższy obraz, aby ją rozwinąć. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
