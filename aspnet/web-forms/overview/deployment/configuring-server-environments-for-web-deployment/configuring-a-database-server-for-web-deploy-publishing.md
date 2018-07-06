---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Konfigurowanie serwera bazy danych dla sieci Web wdrażanie, publikowanie | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób konfigurowania serwera bazy danych programu SQL Server 2008 R2 do obsługi wdrażania w Internecie i publikowania. Zadania opisane w tym temacie są co...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: 9ec938298ea63609c276aed807a67722bf9bd66d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841982"
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a>Konfigurowanie serwera bazy danych dla usługi publikowania Web Deploy
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób konfigurowania serwera bazy danych programu SQL Server 2008 R2 do obsługi wdrażania w Internecie i publikowania.
> 
> Zadania opisane w tym temacie są wspólne dla każdego scenariusza wdrażania&#x2014;nie ma znaczenia, czy serwery sieci web są skonfigurowane do używania zdalnej usługi agenta narzędzia do wdrażania sieci Web usług IIS (Web Deploy), program obsługi wdrażania w sieci Web lub wdrożenie w trybie offline lub Aplikacja jest uruchomiona na jednym serwerze sieci web lub w farmie serwerów. Sposób wdrażania bazy danych może się zmieniać zgodnie z wymagań dotyczących zabezpieczeń oraz inne zagadnienia. Na przykład można wdrożyć bazę danych z lub bez przykładowych danych i mogą wdrażać mapowania roli użytkownika lub ręcznie skonfigurować po wdrożeniu. Jednak sposób konfigurowania serwera bazy danych pozostaje taki sam.


Nie musisz zainstalować wszelkie dodatkowe produkty lub narzędzi do konfigurowania serwera bazy danych do obsługi wdrażania w Internecie. Przy założeniu, że serwer bazy danych i serwera sieci web działają na różnych maszynach, po prostu musisz:

- Zezwalaj na SQL Server do komunikowania się za pomocą protokołu TCP/IP.
- Zezwalać na ruch programu SQL Server za pośrednictwem wszystkich zapór.
- Należy podać konto komputera serwera sieci web logowania programu SQL Server.
- Logowanie konta maszyny są mapowane na wszystkie role wymaganej bazy danych.
- Nadaj konta, które będą uruchamiać wdrożenie uprawnienia twórcy programu SQL Server, jak identyfikator logowania i bazy danych.
- Do obsługi wdrożeń Powtórz tę procedurę, mapy wdrażania konto logowania do **db\_właściciela** roli bazy danych.

W tym temacie pokazują sposób wykonywania każdego z tych procedur. Zadania i wskazówki, w tym temacie założono, że zaczynasz przy użyciu domyślnego wystąpienia programu SQL Server 2008 R2, systemem Windows Server 2008 R2. Przed kontynuowaniem upewnij się, że:

- Systemu Windows Server 2008 R2 z dodatkiem Service Pack 1 i wszystkie dostępne aktualizacje są instalowane.
- Serwer jest przyłączony do domeny.
- Serwer ma statyczny adres IP.
- SQL Server 2008 R2 z dodatkiem Service Pack 1 i wszystkie dostępne aktualizacje są instalowane.

Wystąpienie programu SQL Server musi jedynie zawierać **usługi aparatu bazy danych** roli, która znajduje się automatycznie w każdej instalacji programu SQL Server. Jednak w celu ułatwienia konfiguracji i konserwacji, zaleca się że zawrzesz **narzędzia do zarządzania — podstawowe** i **narzędzia zarządzania — kompletne** ról serwera.

> [!NOTE]
> Aby uzyskać więcej informacji na temat dołączania komputerów do domeny, zobacz [łączenie komputerów do domeny i rejestrowanie na](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Aby uzyskać więcej informacji na temat konfigurowania statycznych adresów IP, zobacz [skonfigurować statyczny adres IP](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Aby uzyskać więcej informacji na temat instalowania programu SQL Server, zobacz [Instalowanie programu SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx).


## <a name="enable-remote-access-to-sql-server"></a>Włącz dostęp zdalny do programu SQL Server

Program SQL Server używa protokołu TCP/IP do komunikowania się z komputerami zdalnymi. Jeśli serwer bazy danych i serwera sieci web znajdują się na różnych komputerach, należy:

- Konfigurowanie ustawień sieciowych programu SQL Server, aby umożliwić komunikację za pośrednictwem protokołu TCP/IP.
- Konfigurowanie sprzętowego lub programowego zapory do zezwalania na ruch TCP (i w niektórych przypadkach protokołu UDP (User Datagram) ruchu) na portach, które korzysta z wystąpienia programu SQL Server.

Aby włączyć program SQL Server do komunikowania się za pośrednictwem protokołu TCP/IP, należy użyć Menedżera konfiguracji SQL Server, aby zmienić konfigurację sieci dla wystąpienia programu SQL Server.

**Aby włączyć program SQL Server do komunikowania się za pomocą protokołu TCP/IP**

1. Na **Start** menu wskaż **wszystkie programy**, kliknij przycisk **programu Microsoft SQL Server 2008 R2**, kliknij przycisk **narzędzia konfiguracji**, a następnie kliknij przycisk **Menedżera konfiguracji SQL Server**.
2. W okienku widoku drzewa rozwiń **konfiguracja sieci programu SQL Server**, a następnie kliknij przycisk **protokoły dla MSSQLSERVER**.

   > [!NOTE]
   > Jeśli zainstalowano wiele wystąpień programu SQL Server, zobaczysz <strong>protokoły dla</strong><em>[nazwa wystąpienia]</em> elementu dla każdego wystąpienia. Należy skonfigurować ustawienia sieciowe na podstawie wystąpienia wystąpienia.
3. W okienku szczegółów kliknij prawym przyciskiem myszy **TCP/IP** wiersza, a następnie kliknij przycisk **Włącz**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. W **ostrzeżenie** okno dialogowe, kliknij przycisk **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. Musisz ponownie uruchomić usługi MSSQLSERVER, zanim nowe konfiguracji sieci zostały zastosowane. Możesz to zrobić w wierszu polecenia z konsoli usługi lub SQL Server Management Studio. W tej procedurze użyjesz programu SQL Server Management Studio.
6. Zamknij Menedżera konfiguracji SQL Server.
7. Na **Start** menu wskaż **wszystkie programy**, kliknij przycisk **programu Microsoft SQL Server 2008 R2**, a następnie kliknij przycisk **SQL Server Management Studio**.
8. W **Połącz z serwerem** okna dialogowego, **nazwy serwera** , wpisz nazwę serwera bazy danych, a następnie kliknij przycisk **Connect**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. W **Eksplorator obiektów** okienku kliknij prawym przyciskiem myszy węzeł serwera nadrzędnego (na przykład **TESTDB1**), a następnie kliknij przycisk **ponowne uruchomienie**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. W **programu Microsoft SQL Server Management Studio** okno dialogowe, kliknij przycisk **tak**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Gdy usługa zostanie ponownie uruchomiony, zamknij program SQL Server Management Studio.

Aby zezwolić na ruch programu SQL Server przez zaporę, najpierw musisz wiedzieć, które porty używa wystąpienia programu SQL Server. To zależy od sposobu tworzenia i skonfigurowane wystąpienie programu SQL Server:

- A *domyślnego wystąpienia* programu SQL Server nasłuchuje (i reaguje na nie) żądania na porcie TCP 1433.
- A *nazwane wystąpienie* programu SQL Server nasłuchuje (i reaguje na nie) żądania na porcie TCP przypisywany dynamicznie.
- Jeśli usługa SQL Server Browser jest włączona, klienci mogą wysyłać zapytania usługi na porcie UDP 1434, aby dowiedzieć się, który port TCP używany dla danego wystąpienia programu SQL Server. Jednak ta usługa zostanie wyłączona często ze względów bezpieczeństwa.

Przy założeniu, że używasz domyślnego wystąpienia programu SQL Server, musisz skonfigurować zaporę, aby zezwolić na ruch.

| Kierunek | Z portu | Do portu | Typ portu |
| --- | --- | --- | --- |
| Dla ruchu przychodzącego | Wszystkie | 1433 | TCP |
| Wychodzące | 1433 | Wszystkie | TCP |
  

> [!NOTE]
> Technicznie rzecz biorąc komputer kliencki będzie używać jeden losowo przypisany port TCP od 1024 do 5000 do komunikowania się z programem SQL Server i odpowiednio ograniczyć reguły zapory. Aby uzyskać więcej informacji na temat programu SQL Server, porty i zapór, zobacz [numery portów TCP/IP wymagany do komunikowania się do bazy danych SQL za pośrednictwem zapory](https://go.microsoft.com/?linkid=9805125) i [porady: Konfigurowanie serwera do nasłuchiwania na konkretnym porcie TCP (Konfiguracja programu SQL Server Menedżer)](https://msdn.microsoft.com/library/ms177440.aspx).


W większości środowisk systemu Windows Server prawdopodobnie musisz skonfigurować zaporę Windows na serwerze bazy danych. Domyślnie Zapora Windows umożliwia cały ruch wychodzący, chyba że regułę. Aby włączyć serwer sieci web dotrzeć do bazy danych, musisz skonfigurować regułę ruchu przychodzącego zezwalającą na ruch TCP na numer portu, który korzysta z wystąpienia programu SQL Server. Jeśli używasz domyślnego wystąpienia programu SQL Server umożliwia następnej procedury konfigurowania tej reguły.

**Aby skonfigurować zaporę Windows, aby umożliwić komunikację z domyślnego wystąpienia programu SQL Server**

1. Na serwerze bazy danych na **Start** menu wskaż **narzędzia administracyjne**, a następnie kliknij przycisk **Zapora Windows z zabezpieczeniami zaawansowanymi**.
2. W okienku widoku drzewa kliknij **reguły ruchu przychodzącego**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. W **akcje** okienku w obszarze **reguły ruchu przychodzącego**, kliknij przycisk **nową regułę**.
4. W Kreatora nowej reguły przychodzącej na **typ reguły** wybierz opcję **portu**, a następnie kliknij przycisk **dalej**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. Na **protokół i porty** strony, upewnij się, że **TCP** jest zaznaczone, a następnie w **określone porty lokalne** wpisz **1433**, a następnie kliknij przycisk **Dalej**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. Na **akcji** zostaw **Zezwalaj na połączenie** zaznaczone, a następnie kliknij przycisk **dalej**.
7. Na **profilu** zostaw **domeny** zaznaczone, wyczyść **prywatnej** i **publicznego** pola wyboru, a następnie kliknij przycisk **Następny**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. Na **nazwa** strony, Nadaj regule nazwę opisową odpowiednio (na przykład **wystąpienia domyślnego programu SQL Server — dostęp do sieci**), a następnie kliknij przycisk **Zakończ**.

Aby uzyskać więcej informacji na temat konfigurowania zapory Windows dla programu SQL Server, zwłaszcza, jeśli chcesz komunikować się z programem SQL Server za pośrednictwem portów niestandardowych lub dynamicznych, zobacz [porady: Konfigurowanie zapory Windows dla dostępu aparatu bazy danych](https://technet.microsoft.com/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Konfigurowanie logowania i bazy danych uprawnienia

Podczas wdrażania aplikacji sieci web do programu Internet Information Services (IIS), aplikacja zostanie uruchomiona przy użyciu tożsamości puli aplikacji. W środowisku domeny tożsamości puli aplikacji, użyj konta komputera serwera, na którym uruchomiony do dostępu do zasobów sieciowych. Konta komputerów mieć postać <em>[nazwa domeny]</em><strong>\</ strong ><em>[nazwa komputera]</em><strong>$</strong>&#x2014;na przykład <strong>FABRIKAM\TESTWEB1$</strong>. Aby zezwolić na dostęp do bazy danych przez sieć aplikacji sieci web, należy:

- Dodaj identyfikator logowania dla konta komputera serwera sieci web do wystąpienia programu SQL Server.
- Mapowanie dane logowanie konta komputera do żadnej roli wymaganej bazy danych (zwykle **db\_datareader** i **db\_datawriter**).

Jeśli aplikacja sieci web jest uruchomiony na farmie serwerów, a nie pojedynczego serwera, należy powtórzyć te procedury dla każdego serwera sieci web w farmie serwerów.

> [!NOTE]
> Aby uzyskać więcej informacji o tożsamości puli aplikacji i uzyskiwanie dostępu do zasobów sieciowych, zobacz [tożsamości puli aplikacji](https://go.microsoft.com/?linkid=9805123).


Może zbliżać się te zadania na różne sposoby. Aby utworzyć identyfikator logowania, można:

- Ręczne tworzenie identyfikatora logowania na serwerze bazy danych przy użyciu języka Transact-SQL lub SQL Server Management Studio.
- Użyj Projekt serwera SQL Server 2008 w programie Visual Studio do tworzenia i wdrażania logowania.

Logowania programu SQL Server jest obiektem poziomu serwera, a nie obiekt poziomu bazy danych, tak nie jest zależna od bazy danych, którą chcesz wdrożyć. W efekcie można utworzyć nazwy logowania w dowolnym momencie, a to najłatwiejsza metoda jest często logowania ręcznie utworzyć na serwerze bazy danych przed rozpoczęciem wdrażania baz danych. Aby utworzyć identyfikator logowania w programie SQL Server Management Studio, można użyć następnej procedury.

**Aby utworzyć identyfikator logowania programu SQL Server dla konta komputera serwera sieci web**

1. Na serwerze bazy danych na **Start** menu wskaż **wszystkie programy**, kliknij przycisk **programu Microsoft SQL Server 2008 R2**, a następnie kliknij przycisk **SQL Server Management Studio** .
2. W **Połącz z serwerem** okna dialogowego, **nazwy serwera** , wpisz nazwę serwera bazy danych, a następnie kliknij przycisk **Connect**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. W **Eksplorator obiektów** okienku kliknij prawym przyciskiem myszy **zabezpieczeń**, wskaż polecenie **New**, a następnie kliknij przycisk **logowania**.
4. W **logowania — nowy** dialogowym **nazwa logowania** wpisz nazwę konta komputera serwera sieci web (na przykład **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. Kliknij przycisk **OK**.

W tym momencie serwer bazy danych jest gotowa do opublikowania narzędzia Web Deploy. Jednak wszystkie rozwiązania, które można wdrożyć nie będzie działać, dopóki mapy dane logowanie konta komputera do ról wymaganej bazy danych. Mapowanie identyfikatora logowania do ról bazy danych wymaga dużo więcej traktować, co Ty nie role mapy aż po wdrożeniu bazy danych. Aby zamapować dane logowanie konta komputera do ról wymaganej bazy danych, można:

- Przypisz role bazy danych logowania na ręcznie, po wdrożeniu bazy danych po raz pierwszy.
- Użyj skryptu powdrożeniowego do przypisywania ról bazy danych do nazwy logowania.

Aby uzyskać więcej informacji na temat Automatyzacja tworzenia danych logowania i mapowania roli bazy danych, zobacz [członkostw roli bazy danych wdrażania w środowiskach testowych](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Alternatywnie służy następnej procedury do mapowania logowania konta komputera do ról wymaganej bazy danych ręcznie. Należy pamiętać, że nie można wykonać tej procedury, dopóki *po* udało Ci się wdrożyć bazy danych.

**Aby zamapować ról bazy danych do logowania konta komputera serwera sieci web**

1. Otwórz program SQL Server Management Studio, tak jak poprzednio.
2. W **Eksplorator obiektów** okienku rozwiń **zabezpieczeń** węzła, rozwiń węzeł **logowania** węzeł, a następnie kliknij dwukrotnie ikonę logowania konta komputera (na przykład **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. W **właściwości logowania** okno dialogowe, kliknij przycisk **mapowania użytkowników**.
4. W **użytkownicy zamapowani do tego logowania** tabeli, wybierz nazwę bazy danych (na przykład **ContactManager**).
5. W **członkostwo roli dla bazy danych:** *[Nazwa bazy danych]* wybierz wymagane uprawnienia. W przypadku przykładowe rozwiązanie Contact Manager, musisz wybrać **db\_datareader** i **db\_datawriter** ról.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. Kliknij przycisk **OK**.

Podczas ręcznego mapowania ról bazy danych często jest większe niż odpowiednie dla środowisk testowych, jest mniej pożądana automatyczna lub z jednym kliknięciem wdrożeń w środowiskach przejściowych lub produkcyjnych. Można znaleźć więcej informacji na temat tego rodzaju zadania przy użyciu skryptów po wdrożeniu w automatyzacji [członkostw roli bazy danych wdrażania w środowiskach testowych](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Aby uzyskać więcej informacji na temat serwera i bazy danych projektów, zobacz [Visual Studio 2010 projektów bazodanowych SQL Server](https://msdn.microsoft.com/library/ff678491.aspx).


## <a name="configure-permissions-for-the-deployment-account"></a>Skonfiguruj uprawnienia dla konta wdrożenia

Jeśli konto, które będzie używane do uruchamiania wdrożenia nie jest administratorem programu SQL Server, należy także utworzyć identyfikator logowania dla tego konta. Aby utworzyć bazę danych, konto musi należeć do **dbcreator** roli serwera lub mieć równoważne uprawnienia.

> [!NOTE]
> Korzystając z narzędzia Web Deploy lub VSDBCMD wdrożyć bazę danych, można użyć poświadczenia Windows lub programu SQL Server (jeśli wystąpienia programu SQL Server jest skonfigurowany do obsługi uwierzytelniania w trybie mieszanym). Następnej procedury przyjęto założenie, że chcesz użyć poświadczeń Windows, ale nie ma nic zatrzymywanie możesz z określania programu SQL Server, nazwę użytkownika i hasła w ciągu połączenia, podczas konfigurowania wdrożenia.


**Aby skonfigurować uprawnienia dla konta wdrożenia**

1. Otwórz program SQL Server Management Studio, tak jak poprzednio.
2. W **Eksplorator obiektów** okienku kliknij prawym przyciskiem myszy **zabezpieczeń**, wskaż polecenie **New**, a następnie kliknij przycisk **logowania**.
3. W **logowania — nowy** dialogowym **nazwa logowania** wpisz nazwę konta usługi wdrażania (na przykład **FABRIKAM\matt**).
4. W **wybierz stronę** okienku kliknij **ról serwera**.
5. Wybierz **dbcreator**, a następnie kliknij przycisk **OK**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Do obsługi kolejnych wdrożeń, należy także dodać na wdrażanie konta **db\_właściciela** roli bazy danych po ich pierwszym wdrożeniu. Jest tak, ponieważ na kolejne wdrożenia w przypadku modyfikowania schematu istniejącą bazę danych, zamiast tworzenia nowej bazy danych. Zgodnie z opisem w poprzedniej sekcji, nie można dodać użytkownika do roli bazy danych, dopóki nie utworzysz bazę danych, ze względów oczywiste.

**Do mapowania logowania konta wdrożenia bazy danych\_właściciela roli bazy danych**

1. Otwórz program SQL Server Management Studio, tak jak poprzednio.
2. W **Eksplorator obiektów** okna, rozwiń węzeł **zabezpieczeń** węzła, rozwiń węzeł **logowania** węzeł, a następnie kliknij dwukrotnie ikonę logowania konta komputera (na przykład **FABRIKAM\matt**).
3. W **właściwości logowania** okno dialogowe, kliknij przycisk **mapowania użytkowników**.
4. W **użytkownicy zamapowani do tego logowania** tabeli, wybierz nazwę bazy danych (na przykład **ContactManager**).
5. W **członkostwo roli dla bazy danych:** *[Nazwa bazy danych]* listy wybierz **db\_właściciela** roli.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. Kliknij przycisk **OK**.

## <a name="conclusion"></a>Wniosek

Serwer bazy danych powinno być teraz gotowy do akceptowania wdrożeń zdalnej bazy danych i zezwolić na dostęp do bazy danych zdalnego serwerów sieci web usług IIS. Przed przystąpieniem do wdrażania i korzystanie z baz danych, można sprawdzić tych kluczowych zagadnieniach:

- Czy skonfigurowano program SQL Server do akceptowania połączeń zdalnych TCP/IP?
- Czy skonfigurowano wszelkie zapory, aby zezwalać na ruch programu SQL Server?
- Czy utworzono dane logowanie konta komputera dla każdego serwera sieci web, który będzie miał dostęp programu SQL Server?
- Wdrożenie bazy danych ma skryptu, aby utworzyć mapowania roli użytkownika, czy należy utworzyć je ręcznie, po wdrożeniu bazy danych po raz pierwszy?
- Możesz utworzono nazwy logowania dla konta wdrożenia i dodać go do **dbcreator** roli serwera?

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące wdrażania projektów bazy danych, zobacz [wdrażanie projektów baz danych](../web-deployment-in-the-enterprise/deploying-database-projects.md). Aby uzyskać wskazówki dotyczące tworzenia członkostw roli bazy danych, uruchamiając skrypt po wdrożeniu, zobacz [członkostw roli bazy danych wdrażania w środowiskach testowych](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Aby uzyskać wskazówki na temat sposobu sprostać wyzwaniom wdrażania unikatowy, które stanowią bazy danych członkostwa, zobacz [wdrażanie baz danych członkostwa w środowiskach przedsiębiorstw](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

> [!div class="step-by-step"]
> [Poprzednie](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [dalej](creating-a-server-farm-with-the-web-farm-framework.md)
