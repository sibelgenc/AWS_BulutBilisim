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

1. AWS RDS (SQL Server) Veritabanı Kurulumu
 
Sürece ilk olarak veritabanı katmanını hazırlayarak başladım. AWS RDS üzerinden bir SQL Server Express örneği oluşturdum.
İşlem: Veritabanının dış dünyadan ve uygulamadan erişilebilir olması için Publicly accessible ayarını aktif ettim.


<img width="880" height="798" alt="Ekran görüntüsü 2025-12-27 115249" src="https://github.com/user-attachments/assets/aa16afdb-b90d-4bce-bea4-aedbcc88c9ae" />


<img width="858" height="795" alt="Ekran görüntüsü 2025-12-27 115340" src="https://github.com/user-attachments/assets/1187ce57-c654-4e11-8c65-14345796c443" />


<img width="1510" height="644" alt="Ekran görüntüsü 2025-12-27 115858" src="https://github.com/user-attachments/assets/47c2291e-0af6-4656-9c27-fa7109219730" />


Bu görsellerde gördüğümüz gibi öncelikle veritabanı yapılandırıyorum.İsmini database-1 yapıyorum ve işlemleri yaptıktan sonra veritabanımız oluşturuluyor.


<img width="1855" height="827" alt="Ekran görüntüsü 2025-12-28 112150" src="https://github.com/user-attachments/assets/7f41c127-9634-4191-ad53-64262459f332" />


Burda da oluşturduğum database'in ayarları gözüküyor.Endpoint, bizim sunucuya yükleyeceğimiz sitenin appsettings.json dosyasına yazılacak adresi temsil ediyor. 


2. Ağ Güvenliği ve Port Ayarları (Security Groups)
   
Veritabanına bağlantı sağlanabilmesi için AWS Security Group üzerinden gerekli izinleri tanımladım.
İşlem: Inbound Rules (Gelen Kurallar) kısmına giderek MSSQL (1433) portunu tüm IP adreslerine (0.0.0.0/0) açtım.


<img width="868" height="794" alt="Ekran görüntüsü 2025-12-27 144023" src="https://github.com/user-attachments/assets/66dae8ef-37f5-45e3-a336-bef485d51980" />


3. Uygulama Konfigürasyonu (Connection String)
   
Uygulamanın canlıdaki veritabanını tanıması için appsettings.json dosyasını güncelledim.
İşlem: RDS sayfasından aldığım Endpoint adresini, kullanıcı adı ve şifremle birleştirerek bağlantı dizesini oluşturdum.


4. Elastic Beanstalk ile Dağıtım (Deployment)
Uygulamayı web sunucusuna yüklemek için Elastic Beanstalk servisini kullandım.
Platform Seçimi: .NET on Linux platformunu tercih ederek hazırladığım ZIP paketini yükledim.
Karşılaşılan Sorun: İlk denemede ortam "Pending Creation" durumunda takıldı.
Çözüm: AWS CloudFormation paneline giderek kilitlenen "stack" yapısını manuel olarak temizledim (Delete) ve kurulumu başarıyla yeniden başlattım.

<img width="1842" height="820" alt="Ekran görüntüsü 2025-12-27 130615" src="https://github.com/user-attachments/assets/d17c1e69-b841-4547-9b67-509633e2230a" />
Oluşturacağımız ortamı yapılandırıyorum.

<img width="1402" height="806" alt="Ekran görüntüsü 2025-12-27 130644" src="https://github.com/user-attachments/assets/5d45a55c-829c-4268-a4bd-4a6d076288d0" />

Burda kodunuzu yükleyin yapıyorum ve yükleyeceğim web sitesinin ziplediğim dosyasını seçiyorum.

<img width="1855" height="793" alt="Ekran görüntüsü 2025-12-27 133137" src="https://github.com/user-attachments/assets/c24f3497-39e6-4b7f-97c2-a51594d5926e" />

Bulut sunucusu ayarlarını da yaptıktan sonra artık ortamımızı oluşturuyoruz.Oluştura tıkladıktan sonra belli bir süre ortamın oluşmasını bekliyoruz.Arkadaki işlemler biraz zaman alıyor.

<img width="1834" height="800" alt="Ekran görüntüsü 2025-12-27 154233" src="https://github.com/user-attachments/assets/390ae814-168a-4a78-b357-9a528a68dab7" />

Ve ardından ortamınız başarıyla başlatıldı yazısı geliyor.Buradaki etki alanındaki linke tıklayarak yüklediğimiz siteyi sorunsuz bir şekilde açabiliyoruz.


Otomasyon ve Kod Parçacıkları

Veritabanı bağlantısı için kullanılan kritik yapılandırma örneği:
"ConnectionStrings": {
    "DefaultConnection": "Server=database-1.cji6iswwmkdu.eu-north-1.rds.amazonaws.com;Database=database-1;User Id=sibel;Password=Password1234*****;TrustServerCertificate=True;"
}


Karşılaşılan Zorluklar ve Çözümler

Zorluk: Elastic Beanstalk ortamının "Pending Creation" aşamasında takılması.
Çözüm: CloudFormation üzerinden hatalı stack silinerek süreç temizlendi ve veritabanı ile uygulama katmanları birbirinden bağımsız (ayrı) kuruldu.

Zorluk: Veritabanı bağlantı hataları.
Çözüm: Inbound Rules (Gelen Kurallar) düzenlenerek 1433 portu erişime açıldı.

Öğrenilen Dersler ve İyileştirmeler

Bulut platformlarında veritabanı ve uygulama sunucularını ayrı yönetmenin (Decoupling) hata ayıklamayı kolaylaştırdığı görüldü.

İyileştirme: Gelecekte bağlantı bilgilerini appsettings.json yerine AWS Secrets Manager kullanarak daha güvenli saklamak hedeflenmektedir.
