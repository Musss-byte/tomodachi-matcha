<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Tomodachi Matcha — Single File</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-green-50 text-gray-900">
  <header class="p-4 bg-green-600 text-white text-center text-2xl font-bold shadow">
    Tomodachi Matcha
  </header>

  <main class="p-4 max-w-4xl mx-auto">
    <h2 class="text-xl font-semibold mb-4">Produk</h2>
    <div id="productList" class="grid grid-cols-2 gap-4"></div>

    <h2 class="text-xl font-semibold mt-8 mb-4">Keranjang</h2>
    <div id="cartList" class="space-y-2"></div>
    <button id="checkoutBtn" class="mt-4 w-full bg-green-600 text-white p-3 rounded-lg">Checkout via WhatsApp</button>
  </main>

  <script>
    const products = [
      { id: 1, name: "Matcha Premium", price: 45000, img: "https://i.imgur.com/8Km9tLL.jpeg" },
      { id: 2, name: "Matcha Latte", price: 35000, img: "https://i.imgur.com/8Km9tLL.jpeg" }
    ];

    const cart = [];

    function renderProducts() {
      const list = document.getElementById("productList");
      list.innerHTML = products.map(p => `
        <div class='bg-white p-3 rounded shadow'>
          <img src='${p.img}' class='w-full rounded mb-2'/>
          <div class='font-semibold'>${p.name}</div>
          <div class='mb-2'>Rp ${p.price.toLocaleString()}</div>
          <button onclick='addToCart(${p.id})' class='bg-green-600 text-white px-3 py-2 rounded w-full'>Tambah</button>
        </div>`).join("");
    }

    function renderCart() {
      const list = document.getElementById("cartList");
      if (cart.length === 0) {
        list.innerHTML = "<div class='text-gray-500'>Keranjang kosong</div>";
        return;
      }
      list.innerHTML = cart.map(item => `
        <div class='flex justify-between bg-white p-3 rounded shadow'>
          <span>${item.name}</span>
          <span>Rp ${item.price.toLocaleString()}</span>
        </div>`).join("");
    }

    function addToCart(id) {
      const product = products.find(p => p.id === id);
      cart.push(product);
      renderCart();
    }

    document.getElementById("checkoutBtn").onclick = () => {
      if (cart.length === 0) return alert("Keranjang masih kosong!");

      const text = cart.map(c => `- ${c.name} (Rp ${c.price.toLocaleString()})`).join("%0A");
      const total = cart.reduce((a, b) => a + b.price, 0);

      const url = `https://wa.me/6281234567890?text=Halo%20saya%20ingin%20pesan:%0A${text}%0ATotal:%20Rp%20${total.toLocaleString()}`;
      window.open(url, "_blank");
    };

    renderProducts();
    renderCart();
  </script>
</body>
</html>
