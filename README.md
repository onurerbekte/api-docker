# Stok API Docker paketi / Inventory API Docker package
**Kurgusal demo proje / Fictional demo project.**

## Türkçe
c) projesindeki FastAPI + SQLite API'nin test/Docker sürümü. Aynı API kaynak kodu ve testleri bu klasörde bağımsız Git deposuna kopyalandı; yeni bir ticari müşteri işi değildir. Çok aşamalı Dockerfile, testlerin başarılı olmasını runtime üretiminin ön koşulu yapar. Runtime root olmayan kullanıcıyla çalışır. Compose SQL verisini named volume'da korur ve yalnızca yerel 127.0.0.1 portuna açılır.

Docker Desktop ve Compose kurulu/çalışır olduğunda:
```powershell
docker compose config
docker build --target test -t inventory-api-test .
docker compose up --build -d --wait
```
API: `http://127.0.0.1:8000/docs`; sağlık: `/health`. Durdurma: `docker compose down` (volume korunur). Veriyi saklamak istiyorsan `down -v` kullanma; volume'u siler. Bu ortamda Docker kurulu olmadığından bu komutların çalışması doğrulanmadı. Yerel Python testi:
```powershell
python -m pip install -r requirements-dev.txt
python -m pytest -q
```
`.github/workflows/ci.yml` GitHub'a sen yükleyince test, Compose yapılandırması, image derlemesi ve health smoke kontrolü çalıştırır; burada çalıştırılmadı. Docker `.dockerignore` ve Git `.gitignore` anahtar, token dosyaları, ortam ve veritabanlarını dışlar. Kimlik doğrulama yok; public üretim servisi değildir.

## English
The test/Docker version of project c's FastAPI + SQLite API. Identical source and tests are copied into this independent Git folder; this is not a separate commercial client engagement. Multi-stage Docker builds require passing tests before producing the non-root runtime. Compose persists SQL data in a named volume and binds the host port to 127.0.0.1 only.

With Docker Desktop/Compose running, use the commands above and open `/docs` on port 8000. Stop using `docker compose down` to preserve the volume; `down -v` removes stored data. Docker was unavailable in this environment, so container execution remains unverified. Local Python tests can be run using the dev requirements.

The CI workflow will test Python, validate Compose, build/run the image, and smoke-test health after you upload the repository. It was not run here. Git and Docker ignore local secrets, environment files, and databases. No authentication; not a public production service.
