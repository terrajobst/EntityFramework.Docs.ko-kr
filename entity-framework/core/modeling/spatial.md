---
title: 공간 데이터-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 5b45f83ca7f02665f52ccfe16b5af506a6046a62
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124433"
---
# <a name="spatial-data"></a>공간 데이터

> [!NOTE]
> 이 기능은 EF Core 2.2에서 추가 되었습니다.

공간 데이터는 물리적 위치와 개체의 모양을 나타냅니다. 많은 데이터베이스는 다른 데이터와 함께 인덱싱 및 쿼리할 수 있도록 이러한 유형의 데이터에 대 한 지원을 제공 합니다. 일반적인 시나리오는 위치에서 지정 된 거리 내에 있는 개체에 대 한 쿼리를 포함 하거나 테두리가 지정 된 위치를 포함 하는 개체를 선택 하는 것입니다. EF Core [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) 공간 라이브러리를 사용 하 여 공간 데이터 형식에 대 한 매핑을 지원 합니다.

## <a name="installing"></a>설치

EF Core에서 공간 데이터를 사용 하려면 적절 한 지원 NuGet 패키지를 설치 해야 합니다. 설치 해야 하는 패키지는 사용 중인 공급자에 따라 달라 집니다.

EF Core 공급자                        | 공간 NuGet 패키지
--------------------------------------- | ---------------------
Microsoft.EntityFrameworkCore.SqlServer | [Microsoft.entityframeworkcore.tools.dotnet. NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
Microsoft.EntityFrameworkCore.Sqlite    | [Microsoft.entityframeworkcore.tools.dotnet NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
Microsoft.EntityFrameworkCore.InMemory  | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite)
Npgsql.EntityFrameworkCore.PostgreSQL   | [Npgsql. PostgreSQL. NetTopologySuite](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a>리버스 엔지니어링

공간 NuGet 패키지는 공간 속성을 사용 하 여 [리버스 엔지니어링](../managing-schemas/scaffolding.md) 모델을 사용할 수도 있지만 `Scaffold-DbContext` 또는 `dotnet ef dbcontext scaffold`을 실행 ***하기 전에*** 패키지를 설치 해야 합니다. 그렇지 않으면 열에 대 한 형식 매핑을 찾을 수 없다는 경고가 표시 되 고 열이 생략 됩니다.

## <a name="nettopologysuite-nts"></a>NetTopologySuite (NTS)

NetTopologySuite는 .NET의 공간 라이브러리입니다. EF Core를 사용 하면 모델에서 NTS 유형을 사용 하 여 데이터베이스의 공간 데이터 형식에 매핑할 수 있습니다.

NTS를 통해 공간 형식에 매핑할 수 있도록 하려면 공급자의 DbContext options builder에서 UseNetTopologySuite 메서드를 호출 합니다. 예를 들어 SQL Server를 사용 하 여 다음과 같이 호출할 수 있습니다.

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

여러 공간 데이터 형식이 있습니다. 사용할 형식은 허용할 셰이프 유형에 따라 달라 집니다. 모델의 속성에 사용할 수 있는 NTS 형식의 계층 구조는 다음과 같습니다. `NetTopologySuite.Geometries` 네임 스페이스 내에 있습니다.

* geometry
  * 요소
  * LineString
  * 다각형
  * GeometryCollection
    * 다중 포인트
    * MultiLineString
    * MultiPolygon

> [!WARNING]
> CircularString, CompoundCurve 및 NTS는에서 지원 되지 않습니다.

기본 Geometry 형식을 사용 하면 모든 형식의 모양을 속성으로 지정할 수 있습니다.

다음 엔터티 클래스를 사용 하 여 [와이드 세계의 가져오기 샘플 데이터베이스](https://go.microsoft.com/fwlink/?LinkID=800630)의 테이블에 매핑할 수 있습니다.

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public Point Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public Geometry Border { get; set; }
}
```

### <a name="creating-values"></a>값 만들기

생성자를 사용 하 여 geometry 개체를 만들 수 있습니다. 그러나 NTS는 기 하 도형 팩터리를 대신 사용 하는 것이 좋습니다. 이를 통해 기본 SRID (좌표에서 사용 되는 공간 참조 시스템)를 지정할 수 있으며 전체 자릿수 모델 (계산 중 사용) 및 좌표 시퀀스 (좌표를 결정)와 같은 고급 작업을 제어할 수 있습니다. 및 측정값을 사용할 수 있습니다.

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> 4326는 GPS 및 기타 지리적 시스템에서 사용 되는 표준인 WGS 84를 나타냅니다.

### <a name="longitude-and-latitude"></a>경도 및 위도

NTS의 좌표는 X 및 Y 값을 기준으로 합니다. 경도 및 위도를 나타내려면 경도에 X를, 위도에 Y를 사용 합니다. 이 값은 일반적으로 이러한 값이 표시 되는 `latitude, longitude` 형식에서 **이전** 입니다.

### <a name="srid-ignored-during-client-operations"></a>클라이언트 작업 중에 무시 되는 SRID

NTS는 작업 중에 SRID 값을 무시 합니다. 평면 좌표계를 가정 합니다. 즉, 경도 및 위도 면에서 좌표를 지정 하는 경우 거리, 길이 및 영역과 같은 일부 클라이언트 계산 값은 단위가 아니라도 단위로 표시 됩니다. 보다 의미 있는 값을 사용 하려면 먼저 이러한 값을 계산 하기 전에 [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) 와 같은 라이브러리를 사용 하 여 좌표를 다른 좌표계에 프로젝션 해야 합니다.

작업이 SQL을 통해 EF Core 의해 서버에서 계산 되는 경우 결과의 단위는 데이터베이스에 의해 결정 됩니다.

다음은 ProjNet4GeoAPI를 사용 하 여 두 도시 간의 거리를 계산 하는 예입니다.

``` csharp
static class GeometryExtensions
{
    static readonly CoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                [4326] = GeographicCoordinateSystem.WGS84.WKT,

                // This coordinate system covers the area of our data.
                // Different data requires a different coordinate system.
                [2855] =
                @"
                    PROJCS[""NAD83(HARN) / Washington North"",
                        GEOGCS[""NAD83(HARN)"",
                            DATUM[""NAD83_High_Accuracy_Regional_Network"",
                                SPHEROID[""GRS 1980"",6378137,298.257222101,
                                    AUTHORITY[""EPSG"",""7019""]],
                                AUTHORITY[""EPSG"",""6152""]],
                            PRIMEM[""Greenwich"",0,
                                AUTHORITY[""EPSG"",""8901""]],
                            UNIT[""degree"",0.01745329251994328,
                                AUTHORITY[""EPSG"",""9122""]],
                            AUTHORITY[""EPSG"",""4152""]],
                        PROJECTION[""Lambert_Conformal_Conic_2SP""],
                        PARAMETER[""standard_parallel_1"",48.73333333333333],
                        PARAMETER[""standard_parallel_2"",47.5],
                        PARAMETER[""latitude_of_origin"",47],
                        PARAMETER[""central_meridian"",-120.8333333333333],
                        PARAMETER[""false_easting"",500000],
                        PARAMETER[""false_northing"",0],
                        UNIT[""metre"",1,
                            AUTHORITY[""EPSG"",""9001""]],
                        AUTHORITY[""EPSG"",""2855""]]
                "
            });

    public static Geometry ProjectTo(this Geometry geometry, int srid)
    {
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        var result = geometry.Copy();
        result.Apply(new MathTransformFilter(transformation.MathTransform));

        return result;
    }

    class MathTransformFilter : ICoordinateSequenceFilter
    {
        readonly MathTransform _transform;

        public MathTransformFilter(MathTransform transform)
            => _transform = transform;

        public bool Done => false;
        public bool GeometryChanged => true;

        public void Filter(CoordinateSequence seq, int i)
        {
            var result = _transform.Transform(
                new[]
                {
                    seq.GetOrdinate(i, Ordinate.X),
                    seq.GetOrdinate(i, Ordinate.Y)
                });
            seq.SetOrdinate(i, Ordinate.X, result[0]);
            seq.SetOrdinate(i, Ordinate.Y, result[1]);
        }
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a>데이터 쿼리

LINQ에서 데이터베이스 함수로 사용할 수 있는 NTS 메서드와 속성은 SQL로 변환 됩니다. 예를 들어 Distance 및 Contains 메서드는 다음 쿼리에서 변환 됩니다. 이 문서의 끝에 있는 표에는 다양 한 EF Core 공급자가 지 원하는 멤버가 나와 있습니다.

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a>SQL Server

SQL Server를 사용 하는 경우 알아야 할 몇 가지 추가 사항이 있습니다.

### <a name="geography-or-geometry"></a>지리 또는 기 하 도형

기본적으로 공간 속성은 SQL Server의 `geography` 열에 매핑됩니다. `geometry`를 사용 하려면 모델에서 [열 유형을 구성](xref:core/modeling/entity-properties#column-data-types) 합니다.

### <a name="geography-polygon-rings"></a>지리 다각형 링

`geography` 열 유형을 사용 하는 경우 SQL Server는 외부 링 (또는 셸) 및 내부 링 (또는 구멍)에 추가 요구 사항을 적용 합니다. 외부 링은 시계 반대 방향으로, 내부는 시계 방향으로 링 해야 합니다. NTS는 데이터베이스에 값을 보내기 전에 유효성을 검사 합니다.

### <a name="fullglobe"></a>FullGlobe

SQL Server에는 `geography` 열 형식을 사용할 때 전체 구형을 나타내는 비표준 geometry 형식이 있습니다. 또한 전체 구형 (외부 링 제외)을 기반으로 하는 다각형을 나타내는 방법도 있습니다. 이러한 두 가지 모두 NTS에서 지원 되지 않습니다.

> [!WARNING]
> 이를 기반으로 하는 FullGlobe 및 다각형은 NTS에서 지원 되지 않습니다.

## <a name="sqlite"></a>SQLite

SQLite를 사용 하는 몇 가지 추가 정보는 다음과 같습니다.

### <a name="installing-spatialite"></a>SpatiaLite 설치

Windows에서 네이티브 mod_spatialite 라이브러리는 NuGet 패키지 종속성으로 배포 됩니다. 다른 플랫폼은 개별적으로 설치 해야 합니다. 일반적으로 소프트웨어 패키지 관리자를 사용 하 여 수행 됩니다. 예를 들어 MacOS에서 Ubuntu 및 Homebrew의 APT를 사용할 수 있습니다.

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

아쉽게도 최신 버전의 PROJ (SpatiaLite의 종속성)는 EF의 기본 [SQLitePCLRaw 번들](/dotnet/standard/data/sqlite/custom-versions#bundles)과 호환 되지 않습니다. 시스템 SQLite 라이브러리를 사용 하는 사용자 지정 [SQLitePCLRaw 공급자](/dotnet/standard/data/sqlite/custom-versions#sqlitepclraw-providers) 를 만들거나 SpatiaLite PROJ 지원을 사용 하지 않도록 설정 하는 사용자 지정 빌드를 설치 하 여이 문제를 해결할 수 있습니다.

``` sh
curl https://www.gaia-gis.it/gaia-sins/libspatialite-4.3.0a.tar.gz | tar -xz
cd libspatialite-4.3.0a

if [[ `uname -s` == Darwin* ]]; then
    # Mac OS requires some minor patching
    sed -i "" "s/shrext_cmds='\`test \\.\$module = .yes && echo .so \\|\\| echo \\.dylib\`'/shrext_cmds='.dylib'/g" configure
fi

./configure --disable-proj
make
make install
```

### <a name="configuring-srid"></a>SRID 구성

SpatiaLite에서 열은 열 당 SRID를 지정 해야 합니다. 기본 SRID `0`입니다. ForSqliteHasSrid 메서드를 사용 하 여 다른 SRID를 지정 합니다.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a>Dimension

SRID와 마찬가지로 열의 차원 (또는 좌표)도 열의 일부로 지정 됩니다. 기본 좌표는 X 및 Y입니다. ForSqliteHasDimension 메서드를 사용 하 여 추가 좌표 (Z 및 M)를 사용 하도록 설정 합니다.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a>번역 된 작업

이 표에서는 각 EF Core 공급자가 SQL로 변환 하는 NTS 멤버를 보여 줍니다.

NetTopologySuite | SQL Server (기 하 도형) | SQL Server (지리) | SQLite | Npgsql
--- |:---:|:---:|:---:|:---:
기 하 도형. 영역 | ✔ | ✔ | ✔ | ✔
Geometry. AsBinary () | ✔ | ✔ | ✔ | ✔
기 하 도형. 고가 () | ✔ | ✔ | ✔ | ✔
Geometry. 경계 | ✔ | | ✔ | ✔
Geometry. Buffer (double) | ✔ | ✔ | ✔ | ✔
Geometry. Buffer (double, int) | | | ✔ | ✔
중심 | ✔ | | ✔ | ✔
Geometry. Contains (Geometry) | ✔ | ✔ | ✔ | ✔
ConvexHull () | ✔ | ✔ | ✔ | ✔
Geometry. CoveredBy (Geometry) | | | ✔ | ✔
기 하 도형. 커버 (Geometry) | | | ✔ | ✔
Geometry (Geometry) | ✔ | | ✔ | ✔
기 하 도형. 차 (기 하 도형) | ✔ | ✔ | ✔ | ✔
Geometry. 차원 | ✔ | ✔ | ✔ | ✔
Geometry. 비연속 (Geometry) | ✔ | ✔ | ✔ | ✔
기 하 도형. 거리 (기 하 도형) | ✔ | ✔ | ✔ | ✔
기 하 도형. 봉투 | ✔ | | ✔ | ✔
Geometry. EqualsExact (Geometry) | | | | ✔
Geometry. EqualsTopologically (Geometry) | ✔ | ✔ | ✔ | ✔
GeometryType | ✔ | ✔ | ✔ | ✔
GetGeometryN (int) | ✔ | | ✔ | ✔
InteriorPoint | ✔ | | ✔ | ✔
기 하 도형. 교차로 (Geometry) | ✔ | ✔ | ✔ | ✔
기 하 도형. 교차 (기 하 도형) | ✔ | ✔ | ✔ | ✔
IsEmpty | ✔ | ✔ | ✔ | ✔
Geometry. IsSimple | ✔ | | ✔ | ✔
Geometry. IsValid | ✔ | ✔ | ✔ | ✔
Geometry. IsWithinDistance (Geometry, double) | ✔ | | ✔ | ✔
기 하 도형. 길이 | ✔ | ✔ | ✔ | ✔
기 하 도형. NumGeometries | ✔ | ✔ | ✔ | ✔
기 하 도형. NumPoints | ✔ | ✔ | ✔ | ✔
OgcGeometryType | ✔ | ✔ | ✔ | ✔
기 하 도형. 겹치기 (기 하 도형) | ✔ | ✔ | ✔ | ✔
기 하 도형. PointOnSurface | ✔ | | ✔ | ✔
Geometry. Relate (Geometry, string) | ✔ | | ✔ | ✔
Geometry. Reverse () | | | ✔ | ✔
SRID | ✔ | ✔ | ✔ | ✔
Geometry. SymmetricDifference (Geometry) | ✔ | ✔ | ✔ | ✔
Geometry. ToBinary () | ✔ | ✔ | ✔ | ✔
Geometry. ToText () | ✔ | ✔ | ✔ | ✔
기 하 도형 (기 하 도형) | ✔ | | ✔ | ✔
Geometry. Union () | | | ✔ | ✔
Geometry. Union (Geometry) | ✔ | ✔ | ✔ | ✔
Geometry. 내 (기 하 도형) | ✔ | ✔ | ✔ | ✔
GeometryCollection | ✔ | ✔ | ✔ | ✔
GeometryCollection [int] | ✔ | ✔ | ✔ | ✔
LineString | ✔ | ✔ | ✔ | ✔
LineString | ✔ | ✔ | ✔ | ✔
LineString. GetPointN (int) | ✔ | ✔ | ✔ | ✔
LineString. IsClosed | ✔ | ✔ | ✔ | ✔
LineString | ✔ | | ✔ | ✔
LineString | ✔ | ✔ | ✔ | ✔
MultiLineString. IsClosed | ✔ | ✔ | ✔ | ✔
Point. M | ✔ | ✔ | ✔ | ✔
Point. X | ✔ | ✔ | ✔ | ✔
Point. Y | ✔ | ✔ | ✔ | ✔
Point. Z | ✔ | ✔ | ✔ | ✔
ExteriorRing | ✔ | ✔ | ✔ | ✔
GetInteriorRingN (int) | ✔ | ✔ | ✔ | ✔
NumInteriorRings | ✔ | ✔ | ✔ | ✔

## <a name="additional-resources"></a>추가 자료

* [SQL Server의 공간 데이터](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [SpatiaLite 홈 페이지](https://www.gaia-gis.it/fossil/libspatialite)
* [Npgsql 공간 설명서](https://www.npgsql.org/efcore/mapping/nts.html)
* [PostGIS 설명서](https://postgis.net/documentation/)
