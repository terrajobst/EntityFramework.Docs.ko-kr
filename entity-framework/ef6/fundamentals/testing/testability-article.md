---
title: 테스트 용이성 및 Entity Framework 4.0
author: divega
ms.date: 2016-10-23
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 17a9f09022531a81042979464de05fbbd2570759
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995231"
---
# <a name="testability-and-entity-framework-40"></a>테스트 용이성 및 Entity Framework 4.0
Scott Allen

게시일: 2010 년 5 월

## <a name="introduction"></a>소개

이 백서에서는 설명 하 고 ADO.NET Entity Framework 4.0 및 Visual Studio 2010을 사용 하 여 테스트 가능한 코드를 작성 하는 방법에 설명 합니다. 이 문서에서는 테스트 기반 디자인 (TDD) 또는 동작 기반 디자인 (BDD)와 같은 특정 테스트 방법론을 중점적으로 시도 하지 않습니다. 대신이 문서는 방법을 중점적으로 ADO.NET Entity Framework를 사용 하 여 아직 격리 하 고 자동화 된 방식으로 테스트를 쉽게 유지 하는 코드를 작성 합니다. 데이터에서 액세스 시나리오를 테스트 프레임 워크를 사용 하는 경우 이러한 패턴을 적용 하는 방법을 참조 하는 일반적인 디자인 패턴에서 살펴보겠습니다. 테스트 가능한 코드에서 이러한 기능 수 어떻게 표시 되는지 확인 하기 위해 프레임 워크의 특정 기능에도 살펴보겠습니다.

## <a name="what-is-testable-code"></a>테스트 가능한 코드 란?

자동화 된 단위 테스트를 사용 하 여 소프트웨어를 확인 하는 기능에는 많은 바람직하지 이점을 제공 합니다. 모든 사용자는 좋은 테스트는 응용 프로그램 및 응용 프로그램의 품질을 위치에 단위 테스트를 포함 하지만 넘어서 버그를 발견 하는 바로 증가 소프트웨어 결함 수가 감소할 것을 알고 있습니다.

훌륭한 단위 테스트 도구 모음 개발 팀을 시간을 절약 하 여 자신이 만든 소프트웨어의 컨트롤에 남아 있습니다. 팀을 기존 코드를 리팩터링 재설계 변경할 수 있으며 재구성 소프트웨어 테스트 도구 모음을 파악 하는 동안 모든 응용 프로그램에 새 구성 요소를 추가한 새로운 요구 사항을 충족 하는 응용 프로그램의 동작을 확인할 수 있습니다. 단위 테스트는 일부 변경 용이 하 게 소프트웨어의 복잡성 증가 함에 따라 유지 관리 용이성을 유지 하는 빠른 피드백 주기입니다.

하지만 단위 테스트는 가격에 포함 되어 있습니다. 팀에서 만들고 단위 테스트를 유지 관리 하는 시간을 투자 해야 합니다. 이러한 테스트를 만드는 데 필요한 작업량에 직접 연관 됩니다 합니다 **테스트 용이성** 기본 소프트웨어입니다. 소프트웨어 테스트를 얼마나 기능 염두에서 테스트 용이성을 사용 하 여 소프트웨어를 설계 팀이 만들어집니다 효율적인 테스트 되지 않은 테스트 가능 소프트웨어를 사용 하 여 작업 팀 보다 빠릅니다.

테스트 용이성 염두에서를 사용 하 여 ADO.NET Entity Framework 4.0 (EF4) 설계 되었습니다. 개발자를 작성 하는 단위 테스트 프레임 워크 코드 자체에 대 한 것은 아닙니다. 대신 EF4의 테스트 용이성 목표 쉽게 프레임 워크를 기반으로 구축 하는 테스트 가능한 코드를 만듭니다. 특정 예제를 살펴보기 전에 테스트 가능한 코드의 품질을 이해 하는 것이 좋습니다.

### <a name="the-qualities-of-testable-code"></a>테스트 가능한 코드의 품질을

테스트할 쉬운 코드를 항상 두 개 이상의 특성을 나타냅니다. 첫째, 테스트 코드는 쉽게 **관찰**합니다. 일부 입력 집합을 지정 하는, 쉽게 코드의 출력을 알 수 여야 합니다. 예를 들어 다음 메서드를 테스트 이므로 쉽게 메서드는 직접 계산의 결과 반환 합니다.

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

메서드를 테스트 하는 것은 메서드는 네트워크 소켓, 데이터베이스 테이블에 또는 다음 코드와 같이 파일에 계산 된 값을 기록 하는 경우 어렵습니다. 테스트 값을 검색 하려면 추가 작업을 수행 해야 합니다.

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

둘째, 테스트 가능한 코드 하기 쉽습니다 **격리**합니다. 테스트 가능한 코드의 잘못 된 예로 다음 의사 코드를 사용해 보겠습니다.

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

메서드는 쉽게 알 수 – 보험 정책에서 전달 하 고 반환 값에 예상된 결과 일치 하는지 확인 수 있습니다. 그러나 메서드를 테스트 하 해야 올바른 스키마를 사용 하 여 설치 되어 있고 메서드 전자 메일을 보내려고 시도 하는 경우 SMTP 서버를 구성 합니다.

단위 테스트는만 메서드 내부에서 계산 논리를 확인 하려고 하지만 테스트 전자 메일 서버가 오프 라인 상태 이므로 있거나 데이터베이스 서버 이동 때문에 실패할 수 있습니다. 이러한 오류의 모두 테스트 확인 하려는 동작에 관련 되지 않습니다. 동작을 격리 하기 어렵습니다.

자주 테스트 가능한 코드를 작성 하기 위해 노력 하는 소프트웨어 개발자는 작성 코드의 중요 한 부분의 분리를 유지 하기 위해 노력 합니다. 위의 메서드는 비즈니스 계산에 집중 하 고 다른 구성 요소에는 데이터베이스 및 메일 구현 세부 정보를 위임 해야 합니다. 단일 책임 원칙 Robert C. Martin이를 호출합니다. 개체 값을 계산한 다음 정책의 같은 단일, 좁은 책임을 캡슐화 해야 합니다. 다른 모든 데이터베이스 및 알림 작업에는 몇 가지 다른 개체의 책임 이어야 합니다. 이 방식으로 작성 된 코드 한 작업에 중점을 둡니다 때문에 격리 하기 쉽습니다.

.NET에서 단일 책임 원칙을 따르고 격리 해야 하는 추상화 했습니다. 인터페이스 정의 사용 하 고 구체적인 형식 대신 인터페이스 추상화를 사용 하는 코드를 강제로 수 있습니다. 이 문서의 뒷부분에서 살펴보겠습니다 위에서 설명한 잘못 된 예제에서는 작업을 수행할 수와 같은 메서드는 인터페이스 하는 방법을 *찾습니다* 데이터베이스로 통신할 등. 그러나 테스트 시간에는 데이터베이스와 통신 하지 않습니다 하지만 대신 메모리에 데이터를 보유 하는 더미 구현을 대체할 수 했습니다. 이 더미 구현인 데이터 액세스 코드 또는 데이터베이스 구성 관련된 문제 로부터 코드를 격리 됩니다.

격리로 추가 이점이 있습니다. 마지막 메서드에서 비즈니스 계산을 실행 하려면 몇 밀리초가 소요만 해야 하지만 다양 한 서버에 자체 테스트 네트워크 및 통신 관련 코드 홉으로 몇 초 동안 실행 될 수 있습니다. 단위 테스트는 신속 하 게 한 작은 변경 사항은 용이 하 게 실행 해야 합니다. 단위 테스트는 또한 반복을 가능 하 고 테스트와 관련 없는 구성 요소에 문제가 발생 하므로 실패 하지 해야 합니다. 관찰을 격리 하기 쉬운 코드를 개발자가 더 쉽게 테스트 코드를 작성 의미를 실행 하려면 테스트에 대 한 대기 시간을 줄이고 쓰고, 더 중요 한 점은 시간이 절약 존재 하지 않는 버그를 추적 합니다.

바랍니다 테스트의 이점을 이해 하 고 테스트 가능한 코드를 보여 주는 품질을 이해할 수 있습니다. Observable 및 격리를 쉽게 유지 하면서 데이터베이스에 데이터를 저장 하는 EF4 작동 하는 코드를 작성 하는 방법에 있지만 먼저 데이터 액세스를 위한 테스트 가능 설계를 설명 합니다. 이번 좁힐 수 했습니다.

## <a name="design-patterns-for-data-persistence"></a>데이터 지 속성에 대 한 디자인 패턴

앞서 소개한 잘못 된 예제 모두에 너무 많은 책임을 담당 했습니다. 잘못 된 첫 번째 예제에서는 계산을 수행 해야 *및* 파일로 작성 합니다. 잘못 된 두 번째 예제 데이터베이스에서 데이터를 읽을 수 있었습니다 *하 고* 비즈니스 계산을 수행 *및* 전자 메일을 보냅니다. 문제를 분리 하 고 다른 구성 요소에 대 한 책임을 위임 하는 작은 메서드를 디자인 하 여 테스트 가능한 코드 작성으로 멀었다고 해야 합니다. 목표 작고 집중 된 추상화에서 작업을 작성 하 여 기능을 구축 하는 것입니다.

작은 데이터 지 속성을 제공 하 고 살펴봅니다 포커스가 있는 추상화가 매우 일반적 이므로 디자인 패턴으로 문서화 된 했습니다. Martin Fowler의 책 엔터프라이즈 응용 프로그램 아키텍처 패턴에는 인쇄에서 이러한 패턴을 설명 하는 첫 번째 작업 이었습니다. 이러한 ADO.NET Entity Framework 구현 하 고 이러한 패턴을 사용 하 여 작동 하는 방법을 보여 줍니다 전에 다음 섹션에서 이러한 패턴의 한 간단한 설명을 제공 합니다.

### <a name="the-repository-pattern"></a>리포지토리 패턴

Fowler는 리포지토리 "간을 중재 도메인과 데이터 도메인 개체에 액세스 하기 위한 컬렉션 유형의 인터페이스를 사용 하 여 매핑 계층"을 표시 합니다. 리포지토리 패턴의 목적은 데이터 액세스 러에서 코드를 격리 하는 것 이며 이전 격리 살펴본 것 처럼 테스트 용이성에 대 한 필수 특성입니다.

격리 됨을 키는 저장소 컬렉션 유형의 인터페이스를 사용 하 여 개체를 노출 하는 방법입니다. 저장소에 있는 모릅니다 방법을 사용 하 여 쓸 논리 저장소 요청 개체를 구체화 합니다. 리포지토리 데이터베이스와 통신할 수 있습니다 또는 개체를 메모리 내 컬렉션에서만 반환할 수 있습니다. 코드 알아야 할 모든는 컬렉션을 유지 하기 위해 표시 되는 저장소 및 검색, 추가 및 컬렉션에서 개체를 삭제할 수 있습니다.

기존.NET 응용 프로그램의 구체적인 리포지토리 종종 다음과 같은 일반 인터페이스에서 상속합니다.

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

EF4에 대 한 구현을 제공 하지만 기본 개념 동일 하 게 유지 하는 경우 인터페이스 정의에 몇 가지 변경을 수행 됩니다. 코드는이 인터페이스를 구현 하는 구체적인 리포지토리를 사용 하 여 해당 기본 키 값이 조건자의 평가에 따라 엔터티 컬렉션을 검색 하 여 엔터티를 검색 하 하거나 간단히 모든 사용 가능한 엔터티를 검색할 수 있습니다. 또한 코드를 추가 하 고 리포지토리 인터페이스를 통해 엔터티를 제거할 수 있습니다.

코드는 IRepository의 Employee 개체를 매개 변수로 받아 다음 작업을 수행할 수 있습니다.

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

코드는 인터페이스 (IRepository의 직원)를 사용 하므로 인터페이스의 다른 구현을 사용 하 여 코드를 제공할 수 있습니다 했습니다. 하나의 구현에는 Microsoft SQL Server 데이터베이스로 EF4 및 지속 개체에서 지 원하는 구현을 수 있습니다. 메모리 내 직원의 목록 개체로 (테스트 중 사용 하 여 하나) 다른 구현을 백업할 수 있습니다. 인터페이스는 코드에서 격리 하는 데 도움이 됩니다.

IRepository를 확인할 수 있습니다&lt;T&gt; 인터페이스 저장 작업을 노출 하지 않습니다. 기존 개체를 업데이트 하려면 어떻게 해야 했습니다. 저장 작업을 수행할는 IRepository 정의 발견할 수 있습니다 하 고 이러한 리포지토리 구현의 즉시 데이터베이스에 개체를 유지 해야 합니다. 그러나 많은 응용 프로그램에서 개체를 개별적으로 유지 하려고 하지 합니다. 대신 개체에 활기를 아마도 다른 리포지토리에서 비즈니스 활동의 일부로 개체만 수정 하 고 다음 단일 원자성 작업의 일환으로 모든 개체를 유지 하려고 합니다. 다행 스럽게도 이러한 유형의 동작을 허용 하는 패턴입니다.

### <a name="the-unit-of-work-pattern"></a>작업 패턴 단위

Fowler 작업 단위는 "및 유지 관리 개체의 목록을 비즈니스 트랜잭션에 의해 영향을 받는 변경 내용에서 작성 하 고 동시성 문제 조정" 이라고 표시 합니다. 이 경우 리포지토리에서 활용 하 고 변경한 개체 변경 내용을 커밋 하도록 작업 단위의 말하는 경우 변경 내용을 유지할 개체 변경 내용을 추적 하려면 작업의 단위입니다. 모든 리포지토리를 추가 하 고 데이터베이스 및도 관리 삭제에는 개체를 삽입할 새 개체를 작업의 단위 책임 이기도 합니다.

ADO.NET 데이터 집합을 사용 하 여 모든 작업 경험이 있는 경우 다음 이미 수 작업 패턴 단위를 사용 하 여 친숙 한 합니다. ADO.NET 데이터 집합은 업데이트, 삭제 및 삽입 DataRow 개체를 추적할 수 있어 수 (사용 하 여 TableAdapter) 조정 모든 변경 내용이 데이터베이스에 있습니다. 그러나 데이터 집합 개체는 기본 데이터베이스의 연결이 끊긴된 하위 집합을 모델링합니다. 작업 패턴 단위에는 비즈니스 개체 및 데이터 액세스 코드에서 격리 되 고 데이터베이스를 인식 하지 않고 도메인 개체를 사용 하 여 작동 하지만 동일한 동작을 보여 줍니다.

.NET 코드에서 작업 단위를 모델링 하는 추상화는 다음과 같습니다.

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

단일 작업 단위를 확인할 수 있습니다 작업 단위에서 리포지토리에 대 한 참조를 노출 하 여 개체에 비즈니스 트랜잭션 중 구체화 하는 모든 엔터티를 추적할 수가 있습니다. 커밋 메서드의 실제 작업 단위에 대 한 구현은 모든 마법이 일어나는 곳 데이터베이스를 사용 하 여 메모리 내 변경을 조정 합니다. 

코드 수는 IUnitOfWork 참조를 지정 하는, 하나 이상의 저장소에서 검색 하는 비즈니스 개체를 변경할 및 원자성 커밋 작업을 사용 하 여 모든 변경 내용을 저장 합니다.

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a>지연 로드 패턴

Fowler 이름 지연 로드를 사용 하 여 "의 모든 데이터를 포함 하지 않는 개체 필요 하지만 되도록 하는 방법을 알고" 설명. 투명 한 지연 로드 했을 때에 중요 한 기능은 테스트 가능한 비즈니스 코드를 작성 하 고 관계형 데이터베이스를 사용 합니다. 예를 들어 다음 코드를 살펴보세요.

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

시간표 컬렉션 채워지는 방법 두 가지 이유가 있습니다. 다음 중 하나는 직원을 인출 하는 메시지가 표시 되 면 직원 리포지토리 직원의 연결된 시간 카드 정보와 함께 직원을 검색할 쿼리를 발급 하는 경우 관계형 데이터베이스가 일반적으로 JOIN 절을 사용 하 여 쿼리가 필요 하 고 발생할 수는 응용 프로그램 보다 자세한 정보를 검색 해야 합니다. 그렇다면 응용 프로그램이 필요 하지 않습니다 시간표 속성을 연결할?

두 번째 답변 "주문형" 시간표 속성을 로드 하는 것입니다. 이 지연 로드 이므로 암시적 및 비즈니스 논리를 투명 하 게 코드 시간 카드 정보를 검색 하는 특수 한 Api를 호출 하지 않습니다. 코드 시간 카드 정보는 필요한 경우를 가정 합니다. 일반적으로 메서드 호출의 런타임 인터 셉 션을 포함 하는 지연 로드를 사용 하 여 관련 된 몇 가지가 있습니다. 코드를 가로채는 데이터베이스와 통신 및 비즈니스 논리가 비즈니스 논리를 사용 가능한 상태로 두고 시간 카드 정보를 검색 하는 일을 담당 합니다. 이 지연 로드 매직 자세한 테스트 가능한 코드의 결과 및 데이터 검색 작업에서 자체를 격리 하는 비즈니스 코드를 허용 합니다.

지연 로드 단점은 하는 경우 응용 프로그램 *않습니다* 코드는 추가 쿼리 실행 시간 카드 정보가 필요 합니다. 성능이 중요 한 응용 프로그램 또는 응용 프로그램 수의 직원 개체를 반복 실행 하 고 쿼리 실행 시간 카드 (문제를 N + 1이 라 불리는 루프의 각 반복 과정에서 검색할 하지만 대부분의 응용 프로그램에 대 한 중요 한 것은 아닙니다. 쿼리 문제) 지연 로딩은 끌기. 이러한 시나리오에서 응용 프로그램 즉시 가능한 가장 효율적인 방식으로 시간 카드 정보를 로드 하려고 수 있습니다.

다행 스럽게도 살펴보겠습니다 EF4 모두 암시적 지연 로드를 지 원하는 방법과 효율적인 즉시 로드 다음 섹션으로 이동 하 고 이러한 패턴을 구현 했습니다.

## <a name="implementing-patterns-with-the-entity-framework"></a>Entity Framework 사용 하 여 패턴 구현

좋은 소식은 모든 마지막 섹션에서 설명 했 듯이 디자인 패턴은 EF4를 사용 하 여 구현 하기가 단순 합니다. 편집 하 여 직원 및 해당 관련된 시간 카드 정보를 표시 하는 간단한 ASP.NET MVC 응용 프로그램을 사용 하려고 하는 방법을 보여 줍니다. 다음 "plain old CLR object"를 사용 하 여 먼저 (Poco). 

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

이러한 클래스 정의 다른 방법과 EF4의 기능에 살펴봅니다 하지만 지 속성 무시 (PI) 최대한으로 이러한 클래스를 유지 하기 위함입니다 약간 변경 됩니다. PI를 알지 *어떻게*, 심지어 *경우*, 데이터베이스 내에서 거주 하는 경우 상태입니다. PI 및 Poco 테스트 가능 소프트웨어를 사용 하 여 작업과 함께 이동합니다. POCO 접근 방식을 사용 하는 개체에는 덜 제한적, 유연 하 고 있는 데이터베이스 없이 작동 하므로 테스트를 쉽게는입니다.

현재 위치에서 Poco를 사용 하 여 Visual Studio에서 엔터티 데이터 모델 (EDM) 만들 수 있습니다 (그림 1 참조). EDM이 엔터티에 대 한 코드 생성을 사용 하지 않습니다. 대신, 우리 답게 만드는 손으로 엔터티를 사용 하려고 합니다. 데이터베이스 스키마를 생성 하 고 EF4 데이터베이스로 개체를 매핑하는 데 필요한 메타 데이터를 제공 하는 EDM만 사용 됩니다.

![eftest_01](~/ef6/media/eftest-01.jpg)

**그림 1**

참고: 먼저 EDM 모델을 개발 하려는 경우 있기을 깨끗 하 고 EDM에서 POCO 코드를 생성 합니다. 데이터 프로그래밍 팀에서 제공 하는 Visual Studio 2010 확장을 사용 하 여이 수행할 수 있습니다. 확장을 다운로드 하려면 Visual Studio의 도구 메뉴에서 확장 관리자를 시작 하 고 "POCO" (그림 2 참조)에 대 한 온라인 템플릿 갤러리를 검색 합니다. EF에 대 한 사용 가능한 여러 POCO 템플릿이 있습니다. 템플릿 사용에 대 한 자세한 내용은 참조 하세요. " [연습: Entity Framework에 대 한 POCO 템플릿을](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)"입니다.

![eftest_02](~/ef6/media/eftest-02.png)

**그림 2**

시작 점으로이 POCO에서 테스트 가능한 코드에 두 가지 방법을 살펴봅니다. 첫 번째 방법은 호출 EF 접근 방식을 추상화 리포지토리 및 작업 단위를 구현 하는 엔터티 프레임 워크 API를 활용 하므로 합니다. 두 번째 접근 방식에서에서는 고유한 사용자 지정 저장소 추상화를 만들고 장점과 각 접근 방식의 단점 중 하나를 참조 하십시오. EF 방법을 탐색 하 여 시작 하겠습니다.  

### <a name="an-ef-centric-implementation"></a>EF 중심 구현

ASP.NET MVC 프로젝트에서 다음 컨트롤러 작업을 고려해 야 합니다. Employee 개체를 검색 하 고 직원의 자세한 뷰를 표시 하려면 결과 반환 하는 작업입니다.

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

코드를 테스트할 수는? 두 개 이상의 테스트가 방법으로 작업의 동작을 확인 해야 합니다. 먼저 작업 반환 올바른 뷰가 – 테스트 하는 쉬운 확인 하려고 합니다. 에서는 올바른 직원을 검색 하는 작업 및 데이터베이스를 쿼리 하는 코드를 실행 하지 않고이 작업을 수행 하고자 확인 하려면 테스트를 작성 하려고도 합니다. 테스트 중인 코드 격리 하려고 해야 합니다. 격리는 데이터 액세스 코드 또는 데이터베이스 구성의 버그 때문에 테스트 실패 하지 확인 합니다. 테스트에 실패할 경우 일부 하위 수준 시스템 구성 요소가 아니라 컨트롤러 논리에 버그가 있을 것을 알 수 했습니다.

격리를 달성 하기 위해 해야 인터페이스를 제공 하는 것과 같은 몇 가지 추상화 이전 리포지토리 및 작업 단위에 대 한 합니다. 리포지토리 패턴은 도메인 개체 및 데이터 매핑 계층 간을 하도록 해야 합니다. 이 시나리오 EF4 *는* 매핑 데이터 계층 및 이미 i o b j 라는 리포지토리 유사 추상화를 제공&lt;T&gt; (System.Data.Objects 네임 스페이스)에서. 인터페이스 정의 다음과 같습니다.

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

I o b j&lt;T&gt; 개체의 컬렉션 유사 하므로 리포지토리에 대 한 요구 사항을 충족 (IEnumerable을 통해&lt;T&gt;) 추가 하 여 시뮬레이션 된 컬렉션에서 개체를 제거 하는 메서드를 제공 합니다. Attach 및 Detach 메서드 EF4 API의 추가 기능을 제공 합니다. I o b j를 사용 하도록&lt;T&gt; 리포지토리에 대 한 인터페이스와 함께 리포지토리를 바인딩할 단위 작업 추상화를 해야 합니다.

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

이 인터페이스의 구체적 구현 하나에서는 SQL Server에 설명 하 고 쉽게 EF4 ObjectContext 클래스를 사용 하 여 만들 수 있습니다. ObjectContext 클래스는 실제 단위 EF4 API의 작업입니다.

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

가져오기는 i o b j&lt;T&gt; 수명을 ObjectContext 개체의 CreateObjectSet 메서드를 호출 하는 것 만큼 쉽습니다. 백그라운드에서 프레임 워크 메타 데이터를 사용할지 구체적인 ObjectSet를 생성 하는 EDM에 제공 했습니다&lt;T&gt;합니다. 사용 하 여 반환 된 i o b j 하겠습니다&lt;T&gt; 인터페이스도 클라이언트 코드의 테스트 용이성을 유지 하도록 도와줍니다.

구체적인 구현은 프로덕션 환경에서는 유용 하지만 IUnitOfWork 추상화는 테스트를 용이 하 게 사용 하는 방법에 집중 해야 합니다.

### <a name="the-test-doubles"></a>Test Double

컨트롤러 작업을 격리 하려면 실제 작업 단위를 (ObjectContext에서 지원 됨)와 테스트 double 또는 "가짜" 작업 단위를 (메모리 내 작업 수행) 간에 전환 하는 기능이 필요 합니다. 이 유형의 전환을 수행 하는 일반적인 방법은 MVC 컨트롤러 작업의 단위를 생성자 매개 변수로 컨트롤러 작업 단위를 전달 대신 인스턴스화할 수 없습니다 하는 것입니다.

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

위의 코드는 종속성 주입의 예입니다. 컨트롤러 (작업 단위) 종속성의 만들지만 컨트롤러에 종속성 주입에 허용 되지 않습니다. MVC 프로젝트에서 컨트롤 (IoC) 컨테이너의 반전 개념을 사용 하 여 조합 하 여 사용자 지정 컨트롤러 팩터리를 사용 하 여 종속성 주입을 자동화할에 공통적으로 적용 됩니다. 이러한 항목은이 문서의 범위를 벗어납니다 있지만이 문서의 끝에 있는 참조에 다음에서 더 알아볼 수 있습니다.

가짜 단위 테스트를 위해 사용할 수 있는 작업 구현의 다음과 같이 보일 수 있습니다.

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

커밋된 속성을 노출 하는 가짜 작업 단위를 확인할 수 있습니다. 경우에 따라 기능 테스트를 용이 하 게 하는 가짜 클래스에 추가 하는 것이 유용 합니다. 이 경우 코드 커밋 속성을 확인 하 여 하나의 작업 단위로 커밋됩니다 관찰 하기 쉽습니다.

가짜 i o b j도 필요&lt;T&gt; 직원과 TimeCard 개체를 메모리에 유지할 수 있습니다. 에서는 제네릭을 사용 하는 단일 구현을 제공할 수 있습니다.

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

이 test double 대리자를 기본 HashSet 작업 대부분이&lt;T&gt; 개체입니다. 참고 해당 i o b j&lt;T&gt; 클래스 (참조 형식)으로 T를 적용 하는 제네릭 제약 조건이 필요 하 고 또한 IQueryable을 구현 하도록 강제로&lt;T&gt;합니다. IQueryable로 표시 하는 메모리 내 컬렉션을 확인 하기 쉽습니다&lt;T&gt; AsQueryable 표준 LINQ 연산자를 사용 합니다.

### <a name="the-tests"></a>테스트

기존 단위 테스트는 단일 테스트 클래스를 사용 하 여 단일 MVC 컨트롤러에서 모든 작업에 대 한 테스트를 모두 유지 하기 위해 됩니다. 작성할 수 있습니다 이러한 테스트 또는 단위 테스트, 형식의 fakes에서 메모리를 사용 하 여 빌드 했습니다. 그러나 피해 야 할 것이 문서에 대 한 모놀리식 응용 프로그램 테스트 클래스 접근 방식 및 대신 그룹 기능의 특정 부분에 초점을 테스트 합니다.  "새 직원 만들기 합니다" 예를 들어 단일 테스트 클래스를 사용 하 여 새 직원을 담당 단일 컨트롤러 동작을 확인 하므로 테스트 하고자 하는 기능을 수 있습니다.

이러한 모든 미세 조정 된 테스트 클래스에 필요한 일반적인 설치 코드가 있습니다. 예를 들어 항상 생성 해야 메모리 내 리포지토리 및 가짜 작업 단위입니다. 또한 직원 컨트롤러 인스턴스에 가짜 삽입 하는 작업 단위를 사용 하 여 해야 합니다. 기본 클래스를 사용 하 여이 일반적인 설치 코드 테스트 클래스에서 공유 됩니다.

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

기본 클래스에서 사용 하 여 "개체 어머니"은 테스트 데이터 생성에 대 한 하나의 일반적인 패턴입니다. 개체 어머니는 여러 테스트 픽스 쳐에서 사용할 테스트 엔터티를 인스턴스화할 팩터리 메서드를 포함 합니다.

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

테스트 픽스 쳐 (그림 3 참조)의 수에 대 한 기본 클래스로 EmployeeControllerTestBase 사용할 수 있습니다. 각 테스트 픽스 쳐를 특정 컨트롤러 작업을 테스트 합니다. 예를 들어, 하나의 테스트 픽스 쳐는 테스트 (직원을 만들기 위한 뷰를 표시)를 HTTP GET 요청 중 사용 되는 만들기 작업을 집중 하 고 다양 한 fixture HTTP POST 요청에 사용 되는 만들기 작업을 중점적 (제출한 정보에 직원 만들기 사용자)입니다. 각 파생된 클래스는 특정 상황에서 특정 테스트 상황에 대 한 결과 확인 하는 데 필요한 어설션을 제공 하는 데 필요한 설치만 합니다.

![eftest_03](~/ef6/media/eftest-03.png)

**그림 3**

여기에 제시 된 명명 규칙 및 테스트 스타일에는 테스트 가능한 코드에 대 한 필요 하지 않습니다. 즉,이 방법 중 하나일 뿐입니다. 그림 4에서는 Jet 수렵과 Resharper에서 실행 중인 테스트는 runner 플러그 인 Visual Studio 2010에 대 한 테스트 보여 줍니다.

![eftest_04](~/ef6/media/eftest-04.png)

**그림 4**

공유 설치 코드를 처리 하는 기본 클래스를 사용 하 여 각 컨트롤러 작업에 대 한 단위 테스트는 작고 쉽게 작성할 수 있습니다. 테스트를 신속 하 게 (것은 수행 하므로 메모리 내 작업)를 실행 및 (단위 테스트 격리 한 것) 때문에 관련 되지 않은 인프라 또는 환경 문제 때문에 실패 하지 않아야 합니다.

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

이러한 테스트의 기본 클래스는 대부분의 설치 작업을 수행합니다. 기본 클래스 생성자 만들고 메모리 내 리포지토리, 작업, 가짜 단위 EmployeeController 클래스의 인스턴스를 기억 합니다. 테스트 클래스에는이 기본 클래스에서 파생 되며 Create 메서드를 테스트의 세부 정보에 중점을 둡니다. 이 경우 구체적인 하느냐, 프로시저를 테스트 하는 모든 단위가 표시 "정렬, assert 및 역할 을" 단계:

-   들어오는 데이터를 시뮬레이션 하기 위해 newEmployee 개체를 만듭니다.
-   EmployeeController 만들기 작업을 호출 하 고는 newEmployee 전달 합니다.
-   만들기 작업 생성 (리포지토리에서 직원 표시 됨) 예상된 결과 확인 합니다.

빌드한 새로운 EmployeeController 작업 중 하나를 테스트할 수 있습니다. 예를 들어 직원 컨트롤러의 인덱스 작업에 대 한 테스트 작성 것과 동일한 테스트에 대 한 기본 설정을 설정 하는 테스트 기본 클래스에서 상속할 수 있습니다. 다시 기본 클래스 메모리 내 리포지토리, 작업, 가짜 단위 이며는 EmployeeController 인스턴스에 만들어집니다. 인덱스 작업에 대 한 테스트 인덱스 동작 호출에 집중 해야 하 고 반환 작업 모델의 품질을 테스트 합니다.

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

메모리에서 fakes를 사용 하 여 만든 테스트는 테스트를 *상태* 소프트웨어입니다. 예를 들어, create 작업 실행 – 후 저장소의 상태를 검사 하려는 만들기 동작을 테스트 하는 경우 저장소 유지 않으면 새 직원?

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

나중에 상호 작용 테스트 살펴보겠습니다. 상호 작용 테스트 테스트 중인 코드가 개체의 적절 한 메서드를 호출 하 고 올바른 매개 변수를 전달 하는 메시지가 표시 됩니다. 지금은 설명으로 넘어가겠습니다 표지 다른 디자인 패턴 – 지연 로드 합니다.

## <a name="eager-loading-and-lazy-loading"></a>즉시 로드, 지연 로드

ASP.NET MVC 웹 시점에서 우리가 하려는 직원의 정보를 표시 하 고 직원의 포함 하는 응용 프로그램 시간 카드를 연결 합니다. 예를 들어 시스템에서 직원의 이름 및 시간 카드의 총 수를 보여 주는 시간 카드 요약 표시가 있을 수 있습니다. 이 기능을 구현 하기 위해 수행할 수 우리는 몇 가지가 있습니다.

### <a name="projection"></a>프로젝션

쉽게 만드는 방법 한 가지 요약 뷰에 표시 하려는 정보에 전용된 모델을 만드는 것입니다. 이 시나리오에서는 모델은 다음과 같이 보일 수 있습니다.

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

Note는 EmployeeSummaryViewModel 엔터티가 아닌 – 즉 사항도 아닙니다 데이터베이스에 유지 하려고 합니다. 이 클래스를 사용 하 여 강력한 형식의 방식으로 데이터를 뷰로 순서 섞기를 하려고만 합니다. 뷰 모델 속성만 없습니다 동작 (메서드 없음)-를 포함 하기 때문에 데이터 전송 개체 (DTO)와 같은 경우 속성에는 이동에 필요한 데이터가 포함 됩니다. LINQ의 표준 프로젝션 연산자 – Select 연산자를 사용 하 여이 뷰 모델을 인스턴스화하는 것이 쉽습니다.

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

위의 코드에 두 가지 주요 기능이 있습니다. 먼저 코드는 여전히 쉽게 관찰 하 고 격리할 수 있기 때문에 테스트 하기가 쉽습니다. Select 연산자 작동과 동일 하 게 실제 작업 단위에 대해는 메모리 내 fakes에 대 한 것도 가능 합니다.

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

두 번째 주목할 만한 기능은 코드 EF4 직원과 시간 카드 정보를 함께 조합 하는 단일 하 고 효율적인 쿼리를 생성 하려면를 허용 하는 방법입니다. 특별 한 Api를 사용 하지 않고 직원 및 시간 카드 정보와 같은 개체를 로드 했습니다. 단순히 코드를 원격 데이터 원본 뿐만 아니라 메모리 내 데이터 원본에 대해 작동 하는 표준 LINQ 연산자를 사용 하 여 필요한 정보를 표현 합니다. EF4 LINQ 쿼리 및 C에서 생성 된 식 트리를 변환할 수 있었습니다\# 단일 하 고 효율적인 T-SQL 쿼리로 컴파일러.

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

뷰 모델 또는 DTO 개체와 이지만 실제 엔터티를 사용 하 여 작동 않으려는 경우 다른 경우가 있습니다. 직원을 먼저 알고 때 *및* 직원의 시간 카드 눈에 띄지 않는 효율적인 방식으로 관련 된 데이터를 적극적으로 로드할 수 있습니다.

### <a name="explicit-eager-loading"></a>명시적 로딩과

비즈니스 논리에 대 한 (또는이 시나리오에서는 컨트롤러 작업 논리) 메커니즘 필요한 관련된 엔터티 정보를 적극적으로 로드 하려는 경우 하도록 리포지토리를 표현할 수 있습니다. EF4 ObjectQuery&lt;T&gt; 클래스 관련된 개체를 쿼리 하는 동안 검색을 지정 하는 Include 메서드를 정의 합니다. EF4 ObjectContext 구체적인 ObjectSet 통해 엔터티를 노출 해야&lt;T&gt; ObjectQuery에서 상속 되는 클래스&lt;T&gt;합니다.  ObjectSet 사용 하는 경우&lt;T&gt; 에서는 다음 코드를 작성할 수는 컨트롤러 작업에 대 한 참조는 각 직원에 대 한 시간 카드 정보의 즉시 로드를 지정 합니다.

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

그러나 테스트 가능한 코드를 유지 하려고 하므로 우리는 노출 되지 않은 ObjectSet&lt;T&gt; 에서 작업 클래스의 실제 단위 외부입니다. 대신에 의존 하는 i o b j&lt;T&gt; 모조를 쉽게 인터페이스 있지만 i o b j&lt;T&gt; 는 Include 메서드를 정의 하지 않습니다. LINQ의 장점을 Include 연산자를 직접 만들 수 있습니다.

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

이 포함 연산자를 확장 메서드로 정의 되어 IQueryable&lt;T&gt; 대신 i o b j&lt;T&gt;합니다. 광범위 한 IQueryable을 비롯 한 가능한 형식이 메서드를 사용 하는 기능 제공&lt;T&gt;, i o b j&lt;T&gt;, ObjectQuery&lt;T&gt;, 및 ObjectSet&lt;T&gt;합니다. 이벤트 기본 시퀀스를 정품 EF4 ObjectQuery 아닙니다&lt;T&gt;, 그런 다음 완료 아무런 문제가 없습니다 및 Include 연산자가 작동 하지 않습니다. 기본 시퀀싱 하는 경우 *됩니다* 는 ObjectQuery&lt;T&gt; (또는 ObjectQuery에서 파생 된&lt;T&gt;), EF4 우리의 요구 사항 추가 데이터에 대 한 참조를 적절 한 SQL을 작성 한 다음 쿼리입니다.

위치에서이 새 연산자를 사용 하 여 리포지토리에서 시간 카드 정보의 즉시 로드를 명시적으로 요청할 수 있습니다 했습니다.

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

에 대해 실제 ObjectContext를 실행 하는 경우 코드를 다음 단일 쿼리를 생성 합니다. 쿼리는 employee 개체를 구체화 하 고 완벽 하 게 해당 시간표 속성을 채우는 데 한 번의 데이터베이스에서 충분 한 정보를 수집 합니다.

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

좋은 소식은 작업 메서드 내에서 코드가 완벽 하 게 테스트할 수 있습니다. 포함 연산자를 지원 하 여 fakes에 대 한 추가 기능을 제공할 필요가 없습니다. 나쁜 소식은 지 속성 무시 유지 하려는 코드 내에서 포함 연산자를 사용 해야 합니다. 테스트 가능한 코드를 작성할 때 평가 해야 하는 균형점을 찾아야 형식의 가장 대표적인 예입니다. 성능 목표를 충족 하기 위해 저장소 추상화 외부 지 속성 우려 누수 수 있도록 해야 하는 경우가 있습니다.

즉시 로드에 대 한 대체는 지연 로드 합니다. 우리가 지연 로드 의미 *하지* 명시적으로 연결 된 데이터에 대 한 요구를 발표 하는 데는 비즈니스 코드가 필요 합니다. 대신, 응용 프로그램에서이 엔터티 사용 및 추가 데이터는 필요한 Entity Framework가 필요할 때 데이터를 로드 합니다.

### <a name="lazy-loading"></a>지연 로드

여기서 알 수 해야 할 부분은 비즈니스 논리를 데이터 시나리오를 가정해 보겠습니다 하기 쉽습니다. 논리는 employee 개체 하지만 다른 실행 경로 시간 카드 정보가 직원에 해당 경로 중 일부 필요 하지 않은 않았고으로 분기할 수 있습니다 것 알고 있습니다. 다음과 같은 시나리오는 데이터에는 필요에 따라 마술 표시 되기 때문에 암시적 지연 로드에 적합 합니다.

지연 로드, 지연 된 라고도 로드는 엔터티 개체에 대해 몇 가지 요구 사항이 설정지 않습니다. Poco 진정한 지 속성 무시를 사용 하 여 지 속성 계층의 모든 요구 사항을 직면 하지 이지만 진정한 지 속성 무시 실질적으로 목표를 달성할 수 없습니다.  대신 지 속성 무시 상대도 단위로 측정합니다. 하기란 상당히 아쉬운 점 지향 지 속성 기본 클래스에서 상속 하거나 Poco에서 지연 로드를 위해 특수 한 컬렉션을 사용 해야 하는 경우. 다행 스럽게도 EF4 줄어들었습니다 솔루션을 있습니다.

### <a name="virtually-undetectable"></a>거의 찾아냅니다.

POCO 개체를 사용 하는 경우 EF4 엔터티에 대 한 런타임 프록시를 동적으로 생성할 수 있습니다. 이러한 프록시 켜지 구체화 된 Poco 및 각 속성을 해석 하 여 추가 서비스를 받을 제공 래핑하고 추가 작업을 수행 하는 작업을 설정 합니다. 이러한 서비스 중 하나는 지연 로딩 기능을 살펴봅니다. 다른 서비스는는 효율적인 변경 내용 추적 프로그램 엔터티의 속성 값을 변경 하는 경우를 기록할 수 있는 메커니즘입니다. 변경 내용 목록은 업데이트 명령을 사용 하 여 수정 된 엔터티를 유지 하기 위해 ObjectContext에서 SaveChanges 메서드를 호출 하는 동안 사용 됩니다.

그러나 하려면 이러한 프록시에 대 한 필요한 속성이 get에 연결 하는 방법 및 가상 멤버를 재정의 하 여이 목표를 달성 하는 엔터티, 및 프록시 설정 작업 합니다. 따라서 암시적 지연 로딩 및 효율적인 변경 내용 추적 하려는 경우에 POCO 클래스 정의 돌아가서 가상으로 속성을 표시 해야 합니다.

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

여전히 Employee 엔터티에 대부분 지 속성 무시 단언할 수 있습니다. 유일한 요구 사항은 가상 멤버를 사용 하는 것을 코드의 테스트 용이성에는 영향이 없습니다. 어떤 특별 한 기본 클래스에서 파생 되거나 지연 로딩에 전용 특수 한 컬렉션을 사용 하 여 필요가 있습니다. 코드 에서처럼 ICollection을 구현 하는 모든 클래스&lt;T&gt; 관련된 엔터티를 포함할 수 있습니다.

이 작업 단위 내에서 확인 해야 하는 한 가지 사소한 변경 이기도 합니다. 지연 로딩은 *해제* 를 ObjectContext 개체와 직접 작업 하는 경우 기본적으로 합니다. 설정할 수 있습니다 ContextOptions 속성에 사용할 수 있도록 지연 된 로드를 속성 이며 지연 로드 어디서 나 사용할 수 있게 하고자 하는 경우이 속성은 실제 작업 단위 내에서 설정할 수 있습니다.

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

암시적 지연 로딩을 사용 하도록 설정, 응용 프로그램 코드는 직원을 사용할 수 있습니다와 직원의 추가 데이터를 로드 하는 EF에 필요한 작업을 인식 하지 못하는 부분을 유지 하면서 시간 카드를 연결 합니다.

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

지연 로딩을 쉽게 응용 프로그램 코드를 작성 하 고 코드를 프록시 매직을 사용 하 여 완전히 테스트 되지 않습니다. 작업 단위의 메모리 fakes 테스트 하는 동안 필요한 경우 연결 된 데이터를 사용 하 여 가짜 엔터티 미리 로드 하기만 하면 됩니다.

이 시점에서는에서는 알아보겠습니다 i o b j를 사용 하 여 저장소에 이르기까지&lt;T&gt; 추상화 숨기기 지 속성 프레임 워크의 모든 기호를 확인 합니다.

## <a name="custom-repositories"></a>사용자 지정 리포지토리

이 문서의 작업 디자인 패턴의 단위 먼저 표시에서는 작업 단위를 모양을 대 한 몇 가지 샘플 코드를 제공 했습니다. 직원과 함께 일 하면서 우리 직원 시간 카드 시나리오를 사용 하 여이 원래 개념을 다시 제공 하겠습니다.

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

이 작업 단위 및 마지막 섹션에서 만든 작업의 단위 간의 기본 차이점은이 작업 단위에 EF4 프레임 워크에서 모든 추상화를 사용 하지 않는 방법 (방법이 없는 i o b j&lt;T&gt;). I o b j&lt;T&gt; works를 리포지토리 인터페이스를 사용 하지만 노출 하는 API도 완벽 하 게 응용 프로그램의 요구를 사용 하 여 정렬 되지 않습니다. 향후이 방식에서에서는 사용자 지정 IRepository를 사용 하 여 리포지토리 나타낼&lt;T&gt; 추상화 합니다.

테스트 기반 디자인, 동작 기반 디자인 및 도메인 기반 방법론 디자인을 팔 로우 하는 대부분의 개발자는 IRepository를 선호&lt;T&gt; 방법은 여러 가지 이유로 합니다. 첫 번째는 IRepository&lt;T&gt; 인터페이스는 "anti-corruption" 계층을 나타냅니다. Eric Evans의 저서 Domain Driven Design 설명 된 대로 손상 방지 레이어를 지 속성 API 등의 인프라 Api에서 도메인 코드를 유지 합니다. 둘째, 개발자 (테스트를 작성 하는 동안 검색) 하는 대로 응용 프로그램의 정확한 요구를 충족 하는 리포지토리로 메서드를 빌드할 수 있습니다. 예를 들어, 우리가 해야 할 수 자주 FindById 메서드 리포지토리 인터페이스를 추가할 수 있도록 ID 값을 사용 하 여 단일 엔터티를 찾습니다.  우리의 IRepository&lt;T&gt; 정의 다음과 같이 표시 됩니다.

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

IQueryable을 사용 하 여 다시 삭제 됩니다 것을 알 수 있습니다&lt;T&gt; 엔터티 컬렉션을 노출 하는 인터페이스입니다. IQueryable&lt;T&gt; EF4 공급자로 이동 하 고 공급자 쿼리의 전체적인 보기를 제공 하도록 LINQ 식 트리를 허용 합니다. 두 번째 옵션을 IEnumerable을 반환 하는 것&lt;T&gt;, EF4 LINQ 공급자 즉, 리포지토리 내에서 작성 한 식만 표시 됩니다. 모든 그룹화, 정렬 및 리포지토리 외부에서 수행 하는 프로젝션 하지 성능이 저하 될 수 있는 데이터베이스에 보내는 SQL 명령으로 구성 됩니다. 반면, IEnumerable만를 반환 하는 리포지토리&lt;T&gt; 새 SQL 명령을 사용 하 여 하를 결과 놀라게 하지 않습니다. 두 방법 모두 작동 하 고 테스트 가능한 두 가지 방법을 모두 유지.

IRepository의 단일 구현이 제공 하는 것이 간단&lt;T&gt; 제네릭과 EF4 ObjectContext API를 사용 하 여 인터페이스입니다.

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

IRepository&lt;T&gt; 접근 방식을 제공 쿼리에 대 한 몇 가지 추가 제어를 클라이언트 엔터티를 가져오는 메서드를 호출 하기 때문입니다. 메서드 내에서는 추가 검사 및 응용 프로그램 제약 조건을 적용 하는 LINQ 연산자를 제공할 수 있습니다. 인터페이스에 제네릭 형식 매개 변수의 두 가지 제약 조건을 확인 합니다. 첫 번째 제약 조건의 ObjectSet에 필요한 클래스 단점 검은&lt;T&gt;, 두 번째 제약 조건을 강제로 IEntity – 응용 프로그램에서 만든 추상화를 구현 하는 엔터티. IEntity 인터페이스에 엔터티를 읽을 수 있는 Id 속성을 강제로 수행 하 고 FindById 메서드에서이 속성을 사용 합니다 했습니다. 다음 코드를 사용 하 여 IEntity 정의 됩니다.

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

IEntity이 엔터티는이 인터페이스를 구현 해야 하므로 작은 지 속성 무시 위반을 간주할 수 있습니다. 지 속성 무시는 절충에 대 한 많은 FindById 기능 인터페이스에 의해 적용 된 제약 조건 보다는 기억 하세요. 인터페이스에 테스트 용이성을 영향을 주지 않습니다.

라이브 IRepository 인스턴스화&lt;T&gt; 인스턴스화를 관리 하는 구체적인 단위의 클라우드 구현 해야 하므로 EF4 ObjectContext에 필요 합니다.

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

### <a name="using-the-custom-repository"></a>사용자 지정 리포지토리를 사용 하 여

I o b j 기반 저장소를 사용 하 여 크게 다르지 않습니다 우리의 사용자 지정 리포지토리를 사용 하 여&lt;T&gt;합니다. 속성에 직접 LINQ 연산자를 적용 하는 대신 먼저 해야 하나 IQueryable을 리포지토리의 메서드를 호출할&lt;T&gt; 참조 합니다.

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

이전에 구현한 사용자 지정 포함 연산자를 변경 하지 않고도 작동을 확인 합니다. 리포지토리의 FindById 메서드는 단일 엔터티를 검색 하는 동안 작업에서 중복 된 논리를 제거 합니다.

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

살펴보았습니다 두 가지 테스트 용이성에 상당한 차이가 없습니다. IRepository의 가짜 구현을 제공할 수 있습니다&lt;T&gt; HashSet가 지원 되는 구체적 클래스를 작성 하 여&lt;직원&gt; -마지막 섹션에서 수행한 것과 같습니다. 그러나 일부 개발자는 모의 개체를 사용 하 여 모의 개체 프레임 워크 fakes를 구축 하는 대신 선호 합니다. 모의 개체를 사용 하 여 구현을 테스트 하 고 다음 섹션에서 모의 개체와 fakes 간의 차이점에 설명 하 고 살펴보겠습니다.

### <a name="testing-with-mocks"></a>모의 개체를 사용한 테스트

"Test double" 어떤 Martin Fowler 호출의 빌드 하는 방법은 여러 가지가 있습니다. Test double (예: 동영상 stunt double)는 테스트 하는 동안 프로덕션 개체를 ""를 실제로 빌드는 개체입니다. 만든 메모리 내 리포지토리는 SQL Server와 통신 하는 리포지토리에 대 한 test double입니다. 단위 테스트 하는 동안 이러한 test double을 사용 하 여 코드를 분리 하 여 빠르게 실행 하는 테스트를 유지 하는 방법을 살펴보았습니다.

빌드한 test double에 구현이 실제, 작동 합니다. 개체의 구체적인 컬렉션 저장 각각 백그라운드 하 고 이러한 추가 하 고 테스트 하는 동안 저장소를 조작에서는이 컬렉션에서 개체 제거. 일부 개발자는 해당 test double이 방법이으로 – 실제 코드 및 작업 구현을 작성 하려고 합니다.  이 test double는 부르는 *fakes*합니다. 작업 구현을 갖고 있지만 사실은 그렇지만도 않습니다 프로덕션 사용에 대 한 충분 한 실제입니다. 가상 repository는 데이터베이스에 실제로 기록 하지 않습니다. 실제로 가짜 SMTP 서버 네트워크를 통해 전자 메일 메시지를 전송 하지 않습니다.

### <a name="mocks-versus-fakes"></a>Fake 및 mock

다른 유형의 double 라고 하는 테스트를 *모의*합니다. Fakes가 작업 구현 하는 동안 mock 구현이 제공 됩니다. 모의 개체 프레임 워크를 활용 하 여 런타임 시 이러한 모의 개체를 생성 하 고 test double로 사용 합니다. 이 섹션에서는 사용할 것 모의 프레임 워크 Moq 오픈 소스입니다. 동적으로 만드는 테스트는 직원 리포지토리에 대 한 이중 Moq를 사용 하는 간단한 예는 다음과 같습니다.

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

Moq는 IRepository 묻습니다&lt;직원&gt; 구현 하며 빌드 하나 동적으로 합니다. IRepository를 구현 하는 개체를 가져올 수 있습니다&lt;직원&gt; Mock의 개체 속성에 액세스 하 여&lt;T&gt; 개체입니다. 이 내부 개체는 컨트롤러에 전달할 수 이며 다시 test double 또는 실제 리포지토리 경우 알 수 없습니다. 개체의 메서드와 실제 구현과 개체의 메서드를 호출 하는 것 처럼 호출할 수 있습니다.

모의 리포지토리 사항을 Add 메서드를 호출 하는 경우 어떻게 되는지 궁금할 수 있어야 합니다. 모의 개체와 관련 없는 구현 이므로 아무 추가 되지 않습니다. 직원은 삭제 되므로 우리가 작성 한 fakes를 사용 하 여 했습니다 같은 백그라운드 구체적인 컬렉션이 없습니다 있습니다. FindById의 반환 값의 경우는 어떨까요? 이 경우 모의 개체 기본값을 반환 되는 것만 작업을 수행 합니다. 참조 형식 (직원)으로 반환 하는 것 이므로 반환 값에 null 값이입니다.

Mock는 실수나; 보일 수 있습니다. 그러나 모의 개체에 이야기 하지 않은 기능을 추가로 두 가지입니다. 첫째, Moq 프레임 워크는 모의 개체에 대 한 모든 호출은 기록 합니다. 코드의 뒷부분에 나오는 것 Moq Add 메서드를 호출 하는 모든 사용자가 아니면 FindById 메서드를 호출 하는 모든 사용자가 요청할 수 있습니다. 이 "블랙 박스" 기록 기능 테스트에서 사용 하는 방법을 나중에 살펴보겠습니다.

두 번째 유용한 기능은 Moq 프로그래밍 모의 개체를 사용 하는 방법을 *기대*합니다. 모의 개체 모든 지정 된 상호 작용에 응답 하는 방법을 지시 하는 관여 합니다. 예를 들어, 수 있다고 예상할은 mock를 프로그래밍 하 고 FindById를 호출 하는 사람이 경우 employee 개체를 반환 하도록 지시 합니다. Moq 프레임 워크 설치 API 및 람다 식을 사용 하 여 이러한 기대를 프로그램.

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

이 샘플 리포지토리를 동적으로 작성 하려면 Moq 요청에 관여를 사용 하 여 리포지토리 우리가 프로그래밍 하는 다음을 예상 개체를 반환할 새 직원 Id 값이 5 인 5의 값을 전달 하 여 FindById 메서드를 호출 하는 사람이 경우 모의 개체를 나타냅니다. 이 테스트에 통과 및 가짜 IRepository 전체 구현이 필요가 없었습니다&lt;T&gt;합니다.

앞서 작성 한 테스트를 다시 방문 하 고 fakes 대신 모의 개체를 사용 하도록 재작업 보겠습니다. 마찬가지로 이전에서는 기본 클래스 모든 컨트롤러의 테스트에 필요한 인프라의 공통 부분을 설정 하 합니다.

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

설정 된 코드는 대개는 동일합니다. Fakes를 사용 하는 대신 모의 개체를 만드는 데 Moq 사용 하겠습니다. 코드 직원 속성을 호출할 때 모의 리포지토리를 반환할 작업의 모의 단위에 대 한 기본 클래스를 정렬 합니다. 모의 설정의 나머지 각 특정 시나리오에 전용으로 테스트 픽스 쳐 내에서 수행 됩니다. 예를 들어, 인덱스 작업에 대 한 테스트 픽스 쳐 작업 모의 리포지토리의 FindAll 메서드를 호출 하는 경우 직원의 목록을 반환할 모의 리포지토리를 설정 합니다.

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

기대치를 제외 하 고 테스트 하기 전에 했습니다 테스트와 유사 합니다. 그러나 모의 프레임 워크의 기록 기능을 사용 하 여 다른 각도에서 테스트 접근 수 것입니다. 다음 섹션에서이 새 관점에서 살펴보겠습니다.

### <a name="state-versus-interaction-testing"></a>와 상호 작용 테스트 상태

모의 개체를 사용 하 여 소프트웨어를 테스트 하 여 다양 한 기술이 있습니다. 한 가지 방법은 상태를 사용 하려면 기반 테스트, 새로운 위해서는이 문서의 지금는 합니다. 상태 기반 소프트웨어의 상태에 대 한 테스트는 어설션 합니다. 마지막 테스트에서는 컨트롤러에서 작업 메서드를 호출 하 고 만든 모델에 대 한 어설션을 작성 해야 합니다. 테스트 상태에 있는 다른 몇 가지 예는 다음과 같습니다.

-   리포지토리 만들기를 실행 한 후 새 employee 개체를 포함을 확인 합니다.
-   인덱스 실행 후 모델 보유 모든 직원의 목록을 확인 합니다.
-   저장소에 없는 지정된 된 직원 삭제를 실행 한 후 확인 합니다.

모의 개체를 사용 하 여 표시 하는 다른 방법은 확인 하는 것 *상호 작용*합니다. 상태 개체의 상태에 대 한 테스트는 어설션을 기반으로 하는 동안 상호 작용 개체는 상호 작용 하는 방법에 대 한 테스트는 어설션 기반 합니다. 예를 들어:

-   컨트롤러 만들기를 실행 하는 경우 저장소의 추가 메서드를 호출을 확인 합니다.
-   인덱스 실행 하는 경우 컨트롤러는 저장소의 FindAll 메서드를 호출을 확인 합니다.
-   편집을 실행할 때 변경 내용을 저장 단위 작업의 Commit 메서드를 호출 하는 컨트롤러를 확인 합니다.

상호 작용 테스트 되지 컬렉션 내에서 배치 하 고 개수를 확인 하기 때문에 테스트 데이터가 종종 필요 합니다. 예를 들어 올바른 값-를 사용 하 여 저장소의 FindById 메서드를 호출 하는 세부 정보 작업을 알고 있다면 다음 작업 아마도 올바르게 작동 합니다. 이 동작은 FindById에서 반환할 테스트 데이터를 설정 하지 않고도 확인할 수 있습니다.

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

위의 테스트 픽스 쳐에서 필요한 유일한 설치는 기본 클래스에서 제공 하 여 설치 합니다. 컨트롤러 작업을 호출 하는 경우 Moq는 모의 리포지토리를 사용 하 여 상호 작용을 기록 합니다. 확인 Moq API를 사용 하 여, 수 요청 Moq 컨트롤러 FindById 적절 한 ID 값을 사용 하 여 호출 합니다. 컨트롤러 메서드를 호출 하지 않은 또는 예기치 않은 매개 변수 값을 사용 하 여 메서드를 호출 하는 경우 Verify 메서드는 예외를 throw 하 고 테스트가 실패 합니다.

만들기 작업을 현재 작업 단위에 커밋 호출을 확인 하려면 또 다른 예는 다음과 같습니다.

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

상호 작용을 테스트 한 위험을 통해 상호 작용을 지정 하는 경향이입니다. 기록 하 고 모의 개체를 사용 하 여 모든 상호 작용을 테스트 한다고 해 서 확인 모의 개체의 기능을 모든 상호 작용을 확인 하려고 해야 합니다. 일부 상호 작용은 구현 세부 정보 및 상호 작용만 확인 해야 *필요한* 현재 테스트를 충족 하도록 합니다.

Mock 또는 fake 간의 선택 하느냐에 달려 테스트 중인 시스템과 개인 (또는 팀) 기본 설정 합니다. 모의 개체 test double을 구현 해야 하는 코드의 양을 감소 시킬 수 있습니다 이지만 일부 사용자 에게만 기대치를 프로그래밍 하 고 상호 작용을 확인 하 편리 하 게 합니다.

## <a name="conclusions"></a>결론

이 문서에 데이터 유지를 위해 ADO.NET Entity Framework를 사용 하는 동안 테스트 가능한 코드를 작성 하는 몇 가지 방법에 알아보았습니다. I o b j 같은 기본 제공된 추상화 활용할 수 있습니다&lt;T&gt;, IRepository와 같은 고유한 추상화를 만들거나&lt;T&gt;합니다.  두 경우 모두 ADO.NET Entity Framework 4.0에서 POCO 지원을 무시 및 가볍고 테스트가 용이한 지속 되도록 하려면 이러한 추상화의 소비자를 수 있습니다. 암시적 지연 로딩 하면 비즈니스 및 응용 프로그램 서비스와 같은 추가 EF4 기능은 관계형 데이터 저장소의 세부 정보에 대 한 걱정 없이 작동 하는 코드입니다. 마지막으로 만들겠습니다. 추상화가 단위 테스트 내에서 모조 또는 모의 쉽게 하 고 이러한 격리 된 매우 빠른 실행을 위해 test double 및 신뢰할 수 있는 테스트 사용할 수 있습니다.

### <a name="additional-resources"></a>추가 리소스

-   Robert C. Martin " [단일 책임 원칙을](http://www.objectmentor.com/resources/articles/srp.pdf)"
-   Martin Fowler [패턴 카탈로그](http://www.martinfowler.com/eaaCatalog/index.html) 에서 *엔터프라이즈 응용 프로그램 아키텍처 패턴*
-   Griffin Caprio " [종속성 주입](https://msdn.microsoft.com/magazine/cc163739.aspx)"
-   Data Programmability 블로그 " [연습: Entity Framework 4.0 사용한 테스트 기반 개발](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)"입니다.
-   Data Programmability 블로그 " [Entity Framework 4.0을 사용 하 여 사용 하 여 리포지토리 및 작업 단위 패턴](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"
-   Dave Astels " [BDD 소개](http://blog.daveastels.com/files/BDD_Intro.pdf)"
-   Aaron Jensen " [컴퓨터 사양 소개](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"
-   Eric Lee, " [MSTest 사용 하 여 BDD](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"
-   Eric Evans " [도메인 기반 디자인](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"
-   Martin Fowler " [스텁 아닌 모의](http://martinfowler.com/articles/mocksArentStubs.html)"
-   Martin Fowler " [이중 테스트](http://martinfowler.com/bliki/TestDouble.html)"
-   Jeremy Miller, " [상호 작용 테스트와 상태](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"
-   [Moq](http://code.google.com/p/moq/)

### <a name="biography"></a>약력

Scott Allen Pluralsight의 기술 담당자의 멤버 이며 OdeToCode.com의 창립자입니다. 상용 소프트웨어 개발의 15 년 동안에서 Scott 8 비트 뛰어난 ASP.NET 웹 응용 프로그램에 포함 된 장치에서 모든 항목에 대 한 솔루션에서 일 했습니다. 그의 블로그인 OdeToCode, 또는 Twitter Scott에 도달할 수 있습니다 [ http://twitter.com/OdeToCode ](http://twitter.com/OdeToCode)합니다.
