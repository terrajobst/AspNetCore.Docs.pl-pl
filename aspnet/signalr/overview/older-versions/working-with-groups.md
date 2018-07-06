---
uid: signalr/overview/older-versions/working-with-groups
title: Praca z grupami w SignalR 1.x | Dokumentacja firmy Microsoft
author: pfletcher
description: W tym temacie opisano, jak można utrwalić informacje o członkostwie w grupie przy użyciu interfejsu API Centrum.
ms.author: aspnetcontent
ms.date: 10/21/2013
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: d0bdf81493ac7b5f929abd7d4336f04736467c66
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803110"
---
<a name="working-with-groups-in-signalr-1x"></a><span data-ttu-id="aa3ee-103">Praca z grupami w SignalR 1.x</span><span class="sxs-lookup"><span data-stu-id="aa3ee-103">Working with Groups in SignalR 1.x</span></span>
====================
<span data-ttu-id="aa3ee-104">przez [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="aa3ee-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="aa3ee-105">W tym temacie opisano, jak dodać użytkowników do grup i zachować informacje o członkostwie w grupie.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-105">This topic describes how to add users to groups and persist group membership information.</span></span>


## <a name="overview"></a><span data-ttu-id="aa3ee-106">Omówienie</span><span class="sxs-lookup"><span data-stu-id="aa3ee-106">Overview</span></span>

<span data-ttu-id="aa3ee-107">Grupami w SignalR udostępnia metody emisji komunikatów określony podzbiór połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-107">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="aa3ee-108">Grupa może zawierać dowolną liczbę klientów, a klient może należeć do dowolnej liczby grup.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-108">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="aa3ee-109">Nie trzeba jawnie tworzyć grupy.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-109">You don't have to explicitly create groups.</span></span> <span data-ttu-id="aa3ee-110">W efekcie grupa zostanie utworzona automatycznie określić jego nazwę w wywołaniu Groups.Add po raz pierwszy, a zostanie usunięta po usunięciu ostatniego połączenia z członkostwa w nim.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-110">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="aa3ee-111">Wprowadzenie do korzystania z grup, zobacz [sposób zarządzania członkostwa w grupie z klasy koncentratora](index.md) w interfejsie API centrów — serwer przewodnik.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-111">For an introduction to using groups, see [How to manage group membership from the Hub class](index.md) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="aa3ee-112">Nie ma żadnych interfejsów API w celu uzyskania listy członkostwa grupy lub Podaj listę grup.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-112">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="aa3ee-113">SignalR wysyła wiadomości do klientów i grup oparty na modelu publikowania/subskrybowania, a serwer nie przechowuje listę grup lub członkostwa w grupach.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-113">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="aa3ee-114">Pozwala to zmaksymalizować skalowalność, ponieważ po każdym dodaniu węzła w farmie sieci web, stan, który przechowuje SignalR są propagowane do nowego węzła.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-114">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="aa3ee-115">Po dodaniu użytkownika do grupy przy użyciu `Groups.Add` metody, użytkownik otrzymuje wiadomości kierowane do tej grupy na czas trwania bieżącego połączenia, ale członkostwo użytkownika w tej grupie nie jest trwały poza bieżącym połączeniu.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-115">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="aa3ee-116">Jeśli chcesz trwale przechowywane informacje o grupach oraz członkostwa w grupie, dane muszą być przechowywane w repozytorium, takich jak bazy danych lub usługi Azure table storage.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-116">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="aa3ee-117">Następnie każdorazowo, gdy użytkownik łączy się z aplikacją, możesz pobrać z repozytorium użytkownik należy do grupy i ręcznie dodać tego użytkownika do tych grup.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-117">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="aa3ee-118">Podczas ponownego łączenia po przerwaniu tymczasowe, użytkownik automatycznie ponownie łączy wcześniej przypisane grupy.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-118">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="aa3ee-119">Automatyczne ponowne przyłączanie grup tylko wtedy, gdy ponowne nawiązywanie połączenia, nie ustanawiania nowego połączenia.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-119">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="aa3ee-120">Token podpisane cyfrowo są przekazywane z klienta, który zawiera listy uprzednio przypisanych grup.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-120">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="aa3ee-121">Jeśli chcesz sprawdzić, czy użytkownik należy do żądanej grupy, można zastąpić domyślne zachowanie.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-121">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="aa3ee-122">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="aa3ee-122">This topic includes the following sections:</span></span>

- [<span data-ttu-id="aa3ee-123">Dodawanie i usuwanie użytkowników</span><span class="sxs-lookup"><span data-stu-id="aa3ee-123">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="aa3ee-124">Wywoływanie elementów członkowskich grupy</span><span class="sxs-lookup"><span data-stu-id="aa3ee-124">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="aa3ee-125">Przechowywanie członkostwa w grupie w bazie danych</span><span class="sxs-lookup"><span data-stu-id="aa3ee-125">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="aa3ee-126">Zapisywanie członkostwa w grupach w usłudze Azure table storage</span><span class="sxs-lookup"><span data-stu-id="aa3ee-126">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="aa3ee-127">Sprawdzanie członkostwa w grupie przy ponownym łączeniu</span><span class="sxs-lookup"><span data-stu-id="aa3ee-127">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="aa3ee-128">Dodawanie i usuwanie użytkowników</span><span class="sxs-lookup"><span data-stu-id="aa3ee-128">Adding and removing users</span></span>

<span data-ttu-id="aa3ee-129">Aby dodać lub usunąć użytkowników z grupy, należy wywołać [Dodaj](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) lub [Usuń](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metod i przekaż identyfikator połączenia użytkownika oraz nazwę grupy jako parametry.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-129">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="aa3ee-130">Nie trzeba ręcznie usunąć użytkownika z grupy, po zakończeniu połączenia.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-130">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="aa3ee-131">W poniższym przykładzie przedstawiono `Groups.Add` i `Groups.Remove` metody używane w metodach koncentratora.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-131">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="aa3ee-132">`Groups.Add` i `Groups.Remove` metody są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-132">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="aa3ee-133">Jeśli chcesz dodać do grupy klienta i natychmiast wysłać wiadomość do klienta przy użyciu grupy, należy upewnić się, że metoda Groups.Add zakończy się pierwsza.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-133">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="aa3ee-134">W poniższych przykładach kodu pokazano, jak to zrobić, za pomocą kodu, który działa w .NET 4.5 i za pomocą kodu, który działa w programie .NET 4.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-134">The following code examples show how to do that, one by using code that works in .NET 4.5, and one by using code that works in .NET 4.</span></span>

#### <a name="asynchronous-net-45-example"></a><span data-ttu-id="aa3ee-135">Asynchroniczne .NET 4.5 przykład</span><span class="sxs-lookup"><span data-stu-id="aa3ee-135">Asynchronous .NET 4.5 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a><span data-ttu-id="aa3ee-136">Asynchroniczne .NET 4 przykład</span><span class="sxs-lookup"><span data-stu-id="aa3ee-136">Asynchronous .NET 4 Example</span></span>

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

<span data-ttu-id="aa3ee-137">Ogólnie rzecz biorąc, nie należy używać `await` podczas wywoływania `Groups.Remove` metody, ponieważ identyfikator połączenia, który próbujesz usunąć przestaną być dostępne.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-137">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="aa3ee-138">W takim przypadku `TaskCanceledException` jest zgłaszany po upłynie limit czasu żądania. Jeśli aplikacja musi zapewnić, że użytkownik został usunięty z grupy przed wysłaniem wiadomości do grupy, możesz dodać `await` przed Groups.Remove, a następnie catch `TaskCanceledException` wyjątek, który może zostać wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-138">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before Groups.Remove, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="aa3ee-139">Wywoływanie elementów członkowskich grupy</span><span class="sxs-lookup"><span data-stu-id="aa3ee-139">Calling members of a group</span></span>

<span data-ttu-id="aa3ee-140">Wiadomości można wysyłać do wszystkich członków grupy lub tylko członkowie określonej grupy, jak pokazano w poniższych przykładach.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-140">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="aa3ee-141">**Wszystkie** połączonych klientów w określonej grupie.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-141">**All** connected clients in a specified group.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- <span data-ttu-id="aa3ee-142">Wszyscy połączeni klienci w określonej grupie **oprócz określonych klientów**, zidentyfikowane przez identyfikator połączenia.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-142">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span> 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- <span data-ttu-id="aa3ee-143">Wszyscy połączeni klienci w określonej grupie **oprócz klienta wywołującego**.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-143">All connected clients in a specified group **except the calling client**.</span></span> 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="aa3ee-144">Przechowywanie członkostwa w grupie w bazie danych</span><span class="sxs-lookup"><span data-stu-id="aa3ee-144">Storing group membership in a database</span></span>

<span data-ttu-id="aa3ee-145">Następujące przykłady przedstawiają sposób przechowywania informacji grupy i użytkownika w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-145">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="aa3ee-146">Możesz użyć dowolnej technologii dostępu do danych; Jednak w poniższym przykładzie pokazano sposób definiowania modeli za pomocą platformy Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-146">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="aa3ee-147">Modele te jednostki odpowiadają tabel bazy danych i pola.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-147">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="aa3ee-148">Do struktury danych może być bardzo zróżnicowana w zależności od wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-148">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="aa3ee-149">Ten przykład zawiera klasę o nazwie `ConversationRoom` będzie unikatowy dla aplikacji, która pozwala użytkownikom na dołączanie do rozmowy dotyczące różnych tematów, takich jak sportu lub ogród.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-149">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="aa3ee-150">W tym przykładzie zawiera również klasy dla połączeń.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-150">This example also includes a class for the connections.</span></span> <span data-ttu-id="aa3ee-151">Klasa połączenia nie jest bezwzględnie wymagane dla śledzenia członkostwa w grupie, ale często jest częścią niezawodne rozwiązanie do śledzenia użytkowników.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-151">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<span data-ttu-id="aa3ee-152">Następnie w piaście, możesz pobrać grupy i użytkownika informacji z bazy danych i ręcznie dodać użytkownika do odpowiednich grup.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-152">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="aa3ee-153">Przykład zawiera kod śledzenia połączeń użytkowników.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-153">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="aa3ee-154">W tym przykładzie `await` — słowo kluczowe nie jest stosowane przed `Groups.Add` ponieważ wiadomość nie są natychmiast wysyłane do członków grupy.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-154">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="aa3ee-155">Jeśli chcesz wysłać wiadomość do wszystkich elementów członkowskich grupy bezpośrednio po dodaniu nowego elementu członkowskiego, będzie chciała zastosować `await` — słowo kluczowe, aby upewnić się, operacja asynchroniczna została zakończona.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-155">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="aa3ee-156">Zapisywanie członkostwa w grupach w usłudze Azure table storage</span><span class="sxs-lookup"><span data-stu-id="aa3ee-156">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="aa3ee-157">Za pomocą usługi Azure table storage do przechowywania informacji o grupy i użytkownika jest podobne do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-157">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="aa3ee-158">Poniższy przykład pokazuje jednostkę tabeli, która przechowuje nazwę użytkownika i nazwę grupy.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-158">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<span data-ttu-id="aa3ee-159">W Centrum możesz pobrać przypisanych grup, gdy użytkownik nawiązuje połączenie.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-159">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="aa3ee-160">Sprawdzanie członkostwa w grupie przy ponownym łączeniu</span><span class="sxs-lookup"><span data-stu-id="aa3ee-160">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="aa3ee-161">Domyślnie SignalR automatycznie ponownie przypisuje użytkownika do odpowiednich grup przy ponownym łączeniu zakłóceniach tymczasowej, np. podczas połączenia jest usunięty i ponownie nawiązane przed upływem limitu czasu połączenia. Informacje o grupie użytkowników jest przekazywany w tokenie, gdy ponowne nawiązywanie połączenia, a ten token jest weryfikowany na serwerze.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-161">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="aa3ee-162">Aby dowiedzieć się, proces weryfikacji przystąpienie użytkowników do grup, zobacz [ponowne przyłączanie grup przy ponownym łączeniu](index.md).</span><span class="sxs-lookup"><span data-stu-id="aa3ee-162">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](index.md).</span></span>

<span data-ttu-id="aa3ee-163">Ogólnie rzecz biorąc należy użyć domyślne zachowanie automatyczne ponowne przyłączanie, ponowne łączenie grup na.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-163">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="aa3ee-164">SignalR grup nie jest przeznaczona mechanizmu zabezpieczeń w celu ograniczania dostępu do poufnych danych.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-164">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="aa3ee-165">Jednak w przypadku aplikacji należy dokładnie sprawdzić członkostwa w grupie użytkowników, przy ponownym łączeniu, zachowanie domyślne można przesłonić.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-165">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="aa3ee-166">Zmiana domyślnego zachowania można dodać obciążenie do bazy danych, ponieważ członkostwo w grupie użytkownika musi zostać pobrany dla każdego ponowne nawiązanie połączenia, a nie tylko wtedy, gdy użytkownik połączy się.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-166">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="aa3ee-167">Jeśli musisz sprawdzić członkostwa w grupie na ponowne łączenie, utworzyć nowy moduł potoku koncentratora, zwracającej listę przypisanych grup, jak pokazano poniżej.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-167">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

<span data-ttu-id="aa3ee-168">Następnie dodaj ten moduł do potoku koncentratora, jak wyróżniono poniżej.</span><span class="sxs-lookup"><span data-stu-id="aa3ee-168">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
