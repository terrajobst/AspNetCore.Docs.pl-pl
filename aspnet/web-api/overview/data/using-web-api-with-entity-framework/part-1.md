---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Używanie składnika Web API 2 z platformą Entity Framework 6 | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym samouczku nauczy Cię, że zaplecze podstawy tworzenia aplikacji sieci web za pomocą interfejsu API sieci Web platformy ASP.NET. W tym samouczku użyto programu Entity Framework 6 dla układ danych...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 266c808e3525787181038d2de473194989039e02
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236526"
---
<a name="using-web-api-2-with-entity-framework-6"></a>Używanie wzorca Web API 2 z programem Entity Framework 6
====================

[Pobierz ukończony projekt](https://github.com/MikeWasson/BookService)

> W tym samouczku dowiesz się, że zaplecze podstawy tworzenia aplikacji sieci web za pomocą interfejsu API sieci Web platformy ASP.NET. W samouczku Entity Framework 6 dla warstwy danych i użyciem Knockout.js dla aplikacji JavaScript po stronie klienta. Samouczek przedstawia również sposób wdrażania aplikacji w usłudze Azure App Service Web Apps.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
>
> - Składnik Web API 2.1
> - Program Visual Studio 2017 (Pobierz program Visual Studio 2017 [tutaj](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.7
> - [Knockout.js](http://knockoutjs.com/) 3.1

W tym samouczku za pomocą wzorca ASP.NET Web API 2 platformy Entity Framework 6 do tworzenia aplikacji sieci web, która manipuluje wewnętrznej bazy danych. Poniżej przedstawiono zrzut ekranu aplikacji, która zostanie utworzona.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

Aplikacja używa projektowania aplikacji jednostronicowej (SPA). "Aplikacja jednostronicowa" jest ogólnym terminem dla aplikacji sieci web, która ładuje z pojedynczą stroną HTML, a następnie aktualizuje stronę dynamicznie, zamiast ładowanie nowych stron. Po załadowaniu strony początkowej aplikacji komunikuje się z serwerem za pośrednictwem żądań AJAX. AJAX żądań zwracany danych JSON, których aplikacje używają do aktualizacji interfejsu użytkownika.

AJAX nie jest nowy, ale obecnie ma platformy JavaScript, które ułatwiają tworzenie i zarządzanie nimi dużej zaawansowanych aplikacji SPA. W tym samouczku [struktura Knockout.js](http://knockoutjs.com/), ale można użyć dowolnej architektury klienta JavaScript.

Poniżej przedstawiono główne bloki konstrukcyjne dla tej aplikacji:

- ASP.NET MVC tworzy stronę HTML.
- ASP.NET Web API obsługuje żądania AJAX i zwraca dane JSON.
- Struktura Knockout.js danych — tworzy powiązanie elementów HTML dane JSON.
- Entity Framework komunikuje się z bazą danych.

## <a name="see-this-app-running-on-azure"></a>Zobacz tej aplikacji działających na platformie Azure

Czy chcesz zobaczyć gotową witrynę działającą jako aplikacja internetowa na żywo? Pełną wersję aplikacji można wdrożyć do konta platformy Azure, wybierając poniższy przycisk.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Aby wdrożyć to rozwiązanie na platformie Azure, musisz mieć konto platformy Azure. Jeśli nie masz jeszcze konta, możesz:

- [Otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymujesz środki , które możesz użyć do wypróbowania płatnych usług platformy Azure, a potem nawet w przypadku ich wyczerpania możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- [Aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - w ramach subskrypcji MSDN każdego miesiąca dostajesz środki, dzięki którym możesz korzystać z płatnych usług platformy Azure.

## <a name="create-the-project"></a>Utwórz projekt

Otwórz program Visual Studio. Z **pliku** menu, wybierz opcję **New**, a następnie wybierz **projektu**. (Lub wybierz **nowy projekt** na stronie początkowej.)

W **nowy projekt** okno dialogowe, wybierz opcję **Web** w okienku po lewej stronie i **aplikacji sieci Web platformy ASP.NET (.NET Framework)** w środkowym okienku. Nadaj projektowi nazwę **BookService** i wybierz **OK**.

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **interfejsu API sieci Web** szablonu.

[![](part-1/_static/image12.png)](part-1/_static/image12.png)


Wybierz **OK** do tworzenia projektu.

## <a name="configure-azure-settings-optional"></a>Konfigurowanie ustawień platformy Azure (opcjonalnie)

Po utworzeniu projektu, można wdrożyć do usługi Azure App Service Web Apps w dowolnym momencie. 

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Publikuj**.

2. W wyświetlonym oknie Wybierz **Start**. **Wybierz lokalizację docelową publikowania** pojawi się okno dialogowe.

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. Wybierz **Utwórz profil**. **Tworzenie usługi App Service** pojawi się okno dialogowe.

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   Zaakceptuj wartości domyślne lub wprowadzić inne wartości dla nazwy aplikacji w grupie zasobów, hosting plan subskrypcji platformy Azure i regionu geograficznego. 

4. Wybierz **utworzyć bazę danych SQL**. **Konfiguruj serwer SQL** pojawi się okno dialogowe. 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   Zaakceptuj wartości domyślne lub wprowadzić inne wartości. Wprowadź **nazwa użytkownika administratora** i **hasło administratora** nowej bazy danych. Wybierz **OK** po zakończeniu. **Tworzenie usługi App Service** pojawi się strona.

5. Wybierz **Utwórz** w celu utworzenia profilu. Pojawi się w prawym dolnym rogu, co oznacza, że wdrożenie w toku. Po krótkiej chwili **Publikuj** okna pojawi się ponownie.

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    Profil został utworzony w celu wdrożenia aplikacji jest teraz dostępna. 


> [!div class="step-by-step"]
> [Next](part-2.md)
