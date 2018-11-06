---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET Web Pages (Razor) przewodnik rozwiązywania problemów | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym artykule opisano problemy, które mogą wystąpić podczas pracy z ASP.NET Web Pages (Razor) oraz sugerowane rozwiązania. Wersje oprogramowania strona początkowa sieci Web ASP.NET...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: ec8cdda5c5b298736a650f82cd6b52d73b6dfe3d
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021199"
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a><span data-ttu-id="f8a71-104">Przewodnik rozwiązywania problemów ze wzorcem ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="f8a71-104">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>
====================
<span data-ttu-id="f8a71-105">przez [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f8a71-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f8a71-106">W tym artykule opisano problemy, które mogą wystąpić podczas pracy z ASP.NET Web Pages (Razor) oraz sugerowane rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="f8a71-106">This article describes issues that you might have when working with ASP.NET Web Pages (Razor) and some suggested solutions.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="f8a71-107">Wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="f8a71-107">Software versions</span></span>
> 
> 
> - <span data-ttu-id="f8a71-108">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="f8a71-108">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="f8a71-109">W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2 i stron sieci Web platformy ASP.NET w wersji 1.0.</span><span class="sxs-lookup"><span data-stu-id="f8a71-109">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="f8a71-110">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="f8a71-110">This topic contains the following sections:</span></span>

- [<span data-ttu-id="f8a71-111">Problemy z uruchamianiem stron</span><span class="sxs-lookup"><span data-stu-id="f8a71-111">Issues with Running Pages</span></span>](#Issues_Running_.cshtml_Pages)
- [<span data-ttu-id="f8a71-112">Problemy z kodem Razor</span><span class="sxs-lookup"><span data-stu-id="f8a71-112">Issues with Razor Code</span></span>](#IssuesWithRazorCode)
- [<span data-ttu-id="f8a71-113">Problemy dotyczące zabezpieczeń i członkostwa</span><span class="sxs-lookup"><span data-stu-id="f8a71-113">Issues with Security and Membership</span></span>](#membership)
- [<span data-ttu-id="f8a71-114">Problemy z wysyłaniem wiadomości E-mail</span><span class="sxs-lookup"><span data-stu-id="f8a71-114">Issues with Sending Email</span></span>](#email)
- [<span data-ttu-id="f8a71-115">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f8a71-115">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="f8a71-116">Pytania ogólne, można zobaczyć [stron ASP.NET Web Pages (Razor) — często zadawane pytania](https://go.microsoft.com/fwlink/?LinkId=253000).</span><span class="sxs-lookup"><span data-stu-id="f8a71-116">For general questions, see [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000).</span></span>

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a><span data-ttu-id="f8a71-117">Problemy z uruchamianiem stron</span><span class="sxs-lookup"><span data-stu-id="f8a71-117">Issues with Running Pages</span></span>

<span data-ttu-id="f8a71-118">Różne problemy mogą uniemożliwić *.cshtml* i *.vbhtml* stron z prawidłowo uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="f8a71-118">A variety of issues can prevent *.cshtml* and *.vbhtml* pages from running properly.</span></span> <span data-ttu-id="f8a71-119">Ta sekcja zawiera typowe komunikaty o błędach i prawdopodobne przyczyny.</span><span class="sxs-lookup"><span data-stu-id="f8a71-119">This section lists common error messages and likely causes.</span></span>

### <a name="http-error-403---forbidden-access-is-denied"></a><span data-ttu-id="f8a71-120">Błąd HTTP 403 — Dostęp zabroniony: Odmowa dostępu</span><span class="sxs-lookup"><span data-stu-id="f8a71-120">HTTP Error 403 - Forbidden: Access is denied</span></span>

<span data-ttu-id="f8a71-121">*Nie masz uprawnień do wyświetlenia tego katalogu lub strony przy użyciu podanych poświadczeń.*</span><span class="sxs-lookup"><span data-stu-id="f8a71-121">*You do not have permission to view this directory or page using the credentials that you supplied.*</span></span>

<span data-ttu-id="f8a71-122">Ten błąd może wystąpić, jeśli serwer nie działa poprawną wersję programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f8a71-122">This error can occur if the server is not running the correct version of the .NET Framework.</span></span> <span data-ttu-id="f8a71-123">Upewnij się, że komputera, na którym uruchomiono serwer (lokalnie lub zdalnie) co najmniej ma zainstalowany program .NET Framework 4.</span><span class="sxs-lookup"><span data-stu-id="f8a71-123">Make sure that the computer that's running the server (locally or remotely) has at least the .NET Framework 4 installed.</span></span> <span data-ttu-id="f8a71-124">Upewnij się, że sama aplikacja jest skonfigurowany do uruchamiania właściwą wersję również.</span><span class="sxs-lookup"><span data-stu-id="f8a71-124">Also make sure that the application itself is configured to run the right version.</span></span>

<span data-ttu-id="f8a71-125">Jeśli widzisz ten problem z lokalnie podczas pracy w programie WebMatrix, kliknij pozycję **witryny** obszaru roboczego, a następnie w widoku drzewa kliknij **ustawienia**.</span><span class="sxs-lookup"><span data-stu-id="f8a71-125">If you see this problem locally while working in WebMatrix, click the **Site** workspace, and then in the treeview click **Settings**.</span></span> <span data-ttu-id="f8a71-126">W **wybierz wersję systemu .NET Framework** , wybierz na liście **.NET 4 (Integrated)**.</span><span class="sxs-lookup"><span data-stu-id="f8a71-126">In the **Select .NET Framework Version** list, select **.NET 4 (Integrated)**.</span></span> <span data-ttu-id="f8a71-127">Jeśli ta wersja jest już ustawiony, spróbuj uruchomić program WebMatrix jako administrator.</span><span class="sxs-lookup"><span data-stu-id="f8a71-127">If this version is already set, try running WebMatrix as an administrator.</span></span>

<span data-ttu-id="f8a71-128">Upewnij się, że katalog główny witryny sieci Web ma co najmniej jeden *.cshtml* plik.</span><span class="sxs-lookup"><span data-stu-id="f8a71-128">Make sure that the root of your website has at least one *.cshtml* file in it.</span></span>

<span data-ttu-id="f8a71-129">Jeśli zostanie wyświetlony ten błąd, gdy serwer sieci web znajduje się na serwerze zdalnym, skontaktuj się z administratorem serwera.</span><span class="sxs-lookup"><span data-stu-id="f8a71-129">If you see this error when the web server is on a remote server, contact the server administrator.</span></span> <span data-ttu-id="f8a71-130">Upewnij się, że serwer ma programu .NET Framework 4 lub nowszy zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="f8a71-130">Make sure that the server has the .NET Framework 4 or later installed.</span></span> <span data-ttu-id="f8a71-131">Upewnij się, że aplikacja jest uruchomiona w puli aplikacji, który jest skonfigurowany do korzystania z tej wersji systemu.NET Framework również.</span><span class="sxs-lookup"><span data-stu-id="f8a71-131">Also make sure that the application is running in an application pool that's configured to use that version of the.NET Framework.</span></span>

<span data-ttu-id="f8a71-132">Jeśli masz kontrolę nad serwer, upewnij się, że działa on poprawną wersję programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f8a71-132">If you have control over the server, make sure it's running the correct version of the .NET Framework.</span></span> <span data-ttu-id="f8a71-133">Możesz również spróbować naprawić instalację, uruchamiając `aspnet_regiis -iru` polecenia.</span><span class="sxs-lookup"><span data-stu-id="f8a71-133">You might also try repairing the installation by running the `aspnet_regiis -iru` command.</span></span> <span data-ttu-id="f8a71-134">(Na przykład po zainstalowaniu usług IIS po zainstalowaniu programu .NET Framework, usługi IIS nie zostanie poprawnie skonfigurowany do uruchomienia strony ASP.NET.) Aby uzyskać więcej informacji, zobacz [narzędzie rejestracji usług IIS platformy ASP.NET (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="f8a71-134">(For example, if you install IIS after you install the .NET Framework, IIS will not be correctly configured to run ASP.NET pages.) For more information, see [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span></span>

### <a name="http-error-40314---forbidden"></a><span data-ttu-id="f8a71-135">Błąd HTTP 403.14 — dostęp zabroniony</span><span class="sxs-lookup"><span data-stu-id="f8a71-135">HTTP Error 403.14 - Forbidden</span></span>

<span data-ttu-id="f8a71-136">*Serwer sieci Web jest skonfigurowany, aby nie wyświetlać listę zawartości tego katalogu.*</span><span class="sxs-lookup"><span data-stu-id="f8a71-136">*The Web server is configured to not list the contents of this directory.*</span></span>

<span data-ttu-id="f8a71-137">Ten błąd może wystąpić, jeśli żądanie jest zasobem, który jest chroniony (takich jak *Web.config* plików) lub w folderze, który jest chroniony (takich jak *aplikacji\_danych* lub *aplikacji\_Kod*).</span><span class="sxs-lookup"><span data-stu-id="f8a71-137">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="http-error-40417---not-found"></a><span data-ttu-id="f8a71-138">Błąd HTTP 404.17 — nie znaleziono</span><span class="sxs-lookup"><span data-stu-id="f8a71-138">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="f8a71-139">*Żądana zawartość wydaje się być skryptu i będzie nie być obsługiwana przez program obsługi plików statycznych.*</span><span class="sxs-lookup"><span data-stu-id="f8a71-139">*The requested content appears to be script and will not be served by the static file handler.*</span></span>

<span data-ttu-id="f8a71-140">Ten błąd może wystąpić, jeśli serwer nie jest prawidłowo skonfigurowany do użycia programu .NET Framework 4 lub nowszym i w związku z tym nie może rozpoznać kodu w `@{ }` bloków.</span><span class="sxs-lookup"><span data-stu-id="f8a71-140">This error can occur if the server is not configured correctly to use the .NET Framework 4 or later, and therefore does not recognize the code in `@{ }` blocks.</span></span> <span data-ttu-id="f8a71-141">Zobacz opis wcześniej *HTTP błąd 403 — Dostęp zabroniony: odmowa dostępu*.</span><span class="sxs-lookup"><span data-stu-id="f8a71-141">See the description earlier for *HTTP Error 403 - Forbidden: Access is denied*.</span></span>

### <a name="http-error-4047---not-found"></a><span data-ttu-id="f8a71-142">Błąd HTTP 404.7 — nie znaleziono</span><span class="sxs-lookup"><span data-stu-id="f8a71-142">HTTP Error 404.7 - Not Found</span></span>

<span data-ttu-id="f8a71-143">*Moduł filtrowania żądań skonfigurowano odmowę rozszerzenie pliku*</span><span class="sxs-lookup"><span data-stu-id="f8a71-143">*The request filtering module is configured to deny the file extension*</span></span>

<span data-ttu-id="f8a71-144">Ten błąd może wystąpić, jeśli *.cshtml* lub *.vbhtml* rozszerzenia zostały jawnie zablokowane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="f8a71-144">This error can occur if *.cshtml* or *.vbhtml* extensions have been explicitly blocked on the server.</span></span> <span data-ttu-id="f8a71-145">Objawem tego problemu jest działające adresy URL, gdy nie obejmują one rozszerzenie, ale adresy URL, które obejmują *.cshtml* lub *.vbhtml* nie działają.</span><span class="sxs-lookup"><span data-stu-id="f8a71-145">A symptom of this problem is that URLs work when they do not include the extension, but URLs that include *.cshtml* or *.vbhtml* do not work.</span></span> <span data-ttu-id="f8a71-146">Możliwe rozwiązanie to aby ponownie włączyć rozszerzenia w tej witrynie *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="f8a71-146">A possible solution is to re-enable the extensions in the site's *Web.config* file.</span></span> <span data-ttu-id="f8a71-147">Poniższy przykład pokazuje, jak włączyć *.cshtml* rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="f8a71-147">The following example shows how to enable the *.cshtml* extension.</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a><span data-ttu-id="f8a71-148">Błąd HTTP 404.8 — nie znaleziono</span><span class="sxs-lookup"><span data-stu-id="f8a71-148">HTTP Error 404.8 - Not Found</span></span>

<span data-ttu-id="f8a71-149">*Moduł filtrowania żądań skonfigurowano odmowę ścieżki w adresie URL, który zawiera sekcja hiddenSegment.*</span><span class="sxs-lookup"><span data-stu-id="f8a71-149">*The request filtering module is configured to deny a path in the URL that contains a hiddenSegment section.*</span></span>

<span data-ttu-id="f8a71-150">Ten błąd może wystąpić, jeśli żądanie jest zasobem, który jest chroniony (takich jak *Web.config* plików) lub w folderze, który jest chroniony (takich jak *aplikacji\_danych* lub *aplikacji\_Kod*).</span><span class="sxs-lookup"><span data-stu-id="f8a71-150">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a><span data-ttu-id="f8a71-151">Ten typ strony nie jest obsługiwany (błąd serwera w aplikacji» /»)</span><span class="sxs-lookup"><span data-stu-id="f8a71-151">This type of page is not served (Server Error in '/' Application)</span></span>

<span data-ttu-id="f8a71-152">Zobacz opis wcześniej 404.17 Błąd HTTP.</span><span class="sxs-lookup"><span data-stu-id="f8a71-152">See the description earlier for HTTP Error 404.17.</span></span>

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a><span data-ttu-id="f8a71-153">Problemy z kodem Razor</span><span class="sxs-lookup"><span data-stu-id="f8a71-153">Issues with Razor code</span></span>

### <a name="the-name-class-does-not-exist-in-the-current-context"></a><span data-ttu-id="f8a71-154">Nazwa "*klasy*" nie istnieje w bieżącym kontekście</span><span class="sxs-lookup"><span data-stu-id="f8a71-154">The name '*class*' does not exist in the current context</span></span>

<span data-ttu-id="f8a71-155">Często przyczyny wystąpienia tego błędu jest to, że `class` odwołania pomocnika, ale pomocnika nie jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="f8a71-155">Often, a reason you see this error is that `class` references a helper, but the helper is not installed.</span></span> <span data-ttu-id="f8a71-156">Na przykład Jeśli spróbujesz użyć pomocnika, ale nie został jeszcze zainstalowany pakiet z pakietów NuGet, zostanie wyświetlony ten błąd.</span><span class="sxs-lookup"><span data-stu-id="f8a71-156">For example, if you try to use a helper, but if you haven't installed the package from NuGet, you'll see this error.</span></span> <span data-ttu-id="f8a71-157">Galerii w programie WebMatrix można użyć, aby znaleźć i zainstalować pomocnika.</span><span class="sxs-lookup"><span data-stu-id="f8a71-157">Use the Gallery in WebMatrix to find and install the helper.</span></span>

<span data-ttu-id="f8a71-158">Jeśli zainstalowano pomocnika, ale strona nadal nie rozpoznaje ich, spróbuj dodać dodać `using` instrukcji w kodzie.</span><span class="sxs-lookup"><span data-stu-id="f8a71-158">If the helper is installed, but the page still doesn't recognize it, try adding add a `using` statement to the code.</span></span> <span data-ttu-id="f8a71-159">W `using` instrukcji, odwołanie do przestrzeni nazw, który zawiera pomocnika.</span><span class="sxs-lookup"><span data-stu-id="f8a71-159">In the `using` statement, reference the namespace that includes the helper.</span></span> <span data-ttu-id="f8a71-160">Na przykład podstawowa pomocników, znajdujących się w pakiecie ASP.NET pomocnicy znajdują się w `System.Web.Helpers` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="f8a71-160">For example, the basic helpers that are in the ASP.NET Web Helpers package are in the `System.Web.Helpers` namespace.</span></span> <span data-ttu-id="f8a71-161">W górnej części strony, której chcesz użyć pomocnika Dodaj następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="f8a71-161">At the top of the page where you want to use the helper, add this line:</span></span>

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a><span data-ttu-id="f8a71-162">Problemy dotyczące zabezpieczeń i członkostwa</span><span class="sxs-lookup"><span data-stu-id="f8a71-162">Issues with Security and Membership</span></span>

<span data-ttu-id="f8a71-163">Jeśli używasz systemu wbudowanych rozwiązań zabezpieczeń (Członkowskimi) w składniku ASP.NET Web Pages (Razor), można napotkać następujące problemy.</span><span class="sxs-lookup"><span data-stu-id="f8a71-163">If you are using the built-in security (membership) system in ASP.NET Web Pages (Razor), you might encounter the following issues.</span></span>

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a><span data-ttu-id="f8a71-164">Aby wywołać tę metodę, właściwość "Membership.Provider" musi być wystąpieniem elementu "ExtendedMembershipProvider"</span><span class="sxs-lookup"><span data-stu-id="f8a71-164">To call this method, the "Membership.Provider" property must be an instance of "ExtendedMembershipProvider"</span></span>

<span data-ttu-id="f8a71-165">Ten błąd może wskazywać, że nie `AspNetSqlMembershipProvider` klasa jest skonfigurowana.</span><span class="sxs-lookup"><span data-stu-id="f8a71-165">This error can indicate that no `AspNetSqlMembershipProvider` class is configured.</span></span> <span data-ttu-id="f8a71-166">(Symptomem jest czy lokacji działa prawidłowo lokalnie, ale zgłasza błąd podczas publikowania do dostawcy hostingu serwera). Jeden rozwiązać ten problem jest jawnie włączyć członkostwo proste przez dodanie poniższego w witrynie *Web.config* pliku:</span><span class="sxs-lookup"><span data-stu-id="f8a71-166">(A symptom is that the site works fine locally but throws this error when you publish it to a hosting provider's server.) One fix for this problem is to explicitly enable simple membership by adding the following to the site's *Web.config* file:</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a><span data-ttu-id="f8a71-167">Problemy z wysyłaniem wiadomości E-mail</span><span class="sxs-lookup"><span data-stu-id="f8a71-167">Issues with Sending Email</span></span>

<span data-ttu-id="f8a71-168">Problemy z wysyłaniem wiadomości e-mail może być wyzwaniem do debugowania.</span><span class="sxs-lookup"><span data-stu-id="f8a71-168">Problems with sending email can be challenging to debug.</span></span> <span data-ttu-id="f8a71-169">Początkowego problemu może być, że nie można nawiązać połączenia z serwerem SMTP.</span><span class="sxs-lookup"><span data-stu-id="f8a71-169">An initial problem can be that you can't connect to the SMTP server.</span></span> <span data-ttu-id="f8a71-170">Jeśli połączenie zostanie nawiązane, ASP.NET przekazywało wiadomość do serwera SMTP.</span><span class="sxs-lookup"><span data-stu-id="f8a71-170">If the connection is successful, ASP.NET hands the message off to the SMTP server.</span></span> <span data-ttu-id="f8a71-171">Jednakże mogą wystąpić problemy z komunikatem sam, który uniemożliwia wysłanie go do serwera SMTP.</span><span class="sxs-lookup"><span data-stu-id="f8a71-171">However, there can be problems with the message itself that prevents the SMTP server from sending it.</span></span>

<span data-ttu-id="f8a71-172">Jeśli aplikacja nie pomyślnie wysłać wiadomości e-mail, spróbuj wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="f8a71-172">If your application does not successfully send email, try the following:</span></span>

- <span data-ttu-id="f8a71-173">Nazwa serwera SMTP jest często podobny `smtp.provider.com` lub `smtp.provider.net`.</span><span class="sxs-lookup"><span data-stu-id="f8a71-173">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="f8a71-174">Jeśli jednak publikowania witryny dostawcy usług hostingowych, nazwę serwera SMTP w tym momencie może być `localhost`.</span><span class="sxs-lookup"><span data-stu-id="f8a71-174">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="f8a71-175">Ta sytuacja występuje, ponieważ po opublikowaniu i witryna jest hostowana na serwerze dostawcy, serwer SMTP, może być lokalne z perspektywy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f8a71-175">This situation occurs because after you've published and your site is running on the provider's server, the SMTP server might be local from the perspective of your application.</span></span> <span data-ttu-id="f8a71-176">Ta zmiana nazwy serwera może oznaczać, że trzeba zmienić nazwę serwera SMTP w ramach procesu publikowania.</span><span class="sxs-lookup"><span data-stu-id="f8a71-176">This change in server names might mean that you have to change the SMTP server name as part of your publishing process.</span></span>
- <span data-ttu-id="f8a71-177">Numer portu jest zwykle 25.</span><span class="sxs-lookup"><span data-stu-id="f8a71-177">The port number is usually 25.</span></span> <span data-ttu-id="f8a71-178">Jednakże niektórzy dostawcy wymagają użycia portu 587 lub niektórych innych portów.</span><span class="sxs-lookup"><span data-stu-id="f8a71-178">However, some providers require you to use port 587 or some other port.</span></span> <span data-ttu-id="f8a71-179">Skontaktuj się z właścicielem serwera SMTP, w jakiego numeru portu spełniają oczekiwane do użycia.</span><span class="sxs-lookup"><span data-stu-id="f8a71-179">Check with the owner of the SMTP server what port number they expect you to use.</span></span>
- <span data-ttu-id="f8a71-180">Upewnij się, że używasz właściwych poświadczeń dostępu.</span><span class="sxs-lookup"><span data-stu-id="f8a71-180">Make sure that you use the right credentials.</span></span> <span data-ttu-id="f8a71-181">Po opublikowaniu witryny dostawcy usług hostingowych, Użyj poświadczeń z dostawcy specjalnie wskazuje, czy do obsługi poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="f8a71-181">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="f8a71-182">Te poświadczenia mogą się różnić od poświadczenia, których używasz do publikowania.</span><span class="sxs-lookup"><span data-stu-id="f8a71-182">These credentials might be different from the credentials you use to publish.</span></span>
- <span data-ttu-id="f8a71-183">Czasami nie potrzebujesz poświadczeń w ogóle.</span><span class="sxs-lookup"><span data-stu-id="f8a71-183">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="f8a71-184">Przy wysyłaniu wiadomości e-mail przy użyciu osobistych usługodawcę internetowego, dostawcy poczty e-mail może już znasz swoje poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="f8a71-184">If you're sending email by using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="f8a71-185">Po opublikowaniu, może być konieczne użycie innych poświadczeń niż podczas testowania na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="f8a71-185">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
- <span data-ttu-id="f8a71-186">Jeśli dostawcy poczty e-mail używa szyfrowania, ustaw `WebMail.EnableSsl` do `true`.</span><span class="sxs-lookup"><span data-stu-id="f8a71-186">If your email provider uses encryption, set `WebMail.EnableSsl` to `true`.</span></span>

<span data-ttu-id="f8a71-187">Występuje błąd podczas wysyłania wiadomości e-mail, może zostać wyświetlony komunikat o błędzie standardowa ASP.NET, która wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="f8a71-187">If there is an error sending email, you might see a standard ASP.NET error message, which looks like this:</span></span>

![Komunikat o błędzie programu ASP.NET, gdy występuje problem z adresem e-mail](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

<span data-ttu-id="f8a71-189">Można również debugować problemy z wysyłaniem wiadomości e-mail przy użyciu `try-catch` bloku, jak w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="f8a71-189">You can also debug problems with sending email by using a `try-catch` block, as in the following example.</span></span> <span data-ttu-id="f8a71-190">Kiedy używasz `try-catch` bloku, program ASP.NET nie wyświetla jego standardowy błąd wiadomości.</span><span class="sxs-lookup"><span data-stu-id="f8a71-190">When you use a `try-catch` block, ASP.NET does not display its standard error messages.</span></span> <span data-ttu-id="f8a71-191">Zamiast tego można przechwycić błąd w `catch` części bloku.</span><span class="sxs-lookup"><span data-stu-id="f8a71-191">Instead, you can capture the error in the `catch` portion of the block.</span></span>

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

<span data-ttu-id="f8a71-192">Zastąp odpowiednie wartości dla `your-SMTP-server-name`i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="f8a71-192">Substitute the appropriate values for `your-SMTP-server-name`, and so on.</span></span> <span data-ttu-id="f8a71-193">Komunikaty o błędach, które można napotkać w ten sposób między innymi następujące:</span><span class="sxs-lookup"><span data-stu-id="f8a71-193">Some of the error messages you might see this way include the following:</span></span>

- <span data-ttu-id="f8a71-194">*Błąd podczas wysyłania wiadomości e-mail.*</span><span class="sxs-lookup"><span data-stu-id="f8a71-194">*Failure sending mail.*</span></span>

    <span data-ttu-id="f8a71-195">—lub—</span><span class="sxs-lookup"><span data-stu-id="f8a71-195">-or-</span></span>

    <span data-ttu-id="f8a71-196">*Próba połączenia nie powiodło się, ponieważ strona połączona nie odpowiedziała poprawnie po określonym czasie albo ustanowione połączenie nie powiodło się, ponieważ połączony host nie odpowiada*</span><span class="sxs-lookup"><span data-stu-id="f8a71-196">*A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond*</span></span>

    <span data-ttu-id="f8a71-197">Ten błąd zazwyczaj oznacza, że aplikacja nie może połączyć się z serwerem SMTP.</span><span class="sxs-lookup"><span data-stu-id="f8a71-197">This error usually means that the application could not connect to the SMTP server.</span></span> <span data-ttu-id="f8a71-198">Sprawdź nazwę serwera i numer portu.</span><span class="sxs-lookup"><span data-stu-id="f8a71-198">Check the server name and port number.</span></span>
- <span data-ttu-id="f8a71-199"><em>Skrzynka pocztowa jest niedostępna. Odpowiedź serwera: 5.1.0 &lt; someuser@invaliddomain &gt; nadawcy odrzucone: nieprawidłowy nadawca domeny</em></span><span class="sxs-lookup"><span data-stu-id="f8a71-199"><em>Mailbox unavailable. The server response was: 5.1.0 &lt;someuser@invaliddomain&gt; sender rejected : invalid sender domain</em></span></span>

    <span data-ttu-id="f8a71-200">Ten komunikat można wskazać, że `From` adres jest nieprawidłowy lub Brak.</span><span class="sxs-lookup"><span data-stu-id="f8a71-200">This message can indicate that the `From` address is not correct or is missing.</span></span>
- <span data-ttu-id="f8a71-201">*Określony ciąg nie jest w formularzu wymaganych dla adresu e-mail.*</span><span class="sxs-lookup"><span data-stu-id="f8a71-201">*The specified string is not in the form required for an email address.*</span></span>

    <span data-ttu-id="f8a71-202">Ten błąd może wskazywać, że wartość `To` lub `From` właściwości nie są rozpoznawane jako adresy e-mail.</span><span class="sxs-lookup"><span data-stu-id="f8a71-202">This error might indicate that the value of the `To` or `From` properties are not recognized as email addresses.</span></span> <span data-ttu-id="f8a71-203">(ASP.NET nie może sprawdzić, czy adres e-mail jest prawidłowy, tylko jej użytkownika w prawidłowym formacie jak *name@domain.com*.)</span><span class="sxs-lookup"><span data-stu-id="f8a71-203">(ASP.NET cannot check that the email address is valid, only that it's in the correct format, like *name@domain.com*.)</span></span>

> [!NOTE]
> <span data-ttu-id="f8a71-204">Usuń kod znaczników, który powoduje wyświetlenie błędu (`@errorMessage`), przed opublikowaniem strony w działającej witrynie.</span><span class="sxs-lookup"><span data-stu-id="f8a71-204">Remove the markup that displays the error (`@errorMessage`) before you publish the page to a live site.</span></span> <span data-ttu-id="f8a71-205">Nie jest dobry pomysł, aby umożliwić użytkownikom wyświetlić komunikaty o błędach, które można uzyskać z serwera.</span><span class="sxs-lookup"><span data-stu-id="f8a71-205">It's not a good idea to let users see error messages that you get from a server.</span></span>


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="f8a71-206">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f8a71-206">Additional Resources</span></span>

[<span data-ttu-id="f8a71-207">ASP.NET Web Pages (Razor) — często zadawane pytania</span><span class="sxs-lookup"><span data-stu-id="f8a71-207">ASP.NET Web Pages (Razor) FAQ</span></span>](https://go.microsoft.com/fwlink/?LinkId=253000)

<span data-ttu-id="f8a71-208">[Program WebMatrix i stron ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum w witrynie sieci Web platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f8a71-208">[WebMatrix and ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum on the ASP.NET website</span></span>
