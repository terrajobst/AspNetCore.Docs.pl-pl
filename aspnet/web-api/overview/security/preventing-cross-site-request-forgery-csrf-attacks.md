---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Cross-Site Request Forgery (CSRF) ataku we wzorcu ASP.NET Web API | Dokumentacja firmy Microsoft
author: MikeWasson
description: Opisuje ataku fałszerstwo żądania międzywitrynowego (CSRF) oraz jak wdrożyć środki anti-CSRF w Web API platformy ASP.NET.
ms.author: aspnetcontent
ms.date: 12/12/2012
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: b8cb42120822c56f620f22a4c7fcbc4bfc5141d3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838533"
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a>Cross-Site Request Forgery (CSRF) ataku we wzorcu ASP.NET Web API
====================
przez [Mike Wasson](https://github.com/MikeWasson)

Cross-Site fałszowaniu żądań Międzywitrynowych to atak, w którym złośliwych witryn wysyła żądanie do lokacji narażony, gdzie użytkownik jest aktualnie zalogowany

Oto przykład ataku typu CSRF:

1. Użytkownik loguje się do `www.example.com` za pomocą uwierzytelniania formularzy.
2. Serwer uwierzytelnia użytkownika. Odpowiedź z serwera zawiera pliku cookie uwierzytelniania.
3. Bez wylogowanie, użytkownik odwiedzi złośliwą witrynę sieci web. Ta witryna złośliwego zawiera poniższy formularz HTML: 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Należy zauważyć, że akcji formularza publikuje lokacji narażony, nie do złośliwych witryn. Jest to część "cross-site" CSRF.
4. Użytkownik klika przycisk Prześlij. Przeglądarka zawiera pliku cookie uwierzytelniania z żądaniem.
5. Żądanie jest uruchomiony na serwerze za pomocą kontekstu uwierzytelniania użytkownika i mogą wykonywać wszystkie uwierzytelniony użytkownik może wykonywać.

Mimo że w tym przykładzie wymaga od użytkownika, kliknij przycisk formularz, złośliwy strony można tak samo jak w prosty sposób uruchamiaj skryptu, który automatycznie przesyła formularz. Ponadto przy użyciu protokołu SSL nie atak CSRF, ponieważ złośliwa witryna może wysłać żądanie "https://".

Zazwyczaj ataków CSRF możliwe są względem witryny sieci web, które używają plików cookie uwierzytelniania, ponieważ przeglądarek wysyłać wszystkie odpowiednie pliki cookie do docelowej witryny sieci web. Jednak ataków CSRF nie są ograniczone do wykorzystania plików cookie. Na przykład uwierzytelnianie podstawowe i szyfrowane są również narażone. Po użytkownik loguje się przy użyciu uwierzytelniania podstawowe lub szyfrowane. przeglądarka automatycznie wysyła poświadczenia zakończenia sesji.

## <a name="anti-forgery-tokens"></a>Tokeny zabezpieczające przed fałszerstwem

Aby zapobiec atakom CSRF, ASP.NET MVC korzysta z tokenów zabezpieczających przed sfałszowaniem, nazywany również *żądania weryfikacji tokenów*.

1. Klient żąda strony HTML, który zawiera formularz.
2. Serwer zawiera dwa tokeny w odpowiedzi. Jeden token jest wysyłany jako pliku cookie. Druga jest umieszczany w ukryte pole formularza. Tokeny są generowane losowo, dzięki czemu osoba atakująca nie można odgadnąć wartości.
3. Gdy klient przesyła formularz, jest w stanie wysyłać oba tokeny ponownie do serwera. Klient wysyła ten token pliku cookie jako plik cookie i wysyła tokenu formularza wewnątrz danych formularza. (Klient przeglądarki wykonuje to automatycznie po użytkownik przesyła formularz.)
4. Jeśli żądanie nie zawiera oba tokeny, serwer nie zezwala na żądanie.

Oto przykład z formularza HTML przy użyciu tokenu formularza:

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

Tokeny zabezpieczające przed fałszerstwem działać, ponieważ złośliwy strony nie może odczytać tokenów użytkownika, ze względu na zasady tego samego źródła. ([Zasady tego samego źródła](http://www.w3.org/Security/wiki/Same_Origin_Policy) zapobiec hostowanych w dwóch różnych witrynach dostęp do siebie nawzajem zawartości dokumentów. Dlatego we wcześniejszym przykładzie złośliwego strony mogą wysyłać żądania do example.com, ale nie może odczytać odpowiedzi).

Zapobieganie atakom CSRF, należy użyć tokenów zabezpieczających przed sfałszowaniem przy użyciu dowolnego protokołu uwierzytelniania, gdzie przeglądarka dyskretnie wysyła poświadczenia po użytkownik się loguje. To obejmuje protokoły uwierzytelniania na podstawie plików cookie, takich jak uwierzytelnianie formularzy, a także protokoły, takie jak uwierzytelnianie podstawowe i szyfrowane.

Dla dowolnej metody nonsafe (POST, PUT, DELETE) należy wymagać tokeny zabezpieczające przed fałszerstwem. Ponadto upewnij się, że bezpieczne metody (GET, HEAD) nie ma żadnych efektów ubocznych. Ponadto jeśli włączysz obsługę między domenami, takich jak mechanizmu CORS i JSONP, następnie nawet bezpiecznych metod, takich jak GET są potencjalnie narażone na ataki CSRF, dzięki czemu osoba atakująca odczytać potencjalnie poufnych danych.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Tokeny zabezpieczające przed fałszerstwem we wzorcu ASP.NET MVC

Aby dodać tokeny zabezpieczające przed fałszerstwem do strony Razor, użyj **HtmlHelper.AntiForgeryToken** metody pomocniczej:

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Ta metoda dodaje ukryte pole formularza i ustawia również tokenu pliku cookie.

## <a name="anti-csrf-and-ajax"></a>Anti-CSRF i AJAX

Tokenu formularza może być problemem w przypadku żądań AJAX, ponieważ żądanie AJAX może wysyłać dane JSON, a nie dane formularza HTML. Rozwiązanie polega na wysyłanie tokenów w niestandardowy nagłówek HTTP. Poniższy kod używa składni Razor do generowania tokenów, a następnie dodaje tokenów z żądaniem AJAX. Tokeny są generowane na serwerze, wywołując **AntiForgery.GetTokens**.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Podczas przetwarzania żądania prowadzenie nagłówek żądania tokenów. Następnie wywołaj **AntiForgery.Validate** metody używane do weryfikacji tokenów. **Weryfikacji** metoda zgłasza wyjątek, jeśli tokeny nie są prawidłowe.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
