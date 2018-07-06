---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publikowanie aplikacji w usłudze Azure usługa Azure App Service | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 66dde7b54ce084eed873afae56fd686d0dc8795f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808230"
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Publikowanie aplikacji w usłudze Azure App Service platformy Azure
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://github.com/MikeWasson/BookService)

W ostatnim kroku opublikujesz aplikację na platformie Azure. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Publikuj**.

![](part-10/_static/image1.png)

Klikając **Publikuj** wywołuje **publikowanie w sieci Web** okna dialogowego. Jeśli zaznaczono **Host w chmurze** podczas pierwszego utworzenia projektu, a następnie połączenie i ustawienia zostały już skonfigurowane. W takim przypadku należy po prostu kliknij **ustawienia** i sprawdź &quot;wykonaj migracje Code First&quot;. (Jeśli nie została sprawdzona **Host w chmurze** na początku, a następnie wykonaj kroki opisane w [następnej sekcji](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Aby wdrożyć aplikację, kliknij przycisk **Publikuj**. Możesz wyświetlić postęp publikowania **działania publikowania internetowego** okna. (Z **widoku** menu, wybierz opcję **Windows inne**, a następnie wybierz **działania publikowania internetowego**.)

![](part-10/_static/image4.png)

Po zakończeniu wdrażania aplikacji programu Visual Studio przeglądarka domyślna automatycznie otwiera adres URL wdrożonych witryn sieci Web i aplikacji, który został utworzony, jest teraz uruchomiona w chmurze. Adres URL w pasku adresu przeglądarki pokazuje, że witryna jest ładowany z Internetu.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Wdrożenie nowej witryny sieci Web

Jeśli nie zaznaczono **Host w chmurze** podczas tworzenia projektu, możesz teraz skonfigurować nową aplikację sieci web. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **Publikuj**. Wybierz **profilu** kartę, a następnie kliknij przycisk **Microsoft Azure Websites**. Nie są obecnie zalogowano do platformy Azure, zostanie wyświetlony monit Zaloguj się.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

W **istniejących witryn sieci Web** okno dialogowe, kliknij przycisk **New**.

![](part-10/_static/image9.png)

Wprowadź nazwę lokacji. Wybierz subskrypcję platformy Azure i region. W obszarze **serwera bazy danych**, wybierz opcję **Utwórz nowy serwer**, lub wybierz istniejący serwer. Kliknij przycisk **Utwórz**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Kliknij przycisk **ustawienia** i sprawdź &quot;wykonaj migracje Code First&quot;. Następnie kliknij przycisk **Publikuj**.

> [!div class="step-by-step"]
> [Poprzednie](part-9.md)
