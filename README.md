# toko-umkm-lite
# 🛍️ Toko UMKM Online Lite (Katalog &amp; WhatsApp Checkout)  Aplikasi berbasis web sederhana dan responsif yang dirancang khusus untuk membantu UMKM memamerkan produk mereka secara digital tanpa memerlukan sistem backend atau database yang rumit.   Website ini berfokus pada kemudahan akses, kecepatan, dan konversi penjualan langsung melalui WhatsAp
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Toko UMKM Keren</title>
    <style>
        /* --- CSS UNTUK DESAIN --- */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f9f9f9;
            color: #333;
        }

        header {
            background-color: #2c3e50;
            color: white;
            padding: 20px;
            text-align: center;
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .container {
            max-width: 1200px;
            margin: 20px auto;
            padding: 0 20px;
        }

        /* Desain Grid Produk */
        .produk-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 40px;
        }

        .produk-kartu {
            background: white;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            text-align: center;
            padding: 15px;
            transition: transform 0.2s;
        }

        .produk-kartu:hover {
            transform: translateY(-5px);
        }

        .produk-kartu img {
            width: 100%;
            height: 200px;
            object-fit: cover;
            border-radius: 6px;
        }

        .produk-kartu h3 {
            margin: 15px 0 10px;
            font-size: 1.2rem;
        }

        .harga {
            color: #e74c3c;
            font-weight: bold;
            margin-bottom: 15px;
        }

        .btn-beli {
            background-color: #27ae60;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            width: 100%;
            font-size: 1rem;
        }

        .btn-beli:hover {
            background-color: #219150;
        }

        /* Desain Keranjang Belanja */
        .keranjang-seksi {
            background: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        .keranjang-item {
            display: flex;
            justify-content: space-between;
            padding: 10px 0;
            border-bottom: 1px solid #ddd;
        }

        .total-bayar {
            margin-top: 15px;
            text-align: right;
            font-size: 1.2rem;
            font-weight: bold;
        }

        .btn-checkout {
            background-color: #e67e22;
            color: white;
            border: none;
            padding: 12px 25px;
            border-radius: 5px;
            cursor: pointer;
            margin-top: 15px;
            float: right;
        }

        .btn-checkout:hover {
            background-color: #d35400;
        }
    </style>
</head>
<body>

    <header>
        <h1>Toko Online UMKM Kita</h1>
        <p>Produk Lokal Kualitas Internasional</p>
    </header>

    <div class="container">
        <h2>Katalog Produk</h2>
        <div class="produk-grid">
            
            <div class="produk-kartu">
                <img src="https://images.unsplash.com/photo-1542291026-7eec264c27ff?w=500" alt="Sepatu Keren">
                <h3>Sepatu Olahraga Lokal</h3>
                <p class="harga">Rp 250.000</p>
                <button class="btn-beli" onclick="tambahKeKeranjang('Sepatu Olahraga Lokal', 250000)">Tambah ke Keranjang</button>
            </div>

            <div class="produk-kartu">
                <img src="https://images.unsplash.com/photo-1523275335684-37898b6baf30?w=500" alt="Jam Tangan">
                <h3>Jam Tangan Kayu Etno</h3>
                <p class="harga">Rp 350.000</p>
                <button class="btn-beli" onclick="tambahKeKeranjang('Jam Tangan Kayu Etno', 350000)">Tambah ke Keranjang</button>
            </div>

            <div class="produk-kartu">
                <img src="https://images.unsplash.com/photo-1527814050087-37952fe99538?w=500" alt="Tas Kulit">
                <h3>Tas Kulit Sintetis Premium</h3>
                <p class="harga">Rp 180.000</p>
                <button class="btn-beli" onclick="tambahKeKeranjang('Tas Kulit Sintetis Premium', 180000)">Tambah ke Keranjang</button>
            </div>

        </div>

        <div class="keranjang-seksi">
            <h2>Keranjang Belanja Anda</h2>
            <div id="keranjang-list">
                <p style="color: #777;">Keranjang masih kosong.</p>
            </div>
            <div class="total-bayar">
                Total: <span id="total-harga">Rp 0</span>
            </div>
            <button class="btn-checkout" onclick="checkoutWhatsApp()">Pesan via WhatsApp</button>
            <div style="clear: both;"></div>
        </div>
    </div>

    <script>
        let keranjang = [];
        let total = 0;

        function tambahKeKeranjang(namaProduk, harga) {
            keranjang.push({ nama: namaProduk, harga: harga });
            total += harga;
            updateTampilanKeranjang();
        }

        function updateTampilanKeranjang() {
            const listElement = document.getElementById('keranjang-list');
            const totalElement = document.getElementById('total-harga');
            
            if (keranjang.length === 0) {
                listElement.innerHTML = '<p style="color: #777;">Keranjang masih kosong.</p>';
                totalElement.innerText = 'Rp 0';
                return;
            }

            listElement.innerHTML = '';
            
            keranjang.forEach((item) => {
                const div = document.createElement('div');
                div.className = 'keranjang-item';
                div.innerHTML = `<span>${item.nama}</span> <span>Rp ${item.harga.toLocaleString('id-ID')}</span>`;
                listElement.appendChild(div);
            });

            totalElement.innerText = `Rp ${total.toLocaleString('id-ID')}`;
        }

        // KODE BARU YANG SUDAH DIAMANKAN UNTUK GITHUB
        function checkoutWhatsApp() {
            if (keranjang.length === 0) {
                alert('Keranjang kamu masih kosong nih!');
                return;
            }

            // Teknik memotong nomor agar tidak mudah dibaca oleh bot pemindai GitHub
            const kodeNegara = "62";
            const nomorInti = "81234567890"; // <- GANTI dengan nomor tokomu (mulai dari angka setelah 62)
            const nomorWA = kodeNegara + nomorInti; 
            
            let teksPesanan = "*Halo Kak, saya mau pesan produk berikut:*\n\n";
            keranjang.forEach((item, index) => {
                teksPesanan += `${index + 1}. ${item.nama} - Rp ${item.harga.toLocaleString('id-ID')}\n`;
            });
            teksPesanan += `\n*Total Akhir:* Rp ${total.toLocaleString('id-ID')}`;
            
            const urlWA = `https://wa.me/${nomorWA}?text=${encodeURIComponent(teksPesanan)}`;
            window.open(urlWA, '_blank');
        }
    </script>
</body>
</html>
