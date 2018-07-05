---
uid: whitepapers/request-validation
title: Żądanie weryfikacji — zapobieganie atakom skryptów | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym dokumencie opisano funkcję weryfikacji żądania programu ASP.NET, where, domyślnie, aplikacja nie będzie mógł przetwarzania niekodowany submitt zawartości HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: ''
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 783fb1ae27d88f9c6d6d3484d26d3e206e7f2fba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372932"
---
<a name="request-validation---preventing-script-attacks"></a>Żądanie weryfikacji — zapobieganie atakom skryptów
====================
> W tym dokumencie opisano funkcję weryfikacji żądania programu ASP.NET, where, domyślnie, aplikacja nie będzie mógł niekodowany zawartość HTML przesłane do serwera przetwarzania. Gdy aplikacja została zaprojektowana w celu bezpiecznego przetwarzania danych HTML można wyłączyć tej funkcji sprawdzania poprawności żądania.
> 
> Stosuje się do platformy ASP.NET 1.1 i ASP.NET 2.0.


Weryfikacja żądania, funkcji programu ASP.NET od wersji 1.1, uniemożliwia serwera przyjmowanie zawartości zawierającego usunięcie zakodowanym formacie HTML. Ta funkcja została zaprojektowana, aby zapobiec atakom niektóre uruchomienie skryptu, zgodnie z którą kod skryptu klienta lub HTML może być nieświadomie przesłane do serwera, przechowywanych i przedstawiony w innym użytkownikom. Firma Microsoft nadal zdecydowanie zaleca zweryfikowanie wszystkich danych wejściowych i kodowanie HTML, gdy jest to odpowiednie.

Na przykład możesz utworzyć stronę sieci Web, który żąda adresu e-mail użytkownika, a następnie magazynów, które adres e-mail w bazie danych. Jeśli użytkownik wprowadzi &lt;skryptu&gt;alertu ("Witaj, ze skryptu")&lt;/SCRIPT&gt; zamiast prawidłowy adres e-mail, umieszczeniem danych ten skrypt można wykonać Jeśli zawartość nie została poprawnie zaszyfrowana. Funkcję weryfikacji żądania programu ASP.NET uniemożliwia to występuje.

## <a name="why-this-feature-is-useful"></a>Dlaczego ta funkcja jest przydatna

Wiele witryn nie są świadomi, że są one otwarte na ataki przez iniekcję kodu prosty skrypt. Celem tych ataków zamazać witryny, wyświetlając HTML lub potencjalnie wykonywanie skryptu klienta, aby przekierować użytkownika do witryny haker, czy ataki przez iniekcję kodu skryptu są problemem i deweloperów sieci Web muszą zmagać się z.

Ataki przez iniekcję kodu skryptu są dotyczą wszystkich deweloperów sieci web, czy używasz platformy ASP.NET, ASP lub innych technologii sieci web.

Funkcję weryfikacji żądania ASP.NET aktywnie zapobiega atakom przez nie zezwala na niekodowany zawartość HTML do przetworzenia przez serwer, chyba że deweloper zdecyduje się zezwolić na tej zawartości.

## <a name="what-to-expect-error-page"></a>Czego można oczekiwać: strona błędu

Poniższym zrzucie ekranu przedstawiono przykładowy kod platformy ASP.NET:

![](request-validation/_static/image1.png)

Używany kod powoduje to prosta strona, która pozwala na wprowadź jakiś tekst w polu tekstowym, kliknij przycisk, a następnie wyświetlić tekst w kontrolce etykiety:

![](request-validation/_static/image2.png)

Jednak były JavaScript, takie jak `<script>alert("hello!")</script>` wprowadzono i przesyłanych, otrzymamy wynik wyjątek:

![](request-validation/_static/image3.png)

Komunikat o błędzie stwierdzający, że "potencjalnie niebezpiecznych Request.Form wykryto wartość" i zawiera więcej szczegółowych informacji w opisie dokładnie co wydarzyło i jak zmienić zachowanie. Na przykład:

Sprawdzania poprawności żądania wykryto klienta potencjalnie niebezpiecznych wartości wejściowej, a przetwarzanie żądania zostało przerwane. Ta wartość może wskazywać na próbę naruszenia zabezpieczeń aplikacji, takich jak atak skryptów między witrynami. Wyłączono weryfikację żądań, ustawiając `validateRequest=false` w dyrektywie strony lub w sekcji konfiguracji. Jednak zdecydowanie zalecane jest, aby aplikacja jawnie sprawdziła wszystkie dane wejściowe w tym przypadku.

## <a name="disabling-request-validation-on-a-page"></a>Wyłączanie weryfikacji żądania na stronie

Aby wyłączyć sprawdzanie poprawności żądań na stronie należy ustawić `validateRequest` atrybutu dyrektywy strony `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Jeśli weryfikacja żądania jest wyłączone, zawartość można przesyłać do strony jest odpowiedzialność deweloperów strony, aby upewnić się, że zawartość jest poprawnie zakodowany lub przetworzone.

## <a name="disabling-request-validation-for-your-application"></a>Wyłączanie weryfikacji żądań aplikacji

Aby wyłączyć weryfikację żądań dla aplikacji, musisz zmodyfikować lub tworzy plik Web.config dla swojej aplikacji i ustawić atrybut parametr validateRequest `<pages />` sekcji `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Jeśli chcesz wyłączyć weryfikację żądań dla wszystkich aplikacji na serwerze, można wprowadzić modyfikacji do pliku Machine.config.

> [!CAUTION]
> Jeśli weryfikacja żądania jest wyłączone, zawartość można przesyłać do aplikacji; jest odpowiedzialność deweloperów aplikacji, aby upewnić się, że zawartość jest poprawnie zakodowany lub przetworzone.

Poniższy kod jest modyfikowany, aby wyłączyć weryfikację żądań:

![](request-validation/_static/image4.png)

Jeśli następujący kod JavaScript został wprowadzony w polu tekstowym teraz `<script>alert("hello!")</script>` wynik byłby:

![](request-validation/_static/image5.png)

Aby temu zapobiec, z weryfikacji żądań wyłączony, należy w formacie HTML kodujemy zawartości.

## <a name="how-to-html-encode-content"></a>Jak w formacie HTML kodowanie zawartości

Wyłączenie Weryfikacja żądania jest dobrze zawartości kodowanie HTML, które będą przechowywane do użytku w przyszłości. Kodowanie HTML automatycznie spowoduje zastąpienie dowolnego "&lt;"lub"&gt;" (wraz z kilku innych symboli) przy użyciu ich kod HTML zakodowane reprezentacji. Na przykład "&lt;"zastępuje"&amp;lt;" i "&gt;"zastępuje"&amp;gt;". Przeglądarki używają tych specjalne kody do wyświetlenia "&lt;"lub"&gt;" w przeglądarce.

Zawartość może być łatwo kodowany w formacie HTML na serwerze za pomocą `Server.HtmlEncode(string)` interfejsu API. Zawartość można też łatwo HTML-zdekodować, oznacza to, że przywróconymi do standardowego HTML za pomocą `Server.HtmlDecode(string)` metody.

![](request-validation/_static/image6.png)

Wynikiem:

![](request-validation/_static/image7.png)
