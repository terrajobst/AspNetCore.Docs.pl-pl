---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Wywoływanie interfejsu API sieci Web z klienta programu .NET (C#) | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: fdb74b0eb74ce7f387f49a0b25ceebd3fc389da9
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/26/2018
ms.locfileid: "32006160"
---
<a name="call-a-web-api-from-a-net-client-c"></a>Wywoływanie interfejsu API sieci Web z klienta programu .NET (C#)
====================
przez [Wasson Jan](https://github.com/MikeWasson) i [Rick Anderson](https://twitter.com/RickAndMSFT)

[Pobieranie ukończone projektu](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample). [Instrukcje pobierania](/aspnet/core/tutorials/#how-to-download-a-sample). 

W tym samouczku przedstawiono sposób wywołania interfejsu API sieci web z poziomu aplikacji .NET przy użyciu [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

W tym samouczku aplikacji klienta są zapisywane, który wykorzystuje następujące interfejsu API sieci web:

| Akcja | Metoda HTTP | Względny identyfikator URI |
| --- | --- | --- |
| Uzyskiwanie produktu według Identyfikatora | POBIERZ | /API/produkty/*id* |
| Tworzenie nowego produktu | POST | / api/produktów |
| Aktualizacji produktu | UMIEŚĆ | /API/produkty/*id* |
| Usuwanie produktu | DELETE | /API/produkty/*id* |

Aby dowiedzieć się, jak zaimplementować ten interfejs API z interfejsu API sieci Web platformy ASP.NET, zobacz [Tworzenie składnika Web API ten obsługuje operacje CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Dla uproszczenia aplikacji klienckiej, w tym samouczku jest aplikacji konsoli systemu Windows. **HttpClient** jest również obsługiwane dla aplikacji Windows Phone i Sklep Windows. Aby uzyskać więcej informacji, zobacz [pisania kodu klienta interfejsu API sieci Web dla wielu platform przy użyciu przenośnej biblioteki](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Tworzenie aplikacji konsoli

W programie Visual Studio Utwórz nową aplikację konsoli systemu Windows o nazwie **HttpClientSample** i wklej poniższy kod:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

Poprzedni kod jest aplikacja kliencka ukończone.

`RunAsync` Uruchamia i bloków, dopóki nie ukończy. Większość **HttpClient** są metody asynchronicznej, ponieważ wykonują operacje We/Wy sieci. Wszystkie zadania asynchroniczne są wykonywane w `RunAsync`. Zwykle aplikacja nie blokuje wątku głównego, ale ta aplikacja nie zezwala interakcji.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Zainstaluj biblioteki klienta interfejsu API sieci Web

Użyj Menedżera pakietów NuGet do zainstalowania pakietu Web API Client Libraries.

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**. W konsoli Menedżera pakietów (PMC), wpisz następujące polecenie:

`Install-Package Microsoft.AspNet.WebApi.Client`

Poprzednie polecenie dodaje następujące pakiety NuGet do projektu:

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Struktury Json.NET jest popularnych framework JSON wysokiej wydajności dla platformy .NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Dodaj klasę modelu

Sprawdź `Product` klasy:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Ta klasa reprezentuje model danych używany przez interfejs API sieci web. Aplikacja może używać **HttpClient** odczytać `Product` wystąpienie z odpowiedzi HTTP. Aplikacja nie ma do pisania kodu deserializacji.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Utwórz i zainicjuj HttpClient

Sprawdź statycznych **HttpClient** właściwości:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient** jest przeznaczony do użycia raz i ponownie cały czas życia aplikacji. Poniższe warunki mogą spowodować **socketexception —** błędów:

* Tworzenie nowego **HttpClient** wystąpienia na żądanie.
* Serwerze krytycznie obciążony.

Tworzenie nowego **HttpClient** wystąpienia na żądanie można spalin dostępnych gniazd.

Poniższy kod inicjuje **HttpClient** wystąpienie:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

Poprzedni kod:

* Ustawia podstawowy identyfikator URI dla żądań HTTP. Zmień numer portu na port używany w aplikacji serwera. Aplikacja nie będzie działać, chyba że używana jest port dla aplikacji serwera.
* Ustawia nagłówek Accept do "application/json". Ustawienie tego nagłówka informuje serwer do wysyłania danych w formacie JSON.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Wyślij żądanie GET można pobrać zasobu

Poniższy kod wysyła żądanie pobrania produktu:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

**GetAsync** metoda wysyła żądanie HTTP GET. Po ukończeniu metody, zwraca **HttpResponseMessage** zawierający odpowiedzi HTTP. Jeśli kod stanu w odpowiedzi kod sukces, treść odpowiedzi zawiera reprezentacja JSON produktu. Wywołanie **ReadAsAsync** do ładunek JSON do zdeserializowania `Product` wystąpienia. **ReadAsAsync** metoda jest asynchroniczne, ponieważ treść odpowiedzi może być dowolnie dużą.

**HttpClient** nie zgłosić wyjątek, jeśli odpowiedź HTTP zawiera kod błędu. Zamiast tego **IsSuccessStatusCode** właściwość jest **false** Jeśli stan to kod błędu. Jeśli wolisz Traktuj kody błędów HTTP jako wyjątki, wywołanie [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) w obiekt odpowiedzi. `EnsureSuccessStatusCode` zgłasza wyjątek, jeśli kod stanu znajduje się poza zakresem 200&ndash;299. Należy pamiętać, że **HttpClient** można zgłaszanie wyjątków z innych powodów &mdash; na przykład, jeśli upłynie limit czasu żądania.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Programy formatujące typy nośnika do deserializacji

Gdy **ReadAsAsync** jest wywoływana bez parametrów, używa domyślnego zestawu *programy formatujące multimedia* do odczytu treści odpowiedzi. Domyślne elementy formatujące obsługuje JSON, XML i danych zakodowanych jako adres url formularza.

Zamiast domyślnego elementy formatujące, możesz podać listę programów formatujących do **ReadAsAsync** metody.  Używanie listę programów formatujących jest przydatna, jeśli element formatujący typu nośnika niestandardowego:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Aby uzyskać więcej informacji, zobacz [programy formatujące multimedia w ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Wysyła żądanie POST, aby utworzyć zasób

Poniższy kod wysyła żądanie POST, który zawiera `Product` wystąpienia w formacie JSON:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

**PostAsJsonAsync** metody:

* Serializuje obiekt do formatu JSON.
* Wysyła ładunek JSON w żądaniu POST.

Jeśli żądanie zakończy się powodzeniem:

* Powinien on zwrócić odpowiedź 201 (utworzono).
* Odpowiedź powinna zawierać adres URL zasobów utworzonej w nagłówku lokalizacji.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Wysyła żądanie PUT, aby aktualizować zasobu

Poniższy kod wysyła żądanie PUT, aby zaktualizować produkt:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

**PutAsJsonAsync** metody działa jak **PostAsJsonAsync**, ale wysyła żądanie PUT zamiast POST.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Wysyła żądanie usunięcia, aby usunąć zasób

Poniższy kod wysyła żądanie usunięcia, aby usunąć produkt:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Żądanie usunięcia takich jak GET, nie ma treści żądania. Nie trzeba określić format JSON i XML z DELETE.

## <a name="test-the-sample"></a>Testowanie próbki

Aby przetestować aplikację klienta:

1. [Pobierz](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) i uruchamianie aplikacji serwera. [Instrukcje pobierania](/aspnet/core/tutorials/#how-to-download-a-sample). Sprawdź, czy serwer aplikacji działa. Dla exaxmple `http://localhost:64195/api/products` powinien zwrócić listę produktów.
2. Ustaw podstawowy identyfikator URI dla żądań HTTP. Zmień numer portu na port używany w aplikacji serwera.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. Uruchom aplikację klienta. Są następujące wyniki:

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
