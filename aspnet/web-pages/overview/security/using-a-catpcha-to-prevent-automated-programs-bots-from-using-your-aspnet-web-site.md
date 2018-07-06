---
uid: web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
title: Korzystanie z mechanizmu CAPTCHA, aby uniemożliwić Boty przy użyciu usługi sieci Web platformy ASP.NET Razor) lokacji | Dokumentacja firmy Microsoft
author: microsoft
description: W tym artykule wyjaśniono, jak użyć ReCaptcha (miara zabezpieczeń), aby uniemożliwić automatyczne programom (robotom) z wykonywania zadań w stron ASP.NET Web Pages (Razor) firma Microsoft...
ms.author: aspnetcontent
ms.date: 05/21/2012
ms.assetid: 2b381a41-2cb3-40c0-8545-1d393e22877f
msc.legacyurl: /web-pages/overview/security/using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site
msc.type: authoredcontent
ms.openlocfilehash: f67eb60c23e0eec46089ceea9b04779492dfa15e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803074"
---
<a name="using-a-captcha-to-prevent-bots-from-using-your-aspnet-web-razor-site"></a>Korzystanie z mechanizmu CAPTCHA, aby uniemożliwić Boty przy użyciu usługi sieci Web platformy ASP.NET Razor) lokacji
====================
przez [firmy Microsoft](https://github.com/microsoft)

> W tym artykule wyjaśniono, jak użyć ReCaptcha (miara zabezpieczeń), aby uniemożliwić automatyczne programom (robotom) z wykonywania zadań w witrynie internetowej ASP.NET Web Pages (Razor).
> 
> **Zawartość:** 
> 
> - Jak dodać CAPTCHA test do witryny.
> 
> Poniżej przedstawiono funkcje platformy ASP.NET, wprowadzona w artykule:
> 
> - `ReCaptcha` Pomocnika.
> 
> > [!NOTE]
> > Informacje przedstawione w tym artykule dotyczy 1.0 stron sieci Web platformy ASP.NET i Web Pages 2.


## <a name="about-captchas"></a>Temat CAPTCHAs

Ilekroć umożliwiają rejestrowanie osób w lokacji lub nawet po prostu wprowadź nazwę i adres URL (na przykład dla komentarza blog), możesz otrzymać nadmiernej ilości fałszywych nazwy. Pozostają one często przez automatyczne programom (robotom), których podejmowana jest próba pozostaw adresy URL w każdej witryny sieci Web, którą można znaleźć. (Typowe motywacja jest do opublikowania adresów URL produktów do sprzedaży).

Możesz pomóc, upewnij się, że użytkownik jest prawdziwa osoba, a nie program komputerowy przy użyciu *CAPTCHA* do weryfikowania użytkowników podczas rejestrowania, lub w przeciwnym razie podał swoją nazwę i witryny. CAPTCHA oznacza całkowicie zautomatyzowanego publicznych Turing test, aby stwierdzić, komputerów i ludzie od siebie. Jest CAPTCHA *odpowiedź na żądanie* test w którym użytkownik jest proszony o czymś, który umożliwia łatwe do osoby w celu ale trudne automatycznych programu w celu. Najczęściej spotykanym typem CAPTCHA jest jednym gdzie można zobaczyć niektóre litery zniekształcony, a monit o ich typu. (Zakłócenia powinien utrudnić botów do odszyfrowania litery.)

## <a name="adding-a-recaptcha-test"></a>Dodawanie testu ReCaptcha

Na stronach ASP.NET, można użyć `ReCaptcha` element pomocniczy służący do renderowania test CAPTCHA, który jest oparty na usłudze ReCaptcha ([http://recaptcha.net](http://recaptcha.net)). `ReCaptcha` Pomocnik wyświetli obraz dwa zniekształcony wyrazy, które użytkownicy muszą wprowadzić poprawnie, zanim zostanie zweryfikowana strony. Odpowiedź użytkownika jest weryfikowana przez usługę ReCaptcha.Net.

![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.jpg)

1. Zarejestruj swoje witryny sieci Web ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Po zakończeniu rejestracji zostanie wyświetlony klucz publiczny i klucz prywatny.
2. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [instalowanie obiekty pomocnicze w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze go.
3. Jeśli nie masz jeszcze  *\_AppStart.cshtml* plików, w folderze głównym witryny sieci Web Utwórz plik o nazwie  *\_AppStart.cshtml*.
4. Dodaj następujący kod `Recaptcha` ustawienia Pomocnika  *\_AppStart.cshtml* pliku: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample1.cshtml?highlight=6-7)]
5. Ustaw `PublicKey` i `PrivateKey` właściwości za pomocą własnych kluczy publicznych i prywatnych.
6. Zapisz  *\_AppStart.cshtml* plik i zamknij go.
7. W folderze głównym witryny sieci Web, należy utworzyć nową stronę o nazwie *Recaptcha.cshtml*.
8. Zastąp istniejącą zawartość następujących czynności: 

    [!code-cshtml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample2.cshtml)]
9. Uruchom *Recaptcha.cshtml* strony w przeglądarce. Jeśli `PrivateKey` wartość jest prawidłowa, na stronie jest wyświetlany formantu ReCaptcha i przycisk. Jeśli nie było ustawić klucze globalnie w  *\_AppStart.html*, strony zostanie wyświetlony błąd. 

    ![](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/_static/image1.png)
10. Wprowadź wyrażenie dla testu. Jeśli przekażesz testu ReCaptcha, zobaczysz komunikat w tym celu. W przeciwnym razie zostanie wyświetlony komunikat o błędzie i kontroli ReCaptcha, zostanie wyświetlony ponownie.

> [!NOTE]
> Jeśli komputer znajduje się w domenie, która używa serwera proxy, może być konieczne skonfigurowanie `defaultproxy` elementu *Web.config* pliku. W poniższym przykładzie przedstawiono *Web.config* plik z `defaultproxy` element skonfigurowana w celu umożliwienia działania usługi ReCaptcha.
> 
> [!code-xml[Main](using-a-catpcha-to-prevent-automated-programs-bots-from-using-your-aspnet-web-site/samples/sample3.xml)]


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


- [Dostosowywanie zachowania dla całej witryny dla witrynach ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
- [ReCaptcha lokacji](https://www.google.com/recaptcha)
