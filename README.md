# 📄 IBM Granite ile Belge Özetleme

**IBM Granite 4.0** ile çalışan, hiyerarşik parçalama ve iki aşamalı özetleme stratejisi kullanarak bölüm bazlı ve kitap genelinde özetler üreten bir belge özetleme pipeline'ı.

---

## 🧠 Genel Bakış

Bu proje şunları göstermektedir:

1. [Docling](https://github.com/DS4SD/docling) kullanarak uzun bir belgeyi (örn. Project Gutenberg'den bir kitap) **indirme ve ayrıştırma**
2. Başlık yapısını koruyarak belgeyi **hiyerarşik olarak parçalara ayırma**
3. Replicate üzerinden IBM Granite kullanarak **her bölümü ayrı ayrı özetleme**
4. Bölüm özetlerini **tek bir birleşik kitap özetinde birleştirme**

Örnek olarak Henry David Thoreau'nun *Walden* adlı eseri kullanılmıştır; ancak pipeline, Docling tarafından desteklenen herhangi bir HTML veya belge kaynağıyla çalışır.

---

## 🏗️ Mimari

```
Belge URL'si
    │
    ▼
DocumentConverter (Docling)
    │
    ▼
HierarchicalChunker ──► chunk_document()
    │
    ▼
merge_chunks()  ──► Parçaları başlığa göre gruplar (bölüm bazında)
    │
    ▼
IBM Granite (bölüm başına) ──► Bölüm Özetleri
    │
    ▼
IBM Granite (tüm özetler) ──► Birleşik Kitap Özeti
```

---

## ✅ Gereksinimler

- Python 3.9+
- Bir [Replicate](https://replicate.com/) API anahtarı
- Project Gutenberg'den belge indirebilmek için internet bağlantısı

---

## 🚀 Başlarken

### 1. Bağımlılıkları yükle

```bash
pip install uv
uv pip install git+https://github.com/ibm-granite-community/utils.git \
    transformers \
    'langchain_replicate @ git+https://github.com/ibm-granite-community/langchain-replicate.git' \
    docling
```

### 2. Replicate API anahtarını ayarla

`token` ortam değişkenini Replicate API anahtarınla ayarla:

```bash
export token=replicate_api_anahtarin
```

### 3. Notebook'u veya scripti çalıştır

**Jupyter Notebook:**
```bash
jupyter notebook Summarize_Documents_with_IBM_Granite.ipynb
```

**Python scripti:**
```bash
python summarize_documents_with_ibm_granite.py
```

---

## ⚙️ Yapılandırma

| Parametre | Varsayılan | Açıklama |
|-----------|------------|----------|
| `model_path` | `ibm-granite/granite-4.0-h-small` | Replicate üzerinden kullanılacak Granite modeli |
| `max_tokens` | `2000` | Üretim başına maksimum token sayısı |
| `min_tokens` | `200` | Üretim başına minimum token sayısı |
| `time.sleep` | `10 saniye` | API çağrıları arasındaki bekleme süresi (hız sınırı) |

Farklı belge bölümlerini veya başlık seviyelerini hedeflemek için `chunk_dropwhile`, `chunk_takewhile` ve `chunk_headings` fonksiyonlarını düzenleyebilirsin.

---

## 🧪 Test Modu

Hızlı doğrulama için `GRANITE_TESTING` ortam değişkenini `true` olarak ayarlayarak işlemi yalnızca ilk 5 belgeyle sınırlayabilirsin — bu sayede API kotası gereksiz yere harcanmaz:

```bash
export GRANITE_TESTING=true
```

---

## 📁 Proje Yapısı

```
.
├── Summarize_Documents_with_IBM_Granite.ipynb   # Jupyter notebook versiyonu
├── summarize_documents_with_ibm_granite.py      # Python script versiyonu
└── README.md
```

---

## 🔗 Bağımlılıklar ve Kaynaklar

- [IBM Granite](https://huggingface.co/ibm-granite) — Özetleme için temel model
- [Replicate](https://replicate.com/) — Model çıkarım API'si
- [LangChain Replicate](https://github.com/ibm-granite-community/langchain-replicate) — Replicate için LangChain entegrasyonu
- [Docling](https://github.com/DS4SD/docling) — Belge ayrıştırma ve parçalama
- [IBM Granite Community Utils](https://github.com/ibm-granite-community/utils) — Notebook araçları ve prompt şablonları
- Kaynak metin: [Walden — Project Gutenberg](https://www.gutenberg.org/ebooks/205)

---

## 📜 Lisans

Bu proje eğitim ve araştırma amaçlıdır. Kullanılan her bağımlılığın ve kaynak metnin kendi lisanslarına lütfen ayrıca göz atınız.
