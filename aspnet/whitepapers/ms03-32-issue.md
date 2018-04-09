---
uid: whitepapers/ms03-32-issue
title: Poprawka błędu "Aplikacja serwera niedostępna", po zastosowaniu aktualizacji zabezpieczeń dla programu Internet Explorer | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym dokumencie opisano poprawkę, która rozwiązuje problem MS03 32 Aktualizacja zabezpieczeń dla programu Internet Explorer, która wpływa na aplikacje programu ASP.NET w wersji 1.0 Wi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: dd1a564cd347364abc3ca5ac0a9ffda448bcede8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Napraw błąd "Aplikacja serwera niedostępna" po zastosowaniu aktualizacji zabezpieczeń dla programu Internet Explorer
====================
> W tym dokumencie opisano poprawkę, która rozwiązuje problem MS03 32 Aktualizacja zabezpieczeń dla programu Internet Explorer, która wpływa na aplikacje ASP.NET 1.0 w systemie Windows XP Professional.
> 
> Dotyczy ASP.NET 1.0 i Windows XP Professional.


Firma Microsoft znalazła problemu z MS03-32 Aktualizacja zabezpieczeń dla poziomu poprawki zabezpieczeń programu Internet Explorer i 1.0 ASP.NET działającą w systemie Windows XP. Ta poprawka można zainstalować ręcznie lub uzyskując najnowsze aktualizacje krytyczne z witryny Windows Update.

Objawem tego problemu jest, że po zainstalowaniu poprawki na komputerze z systemem Windows XP, wszystkie żądania kierowane do aplikacji platformy ASP.NET działających na lokalnym serwerze sieci web IIS 5.1 skutkować komunikat o błędzie informujący "Serwera aplikacji niedostępna". Nie dotyczy to żądań do serwerów sieci web do zdalnego.

Ten problem wpływa tylko na instalacje programem ASP.NET 1.0 w systemie Windows XP. Nie wpływa na komputerach z systemem Windows 2000 lub Windows Server 2003. Ponadto nie ma ona wpływu komputerów z systemem Windows XP z ASP.NET 1.1 zainstalowany.

Należy pamiętać, że ten problem **nie jest** usterki zabezpieczeń z programem ASP.NET. On **nie** otwarcie lub zezwalanie na wszystkie złośliwe ataki aplikacji ASP.NET lub serwera. Zamiast tego jest wyłącznie funkcjonalności błąd spowodowany przez sam poprawki.

Pracujemy na trwałe rozwiązanie tego problemu. W międzyczasie można wykonywać następujące plik wsadowy jako obejścia problemu. Plik wsadowy wykonuje następujące czynności:

1. Zatrzymuje usługi stanu usług IIS i platformy ASP.NET
2. Usuwa i ponownie utworzy konto ASPNET znane hasło tymczasowe
3. Korzysta z systemu Windows `runas` polecenia do uruchomienia pliku wykonywalnego, który tworzy profil użytkownika ASPNET
4. Re-registers ASP.NET. To powoduje utworzenie nowego losowe hasło dla konta i stosuje domyślne ustawienia kontroli dostępu platformy ASP.NET dla niego
5. Uruchamia ponownie usługi IIS

Plik wsadowy zawiera zapisane na stałe tymczasowe hasło "<strong>1pass@word</strong>", który będzie wyświetlany monit o wprowadzenie dla polecenia runas podczas uruchamiania pliku wsadowego. Po zakończeniu wykonywania polecenia Uruchom jako hasła do konta ASPNET zostaje odtworzone w silne losowe wartości. Należy pamiętać, że plik wsadowy może zakończyć się niepowodzeniem, jeśli zapisane na stałe hasło nie spełnia wymagania dotyczące złożoności hasła w danym środowisku. Jeśli tak jest, można zmienić ją na inną wartość, która jest odpowiednia dla danego środowiska.

*> [!IMPORTANT]* Jeśli ustawienia kontroli dostępu niestandardowych lub uprawnień konta bazy danych dla konta ASPNET został dodany, muszą być utworzone ponownie po ukończeniu tego pliku wsadowego. Jest to spowodowane po utworzeniu konta pobierze nowy identyfikator zabezpieczeń (SID).

*> [!IMPORTANT]* Jeśli korzystasz z procesu roboczego ASP.NET z kontem niestandardowym innym niż konto ASPNET, następnie nie należy uruchamiać ten plik wsadowy. Zamiast tego należy logowania interakcyjnego lub przy użyciu tego konta, co spowoduje utworzenie profilu użytkownika dla tego konta za pomocą polecenia Uruchom jako.

Plik wsadowy jest zawarte w archiwum samorozpakowujący się poniżej. Aby użyć go:

1. Użytkownik musi być uruchomiony jako konto z uprawnieniami administratora
2. [Pobierz i otwórz samorozpakowujący się plik wykonywalny](ms03-32-issue/_static/fixup1.exe)
3. Wyodrębnij zawartość do c:\
4. Wybierz polecenie Uruchom... z start menu, a następnie wprowadź `cmd.exe`
5. W oknie polecenia Otwórz wpisz `c:\fixup.cmd`.
6. Po wyświetleniu monitu wprowadź <strong>1pass@word</strong> jako hasło.
7. Jeśli ustawienia kontroli dostępu wcześniej niestandardowych lub uprawnień konta bazy danych dla konta ASPNET, należy ponownie zastosować te ustawienia teraz.

Wiele apologies za niedogodności spowodowało. Dodatkowe informacje będą publikowane po jej udostępnieniu.

Macierz poniżej szczegóły platform i wersje, które wpływa ten problem.

| .NET Framework | Platforma | Dotyczy |
| --- | --- | --- |
| W wersji 1.0 | Windows 2000 Professional | Nie |
| W wersji 1.0 | Windows 2000 Server | Nie |
| W wersji 1.0 | Windows XP Professional | Tak |
| W wersji 1.0 | Windows Server 2003 | Nie |
| W wersji 1.0 | Windows XP Home z Cassini | Nie |
| W wersji 1.1 | Windows 2000 Professional | Nie |
| W wersji 1.1 | Windows 2000 Server | Nie |
| W wersji 1.1 | Windows XP Professional | Nie |
| W wersji 1.1 | Windows Server 2003 | Nie |
| W wersji 1.1 | Windows XP Home z Cassini | Nie |

Dziękujemy,   
 Zespół programu ASP.NET
