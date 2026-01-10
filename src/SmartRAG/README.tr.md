# SmartRAG

**.NET için Çoklu Veritabanı RAG Kütüphanesi**  
Verileriniz hakkında doğal dil ile sorular sorun

SmartRAG, birden fazla veritabanını, belgeyi, görüntüyü ve ses dosyasını doğal dil kullanarak sorgulamanıza olanak tanıyan kapsamlı bir Retrieval-Augmented Generation (RAG) kütüphanesidir. Verilerinizi tek, birleşik bir API ile akıllı konuşmalara dönüştürün.

## 🚀 Hızlı Başlangıç

### Kurulum

```bash
dotnet add package SmartRAG
```

### Temel Kurulum

```csharp
// Web API uygulamaları için
builder.Services.AddSmartRag(builder.Configuration, options =>
{
    options.AIProvider = AIProvider.OpenAI;
    options.StorageProvider = StorageProvider.InMemory;
});

// Konsol uygulamaları için
var serviceProvider = services.UseSmartRag(
    configuration,
    aiProvider: AIProvider.OpenAI,
    storageProvider: StorageProvider.InMemory
);
```

### Yapılandırma

Veritabanı bağlantılarını `appsettings.json` dosyanıza ekleyin:

```json
{
  "SmartRAG": {
    "DatabaseConnections": [
      {
        "Name": "Satış",
        "ConnectionString": "Server=localhost;Database=Satis;...",
        "DatabaseType": "SqlServer"
      }
    ]
  }
}
```

### Kullanım Örneği

```csharp
// Belge yükle
var belge = await documentService.UploadDocumentAsync(
    dosyaStream, dosyaAdi, icerikTipi, "kullanici-123"
);

// Veritabanları, belgeler, görüntüler ve ses dosyalarında birleşik sorgu
var cevap = await searchService.QueryIntelligenceAsync(
    "Son çeyrekte 10.000 TL üzeri alışveriş yapan tüm müşterileri, ödeme geçmişlerini ve verdikleri şikayet veya geri bildirimleri göster"
);
// → AI otomatik olarak sorgu intent'ini analiz eder ve akıllıca yönlendirir:
//   - Yüksek güven + veritabanı sorguları → Sadece veritabanlarını arar
//   - Yüksek güven + belge sorguları → Sadece belgeleri arar
//   - Orta güven → Hem veritabanlarını hem belgeleri arar, sonuçları birleştirir
// → SQL Server (siparişler), MySQL (ödemeler), PostgreSQL (müşteri verileri) sorgular
// → Yüklenen PDF sözleşmelerini, OCR ile taranmış faturaları ve transkript edilmiş çağrı kayıtlarını analiz eder
// → Tüm kaynaklardan birleşik cevap sağlar
```

## ✨ Temel Özellikler

🎯 **Birleşik Sorgu Zekası** - Tek sorgu ile veritabanları, belgeler, görüntüler ve ses dosyalarını otomatik olarak arar  
🧠 **Akıllı Hibrit Yönlendirme** - AI sorgu intent'ini analiz eder ve optimal arama stratejisini otomatik belirler  
🗄️ **Multi-Database RAG** - Birden fazla veritabanını doğal dil ile aynı anda sorgula  
📄 **Çoklu Modal Zeka** - PDF, Word, Excel, Görüntü (OCR), Ses (Konuşma-Metin), ve daha fazlası  
🔌 **MCP Client Entegrasyonu** - Harici MCP sunucularına bağlan ve dış araçlarla yetenekleri genişlet  
📁 **Otomatik Dosya İzleme** - Klasörleri izle ve yeni belgeleri manuel yükleme olmadan otomatik indeksle  
🏠 **%100 Yerel İşleme** - Ollama ve Whisper.net ile GDPR, KVKK, HIPAA uyumlu  
🚀 **Üretim Hazır** - Kurumsal kalite, thread-safe, yüksek performans

## 📊 Desteklenen Veri Kaynakları

**Veritabanları:** SQL Server, MySQL, PostgreSQL, SQLite  
**Belgeler:** PDF, Word, Excel, PowerPoint, Görüntü, Ses  
**AI Modelleri:** OpenAI, Anthropic, Gemini, Azure OpenAI, Ollama (yerel), LM Studio  
**Vektör Depoları:** Qdrant, Redis, InMemory  
**Konuşma Depolama:** Redis, SQLite, FileSystem, InMemory (belge depolamadan bağımsız)  
**Harici Entegrasyonlar:** MCP (Model Context Protocol) sunucuları ile genişletilmiş araç yetenekleri  
**Dosya İzleme:** Gerçek zamanlı belge indeksleme ile otomatik klasör izleme

## 🎯 Gerçek Dünya Kullanım Senaryoları

### Bankacılık - Müşteri Finansal Profili
```csharp
var cevap = await searchService.QueryIntelligenceAsync(
    "Vadesi geçmiş ödemeleri olan müşteriler hangileri ve toplam borç bakiyeleri nedir?"
);
// → Müşteri DB, Ödeme DB, Hesap DB'yi sorgular ve sonuçları birleştirir
// → Kredi kararları için kapsamlı finansal risk değerlendirmesi sağlar
```

### Sağlık - Hasta Bakım Yönetimi
```csharp
var cevap = await searchService.QueryIntelligenceAsync(
    "Son 6 ayda HbA1c kontrolü yaptırmamış diyabet hastalarını göster"
);
// → Hasta DB, Laboratuvar Sonuçları DB, Randevu DB'yi birleştirir ve risk altındaki hastaları belirler
// → Önleyici bakım uyumunu sağlar ve komplikasyonları azaltır
```

### Envanter - Tedarik Zinciri Optimizasyonu
```csharp
var cevap = await searchService.QueryIntelligenceAsync(
    "Stoku azalan ürünler hangileri ve hangi tedarikçiler bunları en hızlı şekilde yenileyebilir?"
);
// → Envanter DB, Tedarikçi DB, Sipariş Geçmişi DB'yi analiz eder ve yeniden stoklama önerileri sağlar
// → Stok tükenmesini önler ve tedarik zinciri verimliliğini optimize eder
```
