---
title: "동시성-EF 핵심 처리"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bce0539d-b0cd-457d-be71-f7ca16f3baea
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: bbd3e154c1b27b16c7d8f8fbf9ed51df0849795c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="handling-concurrency"></a>동시성 처리

속성이 동시성 토큰으로 구성 된 다른 사용자가 해당 레코드에 변경 내용을 저장할 때 데이터베이스의 해당 값을 수정 EF 확인 됩니다.

> [!TIP]  
> 이 문서를 볼 수 있습니다 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) GitHub에서 합니다.

## <a name="how-concurrency-handling-works-in-ef-core"></a>EF 코어에서 동시성 처리의 작동 방식

Entity Framework Core에서 동시성 처리의 작동 방식에 대 한 자세한 설명을 참조 하십시오. [동시성 토큰](../modeling/concurrency.md)합니다.

## <a name="resolving-concurrency-conflicts"></a>동시성 충돌 해결

동시성 충돌을 해결에서는 데이터베이스에서 변경으로 현재 사용자의 보류 중인 변경 내용을 병합 하는 알고리즘을 사용 합니다. 정확한 방법은 응용 프로그램에 따라 달라 집니다 하지만 일반적인 방법은 사용자에 게 값을 표시 하 고 올바른 값을 데이터베이스에 저장할 수를 결정 하는 것입니다.

**동시성 충돌을 해결 하기 위해 사용할 수 있는 값의 세 가지 집합이 있습니다.**

* **현재 값** 응용 프로그램 데이터베이스에 기록 하려고 했던 되는 값입니다.

* **원래 값** 편집을 수행 하기 전에 원래 데이터베이스에서 검색 되는 값입니다.

* **값을 데이터베이스** 현재 데이터베이스에 저장 하는 값입니다.

동시성 충돌을 처리 하려면 catch는 `DbUpdateConcurrencyException` 중 `SaveChanges()`를 사용 하 여 `DbUpdateConcurrencyException.Entries` 영향을 받는 엔터티에 대 한 새 변경 집합을 준비 하 고 다음 다시 시도 하는 `SaveChanges()` 작업 합니다.

다음 예에서 `Person.FirstName` 및 `Person.LastName` 동시성 토큰으로 설정 되도록 합니다. 한 `// TODO:` 포함 응용 프로그램별 논리를 데이터베이스에 저장 된 값을 선택할 수 있는 위치에 대 한 설명입니다.

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Concurrency/Sample.cs?highlight=53,54)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System;
using System.ComponentModel.DataAnnotations;
using System.Linq;

namespace EFSaving.Concurrency
{
    public class Sample
    {
        public static void Run()
        {
            // Ensure database is created and has a person in it
            using (var context = new PersonContext())
            {
                context.Database.EnsureDeleted();
                context.Database.EnsureCreated();

                context.People.Add(new Person { FirstName = "John", LastName = "Doe" });
                context.SaveChanges();
            }

            using (var context = new PersonContext())
            {
                // Fetch a person from database and change phone number
                var person = context.People.Single(p => p.PersonId == 1);
                person.PhoneNumber = "555-555-5555";

                // Change the persons name in the database (will cause a concurrency conflict)
                context.Database.ExecuteSqlCommand("UPDATE dbo.People SET FirstName = 'Jane' WHERE PersonId = 1");

                try
                {
                    // Attempt to save changes to the database
                    context.SaveChanges();
                }
                catch (DbUpdateConcurrencyException ex)
                {
                    foreach (var entry in ex.Entries)
                    {
                        if (entry.Entity is Person)
                        {
                            // Using a NoTracking query means we get the entity but it is not tracked by the context
                            // and will not be merged with existing entities in the context.
                            var databaseEntity = context.People.AsNoTracking().Single(p => p.PersonId == ((Person)entry.Entity).PersonId);
                            var databaseEntry = context.Entry(databaseEntity);

                            foreach (var property in entry.Metadata.GetProperties())
                            {
                                var proposedValue = entry.Property(property.Name).CurrentValue;
                                var originalValue = entry.Property(property.Name).OriginalValue;
                                var databaseValue = databaseEntry.Property(property.Name).CurrentValue;

                                // TODO: Logic to decide which value should be written to database
                                // entry.Property(property.Name).CurrentValue = <value to be saved>;

                                // Update original values to
                                entry.Property(property.Name).OriginalValue = databaseEntry.Property(property.Name).CurrentValue;
                            }
                        }
                        else
                        {
                            throw new NotSupportedException("Don't know how to handle concurrency conflicts for " + entry.Metadata.Name);
                        }
                    }

                    // Retry the save operation
                    context.SaveChanges();
                }
            }
        }

        public class PersonContext : DbContext
        {
            public DbSet<Person> People { get; set; }

            protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            {
                optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFSaving.Concurrency;Trusted_Connection=True;");
            }
        }

        public class Person
        {
            public int PersonId { get; set; }

            [ConcurrencyCheck]
            public string FirstName { get; set; }

            [ConcurrencyCheck]
            public string LastName { get; set; }

            public string PhoneNumber { get; set; }
        }

    }
}
```
