---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET Web Pages przewodnik rozwiązywania problemów (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule opisano problemy, które mogą wystąpić podczas pracy z stron sieci Web platformy ASP.NET (Razor) oraz sugerowane rozwiązania. Wersje oprogramowania Zazn sieci Web ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: ec51169ccea0016712de3fdb28a16a174150a8bd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898516"
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a><span data-ttu-id="76fb8-104">Strony sieci Web platformy ASP.NET (Razor) przewodnik rozwiązywania problemów</span><span class="sxs-lookup"><span data-stu-id="76fb8-104">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>
====================
<span data-ttu-id="76fb8-105">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="76fb8-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="76fb8-106">W tym artykule opisano problemy, które mogą wystąpić podczas pracy z stron sieci Web platformy ASP.NET (Razor) oraz sugerowane rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="76fb8-106">This article describes issues that you might have when working with ASP.NET Web Pages (Razor) and some suggested solutions.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="76fb8-107">Wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="76fb8-107">Software versions</span></span>
> 
> 
> - <span data-ttu-id="76fb8-108">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="76fb8-108">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="76fb8-109">W tym samouczku współdziała również z ASP.NET Web Pages 2 i stron sieci Web ASP.NET w wersji 1.0.</span><span class="sxs-lookup"><span data-stu-id="76fb8-109">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="76fb8-110">Ten temat zawiera następujące sekcje:</span><span class="sxs-lookup"><span data-stu-id="76fb8-110">This topic contains the following sections:</span></span>

- [<span data-ttu-id="76fb8-111">Problemy z uruchamianiem stron</span><span class="sxs-lookup"><span data-stu-id="76fb8-111">Issues with Running Pages</span></span>](#Issues_Running_.cshtml_Pages)
- [<span data-ttu-id="76fb8-112">Problemy z kodu Razor</span><span class="sxs-lookup"><span data-stu-id="76fb8-112">Issues with Razor Code</span></span>](#IssuesWithRazorCode)
- [<span data-ttu-id="76fb8-113">Problemy dotyczące zabezpieczeń i członkostwa</span><span class="sxs-lookup"><span data-stu-id="76fb8-113">Issues with Security and Membership</span></span>](#membership)
- [<span data-ttu-id="76fb8-114">Problemy z wysyłaniem wiadomości E-mail</span><span class="sxs-lookup"><span data-stu-id="76fb8-114">Issues with Sending Email</span></span>](#email)
- [<span data-ttu-id="76fb8-115">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="76fb8-115">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="76fb8-116">Pytania ogólne, zobacz [stron sieci Web platformy ASP.NET (Razor) — często zadawane pytania](https://go.microsoft.com/fwlink/?LinkId=253000).</span><span class="sxs-lookup"><span data-stu-id="76fb8-116">For general questions, see [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000).</span></span>

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a><span data-ttu-id="76fb8-117">Problemy z uruchamianiem stron</span><span class="sxs-lookup"><span data-stu-id="76fb8-117">Issues with Running Pages</span></span>

<span data-ttu-id="76fb8-118">Różne problemy można zapobiec *.cshtml* i *.vbhtml* stron z prawidłowo uruchomiona.</span><span class="sxs-lookup"><span data-stu-id="76fb8-118">A variety of issues can prevent *.cshtml* and *.vbhtml* pages from running properly.</span></span> <span data-ttu-id="76fb8-119">Ta sekcja zawiera typowe komunikaty o błędach i prawdopodobne przyczyny.</span><span class="sxs-lookup"><span data-stu-id="76fb8-119">This section lists common error messages and likely causes.</span></span>

### <a name="http-error-403---forbidden-access-is-denied"></a><span data-ttu-id="76fb8-120">Błąd HTTP 403 — Dostęp zabroniony: Odmowa dostępu</span><span class="sxs-lookup"><span data-stu-id="76fb8-120">HTTP Error 403 - Forbidden: Access is denied</span></span>

<span data-ttu-id="76fb8-121">*Nie masz uprawnień do wyświetlenia tego katalogu lub strony przy użyciu podanych poświadczeń.*</span><span class="sxs-lookup"><span data-stu-id="76fb8-121">*You do not have permission to view this directory or page using the credentials that you supplied.*</span></span>

<span data-ttu-id="76fb8-122">Ten błąd może wystąpić, jeśli serwer nie działa poprawna wersja programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="76fb8-122">This error can occur if the server is not running the correct version of the .NET Framework.</span></span> <span data-ttu-id="76fb8-123">Upewnij się, czy komputera, na którym uruchomiono serwer (lokalnie lub zdalnie) jest co najmniej programu .NET Framework 4 zainstalowane.</span><span class="sxs-lookup"><span data-stu-id="76fb8-123">Make sure that the computer that's running the server (locally or remotely) has at least the .NET Framework 4 installed.</span></span> <span data-ttu-id="76fb8-124">Upewnij się, że sama aplikacja jest skonfigurowany do uruchamiania właściwej wersji również.</span><span class="sxs-lookup"><span data-stu-id="76fb8-124">Also make sure that the application itself is configured to run the right version.</span></span>

<span data-ttu-id="76fb8-125">Jeśli widzisz ten problem z lokalnie podczas pracy w programie WebMatrix, kliknij przycisk **lokacji** obszaru roboczego, a następnie w widoku drzewa kliknij **ustawienia**.</span><span class="sxs-lookup"><span data-stu-id="76fb8-125">If you see this problem locally while working in WebMatrix, click the **Site** workspace, and then in the treeview click **Settings**.</span></span> <span data-ttu-id="76fb8-126">W **wybierz wersję systemu .NET Framework** wybierz **.NET 4 (zintegrowany)**.</span><span class="sxs-lookup"><span data-stu-id="76fb8-126">In the **Select .NET Framework Version** list, select **.NET 4 (Integrated)**.</span></span> <span data-ttu-id="76fb8-127">Jeśli ta wersja jest już ustawiona, spróbuj uruchomić program WebMatrix jako administrator.</span><span class="sxs-lookup"><span data-stu-id="76fb8-127">If this version is already set, try running WebMatrix as an administrator.</span></span>

<span data-ttu-id="76fb8-128">Upewnij się, że w katalogu głównym witryny sieci Web ma co najmniej jedną *.cshtml* pliku w nim.</span><span class="sxs-lookup"><span data-stu-id="76fb8-128">Make sure that the root of your website has at least one *.cshtml* file in it.</span></span>

<span data-ttu-id="76fb8-129">Jeśli zostanie wyświetlony ten błąd, gdy serwer sieci web znajduje się na serwerze zdalnym, skontaktuj się z administratorem serwera.</span><span class="sxs-lookup"><span data-stu-id="76fb8-129">If you see this error when the web server is on a remote server, contact the server administrator.</span></span> <span data-ttu-id="76fb8-130">Upewnij się, że serwer ma programu .NET Framework 4 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="76fb8-130">Make sure that the server has the .NET Framework 4 or later installed.</span></span> <span data-ttu-id="76fb8-131">Upewnij się, że aplikacja działa w puli aplikacji jest skonfigurowana do używania tej wersji środowiska.NET Framework również.</span><span class="sxs-lookup"><span data-stu-id="76fb8-131">Also make sure that the application is running in an application pool that's configured to use that version of the.NET Framework.</span></span>

<span data-ttu-id="76fb8-132">Jeśli masz kontrolę nad serwer, upewnij się, że działa ona poprawna wersja programu .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="76fb8-132">If you have control over the server, make sure it's running the correct version of the .NET Framework.</span></span> <span data-ttu-id="76fb8-133">Możesz również spróbować naprawić instalację, uruchamiając `aspnet_regiis -iru` polecenia.</span><span class="sxs-lookup"><span data-stu-id="76fb8-133">You might also try repairing the installation by running the `aspnet_regiis -iru` command.</span></span> <span data-ttu-id="76fb8-134">(Na przykład po zainstalowaniu programu IIS po zainstalowaniu programu .NET Framework, usługi IIS będzie nie być poprawnie skonfigurowany do uruchamiania stron ASP.NET.) Aby uzyskać więcej informacji, zobacz [narzędzie rejestracji programu ASP.NET usług IIS (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="76fb8-134">(For example, if you install IIS after you install the .NET Framework, IIS will not be correctly configured to run ASP.NET pages.) For more information, see [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span></span>

### <a name="http-error-40314---forbidden"></a><span data-ttu-id="76fb8-135">403.14 — dostęp zabroniony. Błąd HTTP</span><span class="sxs-lookup"><span data-stu-id="76fb8-135">HTTP Error 403.14 - Forbidden</span></span>

<span data-ttu-id="76fb8-136">*Serwer sieci Web skonfigurowano nie listy zawartości tego katalogu.*</span><span class="sxs-lookup"><span data-stu-id="76fb8-136">*The Web server is configured to not list the contents of this directory.*</span></span>

<span data-ttu-id="76fb8-137">Ten błąd może wystąpić, jeśli żądanie zostało wysłane z zasobem, który jest chroniony (takich jak *Web.config* pliku) lub w folderze, który jest chroniony (takich jak *aplikacji\_danych* lub *aplikacji\_Kod*).</span><span class="sxs-lookup"><span data-stu-id="76fb8-137">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="http-error-40417---not-found"></a><span data-ttu-id="76fb8-138">Błąd HTTP 404.17 — nie znaleziono</span><span class="sxs-lookup"><span data-stu-id="76fb8-138">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="76fb8-139">*Żądana zawartość najprawdopodobniej jest skryptem i nie zostanie obsłużona przez obsługę plików statycznych.*</span><span class="sxs-lookup"><span data-stu-id="76fb8-139">*The requested content appears to be script and will not be served by the static file handler.*</span></span>

<span data-ttu-id="76fb8-140">Ten błąd może wystąpić, jeśli serwer nie jest poprawnie skonfigurowany do użycia programu .NET Framework 4 lub później, a zatem nie rozpoznaje kod w `@{ }` bloków.</span><span class="sxs-lookup"><span data-stu-id="76fb8-140">This error can occur if the server is not configured correctly to use the .NET Framework 4 or later, and therefore does not recognize the code in `@{ }` blocks.</span></span> <span data-ttu-id="76fb8-141">Zobacz opis wcześniej *HTTP błąd 403 — Dostęp zabroniony: odmowa dostępu*.</span><span class="sxs-lookup"><span data-stu-id="76fb8-141">See the description earlier for *HTTP Error 403 - Forbidden: Access is denied*.</span></span>

### <a name="http-error-4047---not-found"></a><span data-ttu-id="76fb8-142">Błąd HTTP 404.7 — nie znaleziono</span><span class="sxs-lookup"><span data-stu-id="76fb8-142">HTTP Error 404.7 - Not Found</span></span>

<span data-ttu-id="76fb8-143">*W module filtrowania żądań skonfigurowano odrzucanie podanego rozszerzenia pliku*</span><span class="sxs-lookup"><span data-stu-id="76fb8-143">*The request filtering module is configured to deny the file extension*</span></span>

<span data-ttu-id="76fb8-144">Ten błąd może wystąpić, jeśli *.cshtml* lub *.vbhtml* jawnie zablokowano rozszerzeń na serwerze.</span><span class="sxs-lookup"><span data-stu-id="76fb8-144">This error can occur if *.cshtml* or *.vbhtml* extensions have been explicitly blocked on the server.</span></span> <span data-ttu-id="76fb8-145">Objawem tego problemu jest pracy adresów URL, gdy nie obejmują one rozszerzenie, ale adresów URL, które obejmują *.cshtml* lub *.vbhtml* nie działają.</span><span class="sxs-lookup"><span data-stu-id="76fb8-145">A symptom of this problem is that URLs work when they do not include the extension, but URLs that include *.cshtml* or *.vbhtml* do not work.</span></span> <span data-ttu-id="76fb8-146">Możliwe rozwiązanie jest ponownie włączyć rozszerzenia w tej witrynie *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="76fb8-146">A possible solution is to re-enable the extensions in the site's *Web.config* file.</span></span> <span data-ttu-id="76fb8-147">Poniższy przykład przedstawia sposób włączania *.cshtml* rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="76fb8-147">The following example shows how to enable the *.cshtml* extension.</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a><span data-ttu-id="76fb8-148">Błąd HTTP 404.8 — nie znaleziono</span><span class="sxs-lookup"><span data-stu-id="76fb8-148">HTTP Error 404.8 - Not Found</span></span>

<span data-ttu-id="76fb8-149">*W module filtrowania żądań skonfigurowano odrzucanie ścieżek w adresie URL zawierających sekcję hiddenSegment.*</span><span class="sxs-lookup"><span data-stu-id="76fb8-149">*The request filtering module is configured to deny a path in the URL that contains a hiddenSegment section.*</span></span>

<span data-ttu-id="76fb8-150">Ten błąd może wystąpić, jeśli żądanie zostało wysłane z zasobem, który jest chroniony (takich jak *Web.config* pliku) lub w folderze, który jest chroniony (takich jak *aplikacji\_danych* lub *aplikacji\_Kod*).</span><span class="sxs-lookup"><span data-stu-id="76fb8-150">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a><span data-ttu-id="76fb8-151">Ten typ strony nie jest obsługiwany (błąd serwera w "/" aplikacja)</span><span class="sxs-lookup"><span data-stu-id="76fb8-151">This type of page is not served (Server Error in '/' Application)</span></span>

<span data-ttu-id="76fb8-152">Zobacz opis wcześniej 404.17 błędu HTTP.</span><span class="sxs-lookup"><span data-stu-id="76fb8-152">See the description earlier for HTTP Error 404.17.</span></span>

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a><span data-ttu-id="76fb8-153">Problemy z kodu Razor</span><span class="sxs-lookup"><span data-stu-id="76fb8-153">Issues with Razor code</span></span>

### <a name="the-name-class-does-not-exist-in-the-current-context"></a><span data-ttu-id="76fb8-154">Nazwa "*klasy*" nie istnieje w bieżącym kontekście</span><span class="sxs-lookup"><span data-stu-id="76fb8-154">The name '*class*' does not exist in the current context</span></span>

<span data-ttu-id="76fb8-155">Często przyczyną wystąpienia tego błędu jest to, że `class` odwołania pomocnika, ale pomocnika nie jest zainstalowany.</span><span class="sxs-lookup"><span data-stu-id="76fb8-155">Often, a reason you see this error is that `class` references a helper, but the helper is not installed.</span></span> <span data-ttu-id="76fb8-156">Na przykład Jeśli spróbujesz użyć pomocnika, ale pakiet nie został zainstalowany z pakietu NuGet, zostanie wyświetlony ten błąd.</span><span class="sxs-lookup"><span data-stu-id="76fb8-156">For example, if you try to use a helper, but if you haven't installed the package from NuGet, you'll see this error.</span></span> <span data-ttu-id="76fb8-157">Galerii w programie WebMatrix można użyć, aby znaleźć i zainstalować pomocnika.</span><span class="sxs-lookup"><span data-stu-id="76fb8-157">Use the Gallery in WebMatrix to find and install the helper.</span></span>

<span data-ttu-id="76fb8-158">Jeśli zainstalowano pomocnika, ale strony nadal nie rozpoznaje ją, spróbuj dodać dodać `using` instrukcji w kodzie.</span><span class="sxs-lookup"><span data-stu-id="76fb8-158">If the helper is installed, but the page still doesn't recognize it, try adding add a `using` statement to the code.</span></span> <span data-ttu-id="76fb8-159">W `using` instrukcji, odwołanie do przestrzeni nazw, która zawiera pomocnika.</span><span class="sxs-lookup"><span data-stu-id="76fb8-159">In the `using` statement, reference the namespace that includes the helper.</span></span> <span data-ttu-id="76fb8-160">Na przykład podstawowa wątków, które znajdują się w pakiecie pomocników sieci Web ASP.NET znajdują się w `System.Web.Helpers` przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="76fb8-160">For example, the basic helpers that are in the ASP.NET Web Helpers package are in the `System.Web.Helpers` namespace.</span></span> <span data-ttu-id="76fb8-161">W górnej części strony, której chcesz użyć pomocnika Dodaj następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="76fb8-161">At the top of the page where you want to use the helper, add this line:</span></span>

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a><span data-ttu-id="76fb8-162">Problemy dotyczące zabezpieczeń i członkostwa</span><span class="sxs-lookup"><span data-stu-id="76fb8-162">Issues with Security and Membership</span></span>

<span data-ttu-id="76fb8-163">Jeśli korzystasz z systemu zabezpieczeń (Członkowskimi) na stronach sieci Web platformy ASP.NET (Razor), można napotkać następujące problemy.</span><span class="sxs-lookup"><span data-stu-id="76fb8-163">If you are using the built-in security (membership) system in ASP.NET Web Pages (Razor), you might encounter the following issues.</span></span>

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a><span data-ttu-id="76fb8-164">Aby wywołać tę metodę, właściwość "Membership.Provider" musi być wystąpieniem typu "ExtendedMembershipProvider"</span><span class="sxs-lookup"><span data-stu-id="76fb8-164">To call this method, the "Membership.Provider" property must be an instance of "ExtendedMembershipProvider"</span></span>

<span data-ttu-id="76fb8-165">Ten błąd może oznaczać, że nie `AspNetSqlMembershipProvider` klasa jest skonfigurowana.</span><span class="sxs-lookup"><span data-stu-id="76fb8-165">This error can indicate that no `AspNetSqlMembershipProvider` class is configured.</span></span> <span data-ttu-id="76fb8-166">(Objawem jest że lokacji działa prawidłowo lokalnie, ale zgłasza błąd podczas publikowania do dostawcy hostingu serwera). Jedną poprawkę dotyczącą tego problemu jest jawnie włączyć członkostwa prostego przez dodanie poniższego lokacji *Web.config* pliku:</span><span class="sxs-lookup"><span data-stu-id="76fb8-166">(A symptom is that the site works fine locally but throws this error when you publish it to a hosting provider's server.) One fix for this problem is to explicitly enable simple membership by adding the following to the site's *Web.config* file:</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a><span data-ttu-id="76fb8-167">Problemy z wysyłaniem wiadomości E-mail</span><span class="sxs-lookup"><span data-stu-id="76fb8-167">Issues with Sending Email</span></span>

<span data-ttu-id="76fb8-168">Problemy z wysyłaniem wiadomości e-mail może być wyzwaniem do debugowania.</span><span class="sxs-lookup"><span data-stu-id="76fb8-168">Problems with sending email can be challenging to debug.</span></span> <span data-ttu-id="76fb8-169">Początkowa problem może być, nie można połączyć się z serwerem SMTP.</span><span class="sxs-lookup"><span data-stu-id="76fb8-169">An initial problem can be that you can't connect to the SMTP server.</span></span> <span data-ttu-id="76fb8-170">Jeśli połączenie zostanie nawiązane, ASP.NET przekazuje komunikat do serwera SMTP.</span><span class="sxs-lookup"><span data-stu-id="76fb8-170">If the connection is successful, ASP.NET hands the message off to the SMTP server.</span></span> <span data-ttu-id="76fb8-171">Jednak mogą wystąpić problemy z samej wiadomości, który uniemożliwia wysłanie ich do serwera SMTP.</span><span class="sxs-lookup"><span data-stu-id="76fb8-171">However, there can be problems with the message itself that prevents the SMTP server from sending it.</span></span>

<span data-ttu-id="76fb8-172">Jeśli aplikacja nie pomyślnie wysłać wiadomości e-mail, należy spróbować wykonać następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="76fb8-172">If your application does not successfully send email, try the following:</span></span>

- <span data-ttu-id="76fb8-173">Nazwa serwera SMTP jest często przypominać `smtp.provider.com` lub `smtp.provider.net`.</span><span class="sxs-lookup"><span data-stu-id="76fb8-173">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="76fb8-174">Jeśli jednak publikowania witryny dostawcy hostingu, nazwę serwera SMTP w tym momencie może być `localhost`.</span><span class="sxs-lookup"><span data-stu-id="76fb8-174">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="76fb8-175">Ta sytuacja występuje, ponieważ po opublikowaniu, witryna jest hostowana na serwerze dostawcy, serwer SMTP może być lokalny z punktu widzenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="76fb8-175">This situation occurs because after you've published and your site is running on the provider's server, the SMTP server might be local from the perspective of your application.</span></span> <span data-ttu-id="76fb8-176">Ta zmiana nazwy serwera może to oznaczać trzeba zmienić nazwę serwera SMTP w trakcie procesu publikowania.</span><span class="sxs-lookup"><span data-stu-id="76fb8-176">This change in server names might mean that you have to change the SMTP server name as part of your publishing process.</span></span>
- <span data-ttu-id="76fb8-177">Numer portu jest zwykle 25.</span><span class="sxs-lookup"><span data-stu-id="76fb8-177">The port number is usually 25.</span></span> <span data-ttu-id="76fb8-178">Jednak niektóre dostawców wymagają użycia portu 587 lub pewne inne porty.</span><span class="sxs-lookup"><span data-stu-id="76fb8-178">However, some providers require you to use port 587 or some other port.</span></span> <span data-ttu-id="76fb8-179">Skontaktuj się z właścicielem serwera SMTP, numer portu używanego oczekiwane użycia.</span><span class="sxs-lookup"><span data-stu-id="76fb8-179">Check with the owner of the SMTP server what port number they expect you to use.</span></span>
- <span data-ttu-id="76fb8-180">Upewnij się, że używasz prawidłowych poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="76fb8-180">Make sure that you use the right credentials.</span></span> <span data-ttu-id="76fb8-181">Po opublikowaniu lokacji do dostawcy hostingu, Użyj poświadczeń, które dostawcy specjalnie wskazuje, czy do obsługi poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="76fb8-181">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="76fb8-182">Te poświadczenia mogą się różnić od poświadczeń używanych do opublikowania.</span><span class="sxs-lookup"><span data-stu-id="76fb8-182">These credentials might be different from the credentials you use to publish.</span></span>
- <span data-ttu-id="76fb8-183">Czasami nie potrzebujesz poświadczeń w ogóle.</span><span class="sxs-lookup"><span data-stu-id="76fb8-183">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="76fb8-184">Przy wysyłaniu wiadomości e-mail za pomocą osobistego usługodawca Internetowy, Twój dostawca poczty e-mail może być już wiesz, poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="76fb8-184">If you're sending email by using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="76fb8-185">Po opublikowaniu, może być konieczne przy użyciu innych poświadczeń niż podczas testowania na komputerze lokalnym.</span><span class="sxs-lookup"><span data-stu-id="76fb8-185">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
- <span data-ttu-id="76fb8-186">Jeśli Twój dostawca e-mail używa szyfrowania, ustaw `WebMail.EnableSsl` do `true`.</span><span class="sxs-lookup"><span data-stu-id="76fb8-186">If your email provider uses encryption, set `WebMail.EnableSsl` to `true`.</span></span>

<span data-ttu-id="76fb8-187">W przypadku błąd podczas wysyłania wiadomości e-mail, można napotkać komunikat o błędzie standardowe ASP.NET, która wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="76fb8-187">If there is an error sending email, you might see a standard ASP.NET error message, which looks like this:</span></span>

![Komunikat o błędzie platformy ASP.NET, gdy występuje problem z pocztą e-mail](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

<span data-ttu-id="76fb8-189">Można również debugowania problemów z wysyłaniem wiadomości e-mail przy użyciu `try-catch` bloku, jak w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="76fb8-189">You can also debug problems with sending email by using a `try-catch` block, as in the following example.</span></span> <span data-ttu-id="76fb8-190">Jeśli używasz `try-catch` bloku, program ASP.NET nie są wyświetlane komunikaty jego błąd standardowy.</span><span class="sxs-lookup"><span data-stu-id="76fb8-190">When you use a `try-catch` block, ASP.NET does not display its standard error messages.</span></span> <span data-ttu-id="76fb8-191">Zamiast tego można przechwytywać błąd w `catch` części bloku.</span><span class="sxs-lookup"><span data-stu-id="76fb8-191">Instead, you can capture the error in the `catch` portion of the block.</span></span>

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

<span data-ttu-id="76fb8-192">Zastąp odpowiednimi wartościami dla `your-SMTP-server-name`i tak dalej.</span><span class="sxs-lookup"><span data-stu-id="76fb8-192">Substitute the appropriate values for `your-SMTP-server-name`, and so on.</span></span> <span data-ttu-id="76fb8-193">Komunikaty o błędach, które można napotkać w ten sposób między innymi następujące:</span><span class="sxs-lookup"><span data-stu-id="76fb8-193">Some of the error messages you might see this way include the following:</span></span>

- <span data-ttu-id="76fb8-194">*Błąd podczas wysyłania poczty.*</span><span class="sxs-lookup"><span data-stu-id="76fb8-194">*Failure sending mail.*</span></span>

    <span data-ttu-id="76fb8-195">—lub—</span><span class="sxs-lookup"><span data-stu-id="76fb8-195">-or-</span></span>

    <span data-ttu-id="76fb8-196">*Próba połączenia nie powiodła się, ponieważ strona połączenia nie odpowiada prawidłowo po upływie określonego czasu lub utworzone połączenie nie powiodło się, ponieważ połączony host nie odpowiada*</span><span class="sxs-lookup"><span data-stu-id="76fb8-196">*A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond*</span></span>

    <span data-ttu-id="76fb8-197">Ten błąd oznacza, że aplikacji nie może połączyć się z serwerem SMTP.</span><span class="sxs-lookup"><span data-stu-id="76fb8-197">This error usually means that the application could not connect to the SMTP server.</span></span> <span data-ttu-id="76fb8-198">Sprawdź nazwę serwera i numer portu.</span><span class="sxs-lookup"><span data-stu-id="76fb8-198">Check the server name and port number.</span></span>
- <span data-ttu-id="76fb8-199"><em>Skrzynka pocztowa jest niedostępna. Odpowiedź serwera: 5.1.0 &lt; someuser@invaliddomain &gt; nadawcy odrzucone: nieprawidłowy nadawcy domeny</em></span><span class="sxs-lookup"><span data-stu-id="76fb8-199"><em>Mailbox unavailable. The server response was: 5.1.0 &lt;someuser@invaliddomain&gt; sender rejected : invalid sender domain</em></span></span>

    <span data-ttu-id="76fb8-200">Ten komunikat można wskazać, że `From` adres jest nieprawidłowy lub Brak.</span><span class="sxs-lookup"><span data-stu-id="76fb8-200">This message can indicate that the `From` address is not correct or is missing.</span></span>
- <span data-ttu-id="76fb8-201">*Określony ciąg nie ma formy wymaganej dla adresu e-mail.*</span><span class="sxs-lookup"><span data-stu-id="76fb8-201">*The specified string is not in the form required for an email address.*</span></span>

    <span data-ttu-id="76fb8-202">Ten błąd może oznaczać, że wartość `To` lub `From` właściwości nie są rozpoznawane jako adresy e-mail.</span><span class="sxs-lookup"><span data-stu-id="76fb8-202">This error might indicate that the value of the `To` or `From` properties are not recognized as email addresses.</span></span> <span data-ttu-id="76fb8-203">(Aplikacja ASP.NET nie sprawdza, czy adres e-mail jest prawidłowy tylko jego obiektu w poprawnym formacie tak samo, jak *name@domain.com*.)</span><span class="sxs-lookup"><span data-stu-id="76fb8-203">(ASP.NET cannot check that the email address is valid, only that it's in the correct format, like *name@domain.com*.)</span></span>

> [!NOTE]
> <span data-ttu-id="76fb8-204">Usuń kod znaczników, który zawiera błąd (`@errorMessage`) przed opublikowaniem strony w witrynie na żywo.</span><span class="sxs-lookup"><span data-stu-id="76fb8-204">Remove the markup that displays the error (`@errorMessage`) before you publish the page to a live site.</span></span> <span data-ttu-id="76fb8-205">Nie jest dobrym rozwiązaniem, aby umożliwić użytkownikom wyświetlić komunikaty o błędach, które można uzyskać z serwera.</span><span class="sxs-lookup"><span data-stu-id="76fb8-205">It's not a good idea to let users see error messages that you get from a server.</span></span>


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="76fb8-206">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="76fb8-206">Additional Resources</span></span>

[<span data-ttu-id="76fb8-207">ASP.NET Web Pages (Razor) — często zadawane pytania</span><span class="sxs-lookup"><span data-stu-id="76fb8-207">ASP.NET Web Pages (Razor) FAQ</span></span>](https://go.microsoft.com/fwlink/?LinkId=253000)

<span data-ttu-id="76fb8-208">[Program WebMatrix i stron ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum w witrynie sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="76fb8-208">[WebMatrix and ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum on the ASP.NET website</span></span>
