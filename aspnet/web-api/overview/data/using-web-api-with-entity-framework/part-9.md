---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Dodaj nowy element do bazy danych | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 5845c092c4d7aee12b33b3f0a49c0e944c0fb9aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868376"
---
<a name="add-a-new-item-to-the-database"></a>Dodaj nowy element do bazy danych
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://github.com/MikeWasson/BookService)

W tej sekcji zostaną dodane przez użytkowników w celu utworzenia nowej książki. W app.js Dodaj następujący kod do modelu widoku:

[!code-javascript[Main](part-9/samples/sample1.js)]

W Index.cshtml Zastąp następujący kod:

[!code-html[Main](part-9/samples/sample2.html)]

Z:

[!code-html[Main](part-9/samples/sample3.html)]

Ten kod znaczników tworzy formularz przesyłania nowych autora. Wartości na liście rozwijanej autora jest powiązany z danymi `authors` widocznych w modelu widoku. Do innych wejść formularza, wartości jest powiązany z danymi `newBook` właściwości modelu widoku.

Obsługa przesyłania formularza jest powiązana z `addBook` funkcji:

[!code-html[Main](part-9/samples/sample4.html)]

`addBook` Funkcja odczytuje bieżące wartości dane wejściowe formularza powiązane z danymi można utworzyć obiektu JSON. A następnie go zapisuje obiekt JSON do `/api/books`.

> [!div class="step-by-step"]
> [Poprzednie](part-8.md)
> [dalej](part-10.md)
