---
title: 동시성 충돌 처리-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: 81ae186201fdfac331b1d4e7836b222545fe78b5
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416248"
---
# <a name="handling-concurrency-conflicts"></a>동시성 충돌 처리
낙관적 동시성에는 엔터티가 로드 된 후 데이터가 변경 되지 않은 것으로 낙관적으로 데이터베이스에 엔터티를 저장 하려고 시도 하는 작업이 포함 됩니다. 데이터가 변경 된 것으로 확인 되 면 예외가 throw 되 고 저장을 다시 시도 하기 전에 충돌을 해결 해야 합니다. 이 항목에서는 Entity Framework에서 이러한 예외를 처리 하는 방법을 설명 합니다. 이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

이 게시물은 낙관적 동시성에 대 한 전체 설명을 위한 적절 한 위치가 아닙니다. 다음 섹션에서는 동시성 확인에 대 한 몇 가지 정보를 가정 하 고 일반적인 태스크에 대 한 패턴을 보여 줍니다.  

이러한 패턴은 대부분 [속성 값 작업](~/ef6/saving/change-tracking/property-values.md)에 설명 된 항목을 활용 합니다.  

외래 키가 엔터티의 속성에 매핑되지 않는 독립 연결을 사용할 때 동시성 문제를 해결 하는 것은 외래 키 연결을 사용 하는 경우 보다 훨씬 더 어렵습니다. 따라서 응용 프로그램에서 동시성을 확인 하려는 경우 항상 외래 키를 엔터티에 매핑하는 것이 좋습니다. 아래의 모든 예제에서는 외래 키 연결을 사용 한다고 가정 합니다.  

외래 키 연결을 사용 하는 엔터티를 저장 하려고 시도 하는 동안 낙관적 동시성 예외가 검색 되 면 SaveChanges에서 DbUpdateConcurrencyException이 throw 됩니다.  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a>다시 로드를 사용 하 여 낙관적 동시성 예외 해결 (데이터베이스 wins)  

Reload 메서드를 사용 하 여 엔터티의 현재 값을 데이터베이스의 현재 값으로 덮어쓸 수 있습니다. 엔터티는 일반적으로 특정 형식으로 사용자에 게 다시 제공 되며 다시 변경 하 고 다시 저장 해야 합니다. 예를 들면 다음과 같습니다.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;

        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update the values of the entity that failed to save from the store
            ex.Entries.Single().Reload();
        }

    } while (saveFailed);
}
```  

동시성 예외를 시뮬레이션 하는 좋은 방법은 SaveChanges 호출에 중단점을 설정한 다음 SQL Management Studio와 같은 다른 도구를 사용 하 여 데이터베이스에 저장 되는 엔터티를 수정 하는 것입니다. SaveChanges 앞에 줄을 삽입 하 여 SqlCommand를 사용 하 여 데이터베이스를 직접 업데이트할 수도 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

DbUpdateConcurrencyException의 Entries 메서드는 업데이트 하지 못한 엔터티에 대 한 DbEntityEntry 인스턴스를 반환 합니다. 이 속성은 현재 동시성 문제에 대 한 단일 값을 항상 반환 합니다. 일반 업데이트 예외에 대해 여러 값을 반환할 수 있습니다. 경우에 따라 데이터베이스에서 다시 로드 해야 할 수 있는 모든 엔터티에 대 한 항목을 가져오고 각 엔터티에 대해 다시 로드를 호출 하는 것이 원인일 수 있습니다.  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a>클라이언트 wins로 낙관적 동시성 예외 해결  

엔터티의 값을 데이터베이스의 값으로 덮어쓰기 때문에 다시 로드를 사용 하는 위의 예제를 데이터베이스 wins 또는 저장소 wins 라고도 합니다. 경우에 따라 데이터베이스의 값을 현재 엔터티에 있는 값으로 덮어쓸 수도 있습니다. 이를 클라이언트 wins 라고도 하며, 현재 데이터베이스 값을 가져와 엔터티에 대 한 원래 값으로 설정 하 여 수행할 수 있습니다. 현재 값과 원래 값에 대 한 자세한 내용은 [속성 값 작업](~/ef6/saving/change-tracking/property-values.md) 을 참조 하세요. 예를 들어:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update original values from the database
            var entry = ex.Entries.Single();
            entry.OriginalValues.SetValues(entry.GetDatabaseValues());
        }

    } while (saveFailed);
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a>낙관적 동시성 예외의 사용자 지정 확인  

현재 데이터베이스에 있는 값을 현재 엔터티에 있는 값과 결합할 수 있습니다. 일반적으로 사용자 지정 논리 또는 사용자 조작이 필요 합니다. 예를 들어 현재 값, 데이터베이스의 값 및 확인 된 값의 기본 집합을 포함 하는 사용자에 게 폼을 제공할 수 있습니다. 그런 다음 사용자는 필요에 따라 확인 된 값을 편집 하 고 이러한 값이 데이터베이스에 저장 됩니다. 이 작업은 CurrentValues에서 반환 된 DbPropertyValues 개체와 엔터티 항목의 GetDatabaseValues를 사용 하 여 수행할 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            var entry = ex.Entries.Single();
            var currentValues = entry.CurrentValues;
            var databaseValues = entry.GetDatabaseValues();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValues = databaseValues.Clone();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency(currentValues, databaseValues, resolvedValues);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValues);
        }
    } while (saveFailed);
}

public void HaveUserResolveConcurrency(DbPropertyValues currentValues,
                                       DbPropertyValues databaseValues,
                                       DbPropertyValues resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them edit the resolved values to get the correct resolution.
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a>개체를 사용한 낙관적 동시성 예외의 사용자 지정 확인  

위의 코드는 DbPropertyValues 인스턴스를 사용 하 여 현재, 데이터베이스 및 해결 된 값을 전달 합니다. 경우에 따라이에 대 한 엔터티 형식의 인스턴스를 사용 하는 것이 더 쉬울 수 있습니다. DbPropertyValues의 ToObject 및 SetValues 메서드를 사용 하 여이 작업을 수행할 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            // as instances of the entity type
            var entry = ex.Entries.Single();
            var databaseValues = entry.GetDatabaseValues();
            var databaseValuesAsBlog = (Blog)databaseValues.ToObject();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValuesAsBlog = (Blog)databaseValues.ToObject();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency((Blog)entry.Entity,
                                       databaseValuesAsBlog,
                                       resolvedValuesAsBlog);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValuesAsBlog);
        }

    } while (saveFailed);
}

public void HaveUserResolveConcurrency(Blog entity,
                                       Blog databaseValues,
                                       Blog resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them update the resolved values to get the correct resolution.
}
```  
