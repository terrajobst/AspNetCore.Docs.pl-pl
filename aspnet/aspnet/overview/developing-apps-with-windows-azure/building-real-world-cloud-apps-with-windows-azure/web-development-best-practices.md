---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Sieci Web Development Best Practices (kompilowanie praktyczne aplikacje w chmurze platformy Azure) | Dokumentacja firmy Microsoft
author: MikeWasson
description: "Kompilowanie rzeczywistych World aplikacje w chmurze z Azure Książka elektroniczna jest oparta na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i rozwiązań, które może on..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: de536a0ca39cb752c0962f0c4ae36eb00b586bff
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Najlepsze rozwiązania Projektowanie sieci Web (kompilowanie praktyczne aplikacje w chmurze platformy Azure)
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Dykstra niestandardowy](https://github.com/tdykstra)

[Pobieranie napraw projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę E](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenia rzeczywistych aplikacji w chmurze platformy Azure** Książka elektroniczna opiera się na prezentacji opracowane przez Scott Guthrie. Wyjaśniono 13 wzorców i wskazówki, które mogą pomóc Ci się pomyślnie, tworzenie aplikacji sieci web dla chmury. Informacje o Książka elektroniczna, zobacz [pierwszy rozdział](introduction.md).


Pierwsze trzy wzorce zostały o konfigurowaniu procesu programowania zwinnego; pozostałe są dotyczące architektury i kod. Ten zestaw jest kolekcją web development najlepsze rozwiązania:

- [Serwery sieci web bezstanowych](#stateless) za modułem równoważenia obciążenia inteligentne.
- [Unikaj stanu sesji](#sessionstate) (lub jeśli nie można go uniknąć, użyj rozproszonej pamięci podręcznej, a nie bazy danych).
- [Korzystanie z sieci CDN](#cdn) do pamięci podręcznej krawędzi plików statycznych zasobów (obrazów, skryptów).
- [Używać funkcji asynchronicznej .NET 4.5](#async) celu unikania blokowania połączeń.

Praktyki te są prawidłowe dla wszystkich opracowywanie sieci web, nie tylko dla aplikacji w chmurze, ale są szczególnie ważne w przypadku aplikacji w chmurze. One współdziałać, aby ułatwić optymalne wykorzystanie bardzo elastyczne skalowanie oferowane przez środowisko chmury. Jeśli nie stosować te rozwiązania, uruchomisz do ograniczeń przy próbie skalowania aplikacji.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Warstwy web bezstanowych za modułem równoważenia obciążenia inteligentne

*Warstwa sieci web bezstanowych* oznacza przechowywanie danych aplikacji w systemie pliku lub pamięci serwera sieci web. Utrzymywanie warstwę web bezstanowych umożliwia zarówno lepsze środowisko pracy klienta i oszczędności:

- Warstwa sieci web jest bezstanowych, znajduje się za usługą równoważenia obciążenia szybko reagować na zmiany w aplikacji ruchu przez dynamiczne dodawanie lub usuwanie serwerów. W środowisku chmury, w którym płacisz tylko za zasoby serwera dla tak długo, jak je rzeczywiście używane możliwość odpowiadanie na zmiany w żądanie można przekształcić dużych oszczędności.
- Warstwy web bezstanowych jest pod względem architektury znacznie łatwiejsze skalowanie aplikacji. Zbyt umożliwiająca odpowiadanie na szybkie skalowanie potrzeb i kupować mniej projektowania i testowania w procesie.
- Serwery chmury, takich jak serwery lokalne, muszą być poprawiono i ponownie uruchomić od czasu do czasu; a jeśli warstwa sieci web jest bezstanowych, trasy ruchu, gdy serwer tymczasowo przestanie działać nie będzie powodować błędy lub nieoczekiwane zachowanie.

Większość aplikacji rzeczywistych potrzebne do przechowywania stanu dla sesji sieci web. najistotniejsze jest nie przechowuj go na serwerze sieci web. Stan w inny sposób, takie jak można przechowywać na komputerze klienckim w plikach cookie lub poza procesem po stronie serwera w stanie sesji programu ASP.NET przy użyciu dostawcy pamięci podręcznej. Można przechowywać pliki w [systemu Windows Azure Blob storage](unstructured-blob-storage.md) zamiast lokalnego systemu plików.

Przykład, jak łatwo jest skalowanie aplikacji w systemie Windows Azure Web Sites przypadku bezstanowych warstwę web, zobacz **skali** kartę dla systemu Windows Azure witryny sieci Web w portalu zarządzania:

![Kartę Skala](web-development-best-practices/_static/image1.png)

Jeśli chcesz dodać serwery sieci web, wystarczy przeciągnąć suwak liczby wystąpień w prawo. Ustaw ją na 5, a następnie kliknij przycisk **zapisać**, i w ciągu kilku sekund masz 5 serwerów sieci web w systemie Windows Azure, obsługę ruchu witryny sieci web.

![Pięć wystąpień](web-development-best-practices/_static/image2.png)

Równie łatwo można ustawić liczbę wystąpień do 3 lub powrót do 1. Skalowanie kopii uruchomieniu, co pozwala zaoszczędzić natychmiast, ponieważ systemu Windows Azure opłaty za minutę, nie za godzinę.

Możesz także wybrać systemu Windows Azure, aby automatycznie zwiększyć lub zmniejszyć liczbę serwerów sieci web na podstawie użycia procesora CPU. W poniższym przykładzie gdy użycie Procesora poniżej 60%, co najmniej 2 spowoduje obniżenie liczby serwerów sieci web, a jeśli użycie procesora CPU przekracza 80%, maksymalnie 4 będzie zwiększyć liczbę serwerów sieci web.

![Skalować według użycia Procesora](web-development-best-practices/_static/image3.png)

Lub co zrobić, jeśli wiadomo, że witryna tylko będzie zajęty podczas godzin pracy? Można ustalić Windows Azure, aby uruchomić wiele serwerów podczas dziennych i zmniejszyć na jednym serwerze popołudniami, liczbę spędzonych nocy i w weekendy. Następujący szereg zrzuty ekranu pokazuje, jak można skonfigurować witrynę sieci web, aby uruchomić jeden serwer w poza godzinami pracy i 4 serwerów podczas godzin pracy od 8 do 17: 00.

![Skalować zgodnie z harmonogramem](web-development-best-practices/_static/image4.png)

![Ustawianie czasu harmonogramu](web-development-best-practices/_static/image5.png)

![Dziennych harmonogramu](web-development-best-practices/_static/image6.png)

![Harmonogram weeknight](web-development-best-practices/_static/image7.png)

![Harmonogram weekendowe](web-development-best-practices/_static/image8.png)

I oczywiście wszystko to może odbywać się w skryptach, jak również w portalu.

Aplikacji do skalowania w poziomie jest praktycznie nieograniczona w systemie Windows Azure, tak długo, jak można uniknąć przeszkody dynamiczne dodawanie lub usuwanie maszyn wirtualnych serwera, przechowując bezstanowych warstwa sieci web.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Unikaj stanu sesji

Często nie jest praktyczne w aplikacji rzeczywistych chmury, aby uniknąć przechowywania jakiegoś stanu dla sesji użytkownika, ale niektóre podejścia mieć wpływ na wydajność i skalowalność więcej niż inne. Jeśli masz przechowywanie stanu, najlepszym rozwiązaniem jest zachowywanie małych rozmiarów ilość stanu i zapisz go w plikach cookie. Jeśli, który nie jest możliwe, dalej najlepszym rozwiązaniem jest użycie stanu sesji ASP.NET dostawcy dla [rozproszonych, w pamięci podręcznej](distributed-caching.md#sessionstate). Najgorszy rozwiązania z punktu widzenia skalowalności i wydajności jest korzystanie z bazy danych kopii dostawcę stanu sesji.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Korzystanie z sieci CDN do buforowania zasobów plików statycznych

CDN jest akronimem Content Delivery Network. Podaj plik statyczny zasobów, takich jak obrazy i plików skryptów do dostawcy sieci CDN w warstwie, a dostawca przechowuje te pliki w centrach danych na całym świecie tak, aby wszędzie tam, gdzie osobom dostęp do aplikacji, uzyskiwać stosunkowo szybkich odpowiedzi i małe opóźnienia dla pamięci podręcznej zasoby. Przyspiesza całkowity czas obciążenia w lokacji i zmniejsza obciążenie na serwerach sieci web. CDN są szczególnie istotne, jeśli chcesz się połączyć odbiorców, który jest powszechnie geograficznie rozproszonych.

Windows Azure ma CDN, a inne CDN można użyć w aplikacji, która działa w systemie Windows Azure lub dowolnego środowiska hostingu w sieci web.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Korzystać z obsługi async .NET 4.5 w celu unikania blokowania połączeń

.NET 4.5 rozszerzone języków programowania C# i VB, tak aby była znacznie prostsza do obsługi zadań asynchronicznie. Zaletą programowanie asynchroniczne nie jest tylko do przetwarzania równoległego sytuacji, kiedy zachodzi potrzeba zaczną działać jednocześnie poza wielu wywołań usług sieci web. Umożliwia również umożliwia efektywniejsze wykonywanie na serwerze sieci web i niezawodne w warunkach dużego obciążenia. Serwer sieci web ma tylko ograniczoną liczbę dostępnych wątków i warunkach dużego obciążenia podczas wszystkie wątki są używane, żądań przychodzących trzeba poczekaj, aż wątki są zwalniane. Jeśli kod aplikacji nie obsługuje zadania, takie jak bazy danych zapytań i wywołania usługi sieci web asynchronicznie, wiele wątków niepotrzebnie są również powiązane, gdy serwer jest oczekiwanie na odpowiedź operacji We/Wy. To ogranicza ilość ruchu sieciowego, który serwer może obsłużyć warunkach dużego obciążenia. Programowania asynchronicznego wątków oczekujących dla usługi sieci web lub bazy danych, aby zwrócić dane są zwalniane do obsługi nowych żądań do danych zostanie odebrany. Na serwerze sieci web zajęty setek lub tysięcy żądań, które można następnie przetworzenie niezwłocznie, które mogłyby w przeciwnym razie oczekuje na wątków, aby zwolnić.

Jak przedstawiono wcześniej, jest równie proste zmniejszyć liczbę serwerów sieci web obsługi witryny sieci web, ponieważ ma ono zwiększyć ich. Jeśli serwer może osiągnąć większą przepustowość, nie należy więc jako wiele je i użytkownik może spowodować spadek kosztów, ponieważ wymagają mniejszej liczby serwerów w woluminie danego ruchu niż inaczej w przypadku.

Obsługa modelu programowania asynchronicznego .NET 4.5 jest zawarte w ASP.NET 4.5 dla interfejsów API sieci Web; MVC i formularzy sieci Web w programie Entity Framework 6, a w [interfejsu API systemu Windows Azure Storage](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Obsługa asynchronicznego w programie ASP.NET 4.5

W programie ASP.NET 4.5 obsługę programowania asynchronicznego został dodany, nie tylko język, ale również do struktur interfejsu API sieci Web MVC i formularzy sieci Web. Na przykład metoda akcji kontrolera ASP.NET MVC odbiera dane z żądania sieci web i przekazuje dane do widoku, który następnie tworzy HTML do wysłania do przeglądarki. Często metody akcji musi pobrać danych z bazy danych, usługi sieci web, aby wyświetlić je na stronie sieci web lub zapisać dane wprowadzone na stronie sieci web. W tych scenariuszach jest łatwo wprowadzać asynchronicznej metody akcji: zamiast zwracać *ActionResult* obiekt powrócisz *zadań&lt;ActionResult&gt;*  i oznacz metodę z *async* — słowo kluczowe. Wewnątrz metody gdy wiersz kodu dotyczącego operacji, która obejmuje czas oczekiwania, możesz oznaczyć go — słowo kluczowe await.

Poniżej przedstawiono metodę akcji proste, która wywołuje metodę repozytorium dla zapytania bazy danych:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

A Oto sama metoda, która obsługuje asynchronicznie wywołania bazy danych:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

W obszarze obejmuje kompilator generuje odpowiedni kod asynchronicznego. Gdy aplikacja sprawia, że wywołanie `FindTaskByIdAsync`, sprawia, że program ASP.NET `FindTask` żądania, a następnie cofa wątku roboczego i udostępnia przetwarzać innego żądania. Gdy `FindTask` żądania jest wykonywane, wątek jest uruchomiony ponownie w celu kontynuować przetwarzanie kodu po tym wywołaniu. W okresie przejściowym między `FindTask` żądanie jest inicjowane i gdy dane są zwracane, masz wątku dostępne przydatne pracy, które w przeciwnym wypadku będą powiązane oczekiwania na odpowiedź.

Brak niektórych obciążenie kod asynchroniczny, ale w warunkach niskiego obciążenia, że obciążenie jest niewielka, podczas warunkach duże obciążenie, jaką może przetworzyć żądania, które będą przechowywać oczekiwania na dostępnych wątków.

Było możliwe przeprowadzenie tego rodzaju programowanie asynchroniczne od platformy ASP.NET w wersji 1.1, ale jest trudne do zapisu, podatnych i trudne do debugowania. Teraz Uprościliśmy kodowania go w programie ASP.NET 4.5 nie ma nie trzeba już to zrobić.

### <a name="async-support-in-entity-framework-6"></a>Obsługa asynchronicznego w programie Entity Framework 6

W ramach obsługi asynchronicznych w 4.5 możemy wysłane obsługę asynchronicznego wywołania usługi sieci web, sockets i system plików we/wy, ale najczęściej wzorca dla aplikacji sieci web jest trafienie bazy danych i bibliotek naszych danych nie obsługuje asynchroniczne. Teraz Entity Framework 6 dodaje obsługę asynchronicznego dostępu do bazy danych.

W programie Entity Framework 6 wszystkie metody, które powodują kwerendy lub polecenia do wysłania do bazy danych ma wersje asynchroniczne. W tym przykładzie wyświetlana jest wersja async *znaleźć* metody.

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

Ta obsługa async działa nie tylko do wstawienia, usunięcia, aktualizacji i proste znajduje, współdziała również z zapytań LINQ:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Brak `Async` wersji `ToList` — metoda, ponieważ w tym kodzie to metodę, która powoduje, że zapytanie w celu wysłania do bazy danych. `Where` i `OrderByDescending` metody tylko skonfiguruj zapytanie, podczas `ToListAsync` metoda wykonuje zapytania i zapisuje odpowiedź w `result` zmiennej.

## <a name="summary"></a>Podsumowanie

Można zaimplementować web development najlepsze rozwiązania opisane w tym miejscu w dowolnej sieci web programowania framework oraz innych środowiskach chmury, ale mamy narzędzi w ASP.NET i Windows Azure, aby ułatwić. Jeśli wykonujesz te wzorce, można łatwo skalować w poziomie warstwę web i będzie można zminimalizować wydatków, ponieważ każdy serwer będzie mogła obsługiwać ruch więcej.

[Następnego rozdziału](single-sign-on.md) analizuje jak chmury włącza scenariusze rejestracji jednokrotnej.

## <a name="resources"></a>Resources

Aby uzyskać więcej informacji, zobacz następujące zasoby.

Serwery sieci web bezstanowe:

- [Microsoft Patterns and Practices — wskazówki dotyczące Skalowanie automatyczne](https://msdn.microsoft.com/library/dn589774.aspx).
- [Wyłączanie ARR wystąpienia koligacji w Windows Azure Web Sites](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Wpis w blogu przez Erez Benari wyjaśniono koligacji sesji w systemie Windows Azure Web Sites.

CDN:

- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalności, odporności](https://channel9.msdn.com/Series/FailSafe). Seria filmów dziewięć części Ulrich Homann, Mercuri wytłoków i moduły SIMM znaku. Zawiera omówienie sieci CDN w epizodu 3, zaczynając od 1:34:00.
- [Wzorzec Microsoft Patterns i rozwiązań statycznej zawartości hostingu](https://msdn.microsoft.com/library/dn589776.aspx)
- [Przeglądy CDN](http://www.cdnreviews.com/). Omówienie CDN wiele.

Programowanie asynchroniczne:

- [Używanie metod asynchronicznych na platformie ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Samouczek autorstwa Ricka Andersona.
- [Programowanie asynchroniczne z Async i Await (C# i Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). MSDN oficjalny dokument, który objaśnia uzasadnienie programowanie asynchroniczne, jak to działa w programie ASP.NET 4.5 i jak napisać kod, aby ją wdrożyć.
- [Entity Framework asynchronicznych zapytań i Zapisz](https://msdn.microsoft.com/data/jj819165)
- [Jak tworzyć aplikacje sieci Web ASP.NET przy użyciu asynchronicznej](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Wideo prezentacji przez Tomaszewski Rowan. Zawiera graficzne pokaz programowania asynchronicznego w jaki sposób można ułatwić znaczący wzrost przepływność serwera sieci web w warunkach dużego obciążenia.
- [Przed uszkodzeniami: Tworzenie usługi w chmurze skalowalności, odporności](https://channel9.msdn.com/Series/FailSafe). Seria filmów dziewięć części Ulrich Homann, Mercuri wytłoków i moduły SIMM znaku. Do dyskusji dotyczących wpływu programowania asynchronicznego na skalowalność Zobacz epizodu 4 i epizodu 8.
- [Magiczna użycia metod asynchronicznych w ASP.NET 4.5, a także ważne gotcha](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Wpis w blogu w którym Scott Hanselman o przede wszystkim za pomocą asynchroniczne w aplikacji formularzy sieci Web programu ASP.NET.

Dodatkowe sieci web development najlepsze rozwiązania można znaleźć w następujących zasobach:

- [Poprawka go Przykładowa aplikacja — najlepsze praktyki](the-fix-it-sample-application.md#bestpractices). Dodatek do Książka elektroniczna zamieszczono listę najlepszych rozwiązań, które zostały wdrożone w aplikacji rozwiązać.
- [Lista kontrolna deweloperów sieci Web](http://webdevchecklist.com/asp.net)

>[!div class="step-by-step"]
[Poprzednie](continuous-integration-and-continuous-delivery.md)
[dalej](single-sign-on.md)
