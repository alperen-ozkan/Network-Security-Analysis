# Linux Sistem Temelleri ve Güvenlik Analizi

## 1. Konu Özeti
Linux, siber güvenlik dünyasının temel işletim sistemidir. Dosya sistemi hiyerarşisi, izinler ve kullanıcı yönetimi hem saldırganlar hem de savunmacılar için kritik öneme sahiptir. Kali Linux gibi dağıtımlar ofansif amaçlarla kullanılırken, sunucu tarafında genellikle Debian/Ubuntu/CentOS tercih edilir.

## 2. Ofansif Bakış Açısı (Saldırgan Ne Yapar?)
Saldırganlar sisteme eriştiklerinde kalıcılık sağlamak ve yetki yükseltmek isterler.
* **Komutlar:** `cat /etc/passwd` (kullanıcıları gör), `uname -a` (kernel sürümünü gör), `chmod` (dosya izinlerini değiştir).
* **Hedef:** Root yetkisine ("Superuser") ulaşmak.
* **İz Gizleme:** `.bash_history` dosyasını silerek veya logları temizleyerek tespit edilmeyi zorlaştırmaya çalışırlar.

## 3. Blue Team Analizi ve Tespit (Savunma Stratejisi)
Bir Linux sistem yöneticisi veya güvenlik analisti olarak şüpheli aktiviteleri şöyle tespit ederiz:

### A. Log İnceleme (Log Analysis)
Linux'ta her hareket iz bırakır. Aşağıdaki dosyalar düzenli kontrol edilmelidir:
* **/var/log/auth.log:** Başarısız `sudo` denemeleri, yeni kullanıcı oluşturma aktiviteleri burada görünür.
* **/var/log/syslog:** Genel sistem hataları ve servis aktiviteleri.
* **Komut:** `tail -f /var/log/auth.log` ile anlık izleme yapılabilir.

### B. Dosya Bütünlüğü ve İzinler
* **Kritik Dosyalar:** `/etc/shadow` (parola hashleri) ve `/etc/sudoers` dosyalarında yapılan değişiklikler alarm sebebidir.
* **SUID Bit Kontrolü:** Root yetkisiyle çalışan şüpheli dosyaları bulmak için:
  `find / -perm -4000 2>/dev/null`

### C. Sıkılaştırma (Hardening)
* Gereksiz servislerin kapatılması.
* SSH root girişinin engellenmesi (`PermitRootLogin no`).
* En az yetki prensibi (Least Privilege) uygulanması.
