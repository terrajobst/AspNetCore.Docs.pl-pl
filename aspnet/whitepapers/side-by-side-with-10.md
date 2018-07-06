---
uid: whitepapers/side-by-side-with-10
title: ASP.NET Side-by-Side wykonywania programu .NET Framework 1.0 i 1.1 | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten dokument zawiera opis sposobu instalowania .NET 1.0 i 1.1 platformy .NET na komputerze, umożliwiając aplikacji sieci Web platformy ASP.NET do uruchamiania na dowolną wersję chronometraż...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 1018845e3d2c6603732bfbbde78f4a9183e49d5d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808398"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>ASP.NET Side-by-Side wykonywania programu .NET Framework 1.0 i 1.1
====================
> Ten dokument zawiera opis sposobu instalowania .NET 1.0 i 1.1 platformy .NET na komputerze, umożliwiając aplikacji sieci Web platformy ASP.NET do uruchamiania na obu wersji framework.
> 
> Stosuje się do platformy ASP.NET w wersji 1.0 i 1.1 programu ASP.NET.


W programie ASP.NET: aplikacje mówi się, że jest uruchomiona obok siebie, gdy są zainstalowane na tym samym komputerze, ale korzystają z różnych wersji programu .NET Framework. Poniższy temat zawiera opis sposobu konfigurowania aplikacji ASP.NET w celu wykonania side-by-side i zawiera szczegółowy opis kroków:

- [Obsługa mapowania aplikacji sieci Web platformy .NET Framework w wersji 1.0, podczas instalacji](#1)
- [Mapa aplikacji sieci Web do określonej wersji programu .NET Framework](#2)
- [Znajdź wersję .NET Framework, która używa witryny sieci Web](#3)

Tradycyjnie zaktualizowaniu składnik lub pojedyncza aplikacja na komputerze, starsza wersja została usunięta i zastąpiona nowszą wersją. Jeśli nowa wersja nie jest zgodny z poprzednią wersją, to zwykle przerywa inne aplikacje, które używają składnika lub aplikacji. .NET Framework zapewnia obsługę wykonywania side-by-side, dzięki któremu wiele wersji zestawu lub aplikacji do zainstalowania na tym samym komputerze, w tym samym czasie. Ponieważ jednocześnie może być zainstalowanych wiele wersji, zarządzanych aplikacji można wybrać wersji bez wpływu na aplikacje, które korzystają z różnych wersji.

Domyślnie podczas instalacji programu .NET Framework w wersji 1.1, wszystkich istniejących aplikacji ASP.NET są automatycznie konfigurowane do używania najnowszej wersji programu .NET Framework. Jeśli chcesz, aby aplikacje ASP.NET domyślnie .NET Framework 1.1, kliknij przycisk [tutaj](#1) dowiesz się, jak można temu zapobiec, podczas instalacji.

Jeśli aktualizacja serwera sieci Web platformy .NET Framework 1.1 i ma jedną lub więcej aplikacji sieci Web do uruchamiania programu .NET Framework 1.0, musisz zaktualizować mapę skryptu Internet Information Services (IIS). Mapowanie skryptu jest mechanizmem mapowania rozszerzenia pliku .aspx dla określonej aplikacji sieci Web do wersji programu .NET Framework. Kliknij przycisk [tutaj](#2) dowiesz się, jak do mapowania aplikacji sieci Web do określonej wersji programu .NET Framework.

Możesz użyć Menedżera informacji Internet lub narzędzia rejestracji usług IIS platformy ASP.NET (Aspnet\_regiis.exe) można znaleźć, która wersja środowiska .NET Framework działa konkretnej aplikacji sieci Web. Kliknij przycisk [tutaj](#3) dowiesz się, jak można znaleźć wersji programu .NET Framework, która używa witryny sieci Web.

Pierwsza kwestia importu podczas migracji do programu .NET Framework 1.1 jest, że każda wersja programu .NET Framework używa własnego pliku Machine.config. W rezultacie administrator sieci Web wprowadził zmiany do pliku Machine.config, muszą być migrowane do pliku Machine.config programu .NET Framework 1.1 tych zmian.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Obsługa aplikacji sieci Web mapowania do .NET Framework 1.0, podczas instalacji

Domyślnie wszystkie istniejące aplikacje ASP.NET są automatycznie konfigurowane podczas instalacji w celu korzystania z nowszej wersji programu .NET Framework. Przy użyciu nowszej wersji programu .NET Framework, aplikacje mogą wykorzystać wprowadzono szereg usprawnień i nowych funkcji dostępnych w nowej wersji. W tym samym czasie administratora sieci Web, który może być zapewnia szczegółową kontrolę aplikacji, które zostały zaktualizowane, może uniemożliwić automatyczne ponowne mapowanie wszystkich istniejących aplikacji programu ASP.NET podczas instalacji programu .NET Framework.

Aby uniknąć automatycznego ponownego mapowania całej aplikacji ASP.NET do nowszej wersji programu .NET Framework, administrator sieci Web można za pomocą opcji wiersza polecenia/noaspupgrade Dotnetfx.exe program instalacyjny.

**Aby uniknąć ponownego mapowania łączna liczba aplikacji ASP.NET do nowszej wersji**

1. Przejdź do **Start**.
2. Kliknij pozycję **Uruchom**.
3. Typ **cmd**.
4. Kliknij przycisk **OK**.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. W wierszu polecenia, wpisz następujący wiersz, aby rozpocząć instalację programu .NET Framework: **/c: Dotnetfx.exe "Instalowanie/noaspupgrade?**.  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Kliknij przycisk **tak** w Instalatora programu Microsoft .NET Framework 1.1. Spowoduje to uruchomienie procesu instalacji programu .NET Framework 1.1.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Mapa aplikacji sieci Web do określonej wersji programu .NET Framework

Każda wersja programu .NET Framework zawiera wersję narzędzia rejestracji usług IIS platformy ASP.NET (Aspnet\_regiis.exe). To narzędzie umożliwia administratorom określenie, czy aplikacja sieci Web, być uruchamiany w kontekście określonej wersji programu .NET Framework. Jest to określane jako mapowanie aplikacji sieci Web do wersji programu .NET Framework. Administratorzy muszą wybrać Aspnet\_regiis.exe odpowiadającą wersję programu .NET Framework, która zostanie skojarzona z aplikacją sieci Web. Na przykład administrator, który chce, aby określić, że witryny sieci Web używa .NET Framework 1.1, należy użyć Aspnet\_regiis.exe dostarczanego z .NET Framework 1.1.

Aspnet\_regiis.exe dla wersji 1.0 znajduje się w folderze:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis

Aspnet\_regiis.exe w wersji 1,1 znajduje się w folderze:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis

Aspnet\_regiis.exe oferuje dwie opcje skryptów, mapowania aplikacji sieci Web:

- **-s** ustawia mapę skryptu w ścieżce i w jego podrzędny katalogów.
- **-sn** ustawia mapę skryptu w tylko ścieżki.

Ścieżka definiuje sieci Web usług IIS metadanych ścieżce aplikacji, która jest zdefiniowana w formie W3SVC/ROOT / {WebSiteNumber} / {aplikacji\_nazwa}. Na przykład dla aplikacji sieci Web o nazwie Portal znajduje się w obszarze Domyślna witryna sieci Web, ścieżka metabazy jest W3SVC/1/główny/Portal.

![](side-by-side-with-10/_static/image4.gif)

Należy pamiętać, możesz również użyć z narzędzia o nazwie Edytor metabazy, aby pobrać ścieżkę do metabazy. To narzędzie można pobrać z witryny Microsoft Support w [ https://support.microsoft.com/default.aspx?scid=kb; en-us; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- Uruchom Aspnet\_mapy i jego subapplication skryptów regiis.exe -s W3SVC/1/główny/portalu, aby zaktualizować portal usług IIS.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Uruchom Aspnet\_regiis.exe -sn mapować W3SVC/1/główny/portalu, aby zaktualizować portal skryptów usług IIS, bez wywierania wpływu na aplikacje w portalu? s podkatalogów.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Znajdź wersji .NET Framework, który jest używany przez aplikację sieci Web

Administrator może użyć Menedżera usług internetowych, aby dowiedzieć się, która wersja programu .NET Framework działa witryny sieci Web. Wersje systemów operacyjnych Uruchom Menedżera usług internetowych inaczej. Aby uruchomić programu service manager, wykonaj kroki opisane poniżej.

**Aby uruchomić Menedżera usług internetowych**

1. Przejdź do **Start**.
2. Kliknij pozycję **Uruchom**.
3. Typ **inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. Z Menedżera usług internetowych, wybierz aplikację sieci Web, których wersji programu .NET Framework, aby dowiedzieć się.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Kliknij prawym przyciskiem myszy w aplikacji sieci Web, a następnie kliknij **właściwości.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. W oknie właściwości wybierz **konfiguracji.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. Wybierz z tabeli mapowania aplikacji **.aspx**i kliknij przycisk **Edytuj**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. Z **plik wykonywalny** polu tekstowym Szukaj w katalogu wersji przewijając. Jeśli w katalogu wersji v.1.1.4322, aplikacja zostaje zmapowana do .NET Framework 1.1. Z drugiej strony Jeśli w katalogu wersji v1.0.3705, aplikacja zostaje zmapowana do .NET Framework 1.0.  
  
    ![](side-by-side-with-10/_static/image12.gif)
