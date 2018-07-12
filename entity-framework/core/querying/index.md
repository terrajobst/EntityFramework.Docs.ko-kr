---
title: 데이터 쿼리 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: 447f48b780bc48b7a79153d17dcc1b8ef0cc508c
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949277"
---
# <a name="querying-data"></a><span data-ttu-id="7d4be-102">데이터 쿼리</span><span class="sxs-lookup"><span data-stu-id="7d4be-102">Querying Data</span></span>

<span data-ttu-id="7d4be-103">Entity Framework Core는 LINQ(Language-Integrated Query)를 사용하여 데이터베이스에서 데이터를 쿼리합니다.</span><span class="sxs-lookup"><span data-stu-id="7d4be-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="7d4be-104">LINQ를 사용하면 C#(원하는 .NET 언어)을 사용하여 파생된 컨텍스트 및 엔터티 클래스를 기반으로 강력한 형식의 쿼리를 작성할 수 있습니다. </span><span class="sxs-lookup"><span data-stu-id="7d4be-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="7d4be-105">LINQ 쿼리의 표현이 데이터베이스 공급자로 전달되어 데이터베이스 특정 쿼리 언어로 변환됩니다(예: 관계형 데이터베이스의 경우 SQL).</span><span class="sxs-lookup"><span data-stu-id="7d4be-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (for example, SQL for a relational database).</span></span> <span data-ttu-id="7d4be-106">쿼리 처리 방식에 대한 자세한 내용은 [쿼리 작동 방식](overview.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="7d4be-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
