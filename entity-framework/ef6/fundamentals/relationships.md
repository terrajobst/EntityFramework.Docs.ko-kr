---
title: 관계, 탐색 속성 및 외래 키-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: cc7160f2d0ab7ac0c6009f820441c88590cacfaf
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655872"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a>관계, 탐색 속성 및 외래 키
이 항목에서는 Entity Framework 엔터티 간의 관계를 관리 하는 방법에 대 한 개요를 제공 합니다. 또한 관계를 매핑하고 조작 하는 방법에 대 한 몇 가지 지침을 제공 합니다.

## <a name="relationships-in-ef"></a>EF의 관계

관계형 데이터베이스에서 테이블 간의 관계 (연결 라고도 함)는 외래 키를 통해 정의 됩니다. 외래 키 (FK)는 두 테이블의 데이터 간에 링크를 설정 하 고 적용 하는 데 사용 되는 열 또는 열 조합입니다. 일반적으로 일 대 일, 일 대 다 및 다 대 다와 같은 세 가지 유형의 관계가 있습니다. 일 대 다 관계에서 외래 키는 관계의 다 쪽을 나타내는 테이블에 정의 됩니다. 다 대 다 관계에는 기본 키가 두 관련 테이블의 외래 키로 구성 된 세 번째 테이블 (접합 또는 조인 테이블 이라고 함)을 정의 하는 작업이 포함 됩니다. 일 대 일 관계에서는 기본 키가 외래 키로도 적용 되 고 두 테이블에 대 한 별도의 외래 키 열이 없습니다.

다음 이미지는 일 대 다 관계에 참여 하는 두 테이블을 보여 줍니다. **과정** 테이블은 **부서** 테이블에 연결 하는 **DepartmentID** 열이 포함 되어 있으므로 종속 테이블입니다.

![부서 및 과정 테이블](~/ef6/media/database2.png)

Entity Framework에서 엔터티는 연결 또는 관계를 통해 다른 엔터티와 관련 될 수 있습니다. 각 관계에는 엔터티 형식을 설명 하는 두 개의 end와 해당 관계에 있는 두 엔터티에 대 한 형식 (1, 0 또는 그 다)의 복합성이 포함 됩니다. 관계는 주 역할이 며 종속 역할인 관계의 end를 설명 하는 참조 제약 조건에 의해 제어 될 수 있습니다.

탐색 속성은 두 엔터티 형식 간의 연결을 탐색 하는 방법을 제공 합니다. 모든 개체에는 해당 개체가 참여하는 모든 관계에 대한 탐색 속성이 있습니다. 탐색 속성을 사용 하면 두 방향에서 관계를 탐색 하 고 관리할 수 있습니다. 참조 개체 (복합성이 하나 또는 0 또는 1)를 반환 하거나 복합성이 많은 경우 컬렉션을 반환 합니다. 단방향 탐색을 선택할 수도 있습니다 .이 경우 관계에 참여 하는 형식 중 하나에만 탐색 속성을 정의할 수 있습니다.

모델에 데이터베이스의 외래 키에 매핑되는 속성을 포함 하는 것이 좋습니다. 외래 키 속성이 포함되어 있으면 종속 개체에서 외래 키 값을 수정하여 관계를 만들거나 변경할 수 있습니다. 이러한 연결 종류를 외래 키 연결이라고 합니다. 외래 키를 사용 하는 것은 연결이 끊어진 엔터티로 작업할 때 훨씬 더 중요 합니다. 1-1 또는 1-0으로 작업 하는 경우입니다. 1 관계는 별도의 외래 키 열이 없으며 기본 키 속성은 외래 키로 작동 하며 항상 모델에 포함 됩니다.

모델에 외래 키 열이 포함 되지 않은 경우 연결 정보는 독립 개체로 관리 됩니다. 관계는 외래 키 속성 대신 개체 참조를 통해 추적 됩니다. 이러한 유형의 연결을 *독립 연결*이라고 합니다. *독립적인 연결* 을 수정 하는 가장 일반적인 방법은 연결에 참여 하는 각 엔터티에 대해 생성 되는 탐색 속성을 수정 하는 것입니다.

모델에서 두 종류의 연결 중 하나 또는 둘 모두를 사용할 수 있습니다. 그러나 외래 키만 포함 하는 조인 테이블로 연결 되는 순수한 다대다 관계가 있는 경우 EF는 독립 연결을 사용 하 여 이러한 다대다 관계를 관리 합니다.   

다음 그림은 Entity Framework Designer를 사용 하 여 만든 개념적 모델을 보여 줍니다. 모델은 일 대 다 관계에 참여 하는 두 개의 엔터티를 포함 합니다. 두 엔터티 모두 탐색 속성을 포함 합니다. **강좌** 는 종속 엔터티 이며 **DepartmentID** 외래 키 속성이 정의 되어 있습니다.

![탐색 속성을 사용 하는 부서 및 과정 테이블](~/ef6/media/relationshipefdesigner.png)

다음 코드 조각에서는 Code First를 사용 하 여 만든 것과 동일한 모델을 보여 줍니다.

``` csharp
public class Course
{
  public int CourseID { get; set; }
  public string Title { get; set; }
  public int Credits { get; set; }
  public int DepartmentID { get; set; }
  public virtual Department Department { get; set; }
}

public class Department
{
   public Department()
   {
     this.Courses = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a>관계 구성 또는 매핑

이 페이지의 나머지 부분에서는 관계를 사용 하 여 데이터에 액세스 하 고 조작 하는 방법을 설명 합니다. 모델에서 관계를 설정 하는 방법에 대 한 자세한 내용은 다음 페이지를 참조 하세요.

-   Code First에서 관계를 구성 하려면 [데이터 주석](~/ef6/modeling/code-first/data-annotations.md) 및 [흐름 API-관계](~/ef6/modeling/code-first/fluent/relationships.md)를 참조 하세요.
-   Entity Framework Designer를 사용 하 여 관계를 구성 하려면 [EF Designer와의 관계](~/ef6/modeling/designer/relationships.md)를 참조 하세요.

## <a name="creating-and-modifying-relationships"></a>관계 만들기 및 수정

*외래 키 연결*에서 관계를 변경 하면 `EntityState.Unchanged` 상태의 종속 개체 상태가 `EntityState.Modified`변경 됩니다. 독립적인 관계에서 관계를 변경 해도 종속 개체의 상태는 업데이트 되지 않습니다.

다음 예에서는 외래 키 속성 및 탐색 속성을 사용 하 여 관련 개체를 연결 하는 방법을 보여 줍니다. 외래 키 연결을 사용 하는 방법 중 하나를 사용 하 여 관계를 변경, 생성 또는 수정할 수 있습니다. 독립 연결을 사용할 경우에는 외래 키 속성을 사용할 수 없습니다.

- 다음 예제와 같이 외래 키 속성에 새 값을 할당 합니다.  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- 다음 코드는 외래 키를 **null**로 설정 하 여 관계를 제거 합니다. 외래 키 속성은 null을 허용 해야 합니다.  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > 참조가 추가 된 상태 (이 예제에서는 강좌 개체) 인 경우에는 SaveChanges가 호출 될 때까지 참조 탐색 속성이 새 개체의 키 값과 동기화 되지 않습니다. 개체 컨텍스트에는 추가된 개체에 대한 영구 키가 포함되어 있지 않으므로 해당 개체가 저장되기 전까지는 동기화가 수행되지 않습니다. 관계를 설정 하는 즉시 새 개체가 완전히 동기화 되어야 하는 경우 다음 방법 중 하나를 사용 합니다. *

- 탐색 속성에 새 개체를 할당합니다. 다음 코드는 강좌와 `department`간의 관계를 만듭니다. 개체를 컨텍스트에 연결 하면 `course` `department.Courses` 컬렉션에도 추가 되 고 `course` 개체의 해당 외래 키 속성이 학과의 키 속성 값으로 설정 됩니다.  
  ``` csharp
  course.Department = department;
  ```

- 관계를 삭제 하려면 탐색 속성을 `null`로 설정 합니다. .NET 4.0을 기반으로 하는 Entity Framework를 사용 하는 경우에는 관련 end를 먼저 로드 해야 null로 설정할 수 있습니다. 예를 들면,   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  .NET 4.5을 기반으로 하는 Entity Framework 5.0부터 관련 end를 로드 하지 않고 관계를 null로 설정할 수 있습니다. 다음 메서드를 사용 하 여 현재 값을 null로 설정할 수도 있습니다.   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- 엔터티 컬렉션에서 개체를 삭제하거나 추가합니다. 예를 들어 `Course` 형식의 개체를 `department.Courses` 컬렉션에 추가할 수 있습니다. 이 작업은 특정 **과정** 및 특정 `department`간의 관계를 만듭니다. 개체를 컨텍스트에 연결 하면 **과정** 개체의 부서 참조 및 외래 키 속성이 적절 한 `department`설정 됩니다.  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- `ChangeRelationshipState` 메서드를 사용 하 여 두 엔터티 개체 간 지정 된 관계의 상태를 변경 합니다. 이 메서드는 N 계층 응용 프로그램 및 *독립 연결* 을 사용할 때 가장 일반적으로 사용 됩니다 (외래 키 연결에는 사용할 수 없음). 또한이 방법을 사용 하려면 아래 예제와 같이 `ObjectContext`에 드롭다운 해야 합니다.  
다음 예제에는 강사와 강의 사이에 다 대 다 관계가 있습니다. `ChangeRelationshipState` 메서드를 호출 하 고 `EntityState.Added` 매개 변수를 전달 하면에서 두 개체 사이에 관계가 추가 되었음을 알 수 `SchoolContext` 있습니다.
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  관계를 추가 하는 것이 아니라 업데이트 하는 경우에는 새 관계를 추가한 후 이전 관계를 삭제 해야 합니다.

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a>외래 키와 탐색 속성 간의 변경 내용 동기화

위에서 설명한 방법 중 하나를 사용 하 여 컨텍스트에 연결 된 개체의 관계를 변경 하는 경우 Entity Framework 외래 키, 참조 및 컬렉션을 동기화 상태로 유지 해야 합니다. Entity Framework는 프록시가 있는 POCO 엔터티에 대해이 동기화 (관계 수정이 라고도 함)를 자동으로 관리 합니다. 자세한 내용은 [프록시 작업](~/ef6/fundamentals/proxies.md)을 참조 하세요.

프록시가 없는 POCO 엔터티를 사용 하는 경우 컨텍스트의 관련 개체를 동기화 하기 위해 **DetectChanges** 메서드가 호출 되는지 확인 해야 합니다. 다음 Api는 **DetectChanges** 호출을 자동으로 트리거합니다.

-   `DbSet.Add`
-   `DbSet.AddRange`
-   `DbSet.Remove`
-   `DbSet.RemoveRange`
-   `DbSet.Find`
-   `DbSet.Local`
-   `DbContext.SaveChanges`
-   `DbSet.Attach`
-   `DbContext.GetValidationErrors`
-   `DbContext.Entry`
-   `DbChangeTracker.Entries`
-   `DbSet`에 대해 LINQ 쿼리 실행

## <a name="loading-related-objects"></a>관련 개체 로드

Entity Framework에서는 일반적으로 탐색 속성을 사용 하 여 정의 된 연결에 의해 반환 된 엔터티와 관련 된 엔터티를 로드 합니다. 자세한 내용은 [관련 개체 로드](~/ef6/querying/related-data.md)를 참조 하세요.

> [!NOTE]
> 외래 키 연결에서 종속 개체의 관련된 끝을 로드하면 현재 메모리에 있는 종속 개체의 외래 키 값에 따라 관련 개체가 로드됩니다.

``` csharp
    // Get the course where currently DepartmentID = 2.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

독립 연결에서는 현재 데이터베이스에 있는 외래 키 값에 따라 종속 개체의 관련된 끝이 쿼리됩니다. 그러나 관계가 수정 되었으며 종속 개체에 대 한 참조 속성이 개체 컨텍스트에 로드 된 다른 보안 주체 개체를 가리키는 경우 Entity Framework는 클라이언트에 정의 된 대로 관계를 만들려고 시도 합니다.

## <a name="managing-concurrency"></a>동시성 관리

외래 키와 독립 연결 모두에서 동시성 검사는 모델에 정의 된 엔터티 키와 기타 엔터티 속성을 기반으로 합니다. EF Designer를 사용 하 여 모델을 만드는 경우 `ConcurrencyMode` 특성을 **fixed** 로 설정 하 여 동시성에 대해 속성을 확인 하도록 지정 합니다. Code First를 사용 하 여 모델을 정의 하는 경우 동시성을 확인 하려는 속성에 `ConcurrencyCheck` 주석을 사용 합니다. Code First 작업할 때 `TimeStamp` 주석을 사용 하 여 속성에 동시성을 확인 하도록 지정할 수도 있습니다. 지정 된 클래스에는 타임 스탬프 속성을 하나만 포함할 수 있습니다. Code First는 데이터베이스의 null을 허용 하지 않는 필드에이 속성을 매핑합니다.

동시성 검사 및 해결에 참여 하는 엔터티를 사용할 때는 항상 외래 키 연결을 사용 하는 것이 좋습니다.

자세한 내용은 [동시성 충돌 처리](~/ef6/saving/concurrency.md)를 참조 하세요.

## <a name="working-with-overlapping-keys"></a>겹치는 키 작업

겹치는 키는 키의 일부 속성이 엔터티에 있는 다른 키의 일부이기도 한 복합 키입니다. 독립 연결에서는 겹치는 키를 사용할 수 없습니다. 겹치는 키가 포함된 외래 키 연결을 변경하려면 개체 참조를 사용하는 대신 외래 키 값을 수정하는 것이 좋습니다.
