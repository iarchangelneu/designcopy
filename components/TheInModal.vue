<template>
    <div class="modal fade" id="inModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modalbody">
                    <div class="text-center">
                        <h1>
                            Пополнение баланса
                        </h1>
                    </div>

                    <div class="tabers" v-if="accountType == 'buyer'">
                        <button>ПОПОЛНЕНИЕ</button>
                        <button data-toggle="modal" data-dismiss="modal" aria-label="Close"
                            data-target="#outModal">ВЫВОД</button>
                    </div>
                    <p>Порядок действий для пополнения баланса</p>
                    <p>1. Подтвердите Ваше согласие с правилами нашей системы</p>
                    <label class="custom-checkbox">
                        <input type="checkbox" class="" value="0">
                        <p class="checkbox-text m-0">Я согласен с <NuxtLink to='/terms'>пользовательским соглашением
                            </NuxtLink> и <NuxtLink to='/polytics'>политикой
                                конфиденциальности</NuxtLink>
                        </p>
                    </label>
                    <p>2. Введите сумму, на которую Вы хотите пополнить личный счет, и нажмите на кнопку “Пополнить”. Вы
                        будете переадресованы на сайт платежной системы, где сможете завершить платеж.</p>

                    <form class="d-flex sendMoney">
                        <input type="text" v-model="count" name="count" id="count" placeholder="100 ₸">
                        <button type="button" @click="inMoney()" ref="inBtn">ПОПОЛНИТЬ</button>
                    </form>

                    <div class="modalfooter d-flex">
                        <button :class="{ active: count == 100 }" @click="count = 100">100 ₸</button>
                        <button :class="{ active: count == 1000 }" @click="count = 1000">1000 ₸</button>
                        <button :class="{ active: count == 2000 }" @click="count = 2000">2000 ₸</button>
                        <button :class="{ active: count == 5000 }" @click="count = 5000">5000 ₸</button>
                        <button :class="{ active: count == 10000 }" @click="count = 10000">10000 ₸</button>
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
            count: null,
            pathUrl: 'https://visuality.kz',
            accountType: '',
        }
    },
    methods: {
        inMoney() {
            const token = this.getAuthorizationCookie()
            const csrf = this.getCSRFToken()
            const path = `${this.pathUrl}/api/money/new-pay`
            axios.defaults.headers.common['Authorization'] = `Token ${token}`;
            axios.defaults.headers.common['X-CSRFToken'] = csrf;
            this.$refs.inBtn.innerHTML = 'ОЖИДАЙТЕ'

            axios
                .post(path, {
                    amount: this.count
                })
                .then(response => {
                    console.log(response)
                    window.location.href = response.data.url
                    if (response.status = 201) {
                        this.$refs.inBtn.innerHTML = 'ПОПОЛНИТЬ'
                    }
                    if (response.status == 228) {
                        this.$refs.outBtn.innerHTML = response.data.error_msg
                    }
                })
                .catch(error => {
                    console.error(error)
                    this.$refs.inBtn.innerHTML = 'ПОПОЛНИТЬ'
                })
        },
    },
    mounted() {
        const accType = localStorage.getItem('accountType')
        if (accType == 'buyer-account') {
            this.accountType = 'buyer'

        }
        else if (accType == 'seller-account') {
            this.accountType = 'seller'
        }
        else {
            return
        }
    }
}
</script>
<style scoped>
.tabers {
    display: flex;
    margin-bottom: 15px;
}

.tabers button {
    flex: 1;
    border: 2px solid #fff;
    border-radius: 10px;
    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    font-family: var(--int);
    padding: 17px 0;
}

.tabers button:nth-child(1) {
    border-right: 0;
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;
    background: #fff;
    color: #000;
}

.tabers button:nth-child(2) {
    border-left: 0;
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
    background: transparent;
    color: #fff;
}

.modalfooter {
    gap: 10px;
    flex-wrap: wrap;
    justify-content: center;
    margin-top: 25px;
}

.modalfooter button {
    padding: 12px;
    background: transparent;
    border-radius: 20px;
    border: 2px solid #fff;
    transition: all .3s ease;
    max-width: 108px;
    white-space: nowrap;

    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    font-family: var(--int);
    color: #fff;
}

.modalfooter button:hover {
    color: #000;
    background: #fff;
}

.active {
    color: #000 !important;
    background: #fff !important;
}

.sendMoney button {
    width: 100%;
    border-radius: 20px;
    border: 2px solid #fff;
    background: transparent;
    padding: 17px 0;
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    font-family: var(--int);
    color: #fff;
    transition: all .3s ease;
}

.sendMoney button:hover {
    color: #000;
    background: #fff;
}

.sendMoney input {
    border-radius: 15px;
    border: 2px solid #fff;
    background: transparent;
    padding: 10px;
    text-align: right;

    font-size: 32px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    color: #fff;
    font-family: var(--hel);
    max-width: 227px;
}

.sendMoney input::placeholder {
    color: rgba(0, 0, 0, 0.60);
}

.sendMoney {
    gap: 0 20px;
}

.custom-checkbox a {
    margin-bottom: 0;
    color: #fff;
    text-decoration: underline !important;
}

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
    margin-right: 10px;
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

.custom-checkbox {
    margin-bottom: 25px;
}

.custom-checkbox:last-child {
    margin-bottom: 0 !important;
}

.modalbody p {
    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: 130%;
    font-family: var(--hel);
    color: #fff;
    margin-bottom: 25px;
}

.modalbody h1 {
    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
    margin-bottom: 25px;
}

.modalbody {
    padding: 38px;
}

.modal-content {
    border-radius: 20px;
    background: #3D3D3D;
    border: 0;
    box-shadow: 0px 0px 20px 0px rgba(47, 59, 163, 0.20);
}

@media (min-width: 576px) {
    .modal-dialog {
        max-width: 611px;
        margin: 1.75rem auto;
    }
}

@media (max-width: 500px) {
    .modalbody {
        padding: 18px;
    }

    .modalbody h1 {
        font-size: 20px;
        margin-bottom: 20px;
    }

    .modalbody p {
        font-size: 16px;
        margin-bottom: 10px;
    }

    .sendMoney input {
        max-width: 147px;
        height: 45px;
        font-size: 20px;
    }

    .sendMoney button {
        font-size: 16px;
        padding: 12px 34px;
    }

    .modalfooter button {
        font-size: 16px;
    }

    .modal-dialog {
        display: -ms-flexbox;
        display: flex;
        -ms-flex-align: center;
        align-items: center;
        min-height: calc(100% - 1rem)
    }

}
</style>