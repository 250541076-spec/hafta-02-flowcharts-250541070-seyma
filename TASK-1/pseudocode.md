BAŞLA

  // Sabitler
  GUNLUK_LIMIT ← 5000
  PIN ← "1234"
  HAK ← 3
  bakiye ← 10000
  cekilen_tutar_toplam ← 0

  // PIN Doğrulama
  DÖNGÜ PIN_dogrulama HAK > 0 TEKRAR

    YAZ "Lütfen 4 haneli PIN kodunuzu girin:"
    OKU girilen_pin

    EĞER girilen_pin = PIN İSE
      YAZ "PIN doğru. Hoş geldiniz."
      ÇIKIŞ PIN_dogrulama
    DEĞİLSE
      HAK ← HAK - 1
      EĞER HAK > 0 İSE
        YAZ "Hatalı PIN. Kalan hakkınız: ", HAK
      DEĞİLSE
        YAZ "3 kez hatalı PIN girildi. Kartınız bloke edildi."
        ÇIKIŞ
    BİTİŞ_EĞER

  BİTİŞ_DÖNGÜ

  // İşlem tekrarı döngüsü
  DÖNGÜ islem_tekrari TRUE TEKRAR

    YAZ "Lütfen çekmek istediğiniz tutarı girin:"
    OKU cekilecek_tutar

    // 20 TL katı kontrolü
    EĞER cekilecek_tutar MOD 20 ≠ 0 İSE
      YAZ "Lütfen 20 TL'nin katı bir tutar giriniz."
      DEVAM islem_tekrari
    BİTİŞ_EĞER

    // Bakiye kontrolü
    EĞER cekilecek_tutar > bakiye İSE
      YAZ "Yetersiz bakiye. Mevcut bakiyeniz: ", bakiye
      DEVAM islem_tekrari
    BİTİŞ_EĞER

    // Günlük limit kontrolü
    EĞER cekilen_tutar_toplam + cekilecek_tutar > GUNLUK_LIMIT İSE
      YAZ "Günlük para çekme limitinizi aşıyorsunuz."
      YAZ "Kalan günlük limitiniz: ", GUNLUK_LIMIT - cekilen_tutar_toplam
      DEVAM islem_tekrari
    BİTİŞ_EĞER

    // Tüm kontroller geçtiyse işlem yapılır
    bakiye ← bakiye - cekilecek_tutar
    cekilen_tutar_toplam ← cekilen_tutar_toplam + cekilecek_tutar

    YAZ "İşlem başarılı. Çekilen tutar: ", cekilecek_tutar
    YAZ "Kalan bakiye: ", bakiye
    YAZ "Bugün çekilen toplam tutar: ", cekilen_tutar_toplam

    // Yeni işlem isteği
    YAZ "Başka bir işlem yapmak istiyor musunuz? (E/H)"
    OKU cevap

    EĞER cevap = "E" VEYA cevap = "e" İSE
      DEVAM islem_tekrari
    DEĞİLSE
      YAZ "İyi günler dileriz."
      ÇIKIŞ
    BİTİŞ_EĞER

  BİTİŞ_DÖNGÜ

BİTİŞ
