---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Praca z grupami w SignalR | Dokumentacja firmy Microsoft
author: pfletcher
description: "W tym temacie opisano, jak do utrwalenia informacji o członkostwie grupy przy użyciu interfejsu API koncentratora."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 3befcdbbc735dc4f64c714ba583e026c0c19465d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="working-with-groups-in-signalr"></a>Praca z grupami w SignalR
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym temacie opisano, jak dodać użytkowników do grup i zachować informacje o członkostwie w grupie. 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używane w tym temacie
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR w wersji 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Poprzednie wersje tego tematu
> 
> Aby uzyskać informacje dotyczące starszych wersji biblioteki signalr, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony. Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Omówienie

Grupy w SignalR udostępnia metody emisji wiadomości do określonego podzbiór połączonych klientów. Grupa może zawierać dowolną liczbę klientów, a klient może być członkiem dowolnej liczby grup. Nie trzeba jawnie tworzyć grupy. W efekcie grupy jest tworzony automatycznie określić jego nazwę w wywołaniu Groups.Add po raz pierwszy i zostaje usunięte po usunięciu ostatniego połączenia z członkostwa w nim. Aby obejrzeć wprowadzenie do korzystania z grup, zobacz [jak zarządzać członkostwa w grupie z klasy koncentratora](hubs-api-guide-server.md#groupsfromhub) w interfejsie API koncentratory — przewodnik serwera.

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

Aby dodać lub usunąć użytkowników z grupy, należy wywołać [Dodaj](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) lub [Usuń](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metody i przekaż użytkownika połączenia identyfikator i nazwę grupy jako parametry. Nie trzeba ręcznie usunąć użytkownika z grupy, po zakończeniu połączenia.

W poniższym przykładzie przedstawiono `Groups.Add` i `Groups.Remove` metody używane w metodach koncentratora.

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

`Groups.Add` i `Groups.Remove` metod wykonania asynchronicznie.

Jeśli chcesz dodać do grupy klienta i natychmiast Wyślij wiadomość do klienta przy użyciu grupy masz upewnij się, że metoda Groups.Add kończy się najpierw. W poniższych przykładach kodu pokazują, jak to zrobić.

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

Ogólnie rzecz biorąc, nie należy używać `await` podczas wywoływania metody `Groups.Remove` metody, ponieważ identyfikator połączenia, który próbujesz usunąć przestaną być dostępne. W takim przypadku `TaskCanceledException` jest zgłaszany po upływie limitu czasu żądania. Jeśli aplikacja musi zapewnić, że użytkownik został usunięty z grupy przed wysłaniem wiadomości do grupy, możesz dodać `await` przed Groups.Remove, a następnie catch `TaskCanceledException` wyjątek, który może zostać wygenerowany.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>Wywoływanie elementów członkowskich grupy

Możesz wysłać wiadomości do wszystkich członków grupy lub tylko członkowie określonej grupy, jak pokazano w poniższych przykładach.

- **Wszystkie** połączonych klientów w określonej grupie. 

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- Wszyscy połączeni klienci w określonej grupie **oprócz określonych klientów**, zidentyfikowane przez identyfikator połączenia. 

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- Wszyscy połączeni klienci w określonej grupie **oprócz klienta wywołującego**. 

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>Zapisywanie w bazie danych członkostwa w grupie

Poniższe przykłady przedstawiają sposób przechowywania informacji grup i użytkowników w bazie danych. Można użyć dowolnego technologii dostępu do danych; Jednak w poniższym przykładzie pokazano sposób definiowania modeli używający narzędzia Entity Framework. Te modele jednostki odpowiadają tabele bazy danych i pola. Struktury danych może być bardzo zróżnicowana w zależności od wymagań aplikacji. Ten przykład zawiera klasę o nazwie `ConversationRoom` będzie unikatowy dla aplikacji, która umożliwia użytkownikom join konwersacji o różnych tematów, takich jak sportowych lub ogrodnictwa. W tym przykładzie również zawiera klasę dla połączenia. Klasa połączenia nie jest bezwzględnie wymagane do śledzenia członkostwa w grupie, ale często jest częścią niezawodne rozwiązanie w celu śledzenia użytkowników.

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

Następnie w Centrum, można pobrać informacji o grupie i użytkownika z bazy danych i ręcznie Dodaj użytkownika do odpowiednich grup. Przykład nie ma kodu do śledzenia połączeń użytkowników. W tym przykładzie `await` — słowo kluczowe nie została zastosowana przed `Groups.Add` ponieważ wiadomość nie są natychmiast wysyłane do członków grupy. Jeśli chcesz wysłać wiadomość do wszystkich członków grupy bezpośrednio po dodaniu nowego członka, czy chcesz zastosować `await` — słowo kluczowe, aby się upewnić, że operacja asynchroniczna została ukończona.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Przechowywanie członkostwa w grupie w magazynie tabel platformy Azure

Za pomocą magazynu tabel platformy Azure do przechowywania informacji o grup i użytkowników jest podobne do korzystania z bazy danych. W poniższym przykładzie przedstawiono jednostki tabeli, która przechowuje nazwę użytkownika i nazwę grupy.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

W Centrum pobierania przypisanych grup gdy użytkownik nawiąże połączenie.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Sprawdzanie członkostwa w grupie przy ponownym łączeniu

Domyślnie SignalR automatycznie ponownie przypisuje użytkownika do odpowiednich grup przy ponownym łączeniu z tymczasowego przerw w działaniu, np. gdy połączenie jest porzucona i nawiązał ponownie, zanim upłynie limit czasu połączenia. Informacje o grupie użytkowników jest przekazywana do tokenu, gdy połączyć się ponownie, a ten token jest weryfikowany na serwerze. Informacje o proces weryfikacji ponowne przyłączanie użytkowników do grup, zobacz [ponowne przyłączanie grup przy ponownym łączeniu](../security/introduction-to-security.md#rejoingroup).

Ogólnie rzecz biorąc należy używać domyślne zachowanie automatyczne ponowne przyłączanie się, że grupy na ponowne łączenie. SignalR grup nie są przeznaczone jako mechanizm zabezpieczeń w celu ograniczania dostępu do poufnych danych. Jednak w aplikacji należy dokładnie sprawdzić członkostwa w grupie użytkownika przy ponownym łączeniu, można zastąpić domyślne zachowanie. Zmianę zachowania domyślnego można dodać obciążenie do bazy danych, ponieważ można pobrać członkostwa użytkownika w grupach, dla każdego ponowne nawiązanie połączenia, a nie tylko w przypadku, gdy użytkownik nawiąże połączenie.

Należy sprawdzić członkostwa w grupie na ponownie połączyć, utworzyć nowy moduł potoku koncentratora, który zwraca listę przypisanych grup, jak pokazano poniżej.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

Następnie należy dodać ten moduł do potoku koncentratora, jak wyróżniono poniżej.

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
