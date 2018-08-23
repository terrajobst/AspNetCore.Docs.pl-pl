---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
title: Konfigurowanie serwera sieci Web dla sieci Web wdrażanie, publikowanie (wdrożenie w trybie Offline) | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób konfigurowania serwera sieci web usług IIS do obsługi publikowania w sieci web w trybie offline i wdrażania. Podczas pracy z Internet Information Services (I...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: ba92788f-9f03-44b1-b6b2-af8413e6a35d
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
msc.type: authoredcontent
ms.openlocfilehash: eab69e93417ddedea9214523de34697b044a871e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753906"
---
<a name="configuring-a-web-server-for-web-deploy-publishing-offline-deployment"></a>Konfigurowanie serwera sieci Web dla usługi publikowania (wdrożenie w trybie Offline) Web Deploy
====================
przez [Jason Lee](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób konfigurowania serwera sieci web usług IIS do obsługi publikowania w sieci web w trybie offline i wdrażania.
> 
> Podczas pracy z Internet Information Services (IIS) Narzędzie Web Deployment (Web Deploy) 2.0 lub nowszej, istnieją trzy główne metody umożliwia pobieranie aplikacji lub witryn na serwerze sieci web. Można:
> 
> - Użyj *usługa zdalnego agenta narzędzia Web Deploy*. Takie podejście wymaga mniej konfiguracji serwera sieci web, ale musisz podać poświadczenia administratora lokalnego serwera, aby wdrażać żadnych elementów do serwera.
> - Użyj *obsługi narzędzia Web Deploy*. To podejście jest o wiele bardziej skomplikowane i wymaga więcej nakładu pracy początkowej, aby skonfigurować serwer sieci web. Jednak użycie tej metody, można skonfigurować usługi IIS, aby umożliwić użytkownikom niebędącym administratorami wykonać wdrożenie. Program obsługi wdrażania sieci Web jest dostępna tylko w usługach IIS w wersji 7 lub nowszej.
> - Użyj *wdrożenie w trybie offline*. Takie podejście wymaga co najmniej konfiguracji serwera sieci web, ale administrator serwera, należy ręcznie skopiować pakiet sieci web na serwerze i zaimportuj go za pomocą Menedżera usług IIS.
> 
> Aby uzyskać więcej informacji na temat kluczowych funkcji, zalety i wady tych metod, zobacz [Wybieranie podejścia prawo do wdrażania w Internecie](choosing-the-right-approach-to-web-deployment.md).


Tak, jeśli wdrożenie zdalne ograniczeń infrastruktury lub zabezpieczeń sieci nie było możliwe. Jest to najprawdopodobniej będą w przypadku środowisk produkcyjnych dostępnego z Internetu, w której serwery sieci web są izolowane&#x2014;albo fizycznie lub przez zapory i podsieci&#x2014;od pozostałej części infrastruktury serwerowej.

Oczywiście takie podejście staje się mniej pożądane, jeśli aplikacje sieci web są aktualizowane w regularnych odstępach czasu. Jeśli infrastruktury umożliwia, warto należy rozważyć włączenie zdalnego wdrażania za pomocą obsługi wdrażania sieci Web lub usługi sieci Web wdrożenia zdalnego agenta.

## <a name="task-overview"></a>Omówienie zadań

Aby skonfigurować serwer sieci web do obsługi importu w trybie offline i wdrażania pakietów sieci web, należy:

- Instalowanie usług IIS 7.5 oraz IIS 7, zalecana konfiguracja.
- Zainstaluj narzędzie Web Deploy 2.1 lub nowszej.
- Utwórz witrynę sieci Web usług IIS do hostowania wdrożoną zawartością.
- Wyłącz usługę agenta wdrażania sieci Web.

Aby specjalnie obsługiwać przykładowe rozwiązanie, należy także do:

- Instalowanie programu .NET Framework 4.0.
- Zainstaluj program ASP.NET MVC 3.

W tym temacie pokazują sposób wykonywania każdego z tych procedur. Zadania i wskazówki, w tym temacie założono, że zaczynasz z kompilacją serwera czystego, systemem Windows Server 2008 R2. Przed kontynuowaniem upewnij się, że:

- Systemu Windows Server 2008 R2 z dodatkiem Service Pack 1 i wszystkie dostępne aktualizacje są instalowane.
- Serwer jest przyłączony do domeny.
- Serwer ma statyczny adres IP.

> [!NOTE]
> Aby uzyskać więcej informacji na temat dołączania komputerów do domeny, zobacz [łączenie komputerów do domeny i rejestrowanie na](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Aby uzyskać więcej informacji na temat konfigurowania statycznych adresów IP, zobacz [skonfigurować statyczny adres IP](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="install-products-and-components"></a>Zainstaluj produkty i składniki

Ta sekcja przeprowadzi Cię przez zainstalowanie wymaganych produktów i składników na serwerze sieci web. Przed przystąpieniem do wykonywania, dobrym rozwiązaniem jest uruchamianie Windows Update, aby upewnić się, że serwer jest w pełni bądź na bieżąco.

W takim przypadku należy zainstalować te rzeczy:

- **Zalecana konfiguracja usług IIS 7**. Dzięki temu **serwer sieci Web (IIS)** roli na serwerze sieci web i instaluje zestaw moduły usług IIS i składników, które są potrzebne do obsługi aplikacji ASP.NET.
- **.NET framework 4.0**. Jest to wymagane do uruchamiania aplikacji, które zostały utworzone w tej wersji programu .NET Framework.
- **Sieci Web narzędzia do wdrażania 2.1 lub nowszej**. Spowoduje to zainstalowanie narzędzia Web Deploy (i jego podstawowego pliku wykonywalnego, MSDeploy.exe) na serwerze. Narzędzie Web Deploy umożliwia importowanie i eksportowanie pakietów sieci web i integruje się z usługami IIS.
- **ASP.NET MVC 3**. Spowoduje to zainstalowanie zestawów, należy uruchomić aplikacji MVC 3.

> [!NOTE]
> W tym przewodniku opisano użycie Instalatora platformy sieci Web do zainstalowania i skonfigurowania różnych składników. Mimo że nie musisz użyć Instalatora platformy sieci Web, upraszcza proces instalacji przez automatyczne wykrywanie zależności i zapewnienie zawsze uzyskać najnowsze wersje produktów. Aby uzyskać więcej informacji, zobacz [3.0 Instalatora platformy sieci Web firmy Microsoft](https://go.microsoft.com/?linkid=9805118).


**Aby zainstalować wymagane produktów i składników**

1. Pobierz i zainstaluj [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9805118).
2. Po zakończeniu instalacji zostanie automatycznie uruchomiony Instalator platformy sieci Web.

    > [!NOTE]
    > Teraz możesz uruchamiać Instalatora platformy sieci Web w dowolnym momencie z **Start** menu. Aby to zrobić, na **Start** menu, kliknij przycisk **wszystkie programy**, a następnie kliknij przycisk **Instalatora platformy sieci Web firmy Microsoft**.
3. W górnej części **3.0 Instalatora platformy sieci Web** okna, kliknij przycisk **produktów**.
4. W lewej części okna, w okienku nawigacji kliknij **struktur**.
5. W **Microsoft .NET Framework 4** wiersz, jeśli nie zainstalowano jeszcze programu .NET Framework, kliknij przycisk **Dodaj**.

    > [!NOTE]
    > Może być już zainstalowano program .NET Framework 4.0, za pośrednictwem usługi Windows Update. W przypadku produktu lub składnik jest już zainstalowany, Instalator platformy sieci Web poda to przez zastąpienie **Dodaj** przycisk z tekstem **zainstalowane**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image1.png)
6. W **ASP.NET MVC 3 (Visual Studio 2010)** wiersz, kliknij przycisk **Dodaj**.
7. W okienku nawigacji kliknij **serwera**.
8. W **zalecana konfiguracja programu IIS 7** wiersz, kliknij przycisk **Dodaj**.
9. W **2.1 narzędzia wdrażania Web** wiersz, kliknij przycisk **Dodaj**.
10. Kliknij przycisk **zainstalować**. Instalator platformy sieci Web zostanie wyświetlona lista produktów&#x2014;oraz wszystkie powiązane zależności&#x2014;do zainstalowania i zostanie wyświetlony monit o zaakceptowanie postanowień licencyjnych.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image2.png)
11. Przejrzyj postanowienia licencyjne, a jeśli wyrażasz zgodę na warunki, kliknij przycisk **akceptuję**.
12. Po zakończeniu instalacji kliknij przycisk **Zakończ**, a następnie Zamknij **3.0 Instalatora platformy sieci Web** okna.

Jeśli zainstalowano program .NET Framework 4.0 przed zainstalowaniem usług IIS, należy uruchomić [narzędzie rejestracji usług IIS platformy ASP.NET](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) do zarejestrowania najnowszą wersję platformy ASP.NET z programem IIS. Jeśli tego nie zrobisz, można znaleźć usługi IIS będą udostępniać zawartość statyczną (takich jak pliki HTML) bez żadnych problemów, ale zwróci **HTTP 404.0: błąd — nie można odnaleźć** podczas próby przeglądania zawartości platformy ASP.NET. Aby upewnić się, że program ASP.NET 4.0 jest zarejestrowany, można użyć następnej procedury.

**Aby zarejestrować program ASP.NET 4.0 z usługami IIS**

1. Kliknij przycisk **Start**, a następnie wpisz **polecenia**.
2. W wynikach wyszukiwania, kliknij prawym przyciskiem myszy **polecenia**, a następnie kliknij przycisk **Uruchom jako administrator**.
3. W oknie wiersza polecenia przejdź do **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** katalogu.
4. Wpisz następujące polecenie i naciśnij klawisz Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample1.cmd)]
5. Jeśli planujesz hostowanie aplikacji sieci web 64-bitowe, w dowolnym momencie należy również zarejestrować 64-bitowej wersji programu ASP.NET, za pomocą programu IIS. Aby to zrobić, w oknie wiersza polecenia, przejdź do **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** katalogu.
6. Wpisz następujące polecenie i naciśnij klawisz Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample2.cmd)]

Dobrym rozwiązaniem jest aktualizacja Windows ponownie użyć w tym momencie pobrać i zainstalować wszystkie dostępne aktualizacje dla nowych produktów i składników, którą zainstalowałeś.

## <a name="configure-the-iis-website"></a>Skonfiguruj witrynę sieci Web usług IIS

Przed wdrożeniem zawartości sieci web na serwerze, należy utworzyć i skonfigurować witrynę sieci Web usług IIS do hostowania zawartości. Narzędzie Web Deploy można wdrażać tylko w sieci web pakiety do istniejącej witryny internetowej usług IIS; Witryna sieci Web nie może utworzyć dla Ciebie. Na wysokim poziomie należy wykonać następujące zadania:

- Utwórz folder w systemie plików do hostowania zawartości.
- Tworzenie witryny sieci Web usług IIS do obsługi zawartości i skojarz go z folderu lokalnego.
- Udziel uprawnień do odpowiedniej tożsamości puli aplikacji w lokalnym folderze odczytu.

Mimo że nie ma zatrzymywanie możesz z wdrażania zawartości do domyślnej witryny sieci Web w usługach IIS, to podejście nie jest zalecane dla żadnych innych niż scenariuszy testu lub pokaz. Aby zasymulować w środowisku produkcyjnym, należy utworzyć nową witrynę sieci Web usług IIS przy użyciu ustawień, które są specyficzne dla wymagań aplikacji.

**Aby utworzyć i skonfigurować witrynę sieci Web usług IIS**

1. W lokalnym systemie plików, należy utworzyć folder do przechowywania zawartości (na przykład **C:\DemoSite**).
2. Na **Start** menu wskaż **narzędzia administracyjne**, a następnie kliknij przycisk **Internet Information Services (IIS) Manager**.
3. W Menedżerze usług IIS w **połączeń** okienku rozwiń węzeł serwera (na przykład **PROWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image3.png)
4. Kliknij prawym przyciskiem myszy **witryn** węzłem, a następnie kliknij przycisk **Dodaj witrynę sieci Web**.
5. W **Nazwa lokacji** wpisz nazwę witryny sieci Web usług IIS (na przykład **DemoSite**).
6. W **ścieżkę fizyczną** wpisz (lub przejdź do) ścieżkę do folderu lokalnego (na przykład **C:\DemoSite**).
7. W **portu** wpisz numer portu, na którym chcesz hostować witryny sieci Web (na przykład **85**).

    > [!NOTE]
    > Numery standardowego portu to 80 dla protokołu HTTP i 443 dla protokołu HTTPS. Jednak możesz hostować tę witrynę sieci Web na porcie 80, należy zatrzymać domyślnej witryny sieci Web, zanim mogą uzyskiwać dostęp do witryny.
8. Pozostaw **nazwy hosta** pole puste, chyba że chcesz skonfigurować System nazw domen (DNS, Domain Name System) rekord dla witryny sieci Web, a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image4.png)

    > [!NOTE]
    > W środowisku produkcyjnym prawdopodobnie warto hostowanie witryny sieci Web na porcie 80 i konfigurowanie nagłówka hosta, wraz z pasujących rekordów DNS. Aby uzyskać więcej informacji na temat konfigurowania nagłówki hosta w usługach IIS 7, zobacz [Konfigurowanie nagłówka hosta dla witryny sieci Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Aby uzyskać więcej informacji na temat roli serwera DNS w systemie Windows Server 2008 R2, zobacz [Omówienie serwera DNS](https://technet.microsoft.com/en-gb/library/cc770392.aspx) i [serwer DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. W **akcje** okienku w obszarze **edytowanie witryny**, kliknij przycisk **powiązania**.
10. W **powiązania witryny** okno dialogowe, kliknij przycisk **Dodaj**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image5.png)
11. W **Dodawanie powiązania witryny** okno dialogowe, zestaw **adresu IP** i **portu** aby dopasować go do istniejącej konfiguracji lokacji.
12. W **nazwy hosta** wpisz nazwę serwera sieci web (na przykład **PROWEB1**), a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image6.png)

    > [!NOTE]
    > Pierwszy powiązania witryny umożliwia dostęp do witryny lokalnie przy użyciu adresu IP i portu lub `http://localhost:85`. Drugi powiązania witryny umożliwia dostęp do witryny z innych komputerów w domenie przy użyciu nazwy komputera (na przykład http://proweb1:85).
13. W **powiązania witryny** okno dialogowe, kliknij przycisk **Zamknij**.
14. W **połączeń** okienku kliknij **pul aplikacji**.
15. W **pul aplikacji** , kliknij prawym przyciskiem myszy nazwę puli aplikacji, a potem kliknij **podstawowych ustawień**. Domyślnie nazwa puli aplikacji będą odpowiadać nazwie witryny sieci Web (na przykład **DemoSite**).
16. W **.NET Framework w wersji** listy wybierz **4.0.30319 .NET Framework**, a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image7.png)

    > [!NOTE]
    > Przykładowe rozwiązanie wymaga programu .NET Framework 4.0. To nie jest wymagane dla narzędzia Web Deploy w zasadzie.

Aby dla witryny sieci Web do obsługi zawartości tożsamość puli aplikacji musi mieć uprawnienia odczytu na folder lokalny, w którym jest przechowywana zawartość. W przypadku usług IIS 7.5 pule aplikacji przy użyciu tożsamości puli aplikacji są domyślnie uruchamiane (w przeciwieństwie do poprzednich wersji programu IIS, w którym pule aplikacji będzie zazwyczaj uruchamiane przy użyciu konta Usługa sieciowa). Tożsamość puli aplikacji nie jest kontem rzeczywistego użytkownika i nie są wyświetlane na listę użytkowników lub grup&#x2014;zamiast tego należy go jest tworzony dynamicznie po uruchomieniu puli aplikacji. Każda tożsamość puli aplikacji zostanie dodany do lokalnej **IIS\_IUSRS** grupa zabezpieczeń jako ukryty element.

Aby udzielić uprawnień do tożsamości puli aplikacji do pliku lub folderu, masz dwie opcje:

- Przypisywanie uprawnień do odpowiedniej tożsamości puli aplikacji bezpośrednio przy użyciu formatu <strong>puli aplikacji IIS\</ strong ><em>[Nazwa puli aplikacji]</em>(na przykład <strong>IIS AppPool\DemoSite</strong>).
- Przypisywanie uprawnień do **IIS\_IUSRS** grupy.

Najbardziej typowym podejściem jest przypisywanie uprawnień do lokalnej **IIS\_IUSRS** grupy, ponieważ takie podejście pozwala zmienić pule aplikacji bez konieczności ponownego uprawnienia systemu plików. Następna procedura używa tego podejścia opartego na grupach.

> [!NOTE]
> Aby uzyskać więcej informacji o tożsamości puli aplikacji w usługach IIS 7.5, zobacz [tożsamości puli aplikacji](https://go.microsoft.com/?linkid=9805123).


**Aby skonfigurować uprawnienia do folderu witryny sieci Web usług IIS**

1. W Eksploratorze Windows przejdź do lokalizacji folderu lokalnego.
2. Kliknij prawym przyciskiem myszy folder, a następnie kliknij przycisk **właściwości**.
3. Na **zabezpieczeń** kliknij pozycję **Edytuj**, a następnie kliknij przycisk **Dodaj**.
4. Kliknij przycisk **lokalizacje**. W **lokalizacje** okno dialogowe, wybierz serwer lokalny, a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image8.png)
5. W **Wybieranie użytkowników lub grup** okno dialogowe, typ **IIS\_IUSRS**, kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK**.
6. W <strong>uprawnienia dla</strong><em>[nazwa folderu]</em> okno dialogowe, zwróć uwagę, że przypisano nową grupę <strong>odczytu &amp; wykonania</strong>, <strong>Wyświetl listę folderów zawartość</strong>, i <strong>odczytu</strong> uprawnienia domyślne. Pozostaw to bez zmian, a następnie kliknij przycisk <strong>OK</strong>.
7. Kliknij przycisk <strong>OK</strong> zamknąć <em>[nazwa folderu]</em><strong>właściwości</strong> okno dialogowe.

## <a name="disable-the-remote-agent-service"></a>Wyłącz usługę agenta zdalnego

Po zainstalowaniu narzędzia Web Deploy, Usługa agenta wdrażania sieci Web jest zainstalowany i uruchomiony automatycznie. Ta usługa pozwala na wdrażanie i publikowanie pakietów sieci web z lokalizacji zdalnej. Użytkownik nie będzie używany możliwość zdalnego wdrażania w tym scenariuszu, więc należy zatrzymać i wyłączyć usługę.

> [!NOTE]
> Nie ma potrzeby Zatrzymaj usługę agenta zdalnego, aby można było zaimportować i ręcznie wdrożyć pakiet sieci web. Jednak jest dobrym rozwiązaniem, aby zatrzymać i wyłączyć usługę, jeśli nie zamierzasz go używać.


Można zatrzymać i wyłączyć usługę na wiele sposobów, za pomocą różnych narzędzi wiersza polecenia lub poleceń cmdlet programu Windows PowerShell. Ta procedura opisuje proste podejście oparte na interfejsie użytkownika.

**Aby zatrzymać i wyłączyć usługę agenta zdalnego**

1. Na **Start** menu wskaż **narzędzia administracyjne**, a następnie kliknij przycisk **usług**.
2. W konsoli usługi Znajdź **Usługa agenta wdrażania sieci Web** wiersza.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image9.png)
3. Kliknij prawym przyciskiem myszy **Usługa agenta wdrażania sieci Web**, a następnie kliknij przycisk **właściwości**.
4. W **właściwości Usługa agenta wdrażania sieci Web** okno dialogowe, kliknij przycisk **zatrzymać**.
5. W **uruchamiana** listy wybierz **wyłączone**, a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image10.png)

## <a name="conclusion"></a>Wniosek

W tym momencie serwer sieci web jest gotowa do wdrożenia pakietu internetowego w trybie offline. Przed przystąpieniem do importowania pakietów sieci web w witrynie sieci Web usług IIS, można sprawdzić tych kluczowych zagadnieniach:

- Zostały zarejestrowane programu ASP.NET 4.0 z usługami IIS?
- Tożsamość puli aplikacji ma dostęp do odczytu do folderu źródłowego dla witryny sieci Web?
- Została zatrzymana usługa agenta wdrażania sieci Web?

> [!div class="step-by-step"]
> [Poprzednie](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
> [dalej](configuring-a-database-server-for-web-deploy-publishing.md)
