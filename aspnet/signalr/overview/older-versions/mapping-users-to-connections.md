---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: "Mapowanie użytkowników SignalR do połączenia w SignalR 1.x | Dokumentacja firmy Microsoft"
author: pfletcher
description: "W tym temacie przedstawiono sposób przechowywania informacji o użytkownikach i ich połączenia."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 896bf4142ce090e39ed5697ff053cd56728318ed
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a><span data-ttu-id="ad923-103">Mapowanie użytkowników SignalR do połączenia w SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="ad923-103">Mapping SignalR Users to Connections in SignalR 1.x</span></span>
====================
<span data-ttu-id="ad923-104">przez [Patrick Fletcher](https://github.com/pfletcher), [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ad923-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ad923-105">W tym temacie przedstawiono sposób przechowywania informacji o użytkownikach i ich połączenia.</span><span class="sxs-lookup"><span data-stu-id="ad923-105">This topic shows how to retain information about users and their connections.</span></span>


## <a name="introduction"></a><span data-ttu-id="ad923-106">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="ad923-106">Introduction</span></span>

<span data-ttu-id="ad923-107">Każdy klient nawiązywania połączenia z koncentratorem przekazuje identyfikator unikatowy połączenia. Możesz pobrać tę wartość w `Context.ConnectionId` właściwości kontekst koncentratora.</span><span class="sxs-lookup"><span data-stu-id="ad923-107">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="ad923-108">Jeśli aplikacja wymaga do mapowania użytkownika identyfikator połączenia i utrwalić mapowania, można użyć jednej z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="ad923-108">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- <span data-ttu-id="ad923-109">[Magazyn w pamięci](#inmemory), takich jak słownik</span><span class="sxs-lookup"><span data-stu-id="ad923-109">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="ad923-110">Grupy SignalR dla każdego użytkownika</span><span class="sxs-lookup"><span data-stu-id="ad923-110">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="ad923-111">[Magazynu zewnętrznego, stałe](#database), na przykład tabeli bazy danych lub magazynu tabel platformy Azure</span><span class="sxs-lookup"><span data-stu-id="ad923-111">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="ad923-112">Każda z tych implementacji jest wyświetlany w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="ad923-112">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="ad923-113">Możesz użyć `OnConnected`, `OnDisconnected`, i `OnReconnected` metody `Hub` klasy do śledzenia stanu połączenia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ad923-113">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="ad923-114">Najlepszym rozwiązaniem dla twojej aplikacji jest zależna od:</span><span class="sxs-lookup"><span data-stu-id="ad923-114">The best approach for your application depends on:</span></span>

- <span data-ttu-id="ad923-115">Liczba serwerów sieci web hostującego aplikację.</span><span class="sxs-lookup"><span data-stu-id="ad923-115">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="ad923-116">Czy trzeba uzyskać listę aktualnie połączonych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="ad923-116">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="ad923-117">Określa, czy należy zachować informacje grup i użytkowników, po ponownym uruchomieniu aplikacji lub serwera.</span><span class="sxs-lookup"><span data-stu-id="ad923-117">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="ad923-118">Określa, czy opóźnienie wywoływania zewnętrznego serwera jest problem.</span><span class="sxs-lookup"><span data-stu-id="ad923-118">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="ad923-119">Poniższej tabeli rozwiązania działa te zagadnienia.</span><span class="sxs-lookup"><span data-stu-id="ad923-119">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="ad923-120">Więcej niż jednego serwera</span><span class="sxs-lookup"><span data-stu-id="ad923-120">More than one server</span></span> | <span data-ttu-id="ad923-121">Pobierz listę aktualnie połączonych użytkowników</span><span class="sxs-lookup"><span data-stu-id="ad923-121">Get list of currently connected users</span></span> | <span data-ttu-id="ad923-122">Utrwalanie informacje po uruchomieniu</span><span class="sxs-lookup"><span data-stu-id="ad923-122">Persist information after restarts</span></span> | <span data-ttu-id="ad923-123">Optymalnej wydajności</span><span class="sxs-lookup"><span data-stu-id="ad923-123">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="ad923-124">W pamięci</span><span class="sxs-lookup"><span data-stu-id="ad923-124">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="ad923-125">Grupy jednego użytkownika</span><span class="sxs-lookup"><span data-stu-id="ad923-125">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="ad923-126">Stałe, zewnętrzne</span><span class="sxs-lookup"><span data-stu-id="ad923-126">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="ad923-127">Magazyn w pamięci</span><span class="sxs-lookup"><span data-stu-id="ad923-127">In-memory storage</span></span>

<span data-ttu-id="ad923-128">Poniższe przykłady przedstawiają sposób przechowywania informacji i połączeń w słowniku, który jest przechowywany w pamięci.</span><span class="sxs-lookup"><span data-stu-id="ad923-128">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="ad923-129">Słownik używa `HashSet` do przechowywania identyfikator połączenia. W dowolnym momencie użytkownik może mieć więcej niż jedno połączenie z aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="ad923-129">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="ad923-130">Na przykład użytkownik, który jest połączony za pomocą wielu urządzeń lub więcej niż jedną kartę przeglądarki musi więcej niż jeden identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="ad923-130">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="ad923-131">Jeśli aplikacja zostanie wyłączony, wszystkie informacje zostaną utracone, ale go ponownie różnią się jako użytkownicy ponownie ustanowić połączenia.</span><span class="sxs-lookup"><span data-stu-id="ad923-131">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="ad923-132">Magazynu w pamięci nie działa, jeśli środowisko obejmuje więcej niż jeden serwer sieci web, ponieważ każdy serwer musi zbiór oddzielnego połączenia.</span><span class="sxs-lookup"><span data-stu-id="ad923-132">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="ad923-133">W pierwszym przykładzie klasa, która zarządza mapowanie użytkowników do połączeń.</span><span class="sxs-lookup"><span data-stu-id="ad923-133">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="ad923-134">Klucz dla zestaw HashSet będzie nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ad923-134">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="ad923-135">Następnym przykładzie pokazano sposób użycia klasy mapowania połączenia z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="ad923-135">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="ad923-136">Wystąpienie klasy są przechowywane w nazwie zmiennej `_connections`.</span><span class="sxs-lookup"><span data-stu-id="ad923-136">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="ad923-137">Grupy jednego użytkownika</span><span class="sxs-lookup"><span data-stu-id="ad923-137">Single-user groups</span></span>

<span data-ttu-id="ad923-138">Można utworzyć grupę dla każdego użytkownika, a następnie wyślij wiadomość do tej grupy, jeśli chcesz osiągnąć tylko ten użytkownik.</span><span class="sxs-lookup"><span data-stu-id="ad923-138">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="ad923-139">Nazwa każdej grupy jest nazwa użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ad923-139">The name of each group is the name of the user.</span></span> <span data-ttu-id="ad923-140">Jeśli użytkownik ma więcej niż jedno połączenie, każdy identyfikator połączenia jest dodawane do grupy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ad923-140">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="ad923-141">Nie należy ręcznie usuwać użytkownika z grupy po jego rozłączeniu.</span><span class="sxs-lookup"><span data-stu-id="ad923-141">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="ad923-142">Ta akcja jest wykonywana automatycznie przez platformę SignalR.</span><span class="sxs-lookup"><span data-stu-id="ad923-142">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="ad923-143">Poniższy przykład pokazuje, jak grupy pojedynczego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ad923-143">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="ad923-144">Stałe magazynu zewnętrznego</span><span class="sxs-lookup"><span data-stu-id="ad923-144">Permanent, external storage</span></span>

<span data-ttu-id="ad923-145">W tym temacie przedstawiono sposób korzystania z bazy danych lub magazynu tabel platformy Azure do przechowywania informacji o połączeniu.</span><span class="sxs-lookup"><span data-stu-id="ad923-145">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="ad923-146">Ta metoda działa, gdy używanych jest wiele serwerów sieci web, ponieważ każdy serwer sieci web mogą prowadzić interakcję z tym samym repozytorium danych.</span><span class="sxs-lookup"><span data-stu-id="ad923-146">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="ad923-147">Jeśli serwerów sieci web Zatrzymaj pracy lub ponownego uruchomienia aplikacji, `OnDisconnected` nie jest wywoływana metoda.</span><span class="sxs-lookup"><span data-stu-id="ad923-147">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="ad923-148">W związku z tym jest to możliwe, że repozytorium danych rekordów identyfikatorów połączeń, które nie są już prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="ad923-148">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="ad923-149">Aby wyczyścić te rekordy oddzielone, możesz unieważnić dowolnego połączenia, który został utworzony poza przedział czasu, która ma zastosowanie do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ad923-149">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="ad923-150">Przykłady w tej sekcji zawierają wartość śledzenia, gdy połączenie zostało utworzone, ale nie przedstawiają sposób wyczyścić stare rekordy, ponieważ chcesz to zrobić jako proces w tle.</span><span class="sxs-lookup"><span data-stu-id="ad923-150">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="ad923-151">Baza danych</span><span class="sxs-lookup"><span data-stu-id="ad923-151">Database</span></span>

<span data-ttu-id="ad923-152">Następujące przykłady przedstawiają sposób przechowywania informacji i połączeń w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="ad923-152">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="ad923-153">Można użyć dowolnego technologii dostępu do danych; Jednak w poniższym przykładzie pokazano sposób definiowania modeli używający narzędzia Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ad923-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="ad923-154">Te modele jednostki odpowiadają tabele bazy danych i pola.</span><span class="sxs-lookup"><span data-stu-id="ad923-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="ad923-155">Struktury danych może być bardzo zróżnicowana w zależności od wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ad923-155">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="ad923-156">Pierwszym przykładzie pokazano sposób definiowania jednostki użytkownika, który może być skojarzony z wielu obiektów połączenia.</span><span class="sxs-lookup"><span data-stu-id="ad923-156">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="ad923-157">Następnie w Centrum, można śledzić stan każdego połączenia kodem przedstawionym poniżej.</span><span class="sxs-lookup"><span data-stu-id="ad923-157">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a><span data-ttu-id="ad923-158">Magazyn tabel Azure</span><span class="sxs-lookup"><span data-stu-id="ad923-158">Azure table storage</span></span>

<span data-ttu-id="ad923-159">W poniższym przykładzie magazynu tabel Azure jest podobny do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="ad923-159">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="ad923-160">Nie obejmują wszystkie informacje, że konieczne będzie wprowadzenie do usługi Magazyn tabel Azure.</span><span class="sxs-lookup"><span data-stu-id="ad923-160">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="ad923-161">Aby uzyskać informacje, zobacz [jak używać magazynu tabel w .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="ad923-161">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="ad923-162">W poniższym przykładzie przedstawiono jednostkę tabeli do przechowywania informacji o połączeniu.</span><span class="sxs-lookup"><span data-stu-id="ad923-162">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="ad923-163">Dzieli dane według nazwy użytkownika i informacjami każdej jednostki według identyfikatora połączenia, dzięki czemu użytkownik może mieć wiele połączeń w dowolnym momencie.</span><span class="sxs-lookup"><span data-stu-id="ad923-163">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<span data-ttu-id="ad923-164">Koncentrator służy do śledzenia stanu połączenia każdego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ad923-164">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
