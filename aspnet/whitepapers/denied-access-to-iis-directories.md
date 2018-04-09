---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET odmowa dostępu do katalogów usług IIS | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten dokument zawiera opis, co należy zrobić, jeśli żądanie do aplikacji platformy ASP.NET zwraca błąd "odmowa dostępu do katalogu DirectoryName. Nie można s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: d95423776a6b58fc67ae6c791685543dadd2480c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
<a name="aspnet-denied-access-to-iis-directories"></a>Odmowa dostępu do katalogów usług IIS programu ASP.NET
====================
> Ten dokument zawiera opis co należy zrobić, jeśli żądanie do aplikacji platformy ASP.NET zwraca błąd "odmowa dostępu do *DirectoryName* katalogu. Nie można uruchomić monitorowania zmian w katalogach."
> 
> Dotyczy ASP.NET 1.0 i 1.1 programu ASP.NET.


RTM programu ASP.NET w wersji 1 są obecnie uruchamiane przy użyciu mniej uprzywilejowanego konta systemu windows — w zarejestrowany jako konto "ASPNET" na komputerze lokalnym.

W przypadku niektórych zablokowana systemów, to konto może domyślnie nie ma zabezpieczeń dostęp do odczytu katalogi z zawartością witryny sieci Web, katalogu głównym aplikacji lub katalogu głównego witryny sieci web. W takim przypadku zostanie wyświetlony następujący błąd podczas żądania strony z danej aplikacji sieci web:

![](denied-access-to-iis-directories/_static/image1.jpg)

Aby rozwiązać ten problem, musisz zmienić uprawnienia zabezpieczeń do odpowiednich katalogów.

W szczególności ASP.NET wymagane prawo do odczytu, wykonaj i listy dostępu dla konta ASPNET katalogu głównego witryny sieci web (na przykład: c:\inetpub\wwwroot lub dowolnego katalogu alternatywnych lokacji mogą mieć skonfigurowane w usługach IIS), katalog zawartości i katalog główny aplikacji Aby monitorować zmian w pliku konfiguracji. Katalog główny aplikacji odpowiada ścieżkę do folderu skojarzone z katalogiem wirtualnym aplikacji za pomocą narzędzia administracyjnego usług IIS (inetmgr).

Rozważmy na przykład następująca hierarchia aplikacji w folderze wwwroot.

`C:\inetpub\wwwroot\myapp\default.aspx`

Na przykład konto ASPNET wymaga uprawnień odczytu zdefiniowanych powyżej dla zawartości w Moja_aplikacja i katalogu wwwroot. Pojedynczy dziedziczone listy ACL w folderze głównym Opcjonalnie można także dla obu katalogów, jeśli są one zagnieżdżone.

Aby dodać uprawnienia do katalogu, wykonaj następujące czynności:

- Korzystając z Eksploratora Windows, przejdź do katalogu
- Kliknij prawym przyciskiem myszy folder katalogu, a następnie wybierz pozycję "Właściwości"
- Przejdź na kartę "Zabezpieczenia" w oknie dialogowym właściwości
- Kliknij przycisk "Dodaj" i wprowadź nazwę komputera, a następnie nazwę konta ASPNET. Czy na przykład na komputerze o nazwie "webdev", wprowadź urzWeb\ASPNET i kliknij przycisk "OK".
- Upewnij się, że konto ASPNET ma "odczytu &amp; Execute", "Wyświetlanie zawartości folderu" i "Odczyt" pola wyboru zaznaczone.
- Kliknij przycisk OK, aby zamknąć okno dialogowe i zapisać zmiany.

![](denied-access-to-iis-directories/_static/image2.jpg)

W razie potrzeby, te zmiany można zautomatyzować za pomocą skryptów lub narzędzia "cacls.exe", która jest dostarczana z systemem Windows. Aby uzyskać więcej informacji dotyczących konto ASPNET, zobacz [dokumentu — często zadawane pytania](https://go.microsoft.com/fwlink/?LinkId=5828).

Jeśli aplikacja sieci web danego opiera się na potrzeby zapisu lub modyfikowania uprawnień do danego folderu lub pliku, może zostać przydzielony procedury i sprawdzając pola wyboru "Write" i/lub "Modyfikuj".

Na maszynach, które umożliwia Wszyscy lub dostęp do odczytu grupy użytkowników do tych katalogów (co to jest konfiguracja domyślna), będzie napotkano brak problemów i powyższe kroki nie będą wymagane.
