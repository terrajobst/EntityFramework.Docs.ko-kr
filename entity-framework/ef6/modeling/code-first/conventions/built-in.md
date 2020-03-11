---
title: Code First 규칙-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
ms.openlocfilehash: 4d03a32db5d84eb37c22617a95005b272172a65d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415948"
---
# <a name="code-first-conventions"></a>Code First 규칙
Code First를 사용 하면 또는 Visual Basic .NET 클래스를 C# 사용 하 여 모델을 설명할 수 있습니다. 모델의 기본 셰이프는 규칙을 사용 하 여 검색 됩니다. 규칙은 Code First 작업할 때 클래스 정의에 따라 개념적 모델을 자동으로 구성 하는 데 사용 되는 규칙 집합입니다. 규칙은 System.web. ModelConfiguration 네임 스페이스에 정의 되어 있습니다.  

데이터 주석 또는 흐름 API를 사용 하 여 모델을 추가로 구성할 수 있습니다. 우선 순위는 흐름 API를 통해 구성 되 고 그 다음에 데이터 주석과 규칙을 통해 제공 됩니다. 자세한 내용은 [데이터 주석](~/ef6/modeling/code-first/data-annotations.md), [흐름 api-관계](~/ef6/modeling/code-first/fluent/relationships.md), [흐름 & 속성](~/ef6/modeling/code-first/fluent/types-and-properties.md) 및 [VB.NET를 사용 하 여 흐름 api](~/ef6/modeling/code-first/fluent/vb.md)를 참조 하세요.  

[API 설명서](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx)에서 제공 되는 Code First 규칙의 자세한 목록을 제공 합니다. 이 항목에서는 Code First에서 사용 하는 규칙의 개요를 제공 합니다.  

## <a name="type-discovery"></a>유형 검색  

Code First 개발을 사용 하는 경우 일반적으로 개념 (도메인) 모델을 정의 하는 .NET Framework 클래스를 작성 하는 것으로 시작 합니다. 클래스를 정의 하는 것 외에도 **DbContext** 에 모델에 포함 하려는 형식을 지정 해야 합니다. 이렇게 하려면 **DbContext** 에서 파생 되는 컨텍스트 클래스를 정의 하 고 모델에 포함할 형식의 **dbset** 속성을 노출 합니다. Code First는 이러한 형식을 포함 하 고 참조 된 형식이 다른 어셈블리에 정의 된 경우에도 모든 참조 된 형식을 가져옵니다.  

형식이 상속 계층 구조에 참여 하는 경우 기본 클래스에 대 한 **Dbset** 속성을 정의 하는 데 충분 하며 기본 클래스와 동일한 어셈블리에 있는 경우 파생 형식이 자동으로 포함 됩니다.  

다음 예에서는 **SchoolEntities** 클래스 (**부서**)에 하나의 **dbset** 속성만 정의 되어 있습니다. Code First는이 속성을 사용 하 여 참조 된 형식을 검색 하 고 가져옵니다.  

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

모델에서 형식을 제외 하려면 **Notmapped** 특성 또는 **dbmodelbuilder** 를 사용 합니다. 흐름 API를 무시 합니다.  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a>기본 키 규칙  

클래스의 속성 이름이 "ID" (대/소문자 구분 안 함) 이거나 클래스 이름 앞에 "ID"가 있는 경우 Code First는 속성이 기본 키 임을 유추 합니다. 기본 키 속성의 형식이 numeric 또는 GUID 이면 id 열로 구성 됩니다.  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a>관계 규칙  

Entity Framework 탐색 속성은 두 엔터티 형식 간의 관계를 탐색 하는 방법을 제공 합니다. 모든 개체에는 해당 개체가 참여하는 모든 관계에 대한 탐색 속성이 있습니다. 탐색 속성을 사용 하면 두 방향에서 관계를 탐색 하 고 관리할 수 있습니다. 참조 개체 (복합성이 하나 또는 0 또는 1)를 반환 하거나 복합성이 많은 경우 컬렉션을 반환 합니다. Code First는 형식에 정의 된 탐색 속성을 기준으로 관계를 유추 합니다.  

탐색 속성 외에 종속 개체를 나타내는 형식에 외래 키 속성을 포함 하는 것이 좋습니다. 주 기본 키 속성과 형식이 같으며 이름이 다음 형식 중 하나를 따르는 속성은 관계의 외래 키를 나타냅니다. '\<탐색 속성 이름\>\<주체 기본 키 속성 이름\>', '\<주 클래스 이름\>\<기본 키 속성 이름\>' 또는 '\<주 키 속성 이름\>'. 일치 하는 항목이 여러 개 있으면 위에 나열 된 순서로 우선 순위가 지정 됩니다. 외래 키 검색은 대/소문자를 구분 하지 않습니다. 외래 키 속성이 검색 되 면 외래 키의 null 허용 여부에 따라 관계의 복합성을 유추 Code First 합니다. 속성이 null을 허용 하면 관계가 옵션으로 등록 됩니다. 그렇지 않으면 필요에 따라 관계가 등록 됩니다.  

종속 엔터티의 외래 키가 null을 허용 하지 않는 경우 Code First는 관계에서 cascade delete를 설정 합니다. 종속 엔터티의 외래 키가 null Code First을 허용 하는 경우에는 관계에서 cascade delete를 설정 하지 않고, 주 서버가 삭제 되 면 외래 키가 null로 설정 됩니다. 규칙에 따라 검색 되는 복합성 및 cascade delete 동작은 흐름 API를 사용 하 여 재정의할 수 있습니다.  

다음 예에서는 탐색 속성과 외래 키를 사용 하 여 학과와 강의 클래스 간의 관계를 정의 합니다.  

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
> 동일한 형식 간에 여러 관계가 있는 경우 (예 **: person 클래스** **에** **ReviewedBooks** 및 흐름 **Edbooks** 탐색 속성이 포함 되 고 **book** 클래스에 **Author** **및** **검토자** 탐색 속성이 포함 되어 있는 경우) 데이터 주석 또는 API를 사용 하 여 관계를 수동으로 구성 해야 합니다. 자세한 내용은 [데이터 주석-관계](~/ef6/modeling/code-first/data-annotations.md) 및 [흐름](~/ef6/modeling/code-first/fluent/relationships.md)를 참조 하세요.  

## <a name="complex-types-convention"></a>복합 형식 규칙  

Code First 기본 키를 유추할 수 없는 클래스 정의를 검색 하는 경우 기본 키가 데이터 주석 또는 흐름 API를 통해 등록 되지 않으면 형식이 자동으로 복합 형식으로 등록 됩니다. 복합 형식 검색에서는 엔터티 형식을 참조 하 고 다른 형식의 컬렉션 속성에서 참조 되지 않는 속성이 형식에 포함 되어 있어야 합니다. 다음 클래스 정의를 지정 하는 경우 기본 키가 없기 때문에 **세부 정보** 는 복합 형식 임을 유추할 Code First.  

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

DbContext에서 사용할 연결을 검색 하는 데 사용 하는 규칙에 대해 알아보려면 [연결 및 모델](~/ef6/fundamentals/configuring/connection-strings.md)을 참조 하세요.  

## <a name="removing-conventions"></a>규칙 제거  

System.object 네임 스페이스에 정의 된 모든 규칙을 제거할 수 있습니다. 다음 예에서는 **PluralizingTableNameConvention**를 제거 합니다.  

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

사용자 지정 규칙은 EF6에서 지원 됩니다. 자세한 내용은 [사용자 지정 Code First 규칙](~/ef6/modeling/code-first/conventions/custom.md)을 참조 하세요.
