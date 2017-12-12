---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: "Funkcja szkieletów ASP.NET w programie Visual Studio 2013 | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "Rusztowania ASP.NET jest nową funkcją uwzględnioną w programie Visual Studio 2013."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 425983c1ffff6369276f0723a9947a411a4617eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>Funkcja szkieletów ASP.NET w programie Visual Studio 2013
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> Rusztowania ASP.NET jest nową funkcją uwzględnioną w programie Visual Studio 2013.


## <a name="overview"></a>Omówienie

Rusztowania ASP.NET to platforma generowania kodu dla aplikacji sieci Web ASP.NET. Visual Studio 2013 obejmuje generatory kodu wstępnie zainstalowane dla projektów MVC i interfejsu API sieci Web. Możesz dodać szkielet do projektu, jeśli chcesz szybko dodać kod, który wchodzi w interakcję z modelami danych. Przy użyciu funkcji szkieletów może skrócić czas tworzenia operacji standardowych danych w projekcie.

Domyślnie program Visual Studio 2013 nie obsługuje generowania kodu dla projektu formularzy sieci Web, ale za pomocą szkieletów formularzy sieci Web przez dodanie zależności MVC do projektu lub Instalowanie rozszerzenia. Poniżej przedstawiono obu podejść.

Visual Studio 2013 Update 2 (obecnie RC) zapewnia możliwość rozszerzania szkieletów ASP.NET w celu spełnienia wymagań scenariusza. Dzięki tej funkcji można utworzyć szablon szkieletów dostosowane i dodaj go do okna dialogowego Dodawanie szkieletu nowe. W dostosowany szablon należy określić kod, który jest generowany, gdy Dodawanie elementu szkieletu. Aby uzyskać więcej informacji, zobacz [tworzenie tworzenia szkieletu niestandardowe dla programu Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Wymagania wstępne

Aby korzystać z ASP.NET rusztowania, musi mieć:

- Microsoft Visual Studio 2013
- Narzędzia sieci Web Developer Tools (część instalacji programu Visual Studio 2013 domyślne)
- Platformy sieci Web ASP.NET i narzędzia 2013 (część instalacji programu Visual Studio 2013 domyślne)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Dodawanie elementu szkieletu MVC lub Web API

Aby dodać szkieletu, kliknij prawym przyciskiem myszy projekt lub folder, w ramach projektu, a następnie wybierz **Dodaj** — **nowy element szkieletu**, jak pokazano na poniższej ilustracji.

![Dodawanie elementu szkieletu](aspnet-scaffolding-overview/_static/image1.png)

Z **Dodawanie szkieletu** okna, wybierz typ szkieletu do dodania.

![Wybierz typ szkieletu](aspnet-scaffolding-overview/_static/image2.png)

**Dodaj kontroler** okna daje możliwość wybrać opcje generowania kontrolera, w tym, czy chcesz korzystać z nowych funkcji asynchronicznych z programu Entity Framework 6.

![Dodawanie kontrolera](aspnet-scaffolding-overview/_static/image3.png)

Odpowiednich klas i strony są tworzone dla danego scenariusza. Na przykład na poniższej ilustracji przedstawiono MVC kontroler i widoki, które zostały utworzone za pomocą funkcją szkieletów dla klasy modelu o nazwie filmów.

![Utworzone pliki](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Dodawanie elementu szkieletu do formularzy sieci Web

Aby dodać funkcja szkieletów, który generuje kod formularzy sieci Web, należy zainstalować rozszerzenie dla programu Visual Studio lub Dodaj zależności MVC. Poniżej przedstawiono oba podejścia, ale należy wykonać jedną z tych metod.

### <a name="web-forms-scaffolding-extension"></a>Formularze sieci Web tworzenia szkieletu rozszerzenia

Można zainstalować rozszerzenia programu Visual Studio, które umożliwiają korzystanie z projektem formularzy sieci Web szkieletów. W programie Visual Studio, wybierz **narzędzia** , a następnie **rozszerzenia i aktualizacje**. W tym oknie dialogowym wyszukiwania galerii programu Visual Studio dla **szkieletów formularzy sieci Web**.

![Zainstaluj tworzenia szkieletu formularzy sieci web](aspnet-scaffolding-overview/_static/image5.png)

Aby uzyskać więcej informacji, zobacz [szkieletów formularzy sieci Web](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>Zależności MVC

Aby dodać zależności MVC, zaznacz **Dodaj** - **nowy element szkieletu**. W oknie Dodawanie szkieletu wybierz **zależności MVC**, jak pokazano poniżej.

![Dodaj zależności MVC](aspnet-scaffolding-overview/_static/image6.png)

Dostępne są dwie opcje do tworzenia szkieletu MVC; Minimalne i pełne. W przypadku wybrania minimalny, tylko pakiety NuGet i odwołań dla platformy ASP.NET MVC zostaną dodane do projektu. Jeśli wybierzesz opcję pełne, minimalnym zależności zostaną dodane, a także wymagane pliki zawartości projektu MVC. Aby łatwo szkieletów, zaznacz opcję pełnej zależności.

![Wybierz opcję pełne zależności](aspnet-scaffolding-overview/_static/image7.png)

Po dodaniu zależności, zobaczysz **readme.txt** pliku. Starannie postępuj zgodnie z instrukcjami w tym pliku, aby upewnić się, że projekt działa prawidłowo.

Po wykonaniu kroków w plik readme.txt, można dodać nowy element szkieletu, jak pokazano w poprzedniej sekcji o MVC i interfejsu API sieci Web. Widoki generowane automatycznie i kontrolera będzie działać prawidłowo w ramach projektu.

## <a name="tutorials"></a>Samouczki

Aby utworzyć dostosowany tworzenia szkieletu, zobacz [tworzenie tworzenia szkieletu niestandardowe dla programu Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Aby dostosować wygenerowanych plików, zobacz [sposobu dostosowywania wygenerowanych plików w oknie dialogowym Nowy element szkieletu](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Na przykład przy użyciu funkcji szkieletów z **Database First programowanie**, zobacz [EF Database First z platformą ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Na przykład przy użyciu funkcji szkieletów w **MVC** projektu, zobacz [wprowadzenie do platformy ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Na przykład przy użyciu funkcji szkieletów w **interfejsu API sieci Web** projektu, zobacz [utworzyć interfejs API REST z atrybutu routingu w sieci Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
