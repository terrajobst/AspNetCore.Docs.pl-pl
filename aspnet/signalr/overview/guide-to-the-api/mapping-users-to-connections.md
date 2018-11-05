---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Mapowanie użytkowników SignalR na połączenia | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym temacie przedstawiono sposób przechowywania informacji o użytkownikach i ich połączeń. Patrick Fletcher pomogła zapisu w tym temacie. Wersje oprogramowania, używaną w tym temacie...
ms.author: riande
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 5c7f710ec672d4f81ac0d1606640054e43fc5af8
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021238"
---
<a name="mapping-signalr-users-to-connections"></a>Mapowanie użytkowników usługi SignalR na połączenia
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym temacie przedstawiono sposób przechowywania informacji o użytkownikach i ich połączeń.
>
> Patrick Fletcher pomogła zapisu w tym temacie.
>
> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używaną w tym temacie
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR w wersji 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Poprzednie wersje tego tematu
>
> Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Pytania i komentarze
>
> Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię. Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


## <a name="introduction"></a>Wprowadzenie

Każdy klient nawiązywania połączenia z koncentratorem przekazuje identyfikatora unikatowego połączenia. Możesz pobrać tę wartość w `Context.ConnectionId` właściwość kontekst koncentratora. Jeśli Twoja aplikacja potrzebuje do mapowania użytkownika identyfikator połączenia i utrwalić mapowania, można użyć jednej z następujących czynności:

- [Identyfikator użytkownika dostawcy (SignalR 2)](#IUserIdProvider)
- [Pojemność magazynu w pamięci](#inmemory), takich jak słownik
- [Grupa SignalR dla każdego użytkownika](#groups)
- [Magazynu zewnętrznego, stałe](#database), takich jak tabela bazy danych lub usługi Azure table storage

Każda z tych implementacji jest wyświetlany w tym temacie. Możesz użyć `OnConnected`, `OnDisconnected`, i `OnReconnected` metody `Hub` klasy w celu śledzenia stanu połączenia użytkownika.

Najlepszym rozwiązaniem dla twojej aplikacji zależy od:

- Liczba serwerów sieci web hostuje aplikację.
- Czy należy uzyskać listę aktualnie połączonych użytkowników.
- Czy musisz utrwalić informacje o grupie i użytkownika, po ponownym uruchomieniu aplikacji lub serwera.
- Opóźnienie podczas wywoływania zewnętrznego serwera, czy jest problem.

Pokazano w poniższej tabeli podejście, które działa w przypadku tych zagadnień.

|  | Więcej niż jeden serwer | Pobierz listę aktualnie połączonych użytkowników | Utrwalanie informacji po uruchomieniu | Uzyskania optymalnej wydajności |
| --- | --- | --- | --- | --- |
| Identyfikator użytkownika dostawcy | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| W pamięci |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Grupy pojedynczego użytkownika | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Stałe, zewnętrzne | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>Dostawca IUserID

Ta funkcja pozwala użytkownikom na określenie, co to jest identyfikator userId oparte na IRequest za pomocą nowego interfejsu IUserIdProvider.

**IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Domyślnie będzie implementację, który używa użytkownika `IPrincipal.Identity.Name` jako nazwy użytkownika. Aby zmienić to ustawienie, zarejestruj implementacji `IUserIdProvider` z hostem globalnego, podczas uruchamiania aplikacji:

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

Z w ramach Centrum, można wysyłać komunikaty do tych użytkowników za pośrednictwem następujący interfejs API:

**Wysyłanie komunikatu do określonego użytkownika**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Pojemność magazynu w pamięci

Poniższe przykłady pokazują, jak zachować połączenie i informacji o użytkownikach w słowniku, który jest przechowywany w pamięci. Słownik używa `HashSet` do przechowywania identyfikatora połączenia. W dowolnym momencie użytkownik może mieć więcej niż jednego połączenia do aplikacji SignalR. Na przykład użytkownik, który jest połączony za pośrednictwem wielu urządzeń lub więcej niż jedną kartę przeglądarki będzie mieć więcej niż jeden identyfikator połączenia.

Jeśli aplikacja zostanie wyłączony, wszystkie informacje zostaną utracone, ale jej ponownie różnią się jako użytkownicy ponownie ustanowić połączenia. Pojemność magazynu w pamięci nie działa, jeśli środowisko obejmuje więcej niż jeden serwer sieci web, ponieważ każdy serwer będzie mieć kolekcję osobnych połączeń.

W pierwszym przykładzie pokazano klasę, która zarządza mapowanie użytkowników do połączenia. Klucz dla hashset — będzie nazwa użytkownika.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Następny przykład pokazuje sposób użycia klasy mapowania połączenia z koncentratorem. Wystąpienie klasy są przechowywane w nazwie zmiennej `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Grupy pojedynczego użytkownika

Można utworzyć grupę dla każdego użytkownika, a następnie wyślij wiadomość do tej grupy, gdy chcesz się połączyć tylko ten użytkownik. Nazwa każdej grupy jest nazwa użytkownika. Jeśli użytkownik ma więcej niż jedno połączenie, każdy identyfikator połączenia zostanie dodany do grupy użytkowników.

Nie należy ręcznie usuwać użytkowników z grupy, gdy użytkownik zamknie połączenie. Ta akcja jest wykonywana automatycznie przez strukturę SignalR.

Poniższy przykład pokazuje sposób implementacji grup pojedynczego użytkownika.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Magazynu zewnętrznego, stałe

W tym temacie przedstawiono sposób korzystania z bazy danych lub usługi Azure table storage do przechowywania informacji o połączeniu. Ta metoda działa, gdy masz wiele serwerów sieci web, ponieważ każdy serwer sieci web mogą wchodzić w interakcje z tym samym repozytorium danych. Jeśli serwery sieci web zaprzestanie działania lub aplikacja uruchamia się ponownie, `OnDisconnected` nie jest wywoływana metoda. Dlatego jest to możliwe, że repozytorium danych rekordów dla identyfikatorów połączeń, które nie są już prawidłowe. Aby wyczyścić te rekordy oddzielone, możesz unieważnić dowolnego połączenia, który został utworzony poza przedział czasu, która jest odpowiednia dla aplikacji. Przykłady w tej sekcji zawierają wartość do śledzenia, gdy połączenie zostało utworzone, ale nie przedstawiają sposób wyczyścić stare rekordy, ponieważ może to zrobić jako proces w tle.

### <a name="database"></a>Baza danych

Poniższe przykłady pokazują, jak zachować połączenie i informacji o użytkownikach w bazie danych. Możesz użyć dowolnej technologii dostępu do danych; Jednak w poniższym przykładzie pokazano sposób definiowania modeli za pomocą platformy Entity Framework. Modele te jednostki odpowiadają tabel bazy danych i pola. Do struktury danych może być bardzo zróżnicowana w zależności od wymagań aplikacji.

Pierwszy przykład pokazuje jak zdefiniować jednostki użytkownika, które mogą być skojarzone z wielu jednostek połączenia.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

Następnie z poziomu Centrum można śledzić stan każdego połączenia przy użyciu kodu pokazany poniżej.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Usługa Azure table storage

W poniższym przykładzie magazynu tabel platformy Azure jest podobne do bazy danych. Nie obejmują wszystkich informacji, że konieczne będzie wprowadzenie do usługi Azure Table Storage. Aby uzyskać informacje, zobacz [jak używać magazynu tabel w języku .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

Poniższy przykład pokazuje jednostkę tabeli do przechowywania informacji o połączeniu. Dzieli dane według nazwy użytkownika i identyfikuje każdej jednostki według identyfikatora połączenia, dzięki czemu użytkownik może mieć wiele połączeń w dowolnym momencie.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

W Centrum możesz śledzić stan połączenia każdego użytkownika.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
