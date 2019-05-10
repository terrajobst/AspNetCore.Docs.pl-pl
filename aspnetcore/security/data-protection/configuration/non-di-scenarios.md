---
title: Non-DI aware scenariusze ochrony danych w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak obsługiwać scenariusze ochrony danych, których nie może lub nie chcesz używać usługę oferowaną przez wstrzykiwanie zależności.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 34354c8443f6ae00bcce6ad9bdb6c11aaaa25bf8
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/27/2019
ms.locfileid: "64902995"
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>Non-DI aware scenariusze ochrony danych w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

System ochrony danych programu ASP.NET Core jest zwykle [dodany do kontenera usługi](xref:security/data-protection/consumer-apis/overview) , które są używane przez składniki zależne, za pośrednictwem wstrzykiwanie zależności (DI). Jednakże istnieją przypadki, w której nie jest to wykonalne ani pożądana, szczególnie w przypadku importowania systemu do istniejącej aplikacji.

Do obsługi tych scenariuszy [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) pakiet zawiera konkretny typ [DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider), która zapewnia prosty sposób korzystania z ochrony danych bez polegania na DI. `DataProtectionProvider` Typ implementuje [IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider). Konstruowanie `DataProtectionProvider` tylko wymaga podania [DirectoryInfo](/dotnet/api/system.io.directoryinfo) wystąpienia, aby wskazać, gdzie klucze kryptograficzne dostawcy powinny być przechowywane, jak pokazano w następującym przykładzie kodu:

[!code-none[](non-di-scenarios/_static/nodisample1.cs)]

Domyślnie `DataProtectionProvider` konkretny typ nie szyfruje pierwotne materiału klucza przed wprowadzeniem trwałych do systemu plików. To do obsługi scenariuszy, w którym punktów dla deweloperów do udziału sieciowego i system ochrony danych automatycznie nie można wywnioskować mechanizm szyfrowania nieużywanych odpowiednie.

Ponadto `DataProtectionProvider` nie konkretny typ [izolowania aplikacji](xref:security/data-protection/configuration/overview#per-application-isolation) domyślnie. Wszystkie aplikacje, korzystając z tego samego katalogu kluczy mogą udostępniać ładunków tak długo, jak ich [przeznaczenia parametry](xref:security/data-protection/consumer-apis/purpose-strings) zgodny.

[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) Konstruktor akceptuje wywołanie zwrotne opcjonalna konfiguracja, który może służyć do dostosowania zachowania systemu. Poniższy przykład pokazuje przywracania izolacji z jawnym wywołaniem [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname). W przykładzie pokazano również, konfigurowania systemu, które mają być automatycznie szyfrowane utrwalonych kluczy przy użyciu interfejsu DPAPI Windows. Jeśli wskazuje katalog w udziale UNC, warto zapoznać się z dystrybucji certyfikatu udostępnione dla wszystkich odpowiednich maszyn i skonfigurować system szyfrowania opartego na certyfikatach z wywołaniem [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate).

[!code-none[](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> Wystąpienia elementu `DataProtectionProvider` konkretny typ których tworzenie jest kosztowne. Jeśli aplikacja obsługuje wiele wystąpień tego typu, a wszystkie przy użyciu tego samego katalogu magazynu kluczy, może obniżyć wydajność aplikacji. Jeśli używasz `DataProtectionProvider` typu, zaleca się raz Utwórz ten typ, a następnie użyć go możliwie ponownie. `DataProtectionProvider` Typu i wszystkie [interfejsu IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) wystąpień utworzonych na jej podstawie są odporne na wątki dla wielu obiektów wywołujących.
