---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: Dane wyjściowe poprawa wydajności dzięki pamięci podręcznej (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: W tym samouczku dowiesz się, jak można znacznie zwiększyć wydajność aplikacji sieci web platformy ASP.NET MVC, korzystając z buforowania danych wyjściowych. Możesz...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 516c370941b8f7e5f3528953491057973679586d
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755931"
---
<a name="improving-performance-with-output-caching-c"></a>Poprawa wydajności dzięki buforowaniu (C#) danych wyjściowych
====================
przez [firmy Microsoft](https://github.com/microsoft)

> W tym samouczku dowiesz się, jak można znacznie zwiększyć wydajność aplikacji sieci web platformy ASP.NET MVC, korzystając z buforowania danych wyjściowych. Dowiesz się, jak wyniki zwrócone z akcji kontrolera, tak aby taką samą zawartość nie muszą zostać utworzone czasu każdy nowy użytkownik wywołuje akcję w pamięci podręcznej.


Celem tego samouczka jest wyjaśniają, jak można znacznie zwiększyć wydajność aplikacji ASP.NET MVC, wykorzystując wyjściowej pamięci podręcznej. Wyjściowej pamięci podręcznej umożliwia buforowanie zawartości zwróconej przez akcję kontrolera. W ten sposób tej samej zawartości nie trzeba wygenerować każdym razem, gdy jest wywoływana w tej samej akcji kontrolera.

Wyobraź sobie, na przykład, że aplikacji ASP.NET MVC, wyświetla listę rekordów bazy danych w widoku o nazwie indeksu. Zwykle czas każdy użytkownik wywołuje akcji kontrolera, które zwraca widok indeksu zestawu rekordów bazy danych musi zostać pobrany z bazy danych przez wykonywanie zapytania do bazy danych.

Jeśli z drugiej strony, możesz korzystać z pamięci podręcznej danych wyjściowych można uniknąć wykonywania zapytania do bazy danych, za każdym razem, gdy dowolny użytkownik wywołuje tę samą akcję kontrolera. Widok można pobrać z pamięci podręcznej, zamiast jest ponownie wygenerowane na podstawie akcji kontrolera. Włącza buforowanie można uniknąć wykonywania nadmiarowe działa na serwerze.

## <a name="enabling-output-caching"></a>Włączanie buforowania danych wyjściowych

Możesz włączyć buforowanie danych wyjściowych przez dodanie atrybutu [OutputCache] do akcji kontrolera indywidualnych lub klasą całego kontrolera. Na przykład kontroler w ofercie 1 udostępnia akcję o nazwie indeks(). Dane wyjściowe akcji indeks() jest buforowana przez 10 sekund.

**Wyświetlanie listy 1 – Controllers\HomeController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

W wersji Beta programu ASP.NET MVC, buforowanie danych wyjściowych nie działa dla danego adresu URL, takich jak [ http://www.MySite.com/ ](http://www.mysite.com/). Zamiast tego musisz wprowadzić adres URL podobny [ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index). 

W ofercie 1 danych wyjściowych akcji indeks() jest buforowana przez 10 sekund. Jeśli wolisz, możesz określić znacznie dłuższy czas trwania pamięci podręcznej. Na przykład, jeśli chcesz buforować dane wyjściowe akcji kontrolera, przez jeden dzień następnie można określić czas trwania pamięci podręcznej 86400 sekund (60 sekund \* 60 minut \* 24 godziny).

Ma żadnej gwarancji, że zawartość będą buforowane ilości czasu, który określisz. Wszystkie zasoby pamięci stają się niski, pamięci podręcznej automatycznie zostanie uruchomiony wykluczania zawartości.

Kontrolera głównego w ofercie 1 zwraca widok indeksu w ofercie 2. Nie ma nic specjalnego o tym widoku. Widok indeksu po prostu wyświetla bieżący czas (patrz rysunek 1).

**Wyświetlanie listy 2 — Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**Rysunek 1 — widok indeksu pamięci podręcznej**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

Jeśli wywołać wiele razy indeks() akcję, wprowadzając adres URL /Home/Index na pasku adresu przeglądarki i naciśnięcie przycisku Odśwież/ponowne załadowanie w przeglądarce, wielokrotnie, a następnie czas wyświetlany w widoku indeksu nie zostaną zmienione przez 10 sekund. Tym samym czasie jest wyświetlana, ponieważ widok jest buforowany.

Jest ważne dowiedzieć się, że tego samego widoku jest przechowywane w pamięci podręcznej dla wszystkich osób odwiedza aplikacji. Każda osoba, która wywołuje akcję indeks() otrzyma ten sam zbuforowana wersja elementu widoku indeksu. Oznacza to, czy ilość pracy serwera sieci web należy wykonać, aby obsługiwać widoku indeksu jest znacznie mniejsza.

Wyświetl w ofercie 2 odbywa się do realizacji coś, co jest naprawdę proste. W widoku wyświetlane są tylko bieżący czas. Jednak może równie łatwo pamięci podręcznej widoku, który wyświetla zestaw rekordów bazy danych. W takim przypadku zestaw rekordów bazy danych nie musiałby można pobrać z bazy danych za każdym razem, gdy zostanie wywołana akcji kontrolera, który zwraca widok. Buforowanie może zmniejszyć ilość pracy serwera sieci web i serwera bazy danych, należy wykonać.

Nie używaj strony &lt;% @ OutputCache %&gt; dyrektywy w widoku MVC. Ta dyrektywa jest za pośrednictwem, ze świata formularzy sieci Web i nie powinny być używane w aplikacji ASP.NET MVC.

## <a name="where-content-is-cached"></a>Gdy zawartość jest buforowana

Domyślnie, gdy używasz atrybut [OutputCache] zawartość jest buforowana w trzech miejscach: serwer sieci web, serwery proxy i przeglądarki sieci web. Można kontrolować, dokładnie tak, gdy zawartość jest buforowana, modyfikując właściwość Location elementu atrybutu [OutputCache].

Można ustawić właściwość lokalizacji do jednej z następujących wartości:

> · Wszystkie
> 
> · Klient
> 
> · Podrzędne
> 
> · Serwer
> 
> · Brak
> 
> · ServerAndClient


Domyślnie właściwość Location określono wartość Any. Istnieją jednak sytuacje, w których możesz chcieć pamięci podręcznej tylko w przeglądarce lub tylko na serwerze. Na przykład jeśli są buforowania informacje, które jest spersonalizowane dla każdego użytkownika, następnie można powinna nie buforowanie tych informacji na serwerze. W przypadku wyświetlania różnych informacji do różnych użytkowników powinny pamięci podręcznej informacje tylko na komputerze klienckim.

Na przykład kontroler w ofercie 3 udostępnia akcję o nazwie GetName(), która zwraca bieżącą nazwę użytkownika. Jeśli wtyczka loguje się do witryny sieci Web i wywołuje akcję GetName() następnie akcji zwraca ciąg "Hi Jack". Jeśli później, Jill loguje się do witryny sieci Web i wywołuje akcję GetName() następnie ona również otrzyma ciąg "Hi Jack". Ten ciąg jest buforowana na serwerze sieci web dla wszystkich użytkowników po Jack początkowo wywołuje akcję kontrolera.

**Wyświetlanie listy 3 — Controllers\BadUserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

Najprawdopodobniej kontrolera w ofercie 3 nie działa w sposób, który chcesz. Nie chcesz wyświetlić komunikat "Witaj Jack" Jill.

Nigdy nie powinien pamięci podręcznej personalizowania zawartości w pamięci podręcznej na serwerze. Jednakże możesz chcieć personalizowania zawartości w pamięci podręcznej przeglądarki, aby zwiększyć wydajność w pamięci podręcznej. Jeśli buforowanie zawartości w przeglądarce, a użytkownik wywołuje tę samą akcję kontrolera wiele razy, zawartość można pobrać z pamięci podręcznej przeglądarki zamiast serwera.

Zmodyfikowane kontrolera w ofercie 4 zapisuje w pamięci podręcznej danych wyjściowych akcji GetName(). Jednakże zawartość zostanie zbuforowana tylko w przeglądarce, a nie na serwerze. Dzięki temu, gdy wielu użytkowników, wywołaj metodę GetName(), każda osoba pobiera nazwy użytkownika i nazwa użytkownika nie innej osoby.

**Wyświetlanie listy 4 — Controllers\UserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

Zwróć uwagę, że atrybut [OutputCache] w ofercie 4 lokalizacji ustawioną na wartość OutputCacheLocation.Client. Atrybut [OutputCache] zawiera również właściwość NoStore. Właściwość NoStore służy do informowania serwery proxy i przeglądarki, że nie przechowuj trwałe kopię zawartości w pamięci podręcznej.

## <a name="varying-the-output-cache"></a>Różnicowanie wyjściowej pamięci podręcznej

W niektórych sytuacjach może być różne wersje pamięci podręcznej w tej samej zawartości. Wyobraź sobie, na przykład tworzysz strony wzorzec/szczegół. Strona wzorcowa Wyświetla listę tytułów filmu. Po kliknięciu tytuł, można uzyskać szczegółów wybrany film.

Jeśli na stronie szczegółów można buforować, szczegółowe informacje na ten sam film zostanie wyświetlony niezależnie od tego, w których film możesz kliknąć przycisk. Pierwszy film wybranych przez pierwszego użytkownika, pojawi się dla wszystkich użytkowników w przyszłości.

Aby rozwiązać ten problem, korzystając z zalet właściwość VaryByParam atrybutu [OutputCache]. Ta właściwość umożliwia tworzenie różnych wersji pamięci podręcznej w tej samej zawartości, gdy parametr formularza lub zmienia się parametr ciągu zapytania.

Na przykład kontroler w ofercie 5 udostępnia dwie akcje o nazwie Master() i Details(). Akcja Master() zwraca listę tytułów filmów i akcji Details() zwraca szczegółowe informacje dotyczące wybranego filmu.

**Wyświetlanie listy 5 — Controllers\MoviesController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

Akcja Master() zawiera właściwość VaryByParam o wartości "none". Jest zwracany, gdy wyświetlają Master() akcja jest wywoływana, buforowane tę samą wersję główną. Wszelkie parametry formularza lub ciągu zapytania, parametry są ignorowane (patrz rysunek 2).

**Rysunek 2 — widok /Movies/Master**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**Rysunek 3 — widok szczegółów/filmy /**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

Akcja Details() zawiera właściwość VaryByParam o wartości "Id". Gdy różne wartości parametru Id są przekazywane do akcji kontrolera, są generowane różne wersje pamięci podręcznej w widoku szczegółów.

Jest ważne, aby zrozumieć, że za pomocą wyników właściwości VaryByParam w pamięci podręcznej więcej i nie mniejszy. Inna wersja pamięci podręcznej w widoku szczegółów jest tworzona dla każdej innej wersji parametru identyfikatora.

Właściwość VaryByParam można ustawić następujące wartości:

> \* = Utworzyć innej wersji pamięci podręcznej w każdym przypadku, gdy zmienia się formularz lub kwerendę parametr ciągu.
> 
> Brak = Nigdy tworzyć różne wersje pamięci podręcznej
> 
> Średnikami listę parametrów = tworzenie różnych wersji pamięci podręcznej w każdym przypadku, gdy zmienia się dowolny z parametrów ciągu formularza lub kwerendy, na liście


## <a name="creating-a-cache-profile"></a>Tworzenie profilu pamięci podręcznej

Jako alternatywę do konfigurowania właściwości pamięci podręcznej danych wyjściowych, zmieniając właściwości atrybutu [OutputCache] można utworzyć profil pamięci podręcznej w pliku konfiguracji (web.config) w sieci web. Tworzenie profilu pamięci podręcznej w pliku konfiguracji sieci web oferuje kilka ważnych zalet.

Po pierwsze konfigurując buforowania danych wyjściowych w pliku konfiguracji sieci web, możesz kontrolować, jak zawartość w jednej centralnej lokalizacji pamięci podręcznej na akcji kontrolera. Można utworzyć jeden profil pamięci podręcznej i zastosowaniu profilu do kilka kontrolerów i akcji kontrolera.

Po drugie można zmodyfikować pliku konfiguracji sieci web bez konieczności ponownego kompilowania aplikacji. Jeśli zachodzi potrzeba wyłączenia buforowania dla aplikacji, która została już wdrożona do środowiska produkcyjnego, można po prostu zmodyfikuj profile pamięci podręcznej, zdefiniowana w pliku konfiguracyjnym sieci web. Zmiany wprowadzone w pliku konfiguracji sieci web zostanie automatycznie wykrywane i stosowane.

Na przykład &lt;buforowania&gt; sekcji konfiguracji sieci web w ofercie 6 definiuje o nazwie Cache1Hour profil pamięci podręcznej. &lt;Buforowania&gt; sekcji musi znajdować się w &lt;system.web&gt; sekcję pliku konfiguracji sieci web.

**Wyświetlanie listy 6 — pamięć podręczna sekcja pliku web.config**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

Kontroler w ofercie 7 przedstawiono, jak można zastosować profil Cache1Hour do akcji kontrolera, za pomocą atrybutu [OutputCache].

**Wyświetlanie listy 7 — Controllers\ProfileController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

Jeśli wywołanie akcji indeks() udostępnianych przez administratora w ofercie 7 tym samym czasie zostaną zwrócone przez 1 godzinę.

## <a name="summary"></a>Podsumowanie

Buforowanie danych wyjściowych zawiera bardzo prosty sposób ogromnie zwiększa to wydajność aplikacji ASP.NET MVC. W tym samouczku przedstawiono sposób użycia atrybutu [OutputCache] w pamięci podręcznej danych wyjściowych akcji kontrolera. Przedstawiono również sposób modyfikowania właściwości atrybutu [OutputCache], takie jak właściwości czasu trwania i VaryByParam, aby zmodyfikować sposób pobiera buforowania zawartości. Na koniec pokazaliśmy ci, jak zdefiniować profile pamięci podręcznej w pliku konfiguracji sieci web.

> [!div class="step-by-step"]
> [Poprzednie](understanding-action-filters-cs.md)
> [dalej](adding-dynamic-content-to-a-cached-page-cs.md)
