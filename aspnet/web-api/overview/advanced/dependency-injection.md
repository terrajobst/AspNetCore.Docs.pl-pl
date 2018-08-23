---
uid: web-api/overview/advanced/dependency-injection
title: Wstrzykiwanie zależności we wzorcu ASP.NET Web API 2 | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym samouczku pokazano, jak wstrzykiwanie zależności do poziomu kontrolera interfejsu API sieci Web platformy ASP.NET. Wersje oprogramowania używanych w samouczek zablokowanych witryn sieci Web API 2 Unity aplikacji...
ms.author: riande
ms.date: 01/20/2014
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 41db1af79ed63ff4dd12be37e9cc76e16f1bf5e4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755460"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a>Wstrzykiwanie zależności we wzorcu ASP.NET Web API 2
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> W tym samouczku pokazano, jak wstrzykiwanie zależności do poziomu kontrolera interfejsu API sieci Web platformy ASP.NET.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - Składnik Web API 2
> - [Blok aplikacji Unity](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (działa także w wersji 5)


## <a name="what-is-dependency-injection"></a>Co to jest wstrzykiwanie zależności?

A *zależności* jest dowolny obiekt, który wymaga innego obiektu. Na przykład, jest często zdefiniować [repozytorium](http://martinfowler.com/eaaCatalog/repository.html) obsługująca dostęp do danych. Teraz pokazują z przykładem. Po pierwsze zdefiniujemy modelu domeny:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

W tym miejscu jest klasą prostego repozytorium, która przechowuje elementy w bazie danych przy użyciu platformy Entity Framework.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Teraz czynnością jest zdefiniowanie kontroler Web API, który obsługuje żądania GET `Product` jednostek. (Czy mam pomijając POST i innych metod dla uproszczenia.) W tym miejscu jest pierwsza próba:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Należy zauważyć, że klasa kontrolera jest zależna od `ProductRepository`, i zawiadamiamy kontroler tworzenia `ProductRepository` wystąpienia. Jednak jest to zły pomysł kodowi twardych zależności w ten sposób z kilku powodów.

- Jeśli chcesz zastąpić `ProductRepository` z inną implementacją, trzeba będzie również zmodyfikować klasy kontrolera.
- Jeśli `ProductRepository` ma zależności, należy skonfigurować w kontrolerze. Dla dużego projektu za pomocą wielu kontrolerów kodu konfiguracji staje się rozproszone w projekcie.
- Ciężko jest testy jednostkowe, ponieważ kontroler jest ustalony do wykonywania zapytań w bazie danych. Dla testu jednostkowego należy użyć pozorny ani klas zastępczych repozytorium, które nie jest możliwe z projektem currect.

Można rozwiązać te problemy, zapewniając *wprowadza* repozytorium do kontrolera. Po pierwsze, Refaktoryzuj `ProductRepository` klasy w interfejsie:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Następnie podaj `IProductRepository` jako parametr konstruktora:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

W tym przykładzie użyto [iniekcji konstruktora](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Można również użyć *iniekcji metody ustawiającej*, gdzie ustawiasz zależności za pomocą metody ustawiającej lub właściwości.

Ale teraz występuje problem, ponieważ aplikacja nie tworzy kontroler bezpośrednio. Interfejs API sieci Web tworzy kontroler, kieruje żądanie, gdy interfejs API sieci Web nie nic wiedzieć o `IProductRepository`. Jest to, gdzie mechanizmu rozpoznawania zależności interfejsu API sieci Web jest dostępna w.

## <a name="the-web-api-dependency-resolver"></a>Mechanizm rozpoznawania zależności interfejsu API sieci Web

Definiuje interfejs API sieci Web **elementu IDependencyResolver** interfejs służący do rozpoznawania zależności. Oto definicja interfejsu:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

**IDependencyScope** interfejs ma dwie metody:

- **GetService** tworzy jedno wystąpienie typu.
- **Funkcji GetServices** tworzy kolekcję obiektów określonego typu.

**Elementu IDependencyResolver** dziedziczy metodę **IDependencyScope** i dodaje **BeginScope** metody. Omówię zakresów w dalszej części tego samouczka.

Kiedy internetowy interfejs API tworzy wystąpienie kontrolera, najpierw wywołuje **IDependencyResolver.GetService**, przekazując typ kontrolera. Tego punktu zaczepienia rozszerzalności umożliwia tworzenie kontrolera, rozwiązywanie wszelkich zależności. Jeśli **GetService** zwraca wartość null, internetowy interfejs API szuka konstruktorem klasy kontrolera.

## <a name="dependency-resolution-with-the-unity-container"></a>Rozpoznawanie zależności za pomocą kontenera aparatu Unity

Mimo że można napisać kompletna **elementu IDependencyResolver** wdrożenia od zera, interfejs naprawdę jest przeznaczony do działania jako Most między interfejsu API sieci Web i istniejące kontenery IoC.

Kontenera IoC jest składnikiem oprogramowania, który jest odpowiedzialny za zarządzanie zależnościami. Rejestrowanie typów z kontenerem, a następnie użyć do tworzenia obiektów kontenera. Kontener automatycznie wpadł na pomysł relacji zależności. Wiele kontenerów IoC umożliwiają także kontrolowanie elementów, takich jak okres istnienia obiektów i zakresu.

> [!NOTE]
> "IoC" oznacza "Inwersja kontroli", który jest ogólny wzorzec gdzie struktura wywołuje kod aplikacji. Kontenera IoC tworzy obiekty, które zwykle przepływu sterowania "odwraca".


W tym samouczku użyjemy [Unity](https://msdn.microsoft.com/library/ff647202.aspx) z Microsoft Patterns &amp; rozwiązania. (Obejmują innych popularnych bibliotek [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), i [StructureMap ](http://docs.structuremap.net/).) Menedżer pakietów NuGet można użyć do zainstalowania aparatu Unity. Z **narzędzia** menu w programie Visual Studio, wybierz **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wpisz następujące polecenie:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Oto implementacja **elementu IDependencyResolver** wszystko w kontenerze aparatu Unity.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Jeśli **GetService** metody nie można rozpoznać typu, powinna zwrócić **null**. Jeśli **funkcji GetServices** metody nie można rozpoznać typu, powinien zostać zwrócony obiekt pustą kolekcję. Nie zgłaszają wyjątki dla nieznanego typu.


## <a name="configuring-the-dependency-resolver"></a>Konfigurowanie mechanizmu rozpoznawania zależności

Nastavit mechanizmu rozpoznawania zależności **klasy DependencyResolver** właściwości globalnego **HttpConfiguration** obiektu.

Poniższy kod rejestruje `IProductRepository` interfejs za pomocą aparatu Unity, a następnie tworzy `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Zakres zależności i okresem istnienia kontrolera

Kontrolery są tworzone na żądanie. Do zarządzania okresy istnienia obiektu, **elementu IDependencyResolver** korzysta z koncepcji *zakres*.

Mechanizm rozpoznawania zależności dołączone do **HttpConfiguration** obiekt ma zakres globalny. Kiedy internetowy interfejs API tworzy kontroler, wywołuje **BeginScope**. Ta metoda zwraca **IDependencyScope** reprezentujący zakresem podrzędnym.

Następnie wywołuje internetowy interfejs API **GetService** w zakresie podrzędnym, aby utworzyć kontroler. Po zakończeniu żądania wywołania interfejsu API sieci Web **Dispose** w zakresie podrzędnym. Użyj **Dispose** metodę dispose zależności kontrolera.

Jak zaimplementować **BeginScope** zależy od kontenera IoC. Zakres dla aparatu Unity, odnosi się do kontenerów podrzędnych:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

Większość kontenery IoC mają podobne odpowiedniki.
