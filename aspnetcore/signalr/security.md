---
title: Zagadnienia dotyczące zabezpieczeń w biblioteki SignalR platformy ASP.NET Core
author: tdykstra
description: Dowiedz się, jak używać uwierzytelniania i autoryzacji w biblioteki SignalR platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 10/17/2018
uid: signalr/security
ms.openlocfilehash: 1adf762cd6de4f0cf62e31c0ec6e595a32ed56f8
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477543"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a>Zagadnienia dotyczące zabezpieczeń w biblioteki SignalR platformy ASP.NET Core

Przez [Andrew Stanton pielęgniarki](https://twitter.com/anurse)

Ten artykuł zawiera informacje na temat zabezpieczenia SignalR.

## <a name="cross-origin-resource-sharing"></a>Współużytkowanie zasobów między źródłami

[Współużytkowanie zasobów między źródłami (cors)](https://www.w3.org/TR/cors/) może służyć do Zezwalaj na połączenia SignalR cross-origin w przeglądarce. Jeśli kod JavaScript jest hostowana w innej domenie niż aplikacji SignalR [oprogramowanie pośredniczące CORS](xref:security/cors) musi być włączona, Włącz język JavaScript nawiązać połączenie aplikacji SignalR. Zezwalaj na żądań cross-origin tylko z domen, którym ufasz lub formant. Na przykład:

* Twoja witryna jest hostowana na `http://www.example.com`
* Aplikacja SignalR jest hostowana na `http://signalr.example.com`

Należy skonfigurować mechanizmu CORS w aplikacji SignalR w celu zezwolenia tylko na początku `www.example.com`.

Aby uzyskać więcej informacji na temat konfigurowania mechanizmu CORS, zobacz [Włącz Cross-Origin Requests (CORS)](xref:security/cors). SignalR **wymaga** następujące zasady CORS:

* Zezwalaj na określone pochodzenie oczekiwane. Zezwalanie na wszystkie pochodzenia jest możliwe, ale jest **nie** bezpieczne lub zalecane.
* Metody HTTP `GET` i `POST` muszą być dozwolone.
* Poświadczenia musi być włączona, nawet wtedy, gdy nie jest używane uwierzytelnianie.

Na przykład następujące zasady CORS umożliwia klient przeglądarki SignalR w serwisie `http://example.com` dostęp do aplikacji SignalR w serwisie `http://signalr.example.com`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> SignalR nie jest zgodny z wbudowanej funkcji CORS w usłudze Azure App Service.

## <a name="websocket-origin-restriction"></a>Ograniczenie WebSocket źródła

Zabezpieczenia udostępniane przez mechanizm CORS nie dotyczą funkcji WebSockets. Czy przeglądarek **nie**:

* Wykonywanie żądań krótkiej mechanizmu CORS.
* Przestrzeganie ograniczenia określone w `Access-Control` nagłówków w przypadku wysyłania żądań protokołu WebSocket.

Jednak wysłać przeglądarek `Origin` nagłówka podczas wystawiania żądania protokołu WebSocket. Aplikacje powinny być konfigurowane do sprawdzania poprawności tych nagłówków, aby upewnić się, że dozwolone są tylko WebSockets pochodzące z oczekiwanym źródła.

W programie ASP.NET Core 2.1 i nowsze, nagłówek sprawdzania poprawności można osiągnąć przy użyciu niestandardowego oprogramowania pośredniczącego, umieszczone **przed `UseSignalR`i oprogramowanie pośredniczące uwierzytelniania** w `Configure`:

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> `Origin` Nagłówek jest kontrolowany przez klienta i, podobnie jak `Referer` nagłówka, mogą zostać sfałszowane. Nagłówki te powinny **nie** służyć jako mechanizm uwierzytelniania.

## <a name="access-token-logging"></a>Rejestrowanie tokenu dostępu

Korzystając z funkcji WebSockets lub Server-Sent zdarzeń, klient przeglądarki wysyła ten token dostępu w ciągu zapytania. Odbiera token dostępu za pomocą ciągu zapytania jest zazwyczaj tak bezpieczne, jak przy użyciu standardu `Authorization` nagłówka. Jednak wiele serwerów sieci web Zaloguj się adres URL dla każdego żądania, w tym ciągu zapytania. Rejestrowanie adresy URL mogą rejestrować tokenu dostępu. Najlepszym rozwiązaniem jest można ustawić ustawienia rejestrowania serwera, aby uniemożliwić rejestrowanie tokenów dostępu w sieci web.

## <a name="exceptions"></a>Wyjątki

Komunikaty o wyjątkach są zazwyczaj uważane za dane poufne, które nie może uzyskać dostęp do klienta. Domyślnie SignalR nie wysyła szczegółowe informacje o wyjątku przez metodę koncentratora do klienta. Zamiast tego klient odbiera ogólny komunikat wskazujący, że wystąpił błąd. Dostarczanie komunikatów wyjątku do klienta może zostać zastąpiona przez (na przykład w deweloperskie lub testowe) [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options). Nie należy uwidaczniać komunikaty o wyjątkach do klienta w aplikacjach produkcyjnych.

## <a name="buffer-management"></a>Zarządzanie buforem

SignalR używa buforów dla każdego połączenia w celu zarządzania wiadomości przychodzących i wychodzących. Domyślnie SignalR ogranicza bufory 32 KB. Największy komunikat, który można wysłać klienta lub serwera wynosi 32 KB. Maksymalny rozmiar pamięci używane przez połączenie komunikaty wynosi 32 KB. Jeśli wiadomości, zawsze są mniejsze niż 32 KB, można zmniejszyć limit, który:

* Uniemożliwia klientowi mogła wysyłać wiadomości większych.
* Serwer nigdy nie należy przydzielić dużych buforów, aby akceptować wiadomości.

Jeśli wiadomości są większe niż 32 KB, możesz zwiększyć limit. Oznacza zwiększenie tego limitu:

* Klient może spowodować serwera można przydzielić bufory dużej ilości pamięci.
* Przydział serwera dużych buforów może zmniejszyć liczbę jednoczesnych połączeń.

Istnieją limity dla komunikatów przychodzących i wychodzących, skonfigurowania zarówno na [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) skonfigurowany w obiekcie `MapHub`:

* `ApplicationMaxBufferSize` przedstawia maksymalną liczbę bajtów z zakresu od klienta, bufory serwera. Jeśli klient próbuje wysłać wiadomość przekracza ten limit, połączenie może być zamknięte.
* `TransportMaxBufferSize` reprezentuje maksymalną liczbę bajtów, jaką serwer może wysyłać. Jeśli serwer próbuje wysłać wiadomość (tym wartości zwracane z metod koncentratora) przekracza ten limit, zostanie zgłoszony wyjątek.

Ustawienie wartości limitu `0` wyłącza limit. Usunięcie limitu umożliwia klientowi wysłać komunikat o dowolnym rozmiarze. Złośliwi klienci wysyłania dużych komunikatów może spowodować nadmierną ilość pamięci do przydzielenia. Użycie pamięci nadmiarowe mogą znacznie zmniejszyć liczbę jednoczesnych połączeń.
