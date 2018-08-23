---
uid: whitepapers/ms03-32-issue
title: Poprawka dla błędu "Aplikacja serwera niedostępna" po zastosowaniu aktualizacji zabezpieczeń dla programu Internet Explorer | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym dokumencie opisano poprawkę, która rozwiązuje problem z MS03 32 Aktualizacja zabezpieczeń programu Internet Explorer, który wpływa na aplikacje programu ASP.NET 1.0 systemem Wi...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: ce2d705a93577b0c6d28f86069873c6ecd891db6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757353"
---
<a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Naprawiono błąd "Aplikacja serwera niedostępna" po zastosowaniu aktualizacji zabezpieczeń dla programu Internet Explorer
====================
> W tym dokumencie opisano poprawkę, która rozwiązuje problem z MS03 32 Aktualizacja zabezpieczeń programu Internet Explorer, która ma wpływ na aplikacje programu ASP.NET w wersji 1.0, systemem Windows XP Professional.
> 
> Stosuje się do platformy ASP.NET w wersji 1.0 i Windows XP Professional.


Firma Microsoft znalazła wystąpił problem z MS03 32 aktualizacji zabezpieczeń dla poziomu poprawki zabezpieczeń programu Internet Explorer i ASP.NET 1.0 w systemie Windows XP. Ta poprawka można zainstalować ręcznie lub za pomocą uzyskiwanie najnowszych aktualizacji krytycznych z witryny Windows Update.

Objawem tego problemu jest tym, że po zainstalowaniu poprawki na komputerze Windows XP, wszystkie żądania do aplikacji platformy ASP.NET działających na lokalnym serwerze sieci web usługi IIS 5.1 skutkować komunikat o błędzie informujący o tym, "Serwer aplikacji niedostępna". Nie dotyczy to żądań do serwerów sieci web do zdalnego.

Ten problem ma wpływ tylko na instalacji uruchomionych ASP.NET 1.0 w systemie Windows XP. Nie ma wpływu na maszyny z systemem Windows 2000 lub Windows Server 2003. Ponadto nie ma ona wpływu maszyn z systemem Windows XP przy użyciu platformy ASP.NET 1.1 zainstalowany.

Należy pamiętać, że ten problem **nie** błędów zabezpieczeń, za pomocą platformy ASP.NET. Jego **nie** otwierają lub zezwalanie na wszystkie złośliwe ataki względem aplikacji ASP.NET lub serwera. Zamiast tego jest wyłącznie funkcjonalności usterka spowodowana przez sam poprawkę.

Ciężko pracujemy na trwałe rozwiązanie tego problemu. W międzyczasie można wykonywać następujący plik wsadowy, jako obejście dla problemu. Plik wsadowy wykonuje następujące czynności:

1. Zatrzymanie usług stanu usług IIS i platformy ASP.NET
2. Usuwa i odtwarza konto ASPNET znanych hasło tymczasowe
3. Używa Windows `runas` polecenie, aby uruchomić plik wykonywalny, który tworzy profil użytkownika ASPNET
4. Re-registers ASP.NET. Spowoduje to utworzenie nowego losowe hasło dla konta i stosuje domyślne ustawienia kontroli dostępu platformy ASP.NET dla niego
5. Uruchamia ponownie usługę IIS

Plik wsadowy zawiera zapisane na stałe tymczasowe hasło "<strong>1pass@word</strong>", który będzie wyświetlany monit o wprowadzenie dla polecenia Uruchom jako, po uruchomieniu pliku wsadowego. Po zakończeniu wykonywania polecenia Uruchom jako hasło do konta ASPNET są odtwarzane z silną losową wartość. Należy pamiętać, że plik wsadowy może zakończyć się niepowodzeniem, jeśli hasło zapisane na stałe nie spełnia wymagania dotyczące złożoności hasła w danym środowisku. Jeśli tak jest rzeczywiście, możesz go zmienić na inną wartość, która jest odpowiednia dla użytkowanego środowiska.

*> [!IMPORTANT]* Po dodaniu ustawienia kontroli dostępu niestandardowych lub uprawnień konta bazy danych dla konta ASPNET muszą być utworzone ponownie po ukończeniu tego pliku wsadowego. Jest to spowodowane po utworzeniu konta otrzyma nowy identyfikator zabezpieczeń (SID).

*> [!IMPORTANT]* Jeśli korzystasz z procesu roboczego ASP.NET, przy użyciu niestandardowego konta inne niż konto ASPNET, następnie nie należy uruchamiać ten plik wsadowy. Zamiast tego należy zalogować się interaktywnie lub użyć polecenia Uruchom jako przy użyciu tego konta, które spowoduje utworzenie profilu użytkownika dla tego konta.

Plik wsadowy jest zawarte w archiwum samorozpakowujący się poniżej. Aby użyć go:

1. Użytkownik musi działać jako konto z uprawnieniami administratora
2. [Pobierz i otwórz samorozpakowujący się plik wykonywalny](ms03-32-issue/_static/fixup1.exe)
3. Wyodrębnij zawartość do c:\
4. Wybierz polecenie Uruchom... z start menu, a następnie wprowadź `cmd.exe`
5. W oknie polecenia Otwórz typ `c:\fixup.cmd`.
6. Po wyświetleniu monitu wprowadź <strong>1pass@word</strong> jako hasło.
7. W przypadku ustawienia kontroli dostępu niestandardowych wcześniej lub uprawnień konta bazy danych dla konta ASPNET, należy ponownie zastosować te ustawienia teraz.

Wiele przeprosinami za niedogodności, który spowodował ten. Firma Microsoft prześle dodatkowe informacje po jej udostępnieniu.

Tabela poniżej szczegółowo platformy i wersje dotyczy ten problem.

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
