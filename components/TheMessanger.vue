<template>
    <div class="messanger d-flex">

        <div class="messanger__list" v-if="!isSeller">
            <div class="messanger__item" v-for="chat in chats" :key="chat.id"
                @click="newChat(chat.id, chat.seller.user.first_name)">
                <div class="name d-flex align-items-center">
                    <h3 class="mb-0">{{ chat.seller.user.first_name }}</h3>
                    <small>{{ getLastMessageTime(chat) }}</small>
                </div>
                <p class="mb-0">{{ getLastMessageText(chat) }}</p>
            </div>

        </div>
        <div class="messanger__list" v-if="isSeller">
            <div class="messanger__item" v-for="chat in chats" :key="chat.id">
                <div class="name d-flex align-items-center">
                    <h3 class="mb-0">{{ chat.buyer.user.email }}</h3>
                    <small>{{ getLastMessageTime(chat) }}</small>
                </div>
                <p class="mb-0">{{ getLastMessageText(chat) }}</p>
            </div>

        </div>

        <div class="messanger__chatbox">
            <h3>{{ newName }}</h3>
            <div class="message__form" ref="messageContainer" v-if="!isSeller">
                <div v-for="message in messages" :key="message.time"
                    :class="{ message__to: !message.from_seller, message__from: message.from_seller }">
                    <div>
                        <div class="message">
                            <div class="message__item" :class="{ to: !message.from_seller, from: message.from_seller }">
                                <p class="mb-0">{{ message.text }}</p>
                            </div>
                            <div class="time"
                                :class="{ 'text-right': !message.from_seller, 'text-left': message.from_seller }">
                                <small>{{ formatTime(message.time) }}</small>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="message__form" ref="messageContainer" v-if="isSeller">
                <div v-for="message in messages" :key="message.time"
                    :class="{ message__to: message.from_seller, message__from: !message.from_seller }">
                    <div>
                        <div class="message">
                            <div class="message__item" :class="{ to: message.from_seller, from: !message.from_seller }">
                                <p class="mb-0">{{ message.text }}</p>
                            </div>
                            <div class="time"
                                :class="{ 'text-right': message.from_seller, 'text-left': !message.from_seller }">
                                <small>{{ formatTime(message.time) }}</small>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="messanger__send d-flex">
                <input type="text" class="w-100" v-model="textMessage" placeholder="Введите сообщение..."
                    @keydown.enter.prevent="sendMsg">
                <button @click="sendMsg"><img src="@/assets/img/send.svg" alt=""></button>
            </div>

        </div>
    </div>
</template>
<script>
import global from '~/mixins/global';
import axios from 'axios';
export default {
    mixins: [global],
    props: {
        chatId: Number,
        name: String,
    },
    data() {
        return {
            message: 'Lorem Ipsum Lorem Ipsum Lorem Ipsum Lorem Ipsum Ipsum Ipsum Ipsum Ipsum',
            textMessage: '',
            textMessage2: '',
            newName: this.name,
            newId: this.chatId,
            messages: [],
            isTest: false,
            pathUrl: 'https://visuality.kz',
            msg: [],
            socket: null,
            isSeller: false,
            chats: [],
        }
    },
    computed: {
        lastMessage() {
            // Возвращает последнее сообщение из массива messages, если он не пустой
            return this.chat.messages.length > 0 ? this.chat.messages[this.chat.messages.length - 1] : null;
        },
    },
    methods: {
        getLastMessageTime(chat) {
            // Возвращает отформатированное время последнего сообщения в чате
            const lastMessage = chat.messages && chat.messages.length > 0
                ? chat.messages[chat.messages.length - 1]
                : null;

            return lastMessage && lastMessage.date_time
                ? this.time(lastMessage.date_time)
                : '';
        },
        getLastMessageText(chat) {
            // Возвращает текст последнего сообщения в чате
            const lastMessage = chat.messages && chat.messages.length > 0
                ? chat.messages[chat.messages.length - 1]
                : null;

            return lastMessage && lastMessage.text ? lastMessage.text : '';
        },
        formatTime(timeString) {
            const timeArray = timeString.split(':');
            const hours = timeArray[0];
            const minutes = timeArray[1];
            const formattedHours = hours.padStart(2, '0');
            const formattedMinutes = minutes.padStart(2, '0');
            return `${formattedHours}:${formattedMinutes}`;
        },
        time(dateTime) {
            // Возвращает отформатированное время (часы:минуты) из переданного dateTime
            const date = new Date(dateTime);
            const hours = date.getHours().toString().padStart(2, '0');
            const minutes = date.getMinutes().toString().padStart(2, '0');
            return `${hours}:${minutes}`;
        },
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
        scrollToBottom() {
            const container = this.$refs.messageContainer;
            container.scrollTop = container.scrollHeight;
        },

        sendMsg() {
            if (this.textMessage.trim() === '') return;

            const messageObject = {
                text: this.textMessage,
                from_seller: this.isSeller,
            };

            this.socket.send(JSON.stringify(messageObject));
            this.textMessage = '';
            this.$nextTick(() => {
                this.scrollToBottom();
            });

        },
        onSocketOpen(event) {
            console.log('WebSocket connection opened:', event);

        },
        onSocketMessage(event) {
            console.log(event.data);
            const msg = JSON.parse(event.data);
            if (msg.type === 'chat.message' && msg.messages) {
                // Добавляем только последнее сообщение
                const lastMessage = msg.messages[msg.messages.length - 1];
                this.messages.push(lastMessage);
                this.getChats();
            } else {
                // Обновляем массив messages, заменяя его содержимое на новое
                this.messages = msg;
                this.getChats();
            }

            this.$nextTick(() => {
                this.scrollToBottom();
            });
        },
        onSocketClose(event) {
            console.log('WebSocket connection closed:', event);
        },
        onSocketError(event) {
            console.error('WebSocket error:', event);
        },
        newChat(id, name) {
            this.newId = id
            this.newName = name
            this.socket.close();

            this.startChat()
        },
        startChat() {
            this.socket = new WebSocket(`wss://visuality.kz/ws/messanger/open-chat/${this.newId}`);
            this.socket.addEventListener('open', this.onSocketOpen);
            this.socket.addEventListener('message', this.onSocketMessage);
            this.socket.addEventListener('close', this.onSocketClose);
            this.socket.addEventListener('error', this.onSocketError);

        }
    },

    mounted() {
        this.getChats()
    },
    created() {
        this.startChat()


        const accType = localStorage.getItem('accountType')
        // console.log(accType)
        if (accType == 'buyer-account') {
            this.isSeller = false
        }
        else if (accType == 'seller-account') {
            this.isSeller = true
        }
        else {
            console.log('not authorized')
        }

    },
    beforeDestroy() {
        if (this.socket) {
            this.socket.disconnect();
        }
    },

}
</script>
<script setup>

useSeoMeta({
    title: 'Мессенджер | Design Market',
    ogTitle: 'Мессенджер  | Design Market',
    description: 'Мессенджер  | Design Market',
    ogDescription: 'Мессенджер  | Design Market',
})
</script>
<style scoped>
.toggle {
    position: relative;
    display: inline-block;
    width: 50px;
    height: 24px;
}

.toggle input {
    opacity: 0;
    width: 0;
    height: 0;
}

.slider {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #ccc;
    -webkit-transition: 0.4s;
    transition: 0.4s;
    border-radius: 34px;
}

.slider:before {
    position: absolute;
    content: '';
    height: 20px;
    width: 20px;
    left: 2px;
    bottom: 2px;
    background-color: white;
    -webkit-transition: 0.4s;
    transition: 0.4s;
    border-radius: 50%;
}

input:checked+.slider {
    background-color: #000;
}

input:focus+.slider {
    box-shadow: 0 0 1px #000;
}

input:checked+.slider:before {
    -webkit-transform: translateX(26px);
    -ms-transform: translateX(26px);
    transform: translateX(26px);
}

.message__from {
    display: flex;
}

.message__to {
    display: flex;
    justify-content: flex-end;
}

.messanger__send button {
    background: transparent;
    border-radius: 50%;
    padding: 15px;
    border: 2px solid #fff;
}

.messanger__send input {
    border-radius: 20px;
    border: 2px solid #fff;
    background: transparent;
    padding: 19px 30px;

    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: 110%;
    font-family: var(--hel);
    color: #fff;
}

.messanger__send input::placeholder {
    color: rgba(0, 0, 0, 0.60);
}

.messanger__send {
    margin-top: 10px;
    gap: 20px;
}

.message small {
    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: 130%;
    font-family: var(--hel);
    color: #fff;
}

.message__form {
    margin-top: 45px;
    height: 468px;
    overflow: auto;
}

.message__form::-webkit-scrollbar {
    width: 0px;
    /* в основном для вертикальных полос прокрутки */
    height: 0px;
    /* в основном для горизонтальных полос прокрутки */
}

.message__item p {
    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: 130%;
    font-family: var(--hel);
    color: #fff;
}

.from {
    border-radius: 30px 30px 30px 0px;
}

.to {
    border-radius: 30px 30px 0 30px;
}

.message__item {
    padding: 20px;
    border: 2px solid #fff;
    max-width: 543px;
    margin-bottom: 5px;
}

.message {
    margin-bottom: 51px;
}

.messanger__chatbox h3 {
    font-size: 32px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--hel);
    color: #fff;
}

.messanger__chatbox {
    border-radius: 15px;
    background: #3D3D3D;
    box-shadow: 0px 0px 20px 0px rgba(47, 59, 163, 0.20);
    padding: 20px 40px;
    width: 1110px;
    max-height: 100%;
}

.messanger__item p {
    margin-top: 9px;
    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    font-family: var(--hel);
    color: rgba(255, 255, 255, 0.70);
}

.name h3 {
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    font-family: var(--int);
    color: #fff;
}

.name small {
    font-family: var(--hel);
    color: #fff;
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: 130%;
}

.name {
    gap: 0 187px;
}

.messanger__item {
    border-radius: 15px;
    background: #3D3D3D;
    box-shadow: 0px 0px 20px 0px rgba(47, 59, 163, 0.20);
    padding: 20px 20px 20px 30px;
    margin-bottom: 20px;
    cursor: pointer;
}

.messanger__item:last-child {
    margin-bottom: 0;
}

.messanger {
    margin-top: 54px;
    gap: 0 40px;
    justify-content: space-between;
}

@media (max-width: 1440px) {
    .messanger__chatbox h3 {
        font-size: 20px;
    }

    .message__item p {
        font-size: 16px;
    }

    .message__item {
        padding: 10px;
    }

    .messanger__chatbox {
        padding: 20px;
    }

    .messanger {
        gap: 20px;
        margin-top: 20px;
    }

    .name h3,
    .name small {
        font-size: 18px;
    }

    .messanger__item p {
        font-size: 14px;
    }

}

@media (max-width: 1024px) {
    .messanger__list {
        display: none;
    }

    .messanger__send input {
        font-size: 16px;
        padding: 15px;
    }

    .message {
        margin-bottom: 20px;
    }
}
</style>