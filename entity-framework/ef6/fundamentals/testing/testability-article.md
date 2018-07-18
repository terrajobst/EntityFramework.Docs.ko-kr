---
title: 테스트 용이성 및 Entity Framework 4.0
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
caps.latest.revision: 3
ms.openlocfilehash: 61773f8a23ff54ddb78aeeb5656c669b12f91478
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122370"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="2b575-102">테스트 용이성 및 Entity Framework 4.0</span><span class="sxs-lookup"><span data-stu-id="2b575-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="2b575-103">Scott Allen</span><span class="sxs-lookup"><span data-stu-id="2b575-103">Scott Allen</span></span>

<span data-ttu-id="2b575-104">게시일: 2010 년 5 월</span><span class="sxs-lookup"><span data-stu-id="2b575-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="2b575-105">소개</span><span class="sxs-lookup"><span data-stu-id="2b575-105">Introduction</span></span>

<span data-ttu-id="2b575-106">이 백서에서는 설명 하 고 ADO.NET Entity Framework 4.0 및 Visual Studio 2010을 사용 하 여 테스트 가능한 코드를 작성 하는 방법에 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="2b575-107">이 문서에서는 테스트 기반 디자인 (TDD) 또는 동작 기반 디자인 (BDD)와 같은 특정 테스트 방법론을 중점적으로 시도 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="2b575-108">대신이 문서는 방법을 중점적으로 ADO.NET Entity Framework를 사용 하 여 아직 격리 하 고 자동화 된 방식으로 테스트를 쉽게 유지 하는 코드를 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="2b575-109">데이터에서 액세스 시나리오를 테스트 프레임 워크를 사용 하는 경우 이러한 패턴을 적용 하는 방법을 참조 하는 일반적인 디자인 패턴에서 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="2b575-110">테스트 가능한 코드에서 이러한 기능 수 어떻게 표시 되는지 확인 하기 위해 프레임 워크의 특정 기능에도 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="2b575-111">테스트 가능한 코드 란?</span><span class="sxs-lookup"><span data-stu-id="2b575-111">What Is Testable Code?</span></span>

<span data-ttu-id="2b575-112">자동화 된 단위 테스트를 사용 하 여 소프트웨어를 확인 하는 기능에는 많은 바람직하지 이점을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="2b575-113">모든 사용자는 좋은 테스트는 응용 프로그램 및 응용 프로그램의 품질을 위치에 단위 테스트를 포함 하지만 넘어서 버그를 발견 하는 바로 증가 소프트웨어 결함 수가 감소할 것을 알고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="2b575-114">훌륭한 단위 테스트 도구 모음 개발 팀을 시간을 절약 하 여 자신이 만든 소프트웨어의 컨트롤에 남아 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="2b575-115">팀을 기존 코드를 리팩터링 재설계 변경할 수 있으며 재구성 소프트웨어 테스트 도구 모음을 파악 하는 동안 모든 응용 프로그램에 새 구성 요소를 추가한 새로운 요구 사항을 충족 하는 응용 프로그램의 동작을 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="2b575-116">단위 테스트는 일부 변경 용이 하 게 소프트웨어의 복잡성 증가 함에 따라 유지 관리 용이성을 유지 하는 빠른 피드백 주기입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="2b575-117">하지만 단위 테스트는 가격에 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="2b575-118">팀에서 만들고 단위 테스트를 유지 관리 하는 시간을 투자 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="2b575-119">이러한 테스트를 만드는 데 필요한 작업량에 직접 연관 됩니다 합니다 **테스트 용이성** 기본 소프트웨어입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="2b575-120">소프트웨어 테스트를 얼마나 기능</span><span class="sxs-lookup"><span data-stu-id="2b575-120">How easy is the software to test?</span></span> <span data-ttu-id="2b575-121">염두에서 테스트 용이성을 사용 하 여 소프트웨어를 설계 팀이 만들어집니다 효율적인 테스트 되지 않은 테스트 가능 소프트웨어를 사용 하 여 작업 팀 보다 빠릅니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="2b575-122">테스트 용이성 염두에서를 사용 하 여 ADO.NET Entity Framework 4.0 (EF4) 설계 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="2b575-123">개발자를 작성 하는 단위 테스트 프레임 워크 코드 자체에 대 한 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="2b575-124">대신 EF4의 테스트 용이성 목표 쉽게 프레임 워크를 기반으로 구축 하는 테스트 가능한 코드를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="2b575-125">특정 예제를 살펴보기 전에 테스트 가능한 코드의 품질을 이해 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="2b575-126">테스트 가능한 코드의 품질을</span><span class="sxs-lookup"><span data-stu-id="2b575-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="2b575-127">테스트할 쉬운 코드를 항상 두 개 이상의 특성을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="2b575-128">첫째, 테스트 코드는 쉽게 **관찰**합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="2b575-129">일부 입력 집합을 지정 하는, 쉽게 코드의 출력을 알 수 여야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="2b575-130">예를 들어 다음 메서드를 테스트 이므로 쉽게 메서드는 직접 계산의 결과 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="2b575-131">메서드를 테스트 하는 것은 메서드는 네트워크 소켓, 데이터베이스 테이블에 또는 다음 코드와 같이 파일에 계산 된 값을 기록 하는 경우 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="2b575-132">테스트 값을 검색 하려면 추가 작업을 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="2b575-133">둘째, 테스트 가능한 코드 하기 쉽습니다 **격리**합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="2b575-134">테스트 가능한 코드의 잘못 된 예로 다음 의사 코드를 사용해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

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

<span data-ttu-id="2b575-135">메서드는 쉽게 알 수 – 보험 정책에서 전달 하 고 반환 값에 예상된 결과 일치 하는지 확인 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="2b575-136">그러나 메서드를 테스트 하 해야 올바른 스키마를 사용 하 여 설치 되어 있고 메서드 전자 메일을 보내려고 시도 하는 경우 SMTP 서버를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="2b575-137">단위 테스트는만 메서드 내부에서 계산 논리를 확인 하려고 하지만 테스트 전자 메일 서버가 오프 라인 상태 이므로 있거나 데이터베이스 서버 이동 때문에 실패할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="2b575-138">이러한 오류의 모두 테스트 확인 하려는 동작에 관련 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="2b575-139">동작을 격리 하기 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="2b575-140">자주 테스트 가능한 코드를 작성 하기 위해 노력 하는 소프트웨어 개발자는 작성 코드의 중요 한 부분의 분리를 유지 하기 위해 노력 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="2b575-141">위의 메서드는 비즈니스 계산에 집중 하 고 다른 구성 요소에는 데이터베이스 및 메일 구현 세부 정보를 위임 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="2b575-142">단일 책임 원칙 Robert C. Martin이를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="2b575-143">개체 값을 계산한 다음 정책의 같은 단일, 좁은 책임을 캡슐화 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="2b575-144">다른 모든 데이터베이스 및 알림 작업에는 몇 가지 다른 개체의 책임 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="2b575-145">이 방식으로 작성 된 코드 한 작업에 중점을 둡니다 때문에 격리 하기 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="2b575-146">.NET에서 단일 책임 원칙을 따르고 격리 해야 하는 추상화 했습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="2b575-147">인터페이스 정의 사용 하 고 구체적인 형식 대신 인터페이스 추상화를 사용 하는 코드를 강제로 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="2b575-148">이 문서의 뒷부분에서 살펴보겠습니다 위에서 설명한 잘못 된 예제에서는 작업을 수행할 수와 같은 메서드는 인터페이스 하는 방법을 *찾습니다* 데이터베이스로 통신할 등.</span><span class="sxs-lookup"><span data-stu-id="2b575-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="2b575-149">그러나 테스트 시간에는 데이터베이스와 통신 하지 않습니다 하지만 대신 메모리에 데이터를 보유 하는 더미 구현을 대체할 수 했습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="2b575-150">이 더미 구현인 데이터 액세스 코드 또는 데이터베이스 구성 관련된 문제 로부터 코드를 격리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="2b575-151">격리로 추가 이점이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="2b575-152">마지막 메서드에서 비즈니스 계산을 실행 하려면 몇 밀리초가 소요만 해야 하지만 다양 한 서버에 자체 테스트 네트워크 및 통신 관련 코드 홉으로 몇 초 동안 실행 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="2b575-153">단위 테스트는 신속 하 게 한 작은 변경 사항은 용이 하 게 실행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="2b575-154">단위 테스트는 또한 반복을 가능 하 고 테스트와 관련 없는 구성 요소에 문제가 발생 하므로 실패 하지 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="2b575-155">관찰을 격리 하기 쉬운 코드를 개발자가 더 쉽게 테스트 코드를 작성 의미를 실행 하려면 테스트에 대 한 대기 시간을 줄이고 쓰고, 더 중요 한 점은 시간이 절약 존재 하지 않는 버그를 추적 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="2b575-156">바랍니다 테스트의 이점을 이해 하 고 테스트 가능한 코드를 보여 주는 품질을 이해할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="2b575-157">Observable 및 격리를 쉽게 유지 하면서 데이터베이스에 데이터를 저장 하는 EF4 작동 하는 코드를 작성 하는 방법에 있지만 먼저 데이터 액세스를 위한 테스트 가능 설계를 설명 합니다. 이번 좁힐 수 했습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="2b575-158">데이터 지 속성에 대 한 디자인 패턴</span><span class="sxs-lookup"><span data-stu-id="2b575-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="2b575-159">앞서 소개한 잘못 된 예제 모두에 너무 많은 책임을 담당 했습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="2b575-160">잘못 된 첫 번째 예제에서는 계산을 수행 해야 *및* 파일로 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="2b575-161">잘못 된 두 번째 예제 데이터베이스에서 데이터를 읽을 수 있었습니다 *하 고* 비즈니스 계산을 수행 *및* 전자 메일을 보냅니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="2b575-162">문제를 분리 하 고 다른 구성 요소에 대 한 책임을 위임 하는 작은 메서드를 디자인 하 여 테스트 가능한 코드 작성으로 멀었다고 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="2b575-163">목표 작고 집중 된 추상화에서 작업을 작성 하 여 기능을 구축 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="2b575-164">작은 데이터 지 속성을 제공 하 고 살펴봅니다 포커스가 있는 추상화가 매우 일반적 이므로 디자인 패턴으로 문서화 된 했습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="2b575-165">Martin Fowler의 책 엔터프라이즈 응용 프로그램 아키텍처 패턴에는 인쇄에서 이러한 패턴을 설명 하는 첫 번째 작업 이었습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="2b575-166">이러한 ADO.NET Entity Framework 구현 하 고 이러한 패턴을 사용 하 여 작동 하는 방법을 보여 줍니다 전에 다음 섹션에서 이러한 패턴의 한 간단한 설명을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="2b575-167">리포지토리 패턴</span><span class="sxs-lookup"><span data-stu-id="2b575-167">The Repository Pattern</span></span>

<span data-ttu-id="2b575-168">Fowler는 리포지토리 "간을 중재 도메인과 데이터 도메인 개체에 액세스 하기 위한 컬렉션 유형의 인터페이스를 사용 하 여 매핑 계층"을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="2b575-169">리포지토리 패턴의 목적은 데이터 액세스 러에서 코드를 격리 하는 것 이며 이전 격리 살펴본 것 처럼 테스트 용이성에 대 한 필수 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="2b575-170">격리 됨을 키는 저장소 컬렉션 유형의 인터페이스를 사용 하 여 개체를 노출 하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="2b575-171">저장소에 있는 모릅니다 방법을 사용 하 여 쓸 논리 저장소 요청 개체를 구체화 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="2b575-172">리포지토리 데이터베이스와 통신할 수 있습니다 또는 개체를 메모리 내 컬렉션에서만 반환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="2b575-173">코드 알아야 할 모든는 컬렉션을 유지 하기 위해 표시 되는 저장소 및 검색, 추가 및 컬렉션에서 개체를 삭제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="2b575-174">기존.NET 응용 프로그램의 구체적인 리포지토리 종종 다음과 같은 일반 인터페이스에서 상속합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="2b575-175">EF4에 대 한 구현을 제공 하지만 기본 개념 동일 하 게 유지 하는 경우 인터페이스 정의에 몇 가지 변경을 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="2b575-176">코드는이 인터페이스를 구현 하는 구체적인 리포지토리를 사용 하 여 해당 기본 키 값이 조건자의 평가에 따라 엔터티 컬렉션을 검색 하 여 엔터티를 검색 하 하거나 간단히 모든 사용 가능한 엔터티를 검색할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="2b575-177">또한 코드를 추가 하 고 리포지토리 인터페이스를 통해 엔터티를 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="2b575-178">코드는 IRepository의 Employee 개체를 매개 변수로 받아 다음 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="2b575-179">코드는 인터페이스 (IRepository의 직원)를 사용 하므로 인터페이스의 다른 구현을 사용 하 여 코드를 제공할 수 있습니다 했습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="2b575-180">하나의 구현에는 Microsoft SQL Server 데이터베이스로 EF4 및 지속 개체에서 지 원하는 구현을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="2b575-181">메모리 내 직원의 목록 개체로 (테스트 중 사용 하 여 하나) 다른 구현을 백업할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="2b575-182">인터페이스는 코드에서 격리 하는 데 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="2b575-183">IRepository를 확인할 수 있습니다&lt;T&gt; 인터페이스 저장 작업을 노출 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="2b575-184">기존 개체를 업데이트 하려면 어떻게 해야 했습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-184">How do we update existing objects?</span></span> <span data-ttu-id="2b575-185">저장 작업을 수행할는 IRepository 정의 발견할 수 있습니다 하 고 이러한 리포지토리 구현의 즉시 데이터베이스에 개체를 유지 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="2b575-186">그러나 많은 응용 프로그램에서 개체를 개별적으로 유지 하려고 하지 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="2b575-187">대신 개체에 활기를 아마도 다른 리포지토리에서 비즈니스 활동의 일부로 개체만 수정 하 고 다음 단일 원자성 작업의 일환으로 모든 개체를 유지 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="2b575-188">다행 스럽게도 이러한 유형의 동작을 허용 하는 패턴입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="2b575-189">작업 패턴 단위</span><span class="sxs-lookup"><span data-stu-id="2b575-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="2b575-190">Fowler 작업 단위는 "및 유지 관리 개체의 목록을 비즈니스 트랜잭션에 의해 영향을 받는 변경 내용에서 작성 하 고 동시성 문제 조정" 이라고 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="2b575-191">이 경우 리포지토리에서 활용 하 고 변경한 개체 변경 내용을 커밋 하도록 작업 단위의 말하는 경우 변경 내용을 유지할 개체 변경 내용을 추적 하려면 작업의 단위입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="2b575-192">모든 리포지토리를 추가 하 고 데이터베이스 및도 관리 삭제에는 개체를 삽입할 새 개체를 작업의 단위 책임 이기도 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="2b575-193">ADO.NET 데이터 집합을 사용 하 여 모든 작업 경험이 있는 경우 다음 이미 수 작업 패턴 단위를 사용 하 여 친숙 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="2b575-194">ADO.NET 데이터 집합은 업데이트, 삭제 및 삽입 DataRow 개체를 추적할 수 있어 수 (사용 하 여 TableAdapter) 조정 모든 변경 내용이 데이터베이스에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="2b575-195">그러나 데이터 집합 개체는 기본 데이터베이스의 연결이 끊긴된 하위 집합을 모델링합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="2b575-196">작업 패턴 단위에는 비즈니스 개체 및 데이터 액세스 코드에서 격리 되 고 데이터베이스를 인식 하지 않고 도메인 개체를 사용 하 여 작동 하지만 동일한 동작을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="2b575-197">.NET 코드에서 작업 단위를 모델링 하는 추상화는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="2b575-198">단일 작업 단위를 확인할 수 있습니다 작업 단위에서 리포지토리에 대 한 참조를 노출 하 여 개체에 비즈니스 트랜잭션 중 구체화 하는 모든 엔터티를 추적할 수가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="2b575-199">커밋 메서드의 실제 작업 단위에 대 한 구현은 모든 마법이 일어나는 곳 데이터베이스를 사용 하 여 메모리 내 변경을 조정 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="2b575-200">코드 수는 IUnitOfWork 참조를 지정 하는, 하나 이상의 저장소에서 검색 하는 비즈니스 개체를 변경할 및 원자성 커밋 작업을 사용 하 여 모든 변경 내용을 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="2b575-201">지연 로드 패턴</span><span class="sxs-lookup"><span data-stu-id="2b575-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="2b575-202">Fowler 이름 지연 로드를 사용 하 여 "의 모든 데이터를 포함 하지 않는 개체 필요 하지만 되도록 하는 방법을 알고" 설명.</span><span class="sxs-lookup"><span data-stu-id="2b575-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="2b575-203">투명 한 지연 로드 했을 때에 중요 한 기능은 테스트 가능한 비즈니스 코드를 작성 하 고 관계형 데이터베이스를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="2b575-204">예를 들어 다음 코드를 살펴보세요.</span><span class="sxs-lookup"><span data-stu-id="2b575-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="2b575-205">시간표 컬렉션 채워지는 방법</span><span class="sxs-lookup"><span data-stu-id="2b575-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="2b575-206">두 가지 이유가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-206">There are two possible answers.</span></span> <span data-ttu-id="2b575-207">다음 중 하나는 직원을 인출 하는 메시지가 표시 되 면 직원 리포지토리 직원의 연결된 시간 카드 정보와 함께 직원을 검색할 쿼리를 발급 하는 경우</span><span class="sxs-lookup"><span data-stu-id="2b575-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="2b575-208">관계형 데이터베이스가 일반적으로 JOIN 절을 사용 하 여 쿼리가 필요 하 고 발생할 수는 응용 프로그램 보다 자세한 정보를 검색 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="2b575-209">그렇다면 응용 프로그램이 필요 하지 않습니다 시간표 속성을 연결할?</span><span class="sxs-lookup"><span data-stu-id="2b575-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="2b575-210">두 번째 답변 "주문형" 시간표 속성을 로드 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="2b575-211">이 지연 로드 이므로 암시적 및 비즈니스 논리를 투명 하 게 코드 시간 카드 정보를 검색 하는 특수 한 Api를 호출 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="2b575-212">코드 시간 카드 정보는 필요한 경우를 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="2b575-213">일반적으로 메서드 호출의 런타임 인터 셉 션을 포함 하는 지연 로드를 사용 하 여 관련 된 몇 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="2b575-214">코드를 가로채는 데이터베이스와 통신 및 비즈니스 논리가 비즈니스 논리를 사용 가능한 상태로 두고 시간 카드 정보를 검색 하는 일을 담당 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="2b575-215">이 지연 로드 매직 자세한 테스트 가능한 코드의 결과 및 데이터 검색 작업에서 자체를 격리 하는 비즈니스 코드를 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="2b575-216">지연 로드 단점은 하는 경우 응용 프로그램 *않습니다* 코드는 추가 쿼리 실행 시간 카드 정보가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="2b575-217">성능이 중요 한 응용 프로그램 또는 응용 프로그램 수의 직원 개체를 반복 실행 하 고 쿼리 실행 시간 카드 (문제를 N + 1이 라 불리는 루프의 각 반복 과정에서 검색할 하지만 대부분의 응용 프로그램에 대 한 중요 한 것은 아닙니다. 쿼리 문제) 지연 로딩은 끌기.</span><span class="sxs-lookup"><span data-stu-id="2b575-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="2b575-218">이러한 시나리오에서 응용 프로그램 즉시 가능한 가장 효율적인 방식으로 시간 카드 정보를 로드 하려고 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="2b575-219">다행 스럽게도 살펴보겠습니다 EF4 모두 암시적 지연 로드를 지 원하는 방법과 효율적인 즉시 로드 다음 섹션으로 이동 하 고 이러한 패턴을 구현 했습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="2b575-220">Entity Framework 사용 하 여 패턴 구현</span><span class="sxs-lookup"><span data-stu-id="2b575-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="2b575-221">좋은 소식은 모든 마지막 섹션에서 설명 했 듯이 디자인 패턴은 EF4를 사용 하 여 구현 하기가 단순 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="2b575-222">편집 하 여 직원 및 해당 관련된 시간 카드 정보를 표시 하는 간단한 ASP.NET MVC 응용 프로그램을 사용 하려고 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="2b575-223">다음 "plain old CLR object"를 사용 하 여 먼저 (Poco).</span><span class="sxs-lookup"><span data-stu-id="2b575-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

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

<span data-ttu-id="2b575-224">이러한 클래스 정의 다른 방법과 EF4의 기능에 살펴봅니다 하지만 지 속성 무시 (PI) 최대한으로 이러한 클래스를 유지 하기 위함입니다 약간 변경 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="2b575-225">PI를 알지 *어떻게*, 심지어 *경우*, 데이터베이스 내에서 거주 하는 경우 상태입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="2b575-226">PI 및 Poco 테스트 가능 소프트웨어를 사용 하 여 작업과 함께 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="2b575-227">POCO 접근 방식을 사용 하는 개체에는 덜 제한적, 유연 하 고 있는 데이터베이스 없이 작동 하므로 테스트를 쉽게는입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="2b575-228">현재 위치에서 Poco를 사용 하 여 Visual Studio에서 엔터티 데이터 모델 (EDM) 만들 수 있습니다 (그림 1 참조).</span><span class="sxs-lookup"><span data-stu-id="2b575-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="2b575-229">EDM이 엔터티에 대 한 코드 생성을 사용 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="2b575-230">대신, 우리 답게 만드는 손으로 엔터티를 사용 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="2b575-231">데이터베이스 스키마를 생성 하 고 EF4 데이터베이스로 개체를 매핑하는 데 필요한 메타 데이터를 제공 하는 EDM만 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![eftest_01](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="2b575-233">**그림 1**</span><span class="sxs-lookup"><span data-stu-id="2b575-233">**Figure 1**</span></span>

<span data-ttu-id="2b575-234">참고: 먼저 EDM 모델을 개발 하려는 경우 있기을 깨끗 하 고 EDM에서 POCO 코드를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="2b575-235">데이터 프로그래밍 팀에서 제공 하는 Visual Studio 2010 확장을 사용 하 여이 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="2b575-236">확장을 다운로드 하려면 Visual Studio의 도구 메뉴에서 확장 관리자를 시작 하 고 "POCO" (그림 2 참조)에 대 한 온라인 템플릿 갤러리를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="2b575-237">EF에 대 한 사용 가능한 여러 POCO 템플릿이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="2b575-238">템플릿 사용에 대 한 자세한 내용은 참조 하세요. " [연습: Entity Framework에 대 한 POCO 템플릿을](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)"입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![eftest_02](~/ef6/media/eftest-02.png)

<span data-ttu-id="2b575-240">**그림 2**</span><span class="sxs-lookup"><span data-stu-id="2b575-240">**Figure 2**</span></span>

<span data-ttu-id="2b575-241">시작 점으로이 POCO에서 테스트 가능한 코드에 두 가지 방법을 살펴봅니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="2b575-242">첫 번째 방법은 호출 EF 접근 방식을 추상화 리포지토리 및 작업 단위를 구현 하는 엔터티 프레임 워크 API를 활용 하므로 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="2b575-243">두 번째 접근 방식에서에서는 고유한 사용자 지정 저장소 추상화를 만들고 장점과 각 접근 방식의 단점 중 하나를 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="2b575-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="2b575-244">EF 방법을 탐색 하 여 시작 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="2b575-245">EF 중심 구현</span><span class="sxs-lookup"><span data-stu-id="2b575-245">An EF Centric Implementation</span></span>

<span data-ttu-id="2b575-246">ASP.NET MVC 프로젝트에서 다음 컨트롤러 작업을 고려해 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="2b575-247">Employee 개체를 검색 하 고 직원의 자세한 뷰를 표시 하려면 결과 반환 하는 작업입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="2b575-248">코드를 테스트할 수는?</span><span class="sxs-lookup"><span data-stu-id="2b575-248">Is the code testable?</span></span> <span data-ttu-id="2b575-249">두 개 이상의 테스트가 방법으로 작업의 동작을 확인 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="2b575-250">먼저 작업 반환 올바른 뷰가 – 테스트 하는 쉬운 확인 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="2b575-251">에서는 올바른 직원을 검색 하는 작업 및 데이터베이스를 쿼리 하는 코드를 실행 하지 않고이 작업을 수행 하고자 확인 하려면 테스트를 작성 하려고도 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="2b575-252">테스트 중인 코드 격리 하려고 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="2b575-253">격리는 데이터 액세스 코드 또는 데이터베이스 구성의 버그 때문에 테스트 실패 하지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="2b575-254">테스트에 실패할 경우 일부 하위 수준 시스템 구성 요소가 아니라 컨트롤러 논리에 버그가 있을 것을 알 수 했습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="2b575-255">격리를 달성 하기 위해 해야 인터페이스를 제공 하는 것과 같은 몇 가지 추상화 이전 리포지토리 및 작업 단위에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="2b575-256">리포지토리 패턴은 도메인 개체 및 데이터 매핑 계층 간을 하도록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="2b575-257">이 시나리오 EF4 *는* 매핑 데이터 계층 및 이미 i o b j 라는 리포지토리 유사 추상화를 제공&lt;T&gt; (System.Data.Objects 네임 스페이스)에서.</span><span class="sxs-lookup"><span data-stu-id="2b575-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="2b575-258">인터페이스 정의 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-258">The interface definition looks like the following.</span></span>

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

<span data-ttu-id="2b575-259">I o b j&lt;T&gt; 개체의 컬렉션 유사 하므로 리포지토리에 대 한 요구 사항을 충족 (IEnumerable을 통해&lt;T&gt;) 추가 하 여 시뮬레이션 된 컬렉션에서 개체를 제거 하는 메서드를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="2b575-260">Attach 및 Detach 메서드 EF4 API의 추가 기능을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="2b575-261">I o b j를 사용 하도록&lt;T&gt; 리포지토리에 대 한 인터페이스와 함께 리포지토리를 바인딩할 단위 작업 추상화를 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="2b575-262">이 인터페이스의 구체적 구현 하나에서는 SQL Server에 설명 하 고 쉽게 EF4 ObjectContext 클래스를 사용 하 여 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="2b575-263">ObjectContext 클래스는 실제 단위 EF4 API의 작업입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

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

<span data-ttu-id="2b575-264">가져오기는 i o b j&lt;T&gt; 수명을 ObjectContext 개체의 CreateObjectSet 메서드를 호출 하는 것 만큼 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="2b575-265">백그라운드에서 프레임 워크 메타 데이터를 사용할지 구체적인 ObjectSet를 생성 하는 EDM에 제공 했습니다&lt;T&gt;합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="2b575-266">사용 하 여 반환 된 i o b j 하겠습니다&lt;T&gt; 인터페이스도 클라이언트 코드의 테스트 용이성을 유지 하도록 도와줍니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="2b575-267">구체적인 구현은 프로덕션 환경에서는 유용 하지만 IUnitOfWork 추상화는 테스트를 용이 하 게 사용 하는 방법에 집중 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="2b575-268">Test Double</span><span class="sxs-lookup"><span data-stu-id="2b575-268">The Test Doubles</span></span>

<span data-ttu-id="2b575-269">컨트롤러 작업을 격리 하려면 실제 작업 단위를 (ObjectContext에서 지원 됨)와 테스트 double 또는 "가짜" 작업 단위를 (메모리 내 작업 수행) 간에 전환 하는 기능이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="2b575-270">이 유형의 전환을 수행 하는 일반적인 방법은 MVC 컨트롤러 작업의 단위를 생성자 매개 변수로 컨트롤러 작업 단위를 전달 대신 인스턴스화할 수 없습니다 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="2b575-271">위의 코드는 종속성 주입의 예입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="2b575-272">컨트롤러 (작업 단위) 종속성의 만들지만 컨트롤러에 종속성 주입에 허용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="2b575-273">MVC 프로젝트에서 컨트롤 (IoC) 컨테이너의 반전 개념을 사용 하 여 조합 하 여 사용자 지정 컨트롤러 팩터리를 사용 하 여 종속성 주입을 자동화할에 공통적으로 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="2b575-274">이러한 항목은이 문서의 범위를 벗어납니다 있지만이 문서의 끝에 있는 참조에 다음에서 더 알아볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="2b575-275">가짜 단위 테스트를 위해 사용할 수 있는 작업 구현의 다음과 같이 보일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

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

<span data-ttu-id="2b575-276">커밋된 속성을 노출 하는 가짜 작업 단위를 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="2b575-277">경우에 따라 기능 테스트를 용이 하 게 하는 가짜 클래스에 추가 하는 것이 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="2b575-278">이 경우 코드 커밋 속성을 확인 하 여 하나의 작업 단위로 커밋됩니다 관찰 하기 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="2b575-279">가짜 i o b j도 필요&lt;T&gt; 직원과 TimeCard 개체를 메모리에 유지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="2b575-280">에서는 제네릭을 사용 하는 단일 구현을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-280">We can provide a single implementation using generics.</span></span>

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

<span data-ttu-id="2b575-281">이 test double 대리자를 기본 HashSet 작업 대부분이&lt;T&gt; 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="2b575-282">참고 해당 i o b j&lt;T&gt; 클래스 (참조 형식)으로 T를 적용 하는 제네릭 제약 조건이 필요 하 고 또한 IQueryable을 구현 하도록 강제로&lt;T&gt;합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="2b575-283">IQueryable로 표시 하는 메모리 내 컬렉션을 확인 하기 쉽습니다&lt;T&gt; AsQueryable 표준 LINQ 연산자를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="2b575-284">테스트</span><span class="sxs-lookup"><span data-stu-id="2b575-284">The Tests</span></span>

<span data-ttu-id="2b575-285">기존 단위 테스트는 단일 테스트 클래스를 사용 하 여 단일 MVC 컨트롤러에서 모든 작업에 대 한 테스트를 모두 유지 하기 위해 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="2b575-286">작성할 수 있습니다 이러한 테스트 또는 단위 테스트, 형식의 fakes에서 메모리를 사용 하 여 빌드 했습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="2b575-287">그러나 피해 야 할 것이 문서에 대 한 모놀리식 응용 프로그램 테스트 클래스 접근 방식 및 대신 그룹 기능의 특정 부분에 초점을 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span>  <span data-ttu-id="2b575-288">"새 직원 만들기 합니다" 예를 들어 단일 테스트 클래스를 사용 하 여 새 직원을 담당 단일 컨트롤러 동작을 확인 하므로 테스트 하고자 하는 기능을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-288">For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="2b575-289">이러한 모든 미세 조정 된 테스트 클래스에 필요한 일반적인 설치 코드가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="2b575-290">예를 들어 항상 생성 해야 메모리 내 리포지토리 및 가짜 작업 단위입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="2b575-291">또한 직원 컨트롤러 인스턴스에 가짜 삽입 하는 작업 단위를 사용 하 여 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="2b575-292">기본 클래스를 사용 하 여이 일반적인 설치 코드 테스트 클래스에서 공유 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-292">We’ll share this common setup code across test classes by using a base class.</span></span>

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

<span data-ttu-id="2b575-293">기본 클래스에서 사용 하 여 "개체 어머니"은 테스트 데이터 생성에 대 한 하나의 일반적인 패턴입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="2b575-294">개체 어머니는 여러 테스트 픽스 쳐에서 사용할 테스트 엔터티를 인스턴스화할 팩터리 메서드를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

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

<span data-ttu-id="2b575-295">테스트 픽스 쳐 (그림 3 참조)의 수에 대 한 기본 클래스로 EmployeeControllerTestBase 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="2b575-296">각 테스트 픽스 쳐를 특정 컨트롤러 작업을 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="2b575-297">예를 들어, 하나의 테스트 픽스 쳐는 테스트 (직원을 만들기 위한 뷰를 표시)를 HTTP GET 요청 중 사용 되는 만들기 작업을 집중 하 고 다양 한 fixture HTTP POST 요청에 사용 되는 만들기 작업을 중점적 (제출한 정보에 직원 만들기 사용자)입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="2b575-298">각 파생된 클래스는 특정 상황에서 특정 테스트 상황에 대 한 결과 확인 하는 데 필요한 어설션을 제공 하는 데 필요한 설치만 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![eftest_03](~/ef6/media/eftest-03.png)

<span data-ttu-id="2b575-300">**그림 3**</span><span class="sxs-lookup"><span data-stu-id="2b575-300">**Figure 3**</span></span>

<span data-ttu-id="2b575-301">여기에 제시 된 명명 규칙 및 테스트 스타일에는 테스트 가능한 코드에 대 한 필요 하지 않습니다. 즉,이 방법 중 하나일 뿐입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="2b575-302">그림 4에서는 Jet 수렵과 Resharper에서 실행 중인 테스트는 runner 플러그 인 Visual Studio 2010에 대 한 테스트 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![eftest_04](~/ef6/media/eftest-04.png)

<span data-ttu-id="2b575-304">**그림 4**</span><span class="sxs-lookup"><span data-stu-id="2b575-304">**Figure 4**</span></span>

<span data-ttu-id="2b575-305">공유 설치 코드를 처리 하는 기본 클래스를 사용 하 여 각 컨트롤러 작업에 대 한 단위 테스트는 작고 쉽게 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="2b575-306">테스트를 신속 하 게 (것은 수행 하므로 메모리 내 작업)를 실행 및 (단위 테스트 격리 한 것) 때문에 관련 되지 않은 인프라 또는 환경 문제 때문에 실패 하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

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

<span data-ttu-id="2b575-307">이러한 테스트의 기본 클래스는 대부분의 설치 작업을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="2b575-308">기본 클래스 생성자 만들고 메모리 내 리포지토리, 작업, 가짜 단위 EmployeeController 클래스의 인스턴스를 기억 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="2b575-309">테스트 클래스에는이 기본 클래스에서 파생 되며 Create 메서드를 테스트의 세부 정보에 중점을 둡니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="2b575-310">이 경우 구체적인 하느냐, 프로시저를 테스트 하는 모든 단위가 표시 "정렬, assert 및 역할 을" 단계:</span><span class="sxs-lookup"><span data-stu-id="2b575-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="2b575-311">들어오는 데이터를 시뮬레이션 하기 위해 newEmployee 개체를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="2b575-312">EmployeeController 만들기 작업을 호출 하 고는 newEmployee 전달 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="2b575-313">만들기 작업 생성 (리포지토리에서 직원 표시 됨) 예상된 결과 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="2b575-314">빌드한 새로운 EmployeeController 작업 중 하나를 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="2b575-315">예를 들어 직원 컨트롤러의 인덱스 작업에 대 한 테스트 작성 것과 동일한 테스트에 대 한 기본 설정을 설정 하는 테스트 기본 클래스에서 상속할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="2b575-316">다시 기본 클래스 메모리 내 리포지토리, 작업, 가짜 단위 이며는 EmployeeController 인스턴스에 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="2b575-317">인덱스 작업에 대 한 테스트 인덱스 동작 호출에 집중 해야 하 고 반환 작업 모델의 품질을 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

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

<span data-ttu-id="2b575-318">메모리에서 fakes를 사용 하 여 만든 테스트는 테스트를 *상태* 소프트웨어입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="2b575-319">예를 들어, create 작업 실행 – 후 저장소의 상태를 검사 하려는 만들기 동작을 테스트 하는 경우 저장소 유지 않으면 새 직원?</span><span class="sxs-lookup"><span data-stu-id="2b575-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="2b575-320">나중에 상호 작용 테스트 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="2b575-321">상호 작용 테스트 테스트 중인 코드가 개체의 적절 한 메서드를 호출 하 고 올바른 매개 변수를 전달 하는 메시지가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="2b575-322">지금은 설명으로 넘어가겠습니다 표지 다른 디자인 패턴 – 지연 로드 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="2b575-323">즉시 로드, 지연 로드</span><span class="sxs-lookup"><span data-stu-id="2b575-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="2b575-324">ASP.NET MVC 웹 시점에서 우리가 하려는 직원의 정보를 표시 하 고 직원의 포함 하는 응용 프로그램 시간 카드를 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="2b575-325">예를 들어 시스템에서 직원의 이름 및 시간 카드의 총 수를 보여 주는 시간 카드 요약 표시가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="2b575-326">이 기능을 구현 하기 위해 수행할 수 우리는 몇 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="2b575-327">프로젝션</span><span class="sxs-lookup"><span data-stu-id="2b575-327">Projection</span></span>

<span data-ttu-id="2b575-328">쉽게 만드는 방법 한 가지 요약 뷰에 표시 하려는 정보에 전용된 모델을 만드는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="2b575-329">이 시나리오에서는 모델은 다음과 같이 보일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="2b575-330">Note는 EmployeeSummaryViewModel 엔터티가 아닌 – 즉 사항도 아닙니다 데이터베이스에 유지 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="2b575-331">이 클래스를 사용 하 여 강력한 형식의 방식으로 데이터를 뷰로 순서 섞기를 하려고만 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="2b575-332">뷰 모델 속성만 없습니다 동작 (메서드 없음)-를 포함 하기 때문에 데이터 전송 개체 (DTO)와 같은 경우</span><span class="sxs-lookup"><span data-stu-id="2b575-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="2b575-333">속성에는 이동에 필요한 데이터가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="2b575-334">LINQ의 표준 프로젝션 연산자 – Select 연산자를 사용 하 여이 뷰 모델을 인스턴스화하는 것이 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

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

<span data-ttu-id="2b575-335">위의 코드에 두 가지 주요 기능이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-335">There are two notable features to the above code.</span></span> <span data-ttu-id="2b575-336">먼저 코드는 여전히 쉽게 관찰 하 고 격리할 수 있기 때문에 테스트 하기가 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="2b575-337">Select 연산자 작동과 동일 하 게 실제 작업 단위에 대해는 메모리 내 fakes에 대 한 것도 가능 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

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

<span data-ttu-id="2b575-338">두 번째 주목할 만한 기능은 코드 EF4 직원과 시간 카드 정보를 함께 조합 하는 단일 하 고 효율적인 쿼리를 생성 하려면를 허용 하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="2b575-339">특별 한 Api를 사용 하지 않고 직원 및 시간 카드 정보와 같은 개체를 로드 했습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="2b575-340">단순히 코드를 원격 데이터 원본 뿐만 아니라 메모리 내 데이터 원본에 대해 작동 하는 표준 LINQ 연산자를 사용 하 여 필요한 정보를 표현 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="2b575-341">EF4 LINQ 쿼리 및 C에서 생성 된 식 트리를 변환할 수 있었습니다\# 단일 하 고 효율적인 T-SQL 쿼리로 컴파일러.</span><span class="sxs-lookup"><span data-stu-id="2b575-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

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
         )  AS [Project1]
    )  AS [Limit1]
```

<span data-ttu-id="2b575-342">뷰 모델 또는 DTO 개체와 이지만 실제 엔터티를 사용 하 여 작동 않으려는 경우 다른 경우가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="2b575-343">직원을 먼저 알고 때 *및* 직원의 시간 카드 눈에 띄지 않는 효율적인 방식으로 관련 된 데이터를 적극적으로 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="2b575-344">명시적 로딩과</span><span class="sxs-lookup"><span data-stu-id="2b575-344">Explicit Eager Loading</span></span>

<span data-ttu-id="2b575-345">비즈니스 논리에 대 한 (또는이 시나리오에서는 컨트롤러 작업 논리) 메커니즘 필요한 관련된 엔터티 정보를 적극적으로 로드 하려는 경우 하도록 리포지토리를 표현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="2b575-346">EF4 ObjectQuery&lt;T&gt; 클래스 관련된 개체를 쿼리 하는 동안 검색을 지정 하는 Include 메서드를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="2b575-347">EF4 ObjectContext 구체적인 ObjectSet 통해 엔터티를 노출 해야&lt;T&gt; ObjectQuery에서 상속 되는 클래스&lt;T&gt;합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span>  <span data-ttu-id="2b575-348">ObjectSet 사용 하는 경우&lt;T&gt; 에서는 다음 코드를 작성할 수는 컨트롤러 작업에 대 한 참조는 각 직원에 대 한 시간 카드 정보의 즉시 로드를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-348">If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="2b575-349">그러나 테스트 가능한 코드를 유지 하려고 하므로 우리는 노출 되지 않은 ObjectSet&lt;T&gt; 에서 작업 클래스의 실제 단위 외부입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="2b575-350">대신에 의존 하는 i o b j&lt;T&gt; 모조를 쉽게 인터페이스 있지만 i o b j&lt;T&gt; 는 Include 메서드를 정의 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="2b575-351">LINQ의 장점을 Include 연산자를 직접 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

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

<span data-ttu-id="2b575-352">이 포함 연산자를 확장 메서드로 정의 되어 IQueryable&lt;T&gt; 대신 i o b j&lt;T&gt;합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="2b575-353">광범위 한 IQueryable을 비롯 한 가능한 형식이 메서드를 사용 하는 기능 제공&lt;T&gt;, i o b j&lt;T&gt;, ObjectQuery&lt;T&gt;, 및 ObjectSet&lt;T&gt;합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="2b575-354">이벤트 기본 시퀀스를 정품 EF4 ObjectQuery 아닙니다&lt;T&gt;, 그런 다음 완료 아무런 문제가 없습니다 및 Include 연산자가 작동 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="2b575-355">기본 시퀀싱 하는 경우 *됩니다* 는 ObjectQuery&lt;T&gt; (또는 ObjectQuery에서 파생 된&lt;T&gt;), EF4 우리의 요구 사항 추가 데이터에 대 한 참조를 적절 한 SQL을 작성 한 다음 쿼리입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="2b575-356">위치에서이 새 연산자를 사용 하 여 리포지토리에서 시간 카드 정보의 즉시 로드를 명시적으로 요청할 수 있습니다 했습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="2b575-357">에 대해 실제 ObjectContext를 실행 하는 경우 코드를 다음 단일 쿼리를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="2b575-358">쿼리는 employee 개체를 구체화 하 고 완벽 하 게 해당 시간표 속성을 채우는 데 한 번의 데이터베이스에서 충분 한 정보를 수집 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

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
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

<span data-ttu-id="2b575-359">좋은 소식은 작업 메서드 내에서 코드가 완벽 하 게 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="2b575-360">포함 연산자를 지원 하 여 fakes에 대 한 추가 기능을 제공할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="2b575-361">나쁜 소식은 지 속성 무시 유지 하려는 코드 내에서 포함 연산자를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="2b575-362">테스트 가능한 코드를 작성할 때 평가 해야 하는 균형점을 찾아야 형식의 가장 대표적인 예입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="2b575-363">성능 목표를 충족 하기 위해 저장소 추상화 외부 지 속성 우려 누수 수 있도록 해야 하는 경우가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="2b575-364">즉시 로드에 대 한 대체는 지연 로드 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="2b575-365">우리가 지연 로드 의미 *하지* 명시적으로 연결 된 데이터에 대 한 요구를 발표 하는 데는 비즈니스 코드가 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="2b575-366">대신, 응용 프로그램에서이 엔터티 사용 및 추가 데이터는 필요한 Entity Framework가 필요할 때 데이터를 로드 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="2b575-367">지연 로드</span><span class="sxs-lookup"><span data-stu-id="2b575-367">Lazy Loading</span></span>

<span data-ttu-id="2b575-368">여기서 알 수 해야 할 부분은 비즈니스 논리를 데이터 시나리오를 가정해 보겠습니다 하기 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="2b575-369">논리는 employee 개체 하지만 다른 실행 경로 시간 카드 정보가 직원에 해당 경로 중 일부 필요 하지 않은 않았고으로 분기할 수 있습니다 것 알고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="2b575-370">다음과 같은 시나리오는 데이터에는 필요에 따라 마술 표시 되기 때문에 암시적 지연 로드에 적합 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="2b575-371">지연 로드, 지연 된 라고도 로드는 엔터티 개체에 대해 몇 가지 요구 사항이 설정지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="2b575-372">Poco 진정한 지 속성 무시를 사용 하 여 지 속성 계층의 모든 요구 사항을 직면 하지 이지만 진정한 지 속성 무시 실질적으로 목표를 달성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span>  <span data-ttu-id="2b575-373">대신 지 속성 무시 상대도 단위로 측정합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-373">Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="2b575-374">하기란 상당히 아쉬운 점 지향 지 속성 기본 클래스에서 상속 하거나 Poco에서 지연 로드를 위해 특수 한 컬렉션을 사용 해야 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="2b575-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="2b575-375">다행 스럽게도 EF4 줄어들었습니다 솔루션을 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="2b575-376">거의 찾아냅니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-376">Virtually Undetectable</span></span>

<span data-ttu-id="2b575-377">POCO 개체를 사용 하는 경우 EF4 엔터티에 대 한 런타임 프록시를 동적으로 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="2b575-378">이러한 프록시 켜지 구체화 된 Poco 및 각 속성을 해석 하 여 추가 서비스를 받을 제공 래핑하고 추가 작업을 수행 하는 작업을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="2b575-379">이러한 서비스 중 하나는 지연 로딩 기능을 살펴봅니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="2b575-380">다른 서비스는는 효율적인 변경 내용 추적 프로그램 엔터티의 속성 값을 변경 하는 경우를 기록할 수 있는 메커니즘입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="2b575-381">변경 내용 목록은 업데이트 명령을 사용 하 여 수정 된 엔터티를 유지 하기 위해 ObjectContext에서 SaveChanges 메서드를 호출 하는 동안 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="2b575-382">그러나 하려면 이러한 프록시에 대 한 필요한 속성이 get에 연결 하는 방법 및 가상 멤버를 재정의 하 여이 목표를 달성 하는 엔터티, 및 프록시 설정 작업 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="2b575-383">따라서 암시적 지연 로딩 및 효율적인 변경 내용 추적 하려는 경우에 POCO 클래스 정의 돌아가서 가상으로 속성을 표시 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="2b575-384">여전히 Employee 엔터티에 대부분 지 속성 무시 단언할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="2b575-385">유일한 요구 사항은 가상 멤버를 사용 하는 것을 코드의 테스트 용이성에는 영향이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="2b575-386">어떤 특별 한 기본 클래스에서 파생 되거나 지연 로딩에 전용 특수 한 컬렉션을 사용 하 여 필요가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="2b575-387">코드 에서처럼 ICollection을 구현 하는 모든 클래스&lt;T&gt; 관련된 엔터티를 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="2b575-388">이 작업 단위 내에서 확인 해야 하는 한 가지 사소한 변경 이기도 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="2b575-389">지연 로딩은 *해제* 를 ObjectContext 개체와 직접 작업 하는 경우 기본적으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="2b575-390">설정할 수 있습니다 ContextOptions 속성에 사용할 수 있도록 지연 된 로드를 속성 이며 지연 로드 어디서 나 사용할 수 있게 하고자 하는 경우이 속성은 실제 작업 단위 내에서 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

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

<span data-ttu-id="2b575-391">암시적 지연 로딩을 사용 하도록 설정, 응용 프로그램 코드는 직원을 사용할 수 있습니다와 직원의 추가 데이터를 로드 하는 EF에 필요한 작업을 인식 하지 못하는 부분을 유지 하면서 시간 카드를 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="2b575-392">지연 로딩을 쉽게 응용 프로그램 코드를 작성 하 고 코드를 프록시 매직을 사용 하 여 완전히 테스트 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="2b575-393">작업 단위의 메모리 fakes 테스트 하는 동안 필요한 경우 연결 된 데이터를 사용 하 여 가짜 엔터티 미리 로드 하기만 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="2b575-394">이 시점에서는에서는 알아보겠습니다 i o b j를 사용 하 여 저장소에 이르기까지&lt;T&gt; 추상화 숨기기 지 속성 프레임 워크의 모든 기호를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="2b575-395">사용자 지정 리포지토리</span><span class="sxs-lookup"><span data-stu-id="2b575-395">Custom Repositories</span></span>

<span data-ttu-id="2b575-396">이 문서의 작업 디자인 패턴의 단위 먼저 표시에서는 작업 단위를 모양을 대 한 몇 가지 샘플 코드를 제공 했습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="2b575-397">직원과 함께 일 하면서 우리 직원 시간 카드 시나리오를 사용 하 여이 원래 개념을 다시 제공 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="2b575-398">이 작업 단위 및 마지막 섹션에서 만든 작업의 단위 간의 기본 차이점은이 작업 단위에 EF4 프레임 워크에서 모든 추상화를 사용 하지 않는 방법 (방법이 없는 i o b j&lt;T&gt;).</span><span class="sxs-lookup"><span data-stu-id="2b575-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="2b575-399">I o b j&lt;T&gt; works를 리포지토리 인터페이스를 사용 하지만 노출 하는 API도 완벽 하 게 응용 프로그램의 요구를 사용 하 여 정렬 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="2b575-400">향후이 방식에서에서는 사용자 지정 IRepository를 사용 하 여 리포지토리 나타낼&lt;T&gt; 추상화 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="2b575-401">테스트 기반 디자인, 동작 기반 디자인 및 도메인 기반 방법론 디자인을 팔 로우 하는 대부분의 개발자는 IRepository를 선호&lt;T&gt; 방법은 여러 가지 이유로 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="2b575-402">첫 번째는 IRepository&lt;T&gt; 인터페이스는 "anti-corruption" 계층을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="2b575-403">Eric Evans의 저서 Domain Driven Design 설명 된 대로 손상 방지 레이어를 지 속성 API 등의 인프라 Api에서 도메인 코드를 유지 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="2b575-404">둘째, 개발자 (테스트를 작성 하는 동안 검색) 하는 대로 응용 프로그램의 정확한 요구를 충족 하는 리포지토리로 메서드를 빌드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="2b575-405">예를 들어, 우리가 해야 할 수 자주 FindById 메서드 리포지토리 인터페이스를 추가할 수 있도록 ID 값을 사용 하 여 단일 엔터티를 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span>  <span data-ttu-id="2b575-406">우리의 IRepository&lt;T&gt; 정의 다음과 같이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-406">Our IRepository&lt;T&gt; definition will look like the following.</span></span>

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

<span data-ttu-id="2b575-407">IQueryable을 사용 하 여 다시 삭제 됩니다 것을 알 수 있습니다&lt;T&gt; 엔터티 컬렉션을 노출 하는 인터페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="2b575-408">IQueryable&lt;T&gt; EF4 공급자로 이동 하 고 공급자 쿼리의 전체적인 보기를 제공 하도록 LINQ 식 트리를 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="2b575-409">두 번째 옵션을 IEnumerable을 반환 하는 것&lt;T&gt;, EF4 LINQ 공급자 즉, 리포지토리 내에서 작성 한 식만 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="2b575-410">모든 그룹화, 정렬 및 리포지토리 외부에서 수행 하는 프로젝션 하지 성능이 저하 될 수 있는 데이터베이스에 보내는 SQL 명령으로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="2b575-411">반면, IEnumerable만를 반환 하는 리포지토리&lt;T&gt; 새 SQL 명령을 사용 하 여 하를 결과 놀라게 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="2b575-412">두 방법 모두 작동 하 고 테스트 가능한 두 가지 방법을 모두 유지.</span><span class="sxs-lookup"><span data-stu-id="2b575-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="2b575-413">IRepository의 단일 구현이 제공 하는 것이 간단&lt;T&gt; 제네릭과 EF4 ObjectContext API를 사용 하 여 인터페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

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

<span data-ttu-id="2b575-414">IRepository&lt;T&gt; 접근 방식을 제공 쿼리에 대 한 몇 가지 추가 제어를 클라이언트 엔터티를 가져오는 메서드를 호출 하기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="2b575-415">메서드 내에서는 추가 검사 및 응용 프로그램 제약 조건을 적용 하는 LINQ 연산자를 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="2b575-416">인터페이스에 제네릭 형식 매개 변수의 두 가지 제약 조건을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="2b575-417">첫 번째 제약 조건의 ObjectSet에 필요한 클래스 단점 검은&lt;T&gt;, 두 번째 제약 조건을 강제로 IEntity – 응용 프로그램에서 만든 추상화를 구현 하는 엔터티.</span><span class="sxs-lookup"><span data-stu-id="2b575-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="2b575-418">IEntity 인터페이스에 엔터티를 읽을 수 있는 Id 속성을 강제로 수행 하 고 FindById 메서드에서이 속성을 사용 합니다 했습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="2b575-419">다음 코드를 사용 하 여 IEntity 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="2b575-420">IEntity이 엔터티는이 인터페이스를 구현 해야 하므로 작은 지 속성 무시 위반을 간주할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="2b575-421">지 속성 무시는 절충에 대 한 많은 FindById 기능 인터페이스에 의해 적용 된 제약 조건 보다는 기억 하세요.</span><span class="sxs-lookup"><span data-stu-id="2b575-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="2b575-422">인터페이스에 테스트 용이성을 영향을 주지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="2b575-423">라이브 IRepository 인스턴스화&lt;T&gt; 인스턴스화를 관리 하는 구체적인 단위의 클라우드 구현 해야 하므로 EF4 ObjectContext에 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

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

### <a name="using-the-custom-repository"></a><span data-ttu-id="2b575-424">사용자 지정 리포지토리를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="2b575-424">Using the Custom Repository</span></span>

<span data-ttu-id="2b575-425">I o b j 기반 저장소를 사용 하 여 크게 다르지 않습니다 우리의 사용자 지정 리포지토리를 사용 하 여&lt;T&gt;합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="2b575-426">속성에 직접 LINQ 연산자를 적용 하는 대신 먼저 해야 하나 IQueryable을 리포지토리의 메서드를 호출할&lt;T&gt; 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="2b575-427">이전에 구현한 사용자 지정 포함 연산자를 변경 하지 않고도 작동을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="2b575-428">리포지토리의 FindById 메서드는 단일 엔터티를 검색 하는 동안 작업에서 중복 된 논리를 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="2b575-429">살펴보았습니다 두 가지 테스트 용이성에 상당한 차이가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="2b575-430">IRepository의 가짜 구현을 제공할 수 있습니다&lt;T&gt; HashSet가 지원 되는 구체적 클래스를 작성 하 여&lt;직원&gt; -마지막 섹션에서 수행한 것과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="2b575-431">그러나 일부 개발자는 모의 개체를 사용 하 여 모의 개체 프레임 워크 fakes를 구축 하는 대신 선호 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="2b575-432">모의 개체를 사용 하 여 구현을 테스트 하 고 다음 섹션에서 모의 개체와 fakes 간의 차이점에 설명 하 고 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="2b575-433">모의 개체를 사용한 테스트</span><span class="sxs-lookup"><span data-stu-id="2b575-433">Testing with Mocks</span></span>

<span data-ttu-id="2b575-434">"Test double" 어떤 Martin Fowler 호출의 빌드 하는 방법은 여러 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="2b575-435">Test double (예: 동영상 stunt double)는 테스트 하는 동안 프로덕션 개체를 ""를 실제로 빌드는 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="2b575-436">만든 메모리 내 리포지토리는 SQL Server와 통신 하는 리포지토리에 대 한 test double입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="2b575-437">단위 테스트 하는 동안 이러한 test double을 사용 하 여 코드를 분리 하 여 빠르게 실행 하는 테스트를 유지 하는 방법을 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="2b575-438">빌드한 test double에 구현이 실제, 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="2b575-439">개체의 구체적인 컬렉션 저장 각각 백그라운드 하 고 이러한 추가 하 고 테스트 하는 동안 저장소를 조작에서는이 컬렉션에서 개체 제거.</span><span class="sxs-lookup"><span data-stu-id="2b575-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="2b575-440">일부 개발자는 해당 test double이 방법이으로 – 실제 코드 및 작업 구현을 작성 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span>  <span data-ttu-id="2b575-441">이 test double는 부르는 *fakes*합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-441">These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="2b575-442">작업 구현을 갖고 있지만 사실은 그렇지만도 않습니다 프로덕션 사용에 대 한 충분 한 실제입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="2b575-443">가상 repository는 데이터베이스에 실제로 기록 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="2b575-444">실제로 가짜 SMTP 서버 네트워크를 통해 전자 메일 메시지를 전송 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="2b575-445">Fake 및 mock</span><span class="sxs-lookup"><span data-stu-id="2b575-445">Mocks versus Fakes</span></span>

<span data-ttu-id="2b575-446">다른 유형의 double 라고 하는 테스트를 *모의*합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="2b575-447">Fakes가 작업 구현 하는 동안 mock 구현이 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="2b575-448">모의 개체 프레임 워크를 활용 하 여 런타임 시 이러한 모의 개체를 생성 하 고 test double로 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="2b575-449">이 섹션에서는 사용할 것 모의 프레임 워크 Moq 오픈 소스입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="2b575-450">동적으로 만드는 테스트는 직원 리포지토리에 대 한 이중 Moq를 사용 하는 간단한 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="2b575-451">Moq는 IRepository 묻습니다&lt;직원&gt; 구현 하며 빌드 하나 동적으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="2b575-452">IRepository를 구현 하는 개체를 가져올 수 있습니다&lt;직원&gt; Mock의 개체 속성에 액세스 하 여&lt;T&gt; 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="2b575-453">이 내부 개체는 컨트롤러에 전달할 수 이며 다시 test double 또는 실제 리포지토리 경우 알 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="2b575-454">개체의 메서드와 실제 구현과 개체의 메서드를 호출 하는 것 처럼 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="2b575-455">모의 리포지토리 사항을 Add 메서드를 호출 하는 경우 어떻게 되는지 궁금할 수 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="2b575-456">모의 개체와 관련 없는 구현 이므로 아무 추가 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="2b575-457">직원은 삭제 되므로 우리가 작성 한 fakes를 사용 하 여 했습니다 같은 백그라운드 구체적인 컬렉션이 없습니다 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="2b575-458">FindById의 반환 값의 경우는 어떨까요?</span><span class="sxs-lookup"><span data-stu-id="2b575-458">What about the return value of FindById?</span></span> <span data-ttu-id="2b575-459">이 경우 모의 개체 기본값을 반환 되는 것만 작업을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="2b575-460">참조 형식 (직원)으로 반환 하는 것 이므로 반환 값에 null 값이입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="2b575-461">Mock는 실수나; 보일 수 있습니다. 그러나 모의 개체에 이야기 하지 않은 기능을 추가로 두 가지입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="2b575-462">첫째, Moq 프레임 워크는 모의 개체에 대 한 모든 호출은 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="2b575-463">코드의 뒷부분에 나오는 것 Moq Add 메서드를 호출 하는 모든 사용자가 아니면 FindById 메서드를 호출 하는 모든 사용자가 요청할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="2b575-464">이 "블랙 박스" 기록 기능 테스트에서 사용 하는 방법을 나중에 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="2b575-465">두 번째 유용한 기능은 Moq 프로그래밍 모의 개체를 사용 하는 방법을 *기대*합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="2b575-466">모의 개체 모든 지정 된 상호 작용에 응답 하는 방법을 지시 하는 관여 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="2b575-467">예를 들어, 수 있다고 예상할은 mock를 프로그래밍 하 고 FindById를 호출 하는 사람이 경우 employee 개체를 반환 하도록 지시 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="2b575-468">Moq 프레임 워크 설치 API 및 람다 식을 사용 하 여 이러한 기대를 프로그램.</span><span class="sxs-lookup"><span data-stu-id="2b575-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

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

<span data-ttu-id="2b575-469">이 샘플 리포지토리를 동적으로 작성 하려면 Moq 요청에 관여를 사용 하 여 리포지토리 우리가 프로그래밍 하는 다음을</span><span class="sxs-lookup"><span data-stu-id="2b575-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="2b575-470">예상 개체를 반환할 새 직원 Id 값이 5 인 5의 값을 전달 하 여 FindById 메서드를 호출 하는 사람이 경우 모의 개체를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="2b575-471">이 테스트에 통과 및 가짜 IRepository 전체 구현이 필요가 없었습니다&lt;T&gt;합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="2b575-472">앞서 작성 한 테스트를 다시 방문 하 고 fakes 대신 모의 개체를 사용 하도록 재작업 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="2b575-473">마찬가지로 이전에서는 기본 클래스 모든 컨트롤러의 테스트에 필요한 인프라의 공통 부분을 설정 하 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

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

<span data-ttu-id="2b575-474">설정 된 코드는 대개는 동일합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="2b575-475">Fakes를 사용 하는 대신 모의 개체를 만드는 데 Moq 사용 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="2b575-476">코드 직원 속성을 호출할 때 모의 리포지토리를 반환할 작업의 모의 단위에 대 한 기본 클래스를 정렬 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="2b575-477">모의 설정의 나머지 각 특정 시나리오에 전용으로 테스트 픽스 쳐 내에서 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="2b575-478">예를 들어, 인덱스 작업에 대 한 테스트 픽스 쳐 작업 모의 리포지토리의 FindAll 메서드를 호출 하는 경우 직원의 목록을 반환할 모의 리포지토리를 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

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

<span data-ttu-id="2b575-479">기대치를 제외 하 고 테스트 하기 전에 했습니다 테스트와 유사 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="2b575-480">그러나 모의 프레임 워크의 기록 기능을 사용 하 여 다른 각도에서 테스트 접근 수 것입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="2b575-481">다음 섹션에서이 새 관점에서 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="2b575-482">와 상호 작용 테스트 상태</span><span class="sxs-lookup"><span data-stu-id="2b575-482">State versus Interaction Testing</span></span>

<span data-ttu-id="2b575-483">모의 개체를 사용 하 여 소프트웨어를 테스트 하 여 다양 한 기술이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="2b575-484">한 가지 방법은 상태를 사용 하려면 기반 테스트, 새로운 위해서는이 문서의 지금는 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="2b575-485">상태 기반 소프트웨어의 상태에 대 한 테스트는 어설션 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="2b575-486">마지막 테스트에서는 컨트롤러에서 작업 메서드를 호출 하 고 만든 모델에 대 한 어설션을 작성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="2b575-487">테스트 상태에 있는 다른 몇 가지 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="2b575-488">리포지토리 만들기를 실행 한 후 새 employee 개체를 포함을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="2b575-489">인덱스 실행 후 모델 보유 모든 직원의 목록을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="2b575-490">저장소에 없는 지정된 된 직원 삭제를 실행 한 후 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="2b575-491">모의 개체를 사용 하 여 표시 하는 다른 방법은 확인 하는 것 *상호 작용*합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="2b575-492">상태 개체의 상태에 대 한 테스트는 어설션을 기반으로 하는 동안 상호 작용 개체는 상호 작용 하는 방법에 대 한 테스트는 어설션 기반 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="2b575-493">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="2b575-493">For example:</span></span>

-   <span data-ttu-id="2b575-494">컨트롤러 만들기를 실행 하는 경우 저장소의 추가 메서드를 호출을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="2b575-495">인덱스 실행 하는 경우 컨트롤러는 저장소의 FindAll 메서드를 호출을 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="2b575-496">편집을 실행할 때 변경 내용을 저장 단위 작업의 Commit 메서드를 호출 하는 컨트롤러를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="2b575-497">상호 작용 테스트 되지 컬렉션 내에서 배치 하 고 개수를 확인 하기 때문에 테스트 데이터가 종종 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="2b575-498">예를 들어 올바른 값-를 사용 하 여 저장소의 FindById 메서드를 호출 하는 세부 정보 작업을 알고 있다면 다음 작업 아마도 올바르게 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="2b575-499">이 동작은 FindById에서 반환할 테스트 데이터를 설정 하지 않고도 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

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

<span data-ttu-id="2b575-500">위의 테스트 픽스 쳐에서 필요한 유일한 설치는 기본 클래스에서 제공 하 여 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="2b575-501">컨트롤러 작업을 호출 하는 경우 Moq는 모의 리포지토리를 사용 하 여 상호 작용을 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="2b575-502">확인 Moq API를 사용 하 여, 수 요청 Moq 컨트롤러 FindById 적절 한 ID 값을 사용 하 여 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="2b575-503">컨트롤러 메서드를 호출 하지 않은 또는 예기치 않은 매개 변수 값을 사용 하 여 메서드를 호출 하는 경우 Verify 메서드는 예외를 throw 하 고 테스트가 실패 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="2b575-504">만들기 작업을 현재 작업 단위에 커밋 호출을 확인 하려면 또 다른 예는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="2b575-505">상호 작용을 테스트 한 위험을 통해 상호 작용을 지정 하는 경향이입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="2b575-506">기록 하 고 모의 개체를 사용 하 여 모든 상호 작용을 테스트 한다고 해 서 확인 모의 개체의 기능을 모든 상호 작용을 확인 하려고 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="2b575-507">일부 상호 작용은 구현 세부 정보 및 상호 작용만 확인 해야 *필요한* 현재 테스트를 충족 하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="2b575-508">Mock 또는 fake 간의 선택 하느냐에 달려 테스트 중인 시스템과 개인 (또는 팀) 기본 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="2b575-509">모의 개체 test double을 구현 해야 하는 코드의 양을 감소 시킬 수 있습니다 이지만 일부 사용자 에게만 기대치를 프로그래밍 하 고 상호 작용을 확인 하 편리 하 게 합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="2b575-510">결론</span><span class="sxs-lookup"><span data-stu-id="2b575-510">Conclusions</span></span>

<span data-ttu-id="2b575-511">이 문서에 데이터 유지를 위해 ADO.NET Entity Framework를 사용 하는 동안 테스트 가능한 코드를 작성 하는 몇 가지 방법에 알아보았습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="2b575-512">I o b j 같은 기본 제공된 추상화 활용할 수 있습니다&lt;T&gt;, IRepository와 같은 고유한 추상화를 만들거나&lt;T&gt;합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span>  <span data-ttu-id="2b575-513">두 경우 모두 ADO.NET Entity Framework 4.0에서 POCO 지원을 무시 및 가볍고 테스트가 용이한 지속 되도록 하려면 이러한 추상화의 소비자를 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-513">In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="2b575-514">암시적 지연 로딩 하면 비즈니스 및 응용 프로그램 서비스와 같은 추가 EF4 기능은 관계형 데이터 저장소의 세부 정보에 대 한 걱정 없이 작동 하는 코드입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="2b575-515">마지막으로 만들겠습니다. 추상화가 단위 테스트 내에서 모조 또는 모의 쉽게 하 고 이러한 격리 된 매우 빠른 실행을 위해 test double 및 신뢰할 수 있는 테스트 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="2b575-516">추가 리소스</span><span class="sxs-lookup"><span data-stu-id="2b575-516">Additional Resources</span></span>

-   <span data-ttu-id="2b575-517">Robert C. Martin " [단일 책임 원칙을](http://www.objectmentor.com/resources/articles/srp.pdf)"</span><span class="sxs-lookup"><span data-stu-id="2b575-517">Robert C. Martin, “ [The Single Responsibility Principle](http://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="2b575-518">Martin Fowler [패턴 카탈로그](http://www.martinfowler.com/eaaCatalog/index.html) 에서 *엔터프라이즈 응용 프로그램 아키텍처 패턴*</span><span class="sxs-lookup"><span data-stu-id="2b575-518">Martin Fowler, [Catalog of Patterns](http://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="2b575-519">Griffin Caprio " [종속성 주입](https://msdn.microsoft.com/magazine/cc163739.aspx)"</span><span class="sxs-lookup"><span data-stu-id="2b575-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="2b575-520">Data Programmability 블로그 " [연습: Entity Framework 4.0 사용한 테스트 기반 개발](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)"입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="2b575-521">Data Programmability 블로그 " [Entity Framework 4.0을 사용 하 여 사용 하 여 리포지토리 및 작업 단위 패턴](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"</span><span class="sxs-lookup"><span data-stu-id="2b575-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="2b575-522">Dave Astels " [BDD 소개](http://blog.daveastels.com/files/BDD_Intro.pdf)"</span><span class="sxs-lookup"><span data-stu-id="2b575-522">Dave Astels, “ [BDD Intro](http://blog.daveastels.com/files/BDD_Intro.pdf)”</span></span>
-   <span data-ttu-id="2b575-523">Aaron Jensen " [컴퓨터 사양 소개](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"</span><span class="sxs-lookup"><span data-stu-id="2b575-523">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="2b575-524">Eric Lee, " [MSTest 사용 하 여 BDD](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"</span><span class="sxs-lookup"><span data-stu-id="2b575-524">Eric Lee, “ [BDD with MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="2b575-525">Eric Evans " [도메인 기반 디자인](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"</span><span class="sxs-lookup"><span data-stu-id="2b575-525">Eric Evans, “ [Domain Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="2b575-526">Martin Fowler " [스텁 아닌 모의](http://martinfowler.com/articles/mocksArentStubs.html)"</span><span class="sxs-lookup"><span data-stu-id="2b575-526">Martin Fowler, “ [Mocks Aren’t Stubs](http://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="2b575-527">Martin Fowler " [이중 테스트](http://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="2b575-527">Martin Fowler, “ [Test Double](http://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   <span data-ttu-id="2b575-528">Jeremy Miller, " [상호 작용 테스트와 상태](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"</span><span class="sxs-lookup"><span data-stu-id="2b575-528">Jeremy Miller, “ [State versus Interaction Testing](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)”</span></span>
-   <span data-ttu-id="2b575-529">Moq, \<http://code.google.com/p/moq/></span><span class="sxs-lookup"><span data-stu-id="2b575-529">Moq, \<http://code.google.com/p/moq/></span></span>

### <a name="biography"></a><span data-ttu-id="2b575-530">약력</span><span class="sxs-lookup"><span data-stu-id="2b575-530">Biography</span></span>

<span data-ttu-id="2b575-531">Scott Allen Pluralsight의 기술 담당자의 멤버 이며 OdeToCode.com의 창립자입니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-531">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="2b575-532">상용 소프트웨어 개발의 15 년 동안에서 Scott 8 비트 뛰어난 ASP.NET 웹 응용 프로그램에 포함 된 장치에서 모든 항목에 대 한 솔루션에서 일 했습니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-532">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="2b575-533">그의 블로그인 OdeToCode, 또는 Twitter Scott에 도달할 수 있습니다 \< http://twitter.com/OdeToCode>합니다.</span><span class="sxs-lookup"><span data-stu-id="2b575-533">You can reach Scott on his blog at OdeToCode, or on Twitter at \<http://twitter.com/OdeToCode>.</span></span>
