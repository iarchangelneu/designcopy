<template>
    <div class="empty" v-if="transactions.length <= 0">
        <h2>СПИСОК ТРАНЗАКЦИЙ ОТОБРАЗИТСЯ ПОСЛЕ<br> ПЕРВОЙ ПОКУПКИ</h2>
    </div>
    <div class="transactions" v-else>
        <table class="table table-borderless text-center pc">
            <thead>
                <tr>
                    <th>Тип</th>
                    <th>Цена</th>
                    <th>Дата</th>
                    <th>Статус</th>
                    <th>Состояние счета</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="item in transactions" :key="item.id">
                    <td>{{ item.type_operation }}</td>
                    <td> {{ item.amount.toLocaleString() }} ₸</td>
                    <td>{{ formatDate(item.date) }}</td>
                    <td :class="{ success: item.paid == 1, failure: item.paid == 0 }">{{ item.paid == 1 ? 'Совершено' :
                        item.paid == 0 ? 'Отменено' : item.paid }}</td>
                    <td>{{ calculateAmountNow(item) }} ₸ </td>
                </tr>
                <!-- <tr>
                    <td>Продажа</td>
                    <td>+ 8 000 ₸</td>
                    <td>05.12.22</td>
                    <td>15:47</td>
                    <td class="failure">Отменено</td>
                    <td>18 000 ₸</td>
                </tr> -->

            </tbody>
        </table>
        <div class="mob">
            <div class="transa" v-for="item in transactions" :key="item.id">
                <div class="trans__item">
                    <div class="d-flex align-items-center justify-content-between">
                        <span>{{ item.type_operation }}</span>
                        <small>{{ formatDate(item.date) }}</small>
                        <img src="@/assets/img/succes.svg" alt="" v-if="item.paid == 1">
                        <img src="@/assets/img/failure.svg" alt="" v-if="item.paid == 0">
                    </div>
                    <div class="d-flex justify-content-between prices">
                        <div>
                            <span>Цена</span>
                            <small>+ {{ item.amount.toLocaleString() }} ₸</small>
                        </div>
                        <div>
                            <span>Состояние счета</span>
                            <small>{{ calculateAmountNow(item) }} ₸</small>
                        </div>
                    </div>
                </div>
            </div>

        </div>
    </div>
</template>
<script>
export default {
    props: {
        transactions: [],
    },
    data() {
        return {
            test: false,

        }
    },
    methods: {
        calculateAmountNow(item) {
            if (item.type_operation === 'ВЫВОД' && item.paid === 0) {
                return (item.amount_now + item.amount).toLocaleString();
            } else if (item.type_operation === 'ПОПОЛНЕНИЕ' && item.paid === 0) {
                return (item.amount_now - item.amount).toLocaleString();
            } else {
                return item.amount_now.toLocaleString();
            }
        },
        formatDate(dateTime) {
            // Создаем объект Date из строки времени
            const date = new Date(dateTime);

            // Получаем отдельные компоненты даты и времени
            const day = String(date.getDate()).padStart(2, '0');
            const month = String(date.getMonth() + 1).padStart(2, '0');
            const year = String(date.getFullYear()).slice(-2);
            const hours = String(date.getHours()).padStart(2, '0');
            const minutes = String(date.getMinutes()).padStart(2, '0');

            // Собираем строку в нужном формате
            return `${day}.${month}.${year} ${hours}:${minutes}`;
        },
    }
}
</script>
<style scoped>
.transa {
    border-bottom: 1px solid #ECECEC;
}

.transa:last-child {
    border-bottom: 0 !important;
}

.prices {
    margin-top: 15px;
}

.prices span {
    margin-bottom: 10px;
}

.trans__item span {
    font-size: 15px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    font-family: var(--int);
    color: #fff;
    opacity: 0.5;
    display: block;
}

.trans__item small {
    font-size: 14px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    color: #fff;
    font-family: var(--int);
}

.mob {
    display: none;
}

.empty {
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 180px 0;
}

.empty h2 {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    color: #fff;
    font-family: var(--nt);
}

.transactions td {
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    font-family: var(--hel);
    color: #fff;
}

.transactions th {
    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    font-family: var(--int);
    color: #fff;
    opacity: 0.5;
}

.success {
    color: #03B820 !important;
}

.failure {
    color: #EA0505 !important;
}

.transactions {
    padding: 50px 100px;
    margin-top: 40px;
}

@media (max-width: 1400px) {
    .transactions {
        padding: 50px;
    }
}

@media (max-width: 1024px) {
    .pc {
        display: none;
    }

    .mob {
        display: block;
    }

    .transactions {
        padding: 0;
    }

    .trans__item {
        padding: 30px;
    }
}
</style>