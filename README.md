# AWS_BulutBilisim
Proje Açıklaması ve Hedefleri
Bu proje, bir .NET tabanlı satış sitesi uygulamasının AWS (Amazon Web Services) bulut platformuna taşınmasını, veritabanı yönetimini ve canlıya alınma sürecini kapsamaktadır. Temel hedef, ölçeklenebilir bir bulut altyapısı kullanarak uygulamanın her yerden erişilebilir olmasını sağlamaktır.

Kullanılan Teknolojiler
Yazılım Dili: C# / .NET

Veritabanı: AWS RDS (Microsoft SQL Server Express)

Sunucu Yönetimi: AWS Elastic Beanstalk

Altyapı: AWS VPC, Security Groups

İşletim Sistemi: Amazon Linux 2023

Uygulama Mimari Şeması
Uygulama, Ayrıştırılmış (Decoupled) Mimari kullanılarak kurulmuştur:

Web Katmanı: Elastic Beanstalk üzerinde koşan .NET uygulaması.

Veri Katmanı: Dışarıdan erişime açık, güvenliği optimize edilmiş AWS RDS SQL Server.

Adım Adım Uygulama Notları
RDS Veritabanı Kurulumu: AWS üzerinde SQL Server Express örneği oluşturuldu. Public Access ayarı yapılandırılarak dış erişim sağlandı.

Güvenlik Yapılandırması: Security Group ayarlarından 1433 portu (MSSQL) için giriş izni tanımlandı.

Uygulama Konfigürasyonu: appsettings.json dosyasındaki bağlantı dizesi (Connection String), RDS Endpoint adresiyle güncellendi.

Dağıtım (Deployment): Proje, Elastic Beanstalk üzerinden .NET on Linux platformuna ZIP paketi olarak yüklendi.

Otomatik Veri Tabanı Kurulumu (Seed Data): Uygulama ayağa kalktığında, kod içerisindeki SeedData mekanizması RDS üzerinde tabloları otomatik olarak oluşturdu.

Otomasyon ve Kod Parçacıkları
Veritabanı bağlantısı için kullanılan kritik yapılandırma örneği:
"ConnectionStrings": {
    "DefaultConnection": "Server=database-1.cji6iswwmkdu.eu-north-1.rds.amazonaws.com;Database=SalesDB;User Id=admin;Password=YourPassword;TrustServerCertificate=True;"
}

Karşılaşılan Zorluklar ve Çözümler
Zorluk: Elastic Beanstalk ortamının "Pending Creation" aşamasında takılması.

Çözüm: CloudFormation üzerinden hatalı stack silinerek süreç temizlendi ve veritabanı ile uygulama katmanları birbirinden bağımsız (ayrı) kuruldu.

Zorluk: Veritabanı bağlantı hataları.

Çözüm: Inbound Rules (Gelen Kurallar) düzenlenerek 1433 portu erişime açıldı.

Öğrenilen Dersler ve İyileştirmeler
Bulut platformlarında veritabanı ve uygulama sunucularını ayrı yönetmenin (Decoupling) hata ayıklamayı kolaylaştırdığı görüldü.

İyileştirme: Gelecekte bağlantı bilgilerini appsettings.json yerine AWS Secrets Manager kullanarak daha güvenli saklamak hedeflenmektedir.
