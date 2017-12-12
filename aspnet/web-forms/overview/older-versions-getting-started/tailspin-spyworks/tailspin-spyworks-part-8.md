---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: "Część 8: Końcowe strony, obsługa wyjątków i zawarcia | Dokumentacja firmy Microsoft"
author: JoeStagner
description: "Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 8 dodaje kontaktu strony, strony i wyjątków — informacje..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 0dd1717ff1051f18a78fe77402c7603008b9b486
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>Część 8: Końcowe strony, obsługa wyjątków i zawarcia
====================
przez [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks pokazano, jak bardzo proste jest tworzenie zaawansowanych, skalowalnych aplikacji dla platformy .NET. Przedstawia on poza jak nowe, fantastyczne funkcje programu ASP.NET 4 do tworzenia sklepu online, łącznie z zakupów, wyewidencjonowania i administracji.
> 
> Ta seria samouczek zawiera szczegóły dotyczące wszystkich kroków kompilacji Tailspin Spyworks przykładowej aplikacji. Część 8 dodaje kontaktu strony, strony i obsługa wyjątków — informacje. Jest to zawarcia serii.


## <a id="_Toc260221680"></a>Skontaktuj się z strony (wysyłanie wiadomości e-mail z platformy ASP.NET)

Utwórz nową stronę o nazwie ContactUs.aspx

Przy użyciu narzędzia Projektant, utwórz następującą postać uwzględnieniu specjalne ToolkitScriptManager i kontrolce edytora z AjaxdControlToolkit. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Kliknij dwukrotnie przycisk "Zatwierdź" Generowanie obsługi zdarzeń kliknięcia w kodzie pliku i zaimplementuj metodę można wysłać informacji kontaktowych jako wiadomości e-mail.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Ten kod wymaga, aby plik web.config zawiera wpis w sekcji konfiguracji, który określa serwer SMTP używany do wysyłania poczty.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>Strona — informacje

Utwórz stronę o nazwie AboutUs.aspx i niezależnie od zawartości, które chcesz dodać.

## <a id="_Toc260221682"></a>Globalnego programu obsługi wyjątków

Ponadto w całej aplikacji ma możemy zgłaszane wyjątki i brak nieprzewidzianych okoliczności to zimnych również Przyczyna nieobsługiwanych wyjątków w naszej aplikacji sieci web.

Firma Microsoft nigdy nie mają nieobsługiwany wyjątek ma być wyświetlony dla obiekt odwiedzający witrynę sieci web.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Oprócz trwa olbrzymich użytkowników nieobsługiwanych wyjątków może być również problem z zabezpieczeniami.

Aby rozwiązać ten problem wprowadzimy globalnego programu obsługi wyjątków.

Aby to zrobić, otwórz plik Global.asax i zanotuj następujące programu obsługi zdarzeń wstępnie wygenerowane.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Dodaj kod, aby wdrożyć aplikację\_program obsługi błędów w następujący sposób.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Następnie dodaj stronę o nazwie Error.aspx do rozwiązania i Dodaj następujący fragment kodu znaczników.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Teraz na stronie\_załadować wyodrębniania programu obsługi zdarzeń komunikaty o błędach z obiektu żądanie.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>Zawierania

Firma Microsoft w tym samouczku czy tego ASP.NET WebForms można łatwo utworzyć zaawansowane witryny sieci Web z dostępem do bazy danych, członkostwa, AJAX, itp. bardzo szybko.

Miejmy nadzieję, że w tym samouczku przyznał Ci narzędzia, które należy rozpocząć tworzenie własnych formularzy sieci Web ASP.NET aplikacji!

>[!div class="step-by-step"]
[Poprzednie](tailspin-spyworks-part-7.md)
