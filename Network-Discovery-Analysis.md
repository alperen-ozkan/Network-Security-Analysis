# Ağ Keşfi (Network Discovery) ve Analizi

## 1. Konu Özeti
Bir ağa dahil olan cihazın ilk yaptığı işlem, çevresini tanımaktır (Reconnaissance). Hangi cihazların (Host) açık olduğu, IP ve MAC adresleri, çalışan servisler bu aşamada belirlenir.

## 2. Ofansif Bakış Açısı (Saldırgan Ne Yapar?)
Saldırgan "sessizce" ağ haritasını çıkarmaya çalışır.
* **Araçlar:** `netdiscover`, `nmap`, `arp-scan`.
* **Yöntem:**
  * **ARP Taraması:** Yerel ağdaki tüm IP'lere "Sen orada mısın?" (ARP Request) sorusu gönderilir.
  * **Port Taraması:** `nmap -sS` (SYN Scan) ile hedef makinedeki açık kapılar (22 SSH, 80 HTTP vb.) bulunur.

## 3. Blue Team Analizi ve Tespit (Savunma Stratejisi)

### A. Ağ Trafiği Analizi
Ağda normalin üzerinde bir **ARP trafiği** (ARP Storm) varsa, birisi `netdiscover` çalıştırıyor olabilir.
* **Wireshark:** Filtre olarak `arp` yazıldığında, tek bir kaynaktan (Source) tüm ağa (Broadcast) seri istekler gidiyorsa bu bir taramadır.

### B. Nmap Taramalarını Yakalamak
* **IDS/IPS:** Snort veya Suricata gibi saldırı tespit sistemleri, ardışık port denemelerini (Örn: Bir saniyede 100 farklı porta istek gelmesi) "Port Scanning" olarak işaretler ve bloklar.
* **Loglar:** Hedef sunucunun güvenlik duvarı (Firewall) loglarında "Dropped Packet" sayısındaki ani artış tarama belirtisidir.

### C. Varlık Yönetimi (Asset Management)
Ağda bilinmeyen bir MAC adresi (Örn: `Unknown Vendor`) görüldüğünde, bu cihaz izole edilmeli ve NAC (Network Access Control) çözümleri kullanılmalıdır.
