<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TechStore - Cửa Hàng Công Nghệ</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f4f6f9;
            color: #333;
        }

        /* Thanh điều hướng */
        .navbar {
            background-color: #1e293b;
            color: white;
            padding: 15px 5%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: sticky;
            top: 0;
            z-index: 100;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            gap: 15px;
        }

        .logo {
            font-size: 1.5rem;
            font-weight: bold;
            color: #38bdf8;
            white-space: nowrap;
        }

        .search-bar {
            flex-grow: 1;
            max-width: 450px;
        }

        .search-bar input {
            width: 100%;
            padding: 10px 15px;
            border-radius: 20px;
            border: none;
            outline: none;
        }

        .cart-btn {
            background-color: #38bdf8;
            color: #0f172a;
            border: none;
            padding: 10px 20px;
            border-radius: 8px;
            font-weight: bold;
            cursor: pointer;
            transition: 0.2s;
            white-space: nowrap;
        }

        .cart-btn:hover {
            background-color: #0284c7;
            color: white;
        }

        /* Bộ lọc danh mục */
        .categories {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 12px;
            margin: 25px 0;
            padding: 0 15px;
        }

        .cat-btn {
            padding: 8px 20px;
            border: 1px solid #cbd5e1;
            background: white;
            border-radius: 20px;
            cursor: pointer;
            font-weight: 500;
            transition: 0.2s;
        }

        .cat-btn.active, .cat-btn:hover {
            background-color: #1e293b;
            color: white;
            border-color: #1e293b;
        }

        /* Lưới sản phẩm */
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 20px 50px;
        }

        .product-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
            gap: 25px;
        }

        .product-card {
            background: white;
            border-radius: 12px;
            padding: 20px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05);
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            transition: transform 0.2s;
        }

        .product-card:hover {
            transform: translateY(-5px);
        }

        .product-img {
            width: 100%;
            height: 180px;
            object-fit: cover;
            border-radius: 8px;
            background-color: #e2e8f0;
            margin-bottom: 15px;
        }

        .product-title {
            font-size: 1.1rem;
            margin-bottom: 8px;
        }

        .product-price {
            color: #e11d48;
            font-weight: bold;
            font-size: 1.15rem;
            margin-bottom: 15px;
        }

        .add-btn {
            width: 100%;
            padding: 10px;
            background-color: #2563eb;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            transition: 0.2s;
        }

        .add-btn:hover {
            background-color: #1d4ed8;
        }

        /* Cửa sổ Giỏ hàng (Sidebar) */
        .cart-modal {
            display: none;
            position: fixed;
            top: 0; right: 0;
            width: 100%; height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 200;
            justify-content: flex-end;
        }

        .cart-content {
            width: 400px;
            max-width: 100%;
            background: white;
            height: 100%;
            padding: 20px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }

        .cart-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #e2e8f0;
            padding-bottom: 15px;
        }

        .close-btn {
            background: none;
            border: none;
            font-size: 1.2rem;
            cursor: pointer;
        }

        .cart-items {
            flex-grow: 1;
            overflow-y: auto;
            margin: 15px 0;
        }

        .cart-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 0;
            border-bottom: 1px solid #f1f5f9;
        }

        .qty-controls {
            display: flex;
            align-items: center;
        }

        .qty-controls button {
            padding: 2px 8px;
            margin: 0 5px;
            cursor: pointer;
            border: 1px solid #cbd5e1;
            background: #f8fafc;
            border-radius: 4px;
        }

        .cart-footer {
            border-top: 1px solid #e2e8f0;
            padding-top: 15px;
        }

        .total {
            font-size: 1.2rem;
            font-weight: bold;
            margin-bottom: 15px;
            display: flex;
            justify-content: space-between;
        }

        .checkout-btn {
            width: 100%;
            padding: 12px;
            background-color: #16a34a;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
        }

        .checkout-btn:hover {
            background-color: #15803d;
        }
    </style>
</head>
<body>
    <!-- Thanh điều hướng -->
    <header class="navbar">
        <div class="logo">🛒 TechStore</div>
        <div class="search-bar">
            <input type="text" id="searchInput" placeholder="Tìm kiếm laptop, điện thoại, tai nghe...">
        </div>
        <button class="cart-btn" onclick="toggleCart()">
            Giỏ hàng (<span id="cartCount">0</span>)
        </button>
    </header>

    <!-- Bộ lọc danh mục -->
    <section class="categories">
        <button class="cat-btn active" onclick="filterCategory('all', this)">Tất cả</button>
        <button class="cat-btn" onclick="filterCategory('laptop', this)">Laptop</button>
        <button class="cat-btn" onclick="filterCategory('phone', this)">Điện thoại</button>
        <button class="cat-btn" onclick="filterCategory('accessory', this)">Phụ kiện</button>
    </section>

    <!-- Danh sách sản phẩm -->
    <main class="container">
        <div class="product-grid" id="productList"></div>
    </main>

    <!-- Cửa sổ Giỏ hàng -->
    <div class="cart-modal" id="cartModal">
        <div class="cart-content">
            <div class="cart-header">
                <h2>Giỏ hàng của bạn</h2>
                <button class="close-btn" onclick="toggleCart()">✖</button>
            </div>
            <div class="cart-items" id="cartItems"></div>
            <div class="cart-footer">
                <div class="total">Tổng cộng: <span id="cartTotal">0 ₫</span></div>
                <button class="checkout-btn" onclick="checkout()">Thanh toán ngay</button>
            </div>
        </div>
    </div>

    <script>
        // Dữ liệu sản phẩm mẫu đã chuẩn hóa link ảnh
        const products = [
            { id: 1, name: "MacBook Air M2 13 inch", price: 24500000, category: "laptop", img: "https://unsplash.com" },
            { id: 2, name: "Dell XPS 13 Plus", price: 32900000, category: "laptop", img: "https://unsplash.com" },
            { id: 3, name: "iPhone 15 Pro Max 256GB", price: 29900000, category: "phone", img: "https://unsplash.com" },
            { id: 4, name: "Samsung Galaxy S24 Ultra", price: 27500000, category: "phone", img: "https://unsplash.com" },
            { id: 5, name: "Tai nghe Sony WH-1000XM5", price: 6990000, category: "accessory", img: "https://unsplash.com" },
            { id: 6, name: "Chuột Logitech MX Master 3S", price: 2290000, category: "accessory", img: "https://unsplash.com" }
        ];

        // Khôi phục giỏ hàng từ localStorage nếu có
        let cart = JSON.parse(localStorage.getItem('techstore_cart')) || [];
        let currentCategory = 'all';

        const formatPrice = (price) => price.toLocaleString('vi-VN') + ' ₫';

        function saveCart() {
            localStorage.setItem('techstore_cart', JSON.stringify(cart));
        }

        // Hiển thị danh sách sản phẩm
        function renderProducts() {
            const list = document.getElementById('productList');
            const keyword = document.getElementById('searchInput').value.toLowerCase();

            const filtered = products.filter(p => {
                const matchCat = currentCategory === 'all' || p.category === currentCategory;
                const matchKey = p.name.toLowerCase().includes(keyword);
                return matchCat && matchKey;
});
if (filtered.length === 0) {
list.innerHTML = 'Không tìm thấy sản phẩm phù hợp.';
return;
}
list.innerHTML = filtered.map(p => <div class="product-card"> <div> <img src="${p.img}" alt="${p.name}" class="product-img"> <h3 class="product-title">${p.name}</h3> </div> <div> <div class="product-price">${formatPrice(p.price)}</div> <button class="add-btn" onclick="addToCart(${p.id})">+ Thêm vào giỏ</button> </div> </div>).join('');
}
// Lọc theo danh mục
function filterCategory(category, btn) {
currentCategory = category;
document.querySelectorAll('.cat-btn').forEach(b => b.classList.remove('active'));
btn.classList.add('active');
renderProducts();
}
// Tìm kiếm theo từ khóa
document.getElementById('searchInput').addEventListener('input', renderProducts);
// Thêm vào giỏ hàng
function addToCart(id) {
const product = products.find(p => p.id === id);
const existing = cart.find(item => item.id === id);
if (existing) {
existing.qty += 1;
} else {
cart.push({ ...product, qty: 1 });
}
saveCart();
updateCartUI();
}
// Thay đổi số lượng trong giỏ
function changeQty(id, delta) {
const item = cart.find(i => i.id === id);
if (!item) return;
item.qty += delta;
if (item.qty <= 0) {
cart = cart.filter(i => i.id !== id);
}
saveCart();
updateCartUI();
}
// Cập nhật giao diện giỏ hàng
function updateCartUI() {
const totalQty = cart.reduce((sum, item) => sum + item.qty, 0);
const totalPrice = cart.reduce((sum, item) => sum + item.price * item.qty, 0);
document.getElementById('cartCount').textContent = totalQty;
document.getElementById('cartTotal').textContent = formatPrice(totalPrice);
const cartItemsEl = document.getElementById('cartItems');
if (cart.length === 0) {
cartItemsEl.innerHTML = 'Giỏ hàng đang trống.';
return;
}
cartItemsEl.innerHTML = cart.map(item => <div class="cart-item"> <div> <div style="font-weight: 600;">${item.name}</div> <div style="color: #e11d48; font-size: 0.9rem;">${formatPrice(item.price)}</div> </div> <div class="qty-controls"> <button onclick="changeQty(${item.id}, -1)">-</button> <span>${item.qty}</span> <button onclick="changeQty(${item.id}, 1)">+</button> </div> </div>).join('');
}
// Bật/tắt cửa sổ giỏ hàng
function toggleCart() {
const modal = document.getElementById('cartModal');
modal.style.display = modal.style.display === 'flex' ? 'none' : 'flex';
}
// Đóng giỏ hàng khi bấm ra vùng tối bên ngoài
window.onclick = function(event) {
const modal = document.getElementById('cartModal');
if (event.target === modal) {
modal.style.display = 'none';
}
}
// Thanh toán
function checkout() {
if (cart.length === 0) {
alert('Giỏ hàng của bạn đang trống!');
return;
}
alert('Cảm ơn bạn đã đặt hàng tại TechStore! Đơn hàng đang được xử lý.');
cart = [];
saveCart();
updateCartUI();
toggleCart();
}
// Khởi chạy ban đầu
renderProducts();
updateCartUI();

