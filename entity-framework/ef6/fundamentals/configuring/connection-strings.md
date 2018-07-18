---
title: 연결 문자열 및 EF6-모델
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
caps.latest.revision: 3
ms.openlocfilehash: ca597e68a5b3e2085612669ee81da10ba6969eeb
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122758"
---
# <a name="connection-strings-and-models"></a>연결 문자열 및 모델
이 항목에서는 Entity Framework를 사용 하려면 데이터베이스 연결을 검색 하는 방법 및이 변경 하는 방법을 설명 합니다. Code First 및 EF 디자이너를 사용 하 여 만든 모델은 모두이 항목에서 설명 됩니다.  

일반적으로 Entity Framework 응용 프로그램 DbContext에서 파생 된 클래스를 사용 합니다. 이 파생된 클래스는 컨트롤에 기본 DbContext 클래스의 생성자 중 하나를 호출 합니다.  

- 컨텍스트 데이터베이스에 연결 하는 방법을-즉, 어떻게 연결 문자열은 발견 하 고 사용  
- Code First를 사용 하 여 모델 계산 또는 EF 디자이너를 사용 하 여 만든 모델을 로드 컨텍스트를 사용할지 여부를  
- 추가 고급 옵션  

다음 코드 조각이 표시 일부의 DbContext 생성자를 사용할 수 있습니다.  

## <a name="use-code-first-with-connection-by-convention"></a>규칙에 따라 연결을 사용 하 여 Code First 사용  

응용 프로그램에서 다른 구성을 수행 하지 않은 경우 DbContext에서 매개 변수가 없는 생성자를 호출 하는 다음 규칙을 만든 데이터베이스 연결을 사용 하 여 Code First 모드에서 실행 되도록 DbContext 발생 합니다. 예를 들어:  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

이 예에서 DbContext에 파생된 컨텍스트 class—Demo.EF.BloggingContext—as 데이터베이스 이름은 네임 스페이스 정규화 된 이름을 사용 하 고 SQL Express 또는 LocalDB를 사용 하 여이 데이터베이스에 대 한 연결 문자열을 만듭니다. 둘 다 설치 된 SQL Express 사용 됩니다.  

Visual Studio 2010 기본 및 Visual Studio 2012에서 SQL Express를 포함 하 고 나중에 LocalDB를 포함 합니다. EntityFramework NuGet 패키지를 설치 하는 동안 데이터베이스 서버를 사용할 수를 확인 합니다. 다음 NuGet 패키지 규칙에 따라 연결을 만들 때에 Code First를 사용 하 여 기본 데이터베이스 서버를 설정 하 여 구성 파일을 업데이트 됩니다. SQL Express를 실행 하는 경우 사용 됩니다. SQL Express 사용할 수 없는 경우 다음 LocalDB은 등록할 기본값으로 대신 합니다. 기본 연결 팩터리가 대 한 설정이 포함 되어 있으면 해당 구성 파일에 변경 된 내용이 없습니다.  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a>규칙 및 지정 된 데이터베이스 이름으로 연결을 사용 하 여 Code First 사용  

응용 프로그램에서 다른 구성을 수행 하지 않은 경우 다음 사용 하려는 데이터베이스 이름으로 DbContext에서 string 생성자를 호출 하면 규칙 데이터베이스를 만든 데이터베이스 연결을 사용 하 여 Code First 모드에서 실행 되도록 DbContext 해당 이름입니다. 예를 들어:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

이 예제의 DbContext 데이터베이스 이름으로 "BloggingDatabase"를 사용 하 고 (Visual Studio 2010을 사용 하 여 설치) SQL Express 또는 LocalDB (Visual Studio 2012와 함께 설치 됨)를 사용 하 여이 데이터베이스에 대 한 연결 문자열을 만듭니다. 둘 다 설치 된 SQL Express 사용 됩니다.  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a>App.config/web.config 파일에서 연결 문자열에 Code First 사용  

연결 문자열을 app.config 또는 web.config 파일에 삽입할 수도 있습니다. 예를 들어:  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

이것은 DbContext 이외의 SQL Express 또는 LocalDB 데이터베이스 서버를 사용 하도록 지시 하는 쉬운 방법을-위의 예제에서는 SQL Server Compact Edition 데이터베이스를 지정 합니다.  

연결 문자열의 이름 (사용 하 여 또는 네임 스페이스 한정자 없이) 컨텍스트의 이름과 일치 하는 경우 다음 있습니다 DbContext에서 매개 변수가 없는 생성자를 사용 하는 경우. 연결 문자열 이름이 컨텍스트의 이름과 다른 경우 연결 문자열 이름을 DbContext 생성자에 전달 하 여 Code First 모드에서이 연결을 사용 하는 DbContext를 확인할 수 있습니다. 예를 들어:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

폼을 사용할 수 또는 "이름 =\<연결 문자열 이름\>" DbContext 생성자에 전달 된 문자열에 대 한 합니다. 예를 들어:  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

이 폼을 사용 하면 구성 파일에 대 한 연결 문자열을 예상 되는 명시적입니다. 지정 된 이름의 연결 문자열을 찾을 수 없으면 예외가 throw 됩니다.  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a>Database/Model First app.config/web.config 파일에서 연결 문자열을 사용 하 여  

EF 디자이너를 사용 하 여 만든 모델 다릅니다 Code First는 모델에 이미 있으며 응용 프로그램을 실행 하는 경우 코드에서 생성 되지 않습니다. 모델은 프로젝트에서 EDMX 파일을 일반적으로 존재합니다.  

디자이너는 EF 연결 문자열을 app.config 또는 web.config 파일에 추가 합니다. 이 연결 문자열은 EDMX 파일에서 정보를 찾는 방법에 대 한 정보를 포함 한다는 점에서 특별 합니다. 예를 들어:  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

또한 EF 디자이너의 연결 문자열 이름을 DbContext 생성자에 전달 하 여이 연결을 사용 하려면 DbContext를 지시 하는 코드를 생성 합니다. 예를 들어:  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

기존 모델 대신 코드에서 계산 하려면 Code First를 사용 하 여 로드에 DbContext를 알고 있는 연결 문자열은 사용 하도록 모델의 세부 정보를 포함 하는 EF 연결 문자열입니다.  

## <a name="other-dbcontext-constructor-options"></a>다른 DbContext 생성자 옵션  

DbContext 클래스는 다른 생성자 및 일부 고급 시나리오를 사용 하도록 설정 하는 사용 패턴을 포함 합니다. 이 다음과 같습니다.  

- DbContext 인스턴스를 인스턴스화하지 않고도 Code First 모델을 작성 하는 DbModelBuilder 클래스를 사용할 수 있습니다. 이 결과 DbModel 개체입니다. 다음에 DbContext 인스턴스를 만들 준비가 되었을 때이 DbModel 개체 DbContext 생성자 중 하나에 전달할 수 있습니다.  
- 데이터베이스 또는 연결 문자열 이름만 대신 dbcontext 전체 연결 문자열을 전달할 수 있습니다. 기본적으로이 연결 문자열은 System.Data.SqlClient 공급자로 사용 이 컨텍스트에 다른 구현의 IConnectionFactory 설정 하 여 변경할 수 있습니다. Database.DefaultConnectionFactory 합니다.  
- DbContext 생성자에 전달 하 여 기존 DbConnection 개체를 사용할 수 있습니다. 연결 개체의 인스턴스가 EntityConnection을 계산 하는 대신 사용 되는 연결에 지정 된 모델 됩니다 하는 경우 먼저 코드를 사용 하 여 모델을 합니다. 경우 개체는 다른 형식의 인스턴스-예를 들어 SqlConnection-컨텍스트에서 Code First 모드에 대 한 사용 됩니다.  
- 기존 ObjectContext 기존 컨텍스트를 래핑 DbContext를 만들려면 DbContext 생성자에 전달할 수 있습니다. 이 ObjectContext를 사용 하는 싶지만 DbContext 응용 프로그램의 일부를 활용 하기 위해 기존 응용 프로그램에 사용할 수 있습니다.  
