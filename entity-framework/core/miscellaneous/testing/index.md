---
title: EF Core를 사용하여 구성 요소 테스트 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 1ca900528ed42ad4b41016f22964c3494b0286eb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412798"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="4d5bd-102">EF Core를 사용하여 구성 요소 테스트</span><span class="sxs-lookup"><span data-stu-id="4d5bd-102">Testing components using EF Core</span></span>

<span data-ttu-id="4d5bd-103">실제 데이터베이스 I/O 작업에 부담을 주지 않으면서 실제 데이터베이스에 근접 연결하는 무엇인가를 통해 구성 요소를 테스트하고자 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4d5bd-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="4d5bd-104">이렇게 하는 데는 두 가지 기본적인 옵션이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4d5bd-104">There are two main options for doing this:</span></span>

* <span data-ttu-id="4d5bd-105">[SQLite 메모리 내 모드](sqlite.md)에서는 관계형 데이터베이스처럼 작동하는 공급자에 대해 효율적인 테스트를 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4d5bd-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
* <span data-ttu-id="4d5bd-106">[InMemory 공급자](in-memory.md)는 최소한의 종속성을 갖으면서도 항상 관계형 데이터베이스처럼 작동하지는 않는 가벼운 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="4d5bd-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
