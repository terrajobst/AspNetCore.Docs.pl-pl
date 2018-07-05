---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Tworzenie aplikacji klienckiej OData v4 (C#) | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 1ea6121db781c2d08bc8c76cd07be3c5a4f23f62
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377395"
---
<a name="create-an-odata-v4-client-app-c"></a>Tworzenie aplikacji klienckiej OData v4 (C#)
====================
przez [Mike Wasson](https://github.com/MikeWasson)

W poprzednim samouczku utworzono podstawowej usługi OData, która obsługuje operacje CRUD. Teraz Utwórzmy klienta dla usługi.

Uruchom nowe wystąpienie programu Visual Studio i Utwórz nowy projekt aplikacji konsoli. W **nowy projekt** okno dialogowe, wybierz opcję **zainstalowane** &gt; **szablony** &gt; **Visual C#** &gt; **Pulpitu Windows**i wybierz **aplikację Konsolową** szablonu. Nadaj projektowi nazwę &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> Aplikacja konsoli można również dodać do tego samego rozwiązania programu Visual Studio, który zawiera usługę OData.


## <a name="install-the-odata-client-code-generator"></a>Zainstaluj Generator kodu klienta OData

Z **narzędzia** menu, wybierz opcję **rozszerzenia i aktualizacje**. Wybierz **Online** &gt; **galerii Visual Studio**. W polu wyszukiwania, wyszukaj &quot;generatora kodu klienta OData&quot;. Kliknij przycisk **Pobierz** zainstalować VSIX. Może być wyświetlony monit o ponowne uruchomienie programu Visual Studio.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Uruchom usługę OData lokalnie

Uruchom projekt ProductService z programu Visual Studio. Domyślnie program Visual Studio otworzy w przeglądarce katalog główny aplikacji. Należy pamiętać, identyfikator URI; będzie on potrzebny w następnym kroku. Pozostaw aplikację uruchomioną.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Jeśli oba projekty są umieszczone w tym samym rozwiązaniu, upewnij się uruchomić projekt ProductService bez debugowania. W następnym kroku należy zachować usługi uruchomione podczas modyfikowania projekt aplikacji konsoli.


## <a name="generate-the-service-proxy"></a>Generowanie usługi serwera Proxy

Serwer proxy usługi jest klasą .NET, która definiuje metody uzyskiwania dostępu do usługi OData. Serwer proxy tłumaczy wywołania metody na żądania HTTP. Spowoduje utworzenie klasy serwera proxy, uruchamiając [szablon T4](https://msdn.microsoft.com/library/bb126445.aspx).

Kliknij prawym przyciskiem myszy projekt. Wybierz **Dodaj** &gt; **nowy element**.

![](create-an-odata-v4-client-app/_static/image5.png)

W **Dodaj nowy element** okno dialogowe, wybierz opcję **elementy Visual C#** &gt; **kodu** &gt; **klient OData**. Nazwa szablonu &quot;ProductClient.tt&quot;. Kliknij przycisk **Dodaj** i klikanie ostrzeżenie o zabezpieczeniach.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

W tym momencie otrzymasz błąd można zignorować. Visual Studio automatycznie uruchamia szablonu, ale szablon wymaga niektóre ustawienia konfiguracji pierwszego.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Otwórz plik ProductClient.odata.config. W `Parameter` elementu, Wklej w identyfikatorze URI z projektu ProductService (w poprzednim kroku). Na przykład:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Uruchom szablon ponownie. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy plik ProductClient.tt, a następnie wybierz **Uruchom narzędzie niestandardowe**.

Ten szablon tworzy plik kodu o nazwie ProductClient.cs, który definiuje serwera proxy. Podczas opracowywania aplikacji, jeśli zmienisz punkt końcowy OData, uruchom szablon ponownie, aby zaktualizować serwer proxy.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Użyj serwera Proxy usługi do wywołania usługi OData

Otwórz plik Program.cs i Zastąp standardowy kod poniżej.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Zastąp wartość *serviceUri* identyfikatora URI z usługą wcześniej.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Po uruchomieniu aplikacji powinno danych wyjściowych poniżej:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
