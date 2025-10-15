BAŞLA

sepet ← boş liste
urun_listesi ← [ürün1, ürün2, ürün3, ..., ürünN]  // mevcut ürünler ve stokları

DÖNGÜ alışveriş_devam TRUE TEKRAR

    YAZ "Ürün listesi:"
    DÖNGÜ her ürün için urun_listesi
        YAZ urun.id, "-", urun.isim, "(Stok:", urun.stok, ", Fiyat:", urun.fiyat, ")"
    BİTİŞ_DÖNGÜ

    YAZ "Sepetteki ürünler:"
    EĞER sepet boşsa İSE
        YAZ "Sepetiniz boş."
    DEĞİLSE
        DÖNGÜ her ürün için sepet
            YAZ urun.isim, "-", urun.miktar, "adet"
        BİTİŞ_DÖNGÜ
    BİTİŞ_EĞER

    YAZ "Ne yapmak istiyorsunuz? (Ekle / Güncelle / Sil / Ödeme / Çıkış)"
    OKU islem

    EĞER islem = "Ekle" İSE
        YAZ "Eklemek istediğiniz ürünün ID'sini girin:"
        OKU urun_id
        YAZ "Miktarı girin:"
        OKU miktar

        // Ürün stok kontrolü
        urun ← urun_listesi'nden urun_id'ye sahip ürün

        EĞER urun = null İSE
            YAZ "Geçersiz ürün ID."
        DEĞİLSE
            EĞER miktar ≤ 0 İSE
                YAZ "Geçersiz miktar."
            DEĞİLSE
                EĞER miktar > urun.stok İSE
                    YAZ "Yeterli stok yok. Mevcut stok:", urun.stok
                DEĞİLSE
                    // Sepette ürün var mı kontrol et
                    urun_sepet ← sepet'te urun_id olan ürün

                    EĞER urun_sepet = null İSE
                        sepet'e (urun_id, urun.isim, miktar, urun.fiyat) ekle
                    DEĞİLSE
                        urun_sepet.miktar ← urun_sepet.miktar + miktar
                    BİTİŞ_EĞER

                    YAZ miktar, "adet", urun.isim, "sepete eklendi."
                BİTİŞ_EĞER
            BİTİŞ_EĞER
        BİTİŞ_EĞER

    DEĞİLSE EĞER islem = "Güncelle" İSE
        YAZ "Miktarını güncellemek istediğiniz ürünün ID'sini girin:"
        OKU urun_id

        urun_sepet ← sepet'te urun_id olan ürün
        EĞER urun_sepet = null İSE
            YAZ "Sepette böyle bir ürün yok."
        DEĞİLSE
            YAZ "Yeni miktarı girin:"
            OKU yeni_miktar

            EĞER yeni_miktar < 0 İSE
                YAZ "Miktar negatif olamaz."
            DEĞİLSE EĞER yeni_miktar = 0 İSE
                sepet'ten urun_sepet'i çıkar
                YAZ "Ürün sepetten çıkarıldı."
            DEĞİLSE
                urun ← urun_listesi'nden urun_id'ye sahip ürün
                EĞER yeni_miktar > urun.stok İSE
                    YAZ "Yeterli stok yok. Mevcut stok:", urun.stok
                DEĞİLSE
                    urun_sepet.miktar ← yeni_miktar
                    YAZ "Ürün miktarı güncellendi."
                BİTİŞ_EĞER
            BİTİŞ_EĞER
        BİTİŞ_EĞER

    DEĞİLSE EĞER islem = "Sil" İSE
        YAZ "Silmek istediğiniz ürünün ID'sini girin:"
        OKU urun_id

        urun_sepet ← sepet'te urun_id olan ürün
        EĞER urun_sepet = null İSE
            YAZ "Sepette böyle bir ürün yok."
        DEĞİLSE
            sepet'ten urun_sepet çıkar
            YAZ "Ürün sepetten çıkarıldı."
        BİTİŞ_EĞER

    DEĞİLSE EĞER islem = "Ödeme" İSE
        EĞER sepet boşsa İSE
            YAZ "Sepetiniz boş. Ödeme yapılamaz."
        DEĞİLSE
            toplam ← 0
            DÖNGÜ her ürün için sepet
                toplam ← toplam + (ürün.miktar * ürün.fiyat)
            BİTİŞ_DÖNGÜ

            YAZ "Toplam tutar: ", toplam, " TL"
            YAZ "Ödemeyi onaylıyor musunuz? (E/H)"
            OKU onay

            EĞER onay = "E" VEYA onay = "e" İSE
                DÖNGÜ her ürün için sepet
                    urun_stok ← urun_listesi'nden ürün.id
                    urun_stok.stok ← urun_stok.stok - ürün.miktar
                BİTİŞ_DÖNGÜ

                sepet'i boşalt
                YAZ "Ödeme başarılı. Teşekkürler!"
                ÇIKIŞ
            DEĞİLSE
                YAZ "Ödeme iptal edildi."
            BİTİŞ_EĞER
        BİTİŞ_EĞER

    DEĞİLSE EĞER islem = "Çıkış" İSE
        YAZ "Alışverişten çıkılıyor."
        ÇIKIŞ

    DEĞİLSE
        YAZ "Geçersiz işlem seçildi."

BİTİŞ_DÖNGÜ

BİTİŞ
