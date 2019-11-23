---
title: 테스트 용이성 및 Entity Framework 4.0-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 28ec5446ce9faf98fb8fff141832236d70b29daf
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181584"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="b3ac4-102">테스트 용이성 및 Entity Framework 4.0</span><span class="sxs-lookup"><span data-stu-id="b3ac4-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="b3ac4-103">Scott Allen</span><span class="sxs-lookup"><span data-stu-id="b3ac4-103">Scott Allen</span></span>

<span data-ttu-id="b3ac4-104">게시일: 2010년 5월</span><span class="sxs-lookup"><span data-stu-id="b3ac4-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="b3ac4-105">소개</span><span class="sxs-lookup"><span data-stu-id="b3ac4-105">Introduction</span></span>

<span data-ttu-id="b3ac4-106">이 백서에서는 ADO.NET Entity Framework 4.0 및 Visual Studio 2010를 사용 하 여 테스트 가능한 코드를 작성 하는 방법을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="b3ac4-107">이 문서에서는 TDD (테스트 기반 디자인) 또는 BDD (동작 중심 디자인)와 같은 특정 테스트 방법론에 집중 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="b3ac4-108">대신이 문서에서는 Entity Framework ADO.NET를 사용 하는 코드를 작성 하는 방법에 중점을 두고 있지만 자동화 된 방식으로 격리 하 고 테스트 하는 것은 쉽지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="b3ac4-109">데이터 액세스 시나리오에서 테스트를 용이 하 게 하 고 프레임 워크를 사용할 때 이러한 패턴을 적용 하는 방법을 확인 하는 일반적인 디자인 패턴을 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="b3ac4-110">또한 프레임 워크의 특정 기능을 살펴보고 이러한 기능이 테스트 가능한 코드에서 어떻게 작동 하는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="b3ac4-111">테스트 가능한 코드 란 무엇 인가요?</span><span class="sxs-lookup"><span data-stu-id="b3ac4-111">What Is Testable Code?</span></span>

<span data-ttu-id="b3ac4-112">자동화 된 단위 테스트를 사용 하 여 소프트웨어를 확인 하는 기능은 다양 한 이점을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="b3ac4-113">모든 사용자는 응용 프로그램에서 소프트웨어 결함의 수를 줄이고 응용 프로그램의 품질을 높일 수 있다는 것을 알고 있습니다. 하지만 단위 테스트는 버그를 찾는 것 보다 훨씬 더 효과적입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="b3ac4-114">훌륭한 유닛 테스트 도구 모음을 사용 하면 개발 팀이 시간을 절약 하 고 자신이 만든 소프트웨어를 계속 제어할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="b3ac4-115">팀은 기존 코드를 변경 하 고, 새 요구 사항을 충족 하기 위해 소프트웨어를 리팩터링, 다시 디자인 및 재구성 하 고, 테스트 도구 모음을 통해 응용 프로그램의 동작을 확인 하는 동안 새 구성 요소를 응용 프로그램에 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="b3ac4-116">단위 테스트는 복잡성이 증가 함에 따라 쉽게 변경 하 고 소프트웨어의 유지 관리를 유지 하기 위한 빠른 피드백 주기의 일부입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="b3ac4-117">그러나 유닛 테스트에는 가격이 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="b3ac4-118">팀은 단위 테스트를 만들고 유지 관리 하는 데 시간을 투자 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="b3ac4-119">이러한 테스트를 만드는 데 필요한 작업량은 기본 소프트웨어의 테스트 **용이성** 과 직접적으로 관련이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="b3ac4-120">소프트웨어를 얼마나 쉽게 테스트할 수 있나요?</span><span class="sxs-lookup"><span data-stu-id="b3ac4-120">How easy is the software to test?</span></span> <span data-ttu-id="b3ac4-121">테스트 용이성을 염두에 두면 소프트웨어를 설계 하는 팀은 테스트를 취소 하지 않은 소프트웨어로 작업 하는 것 보다 빠르게 효과적인 테스트를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="b3ac4-122">Microsoft는 테스트 용이성을 염두에 ADO.NET Entity Framework 4.0 (EF4)을 설계 했습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="b3ac4-123">개발자가 프레임 워크 코드 자체에 대해 단위 테스트를 작성 하는 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="b3ac4-124">대신, EF4의 테스트 용이성 목표를 통해 프레임 워크를 기반으로 구축 되는 테스트 가능한 코드를 쉽게 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="b3ac4-125">특정 예제를 살펴보기 전에 테스트 가능한 코드의 품질을 이해 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="b3ac4-126">테스트 가능한 코드의 품질</span><span class="sxs-lookup"><span data-stu-id="b3ac4-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="b3ac4-127">테스트 하기 쉬운 코드에는 항상 두 가지 이상의 특징이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="b3ac4-128">먼저 테스트 가능한 코드는 쉽게 **확인할**수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="b3ac4-129">일부 입력 집합을 지정 하는 경우 코드의 출력을 쉽게 관찰할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="b3ac4-130">예를 들어 메서드가 계산 결과를 직접 반환 하기 때문에 다음 메서드를 테스트 하는 것이 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="b3ac4-131">메서드가 계산 된 값을 네트워크 소켓, 데이터베이스 테이블 또는 다음 코드와 같은 파일에 기록 하는 경우 메서드를 테스트 하기가 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="b3ac4-132">테스트에서는 값을 검색 하기 위해 추가 작업을 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="b3ac4-133">둘째로, 테스트 가능한 코드를 쉽게 **격리할**수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="b3ac4-134">다음 의사 코드를 테스트 가능한 코드의 잘못 된 예로 사용 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

``` csharp
    public int ComputePolicyValue(InsurancePolicy policy) {
        using (var connection = new SqlConnection("dbConnection"))
        using (var command = new SqlCommand(query, connection)) {

            // business calculations omitted ...               

            if (totalValue > notificationThreshold) {
                var message = new MailMessage();
                message.Subject = "Warning!";
                var client = new SmtpClient();
                client.Send(message);
            }
        }
        return totalValue;
    }
```

<span data-ttu-id="b3ac4-135">메서드는 관찰 하기 쉽습니다. 보험 정책을 전달 하 고 반환 값이 예상 결과와 일치 하는지 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="b3ac4-136">그러나 메서드를 테스트 하려면 올바른 스키마를 사용 하 여 데이터베이스를 설치 하 고 메서드에서 전자 메일을 보내려고 시도 하는 경우 SMTP 서버를 구성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="b3ac4-137">단위 테스트는 메서드 내에서 계산 논리를 확인 하려고 하지만 전자 메일 서버가 오프 라인 상태 이거나 데이터베이스 서버가 이동 했기 때문에 테스트가 실패할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="b3ac4-138">이러한 오류는 모두 테스트에서 확인 하려는 동작과 관련이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="b3ac4-139">이 동작은 격리 하기 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="b3ac4-140">테스트 가능한 코드를 작성 하기 위해 노력 하는 소프트웨어 개발자는 종종 작성 하는 코드에서 문제의 분리를 유지 하기 위해 노력 하 고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="b3ac4-141">위의 방법은 비즈니스 계산에 집중 하 고 데이터베이스 및 전자 메일 구현 세부 정보를 다른 구성 요소에 위임 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="b3ac4-142">Robert Martin는이를 단일 책임 원칙으로 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="b3ac4-143">개체는 정책 값을 계산 하는 것과 같은 단일의 좁은 책임을 캡슐화 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="b3ac4-144">다른 모든 데이터베이스와 알림 작업은 다른 개체의 책임입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="b3ac4-145">이러한 방식으로 작성 된 코드는 단일 작업에 초점을 맞추고 있으므로 쉽게 격리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="b3ac4-146">.NET에는 단일 책임 원칙을 따르고 격리를 실현 하는 데 필요한 추상화가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="b3ac4-147">인터페이스 정의를 사용 하 고 코드에서 구체적인 형식 대신 인터페이스 추상화를 사용 하도록 강제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="b3ac4-148">이 문서의 뒷부분에서는 위에 표시 된 잘못 된 예와 같은 메서드가 데이터베이스와 통신 하는 것 처럼 *보이는* 인터페이스를 사용 하는 방법에 대해 알아봅니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="b3ac4-149">그러나 테스트 시에는 데이터베이스와 통신 하지 않고 메모리에 데이터를 저장 하는 더미 구현을 대체할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="b3ac4-150">이 더미 구현은 데이터 액세스 코드 또는 데이터베이스 구성에서 관련 되지 않은 문제의 코드를 격리 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="b3ac4-151">격리에는 추가적인 이점이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="b3ac4-152">마지막 방법의 비즈니스 계산은 실행 하는 데 몇 밀리초 밖에 걸리지 않지만 테스트 자체는 네트워크 주위의 코드 홉으로 몇 초 동안 실행 되 고 다양 한 서버에 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="b3ac4-153">단위 테스트를 빠르게 실행 하 여 작은 변경을 용이 하 게 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="b3ac4-154">테스트와 관련 없는 구성 요소에 문제가 있으므로 단위 테스트도 반복 가능 하 고 실패 하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="b3ac4-155">쉽게 관찰 하 고 격리할 수 있는 코드를 작성 하는 것은 개발자가 코드에 대 한 테스트를 작성 하는 데 시간이 더 쉽고, 테스트가 실행 되기를 기다리는 데 걸리는 시간을 줄이고, 더 중요 한 것은 존재 하지 않는 버그를 추적 하는 시간을 줄여 주는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="b3ac4-156">테스트를 통해 코드에서 제공 하는 품질을 테스트 하 고 이해 하는 데 많은 도움이 될 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="b3ac4-157">EF4를 사용 하 여 데이터를 데이터베이스에 저장 하기 위해 사용 하는 코드를 작성 하는 방법을 해결 하려고 합니다. 관찰 가능 하 고 쉽게 격리할 수 있지만, 먼저 데이터 액세스를 위한 테스트 가능한 디자인을 논의 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="b3ac4-158">데이터 지 속성을 위한 디자인 패턴</span><span class="sxs-lookup"><span data-stu-id="b3ac4-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="b3ac4-159">앞에서 설명한 두 가지 잘못 된 예제에는 책임이 너무 많습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="b3ac4-160">첫 번째 잘못 된 예에서는 계산을 수행 하 *고* 파일에 써야 했습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="b3ac4-161">두 번째 잘못 된 예 *에서는 데이터베이스에서 데이터를 읽고* 비즈니스 계산을 수행 하 *고* 전자 메일을 보내야 했습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="b3ac4-162">문제를 구분 하 고 다른 구성 요소에 대 한 책임을 위임 하는 더 작은 메서드를 디자인 하면 테스트 가능한 코드를 작성 하는 데 매우 발전시키 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="b3ac4-163">목표는 작고 집중 된 추상화에서 작업을 작성 하 여 기능을 빌드하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="b3ac4-164">데이터 지 속성에 도달 하면 원하는 작고 집중 된 추상화가 디자인 패턴으로 문서화 된 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="b3ac4-165">Martin Fowler 's Enterprise 응용 프로그램 아키텍처의 책 패턴은 인쇄에서 이러한 패턴을 설명 하는 첫 번째 작업 이었습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="b3ac4-166">이러한 패턴에 대 한 간략 한 설명을 제공 하기 전에 다음 섹션에서 이러한 패턴에 대 한 간략 한 설명을 제공 합니다. 이러한 패턴을 사용 하 여 이러한 패턴을 구현 하는 방법을 Entity Framework ADO.NET</span><span class="sxs-lookup"><span data-stu-id="b3ac4-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="b3ac4-167">리포지토리 패턴</span><span class="sxs-lookup"><span data-stu-id="b3ac4-167">The Repository Pattern</span></span>

<span data-ttu-id="b3ac4-168">Fowler은 "도메인 개체에 액세스 하기 위해 컬렉션 형식 인터페이스를 사용 하 여 도메인 및 데이터 매핑 계층 간의 중재"을 말합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="b3ac4-169">리포지토리 패턴의 목표는 데이터 액세스 minutiae 코드를 격리 하는 것입니다. 이전에 살펴본 것 처럼 테스트 용이성을 위해 필요한 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="b3ac4-170">격리에 대 한 핵심은 저장소가 컬렉션 형식 인터페이스를 사용 하 여 개체를 노출 하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="b3ac4-171">리포지토리를 사용 하기 위해 작성 하는 논리는 리포지토리가 요청한 개체를 구체화 하는 방법을 알 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="b3ac4-172">리포지토리는 데이터베이스와 통신 하거나 메모리 내 컬렉션에서 개체를 반환 하는 것일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="b3ac4-173">모든 코드에서이 컬렉션을 유지 관리 하는 것 처럼 보이지만 컬렉션에서 개체를 검색, 추가 및 삭제할 수 있음을 알고 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="b3ac4-174">기존 .NET 응용 프로그램에서 구체적인 리포지토리는 종종 다음과 같은 제네릭 인터페이스에서 상속 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="b3ac4-175">EF4에 대 한 구현을 제공할 때 인터페이스 정의를 몇 가지 변경 하지만 기본 개념은 동일 하 게 유지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="b3ac4-176">코드는이 인터페이스를 구현 하는 구체적인 리포지토리를 사용 하 여 기본 키 값으로 엔터티를 검색 하거나, 조건자의 평가를 기준으로 엔터티 컬렉션을 검색 하거나, 사용 가능한 모든 엔터티를 간단히 검색할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="b3ac4-177">또한 코드는 리포지토리 인터페이스를 통해 엔터티를 추가 하 고 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="b3ac4-178">Employee 개체의 IRepository가 지정 된 경우 코드는 다음 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="b3ac4-179">코드는 인터페이스 (Employee의 IRepository)를 사용 하기 때문에 인터페이스의 여러 구현에 코드를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="b3ac4-180">한 가지 구현은 EF4에서 지원 되는 구현 이며 개체를 Microsoft SQL Server 데이터베이스에 유지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="b3ac4-181">다른 구현 (테스트 중에 사용 하는 것)은 Employee 개체의 메모리 내 목록에서 지원할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="b3ac4-182">인터페이스는 코드에서 격리를 얻는 데 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="b3ac4-183">IRepository&lt;T&gt; 인터페이스가 저장 작업을 노출 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="b3ac4-184">기존 개체를 업데이트 하는 방법</span><span class="sxs-lookup"><span data-stu-id="b3ac4-184">How do we update existing objects?</span></span> <span data-ttu-id="b3ac4-185">저장 작업을 포함 하는 IRepository 정의에 걸쳐 있을 수 있으며, 이러한 리포지토리의 구현에서 개체를 데이터베이스에 즉시 보관 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="b3ac4-186">그러나 대부분의 응용 프로그램에서는 개체를 개별적으로 유지 하지 않으려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="b3ac4-187">대신 다른 리포지토리에서 개체를 수명으로 가져오고, 비즈니스 활동의 일부로 개체를 수정한 다음, 단일 원자성 작업의 일부로 모든 개체를 유지 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="b3ac4-188">다행히 이러한 유형의 동작을 허용 하는 패턴이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="b3ac4-189">작업 단위 패턴</span><span class="sxs-lookup"><span data-stu-id="b3ac4-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="b3ac4-190">Fowler는 작업 단위를 "비즈니스 트랜잭션의 영향을 받는 개체의 목록을 유지 관리 하 고, 변경 내용을 기록 하지 않으며 동시성 문제를 해결 하는 작업을 조정 합니다." 라고 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="b3ac4-191">저장소에서 사용 하는 개체에 대 한 변경 내용을 추적 하 고, 변경 내용을 커밋하는 작업 단위에 지시할 때 개체에 대 한 변경 내용을 유지 하는 것은 작업 단위를 담당 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="b3ac4-192">또한 모든 리포지토리에 추가 된 새 개체를 가져와 데이터베이스에 삽입 하 고 삭제를 관리 하는 작업 단위를 책임 집니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="b3ac4-193">ADO.NET 데이터 집합을 사용 하 여 작업을 수행한 적이 있으면 작업 단위 패턴에 이미 익숙해져야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="b3ac4-194">ADO.NET 데이터 집합에는 DataRow 개체의 업데이트, 삭제 및 삽입을 추적 하는 기능이 있으며 TableAdapter를 사용 하 여 모든 변경 내용을 데이터베이스에 맞게 조정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="b3ac4-195">그러나 데이터 집합 개체는 기본 데이터베이스의 연결 되지 않은 하위 집합을 모델링 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="b3ac4-196">작업 단위 패턴은 동일한 동작을 나타내지만 데이터 액세스 코드에서 격리 되 고 데이터베이스를 인식 하지 못하는 비즈니스 개체 및 도메인 개체와 함께 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="b3ac4-197">.NET 코드의 작업 단위를 모델링 하는 추상화는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="b3ac4-198">작업 단위에서 리포지토리 참조를 노출 하면 단일 작업 단위 개체에 비즈니스 트랜잭션 중에 구체화 된 모든 엔터티를 추적할 수 있는 기능이 있는지 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="b3ac4-199">실제 작업 단위에 대 한 Commit 메서드를 구현 하는 것은 모든 매직이 데이터베이스와 메모리 내 변경 내용을 조정 하는 데 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="b3ac4-200">Iit에서 작업 참조를 지정 하는 경우 코드는 하나 이상의 리포지토리에서 검색 된 비즈니스 개체를 변경 하 고 원자성 커밋 작업을 사용 하 여 모든 변경 내용을 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="b3ac4-201">지연 로드 패턴</span><span class="sxs-lookup"><span data-stu-id="b3ac4-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="b3ac4-202">Fowler는 이름 지연 로드를 사용 하 여 "필요한 데이터를 모두 포함 하 고 가져오는 방법을 알고 있는 개체"를 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="b3ac4-203">투명 지연 로드는 테스트 가능한 비즈니스 코드를 작성 하 고 관계형 데이터베이스를 사용 하는 경우 중요 한 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="b3ac4-204">예를 들어 다음 코드를 고려 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="b3ac4-205">TimeCards 컬렉션은 어떻게 채워져 있나요?</span><span class="sxs-lookup"><span data-stu-id="b3ac4-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="b3ac4-206">두 가지 가능한 대답이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-206">There are two possible answers.</span></span> <span data-ttu-id="b3ac4-207">직원 리포지토리가 직원을 인출 하 라는 메시지가 표시 되 면 직원 리포지토리가 직원의 연결 된 시간 카드 정보와 함께 두 직원을 모두 검색 하는 쿼리를 발행 하는 것이 한 가지입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="b3ac4-208">관계형 데이터베이스에는 일반적으로 조인 절이 있는 쿼리가 필요 하며 응용 프로그램 요구 사항 보다 더 많은 정보가 검색 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="b3ac4-209">응용 프로그램에서 TimeCards 속성을 건드리지 않아도 되는 경우는 어떻게 되나요?</span><span class="sxs-lookup"><span data-stu-id="b3ac4-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="b3ac4-210">두 번째 대답은 "요청 시" TimeCards 속성을 로드 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="b3ac4-211">이 지연 로드는 코드에서 특정 Api를 호출 하 여 시간 카드 정보를 검색 하지 않으므로 비즈니스 논리에 대해 암시적이 고 투명 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="b3ac4-212">이 코드에서는 필요할 때 시간 카드 정보가 표시 되는 것으로 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="b3ac4-213">일반적으로 메서드 호출의 런타임 가로채기를 포함 하는 지연 로드와 관련 된 몇 가지 매직이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="b3ac4-214">가로채기 코드는 비즈니스 논리를 자유롭게 비즈니스 논리로 유지 하면서 데이터베이스와 통신 하 고 시간 카드 정보를 검색 하는 일을 담당 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="b3ac4-215">이 지연 로드 magic을 사용 하면 비즈니스 코드를 데이터 검색 작업에서 분리 하 여 더 많은 테스트 가능한 코드를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="b3ac4-216">지연 로드의 *단점은 응용 프로그램에 시간* 카드 정보가 필요할 때 코드가 추가 쿼리를 실행 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="b3ac4-217">이는 많은 응용 프로그램에서 중요 한 것은 아니지만 성능에 민감한 응용 프로그램 또는 응용 프로그램에서 많은 직원 개체를 반복 하 고 루프의 각 반복에서 시간 카드를 검색 하는 쿼리를 실행 하는 것은 문제가 되지 않습니다 (종종 N + 1 이라고 함). 쿼리 문제), 지연 로드는 끌어 옵니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="b3ac4-218">이러한 시나리오에서 응용 프로그램은 최대한 효율적으로 시간 카드 정보를 로드 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="b3ac4-219">다행히 다음 섹션으로 이동 하 여 이러한 패턴을 구현할 때 EF4에서 암시적 지연 로드와 효율적인 즉시 로드를 모두 지 원하는 방법을 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="b3ac4-220">Entity Framework를 사용 하 여 패턴 구현</span><span class="sxs-lookup"><span data-stu-id="b3ac4-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="b3ac4-221">좋은 소식은 마지막 섹션에서 설명한 모든 디자인 패턴을 EF4를 사용 하 여 간단 하 게 구현 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="b3ac4-222">간단한 ASP.NET MVC 응용 프로그램을 사용 하 여 직원 및 관련 시간 카드 정보를 편집 하 고 표시 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="b3ac4-223">다음 "일반 이전 CLR 개체" (POCOs)를 사용 하 여 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

``` csharp
    public class Employee {
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime HireDate { get; set; }
        public ICollection<TimeCard> TimeCards { get; set; }
    }

    public class TimeCard {
        public int Id { get; set; }
        public int Hours { get; set; }
        public DateTime EffectiveDate { get; set; }
    }
```

<span data-ttu-id="b3ac4-224">이러한 클래스 정의는 EF4의 다양 한 접근 방식 및 기능을 살펴보기 때문에 약간 변경 되지만 이러한 클래스를 가능한 한 지 속성 무시 (PI)로 유지 하는 것은 의도 된 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="b3ac4-225">PI 개체는 데이터베이스 내에 유지 *되는 상태를 알 수*없습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="b3ac4-226">PI 및 POCOs는 테스트 가능한 소프트웨어와 함께 직접 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="b3ac4-227">POCO 접근 방법을 사용 하는 개체는 데이터베이스 없이 작동할 수 있으므로 제한이 적고 더 유연 하며 테스트 하기가 더 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="b3ac4-228">POCOs를 사용 하 여 Visual Studio에서 EDM (엔터티 데이터 모델)을 만들 수 있습니다 (그림 1 참조).</span><span class="sxs-lookup"><span data-stu-id="b3ac4-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="b3ac4-229">EDM을 사용 하 여 엔터티에 대 한 코드를 생성 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="b3ac4-230">대신, lovingly 만든 엔터티를 직접 사용 하고자 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="b3ac4-231">EDM을 사용 하 여 데이터베이스 스키마를 생성 하 고 개체를 데이터베이스에 매핑하는 데 필요한 메타 데이터 EF4을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![ef test_01](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="b3ac4-233">**그림 1**</span><span class="sxs-lookup"><span data-stu-id="b3ac4-233">**Figure 1**</span></span>

<span data-ttu-id="b3ac4-234">참고: EDM 모델을 먼저 개발 하려는 경우 EDM에서 정리 된 POCO 코드를 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="b3ac4-235">데이터 프로그래밍 팀에서 제공 하는 Visual Studio 2010 확장을 사용 하 여이 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="b3ac4-236">확장을 다운로드 하려면 Visual Studio의 도구 메뉴에서 확장 관리자를 시작 하 고 "POCO" 템플릿의 온라인 갤러리를 검색 합니다 (그림 2 참조).</span><span class="sxs-lookup"><span data-stu-id="b3ac4-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="b3ac4-237">EF에 사용할 수 있는 몇 가지 POCO 템플릿이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="b3ac4-238">템플릿 사용에 대 한 자세한 내용은 " [연습: Entity Framework에 대 한 POCO 템플릿](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)"을 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![ef test_02](~/ef6/media/eftest-02.png)

<span data-ttu-id="b3ac4-240">**그림 2**</span><span class="sxs-lookup"><span data-stu-id="b3ac4-240">**Figure 2**</span></span>

<span data-ttu-id="b3ac4-241">이 POCO 시작 지점에서 테스트 가능한 코드에 대 한 두 가지 접근 방식을 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="b3ac4-242">첫 번째 방법은 Entity Framework API의 추상화를 활용 하 여 작업 단위 및 리포지토리를 구현 하기 때문에 EF 방법을 호출 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="b3ac4-243">두 번째 방법에서는 고유한 사용자 지정 리포지토리 추상화를 만든 다음 각 방법의 장점과 단점을 살펴봅니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="b3ac4-244">먼저 EF 방식을 살펴보는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="b3ac4-245">EF 중심 구현</span><span class="sxs-lookup"><span data-stu-id="b3ac4-245">An EF Centric Implementation</span></span>

<span data-ttu-id="b3ac4-246">ASP.NET MVC 프로젝트에서 다음 컨트롤러 작업을 고려 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="b3ac4-247">작업은 Employee 개체를 검색 하 고 결과를 반환 하 여 직원의 상세 보기를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="b3ac4-248">코드를 테스트할 수 있나요?</span><span class="sxs-lookup"><span data-stu-id="b3ac4-248">Is the code testable?</span></span> <span data-ttu-id="b3ac4-249">작업의 동작을 확인 하는 데 필요한 두 개 이상의 테스트가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="b3ac4-250">먼저 작업에서 올바른 뷰를 반환 하는지 확인 합니다. 간단히 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="b3ac4-251">또한 작업에서 올바른 직원을 검색 하는지 확인 하는 테스트를 작성 하 고, 데이터베이스를 쿼리 하는 코드를 실행 하지 않고이 작업을 수행 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="b3ac4-252">테스트 중인 코드를 격리 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="b3ac4-253">격리는 데이터 액세스 코드 또는 데이터베이스 구성의 버그로 인해 테스트가 실패 하지 않도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="b3ac4-254">테스트에 실패 하는 경우 일부 낮은 수준의 시스템 구성 요소가 아니라 컨트롤러 논리에 버그가 있음을 알게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="b3ac4-255">격리를 위해 앞에서 리포지토리 및 작업 단위에 대해 제공한 인터페이스와 같은 일부 추상화가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="b3ac4-256">리포지토리 패턴은 도메인 개체와 데이터 매핑 계층 간에 중재 설계 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="b3ac4-257">이 시나리오에서 EF4 *는* 데이터 매핑 계층 이며 IObjectSet&lt;t&gt; 라는 리포지토리와 유사한 추상화를 이미 제공 합니다 (네임 스페이스에서).</span><span class="sxs-lookup"><span data-stu-id="b3ac4-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="b3ac4-258">인터페이스 정의는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-258">The interface definition looks like the following.</span></span>

``` csharp
    public interface IObjectSet<TEntity> :
                     IQueryable<TEntity>,
                     IEnumerable<TEntity>,
                     IQueryable,
                     IEnumerable
                     where TEntity : class
    {
        void AddObject(TEntity entity);
        void Attach(TEntity entity);
        void DeleteObject(TEntity entity);
        void Detach(TEntity entity);
    }
```

<span data-ttu-id="b3ac4-259">IObjectSet&lt;T&gt;는 개체 컬렉션 (IEnumerable&lt;T&gt;를 통해)과 비슷하며 시뮬레이션 된 컬렉션에서 개체를 추가 하 고 제거 하는 메서드를 제공 하기 때문에 리포지토리에 대 한 요구 사항을 충족 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="b3ac4-260">연결 및 분리 메서드는 EF4 API의 추가 기능을 노출 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="b3ac4-261">리포지토리에 대 한 인터페이스로 IObjectSet&lt;T&gt;를 사용 하려면 리포지토리를 함께 바인딩하는 작업 단위가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="b3ac4-262">이 인터페이스의 구체적인 구현은 SQL Server와 통신 하며 EF4의 ObjectContext 클래스를 사용 하 여 쉽게 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="b3ac4-263">ObjectContext 클래스는 EF4 API의 실제 작업 단위입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;
            _context = new ObjectContext(connectionString);
        }

        public IObjectSet<Employee> Employees {
            get { return _context.CreateObjectSet<Employee>(); }
        }

        public IObjectSet<TimeCard> TimeCards {
            get { return _context.CreateObjectSet<TimeCard>(); }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

<span data-ttu-id="b3ac4-264">IObjectSet&lt;T&gt;를 life로 가져오는 것은 ObjectContext 개체의 Createobjectset<tentity> 메서드를 호출 하는 것 만큼 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="b3ac4-265">내부적으로 프레임 워크는 EDM에서 제공한 메타 데이터를 사용 하 여 구체적인 ObjectSet&lt;T&gt;을 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="b3ac4-266">클라이언트 코드에서 테스트 용이성을 유지 하는 데 도움이 되는 IObjectSet&lt;T&gt; 인터페이스를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="b3ac4-267">이 구체적 구현은 프로덕션에서 유용 하지만 테스트를 용이 하 게 하기 위해 Iwork 추상화를 사용 하는 방법에 중점을 두어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="b3ac4-268">테스트 두 배</span><span class="sxs-lookup"><span data-stu-id="b3ac4-268">The Test Doubles</span></span>

<span data-ttu-id="b3ac4-269">컨트롤러 작업을 격리 하려면 실제 작업 단위 (ObjectContext에서 지원)와 테스트 double 또는 "가짜" 작업 단위 (메모리 내 작업 수행) 간을 전환 하는 기능이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="b3ac4-270">이러한 유형의 전환을 수행 하는 일반적인 방법은 MVC 컨트롤러에서 작업 단위를 인스턴스화할 수 있도록 하는 것이 아니라 작업 단위를 컨트롤러에 생성자 매개 변수로 전달 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="b3ac4-271">위의 코드는 종속성 주입의 예입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="b3ac4-272">컨트롤러에서 종속성 (작업 단위)을 만들 수는 없지만 컨트롤러에 종속성을 삽입 하는 것은 허용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="b3ac4-273">MVC 프로젝트에서는 사용자 지정 컨트롤러 팩터리를 IoC (반전 제어) 컨테이너와 함께 사용 하 여 종속성 주입을 자동화 하는 것이 일반적입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="b3ac4-274">이러한 항목은이 문서의 범위를 벗어나 있지만이 문서의 끝에 있는 참조를 따라 자세히 알아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="b3ac4-275">테스트에 사용할 수 있는 가짜 작업 구현 단위는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

``` csharp
    public class InMemoryUnitOfWork : IUnitOfWork {
        public InMemoryUnitOfWork() {
            Committed = false;
        }
        public IObjectSet<Employee> Employees {
            get;
            set;
        }

        public IObjectSet<TimeCard> TimeCards {
            get;
            set;
        }

        public bool Committed { get; set; }
        public void Commit() {
            Committed = true;
        }
    }
```

<span data-ttu-id="b3ac4-276">가짜 작업 단위가 커밋된 속성을 노출 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="b3ac4-277">테스트를 용이 하 게 하는 가짜 클래스에 기능을 추가 하는 것이 유용한 경우도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="b3ac4-278">이 경우, 코드에서 커밋된 속성을 확인 하 여 작업 단위를 커밋하는 지 쉽게 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="b3ac4-279">또한 메모리에 직원 및 출퇴근 시간 기록표 개체를 보관 하는 가짜 IObjectSet&lt;T&gt; 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="b3ac4-280">제네릭을 사용 하 여 단일 구현을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-280">We can provide a single implementation using generics.</span></span>

``` csharp
    public class InMemoryObjectSet<T> : IObjectSet<T> where T : class
        public InMemoryObjectSet()
            : this(Enumerable.Empty<T>()) {
        }
        public InMemoryObjectSet(IEnumerable<T> entities) {
            _set = new HashSet<T>();
            foreach (var entity in entities) {
                _set.Add(entity);
            }
            _queryableSet = _set.AsQueryable();
        }
        public void AddObject(T entity) {
            _set.Add(entity);
        }
        public void Attach(T entity) {
            _set.Add(entity);
        }
        public void DeleteObject(T entity) {
            _set.Remove(entity);
        }
        public void Detach(T entity) {
            _set.Remove(entity);
        }
        public Type ElementType {
            get { return _queryableSet.ElementType; }
        }
        public Expression Expression {
            get { return _queryableSet.Expression; }
        }
        public IQueryProvider Provider {
            get { return _queryableSet.Provider; }
        }
        public IEnumerator<T> GetEnumerator() {
            return _set.GetEnumerator();
        }
        IEnumerator IEnumerable.GetEnumerator() {
            return GetEnumerator();
        }

        readonly HashSet<T> _set;
        readonly IQueryable<T> _queryableSet;
    }
```

<span data-ttu-id="b3ac4-281">이 테스트는 대부분의 작업을 기본 HashSet&lt;T&gt; 개체로 위임 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="b3ac4-282">IObjectSet&lt;T&gt;에는 T를 클래스 (참조 형식)로 적용 하는 제네릭 제약 조건이 필요 하며,이를 통해 IQueryable&lt;T&gt;도 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="b3ac4-283">표준 LINQ 연산자를 사용 하 여 메모리 내 컬렉션을 IQueryable&lt;T&gt;으로 표시 하는 것이 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="b3ac4-284">테스트</span><span class="sxs-lookup"><span data-stu-id="b3ac4-284">The Tests</span></span>

<span data-ttu-id="b3ac4-285">기존 단위 테스트는 단일 테스트 클래스를 사용 하 여 단일 MVC 컨트롤러의 모든 작업에 대 한 모든 테스트를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="b3ac4-286">빌드된 메모리 내 fakes를 사용 하 여 이러한 테스트 또는 모든 형식의 단위 테스트를 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="b3ac4-287">그러나이 문서에서는 모놀리식 테스트 클래스 접근 방식을 사용 하지 않고, 대신 특정 기능에 초점을 맞춘 테스트를 그룹화 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span><span data-ttu-id="b3ac4-288">  예를 들어 "새 직원 만들기"는 테스트 하려는 기능 일 수 있으므로 단일 테스트 클래스를 사용 하 여 새 직원을 만드는 단일 컨트롤러 작업을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-288">  For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="b3ac4-289">이러한 모든 세분화 된 테스트 클래스에 필요한 몇 가지 일반적인 설정 코드가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="b3ac4-290">예를 들어 항상 메모리 내 리포지토리 및 가짜 작업 단위를 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="b3ac4-291">또한 가짜 작업 단위가 삽입 된 employee controller 인스턴스가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="b3ac4-292">기본 클래스를 사용 하 여 테스트 클래스 간에이 일반적인 설치 코드를 공유 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-292">We’ll share this common setup code across test classes by using a base class.</span></span>

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .ToList();
            _repository = new InMemoryObjectSet<Employee>(_employeeData);
            _unitOfWork = new InMemoryUnitOfWork();
            _unitOfWork.Employees = _repository;
            _controller = new EmployeeController(_unitOfWork);
        }

        protected IList<Employee> _employeeData;
        protected EmployeeController _controller;
        protected InMemoryObjectSet<Employee> _repository;
        protected InMemoryUnitOfWork _unitOfWork;
    }
```

<span data-ttu-id="b3ac4-293">기본 클래스에서 사용 하는 "개체 어머니"는 테스트 데이터를 만들기 위한 하나의 일반적인 패턴입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="b3ac4-294">어머니 개체는 여러 테스트 설비에서 사용할 테스트 엔터티를 인스턴스화하는 팩터리 메서드를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

``` csharp
    public static class EmployeeObjectMother {
        public static IEnumerable<Employee> CreateEmployees() {
            yield return new Employee() {
                Id = 1, Name = "Scott", HireDate=new DateTime(2002, 1, 1)
            };
            yield return new Employee() {
                Id = 2, Name = "Poonam", HireDate=new DateTime(2001, 1, 1)
            };
            yield return new Employee() {
                Id = 3, Name = "Simon", HireDate=new DateTime(2008, 1, 1)
            };
        }
        // ... more fake data for different scenarios
    }
```

<span data-ttu-id="b3ac4-295">EmployeeControllerTestBase를 다양 한 테스트 fixture의 기본 클래스로 사용할 수 있습니다 (그림 3 참조).</span><span class="sxs-lookup"><span data-stu-id="b3ac4-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="b3ac4-296">각 테스트 fixture는 특정 컨트롤러 작업을 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="b3ac4-297">예를 들어, 한 테스트 fixture는 HTTP GET 요청 중에 사용 된 만들기 작업을 테스트 하는 데 중점을 두고 (직원을 만들기 위한 보기를 표시 하기 위해) 다른 fixture는 HTTP POST 요청에 사용 된 만들기 작업에 집중 합니다. 직원을 만드는 사용자입니다.)</span><span class="sxs-lookup"><span data-stu-id="b3ac4-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="b3ac4-298">각 파생 클래스는 특정 컨텍스트에 필요한 설치를 담당 하며 특정 테스트 컨텍스트의 결과를 확인 하는 데 필요한 어설션을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![ef test_03](~/ef6/media/eftest-03.png)

<span data-ttu-id="b3ac4-300">**그림 3**</span><span class="sxs-lookup"><span data-stu-id="b3ac4-300">**Figure 3**</span></span>

<span data-ttu-id="b3ac4-301">여기에 제공 된 명명 규칙 및 테스트 스타일은 테스트 가능한 코드에 필요 하지 않습니다. 단 하나의 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="b3ac4-302">그림 4는 Visual Studio 2010 용 Jet Brains Resharper test runner 플러그 인에서 실행 되는 테스트를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![ef test_04](~/ef6/media/eftest-04.png)

<span data-ttu-id="b3ac4-304">**그림 4**</span><span class="sxs-lookup"><span data-stu-id="b3ac4-304">**Figure 4**</span></span>

<span data-ttu-id="b3ac4-305">공유 설정 코드를 처리 하기 위한 기본 클래스를 사용 하는 경우 각 컨트롤러 작업에 대 한 단위 테스트는 작고 간단 하 게 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="b3ac4-306">테스트는 메모리 내 작업을 수행 하기 때문에 신속 하 게 실행 되며 관련이 없는 인프라 또는 환경 문제로 인해 실패 해서는 안 됩니다 (테스트 중인 단위를 격리 했으므로).</span><span class="sxs-lookup"><span data-stu-id="b3ac4-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerCreateActionPostTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldAddNewEmployeeToRepository() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_repository.Contains(_newEmployee));
        }
        [TestMethod]
        public void ShouldCommitUnitOfWork() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_unitOfWork.Committed);
        }
        // ... more tests

        Employee _newEmployee = new Employee() {
            Name = "NEW EMPLOYEE",
            HireDate = new System.DateTime(2010, 1, 1)
        };
    }
```

<span data-ttu-id="b3ac4-307">이러한 테스트에서 기본 클래스는 대부분의 설치 작업을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="b3ac4-308">기본 클래스 생성자는 메모리 내 리포지토리, 가짜 작업 단위 및 EmployeeController 클래스의 인스턴스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="b3ac4-309">테스트 클래스는이 기본 클래스에서 파생 되며, Create 메서드 테스트에 대 한 세부 정보를 중심으로 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="b3ac4-310">이 경우 "정렬, 작업 및 어설션" 단계에 대 한 자세한 내용은 모든 단위 테스트 절차에 있지만.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="b3ac4-311">들어오는 데이터를 시뮬레이트하는 newEmployee 개체를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="b3ac4-312">EmployeeController의 만들기 작업을 호출 하 고 newEmployee를 전달 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="b3ac4-313">만들기 작업에서 예상 된 결과를 생성 하는지 확인 합니다. 직원이 리포지토리에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="b3ac4-314">빌드된 항목을 통해 EmployeeController 작업을 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="b3ac4-315">예를 들어 직원 컨트롤러의 인덱스 작업에 대 한 테스트를 작성 하는 경우 테스트 기본 클래스에서 상속 하 여 테스트에 대해 동일한 기본 설정을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="b3ac4-316">다시 기본 클래스는 메모리 내 리포지토리, 가짜 작업 단위 및 EmployeeController의 인스턴스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="b3ac4-317">인덱스 작업에 대 한 테스트는 인덱스 동작을 호출 하 고 작업에서 반환 하는 모델의 품질을 테스트 하는 데만 집중 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count);
        }
        [TestMethod]
        public void ShouldOrderModelByHiredateAscending() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                         as IEnumerable<Employee>;
            Assert.IsTrue(model.SequenceEqual(
                           _employeeData.OrderBy(e => e.HireDate)));
        }
        // ...
    }
```

<span data-ttu-id="b3ac4-318">메모리 내 fakes를 사용 하 여 만드는 테스트는 소프트웨어의 *상태* 를 테스트 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="b3ac4-319">예를 들어 만들기 작업을 테스트할 때 만들기 작업이 실행 된 후 리포지토리의 상태를 검사 하려고 합니다. 리포지토리가 새 직원을 보유 합니까?</span><span class="sxs-lookup"><span data-stu-id="b3ac4-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="b3ac4-320">나중에 상호 작용 기반 테스트를 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="b3ac4-321">테스트 중인 코드에서 개체에 대 한 적절 한 메서드를 호출 하 고 올바른 매개 변수를 전달 했는지 여부를 상호 작용 기반 테스트에서 요청 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="b3ac4-322">지금은 지연 로드 인 다른 디자인 패턴의 덮개를 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="b3ac4-323">즉시 로드 및 지연 로드</span><span class="sxs-lookup"><span data-stu-id="b3ac4-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="b3ac4-324">ASP.NET MVC 웹 응용 프로그램의 특정 시점에서 직원의 정보를 표시 하 고 직원의 연결 된 시간 카드를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="b3ac4-325">예를 들어 시스템의 직원 이름과 총 시간 카드 수를 표시 하는 시간 카드 요약 표시가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="b3ac4-326">이 기능을 구현 하기 위해 수행할 수 있는 몇 가지 방법이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="b3ac4-327">프로젝션</span><span class="sxs-lookup"><span data-stu-id="b3ac4-327">Projection</span></span>

<span data-ttu-id="b3ac4-328">요약을 만드는 한 가지 쉬운 방법은 보기에 표시 하려는 정보 전용 모델을 생성 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="b3ac4-329">이 시나리오에서 모델은 다음과 같이 표시 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="b3ac4-330">EmployeeSummaryViewModel는 엔터티가 아닙니다. 즉, 데이터베이스에서 유지 하려는 항목이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="b3ac4-331">이 클래스를 사용 하 여 강력한 형식의 데이터를 뷰로 무작위 이동할 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="b3ac4-332">뷰 모델은 동작 (메서드 없음)만 포함 하 고 속성을 포함 하기 때문에 DTO (데이터 전송 개체)와 같습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="b3ac4-333">속성에는 이동 해야 하는 데이터가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="b3ac4-334">LINQ의 표준 프로젝션 연산자 (Select 연산자)를 사용 하 여이 뷰 모델을 쉽게 인스턴스화할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

``` csharp
    public ViewResult Summary(int id) {
        var model = _unitOfWork.Employees
                               .Where(e => e.Id == id)
                               .Select(e => new EmployeeSummaryViewModel
                                  {
                                    Name = e.Name,
                                    TotalTimeCards = e.TimeCards.Count()
                                  })
                               .Single();
        return View(model);
    }
```

<span data-ttu-id="b3ac4-335">위의 코드에는 두 가지 주요 기능이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-335">There are two notable features to the above code.</span></span> <span data-ttu-id="b3ac4-336">먼저, 코드를 쉽게 관찰 하 고 격리할 수 있으므로 쉽게 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="b3ac4-337">Select 연산자는 실제 작업 단위에 대해 수행 되는 것 처럼 메모리 내 fakes에 대해서만 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerSummaryActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithCorrectEmployeeSummary() {
            var id = 1;
            var result = _controller.Summary(id);
            var model = result.ViewData.Model as EmployeeSummaryViewModel;
            Assert.IsTrue(model.TotalTimeCards == 3);
        }
        // ...
    }
```

<span data-ttu-id="b3ac4-338">두 번째 주목할 만한 기능은 코드를 사용 하 여 직원 및 시간 카드 정보를 함께 EF4 하는 효율적인 단일 쿼리를 생성 하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="b3ac4-339">특별 한 Api를 사용 하지 않고 직원 정보 및 시간 카드 정보를 동일한 개체에 로드 했습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="b3ac4-340">이 코드는 메모리 내 데이터 원본 및 원격 데이터 원본에 대해 작동 하는 표준 LINQ 연산자를 사용 하 여 필요한 정보만 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="b3ac4-341">EF4는 LINQ 쿼리 및 C\# 컴파일러로 생성 된 식 트리를 하나의 효율적인 T-sql 쿼리로 변환할 수 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

``` SQL
    SELECT
    [Limit1].[Id] AS [Id],
    [Limit1].[Name] AS [Name],
    [Limit1].[C1] AS [C1]
    FROM (SELECT TOP (2)
      [Project1].[Id] AS [Id],
      [Project1].[Name] AS [Name],
      [Project1].[C1] AS [C1]
      FROM (SELECT
        [Extent1].[Id] AS [Id],
        [Extent1].[Name] AS [Name],
        (SELECT COUNT(1) AS [A1]
         FROM [dbo].[TimeCards] AS [Extent2]
         WHERE [Extent1].[Id] =
               [Extent2].[EmployeeTimeCard_TimeCard_Id]) AS [C1]
              FROM [dbo].[Employees] AS [Extent1]
               WHERE [Extent1].[Id] = @p__linq__0
         )  AS [Project1]
    )  AS [Limit1]
```

<span data-ttu-id="b3ac4-342">뷰 모델 또는 DTO 개체를 사용 하지 않고 실제 엔터티를 사용 하 여 작업 하지 않으려는 경우도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="b3ac4-343">직원 *및* 직원의 시간 카드가 필요 하다 고 생각 되 면 관련 데이터를 원활 하 고 효율적인 방식으로 적극적으로 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="b3ac4-344">명시적인 즉시 로드</span><span class="sxs-lookup"><span data-stu-id="b3ac4-344">Explicit Eager Loading</span></span>

<span data-ttu-id="b3ac4-345">관련 엔터티 정보를 적극적으로 로드 하려면 비즈니스 논리 (또는이 시나리오에서는 컨트롤러 동작 논리)에 대 한 몇 가지 메커니즘을 통해 리포지토리를 원하는 것으로 요구 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="b3ac4-346">EF4 ObjectQuery&lt;T&gt; 클래스는 쿼리 중에 검색할 관련 개체를 지정 하는 Include 메서드를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="b3ac4-347">EF4 ObjectContext는 ObjectQuery&lt;T&gt;에서 상속 되는 구체적인 ObjectSet&lt;T&gt; 클래스를 통해 엔터티를 노출 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span><span data-ttu-id="b3ac4-348">  컨트롤러 작업에서 ObjectSet&lt;T&gt; 참조를 사용 하는 경우 다음 코드를 작성 하 여 각 직원의 시간 카드 정보에 대 한 즉시 로드를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-348">  If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="b3ac4-349">그러나 코드를 테스트 가능 하 게 유지 하려고 하기 때문에 실제 작업 단위 클래스 외부에서&lt;T&gt;의 ObjectSet을 노출 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="b3ac4-350">대신, 가짜 하기가 더 쉬운 IObjectSet&lt;T&gt; 인터페이스를 사용 하지만 IObjectSet&lt;T&gt;는 Include 메서드를 정의 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="b3ac4-351">LINQ의 장점은 자체 Include 연산자를 만들 수 있다는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

``` csharp
    public static class QueryableExtensions {
        public static IQueryable<T> Include<T>
                (this IQueryable<T> sequence, string path) {
            var objectQuery = sequence as ObjectQuery<T>;
            if(objectQuery != null)
            {
                return objectQuery.Include(path);
            }
            return sequence;
        }
    }
```

<span data-ttu-id="b3ac4-352">이 Include 연산자는 IObjectSet&lt;T&gt;대신 IQueryable&lt;T&gt;에 대 한 확장 메서드로 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="b3ac4-353">이를 통해 IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;및 ObjectSet&lt;T&gt;를 비롯 하 여 더 광범위 한 형식으로 메서드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="b3ac4-354">기본 시퀀스가 진짜 EF4 ObjectQuery&lt;T&gt;가 아닌 경우에는 피해를 주지 않으며 Include 연산자는 작동 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="b3ac4-355">기본 시퀀스가 ObjectQuery&lt;T&gt; 이거나 ObjectQuery&lt;T&gt;에서 파생 *됨* ) 이면 추가 데이터에 대 한 요구 사항이 표시 되 고 적절 한 SQL 쿼리가 작성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="b3ac4-356">이 new 연산자를 사용 하면 리포지토리에서 시간 카드 정보를 즉시 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="b3ac4-357">실제 ObjectContext에 대해 실행 하는 경우 코드에서 다음과 같은 단일 쿼리를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="b3ac4-358">이 쿼리는 직원 개체를 구체화 하 고 해당 TimeCards 속성을 완전히 채울 수 있도록 데이터베이스에서 충분 한 정보를 한 번에 수집 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

``` SQL
    SELECT
    [Project1].[Id] AS [Id],
    [Project1].[Name] AS [Name],
    [Project1].[HireDate] AS [HireDate],
    [Project1].[C1] AS [C1],
    [Project1].[Id1] AS [Id1],
    [Project1].[Hours] AS [Hours],
    [Project1].[EffectiveDate] AS [EffectiveDate],
    [Project1].[EmployeeTimeCard_TimeCard_Id] AS [EmployeeTimeCard_TimeCard_Id]
    FROM ( SELECT
         [Extent1].[Id] AS [Id],
         [Extent1].[Name] AS [Name],
         [Extent1].[HireDate] AS [HireDate],
         [Extent2].[Id] AS [Id1],
         [Extent2].[Hours] AS [Hours],
         [Extent2].[EffectiveDate] AS [EffectiveDate],
         [Extent2].[EmployeeTimeCard_TimeCard_Id] AS
                    [EmployeeTimeCard_TimeCard_Id],
         CASE WHEN ([Extent2].[Id] IS NULL) THEN CAST(NULL AS int)
         ELSE 1 END AS [C1]
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

<span data-ttu-id="b3ac4-359">작업 메서드 내의 코드는 완전히 테스트 가능한 상태로 유지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="b3ac4-360">Include 연산자를 지원 하기 위해 fakes에 대 한 추가 기능을 제공 하지 않아도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="b3ac4-361">잘못 된 뉴스는 지 속성 무시를 유지 하기 위해 필요한 코드 내부에서 Include 연산자를 사용 해야 했습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="b3ac4-362">이는 테스트 가능한 코드를 빌드할 때 평가 해야 하는 장단점의 대표적인 예입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="b3ac4-363">성능 목표를 충족 하기 위해 지 속성 문제가 리포지토리 추상화 외부에서 누출 되도록 해야 하는 경우가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="b3ac4-364">즉시 로드 하는 대신 지연 로드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="b3ac4-365">지연 로드는 연결 된 데이터에 대 한 요구 사항을 명시적으로 알리기 위한 비즈니스 코드가 필요 *하지* 않음을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="b3ac4-366">대신 응용 프로그램에서 엔터티를 사용 하 고 추가 데이터가 필요한 경우에는 필요에 따라 데이터를 로드 Entity Framework 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="b3ac4-367">지연 로드</span><span class="sxs-lookup"><span data-stu-id="b3ac4-367">Lazy Loading</span></span>

<span data-ttu-id="b3ac4-368">비즈니스 논리에 필요한 데이터를 모르는 시나리오를 쉽게 생각해 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="b3ac4-369">논리에 employee 개체가 필요 하다는 것을 알 수 있지만, 이러한 경로 중 일부에는 직원의 시간 카드 정보가 필요 하 고 일부는 다른 실행 경로를 분기할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="b3ac4-370">이와 같은 시나리오는 데이터 magically 필요에 따라 나타나므로 암시적 지연 로드에 적합 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="b3ac4-371">지연 로드 (지연 된 로드 라고도 함)는 엔터티 개체에 대 한 몇 가지 요구 사항을 충족 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="b3ac4-372">진정한 지 속성 무시을 포함 하는 POCOs는 지 속성 계층의 요구 사항을 충족 하지 않지만 진정한 지 속성 무시는 실제로 달성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span><span data-ttu-id="b3ac4-373">  대신 지 속성 무시을 상대적 각도로 측정 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-373">  Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="b3ac4-374">지 속성 지향 기본 클래스에서 상속 하거나 특수화 된 컬렉션을 사용 하 여 POCOs에서 지연 로드를 수행 해야 하는 경우에는 바람직하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="b3ac4-375">다행히 EF4는 더 저렴 한 솔루션을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="b3ac4-376">거의 검색할 때 없음</span><span class="sxs-lookup"><span data-stu-id="b3ac4-376">Virtually Undetectable</span></span>

<span data-ttu-id="b3ac4-377">POCO 개체를 사용 하는 경우 EF4는 엔터티에 대 한 런타임 프록시를 동적으로 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="b3ac4-378">이러한 프록시는 구체화 된 POCOs를 래핑하여 추가 작업을 수행 하기 위해 각 속성 get 및 set 작업을 가로채 여 추가 서비스를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="b3ac4-379">이러한 서비스 중 하나는 원하는 지연 로드 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="b3ac4-380">다른 서비스는 프로그램에서 엔터티의 속성 값을 변경할 때 기록할 수 있는 효율적인 변경 내용 추적 메커니즘입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="b3ac4-381">변경 목록은 SaveChanges 메서드에서 UPDATE 명령을 사용 하 여 수정 된 엔터티를 유지 하기 위해 ObjectContext에서 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="b3ac4-382">그러나 이러한 프록시가 작동 하려면 엔터티에 대 한 속성 get 및 set 작업에 연결 하는 방법이 필요 하며, 프록시는 가상 멤버를 재정의 하 여이 목표를 달성 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="b3ac4-383">따라서 암시적 지연 로드 및 효율적인 변경 내용 추적을 원할 경우 POCO 클래스 정의로 돌아가서 속성을 가상으로 표시 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="b3ac4-384">직원 엔터티가 대부분 지 속성을 무시 하는 것으로 말할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="b3ac4-385">유일한 요구 사항은 가상 멤버를 사용 하는 것 이며,이는 코드의 테스트 용이성에 영향을 주지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="b3ac4-386">특별 한 기본 클래스에서 파생 되거나 지연 로드 전용 특수 컬렉션을 사용할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="b3ac4-387">코드에서 설명 하는 것 처럼 ICollection&lt;T&gt;를 구현 하는 모든 클래스는 관련 엔터티를 보유 하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="b3ac4-388">작업 단위 내에서 수행 해야 하는 한 가지 사소한 변경도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="b3ac4-389">ObjectContext 개체로 직접 작업할 때 지연 로드는 기본적으로 *해제* 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="b3ac4-390">지연 된 로드를 사용 하도록 ContextOptions 속성에 설정할 수 있는 속성이 있으며, 모든 위치에서 지연 로드를 사용 하도록 설정 하려는 경우 실제 작업 단위 내에서이 속성을 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
         public SqlUnitOfWork() {
             // ...
             _context = new ObjectContext(connectionString);
             _context.ContextOptions.LazyLoadingEnabled = true;
         }    
         // ...
     }
```

<span data-ttu-id="b3ac4-391">암시적 지연 로드를 사용 하는 경우 응용 프로그램 코드는 추가 데이터를 로드 하는 EF에 필요한 작업을 못한다는 하 고 나머지는 직원 및 직원의 연결 된 시간 카드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="b3ac4-392">지연 로드를 사용 하면 응용 프로그램 코드를 더 쉽게 작성할 수 있으며 프록시 매직을 사용 하면 코드를 완전히 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="b3ac4-393">작업 단위에 대 한 메모리 내 fakes는 테스트 중에 필요한 경우 연결 된 데이터를 사용 하 여 가짜 엔터티를 미리 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="b3ac4-394">이 시점에서 IObjectSet&lt;T&gt;를 사용 하 여 리포지토리를 작성 하 고, 지 속성 프레임 워크의 모든 기호를 숨기는 추상화를 살펴볼 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="b3ac4-395">사용자 지정 리포지토리</span><span class="sxs-lookup"><span data-stu-id="b3ac4-395">Custom Repositories</span></span>

<span data-ttu-id="b3ac4-396">이 문서에서 작업 단위 디자인 패턴을 처음 제공할 때 작업 단위가 표시 될 수 있는 몇 가지 샘플 코드를 제공 했습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="b3ac4-397">우리가 작업 하 고 있는 직원 및 직원 시간 카드 시나리오를 사용 하 여이 원래 아이디어를 다시 제공 해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="b3ac4-398">이 작업 단위와 마지막 섹션에서 만든 작업 단위 간의 주요 차이점은이 작업 단위가 EF4 프레임 워크의 추상화를 사용 하지 않는 것입니다 (IObjectSet&lt;T&gt;).</span><span class="sxs-lookup"><span data-stu-id="b3ac4-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="b3ac4-399">IObjectSet&lt;T&gt;는 리포지토리 인터페이스와 잘 작동 하지만 노출 하는 API는 응용 프로그램의 요구 사항에 완벽 하 게 맞지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="b3ac4-400">이 예정 된 방법에서는 사용자 지정 IRepository&lt;T&gt; 추상화를 사용 하 여 리포지토리를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="b3ac4-401">테스트 기반 디자인, 동작 중심 디자인 및 도메인 기반 방법론 디자인을 따르는 개발자는 여러 가지 이유로 IRepository&lt;T&gt; 접근법을 선호 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="b3ac4-402">먼저 IRepository&lt;T&gt; 인터페이스는 "손상 방지" 계층을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="b3ac4-403">도메인 기반 디자인 설명서에서 Eric Evans에 설명 된 대로 손상 방지 계층은 지 속성 API와 같은 인프라 Api에서 도메인 코드를 유지 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="b3ac4-404">둘째로, 개발자는 테스트를 작성 하는 동안 검색 된 대로 응용 프로그램의 정확한 요구를 충족 하는 메서드를 리포지토리에 빌드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="b3ac4-405">예를 들어 ID 값을 사용 하 여 단일 엔터티를 찾아야 하는 경우가 많으므로 리포지토리 인터페이스에 FindById 메서드를 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span><span data-ttu-id="b3ac4-406">  IRepository&lt;T&gt; 정의는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-406">  Our IRepository&lt;T&gt; definition will look like the following.</span></span>

``` csharp
    public interface IRepository<T>
                    where T : class, IEntity {
        IQueryable<T> FindAll();
        IQueryable<T> FindWhere(Expression<Func\<T, bool>> predicate);
        T FindById(int id);       
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="b3ac4-407">여기서는 IQueryable&lt;T&gt; 인터페이스를 사용 하 여 엔터티 컬렉션을 노출 하도록 다시 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="b3ac4-408">IQueryable&lt;T&gt;를 사용 하면 LINQ 식 트리가 EF4 공급자로 이동 하 고 공급자에 게 쿼리를 전체적으로 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="b3ac4-409">두 번째 옵션은 IEnumerable&lt;T&gt;를 반환 하는 것입니다. 즉, EF4 LINQ 공급자는 리포지토리 내부에서 빌드된 식만 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="b3ac4-410">리포지토리 외부에서 수행 되는 모든 그룹화, 정렬 및 프로젝션은 데이터베이스로 전송 된 SQL 명령으로 구성 되지 않으므로 성능이 저하 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="b3ac4-411">반면에 IEnumerable&lt;T&gt; 결과만 반환 하는 리포지토리는 새 SQL 명령을 사용 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="b3ac4-412">두 방법을 모두 사용할 수 있으며 두 가지 방법을 모두 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="b3ac4-413">제네릭과 EF4 ObjectContext API를 사용 하 여 IRepository&lt;T&gt; 인터페이스의 단일 구현을 제공 하는 것이 간단 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

``` csharp
    public class SqlRepository<T> : IRepository<T>
                                    where T : class, IEntity {
        public SqlRepository(ObjectContext context) {
            _objectSet = context.CreateObjectSet<T>();
        }
        public IQueryable<T> FindAll() {
            return _objectSet;
        }
        public IQueryable<T> FindWhere(
                               Expression<Func\<T, bool>> predicate) {
            return _objectSet.Where(predicate);
        }
        public T FindById(int id) {
            return _objectSet.Single(o => o.Id == id);
        }
        public void Add(T newEntity) {
            _objectSet.AddObject(newEntity);
        }
        public void Remove(T entity) {
            _objectSet.DeleteObject(entity);
        }
        protected ObjectSet<T> _objectSet;
    }
```

<span data-ttu-id="b3ac4-414">클라이언트에서 엔터티를 가져오기 위해 메서드를 호출 해야 하기 때문에 IRepository&lt;T&gt; 접근 방식은 쿼리를 추가로 제어 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="b3ac4-415">메서드 내에서 추가 검사 및 LINQ 연산자를 제공 하 여 응용 프로그램 제약 조건을 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="b3ac4-416">인터페이스에는 제네릭 형식 매개 변수에 대 한 두 개의 제약 조건이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="b3ac4-417">첫 번째 제약 조건은 ObjectSet&lt;T&gt;에 필요한 클래스 단점입니다. 두 번째 제약 조건은 엔터티가 IEntity (응용 프로그램에 대해 생성 된 추상화)를 구현 하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="b3ac4-418">IEntity 인터페이스를 사용 하면 엔터티는 읽을 수 있는 Id 속성을 갖게 되 고 FindById 메서드에서이 속성을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="b3ac4-419">IEntity는 다음 코드를 사용 하 여 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="b3ac4-420">엔터티는이 인터페이스를 구현 하는 데 필요 하므로 IEntity는 지 속성 무시의 작은 위반으로 간주 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="b3ac4-421">지 속성 무시은 절충에 대 한 것 이며, 대부분의 FindById 기능은 인터페이스에 의해 적용 되는 제약 조건 보다 더 큽니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="b3ac4-422">인터페이스는 테스트 용이성에 영향을 주지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="b3ac4-423">&lt;T&gt; 라이브 IRepository를 인스턴스화하면 EF4 ObjectContext가 필요 하므로 구체적인 작업 단위 구현은 인스턴스화를 관리 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;

            _context = new ObjectContext(connectionString);
            _context.ContextOptions.LazyLoadingEnabled = true;
        }

        public IRepository<Employee> Employees {
            get {
                if (_employees == null) {
                    _employees = new SqlRepository<Employee>(_context);
                }
                return _employees;
            }
        }

        public IRepository<TimeCard> TimeCards {
            get {
                if (_timeCards == null) {
                    _timeCards = new SqlRepository<TimeCard>(_context);
                }
                return _timeCards;
            }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        SqlRepository<Employee> _employees = null;
        SqlRepository<TimeCard> _timeCards = null;
        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

### <a name="using-the-custom-repository"></a><span data-ttu-id="b3ac4-424">사용자 지정 리포지토리 사용</span><span class="sxs-lookup"><span data-stu-id="b3ac4-424">Using the Custom Repository</span></span>

<span data-ttu-id="b3ac4-425">사용자 지정 리포지토리를 사용 하는 것은 IObjectSet&lt;T&gt;기반 리포지토리를 사용 하는 것과 크게 다르지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="b3ac4-426">LINQ 연산자를 속성에 직접 적용 하는 대신, 먼저 리포지토리의 메서드 하나를 호출 하 여 IQueryable&lt;T&gt; 참조를 잡기를 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="b3ac4-427">이전에 구현한 custom Include 연산자는 변경 하지 않고 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="b3ac4-428">리포지토리의 FindById 메서드는 단일 엔터티를 검색 하려는 작업에서 중복 된 논리를 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="b3ac4-429">검사 한 두 방법의 테스트 용이성에는 중요 한 차이점이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="b3ac4-430">마지막 섹션에서 했던 것과 마찬가지로, HashSet&lt;직원&gt;에서 지 원하는 구체적인 클래스를 빌드하여 IRepository&lt;T&gt;의 가짜 구현을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="b3ac4-431">그러나 일부 개발자는 fakes를 빌드하는 대신 모의 개체와 모의 개체 프레임 워크를 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="b3ac4-432">Mock를 사용 하 여 구현을 테스트 하 고 다음 섹션에서 mock와 fakes의 차이점에 대해 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="b3ac4-433">Mock로 테스트</span><span class="sxs-lookup"><span data-stu-id="b3ac4-433">Testing with Mocks</span></span>

<span data-ttu-id="b3ac4-434">Martin Fowler에서 "test double"을 호출 하는 방법을 다양 하 게 빌드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="b3ac4-435">Test double (예: movie stunt double)은 테스트 하는 동안 실제 프로덕션 개체의 "독립"에 대해 빌드하는 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="b3ac4-436">만든 메모리 내 리포지토리는 SQL Server와 통신 하는 리포지토리에 대 한 테스트 double입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="b3ac4-437">단위 테스트 중에 이러한 두 가지 테스트를 사용 하 여 코드를 격리 하 고 테스트를 신속 하 게 실행 하는 방법을 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="b3ac4-438">빌드된 두 배의 테스트에는 진정한 작업 구현이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="b3ac4-439">내부적으로는 개체의 구체적인 컬렉션을 저장 하 고 테스트 중에 리포지토리를 조작할 때이 컬렉션에서 개체를 추가 및 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="b3ac4-440">이러한 방식으로 테스트를 빌드하는 개발자는 실제 코드와 작업을 구현 하는 것이 가능 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span><span data-ttu-id="b3ac4-441">  이러한 두 double 테스트는 *fakes*를 호출 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-441">  These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="b3ac4-442">이러한 구현에는 작동 하는 구현이 있지만 프로덕션이 사용 하기에 충분 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="b3ac4-443">가짜 리포지토리는 실제로 데이터베이스에 쓰지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="b3ac4-444">가짜 SMTP 서버는 실제로 네트워크를 통해 전자 메일 메시지를 보내지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="b3ac4-445">Mock와 Fakes 비교</span><span class="sxs-lookup"><span data-stu-id="b3ac4-445">Mocks versus Fakes</span></span>

<span data-ttu-id="b3ac4-446">*모의*형식으로 알려진 다른 형식의 테스트 double이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="b3ac4-447">Fakes에는 작업 구현이 있지만 mock는 구현 없이 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="b3ac4-448">모의 개체 프레임 워크의 도움을 통해 런타임에 이러한 모의 개체를 구성 하 고 테스트 double로 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="b3ac4-449">이 섹션에서는 오픈 소스 모의 프레임 워크 Moq를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="b3ac4-450">다음은 Moq를 사용 하 여 직원 리포지토리에 대해 테스트 double을 동적으로 만드는 간단한 예입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="b3ac4-451">IRepository&lt;Employee&gt; 구현에 대해 Moq를 요청 하 고이를 동적으로 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="b3ac4-452">모의&lt;T&gt; 개체의 개체 속성에 액세스 하 여 IRepository&lt;Employee&gt;를 구현 하는 개체를 가져올 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="b3ac4-453">이 개체는 컨트롤러에 전달할 수 있는 내부 개체 이며 테스트 double 또는 실제 리포지토리입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="b3ac4-454">실제 구현을 사용 하 여 개체에서 메서드를 호출 하는 것 처럼 개체에서 메서드를 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="b3ac4-455">Add 메서드를 호출할 때 모의 리포지토리에서 수행할 작업을 알고 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="b3ac4-456">모의 개체 뒤에는 구현이 없으므로 추가는 아무 작업도 수행 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="b3ac4-457">우리가 작성 한 fakes와 마찬가지로 내부적으로는 구체적인 컬렉션이 없으므로 직원은 무시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="b3ac4-458">FindById의 반환 값은 무엇 인가요?</span><span class="sxs-lookup"><span data-stu-id="b3ac4-458">What about the return value of FindById?</span></span> <span data-ttu-id="b3ac4-459">이 경우 모의 개체는 기본값을 반환 하는 유일한 작업을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="b3ac4-460">참조 형식 (직원)을 반환 하기 때문에 반환 값은 null 값입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="b3ac4-461">Mock는 쓸모가 없을 수 있습니다. 그러나 두 가지 이상의 모의 기능이 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="b3ac4-462">먼저 Moq 프레임 워크는 모의 개체에 대해 수행 된 모든 호출을 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="b3ac4-463">코드에서 나중에 다른 사용자가 Add 메서드를 호출 하거나 FindById 메서드를 호출한 경우 Moq를 요청할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="b3ac4-464">테스트에서이 "블랙 박스" 기록 기능을 사용 하는 방법에 대해 나중에 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="b3ac4-465">두 번째 유용한 기능은 Moq를 사용 하 여 *기대*수준에서 모의 개체를 프로그래밍 하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="b3ac4-466">예상은 모의 개체에 지정 된 상호 작용에 응답 하는 방법을 알려 줍니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="b3ac4-467">예를 들어, mock에 기대 하는 사항을 프로그래밍 하 고 누군가가 FindById을 호출할 때 employee 개체를 반환 하도록 지시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="b3ac4-468">Moq 프레임 워크는 설치 API 및 람다 식을 사용 하 여 이러한 기대를 프로그래밍 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

``` csharp
    [TestMethod]
    public void MockSample() {
        Mock<IRepository<Employee>> mock =
            new Mock<IRepository<Employee>>();
        mock.Setup(m => m.FindById(5))
            .Returns(new Employee {Id = 5});
        IRepository<Employee> repository = mock.Object;
        var employee = repository.FindById(5);
        Assert.IsTrue(employee.Id == 5);
    }
```

<span data-ttu-id="b3ac4-469">이 샘플에서는 리포지토리를 동적으로 빌드하기 위해 Moq를 요청 하 고 예상 대로 리포지토리를 프로그래밍 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="b3ac4-470">예상은 사용자가 값 5를 전달 하는 FindById 메서드를 호출할 때 Id 값이 5 인 새 employee 개체를 반환 하도록 모의 개체에 지시 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="b3ac4-471">이 테스트는 통과 하 고&lt;T&gt;가짜 IRepository에 전체 구현을 빌드할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="b3ac4-472">이전에 작성 한 테스트를 다시 방문 하 고 fakes 대신 mock를 사용 하도록 재작업 해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="b3ac4-473">이전과 마찬가지로 기본 클래스를 사용 하 여 모든 컨트롤러의 테스트에 필요한 인프라의 공통 부분을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .AsQueryable();
            _repository = new Mock<IRepository<Employee>>();
            _unitOfWork = new Mock<IUnitOfWork>();
            _unitOfWork.Setup(u => u.Employees)
                       .Returns(_repository.Object);
            _controller = new EmployeeController(_unitOfWork.Object);
        }

        protected IQueryable<Employee> _employeeData;
        protected Mock<IUnitOfWork> _unitOfWork;
        protected EmployeeController _controller;
        protected Mock<IRepository<Employee>> _repository;
    }
```

<span data-ttu-id="b3ac4-474">설치 코드는 거의 동일 하 게 유지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="b3ac4-475">Fakes를 사용 하는 대신 Moq를 사용 하 여 모의 개체를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="b3ac4-476">기본 클래스는 코드가 Employees 속성을 호출할 때 모의 리포지토리를 반환할 모의 작업 단위를 정렬 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="b3ac4-477">모의 설치의 나머지 부분은 각 특정 시나리오에 전용으로 사용 되는 테스트 설비 내에서 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="b3ac4-478">예를 들어 작업에서 모의 리포지토리의 FindAll 메서드를 호출할 때 인덱스 작업에 대 한 테스트 fixture는 모의 리포지토리를 설정 하 여 직원 목록을 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        public EmployeeControllerIndexActionTests() {
            _repository.Setup(r => r.FindAll())
                        .Returns(_employeeData);
        }
        // .. tests
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count());
        }
        // .. and more tests
    }
```

<span data-ttu-id="b3ac4-479">기대를 제외 하 고 테스트는 이전에 수행한 테스트와 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="b3ac4-480">그러나 모의 프레임 워크의 기록 기능을 사용 하면 다른 각도에서 테스트를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="b3ac4-481">다음 섹션에서이 새로운 큐브 뷰를 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="b3ac4-482">상태 및 상호 작용 테스트</span><span class="sxs-lookup"><span data-stu-id="b3ac4-482">State versus Interaction Testing</span></span>

<span data-ttu-id="b3ac4-483">모의 개체로 소프트웨어를 테스트 하는 데 사용할 수 있는 여러 기술이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="b3ac4-484">한 가지 방법은 지금까지이 문서에서 수행한 작업 인 상태 기반 테스트를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="b3ac4-485">상태 기반 테스트는 소프트웨어 상태에 대 한 어설션을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="b3ac4-486">마지막 테스트에서 컨트롤러에 대 한 작업 메서드를 호출 하 고 빌드해야 하는 모델에 대 한 어설션을 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="b3ac4-487">테스트 상태의 몇 가지 다른 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="b3ac4-488">만들기가 실행 된 후 리포지토리에 새 employee 개체가 포함 되어 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="b3ac4-489">인덱스가 실행 된 후 모델에 모든 직원 목록이 포함 되어 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="b3ac4-490">삭제를 실행 한 후 리포지토리에 지정 된 직원이 포함 되지 않았는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="b3ac4-491">모의 개체로 표시 되는 다른 방법은 *상호 작용*을 확인 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="b3ac4-492">상태 기반 테스트는 개체의 상태에 대 한 어설션을 만들며, 상호 작용 기반 테스트는 개체가 상호 작용 하는 방식에 대 한 어설션을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="b3ac4-493">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-493">For example:</span></span>

-   <span data-ttu-id="b3ac4-494">만들기가 실행 될 때 컨트롤러가 리포지토리의 Add 메서드를 호출 하는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="b3ac4-495">인덱스가 실행 될 때 컨트롤러가 리포지토리의 FindAll 메서드를 호출 하는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="b3ac4-496">편집이 실행 될 때 컨트롤러에서 작업의 커밋 메서드 단위를 호출 하 여 변경 내용을 저장 하는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="b3ac4-497">상호 작용 테스트에는 컬렉션 내부에 poking 수를 확인 하지 않으므로 테스트 데이터가 더 적습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="b3ac4-498">예를 들어 세부 작업에서 올바른 값을 사용 하 여 리포지토리의 FindById 메서드를 호출 하는 경우 동작이 올바르게 동작 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="b3ac4-499">FindById에서 반환할 테스트 데이터를 설정 하지 않고이 동작을 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerDetailsActionTests
               : EmployeeControllerTestBase {
         // ...
        [TestMethod]
        public void ShouldInvokeRepositoryToFindEmployee() {
            var result = _controller.Details(_detailsId);
            _repository.Verify(r => r.FindById(_detailsId));
        }
        int _detailsId = 1;
    }
```

<span data-ttu-id="b3ac4-500">위의 테스트 fixture에 필요한 유일한 설정은 기본 클래스에서 제공 하는 설정입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="b3ac4-501">컨트롤러 작업을 호출 하면 Moq가 모의 리포지토리와의 상호 작용을 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="b3ac4-502">Moq의 Verify API를 사용 하 여 컨트롤러에서 FindById를 호출한 경우 적절 한 ID 값으로 Moq를 요청할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="b3ac4-503">컨트롤러에서 메서드를 호출 하지 않았거나 예기치 않은 매개 변수 값을 사용 하 여 메서드를 호출한 경우에는 Verify 메서드에서 예외가 throw 되 고 테스트가 실패 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="b3ac4-504">다음은 만들기 작업에서 현재 작업 단위에 대해 Commit을 호출 하는지 확인 하는 또 다른 예입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="b3ac4-505">상호 작용 테스트의 한 가지 위험은 상호 작용을 지정 하는 추세입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="b3ac4-506">모의 개체의 모든 상호 작용을 기록 하 고 확인 하는 모의 개체의 기능은 테스트가 모든 상호 작용을 확인 하는 것을 의미 하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="b3ac4-507">일부 상호 작용은 구현 세부 정보 이며 현재 테스트를 충족 하는 데 *필요한* 상호 작용을 확인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="b3ac4-508">Mock 또는 fakes 중에서 선택 하는 항목은 주로 테스트 하는 시스템과 개인 (또는 팀) 기본 설정에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="b3ac4-509">모의 개체는 테스트를 두 배로 구현 하는 데 필요한 코드의 양을 크게 줄일 수 있지만, 모든 사람이 편안한 프로그래밍을 기대 하 고 상호 작용을 확인 하는 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="b3ac4-510">결론</span><span class="sxs-lookup"><span data-stu-id="b3ac4-510">Conclusions</span></span>

<span data-ttu-id="b3ac4-511">이 문서에서는 ADO.NET Entity Framework를 사용 하 여 데이터 지 속성에 대해 테스트 가능한 코드를 만드는 몇 가지 방법을 보여 주었습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="b3ac4-512">IObjectSet&lt;T&gt;와 같은 기본 제공 추상화를 활용 하거나 IRepository&lt;T&gt;와 같은 고유한 추상화를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span><span data-ttu-id="b3ac4-513">  두 경우 모두 ADO.NET Entity Framework 4.0의 POCO 지원을 통해 이러한 추상화를 지속적으로 무시 하 고 매우 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-513">  In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="b3ac4-514">암시적 지연 로드와 같은 추가 EF4 기능을 사용 하면 관계형 데이터 저장소의 세부 정보를 걱정 하지 않고 비즈니스 및 응용 프로그램 서비스 코드를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="b3ac4-515">마지막으로, 만든 추상화는 단위 테스트 내에서 모의 이거나 가짜를 쉽게 수행할 수 있으며, 이러한 테스트 double을 사용 하 여 신속 하 게 실행 되 고 매우 격리 되 고 신뢰할 수 있는 테스트를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="b3ac4-516">추가 리소스</span><span class="sxs-lookup"><span data-stu-id="b3ac4-516">Additional Resources</span></span>

-   <span data-ttu-id="b3ac4-517">Robert C. Martin, " [단일 책임 원칙](https://www.objectmentor.com/resources/articles/srp.pdf)"</span><span class="sxs-lookup"><span data-stu-id="b3ac4-517">Robert C. Martin, “ [The Single Responsibility Principle](https://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="b3ac4-518">Martin Fowler, *엔터프라이즈 응용 프로그램 아키텍처 패턴* 의 [패턴 카탈로그](https://www.martinfowler.com/eaaCatalog/index.html)</span><span class="sxs-lookup"><span data-stu-id="b3ac4-518">Martin Fowler, [Catalog of Patterns](https://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="b3ac4-519">Griffin Caprio, " [종속성 주입](https://msdn.microsoft.com/magazine/cc163739.aspx)"</span><span class="sxs-lookup"><span data-stu-id="b3ac4-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="b3ac4-520">데이터 프로그래밍 블로그, " [연습: Entity Framework 4.0를 사용 하는 테스트 기반 개발](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)"</span><span class="sxs-lookup"><span data-stu-id="b3ac4-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="b3ac4-521">데이터 프로그래밍 블로그, " [Entity Framework 4.0에서 리포지토리 및 작업 단위 패턴 사용](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"</span><span class="sxs-lookup"><span data-stu-id="b3ac4-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="b3ac4-522">Aaron Jensen, " [컴퓨터 사양 소개](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"</span><span class="sxs-lookup"><span data-stu-id="b3ac4-522">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="b3ac4-523">Eric Lee, " [MSTest를 사용](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)하는 BDD"</span><span class="sxs-lookup"><span data-stu-id="b3ac4-523">Eric Lee, “ [BDD with MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="b3ac4-524">Eric Evans, " [도메인 기반 디자인](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"</span><span class="sxs-lookup"><span data-stu-id="b3ac4-524">Eric Evans, “ [Domain Driven Design](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="b3ac4-525">Martin Fowler, " [모의는 스텁이 아닙니다](https://martinfowler.com/articles/mocksArentStubs.html)."</span><span class="sxs-lookup"><span data-stu-id="b3ac4-525">Martin Fowler, “ [Mocks Aren’t Stubs](https://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="b3ac4-526">Martin Fowler, " [Test Double](https://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="b3ac4-526">Martin Fowler, “ [Test Double](https://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   [<span data-ttu-id="b3ac4-527">Moq</span><span class="sxs-lookup"><span data-stu-id="b3ac4-527">Moq</span></span>](https://code.google.com/p/moq/)

### <a name="biography"></a><span data-ttu-id="b3ac4-528">소개</span><span class="sxs-lookup"><span data-stu-id="b3ac4-528">Biography</span></span>

<span data-ttu-id="b3ac4-529">Scott Allen는 Pluralsight 및 창립자 OdeToCode.com의 기술 직원의 멤버입니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-529">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="b3ac4-530">상업적 소프트웨어 개발의 15 년 동안 Scott은 8 비트 embedded 장치에서 확장성이 뛰어난 ASP.NET 웹 응용 프로그램을 위한 솔루션에 대해 노력 했습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-530">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="b3ac4-531">OdeToCode의 블로그 또는 [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode)에서 Scott에 게 연락할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b3ac4-531">You can reach Scott on his blog at OdeToCode, or on Twitter at [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode).</span></span>
