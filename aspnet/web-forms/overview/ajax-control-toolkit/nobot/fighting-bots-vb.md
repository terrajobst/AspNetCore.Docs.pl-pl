---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Walka robotów (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Zautomatyzowanych robotów Sztukateria dzienników sieci Web i innych witryn sieci Web ze spamem przesyłania formularzy komentarz bez interakcji użytkownika. Kontrolki na NoBot ASP.NET AJAX Con...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: 35d5984ac7ff3422bab07a759c93fef3914a22f7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="fighting-bots-vb"></a>Walczących robotów (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)

> Zautomatyzowanych robotów Sztukateria dzienników sieci Web i innych witryn sieci Web ze spamem przesyłania formularzy komentarz bez interakcji użytkownika. Formant NoBot w zestawie narzędzi programu ASP.NET AJAX kontroli może pomóc zwalczania tych robotów.


## <a name="overview"></a>Omówienie

Zautomatyzowanych robotów Sztukateria dzienników sieci Web i innych witryn sieci Web ze spamem przesyłania formularzy komentarz bez interakcji użytkownika. Formant NoBot w zestawie narzędzi programu ASP.NET AJAX kontroli może pomóc zwalczania tych robotów.

## <a name="steps"></a>Kroki

Jednym z podejść wspólnej pokonanie robotów jest za pomocą testu CAPTCHAs całkowicie zautomatyzowanego publicznego Turing można stwierdzić, komputerów i ludzi od siebie. Turing test pierwotnie testu gdzie ktoś potrzebne do określenia, czy partner komunikacji jest człowieka lub maszyny. W sieci web CAPTCHA składa się zwykle z obrazu z niektórych zniekształcony litery od niej. Pomysł jest, czy tylko człowieka może odczytywać litery na obrazie, natomiast algorytmy Rozpoznawania zakończy się niepowodzeniem.

Istnieje kilka wady i zalety tego podejścia, ale omówienie to wykracza poza zakres tego samouczka. Jednak występuje formantu w zestawie narzędzi w kontroli AJAX ASP.NET, oferujący podejście podobne: `NoBot`. Jest ona łatwiej rozwiązać niż CAPTCHA, ale jest bardzo łatwa w użyciu, a opłaty bardzo dobrze nadaje się do witryny sieci Web, takich jak blogi, gdy jest on uznawany za sukcesu, jeśli większość spamu prób są bezcelowe, który `NoBot` możliwość sterowania.

`NoBot` Przechwytuje ogłaszania zwrotnego bieżącego formularza sieci web ASP.NET, jeśli co najmniej jeden z tych warunków jest spełniony:

- Przeglądarka nie może rozwiązać układanki JavaScript (na przykład gdy JavaScript jest dezaktywowana)
- Użytkownik podał formularza do szybkiego
- Adres IP klienta formularz został przesłany zbyt często w danym okresie czasu.

Aby sprawdzić, czy te warunki `NoBot` formant wymaga tych atrybutów (wszystkie z nich opcjonalny):

- `ResponseMinimumDelaySeconds` Minimalna ilość czasu w sekundach między odświeżeniami
- `CutoffWindowSeconds` długość okresu, w którym ogłaszania zwrotnego z jednego adresu IP są środkami
- `CutoffMaximumInstances` Maksymalna ilość sekund dla interwału czasu

Następujące wymagania znaczników tego co najmniej dwie sekundy upłynąć między odświeżeniami i są tylko pięć ogłaszania zwrotnego lub mniej w interwale 30-sekundowym:

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

Następnie w zwykły sposób upewnij się uwzględnić `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX została załadowana i można go używać zestawu narzędzi kontroli:

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

Ponieważ większość kontroli `NoBot` wykonuje wystąpić po stronie serwera, należy sprawdzić wynik tych operacji sprawdzania poprawności. Można to zrobić przez wywołanie metody `NoBot`w `IsValid()` metody. Składa się z jednym argumentem (jako `out` parametru /`ByRef` parametr) jest typu `NoBotState`. Reprezentacji ciągu zawiera przyczyny, jeśli sprawdzenie zakończy się niepowodzeniem i `Valid` inaczej. Poniższy kod wyświetla komunikat zgodnie z `NoBot`do wyniku:

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

Na koniec należy formularza w celu przesyłania i elementu label, zwracać komunikat, a wszystko będzie gotowe!

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

Uruchom ten skrypt, a dezaktywować JavaScript lub Prześlij formularz w pierwszych dwóch sekund lub Prześlij formularz siedem razy w ciągu 30 sekund, zostanie wyświetlony komunikat o błędzie. Jednak rozsądny sposób przy użyciu tego formantu, ponieważ tylko około 90-95% użytkownicy mają JavaScript aktywowany, w związku z tym 5 – 10% użytkowników zakończy się niepowodzeniem `NoBot`do testowania.


[![Ten komunikat o błędzie może być spowodowany robotów](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)

Ten komunikat o błędzie może być spowodowany robotów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](fighting-bots-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](fighting-bots-cs.md)
