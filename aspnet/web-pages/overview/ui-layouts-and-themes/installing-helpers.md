---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalowanie pomocnika we wzorcu ASP.NET Web Pages (Razor) lokacji | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule opisano sposób instalowania pomocnika w witrynie internetowej ASP.NET Web Pages (Razor). Pomocnik jest komponentów wielokrotnego użytku, obejmującą kodu i znaczników w celu na...
ms.author: aspnetcontent
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: db3dff9f2d70577bb0618335c0100b9899e87727
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838617"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Instalowanie pomocnika w witrynie ASP.NET Web Pages (Razor)
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano sposób instalowania pomocnika w witrynie internetowej ASP.NET Web Pages (Razor). A *Pomocnika* to składnik wielokrotnego użytku, który zawiera kod i znaczników w celu wykonania zadania, które mogą być uciążliwe lub złożonych.
> 
> Zawartość:
> 
> - Jak zainstalować pomocnika w witrynie sieci Web utworzonych za pomocą programu WebMatrix 3.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - Program WebMatrix 3


## <a name="overview-of-helpers"></a>Omówienie wątków

Niektóre zadania, ludzie często chcemy na stronach sieci web, które wymagają dużej ilości kodu lub dodatkowe wiedzy. Przykłady obejmują wyświetlanie wykresu dla danych. umieszczanie przycisk "Obserwuj" Twitter na stronie; Wysyłanie wiadomości e-mail z witryny sieci Web; Przycinanie i zmienianie rozmiaru obrazów; Korzystając z systemu PayPal, dla danej witryny. Aby ułatwić wykonaj te rodzaje elementów, ASP.NET Web Pages umożliwia korzystanie z *pomocników*. Pomocnicy są składniki instalowaną lokacją i umożliwiające możesz wykonywać typowe zadania przy użyciu tylko wiersz lub dwóch od kodu Razor.

Strony ASP.NET Web Pages ma kilka pomocników wbudowane. Wiele wątków, są jednak dostępne w pakietach (dodatki), które są dostarczane przy użyciu Menedżera pakietów NuGet. NuGet pozwala wybrać pakiet do zainstalowania, a następnie ta odpowiada za wszystkie szczegóły instalacji.

## <a name="installing-a-helper-in-webmatrix-3"></a>Instalowanie pomocnika w programie WebMatrix 3

1. Program WebMatrix 3 kliknij **NuGet** przycisku.

    ![Okno dialogowe galerii pakietów NuGet w programie WebMatrix](installing-helpers/_static/image1.png)
2. To spowoduje uruchomienie Menedżera pakietów NuGet i wyświetla dostępnych pakietów. W polu wyszukiwania wprowadź słowo kluczowe pomocy, które chcesz zainstalować.

    ![Okno dialogowe galerii pakietów NuGet w programie WebMatrix](installing-helpers/_static/image2.png)
3. Wybierz pakiet, a następnie kliknij przycisk **zainstalować**. Kliknij przycisk **tak** po wyświetleniu monitu, jeśli chcesz zainstalować pakiet i wskazują, że akceptujesz jej warunki.

     Jeśli po raz pierwszy po zainstalowaniu pomocnika NuGet powoduje tworzenie folderów w swojej witrynie sieci Web dla kodu, który tworzy pomocnika.
4. Aby odinstalować pomocnika, kliknij przycisk **galerii** przycisku, kliknij przycisk **zainstalowane** kartę, a następnie wybierz pakiet, który chcesz odinstalować.

## <a name="installing-the-twitter-helper"></a>Instalowanie pomocnika usługi Twitter

Najnowszą wersję interfejsu API usługi Twitter nie jest zgodny z elementem pomocniczym Twitter, które można zainstalować za pośrednictwem pakietu NuGet. Zamiast tego zobacz [Pomocnik usługi Twitter za pomocą programu WebMatrix](twitter-helper.md) tematu zawiera informacje dotyczące sposobu konfigurowania Pomocnik usługi Twitter w projekcie.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


[Przedstawiamy ASP.NET Web Pages 2 — podstawy programowania](../getting-started/introducing-razor-syntax-c.md)

[Pomocnik usługi Twitter za pomocą programu WebMatrix](twitter-helper.md)
