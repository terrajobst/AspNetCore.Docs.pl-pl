---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
title: Testowanie siły hasła (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Hasła są wymagane niemal dowolnego miejsca, dzięki czemu użytkownicy opóźnieniem często muszą korzystać z prostych haseł, łatwych do dzielenia. Formant PasswordStrength w ASP. RZECZOWNIK...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cb4afbae-9b8f-483d-9729-476d4b9f85fc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
msc.type: authoredcontent
ms.openlocfilehash: f5f4a7128f2edbef4fbe95faf9de19bdae5f436e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869221"
---
<a name="testing-the-strength-of-a-password-c"></a>Testowanie siły hasła (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)

> Hasła są wymagane niemal dowolnego miejsca, dzięki czemu użytkownicy opóźnieniem często muszą korzystać z prostych haseł, łatwych do dzielenia. PasswordStrength formantu w zestawie narzędzi programu ASP.NET AJAX formantu można sprawdzić, jaka jest hasło.


## <a name="overview"></a>Omówienie

Hasła są wymagane niemal dowolnego miejsca, dzięki czemu użytkownicy opóźnieniem często muszą korzystać z prostych haseł, łatwych do dzielenia. `PasswordStrength` Formantu w zestawie narzędzi programu ASP.NET AJAX formantu można sprawdzić, jaka jest hasło.

## <a name="steps"></a>Kroki

`PasswordStrength` Kontroli rozszerza pole tekstowe i sprawdza, czy hasło w nim jest wystarczająca. Oferuje wiele opcji za pomocą atrybutów; Poniżej przedstawiono tylko niektóre z nich:

- `MinimumNumericCharacters` Minimalna liczba znaków liczbowych w haśle
- `MinimumSymbolCharacters` minimalną liczbę znaków symbolicznych (nie litery i cyfry), wymagane hasło
- `PreferredPasswordLength` Minimalna długość hasła
- `RequiresUpperAndLowerCaseCharacters` Określa, czy hasło musi używać zarówno wielkich i małych liter

`StrengthIndicatorType` Informacje jak do prezentowania siły hasła, jako tekst (wartość `"Text"`) lub jako rodzaj pasek postępu (wartość `"BarIndicator"`). W `DisplayPosition` atrybutu, należy skonfigurować miejsca wyświetlania informacji. Oto przykład pełną, łącznie z ASP.NET AJAX `ScriptManager` kontroli, `PasswordStrength` kontroli i oczywiście pole tekstowe, w którym użytkownik może wprowadzić hasło. Dla pokazu pola formularza ostatnie jest polem zwykły tekst i nie jest pole hasło tak, aby widoczne podczas tworzenia formularza.

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

Uruchom strony i wpisz nieobecności: tylko po wprowadzeniu małe litery, wielkie litery, cyfry i symbole hasło jest traktowany jako podzielenie.


[![Teraz hasło jest dobra (dość)](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)

Teraz hasło jest dobra (dość) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](testing-the-strength-of-a-password-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](testing-the-strength-of-a-password-vb.md)
