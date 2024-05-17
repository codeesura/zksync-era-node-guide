# zkSync Era Node Kurulum Rehberi

Bu rehber, Ubuntu Linux üzerinde `zkSync Era` düğümünü nasıl kuracağınızı adım adım açıklamaktadır.

## Sistem Gereksinimleri

- CPU: A relatively modern CPU is recommended.
- RAM: 32 GB
- Storage: 300 GB, with the state growing about 1TB per month.
- Network: 100 Mbps connection (1 Gbps+ recommended)

## Adım 1: Visual Studio Code Kurulumu (Yerel Bilgisayarda)

1. **Visual Studio Code'u İndirin ve Kurun:**
   [Visual Studio Code İndirme Sayfası](https://code.visualstudio.com/)

   - İndirme tamamlandığında, kurulum sihirbazını takip ederek Visual Studio Code'u kurun.

2. **SSH Eklentisini Kurun:**
   - Visual Studio Code'u açın.
   - Sol kenar çubuğunda `Extensions` (Uzantılar) sekmesine tıklayın.
   - Arama çubuğuna `Remote - SSH` yazın ve Microsoft tarafından geliştirilen uzantıyı kurun.

## Adım 2: Sunucuya SSH ile Bağlanma (Yerel Bilgisayardan)

1. **Visual Studio Code'da SSH ile Bağlanın:**
   - Visual Studio Code'da `Ctrl+Shift+P` tuşlarına basın ve `Remote-SSH: Connect to Host...` seçeneğini seçin.
   - Açılan metin kutusuna sunucu adresinizi `ssh root@ip_adresi` formatında girin.
   - Şifre veya SSH anahtarınızı girerek bağlantıyı tamamlayın.

   **Not:** `ip_adresi` yerine sunucunuzun gerçek IP adresini yazmalısınız.

2. **Bağlantı Doğrulaması:**
   - Bağlantı başarılı olursa, Visual Studio Code penceresinin sol alt köşesinde bağlı olduğunuz sunucunun adı görünecektir.


3. **Terminali Açma:**
   - Visual Studio Code'da sunucuya bağlandıktan sonra, ekranın üst menüsünde `Terminal` > `New Terminal` seçeneğine tıklayarak terminali açabilirsiniz.
   - Alternatif olarak, klavyenizde `Ctrl+` tuşlarına basarak terminali açabilirsiniz (Ctrl ve sola eğik çizgi tuşu, ESC tuşunun hemen altında bulunur).

## Adım 3: Docker ve Docker Compose Kurulumu (Sunucuda)

Sunucuya başarıyla bağlandıktan sonra, bu işlemleri ve kod bloklarını sunucuda çalıştıracağız.

### Docker Kurulumu

1. **Sistem Paketlerini Güncelleyin:**
   ```sh
   sudo apt update
   ```

2. **Docker'ı Kurun:**
    ```sh
    sudo apt install docker.io
    ```

3. **Docker Servisinin Çalıştığını Doğrulayın:**
    ```sh
    sudo systemctl status docker
    ```

4. **Docker Kurulumunu Doğrulayın:**
    ```sh
    docker --version
    ```

### Docker Compose Kurulumu

1. **Docker Compose’u İndirin:**
    ```sh
    sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    ```

2. **Docker Compose’a Çalıştırma İzni Verin:**
    ```sh
    sudo chmod +x /usr/local/bin/docker-compose
    ```

3. **Kurulumu Doğrulayın:**
    ```sh
    docker-compose --version
    ```

## Adım 4: zkSync Era Düğümünü Başlatma (Sunucuda)

1. **Git'i Yükleyin (Eğer yüklü değilse):**
    ```sh
   sudo apt install git
   ```

2. **Depoyu Klonlayın:**
    ```sh
   git clone https://github.com/matter-labs/zksync-era.git
   ```

3. **Klonlanan Dizinlere Gidin:**
   ```sh
   cd zksync-era/docs/guides/external-node
   cd docker-compose-examples
   ```

4. **Docker Compose ile Düğümü Başlatın:**
   ```sh
   docker-compose -f mainnet-external-node-docker-compose.yml up -d
   ```

5. **Konteynerlerin Çalıştığını Doğrulayın:**
   ```sh
   docker ps
   ```

   Bu komut, şu anda çalışan Docker konteynerlerini listeler. `zkSync Era` düğümüne ait konteynerler listede görünmelidir.

6. **Son 100 Log Kaydını Görüntüleyin ve Takip Edin:**
   ```sh
   docker logs -f --tail 100 docker-compose-examples-external-node-1
   ``` 

Loglardan çıkmak için ctrl + c tuşuna basabilirsiniz. Bu işlem, düğümünüzün çalışmasını durdurmaz; düğüm arka planda çalışmaya devam eder.

## Adım 5: Dashboard'a Erişim için Port Forwarding (Yerel Bilgisayarda)

Docker çalıştırdıktan sonra, 3000 portunda bir dashboard'a erişebilirsiniz. Bu dashboard'u yerel bilgisayarınızdan görüntülemek için Visual Studio Code'da port forwarding yapmanız gerekecek.

1. **Port Forwarding Yapın:**

   - `Ctrl+Shift+P` tuşlarına basarak komut paletini açın.
   - `Forward a Port` yazın ve seçeneği seçin.
   - Port numarasını `3000` olarak girin ve Enter tuşuna basın.

   Alternatif olarak:

   - Visual Studio Code'un sağ alt köşesinde bulunan `PORTS` bölümüne tıklayın.
   - Açılan menüde `Forward Port...` seçeneğine tıklayın.
   - Port numarasını `3000` olarak girin ve Enter tuşuna basın.

2. **Dashboard'a Erişim:**

   - Tarayıcınızı açın ve `http://localhost:3000/d/0/external-node` adresine gidin.
   - Dashboard'a erişiminiz sağlanmış olacaktır.

Bu adım, uzaktaki sunucunuzda çalışan uygulamanın dashboard'unu yerel bilgisayarınızdan kolayca görüntülemenizi sağlar.

## Ek Bilgiler

- **Konteynerlerin Loglarını Görmek İçin:**
  docker logs <container_id>

- **Düğümü Durdurmak İçin:**
  docker-compose -f mainnet-external-node-docker-compose.yml down

