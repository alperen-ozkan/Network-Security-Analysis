# Trafik Analizi ve HTTPS Güvenliği

## 1. Konu Özeti
Ağ trafiğini izlemek (Sniffing), hem saldırganların veri çalmak için hem de savunmacıların saldırıyı anlamak için kullandığı en temel yöntemdir. HTTP (açık metin) ve HTTPS (şifreli) arasındaki fark güvenlik için kritiktir.

## 2. Ofansif Bakış Açısı (Saldırgan Ne Yapar?)
* **Sniffing:** Ağdaki paketleri `Wireshark` veya `tcpdump` ile yakalar.
* **HTTP:** Kullanıcı adı ve parolalar açık metin (Clear Text) gittiği için anında okunur.
* **SSL Stripping:** `hstshijack` veya `bettercap` kullanarak kurbanın HTTPS bağlantısını HTTP'ye düşürmeye çalışır (Downgrade Attack).

## 3. Blue Team Analizi ve Tespit (Savunma Stratejisi)

### A. Protokol Analizi
* Wireshark'ta `http.request.method == "POST"` filtresi ile ağda şifresiz veri gönderen cihazlar tespit edilip uyarılmalıdır.
* Telnet (23), FTP (21), HTTP (80) gibi güvensiz protokoller yerine SSH, SFTP, HTTPS zorunlu kılınmalıdır.

### B. SSL/TLS İncelemesi
* **HSTS (HTTP Strict Transport Security):** Web sunucularında HSTS başlığı aktif edilerek tarayıcıların "sadece HTTPS" kullanması zorunlu hale getirilir. Bu, SSL Stripping saldırılarını engeller.
* **Sertifika Kontrolü:** Ağda geçersiz veya "Self-Signed" (Kendinden imzalı) sertifika üreten bir cihaz varsa, bu bir MITM saldırısı girişimi olabilir.
