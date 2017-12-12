---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalowanie pomocnika w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: "W tym artykule opisano sposób instalowania pomocnika w witrynie sieci Web platformy ASP.NET Web Pages (Razor). Pomocnik jest składnikiem wielokrotnego użytku, który zawiera kod i znaczników na..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 842c5a56d14314217c1e6ad6d48ded28d3cc5b4e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Instalowanie pomocnika w lokacji (Razor) stron sieci Web ASP.NET
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano sposób instalowania pomocnika w witrynie sieci Web platformy ASP.NET Web Pages (Razor). A *Pomocnika* jest składnikiem wielokrotnego użytku, który zawiera kod i znaczników do wykonania zadania, które mogą być niewygodny lub złożonych.
> 
> Zawartość:
> 
> - Jak zainstalować pomocnika w witrynie sieci Web utworzony za pomocą programu WebMatrix 3.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Program WebMatrix 3


## <a name="overview-of-helpers"></a>Omówienie wątków

Niektóre zadania, które osoby często mają być wykonane na stronach sieci web wymaga dużo kodu lub wymagają dodatkowych wiedzy. Przykłady obejmują wyświetlanie wykresu dla danych. umieszczanie przycisk "Obserwuj" Twitter na stronie; Wysyłanie wiadomości e-mail z witryny sieci Web; Przycinanie lub zmiana rozmiaru obrazów; przy użyciu usługi PayPal dla witryny. Aby ułatwić czy rodzaju rzeczy, ASP.NET Web Pages umożliwia używanie *pomocników*. Pomocnicy są składniki zainstalowanie dla witryny, które umożliwiają można wykonywać typowe zadania za pomocą tylko wiersz lub dwóch kodu Razor.

Strony ASP.NET Web Pages ma kilka wątków wbudowane. Wiele wątków są jednak dostępne w pakietach (dodatki), które znajdują się za pomocą Menedżera pakietów NuGet. NuGet pozwala wybrać pakiet do zainstalowania, a następnie dba o szczegóły instalacji.

## <a name="installing-a-helper-in-webmatrix-3"></a>Instalowanie pomocnika w programie WebMatrix 3

1. W programie WebMatrix 3 kliknij **NuGet** przycisku.

    ![Okno dialogowe galerii NuGet w programie WebMatrix](installing-helpers/_static/image1.png)
2. To spowoduje uruchomienie Menedżera pakietów NuGet i wyświetlenie dostępnych pakietów. W polu wyszukiwania wprowadź słowo kluczowe pomocnika, którą chcesz zainstalować.

    ![Okno dialogowe galerii NuGet w programie WebMatrix](installing-helpers/_static/image2.png)
- Wybierz pakiet, a następnie kliknij przycisk **zainstalować**. Kliknij przycisk **tak** po otrzymaniu monitu, jeśli chcesz zainstalować pakiet i zaakceptuj postanowienia.

    Jeśli po raz pierwszy po zainstalowaniu pomocnika NuGet powoduje tworzenie folderów w witryny sieci Web kod, który stanowi pomocnika.
- Aby odinstalować pomocnika, kliknij przycisk **galerii** , kliknij **zainstalowana** , a następnie wybierz pakiet, którego chcesz odinstalować.

## <a name="installing-the-twitter-helper"></a>Instalowanie Pomocnik usługi Twitter

Najnowszą wersję interfejsu API usługi Twitter nie jest zgodny z elementem pomocniczym Twitter, które będą instalowane za pośrednictwem pakietu NuGet. Zamiast tego zobacz [Pomocnik usługi Twitter, za pomocą programu WebMatrix](twitter-helper.md) tematu zawiera informacje dotyczące sposobu konfigurowania Pomocnik usługi Twitter w projekcie.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


[Introducing ASP.NET Web Pages 2 — podstawy programowania](../getting-started/introducing-razor-syntax-c.md)

[Pomocnik usługi Twitter, za pomocą programu WebMatrix](twitter-helper.md)
