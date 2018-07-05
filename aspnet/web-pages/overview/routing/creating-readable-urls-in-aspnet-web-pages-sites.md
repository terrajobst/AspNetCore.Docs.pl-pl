---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Tworzenie adresów URL można odczytać we wzorcu ASP.NET Web Pages witryny (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule opisano, routing w witrynie sieci Web ASP.NET Web Pages (Razor) i jak dzięki temu można używać adresów URL, które były bardziej czytelne i lepszym miejscem dla optymalizacji dla aparatów wyszukiwania. Po otwarciu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: d027be2924f24c1080deb4ee2deacc235bbd1001
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385751"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Tworzenie adresów URL do odczytu w witrynach ASP.NET Web Pages (Razor)
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano, routing w witrynie sieci Web ASP.NET Web Pages (Razor) i jak dzięki temu można używać adresów URL, które były bardziej czytelne i lepszym miejscem dla optymalizacji dla aparatów wyszukiwania.
> 
> Zawartość:
> 
> - Jak aplikacja ASP.NET używa routingu pozwala używać bardziej czytelne i którą można przeszukiwać adresów URL.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2.


## <a name="about-routing"></a>Temat routingu

Adresy URL dla strony w witrynie sieci może mieć wpływ na stopnia działania lokacji. Adres URL to &quot;przyjazna&quot; może ułatwić użytkownicy będą mogli korzystać z witryny. Może on również okazać pomocny przy użyciu optymalizacji dla aparatów wyszukiwania (SEO) dla tej witryny. Witryn sieci Web platformy ASP.NET to możliwość używania przyjazne adresy URL automatycznie.

ASP.NET umożliwia tworzenie istotnych adresów URL, które opisują akcje użytkownika, a nie po prostu wskazuje plik na serwerze. Należy wziąć pod uwagę te adresy URL dla fikcyjnej blog:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Porównaj te adresy URL do poniższych:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

Pierwszy parę, użytkownik musi wiedzieć, że blogu jest wyświetlana przy użyciu *blog.cshtml* strony, a następnie miałby do konstruowania ciągu zapytania, który pobiera odpowiednie zakresu kategorii lub daty. Drugi zestaw przykładów jest znacznie łatwiejsze, pojmować i tworzenia.

Adresy URL, na przykład pierwszy też wskazywać bezpośrednio do określonego pliku (*blog.cshtml*). Jeśli z jakiegoś powodu blogu zostały przeniesione do innego folderu na serwerze lub blogu zostały przepisane, aby użyć innej strony, linki może być nieprawidłowa. Drugi zestaw adresów URL nie wskazuje na konkretnej stronie, nawet jeśli zmiany w implementacji blogu lub lokalizacji, adresy URL nadal będzie nieprawidłowa.

W składniku ASP.NET Web Pages można utworzyć bardziej przyjaznej adresy URL, podobnie jak w powyższych przykładach ponieważ aplikacja ASP.NET używa *routingu*. Routing tworzy mapowanie logicznych z adres URL strony (lub stron), który można wykonać żądania. Ponieważ mapowanie jest logiczną (nie fizycznej, do konkretnego pliku), routing zapewnia dużą elastyczność w sposób definiowania adresów URL dla witryny.

## <a name="how-routing-works"></a>Jak działa Routing

ASP.NET przetwarza żądanie, odczytywanych jest adres URL, aby określić sposób kierowania go. Aplikacja ASP.NET próbuje dopasować poszczególnych segmentów adresu URL do plików na dysku, przechodząc od lewej do prawej. W przypadku dopasowania niczego pozostały w adresie URL jest przekazywany do strony jako *informacji o ścieżce*.

Wyobraź sobie, że ktoś sprawia, że żądanie przy użyciu tego adresu URL:

`http://www.contoso.com/a/b/c`

Wyszukiwanie wykracza następująco:

1. Czy istnieje plik o ścieżkę i nazwę */a/b/c.cshtml*? Jeśli tak, uruchom tę stronę i przekaż żadnych informacji do niego. W przeciwnym razie...
2. Czy istnieje plik o ścieżkę i nazwę */a/b.cshtml*? Jeśli tak, uruchom tę stronę i przekaż wartość `c` do niego. W przeciwnym razie...
3. Czy istnieje plik o ścieżkę i nazwę */a.cshtml*? Jeśli tak, uruchom tę stronę i przekaż wartość `b/c` do niego.

Jeśli wyszukiwanie znaleziono dokładne nie jest zgodna dla *.cshtml* pliki w ich określonych folderów, ASP.NET kontynuuje wyszukiwanie te pliki z kolei:

1. */a/b/c/default.cshtml* (nie informacji o ścieżce).
2. */a/b/c/index.cshtml* (nie informacji o ścieżce).

> [!NOTE]
> Aby być niejasne, żądania dotyczące określonych stron (oznacza to, że te żądania, które obejmują *.cshtml* rozszerzenie nazwy pliku) działają tak samo, jak można oczekiwać. Żądanie, takie jak `http://www.contoso.com/a/b.cshtml` uruchomią się stronę *b.cshtml* porządku.


Wewnątrz strony, można uzyskać informacji o ścieżce za pomocą strony `UrlData` właściwość, która jest słownikiem. Wyobraź sobie, że masz plik o nazwie *ViewCustomers.cshtml* i lokacji pobiera tego żądania:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Zgodnie z opisem w powyższych reguł, żądanie przejdzie do strony. Na stronie można użyć kodu, jak pokazano poniżej, aby pobrać i wyświetlić informacje o ścieżce (w tym przypadku wartość &quot;1000&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Ponieważ routingu nie zawiera pełnej nazwy pliku, może istnieć niejednoznaczności w przypadku stron, które mają taką samą nazwę, ale inne rozszerzenia nazwy pliku (na przykład *MyPage.cshtml* i *MyPage.html*) . Aby uniknąć problemów z routingiem, najlepiej upewnij się, że nie masz strony w witrynie, których nazwy różnią się tylko w ich rozszerzenia.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

[Program WebMatrix - adresy URL, UrlData i routingu pod kątem Wyszukiwarek](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). Ten wpis w blogu przez Mike'a Brind zapewnia pewne dodatkowe szczegóły dotyczące działa jak routing w składniku ASP.NET Web Pages.
