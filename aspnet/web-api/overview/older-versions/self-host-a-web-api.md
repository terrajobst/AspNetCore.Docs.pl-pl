---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Host samodzielny ASP.NET Web API 1 (C#) | Dokumentacja firmy Microsoft
author: MikeWasson
description: "Interfejs API sieci Web ASP.NET nie wymaga usług IIS. Interfejs API sieci web można hosta samodzielnego procesu hosta. Ten samouczek pokazuje, jak udostępniać interfejsu API sieci web wewnątrz applic konsoli..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 564f859e73a88ac9c5f27e9b8f7409ec126642f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="57749-105">Host samodzielny ASP.NET Web API 1 (C#)</span><span class="sxs-lookup"><span data-stu-id="57749-105">Self-Host ASP.NET Web API 1 (C#)</span></span>
====================
<span data-ttu-id="57749-106">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="57749-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="57749-107">Interfejs API sieci Web ASP.NET nie wymaga usług IIS.</span><span class="sxs-lookup"><span data-stu-id="57749-107">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="57749-108">Interfejs API sieci web można hosta samodzielnego procesu hosta.</span><span class="sxs-lookup"><span data-stu-id="57749-108">You can self-host a web API in your own host process.</span></span> <span data-ttu-id="57749-109">W tym samouczku przedstawiono sposób obsługi interfejsu API sieci web w aplikacji konsoli.</span><span class="sxs-lookup"><span data-stu-id="57749-109">This tutorial shows how to host a web API inside a console application.</span></span>
> 
> <span data-ttu-id="57749-110">**Nowe aplikacje powinny używać OWIN do hosta samodzielnego interfejsu API sieci Web.**</span><span class="sxs-lookup"><span data-stu-id="57749-110">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="57749-111">Zobacz [umożliwia OWIN Web API 2 platformy ASP.NET hosta samodzielnego](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="57749-111">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="57749-112">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="57749-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="57749-113">1 interfejs API sieci Web</span><span class="sxs-lookup"><span data-stu-id="57749-113">Web API 1</span></span>
> - <span data-ttu-id="57749-114">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="57749-114">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="57749-115">Utwórz projekt aplikacji konsoli</span><span class="sxs-lookup"><span data-stu-id="57749-115">Create the Console Application Project</span></span>

<span data-ttu-id="57749-116">Uruchom program Visual Studio i wybierz **nowy projekt** z **Start** strony.</span><span class="sxs-lookup"><span data-stu-id="57749-116">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="57749-117">Lub z **pliku** menu, wybierz opcję **nowy** , a następnie **projektu**.</span><span class="sxs-lookup"><span data-stu-id="57749-117">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="57749-118">W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła.</span><span class="sxs-lookup"><span data-stu-id="57749-118">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="57749-119">W obszarze **Visual C#**, wybierz pozycję **Windows**.</span><span class="sxs-lookup"><span data-stu-id="57749-119">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="57749-120">Na liście szablony projektów, wybierz **aplikacji konsoli**.</span><span class="sxs-lookup"><span data-stu-id="57749-120">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="57749-121">Nazwij projekt &quot;SelfHost&quot; i kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="57749-121">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="57749-122">Ustaw docelową platformę (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="57749-122">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="57749-123">Jeśli używasz programu Visual Studio 2010, należy zmienić platformę docelową programu .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="57749-123">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="57749-124">(Domyślnie elementy docelowe projektu szablonu [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="57749-124">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="57749-125">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="57749-125">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="57749-126">W **platformy docelowej** listy rozwijanej pozycję zmienić platformę docelową programu .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="57749-126">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="57749-127">Po wyświetleniu monitu, aby zastosować zmiany, kliknij przycisk **tak**.</span><span class="sxs-lookup"><span data-stu-id="57749-127">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="57749-128">Zainstaluj Menedżera pakietów NuGet</span><span class="sxs-lookup"><span data-stu-id="57749-128">Install NuGet Package Manager</span></span>

<span data-ttu-id="57749-129">Menedżer pakietów NuGet jest najprostszym sposobem, aby dodać zestawy interfejsu API sieci Web do projektu z systemem innym niż ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="57749-129">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="57749-130">Aby sprawdzić, czy jest zainstalowany Menedżer pakietów NuGet, kliknij **narzędzia** menu w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="57749-130">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="57749-131">Jeśli widzisz menu elementu o nazwie **Menedżer pakietów biblioteki**, następnie Menedżer pakietów NuGet.</span><span class="sxs-lookup"><span data-stu-id="57749-131">If you see a menu item called **Library Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="57749-132">Aby zainstalować Menedżera pakietów NuGet:</span><span class="sxs-lookup"><span data-stu-id="57749-132">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="57749-133">Uruchom program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="57749-133">Start Visual Studio.</span></span>
2. <span data-ttu-id="57749-134">Z **narzędzia** menu, wybierz opcję **rozszerzenia i aktualizacje**.</span><span class="sxs-lookup"><span data-stu-id="57749-134">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="57749-135">W **rozszerzenia i aktualizacje** okno dialogowe, wybierz opcję **Online**.</span><span class="sxs-lookup"><span data-stu-id="57749-135">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="57749-136">Jeśli nie widzisz "Menedżer pakietów NuGet", wpisz "Menedżer pakietów nuget", w polu wyszukiwania.</span><span class="sxs-lookup"><span data-stu-id="57749-136">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="57749-137">Wybierz Menedżera pakietów NuGet, a następnie kliknij przycisk **Pobierz**.</span><span class="sxs-lookup"><span data-stu-id="57749-137">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="57749-138">Po zakończeniu pobierania, pojawi się monit do zainstalowania.</span><span class="sxs-lookup"><span data-stu-id="57749-138">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="57749-139">Po zakończeniu instalacji może być monitowany o ponowne uruchomienie programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="57749-139">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="57749-140">Dodaj pakiet NuGet interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="57749-140">Add the Web API NuGet Package</span></span>

<span data-ttu-id="57749-141">Po zainstalowaniu Menedżera pakietów NuGet, Dodaj pakiet Self-Host interfejsu API sieci Web do projektu.</span><span class="sxs-lookup"><span data-stu-id="57749-141">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="57749-142">Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**.</span><span class="sxs-lookup"><span data-stu-id="57749-142">From the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="57749-143">*Uwaga*: Jeśli użytkownik nie ma tego menu elementu, upewnij się, Menedżer pakietów NuGet, że zainstalowane poprawnie.</span><span class="sxs-lookup"><span data-stu-id="57749-143">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="57749-144">Wybierz **Zarządzaj pakietami NuGet dla rozwiązania...**</span><span class="sxs-lookup"><span data-stu-id="57749-144">Select **Manage NuGet Packages for Solution...**</span></span>
3. <span data-ttu-id="57749-145">W **Zarządzaj pakietami NugGet** okno dialogowe, wybierz opcję **Online**.</span><span class="sxs-lookup"><span data-stu-id="57749-145">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="57749-146">W polu wyszukiwania wpisz &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="57749-146">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="57749-147">Wybierz pakiet ASP.NET Web API Self Host, a następnie kliknij przycisk **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="57749-147">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="57749-148">Po zainstalowaniu pakietu, kliknij przycisk **zamknąć** aby zamknąć okno dialogowe.</span><span class="sxs-lookup"><span data-stu-id="57749-148">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="57749-149">Upewnij się zainstalować pakiet o nazwie Microsoft.AspNet.WebApi.SelfHost, nie AspNetWebApi.SelfHost.</span><span class="sxs-lookup"><span data-stu-id="57749-149">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="57749-150">Tworzenie modelu i kontrolera</span><span class="sxs-lookup"><span data-stu-id="57749-150">Create the Model and Controller</span></span>

<span data-ttu-id="57749-151">W tym samouczku korzysta z tej samej klasy modelu i kontrolera jako [wprowadzenie](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) samouczka.</span><span class="sxs-lookup"><span data-stu-id="57749-151">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="57749-152">Dodaj Klasa publiczna o nazwie `Product`.</span><span class="sxs-lookup"><span data-stu-id="57749-152">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="57749-153">Dodaj Klasa publiczna o nazwie `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="57749-153">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="57749-154">Pochodzi ta klasa z **System.Web.Http.ApiController**.</span><span class="sxs-lookup"><span data-stu-id="57749-154">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="57749-155">Aby uzyskać więcej informacji na temat kodu w tym kontrolerze, zobacz [wprowadzenie](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) samouczka.</span><span class="sxs-lookup"><span data-stu-id="57749-155">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="57749-156">Ten kontroler definiuje trzy czynności GET:</span><span class="sxs-lookup"><span data-stu-id="57749-156">This controller defines three GET actions:</span></span>

| <span data-ttu-id="57749-157">Identyfikator URI</span><span class="sxs-lookup"><span data-stu-id="57749-157">URI</span></span> | <span data-ttu-id="57749-158">Opis</span><span class="sxs-lookup"><span data-stu-id="57749-158">Description</span></span> |
| --- | --- |
| <span data-ttu-id="57749-159">/ api/produktów</span><span class="sxs-lookup"><span data-stu-id="57749-159">/api/products</span></span> | <span data-ttu-id="57749-160">Pobranie listy wszystkich produktów.</span><span class="sxs-lookup"><span data-stu-id="57749-160">Get a list of all products.</span></span> |
| <span data-ttu-id="57749-161">/API/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="57749-161">/api/products/*id*</span></span> | <span data-ttu-id="57749-162">Uzyskiwanie produktu według identyfikatora.</span><span class="sxs-lookup"><span data-stu-id="57749-162">Get a product by ID.</span></span> |
| <span data-ttu-id="57749-163">/API/produkty /? kategorii =*kategorii*</span><span class="sxs-lookup"><span data-stu-id="57749-163">/api/products/?category=*category*</span></span> | <span data-ttu-id="57749-164">Pobierz listę produktów według kategorii.</span><span class="sxs-lookup"><span data-stu-id="57749-164">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="57749-165">Interfejs API sieci Web hosta</span><span class="sxs-lookup"><span data-stu-id="57749-165">Host the Web API</span></span>

<span data-ttu-id="57749-166">Otwórz plik Program.cs i dodaj następujące instrukcje using:</span><span class="sxs-lookup"><span data-stu-id="57749-166">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="57749-167">Dodaj następujący kod do **Program** klasy.</span><span class="sxs-lookup"><span data-stu-id="57749-167">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="57749-168">(Opcjonalnie) Dodaj Namespace rezerwację adresu URL HTTP</span><span class="sxs-lookup"><span data-stu-id="57749-168">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="57749-169">Ta aplikacja nasłuchuje `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="57749-169">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="57749-170">Domyślnie nasłuchuje pod adresem HTTP wymaga uprawnień administratora.</span><span class="sxs-lookup"><span data-stu-id="57749-170">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="57749-171">Po uruchomieniu tego samouczka, dlatego może ten błąd: "HTTP nie może zarejestrować adresu URL http://+:8080/" istnieją dwa sposoby, aby uniknąć tego błędu:</span><span class="sxs-lookup"><span data-stu-id="57749-171">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="57749-172">Uruchom program Visual Studio z uprawnieniami administratora z podniesionymi uprawnieniami, lub</span><span class="sxs-lookup"><span data-stu-id="57749-172">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="57749-173">Użyj Netsh.exe, aby uzyskać uprawnienia konta do zarezerwowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="57749-173">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="57749-174">Aby użyć Netsh.exe, otwórz wiersz polecenia z uprawnieniami administratora i wprowadź następujące polecenia: następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="57749-174">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="57749-175">gdzie *machine\username* jest konto użytkownika.</span><span class="sxs-lookup"><span data-stu-id="57749-175">where *machine\username* is your user account.</span></span>

<span data-ttu-id="57749-176">Po zakończeniu hostingu samodzielnego, należy usunąć zastrzeżenie:</span><span class="sxs-lookup"><span data-stu-id="57749-176">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="57749-177">Wywołanie interfejsu API sieci Web z aplikacji klienta (C#)</span><span class="sxs-lookup"><span data-stu-id="57749-177">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="57749-178">Napisz aplikacji konsoli simple, która wywołuje interfejs API sieci web.</span><span class="sxs-lookup"><span data-stu-id="57749-178">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="57749-179">Dodaj nowy projekt aplikacji konsoli do rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="57749-179">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="57749-180">W Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy i wybierz **Dodawanie nowego projektu**.</span><span class="sxs-lookup"><span data-stu-id="57749-180">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="57749-181">Utwórz nową aplikację konsoli o nazwie &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="57749-181">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="57749-182">Użyj Menedżera pakietów NuGet można dodać pakietu ASP.NET Web API Core Libraries:</span><span class="sxs-lookup"><span data-stu-id="57749-182">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="57749-183">Wybierz z menu narzędzia **Menedżer pakietów biblioteki**.</span><span class="sxs-lookup"><span data-stu-id="57749-183">From the Tools menu, select **Library Package Manager**.</span></span>
- <span data-ttu-id="57749-184">Wybierz **Zarządzaj pakietami NuGet dla rozwiązania...**</span><span class="sxs-lookup"><span data-stu-id="57749-184">Select **Manage NuGet Packages for Solution...**</span></span>
- <span data-ttu-id="57749-185">W **Zarządzaj pakietami NuGet** okno dialogowe, wybierz opcję **Online**.</span><span class="sxs-lookup"><span data-stu-id="57749-185">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="57749-186">W polu wyszukiwania wpisz &quot;Microsoft.AspNet.WebApi.Client&quot;.</span><span class="sxs-lookup"><span data-stu-id="57749-186">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="57749-187">Wybierz pakiet Microsoft ASP.NET Web API Client Libraries, a następnie kliknij przycisk **zainstalować**.</span><span class="sxs-lookup"><span data-stu-id="57749-187">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="57749-188">Dodaj odwołanie w ClientApp do projektu SelfHost:</span><span class="sxs-lookup"><span data-stu-id="57749-188">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="57749-189">W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt ClientApp.</span><span class="sxs-lookup"><span data-stu-id="57749-189">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="57749-190">Wybierz **Dodaj odwołanie**.</span><span class="sxs-lookup"><span data-stu-id="57749-190">Select **Add Reference**.</span></span>
- <span data-ttu-id="57749-191">W **Menedżera odwołań** okna dialogowego, w obszarze **rozwiązania**, wybierz pozycję **projekty**.</span><span class="sxs-lookup"><span data-stu-id="57749-191">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="57749-192">Wybierz projekt SelfHost.</span><span class="sxs-lookup"><span data-stu-id="57749-192">Select the SelfHost project.</span></span>
- <span data-ttu-id="57749-193">Kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="57749-193">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="57749-194">Otwórz plik Client/Program.cs.</span><span class="sxs-lookup"><span data-stu-id="57749-194">Open the Client/Program.cs file.</span></span> <span data-ttu-id="57749-195">Dodaj następujące **przy użyciu** instrukcji:</span><span class="sxs-lookup"><span data-stu-id="57749-195">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="57749-196">Dodawanie statycznego **HttpClient** wystąpienie:</span><span class="sxs-lookup"><span data-stu-id="57749-196">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="57749-197">Dodaj następujące metody, aby wyświetlić listę wszystkich produktów, produktu za pomocą Identyfikatora listy i listę produktów według kategorii.</span><span class="sxs-lookup"><span data-stu-id="57749-197">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="57749-198">Każda z tych metod następujące tego samego wzorca:</span><span class="sxs-lookup"><span data-stu-id="57749-198">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="57749-199">Wywołanie **HttpClient.GetAsync** wysłać żądania GET do odpowiedniego identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="57749-199">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="57749-200">Wywołanie **HttpResponseMessage.EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="57749-200">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="57749-201">Ta metoda zgłasza wyjątek, jeśli stan odpowiedzi HTTP jest kod błędu.</span><span class="sxs-lookup"><span data-stu-id="57749-201">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="57749-202">Wywołanie **ReadAsAsync&lt;T&gt;**  deserializować typu CLR z odpowiedzi HTTP.</span><span class="sxs-lookup"><span data-stu-id="57749-202">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="57749-203">Ta metoda jest metodą rozszerzenia, zdefiniowane w **System.Net.Http.HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="57749-203">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="57749-204">**GetAsync** i **ReadAsAsync** metody są asynchroniczne.</span><span class="sxs-lookup"><span data-stu-id="57749-204">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="57749-205">Zwracają **zadań** obiekty reprezentujące operację asynchroniczną.</span><span class="sxs-lookup"><span data-stu-id="57749-205">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="57749-206">Pobieranie **wynik** właściwości blokuje wątek przed zakończeniem operacji.</span><span class="sxs-lookup"><span data-stu-id="57749-206">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="57749-207">Więcej informacji o za pomocą elementu HttpClient, jak wykonać nieblokujące wywołania w tym temacie [wywoływania sieci Web interfejsu API z klienta programu .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="57749-207">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="57749-208">Przed wywołaniem metody te, ustaw właściwość BaseAddress na wystąpienie HttpClient "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="57749-208">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="57749-209">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="57749-209">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="57749-210">To wyjścia poniżej.</span><span class="sxs-lookup"><span data-stu-id="57749-210">This should output the following.</span></span> <span data-ttu-id="57749-211">(Pamiętaj, aby uruchomić aplikację SelfHost najpierw).</span><span class="sxs-lookup"><span data-stu-id="57749-211">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
