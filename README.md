<!DOCTYPE html>
<html lang="uk">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Вироби лобзиком</title>
<style>
  body {
    margin: 0; padding: 0;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #f0f4f8, #d9e2ec);
    color: #333;
  }
  header {
    background-color: #356859;
    color: #e0e0e0;
    padding: 15px 30px;
    text-align: center;
    font-size: 28px;
    font-weight: bold;
    letter-spacing: 2px;
    position: relative;
  }
  nav {
    background: #2a4d42;
    display: flex;
    justify-content: center;
    gap: 30px;
    padding: 10px 0;
  }
  nav button {
    background: transparent;
    border: none;
    color: #e0e0e0;
    font-size: 18px;
    cursor: pointer;
    padding: 8px 20px;
    border-radius: 6px;
    transition: background-color 0.3s;
  }
  nav button:hover, nav button.active {
    background-color: #4a7a6a;
  }
  main {
    max-width: 1100px;
    margin: 30px auto 60px;
    padding: 0 20px;
  }
  .products-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill,minmax(220px,1fr));
    gap: 20px;
  }
  .product-card {
    background: white;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgb(0 0 0 / 0.1);
    display: flex;
    flex-direction: column;
    overflow: hidden;
    cursor: pointer;
    transition: transform 0.2s;
  }
  .product-card:hover {
    transform: scale(1.05);
  }
  .product-card img {
    width: 100%;
    height: 140px;
    object-fit: cover;
  }
  .product-info {
    padding: 15px;
    flex-grow: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  .product-name {
    font-weight: 700;
    font-size: 18px;
    margin-bottom: 6px;
  }
  .product-desc {
    font-size: 14px;
    color: #555;
    margin-bottom: 10px;
    flex-grow: 1;
  }
  .product-details {
    font-size: 13px;
    color: #666;
    margin-bottom: 12px;
  }
  .btn-cart {
    background-color: #4a7a6a;
    color: white;
    border: none;
    padding: 10px 0;
    font-weight: 600;
    border-radius: 6px;
    cursor: pointer;
    transition: background-color 0.25s;
  }
  .btn-cart:hover {
    background-color: #356859;
  }

  /* Кнопка відкриття кошика */
  #cart-button {
    position: fixed;
    top: 20px;
    right: 20px;
    background-color: #e03e2f;
    color: white;
    font-weight: 700;
    font-size: 16px;
    padding: 10px 16px;
    border-radius: 30px;
    cursor: pointer;
    z-index: 1100;
    box-shadow: 0 0 10px #e03e2faa;
    border: none;
  }
  /* Бічна панель кошика */
  #cart-sidebar {
    position: fixed;
    top: 0;
    right: -350px; /* прихована поза екраном */
    width: 350px;
    height: 100vh;
    background: white;
    box-shadow: -3px 0 15px rgba(0,0,0,0.2);
    padding: 20px;
    box-sizing: border-box;
    transition: right 0.3s ease;
    z-index: 1099;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
  }
  #cart-sidebar.open {
    right: 0; /* показати панель */
  }
  #cart-sidebar h2 {
    margin-top: 0;
    margin-bottom: 20px;
    color: #356859;
  }
  .cart-item {
    display: flex;
    gap: 12px;
    margin-bottom: 15px;
    border-bottom: 1px solid #ddd;
    padding-bottom: 10px;
  }
  .cart-item img {
    width: 70px;
    height: 70px;
    object-fit: cover;
    border-radius: 8px;
  }
  .cart-item-info {
    flex-grow: 1;
  }
  .cart-item-name {
    font-weight: 600;
    margin-bottom: 6px;
  }
  .cart-item-price {
    color: #555;
    font-size: 14px;
  }
  .cart-item-remove {
    cursor: pointer;
    font-weight: 700;
    color: #e03e2f;
    font-size: 18px;
    border: none;
    background: transparent;
  }
  #cart-total {
    margin-top: auto;
    font-weight: 700;
    font-size: 18px;
    text-align: right;
    color: #356859;
  }
</style>
</head>
<body>

<header>Вироби лобзиком</header>

<nav>
  <button class="nav-btn active" data-category="all">Всі</button>
  <button class="nav-btn" data-category="тварини">Тварини</button>
  <button class="nav-btn" data-category="іграшки">Іграшки</button>
  <button class="nav-btn" data-category="пазли">Пазли</button>
</nav>

<main>
  <div class="products-grid" id="products-grid"></div>
</main>

<!-- Кнопка для відкриття кошика -->
<button id="cart-button">Кошик (0)</button>

<!-- Бічна панель кошика -->
<div id="cart-sidebar" aria-hidden="true">
  <h2>Кошик</h2>
  <div id="cart-items"></div>
  <div id="cart-total">Всього: 0 грн</div>
</div>

<script>
  const products = [
  {
    id: 2,
    name: "Дерев'яна машинка",
    category: "іграшки",
    size: "15x8 см",
    price: 120,
    img: "https://cdn.pixabay.com/photo/2017/03/14/17/40/car-2142521_960_720.jpg",
    description: "Класична машинка з натурального дерева."
  },
  {
    id: 3,
    name: "Фанерний кораблик",
    category: "іграшки",
    size: "20x10 см",
    price: 140,
    img: "https://cdn.pixabay.com/photo/2018/06/25/15/42/toy-3496630_960_720.jpg",
    description: "Мініатюрний кораблик з фанери, вирізаний лобзиком."
  },
  {
    id: 4,
    name: "Дерев'яна сова",
    category: "тварини",
    size: "12x15 см",
    price: 160,
    img: "https://cdn.pixabay.com/photo/2014/04/03/11/11/owl-310496_960_720.png",
    description: "Фігурка сови з фанери, виготовлена вручну."
  },
  {
    id: 5,
    name: "Фанерний пазл",
    category: "іграшки",
    size: "25x25 см",
    price: 180,
    img: "https://cdn.pixabay.com/photo/2019/11/19/10/44/puzzle-4631337_960_720.jpg",
    description: "Пазл з фанери, що складається з 50 деталей."
  },
  {
    id: 6,
    name: "Дерев'яний підставка для книг",
    category: "декор",
    size: "20x15x10 см",
    price: 220,
    img: "https://cdn.pixabay.com/photo/2016/03/05/19/01/bookshelf-1238242_960_720.jpg",
    description: "Підставка для книг з фанери, міцна і стильна."
  },
  {
    id: 7,
    name: "Фанерний настільний органайзер",
    category: "канцтовари",
    size: "25x10x10 см",
    price: 210,
    img: "https://cdn.pixabay.com/photo/2018/01/02/10/02/organizer-3052618_960_720.jpg",
    description: "Органайзер для ручок і нотаток з фанери."
  },
  {
    id: 8,
    name: "Дерев'яна фоторамка",
    category: "декор",
    size: "20x25 см",
    price: 170,
    img: "https://cdn.pixabay.com/photo/2016/04/23/22/00/frame-1343536_960_720.jpg",
    description: "Рамка з фанери для фото, виготовлена лобзиком."
  },
  {
    id: 9,
    name: "Фанерна коробка для прикрас",
    category: "декор",
    size: "15x10x5 см",
    price: 190,
    img: "https://cdn.pixabay.com/photo/2017/06/23/13/18/box-2437677_960_720.jpg",
    description: "Красива коробка з фанери для зберігання прикрас."
  },
  {
    id: 10,
    name: "Дерев'яний ящик для спецій",
    category: "декор",
    size: "25x15x10 см",
    price: 230,
    img: "https://cdn.pixabay.com/photo/2016/02/13/12/26/spices-1190820_960_720.jpg",
    description: "Ящик з фанери для спецій на кухню."
  },
  {
    id: 11,
    name: "Фанерна підставка для телефону",
    category: "аксесуари",
    size: "10x7 см",
    price: 120,
    img: "https://cdn.pixabay.com/photo/2017/04/05/01/06/phone-2207446_960_720.jpg",
    description: "Зручна підставка для телефону з фанери."
  },
  {
    id: 12,
    name: "Дерев'яна іграшка-лабіринт",
    category: "іграшки",
    size: "20x20 см",
    price: 250,
    img: "https://cdn.pixabay.com/photo/2016/07/30/21/48/maze-1550536_960_720.jpg",
    description: "Іграшка-лабіринт з фанери для розвитку моторики."
  },
  {
    id: 13,
    name: "Фанерна підставка для чашки",
    category: "декор",
    size: "10x10 см",
    price: 80,
    img: "https://cdn.pixabay.com/photo/2017/06/24/08/45/coaster-2437017_960_720.jpg",
    description: "Підставка для чашки з фанери, захищає поверхню."
  },
    {
      id: 1,
      name: "Дерев'яний олень",
      category: "тварини",
      size: "15x20 см",
      price: 150,
      img: "https://cdn.pixabay.com/photo/2016/04/15/12/15/animal-1334846_960_720.jpg",
      description: "Красивий виріб у вигляді оленя, зроблений лобзиком."
    },
    {
      id: 2,
      name: "Дерев'яна сова",
      category: "тварини",
      size: "12x12 см",
      price: 130,
      img: "https://cdn.pixabay.com/photo/2018/08/11/21/27/owl-3595579_960_720.jpg",
      description: "Мила дерев'яна сова, гарна для декору."
    },
    {
      id: 3,
      name: "Дерев'яний ведмідь",
      category: "тварини",
      size: "18x15 см",
      price: 200,
      img: "https://cdn.pixabay.com/photo/2017/09/06/22/26/bear-2723089_960_720.jpg",
      description: "Симпатичний ведмідь з натурального дерева."
    },
    {
      id: 4,
      name: "Іграшковий поїзд",
      category: "іграшки",
      size: "30x10 см",
      price: 250,
      img: "https://cdn.pixabay.com/photo/2017/12/04/21/51/toy-2992607_960_720.jpg",
      description: "Яскравий поїзд для дітей."
    },
    {
      id: 5,
      name: "Пазл з дерева",
      category: "пазли",
      size: "25x25 см",
      price: 220,
      img: "https://cdn.pixabay.com/photo/2017/11/29/03/02/puzzle-2984796_960_720.jpg",
      description: "Складний пазл з дерева."
    },
    {
      id: 6,
      name: "Іграшковий вертоліт",
      category: "іграшки",
      size: "25x15 см",
      price: 270,
      img: "https://cdn.pixabay.com/photo/2017/11/22/23/04/wood-2976770_960_720.jpg",
      description: "Дерев'яний вертоліт для дітей."
    },
  ];

  const cart = [];

  function renderProducts(category = "all") {
    const grid = document.getElementById("products-grid");
    let filtered = products;
    if (category !== "all") {
      filtered = products.filter(p => p.category === category);
    }

    grid.innerHTML = filtered.map(p => `
      <div class="product-card" data-id="${p.id}" title="${p.description}">
        <img src="${p.img}" alt="${p.name}" />
        <div class="product-info">
          <div class="product-name">${p.name}</div>
          <div class="product-desc">${p.description}</div>
          <div class="product-details">Розмір: ${p.size} | Ціна: ${p.price} грн</div>
          <button class="btn-cart" onclick="addToCart(${p.id})">Додати в кошик</button>
        </div>
      </div>
    `).join("");
  }

  function updateCartCounter() {
    document.getElementById("cart-button").textContent = `Кошик (${cart.length})`;
  }

  function renderCart() {
    const cartItemsContainer = document.getElementById("cart-items");
    if (cart.length === 0) {
      cartItemsContainer.innerHTML = "<p>Кошик порожній.</p>";
      document.getElementById("cart-total").textContent = "Всього: 0 грн";
      return;
    }

    let total = 0;
    cartItemsContainer.innerHTML = cart.map((item, idx) => {
      total += item.price;
      return `
        <div class="cart-item">
          <img src="${item.img}" alt="${item.name}" />
          <div class="cart-item-info">
            <div class="cart-item-name">${item.name}</div>
            <div class="cart-item-price">${item.price} грн</div>
          </div>
          <button class="cart-item-remove" onclick="removeFromCart(${idx})" title="Видалити">×</button>
        </div>
      `;
    }).join("");

    document.getElementById("cart-total").textContent = `Всього: ${total} грн`;
  }

  function addToCart(id) {
    const product = products.find(p => p.id === id);
    if (!product) {
      alert("Продукт не знайдено");
      return;
    }
    cart.push(product);
    updateCartCounter();
    renderCart();
  }

  function removeFromCart(index) {
    cart.splice(index, 1);
    updateCartCounter();
    renderCart();
  }

  // Відкриття / закриття кошика
  const cartButton = document.getElementById("cart-button");
  const cartSidebar = document.getElementById("cart-sidebar");

  cartButton.addEventListener("click", () => {
    if (cartSidebar.classList.contains("open")) {
      cartSidebar.classList.remove("open");
      cartSidebar.setAttribute("aria-hidden", "true");
    } else {
      cartSidebar.classList.add("open");
      cartSidebar.setAttribute("aria-hidden", "false");
    }
  });

  // Навігація по категоріях
  document.querySelectorAll("nav .nav-btn").forEach(btn => {
    btn.addEventListener("click", e => {
      document.querySelectorAll("nav .nav-btn").forEach(b => b.classList.remove("active"));
      btn.classList.add("active");
      renderProducts(btn.dataset.category);
    });
  });
  html, body {
  height: 100%;
  width: 100%;
}

main {
  min-height: calc(100vh - 140px); /* 100% висоти мінус шапка та меню */
  display: flex;
  flex-direction: column;
  justify-content: start;
}

  renderProducts();
  updateCartCounter();
  renderCart();

</script>

</body>
</html>
