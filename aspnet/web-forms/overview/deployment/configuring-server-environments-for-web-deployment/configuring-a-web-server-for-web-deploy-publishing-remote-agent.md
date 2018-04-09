---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
title: Konfigurowanie serwera sieci Web dla sieci Web wdrażanie publikowania (agenta zdalnego) | Dokumentacja firmy Microsoft
author: jrjlee
description: W tym temacie opisano sposób konfigurowania serwera sieci web usług Internet Information Services (IIS) do obsługi publikowania w sieci web i wdrożenia przy użyciu wdrożenia sieci Web usług IIS...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 239c7aa8-d09a-4d02-9c0e-6bd52be5f0d5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
msc.type: authoredcontent
ms.openlocfilehash: 8cad6ee45a8331513c72c4079f300fbb06c1ed77
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="configuring-a-web-server-for-web-deploy-publishing-remote-agent"></a>Konfigurowanie serwera sieci Web dla narzędzia Web Deploy publikowania (agenta zdalnego)
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano, jak skonfigurować serwer sieci web usług Internet Information Services (IIS) do obsługi publikowania w sieci web i wdrożenia przy użyciu usługi agenta zdalnego programu IIS (Web Deploy) Narzędzie Web Deployment.
> 
> Podczas pracy z sieci Web Deploy 2.0 lub nowszej, istnieją trzy główne metody można użyć do pobrania Twojej aplikacji lub witryn na serwerze sieci web. Można:
> 
> - Użyj *usługa zdalnego agenta narzędzia Web Deploy*. Ta metoda wymaga mniej konfiguracji serwera sieci web, ale musisz podać poświadczenia administratora lokalnego serwera w celu wdrożenia niczego do serwera.
> - Użyj *narzędzia Web Deploy obsługi*. Ta metoda jest znacznie bardziej złożone i wymaga więcej wysiłku początkowej, aby skonfigurować serwer sieci web. Jednak gdy posłuż się tą metodą, należy skonfigurować usług IIS, aby umożliwić użytkownikom niebędącym administratorami przeprowadzić wdrożenie. Obsługa wdrażania w sieci Web jest dostępna tylko w usługach IIS w wersji 7 lub nowszej.
> - Użyj *wdrożenia w trybie offline*. Takie podejście najmniejsza ilość czynności konfiguracyjnych serwera sieci web, ale administrator serwera, należy ręcznie skopiować pakiet sieci web na serwerze i zaimportuj go za pomocą Menedżera usług IIS.
> 
> Aby uzyskać więcej informacji o najważniejszych funkcjach, zalet i wad z tych metod, zobacz [Wybieranie podejście prawo do wdrożenia w sieci Web](choosing-the-right-approach-to-web-deployment.md).


## <a name="is-the-web-deploy-remote-agent-the-right-approach-for-you"></a>To jest sieci Web wdrażanie agenta zdalnego podejście dla Ciebie?

Tak, jeśli użytkownik, który zostanie wdrożona zawartość można podać poświadczenia administratora na serwerze docelowym. Ta metoda jest często pożądane w przypadku tych typów scenariuszy:

- Środowisk deweloperskich lub testowania, gdzie deweloper ma pełną kontrolę nad docelowego serwera sieci web i serwera bazy danych.
- Mniejszych organizacji, w których jednego użytkownika lub małą grupę użytkowników ma kontrolę nad cyklem życia całej aplikacji.

W partiach większych organizacji, a szczególnie w przypadku środowisk przemieszczania i produkcji często nie jest realistyczne Nadawanie użytkownikom uprawnień administratora na serwerach sieci web. W przypadku serwerów sieci web hostowanej jest to szczególnie mało prawdopodobne w przypadku. Ponadto jeśli planujesz zautomatyzować wdrożenie z serwera kompilacji, nie możesz Użyj poświadczeń administratora dla procesu wdrażania. W tych scenariuszach Konfigurowanie serwerów sieci web do obsługi wdrożenia przy użyciu [program obsługi wdrażania w sieci Web](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md) może zapewnić lepszy wybór.

## <a name="task-overview"></a>Omówienie zadań

W tym temacie opisano sposób konfigurowania serwera sieci web Internet informacji Services (IIS) 7.5, aby zaakceptować i wdrażania pakietów sieci web z komputera zdalnego za pomocą metody sieci Web wdrażanie agenta zdalnego. Musisz:

- Zainstaluj usługi IIS 7.5 oraz IIS 7 zalecana konfiguracja.
- Zainstaluj narzędzie Web Deploy 2.1 lub nowszej.
- Tworzenie witryny sieci Web usług IIS do hostowania wdrożoną zawartością.
- Upewnij się, że jest uruchomiona usługa agenta sieci Web wdrożenia.

W szczególności udostępniać przykładowe rozwiązanie, również należy:

- Zainstaluj program .NET Framework 4.0.
- Zainstaluj program ASP.NET MVC 3.

W tym temacie opisano sposób wykonywania każdego z tych procedur. Zadania i wskazówki, w tym temacie założono zaczynasz z kompilacją czystą serwera z systemem Windows Server 2008 R2. Przed kontynuowaniem upewnij się, że:

- Windows Server 2008 R2 z dodatkiem Service Pack 1 i wszystkie dostępne aktualizacje są instalowane.
- Serwer jest przyłączony do domeny.
- Serwer ma statyczny adres IP.

> [!NOTE]
> Aby uzyskać więcej informacji dotyczących dołączania komputerów do domeny, zobacz [przyłączania komputerów do domeny i rejestrowanie na](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Aby uzyskać więcej informacji na temat konfigurowania statycznych adresów IP, zobacz [skonfigurować statyczny adres IP](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Usługa zdalnego agenta jest obsługiwany przez usługi IIS 6 lub nowszej i nie trzeba będzie być przyłączony do domeny. Jednak kroki opisane w tym samouczku opracowanych i przetestowane na IIS 7.5 i procedury dotyczące inne wersje mogą się różnić.


## <a name="install-products-and-components"></a>Instalowanie produktów i składników

Ta sekcja przeprowadzi Cię przez proces instalacji wymaganych produktów i składników na serwerze sieci web. Przed rozpoczęciem dobrym rozwiązaniem jest uruchomienie usługi Windows Update, aby upewnić się, że serwer jest w pełni aktualne.

W takim przypadku należy zainstalować następujące elementy:

- **Zalecana konfiguracja usług IIS 7**. Dzięki temu **serwer sieci Web (IIS)** roli na serwerze sieci web i instaluje zestaw modułów usług IIS i składników, które są potrzebne do obsługi aplikacji ASP.NET.
- **.NET framework 4.0**. Jest to wymagane do uruchamiania aplikacji, które zostały utworzone w tej wersji programu .NET Framework.
- **Narzędzia Deployment Tool w wersji 2.1 lub nowszej w sieci Web**. Spowoduje to zainstalowanie narzędzia Web Deploy (i jego podstawowy plik wykonywalny MSDeploy.exe) na serwerze. W ramach tego procesu instaluje i uruchamia usługę sieci Web wdrażania agenta. Usługa ta umożliwia wdrażanie pakietów sieci web z komputera zdalnego.
- **ASP.NET MVC 3**. Spowoduje to zainstalowanie zestawów, należy uruchomić aplikacji MVC 3.

> [!NOTE]
> W tym przewodniku opisano użycie Instalatora platformy sieci Web do instalowania i konfigurowania wymaganych składników. Chociaż nie trzeba użyć Instalatora platformy sieci Web, upraszcza proces instalacji automatycznie wykrywanie zależności i zapewnienia zawsze uzyskać najnowsze wersje produktu. Aby uzyskać więcej informacji, zobacz [3.0 Instalatora platformy sieci Web Microsoft](https://go.microsoft.com/?linkid=9805118).


**Aby zainstalować wymagane produkty i składniki**

1. Pobierz i zainstaluj [Instalatora platformy sieci Web](https://go.microsoft.com/?linkid=9805118).
2. Po zakończeniu instalacji automatycznie spowoduje uruchomienie Instalatora platformy sieci Web.

    > [!NOTE]
    > Teraz możesz uruchomić Instalatora platformy sieci Web w dowolnej chwili z **Start** menu. Aby to zrobić, na **Start** menu, kliknij przycisk **wszystkie programy**, a następnie kliknij przycisk **Instalatora platformy sieci Web firmy Microsoft**.
3. W górnej części **3.0 Instalatora platformy sieci Web** okna, kliknij przycisk **produkty**.
4. Po lewej stronie okna, w okienku nawigacji kliknij **struktury**.
5. W **Microsoft .NET Framework 4** wiersz, jeśli nie zainstalowano jeszcze programu .NET Framework, kliknij przycisk **Dodaj**.

    > [!NOTE]
    > Być może został już zainstalowany programu .NET Framework 4.0 za pośrednictwem usługi Windows Update. Jeśli produktu lub składnik jest już zainstalowane, Instalator platformy sieci Web będzie tę informację, zastępując **Dodaj** button z tekstem **zainstalowana**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image1.png)
6. W **ASP.NET MVC 3 (Visual Studio 2010)** wiersz, kliknij przycisk **Dodaj**.
7. W okienku nawigacji kliknij **serwera**.
8. W **usług IIS 7 zalecana konfiguracja** wiersz, kliknij przycisk **Dodaj**.
9. W **2.1 narzędzia wdrażania Web** wiersz, kliknij przycisk **Dodaj**.
10. Kliknij przycisk **zainstalować**. Instalator platformy sieci Web zostanie wyświetlona lista produktów&#x2014;oraz wszystkie skojarzone zależności&#x2014;do zainstalowania i wyświetli monit o zaakceptowanie postanowień licencyjnych.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image2.png)
11. Przejrzyj postanowienia licencyjne, a użytkownik wyraża zgodę na warunki, kliknij przycisk **akceptuję**.
12. Po zakończeniu instalacji kliknij przycisk **Zakończ**, a następnie Zamknij **3.0 Instalatora platformy sieci Web** okna.

Jeśli zainstalowano program .NET Framework 4.0 przed zainstalowaniem usług IIS, musisz uruchomić [narzędzie rejestracji programu ASP.NET usług IIS](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) do rejestrowania najnowszą wersję platformy ASP.NET z programem IIS. Jeśli nie zrobisz, można znaleźć usługi IIS będą udostępniać zawartość statyczną (takich jak pliki HTML) bez problemów, ale zwróci **HTTP 404.0: błąd — nie można odnaleźć** podczas próby przeglądanie zawartości ASP.NET. Ta procedura umożliwia upewnij się, że program ASP.NET 4.0 jest zarejestrowany.

**Aby zarejestrować program ASP.NET 4.0 z usługami IIS**

1. Kliknij przycisk **Start**, a następnie wpisz **wiersza polecenia**.
2. W wynikach wyszukiwania kliknij prawym przyciskiem myszy **wiersza polecenia**, a następnie kliknij przycisk **Uruchom jako administrator**.
3. W oknie wiersza polecenia przejdź do **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** katalogu.
4. Wpisz następujące polecenie, a następnie naciśnij klawisz Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample1.cmd)]
5. Jeśli planujesz hostowanie aplikacji sieci web 64-bitowe w dowolnym momencie, należy również zarejestrować 64-bitowej wersji platformy ASP.NET z programem IIS. Aby to zrobić, w oknie wiersza polecenia, przejdź do **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** katalogu.
6. Wpisz następujące polecenie, a następnie naciśnij klawisz Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample2.cmd)]

Dobrym rozwiązaniem usługi Windows Update ponownie użyć w tym momencie pobrać i zainstalować wszystkie dostępne aktualizacje dla nowych produktów i składników, które zostały zainstalowane.

## <a name="configure-the-iis-website"></a>Konfigurowanie witryny sieci Web usług IIS

Przed wdrożeniem zawartości sieci web na serwerze, należy utworzyć i skonfigurować witrynę sieci Web usług IIS do hostowania zawartości. Narzędzie Web Deploy do istniejącej witryny usług IIS; można wdrożyć tylko pakietów sieci web nie może utworzyć witryny sieci Web dla Ciebie. Na wysokim poziomie należy wykonać następujące zadania:

- Utwórz folder w systemie plików, aby udostępnić zawartość.
- Utwórz witrynę sieci Web usług IIS do obsługi zawartości i skojarzyć go z folderu lokalnego.
- Udziel uprawnienia do tożsamości puli aplikacji na folder lokalny odczytu.

Nie ma nic zatrzymywanie możesz z wdrażanie zawartości do domyślnej witryny sieci Web w usługach IIS, ale to rozwiązanie nie jest zalecane dla żadnych innych niż scenariuszy testu lub demonstracji. Do symulowania środowiska produkcyjnego, należy utworzyć nową witrynę IIS przy użyciu ustawień, które są specyficzne dla wymagań aplikacji.

**Aby utworzyć i skonfigurować witrynę sieci Web usług IIS**

1. W lokalnym systemie plików, należy utworzyć folder do przechowywania zawartości (na przykład **C:\DemoSite**).
2. Na **Start** menu wskaż **narzędzia administracyjne**, a następnie kliknij przycisk **Internet Information Services (IIS) Manager**.
3. W Menedżerze usług IIS w **połączeń** okienku rozwiń węzeł serwera (na przykład **TESTWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image3.png)
4. Kliknij prawym przyciskiem myszy **witryny** węzeł, a następnie kliknij przycisk **Dodaj witrynę sieci Web**.
5. W **nazwa witryny** wpisz nazwę witryny sieci Web usług IIS (na przykład **DemoSite**).
6. W **ścieżka fizyczna** wpisz (lub przejdź do) ścieżka do folderu lokalnego (na przykład **C:\DemoSite**).
7. W **portu** wpisz numer portu, na którym chcesz hosta witryny sieci Web (na przykład **85**).

    > [!NOTE]
    > Numery portów standardowe są 80 dla protokołu HTTP i 443 dla protokołu HTTPS. Jednak w przypadku obsługi tej witryny sieci Web na porcie 80, należy zatrzymać domyślnej witryny sieci Web, zanim można uzyskiwać dostęp do witryny.
8. Pozostaw **nazwy hosta** pole pozostanie puste, chyba że chcesz skonfigurować rekord systemu nazw domen (DNS, Domain Name System) dla witryny sieci Web, a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image4.png)

    > [!NOTE]
    > W środowisku produkcyjnym należy do hostowania witryny sieci Web na porcie 80 i konfigurowanie nagłówka hosta, wraz z pasujących rekordów DNS. Aby uzyskać więcej informacji na temat konfigurowania nagłówków hosta w usługach IIS 7, zobacz [skonfigurować nagłówek hosta dla witryny sieci Web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Aby uzyskać więcej informacji o roli serwera DNS w systemie Windows Server 2008 R2, zobacz [Omówienie serwera DNS](https://technet.microsoft.com/en-gb/library/cc770392.aspx) i [serwera DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. W **akcje** okienku w obszarze **edytowanie witryny**, kliknij przycisk **powiązania**.
10. W **powiązania witryny** okno dialogowe, kliknij przycisk **Dodaj**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image5.png)
11. W **Dodawanie powiązania witryny** okno dialogowe, zestaw **adres IP** i **portu** odpowiadające istniejącą konfigurację lokacji.
12. W **nazwy hosta** wpisz nazwę serwera sieci web (na przykład **TESTWEB1**), a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image6.png)

    > [!NOTE]
    > Pierwszy powiązania witryny umożliwia dostęp do witryny lokalnie za pomocą adresu IP i portu lub `http://localhost:85`. Drugi powiązania witryny umożliwia dostęp do witryny z innych komputerów w domenie przy użyciu nazwy komputera (na przykład http://testweb1:85).
13. W **powiązania witryny** okno dialogowe, kliknij przycisk **Zamknij**.
14. W **połączeń** okienku, kliknij przycisk **pul aplikacji**.
15. W **pul aplikacji** okienku kliknij prawym przyciskiem myszy nazwę puli aplikacji, a następnie kliknij przycisk **podstawowe ustawienia**. Domyślnie nazwa puli aplikacji będzie zgodna z nazwą witryny sieci Web (na przykład **DemoSite**).
16. W **.NET Framework w wersji** listy, wybierz **4.0.30319 .NET Framework**, a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image7.png)

    > [!NOTE]
    > Przykładowe rozwiązanie wymaga programu .NET Framework 4.0. To nie jest wymagane dla narzędzia Web Deploy w zasadzie.

Aby witryny sieci Web do obsługi zawartości tożsamość puli aplikacji musi mieć uprawnienia odczytu na folder lokalny, na którym jest przechowywana zawartość. W usługach IIS 7.5 lub pul aplikacji za pomocą tożsamości puli aplikacji są domyślnie uruchamiane (w przeciwieństwie do poprzednich wersji usług IIS, w których pule aplikacji czy są zazwyczaj uruchamiane przy użyciu konta Usługa sieciowa). Tożsamość puli aplikacji nie jest kontem rzeczywistego użytkownika i nie jest wyświetlany na listę użytkowników lub grup&#x2014;zamiast tego należy go jest tworzony dynamicznie po uruchomieniu puli aplikacji. Każda tożsamość puli aplikacji zostanie dodany do lokalnej **IIS\_IUSRS** grupy zabezpieczeń jako element ukryty.

Aby udzielić uprawnień do pliku lub folderu, tożsamość puli aplikacji są dostępne dwie opcje:

- Przypisywanie uprawnień do odpowiedniej tożsamości puli aplikacji bezpośrednio, w formacie <strong>IIS AppPool\</ strong ><em>[Nazwa puli aplikacji]</em>(na przykład <strong>IIS AppPool\DemoSite</strong>).
- Przypisywanie uprawnień do **IIS\_IUSRS** grupy.

Najbardziej typowym podejściem jest przypisywanie uprawnień do lokalnej **IIS\_IUSRS** grupy, ponieważ takie podejście pozwala zmienić pule aplikacji bez konieczności ponownej konfiguracji uprawnień systemu plików. Następna procedura korzysta z tej metody oparte na grupach.

> [!NOTE]
> Aby uzyskać więcej informacji o tożsamości puli aplikacji w usługach IIS w wersji 7.5, zobacz [tożsamości puli aplikacji](https://go.microsoft.com/?linkid=9805123).


**Aby skonfigurować uprawnienia do folderu witryny sieci Web usług IIS**

1. W Eksploratorze Windows przejdź do lokalizacji folderu lokalnego.
2. Kliknij prawym przyciskiem myszy folder, a następnie kliknij przycisk **właściwości**.
3. Na **zabezpieczeń** , kliknij pozycję **Edytuj**, a następnie kliknij przycisk **Dodaj**.
4. Kliknij przycisk **lokalizacje**. W **lokalizacje** okno dialogowe, wybierz serwer lokalny, a następnie kliknij przycisk **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image8.png)
5. W **Wybieranie użytkowników lub grup** okno dialogowe, typ **IIS\_IUSRS**, kliknij przycisk **Sprawdź nazwy**, a następnie kliknij przycisk **OK**.
6. W <strong>uprawnienia dla</strong><em>[nazwa folderu]</em>okno dialogowe, zwróć uwagę, że nowa grupa zostanie przypisana <strong>odczytu &amp; wykonania</strong>, <strong>listy folderów zawartość</strong>, i <strong>odczytu</strong> uprawnienia domyślne. Pozostaw to bez zmian i kliknij przycisk <strong>OK</strong>.
7. Kliknij przycisk <strong>OK</strong> zamknąć <em>[nazwa folderu]</em><strong>właściwości</strong> okno dialogowe.

Jako ostatnim zadaniem przed przystąpieniem do wdrażania żadnych pakietów sieci web z serwerem, należy upewnieniu się, że jest uruchomiona usługa agenta sieci Web wdrożenia. Podczas wdrażania pakietu z komputera zdalnego, Usługa agenta sieci Web wdrażania jest odpowiedzialny za wyodrębnianie i instalowanie zawartości pakietu. Usługa jest uruchamiane domyślnie po zainstalowaniu narzędzia wdrażania Web i zostanie uruchomiona w ramach tożsamości Network Service.

Można sprawdzić czy usługa jest uruchomiona w wielu różne sposoby, za pomocą różnych narzędzi wiersza polecenia lub poleceń cmdlet programu Windows PowerShell. W tej procedurze opisano prosta metoda oparty na interfejsie użytkownika.

**Aby sprawdzić, czy jest uruchomiona usługa agenta sieci Web wdrożenia**

1. Na **Start** menu wskaż **narzędzia administracyjne**, a następnie kliknij przycisk **usług**.
2. Zlokalizuj **Usługa agenta sieci Web wdrożenia** wiersza, a następnie sprawdź, czy **stan** ustawiono **uruchomiono**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image9.png)
3. Jeśli usługa nie jest już uruchomiona, kliknij przycisk **Start**.

## <a name="configure-firewall-exceptions"></a>Konfiguracja wyjątków zapory

Domyślnie usługa agenta zdalnego nasłuchuje na porcie TCP 80, pod tym adresem URL:

http:// [<em>nazwy serwera</em>] / MSDEPLOYAGENTSERVICE

W większości przypadków nie można skonfigurować reguł zapory dodatkowych dla zdalnej usługi agenta, ponieważ serwery sieci web zwykle nasłuchiwać żądań HTTP na porcie 80. Po dostosowaniu instalacji nasłuchiwanie na porcie niestandardowym, należy do skonfigurowania wyjątków zapory, zgodnie z wymaganiami.

## <a name="conclusion"></a>Wniosek

W tym momencie serwer sieci web jest gotowy do zaakceptowania i zainstalowania pakietów sieci web z komputera zdalnego. Przed przystąpieniem do wdrażania aplikacji sieci web na serwerze, można sprawdzić punkty klucza:

- Zostały zarejestrowane programu ASP.NET 4.0 z usługami IIS?
- Tożsamość puli aplikacji ma dostęp do odczytu do folderu źródłowego dla witryny sieci Web?
- Czy jest uruchomiona usługa agenta sieci Web wdrożenia?

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać wskazówki dotyczące sposobu konfigurowania niestandardowe pliki projektu Microsoft kompilacji Engine (MSBuild) do wdrażania pakietów sieci web do zdalnej usługi agenta, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Poprzednie](scenario-configuring-a-production-environment-for-web-deployment.md)
> [dalej](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
