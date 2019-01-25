---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Mapowanie użytkowników SignalR na połączenia | Dokumentacja firmy Microsoft
author: bradygaster
description: W tym temacie przedstawiono sposób przechowywania informacji o użytkownikach i ich połączeń. Patrick Fletcher pomogła zapisu w tym temacie. Wersje oprogramowania, używaną w tym temacie...
ms.author: bradyg
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 786fc6bbc0b8d430770cf19d1647dbdba26347aa
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837198"
---
<a name="mapping-signalr-users-to-connections"></a><span data-ttu-id="93f53-105">Mapowanie użytkowników usługi SignalR na połączenia</span><span class="sxs-lookup"><span data-stu-id="93f53-105">Mapping SignalR Users to Connections</span></span>
====================
<span data-ttu-id="93f53-106">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="93f53-106">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="93f53-107">W tym temacie przedstawiono sposób przechowywania informacji o użytkownikach i ich połączeń.</span><span class="sxs-lookup"><span data-stu-id="93f53-107">This topic shows how to retain information about users and their connections.</span></span>
>
> <span data-ttu-id="93f53-108">Patrick Fletcher pomogła zapisu w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="93f53-108">Patrick Fletcher helped write this topic.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="93f53-109">Wersje oprogramowania używaną w tym temacie</span><span class="sxs-lookup"><span data-stu-id="93f53-109">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="93f53-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="93f53-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="93f53-111">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="93f53-111">.NET 4.5</span></span>
> - <span data-ttu-id="93f53-112">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="93f53-112">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="93f53-113">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="93f53-113">Previous versions of this topic</span></span>
>
> <span data-ttu-id="93f53-114">Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="93f53-114">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="93f53-115">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="93f53-115">Questions and comments</span></span>
>
> <span data-ttu-id="93f53-116">Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="93f53-116">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="93f53-117">Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="93f53-117">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="introduction"></a><span data-ttu-id="93f53-118">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="93f53-118">Introduction</span></span>

<span data-ttu-id="93f53-119">Każdy klient nawiązywania połączenia z koncentratorem przekazuje identyfikatora unikatowego połączenia. Możesz pobrać tę wartość w `Context.ConnectionId` właściwość kontekst koncentratora.</span><span class="sxs-lookup"><span data-stu-id="93f53-119">Each client connecting to a hub passes a unique connection id. You can retrieve this value in the `Context.ConnectionId` property of the hub context.</span></span> <span data-ttu-id="93f53-120">Jeśli Twoja aplikacja potrzebuje do mapowania użytkownika identyfikator połączenia i utrwalić mapowania, można użyć jednej z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="93f53-120">If your application needs to map a user to the connection id and persist that mapping, you can use one of the following:</span></span>

- [<span data-ttu-id="93f53-121">Identyfikator użytkownika dostawcy (SignalR 2)</span><span class="sxs-lookup"><span data-stu-id="93f53-121">The User ID Provider (SignalR 2)</span></span>](#IUserIdProvider)
- <span data-ttu-id="93f53-122">[Pojemność magazynu w pamięci](#inmemory), takich jak słownik</span><span class="sxs-lookup"><span data-stu-id="93f53-122">[In-memory storage](#inmemory), such as a dictionary</span></span>
- [<span data-ttu-id="93f53-123">Grupa SignalR dla każdego użytkownika</span><span class="sxs-lookup"><span data-stu-id="93f53-123">SignalR group for each user</span></span>](#groups)
- <span data-ttu-id="93f53-124">[Magazynu zewnętrznego, stałe](#database), takich jak tabela bazy danych lub usługi Azure table storage</span><span class="sxs-lookup"><span data-stu-id="93f53-124">[Permanent, external storage](#database), such as a database table or Azure table storage</span></span>

<span data-ttu-id="93f53-125">Każda z tych implementacji jest wyświetlany w tym temacie.</span><span class="sxs-lookup"><span data-stu-id="93f53-125">Each of these implementations is shown in this topic.</span></span> <span data-ttu-id="93f53-126">Możesz użyć `OnConnected`, `OnDisconnected`, i `OnReconnected` metody `Hub` klasy w celu śledzenia stanu połączenia użytkownika.</span><span class="sxs-lookup"><span data-stu-id="93f53-126">You use the `OnConnected`, `OnDisconnected`, and `OnReconnected` methods of the `Hub` class to track the user connection status.</span></span>

<span data-ttu-id="93f53-127">Najlepszym rozwiązaniem dla twojej aplikacji zależy od:</span><span class="sxs-lookup"><span data-stu-id="93f53-127">The best approach for your application depends on:</span></span>

- <span data-ttu-id="93f53-128">Liczba serwerów sieci web hostuje aplikację.</span><span class="sxs-lookup"><span data-stu-id="93f53-128">The number of web servers hosting your application.</span></span>
- <span data-ttu-id="93f53-129">Czy należy uzyskać listę aktualnie połączonych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="93f53-129">Whether you need to get a list of the currently connected users.</span></span>
- <span data-ttu-id="93f53-130">Czy musisz utrwalić informacje o grupie i użytkownika, po ponownym uruchomieniu aplikacji lub serwera.</span><span class="sxs-lookup"><span data-stu-id="93f53-130">Whether you need to persist group and user information when the application or server restarts.</span></span>
- <span data-ttu-id="93f53-131">Opóźnienie podczas wywoływania zewnętrznego serwera, czy jest problem.</span><span class="sxs-lookup"><span data-stu-id="93f53-131">Whether the latency of calling an external server is an issue.</span></span>

<span data-ttu-id="93f53-132">Pokazano w poniższej tabeli podejście, które działa w przypadku tych zagadnień.</span><span class="sxs-lookup"><span data-stu-id="93f53-132">The following table shows which approach works for these considerations.</span></span>

|  | <span data-ttu-id="93f53-133">Więcej niż jeden serwer</span><span class="sxs-lookup"><span data-stu-id="93f53-133">More than one server</span></span> | <span data-ttu-id="93f53-134">Pobierz listę aktualnie połączonych użytkowników</span><span class="sxs-lookup"><span data-stu-id="93f53-134">Get list of currently connected users</span></span> | <span data-ttu-id="93f53-135">Utrwalanie informacji po uruchomieniu</span><span class="sxs-lookup"><span data-stu-id="93f53-135">Persist information after restarts</span></span> | <span data-ttu-id="93f53-136">Uzyskania optymalnej wydajności</span><span class="sxs-lookup"><span data-stu-id="93f53-136">Optimal performance</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="93f53-137">Identyfikator użytkownika dostawcy</span><span class="sxs-lookup"><span data-stu-id="93f53-137">UserID Provider</span></span> | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| <span data-ttu-id="93f53-138">W pamięci</span><span class="sxs-lookup"><span data-stu-id="93f53-138">In-memory</span></span> |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| <span data-ttu-id="93f53-139">Grupy pojedynczego użytkownika</span><span class="sxs-lookup"><span data-stu-id="93f53-139">Single-user groups</span></span> | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| <span data-ttu-id="93f53-140">Stałe, zewnętrzne</span><span class="sxs-lookup"><span data-stu-id="93f53-140">Permanent, external</span></span> | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a><span data-ttu-id="93f53-141">Dostawca IUserID</span><span class="sxs-lookup"><span data-stu-id="93f53-141">IUserID provider</span></span>

<span data-ttu-id="93f53-142">Ta funkcja pozwala użytkownikom na określenie, co to jest identyfikator userId oparte na IRequest za pomocą nowego interfejsu IUserIdProvider.</span><span class="sxs-lookup"><span data-stu-id="93f53-142">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider.</span></span>

<span data-ttu-id="93f53-143">**The IUserIdProvider**</span><span class="sxs-lookup"><span data-stu-id="93f53-143">**The IUserIdProvider**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

<span data-ttu-id="93f53-144">Domyślnie będzie implementację, który używa użytkownika `IPrincipal.Identity.Name` jako nazwy użytkownika.</span><span class="sxs-lookup"><span data-stu-id="93f53-144">By default, there will be an implementation that uses the user's `IPrincipal.Identity.Name` as the user name.</span></span> <span data-ttu-id="93f53-145">Aby zmienić to ustawienie, zarejestruj implementacji `IUserIdProvider` z hostem globalnego, podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="93f53-145">To change this, register your implementation of `IUserIdProvider` with the global host when your application starts:</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<span data-ttu-id="93f53-146">Z w ramach Centrum, można wysyłać komunikaty do tych użytkowników za pośrednictwem następujący interfejs API:</span><span class="sxs-lookup"><span data-stu-id="93f53-146">From within a hub, you'll be able to send messages to these users via the following API:</span></span>

<span data-ttu-id="93f53-147">**Wysyłanie komunikatu do określonego użytkownika**</span><span class="sxs-lookup"><span data-stu-id="93f53-147">**Sending a message to a specific user**</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a><span data-ttu-id="93f53-148">Pojemność magazynu w pamięci</span><span class="sxs-lookup"><span data-stu-id="93f53-148">In-memory storage</span></span>

<span data-ttu-id="93f53-149">Poniższe przykłady pokazują, jak zachować połączenie i informacji o użytkownikach w słowniku, który jest przechowywany w pamięci.</span><span class="sxs-lookup"><span data-stu-id="93f53-149">The following examples show how to retain connection and user information in a dictionary that is stored in memory.</span></span> <span data-ttu-id="93f53-150">Słownik używa `HashSet` do przechowywania identyfikatora połączenia. W dowolnym momencie użytkownik może mieć więcej niż jednego połączenia do aplikacji SignalR.</span><span class="sxs-lookup"><span data-stu-id="93f53-150">The dictionary uses a `HashSet` to store the connection id. At any time a user could have more than one connection to the SignalR application.</span></span> <span data-ttu-id="93f53-151">Na przykład użytkownik, który jest połączony za pośrednictwem wielu urządzeń lub więcej niż jedną kartę przeglądarki będzie mieć więcej niż jeden identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="93f53-151">For example, a user who is connected through multiple devices or more than one browser tab would have more than one connection id.</span></span>

<span data-ttu-id="93f53-152">Jeśli aplikacja zostanie wyłączony, wszystkie informacje zostaną utracone, ale jej ponownie różnią się jako użytkownicy ponownie ustanowić połączenia.</span><span class="sxs-lookup"><span data-stu-id="93f53-152">If the application shuts down, all of the information is lost, but it will be re-populated as the users re-establish their connections.</span></span> <span data-ttu-id="93f53-153">Pojemność magazynu w pamięci nie działa, jeśli środowisko obejmuje więcej niż jeden serwer sieci web, ponieważ każdy serwer będzie mieć kolekcję osobnych połączeń.</span><span class="sxs-lookup"><span data-stu-id="93f53-153">In-memory storage does not work if your environment includes more than one web server because each server would have a separate collection of connections.</span></span>

<span data-ttu-id="93f53-154">W pierwszym przykładzie pokazano klasę, która zarządza mapowanie użytkowników do połączenia.</span><span class="sxs-lookup"><span data-stu-id="93f53-154">The first example shows a class that manages the mapping of users to connections.</span></span> <span data-ttu-id="93f53-155">Klucz dla hashset — będzie nazwa użytkownika.</span><span class="sxs-lookup"><span data-stu-id="93f53-155">The key for the HashSet will be the user's name.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

<span data-ttu-id="93f53-156">Następny przykład pokazuje sposób użycia klasy mapowania połączenia z koncentratorem.</span><span class="sxs-lookup"><span data-stu-id="93f53-156">The next example shows how to use the connection mapping class from a hub.</span></span> <span data-ttu-id="93f53-157">Wystąpienie klasy są przechowywane w nazwie zmiennej `_connections`.</span><span class="sxs-lookup"><span data-stu-id="93f53-157">The instance of the class is stored in a variable name `_connections`.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a><span data-ttu-id="93f53-158">Grupy pojedynczego użytkownika</span><span class="sxs-lookup"><span data-stu-id="93f53-158">Single-user groups</span></span>

<span data-ttu-id="93f53-159">Można utworzyć grupę dla każdego użytkownika, a następnie wyślij wiadomość do tej grupy, gdy chcesz się połączyć tylko ten użytkownik.</span><span class="sxs-lookup"><span data-stu-id="93f53-159">You can create a group for each user, and then send a message to that group when you want to reach only that user.</span></span> <span data-ttu-id="93f53-160">Nazwa każdej grupy jest nazwa użytkownika.</span><span class="sxs-lookup"><span data-stu-id="93f53-160">The name of each group is the name of the user.</span></span> <span data-ttu-id="93f53-161">Jeśli użytkownik ma więcej niż jedno połączenie, każdy identyfikator połączenia zostanie dodany do grupy użytkowników.</span><span class="sxs-lookup"><span data-stu-id="93f53-161">If a user has more than one connection, each connection id is added to the user's group.</span></span>

<span data-ttu-id="93f53-162">Nie należy ręcznie usuwać użytkowników z grupy, gdy użytkownik zamknie połączenie.</span><span class="sxs-lookup"><span data-stu-id="93f53-162">You should not manually remove the user from the group when the user disconnects.</span></span> <span data-ttu-id="93f53-163">Ta akcja jest wykonywana automatycznie przez strukturę SignalR.</span><span class="sxs-lookup"><span data-stu-id="93f53-163">This action is automatically performed by the SignalR framework.</span></span>

<span data-ttu-id="93f53-164">Poniższy przykład pokazuje sposób implementacji grup pojedynczego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="93f53-164">The following example shows how to implement single-user groups.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a><span data-ttu-id="93f53-165">Magazynu zewnętrznego, stałe</span><span class="sxs-lookup"><span data-stu-id="93f53-165">Permanent, external storage</span></span>

<span data-ttu-id="93f53-166">W tym temacie przedstawiono sposób korzystania z bazy danych lub usługi Azure table storage do przechowywania informacji o połączeniu.</span><span class="sxs-lookup"><span data-stu-id="93f53-166">This topic shows how to use either a database or Azure table storage for storing connection information.</span></span> <span data-ttu-id="93f53-167">Ta metoda działa, gdy masz wiele serwerów sieci web, ponieważ każdy serwer sieci web mogą wchodzić w interakcje z tym samym repozytorium danych.</span><span class="sxs-lookup"><span data-stu-id="93f53-167">This approach works when you have multiple web servers because each web server can interact with the same data repository.</span></span> <span data-ttu-id="93f53-168">Jeśli serwery sieci web zaprzestanie działania lub aplikacja uruchamia się ponownie, `OnDisconnected` nie jest wywoływana metoda.</span><span class="sxs-lookup"><span data-stu-id="93f53-168">If your web servers stop working or the application restarts, the `OnDisconnected` method is not called.</span></span> <span data-ttu-id="93f53-169">Dlatego jest to możliwe, że repozytorium danych rekordów dla identyfikatorów połączeń, które nie są już prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="93f53-169">Therefore, it is possible that your data repository will have records for connection ids that are no longer valid.</span></span> <span data-ttu-id="93f53-170">Aby wyczyścić te rekordy oddzielone, możesz unieważnić dowolnego połączenia, który został utworzony poza przedział czasu, która jest odpowiednia dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93f53-170">To clean up these orphaned records, you may wish to invalidate any connection that was created outside of a timeframe that is relevant to your application.</span></span> <span data-ttu-id="93f53-171">Przykłady w tej sekcji zawierają wartość do śledzenia, gdy połączenie zostało utworzone, ale nie przedstawiają sposób wyczyścić stare rekordy, ponieważ może to zrobić jako proces w tle.</span><span class="sxs-lookup"><span data-stu-id="93f53-171">The examples in this section include a value for tracking when the connection was created, but do not show how to clean up old records because you may want to do that as background process.</span></span>

### <a name="database"></a><span data-ttu-id="93f53-172">Baza danych</span><span class="sxs-lookup"><span data-stu-id="93f53-172">Database</span></span>

<span data-ttu-id="93f53-173">Poniższe przykłady pokazują, jak zachować połączenie i informacji o użytkownikach w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="93f53-173">The following examples show how to retain connection and user information in a database.</span></span> <span data-ttu-id="93f53-174">Możesz użyć dowolnej technologii dostępu do danych; Jednak w poniższym przykładzie pokazano sposób definiowania modeli za pomocą platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="93f53-174">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="93f53-175">Modele te jednostki odpowiadają tabel bazy danych i pola.</span><span class="sxs-lookup"><span data-stu-id="93f53-175">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="93f53-176">Do struktury danych może być bardzo zróżnicowana w zależności od wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="93f53-176">Your data structure could vary considerably depending on the requirements of your application.</span></span>

<span data-ttu-id="93f53-177">Pierwszy przykład pokazuje jak zdefiniować jednostki użytkownika, które mogą być skojarzone z wielu jednostek połączenia.</span><span class="sxs-lookup"><span data-stu-id="93f53-177">The first example shows how to define a user entity that can be associated with many connection entities.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

<span data-ttu-id="93f53-178">Następnie z poziomu Centrum można śledzić stan każdego połączenia przy użyciu kodu pokazany poniżej.</span><span class="sxs-lookup"><span data-stu-id="93f53-178">Then, from the hub, you can track the state of each connection with the code shown below.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a><span data-ttu-id="93f53-179">Usługa Azure table storage</span><span class="sxs-lookup"><span data-stu-id="93f53-179">Azure table storage</span></span>

<span data-ttu-id="93f53-180">W poniższym przykładzie magazynu tabel platformy Azure jest podobne do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="93f53-180">The following Azure table storage example is similar to the database example.</span></span> <span data-ttu-id="93f53-181">Nie obejmują wszystkich informacji, że konieczne będzie wprowadzenie do usługi Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="93f53-181">It does not include all of the information that you would need to get started with Azure Table Storage Service.</span></span> <span data-ttu-id="93f53-182">Aby uzyskać informacje, zobacz [jak używać magazynu tabel w języku .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span><span class="sxs-lookup"><span data-stu-id="93f53-182">For information, see [How to use Table storage from .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).</span></span>

<span data-ttu-id="93f53-183">Poniższy przykład pokazuje jednostkę tabeli do przechowywania informacji o połączeniu.</span><span class="sxs-lookup"><span data-stu-id="93f53-183">The following example shows a table entity for storing connection information.</span></span> <span data-ttu-id="93f53-184">Dzieli dane według nazwy użytkownika i identyfikuje każdej jednostki według identyfikatora połączenia, dzięki czemu użytkownik może mieć wiele połączeń w dowolnym momencie.</span><span class="sxs-lookup"><span data-stu-id="93f53-184">It partitions the data by user name, and identifies each entity by the connection id, so a user can have multiple connections at any time.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

<span data-ttu-id="93f53-185">W Centrum możesz śledzić stan połączenia każdego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="93f53-185">In the hub, you track the status of each user's connection.</span></span>

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
