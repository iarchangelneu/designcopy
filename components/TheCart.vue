<template>
    <div class="cart">
        <div class="d-flex justify-content-between align-items-center">
            <h3>корзина</h3>
            <img src="@/assets/img/close.svg" alt="" style="cursor: pointer;" @click="closeHeader">
        </div>

        <div class="cart__body w-100">
            <div class="product d-flex" v-for="item in cart" :key="item.id">
                <div class="product__img">
                    <img :src="this.pathUrl + item.products.main_image" alt="">
                </div>
                <div class="name w-100">
                    <div class="d-flex justify-content-between align-items-start">
                        <span>{{ item.products.name }}</span>
                        <img src="@/assets/img/delete.svg" alt="" style="cursor: pointer;" @click="deleteFromCart(item.id)">
                    </div>
                    <p class="mb-0">{{ (Math.floor(item.products.price - ((item.products.price * item.products.discount) /
                                            100))).toLocaleString() }} ₸</p>
                </div>
            </div>

        </div>

        <div class="cart__footer text-center">
            <button @click="buyProduct()">перейти к оформлению</button>
        </div>
    </div>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios'
export default {
    mixins: [global],
    data() {
        return {
            closed: false,
            pathUrl: 'https://visuality.kz',
            cart: [],
        }
    },
    methods: {
        closeHeader() {
            let sc = $(".cart")[0];
            sc.style.right = '-2000px';
        },
        getCart() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/buyer/all-product-basket`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(path)
                .then(response => {
                    this.cart = response.data
                })
                .catch(error => {
                    console.error(error)
                })
        },
        deleteFromCart(id) {
            const token = this.getAuthorizationCookie()
            const csrf = this.getCSRFToken()
            const path = `${this.pathUrl}/api/buyer/delete-product-basket/${id}`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            axios
                .put(path)
                .then(response => {
                    console.log(response)
                    this.getCart()
                })
                .catch(error => {
                    console.error(error)
                })
        },
        buyProduct() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/buyer/placed-basket`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    console.log(response)
                    this.getCart()
                })
                .catch(error => {
                    console.error(error)
                })
        },
    },

    mounted() {
        const accType = localStorage.getItem('accountType')
        if (accType == 'buyer-account') {
            // this.getCart()
        }
    },

}
</script>
<style scoped>
.cart__footer button {
    border-radius: 50px;
    border: 2px solid #fff;
    background: transparent;
    padding: 13px 26px;

    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #000;

    transition: all .3s ease;
}

.cart__footer button:hover {
    background: #000;
    color: #fff;
}

.cart__footer {
    margin-top: 40px;
}

.cart__body {
    max-height: 600px;
    overflow-y: auto;
}

.product {
    margin-bottom: 30px;
}

.product:last-child {
    margin-bottom: 0;
}

.name p {
    display: flex;
    align-items: flex-end;
    justify-content: end;
    margin-top: 5%;
    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    font-family: var(--int);
}

.name span {
    font-size: 13.229px;
    font-style: normal;
    font-weight: 500;
    line-height: 130%;
    font-family: var(--int);
    color: #000;
    max-width: 164px;
}

.name {
    margin-left: 20px;
}

.product__img {
    border-radius: 15px;
    box-shadow: 0px 0px 20px 0px rgba(47, 59, 163, 0.20);
}

.cart__body {
    margin-top: 76px;
}

.cart h3 {
    font-size: 40px;
    font-style: normal;
    font-weight: 500;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #000;
    margin: 0;
}

.cart {
    background: #fff;
    position: fixed;
    right: -2000px;
    width: 33%;
    height: 100%;
    transition: all .3s ease;
    z-index: 300;
    padding: 60px;
}

.active {
    right: 0 !important;
}

@media (max-width: 1024px) {
    .cart {
        width: 100%;
        padding: 20px;
    }

    .cart h3 {
        font-size: 25px;
    }

    .name {
        margin-left: 0;
    }

    .product {
        flex-direction: column;
    }

    .product__img {
        border-radius: 2px;
    }

    .name p {
        justify-content: left;
        font-size: 16px;
        font-weight: 400;
        margin-top: 10px;
    }

    .product__img img {
        width: 100%;
        max-height: 165px;
    }

    .name {
        margin-top: 10px;
    }

    .name span {
        font-size: 16px;
        font-weight: 500;
        line-height: 130%;
        max-width: 263px;
    }

    .name img {
        width: 40px;
        height: 40px;
    }

    .cart__footer button {
        width: 100%;
        font-size: 16px;
        padding: 13px 0;
    }
}
</style>