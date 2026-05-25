# 🎓 Türkiye Student Evaluation — Veri Madenciliği Analizi

Bu proje, UCI Machine Learning Repository'de yayımlanan **Türkiye Student Evaluation** veri seti üzerinde Orange Data Mining aracıyla gerçekleştirilen üç farklı veri madenciliği analizini kapsamaktadır.

---

## 📊 Veri Seti

| Özellik | Değer |
|---|---|
| Kaynak | [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/262/turkiye+student+evaluation) |
| Kurum | Gazi Üniversitesi |
| Örnek Sayısı | 5.820 |
| Değişken Sayısı | 33 |
| Hedef Değişken | `difficulty` (1–5 kategorik) |

**Değişkenler:**
- `Q1–Q12` → Ders içeriği değerlendirme soruları (Likert, 1–5)
- `Q13–Q28` → Eğitmen performansı değerlendirme soruları (Likert, 1–5)
- `instr` → Eğitmen kodu (kategorik)
- `class` → Ders kodu (kategorik)
- `nb.repeat` → Dersin kaçıncı kez alındığı
- `attendance` → Devam durumu

---

## 🔍 Analizler

### 1. Kümeleme — K-Means
- **Ön işleme:** One-hot encoding + Z-score standardizasyon
- **k=2, 3, 4** için silhouette analizi
- **Seçilen k:** 3 — χ²=290.52, p<0.001
- **Bulgu:** C2 kümesi belirgin şekilde "kolay algılayan öğrenciler"den oluşmakta; C1 ve C3 karma zorluk profillerine sahip

### 2. Sınıflandırma — Random Forest
- **Parametreler:** 100 ağaç, replicable training, class balancing, 5-fold CV
- **Sonuçlar:**

| Metrik | Değer |
|---|---|
| AUC | 0.727 |
| CA | 0.468 |
| F1 | 0.468 |
| MCC | 0.305 |

- **Önemli bulgu:** Devamsızlık (attendance) zorluk algısını belirleyen en güçlü faktör (Gain ratio=0.330); Q sorularından ilk giren Q17 (eğitmen dakikliği, 0.027)

### 3. Birliktelik Kuralları - Apriori
- **Ön işleme:** Equal frequency discretization, 3 aralık
- **Parametreler:** Min. destek=%10, Min. güven=%60
- **Üretilen kural sayısı:** 10.000
- **Öne çıkan örüntü:** `nb.repeat=1` + yüksek eğitmen puanları birlikteliği (Conf=0.989, Lift≈2.0)
- **Bulgu:** Dersi ilk kez alan öğrenciler eğitmeni sistematik olarak daha olumlu değerlendiriyor

---

## 📁 Dosyalar

```
├── README.md
├── student.ows                    # Orange workflow dosyası
└── report/
    ├── report.pdf                 # Analiz raporu
    ├── k-mean.pdf                 # Kümeleme analizi çıktısı
    ├── random_forrest.pdf         # Sınıflandırma analizi çıktısı
    └── assoc_rule.pdf             # Birliktelik kuralları çıktısı
```

---

## 🛠️ Kullanılan Araçlar

- [Orange Data Mining](https://orangedatamining.com/) — görsel veri madenciliği
- [UCI Machine Learning Repository](https://archive.ics.uci.edu/) — veri kaynağı

---

## ▶️ Nasıl Çalıştırılır?

1. [Orange'ı indirin](https://orangedatamining.com/download/)
2. `analysis.ows` dosyasını Orange'da açın
3. File widget'ında veri setinin yolunu güncelleyin
4. Workflow otomatik çalışır

---

## 📌 Temel Bulgular

> Devamsızlık, öğrencinin dersi zorluk algısını belirleyen **en kritik faktördür** — öğretim görevlisi veya ders içeriği sorularından yaklaşık 4 kat daha etkili.

> Dersi **ilk kez alan öğrenciler** öğretim görevlisini sistematik olarak daha olumlu değerlendirmektedir. Bu bulgu, tekrar eden öğrencilerin daha eleştirel bir bakış açısıyla değerlendirme yaptığına işaret etmektedir.

> K-Means kümeleme, öğrenci profillerinin **tek boyutlu olmadığını** ortaya koymaktadır — derse, eğitmene ve devam durumuna göre anlamlı alt gruplar oluşmaktadır.

---

## 👤 İletişim

**Uğur Keskin**
