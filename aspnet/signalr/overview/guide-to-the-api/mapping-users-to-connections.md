---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Mapowanie użytkowników SignalR do połączeń | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym temacie przedstawiono sposób przechowywania informacji o użytkownikach i ich połączenia. Patrick Fletcher pomogła zapisu w tym temacie. Używane w tym temacie wersje oprogramowania...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: c4f95a3b65c57dd7cb7c5c7f1ee09daa17fa9616
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036417"
---
<a name="mapping-signalr-users-to-connections"></a>Mapowanie użytkowników SignalR do połączenia
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym temacie przedstawiono sposób przechowywania informacji o użytkownikach i ich połączenia.
> 
> Patrick Fletcher pomogła zapisu w tym temacie.
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


## <a name="introduction"></a>Wprowadzenie

Każdy klient nawiązywania połączenia z koncentratorem przekazuje identyfikator unikatowy połączenia. Możesz pobrać tę wartość w `Context.ConnectionId` właściwości kontekst koncentratora. Jeśli aplikacja wymaga do mapowania użytkownika identyfikator połączenia i utrwalić mapowania, można użyć jednej z następujących czynności:

- [Identyfikator użytkownika dostawcy (SignalR 2)](#IUserIdProvider)
- [Magazyn w pamięci](#inmemory), takich jak słownik
- [Grupy SignalR dla każdego użytkownika](#groups)
- [Magazynu zewnętrznego, stałe](#database), na przykład tabeli bazy danych lub magazynu tabel platformy Azure

Każda z tych implementacji jest wyświetlany w tym temacie. Możesz użyć `OnConnected`, `OnDisconnected`, i `OnReconnected` metody `Hub` klasy do śledzenia stanu połączenia użytkownika.

Najlepszym rozwiązaniem dla twojej aplikacji jest zależna od:

- Liczba serwerów sieci web hostującego aplikację.
- Czy trzeba uzyskać listę aktualnie połączonych użytkowników.
- Określa, czy należy zachować informacje grup i użytkowników, po ponownym uruchomieniu aplikacji lub serwera.
- Określa, czy opóźnienie wywoływania zewnętrznego serwera jest problem.

Poniższej tabeli rozwiązania działa te zagadnienia.

|  | Więcej niż jednego serwera | Pobierz listę aktualnie połączonych użytkowników | Utrwalanie informacje po uruchomieniu | Optymalnej wydajności |
| --- | --- | --- | --- | --- |
| Identyfikator użytkownika dostawcy | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| W pamięci |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Grupy jednego użytkownika | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Stałe, zewnętrzne | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>Dostawca IUserID

Ta funkcja umożliwia użytkownikom na określenie, co to jest identyfikator userId oparte na IRequest za pomocą nowego interfejsu IUserIdProvider.

**IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Domyślnie będzie implementację, która używa użytkownika `IPrincipal.Identity.Name` jako nazwy użytkownika. Aby zmienić to ustawienie, zarejestruj się w implementacji `IUserIdProvider` z globalnego hosta podczas uruchamiania aplikacji:

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

Z wewnątrz koncentratora, będzie możliwe do wysyłania komunikatów do tych użytkowników za pomocą następujących interfejsu API:

**Wysyłanie wiadomości do określonego użytkownika**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Magazyn w pamięci

Poniższe przykłady przedstawiają sposób przechowywania informacji i połączeń w słowniku, który jest przechowywany w pamięci. Słownik używa `HashSet` do przechowywania identyfikator połączenia. W dowolnym momencie użytkownik może mieć więcej niż jedno połączenie z aplikacji SignalR. Na przykład użytkownik, który jest połączony za pomocą wielu urządzeń lub więcej niż jedną kartę przeglądarki musi więcej niż jeden identyfikator połączenia.

Jeśli aplikacja zostanie wyłączony, wszystkie informacje zostaną utracone, ale go ponownie różnią się jako użytkownicy ponownie ustanowić połączenia. Magazynu w pamięci nie działa, jeśli środowisko obejmuje więcej niż jeden serwer sieci web, ponieważ każdy serwer musi zbiór oddzielnego połączenia.

W pierwszym przykładzie klasa, która zarządza mapowanie użytkowników do połączeń. Klucz dla zestaw HashSet będzie nazwy użytkownika.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Następnym przykładzie pokazano sposób użycia klasy mapowania połączenia z koncentratorem. Wystąpienie klasy są przechowywane w nazwie zmiennej `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Grupy jednego użytkownika

Można utworzyć grupę dla każdego użytkownika, a następnie wyślij wiadomość do tej grupy, jeśli chcesz osiągnąć tylko ten użytkownik. Nazwa każdej grupy jest nazwa użytkownika. Jeśli użytkownik ma więcej niż jedno połączenie, każdy identyfikator połączenia jest dodawane do grupy użytkownika.

Nie należy ręcznie usuwać użytkownika z grupy po jego rozłączeniu. Ta akcja jest wykonywana automatycznie przez platformę SignalR.

Poniższy przykład pokazuje, jak grupy pojedynczego użytkownika.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Stałe magazynu zewnętrznego

W tym temacie przedstawiono sposób korzystania z bazy danych lub magazynu tabel platformy Azure do przechowywania informacji o połączeniu. Ta metoda działa, gdy używanych jest wiele serwerów sieci web, ponieważ każdy serwer sieci web mogą prowadzić interakcję z tym samym repozytorium danych. Jeśli serwerów sieci web Zatrzymaj pracy lub ponownego uruchomienia aplikacji, `OnDisconnected` nie jest wywoływana metoda. W związku z tym jest to możliwe, że repozytorium danych rekordów identyfikatorów połączeń, które nie są już prawidłowe. Aby wyczyścić te rekordy oddzielone, możesz unieważnić dowolnego połączenia, który został utworzony poza przedział czasu, która ma zastosowanie do aplikacji. Przykłady w tej sekcji zawierają wartość śledzenia, gdy połączenie zostało utworzone, ale nie przedstawiają sposób wyczyścić stare rekordy, ponieważ chcesz to zrobić jako proces w tle.

### <a name="database"></a>Baza danych

Następujące przykłady przedstawiają sposób przechowywania informacji i połączeń w bazie danych. Można użyć dowolnego technologii dostępu do danych; Jednak w poniższym przykładzie pokazano sposób definiowania modeli używający narzędzia Entity Framework. Te modele jednostki odpowiadają tabele bazy danych i pola. Struktury danych może być bardzo zróżnicowana w zależności od wymagań aplikacji.

Pierwszym przykładzie pokazano sposób definiowania jednostki użytkownika, który może być skojarzony z wielu obiektów połączenia.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

Następnie w Centrum, można śledzić stan każdego połączenia kodem przedstawionym poniżej.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Magazyn tabel Azure

W poniższym przykładzie magazynu tabel Azure jest podobny do bazy danych. Nie obejmują wszystkie informacje, że konieczne będzie wprowadzenie do usługi Magazyn tabel Azure. Aby uzyskać informacje, zobacz [jak używać magazynu tabel w .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

W poniższym przykładzie przedstawiono jednostkę tabeli do przechowywania informacji o połączeniu. Dzieli dane według nazwy użytkownika i informacjami każdej jednostki według identyfikatora połączenia, dzięki czemu użytkownik może mieć wiele połączeń w dowolnym momencie.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

Koncentrator służy do śledzenia stanu połączenia każdego użytkownika.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
