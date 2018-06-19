---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Śledzenie informacji odwiedzający (Analytics) dla strony sieci Web ASP.NET (Razor) lokacji | Dokumentacja firmy Microsoft
author: tfitzmac
description: Po ich zaakceptujesz przechodzi do witryny sieci Web, można przeanalizować ruchu witryny sieci Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26572933"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Śledzenie informacji odwiedzający (Analytics) dla lokacji (Razor) stron sieci Web ASP.NET
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano, jak dodawać witryny sieci Web analytics do stron w witrynie sieci Web platformy ASP.NET Web Pages (Razor) przy użyciu pomocnika.
> 
> Zawartość:
> 
> - Jak wysyłać informacje o ruchu witryny sieci Web do dostawcy analiz.
> 
> Są to programowania funkcje dodane w artykule programu ASP.NET:
> 
> - `Analytics` Pomocnika.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Strony sieci Web platformy ASP.NET (Razor) 2
> - Biblioteka pomocników sieci Web platformy ASP.NET (pakiet NuGet)


Analytics to ogólny termin technologii, która ruchu w witrynie sieci Web, można zrozumieć, jak użytkownicy korzystają z witryny. Wiele usług analizy są dostępne, łącznie z Google, Yahoo StatCounter i innych usług.

Analiza sposób, działa to, czy utworzysz konto z dostawcy analiz, w którym rejestrowane są lokacji które mają być śledzone. Dostawca wysyła fragment kodu JavaScript, która zawiera identyfikator lub śledzenia kodu dla Twojego konta. Możesz dodać fragment kodu JavaScript do stron sieci web w lokacji, które mają być śledzone. (Zazwyczaj są dodawane fragment analytics stopki lub układ strony lub innych kod znaczników HTML, który pojawia się na każdej stronie w witrynie.) Gdy użytkownik zażąda strony, która zawiera jeden z tych fragmentów kodu JavaScript, fragment wysyła informacje o bieżącej strony do dostawcy analiz, który rejestruje szczegółowe informacje o stronie.

Chcesz przyjrzeć statystyk lokacji logujesz się do witryny sieci Web dostawcy analiz. Można wyświetlić szerokiej gamy raportów o witrynie, takich jak:

- Liczba wyświetleń strony dla poszczególnych stron. Oznacza to, (około) jak wiele osób odwiedzających witrynę, a które strony w witrynie są najbardziej popularnych.
- Jak długo osób wydatkami określone strony. To może określić, np. czy może być utrzymywanie zainteresowania ludzi strony głównej.
- Jakie osób lokacji były na przed ich odwiedzający witrynę. Pomaga to zrozumieć, czy ruchu pochodzi z łącza z wyszukiwanie i tak dalej.
- Gdy osoby odwiedzające witrynę witryny i jak długo pozostają.
- Jakie krajach odwiedzających pochodzą z.
- Jakie przeglądarek i systemów operacyjnych odwiedzających jest używany.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Dodawanie Analytics do strony przy użyciu Pomocnika

Strony sieci Web platformy ASP.NET zawiera kilka wątków analizy (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, i `Analytics.GetStatCounterHtml`) ułatwia zarządzanie przeznaczony analytics fragmenty kodu JavaScript. Zamiast ustaleniem, jak i gdzie do umieść kod JavaScript, musisz wykonać jest dodawanie do strony elementu pomocniczego. Wszystkie informacje potrzebne do zapewnienia jest nazwa konta, identyfikator lub kod śledzenia. (Dla StatCounter, również należy podać kilka dodatkowych wartości.)

W tej procedurze utworzysz stronę układu, która używa `GetGoogleHtml` pomocnika. Jeśli masz już konto, z jednym z innych dostawców analytics, możesz użyć tego konta i dostosować nieznaczne zgodnie z potrzebami.

> [!NOTE]
> Podczas tworzenia konta usługi analytics, możesz zarejestrować adres URL witryny, który ma być śledzenia. Jeśli testujesz wszystko, co na komputerze lokalnym, możesz nie śledzenia ruch rzeczywisty (tylko ruch jest możesz), więc nie będzie mógł rekordu i wyświetlanie statystyk witryny. Jednak w tej procedurze pokazano, jak dodać pomocnika analytics do strony. Podczas publikowania witryny witryny na żywo będzie wysyłać informacje do dostawcy usługi analytics.


1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [pomocników instalowania w lokacji stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze nie zostało dodane.
2. Utworzyć konto w serwisie Google Analytics i zarejestrować nazwę konta.
3. Utwórz stronę układu o nazwie *Analytics.cshtml* i Dodaj następujący kod:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > Należy umieścić wywołanie `Analytics` pomocnika w treści strony sieci web (przed `</body>` tag). W przeciwnym razie przeglądarki nie można uruchomić skryptu.

    Jeśli używasz dostawcy innego analityka, użyj jednej z następujących pomocników:

    - (Yahoo)`@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`
4. Zastąp `myaccount` o nazwie konto, identyfikator lub kod śledzenia, który został utworzony w kroku 1.
5. Uruchom strony w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszar roboczy przed jej uruchomieniem.)
6. W przeglądarce Wyświetl źródło strony. Można wyświetlić kod renderowanym analityka:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Zaloguj się do witryny Google Analytics i sprawdź, czy statystyki dla witryny. Jeśli używasz strony w witrynie na żywo, zobaczysz wpis, który rejestruje wizycie na stronie.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

- [Witryny Google Analytics](https://www.google.com/analytics/)
- [Yahoo! Witryna sieci Web Analytics](http://help.yahoo.com/l/us/yahoo/ywa/)
- [StatCounter lokacji](http://statcounter.com/)
