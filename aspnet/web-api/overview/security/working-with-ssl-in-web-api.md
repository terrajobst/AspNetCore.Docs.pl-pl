---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: "Praca z protokołem SSL w składniku Web API | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Przedstawia sposób użycia protokołu SSL z interfejsu API sieci Web ASP.NET, w tym o korzystaniu z certyfikatów SSL klienta."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 8c631900c8c5ab6097e0cb9fd4a71abbcba1c88b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="working-with-ssl-in-web-api"></a><span data-ttu-id="6f999-103">Praca z protokołem SSL w składniku Web API</span><span class="sxs-lookup"><span data-stu-id="6f999-103">Working with SSL in Web API</span></span>
====================
<span data-ttu-id="6f999-104">przez [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6f999-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="6f999-105">Kilka wspólnych schematów uwierzytelniania nie są bezpieczne za pośrednictwem zwykłego protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="6f999-105">Several common authentication schemes are not secure over plain HTTP.</span></span> <span data-ttu-id="6f999-106">W szczególności uwierzytelnianie podstawowe i uwierzytelnianie oparte na formularzach Wyślij niezaszyfrowane poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="6f999-106">In particular, Basic authentication and forms authentication send unencrypted credentials.</span></span> <span data-ttu-id="6f999-107">Do zabezpieczenia, te schematy uwierzytelniania *musi* używają protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="6f999-107">To be secure, these authentication schemes *must* use SSL.</span></span> <span data-ttu-id="6f999-108">Ponadto certyfikaty SSL klienta może służyć do uwierzytelniania klientów.</span><span class="sxs-lookup"><span data-stu-id="6f999-108">In addition, SSL client certificates can be used to authenticate clients.</span></span>

## <a name="enabling-ssl-on-the-server"></a><span data-ttu-id="6f999-109">Włączanie protokołu SSL na serwerze</span><span class="sxs-lookup"><span data-stu-id="6f999-109">Enabling SSL on the Server</span></span>

<span data-ttu-id="6f999-110">Aby skonfigurować protokół SSL w usługach IIS 7 lub nowszy:</span><span class="sxs-lookup"><span data-stu-id="6f999-110">To set up SSL in IIS 7 or later:</span></span>

- <span data-ttu-id="6f999-111">Utwórz lub uzyskać certyfikat.</span><span class="sxs-lookup"><span data-stu-id="6f999-111">Create or get a certificate.</span></span> <span data-ttu-id="6f999-112">Do testowania, można utworzyć certyfikatu z podpisem własnym.</span><span class="sxs-lookup"><span data-stu-id="6f999-112">For testing, you can create a self-signed certificate.</span></span>
- <span data-ttu-id="6f999-113">Dodaj powiązanie HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6f999-113">Add an HTTPS binding.</span></span>

<span data-ttu-id="6f999-114">Aby uzyskać więcej informacji, zobacz [jak się protokołu SSL w usługach IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span><span class="sxs-lookup"><span data-stu-id="6f999-114">For details, see [How to Set Up SSL on IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).</span></span>

<span data-ttu-id="6f999-115">Do testowania lokalnego, można włączyć protokołu SSL w usługach IIS Express z programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6f999-115">For local testing, you can enable SSL in IIS Express from Visual Studio.</span></span> <span data-ttu-id="6f999-116">W oknie właściwości ustaw **włączony protokół SSL** do **True**.</span><span class="sxs-lookup"><span data-stu-id="6f999-116">In the Properties window, set **SSL Enabled** to **True**.</span></span> <span data-ttu-id="6f999-117">Zanotuj wartość ustawienia **adres URL protokołu SSL**; Użyj tego adresu URL do testowania połączenia HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6f999-117">Note the value of **SSL URL**; use this URL for testing HTTPS connections.</span></span>

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a><span data-ttu-id="6f999-118">Wymuszanie protokołu SSL w kontrolerze interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="6f999-118">Enforcing SSL in a Web API Controller</span></span>

<span data-ttu-id="6f999-119">Jeśli masz zarówno HTTPS i powiązanie HTTP, klienci mogą nadal używać HTTP dostęp do witryny.</span><span class="sxs-lookup"><span data-stu-id="6f999-119">If you have both an HTTPS and an HTTP binding, clients can still use HTTP to access the site.</span></span> <span data-ttu-id="6f999-120">Mogą zezwalać niektórych zasobów był dostępny za pośrednictwem protokołu HTTP, podczas gdy inne zasoby wymagają protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="6f999-120">You might allow some resources to be available through HTTP, while other resources require SSL.</span></span> <span data-ttu-id="6f999-121">W takim przypadku użyj filtru akcji, aby wymagać protokołu SSL do chronionych zasobów.</span><span class="sxs-lookup"><span data-stu-id="6f999-121">In that case, use an action filter to require SSL for the protected resources.</span></span> <span data-ttu-id="6f999-122">Poniższy kod przedstawia filtr uwierzytelniania interfejsu API sieci Web, który sprawdza, czy protokół SSL:</span><span class="sxs-lookup"><span data-stu-id="6f999-122">The following code shows a Web API authentication filter that checks for SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

<span data-ttu-id="6f999-123">Dodaj filtr do wszystkich akcji interfejsu API sieci Web, które wymagają protokołu SSL:</span><span class="sxs-lookup"><span data-stu-id="6f999-123">Add this filter to any Web API actions that require SSL:</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a><span data-ttu-id="6f999-124">Certyfikaty SSL klienta</span><span class="sxs-lookup"><span data-stu-id="6f999-124">SSL Client Certificates</span></span>

<span data-ttu-id="6f999-125">Protokół SSL umożliwia uwierzytelnianie za pomocą certyfikatów infrastruktury kluczy publicznych.</span><span class="sxs-lookup"><span data-stu-id="6f999-125">SSL provides authentication by using Public Key Infrastructure certificates.</span></span> <span data-ttu-id="6f999-126">Serwer musi dostarczyć certyfikat, który służy do uwierzytelniania serwera do klienta.</span><span class="sxs-lookup"><span data-stu-id="6f999-126">The server must provide a certificate that authenticates the server to the client.</span></span> <span data-ttu-id="6f999-127">Mniej często od klienta zapewnienia certyfikatu na serwerze, ale jest jedną z opcji uwierzytelniania klientów.</span><span class="sxs-lookup"><span data-stu-id="6f999-127">It is less common for the client to provide a certificate to the server, but this is one option for authenticating clients.</span></span> <span data-ttu-id="6f999-128">Aby używać certyfikatów klienta przy użyciu protokołu SSL, należy rozdystrybuować podpisanych certyfikatów użytkowników.</span><span class="sxs-lookup"><span data-stu-id="6f999-128">To use client certificates with SSL, you need a way to distribute signed certificates to your users.</span></span> <span data-ttu-id="6f999-129">Dla wielu typów aplikacji nie będą poprawne działanie, ale w niektórych środowiskach (na przykład dla wersji enterprise) może być możliwe.</span><span class="sxs-lookup"><span data-stu-id="6f999-129">For many application types, this will not be a good user experience, but in some environments (for example, enterprise) it may be feasible.</span></span>

| <span data-ttu-id="6f999-130">Zalety</span><span class="sxs-lookup"><span data-stu-id="6f999-130">Advantages</span></span> | <span data-ttu-id="6f999-131">Wady</span><span class="sxs-lookup"><span data-stu-id="6f999-131">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="6f999-132">-Certyfikatu poświadczenia są mocniejszy niż element nazwy użytkownika i hasła.</span><span class="sxs-lookup"><span data-stu-id="6f999-132">- Certificate credentials are stronger than username/password.</span></span> <span data-ttu-id="6f999-133">-SSL zawiera pełną bezpiecznego kanału z uwierzytelnianiem, integralności i szyfrowania wiadomości.</span><span class="sxs-lookup"><span data-stu-id="6f999-133">- SSL provides a complete secure channel, with authentication, message integrity, and message encryption.</span></span> | <span data-ttu-id="6f999-134">-Należy uzyskać i zarządzać certyfikatami infrastruktury kluczy publicznych.</span><span class="sxs-lookup"><span data-stu-id="6f999-134">- You must obtain and manage PKI certificates.</span></span> <span data-ttu-id="6f999-135">Platform klient musi obsługiwać certyfikaty SSL klienta.</span><span class="sxs-lookup"><span data-stu-id="6f999-135">- The client platform must support SSL client certificates.</span></span> |

<span data-ttu-id="6f999-136">Aby skonfigurować usługi IIS, aby akceptować certyfikaty klienta, otwórz Menedżera usług IIS i wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="6f999-136">To configure IIS to accept client certificates, open IIS Manager and perform the following steps:</span></span>

1. <span data-ttu-id="6f999-137">Kliknij węzeł witryny w widoku drzewa.</span><span class="sxs-lookup"><span data-stu-id="6f999-137">Click the site node in the tree view.</span></span>
2. <span data-ttu-id="6f999-138">Kliknij dwukrotnie **ustawienia protokołu SSL** funkcji w środkowym okienku.</span><span class="sxs-lookup"><span data-stu-id="6f999-138">Double-click the **SSL Settings** feature in the middle pane.</span></span>
3. <span data-ttu-id="6f999-139">W obszarze **certyfikaty klienta**, wybierz jedną z następujących opcji:</span><span class="sxs-lookup"><span data-stu-id="6f999-139">Under **Client Certificates**, select one of these options:</span></span> 

    - <span data-ttu-id="6f999-140">**Zaakceptuj**: IIS będzie akceptować certyfikat od klienta, ale nie wymaga jednego.</span><span class="sxs-lookup"><span data-stu-id="6f999-140">**Accept**: IIS will accept a certificate from the client, but does not require one.</span></span>
    - <span data-ttu-id="6f999-141">**Wymagaj**: wymagany certyfikat klienta.</span><span class="sxs-lookup"><span data-stu-id="6f999-141">**Require**: Require a client certificate.</span></span> <span data-ttu-id="6f999-142">(Aby włączyć tę opcję, należy również wybrać opcję "Wymagaj protokołu SSL")</span><span class="sxs-lookup"><span data-stu-id="6f999-142">(To enable this option, you must also select "Require SSL")</span></span>

<span data-ttu-id="6f999-143">W pliku ApplicationHost.config, można również ustawić następujące opcje:</span><span class="sxs-lookup"><span data-stu-id="6f999-143">You can also set these options in the ApplicationHost.config file:</span></span>

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

<span data-ttu-id="6f999-144">**SslNegotiateCert** flagi oznacza, że usługi IIS będzie akceptować certyfikat od klienta, ale nie wymaga jednego (odpowiada opcji "Zaakceptuj" w Menedżerze usług IIS).</span><span class="sxs-lookup"><span data-stu-id="6f999-144">The **SslNegotiateCert** flag means IIS will accept a certificate from the client, but does not require one (equivalent to the "Accept" option in IIS Manager).</span></span> <span data-ttu-id="6f999-145">Aby wymagać certyfikatu, należy ustawić **SslRequireCert** flagi.</span><span class="sxs-lookup"><span data-stu-id="6f999-145">To require a certificate, set the **SslRequireCert** flag.</span></span> <span data-ttu-id="6f999-146">Do testowania, można również ustawić te opcje w usługach IIS Express, w lokalnym hosta aplikacji. Plik konfiguracji znajduje się w "Documents\IISExpress\config".</span><span class="sxs-lookup"><span data-stu-id="6f999-146">For testing, you can also set these options in IIS Express, in the local applicationhost.Config file, located in "Documents\IISExpress\config".</span></span>

### <a name="creating-a-client-certificate-for-testing"></a><span data-ttu-id="6f999-147">Tworzenie certyfikatu klienta do testowania</span><span class="sxs-lookup"><span data-stu-id="6f999-147">Creating a Client Certificate for Testing</span></span>

<span data-ttu-id="6f999-148">Do celów testowych, można użyć [MakeCert.exe](https://msdn.microsoft.com/en-US/library/bfsktky3.aspx) można utworzyć certyfikatu klienta.</span><span class="sxs-lookup"><span data-stu-id="6f999-148">For testing purposes, you can use [MakeCert.exe](https://msdn.microsoft.com/en-US/library/bfsktky3.aspx) to create a client certificate.</span></span> <span data-ttu-id="6f999-149">Najpierw utwórz urząd główny testu:</span><span class="sxs-lookup"><span data-stu-id="6f999-149">First, create a test root authority:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

<span data-ttu-id="6f999-150">MakeCert wyświetli monit o wprowadzenie hasła dla klucza prywatnego.</span><span class="sxs-lookup"><span data-stu-id="6f999-150">Makecert will prompt you to enter a password for the private key.</span></span>

<span data-ttu-id="6f999-151">Następnie Dodaj certyfikat do badania serwera "Zaufane główne urzędy certyfikacji" przechowywania w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="6f999-151">Next, add the certificate to the test server's "Trusted Root Certification Authorities" store, as follows:</span></span>

1. <span data-ttu-id="6f999-152">Otwórz program MMC.</span><span class="sxs-lookup"><span data-stu-id="6f999-152">Open MMC.</span></span>
2. <span data-ttu-id="6f999-153">W obszarze **pliku**, wybierz pozycję **Dodaj/Usuń przystawkę**.</span><span class="sxs-lookup"><span data-stu-id="6f999-153">Under **File**, select **Add/Remove Snap-In**.</span></span>
3. <span data-ttu-id="6f999-154">Wybierz **konto komputera**.</span><span class="sxs-lookup"><span data-stu-id="6f999-154">Select **Computer Account**.</span></span>
4. <span data-ttu-id="6f999-155">Wybierz **komputera lokalnego** i Zakończ pracę kreatora.</span><span class="sxs-lookup"><span data-stu-id="6f999-155">Select **Local computer** and complete the wizard.</span></span>
5. <span data-ttu-id="6f999-156">W okienku nawigacji rozwiń węzeł "Zaufane główne urzędy certyfikacji".</span><span class="sxs-lookup"><span data-stu-id="6f999-156">Under the navigation pane, expand the "Trusted Root Certification Authorities" node.</span></span>
6. <span data-ttu-id="6f999-157">Na **akcji** menu wskaż **wszystkie zadania**, a następnie kliknij przycisk **importu** można uruchomić Kreatora importu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="6f999-157">On the **Action** menu, point to **All Tasks**, and then click **Import** to start the Certificate Import Wizard.</span></span>
7. <span data-ttu-id="6f999-158">Przejdź do pliku certyfikatu TempCA.cer.</span><span class="sxs-lookup"><span data-stu-id="6f999-158">Browse to the certificate file, TempCA.cer.</span></span>
8. <span data-ttu-id="6f999-159">Kliknij przycisk **Otwórz**, następnie kliknij przycisk **dalej** i Zakończ pracę kreatora.</span><span class="sxs-lookup"><span data-stu-id="6f999-159">Click **Open**, then click **Next** and complete the wizard.</span></span> <span data-ttu-id="6f999-160">(Pojawi się monit o ponowne wprowadzenie hasła.)</span><span class="sxs-lookup"><span data-stu-id="6f999-160">(You will be prompted to re-enter the password.)</span></span>

<span data-ttu-id="6f999-161">Teraz należy utworzyć certyfikat klienta, który jest podpisany przez pierwszy certyfikat:</span><span class="sxs-lookup"><span data-stu-id="6f999-161">Now create a client certificate that is signed by the first certificate:</span></span>

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a><span data-ttu-id="6f999-162">W składniku Web API przy użyciu certyfikatów klienta</span><span class="sxs-lookup"><span data-stu-id="6f999-162">Using Client Certificates in Web API</span></span>

<span data-ttu-id="6f999-163">Po stronie serwera można uzyskać certyfikatu klienta, wywołując [GetClientCertificate](https://msdn.microsoft.com/en-us/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) komunikatu żądania.</span><span class="sxs-lookup"><span data-stu-id="6f999-163">On the server side, you can get the client certificate by calling [GetClientCertificate](https://msdn.microsoft.com/en-us/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) on the request message.</span></span> <span data-ttu-id="6f999-164">Metoda zwraca wartość null, jeśli certyfikat klienta.</span><span class="sxs-lookup"><span data-stu-id="6f999-164">The method returns null if there is no client certificate.</span></span> <span data-ttu-id="6f999-165">W przeciwnym razie zwraca **X509Certificate2** wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="6f999-165">Otherwise, it returns an **X509Certificate2** instance.</span></span> <span data-ttu-id="6f999-166">Aby uzyskać informacje z certyfikatu, takie jak wystawcy i podmiotu, użyj tego obiektu.</span><span class="sxs-lookup"><span data-stu-id="6f999-166">Use this object to get information from the certificate, such as the issuer and subject.</span></span> <span data-ttu-id="6f999-167">Następnie można użyć tych informacji do uwierzytelniania i/lub autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="6f999-167">Then you can use this information for authentication and/or authorization.</span></span>

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
