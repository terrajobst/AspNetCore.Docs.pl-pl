---
uid: whitepapers/denied-access-to-iis-directories
title: Platforma ASP.NET odmawia dostępu do katalogów usług IIS | Dokumentacja firmy Microsoft
author: rick-anderson
description: Tym oficjalnym dokumencie opisano, co należy zrobić, jeśli żądanie do aplikacji platformy ASP.NET zwraca błąd "odmowa dostępu do katalogu DirectoryName. Nie udało się s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: ''
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: 22868738e02472f5f433c729967ac324131a0fdc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383065"
---
<a name="aspnet-denied-access-to-iis-directories"></a>Platforma ASP.NET odmawia dostępu do katalogów usług IIS
====================
> Tym oficjalnym dokumencie opisano, co należy zrobić, jeśli żądanie do aplikacji platformy ASP.NET zwraca błąd, "odmowa dostępu do *DirectoryName* katalogu. Nie można uruchomić monitorowania zmiany w katalogu".
> 
> Stosuje się do platformy ASP.NET w wersji 1.0 i 1.1 programu ASP.NET.


Teraz RTM programu ASP.NET w wersji 1 jest wykonywane przy użyciu mniej uprzywilejowanego konta systemu windows — zarejestrowany jako konto "ASPNET" na komputerze lokalnym.

Na niektórych zablokowana systemów, to konto może domyślnie nie ma zabezpieczeń dostęp do odczytu katalogi z zawartością witryny sieci Web, katalog główny aplikacji lub katalogu głównego witryny sieci web. W takim przypadku zostanie wyświetlony następujący błąd podczas żądania strony z danej aplikacji sieci web:

![](denied-access-to-iis-directories/_static/image1.jpg)

Aby rozwiązać ten problem, musisz zmienić uprawnienia zabezpieczeń w odpowiednich katalogów.

W szczególności platformy ASP.NET wymaga odczytu, wykonywanie i listy dostępu do konta ASPNET dla głównym witryny sieci web (na przykład: c:\inetpub\wwwroot lub dowolnego katalogu alternatywnych lokacji, może zostały skonfigurowane w usługach IIS), katalog zawartości i katalog główny aplikacji Aby można było monitorować zmian w pliku konfiguracji. Katalog główny aplikacji odnosi się do ścieżki folderu skojarzony z katalogiem wirtualnym aplikacji za pomocą narzędzia administracyjnego usług IIS (inetmgr).

Na przykład należy wziąć pod uwagę następujące hierarchii aplikacji w folderze wwwroot.

`C:\inetpub\wwwroot\myapp\default.aspx`

W tym przykładzie konto ASPNET wymaga uprawnień odczytu zdefiniowanych powyżej dla zawartości zarówno w myapp, jak i w katalogu wwwroot. Pojedynczy dziedziczone listy ACL dla folderu głównego Opcjonalnie można także w przypadku obu katalogów, jeśli są one zagnieżdżone.

Aby dodać uprawnienia do katalogu, wykonaj następujące czynności:

- W Eksploratorze Windows przejdź do katalogu
- Kliknij prawym przyciskiem folder katalogu i wybierz pozycję "Właściwości"
- Przejdź do karty "Zabezpieczenia" w oknie dialogowym właściwości
- Kliknij przycisk "Dodaj" i wprowadź nazwę komputera, a następnie według nazwy konta ASPNET. Na przykład na komputerze o nazwie "webdev", może wprowadzić urzWeb\ASPNET i kliknę przycisk "OK".
- Upewnij się, że konto ASPNET ma "odczytu &amp; wykonania", "Wyświetlanie zawartości folderu" i "Odczyt" odpowiednie pola wyboru zaznaczone.
- Kliknę przycisk OK, aby zamknąć okno dialogowe i zapisać zmiany.

![](denied-access-to-iis-directories/_static/image2.jpg)

Jeśli to konieczne, te zmiany można zautomatyzować za pomocą skryptów lub narzędzi "cacls.exe", który jest dostarczany z Windows. Aby uzyskać więcej informacji na temat konto ASPNET, zobacz [dokumentu — często zadawane pytania](https://go.microsoft.com/fwlink/?LinkId=5828).

Jeśli danej aplikacji sieci web opiera się na potrzeby zapisu lub modyfikować uprawnienia do określonego folderu lub pliku, mogą być udzielane zgodnie z tą samą procedurą, a następnie zaznaczając pole wyboru "Write" i/lub "Modyfikuj".

W przypadku maszyn, które zezwalają na wszystkich użytkowników lub dostęp do odczytu grupę użytkowników do tych katalogów, (które jest konfiguracja domyślna), zostanie napotkana żadnych problemów i powyższe kroki nie będą wymagane.
