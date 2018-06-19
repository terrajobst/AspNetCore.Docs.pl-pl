---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: Testowanie siły hasła (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Hasła są wymagane niemal dowolnego miejsca, dzięki czemu użytkownicy opóźnieniem często muszą korzystać z prostych haseł, łatwych do dzielenia. Formant PasswordStrength w ASP. RZECZOWNIK...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: 1d46026535f3f5cf82944359599464e8a4725280
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879455"
---
<a name="testing-the-strength-of-a-password-vb"></a><span data-ttu-id="931a7-104">Testowanie siły hasła (VB)</span><span class="sxs-lookup"><span data-stu-id="931a7-104">Testing the Strength of a Password (VB)</span></span>
====================
<span data-ttu-id="931a7-105">przez [Wenz Chrześcijańskie](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="931a7-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="931a7-106">[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="931a7-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)</span></span>

> <span data-ttu-id="931a7-107">Hasła są wymagane niemal dowolnego miejsca, dzięki czemu użytkownicy opóźnieniem często muszą korzystać z prostych haseł, łatwych do dzielenia.</span><span class="sxs-lookup"><span data-stu-id="931a7-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="931a7-108">PasswordStrength formantu w zestawie narzędzi programu ASP.NET AJAX formantu można sprawdzić, jaka jest hasło.</span><span class="sxs-lookup"><span data-stu-id="931a7-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="931a7-109">Omówienie</span><span class="sxs-lookup"><span data-stu-id="931a7-109">Overview</span></span>

<span data-ttu-id="931a7-110">Hasła są wymagane niemal dowolnego miejsca, dzięki czemu użytkownicy opóźnieniem często muszą korzystać z prostych haseł, łatwych do dzielenia.</span><span class="sxs-lookup"><span data-stu-id="931a7-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="931a7-111">`PasswordStrength` Formantu w zestawie narzędzi programu ASP.NET AJAX formantu można sprawdzić, jaka jest hasło.</span><span class="sxs-lookup"><span data-stu-id="931a7-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="931a7-112">Kroki</span><span class="sxs-lookup"><span data-stu-id="931a7-112">Steps</span></span>

<span data-ttu-id="931a7-113">`PasswordStrength` Kontroli rozszerza pole tekstowe i sprawdza, czy hasło w nim jest wystarczająca.</span><span class="sxs-lookup"><span data-stu-id="931a7-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="931a7-114">Oferuje wiele opcji za pomocą atrybutów; Poniżej przedstawiono tylko niektóre z nich:</span><span class="sxs-lookup"><span data-stu-id="931a7-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="931a7-115">`MinimumNumericCharacters` Minimalna liczba znaków liczbowych w haśle</span><span class="sxs-lookup"><span data-stu-id="931a7-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="931a7-116">`MinimumSymbolCharacters` minimalną liczbę znaków symbolicznych (nie litery i cyfry), wymagane hasło</span><span class="sxs-lookup"><span data-stu-id="931a7-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="931a7-117">`PreferredPasswordLength` Minimalna długość hasła</span><span class="sxs-lookup"><span data-stu-id="931a7-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="931a7-118">`RequiresUpperAndLowerCaseCharacters` Określa, czy hasło musi używać zarówno wielkich i małych liter</span><span class="sxs-lookup"><span data-stu-id="931a7-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="931a7-119">`StrengthIndicatorType` Informacje jak do prezentowania siły hasła, jako tekst (wartość `"Text"`) lub jako rodzaj pasek postępu (wartość `"BarIndicator"`).</span><span class="sxs-lookup"><span data-stu-id="931a7-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="931a7-120">W `DisplayPosition` atrybutu, należy skonfigurować miejsca wyświetlania informacji.</span><span class="sxs-lookup"><span data-stu-id="931a7-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="931a7-121">Oto przykład pełną, łącznie z ASP.NET AJAX `ScriptManager` kontroli, `PasswordStrength` kontroli i oczywiście pole tekstowe, w którym użytkownik może wprowadzić hasło.</span><span class="sxs-lookup"><span data-stu-id="931a7-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="931a7-122">Dla pokazu pola formularza ostatnie jest polem zwykły tekst i nie jest pole hasło tak, aby widoczne podczas tworzenia formularza.</span><span class="sxs-lookup"><span data-stu-id="931a7-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

<span data-ttu-id="931a7-123">Uruchom strony i wpisz nieobecności: tylko po wprowadzeniu małe litery, wielkie litery, cyfry i symbole hasło jest traktowany jako podzielenie.</span><span class="sxs-lookup"><span data-stu-id="931a7-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


<span data-ttu-id="931a7-124">[![Teraz hasło jest dobra (dość)](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="931a7-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)</span></span>

<span data-ttu-id="931a7-125">Teraz hasło jest dobra (dość) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](testing-the-strength-of-a-password-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="931a7-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="931a7-126">Poprzednie</span><span class="sxs-lookup"><span data-stu-id="931a7-126">Previous</span></span>](testing-the-strength-of-a-password-cs.md)
