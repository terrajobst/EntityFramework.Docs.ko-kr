---
title: 값 비교자-EF Core
description: 값 비교자를 사용 하 EF Core 속성 값을 비교 하는 방법 제어
author: ajcvickers
ms.date: 03/20/2020
uid: core/modeling/value-comparers
ms.openlocfilehash: 9dfed7b7ef8163f4f5c94a0c81c510807c53c13d
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80148258"
---
# <a name="value-comparers"></a>값 비교자

> [!NOTE]  
> 이 기능은 EF Core 3.0의 새로운 기능입니다.

> [!TIP]  
> 이 문서의 코드는 GitHub에서 실행 가능한 [샘플](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/ValueConversions/)로 찾을 수 있습니다.

## <a name="background"></a>배경

EF Core는 다음과 같은 경우 속성 값을 비교 해야 합니다.

* [업데이트 변경 내용을 검색](xref:core/saving/basic) 하는 과정에서 속성이 변경 되었는지 여부 확인
* 관계를 확인할 때 두 키 값이 같은지 확인 

이는 int, bool, DateTime 등의 일반적인 기본 형식에 대해 자동으로 처리 됩니다.

더 복잡 한 형식의 경우 비교를 수행 하는 방법에 대 한 선택 항목이 필요 합니다.
예를 들어 바이트 배열을 비교할 수 있습니다.

* 새 바이트 배열을 사용 하는 경우에만 차이가 검색 되도록 참조
* 배열에서 바이트의 변형을 검색 하는 심층 비교

기본적으로 EF Core는 키가 아닌 바이트 배열에 대해 이러한 방법 중 첫 번째 방법을 사용 합니다.
즉, 참조만 비교 되 고 기존 바이트 배열을 새 배열로 바꿀 때만 변경 내용이 검색 됩니다.
이는 SaveChanges를 실행할 때 많은 수의 많은 바이트 배열에 대 한 심층 비교를 방지 하는 실용적인 결정입니다.
그러나 다른 이미지로 이미지를 대체 하는 일반적인 시나리오는 성능이 뛰어난 방식으로 처리 됩니다.

반면에 바이트 배열을 사용 하 여 이진 키를 나타내는 경우 참조 일치가 작동 하지 않습니다.
FK 속성을 비교 해야 하는 PK 속성과 _동일한 인스턴스로_ 설정할 가능성이 매우 낮습니다.
따라서 EF Core는 키로 작동 하는 바이트 배열에 대 한 심층 비교를 사용 합니다.
일반적으로 이진 키가 짧기 때문에 성능이 크게 저하 될 가능성은 거의 없습니다.

### <a name="snapshots"></a>스냅샷

변경 가능 형식에 대 한 심층 비교는 EF Core 속성 값의 심층 "스냅숏"을 만드는 기능이 필요 함을 의미 합니다.
대신 참조를 복사 하면 _동일한 개체_이기 때문에 현재 값과 스냅숏이 모두 변경 됩니다.
따라서 변경 가능한 형식에 대 한 심층 비교가 사용 되는 경우 심층 스냅숏 필요 합니다.

## <a name="properties-with-value-converters"></a>값 변환기가 있는 속성

위의 경우 EF Core는 바이트 배열에 대 한 기본 매핑 지원을 포함 하므로 적절 한 기본값을 자동으로 선택할 수 있습니다.
그러나 속성이 [값 변환기](xref:core/modeling/value-conversions)를 통해 매핑되면 EF Core는 사용할 적절 한 비교를 항상 결정할 수는 없습니다.
대신 EF Core는 항상 속성의 형식으로 정의 된 기본 같음 비교를 사용 합니다.
이는 종종 올바르지만 더 복잡 한 형식을 매핑할 때 재정의 해야 할 수 있습니다.

### <a name="simple-immutable-classes"></a>단순한 변경할 수 없는 클래스

에서 값 변환기를 사용 하 여 단순 하 고 변경할 수 없는 클래스를 매핑하는 속성을 고려 합니다.

[!code-csharp[SimpleImmutableClass](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=SimpleImmutableClass)]

[!code-csharp[ConfigureImmutableClassProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=ConfigureImmutableClassProperty)]

이 형식의 속성은 다음과 같은 경우 특별 한 비교 또는 스냅숏이 필요 하지 않습니다.
* 서로 다른 인스턴스가 올바르게 비교 되도록 같음을 재정의 합니다.
* 형식은 변경할 수 없으므로 스냅숏 값을 변경할 가능성이 없습니다.

따라서이 경우 EF Core의 기본 동작은 그대로 있습니다.

### <a name="simple-immutable-structs"></a>단순 변경할 수 없는 구조체

단순 구조체에 대 한 매핑도 단순 하 고 특수 한 비교자 또는 스냅숏 필요 하지 않습니다.

[!code-csharp[SimpleImmutableStruct](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=SimpleImmutableStruct)]

[!code-csharp[ConfigureImmutableStructProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=ConfigureImmutableStructProperty)]

EF Core는 구조체 속성의 컴파일된 멤버 수준 비교를 생성 하는 기본 제공 지원 기능을 제공 합니다.
즉, 구조체에는 EF에 대 한 동등 재정의가 필요 하지 않지만 [다른 이유로](/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type)이 작업을 수행 하도록 선택할 수 있습니다.
또한 구조체를 변경할 수 없고 항상 멤버 수준 복사 되므로 특수 스냅숏 필요 하지 않습니다.
변경 가능한 구조체의 경우에도 마찬가지 이지만 [일반적으로 변경 가능한 구조체를 피해 야](/dotnet/csharp/write-safe-efficient-code)합니다.

### <a name="mutable-classes"></a>변경 가능한 클래스

가능 하면 값 변환기를 사용 하 여 변경할 수 없는 형식 (클래스 또는 구조체)을 사용 하는 것이 좋습니다.
이는 일반적으로 보다 효율적 이며, 변경 가능한 형식을 사용 하는 것 보다 클리너 의미를 갖습니다.

그러나 일반적으로 응용 프로그램에서 변경할 수 없는 형식의 속성을 사용 하는 것이 일반적입니다.
예를 들어, 숫자 목록을 포함 하는 속성을 매핑합니다. 

[!code-csharp[ListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ListProperty)]

[`List<T>` 클래스](/dotnet/api/system.collections.generic.list-1?view=netstandard-2.1):
* 참조가 같은지 여부 동일한 값을 포함 하는 두 목록이 다른 것으로 처리 됩니다.
* 변경 가능 목록에 있는 값을 추가 하 고 제거할 수 있습니다.

List 속성에 대 한 일반적인 값 변환은 다음과 같이 목록을 JSON으로 변환할 수 있습니다.

[!code-csharp[ConfigureListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListProperty)]

그러면 속성에 대 한 `ValueComparer<T>`를 설정 하 여이 변환과의 올바른 비교 EF Core 사용 해야 합니다.

[!code-csharp[ConfigureListPropertyComparer](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListPropertyComparer)]

> [!NOTE]  
> Value 비교자를 설정 하는 모델 작성기 ("흐름") API가 아직 구현 되지 않았습니다.
> 대신 위의 코드는 작성기에서 ' Metadata '로 노출 하는 하위 수준의 IMutableProperty에서 SetValueComparer를 호출 합니다.

`ValueComparer<T>` 생성자는 세 가지 식을 허용 합니다.
* 품질을 확인 하기 위한 식입니다.
* 해시 코드를 생성 하는 식입니다.
* 값을 스냅숏에 대 한 식입니다.  

이 경우 숫자 시퀀스가 동일한 지 확인 하 여 비교를 수행 합니다.

마찬가지로 해시 코드도이 동일한 시퀀스에서 빌드됩니다.
이는 변경 가능한 값에 대 한 해시 코드 이므로 [문제를 일으킬](https://ericlippert.com/2011/02/28/guidelines-and-rules-for-gethashcode/)수 있습니다.
가능 하면 변경할 수 없습니다.

이 스냅숏은 ToList로 목록을 복제 하 여 만듭니다.
다시 한 번이이는 목록을 변경할 경우에만 필요 합니다.
가능 하면 변경할 수 없습니다. 

> [!NOTE]  
> 값 변환기와 비교자는 간단한 대리자가 아닌 식을 사용 하 여 생성 됩니다.
> EF는 이러한 식을 훨씬 더 복잡 한 식 트리에 삽입 한 다음,이를 entity shaper 대리자로 컴파일합니다.
> 개념적으로이는 컴파일러 인라이닝과 비슷합니다.
> 예를 들어, 변환을 수행 하기 위해 다른 메서드를 호출 하는 대신 간단한 변환이 캐스트에서 컴파일될 수 있습니다.    

### <a name="key-comparers"></a>키 비교자

배경 섹션에서는 키 비교가 특수 한 의미 체계를 필요로 하는 이유에 대해 설명 합니다.
기본, 주 또는 외래 키 속성에 설정 하는 경우 키에 적합 한 비교자를 만들어야 합니다.

같은 속성에 다른 의미 체계가 필요한 드문 경우에 [SetKeyValueComparer](/dotnet/api/microsoft.entityframeworkcore.mutablepropertyextensions.setkeyvaluecomparer?view=efcore-3.1) 를 사용 합니다.

> [!NOTE]  
> SetStructuralComparer는 EF Core 5.0에서 오래 되었습니다.
> 대신 SetKeyValueComparer를 사용 해야 합니다.

### <a name="overriding-defaults"></a>기본값 재정의

경우에 따라 EF Core에서 사용 하는 기본 비교가 적절 하지 않을 수 있습니다.
예를 들어 바이트 배열의 크기는 기본적으로 EF Core에서 검색 되지 않습니다.
속성에 다른 비교자를 설정 하 여이를 재정의할 수 있습니다. 

[!code-csharp[OverrideComparer](../../../samples/core/Modeling/ValueConversions/OverridingByteArrayComparisons.cs?name=OverrideComparer)]

EF Core는 이제 바이트 시퀀스를 비교 하 여 바이트 배열 변경이을 검색 합니다.
