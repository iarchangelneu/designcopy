<template>
    <div class="account">
        <h1>личный кабинет</h1>
        <div class="d-flex justify-content-between w-100 crack">
            <div v-if="tabs != 7"></div>
            <div class="backtochat" v-if="tabs == 7">
                <button @click="tabs = 3">
                    <img src="@/assets/img/backarrow.svg" alt="">
                    НАЗАД
                </button>
            </div>
            <div class="tabs d-flex align-items-center justify-content-end pc">

                <button :class="[{ 'active': tabs == 0 }, { 'active': tabs == 5 }, { 'active': tabs == 6 }]"
                    @click="tabs = 0">МОИ ТОВАРЫ</button>
                <button :class="[{ 'active': tabs == 1 }]" @click="tabs = 1">МОИ ПРОДАЖИ</button>
                <button :class="[{ 'active': tabs == 2 }]" @click="tabs = 2">ТРАНЗАКЦИИ</button>
                <button :class="[{ 'active': tabs == 3 }, { 'active': tabs == 7 }]" @click="tabs = 3">ОБРАТНАЯ
                    СВЯЗЬ</button>
                <button :class="[{ 'active': tabs == 4 }]" @click="tabs = 4">АККАУНТ</button>

            </div>
        </div>

        <div class="tabs mob">
            <div>
                <button :class="[{ 'active': tabs == 0 }, { 'active': tabs == 5 }, { 'active': tabs == 6 }]"
                    @click="tabs = 0">МОИ ТОВАРЫ</button>
                <button :class="[{ 'active': tabs == 1 }]" @click="tabs = 1">МОИ ПРОДАЖИ</button>
            </div>
            <div>
                <button :class="[{ 'active': tabs == 2 }]" @click="tabs = 2">ТРАНЗАКЦИИ</button>
                <button :class="[{ 'active': tabs == 3 }, { 'active': tabs == 7 }]" @click="tabs = 3">ОБРАТНАЯ
                    СВЯЗЬ</button>
            </div>
            <div class="text-center w-100">
                <button class="w-100" :class="[{ 'active': tabs == 4 }]" @click="tabs = 4">АККАУНТ</button>
            </div>
        </div>

        <div class="products" v-if="tabs == 0">
            <div class="products__filters d-flex align-items-center justify-content-end">
                <NuxtLink @click="tabs = 5">ДОБАВИТЬ ТОВАР</NuxtLink>

            </div>
            <div class="products__empty" v-if="products.length <= 0">
                <div class="text-center">
                    <h2>У ВАС ПОКА ЧТО НЕ ЗАГРУЖЕНО НИ ОДНОГО ТОВАРА.<br>
                        ДОБАВЬТЕ ТОВАР И НАЧНИТЕ ЗАРАБАТЫВАТЬ!</h2>
                </div>
            </div>
            <div class="products__list" v-else>
                <div class="products__item d-flex" v-for="(product, i) in products" :key="product.id">
                    <span>{{ i + 1 }}.</span>
                    <div class="product__img">
                        <img :src="pathUrl + product.main_image" alt="">
                    </div>

                    <div class="product__desc w-100">
                        <div class="d-flex justify-content-between">
                            <div>
                                <p>
                                    {{ product.name }}
                                </p>
                                <span>КОЛИЧЕСТВО ПРОДАЖ: {{ product.amaunt_buy }}</span>
                            </div>
                            <small>{{ product.price.toLocaleString() }} ₸</small>
                        </div>
                        <div class="d-flex align-items-center justify-content-end product__links">
                            <NuxtLink @click='openEditTab(product.id)' style="cursor: pointer;">ИЗМЕНИТЬ</NuxtLink>
                            <NuxtLink :to="'/product/' + product.id">СТРАНИЦА ТОВАРА</NuxtLink>
                        </div>
                    </div>
                </div>

            </div>

        </div>

        <div class="mysales" v-if="tabs == 1">
            <div class="products__empty" v-if="sales.length <= 0">
                <div class="text-center">
                    <h2>У ВАС ПОКА ЧТО НЕ БЫЛО ПРОДАЖ</h2>
                </div>
            </div>
            <div class="products__list" v-else>
                <div class="products__item d-flex" v-for="(sale, i) in sales" :key="sale.id">
                    <span>{{ i + 1 }}.</span>
                    <div class="product__img">
                        <img :src="pathUrl + sale.products.main_image" alt="">
                    </div>

                    <div class="product__desc w-100">
                        <div class="d-flex justify-content-between">
                            <div>
                                <p>
                                    {{ sale.products.name }}
                                </p>
                                <span>Дата пРОДАЖИ: {{ formatDate(sale.date) }}</span>
                            </div>
                            <small>{{ sale.products.price.toLocaleString() }} ₸</small>
                        </div>
                        <div class="d-flex align-items-center justify-content-end product__links">
                            <NuxtLink @click='tabs = 7' style="cursor: pointer;">Чат с покупателем</NuxtLink>
                            <NuxtLink :to="'/product/' + sale.products.id">Страница товара</NuxtLink>
                        </div>
                    </div>
                </div>

            </div>

        </div>
        <div class="chats" v-if="tabs == 3">
            <div class="chatempty text-center" v-if="chats.length <= 0">
                <h3>У ВАС ПОКА НЕ БЫЛО ЧАТОВ</h3>
            </div>
            <div class="chatlist d-flex" v-else>

                <div class="chatlist__item" v-for="chat in chats" :key="chat.id">
                    <div class="d-flex justify-content-between text">
                        <h3>{{ chat.buyer.user.email }}</h3>
                        <!-- <small>23.07.2023 14:47</small> -->
                    </div>

                    <div class="text-right">
                        <button @click="openChat(chat.id, chat.buyer.user.email)">ОТКРЫТЬ ЧАТ</button>
                    </div>
                </div>

            </div>
        </div>


        <div class="user" v-if="tabs == 4">
            <div class="user__info d-flex">
                <div>
                    <label for="name">Отображаемое имя</label>
                    <div class="edit__name">
                        <input type="text" id="name" name="name" class="nameInput" placeholder="Ваше имя" disabled
                            v-model="name">
                        <img src="@/assets/img/pen.svg" alt="" @click="editUnlock('nameInput')">
                    </div>

                    <label for="email">E-mail</label>
                    <input type="email" name="email" id="email" placeholder="Ваш E-mail" v-model="email">

                    <label for="password">Пароль</label>
                    <input type="password" name="password" id="password" placeholder="Ваш пароль">
                </div>

                <div>
                    <label for="description">Описание профиля</label>
                    <div class="edit__desc">
                        <textarea name="description" class="descInput" id="description" cols="30" rows="10"
                            placeholder="Описание профиля" disabled v-model="description"></textarea>
                        <img src="@/assets/img/pen.svg" alt="" @click="editUnlock('descInput')">
                    </div>
                </div>
            </div>
            <div class="d-flex justify-content-center form__footer">
                <button type="submit" @click="editAccount">СОХРАНИТЬ ИЗМЕНЕНИЯ</button>
                <button type="button" @click="logOut">ВЫЙТИ ИЗ АККАУНТА</button>
            </div>
        </div>

        <TheTrans v-if="tabs == 2" :transactions="transactions"></TheTrans>
        <TheCreate v-if="tabs == 5"></TheCreate>
        <TheEdit v-if="tabs == 6" :productId="sendId"></TheEdit>
        <TheMessanger v-if="tabs == 7" :chatId="chatId" :name="chatName"></TheMessanger>
    </div>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    data() {
        return {
            tabs: 0,
            minPrice: null,
            maxPrice: null,
            filter: false,
            test: false,
            test3: true,
            description: '',
            account: [],
            password: '',
            pathUrl: 'https://visuality.kz',
            email: '',
            name: '',
            sales: [],
            products: [],
            sendId: null,
            chatId: null,
            chats: [],
            chatName: '',
            transactions: [],

        }
    },
    methods: {
        getChats() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/messanger/all-chats`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;

            axios
                .get(path)
                .then(response => {
                    this.chats = response.data
                })
                .catch(error => {
                    console.log(error)
                })
        },
        openChat(chatId, chatName) {
            this.tabs = 7;
            this.chatId = chatId;
            this.chatName = chatName
        },
        formatDate(dateString) {
            const date = new Date(dateString);
            const formattedDate = `${date.getDate().toString().padStart(2, '0')}.${(date.getMonth() + 1).toString().padStart(2, '0')}.${date.getFullYear()}`;
            return formattedDate;
        },
        openEditTab(productId) {
            this.tabs = 6;
            this.sendId = productId;
        },
        editUnlock(elem) {
            const input = document.querySelector(`.${elem}`)

            input.disabled = false
            input.placeholder = ''
            input.focus()
            console.log(elem)
        },
        getAccount() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/seller/seller-lk`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    this.account = response.data
                    this.description = response.data.description
                    this.name = response.data.user.first_name
                    this.sales = response.data.my_sales
                    this.products = response.data.products
                    this.transactions = response.data.transactions

                })
                .catch(error => console.log(error));
        },
        editAccount() {
            const path = `${this.pathUrl}/api/seller/seller-lk/edit/`
            const csrf = this.getCSRFToken()
            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            axios
                .put(path,
                    {
                        user: {
                            first_name: this.name,
                            email: this.email,
                        },
                        description: this.description,
                    }
                )
                .then((res) => {
                    this.name = res.data.user.first_name
                    this.description = res.data.description
                    this.email = res.data.user.email
                })
                .catch((error) => {
                    console.error(error);
                });
        },
    },
    mounted() {
        const accType = localStorage.getItem('accountType')
        console.log(accType)
        if (accType == 'seller-account') {
            this.getAccount()
            this.getChats()
        }
        else {
            window.location.href = '/login'
        }
    }
}
</script>
<script setup>

useSeoMeta({
    title: 'Личный кабинет | Design Market',
    ogTitle: 'Личный кабинет | Design Market',
    description: 'Личный кабинет | Design Market',
    ogDescription: 'Личный кабинет | Design Market',
})
</script>
<style scoped>
.backtochat button {
    border-radius: 10px;
    border: 2px solid #fff;
    background: transparent;
    display: flex;
    align-items: center;
    gap: 10px;

    padding: 17px 40px;
    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    font-family: var(--int);
    color: #fff;
}

.mob {
    display: none;
}

.form__footer button {
    flex: 1;
    max-width: 405px;
    border-radius: 50px;
    border: 2px solid #fff;
    background: transparent;
    padding: 17px 0;

    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    font-family: var(--int);
    color: #fff;
    transition: all .3s ease;
}

.form__footer button:hover {
    color: #000;
    background: #fff;
}

.form__footer {
    margin-top: 40px;
    gap: 0 20px;
}

.edit__desc {
    position: relative;
}

.edit__desc img {
    position: absolute;
    right: 1%;
    top: 3%;

    cursor: pointer;
}

.edit__name {
    position: relative;
}

.edit__name img {
    position: absolute;
    right: 1.5%;
    top: 15%;
    cursor: pointer;
}

.user__info textarea {
    border-radius: 10px;
    border: 2px solid #fff;
    background: transparent;
    padding: 17px 20px;
    width: 58.333vw;
    height: 332px;

    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: 130%;
    font-family: var(--int);
    color: #fff;
}

.user__info label {
    display: block;
    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    text-transform: capitalize;
    font-family: var(--int);
    color: #fff;
    margin-bottom: 10px;
    margin-left: 20px;
}

.user__info input {
    border-radius: 10px;
    border: 2px solid #fff;
    background: transparent;
    padding: 17px 20px;
    height: 60px;
    width: 29.167vw;

    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    font-family: var(--int);
    color: #fff;
    margin-bottom: 40px;
}

.user__info input:last-child {
    margin-bottom: 0;
}

.user__info input::placeholder {
    color: rgba(0, 0, 0, 0.60);
}

.user__info {
    gap: 0 40px;
}

.user {
    margin-top: 54px;

}

.chatempty h3 {
    font-size: 40px;
    font-style: normal;
    font-weight: 300;
    line-height: 150%;
    font-family: var(--int);
    color: #fff;
}

.chatempty {
    margin: 164px 0;
}

.chatlist__item button {
    border-radius: 20px;
    border: 2px solid #fff;
    background: transparent;
    padding: 17px 78px;

    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    font-family: var(--int);
    color: #fff;
    margin-top: 44px;
    transition: all .3s ease;
}

.chatlist__item button:hover {
    color: #000;
    background: #fff;
}

.chatlist__item .text {
    gap: 0 130px;
}

.chatlist__item small {
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: 130%;
    font-family: var(--int);
    color: #000;
}

.chatlist__item h3 {
    font-size: 24px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
}

.chatlist__item {
    border-radius: 20px;
    background: #3D3D3D;
    box-shadow: 0px 0px 20px 0px rgba(47, 59, 163, 0.20);
    padding: 30px 42px;
}

.chatlist {
    margin-top: 54px;
    flex-wrap: wrap;
    gap: 40px;
}

.products__empty h2 {
    font-size: 40px;
    font-style: normal;
    font-weight: 300;
    line-height: 150%;
    font-family: var(--int);
    color: #fff;
    margin-top: 54px;
}

.filters__category {
    gap: 0px 40px;
}

.custom-checkbox p {
    font-family: var(--int);
    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: 110%;
    color: #000;
    white-space: nowrap;
}

.custom-checkbox input[type="checkbox"] {
    opacity: 0;
    position: absolute;
    border-radius: 4px;
}

.custom-checkbox .checkbox-text:before {
    content: url('@/assets/img/check.svg');
    display: inline-block;
    vertical-align: middle;
    width: 20px;
    height: 20px;
    margin-right: 10px;
    border: 1px solid #000;
    border-radius: 4px;
    margin-bottom: 4px;
}

.custom-checkbox input[type="checkbox"]:checked+.checkbox-text:before {
    content: url('@/assets/img/check.svg');
    font-size: 16px;
    color: black;
    text-align: center;
    line-height: 20px;
    background: #000;
}

.custom-checkbox {
    margin-bottom: 22px;
}

.custom-checkbox:last-child {
    margin-bottom: 0 !important;
}

.price__filter input {
    border-radius: 10px;
    border: 2px solid #fff;
    background: transparent;
    text-align: right;

    font-family: var(--int);
    color: #000;
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: 110%;
    max-width: 169px;
    height: 45px;
    padding: 10px 9px;
}

.price__filter input::placeholder {
    color: rgba(0, 0, 0, 0.60);
}

.price__filter {
    gap: 13px;
}

.filter__body h3 {
    font-family: var(--int);
    color: #000;
    font-size: 24px;
    font-style: normal;
    font-weight: 500;
    line-height: normal;
    margin-bottom: 20px;
    text-transform: uppercase;
}

.products__item {
    margin-bottom: 54px;
}

.product__links a {
    border-radius: 20px;
    border: 2px solid #fff;
    padding: 17px 65px;
    background: transparent;
    text-transform: uppercase;

    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    font-family: var(--int);
    color: #fff;
    transition: all .3s ease;
}

.product__links a:hover {
    color: #000;
    background: #fff;
}

.product__links {
    gap: 0 20px;
    margin-top: 100px;
}

.product__desc small {
    font-size: 32px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
    white-space: nowrap;
}

.product__desc p {
    margin-bottom: 30px;
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--hel);
    color: #fff;
}

.product__desc span {
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: 110%;
    text-transform: uppercase;
    font-family: var(--int);
    color: #000;
}

.product__desc {
    margin-left: 40px;
}

.products__list span {
    margin-right: 40px;
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--hel);
    color: #fff;
}

.products__list {
    margin-top: 54px;
}

.product__img {
    border-radius: 30px;
    box-shadow: 0px 0px 20px 0px rgba(47, 59, 163, 0.20);
}

.product__img img {
    border-radius: 30px;
    object-fit: cover;
    width: 29.74vw;
    height: 268.992px;
    object-position: top;
}

.products__filters button,
.products__filters a {
    padding: 17px 40px;
    background: transparent;
    border-radius: 20px;
    border: 2px solid #fff;
    transition: all .3s ease;
    cursor: pointer;

    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    font-family: var(--int);
    color: #fff;
    transition: all .3s ease;

}

.products__filters a:hover {
    color: #000;
    background: #fff;
}

.filter button {
    gap: 0 10px;
}

.filter {
    position: relative;
}

.filter__body {
    position: absolute;
    display: none;
    border-radius: 30px;
    box-shadow: 0px 0px 20px 0px rgba(47, 59, 163, 0.20);
    padding: 8px 20px 20px;
    background: #fff;
    left: -120%;
    z-index: 70;
    margin-top: 20px;
}

.filters__active {
    display: block !important;
}

.products__filters {
    gap: 0 20px;
}

.products {
    margin-top: 54px;
}

.tabs button {
    border: 2px solid #fff;
    padding: 17px;
    background: transparent;
    transition: all .3s ease;
    flex: 1;
    max-width: 250px;

    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    white-space: nowrap;
    font-family: var(--int);
    color: #fff;
}

.tabs button:nth-child(n + 1) {
    border-left: 0;
}

.tabs button:first-child {
    border-left: 2px solid #fff;
    border-top-left-radius: 10px;
    border-bottom-left-radius: 10px;
}

.tabs button:last-child {
    border-top-right-radius: 10px;
    border-bottom-right-radius: 10px;
}

.active {
    background: #fff !important;
    color: #000 !important;
}

.account h1 {
    font-size: 48px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
    margin-bottom: 54px;
}

.account {
    padding: 140px 100px 72px;
}

@media (max-width: 1440px) {
    .account {
        padding: 140px 50px 72px;
    }
}

@media (max-width: 1400px) {
    .products__list span {
        margin-right: 20px;
        font-size: 20px;
    }

    .product__desc p {
        font-size: 20px;
    }

    .product__desc small {
        font-size: 27px;
    }

    .product__desc {
        margin-left: 20px;
    }

    .product__img {
        /* height: auto; */
    }

    .product__links {
        margin-top: 25px;
    }

    .product__links a {
        font-size: 20px;
        padding: 14px 50px;
        white-space: nowrap;
    }

    .products__filters button,
    .products__filters a {
        font-size: 20px;
        padding: 15px 35px;
    }

    .tabs button {
        font-size: 20px;
        padding: 15px;
    }

    .products__item:last-child {
        margin-bottom: 0;
    }

    .chatlist {
        justify-content: center;
        margin-top: 20px;
    }

    .chatlist__item {
        width: 100%;
    }

    .user__info label {
        font-size: 20px;
    }

    .user__info {
        gap: 20px;

    }

}

@media (max-width: 1024px) {
    .user__info {
        flex-direction: column;
    }

    .user__info label {
        font-size: 16px;
    }

    .user__info input,
    .user__info textarea {
        width: 100%;
        font-size: 16px;
        margin-bottom: 20px;
        border: 2px solid #fff;
    }

    .form__footer {

        margin-top: 0;
        flex-direction: column;
        gap: 20px;

    }

    .form__footer button {
        flex: none;
        padding: 13px 16px;
        border: 2px solid #fff;
        font-size: 16px;
    }

    .crack {
        flex-direction: column;
    }

    .backtochat button {
        margin-bottom: 10px;
        font-size: 16px;
        border: 2px solid #fff;
        width: 100%;
        padding: 15px;
        flex: 1;
        justify-content: space-between;
    }

    .mob {
        display: block;
    }

    .account {
        padding: 140px 20px 50px;
    }

    .pc {
        display: none !important;
    }

    .account h1 {
        font-size: 32px;
    }

    .products__list span {
        display: none;
    }

    .products__item {
        flex-direction: column;
    }

    .product__img img {
        width: 100%;
        max-height: 165px;
    }

    .product__img img {}

    .product__desc {
        margin-left: 0;
        margin-top: 10px;
    }

    .products__list span:last-child {
        display: block !important;
        font-size: 16px !important;
    }

    .product__desc small {
        font-size: 20px;
    }

    .product__links {
        justify-content: space-between !important;
        gap: 10px;
    }

    .product__links a {
        flex: 1;
        text-align: center;
        border: 2px solid #fff;
        padding: 13px 15px;
        font-size: 14px;
        text-transform: none;
    }

    .product__desc div:first-child {
        align-items: flex-end;
    }

    .product__desc p {
        margin-bottom: 10px;
        font-size: 16px;
    }

    .products__filters button,
    .products__filters a {
        padding: 13px 17px;
        font-size: 14px;
        border: 2px solid #fff;
        font-weight: 400;
        flex: 1;
        text-align: center;
        white-space: normal;
    }

    .products__list {
        margin-top: 30px;
    }

    .tabs div {
        display: flex;
        margin-bottom: 10px;
        /* justify-content: center; */
    }

    .tabs button {
        font-size: 16px;
        border: 2px solid #fff;
        flex: 1;
        max-width: unset;
    }

    .products {
        margin-top: 20px;
    }

    .filter__body {
        left: -130%;
        padding: 20px 20px 0;
        width: 350px;
    }

    .filters__category {
        flex-direction: column;
    }

    .filters__category label {
        display: block;
    }

    .custom-checkbox:last-child {
        margin-bottom: 26px !important;
    }

    .custom-checkbox {
        margin-bottom: 26px;
    }

    .price__filter input {
        max-width: 145px;
        font-size: 20px;
    }

    .price__filter {
        gap: 4px;
    }

    .filter__body h3 {
        font-size: 20px;
        margin-bottom: 20px;
    }

    .custom-checkbox p {
        font-size: 16px;
    }

    .chatlist__item h3 {
        font-size: 20px;
        margin: 0;
    }

    .chatlist__item small {
        font-size: 16px;
    }

    .chatlist__item button {
        font-size: 20px;
        padding: 15px 30px;
        border: 2px solid #fff;
    }

    .chatlist__item {
        padding: 20px;
    }

    .chatlist__item .text {
        gap: 20px;
    }

    .chatlist {
        gap: 20px;
    }
}

@media (max-width: 500px) {
    .chatlist__item .text {
        gap: 20px;
        flex-direction: column;
    }

    .chatlist__item button {
        margin-top: 20px;
    }
}

@media (max-width: 380px) {
    .filter__body {
        left: -125%;
        padding: 20px 20px 0;
        width: 350px;
    }

    .products__filters button,
    .products__filters a {
        font-size: 14px;
        flex: 1;
    }

    .products__list span:last-child {
        display: block !important;
        font-size: 14px !important;
    }

    .product__desc small {
        font-size: 18px;
    }

    .product__desc p {
        margin-bottom: 10px;
        font-size: 14px;
    }
}
</style>