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
<a name="working-with-groups-in-signalr"></a><span data-ttu-id="722d6-103">Praca z grupami w SignalR</span><span class="sxs-lookup"><span data-stu-id="722d6-103">Working with Groups in SignalR</span></span>
====================
<span data-ttu-id="722d6-104">przez [Patrick Fletcher](https://github.com/pfletcher), [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="722d6-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="722d6-105">W tym temacie opisano, jak dodać użytkowników do grup i zachować informacje o członkostwie w grupie.</span><span class="sxs-lookup"><span data-stu-id="722d6-105">This topic describes how to add users to groups and persist group membership information.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="722d6-106">Wersje oprogramowania używane w tym temacie</span><span class="sxs-lookup"><span data-stu-id="722d6-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="722d6-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="722d6-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="722d6-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="722d6-108">.NET 4.5</span></span>
> - <span data-ttu-id="722d6-109">SignalR w wersji 2</span><span class="sxs-lookup"><span data-stu-id="722d6-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="722d6-110">Poprzednie wersje tego tematu</span><span class="sxs-lookup"><span data-stu-id="722d6-110">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="722d6-111">Aby uzyskać informacje dotyczące starszych wersji biblioteki signalr, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="722d6-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="722d6-112">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="722d6-112">Questions and comments</span></span>
> 
> <span data-ttu-id="722d6-113">Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony.</span><span class="sxs-lookup"><span data-stu-id="722d6-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="722d6-114">Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="722d6-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="overview"></a><span data-ttu-id="722d6-115">Omówienie</span><span class="sxs-lookup"><span data-stu-id="722d6-115">Overview</span></span>

<span data-ttu-id="722d6-116">Grupy w SignalR udostępnia metody emisji wiadomości do określonego podzbiór połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="722d6-116">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="722d6-117">Grupa może zawierać dowolną liczbę klientów, a klient może być członkiem dowolnej liczby grup.</span><span class="sxs-lookup"><span data-stu-id="722d6-117">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="722d6-118">Nie trzeba jawnie tworzyć grupy.</span><span class="sxs-lookup"><span data-stu-id="722d6-118">You don't have to explicitly create groups.</span></span> <span data-ttu-id="722d6-119">W efekcie grupy jest tworzony automatycznie określić jego nazwę w wywołaniu Groups.Add po raz pierwszy i zostaje usunięte po usunięciu ostatniego połączenia z członkostwa w nim.</span><span class="sxs-lookup"><span data-stu-id="722d6-119">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="722d6-120">Aby obejrzeć wprowadzenie do korzystania z grup, zobacz [jak zarządzać członkostwa w grupie z klasy koncentratora](hubs-api-guide-server.md#groupsfromhub) w interfejsie API koncentratory — przewodnik serwera.</span><span class="sxs-lookup"><span data-stu-id="722d6-120">For an introduction to using groups, see [How to manage group membership from the Hub class](hubs-api-guide-server.md#groupsfromhub) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="722d6-121">Brak żadnego interfejsu API do pobierania listy członkostwa grupy lub grup.</span><span class="sxs-lookup"><span data-stu-id="722d6-121">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="722d6-122">SignalR wysyła wiadomości do klientów i grup na podstawie modelu pub/sub, a serwer nie przechowuje list grup lub członkostwa w grupach.</span><span class="sxs-lookup"><span data-stu-id="722d6-122">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="722d6-123">Dzięki temu można zmaksymalizować skalowalność, ponieważ po dodaniu węzła do farmy sieci web SignalR zachowuje stan ma być propagowane do nowego węzła.</span><span class="sxs-lookup"><span data-stu-id="722d6-123">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="722d6-124">Podczas dodawania użytkownika do grupy przy użyciu `Groups.Add` metody, użytkownik odbiera komunikaty kierowane do tej grupy w czasie trwania bieżącego połączenia, ale członkostwa użytkownika w tej grupie nie jest trwały poza bieżącego połączenia.</span><span class="sxs-lookup"><span data-stu-id="722d6-124">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="722d6-125">Jeśli chcesz trwale przechowywane informacje o grupach oraz członkostwa w grupie, dane muszą być przechowywane w repozytorium, takie jak bazy danych lub magazynu tabel platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="722d6-125">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="722d6-126">Następnie każdym razem, gdy użytkownik łączy do aplikacji, możesz pobrać z repozytorium, które użytkownik należy do grupy i ręcznie dodać tego użytkownika do tych grup.</span><span class="sxs-lookup"><span data-stu-id="722d6-126">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="722d6-127">Przy ponownym łączeniu po przerwaniu tymczasowe, użytkownik automatycznie ponownie łączy poprzednio przypisanych grup.</span><span class="sxs-lookup"><span data-stu-id="722d6-127">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="722d6-128">Automatyczne ponowne przyłączanie grupy tylko wtedy, gdy połączyć się ponownie, nie tworząc nowe połączenie.</span><span class="sxs-lookup"><span data-stu-id="722d6-128">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="722d6-129">Token podpisane cyfrowo są przekazywane z klienta, który zawiera listę grup uprzednio przypisane.</span><span class="sxs-lookup"><span data-stu-id="722d6-129">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="722d6-130">Jeśli chcesz sprawdzić, czy użytkownik należy do żądanej grupy, można zastąpić domyślne zachowanie.</span><span class="sxs-lookup"><span data-stu-id="722d6-130">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="722d6-131">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="722d6-131">This topic includes the following sections:</span></span>

- [<span data-ttu-id="722d6-132">Dodawanie i usuwanie użytkowników</span><span class="sxs-lookup"><span data-stu-id="722d6-132">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="722d6-133">Wywoływanie elementów członkowskich grupy</span><span class="sxs-lookup"><span data-stu-id="722d6-133">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="722d6-134">Zapisywanie w bazie danych członkostwa w grupie</span><span class="sxs-lookup"><span data-stu-id="722d6-134">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="722d6-135">Przechowywanie członkostwa w grupie w magazynie tabel platformy Azure</span><span class="sxs-lookup"><span data-stu-id="722d6-135">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="722d6-136">Sprawdzanie członkostwa w grupie przy ponownym łączeniu</span><span class="sxs-lookup"><span data-stu-id="722d6-136">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="722d6-137">Dodawanie i usuwanie użytkowników</span><span class="sxs-lookup"><span data-stu-id="722d6-137">Adding and removing users</span></span>

<span data-ttu-id="722d6-138">Aby dodać lub usunąć użytkowników z grupy, należy wywołać [Dodaj](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) lub [Usuń](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metody i przekaż użytkownika połączenia identyfikator i nazwę grupy jako parametry.</span><span class="sxs-lookup"><span data-stu-id="722d6-138">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="722d6-139">Nie trzeba ręcznie usunąć użytkownika z grupy, po zakończeniu połączenia.</span><span class="sxs-lookup"><span data-stu-id="722d6-139">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="722d6-140">W poniższym przykładzie przedstawiono `Groups.Add` i `Groups.Remove` metody używane w metodach koncentratora.</span><span class="sxs-lookup"><span data-stu-id="722d6-140">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="722d6-141">`Groups.Add` i `Groups.Remove` metod wykonania asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="722d6-141">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="722d6-142">Jeśli chcesz dodać do grupy klienta i natychmiast Wyślij wiadomość do klienta przy użyciu grupy masz upewnij się, że metoda Groups.Add kończy się najpierw.</span><span class="sxs-lookup"><span data-stu-id="722d6-142">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="722d6-143">W poniższych przykładach kodu pokazują, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="722d6-143">The following code examples show how to do that.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

<span data-ttu-id="722d6-144">Ogólnie rzecz biorąc, nie należy używać `await` podczas wywoływania metody `Groups.Remove` metody, ponieważ identyfikator połączenia, który próbujesz usunąć przestaną być dostępne.</span><span class="sxs-lookup"><span data-stu-id="722d6-144">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="722d6-145">W takim przypadku `TaskCanceledException` jest zgłaszany po upływie limitu czasu żądania. Jeśli aplikacja musi zapewnić, że użytkownik został usunięty z grupy przed wysłaniem wiadomości do grupy, możesz dodać `await` przed Groups.Remove, a następnie catch `TaskCanceledException` wyjątek, który może zostać wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="722d6-145">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="722d6-146">Wywoływanie elementów członkowskich grupy</span><span class="sxs-lookup"><span data-stu-id="722d6-146">Calling members of a group</span></span>

<span data-ttu-id="722d6-147">Możesz wysłać wiadomości do wszystkich członków grupy lub tylko członkowie określonej grupy, jak pokazano w poniższych przykładach.</span><span class="sxs-lookup"><span data-stu-id="722d6-147">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="722d6-148">**Wszystkie** połączonych klientów w określonej grupie.</span><span class="sxs-lookup"><span data-stu-id="722d6-148">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- <span data-ttu-id="722d6-149">Wszyscy połączeni klienci w określonej grupie **oprócz określonych klientów**, zidentyfikowane przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="722d6-149">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- <span data-ttu-id="722d6-150">Wszyscy połączeni klienci w określonej grupie **oprócz klienta wywołującego**.</span><span class="sxs-lookup"><span data-stu-id="722d6-150">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="722d6-151">Zapisywanie w bazie danych członkostwa w grupie</span><span class="sxs-lookup"><span data-stu-id="722d6-151">Storing group membership in a database</span></span>

<span data-ttu-id="722d6-152">Poniższe przykłady przedstawiają sposób przechowywania informacji grup i użytkowników w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="722d6-152">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="722d6-153">Można użyć dowolnego technologii dostępu do danych; Jednak w poniższym przykładzie pokazano sposób definiowania modeli używający narzędzia Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="722d6-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="722d6-154">Te modele jednostki odpowiadają tabele bazy danych i pola.</span><span class="sxs-lookup"><span data-stu-id="722d6-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="722d6-155">Struktury danych może być bardzo zróżnicowana w zależności od wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="722d6-155">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="722d6-156">Ten przykład zawiera klasę o nazwie `ConversationRoom` będzie unikatowy dla aplikacji, która umożliwia użytkownikom join konwersacji o różnych tematów, takich jak sportowych lub ogrodnictwa.</span><span class="sxs-lookup"><span data-stu-id="722d6-156">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="722d6-157">W tym przykładzie również zawiera klasę dla połączenia.</span><span class="sxs-lookup"><span data-stu-id="722d6-157">This example also includes a class for the connections.</span></span> <span data-ttu-id="722d6-158">Klasa połączenia nie jest bezwzględnie wymagane do śledzenia członkostwa w grupie, ale często jest częścią niezawodne rozwiązanie w celu śledzenia użytkowników.</span><span class="sxs-lookup"><span data-stu-id="722d6-158">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

<span data-ttu-id="722d6-159">Następnie w Centrum, można pobrać informacji o grupie i użytkownika z bazy danych i ręcznie Dodaj użytkownika do odpowiednich grup.</span><span class="sxs-lookup"><span data-stu-id="722d6-159">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="722d6-160">Przykład nie ma kodu do śledzenia połączeń użytkowników.</span><span class="sxs-lookup"><span data-stu-id="722d6-160">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="722d6-161">W tym przykładzie `await` — słowo kluczowe nie została zastosowana przed `Groups.Add` ponieważ wiadomość nie są natychmiast wysyłane do członków grupy.</span><span class="sxs-lookup"><span data-stu-id="722d6-161">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="722d6-162">Jeśli chcesz wysłać wiadomość do wszystkich członków grupy bezpośrednio po dodaniu nowego członka, czy chcesz zastosować `await` — słowo kluczowe, aby się upewnić, że operacja asynchroniczna została ukończona.</span><span class="sxs-lookup"><span data-stu-id="722d6-162">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="722d6-163">Przechowywanie członkostwa w grupie w magazynie tabel platformy Azure</span><span class="sxs-lookup"><span data-stu-id="722d6-163">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="722d6-164">Za pomocą magazynu tabel platformy Azure do przechowywania informacji o grup i użytkowników jest podobne do korzystania z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="722d6-164">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="722d6-165">W poniższym przykładzie przedstawiono jednostki tabeli, która przechowuje nazwę użytkownika i nazwę grupy.</span><span class="sxs-lookup"><span data-stu-id="722d6-165">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<span data-ttu-id="722d6-166">W Centrum pobierania przypisanych grup gdy użytkownik nawiąże połączenie.</span><span class="sxs-lookup"><span data-stu-id="722d6-166">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="722d6-167">Sprawdzanie członkostwa w grupie przy ponownym łączeniu</span><span class="sxs-lookup"><span data-stu-id="722d6-167">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="722d6-168">Domyślnie SignalR automatycznie ponownie przypisuje użytkownika do odpowiednich grup przy ponownym łączeniu z tymczasowego przerw w działaniu, np. gdy połączenie jest porzucona i nawiązał ponownie, zanim upłynie limit czasu połączenia. Informacje o grupie użytkowników jest przekazywana do tokenu, gdy połączyć się ponownie, a ten token jest weryfikowany na serwerze.</span><span class="sxs-lookup"><span data-stu-id="722d6-168">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="722d6-169">Informacje o proces weryfikacji ponowne przyłączanie użytkowników do grup, zobacz [ponowne przyłączanie grup przy ponownym łączeniu](../security/introduction-to-security.md#rejoingroup).</span><span class="sxs-lookup"><span data-stu-id="722d6-169">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](../security/introduction-to-security.md#rejoingroup).</span></span>

<span data-ttu-id="722d6-170">Ogólnie rzecz biorąc należy używać domyślne zachowanie automatyczne ponowne przyłączanie się, że grupy na ponowne łączenie.</span><span class="sxs-lookup"><span data-stu-id="722d6-170">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="722d6-171">SignalR grup nie są przeznaczone jako mechanizm zabezpieczeń w celu ograniczania dostępu do poufnych danych.</span><span class="sxs-lookup"><span data-stu-id="722d6-171">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="722d6-172">Jednak w aplikacji należy dokładnie sprawdzić członkostwa w grupie użytkownika przy ponownym łączeniu, można zastąpić domyślne zachowanie.</span><span class="sxs-lookup"><span data-stu-id="722d6-172">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="722d6-173">Zmianę zachowania domyślnego można dodać obciążenie do bazy danych, ponieważ można pobrać członkostwa użytkownika w grupach, dla każdego ponowne nawiązanie połączenia, a nie tylko w przypadku, gdy użytkownik nawiąże połączenie.</span><span class="sxs-lookup"><span data-stu-id="722d6-173">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="722d6-174">Należy sprawdzić członkostwa w grupie na ponownie połączyć, utworzyć nowy moduł potoku koncentratora, który zwraca listę przypisanych grup, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="722d6-174">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<span data-ttu-id="722d6-175">Następnie należy dodać ten moduł do potoku koncentratora, jak wyróżniono poniżej.</span><span class="sxs-lookup"><span data-stu-id="722d6-175">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
