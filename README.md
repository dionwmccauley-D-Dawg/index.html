# index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GreenLiving - Sustainable Products</title>
    <meta name="description" content="Shop eco-friendly products like reusable water bottles, bamboo kitchenware, and organic cotton tote bags for a sustainable lifestyle.">
    <meta name="keywords" content="eco-friendly products, sustainable goods, reusable water bottles, bamboo kitchenware, organic cotton tote bags">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://js.stripe.com/v3/"></script>
</head>
<body class="bg-gray-100 font-sans">
    <!-- Header -->
    <header class="bg-green-600 text-white p-4">
        <div class="container mx-auto flex justify-between items-center">
            <h1 class="text-2xl font-bold">GreenLiving</h1>
            <nav>
                <a href="#products" class="mx-2 hover:underline">Products</a>
                <a href="#about" class="mx-2 hover:underline">About</a>
                <a href="#cart" class="mx-2 hover:underline">Cart (<span id="cart-count">0</span>)</a>
            </nav>
        </div>
    </header>

    <!-- Hero Section -->
    <section class="bg-green-100 py-12">
        <div class="container mx-auto text-center">
            <h2 class="text-4xl font-bold mb-4">Sustainable Living, Made Simple</h2>
            <p class="text-lg mb-6">Discover eco-friendly products designed to reduce your environmental impact.</p>
            <a href="#products" class="bg-green-600 text-white px-6 py-3 rounded hover:bg-green-700">Shop Now</a>
        </div>
    </section>

    <!-- Products Section -->
    <section id="products" class="py-12">
        <div class="container mx-auto">
            <h3 class="text-3xl font-bold text-center mb-8">Our Eco-Friendly Products</h3>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <!-- Reusable Water Bottle -->
                <div class="bg-white p-6 rounded shadow">
                    <img src="https://via.placeholder.com/300" alt="Reusable Water Bottle" class="w-full h-48 object-cover mb-4">
                    <h4 class="text-xl font-semibold">Stainless Steel Water Bottle</h4>
                    <p class="text-gray-600 mb-4">$24.99</p>
                    <button class="add-to-cart bg-green-600 text-white px-4 py-2 rounded hover:bg-green-700" data-id="1" data-name="Stainless Steel Water Bottle" data-price="24.99" data-stock="50">Add to Cart</button>
                </div>
                <!-- Bamboo Kitchenware -->
                <div class="bg-white p-6 rounded shadow">
                    <img src="https://via.placeholder.com/300" alt="Bamboo Kitchenware" class="w-full h-48 object-cover mb-4">
                    <h4 class="text-xl font-semibold">Bamboo Utensil Set</h4>
                    <p class="text-gray-600 mb-4">$19.99</p>
                    <button class="add-to-cart bg-green-600 text-white px-4 py-2 rounded hover:bg-green-700" data-id="2" data-name="Bamboo Utensil Set" data-price="19.99" data-stock="30">Add to Cart</button>
                </div>
                <!-- Organic Cotton Tote Bag -->
                <div class="bg-white p-6 rounded shadow">
                    <img src="https://via.placeholder.com/300" alt="Organic Cotton Tote Bag" class="w-full h-48 object-cover mb-4">
                    <h4 class="text-xl font-semibold">Organic Cotton Tote Bag</h4>
                    <p class="text-gray-600 mb-4">$14.99</p>
                    <button class="add-to-cart bg-green-600 text-white px-4 py-2 rounded hover:bg-green-700" data-id="3" data-name="Organic Cotton Tote Bag" data-price="14.99" data-stock="100">Add to Cart</button>
                </div>
            </div>
        </div>
    </section>

    <!-- Cart Section -->
    <section id="cart" class="py-12 bg-gray-50">
        <div class="container mx-auto">
            <h3 class="text-3xl font-bold text-center mb-8">Your Cart</h3>
            <div id="cart-items" class="space-y-4"></div>
            <div class="text-right mt-6">
                <p class="text-xl font-semibold">Total: $<span id="cart-total">0.00</span></p>
                <button id="checkout-btn" class="bg-green-600 text-white px-6 py-3 rounded hover:bg-green-700 mt-4">Proceed to Checkout</button>
            </div>
        </div>
    </section>

    <!-- About Section -->
    <section id="about" class="py-12">
        <div class="container mx-auto text-center">
            <h3 class="text-3xl font-bold mb-8">About GreenLiving</h3>
            <p class="text-lg max-w-2xl mx-auto">GreenLiving is committed to promoting sustainable living through high-quality, eco-friendly products. Our curated selection supports ethical sourcing and environmental responsibility.</p>
        </div>
    </section>

    <!-- Footer -->
    <footer class="bg-green-600 text-white p-4">
        <div class="container mx-auto text-center">
            <p>© 2025 GreenLiving. All rights reserved.</p>
        </div>
    </footer>

    <script>
        // Initialize Stripe
        const stripe = Stripe('pk_test_...');pk_live_51Rn5OUKHOkbIJcHXhvaP6qkzjMJlxgaGULjNdYCkMtX982JUFRh9PNdj4AuZWTwPoqm1VV7jflvahYohOTOlNAam00oCxrWAcY
        // Cart functionality
        let cart = [];

        // Update cart display
        function updateCart() {
            const cartItems = document.getElementById('cart-items');
            const cartCount = document.getElementById('cart-count');
            const cartTotal = document.getElementById('cart-total');
            cartItems.innerHTML = '';
            let total = 0;

            cart.forEach(item => {
                total += item.price * item.quantity;
                const itemElement = document.createElement('div');
                itemElement.className = 'flex justify-between items-center p-4 bg-white rounded shadow';
                itemElement.innerHTML = `
                    <span>${item.name} (x${item.quantity})</span>
                    <span>$${item.price * item.quantity}</span>
                `;
                cartItems.appendChild(itemElement);
            });

            cartCount.textContent = cart.reduce((sum, item) => sum + item.quantity, 0);
            cartTotal.textContent = total.toFixed(2);
        }

        // Add to cart with stock check
        document.querySelectorAll('.add-to-cart').forEach(button => {
            button.addEventListener('click', () => {
                const id = button.dataset.id;
                const name = button.dataset.name;
                const price = parseFloat(button.dataset.price);
                const stock = parseInt(button.dataset.stock);
                const existingItem = cart.find(item => item.id === id);

                if (stock <= 0) {
                    alert('This product is out of stock!');
                    return;
                }

                if (existingItem) {
                    if (existingItem.quantity >= stock) {
                        alert('Cannot add more; stock limit reached!');
                        return;
                    }
                    existingItem.quantity += 1;
                } else {
                    cart.push({ id, name, price, quantity: 1, stock });
                }
                updateCart();
            });
        });

        // Checkout with Stripe
        document.getElementById('checkout-btn').addEventListener('click', async () => {
            if (cart.length === 0) {
                alert('Your cart is empty!');
                return;
            }

            const response = await fetch('/api/create-checkout-session', {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ items: cart })
            });
            const session = await response.json();
            stripe.redirectToCheckout({ sessionId: session.id });
        });
    </script>
</body>
</html>
