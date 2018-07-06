---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
title: Buforowanie danych w architekturze (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W poprzednim samouczku dowiedzieliśmy sposobu stosowania pamięci podręcznej w warstwie prezentacji. W tym samouczku będziemy Dowiedz się, jak korzystać z zalet naszej warstwowej architectu...
ms.author: aspnetcontent
ms.date: 05/30/2007
ms.assetid: d29a7c41-0628-4a23-9dfc-bfea9c6c1054
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-cs
msc.type: authoredcontent
ms.openlocfilehash: 80805bae14654d6817328232453031384ceadad6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820461"
---
<a name="caching-data-in-the-architecture-c"></a>Buforowanie danych w architekturze (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_CS.exe) lub [Pobierz plik PDF](caching-data-in-the-architecture-cs/_static/datatutorial59cs1.pdf)

> W poprzednim samouczku dowiedzieliśmy sposobu stosowania pamięci podręcznej w warstwie prezentacji. W tym samouczku będziemy Dowiedz się, jak korzystać z zalet naszej architektury warstwowej pamięci podręcznej danych na warstwę logiki biznesowej. Możemy to zrobić, rozszerzenie architektury obejmujący warstwy pamięci podręcznej.


## <a name="introduction"></a>Wprowadzenie

Jak widzieliśmy w poprzednim samouczku, buforowanie danych s ObjectDataSource jest tak proste, jak ustawienie kilka właściwości. Niestety kontrolki ObjectDataSource ma zastosowanie, pamięć podręczna w warstwie prezentacji, składającą się ściśle zasad buforowania strony ASP.NET. Jest jednym z powodów tworzenia architektury warstwowej umożliwia takie sprzężenia podzielone. Warstwy logiki biznesowej, na przykład oddziela logiki biznesowej na stronach ASP.NET, podczas gdy warstwa dostępu do danych oddziela szczegóły dostępu do danych. To oddzielenie szczegóły dostępu logikę i dane biznesowe jest preferowane, w całości, ponieważ jego system staje się bardziej czytelny, będzie łatwiejszy w utrzymaniu i bardziej elastyczne zmiany. Pozwala także wiedzy specjalistycznej i dzielenie liczb pracy dewelopera nad Warstwa prezentacji t trzeba należy zapoznać się ze szczegółami s bazy danych w celu wykonywania swojej pracy. Oddzielenie zasad pamięci podręcznej z warstwy prezentacji oferuje podobne korzyści.

W tym samouczku będziemy rozszerzać naszej architektury, aby uwzględnić *warstwy buforowania* (lub CL w skrócie) który wykorzystuje nasze zasady buforowania. Warstwy buforowania będzie zawierać `ProductsCL` klasy, która zapewnia dostęp do informacji o produkcie przy użyciu metod, takich jak `GetProducts()`, `GetProductsByCategoryID(categoryID)`, i tak dalej, aby po wywołaniu, będzie pierwsza próba pobrania danych z pamięci podręcznej. Jeśli pamięć podręczna jest pusta, te metody wywoła odpowiednie `ProductsBLL` metody w LOGIKI, która z kolei może pobrać danych z warstwy DAL. `ProductsCL` Metod w pamięci podręcznej danych pobranych z LOGIKI przed zwróceniem.

Jak pokazano na rysunku 1, CL znajduje się między prezentacji i warstwy logiki biznesowej.


![Warstwy pamięci podręcznej (CL) jest kolejną warstwę w nasze architektury](caching-data-in-the-architecture-cs/_static/image1.png)

**Rysunek 1**: pamięć podręczna warstwy (CL) jest kolejną warstwę w nasze architektury


## <a name="step-1-creating-the-caching-layer-classes"></a>Krok 1: Tworzenie pamięci podręcznej klas warstwy

W tym samouczku utworzymy bardzo prosty CL, za pomocą jednej klasy `ProductsCL` zawierający tylko kilku metod. Tworzenie pełną warstwy buforowania dla całej aplikacji wymaga tworzenia `CategoriesCL`, `EmployeesCL`, i `SuppliersCL` klasy i udostępnieniu metod w klasach te warstwy buforowania dla każdego dostępu lub modyfikacji metody danych w LOGIKI. Podobnie jak w przypadku LOGIKI i warstwy DAL, warstwy buforowania najlepiej powinny zostać wdrożone jako oddzielny projekt biblioteki klas; Jednak firma Microsoft wdroży go jako klasa w `App_Code` folderu.

Do innych klas klarownie oddzielne CL z klas DAL i logiki warstwy Biznesowej umożliwiają s Utwórz nowy podfolder w `App_Code` folderu. Kliknij prawym przyciskiem myszy `App_Code` folder w Eksploratorze rozwiązań wybierz nowy Folder i nazwę nowego folderu `CL`. Po utworzeniu tego folderu, Dodaj do niej nową klasę o nazwie `ProductsCL.cs`.


![Dodaj nowy Folder o nazwie CL i klasę o nazwie ProductsCL.cs](caching-data-in-the-architecture-cs/_static/image2.png)

**Rysunek 2**: Dodaj nowy Folder o nazwie `CL` i klasę o nazwie `ProductsCL.cs`


`ProductsCL` Klasy powinna zawierać ten sam zestaw metod dostępu i modyfikowanie danych tak jak w programie klasą odpowiedniej warstwy logiki biznesowej (`ProductsBLL`). Zamiast tworzenia wszystkie te metody pozwalają s kompilacji tylko kilka tutaj, aby można było uzyskać pewne pojęcie wzorców posługują się CL. W szczególności, dodamy `GetProducts()` i `GetProductsByCategoryID(categoryID)` metod w kroku 3 i `UpdateProduct` przeciążenia w kroku 4. Możesz dodać pozostałe `ProductsCL` metod i `CategoriesCL`, `EmployeesCL`, i `SuppliersCL` klas w wolnym czasie.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Krok 2: Odczytywanie i zapisywanie w pamięci podręcznej danych

Kontrolki ObjectDataSource przedstawione w poprzednim samouczku wewnętrznie funkcję buforowania używa pamięci podręcznej danych ASP.NET do przechowywania danych pobranych z LOGIKI. Pamięć podręczna danych również są dostępne programowo z klasy CodeBehind stron ASP.NET lub klas w ramach architektury s dla aplikacji sieci web. Aby do odczytu i zapisu pamięci podręcznej danych z klasy CodeBehind s strony ASP.NET, należy użyć następującego wzorca:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample1.cs)]

[ `Cache` Klasy](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) s [ `Insert` metoda](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) ma kilka przeciążeń. `Cache["key"] = value` i `Cache.Insert(key, value)` oznaczają to samo i zarówno dodania elementu do pamięci podręcznej przy użyciu określonego klucza bez zdefiniowanego wygaśnięcia. Zazwyczaj chcemy określić wygaśnięcia, podczas dodawania elementu do pamięci podręcznej, jako zależność i/lub na podstawie czasu wygaśnięcia. Użyj jednego z innych `Insert` przeciążenia metody s, aby podać informacje na podstawie zależności lub do czasu wygaśnięcia.

Warstwy buforowania, s metody, należy najpierw sprawdzić, jeśli żądanych danych znajduje się w pamięci podręcznej, a jeśli tak, przywrócić go z tego miejsca. Jeśli żądanych danych nie jest w pamięci podręcznej, musi być wywoływane odpowiedniej metody LOGIKI. Wartość zwracaną powinien pamięci podręcznej, a następnie wróciło, tak jak pokazano na poniższym diagramie sekwencji.


![Metody s warstwy buforowania zwracają dane z pamięci podręcznej, jeśli jego s dostępne](caching-data-in-the-architecture-cs/_static/image3.png)

**Rysunek 3**: warstwy buforowania s metody zwracają dane z pamięci podręcznej, jeśli jego s dostępne


Sekwencja przedstawione na rysunku 3 odbywa się w klasach CL, przy użyciu następującego wzorca:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample2.cs)]

W tym miejscu *typu* jest typem danych znajdujących się w pamięci podręcznej `Northwind.ProductsDataTable`, na przykład *klucza* jest klucz, który unikatowo identyfikuje element pamięci podręcznej. Jeśli element z określonym *klucz* nie znajduje się w pamięci podręcznej, następnie *wystąpienia* będzie `null` i dane zostaną pobrane z odpowiedniej metody LOGIKI i dodane do pamięci podręcznej. Do czasu `return instance` osiągnięciu *wystąpienia* zawiera odwołanie do danych z pamięci podręcznej lub pobierane z LOGIKI.

Pamiętaj użyć wzorca powyżej, podczas uzyskiwania dostępu do danych z pamięci podręcznej. Następujący wzorzec, które na pierwszy rzut oka wygląda równoważnej zawiera niewielka różnica, która wprowadza sytuacji wyścigu. Warunki wyścigu są trudne do debugowania, ponieważ ujawniają się sporadycznie i są trudne do odtworzenia.


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample3.cs)]

Różnica ta sekunda, niepoprawny kod fragment jest fakt, że zamiast przechowywania odwołanie do elementu pamięci podręcznej w zmiennej lokalnej, pamięć podręczną danych odbywa się bezpośrednio w instrukcji warunkowej *i* w `return`. Załóżmy, że po osiągnięciu tego kodu `Cache["key"]` ma wartość inną niż`null`, lecz przed `return` osiągnięta zostanie instrukcja, wyklucza systemu mogą *klucz* z pamięci podręcznej. W tym przypadku rzadkich zwróci kod `null` wartości zamiast obiektu oczekiwanego typu.

> [!NOTE]
> Pamięć podręczna danych jest bezpieczna dla wątków, dzięki czemu komputer potrzebę t do synchronizowania dostępu wątku dla proste operacje odczytu i zapisu. Jednakże jeśli musisz wykonać wiele operacji na danych w pamięci podręcznej, które muszą być niepodzielną ponosisz odpowiedzialność za Implementowanie blokady lub inny mechanizm w celu zapewnienia bezpieczeństwa wątków. Zobacz [synchronizowania dostępu do pamięci podręcznej platformy ASP.NET](http://www.ddj.com/184406369) Aby uzyskać więcej informacji.


Element może zostać programowo wykluczony z pamięci podręcznej danych przy użyciu [ `Remove` metoda](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) w następujący sposób:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample4.cs)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Krok 3: Zwracanie informacji o produkcie z`ProductsCL`klasy

Dla tego samouczka umożliwiają s implementacji dwóch metod do zwracania informacji o produkcie z `ProductsCL` klasy: `GetProducts()` i `GetProductsByCategoryID(categoryID)`. Za pomocą `ProductsBL` klasy w warstwę logiki biznesowej `GetProducts()` metody w CL zwraca informacje dotyczące wszystkich produktów jako `Northwind.ProductsDataTable` obiektu, podczas gdy `GetProductsByCategoryID(categoryID)` zwraca wszystkie produkty z określonej kategorii.

Poniższy kod ilustruje część metody w `ProductsCL` klasy:


[!code-vb[Main](caching-data-in-the-architecture-cs/samples/sample5.vb)]

Najpierw należy zanotować `DataObject` i `DataObjectMethodAttribute` atrybuty stosowane do klasy i metody. Te atrybuty zawierają informacje do kreatora s ObjectDataSource wskazującą, jakie klasy i metody powinna zostać wyświetlona w krokach s kreatora. Ponieważ CL klasy i metody będą uzyskiwać dostęp z elementu ObjectDataSource w warstwie prezentacji, po dodaniu tych atrybutów w celu poprawienia środowiska czasu projektowania. Odwołaj się do [Tworzenie warstwy logiki biznesowej](../introduction/creating-a-business-logic-layer-cs.md) samouczek bardziej szczegółowego opis tych atrybutów i ich skutki.

W `GetProducts()` i `GetProductsByCategoryID(categoryID)` metody, z danymi zwróconymi z `GetCacheItem(key)` metody jest przypisany do zmiennej lokalnej. `GetCacheItem(key)` Metody, która zajmiemy się wkrótce, zwraca wartość określonego elementu z pamięci podręcznej na podstawie *klucz*. Jeśli nie z tych danych znajduje się w pamięci podręcznej, zostanie pobrana z odpowiednich `ProductsBLL` metody klasy, a następnie dodane do pamięci podręcznej, za pomocą `AddCacheItem(key, value)` metody.

`GetCacheItem(key)` i `AddCacheItem(key, value)` metody interfejsu z pamięci podręcznej danych, odczytywanie i zapisywanie wartości odpowiednio. `GetCacheItem(key)` Metoda jest prostsza dwóch. Po prostu zwraca wartość z klasy pamięci podręcznej przy użyciu przekazanego *klucza*:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample6.cs)]

`GetCacheItem(key)` nie używa *klucz* wartość dostarczony, ale zamiast tego wywołania `GetCacheKey(key)` metody, która zwraca *klucz* poprzedzonej ciągiem ProductsCache —. `MasterCacheKeyArray`, Który zawiera ciąg ProductsCache, jest również używany przez `AddCacheItem(key, value)` metody, ponieważ chwilę zobaczymy.

Z kodem klasę strony ASP.NET pamięci podręcznej danych jest możliwy za pomocą `Page` klasy s [ `Cache` właściwość](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)i umożliwia składni `Cache["key"] = value`, zgodnie z opisem w kroku 2. Z klasy w ramach architektury, pamięć podręczną danych jest możliwy za pomocą `HttpRuntime.Cache` lub `HttpContext.Current.Cache`. [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)firmy wpis w blogu [HttpRuntime.Cache programu vs. HttpContext.Current.Cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) informacje o niewielkim wzroście wydajności zaletą używania `HttpRuntime` zamiast `HttpContext.Current`; w związku z tym, `ProductsCL` używa `HttpRuntime`.

> [!NOTE]
> Jeśli architektury jest implementowany przy użyciu biblioteki klas projektów, a następnie należy dodać odwołanie do `System.Web` zestawu, aby można było używać [HttpRuntime](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) i [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) klasy.


Jeśli element nie zostanie znaleziony w pamięci podręcznej, `ProductsCL` metod klasy s uzyskać danych z LOGIKI i dodaj go do pamięć podręczną za pomocą `AddCacheItem(key, value)` metody. Aby dodać *wartość* do pamięci podręcznej moglibyśmy użyć następujący kod, który korzysta z upływem czasu 60 sekundy:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample7.cs)]

`DateTime.Now.AddSeconds(CacheDuration)` Określa na podstawie czasu wygaśnięcia 60 sekund w przyszłości podczas [ `System.Web.Caching.Cache.NoSlidingExpiration` ](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) wskazuje s, ponieważ nie wygaśniecie. Podczas tego `Insert` przeciążenia metody ma wejściowych parametrów dla obu bezwzględnym i przedłużanie ważności, należy można podać tylko jedną z dwóch. Jeśli spróbujesz określić bezwzględnego czasu i przedział czasu `Insert` metoda zgłosi `ArgumentException` wyjątku.

> [!NOTE]
> Ta implementacja `AddCacheItem(key, value)` metoda obecnie ma pewne wady. Firma Microsoft będzie adresu i rozwiązywania tych problemów w kroku 4.


## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Krok 4: Unieważnienia pamięci podręcznej po danych jest zmodyfikowany za pomocą architektury

Wraz z metod pobierania danych warstwy buforowania musi podać tej samej metody co LOGIKI Wstawianie, aktualizowanie i usuwanie danych. Metody modyfikacji danych s CL, nie należy modyfikować dane w pamięci podręcznej, ale zamiast wywoływać metodę LOGIKI s do modyfikacji odpowiednich danych, a następnie unieważnienia pamięci podręcznej. Jak widzieliśmy w poprzednim samouczku jest takie samo zachowanie, który kontrolki ObjectDataSource ma zastosowanie, gdy jego funkcje pamięci podręcznej są włączone i jego `Insert`, `Update`, lub `Delete` metody są wywoływane.

Następujące `UpdateProduct` przeciążenia ilustruje sposób implementacji metody modyfikacji danych w CL:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample8.cs)]

Modyfikowanie danych odpowiedniej warstwy logiki biznesowej, metoda jest wywoływana, ale przed zwróceniem odpowiedzi przez punkt końcowy należy do unieważnienia pamięci podręcznej. Niestety, unieważnienia pamięci podręcznej jest utrudnione ponieważ `ProductsCL` klasy s `GetProducts()` i `GetProductsByCategoryID(categoryID)` metody należy dodać elementy do pamięci podręcznej z różnymi kluczami i `GetProductsByCategoryID(categoryID)` metoda dodaje element pamięci podręcznej różne dla każdego unikatowy *categoryID*.

Gdy unieważnienia pamięci podręcznej, należy usunąć *wszystkich* elementów, które mogły zostać dodane przez `ProductsCL` klasy. Można to osiągnąć przez skojarzenie *zależności w pamięci podręcznej* z każdym elementem dodanych do pamięci podręcznej w `AddCacheItem(key, value)` metody. Ogólnie rzecz biorąc zależności pamięci podręcznej może być inny element w pamięci podręcznej, plikiem systemu plików, lub dane z bazy danych programu Microsoft SQL Server. Gdy zależność zmieni się lub jest usuwane z pamięci podręcznej, elementy pamięci podręcznej jest skojarzony z automatycznie obrazuje z pamięci podręcznej. W tym samouczku chcemy utworzyć dodatkowy element w pamięci podręcznej, która służy jako zależność pamięci podręcznej dla wszystkich elementów dodanych do `ProductsCL` klasy. Dzięki temu wszystkie te elementy można usunąć z pamięci podręcznej przez usunięcie zależności pamięci podręcznej.

Aktualizacja pozwalają s `AddCacheItem(key, value)` forma tak, aby poszczególne elementy dodane do pamięci podręcznej za pomocą tej metody jest skojarzona z zależności jednej pamięci podręcznej:


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample9.cs)]

`MasterCacheKeyArray` jest tablicą ciągów, który zawiera pojedynczą wartość ProductsCache. Po pierwsze element pamięci podręcznej zostaną dodane do pamięci podręcznej i przypisane do bieżącej daty i godziny. Jeśli istnieje już element pamięci podręcznej, jest aktualizowana. Następnie jest tworzony zależności pamięci podręcznej. [ `CacheDependency` Klasy](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) s Konstruktor ma wiele przeciążeń, ale użytego w tym miejscu oczekuje, że dwa `string` tablicy danych wejściowych. Pierwsza z nich określa zestaw plików, które ma być używany jako zależności. Ponieważ firma Microsoft don t ma być używana wszelkie zależności opartych na plikach, wartość `null` jest używany jako pierwszy parametr wejściowy. Drugi parametr wejściowy określa zestaw kluczy pamięci podręcznej do użycia jako zależności. W tym miejscu możemy określić zależność pojedynczego `MasterCacheKeyArray`. `CacheDependency` Jest następnie przekazywany do `Insert` metody.

Za pomocą tej modyfikacji `AddCacheItem(key, value)`, invaliding pamięć podręczna jest proste i polega na usunięciu zależności.


[!code-csharp[Main](caching-data-in-the-architecture-cs/samples/sample10.cs)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Krok 5: Wywoływanie warstwy pamięci podręcznej z warstwy prezentacji

Pamięć podręczna warstwy s klas i metod może służyć do pracy z danymi za pomocą technik możemy ve badany w całym tych samouczków. Aby zilustrować pracy przy użyciu danych z pamięci podręcznej, zapisać zmiany w `ProductsCL` klasy, a następnie otwórz `FromTheArchitecture.aspx` strony w `Caching` folderze i Dodaj GridView. W tagu inteligentnego s GridView należy utworzyć nowe kontrolki ObjectDataSource. W pierwszym kroku s kreator powinien zostać wyświetlony `ProductsCL` klasy jako jedną z opcji z listy rozwijanej.


[![Klasa ProductsCL znajduje się na liście rozwijanej obiektów biznesowych](caching-data-in-the-architecture-cs/_static/image5.png)](caching-data-in-the-architecture-cs/_static/image4.png)

**Rysunek 4**: `ProductsCL` klasa znajduje się na liście rozwijanej obiektów Business ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-in-the-architecture-cs/_static/image6.png))


Po wybraniu `ProductsCL`, kliknij przycisk Dalej. Listy rozwijanej wybierz karta ma dwa elementy — `GetProducts()` i `GetProductsByCategoryID(categoryID)` i na karcie aktualizacji ma jedyny `UpdateProduct` przeciążenia. Wybierz `GetProducts()` metody z wybierz kartę i `UpdateProducts` metody z karty aktualizacji i kliknij przycisk Zakończ.


[![Metody klasy ProductsCL s są wymienione w listy rozwijane](caching-data-in-the-architecture-cs/_static/image8.png)](caching-data-in-the-architecture-cs/_static/image7.png)

**Rysunek 5**: `ProductsCL` metod klasy s są wymienione w listy rozwijanej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](caching-data-in-the-architecture-cs/_static/image9.png))


Po ukończeniu kreatora, program Visual Studio ustawi ObjectDataSource s `OldValuesParameterFormatString` właściwość `original_{0}` i dodaj odpowiednie pola do widoku GridView. Zmiana `OldValuesParameterFormatString` jego wartość domyślną właściwości `{0}`i skonfiguruj GridView do obsługi stronicowania, sortowania i edytowania. Ponieważ `UploadProducts` przeciążenia, które posługują się CL akceptuje tylko nazwę edytowanego produktu s i ceny, ograniczyć widoku GridView, tak aby tylko te pola są edytowalne.

W poprzednim samouczku zdefiniowaliśmy GridView podczas dołączania pól dla `ProductName`, `CategoryName`, i `UnitPrice` pola. Możesz replikować ten formatowania i struktury, w którym to przypadku Twoje s kontrolkami GridView i kontrolki ObjectDataSource deklaratywne znaczników powinien wyglądać podobnie do następującego:


[!code-aspx[Main](caching-data-in-the-architecture-cs/samples/sample11.aspx)]

W tym momencie mamy strona, która korzysta z warstwy pamięci podręcznej. Aby wyświetlić pamięci podręcznej w działaniu, ustawić punkty przerwania `ProductsCL` klasy s `GetProducts()` i `UpdateProduct` metody. Odwiedź stronę w przeglądarce i Przechodź przez kod, podczas sortowania i stronicowania, aby można było wyświetlać dane pobrane z pamięci podręcznej. Następnie zaktualizuj rekord i Zauważ, że pamięć podręczna zostaje unieważniony, i w związku z tym, jest pobierana z LOGIKI po danych jest odbitych do kontrolki GridView.

> [!NOTE]
> Warstwy buforowania, udostępniane do pobrania, towarzyszący w tym artykule nie została ukończona. Zawiera ona tylko jedną klasę `ProductsCL`, który sportowe tylko niewielki podzbiór metod. Ponadto tylko jednej strony ASP.NET używa CL (`~/Caching/FromTheArchitecture.aspx`) wszystkie pozostałe nadal odwoływać się do LOGIKI bezpośrednio. Jeśli planujesz używanie CL w aplikacji, wszystkie wywołania z warstwy prezentacji przejdź do CL, co wymagałoby, klasy s CL i metody omówione tych klas i metod w LOGIKI obecnie używana przez warstwę prezentacji.


## <a name="summary"></a>Podsumowanie

Podczas buforowania można zastosować na warstwę prezentacji z ASP.NET 2.0 s SqlDataSource i kontrolki ObjectDataSource, najlepiej buforowania obowiązki będzie delegowane do oddzielnych warstwy w architekturze. W tym samouczku utworzyliśmy warstwy buforowania, który znajduje się między warstwą prezentacji i warstwy logiki biznesowej. Warstwy buforowania musi udostępniają identyczny zestaw klas i metod, które istnieją w LOGIKI i są wywoływane z warstwy prezentacji.

Przykłady warstwy buforowania, rozważyliśmy w tym i w poprzednich samouczkach uwidocznione *reaktywne ładowania*. Przy użyciu ładowania reaktywne dane są ładowane do pamięci podręcznej, tylko po wysłaniu żądania danych i Brak danych z pamięci podręcznej. Dane mogą być również *aktywnie załadować* w pamięci podręcznej, to technika, służy do ładowania danych w pamięci podręcznej, zanim będzie to wymagane. W następnym samouczku zobaczymy przykładem aktywne ładowanie, gdy spojrzymy na sposób przechowywania wartości statycznej w pamięci podręcznej podczas uruchamiania aplikacji.

Wszystkiego najlepszego programowania!

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Teresa Murph. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](caching-data-with-the-objectdatasource-cs.md)
> [dalej](caching-data-at-application-startup-cs.md)
