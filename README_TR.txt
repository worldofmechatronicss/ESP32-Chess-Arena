ESP32 CHESS ARENA V3 - DENGELI ACILIS CESITLILIGI
================================================

YENILIK: Ilk 20 YARIM HAMLEDE (beyaz+siyah toplam) motorun puanladigi
birbirine yakin adaylardan agirlikli secim yapilir. En iyi hamleye %60,
diger en fazla 2 uygun alternatife toplam %40 sans verilir.
Fark 60 motor skor birimini asarsa alternatif alinmaz. Yeterli guvenilir
aday yoksa en iyi hamle secilir. Bu bir acilis kitabi degildir.
Bilgisayar hamle SECMEZ, sadece FEN ve GOX komutunu gonderir.

KURULUM:
1. Her iki ESP32-S3'e bu surumun firmware/ESP32_Chess_Arena/
   ESP32_Chess_Arena.ino dosyasini Arduino IDE'den yukleyin.
   N8R2 ve N16R8 icin kartin dogru Flash/PSRAM ayarlarini kullanin.
2. Her iki karti USB ile baglayin, Serial Monitor'u kapatin.
3. pc_app/baslat.bat dosyasini acin.
4. COM portlarini ve beyaz karti secip maci baslatin.

PROTOKOL:
PING -> PONG
TIME 3000 -> TIME_OK
GO <FEN> -> MOVE <uci> (her zaman en iyi hamle)
GOX <ply> <FEN> -> MOVE <uci> (ply 0..19 arasinda dengeli cesitlilik)

NOTLAR:
- 20 yarim hamle = her taraf icin yaklasik 10 hamle.
- Puanlar motorun arama skorlaridir, tas puanlariyla ayni degildir.
- Motorun kisa surede aramayi kesmesi aday skorlarini etkileyebilir.
- Her macin farkli baslamasi garanti degildir; guvenilir alternatif yoksa
  motor en iyi hamleyi oynar.
- Fiziksel kartlarda derleme/oyun testi yapilmadi.


V4: Varsayilan motor dusunme suresi 3000 ms, PC yanit bekleme siniri 90000 ms.
10 saniyeden sonra her 10 saniyede uyari gorunur. Hata gunlukleri pc_app/logs klasorune yazilir.
Sadece pc_app klasorunu degistirmek yeterli; firmware V3 ile aynidir.
NOT: V3 uygulamasi zaten en az 40 saniye bekliyordu; V4 90 saniye bekler.
ESP32 kilitlenmesi veya motor hatasi bu degisiklikle otomatik duzelmez.


V5 - EKRAN KAYDIRMA
-------------------
Pencerenin sagina dikey kaydirma cubugu eklendi.
Fare tekerlegiyle asagi/yukari kaydirarak cihaz mesajlari ve alt panelleri gorebilirsiniz.
ESP32 firmware'i degismedi: yeniden Arduino kodu yuklemeniz gerekmez.
