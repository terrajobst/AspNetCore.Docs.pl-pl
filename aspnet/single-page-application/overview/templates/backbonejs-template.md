---
uid: single-page-application/overview/templates/backbonejs-template
title: Szablon backbone | Dokumentacja firmy Microsoft
author: madskristensen
description: Backbone.js SPA szablonu
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/04/2013
ms.topic: article
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 641e149155fbee2655024bec3b76dce5243e7d59
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362508"
---
<a name="backbone-template"></a>Szablon backbone
====================
przez [: Mads Kristensen](https://github.com/madskristensen)

> Szablon Backbone w SPA został napisany przez Kazi Manzur Rashid
> 
> [Pobierz szablon SPA Backbone.js](https://go.microsoft.com/fwlink/?LinkId=293631)


Szablon Backbone.js SPA jest przeznaczony do ułatwiające rozpoczęcie pracy szybkiego tworzenia aplikacji sieci web interactive po stronie klienta przy użyciu [Backbone.js.](http://backbonejs.org/)

Szablon zawiera szkielet początkowy do tworzenia aplikacji w języku Backbone.js we wzorcu ASP.NET MVC. Gotowych zapewnia funkcje logowania użytkowników w warstwie podstawowa, w tym, resetowanie hasła tworzenia nowych kont i logowania użytkownika i potwierdzenie przez użytkownika za pomocą szablonów podstawowy adres e-mail.

Wymagania:

- [Aktualizacja programu ASP.NET i Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Tworzenie projektu sieci szkieletowej szablonu

Pobierz i zainstaluj szablon, klikając przycisk Pobierz powyżej. Szablon jest spakowany jako plik programu Visual Studio rozszerzenia (VSIX). Może być konieczne ponowne uruchomienie programu Visual Studio.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz opcję **Web**. Na liście szablonów projektu wybierz **aplikacji sieci Web programu ASP.NET MVC 4**. Nadaj projektowi nazwę, a następnie kliknij przycisk **OK**.

![](backbonejs-template/_static/image1.png)

W **nowy projekt** kreatora, wybierz projekt SPA Backbone.js.

![](backbonejs-template/_static/image2.png)

Naciśnij kombinację klawiszy Ctrl-F5, aby skompilować i uruchomić aplikację bez debugowania lub naciśnij klawisz F5, aby uruchomić debugowanie.

![](backbonejs-template/_static/image3.png)

Klikając przycisk "Moje konto" powoduje wyświetlenie strony logowania:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Wskazówki: Kod klienta

Spróbujmy zaczyna się po stronie klienta. Skrypty aplikacji klienta znajdują się w folderze ~/Scripts/application. Aplikacja została napisana [TypeScript](http://www.typescriptlang.org/) (plików TS) które są kompilowane do kodu JavaScript (js plików).

**Aplikacja**

`Application` jest zdefiniowany w application.ts. Ten obiekt jest inicjowany aplikacji i działa jako głównej przestrzeni nazw. Utrzymuje informacje konfigurację i stan, który jest współużytkowany przez aplikację, takie jak tego, czy użytkownik jest zalogowany.

`application.start` Metoda tworzy modalne widoków i dołącza obsługę zdarzeń dla zdarzeń na poziomie aplikacji, takich jak logowanie użytkownika. Następnie tworzy domyślny routera i sprawdza, czy określono dowolnego adresu URL po stronie klienta. Jeśli nie, zostanie przekierowany do domyślnego adresu url (#! /).

**Zdarzenia**

Zdarzenia są istotne zawsze w przypadku, gdy tworzenie luźno sprzężonymi składnikami. Aplikacje często wykonują wiele operacji w odpowiedzi na akcję użytkownika. Sieci szkieletowej udostępnia wbudowane zdarzeń za pomocą składników, takich jak Model, kolekcji i widoku. Zamiast tworzyć wzajemnych zależności między tymi składnikami, ten szablon korzysta z modelu "pub/sub": `events` zdefiniowanego w events.ts, obiektu pełniącego funkcję Centrum zdarzeń do publikowania i subskrybowania zdarzenia aplikacji. `events` Obiektu jest klasą pojedynczą. Poniższy kod pokazuje, jak subskrybować zdarzenie, a następnie wyzwalacz zdarzenia:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Router**

W Backbone.js router zapewnia metody routingu stron po stronie klienta i łącząc je w celu działaniach i zdarzeniach. Szablon definiuje pojedynczego routera router.ts. Router activable widoków tworzy i przechowuje informacje o stanie podczas przełączania widoków. (Activable widoki są opisane w następnej sekcji). Początkowo projekt ma dwa widoki fikcyjnego, głównego i wkrótce. Ponadto wprowadzono widoku NotFound jest wyświetlana, jeśli trasa jest nieznany.

**Widoki**

Widoki są definiowane w ~/Scripts/application/widoków. Istnieją dwa rodzaje widoków, activable widoków i modalne okno dialogowe. Widoki activable są wywoływane przez router. Widok activable jest wyświetlany, inne widoki activable stają się nieaktywne. Aby utworzyć widok activable, rozszerzyć widok z `Activable` obiektu:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

Rozszerzanie za pomocą `Activable` dodaje dwie nowe metody w widoku `activate` i `deactivate`. Router wywołuje te metody aktywacji i zdezaktywować się widoku.

Modalne widoki są implementowane jako [Twitter Bootstrap](http://twitter.github.com/bootstrap/) modalne okna dialogowe. `Membership` i `Profile` widoki są modalne widoków. Widoki modelu może być wywoływany przez zdarzenia z dowolnej aplikacji. Na przykład w `Navigation` widoku, klikając link "Moje konto" przedstawia albo `Membership` widoku lub `Profile` widok, w zależności od tego, czy użytkownik jest zalogowany. `Navigation` Dołącza kliknij obsługi zdarzeń, aby wszystkie elementy podrzędne, które mają `data-command` atrybutu. Poniżej przedstawiono kod znaczników HTML:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Oto kod w navigation.ts do podłączania zdarzeń:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modele**

Modele są definiowane w ~/Scripts/application/modeli. Wszystkie modele ma trzy podstawowe czynności: domyślne atrybuty, reguł sprawdzania poprawności i punkt końcowy po stronie serwera. Oto typowy przykład:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Dodatki plug-in**

~/Scripts/application/lib folder zawiera kilka wtyczek jQuery pod ręką. Plik form.ts definiuje dodatku typu plug-in do pracy z danymi formularza. Często potrzebne do serializacji lub deserializacji danych formularza i wyświetlane ewentualne błędy sprawdzania poprawności modelu. Wtyczka form.ts zawiera metody, takie jak `serializeFields`, `deserializeFields`, i `showFieldErrors`. Poniższy przykład pokazuje, jak do serializacji formularza z modelem.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

Wtyczka flashbar.ts zapewnia różne rodzaje komunikatów zwrotnych do użytkownika. Metody są `$.showSuccessbar`, `$.showErrorbar` i `$.showInfobar`. W tle używa Twitter Bootstrap alertów do wyświetlenia wiadomości dobrze animowany.

Wtyczka confirm.ts zastępuje przeglądarkę okna dialogowego, upewnij się, chociaż interfejs API jest nieco inne:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Wskazówki: Kod serwera

Teraz Przyjrzyjmy się po stronie serwera.

**Kontrolery**

W aplikacji jednostronicowej serwera odgrywa małych roli w interfejsie użytkownika. Zwykle serwer renderuje stronę początkową a następnie wysyła i odbiera dane JSON.

Szablon zawiera dwa kontrolerów MVC: `HomeController` renderuje stronę początkową i `SupportsController` służy do potwierdzenia nowych kont użytkowników i resetowania haseł. Innych kontrolerów w szablonie są kontrolery ASP.NET Web API, które wysyłać i odbierać dane JSON. Domyślnie, korzystać z nowych kontrolerów `WebSecurity` klasy, aby wykonywać zadania związane z użytkownikiem. Jednak mają także opcjonalne konstruktorów, które umożliwiają przekazywanie w delegatach do wykonywania tych zadań. To ułatwia testowanie i pozwala zastąpić `WebSecurity` z czymś innym, za pomocą kontenera IoC. Oto przykład:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Widoki

Widoki są przeznaczone do moduły: każda sekcja strony ma swój własny dedykowany widoku. W aplikacji jednostronicowej jest często zawierają widoki, które nie mają żadnego odpowiedniego kontrolera. Może zawierać widok, wywołując `@Html.Partial('myView')`, ale spowoduje pobranie żmudnym. Aby to ułatwić, szablon definiuje metodę Pomocnika `IncludeClientViews`, który powoduje wyświetlenie wszystkich widoków w określonym folderze:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Jeśli nie określono nazwy folderu, domyślna nazwa folderu jest "ClientViews". Jeśli widok klienta używa także widoki częściowe, nazwę widoku częściowego się od znaku podkreślenia (na przykład `_SignUp`). `IncludeClientViews` Metoda nie obejmuje żadnych widoków, których nazwa rozpoczyna się od znaku podkreślenia. Aby uwzględnić widoku częściowego w widoku klienta, należy wywołać `Html.ClientView('SignUp')` zamiast `Html.Partial('_SignUp')`.

**Wysyłanie wiadomości E-mail**

Aby wysłać wiadomość e-mail, są używane w szablonie [pocztowych](http://aboutcode.net/postal). Jednak pocztowych jest wyodrębniony od pozostałej części kodu za pomocą `IMailer` interfejsu, dzięki czemu można łatwo zastąpić go inną implementację. Szablony wiadomości e-mail znajdują się w folderze widoków lub wiadomości E-mail. Adres e-mail nadawcy jest określona w pliku web.config w `sender.email` klucz **appSettings** sekcji. Również gdy `debug="true"` w pliku web.config, aplikacja nie wymaga potwierdzenie adresu e-mail użytkownika, aby przyspieszyć programowanie.

## <a name="github"></a>GitHub

Możesz również znaleźć szablon Backbone.js SPA na [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
