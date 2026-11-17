# index
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
        // Dữ liệu sản phẩm mẫu
        const products = [
            { id: 1, name: "MacBook Air M2 13 inch", price: 24500000, category: "laptop", img: "https://unsplash.com" },
            { id: 2, name: "Dell XPS 13 Plus", price: 32900000, category: "laptop", img: "https://unsplash.com" },
            { id: 3, name: "iPhone 15 Pro Max 256GB", price: 29900000, category: "phone", img: "https://unsplash.com" },
            { id: 4, name: "Samsung Galaxy S24 Ultra", price: 27500000, category: "phone", img: "https://unsplash.com" },
            { id: 5, name: "Tai nghe Sony WH-1000XM5", price: 6900000, category: "accessory", img: "https://unsplash.com" },
            { id: 6, name: "Bàn phím cơ Keychron K2", price: 1850000, category: "accessory", img: "https://unsplash.com" }
        ];

        let cart = [];
        let currentCategory = 'all';

        // Định dạng tiền VNĐ
        function formatMoney(amount) {
            return amount.toLocaleString('vi-VN') + ' ₫';
        }

        // Hiển thị danh sách sản phẩm
        function renderProducts(list) {
            const productList = document.getElementById('productList');
            productList.innerHTML = '';

            if (list.length === 0) {
                productList.innerHTML = '<p style="grid-column: 1/-1; text-align: center; color: #64748b;">Không tìm thấy sản phẩm phù hợp.</p>';
                return;
            }

            list.forEach(product => {
                productList.innerHTML += `
                    <div class="product-card">
                        <div>
                            <img src="${product.img}" alt="${product.name}" class="product-img">
                            <h3 class="product-title">${product.name}</h3>
                            <div class="product-price">${formatMoney(product.price)}</div>
                        </div>
+ Thêm vào giỏ

`;
});
}
// Lọc theo danh mục
function filterCategory(category, btn) {
currentCategory = category;
document.querySelectorAll('.cat-btn').forEach(b => b.classList.remove('active'));
btn.classList.add('active');
applyFilters();
}
// Tìm kiếm sản phẩm
document.getElementById('searchInput').addEventListener('input', applyFilters);
function applyFilters() {
const keyword = document.getElementById('searchInput').value.toLowerCase();
const filtered = products.filter(p => {
const matchCat = currentCategory === 'all' || p.category === currentCategory;
const matchName = p.name.toLowerCase().includes(keyword);
return matchCat && matchName;
});
renderProducts(filtered);
}
// Thêm vào giỏ hàng
function addToCart(id) {
const item = cart.find(i => i.id === id);
if (item) {
item.qty++;
} else {
const product = products.find(p => p.id === id);
cart.push({ ...product, qty: 1 });
}
updateCartUI();
}
// Tăng/giảm số lượng
function changeQty(id, delta) {
const item = cart.find(i => i.id === id);
if (!item) return;
item.qty += delta;
if (item.qty <= 0) {
cart = cart.filter(i => i.id !== id);
}
updateCartUI();
}
// Cập nhật giao diện giỏ hàng
function updateCartUI() {
const cartItems = document.getElementById('cartItems');
const cartCount = document.getElementById('cartCount');
const cartTotal = document.getElementById('cartTotal');
cartItems.innerHTML = '';
let totalQty = 0;
let totalPrice = 0;
if (cart.length === 0) {
cartItems.innerHTML = 'Giỏ hàng đang trống';
}
cart.forEach(item => {
totalQty += item.qty;
totalPrice += item.price * item.qty;
cartItems.innerHTML += <div class="cart-item"> <div> <strong>${item.name}</strong><br> <small>${formatMoney(item.price)}</small> </div> <div class="qty-controls"> <button onclick="changeQty(${item.id}, -1)">-</button> <span>${item.qty}</span> <button onclick="changeQty(${item.id}, 1)">+</button> </div> </div>;
});
cartCount.innerText = totalQty;
cartTotal.innerText = formatMoney(totalPrice);
}
// Bật/tắt cửa sổ giỏ hàng
function toggleCart() {
const modal = document.getElementById('cartModal');
modal.style.display = modal.style.display === 'flex' ? 'none' : 'flex';
}
// Thanh toán
function checkout() {
if (cart.length === 0) {
alert('Giỏ hàng của bạn đang trống!');
return;
}
alert('Đặt hàng thành công! Cảm ơn bạn đã mua sắm tại TechStore.');
cart = [];
updateCartUI();
toggleCart();
}
// Khởi chạy ban đầu
renderProducts(products);
updateCartUI();



