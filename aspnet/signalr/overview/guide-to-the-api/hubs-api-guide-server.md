---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-server
title: Podręcznik biblioteki SignalR platformy ASP.NET do interfejsu API centrów — serwer (C#) | Dokumentacja firmy Microsoft
author: pfletcher
description: Ten dokument zawiera wprowadzenie do programowania po stronie serwera interfejsu API centrów SignalR platformy ASP.NET dla elementu SignalR w wersji 2, przy użyciu przykładów kodu, demonstrując...
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: b19913e5-cd8a-4e4b-a872-5ac7a858a934
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-server
msc.type: authoredcontent
ms.openlocfilehash: f036d2bab466a02fdb566593aca8ec0b7d6aa897
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807508"
---
<a name="aspnet-signalr-hubs-api-guide---server-c"></a>Podręcznik biblioteki SignalR platformy ASP.NET do interfejsu API centrów — serwer (C#)
====================
przez [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

> Ten dokument zawiera wprowadzenie do programowania po stronie serwera interfejsu API centrów SignalR platformy ASP.NET dla elementu SignalR w wersji 2, przy użyciu przykładów kodu, demonstrując typowe opcje.
> 
> Interfejsu API centrów SignalR umożliwia zdalne wywołania procedur (RPC) z serwera do połączonych klientów i od klientów z serwerem. W kodzie serwera należy zdefiniować metody, które mogą być wywoływane przez klientów, a wywołanie metody, które są uruchamiane na komputerze klienckim. W kodzie klienta definiowania metod, które mogą być wywoływane z serwera, a wywołanie metody, które są uruchamiane na serwerze. SignalR zajmuje się wszystkie nadmiar klient serwer dla Ciebie.
> 
> SignalR oferuje również interfejs API niższego poziomu o nazwie połączeń trwałych. Wprowadzenie do SignalR, centra i połączenia trwałego, zobacz [wprowadzenie do SignalR 2](../getting-started/introduction-to-signalr.md).
> 
> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używaną w tym temacie
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR w wersji 2
>   
> 
> 
> ## <a name="topic-versions"></a>Wersje tematu
> 
> Aby uzyskać informacje dotyczące starszych wersji biblioteki SignalR, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Jak się podoba w tym samouczku, i co można było ulepszyć proces w komentarzach u dołu strony, wystaw opinię. Jeśli masz pytania, na które nie są bezpośrednio związane z tego samouczka, możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Omówienie

Ten dokument zawiera następujące sekcje:

- [Jak zarejestrować oprogramowanie pośredniczące SignalR](#route)

    - [Adres URL /signalr](#signalrurl)
    - [Konfigurowanie opcji SignalR](#options)
- [Jak utworzyć i używać klas Centrum](#hubclass)

    - [Okres istnienia obiektu Centrum](#transience)
    - [Wielkości liter pisane Centrum nazw klientów języka JavaScript](#hubnames)
    - [Wiele centrów](#multiplehubs)
    - [Koncentratory silnie Typizowane](#stronglytypedhubs)
- [Sposób definiowania metody w klasie Centrum, który można wywoływać klientów](#hubmethods)

    - [Pisane-wielkość liter w wyrazie nazwy metod w klientów języka JavaScript](#methodnames)
    - [Kiedy są wykonywane asynchronicznie](#asyncmethods)
    - [Definiowanie przeciążenia](#overloads)
    - [Raport z postępów z wywołań metod koncentratora](#progress)
- [Sposób wywoływania metody z klasy koncentratora klienta](#callfromhub)

    - [Wybieranie klientów, którzy będą otrzymywać RPC](#selectingclients)
    - [Nazwy metod weryfikacji kompilacji](#dynamicmethodnames)
    - [Dopasowywanie nazw bez uwzględniania wielkości liter — metoda](#caseinsensitive)
    - [Wykonanie asynchroniczne](#asyncclient)
- [Sposób zarządzania członkostwa w grupie z klasy koncentratora](#groupsfromhub)

    - [Asynchroniczne wykonywanie metody dodawania i usuwania](#asyncgroupmethods)
    - [Trwałość członkostwa grupy](#grouppersistence)
    - [Grupy pojedynczego użytkownika](#singleusergroups)
- [Jak obsługiwać zdarzenia okresu istnienia połączenia w klasie Centrum](#connectionlifetime)

    - [Kiedy są wywoływane onconnected elementu ondisconnected elementu i onreconnected elementu](#onreconnected)
    - [Stan elementu wywołującego, pusta](#nocallerstate)
- [Jak uzyskać informacji o kliencie z właściwości kontekstu](#contextproperty)
- [Jak przekazać stanu między klientami a klasy koncentratora](#passstate)
- [Sposób obsługi błędów w klasie Centrum](#handleErrors)
- [Jak wywołać metody klienta grup i zarządzanie nimi z poza klasy koncentratora](#callfromoutsidehub)

    - [Wywoływanie metody klienta](#callingclientsoutsidehub)
    - [Członkostwo w grupie zarządzania](#managinggroupsoutsidehub)
- [Jak włączyć śledzenie](#tracing)
- [Jak dostosować potoku koncentratory](#hubpipeline)

Dokumentacja na temat klientów programu na ten temat można znaleźć w następujących zasobach:

- [Podręcznik interfejsu API centrów SignalR — klient JavaScript](hubs-api-guide-javascript-client.md)
- [Podręcznik interfejsu API centrów SignalR — klient modelu .NET](hubs-api-guide-net-client.md)

Składniki serwera dla SignalR 2 są dostępne tylko w .NET 4.5. Serwery z systemem .NET 4.0, należy użyć SignalR v1.x.

<a id="route"></a>

## <a name="how-to-register-signalr-middleware"></a>Jak zarejestrować oprogramowanie pośredniczące SignalR

Aby zdefiniować trasę, którego klienci będą używać, aby nawiązać połączenie z Centrum, należy wywołać `MapSignalR` metoda podczas uruchamiania aplikacji. `MapSignalR` jest [— metoda rozszerzenia](https://msdn.microsoft.com/library/vstudio/bb383977.aspx) dla `OwinExtensions` klasy. Poniższy przykład pokazuje jak zdefiniować trasy centrów SignalR, używając klasy początkowej OWIN.

[!code-csharp[Main](hubs-api-guide-server/samples/sample1.cs)]

W przypadku dodawania funkcji SignalR do aplikacji ASP.NET MVC, upewnij się, że trasa SignalR jest dodawana przed innych tras. Aby uzyskać więcej informacji, zobacz [samouczek: wprowadzenie do SignalR 2 i MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).

<a id="signalrurl"></a>

### <a name="the-signalr-url"></a>Adres URL /signalr

Domyślnie jest adres URL trasy, której klienci będą używać, aby nawiązać połączenie z Centrum "/ signalr". (Nie należy mylić tego adresu URL za pomocą adresu URL "/ signalr/centra", który jest automatycznie wygenerowanego pliku JavaScript. Aby uzyskać więcej informacji na temat wygenerowany serwer proxy, zobacz [Podręcznik interfejsu API centrów SignalR — klient JavaScript — wygenerowany serwer proxy i przeznaczenie dla Ciebie](hubs-api-guide-javascript-client.md#genproxy).)

Może to być nadzwyczajne okoliczności, które to podstawowy adres URL nie można używać dla elementu SignalR; na przykład masz folder z projektu o nazwie *signalr* i nie chcesz zmienić nazwę. W takim przypadku można zmienić podstawowy adres URL, jak pokazano w poniższych przykładach (Zastąp "/ signalr" w przykładowym kodzie za pomocą żądany adres URL).

**Kod serwera, który określa adres URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample2.cs?highlight=1)]

**Kod klienta JavaScript, który określa adres URL (z wygenerowany serwer proxy)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample3.js?highlight=1)]

**Kod klienta JavaScript, który określa adres URL (bez wygenerowany serwer proxy)**

[!code-javascript[Main](hubs-api-guide-server/samples/sample4.js?highlight=1)]

**Kod klienta platformy .NET, który określa adres URL**

[!code-csharp[Main](hubs-api-guide-server/samples/sample5.cs?highlight=1)]

<a id="options"></a>

### <a name="configuring-signalr-options"></a>Konfigurowanie opcji SignalR

Przeciążenia `MapSignalR` metody umożliwiają określenie niestandardowego adresu URL, mechanizm rozpoznawania zależności niestandardowej i następujących opcji:

- Włącz wywołania między domenami przy użyciu mechanizmu CORS i JSONP z klientów przeglądarek.

    Zazwyczaj, gdy przeglądarka ładuje stronę z `http://contoso.com`, połączenia SignalR jest w tej samej domenie, w `http://contoso.com/signalr`. Jeśli strona z `http://contoso.com` nawiązuje połączenie `http://fabrikam.com/signalr`, oznacza to połączenie między domenami. Ze względów bezpieczeństwa połączeń między domenami są domyślnie wyłączone. Aby uzyskać więcej informacji, zobacz [ASP.NET SignalR Podręcznik interfejsu API centrów — klient JavaScript — jak nawiązać połączenie między domenami](hubs-api-guide-javascript-client.md#crossdomain).
- Włącz szczegółowe komunikaty o błędach.

    Jeśli wystąpią błędy, domyślne zachowanie SignalR jest wysyłania do klientów komunikat z powiadomieniem bez szczegółowe informacje o co się stało. Szczegółowe informacje o błędzie, wysyłają do klientów nie jest zalecane w środowisku produkcyjnym, ponieważ złośliwi użytkownicy mogą być w stanie użycie informacji o ataku na aplikację. Na potrzeby rozwiązywania problemów można użyć tej opcji tymczasowo włączyć bardziej szczegółowy raportowanie błędów.
- Wyłącz automatycznie generowanych plików serwera proxy JavaScript.

    Domyślnie plik języka JavaScript przy użyciu serwera proxy dla Twoich zajęciach Centrum jest generowany w odpowiedzi na adres URL "/ signalr/centra". Jeśli nie chcesz używać serwery proxy JavaScript lub jeśli chcesz ręcznie wygenerować ten plik i odnoszą się do pliku fizycznego w klientach, można użyć tej opcji można wyłączyć generowania serwera proxy. Aby uzyskać więcej informacji, zobacz [Podręcznik interfejsu API centrów SignalR — klient JavaScript — sposobu tworzenia pliku fizycznego dla elementu SignalR wygenerowany serwer proxy](hubs-api-guide-javascript-client.md#manualproxy).

Poniższy przykład pokazuje, jak określić te opcje i adres URL połączenia SignalR w wywołaniu `MapSignalR` metody. Aby określić niestandardowy adres URL, zastąp "/ signalr" w przykładzie, za pomocą adresu URL, którego chcesz używać.

[!code-csharp[Main](hubs-api-guide-server/samples/sample6.cs)]

<a id="hubclass"></a>

## <a name="how-to-create-and-use-hub-classes"></a>Jak utworzyć i używać klas Centrum

Aby utworzyć Centrum, należy utworzyć klasę, która jest pochodną [Microsoft.Aspnet.Signalr.Hub](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hub(v=vs.111).aspx). Poniższy przykład pokazuje prosty klasę Centrum aplikacji rozmów.

[!code-csharp[Main](hubs-api-guide-server/samples/sample7.cs)]

W tym przykładzie podłączony klient może wywołać `NewContosoChatMessage` metody, a jeśli tak jest, dane otrzymane jest wysłano do wszyscy połączeni klienci.

<a id="transience"></a>

### <a name="hub-object-lifetime"></a>Okres istnienia obiektu Centrum

Nie utworzyć wystąpienia klasy koncentratora lub wywołać jego metody w kodzie na serwerze; wszystko jest wykonywane przez potok koncentratorów SignalR. SignalR tworzy nowe wystąpienie klasy koncentratora każdej potrzebny do obsługi operacji koncentratora, np. kiedy klient nawiązuje połączenie, odłącza lub sprawia, że wywołanie metody do serwera.

Ponieważ wystąpienia klasy koncentratora przejściowy, nie możesz użyć ich do zarządzania stanem z jednego wywołania metody do następnego. Każdorazowo serwer odbiera wywołania metody od klienta, nowe wystąpienie klasy procesów Centrum wiadomości. Do zarządzania stanem przez wiele połączeń i wywołania metody, należy użyć innej metody takie jak bazy danych lub zmienna statyczna klasy koncentratora lub inną klasę, która nie pochodzi od `Hub`. Jeśli będzie się powtarzać dane w pamięci, za pomocą innej metody takie jak zmienna statyczna klasy koncentratora dane zostaną utracone podczas odtwarzania domen aplikacji.

Jeśli chcesz wysyłać komunikaty do klientów z własnego kodu, który działa poza klasy koncentratora, nie przez utworzenie wystąpienia klasy wystąpienie koncentratora, ale możesz to zrobić poprzez odwołanie do obiektu kontekstu SignalR dla swojej klasy koncentratora. Aby uzyskać więcej informacji, zobacz [jak wywołać metody klienta grup i zarządzanie nimi z poza klasy koncentratora](#callfromoutsidehub) w dalszej części tego tematu.

<a id="hubnames"></a>

### <a name="camel-casing-of-hub-names-in-javascript-clients"></a>Wielkości liter pisane Centrum nazw klientów języka JavaScript

Domyślnie klientów języka JavaScript dotyczą Hubs przy użyciu wersji formacie camelcase nazwy klasy. SignalR automatycznie sprawia, że ta zmiana tak, aby kod JavaScript można jest zgodna z konwencjami języka JavaScript. Poprzedni przykład może być traktowana jako `contosoChatHub` w kodzie JavaScript.

**Serwer**

[!code-csharp[Main](hubs-api-guide-server/samples/sample8.cs?highlight=1)]

**Klient JavaScript za pomocą wygenerowanego serwera proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample9.js?highlight=1)]

Jeśli chcesz określić inną nazwę dla klientów użyć, należy dodać `HubName` atrybutu. Kiedy używasz `HubName` atrybutu, nastąpiła żadna zmiana nazwy na format camelcase na klientów języka JavaScript.

**Serwer**

[!code-csharp[Main](hubs-api-guide-server/samples/sample10.cs?highlight=1)]

**Klient JavaScript za pomocą wygenerowanego serwera proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample11.js?highlight=1)]

<a id="multiplehubs"></a>

### <a name="multiple-hubs"></a>Wiele centrów

W aplikacji, można zdefiniować wiele klas koncentratora. Po wykonaniu tej czynności, połączenie jest udostępniane, ale są osobne grupy:

- Wszyscy klienci będą używać tego samego adresu URL do nawiązania połączenia SignalR z usługą ("/ signalr" lub adres URL niestandardowej, jeśli została określona), i czy połączenie jest używany przez wszystkie centra zdefiniowane przez usługę.

    Nie ma żadnej różnicy wydajności, dla wielu centrów w porównaniu do definiowania wszystkich funkcji w Centrum w jednej klasie.
- Wszystkie centra uzyskać te same informacje o żądaniu HTTP.

    Ponieważ wszystkie centra udostępnianie tego samego połączenia, tylko informacje żądania HTTP, który serwer jest, co składa się z oryginalnego żądania HTTP, który nawiązuje połączenia SignalR. Jeśli używasz żądanie połączenia do przekazywania informacji z klienta do serwera, określając ciąg zapytania nie może dostarczyć różne ciągi kwerend do różnych centrów. Wszystkie centra będą otrzymywać te same informacje.
- Wygenerowany plik serwery proxy JavaScript będzie zawierać serwerów proxy do wszystkich centrów w jednym pliku.

    Aby uzyskać informacje o serwery proxy JavaScript, zobacz [Podręcznik interfejsu API centrów SignalR — klient JavaScript — wygenerowany serwer proxy i przeznaczenie dla Ciebie](hubs-api-guide-javascript-client.md#genproxy).
- Grupy są definiowane w obrębie centrów.

    W SignalR, które można zdefiniować nazwę grupy do emisji przeznaczonych do podzestawów połączonych klientów. Grupy są przechowywane osobno dla każdego Centrum. Na przykład utworzyć grupę o nazwie "Administratorzy" zawierałoby jeden zbiór klientów do Twojej `ContosoChatHub` klasy i tej samej nazwy grupy może odwołać się do innego zestawu klientów dla Twojej `StockTickerHub` klasy.

<a id="stronglytypedhubs"></a>
### <a name="strongly-typed-hubs"></a>Koncentratory silnie Typizowane

Aby zdefiniować interfejs dla metod koncentratora, które mogą klienta odwołania i Włącz funkcję Intellisense z metod koncentratora, pochodzi z Centrum `Hub<T>` (zostanie wprowadzony w SignalR 2.1) zamiast `Hub`:

[!code-csharp[Main](hubs-api-guide-server/samples/sample12.cs)]

<a id="hubmethods"></a>

## <a name="how-to-define-methods-in-the-hub-class-that-clients-can-call"></a>Sposób definiowania metody w klasie Centrum, który można wywoływać klientów

Aby udostępnić metody koncentratora, który ma być wywoływane z klienta, należy zadeklarować metodę publiczną, jak pokazano w poniższych przykładach.

[!code-csharp[Main](hubs-api-guide-server/samples/sample13.cs?highlight=3)]

[!code-csharp[Main](hubs-api-guide-server/samples/sample14.cs?highlight=3)]

Można określić zwracany typ i parametry, w tym typy złożone i tablice, tak jak w dowolnej metody języka C#. Wszelkie dane, które wystąpią w parametrach lub zwróć się do obiektu wywołującego jest przekazywana między klientem a serwerem przy użyciu pliku JSON, a SignalR obsługuje wiązania złożone obiekty i tablice obiektów automatycznie.

<a id="methodnames"></a>

### <a name="camel-casing-of-method-names-in-javascript-clients"></a>Pisane-wielkość liter w wyrazie nazwy metod w klientów języka JavaScript

Domyślnie klientów JavaScript dotyczą metod koncentratora za pomocą wersji formacie camelcase nazwy metody. SignalR automatycznie sprawia, że ta zmiana tak, aby kod JavaScript można jest zgodna z konwencjami języka JavaScript.

**Serwer**

[!code-csharp[Main](hubs-api-guide-server/samples/sample15.cs?highlight=1)]

**Klient JavaScript za pomocą wygenerowanego serwera proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample16.js?highlight=1)]

Jeśli chcesz określić inną nazwę dla klientów użyć, należy dodać `HubMethodName` atrybutu.

**Serwer**

[!code-csharp[Main](hubs-api-guide-server/samples/sample17.cs?highlight=1)]

**Klient JavaScript za pomocą wygenerowanego serwera proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample18.js?highlight=1)]

<a id="asyncmethods"></a>

### <a name="when-to-execute-asynchronously"></a>Kiedy są wykonywane asynchronicznie

Jeśli metoda będzie mieć długotrwałych albo do pracy, będzie obejmować oczekiwania, takich jak wyszukiwania w bazie danych lub wywołanie usługi sieci web, upewnij metody koncentratora asynchronicznych, zwracając [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) (zamiast `void` zwracają) lub [ Zadanie&lt;T&gt; ](https://msdn.microsoft.com/library/dd321424.aspx) obiektu (zamiast `T` zwracany typ). Po powrocie `Task` oczekuje obiektu z metody SignalR `Task` do ukończenia, a następnie wysyła nieopakowane wynik do klienta, więc nie ma żadnej różnicy w jak kod jest wywołanie metody w kliencie.

Tworzenie metody koncentratora asynchronicznego pozwala uniknąć blokuje połączenia, gdy używa ona transportu protokołu WebSocket. Gdy metoda Centrum jest wykonywana synchronicznie i transport jest WebSocket, kolejnych wywołań metod koncentratora, z tego samego klienta są blokowane, dopiero po zakończeniu metody koncentratora.

W poniższym przykładzie pokazano taką samą metodę jak kodowane, aby były uruchamiane synchronicznie lub asynchronicznie, a następnie kod klienta JavaScript, który działa w przypadku wywoływania danej wersji.

**Synchroniczne**

[!code-csharp[Main](hubs-api-guide-server/samples/sample19.cs)]

**Asynchroniczne**

[!code-csharp[Main](hubs-api-guide-server/samples/sample20.cs?highlight=1,7-8)]

**Klient JavaScript za pomocą wygenerowanego serwera proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample21.js)]

Aby uzyskać więcej informacji o sposobie używania metod asynchronicznych w programie ASP.NET 4.5, zobacz [przy użyciu metod asynchronicznych w ASP.NET MVC 4](../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="overloads"></a>

### <a name="defining-overloads"></a>Definiowanie przeciążenia

Jeśli chcesz zdefiniować przeciążenia metody, liczba parametrów w każdym przeciążenia musi być różne. W przypadku przeciążenia różnicuje się tylko, określając typy różnych parametrów, zostanie skompilowana klasa Centrum, ale usługi SignalR spowoduje zgłoszenie wyjątku w czasie wykonywania, gdy klienci spróbują do wywołania, jednego z przeciążeń.

<a id="progress"></a>
### <a name="reporting-progress-from-hub-method-invocations"></a>Raport z postępów z wywołań metod koncentratora

SignalR 2.1 dodaje obsługę [wzorzec Raportowanie postępu](https://blogs.msdn.com/b/dotnet/archive/2012/06/06/async-in-4-5-enabling-progress-and-cancellation-in-async-apis.aspx) wprowadzone w .NET 4.5. Aby zaimplementować raportowania postępu, zdefiniuj `IProgress<T>` parametr dla Twojego metodę koncentratora klienta mogą uzyskiwać dostęp do:

[!code-csharp[Main](hubs-api-guide-server/samples/sample22.cs)]

Podczas pisania — metoda długotrwałych serwera, ważne jest, aby użyć wzorca programowania asynchronicznego, takich jak Async / Await zamiast blokowania wątku koncentratora.

<a id="callfromhub"></a>

## <a name="how-to-call-client-methods-from-the-hub-class"></a>Sposób wywoływania metody z klasy koncentratora klienta

Aby wywołać metody klienta, z serwera, należy użyć `Clients` właściwość w metodę w klasie koncentratora. W poniższym przykładzie pokazano kod serwera, który wywołuje `addNewMessageToPage` na wszystkich połączonych klientów i kod klienta, który definiuje metodę w kliencie języka JavaScript.

**Serwer**

[!code-csharp[Main](hubs-api-guide-server/samples/sample23.cs?highlight=5)]

**Klient JavaScript za pomocą wygenerowanego serwera proxy**

[!code-html[Main](hubs-api-guide-server/samples/sample24.html?highlight=1)]

Nie można uzyskać wartość zwracaną z metody klienta; Składnia takich jak `int x = Clients.All.add(1,1)` nie działa.

Można określić typy złożone i tablice parametrów. Poniższy przykład przekazuje typu złożonego do klienta w parametrze metody.

**Kod serwera, który wywołuje metodę klienta przy użyciu obiektu złożonego**

[!code-csharp[Main](hubs-api-guide-server/samples/sample25.cs?highlight=3)]

**Kod serwera, który definiuje obiekt złożony**

[!code-csharp[Main](hubs-api-guide-server/samples/sample26.cs?highlight=1)]

**Klient JavaScript za pomocą wygenerowanego serwera proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample27.js?highlight=2-3)]

<a id="selectingclients"></a>

### <a name="selecting-which-clients-will-receive-the-rpc"></a>Wybieranie klientów, którzy będą otrzymywać RPC

Zwraca właściwości klientów [HubConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubconnectioncontext(v=vs.111).aspx) obiektu, który zawiera kilka opcji umożliwiających określenie, którzy klienci otrzymają RPC:

- Wszyscy połączeni klienci.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample28.cs)]
- Tylko klienta wywołującego.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample29.cs)]
- Wszyscy klienci oprócz klienta wywołującego.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample30.cs)]
- Klientowi szczególne identyfikowane przez identyfikator połączenia.

    [!code-css[Main](hubs-api-guide-server/samples/sample31.css)]

    Ten przykład wywołuje `addContosoChatMessageToPage` na klienta wywołującego i ma ten sam efekt jak użycie `Clients.Caller`.
- Wszyscy połączeni klienci oprócz określonych klientów, identyfikowany przez identyfikator połączenia.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample32.cs)]
- Wszyscy połączeni klienci w określonej grupie.

    [!code-css[Main](hubs-api-guide-server/samples/sample33.css)]
- Wszyscy połączeni klienci, w określonej grupie oprócz określonych klientów, identyfikowany przez identyfikator połączenia.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample34.cs)]
- Wszyscy połączeni klienci, w określonej grupie oprócz klienta wywołującego.

    [!code-css[Main](hubs-api-guide-server/samples/sample35.css)]
- Określony użytkownik identyfikowany przez identyfikator użytkownika.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample36.cs)]

    Domyślnie jest to `IPrincipal.Identity.Name`, ale można to zmienić, [rejestrowanie implementację IUserIdProvider z hostem globalnego](mapping-users-to-connections.md#IUserIdProvider).
- Wszystkich klientów i grup na liście identyfikatorów połączeń.

    [!code-css[Main](hubs-api-guide-server/samples/sample37.css)]
- Lista grup.

    [!code-css[Main](hubs-api-guide-server/samples/sample38.css)]
- Użytkownika według nazwy.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample39.cs)]
- Lista nazw użytkowników (zostanie wprowadzony w SignalR 2.1).

    [!code-csharp[Main](hubs-api-guide-server/samples/sample40.cs)]

<a id="dynamicmethodnames"></a>

### <a name="no-compile-time-validation-for-method-names"></a>Nazwy metod weryfikacji kompilacji

Nazwa metody, które określisz, jest interpretowany jako obiekt dynamiczny, co oznacza, że brak funkcji IntelliSense i weryfikacji kompilacji. Wyrażenie jest obliczane w czasie wykonywania. Podczas wywołania metody które wykonuje, SignalR wysyła nazwę metody i wartości parametrów do klienta, a jeśli klient ma metodę, który pasuje do nazwy, że metoda jest wywoływana i wartości parametrów są przekazywane do niego. Jeśli odpowiadającej metody znajduje się na komputerze klienckim, zgłaszany jest błąd braku. Aby uzyskać informacje o formacie dane, które SignalR przesyła do klientów w tle po wywołaniu metody klienta, zobacz [wprowadzenie do SignalR](../getting-started/introduction-to-signalr.md).

<a id="caseinsensitive"></a>

### <a name="case-insensitive-method-name-matching"></a>Dopasowywanie nazw bez uwzględniania wielkości liter — metoda

Dopasowywaniu nazwy metody nie jest rozróżniana wielkość liter. Na przykład `Clients.All.addContosoChatMessageToPage` będą wykonywane na serwerze `AddContosoChatMessageToPage`, `addcontosochatmessagetopage`, lub `addContosoChatMessageToPage` na komputerze klienckim.

<a id="asyncclient"></a>

### <a name="asynchronous-execution"></a>Wykonanie asynchroniczne

Asynchronicznie wykonuje metodę, która wywołania. Wszelki kod, który jest dostarczany po wywołania metody, aby klient wykona się natychmiast bez oczekiwania na SignalR zakończyć przesyłania danych do klientów, chyba że określisz, że kolejne wiersze kodu powinna czekać na ukończenie metody. Poniższy przykład kodu pokazuje sposób wykonywania dwóch metod klienta po kolei.

**Za pomocą Await (.NET 4.5)**

[!code-csharp[Main](hubs-api-guide-server/samples/sample41.cs?highlight=1,3)]

Jeśli używasz `await` poczekać, aż metodę klienta zakończy się przed wykonaniem następnego wiersza kodu, który nie oznacza to, czy klienci będą otrzymywać komunikat faktycznie, przed wykonaniem następnego wiersza kodu. "Wykonanie" wywołania metody klienta oznacza jedynie, że SignalR zrobił wszystko, co jest niezbędne do wysyłania wiadomości. Weryfikacji, czy klienci się komunikat, należy mieć program ten mechanizm samodzielnie. Na przykład można kod `MessageReceived` metody koncentratora, w `addContosoChatMessageToPage` metody na kliencie, można wywołać `MessageReceived` po wykonaniu niezależnie od pracy należy wykonać na kliencie. W `MessageReceived` Centrum można wykonać pracę zależy od klienta faktycznie odbierania i przetwarzania wywołania metody oryginalnym.

### <a name="how-to-use-a-string-variable-as-the-method-name"></a>Jak używać zmiennej ciągu jako nazwy metody

Jeśli chcesz wywołać metodę klienta przy użyciu zmiennej ciągu jako nazwy metody rzutowania `Clients.All` (lub `Clients.Others`, `Clients.Caller`, itp.) do `IClientProxy` , a następnie wywołać [Invoke (methodName, argumenty...) ](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.iclientproxy.invoke(v=vs.111).aspx).

[!code-csharp[Main](hubs-api-guide-server/samples/sample42.cs)]

<a id="groupsfromhub"></a>

## <a name="how-to-manage-group-membership-from-the-hub-class"></a>Sposób zarządzania członkostwa w grupie z klasy koncentratora

Grupami w SignalR udostępnia metody emisji komunikatów określony podzbiór połączonych klientów. Grupa może zawierać dowolną liczbę klientów, a klient może należeć do dowolnej liczby grup.

Aby zarządzanie członkostwem w grupie, należy użyć [Dodaj](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) i [Usuń](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metod dostarczonych przez `Groups` właściwość klasy koncentratora. W poniższym przykładzie przedstawiono `Groups.Add` i `Groups.Remove` metody używane w metodach koncentratora, które są wywoływane przez kod klienta, a następnie kod klienta JavaScript, która je wywołuje.

**Serwer**

[!code-csharp[Main](hubs-api-guide-server/samples/sample43.cs?highlight=5,10)]

**Klient JavaScript za pomocą wygenerowanego serwera proxy**

[!code-javascript[Main](hubs-api-guide-server/samples/sample44.js)]

[!code-javascript[Main](hubs-api-guide-server/samples/sample45.js)]

Nie trzeba jawnie tworzyć grupy. Obowiązuje grupy jest automatycznie tworzony po raz pierwszy należy określić jego nazwę w wywołaniu `Groups.Add`, i zostanie usunięta po usunięciu ostatniego połączenia z członkostwa w nim.

Nie ma żadnych interfejsów API w celu uzyskania listy członkostwa grupy lub Podaj listę grup. SignalR wysyła wiadomości do klientów i grup na podstawie [model pub/sub](http://en.wikipedia.org/wiki/Publish/subscribe), a serwer nie przechowuje listę grup lub członkostwa w grupach. Pozwala to zmaksymalizować skalowalność, ponieważ po każdym dodaniu węzła w farmie sieci web, stan, który przechowuje SignalR są propagowane do nowego węzła.

<a id="asyncgroupmethods"></a>

### <a name="asynchronous-execution-of-add-and-remove-methods"></a>Asynchroniczne wykonywanie metody dodawania i usuwania

`Groups.Add` i `Groups.Remove` metody są wykonywane asynchronicznie. Jeśli chcesz dodać do grupy klienta i natychmiast wysłać wiadomość do klienta przy użyciu grupy, należy upewnić się, że `Groups.Add` metoda zakończy się pierwsza. Poniższy przykład kodu pokazuje, jak to zrobić.

**Dodawanie klienta do grupy i następnie komunikatów klienta**

[!code-csharp[Main](hubs-api-guide-server/samples/sample46.cs?highlight=1,3)]

<a id="grouppersistence"></a>

### <a name="group-membership-persistence"></a>Trwałość członkostwa grupy

SignalR śledzi połączenia, nie dla użytkowników, więc jeśli użytkownik ma znajdować się w tej samej grupie za każdym razem, gdy użytkownik nawiązuje połączenie, musisz wywołać `Groups.Add` za każdym razem, gdy użytkownik nawiązuje nowe połączenie.

Po do tymczasowej utraty łączności czasami biblioteki SignalR można przywrócić połączenia automatycznie. W takim przypadku SignalR jest przywrócenie tego samego połączenia, nie ustanawiania nowego połączenia, a więc członkostwa w grupie klienta zostanie automatycznie przywrócony. Jest to możliwe nawet, jeśli tymczasowe przerwanie jest wynikiem ponownego rozruchu serwera lub awarii, ponieważ stan połączenia dla każdego klienta, w tym członkostwa w grupach jest zwrotnego do klienta. Jeśli serwer ulegnie awarii, został zastąpiony nowym serwerze, zanim upłynie limit czasu połączenia klienta można automatycznie ponownie połączenie z nowym serwerem i ponownego rejestrowania się w grupach, które jest członkiem.

Jeśli połączenie nie będzie można przywrócić automatycznie po utracie łączności, lub gdy upłynie limit czasu połączenia lub gdy klient odłączy się (na przykład, gdy przeglądarka wraca do nowej strony), członkostwa w grupie zostaną utracone. Następnym razem, gdy użytkownik nawiąże połączenie będzie nowe połączenie. Aby zachować członkostwa w grupach, gdy ten sam użytkownik nawiązuje nowe połączenie, aplikacja musi śledzić skojarzenia między użytkownikami i grupami w celu przywrócenia członkostwa w grupach za każdym razem użytkownik ustanawia nowego połączenia.

Aby uzyskać więcej informacji na temat połączeń i ponowne łączenie, zobacz [sposób obsługi zdarzenia okresu istnienia połączenia w klasie Centrum](#connectionlifetime) w dalszej części tego tematu.

<a id="singleusergroups"></a>

### <a name="single-user-groups"></a>Grupy pojedynczego użytkownika

Aplikacje, które zwykle używanie biblioteki SignalR mieć do śledzenia skojarzenia między użytkownikami i połączeń, aby dowiedzieć się, użytkownika, który wysłał wiadomość, a które użytkownicy powinni otrzymywać wiadomości. Grupy są używane w jednym z dwóch często używane wzorce dla tych czynności.

- Grupy pojedynczego użytkownika.

    Określ nazwę użytkownika jako nazwę grupy i dodać do grupy bieżący identyfikator połączenia, za każdym razem, gdy użytkownik nawiązuje połączenie, lub ponownie nawiąże połączenie. Aby wysyłać komunikaty do użytkownika Wyślij do grupy. Wadą tej metody jest to, że grupa nie zapewniają sposób, aby sprawdzić, czy użytkownik jest w trybie online lub offline.
- Śledź skojarzenia między nazwami użytkowników i identyfikatorów połączeń.

    Przechowywanie skojarzenie między każdej nazwy użytkownika i jeden lub więcej identyfikatorów połączeń słownika lub bazy danych i zaktualizować przechowywanych danych za każdym razem, użytkownik połączeniu lub rozłączeniu. Aby wysyłać komunikaty do użytkownika, należy określić identyfikatorów połączeń. Wadą tej metody jest to, że zajmuje więcej pamięci.

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events-in-the-hub-class"></a>Jak obsługiwać zdarzenia okresu istnienia połączenia w klasie Centrum

Typowe przyczyny Obsługa zdarzeń okresu istnienia połączenia są do śledzenia, czy użytkownik jest połączony, czy nie, a także do śledzenia skojarzenie między nazwy użytkowników i identyfikatorów połączeń. Aby uruchomić własny kod, gdy klienci łączyć i rozłączać, Zastąp `OnConnected`, `OnDisconnected`, i `OnReconnected` metod wirtualnych Centrum klasy, jak pokazano w poniższym przykładzie.

[!code-csharp[Main](hubs-api-guide-server/samples/sample47.cs?highlight=3,14,22)]

<a id="onreconnected"></a>

### <a name="when-onconnected-ondisconnected-and-onreconnected-are-called"></a>Kiedy są wywoływane onconnected elementu ondisconnected elementu i onreconnected elementu

Każdorazowo, przeglądarka wraca do nowej strony, nowe połączenie ma zostać nawiązane, co oznacza, że zostanie wykonana SignalR `OnDisconnected` metody następuje `OnConnected` metody. Po nawiązaniu nowego połączenia SignalR zawsze tworzy nowy identyfikator połączenia.

`OnReconnected` Metoda jest wywoływana, gdy nastąpiła tymczasowe przerwanie w łączności, SignalR automatyczne odzyskiwanie, takie jak kiedy kabla jest tymczasowo odłączony i ponowne łączenie zakończone przed upływem limitu czasu połączenia. `OnDisconnected` Metoda jest wywoływana, gdy klient został odłączony SignalR nie automatycznie ponownie połączyć, np. gdy przeglądarka wraca do nowej strony. Dlatego jest możliwe sekwencji zdarzeń dla danego klienta `OnConnected`, `OnReconnected`, `OnDisconnected`; lub `OnConnected`, `OnDisconnected`. Nie będziesz widzieć sekwencji `OnConnected`, `OnDisconnected`, `OnReconnected` dla danego połączenia.

`OnDisconnected` Metoda nie jest wywoływana w niektórych scenariuszach, na przykład jeśli serwer ulegnie awarii lub domena aplikacji zostanie odtworzona. Gdy inny serwer, który jest dostarczany w wierszu lub domena aplikacji kończy jego odtwarzanie, niektórzy klienci można ponownie połączyć i szybko `OnReconnected` zdarzeń.

Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługa zdarzeń okresu istnienia połączenia w SignalR](handling-connection-lifetime-events.md).

<a id="nocallerstate"></a>

### <a name="caller-state-not-populated"></a>Stan elementu wywołującego, pusta

Metody obsługi zdarzeń okresu istnienia połączenia są nazywane z serwera, co oznacza, że stan, aby umieścić w `state` obiektu na kliencie nie zostanie wypełniony w `Caller` właściwości na serwerze. Aby uzyskać informacje o `state` obiektu i `Caller` właściwości, zobacz [sposób przekazywania stanu między klientami a klasy koncentratora](#passstate) w dalszej części tego tematu.

<a id="contextproperty"></a>

## <a name="how-to-get-information-about-the-client-from-the-context-property"></a>Jak uzyskać informacji o kliencie z właściwości kontekstu

Aby uzyskać informacje dotyczące klienta, użyj `Context` właściwość klasy koncentratora. `Context` Właściwość zwraca [HubCallerContext](https://msdn.microsoft.com/library/jj890883(v=vs.111).aspx) obiektu, który zapewnia dostęp do następujących informacji:

- Identyfikator połączenia klienta wywołującego.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample48.cs?highlight=1)]

    Identyfikator połączenia, który jest identyfikatorem GUID, który jest przypisywany przez SignalR (we własnym kodzie nie można określić wartości). Istnieje jeden identyfikator połączenia dla poszczególnych połączeń i tego samego połączenia, którego identyfikator jest używany przez wszystkie centra, jeśli masz wiele centrów w aplikacji.
- Dane nagłówka HTTP.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample49.cs?highlight=1)]

    Możesz też pobrać nagłówki HTTP od `Context.Headers`. Przyczyna wiele odwołań do tak samo jest fakt, że `Context.Headers` utworzono najpierw `Context.Request` właściwość została dodana później i `Context.Headers` został zachowany na potrzeby zgodności z poprzednimi wersjami.
- Wykonywanie zapytań dotyczących danych ciągu.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample50.cs?highlight=1)]

    Możesz też pobrać dane ciągu zapytania z `Context.QueryString`.

    Ciąg zapytania, które otrzymujesz w tej właściwości jest tą, która została użyta z żądania HTTP, które nawiązaniu połączenia SignalR. Aby dodać parametry ciągu zapytania w kliencie, konfigurowanie połączenia z to wygodny sposób przekazywania danych dotyczących klienta z klienta do serwera. Poniższy przykład przedstawia sposób dodać ciąg zapytania w kliencie JavaScript, korzystając z wygenerowanego serwera proxy.

    [!code-javascript[Main](hubs-api-guide-server/samples/sample51.js?highlight=1)]

    Aby uzyskać więcej informacji na temat ustawiania parametrów ciągu zapytania, zapoznaj się z przewodnikami interfejsu API, aby uzyskać [JavaScript](hubs-api-guide-javascript-client.md) i [.NET](hubs-api-guide-net-client.md) klientów.

    Można znaleźć metody transportu używane dla połączeń w dane ciągu zapytania wraz z innymi wartościami, używane wewnętrznie przez element SignalR:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample52.cs)]

    Wartość `transportMethod` będzie "webSockets", "serverSentEvents", "foreverFrame" lub "longPolling". Należy pamiętać, że jeśli zaznaczysz tę wartość `OnConnected` metody obsługi zdarzeń, w niektórych scenariuszach może być początkowo uzyskać wartość transportu, która nie jest to metoda końcowego wynegocjowanym transport dla połączenia. W tym przypadku metoda spowoduje zgłoszenie wyjątku i zostanie wywołana ponownie później po nawiązaniu metoda końcowego transportu.
- Pliki cookie.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample53.cs?highlight=1)]

    Możesz też pobrać pliki cookie z `Context.RequestCookies`.
- Informacje o użytkowniku.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample54.cs?highlight=1)]
- Obiekt kontekstu HTTP dla żądania:

    [!code-csharp[Main](hubs-api-guide-server/samples/sample55.cs?highlight=1)]

    Użyj tej metody, zamiast `HttpContext.Current` można pobrać `HttpContext` obiektu dla połączenia SignalR.

<a id="passstate"></a>

## <a name="how-to-pass-state-between-clients-and-the-hub-class"></a>Jak przekazać stanu między klientami a klasy koncentratora

Udostępnia serwer proxy klienta `state` obiekt, w którym można przechowywać dane, które mają być przekazywane do serwera z każdego wywołania metody. Na serwerze mają dostęp do danych, to w `Clients.Caller` właściwości w metodach koncentratora, które są wywoływane przez klientów. `Clients.Caller` Właściwość jest pusta dla metody obsługi zdarzeń okresu istnienia połączenia `OnConnected`, `OnDisconnected`, i `OnReconnected`.

Tworzenie lub aktualizowanie danych w `state` obiektu i `Clients.Caller` właściwości działa w obu kierunkach. Można zaktualizować wartości na serwerze i są one przekazywane do klienta.

Poniższy przykład pokazuje kod klienta JavaScript, który zapisuje stan w celu przesłania go do serwera z każdego wywołania metody.

[!code-javascript[Main](hubs-api-guide-server/samples/sample56.js?highlight=1-2)]

Poniższy przykład przedstawia równoważny kod w klienta platformy .NET.

[!code-csharp[Main](hubs-api-guide-server/samples/sample57.cs?highlight=1-2)]

W klasie Centrum uzyskujesz dostęp do tych danych w `Clients.Caller` właściwości. Poniższy przykład pokazuje kod, który pobiera stan określonego w poprzednim przykładzie.

[!code-csharp[Main](hubs-api-guide-server/samples/sample58.cs?highlight=3-4)]

> [!NOTE]
> Ten mechanizm utrwalanie stanu nie jest przeznaczony dla dużych ilości danych, ponieważ wszystko, co umieścić w `state` lub `Clients.Caller` właściwość jest zwrotnego z każdego wywołania metody. Jest to przydatne dla mniejszych elementów, takich jak nazwy użytkowników lub liczniki.


W VB.NET lub koncentrator silnie typizowane, obiekt wywołujący stanu nie są dostępne za pośrednictwem `Clients.Caller`; zamiast tego użyj `Clients.CallerState` (zostanie wprowadzony w SignalR 2.1):

**Za pomocą CallerState w języku C#**

[!code-csharp[Main](hubs-api-guide-server/samples/sample59.cs?highlight=3-4)]

**Za pomocą CallerState w języku Visual Basic**

[!code-vb[Main](hubs-api-guide-server/samples/sample60.vb)]

<a id="handleErrors"></a>

## <a name="how-to-handle-errors-in-the-hub-class"></a>Sposób obsługi błędów w klasie Centrum

Do obsługi błędów występujących w Twoich metodach koncentratora, klasy, należy użyć co najmniej jeden z następujących metod:

- Zabalit kodzie metoda bloków try-catch, a następnie zaloguj się obiekt wyjątku. Dla celów debugowania wyjątku można wysłać do klienta, ale zabezpieczeń przyczyny wysyłanie szczegółowych informacji do klientów w środowisku produkcyjnym nie jest zalecane.
- Utwórz moduł potoku koncentratorów, który obsługuje [OnIncomingError](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.hubpipelinemodule.onincomingerror(v=vs.111).aspx) metody. Poniższy przykład pokazuje moduł potoku, który rejestruje błędy, kod w pliku Startup.cs, która wprowadza moduł potokiem koncentratorów.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample61.cs)]

    [!code-csharp[Main](hubs-api-guide-server/samples/sample62.cs?highlight=4)]
- Użyj `HubException` klasy (zostanie wprowadzony w SignalR 2). Ten błąd może być wyrzucanych z jakiegokolwiek wywołania koncentratora. `HubError` Konstruktor przyjmuje komunikat w formacie ciągu i obiekt do przechowywania dodatkowe dane dotyczące błędu. SignalR spowoduje automatyczne serializować wyjątku i wysyłać je do klienta, w którym będzie służyć do odrzucania lub Niepowodzenie wywołania metody koncentratora.

    Poniższe przykłady kodu przedstawiają sposób throw `HubException` podczas wywołania koncentratora i sposób obsługi wyjątków na komputerach klienckich JavaScript i .NET.

    **Kod serwera ukazujące klasy HubException**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample63.cs)]

    **Kod klienta JavaScript ukazujące odpowiedź do zgłaszania HubException w z koncentratorem**

    [!code-html[Main](hubs-api-guide-server/samples/sample64.html)]

    **Prezentacja odpowiedź do zgłaszania HubException w z koncentratorem kod klienta .NET**

    [!code-csharp[Main](hubs-api-guide-server/samples/sample65.cs)]

Aby uzyskać więcej informacji na temat modułów potoku koncentratora, zobacz [sposobu dostosowywania potoku koncentratory](#hubpipeline) w dalszej części tego tematu.

<a id="tracing"></a>

## <a name="how-to-enable-tracing"></a>Jak włączyć śledzenie

Aby włączyć śledzenie po stronie serwera, należy dodać system.diagnostics element do pliku Web.config, jak pokazano w poniższym przykładzie:

[!code-html[Main](hubs-api-guide-server/samples/sample66.html?highlight=17-72)]

Po uruchomieniu aplikacji w programie Visual Studio możesz wyświetlić dzienniki w **dane wyjściowe** okna.

<a id="callfromoutsidehub"></a>

## <a name="how-to-call-client-methods-and-manage-groups-from-outside-the-hub-class"></a>Jak wywołać metody klienta grup i zarządzanie nimi z poza klasy koncentratora

Do wywołania metody klienta, z innej klasy niż klasa koncentratora, Pobierz odwołanie do obiektu context SignalR dla koncentratora i używać go wywoływać metody na kliencie lub zarządzać grupami.

Poniższy przykład `StockTicker` klasy pobiera obiekt kontekstu, zapisuje go w wystąpieniu klasy, przechowuje wystąpienia klasy statycznej właściwości i używa kontekstu z pojedyncze wystąpienie klasy w celu wywołania `updateStockPrice` metody na komputerach klienckich, które są podłączone do koncentratora o nazwie `StockTickerHub`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample67.cs?highlight=8,24)]

Jeśli musisz używać kontekstu wielu i godziny w obiekcie długotrwałe raz Uzyskaj odwołanie i zapisz go, a nie wprowadzenie go ponownie za każdym razem. Pobieranie kontekstu po zapewnia SignalR i wysyła wiadomości do klientów w tej samej kolejności, w której metody koncentratora wprowadzić klienta wywołania metody. Aby uzyskać samouczek, który pokazuje, jak używać kontekstu SignalR dla koncentratora, zobacz [emisje serwera z użyciem ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md).

<a id="callingclientsoutsidehub"></a>

### <a name="calling-client-methods"></a>Wywoływanie metody klienta

Można określić, którzy klienci otrzymają RPC, ale ma mniej opcji niż pod numerem klasy koncentratora. Przyczyną jest to, że kontekst nie jest skojarzony z określonym wywołania klienta, więc żadnych metod wymagających znajomości bieżący identyfikator połączenia, takie jak `Clients.Others`, lub `Clients.Caller`, lub `Clients.OthersInGroup`, nie są dostępne. Dostępne są następujące opcje:

- Wszyscy połączeni klienci.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample68.cs)]
- Klientowi szczególne identyfikowane przez identyfikator połączenia.

    [!code-css[Main](hubs-api-guide-server/samples/sample69.css)]
- Wszyscy połączeni klienci oprócz określonych klientów, identyfikowany przez identyfikator połączenia.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample70.cs)]
- Wszyscy połączeni klienci w określonej grupie.

    [!code-css[Main](hubs-api-guide-server/samples/sample71.css)]
- Wszystkich połączonych klientów w określonej grupie oprócz określonych klientów, identyfikowany przez identyfikator połączenia.

    [!code-csharp[Main](hubs-api-guide-server/samples/sample72.cs)]

W przypadku wywołania do klasy-Hub metody w klasie koncentratora, możesz przekazać identyfikator bieżącego połączenia i używać go z `Clients.Client`, `Clients.AllExcept`, lub `Clients.Group` do symulacji `Clients.Caller`, `Clients.Others`, lub `Clients.OthersInGroup`. W poniższym przykładzie `MoveShapeHub` klasy przekazuje identyfikator połączenia, aby `Broadcaster` klasy tak, aby `Broadcaster` klasy można symulować `Clients.Others`.

[!code-csharp[Main](hubs-api-guide-server/samples/sample73.cs?highlight=12,36)]

<a id="managinggroupsoutsidehub"></a>

### <a name="managing-group-membership"></a>Członkostwo w grupie zarządzania

Do zarządzania grupami mają te same opcje tak jak w klasie koncentratora.

- Dodawanie klienta do grupy

    [!code-csharp[Main](hubs-api-guide-server/samples/sample74.cs)]
- Usunięcie klienta z grupy

    [!code-css[Main](hubs-api-guide-server/samples/sample75.css)]

<a id="hubpipeline"></a>

## <a name="how-to-customize-the-hubs-pipeline"></a>Jak dostosować potoku koncentratory

Biblioteki SignalR można wstawić własny kod do potoku koncentratora. Poniższy przykład przedstawia niestandardowego modułu potoku koncentratora, który rejestruje każdego przychodzące wywołania metody, odebrane od klienta i wychodzące wywołanie metody wywoływane na komputerze klienckim:

[!code-csharp[Main](hubs-api-guide-server/samples/sample76.cs)]

Poniższy kod w *Startup.cs* pliku rejestruje moduł do działania w potoku koncentratora:

[!code-csharp[Main](hubs-api-guide-server/samples/sample77.cs?highlight=3)]

Istnieje wiele różnych metod, które można przesłonić. Aby uzyskać pełną listę, zobacz [metody HubPipelineModule](https://msdn.microsoft.com/library/jj918633(v=vs.111).aspx).
