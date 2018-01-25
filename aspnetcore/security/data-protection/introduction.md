---
title: Wprowadzenie do ochrony danych
author: rick-anderson
description: "Ten dokument pojęcia związane z ochroną danych i opisano zasady projektowania skojarzone podstawowych interfejsów API platformy ASP.NET."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/introduction
ms.openlocfilehash: b02ef9121e50ab9d9f24032d32f1e65fe73049c0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-data-protection"></a>Wprowadzenie do ochrony danych

Aplikacje sieci Web często muszą przechowywać dane dotyczące zabezpieczeń. System Windows udostępnia DPAPI aplikacji klasycznych, ale to nie nadaje się do aplikacji sieci web. Stos ochrony danych platformy ASP.NET Core zapewniają prosty i łatwy w użyciu API kryptograficznych deweloper może użyć do ochrony danych, w tym zarządzania kluczami i obrotu.

Stos ochrony danych platformy ASP.NET Core zaprojektowano jako długoterminowej zastępuje <machineKey> elementu w programie ASP.NET 1.x - 4.x. Została zaprojektowana w celu rozwiązania wiele niedociągnięć starego stosu kryptograficznych zapewniając poza gotowe rozwiązanie dla większości przypadków użycia, które nowoczesne aplikacje są prawdopodobnie mogą wystąpić.

## <a name="problem-statement"></a>Opis problemu

Instrukcja zasadniczy problem może być krótkiej formie wyrażona w pojedynczym zdaniu: musisz utrwalić zaufanych informacje dotyczące pobierania nowszej, ale nie można zaufać mechanizmu stanu trwałego. W warunkach sieci web to mogą być zapisane jako "Musisz obustronne zaufanego stanu za pomocą niezaufanego klienta."

Canonical przykład to jest plik cookie uwierzytelniania lub elementu nośnego tokenu. Generuje serwer "Mam Groot i uprawnień xyz" token i przekazuje ją do klienta. W przyszłości klienta przedstawi token do serwera, ale serwer musi mieć określonego rodzaju gwarancji, że klient nie sfałszowane tokenu. W związku z tym pierwszego zapotrzebowania: autentyczności () integralność, sprawdzające odporne na próby).

Ponieważ stanu utrwalonego jest uważany za zaufany przez serwer, przewidujemy, że ten stan może zawierać informacje, które są specyficzne dla środowiska pracy. Może to być w postaci ścieżki do pliku, uprawnienia, dojścia lub innych pośrednie odwołanie, lub inne dane specyficzne dla serwera. Zazwyczaj takie informacje nie zostać ujawnione osobom niezaufanego klienta. W związku z tym drugie wymaganie: poufności.

Ponadto ponieważ nowoczesne aplikacje są składnikowa, co możemy w tym samouczku jest pojedynczych składników spowoduje chcesz skorzystać z tego systemu, niezależnie od innych składników w systemie. Na przykład jeśli składnik tokenu elementu nośnego używa tego stosu, powinien działać bez zakłóceń z mechanizm anti-CSRF, który może również korzystać z tym samym stosie. W związku z tym ostatnim wymaganie: izolacji.

Firma Microsoft może dostarczyć więcej ograniczenia Aby zawęzić zakres bieżących wymagań. Przyjęto założenie, że wszystkie usługi działające w ramach cryptosystem są równie zaufane i że dane nie musi zostać wygenerowany lub używane poza usług w naszym bezpośrednią kontrolę. Ponadto wymagane operacje są tak szybko, jak to możliwe, ponieważ każde żądanie usługi sieci web może przejść cryptosystem jeden lub więcej razy. Dzięki temu Kryptografia symetryczna idealny dla naszej scenariusza i firma Microsoft discount kryptografii asymetrycznej do taki czas, który jest potrzebna.

## <a name="design-philosophy"></a>Zasady projektowania klas

Firma Microsoft jest uruchomiony przez identyfikowania problemów z istniejącego stosu. Po było który, możemy badaniu pozioma istniejących rozwiązań i wykazał, że żadne istniejące rozwiązanie dość miał możliwości, które firma Microsoft poszukiwane. Firma Microsoft następnie odtwarzane rozwiązania opartego na kilka wytyczne.

* System powinno oferować łatwość konfiguracji. W idealnym przypadku system będzie niewymagającą konfiguracji i deweloperzy mogą trafień podstaw uruchamiania. W sytuacjach, gdy deweloperzy należy skonfigurować określonej proporcji (na przykład klucza repozytorium) należy zwrócić szczególną uwagę do tworzenia prostych tych określonej konfiguracji.

* Zapewniają prosty interfejs API dla użytkownika. Interfejsy API powinno być łatwe w użyciu poprawnie i trudny do wykorzystania niepoprawnie.

* Deweloperzy nie należy dowiedzieć się zasady zarządzania kluczami. System powinna obsługiwać wybór algorytmu i okresu istnienia klucza w imieniu dewelopera. Najlepiej dewelopera nigdy nie nawet ma dostęp do nieprzetworzonej materiału klucza.

* Klucze powinny być chronione w stanie spoczynku, gdy jest to możliwe. System powinien ustalić odpowiedni mechanizm ochrony i zastosować je automatycznie.

Z tych zasad, pamiętając opracowaliśmy prosty, [łatwy w użyciu](using-data-protection.md) stosu ochrony danych.

Interfejsy API ochrony danych platformy ASP.NET Core nie są głównie przeznaczone do nieograniczonego trwałość ładunki poufne. Innych technologii, takich jak [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) i [usługi Azure Rights Management](https://docs.microsoft.com/rights-management/) są bardziej odpowiednie do scenariusza nieograniczonego magazynu i mają możliwości odpowiednio silne zarządzania kluczami. Inaczej mówiąc, nic nie uniemożliwiają dewelopera przy użyciu interfejsy API ochrony danych platformy ASP.NET Core dla długoterminowej ochrony poufnych danych.

## <a name="audience"></a>Odbiorcy

System ochrony danych jest podzielona na pięć głównych pakietów. Różne aspekty te interfejsy API target trzy główne odbiorców;

1. [Omówienie interfejsów API klienta](consumer-apis/overview.md) deweloperzy aplikacji i platforma docelowa.

   "Nie chcę dowiedzieć się więcej o sposób działania stosu lub jego konfiguracji. Po prostu chcę do wykonania niektórych operacji w jako prosty sposób możliwie z dużym prawdopodobieństwem z użyciem interfejsów API pomyślnie."

2. [Interfejsy API konfiguracji](configuration/overview.md) docelowe deweloperzy aplikacji i administratorów systemu.

   "Potrzebuję systemu ochrony danych stwierdzić, że moje środowisko wymaga ustawienia lub innych niż domyślne ścieżki".

3. Deweloperzy docelowy interfejsów API rozszerzalności odpowiedzialnych za wdrażanie zasad niestandardowych. Użycie tych interfejsów API czy ograniczone do rzadkich sytuacji i napotkał pamiętać deweloperów zabezpieczeń.

   "Należy zastąpić cały składnika w systemie, ponieważ naprawdę unikatowe wymagania funkcjonalne. Chcę Dowiedz się uncommonly używane części powierzchni interfejsu API do zbudowania wtyczkę, która spełnia wymagania Mój."

## <a name="package-layout"></a>Układ pakietu

Stos ochrony danych składa się z pięciu pakietów.

* Microsoft.AspNetCore.DataProtection.Abstractions zawiera podstawowe interfejsy IDataProtectionProvider i interfejsu IDataProtector. Zawiera również metody przydatne rozszerzenia, które mogą ułatwić pracę z tych typów (np. przeciążenia IDataProtector.Protect). Zobacz sekcję interfejsy konsumenta, aby uzyskać więcej informacji. Jeśli ktoś inny włączył jest odpowiedzialny za tworzenie wystąpień systemu ochrony danych i po prostu zużywają interfejsy API, należy do odwołania Microsoft.AspNetCore.DataProtection.Abstractions.

* Microsoft.AspNetCore.DataProtection zawiera implementację core systemu ochrony danych, w tym podstawowe operacje kryptograficzne, zarządzania kluczami, konfiguracji i rozszerzeń. Kto jest odpowiedzialny za tworzenie wystąpień systemu ochrony danych (np. dodanie go do IServiceCollection) lub modyfikowania lub rozszerzanie ich zachowania, należy Microsoft.AspNetCore.DataProtection odwołania.

* Microsoft.AspNetCore.DataProtection.Extensions zawiera dodatkowe interfejsy API, które deweloperzy mogą być przydatne, ale które nie powinny znajdować się w pakiecie core. Na przykład ten pakiet zawiera proste "wystąpienia systemu, wskazując katalogu określonego klucza magazynu bez ustawień iniekcji zależności" API (więcej informacji). Zawiera również metody rozszerzenia dla ograniczanie okresu istnienia chronionych ładunków (więcej informacji).

* Microsoft.AspNetCore.DataProtection.SystemWeb można zainstalować w istniejącej aplikacji ASP.NET 4.x przekierować jego <machineKey> operacje, aby zamiast tego użyć nowego stosu ochrony danych. Zobacz [zgodności](compatibility/replacing-machinekey.md#compatibility-replacing-machinekey) Aby uzyskać więcej informacji.

* Microsoft.AspNetCore.Cryptography.KeyDerivation dostarcza implementację tego hasła PBKDF2 mieszania procedury i mogą być używane przez systemy, które musi obsłużyć bezpiecznego hasła użytkownika. Zobacz [tworzenia skrótu hasła](consumer-apis/password-hashing.md) Aby uzyskać więcej informacji.
