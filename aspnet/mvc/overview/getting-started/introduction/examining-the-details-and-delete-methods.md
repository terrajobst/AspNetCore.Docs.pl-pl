---
uid: mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
title: Badanie metod Details i Delete | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 03/26/2015
ms.assetid: f1d2a916-626c-4a54-8df4-77e6b9fff355
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 3ec5a9a91cf1e2273f91a529779936fd26182fc4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755761"
---
<a name="examining-the-details-and-delete-methods"></a>Badanie metod Details i Delete
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

W tej części samouczka możesz omówione automatycznie generowanych `Details` i `Delete` metody.

## <a name="examining-the-details-and-delete-methods"></a>Badanie metod Details i Delete

Otwórz `Movie` kontrolera i zbadaj `Details` metody.

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

Aparat tworzenia szkieletów MVC, utworzony z tą metodą akcji dodaje komentarz przedstawiający żądania HTTP, który wywołuje tę metodę. W tym przypadku jest `GET` żądanie przy użyciu trzech segmenty adresu URL, `Movies` kontrolera, `Details` metody i `ID` wartość.

Kod najpierw ułatwia wyszukiwanie danych przy użyciu `Find` metody. Ważna funkcja zabezpieczeń wbudowanych w metodzie jest kod sprawdza, czy `Find` znaleziono filmu metody, zanim kod próbuje wykonywać żadnych czynności z nim. Na przykład haker może spowodować błędy do witryny, zmieniając adres URL utworzony przez łącza z `http://localhost:xxxx/Movies/Details/1` na wartość podobną `http://localhost:xxxx/Movies/Details/12345` (lub inną wartość, która nie zawiera rzeczywistych filmu). Jeśli nie zaznaczono dla filmu o wartości null, null filmu spowoduje błąd bazy danych.

Sprawdź `Delete` i `DeleteConfirmed` metody.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Należy zauważyć, że HTTP GET `Delete` metoda nie powoduje usunięcia określonego filmu, zwraca widok filmu w trakcie którego można przesłać (`HttpPost`) usuwania. Wykonywanie operacji usuwania w odpowiedzi na polecenie GET żądania (lub służącego wykonywania operacji Edytuj, Utwórz operacji lub innej operacji, które zmieniają dane) otwiera lukę w zabezpieczeniach. Aby uzyskać więcej informacji na ten temat, zobacz wpis w blogu Autor: Stephen Walther [46 Porada # w programie ASP.NET MVC — nie używaj usunąć łącza, ponieważ mogą tworzyć luki w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

`HttpPost` Nosi nazwę metody, która powoduje usunięcie danych `DeleteConfirmed` zapewnienie metodą HTTP POST unikatowy podpis lub nazwy. Poniżej przedstawiono podpisy dwóch metod:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

Środowisko uruchomieniowe języka wspólnego (CLR) wymaga przeciążonej metody ma unikatowy parametr podpisu (tej samej nazwie metoda, ale inną listę parametrów). Jednak w tym miejscu należy dwie metody usuwania — jeden dla GET--i jeden dla wpisu czy obie pozycje mają taki sam podpis parametru. (Obaj użytkownicy muszą zaakceptować pojedyncze liczby całkowite jako parametr.)

Aby posortować tę możliwość, można zrobić kilka rzeczy. Jeden to nadać różne nazwy metody. To mechanizm tworzenia szkieletów została w poprzednim przykładzie. Jednak wprowadza mały problem: ASP.NET mapuje segmentów adresu URL do metody akcji według nazwy i metodę w przypadku zmiany nazwy, routing zwykle nie będzie mógł odnaleźć tej metody. Rozwiązanie jest widoczny w tym przykładzie jest dodanie `ActionName("Delete")` atrybutu `DeleteConfirmed` metody. Skutecznie wykonuje to mapowanie systemu routingu, aby adres URL, który zawiera */Delete/* dla wpisu znajdzie żądanie `DeleteConfirmed` metody.

Innym typowym sposobem uniknięcia problemu z metod, które mają identyczne nazwy i wzory podpisów jest sztucznie Zmień podpis metody POST, aby uwzględnić Nieużywany parametr. Na przykład niektórzy deweloperzy dodać typ parametru `FormCollection` który jest przekazywany do metody POST, a następnie po prostu nie używaj parametru:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Podsumowanie

Masz teraz kompletnej aplikacji ASP.NET MVC, która przechowuje dane w lokalnej bazie danych bazy danych. Można utworzyć, Odczyt, aktualizowanie, usuwanie i wyszukaj filmy.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Następne kroki

Po skompilowane i przetestowane aplikacji sieci web, następnym krokiem jest udostępnić go innym osobom korzystanie przez Internet. Aby to zrobić, należy wdrożyć ją do dostawcy usług hosta sieci web. Firma Microsoft oferuje bezpłatny internetowy hostowanie do 10 witryn sieci web w [bezpłatne konto wersji próbnej platformy Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Czy zasugerować obok wykonaj Moje samouczek [wdrażanie aplikacji platformy ASP.NET MVC Secure z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL na platformie Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Doskonałe samouczek jest poziomu pośredniego Tom Dykstra [Tworzenie modelu danych Entity Framework dla aplikacji ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [Witryna StackOverflow](http://stackoverflow.com/help) i [fora platformy ASP.NET MVC](https://forums.asp.net/1146.aspx) są doskonałym umieszcza zadawać pytania. Postępuj zgodnie z [mnie](https://twitter.com/RickAndMSFT) w serwisie twitter, dzięki czemu można uzyskać aktualizacje na Moje najnowsze samouczki.

Opinia jest powitalnej.

— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [Poprzednie](adding-validation.md)
