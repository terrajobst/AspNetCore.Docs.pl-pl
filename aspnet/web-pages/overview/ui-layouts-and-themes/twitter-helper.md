---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Pomocnik ze stronami sieci Web programu ASP.NET | Dokumentacja firmy Microsoft
author: tfitzmac
description: "Ten temat i aplikacji pokazują, jak dodać usługi Twitter pomocnika do projektu 3 programu WebMatrix. Zawiera kod Pomocnik usługi Twitter i pokazuje sposób wywoływania pomocnika..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 07d9c4d485c42b78a42c54c9740af5f67cb44763
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="twitter-helper-with-aspnet-web-pages"></a>Pomocnik usługi Twitter ze stronami sieci Web ASP.NET
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Ten temat i aplikacji pokazują, jak dodać usługi Twitter pomocnika do projektu 3 programu WebMatrix. Zawiera kod Pomocnik usługi Twitter i przedstawia sposób wywołania metody pomocnicze.
> 
> Ten kod do pliku Twitter.cshtml został opracowany przez **przesuwanie niż** firmy Microsoft.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Strony sieci Web platformy ASP.NET (Razor) 3
>   
> 
> W tym samouczku współdziała również z programu ASP.NET Web Pages 2.


## <a name="introduction"></a>Wprowadzenie

W tym temacie przedstawiono sposób dodawania usługi Twitter pomocnika do aplikacji i użyć składni Razor do wywołania metody pomocnicze. Pomocnik usługi Twitter można łatwo zastosować przycisków Twitter i elementy widget w aplikacji. Aby użyć elementu widget Twitter, takie jak harmonogram użytkownika lub wyniki wyszukiwania hasztagiem, należy najpierw utworzyć [elementu widget w serwisie Twitter](https://twitter.com/settings/widgets). Po utworzeniu sieci widget, otrzymasz identyfikator elementu widget. Ten identyfikator elementu widget należy przekazać jako parametru podczas wywoływania metody pomocnicze, które pokazują elementu widget.

Ten temat został zapisany w wersji 1.1 interfejs API usługi Twitter. Dodając bezpośrednio kodu Pomocnik usługi Twitter do projektu, należy zaktualizować kodu pomocnika w przypadku zmiany interfejs API usługi Twitter.

Aby uzyskać informacje o instalowaniu programu WebMatrix, zobacz [wprowadzenie ASP.NET stron sieci Web 2 — wprowadzenie](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Dodaj do projektu Pomocnik usługi Twitter

Aby dodać Pomocnik usługi Twitter, najpierw Dodaj folder o nazwie **aplikacji\_kod** do projektu. Następnie utwórz plik o nazwie **Twitter.cshtml**.

![Folderze App_Code](twitter-helper/_static/image1.png)

Zastąp następujący kod w kodzie domyślnym w Twitter.cshtml.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Wywołanie metody Twitter ze stron sieci web

Poniższy przykład przedstawia użycie metody Pomocnik usługi Twitter ze strony w projekcie. W projekcie można zastąpić wartości parametru z wartościami, które są odpowiednie do potrzeb. Identyfikatory podanego elementu widget można używać, aby poznać sposób działania metody, ale można generować własne elementy widget dla projektu.

Nie wszystkie parametry pokazano poniżej są wymagane. Parametry opcjonalne są używane do dostosowywania wyświetlania przycisku lub elementu widget. Na przykład przycisk Wykonaj wymaga tylko nazwy użytkownika, które należy wykonać, ale w przykładzie pokazano sposób obejmują liczbę adherentami i jak określić rozmiar przycisku i języka.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Zobacz wyniki

Powyższy kod tworzy następujące przyciski i elementy widget. Tych przycisków i elementy widget są w pełni funkcjonalna, nie zrzuty ekranu. Przycisk Wykonaj jest wyświetlany w języku hiszpańskim, ponieważ ustawiono parametr language **es**.

### <a name="follow-button"></a>Postępuj zgodnie z przycisku

[Postępuj zgodnie z @aspnet)](https://twitter.com/aspnet)<script>! — funkcja (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'; Jeśli (! d.getElementById(id)) {js = d.createElement(s); js.id = Identyfikator; js.src = p + ": / / platform.twitter.com/widgets.js; fjs.parentNode.insertBefore (js, fjs);}} (dokument, "script", "twitter wjs");</script>

### <a name="tweet-button"></a>Przycisk tweet

[Tweet](https://twitter.com/share)<script>! — funkcja (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'; Jeśli (! d.getElementById(id)) {js = d.createElement(s); js.id = Identyfikator; js.src = p + ": / / platform.twitter.com/widgets.js; fjs.parentNode.insertBefore (js, fjs);}} (dokument, "script", "twitter wjs");</script>

### <a name="user-timeline-profile"></a>Oś czasu użytkownika (profil)

[Tweetów przez @aspnet ](https://twitter.com/aspnet) <script>! — funkcja (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'; Jeśli (! d.getElementById(id)) {js = d.createElement(s); js.id = Identyfikator; js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (js, fjs);}} (dokument, "skrypt", "twitter wjs");</script>

### <a name="favorites"></a>Ulubione

[Ulubione Tweetów przez @Microsoft ](https://twitter.com/Microsoft/favorites) <script>! — funkcja (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'; Jeśli (! d.getElementById(id)) {js = d.createElement(s); js.id = Identyfikator; js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (js, fjs);}} (dokument, "skrypt", "twitter wjs");</script>

### <a name="list"></a>Lista

[Tweetów z @Microsoft/MS \_konsumenta\_pasma](https://twitter.com/microsoft/ms-consumer-brands/)<script>! — funkcja (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'; Jeśli (! d.getElementById(id)) {js = d.createElement(s); js.id = Identyfikator; js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (js, fjs);}} (dokument, "skrypt", "twitter wjs");</script>

### <a name="search"></a>Wyszukaj

[Tweetów o &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>! — funkcja (d, s, id) {var js, fjs = d.getElementsByTagName(s) [0], p = /^http:/.test(d.location)? 'http': 'https'; Jeśli (! d.getElementById(id)) {js = d.createElement(s); js.id = Identyfikator; js.src = p + ": / / platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore (js, fjs);}} (dokument, "skrypt", "twitter wjs");</script>
