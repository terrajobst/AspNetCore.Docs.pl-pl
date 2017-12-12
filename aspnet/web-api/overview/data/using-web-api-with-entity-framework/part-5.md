---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: "Tworzenie obiektów Transfer danych (DTOs) | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: e179dcd52200734a45c84b3c7c3069a4727904c1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="create-data-transfer-objects-dtos"></a>Tworzenie obiektów Transfer danych (DTOs)
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://github.com/MikeWasson/BookService)

Prawo teraz naszych interfejsu API sieci web udostępnia jednostki bazy danych do klienta. Klient odbiera danych, która mapuje bezpośrednio do tabel bazy danych. Jednak, że nie zawsze jest dobrym rozwiązaniem. Czasami chcesz zmienić kształt danych, który możesz wysłać do klienta. Na przykład możesz chcieć:

- Usuń odwołania cykliczne (zobacz poprzedniej sekcji).
- Ukryj konkretnej właściwości, które klienci nie powinni do wyświetlenia.
- Pominąć niektóre właściwości, aby zmniejszyć rozmiar ładunku.
- Spłaszczanie wykresów obiektów, zawierających zagnieżdżone obiekty, aby były wygodniejsze dla klientów.
- Unikaj "zbyt księgowej" luk w zabezpieczeniach. (Zobacz [sprawdzania poprawności modelu](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) omówienie publikowanie nadmierne.)
- Oddziel z warstwy usług z warstwą bazy danych.

Aby to zrobić, można zdefiniować *obiektu transferu danych* (DTO). DTO jest obiekt, który definiuje sposób dane będą wysyłane przez sieć. Zobaczmy, jak to zrobić z jednostką książki. W folderze modeli Dodaj dwie klasy DTO:

[!code-csharp[Main](part-5/samples/sample1.cs)]

`BookDetailDTO` Klasa zawiera wszystkie właściwości modelu książki, z wyjątkiem `AuthorName` jest ciągiem, który będzie przechowywana nazwa autora. `BookDTO` Klasa zawiera podzbiór właściwości z `BookDetailDTO`.

Następnie zastąp te dwie metody GET w `BooksController` klasy z wersjami, które zwracają DTOs. Użyjemy LINQ **wybierz** instrukcji, aby przekonwertować z jednostek książki DTOs.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Oto SQL generowane przez nowy `GetBooks` metody. Widać, że EF tłumaczy LINQ **wybierz** w instrukcji SQL SELECT.

[!code-sql[Main](part-5/samples/sample3.sql)]

Na koniec zmodyfikuj `PostBook` metodę, aby zwrócić DTO.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> W tym samouczku będziemy podczas konwertowania na DTOs ręcznie w kodzie. Innym rozwiązaniem jest korzystanie z biblioteki, takich jak [AutoMapper](http://automapper.org/) automatycznie obsługująca konwersji.

>[!div class="step-by-step"]
[Poprzednie](part-4.md)
[dalej](part-6.md)
