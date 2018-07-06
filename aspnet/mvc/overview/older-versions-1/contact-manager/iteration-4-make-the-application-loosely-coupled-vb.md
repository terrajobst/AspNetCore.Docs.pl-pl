---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
title: 'Iteracja 4 # — wprowadzić luźne sprzężenie aplikacji (VB) | Dokumentacja firmy Microsoft'
author: microsoft
description: W tym trzeci iteracji możemy skorzystać z kilku wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Aby uzyskać...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 92c70297-4430-4e4e-919a-9c2333a8d09a
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
msc.type: authoredcontent
ms.openlocfilehash: 246069902e67d3d901bbd40e950198825a5ab01d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804675"
---
<a name="iteration-4--make-the-application-loosely-coupled-vb"></a>Iteracja 4 # — wprowadzić luźne sprzężenie aplikacji (VB)
====================
przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz program Code](iteration-4-make-the-application-loosely-coupled-vb/_static/contactmanager_4_vb1.zip)

> W tym trzeci iteracji możemy skorzystać z kilku wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Na przykład możemy refaktoryzować naszej aplikacji do korzystania z wzorca repozytorium i wzorzec iniekcji zależności.


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Tworzenie aplikacji zarządzania kontaktami platformy ASP.NET MVC (VB)

W tej serii samouczków wbudowujemy całej aplikacji zarządzania skontaktuj się z od początku do zakończenia. Aplikacja Contact Manager umożliwia przechowywanie informacji kontaktowych — nazwy, numerów telefonów i adresów e-mail — lista osób.

Firma Microsoft tworzy aplikację za pośrednictwem wiele iteracji. Z każdą iteracją można stopniowo ulepszyć aplikację. Celem tego wielu podejścia iteracji jest, aby umożliwić Ci zrozumienie przyczyn wprowadzenia poszczególnych zmian.

- Iteracja #1 — Tworzenie aplikacji. W pierwszej iteracji utworzymy Contact Manager w najprostszym sposobem możliwe. Dodano obsługę operacji podstawowej bazy danych: tworzenia, odczytu, aktualizacji i usuwania (CRUD).

- Iteracja 2 # — należy wyglądu nieuprzywilejowany aplikacji. W tej iteracji możemy poprawić wygląd aplikacji przez zmodyfikowanie domyślnych strony wzorcowej widoku platformy ASP.NET MVC i kaskadowych arkuszy stylów.

- Iteracja #3 — Dodawanie weryfikacji formularza. W trzecim iteracji dodamy weryfikacji formularza podstawowego. Firma Microsoft ochronić przed przesłaniem formularza nie kończą działania wymaganych pól formularza. Możemy zweryfikować adresy e-mail oraz numerów telefonów.

- Iteracja 4 # — należy luźne sprzężenie aplikacji. W tym trzeci iteracji możemy skorzystać z kilku wzorców projektowych oprogramowania, aby ułatwić konserwację i modyfikowanie aplikacji Contact Manager. Na przykład możemy refaktoryzować naszej aplikacji do korzystania z wzorca repozytorium i wzorzec iniekcji zależności.

- Iteracja #5 — Tworzenie testów jednostkowych. W piątej iteracji ułatwiamy naszej aplikacji ułatwia konserwację i modyfikowanie, dodając testów jednostkowych. Firma Microsoft testowanie naszych zajęć modelu danych i tworzenie testów jednostkowych dla naszych kontrolery i logikę weryfikacji.

- Iteracja #6 — korzystanie z projektowania opartego na testach. W tym szóstego iteracji dodamy nowe funkcje do naszej aplikacji, najpierw pisanie testów jednostkowych i pisanie kodu dla testów jednostkowych. W tym iteracji dodamy grup kontaktów.

- Iteracja #7 — dodawanie funkcji Ajax. W siódmej iteracji można ulepszyć czas odpowiedzi i wydajności naszych aplikacji przez dodanie obsługi technologii AJAX.

## <a name="this-iteration"></a>Tej iteracji

W czwartym iteracji aplikacji Contact Manager refaktoryzacji aplikacji, aby wprowadzić więcej luźne sprzężenie aplikacji. Gdy aplikacja jest luźno powiązane, można zmodyfikować kod w jednej części aplikacji bez konieczności modyfikowania kodu w innych częściach aplikacji. Aplikacje luźno powiązane są bardziej odporne na zmiany.

Obecnie wszystkie logika dostępu i sprawdzanie poprawności danych używanych przez aplikację Contact Manager znajduje się do klas kontrolera. Jest to zły pomysł. Zawsze, gdy trzeba zmodyfikować jednej części aplikacji, istnieje ryzyko wprowadzenia błędów do innej części aplikacji. Jeśli zmodyfikujesz logikę weryfikacji, istnieje ryzyko na przykład wprowadzenie nowych usterek do logiki dostępu lub kontrolera danych.

> [!NOTE] 
> 
> (SRP), klasy nigdy nie powinny mieć więcej niż jednym z powodów zmiany. Mieszanie kontrolera, weryfikacji i logiki bazy danych jest ogromną naruszenie jednej zasady odpowiedzialności.


Istnieje kilka przyczyn, które może być konieczne zmodyfikowanie aplikacji. Może być konieczne dodanie nowej funkcji do aplikacji, może być konieczne naprawienie usterki w aplikacji lub konieczne może się okazać zmodyfikowanie implementacji funkcji aplikacji. Aplikacje są rzadko statyczne. Charakteryzują się rozwijać i mutować wraz z upływem czasu.

Wyobraź sobie, na przykład, możesz zdecydować zmienić sposób implementacji usługi warstwy dostępu do danych. Po prawej stronie, aplikacja Contact Manager używa teraz Microsoft Entity Framework dostęp do bazy danych. Jednak można zdecydować przeprowadzić migrację do technologii dostępu do nowych lub alternatywne danych, takich jak architektury ADO.NET Data Services lub NHibernate. Jednak ponieważ kod dostępu do danych nie jest odizolowana od kodu sprawdzania poprawności i kontroler, istnieje żaden sposób modyfikować kod dostępu do danych w aplikacji bez konieczności modyfikacji innego kodu, który nie jest bezpośrednio związane z dostępem do danych.

Gdy aplikacja jest luźno powiązane, z drugiej strony zmiana wprowadzona do jednej części aplikacji bez ingerowania w pozostałe części aplikacji. Na przykład możesz przełączyć technologii dostępu do danych bez modyfikowania logikę weryfikacji lub kontrolera.

W tym iteracji możemy skorzystać z kilka wzorców projektowania oprogramowania, które pozwalają nam Refaktoryzuj naszej aplikacji Contact Manager do bardziej luźno powiązanych aplikacji. Gdy firma Microsoft będzie gotowe, Contact Manager wygrał t nic robić, jego t są przeprowadzić przed. Jednak firma Microsoft będzie można zmienić w przyszłości łatwiej aplikacji.

> [!NOTE] 
> 
> Refaktoryzacja jest proces ponownego zapisywania aplikacji w taki sposób, że nie utracić wszelkie istniejące funkcje.


## <a name="using-the-repository-software-design-pattern"></a>Przy użyciu wzorca projektowego oprogramowania repozytorium

Nasz pierwszy zmiana ma na celu skorzystaj z zalet wzorzec projektowania oprogramowania o nazwie wzorca repozytorium. Użyjemy wzorca repozytorium do izolowania nasz kod dostępu do danych z pozostałą część naszej aplikacji.

Implementacja wzorca repozytorium wymaga od nas wykonaj następujące dwa kroki:

1. Tworzenie interfejsu
2. Utwórz konkretną klasę, która implementuje interfejs

Najpierw musimy utworzyć interfejs, który zawiera opis wszystkich metod dostępu do danych, które należy wykonać. Interfejs IContactManagerRepository znajduje się w ofercie 1. Ten interfejs w tym artykule opisano pięć metod: CreateContact(), DeleteContact(), EditContact(), GetContact i ListContacts().

**Wyświetlanie listy 1 - Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample1.vb)]

Następnie należy utworzyć konkretną klasę, która implementuje interfejs IContactManagerRepository. Ponieważ firma Microsoft korzysta z programu Entity Framework Microsoft dostęp do bazy danych, utworzymy nową klasę o nazwie EntityContactManagerRepository. Ta klasa znajduje się w ofercie 2.

**Wyświetlanie listy 2 - Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample2.vb)]

Należy zauważyć, że klasa EntityContactManagerRepository implementuje interfejs IContactManagerRepository. Klasa implementuje wszystkich pięciu z metod opisanych w tym interfejsie.

Być może zastanawiasz się, dlaczego musimy odblokowane za pomocą interfejsu. Dlaczego musimy utworzyć interfejs i klasę, która implementuje go?

Z jednym wyjątkiem pozostała część naszej aplikacji będą korzystać z interfejsu i klasy konkretnej. Zamiast wywoływania metod udostępnianych przez klasę EntityContactManagerRepository, zadzwonimy metod udostępnianych przez interfejs IContactManagerRepository.

W ten sposób możemy implementować interfejs z nową klasę bez konieczności modyfikowania pozostałą część naszej aplikacji. Na przykład w przyszłości, firma Microsoft ma Implementowanie klasy DataServicesContactManagerRepository, który implementuje interfejs IContactManagerRepository. Klasy DataServicesContactManagerRepository może używać usług ADO.NET Data Services dostępu do bazy danych zamiast Microsoft Entity Framework.

Jeśli w kodzie naszej aplikacji jest programowane interfejsu IContactManagerRepository zamiast konkretnych klas EntityContactManagerRepository następnie możemy przełączyć konkretnych klas bez modyfikowania pozostałe naszego kodu. Na przykład możemy przełączyć się z klasy EntityContactManagerRepository do klasy DataServicesContactManagerRepository bez modyfikowania naszych logika dostępu lub sprawdzania poprawności danych.

Programowanie w oparciu o interfejsy (abstrakcje) zamiast konkretnych klas sprawia, że nasza aplikacja jest bardziej odporne na zmiany.

> [!NOTE] 
> 
> Można szybko utworzyć interfejs z klasą konkretną z poziomu programu Visual Studio, wybierając opcję menu refaktoryzacji, Wyodrębnij interfejs. Na przykład można najpierw utworzyć klasy EntityContactManagerRepository, a następnie użyć do automatycznego wygenerowania interfejsu IContactManagerRepository Wyodrębnij interfejs.


## <a name="using-the-dependency-injection-software-design-pattern"></a>Przy użyciu wzorca projektowego oprogramowania wstrzykiwania zależności

Teraz, gdy będziemy zostały przeniesione nasz kod dostępu do danych do osobnej klasy repozytorium, należy zmodyfikować kontroler naszych kontakt, aby użyć tej klasy. Firma Microsoft będzie korzystać z wzorzec projektowania oprogramowania o nazwie wstrzykiwanie zależności, aby użyć klasy repozytorium w kontrolera.

Zmodyfikowane kontrolera skontaktuj się z pomocą znajduje się w ofercie 3.

**Wyświetlanie listy 3 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample3.vb)]

Należy zauważyć, że kontroler kontaktu w ofercie 3 ma dwa konstruktory. Pierwszy Konstruktor przekazuje konkretne wystąpienie interfejsu IContactManagerRepository drugi Konstruktor. Skontaktuj się z kontrolerem klasy wykorzystuje *iniekcji zależności konstruktora*.

Jeden i tylko miejsce służy klasa EntityContactManagerRepository jest pierwszy Konstruktor. W pozostałej części klasy używa interfejsu IContactManagerRepository zamiast konkretnych klas EntityContactManagerRepository.

Dzięki temu można łatwo zmienić implementacji klasy IContactManagerRepository w przyszłości. Jeśli chcesz użyć klasy DataServicesContactRepository zamiast klasy EntityContactManagerRepository, po prostu zmodyfikuj pierwszy Konstruktor.

Wstrzykiwanie zależności Konstruktor sprawia, że klasa kontrolera skontaktuj się z bardzo sprawdzalnego działa zgodnie. Testy jednostkowe można utworzyć wystąpienie skontaktuj się z kontrolerem, przekazując implementację testową klasy IContactManagerRepository. Ta funkcja iniekcji zależności będzie dla nas bardzo ważna w następnej iteracji, gdy będziemy tworzyć testy jednostkowe dla aplikacji Contact Manager.

> [!NOTE] 
> 
> Jeśli chcesz całkowicie oddzielić klasy kontrolera kontaktu z określonej implementacji interfejsu IContactManagerRepository następnie możesz korzystać z zalet strukturę, która obsługuje wstrzykiwanie zależności, takie jak StructureMap lub firmy Microsoft Entity Framework (MEF). Dzięki wykorzystaniu framework wstrzykiwanie zależności nie będą już potrzebne do odwoływania się do konkretnej klasy w kodzie.


## <a name="creating-a-service-layer"></a>Tworzenie warstwy usług

Być może Zauważyłeś, że nasze logikę weryfikacji jest nadal mieszana z naszych logiką kontrolera w klasie kontrolera zmodyfikowane w ofercie 3. Z tego samego powodu jest dobry pomysł, aby odizolować naszych logiką dostępu do danych jest dobry pomysł, aby odizolować naszych logikę weryfikacji.

Aby rozwiązać ten problem, można utworzyć oddzielny [warstwy usług](http://martinfowler.com/eaaCatalog/serviceLayer.html). Warstwy usług jest oddzielne warstwy, która Wstaw między naszych kontrolera i klasy repozytorium. Warstwy usługi zawiera naszych logiki biznesowej, w tym wszystkie nasze logikę weryfikacji.

ContactManagerService znajduje się w ofercie 4. Zawiera logikę weryfikacji z klasy controller kontaktu.

**Wyświetlanie listy 4 - Models\ContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample4.vb)]

Należy zauważyć, że Konstruktor ContactManagerService wymaga ValidationDictionary. Warstwy usług komunikuje się z warstwą kontrolera za pośrednictwem tego ValidationDictionary. Omawiane ValidationDictionary szczegółowo w poniższej sekcji, gdy omówimy wzorzec Dekoratora.

Zwróć uwagę, co więcej, że ContactManagerService implementuje interfejs IContactManagerService. Należy zawsze wszelkich starań, aby programować przy użyciu interfejsów zamiast konkretnych klas. Inne klasy w aplikacji Contact Manager wchodzi w interakcję z klasą ContactManagerService bezpośrednio. Zamiast tego z jednym wyjątkiem pozostałą częścią aplikacji Contact Manager jest programowane interfejsu IContactManagerService.

Interfejs IContactManagerService znajduje się w ofercie 5.

**Wyświetlanie listy 5 - Models\IContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample5.vb)]

Zmodyfikowane klasy kontrolera kontaktu znajduje się w ofercie 6. Należy zauważyć, że kontroler skontaktuj się z już nie wchodzi w interakcję z repozytorium ContactManager. Zamiast tego kontrolera skontaktuj się z pomocą wchodzi w interakcję z usługą ContactManager. Każda warstwa jest izolowana jak najszerzej z innych warstw.

**Wyświetlanie listy 6 - Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample6.vb)]

Nasza aplikacja już nie działa afoul o pojedynczej odpowiedzialności zasady (SRP). Skontaktuj się z kontrolera w ofercie 6 zostały usunięte odpowiedzialności, co inne niż sterowanie przepływem wykonania aplikacji. Całą logikę weryfikacji został usunięty z kontrolera skontaktuj się z pomocą i wypchnięte do warstwy usług. Całą logikę bazy danych zostało wypchnięte do warstwy repozytorium.

## <a name="using-the-decorator-pattern"></a>Przy użyciu wzorca Dekoratora

Chcemy można było całkowicie oddzielić nasze warstwy usług z naszych warstwy kontrolera. W zasadzie firma Microsoft należy skompilować nasze warstwy usług w oddzielnym zestawie z naszych warstwy kontrolera bez konieczności dodawania odwołania do naszej aplikacji MVC.

Jednak nasze warstwy usług musi mieć możliwość przekazywania komunikatów o błędach weryfikacji do warstwy kontrolera. Jak możemy włączyć warstwy usługi, aby komunikować się komunikaty o błędach weryfikacji bez sprzężenia kontrolera i warstwy usług Będziemy korzystać z zalet wzorzec projektowania oprogramowania o nazwie [wzorzec Dekoratora](http://en.wikipedia.org/wiki/Decorator_pattern).

Kontroler używa ModelStateDictionary, o nazwie ModelState do reprezentowania błędy sprawdzania poprawności. W związku z tym być może uznasz, że do przekazania ModelState z kontrolera warstwy do warstwy usług. Jednak w warstwie usługi za pomocą ModelState czyniłyby warstwą usługi zależne od funkcji platformę ASP.NET MVC. Powinien to być nieprawidłowy, ponieważ w przyszłości, możesz chcieć użyć warstwy usług z aplikacji WPF, nie aplikacji ASP.NET MVC. W takim przypadku wouldn t ma dotyczyć odwołanie struktury ASP.NET MVC, aby użyć klasy ModelStateDictionary.

Wzorzec Dekoratora można opakować istniejącej klasy w nowej klasy, aby zaimplementować interfejs. Projekcie Contact Manager zawiera klasę ModelStateWrapper zawarte w ofercie 7. Klasa ModelStateWrapper implementuje interfejs w ofercie 8.

**Wyświetlanie listy 7 - Models\Validation\ModelStateWrapper.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample7.vb)]

**Wyświetlanie listy 8 - Models\Validation\IValidationDictionary.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample8.vb)]

Jeśli Zamknij Spójrz na listę 5 następnie zobaczysz warstwy usług ContactManager wyłącznie używa interfejsu IValidationDictionary. Usługa ContactManager jest zależne od klasy ModelStateDictionary. Gdy kontroler kontaktu tworzy usługę ContactManager, kontroler opakowuje jego ModelState następująco:

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample9.vb)]

## <a name="summary"></a>Podsumowanie

W tym iteracji firma Microsoft nie dodano żadnych nowych funkcji do aplikacji Contact Manager. Celem tej iteracji było Refaktoryzuj wtedy ułatwia konserwację i modyfikowanie aplikacji Contact Manager.

Po pierwsze wprowadziliśmy wzorzec projektowy oprogramowania repozytorium. Firma Microsoft migrację wszystkich kod dostępu do danych do osobnej klasy ContactManager repozytorium.

Firma Microsoft również samodzielnie naszych logikę weryfikacji z naszych logiką kontrolera. Utworzyliśmy warstwy osobną usługą, która zawiera wszystkie nasze kod sprawdzania poprawności. Warstwa kontrolera wchodzi w interakcję z warstwy usług i warstwy usług korzysta z warstwy repozytorium.

Podczas tworzenia warstwy usług, firma Microsoft skorzystała wzorca Dekoratora do izolowania ModelState od naszych warstwy usług. W naszym warstwy usług, firma Microsoft zaprogramowane interfejsu IValidationDictionary zamiast ModelState.

Ponadto firma Microsoft skorzystała z wzorzec projektowania oprogramowania o nazwie wzorzec iniekcji zależności. Ten wzorzec pozwala nam programować przy użyciu interfejsów (abstrakcje) zamiast konkretnych klas. Implementowanie wzorca projektowego wstrzykiwanie zależności sprawia, że nasz kod bardziej sprawdzalnego działa zgodnie. W następnej iteracji dodamy testów jednostkowych do naszego projektu.

> [!div class="step-by-step"]
> [Poprzednie](iteration-3-add-form-validation-vb.md)
> [dalej](iteration-5-create-unit-tests-vb.md)
