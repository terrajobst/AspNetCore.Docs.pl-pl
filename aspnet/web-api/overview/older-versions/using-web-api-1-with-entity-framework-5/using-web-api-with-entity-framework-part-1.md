---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Część 1: Omówienie i tworzenie projektu | Dokumentacja firmy Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: 0540f3142d73fef616e30544bb1130b75c0bb436
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816308"
---
<a name="part-1-overview-and-creating-the-project"></a>Część 1: Omówienie i tworzenie projektu
====================
przez [Mike Wasson](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework to platforma mapowania obiektowo/relacyjny. Obiektów domeny w kodzie jest on mapowany do jednostek w relacyjnej bazie danych. W większości przypadków nie masz już martwić się o warstwy bazy danych, ponieważ Entity Framework zajmuje się go dla Ciebie. Twój kod manipuluje obiektami, a zmiany zostaną utrwalone w bazie danych.

## <a name="about-the-tutorial"></a>Samouczek — informacje

W tym samouczku utworzysz aplikację sklepu proste. Istnieją dwie główne części aplikacji. Normalne użytkownicy mogą przeglądać produktów i tworzenie zamówień:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Administratorzy mogą tworzyć, usuń lub Edytuj produktów:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Umiejętności, których dowiesz się

Oto, dowiesz się:

- Jak używać programu Entity Framework za pomocą interfejsu API sieci Web platformy ASP.NET.
- Jak korzystać z użyciem knockout.js do tworzenia dynamicznego interfejsu użytkownika klienta.
- Jak używać uwierzytelniania formularzy z interfejsu API sieci Web do uwierzytelniania użytkowników.

Mimo że w tym samouczku jest niezależny, warto najpierw przeczytać następujące samouczki:

- [Usługi pierwszego wzorca ASP.NET Web API](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Tworzenie internetowego interfejsu API obsługującego operacje CRUD](../creating-a-web-api-that-supports-crud-operations.md)

Pewną wiedzę na temat [platformy ASP.NET MVC](../../../../mvc/index.md) jest również przydatna.

## <a name="overview"></a>Omówienie

Na wysokim poziomie poniżej przedstawiono architekturę aplikacji:

- ASP.NET MVC wygeneruje stron HTML dla klienta.
- ASP.NET Web API udostępnia operacje CRUD na danych (produkty i zamówienia).
- Entity Framework tłumaczy modeli języka C# używane przez interfejs API sieci Web do jednostek bazy danych.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

Na poniższym diagramie przedstawiono, jak obiekty domeny są reprezentowane w różnych warstwach aplikacji: Warstwa bazy danych, model obiektów, a na końcu formatu o komunikacji sieciowej, który jest używany do przesyłania danych do klienta za pośrednictwem protokołu HTTP.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

Można utworzyć projekt samouczka przy użyciu programu Visual Web Developer Express lub pełnej wersji programu Visual Studio.

Z **Start** kliknij **nowy projekt**.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz opcję **Web**. Na liście szablonów projektu wybierz **aplikacji sieci Web programu ASP.NET MVC 4**. Nadaj projektowi nazwę "ProductStore", a następnie kliknij przycisk **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

W **nowego projektu programu ASP.NET MVC 4** okno dialogowe, wybierz opcję **aplikacji internetowej** i kliknij przycisk **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

Szablon "Aplikacji internetowej" umożliwia utworzenie aplikacji platformy ASP.NET MVC, która obsługuje uwierzytelnianie formularzy. Jeśli uruchomisz aplikację teraz, jest już niektóre funkcje:

- Nowi użytkownicy mogą zarejestrować, klikając link "Register" w prawym górnym rogu.
- Zarejestrowani użytkownicy mogą się zalogować, klikając link "Zaloguj".

Informacje o członkostwie są utrwalane w bazie danych, która zostanie utworzona automatycznie. Aby uzyskać więcej informacji na temat uwierzytelniania formularzy we wzorcu ASP.NET MVC, zobacz [Instruktaż: używanie uwierzytelniania formularzy we wzorcu ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Zaktualizuj plik CSS

Ten krok jest kosmetycznych, ale spowoduje to, że strony renderowania, takich jak wcześniej zrzutów ekranu.

W Eksploratorze rozwiązań rozwiń folder zawartości, a następnie otwórz plik o nazwie pliku Site.css. Dodaj następujące style CSS:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [Next](using-web-api-with-entity-framework-part-2.md)
