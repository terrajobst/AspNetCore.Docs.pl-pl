---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
title: "Testowanie siły hasła (VB) | Dokumentacja firmy Microsoft"
author: wenz
description: "Hasła są wymagane niemal dowolnego miejsca, dzięki czemu użytkownicy opóźnieniem często muszą korzystać z prostych haseł, łatwych do dzielenia. Formant PasswordStrength w ASP. RZECZOWNIK..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9215a37f-3133-4887-8ed2-3689f3a53551
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-vb
msc.type: authoredcontent
ms.openlocfilehash: 7f09a05fd4b5771b7ab532d40476fe45cbd3fe38
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="testing-the-strength-of-a-password-vb"></a>Testowanie siły hasła (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0VB.pdf)

> Hasła są wymagane niemal dowolnego miejsca, dzięki czemu użytkownicy opóźnieniem często muszą korzystać z prostych haseł, łatwych do dzielenia. PasswordStrength formantu w zestawie narzędzi programu ASP.NET AJAX formantu można sprawdzić, jaka jest hasło.


## <a name="overview"></a>Omówienie

Hasła są wymagane niemal dowolnego miejsca, dzięki czemu użytkownicy opóźnieniem często muszą korzystać z prostych haseł, łatwych do dzielenia. `PasswordStrength` Formantu w zestawie narzędzi programu ASP.NET AJAX formantu można sprawdzić, jaka jest hasło.

## <a name="steps"></a>Kroki

`PasswordStrength` Kontroli rozszerza pole tekstowe i sprawdza, czy hasło w nim jest wystarczająca. Oferuje wiele opcji za pomocą atrybutów; Poniżej przedstawiono tylko niektóre z nich:

- `MinimumNumericCharacters`Minimalna liczba znaków liczbowych w haśle
- `MinimumSymbolCharacters`minimalną liczbę znaków symbolicznych (nie litery i cyfry), wymagane hasło
- `PreferredPasswordLength`Minimalna długość hasła
- `RequiresUpperAndLowerCaseCharacters`Określa, czy hasło musi używać zarówno wielkich i małych liter

`StrengthIndicatorType` Informacje jak do prezentowania siły hasła, jako tekst (wartość `"Text"`) lub jako rodzaj pasek postępu (wartość `"BarIndicator"`). W `DisplayPosition` atrybutu, należy skonfigurować miejsca wyświetlania informacji. Oto przykład pełną, łącznie z ASP.NET AJAX `ScriptManager` kontroli, `PasswordStrength` kontroli i oczywiście pole tekstowe, w którym użytkownik może wprowadzić hasło. Dla pokazu pola formularza ostatnie jest polem zwykły tekst i nie jest pole hasło tak, aby widoczne podczas tworzenia formularza.

[!code-aspx[Main](testing-the-strength-of-a-password-vb/samples/sample1.aspx)]

Uruchom strony i wpisz nieobecności: tylko po wprowadzeniu małe litery, wielkie litery, cyfry i symbole hasło jest traktowany jako podzielenie.


[![Teraz hasło jest dobra (dość)](testing-the-strength-of-a-password-vb/_static/image2.png)](testing-the-strength-of-a-password-vb/_static/image1.png)

Teraz hasło jest dobra (dość) ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](testing-the-strength-of-a-password-vb/_static/image3.png))

>[!div class="step-by-step"]
[Poprzednie](testing-the-strength-of-a-password-cs.md)
