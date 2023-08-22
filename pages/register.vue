<template>
    <div class="login d-flex">
        <div class="logo">
            <div class="img">
                <img src="@/assets/img/logoform.svg" class="img-fluid" alt="">
            </div>
        </div>
        <div class="login__form">
            <div class="form__body">
                <div>
                    <div class="text-center">
                        <h1>Регистрация</h1>
                    </div>
                    <input type="email" name="email" id="email" placeholder="E-mail" v-model="email">
                    <input type="password" name="password" id="password" placeholder="Пароль" v-model="password">
                    <input type="password" name="repeat-password" id="repeat-password" placeholder="Повторите пароль"
                        v-model="repeat__password">
                    <div class="switch-button">
                        <span class="active" :style="{ left: activeSwitchPosition }"></span>
                        <button type="button" class="switch-button-case left"
                            :class="{ 'active-case': activeSwitchPosition === '0%' }"
                            @click="switchLeft">ПОКУПАТЕЛЬ</button>
                        <button type="button" class="switch-button-case right"
                            :class="{ 'active-case': activeSwitchPosition === '50%' }"
                            @click="switchRight">ПРОДАВЕЦ</button>
                    </div>
                    <label class="custom-checkbox">
                        <input type="checkbox" class="" value="0">
                        <p class="checkbox-text m-0">Принимаю условия <NuxtLink to='/terms'>Пользовательского соглашения
                            </NuxtLink> и <NuxtLink to='/polytics'>Политики
                                конфиденциальности</NuxtLink>
                        </p>
                    </label>
                    <button class="w-100 register" @click="register">ЗАРЕГИСТРИРОВАТЬСЯ</button>
                    <div class="text-center">
                        <span>Уже есть аккаунт? <NuxtLink to='/login'>Войти</NuxtLink></span>

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
            activeSwitchPosition: '50%',
            email: '',
            password: '',
            repeat__password: '',
            pathUrl: 'https://visuality.kz',
        }
    },
    methods: {
        switchLeft() {
            this.activeSwitchPosition = '0%';
        },
        switchRight() {
            this.activeSwitchPosition = '50%';
        },
        register() {
            const buyer = `${this.pathUrl}/api/main/registration/buyer`
            const seller = `${this.pathUrl}/api/main/registration/seller`
            const csrf = this.getCSRFToken()
            if (this.activeSwitchPosition == '50%') {
                axios.defaults.headers.common['X-CSRFToken'] = csrf;
                axios
                    .post(seller, { email: this.email, password: this.password, username: this.email })
                    .then((res) => {
                        document.cookie = `Authorization=${res.data.token}; expires=Fri, 31 Dec 2023 23:59:59 GMT; path=/`;
                        console.log(res)
                        localStorage.setItem('accountType', res.data.redirect_url)
                        window.location.href = res.data.redirect_url
                    })
                    .catch((error) => {
                        console.error(error);
                    });
            }
            else {
                axios.defaults.headers.common['X-CSRFToken'] = csrf;
                axios
                    .post(buyer, { email: this.email, password: this.password, username: this.email })
                    .then((res) => {
                        document.cookie = `Authorization=${res.data.token}; expires=Fri, 31 Dec 2023 23:59:59 GMT; path=/`;
                        console.log(res)
                        localStorage.setItem('accountType', res.data.redirect_url)
                        window.location.href = '/'
                    })
                    .catch((error) => {
                        console.error(error);
                    });
            }
        },
    },
}
</script>
<script setup>
useSeoMeta({
    title: 'Регистрация | Design Market',
    ogTitle: 'Регистрация | Design Market',
    description: 'Регистрация | Design Market',
    ogDescription: 'Регистрация | Design Market',
})
</script>
<style scoped>
.custom-checkbox input[type="checkbox"] {
    opacity: 0;
    position: absolute;
    border-radius: 4px;
}

.custom-checkbox .checkbox-text:before {
    content: '';
    display: inline-block;
    vertical-align: middle;
    width: 20px;
    height: 20px;
    margin-right: 12px;
    border: 1px solid #fff;
    border-radius: 4px;
    margin-bottom: 4px;
}

.custom-checkbox input[type="checkbox"]:checked+.checkbox-text:before {
    content: url('@/assets/img/check.svg');
    font-size: 16px;
    color: black;
    text-align: center;
    line-height: 20px;
    background: #fff;
}

.custom-checkbox a {
    color: #fff;
    text-decoration: underline !important;
    z-index: 50;
}

.custom-checkbox p {
    font-size: 16px;
    font-style: normal;
    font-weight: 400;
    line-height: 130%;
    font-family: var(--hel);
    color: #fff;
}

.custom-checkbox {
    margin-bottom: 30px;
}

.switch-button {
    width: 100%;
    height: 60px;
    text-align: center;
    position: relative;
    left: 50%;
    -webkit-transform: translate3D(-50%, -50%, 0);
    transform: translate3D(-50%, -50%, 0);
    will-change: transform;
    z-index: 197 !important;
    cursor: pointer;
    transition: .3s ease all;
    border: 2px solid #fff;
    background: transparent;
    border-radius: 8px;
    margin-top: 50px;
}

.switch-button-case {
    display: inline-block;
    background: none;
    width: 50%;
    height: 100%;
    color: #36589f;
    position: relative;
    border: none;
    transition: .3s ease all;

    padding-bottom: 1px;

    font-style: normal;
    font-weight: 300;
    font-size: 20px;
    line-height: 110%;
    text-transform: uppercase;
    color: #fff;
    font-family: var(--int);
}

.switch-button-case:hover {
    color: grey;
    cursor: pointer;
}

.switch-button-case:focus {
    outline: none;
}

.switch-button .active {
    color: #fff;
    background-color: #fff;
    box-shadow: 0px 1px 3px rgba(0, 0, 0, 0.1), 0px 1px 2px rgba(0, 0, 0, 0.06);
    border-radius: 6px;
    position: absolute;
    left: 0;
    top: 0;
    width: 50%;
    height: 100%;

    z-index: -1 !important;
    margin-top: 0;
    margin-left: 0;
    transition: .3s ease-out all;
}

.switch-button .active-case {
    color: #000;
}

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

.register {
    border-radius: 20px;
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

.register:hover {
    background: #fff;
    color: #000;
}

.form__body input {
    display: block;
    width: 100%;
    margin-bottom: 20px;
    border-radius: 10px;
    border: 2px solid #fff;
    background: transparent;
    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: 110%;
    font-family: var(--int);
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
        justify-content: flex-start;
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

    .switch-button button {
        border: 0px solid #fff;
        border-radius: 8px;
    }

    .switch-button {
        border: 2px solid #fff;
    }

    .custom-checkbox {
        margin-bottom: 20px;
    }
}
</style>