---
uid: whitepapers/request-validation
title: Żądanie weryfikacji — zapobieganie atakom skryptów | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym dokumencie opisano funkcję weryfikacji żądania programu ASP.NET, where, domyślnie, aplikacja nie będzie mógł przetwarzania niekodowany submitt zawartości HTML...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 087f30428602137e01f574825f3ebcd4db9285ff
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752060"
---
<a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="5b9b3-103">Żądanie weryfikacji — zapobieganie atakom skryptów</span><span class="sxs-lookup"><span data-stu-id="5b9b3-103">Request Validation - Preventing Script Attacks</span></span>
====================
> <span data-ttu-id="5b9b3-104">W tym dokumencie opisano funkcję weryfikacji żądania programu ASP.NET, where, domyślnie, aplikacja nie będzie mógł niekodowany zawartość HTML przesłane do serwera przetwarzania.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="5b9b3-105">Gdy aplikacja została zaprojektowana w celu bezpiecznego przetwarzania danych HTML można wyłączyć tej funkcji sprawdzania poprawności żądania.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="5b9b3-106">Stosuje się do platformy ASP.NET 1.1 i ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>


<span data-ttu-id="5b9b3-107">Weryfikacja żądania, funkcji programu ASP.NET od wersji 1.1, uniemożliwia serwera przyjmowanie zawartości zawierającego usunięcie zakodowanym formacie HTML.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="5b9b3-108">Ta funkcja została zaprojektowana, aby zapobiec atakom niektóre uruchomienie skryptu, zgodnie z którą kod skryptu klienta lub HTML może być nieświadomie przesłane do serwera, przechowywanych i przedstawiony w innym użytkownikom.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="5b9b3-109">Firma Microsoft nadal zdecydowanie zaleca zweryfikowanie wszystkich danych wejściowych i kodowanie HTML, gdy jest to odpowiednie.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="5b9b3-110">Na przykład możesz utworzyć stronę sieci Web, który żąda adresu e-mail użytkownika, a następnie magazynów, które adres e-mail w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="5b9b3-111">Jeśli użytkownik wprowadzi &lt;skryptu&gt;alertu ("Witaj, ze skryptu")&lt;/SCRIPT&gt; zamiast prawidłowy adres e-mail, umieszczeniem danych ten skrypt można wykonać Jeśli zawartość nie została poprawnie zaszyfrowana.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="5b9b3-112">Funkcję weryfikacji żądania programu ASP.NET uniemożliwia to występuje.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="5b9b3-113">Dlaczego ta funkcja jest przydatna</span><span class="sxs-lookup"><span data-stu-id="5b9b3-113">Why this feature is useful</span></span>

<span data-ttu-id="5b9b3-114">Wiele witryn nie są świadomi, że są one otwarte na ataki przez iniekcję kodu prosty skrypt.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="5b9b3-115">Celem tych ataków zamazać witryny, wyświetlając HTML lub potencjalnie wykonywanie skryptu klienta, aby przekierować użytkownika do witryny haker, czy ataki przez iniekcję kodu skryptu są problemem i deweloperów sieci Web muszą zmagać się z.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="5b9b3-116">Ataki przez iniekcję kodu skryptu są dotyczą wszystkich deweloperów sieci web, czy używasz platformy ASP.NET, ASP lub innych technologii sieci web.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="5b9b3-117">Funkcję weryfikacji żądania ASP.NET aktywnie zapobiega atakom przez nie zezwala na niekodowany zawartość HTML do przetworzenia przez serwer, chyba że deweloper zdecyduje się zezwolić na tej zawartości.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="5b9b3-118">Czego można oczekiwać: strona błędu</span><span class="sxs-lookup"><span data-stu-id="5b9b3-118">What to expect: Error Page</span></span>

<span data-ttu-id="5b9b3-119">Poniższym zrzucie ekranu przedstawiono przykładowy kod platformy ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="5b9b3-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="5b9b3-120">Używany kod powoduje to prosta strona, która pozwala na wprowadź jakiś tekst w polu tekstowym, kliknij przycisk, a następnie wyświetlić tekst w kontrolce etykiety:</span><span class="sxs-lookup"><span data-stu-id="5b9b3-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="5b9b3-121">Jednak były JavaScript, takie jak `<script>alert("hello!")</script>` wprowadzono i przesyłanych, otrzymamy wynik wyjątek:</span><span class="sxs-lookup"><span data-stu-id="5b9b3-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="5b9b3-122">Komunikat o błędzie stwierdzający, że "potencjalnie niebezpiecznych Request.Form wykryto wartość" i zawiera więcej szczegółowych informacji w opisie dokładnie co wydarzyło i jak zmienić zachowanie.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="5b9b3-123">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="5b9b3-123">For example:</span></span>

<span data-ttu-id="5b9b3-124">Sprawdzania poprawności żądania wykryto klienta potencjalnie niebezpiecznych wartości wejściowej, a przetwarzanie żądania zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="5b9b3-125">Ta wartość może wskazywać na próbę naruszenia zabezpieczeń aplikacji, takich jak atak skryptów między witrynami.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="5b9b3-126">Wyłączono weryfikację żądań, ustawiając `validateRequest=false` w dyrektywie strony lub w sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="5b9b3-127">Jednak zdecydowanie zalecane jest, aby aplikacja jawnie sprawdziła wszystkie dane wejściowe w tym przypadku.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="5b9b3-128">Wyłączanie weryfikacji żądania na stronie</span><span class="sxs-lookup"><span data-stu-id="5b9b3-128">Disabling request validation on a page</span></span>

<span data-ttu-id="5b9b3-129">Aby wyłączyć sprawdzanie poprawności żądań na stronie należy ustawić `validateRequest` atrybutu dyrektywy strony `false`:</span><span class="sxs-lookup"><span data-stu-id="5b9b3-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="5b9b3-130">Jeśli weryfikacja żądania jest wyłączone, zawartość można przesyłać do strony jest odpowiedzialność deweloperów strony, aby upewnić się, że zawartość jest poprawnie zakodowany lub przetworzone.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="5b9b3-131">Wyłączanie weryfikacji żądań aplikacji</span><span class="sxs-lookup"><span data-stu-id="5b9b3-131">Disabling request validation for your application</span></span>

<span data-ttu-id="5b9b3-132">Aby wyłączyć weryfikację żądań dla aplikacji, musisz zmodyfikować lub tworzy plik Web.config dla swojej aplikacji i ustawić atrybut parametr validateRequest `<pages />` sekcji `false`:</span><span class="sxs-lookup"><span data-stu-id="5b9b3-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="5b9b3-133">Jeśli chcesz wyłączyć weryfikację żądań dla wszystkich aplikacji na serwerze, można wprowadzić modyfikacji do pliku Machine.config.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="5b9b3-134">Jeśli weryfikacja żądania jest wyłączone, zawartość można przesyłać do aplikacji; jest odpowiedzialność deweloperów aplikacji, aby upewnić się, że zawartość jest poprawnie zakodowany lub przetworzone.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="5b9b3-135">Poniższy kod jest modyfikowany, aby wyłączyć weryfikację żądań:</span><span class="sxs-lookup"><span data-stu-id="5b9b3-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="5b9b3-136">Jeśli następujący kod JavaScript został wprowadzony w polu tekstowym teraz `<script>alert("hello!")</script>` wynik byłby:</span><span class="sxs-lookup"><span data-stu-id="5b9b3-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="5b9b3-137">Aby temu zapobiec, z weryfikacji żądań wyłączony, należy w formacie HTML kodujemy zawartości.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="5b9b3-138">Jak w formacie HTML kodowanie zawartości</span><span class="sxs-lookup"><span data-stu-id="5b9b3-138">How to HTML encode content</span></span>

<span data-ttu-id="5b9b3-139">Wyłączenie Weryfikacja żądania jest dobrze zawartości kodowanie HTML, które będą przechowywane do użytku w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="5b9b3-140">Kodowanie HTML automatycznie spowoduje zastąpienie dowolnego "&lt;"lub"&gt;" (wraz z kilku innych symboli) przy użyciu ich kod HTML zakodowane reprezentacji.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="5b9b3-141">Na przykład "&lt;"zastępuje"&amp;lt;" i "&gt;"zastępuje"&amp;gt;".</span><span class="sxs-lookup"><span data-stu-id="5b9b3-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="5b9b3-142">Przeglądarki używają tych specjalne kody do wyświetlenia "&lt;"lub"&gt;" w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="5b9b3-143">Zawartość może być łatwo kodowany w formacie HTML na serwerze za pomocą `Server.HtmlEncode(string)` interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="5b9b3-144">Zawartość można też łatwo HTML-zdekodować, oznacza to, że przywróconymi do standardowego HTML za pomocą `Server.HtmlDecode(string)` metody.</span><span class="sxs-lookup"><span data-stu-id="5b9b3-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="5b9b3-145">Wynikiem:</span><span class="sxs-lookup"><span data-stu-id="5b9b3-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
