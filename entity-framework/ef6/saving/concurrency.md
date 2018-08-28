---
title: 동시성 충돌 처리-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: f233af217287dd6bf35e5b7fea8e44974168b312
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997812"
---
# <a name="handling-concurrency-conflicts"></a>동시성 충돌 처리
낙관적 동시성은 낙관적으로 로드 된 데이터베이스 데이터 엔터티 이후 변경 되지 않은 기대에 엔터티를 저장 하려고 합니다. 것으로 확인 되는 경우 예외가 발생 하 고 다시 저장 하기 전에 충돌을 해결 해야 하는 데이터가 변경 되었습니다. 이 항목에서는 Entity Framework에서 이러한 예외를 처리 하는 방법에 설명 합니다. 이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.  

이 게시물에 대 한 전체 설명은 낙관적 동시성을 적절 한 위치로 아닙니다. 아래 섹션에서는 동시성 확인에 대 한 지식을 가정 하 고 일반적인 작업에 대 한 패턴을 보여 줍니다.  

이러한 패턴의 대부분에서 다루는 항목 사용 [속성 값을 사용 하 여 작업](~/ef6/saving/change-tracking/property-values.md)합니다.  

(여기서 외래 키에 매핑되지 않은 엔터티에 속성) 독립 연결을 사용할 때 동시성 문제를 해결 하는 것은 외래 키 연결을 사용할 때 보다 훨씬 더 어렵습니다. 따라서 응용 프로그램에서 동시성 확인을 수행 하려는 경우 엔터티를 외래 키에 항상 매핑하는 것이 좋습니다. 아래의 모든 예제는 외래 키 연결을 사용 한다고 가정 합니다.  

외래 키 연결을 사용 하는 엔터티를 저장 하는 동안 낙관적 동시성 예외가 발견 되는 DbUpdateConcurrencyException SaveChanges에서 throw 됩니다.  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a>다시 로드 (데이터베이스 wins)를 사용 하 여 낙관적 동시성 예외를 확인합니다.  

다시 로드 메서드는 데이터베이스의 현재 값을 사용 하 여 엔터티의 현재 값을 덮어써서 데 사용할 수 있습니다. 엔터티는 다음 일반적으로 다시 모종의 형태로 사용자에 게 고 변경 사항을 다시 확인 하 고 다시 저장 하려고 해야 합니다. 예를 들어:  

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

동시성 예외를 시뮬레이션 하기 위해 SaveChanges 호출에 중단점을 설정 하 고 다음 SQL Management Studio와 같은 다른 도구를 사용 하 여 데이터베이스에 저장 되는 엔터티를 수정 하는 것이 좋습니다. 또한 SqlCommand를 사용 하 여 직접 데이터베이스를 업데이트 하는 SaveChanges 앞에 줄을 삽입할 수 있습니다. 예를 들어:  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

DbUpdateConcurrencyException 항목 메서드 업데이트에 실패 한 엔터티에 대 한 DbEntityEntry 인스턴스를 반환 합니다. (이 속성 현재은 항상 반환 동시성 문제에 대 한 단일 값입니다. 이 값을 반환할 수 여러 일반적인 업데이트 예외에 대 한 합니다.) 이들 각각에 대해 호출을 다시 로드 및 일부 상황에서는 대신 데이터베이스에서 다시 로드 해야 하는 모든 엔터티에 대 한 항목을 가져올 수 있습니다.  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a>클라이언트 우선으로 낙관적 동시성 예외를 확인합니다.  

다시 로드를 사용 하는 위의 예제에는 데이터베이스 wins 라고 합니다. 또는 엔터티 내의 값은 데이터베이스에서 값으로 덮어쓰기 때문에 저장소 우선 합니다. 경우에 따라 반대를 엔터티의 현재 값을 사용 하 여 데이터베이스의 값을 덮어쓸 수도 있습니다. 클라이언트 우선 라고 하 고 현재 데이터베이스 값을 가져오고 엔터티에 대 한 원래 값으로 설정 하 여 수행할 수 있습니다. (참조 [속성 값을 사용 하 여 작업](~/ef6/saving/change-tracking/property-values.md) 현재 값인 원래 값에 대 한 정보에 대 한 합니다.) 예를 들어:  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a>사용자 지정 해결 낙관적 동시성 예외  

경우에 따라 다음 엔터티의 현재 값을 사용 하 여 데이터베이스의 현재 값을 결합 하는 것이 좋습니다. 일반적으로 일부 사용자 지정 논리 또는 사용자 상호 작용이 필요합니다. 예를 들어, 현재 값을 데이터베이스에서 값을 포함 하는 사용자에 게 표시 수 있습니다 하 고 확인 되는 값의 기본 설정 합니다. 사용자는 필요에 따라 확인 된 값을 수정 합니다. 이러한 확인 되는 값을 데이터베이스에 저장 하는 것을 이렇게 하려면 엔터티의 항목 CurrentValues 및 GetDatabaseValues에서 반환 된 DbPropertyValues 개체를 사용 하 여 합니다. 예를 들어:  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a>사용자 지정 개체를 사용 하 여 낙관적 동시성 예외 확인  

위의 코드는 현재 전달, 데이터베이스 및 확인 되는 값에 대 한 DbPropertyValues 인스턴스를 사용합니다. 경우에 따라 쉽게이 엔터티 형식의 인스턴스를 사용할 수 있습니다. 이렇게 하려면 DbPropertyValues의 ToObject 및 SetValues 메서드를 사용 합니다. 예를 들어:  

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
