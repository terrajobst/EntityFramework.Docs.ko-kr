---
title: 연결 문자열 및 모델-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: 2c9f084107e4de7f5439bf0082b46a3b538496e0
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415954"
---
# <a name="connection-strings-and-models"></a>연결 문자열 및 모델
이 항목에서는 Entity Framework에서 사용할 데이터베이스 연결을 검색 하는 방법 및 변경할 수 있는 방법에 대해 설명 합니다. Code First 및 EF Designer를 사용 하 여 만든 모델은 모두이 항목에서 다룹니다.  

일반적으로 Entity Framework 응용 프로그램은 DbContext에서 파생 된 클래스를 사용 합니다. 이 파생 클래스는 기본 DbContext 클래스의 생성자 중 하나를 호출 하 여를 제어 합니다.  

- 컨텍스트에서 데이터베이스에 연결 하는 방법, 즉 연결 문자열을 찾거나 사용 하는 방법  
- 컨텍스트에서 Code First를 사용 하 여 모델을 계산 하거나 EF Designer를 사용 하 여 만든 모델을 로드할지 여부  
- 추가 고급 옵션  

다음 조각은 DbContext 생성자를 사용 하는 몇 가지 방법을 보여 줍니다.  

## <a name="use-code-first-with-connection-by-convention"></a>규칙에 따라 연결 Code First 사용  

응용 프로그램에서 다른 구성을 수행 하지 않은 경우 DbContext에서 매개 변수가 없는 생성자를 호출 하면 DbContext가 규칙에 따라 생성 된 데이터베이스 연결을 사용 하 여 Code First 모드로 실행 됩니다. 예를 들면 다음과 같습니다.  

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

이 예제에서 DbContext는 파생 컨텍스트 클래스 (BloggingContext)의 네임 스페이스로 정규화 된 이름을 데이터베이스 이름으로 사용 하 고 SQL Express 또는 LocalDB를 사용 하 여이 데이터베이스에 대 한 연결 문자열을 만듭니다. 둘 다 설치 된 경우 SQL Express가 사용 됩니다.  

Visual Studio 2010에는 기본적으로 SQL Express가 포함 되 고 Visual Studio 2012 이상에는 LocalDB가 포함 됩니다. 설치 하는 동안 EntityFramework NuGet 패키지는 사용할 수 있는 데이터베이스 서버를 확인 합니다. 그런 다음 NuGet 패키지는 규칙에 따라 연결을 만들 때 Code First 사용 하는 기본 데이터베이스 서버를 설정 하 여 구성 파일을 업데이트 합니다. SQL Express를 실행 하는 경우 사용 됩니다. SQL Express를 사용할 수 없는 경우 LocalDB가 기본값으로 등록 됩니다. 기본 연결 팩터리의 설정이 이미 포함 되어 있으면 구성 파일이 변경 되지 않습니다.  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a>규칙 및 지정 된 데이터베이스 이름으로 연결 Code First 사용  

응용 프로그램에서 다른 구성을 수행 하지 않은 경우 사용 하려는 데이터베이스 이름으로 DbContext의 문자열 생성자를 호출 하면 DbContext가 데이터베이스에 대해 규칙에 따라 생성 된 데이터베이스 연결을 사용 하 여 Code First 모드에서 실행 됩니다. 해당 이름입니다. 예를 들면 다음과 같습니다.  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

이 예제에서 DbContext는 데이터베이스 이름으로 "BloggingDatabase"를 사용 하 고 SQL Express (Visual Studio 2010와 함께 설치 됨) 또는 LocalDB (Visual Studio 2012과 함께 설치 됨)를 사용 하 여이 데이터베이스에 대 한 연결 문자열을 만듭니다. 둘 다 설치 된 경우 SQL Express가 사용 됩니다.  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a>App.config/web.config 파일에 연결 문자열을 사용 하 여 Code First 사용  

App.config 또는 web.config 파일에 연결 문자열을 배치 하도록 선택할 수 있습니다. 예를 들면 다음과 같습니다.  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

이 방법은 SQL Express 또는 LocalDB 이외의 데이터베이스 서버를 사용 하도록 DbContext에 지시 하는 쉬운 방법입니다. 위의 예에서는 SQL Server Compact Edition 데이터베이스를 지정 합니다.  

연결 문자열의 이름이 네임 스페이스 정규화를 사용 하거나 사용 하지 않는 컨텍스트 이름과 일치 하는 경우에는 매개 변수가 없는 생성자가 사용 될 때 DbContext에 의해 검색 됩니다. 연결 문자열 이름이 컨텍스트 이름과 다른 경우 연결 문자열 이름을 DbContext 생성자에 전달 하 여 Code First 모드에서이 연결을 사용 하도록 DbContext에 지시할 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

또는 DbContext 생성자에 전달 된 문자열에 대해 "name =\<연결 문자열 이름\>" 형식을 사용할 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

이 폼을 사용 하면 연결 문자열을 구성 파일에서 찾을 수 있습니다. 지정 된 이름을 가진 연결 문자열을 찾을 수 없는 경우 예외가 throw 됩니다.  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a>App.config/web.config 파일에 연결 문자열을 사용 하는 데이터베이스/Model First  

EF Designer를 사용 하 여 만든 모델은 모델이 이미 존재 하 고 응용 프로그램이 실행 될 때 코드에서 생성 되지 않기 때문에 Code First와 다릅니다. 모델은 일반적으로 프로젝트의 EDMX 파일로 존재 합니다.  

디자이너는 app.config 또는 web.config 파일에 EF 연결 문자열을 추가 합니다. 이 연결 문자열은 EDMX 파일에서 정보를 찾는 방법에 대 한 정보를 포함 하 고 있으므로 특수 합니다. 예를 들면 다음과 같습니다.  

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

또한 EF 디자이너는 연결 문자열 이름을 DbContext 생성자에 전달 하 여이 연결을 사용 하도록 DbContext에 지시 하는 코드도 생성 합니다. 예를 들면 다음과 같습니다.  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

연결 문자열은 사용할 모델의 세부 정보를 포함 하는 EF 연결 문자열 이므로 DbContext은 코드에서 계산 하는 데 Code First를 사용 하는 대신 기존 모델을 로드 하는 것을 알고 있습니다.  

## <a name="other-dbcontext-constructor-options"></a>기타 DbContext 생성자 옵션  

DbContext 클래스에는 고급 시나리오를 사용할 수 있는 다른 생성자 및 사용 패턴이 포함 되어 있습니다. 그 중 일부는 다음과 같습니다.  

- DbModelBuilder 클래스를 사용 하 여 DbContext 인스턴스를 인스턴스화하지 않고 Code First 모델을 빌드할 수 있습니다. 이 결과는 DbModel 개체입니다. 그런 다음 DbContext 인스턴스를 만들 준비가 되 면 DbContext 생성자 중 하나에이 DbModel 개체를 전달할 수 있습니다.  
- 데이터베이스 또는 연결 문자열 이름 뿐만 아니라 전체 연결 문자열을 DbContext에 전달할 수 있습니다. 기본적으로이 연결 문자열은 System.web. SqlClient 공급자와 함께 사용 됩니다. 이는 IConnectionFactory의 다른 구현을 컨텍스트로 설정 하 여 변경할 수 있습니다. 데이터베이스. DefaultConnectionFactory.  
- 기존 DbConnection 개체를 DbContext 생성자에 전달 하 여 사용할 수 있습니다. 연결 개체가 EntityConnection 인스턴스이면 연결에 지정 된 모델이 Code First를 사용 하 여 모델을 계산 하는 대신 사용 됩니다. 개체가 다른 형식의 인스턴스인 경우 (예: SqlConnection) 컨텍스트는이를 Code First 모드에 사용 합니다.  
- 기존 ObjectContext를 DbContext 생성자에 전달 하 여 기존 컨텍스트를 DbContext 래핑할 수 있습니다. 이는 ObjectContext를 사용 하지만 응용 프로그램의 일부 부분에서 DbContext를 활용 하려는 기존 응용 프로그램에 사용할 수 있습니다.  
