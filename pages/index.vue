<template>
    <section>
        <div class="main">
            <swiper :slidesPerView="1" :pagination="pagination" :modules="modules" class="mySwiper" :navigation="navigation"
                ref="swiper">
                <!-- Слайды -->
                <swiper-slide>
                    <div class="d-flex mainslide">
                        <h1 class="mobtitle">Шаблон сайта о путешествиях в горы</h1>
                        <img class="img-fluid" src="@/assets/img/mainsl.png" alt="" style="">

                        <div class="ml-5">
                            <h1 class="pctitle">Шаблон сайта о путешествиях в горы</h1>

                            <div class="mainbtn">
                                <NuxtLink to="/product/10">ПОДРОБНЕЕ</NuxtLink>
                            </div>
                        </div>
                    </div>
                </swiper-slide>
                <swiper-slide>
                    <div class="d-flex mainslide">
                        <h1 class="mobtitle">Шаблон сайта магазина мебели</h1>
                        <img class="img-fluid" src="@/assets/img/mainsl2.png" alt="" style="">

                        <div class="ml-5">
                            <h1 class="pctitle">Шаблон сайта магазина мебели</h1>

                            <div class="mainbtn">
                                <NuxtLink to="/product/13">ПОДРОБНЕЕ</NuxtLink>
                            </div>
                        </div>
                    </div>
                </swiper-slide>
                <swiper-slide>
                    <div class="d-flex mainslide">
                        <h1 class="mobtitle">Шаблон сайта аренды автомобилей</h1>
                        <img class="img-fluid" src="@/assets/img/mainsl3.png" alt="" style="">

                        <div class="ml-5">
                            <h1 class="pctitle">Шаблон сайта аренды автомобилей</h1>

                            <div class="mainbtn">
                                <NuxtLink to="/product/20">ПОДРОБНЕЕ</NuxtLink>
                            </div>
                        </div>
                    </div>
                </swiper-slide>
                <swiper-slide>
                    <div class="d-flex mainslide">
                        <h1 class="mobtitle">Шаблон сайта отеля</h1>
                        <img class="img-fluid" src="@/assets/img/mainsl4.png" alt="" style="">

                        <div class="ml-5">
                            <h1 class="pctitle">Шаблон сайта отеля</h1>

                            <div class="mainbtn">
                                <NuxtLink to="/product/22   ">ПОДРОБНЕЕ</NuxtLink>
                            </div>
                        </div>
                    </div>
                </swiper-slide>


                <!-- Навигация -->
                <div class="navslider d-flex align-items-center">
                    <div class="swiper-button-prev2">
                        <img class="img-fluid" src="@/assets/img/prev.svg">
                    </div>
                    <div class="cal">
                        <div class="swiper-pagination2"></div>
                    </div>


                    <div class="swiper-button-next2">
                        <img class="img-fluid" src="@/assets/img/next.svg">
                    </div>
                </div>
            </swiper>
        </div>

        <div class="sales">
            <h1>Акции и предложения</h1>
            <swiper :slidesPerView="1" :pagination="pagination__sales" :modules="modules" class="mySwiper2">
                <swiper-slide>
                    <div class="d-flex calo">
                        <div class="bigimg">
                            <img src="@/assets/img/sale1.png" alt="">
                            <div class="smallhover">
                                <div class="hvrbtn">
                                    <NuxtLink to="/product/21" class="lala">ПОДРОБНЕЕ</NuxtLink>
                                </div>
                            </div>
                            <div class="sale2">
                                <span>- 10%</span>
                            </div>
                        </div>
                        <div class="smallimgs">
                            <div class="smallsh">
                                <img src="@/assets/img/sale2.png" alt="">
                                <div class="smallhover">
                                    <div class="hvrbtn">
                                        <NuxtLink to="/product/27">ПОДРОБНЕЕ</NuxtLink>
                                    </div>
                                </div>
                                <div class="sale">
                                    <span>- 10%</span>
                                </div>
                            </div>
                            <div class="smallsh">
                                <img src="@/assets/img/sale3.png" alt="">
                                <div class="smallhover">
                                    <div class="hvrbtn">
                                        <NuxtLink to="/product/19">ПОДРОБНЕЕ</NuxtLink>
                                    </div>

                                </div>
                                <div class="sale">
                                    <span>- 5%</span>
                                </div>
                            </div>
                            <div class="smallsh">
                                <img src="@/assets/img/sale4.png" alt="">
                                <div class="smallhover">
                                    <div class="hvrbtn">
                                        <NuxtLink to="/product/25">ПОДРОБНЕЕ</NuxtLink>
                                    </div>

                                </div>
                                <div class="sale">
                                    <span>- 5%</span>
                                </div>
                            </div>
                            <div class="smallsh">
                                <img src="@/assets/img/sale5.png" alt="">
                                <div class="smallhover">
                                    <div class="hvrbtn">
                                        <NuxtLink to="/product/16">ПОДРОБНЕЕ</NuxtLink>
                                    </div>

                                </div>
                                <div class="sale">
                                    <span>- 10%</span>
                                </div>
                            </div>

                        </div>
                    </div>
                </swiper-slide>
                <swiper-slide>
                    <div class="d-flex calo">
                        <div class="bigimg">
                            <img src="@/assets/img/sale6.png" alt="">
                            <div class="smallhover">
                                <div class="hvrbtn">
                                    <NuxtLink to="/product/12" class="lala">ПОДРОБНЕЕ</NuxtLink>
                                </div>
                            </div>
                            <div class="sale2">
                                <span>- 1%</span>
                            </div>
                        </div>
                        <div class="smallimgs">
                            <div class="smallsh">
                                <img src="@/assets/img/sale7.png" alt="">
                                <div class="smallhover">
                                    <div class="hvrbtn">
                                        <NuxtLink to="/product/24">ПОДРОБНЕЕ</NuxtLink>
                                    </div>
                                </div>
                                <div class="sale">
                                    <span>- 10%</span>
                                </div>
                            </div>
                            <div class="smallsh">
                                <img src="@/assets/img/sale8.png" alt="">
                                <div class="smallhover">
                                    <div class="hvrbtn">
                                        <NuxtLink to="/product/26">ПОДРОБНЕЕ</NuxtLink>
                                    </div>

                                </div>
                                <div class="sale">
                                    <span>- 5%</span>
                                </div>
                            </div>
                            <div class="smallsh">
                                <img src="@/assets/img/sale9.png" alt="">
                                <div class="smallhover">
                                    <div class="hvrbtn">
                                        <NuxtLink to="/product/18">ПОДРОБНЕЕ</NuxtLink>
                                    </div>

                                </div>
                                <div class="sale">
                                    <span>- 10%</span>
                                </div>
                            </div>
                            <div class="smallsh">
                                <img src="@/assets/img/sale10.png" alt="">
                                <div class="smallhover">
                                    <div class="hvrbtn">
                                        <NuxtLink to="/product/14">ПОДРОБНЕЕ</NuxtLink>
                                    </div>

                                </div>
                                <div class="sale">
                                    <span>- 10%</span>
                                </div>
                            </div>

                        </div>
                    </div>
                </swiper-slide>
                <swiper-slide>
                    <div class="d-flex calo">
                        <div class="bigimg">
                            <img src="@/assets/img/sale11.png" alt="">
                            <div class="smallhover">
                                <div class="hvrbtn">
                                    <NuxtLink to="/product/17" class="lala">ПОДРОБНЕЕ</NuxtLink>
                                </div>
                            </div>
                            <div class="sale2">
                                <span>- 5%</span>
                            </div>
                        </div>
                        <div class="smallimgs">
                            <div class="smallsh">
                                <img src="@/assets/img/sale12.png" alt="">
                                <div class="smallhover">
                                    <div class="hvrbtn">
                                        <NuxtLink to="/product/8">ПОДРОБНЕЕ</NuxtLink>
                                    </div>
                                </div>
                                <div class="sale">
                                    <span>- 5%</span>
                                </div>
                            </div>
                            <div class="smallsh">
                                <img src="@/assets/img/sale13.png" alt="">
                                <div class="smallhover">
                                    <div class="hvrbtn">
                                        <NuxtLink to="/product/9">ПОДРОБНЕЕ</NuxtLink>
                                    </div>

                                </div>
                                <div class="sale">
                                    <span>- 10%</span>
                                </div>
                            </div>
                            <div class="smallsh">
                                <img src="@/assets/img/sale14.png" alt="">
                                <div class="smallhover">
                                    <div class="hvrbtn">
                                        <NuxtLink to="/product/3">ПОДРОБНЕЕ</NuxtLink>
                                    </div>

                                </div>
                                <div class="sale">
                                    <span>- 5%</span>
                                </div>
                            </div>
                            <div class="smallsh">
                                <img src="@/assets/img/sale15.png" alt="">
                                <div class="smallhover">
                                    <div class="hvrbtn">
                                        <NuxtLink to="/product/7">ПОДРОБНЕЕ</NuxtLink>
                                    </div>

                                </div>
                                <div class="sale">
                                    <span>- 5%</span>
                                </div>
                            </div>

                        </div>
                    </div>
                </swiper-slide>



            </swiper>
            <div class="sales-pagination"></div>
        </div>

        <div class="categories">
            <h1>категории</h1>

            <div class="category d-flex">
                <div class="cats">
                    <NuxtLink to="/catalog?category=4" class="cat d-flex align-items-center">

                        <img class="img-fluid" src="@/assets/img/cat1.svg" alt="">
                        <span>ОДЕЖДА И МОДА</span>
                    </NuxtLink>
                    <NuxtLink to="/catalog?category=8" class="cat d-flex align-items-center">
                        <img class="img-fluid" src="@/assets/img/cat2.svg" alt="">
                        <span>ПРЕДПРИЯТИЯ</span>
                    </NuxtLink>
                </div>
                <NuxtLink to="/catalog?category=2" class="bigcat">
                    <div class="text-center">
                        <img class="img-fluid" src="@/assets/img/cat3.svg" alt="">
                        <span class="d-block">ЕДА И РЕСТОРАНЫ</span>
                    </div>
                </NuxtLink>
                <div class="cats">
                    <NuxtLink to="/catalog?category=3" class="cat d-flex align-items-center">

                        <img class="img-fluid" src="@/assets/img/cat4.svg" alt="">
                        <span>ОБРАЗОВАНИЕ</span>
                    </NuxtLink>
                    <NuxtLink to="/catalog?category=6" class="cat d-flex align-items-center">
                        <img class="img-fluid" src="@/assets/img/cat5.svg" alt="">
                        <span>ПУТЕШЕСТВИЯ</span>
                    </NuxtLink>
                </div>
                <NuxtLink to="/catalog?category=1" class="bigcat mr-0">
                    <div class="text-center">
                        <img class="img-fluid" src="@/assets/img/cat6.svg" alt="">
                        <span class="d-block">РАЗВЛЕЧЕНИЯ</span>
                    </div>
                </NuxtLink>
            </div>

            <div class="category d-flex" style="margin-top: 17px;">
                <NuxtLink to="/catalog?category=12" class="bigcat ml-0">
                    <div class="text-center">
                        <img class="img-fluid" src="@/assets/img/cat7.svg" alt="">
                        <span class="d-block">ТЕХНИКА</span>
                    </div>
                </NuxtLink>
                <div class="cats">
                    <NuxtLink to="/catalog?category=5" class="cat d-flex align-items-center">

                        <img class="img-fluid" src="@/assets/img/cat8.svg" alt="">
                        <span>АВТОМОБИЛИ</span>
                    </NuxtLink>
                    <NuxtLink to="/catalog?category=10" class="cat d-flex align-items-center">
                        <img class="img-fluid" src="@/assets/img/cat9.svg" alt="">
                        <span>МЕДИЦИНА</span>
                    </NuxtLink>
                </div>
                <NuxtLink to="/catalog?category=9" class="bigcat">
                    <div class="text-center">
                        <img class="img-fluid" src="@/assets/img/cat10.svg" alt="">
                        <span class="d-block">НЕДВИЖИМОСТЬ</span>
                    </div>
                </NuxtLink>
                <div class="cats">
                    <NuxtLink to="/catalog?category=7" class="cat d-flex align-items-center">

                        <img class="img-fluid" src="@/assets/img/cat11.svg" alt="">
                        <span>ПИТОМЦЫ</span>
                    </NuxtLink>
                    <NuxtLink to="/catalog?category=11" class="cat d-flex align-items-center">
                        <img class="img-fluid" src="@/assets/img/cat12.svg" alt="">
                        <span>ФИНАНСЫ</span>
                    </NuxtLink>
                </div>

            </div>

            <div class="mobcategory">
                <NuxtLink to="/catalog?category=4" class="mobcategory__item">
                    <div class="text-center">
                        <img src="@/assets/img/cat1.svg" alt="">
                        <span>ОДЕЖДА И МОДА</span>
                    </div>
                </NuxtLink>
                <NuxtLink to="/catalog?category=8" class="mobcategory__item">
                    <div class="text-center">
                        <img src="@/assets/img/cat2.svg" alt="">
                        <span>ПРЕДПРИЯТИЯ</span>
                    </div>
                </NuxtLink>
                <NuxtLink to="/catalog?category=2" class="mobcategory__item">
                    <div class="text-center">
                        <img src="@/assets/img/cat3.svg" alt="">
                        <span>ЕДА И РЕСТОРАНЫ</span>
                    </div>
                </NuxtLink>
                <NuxtLink to="/catalog?category=3" class="mobcategory__item">
                    <div class="text-center">
                        <img src="@/assets/img/cat4.svg" alt="">
                        <span>ОБРАЗОВАНИЕ</span>
                    </div>
                </NuxtLink>
                <NuxtLink to="/catalog?category=6" class="mobcategory__item">
                    <div class="text-center">
                        <img src="@/assets/img/cat5.svg" alt="">
                        <span>ПУТЕШЕСТВИЯ</span>
                    </div>
                </NuxtLink>
                <NuxtLink to="/catalog?category=1" class="mobcategory__item">
                    <div class="text-center">
                        <img src="@/assets/img/cat6.svg" alt="">
                        <span>РАЗВЛЕЧЕНИЯ</span>
                    </div>
                </NuxtLink>
                <NuxtLink to="/catalog?category=12" class="mobcategory__item">
                    <div class="text-center">
                        <img src="@/assets/img/cat7.svg" alt="">
                        <span>ТЕХНИКА</span>
                    </div>
                </NuxtLink>
                <NuxtLink to="/catalog?category=5" class="mobcategory__item">
                    <div class="text-center">
                        <img src="@/assets/img/cat8.svg" alt="">
                        <span>АВТОМОБИЛИ</span>
                    </div>
                </NuxtLink>
                <NuxtLink to="/catalog?category=10" class="mobcategory__item">
                    <div class="text-center">
                        <img src="@/assets/img/cat9.svg" alt="">
                        <span>МЕДИЦИНА</span>
                    </div>
                </NuxtLink>
                <NuxtLink to="/catalog?category=9" class="mobcategory__item">
                    <div class="text-center">
                        <img src="@/assets/img/cat10.svg" alt="">
                        <span>НЕДВИЖИМОСТЬ</span>
                    </div>
                </NuxtLink>
                <NuxtLink to="/catalog?category=7" class="mobcategory__item">
                    <div class="text-center">
                        <img src="@/assets/img/cat11.svg" alt="">
                        <span>ПИТОМЦЫ</span>
                    </div>
                </NuxtLink>
                <NuxtLink to="/catalog?category=11" class="mobcategory__item">
                    <div class="text-center">
                        <img src="@/assets/img/cat12.svg" alt="">
                        <span>ФИНАНСЫ</span>
                    </div>
                </NuxtLink>
            </div>
        </div>

        <div class="textblock d-flex">
            <img src="@/assets/img/textblock.png" class="img-fluid" alt="">

            <div>
                <h2>Зарабатывайте с помощью своих дизайнов на нашей платформе</h2>
                <p>Design Market поможет дизайнерам успешно монетизировать свои уникальные работы.
                </p>
                <p> Простой интерфейс позволит быстро загрузить работу и начать продажи шаблонов дизайна, не прерывая
                    творческий процесс.</p>

                <button onclick="window.location.href = '/for-designer'">узнать подробнее</button>
            </div>
        </div>

        <div class="popular">
            <h1>популярные дизайны</h1>
            <div v-if="populars.length <= 0"></div>
            <div class="popular__block d-flex" v-else>
                <NuxtLink class="popular__item" v-for="popular in populars" :key="popular.id"
                    :to="'/product/' + popular.id">
                    <div class="w-100 text-center pop__type">
                        <h3>{{ popular.name }}</h3>
                    </div>
                    <img :src="popular.main_image" alt="" class="img-fluid">

                    <div class="price">
                        {{ popular.price.toLocaleString() }} ₸
                    </div>
                </NuxtLink>

            </div>

            <div class="text-center linktoshop">
                <NuxtLink to="/catalog">Посмотреть все</NuxtLink>
            </div>
        </div>


    </section>
</template>
  
<script>
import { Navigation, Pagination } from 'swiper/modules';
import { Swiper, SwiperSlide } from 'swiper/vue';
import 'swiper/css';
import 'swiper/css/pagination';
import 'swiper/css/navigation';
import axios from 'axios'
export default {
    components: {
        Swiper,
        SwiperSlide,
    },
    data() {
        return {
            modules: [Navigation, Pagination],
            navigation: {
                prevEl: '.swiper-button-prev2',
                nextEl: '.swiper-button-next2',
            },
            pagination: {
                el: '.swiper-pagination2',
                type: 'fraction',

            },
            pagination__sales: {
                el: '.sales-pagination',
                clickable: true,
            },
            currentSlideIndex: 0,
            totalSlides: 0,
            pathUrl: 'https://visuality.kz',
            populars: [],
        };
    },
    methods: {
        getPopular() {
            const path = `${this.pathUrl}/api/products/popular-product?amount_products=6`
            axios
                .get(path)
                .then(response => {
                    this.populars = response.data
                })
                .catch(error => {
                    console.log(error)
                })
        }
    },
    mounted() {
        this.getPopular()
    }


};
</script>
<script setup>
useSeoMeta({
    title: 'Главная | Design Market',
    ogTitle: 'Главная | Design Market',
    description: 'Главная | Design Market',
    ogDescription: 'Главная | Design Market',
})
</script>
<style>
.bigimg {
    max-width: 855px;
    max-height: 480px;
}

.smallsh {
    max-width: 412px;
    max-height: 230px;
}

.mobcategory {
    display: none;
}

.mobtitle {
    display: none;
}

.lala {
    border-radius: 20px !important;
    border: 3px solid #FFF !important;
    padding: 16px 30px !important;

    font-size: 24px !important;
}

.linktoshop {
    margin-top: 48px;
}

.linktoshop a {
    border-radius: 20px;
    border: 3px solid #fff;
    background: transparent;
    padding: 15px 32px;

    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;

    transition: all .3s ease;
}

.linktoshop a:hover {
    color: #000;
    background: #fff;
}

.popular__item {
    position: relative;
    border-radius: 20px;
    height: fit-content;
    background: #fff;
    overflow: hidden;
}

.popular__item img {
    width: 43.75vw;
    height: 20.625vw;
    object-fit: cover;
    object-position: top;
    max-height: 396px;
}

.price {
    position: absolute;
    right: 0;
    bottom: 0;
    border-radius: 20px 0px;
    background: linear-gradient(90deg, #8D70DA 0%, #F66A4F 100%);
    padding: 9px 20px;

    color: #fff;
    font-size: 36px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    font-family: var(--int);
}

.pop__type {
    border-radius: 20px 20px 0px 0px;
    background: #3D3D3D;
    padding: 15px 0;
}

.pop__type h3 {
    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
    margin: 0;
}

a {
    text-decoration: none !important;
}

.popular__block {
    flex-wrap: wrap;
    gap: 40px;
}

.popular {
    margin-top: 72px;
    margin-bottom: 72px;
}

.popular h1 {
    font-size: 48px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
    margin-bottom: 48px;
}

.textblock button {
    margin-top: 58px;
    border-radius: 20px;
    border: 3px solid #fff;
    background: transparent;
    padding: 15px 57px;

    color: #fff;
    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    transition: all .3s ease;
}

.textblock button:hover {
    color: #000;
    background: #fff;
}

.textblock h2 {
    font-size: 48px;
    font-style: normal;
    font-weight: 300;
    line-height: 130%;
    color: #fff;
    font-family: var(--int);
    margin-bottom: 48px;
}

.textblock p {
    font-size: 24px;
    font-style: normal;
    font-weight: 300;
    line-height: 130%;
    font-family: var(--hel);
    color: #fff;
}

.textblock p:first-of-type {
    margin-bottom: 30px;
}

.textblock {
    gap: 0 40px;
}

.textblock {
    margin-top: 72px;
}

.bigcat {
    width: 21.615vw;
    height: 16.51vw;
    padding: 40px 55px 52px;
    border-radius: 20px;
    background: #3D3D3D;
    margin: 0 20px;
    transition: all .3s ease;
}

.bigcat:hover {
    box-shadow: 0px 0px 15px 0px rgba(246, 106, 79, 0.20);
}


.bigcat span {
    white-space: nowrap;
    margin-top: 30px;
}

svg {
    display: inline-block;
}

stop {
    transition: .4s ease;
}

/* Use the colors to transition to */
svg:hover stop:first-child {
    stop-color: #1EBEE0;
}

svg:hover stop:last-child {
    stop-color: #952491;
}

.bigcat,
.cat {
    cursor: pointer;
}

.cat span,
.bigcat span {
    font-size: 1.25vw;
    font-style: normal;
    font-weight: 300;
    line-height: 140%;
    font-family: var(--int);
    color: #fff;
}

.cat {
    gap: 20px;
    border-radius: 20px;
    background: #3D3D3D;
    padding: 30px 25px 30px 35px;
    width: 21.615vw;
    height: 7.813vw;
    transition: all .3s ease;
}

.cat:hover {
    box-shadow: 0px 0px 15px 0px rgba(246, 106, 79, 0.20);
}

.cats .cat:nth-child(1) {
    margin-bottom: 17px;
}

.categories h1 {
    font-size: 48px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
    margin-bottom: 48px;
}



.sale span {
    font-size: 32px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    color: #fff;
    font-family: var(--int);
    padding: 7px 20px;
    display: block;
}


.sale {
    border-radius: 0px 20px;
    background: linear-gradient(90deg, #8D70DA 0%, #F66A4F 100%);
    position: absolute;
    left: 0;
    bottom: 0;
}

.sale2 span {
    font-size: 36px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    color: #fff;
    font-family: var(--int);
    padding: 7px 22px;
    display: block;
}

.sale2 {
    border-radius: 0px 20px;
    background: linear-gradient(90deg, #8D70DA 0%, #F66A4F 100%);
    position: absolute;
    left: 0;
    bottom: 0;
}

.hvrbtn {
    display: flex;
    justify-content: center;
    align-items: center;
}

.smallhover {
    display: flex;
    justify-content: center;
    align-items: center;

    position: absolute;
    opacity: 0;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border-radius: 20px;
    backdrop-filter: blur(3.5px);
    transition: all .3s ease;
    overflow: hidden;
}

.smallhover::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.3);
    /* Цвет и прозрачность */
    z-index: -1;
    pointer-events: none;
    /* Чтобы псевдоэлемент не мешал взаимодействию с содержимым */
    transition: all .3s ease;
}

.smallhover a {
    border-radius: 20px;
    border: 3px solid #FFF;
    padding: 16px 30px;
    background: transparent;
    display: inline-block;
    font-size: 20px;

    color: #fff;
    font-family: var(--int);
    text-transform: uppercase;
    transition: all .3s ease;
}

.smallhover a:hover {
    color: #000;
    background: #fff;
}

.smallsh {
    position: relative;
    border-radius: 50px;

}

.bigimg {
    position: relative;
    border-radius: 50px;
}


.smallsh:hover .smallhover,
.bigimg:hover .smallhover {
    opacity: 1;
}

.swiper-pagination-bullet {
    opacity: 1;
    transition: all .3s ease;
    width: 29px;
    border-radius: 20px;
    background: #fff;
}

.swiper-pagination-bullet-active {
    background: linear-gradient(to right, #8D70DA, #F66A4F);
    width: 80px;
    border-radius: 20px;
    transition: all .3s ease;
}

.sales-pagination {
    position: absolute;
    right: 0;
    left: auto !important;
    z-index: 100;
    width: auto !important;
}

.smallimgs {
    display: flex;
    flex-wrap: wrap;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
    grid-template-areas: ". .";
    margin-left: 20px;
}

.sales h1 {
    font-size: 48px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    margin-bottom: 47px;
}

.sales {
    margin-top: 47px;
    padding-bottom: 40px;
    position: relative;
}

.cal {
    margin: 0 20px;
}

.main a {
    margin-top: 20px;
    border-radius: 20px;
    border: 3px solid #fff;
    background: transparent;
    padding: 18px 68px;
    display: inline-block;

    color: #fff;
    font-size: 32px;
    font-style: normal;
    font-weight: 300;
    line-height: 110%;
    font-family: var(--int);
    transition: all .3s ease;
    text-decoration: none;
}

.main a:hover {
    color: #000;
    background: #fff;
}

.mySwiper,
.mySwiper2 {
    position: relative;
}

.main h1 {
    font-family: var(--int);
    font-size: 64px;
    font-style: normal;
    font-weight: 400;
    line-height: 110%;
    color: #fff;
    max-width: 642px;
}

.counter {
    width: 100px;
}

.navslider {
    position: absolute;
    bottom: 0;
    right: 0;
    z-index: 100;
}

.navslider img {
    cursor: pointer;
}

.swiper-pagination2 {
    font-family: var(--int);
    font-size: 40px;
    font-style: normal;
    font-weight: 400;
    line-height: 110%;
    color: #fff;

}

section {
    padding: 120px 100px 0;
}

.main img {
    max-width: 1060px;
    max-height: 614px;
}

@media (max-width: 1800px) {
    .main img {
        max-width: 861px;
    }

    .main h1 {
        font-size: 65px;
    }

    .calo {
        flex-direction: column;
        justify-content: center;
        align-items: center;
        gap: 20px;
    }

    .smallimgs {
        margin-left: 0;
        justify-content: center;
    }

}

@media (max-width: 1670px) {
    .navslider {
        right: 0;
    }

    section {
        padding: 140px 80px 0;
    }

    .popular__item {
        width: 100%;
    }

    .popular__item img {
        width: 100%;
    }

    .sales h1,
    .categories h1,
    .popular h1 {
        font-size: 45px;
    }

    .bigcat img {
        max-width: 50%;
    }

    .bigcat {
        padding: 30px 31px 10px
    }

    .cat {
        padding: 15px 12px 15px 15px;
    }

    .cat img {
        max-width: 30%;
    }

    .cat span,
    .bigcat span {
        font-size: 16px;
    }

    .textblock img {
        max-width: 50%;
    }

    .popular__block {
        justify-content: center;
    }

    .textblock {
        /* align-items: flex-start; */
    }

    .textblock p:first-of-type {
        margin-bottom: 20px;
    }

    .textblock h2 {
        font-size: 35px;
        margin-bottom: 20px;
        line-height: 110%;
    }

    .textblock p {
        font-size: 18px;
    }

    .textblock button {
        margin-top: 20px;
    }

    .textblock button {
        font-size: 18px;
    }

    .linktoshop a {
        font-size: 18px;
    }
}

@media (max-width: 1500px) {
    .main img {
        max-width: 761px;
    }

    .main h1 {
        font-size: 55px;
    }

    .main a {
        font-size: 20px;
    }
}

@media (max-width: 1300px) {
    .main img {
        max-width: 561px;
    }

    .main h1 {
        font-size: 35px;
    }

    .swiper-pagination2 {
        font-size: 20px;
    }

    .main a {
        padding: 18px 50px;
    }
}

@media (max-width: 1025px) {
    section {
        padding: 140px 30px 0;
    }
}

@media (max-width: 1024px) {
    .popular__item img {
        width: 100%;
    }

    .mobtitle {
        display: block;
    }

    .pctitle {
        display: none;
    }

    .mainslide {
        flex-direction: column;

    }

    .main img {
        max-width: 100%;
    }

    .mainbtn {
        text-align: left;
    }

    .main h1 {
        font-size: 24px;
    }

    .main a {
        font-size: 16px;
        padding: 13px 48px;
    }

    .navslider {
        margin-top: 20px;
    }

    .swiper-button-prev2 img,
    .swiper-button-next2 img {
        max-width: 75%;
    }

    .swiper-pagination2 {
        margin-right: 5%;
    }

    .cal {
        margin: 0 15px 0 0;
    }

    .swiper-pagination2 {
        font-size: 22px;
    }

    .mainslide div {
        margin-left: 0 !important;
    }

    .sales {
        margin-top: 20px;
    }

    .sales h1,
    .categories h1,
    .popular h1 {
        font-size: 20px;
        margin-bottom: 10px;
    }

    .bigimg img {
        max-width: 100%;
    }

    .smallsh img {
        max-width: 100%;
    }

    .smallsh {
        max-width: 48%;
    }

    .calo {
        gap: 25px;
    }

    .smallimgs {
        justify-content: left;
        gap: 25px;
    }

    section {
        padding: 140px 20px 0;
    }

    .category {
        display: none !important;
    }

    .mobcategory {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        gap: 20px;
    }

    .mobcategory__item {
        border-radius: 20px;
        /* border: 2px solid #fff; */
        background: #3D3D3D;
        padding: 17px 20px;
        width: 40%;
    }

    .mobcategory__item img {
        width: 90px;
        height: 90px;
    }

    .mobcategory__item span {
        display: block;
        font-size: 14px;
        font-style: normal;
        font-weight: 300;
        line-height: 130%;
        font-family: var(--hel);
        color: #fff;
        margin-top: 14px;
    }

    .textblock {
        flex-direction: column;
        margin-top: 20px;
    }

    .textblock img {
        max-width: 100%;
    }

    .textblock h2 {
        font-size: 20px;
        margin: 10px 0;
    }

    .textblock p {
        font-size: 16px;
        margin: 0;
    }

    .textblock p:first-of-type {
        margin-bottom: 10px;
    }

    .textblock button {
        width: 100%;
        padding: 16px 0;
        font-size: 20px;
    }

    .popular {
        margin-top: 30px;
        margin-bottom: 50px;
    }

    .pop__type h3 {
        font-size: 12px;
    }

    .pop__type {
        padding: 5px 0;
    }

    .price {
        border-radius: 15px 0px;
        font-size: 16.667px;
        padding: 3px 10px;
    }

    .linktoshop a {
        width: 100%;
        padding: 15px 0;
        font-size: 20px;
        display: block;
    }

    .linktoshop {
        margin-top: 30px;
    }

    .popular__item {
        width: 100%;
    }

    .sale,
    .sale2 {
        border-radius: 0 15px;
    }
}

@media (max-width: 500px) {
    .mobcategory__item {
        max-width: 165px;
        width: 165px;
    }

    .mobcategory__item span {
        font-size: 15px;
    }

    .main a {
        font-size: 16px;
        padding: 13px 30px;
        max-width: 196px;
    }

    .navslider {
        margin-top: 20px;
        max-width: 134px;
    }

    .swiper-button-prev2,
    .swiper-button-next2 {
        width: 53px;
        height: 47px;
    }

    .swiper-button-prev2 img,
    .swiper-button-next2 img {
        max-width: 100%;
        height: 100%;
    }

    .swiper-pagination2 {
        font-size: 16px;
        white-space: nowrap;
    }

    .cal {
        margin: 0 10px;
    }

    .smallhover {
        border-radius: 20px;
        opacity: 1;
        backdrop-filter: none;
    }

    .smallsh {
        max-width: 45%;
    }

    .smallhover a {
        font-size: 16px;
        display: none;

    }

    .sale span,
    .sale2 span {
        padding: 3px 11px;
        font-size: 16px;
    }

    .smallsh img {
        max-width: 172px;
    }

    .smallimgs {
        gap: 10px 20px;
    }

    .calo {
        gap: 10px 20px;
    }

    .main a {
        border: 2px solid #fff;
    }
}

@media (max-width: 389px) {
    .smallsh img {
        max-width: 162px;
    }

    .mobcategory__item span {
        font-size: 14px;
    }

    .mobcategory__item {
        max-width: 156px;
    }
}

@media (max-width: 374px) {
    .mobcategory__item {
        width: 100%;
        max-width: 100%;
    }
}
</style>