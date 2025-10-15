BAŞLA

// Sistem Başlatma
YAZ "Akıllı Ev Güvenlik Sistemi Başlatılıyor..."
sensörler ← [hırsızlık_sensörü, yangın_sensörü, gaz_sensörü, kapı_pencereler_sensörü]
alarm_durumu ← FALSE
kullanici_girisi ← FALSE

// Kullanıcı Girişi (Opsiyonel)
YAZ "Kullanıcı adı giriniz:"
OKU kullanici_adi
YAZ "Şifre giriniz:"
OKU sifre

EĞER kullanici_dogrula(kullanici_adi, sifre) İSE
    kullanici_girisi ← TRUE
    YAZ "Giriş başarılı."
DEĞİLSE
    YAZ "Giriş başarısız. Sistem izleme modunda çalışıyor."
BİTİŞ_EĞER

// Sürekli Sensör Kontrol Döngüsü
DÖNGÜ TRUE
    her sensör için sensörler
        sensör_durumu ← sensör_verisi_oku(sensör)

        EĞER sensör_durumu = TEHLİKE İSE
            alarm_durumu ← TRUE
            YAZ sensör + " tehlike tespit edildi!"
            alarm_sesini_çal()
            bildirim_gönder("Acil durum! " + sensör + " tehlikesi algılandı.")
            kayit_et(sensör, "tehlike", zaman())
        DEĞİLSE
            YAZ sensör + " durumu normal."
        BİTİŞ_EĞER
    BİTİŞ_DÖNGÜ

    EĞER alarm_durumu = TRUE İSE
        YAZ "Alarm aktif. Kullanıcı müdahalesi bekleniyor."
        YAZ "Alarmı kapatmak için şifre giriniz:"
        OKU girilen_sifre
        EĞER sifre_kontrol(girilen_sifre) = DOĞRU İSE
            alarm_durumu ← FALSE
            alarmı_kapat()
            YAZ "Alarm kapatıldı."
            bildirim_gönder("Alarm kapatıldı.")
        DEĞİLSE
            YAZ "Yanlış şifre! Alarm devam ediyor."
        BİTİŞ_EĞER
    BİTİŞ_EĞER

    // Kullanıcı komutları kontrolü (opsiyonel)
    EĞER kullanici_girisi = TRUE İSE
        YAZ "1. Alarm Durumu\n2. Sensör Durumları\n3. Alarmı Kapat\n4. Sistemden Çıkış"
        YAZ "Seçiminizi yapınız:"
        OKU secim

        EĞER secim = 1 İSE
            YAZ "Alarm Durumu:", alarm_durumu
        DEĞİLSE EĞER secim = 2 İSE
            her sensör için sensörler
                YAZ sensör + " durumu: " + sensör_verisi_oku(sensör)
            BİTİŞ_DÖNGÜ
        DEĞİLSE EĞER secim = 3 İSE
            YAZ "Şifre giriniz:"
            OKU kapatma_sifre
            EĞER sifre_kontrol(kapatma_sifre) = DOĞRU İSE
                alarm_durumu ← FALSE
                alarmı_kapat()
                YAZ "Alarm kapatıldı."
                bildirim_gönder("Alarm kapatıldı.")
            DEĞİLSE
                YAZ "Yanlış şifre! Alarm devam ediyor."
            BİTİŞ_EĞER
        DEĞİLSE EĞER secim = 4 İSE
            YAZ "Sistemden çıkılıyor."
            ÇIKIŞ
        DEĞİLSE
            YAZ "Geçersiz seçim."
        BİTİŞ_EĞER
    BİTİŞ_EĞER

    BEKLE(1 saniye)  // Sensör okuma aralığı
BİTİŞ_DÖNGÜ

BİTİŞ
