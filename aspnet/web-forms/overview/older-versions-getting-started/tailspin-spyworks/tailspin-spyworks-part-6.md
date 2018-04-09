---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Część 6: Członkostwo ASP.NET | Dokumentacja firmy Microsoft'
author: JoeStagner
description: Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 6 dodaje członkostwa ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 83e9bc780ea8face3e0f55fdf8c00e13b60f80a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="part-6-aspnet-membership"></a>Część 6: Członkostwo ASP.NET
====================
przez [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks pokazano, jak bardzo proste jest tworzenie zaawansowanych, skalowalnych aplikacji dla platformy .NET. Przedstawia on poza jak nowe, fantastyczne funkcje programu ASP.NET 4 do tworzenia sklepu online, łącznie z zakupów, wyewidencjonowania i administracji.
> 
> Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 6 dodaje członkostwa ASP.NET.


## <a id="_Toc260221672"></a>  Praca z członkostwa ASP.NET

![](tailspin-spyworks-part-6/_static/image1.png)

Kliknij przycisk zabezpieczeń

![](tailspin-spyworks-part-6/_static/image1.jpg)

Upewnij się, że użyto uwierzytelniania formularzy.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Umożliwia utworzenie kilku użytkowników link "Tworzenie użytkownika".

![](tailspin-spyworks-part-6/_static/image3.jpg)

Na koniec można znaleźć w oknie Eksploratora rozwiązań i Odśwież widok.

![](tailspin-spyworks-part-6/_static/image2.png)

Należy pamiętać, że ASPNETDB. Utworzono MDF poprawnie. Ten plik zawiera tabele do obsługi usług platformy ASP.NET core, takich jak członkostwo.

Można teraz rozpocząć wdrażanie realizacji.

Rozpocznij od utworzenia strony CheckOut.aspx.

Na stronie CheckOut.aspx tylko powinny być dostępne dla użytkowników, którzy są zalogowani, firma Microsoft będzie ograniczanie dostępu do rejestrowane w przystawce Użytkownicy i Przekierowanie użytkowników, którzy nie jest zalogowany do strony logowania.

W tym celu dodamy następujących sekcji konfiguracji naszych pliku web.config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

Szablon aplikacji formularzy sieci Web programu ASP.NET dodano sekcję uwierzytelniania do naszej pliku web.config i automatycznie ustanowić domyślną stronę logowania.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Firma Microsoft należy zmodyfikować plik do migracji anonimowe koszyk, gdy użytkownik loguje się kodzie Login.aspx. Zmień stronę\_załadować zdarzeń w następujący sposób.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Następnie Dodaj program obsługi zdarzeń "LoggedIn" jak nazwa sesji do nowo zalogowanego użytkownika i zmień identyfikator sesji tymczasowe w koszyku do użytkownika, wywołując metodę MigrateCart w naszej klasy MyShoppingCart. (Zaimplementowana w pliku CS)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implementacja metody MigrateCart() podobny.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

W checkout.aspx użyjemy obiektu EntityDataSource i Element GridView w naszym wyewidencjonowanie strony podobnie NAS w naszej stronie koszyka.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Należy pamiętać, że nasze kontrolki widoku siatki Określa program obsługi zdarzeń "ondatabound" o nazwie MyList\_RowDataBound więc warto implementacji programu obsługi zdarzeń następująco.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Ten utrzymuje — metoda sumę zakupy koszyka każdy wiersz jest powiązany i aktualizuje dolnego wiersza widoku GridView.

Na tym etapie wdrożonych prezentacji "przeglądu" zlecenia do umieszczenia.

Załóżmy obsługi scenariusza pusty koszyka, dodając kilka wierszy kodu do strony\_zdarzeń obciążenia:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Gdy użytkownik kliknie przycisk "Zatwierdź" Firma Microsoft będzie wykonaj następujący kod do obsługi zdarzeń kliknij przycisk przesyłania.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

"Rodzaje" proces przesyłania zamówienia jest realizowane w metodzie SubmitOrder() klasy Nasze MyShoppingCart.

SubmitOrder następujące czynności:

- Wykonaj wszystkie pozycje w koszyku i używać ich do tworzenia nowego rekordu zlecenia i skojarzonych rekordów SzczegółyZamówień.
- Oblicz Data wysyłki.
- Wyczyść koszyk.


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Na potrzeby tej przykładowej aplikacji firma Microsoft będzie obliczać Data wysyłki po prostu dodając dwa dni do daty bieżącej.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Uruchamianie aplikacji teraz pozwolą firmie Microsoft w celu przetestowania zakupów procesu od początku do końca.

> [!div class="step-by-step"]
> [Poprzednie](tailspin-spyworks-part-5.md)
> [dalej](tailspin-spyworks-part-7.md)
