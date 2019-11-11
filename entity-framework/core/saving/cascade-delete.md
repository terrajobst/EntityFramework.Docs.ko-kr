---
title: 하위 삭제 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
uid: core/saving/cascade-delete
ms.openlocfilehash: 51c8b6f4517a3f87821ed1e4e2d60549e06ed39d
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656068"
---
# <a name="cascade-delete"></a>하위 삭제

하위 삭제는 데이터베이스 용어에서 행을 삭제하여 관련 행 삭제를 자동으로 트리거하도록 하는 특성을 설명하는 데 일반적으로 사용됩니다. EF Core에서 삭제 동작을 설명하는 밀접하게 관련된 개념으로 부모와의 관계가 끊어진 경우 자식 엔터티의 자동 삭제가 있습니다. 일반적으로 “고아 삭제”라고 합니다.

EF Core는 여러 다른 삭제 동작을 구현하며 개별 관계의 삭제 동작 구성을 허용합니다. 또한 EF Core는 [관계의 필수 여부](../modeling/relationships.md#required-and-optional-relationships)에 따라 각 관계에 대해 유용한 기본 삭제 동작을 자동으로 구성하는 규칙을 구현합니다.

## <a name="delete-behaviors"></a>삭제 동작

삭제 동작은 *DeleteBehavior* 열거자 유형에 정의되며 *OnDelete* 흐름 API에 전달하여 주/부모 엔터티의 삭제 또는 종속/자식 엔터티에 대한 관계 끊기가 종속/자식 엔터티에 부작용을 일으키는지 여부를 제어할 수 있습니다.

주/부모 엔터티가 삭제되거나 자식에 대한 관계가 끊어지는 경우 EF에서 다음 세 가지 작업을 수행할 수 있습니다.

* 자식/종속을 삭제할 수 있습니다.
* 자식의 외래 키 값을 null로 설정할 수 있습니다.
* 자식을 변경하지 않고 그대로 유지합니다.

> [!NOTE]  
> EF Core 모델에 구성된 삭제 동작은 주 엔터티가 EF Core를 사용하여 삭제되고 종속 엔터티가 메모리에 로드되는 경우(즉, 추적된 종속 항목의 경우)에만 적용됩니다. 컨텍스트에서 추적되지 않는 데이터에 필요한 작업이 적용되도록 데이터베이스에서 해당 하위 삭제 동작을 설정해야 합니다. EF Core를 사용하여 데이터베이스를 만드는 경우에는 이 하위 삭제 동작이 자동으로 설정됩니다.

위의 두 번째 작업에서 외래 키가 null 허용이 아닌 경우 외래 키 값을 null로 설정할 수 없습니다. null을 허용하지 않는 외래 키는 필수 관계와 동일합니다. 이러한 경우 EF Core는 SaveChanges가 호출될 때까지 외래 키 속성이 null로 표시되었음을 추적하며, 이때 변경 내용을 데이터베이스에 유지할 수 없기 때문에 예외가 throw됩니다. 이는 데이터베이스에서 제약 조건 위반을 가져오는 것과 유사합니다.

아래 표에 나열된 대로 네 가지 삭제 동작이 있습니다.

### <a name="optional-relationships"></a>선택적 관계

선택적 관계(null 허용 외래 키)인 경우 null 외래 키 값을 저장하여 다음과 같은 효과가 발생하도록 할 수 ‘있습니다’. 

| 동작 이름               | 메모리의 종속/자식에 대한 영향    | 데이터베이스의 종속/자식에 대한 영향  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| **Cascade**                 | 엔터티가 삭제됨                   | 엔터티가 삭제됨                   |
| **ClientSetNull**(기본값) | 외래 키 속성이 null로 설정됨 | 없음                                   |
| **SetNull**                 | 외래 키 속성이 null로 설정됨 | 외래 키 속성이 null로 설정됨 |
| **Restrict**                | 없음                                   | 없음                                   |

### <a name="required-relationships"></a>필수 관계

필수 관계(null 허용 외래 키)인 경우 null 외래 키 값을 저장하여 다음과 같은 효과가 발생하도록 할 수 ‘없습니다’. 

| 동작 이름         | 메모리의 종속/자식에 대한 영향 | 데이터베이스의 종속/자식에 대한 영향 |
|:----------------------|:------------------------------------|:--------------------------------------|
| **Cascade**(기본값) | 엔터티가 삭제됨                | 엔터티가 삭제됨                  |
| **ClientSetNull**     | SaveChanges가 throw됨                  | 없음                                  |
| **SetNull**           | SaveChanges가 throw됨                  | SaveChanges가 throw됨                    |
| **Restrict**          | 없음                                | 없음                                  |

위의 표에서 ‘없음’은 제약 조건 위반을 발생시킬 수 있습니다.  예를 들어 주/자식 엔터티가 삭제되었지만 종속/자식의 외래 키를 변경하는 작업을 수행하지 않으면 외래 제약 조건 위반으로 인해 데이터베이스에서 SaveChanges가 throw될 수 있습니다.

상위 수준에서 다음을 수행합니다.

* 부모가 있어야 엔터티가 존재할 수 있는 경우 EF에서 자동으로 하위를 삭제할 수 있도록 하려면 *Cascade*를 사용합니다.
  * 부모가 있어야 존재할 수 있는 엔터티는 일반적으로 *Cascade*가 기본값인 필수 관계를 사용합니다.
* 엔터티에 부모가 있거나 없을 수 있는 경우 EF에서 외래 키를 자동으로 무효화하도록 하려면 *ClientSetNull*을 사용합니다.
  * 부모가 없어야 존재할 수 있는 엔터티는 일반적으로 *ClientSetNull*이 기본값인 선택적 관계를 사용합니다.
  * 자식 엔터티가 로드되지 않은 경우에도 데이터베이스에서 null 값을 자식 외래 키로 전파하도록 하려면 *SetNull*을 사용합니다. 그러나 데이터베이스에서 이를 지원해야 하며, 이렇게 데이터베이스를 구성하면 다른 제한 사항이 나타날 수 있으므로 실제로는 이 옵션이 비현실적인 경우가 많습니다. 이로 인해 *SetNull*은 기본값이 아닙니다.
* EF Core에서 엔터티를 자동으로 삭제하거나 외래 키를 자동으로 무효화하도록 하지 않으려면 *Restrict*를 사용합니다. 이 경우 코드에서 자식 엔터티 및 해당 외래 키 값을 수동으로 동기화된 상태로 유지하도록 해야 합니다. 그러지 않으면 제약 조건 예외가 throw됩니다.

> [!NOTE]
> EF6와 달리 EF Core에서는 하위 삭제 효과가 즉시 발생하지 않고 SaveChanges가 호출될 때만 발생합니다.

> [!NOTE]  
> **EF Core 2.0의 변경 내용:** 이전 릴리스에서는 *Restrict*로 인해 추적된 종속 엔터티의 선택적 외래 키 속성이 null로 설정되었으며, 선택적 관계에 대한 기본 삭제 동작이었습니다. EF Core 2.0에서는 해당 동작을 나타내기 위해 *ClientSetNull*이 도입되었으며 선택적 관계의 기본값이 되었습니다. 종속 엔터티에 의도하지 않은 영향을 주지 않도록 *Restrict*의 동작이 조정되었습니다.

## <a name="entity-deletion-examples"></a>엔터티 삭제 예제

다음 코드는 다운로드하여 실행할 수 있는 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/)의 일부입니다. 이 샘플에서는 부모 엔터티가 삭제될 경우 선택적 관계와 필수 관계에 대한 각 삭제 동작에서 발생하는 상황을 보여줍니다.

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

각 변형을 살펴보고 어떤 상황이 발생하는지 파악해 보겠습니다.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>필수 또는 선택적 관계의 DeleteBehavior.Cascade

```console
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* 블로그가 삭제됨으로 표시됨
* SaveChanges까지 하위 삭제가 수행되지 않으므로 게시물이 처음에 변경되지 않은 상태로 유지됨
* SaveChanges에서 두 종속/자식(게시물)에 대해 삭제를 전송한 다음, 주/부모(블로그)에 대해 삭제를 전송함
* 저장 후에는 모든 엔터티가 데이터베이스에서 삭제되었으므로 분리됨

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>필수 관계의 DeleteBehavior.ClientSetNull 또는 DeleteBehavior.SetNull

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* 블로그가 삭제됨으로 표시됨
* SaveChanges까지 하위 삭제가 수행되지 않으므로 게시물이 처음에 변경되지 않은 상태로 유지됨
* SaveChanges에서 게시물 FK를 null로 설정하려고 시도하지만 FK가 null을 허용하지 않으므로 실패함

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>선택적 관계의 DeleteBehavior.ClientSetNull 또는 DeleteBehavior.SetNull

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* 블로그가 삭제됨으로 표시됨
* SaveChanges까지 하위 삭제가 수행되지 않으므로 게시물이 처음에 변경되지 않은 상태로 유지됨
* SaveChanges에서 주/부모(블로그)를 삭제하기 전에 두 종속/하위(게시물)의 FK를 null로 설정함
* 저장 후에는 주/부모(블로그)가 삭제되지만 종속/자식(게시물)은 계속 추적됨
* 추적된 종속/자식(게시물)은 null FK 값을 갖고 삭제된 주/부모(블로그)에 대한 참조는 제거됨

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>필수 또는 선택적 관계의 DeleteBehavior.Restrict

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* 블로그가 삭제됨으로 표시됨
* SaveChanges까지 하위 삭제가 수행되지 않으므로 게시물이 처음에 변경되지 않은 상태로 유지됨
* *Restrict*에서 FK를 null로 자동 설정하지 않도록 EF에 지시하기 때문에 null이 아닌 상태로 유지되며 저장하지 않고 SaveChanges가 throw됨

## <a name="delete-orphans-examples"></a>고아 삭제 예제

다음 코드는 다운로드하여 실행할 수 있는 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/CascadeDelete/)의 일부입니다. 이 샘플에서는 부모/주와 자식/종속 같 관계가 끊어진 경우 선택적 관계와 필수 관계에 대한 각 삭제 동작에서 발생하는 상황을 보여줍니다. 이 예제에서는 주/부모(블로그)의 컬렉션 탐색 속성에서 종속/자식(게시물)을 제거하여 관계를 끊습니다. 그러나 주/부모에 대한 종속/자식의 참조가 무효화되는 경우에는 동작이 동일합니다.

[!code-csharp[Main](../../../samples/core/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

각 변형을 살펴보고 어떤 상황이 발생하는지 파악해 보겠습니다.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>필수 또는 선택적 관계의 DeleteBehavior.Cascade

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '2' is in state Detached with FK '1' and no reference to a blog.
```

* 관계를 끊으면 FK가 null로 표시되기 때문에 게시물이 수정됨으로 표시됨
  * FK가 null을 허용하지 않으면 null로 표시되는 경우에도 실제 값은 변경되지 않음
* SaveChanges는 종속/자식(게시물)에 대한 삭제를 전송함
* 저장 후에는 종속/자식(게시물)이 데이터베이스에서 삭제되었으므로 분리됨

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>필수 관계의 DeleteBehavior.ClientSetNull 또는 DeleteBehavior.SetNull

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* 관계를 끊으면 FK가 null로 표시되기 때문에 게시물이 수정됨으로 표시됨
  * FK가 null을 허용하지 않으면 null로 표시되는 경우에도 실제 값은 변경되지 않음
* SaveChanges에서 게시물 FK를 null로 설정하려고 시도하지만 FK가 null을 허용하지 않으므로 실패함

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>선택적 관계의 DeleteBehavior.ClientSetNull 또는 DeleteBehavior.SetNull

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '2' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '2' is in state Unchanged with FK 'null' and no reference to a blog.
```

* 관계를 끊으면 FK가 null로 표시되기 때문에 게시물이 수정됨으로 표시됨
  * FK가 null을 허용하지 않으면 null로 표시되는 경우에도 실제 값은 변경되지 않음
* SaveChanges에서 두 종속/자식(게시물)의 FK를 null로 설정함
* 저장 후에는 종속/자식(게시물)이 null FK 값을 갖고 삭제된 주/부모(블로그)에 대한 참조가 제거됨

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>필수 또는 선택적 관계의 DeleteBehavior.Restrict

``` output
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '2' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '2' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* 관계를 끊으면 FK가 null로 표시되기 때문에 게시물이 수정됨으로 표시됨
  * FK가 null을 허용하지 않으면 null로 표시되는 경우에도 실제 값은 변경되지 않음
* *Restrict*에서 FK를 null로 자동 설정하지 않도록 EF에 지시하기 때문에 null이 아닌 상태로 유지되며 저장하지 않고 SaveChanges가 throw됨

## <a name="cascading-to-untracked-entities"></a>추적되지 않은 엔터티에 대한 하위 삭제

*SaveChanges*를 호출하면 하위 삭제 규칙이 컨텍스트에서 추적되는 모든 개체에 적용됩니다. 이 상황은 위에 표시된 모든 예제에서 발생하므로, 주/부모(블로그)와 모든 종속/자식(게시물)을 삭제하도록 SQL이 생성되었습니다.

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

`Include(b => b.Posts)`를 사용하지 않고 게시물도 포함하도록 블로그를 쿼리하는 경우처럼 주 엔터티만 로드되는 경우에는 SaveChanges에서 주/부모를 삭제하는 SQL만 생성됩니다.

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

데이터베이스에서 해당 하위 삭제 동작을 구성한 경우에만 종속/자식(게시물)이 삭제됩니다. EF를 사용하여 데이터베이스를 만드는 경우에는 이 하위 삭제 동작이 자동으로 설정됩니다.
