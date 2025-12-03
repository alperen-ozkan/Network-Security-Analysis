# ARP Zehirlenmesi (ARP Poisoning / MITM) Analizi

## 1. Konu Özeti
ARP (Address Resolution Protocol), IP adreslerini MAC adresleri ile eşleştirmek için kullanılır. Ancak doğrulama mekanizması yoktur, yani beyan esastır.

## 2. Ofansif Bakış Açısı (Saldırgan Ne Yapar?)
Saldırgan, hedef ve modem arasına girerek trafiği dinler.
* **Araçlar:** `arpspoof`, `bettercap`.
* **Süreç:**
  1. Hedefe: "Ben Modemim".
  2. Modeme: "Ben Hedefim".
  3. `ip_forward` ile trafiği aktar.

## 3. Blue Team Analizi ve Tespit (Savunma Stratejisi)

### A. Wireshark Analizi
* **Duplicate IP Address:** Wireshark veya sistem loglarında "Duplicate IP address detected for..." uyarısı MITM'in en net kanıtıdır.
* **Filtre:** `arp.duplicate-address-detected`.

### B. Komut Satırı (`arp -a`)
Terminalde ağ geçidi (Gateway) MAC adresi ile başka bir cihazın MAC adresinin aynı görünmesi zehirlenme işaretidir.

### C. Önlemler
* **DAI (Dynamic ARP Inspection):** Akıllı switchlerde bu özellik açılarak ARP paketleri denetlenir.
* **VPN:** Trafik şifrelendiği için aradaki kişi veriyi okuyamaz.
