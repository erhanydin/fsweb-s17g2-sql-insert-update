# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

# Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

## Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]



# Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 



	1) ÖRNEK SORU: Yazar tablosunu KEMAL UYUMAZ isimli yazarı ekleyin.

		CEVAP:  insert into yazar(yazarad,yazarsoyad) values("Kemal","Uyumaz")
	

	
	2) Biyografi türünü tür tablosuna ekleyiniz.

		CEVAP: insert into tur(turadi) values ('Biyografi');
	
	
	3) 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri tek sorguda ekleyin. 

		CEVAP: insert into ogrenci (ograd, ogrsoyad, cinsiyet, sinif)
		values ('Çağlar', 'Üzümcü', 'E', '10A'),
		('Leyla', 'Alagöz', 'K', '9B'),
		('Ayşe', 'Bektaş', 'K', '11C');
	
	
	4) Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz.	
	
		CEVAP: insert into yazar (yazarad, yazarsoyad) 
			select ograd, ogrsoyad from ogrenci 
			order by rand() limit 1;
	
	5) Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.

		CEVAP: insert into yazar (yazarad, yazarsoyad) 
			select ograd, ogrsoyad from ogrenci
			where ogrno < 30 and ogrno > 10;
	
	
	6) Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
	(Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)
	
	
	7) 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.

		CEVAP: update ogrenci set sinif = '10C' where ogrno = 3;
	
	
	8) 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın

		CEVAP: update ogrenci set sinif = '10A' where sinif = '9A';
	
	
	9) Tüm öğrencilerin puanını 5 puan arttırın.

		CEVAP: update ogrenci set puan = puan + 5 where ogrno != 0;
	
	
	10) 25 numaralı yazarı silin.

		CEVAP: delete from yazar where yazarno = 25;


	11) Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)

		CEVAP: select * from ogrenci where dtarih is null;
	
	
	12) Doğum tarihi null olan öğrencileri silin.

		CEVAP: delete from ogrenci where dtarih is null; 
	
	
	13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.

		CEVAP: update kitap set puan = puan + 2 where kitapadi like 'a%';
	
	
	14) Kişisel Gelişim isimli bir tür oluşturun.

		CEVAP: insert into tur (turadi) values ('Kişisel Gelişim');
	
	
	15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.

		CEVAP: update kitap set turno = 9 where kitapadi = 'Başarı Rehberi';
	
	
	16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen "ogrencilistesi" adında bir prosedür oluşturun.

		CEVAP: DELIMITER &&
			create procedure ogrencilistesii()
			begin
			select * from ogrenci;
			end &&
			delimiter ;
	
	
	17) Öğrenci tablosuna yeni öğrenci eklemek için "ekle" adında bir prosedür oluşturun.

		CEVAP: DELIMITER &&
				create procedure ekle (
				in ad varchar(20), 
				in soyad varchar(20), 
				in gender varchar(20),
				in osinif varchar(20))
				begin
				insert into ogrenci (ograd, ogrsoyad, cinsiyet, sinif) 
				values (ad, soyad, gender, osinif);
				end && 
				delimiter ;
	
	
	18) Öğrenci noya göre öğrenci silebilmeyi sağlayan "sil" adında bir prosedür oluşturun.

		CEVAP: DELIMITER && 
				create procedure sil (in ogno int) 
				begin 
				delete from ogrenci where ogrno = ogno;
				end && 
				delimiter ;
	
	
	19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir prosedür oluşturun.

		CEVAP: DELIMITER &&
				create procedure sinifdegistir(in ono integer, in osinif varchar(20))
				begin
				update ogrenci set sinif=osinif where ogrno = ono;
				end &&

				delimiter ;
	
	
	20) Öğrenci adı ve soyadını "Ad Soyad" olarak birleştirip, ad soyada göre kolayca arama yapmayı sağlayan bir prosedür yazın.

		CEVAP: DELIMITER &&
				create procedure searchadsoyad(in adsoyadara varchar(50))
				begin
				select * from ogrenci 
				where concat(ograd, " " ,ogrsoyad) like concat("%", adsoyadara, "%");
				end &&
				delimiter ;
	
	
	21) Daha önceden oluşturduğunu tüm prosedürleri silin.
	
	
	#Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)
	22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.

		CEVAP: select * from kitap where turno = (select turno from tur where turadi = 'Dram');
	
	
	23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.

		CEVAP: select * from kitap where yazarno in (select yazarno from yazar where yazarad like 'e%');
	
	
	24) Kitap okumayan öğrencileri listeleyiniz.

		CEVAP: select * from ogrenci where ogrno not in (select ogrno from islem);
	
	
	25) Okunmayan kitapları listeleyiniz

		CEVAP: select * from kitap where kitapno not in (select kitapno from islem);

	
	26) Mayıs ayında okunmayan kitapları listeleyiniz.

		CEVAP: select * from kitap where kitapno not in (select kitapno from islem where atarih like '_____05%');
