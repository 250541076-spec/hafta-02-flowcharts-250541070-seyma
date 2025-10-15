BAŞLA

// Kullanıcı Girişi
deneme_sayisi ← 0
maks_hak ← 3

DÖNGÜ deneme_sayisi < maks_hak
    YAZ "Kullanıcı Adı Giriniz:"
    OKU kullanici_adi
    YAZ "Şifre Giriniz:"
    OKU sifre

    EĞER kullanıcı_bilgileri_dogru(kullanici_adi, sifre) İSE
        YAZ "Giriş başarılı."
        ÇIKIŞ_DÖNGÜ
    DEĞİLSE
        deneme_sayisi ← deneme_sayisi + 1
        YAZ "Hatalı giriş. Kalan hak:", maks_hak - deneme_sayisi
BİTİŞ_DÖNGÜ

EĞER deneme_sayisi = maks_hak İSE
    YAZ "Hesap bloke oldu. Lütfen destek ile iletişime geçin."
    ÇIKIŞ
BİTİŞ_EĞER

// Ana Menü
DÖNGÜ TRUE
    YAZ "1. Randevu Al"
    YAZ "2. Randevu Görüntüle"
    YAZ "3. Randevu İptal Et"
    YAZ "4. Tahlil Sonuçlarını Görüntüle"
    YAZ "5. Çıkış"
    YAZ "Seçiminizi yapınız:"
    OKU secim

    EĞER secim = 1 İSE
        // Randevu alma işlemi
        uygun_birimler ← birimleri_getir()
        YAZ "Birim Seçiniz:"
        DÖNGÜ her birim için uygun_birimler
            YAZ birim.id, "-", birim.ad
        BİTİŞ_DÖNGÜ
        OKU secilen_birim_id

        uygun_doktorlar ← doktorlari_getir(secilen_birim_id)
        YAZ "Doktor Seçiniz:"
        DÖNGÜ her doktor için uygun_doktorlar
            YAZ doktor.id, "-", doktor.ad
        BİTİŞ_DÖNGÜ
        OKU secilen_doktor_id

        YAZ "Randevu Tarihi Giriniz (YYYY-AA-GG):"
        OKU tarih

        EĞER randevu_musait_mi(secilen_doktor_id, tarih) İSE
            randevu_olustur(kullanici_adi, secilen_doktor_id, tarih)
            YAZ "Randevunuz başarıyla alındı."
        DEĞİLSE
            YAZ "Seçilen tarihte randevu dolu."
        BİTİŞ_EĞER

    DEĞİLSE EĞER secim = 2 İSE
        randevular ← kullanici_randevulari_getir(kullanici_adi)
        EĞER randevular boşsa İSE
            YAZ "Hiç randevunuz bulunmamaktadır."
        DEĞİLSE
            DÖNGÜ her randevu için randevular
                YAZ "Doktor:", randevu.doktor_ad, "Tarih:", randevu.tarih
            BİTİŞ_DÖNGÜ
        BİTİŞ_EĞER

    DEĞİLSE EĞER secim = 3 İSE
        randevular ← kullanici_randevulari_getir(kullanici_adi)
        EĞER randevular boşsa İSE
            YAZ "İptal edilecek randevunuz yok."
        DEĞİLSE
            YAZ "İptal etmek istediğiniz randevunun ID'sini giriniz:"
            DÖNGÜ her randevu için randevular
                YAZ randevu.id, " - ", randevu.doktor_ad, " ", randevu.tarih
            BİTİŞ_DÖNGÜ
            OKU randevu_id

            EĞER randevu_id geçerli ve kullaniciya ait İSE
                randevu_iptal_et(randevu_id)
                YAZ "Randevunuz iptal edildi."
            DEĞİLSE
                YAZ "Geçersiz randevu ID."
            BİTİŞ_EĞER
        BİTİŞ_EĞER

    DEĞİLSE EĞER secim = 4 İSE
        tahlil_sonuc ← kullanici_tahlil_sonuc_getir(kullanici_adi)
        EĞER tahlil_sonuc boşsa İSE
            YAZ "Tahlil sonucunuz bulunmamaktadır."
        DEĞİLSE
            YAZ "Tahlil Sonuçlarınız:"
            DÖNGÜ her tahlil için tahlil_sonuc
                YAZ "Test:", tahlil.test_adi, "Sonuç:", tahlil.sonuc, "Tarih:", tahlil.tarih
            BİTİŞ_DÖNGÜ
        BİTİŞ_EĞER

    DEĞİLSE EĞER secim = 5 İSE
        YAZ "Sistemden çıkılıyor."
        ÇIKIŞ
    DEĞİLSE
        YAZ "Geçersiz seçim."

BİTİŞ_DÖNGÜ

BİTİŞ
