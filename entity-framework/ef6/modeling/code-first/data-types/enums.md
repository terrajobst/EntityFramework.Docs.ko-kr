---
title: 열거형 지원-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415792"
---
# <a name="enum-support---code-first"></a>열거형 지원-Code First
> [!NOTE]
> **EF5** 이상-이 페이지에서 설명 하는 기능, api 등은 Entity Framework 5에서 도입 되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

이 비디오 및 단계별 연습에서는 Entity Framework Code First에서 열거형 형식을 사용 하는 방법을 보여 줍니다. 또한 LINQ 쿼리에서 열거형을 사용 하는 방법도 보여 줍니다.

이 연습에서는 Code First 사용 하 여 새 데이터베이스를 만들지만 Code First를 사용 하 여 [기존 데이터베이스에 매핑할](~/ef6/modeling/code-first/workflows/existing-database.md)수도 있습니다.

열거형 지원은 Entity Framework 5에서 도입 되었습니다. 열거형, 공간 데이터 형식 및 테이블 반환 함수와 같은 새로운 기능을 사용 하려면 .NET Framework 4.5를 대상으로 해야 합니다. Visual Studio 2012는 기본적으로 .NET 4.5를 대상으로 합니다.

Entity Framework에서 열거형은 **Byte**, **Int16**, **Int32**, **Int64** 또는 **SByte**의 기본 형식을 가질 수 있습니다.

## <a name="watch-the-video"></a>비디오 보기
이 비디오는 Entity Framework Code First에서 열거형 형식을 사용 하는 방법을 보여 줍니다. 또한 LINQ 쿼리에서 열거형을 사용 하는 방법도 보여 줍니다.

제공한 **사람**: 줄리아 Kornich

**비디오**: [wmv](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [wmv (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)

## <a name="pre-requisites"></a>필수 구성 요소

이 연습을 완료 하려면 Visual Studio 2012, Ultimate, Premium, Professional 또는 Web Express edition을 설치 해야 합니다.

 

## <a name="set-up-the-project"></a>프로젝트 설정

1.  Visual Studio 2012 열기
2.  **파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트** 를 클릭 합니다.
3.  왼쪽 창에서 **Visual C\#** 을 클릭 한 다음 **콘솔** 템플릿을 선택 합니다.
4.  **Enumcodefirst** 를 프로젝트 이름으로 입력 하 고 **확인을** 클릭 합니다.

## <a name="define-a-new-model-using-code-first"></a>Code First를 사용 하 여 새 모델 정의

Code First 개발을 사용 하는 경우 일반적으로 개념 (도메인) 모델을 정의 하는 .NET Framework 클래스를 작성 하는 것으로 시작 합니다. 아래 코드는 부서 클래스를 정의 합니다.

또한이 코드는 DepartmentNames 열거형을 정의 합니다. 기본적으로 열거형은 **int** 형식입니다. 학과 클래스의 Name 속성은 DepartmentNames 유형입니다.

Program.cs 파일을 열고 다음 클래스 정의를 붙여 넣습니다.

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
 

## <a name="define-the-dbcontext-derived-type"></a>DbContext 파생 형식 정의

엔터티를 정의 하는 것 외에도 DbContext에서 파생 되 고 DbSet&lt;&gt; 속성을 노출 하는 클래스를 정의 해야 합니다. DbSet&lt;&gt; 속성을 사용 하면 컨텍스트에서 모델에 포함 하려는 형식을 알 수 있습니다.

DbContext 파생 형식의 인스턴스는 런타임 중에 엔터티 개체를 관리 합니다. 여기에는 데이터베이스의 데이터로 개체 채우기, 변경 내용 추적 및 데이터베이스에 데이터 유지가 포함 됩니다.

DbContext 및 DbSet 형식은 EntityFramework 어셈블리에서 정의 됩니다. EntityFramework NuGet 패키지를 사용 하 여이 DLL에 대 한 참조를 추가 합니다.

1.  솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 합니다.
2.  **NuGet 패키지 관리 ...** 를 선택 합니다.
3.  NuGet 패키지 관리 대화 상자에서 **온라인** 탭을 선택 하 고 **entityframework** 패키지를 선택 합니다.
4.  **설치**를 클릭합니다.

EntityFramework 어셈블리 외에도 System.componentmodel 및 system.xml 어셈블리에 대 한 참조도 추가 됩니다 (참고).

Program.cs 파일의 맨 위에 다음 using 문을 추가 합니다.

``` csharp
using System.Data.Entity;
```

Program.cs에서 컨텍스트 정의를 추가 합니다. 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a>데이터 유지 및 검색

Main 메서드가 정의 된 Program.cs 파일을 엽니다. Main 함수에 다음 코드를 추가 합니다. 이 코드는 컨텍스트에 새 부서 개체를 추가 합니다. 그런 다음 데이터를 저장 합니다. 또한이 코드는 이름이 DepartmentNames 인 부서를 반환 하는 LINQ 쿼리를 실행 합니다.

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

애플리케이션을 컴파일하고 실행합니다. 프로그램에서는 다음이 출력됩니다.

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a>생성 된 데이터베이스 보기

응용 프로그램을 처음 실행 하면 Entity Framework 데이터베이스를 만듭니다. Visual Studio 2012가 설치 되어 있으므로 데이터베이스가 LocalDB 인스턴스에 생성 됩니다. 기본적으로 Entity Framework은 파생 컨텍스트의 정규화 된 이름 (이 예에서는 **Enumcodefirst. EnumTestContext**) 뒤에 데이터베이스 이름을 부여 합니다. 기존 데이터베이스가 사용 되는 이후 시간입니다.  

데이터베이스를 만든 후 모델을 변경 하는 경우 Code First 마이그레이션를 사용 하 여 데이터베이스 스키마를 업데이트 해야 합니다. 마이그레이션을 사용 하는 예제는 [새 데이터베이스에 대 한 Code First를](~/ef6/modeling/code-first/workflows/new-database.md) 참조 하세요.

데이터베이스 및 데이터를 보려면 다음을 수행 합니다.

1.  Visual Studio 2012 주 메뉴에서 **보기** -&gt; **SQL Server 개체 탐색기**를 선택 합니다.
2.  LocalDB가 서버 목록에 없으면 **SQL Server** 에서 마우스 오른쪽 단추를 클릭 하 고 추가를 선택 **SQL Server** 기본 **Windows 인증** 을 사용 하 여 localdb 인스턴스에 연결 합니다.
3.  LocalDB 노드 확장
4.  **데이터베이스** 폴더를 펼침 새 데이터베이스를 확인 하 고 **부서** 테이블을 탐색 합니다. Code First이는 열거형 형식에 매핑되는 테이블을 만들지 않습니다.
5.  데이터를 보려면 테이블을 마우스 오른쪽 단추로 클릭 하 고 **데이터 보기** 를 선택 합니다.

## <a name="summary"></a>요약

이 연습에서는 Entity Framework Code First에서 열거형 형식을 사용 하는 방법을 살펴보았습니다. 
