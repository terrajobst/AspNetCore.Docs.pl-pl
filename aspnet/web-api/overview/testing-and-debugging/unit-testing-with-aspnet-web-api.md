---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: ASP.NET Web API 2 testy jednostkowe | Dokumentacja firmy Microsoft
author: tfitzmac
description: Tej wskazówki i aplikacji przedstawiają sposób tworzenia testów jednostkowych proste dla aplikacji sieci Web API 2. W tym samouczku pokazano, jak dołączyć proj testu jednostki...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4d6102dd81589e41894d8ecd95bf9ddd761a65bd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042748"
---
<a name="unit-testing-aspnet-web-api-2"></a>ASP.NET Web API 2 testy jednostkowe
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> Tej wskazówki i aplikacji przedstawiają sposób tworzenia testów jednostkowych proste dla aplikacji sieci Web API 2. W tym samouczku pokazano, jak dołączyć jednostkowy projekt testowy do rozwiązania, a zapisu metody testowe, które są zwracane wartości z metody kontrolera.
> 
> W tym samouczku założono, że czytelnik zna podstawowe pojęcia związane z interfejsu API sieci Web platformy ASP.NET. Samouczek wprowadzający, zobacz [wprowadzenie do korzystania z programu ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> Testy jednostkowe w tym temacie są celowo ograniczone do scenariuszy proste danych. W celu testowania bardziej zaawansowanych scenariuszy danych, zobacz [Mocking Entity Framework podczas jednostki testowania ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
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

    - [Dodaj jednostkowy projekt testowy, podczas tworzenia aplikacji](#whencreate)
    - [Dodaj jednostkowy projekt testowy do istniejącej aplikacji](#addtoexisting)
- [Konfigurowanie aplikacji sieci Web API 2](#setupproject)
- [Instalowanie pakietów NuGet w projekcie testowym](#testpackages)
- [Tworzenie testów](#tests)
- [Uruchom testy](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Wymagania wstępne

Visual Studio 2017 Community, Professional or Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Pobierz kod

Pobierz [projektu zakończone](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Projekt do pobrania zawiera kod testu jednostkowego dla tego tematu i [Mocking Entity Framework podczas jednostki testowania składnika ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) tematu.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Tworzenie aplikacji z jednostkowy projekt testowy

Możesz utworzyć jednostkowy projekt testowy, podczas tworzenia aplikacji lub Dodaj jednostkowy projekt testowy do istniejącej aplikacji. Ten samouczek pokazuje obie metody tworzenia jednostkowy projekt testowy. Aby użyć w tym samouczku, można użyć jednej metody.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Dodaj jednostkowy projekt testowy, podczas tworzenia aplikacji

Tworzenie nowej aplikacji sieci Web programu ASP.NET o nazwie **StoreApp**.

![Tworzenie projektu](unit-testing-with-aspnet-web-api/_static/image1.png)

W systemie windows nowy projekt ASP.NET, wybierz **pusty** szablon i Dodaj foldery i podstawowe odwołania dla interfejsu API sieci Web. Wybierz **Dodaj testy jednostkowe** opcji. Automatycznie nosi nazwę jednostkowy projekt testowy **StoreApp.Tests**. Możesz pozostawić tę nazwę.

![Tworzenie projektu testu jednostki](unit-testing-with-aspnet-web-api/_static/image2.png)

Po utworzeniu aplikacji, zobaczysz, że zawiera dwa projekty.

![dwa projekty](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Dodaj jednostkowy projekt testowy do istniejącej aplikacji

Jeśli nie utworzono jednostkowy projekt testowy podczas tworzenia aplikacji, możesz dodać kategorię, w dowolnym momencie. Na przykład, załóżmy, że masz już aplikację o nazwie StoreApp, i chcesz dodać testów jednostkowych. Aby dodać jednostkowy projekt testowy, kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **Dodaj** i **nowy projekt**.

![Dodaj nowy projekt do rozwiązania](unit-testing-with-aspnet-web-api/_static/image4.png)

Wybierz **testu** w okienku po lewej stronie, a następnie wybierz **jednostkowy projekt testowy** dla typu projektu. Nazwij projekt **StoreApp.Tests**.

![Dodaj jednostkowy projekt testowy](unit-testing-with-aspnet-web-api/_static/image5.png)

Zobaczysz jednostkowy projekt testowy w rozwiązaniu.

W jednostkowy projekt testowy Dodaj odwołanie projektu do oryginalnego projektu.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Konfigurowanie aplikacji sieci Web API 2

W projekcie StoreApp dodać plik klasy do **modele** folder o nazwie **Product.cs**. Zastąp zawartość pliku następującym kodem.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Skompiluj rozwiązanie.

Kliknij prawym przyciskiem myszy folder kontrolery, a następnie wybierz **Dodaj** i **nowy element szkieletu**. Wybierz **pusty kontroler — interfejs API sieci Web 2**.

![Dodawanie nowego kontrolera](unit-testing-with-aspnet-web-api/_static/image6.png)

Ustaw nazwę kontrolera **SimpleProductController**i kliknij przycisk **Dodaj**.

![Określ kontroler](unit-testing-with-aspnet-web-api/_static/image7.png)

Zastąp istniejący kod następującym kodem. Aby uprościć w tym przykładzie, dane są przechowywane w listy, a nie bazy danych. Lista zdefiniowane w tej klasie reprezentuje danych produkcyjnych. Zwróć uwagę, że kontroler zawiera konstruktora, który przyjmuje jako parametr listę obiektów produktu. Ten konstruktor umożliwia przekazywanie danych testowych podczas testowania jednostek. Kontroler obejmuje również dwa **async** metod w celu zilustrowania testowaniu metod asynchronicznych jednostki. Te metody asynchroniczne zostały zaimplementowane przez wywołanie metody **Task.FromResult** aby zminimalizować nadmiarowe kod, ale zwykle metody obejmuje operacje wymagają dużej ilości zasobów.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

Metoda GetProduct Zwraca wystąpienie klasy **IHttpActionResult** interfejsu. IHttpActionResult jest jedną z nowych funkcji w sieci Web API 2, a jego upraszcza tworzenie testów jednostek. Klasy, które implementują interfejs IHttpActionResult znajdują się w [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) przestrzeni nazw. Te klasy reprezentuje możliwe odpowiedzi z akcji żądania i odpowiadają one kody stanu HTTP.

Skompiluj rozwiązanie.

Teraz można przystąpić do konfigurowania projektu testowego.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Instalowanie pakietów NuGet w projekcie testowym

Użycie pustego szablonu do tworzenia aplikacji, jednostkowy projekt testowy (StoreApp.Tests) nie zawiera żadnych zainstalowanych pakietów NuGet. Inne szablony, takie jak szablon interfejsu API sieci Web obejmują niektórych pakietów NuGet w jednostkowy projekt testowy. W tym samouczku musi zawierać pakietu Microsoft ASP.NET Web API 2 Core do projektu testowego.

Kliknij prawym przyciskiem myszy projekt StoreApp.Tests i wybierz **Zarządzaj pakietami NuGet**. Należy wybrać projekt StoreApp.Tests dodawania pakietów do tego projektu.

![Zarządzaj pakietami](unit-testing-with-aspnet-web-api/_static/image8.png)

Znajdź i zainstaluj pakiet Microsoft ASP.NET Web API 2 Core.

![Zainstaluj pakiet podstawowy interfejs api sieci web](unit-testing-with-aspnet-web-api/_static/image9.png)

Zamknij okno Zarządzanie pakietami NuGet.

<a id="tests"></a>
## <a name="create-tests"></a>Tworzenie testów

Domyślnie projekt testowy zawiera plik pusty test o nazwie UnitTest1.cs. Ten plik zawiera atrybuty się, że jest używany do utworzenia metody testowe. Dla testów jednostkowych możesz użyć tego pliku lub utworzyć własny plik.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

W tym samouczku utworzysz własnego klasy testowej. Można usunąć pliku UnitTest1.cs. Dodaj klasę o nazwie **TestSimpleProductController.cs**i Zastąp kod następującym kodem.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Uruchom testy

Teraz można przystąpić do uruchomienia testów. Wszystkie metody, które są oznaczone ikoną z **TestMethod** atrybut zostanie przetestowana. Z **testu** element menu, uruchom testy.

![uruchom testy](unit-testing-with-aspnet-web-api/_static/image11.png)

Otwórz **Eksploratora testów** okna i zwróć uwagę, wyniki testów.

![wyniki testu](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Podsumowanie

W tym samouczku została ukończona. Dane w tym samouczku został celowo uproszczona skoncentrować się na warunki testowania jednostek. W celu testowania bardziej zaawansowanych scenariuszy danych, zobacz [Mocking Entity Framework podczas jednostki testowania ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
