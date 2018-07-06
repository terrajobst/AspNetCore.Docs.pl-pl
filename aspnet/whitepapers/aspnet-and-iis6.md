---
uid: whitepapers/aspnet-and-iis6
title: Uruchamianie platformy ASP.NET 1.1 z usługami IIS 6.0 | Dokumentacja firmy Microsoft
author: rick-anderson
description: Gdy system Windows Server 2003 obejmuje zarówno usług IIS 6.0 i ASP.NET 1.1, składniki te są domyślnie wyłączone. Ten dokument opisuje sposób włączania usług IIS 6.0...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 338059104f46a6c5517212db6e1a54c12677e133
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839297"
---
<a name="running-aspnet-11-with-iis-60"></a>Uruchamianie platformy ASP.NET 1.1 z usługami IIS 6.0
====================
> Gdy system Windows Server 2003 obejmuje zarówno usług IIS 6.0 i ASP.NET 1.1, składniki te są domyślnie wyłączone. Ten oficjalny dokument, w tym artykule opisano sposób włączania usług IIS 6.0 i ASP.NET 1.1 i zaleca kilka ustawień konfiguracji, aby uzyskać optymalną wydajność usług IIS i platformy ASP.NET.
> 
> Stosuje się do platformy ASP.NET 1.1 oraz IIS 6.0.


Platformy ASP.NET 1.1 jest dostarczany z programem Windows Server 2003, która zawiera też najnowszą wersję programu Internet Information Server (IIS) w wersji 6.0. Usługi IIS 6.0 i ASP.NET 1.1 są przeznaczone do bezproblemowo integrują się i platformy ASP.NET teraz wartością domyślną jest nowy model proces roboczy usług IIS 6.0.

## <a name="aspnet-11-is-not-installed-by-default"></a>Platformy ASP.NET 1.1 nie jest instalowany domyślnie

W przeciwieństwie do poprzednich wersji systemów operacyjnych serwera firmy Microsoft Internet Information Server (IIS) jest domyślnie wyłączona; nie jest ASP.NET 1.1. Dostępne są dwie opcje umożliwiające usług IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>Włączanie usług IIS, opcja #1 — Kreator konfiguracji serwera

Windows Server 2003 jest dostarczany nowe "Kreatora konfiguracji serwera" Aby poprawnie skonfigurować serwer w trybie żądaną.

Aby uruchomić kreatora — należy pamiętać, aby uruchomić kreatora, użytkownik musi być zalogowany jako administrator — przejdź do: Start | Programy | Narzędzia administracyjne i wybierz opcję "Konfigurowanie serwera".

Po wybraniu, powinien zostać wyświetlony ekran powitalny "Kreatora konfiguracji serwera":

![](aspnet-and-iis6/_static/image1.jpg)

Kliknij przycisk "dalej &gt;":

![](aspnet-and-iis6/_static/image2.jpg)

Kliknij przycisk "dalej &gt;"

![](aspnet-and-iis6/_static/image3.jpg)

Na tym ekranie należy wybrać "serwer aplikacji (usługi IIS, ASP.NET) jako opcji, aby skonfigurować.

Kliknij przycisk "dalej &gt;".

![](aspnet-and-iis6/_static/image4.jpg)

Po wybraniu, aby skonfigurować serwer jako serwer aplikacji, ten ekran pojawi się monit, jakie dodatkowe funkcje, które powinny być instalowane. Żadna opcja jest zaznaczona domyślnie. Aby włączyć automatyczne ASP.NET, należy wybrać "Włącz ASP. NET ".

Kliknij przycisk "dalej &gt;".

![](aspnet-and-iis6/_static/image5.jpg)

Ten ekran przedstawia opcje są do zainstalowania.

Kliknij przycisk "dalej &gt;".

![](aspnet-and-iis6/_static/image6.jpg)

Zostanie wyświetlony następujący ekran, gdy wybrane opcje są instalowane. Jest normalnym zjawiskiem, inne okno dialogowe, które pola są wyświetlane jako usługi, są instalowane. Ponadto monit może zostać lokalizacji programu instalacyjnego dysku CD Windows 2003 Server.

Kliknij przycisk "dalej &gt;" po zakończeniu.

![](aspnet-and-iis6/_static/image7.jpg)

Kliknij przycisk "Zakończ",-Windows Server 2003 jest teraz skonfigurowane do obsługi usług IIS 6.0 i ASP.NET 1.1.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>Włączanie usług IIS, opcja #2 - ręcznego konfigurowania usług IIS i platformy ASP.NET

Jeśli nie chcesz używać "Kreatora konfigurowania serwera" możesz opcjonalnie zainstalować usług IIS 6.0 i ASP.NET 1.1 apletu "Dodaj lub usuń programy" w Panelu sterowania.

Najpierw otwórz Panel sterowania:

![](aspnet-and-iis6/_static/image8.jpg)

Następnie kliknij przycisk "Dodaj lub usuń Windows składników" zostanie otwarte Kreatora składników Windows:

![](aspnet-and-iis6/_static/image9.jpg)

Wyróżnij i sprawdź "Serwer aplikacji", a następnie kliknij przycisk "Szczegóły"? Przycisk:

![](aspnet-and-iis6/_static/image10.jpg)

Aby zainstalować program ASP.NET, zapoznaj się z ' ASP. NET ".

Kliknij przycisk OK, aby powrócić do Kreatora składników Windows. Kliknij przycisk "dalej &gt;" za pomocą Kreatora składnika Windows, aby rozpocząć instalowanie:

![](aspnet-and-iis6/_static/image11.jpg)

Jest normalnym zjawiskiem, inne okno dialogowe, które pola są wyświetlane jako usługi, są instalowane. Ponadto monit może zostać lokalizacji programu instalacyjnego dysku CD Windows 2003 Server.

Po zakończeniu instalacji zostanie wyświetlony ostatni ekran kreatora składnika Windows:

![](aspnet-and-iis6/_static/image12.jpg)

Usługi IIS 6.0 i ASP.NET 1.1 są teraz skonfigurowane i dostępne.

## <a name="recommended-settings"></a>Zalecane ustawienia

Podczas uruchamiania 1.1 platformy ASP.NET w usługach IIS 6.0, istnieje kilka ustawień konfiguracji, które są zalecane, aby uzyskać optymalną wydajność z platformy ASP.NET:

- Konfigurowanie limity pamięci procesu roboczego
- Konfigurowanie procesu podrzędnego

### <a name="configuring-worker-process-memory-limits"></a>Konfigurowanie limity pamięci procesu roboczego

Domyślnie usługi IIS 6.0 nie ustawia limit ilości pamięci, która może używać usług IIS. ASP. NET w pamięci podręcznej funkcji opiera się na ograniczenie pamięci, dlatego pamięci podręcznej aktywnie usunąć nieużywane elementy z pamięci.

Zalecane jest skonfigurowanie odtwarzanie funkcja usług IIS 6.0 pamięci. Aby skonfigurować ten Otwórz Menedżera internetowych usług informacyjnych (Start | Programy | Narzędzia administracyjne | Internet Information Services). Po otwarciu rozwiń folder "Puli aplikacji":

Dla każdej puli aplikacji:

![](aspnet-and-iis6/_static/image13.jpg)

1. Kliknij prawym przyciskiem myszy pulę aplikacji, np. "DefaultAppPool" i wybierz polecenie Właściwości:

![](aspnet-and-iis6/_static/image14.jpg)

2. Następnie Włącz odtwarzanie pamięci, klikając opcję "maksymalne użycie pamięci (w megabajtach):". Wartość nie powinna być większa niż ilość pamięci fizycznej (nie wirtualnym) na serwerze, dobrego pamięci fizycznej, np. do serwera z 512MB pamięci fizycznej wybierz 310 o 60%. Zalecane jest również, że maksymalna przekracza 800MB podczas korzystania z przestrzeni adresowej 2GB. Jeśli przestrzeń adresowa pamięci serwera to 3GB, limit pamięci maksymalnej dla procesu roboczego może być możliwie jak 1, 800MB:

![](aspnet-and-iis6/_static/image15.jpg)

Kliknij przycisk "Zastosuj" i "OK", aby zamknąć okno dialogowe właściwości. Powtórz tę czynność dla wszystkich dostępnych pul aplikacji.

### <a name="configuring-worker-recycling"></a>Konfigurowanie odtwarzania procesu roboczego

Domyślnie usługi IIS 6.0 jest skonfigurowany do Odtwórz jej proces roboczy co 29 godziny. Jest to nieco agresywne dla aplikacji ASP.NET i zaleca się, że automatycznego procesu podrzędnego jest wyłączona.

Aby wyłączyć automatyczne procesu podrzędnego, najpierw otwórz Menedżera internetowych usług informacyjnych (Start | Programy | Narzędzia administracyjne | Internet Information Services). Po otwarciu rozwiń folder "Puli aplikacji":

![](aspnet-and-iis6/_static/image16.jpg)

Dla każdej puli aplikacji:

1. Kliknij prawym przyciskiem myszy pulę aplikacji, np. "DefaultAppPool" i wybierz polecenie Właściwości:

![](aspnet-and-iis6/_static/image17.jpg)

2. Usuń zaznaczenie pola wyboru "Odtwórz proces roboczy (w minutach):":

![](aspnet-and-iis6/_static/image18.jpg)

Kliknij przycisk "Zastosuj" i "OK", aby zamknąć okno dialogowe właściwości. Powtórz tę czynność dla wszystkich dostępnych pul aplikacji.

## <a name="granting-write-access-to-the-file-system"></a>Udzielanie uprawnienia do zapisu w systemie plików

Jeśli aplikacja wymaga uprawnienia do zapisu w systemie plików, a w przypadku korzystania z systemu plików NTFS, będzie konieczne zmodyfikowanie kontroli dostępu listy (ACL) na folder lub plik, aby udzielić dostępu programu ASP.NET do.

Na przykład udzielić ASP.NET uprawnienia do zapisu w c:\inetpub\wwwroot najpierw otwórz Eksploratora i przejdź do katalogu:

![](aspnet-and-iis6/_static/image19.jpg)

Następnie kliknij prawym przyciskiem myszy w katalogu, np. "wwwroot" i wybierz polecenie Właściwości. Po otwarciu okna dialogowego właściwości wybierz kartę "Zabezpieczenia":

![](aspnet-and-iis6/_static/image20.jpg)

Katalog c:\inetpub\wwwroot\ jest specjalne w tym specjalnej grupy usług IIS 6.0 "usług IIS\_WPG" jest już przydzielone odczytu &amp; uprawnień wykonywanie, wyświetlanie zawartości folderu oraz Odczyt. Jednak aby udzielić uprawnień do zapisu, musisz kliknąć przycisk wyboru Zezwalaj zapisu:

![](aspnet-and-iis6/_static/image21.jpg)

Usługi IIS 6.0 ma teraz uprawnienia do zapisu dla tego folderu. Aby przyznać uprawnienia do zapisu w innych folderach, wykonaj poniższe kroki, — należy pamiętać, może być konieczne dodanie usług IIS\_WPG grupy, jeśli jeszcze nie istnieje.

> [!CAUTION]
> Udzielanie uprawnienia zapisu w programu IIS\_WPG umożliwi dowolnej aplikacji platformy ASP.NET do zapisu do tego katalogu.

## <a name="supporting-integrated-authentication-with-sql-server"></a>Obsługa uwierzytelniania zintegrowanego z programem SQL Server

Zintegrowane uwierzytelnianie umożliwia SQL Server, aby korzystać z uwierzytelniania systemu Windows NT do sprawdzania poprawności konta logowania programu SQL Server. Dzięki temu użytkownikowi na pominięcie standardowego procesu logowania programu SQL Server. Takie podejście użytkownik sieci umożliwia dostęp do bazy danych programu SQL Server bez podawania identyfikator oddzielne logowania i hasła, ponieważ program SQL Server uzyskuje użytkownika i hasło z procesu zabezpieczeń sieci systemu Windows NT.

Wybieranie uwierzytelniania zintegrowanego dla aplikacji ASP.NET jest dobrym rozwiązaniem, ponieważ żadne poświadczenia nie nigdy nie są przechowywane w ramach parametrów połączenia dla aplikacji. Zamiast parametry połączenia używane do połączenia z serwerem SQL będzie wyglądać w następujący sposób:

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Te parametry połączenia zawiera informacje dla programu SQL Server przy użyciu poświadczeń Windows aplikacji, próba dostępu do serwera SQL. W przypadku ASP.NET/IIS 6 powinien to być konto w usługach IIS\_WPG grupy.

Aby włączyć zintegrowane uwierzytelnianie między serwerem SQL i programu ASP.NET, należy najpierw upewnić się, że program SQL Server jest skonfigurowany dla zintegrowanego uwierzytelniania lub uwierzytelnianie w trybie mieszanym — skontaktuj się z administratora bazy danych, aby to ustalić. Jeśli program SQL Server znajduje się w jednym z tych dwóch trybów, można użyć uwierzytelniania zintegrowanego.

Otwórz program SQL Server Enterprise Manager (Start | Programy | Program Microsoft SQL Server | Enterprise Manager), wybierz odpowiedni serwer, a następnie rozwiń folder zabezpieczeń:

![](aspnet-and-iis6/_static/image22.jpg)

Jeśli "BUILTINT\IIS\_WPG" grupa nie ma na liście, kliknij prawym przyciskiem myszy na nazwach logowania i wybierz nowy identyfikator logowania:

![](aspnet-and-iis6/_static/image23.jpg)

W "Nazwa:" Wprowadź w polu tekstowym "\IIS [nazwa serwera/domeny]\_WPG" lub kliknij przycisk wielokropka, aby otworzyć selektor użytkownika/grupy systemu Windows NT:

![](aspnet-and-iis6/_static/image24.jpg)

Wybierz pozycję IIS bieżącej maszyny\_WPG grupy i kliknij przycisk "Add" i OK, aby zamknąć selektora.

Następnie należy również ustawić domyślną bazę danych i uprawnień dostępu do bazy danych. Można ustawić domyślną bazę danych wybierz z listy rozwijanej listy, np. poniżej Northwind jest zaznaczone:

![](aspnet-and-iis6/_static/image25.jpg)

Następnie kliknij na karcie dostęp do bazy danych:

![](aspnet-and-iis6/_static/image26.jpg)

Kliknij pole wyboru dla każdej bazy danych, który chcesz zezwolić na dostęp do zezwalania na. Należy również wybrać ról bazy danych, sprawdzanie db\_właściciela zapewni identyfikatora logowania ma wszystkie uprawnienia wymagane do zarządzania i użyć wybranej bazy danych.

Kliknij przycisk OK, aby zamknąć okno dialogowe właściwości. Twoja aplikacja ASP.NET jest teraz skonfigurowane do obsługi zintegrowanego uwierzytelniania programu SQL Server.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>Nie uruchamiaj programu ASP.NET 1.0 w trybie macierzystym usług IIS 6.0

ASP.NET 1.0 z usługami IIS 6.0 jest obsługiwana tylko w trybie zgodności usług IIS 5.

Aby skonfigurować ASP.NET 1.0 do uruchamiania w trybie zgodności z programem IIS 5.0, otworzyć Menedżera usług internetowych i witryn sieci Web kliknij prawym przyciskiem myszy i wybierz polecenie Właściwości:

![](aspnet-and-iis6/_static/image27.jpg)

Przejdź do karty usługę i sprawdź? Uruchom usługę WWW w trybie izolacji 5.0 usług IIS?:

![](aspnet-and-iis6/_static/image28.jpg)
