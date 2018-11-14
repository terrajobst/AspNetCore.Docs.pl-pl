---
title: Używane analizatory interfejsu API sieci web
author: pranavkm
description: Więcej informacji o analizatorach interfejsu API sieci web w Microsoft.AspNetCore.Mvc.Api.Analyzers.
monikerRange: '>= aspnetcore-2.2'
ms.author: pranavkm
ms.custom: mvc
ms.date: 11/13/2018
uid: web-api/advanced/analyzers
ms.openlocfilehash: 89424d89ec2b3125fd3c6b7c86fed2d292b153e6
ms.sourcegitcommit: f202864efca81a72ea7120c0692940c40d9d0630
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51635364"
---
# <a name="use-web-api-analyzers"></a>Używane analizatory interfejsu API sieci web

Platforma ASP.NET Core 2.2 wprowadza [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers) pakiet NuGet zawierający analizatory internetowych interfejsów API. Analizatory pracować kontrolerów, oznaczony za pomocą <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute>, podczas kompilowania [konwencje interfejsu API](xref:web-api/advanced/conventions).

## <a name="package-installation"></a>Instalacja pakietu

`Microsoft.AspNetCore.Mvc.Api.Analyzers` można dodać jeden z następujących metod:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Z **Konsola Menedżera pakietów** okna:
  * Przejdź do **widoku** > **innych Windows** > **Konsola Menedżera pakietów**.
  * Przejdź do katalogu, w którym *ApiConventions.csproj* plik istnieje.
  * Wykonaj następujące polecenie:

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

* Z **Zarządzaj pakietami NuGet** okno dialogowe:
  * Kliknij prawym przyciskiem myszy projekt w **Eksploratora rozwiązań** > **Zarządzaj pakietami NuGet**.
  * Ustaw **źródła pakietu** na stronie "nuget.org".
  * Wprowadź "Microsoft.AspNetCore.Mvc.Api.Analyzers" w polu wyszukiwania.
  * Wybierz pakiet "Microsoft.AspNetCore.Mvc.Api.Analyzers" z **Przeglądaj** kartę, a następnie kliknij przycisk **zainstalować**.

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* Kliknij prawym przyciskiem myszy *pakietów* folderu w **konsoli rozwiązania** > **Dodawanie pakietów...** .
* Ustaw **Dodawanie pakietów** okna **źródła** menu rozwijane "nuget.org".
* Wprowadź "Microsoft.AspNetCore.Mvc.Api.Analyzers" w polu wyszukiwania.
* Wybierz pakiet "Microsoft.AspNetCore.Mvc.Api.Analyzers" w okienku wyników, a następnie kliknij przycisk **Dodaj pakiet**.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Uruchom następujące polecenie z **zintegrowany Terminal**:

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Uruchom następujące polecenie:

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

## <a name="analyzers-for-api-conventions"></a>Analizatory konwencje interfejsu API

Otwarte dokumenty interfejsu API zawierają kody stanów i typów odpowiedzi, które mogą zwrócić akcji. W programie ASP.NET Core MVC atrybuty takie jak <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> i <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> służą do dokumentowania akcji. <xref:tutorials/web-api-help-pages-using-swagger> Przechodzi do bardziej szczegółowo w dokumentacji interfejsu API.

Jedną z analizatory w pakiecie sprawdza kontrolerów, oznaczony za pomocą <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> i określa akcje, które nie całkowicie dokumentu ich odpowiedzi. Rozważmy następujący przykład:

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=9)]

Poprzednią akcję dokumenty HTTP 200 Powodzenie zwracany typ, ale nie dokumentu kod stanu błędu HTTP 404. Analizator raporty dokumentacji brakuje kod stanu HTTP 404 jako ostrzeżenie. Podano opcję, aby rozwiązać ten problem.
