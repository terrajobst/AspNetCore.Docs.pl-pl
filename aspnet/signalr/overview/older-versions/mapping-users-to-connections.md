---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: Mapowanie użytkowników SignalR na połączenia w SignalR 1.x | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym temacie przedstawiono sposób przechowywania informacji o użytkownikach i ich połączeń.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 3ce651fa523743da536a9b73bb9bb8e21d8845c6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754459"
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a>Mapowanie użytkowników SignalR na połączenia w SignalR 1.x
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

> W tym temacie przedstawiono sposób przechowywania informacji o użytkownikach i ich połączeń.


## <a name="introduction"></a>Wprowadzenie

Każdy klient nawiązywania połączenia z koncentratorem przekazuje identyfikatora unikatowego połączenia. Możesz pobrać tę wartość w `Context.ConnectionId` właściwość kontekst koncentratora. Jeśli Twoja aplikacja potrzebuje do mapowania użytkownika identyfikator połączenia i utrwalić mapowania, można użyć jednej z następujących czynności:

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
| W pamięci |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| Grupy pojedynczego użytkownika | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| Stałe, zewnętrzne | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Pojemność magazynu w pamięci

Poniższe przykłady pokazują, jak zachować połączenie i informacji o użytkownikach w słowniku, który jest przechowywany w pamięci. Słownik używa `HashSet` do przechowywania identyfikatora połączenia. W dowolnym momencie użytkownik może mieć więcej niż jednego połączenia do aplikacji SignalR. Na przykład użytkownik, który jest połączony za pośrednictwem wielu urządzeń lub więcej niż jedną kartę przeglądarki będzie mieć więcej niż jeden identyfikator połączenia.

Jeśli aplikacja zostanie wyłączony, wszystkie informacje zostaną utracone, ale jej ponownie różnią się jako użytkownicy ponownie ustanowić połączenia. Pojemność magazynu w pamięci nie działa, jeśli środowisko obejmuje więcej niż jeden serwer sieci web, ponieważ każdy serwer będzie mieć kolekcję osobnych połączeń.

W pierwszym przykładzie pokazano klasę, która zarządza mapowanie użytkowników do połączenia. Klucz dla hashset — będzie nazwa użytkownika.

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Następny przykład pokazuje sposób użycia klasy mapowania połączenia z koncentratorem. Wystąpienie klasy są przechowywane w nazwie zmiennej `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Grupy pojedynczego użytkownika

Można utworzyć grupę dla każdego użytkownika, a następnie wyślij wiadomość do tej grupy, gdy chcesz się połączyć tylko ten użytkownik. Nazwa każdej grupy jest nazwa użytkownika. Jeśli użytkownik ma więcej niż jedno połączenie, każdy identyfikator połączenia zostanie dodany do grupy użytkowników.

Nie należy ręcznie usuwać użytkowników z grupy, gdy użytkownik zamknie połączenie. Ta akcja jest wykonywana automatycznie przez strukturę SignalR.

Poniższy przykład pokazuje sposób implementacji grup pojedynczego użytkownika.

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Magazynu zewnętrznego, stałe

W tym temacie przedstawiono sposób korzystania z bazy danych lub usługi Azure table storage do przechowywania informacji o połączeniu. Ta metoda działa, gdy masz wiele serwerów sieci web, ponieważ każdy serwer sieci web mogą wchodzić w interakcje z tym samym repozytorium danych. Jeśli serwery sieci web zaprzestanie działania lub aplikacja uruchamia się ponownie, `OnDisconnected` nie jest wywoływana metoda. Dlatego jest to możliwe, że repozytorium danych rekordów dla identyfikatorów połączeń, które nie są już prawidłowe. Aby wyczyścić te rekordy oddzielone, możesz unieważnić dowolnego połączenia, który został utworzony poza przedział czasu, która jest odpowiednia dla aplikacji. Przykłady w tej sekcji zawierają wartość do śledzenia, gdy połączenie zostało utworzone, ale nie przedstawiają sposób wyczyścić stare rekordy, ponieważ może to zrobić jako proces w tle.

### <a name="database"></a>Baza danych

Poniższe przykłady pokazują, jak zachować połączenie i informacji o użytkownikach w bazie danych. Możesz użyć dowolnej technologii dostępu do danych; Jednak w poniższym przykładzie pokazano sposób definiowania modeli za pomocą platformy Entity Framework. Modele te jednostki odpowiadają tabel bazy danych i pola. Do struktury danych może być bardzo zróżnicowana w zależności od wymagań aplikacji.

Pierwszy przykład pokazuje jak zdefiniować jednostki użytkownika, które mogą być skojarzone z wielu jednostek połączenia.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Następnie z poziomu Centrum można śledzić stan każdego połączenia przy użyciu kodu pokazany poniżej.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a>Usługa Azure table storage

W poniższym przykładzie magazynu tabel platformy Azure jest podobne do bazy danych. Nie obejmują wszystkich informacji, że konieczne będzie wprowadzenie do usługi Azure Table Storage. Aby uzyskać informacje, zobacz [jak używać magazynu tabel w języku .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

Poniższy przykład pokazuje jednostkę tabeli do przechowywania informacji o połączeniu. Dzieli dane według nazwy użytkownika i identyfikuje każdej jednostki według identyfikatora połączenia, dzięki czemu użytkownik może mieć wiele połączeń w dowolnym momencie.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

W Centrum możesz śledzić stan połączenia każdego użytkownika.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
