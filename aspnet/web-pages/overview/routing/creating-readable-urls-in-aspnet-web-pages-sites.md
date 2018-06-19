---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Tworzenie czytelnych adresów URL w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule opisano routingu w witrynie sieci Web platformy ASP.NET Web Pages (Razor) i jak to pozwala używać adresów URL, które są bardziej czytelny i lepsze pod kątem Wyszukiwarek. Po otwarciu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 7858b7cbd6dccafb2867ed9a1d102561e211e435
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573230"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Tworzenie czytelnych adresów URL witryny sieci Web ASP.NET (Razor) stron
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano routingu w witrynie sieci Web platformy ASP.NET Web Pages (Razor) i jak to pozwala używać adresów URL, które są bardziej czytelny i lepsze pod kątem Wyszukiwarek.
> 
> Zawartość:
> 
> - Jak program ASP.NET używa routingu do pozwala korzystać z bardziej czytelny i można wyszukiwać adresów URL.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Strony sieci Web platformy ASP.NET (Razor) 3
>   
> 
> W tym samouczku współdziała również z programu ASP.NET Web Pages 2.


## <a name="about-routing"></a>Informacje routingu

Adresy URL dla stron w witrynie mogą mieć wpływ na jak witryna będzie działać. Adres URL to jest &quot;przyjazną&quot; można ułatwić użytkownikom korzystania z witryny. Może również pomóc z optymalizacji dla aparatów wyszukiwania (SEO) dla tej lokacji. Witryny sieci Web programu ASP.NET umożliwiają automatycznie używać przyjaznych adresów URL.

ASP.NET umożliwia tworzenie łatwy do rozpoznania adresów URL, które opisują akcje użytkownika, a nie tylko wskazuje plik na serwerze. Należy rozważyć te adresy URL dla fikcyjnej blogu:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Porównanie tych adresów URL do następujących protokołów:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

W pierwszym pary, użytkownik musi wiedzieć, że blogu jest wyświetlany za pomocą *blog.cshtml* strony i będzie zmuszony do skonstruowania ciągu zapytania, który pobiera prawo zakres kategorii lub daty. Drugi zestaw przykłady jest znacznie łatwiejsze do uzyskiwania i tworzenia.

Adresy URL, na przykład pierwszy także wskazywać bezpośrednio do określonego pliku (*blog.cshtml*). Jeśli z jakiegoś powodu blogu zostały przeniesione do innego folderu na serwerze lub blogu zostały ulegną używać innej strony, łącza może być problem. Drugi zestaw adresów URL nie wskazuje na określoną stronę, nawet jeśli zmieni się implementacja blogu lub lokalizacji, adresy URL nadal będzie ważny.

W składniku ASP.NET Web Pages można utworzyć bardziej przyjazny adresów URL, podobnie jak w powyższych przykładach, ponieważ program ASP.NET używa *routingu*. Routing tworzy mapowanie logicznych z adres URL do strony (lub stron), który można wykonać żądania. Ponieważ mapowanie jest logiczną (nie fizyczne, do określonego pliku), routing zapewnia dużą elastyczność w sposób definiowania adresów URL dla witryny.

## <a name="how-routing-works"></a>Jak działa Routing

Kiedy ASP.NET przetwarza żądanie, otrzymuje adres URL, aby określić sposób kierowania go. Aplikacja ASP.NET próbuje odpowiada poszczególnych segmentów adresu URL do plików na dysku, przechodząc od lewej do prawej. Jeśli istnieje dopasowanie, niczego pozostałych w adresie URL jest przekazywany do strony jako *informacje o ścieżce*.

Załóżmy, że ktoś wysyła żądanie przy użyciu tego adresu URL:

`http://www.contoso.com/a/b/c`

Wyszukiwanie przechodzi następująco:

1. Istnieje już plik ze ścieżką i nazwą */a/b/c.cshtml*? Jeśli tak, uruchom na tej stronie i przekazywania do niej żadne informacje. W przeciwnym razie...
2. Istnieje już plik ze ścieżką i nazwą */a/b.cshtml*? Jeśli tak, uruchom na tej stronie i przekaż wartość `c` do niego. W przeciwnym razie...
3. Istnieje już plik ze ścieżką i nazwą */a.cshtml*? Jeśli tak, uruchom na tej stronie i przekaż wartość `b/c` do niego.

Jeśli wyszukiwania znaleziono dokładne nie pasuje do *.cshtml* plików w ich określonych folderach, ASP.NET kontynuuje wyszukiwanie tych plików z kolei:

1. */a/b/c/default.cshtml* (nie informacji o ścieżce).
2. */a/b/c/index.cshtml* (nie informacji o ścieżce).

> [!NOTE]
> Być wyczyszczone, żądania dotyczące określonych stron (czyli żądań, które obejmują *.cshtml* rozszerzenie nazwy pliku) działają tak samo, jak można oczekiwać. Żądanie, takich jak `http://www.contoso.com/a/b.cshtml` uruchomi strony *b.cshtml* tylko poprawnie.


Wewnątrz strony, możesz uzyskać informacje o ścieżce za pośrednictwem strony `UrlData` właściwość, która jest słownikiem. Załóżmy, że plik o nazwie *ViewCustomers.cshtml* i lokacji pobiera tego żądania:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Zgodnie z opisem w powyższych reguł, żądanie przejdzie do strony. Wewnątrz strony, aby pobrać i wyświetlić informacje o ścieżce można użyć kodu podobne do poniższych (w tym przypadku wartość &quot;1000&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Ponieważ routingu nie zawiera pełnej nazwy pliku, może istnieć niejednoznaczności w przypadku stron, które mają takie same nazwy, ale inne rozszerzenia nazwy pliku (na przykład *MyPage.cshtml* i *MyPage.html*) . Aby uniknąć problemów z routingiem, najlepiej upewnij się, że nie masz strony w witrynie, których nazwy różnią się jedynie ich rozszerzenia.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

[Program WebMatrix - adresów URL, UrlData i Routing pod kątem Wyszukiwarek](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). Ten wpis w blogu przez Brind Jan zawiera niektóre dodatkowe szczegóły dotyczące działania jak routingu w składniku ASP.NET Web Pages.
