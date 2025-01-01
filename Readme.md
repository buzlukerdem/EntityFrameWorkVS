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

Kullanımı:
* Scaffold talimatı ile veritabanının kod kısmında modellenmesi.
Talimat;
**Scaffold-DbContext 'Connection String' Microsoft.EntityFrameworkCore.[DBProvider]**
* Connection String i elde etmek için link => [ConnectionStrings](https://www.connectionstrings.com/)
* Database Provider kütüphanesi için link =>
[DB-Providers](https://learn.microsoft.com/en-us/ef/core/providers/?tabs=dotnet-core-cli)

Package Manager Console üzerinde Scaffold Talimatları;
**1.Veritabanı'nın Modellenmesi**
```
Scaffold-DbContext'Connection String' Microsoft.EntityFrameworkCore.SqlServer
```
**2.Veritabanı değişikliğinde Model Güncelleme(Force)**
```
Scaffold-DbContext'Connection String' Microsoft.EntityFrameworkCore.SqlServer -Force
```
**3.Belirli Tablolar ile Modelleme(Tables)**
```
Scaffold-DbContext'Connection String' Microsoft.EntityFrameworkCore.SqlServer -Tables Table1, Table2, Table3
```
**4.Özel NameSpace ve Path ismi ile Modelleme(Contextdir,Outputdir)**
```
Scaffold-DbContext'Connection String' Microsoft.EntityFrameworkCore.SqlServer -ContextDir Contexts -OutputDir Entities
```
Context ve Entity class'ları **PARTIAL** olarak oluşturulmaktadır.

**Microsoft.EntityFrameworkCore.Tools** ve kullanılacak olan veritabanı hangisi ise ona uygun provider Nuget üzerinden yüklenmelidir.


##### 2. CODE FIRST APPROACH/YAKLAŞIM
* Önceden oluşturulmamış veritabanınının kod kısmında modellenmesi ile veritabanı, veritabanı sunucusunda oluşturulmaktadır.
* Migration ile gerçekleştirilmektedir.
* Veritabanında herhangi bir değişiklik kontrol edilip güncelleme davranışına gerek duyulmamaktadır.
* Oracle - PostgreSQL - SQL Server vs. ef core desteklenen veritabanlarında kullanılabilirdir.

Kod kısmmında modellenen veritabanı ve tabloları **DbContext** sınıfında **migrate** edilerek veritabanında oluşturmaktadır.

Migrate işleminden önce aktarılacak olan Veritabanı Tablolarına karşılık gelecek entity modelleri oluşturulmalıdır.
Oluşturulan Entity Modelleri DbContext sınıfında DbSet ile tanımlanmalıdır.
Kalıtımsal olarak gelen OnCofiguring metotu ile kullanılacak server optionsBuilder nesnesi ile ConnectionString bildirilir.
 <img src="/images/sqloptions.png" alt="Alt Text" style="width:175%; height:auto;">

**Microsoft.EntityFrameworkCore.Tools** ve kullanılacak olan veritabanı hangisi ise ona uygun PROVIDER Nuget üzerinden yüklenmelidir.

**Migration Sınıfı Oluşturma Package Manager Console'da**
```
add-migration [MigrateName]
```
* Oluşan MigrateName adında Migration sınfından kalıtım alan sınıf Up ve Down metotları içermektedir.
**Up**: Ekleme Operasyonu Yürütmektedir.
**Down**: Geri Alma Operasyonu Yürütmektedir.

Bu sınıf, veritabanına migrate ile gönderilmektedir.
<h5>Up Fonksiyonu</h5>

```
update-database 
```

<h5>Down Fonksiyonu(Migrationa Geri Dönme)</h5>

```
update-database [MigName]
```

<h5>Migration Silme</h5>

```
remove-migration
```


##### VERILERDE İŞLEMLER

* **1.EKLEME**
Veritabanı context nesnesi üretilmesi gerekmekedir.
Context üzerinden **AddAsync** metodu ile entity nesnesini ekleme işlemi gerçekleştirilir.
Nesnenin Entry State'i Added durumundadır.Context nesnesi ile **SaveChangesAsync()** metodu ile veritabanının anlayacağı sorguyu oluşturur ve execute eder.
Veritabanı arkada bir transaction işlemi yürütmektedir. Yapılacak her işlemden sonra SaveChanges metodunu tetiklemek maliyetli olacaktır. Yapılacak işlemler oluşturulduktan sonra SaveChanges metodunu tetiklemek daha doğru olacaktır.
Sorgulama esnasında herhangi biri başarısız olursa **RollBack** ile tüm işlemler geri alınmaktadır.
AddRange metodu ile birden çok veri ekleme işlemi gerçekleştirilebilmektedir.
<br>

* **2.GÜNCELLEME**
Güncellenecek olan veriye öncelikle erişilmesi gerekmektedir. Erişim Entity'nin primary keyi yani Id'si üzerinden sağlanmaktadır.
Elde edilen veri üzerinden yapılan güncellemelerden sonra SaveChanges ile veritabanına çalışacak sorgu gönderilip execute edilmektedir.
(<a href="#ChangeTracker">**ChangeTracker**</a>) context üzerinden talep edilen nesnenin takibini sağlamaktadır.
Yapılacak İşlemleri(Update-Delete) takip etmekte ve SaveChanges metoduna ile oluşturulacak sorguyu ayırt etmeyi sağlamaktadır.
<br>

* **3.SİLME**
Id'ye göre elde edilen veri context nesnesinin Remove metotu ile deleted state olmaktadır ve SaveChanges ile veritabanına çalışacak sorgu gönderilip execute edilmektedir.
**RemoveRange** ile birden fazla nesne silme işlemi gerçekleştirilebilmektedir.
<br>

##### VERILERDE SORGULAMA
Sorgulama durumları;
* **IQueryable**: Oluşturulan sorgunun execute edilmemiş hali/sorgulama durumunu temsil eder.
* **IEnumerable**: Sorgunun çalışıp execute edilmiş hali, sorgu neticesinde verilerin in memory'e yüklenme durumunu temsil eder.

**Method Syntax** ile verileri elde ederken Context nesnesi üzerinden DbSet Entity'e karşılık Tablo adı .ToListAsync() metodu ile execute edilir.(IEnumerable Durumu.)
```Csharp
var values = await context.TableName.ToListAsync();
```
**QuerySyntax** ile veriler Linq sorgusu ile elde edilir ve .ToListAsync() metodu ile execute edilir.
```Csharp
var values = (from v in context.TableName
              select v).ToListAsync();
```

**DEFERRED EXECUTION**
Ertelenmiş çalışma anlamına gelmektedir. IQueryable durumunda olan sorgu yazıldığı noktada tetiklenmez. Execute edildiği noktada tetiklenir. Sorgulama içerisinde kullanılan değişkenler sonradan değişiklik gösterirse, sorgu çalışmadan önce değişkenlerin son halini ele almaktadır.
* Example:
 ```Csharp
 int _id = 5;
var values = from v in context.TableName
             where v.Id == _id
             select v;
_id = 10;
await values.ToListAsync();
```



<h3><a id="ChangeTracker">Change Tracker</a></br></h3>
Context nesnesinden gelen veriler/nesneler otomatik olarak Change Tracker mekanizması ile takip edilir.
Veriler üzerinde yapılan işlemleri uygun SQL sorgu formatına generate eder.
ChangeTracker propertysi: Takibi yapılan nesnelere erişebilmeyi ve işlemler yapabilmeyi sağlamaktadır.<br>
<br>

**Property**
```csharp
DbContext context = new();
context.ChangeTracker;
```

**DetectChanges Method**
Veriler üzerinde yapılan değişikliklerin ChangeTracker mekanızmasınının takip ettiğinden emin olmak için SaveChanges metodundan önce DetectChanges metodu ile manuel bir şekilde kullanılabilmektedir. SaveChanges özünde DetectChanges metodunu çağırmaktadır.

**Entries Method**
Takip eidlen nesnelerin bilgisini EntityEntry olarak elde etmeyi sağlamaktadır.
EntityState e göre belirli işlemler/operasyonlar sağlayabilinmektedir.

**AcceptAllChanges Method**
SaveChanges() metodunun default parametresi true'dur. Tetiklendiğinde yapılan değişikliklerin doğru olduğunu kabul etmekte  ve nesnelerin takibini kesmektedir. Beklenmeyen bir olası hata durumunda nesnelerin takibini bıraktığı için düzeltme işlemleri gerçekleştirilemeyecektir.
**SaveChanges(false)** başarılı veya başarısız takip etmeyi bırakmamaktadır. Bu yüzden değişiklik/onarım yapmaya olanak sağlamaktadır.
AcceptAllChanges metodu değişikliklerin onaylandığı ve takibinin bırakılmasını sağlamaktadır.
```csharp
DbContext context = new();
await context.SaveChangesAsync(false);
/// Belirli Operasyonlar...
context.ChangeTracker.AcceptAllChanges();
```

<h4>Entity States</h4>
Entity nesnelerinin durumları;

* Detached: Nesnenin takip edilmediğini ifade etmekdeir.
* Added: Henüz veritabanına işlenmemiş verinin savechanges metodu ile insert sorgusu oluşturacağı anlamına gelmektedir.
* Unchanged: Nesne üzerinde herhangi bir değişiklik olmadığını ifade etmektedir.
* Modified: Nesne üzerinde değişiklik olduğunda update işlemi yapacağı anlamına gelmektedir.
* Deleted: Nesne Remove metodu ile kaldırıldığında Delete işlemiyapacağı anlamına gelmektedir.

