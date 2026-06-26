<script setup>
import {
  ref,
  onMounted,
  onUnmounted,
  defineEmits,
  inject
} from 'vue'
import { RouterLink } from 'vue-router'

const searchQuery = ref('')
const isDropdownOpen = ref(false)
const isCatalogOpen = ref(false)
const isMegaMenuOpen = ref(false)

const filters = ref([
  { value: 'all', label: 'Везде' },
  { value: 'products', label: 'Товары' },
  { value: 'services', label: 'Услуги' },
  { value: 'companies', label: 'Компании' }
])

const selectedFilter = ref(filters.value[0])
const favoritesCount = ref(3)

const cartCount = inject('cartCount', ref(0))

const isCustomersDropdownOpen = ref(false)
const isSuppliersDropdownOpen = ref(false)

const customersLinks = ref([
  { to: '/work', text: 'Как это работает' },
  { to: '/def', text: 'Защита покупателя' },
  { to: '/pay', text: 'Условия оплаты' },
  { to: '/us', text: 'Условия использования' },
  { to: '/acc', text: 'Регистрация аккаунта' }
])

const suppliersLinks = ref([
  { to: '/work', text: 'Как стать продавцом' },
  { to: '/def', text: 'Правила участия' },
  { to: '/pay', text: 'Личный кабинет продавца' }
])

const categories = ref([
  { id: 1, name: 'Акции', link: '/catalog/sales', icon: 'fire', bold: true },
  { id: 2, name: 'Компьютеры и оргтехника', link: '/catalog/computers', icon: 'computer' },
  { id: 3, name: 'Электроника', link: '/catalog/electronics', icon: 'electronics' },
  { id: 4, name: 'Бытовая техника', link: '/catalog/appliances', icon: 'appliance' },
  { id: 5, name: 'Одежда для мужчин', link: '/catalog/men', icon: 'men', bold: true },
  { id: 6, name: 'Одежда для женщин', link: '/catalog/women', icon: 'women' },
  { id: 7, name: 'Все для детей', link: '/catalog/kids', icon: 'kids' },
  { id: 8, name: 'Автотовары', link: '/catalog/auto', icon: 'auto' },
  { id: 9, name: 'Красота и здоровье', link: '/catalog/beauty', icon: 'beauty' },
  { id: 10, name: 'Спорт и развлечения', link: '/catalog/sport', icon: 'sport' },
  { id: 11, name: 'Товары для животных', link: '/catalog/pets', icon: 'pets' },
  { id: 12, name: 'Хобби и творчество', link: '/catalog/hobby', icon: 'hobby' },
  { id: 13, name: 'Канцелярские товары', link: '/catalog/stationery', icon: 'stationery' },
  { id: 14, name: 'Бытовая химия', link: '/catalog/chemicals', icon: 'chemicals' },
  { id: 15, name: 'Premium', link: '/catalog/premium', icon: 'premium', premium: true },
  { id: 16, name: 'Уцененные товары', link: '/catalog/discounted', icon: 'discounted', discounted: true }
])

const megaMenuData = ref([
  {
    title: 'Укладка волос',
    items: ['Электрощипцы и плойки', 'Выпрямители', 'Мультистайлеры', 'Электробигуди', 'Фены и фены-щётки', 'Термощётки', 'Средства для укладки']
  },
  {
    title: 'Бритьё и стрижка',
    items: ['Электробритвы', 'Машинки для стрижки', 'Триммеры', 'OneBlade Pro', 'Аксессуары для электробритв']
  },
  {
    title: 'Товары для красоты',
    items: ['Эпиляторы', 'Для ухода за лицом', 'Для ухода за ногами', 'Маникюрные наборы', 'Зеркала косметические']
  },
  {
    title: 'Товары для ухода',
    items: ['Эпиляторы', 'Для ухода за лицом', 'Для ухода за ногами', 'Маникюрные наборы', 'Зеркала косметические']
  },
  {
    title: 'Товары для здоровья',
    items: ['Зубные щётки', 'Ирригаторы и зубные центры', 'Весы', 'Массажёры', 'Массажные ванночки для ног', 'Ингаляторы', 'Тонометры', 'Устройства для дезинфекции']
  },
  {
    title: 'Товары для детей',
    items: ['Видеоняни', 'Зубные щётки', 'Увлажнители воздуха', 'Ингаляторы', 'Детские светильники', 'Пароварки-блендеры', 'Товары для кормления', 'Детский проектор']
  },
  {
    title: 'Товары для фитнеса',
    items: ['Смарт-часы', 'Фитнес-браслеты', 'Спортивные часы', 'MP3-плееры', 'Умные весы', 'Гироскутеры', 'Спортивные наушники', 'Электрические велосипеды']
  },
  {
    title: 'Товары для детей',
    items: ['Видеоняни', 'Зубные щётки', 'Увлажнители воздуха', 'Ингаляторы', 'Детские светильники', 'Пароварки-блендеры', 'Товары для кормления', 'Детский проектор']
  }
])

// Баннеры
const banners = ref([])
const currentBannerIndex = ref(0)

const toggleDropdown = () => {
  isDropdownOpen.value = !isDropdownOpen.value
}

const selectFilter = (filter) => {
  selectedFilter.value = filter
  isDropdownOpen.value = false
}

const performSearch = () => {
  if (searchQuery.value.trim()) {
    emit('search', {
      query: searchQuery.value,
      filter: selectedFilter.value.value
    })
  }
}

const toggleCustomersDropdown = () => {
  isCustomersDropdownOpen.value = !isCustomersDropdownOpen.value
  isSuppliersDropdownOpen.value = false
}

const toggleSuppliersDropdown = () => {
  isSuppliersDropdownOpen.value = !isSuppliersDropdownOpen.value
  isCustomersDropdownOpen.value = false
}

const toggleCatalog = () => {
  isCatalogOpen.value = !isCatalogOpen.value
  if (!isCatalogOpen.value) {
    isMegaMenuOpen.value = false
  }
}

const toggleMegaMenu = () => {
  isMegaMenuOpen.value = !isMegaMenuOpen.value
}

const closeAll = () => {
  isCatalogOpen.value = false
  isMegaMenuOpen.value = false
}

// Навигация по баннерам
const nextBanner = () => {
  if (banners.value.length > 0) {
    currentBannerIndex.value = (currentBannerIndex.value + 1) % banners.value.length
  }
}

const prevBanner = () => {
  if (banners.value.length > 0) {
    currentBannerIndex.value = (currentBannerIndex.value - 1 + banners.value.length) % banners.value.length
  }
}

const handleClickOutside = (event) => {
  if (!event.target.closest('.search-filter-wrapper')) {
    isDropdownOpen.value = false
  }
  if (!event.target.closest('.customers-dropdown-wrapper')) {
    isCustomersDropdownOpen.value = false
    isSuppliersDropdownOpen.value = false
  }
  if (isCatalogOpen.value && !event.target.closest('.catalog-dropdown-menu') && !event.target.closest('.catalog-img')) {
    closeAll()
  }
}

const handleLogin = () => {
  console.log('Вход')
}

const handleFavorites = () => {
  console.log('Избранное')
}

const handleCart = () => {
  console.log('Корзина')
}

const getCartCount = () => {
  return cartCount.value
}

defineExpose({
  getCartCount
})

onMounted(async () => {
  document.addEventListener('click', handleClickOutside)

  // Загрузка баннеров
  try {
    const bannersRes = await fetch('/data/banners.json')
    const bannersData = await bannersRes.json()
    banners.value = bannersData.banners || []
  } catch (error) {
    console.error('Error loading banners:', error)
  }
})

onUnmounted(() => {
  document.removeEventListener('click', handleClickOutside)
})

const emit = defineEmits(['search'])
</script>

<template>
  <div class="header-container">
    <div class="Upheader">
      <div class="customers">
        <div class="customers-dropdown-wrapper">
          <div class="customers1" @click="toggleCustomersDropdown">
            Покупателям
            <img src="/images/Header/arrow.svg" alt="catalog" width="10" height="10" style="padding-right: 20px; padding-left: 5px; " />
          </div>
          <transition name="dropdown">
            <div v-if="isCustomersDropdownOpen" class="customers-dropdown-menu">
              <h3 class="dropdown-title">Покупателям</h3>
              <router-link v-for="link in customersLinks" :key="link.to" :to="link.to" class="dropdown-link">{{ link.text }}</router-link>
            </div>
          </transition>
        </div>

        <div class="customers-dropdown-wrapper">
          <div class="customers2" @click="toggleSuppliersDropdown">
            Поставщики
            <img src="/images/Header/arrow.svg" alt="catalog" width="10" height="10" style="padding-right: 20px; padding-left: 5px; " />
          </div>
          <transition name="dropdown">
            <div v-if="isSuppliersDropdownOpen" class="customers-dropdown-menu">
              <h3 class="dropdown-title">Поставщикам</h3>
              <router-link v-for="link in suppliersLinks" :key="link.to" :to="link.to" class="dropdown-link">{{ link.text }}</router-link>
            </div>
          </transition>
        </div>

        <div class="customers3">Частые вопросы</div>
      </div>

      <div class="language">
        <img class="language1" src="/images/Header/ru.svg" alt="rulang">
        <span class="divider">|</span>
        <div class="language2">Русский</div>
        <span class="divider">|</span>
        <div class="language2">₽</div>
        <img src="/images/Header/arrow.svg" alt="catalog">
      </div>
    </div>

    <div class="Botheader">
      <div class="Catalog" style="position: relative;">
        <img class="catalog-img" src="/images/Header/catalog.png" alt="catalog" @click="toggleCatalog" style="cursor: pointer;">

        <!-- Выпадающее меню каталога -->
        <transition name="catalog-dropdown">
          <div v-if="isCatalogOpen" class="catalog-dropdown-menu" :class="{ 'mega-open': isMegaMenuOpen }">

            <!-- Левая панель со списком категорий -->
            <div class="catalog-sidebar">
              <div class="catalog-header">
                <div class="catalog-dots">
                  <span></span>
                  <span></span>
                  <span></span>
                </div>
                <h3 class="catalog-title">Популярные категории</h3>
              </div>

              <div class="catalog-list">
                <RouterLink
                    v-for="category in categories"
                    :key="category.id"
                    :to="category.link"
                    class="catalog-item"
                    :class="{
                      'bold-item': category.bold,
                      'premium-item': category.premium,
                      'discounted-item': category.discounted
                    }"
                    @click="closeAll"
                >
                  <svg class="catalog-icon-svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
                    <template v-if="category.icon === 'fire'">
                      <path d="M8.5 14.5A2.5 2.5 0 0011 12c0-1.38-.5-2-1-3-1.072-2.143-.224-4.054 2-6 .5 2.5 2 4.9 4 6.5 2 1.6 3 3.5 3 5.5a7 7 0 11-14 0c0-1.153.433-2.294 1-3a2.5 2.5 0 002.5 2.5z"></path>
                    </template>
                    <template v-else-if="category.icon === 'computer'">
                      <rect x="2" y="3" width="20" height="14" rx="2" ry="2"></rect>
                      <line x1="8" y1="21" x2="16" y2="21"></line>
                      <line x1="12" y1="17" x2="12" y2="21"></line>
                    </template>
                    <template v-else-if="category.icon === 'electronics'">
                      <path d="M12 2v6"></path>
                      <path d="M12 18v4"></path>
                      <rect x="8" y="8" width="8" height="10" rx="1"></rect>
                      <path d="M9 12h6"></path>
                      <path d="M12 9v6"></path>
                    </template>
                    <template v-else-if="category.icon === 'appliance'">
                      <rect x="3" y="4" width="18" height="16" rx="2"></rect>
                      <rect x="7" y="8" width="10" height="8" rx="1"></rect>
                      <circle cx="17" cy="7" r="1" fill="currentColor"></circle>
                    </template>
                    <template v-else-if="category.icon === 'men'">
                      <path d="M9 3h6l3 4-2 1v13H8V8L6 7z"></path>
                      <path d="M9 3c0 1.5 1.5 3 3 3s3-1.5 3-3"></path>
                    </template>
                    <template v-else-if="category.icon === 'women'">
                      <path d="M12 3L8 7l2 2v12h4V9l2-2z"></path>
                      <path d="M9 7c0-1.5 1.5-3 3-3s3 1.5 3 3"></path>
                    </template>
                    <template v-else-if="category.icon === 'kids'">
                      <circle cx="12" cy="8" r="4"></circle>
                      <path d="M6 21v-2a4 4 0 014-4h4a4 4 0 014 4v2"></path>
                      <path d="M9 6l-1-2"></path>
                      <path d="M15 6l1-2"></path>
                    </template>
                    <template v-else-if="category.icon === 'auto'">
                      <circle cx="12" cy="12" r="9"></circle>
                      <circle cx="12" cy="12" r="3"></circle>
                      <line x1="12" y1="3" x2="12" y2="9"></line>
                      <line x1="12" y1="15" x2="12" y2="21"></line>
                      <line x1="3" y1="12" x2="9" y2="12"></line>
                      <line x1="15" y1="12" x2="21" y2="12"></line>
                    </template>
                    <template v-else-if="category.icon === 'beauty'">
                      <path d="M12 22c-4-3-8-6-8-11a4 4 0 018 0 4 4 0 018 0c0 5-4 8-8 11z"></path>
                      <path d="M12 11c-2-2-4-4-4-7"></path>
                      <path d="M12 11c2-2 4-4 4-7"></path>
                    </template>
                    <template v-else-if="category.icon === 'sport'">
                      <circle cx="12" cy="12" r="10"></circle>
                      <path d="M5.5 5.5l13 13"></path>
                      <path d="M12 2a15 15 0 014 10 15 15 0 01-4 10"></path>
                      <path d="M12 2a15 15 0 00-4 10 15 15 0 004 10"></path>
                      <line x1="2" y1="12" x2="22" y2="12"></line>
                    </template>
                    <template v-else-if="category.icon === 'pets'">
                      <path d="M12 10c-2 0-4 2-4 5v3h8v-3c0-3-2-5-4-5z"></path>
                      <circle cx="7" cy="7" r="2"></circle>
                      <circle cx="17" cy="7" r="2"></circle>
                      <circle cx="5" cy="11" r="1.5"></circle>
                      <circle cx="19" cy="11" r="1.5"></circle>
                    </template>
                    <template v-else-if="category.icon === 'hobby'">
                      <path d="M12 2L2 7l10 5 10-5-10-5z"></path>
                      <path d="M2 17l10 5 10-5"></path>
                      <path d="M2 12l10 5 10-5"></path>
                    </template>
                    <template v-else-if="category.icon === 'stationery'">
                      <path d="M4 19.5A2.5 2.5 0 016.5 17H20"></path>
                      <path d="M6.5 2H20v20H6.5A2.5 2.5 0 014 19.5v-15A2.5 2.5 0 016.5 2z"></path>
                    </template>
                    <template v-else-if="category.icon === 'chemicals'">
                      <path d="M10 2v7.31L6 15v7h12v-7l-4-5.69V2h-4z"></path>
                      <path d="M8.5 2h7"></path>
                      <path d="M14 9.5a2.5 2.5 0 01-5 0"></path>
                    </template>
                    <template v-else-if="category.icon === 'premium'">
                      <path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z"></path>
                    </template>
                    <template v-else-if="category.icon === 'discounted'">
                      <circle cx="12" cy="12" r="10"></circle>
                      <line x1="4.93" y1="4.93" x2="19.07" y2="19.07"></line>
                    </template>
                  </svg>
                  <span class="catalog-name">{{ category.name }}</span>
                </RouterLink>
              </div>

              <div class="catalog-footer">
                <button class="all-categories-btn" @click="toggleMegaMenu">
                  Все категории
                  <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" :class="{ 'rotate-icon': isMegaMenuOpen }">
                    <polyline points="6 9 12 15 18 9"></polyline>
                  </svg>
                </button>
              </div>
            </div>

            <!-- Мега-меню со всеми подкатегориями -->
            <transition name="mega-menu">
              <div v-if="isMegaMenuOpen" class="mega-menu-content">
                <div class="mega-menu-grid">
                  <div
                      v-for="(subcat, index) in megaMenuData"
                      :key="index"
                      class="mega-menu-column"
                  >
                    <h4 class="mega-menu-title">{{ subcat.title }}</h4>
                    <ul class="mega-menu-list">
                      <li v-for="(item, itemIndex) in subcat.items" :key="itemIndex" class="mega-menu-item">
                        <a href="#" class="mega-menu-link">{{ item }}</a>
                      </li>
                    </ul>
                  </div>
                </div>

                <!-- Баннер на всю ширину -->
                <div class="mega-banner-full">
                  <button class="mega-banner-arrow mega-banner-arrow-left" @click="prevBanner">
                    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                      <polyline points="15 18 9 12 15 6"></polyline>
                    </svg>
                  </button>

                  <div
                      v-for="(banner, index) in banners"
                      :key="banner.id"
                      class="mega-banner-item-full"
                      v-show="index === currentBannerIndex"
                  >
                    <img :src="banner.image" :alt="'Banner ' + banner.id" class="mega-banner-img-full">
                  </div>

                  <button class="mega-banner-arrow mega-banner-arrow-right" @click="nextBanner">
                    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                      <polyline points="9 18 15 12 9 6"></polyline>
                    </svg>
                  </button>
                </div>
              </div>
            </transition>
          </div>
        </transition>

        <router-link to="/"> <img class="logo1" src="/images/Header/PlaceMik.svg" alt="PlaceMik"></router-link>
        <div class="Place">
          <div class="search-container">
            <div class="search-filter-wrapper">
              <div class="search-wrapper">
                <button class="search-filter" @click="toggleDropdown" :class="{ active: isDropdownOpen }">
                  <span class="search-filter-text">{{ selectedFilter.label }}</span>
                  <span class="search-filter-arrow">▼</span>
                </button>
                <input v-model="searchQuery" type="text" class="search-input" placeholder="Поиск..." @keyup.enter="performSearch">
                <button class="search-button" @click="performSearch">
                  <svg class="search-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <circle cx="11" cy="11" r="8"></circle>
                    <path d="m21 21-4.35-4.35"></path>
                  </svg>
                </button>
              </div>
              <transition name="dropdown">
                <div v-if="isDropdownOpen" class="dropdown-menu">
                  <div v-for="filter in filters" :key="filter.value" class="dropdown-item" :class="{ active: selectedFilter.value === filter.value }" @click="selectFilter(filter)">{{ filter.label }}</div>
                </div>
              </transition>
            </div>
          </div>
        </div>
      </div>

      <div class="Icons">
        <div class="icon-item" @click="handleLogin">
          <img src="/images/Header/login.svg" alt="Войти" class="icon-img">
          <span class="icon-label">Войти</span>
        </div>

        <div class="icon-item" @click="handleFavorites">
          <div class="icon-wrapper">
            <img src="/images/Header/favorite.svg" alt="Избранное" class="icon-img">
            <span v-if="favoritesCount > 0" class="icon-badge">{{ favoritesCount }}</span>
          </div>
          <span class="icon-label">Избранное</span>
        </div>

        <div class="icon-item" @click="handleCart">
          <div class="icon-wrapper">
            <img src="/images/Header/cart.svg" alt="Корзина" class="icon-img">
            <span v-if="cartCount > 0" class="icon-badge cart-badge">{{ cartCount }}</span>
          </div>
          <span class="icon-label">Корзина</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.header-container {
  max-width: 1366px;
  margin: 0 auto;
  padding: 0 20px;
}

.Upheader {
  display: flex;
  flex-direction: row;
  padding-bottom: 10px;
  margin-top: 20px;
}

.customers {
  margin: auto auto auto 320px;
  display: flex;
  width: 40%;
  flex-direction: row;
  justify-content: start;
  align-items: center;
}

.customers-dropdown-wrapper {
  position: relative;
  cursor: pointer;
}

.customers1,
.customers2,
.customers3 {
  display: flex;
  align-items: center;
}

.customers-dropdown-menu {
  position: absolute;
  top: calc(100% + 10px);
  left: 0;
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  min-width: 220px;
  z-index: 1000;
  padding: 15px 0;
}

.dropdown-title {
  font-size: 16px;
  font-weight: bold;
  padding: 0 20px 10px 20px;
  margin: 0;
  color: #333;
  border-bottom: 1px solid #eee;
  margin-bottom: 10px;
}

.dropdown-link {
  display: block;
  padding: 10px 20px;
  color: #333;
  text-decoration: none;
  transition: background 0.2s;
  font-size: 14px;
}

.dropdown-link:hover {
  background: #fdf2f8;
  color: #9b59b6;
}

.language {
  display: flex;
  flex-direction: row;
  justify-content: end;
  align-items: center;
}

.language1 {
  padding-right: 6px;
}

.divider {
  color: #999;
}

.search-container {
  width: 100%;
}

.search-filter-wrapper {
  position: relative;
}

.search-wrapper {
  display: flex;
  align-items: stretch;
  border: 2px solid #9b59b6;
  overflow: hidden;
  background: white;
  transition: border-color 0.3s;
}

.search-wrapper:focus-within {
  border-color: #8e44ad;
  box-shadow: 0 0 0 3px rgba(155, 89, 182, 0.1);
}

.search-filter {
  display: flex;
  align-items: center;
  padding: 12px 20px;
  background: #fdf2f8;
  border: none;
  cursor: pointer;
  font-size: 15px;
  color: #333;
  transition: background 0.3s;
  font-family: inherit;
}

.search-filter:hover {
  background: #fce7f3;
}

.search-filter.active {
  background: #fce7f3;
}

.search-filter-text {
  margin-right: 8px;
}

.search-filter-arrow {
  font-size: 10px;
  transition: transform 0.3s;
}

.search-filter.active .search-filter-arrow {
  transform: rotate(180deg);
}

.search-input {
  flex: 1;
  border: none;
  padding: 12px 16px;
  font-size: 15px;
  outline: none;
  min-width: 0;
  font-family: inherit;
}

.search-button {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 20px;
  background: transparent;
  border: none;
  cursor: pointer;
  color: #333;
  transition: background 0.3s;
}

.search-button:hover {
  background: #f5f5f5;
}

.search-icon {
  width: 20px;
  height: 20px;
}

.dropdown-menu {
  position: absolute;
  top: calc(100% + 4px);
  left: 0;
  background: white;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  min-width: 200px;
  z-index: 1000;
  overflow: hidden;
}

.dropdown-item {
  padding: 12px 20px;
  cursor: pointer;
  transition: background 0.2s;
}

.dropdown-item:hover {
  background: #fdf2f8;
}

.dropdown-item.active {
  background: #fdf2f8;
  color: #9b59b6;
}

.dropdown-enter-active,
.dropdown-leave-active {
  transition: all 0.2s ease;
}

.dropdown-enter-from,
.dropdown-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}

/* ===== КАТАЛОГ DROPDOWN ===== */
.catalog-dropdown-menu {
  position: absolute;
  top: calc(100% + 8px);
  left: 0;
  display: flex;
  background: white;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.15);
  z-index: 1000;
  overflow: hidden;
  transition: all 0.3s ease;
}

.catalog-dropdown-menu.mega-open {
  width: 100%;
  max-width: 1100px;
}

.catalog-sidebar {
  width: 280px;
  min-width: 280px;
  background: #f5f0fa;
  display: flex;
  flex-direction: column;
}

.catalog-header {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 18px 22px;
  background: linear-gradient(135deg, #c970d0 0%, #7b68ee 100%);
  color: white;
}

.catalog-dots {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.catalog-dots span {
  width: 4px;
  height: 4px;
  background: white;
  border-radius: 50%;
}

.catalog-title {
  font-size: 17px;
  font-weight: 700;
  margin: 0;
  letter-spacing: 0.3px;
}

.catalog-list {
  padding: 6px 0;
  flex: 1;
  overflow-y: auto;
  max-height: 520px;
}

.catalog-item {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 11px 22px;
  text-decoration: none;
  color: #333;
  transition: all 0.15s;
  cursor: pointer;
  font-size: 15px;
}

.catalog-item:hover {
  background: rgba(155, 89, 182, 0.1);
  color: #9b59b6;
}

.catalog-item.bold-item {
  font-weight: 700;
  color: #1a1a1a;
}

.catalog-item.premium-item {
  color: #9b59b6;
  font-weight: 600;
}

.catalog-item.discounted-item {
  color: #666;
}

.catalog-icon-svg {
  width: 22px;
  height: 22px;
  color: #9b59b6;
  flex-shrink: 0;
}

.catalog-name {
  flex: 1;
  font-size: 14px;
}

.catalog-footer {
  padding: 14px 22px;
  border-top: 1px solid #e8e0f0;
}

.all-categories-btn {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  background: none;
  border: none;
  color: #9b59b6;
  font-size: 15px;
  font-weight: 600;
  cursor: pointer;
  padding: 8px 0;
  font-family: inherit;
  transition: color 0.2s;
}

.all-categories-btn:hover {
  color: #8e44ad;
}

.rotate-icon {
  transform: rotate(180deg);
  transition: transform 0.3s ease;
}

.all-categories-btn svg {
  width: 16px;
  height: 16px;
  transition: transform 0.3s ease;
}

/* ===== МЕГА-МЕНЮ ===== */
.mega-menu-content {
  flex: 1;
  padding: 24px 30px;
  background: white;
  min-width: 700px;
  overflow-y: auto;
  max-height: 580px;
}

.mega-menu-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px 30px;
  margin-bottom: 20px;
}

.mega-menu-column {
  display: flex;
  flex-direction: column;
}

.mega-menu-title {
  font-size: 15px;
  font-weight: 700;
  color: #1a1a1a;
  margin: 0 0 10px 0;
}

.mega-menu-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.mega-menu-item {
  margin-bottom: 4px;
}

.mega-menu-link {
  color: #555;
  text-decoration: none;
  font-size: 13px;
  transition: all 0.2s;
  display: block;
  padding: 2px 0;
  line-height: 1.4;
}

.mega-menu-link:hover {
  color: #9b59b6;
}

/* ===== БАННЕР НА ВСЮ ШИРИНУ ===== */
.mega-banner-full {
  position: relative;
  width: 100%;
  overflow: hidden;
  margin-top: 10px;
}

.mega-banner-item-full {
  width: 100%;
  height: 220px;
  overflow: hidden;
}

.mega-banner-img-full {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.mega-banner-arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 44px;
  height: 44px;
  border-radius: 50%;
  border: none;
  background: linear-gradient(135deg, #c9a0dc 0%, #a8b8e8 100%);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  z-index: 2;
  transition: all 0.3s;
  box-shadow: 0 2px 8px rgba(155, 89, 182, 0.3);
}

.mega-banner-arrow:hover {
  transform: translateY(-50%) scale(1.08);
  box-shadow: 0 4px 16px rgba(155, 89, 182, 0.5);
}

.mega-banner-arrow svg {
  stroke: white;
  width: 20px;
  height: 20px;
}

.mega-banner-arrow-left {
  left: 16px;
}

.mega-banner-arrow-right {
  right: 16px;
  background: linear-gradient(135deg, #9b59b6 0%, #667eea 100%);
}

.mega-menu-enter-active,
.mega-menu-leave-active {
  transition: all 0.3s ease;
  overflow: hidden;
}

.mega-menu-enter-from {
  opacity: 0;
  max-width: 0;
  padding: 0;
}

.mega-menu-leave-to {
  opacity: 0;
  max-width: 0;
  padding: 0;
}

.catalog-dropdown-enter-active,
.catalog-dropdown-leave-active {
  transition: all 0.25s ease;
}

.catalog-dropdown-enter-from {
  opacity: 0;
  transform: translateY(-10px);
}

.catalog-dropdown-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}

.Place {
  display: flex;
  align-items: center;
  justify-content: start;
  margin-left: 15px;
  width: 100%;
}

.Icons {
  display: flex;
  flex-direction: row;
  gap: 30px;
  align-items: center;
  justify-content: end;
}

.icon-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  cursor: pointer;
}

.icon-wrapper {
  position: relative;
}

.icon-img {
  width: 32px;
  height: 32px;
  object-fit: contain;
}

.icon-badge {
  position: absolute;
  top: -8px;
  right: -8px;
  background: #ff4757;
  color: white;
  font-size: 11px;
  font-weight: bold;
  min-width: 20px;
  height: 20px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.cart-badge {
  background: #ff4757;
  animation: bounce 0.3s ease;
}

@keyframes bounce {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.2); }
}

.icon-label {
  font-size: 12px;
  margin-top: 5px;
  color: #333;
}

.Botheader {
  display: flex;
  flex-direction: row;
  align-items: center;
  width: 100%;
  gap: 20px;
}

.Catalog {
  display: flex;
  align-items: center;
  width: 100%;
  justify-content: start;
}

.catalog-img {
  display: flex;
  padding-right: 32px;
}

.language {
  display: flex;
  align-items: center;
}
@media (max-width: 1024px) {
  .header-container {
    padding: 0 15px;
  }

  .Upheader {
    flex-direction: column;
    gap: 10px;
    margin-top: 10px;
  }

  .customers {
    margin: 0;
    width: 100%;
    justify-content: center;
    gap: 20px;
  }

  .language {
    justify-content: center;
  }

  .Botheader {
    flex-wrap: wrap;
    gap: 15px;
  }

  .Catalog {
    width: 100%;
    order: 1;
  }

  .Place {
    width: 100%;
    order: 3;
    margin-left: 0;
    margin-top: 10px;
  }

  .Icons {
    order: 2;
    gap: 20px;
  }

  .catalog-dropdown-menu.mega-open {
    max-width: 100%;
    left: -100px;
  }

  .mega-menu-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 768px) {
  .customers1,
  .customers2 {
    font-size: 13px;
  }

  .customers3 {
    display: none;
  }

  .search-wrapper {
    flex-direction: column;
  }

  .search-filter {
    width: 100%;
    justify-content: center;
    border-bottom: 1px solid #e0e0e0;
  }

  .search-input {
    width: 100%;
    border: none;
  }

  .search-button {
    width: 100%;
    padding: 12px;
    background: #9b59b6;
  }

  .search-button:hover {
    background: #8e44ad;
  }

  .search-icon {
    stroke: white;
  }

  .icon-label {
    display: none;
  }

  .icon-img {
    width: 28px;
    height: 28px;
  }

  .catalog-sidebar {
    width: 240px;
    min-width: 240px;
  }

  .mega-menu-content {
    min-width: auto;
    padding: 15px;
  }

  .mega-menu-grid {
    grid-template-columns: 1fr;
    gap: 15px;
  }

  .mega-banner-item-full {
    height: 150px;
  }
}

@media (max-width: 480px) {
  .catalog-img {
    width: 24px;
    height: 24px;
  }

  .logo1 {
    max-width: 120px;
    height: auto;
  }

  .Icons {
    gap: 15px;
  }

  .icon-wrapper {
    position: relative;
  }

  .icon-badge {
    font-size: 9px;
    min-width: 16px;
    height: 16px;
    padding: 0 4px;
  }
}
</style>