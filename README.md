<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>FASHION STORE</title>
<style>
body{margin:0;font-family:Arial;color:#fff;background:url('https://images.unsplash.com/photo-1521334884684-d80222895322') center/cover fixed}
header{padding:20px;text-align:center;background:rgba(0,0,0,0.8);font-size:24px}
.container{max-width:1200px;margin:auto;padding:20px}
.topbar{display:flex;gap:10px;margin-bottom:15px}
input{padding:10px;border-radius:8px;border:none;width:100%}
.btn{background:linear-gradient(90deg,#ff3c3c,#ff6a6a);padding:10px;border-radius:8px;cursor:pointer;box-shadow:0 0 10px rgba(255,60,60,0.6)}
.products{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:15px}
.card{background:rgba(0,0,0,0.7);padding:15px;border-radius:12px;text-align:center}
.card img{width:100%;border-radius:10px}
.price{color:#ff3c3c;margin:10px 0}
.desc{font-size:13px;color:#ccc;margin-bottom:10px}
.cart{position:fixed;right:-320px;top:0;width:300px;height:100%;background:linear-gradient(180deg, rgba(20,20,20,0.95), rgba(255,60,60,0.2));padding:20px;transition:.3s;backdrop-filter:blur(10px);border-left:2px solid #ff3c3c;box-shadow:-10px 0 30px rgba(255,60,60,0.3)}
.active{right:0}
.remove{background:#ff3c3c;padding:4px 7px;border-radius:6px;margin-left:5px;cursor:pointer;color:#fff}
</style>
</head>
<body>

<header>FASHION STORE 👕</header>

<div class="container">

<div class="topbar">
<input placeholder="Поиск одежды..." oninput="search(this.value)">
<div class="btn" onclick="toggleCart()">🛒 <span id="count">0</span></div>
</div>

<div class="products" id="products"></div>

</div>

<div class="cart" id="cart">
<button onclick="toggleCart()" style="background:#ff3c3c;border:none;padding:6px 10px;border-radius:8px;color:#fff;cursor:pointer;float:right">✖</button>
<h3>Корзина</h3>
<div id="items"></div>
<h4 id="total"></h4>
</div>

<script>
let products=[
{name:'Футболка Oversize White',price:800,desc:'Свободная белая футболка из хлопка, идеально для повседневного стиля.',img:'https://images.unsplash.com/photo-1520975922284-9e0a1bb8c3b2?auto=format&fit=crop&w=400&q=80'},
{name:'Футболка Oversize Black',price:800,desc:'Чёрная oversize футболка — базовый элемент гардероба.',img:'https://images.unsplash.com/photo-1503342217505-b0a15ec3261c?auto=format&fit=crop&w=400&q=80'},
{name:'Худи Black',price:1500,desc:'Тёплое чёрное худи с капюшоном, идеально для streetwear.',img:'https://images.unsplash.com/photo-1602810318383-e386cc2a3ccf?auto=format&fit=crop&w=400&q=80'},
{name:'Худи Grey',price:1500,desc:'Серое худи с мягкой тканью и удобной посадкой.',img:'https://images.unsplash.com/photo-1523381210434-271e8be1f52b?auto=format&fit=crop&w=400&q=80'},
{name:'Джинсы Slim Blue',price:1800,desc:'Синие джинсы slim fit — стиль и комфорт.',img:'https://images.unsplash.com/photo-1541099649105-f69ad21f3246?auto=format&fit=crop&w=400&q=80'},
{name:'Джинсы Black',price:1800,desc:'Чёрные джинсы — универсальный вариант на каждый день.',img:'https://images.unsplash.com/photo-1514997130083-0b3b54e2b43b?auto=format&fit=crop&w=400&q=80'},
{name:'Куртка Street Black',price:3200,desc:'Стильная чёрная куртка для холодной погоды.',img:'https://images.unsplash.com/photo-1539533018447-63fcce2678e3?auto=format&fit=crop&w=400&q=80'},
{name:'Куртка Street Red',price:3200,desc:'Яркая красная куртка — выделяйся из толпы.',img:'https://images.unsplash.com/photo-1600185365483-26d7a4cc7519?auto=format&fit=crop&w=400&q=80'},
{name:'Кепка Street',price:600,desc:'Стильная кепка для повседневного образа.',img:'https://images.unsplash.com/photo-1512436991641-6745cdb1723f?auto=format&fit=crop&w=400&q=80'},
{name:'Кроссовки White',price:2800,desc:'Белые кроссовки — идеальны под любой стиль.',img:'https://images.unsplash.com/photo-1528701800489-20be3c7b3a5f?auto=format&fit=crop&w=400&q=80'}
];

let cart=[];

function render(list){
let el=document.getElementById('products');el.innerHTML='';
list.forEach(p=>{
el.innerHTML+=`<div class='card'>
<img src='${p.img}'>
<h3>${p.name}</h3>
<div class='desc'>${p.desc}</div>
<div class='price'>${p.price} грн</div>
<div class='btn' onclick="add('${p.name}',${p.price})">Купить</div>
</div>`});}

function search(v){render(products.filter(p=>p.name.toLowerCase().includes(v.toLowerCase())))}

function add(name,price){cart.push({name,price});update();}

function removeItem(index){
cart.splice(index,1);
update();
}

function update(){
let el=document.getElementById('items');let t=0;el.innerHTML='';
cart.forEach((i,idx)=>{
t+=i.price;
el.innerHTML+=`${i.name} - ${i.price} грн <span class='remove' onclick='removeItem(${idx})'>❌</span><br>`;
});
document.getElementById('count').innerText=cart.length;
document.getElementById('total').innerText='Итого '+t+' грн';
}

function toggleCart(){document.getElementById('cart').classList.toggle('active')}

render(products);
</script>

</body>
</html>
