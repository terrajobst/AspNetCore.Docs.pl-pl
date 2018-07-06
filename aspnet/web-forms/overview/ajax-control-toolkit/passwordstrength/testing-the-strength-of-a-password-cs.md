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
<a name="testing-the-strength-of-a-password-c"></a>Testowanie siły hasła (C#)
====================
przez [Christian Wenz](https://github.com/wenz)

[Pobierz program Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)

> Hasła są wymagane praktycznie dowolnym miejscu, tak aby użytkownicy z opóźnieniem często wybierz prostych haseł, które są łatwe do przerwania. Na formant PasswordStrength ASP.NET AJAX Control Toolkit można sprawdzić, jak dobre jest hasło.


## <a name="overview"></a>Omówienie

Hasła są wymagane praktycznie dowolnym miejscu, tak aby użytkownicy z opóźnieniem często wybierz prostych haseł, które są łatwe do przerwania. `PasswordStrength` Sterowania w programie ASP.NET AJAX Control Toolkit można sprawdzić, jak dobre jest hasło.

## <a name="steps"></a>Kroki

`PasswordStrength` Kontroli rozszerza pole tekstowe i sprawdza, czy hasło w niej jest wystarczająco dobre. Oferuje ona wiele opcji za pomocą atrybutów, a Poniżej przedstawiono tylko niektóre z nich:

- `MinimumNumericCharacters` Minimalna liczba znaków w haśle
- `MinimumSymbolCharacters` minimalną liczbę znaków symbolicznych (nie litery i cyfry), wymagana w haśle
- `PreferredPasswordLength` Minimalna długość hasła
- `RequiresUpperAndLowerCaseCharacters` czy hasło musi używać zarówno małe i wielkie litery

`StrengthIndicatorType` Informacje jak prezentować siły hasła, jako tekst (wartość `"Text"`) lub jako rodzaj pasek postępu (wartość `"BarIndicator"`). W `DisplayPosition` atrybut, można skonfigurować miejsca wyświetlania informacji. Oto kompletny przykład, w tym ASP.NET AJAX `ScriptManager` kontroli `PasswordStrength` kontrolki i pola tekstowego, w którym użytkownik może wprowadzić hasło. Celach demonstracyjnych pole formularza ostatnim jest pola zwykły tekst i nie jest pole hasło, dzięki czemu można zobaczyć podczas programowania, w miarę wpisywania.

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

Uruchom stronę, a następnie wpisz natychmiast: tylko po wprowadzeniu małe litery, wielkie litery, cyfry i symbole hasło jest uznawany za jako podzielenie.


[![Hasło jest dobre (dość)](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)

Hasło jest dobre (dość) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](testing-the-strength-of-a-password-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](testing-the-strength-of-a-password-vb.md)
