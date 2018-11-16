---
title: 공간 데이터-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 49c18758af2f2383ea082ead2f6df4c022152b36
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688793"
---
# <a name="spatial-data"></a>공간 데이터

> [!NOTE]
> 이 기능은 EF Core 2.2의 새로운 기능입니다.

공간 데이터는 물리적 위치와 개체의 모양을 나타냅니다. 많은 데이터베이스는 인덱싱 및 다른 데이터와 함께 쿼리할 수 있도록 이러한 종류의 데이터를 지원 합니다. 위치에서 지정 된 거리 내에 있는 개체에 대 한 쿼리 또는 테두리가 지정된 된 위치를 포함 하는 개체를 선택 하면 일반적인 시나리오에 포함 됩니다. EF Core를 사용 하 여 공간 데이터 형식 매핑을 지원 합니다 [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) 공간 라이브러리입니다.

## <a name="installing"></a>설치

공간 데이터 및 EF Core를 사용 하려면 적절 한 지원 NuGet 패키지를 설치 해야 합니다. 설치 해야 하는 패키지는 사용 중인 공급자에 따라 달라 집니다.

EF Core 공급자                        | 공간 NuGet 패키지
--------------------------------------- | ---------------------
Microsoft.EntityFrameworkCore.SqlServer | [Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
Microsoft.EntityFrameworkCore.Sqlite    | [Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
Microsoft.EntityFrameworkCore.InMemory  | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite)
Npgsql.EntityFrameworkCore.PostgreSQL   | [Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a>리버스 엔지니어링

공간 NuGet 패키지 사용도 [리버스 엔지니어링](../managing-schemas/scaffolding.md) 패키지를 설치 해야 할 수 있지만 공간 속성을 사용 하 여 모델 ***하기 전에*** 실행 `Scaffold-DbContext` 또는 `dotnet ef dbcontext scaffold`합니다. 이렇게 하지 않으면 열에 대 한 형식 매핑을 찾지에 대 한 경고를 받게 됩니다 및 열을 건너뜁니다.

## <a name="nettopologysuite-nts"></a>NetTopologySuite (NTS)

NetTopologySuite에.NET에 대 한 공간 라이브러리입니다. EF Core 모델에서 NTS 형식을 사용 하 여 데이터베이스의 형식은 공간 데이터 매핑 수 있습니다.

NTS 통해 공간 형식에 대 한 매핑을 사용 하려면 공급자의 DbContext 옵션 작성기 UseNetTopologySuite 메서드를 호출 합니다. 예를 들어, SQL Server를 사용 하 여 다음과 같이 호출할는 있습니다.

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

공간 데이터 유형은 여러 가지가 있습니다. 사용 하는 형식을 허용 하려는 모양의 종류에 따라 달라 집니다. 속성에 대 한 모델에 사용할 수 있는 NTS 형식의 계층 구조는 다음과 같습니다. 내는 `NetTopologySuite.Geometries` 네임 스페이스입니다. GeoAPI 패키지에 해당 하는 인터페이스가 (`GeoAPI.Geometries` 네임 스페이스)도 사용할 수 있습니다.

* 기 하 도형
  * 요소
  * LineString
  * 다각형
  * GeometryCollection
    * MultiPoint
    * MultiLineString
    * MultiPolygon

> [!WARNING]
> CircularString, CompoundCurve, 및 CurePolygon NTS에서 지원 되지 않습니다.

기본 기 하 도형 형식을 사용 하 여 모든 종류를의 셰이프를 속성으로 지정할 수 있습니다.

다음 엔터티 클래스를 사용 하 여 테이블에 매핑할 수 없습니다는 [Wide World Importers 샘플 데이터베이스](http://go.microsoft.com/fwlink/?LinkID=800630)합니다.

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public IPoint Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public IGeometry Border { get; set; }
}
```

### <a name="creating-values"></a>값 만들기

생성자를 사용 하 여 기 하 도형 개체를 만들려면 그러나 NTS 대신 geometry 팩터리를 사용 하 여 것이 좋습니다. 이 기본 SRID (공간 참조 시스템 좌표로 사용)을 지정할 수 있습니다 및을 사용 하면 고급 등 (계산 하는 동안 사용 됨)는 전체 자릿수 모델 및 좌표 (좌표는-크기를 결정 합니다. 시퀀스에 대 한 제어 측정값 제공 되며).

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> 4326은 GPS와 다른 지리적 시스템에 사용 되는 표준 WGS 84을 가리킵니다.

### <a name="longitude-and-latitude"></a>경도 및 위도

NTS에 좌표가의 측면에서 X 및 Y 값입니다. 경도 및 위도 나타내는, 경도 및 Y에 대 한 X를 사용 하 여 위도입니다. 이 **이전 버전과** 에서 `latitude, longitude` 는 일반적으로 표시 되는이 값 형식입니다.

### <a name="srid-ignored-during-client-operations"></a>SRID는 클라이언트 작업 중 무시

NTS 작업 중 SRID 값을 무시합니다. 평면 좌표계를 가정합니다. 즉, 경도 및 위도, 거리, 길이 및 영역도 하지 미터의에서 수와 같은 일부 클라이언트 평가 값 기준으로 좌표를 지정 하는 경우. 보다 의미 있는 값에 대 한 해야 프로젝트와 같은 라이브러리를 사용 하 여 다른 좌표 시스템 좌표 [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) 이러한 값을 계산 하기 전에 합니다.

SQL 통해 EF Core에서 서버 평가 작업 인 경우 결과 단위 데이터베이스에 의해 결정 됩니다.

ProjNet4GeoAPI를 사용 하 여 두 도시 간의 거리를 계산 하는 예제는 다음과 같습니다.

``` csharp
static class GeometryExtensions
{
    static readonly IGeometryServices _geometryServices = NtsGeometryServices.Instance;
    static readonly ICoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                // (3857 and 4326 included automatically)

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

    public static IGeometry ProjectTo(this IGeometry geometry, int srid)
    {
        var geometryFactory = _geometryServices.CreateGeometryFactory(srid);
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        return GeometryTransform.TransformGeometry(
            geometryFactory,
            geometry,
            transformation.MathTransform);
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a>데이터 쿼리

Linq에서 NTS 메서드와 데이터베이스 함수를 사용할 수 있는 속성 SQL 변환 됩니다. 예를 들어, 거리 및 Contains 메서드를 다음 쿼리와에서 변환 됩니다. 이 문서의 끝에 있는 표에서 다양 한 EF Core 공급자가 지원 되는 멤버를 보여 줍니다.

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a>SQL Server

SQL Server를 사용 하는 경우 몇 가지 추가 사항을 고려해 야 합니다.

### <a name="geography-or-geometry"></a>지리 또는 기 하 도형

기본적으로 공간 속성에 매핑됩니다 `geography` SQL Server의 열입니다. 사용 하도록 `geometry`, [열 유형을 구성](xref:core/modeling/relational/data-types) 모델에 있습니다.

### <a name="geography-polygon-rings"></a>지리 다각형 링

사용 하는 경우는 `geography` 열 형식, SQL Server는 외부 링 (또는 셸)에서 추가 요구 사항 및 내부 링 (또는 빈 영역이). 외부 링 시계 반대 방향으로 방향 이어야 하며 내부 링이 시계 방향으로 합니다. NTS 값 데이터베이스로 보내기 전에이 확인 합니다.

### <a name="fullglobe"></a>FullGlobe

SQL Server를 사용 하는 경우 전체 구형을 나타내는 geometry 비표준 형식에는 `geography` 열 형식입니다. 또한 전체 구형 (없이 외부 링)에 따라 다각형을 나타낼 수가 있습니다. 이러한 작업은 NTS에서 지원 됩니다.

> [!WARNING]
> FullGlobe 및 다각형에 따라 NTS에서 지원 되지 않습니다.

## <a name="sqlite"></a>SQLite

SQLite를 사용 하는 몇 가지 추가 정보는 다음과 같습니다.

### <a name="installing-spatialite"></a>SpatiaLite 설치

Windows에서 네이티브 mod_spatialite 라이브러리는 NuGet 패키지 종속성으로 배포 됩니다. 다른 플랫폼을 별도로 설치 해야 합니다. 일반적으로 이렇게 소프트웨어 패키지 관리자를 사용 합니다. 예를 들어, Ubuntu 및 MacOS에서 Homebrew에서 APT를 사용할 수 있습니다.

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a>SRID를 구성합니다.

SpatiaLite, 열 열당 SRID를 지정 해야 합니다. 기본 SRID는 `0`합니다. ForSqliteHasSrid 메서드를 사용 하 여 다른 SRID를 지정 합니다.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a>크기

SRID와 마찬가지로, 열의 차원 (또는 부분)도 지정 됩니다 열의 일부로. 기본 좌표는 X 및 Z와 M Y. 사용 추가 부분 ForSqliteHasDimension 메서드를 사용 합니다.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a>번역된 작업

이 표에서 NTS 멤버 각 EF Core 공급자에서 SQL로 변환 됩니다.

NetTopologySuite | SQL Server (geometry) | SQL Server (geography) | SQLite | Npgsql
--- |:---:|:---:|:---:|:---:
Geometry.Area | ✔ | ✔ | ✔ | ✔
Geometry.AsBinary() | ✔ | ✔ | ✔ | ✔
Geometry.AsText() | ✔ | ✔ | ✔ | ✔
Geometry.Boundary | ✔ | | ✔ | ✔
Geometry.Buffer(double) | ✔ | ✔ | ✔ | ✔
Geometry.Buffer (double, int) | | | ✔
Geometry.Centroid | ✔ | | ✔ | ✔
Geometry.Contains(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ConvexHull() | ✔ | ✔ | ✔ | ✔
Geometry.CoveredBy(Geometry) | | | ✔ | ✔
Geometry.Covers(Geometry) | | | ✔ | ✔
Geometry.Crosses(Geometry) | ✔ | | ✔ | ✔
Geometry.Difference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Dimension | ✔ | ✔ | ✔ | ✔
Geometry.Disjoint(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Distance(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Envelope | ✔ | | ✔ | ✔
Geometry.EqualsExact(Geometry) | | | | ✔
Geometry.EqualsTopologically(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.GeometryType | ✔ | ✔ | ✔ | ✔
Geometry.GetGeometryN(int) | ✔ | | ✔ | ✔
Geometry.InteriorPoint | ✔ | | ✔
Geometry.Intersection(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Intersects(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.IsEmpty | ✔ | ✔ | ✔ | ✔
Geometry.IsSimple | ✔ | | ✔ | ✔
Geometry.IsValid | ✔ | ✔ | ✔ | ✔
Geometry.IsWithinDistance (Geometry, double) | ✔ | | ✔
Geometry.Length | ✔ | ✔ | ✔ | ✔
Geometry.NumGeometries | ✔ | ✔ | ✔ | ✔
Geometry.NumPoints | ✔ | ✔ | ✔ | ✔
Geometry.OgcGeometryType | ✔ | ✔ | ✔
Geometry.Overlaps(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.PointOnSurface | ✔ | | ✔ | ✔
Geometry.Relate (Geometry, 문자열) | ✔ | | ✔ | ✔
Geometry.Reverse() | | | ✔ | ✔
Geometry.SRID | ✔ | ✔ | ✔ | ✔
Geometry.SymmetricDifference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ToBinary() | ✔ | ✔ | ✔ | ✔
Geometry.ToText() | ✔ | ✔ | ✔ | ✔
Geometry.Touches(Geometry) | ✔ | | ✔ | ✔
Geometry.Union() | | | ✔
Geometry.Union(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Within(Geometry) | ✔ | ✔ | ✔ | ✔
GeometryCollection.Count | ✔ | ✔ | ✔ | ✔
GeometryCollection [int] | ✔ | ✔ | ✔ | ✔
LineString.Count | ✔ | ✔ | ✔ | ✔
LineString.EndPoint | ✔ | ✔ | ✔ | ✔
LineString.GetPointN(int) | ✔ | ✔ | ✔ | ✔
LineString.IsClosed | ✔ | ✔ | ✔ | ✔
LineString.IsRing | ✔ | | ✔ | ✔
LineString.StartPoint | ✔ | ✔ | ✔ | ✔
MultiLineString.IsClosed | ✔ | ✔ | ✔ | ✔
Point.M | ✔ | ✔ | ✔ | ✔
Point.X | ✔ | ✔ | ✔ | ✔
Point.Y | ✔ | ✔ | ✔ | ✔
Point.Z | ✔ | ✔ | ✔ | ✔
Polygon.ExteriorRing | ✔ | ✔ | ✔ | ✔
Polygon.GetInteriorRingN(int) | ✔ | ✔ | ✔ | ✔
Polygon.NumInteriorRings | ✔ | ✔ | ✔ | ✔

## <a name="additional-resources"></a>추가 자료

* [SQL server에서 공간 데이터](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [SpatiaLite 홈 페이지](https://www.gaia-gis.it/fossil/libspatialite)
* [PostGIS 설명서](http://postgis.net/documentation/)
