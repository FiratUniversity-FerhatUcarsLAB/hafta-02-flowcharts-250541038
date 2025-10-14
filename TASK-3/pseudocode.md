digraph RandevuSistemi {
    rankdir=LR; // Soldan sağa akış
    node [shape=box, style="filled", fillcolor="#ADD8E6"]; // Düğümleri kutu yap ve açık mavi doldur

    // Başlangıç ve Bitiş
    Baslangic [label="BAŞLAT", shape=circle, fillcolor="#90EE90"];
    Bitis [label="SON", shape=circle, fillcolor="#F08080"];

    // Adımlar
    A1 [label="1. Kimlik Doğrulama (TCKN/Şifre)"];
    A2 [label="2. Poliklinik Seçimi"];
    A3 [label="3. Doktor Listesi Görüntüleme"];
    A4 [label="4. Uygun Tarih/Saatleri Getir"];
    A5 [label="5. Randevu Onaylama ve Kayıt"];
    A6 [label="6. SMS Gönderme"];

    // Karar Noktaları
    D1 [label="Doğrulama Başarılı mı?", shape=diamond, fillcolor="#FFD700"];
    D2 [label="Doktor Var mı?", shape=diamond, fillcolor="#FFD700"];
    D3 [label="Randevu Kaydı Başarılı mı?", shape=diamond, fillcolor="#FFD700"];

    // Akış Bağlantıları
    Baslangic -> A1;
    A1 -> D1;

    // Doğrulama Akışı
    D1 -> A2 [label="Evet"];
    D1 -> A1 [label="Hayır", style=dashed, color=red]; // Tekrar Dene

    // Doktor Akışı
    A2 -> A3;
    A3 -> D2;
    D2 -> A4 [label="Evet"];
    D2 -> A2 [label="Hayır/Geri Dön", style=dashed, color=red]; // Poliklinik Seçimine Geri Dön

    // Randevu Akışı
    A4 -> A5;
    A5 -> D3;

    // Kayıt Akışı
    D3 -> A6 [label="Evet"];
    D3 -> Bitis [label="Hayır/Hata", style=dashed, color=red];

    // Bitiş
    A6 -> Bitis;
}


2.MODÜL   

digraph RandevuSistemi {
    rankdir=LR; // Soldan sağa akış
    node [shape=box, style="filled", fillcolor="#ADD8E6"]; // Düğümleri kutu yap ve açık mavi doldur

    // Başlangıç ve Bitiş
    Baslangic [label="BAŞLAT", shape=circle, fillcolor="#90EE90"];
    Bitis [label="SON", shape=circle, fillcolor="#F08080"];

    // Adımlar
    A1 [label="1. Kimlik Doğrulama (TCKN/Şifre)"];
    A2 [label="2. Poliklinik Seçimi"];
    A3 [label="3. Doktor Listesi Görüntüleme"];
    A4 [label="4. Uygun Tarih/Saatleri Getir"];
    A5 [label="5. Randevu Onaylama ve Kayıt"];
    A6 [label="6. SMS Gönderme"];

    // Karar Noktaları
    D1 [label="Doğrulama Başarılı mı?", shape=diamond, fillcolor="#FFD700"];
    D2 [label="Doktor Var mı?", shape=diamond, fillcolor="#FFD700"];
    D3 [label="Randevu Kaydı Başarılı mı?", shape=diamond, fillcolor="#FFD700"];

    // Akış Bağlantıları
    Baslangic -> A1;
    A1 -> D1;

    // Doğrulama Akışı
    D1 -> A2 [label="Evet"];
    D1 -> A1 [label="Hayır", style=dashed, color=red]; // Tekrar Dene

    // Doktor Akışı
    A2 -> A3;
    A3 -> D2;
    D2 -> A4 [label="Evet"];
    D2 -> A2 [label="Hayır/Geri Dön", style=dashed, color=red]; // Poliklinik Seçimine Geri Dön

    // Randevu Akışı
    A4 -> A5;
    A5 -> D3;

    // Kayıt Akışı
    D3 -> A6 [label="Evet"];
    D3 -> Bitis [label="Hayır/Hata", style=dashed, color=red];

    // Bitiş
    A6 -> Bitis;
}



BİRLEŞTİRİLMİŞ HALİ 


FONKSİYON Ana_Hastane_Portali_Baslat():
    Kullanici_Dogrulandi = YANLIS
    
    // Tek Birleşik Kimlik Doğrulama Ekranı
    TEKRAR_GIRIS_DENEMESI:
        KimlikNo = KULLANICIDAN_GIRDI_AL("T.C. Kimlik Numarası:")
        Sifre_veya_DogumTarihi = KULLANICIDAN_GIRDI_AL("Şifreniz veya Doğum Tarihiniz (GG.AA.YYYY):")

        // **Not:** Gerçek sistemde tek tip kimlik doğrulama yöntemi olmalıdır.
        EĞER Kimlik_Dogrula(KimlikNo, Sifre_veya_DogumTarihi) BAŞARILI İSE:
            Kullanici_Dogrulandi = DOĞRU
            Giris_Bilgileri = KimlikNo
        DEĞİLSE:
            EKRANA_YAZ("Hata: Geçersiz giriş. Tekrar deneyin.")
            GOTO TEKRAR_GIRIS_DENEMESI // Deneme sayısını sınırlayın!
        
        EĞER Kullanici_Dogrulandi EŞİTTİR DOĞRU İSE:
            
            // Ana Menü
            ANA_MENU:
                EKRANA_YAZ("Lütfen yapmak istediğiniz işlemi seçin:")
                EKRANA_YAZ("[1] Randevu Al/Görüntüle (Modül 1)")
                EKRANA_YAZ("[2] Tahlil Sonucu Görüntüle (Modül 2)")
                EKRANA_YAZ("[3] Çıkış")
                Secim = KULLANICIDAN_SECIM_AL_SAYISAL()

                EĞER Secim EŞİTTİR 1 İSE:
                    Randevu_Modul_Baslat(Giris_Bilgileri)
                DEĞİLSE EĞER Secim EŞİTTİR 2 İSE:
                    Tahlil_Modul_Baslat(Giris_Bilgileri)
                DEĞİLSE EĞER Secim EŞİTTİR 3 İSE:
                    EKRANA_YAZ("Oturum kapatılıyor. Hoşça kalın.")
                    ÇIKIŞ YAP
                DEĞİLSE:
                    EKRANA_YAZ("Geçersiz seçim.")
                    GOTO ANA_MENU
        
        DÖN // Fonksiyon Sonu

// ----------------------------------------------------------------------
// MODÜL 1: RANDEVU SİSTEMİ FONKSİYONU
// ----------------------------------------------------------------------

FONKSIYON Randevu_Modul_Baslat(KullaniciKimlikNo):

    // 1. Randevu Yönetimi Menüsü
    RAND_MENU:
        EKRANA_YAZ("[1] Yeni Randevu Al")
        EKRANA_YAZ("[2] Mevcut Randevularımı Görüntüle/İptal Et")
        EKRANA_YAZ("[3] Ana Menüye Dön")
        AltSecim = KULLANICIDAN_SECIM_AL_SAYISAL()

        EĞER AltSecim EŞİTTİR 1 İSE:
            GOTO YENI_RANDEVU_AKISI
        DEĞİLSE EĞER AltSecim EŞİTTİR 2 İSE:
            // ... (Görüntüleme/İptal Akışı) ...
            GOTO RAND_MENU
        DEĞİLSE EĞER AltSecim EŞİTTİR 3 İSE:
            DÖN // Ana Menüye geri döner
        DEĞİLSE:
            GOTO RAND_MENU

    YENI_RANDEVU_AKISI:
    
        // 2. Poliklinik Seçimi (Tekrar)
        POLIKLINIK_SECIM_ADIM:
        PoliklinikListesi = Poliklinikleri_Veritabani_Getir()
        SecilenPoliklinik = KULLANICIDAN_SECIM_AL(PoliklinikListesi)

        // 3. Doktor Listesi Görüntüleme (Tekrar)
        DoktorListesi = Poliklinik_Doktorlarini_Getir(SecilenPoliklinik)

        EĞER DoktorListesi BOŞ DEĞİL İSE:
            SecilenDoktor = KULLANICIDAN_SECIM_AL(DoktorListesi)
        DEĞİLSE:
            EKRANA_YAZ("Doktor bulunamadı.")
            GOTO POLIKLINIK_SECIM_ADIM
            
        // 4. Uygun Saatleri Gösterme
        SecilenTarihSaat = Uygun_Randevu_Saatlerini_Goster_ve_Sec(SecilenDoktor)
        
        // 5. Randevu Onaylama ve Kayıt
        YeniRandevuID = Randevu_Veritabanina_Kaydet(KullaniciKimlikNo, SecilenDoktor, SecilenPoliklinik, SecilenTarihSaat)

        EĞER YeniRandevuID BAŞARILI İSE:
            EKRANA_YAZ("Randevunuz oluşturulmuştur. ID: " + YeniRandevuID)
            
            // 6. SMS Gönderme
            KullaniciTelefonNo = Kullanici_Telefon_Bilgisini_Getir(KullaniciKimlikNo)
            SMS_Gonder(KullaniciTelefonNo, "Randevunuz oluşturuldu.")
            
        DEĞİLSE:
            EKRANA_YAZ("Hata: Randevu oluşturulamadı.")

    GOTO RAND_MENU // İşlem bitince Randevu Menüsüne dön.

// ----------------------------------------------------------------------
// MODÜL 2: TAHLİL SİSTEMİ FONKSİYONU
// ----------------------------------------------------------------------

FONKSIYON Tahlil_Modul_Baslat(KullaniciKimlikNo):

    // 1. Tahlil Listesini Görüntüleme
    TahlilListesi = Kullanici_Tahlillerini_Getir(KullaniciKimlikNo)
    
    EĞER TahlilListesi BOŞ İSE:
        EKRANA_YAZ("Adınıza ait tahlil bulunmamaktadır.")
        DÖN // Ana Menüye geri dön
        
    EKRANA_YAZ("Lütfen durumunu görmek istediğiniz tahlili seçin:")
    SecilenTahlilID = KULLANICIDAN_SECIM_AL(TahlilListesi)

    // 2. Tahlil Sonuç Durumu Kontrolü
    TahlilDurumu = Tahlil_Sonuc_Durumunu_Getir(SecilenTahlilID)

    EĞER TahlilDurumu EŞİTTİR "HAZIR" İSE:
        
        // 3. Görüntüleme ve PDF İndirme Seçeneği
        EKRANA_YAZ("Seçenekler: [1] Görüntüle, [2] PDF İndir")
        Secim = KULLANICIDAN_SECIM_AL_SAYISAL()

        EĞER Secim EŞİTTİR 2 İSE:
            // PDF İndirme İşlemi
            Dosya_Indirme_Baslat(Tahlil_Sonuc_PDF_Olustur(SecilenTahlilID))
        DEĞİLSE:
            // Sonucu Ekranda Gösterme
            Tahlil_Sonucunu_Ekranda_Goster(SecilenTahlilID)
            
    DEĞİLSE EĞER TahlilDurumu EŞİTTİR "İŞLENİYOR" İSE:
        EKRANA_YAZ("Tahliliniz şu anda laboratuvarda İŞLENİYOR.")
        EKRANA_YAZ("Tahmini süre: " + TahminiSureyi_Hesapla(SecilenTahlilID))
        
    DEĞİLSE:
        EKRANA_YAZ("Tahlil durumu bilinmiyor/alınmadı.")
        
    DÖN // Ana Menüye geri döner
    
