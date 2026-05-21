# e-commerce-analysis
# E-Commerce Müştəri Analizi

Bu layihə bir onlayn pərakəndə satış şirkətinin 2010–2011-ci illər üzrə əməliyyat məlumatlarını təhlil edir. Məqsəd müştəri davranışını başa düşmək, gəlir strukturunu ortaya qoymaq və strateji qərarlar üçün əsas insight-lər çıxarmaqdır.

---

## Dataset Haqqında

| Xüsusiyyət | Dəyər |
|---|---|
| Fayl | `e_commerce_last.csv` |
| Əhatə etdiyi dövr | Dekabr 2010 – Dekabr 2011 |
| Əməliyyat sayı | 524,878 |
| Unikal müştəri | 4,339 |
| Unikal məhsul | 3,922 |
| Toplam gəlir | £10.6M |
| Sütunlar | `invoiceno`, `stockcode`, `description`, `quantity`, `invoicedate`, `unitprice`, `customerid`, `country`, `season`, `revenue`, `segment` |

---

## Notebook: `e-commer.ipynb`

Notebook məlumatı 6 mərhələdə emal edir:

**1. Yükləmə və standartlaşdırma** — `latin1` encoding ilə CSV oxunur, sütun adları kiçik hərflərə çevrilir, tarix sütunu düzgün formatlanır.

**2. Null dəyərlərin idarəsi** — Müştəri ID-lərinin 27%-i boş idi. Məlumat itkisinin qarşısını almaq üçün silinmək əvəzinə `"unknown"` ilə dolduruldu. Yalnız `description` sütunundakı null-lar silindi.

**3. Dublikatların təmizlənməsi** — Eyni saatda baş verən dublikat əməliyyatlar həqiqi sifariş sayılmadığı üçün çıxarıldı.

**4. Yeni sütunların yaradılması** — `season` (mövsüm) və `revenue = unitprice × quantity` sütunları əlavə edildi. Gəliri sıfır və ya mənfi olan sətirlər silindi.

**5. RFM analizi** — Hər müştəri üçün üç metrik hesablandı:
- **Recency** — son alışdan neçə gün keçib
- **Frequency** — neçə dəfə alıb
- **Monetary** — nə qədər xərcləyib

Hər metrik 1–5 arası qiymətləndirildi və 3 rəqəmli `rfm_score` yarandı. Məsələn, `555` mükəmməl müştərini bildirir.

**6. Seqmentasiya** — RFM skoruna əsasən hər müştəriyə bir seqment təyin edildi:

| Şərt | Seqment |
|---|---|
| `rfm_score == "555"` | Best Customers |
| İlk rəqəm 5 | Recent Customers |
| İkinci rəqəm 5 | Frequent Buyers |
| Üçüncü rəqəm 5 | Big Spenders |
| Digərləri | At Risk |

---

## Əsas Insight-lər

### Müştəri Konsentrasiyası

Cəmi 309 nəfər "Best Customers" bütün gəlirin 50%-ni verir. Top 10% müştəri isə gəlirin 67.7%-ni təmin edir. Bu, biznesin çox az sayda müştəriyə güclü asılılığını göstərir. Bu qrupu qorumaq üçün VIP proqramı, şəxsi xidmət və ya prioritet dəstək mexanizmləri tətbiq edilməlidir.

### At Risk — £1.62M 

2,831 müştəri ortalama 127 gündür, yəni 4 aydan çoxdur, heç bir alış etməyib. Bu, bütün müştəri bazasının 65%-idir. Win-back e-mail kampaniyası, kiçik bir endirim və ya təşviqedici mesaj
bu kütlənin bir hissəsini geri qaytara bilər.

### Birinci Alışdan Sonra Tərk Etmə

Müştərinin 34%-i, yəni 1,493 nəfər yalnız bir dəfə alıb və bir daha qayıtmayıb. İlk alışdan 3–7 gün sonra göndərilən avtomatik e-mail, upsell təklifi və ya məmnuniyyət sorğusu bu rəqəmi əhəmiyyətli dərəcədə yaxşılaşdıra bilər.

### Big Spenders — Gizli Potensial

174 nəfərdən ibarət bu seqment hər əməliyyatda ortalama £64 xərcləyir — bu, ümumi ortalamanın 3 qatıdır. Lakin sifariş tezliyi aşağıdır. Abunəlik modeli, erkən giriş imkanı və ya eksklüziv koleksiya kimi mexanizmlər bu qrupu daha tez-tez alışa yönəldə bilər.

### Beynəlxalq Bazarın Zəif İstifadəsi

37 ölkə mövcuddur, lakin hamısı birlikdə cəmi 15.4% gəlir verir. Almaniya, Fransa və Hollandiya nisbətən yaxşı göstəricilər versə də, hələ çox kiçikdir. Lokallaşdırılmış kampaniya, öz dilində müştəri xidməti və ya regional anbar infrastrukturu bu payı əhəmiyyətli dərəcədə artıra bilər.

### Mövsümilik

Payız gəlirin 35%-ni verir, yay isə ən zəif mövsümdür. Maraqlısı odur ki, ortalama əməliyyat dəyəri mövsümdən asılı olaraq çox dəyişmir — payızda müştəri sadəcə daha çox gəlir, daha çox xərcləmir. Bu səbəbdən yay aylarında trafik artırmağa, payız aylarında isə ortalama sifariş dəyərini yüksəltməyə yönəlik taktikalar daha effektiv olacaq.

---

## Strateji Tövsiyələr

**Kritik** — 2,831 "At Risk" müştəriyə win-back kampaniyası başladılmalıdır. £1.62M saxlanma potensialı var.

**Diqqət** — Müştərinin 34%-i ikinci alış etməyib. İlk alışdan sonra avtomatik iletişim axını qurulmalıdır.

**Fokus** — 309 nəfər "Best Customer" gəlirin 50%-ni verir. VIP proqramı bu layihənin əsas prioriteti olmalıdır.

**Böyümə** — 37 ölkə hələ 15.4% gəlir verir. Almaniya, Fransa, Hollandiya üçün lokallaşdırılmış kampaniya böyük imkandır.

**Məhsul** — White Heart T-Light Holder (2,256 sifariş) və Regency Cakestand (£174K gəlir) həm populyar, həm də gəlirlidir. Bu kateqoriyaya investisiya artırılmalıdır.
