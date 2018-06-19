---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Wysyłanie danych formularza HTML zakodowanych w składniku ASP.NET Web API: Przekaż i wieloczęściowej wiadomości MIME pliku | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 331d0e520a1fd8ec84aecd09a9c9e6d286c5893b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28040145"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Wysyłanie danych formularza HTML zakodowanych w składniku ASP.NET Web API: Przekaż i wieloczęściowej wiadomości MIME pliku
====================
przez [Wasson Jan](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Część 2: Przekazywanie pliku i wieloczęściowej wiadomości MIME

Ten samouczek pokazuje sposób przekazywania plików do interfejsu API sieci web. Zawiera również opis sposobu przetwarzania danych wieloczęściowej wiadomości MIME.

> [!NOTE]
> [Pobieranie ukończone projektu](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).


Oto przykład formularza HTML do przekazywania pliku:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Ten formularz zawiera kontrolki wprowadzania tekstu i kontrolki wprowadzania w formie pliku. Gdy formularz zawiera kontrolki wprowadzania w formie pliku, **typ kodowania** atrybut powinien mieć zawsze &quot;multipart/dane formularza&quot;, który określa, czy formularz zostanie wysłany jako wieloczęściowej wiadomości MIME.

Format wieloczęściowej wiadomości MIME jest łatwiej zrozumieć patrząc na przykład żądania:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Ten komunikat jest podzielony na dwa *części*, po jednej dla każdego formantu formularza. Granice części są oznaczone wiersze rozpoczynające się kreski.

> [!NOTE]
> Granic część zawiera składnik losowe (&quot;41184676334&quot;) aby upewnić się, że ciąg granic nie przypadkowo pojawia się w części komunikatu.


Każda część zawiera co najmniej jeden nagłówek, a następnie zawartości części.

- Nagłówek Content-Disposition zawiera nazwę formantu. W przypadku plików także zawiera nazwę pliku.
- Nagłówek Content-Type opisano w części danych. Jeśli ten nagłówek zostanie pominięty, wartość domyślna to zwykły tekst.

W poprzednim przykładzie użytkownik przekazany plik o nazwie GrandCanyon.jpg, o typ zawartości image/jpeg; wartość pola tekstowego &quot;wakacje&quot;.

## <a name="file-upload"></a>Przekazywanie pliku

Teraz Przyjrzyjmy się kontrolera interfejsu API sieci Web służącą do odczytywania plików z wieloczęściowej wiadomości MIME. Kontroler odczyta pliki asynchronicznie. Interfejs API sieci Web obsługuje akcje asynchroniczne przy użyciu [model programowania opartego na zadaniach](https://msdn.microsoft.com/library/dd460693.aspx). Po pierwsze, Oto kod, jeśli ma być przeznaczona dla platformy .NET Framework 4.5, która obsługuje **async** i **await** słów kluczowych.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Zwróć uwagę, że akcji kontrolera nie przyjmuje żadnych parametrów. Wynika to z treści żądania wewnątrz działania, możemy przetworzyć bez wywoływania elementu formatującego typu nośnika.

**IsMultipartContent** metoda sprawdza, czy żądanie zawiera wieloczęściowej wiadomości MIME. W przeciwnym razie kontrolera zwraca kod stanu HTTP 415 (nieobsługiwany typ nośnika).

**MultipartFormDataStreamProvider** klasy jest obiekt pomocnika, który przydziela strumieni plików przekazanych plików. Aby odczytać wiadomości wieloczęściowej MIME, należy wywołać **ReadAsMultipartAsync** metody. Ta metoda wyodrębnia wszystkie części komunikatu i zapisuje je w strumieni dostarczonych przez **MultipartFormDataStreamProvider**.

Po zakończeniu działania metody, można uzyskać informacji o plikach z **FileData** właściwość, która jest kolekcją z **MultipartFileData** obiektów.

- **MultipartFileData.FileName** jest nazwą pliku lokalnego na serwerze, w którym został zapisany plik.
- **MultipartFileData.Headers** zawiera nagłówek części (*nie* nagłówek żądania). Umożliwia to dostęp do zawartości\_nagłówki typu zawartości i dyspozycji.

Jak wynika z nazwy, **ReadAsMultipartAsync** jest metody asynchronicznej. Aby wykonywać zadania po zakończeniu metody, należy użyć [zadania kontynuacji](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) lub **await** — słowo kluczowe (.NET 4.5).

W tym miejscu jest wersja programu .NET Framework 4.0 poprzedni kod:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Dane kontroli odczytu formularza

Formularza HTML, który można wcześniej miała kontrolki wprowadzania tekstu.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Można pobrać wartości formantu z **FormData** właściwość **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** jest **NameValueCollection** zawierający pary nazwa/wartość dla formantów formularza. Nazwa kolekcji może zawierać zduplikowanych kluczy. Należy wziąć pod uwagę tego formularza:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

Treść żądania może wyglądać następująco:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

W takim przypadku **FormData** kolekcja zawiera następujące pary klucz wartość:

- podróży: przesyłania danych
- opcje: nonstop
- opcje: dat
- stanowisko: okna
