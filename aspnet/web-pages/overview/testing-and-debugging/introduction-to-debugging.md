---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Wprowadzenie do debugowania ASP.NET Web Pages (Razor) witryn | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Debugowanie jest proces znajdowaniem i naprawianiem błędów na swoich stronach kodu. W tym rozdziale przedstawiono niektóre narzędzia i techniki, można użyć do debugowania i analyz...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: e5302492f01cbd507e0b0fd995f21621bf6f04c8
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021316"
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>Wprowadzenie do debugowania ASP.NET Web Pages (Razor) witryn
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano różne sposoby debugowania stron w witrynie internetowej ASP.NET Web Pages (Razor). Debugowanie jest proces znajdowaniem i naprawianiem błędów na swoich stronach kodu.
>
> **Zawartość:**
>
> - Jak wyświetlić informacje, które ułatwia analizowanie i debugowanie stron.
> - Jak używać debugowania narzędzi w programie Visual Studio.
>
>
> Poniżej przedstawiono funkcje platformy ASP.NET, wprowadzona w artykule:
>
> - `ServerInfo` Pomocnika.
> - `ObjectInfo` Pomocnik.
>
>
> ## <a name="software-versions"></a>Wersje oprogramowania
>
>
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
>
>
> W tym samouczku współpracuje również z wzorca ASP.NET Web Pages 2. Można użyć program WebMatrix 3, ale zintegrowany debuger nie jest obsługiwane.


Jest istotnym elementem Rozwiązywanie problemów z błędami i problemów w kodzie w pierwszej kolejności ich unikać. Możesz to zrobić poprzez umieszczenie sekcji kodu, które mogą spowodować błędy w `try/catch` bloków. Aby uzyskać więcej informacji, zobacz sekcję na obsługi błędów w [wprowadzenie do platformy ASP.NET sieci Web programowania z użyciem składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890).

`ServerInfo` Pomocnika jest narzędziem diagnostycznym, który zawiera przegląd informacji o środowisku serwera sieci web hostującego strony sieci. Pokazano także informacje o żądaniu HTTP, która jest wysyłana, gdy przeglądarka żąda strony. `ServerInfo` Pomocnik wyświetli bieżącej tożsamości użytkownika, typ przeglądarki, który zgłosił żądanie, i tak dalej. Tego rodzaju informacje mogą pomóc rozwiązać typowe problemy.

1. Utwórz nową stronę sieci web o nazwie *ServerInfo.cshtml*.
2. Na końcu strony, tuż przed zamykającym `</body>` tagów, należy dodać `@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    Możesz dodać `ServerInfo` kod w dowolnym miejscu strony. Ale dodanie go na końcu będą przechowywane oddzielnie od innych treści strony, dzięki czemu łatwiej odczytać dane wyjściowe.

    > [!NOTE]
    >
    > **Ważne** przed przeniesieniem stron sieci web na serwerze produkcyjnym, należy usunąć wszelkie kod diagnostyczny ze stron sieci web. Dotyczy to `ServerInfo` pomocnika, a także inne techniki diagnostyczne w tym artykule obejmujące, dodając kod do strony. Ponieważ informacje tego typu mogą być przydatne dla osób z wadami złośliwego działania, które nie mają odwiedzających witrynę sieci Web, aby wyświetlić informacje o Twojej nazwy serwera, nazwy użytkowników, ścieżki na serwerze i podobne szczegóły.
3. Zapisz stronę i uruchom go w przeglądarce.

    ![Debugging-1](introduction-to-debugging/_static/image1.jpg)

    `ServerInfo` Pomocnik wyświetli cztery tabele informacje na stronie:

   - Konfiguracja serwera. Ta sekcja zawiera informacje o hostingu serwera sieci web, w tym nazwę komputera, wersja korzystasz z platformy ASP.NET, nazwa domeny i czas serwera.
   - Zmienne serwera programu ASP.NET. Ta sekcja zawiera szczegółowe informacje o wiele szczegółów protokołu HTTP (o nazwie zmienne HTTP) i wartości, które są dostępne w ramach każdego żądania strony sieci web.
   - Informacje o środowisko wykonawcze protokołu HTTP. Ta sekcja zawiera szczegółowe informacje o tym, która wersja programu Microsoft .NET Framework działa strony sieci web, ścieżka, szczegółowe informacje o pamięci podręcznej i tak dalej. (Jak przedstawiono w [wprowadzenie do platformy ASP.NET sieci Web programowania z użyciem składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890), za pomocą Razor składni są tworzone przy użyciu technologii serwera sieci web platformy ASP.NET firmy Microsoft, która sama rozbudowane oprogramowania w oparciu stron ASP.NET Web Pages Tworzenie biblioteki o nazwie .NET Framework.)
   - Zmienne środowiskowe. Ta sekcja zawiera listę wszystkich zmiennych środowiskowych lokalnych i ich wartości, na serwerze sieci web.

     Pełny opis wszystkich serwera i żądania informacje wykracza poza zakres tego artykułu, ale możesz zobaczyć, że `ServerInfo` pomocnika zwraca dużą ilość informacji diagnostycznych. Aby uzyskać więcej informacji o wartościach, `ServerInfo` zwraca, zobacz [rozpoznawane zmienne środowiskowe](https://technet.microsoft.com/library/dd560744(WS.10).aspx) w witrynie Microsoft TechNet i [zmienne serwera usług IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) w witrynie MSDN.

## <a name="embedding-output-expressions-to-display-page-values"></a>Osadzanie wyrażeń dane wyjściowe do wyświetlania wartości strony

Innym sposobem, aby zobaczyć, co się dzieje w kodzie jest osadzanie wyrażeń danych wyjściowych strony. Jak już wiadomo, można bezpośrednio danych wyjściowych wartość zmiennej przez dodanie podobny `@myVariable` lub `@(subTotal * 12)` ze stroną. Do debugowania, można umieścić wyrażenia te dane wyjściowe w strategiczny punktach w kodzie. Dzięki temu można sprawdzić wartości zmiennych klucza lub wynikiem obliczenia, po uruchomieniu strony. Po zakończeniu debugowania, możesz usunąć wyrażeń lub komentarz je automatycznie. Ta procedura przedstawia typowy sposób debugować strony przy użyciu wyrażenia osadzone.

1. Tworzenie nowej strony programu WebMatrix, który nosi nazwę *OutputExpression.cshtml*.
2. Zastąp zawartości przy użyciu następujących stron:

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    W przykładzie użyto `switch` instrukcję, aby sprawdzić wartość `weekday` zmiennych, a następnie wyświetlana jest komunikat różne wyniki w zależności od dzień tygodnia. W tym przykładzie `if` bloku, w pierwszym bloku kodu zmienia arbitralnie dzień tygodnia, dodając jeden dzień na wartość bieżący dzień tygodnia. Jest to błąd, który został wprowadzony w celach ilustracyjnych.
3. Zapisz stronę i uruchom go w przeglądarce.

    Strony wyświetli komunikat o niewłaściwej dzień tygodnia. Niezależnie od dnia tygodnia faktycznie jest, zostanie wyświetlony komunikat później przez jeden dzień. Mimo że w tym przypadku wiesz, dlaczego wiadomości jest wyłączona (ponieważ celowo ustawiana wartość niepoprawne dnia), w rzeczywistości jest często wiadomo, gdzie przebieg procesu problem w kodzie. Aby debugować, musisz dowiedzieć się, co dzieje się do wartości zmiennych i obiektów kluczy takich jak `weekday`.
4. Dodaj dane wyjściowe wyrażeń, wstawiając `@weekday` pokazany w dwóch miejscach wskazywanym przez komentarze w kodzie. Wyrażenia te dane wyjściowe będą wyświetlane wartości zmiennej w tym momencie podczas wykonywania kodu.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. Zapisz i Uruchom stronę w przeglądarce.

    Zostanie wyświetlona strona rzeczywistych dzień tygodnia, najpierw, a następnie zaktualizowane dzień tygodnia, który wynikiem dodawania jeden dzień, a następnie Wynikowy komunikat z `switch` instrukcji. Dane wyjściowe z dwóch wyrażeń zmiennych (`@weekday`) nie zawiera spacji między dni, ponieważ nie został dodany kod HTML `<p>` tagów, aby dane wyjściowe; wyrażenia znajdują się tylko do testowania.

    ![Debugging-2](introduction-to-debugging/_static/image2.jpg)

    Teraz możesz zobaczyć, gdzie znajduje się błąd. Kiedy najpierw wyświetlamy `weekday` zmiennej w kodzie, pokazuje poprawnym dniem. Po wyświetleniu go za drugim razem po `if` blokuje w kodzie, dzień jest wyłączona przez jeden. Aby było wiadomo, czy ma zdarzyło się coś między pierwszym i drugim wygląd zmiennej dzień tygodnia. Gdyby to prawdziwej usterki, tego rodzaju podejście pomóc zawęzić lokalizacji kodu, który jest przyczyną problemu.
6. Napraw kod na stronie, usuwając wyrażeń dwóch danych wyjściowych, które dodano i usuwanie kod, który zmienia dzień tygodnia. Pełną, pozostałe bloku kodu wygląda następująco:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. Uruchom stronę w przeglądarce. Tym razem, zostanie wyświetlony komunikat poprawne wyświetlany dla rzeczywistego dzień tygodnia.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>Aby wyświetlić wartości obiektu przy użyciu Pomocnika ObjectInfo

`ObjectInfo` Pomocnika Wyświetla typ i wartość każdego obiektu przekazywania do niej. Służy do wyświetlania wartości zmiennych i obiektów w kodzie (tak samo jak przy użyciu wyrażeń dane wyjściowe w poprzednim przykładzie), a także możesz zobaczyć dane typu informacji o obiekcie.

1. Otwórz plik o nazwie *OutputExpression.cshtml* utworzonego wcześniej.
2. Zastąp cały kod na stronie następujący blok kodu:

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. Zapisz i Uruchom stronę w przeglądarce.

    ![Debugging-4](introduction-to-debugging/_static/image3.jpg)

    W tym przykładzie `ObjectInfo` Pomocnik wyświetli dwa elementy:

   - Typ. Dla zmiennej pierwszy typ jest `DayOfWeek`. Druga zmienna jest typ `String`.
   - Wartość. W tym przypadku, ponieważ wartość zmiennej powitanie jest już wyświetlane na stronie, wartość jest wyświetlana ponownie gdy możesz przekazać zmienną do `ObjectInfo`.

     W przypadku bardziej złożonych obiektów `ObjectInfo` pomocnika można wyświetlić więcej informacji &#8212; zasadniczo można wyświetlać, typy i wartości wszystkich właściwości tego obiektu.

## <a name="using-debugging-tools-in-visual-studio"></a>Za pomocą narzędzia do debugowania w programie Visual Studio

W przypadku bardziej kompleksowe środowisko debugowania, użyj [programu Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Za pomocą programu Visual Studio można ustawić punkt przerwania w kodzie, w wierszu, który ma zostać sprawdzony.

![Ustaw punkt przerwania](introduction-to-debugging/_static/image1.png)

Podczas testowania witryny sieci web, wykonywanie kodu zostanie zatrzymany w punkcie przerwania.

![osiągnięcia punktu przerwania](introduction-to-debugging/_static/image2.png)

Można sprawdzić bieżące wartości zmiennych i krok po kroku kodu wiersz po wierszu.

![Zobacz wartości](introduction-to-debugging/_static/image3.png)

Aby uzyskać informacje o użyciu zintegrowany debuger programu Visual Studio debugowanie stron Razor programu ASP.NET, zobacz [programowania stron ASP.NET Web Pages (Razor) przy użyciu programu Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Programowanie wzorca ASP.NET Web Pages (Razor) przy użyciu programu Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)
- [Zmienne serwera usług IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)
- [Rozpoznane zmienne środowiskowe](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)
