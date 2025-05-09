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
* Veritabanını temsil edecek sınıf DbContext olarak oluşturulmaktadır ve **DbContext** sınıfından sınıfından kalıtım almalıdır..
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
Kalıtımsal olarak gelen OnCofiguring metodu ile kullanılacak server optionsBuilder nesnesi ile ConnectionString bildirilir.
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
Id'ye göre elde edilen veri context nesnesinin Remove metodu ile deleted state olmaktadır ve SaveChanges ile veritabanına çalışacak sorgu gönderilip execute edilmektedir.
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
* Modified: Nesne üzerinde değişiklik olduğunda Update işlemi yapacağı anlamına gelmektedir.
* Deleted: Nesne Remove metodu ile kaldırıldığında Delete işlemiyapacağı anlamına gelmektedir.

SaveChanges metodu DbcContext sınıfında virtual ile işaretlenmiş ve override edilebilir bir yapıdadır. Override edilerek şartlara göre kullanım amacına göre yapılandırılabilmektedir.

!!![] ChangeTracker ile takip edilen veriler boyutuna göre belirli bir maliyete sebep olabilmektedir.
Elde edilen tüm verilerin takibinin yapılması ve ayrıca veriler sadece listelenmeye tabi tutulacaksa gerekmeye gerekmeyebilmektedir.
<h4>AsNoTracking</h4>
 AsNoTracking metodu ile maliyet azaltılabilir ve 
 elde edilen veriler üzerinde herhangi bir değişiklik(update) işlemi yapılamamaktadır.

AsNoTracking metodu IQueryable durumundayken kullanılır ve daha sonra sorgu execute edilir.
```csharp
DbContext context = new();
await context.TableName1.Include(tb1 => tb1.TableName2).AsNoTracking().ToListAsync();
```
İlişkisel verilerde maliyet olabilmektedir. ChangeTracker'da elde edilen veriler ilişkisel olduğu verilerde aynı değere sahip nesneler memoryde olacağı için tekrar tekrar üretilmemektedir.
Ancak AsNoTracking metodunda bu durum her seferinde yeni nesne oluşturarak ilişkilendirir.

<h4>AsNoTrackingWithIdendityResolution</h4>
Hem takip mekanizması koparılır hem de yinelenen datalarda ChangeTrackor da olduğu gibi bir davranış sergilemek için AsNoTrackingWithIdentityResolution metodunu kullanabilmektedir.

```csharp
DbContext context = new();
await context.TableName1.Include(tb1 => tb1.TableName2).AsNoTrackingWithIdentityResolution()ToListAsync();
```

<h4>UseQueryTrackingBehavior</h4>
Context üzerinden gelen verilerin takibinde ChangeTracker mekanizmasının davranışını değiştirmemizi sağlayan konfigürasyon metodudur.
OptionsBuilder ile bu metod kullanılabilmektedir.

```csharp
// Takip edilmemesine gore ayarlama...
optionsBuilder.UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```
<h4>AsTracking</h4>
Özünde ChangeTrackor mekanizması davranışı sergilemektedir. NoTracking durumunda değişiklik yapılarak nesnelerin takibi tekrar aktif edilebilmektedir.

```csharp
// Takip edilmemesine gore ayarlama...
await context.TableName.AsTracking().ToListAsync();
```
<br>

##### İLİŞKİ YAPILANMASI
**Principal Entity**
Kendi başına var olan tabloyu modelleyen entity'dir.
**Dependent Entity**
Bir başka tabloya ilişkisel olarak bağımlı entity'dir.
**Foreign Key**
Principal Entity ile Dependent Entity arasındaki ilişkiyi sağlar. Dependent Entity içerisinde tanımlanır.
**Principal Key**
Principal Entity içerisinde tanımlanır.

**Navigation Property**
İlişkisel tablolar arasındaki fiziksel erişimi class içerisinde Navigation propertyleri sağlamaktadır.

</br>

<h4>Birebir İlişki Türü</h4>
Entity sınıflarında ilişkisel türde olacak veriler Navigation Propertyler ile sağlanmaktadır.
<br></br>

**Araç Ve Plaka Örneği;**
```csharp
//Principal Entity
class Arac
{
    public int AracId { get; set; }
    public string AracModel { get; set; }
    //Navigator Property
    public AracPlaka AracPlaka { get; set; }
}

// Dependent Entity
class AracPlaka
{
    public int AracPlakaId { get; set; }
    public string AracPlakaNumarasi{ get; set; }
    //Navigator Property
    public Arac Arac { get; set; }
}
```
<br>

**Default Convention** olarak kullanımında bağımlı olacak entityi Foreign Key karşılık property tanımlanarak belirtilebilmektedir.
Migrate neticesinde AracPlaka entitysi içerisinde tanımlanmış AracId foreign key olarak oluştulmakta ve Arac entitysinin AracId kolonuyla eşleştirilmektedir.

```csharp
// Dependent Entity
class AracPlaka
{
    public int AracPlakaId { get; set; }
    //ForeignKey
    public int AracId { get; set; }
    public string AracPlakaNumarasi { get; set; }
    //Navigator Property
    public Arac Arac { get; set; }
}
```
<br>

**Data Annotations;**
Data Annotations ile Foreign Key e karşılık gelecek property ismini değiştirebilmekte ve Dependent Entity Id'sini hem Primary Key hemde Foreign Key olarak ForeignKey Attribute'u ile tanımlanabilmektedir.

```csharp
// Dependent Entity
class AracPlaka
{
    [Key, ForeignKey(nameof(Arac))]
    public int AracPlakaId { get; set; }
    public string AracPlakaNumarasi { get; set; }
    //Navigator Property
    public Arac Arac { get; set; }
}
```
<br>

**Fluent Api**
Fluent Api Context sınıfı içerisinde OnModelCreating Metodunun override edilmesi ile konfigürasyonel olarak kullabilmektedir. 
```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<AracPlaka>()
        .HasKey(a => a.AracPlakaId);

    modelBuilder.Entity<Arac>()
        .HasOne(a => a.AracPlaka)
        .WithOne(ap => ap.Arac)
        .HasForeignKey<AracPlaka>(ap => ap.AracPlakaId);
}
```


<h4>Bireçok İlişki Türü</h4>
Bireçok ilişki durumunda foreign key tanımlamaya gerek duyulmamaktadır.
Navigation Propertyler ile ilişki sağlanmaktadır.
Default olarak arka planda KitapEntity içerisinde YazarId'ye karşılık kolon oluşturmaktadır.
<br></br>

**Ürün Ve Marka Örneği;**
```csharp
//Dependent Entity
class Urun
{
    public int UrunId { get; set; }
    public string UrunAdi { get; set; }
    //Navigator Property
    public Marka Marka { get; set; }
}

// Principal Entity
class Marka
{
    public int MarkaId { get; set; }
    public string MarkaAdi { get; set; }
    //Navigator Property
    public ICollection<Urun> Urunler { get; set; }
}
```

<h4>Çoka Çok İlişki Türü</h4>
İki entity arasında ilişkinin sağlanması için navigation propertyler üzerinden bir CrossTable gerekmektedir.
EF Core CrossTable entitysini kendisi oluşturmaktadır.
KitapYazar veya YazarKitap olarak bir entity tanımlayacak ve composite olarak Primary Key tanımlayacaktır.

**Kitap Ve Yazar Örneği;**
```csharp
class Kitap
{
    public int KitapId { get; set; }
    public string KitapAdi { get; set; }
    //Navigator Property
    public ICollection<Yazar> Yazarlar { get; set; }
}

class Yazar
{
    public int YazarId { get; set; }
    public string YazarAdi { get; set; }
    //Navigator Property
    public ICollection<Kitap> Kitaplar { get; set; }
}
```
<br>

##### İLİŞKİSEL VERILERDE TANIMLAMALAR


1-1 ilişkiye sahip Entityler 
<br>

Kullanım;
```csharp

class PrincipalEntity
{
    public int Id { get; set; }
    public string Pe { get; set; }
    public DependentEntity DependentEntity{ get; set; }
}

class DependentEntity
{
    public int Id { get; set; }
    public string De { get; set; }
    public PrincipalEntity PrincipalEntity{ get; set; }
}
```
<br>
1-N ilişkiye sahip Entityler arasındaki yapılanmada Principal Entity'nin constructor() sınıfında HashSet<T> koleksiyonu ile nesne oluşturulmakta ve bu nesne Navigation Property'e verilmektedir. Nesne üretilirken koleksiyonun null referans exception hatası vermemesi için verilmektedir.
<br>

Kullanım;
```csharp
class PrincipalEntity
{
    public PrincipalEntity()
    {
        DependentEntities = new HashSet<DependentEntity>();
    }
    public int Id { get; set; }
    public string Pe { get; set; }
    public ICollection<DependentEntity> DependentEntities{ get; set; }
}

class DependentEntity
{
    public int Id { get; set; }
    public string De { get; set; }
    public PrincipalEntity PrincipalEntity{ get; set; }
}
```
<br>

N-N ilişkiye sahip Entityler üzerinden veri eklemede hem CrossTable Entitysi oluşturulup hemde oluşturulmadan kullanılabilmektedir.
Oluşturup kullanıldığında class nesnesine erişebilmeyi ve erişip manevratik işlemler yapabilmeyi sağlamaktadır.

CrossTable ile kullanım;

```csharp
class PrincipalEntity
{
    public PrincipalEntity()
    {
        DependentEntities = new HashSet<DependentEntity>();
    }
    public int Id { get; set; }
    public string Pe { get; set; }
    public ICollection<PrincipalDepdendent> DependentEntities{ get; set; }
}

class PrincipalDepdendent
{
    public int PrincipalEntityId { get; set; }
    public int DependentEntityId { get; set; }
    public PrincipalEntity PrincipalEntity { get; set; }
    public DependentEntity DependentEntity { get; set; }
}

class DependentEntity
{
    public DependentEntity()
    {
        PrincipalEntities = new HashSet<PrincipalDepdendent>();
    }
    public int Id { get; set; }
    public string De { get; set; }
    public ICollection<PrincipalDepdendent> PrincipalEntities{ get; set; }
}
```
<br><br>

İlişkisel tablolarda silme işlemi durumunda;
3 Farklı davranış uygulanabilmektedir.

* **1- Cascade**
Principal Entity tablosundan silinen veriyle Denepdent Entity bağımlı tablosunda bulunan ilişkili verilerin silinmesini sağlar.

* **2- SetNull**
Principal Entity tablosundan silinen veriyle bağımlı tablodaki ilişkisel verilere NULL değerin atanmasını sağlar.
1-1 ilişki durumunda setnull kullanılmamaktadır. Primary KEY null olamayacağı için...

* **3- Restrict**
Principal Entity tablosundan bir veri silinmeye çalışıldığında o veriye karşılık bağımlı tablodaki verilerin silinme işlemini engeller.
<br>

<h4>Backing Fields</h4>
Entity içerisinde tanımlanan propertyler yerine field'ların kullanımlasını sağlar.
Bu davranış ile field üzerinedn bir kapsulleme işlemi sağlanabilir.<br>
<br>

Kullanım;
```csharp
class Entity
{
    public int Id { get; set; }
    public string name;
    public string Name { get => name; set=> name = value; }
}
```
<br>

Verilerin sadece field üzerinden kullanılması için **BackingField Attribute** ile tanımlana yapılabilir.


```csharp
class Entity
{
    public int Id { get; set; }
    public string name;
    [BackingField(nameof(name))]
    public string Name { get; set; }
}
```
<br>

Fluent api ile de backing field bildirimi gerçekleştirilebilir. **HasField()** metodu ile field bildirimi sağlanır.

```csharp
modelBuilder.Entity<EntityName>()
    .Property(en => en.Name)
    .HasField(nameof(EntityName.name));
```
<br>

##### Field Only Properties
Entitylerde gelecek olan değerlerin property yerine metotlar ile karşılanması veya belirli propertylerin gerektiği durumlarda gösterilmemesi için kullanılabilir.


<h4>Shadow Property</h4>
Fiziksel olarak entity sınıfları içerisinde tanımlanmayan, entity instance'ında bulunmayacak ancak arka planda oluşturulan propertylerdir. Entity içerisinde tanımlanmayan ilişkiyi sağlayacak foreign key propertysi arka planda oluşturulmaktadır.<br>
<br>

Fluent API da Shadow Property oluşturma;
```csharp
modelBuilder.Entity<EntityName>()
    .Property<PropertyType>("MyShadowProperty");
```
<br>

Change Tracker ile Shadow Property'ye erişim;
```csharp
// Entry metodu ile propertyname i verilerek erişim
context.Entry(EntityName).Property("MyShadowProperty");
```
<br>

EF.Property Static yapısı ile Shadow Property'ye erişim;
```csharp
// LINQ sorgularında erişim.
context.EntityName.OrderBy(e => EF.Property<PropertyType>(e, "MyShadowProperty"));
```
<br>

##### ENTITY CONFIGURATIONS
Uygulamada EF-CORE ile default çalışan yapıların davranışları özelleştirilebilir.
Konfigürasyonel ayarlar Context sınıfındaki OnModelCreating metodunda gerçekleştirilebilir.
<br>

* **Table Attribute**
Tablo isimleri default olarak DBSet property isimlerinden alınır. Table attribute u ile yapılandırılabilir.

```csharp
[Table("Kisiler")]
class Employee{}
```
<br>

* **ToTable Method**
Fluent API da modelBuilder ile de yapılandırma gerçekleştirilebilir.

```csharp
modelBuilder.Entity<Employee>()
        .ToTable("Kisiler");
```
<br>

* **Column Attribute**
Tabloların kolonları entity sınıflarında propertylere karşılık gelmekte ve default olarak kolon adları, tipleri property isimlerinden ve tiplerinden alınır.

```csharp
class Employee
{
    [Column("Isim", TypeName = "")]
    public string Name [ get; set; ]
}
```
<br>

* **Column Methods**
Fluent API da modelBuilder ile Property üzerinden yapılandırma gerçekleştirilebilir.

```csharp
modelBuilder.Entity<Employee>()
        .Property(e => e.Name)
        .HasColumnName("Isim")
        .HasColumnType("");
```
<br>

* **ForeignKey Attribute**
İlişkisel tablolarda esas tablodaki verilerin tutulacağı kolon bağımlı entity içerisinde foreign key olarak tanımlanmakta.
Tanımlanma olmadığında EF-CORE default olarak shadow propety davranışı sergiler.

```csharp
class Employee
{
    [ForeignKey(nameOf(Department))]
    public int DepartmentId [ get; set; ]
    public Department Department { get; set; }
}
```
<br>

* **HasForeignKey Method**
ModelBuilder da öncelikle ilişki türü yapılandırılmalı sonra foreignkey tanımlanmalıdır.

```csharp
modelBuilder.Entity<Employee>()
        .HasOne(e => e.Department)
        .WithMany(d => d.Employee)
        .HasForeignKey(e => e.DepartmentId);
```
<br>

* **NotMapped Attribute**
Entity içerisindeki propertlyerin tümü default olarak tablo kolonlarına modellenir.
Bazı durumlarda kolona modellenmemesi gereken bir property olduğu zaman NotMapped attribute'u ile işaretlenebilir.

```csharp
class Employee
{
    public int Id { get; set; }
    [NotMapped]
    public string MyPropety { get; set; }
}
```
<br>

* **Ignore Method**
Fluent API'da modelBuilder üzerinden ignore metodu ile de aynı işlem gerçekleştirilebilir.

```csharp
modelBuilder.Entity<Employee>()
        .Ignore(e => e.MyPropety);
```
<br>

* **Key Attribute**
EF-CORE default olarak id, EntityId, ID, EntityID propertyName'lerini PK olarak algılar ve constraint uygular.
Key attribute u ile istenilen property'e primary key tanımlanabilir.

```csharp
class Employee
{
    [Key]
    public int EmployeeNo { get; set; }
}
```
<br>

* **HasKey Method**
Fluent API'da aynı işlem gerçekleştirilebilir.

```csharp
modelBuilder.Entity<Employee>()
        .HasKey(e=> e.EmployeeNo);
```
<br>

* **Required Attribute**
Bir kolonun nullable olmayacağını belirtir.
EF-CORE Default olarak not null davranışı sergiler. ? operatörü ile nullable davranış sergilenebilir.

```csharp
class Employee
{
    [Required]
    public string Email { get; set; }
}
```
<br>


* **IsRequired Method**
Fluent API'da aynı işlem gerçekleştirilebilir.

```csharp
modelBuilder.Entity<Employee>()
        .Property(e => e.Email).isRequired();
```
<br>

* **Length Attributes**
Entity Propertylerinde sınırlama getirilebilmektedir.

```csharp
class Employee
{
    [MaxLength(20)]
    public string Name { get; set; }
    [StringLength(20)]
    public string Surname { get; set; }
}
```
<br>

* **HasMaxLength Method**
modelBuilder da aynı işlem gerçekleştirilebilir.

```csharp
modelBuilder.Entity<Employee>()
        .Property(e => e.Name).HasMaxLength(20);
```
<br>

* **Precision Attribute**
Küsüratlı sayılarda belirli bir hane belirtmek istenildiğinde kullanılabilmektedir.

```csharp
class Employee
{
    [Presicion(5,3)]
    public decimal Salary { get; set; }
}
```
<br>


* **HasPrecision Method**
```csharp
modelBuilder.Entity<Employee>()
        .Property(e => e.Salary).HasPrecision(5,3);
```
<br>

* **Unicode Attribute**
Kolon içerisinde unicode karakterler kullanılacaksa bu yapılandırma uygulanır.

```csharp
class Employee
{
    [Unicode]
    public string Address { get; set; }
}
```
<br>

* **IsUnicode Method**

```csharp
modelBuilder.Entity<Employee>()
        .Property(e => e.Address).IsUniCode();
```
<br>

* **Comment Attribute**
Veritabanı nesneleri üzerinde bir açıklama eklemek için Comment kullanılabilir.

```csharp
class Employee
{
    [Comment("Açıklama..")]
    public string MyP { get; set; }
}
```
<br>

* **HasComment Method**

```csharp
modelBuilder.Entity<Employee>()
        .HasComment("Tablo Açıklama...");
        .Property(e => e.MyP)
        .HasComment("Kolon açıklama...")
```
<br>


* **CompositeKey**
Birden fazla kolonu HasKey methodu ile primary key olarak tanımlanmasını sağlar.

```csharp
modelBuilder.Entity<Employee>()
        .HasKey(e => new {e.Id, e.Id2});

```
<br>

* **HasDefaultSchema**
Veritabanındaki default schema yapılandırılabilir.

```csharp
modelBuilder.HasDefaultSchema("t");

```
<br>

* **HasDefaultValue**
Tabloda bir kolon a değer gönderilmediği durumda default olarak değer atama.

```csharp
modelBuilder.Entity<Employee>()
        .Propety(e => e.Name)
        .HasDefaultValue("")
```
<br>

* **HasDefaultValueSql**
Tabloda bir kolon a değer gönderilmediği durumda bir sql komutundan default olarak değer alır.

```csharp
modelBuilder.Entity<Employee>()
        .Propety(e => e.Date)
        .HasDefaultValueSql("GETDATE()");
```
<br>

* **HasComputedColumnSql**
Tablolarda birden fazla kolon arasında verilere işlem uygulayarak ayrı kolonda tutulmasını sağlar.

```csharp
class Test
{
    public int ValueA { get; set; }
    public int ValueB { get; set; }
    public int ComputedValue { get; set; }
}

modelBuilder.Entity<Employee>()
        .Propety(t => t.ComputedValue)
        .HasComputedColumnySql("[ValueA] * [ValueB]");
```
<br>

* **HasConstraintName**
Bağımlı entity içerisinde tanımlanan foreign key'e constraint uygulayarak name'i değiştirilebilir.

```csharp
modelBuilder.Entity<Employee>()
        .HasOne(e => e.Department)
        .WithMany(d => d.Employee)
        .HasForeignKey(e => e.DepartmentId)
        .HasConstraintName("Department");
```
<br>

* **HasData**
Migration yapılanmasında veritabanına hazır veriler(**SeedData**) gönderilmesi istenirse HasData method u kullanılabilir.

!!![] Manuel olarak entity nesnelerinin id değerleri girilmelidir.

```csharp
modelBuilder.Entity<Employee>()
        .HasData(new Employee
        {
            Id = 1,
            Name = "E1",
            Surname = "E2"
        });
```
<br>

* **HasField**
Verilerin sadece field üzerinden kullanılması için **BackingField** ile tanımlana yapılabilir.

```csharp
modelBuilder.Entity<Employee>()
    .Property(e => e.Name)
    .HasField(nameof(Employee.name));
```
<br>

* **HasField**
Entityde bir primary key kolonu olmayacaksa bunun yapılandırılması gerekir.

```csharp
modelBuilder.Entity<Employee>()
        .HasNoKey();
```
<br>

* **HasQueryFilter**
Bir entitye karşılık olarak uygulama bazında global bir Query Filter tanımlamaktır.
Elde edilecek veriler tanımlanan sorgu ile default gelecektir.

```csharp
modelBuilder.Entity<Employee>()
        .HasQueryFilter(e => e.Active == true);
```
<br>


**IEntityTypeConfiguration**
Entity'lerde yapılacak olan konfigürasyonları o entity'e özel olarak ayrı bir dosyada yapmayı sağlayan bir arayüzdür.
Merkezi bir yapılandırma noktası sağlar.
Entity konfigürasyonlarının yönetilebilirliğini-okunabilirliğini sağlayacaktır.

Entity konfigürasyonları yapılacak class **IEntityTypeConfiguration<*T>** arayüzünden implement edilir.
**EntityTypeBuilder** ile gelen builder parametresi üzerinden konfigürasyonel çalışmalar yapılabilmektedir.

```csharp
class EmployeeConfiguration : IEntityTypeConfiguration<Employee>
{
    public void Configure(EntityTypeBuilder<Employee> builder)
    {
        builder.Property();,
        builder.Haskey();
        //...
    }
}
```
<br>

Konfigürasyonel sınıflar Context sınıfı içerisindeki OnModelCreating de **ApplyConfiguration** metodu ile bildirilir.

```csharp
modelBuilder.ApplyConfiguration(new EmployeeConfiguration());
```
<br>

**ApplyConfigurationsFromAssembly** metodu ile tek tek bildirmek yerine bulunduğu Assembly seviyesinde bildirilerek tek seferlik işlemde gerçekleştirilebilir.

```csharp
modelBuilder.ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());
```
<br>
<br>


<h3>SEED DATA</h3>

EF Core ile migrate edilecek veritabanı için hazır veriler oluşturmayı sağlar.
Migration işleminde bu veriler hedef tablolarına gönderilirler.
<br>

* Test için geçici verilere ihtiyaç için kullanılabilir.
* Yazılım için temel konfigürasyonel değerler varsa kullanılabilir.

Seed Datalar, OnModelCreating metodu içerisinde HasData fonksiyonu ile eklenir.
Primary Key değerleri manuel olarak eklemek zorunludur.

Örnek kullanım;

```csharp
modelBuilder.Entity<Employee>()
    .HasData(
        new Employee(){
            Id = 1, 
            Name = "Employee1"
        },
         new Employee(){
            Id = 2, 
            Name = "Employee2"
        }
    );
```

<br><br>

<h3>TABLE PER HIERARCHY PATTERN </h3>

Kalıtımsal hiyerarşideki tüm entityler için TPH davranışı ile tek bir tablo oluşturulur ve bu tablodaki entity kolonları **DISCRIMINATOR** kolonu ile ayırt edilir.

EF Core default olarak TPH davranışı sergiler.

Kalıtımsal durum;
```csharp
abstract class Person
{
    public int Id { get; set; }
    public string? Name { get; set; }
    public string? Surname { get; set; }
}
class Employee : Person
{
    public string? Department { get; set; }
}

class Customer : Person
{
    public string? Address { get; set; }
}

```
<br>

Discriminator kolonu modelbuilder'da HasDiscriminator ve HasValue fonksiyonları ile özelleştirilebilir. Default olarak tabloda verileri entity isimlerinden alır.


Discriminator özelleştirme;
```csharp
modelBuilder.Entity<Person>()
    .HasDiscriminator<string>("MyDisc")
    .HasValue<Customer>('C')
    .HasValue<Employee>('E');
```
<br>

EF Core arka planda hangi davranış kullanılıyorsa ona uygun modellemeyi kendisi sağlamaktadır.

Veri ekleme-silme-güncelleme normal şekilde gerçekleştirilir.

Kalıtımsal ilişkiye sahip entity için veri sorgulama yapılıyorsa, üst sınıfa yapılan bir sorgulamada kalıtım verdiği alt sınıf verileride kapsanmaktadır.

<br><br>

<h3>TABLE PER TYPE PATTERN </h3>

Entitylerin aralarında kalıtımsal ilişkiye sahip olduğu durumlarda her bir entity için tablo oluşturulur.
Bu tablolar arasında 1-1 bir ilişki vardır.

Modelbuilder ile ToTable fonksiyonu kullanılarak TPT davranışı sergilenebilmektedir.

```csharp
modelBuilder.Entity<Person>()
    .ToTable("Persons");
modelBuilder.Entity<Employee>()
    .ToTable("Employees");
modelBuilder.Entity<Customer>()
    .ToTable("Customers");
```
<br>

TPH ye göre daha yavaşdır.

<br><br>

<h3>TABLE PER CONCRETE PATTERN </h3>

Entitylerin aralarında kalıtımsal ilişkiye sahip durumlarda sadece concrete entitylerin tabloları oluşturulur.

TPT ye göre daha performanslıdır.
1-1 ilişki yoktur.
Modelbuilder daki UseTpcMappingStrategy fonksiyonu ile kullanılır.

Kullanım;

```csharp
modelBuilder.Entity<Person>()
    .UseTpcMappingStrategy();
```


<br><br>

<h3>INDEXES</h3>

Indexleme davranışı, bir veya birden fazla column'a dayalı sorgularda daha fazla verim ve performans sağlar. Indexlenen column'lar Index tablosunda tutulurlar.

**Primary Key - Foreign Key - Alternate Key** default olarak indexlenirler.

<br>

**Indexleme Davranışları**;
* Index attribute:
```csharp
[Index(nameof(Name))]
class Person
{
    public int Id { get; set; }
    public string? Name { get; set; }
    public string? Surname { get; set; }
    public int? Age { get; set; }
}
```
<br>

* **Has Index metodu**:
```csharp
modelBuilder.Entity<Person>()
    .HasIndex(p => p.Name);
```
<br>

* **Composite Index**:
```csharp
[Index(nameof(Name), nameof(Surname))]
class Person{}

//Fluent Api'da
modelBuilder.Entity<Person>()
    .HasIndex(p => new{ p.Name, p.Surname});
```
<br>

* **IsUnique** ile unique yapma:
```csharp
[Index(nameof(Name), IsUnique = true)]
class Person{}

//Fluent Api'da
modelBuilder.Entity<Person>()
    .HasIndex(p => p.Name)
    .IsUnique();
```
<br>

* **AllDescending**: Tüm indexlemelerde sıralama davranış gösterir. Default Descending'dir. Ascending davranışa göre ayarlanabilir.
```csharp
[Index(nameof(Name), AllDescending = false)]
class Person{}
```
<br>

* **IsDescending**: Column'a göre sıralama davranışı gösterir.

```csharp
[Index(nameof(Name), IsDescending = false)]
class Person{}
```
<br>

* **HasDatabaseName** ile index ismi özelleştirilebilir:
```csharp
[Index(nameof(Name), Name = "MyNameIndex")]
class Person{}

// Fluent Api
modelBuilder.Entity<Person>()
    .HasIndex(p => p.Name)
    .HasDatabaseName("MyNameIndex");
```
<br>

* **Filter** uygulama (Null olmayan verilere göre indexleme):
```csharp
// Fluent Api
modelBuilder.Entity<Person>()
    .HasIndex(p => p.Name)
    .HasFilter("[NAME] IS NOT NULL");
```
<br>

* **Include Properties Method**: Indexlenen column'lar dışında elde edilecek veride başka bir column'unda index tablosuna bildiriminde bulunulabilir.
```csharp
// Fluent Api
modelBuilder.Entity<Person>()
    .HasIndex(p => new{ p.Name, p.Surname})
    .IncludeProperties(p => p.Age);

```

<br><br>

<h3>SEQUENCES</h3>

Secuence, veritabanında benzersiz ve ardışık şekilde sayısal değerler üreten bir nesnedir.
Birden fazla tablo tarafından kullanılabilirdir.
Veritabanına göre çalışmalar yapmak gerekmektedir.
Kullanıldığı durumlarda identity kolonu pasifleştirilmiş olur.

Kullanım;

* HasSequence method.
```csharp
modelBuilder.HasSequence("MySequence")
```

* Tablo kolonuna HasDefaultValueSql metodu ile değer verme;
```csharp
modelBuilder.Entity<Person>()
    .Property(p => p.Id)
    .HasDefaultValueSql("NEXT VALUE FOR MySequence")
```

* Özelleştirmede bulunulabilir;
```csharp
// first 10 - increment 10 - 10 - 10 => 10 - 20 -30
modelBuilder.HasSequence("MySequence")
    .StartsAt(10)
    .IncrementBy(10)
```

<br>

Identity ile arasındaki fark, sequence bir veritabanı nesnesidir ve değeri diskten değil RAM'den alır. Böylece identity'e göre daha hızlı bir performans gösterebilir.

<br><br>

<h3>LOADING RELATED DATA</h3>

<br>

<h5>EAGER LOADING</h5>

Eager Loading: Sorgulama süreçlerinde sorguya ilişkisel verilerin de eklenmesini iradeli bir şekilde sağlar.

* **Include method**: İlişkisel tabloları sorguya dahil etmeyi sağlar. Aralarındaki navigation property tekil ise bu durum sağlanır.

<br>

* **ThenInclude method**: Include edilen tabloların ilişkili olduğu diğer tablolarda sorgu ekleyebilmek için kullanılır. Navigation property'nin koleksiyonel olması durumunda Include yerine kullanılır.

<br>

* **Filtered Include**: Sorgulama süreçlerinde include edilirken sonuçlar üzerinde filtreleme - sıralama yapabilmeyi sağlar.
(Where - OrderBy - OrderByDescending - ThenBy - ThenByDescending - Skip - Take fonksiyonlarında kullanılabilmektedir).

<br>

!!![] Önceden execute edilmiş/sorgulanmış ve change tracker ile takip edilmiş veriler/belleğe konulmuş veriler var ise filtreleme sonucu veriler bellekten alınır.

<br>

* AutoInclude method: Bu method ile sürekli kullanılacak bir sorguda include edilen bir tablo varsa merkezi bir hale getirmeyi sağlar.
```csharp
modelBuilder.Entity<Person>()
    .Navigation(p => p.Region)
    .AutoInclude();
```

<br>

* Kalıtımsal durumlara sahip tablolar üzerinde sorgulamada Include edilecek tablo CAST veya AS operatörü kullanılmalıdır.

<br><br>

<h5>EXPLICIT LOADING</h5>
Oluşturulan sorgularda şarta göre include işlemleri yapmayı sağlar.

* Reference method: Sorguya eklenmek istenilen tablonun navigation propertysinin tekil türde olması gerekir. Tekil ise bu method kullanılarak join işlemi yapılır.

```csharp
var persons = await context.Persons.ToListAsync();

if(...)
    await context.Entry(persons).Reference(p => p.Region).LoadAsync();
```

<br>

* Collection method: Sorguya eklenmek istenilen tablonun navigation propertysinin çoğul türde olması gerekir. Çoğul ise bu method kullanılarak join işlemi yapılır.

```csharp
var persons = await context.Persons.ToListAsync();

if(...)
    await context.Entry(persons).Collection(p => p.Orders).LoadAsync();
```

<br><br>


<h5>LAZY LOADING</h5>

Sorguda elde edilen veriler üzerinden ilişkisel verileri kullanmak istenildiğinde sorgu arka planda oluşturularak execute edilir(Lazy Davranış ile).

<br>

**Proxy** yapısı ile Lazy Loading davranışı sergilenebilir.

Kullanmak için: **Microsoft.EntityFrameworkCore.Proxies** kütüphanesi yüklenmelidir.
Ayrıca optinosBuilder üzerinden **UseLazyLoadingProxies** methodu çağrılmalıdır.
Kullanılacak entitylerde Navigation Propertyler virtual ile işaretlenmiş olmalıdır.

```csharp
class Person
{
    public int Id { get; set; }
    public string? Name { get; set; }
    public string? Surname { get; set; }
    public virtual List<Order> Orders { get; set; }
    public virtual Region Region { get; set; }
}
```

Navigation Propertyler tetiklendikçe sorgu neticesinde veriler belleğe yüklenecektir.

!!![a] LazyLoading kullanırken N+1 problemi yaşanabilmektedir. O yüzden kullanım açısından maliyetli ve performansı düşürücü olabilmektedir. Navigation propertyler döngüsel durumlarda n+1 kadar sorguyu tekrar tetikleyecektir.
