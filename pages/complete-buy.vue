<template>
    <div class="complete">
        <h1>покупка</h1>

        <div class="d-flex complete__body">
            <div class="left__side w-100">
                <div class="cart__item" v-for="item in cart" :key="item.id">
                    <div class="cart__img">
                        <img :src="pathUrl + item.products.main_image" alt="">
                    </div>
                    <div class="name">
                        <span>{{ item.products.name }}</span>
                        <small>{{ (Math.floor(item.products.price - ((item.products.price * item.products.discount) /
                                                    100))).toLocaleString() }} ₸</small>
                    </div>
                </div>
            </div>

            <div class="right__side">
                <div class="info__body">
                    <div class="text-center">
                        <h2>Подтверждение покупки</h2>
                    </div>

                    <div class="tilt">
                        <div class="d-flex justify-content-between align-items-center gage">
                            <span>Количество товаров:</span>
                            <small>{{ cartLength }}</small>
                        </div>
                        <div class="d-flex justify-content-between align-items-center gage">
                            <span>Общая сумма:</span>
                            <small>{{ formatPrice(calculateTotal()) }} ₸</small>
                        </div>
                        <p>Сумма будет списана с Вашего счета. Товары отобразятся во вкладке “Мои покупки” в Вашем Личном
                            кабинете. </p>

                        <button class="w-100" ref="buyBtn" @click="buyProduct()">
                            подтвердить покупку
                        </button>
                    </div>
                </div>
            </div>
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
            cart: [],
            pathUrl: 'https://visuality.kz',
            cartLength: localStorage.getItem('cartLength'),
        }
    },
    methods: {
        buyProduct() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/buyer/placed-basket`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            this.$refs.buyBtn.innerHTML = 'Оформляем'
            axios
                .get(path)
                .then(response => {
                    console.log(response)
                    this.getCart()
                    if (response.status == 204) {
                        this.$refs.buyBtn.innerHTML = 'Недостаточно средств'
                    }
                    if (response.status == 201) {
                        // this.getBuyer()
                        this.$refs.buyBtn.innerHTML = 'Оплата прошла успешно'

                        setTimeout(() => {
                            window.location.href = '/catalog'
                        }, 3);
                        // window.location.href = '/buyer-account'
                    }
                })
                .catch(error => {
                    console.error(error)
                })

            // window.location.href = '/complete-buy'
        },
        getCart() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/buyer/all-product-basket`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(path)
                .then(response => {
                    this.cart = response.data
                    localStorage.setItem('cartLength', this.cart.length)
                })
                .catch(error => {
                    console.error(error)
                })
        },
        calculateTotal() {
            let total = 0;

            this.cart.forEach(item => {
                const { price, discount } = item.products;
                const discountedPrice = price * (1 - discount / 100); // Преобразуем скидку в коэффициент
                total += discountedPrice * item.amount;
            });

            return total;
        },
        formatPrice(price) {
            return price.toLocaleString('ru-RU');
        }
    },
    mounted() {
        this.getCart()
    }
}
</script >
<script setup>

useSeoMeta({
    title: 'Подтвердите покупку | Design Market',
    ogTitle: 'Подтвердите покупку | Design Market',
    description: 'Подтвердите покупку | Design Market',
    ogDescription: 'Подтвердите покупку | Design Market',
})
</script>
<style scoped>
.tilt {
    margin-top: 32px;
}

.gage {
    margin-bottom: 39px;
}

.tilt span {
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    font-family: var(--hel);
    color: #fff;
}

.tilt button {
    padding: 17px 0;
    background: transparent;
    border-radius: 20px;
    border: 2px solid #fff;

    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
    transition: all .3s ease;
}

.tilt button:hover {
    color: #000;
    background: #fff;
}

.tilt p {
    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    font-family: var(--hel);
    color: #fff;
}

.tilt small {
    font-size: 32px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
}

.info__body h2 {
    font-size: 20px;
    font-style: normal;
    font-weight: 500;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
}

.info__body {
    border-radius: 20px;
    background: #3D3D3D;
    box-shadow: 0px 0px 15px 0px rgba(47, 59, 163, 0.20);
    padding: 40px;
    width: 31.719vw;
}

.name {
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    /* gap: 0 137px; */
    width: 100%;
}

.name span {
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    text-transform: uppercase;
    color: #fff;
    font-family: var(--hel);
}

.name small {
    font-size: 32px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    text-transform: uppercase;
    color: #fff;
    font-family: var(--int);
    white-space: nowrap;
}

.cart__img {
    border-radius: 21.909px;
    box-shadow: 0px 0px 14.605955123901367px 0px rgba(47, 59, 163, 0.20);
    width: 17.083vw;
    height: 8.021vw;
}

.cart__img img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: top;
    border-radius: 21.909px;
}

.cart__item {
    display: flex;
    gap: 36px;
    margin-bottom: 32px;
    width: 100%;
}

.complete {
    padding: 140px 100px 72px;
}

.complete h1 {
    font-size: 64px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
}

.complete__body {
    gap: 65px;
    margin-top: 54px;
}

@media (max-width: 1440px) {
    .complete {
        padding: 140px 50px 72px;
    }

    .name span {
        font-size: 18px;
    }

    .name small,
    .tilt small {
        font-size: 24px;
    }

    .tilt p {
        font-size: 16px;
    }

    .tilt button {
        font-size: 18px;
    }

    .info__body {
        padding: 20px;
    }

    .info__body h2,
    .tilt span {
        font-size: 18px;
    }
}

@media (max-width: 1024px) {
    .complete__body {
        flex-direction: column;
    }

    .cart__item {
        display: block;
        margin-bottom: 20px;
    }

    .cart__img {
        width: 100%;
        height: 165px;
    }

    .name {
        margin-top: 10px;
    }

    .name span {
        font-size: 16px;
        font-style: normal;
        font-weight: 500;
        line-height: 130%
    }

    .name small {
        font-size: 16px;
        font-style: normal;
        font-weight: 400;
        line-height: normal;
    }

    .info__body {
        width: 100%;
        padding: 30px;
    }

    .info__body h2 {
        font-size: 16px;
    }

    .info__body span,
    .info__body small {
        font-size: 20px;
    }

    .complete {
        padding: 100px 20px 20px;
    }

    .complete__body {
        gap: 20px;
    }

    .complete h1 {
        display: none;
    }
}
</style>