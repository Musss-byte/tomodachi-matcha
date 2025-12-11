<html lang="id">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Tomodachi Matcha Store — Premium</title>

<script src="https://cdn.tailwindcss.com"></script>

<style>
  body {
    font-family: 'Inter', sans-serif;
    background: linear-gradient(135deg, #cfe8cf, #b5ddb5, #9fce9f);
  }
  .glass {
    background: rgba(255,255,255,0.7);
    backdrop-filter: blur(16px);
    border: 1px solid rgba(255,255,255,0.4);
  }
</style>
</head>

<body class="text-[#1f2d1f]">

<!-- NAVBAR PREMIUM -->
<nav class="w-full fixed top-0 left-0 z-50 glass shadow-lg">
  <div class="max-w-6xl mx-auto flex items-center justify-between p-4">
    <div class="flex items-center gap-3">
      <img src="logo.png" class="w-12 h-12 rounded-full shadow-md" />
      <h1 class="text-2xl font-bold text-[#26452f] tracking-wide">
        Tomodachi Matcha
      </h1>
    </div>

    <button
      id="cartBtn"
      class="relative px-4 py-2 bg-[#3e7f4c] hover:bg-[#336a40] transition text-white rounded-full shadow-md font-medium"
    >
      🛒 Keranjang
      <span
        id="cartCount"
        class="absolute -top-2 -right-2 bg-red-500 text-white text-xs w-6 h-6 flex items-center justify-center rounded-full shadow"
      >0</span>
    </button>
  </div>
</nav>

<!-- OVERLAY UNTUK CLOSE CART -->
<div id="cartOverlay" class="fixed inset-0 bg-black/40 hidden z-[55]"></div>

<!-- CONTENT -->
<div class="pt-28 max-w-6xl mx-auto p-4">
  <h2 class="text-3xl font-bold mb-6 text-[#274e32]">
    Produk Matcha Premium 🍃
  </h2>
  <div id="productGrid"
       class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
  </div>
</div>

<!-- CART SIDEBAR PREMIUM -->
<div id="cartSidebar"
     class="fixed top-0 right-0 w-80 h-full glass shadow-2xl p-5 transform translate-x-full transition-all duration-300 z-[60] overflow-y-auto rounded-l-2xl">

  <!-- Header + Tombol Close -->
  <div class="flex items-center justify-between mb-3">
    <h2 class="text-2xl font-semibold text-[#244b2f]">Keranjang</h2>
    <button id="closeCart" class="text-xl px-2 text-[#244b2f] hover:text-red-500 transition">✖</button>
  </div>

  <div id="cartList" class="flex flex-col gap-3"></div>

  <div class="border-t border-[#d9e8d9] mt-4 pt-4">
    <label class="block text-sm font-medium">Pilih Kota (Ongkir)</label>
    <select id="citySelect" class="w-full border p-2 rounded mt-1 bg-white/70">
      <option value="Kedung Halang">Kedung Halang</option>
      <option value="Cilebut">Cilebut</option>
      <option value="Bogor Kota">Bogor Kota</option>
      <option value="Ciawi">Ciawi</option>
      <option value="Cibinong">Cibinong</option>
    </select>
  </div>

  <div class="mt-4">
    <label class="block text-sm font-medium">Kode Kupon</label>
    <input id="Matcha10" class="w-full border p-2 rounded mt-1 bg-white/70" placeholder="DISKON10" />
  </div>

  <div class="mt-4 text-sm space-y-1">
    <p>Subtotal: <span id="subtotalBox" class="font-semibold"></span></p>
    <p>Diskon: <span id="discountBox" class="font-semibold text-green-700"></span></p>
    <p>Ongkir: <span id="ongkirBox" class="font-semibold"></span></p>

    <p class="text-xl font-bold mt-3">TOTAL: <span id="totalBox"></span></p>
  </div>

  <button
    id="checkoutBtn"
    class="w-full bg-[#3e7f4c] hover:bg-[#336a40] transition text-white p-3 rounded-lg mt-5 shadow-md font-medium">
    Checkout
  </button>
</div>

<!-- CHECKOUT MODAL -->
<div id="checkoutModal" class="fixed inset-0 bg-black/40 hidden items-center justify-center z-[70] p-4">
  <div class="glass p-6 rounded-2xl max-w-md w-full shadow-2xl">

    <h2 class="text-2xl font-bold mb-4 text-[#274e32]">Checkout</h2>

    <input id="name" class="w-full border p-2 rounded mb-3 bg-white/70" placeholder="Nama lengkap" />
    <input id="address" class="w-full border p-2 rounded mb-3 bg-white/70" placeholder="Alamat lengkap" />
    <input id="phone" class="w-full border p-2 rounded mb-3 bg-white/70" placeholder="Nomor telepon" />

    <label class="text-sm font-medium">Metode Pembayaran</label>
    <div class="mt-1 mb-4 flex flex-col gap-1 text-sm">
      <label><input type="radio" name="paymentMethod" value="COD" checked /> COD</label>
      <label><input type="radio" name="paymentMethod" value="Transfer Bank" /> Transfer Bank</label>
    </div>

    <div class="flex gap-3">
      <button id="sendWA" class="flex-1 bg-[#3e7f4c] hover:bg-[#336a40] text-white p-2 rounded-lg">Kirim WhatsApp</button>
      <button id="closeModal" class="flex-1 bg-gray-300 p-2 rounded-lg">Batal</button>
    </div>
  </div>
</div>

<script>

/* ——————————————————————————————
        PRODUK MATCHA PREMIUM
—————————————————————————————— */
const products = [
  { id: 1, name: "Matcha Latte", price: 25000, img: "p1.jpg" },
  { id: 2, name: "Matcha Premium", price: 30000, img: "p2.jpg" },
  { id: 3, name: "Matcha Soft Cream", price: 20000, img: "p3.jpg" },
  { id: 4, name: "Matcha Cookies", price: 15000, img: "p4.jpg" },
  { id: 5, name: "Matcha Powder", price: 40000, img: "p5.jpg" },
];

let cart = JSON.parse(localStorage.getItem("cart") || "[]");

const grid = document.getElementById('productGrid');

grid.innerHTML = products.map(p => `
  <div class="glass rounded-xl shadow-xl p-4 hover:shadow-2xl transition border border-[#d9e8d9]">
    <img src="${p.img}" class="w-full h-44 object-cover rounded-lg mb-3 shadow-md" />
    <h3 class="font-semibold text-xl text-[#274e32]">${p.name}</h3>
    <p class="text-[#3e7f4c] font-bold text-lg">Rp ${p.price.toLocaleString()}</p>
    <button onclick="addToCart(${p.id})"
            class="mt-3 w-full bg-[#3e7f4c] hover:bg-[#336a40] transition text-white p-2 rounded-lg shadow-md">
      Tambah ke Keranjang
    </button>
  </div>
`).join("");

/* ——————————————————————————————
                  CART
—————————————————————————————— */
function saveCart(){ localStorage.setItem("cart", JSON.stringify(cart)); }

function addToCart(id){
  let item = cart.find(i => i.id === id);
  if(item){ item.qty++; }
  else {
    const p = products.find(x => x.id === id);
    cart.push({ ...p, qty: 1 });
  }
  saveCart();
  renderCart();
}

function changeQty(id, delta){
  let item = cart.find(i => i.id === id);
  if(!item) return;
  item.qty = Math.max(1, item.qty + delta);
  saveCart(); renderCart();
}

function removeItem(id){
  cart = cart.filter(i => i.id !== id);
  saveCart(); renderCart();
}

const cityShipping = {
  "Kedung Halang": 10000,
  "Cilebut": 12000,
  "Bogor Kota": 15000,
  "Ciawi": 18000,
  "Cibinong": 20000
};

function renderCart(){
  document.getElementById('cartCount').innerText = cart.reduce((a,b)=>a+b.qty,0);

  const list = document.getElementById('cartList');

  if(cart.length === 0){
    list.innerHTML = `<p class='text-gray-500'>Keranjang kosong.</p>`;
    return;
  }

  list.innerHTML = cart.map(i => `
    <div class="flex items-center gap-3 bg-white/60 rounded-xl p-3 shadow border border-[#d9e8d9]">
      <img src="${i.img}" class="w-14 h-14 rounded-lg object-cover shadow" />
      <div class="flex-1">
        <p class="font-medium text-[#2f4f36]">${i.name}</p>
        <p class="text-sm text-[#3e7f4c] font-semibold">Rp ${i.price.toLocaleString()}</p>
        
        <div class="flex items-center gap-2 mt-1">
          <button onclick="changeQty(${i.id}, -1)"
                  class="px-2 bg-gray-300 rounded">-</button>
          <span>${i.qty}</span>
          <button onclick="changeQty(${i.id}, 1)"
                  class="px-2 bg-gray-300 rounded">+</button>
        </div>
      </div>

      <div class="text-right">
        <p class="font-semibold text-[#274e32]">
          Rp ${(i.price*i.qty).toLocaleString()}
        </p>
        <button onclick="removeItem(${i.id})" class="text-red-500 text-sm mt-1">Hapus</button>
      </div>
    </div>
  `).join("");

  let subtotal = cart.reduce((a,b)=> a + b.price*b.qty, 0);
  let coupon = document.getElementById('couponInput').value;
  let discount = coupon === "DISKON10" ? subtotal * 0.10 : 0;
  let ongkir = cityShipping[document.getElementById('citySelect').value] || 0;
  let total = subtotal - discount + ongkir;

  document.getElementById('subtotalBox').innerText = `Rp ${subtotal.toLocaleString()}`;
  document.getElementById('discountBox').innerText = `Rp ${discount.toLocaleString()}`;
  document.getElementById('ongkirBox').innerText = `Rp ${ongkir.toLocaleString()}`;
  document.getElementById('totalBox').innerText = `Rp ${total.toLocaleString()}`;
}

renderCart();

document.getElementById('couponInput').addEventListener('input', renderCart);
document.getElementById('citySelect').addEventListener('change', renderCart);

/* ——————————————————————————————
            CART OPEN / CLOSE
—————————————————————————————— */
const cartSidebar = document.getElementById('cartSidebar');
const cartOverlay = document.getElementById('cartOverlay');
const closeCartBtn = document.getElementById('closeCart');

// Buka cart
document.getElementById('cartBtn').onclick = () => {
  cartSidebar.classList.remove('translate-x-full');
  cartOverlay.classList.remove('hidden');
};

// Tombol ✖
closeCartBtn.onclick = () => {
  cartSidebar.classList.add('translate-x-full');
  cartOverlay.classList.add('hidden');
};

// Klik area luar
cartOverlay.onclick = () => {
  cartSidebar.classList.add('translate-x-full');
  cartOverlay.classList.add('hidden');
};

/* ——————————————————————————————
              MODAL CHECKOUT
—————————————————————————————— */
document.getElementById('checkoutBtn').onclick = () => {
  if(cart.length===0) return alert("Keranjang kosong!");
  const modal = document.getElementById('checkoutModal');
  modal.classList.remove('hidden');
  modal.classList.add('flex');
};

document.getElementById('closeModal').onclick = () => {
  document.getElementById('checkoutModal').classList.add('hidden');
};

document.getElementById('sendWA').onclick = () => {
  const name = document.getElementById('name').value;
  const addr = document.getElementById('address').value;
  const phone = document.getElementById('phone').value;
  const pay = document.querySelector("input[name='paymentMethod']:checked").value;
  const city = document.getElementById('citySelect').value;

  let text = `Halo, saya ingin memesan:%0A`;
  cart.forEach(i=>{
    text += `- ${i.name} x${i.qty} = Rp ${(i.price*i.qty).toLocaleString()}%0A`;
  });

  text += `%0A Kota: ${city} (Ongkir Rp ${cityShipping[city].toLocaleString()})%0A`;
  text += `Metode Pembayaran: ${pay}%0A%0A`;
  text += `Nama: ${name}%0AAlamat: ${addr}%0ANo HP: ${phone}`;

  window.open(`https://wa.me/62838795289830?text=${text}`);
};

</script>

</body>
</html>
