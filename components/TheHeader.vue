<template>
    <header v-if="!hideFooterOnPages.includes($route.name)">
        <div class="d-flex align-items-center justify-content-between w-100 pc">
            <div class="first">
                <NuxtLink to="/catalog">Каталог</NuxtLink>
                <NuxtLink to="/catalog?search=true">Поиск</NuxtLink>
            </div>

            <NuxtLink to="/" class="logo">
                <img src="@/assets/img/headerlogo.svg" alt="">
            </NuxtLink>

            <div class="first">
                <div class="cartrap" v-if="accountType == 'buyer'">
                    <img src="@/assets/img/cart.svg" style="cursor: pointer;" alt="" @click="openHeader">
                    <span>{{ cartLength }}</span>
                </div>
                <div class="first" v-if="!isAuth">
                    <NuxtLink to="/login">Войти</NuxtLink>
                    <NuxtLink to="/register">регистрация</NuxtLink>
                </div>
                <div v-else class="first">
                    <NuxtLink style="cursor:pointer" data-toggle="modal"
                        :data-target="accountType === 'seller' ? '#outModal' : '#inModal'">{{ userBalance }} ₸
                    </NuxtLink>
                    <NuxtLink :to="this.accountUrl">личный кабинет</NuxtLink>
                </div>
            </div>

        </div>
        <div class="mob">
            <div class="d-flex justify-content-between">
                <NuxtLink to="/">
                    <img src="@/assets/img/headerlogo.svg" alt="">
                </NuxtLink>
                <div class="d-flex burg">
                    <div class="cartrap mt-3" v-if="accountType == 'buyer'">
                        <img src="@/assets/img/cart.svg" style="cursor: pointer;" alt="" @click="openHeader">
                        <span>{{ cartLength }}</span>
                    </div>

                    <div>
                        <input id="menu__toggle" type="checkbox" />
                        <label class="menu__btn" for="menu__toggle">
                            <span></span>
                        </label>


                        <ul class="menu__box">
                            <div class="mobmenu">
                                <div class="links">
                                    <div v-if="!isAuth">
                                        <NuxtLink to="/login" @click="closepls">Войти</NuxtLink>
                                        <NuxtLink to="/register" @click="closepls">регистрация</NuxtLink>
                                    </div>
                                    <div v-else>
                                        <NuxtLink style="cursor: pointer;" alt="" data-toggle="modal"
                                            :data-target="accountType === 'seller' ? '#outModal' : '#inModal'">{{
                                                userBalance }} ₸</NuxtLink>
                                        <NuxtLink :to="this.accountUrl" @click="closepls">Личный кабинет</NuxtLink>
                                    </div>
                                    <NuxtLink to="/catalog" @click="closepls">Каталог</NuxtLink>
                                    <NuxtLink to="/catalog?search=true" @click="closepls">поиск</NuxtLink>

                                    <div class="text-center">
                                        <img src="@/assets/img/headerlogo.svg" alt="">
                                    </div>
                                </div>
                            </div>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </header>
    <TheOutModal></TheOutModal>
    <TheInModal></TheInModal>


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
            <button @click="buyProduct()" ref='cartBtn'>перейти к оформлению</button>
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
            cartOpen: false,
            hideFooterOnPages: ['login', 'register'],
            closed: false,
            pathUrl: 'https://visuality.kz',
            userBalance: null,
            accountType: '',
            cartLength: localStorage.getItem('cartLength')

        }
    },
    methods: {
        closepls() {
            document.querySelector('#menu__toggle').checked = false
        },
        openHeader() {
            let sc = $(".cart")[0];
            sc.style.right = 0;
            this.getCart()
        },
        getBuyer() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/buyer/buyer-lk`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    this.userBalance = response.data.balance

                })
                .catch(error => console.log(error));
        },
        closeHeader() {
            let sc = $(".cart")[0];
            sc.style.right = '-2000px';
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
            // const token = this.getAuthorizationCookie()
            // const path = `${this.pathUrl}/api/buyer/placed-basket`
            // axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            // this.$refs.cartBtn.innerHTML = 'Оформляем'
            // axios
            //     .get(path)
            //     .then(response => {
            //         console.log(response)
            //         this.getCart()
            //         if (response.status == 204) {
            //             this.$refs.cartBtn.innerHTML = 'Недостаточно средств'
            //         }
            //         if (response.status == 201) {
            //             this.getBuyer()
            //             this.$refs.cartBtn.innerHTML = 'Переходим к оплате'
            //             // window.location.href = '/buyer-account'
            //         }
            //     })
            //     .catch(error => {
            //         console.error(error)
            //     })

            window.location.href = '/complete-buy'
        },
        getSeller() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/seller/seller-lk`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    this.userBalance = response.data.balance

                })
                .catch(error => console.log(error));
        }
    },
    mounted() {
        const accType = localStorage.getItem('accountType')
        if (accType == 'buyer-account') {
            this.getBuyer()
            this.accountType = 'buyer'
            setInterval(() => {
                this.cartLength = localStorage.getItem('cartLength')
            }, 1);
        }
        else if (accType == 'seller-account') {
            this.getSeller()
            this.accountType = 'seller'
        }
        else {
            console.log('not authorized')
        }

    }
}
</script>
<style scoped>
.cartrap {
    position: relative;
}

.cartrap span {
    background: #fff;
    color: #000;
    font-family: var(--int);
    font-size: 12px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    padding: 1px 5px;
    border-radius: 100px;
    position: absolute;
    right: -15%;
}

.cart__footer button {
    border-radius: 20px;
    border: 2px solid #fff;
    background: transparent;
    padding: 13px 26px;

    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;

    transition: all .3s ease;
}

.cart__footer button:hover {
    background: #fff;
    color: #000;
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
    font-size: 16px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    font-family: var(--int);
    color: #fff;
}

.name span {
    font-size: 13.229px;
    font-style: normal;
    font-weight: 500;
    line-height: 130%;
    font-family: var(--hel);
    color: #fff;
    max-width: 164px;
}

.name {
    margin-left: 20px;
}

.product__img {
    border-radius: 15px;
    box-shadow: 0px 0px 20px 0px rgba(47, 59, 163, 0.20);
}

.product__img img {
    border-radius: 30px;
    object-fit: cover;
    width: 9.167vw;
    height: 83px;
}

.cart__body {
    margin-top: 76px;
}

.cart h3 {
    font-size: 36px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
    margin: 0;
}

.cart {
    background: #161616;
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

.mobmenu .links img {
    margin-top: 150px;
}

.mobmenu .links a {
    display: block;
    margin-bottom: 40px;
    font-size: 24px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
}


.menu__box a:hover {
    background: transparent;
}

#menu__toggle {
    opacity: 0;
}

.menu__btn {
    display: flex;
    /* используем flex для центрирования содержимого */
    align-items: center;
    /* центрируем содержимое кнопки */
    width: 40px;
    height: 20px;
    cursor: pointer;
    z-index: 100000000;
    color: #fff;
    position: relative;
    transform: rotate(180deg);
}

.menu__btn>span {
    display: block;
    position: absolute;
    width: 100%;
    height: 2px;
    background-color: #fff;
}

.menu__btn>span::before,
.menu__btn>span::after {
    display: block;
    position: absolute;
    width: 120%;
    height: 2px;
    background-color: #fff;
}

.menu__btn>span::before {
    content: '';
    top: -8px;
}

.menu__btn>span::after {
    content: '';
    top: 8px;
}

.menu__box {
    display: block;
    position: fixed;
    visibility: hidden;
    top: -100%;
    left: 0;
    width: 100%;
    height: 100%;
    margin: 0;
    padding: 20px 0;
    list-style: none;
    text-align: left;
    background: #242424;
    box-shadow: 1px 0px 6px rgba(0, 0, 0, .2);
}

.traparmy {
    top: -100% !important;
}

/* элементы меню */
.menu__item {
    display: block;
    padding: 12px 24px;
    color: #333;
    font-family: 'Roboto', sans-serif;
    font-size: 20px;
    font-weight: 600;
    text-decoration: none;
    z-index: 100000;
}

.menu__item:hover {
    background-color: #CFD8DC;
}

#menu__toggle:checked~.menu__btn>span {
    transform: rotate(45deg);
    background-color: #fff;
    border-radius: 10px;

}

#menu__toggle:checked~.menu__btn>span::before {
    top: 0;
    transform: rotate(0);
    background-color: #fff;
    border-radius: 10px;

}

#menu__toggle:checked~.menu__btn>span::after {
    top: 0;
    transform: rotate(90deg);
    background-color: #fff;
    border-radius: 10px;
}

#menu__toggle:checked~.menu__box {
    visibility: visible;
    top: 0;
}

.menu__btn>span,
.menu__btn>span::before,
.menu__btn>span::after {
    transition-duration: .25s;
}

.menu__box {
    transition-duration: .25s;
    z-index: 100000;
}

.menu__item {
    transition-duration: .25s;
}

.mobmenu {
    padding: 75px 40px 0;
}

.burg {
    gap: 0 30px;
}

.logo {
    margin-left: 10%;
}

header a {
    text-decoration: none !important;
    font-size: 16px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff !important;
}

.first {
    display: flex;
    align-items: center;
    gap: 0 72px;
}

header {
    padding: 25px 100px;
    width: 100%;
    position: fixed;
    background: #242424;
    z-index: 200;
}

.mob {
    display: none;
}

@media (max-width: 1670px) {
    header {
        padding: 25px 50px;
    }
}

@media (max-width: 1025px) {
    header {
        padding: 25px 30px;
    }

    .first {
        gap: 0 30px;
    }

    header a {
        font-size: 18px;
    }
}

@media (max-width: 1024px) {
    .pc {
        display: none !important;
    }

    .mob {
        display: block;
    }
}

@media (max-width: 380px) {
    .mobmenu {
        padding: 75px 20px 0;
    }
}
</style>