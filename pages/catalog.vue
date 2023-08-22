<template>
    <div class="catalog">
        <div class="input-group mb-3">
            <div class="input-group-prepend">
                <span class="input-group-text" id="basic-addon1"><img src="@/assets/img/lupa.svg" alt=""></span>
            </div>
            <input v-model="searchQuery" type="text" class="form-control" ref="searchInput" placeholder="ПОИСК"
                aria-label="Поиск" aria-describedby="basic-addon1" @input="searchProducts">
        </div>
        <h1>каталог дизайна</h1>

        <div class="catalog__filters d-flex justify-content-end">
            <div class="sort">
                <button class="d-flex align-items-center" @click="sort = !sort">
                    <span>СОРТИРОВКА</span>
                    <img src="@/assets/img/filterarrow.svg" class="arrow" alt="">
                </button>

                <div class="sort__body" :class="{ filters__active: sort }">
                    <div class="text-center">
                        <small @click="sortBy('price')">СНАЧАЛА ДЕШЕВЛЕ</small>
                        <hr>
                        <small @click="sortBy('-price')">СНАЧАЛА ДОРОЖЕ</small>
                        <hr>
                        <small @click="sortBy('-date_activate')">САМЫЕ НОВЫЕ</small>
                        <hr>
                        <small @click="sortBy('date_activate')">САМЫЕ СТАРЫЕ</small>

                    </div>
                </div>
            </div>
            <div class="filter">
                <button class="d-flex align-items-center" @click="filter = !filter">
                    <span>ФИЛЬТРЫ</span>
                    <img src="@/assets/img/filterarrow.svg" class="arrow" alt="">
                </button>

                <div class="filter__body" :class="[{ 'filters__active': filter }]">
                    <h3>Цена</h3>
                    <div class="price__filter d-flex align-items-center">
                        <input type="number" v-model="minPrice" placeholder="От" @input="applyFilters">
                        <img src="@/assets/img/filterline.svg" alt="">
                        <input type="number" v-model="maxPrice" placeholder="До" @input="applyFilters">
                    </div>

                    <h3 class="mt-4">КАТЕГОРИИ</h3>
                    <div class="d-flex filters__category">
                        <div>
                            <label class="custom-checkbox" v-for="(category, index) in categories1" :key="index">
                                <input type="checkbox" :value="index + 1" v-model="selectedCategories"
                                    @change="applyFilters">
                                <p class="checkbox-text m-0">{{ category }}</p>
                            </label>
                        </div>
                        <div>
                            <label class="custom-checkbox" v-for="(category, index) in categories2" :key="index + 6">
                                <input type="checkbox" :value="index + 9" v-model="selectedCategories"
                                    @change="applyFilters">
                                <p class="checkbox-text m-0">{{ category }}</p>
                            </label>
                        </div>
                    </div>

                </div>
            </div>
        </div>
        <div v-if="products.length <= 0"></div>
        <div class="catalog__block" v-else>

            <NuxtLink v-for="product in products.results" :key="product.id" :to="'/product/' + product.id">
                <div class="catalog__item">
                    <div class="category">
                        <div class="text-center">

                            <h2>{{ product.name }}</h2>

                        </div>
                    </div>
                    <img :src="product.main_image"
                        :class="{ notimg: product.main_image == 'https://visuality.kz/media/media/system/no_photo.jpg' }"
                        alt="">
                    <div class="price">
                        {{ (Math.floor(product.price - ((product.price * product.discount) / 100))).toLocaleString() }} ₸
                    </div>
                </div>
            </NuxtLink>
        </div>

        <div class="text-center showmore">
            <button @click="loadMoreProducts()" ref="showmore">показать еще</button>
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
            filter: false,
            sort: false,
            products: [],
            minPrice: null,
            maxPrice: null,
            pathUrl: 'https://visuality.kz',
            categories: {
                1: "Развлечения",
                2: "Еда и рестораны",
                3: "Образование",
                4: "Одежда и мода",
                5: "Автомобили",
                6: "Путешествия",
                7: "Питомцы",
                8: "Предприятия",
                9: "Недвижимость",
                10: "Медицина",
                11: "Финансы",
                12: "Техника",
                13: "Бизнес",
                14: "Продажи",
                15: "Другое",
                16: "Спорт"
            },
            categories1: ["РАЗВЛЕЧЕНИЯ", "ЕДА И РЕСТОРАНЫ", "ОБРАЗОВАНИЕ", "ОДЕЖДА И МОДА", "АВТОМОБИЛИ", "ПУТЕШЕСТВИЯ", "БИЗНЕС", "ПРОДАЖИ"],
            categories2: ["ПИТОМЦЫ", "ПРЕДПРИЯТИЯ", "НЕДВИЖИМОСТЬ", "МЕДИЦИНА", "ФИНАНСЫ", "ТЕХНИКА", "СПОРТ", "ДРУГОЕ"],
            selectedCategories: [],
            shouldFocusInput: false,
            searchQuery: ''
        }
    },
    methods: {
        getProducts() {
            const queryParams = new URLSearchParams(window.location.search);
            const categoryParam = queryParams.get('category');

            let url = `${this.pathUrl}/api/products/all-product`;
            if (categoryParam) {
                url += `?category__in=${categoryParam}`;
            }

            axios
                .get(url)
                .then(response => {
                    this.products = response.data;
                })
                .catch(error => {
                    console.error(error);
                });
        },
        loadMoreProducts() {
            if (this.products.next) {
                axios
                    .get(this.products.next)
                    .then(response => {
                        // Добавляем новые продукты к существующим
                        this.products.results.push(...response.data.results);
                        this.products.next = response.data.next;
                    })
                    .catch(error => {
                        console.error(error);
                    });
            }
            else {
                this.$refs.showmore.innerHTML = 'Больше ничего нет (;'
            }
        },
        sortBy(ordering) {
            this.sort = false
            const path = `${this.pathUrl}/api/products/all-product?ordering=${ordering}`;
            axios
                .get(path)
                .then(response => {
                    this.products = response.data;

                })
                .catch(error => {
                    console.error(error);
                });
        },

        applyFilters() {
            const params = new URLSearchParams();
            if (this.minPrice !== null) {
                params.append('price__gte', this.minPrice);
            }
            if (this.maxPrice !== null) {
                params.append('price__lte', this.maxPrice);
            }

            if (this.selectedCategories.length > 0) {
                params.append('category__in', this.selectedCategories.join(','));
            }
            if (this.searchQuery) {
                params.append('name__icontains', this.searchQuery);
            }

            this.fetchFilteredProducts(params);
        },
        fetchFilteredProducts(params) {
            const path = `${this.pathUrl}/api/products/all-product?${params.toString()}`;
            axios
                .get(path)
                .then(response => {
                    this.products = response.data;
                })
                .catch(error => {
                    console.error(error);
                });
        },
        searchProducts() {
            const query = this.searchQuery.trim();
            if (query) {
                const queryParams = `?name__icontains=${query}`;
                this.fetchSearchResults(queryParams);
            } else {
                this.getProducts();
            }
        },
        fetchSearchResults(queryParams) {
            const path = `${this.pathUrl}/api/products/all-product${queryParams}`;
            axios
                .get(path)
                .then(response => {
                    this.products = response.data;
                })
                .catch(error => {
                    console.error(error);
                });
        },
        checkQueryParam() {
            const params = new URLSearchParams(window.location.search);
            this.shouldFocusInput = params.get('search') === 'true';
        }
    },
    mounted() {
        this.checkQueryParam();
        if (this.shouldFocusInput) {
            this.$refs.searchInput.focus();
        }
    },
    created() {
        this.getProducts()
        const queryParams = new URLSearchParams(window.location.search);
        const categoryParam = queryParams.get('category');
        if (categoryParam) {
            this.selectedCategories = categoryParam.split(',').map(Number);
        }
        this.applyFilters();
    }
}
</script >
<script setup>

useSeoMeta({
    title: 'Каталог | Design Market',
    ogTitle: 'Каталог | Design Market',
    description: 'Каталог | Design Market',
    ogDescription: 'Каталог | Design Market',
})
</script>
<style scoped>
.form-control {
    border-radius: 20px;
    border: 2px solid #fff;
    background: transparent;
    border-left: 0;
    font-family: var(--int);
    color: #fff;
    padding: 20px 20px;
    padding-left: 0;
    box-shadow: none !important;
    font-size: 14px;
}

.input-group-text {
    background: transparent;

    border-radius: 30px;
    border: 2px solid #fff;
    border-right: 0;
    padding: 0 10px;
}

.form-control::placeholder {
    opacity: 0.5;
}

#loader {
    display: block;
    position: relative;
    left: 50%;
    top: 50%;
    width: 100px;
    height: 100px;
    margin: 20px 0 0 -55px;
    border-radius: 50%;
    border: 3px solid transparent;
    border-top-color: #0F172A;
    -webkit-animation: spin 2s linear infinite;
    animation: spin 2s linear infinite;
}

#loader:before {
    content: "";
    position: absolute;
    top: 5px;
    left: 5px;
    right: 5px;
    bottom: 5px;
    border-radius: 50%;
    border: 3px solid transparent;
    border-top-color: #4F46E5;
    -webkit-animation: spin 3s linear infinite;
    animation: spin 3s linear infinite;
}

#loader:after {
    content: "";
    position: absolute;
    top: 15px;
    left: 15px;
    right: 15px;
    bottom: 15px;
    border-radius: 50%;
    border: 3px solid transparent;
    border-top-color: #000;
    -webkit-animation: spin 1.5s linear infinite;
    animation: spin 1.5s linear infinite;
}

@-webkit-keyframes spin {
    0% {
        -webkit-transform: rotate(0deg);
        -ms-transform: rotate(0deg);
        transform: rotate(0deg);
    }

    100% {
        -webkit-transform: rotate(360deg);
        -ms-transform: rotate(360deg);
        transform: rotate(360deg);
    }
}

@keyframes spin {
    0% {
        -webkit-transform: rotate(0deg);
        -ms-transform: rotate(0deg);
        transform: rotate(0deg);
    }

    100% {
        -webkit-transform: rotate(360deg);
        -ms-transform: rotate(360deg);
        transform: rotate(360deg);
    }
}

hr {
    margin: 10px 0;
    border-top: 1px solid #fff;
}

.sort__body small {
    font-size: 1.042vw;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    color: #fff;
    font-family: var(--int);
    white-space: nowrap;
    cursor: pointer;
}

.sort {
    position: relative;
}

.sort__body {
    display: none;
    border-radius: 15px;
    background: #3D3D3D;
    box-shadow: 0px 0px 20px 0px rgba(47, 59, 163, 0.20);
    padding: 10px 40px;
    position: absolute;
    width: 100%;
    margin-top: 20px;
    z-index: 50;
}

.filters__category {
    gap: 0px 40px;
}

.custom-checkbox p {
    font-family: var(--hel);
    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: 110%;
    color: #fff;
    white-space: nowrap;
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

label {
    display: block;
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

    font-family: var(--hel);
    color: #fff;
    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: 110%;
    max-width: 169px;
    height: 45px;
    padding: 10px 9px;
}

.price__filter input::placeholder {
    color: rgba(255, 255, 255, 0.60);
}

.price__filter {
    gap: 13px;
}

.filter__body h3 {
    font-family: var(--int);
    color: #fff;
    font-size: 20px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    margin-bottom: 20px;
    text-transform: uppercase;
}

.products__filters button,
.products__filters a {
    padding: 17px 40px;
    background: transparent;
    border-radius: 50px;
    border: 2px solid #fff;
    transition: all .3s ease;
    cursor: pointer;

    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: 110%;
    font-family: var(--int);
    color: #000;
    transition: all .3s ease;

}

.products__filters a:hover {
    color: #fff;
    background: #000;
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
    border-radius: 15px;
    background: #3D3D3D;
    box-shadow: 0px 0px 20px 0px rgba(47, 59, 163, 0.20);
    padding: 8px 20px 20px;
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

.showmore button {
    border-radius: 20px;
    border: 2px solid #DEDEDE;
    background: transparent;
    padding: 13px 44px;
    color: #fff;

    font-size: 20px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    transition: all .3s ease;
}

.showmore button:hover {
    color: #000;
    background: #fff;
}

.showmore {
    margin-top: 40px;
}

.price {
    position: absolute;
    bottom: 0;
    right: 0;
    padding: 5px 14px;

    font-size: 26.458px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    font-family: var(--int);
}

.catalog__item {
    position: relative;
    box-shadow: 0px 0px 20px 0px rgba(47, 59, 163, 0.20);
    border-radius: 20px;
    overflow: hidden;
}

.catalog__item img {
    width: 556px;
    height: 262px;
    object-fit: cover;
    object-position: top;
    border-radius: 0;
}

.notimg {
    object-position: center !important;
}

.category h2 {
    margin: 0;
    font-size: 16px;
    font-style: normal;
    font-weight: 300;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
}

.category {
    border-radius: 15px 15px 0px 0px;
    background: #3D3D3D;
    padding: 8px 0;
    display: block !important;
}

.catalog__block {
    margin-top: 40px;
    display: flex;
    justify-content: flex-start;
    flex-wrap: wrap;
    gap: 26px;
}

.catalog__filters button {
    border-radius: 20px;
    border: 3px solid #DEDEDE;
    padding: 15px 40px;
    background: transparent;
    gap: 0 10px;

    font-size: 24px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    font-family: var(--int);
    color: #fff;
    transition: all .3s ease;
}

/* .catalog__filters button:hover {
    color: #fff;
    background: #000;
}

.catalog__filters button:hover .arrow {
    filter: grayscale(100%);
} */

.catalog__filters {
    gap: 0 20px;
}

.catalog h1 {
    font-size: 64px;
    font-style: normal;
    font-weight: 400;
    line-height: normal;
    text-transform: uppercase;
    font-family: var(--int);
    color: #fff;
    margin-bottom: 40px;
}

.catalog {
    padding: 140px 100px 72px;
}

@media (max-width: 1440px) {
    .catalog {
        padding: 140px 50px 72px;
    }

    .catalog__block {
        justify-content: center;
    }
}

@media (max-width: 1250px) {
    .catalog__block a {
        width: 100%;
    }

    .catalog__item img {
        width: 100%;
        max-height: 165px;
        border-radius: 0 0 33px 33px;
    }

    .catalog__item {
        border-radius: 33px;
    }
}

@media (max-width: 1024px) {
    .catalog h1 {
        font-size: 32px;
        margin-bottom: 20px;
    }

    .catalog {
        padding: 140px 20px 50px;
    }

    .catalog__filters {
        flex-direction: column;
        gap: 20px;
    }

    .catalog__filters button {
        width: 100%;
        justify-content: space-between;
        padding: 10px 20px;
        font-size: 20px;
    }

    .catalog__block {
        margin-top: 20px;
        gap: 20px;
    }

    .category {
        padding: 5px 0;
    }

    .category h2 {
        font-size: 12px;
        font-weight: 400;
    }

    .price {
        padding: 3px 10px;
        font-size: 16px;
    }

    .sort__body small {
        font-size: 14px;
    }

    .filter__body {
        left: 0;
        padding: 20px 20px 0;
        width: 100%;
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

    .showmore button {
        font-size: 20px;
        padding: 10px 60px;
    }

    .showmore {
        margin-top: 30px;
    }

}
</style>