# Entity Framework Notları

#### Veritabanı - Yazılım İlişkisi 
* Veritabanı ve Yazılım arasındaki ilişkide SQL Sorguları ile veriler elde edilip işlenmektedir.
* Kodun kirlenmesine sebep olur.
* Veritabanına Bağımlılık oluşur.
* Veritabanından gelen veriler manuel olarak dönüştürülmek zorunda kalmaktadır.
* Bizlerin SQL sorgularımızı kod içerisinden soyutlamamız gerekir.
* Kodun içerisinde yazılacak sorguları QueryObject patternı ile belli bi seviyede soyutlayabilmekteyiz.



### ORM?
 <img src="/images/orm2.png" alt="Alt Text" style="width:95%; height:auto;">


* Yazılım ve veritebanı arasında sorgular ile elde edilecek veriler **OOP(Object Orianted Programming)** ile sağlanmaktadır.
* SQL sorgularına olan bağlılık soyutlanmış olacaktır. 
* Veritabanı objeleri bir OOP nesnesi gibi görülerek ele alınmaktadır.
*  Veritabanı – Veritabanı Tabloları - Veritabanı Tablo Verileri Nesneler ile eşleştirme davranışı sergiler.
* Veritabanı ve SQL sorguları bağımlılığını soyutlamaktadır.
* Veritabanını temsil edecek bir Context Class referans edeceği tablolara karşılık sınıfları içerisinde member olarak bulundurur.
* Veritabanını temsil edecek sınıf DbContext olarak oluşturulmaktadır ve DbContext sınıfından sınıfından kalıtım almalıdır..
##### DbContext
```CSharp
public class DatabaseNameDbContext : DbContext
{
// DbContextten türetildi...
}
```
* Veritabanı nesnesi ile tablo nesneleri arasında nesnel ilişikiyi sağlamaktadır.
* Sorgulamalar DbContext sınıfı üzerinden yürütülür.
* Change Tracking ile sorgu verilerinde değişiklikler takip edilebilmektedir.
* Caching operasyonlarını yönetebilmektedir.

##### ENTITY
* Veritabanındaki tabloları temsil edecek sınıftır.
* Entity sınıfları, DbContext sınıfı içerisinde **DbSet<T.>** ile propety olarak bildirilmektedir.
* Sorgulanabilir sınıflar olması için DbContext içerisinde tanımlanır.
``` Csharp
public class DatabaseNameDbContext : DbContext
{
    public DbSet<Entity1> Entity1s{get;set;}
    public DbSet<Entity2> Entity2s{get;set;}
    public DbSet<Entity3> Entity3s{get;set;}
}
```
* Veritabanı Tablolarının Kolonları Entity sınıfı içerisinde property olarak tanımlanır.
``` Csharp
public class Entity1
{
    public int Entity1Id{get;set;}
    public string Entity1Name{get;set;}
}
```
* Veritabanındaki veriler Entity sınıfının instance/object/veri 'ına karşılık gelmektedir.
``` Csharp
// Veritabanı tablosunda satırdaki veriye karşılık gelen entity nesnesi...
new Entity1()
{
 Entity1Id = 1,
 Entity1Name = EName
}
```

ORM tool'u: **Entity Framework Core**


#### ENTITY FRAMEWORK CORE
* Open Source(Açık Kaynaklıdır.)
* Geliştirilebilir yapıdadır.
* Code First - Database First yaklaşımları ile veritabanı bağlantısını sağlamaktadır.
* LINQ desteklemektedir.

Kodlama sürecinde bizlere;
* Veritabanı-Veritabanı Tabloları
* İlişkili sorgular
* Function
* View
* Stored Procedure
nesnelerini üretip kullanma imkanı sağlamaktadır.

EF Core veritabanı ile çalışmaları nesnel hale getirir.
2 farklı yaklaşım sergilemektedir.
**Veritabanının önceden var olup olmaması yaklaşımı etkilemektedir.**
##### 1. DATABASE FIRST APPROACH/YAKLAŞIM
* Önceden oluşturulmuş bir veritabanı durumu var ise Database First yaklaşımı tercih edilmektedir.
* Veritabanını belirli tool'lar ile kod kısmında OOP eşliğinde modellenmektedir.
* Modelleme işlemi **Scaffold** talimati ile gerçekleştirilmektedir.
* Hazır veritabanını hızlıca modelleme imkanı sağlar.
* Oracle - PostgreSQL - SQL Server vs. ef core desteklenen veritabanlarında kullanılabilirdir.

##### 2. CODE FIRST APPROACH/YAKLAŞIM
* Önceden oluşturulmamış veritabanınının kod kısmında modellenmesi ile veritabanı, veritabanı sunucusunda oluşturulmaktadır.
* Migration ile gerçekleştirilmektedir.
* Veritabanında herhangi bir değişiklik kontrol edilip güncelleme davranışına gerek duyulmamaktadır.
* Oracle - PostgreSQL - SQL Server vs. ef core desteklenen veritabanlarında kullanılabilirdir.











