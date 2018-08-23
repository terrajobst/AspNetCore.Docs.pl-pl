---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Pozorowanie programu Entity Framework podczas testowania ASP.NET Web API 2 jednostek | Dokumentacja firmy Microsoft
author: tfitzmac
description: Tej wskazówki i aplikacji przedstawiają sposób tworzenia testów jednostkowych dla aplikacji sieci Web API 2, który używa programu Entity Framework. Pokazuje, jak zmodyfikować...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 0bc5ab59583a2be3f889ba05d26c6cda4589057d
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754144"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Pozorowanie programu Entity Framework podczas testowania ASP.NET Web API 2 jednostek
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

[Pobieranie ukończone projektu](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Tej wskazówki i aplikacji przedstawiają sposób tworzenia testów jednostkowych dla aplikacji sieci Web API 2, który używa programu Entity Framework. Pokazuje sposób modyfikowania szkieletu kontrolera, aby umożliwić przekazywanie obiekt kontekstu do testowania oraz sposób tworzenia obiektów testowych, które działają z platformą Entity Framework.
> 
> Aby zapoznać się z wprowadzeniem do testowanie jednostek za pomocą interfejsu API sieci Web platformy ASP.NET, zobacz [testów jednostkowych przy użyciu wzorca ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).
> 
> W tym samouczku przyjęto założenie, że znasz podstawowe pojęcia dotyczące środowiska ASP.NET Web API. Aby uzyskać Samouczek wprowadzający, zobacz [wprowadzenie do wzorca ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Składnik Web API 2


## <a name="in-this-topic"></a>W tym temacie:

Ten temat zawiera następujące sekcje:

- [Wymagania wstępne](#prereqs)
- [Pobierz program code](#download)
- [Tworzenie aplikacji przy użyciu projektu testu jednostkowego](#appwithunittest)
- [Tworzenie klasy modelu](#modelclass)
- [Dodawanie kontrolera](#controller)
- [Dodaj wstrzykiwanie zależności](#dependency)
- [Instalowanie pakietów NuGet w projekcie testowym](#testpackages)
- [Tworzenie kontekstu testu](#testcontext)
- [Tworzenie testów](#tests)
- [Uruchom testy](#runtests)

Jeśli wykonano już kroki opisane w [testów jednostkowych przy użyciu wzorca ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), możesz przejść do sekcji [dodać kontrolera](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Wymagania wstępne

Program Visual Studio 2017 Community, Professional lub Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Pobierz program code

Pobierz [projektu ukończona](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Projekt do pobrania zawiera kod testu jednostkowego dla tego tematu i [jednostki testowania wzorca ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) tematu.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Tworzenie aplikacji przy użyciu projektu testu jednostkowego

Możesz utworzyć projekt testów jednostkowych, podczas tworzenia aplikacji lub dodać projekt testów jednostkowych do istniejących aplikacji. Ten samouczek przedstawia tworzenie projektu testu jednostkowego, podczas tworzenia aplikacji.

Utwórz nową aplikację sieci Web programu ASP.NET o nazwie **StoreApp**.

W systemie windows nowy projekt ASP.NET, wybierz **pusty** szablon i Dodaj foldery i podstawowe odwołania dla internetowego interfejsu API. Wybierz **dodać testy jednostkowe** opcji. Projekt testów jednostkowych ma automatycznie nadawaną nazwę **StoreApp.Tests**. Możesz zachować tę nazwę.

![Tworzenie projektu testu jednostkowego](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Po utworzeniu aplikacji, zostanie wyświetlony zawiera dwa projekty — **StoreApp** i **StoreApp.Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Tworzenie klasy modelu

W projekcie StoreApp Dodaj plik klasy do **modeli** folder o nazwie **Product.cs**. Zastąp zawartość pliku następującym kodem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Skompiluj rozwiązanie.

<a id="controller"></a>
## <a name="add-the-controller"></a>Dodawanie kontrolera

Kliknij prawym przyciskiem myszy folder kontrolerów, a następnie wybierz pozycję **Dodaj** i **nowy element szkieletu**. Wybierz kontroler 2 internetowego interfejsu API z akcjami używający narzędzia Entity Framework.

![Dodaj nowy kontroler](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Ustaw następujące wartości:

- Nazwa kontrolera: **ProductController**
- Klasa modelu: **produktu**
- Klasa kontekstu danych: [wybierz **nowy kontekst danych** przycisk, który wypełnia wartości widoczne poniżej]

![Określ kontroler](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Kliknij przycisk **Dodaj** do tworzenia kontrolera przy użyciu automatycznie generowanego kodu. Kod zawiera metody do tworzenia, pobierania, aktualizowania i usuwania wystąpień klasy produktu. Poniższy kod przedstawia metodę dla dodawania produktu. Należy zauważyć, że metoda zwraca wystąpienie **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult jest jedną z nowych funkcji w sieci Web API 2 i upraszcza tworzenie testów jednostkowych.

W następnej sekcji dostosujesz wygenerowany kod w celu ułatwienia przekazywanie obiektów testowych do kontrolera.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Dodaj wstrzykiwanie zależności

Obecnie klasy ProductController jest ustalony do korzystania z wystąpienia klasy StoreAppContext. Wzorzec o nazwie wstrzykiwanie zależności użyje do modyfikowania aplikacji i usunąć tej ustalonych zależności. Dzięki pozbyciu się do tej zależności możesz przekazać makiety obiektu podczas testowania.

Kliknij prawym przyciskiem myszy **modeli** folderze i Dodaj nowy interfejs o nazwie **IStoreAppContext**.

Zastąp kod następującym kodem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Otwórz plik StoreAppContext.cs i wprowadź następujące zmiany wyróżnione. Istotne zmiany, należy pamiętać, są następujące:

- Klasa StoreAppContext implementuje interfejs IStoreAppContext
- Metoda MarkAsModified jest zaimplementowana.


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Otwórz plik ProductController.cs. Zmień istniejący kod, aby dopasować wyróżniony kod. Te zmiany zepsuć zależności na StoreAppContext i włączyć inne klasy przekazać innego obiektu klasy kontekstu. Ta zmiana spowoduje włączenie przekazania w kontekście testu podczas testów jednostkowych.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

Istnieje jeden więcej zmian, które należy wykonać w ProductController. W **PutProduct** metody, Zastąp wiersz, który ustawia stan jednostki można modyfikować za pomocą wywołanie metody MarkAsModified.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Skompiluj rozwiązanie.

Teraz można przystąpić do konfigurowania projektu testowego.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Instalowanie pakietów NuGet w projekcie testowym

Gdy używasz pusty szablon do tworzenia aplikacji projektu testu jednostkowego (StoreApp.Tests) nie obejmuje wszystkie zainstalowane pakiety NuGet. Inne szablony, takie jak szablon interfejsu API sieci Web obejmują niektóre pakiety NuGet w projekcie testów jednostkowych. W tym samouczku musi zawierać pakiet platformy Entity Framework i pakiet Microsoft ASP.NET Web API 2 Core do projektu testowego.

Kliknij prawym przyciskiem myszy projekt StoreApp.Tests, a następnie wybierz pozycję **Zarządzaj pakietami NuGet**. Musisz wybrać projekt StoreApp.Tests, aby dodać pakiety do tego projektu.

![Zarządzanie pakietami](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

Z pakietów w trybie Online należy znaleźć i zainstalować pakiet EntityFramework (w wersji 6.0 lub nowszej). Jeśli wygląda na to, że pakiet EntityFramework jest już zainstalowany, może wybrany projekt StoreApp zamiast StoreApp.Tests projektu.

![Dodawanie programu Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Znajdź i zainstaluj pakiet Microsoft ASP.NET Web API 2 Core.

![Zainstaluj pakiet podstawowego interfejsu api sieci web](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Zamknij okno Zarządzanie pakietami NuGet.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Tworzenie kontekstu testu

Dodaj klasę o nazwie **TestDbSet** do projektu testu. Ta klasa służy jako klasa bazowa dla zestawu danych testowych. Zastąp kod następującym kodem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Dodaj klasę o nazwie **TestProductDbSet** do projektu testu, który zawiera następujący kod.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Dodaj klasę o nazwie **TestStoreAppContext** i Zastąp istniejący kod następującym kodem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Tworzenie testów

Domyślnie projektu testowego zawiera plik pusty test o nazwie **UnitTest1.cs**. Ten plik zawiera atrybuty, że umożliwia tworzenie metod testowych. Na potrzeby tego samouczka możesz usunąć ten plik, ponieważ spowoduje dodanie nowej klasy testowej.

Dodaj klasę o nazwie **TestProductController** do projektu testu. Zastąp kod następującym kodem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Uruchom testy

Teraz można przystąpić do uruchomienia testów. Wszystkie metody, które są oznaczone **TestMethod** atrybut, który będzie testowany. Z **testu** elementu menu, uruchom testy.

![uruchom testy](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Otwórz **Eksplorator testów** okna i zwróć uwagę, wyniki testów.

![wyniki testu](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
