---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Tworzenie aplikacji klienckich OData v4 (C#) | Dokumentacja firmy Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 51a3c7b9c5b6525d6d82b9a45910f58b71268b7f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036703"
---
<a name="create-an-odata-v4-client-app-c"></a>Tworzenie aplikacji klienckich OData v4 (C#)
====================
przez [Wasson Jan](https://github.com/MikeWasson)

W poprzednich samouczka utworzono podstawowej usługi OData, która obsługuje operacje CRUD. Teraz Utwórzmy klienta dla usługi.

Uruchom nowe wystąpienie programu Visual Studio i Utwórz nowy projekt aplikacji konsoli. W **nowy projekt** okno dialogowe, wybierz opcję **zainstalowana** &gt; **szablony** &gt; **Visual C#** &gt; **Windows Desktop**i wybierz **aplikacji konsoli** szablonu. Nazwij projekt &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> Można również dodać aplikację konsoli do tego samego rozwiązania Visual Studio, który zawiera usługi OData.


## <a name="install-the-odata-client-code-generator"></a>Zainstaluj klienta OData Generator kodu

Z **narzędzia** menu, wybierz opcję **rozszerzenia i aktualizacje**. Wybierz **Online** &gt; **galerii programu Visual Studio**. W polu wyszukiwania, wyszukaj &quot;generatora kodu klienta OData&quot;. Kliknij przycisk **Pobierz** do zainstalowania pliku VSIX. Może być wyświetlony monit o ponowne uruchomienie programu Visual Studio.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Uruchamianie usługi OData lokalnie

Uruchom projekt ProductService z programu Visual Studio. Domyślnie program Visual Studio uruchamia przeglądarki do katalogu głównego aplikacji. Należy pamiętać, identyfikator URI; będzie on potrzebny w następnym kroku. Pozostaw aplikacja była uruchomiona.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Jeśli oba projekty w tym samym rozwiązaniu, upewnij się uruchomić projekt ProductService bez debugowania. W następnym kroku należy zachować usługę uruchomiony podczas modyfikowania projekt aplikacji konsoli.


## <a name="generate-the-service-proxy"></a>Generowanie serwera Proxy usługi

Serwer proxy usługi jest klasą .NET, który definiuje metody do uzyskiwania dostępu do usługi OData. Serwer proxy tłumaczy wywołania metody do żądania HTTP. Utworzysz klasy serwera proxy, uruchamiając [szablon T4](https://msdn.microsoft.com/library/bb126445.aspx).

Kliknij prawym przyciskiem myszy projekt. Wybierz **dodać** &gt; **nowy element**.

![](create-an-odata-v4-client-app/_static/image5.png)

W **Dodaj nowy element** okno dialogowe, wybierz opcję **Visual C# elementów** &gt; **kod** &gt; **klient OData**. Nazwa szablonu &quot;ProductClient.tt&quot;. Kliknij przycisk **Dodaj** i kliknij przycisk za pomocą ostrzeżenie o zabezpieczeniach.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

W tym momencie zostanie wyświetlony błąd, można zignorować. Visual Studio automatycznie uruchamia szablonu, ale szablon wymaga niektóre ustawienia konfiguracji pierwszy.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Otwórz plik ProductClient.odata.config. W `Parameter` elementu, Wklej w identyfikatorze URI z projektu ProductService (w poprzednim kroku). Na przykład:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Uruchom szablon ponownie. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy plik ProductClient.tt i wybierz **Uruchom narzędzie niestandardowe**.

Szablon tworzy plik kodu o nazwie ProductClient.cs, który definiuje serwera proxy. Podczas opracowywania aplikacji, jeśli zmienisz punktu końcowego OData, uruchom szablon ponownie, aby zaktualizować serwer proxy.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Użyj serwera Proxy usługi do wywoływania usługi OData

Otwórz plik Program.cs i Zastąp schematyczny kod poniżej.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Zastąp wartość *serviceUri* z identyfikatorem URI usługi z wcześniej.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Po uruchomieniu aplikacji powinno danych wyjściowych poniżej:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
