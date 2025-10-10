Ad-Soyad:Muhammed Metin Kızıl
Öğrenci No:250541038

BAŞLA

    // Tanımlamalar
    DEĞİŞKEN BAKIYE = 5000     // Kullanıcının mevcut bakiyesi (Örnek)
    DEĞİŞKEN GUNLUK_LIMIT = 1000 // Günlük para çekme limiti
    DEĞİŞKEN CEKILEN_TOPLAM = 0 // Gün içinde çekilen toplam tutar
    DEĞİŞKEN PIN_HAKKI = 3
    DEĞİŞKEN DOGRU_PIN = 1234  // Doğru PIN (Örnek)
    DEĞİŞKEN CEVAP = "EVET"
    
    
    //---------------------------------------------------------
    // 1. PIN DOĞRULAMA DÖNGÜSÜ (3 HAK)
    //---------------------------------------------------------
    
    DÖNGÜ (PIN_HAKKI > 0)
    
        YAZ "Lütfen PIN kodunuzu girin:"
        OKU GIRILEN_PIN
        
        EĞER GIRILEN_PIN = DOGRU_PIN İSE
            PIN_HAKKI = 0 // Döngüden çıkmak için hakkı sıfırla
            YAZ "PIN Doğrulama Başarılı."
            PIN_DOGRULAMA_BASARILI = DOĞRU
        DEĞİLSE
            PIN_HAKKI = PIN_HAKKI - 1
            YAZ "Hatalı PIN. Kalan hakkınız: " + PIN_HAKKI
            PIN_DOGRULAMA_BASARILI = YANLIŞ
        BİTİR EĞER
        
    BİTİR DÖNGÜ
    
    
    //---------------------------------------------------------
    // 2. ANA İŞLEM DÖNGÜSÜ (İŞLEM TEKRARI SEÇENEĞİ)
    //---------------------------------------------------------

    EĞER PIN_DOGRULAMA_BASARILI İSE
        
        DÖNGÜ (CEVAP = "EVET")
            
            YAZ "Çekmek istediğiniz tutarı giriniz (20 TL'nin katları olmalıdır):"
            OKU CEKILECEK_TUTAR
            
            // 3. TUTAR KONTROLÜ (20 TL KATLARI)
            EĞER (CEKILECEK_TUTAR MOD 20) = 0 İSE
                
                // 4. BAKİYE KONTROLÜ
                EĞER CEKILECEK_TUTAR <= BAKIYE İSE
                    
                    // 5. GÜNLÜK LİMİT KONTROLÜ
                    EĞER (CEKILEN_TOPLAM + CEKILECEK_TUTAR) <= GUNLUK_LIMIT İSE
                        
                        // İŞLEM BAŞARILI
                        BAKIYE = BAKIYE - CEKILECEK_TUTAR
                        CEKILEN_TOPLAM = CEKILEN_TOPLAM + CEKILECEK_TUTAR
                        
                        YAZ "İşlem Başarılı. Lütfen paranızı alın."
                        YAZ "Yeni bakiyeniz: " + BAKIYE
                        YAZ "Bugün kalan çekme limitiniz: " + (GUNLUK_LIMIT - CEKILEN_TOPLAM)
                        
                    DEĞİLSE
                        YAZ "Hata: Günlük para çekme limitinizi aşıyorsunuz. Kalan limit: " + (GUNLUK_LIMIT - CEKILEN_TOPLAM)
                    BİTİR EĞER
                    
                DEĞİLSE
                    YAZ "Hata: Hesabınızda yeterli bakiye bulunmamaktadır. Mevcut bakiye: " + BAKIYE
                BİTİR EĞER
                
            DEĞİLSE
                YAZ "Hata: Lütfen 20 TL'nin tam katları şeklinde bir tutar giriniz."
            BİTİR EĞER
            
            
            // İŞLEM TEKRARI SEÇENEĞİ
            YAZ "Başka bir işlem yapmak ister misiniz? (EVET/HAYIR)"
            OKU CEVAP
            
        BİTİR DÖNGÜ
        
        YAZ "İyi günler dileriz."
        
    DEĞİLSE // PIN doğrulama başarısız olduysa
        YAZ "PIN'inizi 3 kez yanlış girdiniz. Kartınıza el konulmuştur/İşlem iptal edilmiştir."
    BİTİR EĞER
    
BİTİR
  
