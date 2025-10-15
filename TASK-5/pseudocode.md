Başla

SistemAktifMi ← true

SÜREKLİ SİSTEM DÖNGÜSÜ:
DO
    EĞER SistemAktifMi DEĞİLSE
        Devam et (döngü başına)
    SON

    // Sensörlerden veri oku
    hareketVarMi ← HareketSensörüOku()
    kapilarAcikMi ← KapiPencereSensörüOku()
    evSahibiEvdeMi ← EvSahibiKontrol()

    // Kamera aktivasyonu
    EĞER hareketVarMi VEYA kapilarAcikMi ISE
        KamerayiAktifEt()
    SON

    // Alarm durumunu belirle
    EĞER hareketVarMi VEYA kapilarAcikMi ISE
        EĞER evSahibiEvdeMi ISE
            // Yanlış alarm, işlem yapma
            Devam et
        DEĞİLSE
            // Tehdit seviyesini belirle
            SEVİYE ← AlarmSeviyesiBelirle(hareketVarMi, kapilarAcikMi)
            
            // Alarmı çalıştır
            AlarmCalistir(SEVİYE)

            // Bildirim gönder
            BildirimGonder(SMS, App, Email, SEVİYE)

            // Alarm sıfırlanana kadar bekle
            DO
                alarmSifirlandiMi ← AlarmSifirlamaKontrol()
                BEKLE(5 saniye)
            WHILE alarmSifirlandiMi DEĞİL
        SON
    SON

    // Bekle ve tekrar kontrol et
    BEKLE(1 saniye)

WHILE DOĞRU  // Sonsuz döngü
