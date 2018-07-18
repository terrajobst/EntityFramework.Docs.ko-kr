---
title: 첫 번째 규칙-EF6 코드
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
caps.latest.revision: 4
ms.openlocfilehash: b124a034ba900780cc4d7e1354408cd3995e874e
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121874"
---
# <a name="code-first-conventions"></a>코드의 첫 번째 규칙
먼저 코드를 사용 하면 C# 또는 Visual Basic.NET 클래스를 사용 하 여 모델을 설명할 수 있습니다. 모델의 기본 셰이프 규칙을 사용 하 여 검색 됩니다. 규칙은 자동으로 Code First를 사용 하 여 작업 하는 경우 클래스 정의 기반으로 개념적 모델을 구성 하는 데 사용 되는 규칙의 집합입니다. 규칙은 System.Data.Entity.ModelConfiguration.Conventions 네임 스페이스에 정의 됩니다.  

데이터 주석 또는 fluent API를 사용 하 여 모델을 추가로 구성할 수 있습니다. 데이터 주석 및 규칙 뒤에 흐름 API 통해 구성에 우선 순위가 지정 됩니다. 자세한 내용은 참조 하세요. [데이터 주석](~/ef6/modeling/code-first/data-annotations.md), [Fluent API-관계](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API-형식 및 속성](~/ef6/modeling/code-first/fluent/types-and-properties.md) 고 [VB.NET를사용하여FluentAPI](~/ef6/modeling/code-first/fluent/vb.md).  

Code First 규칙의 세부 목록을의 수를 [API 설명서](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx)합니다. 이 항목에서는 Code First에서 사용 된 규칙의 개요를 제공 합니다.  

## <a name="type-discovery"></a>형식 검색  

Code First 개발을 사용 하는 경우 일반적으로 개념적 (도메인) 모델을 정의 하는.NET Framework 클래스를 작성 하 여 시작 합니다. 클래스를 정의 하는 것 외에도 또한 수 있도록 해야 **DbContext** 모델에 포함 하려는 유형을 알고 있어야 합니다. 파생 되는 컨텍스트 클래스를 정의 하면이 위해 **DbContext** 노출 **DbSet** 모델의 일부가 되도록 하려는 형식에 대 한 속성입니다. 코드에 먼저 이러한 형식은 포함 하 고도 가져오게 됩니다 모든 참조 형식을 참조 된 형식을 다른 어셈블리에 정의 된 경우에 합니다.  

형식 상속 계층 구조에 참여할 경우 정의 하기에 충분 한 **DbSet** 기본 클래스와 동일한 어셈블리에 있는 경우 기본 클래스 및 파생된 형식에 대 한 속성을 자동으로 포함 됩니다.  

다음 예에서 하나씩만 있기 **DbSet** 에 정의 된 속성을 **SchoolEntities** 클래스 (**부서**). 코드는 먼저 검색 하 고 모든 참조 된 형식에서 끌어오기를이 속성을 사용 합니다.  

``` csharp
public class SchoolEntities : DbContext
{
    public DbSet<Department> Departments { get; set; }
}

public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public string Location { get; set; }
    public string Days { get; set; }
    public System.DateTime Time { get; set; }
}
```  

모델에서 형식을 제외 하려는 경우 사용 합니다 **NotMapped** 특성 또는 **DbModelBuilder.Ignore** fluent API.  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a>기본 키 규칙  

먼저 코드는 클래스의 속성 (없습니다 대/소문자 구분), "ID" 라고 합니다. 또는 클래스 이름 뒤에 "ID"는 경우 속성이 기본 키 임을 유추 합니다. 기본 키 속성의 형식이 숫자 또는 GUID 경우 id 열으로 구성 합니다.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a>관계 규칙  

Entity Framework, 탐색 속성에는 두 엔터티 형식 간의 관계를 탐색 하는 방법을 제공 합니다. 모든 개체에는 해당 개체가 참여하는 모든 관계에 대한 탐색 속성이 있습니다. 탐색 속성을 사용 하면 탐색 하 고 참조 개체를 반환 합니다. 양방향에서 관계 관리 (복합성이 중 하나인 경우 또는 0 또는 1) 또는 컬렉션 (복합성이 많은) 경우입니다. 코드는 먼저 여러분의 형식에 정의 된 탐색 속성을 기준으로 관계를 유추 합니다.  

탐색 속성 외에도 종속 개체를 나타내는 형식에 외래 키 속성을 포함 하는 것이 좋습니다. 계정 기본 키 속성으로 동일한 데이터 형식 및 이름을 다음 형식 중 하나를 따르는 속성 관계에 대 한 외래 키를 나타냅니다. '\<탐색 속성 이름\>\<주 기본 키 속성 이름\>','\<주 클래스 이름\>\<기본 키 속성 이름\>', 또는 '\<기본 키 속성을 주체 이름\>'. 여러 일치 항목이 발견 되 면 우선 순위는 위에 나열 된 순서로 지정 됩니다. 외래 키 검색에 대/소문자 구분 없는 경우 Code First는 외래 키 속성이 검색 되 면 외래 키의 null 허용 여부에 따라 관계의 복합성을 유추 합니다. 속성이 null을 허용 하는 경우 다음 관계로 등록 되어 선택 사항입니다. 관계에 등록은 필요에 따라 합니다.  

종속 엔터티에 외래 키를 nullable 없는 경우 Code First 설정한 cascade delete 관계에 있습니다. 종속 엔터티에 외래 키에서 null을 허용 하 고 Code First에서 설정 하지 않습니다 cascade delete 관계 보안 주체가 삭제 될 때 외래 키 설정이 적용 됩니다 하는 경우 null입니다. 복합성 및 cascade 동작에서 검색을 삭제 흐름 API를 사용 하 여 규칙을 재정의할 수 있습니다.  

다음 예제에서는 탐색 속성 및 foreign key 부서 및 강좌 클래스 간의 관계를 정의 하려면 사용 됩니다.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}
```  

> [!NOTE]
> 동일한 형식 간에 여러 관계가 있는 경우 (예를 들어, 정의한를 **Person** 및 **책** 클래스 위치를 **Person** 클래스를 포함 합니다  **ReviewedBooks** 하 고 **AuthoredBooks** 탐색 속성 및 **책** 클래스를 포함 합니다 **작성자** 및  **검토자** 탐색 속성) 데이터 주석 또는 fluent API를 사용 하 여 관계를 수동으로 구성 해야 합니다. 자세한 내용은 [데이터 주석-관계](~/ef6/modeling/code-first/data-annotations.md) 하 고 [Fluent API-관계](~/ef6/modeling/code-first/fluent/relationships.md)합니다.  

## <a name="complex-types-convention"></a>복합 형식 규칙  

Code First는 기본 키를 유추할 수 없습니다, 그리고 및 기본 키 데이터 주석 또는 fluent API를 통해 등록 된 클래스 정의 검색 하는 경우 형식은 복합 형식으로 등록 자동으로 됩니다. 복합 형식 검색 형식에는 엔터티 형식을 참조 하 고 다른 형식의 컬렉션 속성에서 참조 되지 않는 속성 없는지도 필요 합니다. 다음 클래스 정의 Code First 유추 하는 **세부 정보** 기본 키를 있기 때문에 복합 유형입니다.  

``` csharp
public partial class OnsiteCourse : Course
{
    public OnsiteCourse()
    {
        Details = new Details();
    }

    public Details Details { get; set; }
}

public class Details
{
    public System.DateTime Time { get; set; }
    public string Location { get; set; }
    public string Days { get; set; }
}
```  

## <a name="connection-string-convention"></a>연결 문자열 규칙  

DbContext 참조를 사용 하 여 연결을 검색 하는 규칙에 알아보려면 [모델과 연결](~/ef6/fundamentals/configuring/connection-strings.md)합니다.  

## <a name="removing-conventions"></a>규칙 제거  

System.Data.Entity.ModelConfiguration.Conventions 네임 스페이스에 정의 된 규칙을 제거할 수 있습니다. 다음 예제에서는 제거 **PluralizingTableNameConvention**합니다.  

``` csharp
public class SchoolEntities : DbContext
{
     . . .

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention, the generated tables  
        // will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}
```  

## <a name="custom-conventions"></a>사용자 지정 규칙  

사용자 지정 규칙은 ef6에서부터 지원 됩니다. 자세한 내용은 참조 [사용자 지정 코드의 첫 번째 규칙](~/ef6/modeling/code-first/conventions/custom.md)합니다.
