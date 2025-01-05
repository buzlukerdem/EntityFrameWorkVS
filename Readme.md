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
* Modified: Nesne üzerinde değişiklik olduğunda Update işlemi yapacağı anlamına gelmektedir.
* Deleted: Nesne Remove metodu ile kaldırıldığında Delete işlemiyapacağı anlamına gelmektedir.

SaveChanges metodu DbcContext sınıfında virtual ile işaretlenmiş ve override edilebilir bir yapıdadır. Override edilerek şartlara göre kullanım amacına göre yapılandırılabilmektedir.

! ChangeTracker ile takip edilen veriler boyutuna göre belirli bir maliyete sebep olabilmektedir.
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