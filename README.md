<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FASHION STORE</title>
<style>
* {box-sizing: border-box;}
body {
    margin: 0;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    color: #fff;
    background: url('[images.unsplash.com](https://images.unsplash.com/photo-1521334884684-d80222895322)') center/cover fixed;
    min-height: 100vh;
}

header {
    padding: 20px;
    text-align: center;
    background: rgba(0,0,0,0.9);
    font-size: 28px;
    font-weight: bold;
    box-shadow: 0 2px 20px rgba(0,0,0,0.5);
}

.container {
    max-width: 1200px;
    margin: auto;
    padding: 20px;
}

.topbar {
    display: flex;
    gap: 15px;
    margin-bottom: 25px;
    align-items: center;
    flex-wrap: wrap;
}

input {
    padding: 12px 16px;
    border-radius: 25px;
    border: none;
    width: 300px;
    max-width: 100%;
    font-size: 16px;
    background: rgba(255,255,255,0.95);
    color: #333;
    box-shadow: 0 4px 15px rgba(0,0,0,0.3);
}

.btn {
    background: linear-gradient(45deg, #ff3c3c, #ff6a6a);
    padding: 12px 20px;
    border-radius: 25px;
    cursor: pointer;
    box-shadow: 0 4px 15px rgba(255,60,60,0.4);
    border: none;
    font-weight: bold;
    transition: all 0.3s ease;
    display: flex;
    align-items: center;
    gap: 8px;
}

.btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 6px 20px rgba(255,60,60,0.6);
}

.products {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 20px;
    margin-bottom: 20px;
}

.card {
    background: rgba(0,0,0,0.85);
    padding: 20px;
    border-radius: 20px;
    text-align: center;
    transition: all 0.3s ease;
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255,255,255,0.1);
}

.card:hover {
    transform: translateY(-10px);
    box-shadow: 0 20px 40px rgba(0,0,0,0.5);
    background: rgba(0,0,0,0.95);
}

.card img {
    width: 100%;
    height: 200px;
    object-fit: cover;
    border-radius: 15px;
    margin-bottom: 15px;
}

.card h3 {
    margin: 10px 0;
    font-size: 18px;
}

.price {
    color: #ff3c3c;
    font-size: 24px;
    font-weight: bold;
    margin: 15px 0;
}

.desc {
    font-size: 14px;
    color: #ddd;
    margin-bottom: 15px;
    line-height: 1.4;
}

.cart {
    position: fixed;
    right: -350px;
    top: 0;
    width: 340px;
    height: 100%;
    background: linear-gradient(180deg, rgba(20,20,20,0.98), rgba(255,60,60,0.15));
    padding: 25px;
    transition: right 0.3s ease;
    backdrop-filter: blur(15px);
    border-left: 3px solid #ff3c3c;
    box-shadow: -15px 0 50px rgba(255,60,60,0.3);
    overflow-y: auto;
    z-index: 1000;
}

.cart.active {
    right: 0;
}

.cart h3 {
    margin: 0 0 20px 0;
    font-size: 24px;
    text-align: center;
}

.close-btn {
    position: absolute;
    top: 20px;
    right: 20px;
    background: #ff3c3c;
    border: none;
    padding: 10px;
    border-radius: 50%;
    color: #fff;
    cursor: pointer;
    font-size: 18px;
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.cart-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px 0;
    border-bottom: 1px solid rgba(255,255,255,0.1);
    margin-bottom: 10px;
}

.cart-item-name {
    flex: 1;
    font-size: 16px;
}

.remove {
    background: #ff3c3c;
    padding: 6px 10px;
    border-radius: 15px;
    cursor: pointer;
    color: #fff;
    font-size: 14px;
    border: none;
    transition: all 0.2s;
}

.remove:hover {
    background: #cc3333;
    transform: scale(1.05);
}

#total {
    font-size: 24px;
    font-weight: bold;
    text-align: center;
    margin-top: 20px;
    padding: 15px;
    background: rgba(255,255,255,0.1);
    border-radius: 15px;
}

.empty-cart {
    text-align: center;
    color: #ccc;
    font-size: 18px;
    margin-top: 50px;
}

@media (max-width: 768px) {
    .topbar {flex-direction: column;}
    input {width: 100%;}
    .products {grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));}
    .cart {width: 100%; right: -100%;}
}
</style>
</head>
<body>

<header>FASHION STORE 👕</header>

<div class="container">
    <div class="topbar">
        <input id="search" placeholder="🔍 Поиск одежды..." oninput="searchProducts(this.value)">
        <div class="btn" onclick="toggleCart()">
            🛒 <span id="cartCount">0</span>
        </div>
    </div>

    <div class="products" id="products"></div>
</div>

<div class="cart" id="cart">
    <button class="close-btn" onclick="toggleCart()">✖</button>
    <h3>🛒 Корзина</h3>
    <div id="cartItems"></div>
    <h4 id="total">Итого: 0 грн</h4>
</div>

<script>
let products = [
    {name:'Футболка Oversize White', price:800, desc:'Свободная белая футболка из хлопка, идеально для повседневного стиля.', img:'[images.unsplash.com](https://images.unsplash.com/photo-1520975922284-9e0a1bb8c3b2?auto=format&fit=crop&w=400&q=80)'},
    {name:'Футболка Oversize Black', price:800, desc:'Чёрная oversize футболка — базовый элемент гардероба.', img:'[images.unsplash.com](https://images.unsplash.com/photo-1503342217505-b0a15ec3261c?auto=format&fit=crop&w=400&q=80)'},
    {name:'Худи Black', price:1500, desc:'Тёплое чёрное худи с капюшоном, идеально для streetwear.', img:'[images.unsplash.com](https://images.unsplash.com/photo-1602810318383-e386cc2a3ccf?auto=format&fit=crop&w=400&q=80)'},
    {name:'Худи Grey', price:1500, desc:'Серое худи с мягкой тканью и удобной посадкой.', img:'[images.unsplash.com](https://images.unsplash.com/photo-1523381210434-271e8be1f52b?auto=format&fit=crop&w=400&q=80)'},
    {name:'Джинсы Slim Blue', price:1800, desc:'Синие джинсы slim fit — стиль и комфорт.', img:'[images.unsplash.com](https://images.unsplash.com/photo-1541099649105-f69ad21f3246?auto=format&fit=crop&w=400&q=80)'},
    {name:'Джинсы Black', price:1800, desc:'Чёрные джинсы — универсальный вариант на каждый день.', img:'[images.unsplash.com](https://images.unsplash.com/photo-1514997130083-0b3b54e2b43b?auto=format&fit=crop&w=400&q=80)'},
    {name:'Куртка Street Black', price:3200, desc:'Стильная чёрная куртка для холодной погоды.', img:'[images.unsplash.com](https://images.unsplash.com/photo-1539533018447-63fcce2678e3?auto=format&fit=crop&w=400&q=80)'},
    {name:'Куртка Street Red', price:3200, desc:'Яркая красная куртка — выделяйся из толпы.', img:'[images.unsplash.com](https://images.unsplash.com/photo-1600185365483-26d7a4cc7519?auto=format&fit=crop&w=400&q=80)'},
    {name:'Кепка Street', price:600, desc:'Стильная кепка для повседневного образа.', img:'[images.unsplash.com](https://images.unsplash.com/photo-1512436991641-6745cdb1723f?auto=format&fit=crop&w=400&q=80)'},
    {name:'Кроссовки White', price:2800, desc:'Белые кроссовки — идеальны под любой стиль.', img:'[images.unsplash.com](https://images.unsplash.com/photo-1528701800489-20be3c7b3a5f?auto=format&fit=crop&w=400&q=80)'}
];

let cart = [];

function renderProducts(productList) {
    const container = document.getElementById('products');
    container.innerHTML = '';
    
    productList.forEach(product => {
        const card = document.createElement('div');
        card.className = 'card';
        card.innerHTML = `
            <img src="${product.img}" alt="${product.name}" loading="lazy">
            <h3>${product.name}</h3>
            <div class="desc">${product.desc}</div>
            <div class="price">${product.price.toLocaleString()} грн</div>
            <button class="btn" onclick="addToCart('${product.name}', ${product.price})">Добавить в корзину</button>
        `;
        container.appendChild(card);
    });
}

function searchProducts(query) {
    const filtered = products.filter(product => 
        product.name.toLowerCase().includes(query.toLowerCase())
    );
    renderProducts(filtered);
}

function addToCart(name, price) {
    // Проверяем, есть ли уже такой товар
    const existingItem = cart.find(item => item.name === name);
    if (existingItem) {
        existingItem.quantity += 1;
    } else {
        cart.push({name, price, quantity: 1});
    }
    updateCart();
    // Визуальная обратная связь
    showNotification(`${name} добавлен в корзину!`);
}

function removeFromCart(index) {
    cart.splice(index, 1);
    updateCart();
}

function updateCart() {
    const itemsContainer = document.getElementById('cartItems');
    const cartCount = document.getElementById('cartCount');
    const totalElement = document.getElementById('total');
    
    let total = 0;
    let totalItems = 0;
    
    itemsContainer.innerHTML = '';
    
    if (cart.length === 0) {
        itemsContainer.innerHTML = '<div class="empty-cart">Корзина пуста 😢</div>';
    } else {
        cart.forEach((item, index) => {
            const itemTotal = item.price * item.quantity;
            total += itemTotal;
            totalItems += item.quantity;
            
            const cartItem = document.createElement('div');
            cartItem.className = 'cart-item';
            cartItem.innerHTML = `
                <div class="cart-item-name">
                    ${item.name} ×${item.quantity} - ${itemTotal.toLocaleString()} грн
                </div>
                <button class="remove" onclick="removeFromCart(${index})">Удалить</button>
            `;
            itemsContainer.appendChild(cartItem);
        });
    }
    
    cartCount.textContent = totalItems;
    totalElement.textContent = `Итого: ${total.toLocaleString()} грн`;
}

function toggleCart() {
    document.getElementById('cart').classList.toggle('active');
}

function showNotification(message) {
    // Простое уведомление
    const notification = document.createElement('div');
    notification.style.cssText = `
        position: fixed; top: 20px; right: 20px; 
        background: #4CAF50; color: white; padding: 15px 20px; 
        border-radius: 10px; z-index: 1001; 
        animation: slideIn 0.3s ease;
    `;
    notification.textContent = message;
    document.body.appendChild(notification);
    
    setTimeout(() => {
        notification.remove();
    }, 3000);
}

// Инициализация
renderProducts(products);
updateCart();
</script>

</body>
</html>
