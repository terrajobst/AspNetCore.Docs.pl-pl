---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Zwalczanie Botów (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Zautomatyzowanych robotów Sztukateria dzienników w sieci Web i innych witryn sieci Web ze spamem, przesyłać formularze komentarz bez żadnej interakcji użytkownika. Kontrolka NoBot w Con AJAX programu ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: 1f919d130cc97b5f892cc01d58a0bcba790a98e3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362521"
---
<a name="fighting-bots-vb"></a><span data-ttu-id="5794b-104">Zwalczanie Botów (VB)</span><span class="sxs-lookup"><span data-stu-id="5794b-104">Fighting Bots (VB)</span></span>
====================
<span data-ttu-id="5794b-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5794b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5794b-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5794b-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span></span>

> <span data-ttu-id="5794b-107">Zautomatyzowanych robotów Sztukateria dzienników w sieci Web i innych witryn sieci Web ze spamem, przesyłać formularze komentarz bez żadnej interakcji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5794b-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="5794b-108">Kontrolka NoBot w ASP.NET AJAX Control Toolkit może pomóc walczą o tych botów.</span><span class="sxs-lookup"><span data-stu-id="5794b-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="5794b-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="5794b-109">Overview</span></span>

<span data-ttu-id="5794b-110">Zautomatyzowanych robotów Sztukateria dzienników w sieci Web i innych witryn sieci Web ze spamem, przesyłać formularze komentarz bez żadnej interakcji użytkownika.</span><span class="sxs-lookup"><span data-stu-id="5794b-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="5794b-111">Kontrolka NoBot w ASP.NET AJAX Control Toolkit może pomóc walczą o tych botów.</span><span class="sxs-lookup"><span data-stu-id="5794b-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="5794b-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="5794b-112">Steps</span></span>

<span data-ttu-id="5794b-113">Typową metodą pokonania Boty jest Użyj CAPTCHAs całkowicie zautomatyzowanego publicznych Turing testu, aby stwierdzić, komputerów i ludzie od siebie.</span><span class="sxs-lookup"><span data-stu-id="5794b-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="5794b-114">Turing test była pierwotnie testu tam, gdzie ktoś konieczne podjęcie decyzji partnera komunikacji jest maszynę lub ludzi.</span><span class="sxs-lookup"><span data-stu-id="5794b-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="5794b-115">W sieci web CAPTCHA składa się zwykle obrazu niektóre litery zniekształcony na nim.</span><span class="sxs-lookup"><span data-stu-id="5794b-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="5794b-116">Chodzi o to, że tylko człowiek może odczytać litery na obrazie, natomiast algorytmy optyczne rozpoznawanie znaków zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="5794b-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="5794b-117">Istnieje kilka zalet i wad tego podejścia, ale dyskusję na temat to wykracza poza zakres tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="5794b-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="5794b-118">Istnieje jednak kontrolki w ASP.NET AJAX Control Toolkit, który zapewnia podejście podobne: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="5794b-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="5794b-119">Jest to łatwiejsze do pokonania niż z mechanizmu CAPTCHA, ale jest bardzo łatwa w użyciu i taryf bardzo dobrze nadaje się do witryny sieci Web, takich jak blogi, gdzie uważa się sukcesem, jeśli większość spamu prób jest bezcelowe, który `NoBot` kontroli można zrobić.</span><span class="sxs-lookup"><span data-stu-id="5794b-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="5794b-120">`NoBot` Przechwytuje zwrotu bieżącego formularza sieci web platformy ASP.NET, jeśli co najmniej jeden z tych warunków jest spełniony:</span><span class="sxs-lookup"><span data-stu-id="5794b-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="5794b-121">Przeglądarka nie może rozwiązać układanki JavaScript (na przykład gdy JavaScript jest dezaktywowany)</span><span class="sxs-lookup"><span data-stu-id="5794b-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="5794b-122">Formularz, aby szybko zostały przesłane przez użytkownika</span><span class="sxs-lookup"><span data-stu-id="5794b-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="5794b-123">Adres IP klienta przesłanego formularza zbyt często w pewien czas.</span><span class="sxs-lookup"><span data-stu-id="5794b-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="5794b-124">Aby sprawdzić, czy te warunki `NoBot` formant wymaga tych atrybutów (wszystkie z nich, które są opcjonalne):</span><span class="sxs-lookup"><span data-stu-id="5794b-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="5794b-125">`ResponseMinimumDelaySeconds` Minimalny czas w sekundach między ogłoszeniami wstecznymi</span><span class="sxs-lookup"><span data-stu-id="5794b-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="5794b-126">`CutoffWindowSeconds` długość interwału czasu, w którym ogłaszania zwrotnego w jeden adres IP jest miar</span><span class="sxs-lookup"><span data-stu-id="5794b-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="5794b-127">`CutoffMaximumInstances` Maksymalna ilość sekundy na przedział czasu</span><span class="sxs-lookup"><span data-stu-id="5794b-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="5794b-128">Następujące wymagania znaczników tego co najmniej dwie sekundy upłynąć między ogłoszeniami wstecznymi oraz czy tylko pięć ogłaszania zwrotnego lub szybciej w interwale 30-sekundowym:</span><span class="sxs-lookup"><span data-stu-id="5794b-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

<span data-ttu-id="5794b-129">Następnie w zwykły sposób upewnij się uwzględnić `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX jest ładowany i można go używać razem sterowania:</span><span class="sxs-lookup"><span data-stu-id="5794b-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

<span data-ttu-id="5794b-130">Ponieważ większość kontroli `NoBot` robi wystąpić po stronie serwera, musisz sprawdzić wynik tych operacji sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="5794b-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="5794b-131">Można to zrobić, wywołując `NoBot`firmy `IsValid()` metody.</span><span class="sxs-lookup"><span data-stu-id="5794b-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="5794b-132">Ma jeden argument (jako `out` parametr /`ByRef` parametru) jest typu `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="5794b-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="5794b-133">Ciąg reprezentujący zawiera przyczynę niepowodzenia sprawdzania i `Valid` inaczej.</span><span class="sxs-lookup"><span data-stu-id="5794b-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="5794b-134">Poniższy kod wyświetla komunikat, zgodnie z opisem w `NoBot`w wyniku:</span><span class="sxs-lookup"><span data-stu-id="5794b-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

<span data-ttu-id="5794b-135">Na koniec należy formularza w celu przesyłania i elementu label, aby wyprowadzić komunikatu i wszystko jest gotowe!</span><span class="sxs-lookup"><span data-stu-id="5794b-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

<span data-ttu-id="5794b-136">Uruchom ten skrypt, a dezaktywować JavaScript lub Prześlij formularz w ciągu pierwszych dwóch sekund lub Prześlij formularz siedem razy w ciągu 30 sekund, zostanie wyświetlony komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="5794b-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="5794b-137">Jednak należy uważnie użyć tej kontrolki, ponieważ tylko około 90-95% użytkownicy mają JavaScript aktywowany, w związku z tym 5 – 10% użytkowników zakończy się niepowodzeniem `NoBot`użytkownika testu.</span><span class="sxs-lookup"><span data-stu-id="5794b-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="5794b-138">[![Ten komunikat o błędzie może być spowodowane przez robota](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5794b-138">[![This error message could have been caused by a bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span></span>

<span data-ttu-id="5794b-139">Ten komunikat o błędzie może być spowodowane przez robota ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](fighting-bots-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5794b-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5794b-140">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="5794b-140">Previous</span></span>](fighting-bots-cs.md)
