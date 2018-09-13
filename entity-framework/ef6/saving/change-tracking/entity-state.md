---
title: 엔터티 상태-EF6 사용
author: divega
ms.date: 10/23/2016
ms.assetid: acb27f46-3f3a-4179-874a-d6bea5d7120c
ms.openlocfilehash: c1dde7810d1dfa8a73e6bd2cf091b24be6b865d8
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490670"
---
# <a name="working-with-entity-states"></a><span data-ttu-id="f46f0-102">엔터티 상태를 사용 하 여 작업</span><span class="sxs-lookup"><span data-stu-id="f46f0-102">Working with entity states</span></span>
<span data-ttu-id="f46f0-103">추가 및 컨텍스트에 엔터티를 연결 하는 방법 및 Entity Framework SaveChanges 하는 동안 이러한을 처리 하는 방법을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-103">This topic will cover how to add and attach entities to a context and how Entity Framework processes these during SaveChanges.</span></span>
<span data-ttu-id="f46f0-104">Entity Framework에 상태의 엔터티가 컨텍스트에 연결 되어 있지만 연결이 끊긴 또는 N 계층 시나리오에서는 EF 엔터티 상태를 알 수 있어야 하는 추적을 처리 합니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-104">Entity Framework takes care of tracking the state of entities while they are connected to a context, but in disconnected or N-Tier scenarios you can let EF know what state your entities should be in.</span></span>
<span data-ttu-id="f46f0-105">이 토픽에서 설명하는 방법은 Code First 및 EF 디자이너를 사용하여 만든 모델에 동일하게 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="entity-states-and-savechanges"></a><span data-ttu-id="f46f0-106">엔터티 상태 및 SaveChanges</span><span class="sxs-lookup"><span data-stu-id="f46f0-106">Entity states and SaveChanges</span></span>

<span data-ttu-id="f46f0-107">엔터티의는 EntityState 열거에 의해 정의 된 대로 5 상태 중 하나일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-107">An entity can be in one of five states as defined by the EntityState enumeration.</span></span> <span data-ttu-id="f46f0-108">이러한 상태는:</span><span class="sxs-lookup"><span data-stu-id="f46f0-108">These states are:</span></span>  

- <span data-ttu-id="f46f0-109">추가: 엔터티가 컨텍스트에서 추적 되 고 있지만 아직 데이터베이스에 존재 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-109">Added: the entity is being tracked by the context but does not yet exist in the database</span></span>  
- <span data-ttu-id="f46f0-110">변경 되지 않습니다: 엔터티 컨텍스트에 의해 추적 되 고 데이터베이스에 존재 하 고 데이터베이스의 값에서 해당 속성 값이 변경 되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-110">Unchanged: the entity is being tracked by the context and exists in the database, and its property values have not changed from the values in the database</span></span>  
- <span data-ttu-id="f46f0-111">수정: 엔터티 컨텍스트에 의해 추적 되 고 데이터베이스에 존재 하 고 일부 또는 모든 속성 값이 수정 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-111">Modified: the entity is being tracked by the context and exists in the database, and some or all of its property values have been modified</span></span>  
- <span data-ttu-id="f46f0-112">삭제 됨: 엔터티 컨텍스트에 의해 추적 되 고 데이터베이스에 있지만 다음에 SaveChanges가 호출 될 데이터베이스에서 삭제 되도록 표시 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-112">Deleted: the entity is being tracked by the context and exists in the database, but has been marked for deletion from the database the next time SaveChanges is called</span></span>  
- <span data-ttu-id="f46f0-113">분리 된: 엔터티 추적 중이지 않은 컨텍스트에서</span><span class="sxs-lookup"><span data-stu-id="f46f0-113">Detached: the entity is not being tracked by the context</span></span>  

<span data-ttu-id="f46f0-114">SaveChanges 다양 한 상태의 엔터티에 대 한 여러 가지를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-114">SaveChanges does different things for entities in different states:</span></span>  

- <span data-ttu-id="f46f0-115">SaveChanges에서 변경 되지 않은 엔터티를 검사 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-115">Unchanged entities are not touched by SaveChanges.</span></span> <span data-ttu-id="f46f0-116">업데이트 변경 되지 않은 상태의 엔터티에 대 한 데이터베이스에 전송 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-116">Updates are not sent to the database for entities in the Unchanged state.</span></span>  
- <span data-ttu-id="f46f0-117">추가 엔터티를 데이터베이스에 삽입 되 고 후 될 때 변경 되지 않기 SaveChanges를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-117">Added entities are inserted into the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="f46f0-118">수정 된 엔터티의 데이터베이스에서 업데이트 되 고 후 될 때 변경 되지 않기 SaveChanges를 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-118">Modified entities are updated in the database and then become Unchanged when SaveChanges returns.</span></span>  
- <span data-ttu-id="f46f0-119">삭제 된 엔터티는 데이터베이스에서 삭제 되 고 컨텍스트에서 분리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-119">Deleted entities are deleted from the database and are then detached from the context.</span></span>  

<span data-ttu-id="f46f0-120">다음 예제에서는 엔터티 또는 엔터티 그래프의 상태를 변경할 수 있습니다 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-120">The following examples show ways in which the state of an entity or an entity graph can be changed.</span></span>  

## <a name="adding-a-new-entity-to-the-context"></a><span data-ttu-id="f46f0-121">새 엔터티를 컨텍스트에 추가</span><span class="sxs-lookup"><span data-stu-id="f46f0-121">Adding a new entity to the context</span></span>  

<span data-ttu-id="f46f0-122">DbSet에 Add 메서드를 호출 하 여 새 엔터티를 컨텍스트에 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-122">A new entity can be added to the context by calling the Add method on DbSet.</span></span>
<span data-ttu-id="f46f0-123">추가 엔터티를 Added 상태로 삽입 되도록 데이터베이스에 다음에 SaveChanges가 호출 될 때를 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-123">This puts the entity into the Added state, meaning that it will be inserted into the database the next time that SaveChanges is called.</span></span>
<span data-ttu-id="f46f0-124">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="f46f0-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Blogs.Add(blog);
    context.SaveChanges();
}
```  

<span data-ttu-id="f46f0-125">새 엔터티를 컨텍스트에 추가 하는 다른 방법은 Added로 상태를 변경할 것입니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-125">Another way to add a new entity to the context is to change its state to Added.</span></span> <span data-ttu-id="f46f0-126">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="f46f0-126">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = new Blog { Name = "ADO.NET Blog" };
    context.Entry(blog).State = EntityState.Added;
    context.SaveChanges();
}
```  

<span data-ttu-id="f46f0-127">마지막으로, 다른 엔터티를 이미 추적 중인 최대 연결 하 여 새 엔터티를 컨텍스트에 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-127">Finally, you can add a new entity to the context by hooking it up to another entity that is already being tracked.</span></span>
<span data-ttu-id="f46f0-128">다른 엔터티의 컬렉션 탐색 속성에 새 엔터티를 추가 하거나 새 엔터티를 가리키도록 다른 엔터티의 참조 탐색 속성을 설정 하 여 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-128">This could be by adding the new entity to the collection navigation property of another entity or by setting a reference navigation property of another entity to point to the new entity.</span></span> <span data-ttu-id="f46f0-129">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="f46f0-129">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    // Add a new User by setting a reference from a tracked Blog
    var blog = context.Blogs.Find(1);
    blog.Owner = new User { UserName = "johndoe1987" };

    // Add a new Post by adding to the collection of a tracked Blog
    var blog = context.Blogs.Find(2);
    blog.Posts.Add(new Post { Name = "How to Add Entities" });

    context.SaveChanges();
}
```  

<span data-ttu-id="f46f0-130">모든 엔터티에 추가 되지 않은 다른 엔터티에 대 한 참조 있으면 이러한 예제에 대 한 이러한 새 엔터티를 아직 추적 다음 컨텍스트에 추가 됩니다 않으며 다음에 SaveChanges가 호출 될 때 데이터베이스에 삽입 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-130">Note that for all of these examples if the entity being added has references to other entities that are not yet tracked then these new entities will also be added to the context and will be inserted into the database the next time that SaveChanges is called.</span></span>  

## <a name="attaching-an-existing-entity-to-the-context"></a><span data-ttu-id="f46f0-131">기존 엔터티를 컨텍스트에 연결</span><span class="sxs-lookup"><span data-stu-id="f46f0-131">Attaching an existing entity to the context</span></span>  

<span data-ttu-id="f46f0-132">있는 경우 이미 알고 있는 엔터티가 에서만 데이터베이스는 현재 추적 중이지 않은 컨텍스트에서 DbSet에서 Attach 메서드를 사용 하 여 엔터티를 추적 하는 컨텍스트를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-132">If you have an entity that you know already exists in the database but which is not currently being tracked by the context then you can tell the context to track the entity using the Attach method on DbSet.</span></span> <span data-ttu-id="f46f0-133">엔터티가 Unchanged 상태로 컨텍스트에 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-133">The entity will be in the Unchanged state in the context.</span></span> <span data-ttu-id="f46f0-134">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="f46f0-134">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="f46f0-135">내용이 적용 될 데이터베이스에 연결 된 엔터티를 조작 하는 다른 수행 하지 않고 SaveChanges가 호출 될 note 합니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-135">Note that no changes will be made to the database if SaveChanges is called without doing any other manipulation of the attached entity.</span></span> <span data-ttu-id="f46f0-136">엔터티가 Unchanged 상태로 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-136">This is because the entity is in the Unchanged state.</span></span>  

<span data-ttu-id="f46f0-137">기존 엔터티를 컨텍스트에 연결 하는 다른 방법은 Unchanged로 상태를 변경할 것입니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-137">Another way to attach an existing entity to the context is to change its state to Unchanged.</span></span> <span data-ttu-id="f46f0-138">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="f46f0-138">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="f46f0-139">두이 예제 모두에 대 한 연결 되 고 있는 엔터티에 아직 추적 되지 않는 다른 엔터티에 대 한 참조 하는 경우 이러한 새 엔터티는 또한 연결 Unchanged 상태로 컨텍스트에 참고 합니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-139">Note that for both of these examples if the entity being attached has references to other entities that are not yet tracked then these new entities will also attached to the context in the Unchanged state.</span></span>  

## <a name="attaching-an-existing-but-modified-entity-to-the-context"></a><span data-ttu-id="f46f0-140">기존 하지만 수정 된 엔터티를 컨텍스트에 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-140">Attaching an existing but modified entity to the context</span></span>  

<span data-ttu-id="f46f0-141">있는 경우 이미 알고 있는 엔터티의 있지만 데이터베이스에 변경 내용의 되었을 엔터티를 연결 하 고 해당 상태를 Modified로 설정 하는 컨텍스트를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-141">If you have an entity that you know already exists in the database but to which changes may have been made then you can tell the context to attach the entity and set its state to Modified.</span></span>
<span data-ttu-id="f46f0-142">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="f46f0-142">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Entry(existingBlog).State = EntityState.Modified;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="f46f0-143">수정 된 상태를 변경 하면 수정 된 엔터티의 모든 속성이 표시 됩니다 및 SaveChanges가 호출 될 때 모든 속성 값을이 데이터베이스로 전송 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-143">When you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  

<span data-ttu-id="f46f0-144">연결 중인 엔터티가 아직 추적 되지 않는 다른 엔터티에 대 한 참조에 이러한 새 엔터티가 Unchanged 상태로 컨텍스트에 연결 됩니다 참고-이러한는 자동으로 만들지 수정 합니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-144">Note that if the entity being attached has references to other entities that are not yet tracked, then these new entities will attached to the context in the Unchanged state—they will not automatically be made Modified.</span></span>
<span data-ttu-id="f46f0-145">수정 된 표시 해야 하는 여러 엔터티가 있는 경우 이러한 각 엔터티에 대 한 상태가 개별적으로 설정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-145">If you have multiple entities that need to be marked Modified you should set the state for each of these entities individually.</span></span>  

## <a name="changing-the-state-of-a-tracked-entity"></a><span data-ttu-id="f46f0-146">추적 된 엔터티의 상태를 변경합니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-146">Changing the state of a tracked entity</span></span>  

<span data-ttu-id="f46f0-147">해당 항목의 상태 속성을 설정 하 여 이미 추적 중인 엔터티의 상태를 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-147">You can change the state of an entity that is already being tracked by setting the State property on its entry.</span></span> <span data-ttu-id="f46f0-148">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="f46f0-148">For example:</span></span>  

``` csharp
var existingBlog = new Blog { BlogId = 1, Name = "ADO.NET Blog" };

using (var context = new BloggingContext())
{
    context.Blogs.Attach(existingBlog);
    context.Entry(existingBlog).State = EntityState.Unchanged;

    // Do some more work...  

    context.SaveChanges();
}
```  

<span data-ttu-id="f46f0-149">이미 추적 된 엔터티에 대 한 추가 또는 연결 호출 사용할 수도 있습니다 엔터티 상태를 변경 하려면 note 합니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-149">Note that calling Add or Attach for an entity that is already tracked can also be used to change the entity state.</span></span> <span data-ttu-id="f46f0-150">예를 들어, 현재 추가 상태에 있는 엔터티에 대 한 연결을 호출 Unchanged로 상태로 변경 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-150">For example, calling Attach for an entity that is currently in the Added state will change its state to Unchanged.</span></span>  

## <a name="insert-or-update-pattern"></a><span data-ttu-id="f46f0-151">삽입 또는 업데이트 패턴</span><span class="sxs-lookup"><span data-stu-id="f46f0-151">Insert or update pattern</span></span>  

<span data-ttu-id="f46f0-152">새 (따라서 데이터베이스 삽입)으로 엔터티를 추가 하거나 기존 엔터티를 연결 및 수정 (데이터베이스 업데이트의 결과)으로 표시 하려면 일부 응용 프로그램에 대 한 일반적인 패턴은 기본 키 값에 따라 합니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-152">A common pattern for some applications is to either Add an entity as new (resulting in a database insert) or Attach an entity as existing and mark it as modified (resulting in a database update) depending on the value of the primary key.</span></span>
<span data-ttu-id="f46f0-153">예를 들어, 데이터베이스에서 생성 된 정수 기본 키를 사용 하는 경우 새 0 키를 사용 하 여 엔터티 및 엔터티의 기존 0이 아닌 키를 사용 하 여 처리에 공통적으로 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-153">For example, when using database generated integer primary keys it is common to treat an entity with a zero key as new and an entity with a non-zero key as existing.</span></span>
<span data-ttu-id="f46f0-154">기본 키 값의 검사에 따라 엔터티 상태를 설정 하 여이 패턴을 구현할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-154">This pattern can be achieved by setting the entity state based on a check of the primary key value.</span></span> <span data-ttu-id="f46f0-155">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="f46f0-155">For example:</span></span>  

``` csharp
public void InsertOrUpdate(Blog blog)
{
    using (var context = new BloggingContext())
    {
        context.Entry(blog).State = blog.BlogId == 0 ?
                                   EntityState.Added :
                                   EntityState.Modified;

        context.SaveChanges();
    }
}
```  

<span data-ttu-id="f46f0-156">수정 된 상태를 변경 하면 수정 된 엔터티의 모든 속성이 표시 됩니다 및 SaveChanges가 호출 될 때 데이터베이스에 모든 속성 값을 보낼 수는 note 합니다.</span><span class="sxs-lookup"><span data-stu-id="f46f0-156">Note that when you change the state to Modified all the properties of the entity will be marked as modified and all the property values will be sent to the database when SaveChanges is called.</span></span>  
