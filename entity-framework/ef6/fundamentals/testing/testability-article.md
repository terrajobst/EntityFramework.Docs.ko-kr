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
# <a name="testability-and-entity-framework-40"></a>테스트 용이성 및 Entity Framework 4.0
Scott Allen

게시일: 2010년 5월

## <a name="introduction"></a>소개

이 백서에서는 ADO.NET Entity Framework 4.0 및 Visual Studio 2010를 사용 하 여 테스트 가능한 코드를 작성 하는 방법을 설명 합니다. 이 문서에서는 TDD (테스트 기반 디자인) 또는 BDD (동작 중심 디자인)와 같은 특정 테스트 방법론에 집중 하지 않습니다. 대신이 문서에서는 Entity Framework ADO.NET를 사용 하는 코드를 작성 하는 방법에 중점을 두고 있지만 자동화 된 방식으로 격리 하 고 테스트 하는 것은 쉽지 않습니다. 데이터 액세스 시나리오에서 테스트를 용이 하 게 하 고 프레임 워크를 사용할 때 이러한 패턴을 적용 하는 방법을 확인 하는 일반적인 디자인 패턴을 살펴보겠습니다. 또한 프레임 워크의 특정 기능을 살펴보고 이러한 기능이 테스트 가능한 코드에서 어떻게 작동 하는지 확인 합니다.

## <a name="what-is-testable-code"></a>테스트 가능한 코드 란 무엇 인가요?

자동화 된 단위 테스트를 사용 하 여 소프트웨어를 확인 하는 기능은 다양 한 이점을 제공 합니다. 모든 사용자는 응용 프로그램에서 소프트웨어 결함의 수를 줄이고 응용 프로그램의 품질을 높일 수 있다는 것을 알고 있습니다. 하지만 단위 테스트는 버그를 찾는 것 보다 훨씬 더 효과적입니다.

훌륭한 유닛 테스트 도구 모음을 사용 하면 개발 팀이 시간을 절약 하 고 자신이 만든 소프트웨어를 계속 제어할 수 있습니다. 팀은 기존 코드를 변경 하 고, 새 요구 사항을 충족 하기 위해 소프트웨어를 리팩터링, 다시 디자인 및 재구성 하 고, 테스트 도구 모음을 통해 응용 프로그램의 동작을 확인 하는 동안 새 구성 요소를 응용 프로그램에 추가할 수 있습니다. 단위 테스트는 복잡성이 증가 함에 따라 쉽게 변경 하 고 소프트웨어의 유지 관리를 유지 하기 위한 빠른 피드백 주기의 일부입니다.

그러나 유닛 테스트에는 가격이 제공 됩니다. 팀은 단위 테스트를 만들고 유지 관리 하는 데 시간을 투자 해야 합니다. 이러한 테스트를 만드는 데 필요한 작업량은 기본 소프트웨어의 테스트 **용이성** 과 직접적으로 관련이 있습니다. 소프트웨어를 얼마나 쉽게 테스트할 수 있나요? 테스트 용이성을 염두에 두면 소프트웨어를 설계 하는 팀은 테스트를 취소 하지 않은 소프트웨어로 작업 하는 것 보다 빠르게 효과적인 테스트를 만들 수 있습니다.

Microsoft는 테스트 용이성을 염두에 ADO.NET Entity Framework 4.0 (EF4)을 설계 했습니다. 개발자가 프레임 워크 코드 자체에 대해 단위 테스트를 작성 하는 것은 아닙니다. 대신, EF4의 테스트 용이성 목표를 통해 프레임 워크를 기반으로 구축 되는 테스트 가능한 코드를 쉽게 만들 수 있습니다. 특정 예제를 살펴보기 전에 테스트 가능한 코드의 품질을 이해 하는 것이 좋습니다.

### <a name="the-qualities-of-testable-code"></a>테스트 가능한 코드의 품질

테스트 하기 쉬운 코드에는 항상 두 가지 이상의 특징이 있습니다. 먼저 테스트 가능한 코드는 쉽게 **확인할**수 있습니다. 일부 입력 집합을 지정 하는 경우 코드의 출력을 쉽게 관찰할 수 있어야 합니다. 예를 들어 메서드가 계산 결과를 직접 반환 하기 때문에 다음 메서드를 테스트 하는 것이 쉽습니다.

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

메서드가 계산 된 값을 네트워크 소켓, 데이터베이스 테이블 또는 다음 코드와 같은 파일에 기록 하는 경우 메서드를 테스트 하기가 어렵습니다. 테스트에서는 값을 검색 하기 위해 추가 작업을 수행 해야 합니다.

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

둘째로, 테스트 가능한 코드를 쉽게 **격리할**수 있습니다. 다음 의사 코드를 테스트 가능한 코드의 잘못 된 예로 사용 하겠습니다.

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

메서드는 관찰 하기 쉽습니다. 보험 정책을 전달 하 고 반환 값이 예상 결과와 일치 하는지 확인할 수 있습니다. 그러나 메서드를 테스트 하려면 올바른 스키마를 사용 하 여 데이터베이스를 설치 하 고 메서드에서 전자 메일을 보내려고 시도 하는 경우 SMTP 서버를 구성 해야 합니다.

단위 테스트는 메서드 내에서 계산 논리를 확인 하려고 하지만 전자 메일 서버가 오프 라인 상태 이거나 데이터베이스 서버가 이동 했기 때문에 테스트가 실패할 수 있습니다. 이러한 오류는 모두 테스트에서 확인 하려는 동작과 관련이 없습니다. 이 동작은 격리 하기 어렵습니다.

테스트 가능한 코드를 작성 하기 위해 노력 하는 소프트웨어 개발자는 종종 작성 하는 코드에서 문제의 분리를 유지 하기 위해 노력 하 고 있습니다. 위의 방법은 비즈니스 계산에 집중 하 고 데이터베이스 및 전자 메일 구현 세부 정보를 다른 구성 요소에 위임 해야 합니다. Robert Martin는이를 단일 책임 원칙으로 호출 합니다. 개체는 정책 값을 계산 하는 것과 같은 단일의 좁은 책임을 캡슐화 해야 합니다. 다른 모든 데이터베이스와 알림 작업은 다른 개체의 책임입니다. 이러한 방식으로 작성 된 코드는 단일 작업에 초점을 맞추고 있으므로 쉽게 격리할 수 있습니다.

.NET에는 단일 책임 원칙을 따르고 격리를 실현 하는 데 필요한 추상화가 있습니다. 인터페이스 정의를 사용 하 고 코드에서 구체적인 형식 대신 인터페이스 추상화를 사용 하도록 강제할 수 있습니다. 이 문서의 뒷부분에서는 위에 표시 된 잘못 된 예와 같은 메서드가 데이터베이스와 통신 하는 것 처럼 *보이는* 인터페이스를 사용 하는 방법에 대해 알아봅니다. 그러나 테스트 시에는 데이터베이스와 통신 하지 않고 메모리에 데이터를 저장 하는 더미 구현을 대체할 수 있습니다. 이 더미 구현은 데이터 액세스 코드 또는 데이터베이스 구성에서 관련 되지 않은 문제의 코드를 격리 합니다.

격리에는 추가적인 이점이 있습니다. 마지막 방법의 비즈니스 계산은 실행 하는 데 몇 밀리초 밖에 걸리지 않지만 테스트 자체는 네트워크 주위의 코드 홉으로 몇 초 동안 실행 되 고 다양 한 서버에 연결할 수 있습니다. 단위 테스트를 빠르게 실행 하 여 작은 변경을 용이 하 게 합니다. 테스트와 관련 없는 구성 요소에 문제가 있으므로 단위 테스트도 반복 가능 하 고 실패 하지 않아야 합니다. 쉽게 관찰 하 고 격리할 수 있는 코드를 작성 하는 것은 개발자가 코드에 대 한 테스트를 작성 하는 데 시간이 더 쉽고, 테스트가 실행 되기를 기다리는 데 걸리는 시간을 줄이고, 더 중요 한 것은 존재 하지 않는 버그를 추적 하는 시간을 줄여 주는 것입니다.

테스트를 통해 코드에서 제공 하는 품질을 테스트 하 고 이해 하는 데 많은 도움이 될 것입니다. EF4를 사용 하 여 데이터를 데이터베이스에 저장 하기 위해 사용 하는 코드를 작성 하는 방법을 해결 하려고 합니다. 관찰 가능 하 고 쉽게 격리할 수 있지만, 먼저 데이터 액세스를 위한 테스트 가능한 디자인을 논의 합니다.

## <a name="design-patterns-for-data-persistence"></a>데이터 지 속성을 위한 디자인 패턴

앞에서 설명한 두 가지 잘못 된 예제에는 책임이 너무 많습니다. 첫 번째 잘못 된 예에서는 계산을 수행 하 *고* 파일에 써야 했습니다. 두 번째 잘못 된 예 *에서는 데이터베이스에서 데이터를 읽고* 비즈니스 계산을 수행 하 *고* 전자 메일을 보내야 했습니다. 문제를 구분 하 고 다른 구성 요소에 대 한 책임을 위임 하는 더 작은 메서드를 디자인 하면 테스트 가능한 코드를 작성 하는 데 매우 발전시키 됩니다. 목표는 작고 집중 된 추상화에서 작업을 작성 하 여 기능을 빌드하는 것입니다.

데이터 지 속성에 도달 하면 원하는 작고 집중 된 추상화가 디자인 패턴으로 문서화 된 것입니다. Martin Fowler 's Enterprise 응용 프로그램 아키텍처의 책 패턴은 인쇄에서 이러한 패턴을 설명 하는 첫 번째 작업 이었습니다. 이러한 패턴에 대 한 간략 한 설명을 제공 하기 전에 다음 섹션에서 이러한 패턴에 대 한 간략 한 설명을 제공 합니다. 이러한 패턴을 사용 하 여 이러한 패턴을 구현 하는 방법을 Entity Framework ADO.NET

### <a name="the-repository-pattern"></a>리포지토리 패턴

Fowler은 "도메인 개체에 액세스 하기 위해 컬렉션 형식 인터페이스를 사용 하 여 도메인 및 데이터 매핑 계층 간의 중재"을 말합니다. 리포지토리 패턴의 목표는 데이터 액세스 minutiae 코드를 격리 하는 것입니다. 이전에 살펴본 것 처럼 테스트 용이성을 위해 필요한 특성입니다.

격리에 대 한 핵심은 저장소가 컬렉션 형식 인터페이스를 사용 하 여 개체를 노출 하는 방법입니다. 리포지토리를 사용 하기 위해 작성 하는 논리는 리포지토리가 요청한 개체를 구체화 하는 방법을 알 수 없습니다. 리포지토리는 데이터베이스와 통신 하거나 메모리 내 컬렉션에서 개체를 반환 하는 것일 수 있습니다. 모든 코드에서이 컬렉션을 유지 관리 하는 것 처럼 보이지만 컬렉션에서 개체를 검색, 추가 및 삭제할 수 있음을 알고 있어야 합니다.

기존 .NET 응용 프로그램에서 구체적인 리포지토리는 종종 다음과 같은 제네릭 인터페이스에서 상속 됩니다.

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

EF4에 대 한 구현을 제공할 때 인터페이스 정의를 몇 가지 변경 하지만 기본 개념은 동일 하 게 유지 됩니다. 코드는이 인터페이스를 구현 하는 구체적인 리포지토리를 사용 하 여 기본 키 값으로 엔터티를 검색 하거나, 조건자의 평가를 기준으로 엔터티 컬렉션을 검색 하거나, 사용 가능한 모든 엔터티를 간단히 검색할 수 있습니다. 또한 코드는 리포지토리 인터페이스를 통해 엔터티를 추가 하 고 제거할 수 있습니다.

Employee 개체의 IRepository가 지정 된 경우 코드는 다음 작업을 수행할 수 있습니다.

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

코드는 인터페이스 (Employee의 IRepository)를 사용 하기 때문에 인터페이스의 여러 구현에 코드를 제공할 수 있습니다. 한 가지 구현은 EF4에서 지원 되는 구현 이며 개체를 Microsoft SQL Server 데이터베이스에 유지할 수 있습니다. 다른 구현 (테스트 중에 사용 하는 것)은 Employee 개체의 메모리 내 목록에서 지원할 수 있습니다. 인터페이스는 코드에서 격리를 얻는 데 도움이 됩니다.

IRepository&lt;T&gt; 인터페이스가 저장 작업을 노출 하지 않습니다. 기존 개체를 업데이트 하는 방법 저장 작업을 포함 하는 IRepository 정의에 걸쳐 있을 수 있으며, 이러한 리포지토리의 구현에서 개체를 데이터베이스에 즉시 보관 해야 합니다. 그러나 대부분의 응용 프로그램에서는 개체를 개별적으로 유지 하지 않으려고 합니다. 대신 다른 리포지토리에서 개체를 수명으로 가져오고, 비즈니스 활동의 일부로 개체를 수정한 다음, 단일 원자성 작업의 일부로 모든 개체를 유지 하려고 합니다. 다행히 이러한 유형의 동작을 허용 하는 패턴이 있습니다.

### <a name="the-unit-of-work-pattern"></a>작업 단위 패턴

Fowler는 작업 단위를 "비즈니스 트랜잭션의 영향을 받는 개체의 목록을 유지 관리 하 고, 변경 내용을 기록 하지 않으며 동시성 문제를 해결 하는 작업을 조정 합니다." 라고 표시 합니다. 저장소에서 사용 하는 개체에 대 한 변경 내용을 추적 하 고, 변경 내용을 커밋하는 작업 단위에 지시할 때 개체에 대 한 변경 내용을 유지 하는 것은 작업 단위를 담당 합니다. 또한 모든 리포지토리에 추가 된 새 개체를 가져와 데이터베이스에 삽입 하 고 삭제를 관리 하는 작업 단위를 책임 집니다.

ADO.NET 데이터 집합을 사용 하 여 작업을 수행한 적이 있으면 작업 단위 패턴에 이미 익숙해져야 합니다. ADO.NET 데이터 집합에는 DataRow 개체의 업데이트, 삭제 및 삽입을 추적 하는 기능이 있으며 TableAdapter를 사용 하 여 모든 변경 내용을 데이터베이스에 맞게 조정할 수 있습니다. 그러나 데이터 집합 개체는 기본 데이터베이스의 연결 되지 않은 하위 집합을 모델링 합니다. 작업 단위 패턴은 동일한 동작을 나타내지만 데이터 액세스 코드에서 격리 되 고 데이터베이스를 인식 하지 못하는 비즈니스 개체 및 도메인 개체와 함께 작동 합니다.

.NET 코드의 작업 단위를 모델링 하는 추상화는 다음과 같습니다.

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

작업 단위에서 리포지토리 참조를 노출 하면 단일 작업 단위 개체에 비즈니스 트랜잭션 중에 구체화 된 모든 엔터티를 추적할 수 있는 기능이 있는지 확인할 수 있습니다. 실제 작업 단위에 대 한 Commit 메서드를 구현 하는 것은 모든 매직이 데이터베이스와 메모리 내 변경 내용을 조정 하는 데 발생 합니다. 

Iit에서 작업 참조를 지정 하는 경우 코드는 하나 이상의 리포지토리에서 검색 된 비즈니스 개체를 변경 하 고 원자성 커밋 작업을 사용 하 여 모든 변경 내용을 저장할 수 있습니다.

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a>지연 로드 패턴

Fowler는 이름 지연 로드를 사용 하 여 "필요한 데이터를 모두 포함 하 고 가져오는 방법을 알고 있는 개체"를 설명 합니다. 투명 지연 로드는 테스트 가능한 비즈니스 코드를 작성 하 고 관계형 데이터베이스를 사용 하는 경우 중요 한 기능입니다. 예를 들어 다음 코드를 고려 합니다.

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

TimeCards 컬렉션은 어떻게 채워져 있나요? 두 가지 가능한 대답이 있습니다. 직원 리포지토리가 직원을 인출 하 라는 메시지가 표시 되 면 직원 리포지토리가 직원의 연결 된 시간 카드 정보와 함께 두 직원을 모두 검색 하는 쿼리를 발행 하는 것이 한 가지입니다. 관계형 데이터베이스에는 일반적으로 조인 절이 있는 쿼리가 필요 하며 응용 프로그램 요구 사항 보다 더 많은 정보가 검색 될 수 있습니다. 응용 프로그램에서 TimeCards 속성을 건드리지 않아도 되는 경우는 어떻게 되나요?

두 번째 대답은 "요청 시" TimeCards 속성을 로드 하는 것입니다. 이 지연 로드는 코드에서 특정 Api를 호출 하 여 시간 카드 정보를 검색 하지 않으므로 비즈니스 논리에 대해 암시적이 고 투명 합니다. 이 코드에서는 필요할 때 시간 카드 정보가 표시 되는 것으로 가정 합니다. 일반적으로 메서드 호출의 런타임 가로채기를 포함 하는 지연 로드와 관련 된 몇 가지 매직이 있습니다. 가로채기 코드는 비즈니스 논리를 자유롭게 비즈니스 논리로 유지 하면서 데이터베이스와 통신 하 고 시간 카드 정보를 검색 하는 일을 담당 합니다. 이 지연 로드 magic을 사용 하면 비즈니스 코드를 데이터 검색 작업에서 분리 하 여 더 많은 테스트 가능한 코드를 생성 합니다.

지연 로드의 *단점은 응용 프로그램에 시간* 카드 정보가 필요할 때 코드가 추가 쿼리를 실행 하는 것입니다. 이는 많은 응용 프로그램에서 중요 한 것은 아니지만 성능에 민감한 응용 프로그램 또는 응용 프로그램에서 많은 직원 개체를 반복 하 고 루프의 각 반복에서 시간 카드를 검색 하는 쿼리를 실행 하는 것은 문제가 되지 않습니다 (종종 N + 1 이라고 함). 쿼리 문제), 지연 로드는 끌어 옵니다. 이러한 시나리오에서 응용 프로그램은 최대한 효율적으로 시간 카드 정보를 로드 하는 것이 좋습니다.

다행히 다음 섹션으로 이동 하 여 이러한 패턴을 구현할 때 EF4에서 암시적 지연 로드와 효율적인 즉시 로드를 모두 지 원하는 방법을 살펴보겠습니다.

## <a name="implementing-patterns-with-the-entity-framework"></a>Entity Framework를 사용 하 여 패턴 구현

좋은 소식은 마지막 섹션에서 설명한 모든 디자인 패턴을 EF4를 사용 하 여 간단 하 게 구현 하는 것입니다. 간단한 ASP.NET MVC 응용 프로그램을 사용 하 여 직원 및 관련 시간 카드 정보를 편집 하 고 표시 하는 방법을 보여 줍니다. 다음 "일반 이전 CLR 개체" (POCOs)를 사용 하 여 시작 합니다. 

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

이러한 클래스 정의는 EF4의 다양 한 접근 방식 및 기능을 살펴보기 때문에 약간 변경 되지만 이러한 클래스를 가능한 한 지 속성 무시 (PI)로 유지 하는 것은 의도 된 것입니다. PI 개체는 데이터베이스 내에 유지 *되는 상태를 알 수*없습니다. PI 및 POCOs는 테스트 가능한 소프트웨어와 함께 직접 이동 합니다. POCO 접근 방법을 사용 하는 개체는 데이터베이스 없이 작동할 수 있으므로 제한이 적고 더 유연 하며 테스트 하기가 더 쉽습니다.

POCOs를 사용 하 여 Visual Studio에서 EDM (엔터티 데이터 모델)을 만들 수 있습니다 (그림 1 참조). EDM을 사용 하 여 엔터티에 대 한 코드를 생성 하지 않습니다. 대신, lovingly 만든 엔터티를 직접 사용 하고자 합니다. EDM을 사용 하 여 데이터베이스 스키마를 생성 하 고 개체를 데이터베이스에 매핑하는 데 필요한 메타 데이터 EF4을 제공 합니다.

![ef test_01](~/ef6/media/eftest-01.jpg)

**그림 1**

참고: EDM 모델을 먼저 개발 하려는 경우 EDM에서 정리 된 POCO 코드를 생성할 수 있습니다. 데이터 프로그래밍 팀에서 제공 하는 Visual Studio 2010 확장을 사용 하 여이 작업을 수행할 수 있습니다. 확장을 다운로드 하려면 Visual Studio의 도구 메뉴에서 확장 관리자를 시작 하 고 "POCO" 템플릿의 온라인 갤러리를 검색 합니다 (그림 2 참조). EF에 사용할 수 있는 몇 가지 POCO 템플릿이 있습니다. 템플릿 사용에 대 한 자세한 내용은 " [연습: Entity Framework에 대 한 POCO 템플릿](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)"을 참조 하십시오.

![ef test_02](~/ef6/media/eftest-02.png)

**그림 2**

이 POCO 시작 지점에서 테스트 가능한 코드에 대 한 두 가지 접근 방식을 살펴보겠습니다. 첫 번째 방법은 Entity Framework API의 추상화를 활용 하 여 작업 단위 및 리포지토리를 구현 하기 때문에 EF 방법을 호출 하는 것입니다. 두 번째 방법에서는 고유한 사용자 지정 리포지토리 추상화를 만든 다음 각 방법의 장점과 단점을 살펴봅니다. 먼저 EF 방식을 살펴보는 것입니다.  

### <a name="an-ef-centric-implementation"></a>EF 중심 구현

ASP.NET MVC 프로젝트에서 다음 컨트롤러 작업을 고려 합니다. 작업은 Employee 개체를 검색 하 고 결과를 반환 하 여 직원의 상세 보기를 표시 합니다.

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

코드를 테스트할 수 있나요? 작업의 동작을 확인 하는 데 필요한 두 개 이상의 테스트가 있습니다. 먼저 작업에서 올바른 뷰를 반환 하는지 확인 합니다. 간단히 테스트 합니다. 또한 작업에서 올바른 직원을 검색 하는지 확인 하는 테스트를 작성 하 고, 데이터베이스를 쿼리 하는 코드를 실행 하지 않고이 작업을 수행 하려고 합니다. 테스트 중인 코드를 격리 하려고 합니다. 격리는 데이터 액세스 코드 또는 데이터베이스 구성의 버그로 인해 테스트가 실패 하지 않도록 합니다. 테스트에 실패 하는 경우 일부 낮은 수준의 시스템 구성 요소가 아니라 컨트롤러 논리에 버그가 있음을 알게 됩니다.

격리를 위해 앞에서 리포지토리 및 작업 단위에 대해 제공한 인터페이스와 같은 일부 추상화가 필요 합니다. 리포지토리 패턴은 도메인 개체와 데이터 매핑 계층 간에 중재 설계 되었습니다. 이 시나리오에서 EF4 *는* 데이터 매핑 계층 이며 IObjectSet&lt;t&gt; 라는 리포지토리와 유사한 추상화를 이미 제공 합니다 (네임 스페이스에서). 인터페이스 정의는 다음과 같습니다.

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

IObjectSet&lt;T&gt;는 개체 컬렉션 (IEnumerable&lt;T&gt;를 통해)과 비슷하며 시뮬레이션 된 컬렉션에서 개체를 추가 하 고 제거 하는 메서드를 제공 하기 때문에 리포지토리에 대 한 요구 사항을 충족 합니다. 연결 및 분리 메서드는 EF4 API의 추가 기능을 노출 합니다. 리포지토리에 대 한 인터페이스로 IObjectSet&lt;T&gt;를 사용 하려면 리포지토리를 함께 바인딩하는 작업 단위가 필요 합니다.

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

이 인터페이스의 구체적인 구현은 SQL Server와 통신 하며 EF4의 ObjectContext 클래스를 사용 하 여 쉽게 만들 수 있습니다. ObjectContext 클래스는 EF4 API의 실제 작업 단위입니다.

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

IObjectSet&lt;T&gt;를 life로 가져오는 것은 ObjectContext 개체의 Createobjectset<tentity> 메서드를 호출 하는 것 만큼 쉽습니다. 내부적으로 프레임 워크는 EDM에서 제공한 메타 데이터를 사용 하 여 구체적인 ObjectSet&lt;T&gt;을 생성 합니다. 클라이언트 코드에서 테스트 용이성을 유지 하는 데 도움이 되는 IObjectSet&lt;T&gt; 인터페이스를 반환 합니다.

이 구체적 구현은 프로덕션에서 유용 하지만 테스트를 용이 하 게 하기 위해 Iwork 추상화를 사용 하는 방법에 중점을 두어야 합니다.

### <a name="the-test-doubles"></a>테스트 두 배

컨트롤러 작업을 격리 하려면 실제 작업 단위 (ObjectContext에서 지원)와 테스트 double 또는 "가짜" 작업 단위 (메모리 내 작업 수행) 간을 전환 하는 기능이 필요 합니다. 이러한 유형의 전환을 수행 하는 일반적인 방법은 MVC 컨트롤러에서 작업 단위를 인스턴스화할 수 있도록 하는 것이 아니라 작업 단위를 컨트롤러에 생성자 매개 변수로 전달 하는 것입니다.

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

위의 코드는 종속성 주입의 예입니다. 컨트롤러에서 종속성 (작업 단위)을 만들 수는 없지만 컨트롤러에 종속성을 삽입 하는 것은 허용 되지 않습니다. MVC 프로젝트에서는 사용자 지정 컨트롤러 팩터리를 IoC (반전 제어) 컨테이너와 함께 사용 하 여 종속성 주입을 자동화 하는 것이 일반적입니다. 이러한 항목은이 문서의 범위를 벗어나 있지만이 문서의 끝에 있는 참조를 따라 자세히 알아볼 수 있습니다.

테스트에 사용할 수 있는 가짜 작업 구현 단위는 다음과 같습니다.

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

가짜 작업 단위가 커밋된 속성을 노출 합니다. 테스트를 용이 하 게 하는 가짜 클래스에 기능을 추가 하는 것이 유용한 경우도 있습니다. 이 경우, 코드에서 커밋된 속성을 확인 하 여 작업 단위를 커밋하는 지 쉽게 확인할 수 있습니다.

또한 메모리에 직원 및 출퇴근 시간 기록표 개체를 보관 하는 가짜 IObjectSet&lt;T&gt; 필요 합니다. 제네릭을 사용 하 여 단일 구현을 제공할 수 있습니다.

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

이 테스트는 대부분의 작업을 기본 HashSet&lt;T&gt; 개체로 위임 합니다. IObjectSet&lt;T&gt;에는 T를 클래스 (참조 형식)로 적용 하는 제네릭 제약 조건이 필요 하며,이를 통해 IQueryable&lt;T&gt;도 구현 합니다. 표준 LINQ 연산자를 사용 하 여 메모리 내 컬렉션을 IQueryable&lt;T&gt;으로 표시 하는 것이 쉽습니다.

### <a name="the-tests"></a>테스트

기존 단위 테스트는 단일 테스트 클래스를 사용 하 여 단일 MVC 컨트롤러의 모든 작업에 대 한 모든 테스트를 저장 합니다. 빌드된 메모리 내 fakes를 사용 하 여 이러한 테스트 또는 모든 형식의 단위 테스트를 작성할 수 있습니다. 그러나이 문서에서는 모놀리식 테스트 클래스 접근 방식을 사용 하지 않고, 대신 특정 기능에 초점을 맞춘 테스트를 그룹화 합니다.  예를 들어 "새 직원 만들기"는 테스트 하려는 기능 일 수 있으므로 단일 테스트 클래스를 사용 하 여 새 직원을 만드는 단일 컨트롤러 작업을 확인 합니다.

이러한 모든 세분화 된 테스트 클래스에 필요한 몇 가지 일반적인 설정 코드가 있습니다. 예를 들어 항상 메모리 내 리포지토리 및 가짜 작업 단위를 만들어야 합니다. 또한 가짜 작업 단위가 삽입 된 employee controller 인스턴스가 필요 합니다. 기본 클래스를 사용 하 여 테스트 클래스 간에이 일반적인 설치 코드를 공유 합니다.

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

기본 클래스에서 사용 하는 "개체 어머니"는 테스트 데이터를 만들기 위한 하나의 일반적인 패턴입니다. 어머니 개체는 여러 테스트 설비에서 사용할 테스트 엔터티를 인스턴스화하는 팩터리 메서드를 포함 합니다.

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

EmployeeControllerTestBase를 다양 한 테스트 fixture의 기본 클래스로 사용할 수 있습니다 (그림 3 참조). 각 테스트 fixture는 특정 컨트롤러 작업을 테스트 합니다. 예를 들어, 한 테스트 fixture는 HTTP GET 요청 중에 사용 된 만들기 작업을 테스트 하는 데 중점을 두고 (직원을 만들기 위한 보기를 표시 하기 위해) 다른 fixture는 HTTP POST 요청에 사용 된 만들기 작업에 집중 합니다. 직원을 만드는 사용자입니다.) 각 파생 클래스는 특정 컨텍스트에 필요한 설치를 담당 하며 특정 테스트 컨텍스트의 결과를 확인 하는 데 필요한 어설션을 제공 합니다.

![ef test_03](~/ef6/media/eftest-03.png)

**그림 3**

여기에 제공 된 명명 규칙 및 테스트 스타일은 테스트 가능한 코드에 필요 하지 않습니다. 단 하나의 방법입니다. 그림 4는 Visual Studio 2010 용 Jet Brains Resharper test runner 플러그 인에서 실행 되는 테스트를 보여 줍니다.

![ef test_04](~/ef6/media/eftest-04.png)

**그림 4**

공유 설정 코드를 처리 하기 위한 기본 클래스를 사용 하는 경우 각 컨트롤러 작업에 대 한 단위 테스트는 작고 간단 하 게 작성할 수 있습니다. 테스트는 메모리 내 작업을 수행 하기 때문에 신속 하 게 실행 되며 관련이 없는 인프라 또는 환경 문제로 인해 실패 해서는 안 됩니다 (테스트 중인 단위를 격리 했으므로).

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

이러한 테스트에서 기본 클래스는 대부분의 설치 작업을 수행 합니다. 기본 클래스 생성자는 메모리 내 리포지토리, 가짜 작업 단위 및 EmployeeController 클래스의 인스턴스를 만듭니다. 테스트 클래스는이 기본 클래스에서 파생 되며, Create 메서드 테스트에 대 한 세부 정보를 중심으로 설명 합니다. 이 경우 "정렬, 작업 및 어설션" 단계에 대 한 자세한 내용은 모든 단위 테스트 절차에 있지만.

-   들어오는 데이터를 시뮬레이트하는 newEmployee 개체를 만듭니다.
-   EmployeeController의 만들기 작업을 호출 하 고 newEmployee를 전달 합니다.
-   만들기 작업에서 예상 된 결과를 생성 하는지 확인 합니다. 직원이 리포지토리에 표시 됩니다.

빌드된 항목을 통해 EmployeeController 작업을 테스트할 수 있습니다. 예를 들어 직원 컨트롤러의 인덱스 작업에 대 한 테스트를 작성 하는 경우 테스트 기본 클래스에서 상속 하 여 테스트에 대해 동일한 기본 설정을 구성할 수 있습니다. 다시 기본 클래스는 메모리 내 리포지토리, 가짜 작업 단위 및 EmployeeController의 인스턴스를 만듭니다. 인덱스 작업에 대 한 테스트는 인덱스 동작을 호출 하 고 작업에서 반환 하는 모델의 품질을 테스트 하는 데만 집중 하면 됩니다.

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

메모리 내 fakes를 사용 하 여 만드는 테스트는 소프트웨어의 *상태* 를 테스트 하는 것입니다. 예를 들어 만들기 작업을 테스트할 때 만들기 작업이 실행 된 후 리포지토리의 상태를 검사 하려고 합니다. 리포지토리가 새 직원을 보유 합니까?

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

나중에 상호 작용 기반 테스트를 살펴보겠습니다. 테스트 중인 코드에서 개체에 대 한 적절 한 메서드를 호출 하 고 올바른 매개 변수를 전달 했는지 여부를 상호 작용 기반 테스트에서 요청 합니다. 지금은 지연 로드 인 다른 디자인 패턴의 덮개를 이동 합니다.

## <a name="eager-loading-and-lazy-loading"></a>즉시 로드 및 지연 로드

ASP.NET MVC 웹 응용 프로그램의 특정 시점에서 직원의 정보를 표시 하 고 직원의 연결 된 시간 카드를 포함할 수 있습니다. 예를 들어 시스템의 직원 이름과 총 시간 카드 수를 표시 하는 시간 카드 요약 표시가 있을 수 있습니다. 이 기능을 구현 하기 위해 수행할 수 있는 몇 가지 방법이 있습니다.

### <a name="projection"></a>프로젝션

요약을 만드는 한 가지 쉬운 방법은 보기에 표시 하려는 정보 전용 모델을 생성 하는 것입니다. 이 시나리오에서 모델은 다음과 같이 표시 될 수 있습니다.

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

EmployeeSummaryViewModel는 엔터티가 아닙니다. 즉, 데이터베이스에서 유지 하려는 항목이 아닙니다. 이 클래스를 사용 하 여 강력한 형식의 데이터를 뷰로 무작위 이동할 예정입니다. 뷰 모델은 동작 (메서드 없음)만 포함 하 고 속성을 포함 하기 때문에 DTO (데이터 전송 개체)와 같습니다. 속성에는 이동 해야 하는 데이터가 포함 됩니다. LINQ의 표준 프로젝션 연산자 (Select 연산자)를 사용 하 여이 뷰 모델을 쉽게 인스턴스화할 수 있습니다.

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

위의 코드에는 두 가지 주요 기능이 있습니다. 먼저, 코드를 쉽게 관찰 하 고 격리할 수 있으므로 쉽게 테스트할 수 있습니다. Select 연산자는 실제 작업 단위에 대해 수행 되는 것 처럼 메모리 내 fakes에 대해서만 작동 합니다.

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

두 번째 주목할 만한 기능은 코드를 사용 하 여 직원 및 시간 카드 정보를 함께 EF4 하는 효율적인 단일 쿼리를 생성 하는 방법입니다. 특별 한 Api를 사용 하지 않고 직원 정보 및 시간 카드 정보를 동일한 개체에 로드 했습니다. 이 코드는 메모리 내 데이터 원본 및 원격 데이터 원본에 대해 작동 하는 표준 LINQ 연산자를 사용 하 여 필요한 정보만 표시 합니다. EF4는 LINQ 쿼리 및 C\# 컴파일러로 생성 된 식 트리를 하나의 효율적인 T-sql 쿼리로 변환할 수 있었습니다.

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

뷰 모델 또는 DTO 개체를 사용 하지 않고 실제 엔터티를 사용 하 여 작업 하지 않으려는 경우도 있습니다. 직원 *및* 직원의 시간 카드가 필요 하다 고 생각 되 면 관련 데이터를 원활 하 고 효율적인 방식으로 적극적으로 로드할 수 있습니다.

### <a name="explicit-eager-loading"></a>명시적인 즉시 로드

관련 엔터티 정보를 적극적으로 로드 하려면 비즈니스 논리 (또는이 시나리오에서는 컨트롤러 동작 논리)에 대 한 몇 가지 메커니즘을 통해 리포지토리를 원하는 것으로 요구 합니다. EF4 ObjectQuery&lt;T&gt; 클래스는 쿼리 중에 검색할 관련 개체를 지정 하는 Include 메서드를 정의 합니다. EF4 ObjectContext는 ObjectQuery&lt;T&gt;에서 상속 되는 구체적인 ObjectSet&lt;T&gt; 클래스를 통해 엔터티를 노출 합니다.  컨트롤러 작업에서 ObjectSet&lt;T&gt; 참조를 사용 하는 경우 다음 코드를 작성 하 여 각 직원의 시간 카드 정보에 대 한 즉시 로드를 지정할 수 있습니다.

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

그러나 코드를 테스트 가능 하 게 유지 하려고 하기 때문에 실제 작업 단위 클래스 외부에서&lt;T&gt;의 ObjectSet을 노출 하지 않습니다. 대신, 가짜 하기가 더 쉬운 IObjectSet&lt;T&gt; 인터페이스를 사용 하지만 IObjectSet&lt;T&gt;는 Include 메서드를 정의 하지 않습니다. LINQ의 장점은 자체 Include 연산자를 만들 수 있다는 것입니다.

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

이 Include 연산자는 IObjectSet&lt;T&gt;대신 IQueryable&lt;T&gt;에 대 한 확장 메서드로 정의 됩니다. 이를 통해 IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;및 ObjectSet&lt;T&gt;를 비롯 하 여 더 광범위 한 형식으로 메서드를 사용할 수 있습니다. 기본 시퀀스가 진짜 EF4 ObjectQuery&lt;T&gt;가 아닌 경우에는 피해를 주지 않으며 Include 연산자는 작동 하지 않습니다. 기본 시퀀스가 ObjectQuery&lt;T&gt; 이거나 ObjectQuery&lt;T&gt;에서 파생 *됨* ) 이면 추가 데이터에 대 한 요구 사항이 표시 되 고 적절 한 SQL 쿼리가 작성 됩니다.

이 new 연산자를 사용 하면 리포지토리에서 시간 카드 정보를 즉시 로드할 수 있습니다.

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

실제 ObjectContext에 대해 실행 하는 경우 코드에서 다음과 같은 단일 쿼리를 생성 합니다. 이 쿼리는 직원 개체를 구체화 하 고 해당 TimeCards 속성을 완전히 채울 수 있도록 데이터베이스에서 충분 한 정보를 한 번에 수집 합니다.

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

작업 메서드 내의 코드는 완전히 테스트 가능한 상태로 유지 됩니다. Include 연산자를 지원 하기 위해 fakes에 대 한 추가 기능을 제공 하지 않아도 됩니다. 잘못 된 뉴스는 지 속성 무시를 유지 하기 위해 필요한 코드 내부에서 Include 연산자를 사용 해야 했습니다. 이는 테스트 가능한 코드를 빌드할 때 평가 해야 하는 장단점의 대표적인 예입니다. 성능 목표를 충족 하기 위해 지 속성 문제가 리포지토리 추상화 외부에서 누출 되도록 해야 하는 경우가 있습니다.

즉시 로드 하는 대신 지연 로드를 사용할 수 있습니다. 지연 로드는 연결 된 데이터에 대 한 요구 사항을 명시적으로 알리기 위한 비즈니스 코드가 필요 *하지* 않음을 의미 합니다. 대신 응용 프로그램에서 엔터티를 사용 하 고 추가 데이터가 필요한 경우에는 필요에 따라 데이터를 로드 Entity Framework 합니다.

### <a name="lazy-loading"></a>지연 로드

비즈니스 논리에 필요한 데이터를 모르는 시나리오를 쉽게 생각해 볼 수 있습니다. 논리에 employee 개체가 필요 하다는 것을 알 수 있지만, 이러한 경로 중 일부에는 직원의 시간 카드 정보가 필요 하 고 일부는 다른 실행 경로를 분기할 수 있습니다. 이와 같은 시나리오는 데이터 magically 필요에 따라 나타나므로 암시적 지연 로드에 적합 합니다.

지연 로드 (지연 된 로드 라고도 함)는 엔터티 개체에 대 한 몇 가지 요구 사항을 충족 합니다. 진정한 지 속성 무시을 포함 하는 POCOs는 지 속성 계층의 요구 사항을 충족 하지 않지만 진정한 지 속성 무시는 실제로 달성할 수 없습니다.  대신 지 속성 무시을 상대적 각도로 측정 합니다. 지 속성 지향 기본 클래스에서 상속 하거나 특수화 된 컬렉션을 사용 하 여 POCOs에서 지연 로드를 수행 해야 하는 경우에는 바람직하지 않습니다. 다행히 EF4는 더 저렴 한 솔루션을 포함 합니다.

### <a name="virtually-undetectable"></a>거의 검색할 때 없음

POCO 개체를 사용 하는 경우 EF4는 엔터티에 대 한 런타임 프록시를 동적으로 생성할 수 있습니다. 이러한 프록시는 구체화 된 POCOs를 래핑하여 추가 작업을 수행 하기 위해 각 속성 get 및 set 작업을 가로채 여 추가 서비스를 제공 합니다. 이러한 서비스 중 하나는 원하는 지연 로드 기능입니다. 다른 서비스는 프로그램에서 엔터티의 속성 값을 변경할 때 기록할 수 있는 효율적인 변경 내용 추적 메커니즘입니다. 변경 목록은 SaveChanges 메서드에서 UPDATE 명령을 사용 하 여 수정 된 엔터티를 유지 하기 위해 ObjectContext에서 사용 됩니다.

그러나 이러한 프록시가 작동 하려면 엔터티에 대 한 속성 get 및 set 작업에 연결 하는 방법이 필요 하며, 프록시는 가상 멤버를 재정의 하 여이 목표를 달성 합니다. 따라서 암시적 지연 로드 및 효율적인 변경 내용 추적을 원할 경우 POCO 클래스 정의로 돌아가서 속성을 가상으로 표시 해야 합니다.

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

직원 엔터티가 대부분 지 속성을 무시 하는 것으로 말할 수도 있습니다. 유일한 요구 사항은 가상 멤버를 사용 하는 것 이며,이는 코드의 테스트 용이성에 영향을 주지 않습니다. 특별 한 기본 클래스에서 파생 되거나 지연 로드 전용 특수 컬렉션을 사용할 필요가 없습니다. 코드에서 설명 하는 것 처럼 ICollection&lt;T&gt;를 구현 하는 모든 클래스는 관련 엔터티를 보유 하는 데 사용할 수 있습니다.

작업 단위 내에서 수행 해야 하는 한 가지 사소한 변경도 있습니다. ObjectContext 개체로 직접 작업할 때 지연 로드는 기본적으로 *해제* 되어 있습니다. 지연 된 로드를 사용 하도록 ContextOptions 속성에 설정할 수 있는 속성이 있으며, 모든 위치에서 지연 로드를 사용 하도록 설정 하려는 경우 실제 작업 단위 내에서이 속성을 설정할 수 있습니다.

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

암시적 지연 로드를 사용 하는 경우 응용 프로그램 코드는 추가 데이터를 로드 하는 EF에 필요한 작업을 못한다는 하 고 나머지는 직원 및 직원의 연결 된 시간 카드를 사용할 수 있습니다.

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

지연 로드를 사용 하면 응용 프로그램 코드를 더 쉽게 작성할 수 있으며 프록시 매직을 사용 하면 코드를 완전히 테스트할 수 있습니다. 작업 단위에 대 한 메모리 내 fakes는 테스트 중에 필요한 경우 연결 된 데이터를 사용 하 여 가짜 엔터티를 미리 로드할 수 있습니다.

이 시점에서 IObjectSet&lt;T&gt;를 사용 하 여 리포지토리를 작성 하 고, 지 속성 프레임 워크의 모든 기호를 숨기는 추상화를 살펴볼 것입니다.

## <a name="custom-repositories"></a>사용자 지정 리포지토리

이 문서에서 작업 단위 디자인 패턴을 처음 제공할 때 작업 단위가 표시 될 수 있는 몇 가지 샘플 코드를 제공 했습니다. 우리가 작업 하 고 있는 직원 및 직원 시간 카드 시나리오를 사용 하 여이 원래 아이디어를 다시 제공 해 보겠습니다.

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

이 작업 단위와 마지막 섹션에서 만든 작업 단위 간의 주요 차이점은이 작업 단위가 EF4 프레임 워크의 추상화를 사용 하지 않는 것입니다 (IObjectSet&lt;T&gt;). IObjectSet&lt;T&gt;는 리포지토리 인터페이스와 잘 작동 하지만 노출 하는 API는 응용 프로그램의 요구 사항에 완벽 하 게 맞지 않을 수 있습니다. 이 예정 된 방법에서는 사용자 지정 IRepository&lt;T&gt; 추상화를 사용 하 여 리포지토리를 나타냅니다.

테스트 기반 디자인, 동작 중심 디자인 및 도메인 기반 방법론 디자인을 따르는 개발자는 여러 가지 이유로 IRepository&lt;T&gt; 접근법을 선호 합니다. 먼저 IRepository&lt;T&gt; 인터페이스는 "손상 방지" 계층을 나타냅니다. 도메인 기반 디자인 설명서에서 Eric Evans에 설명 된 대로 손상 방지 계층은 지 속성 API와 같은 인프라 Api에서 도메인 코드를 유지 합니다. 둘째로, 개발자는 테스트를 작성 하는 동안 검색 된 대로 응용 프로그램의 정확한 요구를 충족 하는 메서드를 리포지토리에 빌드할 수 있습니다. 예를 들어 ID 값을 사용 하 여 단일 엔터티를 찾아야 하는 경우가 많으므로 리포지토리 인터페이스에 FindById 메서드를 추가할 수 있습니다.  IRepository&lt;T&gt; 정의는 다음과 같습니다.

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

여기서는 IQueryable&lt;T&gt; 인터페이스를 사용 하 여 엔터티 컬렉션을 노출 하도록 다시 삭제 합니다. IQueryable&lt;T&gt;를 사용 하면 LINQ 식 트리가 EF4 공급자로 이동 하 고 공급자에 게 쿼리를 전체적으로 볼 수 있습니다. 두 번째 옵션은 IEnumerable&lt;T&gt;를 반환 하는 것입니다. 즉, EF4 LINQ 공급자는 리포지토리 내부에서 빌드된 식만 볼 수 있습니다. 리포지토리 외부에서 수행 되는 모든 그룹화, 정렬 및 프로젝션은 데이터베이스로 전송 된 SQL 명령으로 구성 되지 않으므로 성능이 저하 될 수 있습니다. 반면에 IEnumerable&lt;T&gt; 결과만 반환 하는 리포지토리는 새 SQL 명령을 사용 하지 않습니다. 두 방법을 모두 사용할 수 있으며 두 가지 방법을 모두 테스트할 수 있습니다.

제네릭과 EF4 ObjectContext API를 사용 하 여 IRepository&lt;T&gt; 인터페이스의 단일 구현을 제공 하는 것이 간단 합니다.

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

클라이언트에서 엔터티를 가져오기 위해 메서드를 호출 해야 하기 때문에 IRepository&lt;T&gt; 접근 방식은 쿼리를 추가로 제어 합니다. 메서드 내에서 추가 검사 및 LINQ 연산자를 제공 하 여 응용 프로그램 제약 조건을 적용할 수 있습니다. 인터페이스에는 제네릭 형식 매개 변수에 대 한 두 개의 제약 조건이 있습니다. 첫 번째 제약 조건은 ObjectSet&lt;T&gt;에 필요한 클래스 단점입니다. 두 번째 제약 조건은 엔터티가 IEntity (응용 프로그램에 대해 생성 된 추상화)를 구현 하도록 합니다. IEntity 인터페이스를 사용 하면 엔터티는 읽을 수 있는 Id 속성을 갖게 되 고 FindById 메서드에서이 속성을 사용할 수 있습니다. IEntity는 다음 코드를 사용 하 여 정의 됩니다.

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

엔터티는이 인터페이스를 구현 하는 데 필요 하므로 IEntity는 지 속성 무시의 작은 위반으로 간주 될 수 있습니다. 지 속성 무시은 절충에 대 한 것 이며, 대부분의 FindById 기능은 인터페이스에 의해 적용 되는 제약 조건 보다 더 큽니다. 인터페이스는 테스트 용이성에 영향을 주지 않습니다.

&lt;T&gt; 라이브 IRepository를 인스턴스화하면 EF4 ObjectContext가 필요 하므로 구체적인 작업 단위 구현은 인스턴스화를 관리 해야 합니다.

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

### <a name="using-the-custom-repository"></a>사용자 지정 리포지토리 사용

사용자 지정 리포지토리를 사용 하는 것은 IObjectSet&lt;T&gt;기반 리포지토리를 사용 하는 것과 크게 다르지 않습니다. LINQ 연산자를 속성에 직접 적용 하는 대신, 먼저 리포지토리의 메서드 하나를 호출 하 여 IQueryable&lt;T&gt; 참조를 잡기를 수행 해야 합니다.

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

이전에 구현한 custom Include 연산자는 변경 하지 않고 작동 합니다. 리포지토리의 FindById 메서드는 단일 엔터티를 검색 하려는 작업에서 중복 된 논리를 제거 합니다.

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

검사 한 두 방법의 테스트 용이성에는 중요 한 차이점이 없습니다. 마지막 섹션에서 했던 것과 마찬가지로, HashSet&lt;직원&gt;에서 지 원하는 구체적인 클래스를 빌드하여 IRepository&lt;T&gt;의 가짜 구현을 제공할 수 있습니다. 그러나 일부 개발자는 fakes를 빌드하는 대신 모의 개체와 모의 개체 프레임 워크를 사용 하는 것이 좋습니다. Mock를 사용 하 여 구현을 테스트 하 고 다음 섹션에서 mock와 fakes의 차이점에 대해 살펴보겠습니다.

### <a name="testing-with-mocks"></a>Mock로 테스트

Martin Fowler에서 "test double"을 호출 하는 방법을 다양 하 게 빌드할 수 있습니다. Test double (예: movie stunt double)은 테스트 하는 동안 실제 프로덕션 개체의 "독립"에 대해 빌드하는 개체입니다. 만든 메모리 내 리포지토리는 SQL Server와 통신 하는 리포지토리에 대 한 테스트 double입니다. 단위 테스트 중에 이러한 두 가지 테스트를 사용 하 여 코드를 격리 하 고 테스트를 신속 하 게 실행 하는 방법을 살펴보았습니다.

빌드된 두 배의 테스트에는 진정한 작업 구현이 있습니다. 내부적으로는 개체의 구체적인 컬렉션을 저장 하 고 테스트 중에 리포지토리를 조작할 때이 컬렉션에서 개체를 추가 및 제거 합니다. 이러한 방식으로 테스트를 빌드하는 개발자는 실제 코드와 작업을 구현 하는 것이 가능 합니다.  이러한 두 double 테스트는 *fakes*를 호출 하는 것입니다. 이러한 구현에는 작동 하는 구현이 있지만 프로덕션이 사용 하기에 충분 하지 않습니다. 가짜 리포지토리는 실제로 데이터베이스에 쓰지 않습니다. 가짜 SMTP 서버는 실제로 네트워크를 통해 전자 메일 메시지를 보내지 않습니다.

### <a name="mocks-versus-fakes"></a>Mock와 Fakes 비교

*모의*형식으로 알려진 다른 형식의 테스트 double이 있습니다. Fakes에는 작업 구현이 있지만 mock는 구현 없이 제공 됩니다. 모의 개체 프레임 워크의 도움을 통해 런타임에 이러한 모의 개체를 구성 하 고 테스트 double로 사용 합니다. 이 섹션에서는 오픈 소스 모의 프레임 워크 Moq를 사용 합니다. 다음은 Moq를 사용 하 여 직원 리포지토리에 대해 테스트 double을 동적으로 만드는 간단한 예입니다.

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

IRepository&lt;Employee&gt; 구현에 대해 Moq를 요청 하 고이를 동적으로 작성 합니다. 모의&lt;T&gt; 개체의 개체 속성에 액세스 하 여 IRepository&lt;Employee&gt;를 구현 하는 개체를 가져올 수 있습니다. 이 개체는 컨트롤러에 전달할 수 있는 내부 개체 이며 테스트 double 또는 실제 리포지토리입니다. 실제 구현을 사용 하 여 개체에서 메서드를 호출 하는 것 처럼 개체에서 메서드를 호출할 수 있습니다.

Add 메서드를 호출할 때 모의 리포지토리에서 수행할 작업을 알고 있어야 합니다. 모의 개체 뒤에는 구현이 없으므로 추가는 아무 작업도 수행 하지 않습니다. 우리가 작성 한 fakes와 마찬가지로 내부적으로는 구체적인 컬렉션이 없으므로 직원은 무시 됩니다. FindById의 반환 값은 무엇 인가요? 이 경우 모의 개체는 기본값을 반환 하는 유일한 작업을 수행 합니다. 참조 형식 (직원)을 반환 하기 때문에 반환 값은 null 값입니다.

Mock는 쓸모가 없을 수 있습니다. 그러나 두 가지 이상의 모의 기능이 제공 됩니다. 먼저 Moq 프레임 워크는 모의 개체에 대해 수행 된 모든 호출을 기록 합니다. 코드에서 나중에 다른 사용자가 Add 메서드를 호출 하거나 FindById 메서드를 호출한 경우 Moq를 요청할 수 있습니다. 테스트에서이 "블랙 박스" 기록 기능을 사용 하는 방법에 대해 나중에 살펴보겠습니다.

두 번째 유용한 기능은 Moq를 사용 하 여 *기대*수준에서 모의 개체를 프로그래밍 하는 방법입니다. 예상은 모의 개체에 지정 된 상호 작용에 응답 하는 방법을 알려 줍니다. 예를 들어, mock에 기대 하는 사항을 프로그래밍 하 고 누군가가 FindById을 호출할 때 employee 개체를 반환 하도록 지시할 수 있습니다. Moq 프레임 워크는 설치 API 및 람다 식을 사용 하 여 이러한 기대를 프로그래밍 합니다.

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

이 샘플에서는 리포지토리를 동적으로 빌드하기 위해 Moq를 요청 하 고 예상 대로 리포지토리를 프로그래밍 합니다. 예상은 사용자가 값 5를 전달 하는 FindById 메서드를 호출할 때 Id 값이 5 인 새 employee 개체를 반환 하도록 모의 개체에 지시 합니다. 이 테스트는 통과 하 고&lt;T&gt;가짜 IRepository에 전체 구현을 빌드할 필요가 없습니다.

이전에 작성 한 테스트를 다시 방문 하 고 fakes 대신 mock를 사용 하도록 재작업 해 보겠습니다. 이전과 마찬가지로 기본 클래스를 사용 하 여 모든 컨트롤러의 테스트에 필요한 인프라의 공통 부분을 설정 합니다.

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

설치 코드는 거의 동일 하 게 유지 됩니다. Fakes를 사용 하는 대신 Moq를 사용 하 여 모의 개체를 생성 합니다. 기본 클래스는 코드가 Employees 속성을 호출할 때 모의 리포지토리를 반환할 모의 작업 단위를 정렬 합니다. 모의 설치의 나머지 부분은 각 특정 시나리오에 전용으로 사용 되는 테스트 설비 내에서 발생 합니다. 예를 들어 작업에서 모의 리포지토리의 FindAll 메서드를 호출할 때 인덱스 작업에 대 한 테스트 fixture는 모의 리포지토리를 설정 하 여 직원 목록을 반환 합니다.

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

기대를 제외 하 고 테스트는 이전에 수행한 테스트와 비슷합니다. 그러나 모의 프레임 워크의 기록 기능을 사용 하면 다른 각도에서 테스트를 수행할 수 있습니다. 다음 섹션에서이 새로운 큐브 뷰를 살펴보겠습니다.

### <a name="state-versus-interaction-testing"></a>상태 및 상호 작용 테스트

모의 개체로 소프트웨어를 테스트 하는 데 사용할 수 있는 여러 기술이 있습니다. 한 가지 방법은 지금까지이 문서에서 수행한 작업 인 상태 기반 테스트를 사용 하는 것입니다. 상태 기반 테스트는 소프트웨어 상태에 대 한 어설션을 만듭니다. 마지막 테스트에서 컨트롤러에 대 한 작업 메서드를 호출 하 고 빌드해야 하는 모델에 대 한 어설션을 만들었습니다. 테스트 상태의 몇 가지 다른 예는 다음과 같습니다.

-   만들기가 실행 된 후 리포지토리에 새 employee 개체가 포함 되어 있는지 확인 합니다.
-   인덱스가 실행 된 후 모델에 모든 직원 목록이 포함 되어 있는지 확인 합니다.
-   삭제를 실행 한 후 리포지토리에 지정 된 직원이 포함 되지 않았는지 확인 합니다.

모의 개체로 표시 되는 다른 방법은 *상호 작용*을 확인 하는 것입니다. 상태 기반 테스트는 개체의 상태에 대 한 어설션을 만들며, 상호 작용 기반 테스트는 개체가 상호 작용 하는 방식에 대 한 어설션을 만듭니다. 예를 들면 다음과 같습니다.

-   만들기가 실행 될 때 컨트롤러가 리포지토리의 Add 메서드를 호출 하는지 확인 합니다.
-   인덱스가 실행 될 때 컨트롤러가 리포지토리의 FindAll 메서드를 호출 하는지 확인 합니다.
-   편집이 실행 될 때 컨트롤러에서 작업의 커밋 메서드 단위를 호출 하 여 변경 내용을 저장 하는지 확인 합니다.

상호 작용 테스트에는 컬렉션 내부에 poking 수를 확인 하지 않으므로 테스트 데이터가 더 적습니다. 예를 들어 세부 작업에서 올바른 값을 사용 하 여 리포지토리의 FindById 메서드를 호출 하는 경우 동작이 올바르게 동작 하는 것입니다. FindById에서 반환할 테스트 데이터를 설정 하지 않고이 동작을 확인할 수 있습니다.

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

위의 테스트 fixture에 필요한 유일한 설정은 기본 클래스에서 제공 하는 설정입니다. 컨트롤러 작업을 호출 하면 Moq가 모의 리포지토리와의 상호 작용을 기록 합니다. Moq의 Verify API를 사용 하 여 컨트롤러에서 FindById를 호출한 경우 적절 한 ID 값으로 Moq를 요청할 수 있습니다. 컨트롤러에서 메서드를 호출 하지 않았거나 예기치 않은 매개 변수 값을 사용 하 여 메서드를 호출한 경우에는 Verify 메서드에서 예외가 throw 되 고 테스트가 실패 합니다.

다음은 만들기 작업에서 현재 작업 단위에 대해 Commit을 호출 하는지 확인 하는 또 다른 예입니다.

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

상호 작용 테스트의 한 가지 위험은 상호 작용을 지정 하는 추세입니다. 모의 개체의 모든 상호 작용을 기록 하 고 확인 하는 모의 개체의 기능은 테스트가 모든 상호 작용을 확인 하는 것을 의미 하지는 않습니다. 일부 상호 작용은 구현 세부 정보 이며 현재 테스트를 충족 하는 데 *필요한* 상호 작용을 확인 해야 합니다.

Mock 또는 fakes 중에서 선택 하는 항목은 주로 테스트 하는 시스템과 개인 (또는 팀) 기본 설정에 따라 달라 집니다. 모의 개체는 테스트를 두 배로 구현 하는 데 필요한 코드의 양을 크게 줄일 수 있지만, 모든 사람이 편안한 프로그래밍을 기대 하 고 상호 작용을 확인 하는 것은 아닙니다.

## <a name="conclusions"></a>결론

이 문서에서는 ADO.NET Entity Framework를 사용 하 여 데이터 지 속성에 대해 테스트 가능한 코드를 만드는 몇 가지 방법을 보여 주었습니다. IObjectSet&lt;T&gt;와 같은 기본 제공 추상화를 활용 하거나 IRepository&lt;T&gt;와 같은 고유한 추상화를 만들 수 있습니다.  두 경우 모두 ADO.NET Entity Framework 4.0의 POCO 지원을 통해 이러한 추상화를 지속적으로 무시 하 고 매우 테스트할 수 있습니다. 암시적 지연 로드와 같은 추가 EF4 기능을 사용 하면 관계형 데이터 저장소의 세부 정보를 걱정 하지 않고 비즈니스 및 응용 프로그램 서비스 코드를 사용할 수 있습니다. 마지막으로, 만든 추상화는 단위 테스트 내에서 모의 이거나 가짜를 쉽게 수행할 수 있으며, 이러한 테스트 double을 사용 하 여 신속 하 게 실행 되 고 매우 격리 되 고 신뢰할 수 있는 테스트를 수행할 수 있습니다.

### <a name="additional-resources"></a>추가 리소스

-   Robert C. Martin, " [단일 책임 원칙](https://www.objectmentor.com/resources/articles/srp.pdf)"
-   Martin Fowler, *엔터프라이즈 응용 프로그램 아키텍처 패턴* 의 [패턴 카탈로그](https://www.martinfowler.com/eaaCatalog/index.html)
-   Griffin Caprio, " [종속성 주입](https://msdn.microsoft.com/magazine/cc163739.aspx)"
-   데이터 프로그래밍 블로그, " [연습: Entity Framework 4.0를 사용 하는 테스트 기반 개발](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)"
-   데이터 프로그래밍 블로그, " [Entity Framework 4.0에서 리포지토리 및 작업 단위 패턴 사용](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"
-   Aaron Jensen, " [컴퓨터 사양 소개](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"
-   Eric Lee, " [MSTest를 사용](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)하는 BDD"
-   Eric Evans, " [도메인 기반 디자인](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"
-   Martin Fowler, " [모의는 스텁이 아닙니다](https://martinfowler.com/articles/mocksArentStubs.html)."
-   Martin Fowler, " [Test Double](https://martinfowler.com/bliki/TestDouble.html)"
-   [Moq](https://code.google.com/p/moq/)

### <a name="biography"></a>소개

Scott Allen는 Pluralsight 및 창립자 OdeToCode.com의 기술 직원의 멤버입니다. 상업적 소프트웨어 개발의 15 년 동안 Scott은 8 비트 embedded 장치에서 확장성이 뛰어난 ASP.NET 웹 응용 프로그램을 위한 솔루션에 대해 노력 했습니다. OdeToCode의 블로그 또는 [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode)에서 Scott에 게 연락할 수 있습니다.
