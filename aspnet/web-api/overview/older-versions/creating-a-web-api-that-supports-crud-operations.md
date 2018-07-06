---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Włączanie operacji CRUD we wzorcu ASP.NET Web API 1 | Dokumentacja firmy Microsoft
author: MikeWasson
description: W tym samouczku pokazano, jak do obsługi operacji CRUD w usłudze HTTP przy użyciu interfejsu API sieci Web platformy ASP.NET. Wersje oprogramowania używanych w samouczek Visual Studio 2012 Web AP...
ms.author: aspnetcontent
ms.date: 01/28/2012
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 4ea9d4ce9a14e77fa1a63ee82bc1447b6e41378c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814365"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Włączanie operacji CRUD we wzorcu ASP.NET Web API 1
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> W tym samouczku pokazano, jak do obsługi operacji CRUD w usłudze HTTP przy użyciu interfejsu API sieci Web platformy ASP.NET.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - Visual Studio 2012
> - Składnik Web API 1 (dotyczy również Web API 2)


Oznacza CRUD &quot;tworzenia, odczytu, aktualizacji i usuwania,&quot; służą do czterech operacji podstawowej bazy danych. Wiele usług HTTP również model operacji CRUD, za pośrednictwem REST lub interfejsów API REST podobne.

W tym samouczku utworzysz bardzo prosty internetowego interfejsu API do zarządzania listą produktów. Każdy produkt będzie zawierać nazwę, cenę i kategorii (takich jak &quot;zabawki&quot; lub &quot;sprzętu&quot;), oraz identyfikator produktu.

Produkty interfejsu API powoduje to udostępnienie następujących metod.

| Akcja | Metoda HTTP | Względny identyfikator URI |
| --- | --- | --- |
| Pobierz listę wszystkich produktów | POBIERZ | / api/produktów |
| Pobieranie produktu według Identyfikatora | POBIERZ | / InterfejsAPI/produkty/*identyfikator* |
| Pobierz produkt według kategorii | POBIERZ | produkty/api /? kategorii =*kategorii* |
| Tworzenie nowego produktu | POST | / api/produktów |
| Aktualizacja produktu | PUT | / InterfejsAPI/produkty/*identyfikator* |
| Usuwanie produktu | DELETE | / InterfejsAPI/produkty/*identyfikator* |

Należy zauważyć, że niektóre identyfikatory URI, Dołącz identyfikator produktu ścieżki. Na przykład, aby uzyskać produkt o identyfikatorze to 28, klient wysyła żądanie GET `http://hostname/api/products/28`.

### <a name="resources"></a>Resources

Produkty API definiuje identyfikatorów URI dla dwóch typów zasobów:

| Zasób | Identyfikator URI |
| --- | --- |
| Lista wszystkich produktów. | / api/produktów |
| Indywidualnych produktów. | / InterfejsAPI/produkty/*identyfikator* |

### <a name="methods"></a>Metody

Cztery główne metody HTTP (GET, PUT, POST i DELETE) mogą być mapowane na operacje CRUD w następujący sposób:

- GET pobiera reprezentację zasobu o wskazanym identyfikatorze URI. Pobierz powinien mieć żadnych efektów ubocznych, na serwerze.
- Umieść aktualizuje zasób o wskazanym identyfikatorze URI. PUT można również utworzyć nowy zasób o określonym identyfikatorze URI, jeśli serwer umożliwia klientom określić nowe identyfikatory URI. W tym samouczku interfejs API nie obsługuje tworzenia za pomocą PUT.
- POST utworzy nowy zasób. Serwer przypisuje identyfikator URI dla nowego obiektu i zwraca ten identyfikator URI jako część komunikatu odpowiedzi.
- DELETE Usuwa zasób o wskazanym identyfikatorze URI.

Uwaga: Metody PUT zastępuje jednostki w całym produkcie. Oznacza to, że klient oczekuje się, Wyślij pełną reprezentację zaktualizowanego produktu. Jeśli chcesz obsługiwać aktualizacje częściowe, metoda PATCH jest preferowana. W tym samouczku nie implementuje poprawki.

## <a name="create-a-new-web-api-project"></a>Utwórz nowy projekt interfejsu API sieci Web

Uruchom za pomocą programu Visual Studio i wybierz **nowy projekt** z **Start** strony. Lub z **pliku** menu, wybierz opcję **New** i następnie **projektu**.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz opcję **Web**. Na liście szablonów projektu wybierz **aplikacji sieci Web programu ASP.NET MVC 4**. Nadaj projektowi nazwę &quot;ProductStore&quot; i kliknij przycisk **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

W **nowego projektu programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **interfejsu API sieci Web** i kliknij przycisk **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Dodawanie modelu

A *modelu* jest obiektem, który reprezentuje dane w aplikacji. W programie ASP.NET Web API silnie typizowanych obiektów CLR można użyć jako modeli, a ich będzie automatycznie serializowany do XML lub JSON dla klienta.

Dla interfejsu API ProductStore naszych danych składa się z produktów, dlatego utworzymy nową klasę o nazwie `Product`.

Eksplorator rozwiązań nie jest jeszcze widoczny, kliknij przycisk **widoku** menu, a następnie wybierz **Eksploratora rozwiązań**. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modeli** folderu. Wybierz z menu kontekstowego, **Dodaj**, a następnie wybierz **klasy**. Nazwa klasy &quot;produktu&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Dodaj następujące właściwości do `Product` klasy.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Dodawanie repozytorium

Musimy zapisują zbiór produktów. To dobry pomysł, aby oddzielić kolekcji z naszej implementacji usługi. W ten sposób możemy zmienić magazyn zapasowy bez konieczności ponownego zapisu klasę usługi. Tego typu konstrukcji jest nazywana *repozytorium* wzorca. Rozpocznij, definiując ogólny interfejs umożliwiający repozytorium.

W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy **modeli** folderu. Wybierz **Dodaj**, a następnie wybierz **nowy element**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń węzeł języka C#. W języku C#, wybierz **kodu**. Na liście szablonów kodu Wybierz **interfejsu**. Nazwa interfejsu &quot;IProductRepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Dodaj następującą implementacją:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Teraz Dodaj klasę do folderu modeli, o nazwie &quot;ProductRepository.&quot; Ta klasa wdroży `IProductRespository` interfejsu. Dodaj następującą implementacją:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

Repozytorium przechowuje listy w lokalnej pamięci. Dotyczy to OK, aby zapoznać się z samouczkiem, ale w rzeczywistej aplikacji będzie przechowywania danych zewnętrznie, albo bazę danych lub w magazynie w chmurze. Wzorzec repozytorium ułatwi Zmień implementację później.

## <a name="adding-a-web-api-controller"></a>Dodawanie kontrolera interfejsu API sieci Web

Użytkownicy mający doświadczenie z platformą ASP.NET MVC, następnie znasz już kontrolerów. W programie ASP.NET Web API *kontrolera* to klasa, która obsługuje żądania HTTP od klienta. Kreator nowego projektu utworzone dwa kontrolery automatycznie podczas tworzenia projektu. Aby je wyświetlić, rozwiń folder kontrolerów w Eksploratorze rozwiązań.

- HomeController to tradycyjne kontrolera ASP.NET MVC. Jest odpowiedzialny za obsługi stron HTML dla witryny, a nie jest bezpośrednio związana z naszego interfejsu API sieci web.
- ValuesController jest przykład controller WebAPI.

Przejdź dalej i Usuń ValuesController, klikając prawym przyciskiem myszy plik w Eksploratorze rozwiązań i wybierając polecenie **usunąć.** Teraz Dodaj nowy kontroler, w następujący sposób:

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy folder kontrolerów. Wybierz **Dodaj** , a następnie wybierz **kontrolera**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

W **Dodaj kontroler** kreatora, nazwy kontrolera &quot;ProductsController&quot;. W **szablonu** listy rozwijanej wybierz **pusty Kontroler interfejsu API**. Następnie kliknij przycisk **Dodaj**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Nie jest niezbędne do Twojej contollers należy umieścić w folderze o nazwie kontrolerów. Nazwa folderu nie jest ważna; jest po prostu wygodny sposób organizowania plików źródłowych.


**Dodaj kontroler** Kreator utworzy plik o nazwie ProductsController.cs w folderze kontrolerów. Jeśli ten plik nie jest jeszcze otwarty, kliknij dwukrotnie plik, aby go otworzyć. Dodaj następujący kod **przy użyciu** instrukcji:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Dodaj pola zawierające **IProductRepository** wystąpienia.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> Wywoływanie `new ProductRepository()` w kontrolerze nie jest najlepszy projekt, ponieważ wiąże kontroler konkretnej implementacji `IProductRepository`. Aby uzyskać lepszym rozwiązaniem, zobacz [za pomocą mechanizmu rozpoznawania zależności dla interfejsu API sieci Web](../advanced/dependency-injection.md).


## <a name="getting-a-resource"></a>Wprowadzenie do zasobu

Interfejs API ProductStore udostępni kilka &quot;odczytu&quot; czynności, jak metod HTTP GET. Każda akcja odnoszą się do metody w `ProductsController` klasy.

| Akcja | Metoda HTTP | Względny identyfikator URI |
| --- | --- | --- |
| Pobierz listę wszystkich produktów | POBIERZ | / api/produktów |
| Pobieranie produktu według Identyfikatora | POBIERZ | / InterfejsAPI/produkty/*identyfikator* |
| Pobierz produkt według kategorii | POBIERZ | produkty/api /? kategorii =*kategorii* |

Aby uzyskać listę wszystkich produktów, należy dodać tę metodę w celu `ProductsController` klasy:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

Nazwa metody rozpoczyna się od &quot;uzyskać&quot;, więc zgodnie z Konwencją, jest on mapowany na żądania GET. Ponadto, ponieważ metoda nie ma parametrów, jest on mapowany do identyfikatora URI, który nie zawiera *&quot;identyfikator&quot;* segmentów w ścieżce.

Aby uzyskać produkt za pomocą Identyfikatora, należy dodać tę metodę w celu `ProductsController` klasy:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Ta nazwa metody również zaczyna się od &quot;uzyskać&quot;, ale metoda ma parametr o nazwie *identyfikator*. Ten parametr jest mapowany na &quot;identyfikator&quot; segmentu ścieżki identyfikatora URI. Struktury ASP.NET Web API automatycznie konwertuje identyfikator na prawidłowy typ danych (**int**) dla parametru.

Metoda GetProduct zgłasza wyjątek typu **HttpResponseException** Jeśli *identyfikator* jest nieprawidłowy. Wyjątek ten będzie można przetłumaczyć przez platformę błąd 404 (nie znaleziono).

Na koniec Dodaj metodę, aby znaleźć produkty według kategorii:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Jeśli identyfikator URI żądania zawiera ciąg zapytania, interfejs API sieci Web próbuje dopasować parametry zapytania do parametrów dla metody kontrolera. W związku z tym, identyfikator URI w postaci "interfejsu api/produktów? kategorii =*kategorii*" będzie zmapowana do tej metody.

## <a name="creating-a-resource"></a>Tworzenie zasobu

Następnie dodamy metodę `ProductsController` klasy w celu utworzenia nowego produktu. Poniżej przedstawiono prosty implementacji metody:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Należy zwrócić uwagę dwa elementy o tej metodzie:

- Nazwa metody rozpoczyna się od &quot;wpis... &quot;. Aby utworzyć nowy produkt, klient wysyła żądanie HTTP POST.
- Ta metoda przyjmuje parametr typu produktu. W interfejsie API sieci Web parametrów o złożonych typach są deserializacji z treści żądania. W związku z tym oczekujemy, że to klientowi wysłanie zserializowana reprezentacja obiektu produktu w formacie XML lub JSON.

Ta implementacja będzie działać, ale nie jest jeszcze ukończone. W idealnym przypadku prosimy o poświęcenie odpowiedzi HTTP są następujące:

- **Kod odpowiedzi:** domyślnie struktury Web API ustawia kod stanu odpowiedzi 200 (OK). Jednak zgodnie z protokołem HTTP/1.1, jeśli żądanie POST prowadzi do utworzenia zasobu, serwer powinien odpowiadać ze stanem 201 (utworzono).
- **Lokalizacja:** podczas serwera umożliwia utworzenie zasobu, powinien on zawierać identyfikator URI nowego zasobu w nagłówku Location odpowiedzi.

ASP.NET Web API można łatwo manipulować komunikatu odpowiedzi HTTP. Oto implementacja ulepszone:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Należy zauważyć, że typ zwracany metody jest teraz **obiektu HttpResponseMessage**. Zwracając **obiektu HttpResponseMessage** zamiast produktu, możemy kontrolować szczegóły komunikatu odpowiedzi HTTP, łącznie z kodem stanu i nagłówek Location.

**CreateResponse** metoda tworzy **obiektu HttpResponseMessage** i automatycznie zapisuje zserializowana reprezentacja obiektu produktu w treści fo komunikat odpowiedzi.

> [!NOTE]
> Nie można zweryfikować w tym przykładzie `Product`. Aby uzyskać informacji o weryfikacji modelu, zobacz [weryfikacji modelu programu ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).


## <a name="updating-a-resource"></a>Aktualizowanie zasobu

Aktualizowanie produktu za pomocą PUT jest proste:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

Nazwa metody rozpoczyna się od &quot;umieścić... &quot;, więc internetowego interfejsu API pasujących do żądania PUT. Ta metoda przyjmuje dwa parametry: identyfikator produktu i zaktualizowanych produktów. *Identyfikator* parametru jest pobierana z ścieżka identyfikatora URI i *produktu* parametru jest przeprowadzona z treści żądania. Domyślnie w ramach interfejsu API sieci Web platformy ASP.NET tworzy typy proste parametrów na podstawie trasy i typów złożonych z treści żądania.

## <a name="deleting-a-resource"></a>Usuwanie zasobu

Aby usunąć resourse, należy zdefiniować metodę "Usuń...".

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Jeśli żądanie usunięcia zakończy się powodzeniem, może zwrócić stanu 200 (OK) z treści jednostki opisującą stan; Stan 202 (zaakceptowano), jeśli usunięcie jest nadal oczekujące; Stan lub 204 (Brak zawartości) z bez treści jednostki. W tym przypadku `DeleteProduct` metoda ma `void` zwracany typ, więc ASP.NET Web API automatycznie przekłada to stan kod 204 (Brak zawartości).
