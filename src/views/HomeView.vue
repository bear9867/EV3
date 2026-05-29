<template>
    <div class="container">
    <div class="shop-layout">
    <!--Lista-->
    <div class="products-section">
        <h2>Productos disponibles</h2>

        <ul v-for="product in products" :key="product.id">
            <li class="product-item">
                <img :src="`/public/Imagenes/product-${product.id}-${product.name.replace(' ', '-')}.webp`" alt="Imagen del producto" />

                <div class="product-info">
                    <span>
                        {{ product.name }} - Precio $: {{FormatoPeso(product.price)}} - Stock: {{ product.stock }}
                    </span>

                    <button @click="addToCart(product)" class="add-to-cart-btn">
                        <img src="/public/Imagenes/iCarrito.png" class="button-icon" alt="Carrito">
                        Agregar al carrito
                    </button>
                </div>
            </li>
        </ul>
    </div>

    <!--Carrito-->
    <div class="cart-section">
        <h2>Productos en el carrito</h2>

        <ul v-for="item in cart" :key="item.product.id">
            <li class="cart-item">
                <img :src="`/public/Imagenes/product-${item.product.id}-${item.product.name.replace(' ', '-')}.webp`" alt="Imagen del producto" />
                
                <div class="cart-info">
                    <span>
                        {{ item.product.name }}-Cantidad: {{ item.quantity }}
                    </span>
                    <button @click="removeFromCart(item)" class="remove-from-cart-btn">
                        <img src="/public/Imagenes/iBasura.png" class="button-icon" alt="Basurero">
                        Remover del carrito
                    </button>
                </div>
            </li>
        </ul>
        
        <br>

        <button v-if="cart.length > 0" @click="removeAllCart" class="remove-from-cart-btn">
            <img src="/public/Imagenes/iTodaBasura.png" class="button-iconBasuraTotal" alt="CamionBasurero">
            Vaciar carrito
        </button>
        <p v-else class="empty-cart-message">
            No hay productos en el carrito
        </p>

        <p class="total-label">
            Total a pagar: {{FormatoPeso(total)}}
        </p>
        </div>
    </div>
</div>
</template>

<script>
    export default {
        data() {
            return {
                products: [
                    {id: 1, name: 'Audifonos', price: 30000, stock: 3 },
                    {id: 2, name: 'Mouse', price: 20000, stock: 5},
                    {id: 3, name: 'Teclado', price: 15000, stock: 10},
                    {id: 4, name: 'Gabinete', price: 35000, stock: 4},
                    {id: 5, name: 'Pantalla', price: 175000, stock: 3},
                    {id: 6, name: 'Silla', price: 150000, stock: 2}
                ],
                cart: []
            };
        },
        computed: {
            total() {
                return this.cart.reduce((total, item) => total + (item.product.price * item.quantity), 0);
            }
            
        },
        methods: {
        FormatoPeso(precio) {
            return new Intl.NumberFormat('es-CL', {
                style: 'currency',
                currency: 'CLP'
            }).format(precio);
        },
            addToCart(product) {
                let cartItem = this.cart.find(item => item.product.id === product.id);
                if (cartItem) {
                    if (cartItem.quantity < product.stock) {
                        cartItem.quantity++;
                    } else {
                        alert("¡No hay mas unidades disponibles en stock");
                    }
                } else {
                    this.cart.push({product: product, quantity: 1});
                }
            },
            removeFromCart(item) {
                let index = this.cart.indexOf(item);
                if (index !== -1) {
                    if (item.quantity > 1) {
                        item.quantity--;
                    } else {
                        this.cart.splice(index, 1);
                    }
                }
            },
            removeAllCart() {
                this.cart = [];
            }
        }
    };
</script>

<style scoped>

    .container {
        width: 80%; 
         max-width: 1300px;
        margin: 0 auto;
        text-align: left;
    }
    .shop-layout {
        display: flex;
        justify-content: space-between;
        align-items: flex-start;
        gap: 40px;
    margin-top: 30px;
    }

    .products-section,
    .cart-section {
        width: 48%;
    }

    .button-icon {
        width: 14px;
        height: 14px;
    }
    .button-iconBasuraTotal {
        width: 30px;
        height: 30px;
    }

    ul {
        list-style: none;
        padding: 0;
    }

    .product-item,
    .cart-item {
        display: flex;
        align-items: center;
        margin-bottom: 20px;
    }

    img {
        width: 50px;
        height: 50px;
        margin-right: 10px;
    }

    .product-info,
    .cart-info {
        flex: 1;
    }

    .add-to-cart-btn,
    .remove-from-cart-btn {
        background-color: #291382;
        color: white;
        padding: 10px 20px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        display: flex;
        align-items: center;
    }

    .add-to-cart-btn i,
    .remove-from-cart-btn i {
        margin-right: 10px;
    }

    .total-label {
        font-weight: bold;
        font-size: 20px;
        margin-top: 20px;
    }
    @media (max-width: 768px) {
    .shop-layout {
        flex-direction: column;
    }

    .products-section,
    .cart-section {
        width: 100%;
    }
}
</style>
