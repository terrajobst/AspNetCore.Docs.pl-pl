---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: "Implementacja dostawcy magazynu tożsamości ASP.NET MySQL niestandardowych | Dokumentacja firmy Microsoft"
author: raquelsa
description: "ASP.NET Identity jest rozszerzalny system, co pozwala na tworzenie własnego dostawcę magazynu i podłącz go do aplikacji bez ponownego pracy nazwę..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: 3bfbccd91705755fc24bb8305fff171baa26f370
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Implementacja dostawcy magazynu tożsamości ASP.NET MySQL niestandardowych
====================
przez [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [FitzMacken niestandardowy](https://github.com/tfitzmac)

> ASP.NET Identity jest rozszerzalny system, co pozwala na tworzenie własnego dostawcę magazynu i podłącz go do aplikacji bez ponownego pracy aplikacji. W tym temacie opisano sposób tworzenia dostawcy magazynu MySQL dla tożsamości ASP.NET. Omówienie tworzenia dostawców niestandardowych magazynu, zobacz [omówienie z niestandardowego dostawcy magazynu dla tożsamości ASP.NET](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Do ukończenia tego samouczka, musi mieć programu Visual Studio 2013 Update 2.
> 
> W tym samouczku obejmują:
> 
> - Pokazują, jak utworzyć wystąpienie bazy danych MySQL na platformie Azure.
> - W tym temacie opisano tworzenie tabel i zarządzanie nimi zdalnej bazy danych na platformie Azure za pomocą narzędzia klienta MySQL (MySQL Workbench).
> - Pokaż jak zastąpić domyślny Wdrażanie magazynu ASP.NET Identity naszych implementacja niestandardowa na projekt aplikacji MVC.
> 
> W tym samouczku pierwotnie zapisał Raquel Soares De Almeida i Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ). Przykładowy projekt został zaktualizowany na potrzeby 2.0 tożsamości przez Suhas Joshi. Temat został zaktualizowany na potrzeby 2.0 tożsamości przez FitzMacken niestandardowego.


## <a name="download-completed-project"></a>Pobieranie ukończone projektu

Na końcu tego samouczka należy projekt aplikacji dla platformy MVC ASP.NET Identity pracy z bazy danych MySQL hostowanej na platformie Azure.

Możesz pobrać ukończone dostawcy magazynu MySQL w [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).

## <a name="the-steps-you-will-perform"></a>Czynności, które ma zostać wykonane

W tym samouczku obejmują:

1. Utwórz bazę danych MySQL na platformie Azure
2. Tworzenie tabel ASP.NET Identity w MySQL
3. Tworzenie aplikacji MVC i skonfigurować go do używania dostawcy programu MySQL
4. Uruchamianie aplikacji

W tym temacie nie opisano architektury ASP.NET Identity i decyzje, które należy wykonać podczas implementowania dostawcy magazynu klienta. Informacje, zobacz [omówienie z niestandardowego dostawcy magazynu dla tożsamości ASP.NET](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Przegląd klasy dostawcy magazynu MySQL

Przed przeskakiwanie do kroki, aby utworzyć dostawcę magazynu MySQL, Przyjrzyjmy się klasy, które tworzą dostawcę magazynu. Konieczne będzie klas, które zarządzają operacje bazy danych i klasy, które są nazywane z aplikacji do zarządzania użytkownikami i rolami.

### <a name="storage-classes"></a>Klasy magazynu

- [IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) — zawiera właściwości dla użytkownika.
- [Magazynie UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) — zawiera operacje dodawania, aktualizowania i pobierania użytkowników.
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) — zawiera właściwości dla ról.
- [Elemencie RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) — zawiera operacje dodawania, usuwania, aktualizowania i pobierania ról.

### <a name="data-access-layer-classes"></a>Klasy warstwy dostępu do danych

Na przykład klasy warstwy dostępu do danych zawierają instrukcje SQL do pracy z tabel. Jednak w kodzie można używać mapowania obiektów relacyjnych (ORM) takie jak Entity Framework lub NHibernate. W szczególności aplikacji mogą wystąpić pogorszenie wydajności bez ORM opóźnionego ładowania i buforowania obiektów. Aby uzyskać więcej informacji, zobacz [ASP.NET 2.0 tożsamości bez programu Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) — zawiera połączenia z bazą danych MySQL i metody do wykonywania operacji bazy danych. Magazynie UserStore i elemencie RoleStore są oba utworzona przy użyciu wystąpienia tej klasy.
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) — zawiera operacje bazy danych dla tabeli, która przechowuje role.
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) — zawiera operacje bazy danych dla tabeli, która przechowuje oświadczenia użytkownika.
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) — zawiera operacje bazy danych dla tabeli, która przechowuje informacje logowania użytkownika.
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) — zawiera operacje bazy danych dla tabeli, która przechowuje użytkowników, którzy są przypisane do ról.
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) — zawiera operacje bazy danych dla tabeli, która przechowuje użytkowników.

## <a name="create-a-mysql-database-instance-on-azure"></a>Utwórz wystąpienie bazy danych MySQL na platformie Azure

1. Zaloguj się do [portalu Azure](https://manage.windowsazure.com/).
2. Kliknij przycisk **+ nowy** w dolnej części strony, a następnie wybierz **MAGAZYNU**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. W **wybierz i dodatek** kreatora wybierz **baza danych ClearDB MySQL** i kliknij przycisk Dalej strzałkę w prawym dolnym rogu okna dialogowego.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Zachowaj ustawienie domyślne **wolne** zaplanować i zmienić **nazwa** do **IdentityMySQLDatabase**. Wybierz region najbliższego, a następnie kliknij strzałkę dalej.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Kliknij znacznik wyboru, aby zakończyć tworzenie bazy danych.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Po utworzeniu bazy danych, można zarządzać nim z **dodatki** kartę w portalu zarządzania.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Informacje dotyczące połączenia bazy danych można uzyskać, klikając **informacje o połączeniu** w dolnej części strony.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Skopiuj parametry połączenia, klikając przycisk Kopiuj i zapisz go, dzięki czemu można użyć później w aplikacji MVC.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>Tworzenie tabel w bazie danych MySQL tożsamości platformy ASP.NET

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Zainstaluj narzędzia MySQL Workbench, aby połączyć i zarządzanie bazą danych MySQL

1. Zainstaluj **MySQL Workbench** narzędzia z [MySQL pobiera strony](http://dev.mysql.com/downloads/windows/installer/)
2. Uruchom aplikację i Dodaj kliknij na **MySQLConnections +** przycisk, aby dodać nowe połączenie. Użyj danych parametry połączenia skopiowane z bazy danych MySQL na platformie Azure, utworzony wcześniej w tym samouczku.
3. Po ustanowieniu połączenia, Otwórz nowe **zapytania** karcie; Wklej poleceń z [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) w zapytaniu i wykonaj go w celu tworzenia tabel bazy danych.
4. Masz teraz wszystkie ASP.NET Identity niezbędne tabele utworzone w bazie danych MySQL hostowanej na platformie Azure, jak pokazano poniżej.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Utwórz projekt aplikacji dla platformy MVC z szablonu i skonfigurować go do używania dostawcy MySQL

W razie potrzeby zainstaluj program [programu Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) z Update 2.

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>Pobierz projekt ASP.NET.Identity.MySQL w witrynie CodePlex

1. Przejdź do adresu URL repozytorium w [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).
2. Pobierz kod źródłowy.
3. Wyodrębnij plik zip do folderu lokalnego.
4. Otwórz rozwiązanie AspNet.Identity.MySQL i skompiluj go.

### <a name="create-a-new-mvc-application-project-from-template"></a>Utwórz nowy projekt aplikacji MVC z szablonu

1. Kliknij prawym przyciskiem myszy **AspNet.Identity.MySQL** rozwiązania i **Dodaj**, **nowy projekt**
2. W **Dodawanie nowego projektu** oknie dialogowym wybierz pozycję **Visual C#** po lewej stronie, następnie **Web** , a następnie wybierz **aplikacji sieci Web ASP.NET**. Nazwij swój projekt **IdentityMySQLDemo**; a następnie kliknij przycisk OK.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. W **nowy projekt ASP.NET** okna dialogowego, wybierz szablon MVC przy użyciu opcji domyślnej (który obejmuje **indywidualnych kont użytkowników** jako metody uwierzytelniania) i kliknij przycisk **OK** .![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt IdentityMySQLDemo i wybierz **Zarządzaj pakietami NuGet**. W oknie dialogowym pole wyszukiwania tekstu wpisz **Identity.EntityFramework**. Wybierz z listy wyników tego pakietu, a następnie kliknij przycisk **Odinstaluj**. Pojawi się monit dezinstalacji zależności pakietu platformy EntityFramework. Kliknij Tak, jak firma Microsoft nie będą już tego pakietu do tej aplikacji.
5. Kliknij prawym przyciskiem myszy projekt IdentityMySQLDemo, wybierz **Dodaj**, **odwołania, rozwiązania, projekty;** wybierz projekt AspNet.Identity.MySQL, a następnie kliknij przycisk **OK**.
6. W projekcie IdentityMySQLDemo Zamień wszystkie odwołania do  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
 with  
     `using AspNet.Identity.MySQL;`
7. W IdentityModels.cs, ustaw **ApplicationDbContext** pochodzić z **MySqlDatabase** i obejmują contructor, który przyjmować jeden parametr o nazwie połączenia.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Otwórz plik IdentityConfig.cs. W **ApplicationUserManager.Create** metoda, Zastąp uruchamianiu interfejs UserManager następującym kodem:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Otwórz plik web.config i Zastąp ciąg połączenia DefaultConnection tego wpisu, zastępując wartości wyróżnione parametry połączenia bazy danych MySQL utworzonej w poprzednich krokach:  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Uruchom aplikację i nawiąż połączenie z bazą danych MySQL

1. Kliknij prawym przyciskiem myszy **IdentityMySQLDemo** projekt i wybierz **Ustaw jako projekt startowy**
2. Naciśnij klawisz **Ctrl + F5** Aby skompilować i uruchomić aplikację.
3. Polecenie **zarejestrować** kartę w górnej części strony.
4. Wprowadź nową nazwę użytkownika i hasło, a następnie kliknij polecenie **zarejestrować**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. Nowy użytkownik jest teraz zarejestrowane i zalogowany.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Wróć do narzędzia MySQL Workbench i sprawdzić **IdentityMySQLDatabase** zawartości tabeli. Tabela użytkowników zapisów należy sprawdzić, jak zarejestrować nowych użytkowników.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o sposobie włączania innych metod uwierzytelniania, w tym aplikacji, zapoznaj się [tworzenie aplikacji ASP.NET MVC 5 z usługi Facebook i Google OAuth2 i OpenID logowania jednokrotnego](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Aby dowiedzieć się, jak zintegrować z bazy danych z OAuth i konfigurować role, aby ograniczyć dostęp użytkowników do aplikacji, zobacz [wdrażanie aplikacji bezpiecznego ASP.NET MVC 5 z członkostwa, OAuth i bazy danych SQL Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
