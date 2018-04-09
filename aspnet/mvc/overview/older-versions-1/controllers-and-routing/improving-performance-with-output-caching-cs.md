---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
title: Poprawa wydajności przy użyciu Output buforowania (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: Z tego samouczka dowiesz się, jak można znacznie poprawić wydajność aplikacji sieci web platformy ASP.NET MVC dzięki wykorzystaniu buforowanie danych wyjściowych. Możesz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 521c9117-81cd-4d8d-9d96-0256dc7bf50f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-cs
msc.type: authoredcontent
ms.openlocfilehash: 8958caa5a0ccad669ca861bed261102625be5cb6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="improving-performance-with-output-caching-c"></a>Poprawa wydajności przy użyciu reguł wyjściowej pamięci podręcznej (C#)
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Z tego samouczka dowiesz się, jak można znacznie poprawić wydajność aplikacji sieci web platformy ASP.NET MVC dzięki wykorzystaniu buforowanie danych wyjściowych. Sposób buforować wyniki zwrócone z akcji kontrolera, dzięki czemu tę samą zawartość nie muszą zostać utworzone czasu każdy nowy użytkownik wywołuje akcję.


Celem tego samouczka jest wyjaśnienie, jak można znacznie poprawić wydajność aplikacji platformy ASP.NET MVC dzięki wykorzystaniu pamięci podręcznej danych wyjściowych. Wyjściowej pamięci podręcznej umożliwia buforowanie zawartości zwrócony przez akcji kontrolera. W ten sposób tej samej zawartości nie trzeba można wygenerować każdym razem, gdy ta sama akcja kontrolera jest wywoływany.

Załóżmy, że aplikacji ASP.NET MVC zostanie wyświetlona lista rekordów bazy danych w widoku o nazwie indeksu. Zwykle czas każdy użytkownik wywołuje akcji kontrolera, który zwraca widok indeksu zestaw rekordów bazy danych musi zostać pobrany z bazy danych, wykonując zapytanie bazy danych.

Jeśli z drugiej strony, możesz korzystać z pamięci podręcznej danych wyjściowych można uniknąć wykonywania zapytania do bazy danych, za każdym razem, gdy każdy użytkownik wywołuje tę samą akcję kontrolera. Widok można pobrać z pamięci podręcznej, zamiast ponownie generowane z akcji kontrolera. Włącza buforowanie można uniknąć wykonywania nadmiarowe działa na serwerze.

## <a name="enabling-output-caching"></a>Włączanie buforowania danych wyjściowych

Należy włączyć buforowanie danych wyjściowych przez dodanie atrybutu [OutputCache] do akcji kontrolera poszczególnych lub klasy całego kontrolera. Na przykład kontrolera 1 lista przedstawia o nazwie indeks() akcji. Dane wyjściowe działania indeks() jest buforowana przez 10 sekund.

**Wyświetlanie listy 1 – Controllers\HomeController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample1.cs)]

W wersji Beta programu ASP.NET MVC, buforowanie danych wyjściowych nie działa dla danego adresu URL, takie jak [ http://www.MySite.com/ ](http://www.mysite.com/). Zamiast tego należy wprowadzić adres URL podobny [ http://www.MySite.com/Home/Index ](http://www.mysite.com/Home/Index). 

Wyświetlanie listy 1 buforowania danych wyjściowych akcji indeks() 10 sekund. Jeśli wolisz, możesz określić wiele dłuższy czas buforowania. Na przykład, jeśli chcesz buforować wyniki akcji kontrolera przez jeden dzień następnie można określić czas trwania pamięci podręcznej 86400 sekund (60 sekund \* 60 minut \* 24 godzin).

Brak żadnej gwarancji, że zawartość będą buforowane ilości czasu, który określisz. Zabraknąć zasobów pamięci pamięci podręcznej automatycznie zostanie uruchomiony wykluczania zawartości.

Kontrolera 1 wyświetlania w głównej zwraca widok indeksu w wyświetlania 2. Nie ma specjalnych temat tego widoku. Widok indeksu po prostu wyświetla bieżący czas (zobacz rysunek 1).

**Wyświetlanie listy 2 — Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-cs/samples/sample2.aspx)]

**Rysunek 1 — widok indeksu pamięci podręcznej**

![clip_image002](improving-performance-with-output-caching-cs/_static/image1.jpg)

Jeśli wywołanie wielokrotnie akcji indeks() wprowadzając /Home adresu URL/indeksu na pasku adresu przeglądarki, a naciśnięcie przycisku Odśwież/Załaduj ponownie w przeglądarce, wielokrotnie, a następnie czasu wyświetlanych w widoku indeksu nie zmieni się przez 10 sekund. Jednocześnie jest wyświetlana, ponieważ widok jest buforowana.

Należy zrozumieć, że dla każdej osoby, osób odwiedzających aplikacji jest buforowana tego samego widoku. Każdy użytkownik, który wywołuje akcję indeks() otrzyma tej samej wersji buforowanego widoku indeksu. Oznacza to, że ilość pracy serwera sieci web należy wykonać, aby obsługiwać widoku indeksu jest znacznie zmniejszyć.

Widok wyświetlania 2 odbywa się do realizacji coś naprawdę proste. W widoku wyświetlane są tylko bieżącego czasu. Jednak może równie łatwo pamięci podręcznej wyświetlony widok zawierający zestaw rekordów bazy danych. W takim przypadku zestaw rekordów bazy danych nie musi mają zostać pobrane z bazy danych za każdym razem, gdy jest wywoływana akcji kontrolera, który zwraca widok. Buforowanie może zmniejszyć ilość pracy serwera sieci web i serwera bazy danych należy wykonać.

Nie używaj strony &lt;% @ OutputCache %&gt; dyrektywy w widoku MVC. Ta dyrektywa jest marginesy za pośrednictwem ze świata formularzy sieci Web i nie powinna być używana w aplikacji platformy ASP.NET MVC.

## <a name="where-content-is-cached"></a>Gdy zawartość jest buforowana

Domyślnie, gdy używasz atrybutu [OutputCache] zawartość jest buforowana w trzech miejscach: serwer sieci web, wszystkie serwery proxy i przeglądarki sieci web. Można kontrolować, dokładnie, gdy zawartość jest buforowana przez modyfikowanie właściwości Location atrybutu [OutputCache].

Można ustawić właściwości Location do jednej z następujących wartości:

> · Any
> 
> · Klienta
> 
> · Downstream
> 
> · Serwer
> 
> · Brak
> 
> · ServerAndClient


Domyślnie właściwości Location ma żadnych wartości. Jednakże istnieją sytuacje, w których warto pamięci podręcznej tylko w przeglądarce lub tylko na serwerze. Na przykład jeśli są buforowania informacji spersonalizowaną dla każdego użytkownika następnie użytkownik powinien nie buforowanie tych informacji na serwerze. W przypadku wyświetlania różnych informacji do różnych użytkowników powinny pamięci podręcznej informacje tylko na kliencie.

Na przykład kontroler w 3 lista przedstawia akcji o nazwie GetName(), która zwraca bieżącą nazwę użytkownika. Jeśli wtyczka logowania do witryny sieci Web i wywołuje akcję GetName() następnie akcji zwraca ciąg "Hi gniazda". Jeśli następnie Jill logowania do witryny sieci Web i wywołuje akcję GetName() następnie ona również otrzyma ciąg "Hi gniazda". Ciąg są buforowane na serwerze sieci web dla wszystkich użytkowników, po gniazda początkowo wywołuje akcji kontrolera.

**Wyświetlanie listy 3 — Controllers\BadUserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample3.cs)]

Najprawdopodobniej kontrolera w 3 wyświetlania nie działa w sposób, który ma. Nie chcesz wyświetlenie komunikatu "Hi gniazda" Jill.

Nigdy nie powinien pamięci podręcznej personalizowania zawartości w pamięci podręcznej serwera. Jednak można buforować personalizowania zawartości w pamięci podręcznej przeglądarki, aby zwiększyć wydajność. Jeśli buforowanie zawartości w przeglądarce, a użytkownik wywołuje tę samą akcję kontrolera wiele razy, zawartość można pobrać z pamięci podręcznej przeglądarki zamiast serwera.

Zmodyfikowane kontrolera w listę 4 buforuje dane wyjściowe działania GetName(). Jednak zawartość jest buforowana tylko w przeglądarce, a nie na serwerze. W ten sposób w przypadku wielu użytkowników wywołania metody GetName() każda osoba pobiera nazwy użytkownika i nazwa użytkownika nie innej osoby.

**Wyświetlanie listy 4 — Controllers\UserController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample4.cs)]

Należy zauważyć, że atrybut [OutputCache] w przypadku wyświetlania 4 zawiera właściwość lokalizacji ustawioną wartość OutputCacheLocation.Client. Atrybut [OutputCache] zawiera także właściwości NoStore. Właściwość NoStore jest używana do informują serwerów proxy i przeglądarki, że nie Przechowaj trwałą kopię zawartości w pamięci podręcznej.

## <a name="varying-the-output-cache"></a>Zróżnicowanie wyjściowej pamięci podręcznej

W niektórych sytuacjach może być różne wersje tej samej zawartości pamięci podręcznej. Załóżmy, że tworzenia strony wzorzec/szczegół. Strony wzorcowej Wyświetla listę tytułów filmu. Po kliknięciu tytuł, możesz uzyskać szczegółowe informacje wybrany film.

Jeśli buforowanie strony szczegółów, następnie szczegóły dla tego samego filmu pojawi się niezależnie od tego, które filmu kliknij. Pierwszy filmu wybranych przez pierwszego użytkownika będzie widoczny dla wszystkich przyszłych użytkowników.

Aby rozwiązać ten problem, należy wykorzystać właściwość VaryByParam atrybutu [OutputCache]. Ta właściwość umożliwia tworzenie pamięci podręcznej wersje tej samej zawartości, gdy parametr formularza lub zmienia się parametr ciągu zapytania.

Na przykład kontroler w listę 5 udostępnia dwa działania o nazwie Master() i Details(). Akcja Master() zwraca listę tytułów filmów i akcji Details() Zwraca szczegóły wybrany film.

**Wyświetlanie listy 5 — Controllers\MoviesController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample5.cs)]

Akcja Master() zawiera właściwość VaryByParam o wartości "none". Po wywołaniu akcji Master() tej samej wersji pamięci podręcznej wzorca wyświetlić są zwracane. Żadnych parametrów formularza lub ciągu zapytania, które parametry są ignorowane (patrz rysunek 2).

**Rysunek 2 — widok /Movies/Master**

![clip_image004](improving-performance-with-output-caching-cs/_static/image2.jpg)

**Rysunek 3 – widoku szczegółów/filmów /**

![clip_image006](improving-performance-with-output-caching-cs/_static/image3.jpg)

Akcja Details() zawiera właściwość VaryByParam o wartości "Id". Różne wersje pamięci podręcznej w widoku szczegółów są generowane, gdy różne wartości parametru identyfikatora są przekazywane do akcji kontrolera.

Należy pamiętać, że za pomocą właściwości VaryByParam wyników w ramach buforowania więcej i mniej nie jest. Inna wersja buforowanego widoku szczegółów jest tworzona dla różnych wersji parametru identyfikatora.

Można ustawić właściwości VaryByParam na następujące wartości:

> \* = Utworzyć inną wersję pamięci podręcznej zawsze, gdy zmienia się parametr typu string lub kwerendzie formularza.
> 
> Brak = Nigdy tworzyć różne wersje pamięci podręcznej
> 
> Średnikami lista parametrów = Utwórz różne wersje pamięci podręcznej w każdym przypadku, gdy zmienia się wszystkie parametry ciągu formularza lub kwerendy, na liście


## <a name="creating-a-cache-profile"></a>Tworzenie profilu pamięci podręcznej

Jako alternatywę do skonfigurowania właściwości pamięci podręcznej danych wyjściowych przez modyfikowanie właściwości atrybutu [OutputCache] można utworzyć profil pamięci podręcznej w pliku konfiguracyjnym (web.config) w sieci web. Tworzenie profilu pamięci podręcznej w pliku konfiguracji sieci web udostępnia kilka ważnych zalet.

Po pierwsze konfigurując buforowanie danych wyjściowych w pliku konfiguracji sieci web, można kontrolować sposób akcji kontrolera buforowanie zawartości w jednej centralnej lokalizacji. Można utworzyć jeden profil pamięci podręcznej i dotyczą profil kilka kontrolerów i akcji kontrolera.

Po drugie można zmodyfikować pliku konfiguracji sieci web bez konieczności ponownego kompilowania aplikacji. Jeśli trzeba wyłączyć buforowanie dla aplikacji, która już została wdrożona do środowiska produkcyjnego, można po prostu zmodyfikować profile pamięci podręcznej określonych w pliku konfiguracji sieci web. Zmiany wprowadzone w pliku konfiguracji sieci web zostanie automatycznie wykrywane i stosowane.

Na przykład &lt;buforowanie&gt; sekcji konfiguracji sieci web w 6 wyświetlania definiuje o nazwie Cache1Hour profilu pamięci podręcznej. &lt;Buforowanie&gt; sekcji musi występować w &lt;system.web&gt; sekcji pliku konfiguracji sieci web.

**Wyświetlanie listy 6 — buforowanie sekcja pliku web.config**

[!code-xml[Main](improving-performance-with-output-caching-cs/samples/sample6.xml)]

Kontroler w wyświetlania 7 przedstawiono, jak profil Cache1Hour można zastosować do akcji kontrolera, atrybutem [OutputCache].

**Wyświetlanie listy 7 — Controllers\ProfileController.cs**

[!code-csharp[Main](improving-performance-with-output-caching-cs/samples/sample7.cs)]

Jeśli wywołanie akcji indeks() udostępnianych przez kontrolera w wyświetlania 7 jednocześnie zostanie zwrócony przez godzinę.

## <a name="summary"></a>Podsumowanie

Buforowanie danych wyjściowych zawiera bardzo łatwa metoda znacznie poprawy wydajności aplikacji ASP.NET MVC. W tym samouczku przedstawiono sposób użycia atrybutu [OutputCache] do buforowania danych wyjściowych akcji kontrolera. Przedstawiono również sposób zmodyfikować właściwości atrybutu [OutputCache], takie jak właściwości Duration i VaryByParam można zmodyfikować sposób pobiera buforowania zawartości. Ponadto przedstawiono sposób definiowania profilów pamięci podręcznej w pliku konfiguracji sieci web.

> [!div class="step-by-step"]
> [Poprzednie](understanding-action-filters-cs.md)
> [dalej](adding-dynamic-content-to-a-cached-page-cs.md)
