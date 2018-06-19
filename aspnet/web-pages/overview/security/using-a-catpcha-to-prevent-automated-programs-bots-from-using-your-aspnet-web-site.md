---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Lokacji przy użyciu CAPTCHA, aby uniemożliwić korzystanie z sieci Web ASP.NET Razor robotów) | Dokumentacja firmy Microsoft
author: microsoft
description: W tym artykule opisano sposób użycia ReCaptcha (miara zabezpieczeń), aby zapobiec próbom (robotów) wykonywanie zadań w strony sieci Web ASP.NET (Razor) firma Microsoft...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2012
ms.topic: article
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: 75e80a3e7ebe787852152404bf2e0bf88a1a6a56
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573281"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Lokacji przy użyciu CAPTCHA, aby uniemożliwić korzystanie z sieci Web ASP.NET Razor robotów)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> W tym artykule opisano sposób użycia ReCaptcha (miara zabezpieczeń), aby zapobiec próbom (robotów) wykonywanie zadań w witrynie sieci Web platformy ASP.NET Web Pages (Razor).
> 
> **Zawartość:** 
> 
> - Jak dodać testu CAPTCHA do witryny.
> 
> Poniżej przedstawiono funkcje platformy ASP.NET, wprowadzone w artykule:
> 
> - `ReCaptcha` Pomocnika.
> 
> > [!NOTE]
> > Informacje przedstawione w tym artykule dotyczą 1.0 stron sieci Web ASP.NET i 2 stron sieci Web.


## <a name="about-captchas"></a>O CAPTCHAs

Kiedykolwiek let osób zarejestrować w witrynie lub nawet po prostu wprowadź nazwę i adres URL (jak komentarza blogu), może spowodować, że nadmiernej fałszywych nazw. Te często są zapisywane przez programy automatyczne (robotów), które próbuje pozostaw adresy URL w każdej witryny sieci Web, które można znaleźć. (Typowe motywacją jest opublikowania adresów URL produktów na sprzedaż).

Można je, upewnij się, że użytkownik jest prawdziwa osoba, a nie program komputerowy przy użyciu *CAPTCHA* do weryfikowania użytkowników podczas rejestrowania lub w przeciwnym razie wprowadzić nazwę i witryny. CAPTCHA oznacza testu całkowicie zautomatyzowanego publicznego Turing stwierdzić, komputerów i ludzi od siebie. Jest CAPTCHA *odpowiedź na żądanie* test w którym użytkownik jest proszony o czymś, który umożliwia łatwe osoby robić, ale trudne automatycznych programu do. Najczęściej spotykanym typem CAPTCHA jest jednym gdzie Zobacz niektóre litery zniekształcony i musiał wpisać je. (Zakłócenia powinien być trudne dla robotów do odszyfrowania litery.)

## <a name="adding-a-recaptcha-test"></a>Dodawanie testu ReCaptcha

Strony ASP.NET można `ReCaptcha` pomocnika do renderowania test CAPTCHA, który jest oparty na usłudze ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). `ReCaptcha` Pomocnik wyświetli obraz dwóch zniekształcony słowa, które użytkownicy musieli wprowadzać prawidłowo przed sprawdzania poprawności strony. Odpowiedź użytkownika jest zweryfikowana przez usługę ReCaptcha.Net.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Zarejestruj witryny sieci Web w ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Po zakończeniu rejestracji zostanie wyświetlony klucz publiczny i klucz prywatny.
2. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli nie jest jeszcze.
3. Jeśli nie masz jeszcze  *\_AppStart.cshtml* plików, w folderze głównym witryny sieci Web Utwórz plik o nazwie  *\_AppStart.cshtml*.
4. Dodaj następujące `Recaptcha` ustawienia pomocy w  *\_AppStart.cshtml* pliku: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Ustaw `PublicKey` i `PrivateKey` właściwości za pomocą własnych kluczy publicznych i prywatnych.
6. Zapisz  *\_AppStart.cshtml* plik i zamknij go.
7. W folderze głównym witryny sieci Web, Utwórz nową stronę o nazwie *Recaptcha.cshtml*.
8. Zastąp istniejącą zawartość następujących czynności: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Uruchom *Recaptcha.cshtml* strony w przeglądarce. Jeśli `PrivateKey` wartość jest prawidłowa, zostaje wyświetlona strona kontroli ReCaptcha i przycisk. Jeśli nie ma ustawić klucze globalnie w  *\_AppStart.html*, strona będzie wyświetlana wystąpił błąd. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Wprowadź słowa dla testu. W przypadku przekazania testu ReCaptcha, zobaczysz komunikat w tym celu. W przeciwnym razie zostanie wyświetlony komunikat o błędzie i kontroli ReCaptcha, zostanie wyświetlony ponownie.

> [!NOTE]
> Jeśli komputer znajduje się w domenie, która używa serwera proxy, może być konieczne skonfigurowanie `defaultproxy` elementu *Web.config* pliku. W poniższym przykładzie przedstawiono *Web.config* pliku z `defaultproxy` element skonfigurowany tak, aby włączyć usługę ReCaptcha do pracy.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


- [Dostosowywanie zachowania całej lokacji dla lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
- [ReCaptcha lokacji](https://www.google.com/recaptcha)
