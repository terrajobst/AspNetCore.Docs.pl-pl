---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: ASP.NET Web API 2 testy jednostkowe | Dokumentacja firmy Microsoft
author: tfitzmac
description: Tej wskazówki i aplikacji przedstawiają sposób tworzenia testów jednostkowych proste dla aplikacji sieci Web API 2. W tym samouczku pokazano, jak dołączyć proj testu jednostki...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 91cca2789b66b7b8983f8786b506c5fc71db8b75
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795437"
---
<a name="unit-testing-aspnet-web-api-2"></a>ASP.NET Web API 2 testy jednostkowe
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

[Pobieranie ukończone projektu](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Tej wskazówki i aplikacji przedstawiają sposób tworzenia testów jednostkowych proste dla aplikacji sieci Web API 2. W tym samouczku pokazano, jak do uwzględnienia projekt testów jednostkowych w rozwiązaniu, a następnie zapisać metody testowe, sprawdzające wartości zwracane przez metodę kontrolera.
>
> W tym samouczku przyjęto założenie, że znasz podstawowe pojęcia dotyczące środowiska ASP.NET Web API. Aby uzyskać Samouczek wprowadzający, zobacz [wprowadzenie do wzorca ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
>
> Testy jednostkowe w tym temacie są celowo ograniczone do prostych danych scenariuszy. Do testowania bardziej zaawansowanych scenariuszy danych jednostki, zobacz [pozorowanie programu Entity Framework podczas jednostki testowania wzorca ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Składnik Web API 2

## <a name="in-this-topic"></a>W tym temacie:

Ten temat zawiera następujące sekcje:

- [Wymagania wstępne](#prereqs)
- [Pobierz program code](#download)
- [Tworzenie aplikacji przy użyciu projektu testu jednostkowego](#appwithunittest)
    - [Dodaj projekt testu jednostkowego, podczas tworzenia aplikacji](#whencreate)
    - [Dodaj projekt testu jednostkowego do istniejącej aplikacji](#addtoexisting)
- [Konfigurowanie aplikacji sieci Web API 2](#setupproject)
- [Instalowanie pakietów NuGet w projekcie testowym](#testpackages)
- [Tworzenie testów](#tests)
- [Uruchom testy](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Wymagania wstępne

[Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional lub Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Pobierz program code

Pobierz [projektu ukończona](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Projekt do pobrania zawiera kod testu jednostkowego dla tego tematu i [pozorowanie programu Entity Framework podczas jednostki testowania ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) tematu.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Tworzenie aplikacji przy użyciu projektu testu jednostkowego

Możesz utworzyć projekt testów jednostkowych, podczas tworzenia aplikacji lub dodać projekt testów jednostkowych do istniejących aplikacji. Ten samouczek przedstawia tworzenie projektu testu jednostkowego obu metod. Aby wykonać kroki tego samouczka, można użyć każda z tych metod.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Dodaj projekt testu jednostkowego, podczas tworzenia aplikacji

Utwórz nową aplikację sieci Web programu ASP.NET o nazwie **StoreApp**.

![Tworzenie projektu](unit-testing-with-aspnet-web-api/_static/image1.png)

W systemie windows nowy projekt ASP.NET, wybierz **pusty** szablon i Dodaj foldery i podstawowe odwołania dla internetowego interfejsu API. Wybierz **dodać testy jednostkowe** opcji. Projekt testów jednostkowych ma automatycznie nadawaną nazwę **StoreApp.Tests**. Możesz zachować tę nazwę.

![Tworzenie projektu testu jednostkowego](unit-testing-with-aspnet-web-api/_static/image2.png)

Po utworzeniu aplikacji, zobaczysz, że zawiera dwa projekty.

![dwa projekty](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Dodaj projekt testu jednostkowego do istniejącej aplikacji

Jeśli nie utworzono projekt testów jednostkowych podczas tworzenia aplikacji, możesz dodać jeden w dowolnym momencie. Na przykład, załóżmy, że masz już aplikację o nazwie StoreApp i chcesz dodać testy jednostkowe. Aby dodać projekt testu jednostkowego, kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **Dodaj** i **nowy projekt**.

![Dodaj nowy projekt do rozwiązania](unit-testing-with-aspnet-web-api/_static/image4.png)

Wybierz **testu** w okienku po lewej stronie i wybierz **projektu testu jednostkowego** dla typu projektu. Nadaj projektowi nazwę **StoreApp.Tests**.

![Dodaj projekt testu jednostkowego](unit-testing-with-aspnet-web-api/_static/image5.png)

Zobaczysz projekt testów jednostkowych w rozwiązaniu.

W projekcie testu jednostki Dodaj odwołanie do oryginalnego projektu.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Konfigurowanie aplikacji sieci Web API 2

W projekcie StoreApp Dodaj plik klasy do **modeli** folder o nazwie **Product.cs**. Zastąp zawartość pliku następującym kodem.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Skompiluj rozwiązanie.

Kliknij prawym przyciskiem myszy folder kontrolerów, a następnie wybierz pozycję **Dodaj** i **nowy element szkieletu**. Wybierz **pusty kontroler - Web API 2**.

![Dodaj nowy kontroler](unit-testing-with-aspnet-web-api/_static/image6.png)

Ustaw nazwę kontrolera **SimpleProductController**i kliknij przycisk **Dodaj**.

![Określ kontroler](unit-testing-with-aspnet-web-api/_static/image7.png)

Zastąp istniejący kod następującym kodem. Aby uprościć ten przykład, dane są przechowywane w listy, a nie bazy danych. Listy zdefiniowanej w tej klasie reprezentuje danych produkcyjnych. Zwróć uwagę, że kontroler konstruktora, który przyjmuje jako parametr listę obiektów produktu. Ten konstruktor umożliwia przekazywanie danych testowych podczas testów jednostkowych. Kontroler obejmuje również dwa **async** metody, aby zilustrować metody asynchroniczne testy jednostkowe. Te metody asynchroniczne zostały zaimplementowane przez wywołanie metody **Task.FromResult** zminimalizować nadmiarowe kod, ale zazwyczaj metody obejmuje operacje dużej ilości zasobów.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

Metoda GetProduct Zwraca wystąpienie **IHttpActionResult** interfejsu. IHttpActionResult jest jedną z nowych funkcji w sieci Web API 2 i upraszcza tworzenie testów jednostkowych. Klasy, które implementują interfejs IHttpActionResult znajdują się w [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) przestrzeni nazw. Te klasy reprezentują możliwe odpowiedzi z akcji żądania, a odpowiadają one kodów stanu HTTP.

Skompiluj rozwiązanie.

Teraz można przystąpić do konfigurowania projektu testowego.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Instalowanie pakietów NuGet w projekcie testowym

Gdy używasz pusty szablon do tworzenia aplikacji projektu testu jednostkowego (StoreApp.Tests) nie obejmuje wszystkie zainstalowane pakiety NuGet. Inne szablony, takie jak szablon interfejsu API sieci Web obejmują niektóre pakiety NuGet w projekcie testów jednostkowych. W tym samouczku musi zawierać pakiet Microsoft ASP.NET Web API 2 Core do projektu testowego.

Kliknij prawym przyciskiem myszy projekt StoreApp.Tests, a następnie wybierz pozycję **Zarządzaj pakietami NuGet**. Musisz wybrać projekt StoreApp.Tests, aby dodać pakiety do tego projektu.

![Zarządzanie pakietami](unit-testing-with-aspnet-web-api/_static/image8.png)

Znajdź i zainstaluj pakiet Microsoft ASP.NET Web API 2 Core.

![Zainstaluj pakiet podstawowego interfejsu api sieci web](unit-testing-with-aspnet-web-api/_static/image9.png)

Zamknij okno Zarządzanie pakietami NuGet.

<a id="tests"></a>
## <a name="create-tests"></a>Tworzenie testów

Domyślnie projektu testowego zawiera plik pusty test o nazwie UnitTest1.cs. Ten plik zawiera atrybuty, że umożliwia tworzenie metod testowych. Dla testów jednostkowych można użyć tego pliku lub Utwórz własny plik.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

W tym samouczku utworzysz klasy testowej. Możesz usunąć plik UnitTest1.cs. Dodaj klasę o nazwie **TestSimpleProductController.cs**i Zastąp kod następującym kodem.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Uruchom testy

Teraz można przystąpić do uruchomienia testów. Wszystkie metody, które są oznaczone **TestMethod** atrybut, który będzie testowany. Z **testu** elementu menu, uruchom testy.

![uruchom testy](unit-testing-with-aspnet-web-api/_static/image11.png)

Otwórz **Eksplorator testów** okna i zwróć uwagę, wyniki testów.

![wyniki testu](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Podsumowanie

W tym samouczku została ukończona. Danych w ramach tego samouczka został celowo uproszczona skoncentrować się na warunki testów jednostkowych. Do testowania bardziej zaawansowanych scenariuszy danych jednostki, zobacz [pozorowanie programu Entity Framework podczas jednostki testowania wzorca ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
