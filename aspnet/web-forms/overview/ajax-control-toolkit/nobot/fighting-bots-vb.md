---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: "Walka robotów (VB) | Dokumentacja firmy Microsoft"
author: wenz
description: "Zautomatyzowanych robotów Sztukateria dzienników sieci Web i innych witryn sieci Web ze spamem przesyłania formularzy komentarz bez interakcji użytkownika. Kontrolki na NoBot ASP.NET AJAX Con..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: 3b786fd8605c7521a4aae8e49ca236363a71b572
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="fighting-bots-vb"></a><span data-ttu-id="68702-104">Walczących robotów (VB)</span><span class="sxs-lookup"><span data-stu-id="68702-104">Fighting Bots (VB)</span></span>
====================
<span data-ttu-id="68702-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="68702-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="68702-106">[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="68702-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span></span>

> <span data-ttu-id="68702-107">Zautomatyzowanych robotów Sztukateria dzienników sieci Web i innych witryn sieci Web ze spamem przesyłania formularzy komentarz bez interakcji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="68702-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="68702-108">Formant NoBot w zestawie narzędzi programu ASP.NET AJAX kontroli może pomóc zwalczania tych robotów.</span><span class="sxs-lookup"><span data-stu-id="68702-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="68702-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="68702-109">Overview</span></span>

<span data-ttu-id="68702-110">Zautomatyzowanych robotów Sztukateria dzienników sieci Web i innych witryn sieci Web ze spamem przesyłania formularzy komentarz bez interakcji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="68702-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="68702-111">Formant NoBot w zestawie narzędzi programu ASP.NET AJAX kontroli może pomóc zwalczania tych robotów.</span><span class="sxs-lookup"><span data-stu-id="68702-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="68702-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="68702-112">Steps</span></span>

<span data-ttu-id="68702-113">Jednym z podejść wspólnej pokonanie robotów jest za pomocą testu CAPTCHAs całkowicie zautomatyzowanego publicznego Turing można stwierdzić, komputerów i ludzi od siebie.</span><span class="sxs-lookup"><span data-stu-id="68702-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="68702-114">Turing test pierwotnie testu gdzie ktoś potrzebne do określenia, czy partner komunikacji jest człowieka lub maszyny.</span><span class="sxs-lookup"><span data-stu-id="68702-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="68702-115">W sieci web CAPTCHA składa się zwykle z obrazu z niektórych zniekształcony litery od niej.</span><span class="sxs-lookup"><span data-stu-id="68702-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="68702-116">Pomysł jest, czy tylko człowieka może odczytywać litery na obrazie, natomiast algorytmy Rozpoznawania zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="68702-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="68702-117">Istnieje kilka wady i zalety tego podejścia, ale omówienie to wykracza poza zakres tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="68702-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="68702-118">Jednak występuje formantu w zestawie narzędzi w kontroli AJAX ASP.NET, oferujący podejście podobne: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="68702-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="68702-119">Jest ona łatwiej rozwiązać niż CAPTCHA, ale jest bardzo łatwa w użyciu, a opłaty bardzo dobrze nadaje się do witryny sieci Web, takich jak blogi, gdy jest on uznawany za sukcesu, jeśli większość spamu prób są bezcelowe, który `NoBot` możliwość sterowania.</span><span class="sxs-lookup"><span data-stu-id="68702-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="68702-120">`NoBot`Przechwytuje ogłaszania zwrotnego bieżącego formularza sieci web ASP.NET, jeśli co najmniej jeden z tych warunków jest spełniony:</span><span class="sxs-lookup"><span data-stu-id="68702-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="68702-121">Przeglądarka nie może rozwiązać układanki JavaScript (na przykład gdy JavaScript jest dezaktywowana)</span><span class="sxs-lookup"><span data-stu-id="68702-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="68702-122">Użytkownik podał formularza do szybkiego</span><span class="sxs-lookup"><span data-stu-id="68702-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="68702-123">Adres IP klienta formularz został przesłany zbyt często w danym okresie czasu.</span><span class="sxs-lookup"><span data-stu-id="68702-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="68702-124">Aby sprawdzić, czy te warunki `NoBot` formant wymaga tych atrybutów (wszystkie z nich opcjonalny):</span><span class="sxs-lookup"><span data-stu-id="68702-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="68702-125">`ResponseMinimumDelaySeconds`Minimalna ilość czasu w sekundach między odświeżeniami</span><span class="sxs-lookup"><span data-stu-id="68702-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="68702-126">`CutoffWindowSeconds`długość okresu, w którym ogłaszania zwrotnego z jednego adresu IP są środkami</span><span class="sxs-lookup"><span data-stu-id="68702-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="68702-127">`CutoffMaximumInstances`Maksymalna ilość sekund dla interwału czasu</span><span class="sxs-lookup"><span data-stu-id="68702-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="68702-128">Następujące wymagania znaczników tego co najmniej dwie sekundy upłynąć między odświeżeniami i są tylko pięć ogłaszania zwrotnego lub mniej w interwale 30-sekundowym:</span><span class="sxs-lookup"><span data-stu-id="68702-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

<span data-ttu-id="68702-129">Następnie w zwykły sposób upewnij się uwzględnić `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX została załadowana i można go używać zestawu narzędzi kontroli:</span><span class="sxs-lookup"><span data-stu-id="68702-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

<span data-ttu-id="68702-130">Ponieważ większość kontroli `NoBot` wykonuje wystąpić po stronie serwera, należy sprawdzić wynik tych operacji sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="68702-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="68702-131">Można to zrobić przez wywołanie metody `NoBot`w `IsValid()` metody.</span><span class="sxs-lookup"><span data-stu-id="68702-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="68702-132">Składa się z jednym argumentem (jako `out` parametru /`ByRef` parametr) jest typu `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="68702-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="68702-133">Reprezentacji ciągu zawiera przyczyny, jeśli sprawdzenie zakończy się niepowodzeniem i `Valid` inaczej.</span><span class="sxs-lookup"><span data-stu-id="68702-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="68702-134">Poniższy kod wyświetla komunikat zgodnie z `NoBot`do wyniku:</span><span class="sxs-lookup"><span data-stu-id="68702-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

<span data-ttu-id="68702-135">Na koniec należy formularza w celu przesyłania i elementu label, zwracać komunikat, a wszystko będzie gotowe!</span><span class="sxs-lookup"><span data-stu-id="68702-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

<span data-ttu-id="68702-136">Uruchom ten skrypt, a dezaktywować JavaScript lub Prześlij formularz w pierwszych dwóch sekund lub Prześlij formularz siedem razy w ciągu 30 sekund, zostanie wyświetlony komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="68702-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="68702-137">Jednak rozsądny sposób przy użyciu tego formantu, ponieważ tylko około 90-95% użytkownicy mają JavaScript aktywowany, w związku z tym 5 – 10% użytkowników zakończy się niepowodzeniem `NoBot`do testowania.</span><span class="sxs-lookup"><span data-stu-id="68702-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="68702-138">[![Ten komunikat o błędzie może być spowodowany robotów](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="68702-138">[![This error message could have been caused by a bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span></span>

<span data-ttu-id="68702-139">Ten komunikat o błędzie może być spowodowany robotów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](fighting-bots-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="68702-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="68702-140">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="68702-140">Previous</span></span>](fighting-bots-cs.md)