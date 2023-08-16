<template>
    <div class="designer">
        <div class="d-flex designer__block">
            <img src="@/assets/img/designer.png" class="img-fluid" alt="">
            <div>
                <h1>Заработай на своих навыках в дизайне</h1>

                <p>Design Market - это современная платформа, которая дает дизайнерам возможность превратить свои работы в
                    пассивный заработок</p>

                <ol>
                    <li>Зарегистрируйся как продавец на Design Market</li>
                    <li>Создай шаблон дизайна сайта</li>
                    <li>Используй в своей работе Auto Layout, компоненты, и другие возможности Figma, упрощающие дальнейшую
                        работу с шаблоном</li>
                    <li>“Почисти” файл от ненужных элементов, подпиши названия страниц, секций, фреймов и других элементов
                    </li>
                    <li>В Личном кабинете нашей платформы создай новый товар</li>
                    <li>Загрузи иллюстрации своего дизайна</li>
                    <li>Напиши описание для своего товара (расскажи какой стиль используется, кому подойдет такой сайт и
                        т.д.)</li>
                    <li>Опубликуй и ожидай продаж твоих шаблонов!</li>
                </ol>

                <NuxtLink to="/register">ЗАРЕГИСТРИРОВАТЬСЯ</NuxtLink>
            </div>
        </div>
    </div>
</template>
<script setup>
useSeoMeta({
    title: 'Для дизайнера | Design Market',
    ogTitle: 'Для дизайнера | Design Market',
    description: 'Для дизайнера | Design Market',
    ogDescription: 'Для дизайнера | Design Market',
})
</script>
<style scoped>
.designer__block a {
    margin-top: 48px;
    display: inline-block;
    text-align: center;
    border-radius: 20px;
    border: 2px solid #fff;
    background: transparent;
    padding: 17px 103px;

    font-family: var(--int);
    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    color: #fff;
    max-width: 494px;
    transition: all .3s ease;
}

.designer__block a:hover {
    color: #000;
    background: #fff;
}

.designer__block p {
    margin-bottom: 20px;
}

.designer__block li {
    margin-bottom: 10px;
}

.designer__block p,
ul,
li,
ol {
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: 130%;
    font-family: var(--hel);
    color: #fff;
}

.designer__block h1 {
    font-size: 64px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    font-family: var(--int);
    color: #fff;
    margin-bottom: 48px;
}

.designer__block {
    gap: 0 40px;
}

.designer {
    padding: 140px 100px 72px;
}

@media (max-width: 1600px) {
    .designer__block h1 {
        font-size: 40px;
        margin-bottom: 20px;
    }

    .designer__block p,
    ul,
    li,
    ol {
        font-size: 18px;
    }

    .designer {
        padding: 140px 50px 72px;
    }

    .designer__block a {
        margin-top: 20px;
    }

    .designer__block img {
        max-width: 50%;
    }
}

@media (max-width: 1024px) {
    .designer {
        padding: 140px 20px 50px;
    }

    .designer__block {
        flex-direction: column;
        gap: 10px;
    }

    .designer__block h1 {
        font-size: 24px;
    }

    .designer__block img {
        max-width: 100%;
    }

    .designer__block p,
    ul,
    li,
    ol {
        font-size: 16px;
    }

    .designer__block a {
        max-width: 100%;
        width: 100%;
        padding: 10px 0;
        text-align: center;
        border: 2px solid #fff;
    }
}
</style>