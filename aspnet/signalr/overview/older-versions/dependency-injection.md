---
uid: signalr/overview/older-versions/dependency-injection
title: Iniekcji zależności w bibliotece SignalR 1.x | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/15/2013
ms.topic: article
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 6fd155adc9a0aa71b66db7a51062a51fb0c1feca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
ms.locfileid: "26565952"
---
<a name="dependency-injection-in-signalr-1x"></a>Iniekcji zależności w bibliotece SignalR 1.x
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

Iniekcji zależności jest sposobem usunięcia ustalonych zależności między obiektami, ułatwiając użytkownikom, aby zastąpić obiekt zależności, albo do testowania (przy użyciu obiektów zasymulować) lub zmienić zachowania w czasie wykonywania. W tym samouczku przedstawiono sposób wykonywania iniekcji zależności na koncentratorów SignalR. Ponadto sposobu używania kontenery Inwersja kontroli z SignalR. Kontener Inwersja kontroli jest ogólnych ram iniekcji zależności.

## <a name="what-is-dependency-injection"></a>Co to jest iniekcji zależności?

Pomiń tę sekcję, jeśli już znasz iniekcji zależności.

*Iniekcji zależności* (Podpisane) to wzorzec, gdy nie są odpowiedzialne za tworzenie własnych zależności obiektów. Poniżej przedstawiono prosty przykład do Motywuj Podpisane. Załóżmy, że został wybrany obiekt, który musi komunikaty dziennika. Można zdefiniować interfejsu rejestrowania:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Obiekt, umożliwia tworzenie `ILogger` rejestrować komunikatów:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

To działa, ale nie jest najlepszym projektu. Jeśli chcesz zamienić `FileLogger` z inną `ILogger` implementacji, należy zmodyfikować `SomeComponent`. Przypuszczenia, że wiele innych obiektów użyj `FileLogger`, musisz zmienić ich wszystkich. Lub jeśli użytkownik chce utworzyć `FileLogger` pojedynczą, również należy wprowadzić zmiany w całej aplikacji.

Lepszym rozwiązaniem jest "wstrzyknąć" `ILogger` do obiektu — na przykład za pomocą argumentu konstruktora:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Teraz obiektu nie odpowiada wybierania który `ILogger` do użycia. Możesz swich `ILogger` implementacje bez zmieniania obiektów, które od niego zależne.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Ten wzorzec jest nazywany [iniekcji konstruktora](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Inny wzorzec jest iniekcji setter, których wartość zależności za pomocą metody ustawiającej lub właściwości.

## <a name="simple-dependency-injection-in-signalr"></a>Iniekcji zależności proste w SignalR

Należy wziąć pod uwagę aplikacji czatu z samouczka [wprowadzenie SignalR](../getting-started/tutorial-getting-started-with-signalr.md). Oto klasy koncentratora od tej aplikacji:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Załóżmy, że chcesz przechowywanie wiadomości rozmów na serwerze przed ich wysłaniem. Może zdefiniować interfejs, który abstracts tę funkcję i użyć Podpisane iniekcję interfejs do `ChatHub` klasy.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Problem polega na to, że aplikacji SignalR nie tworzy bezpośrednio koncentratory; SignalR utworzy je. Domyślnie SignalR oczekuje ma bezparametrowego konstruktora klasy koncentratora. Jednak użytkownik może łatwo rejestrować funkcję do tworzenia wystąpień koncentratorów i ta funkcja służy do wykonywania Podpisane. Zarejestruj przez wywołanie funkcji **GlobalHost.DependencyResolver.Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Teraz SignalR wywoła tej funkcji anonimowej zawsze, gdy trzeba utworzyć `ChatHub` wystąpienia.

## <a name="ioc-containers"></a>Kontenery Inwersja kontroli

Poprzedni kod wystarcza do prostych przypadkach. Jednak nadal musiały zapisu to:

[!code-css[Main](dependency-injection/samples/sample8.css)]

W złożonych aplikacji z wiele zależności konieczne może być napisać dużą ten kod "okablowania". Ten kod może być trudno będzie utrzymać, zwłaszcza, jeśli są zagnieżdżone zależności. Również jest trudna do testu jednostkowego.

Jedynym rozwiązaniem jest umieszczenie w kontenerze Inwersja kontroli. Kontenera IoC to składnik oprogramowania, który jest odpowiedzialny za zarządzanie zależności. Zarejestrować typy z kontenerem, a następnie użyć do tworzenia obiektów kontenera. Kontener danych liczbowych automatycznie limit relacji zależności. Również wiele kontenerów Inwersja kontroli pozwala na kontrolowanie okres istnienia obiektów i zakresu.

> [!NOTE]
> "IoC" oznacza "Inwersja kontroli", która jest wzorzec ogólne gdzie to struktura wywołuje kod aplikacji. Kontenera IoC tworzy obiekty, które zwykle przepływu sterowania "odwraca".


## <a name="using-ioc-containers-in-signalr"></a>Używanie kontenerów Inwersja kontroli w SignalR

Prawdopodobnie zbyt proste do korzystania z kontenera IoC aplikacji czatu. Zamiast tego, Przyjrzyjmy się [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) próbki.

Przykładowe StockTicker definiuje dwie klasy głównym:

- `StockTickerHub`: Centrum klasy, która zarządza połączeń klientów.
- `StockTicker`: Jako pojedyncza, która zawiera giełdowych i okresowo aktualizuje je.

`StockTickerHub`zawiera odwołanie do `StockTicker` jako pojedynczej, podczas gdy `StockTicker` zawiera odwołanie do **IHubConnectionContext** dla `StockTickerHub`. Ten interfejs używa do komunikacji z `StockTickerHub` wystąpień. (Aby uzyskać więcej informacji, zobacz [serwera emisji z ASP.NET SignalR](index.md).)

Możemy użyć kontenera IoC do nieco untangle te zależności. Po pierwsze odrobinę uprościmy badane `StockTickerHub` i `StockTicker` klasy. W poniższym kodzie I zostały oznaczone jako komentarz części firma Microsoft nie wymagają.

Usuń konstruktora bez parametrów z `StockTicker`. Zamiast tego należy zawsze używamy Podpisane utworzyć koncentratora.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

W przypadku StockTicker usunąć pojedyncze wystąpienie. Później użyjemy kontenera IoC do sterowania StockTicker przez czas ich istnienia. Sprawdź także, konstruktora publicznego.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

Następnie możemy zrefaktoryzuj kod przez utworzenie interfejsu `StockTicker`. Użyjemy tego interfejsu rozdzielenie `StockTickerHub` z `StockTicker` klasy.

Visual Studio sprawia, że to rodzaj refaktoryzacji łatwe. Otwórz plik StockTicker.cs, kliknij prawym przyciskiem myszy `StockTicker` deklaracji klasy, a następnie wybierz **Refaktoryzuj** ... **Wyodrębnij Interface**.

![](dependency-injection/_static/image1.png)

W **wyodrębniania interfejsu** okna dialogowego, kliknij przycisk **Zaznacz wszystko**. Pozostaw wartości domyślne. Kliknij przycisk **OK**.

![](dependency-injection/_static/image2.png)

Program Visual Studio tworzy nowy interfejs o nazwie `IStockTicker`, a także zmianę `StockTicker` pochodzić z `IStockTicker`.

Otwórz plik IStockTicker.cs i zmienić interfejsu **publicznego**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

W `StockTickerHub` klasy, zmień dwa wystąpienia `StockTicker` do `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

Tworzenie `IStockTicker` interfejsu nie jest to niezbędne, ale miała pokazanie, jak Podpisane może pomóc zmniejszyć sprzężenia między składnikami aplikacji.

## <a name="add-the-ninject-library"></a>Dodaj bibliotekę Ninject

Istnieje wiele kontenerów Inwersja kontroli open source dla platformy .NET. W tym samouczku będziesz używać [Ninject](http://www.ninject.org/). (Obejmują innych popularnych bibliotek [Windsor zamku](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), i [StructureMap ](http://docs.structuremap.net).)

Użyj Menedżera pakietów NuGet do zainstalowania [Ninject biblioteki](https://nuget.org/packages/Ninject/3.0.1.10). W programie Visual Studio z **narzędzia** menu wybierz opcję **Menedżer pakietów biblioteki** | **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Zastąp mechanizmu rozpoznawania zależności biblioteki SignalR

Aby użyć Ninject w SignalR, Utwórz klasę, która jest pochodną **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Ta klasa zastępuje **GetService** i **metodę GetServices** metody **DefaultDependencyResolver**. SignalR wywołania tych metod, aby utworzyć różnych obiektów w czasie wykonywania, w tym wystąpień koncentratorów, a także różne usługi używana wewnętrznie przez SignalR.

- **GetService** metoda tworzy pojedynczego wystąpienia typu. Przesłonić tę metodę do wywołania jądra Ninject **TryGet** metody. Jeśli ta metoda zwraca wartość null, wrócić do domyślny program rozpoznawania nazw.
- **Metodę GetServices** metoda tworzy kolekcję obiektów określonego typu. Zastępuje tę metodę do łączenia wyników z Ninject z wyników z mechanizmem rozpoznawania.

## <a name="configure-ninject-bindings"></a>Skonfiguruj powiązania Ninject

Teraz użyjemy Ninject Aby zadeklarować typ powiązania.

Otwórz plik RegisterHubs.cs. W `RegisterHubs.Start` metody tworzenia kontenera Ninject, który wywołuje Ninject *jądra*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Utwórz wystąpienie naszych mechanizmu rozpoznawania zależności niestandardowych:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Utwórz powiązanie dla `IStockTicker` w następujący sposób:

[!code-html[Main](dependency-injection/samples/sample17.html)]

Ten kod jest informacją dwie czynności. Pierwsza strona, gdy aplikacja musi `IStockTicker`, jądra należy utworzyć wystąpienia `StockTicker`. Drugi, `StockTicker` klasy powinny być utworzone jako pojedynczego obiektu. Ninject utworzyć jedno wystąpienie obiektu i zwraca to samo wystąpienie dla każdego żądania.

Utwórz powiązanie dla **IHubConnectionContext** w następujący sposób:

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Ten kod creatres funkcji anonimowej, która zwraca **IHubConnection**. **WhenInjectedInto** metody informuje Ninject, aby użyć tej funkcji tylko podczas tworzenia `IStockTicker` wystąpień. Przyczyną jest to, że tworzy SignalR **IHubConnectionContext** wewnętrznie, wystąpienia i nie chcemy zastąpić, jak SignalR tworzy je. Ta funkcja ma zastosowanie tylko do naszej `StockTicker` klasy.

Przekaż do mechanizmu rozpoznawania zależności **MapHubs** metody:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Teraz SignalR będzie używać programu rozpoznawania nazw, określonych w **MapHubs**, zamiast domyślny program rozpoznawania nazw.

W tym miejscu jest kompletny kod dla `RegisterHubs.Start`.

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

Aby uruchomić aplikację StockTicker w programie Visual Studio, naciśnij klawisz F5. W oknie przeglądarki przejdź do `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

Aplikacja ma dokładnie tak samo jak przed. (Aby uzyskać opis, zobacz [serwera emisji z ASP.NET SignalR](index.md).) Firma Microsoft nie zostały zmienione zachowanie; po prostu łatwiejsza kod do testowania, obsługa i rozwijać.
