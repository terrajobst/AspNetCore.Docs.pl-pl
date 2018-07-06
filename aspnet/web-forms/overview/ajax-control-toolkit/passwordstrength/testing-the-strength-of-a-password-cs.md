---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
title: Testowanie siły hasła (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Hasła są wymagane praktycznie dowolnym miejscu, tak aby użytkownicy z opóźnieniem często wybierz prostych haseł, które są łatwe do przerwania. Kontrolka PasswordStrength w ASP. RZECZOWNIK...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: cb4afbae-9b8f-483d-9729-476d4b9f85fc
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
msc.type: authoredcontent
ms.openlocfilehash: 2eec810a0e37347059d3e82a37b25d5663eab987
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839807"
---
<a name="testing-the-strength-of-a-password-c"></a><span data-ttu-id="bb233-104">Testowanie siły hasła (C#)</span><span class="sxs-lookup"><span data-stu-id="bb233-104">Testing the Strength of a Password (C#)</span></span>
====================
<span data-ttu-id="bb233-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bb233-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bb233-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="bb233-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)</span></span>

> <span data-ttu-id="bb233-107">Hasła są wymagane praktycznie dowolnym miejscu, tak aby użytkownicy z opóźnieniem często wybierz prostych haseł, które są łatwe do przerwania.</span><span class="sxs-lookup"><span data-stu-id="bb233-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="bb233-108">Na formant PasswordStrength ASP.NET AJAX Control Toolkit można sprawdzić, jak dobre jest hasło.</span><span class="sxs-lookup"><span data-stu-id="bb233-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="bb233-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="bb233-109">Overview</span></span>

<span data-ttu-id="bb233-110">Hasła są wymagane praktycznie dowolnym miejscu, tak aby użytkownicy z opóźnieniem często wybierz prostych haseł, które są łatwe do przerwania.</span><span class="sxs-lookup"><span data-stu-id="bb233-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="bb233-111">`PasswordStrength` Sterowania w programie ASP.NET AJAX Control Toolkit można sprawdzić, jak dobre jest hasło.</span><span class="sxs-lookup"><span data-stu-id="bb233-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="bb233-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="bb233-112">Steps</span></span>

<span data-ttu-id="bb233-113">`PasswordStrength` Kontroli rozszerza pole tekstowe i sprawdza, czy hasło w niej jest wystarczająco dobre.</span><span class="sxs-lookup"><span data-stu-id="bb233-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="bb233-114">Oferuje ona wiele opcji za pomocą atrybutów, a Poniżej przedstawiono tylko niektóre z nich:</span><span class="sxs-lookup"><span data-stu-id="bb233-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="bb233-115">`MinimumNumericCharacters` Minimalna liczba znaków w haśle</span><span class="sxs-lookup"><span data-stu-id="bb233-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="bb233-116">`MinimumSymbolCharacters` minimalną liczbę znaków symbolicznych (nie litery i cyfry), wymagana w haśle</span><span class="sxs-lookup"><span data-stu-id="bb233-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="bb233-117">`PreferredPasswordLength` Minimalna długość hasła</span><span class="sxs-lookup"><span data-stu-id="bb233-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="bb233-118">`RequiresUpperAndLowerCaseCharacters` czy hasło musi używać zarówno małe i wielkie litery</span><span class="sxs-lookup"><span data-stu-id="bb233-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="bb233-119">`StrengthIndicatorType` Informacje jak prezentować siły hasła, jako tekst (wartość `"Text"`) lub jako rodzaj pasek postępu (wartość `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="bb233-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="bb233-120">W `DisplayPosition` atrybut, można skonfigurować miejsca wyświetlania informacji.</span><span class="sxs-lookup"><span data-stu-id="bb233-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="bb233-121">Oto kompletny przykład, w tym ASP.NET AJAX `ScriptManager` kontroli `PasswordStrength` kontrolki i pola tekstowego, w którym użytkownik może wprowadzić hasło.</span><span class="sxs-lookup"><span data-stu-id="bb233-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="bb233-122">Celach demonstracyjnych pole formularza ostatnim jest pola zwykły tekst i nie jest pole hasło, dzięki czemu można zobaczyć podczas programowania, w miarę wpisywania.</span><span class="sxs-lookup"><span data-stu-id="bb233-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

<span data-ttu-id="bb233-123">Uruchom stronę, a następnie wpisz natychmiast: tylko po wprowadzeniu małe litery, wielkie litery, cyfry i symbole hasło jest uznawany za jako podzielenie.</span><span class="sxs-lookup"><span data-stu-id="bb233-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


<span data-ttu-id="bb233-124">[![Hasło jest dobre (dość)](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bb233-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)</span></span>

<span data-ttu-id="bb233-125">Hasło jest dobre (dość) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](testing-the-strength-of-a-password-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bb233-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bb233-126">Next</span><span class="sxs-lookup"><span data-stu-id="bb233-126">Next</span></span>](testing-the-strength-of-a-password-vb.md)
