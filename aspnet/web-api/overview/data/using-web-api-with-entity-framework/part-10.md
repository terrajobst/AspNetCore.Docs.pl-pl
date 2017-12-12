---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: "Publikowanie aplikacji w usłudze Azure Azure App Service | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 08994815cb339800619caacdcb8d717e9986f9d5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Publikowanie aplikacji w usłudze Azure Azure App Service
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://github.com/MikeWasson/BookService)

Jako ostatni krok opublikuje aplikację na platformie Azure. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **publikowania**.

![](part-10/_static/image1.png)

Kliknięcie przycisku **publikowania** wywołuje **publikowanie w sieci Web** okna dialogowego. Jeśli zaznaczono **Host w chmurze** po pierwszym tworzeniu projektu, a następnie połączenie i ustawienia są już skonfigurowane. W takim przypadku wystarczy kliknąć **ustawienia** i sprawdź &quot;wykonaj migracje Code First&quot;. (Jeśli nie została sprawdzona **Host w chmurze** na początku, a następnie wykonaj kroki opisane w [następnej sekcji](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Aby wdrożyć aplikację, kliknij przycisk **publikowania**. Możesz wyświetlić postęp publikowania w **aktywności publikowania w sieci Web** okna. (Z **widoku** menu, wybierz opcję **inne okna**, a następnie wybierz pozycję **aktywności publikowania w sieci Web**.)

![](part-10/_static/image4.png)

Po zakończeniu pracy programu Visual Studio, wdrażania aplikacji, przeglądarka domyślna automatycznie otwiera adres URL wdrożonej witryny sieci Web, a utworzona aplikacja działa w chmurze. Adres URL na pasku adresu przeglądarki pokazuje, że lokacja jest ładowany z Internetu.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Wdrożenie nowej witryny sieci Web

Jeśli nie zaznaczono **Host w chmurze** podczas tworzenia projektu, możesz teraz skonfigurować nową aplikację sieci web. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz **publikowania**. Wybierz **profilu** i kliknij polecenie **witryn sieci Web Microsoft Azure**. Jeśli użytkownik nie jest obecnie zalogowany na platformie Azure, pojawi się monit do logowania.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

W **istniejących witryn sieci Web** okna dialogowego, kliknij przycisk **nowy**.

![](part-10/_static/image9.png)

Wprowadź nazwę lokacji. Wybierz subskrypcję platformy Azure i region. W obszarze **serwera bazy danych**, wybierz pozycję **Utwórz nowy serwer**, lub wybierz istniejący serwer. Kliknij przycisk **Utwórz**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Kliknij przycisk **ustawienia** i sprawdź &quot;wykonaj migracje Code First&quot;. Następnie kliknij przycisk **publikowania**.

>[!div class="step-by-step"]
[Poprzednie](part-9.md)
