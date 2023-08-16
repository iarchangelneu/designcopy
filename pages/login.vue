<template>
    <div class="login d-flex">
        <div class="logo">
            <div class="img">
                <img src="@/assets/img/logoform.svg" class="img-fluid" alt="">
            </div>
        </div>
        <div class="login__form">
            <div class="form__body">
                <div class="cal">
                    <div class="text-center">
                        <h1>аВТОРИЗАЦИЯ</h1>
                    </div>
                    <input type="email" name="email" id="email" placeholder="E-mail" v-model="email">
                    <input type="password" name="password" id="password" placeholder="Пароль" v-model="password">

                    <button class="w-100" @click="login">Войти</button>
                    <div class="text-center">
                        <span>Еще нет аккаунта? <NuxtLink to='/register'>Регистрация</NuxtLink></span>
                    </div>
                </div>
            </div>
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
            email: '',
            password: '',
            pathUrl: 'https://themes.kz',
        }
    },
    methods: {
        login() {
            const path = `${this.pathUrl}/api/main/authorization`
            const csrf = this.getCSRFToken()

            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            axios
                .post(path, { username: this.email, password: this.password })
                .then((res) => {

                    if (res.status == 202) {

                        document.cookie = `Authorization=${res.data.token}; expires=Fri, 31 Dec 2023 23:59:59 GMT; path=/`;
                        localStorage.setItem('accountType', res.data.redirect_url)
                        if (res.data.redirect_url == 'buyer-account') {
                            window.location.href = '/'
                        }
                        if (res.data.redirect_url == 'seller-account') {
                            window.location.href = '/seller-account'
                        }
                    }
                    else {

                    }
                    console.log(res)
                })
                .catch((error) => {
                    console.log(error);
                    // this.error = error.response.data.non_field_errors[0]
                });
        }
    }
}
</script >
<script setup>
useSeoMeta({
    title: 'Авторизация | Design Market',
    ogTitle: 'Авторизация | Design Market',
    description: 'Авторизация | Design Market',
    ogDescription: 'Авторизация | Design Market',
})
</script>
<style scoped>
.logo {
    background: transparent;
    width: 100%;
}

.form__body span a {
    color: #fff;
    text-decoration: underline !important;
}

.form__body span {
    display: block;
    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    color: #fff;
    font-family: var(--int);
    margin-top: 20px;
}

.form__body button {
    border-radius: 20px;
    border: 2px solid #fff;
    background: transparent;
    padding: 17px 0;

    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: 110%;
    font-family: var(--int);
    color: #fff;
    transition: all .3s ease;
}

.form__body button:hover {
    background: #fff;
    color: #000;
}

.form__body input {
    display: block;
    width: 625px;
    margin-bottom: 20px;
    border-radius: 10px;
    border: 2px solid #fff;
    background: transparent;
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: 110%;
    font-family: var(--hel);
    color: #fff;

    padding: 17px 23px;
}

.form__body input::placeholder {
    color: rgba(0, 0, 0, 0.60);
}

.form__body h1 {
    font-size: 48px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
    margin-bottom: 30px;
}

.login__form {
    padding: 0 47px;
    background: #161616;
}

.login {
    height: 100vh;
}

.login__form,
.logo {
    display: flex;
    justify-content: center;
    align-items: center;
}

.form__body,
.img {
    display: flex;
    flex-direction: column;
    align-items: center;
}

@media (max-width: 1024px) {
    .form__body input {
        width: 100%;
        height: 45px;
        font-size: 20px;
        border: 2px solid #fff;
    }

    .form__body button {
        font-size: 16px;
        padding: 13px 0;
        border: 2px solid #fff;
    }

    .form__body span {
        font-size: 16px;
    }

    .login {
        flex-direction: column-reverse;
        padding: 50px 20px 0;
        justify-content: start;
    }

    .form__body,
    .cal {
        width: 100%;
    }

    .form__body h1 {
        font-size: 20px;
        margin-bottom: 30px;
    }


    .login__form {
        background: transparent;
        padding: 0;
        margin-bottom: 100px;
    }
}
</style>