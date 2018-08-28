---
title: 열거형 지원-Code First – EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 529370ef7aba3975a36cbfdf5c439ee87b563c7c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997236"
---
# <a name="enum-support---code-first"></a>열거형 지원-Code First
> [!NOTE]
> **EF5 이상만** -Api 기능 등이이 페이지에 설명 된 Entity Framework 5에서 도입 되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

이 비디오 및 단계별 연습에서는 Entity Framework Code First를 사용 하 여 열거형을 사용 하는 방법을 보여 줍니다. 또한 LINQ 쿼리에서 열거형을 사용 하는 방법을 보여 줍니다.

새 데이터베이스를 만들려면이 연습에서는 Code First 사용 하지만 사용할 수도 있습니다 [Code First는 기존 데이터베이스에 매핑할](~/ef6/modeling/code-first/workflows/existing-database.md)합니다.

열거형 지원 Entity Framework 5에서 도입 되었습니다. 테이블 반환 함수, 열거형 및 공간 데이터 형식 등 새 기능을 사용 하려면.NET Framework 4.5 대상으로 해야 합니다. Visual Studio 2012는 기본적으로.NET 4.5를 대상으로 합니다.

Entity Framework의 열거형에는 다음 기본 형식이 있을 수 있습니다: **바이트**, **Int16**를 **Int32**를 **Int64** , 또는 **SByte**합니다.

## <a name="watch-the-video"></a>비디오를 시청 하세요.
이 비디오에는 Entity Framework Code First를 사용 하 여 열거형을 사용 하는 방법을 보여 줍니다. 또한 LINQ 쿼리에서 열거형을 사용 하는 방법을 보여 줍니다.

**제공한**: Julia Kornich

**비디오**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)

## <a name="pre-requisites"></a>필수 조건

Visual Studio 2012 Ultimate, Premium, Professional, Web Express edition이 연습을 완료 하려면 설치 해야 합니다.

 

## <a name="set-up-the-project"></a>프로젝트 설정

1.  Visual Studio 2012 열기
2.  에 **파일** 메뉴에서 **새로 만들기**를 클릭 하 고 **프로젝트**
3.  왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔** 템플릿
4.  입력 **EnumCodeFirst** 고 프로젝트의 이름으로 **확인**

## <a name="define-a-new-model-using-code-first"></a>Code First를 사용 하 여 새 모델을 정의 합니다.

Code First 개발을 사용 하는 경우 일반적으로 개념적 (도메인) 모델을 정의 하는.NET Framework 클래스를 작성 하 여 시작 합니다. 아래 코드는 부서 클래스를 정의합니다.

코드는 또한 DepartmentNames 열거형을 정의합니다. 기본적으로 열거형에는 **int** 형식입니다. 부서 클래스의 이름 속성 DepartmentNames 형식입니다.

Program.cs 파일을 열고 다음 클래스 정의 붙여 넣습니다.

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a>정의 DbContext 파생 형식

엔터티를 정의 하는 것 외에도 DbContext에서 파생 되며 DbSet을 노출 하는 클래스를 정의 해야&lt;TEntity&gt; 속성입니다. DbSet&lt;TEntity&gt; 모델에 포함 하려는 유형을 알고 있어야 하는 컨텍스트 속성을 사용 합니다.

런타임에 데이터베이스에서 데이터를 사용 하 여 개체를 채우는 포함 하는 엔터티 개체를 관리 하는 파생 된 DbContext 형식의 인스턴스, 데이터베이스, 추적 및 저장 데이터를 변경 합니다.

DbContext 형식과 DbSet EntityFramework 어셈블리에 정의 됩니다. EntityFramework NuGet 패키지를 사용 하 여이 DLL에 대 한 참조를 추가 합니다.

1.  솔루션 탐색기에서 프로젝트 이름을 오른쪽 단추로 클릭 합니다.
2.  선택 **NuGet 패키지 관리...**
3.  NuGet 패키지 관리 대화 상자에서 선택 합니다 **온라인** 탭을 선택 합니다 **EntityFramework** 패키지.
4.  클릭 **설치**

Note는 EntityFramework 어셈블리 외에도 System.Data.Entity 및 System.ComponentModel.DataAnnotations 어셈블리에 대 한 참조로도 함께 추가 됩니다.

추가 다음 Program.cs 파일의 맨 위에 있는 문을 사용 하 여:

``` csharp
using System.Data.Entity;
```

Program.cs의 상황에 맞는 정의 추가 합니다. 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a>유지 하 고 데이터를 검색 합니다.

Main 메서드에 정의 되어 있는 Program.cs 파일을 엽니다. 다음 코드를 Main 함수에 추가 합니다. 새 부서 개체 컨텍스트를 추가 하는 코드입니다. 데이터를 저장합니다. 코드는 또한 여기서 name은 DepartmentNames.English 부서를 반환 하는 LINQ 쿼리를 실행 합니다.

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

응용 프로그램을 컴파일하고 실행합니다. 프로그램에서는 다음이 출력됩니다.

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a>생성된 된 데이터베이스 뷰

처음으로 응용 프로그램을 실행 하면 Entity Framework를 데이터베이스를 만듭니다. Visual Studio 2012를 설치 했으므로 LocalDB 인스턴스에서 데이터베이스가 만들어집니다. 기본적으로 Entity Framework에 이름을 파생 컨텍스트의 정규화 된 이름 뒤에 오는 하는 데이터베이스 (이 예에 대 한 **EnumCodeFirst.EnumTestContext**). 사용할 기존 데이터베이스를 이후 시간입니다.  

참고로 하는 경우 변경 내용을 모델 데이터베이스가 만들어진 후에 사용 하는 Code First 마이그레이션을 데이터베이스 스키마를 업데이트 합니다. 참조 [Code First 새 데이터베이스로](~/ef6/modeling/code-first/workflows/new-database.md) 마이그레이션을 사용 하 여의 예입니다.

데이터베이스 및 데이터를 보려면 다음을 수행 합니다.

1.  Visual Studio 2012 주 메뉴에서 선택 **뷰**  - &gt; **SQL Server 개체 탐색기**합니다.
2.  LocalDB 서버 목록에 없는 경우에서 마우스 오른쪽 단추를 클릭 **SQL Server** 선택한 **SQL Server 추가** 기본값을 사용 하 여 **Windows 인증** 에 연결 하는 LocalDB 인스턴스
3.  LocalDB 노드를 확장 합니다.
4.  펼치려면 합니다 **데이터베이스** 폴더를 찾아 새 데이터베이스가 표시를 합니다 **부서** Code First 만들어지지는지 않습니다 열거형 형식으로 매핑하는 테이블을 보면 테이블
5.  데이터를 보고, 테이블을 마우스 오른쪽 단추로 클릭 합니다. 선택 **데이터 보기**

## <a name="summary"></a>요약

이 연습에서는 Entity Framework Code First를 사용 하 여 열거형을 사용 하는 방법에 살펴보았습니다. 
