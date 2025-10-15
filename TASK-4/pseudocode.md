BAŞLA

// Kullanıcı Girişi
deneme_sayisi ← 0
maks_hak ← 3

DÖNGÜ deneme_sayisi < maks_hak
    YAZ "Öğrenci numaranızı giriniz:"
    OKU ogrenci_no
    YAZ "Şifrenizi giriniz:"
    OKU sifre

    EĞER kullanici_dogrula(ogrenci_no, sifre) İSE
        YAZ "Giriş başarılı."
        ÇIKIŞ_DÖNGÜ
    DEĞİLSE
        deneme_sayisi ← deneme_sayisi + 1
        YAZ "Hatalı giriş. Kalan hak:", maks_hak - deneme_sayisi
BİTİŞ_DÖNGÜ

EĞER deneme_sayisi = maks_hak İSE
    YAZ "Hesap bloke oldu. Lütfen öğrenci işleri ile iletişime geçin."
    ÇIKIŞ
BİTİŞ_EĞER

// Ana Menü
DÖNGÜ TRUE
    YAZ "1. Ders Listesini Görüntüle"
    YAZ "2. Ders Seç"
    YAZ "3. Ders Bırak"
    YAZ "4. Kayıtlı Dersleri Görüntüle"
    YAZ "5. Çıkış"
    YAZ "Seçiminizi yapınız:"
    OKU secim

    EĞER secim = 1 İSE
        dersler ← tum_dersleri_getir()
        DÖNGÜ her ders için dersler
            YAZ "Ders Kodu:", ders.kodu, "Adı:", ders.adi, "Kontenjan:", ders.kontenjan, "Kayıtlı:", ders.kayitli_ogrenci_sayisi
        BİTİŞ_DÖNGÜ

    DEĞİLSE EĞER secim = 2 İSE
        YAZ "Kayıt olmak istediğiniz ders kodunu giriniz:"
        OKU ders_kodu

        EĞER ders_var_mi(ders_kodu) DEĞİLSE
            YAZ "Ders bulunamadı."
            DEVAM
        BİTİŞ_EĞER

        EĞER on_kosul_kontrol(ogrenci_no, ders_kodu) = FALSE İSE
            YAZ "Bu ders için ön koşullar sağlanmamış."
            DEVAM
        BİTİŞ_EĞER

        EĞER kontenjan_doldu_mu(ders_kodu) = TRUE İSE
            YAZ "Ders kontenjanı dolmuştur."
            DEVAM
        BİTİŞ_EĞER

        EĞER program_cakisiyor_mu(ogrenci_no, ders_kodu) = TRUE İSE
            YAZ "Ders programınız ile çakışıyor."
            DEVAM
        BİTİŞ_EĞER

        ders_kayit_et(ogrenci_no, ders_kodu)
        YAZ "Derse kayıt başarılı."

    DEĞİLSE EĞER secim = 3 İSE
        kayitli_dersler ← ogrenci_derslerini_getir(ogrenci_no)
        EĞER kayitli_dersler boşsa İSE
            YAZ "Kayıtlı dersiniz yok."
            DEVAM
        BİTİŞ_EĞER

        YAZ "Bırakmak istediğiniz ders kodunu giriniz:"
        OKU ders_kodu

        EĞER ders_kayitli_mi(ogrenci_no, ders_kodu) = FALSE İSE
            YAZ "Bu derse kayıtlı değilsiniz."
            DEVAM
        BİTİŞ_EĞER

        ders_kayit_sil(ogrenci_no, ders_kodu)
        YAZ "Ders bırakıldı."

    DEĞİLSE EĞER secim = 4 İSE
        kayitli_dersler ← ogrenci_derslerini_getir(ogrenci_no)
        EĞER kayitli_dersler boşsa İSE
            YAZ "Kayıtlı dersiniz yok."
        DEĞİLSE
            DÖNGÜ her ders için kayitli_dersler
                YAZ "Ders Kodu:", ders.kodu, "Adı:", ders.adi
            BİTİŞ_DÖNGÜ
        BİTİŞ_EĞER

    DEĞİLSE EĞER secim = 5 İSE
        YAZ "Sistemden çıkılıyor."
        ÇIKIŞ

    DEĞİLSE
        YAZ "Geçersiz seçim."
BİTİŞ_DÖNGÜ

BİTİŞ
