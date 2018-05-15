---
title: Wprowadzenie do platformy ASP.NET Core SignalR
author: rachelappel
description: Dowiedz się, jak biblioteka ASP.NET Core SignalR ułatwia dodawanie w czasie rzeczywistym funkcjonalności do aplikacji.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/25/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction
ms.openlocfilehash: f05b7cbf05372dc5d5cdadaf5a534d7a9d9bfecc
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/09/2018
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="60a85-103">Wprowadzenie do platformy ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="60a85-103">Introduction to ASP.NET Core SignalR</span></span>

<span data-ttu-id="60a85-104">Przez [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="60a85-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="60a85-105">Co to jest SignalR?</span><span class="sxs-lookup"><span data-stu-id="60a85-105">What is SignalR?</span></span>

<span data-ttu-id="60a85-106">SignalR platformy ASP.NET Core to biblioteki, która ułatwia dodawanie funkcji sieci web w czasie rzeczywistym do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="60a85-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="60a85-107">Funkcje sieci web w czasie rzeczywistym umożliwia natychmiastowe kod po stronie serwera do wypychania zawartości do klientów.</span><span class="sxs-lookup"><span data-stu-id="60a85-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="60a85-108">Nadaje SignalR:</span><span class="sxs-lookup"><span data-stu-id="60a85-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="60a85-109">Aplikacje, które wymagają wysokiej częstotliwości aktualizacji z serwera.</span><span class="sxs-lookup"><span data-stu-id="60a85-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="60a85-110">Przykłady są gier, sieci społecznościowych głosowania, aukcji, map i GPS aplikacji.</span><span class="sxs-lookup"><span data-stu-id="60a85-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="60a85-111">Pulpity nawigacyjne i monitorowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="60a85-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="60a85-112">Przykładem mogą być firmy pulpity nawigacyjne, błyskawiczne aktualizacji sprzedaży, lub przesyłane alerty.</span><span class="sxs-lookup"><span data-stu-id="60a85-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="60a85-113">Aplikacje współpracy.</span><span class="sxs-lookup"><span data-stu-id="60a85-113">Collaborative apps.</span></span> <span data-ttu-id="60a85-114">Tablica aplikacji i zespołu spotkania oprogramowania to przykłady współpracy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="60a85-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="60a85-115">Aplikacje, które wymagają powiadomienia.</span><span class="sxs-lookup"><span data-stu-id="60a85-115">Apps that require notifications.</span></span> <span data-ttu-id="60a85-116">Sieci społecznościowych, poczty e-mail, rozmowy, gier, alerty podróży i wiele innych aplikacji użyj powiadomień.</span><span class="sxs-lookup"><span data-stu-id="60a85-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="60a85-117">Biblioteka SignalR udostępnia interfejs API do tworzenia klienta serwera [zdalnych wywołań procedur (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="60a85-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="60a85-118">RPC wywołują funkcje JavaScript na komputerach klienckich z kodu .NET Core po stronie serwera.</span><span class="sxs-lookup"><span data-stu-id="60a85-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="60a85-119">Biblioteka SignalR dla platformy ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="60a85-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="60a85-120">Obsługuje zarządzanie połączeniami automatycznie.</span><span class="sxs-lookup"><span data-stu-id="60a85-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="60a85-121">Umożliwia emituje wiadomości jednocześnie do wszystkich połączonych klientów.</span><span class="sxs-lookup"><span data-stu-id="60a85-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="60a85-122">Na przykład pokoju rozmów.</span><span class="sxs-lookup"><span data-stu-id="60a85-122">For example, a chat room.</span></span>
* <span data-ttu-id="60a85-123">Umożliwia wysyłanie komunikatów do określonych klientów lub grupy klientów.</span><span class="sxs-lookup"><span data-stu-id="60a85-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="60a85-124">Jest open-powierzając jej ich konserwację na [GitHub](https://github.com/aspnet/signalr).</span><span class="sxs-lookup"><span data-stu-id="60a85-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="60a85-125">Skalowalne.</span><span class="sxs-lookup"><span data-stu-id="60a85-125">Scalable.</span></span>

<span data-ttu-id="60a85-126">Połączenie między klientem a serwerem jest trwała, w przeciwieństwie do połączenia HTTP.</span><span class="sxs-lookup"><span data-stu-id="60a85-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="60a85-127">Transporty</span><span class="sxs-lookup"><span data-stu-id="60a85-127">Transports</span></span>

<span data-ttu-id="60a85-128">Abstract SignalR przez kilka technik tworzenia aplikacji sieci web czasu rzeczywistego.</span><span class="sxs-lookup"><span data-stu-id="60a85-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="60a85-129">[Protokół WebSockets](https://tools.ietf.org/html/rfc7118) jest optymalna transportu, ale innych technik, takich jak zdarzenia Server-Sent i długi sondowania można użyć w przypadku te nie są dostępne.</span><span class="sxs-lookup"><span data-stu-id="60a85-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="60a85-130">SignalR automatycznie wykryje i zainicjować odpowiednie transportu oparte na funkcji serwera i klienta są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="60a85-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="60a85-131">Koncentratory</span><span class="sxs-lookup"><span data-stu-id="60a85-131">Hubs</span></span>

<span data-ttu-id="60a85-132">Biblioteka SignalR używa koncentratory do komunikacji między klientami a serwerami.</span><span class="sxs-lookup"><span data-stu-id="60a85-132">SignalR uses hubs to communicate between clients and servers.</span></span>

<span data-ttu-id="60a85-133">Koncentrator jest wysokiego poziomu potok, który umożliwia klienta i serwera, wywoływanie metod na siebie.</span><span class="sxs-lookup"><span data-stu-id="60a85-133">A hub is a high-level pipeline that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="60a85-134">SignalR obsługuje wysyłki poza granicami maszyny automatycznie, co pozwala klientom wywoływać metod na serwerze jako łatwo jako metody lokalne i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="60a85-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="60a85-135">Koncentratory zezwala na przekazywanie jednoznacznie parametrów do metod, co pozwala wiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="60a85-135">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="60a85-136">Biblioteka SignalR udostępnia dwa protokoły wbudowanych koncentratora: protokół tekst na podstawie JSON i protokół binarny na podstawie [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="60a85-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="60a85-137">MessagePack tworzy zazwyczaj wiadomości mniejszych niż przy użyciu formatu JSON.</span><span class="sxs-lookup"><span data-stu-id="60a85-137">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="60a85-138">Starsze przeglądarki musi obsługiwać [XHR poziom 2](https://caniuse.com/#feat=xhr2) do obsługi protokołu MessagePack.</span><span class="sxs-lookup"><span data-stu-id="60a85-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="60a85-139">Koncentratory wywoływać kod po stronie klienta przez wysyłanie wiadomości przy użyciu aktywny transport.</span><span class="sxs-lookup"><span data-stu-id="60a85-139">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="60a85-140">Komunikaty zawierają nazwę i parametry metody po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="60a85-140">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="60a85-141">Obiekty wysyłane jako parametry metody są deserializacji za pomocą protokołu skonfigurowany.</span><span class="sxs-lookup"><span data-stu-id="60a85-141">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="60a85-142">Klient próbuje jest zgodna z nazwą metody w kodzie po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="60a85-142">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="60a85-143">W przypadku dopasowania w metodzie klienta jest wykonywane przy użyciu danych parametru zdeserializowany.</span><span class="sxs-lookup"><span data-stu-id="60a85-143">When a match happens, the client method runs using the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="60a85-144">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="60a85-144">Additional resources</span></span>

* [<span data-ttu-id="60a85-145">Rozpoczynanie pracy z SignalR dla platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="60a85-145">Get started with SignalR for ASP.NET Core</span></span>](xref:signalr/get-started)
* [<span data-ttu-id="60a85-146">Obsługiwane platformy</span><span class="sxs-lookup"><span data-stu-id="60a85-146">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="60a85-147">Centra</span><span class="sxs-lookup"><span data-stu-id="60a85-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="60a85-148">Klient JavaScript</span><span class="sxs-lookup"><span data-stu-id="60a85-148">JavaScript client</span></span>](xref:signalr/javascript-client)