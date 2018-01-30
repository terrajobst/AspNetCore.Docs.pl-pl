---
uid: whitepapers/request-validation
title: "Żądanie sprawdzania poprawności - zapobieganie atakom skryptu | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym dokumencie opisano funkcję sprawdzania poprawności żądania programu ASP.NET, w którym, domyślnie aplikacji nie będzie mógł przetwarzania niekodowany submitt zawartości HTML..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0b24fe2193d2c7a858667505bad9ed0b1d70a328
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
<a name="request-validation---preventing-script-attacks"></a>Żądanie sprawdzania poprawności - zapobieganie atakom skryptu
====================
> W tym dokumencie opisano funkcję sprawdzania poprawności żądania programu ASP.NET, w którym, domyślnie aplikacji nie będzie mógł przetwarzania niekodowany zawartość HTML przesłane do serwera. Gdy aplikacja zostało zaprojektowane do bezpiecznego przetwarzania danych HTML można wyłączyć tej funkcji sprawdzania poprawności żądania.
> 
> Dotyczy ASP.NET 1.1 i programu ASP.NET 2.0.


Weryfikacja żądania, funkcji programu ASP.NET od wersji 1.1, zapobiega akceptowanie zawartości zawierającego HTML bez zakodowanego przez serwer. Ta funkcja została zaprojektowana, aby zapobiec atakom niektórych uruchomienie skryptu, zgodnie z którymi kodu skryptu klienta lub HTML może być nieświadomie przesłać na serwer, przechowywane i następnie widoczne dla innych użytkowników. Nadal zdecydowanie zaleca się zweryfikowanie wszystkich danych wejściowych, a kodowanie HTML, gdy jest to odpowiednie.

Na przykład można utworzyć strony sieci Web, który żąda adresu e-mail użytkownika, a następnie zapisuje, które adres e-mail w bazie danych. Jeśli użytkownik wprowadzi &lt;skryptu&gt;alert ("Witaj, skryptu")&lt;/SCRIPT&gt; zamiast prawidłowy adres e-mail, gdy dane są prezentowane, ten skrypt mogą być wykonywane, jeśli zawartość nie została poprawnie zaszyfrowana. Funkcja sprawdzania poprawności żądania programu ASP.NET zapobiega to zapobiec.

## <a name="why-this-feature-is-useful"></a>Dlaczego ta funkcja jest przydatna

Wiele witryn nie są znane, że są one otwarte na ataki iniekcji prostego skryptu. Celem takiego ataku jest zamazać lokacji za pomocą wyświetlania HTML lub potencjalnie uruchomienia skryptu klienta w celu przekierowanie użytkownika do witryny to hakerom, ataki uruchomienie skryptu czy problem, który deweloperów sieci Web musi będą konkurować o.

Ataki uruchomienie skryptu są dotyczą wszystkich deweloperów sieci web, czy używają ASP.NET, ASP lub inne technologie projektowania sieci web.

Funkcję weryfikacji żądania ASP.NET aktywnego uniemożliwia ataki, nie zezwalając niekodowany zawartość HTML do przetworzenia przez serwer, chyba że Deweloper decyduje zezwolić tej zawartości.

## <a name="what-to-expect-error-page"></a>Czego można oczekiwać: strona błędu

Ekranu zrzut poniżej przedstawiono niektóre przykładowy kod ASP.NET:

![](request-validation/_static/image1.png)

Uruchomiona wyniki tego kodu w to prosta strona, która pozwala na wprowadzenie tekstu w polu tekstowym, kliknij przycisk i wyświetlanie tekstu w formancie etykiety:

![](request-validation/_static/image2.png)

Jednak były JavaScript, takich jak `<script>alert("hello!")</script>` wprowadzona i przesyłanych może pobrać wyjątek:

![](request-validation/_static/image3.png)

Komunikat o błędzie stwierdza, że "potencjalnie niebezpiecznych Request.Form wykryto wartość" i zawiera więcej szczegółów w opisie dokładnie zaistniałych sytuacji oraz sposobu zmiany zachowania. Na przykład:

Sprawdzania poprawności żądania wykryto potencjalnie niebezpieczną klienta wartości wejściowej, a przetwarzanie żądania zostało przerwane. Ta wartość może wskazywać próbę naruszenia zabezpieczeń aplikacji, takich jak atak skryptowy między witrynami. Sprawdzanie poprawności żądań można wyłączyć, ustawiając `validateRequest=false` w dyrektywie strony lub w sekcji konfiguracji. Jednak zdecydowanie zalecane jest, aby aplikacja jawnie sprawdziła wszystkie dane wejściowe w takim przypadku.

## <a name="disabling-request-validation-on-a-page"></a>Wyłączenie sprawdzania poprawności żądania na stronie

Aby wyłączyć sprawdzanie poprawności żądań na stronie, musisz ustawić `validateRequest` atrybutu dyrektywy strony `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Jeśli weryfikacja żądania jest wyłączone, zawartość może zostać przesłane do strony; jest odpowiedzialność developer strony, aby upewnić się, że zawartość jest poprawnie zakodowany lub przetworzone.

## <a name="disabling-request-validation-for-your-application"></a>Wyłączenie sprawdzania poprawności żądania aplikacji

Aby wyłączyć weryfikację żądań dla aplikacji, należy zmodyfikować lub tworzenie pliku Web.config aplikacji i ustawić atrybut parametr validateRequest `<pages />` sekcji do `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Jeśli chcesz wyłączyć weryfikację żądań dla wszystkich aplikacji na serwerze, istnieje możliwość modyfikacji do pliku Machine.config.

> [!CAUTION]
> Jeśli weryfikacja żądania jest wyłączone, zawartość może zostać przesłane do aplikacji; jest obowiązkiem Deweloper aplikacji, aby upewnić się, że zawartość jest poprawnie zakodowany lub przetworzone.

Poniższy kod zmienia się, aby wyłączyć sprawdzanie poprawności żądań:

![](request-validation/_static/image4.png)

Teraz, jeśli następujące JavaScript została wprowadzona w polu tekstowym `<script>alert("hello!")</script>` będą:

![](request-validation/_static/image5.png)

Aby temu zapobiec, z weryfikacji żądań wyłączone, firma Microsoft potrzebne do formatu HTML kodowania zawartości.

## <a name="how-to-html-encode-content"></a>Jak HTML kodowanie zawartości

Wyłączenie Weryfikacja żądania jest dobrym rozwiązaniem kodowanie HTML zawartości, które będą przechowywane do użytku w przyszłości. Kodowanie HTML automatycznie zastąpią wszelkie "&lt;"lub"&gt;" (wraz z kilku innych symboli) z ich odpowiednich HTML zakodowane reprezentacji. Na przykład "&lt;"zastępuje"&amp;lt;" i "&gt;"zastępuje"&amp;gt;". Przeglądarki używają tych specjalne kody do wyświetlenia "&lt;"lub"&gt;" w przeglądarce.

Zawartość może być łatwo kodowany w formacie HTML przy użyciu serwera `Server.HtmlEncode(string)` interfejsu API. Zawartość można też łatwo HTML-zdekodowany, oznacza to, przywrócić ponownie przy użyciu standardowego kodu HTML `Server.HtmlDecode(string)` metody.

![](request-validation/_static/image6.png)

Wynikiem:

![](request-validation/_static/image7.png)
