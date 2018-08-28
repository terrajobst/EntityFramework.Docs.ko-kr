---
title: DbContext-EF6 사용
author: divega
ms.date: 2016-10-23
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: f95f503c4e40e65503d5af0c1b686d0055728bfe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998148"
---
# <a name="working-with-dbcontext"></a>DbContext를 사용 하 여 작업

Entity Framework를 사용 하 여 쿼리, 삽입, 업데이트 및.NET 개체를 사용 하 여 데이터를 삭제 하려면, 먼저 해야 [모델을 만드는](~/ef6/modeling/index.md) 엔터티 및 데이터베이스의 테이블을 모델에 정의 된 관계를 매핑하는 합니다.

응용 프로그램 상호 작용 기본 클래스는 모델을 만든 후 `System.Data.Entity.DbContext` (컨텍스트 클래스 라고도 함). 모델에 연결 된 DbContext를 사용할 수 있습니다.
- 쓰기 및 쿼리를 실행 합니다.   
- 쿼리 결과를 엔터티 개체로 구체화
- 이러한 개체에 대 한 변경 내용 추적
- 개체 변경 내용을 다시 데이터베이스에 유지
- UI 컨트롤에 메모리에 개체 바인딩

이 페이지는 컨텍스트 클래스를 관리 하는 방법에 몇 가지 지침을 제공 합니다.  

## <a name="defining-a-dbcontext-derived-class"></a>파생 된 DbContext 클래스를 정의합니다.  

컨텍스트를 사용 하려면 DbContext에서 파생 되며 컨텍스트에서 지정 된 엔터티의 컬렉션을 나타내는 DbSet 속성을 표시 하는 클래스를 정의 하는 것이 좋습니다. EF 디자이너를 사용 하 여 작업 하는 경우 컨텍스트를 생성 됩니다. Code First를 사용 하 여 작업 하는 경우는 일반적으로 컨텍스트 직접 작성 합니다.  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

에 대 한 쿼리를 추가 하는 컨텍스트를 만든 후 (사용 하 여 `Add` 또는 `Attach` 메서드) 또는 제거 (사용 하 여 `Remove`) 이러한 속성을 통해 컨텍스트의 엔터티를에서 합니다. 액세스는 `DbSet` 컨텍스트 개체의 속성에 지정 된 형식의 모든 엔터티를 반환 하는 시작 쿼리를 나타냅니다. Note만 속성에 액세스 하는 쿼리 실행 되지 않습니다. 쿼리를 실행 하는 경우:  

- `foreach`(C#) 또는 `For Each`(Visual Basic) 문에 의해 열거된 경우  
- 컬렉션 작업에 의해 같은 열거 `ToArray`, `ToDictionary`, 또는 `ToList`합니다.  
- 와 같은 LINQ 연산자 `First` 또는 `Any` 쿼리의 가장 바깥쪽 부분에 지정 됩니다.  
- 호출 되는 다음 방법 중 하나: 합니다 `Load` 확장 메서드를 `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`, 및 `DbSet<T>.Find`없으면 지정된 된 키를 사용 하 여 엔터티의 이미 컨텍스트에 로드, 합니다.  

## <a name="lifetime"></a>수명  

컨텍스트의 수명 인스턴스가 생성 되 고 인스턴스를 삭제 또는 가비지 수집을 종료 하는 경우 시작 합니다. 사용 하 여 **를 사용 하 여** 컨텍스트에서 제어 블록의 끝에서 삭제 되도록 하려면 모든 리소스를 사용 하려는 경우. 사용 하는 경우 **를 사용 하 여**컴파일러가 자동으로 try/finally 블록을 만들고에서 dispose를 호출 합니다 **마지막으로** 블록입니다.  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

컨텍스트의 수명 결정 하는 경우 몇 가지 일반적인 지침 다음과 같습니다.  

- 웹 응용 프로그램에서 작업할 때 요청당 컨텍스트 인스턴스를 사용 합니다.  
- Windows Presentation Foundation (WPF) 또는 Windows Forms에서 작업할 때 폼 당 하나씩 컨텍스트 인스턴스를 사용 합니다. 이렇게 하면 변경 내용 추적 기능을 사용 하 여 해당 컨텍스트를 제공 합니다.  
- 컨텍스트 인스턴스는 종속성 주입 컨테이너에 의해 만들어진 경우 일반적으로 컨텍스트를 삭제 하려면 컨테이너의 책임입니다.
- 응용 프로그램 코드에서는 컨텍스트를 만들 경우 더 이상 필요한 경우 컨텍스트를 삭제 해야 합니다.  
- 장기 실행 컨텍스트를 사용 하 여 작업 하는 경우 다음 사항을 고려 합니다.  
    - 메모리에 더 많은 개체와 해당 참조를 로드할 때 컨텍스트의 메모리 소비 신속 하 게 늘릴 수 있습니다. 이로 인해 성능 문제가 발생할 수 있습니다.  
    - 컨텍스트는 스레드로부터 안전 하지 않습니다, 따라서 해당 공유 해서는 안 여러 스레드에서 동시에 대 한 작업을 수행 합니다.
    - 예외로 인해 컨텍스트를 복구할 수 없는 상태인 것으로, 전체 응용 프로그램에서 종료할 수 있습니다.  
    - 데이터가 쿼리되는 시간과 업데이트되는 시간의 간격이 커짐에 따라 동시성 관련 문제가 발생할 가능성이 높아집니다.  

## <a name="connections"></a>연결  

기본적으로 컨텍스트는 데이터베이스에 대 한 연결을 관리합니다. 컨텍스트 열리고 필요에 따라 연결을 닫습니다. 예를 들어 컨텍스트는 쿼리를 실행 하려면 연결을 열고 모든 결과 집합 처리 되 면 연결을 닫습니다.  

연결이 열리고 닫히는 때를 세부적으로 제어해야 하는 경우가 있습니다. 예를 들어, SQL Server Compact를 사용 하 여 작업을 하는 경우 종종 좋습니다 데이터베이스에 성능 향상을 위해 응용 프로그램의 수명에 대 한 별도 열린 연결을 유지 하기 위해. `Connection` 속성을 사용하여 이 프로세스를 수동으로 관리할 수 있습니다.  
