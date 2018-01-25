---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: "Mapowanie użytkowników SignalR do połączeń | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W tym temacie przedstawiono sposób przechowywania informacji o użytkownikach i ich połączenia. Patrick Fletcher pomogła zapisu w tym temacie. Używane w tym temacie wersje oprogramowania..."
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
---
<a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="11a27-105">Mapowanie użytkowników SignalR do połączenia</span><span class="sxs-lookup"><span data-stu-id="11a27-105">Mapping SignalR Users to Connections</span></span>
====================
<span data-ttu-id="11a27-106">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="11a27-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="11a27-107">W tym temacie przedstawiono sposób przechowywania informacji o użytkownikach i ich połączenia.</span><span class="sxs-lookup"><span data-stu-id="11a27-107">This topic shows how to retain information about users and their connections.</span></span>
> 
> <span data-ttu-id="11a27-108">Patrick Fletcher pomogła zapisu w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="11a27-108">Patrick Fletcher helped write this topic.</span></span>
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="11a27-109">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="11a27-109">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="11a27-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="11a27-110">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="11a27-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="11a27-111">.NET 4.5</span></span>
> - <span data-ttu-id="11a27-112">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="11a27-112">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="11a27-113">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="11a27-113">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="11a27-114">Aby uzyskać informacje dotyczące starszych wersji biblioteki signalr, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="11a27-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="11a27-115">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="11a27-115">Questions and comments</span></span>
> 
> <span data-ttu-id="11a27-116">Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony.</span><span class="sxs-lookup"><span data-stu-id="11a27-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="11a27-117">Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="11a27-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="introduction"></a><span data-ttu-id="11a27-118">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="11a27-118">Introduction</span></span>

<span data-ttu-id="11a27-119">Każdy klient nawiązywania połączenia z koncentratorem przekazuje identyfikator unikatowy połączenia. Możesz pobrać tę wartość w `Context.ConnectionId` właściwości kontekst koncentratora.</span><span class="sxs-lookup"><span data-stu-id="11a27-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="11a27-120">Jeśli aplikacja wymaga do mapowania użytkownika identyfikator połączenia i utrwalić mapowania, można użyć jednej z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="11a27-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="11a27-121">Identyfikator użytkownika dostawcy (SignalR 2)</span><span class="sxs-lookup"><span data-stu-id="11a27-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="11a27-122">[Magazyn w pamięci](#inmemory), takich jak słownik</span><span class="sxs-lookup"><span data-stu-id="11a27-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="11a27-123">Grupy SignalR dla każdego użytkownika</span><span class="sxs-lookup"><span data-stu-id="11a27-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="11a27-124">[Magazynu zewnętrznego, stałe](#database), na przykład tabeli bazy danych lub magazynu tabel platformy Azure</span><span class="sxs-lookup"><span data-stu-id="11a27-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="11a27-125">Każda z tych implementacji jest wyświetlany w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="11a27-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="11a27-126">Możesz użyć `OnConnected`, `OnDisconnected`, i `OnReconnected` metody `Hub` klasy do śledzenia stanu połączenia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="11a27-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="11a27-127">Najlepszym rozwiązaniem dla twojej aplikacji jest zależna od:</span><span class="sxs-lookup"><span data-stu-id="11a27-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="11a27-128">Liczba serwerów sieci web hostującego aplikację.</span><span class="sxs-lookup"><span data-stu-id="11a27-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="11a27-129">Czy trzeba uzyskać listę aktualnie połączonych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="11a27-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="11a27-130">Określa, czy należy zachować informacje grup i użytkowników, po ponownym uruchomieniu aplikacji lub serwera.</span><span class="sxs-lookup"><span data-stu-id="11a27-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="11a27-131">Określa, czy opóźnienie wywoływania zewnętrznego serwera jest problem.</span><span class="sxs-lookup"><span data-stu-id="11a27-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="11a27-132">Poniższej tabeli rozwiązania działa te zagadnienia.</span><span class="sxs-lookup"><span data-stu-id="11a27-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="11a27-133">Więcej niż jednego serwera</span><span class="sxs-lookup"><span data-stu-id="11a27-133">More than one server</span></span> | <span data-ttu-id="11a27-134">Pobierz listę aktualnie połączonych użytkowników</span><span class="sxs-lookup"><span data-stu-id="11a27-134">Get list of currently connected users</span></span> | <span data-ttu-id="11a27-135">Utrwalanie informacje po uruchomieniu</span><span class="sxs-lookup"><span data-stu-id="11a27-135">Persist information after restarts</span></span> | <span data-ttu-id="11a27-136">Optymalnej wydajności</span><span class="sxs-lookup"><span data-stu-id="11a27-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="11a27-137">Identyfikator użytkownika dostawcy</span><span class="sxs-lookup"><span data-stu-id="11a27-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="11a27-138">W pamięci</span><span class="sxs-lookup"><span data-stu-id="11a27-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="11a27-139">Grupy jednego użytkownika</span><span class="sxs-lookup"><span data-stu-id="11a27-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="11a27-140">Stałe, zewnętrzne</span><span class="sxs-lookup"><span data-stu-id="11a27-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="11a27-141">Dostawca IUserID</span><span class="sxs-lookup"><span data-stu-id="11a27-141">IUserID provider</span></span>

<span data-ttu-id="11a27-142">Ta funkcja umożliwia użytkownikom na określenie, co to jest identyfikator userId oparte na IRequest za pomocą nowego interfejsu IUserIdProvider.</span><span class="sxs-lookup"><span data-stu-id="11a27-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="11a27-143">**IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="11a27-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="11a27-144">Domyślnie będzie implementację, która używa użytkownika `IPrincipal.Identity.Name` jako nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="11a27-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="11a27-145">Aby zmienić to ustawienie, zarejestruj się w implementacji `IUserIdProvider` z globalnego hosta podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="11a27-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="11a27-146">Z wewnątrz koncentratora, będzie możliwe do wysyłania komunikatów do tych użytkowników za pomocą następujących interfejsu API:</span><span class="sxs-lookup"><span data-stu-id="11a27-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="11a27-147">**Wysyłanie wiadomości do określonego użytkownika**</span><span class="sxs-lookup"><span data-stu-id="11a27-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="11a27-148">Magazyn w pamięci</span><span class="sxs-lookup"><span data-stu-id="11a27-148">In-memory storage</span></span>

<span data-ttu-id="11a27-149">Poniższe przykłady przedstawiają sposób przechowywania informacji i połączeń w słowniku, który jest przechowywany w pamięci.</span><span class="sxs-lookup"><span data-stu-id="11a27-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="11a27-150">Słownik używa `HashSet` do przechowywania identyfikator połączenia. W dowolnym momencie użytkownik może mieć więcej niż jedno połączenie z aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="11a27-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="11a27-151">Na przykład użytkownik, który jest połączony za pomocą wielu urządzeń lub więcej niż jedną kartę przeglądarki musi więcej niż jeden identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="11a27-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="11a27-152">Jeśli aplikacja zostanie wyłączony, wszystkie informacje zostaną utracone, ale go ponownie różnią się jako użytkownicy ponownie ustanowić połączenia.</span><span class="sxs-lookup"><span data-stu-id="11a27-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="11a27-153">Magazynu w pamięci nie działa, jeśli środowisko obejmuje więcej niż jeden serwer sieci web, ponieważ każdy serwer musi zbiór oddzielnego połączenia.</span><span class="sxs-lookup"><span data-stu-id="11a27-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="11a27-154">W pierwszym przykładzie klasa, która zarządza mapowanie użytkowników do połączeń.</span><span class="sxs-lookup"><span data-stu-id="11a27-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="11a27-155">Klucz dla zestaw HashSet będzie nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="11a27-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="11a27-156">Następnym przykładzie pokazano sposób użycia klasy mapowania połączenia z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="11a27-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="11a27-157">Wystąpienie klasy są przechowywane w nazwie zmiennej `_connections`.</span><span class="sxs-lookup"><span data-stu-id="11a27-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="11a27-158">Grupy jednego użytkownika</span><span class="sxs-lookup"><span data-stu-id="11a27-158">Single-user groups</span></span>

<span data-ttu-id="11a27-159">Można utworzyć grupę dla każdego użytkownika, a następnie wyślij wiadomość do tej grupy, jeśli chcesz osiągnąć tylko ten użytkownik.</span><span class="sxs-lookup"><span data-stu-id="11a27-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="11a27-160">Nazwa każdej grupy jest nazwa użytkownika.</span><span class="sxs-lookup"><span data-stu-id="11a27-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="11a27-161">Jeśli użytkownik ma więcej niż jedno połączenie, każdy identyfikator połączenia jest dodawane do grupy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="11a27-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="11a27-162">Nie należy ręcznie usuwać użytkownika z grupy po jego rozłączeniu.</span><span class="sxs-lookup"><span data-stu-id="11a27-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="11a27-163">Ta akcja jest wykonywana automatycznie przez platformę SignalR.</span><span class="sxs-lookup"><span data-stu-id="11a27-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="11a27-164">Poniższy przykład pokazuje, jak grupy pojedynczego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="11a27-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="11a27-165">Stałe magazynu zewnętrznego</span><span class="sxs-lookup"><span data-stu-id="11a27-165">Permanent, external storage</span></span>

<span data-ttu-id="11a27-166">W tym temacie przedstawiono sposób korzystania z bazy danych lub magazynu tabel platformy Azure do przechowywania informacji o połączeniu.</span><span class="sxs-lookup"><span data-stu-id="11a27-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="11a27-167">Ta metoda działa, gdy używanych jest wiele serwerów sieci web, ponieważ każdy serwer sieci web mogą prowadzić interakcję z tym samym repozytorium danych.</span><span class="sxs-lookup"><span data-stu-id="11a27-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="11a27-168">Jeśli serwerów sieci web Zatrzymaj pracy lub ponownego uruchomienia aplikacji, `OnDisconnected` nie jest wywoływana metoda.</span><span class="sxs-lookup"><span data-stu-id="11a27-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="11a27-169">W związku z tym jest to możliwe, że repozytorium danych rekordów identyfikatorów połączeń, które nie są już prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="11a27-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="11a27-170">Aby wyczyścić te rekordy oddzielone, możesz unieważnić dowolnego połączenia, który został utworzony poza przedział czasu, która ma zastosowanie do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="11a27-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="11a27-171">Przykłady w tej sekcji zawierają wartość śledzenia, gdy połączenie zostało utworzone, ale nie przedstawiają sposób wyczyścić stare rekordy, ponieważ chcesz to zrobić jako proces w tle.</span><span class="sxs-lookup"><span data-stu-id="11a27-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="11a27-172">Baza danych</span><span class="sxs-lookup"><span data-stu-id="11a27-172">Database</span></span>

<span data-ttu-id="11a27-173">Następujące przykłady przedstawiają sposób przechowywania informacji i połączeń w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="11a27-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="11a27-174">Można użyć dowolnego technologii dostępu do danych; Jednak w poniższym przykładzie pokazano sposób definiowania modeli używający narzędzia Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="11a27-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="11a27-175">Te modele jednostki odpowiadają tabele bazy danych i pola.</span><span class="sxs-lookup"><span data-stu-id="11a27-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="11a27-176">Struktury danych może być bardzo zróżnicowana w zależności od wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="11a27-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="11a27-177">Pierwszym przykładzie pokazano sposób definiowania jednostki użytkownika, który może być skojarzony z wielu obiektów połączenia.</span><span class="sxs-lookup"><span data-stu-id="11a27-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="11a27-178">Następnie w Centrum, można śledzić stan każdego połączenia kodem przedstawionym poniżej.</span><span class="sxs-lookup"><span data-stu-id="11a27-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="11a27-179">Magazyn tabel Azure</span><span class="sxs-lookup"><span data-stu-id="11a27-179">Azure table storage</span></span>

<span data-ttu-id="11a27-180">W poniższym przykładzie magazynu tabel Azure jest podobny do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="11a27-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="11a27-181">Nie obejmują wszystkie informacje, że konieczne będzie wprowadzenie do usługi Magazyn tabel Azure.</span><span class="sxs-lookup"><span data-stu-id="11a27-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="11a27-182">Aby uzyskać informacje, zobacz [jak używać magazynu tabel w .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="11a27-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="11a27-183">W poniższym przykładzie przedstawiono jednostkę tabeli do przechowywania informacji o połączeniu.</span><span class="sxs-lookup"><span data-stu-id="11a27-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="11a27-184">Dzieli dane według nazwy użytkownika i informacjami każdej jednostki według identyfikatora połączenia, dzięki czemu użytkownik może mieć wiele połączeń w dowolnym momencie.</span><span class="sxs-lookup"><span data-stu-id="11a27-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="11a27-185">Koncentrator służy do śledzenia stanu połączenia każdego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="11a27-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
