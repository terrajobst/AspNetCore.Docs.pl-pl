---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Śledzenie informacji odwiedzający (analiza) for an ASP.NET Web Pages (Razor) lokacji | Dokumentacja firmy Microsoft
author: tfitzmac
description: Po trafiła do Ciebie witryny sieci Web, pracę, możesz chcieć analizowanie ruchu witryny sieci Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 48782606083b4aa1e32adf6163bcb3f2d9828bc3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387140"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Obiekt odwiedzający informacji (analiza) witrynie ASP.NET Web Pages (Razor) ze śledzenia
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano, jak dodać analizy witryn sieci Web do stron w witrynie internetowej ASP.NET Web Pages (Razor) za pomocą pomocnika.
> 
> Zawartość:
> 
> - Jak wysyłać informacje o ruchu witryny sieci Web do dostawcy analiz.
> 
> Poniżej przedstawiono funkcje wprowadzone w artykule programowania programu ASP.NET:
> 
> - `Analytics` Pomocnika.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - Biblioteka pomocników sieci Web platformy ASP.NET (pakiet NuGet)


Analiza jest ogólnym terminem dla technologii, która mierzy ruch w witrynie sieci Web, aby zrozumieć, jak użytkownicy korzystają z witryny. Wiele usług analizy są dostępne, łącznie z usługami Google, Yahoo, StatCounter i innych.

Analiza sposób, którego działa, można założyć konto za pomocą dostawcy analiz, w której możesz zarejestrować lokację, mają być śledzone. Dostawca wysyła fragment kodu JavaScript, która zawiera identyfikator lub kod konta śledzenia. Dodaj fragment kodu JavaScript do stron sieci web w lokacji, który chcesz śledzić. (Zazwyczaj dodajesz fragment analytics stopki lub układ strony lub innych znaczników HTML, który pojawia się na każdej stronie w witrynie.) Gdy użytkownicy żądają strony, która zawiera jeden z tych fragmentów kodu JavaScript, fragment kodu wysyła informacje o bieżącej strony do dostawcy analiz, który rejestruje szczegółowe informacje o stronie.

Chcesz się jej przyjrzeć statystyk lokacji logujesz się do witryny sieci Web dostawcy analiz. Można wyświetlić różne rodzaje raportów o witrynie, takich jak:

- Liczba wyświetleń stron dla poszczególnych stron. Oznacza to, (około), jak wiele osób odwiedzających witrynę, a które strony w witrynie są najbardziej popularne.
- Ile osób możesz wydać na określone strony. To może określić, np. czy strona główna może być utrzymywanie zainteresowania osób.
- Jakie witryn osób były na przed ich odwiedzane witryny. Pomaga to zrozumieć, czy ruch sieciowy pochodzi z łącza z wyszukiwania i tak dalej.
- Gdy ludzie odwiedzić witrynę i jak długo pozostają.
- Jakich krajach odwiedzających pochodzą z.
- Jakie przeglądarki i systemy operacyjne korzystają z odwiedzających.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Dodawanie analizy do strony przy użyciu Pomocnika

ASP.NET Web Pages obejmuje kilka pomocników analytics (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, i `Analytics.GetStatCounterHtml`) ułatwia zarządzanie fragmenty kodu JavaScript, używane do analizy. Zamiast ustalenie, jak i gdzie umieścić kod JavaScript wszystko, co należy zrobić, to dodanie pomocnika do strony. Wszystkie informacje potrzebne do zapewnienia jest nazwa konta, identyfikator lub kod śledzenia. (Dla StatCounter, również należy podać kilka dodatkowych wartości.)

W tej procedurze utworzysz stronę układu, który używa `GetGoogleHtml` pomocnika. Jeśli masz już konto, przy użyciu jednego z innych dostawców analytics, możesz zamiast tego użyj tego konta i wprowadzić niewielkie korekty, zgodnie z potrzebami.

> [!NOTE]
> Podczas tworzenia konta usługi analytics, możesz zarejestrować adres URL witryny, która ma być śledzenia. Jeśli testujesz wszystko na komputerze lokalnym, użytkownik nie będzie śledzenia rzeczywisty ruch (tylko ruch jest użytkownik), dzięki czemu nie będzie mógł rejestrowanie i wyświetlanie statystyk witryny. Ale w tej procedurze pokazano, jak dodać pomocnika analytics do strony. Podczas publikowania witryny działającej witryny będzie wysyłać informacje do dostawcy analiz.


1. Dodaj bibliotekę pomocników platformy ASP.NET sieci Web do witryny sieci Web, zgodnie z opisem w [instalowanie obiekty pomocnicze w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze nie został dodany.
2. Utwórz konto za pomocą usługi Google Analytics i zapisać jego nazwę konta.
3. Tworzenie układu strony o nazwie *Analytics.cshtml* i Dodaj następujący kod:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > Należy umieścić wywołanie `Analytics` pomocnika w treści strony sieci web (przed `</body>` tagu). W przeciwnym razie przeglądarki nie uruchomi skrypt.

    Jeśli używasz dostawcy analizy różnych użyć jednej z następujących pomocników:

    - (Yahoo) `@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`
4. Zastąp `myaccount` o nazwie konto, identyfikator lub kod śledzenia, który został utworzony w kroku 1.
5. Uruchom stronę w przeglądarce. (Upewnij się, że strona jest zaznaczona w **pliki** obszaru roboczego przed jej uruchomieniem.)
6. W przeglądarce Wyświetl źródło strony. Będzie można wyświetlić kod renderowanych analytics:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Zaloguj się w witrynie Google Analytics i zbadaj statystyki dla danej witryny. Jeśli korzystasz z stronę w witrynie na żywo, zostanie wyświetlony wpis, który rejestruje odwiedziny na stronę.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby

- [Witryna usługi Google Analytics](https://www.google.com/analytics/)
- [Yahoo! Analiza witrynę sieci Web](http://help.yahoo.com/l/us/yahoo/ywa/)
- [StatCounter lokacji](http://statcounter.com/)
