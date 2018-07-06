---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Implementowanie dostawcy magazynu MySQL niestandardowe produktu ASP.NET Identity | Dokumentacja firmy Microsoft
author: raquelsa
description: ASP.NET Identity jest rozszerzalny system, co pozwala na tworzenie własnego dostawcę magazynu i podłącz go do aplikacji bez ponownego pracy likacji...
ms.author: aspnetcontent
ms.date: 05/22/2015
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: e5784991a95dcff0db38d87707bc83555da716df
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805678"
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Implementowanie dostawcy magazynu MySQL niestandardowe produktu ASP.NET Identity
====================
przez [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity jest rozszerzalny system, co pozwala na tworzenie własnego dostawcę magazynu i podłącz go do aplikacji bez ponownego pracy aplikacji. W tym temacie opisano sposób tworzenia dostawcy magazynu MySQL dla produktu ASP.NET Identity. Aby zapoznać się z omówieniem Tworzenie niestandardowych dostawców magazynu, zobacz [przegląd z niestandardowych dostawców magazynu dla produktu ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Do ukończenia tego samouczka, musi mieć programu Visual Studio 2013 z aktualizacją Update 2.
> 
> W tym samouczku wykonują następujące czynności:
> 
> - W tym temacie opisano tworzenie wystąpienia bazy danych MySQL na platformie Azure.
> - Pokazują, jak tworzyć tabele oraz zarządzanie zdalne bazy danych na platformie Azure za pomocą narzędzia klienta MySQL (MySQL Workbench).
> - Pokazują, jak zastąpić domyślną implementację magazynu tożsamości ASP.NET przy użyciu naszej implementacji niestandardowych w projekcie aplikacji MVC.
> 
> W tym samouczku pierwotnie został napisany przez Raquel Soares De Almeida i Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ). Przykładowy projekt został zaktualizowany na potrzeby tożsamości w wersji 2.0, Suhas Joshi. Temat został zaktualizowany na potrzeby tożsamości w wersji 2.0, Tom FitzMacken.


## <a name="download-completed-project"></a>Zakończono pobieranie projektu

Na końcu tego samouczka będziesz mieć projekt aplikacji MVC w produkcie ASP.NET Identity Praca z bazy danych MySQL hostowanej na platformie Azure.

Możesz pobrać zakończone dostawcy magazynu MySQL w [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).

## <a name="the-steps-you-will-perform"></a>Kroki, które należy wykonać

W tym samouczku wykonasz następujące czynności:

1. Tworzenie bazy danych MySQL na platformie Azure
2. Tworzenie tabel w bazie danych MySQL produktu ASP.NET Identity
3. Tworzenie aplikacji MVC i skonfigurować go do używania dostawcy bazy danych MySQL
4. Uruchamianie aplikacji

W tym temacie nie opisano architektury produktu ASP.NET Identity i decyzje, które należy wykonać podczas implementowania dostawcy magazynu klienta. Aby uzyskać te informacje, zobacz [przegląd z niestandardowego dostawcy magazynu dla produktu ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Przegląd klasy dostawcy magazynu MySQL

Zanim przejdziemy do kroki, aby utworzyć dostawcy magazynu MySQL, Przyjrzyjmy się klasy, które tworzą dostawcę magazynu. Konieczne będzie klas, które zarządzają operacji bazy danych i klas, które są wywoływane z poziomu aplikacji do zarządzania użytkownikami i rolami.

### <a name="storage-classes"></a>Klasy magazynu

- [IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) — zawiera właściwości użytkownika.
- [Magazynie UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) — zawiera operacje dodawania, aktualizowania i pobierania użytkowników.
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) — zawiera właściwości dla ról.
- [Elemencie RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) — zawiera operacje dodawania, usuwania, aktualizowania i pobierania ról.

### <a name="data-access-layer-classes"></a>Klasy warstwy dostępu do danych

W tym przykładzie klasy warstwy dostępu do danych zawierają instrukcje SQL do pracy z tabel. Jednak w kodzie można używać mapowania obiektowo relacyjny (ORM), takich jak Entity Framework lub NHibernate. W szczególności aplikacji może wystąpić spadek wydajności bez ORM, obejmującą powolne ładowanie i buforowania obiektów. Aby uzyskać więcej informacji, zobacz [bez programu Entity Framework w wersji 2.0 tożsamości ASP.NET?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) — zawiera metody służące do wykonywania operacji w bazie danych i połączenie z bazą danych MySQL. Magazynie UserStore i elemencie RoleStore są zarówno tworzone przy użyciu wystąpienia tej klasy.
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) — zawiera operacje bazy danych dla tabeli, która przechowuje role.
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) — zawiera operacje bazy danych dla tabeli, która przechowuje oświadczenia użytkownika.
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) — zawiera operacje bazy danych dla tabeli, która przechowuje dane logowania użytkownika.
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) — zawiera operacje bazy danych dla tabeli, który przechowuje, czyli użytkownicy przypisani do ról.
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) — zawiera operacje bazy danych dla tabeli, która przechowuje użytkowników.

## <a name="create-a-mysql-database-instance-on-azure"></a>Tworzenie wystąpienia bazy danych MySQL na platformie Azure

1. Zaloguj się do [witryny Azure Portal](https://manage.windowsazure.com/).
2. Kliknij przycisk **+ nowy** w dolnej części strony, a następnie wybierz **MAGAZYNU**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. W **wybierz i dodatek** kreatora wybierz **bazy danych MySQL ClearDB** i kliknij strzałkę dalej w prawym dolnym rogu okna dialogowego.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Zachowaj wartość domyślną **bezpłatna** planowania i zmienić **nazwa** do **IdentityMySQLDatabase**. Wybierz region najbliższą, a następnie kliknij strzałkę dalej.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Kliknij znacznik wyboru, aby zakończyć tworzenie bazy danych.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Po utworzeniu bazy danych można zarządzać nim z **dodatki** karta w portalu zarządzania.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Informacje o połączeniu bazy danych można uzyskać, klikając **informacje o połączeniu** w dolnej części strony.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Skopiuj parametry połączenia, klikając przycisk kopiowania i zapisz go, aby można było używać później w aplikacji MVC.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>Tworzenie produktu ASP.NET Identity tabel w bazie danych MySQL

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Zainstaluj narzędzie MySQL Workbench do nawiązywania połączeń i zarządzania bazą danych MySQL

1. Zainstaluj **połączenia aplikacji MySQL Workbench** narzędzia z [strony pobierania MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Uruchom aplikację i dodać kliknij na **MySQLConnections +** przycisk, aby dodać nowe połączenie. Przy użyciu danych parametry połączenia skopiowane z bazy danych Azure MySQL utworzonej we wcześniejszej części tego samouczka.
3. Po ustanowieniu połączenia, otwórz nowy **zapytania** karcie; Wklej poleceń z [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) do zapytania i uruchomić go w celu tworzenia tabel bazy danych.
4. Masz teraz wszystkie produktu ASP.NET Identity niezbędne tabele utworzone w bazie danych programu MySQL hostowanej na platformie Azure, jak pokazano poniżej.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Tworzenie projektu aplikacji MVC za pomocą szablonu i skonfigurować go do używania dostawcy bazy danych MySQL

Jeśli to konieczne, zainstaluj albo [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) z aktualizacją Update 2.

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>Pobieranie projektu ASP.NET.Identity.MySQL z witryny CodePlex

1. Przejdź do adresu URL repozytorium na [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).
2. Pobierz kod źródłowy.
3. Wyodrębnij plik zip do folderu lokalnego.
4. Otwórz rozwiązanie AspNet.Identity.MySQL i skompiluj je.

### <a name="create-a-new-mvc-application-project-from-template"></a>Utwórz nowy projekt aplikacji MVC za pomocą szablonu

1. Kliknij prawym przyciskiem myszy **AspNet.Identity.MySQL** rozwiązania i **Dodaj**, **nowy projekt**
2. W **Dodaj nowy projekt** okna dialogowego wybierz **Visual C#** po lewej stronie, następnie **Web** , a następnie wybierz **aplikacji sieci Web ASP.NET**. Nazwij swój projekt **IdentityMySQLDemo**; a następnie kliknij przycisk OK.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. W **nowy projekt ASP.NET** okno dialogowe, wybierz szablon MVC, z opcjami domyślnymi (który zawiera **indywidualne konta użytkowników** jako metodę uwierzytelniania) i kliknij przycisk **OK** .![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt IdentityMySQLDemo, a następnie wybierz **Zarządzaj pakietami NuGet**. W oknie dialogowym pole tekstowe wyszukiwania wpisz **Identity.EntityFramework**. Wybierz ten pakiet na liście wyników i kliknij przycisk **Odinstaluj**. Zostanie wyświetlony monit Odinstaluj pakiet zależności platformy EntityFramework. Kliknij Tak, jak firma Microsoft nie będą już ten pakiet w tej aplikacji.
5. Kliknij prawym przyciskiem myszy projekt IdentityMySQLDemo, wybierz **Dodaj**, **odwołania, rozwiązania, projekty;** zaznacz projekt AspNet.Identity.MySQL, a następnie kliknij przycisk **OK**.
6. W projekcie IdentityMySQLDemo Zastąp wszystkie odwołania do  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   with  
     `using AspNet.Identity.MySQL;`
7. W IdentityModels.cs, ustaw **ApplicationDbContext** wyprowadzenia z **MySqlDatabase** i zawierać konstruktora, który przyjmować jeden parametr o nazwie połączenia.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Otwórz plik IdentityConfig.cs. W **ApplicationUserManager.Create** metody, Zastąp wystąpienia menedżera UserManager następującym kodem:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Otwórz plik web.config i Zastąp ciąg DefaultConnection ten wpis, zastępując wartości wyróżnione parametry połączenia bazy danych MySQL, który został utworzony w poprzednich krokach:  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Uruchom aplikację i Połącz z bazą danych MySQL

1. Kliknij prawym przyciskiem myszy **IdentityMySQLDemo** projektu, a następnie wybierz **Ustaw jako projekt startowy**
2. Naciśnij klawisz **klawiszy Ctrl + F5** Aby skompilować i uruchomić aplikację.
3. Kliknij pozycję **zarejestrować** kartę w górnej części strony.
4. Wprowadź nową nazwę użytkownika i hasło, a następnie kliknij polecenie **zarejestrować**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. Nowy użytkownik jest teraz zarejestrowany i zalogowany.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Wróć do narzędzia MySQL Workbench i sprawdzić **IdentityMySQLDatabase** zawartości tabeli. Tabela użytkowników zapisów należy sprawdzić, jak zarejestrować nowych użytkowników.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji o sposobie włączania innych metod uwierzytelniania, w tej aplikacji, zapoznaj się [tworzenie aplikacji platformy ASP.NET MVC 5 za pomocą usługi Facebook i Google OAuth2 i OpenID logowanie jednokrotne](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Aby dowiedzieć się, jak zintegrować swoje bazy danych z uwierzytelnianiem OAuth i konfigurowanie ról, aby ograniczyć dostęp użytkowników do aplikacji, zobacz [wdrażanie aplikacji bezpiecznego ASP.NET MVC 5 z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL na platformie Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
