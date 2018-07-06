---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Wysyłanie danych formularza HTML we wzorcu ASP.NET Web API: przekazywanie pliku i wieloczęściowej wiadomości MIME | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/21/2012
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: adaa59b47cb16def5b423181ca06729d43cd77de
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804348"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Wysyłanie danych formularza HTML we wzorcu ASP.NET Web API: przekazywanie pliku i wieloczęściowej wiadomości MIME
====================
przez [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Część 2: Przekazywanie pliku i wieloczęściowej wiadomości MIME

W tym samouczku pokazano, jak przekazać pliki do internetowego interfejsu API. Opisuje ona również, jak do przetwarzania danych wieloczęściowej wiadomości MIME.

> [!NOTE]
> [Pobieranie ukończone projektu](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).


Oto przykład z formularza HTML służącego do przekazywania pliku:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Ten formularz zawiera kontrolkę wprowadzania tekstu formanty i pliku wejściowego. Gdy formularza zawiera kontrolki wprowadzania w pliku **typ kodowania** atrybut powinien mieć zawsze &quot;multipart/formularza data&quot;, która określa, że formularz zostanie wysłana jako wieloczęściowej wiadomości MIME.

Format wieloczęściowej wiadomości MIME jest łatwiej zrozumieć, analizując przykładowe żądanie:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Ten komunikat jest podzielona na dwie *części*, jeden dla każdego formantu formularza. Część granice są oznaczone wiersze rozpoczynające się z kreskami.

> [!NOTE]
> Granic część zawiera składnik losowy (&quot;41184676334&quot;) aby upewnić się, że ciąg granic nie przypadkowo pojawia się w części komunikatu.


Każda część zawiera jeden lub więcej nagłówków, następuje zawartości części.

- Nagłówek Content-Disposition zawiera nazwę formantu. Dla plików także zawiera nazwę pliku.
- Nagłówek Content-Type opisano je w części. W przypadku pominięcia tego pliku nagłówkowego, wartość domyślna to text/plain.

W poprzednim przykładzie użytkownik przekazał plik o nazwie GrandCanyon.jpg, typ zawartości image/jpeg; wartość wprowadzania tekstu &quot;wakacje&quot;.

## <a name="file-upload"></a>Przekazywanie pliku

Teraz Przyjrzyjmy się kontrolera interfejsu API sieci Web, która odczytuje pliki z wieloczęściowej wiadomości MIME. Kontroler odczyta pliki asynchronicznie. Internetowy interfejs API obsługuje akcje asynchroniczne przy użyciu [modelu programowania opartego na zadaniach](https://msdn.microsoft.com/library/dd460693.aspx). Po pierwsze, Oto kod, jeśli są przeznaczone dla .NET Framework 4.5, która obsługuje **async** i **await** słów kluczowych.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Należy zauważyć, że akcji kontrolera nie przyjmuje żadnych parametrów. Wynika to z treści żądania w akcji, możemy przetworzyć bez wywoływania elementu formatującego typu nośnika.

**IsMultipartContent** metoda sprawdza, czy żądanie zawiera wieloczęściowej wiadomości MIME. W przeciwnym razie ten kontroler zwraca kod stanu HTTP 415 (nieobsługiwany typ nośnika).

**MultipartFormDataStreamProvider** klasa jest obiektem pomocnika, który przydziela strumieni plików przekazywanych plików. Aby odczytać wiadomości wieloczęściowej MIME, należy wywołać **ReadAsMultipartAsync** metody. Ta metoda wyodrębnia wszystkie części komunikatu i zapisuje je do strumieni, dostarczone przez **MultipartFormDataStreamProvider**.

Po zakończeniu działania metody, można uzyskać informacji o plikach z **FileData** właściwość, która jest kolekcją z **MultipartFileData** obiektów.

- **MultipartFileData.FileName** jest nazwą pliku lokalnego na serwerze, w którym plik został zapisany.
- **MultipartFileData.Headers** zawiera nagłówek części (*nie* nagłówek żądania). Umożliwia to dostęp do zawartości\_nagłówki dyspozycji i Content-Type.

Jak sugeruje nazwa, **ReadAsMultipartAsync** jest metodą asynchroniczną. Do wykonywania pracy po zakończeniu metody, należy użyć [zadanie kontynuacji](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) lub **await** — słowo kluczowe (.NET 4.5).

Oto wersja programu .NET Framework 4.0 poprzedni kod:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Odczytywania danych kontrolki formularza

Formularza HTML, wcześniej pokazujący miał kontrolki wprowadzania tekstu.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Można uzyskać wartości kontrolki z **pobranie** właściwość **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**Pobranie** jest **elementu NameValueCollection** zawierający pary nazwa/wartość dla kontrolek formularza. Kolekcja może zawierać zduplikowane klucze. Należy wziąć pod uwagę tego formularza:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

Treść żądania może wyglądać następująco:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

W takim przypadku **pobranie** kolekcja będzie zawierać następujące pary klucz/wartość:

- podróży: dwustronnej konwersji
- opcje: nonstop
- opcje: dat
- stanowisko: okna
