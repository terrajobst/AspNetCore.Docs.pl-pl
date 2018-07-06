---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Część 6: Członkostwo ASP.NET | Dokumentacja firmy Microsoft'
author: JoeStagner
description: W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 6 dodaje członkostwa ASP.NET.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 303c1edf548db1da9ef61d94bdd8157d6afbb2e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804423"
---
<a name="part-6-aspnet-membership"></a>Część 6: Członkostwo ASP.NET
====================
przez [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks pokazuje, jak bardzo łatwo jest tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET. Przedstawia on poza sposób użycia wspaniałych nowych funkcjach w ASP.NET 4 do tworzenia sklep online, m.in. zakupy wyewidencjonowanie i Administracja.
> 
> W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 6 dodaje członkostwa ASP.NET.


## <a id="_Toc260221672"></a>  Praca z członkostwa ASP.NET

![](tailspin-spyworks-part-6/_static/image1.png)

Kliknij pozycję Zabezpieczenia

![](tailspin-spyworks-part-6/_static/image1.jpg)

Upewnij się, że używasz uwierzytelniania formularzy.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Użyj linku "Utwórz użytkownika", aby utworzyć kilka użytkowników.

![](tailspin-spyworks-part-6/_static/image3.jpg)

Gdy skończysz, można znaleźć w oknie Eksploratora rozwiązań, a następnie Odśwież widok.

![](tailspin-spyworks-part-6/_static/image2.png)

Należy pamiętać, że ASPNETDB. Utworzono MDF dobrym rozwiązaniem. Ten plik zawiera tabele do obsługi usług ASP.NET core, takich jak członkostwo.

Można teraz rozpocząć implementowanie rozpoczęcie procesu realizowania zamówienia.

Rozpocznij od utworzenia strony CheckOut.aspx.

Na stronie CheckOut.aspx powinny być dostępne jedynie dla użytkowników, którzy są zalogowani, dzięki czemu będziemy ograniczać przydziału obowiązków dostęp do rejestrowane w przystawce Użytkownicy i przekierowuje użytkowników, którzy nie jest zalogowany do strony logowania.

W tym celu dodamy następujących sekcji konfiguracji naszego pliku web.config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

Szablon dla aplikacji formularzy sieci Web programu ASP.NET dodano sekcję uwierzytelniania do naszego pliku web.config i automatycznie nawiązane domyślną stronę logowania.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Firma Microsoft należy zmodyfikować kod Login.aspx związany z pliku do migracji anonimowe koszyka, jeśli użytkownik loguje. Zmiany strony\_załadować zdarzeń w następujący sposób.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Następnie Dodaj program obsługi zdarzeń "LoggedIn" następująco ustawiona nazwa sesji na nowo zalogowanego użytkownika i zmienić identyfikator tymczasowych sesji w koszyku do tego użytkownika, wywołując metodę MigrateCart w klasie naszych MyShoppingCart. (Zaimplementowano w pliku .cs)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implementacja metody MigrateCart() następująco.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

W checkout.aspx użyjemy EntityDataSource i GridView w naszym wyewidencjonowanie strony, na ile My mieliśmy w naszej stronie koszyka.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Należy pamiętać, że nasze kontrolki GridView określa procedurę obsługi zdarzeń "ondatabound" o nazwie MyList\_RowDataBound więc wykonania tego programu obsługi zdarzeń, takich jak to.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

To Suma zakupów koszyka każdy wiersz jest powiązany i aktualizuje dolny wiersz GridView przechowuje metody.

Na tym etapie zaimplementowano rozwiązania tlp prezentacji "Recenzja" zlecenia do umieszczenia.

Umożliwia obsługę scenariusza pusty koszyka, dodając kilka wierszy kodu do strony\_załadowane zdarzenie:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Gdy użytkownik kliknie przycisk "Zatwierdź" Firma Microsoft będzie wykonaj następujący kod w obsłudze zdarzeń kliknij przycisk Prześlij.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

"Rodzaje" proces przesyłania zamówienia jest realizowane w metodzie SubmitOrder() klasy Nasze MyShoppingCart.

SubmitOrder wykonują następujące czynności:

- Wykonaj wszystkie pozycje w koszyku i używać ich do tworzenia nowego rekordu zlecenia i skojarzonych rekordów OrderDetails.
- Oblicz Data wysyłki.
- Wyczyść koszyk sklepowy.


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Na potrzeby tej przykładowej aplikacji będzie obliczania Data wysyłki, po prostu dodając dwa dni data bieżąca.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Uruchomiona jest aplikacja teraz pozwolą nam Przetestuj proces zakupów od początku do końca.

> [!div class="step-by-step"]
> [Poprzednie](tailspin-spyworks-part-5.md)
> [dalej](tailspin-spyworks-part-7.md)
