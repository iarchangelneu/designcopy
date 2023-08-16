<template>
    <div v-if="product.length <= 0"></div>
    <div class="product d-flex align-items-start" v-else>

        <div class="product__preview">
            <img :src="product.main_image" class="img-fluid" alt="">
        </div>

        <div class="author">
            <div class="text-center">
                <h1>{{ product.name }}</h1>
            </div>
            <div class="about__author">
                <div class="d-flex justify-content-between align-items-center name">
                    <p class="mb-0">Автор:</p>
                    <span>{{ seller.first_name }}</span>
                </div>
                <div class="d-flex justify-content-between align-items-center category">
                    <p class="mb-0">Категория:</p>
                    <span>{{ category }}</span>
                </div>

                <p class="mb-0">Описание:</p>

                <small>{{ product.description_1 }}</small>

            </div>

            <div class="author__price d-flex justify-content-between align-items-end">
                <small>
                    {{ product.price.toLocaleString() }} ₸
                    <img src="@/assets/img/saled.svg" alt="">
                </small>
                <span>{{ (Math.floor(product.price - ((product.price * product.discount) / 100))).toLocaleString() }}
                    ₸</span>
                <div class="gradik">
                    <p class="mb-0">-{{ product.discount }}%</p>
                </div>
            </div>
            <small class="mt-3 ml-2 d-block" ref="apiMessage">{{ apiMessage }}</small>
            <button class="w-100" ref="cartBtn" @click="addToCart()">В КОРЗИНУ</button>

        </div>
    </div>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    data() {
        return {
            productId: this.$route.params.id,
            product: [],
            seller: [],
            pathUrl: 'https://themes.kz',
            category: '',
        }
    },
    methods: {
        getProduct() {
            const path = `${this.pathUrl}/api/products/detail-product/${this.productId}`
            axios
                .get(path)
                .then(response => {
                    this.product = response.data
                    this.seller = response.data.seller.user
                    this.category = response.data.category.category_name
                })
                .catch(error => {
                    console.error(error)
                })
        },
        addToCart() {
            const path = `${this.pathUrl}/api/buyer/add-product-basket`
            const csrf = this.getCSRFToken()
            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            axios
                .post(path, {
                    products: this.product.id,
                    amount: 1,
                })
                .then(response => {
                    if (response.status == 201) {
                        this.apiMessage = 'Товар успешно добавлен в корзину!'
                        this.$refs.apiMessage.classList.add('green')
                        this.$refs.cartBtn.disabled = true
                        this.$refs.cartBtn.classList.add('disabled')
                        this.getCart()
                    }
                    else {
                        this.apiMessage = 'Произошла ошибка, попробуйте еще раз'
                        this.$refs.apiMessage.classList.add('red')
                        this.$refs.cartBtn.disabled = false

                    }
                })
                .catch(error => {
                    console.error(error)
                })
        }
    },
    mounted() {
        this.getProduct()
    }
}
</script >
<script setup>
const route = useRoute();
const itemId = route.params.id
const productDetails = await fetch(`https://themes.kz/api/products/detail-product/${itemId}`).then(res => res.json()).then(data => data);
const title = `${productDetails.name}`
useSeoMeta({
    title: () => title + ' | Design Market',
    ogTitle: () => title + ' | Design Market',
    description: () => title + ' | Design Market',
    ogDescription: () => title + ' | Design Market',
})

</script>
<style scoped>
.author button {
    margin-top: 20px;
    background: transparent;
    border-radius: 20px;
    border: 2px solid #fff;
    padding: 17px 0;

    font-size: 24px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    font-family: var(--int);
    color: #fff;
    transition: all .3s ease;
}

.red {
    color: red;
    font-family: var(--int);
}

.disabled {
    opacity: 0.5;
    background: #000 !important;
    color: #fff !important;
}

.green {
    color: green;
    font-family: var(--int);
}

.author button:hover {
    color: #000;
    background: #fff;
}

.gradik {
    background-image: linear-gradient(90deg, #8D70DA 0%, #F66A4F 100%);
    color: transparent;
    background-clip: text;
}

.author__price p {
    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    padding: 0 23px;
    border-image: url('@/assets/img/border.svg');
    border-image-slice: 9;
    border-image-width: 8px;
    border-style: solid;

}

.author__price span {
    font-size: 48px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
}

.author__price small img {
    position: absolute;
    left: 2%;
    bottom: 10%;
    /* transform: rotate(5deg); */
}

.author__price small {
    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
    position: relative;
}

.author__price {
    margin-top: 20px;
}

.about__author small {
    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: 130%;
    font-family: var(--hel);
    color: #fff;
    margin-top: 20px;
    display: block;
}

.category {
    margin-bottom: 25px;
    display: flex !important;
}

.name {
    margin-bottom: 30px;
}

.about__author span {
    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    font-family: var(--int);
    color: #fff;

    border-radius: 20px;
    border: 2px solid #fff;
    padding: 5px 15px;
}

.about__author p {
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    font-family: var(--hel);
    color: #fff;
}

.about__author {
    margin-top: 40px;
}

.author h1 {
    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
}

.author {
    border-radius: 20px;
    background: #3D3D3D;
    box-shadow: 0px 0px 15px 0px rgba(47, 59, 163, 0.20);
    padding: 40px;
    width: 31.719vw;
    position: sticky;
    top: 140px;
    bottom: 72px;
}

.product__preview {
    width: 55.781vw;
    border-radius: 50px;
    box-shadow: 0px 0px 20px 0px rgba(47, 59, 163, 0.20);
}

.product__preview img {
    width: 100%;
    border-radius: 50px;
}

.product {
    padding: 140px 100px 72px;
    gap: 0 40px;
}

@media (max-width: 1600px) {
    .author__price span {
        font-size: 40px;
    }

    .author h1 {
        font-size: 20px;
    }
}

@media (max-width: 1500px) {
    .product {
        padding: 140px 50px 72px;
    }

    .about__author p {
        font-size: 20px;
    }

    .about__author span {
        font-size: 20px;
    }

    .about__author small {
        font-size: 16px;
    }

    .author__price span {
        font-size: 30px;
    }

    .author__price small {
        font-size: 20px;
    }

    .author__price p {
        font-size: 16px;
        padding: 0 13px;
        border-image-slice: 10;
    }

    .author button {
        font-size: 20px;
    }

}

@media (max-width: 1250px) {
    .author {
        padding: 20px;
    }
}

@media (max-width: 1024px) {
    .product__preview {
        width: 100%;
    }

    .product {
        padding: 140px 20px 50px;
        flex-direction: column-reverse;
        gap: 30px;
    }

    .author {
        position: relative;
        top: 0;
        bottom: 0;
        width: 100%;
        max-width: 100%;
    }

    .author h1 {
        font-size: 16px;
    }

    .about__author p {
        font-size: 16px;
    }

    .name,
    .category {
        flex-direction: column;
        justify-content: left !important;
        align-items: flex-start !important;
        gap: 15px;
        margin-bottom: 20px;
    }

    .about__author {
        margin-top: 20px;
    }

    .about__author span {
        font-size: 16px;
    }

    .author button {
        padding: 10px 0;
        border: 2px solid #fff;
    }

    .author__price span {
        font-size: 24px;
    }

}
</style>