---
uid: whitepapers/request-validation
title: "Żądanie sprawdzania poprawności - zapobieganie atakom skryptu | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym dokumencie opisano funkcję sprawdzania poprawności żądania programu ASP.NET, w którym, domyślnie aplikacji nie będzie mógł przetwarzania niekodowany submitt zawartości HTML..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0b24fe2193d2c7a858667505bad9ed0b1d70a328
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
<a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="7e332-103">Żądanie sprawdzania poprawności - zapobieganie atakom skryptu</span><span class="sxs-lookup"><span data-stu-id="7e332-103">Request Validation - Preventing Script Attacks</span></span>
====================
> <span data-ttu-id="7e332-104">W tym dokumencie opisano funkcję sprawdzania poprawności żądania programu ASP.NET, w którym, domyślnie aplikacji nie będzie mógł przetwarzania niekodowany zawartość HTML przesłane do serwera.</span><span class="sxs-lookup"><span data-stu-id="7e332-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="7e332-105">Gdy aplikacja zostało zaprojektowane do bezpiecznego przetwarzania danych HTML można wyłączyć tej funkcji sprawdzania poprawności żądania.</span><span class="sxs-lookup"><span data-stu-id="7e332-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="7e332-106">Dotyczy ASP.NET 1.1 i programu ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="7e332-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>


<span data-ttu-id="7e332-107">Weryfikacja żądania, funkcji programu ASP.NET od wersji 1.1, zapobiega akceptowanie zawartości zawierającego HTML bez zakodowanego przez serwer.</span><span class="sxs-lookup"><span data-stu-id="7e332-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="7e332-108">Ta funkcja została zaprojektowana, aby zapobiec atakom niektórych uruchomienie skryptu, zgodnie z którymi kodu skryptu klienta lub HTML może być nieświadomie przesłać na serwer, przechowywane i następnie widoczne dla innych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="7e332-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="7e332-109">Nadal zdecydowanie zaleca się zweryfikowanie wszystkich danych wejściowych, a kodowanie HTML, gdy jest to odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="7e332-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="7e332-110">Na przykład można utworzyć strony sieci Web, który żąda adresu e-mail użytkownika, a następnie zapisuje, które adres e-mail w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7e332-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="7e332-111">Jeśli użytkownik wprowadzi &lt;skryptu&gt;alert ("Witaj, skryptu")&lt;/SCRIPT&gt; zamiast prawidłowy adres e-mail, gdy dane są prezentowane, ten skrypt mogą być wykonywane, jeśli zawartość nie została poprawnie zaszyfrowana.</span><span class="sxs-lookup"><span data-stu-id="7e332-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="7e332-112">Funkcja sprawdzania poprawności żądania programu ASP.NET zapobiega to zapobiec.</span><span class="sxs-lookup"><span data-stu-id="7e332-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="7e332-113">Dlaczego ta funkcja jest przydatna</span><span class="sxs-lookup"><span data-stu-id="7e332-113">Why this feature is useful</span></span>

<span data-ttu-id="7e332-114">Wiele witryn nie są znane, że są one otwarte na ataki iniekcji prostego skryptu.</span><span class="sxs-lookup"><span data-stu-id="7e332-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="7e332-115">Celem takiego ataku jest zamazać lokacji za pomocą wyświetlania HTML lub potencjalnie uruchomienia skryptu klienta w celu przekierowanie użytkownika do witryny to hakerom, ataki uruchomienie skryptu czy problem, który deweloperów sieci Web musi będą konkurować o.</span><span class="sxs-lookup"><span data-stu-id="7e332-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="7e332-116">Ataki uruchomienie skryptu są dotyczą wszystkich deweloperów sieci web, czy używają ASP.NET, ASP lub inne technologie projektowania sieci web.</span><span class="sxs-lookup"><span data-stu-id="7e332-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="7e332-117">Funkcję weryfikacji żądania ASP.NET aktywnego uniemożliwia ataki, nie zezwalając niekodowany zawartość HTML do przetworzenia przez serwer, chyba że Deweloper decyduje zezwolić tej zawartości.</span><span class="sxs-lookup"><span data-stu-id="7e332-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="7e332-118">Czego można oczekiwać: strona błędu</span><span class="sxs-lookup"><span data-stu-id="7e332-118">What to expect: Error Page</span></span>

<span data-ttu-id="7e332-119">Ekranu zrzut poniżej przedstawiono niektóre przykładowy kod ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="7e332-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="7e332-120">Uruchomiona wyniki tego kodu w to prosta strona, która pozwala na wprowadzenie tekstu w polu tekstowym, kliknij przycisk i wyświetlanie tekstu w formancie etykiety:</span><span class="sxs-lookup"><span data-stu-id="7e332-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="7e332-121">Jednak były JavaScript, takich jak `<script>alert("hello!")</script>` wprowadzona i przesyłanych może pobrać wyjątek:</span><span class="sxs-lookup"><span data-stu-id="7e332-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="7e332-122">Komunikat o błędzie stwierdza, że "potencjalnie niebezpiecznych Request.Form wykryto wartość" i zawiera więcej szczegółów w opisie dokładnie zaistniałych sytuacji oraz sposobu zmiany zachowania.</span><span class="sxs-lookup"><span data-stu-id="7e332-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="7e332-123">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="7e332-123">For example:</span></span>

<span data-ttu-id="7e332-124">Sprawdzania poprawności żądania wykryto potencjalnie niebezpieczną klienta wartości wejściowej, a przetwarzanie żądania zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="7e332-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="7e332-125">Ta wartość może wskazywać próbę naruszenia zabezpieczeń aplikacji, takich jak atak skryptowy między witrynami.</span><span class="sxs-lookup"><span data-stu-id="7e332-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="7e332-126">Sprawdzanie poprawności żądań można wyłączyć, ustawiając `validateRequest=false` w dyrektywie strony lub w sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7e332-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="7e332-127">Jednak zdecydowanie zalecane jest, aby aplikacja jawnie sprawdziła wszystkie dane wejściowe w takim przypadku.</span><span class="sxs-lookup"><span data-stu-id="7e332-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="7e332-128">Wyłączenie sprawdzania poprawności żądania na stronie</span><span class="sxs-lookup"><span data-stu-id="7e332-128">Disabling request validation on a page</span></span>

<span data-ttu-id="7e332-129">Aby wyłączyć sprawdzanie poprawności żądań na stronie, musisz ustawić `validateRequest` atrybutu dyrektywy strony `false`:</span><span class="sxs-lookup"><span data-stu-id="7e332-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="7e332-130">Jeśli weryfikacja żądania jest wyłączone, zawartość może zostać przesłane do strony; jest odpowiedzialność developer strony, aby upewnić się, że zawartość jest poprawnie zakodowany lub przetworzone.</span><span class="sxs-lookup"><span data-stu-id="7e332-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="7e332-131">Wyłączenie sprawdzania poprawności żądania aplikacji</span><span class="sxs-lookup"><span data-stu-id="7e332-131">Disabling request validation for your application</span></span>

<span data-ttu-id="7e332-132">Aby wyłączyć weryfikację żądań dla aplikacji, należy zmodyfikować lub tworzenie pliku Web.config aplikacji i ustawić atrybut parametr validateRequest `<pages />` sekcji do `false`:</span><span class="sxs-lookup"><span data-stu-id="7e332-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="7e332-133">Jeśli chcesz wyłączyć weryfikację żądań dla wszystkich aplikacji na serwerze, istnieje możliwość modyfikacji do pliku Machine.config.</span><span class="sxs-lookup"><span data-stu-id="7e332-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="7e332-134">Jeśli weryfikacja żądania jest wyłączone, zawartość może zostać przesłane do aplikacji; jest obowiązkiem Deweloper aplikacji, aby upewnić się, że zawartość jest poprawnie zakodowany lub przetworzone.</span><span class="sxs-lookup"><span data-stu-id="7e332-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="7e332-135">Poniższy kod zmienia się, aby wyłączyć sprawdzanie poprawności żądań:</span><span class="sxs-lookup"><span data-stu-id="7e332-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="7e332-136">Teraz, jeśli następujące JavaScript została wprowadzona w polu tekstowym `<script>alert("hello!")</script>` będą:</span><span class="sxs-lookup"><span data-stu-id="7e332-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="7e332-137">Aby temu zapobiec, z weryfikacji żądań wyłączone, firma Microsoft potrzebne do formatu HTML kodowania zawartości.</span><span class="sxs-lookup"><span data-stu-id="7e332-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="7e332-138">Jak HTML kodowanie zawartości</span><span class="sxs-lookup"><span data-stu-id="7e332-138">How to HTML encode content</span></span>

<span data-ttu-id="7e332-139">Wyłączenie Weryfikacja żądania jest dobrym rozwiązaniem kodowanie HTML zawartości, które będą przechowywane do użytku w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="7e332-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="7e332-140">Kodowanie HTML automatycznie zastąpią wszelkie "&lt;"lub"&gt;" (wraz z kilku innych symboli) z ich odpowiednich HTML zakodowane reprezentacji.</span><span class="sxs-lookup"><span data-stu-id="7e332-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="7e332-141">Na przykład "&lt;"zastępuje"&amp;lt;" i "&gt;"zastępuje"&amp;gt;".</span><span class="sxs-lookup"><span data-stu-id="7e332-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="7e332-142">Przeglądarki używają tych specjalne kody do wyświetlenia "&lt;"lub"&gt;" w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="7e332-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="7e332-143">Zawartość może być łatwo kodowany w formacie HTML przy użyciu serwera `Server.HtmlEncode(string)` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="7e332-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="7e332-144">Zawartość można też łatwo HTML-zdekodowany, oznacza to, przywrócić ponownie przy użyciu standardowego kodu HTML `Server.HtmlDecode(string)` metody.</span><span class="sxs-lookup"><span data-stu-id="7e332-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="7e332-145">Wynikiem:</span><span class="sxs-lookup"><span data-stu-id="7e332-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
