---
title: "하위 삭제-EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: a9481fe851cc264ab3eaecad052c2e683ae57a44
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2017
---
# <a name="cascade-delete"></a>하위 삭제

모두 삭제 한 행을 관련 된 행의 삭제를 자동으로 트리거하도록의 삭제를 허용 하는 특징을 설명 하기 위해 데이터베이스 용어에서 주로 사용 됩니다. EF 코어 delete 동작도 적용 밀접 하 게 관련 된 개념은 하나의 자식 엔터티 관계 상위 되었을 때 자동으로 삭제할 어렵다는-일반적으로 "고아 파일 삭제" 이라고이 i입니다.

EF 코어는 몇 가지 다른 삭제 동작을 구현 하 고 개별 관계의 delete 동작의 구성을 허용 합니다. EF 코어 [requiredness 관계의](... /modeling/relationships.md#required-and-optional-relationships) 에 따라 각 관계에 대 한 유용한 기본값 삭제 동작을 자동으로 구성 하는 규칙을 구현 입니다.

## <a name="delete-behaviors"></a>동작 삭제
삭제 동작에 정의 된는 *DeleteBehavior* 열거자 입력 하 고에 전달 될 수는 *OnDelete* fluent API를 제어 하는지 여부를 사용자/부모 엔터티 또는의 비활성화할 삭제는 엔터티에도 종속/자식 관계에 종속/자식 엔터티 부작용 있어야 합니다.

세 가지 작업 EF는 사용자/부모 엔터티가 삭제 되거나 자식에 대 한 관계를 끊는 때 수행할 수 있습니다.
* 자식/종속 항목을 삭제할 수 있습니다.
* 어린이 외래 키 값을 설정할 수 있습니다 null로
* 자식이 변경 되지 않습니다.

> [!NOTE]  
> EF 핵심 모델에서 구성 된 delete 동작 EF 코어를 사용 하 여 주 엔터티가 삭제 되 고 종속 엔터티가 (즉, 추적 된 종속 파일)의 메모리에 로드 된 경우에 적용 됩니다. 해당 cascade 동작 컨텍스트에서 추적 중이지 않은 데이터를 확인 하기 위해 데이터베이스에서 설치 프로그램에 적용 되는 데 필요한 동작에 있어야 합니다. EF 코어를 사용 하 여 데이터베이스를 만드는 경우이 cascade 동작이 있습니다 설치 됩니다.

위의 두 번째 작업에 대 한 외래 키 값을 null로 설정지 않습니다 외래 키에서 null을 허용 하는 경우에 유효 합니다. (Nullable이 아닌 외래 키가 필수 관계 같음.) 이러한 경우 EF 코어 외래 키 속성이 될 때 변경 내용을 데이터베이스에 유지할 수 없는 때문에 예외가 throw 됩니다 SaveChanges를 호출할 때까지 null로 표시 되어 있습니다를 추적 합니다. 이것은 데이터베이스에서 제약 조건 위반을 얻는 비슷합니다.

4 개 delete 되어 동작은 아래 표에 나와 있습니다. 선택적 관계 (nullable 외래 키)에 대 한 것 _은_ 그 결과 다음과 같은 효과가 null 외래 키 값을 저장할 수 있습니다.

| 동작 이름 | 종속/자식 메모리에 대 한 영향 | 종속/자식 데이터베이스에 대 한 영향
|-|-|-
| **계단식 배열** | 엔터티 삭제 | 엔터티 삭제
| **ClientSetNull** (기본값) | 외래 키 속성은 null로 | 없음
| **SetNull** | 외래 키 속성은 null로 | 외래 키 속성은 null로
| **제한** | 없음 | 없음

필요한 관계 (nullable이 아닌 외래 키)에 대 한 _하지_ 그 결과 다음과 같은 효과가 null 외래 키 값을 저장할 수:

| 동작 이름 | 종속/자식 메모리에 대 한 영향 | 종속/자식 데이터베이스에 대 한 영향
|-|-|-
| **Cascade** (기본값) | 엔터티 삭제 | 엔터티 삭제
| **ClientSetNull** | SaveChanges를 발생 시킵니다. | 없음
| **SetNull** | SaveChanges를 발생 시킵니다. | SaveChanges를 발생 시킵니다.
| **제한** | 없음 | 없음

위의 표에 *None* 제약 조건 위반이 발생할 수 있습니다. 예를 들어 주/자식 엔터티 삭제 표시 되지만 아무 작업도 수행 종속/자식의 외래 키를 변경 하려면 다음 데이터베이스 가능성이를 throw 합니다 SaveChanges 외부 제약 조건 위반 합니다.

상위 수준:
* 부모 없이 존재할 수 없는 엔터티 있고 EF 자식을 자동으로 삭제 하기 위한 처리 하기를 원하는 다음 사용 하 여 *Cascade*합니다.
  * 부모 일반적으로 확인 없이 존재할 수 없는 엔터티 필요한 관계를 활용 *Cascade* 값이 기본값입니다.
* 수도 부모가 없을 수 있는 엔터티가 있고 EF, 외래 키 무효화 처리 하기를 원하는 다음 사용 하 여 *ClientSetNull*
  * 부모 일반적으로 확인 없이 존재할 수 있는 엔터티 선택적 관계를 사용 하 여 *ClientSetNull* 값이 기본값입니다.
  * 데이터베이스에도 자식 외래 키에 null 값을 전파 하려고 할 경우 자식 엔터티 로드 되지 않습니다, 사용 하 여 *SetNull*합니다. 그러나 note 데이터베이스는이 지원 해야 않으며 다른 제한이 발생할 수 있습니다 이와 같은 데이터베이스를 구성 하는 실제로 사용 하면이 옵션으로 합니다. 이 인해 *SetNull* 기본값이 아닙니다.
* EF 핵심을 적이 자동으로 엔터티를 삭제 하거나 자동으로 외래 키 null 다음 사용 하지 않으려면 *Restrict*합니다. 이 위해서는 코드 유지는 자식 엔터티 및 해당 외래 키 값 동기화 수동으로 참고 그렇지 않은 경우 제약 조건 예외가 throw 됩니다.

> [!NOTE]
> 달리 EF6, EF 코어의 영향이 발생 하지 않으므로 즉시 않고 대신 SaveChanges를 호출할 때에 합니다.

> [!NOTE]  
> **EF 코어 2.0의 변경 내용:** 이전 릴리스에서 *Restrict* 로 설정 해야 추적 된 종속 엔터티에 외래 키 속성 (옵션)로 인해 null 및 기본 선택적 관계에 대 한 동작을 삭제 했습니다. EF 코어 2.0에서는 *ClientSetNull* 선택적 관계에 대 한 기본값을 문제가 있고 해당 동작을 표시 하기 위해 도입 되었습니다. 동작은 *Restrict* 종속 엔터티가에 예기치 않은 결과가 사용 해야 조정 되었습니다.

## <a name="entity-deletion-examples"></a>엔터티 삭제 예

아래 코드의 일부인는 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) 하는 실행 다운로드할 수 있습니다. 어떤 일이 생기 선택 및 필수 관계에 대 한 각 삭제 동작에 대 한 부모 엔터티가 삭제 될 때 보여 줍니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

상황을 이해 하려면 각 변형을 살펴보겠습니다.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>필수 또는 선택적 관계와 DeleteBehavior.Cascade

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* 블로그 삭제로 표시
* 단계적 SaveChanges 될 때까지 발생 하지 않으므로 이후 처음 게시물 Unchanged 유지
* SaveChanges 보냅니다 삭제와에 대 한 종속 항목/하위 (게시) 다음 사용자/부모 (블로그)
* 저장 한 후 모든 엔터티는 이제 데이터베이스에서 삭제 한 후 분리

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull 또는 필수 관계와 DeleteBehavior.SetNull

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* 블로그 삭제로 표시
* 단계적 SaveChanges 될 때까지 발생 하지 않으므로 이후 처음 게시물 Unchanged 유지
* SaveChanges FK post null로 설정 하려고 시도 하지만 외래 키에서 null을 허용 하기 때문에이 작업이 실패 하면

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull 또는 선택적 관계와 DeleteBehavior.SetNull

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* 블로그 삭제로 표시
* 단계적 SaveChanges 될 때까지 발생 하지 않으므로 이후 처음 게시물 Unchanged 유지
* SaveChanges 시도 사용자/부모 (블로그)를 삭제 하기 전에 두 종속 항목/하위 (게시물) FK을 null로 설정
* 저장 한 후 사용자/부모 (블로그)는 삭제 되지만 종속 항목/하위 (게시)를 여전히 추적
* 추적 된 종속 항목/하위 (게시물) 이제 null 외래 키 값이 있는 않으며 삭제 된 사용자/부모 (블로그)에 대 한 참조의 삭제 되었습니다.

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>필수 또는 선택적 관계와 DeleteBehavior.Restrict

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* 블로그 삭제로 표시
* 단계적 SaveChanges 될 때까지 발생 하지 않으므로 이후 처음 게시물 Unchanged 유지
* 이후 *Restrict* 자동으로 null로 외래 키를 설정 하는 EF 지시 null이 아닌 및 저장 하지 않고 SaveChanges를 발생 시킵니다.

## <a name="delete-orphans-examples"></a>삭제 고아 예

아래 코드의 일부인는 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) 하는 실행 다운로드할 수 있습니다. 샘플 하는 경우 선택 사항이 고 필수 관계에 대 한 각 삭제 동작에 대 한 부모/보안 주체 및 해당 자식/종속 항목 간의 관계를 끊는 보여 줍니다. 이 예제에서는 사용자/부모 (블로그)의 컬렉션 탐색 속성에서 종속 항목/하위 (게시)를 제거 하 여 관계를 끊는 합니다. 그러나 동작은 동일한 사용자/부모에 종속/자식 참조 대신 않게 닫히면 될 경우입니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

상황을 이해 하려면 각 변형을 살펴보겠습니다.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>필수 또는 선택적 관계와 DeleteBehavior.Cascade

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* Null로 표시 되어야 FK 발생 관계 비활성화할 게시물 Modified로 표시 됩니다.
  * 외래 키에서 null을 허용 하는 경우 다음의 실제 값은 변경 되지 null로 표시 되어 있지만
* SaveChanges 종속 항목/자식 (게시)에 대 한 삭제를 보냅니다.
* 저장 한 후 종속 항목/하위 (게시물)는 이제 데이터베이스에서 삭제 한 후 분리

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull 또는 필수 관계와 DeleteBehavior.SetNull

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* Null로 표시 되어야 FK 발생 관계 비활성화할 게시물 Modified로 표시 됩니다.
  * 외래 키에서 null을 허용 하는 경우 다음의 실제 값은 변경 되지 null로 표시 되어 있지만
* SaveChanges FK post null로 설정 하려고 시도 하지만 외래 키에서 null을 허용 하기 때문에이 작업이 실패 하면

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull 또는 선택적 관계와 DeleteBehavior.SetNull

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* Null로 표시 되어야 FK 발생 관계 비활성화할 게시물 Modified로 표시 됩니다.
  * 외래 키에서 null을 허용 하는 경우 다음의 실제 값은 변경 되지 null로 표시 되어 있지만
* SaveChanges를 null로 두 종속 항목/하위 (게시물) FK 설정
* 저장 한 후 종속 항목/하위 (게시물) 이제 null 외래 키 값이 있는 및 삭제 된 사용자/부모 (블로그)에 대 한 참조는 제거 되었습니다.

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>필수 또는 선택적 관계와 DeleteBehavior.Restrict

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* Null로 표시 되어야 FK 발생 관계 비활성화할 게시물 Modified로 표시 됩니다.
  * 외래 키에서 null을 허용 하는 경우 다음의 실제 값은 변경 되지 null로 표시 되어 있지만
* 이후 *Restrict* 자동으로 null로 외래 키를 설정 하는 EF 지시 null이 아닌 및 저장 하지 않고 SaveChanges를 발생 시킵니다.

## <a name="cascading-to-untracked-entities"></a>추적 되지 않은 엔터티에도 종속

호출 하는 경우 *SaveChanges*, cascade 규칙 삭제는 컨텍스트에 의해 추적 되는 엔터티에 적용 됩니다. 이러한 모든 상황은 위에 표시 된 예에서는 이기 때문에 SQL 사용자/부모 (블로그)와 모든 종속 항목/하위 (게시)을 모두 삭제 하도록 생성:

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

경우에에서 주 서버를 로드-예를 들어 때 쿼리가 만들어지는 없이 블로그에 대 한 프로그램 `Include(b => b.Posts)` 게시물-포함 하도록 한 다음 SaveChanges 생성 되 고 주/부모를 삭제 하는 SQL:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

종속 항목/하위 (게시물) 데이터베이스에 해당 cascade 동작을 구성 하는 경우에 삭제 됩니다. EF를 사용 하 여 데이터베이스를 만드는 경우이 cascade 동작이 있습니다 설치 됩니다.
