---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Dodaj nowy element do bazy danych | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 36251ba907a6f580b63f0fded0591c26b6ff879e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818511"
---
<a name="add-a-new-item-to-the-database"></a>Dodaj nowy element do bazy danych
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](https://github.com/MikeWasson/BookService)

W tej sekcji dodasz możliwości dla użytkowników w celu utworzenia nowej książki. W app.js Dodaj następujący kod do modelu widoku:

[!code-javascript[Main](part-9/samples/sample1.js)]

W Index.cshtml Zastąp następujący kod:

[!code-html[Main](part-9/samples/sample2.html)]

Za pomocą:

[!code-html[Main](part-9/samples/sample3.html)]

Ten kod znaczników tworzy formularz przesyłania nowego autora. Wartości na liście rozwijanej autor jest powiązany z danymi `authors` zauważalne w modelu widoku. Dla innych nakładów formularza, wartości są powiązane z danymi do `newBook` właściwości modelu widoku.

Obsługa przesyłania formularza jest powiązana z `addBook` funkcji:

[!code-html[Main](part-9/samples/sample4.html)]

`addBook` Funkcja odczytuje bieżące wartości składników powiązane z danymi formularza można utworzyć obiektu JSON. A następnie wysyła on żądanie POST obiekt JSON służący do `/api/books`.

> [!div class="step-by-step"]
> [Poprzednie](part-8.md)
> [dalej](part-10.md)
