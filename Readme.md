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
```CSharp
DBContext context = new();
// bir Context nesnesi uretildi.
var employees = context.Employees.ToList();
```
Veriler object/instance olarak elde edilir.





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















