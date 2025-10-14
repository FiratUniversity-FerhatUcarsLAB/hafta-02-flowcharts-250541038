FONKSİYON ANA_PROGRAM()
    BAŞLA

    // 1. Kullanıcı Giriş Kontrolü (Koşul)
    DOĞRU İSE KULLANICI_GİRİŞ_KONTROLÜ() EĞER
        KULLANICI_GİRİŞİ_BAŞARILI = EVET
    DEĞİLSE
        EKRANA_YAZ("Giriş yapınız veya kaydolunuz.")
        TEKRARLA: KULLANICI_GİRİŞ_KONTROLÜ()
    SON_EĞER

    // 2. Ürün Kategorileri Arasında Gezinme (Döngü)
    DÖNGÜ: UrunEklemeDongusu
        EKRANA_YAZ("3. Ürün Kategorilerine Göz Atılıyor...")

        // 3. Ürün Sepete Ekle
        ÜRÜN_SEÇ = KULLANICIDAN_AL("Sepete eklemek istediğiniz ürünün ID'sini girin.")

        // 4. Stok Kontrolü (Koşul)
        DOĞRU İSE STOK_DURUMU(ÜRÜN_SEÇ) > 0 EĞER
            SEPETE_EKLE(ÜRÜN_SEÇ)
            EKRANA_YAZ(ÜRÜN_SEÇ + " sepete eklendi.")
        DEĞİLSE
            EKRANA_YAZ("HATA: Ürün stokta kalmamıştır. Başka ürün seçiniz.")
        SON_EĞER

        // Döngü Kontrolü
        CEVAP = KULLANICIDAN_AL("Başka bir ürün eklemek ister misiniz? (E/H)")
        EĞER CEVAP == 'H' İSE
            DÖNGÜDEN_ÇIK
        SON_EĞER
    DÖNGÜ_SONU: UrunEklemeDongusu

    // 5. Sepeti Görüntüleme ve Düzenleme (Döngü)
    DÖNGÜ: SepetDuzenlemeDongusu
        SEPETİ_GÖRÜNTÜLE()
        
        CEVAP = KULLANICIDAN_AL("Sepeti düzenlemek (silme/adet değiştirme) ister misiniz? (E/H)")
        EĞER CEVAP == 'E' İSE
            // Düzenleme işlemleri burada yapılır
            SEPETİ_DÜZENLE() 
        DEĞİLSE
            DÖNGÜDEN_ÇIK
        SON_EĞER
    DÖNGÜ_SONU: SepetDuzenlemeDongusu


    // 6. İndirim Kodu Uygulanabilir (Koşul)
    CEVAP = KULLANICIDAN_AL("İndirim kodu kullanmak ister misiniz? (E/H)")
    EĞER CEVAP == 'E' İSE
        KOD = KULLANICIDAN_AL("Lütfen indirim kodunu giriniz.")
        DOĞRU İSE İNDİRİM_KODU_UYGULA(KOD) EĞER
            EKRANA_YAZ("İndirim kodu başarıyla uygulandı.")
        DEĞİLSE
            EKRANA_YAZ("HATA: Geçersiz veya süresi dolmuş kod.")
        SON_EĞER
    SON_EĞER

    // 7. Minimum 50 TL Kontrolü (Koşul)
    SEPET_TUTARI = SEPET_TOPLAM_TUTARI_HESAPLA()
    DOĞRU İSE SEPET_TUTARI < 50 EĞER
        EKRANA_YAZ("HATA: Siparişiniz minimum 50 TL tutarında olmalıdır.")
        TEKRARLA: UrunEklemeDongusu // Ürün eklemeye geri dön
    SON_EĞER

    // 8. Kargo Ücreti Hesaplama (Koşul: >= 200 TL ücretsiz)
    EĞER SEPET_TUTARI >= 200 İSE
        KARGO_ÜCRETİ = 0
        EKRANA_YAZ("Tebrikler! Kargo ücretsiz.")
    DEĞİLSE
        KARGO_ÜCRETİ = 20
        EKRANA_YAZ("Kargo ücreti: " + KARGO_ÜCRETİ + " TL")
    SON_EĞER

    TOPLAM_ÖDEME = SEPET_TUTARI + KARGO_ÜCRETİ

    // 9. Ödeme Yöntemi Seçimi (Koşul)
    ODEME_YÖNTEMİ_SEÇİMİ()
    DOĞRU İSE ÖDEME_YÖNTEMİ_SEÇİLDİ EĞER
        EKRANA_YAZ("Ödeme yöntemi başarıyla seçildi.")
    DEĞİLSE
        EKRANA_YAZ("HATA: Lütfen bir ödeme yöntemi seçiniz.")
        TEKRARLA: ODEME_YÖNTEMİ_SEÇİMİ()
    SON_EĞER

    // 10. Sipariş Onayı ve Ödeme
    EKRANA_YAZ("ÖDENECEK TUTAR: " + TOPLAM_ÖDEME + " TL")
    CEVAP = KULLANICIDAN_AL("Siparişi onaylıyor ve ödemeye geçiyor musunuz? (E/H)")
    
    EĞER CEVAP == 'E' İSE
        ÖDEME_İŞLEMİ_BAŞLAT(TOPLAM_ÖDEME)
        EKRANA_YAZ("Siparişiniz başarıyla oluşturuldu. Teşekkür ederiz.")
    DEĞİLSE
        EKRANA_YAZ("Sipariş iptal edildi.")
    SON_EĞER

    SON
