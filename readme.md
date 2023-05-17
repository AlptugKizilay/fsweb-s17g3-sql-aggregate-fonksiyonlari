# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]


##### Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 


MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
	#ilk 3 soruyu join kullanmadan yazın.
	1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	

	
	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	
select k.kitapadi, t.turadi from kitap k
left join tur t on t.turno = k.turno 
where t.turadi like 'hikaye%' or turadi like'fıkra%'
order by turadi

	
	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
select o.ogrno, o.ograd, o.ogrsoyad, o.sinif, k.kitapadi from ogrenci as o
left join islem as i on i.ogrno = o.ogrno 
left join kitap as k on k.kitapno = i.kitapno
where o.sinif like '10B%' or o.sinif like '10C%'
order by o.ogrno
	
	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
select o.ogrno, o.ograd, o.ogrsoyad, i.atarih from ogrenci as o
inner join islem as i on i.ogrno = o.ogrno 
order by i.atarih
	
	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
select k.kitapadi, t.turadi from kitap k
left join tur t on t.turno = k.turno 
where t.turadi like 'hikaye%' or turadi like'fıkra%'
order by turadi
	
	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
select o.ogrno, o.ograd, o.ogrsoyad, o.sinif, k.kitapadi from ogrenci as o
left join islem as i on i.ogrno = o.ogrno 
left join kitap as k on k.kitapno = i.kitapno
where o.sinif like '10B%' or o.sinif like '10C%'
order by o.ogrno
	
	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
select o.ogrno, o.ograd, o.ogrsoyad, i.atarih from ogrenci as o
inner join islem as i on i.ogrno = o.ogrno 
order by i.atarih	
	
	8) Kitap almayan öğrencileri listeleyin.
select o.ogrno, o.ograd, o.ogrsoyad from ogrenci as o
left join islem as i on i.ogrno = o.ogrno 
where i.kitapno is null	
	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.

select k.kitapno, k.kitapadi, count(i.kitapno) as 'Alınma Sayısı' from kitap k
	inner join islem i on i.kitapno = k.kitapno 
	group by k.kitapno	
	
	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

select k.kitapno, k.kitapadi, count(i.kitapno) as 'Alınma Sayısı' from kitap k
	left join islem i on i.kitapno = k.kitapno 
	group by k.kitapno

	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
select o.ogrno, o.ograd , o.ogrsoyad , k.kitapadi  from ogrenci o
	left join islem i on i.ogrno = o.ogrno 
	left join kitap k on k.kitapno = i.kitapno 
	order by o.ograd 
	
	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
select o.ogrno, o.ograd , o.ogrsoyad , k.kitapadi, y.yazarad , y.yazarsoyad  from ogrenci o
    left join islem i on i.ogrno = o.ogrno 
    left join kitap k on k.kitapno = i.kitapno 
    left join yazar y on y.yazarno = k.yazarno       
	
	
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
select o.ogrno, o.ograd ,o.sinif, o.ogrsoyad , count(k.kitapno) as 'Okunan Kitap Sayısı' from ogrenci o
join islem i on i.ogrno = o.ogrno 
join kitap k on k.kitapno = i.kitapno 
where o.sinif like '10B%' or o.sinif like '10c%' 
group by o.ograd 
order by o.sinif
	
	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG
select AVG(k.sayfasayisi) as 'Ortalama Sayfa Sayisi' from kitap k
	
	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
select k.kitapadi , k.sayfasayisi from kitap k
where k.sayfasayisi >  (SELECT AVG(sayfasayisi) from kitap)	
	
	16) Öğrenci tablosundaki öğrenci sayısını gösterin
select count(ogrno) from ogrenci	
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
select count(ogrno) as 'Öğrenci Sayisi' from ogrenci	
	
	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
select count(distinct ograd) as 'uniqe name' from ogrenci
	
	
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
	
	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
	
	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
	
	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
	
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
	
	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
	
	
	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

	
	26) Her sınıftaki öğrenci sayısını bulunuz.
	
	
	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
	
	
	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
	
	
	29) Her öğrencinin okuduğu kitap sayısını getiriniz.
