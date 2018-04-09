---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
title: Buforowanie danych w architekturze (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W poprzednim samouczku opisano sposób stosowania buforowanie w warstwie prezentacji. W tym samouczku będziemy Dowiedz się, jak skorzystać z naszych warstwowego architectu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 5e189dd7-f4f9-4f28-9b3a-6cb7d392e9c7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
msc.type: authoredcontent
ms.openlocfilehash: 08f83c129d589859723249becb818386bfff19bf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="caching-data-in-the-architecture-vb"></a>Buforowanie danych w architekturze (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_VB.exe) lub [pobierania plików PDF](caching-data-in-the-architecture-vb/_static/datatutorial59vb1.pdf)

> W poprzednim samouczku opisano sposób stosowania buforowanie w warstwie prezentacji. W tym samouczku będziemy Dowiedz się, jak skorzystać z naszych warstwowa architektura do pamięci podręcznej danych w warstwie logiki biznesowej. Firma Microsoft to zrobić przez rozszerzanie architektury do dołączenia do buforowania warstwy.


## <a name="introduction"></a>Wprowadzenie

Jak widzieliśmy w poprzednim samouczek buforowanie danych s ObjectDataSource jest tak proste, jak ustawienie kilka właściwości. Niestety ObjectDataSource stosuje buforowanie w warstwie prezentacji, ściśle składającą zasad buforowania z strony ASP.NET. Jednym z powodów tworzenia warstwowa architektura jest umożliwienie takie sprzężenia uszkodzenie. Warstwy logiki biznesowej, na przykład oddziela logiki biznesowej ze stron ASP.NET podczas Warstwa dostępu do danych oddziela szczegółów dostępu do danych. To oddzielenie szczegóły dostępu logikę i dane biznesowe jest preferowana, w części, ponieważ sprawia, że system był bardziej czytelny, więcej utrzymaniu i bardziej elastyczne, aby zmienić. Umożliwia także dla domeny wiedzy i podziału pracy Deweloper pracujący nad warstwę prezentacji t muszą być zapoznać się ze szczegółami s bazy danych, aby wykonać swoje zadania. Rozdzielenie zasad buforowania z warstwy prezentacji oferuje podobne korzyści.

W tym samouczku będziemy rozszerzać Nasza architektura, aby uwzględnić *buforowanie warstwy* (lub CL skrócie) która jest stosowana w naszym zasad buforowania. Buforowanie warstwa będzie zawierać `ProductsCL` klasy, która zapewnia dostęp do informacji o produkcie z metody, takie jak `GetProducts()`, `GetProductsByCategoryID(categoryID)`, itd., gdy została wywołana, będzie pierwsza próba pobrania danych z pamięci podręcznej. Jeśli pamięć podręczna jest pusta, te metody wywoła odpowiednie `ProductsBLL` metoda logiki warstwy Biznesowej, które z kolei może pobrać danych z warstwy DAL. `ProductsCL` Metody buforować dane pobrane z logiki warstwy Biznesowej przed zwróceniem.

Jak pokazano na rysunku 1, CL znajduje się między prezentacji i warstwy logiki biznesowej.


![Warstwa buforowania (CL) jest kolejną warstwę nasze architektury](caching-data-in-the-architecture-vb/_static/image1.png)

**Rysunek 1**: buforowania warstwy (CL) jest kolejną warstwę nasze architektury


## <a name="step-1-creating-the-caching-layer-classes"></a>Krok 1: Tworzenie buforowania klas warstwy

W tym samouczku utworzymy bardzo proste CL z jednej klasy `ProductsCL` mający tylko kilku metod. Tworzenie pełną warstwy buforowanie dla całej aplikacji wymaga utworzenia `CategoriesCL`, `EmployeesCL`, i `SuppliersCL` klasy i udostępnieniu metod w tych klas warstwy buforowania dla każdego dostępu lub zmiana metody danych w logiki warstwy Biznesowej. Podobnie jak w przypadku logiki warstwy Biznesowej i warstwy DAL, buforowanie warstwy powinny być implementowane w idealnym przypadku jako osobne projektu biblioteki klas; jednak wprowadzimy go jako klasa w `App_Code` folderu.

Więcej bezpośrednio oddzielne CL klas z klas DAL i logiki warstwy Biznesowej, umożliwiają s Utwórz nowy podfolder w `App_Code` folderu. Kliknij prawym przyciskiem myszy `App_Code` folder w Eksploratorze rozwiązań wybierz nowy Folder i nadaj nazwę nowego folderu `CL`. Po utworzeniu tego folderu, Dodaj do niej nową klasę o nazwie `ProductsCL.vb`.


![Dodaj nowy Folder o nazwie CL i klasę o nazwie ProductsCL.vb](caching-data-in-the-architecture-vb/_static/image2.png)

**Rysunek 2**: Dodaj nowy Folder o nazwie `CL` i klasę o nazwie `ProductsCL.vb`


`ProductsCL` Klasy powinny zawierać ten sam zestaw metod dostępu i modyfikacji danych tak jak w odpowiedniej klasie warstwy logiki biznesowej (`ProductsBLL`). Zamiast tworzenia wszystkie te metody umożliwiają s tylko kompilacji kilka tutaj, aby uzyskać pewne pojęcie o wzorce używane przez CL. W szczególności dodamy `GetProducts()` i `GetProductsByCategoryID(categoryID)` metod w kroku 3 i `UpdateProduct` przeciążenia w kroku 4. Możesz dodać pozostałe `ProductsCL` metod i `CategoriesCL`, `EmployeesCL`, i `SuppliersCL` klas w wolnym czasie.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Krok 2: Odczytywanie i zapisywanie do pamięci podręcznej danych

ObjectDataSource przedstawione w powyższej samouczek wewnętrznie funkcji buforowania używa pamięci podręcznej programu ASP.NET danych do przechowywania danych pobranych z logiki warstwy Biznesowej. Pamięć podręczna danych również można uzyskać programistycznie z klasy związane z kodem stron ASP.NET lub z klas w architekturze s aplikacji sieci web. Do odczytu i zapisu w pamięci podręcznej danych z klasy związane z kodem s strony ASP.NET, użyj następującego wzorca:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample1.vb)]

[ `Cache` Klasy](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [ `Insert` metody](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) ma kilka przeciążeń. `Cache("key") = value` i `Cache.Insert(key, value)` to samo i zarówno dodania elementu do pamięci podręcznej przy użyciu określonego klucza bez zdefiniowanego wygaśnięcia. Zazwyczaj chcemy Określ wygaśnięcia podczas dodawania elementu do pamięci podręcznej, zarówno jako zależność i/lub na podstawie czasu wygaśnięcia. Użyj jednego z innych `Insert` przeciążenia metody s, aby podać informacje na podstawie zależności lub czasu wygaśnięcia.

Buforowanie warstwy s metody, należy najpierw sprawdź, czy żądane dane są w pamięci podręcznej, a jeśli tak, przywrócić go stamtąd. Jeśli żądanych danych nie jest w pamięci podręcznej, musi być wywoływane odpowiedniej metody logiki warstwy Biznesowej. Jego wartość zwrotna, należy w pamięci podręcznej i zwracany, jak pokazano na poniższym diagramie sekwencji.


![Metody s buforowanie warstwy zwracać dane z pamięci podręcznej, jeśli jego s dostępne](caching-data-in-the-architecture-vb/_static/image3.png)

**Rysunek 3**: warstwa buforowanie s metody zwracają dane z pamięci podręcznej, jeśli jego s dostępne


Sekwencja przedstawione na rysunku 3 odbywa się w klasach CL przy użyciu następującego wzorca:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample2.vb)]

W tym miejscu *typu* jest typu danych są przechowywane w pamięci podręcznej `Northwind.ProductsDataTable`, na przykład *klucza* jest klucz, który unikatowo identyfikuje element pamięci podręcznej. Jeśli element z określonym *klucza* nie znajduje się w pamięci podręcznej, następnie *wystąpienia* będzie `Nothing` i dane zostaną pobrane z odpowiedniej metody logiki warstwy Biznesowej i dodane do pamięci podręcznej. W czasie `Return instance` osiągnięciu *wystąpienia* zawiera odwołanie do danych z pamięci podręcznej lub pobierane z logiki warstwy Biznesowej.

Należy użyć powyżej wzorzec podczas uzyskiwania dostępu do danych z pamięci podręcznej. Następującego wzorca, który na pierwszy rzut oka wygląda równoważnej zawiera niewielka różnica wprowadzającej wyścigu. Warunki wyścigu są trudne do debugowania, ponieważ ujawnić się sporadycznie i trudnych do odtworzenia.


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample3.vb)]

Różnica w drugą fragment niepoprawny kod jest to, że zamiast odwołania do elementu pamięci podręcznej są przechowywane w zmiennej lokalnej, pamięć podręczna danych jest dostępny bezpośrednio w instrukcji warunkowej *i* w `Return`. Załóżmy, że po osiągnięciu tego kodu `Cache("key")` nie jest `Nothing`, ale przed wysłaniem `Return` osiągnięciu instrukcji, system wyklucza mogą *klucza* z pamięci podręcznej. W tym przypadku rzadkich zwróci kod `Nothing` zamiast oczekiwanego typu obiektu.

> [!NOTE]
> Pamięć podręczna danych jest bezpieczne wątkowo, dzięki czemu nie trzeba synchronizujący dostęp wątku dla prostego odczytów i zapisów. Jednak jeśli zachodzi potrzeba wykonania wielu operacji na danych w pamięci podręcznej, które muszą być atomic, jest odpowiedzialny za wdrażanie blokady lub innych mechanizmu w celu zapewnienia bezpieczeństwa wątków. Zobacz [synchronizowanie dostęp do pamięci podręcznej programu ASP.NET](http://www.ddj.com/184406369) Aby uzyskać więcej informacji.


Element może zostać wykluczony programowo z pamięci podręcznej danych przy użyciu [ `Remove` metody](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) w następujący sposób:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample4.vb)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Krok 3: Zwracanie informacji o produkcie z`ProductsCL`— klasa

Dla tego samouczka let s implementuje dwie metody w celu zwrócenia informacji o produkcie z `ProductsCL` klasy: `GetProducts()` i `GetProductsByCategoryID(categoryID)`. Jak `ProductsBL` klasy warstwy logiki biznesowej, `GetProducts()` metody w CL zwraca informacje o wszystkich produktów jako `Northwind.ProductsDataTable` obiektu, podczas `GetProductsByCategoryID(categoryID)` zwraca wszystkie produkty z określonej kategorii.

Poniższy kod przedstawia część metod w `ProductsCL` klasy:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample5.vb)]

Najpierw należy zanotować `DataObject` i `DataObjectMethodAttribute` atrybuty stosowane do klasy i metody. Te atrybuty zawierają informacje do kreatora s ObjectDataSource wskazującą, jakie klasy i metody powinny być wyświetlane w s kroki kreatora. Ponieważ CL klasy i metody będą mieli dostęp z ObjectDataSource w warstwie prezentacji, po dodaniu tych atrybutów, aby ulepszyć środowisko czasu projektowania. Odwołaj się do [Tworzenie warstwy logiki biznesowej](../introduction/creating-a-business-logic-layer-vb.md) samouczka bardziej szczegółowego opis tych atrybutów i ich wpływu.

W `GetProducts()` i `GetProductsByCategoryID(categoryID)` metody, dane zwrócone w wyniku `GetCacheItem(key)` metody jest przypisany do zmiennej lokalnej. `GetCacheItem(key)` Metodę, która zajmiemy się wkrótce, zwraca konkretny element z pamięci podręcznej oparte na określony *klucza*. Jeśli nie z tych danych zostanie znaleziony w pamięci podręcznej, są pobierane z odpowiadającego `ProductsBLL` metoda klasy, a następnie dodany do pamięci podręcznej przy użyciu `AddCacheItem(key, value)` metody.

`GetCacheItem(key)` i `AddCacheItem(key, value)` metody interfejsu odpowiednio z pamięci podręcznej danych, odczytywanie i zapisywanie wartości. `GetCacheItem(key)` Metoda jest prostsza dwóch. Po prostu zwraca wartość z klasy pamięci podręcznej, za pomocą przekazany do *klucza*:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample6.vb)]

`GetCacheItem(key)` nie używa *klucza* wartość jak podany, ale zamiast tego wywołania `GetCacheKey(key)` metody, która zwraca *klucza* poprzedzony przez ProductsCache —. `MasterCacheKeyArray`, Które zawiera ciąg ProductsCache, jest już używana przez `AddCacheItem(key, value)` metody, ponieważ zajmiemy się na chwilę.

Z kodem klasę strony ASP.NET pamięci podręcznej danych jest możliwy za pomocą `Page` klasy s [ `Cache` właściwości](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)i umożliwia składnię `Cache("key") = value`, zgodnie z opisem w kroku 2. Po klasie w architekturze pamięci podręcznej danych jest możliwy za pomocą `HttpRuntime.Cache` lub `HttpContext.Current.Cache`. [Peterowi Johnson](https://weblogs.asp.net/pjohnson/default.aspx)jego wpis w blogu [HttpRuntime.Cache vs. HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) uwagi dotyczące wydajności nieznaczne zaletą używania `HttpRuntime` zamiast `HttpContext.Current`; w rezultacie `ProductsCL` używa `HttpRuntime`.

> [!NOTE]
> Jeśli architektury jest implementowane za pomocą projektów biblioteki klas, musisz dodać odwołanie do `System.Web` zestawu, aby można było używać [ `HttpRuntime` ](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) i [ `HttpContext` ](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) klasy.


Jeśli element nie zostanie znaleziony w pamięci podręcznej, `ProductsCL` metod klasy s pobrać danych z logiki warstwy Biznesowej i dodaj go do pamięci podręcznej przy użyciu `AddCacheItem(key, value)` metody. Aby dodać *wartość* do pamięci podręcznej możemy użyć następujący kod, który używa upływie 60 sekund:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample7.vb)]

`DateTime.Now.AddSeconds(CacheDuration)` Określa okres ważności na podstawie czasu 60 sekund w przyszłości podczas [ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) wskazuje s, ponieważ nie wygasanie przewijania. Podczas to `Insert` przeciążenie metody ma wejściowych parametry dla obu bezwzględnym i przedłużanie ważności, możesz udostępniać jedno z nich. Jeśli spróbujesz określić bezwzględny czas i przedział czasu, `Insert` metoda zgłosi `ArgumentException` wyjątku.

> [!NOTE]
> Ta implementacja `AddCacheItem(key, value)` metoda ma obecnie niektóre nieprawidłowości. Firma Microsoft będzie adresu i rozwiązać te problemy w kroku 4.


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Krok 4: Unieważniania pamięci podręcznej podczas danych jest zmodyfikowany za pomocą architektury

Wraz z metod pobierania danych buforowanie warstwy należy podać te same metody jako logiki warstwy Biznesowej, wstawianie, aktualizowanie i usuwanie danych. Metody modyfikacji danych s CL nie należy modyfikować buforowane dane, ale zamiast wywoływać metodę logiki warstwy Biznesowej s do modyfikacji odpowiednich danych i następnie unieważnić pamięci podręcznej. Jak widzieliśmy w poprzednim samouczek jest takie samo zachowanie stosowanym przez element ObjectDataSource po włączeniu buforowania funkcji i jej `Insert`, `Update`, lub `Delete` metody są wywoływane.

Następujące `UpdateProduct` przeciążenia ilustruje sposób implementowania metod modyfikacji danych w CL:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample8.vb)]

Modyfikacja odpowiednie dane warstwy logiki biznesowej — metoda jest wywoływana, ale przed zwróceniem odpowiedzi musimy unieważnienie pamięci podręcznej. Niestety, unieważniania pamięci podręcznej nie jest utrudnione ponieważ `ProductsCL` klasy s `GetProducts()` i `GetProductsByCategoryID(categoryID)` metody należy dodać elementy do pamięci podręcznej z różnymi kluczami i `GetProductsByCategoryID(categoryID)` metoda dodaje element różnych pamięci podręcznej dla każdego unikatowy *categoryID*.

Gdy unieważniania pamięci podręcznej, należy usunąć *wszystkie* elementów, które mogły zostać dodane przez `ProductsCL` klasy. Można to zrobić przez skojarzenie *pamięci podręcznej zależności* z każdym elementem dodanych do pamięci podręcznej w `AddCacheItem(key, value)` metody. Ogólnie rzecz biorąc zależności pamięci podręcznej może być innego elementu w pamięci podręcznej, pliku w systemie plików lub dane z bazy danych programu Microsoft SQL Server. Gdy zależności zmiany lub jest usunięty z pamięci podręcznej, elementy pamięci podręcznej skojarzonej z nim automatycznie są usunięty z pamięci podręcznej. W tym samouczku, chcemy, aby utworzyć dodatkowe elementu w pamięci podręcznej, która służy jako zależność pamięci podręcznej dla wszystkich elementów dodane za pośrednictwem `ProductsCL` klasy. W ten sposób wszystkie te elementy można usunąć z pamięci podręcznej przez usunięcie zależności pamięci podręcznej.

Aktualizacja Let s `AddCacheItem(key, value)` — metoda poszczególnych elementów dodanych do pamięci podręcznej za pomocą tej metody, dzięki którym jest skojarzony z zależnością jednej pamięci podręcznej:


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample9.vb)]

`MasterCacheKeyArray` jest tablicą ciągów, który zawiera pojedynczą wartość ProductsCache. Po pierwsze element pamięci podręcznej zostanie dodany do pamięci podręcznej i przypisane do bieżącej daty i godziny. Jeśli istnieje już element pamięci podręcznej, jest aktualizowana. Następnie jest tworzony zależności pamięci podręcznej. [ `CacheDependency` Klasy](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) s Konstruktor ma kilka przeciążeń, ale używaną w tym miejscu oczekuje dwóch `String` tablicy danych wejściowych. Pierwsza z nich określa zestaw plików, które mają być używane jako zależności. Ponieważ firma Microsoft ADAM Chcę używać zależności opartych na plikach, wartość `Nothing` służy do pierwszego parametru wejściowego. Drugi parametr wejściowy określa zestaw kluczy pamięci podręcznej do użycia jako zależności. W tym miejscu możemy określić naszego jednego zależności `MasterCacheKeyArray`. `CacheDependency` Są następnie przekazywane do `Insert` metody.

Z tym modyfikację `AddCacheItem(key, value)`, invaliding pamięci podręcznej jest tak proste, jak usunięcie zależności.


[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample10.vb)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Krok 5: Wywoływanie buforowania warstwy z warstwy prezentacji

Buforowanie warstwy s klasy i metody może służyć do pracy z danymi przy użyciu technik możemy stawienia zbadać w całym te samouczki. Aby zilustrować pracę z danymi w pamięci podręcznej, Zapisz zmiany, aby `ProductsCL` klasy, a następnie otwórz `FromTheArchitecture.aspx` strony `Caching` folderu i Dodaj element GridView. Z widoku GridView s tagu Utwórz nowy element ObjectDataSource. W pierwszym kroku kreatora s powinna zostać wyświetlona `ProductsCL` klasy jako jedną z opcji z listy rozwijanej.


[![Klasa ProductsCL znajduje się na liście rozwijanej obiektu biznesowa](caching-data-in-the-architecture-vb/_static/image5.png)](caching-data-in-the-architecture-vb/_static/image4.png)

**Rysunek 4**: `ProductsCL` klasa znajduje się na liście rozwijanej obiektu Business ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-in-the-architecture-vb/_static/image6.png))


Po wybraniu `ProductsCL`, kliknij przycisk Dalej. Listy rozwijanej wybierz karcie zawiera dwa elementy - `GetProducts()` i `GetProductsByCategoryID(categoryID)` i na karcie Aktualizacja ma jedyny `UpdateProduct` przeciążenia. Wybierz `GetProducts()` metody z wybierz kartę i `UpdateProducts` metody na karcie AKTUALIZACJĘ i kliknij przycisk Zakończ.


[![Metody klasy ProductsCL s są wymienione w listy rozwijanej](caching-data-in-the-architecture-vb/_static/image8.png)](caching-data-in-the-architecture-vb/_static/image7.png)

**Rysunek 5**: `ProductsCL` metod klasy s są wymienione w listy rozwijanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-in-the-architecture-vb/_static/image9.png))


Po zakończeniu działania kreatora programu Visual Studio ustawi ObjectDataSource s `OldValuesParameterFormatString` właściwości `original_{0}` i dodaj odpowiednie pola do widoku GridView. Zmień `OldValuesParameterFormatString` jego wartość domyślna właściwości `{0}`i skonfigurować do obsługi stronicowania, sortowanie i edytowanie widoku GridView. Ponieważ `UploadProducts` używane przez CL przeciążenie akceptuje tylko nazwa produktu edytowanych s i cen, ograniczyć widoku GridView, tak aby tylko te pola są edytowalne.

W poprzednim samouczek zdefiniowanego GridView zawierać pól dla `ProductName`, `CategoryName`, i `UnitPrice` pól. Możesz replikować to formatowanie i struktury, w tym przypadku z widoku GridView i ObjectDataSource s deklaratywne znaczników powinien wyglądać podobnie do następującego:


[!code-aspx[Main](caching-data-in-the-architecture-vb/samples/sample11.aspx)]

W tym momencie mamy strona używa warstwy buforowania. Aby wyświetlić pamięci podręcznej w akcji, ustaw punkty przerwania w `ProductsCL` klasy s `GetProducts()` i `UpdateProduct` metody. Odwiedź stronę w przeglądarce i kroków kodu podczas sortowania i stronicowania, aby wyświetlić dane pobierane z pamięci podręcznej. Następnie zaktualizuj rekord i należy pamiętać, że unieważnienia pamięci podręcznej, a w rezultacie jest pobierana z logiki warstwy Biznesowej w przypadku danych jest odbitych do widoku GridView.

> [!NOTE]
> Warstwa buforowanie w pobierania towarzyszące w tym artykule nie zostało zakończone. Zawiera tylko jedną klasę `ProductsCL`, który sportowy tylko kilku metod. Ponadto tylko jednej strony ASP.NET używa CL (`~/Caching/FromTheArchitecture.aspx`) wszystkich innych nadal logiki warstwy Biznesowej bezpośrednie odwołanie. Jeśli planujesz używanie CL w aplikacji, wszystkie wywołania z warstwy prezentacji należy przejdź do CL, które wymagałyby, który klasy s CL, i metody pasuje do tych klasy i metody w logiki warstwy Biznesowej aktualnie używane przez warstwę prezentacji.


## <a name="summary"></a>Podsumowanie

Podczas buforowania, mogą być stosowane na warstwę prezentacji z programu ASP.NET 2.0 s SqlDataSource i kontrolki ObjectDataSource, najlepiej buforowanie obowiązki będzie delegowane do oddzielnych warstwy architektury. W tym samouczku utworzyliśmy buforowanie warstwy, która znajduje się między warstwą prezentacji a warstwy logiki biznesowej. Buforowanie warstwy musi podać ten sam zestaw klasy i metody, które istnieją w logiki warstwy Biznesowej i są nazywane z warstwy prezentacji.

Przykłady buforowanie warstwy możemy przedstawione w tym i samouczki poprzedniego wystawiane *reaktywne ładowania*. Z reaktywne ładowania danych została załadowana do pamięci podręcznej tylko po wysłaniu żądania danych i brakuje danych z pamięci podręcznej. Dane mogą być również *aktywnego załadować* do pamięci podręcznej, technika który ładuje dane w pamięci podręcznej przed będzie to wymagane. W następnym samouczku przedstawiono będzie przykładem aktywnego ładowania, jeśli przyjrzymy się jak przechowywać wartości statyczne do pamięci podręcznej podczas uruchamiania aplikacji.

Programowanie przyjemność!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Teresa Murphy. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](caching-data-with-the-objectdatasource-vb.md)
> [dalej](caching-data-at-application-startup-vb.md)
