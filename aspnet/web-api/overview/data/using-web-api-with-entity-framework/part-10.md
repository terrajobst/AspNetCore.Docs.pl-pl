---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publikowanie aplikacji w usłudze Azure usługa Azure App Service | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 5c1a70ceded85681046065881f62c5c95c5d8740
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752265"
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Publikowanie aplikacji w usłudze Azure App Service platformy Azure
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://github.com/MikeWasson/BookService)

W ostatnim kroku opublikujesz aplikację na platformie Azure. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy na projekt i wybierz **Publikuj**.

![](part-10/_static/image1.png)

Kliknięcie **Publikuj** wywołuje okno dialogowe **Publikowanie w sieci Web**. Jeśli podczas tworzenia projektu zaznaczono **Host w chmurze**, to połączenie i ustawienia zostały już skonfigurowane. W takim przypadku należy po prostu kliknąć **Ustawienia** i zaznaczyć opcję &quot;wykonaj migracje Code First&quot;. (Jeśli na początku nie została zaznaczona opcja **Host w chmurze**, wykonaj kroki opisane w [następnej sekcji](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Aby wdrożyć aplikację, kliknij przycisk **Publikuj**. Możesz wyświetlić postęp publikowania w oknie **Działania publikowania internetowego**. (Z menu **Widok** wybierz opcję **Windows inne**, a następnie wybierz **Działania publikowania internetowego**.)

![](part-10/_static/image4.png)

Po zakończeniu wdrażania aplikacji w programie Visual Studio zostanie automatycznie uruchomiona przeglądarka domyślna, która przejdzie pod adres URL wdrożonej witryny sieci Web. Utworzona aplikacja jest teraz uruchomione w chmurze. Adres URL w pasku adresu przeglądarki pokazuje, że witryna jest ładowana z Internetu.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Wdrożenie nowej witryny sieci Web

Jeśli nie zaznaczono opcji **Host w chmurze** podczas tworzenia projektu, możesz teraz skonfigurować nową aplikację sieci web. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Publikuj**. Następnie wybierz kartę **Profil**, po czym kliknij przycisk **Microsoft Azure Websites**. Jeśli nie jesteś zalogowany do Azure, wyświetli się monit o zalogowanie.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

W oknie dialogowym **istniejących witryn sieci Web** kliknij przycisk **New**.

![](part-10/_static/image9.png)

Wprowadź nazwę lokacji. Wybierz subskrypcję platformy Azure i region. W obszarze **serwera bazy danych**, wybierz opcję **Utwórz nowy serwer**, lub wybierz istniejący serwer. Kliknij przycisk **Utwórz**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Kliknij przycisk **Ustawienia** i zaznacz opcję &quot;Wykonaj migracje Code First&quot;. Następnie kliknij przycisk **Publikuj**.

> [!div class="step-by-step"]
> [Poprzednie](part-9.md)
