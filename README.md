# ESP32 Chess Arena ♟️

**İki ESP32-S3, tek satranç tahtası.** ESP32-S3 N8R2 ve N16R8, kendi üzerlerinde çalışan satranç motorlarıyla birbirine karşı oynar. Windows'ta Python ile hazırlanan arayüz hamleleri gösterir, kuralları denetler ve maç kayıtlarını tutar.

> **Önemli:** Hamleleri bilgisayar hesaplamaz. PC, FEN pozisyonunu kartlara gönderir ve gelen hamleleri doğrulayıp görselleştirir.

## Özellikler

- İki bağımsız ESP32-S3 satranç motoru (N8R2 / N16R8)
- USB seri haberleşme ile canlı oyun
- Beyaz/siyah kart seçimi ve COM port seçimi
- Yasal hamle denetimi, maç sonucu ve alınan taş puanları
- PGN dışa aktarma
- İlk 20 yarım hamlede ağırlıklı açılış çeşitliliği (`GOX`)
- Ayarlanabilir düşünme süresi (varsayılan **3000 ms**)
- PC yanıt zaman aşımı (varsayılan **90000 ms**) ve gecikme uyarıları
- Dikey kaydırılabilir arayüz ve hata günlükleri

## Gereksinimler

**Donanım:** ESP32-S3 N8R2, ESP32-S3 N16R8, iki USB veri kablosu ve Windows bilgisayar.

**Yazılım:** Arduino IDE (ESP32 kart desteği), Python 3, `python-chess` ve `pyserial`. Uygulama `tkinter` kullanır.

## Kurulum

1. `firmware/ESP32_Chess_Arena/ESP32_Chess_Arena.ino` dosyasını Arduino IDE ile açın.
2. **Her iki** ESP32-S3 karta yükleyin. Her kart için doğru kart, flash ve PSRAM seçeneklerini seçin.
3. İki kartı USB ile bilgisayara bağlayın. **Arduino Seri Monitör'ü kapatın.**
4. Windows'ta `pc_app/baslat.bat` dosyasını çalıştırın. Alternatif olarak:

   ```powershell
   cd pc_app
   py -3 -m pip install -r requirements.txt
   py -3 chess_arena.py
   ```

5. Arayüzde iki COM portunu, beyaz oynayacak kartı ve düşünme süresini seçip maçı başlatın.

> İlk çalıştırmada `baslat.bat` gerekli Python paketlerini kurmak için internet bağlantısı kullanabilir.

## Seri port protokolü

Baud hızı: **115200**. Komutları satır sonu ile gönderin.

| PC → ESP32 | ESP32 → PC | Açıklama |
| --- | --- | --- |
| `PING` | `PONG` | Bağlantı testi |
| `TIME 3000` | `TIME_OK` | Motorun hedef düşünme süresi |
| `GO <FEN>` | `MOVE <uci>` veya `NOMOVE` | Pozisyondan hamle isteği |
| `GOX <ply> <FEN>` | `MOVE <uci>` veya `NOMOVE` | İlk 20 yarım hamlede açılış çeşitliliği |

Örnek:

```text
PING
PONG
TIME 3000
TIME_OK
GO rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1
MOVE b1c3
```

Motor, hamleyi üretirken arama derinliğiyle ilgili ek tanılama satırları yazabilir.

## Proje yapısı

```text
ESP32_Chess_Arena/
├── firmware/
│   └── ESP32_Chess_Arena/
│       └── ESP32_Chess_Arena.ino
├── pc_app/
│   ├── chess_arena.py
│   ├── requirements.txt
│   └── baslat.bat
├── docs/
│   └── TROUBLESHOOTING.md
├── .gitignore
├── README.md
└── README_TR.txt
```

## Bilinen sınırlamalar

- Bazı maçlarda kartlardan biri hamleye yanıt vermeyebilir; 90 saniyelik PC zaman aşımı motorun kilitlenmesini **çözmez**, yalnızca daha uzun bekler.
- Seri Monitör ile Python uygulaması aynı COM portunu aynı anda kullanamaz.
- N8R2 ile N16R8'in farklı bellek kapasiteleri, aynı motorun mutlaka farklı satranç gücünde olacağı anlamına gelmez.
- Donanım üzerinde uzun maç / tüm kart ayarları için otomatik doğrulama yapılmamıştır.

## Kaynak ve atıf

Firmware, **Sergey Urusov'un “ESP32 chess engine 1.0”** çalışmasını temel alır; orijinal kaynak kodunda yazar adı yer almaktadır. Satranç motorunun özgün kodu bu projenin tamamen sıfırdan yazılmış bir bileşeni olarak sunulmamalıdır.

**Lisans notu:** Özgün motorun yeniden dağıtım lisansı bu pakette doğrulanmış değildir. Repoyu herkese açık yayımlamadan önce özgün projenin lisansını ve kodu yeniden yayımlama iznini kontrol edin. Bu nedenle projeye otomatik olarak MIT/GPL lisansı eklenmedi.

## İçerik

World of Mechatronics — ESP32, gömülü sistemler ve maker projeleri.
