---
title: Code First 데이터 주석-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 9fac2a90c46d78ff5fd632800cc0050276467773
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415858"
---
# <a name="code-first-data-annotations"></a>Code First 데이터 주석
> [!NOTE]
> **Ef 4.1** 이상-이 페이지에서 설명 하는 기능, api 등은 Entity Framework 4.1에서 도입 되었습니다. 이전 버전을 사용 하는 경우이 정보 중 일부 또는 전체가 적용 되지 않습니다.

이 페이지의 내용은 원래 Julie Lerman (\<http://thedatafarm.com>)에서 작성 한 문서에서 적용 됩니다.

Entity Framework Code First를 사용 하면 EF가 쿼리, 변경 내용 추적 및 업데이트 기능을 수행 하기 위해 사용 하는 모델을 나타내는 데 고유한 도메인 클래스를 사용할 수 있습니다. Code First은 ' 구성에 대 한 규칙 ' 이라고 하는 프로그래밍 패턴을 활용 합니다. Code First는 클래스가 Entity Framework 규칙을 따르는 것으로 가정 하며,이 경우에서 해당 작업을 수행 하는 방법을 자동으로 해결 합니다. 그러나 클래스가 이러한 규칙을 따르지 않는 경우 클래스에 구성을 추가 하 여 필수 정보를 사용 하 여 EF를 제공할 수 있습니다.

Code First은 이러한 구성을 클래스에 추가 하는 두 가지 방법을 제공 합니다. 하나는 DataAnnotations 이라는 간단한 특성을 사용 하 고 두 번째는 Code First의 흐름 API를 사용 하는 것입니다 .이 API는 코드에서 명령적으로 구성을 설명 하는 방법을 제공 합니다.

이 문서에서는 System.componentmodel 네임 스페이스에서 DataAnnotations을 사용 하 여 클래스를 구성 하는 방법을 집중적으로 설명 합니다. 가장 일반적으로 필요한 구성을 강조 표시 합니다. 이러한 응용 프로그램이 클라이언트 쪽 유효성 검사에 동일한 주석을 활용할 수 있도록 하는 ASP.NET MVC와 같은 여러 .NET 응용 프로그램 에서도 DataAnnotations을 인식 합니다.


## <a name="the-model"></a>모델

간단한 클래스 쌍으로 블로그 및 게시물을 사용 하 여 Code First DataAnnotations을 보여 드리겠습니다.

``` csharp
    public class Blog
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }

    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public DateTime DateCreated { get; set; }
        public string Content { get; set; }
        public int BlogId { get; set; }
        public ICollection<Comment> Comments { get; set; }
    }
```

이와 같이 블로그 및 게시물 클래스는 코드 첫 번째 규칙을 편리 하 게 따르고 EF 호환성을 사용 하도록 조정 하지 않아도 됩니다. 그러나 주석을 사용 하 여 클래스와 해당 클래스가 매핑되는 데이터베이스에 대 한 추가 정보를 EF에 제공할 수도 있습니다.

 

## <a name="key"></a>Key

Entity Framework는 엔터티 추적에 사용 되는 키 값이 있는 모든 엔터티에 사용 됩니다. Code First의 한 가지 규칙은 암시적 키 속성입니다. Code First는 "Id" 라는 속성 또는 클래스 이름 및 "Id"의 조합 (예: "BlogId")을 찾습니다. 이 속성은 데이터베이스의 기본 키 열에 매핑됩니다.

블로그 및 게시물 클래스는 모두이 규칙을 따릅니다. 그렇지 않으면 어떻게 되나요? 블로그에서 이름이 *Primarytrackingkey* 대신 또는 *foo*를 사용 하는 경우 Code first가이 규칙과 일치 하는 속성을 찾지 못하면 키 속성이 있어야 Entity Framework 요구 사항으로 인해 예외가 throw 됩니다. 키 주석을 사용 하 여 EntityKey로 사용할 속성을 지정할 수 있습니다.

``` csharp
    public class Blog
    {
        [Key]
        public int PrimaryTrackingKey { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }
```

Code first의 데이터베이스 생성 기능을 사용 하는 경우 블로그 테이블에는 기본적으로 Id로 정의 되는 PrimaryTrackingKey 라는 기본 키 열이 있습니다.

![기본 키가 포함 된 블로그 테이블](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a>복합 키

Entity Framework는 둘 이상의 속성으로 구성 된 복합 키-기본 키를 지원 합니다. 예를 들어 기본 키가 PassportNumber 및 IssuingCountry의 조합 인 Passport 클래스가 있을 수 있습니다.

``` csharp
    public class Passport
    {
        [Key]
        public int PassportNumber { get; set; }
        [Key]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

EF 모델에서 위의 클래스를 사용 하려고 하면 `InvalidOperationException`발생 합니다.

*' Passport ' 형식의 복합 기본 키 순서를 확인할 수 없습니다. ColumnAttribute 또는 HasKey 메서드를 사용 하 여 복합 기본 키의 순서를 지정 합니다.*

복합 키를 사용 하려면 Entity Framework 키 속성의 순서를 정의 해야 합니다. 열 주석을 사용 하 여 순서를 지정할 수 있습니다.

>[!NOTE]
> 순서 값은 인덱스 기반이 아니라 상대적 이므로 값을 사용할 수 있습니다. 예를 들어 100 및 200은 1과 2 대신 사용할 수 있습니다.

``` csharp
    public class Passport
    {
        [Key]
        [Column(Order=1)]
        public int PassportNumber { get; set; }
        [Key]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

복합 외래 키가 있는 엔터티가 있는 경우 해당 하는 기본 키 속성에 사용한 것과 동일한 열 순서 지정을 지정 해야 합니다.

외래 키 속성 내의 상대 정렬만 동일 해야 합니다. **Order** 에 할당 된 정확한 값은 일치할 필요가 없습니다. 예를 들어 다음 클래스에서 3과 4는 1과 2 대신 사용할 수 있습니다.

``` csharp
    public class PassportStamp
    {
        [Key]
        public int StampId { get; set; }
        public DateTime Stamped { get; set; }
        public string StampingCountry { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 1)]
        public int PassportNumber { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }

        public Passport Passport { get; set; }
    }
```

## <a name="required"></a>필수

필요한 주석은 EF에 특정 속성이 필요 함을 나타냅니다.

Required 속성에 Required를 추가 하면 EF (및 MVC)가 속성에 데이터를 포함 하는지 확인 합니다.

``` csharp
    [Required]
    public string Title { get; set; }
```

응용 프로그램에서 코드 또는 태그를 추가로 변경 하지 않으면 MVC 응용 프로그램은 클라이언트 쪽 유효성 검사를 수행 하 고 속성 및 주석 이름을 사용 하 여 동적으로 메시지를 작성 하는 경우에도 마찬가지입니다.

![제목이 있는 만들기 페이지가 필요 합니다. 오류](~/ef6/media/jj591583-figure02.png)

필수 특성은 매핑된 속성이 null을 허용 하지 않도록 설정 하 여 생성 된 데이터베이스에도 영향을 줍니다. 제목 필드가 "not null"로 변경 되었습니다.

>[!NOTE]
> 속성이 필요한 경우에도 데이터베이스의 열을 null을 허용 하지 않는 경우도 있습니다. 예를 들어 여러 형식에 대해 TPH 상속 전략 데이터를 사용 하는 경우 단일 테이블에 저장 됩니다. 파생 된 형식에 필수 속성이 포함 된 경우에는 해당 열을 null을 허용 하지 않는 형식으로 만들 수 없습니다.

 

![블로그 테이블](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a>MaxLength 및 MinLength

MaxLength 및 MinLength 특성을 사용 하면 필요한 경우와 마찬가지로 추가 속성 유효성 검사를 지정할 수 있습니다.

다음은 길이 요구 사항이 있는 BloggerName입니다. 또한이 예제에서는 특성을 결합 하는 방법을 보여 줍니다.

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

MaxLength 주석은 속성의 길이를 10으로 설정 하 여 데이터베이스에 영향을 줍니다.

![BloggerName 열의 최대 길이를 표시 하는 블로그 테이블](~/ef6/media/jj591583-figure04.png)

MVC 클라이언트 쪽 주석 및 EF 4.1 서버 쪽 주석은이 유효성 검사를 모두 수행 하 고, 다시 동적으로 오류 메시지를 작성 합니다. "BloggerName 필드는 최대 길이가 ' 10 ' 인 문자열 또는 배열 형식 이어야 합니다." 이 메시지는 약간 깁니다. 많은 주석을 사용 하면 ErrorMessage 특성으로 오류 메시지를 지정할 수 있습니다.

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

필요한 주석에 ErrorMessage를 지정할 수도 있습니다.

![사용자 지정 오류 메시지를 사용 하 여 페이지 만들기](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a>NotMapped

Code first 규칙은 지원 되는 데이터 형식에 해당 하는 모든 속성이 데이터베이스에 표시 되도록 규정 합니다. 그러나 응용 프로그램에서 항상 그렇지는 않습니다. 예를 들어 Title 및 BloggerName 필드를 기반으로 코드를 만드는 블로그 클래스의 속성이 있을 수 있습니다. 이 속성은 동적으로 만들 수 있으며 저장할 필요가 없습니다. 이 BlogCode 속성과 같이 NotMapped 않은 주석을 사용 하 여 데이터베이스에 매핑되지 않는 모든 속성을 표시할 수 있습니다.

``` csharp
    [NotMapped]
    public string BlogCode
    {
        get
        {
            return Title.Substring(0, 1) + ":" + BloggerName.Substring(0, 1);
        }
    }
```

 

## <a name="complextype"></a>ComplexType

클래스 집합에서 도메인 엔터티를 설명한 다음 해당 클래스를 계층화 하 여 전체 엔터티를 설명 하는 것은 드문 일이 아닙니다. 예를 들어 BlogDetails 라는 클래스를 모델에 추가할 수 있습니다.

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

BlogDetails에는 어떤 유형의 키 속성도 없습니다. 도메인 기반 디자인에서 BlogDetails를 값 개체 라고 합니다. Entity Framework은 값 개체를 복합 형식으로 참조 합니다.  복합 형식은 자체적으로 추적할 수 없습니다.

그러나 블로그 클래스의 속성으로 BlogDetails는 블로그 개체의 일부로 추적 됩니다. Code first에서이를 인식할 수 있도록 하려면 BlogDetails 클래스를 ComplexType로 표시 해야 합니다.

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

이제 블로그 클래스에서 속성을 추가 하 여 해당 블로그의 BlogDetails를 나타낼 수 있습니다.

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

데이터베이스에서 블로그 표에는 BlogDetail 속성에 포함 된 속성을 포함 하 여 블로그의 모든 속성이 포함 됩니다. 기본적으로 각 항목은 복합 유형인 BlogDetail의 이름 앞에와 야 합니다.

![복합 형식의 블로그 테이블](~/ef6/media/jj591583-figure06.png)


## <a name="concurrencycheck"></a>ConcurrencyCheck

ConcurrencyCheck 주석을 사용 하면 사용자가 엔터티를 편집 하거나 삭제할 때 데이터베이스에서 동시성 검사에 사용할 하나 이상의 속성에 플래그를 지정할 수 있습니다. EF Designer를 사용 하 여 작업 한 경우 속성의 ConcurrencyMode을 Fixed로 설정 하 여 정렬 합니다.

ConcurrencyCheck을 BloggerName 속성에 추가 하 여 어떻게 작동 하는지 살펴보겠습니다.

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

SaveChanges가 호출 될 때 BloggerName 필드의 ConcurrencyCheck 주석으로 인해 해당 속성의 원래 값이 업데이트에 사용 됩니다. 이 명령은 키 값 뿐만 아니라 BloggerName의 원래 값에 대해서만 필터링 하 여 올바른 행을 찾으려고 시도 합니다.  다음은 데이터베이스에 전송 되는 UPDATE 명령의 중요 한 부분입니다. 여기서 명령은 PrimaryTrackingKey가 1이 고 BloggerName가 데이터베이스에서 검색 되었을 때 원래 값인 "Julie" 인 행을 업데이트 합니다.

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

다른 사람이 해당 블로그의 블로거 이름을 변경한 경우이 업데이트는 실패 하 고 처리 해야 하는 DbUpdateConcurrencyException을 받게 됩니다.

 

## <a name="timestamp"></a>TimeStamp

동시성 검사를 위해 rowversion 또는 timestamp 필드를 사용 하는 것이 더 일반적입니다. 그러나 ConcurrencyCheck 주석을 사용 하는 대신 속성 형식이 바이트 배열인 경우 더 구체적인 타임 스탬프 주석을 사용할 수 있습니다. Code first는 타임 스탬프 속성을 ConcurrencyCheck 속성과 동일 하 게 처리 하지만 code first에서 생성 하는 데이터베이스 필드가 null을 허용 하지 않는지 확인 합니다. 지정 된 클래스에는 타임 스탬프 속성을 하나만 사용할 수 있습니다.

블로그 클래스에 다음 속성을 추가 합니다.

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

code first에서 데이터베이스 테이블에 null을 허용 하지 않는 timestamp 열을 만듭니다.

![타임 스탬프 열이 있는 블로그 테이블](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a>테이블 및 열

데이터베이스 Code First 만들 수 있도록 하려면 만들고 있는 테이블 및 열의 이름을 변경 하는 것이 좋습니다. 기존 데이터베이스에서 Code First를 사용할 수도 있습니다. 그러나 도메인의 클래스 및 속성 이름이 데이터베이스의 테이블 및 열 이름과 일치 하지 않는 경우도 있습니다.

내 클래스의 이름은 블로그 이며 규칙에 따라 먼저 code first로 가정 하 고이는 블로그 라는 테이블에 매핑됩니다. 그렇지 않은 경우 테이블 특성이 있는 테이블의 이름을 지정할 수 있습니다. 예를 들어 주석은 테이블 이름이 InternalBlogs 임을 지정 합니다.

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

열 주석은 매핑된 열의 특성을 지정 하는 것 보다 더 많은 어 뎁 트입니다. 테이블에서 이름, 데이터 형식 또는 열이 표시 되는 순서를 규정 수 있습니다. 열 특성의 예는 다음과 같습니다.

``` csharp
    [Column("BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

열의 TypeName 특성과 DataType DataAnnotation을 혼동 하지 마세요. DataType은 UI에 사용 되는 주석이 며 Code First에서 무시 됩니다.

다시 생성 된 후의 테이블은 다음과 같습니다. 테이블 이름이 InternalBlogs로 변경 되었으며 복합 형식의 설명 열은 이제 BlogDescription입니다. 주석에 이름이 지정 되었으므로 code first는 복합 형식의 이름으로 열 이름을 시작 하는 규칙을 사용 하지 않습니다.

![블로그 테이블 및 열 이름 바꾸기](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a>DatabaseGenerated

중요 한 데이터베이스 기능은 계산 된 속성을 가질 수 있는 기능입니다. Code First 클래스를 계산 열이 포함 된 테이블에 매핑하는 경우에는 Entity Framework 해당 열을 업데이트 하려고 하지 않습니다. 그러나 데이터를 삽입 하거나 업데이트 한 후 EF가 데이터베이스에서 해당 값을 반환 하도록 하려고 합니다. DatabaseGenerated 주석을 사용 하 여 계산 된 열거형과 함께 클래스의 해당 속성에 플래그를 지정할 수 있습니다. 다른 열거형은 None 및 Identity입니다.

``` csharp
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime DateCreated { get; set; }
```

Code first가 데이터베이스를 생성할 때 byte 또는 timestamp 열에 생성 된 데이터베이스를 사용할 수 있습니다. 그렇지 않으면 code first가 계산 열에 대 한 수식을 확인할 수 없기 때문에 기존 데이터베이스를 가리킬 때만이를 사용 해야 합니다.

위에서 읽은 것은 기본적으로 정수 인 키 속성은 데이터베이스의 id 키가 됩니다. 이는 생성 된 DatabaseGenerated DatabaseGeneratedOption로 설정 하는 것과 같습니다. Id 키가 되도록 하려면 값을 DatabaseGeneratedOption로 설정 하면 됩니다.

 

## <a name="index"></a>Index

> [!NOTE]
> **Ef 6.1** 이상-인덱스 특성이 Entity Framework 6.1에서 도입 되었습니다. 이전 버전을 사용 하는 경우이 섹션의 정보는 적용 되지 않습니다.

**Indexattribute**를 사용 하 여 하나 이상의 열에 인덱스를 만들 수 있습니다. 하나 이상의 속성에 특성을 추가 하면 EF가 데이터베이스를 만들 때 데이터베이스에 해당 인덱스를 만들거나 Code First 마이그레이션를 사용 하는 경우 해당 **createindex** 호출을 스 캐 폴드 수 있습니다.

예를 들어 다음 코드는 데이터베이스에 있는 **게시물** 테이블의 **등급** 열에 인덱스를 만듭니다.

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index]
        public int Rating { get; set; }
        public int BlogId { get; set; }
    }
```

기본적으로 인덱스는 **ix\_&lt;속성 이름&gt;** (위 예제의 Ix\_등급)으로 지정 됩니다. 하지만 인덱스의 이름을 지정할 수도 있습니다. 다음 예에서는 인덱스 이름을 **PostRatingIndex**로 지정 합니다.

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

기본적으로 인덱스는 고유 하지 않지만 **IsUnique** 명명 된 매개 변수를 사용 하 여 인덱스를 고유 하 게 지정할 수 있습니다. 다음 예에서는 **사용자**의 로그인 이름에 대 한 고유 인덱스를 소개 합니다.

``` csharp
    public class User
    {
        public int UserId { get; set; }

        [Index(IsUnique = true)]
        [StringLength(200)]
        public string Username { get; set; }

        public string DisplayName { get; set; }
    }
```

### <a name="multiple-column-indexes"></a>여러 열 인덱스

여러 열에 걸쳐 있는 인덱스는 지정 된 테이블에 대 한 여러 인덱스 주석에 동일한 이름을 사용 하 여 지정 됩니다. 다중 열 인덱스를 만들 때는 인덱스의 열에 대 한 순서를 지정 해야 합니다. 예를 들어 다음 코드는 **IX\_BlogIdAndRating**이라는 **등급** 및 **BlogId** 에 대 한 다중 열 인덱스를 만듭니다. **BlogId** 는 인덱스의 첫 번째 열이 고 **등급** 은 second입니다.

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index("IX_BlogIdAndRating", 2)]
        public int Rating { get; set; }
        [Index("IX_BlogIdAndRating", 1)]
        public int BlogId { get; set; }
    }
```

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a>관계 특성: InverseProperty 및 ForeignKey

> [!NOTE]
> 이 페이지에서는 데이터 주석을 사용 하 여 Code First 모델에서 관계를 설정 하는 방법에 대 한 정보를 제공 합니다. EF의 관계 및 관계를 사용 하 여 데이터에 액세스 하 고 조작 하는 방법에 대 한 일반적인 내용은 [관계 & 탐색 속성](~/ef6/fundamentals/relationships.md)을 참조 하세요.

Code first 규칙은 모델에서 가장 일반적으로 사용 되는 관계를 처리 하지만 도움이 필요한 경우도 있습니다.

블로그 클래스의 키 속성 이름을 변경 하면 Post와의 관계에 문제가 발생 했습니다. 

데이터베이스를 생성할 때 code first는 Post 클래스에서 BlogId 속성을 확인 하 고 클래스 이름과 "Id"와 일치 하는 규칙에 따라 블로그 클래스에 대 한 외래 키로이 속성을 인식 합니다. 그러나 블로그 클래스에는 BlogId 속성이 없습니다. 이를 위한 솔루션은 Post에서 탐색 속성을 만들고 외래 DataAnnotation을 사용 하 여 코드에서 BlogId 속성을 사용 하 여 두 클래스 간의 관계를 만드는 방법 뿐만 아니라에서 제약 조건을 지정 하는 방법을 이해 하는 데 도움이 됩니다. 데이터.

``` csharp
    public class Post
    {
            public int Id { get; set; }
            public string Title { get; set; }
            public DateTime DateCreated { get; set; }
            public string Content { get; set; }
            public int BlogId { get; set; }
            [ForeignKey("BlogId")]
            public Blog Blog { get; set; }
            public ICollection<Comment> Comments { get; set; }
    }
```

데이터베이스의 제약 조건은 InternalBlogs. PrimaryTrackingKey 및 BlogId 간의 관계를 보여 줍니다. 

![InternalBlogs 관계. PrimaryTrackingKey 및 BlogId](~/ef6/media/jj591583-figure09.png)

InverseProperty는 클래스 간에 여러 관계가 있는 경우에 사용 됩니다.

Post 클래스에서는 블로그 게시물을 수정한 사람 뿐만 아니라 블로그 게시물을 편집한 사람을 추적 하는 것이 좋습니다. 다음은 Post 클래스에 대 한 두 가지 새로운 탐색 속성입니다.

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

또한 이러한 속성이 참조 하는 Person 클래스에를 추가 해야 합니다. Person 클래스에는 사람이 기록한 모든 게시물에 대 한 탐색 속성과 해당 사용자가 업데이트 한 모든 게시물에 대 한 탐색 속성이 있습니다.

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

Code first는 두 클래스의 속성을 고유 하 게 일치 시킬 수 없습니다. 게시물에 대 한 데이터베이스 테이블에는 CreatedBy person에 대 한 하나의 외래 키와 UpdatedBy person에 대 한 외래 키가 있어야 하지만 code first는 Person\_Id, Person\_Id1, CreatedBy\_Id 및 UpdatedBy\_Id 라는 4 개의 외래 키 속성을 만듭니다.

![추가 외래 키가 있는 테이블 게시](~/ef6/media/jj591583-figure10.png)

이러한 문제를 해결 하기 위해 InverseProperty 주석을 사용 하 여 속성의 맞춤을 지정할 수 있습니다.

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

사용자의 PostsWritten 속성은이가 Post 형식을 참조 한다는 것을 알고 있으므로 CreatedBy에 대 한 관계를 빌드합니다. 마찬가지로 PostUpdatedBy에 연결 됩니다. 그리고 code first는 추가 외래 키를 만들지 않습니다.

![추가 외래 키가 없는 게시물 테이블](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a>요약

DataAnnotations을 사용 하면 코드의 첫 번째 클래스에서 클라이언트 및 서버 쪽 유효성 검사를 설명할 수 있을 뿐만 아니라 해당 규칙에 따라 코드에서 가장 먼저 수행 하는 가정을 개선 하 고 수정할 수도 있습니다. DataAnnotations을 사용 하면 데이터베이스 스키마를 생성할 수 있을 뿐만 아니라 code first 클래스를 기존 데이터베이스에 매핑할 수도 있습니다.

데이터 주석은 매우 유연 하지만, 코드 첫 번째 클래스에서 수행할 수 있는 가장 일반적으로 필요한 구성 변경만 제공 한다는 점에 유의 하세요. 일부 edge 사례에 대 한 클래스를 구성 하려면 대체 구성 메커니즘 Code First의 흐름 API를 확인 해야 합니다.
