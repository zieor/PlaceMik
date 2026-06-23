<script setup>
import {
  ref,
  onMounted,
  onUnmounted,
  defineEmits
} from 'vue'

const searchQuery = ref('')
const isDropdownOpen = ref(false)

const filters = ref([
  { value: 'all', label: 'Везде' },
  { value: 'products', label: 'Товары' },
  { value: 'services', label: 'Услуги' },
  { value: 'companies', label: 'Компании' }
])

const selectedFilter = ref(filters.value[0])

const favoritesCount = ref(3)
const cartCount = ref(2)

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

const handleClickOutside = (event) => {
  if (!event.target.closest('.search-filter-wrapper')) {
    isDropdownOpen.value = false
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

onMounted(() => {
  document.addEventListener('click', handleClickOutside)
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
        <div class="customers1">Покупателям</div>
        <img  src="/images/Header/arrow.svg" alt="catalog" width="10" height="10" style="padding-right: 20px; padding-left: 5px; " />
        <div class="customers2">Поставщики</div>
        <img  src="/images/Header/arrow.svg" alt="catalog" width="10" height="10" style="padding-right: 20px; padding-left: 5px; " />
        <div class="customers3">Частые вопросы</div>
        <img  src="/images/Header/arrow.svg" alt="catalog" width="10" height="10" style="padding-left: 5px; " />
      </div>

      <div class="language">
        <img class="language1" src="/images/Header/ru.svg" alt="rulang">
        <span class="divider">|</span>
        <div class="language2">Русский</div>
        <span class="divider">|</span>
        <div class="language2">₽</div>
        <img  src="/images/Header/arrow.svg" alt="catalog">
      </div>
    </div>

    <div class="Botheader">
      <div class="Catalog">
        <img class="catalog-img" src="/images/Header/catalog.svg" alt="catalog">
        <img src="/images/Header/PlaceMik.svg" alt="PlaceMik">
        <div class="Place">
          <div class="search-container">
            <div class="search-filter-wrapper">
              <div class="search-wrapper">
                <button
                    class="search-filter"
                    @click="toggleDropdown"
                    :class="{ active: isDropdownOpen }"
                >
                  <span class="search-filter-text">{{ selectedFilter.label }}</span>
                  <span class="search-filter-arrow">▼</span>
                </button>

                <input
                    v-model="searchQuery"
                    type="text"
                    class="search-input"
                    placeholder="Поиск..."
                    @keyup.enter="performSearch"
                >

                <button class="search-button" @click="performSearch">
                  <svg class="search-icon" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
                    <circle cx="11" cy="11" r="8"></circle>
                    <path d="m21 21-4.35-4.35"></path>
                  </svg>
                </button>
              </div>

              <transition name="dropdown">
                <div v-if="isDropdownOpen" class="dropdown-menu">
                  <div
                      v-for="filter in filters"
                      :key="filter.value"
                      class="dropdown-item"
                      :class="{ active: selectedFilter.value === filter.value }"
                      @click="selectFilter(filter)"
                  >
                    {{ filter.label }}
                  </div>
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
            <span v-if="cartCount > 0" class="icon-badge">{{ cartCount }}</span>
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
  margin: 0 auto;
  display: flex;
  width: 40%;
  flex-direction: row;
  justify-content: start;

  align-items: center;
}

.language {
  display: flex;
  flex-direction: row;
  justify-content: end;
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
  border-radius: 8px;
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
  top: -5px;
  right: -5px;
  background: #ff4757;
  color: white;
  font-size: 11px;
  font-weight: bold;
  min-width: 18px;
  height: 18px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
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
.catalog-img{
  display: flex;
  padding-bottom: 20px;
}
.language{
  display: flex;
  align-items: center;
}
</style>