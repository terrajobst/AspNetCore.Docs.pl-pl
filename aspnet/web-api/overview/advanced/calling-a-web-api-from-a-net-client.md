---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Wywoływanie interfejsu API sieci Web z klienta programu .NET (C#) | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: riande
ms.date: 11/24/2017
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: be237bee43bc5e32939cb0b3e0948fd8b35bd1eb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752468"
---
<a name="call-a-web-api-from-a-net-client-c"></a>Wywoływanie interfejsu API sieci Web z klienta programu .NET (C#)
====================
przez [Mike Wasson](https://github.com/MikeWasson) i [Rick Anderson](https://twitter.com/RickAndMSFT)

[Pobieranie ukończone projektu](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample). [Instrukcje pobierania](/aspnet/core/tutorials/#how-to-download-a-sample). 

W tym samouczku przedstawiono sposób wywoływania interfejsu API sieci web z poziomu aplikacji .NET przy użyciu [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

W tym samouczku aplikacja kliencka jest zapisywane, zużywa następujący internetowy interfejs API:

| Akcja | Metoda HTTP | Względny identyfikator URI |
| --- | --- | --- |
| Pobieranie produktu według Identyfikatora | POBIERZ | / InterfejsAPI/produkty/*identyfikator* |
| Tworzenie nowego produktu | POST | / api/produktów |
| Aktualizacja produktu | PUT | / InterfejsAPI/produkty/*identyfikator* |
| Usuwanie produktu | DELETE | / InterfejsAPI/produkty/*identyfikator* |

Aby dowiedzieć się, jak zaimplementować ten interfejs API za pomocą interfejsu API sieci Web platformy ASP.NET, zobacz [Tworzenie internetowego interfejsu API tej operacji CRUD obsługuje](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Dla uproszczenia aplikacji klienckiej w tym samouczku jest aplikacją konsoli Windows. **HttpClient** jest również obsługiwana dla aplikacji Windows Phone i Windows Store. Aby uzyskać więcej informacji, zobacz [pisanie kodu klienta interfejsu API sieci Web dla wielu platform przy użyciu przenośnych bibliotek](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Tworzenie aplikacji konsoli

W programie Visual Studio Utwórz nową aplikację konsoli Windows o nazwie **HttpClientSample** i wklej następujący kod:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

Powyższy kod jest aplikacją kliencką ukończone.

`RunAsync` Przebiegi i bloki przed zakończeniem tego procesu. Większość **HttpClient** metody są asynchroniczne, ponieważ wykonują we/wy sieci. Wszystkie zadania asynchroniczne są wykonywane tylko wewnątrz `RunAsync`. Zwykle aplikacja nie blokuje wątku głównego, ale ta aplikacja nie zezwala na interakcji.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Zainstaluj biblioteki klienta interfejsu API sieci Web

Użyj Menedżera pakietów NuGet można zainstalować pakietu biblioteki klienta interfejsu API sieci Web.

Z **narzędzia** menu, wybierz opcję **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**. W konsoli Menedżera pakietów (PMC), wpisz następujące polecenie:

`Install-Package Microsoft.AspNet.WebApi.Client`

Poprzednie polecenie dodaje następujące pakiety NuGet do projektu:

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Program Json.NET to popularne środowisko JSON o wysokiej wydajności dla platformy .NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Dodawanie klasy modelu

Sprawdź `Product` klasy:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Ta klasa pasuje do modelu danych, używane przez internetowy interfejs API. Aplikacja może używać **HttpClient** odczytać `Product` wystąpienie z odpowiedzi HTTP. Aplikacja nie ma do pisania kodu deserializacji.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Tworzenie i Inicjowanie klasy HttpClient

Sprawdź statycznej **HttpClient** właściwości:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient** jest przeznaczony do jednorazowego utworzenia ich i ponownie używane w całym cyklu życia aplikacji. Następujące warunki mogą spowodować **SocketException** błędów:

* Tworzenie nowego **HttpClient** wystąpienia na żądanie.
* Serwer z dużym obciążeniem.

Tworzenie nowego **HttpClient** wystąpienia na żądanie może wyczerpać dostępne gniazda.

Poniższy kod inicjuje **HttpClient** wystąpienie:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

Powyższy kod:

* Ustawia podstawowy identyfikator URI dla żądań HTTP. Zmień numer portu na port używany w aplikacji serwera. Aplikacja nie będzie działać, chyba że używany jest port dla aplikacji serwera.
* Ustawia nagłówek Accept do "application/json". Ustawienie tego pliku nagłówkowego nakazuje serwerowi na wysyłanie danych w formacie JSON.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Wyślij żądanie Pobierz do pobrania zasobu

Poniższy kod wysyła żądanie pobrania produktu:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

**GetAsync** metoda wysyła żądanie HTTP GET. Po ukończeniu metody, zwraca **obiektu HttpResponseMessage** zawierający odpowiedzi HTTP. Jeśli kod stanu w odpowiedzi jest kod powodzenia, treść odpowiedzi zawiera reprezentację JSON produktu. Wywołaj **ReadAsAsync** do ładunku JSON do zdeserializowania `Product` wystąpienia. **ReadAsAsync** metoda jest asynchroniczne, ponieważ treść odpowiedzi mogą być dowolnie duże.

**HttpClient** nie zgłasza wyjątku, gdy odpowiedź HTTP zawiera kod błędu. Zamiast tego **IsSuccessStatusCode** właściwość **false** Jeśli stan jest kod błędu. Jeśli wolisz traktować kody błędów HTTP jako wyjątki, wywołania [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) na obiekcie odpowiedzi. `EnsureSuccessStatusCode` zgłasza wyjątek, jeśli kod stanu mieści się poza zakresem 200&ndash;299. Należy pamiętać, że **HttpClient** może generować wyjątki z innych przyczyn &mdash; na przykład, jeśli upłynie limit czasu żądania.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Programy formatujące typy nośnika do deserializacji

Gdy **ReadAsAsync** jest wywoływana bez parametrów, używa domyślny zestaw *programy formatujące multimedia* do odczytu treści odpowiedzi. Domyślne elementy formatujące obsługi formatu JSON, XML i dane zakodowane jako adres url formularza.

Zamiast używania elementy formatujące domyślne, możesz podać listę programów formatujących do **ReadAsAsync** metody.  Korzystanie z listy elementów formatujących jest przydatna, jeśli element formatujący typu nośnika niestandardowego:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Aby uzyskać więcej informacji, zobacz [programy formatujące multimedia w programie ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Wysłanie żądania POST do utworzenia zasobu

Poniższy kod wysyła żądanie POST, która zawiera `Product` wystąpienia w formacie JSON:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

**PostAsJsonAsync** metody:

* Serializuje obiekt do formatu JSON.
* Wysyła żądanie POST ładunek JSON.

Jeśli żądanie zakończy się powodzeniem:

* Powinien on zwrócić odpowiedź 201 (utworzono).
* Odpowiedź powinna zawierać adres URL zasobów utworzonych w nagłówku Location.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Wysyła żądanie PUT, aby zaktualizować zasobu

Poniższy kod wysyła żądanie PUT do aktualizacji produktu:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

**PutAsJsonAsync** metoda działa jak **PostAsJsonAsync**, z tą różnicą, że wysyła żądanie PUT zamiast wpisu.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Trwa wysyłanie żądania usunięcia, można usunąć zasobu

Poniższy kod wysyła żądanie usunięcia, można usunąć produktu:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Żądanie usunięcia takich jak GET, nie ma treści żądania. Nie potrzebujesz określić format JSON lub XML za pomocą usuwania.

## <a name="test-the-sample"></a>Test próbki

Aby przetestować aplikację klienta:

1. [Pobierz](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) i uruchamianie aplikacji serwera. [Instrukcje pobierania](/aspnet/core/tutorials/#how-to-download-a-sample). Sprawdź, czy działa aplikacja serwera. Aby uzyskać exaxmple `http://localhost:64195/api/products` powinna zwrócić listę produktów.
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
