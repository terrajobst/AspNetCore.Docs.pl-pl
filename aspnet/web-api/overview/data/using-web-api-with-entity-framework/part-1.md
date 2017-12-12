---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: "Używanie składnika Web API 2 z programu Entity Framework 6 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "W tym samouczku zostanie nauczyć się, że tworzenie aplikacji sieci web ze składnika ASP.NET Web API zaplecza. W samouczku Entity Framework 6 dla układ dane..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 42139f8c158dd84cfc30f23c013343348b0c008a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-web-api-2-with-entity-framework-6"></a>Używanie składnika Web API 2 z programu Entity Framework 6
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://github.com/MikeWasson/BookService)

> W tym samouczku zostanie nauczyć się, że tworzenie aplikacji sieci web ze składnika ASP.NET Web API zaplecza. W samouczku Entity Framework 6 dla warstwy danych i Knockout.js dla aplikacji JavaScript po stronie klienta. Samouczek również pokazuje, jak wdrożyć aplikację do aplikacji sieci Web usługi aplikacji Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - 2.1 interfejsu API sieci Web
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> - [Knockout.js](http://knockoutjs.com/) 3.1


Ten samouczek używa ASP.NET Web API 2 z programu Entity Framework 6 do utworzenia aplikacji sieci web, która obsługuje wewnętrznej bazy danych. Poniżej przedstawiono zrzut ekranu aplikacji, która zostanie utworzona.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

Projekt jednostronicowej aplikacji JEDNOSTRONICOWEJ korzysta z aplikacji. "Aplikacji jednostronicowej" to ogólny termin dla aplikacji sieci web, który ładuje pojedynczej strony HTML, a następnie aktualizuje stronę dynamicznie, zamiast ładowania nowych stron. Po załadowaniu strony początkowej aplikacji komunikuje się z serwerem za pośrednictwem żądania AJAX. AJAX żądań zwracanych danych JSON, których aplikacje używają do aktualizacji w Interfejsie użytkownika.

AJAX nie jest nowy, ale obecnie jest struktury języka JavaScript, ułatwiające tworzenie i obsługa dużej aplikacji JEDNOSTRONICOWEJ zaawansowane. W tym samouczku używana [Knockout.js](http://knockoutjs.com/), ale możesz użyć dowolnej architektury JavaScript klienta.

Poniżej przedstawiono główne bloków konstrukcyjnych dla tej aplikacji:

- ASP.NET MVC tworzy stronę HTML.
- Interfejs API sieci Web platformy ASP.NET obsługuje żądania AJAX i zwraca dane JSON.
- Knockout.js danych wiązania elementów HTML w danych JSON.
- Entity Framework komunikuje się z bazą danych.

## <a name="see-this-app-running-on-azure"></a>Zobacz tej aplikacji działających na platformie Azure

Czy chcesz w witrynie Zakończono uruchomione jako aplikacji sieci web? Pełną wersję aplikacji można wdrożyć do konta platformy Azure w celu wystarczy kliknąć poniższy przycisk.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Potrzebujesz konta platformy Azure, aby wdrożyć to rozwiązanie do platformy Azure. Jeśli nie masz już konto, dostępne są następujące opcje:

- [Otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) — otrzymasz kredyt służy do wypróbowania płatnych usług Azure, a nawet po wyczerpaniu kredytu możesz zachować konto i korzystać z bezpłatnych usług platformy Azure.
- [Aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -subskrypcji Your MSDN otrzymasz kredyt, co miesiąc, używanego programu płatnych usług Azure.

## <a name="create-the-project"></a>Tworzenie projektu

Otwórz program Visual Studio. Z **pliku** menu, wybierz opcję **nowy**, a następnie wybierz pozycję **projektu**. (Lub kliknij przycisk **nowy projekt** na stronie początkowej.)

W **nowy projekt** okna dialogowego, kliknij przycisk **Web** w okienku po lewej stronie i **aplikacji sieci Web ASP.NET** w środkowym okienku. Nazwij projekt BookService, a następnie kliknij przycisk **OK**.

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

W **nowy projekt ASP.NET** okno dialogowe, wybierz opcję **interfejsu API sieci Web** szablonu.

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

Jeśli chcesz udostępnić projekt w usłudze Azure App Service, pozostaw **Hostuj w chmurze** zaznaczonym polem.

Kliknij przycisk **OK** Aby utworzyć projekt.

## <a name="configure-azure-settings-optional"></a>Konfigurowanie ustawień platformy Azure (opcjonalnie)

Jeśli **Host w chmurze** zaznaczeniu opcji programu Visual Studio spowoduje wyświetlenie monitu do logowania do systemu Microsoft Azure

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

Po zalogowaniu się do platformy Azure, programu Visual Studio monituje do skonfigurowania aplikacji sieci web. Wprowadź nazwę lokacji, wybierz subskrypcję platformy Azure, a następnie wybierz region geograficzny. W obszarze **serwera bazy danych**, wybierz pozycję **Utwórz nowy serwer**. Wprowadź nazwę użytkownika administratora i hasło.

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

>[!div class="step-by-step"]
[Dalej](part-2.md)
