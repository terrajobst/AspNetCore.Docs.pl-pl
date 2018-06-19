---
uid: signalr/overview/older-versions/working-with-groups
title: Praca z grupami w SignalR 1.x | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym temacie opisano, jak do utrwalenia informacji o członkostwie grupy przy użyciu interfejsu API koncentratora.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 7bc0ff73ade72729cc5e1217b3fe704ac0d8cab8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036547"
---
<a name="working-with-groups-in-signalr-1x"></a>Praca z grupami w SignalR 1.x
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym temacie opisano, jak dodać użytkowników do grup i zachować informacje o członkostwie w grupie.


## <a name="overview"></a>Omówienie

Grupy w SignalR udostępnia metody emisji wiadomości do określonego podzbiór połączonych klientów. Grupa może zawierać dowolną liczbę klientów, a klient może być członkiem dowolnej liczby grup. Nie trzeba jawnie tworzyć grupy. W efekcie grupy jest tworzony automatycznie określić jego nazwę w wywołaniu Groups.Add po raz pierwszy i zostaje usunięte po usunięciu ostatniego połączenia z członkostwa w nim. Aby obejrzeć wprowadzenie do korzystania z grup, zobacz [jak zarządzać członkostwa w grupie z klasy koncentratora](index.md) w interfejsie API koncentratory — przewodnik serwera.

Brak żadnego interfejsu API do pobierania listy członkostwa grupy lub grup. SignalR wysyła wiadomości do klientów i grup na podstawie modelu pub/sub, a serwer nie przechowuje list grup lub członkostwa w grupach. Dzięki temu można zmaksymalizować skalowalność, ponieważ po dodaniu węzła do farmy sieci web SignalR zachowuje stan ma być propagowane do nowego węzła.

Podczas dodawania użytkownika do grupy przy użyciu `Groups.Add` metody, użytkownik odbiera komunikaty kierowane do tej grupy w czasie trwania bieżącego połączenia, ale członkostwa użytkownika w tej grupie nie jest trwały poza bieżącego połączenia. Jeśli chcesz trwale przechowywane informacje o grupach oraz członkostwa w grupie, dane muszą być przechowywane w repozytorium, takie jak bazy danych lub magazynu tabel platformy Azure. Następnie każdym razem, gdy użytkownik łączy do aplikacji, możesz pobrać z repozytorium, które użytkownik należy do grupy i ręcznie dodać tego użytkownika do tych grup.

Przy ponownym łączeniu po przerwaniu tymczasowe, użytkownik automatycznie ponownie łączy poprzednio przypisanych grup. Automatyczne ponowne przyłączanie grupy tylko wtedy, gdy połączyć się ponownie, nie tworząc nowe połączenie. Token podpisane cyfrowo są przekazywane z klienta, który zawiera listę grup uprzednio przypisane. Jeśli chcesz sprawdzić, czy użytkownik należy do żądanej grupy, można zastąpić domyślne zachowanie.

Ten temat zawiera następujące sekcje:

- [Dodawanie i usuwanie użytkowników](#add)
- [Wywoływanie elementów członkowskich grupy](#call)
- [Zapisywanie w bazie danych członkostwa w grupie](#storedatabase)
- [Przechowywanie członkostwa w grupie w magazynie tabel platformy Azure](#storeazuretable)
- [Sprawdzanie członkostwa w grupie przy ponownym łączeniu](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>Dodawanie i usuwanie użytkowników

Aby dodać lub usunąć użytkowników z grupy, należy wywołać [Dodaj](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) lub [Usuń](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metody i przekaż użytkownika połączenia identyfikator i nazwę grupy jako parametry. Nie trzeba ręcznie usunąć użytkownika z grupy, po zakończeniu połączenia.

W poniższym przykładzie przedstawiono `Groups.Add` i `Groups.Remove` metody używane w metodach koncentratora.

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

`Groups.Add` i `Groups.Remove` metod wykonania asynchronicznie.

Jeśli chcesz dodać do grupy klienta i natychmiast Wyślij wiadomość do klienta przy użyciu grupy masz upewnij się, że metoda Groups.Add kończy się najpierw. W poniższych przykładach kodu pokazują, jak to zrobić, za pomocą kodu, który działa w .NET 4.5 i za pomocą kodu, który działa w .NET 4.

#### <a name="asynchronous-net-45-example"></a>Asynchroniczne .NET 4.5 przykład

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a>Asynchroniczne .NET 4 przykład

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

Ogólnie rzecz biorąc, nie należy używać `await` podczas wywoływania metody `Groups.Remove` metody, ponieważ identyfikator połączenia, który próbujesz usunąć przestaną być dostępne. W takim przypadku `TaskCanceledException` jest zgłaszany po upływie limitu czasu żądania. Jeśli aplikacja musi zapewnić, że użytkownik został usunięty z grupy przed wysłaniem wiadomości do grupy, możesz dodać `await` przed Groups.Remove, a następnie catch `TaskCanceledException` wyjątek, który może zostać wygenerowany.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>Wywoływanie elementów członkowskich grupy

Możesz wysłać wiadomości do wszystkich członków grupy lub tylko członkowie określonej grupy, jak pokazano w poniższych przykładach.

- **Wszystkie** połączonych klientów w określonej grupie. 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- Wszyscy połączeni klienci w określonej grupie **oprócz określonych klientów**, zidentyfikowane przez identyfikator połączenia. 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- Wszyscy połączeni klienci w określonej grupie **oprócz klienta wywołującego**. 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>Zapisywanie w bazie danych członkostwa w grupie

Poniższe przykłady przedstawiają sposób przechowywania informacji grup i użytkowników w bazie danych. Można użyć dowolnego technologii dostępu do danych; Jednak w poniższym przykładzie pokazano sposób definiowania modeli używający narzędzia Entity Framework. Te modele jednostki odpowiadają tabele bazy danych i pola. Struktury danych może być bardzo zróżnicowana w zależności od wymagań aplikacji. Ten przykład zawiera klasę o nazwie `ConversationRoom` będzie unikatowy dla aplikacji, która umożliwia użytkownikom join konwersacji o różnych tematów, takich jak sportowych lub ogrodnictwa. W tym przykładzie również zawiera klasę dla połączenia. Klasa połączenia nie jest bezwzględnie wymagane do śledzenia członkostwa w grupie, ale często jest częścią niezawodne rozwiązanie w celu śledzenia użytkowników.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

Następnie w Centrum, można pobrać informacji o grupie i użytkownika z bazy danych i ręcznie Dodaj użytkownika do odpowiednich grup. Przykład nie ma kodu do śledzenia połączeń użytkowników. W tym przykładzie `await` — słowo kluczowe nie została zastosowana przed `Groups.Add` ponieważ wiadomość nie są natychmiast wysyłane do członków grupy. Jeśli chcesz wysłać wiadomość do wszystkich członków grupy bezpośrednio po dodaniu nowego członka, czy chcesz zastosować `await` — słowo kluczowe, aby się upewnić, że operacja asynchroniczna została ukończona.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Przechowywanie członkostwa w grupie w magazynie tabel platformy Azure

Za pomocą magazynu tabel platformy Azure do przechowywania informacji o grup i użytkowników jest podobne do korzystania z bazy danych. W poniższym przykładzie przedstawiono jednostki tabeli, która przechowuje nazwę użytkownika i nazwę grupy.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

W Centrum pobierania przypisanych grup gdy użytkownik nawiąże połączenie.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Sprawdzanie członkostwa w grupie przy ponownym łączeniu

Domyślnie SignalR automatycznie ponownie przypisuje użytkownika do odpowiednich grup przy ponownym łączeniu z tymczasowego przerw w działaniu, np. gdy połączenie jest porzucona i nawiązał ponownie, zanim upłynie limit czasu połączenia. Informacje o grupie użytkowników jest przekazywana do tokenu, gdy połączyć się ponownie, a ten token jest weryfikowany na serwerze. Informacje o proces weryfikacji ponowne przyłączanie użytkowników do grup, zobacz [ponowne przyłączanie grup przy ponownym łączeniu](index.md).

Ogólnie rzecz biorąc należy używać domyślne zachowanie automatyczne ponowne przyłączanie się, że grupy na ponowne łączenie. SignalR grup nie są przeznaczone jako mechanizm zabezpieczeń w celu ograniczania dostępu do poufnych danych. Jednak w aplikacji należy dokładnie sprawdzić członkostwa w grupie użytkownika przy ponownym łączeniu, można zastąpić domyślne zachowanie. Zmianę zachowania domyślnego można dodać obciążenie do bazy danych, ponieważ można pobrać członkostwa użytkownika w grupach, dla każdego ponowne nawiązanie połączenia, a nie tylko w przypadku, gdy użytkownik nawiąże połączenie.

Należy sprawdzić członkostwa w grupie na ponownie połączyć, utworzyć nowy moduł potoku koncentratora, który zwraca listę przypisanych grup, jak pokazano poniżej.

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

Następnie należy dodać ten moduł do potoku koncentratora, jak wyróżniono poniżej.

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
