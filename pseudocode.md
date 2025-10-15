// **BAŞLANGIÇ**
FONKSIYON AkilliEvGuvenlikSistemi()
  SISTEM_AKTIF = DOGRU           // Ana sistem anahtarı (Kullanıcı tarafından ayarlanır)
  EV_SAHIBI_EVDE = YANLIS        // Yanlış alarm kontrolü için başlangıç durumu
  
  // Ana DÖNGÜ: Güvenlik sisteminin 7/24 sürekli çalıştığı döngü
  DOGRU_OLDUGU_SURECE_TEKRARLA:
    
    EGER SISTEM_AKTIF ISE
      // 1. Sensör Okuma Döngüsü (Her İterasyonda)
      HAREKET_ALGILANDI = SensorOku("Hareket")
      GIRIS_IHLALI = SensorOku("KapiPencere")

      // 2. Tehdit Algılama Kontrolü
      EGER HAREKET_ALGILANDI VEYA GIRIS_IHLALI ISE
        YAZDIR(">>> IHLAL ALGILANDI: Analiz Ediliyor...")

        // 3. Alarm Seviyesi Belirleme
        EGER HAREKET_ALGILANDI VE GIRIS_IHLALI ISE
          ALARM_SEVIYESI = 3 // YÜKSEK: Ciddi ve Çift İhlal
        DEGILSE EGER HAREKET_ALGILANDI VEYA GIRIS_IHLALI ISE
          ALARM_SEVIYESI = 2 // ORTA: Tek İhlal (Giriş veya Yoğun Hareket)
        DEGILSE
          ALARM_SEVIYESI = 1 // DÜŞÜK: Hafif/Şüpheli Hareket (Ön Uyarı)
        BITIR_EGER

        // 4. Kamera Aktivasyonu (Seviye 2 ve üzeri için)
        EGER ALARM_SEVIYESI >= 2 ISE
          KameraAktivasyonu(DOGRU)
          YAZDIR("Kamera kaydı başlatıldı.")
        BITIR_EGER

        // 5. Yanlış Alarm Kontrolü ve Doğrulama Aşaması
        YANLIS_ALARM_OLASILIGI = EV_SAHIBI_EVDE // Ev sahibi evdeyse düşük seviyede yanlış alarm olasılığı var

        EGER YANLIS_ALARM_OLASILIGI VEYA ALARM_SEVIYESI < 3 ISE
          YAZDIR("Doğrulama bekleniyor. Ev sahibi onayı aranıyor.")
          BildirimGonder(ALARM_SEVIYESI, "UYARI: Olası yanlış alarm. Doğrulama bekleniyor.", "App")
          
          BEKLE(10 Saniye) // Ev sahibinin komut göndermesi için süre
          KOMUT = KomutOku() // 'SIFIRLA' veya 'DEVAM_ET'

          EGER KOMUT == "SIFIRLA" ISE
            KameraAktivasyonu(YANLIS)
            YAZDIR("Alarm sıfırlandı. Döngü başına dönülüyor.")
            DEVAM_ET // Ana döngünün bir sonraki iterasyonuna geç
          BITIR_EGER
        BITIR_EGER
        
        // 6. Nihai Alarm ve Kritik Bildirimler (Sıfırlanmadıysa veya Yüksek Seviyeyse)
        AlarmCal(ALARM_SEVIYESI)
        BildirimGonder(ALARM_SEVIYESI, "!!! ACIL GÜVENLIK IHLALI !!!", "SMS", "App", "Email")
        YAZDIR("FULL ALARM AKTIF. Polis/Güvenlik birimine bildirim gönderildi.")
        
        // 7. Alarm Sıfırlama Bekleme Döngüsü
        ALARM_DURUMU_DEVAM:
        KOMUT = KomutOku()
        EGER KOMUT != "SIFIRLA" ISE
          BEKLE(5 Saniye)
          GIT ALARM_DURUMU_DEVAM // Sıfırlama gelene kadar alarmı sürdür
        BITIR_EGER
        
        AlarmSifirla()
        KameraAktivasyonu(YANLIS)
        YAZDIR("Sistem Başarıyla Sıfırlandı.")

      BITIR_EGER // Tehdit kontrolü sonu

      // 8. Bekle ve Tekrar Kontrol Et
      BEKLE(2 Saniye) // Bir sonraki hızlı okuma iterasyonu için bekleme

    BITIR_EGER // Sistem Aktif mi kontrolü sonu

  BITIR_DOGRU_OLDUGU_SURECE_TEKRARLA // Sonsuz Döngü Sonu

BITIR_FONKSIYON
