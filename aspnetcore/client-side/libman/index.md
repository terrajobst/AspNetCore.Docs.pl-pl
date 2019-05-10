---
title: Biblioteka klienta przejęcia w programie ASP.NET Core za pomocą LibMan
author: scottaddie
description: Dowiedz się, jak zainstalować zasoby biblioteki po stronie klienta w projektach programu ASP.NET Core za pomocą Menedżera biblioteki (LibMan).
ms.author: scaddie
ms.custom: mvc
ms.date: 08/14/2018
uid: client-side/libman/index
ms.openlocfilehash: a6ff0cc3342cfac74739387aa17046ed5050232f
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64900151"
---
# <a name="client-side-library-acquisition-in-aspnet-core-with-libman"></a>Biblioteka klienta przejęcia w programie ASP.NET Core za pomocą LibMan

Przez [Scott Addie](https://twitter.com/Scott_Addie)

Menedżer biblioteki (LibMan) jest narzędziem przejęcia biblioteki lekki, po stronie klienta. LibMan pobiera popularnych bibliotek i struktur, z systemu plików lub [sieci dostarczania zawartości (CDN)](https://wikipedia.org/wiki/Content_delivery_network). Obsługiwane CDN obejmują [CDNJS](https://cdnjs.com/) i [unpkg](https://unpkg.com/#/). Pliki wybranej biblioteki są pobierane i umieszczane w odpowiedniej lokalizacji w projekcie platformy ASP.NET Core.

## <a name="libman-use-cases"></a>LibMan przypadki użycia

LibMan oferuje następujące korzyści:

* Pobierane są pliki biblioteki, które są potrzebne.
* Dodatkowe narzędzia, takie jak [Node.js](https://nodejs.org), [npm](https://www.npmjs.com), i [WebPack](https://webpack.js.org), nie jest konieczne w celu uzyskania podzbiór plików w bibliotece.
* Pliki można umieścić w określonej lokalizacji, bez konieczności uciekania się do zadań kompilacji lub kopiowania plików ręczne.

Aby uzyskać więcej informacji na temat korzyści firmy LibMan Obejrzyj [nowoczesnych frontonu sieci web development w programie Visual Studio 2017: LibMan segment](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s).

LibMan jest system zarządzania pakietami. Jeśli już używasz Menedżera pakietów, takich jak npm lub [yarn](https://yarnpkg.com), dalej. LibMan nie został opracowany, aby zastąpić tych narzędzi.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:client-side/libman/libman-vs>
* <xref:client-side/libman/libman-cli>
* [Repozytorium LibMan GitHub](https://github.com/aspnet/LibraryManager)
