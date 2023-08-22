<template>
    <div class="account">
        <h1>личный кабинет</h1>
        <div class="d-flex justify-content-between w-100 crack">
            <div v-if="tabs != 4"></div>
            <div class="backtochat" v-if="tabs == 4">
                <button @click="tabs = 2">
                    <img src="@/assets/img/backarrow.svg" alt="">
                    НАЗАД
                </button>
            </div>
            <div class="tabs d-flex align-items-center justify-content-end pc">

                <button :class="[{ 'active': tabs == 0 }]" @click="tabs = 0">МОИ ПОКУПКИ</button>
                <button :class="[{ 'active': tabs == 1 }]" @click="tabs = 1">ТРАНЗАКЦИИ</button>
                <button :class="[{ 'active': tabs == 2 }, { 'active': tabs == 4 }]" @click="tabs = 2">ОБРАТНАЯ
                    СВЯЗЬ</button>
                <button :class="[{ 'active': tabs == 3 }]" @click="tabs = 3">АККАУНТ</button>
            </div>
        </div>

        <div class="tabs mob">
            <div>
                <button :class="[{ 'active': tabs == 0 }]" @click="tabs = 0">МОИ ПОКУПКИ</button>
                <button :class="[{ 'active': tabs == 1 }]" @click="tabs = 1">ТРАНЗАКЦИИ</button>
            </div>
            <div>
                <button :class="[{ 'active': tabs == 2 }, { 'active': tabs == 4 }]" @click="tabs = 2">ОБРАТНАЯ
                    СВЯЗЬ</button>
                <button :class="[{ 'active': tabs == 3 }]" @click="tabs = 3">АККАУНТ</button>
            </div>

        </div>


        <div class="mysales" v-if="tabs == 0">
            <div class="products__empty" v-if="buys.length <= 0">
                <div class="text-center">
                    <h2>У ВАС ПОКА ЧТО НЕ БЫЛО ПОКУПОК<br>
                        ПЕРЕЙТИ К ПОКУПКАМ</h2>

                    <NuxtLink to="/catalog">
                        Каталог
                    </NuxtLink>
                </div>
            </div>
            <div class="products__list" v-else>
                <div class="products__item d-flex" v-for="(buy, i) in buys" :key="buy.id">
                    <span>{{ i + 1 }}.</span>
                    <div class="product__img">
                        <img :src="buy.products.main_image" alt="">
                    </div>

                    <div class="product__desc w-100">
                        <div class="d-flex justify-content-between">
                            <div>
                                <p>
                                    {{ buy.products.name }}
                                </p>
                                <span>Дата покупки: {{ formatDate(buy.date) }}</span>
                                <!-- <h3>АВТОР:
                                    <small>Александр Иванов</small>
                                </h3> -->
                            </div>
                            <small>{{ buy.products.price.toLocaleString() }} ₸</small>
                        </div>
                        <div class="d-flex align-items-center justify-content-end product__links">
                            <NuxtLink @click='createChat(buy.seller.id, buy.seller.user.first_name)'
                                style="cursor: pointer;">ЧАТ С ПРОДАВЦОМ
                            </NuxtLink>
                            <NuxtLink v-if="buy.products.main_file" :to="buy.products.main_file">СКАЧАТЬ ФАЙЛ
                            </NuxtLink>
                        </div>
                    </div>
                </div>
            </div>

        </div>
        <div class="chats" v-if="tabs == 2">
            <div class="chatempty text-center" v-if="chats.length <= 0">
                <h3>У ВАС ПОКА НЕ БЫЛО ЧАТОВ</h3>
            </div>
            <div class="chatlist d-flex" v-else>
                <div class="chatlist__item" v-for="chat in chats" :key="chat.id">
                    <div class="d-flex justify-content-between text">
                        <h3>{{ chat.seller.user.first_name }}</h3>
                        <!-- <small>23.07.2023 14:47</small> -->
                    </div>

                    <div class="text-right">
                        <button @click="openChat(chat.id, chat.seller.user.first_name)">ОТКРЫТЬ ЧАТ</button>
                    </div>
                </div>
            </div>
        </div>


        <form class="user" v-if="tabs == 3">
            <div class="user__info d-flex justify-content-center">
                <div>
                    <label for="email">E-mail</label>
                    <input type="email" name="email" id="email" placeholder="Ваш e-mail" v-model="email">
                </div>
                <div>
                    <label for="password">Пароль</label>
                    <input type="password" name="password" id="password" placeholder="Ваш пароль" v-model="password">
                </div>
            </div>
            <div class="text-center">
                <button type="button" @click="logOut">ВЫЙТИ ИЗ АККАУНТА</button>
            </div>
        </form>
        <TheTrans v-if="tabs == 1" :transactions="transactions"></TheTrans>
        <TheMessanger v-if="tabs == 4" :chatId="chatId" :name="chatName"></TheMessanger>
    </div>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    data() {
        return {
            tabs: 3,
            test: true,
            account: [],
            password: '',
            email: '',
            chats: [],
            seller: [],
            pathUrl: 'https://visuality.kz',
            buys: [],
            sendId: null,
            chatId: null,
            myId: null,
            transactions: []
        }
    },
    methods: {
        formatDate(dateString) {
            const date = new Date(dateString);
            const formattedDate = `${date.getDate().toString().padStart(2, '0')}.${(date.getMonth() + 1).toString().padStart(2, '0')}.${date.getFullYear()}`;
            return formattedDate;
        },
        getAccount() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/buyer/buyer-lk`;
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    this.account = response.data
                    this.myId = response.data.id
                    this.transactions = response.data.transactions
                    this.email = response.data.user.email

                })
                .catch(error => console.log(error));
        },
        openChat(chatId, chatName) {
            this.tabs = 4;
            this.chatId = chatId;
            this.chatName = chatName
        },
        getBuys() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/buyer/buyer-lk/my-purchases`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(response => {
                    this.buys = response.data
                })
                .catch(error => {
                    console.error(error)
                })
        },
        createChat(id, name) {
            const token = this.getAuthorizationCookie()
            const csrf = this.getCSRFToken()
            const path = `${this.pathUrl}/api/messanger/new-chat`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            axios
                .post(path, {
                    buyer: this.myId,
                    seller: id,
                })
                .then(response => {
                    const chatId = response.data.chat_id
                    this.openChat(chatId, name)
                })
                .catch(error => console.log(error))
        },
        getChats() {
            const token = this.getAuthorizationCookie()
            const path = `${this.pathUrl}/api/messanger/all-chats`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios
                .get(path)
                .then(res => {
                    this.chats = res.data
                })
                .catch(error => console.log(error))
        }
    },
    mounted() {
        const accType = localStorage.getItem('accountType')
        console.log(accType)
        if (accType == 'buyer-account') {
            this.getAccount()
            this.getChats()
            this.getBuys()
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

.user button {
    margin-top: 40px;
    background: transparent;
    border-radius: 20px;
    border: 2px solid #fff;
    padding: 17px 73px;

    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    font-family: var(--int);
    color: #fff;
    transition: all .3s ease;
}

.user button:hover {
    color: #000;
    background: #fff;
}

.user__info {
    gap: 0 20px;
}

.user__info label {
    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    font-family: var(--int);
    color: #fff;
    display: block;
}

.user__info input {
    border-radius: 10px;
    border: 2px solid #fff;
    background: transparent;
    padding: 17px 20px;

    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: 110%;
    color: #fff;
    font-family: var(--hel);
    height: 60px;
    width: 29.167vw;
}

.user__info input::placeholder {
    color: rgba(0, 0, 0, 0.60);
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
    padding: 30px 42px;
}

.chatlist {
    margin-top: 54px;
    flex-wrap: wrap;
    gap: 40px;
}

.products__empty a {
    border-radius: 20px;
    border: 2px solid #fff;
    background: transparent;
    padding: 17px 5.625vw;

    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    font-family: var(--int);
    color: #fff;
    margin-top: 54px;
    transition: all .3s ease;
}

.products__empty a:hover {
    color: #000;
    background: #fff;
}

.products__empty h2 {
    font-size: 40px;
    font-style: normal;
    font-weight: 400;
    line-height: 150%;
    font-family: var(--int);
    color: #fff;
    margin-top: 107px;
    margin-bottom: 54px;
}

.product__desc h3 {
    white-space: nowrap;
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: 110%;
    color: #000;
    font-family: var(--int);
    margin-top: 30px;
}

.product__desc h3 small {
    white-space: nowrap;
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: 110%;
    color: #000;
    font-family: var(--int);
    margin-top: 30px;
    text-decoration: underline;
    text-transform: none;
}

.products__item {
    margin-bottom: 54px;
}

.product__links a {
    border-radius: 20px;
    border: 2px solid #fff;
    padding: 17px 65px;
    background: transparent;

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
    font-family: var(--hel);
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
    border-radius: 20px;
    height: fit-content;
    box-shadow: 0px 0px 20px 0px rgba(47, 59, 163, 0.20);
}

.product__img img {
    border-radius: 20px;
    object-fit: cover;
    width: 29.74vw;
    height: 268.992px;
    object-position: top;
}

.tabs button {
    border: 2px solid #fff;
    padding: 17px;
    background: transparent;
    transition: all .3s ease;
    flex: 1;
    max-width: 250px;
    white-space: nowrap;

    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
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
        height: fit-content;
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

    .user button {
        margin-top: 20px;
        font-size: 20px;
        border: 2px solid #fff;
        width: 100%;
        padding: 15px 0;
    }

    .user__info input {
        margin-bottom: 10px;
    }

    .user {
        margin-top: 20px;
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