---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: "Zapobieganie Cross-Site (CSRF) Fałszerstwie żądania w składniku ASP.NET Web API | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "Opisuje żądania międzywitrynowego ataku sfałszowaniem (CSRF) oraz wdrożenie środków anti-CSRF w interfejsu API sieci Web platformy ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 1cd03f3b396cc2ece1d8dbe6820f6277c02d8e62
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-web-api"></a>Zapobieganie Cross-Site (CSRF) Fałszerstwie żądania w składniku ASP.NET Web API
====================
przez [Wasson Jan](https://github.com/MikeWasson)

Sfałszowaniem żądania między witrynami (CSRF) jest atak, gdzie niebezpiecznej witryny wysyła żądanie do narażone lokacji, w którym użytkownik jest aktualnie zalogowany

Oto przykład ataku CSRF:

1. Uwierzytelnianie przy użyciu formularzy logowania użytkownika do www.example.com.
2. Serwer uwierzytelnia użytkownika. Odpowiedź z serwera zawiera pliku cookie uwierzytelniania.
3. Bez rejestrowania, użytkownik odwiedzi złośliwą witrynę sieci web. Ta witryna złośliwego zawiera poniższy formularz HTML: 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Należy zauważyć, że akcja formularza zapisuje do lokacji narażony, nie niebezpiecznej witryny. Jest to część CSRF "cross-site".
4. Użytkownik klika przycisk Prześlij. Przeglądarka zawiera pliku cookie uwierzytelniania z żądaniem.
5. Żądanie działa na serwerze z kontekstem uwierzytelniania użytkownika i mogą wykonywać wszystkie uwierzytelniony użytkownik może wykonywać.

Mimo że w tym przykładzie wymaga od użytkownika kliknij przycisk formularz, strony złośliwych może równie łatwo uruchomić skrypt, który automatycznie wyśle formularz. Ponadto przy użyciu protokołu SSL nie atak CSRF, ponieważ złośliwa witryna można wysyłać żądania "https://".

Zazwyczaj CSRF ataki są możliwe przed witryn sieci web, które używają plików cookie do uwierzytelniania, ponieważ przeglądarki wysyłać wszystkich odpowiednich plików cookie do docelowej witryny sieci web. Jednak ataków CSRF nie są ograniczone do wykorzystania plików cookie. Na przykład uwierzytelnianie podstawowe i szyfrowane również są zagrożone. Po zalogowaniu się użytkownika za pomocą uwierzytelnianie podstawowe lub szyfrowane. przeglądarka automatycznie wysyła poświadczenia zakończenia sesji.

## <a name="anti-forgery-tokens"></a>Tokenów zabezpieczających przed sfałszowaniem

Aby zapobiec atakom CSRF, ASP.NET MVC korzysta z tokenów zabezpieczających przed sfałszowaniem, nazywany również *żądania weryfikacji tokenów*.

1. Strona HTML, która zawiera formularz żądania klienta.
2. Serwer zawiera dwa tokeny w odpowiedzi. Jeden token jest wysyłany jako plik cookie. Druga znajduje się w polu ukrytym. Tokeny są generowane losowo tak, aby Atakujący dokonuje nie można odgadnąć wartości.
3. Gdy klient przesyła formularz, przesyła oba tokeny do serwera. Klient wysyła ten token pliku cookie jako plik cookie, a następnie wysyła tokenu formularza wewnątrz dane formularza. (Klient przeglądarki automatycznie dzieje się tak, gdy użytkownik przesyła formularz.)
4. Jeśli żądanie nie zawiera oba tokeny, serwer nie zezwala na żądanie.

Oto przykład formularza HTML przy użyciu tokenu w ukrytym:

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

Tokenów zabezpieczających przed sfałszowaniem działać, ponieważ złośliwy strony nie może odczytać tokenów użytkownika, z powodu zasad tego samego źródła. ([Zasady samego pochodzenia](http://www.w3.org/Security/wiki/Same_Origin_Policy) dokumentów hostowanej w dwóch różnych witrynach dostęp do jego zawartości. Dlatego w przypadku poprzedniego przykładu, strony złośliwych mogą wysyłać żądania do example.com, ale nie można odczytać odpowiedzi).

Aby zapobiegać atakom CSRF, użyj tokenów zabezpieczających przed sfałszowaniem z dowolnego protokołu uwierzytelniania, których przeglądarka dyskretnie wysyła poświadczenia po zalogowaniu się użytkownika. To zawiera protokoły uwierzytelniania opartego na pliku cookie, takich jak uwierzytelnianie formularzy, a także protokołów, takich jak uwierzytelnianie podstawowe i szyfrowane.

Dla żadnych metod nonsafe (POST, PUT, DELETE), należy włączyć tokenów zabezpieczających przed sfałszowaniem. Ponadto upewnij się, że bezpieczne metody (GET, HEAD) nie mają żadnych efektów ubocznych. Ponadto jeśli zostanie włączona obsługa między domenami, takie jak CORS lub JSONP, następnie nawet bezpiecznych metod, takich jak GET są potencjalnie narażone na ataki CSRF, co umożliwi jej uzyskanie odczytać potencjalnie poufnych danych.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Tokenów zabezpieczających przed sfałszowaniem na platformie ASP.NET MVC

Aby dodać tokenów zabezpieczających przed sfałszowaniem do strony Razor, użyj **HtmlHelper.AntiForgeryToken** metody pomocniczej:

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Ta metoda dodaje ukryte pole formularza i określa również token pliku cookie.

## <a name="anti-csrf-and-ajax"></a>Anti-CSRF i AJAX

Tokenu formularza może to stanowić problem dla żądania AJAX, ponieważ żądanie AJAX może wysyłać dane JSON, a nie dane formularza HTML. Rozwiązanie polega na wysyłanie tokenów w niestandardowy nagłówek HTTP. Poniższy kod używa składni Razor do generowania tokenów, a następnie dodaje tokenów na żądanie AJAX. Tokeny są generowane na serwerze przez wywołanie metody **AntiForgery.GetTokens**.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Podczas przetwarzania żądania, Wyodrębnij tokenów w nagłówku żądania. Następnie wywołaj **AntiForgery.Validate** metody do weryfikacji tokenów. **Weryfikacji** metoda zgłasza wyjątek, jeśli tokenów nie są prawidłowe.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
