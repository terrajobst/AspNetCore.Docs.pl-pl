---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: "Część 1: Omówienie i tworzenia projektu | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: 47af34c72f1959756f5d68e0e80052e700c7b19c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="part-1-overview-and-creating-the-project"></a>Część 1: Omówienie i tworzenia projektu
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework jest obiekt/relacyjne framework mapowania. Mapowania obiektów domeny w kodzie jednostek relacyjnej bazy danych. W większości przypadków nie masz martwić się o warstwie bazy danych, ponieważ Entity Framework zajmuje się go dla Ciebie. Kod manipuluje obiektami, a zmiany są umieszczone w bazie danych.

## <a name="about-the-tutorial"></a>Dotyczące samouczka

W tym samouczku utworzysz aplikacją ze sklepu proste. Istnieją dwie główne części aplikacji. Normalne użytkownicy mogą wyświetlać produktów i tworzenie zleceń:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Administratorzy mogą tworzyć, usuwać lub edytować produktów:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Dowiesz się umiejętności

Oto dowiesz się:

- Jak korzystać z interfejsu API sieci Web platformy ASP.NET Entity Framework.
- Jak używać knockout.js do tworzenia dynamicznego interfejsu użytkownika klienta.
- Jak używać uwierzytelniania formularzy z interfejsu API sieci Web do uwierzytelniania użytkowników.

Mimo że w tym samouczku są niezależne, warto najpierw przeczytać następujące samouczki:

- [Pierwszy ASP.NET interfejsu API sieci Web](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Tworzenie składnika Web API obsługującego operacje CRUD](../creating-a-web-api-that-supports-crud-operations.md)

Niektóre znajomość [ASP.NET MVC](../../../../mvc/index.md) jest również przydatne.

## <a name="overview"></a>Omówienie

Na wysokim poziomie Oto architektury aplikacji:

- ASP.NET MVC wygeneruje stron HTML dla klienta.
- Interfejs API sieci Web ASP.NET udostępnia operacje CRUD na danych (produktów i zamówienia).
- Entity Framework tłumaczy modeli C# używanych przez interfejs API sieci Web do bazy danych jednostki.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

Na poniższym diagramie przedstawiono, jak obiektów domeny znajdują się w różnych warstwach aplikacji: Warstwa bazy danych, model obiektów, a na końcu format danych przesyłanych w sieci, który jest używany do przesyłania danych do klienta za pośrednictwem protokołu HTTP.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

Można utworzyć projekt samouczka przy użyciu programu Visual Web Developer Express lub pełnej wersji programu Visual Studio.

Z **Start** kliknij przycisk **nowy projekt**.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz pozycję **Web**. Na liście szablony projektów, wybierz **aplikacji sieci Web programu ASP.NET MVC 4**. Nazwij projekt "ProductStore", a następnie kliknij przycisk **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

W **nowy projekt programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej** i kliknij przycisk **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

Szablon "Aplikacji internetowej" służy do tworzenia aplikacji platformy ASP.NET MVC, który obsługuje uwierzytelnianie formularzy. Jeśli uruchomisz aplikację teraz już niektóre funkcje:

- Nowi użytkownicy mogą zarejestrować, klikając łącze "Register" w prawym górnym rogu.
- Zarejestrowani użytkownicy może się zalogować, klikając łącze "Zaloguj".

Informacje o członkostwie jest zachowywane w bazie danych, który jest tworzony automatycznie. Aby uzyskać więcej informacji na temat uwierzytelniania formularzy w programie ASP.NET MVC, zobacz [wskazówki: używanie uwierzytelniania formularzy w programie ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Aktualizowanie pliku CSS

Ten krok jest bardzo drobny, ale spowoduje to, że strony renderowania jak wcześniej zrzutów ekranu.

W Eksploratorze rozwiązań rozwiń folder zawartości i Otwórz plik o nazwie Site.css. Dodaj następujące style CSS:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

>[!div class="step-by-step"]
[Next](using-web-api-with-entity-framework-part-2.md)
