---
uid: whitepapers/side-by-side-with-10
title: Wykonanie programu ASP.NET Side-by-Side programu .NET Framework 1.0 i 1.1 | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten dokument zawiera opis sposobu instalowania zarówno .NET 1.0 i .NET 1.1 na tym komputerze, umożliwiając uruchamianie na danej wersji obra aplikacji sieci Web ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 939b04d440955b184bf5f4c40a2ef8175641edfa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573359"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>Wykonanie programu ASP.NET Side-by-Side programu .NET Framework 1.0 i 1.1
====================
> Ten dokument zawiera opis sposobu instalowania zarówno .NET 1.0 i .NET 1.1 na tym komputerze, umożliwiając aplikacji sieci Web ASP.NET do uruchamiania w obu wersji Framework.
> 
> Dotyczy ASP.NET 1.0 i 1.1 programu ASP.NET.


W programie ASP.NET aplikacje są określane jako być uruchomiona obok siebie, gdy są zainstalowane na tym samym komputerze, ale korzystają z różnych wersji programu .NET Framework. Poniższy temat zawiera opis sposobu konfigurowania aplikacji ASP.NET do wykonywania side-by-side i zawiera szczegółowy opis kroków, aby:

- [Obsługa mapowania aplikacji sieci Web .NET Framework w wersji 1.0 podczas instalacji](#1)
- [Mapa aplikacji sieci Web do określonej wersji programu .NET Framework](#2)
- [Znajdź wersji programu .NET Framework, która używa witryny sieci Web](#3)

Zazwyczaj podczas aktualizacji składnika lub aplikacji na komputerze, starsza wersja jest usunięte i zastąpione nowszą wersję. Jeśli nowa wersja nie jest zgodny z poprzedniej wersji, to zwykle dzieli inne aplikacje, które używają tego składnika lub aplikacji. .NET Framework zapewnia obsługę wykonywania side-by-side, dzięki któremu wiele wersji zestawu lub aplikacji do zainstalowania na tym samym komputerze, w tym samym czasie. Ponieważ jednocześnie można zainstalować wiele wersji, zarządzane aplikacje mogą wybierać wersji do użycia, bez wywierania wpływu na aplikacje, które używają różnych wersji.

Domyślnie podczas instalacji programu .NET Framework w wersji 1.1, wszystkie istniejące aplikacje ASP.NET są automatycznie konfigurowane, aby używać najnowszej wersji programu .NET Framework. Jeśli nie chcesz, aby aplikacje ASP.NET do domyślnego programu .NET Framework 1.1, kliknij przycisk [tutaj](#1) Aby dowiedzieć się, jak temu zapobiec, podczas instalacji.

Jeśli aktualizacja serwera sieci Web do programu .NET Framework 1.1 i mają co najmniej jednej aplikacji sieci Web do uruchamiania programu .NET Framework 1.0, musisz zaktualizować mapę skryptu Internet Information Services (IIS). Mapowanie skryptu to mechanizm mapowania rozszerzenia plików .aspx dla określonej aplikacji sieci Web do wersji programu .NET Framework. Kliknij przycisk [tutaj](#2) sposób mapowania aplikacji sieci Web do określonej wersji programu .NET Framework.

Możesz użyć Menedżera informacji Internet lub narzędzie rejestracji programu ASP.NET usług IIS (Aspnet\_regiis.exe) można znaleźć określonej aplikacji sieci Web jest posiadanej wersji .NET Framework. Kliknij przycisk [tutaj](#3) więcej informacji na temat można znaleźć wersji programu .NET Framework, która używa witryny sieci Web.

Jeden import podczas migracji do programu .NET Framework 1.1 to, że każda wersja programu .NET Framework używa własnego pliku Machine.config. W związku z tym kontem administratora sieci Web wprowadził zmiany w pliku Machine.config, muszą być migrowane do pliku Machine.config programu .NET Framework 1.1 tych zmian.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Obsługa mapowania aplikacji sieci Web do programu .NET Framework 1.0 podczas instalacji

Domyślnie wszystkie istniejące aplikacje ASP.NET są automatycznie konfigurowane podczas instalacji do nowszej wersji programu .NET Framework. Przy użyciu nowszej wersji programu .NET Framework, aplikacje można w pełni korzystać ulepszeń oraz nowych funkcji dostępnych w nowej wersji. W tym samym czasie administratora sieci Web, który może być szczegółową kontrolę nad aplikacji, które zostały zaktualizowane, można zapobiec automatycznego ponownego mapowania wszystkich istniejących aplikacji ASP.NET podczas instalacji programu .NET Framework.

Aby uniknąć automatycznego ponownego mapowania całej aplikacji ASP.NET do nowszej wersji programu .NET Framework, administrator sieci Web służy opcji wiersza polecenia/noaspupgrade Dotnetfx.exe programu instalacyjnego.

**Aby zapobiec całkowita ponowne mapowanie aplikacji ASP.NET do nowszej wersji**

1. Przejdź do **Start**.
2. Polecenie **Uruchom**.
3. Typ **cmd**.
4. Kliknij przycisk **OK**.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. W wierszu polecenia, wpisz następujące polecenie, aby rozpocząć instalację programu .NET Framework: **Dotnetfx.exe / / c: "Zainstaluj/noaspupgrade?**.  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Kliknij przycisk **tak** w instalacji programu Microsoft .NET Framework 1.1. Spowoduje to uruchomienie procesu instalacji programu .NET Framework 1.1.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Mapa aplikacji sieci Web do określonej wersji programu .NET Framework

Każda wersja programu .NET Framework obejmuje wersję narzędzia rejestracji ASP.NET usług IIS (Aspnet\_regiis.exe). To narzędzie umożliwia administratorom określenie, że w obszarze określonej wersji programu .NET Framework można uruchomić aplikacji sieci Web. Jest to określane jako mapowanie aplikacji sieci Web do wersji programu .NET Framework. Administratorzy muszą wybrać Aspnet\_regiis.exe odpowiadający wersji programu .NET Framework, która zostanie skojarzona z aplikacją sieci Web. Na przykład Administratorzy, którzy chcą, aby określić witrynę sieci Web, użyj programu .NET Framework 1.1 musi użyć Aspnet\_regiis.exe dołączoną do programu .NET Framework 1.1.

Aspnet\_regiis.exe dla wersji 1.0 znajduje się pod adresem:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705** \aspnet\_regiis

Aspnet\_regiis.exe dla wersji 1,1 znajduje się pod adresem:

- C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322** \aspnet\_regiis

Aspnet\_regiis.exe są dostępne dwie opcje dla skryptu mapowania aplikacji sieci Web:

- **-s** ustawia mapę skryptu w ścieżce i jego podrzędny katalogów.
- **-sn** ustawia mapę skryptu tylko na ścieżce.

Ścieżka definiuje sieci Web aplikacji internetowych usług informacyjnych metadanych ścieżki, która jest zdefiniowana w formie W3SVC/ROOT / {WebSiteNumber} / {aplikacji\_nazwa}. Na przykład dla aplikacji sieci Web, nazywany portalem znajduje się w domyślnej witryny sieci Web, ścieżka metabazy jest W3SVC/1/główny/portalu.

![](side-by-side-with-10/_static/image4.gif)

Należy pamiętać, że umożliwia także narzędzie edytora metabazy można uzyskać ścieżki metabazy. To narzędzie można pobrać z witryny Microsoft Support w [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- Uruchom Aspnet\_regiis.exe -s W3SVC/1/główny/portalu, aby zaktualizować portalu usług IIS skrypt mapy i jego subapplication.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Uruchom Aspnet\_regiis.exe -sn mapować W3SVC/1/główny/portalu, aby zaktualizować ten skrypt portalu usług IIS, bez wywierania wpływu na aplikacje w portalu? s podkatalogów.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Znajdź wersja programu .NET Framework, który jest używany przez aplikację sieci Web

Administrator może użyć Menedżera usług internetowych, aby znaleźć witryny sieci Web jest uruchamiana wersji programu .NET Framework. Wersje systemów operacyjnych Uruchom Menedżera usług internetowych inaczej. Aby uruchomić programu service manager, wykonaj kroki opisane poniżej.

**Aby uruchomić Menedżera usług internetowych**

1. Przejdź do **Start**.
2. Polecenie **Uruchom**.
3. Typ **inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. Z Menedżera usług internetowych, wybierz aplikację sieci Web, którego wersja programu .NET Framework, aby dowiedzieć się.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Kliknij prawym przyciskiem myszy w aplikacji sieci Web, a następnie kliknij polecenie **właściwości.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. W oknie właściwości wybierz **konfiguracji.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. Wybierz z tabeli mapowania aplikacji **.aspx**i kliknij przycisk **Edytuj**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. Z **plik wykonywalny** pole tekstowe, znajduje się w katalogu wersji przewijając widok. Jeśli katalog wersji jest v.1.1.4322, aplikacja zostaje zmapowana do programu .NET Framework 1.1. Z drugiej strony Jeśli katalog wersji jest v1.0.3705, aplikacja zostaje zmapowana do programu .NET Framework 1.0.  
  
    ![](side-by-side-with-10/_static/image12.gif)
