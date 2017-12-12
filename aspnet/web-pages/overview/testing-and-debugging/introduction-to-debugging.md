---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Wprowadzenie do debugowania ASP.NET Web Pages witryny (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: "Debugowanie jest procesem znajdowania i naprawiania błędów na swoich stronach kodu. W tym rozdziale przedstawiono niektóre narzędzia i techniki, które można używać do debugowania i analyz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: 2bc1f096540d17095ef760eed67b458fcd4e1372
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>Wprowadzenie do debugowania ASP.NET Web Pages witryny (Razor)
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule opisano różne sposoby debugowanie stron w witrynie sieci Web platformy ASP.NET Web Pages (Razor). Debugowanie jest procesem znajdowania i naprawiania błędów na swoich stronach kodu.
> 
> **Zawartość:** 
> 
> - Jak wyświetlić informacje pomocne w analizowanie i debugowanie stron.
> - Jak używać debugowania narzędzi w programie Visual Studio.
>   
> 
> Poniżej przedstawiono funkcje platformy ASP.NET, wprowadzone w artykule:
> 
> - `ServerInfo` Pomocnika.
> - `ObjectInfo`Pomocnik.
>   
> 
> ## <a name="software-versions"></a>Wersje oprogramowania
> 
> 
> - Strony sieci Web platformy ASP.NET (Razor) 3
> - Visual Studio 2013
>   
> 
> W tym samouczku współdziała również z programu ASP.NET Web Pages 2. Można używać 3 programu WebMatrix, ale zintegrowane debuger nie jest obsługiwane.


W pierwszej kolejności ich uniknięcie jest ważnym aspektem rozwiązywania problemów dotyczących błędów i problemów w kodzie. Możesz to zrobić przez umieszczenie części kodu, które mogą powodować błędy w `try/catch` bloków. Aby uzyskać więcej informacji, zobacz sekcję dotyczącą obsługi błędów w [wprowadzenie do platformy ASP.NET Web programowania przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890).

`ServerInfo` Pomocnika jest narzędziem diagnostycznym, który zawiera przegląd informacji o środowisku serwera sieci web obsługującym ze strony. Pokazano także, informacje o żądaniu HTTP, który jest wysyłany, gdy przeglądarka żąda strony. `ServerInfo` Pomocnik wyświetli Bieżąca tożsamość użytkownika, typ przeglądarki, który zgłosił żądanie, i tak dalej. Tego rodzaju informacje mogą pomóc w rozwiązywania typowych problemów.

1. Tworzenie nowej strony sieci web o nazwie *ServerInfo.cshtml*.
2. Na końcu strony tuż przed zamknięciem `</body>` tagów, Dodaj `@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    Możesz dodać `ServerInfo` kodu w dowolnym miejscu strony. Zachowując dodanie go przed upływem będą dane wyjściowe oddzielona od innych treści strony, co ułatwia odczytu.

    > [!NOTE] 
    > 
    > **Ważne** należy usunąć wszelkie kod diagnostyczny ze stron sieci web przed przeniesieniem stron sieci web na serwerze produkcyjnym. Dotyczy to `ServerInfo` pomocnika, jak również inne techniki diagnostyczne w tym artykule obejmujących dodawanie kodu do strony. Ponieważ tego rodzaju informacje mogą być przydatne do osób ze złośliwymi działaniami, które nie mają odwiedzających witrynę sieci Web, aby wyświetlić informacje o Twojej nazwy serwera, nazwy użytkowników, ścieżki na serwerze i podobne szczegóły.
3. Strony i uruchom go w przeglądarce.

    ![Debugowanie 1](introduction-to-debugging/_static/image1.jpg)

    `ServerInfo` Pomocnik wyświetli cztery tabel zawierających informacje na stronie:

    - Konfiguracja serwera. Ta sekcja zawiera informacje dotyczące hostingu serwera sieci web, w tym nazwę komputera, wersja ASP.NET używasz, nazwę domeny i czasu serwera.
    - Zmienne serwera programu ASP.NET. Ta sekcja zawiera szczegółowe informacje o wielu szczegóły protokołu HTTP (zwane zmienne HTTP) i wartości, które są częścią każdym żądaniu strony sieci web.
    - Informacje środowiska wykonawczego protokołu HTTP. Ta sekcja zawiera szczegółowe informacje o tym, że wersji programu Microsoft .NET Framework strony sieci web działa w ramach, ścieżka, szczegółowe informacje o pamięci podręcznej i tak dalej. (W wyniku uczenia się w [wprowadzenie do platformy ASP.NET Web programowania przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkId=202890), przy użyciu składni jest oparty na technologii serwera sieci web ASP.NET firmy Microsoft, która jest oparta na szeroką gamę oprogramowania elementu Razor strony sieci Web ASP.NET Biblioteka programowanie nazywana programu .NET Framework).
    - Zmienne środowiskowe. Ta sekcja zawiera listę wszystkich zmiennych środowiska lokalnego i ich wartości, na serwerze sieci web.

    Pełny opis wszystkie dane serwera i żądania wykracza poza zakres tego artykułu, ale można stwierdzić, że `ServerInfo` pomocnika zwraca wiele informacji diagnostycznych. Aby uzyskać więcej informacji o wartościach który `ServerInfo` zwraca, zobacz [rozpoznany zmiennych środowiskowych](https://technet.microsoft.com/en-us/library/dd560744(WS.10).aspx) w witrynie Microsoft TechNet w sieci Web i [zmienne serwera usług IIS](https://msdn.microsoft.com/en-us/library/ms524602(VS.90).aspx) w witrynie MSDN.

## <a name="embedding-output-expressions-to-display-page-values"></a>Osadzanie wyrażeń dane wyjściowe do wyświetlania wartości strony

Inny sposób, aby zobaczyć, co dzieje się w kodzie jest osadzanie wyrażeń w danych wyjściowych strony. Wiesz, można bezpośrednio output wartość zmiennej, dodając wyglądać mniej więcej tak `@myVariable` lub `@(subTotal * 12)` do strony. Dla celów debugowania można umieścić wyrażenia te dane wyjściowe w punktach strategicznych w kodzie. Dzięki temu można zobaczyć wartości zmiennych klucza lub wyników obliczeń, po uruchomieniu strony. Po zakończeniu debugowania, można usunąć wyrażenia lub komentarz je. W tej procedurze przedstawiono typowym sposobem na potrzeby debugowania strona wyrażenia osadzone.

1. Utwórz nową stronę programu WebMatrix, o nazwie *OutputExpression.cshtml*.
2. Zastąp strony zawartości z następujących czynności:

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    W przykładzie użyto `switch` instrukcji, aby sprawdzić wartość `weekday` zmiennej, a następnie wyświetlania komunikatu różne wyniki, w zależności od tego, który dzień tygodnia jest. W tym przykładzie `if` bloku w pierwszym bloku kodu zmienia arbitralnie dzień tygodnia, dodając jeden dzień na wartość bieżącego dnia tygodnia. Jest to błąd wprowadzone w celach ilustracyjnych.
3. Strony i uruchom go w przeglądarce.

    Strona wyświetla komunikat nieprawidłowy dzień tygodnia. Niezależnie od dnia tygodnia faktycznie jest, zobaczysz komunikat później przez jeden dzień. Mimo że w takim przypadku wiadomo, dlaczego wiadomość jest wyłączone (ponieważ kod celowo ustawia wartość dnia niepoprawne), w rzeczywistości często jest trudne do wiedzieć, gdzie rzeczy są nieprawidłowe w kodzie. Aby debugować, należy dowiedzieć się, co dzieje się do wartości zmiennych i obiektów kluczy takich jak `weekday`.
4. Dodawanie danych wyjściowych wyrażenia wstawiając `@weekday` jak pokazano w dwóch miejscach wskazywanym przez komentarze w kodzie. Wyrażenia te dane wyjściowe będą wyświetlane wartości zmiennej w tym momencie na wykonanie kodu.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. Zapisz i uruchom strony w przeglądarce.

    Zostaje wyświetlona strona rzeczywistych dzień tygodnia najpierw, a następnie zaktualizowane dzień tygodnia powoduje dodawanie jeden dzień, a następnie komunikat wynikowy od `switch` instrukcji. Dane wyjściowe z dwóch wyrażeń zmiennej (`@weekday`) ma bez spacji między dni, ponieważ nie zostały dodane kodu HTML `<p>` znaczniki, aby dane wyjściowe; wyrażenia znajdują się tylko do testowania.

    ![Debugowanie 2](introduction-to-debugging/_static/image2.jpg)

    Spowoduje to wyświetlenie w przypadku błędu. Po pierwszym wyświetleniu `weekday` zmiennych w kodzie, zawiera poprawny dzień. Po wyświetleniu go za drugim razem po `if` bloków w kodzie, dzień jest wyłączony, przez co. Aby wiedzieć, czy jest coś się stało między wygląd pierwszej i drugiej zmiennej dzień tygodnia. Jeżeli to rzeczywiste usterki, tego rodzaju podejście pomóc zawęzić lokalizacji kodu, który jest przyczyną problemu.
6. Usuń kod na stronie Usuwanie wyrażenia dwóch danych wyjściowych, dodane i usuwając kod, który zmienia dzień tygodnia. Pełną, pozostałe bloku kodu wygląda następująco:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. Uruchom strony w przeglądarce. Teraz, zostanie wyświetlony komunikat poprawne wyświetlane rzeczywiste dzień tygodnia.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>Przy użyciu Pomocnika ObjectInfo do wyświetlania wartości obiektu

`ObjectInfo` Pomocnika Wyświetla typ i wartość każdego obiektu przekazywania do niej. Służy do wyświetlania wartości zmiennych i obiektów w kodzie (tak samo jak za pomocą wyrażeń dane wyjściowe w poprzednim przykładzie), a także widoczny typ informacji o obiekcie danych.

1. Otwórz plik o nazwie *OutputExpression.cshtml* utworzony wcześniej.
2. Zastąp cały kod na stronie następujący blok kodu:

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. Zapisz i uruchom strony w przeglądarce.

    ![Debugowanie 4](introduction-to-debugging/_static/image3.jpg)

    W tym przykładzie `ObjectInfo` Pomocnik wyświetli dwóch elementów:

    - Typ. Pierwszej zmiennej jest typ `DayOfWeek`. Druga zmienna jest typ `String`.
    - Wartość. W takim przypadku, ponieważ wartość zmiennej pozdrowienia jest już wyświetlane na stronie, wartość jest wyświetlana ponownie, gdy zmienna do `ObjectInfo`.

    W przypadku bardziej złożonych obiektów `ObjectInfo` pomocnika można wyświetlić więcej informacji &#8212; zasadniczo, może on zawierać typy i wartości wszystkich właściwości obiektu.

## <a name="using-debugging-tools-in-visual-studio"></a>Za pomocą narzędzi debugowania w programie Visual Studio

Bardziej zaawansowane środowisko debugowania, użyj programu Visual Studio 2013 lub wolnych [programu Visual Studio Express 2013 for Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express). Z programem Visual Studio można ustawić punktu przerwania w kodzie w wierszu, który chcesz sprawdzić.

![Ustaw punkt przerwania](introduction-to-debugging/_static/image1.png)

Podczas testowania witryny sieci web na punkt przerwania zostaje zatrzymana wykonywanie kodu.

![osiągnąć punktu przerwania](introduction-to-debugging/_static/image2.png)

Można sprawdzić bieżące wartości zmiennych i kroków kodu wiersz po wierszu.

![Zobacz wartości](introduction-to-debugging/_static/image3.png)

Aby dowiedzieć się, jak za pomocą zintegrowanego debugera programu Visual Studio debugowanie stron ASP.NET Razor, zobacz [programowania stron sieci Web platformy ASP.NET (Razor) przy użyciu programu Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Programowanie stron sieci Web platformy ASP.NET (Razor) za pomocą programu Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)
- [Zmienne serwera usług IIS](https://msdn.microsoft.com/en-us/library/ms524602(VS.90).aspx) (MSDN)
- [Rozpoznany zmiennych środowiskowych](https://technet.microsoft.com/en-us/library/dd560744(WS.10).aspx) (TechNet)
