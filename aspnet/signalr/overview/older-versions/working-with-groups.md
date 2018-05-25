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
---
<a name="working-with-groups-in-signalr-1x"></a><span data-ttu-id="cb07c-103">Praca z grupami w SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="cb07c-103">Working with Groups in SignalR 1.x</span></span>
====================
<span data-ttu-id="cb07c-104">przez [Patrick Fletcher](https://github.com/pfletcher), [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="cb07c-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="cb07c-105">W tym temacie opisano, jak dodać użytkowników do grup i zachować informacje o członkostwie w grupie.</span><span class="sxs-lookup"><span data-stu-id="cb07c-105">This topic describes how to add users to groups and persist group membership information.</span></span>


## <a name="overview"></a><span data-ttu-id="cb07c-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="cb07c-106">Overview</span></span>

<span data-ttu-id="cb07c-107">Grupy w SignalR udostępnia metody emisji wiadomości do określonego podzbiór połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="cb07c-107">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="cb07c-108">Grupa może zawierać dowolną liczbę klientów, a klient może być członkiem dowolnej liczby grup.</span><span class="sxs-lookup"><span data-stu-id="cb07c-108">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="cb07c-109">Nie trzeba jawnie tworzyć grupy.</span><span class="sxs-lookup"><span data-stu-id="cb07c-109">You don't have to explicitly create groups.</span></span> <span data-ttu-id="cb07c-110">W efekcie grupy jest tworzony automatycznie określić jego nazwę w wywołaniu Groups.Add po raz pierwszy i zostaje usunięte po usunięciu ostatniego połączenia z członkostwa w nim.</span><span class="sxs-lookup"><span data-stu-id="cb07c-110">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="cb07c-111">Aby obejrzeć wprowadzenie do korzystania z grup, zobacz [jak zarządzać członkostwa w grupie z klasy koncentratora](index.md) w interfejsie API koncentratory — przewodnik serwera.</span><span class="sxs-lookup"><span data-stu-id="cb07c-111">For an introduction to using groups, see [How to manage group membership from the Hub class](index.md) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="cb07c-112">Brak żadnego interfejsu API do pobierania listy członkostwa grupy lub grup.</span><span class="sxs-lookup"><span data-stu-id="cb07c-112">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="cb07c-113">SignalR wysyła wiadomości do klientów i grup na podstawie modelu pub/sub, a serwer nie przechowuje list grup lub członkostwa w grupach.</span><span class="sxs-lookup"><span data-stu-id="cb07c-113">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="cb07c-114">Dzięki temu można zmaksymalizować skalowalność, ponieważ po dodaniu węzła do farmy sieci web SignalR zachowuje stan ma być propagowane do nowego węzła.</span><span class="sxs-lookup"><span data-stu-id="cb07c-114">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="cb07c-115">Podczas dodawania użytkownika do grupy przy użyciu `Groups.Add` metody, użytkownik odbiera komunikaty kierowane do tej grupy w czasie trwania bieżącego połączenia, ale członkostwa użytkownika w tej grupie nie jest trwały poza bieżącego połączenia.</span><span class="sxs-lookup"><span data-stu-id="cb07c-115">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="cb07c-116">Jeśli chcesz trwale przechowywane informacje o grupach oraz członkostwa w grupie, dane muszą być przechowywane w repozytorium, takie jak bazy danych lub magazynu tabel platformy Azure.</span><span class="sxs-lookup"><span data-stu-id="cb07c-116">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="cb07c-117">Następnie każdym razem, gdy użytkownik łączy do aplikacji, możesz pobrać z repozytorium, które użytkownik należy do grupy i ręcznie dodać tego użytkownika do tych grup.</span><span class="sxs-lookup"><span data-stu-id="cb07c-117">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="cb07c-118">Przy ponownym łączeniu po przerwaniu tymczasowe, użytkownik automatycznie ponownie łączy poprzednio przypisanych grup.</span><span class="sxs-lookup"><span data-stu-id="cb07c-118">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="cb07c-119">Automatyczne ponowne przyłączanie grupy tylko wtedy, gdy połączyć się ponownie, nie tworząc nowe połączenie.</span><span class="sxs-lookup"><span data-stu-id="cb07c-119">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="cb07c-120">Token podpisane cyfrowo są przekazywane z klienta, który zawiera listę grup uprzednio przypisane.</span><span class="sxs-lookup"><span data-stu-id="cb07c-120">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="cb07c-121">Jeśli chcesz sprawdzić, czy użytkownik należy do żądanej grupy, można zastąpić domyślne zachowanie.</span><span class="sxs-lookup"><span data-stu-id="cb07c-121">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="cb07c-122">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="cb07c-122">This topic includes the following sections:</span></span>

- [<span data-ttu-id="cb07c-123">Dodawanie i usuwanie użytkowników</span><span class="sxs-lookup"><span data-stu-id="cb07c-123">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="cb07c-124">Wywoływanie elementów członkowskich grupy</span><span class="sxs-lookup"><span data-stu-id="cb07c-124">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="cb07c-125">Zapisywanie w bazie danych członkostwa w grupie</span><span class="sxs-lookup"><span data-stu-id="cb07c-125">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="cb07c-126">Przechowywanie członkostwa w grupie w magazynie tabel platformy Azure</span><span class="sxs-lookup"><span data-stu-id="cb07c-126">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="cb07c-127">Sprawdzanie członkostwa w grupie przy ponownym łączeniu</span><span class="sxs-lookup"><span data-stu-id="cb07c-127">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="cb07c-128">Dodawanie i usuwanie użytkowników</span><span class="sxs-lookup"><span data-stu-id="cb07c-128">Adding and removing users</span></span>

<span data-ttu-id="cb07c-129">Aby dodać lub usunąć użytkowników z grupy, należy wywołać [Dodaj](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) lub [Usuń](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metody i przekaż użytkownika połączenia identyfikator i nazwę grupy jako parametry.</span><span class="sxs-lookup"><span data-stu-id="cb07c-129">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="cb07c-130">Nie trzeba ręcznie usunąć użytkownika z grupy, po zakończeniu połączenia.</span><span class="sxs-lookup"><span data-stu-id="cb07c-130">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="cb07c-131">W poniższym przykładzie przedstawiono `Groups.Add` i `Groups.Remove` metody używane w metodach koncentratora.</span><span class="sxs-lookup"><span data-stu-id="cb07c-131">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="cb07c-132">`Groups.Add` i `Groups.Remove` metod wykonania asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="cb07c-132">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="cb07c-133">Jeśli chcesz dodać do grupy klienta i natychmiast Wyślij wiadomość do klienta przy użyciu grupy masz upewnij się, że metoda Groups.Add kończy się najpierw.</span><span class="sxs-lookup"><span data-stu-id="cb07c-133">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="cb07c-134">W poniższych przykładach kodu pokazują, jak to zrobić, za pomocą kodu, który działa w .NET 4.5 i za pomocą kodu, który działa w .NET 4.</span><span class="sxs-lookup"><span data-stu-id="cb07c-134">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4.</span></span>

#### <a name="asynchronous-net-45-example"></a><span data-ttu-id="cb07c-135">Asynchroniczne .NET 4.5 przykład</span><span class="sxs-lookup"><span data-stu-id="cb07c-135">Asynchronous .NET 4.5 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a><span data-ttu-id="cb07c-136">Asynchroniczne .NET 4 przykład</span><span class="sxs-lookup"><span data-stu-id="cb07c-136">Asynchronous .NET 4 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

<span data-ttu-id="cb07c-137">Ogólnie rzecz biorąc, nie należy używać `await` podczas wywoływania metody `Groups.Remove` metody, ponieważ identyfikator połączenia, który próbujesz usunąć przestaną być dostępne.</span><span class="sxs-lookup"><span data-stu-id="cb07c-137">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="cb07c-138">W takim przypadku `TaskCanceledException` jest zgłaszany po upływie limitu czasu żądania. Jeśli aplikacja musi zapewnić, że użytkownik został usunięty z grupy przed wysłaniem wiadomości do grupy, możesz dodać `await` przed Groups.Remove, a następnie catch `TaskCanceledException` wyjątek, który może zostać wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="cb07c-138">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="cb07c-139">Wywoływanie elementów członkowskich grupy</span><span class="sxs-lookup"><span data-stu-id="cb07c-139">Calling members of a group</span></span>

<span data-ttu-id="cb07c-140">Możesz wysłać wiadomości do wszystkich członków grupy lub tylko członkowie określonej grupy, jak pokazano w poniższych przykładach.</span><span class="sxs-lookup"><span data-stu-id="cb07c-140">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="cb07c-141">**Wszystkie** połączonych klientów w określonej grupie.</span><span class="sxs-lookup"><span data-stu-id="cb07c-141">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- <span data-ttu-id="cb07c-142">Wszyscy połączeni klienci w określonej grupie **oprócz określonych klientów**, zidentyfikowane przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="cb07c-142">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- <span data-ttu-id="cb07c-143">Wszyscy połączeni klienci w określonej grupie **oprócz klienta wywołującego**.</span><span class="sxs-lookup"><span data-stu-id="cb07c-143">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="cb07c-144">Zapisywanie w bazie danych członkostwa w grupie</span><span class="sxs-lookup"><span data-stu-id="cb07c-144">Storing group membership in a database</span></span>

<span data-ttu-id="cb07c-145">Poniższe przykłady przedstawiają sposób przechowywania informacji grup i użytkowników w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="cb07c-145">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="cb07c-146">Można użyć dowolnego technologii dostępu do danych; Jednak w poniższym przykładzie pokazano sposób definiowania modeli używający narzędzia Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cb07c-146">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="cb07c-147">Te modele jednostki odpowiadają tabele bazy danych i pola.</span><span class="sxs-lookup"><span data-stu-id="cb07c-147">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="cb07c-148">Struktury danych może być bardzo zróżnicowana w zależności od wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="cb07c-148">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="cb07c-149">Ten przykład zawiera klasę o nazwie `ConversationRoom` będzie unikatowy dla aplikacji, która umożliwia użytkownikom join konwersacji o różnych tematów, takich jak sportowych lub ogrodnictwa.</span><span class="sxs-lookup"><span data-stu-id="cb07c-149">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="cb07c-150">W tym przykładzie również zawiera klasę dla połączenia.</span><span class="sxs-lookup"><span data-stu-id="cb07c-150">This example also includes a class for the connections.</span></span> <span data-ttu-id="cb07c-151">Klasa połączenia nie jest bezwzględnie wymagane do śledzenia członkostwa w grupie, ale często jest częścią niezawodne rozwiązanie w celu śledzenia użytkowników.</span><span class="sxs-lookup"><span data-stu-id="cb07c-151">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<span data-ttu-id="cb07c-152">Następnie w Centrum, można pobrać informacji o grupie i użytkownika z bazy danych i ręcznie Dodaj użytkownika do odpowiednich grup.</span><span class="sxs-lookup"><span data-stu-id="cb07c-152">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="cb07c-153">Przykład nie ma kodu do śledzenia połączeń użytkowników.</span><span class="sxs-lookup"><span data-stu-id="cb07c-153">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="cb07c-154">W tym przykładzie `await` — słowo kluczowe nie została zastosowana przed `Groups.Add` ponieważ wiadomość nie są natychmiast wysyłane do członków grupy.</span><span class="sxs-lookup"><span data-stu-id="cb07c-154">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="cb07c-155">Jeśli chcesz wysłać wiadomość do wszystkich członków grupy bezpośrednio po dodaniu nowego członka, czy chcesz zastosować `await` — słowo kluczowe, aby się upewnić, że operacja asynchroniczna została ukończona.</span><span class="sxs-lookup"><span data-stu-id="cb07c-155">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="cb07c-156">Przechowywanie członkostwa w grupie w magazynie tabel platformy Azure</span><span class="sxs-lookup"><span data-stu-id="cb07c-156">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="cb07c-157">Za pomocą magazynu tabel platformy Azure do przechowywania informacji o grup i użytkowników jest podobne do korzystania z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="cb07c-157">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="cb07c-158">W poniższym przykładzie przedstawiono jednostki tabeli, która przechowuje nazwę użytkownika i nazwę grupy.</span><span class="sxs-lookup"><span data-stu-id="cb07c-158">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<span data-ttu-id="cb07c-159">W Centrum pobierania przypisanych grup gdy użytkownik nawiąże połączenie.</span><span class="sxs-lookup"><span data-stu-id="cb07c-159">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="cb07c-160">Sprawdzanie członkostwa w grupie przy ponownym łączeniu</span><span class="sxs-lookup"><span data-stu-id="cb07c-160">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="cb07c-161">Domyślnie SignalR automatycznie ponownie przypisuje użytkownika do odpowiednich grup przy ponownym łączeniu z tymczasowego przerw w działaniu, np. gdy połączenie jest porzucona i nawiązał ponownie, zanim upłynie limit czasu połączenia. Informacje o grupie użytkowników jest przekazywana do tokenu, gdy połączyć się ponownie, a ten token jest weryfikowany na serwerze.</span><span class="sxs-lookup"><span data-stu-id="cb07c-161">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="cb07c-162">Informacje o proces weryfikacji ponowne przyłączanie użytkowników do grup, zobacz [ponowne przyłączanie grup przy ponownym łączeniu](index.md).</span><span class="sxs-lookup"><span data-stu-id="cb07c-162">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](index.md).</span></span>

<span data-ttu-id="cb07c-163">Ogólnie rzecz biorąc należy używać domyślne zachowanie automatyczne ponowne przyłączanie się, że grupy na ponowne łączenie.</span><span class="sxs-lookup"><span data-stu-id="cb07c-163">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="cb07c-164">SignalR grup nie są przeznaczone jako mechanizm zabezpieczeń w celu ograniczania dostępu do poufnych danych.</span><span class="sxs-lookup"><span data-stu-id="cb07c-164">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="cb07c-165">Jednak w aplikacji należy dokładnie sprawdzić członkostwa w grupie użytkownika przy ponownym łączeniu, można zastąpić domyślne zachowanie.</span><span class="sxs-lookup"><span data-stu-id="cb07c-165">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="cb07c-166">Zmianę zachowania domyślnego można dodać obciążenie do bazy danych, ponieważ można pobrać członkostwa użytkownika w grupach, dla każdego ponowne nawiązanie połączenia, a nie tylko w przypadku, gdy użytkownik nawiąże połączenie.</span><span class="sxs-lookup"><span data-stu-id="cb07c-166">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="cb07c-167">Należy sprawdzić członkostwa w grupie na ponownie połączyć, utworzyć nowy moduł potoku koncentratora, który zwraca listę przypisanych grup, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="cb07c-167">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

<span data-ttu-id="cb07c-168">Następnie należy dodać ten moduł do potoku koncentratora, jak wyróżniono poniżej.</span><span class="sxs-lookup"><span data-stu-id="cb07c-168">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
