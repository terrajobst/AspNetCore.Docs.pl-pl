---
uid: web-api/overview/advanced/dependency-injection
title: "Iniekcji zależności w składniku ASP.NET Web API 2 | Dokumentacja firmy Microsoft"
author: MikeWasson
description: "W tym samouczku przedstawiono sposób wstrzyknięcia zależności w kontrolerze interfejsu API sieci Web platformy ASP.NET. Wersje oprogramowania używany w samouczek zablokowanych witryn sieci Web API 2 platformy Unity aplikacji..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: b4cf39c59ed257b0014dbdbecef3eb7bc48f410d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-aspnet-web-api-2"></a>Iniekcji zależności w składniku ASP.NET Web API 2
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> W tym samouczku przedstawiono sposób wstrzyknięcia zależności w kontrolerze interfejsu API sieci Web platformy ASP.NET.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Składnik Web API 2
> - [Blokowanie aplikacji Unity](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (działa także w wersji 5)


## <a name="what-is-dependency-injection"></a>Co to jest iniekcji zależności?

A *zależności* jest dowolny obiekt, który wymaga innego obiektu. Na przykład jest często do definiowania [repozytorium](http://martinfowler.com/eaaCatalog/repository.html) obsługująca dostęp do danych. Załóżmy przedstawiono przykład. Po pierwsze zdefiniujemy modelu domeny:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Oto prosty repozytorium klasę, która przechowuje elementy w bazie danych przy użyciu programu Entity Framework.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Teraz zdefiniujmy kontrolera interfejsu API sieci Web, która obsługuje żądania GET `Product` jednostek. (I używam pomijając POST i innych metod dla uproszczenia.) W tym miejscu jest pierwsza próba:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Należy zauważyć, że klasa kontrolera jest zależny od `ProductRepository`, możemy są co kontroler, Utwórz `ProductRepository` wystąpienia. Jednak jest dobrym pomysłem twardych kodu zależności w ten sposób z kilku powodów.

- Jeśli chcesz zamienić `ProductRepository` z implementacją innego, należy również zmodyfikować klasy kontrolera.
- Jeśli `ProductRepository` ma zależności, należy skonfigurować wewnątrz kontrolera. Dla dużych projektów z wielu kontrolerów kodu konfiguracji staje się znajdują się na projekt.
- Trudno jest testu jednostkowego, ponieważ kontroler jest ustalony na zapytanie bazy danych. Dla testu jednostkowego należy użyć makiety lub stub repozytorium, co nie jest możliwe w projekcie currect.

Można rozwiązać te problemy przez *wstrzyknięcie* repozytorium do kontrolera. Po pierwsze, Refaktoryzuj `ProductRepository` klasy w interfejsie:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Następnie podaj `IProductRepository` jako parametru konstruktora:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

W tym przykładzie użyto [iniekcji konstruktora](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Można również użyć *iniekcji metody ustawiającej*, których wartość zależności za pomocą metody ustawiającej lub właściwości.

Ale teraz występuje problem, ponieważ aplikacja nie bezpośrednio tworzy kontroler. Interfejs API sieci Web tworzy kontroler, gdy kieruje żądanie i interfejsu API sieci Web nie ma żadnych informacji dotyczących `IProductRepository`. Jest to, gdzie mechanizmu rozpoznawania zależności interfejsu API sieci Web jest dostarczany.

## <a name="the-web-api-dependency-resolver"></a>Mechanizm rozpoznawania zależności interfejsu API sieci Web

Definiuje interfejs API sieci Web **elementu IDependencyResolver** interfejs do rozpoznawania zależności. W tym miejscu znajduje się definicja interfejsu:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

**IDependencyScope** interfejs ma dwóch metod:

- **Funkcja GetService** tworzy jedno wystąpienie typu.
- **Metodę GetServices** tworzy kolekcję obiektów określonego typu.

**Elementu IDependencyResolver** dziedziczy metody **IDependencyScope** i dodaje **BeginScope** metody. Będzie porozmawiać o zakresach w dalszej części tego samouczka.

Gdy interfejs API sieci Web tworzy wystąpienie kontrolera, najpierw wywołuje **IDependencyResolver.GetService**, przekazując typ kontrolera. Tego punktu zaczepienia rozszerzalności umożliwia utworzenie kontrolera, rozpoznawania zależności. Jeśli **GetService** zwraca wartość null, interfejsu API sieci Web szuka konstruktora dla klasy kontrolera.

## <a name="dependency-resolution-with-the-unity-container"></a>Rozpoznawanie zależności z kontenerem Unity

Mimo że można zapisać pełnego **elementu IDependencyResolver** implementacji od początku, interfejs naprawdę jest przeznaczony do działania jako mostka między interfejsu API sieci Web i istniejące kontenery Inwersja kontroli.

Kontenera IoC to składnik oprogramowania, który jest odpowiedzialny za zarządzanie zależności. Zarejestrować typy z kontenerem, a następnie użyć do tworzenia obiektów kontenera. Kontener danych liczbowych automatycznie limit relacji zależności. Również wiele kontenerów Inwersja kontroli pozwala na kontrolowanie okres istnienia obiektów i zakresu.

> [!NOTE]
> "IoC" oznacza "Inwersja kontroli", która jest wzorzec ogólne gdzie to struktura wywołuje kod aplikacji. Kontenera IoC tworzy obiekty, które zwykle przepływu sterowania "odwraca".


W tym samouczku, użyjemy [Unity](https://msdn.microsoft.com/en-us/library/ff647202.aspx) z Microsoft Patterns &amp; rozwiązania. (Obejmują innych popularnych bibliotek [Windsor zamku](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), i [StructureMap ](http://docs.structuremap.net/).) Menedżer pakietów NuGet służy do instalowania Unity. Z **narzędzia** menu w programie Visual Studio, wybierz **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wpisz następujące polecenie:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

W tym miejscu jest implementacją **elementu IDependencyResolver** który opakowuje kontenera Unity.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Jeśli **GetService** metody nie można rozpoznać typu, powinny zwrócić **null**. Jeśli **metodę GetServices** metody nie można rozpoznać typu, powinien zostać zwrócony obiekt pustej kolekcji. Nie zgłaszają wyjątki dla nieznanych typów.


## <a name="configuring-the-dependency-resolver"></a>Konfigurowanie mechanizmu rozpoznawania zależności

Ustaw mechanizmu rozpoznawania zależności na **klasy DependencyResolver** właściwości globalne **HttpConfiguration** obiektu.

Poniższy kod rejestruje `IProductRepository` interfejsu z Unity, a następnie tworzy `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Zakres zależności i okresem istnienia kontrolera

Kontrolery są tworzone na żądanie. Aby zarządzać okresy istnienia obiektu, **elementu IDependencyResolver** korzysta z koncepcji *zakres*.

Mechanizm rozpoznawania zależności dołączony do **HttpConfiguration** obiekt ma zasięg globalny. Gdy interfejs API sieci Web tworzy kontrolera, wywołuje **BeginScope**. Ta metoda zwraca **IDependencyScope** reprezentujący zakresie podrzędnym.

Następnie wywołuje interfejs API sieci Web **GetService** w zakresie podrzędnym, aby utworzyć kontroler. Po zakończeniu żądania wywołania interfejsu API sieci Web **Dispose** w zakresie podrzędnym. Użyj **Dispose** metody zlikwidować zależności kontrolera.

Jak zaimplementować **BeginScope** zależy kontenera IoC. W przypadku Unity zakres odpowiada kontenera podrzędnego:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

Większość kontenerów Inwersja kontroli mają podobne odpowiedniki.
