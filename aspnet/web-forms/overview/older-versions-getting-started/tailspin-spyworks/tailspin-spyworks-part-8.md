---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Część 8: Końcowe strony, obsługa wyjątków i zawarcia | Dokumentacja firmy Microsoft'
author: JoeStagner
description: W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 8 dodaje skontaktuj się z pomocą strony, strony i wyjątków — informacje...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 3b49ee53e82933de9b50960779c28ca6ab7441e5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757136"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>Część 8: Końcowe strony, obsługa wyjątków i zawarcia
====================
przez [Stagner Jan](https://github.com/JoeStagner)

> Tailspin Spyworks pokazuje, jak bardzo łatwo jest tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET. Przedstawia on poza sposób użycia wspaniałych nowych funkcjach w ASP.NET 4 do tworzenia sklep online, m.in. zakupy wyewidencjonowanie i Administracja.
> 
> W tej serii samouczków zawiera szczegóły wszystkich kroków kompilacji Przykładowa aplikacja Tailspin Spyworks. Część 8 dodaje kontaktu strony, strony i obsługa wyjątków — informacje. To jest zawarcie tej serii.


## <a id="_Toc260221680"></a>  Skontaktuj się z strony (wysyłanie wiadomości e-mail z platformy ASP.NET)

Utwórz nową stronę o nazwie ContactUs.aspx

Za pomocą projektanta, utwórz następującą postać biorąc ważne, aby uwzględnić ToolkitScriptManager i kontrolka edytora z AjaxdControlToolkit. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Kliknij dwukrotnie przycisk "Prześlij", aby wygenerować obsługi zdarzeń kliknięcia w kodzie pliku i zaimplementować metodę, aby wysyłać informacje kontaktowe jako wiadomości e-mail.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Ten kod wymaga, że plik web.config zawiera wpis w sekcji konfiguracji, który określa serwer SMTP używany do wysyłania wiadomości e-mail.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  Informacje o stronie

Utwórz stronę o nazwie AboutUs.aspx i niezależnie od zawartości, które chcesz dodać.

## <a id="_Toc260221682"></a>  Globalnego programu obsługi wyjątków

Na koniec w całej aplikacji firma Microsoft ma zgłaszane i nieprzewidzianych okoliczności oznacza zimnych także przyczyną braku obsługi wyjątków w naszej aplikacji sieci web.

Firma Microsoft nigdy nie ma nieobsługiwany wyjątek ma być wyświetlany obiekt odwiedzający witrynę sieci web.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Oprócz trwa panowanie komfortu nieobsługiwanych wyjątków może być również problem z zabezpieczeniami.

Aby rozwiązać ten problem wdrażamy globalnego programu obsługi wyjątków.

Aby to zrobić, otwórz plik Global.asax i zanotuj następującą obsługę zdarzeń wstępnie wygenerowane.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Dodaj kod, aby wdrożyć aplikację\_procedurę obsługi błędów w następujący sposób.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Następnie dodaj stronę o nazwie Error.aspx do rozwiązania i Dodaj następujący fragment kodu znaczników.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Teraz na stronie\_obciążenia wyodrębniania procedury obsługi zdarzeń, komunikaty o błędach z obiektu żądania.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  Podsumowanie

Zobaczyliśmy, że formularzy sieci Web platformy ASP.NET można łatwo utworzyć zaawansowane witryny sieci Web z dostępu do bazy danych, członkostwo w technologii AJAX, itp. bardzo szybko.

Miejmy nadzieję w tym samouczku przyznał Ci narzędzia, których potrzebujesz do rozpoczęcia tworzenia własnych WebForms ASP.NET aplikacji!

> [!div class="step-by-step"]
> [Poprzednie](tailspin-spyworks-part-7.md)
