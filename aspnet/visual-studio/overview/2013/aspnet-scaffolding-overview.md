---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Funkcja tworzenia szkieletu ASP.NET w programie Visual Studio 2013 | Dokumentacja firmy Microsoft
author: tfitzmac
description: Funkcja tworzenia szkieletu ASP.NET jest nową funkcję, która znajduje się w programie Visual Studio 2013.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: c554f592e57f2e6017f7fcfcc9b4c98051e21b37
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752994"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>Funkcja tworzenia szkieletu ASP.NET w programie Visual Studio 2013
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> Funkcja tworzenia szkieletu ASP.NET jest nową funkcję, która znajduje się w programie Visual Studio 2013.


## <a name="overview"></a>Omówienie

Funkcja tworzenia szkieletu ASP.NET jest struktura generowania kodu dla aplikacji sieci Web ASP.NET. Visual Studio 2013 obejmuje generatorów kodu wstępnie zainstalowane dla projektów MVC i interfejs API sieci Web. Dodasz szkieletu do projektu, jeśli chcesz szybko dodać kod, który wchodzi w interakcję z modelami danych. Szkieletów może skrócić czas do opracowywania działań standardowych danych w projekcie.

Domyślnie program Visual Studio 2013 nie obsługuje generowania kodu dla projektu formularzy sieci Web, ale za pomocą tworzenia szkieletów formularzy sieci Web przez dodanie zależności MVC do projektu lub Instalowanie rozszerzenia. Poniżej przedstawiono oba podejścia.

Visual Studio 2013 Update 2 (obecnie RC) zapewnia możliwość rozszerzania funkcja tworzenia szkieletu ASP.NET w celu spełnienia wymagań scenariusza. Dzięki tej funkcji można utworzyć szablon dostosowane szkieletu i dodać go do okna dialogowego Dodawanie szkieletu nowe. W ramach dostosowanego szablonu możesz określić kod, który jest generowany, gdy Dodawanie elementu szkieletu. Aby uzyskać więcej informacji, zobacz [tworzenia niestandardowego Generator szkieletu dla programu Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Wymagania wstępne

Aby korzystać z ASP.NET tworzenie szkieletów, musisz mieć:

- Microsoft Visual Studio 2013
- Narzędzia dla deweloperów sieci Web (część domyślnej instalacji programu Visual Studio 2013)
- Struktury sieci Web platformy ASP.NET i Tools 2013 (część domyślnej instalacji programu Visual Studio 2013)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Dodawanie elementu szkieletu do MVC lub interfejsu API sieci Web

W celu dodania szkieletu, kliknij prawym przyciskiem myszy projekt lub folder w projekcie, a następnie wybierz **Dodaj** — **nowy element szkieletu**, jak pokazano na poniższej ilustracji.

![Dodawanie elementu szkieletu](aspnet-scaffolding-overview/_static/image1.png)

Z **Dodawanie szkieletu** okna, wybierz typ szkieletu do dodania.

![Wybierz typ szkieletu](aspnet-scaffolding-overview/_static/image2.png)

**Dodaj kontroler** okno pozwala wybrać opcje generowania kontrolera, w tym, czy chcesz korzystać z nowych funkcji asynchronicznej z platformy Entity Framework 6.

![Dodawanie kontrolera](aspnet-scaffolding-overview/_static/image3.png)

Odpowiednich klas i strony są tworzone dla danego scenariusza. Na przykład na poniższej ilustracji przedstawiono kontroler MVC i widoki, które zostały utworzone za pomocą funkcją szkieletów dla klasę modelu o nazwie filmów.

![Utworzone pliki](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Dodawanie elementu szkieletu do formularzy sieci Web

Aby dodać tworzenia szkieletów, które generuje kod formularzy sieci Web, możesz zainstalować rozszerzenie programu Visual Studio lub Dodaj zależności MVC. Poniżej przedstawiono oba podejścia, ale należy wykonać jedną z tych metod.

### <a name="web-forms-scaffolding-extension"></a>Formularze sieci Web, tworzenia szkieletów rozszerzenia

Można zainstalować rozszerzenia programu Visual Studio, które umożliwiają tworzenie szkieletu za pomocą projektu formularzy sieci Web. W programie Visual Studio, wybierz **narzędzia** i następnie **rozszerzenia i aktualizacje**. W tym oknie dialogowym wyszukiwania galerii Visual Studio dla **szkieletu formularzy sieci Web**.

![Zainstaluj formularzy sieci web, tworzenia szkieletów](aspnet-scaffolding-overview/_static/image5.png)

Aby uzyskać więcej informacji, zobacz [szkieletu formularzy sieci Web](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>Zależności MVC

Aby dodać zależności MVC, wybierz **Dodaj** - **nowy element szkieletu**. W oknie Dodawanie szkieletu wybierz **zależności MVC**, jak pokazano poniżej.

![Dodaj zależności MVC](aspnet-scaffolding-overview/_static/image6.png)

Dostępne są dwie opcje na potrzeby tworzenia szkieletów MVC; Minimalna i pełne. Jeśli wybierzesz przycisku minimalnych tylko pakiety NuGet i odwołania dla platformy ASP.NET MVC są dodawane do projektu. Jeśli wybierzesz opcję pełnej, minimalnym zależności zostaną dodane, a także wymagane pliki zawartości projektu MVC. Aby łatwo korzystać z tworzenia szkieletów, wybierz pełną zależności.

![Wybierz opcję pełnej zależności](aspnet-scaffolding-overview/_static/image7.png)

Po dodaniu zależności, zobaczysz **readme.txt** pliku. Dokładnie postępuj zgodnie z instrukcjami w tym pliku, aby upewnić się, że projekt działa prawidłowo.

Po zakończeniu kroków z pliku readme.txt można dodać nowy element szkieletowy, jak pokazano w poprzedniej sekcji o MVC i interfejs API sieci Web. Widoki generowane automatycznie i kontroler będzie działać prawidłowo w obrębie projektu.

## <a name="tutorials"></a>Samouczki

Aby utworzyć niestandardowe Generator szkieletu, zobacz [tworzenia niestandardowego Generator szkieletu dla programu Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Aby dostosować wygenerowanych plików, zobacz [jak dostosować wygenerowane pliki w oknie dialogowym Nowy element szkieletu](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Na przykład szkieletów z **rozwoju Database First**, zobacz [EF Database First ze wzorca ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Na przykład szkieletów w **MVC** projektu, zobacz [wprowadzenie do ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Na przykład szkieletów w **interfejsu API sieci Web** projektu, zobacz [Tworzenie interfejsu API REST przy użyciu atrybutu routingu w sieci Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
