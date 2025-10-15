FONKSIYON UNIVERSITE_DERS_KAYIT_SISTEMI_CALISTIR()
    // 1. GİRİŞ
    DERS_SEPETI = Boş Liste
    TOPLAM_KREDI = 0
    DURUM = "Giriş Bekliyor"
    DANISMAN_ONAYI_GEREKLI = YANLIS

    YAZDIR("Üniversite Ders Kayıt Sistemine Hoş Geldiniz.")
    OGRENCI_NO = KULLANICIDAN_AL("Öğrenci No girin:")
    SIFRE = KULLANICIDAN_AL("Şifre girin:")

    EGERR (KullaniciyiDogrula(OGRENCI_NO, SIFRE) = DOGRU) ISE
        OGRENCI_BILGILERI = OgrenciBilgileriniYukle(OGRENCI_NO)
        GNO = OGRENCI_BILGILERI.GPA

        // 2. DERS LİSTESİ GÖRÜNTÜLEME
        DersListesiniGoster()

        // 3. DERS EKLEME/ÇIKARMA DÖNGÜSÜ
        SECIM_DURUMU = DOGRU
        DONGU_SURECE (SECIM_DURUMU = DOGRU) YAP
            YAZDIR("Yapmak istediğiniz işlemi seçin: (1: Ders Ekle, 2: Ders Çıkar, 3: Bitir)")
            ISLEM = KULLANICIDAN_AL()

            EGERR ISLEM = 1 ISE // DERS EKLEME
                DERS_KODU = KULLANICIDAN_AL("Eklemek istediğiniz dersin kodunu girin:")
                DERS = DersBilgileriniYukle(DERS_KODU)

                EGERR DERS boş DEĞİLSE ISE
                    // KONTROL 1: KONTENJAN KONTROLÜ
                    EGERR DERS.KayıtlıOgrenciSayisi < DERS.Kontenjan ISE
                        // KONTROL 2: ÖN KOŞUL KONTROLÜ
                        EGERR OnKosulKarsilaniyor(OGRENCI_NO, DERS_KODU) ISE
                            // KONTROL 3: ZAMAN ÇAKIŞMASI KONTROLÜ
                            EGERR ZamanCakismasiYok(DERS_SEPETI, DERS) ISE

                                YENI_TOPLAM_KREDI = TOPLAM_KREDI + DERS.Kredi
                                // KONTROL 4: KREDİ LİMİTİ KONTROLÜ (Toplam Kredi <= 35)
                                EGERR YENI_TOPLAM_KREDI <= 35 ISE
                                    DERS_SEPETI.Ekle(DERS)
                                    TOPLAM_KREDI = YENI_TOPLAM_KREDI
                                    YAZDIR(DERS_KODU + " başarıyla eklendi. Yeni toplam kredi: " + TOPLAM_KREDI)
                                BASKA ISE
                                    YAZDIR("HATA: Kredi limiti (35) aşılıyor. Ders eklenemedi.")
                                SON_EGERR
                            BASKA ISE
                                YAZDIR("HATA: Seçilen ders, mevcut derslerinizle zaman çakışması yaşıyor.")
                            SON_EGERR
                        BASKA ISE
                            YAZDIR("HATA: Ön koşul derslerini tamamlamadınız.")
                        SON_EGERR
                    BASKA ISE
                        YAZDIR("HATA: Dersin kontenjanı dolu.")
                    SON_EGERR
                BASKA ISE
                    YAZDIR("HATA: Geçersiz ders kodu.")
                SON_EGERR

            BASKA EGERR ISLEM = 2 ISE // DERS ÇIKARMA
                DERS_KODU = KULLANICIDAN_AL("Çıkarmak istediğiniz dersin kodunu girin:")
                DERS = DERS_SEPETI.Bul(DERS_KODU)
                EGERR DERS boş DEĞİLSE ISE
                    DERS_SEPETI.Çıkar(DERS)
                    TOPLAM_KREDI = TOPLAM_KREDI - DERS.Kredi
                    YAZDIR(DERS_KODU + " başarıyla çıkarıldı. Yeni toplam kredi: " + TOPLAM_KREDI)
                BASKA ISE
                    YAZDIR("HATA: Seçtiğiniz ders, sepetinizde bulunmuyor.")
                SON_EGERR

            BASKA EGERR ISLEM = 3 ISE // BİTİR
                SECIM_DURUMU = YANLIS
            BASKA ISE
                YAZDIR("Geçersiz işlem seçimi.")
            SON_EGERR
        SON_DONGU

        // 5. DANIŞMAN ONAYI KONTROLÜ
        EGERR GNO < 2.50 ISE
            DANISMAN_ONAYI_GEREKLI = DOGRU
            DURUM = "Danışman Onayı Bekliyor"
        BASKA ISE
            DURUM = "Kayıt Tamamlandı"
        SON_EGERR

        // 6. KAYIT ÖZETİ GÖSTERME VE ONAYLAMA
        YAZDIR("--- KAYIT ÖZETİ ---")
        YAZDIR("Toplam Kredi: " + TOPLAM_KREDI)
        YAZDIR("Seçilen Dersler: " + DERS_SEPETI.DersleriListele())
        YAZDIR("Danışman Onayı Gerekiyor Mu?: " + (DANISMAN_ONAYI_GEREKLI ? "EVET" : "HAYIR"))
        YAZDIR("Kayıt Durumu: " + DURUM)

        // Veritabanına Kayıt İşlemi
        KaydiKaydet(OGRENCI_NO, DERS_SEPETI, DURUM)

        YAZDIR("Ders kayıt işlemi sona erdi.")

    BASKA ISE
        YAZDIR("HATA: Geçersiz öğrenci numarası veya şifre.")
    SON_EGERR

SON_FONKSIYON
