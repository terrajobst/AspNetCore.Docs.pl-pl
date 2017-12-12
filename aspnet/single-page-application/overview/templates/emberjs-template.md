---
uid: single-page-application/overview/templates/emberjs-template
title: Szablon EmberJS | Dokumentacja firmy Microsoft
author: xqiu
description: Szablon EmberJS
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="emberjs-template"></a>Szablon EmberJS
====================
przez [Xinyang Qiu](https://github.com/xqiu)

> Szablon MVC EmberJS została napisana przez Nathan Totten, Thiago Santos i Xinyang Qiu.
> 
> [Pobieranie szablonu EmberJS MVC](https://go.microsoft.com/fwlink/?LinkId=282647)


Szablon EmberJS SPA został zaprojektowany ułatwiających rozpoczęcie pracy szybkie opracowywanie aplikacji sieci web po stronie klienta interactive przy użyciu EmberJS.

"Jednostronicowej aplikacji" (SPA) jest ogólny termin dla aplikacji sieci web, który ładuje pojedynczej strony HTML, a następnie aktualizuje stronę dynamicznie, zamiast ładowania nowych stron. Po załadowaniu stronę początkową aplikacja JEDNOSTRONICOWA komunikuje się z serwerem za pośrednictwem żądania AJAX.

![](emberjs-template/_static/image1.png)

AJAX ma nowych danych, ale obecnie istnieją struktury języka JavaScript, ułatwiające tworzenie i obsługa dużej aplikacji JEDNOSTRONICOWEJ zaawansowane. Ponadto HTML 5 i CSS3 są ułatwiając tworzenie rozbudowanych UI.

Szablon SPA EmberJS używa [Członkowskimi](http://emberjs.com/) biblioteka języka JavaScript do obsługi aktualizacji strony z żądania AJAX. Ember.js używa powiązanie danych w celu synchronizowania strony przy użyciu najnowszych danych. Dzięki temu nie trzeba pisać kod, który przeprowadzi Cię przez dane JSON i aktualizuje modelu DOM. Zamiast tego należy umieścić deklaratywne atrybuty HTML, który poinformuje Ember.js jak przedstawiają dane.

Po stronie serwera jest niemal identyczny szablonu EmberJS [szablonu SPA elementami KnockoutJS](../introduction/knockoutjs-template.md). ASP.NET MVC jest używany do obsługi dokumentów HTML i ASP.NET Web API do obsługi żądań AJAX z klienta. Aby uzyskać więcej informacji na temat tych aspektów szablonu dotyczą [szablonu elementami KnockoutJS](../introduction/knockoutjs-template.md) dokumentacji. Ten temat koncentruje się na temat różnic między szablon Knockout i szablon EmberJS.

## <a name="create-an-emberjs-spa-template-project"></a>Tworzenie projektu szablonu SPA EmberJS

Pobierz i zainstaluj szablonu, klikając przycisk Pobierz powyżej. Może być konieczne ponowne uruchomienie programu Visual Studio.

W **szablony** okienku wybierz **zainstalowane szablony** i rozwiń **Visual C#** węzła. W obszarze **Visual C#**, wybierz pozycję **Web**. Na liście szablony projektów, wybierz **aplikacji sieci Web programu ASP.NET MVC 4**. Nazwij projekt i kliknij przycisk **OK**.

![](emberjs-template/_static/image2.png)

W **nowy projekt** kreatora wybierz **Ember.js SPA projektu**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Omówienie szablonu SPA EmberJS

Szablon EmberJS używa kombinacji jQuery, Ember.js, Handlebars.js, aby utworzyć smooth, interaktywnego interfejsu użytkownika.

Ember.js jest biblioteka języka JavaScript, który korzysta ze wzorca MVC po stronie klienta.

- A *szablonu*robaków napisanych w języku tworzenia szablonów odległość, w tym artykule opisano interfejs użytkownika aplikacji. W trybie wersji [kompilatora odległość](https://github.com/Myslik/csharp-ember-handlebars) służy do pakietu i skompilować odległość szablonu.
- A *modelu* przechowuje dane aplikacji, która otrzymuje od serwera (ToDo list i zadań do wykonania).
- A *kontrolera* przechowuje stan aplikacji. Kontrolery często zawierają dane modelu do odpowiednich szablonów.
- A *widoku* tłumaczy pierwotnych zdarzeń z aplikacji i przekazuje je do kontrolera.
- A *router* zarządza stanem aplikacji, synchronizacja szablony i adresy URL.

Ponadto biblioteka Członkowskimi danych można zsynchronizować obiektów JSON (uzyskane z serwerem za pośrednictwem interfejsu API RESTful) i modele klienta.

Szablon EmberJS SPA organizuje skrypty w ośmiu warstwy:

- webapi\_adapter.js, webapi\_serializer.js: rozszerza biblioteki Członkowskimi danych do pracy z interfejsu API sieci Web platformy ASP.NET.
- Scripts/helpers.js: Określa nowe pomocników odległość Członkowskimi.
- Scripts/App.js: Tworzenie aplikacji i konfiguruje serializator i karty.
- Skrypty/app/modeli/\*js: definiuje modeli.
- Widokówskryptów/app/\*js: zawiera definicje widoków.
- Skrypty/app/kontrolerów/\*js: definiuje kontrolerów.
- Skrypty/app/tras, Scripts/app/router.js: Definiuje trasy.
- Szablony /\*.hbs: definiuje szablony odległość.

Oto niektóre z tych skryptów bardziej szczegółowo.

## <a name="models"></a>Modele

Modele definiuje się w folderze skryptów/app/modeli. Istnieją dwa pliki modelu: todoItem.js i todoList.js.

**TODO.model.js** definiuje modeli po stronie klienta (przeglądarki) do listy zadań do wykonania. Istnieją dwie klasy modelu: todoItem i listy zadań. W Członkowskimi modele są podklasy DS. Model. Model może mieć właściwości z atrybutami:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Modele można zdefiniować relacji z innymi modelami:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Modele mogą mieć obliczana właściwości, które wiążą się do innych właściwości:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Modele może mieć obserwatora funkcji, które są wywoływane, gdy właściwość obserwowanych:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Widoki

Widoki są definiowane w folderze skryptów/app/widoków. Widok tłumaczy zdarzeń z aplikacji interfejsu użytkownika. Program obsługi zdarzeń można wywołania zwrotnego funkcje kontrolera lub po prostu bezpośrednio wywołać kontekstu danych.

Na przykład następujący kod jest z views/TodoItemEditView.js. Definiuje zdarzenia obsługi dla pola wejściowego tekstu.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Kontrolera

Kontrolery są definiowane w folderze skryptów/app/kontrolerów. Do reprezentowania pojedynczego modelu, należy rozszerzyć `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Kontroler może również reprezentować kolekcji modeli przez rozszerzenie `Ember.ArrayController`. Na przykład TodoListController reprezentuje tablicę `todoList` obiektów. Kontroler sortowane według Identyfikatora listy zadań, w kolejności malejącej:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

Kontroler definiuje funkcję o nazwie `addTodoList`, która tworzy nowy todoList i dodaje go do tablicy. Aby zobaczyć, jak ta funkcja jest wywoływana, otwórz plik szablonu o nazwie todoListTemplate.html w folderze szablonów. Poniższy kod szablonu wiąże przycisk, aby `addTodoList` funkcji:

[!code-html[Main](emberjs-template/samples/sample8.html)]

Zawiera także kontrolerem `error` właściwość, która zawiera komunikat o błędzie. Oto kod szablonu, aby wyświetlić komunikat o błędzie (również w todoListTemplate.html):

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Trasy

Router.js definiuje tras i domyślny szablon do wyświetlenia, ustawia stan aplikacji i pasuje do tras adresów URL:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js ładuje dane dla TodoListRoute przez zastąpienie funkcja setupController:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Członkowskimi używa konwencji nazewnictwa w celu dopasowania adresy URL, nazwy tras, kontrolerów i szablony. Aby uzyskać więcej informacji, zobacz [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) w dokumentacji EmberJS.

## <a name="templates"></a>Szablony

Folder szablonów zawiera cztery szablony:

- Application.HBS: domyślny szablon, który jest renderowany po uruchomieniu aplikacji.
- About.HBS: szablonu trasy "/ około".
- index.HBS: szablon dla głównego trasy "/".
- todoList.hbs: szablonu dla "/ todo" trasy.
- \_navbar.HBS: szablon definiuje menu nawigacji.

Szablon aplikacji działa jak strony wzorcowej. Zawiera nagłówek, stopka i "{{gniazda}}" do wstawienia innych szablonów w zależności od trasy. Aby uzyskać więcej informacji na temat szablonów aplikacji w Członkowskimi, zobacz [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

"/ TodoList" szablon zawiera dwóch wyrażeń pętli. Pętla poza `{{#each controller}}`oraz wewnątrz pętli jest `{{#each todos}}`. Poniższy kod przedstawia wbudowaną `Ember.Checkbox` wyświetlić dostosowany `App.TodoItemEditView`i link `deleteTodo` akcji.

[!code-html[Main](emberjs-template/samples/sample12.html)]

`HtmlHelperExtensions` Pomocnika definiuje klasa zdefiniowana w Controllers/HtmlHelperExensions.cs, funkcja do buforowania i Wstaw szablon pliki, kiedy **debugowania** ma ustawioną wartość **true** w pliku Web.config. Ta funkcja jest wywoływana z pliku widoku programu ASP.NET MVC zdefiniowane w Views/Home/App.cshtml:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Wywołać bez argumentów funkcji renderuje wszystkie pliki szablonów w folderze szablonów. Można również określić podfolderu lub pliku określonego szablonu.

Gdy **debugowania** jest **false** w pliku Web.config, aplikacja zawiera element pakietu "~/bundles/templates". Ten element pakiet został dodany w BundleConfig.cs za pomocą biblioteki kompilatora odległość:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
