---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Pomocnik ze strony sieci Web ASP.NET w usłudze Twitter | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym temacie i aplikacji pokazują, jak dodać Pomocnik usługi Twitter do projektu program WebMatrix 3. Zawiera kod Pomocnik usługi Twitter i przedstawiono sposób wywoływania pomocnika...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: fabe9f2b84d278a766dc8d8b7bfc00e1eb967127
ms.sourcegitcommit: 710fc5fcac258cc8415976dc66bdb355b3e061d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52299433"
---
<a name="twitter-helper-with-aspnet-web-pages"></a>Pomocnik usługi Twitter we wzorcu ASP.NET Web Pages
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> [!IMPORTANT]
> Pomocnicy Twitter są nieaktualne. Aby w serwisie Twitter najnowsze narzędzia zaangażowania dla witryn sieci Web, zobacz [usługi Twitter dla witryn sieci Web — Omówienie](https://developer.twitter.com/en/docs/twitter-for-websites/overview).

> W tym temacie i aplikacji pokazują, jak dodać Pomocnik usługi Twitter do projektu program WebMatrix 3. Zawiera kod Pomocnik usługi Twitter i przedstawiono sposób wywoływania metody pomocnika.
> 
> Ten kod do pliku Twitter.cshtml został opracowany przez **Pan niż** przez firmę Microsoft.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.


## <a name="introduction"></a>Wprowadzenie

W tym temacie przedstawiono sposób dodawania Pomocnik usługi Twitter do aplikacji i użyć składni Razor do wywołania metody pomocnika. Pomocnik usługi Twitter ułatwia przycisków w usłudze Twitter i widżetów w aplikacji. Aby użyć elementu widget Twitter, takie jak osi czasu użytkownika lub wyniki wyszukiwania dla hasztagiem, należy najpierw utworzyć [widżetów w serwisie Twitter](https://twitter.com/settings/widgets). Po utworzeniu usługi widżet, zostanie wyświetlony identyfikator elementu widget. Przekażesz ten widżet identyfikator jako parametr podczas wywoływania metody pomocnika, które pokazują elementu widget.

Ten temat został napisany dla wersji 1.1 interfejsu API usługi Twitter. Dodając bezpośrednio kodu Pomocnik usługi Twitter do projektu, należy zaktualizować kod pomocnika zmiany interfejsu API usługi Twitter.

Aby uzyskać informacje o instalowaniu programu WebMatrix, zobacz [Przedstawiamy ASP.NET Web Pages 2 — wprowadzenie](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Dodaj Pomocnik usługi Twitter do projektu

Aby dodać Pomocnik usługi Twitter, należy najpierw dodać folder o nazwie **aplikacji\_kodu** do projektu. Następnie utwórz plik o nazwie **Twitter.cshtml**.

![Folderze App_Code](twitter-helper/_static/image1.png)

Zamień domyślny kod w Twitter.cshtml następujący kod.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Wywoływanie metod Twitter ze stron sieci web

Poniższy przykład przedstawia sposób użycia metody Pomocnik usługi Twitter ze strony w twoim projekcie. W projekcie należy zastąpić wartości parametru z wartościami, które mają zastosowanie do Twoich potrzeb. Identyfikatory podana, widżetów można użyć, aby poznać sposób działania metody, ale chcesz wygenerować własne elementy widget w projekcie.

Nie wszystkie parametry poniżej są wymagane. Następujące parametry opcjonalne są używane do dostosować sposób wyświetlania przycisku lub elementu widget. Na przykład przycisk Obserwuj wymaga tylko nazwę użytkownika, aby wykonać, ale w przykładzie pokazano sposób obejmują liczbę obserwatorów i jak określić rozmiar przycisku i język.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Zobacz wyniki

Powyższy kod generuje następujące przyciski i elementy widget. Te przyciski i elementy widget są w pełni funkcjonalne, nie zrzuty ekranu. Postępuj zgodnie z przycisku jest wyświetlany w języku hiszpańskim, ponieważ ustawiono parametr języka **es**.

### <a name="follow-button"></a>Postępuj zgodnie z przycisku

[Postępuj zgodnie z @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="tweet-button"></a>Przycisk tweetu

[Tweetu](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="user-timeline-profile"></a>Oś czasu użytkownika (profil)

[Tweety według @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="favorites"></a>Ulubione

[Ulubione Tweety według @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="list"></a>Lista

[Opublikuje z @Microsoft/MS \_konsumenta\_Bollingera](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="search"></a>Wyszukaj

[Opublikuje tweet dotyczący &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`
