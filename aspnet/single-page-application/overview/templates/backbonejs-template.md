---
uid: single-page-application/overview/templates/backbonejs-template
title: Szablon szkieletu | Dokumentacja firmy Microsoft
author: madskristensen
description: Backbone.js SPA szablonu
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/04/2013
ms.topic: article
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 3b8eabd3cefcb96dc40bbf6cc6e3ee81accb0d7c
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566309"
---
<a name="backbone-template"></a>Szablon sieci szkieletowej
====================
przez [Mads Kristensen](https://github.com/madskristensen)

> Szablon SPA szkieletu zapisał Kazi Manzur Rashid
> 
> [Pobierz szablon SPA Backbone.js](https://go.microsoft.com/fwlink/?LinkId=293631)


Szablon Backbone.js SPA zaprojektowano ułatwiających rozpoczęcie pracy szybkie opracowywanie aplikacji sieci web po stronie klienta interactive przy użyciu [Backbone.js.](http://backbonejs.org/)

Szablon zapewnia początkowej szkielet opracowanie aplikacji Backbone.js na platformie ASP.NET MVC. Poza pole zawiera funkcję logowania użytkownika podstawowego, tym resetowania hasła rejestrację, logowanie, użytkownika i potwierdzenie przez użytkownika z szablonami podstawowe wiadomości e-mail.

Wymagania:

- [Aktualizacja programu ASP.NET i 2012.2 narzędzi sieci Web](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Tworzenie szablonu projektu sieci szkieletowej

Pobierz i zainstaluj szablonu, klikając przycisk Pobierz powyżej. Szablon jest dostarczana jako plik rozszerzenia serwera Visual Studio (VSIX). Może być konieczne ponowne uruchomienie programu Visual Studio.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz pozycję **Web**. Na liście szablony projektów, wybierz **aplikacji sieci Web programu ASP.NET MVC 4**. Nazwij projekt i kliknij przycisk **OK**.

![](backbonejs-template/_static/image1.png)

W **nowy projekt** kreatora, wybierz projekt JEDNOSTRONICOWEJ Backbone.js.

![](backbonejs-template/_static/image2.png)

Naciśnij klawisze Ctrl-F5, aby skompilować i uruchomić aplikację bez debugowania, lub naciśnij klawisz F5, aby uruchomić z debugowaniem.

![](backbonejs-template/_static/image3.png)

Klikając przycisk "Moje konto" powoduje wyświetlenie strony logowania:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Wskazówki: Kodu klienta

Załóżmy rozpoczyna się po stronie klienta. Skrypty aplikacji klienta znajdują się w folderze ~/Scripts/application. Aplikacja została napisana [TypeScript](http://www.typescriptlang.org/) (pliki TS) kompilowane na język JavaScript (js plików).

**Aplikacja**

`Application` jest zdefiniowany w application.ts. Ten obiekt inicjuje aplikacji i działa jako głównej przestrzeni nazw. Przechowuje informacje konfiguracji i stanie, który jest współużytkowany przez aplikację, takie jak określa, czy użytkownik jest zalogowany.

`application.start` Metoda tworzy modalne widoków i dołącza programy obsługi zdarzeń dla zdarzenia na poziomie aplikacji, takich jak logowanie użytkowników. Następnie tworzy routera domyślnego i sprawdza, czy określono dowolny adres URL po stronie klienta. Jeśli nie, przekierowuje do domyślnego adresu url (#! /).

**Zdarzenia**

Zdarzenia są zawsze ważne podczas opracowywania luźno powiązane składniki. Aplikacje często wykonywać wiele operacji w odpowiedzi na akcję użytkownika. Szkielet zawiera wbudowane zdarzenia ze składnikami modelu, kolekcji i widoku. Zamiast tworzyć między zależności między tymi składnikami, szablon wykorzystuje model "pub/sub": `events` obiekt zdefiniowany w events.ts, pełni funkcję Centrum zdarzeń do publikowania i subskrybowanie zdarzeń aplikacji. `events` Obiektu jest klasą pojedynczą. Poniższy kod przedstawia sposób subskrybować zdarzenia, a następnie wyzwala zdarzenia:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Router**

W Backbone.js router zapewnia metody routingu strony po stronie klienta oraz łącząc je do działań i zdarzeń. Szablon definiuje jednym routerem router.ts. Router tworzy activable widoki i przechowuje informacje o stanie podczas przełączania widoków. (Activable widoki są opisane w następnej sekcji). Początkowo projekt zawiera dwa widoki fikcyjny, macierzystego i informacje o. Ma również widoku NotFound, która jest wyświetlana, jeśli trasa jest nieznana.

**Widoki**

Widoki są definiowane w ~/Scripts/application/widoków. Istnieją dwa rodzaje widoków, activable widoki i modalnego okna dialogowego. Widoki activable są wywoływane przez router. Widok activable jest wyświetlany, wszystkie widoki activable stają się nieaktywne. Aby utworzyć widok activable, rozszerzanie widoku z `Activable` obiektu:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

Rozszerzanie z `Activable` dodaje dwie nowe metody do widoku, `activate` i `deactivate`. Router wywołania tych metod, aby aktywować i zdezaktywować widoku.

Modalne widoki są zaimplementowane jako [Twitter Bootstrap](http://twitter.github.com/bootstrap/) modalne okna dialogowe. `Membership` i `Profile` widoki są modalne widoków. Widoki modelu może być wywoływany przez zdarzeń aplikacji. Na przykład w `Navigation` przedstawia widok, klikając łącze "Moje konto", albo `Membership` widoku lub `Profile` widok, w zależności od tego, czy użytkownik jest zalogowany. `Navigation` Dołącza kliknięcie uchwytów zdarzeń, aby wszystkie elementy podrzędne, które mają `data-command` atrybutu. Oto kod znaczników HTML:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Oto kod w navigation.ts do podpinanie zdarzeń:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modele**

Modele są definiowane w ~/Scripts/application/modeli. Wszystkie modele ma trzy podstawowe czynności: domyślne atrybuty, reguły sprawdzania poprawności i punkt końcowy po stronie serwera. Oto typowy przykład:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Dodatki plug-in**

~/Scripts/application/lib folder zawiera kilka wtyczek przydatną jQuery. Plik form.ts definiuje wtyczki do pracy z danymi formularza. Często potrzebne do serializacji lub deserializacji danych formularza i Pokaż wszystkie błędy weryfikacji modelu. Wtyczka form.ts ma metody `serializeFields`, `deserializeFields`, i `showFieldErrors`. Poniższy przykład pokazuje, jak do formularza z modelem serializacji.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

Wtyczka flashbar.ts daje różne rodzaje opinii dla użytkownika. Metody są `$.showSuccessbar`, `$.showErrorbar` i `$.showInfobar`. W tle używa Twitter Bootstrap alertów do wyświetlenia wiadomości dobrze animowany.

Wtyczka confirm.ts zastępuje przeglądarki potwierdzić okna dialogowego, chociaż jest nieco inny interfejs API:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Wskazówki: Kod serwera

Teraz Przyjrzyjmy się po stronie serwera.

**Kontrolery**

W aplikacji jednej strony serwera odgrywa małych roli w interfejsie użytkownika. Zwykle serwer renderuje stronę początkową, a następnie wysyła i odbiera dane JSON.

Szablon ma dwa kontrolerów MVC: `HomeController` renderuje stronę początkową i `SupportsController` służy do potwierdzenia nowych kont użytkowników i resetowania haseł. Wszystkie kontrolery w szablonie są kontrolery interfejsu API sieci Web platformy ASP.NET, które wysyłać i odbierać dane JSON. Domyślnie kontrolery korzystać z nowych `WebSecurity` klasy do wykonywania zadań związanych z użytkownika. Jednak również mają konstruktorów opcjonalne, które umożliwiają przekazywanie w delegatach do wykonywania tych zadań. To ułatwia testowanie i umożliwia wymianę `WebSecurity` na inny, korzystając z kontenera IoC. Oto przykład:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Widoki

Widoki są zaprojektowane jako moduły: każda sekcja strony ma własne dedykowane widoku. W aplikacji jednej strony jest często zawierają widoki, które nie mają żadnych odpowiedniego kontrolera. Widok może zawierać przez wywołanie metody `@Html.Partial('myView')`, ale to pobiera nużące. Aby ułatwić, szablon definiuje metodę pomocniczą, `IncludeClientViews`, który renderuje wszystkie widoki w określonym folderze:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Jeśli nie określono nazwy folderu, domyślna nazwa folderu jest "ClientViews". Jeśli widok klienta używa także widoki częściowe, nazwę widoku częściowego się od znaku podkreślenia (na przykład `_SignUp`). `IncludeClientViews` — Metoda nie obejmuje żadnych widoków, których nazwa rozpoczyna się od znaku podkreślenia. Aby uwzględnić widoku częściowego w widoku klienta, należy wywołać `Html.ClientView('SignUp')` zamiast `Html.Partial('_SignUp')`.

**Wysyłanie wiadomości E-mail**

Aby wysłać wiadomość e-mail, używa szablonu [pocztowych](http://aboutcode.net/postal). Jednak pocztowych jest pobieranej z resztą kodu przy użyciu `IMailer` interfejsu, dlatego łatwo można zastąpić go z implementacją innego. Szablony wiadomości e-mail znajdują się w folderze widoki lub wiadomości E-mail. Adres e-mail nadawcy jest określony w pliku web.config w `sender.email` klucza z **appSettings** sekcji. Ponadto, kiedy `debug="true"` w pliku web.config, aplikacja nie wymaga potwierdzenie adresu e-mail użytkownika, aby przyspieszyć programowanie.

## <a name="github"></a>GitHub

Możesz również znaleźć szablonu Backbone.js SPA na [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
