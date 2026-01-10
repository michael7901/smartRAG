# SmartRAG Demo için Docker Kurulum Rehberi

<p align="center">
  <img src="../../icon.svg" alt="SmartRAG Logo" width="100" height="100">
</p>

<p align="center">
  <b>Tüm yerel servislerle SmartRAG Demo için tam Docker kurulumu</b>
</p>

<p align="center">
  <a href="../../README.tr.md"><img src="https://img.shields.io/badge/📚-Ana_README-blue?style=for-the-badge&logo=book" alt="Ana README"/></a>
  <a href="README-Docker.md"><img src="https://img.shields.io/badge/🇺🇸-English_README-blue?style=for-the-badge" alt="English README"/></a>
</p>

---

Docker kullanarak tüm yerel servislerle SmartRAG'ı çalıştırmak için tam rehber.

## 🐳 Docker Compose Konfigürasyonu

### **Servis Detayları**

| Servis | Image | Container | Portlar | Volume | Sağlık Kontrolü |
|---------|-------|-----------|-------|--------|--------------|
| **SQL Server** | `mcr.microsoft.com/mssql/server:2022-latest` | `smartrag-sqlserver-test` | `1433:1433` | `sqlserver-data` | SQL sorgu testi |
| **MySQL** | `mysql:8.0` | `smartrag-mysql-test` | `3306:3306` | `mysql-data` | MySQL ping testi |
| **PostgreSQL** | `postgres:16-alpine` | `smartrag-postgres-test` | `5432:5432` | `postgres-data` | PostgreSQL hazır testi |
| **Ollama** | `ollama/ollama:latest` | `smartrag-ollama` | `11434:11434` | `ollama-data` | Ollama liste testi |
| **Qdrant** | `qdrant/qdrant:latest` | `smartrag-qdrant` | `6333:6333`, `6334:6334` | `qdrant-data` | HTTP sağlık kontrolü |
| **Redis** | `redis/redis-stack-server:latest` | `smartrag-redis` | `6379:6379`, `8001:8001` | `redis-data` | Redis ping testi |

### **Volume Kalıcılığı**
Tüm veriler Docker volume'ları kullanarak container yeniden başlatmalarında korunur:
- `sqlserver-data` - SQL Server veritabanı dosyaları
- `mysql-data` - MySQL veritabanı dosyaları  
- `postgres-data` - PostgreSQL veritabanı dosyaları
- `ollama-data` - Ollama AI modelleri ve önbellek
- `qdrant-data` - Qdrant vektör veritabanı dosyaları
- `redis-data` - Redis önbellek ve kalıcılık dosyaları

### **Sağlık Kontrolleri**
Tüm servisler otomatik yeniden başlatma ile sağlık kontrolleri içerir:
- **Aralık**: 10-30 saniye (servise bağlı)
- **Zaman Aşımı**: 5-10 saniye
- **Deneme**: 3-5 deneme
- **Yeniden Başlatma Politikası**: `unless-stopped`

---

## 📦 Ne Kurulur

Bu Docker kurulumu tam SmartRAG fonksiyonalitesi için **6 servis** içerir:

### AI & Depolama
- **Ollama** - Yerel AI modelleri (LLaMA, Phi, Mistral, vb.)
- **Qdrant** - Embedding'ler için vektör veritabanı
- **Redis** - Önbellek ve vektör depolama

### Veritabanları
- **SQL Server 2022 Express** - Demo verisi için birincil veritabanı
- **MySQL 8.0** - Demo verisi için ikincil veritabanı  
- **PostgreSQL 16** - Demo verisi için üçüncül veritabanı

## 🚀 Hızlı Başlangıç

**Neden Docker?** Sıfır konfigürasyonla dakikalar içinde SmartRAG'ı çalıştırın. Tüm veritabanları, AI modelleri ve vektör depoları önceden yapılandırılmış ve kullanıma hazır.

### 1. Ortam Kurulumu (Önerilen)

Daha iyi güvenlik için varsayılan şifreler yerine ortam değişkenlerini kullanın:

```bash
# Örnek dosyayı kopyala
cp env.example .env

# Güvenli şifrelerle .env dosyasını düzenle
nano .env  # veya tercih ettiğiniz editörü kullanın
```

**Örnek .env dosyası:**
```bash
# SQL Server Konfigürasyonu
SQLSERVER_SA_PASSWORD=YourSecurePassword123!

# MySQL Konfigürasyonu  
MYSQL_ROOT_PASSWORD=YourMySQLPassword456!

# PostgreSQL Konfigürasyonu
POSTGRES_PASSWORD=YourPostgresPassword789!
```

### 2. Tüm Servisleri Başlat

```bash
cd examples/SmartRAG.Demo
docker-compose up -d
```

Tüm servislerin başlaması için **10-15 saniye** bekleyin.

### 3. Servisleri Doğrula

```bash
docker-compose ps
```

Tüm 6 container'ın "healthy" durumunda çalıştığını görmelisiniz.

### 4. Ollama Modellerini Kur

```bash
# LLaMA 3.2'yi indir (ana AI modeli)
docker exec -it smartrag-ollama ollama pull llama3.2

# Embedding modelini indir (RAG için gerekli)
docker exec -it smartrag-ollama ollama pull nomic-embed-text
```

**Not**: İlk indirme internet hızınıza bağlı olarak 5-10 dakika sürebilir.

### 5. Uygulamayı Çalıştır

```bash
dotnet run
```

İstendiğinde **"1. LOCAL Environment"** seçeneğini seçin.

## 🔧 Bireysel Servis Yönetimi

### Bireysel Servisleri Başlat

```bash
# AI Servisleri
docker-compose up -d smartrag-ollama
docker-compose up -d smartrag-qdrant
docker-compose up -d smartrag-redis

# Veritabanları
docker-compose up -d smartrag-sqlserver
docker-compose up -d smartrag-mysql
docker-compose up -d smartrag-postgres
```

### Servisleri Durdur

```bash
# Tümünü durdur
docker-compose stop

# Belirli servisi durdur
docker-compose stop smartrag-ollama
```

### Servisleri Yeniden Başlat

```bash
# Tümünü yeniden başlat
docker-compose restart

# Belirli servisi yeniden başlat
docker-compose restart smartrag-qdrant
```

### Logları Görüntüle

```bash
# Tüm servisler
docker-compose logs

# Belirli servis
docker-compose logs smartrag-ollama
docker-compose logs smartrag-qdrant
docker-compose logs smartrag-redis

# Logları takip et (gerçek zamanlı)
docker-compose logs -f smartrag-ollama
```

## 📊 Bağlantı Detayları

### Ollama (AI)
- **Endpoint**: http://localhost:11434
- **API**: http://localhost:11434/api/tags
- **Container**: smartrag-ollama
- **Volume**: ollama-data

**Bağlantı Testi:**
```bash
curl http://localhost:11434/api/tags
```

### Qdrant (Vektör Veritabanı)
- **HTTP**: http://localhost:6333
- **gRPC**: localhost:6334
- **Dashboard**: http://localhost:6333/dashboard
- **Container**: smartrag-qdrant
- **Volume**: qdrant-data

**Bağlantı Testi:**
```bash
curl http://localhost:6333/health
```

### Redis (Önbellek & Vektör Deposu)
- **Host**: localhost:6379
- **RedisInsight UI**: http://localhost:8001 (opsiyonel web arayüzü)
- **Container**: smartrag-redis
- **Volume**: redis-data
- **Kalıcılık**: AOF (Append Only File) etkin
- **RediSearch**: Dahil (vektör benzerlik araması etkin)

**Bağlantı Testi:**
```bash
docker exec -it smartrag-redis redis-cli ping
# PONG döndürmeli

# RediSearch modülünü doğrula
docker exec -it smartrag-redis redis-cli MODULE LIST
# search modülünü göstermeli
```

### SQL Server
- **Host**: localhost,1433
- **Kullanıcı Adı**: sa
- **Şifre**: `${SQLSERVER_SA_PASSWORD}` (ortam değişkeni gerekli)
- **Veritabanı**: PrimaryDemoDB (uygulama tarafından oluşturulur)
- **Container**: smartrag-sqlserver-test
- **Volume**: sqlserver-data

**Bağlantı Testi:**
```bash
# Ortam değişkeni kullanarak
export SQLSERVER_SA_PASSWORD="your_secure_password"
docker exec -it smartrag-sqlserver-test /opt/mssql-tools18/bin/sqlcmd -S localhost -U sa -P $SQLSERVER_SA_PASSWORD -C -Q "SELECT @@VERSION"
```

### MySQL
- **Host**: localhost:3306
- **Kullanıcı Adı**: root
- **Şifre**: `${MYSQL_ROOT_PASSWORD}` (ortam değişkeni gerekli)
- **Veritabanı**: SecondaryDemoDB (uygulama tarafından oluşturulur)
- **Container**: smartrag-mysql-test
- **Volume**: mysql-data

**Bağlantı Testi:**
```bash
docker exec -it smartrag-mysql-test mysql -u root -p${MYSQL_ROOT_PASSWORD} -e "SELECT VERSION()"
```

### PostgreSQL
- **Host**: localhost:5432
- **Kullanıcı Adı**: postgres
- **Şifre**: `${POSTGRES_PASSWORD}` (ortam değişkeni gerekli)
- **Veritabanı**: TertiaryDemoDB (uygulama tarafından oluşturulur)
- **Container**: smartrag-postgres-test
- **Volume**: postgres-data

**Bağlantı Testi:**
```bash
docker exec -it smartrag-postgres-test psql -U postgres -c "SELECT version()"
```

## 🤖 Ollama Model Yönetimi

### Kurulu Modelleri Listele

```bash
docker exec -it smartrag-ollama ollama list
```

### Model İndir/Çek

```bash
# Önerilen modeller
docker exec -it smartrag-ollama ollama pull llama3.2        # Ana AI (3.2GB)
docker exec -it smartrag-ollama ollama pull llama3.2:1b     # Hafif (1.3GB)
docker exec -it smartrag-ollama ollama pull nomic-embed-text # Embedding'ler (274MB)
docker exec -it smartrag-ollama ollama pull phi3            # Microsoft Phi (2.3GB)
docker exec -it smartrag-ollama ollama pull mistral         # Mistral 7B (4.1GB)
docker exec -it smartrag-ollama ollama pull qwen2.5         # Alibaba Qwen (4.7GB)
```

### Modelleri Kaldır

```bash
docker exec -it smartrag-ollama ollama rm llama3.2
```

### Model Bilgilerini Kontrol Et

```bash
docker exec -it smartrag-ollama ollama show llama3.2
```

## 💾 Veri Kalıcılığı

Tüm veriler Docker volume'larında saklanır ve container yeniden başlatmalarında korunur.

### Volume'ları Listele

```bash
docker volume ls | grep smartrag
```

### Volume'u İncele

```bash
docker volume inspect smartrag-localdemo_ollama-data
docker volume inspect smartrag-localdemo_qdrant-data
docker volume inspect smartrag-localdemo_redis-data
```

### Veriyi Yedekle

```bash
# Ollama modellerini yedekle
docker run --rm -v smartrag-localdemo_ollama-data:/data -v $(pwd):/backup alpine tar czf /backup/ollama-backup.tar.gz -C /data .

# Qdrant'ı yedekle
docker run --rm -v smartrag-localdemo_qdrant-data:/data -v $(pwd):/backup alpine tar czf /backup/qdrant-backup.tar.gz -C /data .
```

### Veriyi Geri Yükle

```bash
# Ollama'yı geri yükle
docker run --rm -v smartrag-localdemo_ollama-data:/data -v $(pwd):/backup alpine sh -c "cd /data && tar xzf /backup/ollama-backup.tar.gz"
```

## 🧹 Temizlik

### Container'ları Kaldır (Veriyi Koru)

```bash
docker-compose down
```

### Her Şeyi Kaldır (⚠️ Veri Dahil)

```bash
docker-compose down -v
```

### Belirli Volume'u Kaldır

```bash
docker volume rm smartrag-localdemo_ollama-data
```

### Kullanılmayan Docker Kaynaklarını Temizle

```bash
docker system prune -a
```

## 🔧 Sorun Giderme

### Port Zaten Kullanımda

Bir port meşgulse, `docker-compose.yml` dosyasında değiştirin:

```yaml
# Örnek: Ollama portunu 11434'ten 11435'e değiştir
ollama:
  ports:
    - "11435:11434"  # Host:Container
```

Sonra `appsettings.Development.json` dosyasını güncelleyin:
```json
{
  "AI": {
    "Custom": {
      "Endpoint": "http://localhost:11435"
    }
  }
}
```

### Container Başlamıyor

```bash
# Docker'ın çalıştığını kontrol et
docker --version

# Container loglarını kontrol et
docker logs smartrag-ollama

# Docker Desktop'ı yeniden başlat
# (Windows'ta bazen gerekli)
```

### Disk Alanı Doldu

```bash
# Docker disk kullanımını kontrol et
docker system df

# Kullanılmayan görüntüleri kaldır
docker image prune -a

# Kullanılmayan volume'ları kaldır
docker volume prune
```

### Sağlık Kontrolü Başarısız

```bash
# Sağlık durumunu kontrol et
docker inspect smartrag-ollama | grep -A 10 Health

# Sağlık endpoint'ini manuel olarak test et
curl http://localhost:11434/api/tags   # Ollama
curl http://localhost:6333/health      # Qdrant
docker exec -it smartrag-redis redis-cli ping  # Redis
```

### Ollama Model İndirme Takıldı

```bash
# Ollama loglarını kontrol et
docker logs -f smartrag-ollama

# Ollama'yı yeniden başlat
docker-compose restart ollama

# Tekrar indirmeyi dene
docker exec -it smartrag-ollama ollama pull llama3.2
```

## 📊 Kaynak Kullanımı

### Tipik Kaynak Tüketimi

| Servis | RAM | Disk |
|---------|-----|------|
| Ollama (boşta) | ~200MB | ~500MB temel |
| Ollama + llama3.2 | ~2-4GB | ~3.2GB |
| Ollama + birden fazla model | ~4-8GB | ~10-15GB |
| Qdrant | ~100MB | ~50MB + veri |
| Redis | ~50MB | ~10MB + önbellek |
| SQL Server | ~2GB | ~500MB |
| MySQL | ~300MB | ~200MB |
| PostgreSQL | ~50MB | ~100MB |

**Önerilen Sistem:**
- **RAM**: Minimum 8GB, önerilen 16GB
- **Disk**: 20GB boş alan
- **CPU**: AI çıkarımı için 4+ çekirdek önerilen

### Kaynak Kullanımını İzle

```bash
# Gerçek zamanlı istatistikler
docker stats

# Belirli container'ı kontrol et
docker stats smartrag-ollama
```

## 🔒 Güvenlik Notları

⚠️ **Bu kurulum sadece yerel geliştirme/test için!**

- Varsayılan şifreler kasıtlı olarak basit
- Servisler sadece localhost'ta açık
- **ASLA bu konfigürasyonları üretimde kullanmayın**
- **ASLA bu portları internete açmayın**

### Güvenlik için Ortam Değişkenleri

Daha iyi güvenlik için hardcoded şifreler yerine ortam değişkenlerini kullanın:

```bash
# Güvenli şifreler belirle
export SQLSERVER_SA_PASSWORD="YourSecurePassword123!"
export MYSQL_ROOT_PASSWORD="YourMySQLPassword456!"
export POSTGRES_PASSWORD="YourPostgresPassword789!"

# Özel şifrelerle başlat
docker-compose up -d
```

Üretim dağıtımları için:
- Ortam değişkenleri ile güçlü, benzersiz şifreler kullanın
- TLS/SSL şifrelemesini etkinleştirin
- Uygun kimlik doğrulama kullanın
- Güvenlik duvarının arkasında çalıştırın
- Hassas veriler için Docker secret'ları kullanın

## 🌐 Alternatif: Manuel Kurulum

Docker kullanmayı tercih etmiyorsanız:

### Ollama
- İndir: https://ollama.ai
- Yerel olarak kur
- Modeller şurada saklanır: `~/.ollama/models`

### Qdrant
- İndir: https://qdrant.tech/documentation/guides/installation/
- Veya Qdrant Cloud kullan: https://cloud.qdrant.io

### Redis
- **Önemli**: Vektör arama için `redis/redis-stack-server` kullanın (RediSearch modülü dahil)
- Docker: `docker run -d -p 6379:6379 redis/redis-stack-server:latest`
- Windows: https://github.com/microsoftarchive/redis/releases (RediSearch modülü kurulumu gerekli)
- Linux: `sudo apt install redis-server` (RediSearch modülü kurulumu gerekli)
- macOS: `brew install redis` (RediSearch modülü kurulumu gerekli)
- **Not**: Standart Redis RediSearch içermez. RediSearch olmadan sadece metin araması çalışır (vektör benzerlik araması çalışmaz).

## 📚 Ek Kaynaklar

- **Ollama Dokümantasyonu**: https://ollama.ai/docs
- **Qdrant Dokümantasyonu**: https://qdrant.tech/documentation/
- **Redis Dokümantasyonu**: https://redis.io/documentation

## 🤝 İletişim

Sorunlar veya sorular için:
- **LinkedIn**: https://www.linkedin.com/in/barisyerlikaya/
- **NuGet**: https://www.nuget.org/packages/SmartRAG
- **Email**: b.yerlikaya@outlook.com
