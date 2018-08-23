---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Włączanie uwierzytelniania Windows w projekcie Katana | Dokumentacja firmy Microsoft
author: MikeWasson
description: 'W tym artykule przedstawiono sposób włączania uwierzytelniania Windows w projekcie Katana. Poruszono w nim dwa scenariusze: Korzystanie z usług IIS do hosta Katana i za pomocą HttpListener na potrzeby samodzielnego hostowania Kat...'
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 0aa578020a1f02fa68c74e758014c642219b4265
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756700"
---
<a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="70e98-104">Włączanie uwierzytelniania Windows w projekcie Katana</span><span class="sxs-lookup"><span data-stu-id="70e98-104">Enabling Windows Authentication in Katana</span></span>
====================
<span data-ttu-id="70e98-105">przez [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="70e98-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="70e98-106">W tym artykule przedstawiono sposób włączania uwierzytelniania Windows w projekcie Katana.</span><span class="sxs-lookup"><span data-stu-id="70e98-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="70e98-107">Poruszono w nim dwa scenariusze: Korzystanie z usług IIS do hosta Katana i za pomocą HttpListener na potrzeby samodzielnego hostowania Katana w procesie niestandardowym.</span><span class="sxs-lookup"><span data-stu-id="70e98-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="70e98-108">Dziękujemy za Barry Dorrans David Matson i Chris Ross przeglądania w tym artykule.</span><span class="sxs-lookup"><span data-stu-id="70e98-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>


<span data-ttu-id="70e98-109">Katana to implementacja firmy Microsoft [OWIN](http://owin.org/), Open Web Interface for .NET.</span><span class="sxs-lookup"><span data-stu-id="70e98-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="70e98-110">Możesz przeczytać wprowadzenie do OWIN i Katana [tutaj](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="70e98-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="70e98-111">Architektura OWIN zawiera kilka warstw:</span><span class="sxs-lookup"><span data-stu-id="70e98-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="70e98-112">Host: Zarządza proces, w którym jest uruchamiane w potoku OWIN.</span><span class="sxs-lookup"><span data-stu-id="70e98-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="70e98-113">Serwer: Otwiera gniazda sieciowego i nasłuchuje żądań.</span><span class="sxs-lookup"><span data-stu-id="70e98-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="70e98-114">Oprogramowanie pośredniczące: Przetwarza żądania HTTP i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="70e98-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="70e98-115">Katana aktualnie obsługuje dwa serwery, które obsługuje zintegrowane uwierzytelnianie Windows:</span><span class="sxs-lookup"><span data-stu-id="70e98-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="70e98-116">**Microsoft.Owin.Host.SystemWeb**.</span><span class="sxs-lookup"><span data-stu-id="70e98-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="70e98-117">Używa usług IIS przy użyciu potoku platformy ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="70e98-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="70e98-118">**Microsoft.Owin.Host.HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="70e98-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="70e98-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="70e98-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="70e98-120">Ten serwer obecnie jest to opcja domyślna, gdy hostingu samodzielnego Katana.</span><span class="sxs-lookup"><span data-stu-id="70e98-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="70e98-121">Katana nie jest aktualnie dostępny oprogramowanie pośredniczące OWIN do uwierzytelniania Windows, ponieważ ta funkcja jest już dostępne na serwerach.</span><span class="sxs-lookup"><span data-stu-id="70e98-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>


## <a name="windows-authentication-in-iis"></a><span data-ttu-id="70e98-122">Windows uwierzytelniania w usługach IIS</span><span class="sxs-lookup"><span data-stu-id="70e98-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="70e98-123">Używając Microsoft.Owin.Host.SystemWeb, można po prostu włącz uwierzytelnianie Windows w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="70e98-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="70e98-124">Zacznijmy od utworzenia nowej aplikacji platformy ASP.NET przy użyciu szablonu projektu "Aplikacja sieci Web ASP.NET pusty".</span><span class="sxs-lookup"><span data-stu-id="70e98-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="70e98-125">Następnie dodaj pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="70e98-125">Next, add NuGet packages.</span></span> <span data-ttu-id="70e98-126">Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="70e98-126">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="70e98-127">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="70e98-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="70e98-128">Teraz Dodaj klasę o nazwie `Startup` następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="70e98-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="70e98-129">To wszystko, czego potrzebujesz do tworzenia aplikacji "Hello world" dla OWIN, uruchomionych w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="70e98-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="70e98-130">Naciśnij klawisz F5, aby debugować aplikację.</span><span class="sxs-lookup"><span data-stu-id="70e98-130">Press F5 to debug the application.</span></span> <span data-ttu-id="70e98-131">Powinien zostać wyświetlony "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="70e98-131">You should see "Hello World!"</span></span> <span data-ttu-id="70e98-132">w oknie przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="70e98-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="70e98-133">Następnie firma Microsoft będzie włączyć uwierzytelnianie Windows w usługach IIS Express.</span><span class="sxs-lookup"><span data-stu-id="70e98-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="70e98-134">Z **widoku** menu, wybierz opcję **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="70e98-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="70e98-135">Kliknij nazwę projektu w Eksploratorze rozwiązań, aby wyświetlić właściwości projektu.</span><span class="sxs-lookup"><span data-stu-id="70e98-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="70e98-136">W **właściwości** oknie **uwierzytelnianie anonimowe** do **wyłączone** i ustaw **uwierzytelniania Windows** do  **Włączone**.</span><span class="sxs-lookup"><span data-stu-id="70e98-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="70e98-137">Po uruchomieniu aplikacji w programie Visual Studio, usług IIS Express będzie wymagać poświadczeń Windows.</span><span class="sxs-lookup"><span data-stu-id="70e98-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="70e98-138">Widać to przy użyciu [Fiddler](http://fiddler2.com/home) lub innego protokołu HTTP, narzędzia debugowania.</span><span class="sxs-lookup"><span data-stu-id="70e98-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="70e98-139">Poniżej przedstawiono przykład odpowiedzi HTTP:</span><span class="sxs-lookup"><span data-stu-id="70e98-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="70e98-140">Nagłówki WWW-Authenticate, w tym odpowiedzi wskazują, że serwer obsługuje [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protokołu, który korzysta z protokołu Kerberos lub NTLM.</span><span class="sxs-lookup"><span data-stu-id="70e98-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="70e98-141">Później, podczas wdrażania aplikacji na serwerze, postępuj zgodnie z [te kroki](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) Aby włączyć uwierzytelnianie Windows w usługach IIS na tym serwerze.</span><span class="sxs-lookup"><span data-stu-id="70e98-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="70e98-142">Uwierzytelnianie Windows w HttpListener</span><span class="sxs-lookup"><span data-stu-id="70e98-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="70e98-143">Jeśli są używane na potrzeby samodzielnego hostowania Katana Microsoft.Owin.Host.HttpListener, można włączyć uwierzytelnianie Windows bezpośrednio na **HttpListener** wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="70e98-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="70e98-144">Najpierw utwórz nową aplikację konsoli.</span><span class="sxs-lookup"><span data-stu-id="70e98-144">First, create a new console application.</span></span> <span data-ttu-id="70e98-145">Następnie dodaj pakiety NuGet.</span><span class="sxs-lookup"><span data-stu-id="70e98-145">Next, add NuGet packages.</span></span> <span data-ttu-id="70e98-146">Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**.</span><span class="sxs-lookup"><span data-stu-id="70e98-146">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="70e98-147">W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="70e98-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="70e98-148">Teraz Dodaj klasę o nazwie `Startup` następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="70e98-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="70e98-149">Ta klasa implementuje w tym samym przykładzie "Hello world" z przed, ale także ustawia uwierzytelniania Windows jako schematu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="70e98-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="70e98-150">Wewnątrz `Main` funkcji, zacznij potoku OWIN:</span><span class="sxs-lookup"><span data-stu-id="70e98-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="70e98-151">W narzędziu Fiddler, aby upewnić się, że aplikacja używa uwierzytelniania Windows można wysyłać żądania:</span><span class="sxs-lookup"><span data-stu-id="70e98-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="70e98-152">Tematy pokrewne</span><span class="sxs-lookup"><span data-stu-id="70e98-152">Related Topics</span></span>

[<span data-ttu-id="70e98-153">Omówienie projektu Katana</span><span class="sxs-lookup"><span data-stu-id="70e98-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="70e98-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="70e98-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="70e98-155">Omówienie uwierzytelniania formularzy OWIN w MVC 5</span><span class="sxs-lookup"><span data-stu-id="70e98-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
