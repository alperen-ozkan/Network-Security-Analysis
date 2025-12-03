# Kablosuz Ağ (Wi-Fi) Güvenlik Analizi

## 1. Konu Özeti
Kablosuz ağlar, fiziksel erişim gerektirmediği için saldırıya açıktır. WEP, WPA ve WPA2 protokolleri zamanla çeşitli zafiyetler barındırmıştır.

## 2. Ofansif Bakış Açısı (Saldırgan Ne Yapar?)
Saldırganın amacı ağa yetkisiz erişim sağlamaktır.
* **Deauthentication Saldırısı:** Bağlı kullanıcıyı ağdan düşürerek, tekrar bağlanırken gönderdiği "Handshake" paketini yakalamak (`aireplay-ng`).
* **Kırma (Cracking):** Yakalanan şifreli el sıkışma paketini `aircrack-ng` ve wordlistler (örn: `rockyou.txt`) ile kırmaya çalışmak.
* **Araçlar:** `airmon-ng` (Monitor modu), `airodump-ng` (Trafik izleme).

## 3. Blue Team Analizi ve Tespit (Savunma Stratejisi)

### A. Deauth Saldırısını Tespit Etme
* **Belirti:** Wi-Fi bağlantısının sürekli kopması ve tekrar gelmesi.
* **Analiz:** Wireshark'ta `wlan.fc.type_subtype == 0x000c` filtresi ile Deauthentication paketleri görülebilir. Eğer bu paketlerden binlerce varsa, saldırı altındasınızdır.

### B. WIDS (Wireless Intrusion Detection System)
Kurumsal Access Point'ler (AP), "Rogue AP" (Sahte Erişim Noktası) veya aşırı Deauth paketlerini tespit edip yöneticilere alarm üretir.

### C. Önlemler
* **Şifreleme:** WEP asla kullanılmamalıdır. WPA2 (AES) veya mümkünse WPA3 kullanılmalıdır.
* **Parola Politikası:** Sözlük saldırılarını (Dictionary Attacks) önlemek için parolalar en az 12 karakterli, karmaşık ve anlamsız olmalıdır.
* **MAC Filtreleme:** Sadece izinli cihazların ağa girmesi (Bireysel kullanımda etkilidir ama MAC adresi taklit edilebilir).
