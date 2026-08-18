<script>
  let products = $state([
    {
      id: 1,
      name: 'Chicken',
      price: 350,
      quantity: 0
    },
    {
      id: 2,
      name: 'Chips',
      price: 150,
      quantity: 0
    },
    {
      id: 3,
      name: 'Soda',
      price: 70,
      quantity: 0
    }
  ]);

  let customerName = $state('');
  let location = $state('');

  const businessNumber = '254712345678';

  let total = $derived(
    products.reduce(
      (sum, product) =>
        sum + product.price * product.quantity,
      0
    )
  );

  // @ts-ignore
  function increase(productId) {
    const product = products.find(
      (product) => product.id === productId
    );

    if (product) {
      product.quantity += 1;
    }
  }

  // @ts-ignore
  function decrease(productId) {
    const product = products.find(
      (product) => product.id === productId
    );

    if (product && product.quantity > 0) {
      product.quantity -= 1;
    }
  }

  function orderOnWhatsApp() {
    const orderedProducts = products.filter(
      (product) => product.quantity > 0
    );

    if (orderedProducts.length === 0) {
      alert('Please choose at least one product.');
      return;
    }

    if (!customerName.trim()) {
      alert('Please enter your name.');
      return;
    }

    if (!location.trim()) {
      alert('Please enter your delivery location.');
      return;
    }

    let message =
      'Hello, I would like to place an order.\n\n';

    message += `Name: ${customerName}\n`;
    message += `Location: ${location}\n\n`;

    message += 'Order:\n';

    for (const product of orderedProducts) {
      const productTotal =
        product.price * product.quantity;

      message += `${product.name} x ${product.quantity} - KSh ${productTotal}\n`;
    }

    message += `\nTotal: KSh ${total}`;

    const whatsappUrl =
      `https://wa.me/${businessNumber}?text=${encodeURIComponent(message)}`;

    window.open(whatsappUrl, '_blank');
  }
</script>

<div class="container">
  <header>
    <h1>Mama Jane's Kitchen</h1>
    <p>Choose what you'd like to order.</p>
  </header>

  <section class="products">
    {#each products as product}
      <div class="product">
        <div>
          <h2>{product.name}</h2>
          <p>KSh {product.price}</p>
        </div>

        <div class="quantity">
          <button
            onclick={() => decrease(product.id)}
          >
            -
          </button>

          <span>{product.quantity}</span>

          <button
            onclick={() => increase(product.id)}
          >
            +
          </button>
        </div>
      </div>
    {/each}
  </section>

  <section class="order">
    <h2>Total: KSh {total}</h2>

    <label>
      Your name

      <input
        type="text"
        bind:value={customerName}
        placeholder="Enter your name"
      />
    </label>

    <label>
      Delivery location

      <input
        type="text"
        bind:value={location}
        placeholder="Where should we deliver?"
      />
    </label>

    <button
      class="whatsapp-button"
      onclick={orderOnWhatsApp}
    >
      Order on WhatsApp
    </button>
  </section>
</div>

<style>
  :global(body) {
    margin: 0;
    font-family: Arial, sans-serif;
    background: #f4f4f4;
    color: #222;
  }

  .container {
    max-width: 600px;
    margin: 0 auto;
    padding: 24px 16px;
  }

  header {
    text-align: center;
    margin-bottom: 24px;
  }

  header h1 {
    margin-bottom: 8px;
  }

  header p {
    color: #666;
  }

  .product {
    background: white;
    padding: 16px;
    margin-bottom: 12px;
    border-radius: 12px;

    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .product h2 {
    margin: 0 0 6px;
    font-size: 18px;
  }

  .product p {
    margin: 0;
    color: #666;
  }

  .quantity {
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .quantity button {
    width: 40px;
    height: 40px;
    border: none;
    border-radius: 8px;
    background: #eee;
    font-size: 20px;
    cursor: pointer;
  }

  .quantity span {
    min-width: 20px;
    text-align: center;
    font-weight: bold;
  }

  .order {
    background: white;
    padding: 20px;
    margin-top: 20px;
    border-radius: 12px;
  }

  .order h2 {
    margin-top: 0;
  }

  label {
    display: block;
    margin-top: 16px;
    font-weight: bold;
  }

  input {
    display: block;
    width: 100%;
    box-sizing: border-box;
    margin-top: 8px;
    padding: 14px;
    border: 1px solid #ddd;
    border-radius: 8px;
    font-size: 16px;
  }

  .whatsapp-button {
    width: 100%;
    margin-top: 20px;
    padding: 16px;
    border: none;
    border-radius: 10px;
    background: #25d366;
    color: white;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
  }

  .whatsapp-button:hover {
    background: #20bd5a;
  }
</style>