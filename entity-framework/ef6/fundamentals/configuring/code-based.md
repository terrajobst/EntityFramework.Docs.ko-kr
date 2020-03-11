---
title: 코드 기반 구성-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: 079a4ab30af74eac8b1f51ece5801ff40a867a29
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414802"
---
# <a name="code-based-configuration"></a>코드 기반 구성
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

구성 파일 (app.config/web.config) 또는 코드를 통해 Entity Framework 응용 프로그램에 대 한 구성을 지정할 수 있습니다. 후자를 코드 기반 구성 이라고 합니다.  

구성 파일의 구성은 [별도의 문서](config-file.md)에 설명 되어 있습니다. 구성 파일이 코드 기반 구성 보다 우선적으로 적용 됩니다. 즉, 구성 옵션이 코드와 구성 파일 모두에 설정 된 경우 구성 파일의 설정이 사용 됩니다.  

## <a name="using-dbconfiguration"></a>DbConfiguration 사용  

EF6 이상의 하위 클래스를 만들어 이러한 구성 요소의 코드 기반 구성을 수행할 수 있습니다. DbConfiguration를 서브클래싱 할 때 다음 지침을 따라야 합니다.  

- 응용 프로그램에 대해 DbConfiguration 클래스를 하나만 만듭니다. 이 클래스는 앱 도메인 전체 설정을 지정 합니다.  
- DbConfiguration 클래스를 DbContext 클래스와 동일한 어셈블리에 저장 합니다. (이를 변경 하려는 경우 *이동 DbConfiguration* 섹션을 참조 하세요.)  
- DbConfiguration 클래스에 매개 변수가 없는 public 생성자를 제공 합니다.  
- 이 생성자 내에서 보호 된 DbConfiguration 메서드를 호출 하 여 구성 옵션을 설정 합니다.  

이러한 지침에 따라 EF는 모델에 액세스 해야 하는 도구와 응용 프로그램이 실행 될 때 모두 자동으로 구성을 검색 하 고 사용할 수 있습니다.  

## <a name="example"></a>예제  

DbConfiguration에서 파생 된 클래스는 다음과 같습니다.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

namespace MyNamespace
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            SetDefaultConnectionFactory(new LocalDbConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

이 클래스는 SQL Azure 실행 전략을 사용 하도록 EF를 설정 하 여 실패 한 데이터베이스 작업을 자동으로 다시 시도 하 고 Code First에서 규칙으로 만들어진 데이터베이스에 로컬 DB를 사용 합니다.  

## <a name="moving-dbconfiguration"></a>DbConfiguration 이동  

DbConfiguration 클래스를 DbContext 클래스와 동일한 어셈블리에 저장할 수 없는 경우가 있습니다. 예를 들어 두 개의 DbContext 클래스가 서로 다른 어셈블리에 있을 수 있습니다. 이를 처리 하는 두 가지 옵션이 있습니다.  

첫 번째 옵션은 구성 파일을 사용 하 여 사용할 DbConfiguration 인스턴스를 지정 하는 것입니다. 이렇게 하려면 entityFramework 섹션의 codeConfigurationType 특성을 설정 합니다. 예를 들면 다음과 같습니다.  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

CodeConfigurationType의 값은 DbConfiguration 클래스의 어셈블리 및 네임 스페이스 정규화 된 이름 이어야 합니다.  

두 번째 옵션은 컨텍스트 클래스에 DbConfigurationTypeAttribute를 추가 하는 것입니다. 예를 들면 다음과 같습니다.  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

특성에 전달 된 값은 위에 표시 된 것 처럼 DbConfiguration 형식 이거나 어셈블리 및 네임 스페이스 정규화 된 형식 이름 문자열 일 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a>DbConfiguration 명시적 설정  

DbContext 형식을 사용 하기 전에 구성이 필요할 수 있는 경우가 있습니다. 예를 들면 다음과 같습니다.  

- DbModelBuilder를 사용 하 여 컨텍스트가 없는 모델 빌드  
- 응용 프로그램 컨텍스트를 사용 하기 전에 해당 컨텍스트가 사용 되는 DbContext를 활용 하는 다른 프레임 워크/유틸리티 코드 사용  

이러한 상황에서 EF는 구성을 자동으로 검색할 수 없으며 대신 다음 중 하나를 수행 해야 합니다.  

- 위의 *dbconfiguration* 섹션에서 설명한 대로 구성 파일에서 dbconfiguration 유형을 설정 합니다.
- 응용 프로그램 시작 중에 정적 DbConfiguration. SetConfiguration 메서드를 호출 합니다.  

## <a name="overriding-dbconfiguration"></a>DbConfiguration 재정의  

DbConfiguration에서 구성 집합을 재정의 해야 하는 경우도 있습니다. 이는 일반적으로 응용 프로그램 개발자가 수행 하는 것이 아니라 파생 된 DbConfiguration 클래스를 사용할 수 없는 타사 공급자 및 플러그 인을 통해 수행 됩니다.  

이를 위해 EntityFramework는 잠금 해제 되기 직전에 기존 구성을 수정할 수 있는 이벤트 처리기를 등록할 수 있도록 합니다.  또한 EF service locator에서 반환 하는 모든 서비스를 바꾸기 위한 sugar 메서드를 제공 합니다. 이를 사용 하는 방법은 다음과 같습니다.  

- EF를 사용 하기 전에 앱을 시작할 때 플러그 인 또는 공급자는이 이벤트에 대 한 이벤트 처리기 메서드를 등록 해야 합니다. 이는 응용 프로그램이 EF를 사용 하기 전에 발생 해야 합니다.  
- 이벤트 처리기는 교체 해야 하는 모든 서비스에 대해 ReplaceService를 호출 합니다.  

예를 들어 IDbConnectionFactory 및 DbProviderService를 대체 하려면 다음과 같이 처리기를 등록 합니다.  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

위의 코드에서 MyProviderServices 및 Myproviderservices는 서비스의 구현을 나타냅니다.  

다른 종속성 처리기를 추가 하 여 동일한 결과를 얻을 수도 있습니다.  

이러한 방식으로 DbProviderFactory 래핑할 수도 있지만이 경우 ef 외부의 DbProviderFactory를 사용 하는 것이 아니라 EF에만 영향을 줍니다. 이러한 이유로 이전 처럼 DbProviderFactory 래핑할 수 있습니다.  

또한 패키지 관리자 콘솔에서 마이그레이션을 실행 하는 경우와 같이 응용 프로그램 외부에서 실행 하는 서비스를 염두에 두어야 합니다. 콘솔에서 마이그레이션을 실행 하면 DbConfiguration 찾기가 시도 됩니다. 그러나 래핑된 서비스를 가져올 것인지 여부는 등록 된 이벤트 처리기의 위치에 따라 달라 집니다. DbConfiguration 생성의 일부로 등록 된 경우 코드를 실행 하 고 서비스를 래핑해야 합니다. 일반적으로이 경우에는 이러한 경우가 발생 하지 않습니다. 즉, 도구는 래핑된 서비스를 가져오지 않습니다.  
