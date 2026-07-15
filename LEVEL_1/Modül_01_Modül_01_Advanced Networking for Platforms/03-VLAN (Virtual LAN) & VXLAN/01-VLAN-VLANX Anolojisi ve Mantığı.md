# 🌐 Modül 01 / Konu 03: VLAN & VXLAN (Overlay Ağların Sırrı)

### A) VLAN Analojisi (Kurumsal Ofis Bölümlemesi)

Büyük bir şirketin tek bir devasa katında, duvarlar olmadan 500 kişinin aynı açık ofiste çalıştığını düşün (**Single Broadcast Domain**).

İnsan Kaynakları (İK) departmanından biri bağırdığında, o sesi muhasebeci de, yazılımcı da, stajyer de duymak zorunda kalır. Bu durum iki büyük bela yaratır:

1. **Gürültü (Broadcast Trafiği):** Herkes her saniye gereksiz sesleri dinler, bilgisayarların işlemcileri yorulur.
2. **Güvenlik (Security):** İK yöneticisi yan masayla maaş konuşurken stajyer kulak misafiri olabilir.



**Çözüm:** Çözüm olarak o devasa odayı ses geçirmeyen cam panellerle bölmelere ayırırsın (**VLAN**).
* Bir cam odaya "İK" (**VLAN 10**), diğerine "Yazılım" (**VLAN 20**) dersin.
* Artık İK odasında biri bağırdığında o ses camın dışına çıkamaz. Yazılımcılar gürültüden kurtulur.
* Eğer İK ile Yazılımcı konuşmak isterse, aradaki tek kapıdan (yani bir **Router**'dan) geçip kimlik göstermek zorundadırlar.

---

### B) VXLAN Analojisi (Şehirler Arası Gizli Tünel)

Şirket büyüdü; İstanbul ve Ankara'da iki farklı ofis açtı. İstanbul'daki Yazılım ekibi (VLAN 20) ile Ankara'daki Yazılım ekibinin sanki aynı odadaymış, arada hiç internet yokmuş gibi doğrudan konuşmasını istiyorsun.

Ancak iki şehir arasında koca bir otoban (**İnternet / IP Ağları**) var. Otobandaki kurallara göre sadece tırlar (**IP Paketleri**) gidebilir; senin binanın içindeki o küçük el arabaları (**L2 Ethernet Frame'leri**) otobana çıkamaz, yasaktır.



**Çözüm:** İstanbul'daki ofisin kapısına bir paketleme makinesi (**VTEP**) koyarsın. 
1. El arabasını (L2 paketini) alır, üzerine otobanda gitmeye uygun büyük bir kargo kutusu (**IP/UDP paketi**) geçirir. 
2. Kutunun üzerine *"Ankara Ofisindeki Yazılım Bölümüne gitsin"* yazar.
3. Kargo kutusu otobandan (IP ağından) hızla geçer, Ankara'daki kapıya ulaştığında oradaki makine (**VTEP**) kutuyu açar, içinden bizim el arabasını çıkarır ve Ankara'daki yazılımcıya teslim eder. 

İşte bu tünelleme işlemine **VXLAN (Overlay)** denir.
