---
title: 관계, 탐색 속성 및 EF6 외래 키
author: divega
ms.date: 2016-10-23
ms.assetid: 8a21ae73-6d9b-4b50-838a-ec1fddffcf37
ms.openlocfilehash: a1653afd609280ab572ef88a9fcf8a6275b79fd6
ms.sourcegitcommit: a81aed575372637997b18a0f9466d8fefb33350a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2018
ms.locfileid: "43821402"
---
# <a name="relationships-navigation-properties-and-foreign-keys"></a>관계, 탐색 속성 및 외래 키
이 항목에서는 Entity Framework는 엔터티 간의 관계를 관리 하는 방법의 개요를 제공 합니다. 또한 매핑 관계를 조작 하는 방법에 대 한 지침을 제공 합니다.

## <a name="relationships-in-ef"></a>EF의 관계

관계형 데이터베이스에서 테이블 간의 관계 (연결이 라고도 함)는 외래 키를 통해 정의 됩니다. 외래 키 (FK)는 열 또는 설정 하 고 두 테이블의 데이터 간 연결을 강제 적용 하는 데 사용 되는 열 조합. 세 가지 유형의 관계는 일반적으로: 한 일, 1 대 다 및 다 대 다. 1 대 다 관계에서 외래 키 관계의 다 쪽에 있는 테이블에 정의 됩니다. 다 대 다 관계는 두 관련된 테이블의 외래 키의 기본 키가 포함 구성 됩니다 (병합 또는 조인 테이블 이라고 함), 세 번째 테이블 정의 포함 됩니다. 한 일 관계에서 기본 키 또한 외래 키로 작동 하 고 두 테이블 중 하나에 대 한 별도 외래 키 열이 없습니다.

다음 이미지에 일 대 다 관계에 참여 하는 두 테이블을 보여 줍니다. **과정** 있기 때문에 테이블은 종속 테이블 합니다 **DepartmentID** 에 연결 하는 열을 **부서** 테이블.

![Database2](~/ef6/media/database2.png)

Entity Framework의 엔터티 연결 또는 관계를 통해 다른 엔터티와 연결할 수 있습니다. 각 관계는 엔터티 형식 및 해당 관계의 두 엔터티 형식 (1, 0 또는 1 인 이상의)의 다중성에 설명 하는 두 개의 끝을 포함 합니다. 관계는 주 역할 관계의 끝을 설명 하는 참조 제약 조건에 의해 관리 되기도 종속 역할 인지 합니다.

탐색 속성에는 두 엔터티 형식 간의 연결을 탐색 하는 방법을 제공 합니다. 모든 개체에는 해당 개체가 참여하는 모든 관계에 대한 탐색 속성이 있습니다. 탐색 속성을 사용 하면 탐색 하 고 참조 개체를 반환 합니다. 양방향에서 관계 관리 (복합성이 중 하나인 경우 또는 0 또는 1) 또는 컬렉션 (복합성이 많은) 경우입니다. 수도 있습니다 단방향 탐색 하는 경우에 탐색 속성 관계에 참여 하는 형식 중 하나 에서만 및 정의한 둘 다에 없는 합니다.

데이터베이스의 외래 키에 매핑되는 모델의 속성을 포함 하는 것이 좋습니다. 외래 키 속성이 포함되어 있으면 종속 개체에서 외래 키 값을 수정하여 관계를 만들거나 변경할 수 있습니다. 이러한 연결 종류를 외래 키 연결이라고 합니다. 외래 키를 사용 하 여 연결이 끊어진된 엔터티를 사용 하 여 작업 하는 경우 훨씬 더 반드시 합니다. 참고로 때 1-0 또는 1-1을 사용 하 여 작업... 1 관계를 별도 외래 키 열이 없습니다 하 고 기본 키 속성의 외래 키로 모델에 항상 포함 됩니다.

모델에 외래 키 열 포함 되지 않습니다, 경우 연결 정보는 독립적 개체로 관리 됩니다. 관계는 외래 키 속성 대신 개체 참조를 통해 추적 됩니다. 이 유형의 연결 호출 되는 *독립 연결*합니다. 수정 하는 가장 일반적인 방법은 *독립 연결* 연결에 참여 하는 각 엔터티에 대해 생성 되는 탐색 속성을 수정 하는 것입니다.

모델에서 두 종류의 연결 중 하나 또는 둘 모두를 사용할 수 있습니다. 그러나 순수 다 대 다 관계에서 외래 키만 포함 하는 조인 테이블이 연결 된 경우는 EF는 이러한 다 대 다 관계를 관리 하는 독립 연결을 사용 합니다.   

다음 이미지에서는 Entity Framework 디자이너를 사용 하 여 생성 된 개념적 모델을 보여 줍니다. 모델에 일 대 다 관계에 참여 하는 두 개의 엔터티가 포함 되어 있습니다. 두 엔터티는 탐색 속성입니다. **코스** 종속 엔터티가 있고 합니다 **DepartmentID** 외래 키 속성을 정의 합니다.

![RelationshipEFDesigner](~/ef6/media/relationshipefdesigner.png)

다음 코드 조각은 Code First를 사용 하 여 만든 동일한 모델을 보여 줍니다.

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
     this.Course = new HashSet<Course>();
   }  
   public int DepartmentID { get; set; }
   public string Name { get; set; }
   public decimal Budget { get; set; }
   public DateTime StartDate { get; set; }
   public int? Administrator {get ; set; }
   public virtual ICollection<Course> Courses { get; set; }
}
```

## <a name="configuring-or-mapping-relationships"></a>구성 또는 관계 매핑

이 페이지의 나머지 부분에서는 관계를 사용 하 여 데이터 액세스 및 조작 하는 방법을 설명 합니다. 모델의 관계 설정에 대 한 내용은 다음 페이지를 참조 합니다.

-   Code First에서 관계를 구성 하려면 [데이터 주석](~/ef6/modeling/code-first/data-annotations.md) 하 고 [Fluent API-관계](~/ef6/modeling/code-first/fluent/relationships.md)합니다.
-   Entity Framework 디자이너를 사용 하 여 관계를 구성 하려면 참조 [EF 디자이너를 사용 하 여 관계](~/ef6/modeling/designer/relationships.md)합니다.

## <a name="creating-and-modifying-relationships"></a>관계 만들기 및 수정

에 *외래 키 연결*관계를 포함 하는 종속 개체의 상태를 변경 하는 경우는 `EntityState.Unchanged` 상태가 변경 `EntityState.Modified`합니다. 독립적 관계에 관계 변경는 종속 개체의 상태를 업데이트 되지 않습니다.

다음 예제에서는 관련된 개체를 연결 하는 외래 키 속성과 탐색 속성을 사용 하는 방법을 보여 줍니다. 외래 키 연결을 사용 하 여 변경, 생성 또는 관계를 수정 하려면 두 방법 중 하나를 사용할 수 있습니다. 독립 연결을 사용할 경우에는 외래 키 속성을 사용할 수 없습니다.

- 다음 예제와 같이 외래 키 속성에 새 값을 할당 합니다.  
  ``` csharp
  course.DepartmentID = newCourse.DepartmentID;
  ```

- 다음 코드는 외래 키를 설정 하 여 관계를 제거 **null**합니다. 참고, 외래 키 속성이 null을 허용 해야 합니다.  
  ``` csharp
  course.DepartmentID = null;
  ```

  >[!NOTE]
  > (이 예에서는 과정 개체)에 추가 된 상태에 대 한 참조 인 경우 참조 탐색 속성이 동기화 되지 않습니다는 새 개체의 키 값을 사용 하 여 SaveChanges가 호출 될 때까지 합니다. 개체 컨텍스트에는 추가된 개체에 대한 영구 키가 포함되어 있지 않으므로 해당 개체가 저장되기 전까지는 동기화가 수행되지 않습니다. 관계를 설정 하는 즉시 완전 하 게 동기화 하는 새 개체를 해야 하는 경우 다음 methods.* 중 하나 사용

- 탐색 속성에 새 개체를 할당합니다. 다음 코드는 과정을 사이의 관계를 만듭니다 및 `department`합니다. 개체 컨텍스트에 연결 하는 경우는 `course` 에 추가 됩니다는 `department.Courses` 컬렉션 및 해당 외래 키 속성에는 `course` 개체의 키 속성 값을로 설정 됩니다.  
  ``` csharp
  course.Department = department;
  ```

- 관계를 삭제 하려면 탐색 속성을 설정 `null`합니다. .NET 4.0을 기반으로 하는 Entity Framework를 사용 하 여 작업 하는 경우 관련된 end를 null로 설정 하기 전에 로드 되도록 해야 합니다. 예를 들어:   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).Load();
  course.Department = null;
  ```

  .NET 4.5를 기반으로 하는 Entity Framework 5.0부터 관련된 end를 로드 하지 않고 null로 관계를 설정할 수 있습니다. 또한 다음 메서드를 사용 하 여 null로 현재 값을 설정할 수 있습니다.   
  ``` csharp
  context.Entry(course).Reference(c => c.Department).CurrentValue = null;
  ```

- 엔터티 컬렉션에서 개체를 삭제하거나 추가합니다. 예를 들어 형식의 개체를 추가할 수 있습니다 `Course` 에 `department.Courses` 컬렉션입니다. 이 작업은 특정 간의 관계를 만듭니다 **코스** 및 특정 `department`합니다. 개체 컨텍스트, 부서 참조 및 외래 키 속성에 연결 되어 있으면 합니다 **코스** 적절 한 개체를 설정할 `department`합니다.  
  ``` csharp
  department.Courses.Add(newCourse);
  ```

- 사용 하 여는 `ChangeRelationshipState` 두 엔터티 개체 간 관계의 상태를 변경 하는 방법입니다. N 계층 응용 프로그램을 사용 하 여 작업 하는 경우에 가장 일반적으로이 메서드는 및 *독립 연결* (사용할 수 없습니다는 외래 키 연결을 사용 하 여). 또한이 방법을 사용 하려면 삭제 해야 합니다 아래로 `ObjectContext`아래 예제에 나와 있는 것 처럼 합니다.  
다음 예제에서는 강사 및 강좌 다 대 다 관계가 있습니다. 호출을 `ChangeRelationshipState` 메서드와 전달을 `EntityState.Added` 매개 변수 수는 `SchoolContext` 두 개체 간의 관계 추가 된 것을 알고:
  ``` csharp

  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, instructor, c => c.Instructor, EntityState.Added);
  ```

  (뿐 아니라 추가)을 업데이트 하는 경우 관계를 새로 추가한 후 기존 관계를 삭제 해야 합니다.

  ``` csharp
  ((IObjectContextAdapter)context).ObjectContext.
    ObjectStateManager.
    ChangeRelationshipState(course, oldInstructor, c => c.Instructor, EntityState.Deleted);
  ```

## <a name="synchronizing-the-changes-between-the-foreign-keys-and-navigation-properties"></a>외래 키 속성과 탐색 간에 변경 내용 동기화

위에서 설명한 방법 중 하나를 사용 하 여 컨텍스트에 연결 된 개체의 관계를 변경 하면 Entity Framework에서는 외래 키, 참조 및 컬렉션에 동기화를 유지 해야 합니다. Entity Framework는 자동으로 프록시를 사용 하 여 POCO 엔터티에 대 한 (라고도 관계 픽스업)이이 동기화를 관리합니다. 자세한 내용은 [프록시를 사용 하 여 작업](~/ef6/fundamentals/proxies.md)합니다.

프록시가 없는 POCO 엔터티를 사용 하는 경우 해야 하는 **DetectChanges** 메서드를 호출 하는 컨텍스트에서 관련된 개체를 동기화 합니다. 이때 다음과 같은 Api를 자동으로 트리거하는 **DetectChanges** 호출 합니다.

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
-   에 대 한 쿼리는 LINQ를 실행 한 `DbSet`

## <a name="loading-related-objects"></a>관련 개체 로드

가장 일반적으로 사용 하면 Entity Framework에서 탐색 속성을 사용 하 여 정의 된 연결에 의해 반환된 된 엔터티를 관련 된 엔터티를 로드 합니다. 자세한 내용은 [관련 개체 로드](~/ef6/querying/related-data.md)합니다.

> [!NOTE]
> 외래 키 연결에서 종속 개체의 관련된 끝을 로드하면 현재 메모리에 있는 종속 개체의 외래 키 값에 따라 관련 개체가 로드됩니다.

``` csharp
    // Get the course where currently DepartmentID = 1.
    Course course2 = context.Courses.First(c=>c.DepartmentID == 2);

    // Use DepartmentID foreign key property
    // to change the association.
    course2.DepartmentID = 3;

    // Load the related Department where DepartmentID = 3
    context.Entry(course).Reference(c => c.Department).Load();
```

독립 연결에서는 현재 데이터베이스에 있는 외래 키 값에 따라 종속 개체의 관련된 끝이 쿼리됩니다. 그러나 관계가 수정 되었고 및 Entity Framework 개체 컨텍스트에 로드 되는 다른 보안 주체 개체에 종속 개체 요소에 대해 참조 속성으로 관계를 만들려고 시도 하는 경우 클라이언트에서 정의 됩니다.

## <a name="managing-concurrency"></a>동시성 관리

외래 키 연결과 독립 연결 모두에서 동시성 검사는 엔터티 키 및 모델에 정의 된 다른 엔터티 속성을 기반으로 합니다. EF 디자이너를 사용 하 여 모델 만들기를 설정 합니다 `ConcurrencyMode` 특성을 **고정** 동시성 속성을 검사 해야 않도록 지정 합니다. 사용 하 여 Code First 모델 정의 사용 하 여는 `ConcurrencyCheck` 속성을 검사할 동시성에 대 한 주석입니다. Code First를 사용 하 여 작업할 때 사용할 수도 있습니다는 `TimeStamp` 주석 동시성에 대 한 속성을 검사 해야 않도록 지정 합니다. 지정된 된 클래스의 타임 스탬프 속성을 하나만 있습니다. 코드는 먼저 데이터베이스에서 null이 아닌 필드에이 속성을 매핑합니다.

동시성 검사 및 확인에 참여 하는 엔터티를 사용 하 여 작업할 때 외래 키 연결을 항상 사용 하는 것이 좋습니다.

자세한 내용은 [동시성 충돌 처리](~/ef6/saving/concurrency.md)합니다.

## <a name="working-with-overlapping-keys"></a>겹치는 키 사용

겹치는 키는 키의 일부 속성이 엔터티에 있는 다른 키의 일부이기도 한 복합 키입니다. 독립 연결에서는 겹치는 키를 사용할 수 없습니다. 겹치는 키가 포함된 외래 키 연결을 변경하려면 개체 참조를 사용하는 대신 외래 키 값을 수정하는 것이 좋습니다.
