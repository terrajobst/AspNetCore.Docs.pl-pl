---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: "Mocking programu Entity Framework podczas testowania składnika ASP.NET Web API 2 jednostek | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "Tej wskazówki i aplikacji przedstawiają sposób tworzenia testów jednostkowych dla aplikacji sieci Web API 2, który używa programu Entity Framework. Widoczny jest sposób modyfikowania..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: abfde7edec85812de3560f4edefb110c3e374580
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2018
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Mocking programu Entity Framework podczas testowania składnika ASP.NET Web API 2 jednostek
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> Tej wskazówki i aplikacji przedstawiają sposób tworzenia testów jednostkowych dla aplikacji sieci Web API 2, który używa programu Entity Framework. Widoczny jest sposób modyfikowania szkieletu kontrolera, aby umożliwić przekazywanie obiektu kontekstu do testowania oraz sposobu tworzenia obiektów testu, które współpracują z programu Entity Framework.
> 
> Aby obejrzeć wprowadzenie do testy jednostkowe za pomocą interfejsu API sieci Web platformy ASP.NET, zobacz [testów jednostkowych z ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).
> 
> W tym samouczku założono, że czytelnik zna podstawowe pojęcia związane z interfejsu API sieci Web platformy ASP.NET. Samouczek wprowadzający, zobacz [wprowadzenie do korzystania z programu ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Składnik Web API 2


## <a name="in-this-topic"></a>W tym temacie:

Ten temat zawiera następujące sekcje:

- [Wymagania wstępne](#prereqs)
- [Pobierz kod](#download)
- [Tworzenie aplikacji z jednostkowy projekt testowy](#appwithunittest)
- [Utwórz klasę modelu](#modelclass)
- [Dodawanie kontrolera](#controller)
- [Dodaj iniekcji zależności](#dependency)
- [Instalowanie pakietów NuGet w projekcie testowym](#testpackages)
- [Tworzenie kontekstu testu](#testcontext)
- [Tworzenie testów](#tests)
- [Uruchom testy](#runtests)

Jeśli wykonano już kroki opisane w [testów jednostkowych z ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), możesz przejść do sekcji [dodać kontroler](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Wymagania wstępne

Visual Studio 2017 Community, Professional or Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Pobierz kod

Pobierz [projektu zakończone](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Projekt do pobrania zawiera kod testu jednostkowego dla tego tematu i [jednostek testowania ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) tematu.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Tworzenie aplikacji z jednostkowy projekt testowy

Możesz utworzyć jednostkowy projekt testowy, podczas tworzenia aplikacji lub Dodaj jednostkowy projekt testowy do istniejącej aplikacji. Ten samouczek przedstawia tworzenie jednostkowy projekt testowy, podczas tworzenia aplikacji.

Tworzenie nowej aplikacji sieci Web programu ASP.NET o nazwie **StoreApp**.

W systemie windows nowy projekt ASP.NET, wybierz **pusty** szablon i Dodaj foldery i podstawowe odwołania dla interfejsu API sieci Web. Wybierz **Dodaj testy jednostkowe** opcji. Automatycznie nosi nazwę jednostkowy projekt testowy **StoreApp.Tests**. Możesz pozostawić tę nazwę.

![Tworzenie projektu testu jednostki](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Po utworzeniu aplikacji będą widzieli zawiera dwa projekty — **StoreApp** i **StoreApp.Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Utwórz klasę modelu

W projekcie StoreApp dodać plik klasy do **modele** folder o nazwie **Product.cs**. Zastąp zawartość pliku następującym kodem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Skompiluj rozwiązanie.

<a id="controller"></a>
## <a name="add-the-controller"></a>Dodawanie kontrolera

Kliknij prawym przyciskiem myszy folder kontrolery, a następnie wybierz **Dodaj** i **nowy element szkieletu**. Wybierz kontroler programu Web API 2 z akcjami używający narzędzia Entity Framework.

![Dodawanie nowego kontrolera](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Ustaw następujące wartości:

- Nazwa kontrolera: **ProductController**
- Klasa modelu: **produktu**
- Klasa kontekstu danych: [wybierz **nowy kontekst danych** przycisku, wypełnia wartości przedstawione poniżej]

![Określ kontroler](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Kliknij przycisk **Dodaj** do utworzenia kontrolera automatycznie wygenerowany kod. Kod zawiera metody tworzenia, pobierania, aktualizowania i usuwania wystąpienia klasy produktu. Poniższy kod przedstawia metody do dodawania produktu. Należy zauważyć, że ta metoda zwraca wystąpienie **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult jest jedną z nowych funkcji w sieci Web API 2, a jego upraszcza tworzenie testów jednostek.

W następnej sekcji, możesz dostosować wygenerowany kod w celu ułatwienia przekazywanie obiektów testu na kontrolerze.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Dodaj iniekcji zależności

Klasa ProductController jest obecnie ustalony Aby użyć wystąpienia klasy StoreAppContext. Do modyfikowania aplikacji i tej zależności z wpisaną na stałe usunąć użyjesz wzorzec o nazwie iniekcji zależności. Dzięki pozbyciu się tym zależności można przekazać w obiekcie testowych podczas testowania.

Kliknij prawym przyciskiem myszy **modele** folderu i Dodaj nowy interfejs o nazwie **IStoreAppContext**.

Zastąp kod następującym kodem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Otwórz plik StoreAppContext.cs i wprowadź następujące zmiany zaznaczony. Ważne zmiany, należy pamiętać, są następujące:

- Klasa StoreAppContext implementuje interfejs IStoreAppContext
- Metoda MarkAsModified jest zaimplementowana.


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Otwórz plik ProductController.cs. Zmień istniejący kod, aby dopasować wyróżniony kod. Te zmiany zepsuć zależności na StoreAppContext i Włącz innych klas umożliwia przekazywanie dla klasy kontekstu na inny obiekt. Ta zmiana pozwoli podczas testów jednostkowych należy przekazać w kontekście testu.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

Brak jednej więcej zmiany, które należy wykonać w ProductController. W **PutProduct** metoda, Zastąp wiersza, która ustawia stan jednostki zmodyfikowane przez wywołanie do metody MarkAsModified.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Skompiluj rozwiązanie.

Teraz można przystąpić do konfigurowania projektu testowego.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Instalowanie pakietów NuGet w projekcie testowym

Użycie pustego szablonu do tworzenia aplikacji, jednostkowy projekt testowy (StoreApp.Tests) nie zawiera żadnych zainstalowanych pakietów NuGet. Inne szablony, takie jak szablon interfejsu API sieci Web obejmują niektórych pakietów NuGet w jednostkowy projekt testowy. W tym samouczku muszą zawierać packge Entity Framework i pakietu Microsoft ASP.NET Web API 2 Core do projektu testowego.

Kliknij prawym przyciskiem myszy projekt StoreApp.Tests i wybierz **Zarządzaj pakietami NuGet**. Należy wybrać projekt StoreApp.Tests dodawania pakietów do tego projektu.

![Zarządzaj pakietami](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

Z pakietów w trybie Online Znajdź i zainstaluj pakiet EntityFramework (w wersji 6.0 lub nowszej). Jeśli wygląda na to, że pakiet EntityFramework jest już zainstalowany, być może wybrano projektu StoreApp zamiast StoreApp.Tests projektu.

![Dodaj Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Znajdź i zainstaluj pakiet Microsoft ASP.NET Web API 2 Core.

![Zainstaluj pakiet podstawowy interfejs api sieci web](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Zamknij okno Zarządzanie pakietami NuGet.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Tworzenie kontekstu testu

Dodaj klasę o nazwie **TestDbSet** do projektu testowego. Ta klasa służy jako klasa podstawowa dla użytkownika testowego zestawu danych. Zastąp kod następującym kodem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Dodaj klasę o nazwie **TestProductDbSet** do projektu testowego, który zawiera następujący kod.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Dodaj klasę o nazwie **TestStoreAppContext** i Zastąp istniejący kod następującym kodem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Tworzenie testów

Domyślnie projekt testowy zawiera plik pusty test o nazwie **UnitTest1.cs**. Ten plik zawiera atrybuty się, że jest używany do utworzenia metody testowe. W tym samouczku można usunąć tego pliku, ponieważ doda nową klasę testów.

Dodaj klasę o nazwie **TestProductController** do projektu testowego. Zastąp kod następującym kodem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Uruchom testy

Teraz można przystąpić do uruchomienia testów. Wszystkie metody, które są oznaczone ikoną z **TestMethod** atrybut zostanie przetestowana. Z **testu** element menu, uruchom testy.

![uruchom testy](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Otwórz **Eksploratora testów** okna i zwróć uwagę, wyniki testów.

![wyniki testu](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
