---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Najlepsze praktyki programowania aplikacji (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure) w sieci Web | Dokumentacja firmy Microsoft
author: MikeWasson
description: Tworzenie rzeczywistych aplikacji w chmurze za pomocą platformy Azure Książka elektroniczna jest oparta na prezentacji, opracowane przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które może on...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: f31798032c3b690fcb6dfa8326580255f3412826
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371041"
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Najlepsze praktyki programowania aplikacji sieci Web (tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure)
====================
przez [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Pobierz go naprawić projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie rzeczywistych aplikacji w chmurze dzięki platformie Azure** Książka elektroniczna jest oparta na prezentacji opracowany przez Scotta Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc Ci odnieść sukces, tworzenie aplikacji sieci web w chmurze. Aby uzyskać informacji o książce elektronicznej, zobacz [pierwszy rozdział](introduction.md).


Pierwsze trzy wzorce zostały o konfigurowaniu procesem zwinne Wytwarzanie oprogramowania; pozostałe są o architekturze i kodu. Ten zestaw jest kolekcją najlepsze praktyki programowania aplikacji sieci web:

- [Serwery sieci web bezstanowe](#stateless) za modułem równoważenia obciążenia inteligentne.
- [Należy unikać stanu sesji](#sessionstate) (lub jeśli nie można uniknąć, użyj rozproszonej pamięci podręcznej, a nie bazy danych).
- [Korzystanie z sieci CDN](#cdn) do pamięci podręcznej przeglądarki edge przechowywanie statycznych zasobów plikowych (obrazów, skryptów).
- [Korzystanie z pomocy technicznej async .NET 4.5](#async) celu unikania blokowania połączeń.

Te praktyki są prawidłowe dla wszystkich programowania dla sieci web, nie tylko dla aplikacji w chmurze, ale są one szczególnie ważne w przypadku aplikacji w chmurze. One współdziałają ułatwiające optymalnie wykorzystać możliwość elastycznego skalowania oferowane przez środowisko chmury. Jeśli nie wykonasz te rozwiązania, należy uruchomić do ograniczenia przy próbie skalować swoją aplikację.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Warstwa bezstanowej internetowej za modułem równoważenia obciążenia inteligentne

*Warstwy internetowej o bezstanowa* oznacza, że nie przechowuj wszystkie dane aplikacji w systemie pamięci lub pliku serwera sieci web. Utrzymywanie warstwy internetowej bezstanowe pozwala na oszczędność pieniędzy i zapewnić lepsze doświadczenia klientów:

- Warstwa sieci web jest bezstanowy i znajduje się za modułem równoważenia obciążenia, użytkownik może szybko reagować na zmiany w ruchu aplikacji przez dynamiczne dodawanie lub usuwanie serwerów. W środowisku chmury, w którym płacisz tylko za zasoby serwera dla tak długo, jak faktycznie korzystasz z nich zdolność do reagowania na zmiany zapotrzebowania można przekształcić zapewniło dużą oszczędność.
- Warstwy sieci web o bezstanowa pod względem architektury znacznie łatwiej jest skalowania aplikacji. Która zbyt pozwala reagować na skalowanie potrzeb szybciej i poświęcać mniej pieniędzy na programowanie i testowanie w procesie.
- Serwery chmury, takie jak na serwerach lokalnych, należy zastosować poprawki względem jakiegokolwiek i ponownie uruchamiane od czasu do czasu; a jeśli warstwa sieci web jest bezstanowy, ruch trasy, jeśli serwer ulegnie awarii tymczasowo nie będzie powodować błędy lub nieoczekiwane zachowanie.

Większość aplikacji w rzeczywistych warunkach potrzebne do przechowywania stanu dla sesji sieci web. głównym punktem jest nie Zapisz go na serwerze sieci web. Stan w inny sposób, takie jak można przechowywać na komputerze klienckim w plikach cookie lub poza procesem po stronie serwera w stanie sesji programu ASP.NET przy użyciu dostawcy pamięci podręcznej. Można przechowywać pliki w [Windows Azure Blob storage](unstructured-blob-storage.md) zamiast lokalnego systemu plików.

Jako przykład to, jak łatwo skalować aplikację w programie Windows Azure Web Sites, jeśli warstwy internetowej jest bezstanowy, zobacz **skalowania** kartę dla Windows Azure witryny sieci Web w portalu zarządzania:

![Kartę Skala](web-development-best-practices/_static/image1.png)

Jeśli chcesz dodać serwery sieci web, można po prostu przeciągnij suwak liczba wystąpienia, po prawej stronie. Ustaw ją na 5, a następnie kliknij przycisk **Zapisz**, i w ciągu kilku sekund masz 5 serwerów sieci web w systemie Windows Azure, obsługę ruchu witryny sieci web.

![Pięć wystąpień](web-development-best-practices/_static/image2.png)

Równie łatwo można ustawić liczbę wystąpień w dół do 3 lub wróci ponownie do 1. Gdy skalowanie w dół zaczynasz oszczędzać pieniądze natychmiast, ponieważ Windows Azure, opłaty są naliczane za minutę, nie za godzinę.

Możesz również poinformować platformy Windows Azure, aby automatycznie zwiększać lub zmniejszać liczbę serwerów sieci web na podstawie użycia procesora CPU. W poniższym przykładzie gdy użycie procesora CPU spadnie poniżej 60%, co najmniej 2 zmniejszy liczby serwerów sieci web, a awaria użycie procesora CPU przekracza 80%, maksymalnie do 4 będzie zwiększyć liczbę serwerów sieci web.

![Możesz zmieniać skalę, użycie procesora CPU](web-development-best-practices/_static/image3.png)

Lub co zrobić, jeśli wiadomo, że witryny sieci tylko będzie zajęty, poza godzinami pracy? Możesz przekazać platformy Windows Azure, aby uruchomić wiele serwerów podczas dziennych i zmniejszyć do pojedynczego serwera popołudniami, liczbę spędzonych nocy, a także podczas weekendów. Poniższą sekwencję zrzuty ekranu pokazuje, jak skonfigurować witrynę sieci web, aby uruchomić jeden serwer w poza godzinami pracy i 4 serwery poza godzinami pracy od 8: 00 do 17: 00.

![Skalowanie zgodnie z harmonogramem](web-development-best-practices/_static/image4.png)

![Ustawianie czasu harmonogramu](web-development-best-practices/_static/image5.png)

![Dziennych harmonogramu](web-development-best-practices/_static/image6.png)

![Harmonogram weeknight](web-development-best-practices/_static/image7.png)

![Harmonogram weekend](web-development-best-practices/_static/image8.png)

I oczywiście wszystko to może odbywać się w skryptach, jak również tak jak w portalu.

Zdolność aplikacji do skalowania w poziomie jest niemal nieograniczoną na platformie Windows Azure, tak długo, jak można uniknąć przeszkody w celu dynamiczne dodawanie lub usuwanie maszyn wirtualnych z serwera, przechowując bezstanowe warstwa sieci web.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Należy unikać stanu sesji

Często nie jest praktyczne w chmurze w rzeczywistych warunkach aplikacji bez przechowywania pewnego rodzaju stanu dla sesji użytkownika, ale niektóre podejścia wpłynąć na wydajność i skalowalność więcej niż inne. W przypadku przechowywania stanu, najlepszym rozwiązaniem jest zachowywanie małych ilości stanu i zapisz go w plikach cookie. Jeśli nie jest to możliwe, dalej najlepszym rozwiązaniem jest stanu sesji platformy ASP.NET za pomocą dostawcy dla [rozproszona, wewnątrzpamięciowa pamięć podręczna](distributed-caching.md#sessionstate). Najgorszy rozwiązania z punktu widzenia skalowalności i wydajności jest korzystanie z bazy danych kopii dostawca stanu sesji.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Korzystanie z sieci CDN do buforowania przechowywanie statycznych zasobów plikowych

Sieć CDN jest akronimem Content Delivery Network. Podaj przechowywanie statycznych zasobów plikowych, takich jak obrazy i pliki skryptów do dostawcy sieci CDN, a dostawca buforuje te pliki w centrach danych na całym świecie, tak, aby wszędzie tam, gdzie osobom dostęp do Twoich aplikacji, uzyskiwać stosunkowo szybkich odpowiedzi i małe opóźnienia dla pamięci podręcznej zasoby. Przyspiesza ogólny czas ładowania, witryny i zmniejsza obciążenie na serwerach sieci web. Sieci CDN są szczególnie ważne, jeśli chcesz się połączyć na odbiorców, którzy powszechnie rozpowszechniane w geograficznie.

System Windows Azure oferuje usługi CDN, a inne usługi CDN można użyć w aplikacji, która działa na platformie Windows Azure lub dowolnego środowiska hostingu w sieci web.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Korzystać z pomocy technicznej async .NET 4.5 w celu unikania blokowania połączeń

.NET 4.5 rozszerzone językach programowania C# i VB w celu przyspiesza do obsługi zadań asynchronicznie. Korzyści wynikające z programowania asynchronicznego nie jest po prostu przetwarzanie równoległe sytuacji, gdy zachodzi potrzeba jednocześnie uruchamiać wiele wywołań usługi sieci web. Umożliwia ona także serwer sieci web do wykonywania bardziej wydajne i niezawodne w warunkach dużym obciążeniem. Serwer sieci web ma tylko ograniczoną liczbę dostępnych wątków i w warunkach obciążenia o wysokiej gdy wszystkie wątki są używane, przychodzących żądań trzeba poczekać, aż wątki są zwalniane. Jeśli kod aplikacji nie obsługuje zadań, takich jak bazy danych zapytań i wywołania usługi sieci web asynchronicznie, wiele wątków niepotrzebnie są powiązane, gdy serwer oczekuje na odpowiedź operacji We/Wy. Ogranicza to ilość ruchu sieciowego, które serwer może obsłużyć w warunkach dużym obciążeniem. Za pomocą programowania asynchronicznego, wątki, które oczekują na usługi sieci web lub bazy danych, aby zwrócić dane są zwalniane do nowych żądań usługi aż do danych została odebrana. Na serwerze sieci web zajęty setek lub tysięcy żądań, które następnie mogą być przetwarzane niezwłocznie które w przeciwnym razie będzie czekał wątków, aby zwolnić.

Jak wcześniej, jest bardzo proste zmniejszyć liczbę serwerów sieci web, obsługa witryny sieci web, ponieważ ma ono zwiększyć ich. Jeśli serwer może osiągnąć większą przepustowość, nie należy więc jako wiele z nich i zmniejszyć koszty, ponieważ wymaga mniejszej liczby serwerów w woluminie danego ruchu sieciowego niż inaczej w przypadku.

Obsługę asynchronicznego modelu programowania .NET 4.5 znajduje się w programie ASP.NET 4.5 Web Forms, MVC i interfejsu API sieci Web; w programie Entity Framework 6, a w [interfejsu API usługi Windows Azure Storage](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Asynchroniczna pomoc techniczna w programie ASP.NET 4.5

W programie ASP.NET 4.5 wsparcie dla programowania asynchronicznego został dodany, nie tylko do języka, ale także do platformy MVC, Web Forms i interfejsu API sieci Web. Na przykład metoda akcji kontrolera ASP.NET MVC odbiera dane z żądania sieci web i przekazuje dane do widoku, które tworzy kod HTML do wysłania do przeglądarki. Często metody akcji musi pobierać dane z bazy danych, usługi sieci web, aby wyświetlić go na stronie sieci web lub aby zapisać dane wprowadzone na stronie sieci web. W tych scenariuszach można łatwo stworzyć asynchronicznej metody akcji jest: zamiast zwracać *ActionResult* obiektu powrócisz *zadań&lt;ActionResult&gt;*  i oznacz metodę za pomocą *async* — słowo kluczowe. Wewnątrz metody gdy wiersz kodu dotyczącego operacji, która obejmuje czas oczekiwania, możesz oznaczyć go za pomocą słowa kluczowego await.

Poniżej przedstawiono metody prostej akcji, która wywołuje metodę repozytorium dla zapytania do bazy danych:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

A Oto tej samej metody, która obsługuje asynchronicznie wywołanie bazy danych:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

Dzieje się w tle, kompilator generuje odpowiedni kod asynchroniczny. Jeśli aplikacja sprawia, że wywołanie `FindTaskByIdAsync`, sprawia, że program ASP.NET `FindTask` żądania, a następnie rozwija wątku roboczego i udostępnia je przetwarzać kolejne żądanie. Gdy `FindTask` żądania jest wykonywane, wątek jest uruchomiony ponownie w celu kontynuować przetwarzanie kod, który pochodzi od tego wywołania. W okresie przejściowym między `FindTask` żądanie jest inicjowane, a gdy dane są zwracane, masz wątku dostępne do wykonywania użytecznej pracy, które w przeciwnym wypadku będą powiązane oczekiwania na odpowiedź.

Istnieje pewien narzut dla kodu asynchronicznego w warunkach o niskim obciążeniu, że obciążenie jest jednak niewielkie podczas w warunkach obciążenia o wysokiej, których jesteś w stanie przetworzyć żądania, które w przeciwnym razie mogłyby zostać wstrzymane do czasu na dostępne wątki.

Było możliwe do zrobienia tego rodzaju programowania asynchronicznego, ponieważ ASP.NET 1.1, ale jest trudne do pisania, podatne na błędy i problemy z debugowaniem. Teraz, gdy Uprościliśmy kodowania go w programie ASP.NET 4.5 nie ma powodu nie można wykonać go dłużej.

### <a name="async-support-in-entity-framework-6"></a>Asynchroniczna pomoc techniczna w Entity Framework 6

Jako część asynchroniczna pomoc techniczna w 4.5, firma Microsoft dostarczane asynchroniczna pomoc techniczna dla wywołania usługi sieci web sockets i system plików we/wy, ale Najczęstszym wzorcem dla aplikacji sieci web jest trafień bazę danych, a naszych bibliotek danych nie obsługuje asynchroniczne. Teraz Entity Framework 6 dodaje async obsługę dostępu do bazy danych.

W programie Entity Framework 6 wszystkie metody, które powodują kwerendy lub polecenia do wysłania do bazy danych ma wersje asynchroniczne. W tym przykładzie przedstawiono wersja async *znaleźć* metody.

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

I to obsługa async działa nie tylko dla operacji wstawienia, usuwa, aktualizacji i proste znajduje, współdziała również z zapytań LINQ:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Brak `Async` wersję `ToList` metody ponieważ w tym kodzie metody, która powoduje, że zapytanie w celu wysłania do bazy danych. `Where` i `OrderByDescending` metody tylko skonfigurować zapytania, podczas gdy `ToListAsync` metoda wykonuje kwerendę i zapisuje odpowiedź w `result` zmiennej.

## <a name="summary"></a>Podsumowanie

Można wdrożyć w sieci web development najlepsze rozwiązania opisane w tym miejscu, w dowolnej sieci web programowania framework i dowolnego środowiska chmury, ale mamy narzędzi na platformie ASP.NET i Windows Azure, aby ułatwić. Jeśli wykonasz te wzorce, można łatwo skalować w poziomie warstwy internetowej, a następnie będzie można zminimalizować wydatków, ponieważ każdy serwer będzie mógł obsłużyć większy ruch.

[Następny rozdział](single-sign-on.md) analizuje jak chmura umożliwia scenariusze rejestracji jednokrotnej.

## <a name="resources"></a>Resources

Więcej informacji na ten temat można znaleźć w następujących zasobach.

Serwery bezstanowej internetowej:

- [Microsoft Patterns and Practices — wskazówki dotyczące skalowania automatycznego](https://msdn.microsoft.com/library/dn589774.aspx).
- [Wyłączanie funkcji ARR wystąpienia koligacji w systemie Windows Azure Web Sites](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Wpis na blogu autorstwa Ereza Benariego wyjaśnia, koligacja sesji narzędzia Windows Azure Web Sites.

CDN:

- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalne, odporne](https://channel9.msdn.com/Series/FailSafe). Seria filmów dziewięć części Ulrich Homann, Marc Mercuri i — Markiem Simmsem. Zobacz Omówienie usługi CDN w odcinek 3 począwszy od 1:34:00.
- [Wzorzec Microsoft Patterns i praktyki Hosting zawartości statycznej](https://msdn.microsoft.com/library/dn589776.aspx)
- [Przeglądy CDN](http://www.cdnreviews.com/). Omówienie liczbę CDN.

Programowanie asynchroniczne:

- [Używanie metod asynchronicznych we wzorcu ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Samouczek autorstwa Ricka Andersona.
- [Programowanie asynchroniczne z Async i Await (C# i Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). MSDN oficjalny dokument, który objaśnia, uzasadnienie dla programowania asynchronicznego, jak to działa w programie ASP.NET 4.5 i jak napisać kod do jego wdrożenia.
- [Entity Framework Async zapytania i Zapisz](https://msdn.microsoft.com/data/jj819165)
- [Jak tworzyć aplikacje sieci Web platformy ASP.NET przy użyciu Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Prezentacja wideo przez Rowan Miller. Zawiera pokaz graficznych programowania asynchronicznego w jaki sposób można ułatwić znaczący wzrost przepływności serwerów sieci web w warunkach obciążenia o wysokiej.
- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalne, odporne](https://channel9.msdn.com/Series/FailSafe). Seria filmów dziewięć części Ulrich Homann, Marc Mercuri i — Markiem Simmsem. Do dyskusji dotyczących wpływu programowanie asynchroniczne na skalowalność Zobacz odcinek 4 i odcinek 8.
- [Magiczna używania metod asynchronicznych w ASP.NET 4.5, a także ważne problemy](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Wpis na blogu autorstwa Scotta Hanselmana, przede wszystkim dotyczące używania asynchroniczne w aplikacjach formularzy sieci Web ASP.NET.

Najlepsze praktyki programowania aplikacji sieci web dodatkowe na ten temat można znaleźć w następujących zasobach:

- [Poprawka go przykładowej aplikacji — najlepsze rozwiązania](the-fix-it-sample-application.md#bestpractices). Dodatek do tę książkę elektroniczną wymieniono szereg najlepszych rozwiązań, które zostały wdrożone w aplikacji naprawić.
- [Lista kontrolna dla deweloperów sieci Web](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Poprzednie](continuous-integration-and-continuous-delivery.md)
> [dalej](single-sign-on.md)
