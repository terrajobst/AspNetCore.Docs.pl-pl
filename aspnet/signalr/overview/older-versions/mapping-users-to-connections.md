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
ms.openlocfilehash: ba7bbea25cd35f8ba23901c5f8db08404c91bd52
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287491"
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="80cc6-103">Mapowanie użytkowników usługi SignalR na połączenia w usłudze SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="80cc6-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>
====================
<span data-ttu-id="80cc6-104">przez [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="80cc6-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="80cc6-105">W tym temacie przedstawiono sposób przechowywania informacji o użytkownikach i ich połączeń.</span><span class="sxs-lookup"><span data-stu-id="80cc6-105">This topic shows how to retain information about users and their connections.</span></span>


## <a name="introduction"></a><span data-ttu-id="80cc6-106">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="80cc6-106">Introduction</span></span>

<span data-ttu-id="80cc6-107">Każdy klient nawiązywania połączenia z koncentratorem przekazuje identyfikatora unikatowego połączenia. Możesz pobrać tę wartość w `Context.ConnectionId` właściwość kontekst koncentratora.</span><span class="sxs-lookup"><span data-stu-id="80cc6-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="80cc6-108">Jeśli Twoja aplikacja potrzebuje do mapowania użytkownika identyfikator połączenia i utrwalić mapowania, można użyć jednej z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="80cc6-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="80cc6-109">[Pojemność magazynu w pamięci](#inmemory), takich jak słownik</span><span class="sxs-lookup"><span data-stu-id="80cc6-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="80cc6-110">Grupa SignalR dla każdego użytkownika</span><span class="sxs-lookup"><span data-stu-id="80cc6-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="80cc6-111">[Magazynu zewnętrznego, stałe](#database), takich jak tabela bazy danych lub usługi Azure table storage</span><span class="sxs-lookup"><span data-stu-id="80cc6-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="80cc6-112">Każda z tych implementacji jest wyświetlany w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="80cc6-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="80cc6-113">Możesz użyć `OnConnected`, `OnDisconnected`, i `OnReconnected` metody `Hub` klasy w celu śledzenia stanu połączenia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="80cc6-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="80cc6-114">Najlepszym rozwiązaniem dla twojej aplikacji zależy od:</span><span class="sxs-lookup"><span data-stu-id="80cc6-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="80cc6-115">Liczba serwerów sieci web hostuje aplikację.</span><span class="sxs-lookup"><span data-stu-id="80cc6-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="80cc6-116">Czy należy uzyskać listę aktualnie połączonych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="80cc6-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="80cc6-117">Czy musisz utrwalić informacje o grupie i użytkownika, po ponownym uruchomieniu aplikacji lub serwera.</span><span class="sxs-lookup"><span data-stu-id="80cc6-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="80cc6-118">Opóźnienie podczas wywoływania zewnętrznego serwera, czy jest problem.</span><span class="sxs-lookup"><span data-stu-id="80cc6-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="80cc6-119">Pokazano w poniższej tabeli podejście, które działa w przypadku tych zagadnień.</span><span class="sxs-lookup"><span data-stu-id="80cc6-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="80cc6-120">Więcej niż jeden serwer</span><span class="sxs-lookup"><span data-stu-id="80cc6-120">More than one server</span></span> | <span data-ttu-id="80cc6-121">Pobierz listę aktualnie połączonych użytkowników</span><span class="sxs-lookup"><span data-stu-id="80cc6-121">Get list of currently connected users</span></span> | <span data-ttu-id="80cc6-122">Utrwalanie informacji po uruchomieniu</span><span class="sxs-lookup"><span data-stu-id="80cc6-122">Persist information after restarts</span></span> | <span data-ttu-id="80cc6-123">Uzyskania optymalnej wydajności</span><span class="sxs-lookup"><span data-stu-id="80cc6-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="80cc6-124">W pamięci</span><span class="sxs-lookup"><span data-stu-id="80cc6-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="80cc6-125">Grupy pojedynczego użytkownika</span><span class="sxs-lookup"><span data-stu-id="80cc6-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="80cc6-126">Stałe, zewnętrzne</span><span class="sxs-lookup"><span data-stu-id="80cc6-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="80cc6-127">Pojemność magazynu w pamięci</span><span class="sxs-lookup"><span data-stu-id="80cc6-127">In-memory storage</span></span>

<span data-ttu-id="80cc6-128">Poniższe przykłady pokazują, jak zachować połączenie i informacji o użytkownikach w słowniku, który jest przechowywany w pamięci.</span><span class="sxs-lookup"><span data-stu-id="80cc6-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="80cc6-129">Słownik używa `HashSet` do przechowywania identyfikatora połączenia. W dowolnym momencie użytkownik może mieć więcej niż jednego połączenia do aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="80cc6-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="80cc6-130">Na przykład użytkownik, który jest połączony za pośrednictwem wielu urządzeń lub więcej niż jedną kartę przeglądarki będzie mieć więcej niż jeden identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="80cc6-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="80cc6-131">Jeśli aplikacja zostanie wyłączony, wszystkie informacje zostaną utracone, ale jej ponownie różnią się jako użytkownicy ponownie ustanowić połączenia.</span><span class="sxs-lookup"><span data-stu-id="80cc6-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="80cc6-132">Pojemność magazynu w pamięci nie działa, jeśli środowisko obejmuje więcej niż jeden serwer sieci web, ponieważ każdy serwer będzie mieć kolekcję osobnych połączeń.</span><span class="sxs-lookup"><span data-stu-id="80cc6-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="80cc6-133">W pierwszym przykładzie pokazano klasę, która zarządza mapowanie użytkowników do połączenia.</span><span class="sxs-lookup"><span data-stu-id="80cc6-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="80cc6-134">Klucz dla hashset — będzie nazwa użytkownika.</span><span class="sxs-lookup"><span data-stu-id="80cc6-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="80cc6-135">Następny przykład pokazuje sposób użycia klasy mapowania połączenia z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="80cc6-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="80cc6-136">Wystąpienie klasy są przechowywane w nazwie zmiennej `_connections`.</span><span class="sxs-lookup"><span data-stu-id="80cc6-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="80cc6-137">Grupy pojedynczego użytkownika</span><span class="sxs-lookup"><span data-stu-id="80cc6-137">Single-user groups</span></span>

<span data-ttu-id="80cc6-138">Można utworzyć grupę dla każdego użytkownika, a następnie wyślij wiadomość do tej grupy, gdy chcesz się połączyć tylko ten użytkownik.</span><span class="sxs-lookup"><span data-stu-id="80cc6-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="80cc6-139">Nazwa każdej grupy jest nazwa użytkownika.</span><span class="sxs-lookup"><span data-stu-id="80cc6-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="80cc6-140">Jeśli użytkownik ma więcej niż jedno połączenie, każdy identyfikator połączenia zostanie dodany do grupy użytkowników.</span><span class="sxs-lookup"><span data-stu-id="80cc6-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="80cc6-141">Nie należy ręcznie usuwać użytkowników z grupy, gdy użytkownik zamknie połączenie.</span><span class="sxs-lookup"><span data-stu-id="80cc6-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="80cc6-142">Ta akcja jest wykonywana automatycznie przez strukturę SignalR.</span><span class="sxs-lookup"><span data-stu-id="80cc6-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="80cc6-143">Poniższy przykład pokazuje sposób implementacji grup pojedynczego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="80cc6-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="80cc6-144">Magazynu zewnętrznego, stałe</span><span class="sxs-lookup"><span data-stu-id="80cc6-144">Permanent, external storage</span></span>

<span data-ttu-id="80cc6-145">W tym temacie przedstawiono sposób korzystania z bazy danych lub usługi Azure table storage do przechowywania informacji o połączeniu.</span><span class="sxs-lookup"><span data-stu-id="80cc6-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="80cc6-146">Ta metoda działa, gdy masz wiele serwerów sieci web, ponieważ każdy serwer sieci web mogą wchodzić w interakcje z tym samym repozytorium danych.</span><span class="sxs-lookup"><span data-stu-id="80cc6-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="80cc6-147">Jeśli serwery sieci web zaprzestanie działania lub aplikacja uruchamia się ponownie, `OnDisconnected` nie jest wywoływana metoda.</span><span class="sxs-lookup"><span data-stu-id="80cc6-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="80cc6-148">Dlatego jest to możliwe, że repozytorium danych rekordów dla identyfikatorów połączeń, które nie są już prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="80cc6-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="80cc6-149">Aby wyczyścić te rekordy oddzielone, możesz unieważnić dowolnego połączenia, który został utworzony poza przedział czasu, która jest odpowiednia dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="80cc6-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="80cc6-150">Przykłady w tej sekcji zawierają wartość do śledzenia, gdy połączenie zostało utworzone, ale nie przedstawiają sposób wyczyścić stare rekordy, ponieważ może to zrobić jako proces w tle.</span><span class="sxs-lookup"><span data-stu-id="80cc6-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="80cc6-151">Baza danych</span><span class="sxs-lookup"><span data-stu-id="80cc6-151">Database</span></span>

<span data-ttu-id="80cc6-152">Poniższe przykłady pokazują, jak zachować połączenie i informacji o użytkownikach w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="80cc6-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="80cc6-153">Możesz użyć dowolnej technologii dostępu do danych; Jednak w poniższym przykładzie pokazano sposób definiowania modeli za pomocą platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="80cc6-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="80cc6-154">Modele te jednostki odpowiadają tabel bazy danych i pola.</span><span class="sxs-lookup"><span data-stu-id="80cc6-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="80cc6-155">Do struktury danych może być bardzo zróżnicowana w zależności od wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="80cc6-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="80cc6-156">Pierwszy przykład pokazuje jak zdefiniować jednostki użytkownika, które mogą być skojarzone z wielu jednostek połączenia.</span><span class="sxs-lookup"><span data-stu-id="80cc6-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="80cc6-157">Następnie z poziomu Centrum można śledzić stan każdego połączenia przy użyciu kodu pokazany poniżej.</span><span class="sxs-lookup"><span data-stu-id="80cc6-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="80cc6-158">Usługa Azure table storage</span><span class="sxs-lookup"><span data-stu-id="80cc6-158">Azure table storage</span></span>

<span data-ttu-id="80cc6-159">W poniższym przykładzie magazynu tabel platformy Azure jest podobne do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="80cc6-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="80cc6-160">Nie obejmują wszystkich informacji, że konieczne będzie wprowadzenie do usługi Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="80cc6-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="80cc6-161">Aby uzyskać informacje, zobacz [jak używać magazynu tabel w języku .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="80cc6-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="80cc6-162">Poniższy przykład pokazuje jednostkę tabeli do przechowywania informacji o połączeniu.</span><span class="sxs-lookup"><span data-stu-id="80cc6-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="80cc6-163">Dzieli dane według nazwy użytkownika i identyfikuje każdej jednostki według identyfikatora połączenia, dzięki czemu użytkownik może mieć wiele połączeń w dowolnym momencie.</span><span class="sxs-lookup"><span data-stu-id="80cc6-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="80cc6-164">W Centrum możesz śledzić stan połączenia każdego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="80cc6-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
