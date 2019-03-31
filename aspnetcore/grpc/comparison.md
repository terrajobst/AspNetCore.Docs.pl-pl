---
title: Porównanie usług gRPC za pomocą interfejsów API protokołu HTTP
author: jamesnk
description: Dowiedz się, jak porównuje gRPC za pomocą interfejsów API protokołu HTTP i co ma zaleca się, że scenariusze są.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 03/26/2019
uid: grpc/comparison
ms.openlocfilehash: 280d0c2be2a83e5d80cedeaa472e33c28ac983f9
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750495"
---
# <a name="comparing-grpc-and-http-apis"></a>Porównanie gRPC i interfejsów API protokołu HTTP

Przez [James Newton — King](https://twitter.com/jamesnk)

W tym artykule przedstawiono porównanie [gRPC](https://grpc.io/docs/guides/) i interfejsów API protokołu HTTP i zaleca scenariusze użycia gRPC za pośrednictwem innych technologii.

#### <a name="overview"></a>Omówienie

|    Funkcja             |    gRPC                                                 |    Interfejsy API protokołu HTTP przy użyciu formatu JSON                       |
|------------------------|---------------------------------------------------------|----------------------------------------------|
|    Kontrakt            |    Wymagane (`*.proto`)                                 |    Opcjonalnie (OpenAPI)                        |
|    Transport           |    HTTP/2                                               |    HTTP                                      |
|    ładunek             |    [Formatu Protobuf (mały, dane binarne)](#performance)             |    JSON (dużych do odczytu)              |
|    Prescriptiveness    |    [Specyfikacja Strict](#strict-specification)        |    Luźne. Wszelkie HTTP jest prawidłowa                  |
|    Przesyłanie strumieniowe           |    [Klienta, serwera, dwukierunkowych](#streaming)         |    Klienta, serwera                            |
|    Obsługa przeglądarek     |    [Nie (wymaga grpc w sieci web)](#limited-browser-support)   |    Tak                                       |
|    Zabezpieczenia            |    Transport (HTTPS)                                    |    Transport (HTTPS)                         |
|    Generacji kodu klienta     |    [Tak](#code-generation)                              |    Plik OpenAPI i narzędzia innych firm             |

## <a name="grpc-strengths"></a>gRPC siły

### <a name="performance"></a>Wydajność

gRPC komunikaty są serializowane, za pomocą [Protobuf](https://developers.google.com/protocol-buffers/docs/overview), ma format komunikatu binarnego wydajne. Formatu Protobuf bardzo szybko serializuje się na serwerze i kliencie. Formatu Protobuf powoduje serializacji ładunków małych komunikatów, ważne w scenariuszach ograniczonej przepustowości, takich jak aplikacje mobilne.

gRPC jest przeznaczona dla protokołu HTTP/2, główne korektą protokołu HTTP, który zapewnia korzyści istotnie poprawiającą wydajność przy użyciu protokołu HTTP 1.x:

* Binarny ramek i kompresji. Protokół HTTP/2 jest zwarty i efektywne, zarówno w wysyłania i odbierania.
* Funkcje multipleksowania wielu wywołań HTTP/2 za pośrednictwem jednego połączenia TCP. Eliminuje funkcje multipleksowania [blokuje head wiersza](https://en.wikipedia.org/wiki/Head-of-line_blocking).

### <a name="code-generation"></a>Generowanie kodu

Wszystkie struktury gRPC zapewnia najwyższej jakości pomoc techniczną dla generowania kodu. Plik core gRPC programowania jest [ `*.proto` pliku](https://developers.google.com/protocol-buffers/docs/proto3), definiujący skontaktuj się z usług gRPC i komunikatów. Z tego pliku gRPC struktury kodu wygeneruje klasę bazową usługę, wiadomości i pełne klienta.

Udostępniając `*.proto` pliku między serwera i klienta, wiadomości i kod klienta mogą być generowane z elementu end-to-end. Generowanie kodu klienta eliminuje duplikatów wiadomości na kliencie i serwerze i tworzy silnie typizowaną klienta dla Ciebie. Brak konieczności pisania klienta pozwala zaoszczędzić czas znaczące rozwoju w aplikacjach z wieloma usługami.

### <a name="strict-specification"></a>Specyfikacja Strict

gRPC pozwala zaoszczędzić czas dla deweloperów za pośrednictwem jego prostotę. [Specyfikacji gRPC](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md) jest przetestowanego rozwiązania ze szczegółami o metodzie usług gRPC wygląda następująco. Istnieje nie formalną umowę z jakich interfejsu API protokołu HTTP przy użyciu formatu JSON powinien wyglądać następująco. Brak umowy tworzy dyskusji nad formatem adresów URL, zleceń HTTP, a kody odpowiedzi. gRPC eliminuje dyskusji z informacją o tym, jak metoda gRPC musi wyglądać specyfikacją.

### <a name="streaming"></a>Przesyłanie strumieniowe

Protokołu HTTP/2 stanowi podstawę dla strumieni długotrwałe, w czasie rzeczywistym komunikacji. gRPC zapewnia najwyższej jakości pomoc techniczna za pośrednictwem protokołu HTTP/2 do przesyłania strumieniowego.

Usługa gRPC obsługuje wszystkie kombinacje przesyłania strumieniowego:

* Jednoargumentowe (nie streaming)
* Od serwera do klienta, przesyłania strumieniowego
* Klient serwera przesyłania strumieniowego
* Dwukierunkowe przesyłanie strumieniowe

### <a name="deadlinetimeouts-and-cancellation"></a>Ostateczny termin/limity czasu i anulowania

gRPC umożliwia klientom określić, jak długo są gotowi oczekiwania dla wykonania zdalnego wywołania procedury. [Termin](https://grpc.io/blog/deadlines) są wysyłane do serwera i serwera można określić, jaką akcję należy podjąć w przypadku przekroczenia terminu ostatecznego. Na przykład serwer może spowodować anulowanie w toku gRPC/HTTP/żądań bazy danych przekroczenia limitu czasu.

Propagowanie terminu ostatecznego i anulowania przy użyciu podrzędnych gRPC wywołania ułatwia wymuszanie limitów użycia zasobów.

## <a name="grpc-recommended-scenarios"></a>gRPC zalecane scenariusze

gRPC dobrze nadaje się do następujących scenariuszy:

* **Mikrousługi** -gRPC jest zaprojektowana małe opóźnienia i wysoką przepływność komunikacji. gRPC to idealne narzędzie do uproszczonego mikrousług, gdzie wydajność ma kluczowe znaczenie.
* **Punkt-punkt w czasie rzeczywistym komunikacji** -gRPC zapewnia doskonałą obsługę dwukierunkowe przesyłanie strumieniowe. gRPC usług można wypchnąć wiadomości w czasie rzeczywistym bez sondowania.
* **Środowisk Polygot** -gRPC wspomagają wszystkich języków programowania popularnych wprowadzania gRPC dobrym rozwiązaniem w środowiskach wielojęzycznych.
* **Ograniczone środowiskach sieci** -gRPC komunikaty są serializowane przy użyciu formatu Protobuf, format wiadomości uproszczone. GRPC wiadomości, zawsze będzie mniejszy niż równoważna komunikat JSON.

## <a name="grpc-weaknesses"></a>gRPC słabe strony

### <a name="limited-browser-support"></a>Obsługa przeglądarek ograniczone

Nie można bezpośrednio wywołać usługę sieci gRPC z poziomu przeglądarki już dziś. gRPC intensywnie korzysta z protokołu HTTP/2 funkcje i przeglądarka nie zapewnia poziom kontroli wymagane przez żądania sieci web do obsługi klienta gRPC. Na przykład przeglądarek nie zezwalaj na obiekt wywołujący wymagać protokołu HTTP/2 lub zapewnić dostęp do podstawowych ramki protokołu HTTP/2.

[gRPC Web](https://grpc.io/docs/tutorials/basic/web.html) to technologia dodatkowe od zespołu gRPC, który zapewnia obsługę gRPC ograniczone w przeglądarce. gRPC Web składa się z dwóch części: klienta JavaScript, który obsługuje wszystkie nowoczesne przeglądarki i gRPC internetowym serwerem proxy na serwerze. Klient sieci Web gRPC wywołuje serwer proxy i serwer proxy będzie przekazywać w odpowiedzi na żądania gRPC serwerowi gRPC.

Nie wszystkie funkcje gRPC firmy są obsługiwane przez gRPC w sieci Web. Klienta i przesyłania strumieniowego dwukierunkowe nie jest obsługiwana i ma ograniczoną obsługę przesyłania strumieniowego serwera.

### <a name="not-human-readable"></a>Nie ludzi do odczytu

Żądania interfejsu API protokołu HTTP są wysyłane w postaci tekstu i można przeczytać, a utworzone przez ludzi.

gRPC komunikaty są zakodowane przy użyciu formatu Protobuf domyślnie. W trakcie efektywne wysyłanie i odbieranie Protobuf jego format binarny nie jest ludzi do odczytu. Formatu Protobuf wymaga komunikatu opis interfejsu określonego w `*.proto` pliku poprawnie zdeserializować. Dodatkowe narzędzia musi służyć do analizowania ładunków Protobuf przesyłania i tworzą żądania ręcznie.
Funkcje, takie jak [odbicia serwera](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md) i [narzędzia wiersza polecenia gRPC](https://github.com/grpc/grpc/blob/master/doc/command_line_tool.md) istnieją, aby obejść to ograniczenie.
Ponadto Protobuf komunikaty pomocy technicznej [konwersji do i z formatu JSON](https://developers.google.com/protocol-buffers/docs/proto3#json). Konwersja wbudowana JSON zapewnia sposób konwertować protobuf wiadomości w postaci czytelnej dla człowieka podczas debugowania.

## <a name="alternative-framework-scenarios"></a>Scenariusze alternatywnych Framework

Inne struktury zaleca się za pośrednictwem gRPC w następujących scenariuszach:

* **Dostępne interfejsy API przeglądarki** -gRPC nie jest w pełni obsługiwana w przeglądarce. gRPC w sieci Web może oferować obsługę przeglądarki, ale ma ograniczenia i przedstawiono serwer proxy usługi.
* **Emisja komunikację w czasie rzeczywistym** — gRPC obsługuje komunikację w czasie rzeczywistym za pośrednictwem przesyłania strumieniowego, ale nie ma koncepcji emituje komunikat się do zarejestrowanych połączeń. Na przykład w scenariuszu pokoju rozmów, gdzie nowych wiadomości rozmów mają być wysyłane do wszystkich klientów w pokoju rozmów, każde wywołanie gRPC musiałaby indywidualnie strumienia nowych wiadomości rozmów do klienta. [SignalR](xref:signalr/introduction) to dobre środowisko dla tego scenariusza. Ma ona pojęcia połączeń trwałych i wbudowaną obsługę komunikatów emisji.
* **Komunikacja między przetwarzania komunikacji** — proces należałoby hostować serwer gRPC protokołu HTTP/2 do akceptowania połączeń przychodzących. Windows transfer przetwarzania komunikacji [nazwanych potoków z programem WCF](/dotnet/framework/wcf/feature-details/choosing-a-transport#when-to-use-the-named-pipe-transport) jest szybkie i metodę komunikacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
