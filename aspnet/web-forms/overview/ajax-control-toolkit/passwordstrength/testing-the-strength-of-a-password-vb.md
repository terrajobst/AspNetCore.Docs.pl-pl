---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Testowanie siły hasła (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Hasła są wymagane praktycznie dowolnym miejscu, tak aby użytkownicy z opóźnieniem często wybierz prostych haseł, które są łatwe do przerwania. Kontrolka PasswordStrength w ASP. RZECZOWNIK...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: 3faf9996c73fb5aaa427b515d396f36663cf1801
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379705"
---
<a name="testing-the-strength-of-a-password-vb"></a><span data-ttu-id="d6b41-104">Testowanie siły hasła (VB)</span><span class="sxs-lookup"><span data-stu-id="d6b41-104">Testing the Strength of a Password (VB)</span></span>
====================
<span data-ttu-id="d6b41-105">przez [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d6b41-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d6b41-106">[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="d6b41-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span></span>

> <span data-ttu-id="d6b41-107">Hasła są wymagane praktycznie dowolnym miejscu, tak aby użytkownicy z opóźnieniem często wybierz prostych haseł, które są łatwe do przerwania.</span><span class="sxs-lookup"><span data-stu-id="d6b41-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="d6b41-108">Na formant PasswordStrength ASP.NET AJAX Control Toolkit można sprawdzić, jak dobre jest hasło.</span><span class="sxs-lookup"><span data-stu-id="d6b41-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="d6b41-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="d6b41-109">Overview</span></span>

<span data-ttu-id="d6b41-110">Hasła są wymagane praktycznie dowolnym miejscu, tak aby użytkownicy z opóźnieniem często wybierz prostych haseł, które są łatwe do przerwania.</span><span class="sxs-lookup"><span data-stu-id="d6b41-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="d6b41-111">`PasswordStrength` Sterowania w programie ASP.NET AJAX Control Toolkit można sprawdzić, jak dobre jest hasło.</span><span class="sxs-lookup"><span data-stu-id="d6b41-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="d6b41-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="d6b41-112">Steps</span></span>

<span data-ttu-id="d6b41-113">`PasswordStrength` Kontroli rozszerza pole tekstowe i sprawdza, czy hasło w niej jest wystarczająco dobre.</span><span class="sxs-lookup"><span data-stu-id="d6b41-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="d6b41-114">Oferuje ona wiele opcji za pomocą atrybutów, a Poniżej przedstawiono tylko niektóre z nich:</span><span class="sxs-lookup"><span data-stu-id="d6b41-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="d6b41-115">`MinimumNumericCharacters` Minimalna liczba znaków w haśle</span><span class="sxs-lookup"><span data-stu-id="d6b41-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="d6b41-116">`MinimumSymbolCharacters` minimalną liczbę znaków symbolicznych (nie litery i cyfry), wymagana w haśle</span><span class="sxs-lookup"><span data-stu-id="d6b41-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="d6b41-117">`PreferredPasswordLength` Minimalna długość hasła</span><span class="sxs-lookup"><span data-stu-id="d6b41-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="d6b41-118">`RequiresUpperAndLowerCaseCharacters` czy hasło musi używać zarówno małe i wielkie litery</span><span class="sxs-lookup"><span data-stu-id="d6b41-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="d6b41-119">`StrengthIndicatorType` Informacje jak prezentować siły hasła, jako tekst (wartość `"Text"`) lub jako rodzaj pasek postępu (wartość `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="d6b41-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="d6b41-120">W `DisplayPosition` atrybut, można skonfigurować miejsca wyświetlania informacji.</span><span class="sxs-lookup"><span data-stu-id="d6b41-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="d6b41-121">Oto kompletny przykład, w tym ASP.NET AJAX `ScriptManager` kontroli `PasswordStrength` kontrolki i pola tekstowego, w którym użytkownik może wprowadzić hasło.</span><span class="sxs-lookup"><span data-stu-id="d6b41-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="d6b41-122">Celach demonstracyjnych pole formularza ostatnim jest pola zwykły tekst i nie jest pole hasło, dzięki czemu można zobaczyć podczas programowania, w miarę wpisywania.</span><span class="sxs-lookup"><span data-stu-id="d6b41-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

<span data-ttu-id="d6b41-123">Uruchom stronę, a następnie wpisz natychmiast: tylko po wprowadzeniu małe litery, wielkie litery, cyfry i symbole hasło jest uznawany za jako podzielenie.</span><span class="sxs-lookup"><span data-stu-id="d6b41-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


<span data-ttu-id="d6b41-124">[![Hasło jest dobre (dość)](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d6b41-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span></span>

<span data-ttu-id="d6b41-125">Hasło jest dobre (dość) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](testing-the-strength-of-a-password-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d6b41-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d6b41-126">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="d6b41-126">Previous</span></span>](testing-the-strength-of-a-password-cs.md)
