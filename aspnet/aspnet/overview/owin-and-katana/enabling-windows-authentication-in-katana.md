---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: "Włączanie uwierzytelniania systemu Windows w kolekcji Katana | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "W tym artykule pokazano, jak włączyć uwierzytelnianie systemu Windows w kolekcji Katana. Obejmuje dwa scenariusze: za pomocą usług IIS do hosta Katana i hosta samodzielnego Kat za pomocą HttpListener..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 8a26d356f7abafba021199761f9a49dcb81765c5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="a1f64-104">Włączanie uwierzytelniania systemu Windows w kolekcji Katana</span><span class="sxs-lookup"><span data-stu-id="a1f64-104">Enabling Windows Authentication in Katana</span></span>
====================
<span data-ttu-id="a1f64-105">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a1f64-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="a1f64-106">W tym artykule pokazano, jak włączyć uwierzytelnianie systemu Windows w kolekcji Katana.</span><span class="sxs-lookup"><span data-stu-id="a1f64-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="a1f64-107">Obejmuje dwa scenariusze: za pomocą usług IIS do hosta Katana i za pomocą HttpListener hosta samodzielnego Katana niestandardowy proces.</span><span class="sxs-lookup"><span data-stu-id="a1f64-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="a1f64-108">Dzięki użyciu Dorrans Marcin Matson Dominik i Krzysztof Roaming weryfikacji w tym artykule.</span><span class="sxs-lookup"><span data-stu-id="a1f64-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>


<span data-ttu-id="a1f64-109">Katana to implementacja firmy Microsoft [OWIN](http://owin.org/), Otwórz interfejs sieci Web dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="a1f64-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="a1f64-110">Wprowadzenie do OWIN i Katana może odczytywać [tutaj](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="a1f64-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="a1f64-111">Architektura OWIN zawiera kilka warstw:</span><span class="sxs-lookup"><span data-stu-id="a1f64-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="a1f64-112">Host: Zarządza procesem, w której jest uruchamiana potoku OWIN.</span><span class="sxs-lookup"><span data-stu-id="a1f64-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="a1f64-113">Serwer: Otwiera gniazda sieci i nasłuchuje żądań.</span><span class="sxs-lookup"><span data-stu-id="a1f64-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="a1f64-114">Oprogramowanie pośredniczące: Przetwarza żądania HTTP i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="a1f64-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="a1f64-115">Katana obecnie zawiera dwa serwery, które obsługuje zintegrowane uwierzytelnianie systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="a1f64-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="a1f64-116">**Microsoft.Owin.Host.SystemWeb**.</span><span class="sxs-lookup"><span data-stu-id="a1f64-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="a1f64-117">Korzysta z usług IIS z potoku ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a1f64-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="a1f64-118">**Microsoft.Owin.Host.HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="a1f64-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="a1f64-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="a1f64-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="a1f64-120">Ten serwer jest obecnie opcją domyślną, gdy hostingu samodzielnego Katana.</span><span class="sxs-lookup"><span data-stu-id="a1f64-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="a1f64-121">Katana nie jest aktualnie dostępny oprogramowanie pośredniczące OWIN do uwierzytelniania systemu Windows, ponieważ ta funkcja jest już dostępny na serwerach.</span><span class="sxs-lookup"><span data-stu-id="a1f64-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>


## <a name="windows-authentication-in-iis"></a><span data-ttu-id="a1f64-122">Uwierzytelnianie systemu Windows w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="a1f64-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="a1f64-123">Używając Microsoft.Owin.Host.SystemWeb, może po prostu włącz uwierzytelnianie systemu Windows w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="a1f64-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="a1f64-124">Zacznijmy od utworzenia nowej aplikacji ASP.NET przy użyciu szablonu projektu "Aplikacja sieci Web ASP.NET pusty".</span><span class="sxs-lookup"><span data-stu-id="a1f64-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="a1f64-125">Następnie dodaj pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="a1f64-125">Next, add NuGet packages.</span></span> <span data-ttu-id="a1f64-126">Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="a1f64-126">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="a1f64-127">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="a1f64-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="a1f64-128">Teraz Dodaj klasę o nazwie `Startup` z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a1f64-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="a1f64-129">To wszystko, musisz utworzyć aplikację "Hello world" dla OWIN działającą na serwerze IIS.</span><span class="sxs-lookup"><span data-stu-id="a1f64-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="a1f64-130">Naciśnij klawisz F5, aby debugować aplikację.</span><span class="sxs-lookup"><span data-stu-id="a1f64-130">Press F5 to debug the application.</span></span> <span data-ttu-id="a1f64-131">Powinny pojawić się "Witaj świecie!"</span><span class="sxs-lookup"><span data-stu-id="a1f64-131">You should see "Hello World!"</span></span> <span data-ttu-id="a1f64-132">w oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a1f64-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="a1f64-133">Następnie firma Microsoft będzie włączyć uwierzytelnianie systemu Windows w usługach IIS Express.</span><span class="sxs-lookup"><span data-stu-id="a1f64-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="a1f64-134">Z **widoku** menu, wybierz opcję **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="a1f64-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="a1f64-135">Kliknij nazwę projektu w Eksploratorze rozwiązań, aby wyświetlić właściwości projektu.</span><span class="sxs-lookup"><span data-stu-id="a1f64-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="a1f64-136">W **właściwości** ustaw **uwierzytelnianie anonimowe** do **wyłączone** i ustaw **uwierzytelniania systemu Windows** do  **Włączone**.</span><span class="sxs-lookup"><span data-stu-id="a1f64-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="a1f64-137">Po uruchomieniu aplikacji w programie Visual Studio, usługi IIS Express wymaga poświadczeń użytkownika systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="a1f64-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="a1f64-138">Zobacz ten przy użyciu [Fiddler](http://fiddler2.com/home) lub innego protokołu HTTP, narzędzia debugowania.</span><span class="sxs-lookup"><span data-stu-id="a1f64-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="a1f64-139">Oto przykład odpowiedzi HTTP:</span><span class="sxs-lookup"><span data-stu-id="a1f64-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="a1f64-140">Nagłówki WWW-Authenticate, w tej odpowiedzi wskazują, że serwer obsługuje [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protokołu, który korzysta z protokołu Kerberos lub NTLM.</span><span class="sxs-lookup"><span data-stu-id="a1f64-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="a1f64-141">Później, podczas wdrażania aplikacji na serwerze, wykonaj [te kroki](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) Aby włączyć uwierzytelnianie systemu Windows w usługach IIS na tym serwerze.</span><span class="sxs-lookup"><span data-stu-id="a1f64-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="a1f64-142">Uwierzytelnianie systemu Windows w HttpListener</span><span class="sxs-lookup"><span data-stu-id="a1f64-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="a1f64-143">Jeśli używasz Microsoft.Owin.Host.HttpListener do hosta samodzielnego Katana, możesz je włączyć uwierzytelnianie systemu Windows bezpośrednio na **HttpListener** wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="a1f64-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="a1f64-144">Najpierw utwórz nową aplikację konsoli.</span><span class="sxs-lookup"><span data-stu-id="a1f64-144">First, create a new console application.</span></span> <span data-ttu-id="a1f64-145">Następnie dodaj pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="a1f64-145">Next, add NuGet packages.</span></span> <span data-ttu-id="a1f64-146">Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="a1f64-146">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="a1f64-147">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="a1f64-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="a1f64-148">Teraz Dodaj klasę o nazwie `Startup` z następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="a1f64-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="a1f64-149">Ta klasa implementuje w tym samym przykładzie "Hello world" z przed, ale także ustawia uwierzytelniania systemu Windows jako schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="a1f64-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="a1f64-150">Wewnątrz `Main` funkcji, Rozpocznij potoku OWIN:</span><span class="sxs-lookup"><span data-stu-id="a1f64-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="a1f64-151">W narzędziu Fiddler, aby upewnić się, że aplikacja używa uwierzytelniania systemu Windows można wysyłać żądania:</span><span class="sxs-lookup"><span data-stu-id="a1f64-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="a1f64-152">Tematy pokrewne</span><span class="sxs-lookup"><span data-stu-id="a1f64-152">Related Topics</span></span>

[<span data-ttu-id="a1f64-153">Omówienie projektu Katana</span><span class="sxs-lookup"><span data-stu-id="a1f64-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="a1f64-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="a1f64-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="a1f64-155">Opis uwierzytelniania formularzy OWIN w nazwie wzorca MVC 5</span><span class="sxs-lookup"><span data-stu-id="a1f64-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
