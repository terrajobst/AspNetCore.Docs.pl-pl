---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Konfigurowanie serwera bazy danych dla sieci Web wdrażanie, publikowanie | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób konfigurowania serwera bazy danych programu SQL Server 2008 R2 do obsługi wdrożenia sieci web i publikowania. Zadania opisane w tym temacie są co...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: a2340c0d561ed274e281b5f6d942af0a2027315a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885577"
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a>Konfigurowanie serwera bazy danych dla publikowania narzędzia Web Deploy
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób konfigurowania serwera bazy danych programu SQL Server 2008 R2 do obsługi wdrożenia sieci web i publikowania.
> 
> W tym temacie opisano zadania są wspólne dla każdego scenariusza wdrażania&#x2014;nie ma znaczenia, czy serwerów sieci web są skonfigurowane do używania usługi agenta zdalnego Narzędzie wdrażania Web usług IIS (Web Deploy), program obsługi wdrażania w sieci Web lub wdrożenia w trybie offline lub z aplikacji Aplikacja jest uruchomiona na serwerze sieci web jednej lub farmy serwerów. Sposób wdrażania bazy danych mogą ulec zmianie w zależności od wymagań dotyczących zabezpieczeń oraz inne kwestie. Na przykład mogą wdrożyć bazę danych z lub bez przykładowych danych i może wdrożyć mapowania roli użytkownika lub ręcznie skonfigurować po wdrożeniu. Jednak sposób konfigurowania serwera bazy danych jest taka sama.


Nie trzeba zainstalować wszystkie dodatkowe produkty lub narzędzi do konfigurowania serwera bazy danych do obsługi wdrożenia sieci web. Przy założeniu, że serwer bazy danych oraz serwera sieci web, uruchom na różnych komputerach, musisz po prostu:

- Zezwolenie na program SQL Server do komunikowania się przy użyciu protokołu TCP/IP.
- Zezwalaj na ruch programu SQL Server za pośrednictwem wszelkie zapory.
- Należy podać konto komputera serwera sieci web logowania programu SQL Server.
- Mapowanie logowania konta komputera do żadnej roli wymaganej bazy danych.
- Należy podać konto, które będą uruchamiać wdrożenie uprawnień twórcy programu SQL Server, jak logowanie i bazy danych.
- Do obsługi wdrożeń powtarzania, mapy wdrażania logowania konta do **db\_właściciela** roli bazy danych.

W tym temacie opisano sposób wykonywania każdego z tych procedur. Zadania i wskazówki, w tym temacie założono zaczynasz z domyślnego wystąpienia programu SQL Server 2008 R2 z systemu Windows Server 2008 R2. Przed kontynuowaniem upewnij się, że:

- Windows Server 2008 R2 z dodatkiem Service Pack 1 i wszystkie dostępne aktualizacje są instalowane.
- Serwer jest przyłączony do domeny.
- Serwer ma statyczny adres IP.
- SQL Server 2008 R2 z dodatkiem Service Pack 1 i wszystkie dostępne aktualizacje są instalowane.

Wystąpienie programu SQL Server musi jedynie zawierać **usługi aparatu bazy danych** roli, która jest automatycznie uwzględnione w żadnej instalacji programu SQL Server. W celu ułatwienia konfiguracji i konserwacji, firma Microsoft zaleca jednak możesz uwzględnić **narzędzia do zarządzania — podstawowe** i **narzędzia do zarządzania — Complete** ról serwera.

> [!NOTE]
> Aby uzyskać więcej informacji dotyczących dołączania komputerów do domeny, zobacz [przyłączania komputerów do domeny i rejestrowanie na](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Aby uzyskać więcej informacji na temat konfigurowania statycznych adresów IP, zobacz [skonfigurować statyczny adres IP](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Aby uzyskać więcej informacji na temat instalowania programu SQL Server, zobacz [Instalowanie programu SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx).


## <a name="enable-remote-access-to-sql-server"></a>Włączenie dostępu zdalnego do programu SQL Server

Program SQL Server używa protokołu TCP/IP do komunikowania się z komputerów zdalnych. Jeśli serwer bazy danych i serwera sieci web znajdują się na różnych komputerach, musisz:

- Konfigurowanie ustawień sieciowych programu SQL Server, aby umożliwić komunikację za pośrednictwem protokołu TCP/IP.
- Skonfiguruj wszystkie zapory sprzętu lub oprogramowania, aby zezwolić na ruch TCP (i w niektórych przypadkach protokołu UDP (User Datagram) ruchu) na portach używanych przez wystąpienie programu SQL Server.

Aby włączyć program SQL Server do komunikowania się za pośrednictwem protokołu TCP/IP, należy użyć programu SQL Server Configuration Manager, aby zmienić konfigurację sieci dla wystąpienia programu SQL Server.

**Aby włączyć program SQL Server do komunikowania się przy użyciu protokołu TCP/IP**

1. Na **Start** menu wskaż **wszystkie programy**, kliknij przycisk **Microsoft SQL Server 2008 R2**, kliknij przycisk **narzędzia do konfiguracji**, a następnie kliknij przycisk **Programu SQL Server Configuration Manager**.
2. W okienku widoku drzewa rozwiń **konfigurację sieci programu SQL Server**, a następnie kliknij przycisk **protokoły dla elementu MSSQLSERVER**.

   > [!NOTE]
   > Jeśli zainstalowano wiele wystąpień programu SQL Server, zostanie wyświetlone <strong>protokoły dla</strong><em>[nazwa wystąpienia]</em> elementu dla każdego wystąpienia. Należy skonfigurować ustawienia sieci na podstawie wystąpienia przez wystąpienie.
3. W okienku szczegółów kliknij prawym przyciskiem myszy **TCP/IP** wiersza, a następnie kliknij przycisk **włączyć**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. W **ostrzeżenie** okno dialogowe, kliknij przycisk **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. Musisz ponownie uruchomić usługi MSSQLSERVER, zanim nowej konfiguracji sieci zostanie uwzględnione. Możesz to zrobić w wierszu polecenia z konsoli usługi lub SQL Server Management Studio. W tej procedurze użyjesz programu SQL Server Management Studio.
6. Zamykanie programu SQL Server Configuration Manager.
7. Na **Start** menu wskaż **wszystkie programy**, kliknij przycisk **Microsoft SQL Server 2008 R2**, a następnie kliknij przycisk **programu SQL Server Management Studio**.
8. W **Połącz z serwerem** okna dialogowego, **nazwy serwera** , wpisz nazwę serwera bazy danych, a następnie kliknij przycisk **Connect**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. W **Eksplorator obiektów** okienku kliknij prawym przyciskiem myszy węzeł serwera nadrzędnego (na przykład **TESTDB1**), a następnie kliknij przycisk **ponownego uruchomienia**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. W **Microsoft SQL Server Management Studio** okno dialogowe, kliknij przycisk **tak**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Po ponownym uruchomieniu usługi Zamknij program SQL Server Management Studio.

Aby zezwolić na ruch programu SQL Server przez zaporę, najpierw trzeba znać wystąpienia programu SQL Server używa portów. To zależy od sposobu utworzenia i skonfigurowania wystąpienia programu SQL Server:

- A *domyślnego wystąpienia* programu SQL Server nasłuchuje (i odpowiada) żądania na porcie TCP 1433.
- A *nazwane wystąpienie* programu SQL Server nasłuchuje (i odpowiada) żądania na porcie TCP przypisywany dynamicznie.
- Jeśli włączona jest usługa SQL Server Browser, klienci mogą wykonywać kwerendę usługi na porcie UDP 1434, aby dowiedzieć się, które port TCP dla danego wystąpienia programu SQL Server. Jednak ta usługa jest często wyłączone ze względów bezpieczeństwa.

Przy założeniu, że używasz domyślnego wystąpienia programu SQL Server, należy skonfigurować zaporę tak, aby zezwolić na ruch.

| Kierunek | Z portu | Do portu | Typ portu |
| --- | --- | --- | --- |
| Ruchu przychodzącego | wszystkie | 1433 | TCP |
| Wychodzące | 1433 | wszystkie | TCP |
  

> [!NOTE]
> Z technicznego punktu widzenia komputer kliencki użyje jeden losowo przypisany port TCP od 1024 do 5000 do komunikowania się z programem SQL Server i odpowiednio ograniczyć reguł zapory. Aby uzyskać więcej informacji na porty serwera SQL, a zaporach, zobacz [numery portów TCP/IP wymagane do komunikacji za pośrednictwem zapory SQL](https://go.microsoft.com/?linkid=9805125) i [porady: Konfigurowanie serwera do nasłuchiwania na konkretnym porcie TCP (SQL Server Configuration Menedżer)](https://msdn.microsoft.com/library/ms177440.aspx).


W większości środowisk systemu Windows Server prawdopodobnie musisz skonfigurować Zaporę systemu Windows na serwerze bazy danych. Domyślnie Zapora systemu Windows umożliwia ruch wychodzący, chyba że reguły. Aby włączyć serwer sieci web do bazy danych, musisz skonfigurować regułę ruchu przychodzącego, który umożliwia ruch TCP na numer portu, który korzysta z wystąpienia programu SQL Server. Jeśli używasz domyślnego wystąpienia programu SQL Server dalej procedura służy do konfigurowania tej reguły.

**Aby skonfigurować Zaporę systemu Windows, aby umożliwić komunikację z domyślnego wystąpienia programu SQL Server**

1. Na serwerze bazy danych na **Start** menu wskaż **narzędzia administracyjne**, a następnie kliknij przycisk **Zapora systemu Windows z zabezpieczeniami zaawansowanymi**.
2. W okienku widoku drzewa kliknij **reguły ruchu przychodzącego**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. W **akcje** okienku w obszarze **reguły ruchu przychodzącego**, kliknij przycisk **nową regułę**.
4. W Kreatora nowej reguły przychodzącej na **typ reguły** wybierz pozycję **portu**, a następnie kliknij przycisk **dalej**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. Na **protokoły i porty** strony, upewnij się, że **TCP** jest zaznaczone oraz w **określone porty lokalne** wpisz **1433**, a następnie kliknij przycisk **Dalej**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. Na **akcji** zostaw **zezwalały na połączenie** zaznaczone, a następnie kliknij przycisk **dalej**.
7. Na **profilu** zostaw **domeny** zaznaczone, wyczyść **prywatnej** i **publicznego** pola wyboru, a następnie kliknij przycisk **Następny**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. Na **nazwa** strony, Nadaj regule nazwę opisową odpowiednio (na przykład **wystąpienia domyślnego programu SQL Server — dostępu do sieci**), a następnie kliknij przycisk **Zakończ**.

Aby uzyskać więcej informacji na temat konfigurowania Zapory systemu Windows dla programu SQL Server, zwłaszcza w przypadku, gdy potrzebne do komunikowania się z programem SQL Server za pośrednictwem portów niestandardowych lub dynamicznej, zobacz [porady: Konfigurowanie Zapory systemu Windows dla dostępu aparatu bazy danych](https://technet.microsoft.com/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Konfigurowanie logowania i bazy danych uprawnienia

Podczas wdrażania aplikacji sieci web do programu Internet Information Services (IIS), aplikacja zostanie uruchomiona przy użyciu tożsamości puli aplikacji. W środowisku domeny tożsamości puli aplikacji, użyj konta komputera serwera, na którym uruchomiony do dostępu do zasobów sieciowych. Konta komputera formę <em>[nazwa domeny]</em><strong>\</ strong ><em>[nazwa komputera]</em><strong>$</strong>&#x2014;na przykład <strong>FABRIKAM\TESTWEB1$</strong>. Aby zezwolić na dostęp do bazy danych w sieci przez aplikację sieci web, musisz:

- Dodaj dane logowania dla konta komputera serwera sieci web do wystąpienia programu SQL Server.
- Mapowanie logowania konta komputera do żadnej roli wymaganej bazy danych (zazwyczaj **db\_datareader** i **db\_datawriter**).

Aplikacja sieci web jest uruchomiona na farmie serwerów, a nie pojedynczego serwera, należy powtórzyć te procedury dla każdego serwera sieci web w farmie serwerów.

> [!NOTE]
> Aby uzyskać więcej informacji o tożsamości puli aplikacji i dostęp do zasobów sieciowych, zobacz [tożsamości puli aplikacji](https://go.microsoft.com/?linkid=9805123).


Można osiągają te zadania na różne sposoby. Aby utworzyć nazwy logowania, można:

- Ręczne tworzenie danych logowania na serwerze bazy danych przy użyciu języka Transact-SQL lub SQL Server Management Studio.
- Użyj Projekt serwera SQL Server 2008 w programie Visual Studio do tworzenia i wdrażania logowania.

Logowania programu SQL Server jest obiektem poziom serwera, a nie obiektem poziomu bazy danych, więc nie jest zależna od bazy danych, którą chcesz wdrożyć. Tak można utworzyć nazwy logowania w dowolnym momencie i najprostszym rozwiązaniem jest często logowanie ręcznie utworzyć na serwerze bazy danych przed rozpoczęciem wdrażania baz danych. Następna procedura umożliwia utworzenie nazwy logowania w programie SQL Server Management Studio.

**Aby utworzyć identyfikator logowania programu SQL Server dla konta komputera serwera sieci web**

1. Na serwerze bazy danych na **Start** menu wskaż **wszystkie programy**, kliknij przycisk **Microsoft SQL Server 2008 R2**, a następnie kliknij przycisk **SQL Server Management Studio** .
2. W **Połącz z serwerem** okna dialogowego, **nazwy serwera** , wpisz nazwę serwera bazy danych, a następnie kliknij przycisk **Connect**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. W **Eksplorator obiektów** okienku kliknij prawym przyciskiem myszy **zabezpieczeń**, wskaż polecenie **nowy**, a następnie kliknij przycisk **logowania**.
4. W **logowania — nowy** okna dialogowego, **nazwa logowania** wpisz nazwę konta komputera serwera sieci web (na przykład **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. Kliknij przycisk **OK**.

W tym momencie serwer bazy danych jest gotowy do publikowania narzędzia Web Deploy. Jednak wdrażanego rozwiązań nie będzie działać, dopóki mapowania nazwy logowania konta komputera do ról bazy danych wymagane. Mapowania nazwy logowania do ról bazy danych wymaga dużo więcej traktować, jako użytkownik nie ról mapy dopiero po wdrożeniu bazy danych. Aby mapować logowania konta komputera do ról wymaganej bazy danych, można:

- Przypisz role bazy danych do logowania się ręcznie, po wdrożeniu bazy danych po raz pierwszy.
- Aby przypisać role bazy danych do nazwy logowania za pomocą skryptu po wdrożeniu.

Aby uzyskać więcej informacji na temat automatyzacji tworzenia logowania i mapowania roli bazy danych, zobacz [wdrażanie członkostwo roli bazy danych do środowisk testowych](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Alternatywnie służy następnej procedury do mapowania logowania konta komputera do ról bazy danych wymagane ręcznie. Należy pamiętać, że nie można wykonać tej procedury do *po* wdrożeniu bazy danych.

**Aby mapować ról bazy danych do logowania konta komputera serwera sieci web**

1. Otwórz program SQL Server Management Studio jak poprzednio.
2. W **Eksplorator obiektów** okienku rozwiń **zabezpieczeń** węzła, rozwiń węzeł **logowania** węzeł, a następnie kliknij dwukrotnie ikonę logowania konta komputera (na przykład **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. W **właściwości logowania** okno dialogowe, kliknij przycisk **mapowania użytkowników**.
4. W **użytkownicy zamapowani do tego logowania** tabeli, wybierz nazwę bazy danych (na przykład **ContactManager**).
5. W **członkostwo roli dla bazy danych:** *[Nazwa bazy danych]* wybierz wymagane uprawnienia. W przypadku kontaktów Menedżerze przykładowe rozwiązanie, należy wybrać **db\_datareader** i **db\_datawriter** ról.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. Kliknij przycisk **OK**.

Ręcznie mapowania ról bazy danych często jest większe niż odpowiednie dla środowisk testowych, jest mniej pożądana wdrożeń automatyczna lub z jednym kliknięciem w środowiskach, w tymczasowym czy produkcyjnym. Można znaleźć więcej informacji na temat automatyzacji tego rodzaju zadań za pomocą skryptów po wdrożeniu w [wdrażanie członkostwo roli bazy danych do środowisk testowych](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Aby uzyskać więcej informacji na serwer i projektów bazy danych, zobacz [projektów dodatku programu SQL Server bazy danych dla programu Visual Studio 2010](https://msdn.microsoft.com/library/ff678491.aspx).


## <a name="configure-permissions-for-the-deployment-account"></a>Skonfiguruj uprawnienia dla konta wdrożenia

Jeśli konto, które będzie używane do uruchamiania wdrożenia nie jest administratorem programu SQL Server, należy także utworzyć danych logowania dla tego konta. Aby można było utworzyć bazę danych, konto musi być członkiem **dbcreator** roli serwera lub mieć równoważne uprawnienia.

> [!NOTE]
> Użycie narzędzia Web Deploy lub VSDBCMD wdrażania bazy danych, mogą używać poświadczeń systemu Windows lub poświadczenia serwera SQL (jeśli wystąpienia programu SQL Server jest skonfigurowany do obsługi uwierzytelniania w trybie mieszanym). Następnej procedurze przyjęto założenie, że chcesz używać poświadczeń systemu Windows, ale nie ma nic zatrzymywanie możesz z określania programu SQL Server, nazwę użytkownika i hasło w ciągu połączenia podczas konfigurowania wdrożenia.


**Aby skonfigurować uprawnienia dla konta wdrożenia**

1. Otwórz program SQL Server Management Studio jak poprzednio.
2. W **Eksplorator obiektów** okienku kliknij prawym przyciskiem myszy **zabezpieczeń**, wskaż polecenie **nowy**, a następnie kliknij przycisk **logowania**.
3. W **logowania — nowy** okna dialogowego, **nazwa logowania** wpisz nazwę konta wdrażania (na przykład **FABRIKAM\matt**).
4. W **wybierz stronę** okienku, kliknij przycisk **ról serwera**.
5. Wybierz **dbcreator**, a następnie kliknij przycisk **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Do obsługi kolejnych wdrożeń, również należy dodać konto wdrażanie **db\_właściciela** roli bazy danych po ich pierwszym wdrożeniu. Jest tak, ponieważ na kolejne wdrożenia w przypadku modyfikowania schematu istniejącej bazy danych, zamiast tworzenia nowej bazy danych. Zgodnie z opisem w poprzedniej sekcji, nie można dodać użytkownika do roli bazy danych, aż po utworzeniu bazy danych, ze względów oczywiste.

**Do mapowania nazwy logowania konta wdrażania bazy danych\_właściciela roli bazy danych**

1. Otwórz program SQL Server Management Studio jak poprzednio.
2. W **Eksplorator obiektów** okna, rozwiń węzeł **zabezpieczeń** węzła, rozwiń węzeł **logowania** węzeł, a następnie kliknij dwukrotnie ikonę logowania konta komputera (na przykład **FABRIKAM\matt**).
3. W **właściwości logowania** okno dialogowe, kliknij przycisk **mapowania użytkowników**.
4. W **użytkownicy zamapowani do tego logowania** tabeli, wybierz nazwę bazy danych (na przykład **ContactManager**).
5. W **członkostwo roli dla bazy danych:** *[Nazwa bazy danych]* listy, wybierz **db\_właściciela** roli.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. Kliknij przycisk **OK**.

## <a name="conclusion"></a>Wniosek

Serwer bazy danych powinno być teraz gotowy do akceptowania wdrożeń zdalnej bazy danych i umożliwia zdalne serwery sieci web usług IIS można uzyskać dostępu do baz danych. Przed przystąpieniem do wdrażania i korzystać z baz danych, można sprawdzić punkty klucza:

- Czy skonfigurowano program SQL Server do akceptowania połączeń TCP/IP zdalny?
- Czy skonfigurowano wszelkie zapory zezwalającą na ruch programu SQL Server?
- Czy utworzono logowania konta komputera, dla każdego serwera sieci web, które będą uzyskiwać dostęp do programu SQL Server?
- Wdrożenie bazy danych zawiera skrypt umożliwiający utworzenie mapowania roli użytkownika, czy należy utworzyć je ręcznie, po wdrożeniu bazy danych po raz pierwszy?
- Możesz utworzono logowania dla konta wdrożenia i dodać go do **dbcreator** roli serwera?

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące wdrażania projektów bazy danych, zobacz [wdrażania projektów bazy danych](../web-deployment-in-the-enterprise/deploying-database-projects.md). Aby uzyskać wskazówki dotyczące tworzenia członkostwo roli bazy danych, uruchamiając skrypt po wdrożeniu, zobacz [wdrażanie członkostwo roli bazy danych do środowisk testowych](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Aby uzyskać wskazówki dotyczące sposobu spełnia wyzwania unikatowego wdrożenia, które stanowią bazy danych członkostwa, zobacz [wdrażanie baz danych członkostwa w środowiskach przedsiębiorstw](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

> [!div class="step-by-step"]
> [Poprzednie](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [dalej](creating-a-server-farm-with-the-web-farm-framework.md)
