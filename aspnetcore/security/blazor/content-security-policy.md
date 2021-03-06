---
title: Wymuś zasady zabezpieczeń zawartości dla ASP.NET Core Blazor
author: guardrex
description: Dowiedz się, jak używać zasad zabezpieczeń zawartości (CSP) w aplikacjach Blazor ASP.NET Core, aby pomóc w ochronie przed atakami opartymi na wielu lokacjach (XSS).
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/02/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/content-security-policy
ms.openlocfilehash: 1cfebf7b3d3bbb98a671b6f2db7c6518cda74b65
ms.sourcegitcommit: 51c86c003ab5436598dbc42f26ea4a83a795fd6e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2020
ms.locfileid: "78964551"
---
# <a name="enforce-a-content-security-policy-for-aspnet-core-opno-locblazor"></a>Wymuś zasady zabezpieczeń zawartości dla ASP.NET Core Blazor

Autorzy [Javier Calvarro Nelson](https://github.com/javiercn) i [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[Skrypt między lokacjami (XSS)](xref:security/cross-site-scripting) to luka w zabezpieczeniach, w której osoba atakująca umieszcza jeden lub więcej złośliwych skryptów po stronie klienta w renderowanej zawartości aplikacji. Zasady zabezpieczeń zawartości (CSP) pomagają chronić przed atakami typu XSS, informując przeglądarkę o prawidłowym:

* Źródła dla załadowanej zawartości, w tym skrypty, arkusze stylów i obrazy.
* Akcje podejmowane przez stronę, określając dozwolone docelowe adresy URL formularzy.
* Wtyczki, które mogą zostać załadowane.

Aby zastosować dostawcę CSP do aplikacji, deweloper określa kilka *dyrektyw* dotyczących zabezpieczeń zawartości dostawcy CSP w co najmniej jednym `Content-Security-Policy` nagłówku lub `<meta>`.

Zasady są oceniane przez przeglądarkę podczas ładowania strony. Przeglądarka sprawdzi źródła strony i określi, czy spełniają one wymagania dyrektyw dotyczących zabezpieczeń zawartości. Gdy dyrektywy zasad nie są spełnione dla zasobu, przeglądarka nie załaduje zasobu. Rozważmy na przykład zasady, które nie zezwalają na wykonywanie skryptów innych firm. Gdy strona zawiera tag `<script>` z pochodzeniem innej firmy w atrybucie `src`, przeglądarka uniemożliwi załadowanie skryptu.

Dostawca CSP jest obsługiwany w większości nowoczesnych przeglądarek klasycznych i mobilnych, takich jak Chrome, Edge, Firefox, operac i Safari. Dostawca CSP jest zalecany w przypadku aplikacji Blazor.

## <a name="policy-directives"></a>Dyrektywy zasad

Określ w minimalny sposób następujące dyrektywy i źródła dla Blazor aplikacji. Dodaj dodatkowe dyrektywy i źródła zgodnie z wymaganiami. Poniższe dyrektywy są używane w sekcji [stosowanie zasad](#apply-the-policy) w tym artykule, w której znajdują się przykładowe zasady zabezpieczeń dla Blazor webassembly i Blazor Server:

* [identyfikator uri](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/base-uri) &ndash; ogranicza adresy url tagu `<base>` strony. Określ `self`, aby wskazać, że pochodzenie aplikacji, w tym schemat i numer portu, jest prawidłowym źródłem.
* [blok All-Mixed-content](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/block-all-mixed-content) &ndash; uniemożliwia ładowanie mieszanych zawartości http i https.
* [default-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) &ndash; wskazuje rezerwę dla dyrektyw źródłowych, które nie są jawnie określone przez zasady. Określ `self`, aby wskazać, że pochodzenie aplikacji, w tym schemat i numer portu, jest prawidłowym źródłem.
* [img-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/img-src) &ndash; wskazuje prawidłowe źródła obrazów.
  * Określ `data:`, aby zezwolić na ładowanie obrazów z `data:` adresów URL.
  * Określ `https:`, aby zezwolić na ładowanie obrazów z punktów końcowych HTTPS.
* [obiekt-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/object-src) &ndash; wskazuje prawidłowe źródła dla tagów `<object>`, `<embed>`i `<applet>`. Określ `none`, aby uniemożliwić wszystkie źródła adresów URL.
* [skrypt-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/script-src) &ndash; wskazuje prawidłowe źródła dla skryptów.
  * Określ `https://stackpath.bootstrapcdn.com/` Źródło hosta dla skryptów Bootstrap.
  * Określ `self`, aby wskazać, że pochodzenie aplikacji, w tym schemat i numer portu, jest prawidłowym źródłem.
  * W aplikacji Blazor webassembly:
    * Określ następujące skróty, aby zezwolić na załadowanie wymaganych skryptów wbudowanych Blazor webassembly:
      * `sha256-v8ZC9OgMhcnEQ/Me77/R9TlJfzOBqrMTW8e1KuqLaqc=`
      * `sha256-If//FtbPc03afjLezvWHnC3Nbu4fDM04IIzkPaf3pH0=`
      * `sha256-v8v3RKRPmN4odZ1CWM5gw80QKPCCWMcpNeOmimNL2AA=`
    * Określ `unsafe-eval`, aby użyć `eval()` i metod tworzenia kodu z ciągów.
  * W aplikacji serwera Blazor Określ skrót `sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=` dla skryptu wbudowanego, który wykonuje wykrywanie powrotu dla arkuszy stylów.
* [styl-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/style-src) &ndash; wskazuje prawidłowe źródła dla arkuszy stylów.
  * Określ `https://stackpath.bootstrapcdn.com/` Źródło hosta dla arkuszy stylów ładowania.
  * Określ `self`, aby wskazać, że pochodzenie aplikacji, w tym schemat i numer portu, jest prawidłowym źródłem.
  * Określ `unsafe-inline`, aby zezwolić na używanie wbudowanych stylów. Deklaracja wbudowana jest wymagana dla interfejsu użytkownika w aplikacjach Blazor Server na potrzeby ponownego łączenia klienta i serwera po początkowym żądaniu. W przyszłej wersji, wbudowane style mogą zostać usunięte, aby `unsafe-inline` nie było już wymagane.
* [uaktualnianie — niezabezpieczone-żądania](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/upgrade-insecure-requests) &ndash; wskazuje, że adresy URL zawartości ze źródeł niezabezpieczonych (http) powinny zostać bezpiecznie pobrane za pośrednictwem protokołu HTTPS.

Powyższe dyrektywy są obsługiwane przez wszystkie przeglądarki z wyjątkiem programu Microsoft Internet Explorer.

Aby uzyskać skróty SHA dla dodatkowych skryptów wbudowanych:

* Zastosuj dostawcę CSP pokazany w sekcji [Zastosuj zasady](#apply-the-policy) .
* Uzyskaj dostęp do konsoli narzędzia deweloperskie w przeglądarce podczas lokalnego uruchamiania aplikacji. Przeglądarka oblicza i wyświetla skróty dla zablokowanych skryptów, gdy występuje nagłówek CSP lub tag `meta`.
* Skopiuj skróty udostępnione przez przeglądarkę do źródeł `script-src`. Używaj pojedynczych cudzysłowów wokół każdego skrótu.

Aby uzyskać informacje na temat obsługi macierzy dla poziomu zasad zabezpieczeń zawartości w przeglądarce, zobacz [: Czy można korzystać z poziomu zasad zabezpieczeń zawartości 2](https://www.caniuse.com/#feat=contentsecuritypolicy2).

## <a name="apply-the-policy"></a>Zastosowanie zasad

Użyj tagu `<meta>`, aby zastosować zasady:

* Ustaw wartość atrybutu `http-equiv` na `Content-Security-Policy`.
* Umieść dyrektywy w `content` wartość atrybutu. Oddzielne dyrektywy z średnikiem (`;`).
* Zawsze umieszczaj tag `meta` w zawartości `<head>`.

W poniższych sekcjach przedstawiono przykładowe zasady dla Blazor webassembly i Blazor Server. Te przykłady dotyczą wersji tego artykułu dla każdej wersji Blazor. Aby użyć wersji odpowiedniej dla wersji, wybierz wersję dokumentu z selektorem listy rozwijanej **wersji** na tej stronie sieci Web.

### <a name="opno-locblazor-webassembly"></a>Blazor webassembly

W `<head>` zawartości strony hosta *wwwroot/index.html* Zastosuj dyrektywy opisane w sekcji [dyrektywy zasad](#policy-directives) :

```html
<meta http-equiv="Content-Security-Policy" 
      content="base-uri 'self';
               block-all-mixed-content;
               default-src 'self';
               img-src data: https:;
               object-src 'none';
               script-src https://stackpath.bootstrapcdn.com/ 
                          'self' 
                          'sha256-v8ZC9OgMhcnEQ/Me77/R9TlJfzOBqrMTW8e1KuqLaqc=' 
                          'sha256-If//FtbPc03afjLezvWHnC3Nbu4fDM04IIzkPaf3pH0=' 
                          'sha256-v8v3RKRPmN4odZ1CWM5gw80QKPCCWMcpNeOmimNL2AA=' 
                          'unsafe-eval';
               style-src https://stackpath.bootstrapcdn.com/
                         'self'
                         'unsafe-inline';
               upgrade-insecure-requests;">
```

### <a name="opno-locblazor-server"></a>Serwer Blazor

W `<head>` zawartości strony hosta *stron/_Host. cshtml* Zastosuj dyrektywy opisane w sekcji [dyrektywy zasad](#policy-directives) :

```cshtml
<meta http-equiv="Content-Security-Policy" 
      content="base-uri 'self';
               block-all-mixed-content;
               default-src 'self';
               img-src data: https:;
               object-src 'none';
               script-src https://stackpath.bootstrapcdn.com/ 
                          'self' 
                          'sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=';
               style-src https://stackpath.bootstrapcdn.com/
                         'self' 
                         'unsafe-inline';
               upgrade-insecure-requests;">
```

## <a name="meta-tag-limitations"></a>Ograniczenia tagów Meta

Zasady tagu `<meta>` nie obsługują następujących dyrektyw:

* [Ramka — elementy nadrzędne](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/frame-ancestors)
* [Zgłoś do](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
* [Raport — identyfikator URI](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-uri)
* [rozwiązania](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/sandbox)

Aby zapewnić obsługę powyższych dyrektyw, użyj nagłówka o nazwie `Content-Security-Policy`. Ciąg dyrektywy jest wartością nagłówka.

## <a name="test-a-policy-and-receive-violation-reports"></a>Testowanie zasad i otrzymywanie raportów o naruszeniu

Testowanie pomaga upewnić się, że skrypty innych firm nie są przypadkowo blokowane podczas kompilowania zasad początkowych.

Aby przetestować zasady w określonym czasie bez wymuszania dyrektyw zasad, `<meta>` ustaw atrybut `http-equiv` lub nazwę nagłówka zasad opartych na nagłówkach, które mają być `Content-Security-Policy-Report-Only`. Raporty o błędach są wysyłane jako dokumenty JSON do określonego adresu URL. Aby uzyskać więcej informacji, zobacz [powiadomienia MDN Web docs: Content-Security-Policy-Report-Only](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy-Report-Only).

Aby zgłaszać naruszenia zasad, gdy zasady są aktywne, zobacz następujące artykuły:

* [Zgłoś do](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
* [Raport — identyfikator URI](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-uri)

Mimo że `report-uri` nie jest już zalecane do użycia, obie dyrektywy należy stosować do momentu, gdy `report-to` jest obsługiwany przez wszystkie główne przeglądarki. Nie używaj wyłącznie `report-uri`, ponieważ obsługa `report-uri` może być porzucana *w dowolnym momencie* w przeglądarkach. Usuń obsługę `report-uri` w zasadach, gdy `report-to` jest w pełni obsługiwany. Aby śledzić przyjęcie `report-to`, zobacz temat [czy można użyć: raport-do](https://caniuse.com/#feat=mdn-http_headers_csp_content-security-policy_report-to).

Testowanie i aktualizowanie zasad aplikacji w każdej wersji.

## <a name="troubleshoot"></a>Rozwiązywanie problemów

* Błędy są wyświetlane w konsoli narzędzia deweloperskie w przeglądarce. Przeglądarki zawierają informacje o:
  * Elementy, które nie są zgodne z zasadami.
  * Jak zmodyfikować zasady, aby umożliwić zablokowanie elementu.
* Zasady są całkowicie skuteczne tylko wtedy, gdy przeglądarka klienta obsługuje wszystkie dyrektywy dołączone. Aby zapoznać się z bieżącą matrycą obsługi, zobacz temat [: Content-Security-Policy](https://caniuse.com/#search=Content-Security-Policy).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [POWIADOMIENIA MDN Web docs: Content-Security-Policy](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy)
* [Poziom zabezpieczeń zawartości 2](https://www.w3.org/TR/CSP2/)
* [Ewaluatora usługi Google CSP](https://csp-evaluator.withgoogle.com/)
