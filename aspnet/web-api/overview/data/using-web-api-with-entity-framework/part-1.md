---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Używanie składnika Web API 2 z platformą Entity Framework 6 | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym samouczku nauczy Cię, że zaplecze podstawy tworzenia aplikacji sieci web za pomocą interfejsu API sieci Web platformy ASP.NET. W tym samouczku użyto programu Entity Framework 6 dla układ danych...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: d65c0ea35ec766ef9d9093c6502230f9de72a3f3
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795217"
---
<a name="using-web-api-2-with-entity-framework-6"></a>Używanie składnika Web API 2 z platformą Entity Framework 6
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://github.com/MikeWasson/BookService)

> W tym samouczku nauczy Cię, że zaplecze podstawy tworzenia aplikacji sieci web za pomocą interfejsu API sieci Web platformy ASP.NET. W samouczku Entity Framework 6 dla warstwy danych i użyciem Knockout.js dla aplikacji JavaScript po stronie klienta. Samouczek przedstawia również sposób wdrażania aplikacji w usłudze Azure App Service Web Apps.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
>
> - Składnik Web API 2.1
> - Visual Studio 2013 (Pobierz program Visual Studio 2017 [tutaj](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.5
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

Czy chcesz w witrynie Zakończono działającego jako aplikacja sieci web? Pełną wersję aplikacji można wdrożyć do konta platformy Azure, po prostu kliknąć poniższy przycisk.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Potrzebujesz konta platformy Azure, aby wdrożyć to rozwiązanie na platformie Azure. Jeśli nie masz już konto, masz następujące opcje:

- [Otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług platformy Azure, a nawet w przypadku, po ich wyczerpaniu nawet możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- [Aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -ramach subskrypcji MSDN daje środki na korzystanie z każdego miesiąca, używanego do płatne usługi platformy Azure.

## <a name="create-the-project"></a>Tworzenie projektu

Otwórz program Visual Studio. Z **pliku** menu, wybierz opcję **New**, a następnie wybierz **projektu**. (Lub kliknij przycisk **nowy projekt** na stronie początkowej.)

W **nowy projekt** okno dialogowe, kliknij przycisk **Web** w okienku po lewej stronie i **aplikacji sieci Web ASP.NET** w środkowym okienku. Nazwij projekt BookService, a następnie kliknij przycisk **OK**.

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **interfejsu API sieci Web** szablonu.

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

Do hostowania projektu w usłudze Azure App Service, należy pozostawić **Hostuj w chmurze** polem.

Kliknij przycisk **OK** do tworzenia projektu.

## <a name="configure-azure-settings-optional"></a>Konfigurowanie ustawień platformy Azure (opcjonalnie)

Jeśli pozostawiono **Host w chmurze** zaznaczeniu opcji programu Visual Studio wyświetli monit do logowania do systemu Microsoft Azure

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

Po zalogowaniu się do platformy Azure w Visual Studio zostanie wyświetlony monit o skonfigurowanie aplikacji sieci web. Wprowadź nazwę dla tej witryny, wybierz swoją subskrypcję platformy Azure i wybierz region geograficzny. W obszarze **serwera bazy danych**, wybierz opcję **Utwórz nowy serwer**. Wprowadź nazwę użytkownika administratora i hasło.

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [Next](part-2.md)
