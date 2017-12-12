---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: "Używanie składnika Web API z formularzami sieci Web programu ASP.NET | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2012
ms.topic: article
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: af918671a8bcc97a0050ea033ccd14dd96416e73
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
<a name="using-web-api-with-aspnet-web-forms"></a>Używanie składnika Web API z formularzami sieci Web ASP.NET
====================
przez [Wasson Jan](https://github.com/MikeWasson)

Chociaż interfejsu API sieci Web platformy ASP.NET jest dostarczana z platformą ASP.NET MVC, jest łatwe dodawanie interfejsu API sieci Web do tradycyjnych aplikacji formularzy sieci Web ASP.NET. Ten samouczek przedstawia kroki.

## <a name="overview"></a>Omówienie

Aby użyć interfejsu API sieci Web w aplikacji formularzy sieci Web, istnieją dwa podstawowe kroki:

- Dodawanie kontrolera interfejsu API sieci Web, która jest pochodną **klasy ApiController** klasy.
- Dodaj do tabeli tras **aplikacji\_Start** metody.

## <a name="create-a-web-forms-project"></a>Tworzenie projektu formularzy sieci Web

Uruchom program Visual Studio i wybierz **nowy projekt** z **Start** strony. Lub z **pliku** menu, wybierz opcję **nowy** , a następnie **projektu**.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz pozycję **Web**. Na liście szablony projektów, wybierz **aplikacji formularzy sieci Web ASP.NET**. Wprowadź nazwę dla projektu, a następnie kliknij przycisk **OK**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Tworzenie modelu i kontrolera

W tym samouczku korzysta z tej samej klasy modelu i kontrolera jako [wprowadzenie](tutorial-your-first-web-api.md) samouczka.

Najpierw Dodaj klasę modelu. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj klasę**. Nazwa klasy produktu, a następnie dodaj następujące wdrożenia:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Następnie dodaj do projektu., A kontroler Web API *kontrolera* jest obiekt, który obsługuje żądania HTTP dla interfejsu API sieci Web.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt. Wybierz **Dodaj nowy element**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

W obszarze **zainstalowane szablony**, rozwiń węzeł **Visual C#** i wybierz **Web**. Następnie wybierz z listy szablonów, **klasy kontrolera interfejsu API sieci Web**. Nazwa kontrolera "ProductsController", a następnie kliknij przycisk **Dodaj**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

**Dodaj nowy element** Kreator utworzy plik o nazwie ProductsController.cs. Usunięcie metod, które uwzględnione kreatora i dodaj następujące metody:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Aby uzyskać więcej informacji na temat kodu w tym kontrolerze, zobacz [wprowadzenie](tutorial-your-first-web-api.md) samouczka.

## <a name="add-routing-information"></a>Dodawanie informacji o routingu

Następnie dodamy trasy URI tak tego identyfikatorów URI w postaci &quot;/api/produkty/&quot; są kierowane do kontrolera.

W **Eksploratora rozwiązań**, kliknij dwukrotnie plik Global.asax można otworzyć pliku CodeBehind Global.asax.cs. Dodaj następujące **przy użyciu** instrukcji.

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

Następnie dodaj następujący kod, aby **aplikacji\_Start** metody:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Aby uzyskać więcej informacji o tabelach routingu, zobacz [routingu na platformie ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Dodawanie strony klienta AJAX

To wszystko, czego potrzebujesz do tworzenia interfejsu API, których klienci mogą uzyskiwać dostęp w sieci web. Teraz Dodajmy strona HTML, która używa technologii jQuery do wywołania interfejsu API.

Upewnij się, że strona wzorcowa (na przykład *Site.Master*) zawiera `ContentPlaceHolder` z `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Otwórz plik Default.aspx. Zastąp tekstu standardowego, który znajduje się w sekcji głównej zawartości, jak pokazano:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

Następnie dodaj odwołanie do pliku źródłowego jQuery w `HeaderContent` sekcji:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Uwaga: Możesz łatwo dodać odwołanie do skryptu przez przeciąganie i upuszczanie plików z **Eksploratora rozwiązań** do okna edytora kodu.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

Poniżej jQuery tag skryptu Dodaj następujący blok skryptu:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

Podczas ładowania dokumentu, ten skrypt zgłasza żądanie AJAX do &quot;interfejsu api/produkty&quot;. Żądanie zwraca listę produktów w formacie JSON. Skrypt ten dodaje informacji o produkcie do tabeli HTML.

Po uruchomieniu aplikacji powinien wyglądać następująco:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
