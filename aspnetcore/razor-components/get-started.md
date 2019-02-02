---
title: Wprowadzenie do składników Razor
author: guardrex
description: Dowiedz się, jak rozpocząć pracę z architekturą składniki Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/get-started
ms.openlocfilehash: c83af10fd84bc8238f5fe20c66b91ba17de80ae3
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668126"
---
# <a name="get-started-with-razor-components"></a>Wprowadzenie do składników Razor

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="setup"></a>Konfiguracja

Zainstaluj następujące czynności:

1. [Zestaw SDK programu .NET core 2.1](https://go.microsoft.com/fwlink/?linkid=873092) (2.1.500 lub nowszej).
1. [Program Visual Studio 2017](https://go.microsoft.com/fwlink/?linkid=873093) (15.9 lub nowszej) z *ASP.NET i tworzenie aplikacji internetowych* wybranym pakietem roboczym.
1. Najnowsze [rozszerzenia usług językowych Blazor](https://go.microsoft.com/fwlink/?linkid=870389) z witryny Marketplace programu Visual Studio.
1. Szablony Blazor w wierszu polecenia:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates
   ```

Aby utworzyć swój pierwszy projekt w programie Visual Studio:

1. Wybierz **pliku** > **nowy projekt** > **Web** > **aplikacji sieci Web platformy ASP.NET Core**.
1. Upewnij się, że **platformy .NET Core** i **platformy ASP.NET Core 2.1** wybrano u góry.
1. Wybierz szablon Blazor **OK**.

   ![Nowe okno dialogowe aplikacji](https://msdnshared.blob.core.windows.net/media/2018/07/new-blazor-app-dialog-0.5.0.png)

1. Naciśnij klawisz **Ctrl-F5** do uruchomienia aplikacji *bez debugera*. Uruchamianie przy użyciu debugera (**F5**) nie jest obsługiwana w tej chwili.

Aby utworzyć nową aplikację Blazor z wiersza polecenia:

```console
dotnet new blazor -o BlazorApp1
cd BlazorApp1
dotnet run
```

Gratulacje! Po prostu uruchomiono swoją pierwszą aplikację Blazor!

![Strona główna aplikacji Blazor](https://msdnshared.blob.core.windows.net/media/2018/04/blazor-bootstrap-4.png)

## <a name="help--feedback"></a>Pomoc i opinie

Twoja opinia jest dla nas szczególnie ważna w tej fazie eksperymentalne dla Blazor. W przypadku napotkania problemów lub masz pytania, podczas próby się Blazor, Daj nam znać!

* [Plik problemów w usłudze GitHub](https://github.com/aspnet/AspNetCore/issues) przypadku napotkania problemów lub sugestie dotyczące ulepszenia.
* Porozmawiaj z nami i społeczności Blazor na [dotyczącym oprogramowania Gitter](https://gitter.im/aspnet/blazor) czy uzyskać została zablokowana na udostępnianie Blazor pracy za Ciebie.

Po wypróbowaniu Blazor Podaj nam co myślisz, wykorzystując naszą ankietę w ramach produktu. Kliknij łącze ankiety, wyświetlany na stronie głównej aplikacji, gdy uruchomiony jeden z szablonów projektów Blazor:

![Udział w ankiecie Blazor](https://msdnshared.blob.core.windows.net/media/2018/05/blazor-survey-new.png)

## <a name="next-steps"></a>Następne kroki

<xref:tutorials/first-blazor-app>
