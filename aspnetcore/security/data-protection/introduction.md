---
title: Ochrona danych platformy ASP.NET Core
author: rick-anderson
description: Informacje na temat koncepcji ochrony danych i zasad dotyczących projektowania interfejsów API do ochrony danych usługi ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/introduction
ms.openlocfilehash: 29a2bbef6f2fd9b61541173af143926ca82bfad7
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095698"
---
# <a name="aspnet-core-data-protection"></a>Ochrona danych platformy ASP.NET Core

Aplikacje sieci Web często muszą przechowywać dane dotyczące zabezpieczeń. Windows udostępnia DPAPI dla aplikacji klasycznych, ale to nie nadaje się do aplikacji sieci web. ASP.NET Core ochrony przy użyciu stosu danych zapewniają prosty i łatwy w użyciu cryptographic API Deweloper służy do ochrony danych, w tym zarządzania kluczami i wymiany.

Stos ochrony danych programu ASP.NET Core zaprojektowano w celu służyć jako zamiennika długoterminowe &lt;machineKey&gt; elementu w programie ASP.NET: 1.x - 4.x. Została zaprojektowana w celu wiele wad stare kryptograficznych stosu przy jednoczesnym zapewnieniu poza wbudowane rozwiązanie dla większości przypadków użycia nowoczesnych aplikacji prawdopodobnie mogą wystąpić.

## <a name="problem-statement"></a>Opis problemu

Instrukcja zasadniczy problem może być krótkiej formie wyrażona w jednym zdaniu: musisz utrwalić zaufane informacje dotyczące pobierania nowsze, ale nie można zaufać mechanizmu stanu trwałego. W warunkach sieci web to mogą być zapisane jako "Potrzebuję obustronne zaufanego stanu za pomocą niezaufanego klienta".

Przykład canonical jest plik cookie uwierzytelniania lub elementu nośnego tokenu. Serwer generuje "Jestem Groot i mają uprawnienia xyz" token i przekazuje go do klienta. W przyszłości klienta spowoduje wyświetlenie tego tokenu do serwera, ale serwer wymaga pewnego rodzaju zapewnienie, że klient nie zostało sfałszowane tokenu. Dlatego pierwszego zapotrzebowania: autentyczności (zwane) integralność, sprawdzające odporne).

Stan utrwalony jest zaufany przez serwer, przewidujemy czy ten stan może zawierać informacje, które są specyficzne dla środowiska pracy. Może to być w postaci ścieżki do pliku, uprawnienia, dojście lub innych pośrednie odwołanie, lub inne dane specyficzne dla serwera. Zazwyczaj takie informacje nie być ujawniona do niezaufanego klienta. Ten sposób drugie wymaganie: poufności.

Na koniec ponieważ nowoczesnych aplikacji są składającej, co widzieliśmy jest poszczególne składniki będą chcieli korzystać z zalet tego systemu, niezależnie od innych składników w systemie. Na przykład jeśli składnik tokenu elementu nośnego używa tego stosu, powinna ona działać bez zakłóceń z mechanizmu CSRF chroniących przed złośliwym, które również mogą używać tego samego stosu. Zatem końcowego wymaganie: izolacji.

Oferujemy dalszego ograniczenia w celu zawężenia zakresu naszych wymagań w zakresie. Przyjęto założenie, że wszystkie usługi działające w ramach cryptosystem są równie zaufane i że danych nie trzeba będzie generowany lub używane poza usługami na mocy naszych bezpośrednią kontrolę. Ponadto firma Microsoft wymaga, że operacje są tak szybko, jak to możliwe, ponieważ każde żądanie usługi sieci web mogą zostać przekazane za pośrednictwem cryptosystem jeden lub więcej razy. To sprawia, że kryptografia symetryczna idealne rozwiązanie w naszym scenariuszu, a firma Microsoft rabatów kryptografii asymetrycznego, dopóki taki czas, gdy jest potrzebna.

## <a name="design-philosophy"></a>Zasady projektowania klas

Rozpoczęliśmy od zidentyfikowania problemów z istniejącego stosu. Po mieliśmy, firma Microsoft zbadany krajobrazu istniejących rozwiązań i zakończony, że żadne istniejące rozwiązanie dość miał możliwości, których poszukiwane firma Microsoft. Firma Microsoft następnie zaprojektowany rozwiązanie oparte na kilka wytyczne.

* System powinno oferować się łatwość konfiguracji. W idealnym system będzie niewymagającą konfiguracji, a deweloperzy mogą sprawnie Rozpocznij pracę. W sytuacjach, w którym deweloperzy konieczne skonfigurowanie określonej proporcji (np. klucza repozytorium) należy zwrócić szczególną uwagę znaczenie tych określonych konfiguracji jest proste.

* Zapewniają prosty interfejs API udostępnianych klientom. Interfejsy API należy wykorzystać poprawnie i trudne w użyciu niepoprawnie.

* Deweloperzy nie powinien Dowiedz się, zasady zarządzania kluczami. System powinien obsługiwać algorytm wybór i okresu istnienia klucza w imieniu dewelopera. W idealnym Deweloper nigdy nawet powinni mieć dostęp do surowego materiału klucza.

* Klucze powinny być chronione w spoczynku, gdy jest to możliwe. System powinien ustalić odpowiedni mechanizm ochrony i automatycznie stosować.

Za pomocą tych zasad, pamiętając opracowaliśmy prosty, [łatwy w użyciu](xref:security/data-protection/using-data-protection) stosu ochrony danych.

Interfejsy API ochrony danych programu ASP.NET Core nie są przeznaczone głównie dla nieokreślony stan trwały ładunki poufne. Inne technologie, takie jak [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) i [usługi Azure Rights Management](https://docs.microsoft.com/rights-management/) są bardziej odpowiednie do scenariusza nieograniczony magazyn i mają możliwości odpowiednio silne zarządzania kluczami. Inaczej mówiąc, nic nie uniemożliwiają dewelopera przy użyciu platformy ASP.NET Core interfejsy API ochrony danych dla długoterminowej ochrony poufnych danych.

## <a name="audience"></a>Odbiorcy

System ochrony danych jest podzielona na pięć głównych pakietów. Różne aspekty tych interfejsów API odbiorców trzy główne;

1. [Omówienie interfejsów API przeznaczonych dla klientów](xref:security/data-protection/consumer-apis/overview) docelowa, deweloperom aplikacji i struktury.

   "Nie chcę dowiedzieć się więcej o sposób działania stosu, ani o jego konfiguracji. Czy mogę po prostu chcesz wykonać kilka operacji w prosty sposób możliwie z dużym prawdopodobieństwem pomyślnie korzystania z interfejsów API."

2. [Interfejsy API konfiguracji](xref:security/data-protection/configuration/overview) docelowa, deweloperom aplikacji i administratorów systemu.

   "Musisz poinformować system ochrony danych, czy moje środowisko wymaga ustawienia lub innych niż domyślne ścieżki."

3. Deweloperzy docelowej interfejsów API rozszerzalności odpowiedzialnych za wdrażanie zasad niestandardowych. Użycie tych interfejsów API będzie ograniczone do rzadkich sytuacjach i doświadczenie deweloperów pamiętać zabezpieczeń.

   "Należy zastąpić składnika całego systemu, ponieważ naprawdę unikatowe wymagania funkcjonalne. Chcę dowiedzieć się uncommonly używane części powierzchni interfejsu API, aby utworzyć dodatek, który spełnia moich wymagań."

## <a name="package-layout"></a>Układ pakietu

Stos ochrony danych składa się z pięciu pakietów.

* Microsoft.AspNetCore.DataProtection.Abstractions zawiera podstawowe interfejsy IDataProtectionProvider i interfejsu IDataProtector. Zawiera ona także metody przydatne rozszerzenia, które mogą pomóc w pracy z tymi typami (np. przeciążenia IDataProtector.Protect). Zobacz sekcję interfejsów konsumenta, aby uzyskać więcej informacji. Jeśli ktoś inny jest odpowiedzialny za utworzenie wystąpienia system ochrony danych i możesz po prostu korzystanie z interfejsów API, będziesz chciał odwołanie Microsoft.AspNetCore.DataProtection.Abstractions.

* Microsoft.AspNetCore.DataProtection zawiera implementację podstawowego systemu ochrony danych, w tym podstawowe operacje kryptograficzne, zarządzanie kluczami, konfiguracji i rozszerzalności. Jeśli jesteś odpowiedzialny za utworzenie wystąpienia system ochrony danych (np. dodanie go do IServiceCollection) lub modyfikowania i rozszerzania jej zachowanie, będziesz chciał odwołanie Microsoft.AspNetCore.DataProtection.

* Microsoft.AspNetCore.DataProtection.Extensions zawiera dodatkowe interfejsy API, które deweloperzy mogą okazać się przydatne, ale które nie powinny znajdować się w pakiecie core. Na przykład ten pakiet zawiera prosty "wystąpienia systemu, wskazując katalogu określonego magazynu kluczy z Instalatorem iniekcji nie zależności" interfejs API (więcej informacji). Zawiera ona także metody rozszerzenia dla ograniczanie okresu istnienia ładunków chronionych (więcej informacji).

* Microsoft.AspNetCore.DataProtection.SystemWeb można zainstalować na istniejącej aplikacji ASP.NET 4.x przekierować jego &lt;machineKey&gt; operacje, aby zamiast tego używać nowego stosu ochrony danych. Zobacz [zgodności](xref:security/data-protection/compatibility/replacing-machinekey#compatibility-replacing-machinekey) Aby uzyskać więcej informacji.

* Microsoft.AspNetCore.Cryptography.KeyDerivation dostarcza implementację PBKDF2 skrótu procedury i mogą być używane przez systemy, które musisz bezpiecznie obsługiwać haseł użytkowników. Zobacz [wyznaczania wartości skrótu hasła](xref:security/data-protection/consumer-apis/password-hashing) Aby uzyskać więcej informacji.

## <a name="additional-resources"></a>Dodatkowe zasoby

<xref:host-and-deploy/web-farm>
