---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Włączanie operacji CRUD w składniku ASP.NET Web API 1 | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym samouczku przedstawiono sposób obsługi operacji CRUD usługi HTTP przy użyciu interfejsu API sieci Web platformy ASP.NET. Używane w samouczek Visual Studio 2012 Web region wersje oprogramowania...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 69b7d5453b6ff36d6e28a69428b016cb8cfd06e9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "29153011"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Włączanie operacji CRUD w składniku ASP.NET Web API 1
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> W tym samouczku przedstawiono sposób obsługi operacji CRUD usługi HTTP przy użyciu interfejsu API sieci Web platformy ASP.NET.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Visual Studio 2012
> - Składnik Web API 1 (dotyczy również 2 interfejsu API sieci Web)


Oznacza CRUD &quot;tworzenia, odczytu, aktualizacji i usuwania,&quot; będące cztery operacje podstawowej bazy danych. Wiele usług HTTP modelu również operacji CRUD za pośrednictwem interfejsu API REST przypominającej lub REST.

W tym samouczku utworzysz bardzo proste składnika web API do zarządzania listą produktów. Każdy produkt będzie zawierać nazwę, ceny i kategorii (takich jak &quot;toys&quot; lub &quot;sprzętu&quot;), oraz identyfikatora produktu.

Powoduje to udostępnienie następujących metod produktów interfejsu API.

| Akcja | Metoda HTTP | Względny identyfikator URI |
| --- | --- | --- |
| Pobranie listy wszystkich produktów | POBIERZ | / api/produktów |
| Uzyskiwanie produktu według Identyfikatora | POBIERZ | /API/produkty/*id* |
| Uzyskiwanie produktu według kategorii | POBIERZ | produkty/api /? kategorii =*kategorii* |
| Tworzenie nowego produktu | POST | / api/produktów |
| Aktualizacji produktu | UMIEŚĆ | /API/produkty/*id* |
| Usuwanie produktu | DELETE | /API/produkty/*id* |

Zwróć uwagę, niektóre identyfikatory URI są identyfikator produktu w ścieżce. Na przykład, aby uzyskać produktu o identyfikatorze jest 28, klient wysyła żądanie GET `http://hostname/api/products/28`.

### <a name="resources"></a>Resources

Produkty interfejsu API definiuje identyfikatorów URI dla dwa typy zasobów:

| Zasób | Identyfikator URI |
| --- | --- |
| Lista wszystkich produktów. | / api/produktów |
| Indywidualnych produktów. | /API/produkty/*id* |

### <a name="methods"></a>Metody

Cztery główne metody HTTP (GET, PUT, POST i DELETE) mogą być mapowane na operacje CRUD w następujący sposób:

- GET pobiera reprezentacja zasobu określonego identyfikatora URI. GET, powinien mieć żadnych efektów ubocznych na serwerze.
- Umieść aktualizuje zasób o określonym identyfikatorze URI. PUT można również utworzyć nowy zasób o określonym identyfikatorze URI, jeśli serwer umożliwia klientom określ nowy identyfikator URI. W tym samouczku interfejs API nie obsługuje tworzenia za pomocą PUT.
- POST tworzy nowy zasób. Serwer przypisuje identyfikator URI dla nowego obiektu i zwraca ten identyfikator URI jako część komunikatu odpowiedzi.
- Usuń Usuwa zasób o określonym identyfikatorze URI.

Uwaga: Metody PUT zastępuje jednostki całego produktu. Oznacza to klient powinien wysłać pełną reprezentację zaktualizowane produktu. Jeśli chcesz obsługiwać aktualizacje częściowe metoda poprawki jest zalecana. W tym samouczku nie implementuje poprawki.

## <a name="create-a-new-web-api-project"></a>Utwórz nowy projekt interfejsu API sieci Web

Zacznij od uruchomienia programu Visual Studio i wybierz **nowy projekt** z **Start** strony. Lub z **pliku** menu, wybierz opcję **nowy** , a następnie **projektu**.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz pozycję **Web**. Na liście szablony projektów, wybierz **aplikacji sieci Web programu ASP.NET MVC 4**. Nazwij projekt &quot;ProductStore&quot; i kliknij przycisk **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **interfejsu API sieci Web** i kliknij przycisk **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Dodawanie modelu

A *modelu* jest obiekt, który reprezentuje dane w aplikacji. W interfejsie API sieci Web ASP.NET silnie typizowanych obiektów CLR można użyć jako modele i one będą automatycznie wykonywane szeregowo XML lub JSON dla klienta.

ProductStore API naszych danych składa się z produktów, dlatego utworzymy nową klasę o nazwie `Product`.

Jeśli w Eksploratorze rozwiązań nie jest widoczny, kliknij przycisk **widoku** menu i wybierz **Eksploratora rozwiązań**. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modele** folderu. Wybierz z meny kontekstu **Dodaj**, a następnie wybierz pozycję **klasy**. Nazwa klasy &quot;produktu&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Dodaj następujące właściwości `Product` klasy.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Dodawanie repozytorium

Musimy przechowywanie kolekcji produktów. Jest dobrym pomysłem jest oddzielnych kolekcji z naszych implementacji usługi. W ten sposób możemy zmienić magazynu zapasowego bez ponownego tworzenia klasy usługi. Ten typ projektu jest nazywany *repozytorium* wzorca. Rozpocznij od zdefiniowania ogólny interfejs umożliwiający repozytorium.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modele** folderu. Wybierz **Dodaj**, a następnie wybierz pozycję **nowy element**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń węzeł C#. W obszarze C#, wybierz **kod**. Na liście szablony kodu, wybierz **interfejsu**. Nazwa interfejsu &quot;IProductRepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Dodaj następujące wdrożenia:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Teraz Dodaj kolejną klasę do folderu modeli o nazwie &quot;ProductRepository.&quot; Ta klasa będzie implementowany `IProductRespository` interfejsu. Dodaj następujące wdrożenia:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

Repozytorium przechowuje listy w pamięci lokalnej. Jest to samouczek, ale w rzeczywistej aplikacji będzie przechowywać dane zewnętrznie, albo bazy danych lub w magazynie w chmurze. Wzorzec repozytorium ułatwi Zmień implementację później.

## <a name="adding-a-web-api-controller"></a>Dodawanie kontrolera interfejsu API sieci Web

Użytkownicy mający doświadczenie z platformą ASP.NET MVC, następnie znasz już kontrolerów. W interfejsie API sieci Web ASP.NET *kontrolera* jest klasa, która obsługuje żądania HTTP od klienta. Kreator nowego projektu utworzone dwa kontrolery automatycznie podczas tworzenia projektu. Aby je wyświetlić, rozwiń folder kontrolerów w Eksploratorze rozwiązań.

- HomeController jest tradycyjnych kontrolera ASP.NET MVC. Jest odpowiedzialny za obsługująca stron HTML dla lokacji i nie jest bezpośrednio powiązana z naszych interfejsu API sieci web.
- ValuesController jest przykład WebAPI kontrolera.

Przejdź dalej i usunąć ValuesController, prawym przyciskiem myszy plik w Eksploratorze rozwiązań i wybierając **usunąć.** Teraz Dodaj nowy kontroler, w następujący sposób:

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy folder kontrolery. Wybierz **Dodaj** , a następnie wybierz **kontrolera**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

W **Dodaj kontroler** kreatora, nazwy kontrolera &quot;ProductsController&quot;. W **szablonu** listy rozwijanej wybierz **pusty Kontroler interfejsu API**. Następnie kliknij przycisk **Dodaj**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Nie jest konieczne w celu uruchomienia programu contollers folder o nazwie kontrolerów. Nazwa folderu nie jest ważna; jest po prostu wygodny sposób organizowania plików źródłowych.


**Dodaj kontroler** Kreator utworzy plik o nazwie ProductsController.cs w folderze kontrolerów. Jeśli ten plik nie jest jeszcze otwarty, kliknij dwukrotnie plik, aby go otworzyć. Dodaj następujące **przy użyciu** instrukcji:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Dodaj pola zawierające **IProductRepository** wystąpienia.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> Wywoływanie `new ProductRepository()` w kontrolerze nie jest najlepszym projektu, ponieważ wiąże kontrolera w celu wykonania konkretnego `IProductRepository`. Aby lepszym rozwiązaniem, zobacz [za pomocą mechanizmu rozpoznawania zależności dla interfejsu API sieci Web](../advanced/dependency-injection.md).


## <a name="getting-a-resource"></a>Pobieranie zasobu

Interfejs API ProductStore uwidoczni kilka &quot;odczytu&quot; akcje jako metody HTTP GET. Każda akcja odpowiada metody w `ProductsController` klasy.

| Akcja | Metoda HTTP | Względny identyfikator URI |
| --- | --- | --- |
| Pobranie listy wszystkich produktów | POBIERZ | / api/produktów |
| Uzyskiwanie produktu według Identyfikatora | POBIERZ | /API/produkty/*id* |
| Uzyskiwanie produktu według kategorii | POBIERZ | produkty/api /? kategorii =*kategorii* |

Aby uzyskać listę wszystkich produktów, Dodaj tę metodę w celu `ProductsController` klasy:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

Nazwa metody rozpoczyna się od &quot;uzyskać&quot;, więc Konwencja mapowania żądania GET. Ponadto, ponieważ metoda nie ma parametrów, mapowania identyfikatora URI, który nie zawiera *&quot;identyfikator&quot;* segment w ścieżce.

Uzyskanie produktu przez identyfikator, Dodaj tę metodę w celu `ProductsController` klasy:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Ta nazwa metody również rozpoczyna się od &quot;uzyskać&quot;, ale metoda ma parametr o nazwie *identyfikator*. Ten parametr jest mapowany na &quot;identyfikator&quot; segmentu ścieżki identyfikatora URI. Platformę ASP.NET Web API automatycznie konwertuje identyfikator prawidłowy typ danych (**int**) dla parametru.

Metoda GetProduct zgłasza wyjątek typu **HttpResponseException** Jeśli *identyfikator* jest nieprawidłowy. Ten wyjątek zostanie zamieniona przez platformę na błąd 404 (nie znaleziono).

Na koniec należy dodać metody do znalezienia produktów według kategorii:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Jeśli identyfikator URI żądania zawiera ciąg zapytania, interfejsu API sieci Web próbuje dopasować parametry na parametry dla metody kontrolera. W związku z tym identyfikatorem URI w postaci "interfejsu api/produktów? kategorii =*kategorii*" przypisze do tej metody.

## <a name="creating-a-resource"></a>Tworzenie zasobu

Następnie dodamy metodę `ProductsController` klasy w celu utworzenia nowego produktu. Oto prosty implementacji metody:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Należy uwzględnić dwa elementy o tej metody:

- Nazwa metody rozpoczyna się od &quot;Post... &quot;. Aby utworzyć nowy produkt, klient wysyła żądanie HTTP POST.
- Metoda korzysta z parametru typu produkt. W składniku Web API parametry o typach złożonych, są przeprowadzić deserializacji treści żądania. W związku z tym oczekujemy klientowi wysłanie zserializowana reprezentacja obiektu produktu w formacie XML lub JSON.

Ta implementacja będzie działać, ale nie zostało jeszcze zakończone. Najlepiej, jeśli chcemy odpowiedzi HTTP są następujące:

- **Kod odpowiedzi:** domyślnie przez strukturę interfejsu API sieci Web ustawia kod stanu odpowiedzi 200 (OK). Jednak zgodnie z protokołu HTTP/1.1, gdy żądanie POST powoduje utworzenie zasobu, serwer powinien Odpowiedz, podając stanu 201 (utworzono).
- **Lokalizacja:** gdy serwer tworzy zasób, powinny zawierać identyfikatora URI nowego zasobu w nagłówku lokalizacji odpowiedzi.

ASP.NET Web API ułatwia manipulowania komunikat odpowiedzi HTTP. Oto ulepszone implementacji:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Należy zauważyć, że typ zwracany metody jest teraz **HttpResponseMessage**. Zwracając **HttpResponseMessage** zamiast produktu, można sterować szczegóły komunikatu odpowiedzi HTTP, w tym kod stanu i nagłówek Location.

**CreateResponse** metoda tworzy **HttpResponseMessage** i automatycznie zapisuje zserializowana reprezentacja obiektu produktu w treści fo komunikat odpowiedzi.

> [!NOTE]
> W tym przykładzie nie można zweryfikować `Product`. Aby uzyskać informacje o weryfikacji modelu, zobacz [weryfikacji modelu w ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).


## <a name="updating-a-resource"></a>Aktualizowanie zasobu

Aktualizowanie produktu za pomocą PUT jest bezpośrednia:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

Nazwa metody rozpoczyna się od &quot;Put... &quot;, więc interfejsu API sieci Web dopasowuje je do żądania PUT. Metoda przyjmuje dwa parametry: identyfikator produktu i zaktualizowane produktu. *Identyfikator* parametru jest pobierana z ścieżka identyfikatora URI i *produktu* parametru jest przeprowadzić deserializacji treści żądania. Domyślnie przez platformę ASP.NET Web API przyjmuje typów prostych parametru na podstawie trasy i typów złożonych z treści żądania.

## <a name="deleting-a-resource"></a>Usunięcie zasobu

Aby usunąć resourse, zdefiniuj metodę "Usuń...".

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Jeśli żądanie usunięcia zakończy się powodzeniem, może on zwrócić stanu 200 (OK) z treści jednostki opisujące stan; Stan 202 (zaakceptowane), jeśli usunięcie jest nadal oczekujące; Stan lub 204 (bez zawartości) z nie treści jednostki. W takim przypadku `DeleteProduct` metoda ma `void` typ zwracany, więc interfejsu API sieci Web platformy ASP.NET automatycznie przekłada to stan kod 204 (bez zawartości).
