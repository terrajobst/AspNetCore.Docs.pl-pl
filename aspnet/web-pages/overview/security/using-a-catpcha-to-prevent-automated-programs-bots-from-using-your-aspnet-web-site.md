---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Korzystanie z mechanizmu CAPTCHA, aby uniemożliwić Boty przy użyciu usługi sieci Web platformy ASP.NET Razor) lokacji | Dokumentacja firmy Microsoft
author: microsoft
description: W tym artykule wyjaśniono, jak użyć ReCaptcha (miara zabezpieczeń), aby uniemożliwić automatyczne programom (robotom) z wykonywania zadań w stron ASP.NET Web Pages (Razor) firma Microsoft...
ms.author: aspnetcontent
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: f67eb60c23e0eec46089ceea9b04779492dfa15e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803074"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a><span data-ttu-id="41189-103">Korzystanie z mechanizmu CAPTCHA, aby uniemożliwić Boty przy użyciu usługi sieci Web platformy ASP.NET Razor) lokacji</span><span class="sxs-lookup"><span data-stu-id="41189-103">Using a CAPTCHA to Prevent Bots from Using Your ASP.NET Web Razor) Site</span></span>
====================
<span data-ttu-id="41189-104">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="41189-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="41189-105">W tym artykule wyjaśniono, jak użyć ReCaptcha (miara zabezpieczeń), aby uniemożliwić automatyczne programom (robotom) z wykonywania zadań w witrynie internetowej ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="41189-105">This article explains how to use ReCaptcha (a security measure) to prevent automated programs (bots) from performing tasks in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="41189-106">**Zawartość:**</span><span class="sxs-lookup"><span data-stu-id="41189-106">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="41189-107">Jak dodać CAPTCHA test do witryny.</span><span class="sxs-lookup"><span data-stu-id="41189-107">How to add a CAPTCHA test to your site.</span></span>
> 
> <span data-ttu-id="41189-108">Poniżej przedstawiono funkcje platformy ASP.NET, wprowadzona w artykule:</span><span class="sxs-lookup"><span data-stu-id="41189-108">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="41189-109">`ReCaptcha` Pomocnika.</span><span class="sxs-lookup"><span data-stu-id="41189-109">The `ReCaptcha` helper.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="41189-110">Informacje przedstawione w tym artykule dotyczy 1.0 stron sieci Web platformy ASP.NET i Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="41189-110">The information in this article applies to ASP.NET Web Pages 1.0 and Web Pages 2.</span></span>


## <a name="about-captchas"></a><span data-ttu-id="41189-111">Temat CAPTCHAs</span><span class="sxs-lookup"><span data-stu-id="41189-111">About CAPTCHAs</span></span>

<span data-ttu-id="41189-112">Ilekroć umożliwiają rejestrowanie osób w lokacji lub nawet po prostu wprowadź nazwę i adres URL (na przykład dla komentarza blog), możesz otrzymać nadmiernej ilości fałszywych nazwy.</span><span class="sxs-lookup"><span data-stu-id="41189-112">Any time you let people register in your site, or even just enter a name and URL (like for a blog comment), you might get a flood of fake names.</span></span> <span data-ttu-id="41189-113">Pozostają one często przez automatyczne programom (robotom), których podejmowana jest próba pozostaw adresy URL w każdej witryny sieci Web, którą można znaleźć.</span><span class="sxs-lookup"><span data-stu-id="41189-113">These are often left by automated programs (bots) that try to leave URLs in every website they can find.</span></span> <span data-ttu-id="41189-114">(Typowe motywacja jest do opublikowania adresów URL produktów do sprzedaży).</span><span class="sxs-lookup"><span data-stu-id="41189-114">(A common motivation is to post the URLs of products for sale.)</span></span>

<span data-ttu-id="41189-115">Możesz pomóc, upewnij się, że użytkownik jest prawdziwa osoba, a nie program komputerowy przy użyciu *CAPTCHA* do weryfikowania użytkowników podczas rejestrowania, lub w przeciwnym razie podał swoją nazwę i witryny.</span><span class="sxs-lookup"><span data-stu-id="41189-115">You can help make sure that a user is real person and not a computer program by using a *CAPTCHA* to validate users when they register or otherwise enter their name and site.</span></span> <span data-ttu-id="41189-116">CAPTCHA oznacza całkowicie zautomatyzowanego publicznych Turing test, aby stwierdzić, komputerów i ludzie od siebie.</span><span class="sxs-lookup"><span data-stu-id="41189-116">CAPTCHA stands for Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="41189-117">Jest CAPTCHA *odpowiedź na żądanie* test w którym użytkownik jest proszony o czymś, który umożliwia łatwe do osoby w celu ale trudne automatycznych programu w celu.</span><span class="sxs-lookup"><span data-stu-id="41189-117">A CAPTCHA is a *challenge-response* test in which the user is asked to do something that is easy for a person to do but hard for an automated program to do.</span></span> <span data-ttu-id="41189-118">Najczęściej spotykanym typem CAPTCHA jest jednym gdzie można zobaczyć niektóre litery zniekształcony, a monit o ich typu.</span><span class="sxs-lookup"><span data-stu-id="41189-118">The most common type of CAPTCHA is one where you see some distorted letters and are asked to type them.</span></span> <span data-ttu-id="41189-119">(Zakłócenia powinien utrudnić botów do odszyfrowania litery.)</span><span class="sxs-lookup"><span data-stu-id="41189-119">(The distortion is supposed to make it hard for bots to decipher the letters.)</span></span>

## <a name="adding-a-recaptcha-test"></a><span data-ttu-id="41189-120">Dodawanie testu ReCaptcha</span><span class="sxs-lookup"><span data-stu-id="41189-120">Adding a ReCaptcha Test</span></span>

<span data-ttu-id="41189-121">Na stronach ASP.NET, można użyć `ReCaptcha` element pomocniczy służący do renderowania test CAPTCHA, który jest oparty na usłudze ReCaptcha ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="41189-121">In ASP.NET pages, you can use the `ReCaptcha` helper to render a CAPTCHA test that is based on the ReCaptcha service ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="41189-122">`ReCaptcha` Pomocnik wyświetli obraz dwa zniekształcony wyrazy, które użytkownicy muszą wprowadzić poprawnie, zanim zostanie zweryfikowana strony.</span><span class="sxs-lookup"><span data-stu-id="41189-122">The `ReCaptcha` helper displays an image of two distorted words that users have to enter correctly before the page is validated.</span></span> <span data-ttu-id="41189-123">Odpowiedź użytkownika jest weryfikowana przez usługę ReCaptcha.Net.</span><span class="sxs-lookup"><span data-stu-id="41189-123">The user response is validated by the ReCaptcha.Net service.</span></span>

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. <span data-ttu-id="41189-124">Zarejestruj swoje witryny sieci Web ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span><span class="sxs-lookup"><span data-stu-id="41189-124">Register your website at ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)).</span></span> <span data-ttu-id="41189-125">Po zakończeniu rejestracji zostanie wyświetlony klucz publiczny i klucz prywatny.</span><span class="sxs-lookup"><span data-stu-id="41189-125">When you've completed registration, you'll get a public key and a private key.</span></span>
2. <span data-ttu-id="41189-126">Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [instalowanie obiekty pomocnicze w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze go.</span><span class="sxs-lookup"><span data-stu-id="41189-126">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
3. <span data-ttu-id="41189-127">Jeśli nie masz jeszcze  *\_AppStart.cshtml* plików, w folderze głównym witryny sieci Web Utwórz plik o nazwie  *\_AppStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="41189-127">If you don't already have a *\_AppStart.cshtml* file, in the root folder of a website create a file named *\_AppStart.cshtml*.</span></span>
4. <span data-ttu-id="41189-128">Dodaj następujący kod `Recaptcha` ustawienia Pomocnika  *\_AppStart.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="41189-128">Add the following `Recaptcha` helper settings in the *\_AppStart.cshtml* file:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. <span data-ttu-id="41189-129">Ustaw `PublicKey` i `PrivateKey` właściwości za pomocą własnych kluczy publicznych i prywatnych.</span><span class="sxs-lookup"><span data-stu-id="41189-129">Set the `PublicKey` and `PrivateKey` properties using your own public and private keys.</span></span>
6. <span data-ttu-id="41189-130">Zapisz  *\_AppStart.cshtml* plik i zamknij go.</span><span class="sxs-lookup"><span data-stu-id="41189-130">Save the *\_AppStart.cshtml* file and close it.</span></span>
7. <span data-ttu-id="41189-131">W folderze głównym witryny sieci Web, należy utworzyć nową stronę o nazwie *Recaptcha.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="41189-131">In the root folder of a website, create new page named *Recaptcha.cshtml*.</span></span>
8. <span data-ttu-id="41189-132">Zastąp istniejącą zawartość następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="41189-132">Replace the existing content with the following:</span></span> 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. <span data-ttu-id="41189-133">Uruchom *Recaptcha.cshtml* strony w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="41189-133">Run the *Recaptcha.cshtml* page in a browser.</span></span> <span data-ttu-id="41189-134">Jeśli `PrivateKey` wartość jest prawidłowa, na stronie jest wyświetlany formantu ReCaptcha i przycisk.</span><span class="sxs-lookup"><span data-stu-id="41189-134">If the `PrivateKey` value is valid, the page displays the ReCaptcha control and a button.</span></span> <span data-ttu-id="41189-135">Jeśli nie było ustawić klucze globalnie w  *\_AppStart.html*, strony zostanie wyświetlony błąd.</span><span class="sxs-lookup"><span data-stu-id="41189-135">If you had not set the keys globally in *\_AppStart.html*, the page would display an error.</span></span> 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. <span data-ttu-id="41189-136">Wprowadź wyrażenie dla testu.</span><span class="sxs-lookup"><span data-stu-id="41189-136">Enter the words for the test.</span></span> <span data-ttu-id="41189-137">Jeśli przekażesz testu ReCaptcha, zobaczysz komunikat w tym celu.</span><span class="sxs-lookup"><span data-stu-id="41189-137">If you pass the ReCaptcha test, you see a message to that effect.</span></span> <span data-ttu-id="41189-138">W przeciwnym razie zostanie wyświetlony komunikat o błędzie i kontroli ReCaptcha, zostanie wyświetlony ponownie.</span><span class="sxs-lookup"><span data-stu-id="41189-138">Otherwise you see an error message and the ReCaptcha control is redisplayed.</span></span>

> [!NOTE]
> <span data-ttu-id="41189-139">Jeśli komputer znajduje się w domenie, która używa serwera proxy, może być konieczne skonfigurowanie `defaultproxy` elementu *Web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="41189-139">If your computer is on a domain that uses proxy server, you might need to configure the `defaultproxy` element of the *Web.config* file.</span></span> <span data-ttu-id="41189-140">W poniższym przykładzie przedstawiono *Web.config* plik z `defaultproxy` element skonfigurowana w celu umożliwienia działania usługi ReCaptcha.</span><span class="sxs-lookup"><span data-stu-id="41189-140">The following example shows a *Web.config* file with the `defaultproxy` element configured to enable the ReCaptcha service to work.</span></span>
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="41189-141">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="41189-141">Additional Resources</span></span>


- [<span data-ttu-id="41189-142">Dostosowywanie zachowania dla całej witryny dla witrynach ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="41189-142">Customizing Site-Wide Behavior for ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="41189-143">ReCaptcha lokacji</span><span class="sxs-lookup"><span data-stu-id="41189-143">ReCaptcha site</span></span>](https://www.google.com/recaptcha)
