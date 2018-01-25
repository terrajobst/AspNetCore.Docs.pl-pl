---
uid: mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
title: "Sprawdzenie szczegółów i usuwania metody | Dokumentacja firmy Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: f1d2a916-626c-4a54-8df4-77e6b9fff355
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: a4e2b075497e08334183519bf8942e4af6f7a727
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="examining-the-details-and-delete-methods"></a>Badanie informacji i metody Delete
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

W tej części samouczka będziesz należy zbadać automatycznie generowanych `Details` i `Delete` metody.

## <a name="examining-the-details-and-delete-methods"></a>Badanie informacji i metody Delete

Otwórz `Movie` kontrolera i sprawdź, czy `Details` metody.

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

Aparat szkieletów MVC utworzony tą metodą akcji dodaje komentarz przedstawiający żądania HTTP, która wywołuje metodę. W tym przypadku jest `GET` żądania z trzech segmenty adresu URL, `Movies` kontrolera, `Details` — metoda i `ID` wartość.

Kod najpierw ułatwia wyszukiwanie danych przy użyciu `Find` metody. Jest to funkcja zabezpieczeń ważne wbudowanych w metodzie kod sprawdza, czy `Find` znaleziono metody filmu przed próbuje podejmować żadnych działań z nim kod. Na przykład haker może wprowadzić błędy do witryny, zmieniając adres URL utworzony przez łącza z `http://localhost:xxxx/Movies/Details/1` podobną `http://localhost:xxxx/Movies/Details/12345` (lub inne wartości nie reprezentuje rzeczywisty film). Jeśli nie zaznaczono filmu wartości null, null filmu spowoduje błąd bazy danych.

Sprawdź `Delete` i `DeleteConfirmed` metody.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Należy pamiętać, że `HTTP Get``Delete` — metoda nie powoduje usunięcia określonego filmu, zwraca widok filmu którego można przesłać (`HttpPost`) usunięcia. Wykonywanie operacji usuwania w odpowiedzi na polecenie GET żądania (lub dla tej sprawy wykonywania operacji edycji, Utwórz operację lub innej operacji, które zmienia dane) otwiera luka w zabezpieczeniach. Aby uzyskać więcej informacji na ten temat, zobacz wpis w blogu Stephen Walther [46 Porada # w programie ASP.NET MVC — nie używaj usunąć łącza, ponieważ mogą one tworzyć luk w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

`HttpPost` Nosi nazwę metody, która powoduje usunięcie danych `DeleteConfirmed` umożliwiają metodą HTTP POST unikatowego podpisu lub nazwę. Poniżej przedstawiono podpisy dwóch metod:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

Środowisko uruchomieniowe języka wspólnego (CLR) wymaga przeciążonej metody ma unikatowy parametr podpisu (tej samej nazwy metody, ale lista różnych parametrów). Jednak w tym miejscu należy Delete sposobów — jeden dla GET - i jeden dla żądania POST czy mają tę samą sygnaturę parametru. (Oba muszą zaakceptować pojedynczego całkowitą jako parametr.)

Aby posortować tę możliwość, można wykonać kilka rzeczy. Jeden jest zapewniają różne nazwy metody. To mechanizm szkieletów został w poprzednim przykładzie. Jednak powstaje mały problem: ASP.NET mapuje segmentów adresu URL do metody akcji według nazwy i zmiana metody routingu zwykle nie będą mogli odnaleźć tej metody. Rozwiązanie, to zostanie wyświetlony w tym przykładzie jest dodanie `ActionName("Delete")` atrybutu `DeleteConfirmed` metody. To skutecznie wykonuje mapowania systemu routingu, aby adres URL, który zawiera */Delete/* POST znajdzie żądanie `DeleteConfirmed` metody.

Inny typowy sposób, aby uniknąć problemów z metod, które mają identyczne nazwy i podpisy jest sztucznie Zmiana podpisu metody POST, aby uwzględnić parametrem nieużywane. Na przykład, niektórzy deweloperzy dodać parametr typu `FormCollection` przekazanego do metody POST, a następnie po prostu nie używaj parametru:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Podsumowanie

Masz teraz kompletna aplikacja platformy ASP.NET MVC, która przechowuje dane w lokalnej bazie danych bazy danych. Można utworzyć, odczytu, aktualizacji, usuwania i wyszukiwać filmy.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Następne kroki

Po utworzone i przetestowane aplikacji sieci web, następnym krokiem jest aby udostępnić ją innym użytkownikom za pośrednictwem Internetu. W tym celu należy wdrożyć ją do usługi hosta sieci web. Firma Microsoft oferuje usługi hostingu sieci web wolnego maksymalnie 10 witryn sieci web w [bezpłatnego konta wersji próbnej Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). I zasugerować dalej wykonaj Moje samouczek [wdrażanie aplikacji bezpiecznego ASP.NET MVC z członkostwa, OAuth i bazy danych SQL Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Samouczek znakomity jest poziomu pośredniego Tomasz Dykstra [tworzenia modelu danych struktury jednostek dla aplikacji platformy ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [StackOverflow](http://stackoverflow.com/help) i [ASP.NET MVC forum](https://forums.asp.net/1146.aspx) jest doskonałym umieszcza zadawać pytania. Postępuj zgodnie z [mnie](https://twitter.com/RickAndMSFT) w serwisie twitter, aby aktualizacje na Moje najnowsze samouczki.

Opinie użytkowników są powitalnej.

— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter:[@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Scott Hanselman](http://www.hanselman.com/blog/) twitter:[@shanselman](https://twitter.com/shanselman)

>[!div class="step-by-step"]
[Poprzednie](adding-validation.md)
