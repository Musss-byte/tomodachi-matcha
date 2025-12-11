<html lang="id">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Tomodachi Matcha — Store</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
  body { font-family: 'Inter', sans-serif; background: linear-gradient(135deg,#cfe8cf,#b5ddb5,#9fce9f); }
  .glass { background: rgba(255,255,255,0.78); backdrop-filter: blur(16px); border: 1px solid rgba(255,255,255,0.35); }
  a:hover { text-decoration: underline; }
</style>
</head>
<body class="min-h-screen text-[#16321f]">

<!-- NAVBAR -->
<nav class="glass fixed top-0 left-0 w-full px-6 py-4 flex justify-between items-center shadow-lg z-50">
  <div class="flex items-center gap-4">
    <img src="logo.png" alt="logo" class="w-10 h-10 rounded-full shadow-sm" />
    <div>
      <h1 class="text-2xl font-bold">🍃 Tomodachi Matcha</h1>
      <div class="text-xs text-green-800">Premium Matcha — pesan cepat lewat WhatsApp</div>
    </div>
  </div>

  <div class="flex items-center gap-3">
    <div class="text-sm text-green-900 mr-2" id="mini-cart-count">Keranjang: 0</div>
    <button onclick="toggleCart()" class="relative px-4 py-2 bg-green-700 text-white rounded-xl hover:bg-green-800 transition">
      🛒 Keranjang
      <span id="cart-count" class="absolute -top-2 -right-2 bg-red-600 text-white text-xs w-6 h-6 flex items-center justify-center rounded-full">0</span>
    </button>
  </div>
</nav>

<!-- MAIN CONTENT -->
<main class="pt-28 max-w-6xl mx-auto px-6 pb-16">
  <header class="mb-8">
    <h2 class="text-3xl font-bold text-green-900">Produk Matcha Premium</h2>
    <p class="text-sm text-green-800 mt-1">Pilih produk, tambahkan ke keranjang, dan checkout lewat WhatsApp.</p>
  </header>

  <section id="products" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6"></section>
</main>

<!-- CART OVERLAY -->
<div id="cart-overlay" class="hidden fixed inset-0 bg-black/40 z-40" onclick="closeCart()"></div>

<!-- CART PANEL -->
<aside id="cart-panel" class="hidden fixed right-0 top-0 h-full w-96 glass p-5 shadow-2xl z-50 flex flex-col">
  <div class="flex items-center justify-between mb-4">
    <h3 class="text-2xl font-bold text-green-900">Keranjang</h3>
    <div class="flex items-center gap-2">
      <button id="clear-cart" class="text-sm text-red-600 hover:underline mr-2" onclick="clearCart()">Kosongkan</button>
      <button onclick="closeCart()" class="text-xl font-bold text-green-900">✖</button>
    </div>
  </div>

  <div id="cart-items" class="flex-1 overflow-auto space-y-3"></div>

  <div class="mt-4 border-t pt-4 space-y-3 text-sm">
    <!-- Distance input for shipping -->
    <div>
      <label class="block font-medium">Jarak pengiriman (km)</label>
      <input id="distance-km" type="number" min="0" step="0.1" placeholder="Masukkan jarak (contoh: 12.5)" class="w-full p-2 border rounded mt-1 bg-white/70" />
      <div class="text-xs text-gray-600 mt-1">Hitung ongkir otomatis: jarak × tarif per km.</div>
    </div>

    <!-- Rate per km (configurable by owner) -->
    <div class="flex items-center gap-2">
      <label class="text-sm">Tarif / km:</label>
      <div class="font-semibold">Rp <span id="rate-per-km">2000</span></div>
    </div>

    <!-- Coupon -->
    <div>
      <label class="block font-medium">Kupon (min pembelian)</label>
      <div class="flex gap-2 mt-1">
        <input id="coupon-code" class="flex-1 p-2 border rounded bg-white/70" placeholder="Masukkan kode kupon" />
        <button id="apply-coupon" class="px-3 py-2 rounded bg-yellow-500 text-white">Terapkan</button>
      </div>
      <div id="coupon-help" class="text-xs text-gray-700 mt-1">
        Contoh: MATCHA100 (10% min Rp100.000), MATCHA200 (20% min Rp200.000), HEMAT5 (Rp5.000 min Rp50.000)
      </div>
    </div>

    <!-- Payment method -->
    <div>
      <label class="block font-medium">Metode Pembayaran</label>
      <select id="payment-method" class="w-full p-2 border rounded bg-white/70 mt-1">
        <option value="COD">COD (Bayar di Tempat)</option>
        <option value="TRANSFER">Transfer Bank</option>
      </select>
      <div id="bank-info" class="hidden mt-2 p-2 bg-white/70 rounded text-sm">
        <div class="font-semibold">Rekening transfer (contoh):</div>
        <div>BCA — 1234567890</div>
        <div>A/N Tomodachi Matcha</div>
      </div>
    </div>

    <!-- Summary -->
    <div class="pt-2 border-t">
      <div class="flex justify-between"><span>Subtotal</span><span id="summary-subtotal">Rp0</span></div>
      <div class="flex justify-between"><span>Diskon</span><span id="summary-discount">Rp0</span></div>
      <div class="flex justify-between"><span>Ongkir</span><span id="summary-shipping">Rp0</span></div>
      <div class="flex justify-between font-bold text-lg pt-2"><span>Total</span><span id="summary-total">Rp0</span></div>
    </div>

    <div>
      <button id="btn-checkout" class="w-full bg-green-700 text-white p-3 rounded-xl hover:bg-green-800" onclick="openCheckout()">Checkout</button>
    </div>
  </div>
</aside>

<!-- FOOTER with About & Contact -->
<footer class="mt-12">
  <div class="max-w-6xl mx-auto px-6 py-8 text-sm text-green-900">
    <div class="flex flex-col md:flex-row justify-between gap-6">
      <div>
        <h4 class="font-bold text-lg mb-2">Tomodachi Matcha</h4>
        <p>Matcha premium — diproses dengan hati. Kirim pesanan lewat WhatsApp dan kami akan segera follow up.</p>
      </div>

      <div>
        <h5 class="font-semibold mb-1">About</h5>
        <p class="max-w-sm">Tomodachi Matcha hadir untuk menyediakan matcha berkualitas tinggi, rasa autentik Jepang dengan sentuhan lokal.</p>
      </div>

      <div>
        <h5 class="font-semibold mb-1">Contact</h5>
        <p>WhatsApp: <a href="https://wa.me/6280000000000" target="_blank" class="text-green-800">+62 800-0000-0000</a></p>
        <p>Email: <a href="mailto:info@tomodachi-matcha.example" class="text-green-800">info@tomodachi-matcha.example</a></p>
        <p>Alamat: Jl. Matcha No.1, Jakarta</p>
      </div>
    </div>

    <div class="text-center text-xs text-gray-700 mt-6">© <span id="footer-year"></span> Tomodachi Matcha — All rights reserved</div>
  </div>
</footer>

<!-- Checkout Modal -->
<div id="checkout-modal" class="hidden fixed inset-0 bg-black/40 z-50 items-center justify-center p-4">
  <div class="glass w-full max-w-lg p-6 rounded-2xl shadow-2xl">
    <h3 class="text-2xl font-bold text-green-900 mb-3">Form Checkout</h3>

    <div class="grid grid-cols-1 gap-3">
      <input id="cust-name" class="w-full p-3 border rounded" placeholder="Nama lengkap" />
      <input id="cust-phone" class="w-full p-3 border rounded" placeholder="Nomor telepon" />
      <textarea id="cust-address" class="w-full p-3 border rounded" rows="3" placeholder="Alamat lengkap"></textarea>
      <input id="cust-city" class="w-full p-3 border rounded" placeholder="Kota / Kabupaten" />
      <input id="cust-postal" class="w-full p-3 border rounded" placeholder="Kode Pos" />
      <div class="text-sm text-gray-700">Periksa kembali data sebelum mengirim pesanan via WhatsApp.</div>

      <div class="flex gap-3 mt-3">
        <button onclick="submitCheckout()" class="flex-1 bg-green-700 text-white p-3 rounded">Kirim via WhatsApp</button>
        <button onclick="closeCheckout()" class="flex-1 bg-gray-300 p-3 rounded">Batal</button>
      </div>
    </div>
  </div>
</div>

<script>
/* ========== CONFIG ========== */
const SHOP_PHONE = "6280000000000"; // ganti nomor tujuan WA
const RATE_PER_KM = 2000; // tarif per km untuk ongkir
const COUPONS = [
  { code: "MATCHA100", type: "percent", value: 10, min: 100000 }, // 10% min 100k
  { code: "MATCHA200", type: "percent", value: 20, min: 200000 }, // 20% min 200k
  { code: "HEMAT5", type: "fixed", value: 5000, min: 50000 } // Rp5.000 off min 50k
];

/* ========== DATA PRODUK ========== */
const PRODUCTS = [
  { id:1, name:"Matcha Premium 100g", price:85000, img:"https://i.imgur.com/4ZQZ4Zy.png" },
  { id:2, name:"Matcha Ceremonial 50g", price:120000, img:"https://i.imgur.com/4ZQZ4Zy.png" },
  { id:3, name:"Matcha Latte Blend", price:75000, img:"https://i.imgur.com/4ZQZ4Zy.png" },
  { id:4, name:"Matcha Cookies", price:45000, img:"https://i.imgur.com/4ZQZ4Zy.png" },
  { id:5, name:"Matcha Tea Bag", price:30000, img:"https://i.imgur.com/4ZQZ4Zy.png" }
];

/* ========== STATE ========== */
let cart = JSON.parse(localStorage.getItem("tm_cart_v3") || "[]");
let appliedCoupon = null;

/* ========== DOM refs ========== */
const productsEl = document.getElementById("products") || document.getElementById("products");
const productListEl = document.getElementById("products") || document.getElementById("products");
const cartItemsEl = document.getElementById("cart-items");
const ratePerKmEl = document.getElementById("rate-per-km");
const couponInput = document.getElementById("coupon-code");
const applyCouponBtn = document.getElementById("apply-coupon");
const bankInfoEl = document.getElementById("bank-info");
const paymentMethodEl = document.getElementById("payment-method");

/* ========== INIT UI ========== */
document.getElementById("footer-year").innerText = new Date().getFullYear();
document.getElementById("rate-per-km").innerText = RATE_PER_KM.toLocaleString();

/* Render product cards */
function renderProducts(){
  const el = document.getElementById("products");
  el.innerHTML = "";
  PRODUCTS.forEach(p=>{
    el.innerHTML += `
      <article class="glass rounded-2xl p-4 shadow hover:shadow-2xl transition">
        <img src="${p.img}" class="w-full h-44 object-cover rounded-lg mb-3" alt="${p.name}" />
        <h4 class="font-semibold text-lg">${p.name}</h4>
        <div class="mt-1 font-bold text-green-800">Rp${p.price.toLocaleString()}</div>
        <button onclick="addToCart(${p.id})" class="mt-3 w-full bg-green-700 text-white p-2 rounded-xl">Tambah ke Keranjang</button>
      </article>
    `;
  });
}

/* ========== CART CRUD & RENDER ========== */
function saveState(){ localStorage.setItem("tm_cart_v3", JSON.stringify(cart)); }

function addToCart(id){
  const found = cart.find(i=>i.id===id);
  if(found) found.qty++;
  else {
    const p = PRODUCTS.find(x=>x.id===id);
    cart.push({...p, qty:1});
  }
  saveState(); renderCart();
}

function changeQty(id, delta){
  const it = cart.find(i=>i.id===id);
  if(!it) return;
  it.qty = Math.max(0, it.qty + delta);
  if(it.qty===0) cart = cart.filter(i=>i.id!==id);
  saveState(); renderCart();
}

function removeItem(id){
  cart = cart.filter(i=>i.id!==id);
  saveState(); renderCart();
}

function clearCart(){
  if(!confirm("Kosongkan seluruh keranjang?")) return;
  cart = []; appliedCoupon = null; couponInput.value="";
  saveState(); renderCart();
}

function formatIDR(v){ return "Rp " + Number(v).toLocaleString("id-ID"); }

function computeSummary(){
  const subtotal = cart.reduce((s,i)=> s + i.price * i.qty, 0);
  // coupon
  let discount = 0;
  if(appliedCoupon){
    if(appliedCoupon.type === "percent"){
      discount = Math.round(subtotal * (appliedCoupon.value/100));
    } else if(appliedCoupon.type === "fixed"){
      discount = appliedCoupon.value;
    }
  }
  // shipping from distance
  const km = parseFloat(document.getElementById("distance-km").value || "0");
  const shipping = Math.max(0, Math.round(km * RATE_PER_KM));
  const total = Math.max(0, subtotal - discount + shipping);
  return { subtotal, discount, shipping, total, km };
}

function renderCart(){
  // mini counts
  document.getElementById("cart-count").innerText = cart.reduce((s,i)=>s+i.qty,0);
  document.getElementById("mini-cart-count").innerText = "Keranjang: " + cart.reduce((s,i)=>s+i.qty,0);

  // cart items list
  const el = cartItemsEl;
  el.innerHTML = "";
  if(cart.length===0){
    el.innerHTML = `<div class="text-gray-700">Keranjang kosong — tambahkan produk.</div>`;
  } else {
    cart.forEach(it=>{
      el.innerHTML += `
        <div class="flex gap-3 border p-3 rounded-lg bg-white/70">
          <img src="${it.img}" class="w-16 h-16 rounded object-cover" />
          <div class="flex-1">
            <div class="font-semibold">${it.name}</div>
            <div class="text-sm text-green-800">Rp${it.price.toLocaleString()}</div>
            <div class="flex items-center gap-2 mt-2">
              <button onclick="changeQty(${it.id}, -1)" class="px-2 bg-green-700 text-white rounded">−</button>
              <div class="px-2">${it.qty}</div>
              <button onclick="changeQty(${it.id}, 1)" class="px-2 bg-green-700 text-white rounded">+</button>
            </div>
          </div>
          <div class="text-right">
            <div class="font-semibold">Rp${(it.price*it.qty).toLocaleString()}</div>
            <button onclick="removeItem(${it.id})" class="text-red-600 text-sm mt-2">Hapus</button>
          </div>
        </div>
      `;
    });
  }

  // summary
  const { subtotal, discount, shipping, total } = computeSummary();
  document.getElementById("summary-subtotal").innerText = formatIDR(subtotal);
  document.getElementById("summary-discount").innerText = "-" + formatIDR(discount);
  document.getElementById("summary-shipping").innerText = formatIDR(shipping);
  document.getElementById("summary-total").innerText = formatIDR(total);
}

/* ========== COUPON LOGIC (type C: min purchase) ========== */
applyCouponBtn.addEventListener("click", ()=>{
  const code = (couponInput.value||"").trim().toUpperCase();
  if(!code) return alert("Masukkan kode kupon.");
  const found = COUPONS.find(c=>c.code===code);
  if(!found) { appliedCoupon = null; alert("Kupon tidak valid."); renderCart(); return; }
  const subtotal = cart.reduce((s,i)=> s + i.price * i.qty, 0);
  if(subtotal < found.min){
    appliedCoupon = null;
    alert(`Kupon ${code} membutuhkan minimal belanja ${formatIDR(found.min)}.`);
    return;
  }
  appliedCoupon = found;
  alert(`Kupon ${code} berhasil diterapkan!`);
  renderCart();
});

/* ========== PAYMENT METHOD UI ========== */
paymentMethodEl.addEventListener("change", ()=>{
  const bankInfo = document.getElementById("bank-info");
  bankInfo.classList.toggle("hidden", paymentMethodEl.value !== "TRANSFER");
});

/* ========== CART OPEN/CLOSE ========== */
function openCart(){
  document.getElementById("cart-panel").classList.remove("hidden");
  document.getElementById("cart-overlay").classList.remove("hidden");
}
function closeCart(){
  document.getElementById("cart-panel").classList.add("hidden");
  document.getElementById("cart-overlay").classList.add("hidden");
}
function toggleCart(){
  const panel = document.getElementById("cart-panel");
  const overlay = document.getElementById("cart-overlay");
  panel.classList.toggle("hidden");
  overlay.classList.toggle("hidden");
}

/* ========== CHECKOUT FLOW ========== */
function openCheckout(){
  if(cart.length===0) return alert("Keranjang masih kosong!");
  document.getElementById("checkout-modal").classList.remove("hidden");
  document.getElementById("checkout-modal").classList.add("flex");
}

function closeCheckout(){
  document.getElementById("checkout-modal").classList.add("hidden");
  document.getElementById("checkout-modal").classList.remove("flex");
}

function submitCheckout(){
  // read customer data
  const name = document.getElementById("cust-name").value.trim();
  const phone = document.getElementById("cust-phone").value.trim();
  const address = document.getElementById("cust-address").value.trim();
  const city = document.getElementById("cust-city").value.trim();
  const postal = document.getElementById("cust-postal").value.trim();
  const payment = document.getElementById("payment-method").value;

  if(!name || !phone || !address || !city){
    return alert("Lengkapi Nama, Telepon, Alamat, dan Kota sebelum checkout.");
  }

  // build message
  const { subtotal, discount, shipping, total, km } = computeSummary();

  let lines = [];
  lines.push("Halo Tomodachi Matcha 👋");
  lines.push("Saya ingin melakukan pemesanan:");
  cart.forEach((it, idx) => {
    lines.push(`${idx+1}. ${it.name} — ${it.qty} x ${formatIDR(it.price)} = ${formatIDR(it.price * it.qty)}`);
  });
  lines.push(`Subtotal: ${formatIDR(subtotal)}`);
  if(appliedCoupon) lines.push(`Diskon (${appliedCoupon.code}): -${formatIDR(discount)}`);
  lines.push(`Ongkir (jarak ${km} km × Rp${RATE_PER_KM.toLocaleString()}): ${formatIDR(shipping)}`);
  lines.push(`Total: ${formatIDR(total)}`);
  lines.push("");
  lines.push("Metode Pembayaran: " + payment);
  if(payment === "TRANSFER"){
    lines.push("Rekening: BCA 1234567890 (A/N Tomodachi Matcha)");
  }
  lines.push("");
  lines.push("Data Pengiriman:");
  lines.push("Nama: " + name);
  lines.push("No HP: " + phone);
  lines.push("Alamat: " + address);
  lines.push("Kota: " + city);
  lines.push("Kode Pos: " + postal);

  const message = encodeURIComponent(lines.join("\n"));
  const waUrl = `https://wa.me/${SHOP_PHONE}?text=${message}`;

  window.open(waUrl, "_blank");
  // optional: clear cart after checkout:
  // cart = []; appliedCoupon = null; saveState(); renderCart(); closeCheckout(); closeCart();
}

/* ========== INITIALIZE ========== */
renderProducts();
renderCart();

// keep inputs reactive: recompute when distance changes
document.getElementById("distance-km").addEventListener("input", renderCart);
couponInput.addEventListener("input", ()=>{ if(!couponInput.value) { appliedCoupon=null; renderCart(); } });

</script>
</body>
</html>
