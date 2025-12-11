<html lang="id">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Tomodachi Matcha</title>

  <!-- Tailwind CDN -->
  <script src="https://cdn.tailwindcss.com"></script>

  <!-- Heroicons -->
  <script src="https://unpkg.com/heroicons@2.0.18/dist/heroicons.js"></script>

  <style>
    body {
      background-color: #f6fdf7;
    }
  </style>
</head>

<body class="text-green-900">

  <!-- NAVBAR -->
  <nav class="flex items-center justify-between py-4 px-6 bg-white shadow">
    <div class="flex items-center gap-3">
      <img src="logo.png" alt="Logo" class="h-10 w-10 rounded-full">
      <h1 class="text-2xl font-bold text-green-900">Tomodachi Matcha</h1>
    </div>

    <div class="flex items-center gap-6">
      <a href="#produk" class="font-semibold">Produk</a>

      <!-- KERANJANG DIGANTI IKON -->
      <button id="cart-btn" class="relative">
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.8"
          stroke="currentColor" class="w-7 h-7 text-green-900">
          <path stroke-linecap="round" stroke-linejoin="round"
            d="M2.25 3h1.386c.51 0 .955.343 1.087.836l.383 1.437M7.5 
              14.25a3 3 0 1 0 0 6 3 3 0 0 0 0-6Zm9 3a3 3 0 1 1-6 
              0m6 0a3 3 0 1 0 6 0m-6 
              0H9m12-3.75-1.5-6.75H4.106M20.25 
              10.5H6.857" />
        </svg>

        <span id="cart-count"
          class="absolute -top-1 -right-1 bg-red-500 text-white text-xs px-1.5 py-0.5 rounded-full hidden">0</span>
      </button>
    </div>
  </nav>

  <!-- HEADER -->
  <header class="text-center py-10">
    <h2 class="text-3xl font-bold text-green-800">Selamat Datang di Tomodachi Matcha</h2>
    <p class="mt-2 text-green-700">Matcha premium, rasa original Jepang 🍵</p>
  </header>

  <!-- PRODUK -->
  <section id="produk" class="px-6">
    <h3 class="text-2xl font-bold mb-4">Produk Kami</h3>

    <div class="grid grid-cols-2 md:grid-cols-3 gap-4">

      <!-- PRODUCT EXAMPLE -->
      <div class="bg-white p-4 rounded-xl shadow border">
        <img src="produk1.jpg" class="rounded-lg w-full h-40 object-cover" />
        <h4 class="font-bold mt-2">Matcha Latte</h4>
        <p class="text-green-700">Rp 18.000</p>
        <button onclick="addToCart('Matcha Latte', 18000)"
          class="mt-2 bg-green-600 text-white px-4 py-2 rounded-lg w-full">Tambah</button>
      </div>

      <div class="bg-white p-4 rounded-xl shadow border">
        <img src="produk2.jpg" class="rounded-lg w-full h-40 object-cover" />
        <h4 class="font-bold mt-2">Matcha Original</h4>
        <p class="text-green-700">Rp 15.000</p>
        <button onclick="addToCart('Matcha Original', 15000)"
          class="mt-2 bg-green-600 text-white px-4 py-2 rounded-lg w-full">Tambah</button>
      </div>

    </div>
  </section>

  <!-- AMBIL LOKASI OTOMATIS -->
  <section class="px-6 mt-10">
    <h3 class="text-xl font-bold mb-3">Ambil Lokasi Otomatis (GPS)</h3>

    <button onclick="getLocation()"
      class="bg-green-700 text-white px-5 py-2 rounded-lg active:scale-95 shadow">
      Ambil Lokasi Saya 📍
    </button>

    <p class="mt-2 text-green-700" id="gps-result"></p>
  </section>

  <!-- GOOGLE MAPS LOKASI BISNIS -->
  <section class="px-6 mt-10">
    <h3 class="text-xl font-bold">Lokasi Bisnis Kami</h3>
    <p class="text-green-700">Jl Pembangunan RT 02 RW 05 Kedung Halang, Bogor Utara</p>

    <div class="max-w-xl mt-4 rounded-xl overflow-hidden shadow-lg border border-green-200">
      <iframe
        src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3967.31372696543!2d106.8279876!3d-6.562045199999999!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x2e69c9c81cecc7df%3A0x8a83b2e1fa89bbf0!2sKedung%20Halang%2C%20Bogor%20Utara!5e0!3m2!1sid!2sid!4v1700000000000!5m2!1sid!2sid"
        width="100%" height="300" allowfullscreen loading="lazy"></iframe>
    </div>
  </section>

  <!-- ABOUT -->
  <section class="px-6 mt-10">
    <h3 class="text-xl font-bold">Tentang Kami</h3>
    <p class="text-green-700 mt-2">
      Tomodachi Matcha menghadirkan minuman matcha premium dengan cita rasa autentik Jepang.
      Dibuat dari bahan alami tanpa pengawet.
    </p>
  </section>

  <!-- CONTACT -->
  <section class="px-6 mt-10">
    <h3 class="text-xl font-bold">Kontak Kami</h3>

    <p class="mt-2">📞 WhatsApp: <a href="https://wa.me/628XXXXXX" class="text-green-700 underline">Hubungi Kami</a></p>
    <p>📍 Bogor Utara, Kota Bogor</p>
  </section>

  <!-- FOOTER -->
  <footer class="text-center py-10 mt-10 text-green-800">
    <p>© 2025 Tomodachi Matcha. All Rights Reserved.</p>
  </footer>

  <!-- SCRIPT -->
  <script>
    let cart = [];

    function addToCart(n, p) {
      cart.push({ name: n, price: p });
      document.getElementById("cart-count").innerText = cart.length;
      document.getElementById("cart-count").classList.remove("hidden");
      alert(n + " ditambahkan ke keranjang!");
    }

    // GPS
    function getLocation() {
      if (!navigator.geolocation) {
        document.getElementById("gps-result").innerText = "Browser tidak mendukung GPS.";
        return;
      }

      navigator.geolocation.getCurrentPosition(pos => {
        const lat = pos.coords.latitude;
        const lon = pos.coords.longitude;

        document.getElementById("gps-result").innerHTML =
          "Lokasi Anda: <br>Latitude: " + lat + "<br>Longitude: " + lon;
      }, () => {
        document.getElementById("gps-result").innerText = "Tidak dapat mengambil lokasi.";
      });
    }
  </script>

</body>

</html>
