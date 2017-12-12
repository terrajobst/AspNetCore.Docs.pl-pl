---
uid: whitepapers/aspnet-and-iis6
title: "Uruchomiony program ASP.NET 1.1 z usługami IIS 6.0 | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "Gdy system Windows Server 2003 obejmuje zarówno usług IIS 6.0 i ASP.NET 1.1, składniki te są domyślnie wyłączone. Ten dokument zawiera opis sposobu włączania usług IIS 6.0..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1fcac7b8bc295ccf4e36189295b6bc2e4d328623
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="running-aspnet-11-with-iis-60"></a>Uruchomiony program ASP.NET 1.1 z usługami IIS 6.0
====================
> Gdy system Windows Server 2003 obejmuje zarówno usług IIS 6.0 i ASP.NET 1.1, składniki te są domyślnie wyłączone. Ten dokument zawiera opis sposobu włączania usług IIS 6.0 i ASP.NET 1.1 i zaleca kilka ustawień konfiguracji, aby uzyskać optymalną wydajność usług IIS i platformy ASP.NET.
> 
> Dotyczy ASP.NET 1.1 i IIS 6.0.


ASP.NET 1.1 jest dostarczany z programem Windows Server 2003, który obejmuje również najnowszej wersji programu Internet Information Server (IIS) w wersji 6.0. Usługi IIS 6.0 i ASP.NET 1.1 są przeznaczone do integrują i ASP.NET teraz domyślnie ustawiany na nowy model procesu roboczego usług IIS 6.0.

## <a name="aspnet-11-is-not-installed-by-default"></a>Domyślnie nie jest zainstalowany program ASP.NET 1.1

W przeciwieństwie do poprzednich wersji systemów operacyjnych firmy Microsoft serwera Internet Information Server (IIS) nie jest włączona domyślnie; nie jest ASP.NET 1.1. Dostępne są dwie opcje dotyczące włączania usługi IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>Włączanie usług IIS, opcja #1 - Kreatora konfiguracji serwera

Windows Server 2003 jest dostarczany nowe "Kreator konfigurowania serwera" Aby poprawnie skonfigurować serwer w trybie żądany.

Aby uruchomić Kreatora - note, aby uruchomić kreatora, musi być zalogowany jako administrator - przejdź do: Start | Programy | Narzędzia administracyjne i wybierz opcję "Konfigurowanie serwera".

Po wybraniu powinna zostać wyświetlona ekranu otwierającego 'Kreator konfigurowania serwera':

![](aspnet-and-iis6/_static/image1.jpg)

Kliknij przycisk "dalej &gt;":

![](aspnet-and-iis6/_static/image2.jpg)

Kliknij przycisk "dalej &gt;"

![](aspnet-and-iis6/_static/image3.jpg)

Na tym ekranie musisz wybrać "serwer aplikacji (IIS, platformy ASP.NET) jako opcje do skonfigurowania.

Kliknij przycisk "dalej &gt;".

![](aspnet-and-iis6/_static/image4.jpg)

Po wybraniu, aby skonfigurować serwer jako serwer aplikacji, zostanie wyświetlony ten ekran monitowania, jakie dodatkowe funkcje powinna zostać zainstalowana. Żadna opcja jest domyślnie zaznaczona. Aby automatycznie włączyć platformę ASP.NET, należy wybrać "Włącz ASP. NET ".

Kliknij przycisk "dalej &gt;".

![](aspnet-and-iis6/_static/image5.jpg)

Ten ekran wyświetla, jakie są opcje do zainstalowania.

Kliknij przycisk "dalej &gt;".

![](aspnet-and-iis6/_static/image6.jpg)

Zostanie wyświetlony następujący ekran, gdy wybrane opcje są instalowane. Jest to normalne, aby wyświetlić inne okna dialogowego, które pola są wyświetlane jako usługi są instalowane. Może również wyświetlony monit dla lokalizacji programu instalacyjnego dysku CD systemu Windows Server 2003.

Kliknij przycisk "dalej &gt;" po zakończeniu.

![](aspnet-and-iis6/_static/image7.jpg)

Kliknij przycisk "Zakończ" - Windows Server 2003 jest teraz skonfigurowane do obsługi usług IIS 6.0 i ASP.NET 1.1.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>Włączanie usług IIS, opcja #2 - ręcznego konfigurowania usług IIS i platformy ASP.NET

Jeśli nie chcesz, aby użyć "Kreator konfigurowania serwera" można opcjonalnie zainstalować usług IIS 6.0 i ASP.NET 1.1 apletu "Dodaj lub usuń programy" w Panelu sterowania.

Najpierw otwórz Panel sterowania:

![](aspnet-and-iis6/_static/image8.jpg)

Następnie kliknij przycisk "Dodaj/Usuń składniki systemu Windows", który zostanie otwarty Kreator składników systemu Windows:

![](aspnet-and-iis6/_static/image9.jpg)

Wyróżnij i zaznacz pozycję "Serwer aplikacji", a następnie kliknij przycisk "Szczegóły"? przycisk:

![](aspnet-and-iis6/_static/image10.jpg)

Aby zainstalować program ASP.NET, sprawdź "ASP. NET ".

Kliknij przycisk OK, aby powrócić do Kreatora składników systemu Windows. Kliknij przycisk "dalej &gt;" z Kreatora składników systemu Windows, aby rozpocząć instalowanie:

![](aspnet-and-iis6/_static/image11.jpg)

Jest to normalne, aby wyświetlić inne okna dialogowego, które pola są wyświetlane jako usługi są instalowane. Może również wyświetlony monit dla lokalizacji programu instalacyjnego dysku CD systemu Windows Server 2003.

Po zakończeniu instalacji zostanie wyświetlony ostatni ekran kreatora składników systemu Windows:

![](aspnet-and-iis6/_static/image12.jpg)

Usługi IIS 6.0 i ASP.NET 1.1 są teraz skonfigurowane i dostępne.

## <a name="recommended-settings"></a>Zalecane ustawienia

Podczas uruchamiania ASP.NET 1.1 w usługach IIS 6.0, istnieje kilka ustawień konfiguracji, które są zalecane, aby uzyskać optymalną wydajność z platformy ASP.NET:

- Konfigurowanie limitów pamięci procesu roboczego
- Konfigurowanie procesu podrzędnego

### <a name="configuring-worker-process-memory-limits"></a>Konfigurowanie limitów pamięci procesu roboczego

Domyślnie usługi IIS 6.0 nie ustawić limit ilości pamięci, która może używać usług IIS. ŚRODOWISKO ASP. NET w pamięci podręcznej funkcji polega na ograniczenia pamięci, więc pamięci podręcznej z pamięci można usunąć aktywnego nieużywanych elementów.

Zalecane jest skonfigurowanie pamięci odtwarzania funkcja usług IIS 6.0. Aby skonfigurować ten Otwórz Menedżera internetowych usług informacyjnych (Start | Programy | Narzędzia administracyjne | Internet Information Services). Po otwarciu rozwiń folder "Puli aplikacji":

Dla każdej puli aplikacji:

![](aspnet-and-iis6/_static/image13.jpg)

1. Kliknij prawym przyciskiem myszy pulę aplikacji, np. "Domyślna pula aplikacji", a następnie wybierz opcję 'właściwości':

![](aspnet-and-iis6/_static/image14.jpg)

2. Następnie włącz opcję odtwarzanie pamięci, klikając jeden "maksymalne użycie pamięci (w MB):". Wartość nie powinna być większa niż ilość pamięci fizycznej (niewirtualna) na serwerze, dobrego 60% pamięci fizycznej, np. do serwera z 512MB pamięci fizycznej wybierz 310. Zalecane jest również, że maksymalna przekracza 800MB przy użyciu przestrzeni adresowej 2GB. Jeśli przestrzeń adresów pamięci serwera jest 3GB, może być możliwie jak 1, 800MB limit pamięci maksymalnej dla procesu roboczego:

![](aspnet-and-iis6/_static/image15.jpg)

Kliknij przycisk "Zastosuj" i "OK", aby zamknąć okno dialogowe właściwości. Powtórz tę czynność dla wszystkich dostępnych pul aplikacji.

### <a name="configuring-worker-recycling"></a>Konfigurowanie odtwarzania procesu roboczego

Domyślnie usług IIS 6.0 jest skonfigurowany do odtworzenia procesu roboczego co 29 godzin. Jest nieco agresywne aplikacji opartych na programie ASP.NET i zaleca się, że automatyczne procesu podrzędnego jest wyłączona.

Aby wyłączyć automatyczne procesu podrzędnego, najpierw otwórz Menedżera internetowych usług informacyjnych (Start | Programy | Narzędzia administracyjne | Internet Information Services). Po otwarciu rozwiń folder "Puli aplikacji":

![](aspnet-and-iis6/_static/image16.jpg)

Dla każdej puli aplikacji:

1. Kliknij prawym przyciskiem myszy pulę aplikacji, np. "Domyślna pula aplikacji", a następnie wybierz opcję 'właściwości':

![](aspnet-and-iis6/_static/image17.jpg)

2. Usuń zaznaczenie pola wyboru "Odtwórz proces roboczy (w minutach):":

![](aspnet-and-iis6/_static/image18.jpg)

Kliknij przycisk "Zastosuj" i "OK", aby zamknąć okno dialogowe właściwości. Powtórz tę czynność dla wszystkich dostępnych pul aplikacji.

## <a name="granting-write-access-to-the-file-system"></a>Udzielanie dostępu do zapisu w systemie plików

Jeśli aplikacja wymaga uprawnienia do zapisu w systemie plików i używasz należy zmodyfikować kontroli dostępu listy (ACL) na folder lub plik, aby przyznać aplikacji ASP.NET dostępu do systemu plików NTFS.

Na przykład aby udzielić ASP.NET do zapisu c:\inetpub\wwwroot najpierw otwórz Eksploratora i przejdź do katalogu:

![](aspnet-and-iis6/_static/image19.jpg)

Następnie kliknij prawym przyciskiem myszy w katalogu, np. "wwwroot" i wybierz polecenie Właściwości. Po otwarciu okna dialogowego właściwości, wybierz kartę "Zabezpieczenia":

![](aspnet-and-iis6/_static/image20.jpg)

Katalog c:\inetpub\wwwroot\ jest specjalne katalogu, w tym specjalnych grupy usług IIS 6.0 "IIS\_WPG" jest już przydzielone odczytu &amp; uprawnienia Execute, wyświetlanie zawartości folderu i Odczyt. Jednak aby udzielić uprawnień do zapisu, musisz kliknąć przycisk wyboru Zezwalaj do zapisu:

![](aspnet-and-iis6/_static/image21.jpg)

Usługi IIS 6.0 ma teraz uprawnienia do zapisu w tym folderze. Aby przyznać uprawnienia do zapisu w innych folderach, wykonaj te kroki — Uwaga: może być konieczne dodanie IIS\_WPG grupy, jeśli jeszcze nie istnieje.

> [!CAUTION]
> Udzielanie uprawnienie do zapisu w usługach IIS\_WPG umożliwi dowolnej aplikacji ASP.NET do zapisu do tego katalogu.

## <a name="supporting-integrated-authentication-with-sql-server"></a>Obsługa uwierzytelniania zintegrowanego z programem SQL Server

Uwierzytelnianie zintegrowane umożliwia dla programu SQL Server korzystać z uwierzytelniania systemu Windows NT można sprawdzić poprawności konta logowania programu SQL Server. Dzięki temu użytkownikowi na pominięcie w standardowym procesie logowania programu SQL Server. Takie podejście użytkownik sieci umożliwia dostęp do bazy danych programu SQL Server bez podawania identyfikator oddzielne logowania i hasła, ponieważ program SQL Server uzyskuje użytkownika i hasło z procesu zabezpieczeń sieci systemu Windows NT.

Wybór zintegrowanego uwierzytelniania dla aplikacji ASP.NET jest dobrym rozwiązaniem, ponieważ nie poświadczenia kiedykolwiek są przechowywane w ciągu połączenia dla aplikacji. Zamiast parametry połączenia używane do połączenia z serwerem SQL będzie wyglądać w następujący sposób:

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Ten ciąg połączenia informuje programu SQL Server do używania poświadczeń systemu Windows aplikacji próby uzyskania dostępu do programu SQL Server. W przypadku ASP.NET/IIS 6 to konto w usługach IIS\_WPG grupy.

Aby włączyć zintegrowane uwierzytelnianie między serwerem SQL i programu ASP.NET, należy najpierw upewnij się, że program SQL Server jest skonfigurowany dla zintegrowanego uwierzytelniania lub uwierzytelniania w trybie mieszanym - skontaktuj się z administratora bazy danych, aby ustalić tę liczbę. Jeśli program SQL Server znajduje się w jednym z tych dwóch trybów, można użyć zintegrowane uwierzytelnianie.

Otwórz Menedżer usług SQL Server Enterprise (Start | Programy | Program Microsoft SQL Server | Enterprise Manager), wybierz odpowiedni serwer, a następnie rozwiń folder zabezpieczeń:

![](aspnet-and-iis6/_static/image22.jpg)

Jeśli "BUILTINT\IIS\_WPG" grupa nie ma na liście, kliknij prawym przyciskiem myszy na nazwy logowania i wybrać nowe dane logowania:

![](aspnet-and-iis6/_static/image23.jpg)

W "Nazwa:' pole tekstowe albo wprowadź" [nazwa serwera/domeny] \IIS\_WPG "lub kliknij przycisk z wielokropkiem, aby otworzyć okno wybierania użytkownika/grupy systemu Windows NT:

![](aspnet-and-iis6/_static/image24.jpg)

Wybierz IIS bieżącej maszyny\_WPG grupy i kliknij przycisk "Dodaj" i OK, aby zamknąć selektora.

Następnie należy również ustawić domyślną bazę danych i uprawnień dostępu do bazy danych. Można ustawić domyślnej bazy danych wybierz z listy rozwijanej listy, np. poniżej Northwind wybrano:

![](aspnet-and-iis6/_static/image25.jpg)

Kliknij przycisk Dalej, na karcie dostęp do bazy danych:

![](aspnet-and-iis6/_static/image26.jpg)

Kliknij pole wyboru dla każdej bazy danych, którą chcesz zezwolić na dostęp do zezwalania na. Należy również wybrać role bazy danych db sprawdzanie\_właściciela zapewni logowanie ma wszystkie uprawnienia niezbędne do zarządzania i użyć wybranej bazy danych.

Kliknij przycisk OK, aby zamknąć okno dialogowe właściwości. Aplikacja ASP.NET jest teraz skonfigurowane do obsługi zintegrowanego uwierzytelniania programu SQL Server.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>Nie uruchamiaj programu ASP.NET w wersji 1.0 w trybie macierzystym usług IIS 6.0

ASP.NET 1.0 w usługach IIS 6.0 jest obsługiwana tylko w trybie zgodności usług IIS 5.

Aby skonfigurować 1.0 ASP.NET do uruchamiania w trybie zgodności z programem IIS 5.0, otwórz Menedżera usług internetowych i witryn sieci Web kliknij prawym przyciskiem myszy i wybierz polecenie Właściwości:

![](aspnet-and-iis6/_static/image27.jpg)

Przejdź do karty usługi i sprawdź? Uruchom usługę WWW w trybie izolacji 5.0 usług IIS?

![](aspnet-and-iis6/_static/image28.jpg)
