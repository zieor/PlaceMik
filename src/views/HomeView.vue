<script setup>
import { ref, onMounted, inject } from 'vue'

const emit = defineEmits(['add-to-cart'])

const products = ref([])
const newProducts = ref([])
const recommendedProducts = ref([])
const viewedProducts = ref([])
const banners = ref([])
const sideBanners = ref([])
const shops = ref([])
const loading = ref(true)
const hoveredProductId = ref(null)

const addToCartGlobal = inject('addToCart', () => {})

onMounted(async () => {
  try {
    const [productsRes, bannersRes] = await Promise.all([
      fetch('/data/products.json'),
      fetch('/data/banners.json')
    ])

    const productsData = await productsRes.json()
    const bannersData = await bannersRes.json()

    products.value = productsData.products
    newProducts.value = productsData.newProducts || []
    recommendedProducts.value = productsData.recommendedProducts || []
    viewedProducts.value = productsData.viewedProducts || []
    banners.value = bannersData.banners
    sideBanners.value = bannersData.sideBanners
    shops.value = bannersData.shops || []
  } catch (error) {
    console.error('Error loading data:', error)
  } finally {
    loading.value = false
  }
})

const formatPrice = (price) => {
  return new Intl.NumberFormat('ru-RU').format(price)
}

const addToCart = (product) => {
  addToCartGlobal(product)
  emit('add-to-cart', product)
}

const handleMouseEnter = (productId) => {
  hoveredProductId.value = productId
}

const handleMouseLeave = () => {
  hoveredProductId.value = null
}
</script>

<template>
  <div class="main-section">
    <div class="banners-container">
      <div class="main-banner-wrapper">
        <button class="banner-arrow banner-arrow-left">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <polyline points="15 18 9 12 15 6"></polyline>
          </svg>
        </button>

        <div
            v-for="banner in banners"
            :key="banner.id"
            class="banner-item main"
        >
          <img :src="banner.image" alt="Banner" class="banner-img">
        </div>

        <button class="banner-arrow banner-arrow-right">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <polyline points="9 18 15 12 9 6"></polyline>
          </svg>
        </button>
      </div>

      <div class="side-banners">
        <div
            v-for="banner in sideBanners"
            :key="banner.id"
            class="banner-item side"
        >
          <img :src="banner.image" alt="Side Banner" class="banner-img">
        </div>
      </div>
    </div>

    <div class="products-section sale-section">
      <h2 class="section-title">Успей купить</h2>

      <div v-if="loading" class="loading">
        <div class="loading-spinner"></div>
      </div>

      <div v-else class="products-grid">
        <div
            v-for="product in products"
            :key="product.id"
            class="product-card"
            :class="{ 'is-hovered': hoveredProductId === product.id }"
            @mouseenter="handleMouseEnter(product.id)"
            @mouseleave="handleMouseLeave"
        >
          <div class="product-badge" v-if="product.discount">
            -{{ product.discount }}%
          </div>

          <div class="product-image">
            <img :src="product.image" :alt="product.name">
          </div>

          <div class="product-info-default">
            <div class="product-price">
              <span class="current-price">{{ formatPrice(product.price) }} ₽</span>
              <span class="old-price" v-if="product.oldPrice">
                {{ formatPrice(product.oldPrice) }} ₽
              </span>
            </div>
            <h3 class="product-name-short">{{ product.name }}</h3>
          </div>

          <div class="product-info-expanded">
            <div class="product-price">
              <span class="current-price">{{ formatPrice(product.price) }} ₽</span>
              <span class="old-price" v-if="product.oldPrice">
                {{ formatPrice(product.oldPrice) }} ₽
              </span>
            </div>

            <div class="product-rating" v-if="product.rating">
              <span class="rating-value">{{ product.rating }}</span>
              <span class="star-icon">★</span>
              <span class="reviews-count" v-if="product.reviews">
                {{ product.reviews }} отзывов
              </span>
            </div>

            <h4 class="product-brand">{{ product.brand }}</h4>
            <p class="product-name-full">{{ product.name }}</p>

            <button class="add-to-cart-btn" @click="addToCart(product)">
              В корзину
            </button>
          </div>
        </div>
      </div>

      <div class="view-all-wrapper">
        <router-link to="/products?category=sale" class="view-all-btn">
          Все товары
        </router-link>
      </div>
    </div>

    <div class="products-section new-section" v-if="newProducts.length > 0">
      <h2 class="section-title">Новинки</h2>

      <div v-if="loading" class="loading">
        <div class="loading-spinner"></div>
      </div>

      <div v-else class="products-grid">
        <div
            v-for="product in newProducts"
            :key="product.id"
            class="product-card"
            :class="{ 'is-hovered': hoveredProductId === product.id }"
            @mouseenter="handleMouseEnter(product.id)"
            @mouseleave="handleMouseLeave"
        >
          <div class="product-badge" v-if="product.discount">
            -{{ product.discount }}%
          </div>

          <div class="product-image">
            <img :src="product.image" :alt="product.name">
          </div>

          <div class="product-info-default">
            <div class="product-price">
              <span class="current-price">{{ formatPrice(product.price) }} ₽</span>
              <span class="old-price" v-if="product.oldPrice">
                {{ formatPrice(product.oldPrice) }} ₽
              </span>
            </div>
            <h3 class="product-name-short">{{ product.name }}</h3>
          </div>

          <div class="product-info-expanded">
            <div class="product-price">
              <span class="current-price">{{ formatPrice(product.price) }} ₽</span>
              <span class="old-price" v-if="product.oldPrice">
                {{ formatPrice(product.oldPrice) }} ₽
              </span>
            </div>

            <div class="product-rating" v-if="product.rating">
              <span class="rating-value">{{ product.rating }}</span>
              <span class="star-icon">★</span>
              <span class="reviews-count" v-if="product.reviews">
                {{ product.reviews }} отзывов
              </span>
            </div>

            <h4 class="product-brand">{{ product.brand }}</h4>
            <p class="product-name-full">{{ product.name }}</p>

            <button class="add-to-cart-btn" @click="addToCart(product)">
              В корзину
            </button>
          </div>
        </div>
      </div>

      <div class="view-all-wrapper">
        <router-link to="/products?category=new" class="view-all-btn">
          Все товары
        </router-link>
      </div>
    </div>

    <div class="products-section shops-section" v-if="shops.length > 0">
      <div class="section-header">
        <h2 class="section-title">Магазины для вас</h2>
      </div>

      <div v-if="loading" class="loading">
        <div class="loading-spinner"></div>
      </div>

      <div v-else class="shops-grid">
        <router-link
            v-for="shop in shops"
            :key="shop.id"
            to="/"
            class="shop-card"
        >
          <template v-if="shop.type === 'shop'">
            <div class="shop-content">
              <img :src="shop.logo" :alt="shop.brand + ' logo'" class="shop-image">
              <span class="shop-brand">{{ shop.brand }}</span>
            </div>
          </template>

          <template v-else-if="shop.type === 'promo'">
            <img :src="shop.image" alt="Promo" class="shop-promo-img">
          </template>
        </router-link>
      </div>

      <div class="view-all-wrapper">
        <router-link to="/" class="view-all-btn">
          Все магазины
        </router-link>
      </div>
    </div>

    <div class="products-section recommended-section" v-if="recommendedProducts.length > 0">
      <h2 class="section-title">Рекомендуемые для вас товары</h2>

      <div v-if="loading" class="loading">
        <div class="loading-spinner"></div>
      </div>

      <div v-else class="products-grid recommended-grid">
        <div
            v-for="product in recommendedProducts"
            :key="product.id"
            class="product-card"
            :class="{ 'is-hovered': hoveredProductId === product.id }"
            @mouseenter="handleMouseEnter(product.id)"
            @mouseleave="handleMouseLeave"
        >
          <div class="product-badge" v-if="product.discount">
            -{{ product.discount }}%
          </div>

          <div class="product-image">
            <img :src="product.image" :alt="product.name">
          </div>

          <div class="product-info-default">
            <div class="product-price">
              <span class="current-price">{{ formatPrice(product.price) }} ₽</span>
              <span class="old-price" v-if="product.oldPrice">
                {{ formatPrice(product.oldPrice) }} ₽
              </span>
            </div>
            <h3 class="product-name-short">{{ product.name }}</h3>
          </div>

          <div class="product-info-expanded">
            <div class="product-price">
              <span class="current-price">{{ formatPrice(product.price) }} ₽</span>
              <span class="old-price" v-if="product.oldPrice">
                {{ formatPrice(product.oldPrice) }} ₽
              </span>
            </div>

            <div class="product-rating" v-if="product.rating">
              <span class="rating-value">{{ product.rating }}</span>
              <span class="star-icon">★</span>
              <span class="reviews-count" v-if="product.reviews">
                {{ product.reviews }} отзывов
              </span>
            </div>

            <h4 class="product-brand">{{ product.brand }}</h4>
            <p class="product-name-full">{{ product.name }}</p>

            <button class="add-to-cart-btn" @click="addToCart(product)">
              В корзину
            </button>
          </div>
        </div>
      </div>

      <div class="view-all-wrapper">
        <router-link to="/products" class="view-all-btn">
          Все товары
        </router-link>
      </div>
    </div>

    <div class="info-section">
      <h2 class="info-title">Широкий ассортимент и высокое качество</h2>

      <div class="info-content">
        <p>
          Интернет-магазин PlaceMix — это доступные цены, широкий, регулярно обновляемый ассортимент.
          В онлайн-каталоге PlaceMix представлено около 35 000 ведущих брендов женской, мужской и
          детской одежды и обуви. Покупателям предлагается электроника, книжная продукция, детские товары.
          В интернет-магазине можно приобрести продукцию для дома, продукты питания, товары для красоты,
          ювелирные изделия, игрушки. Для удобства пользования онлайн-каталог поделен на разделы,
          все товары можно сортировать по ряду критериев: цена, материал изготовления, сезонность, бренд.
          Интернет-магазин PlaceMix регулярно проводит масштабные распродажи. В рамках таких акций
          предоставляются большие скидки (до 95%) на одежду, обувь, детские товары. Условия распродаж
          распространяются и на электронику, продукты питания, товары для дома, книги и многое другое.
        </p>
      </div>
    </div>

    <div class="products-section viewed-section" v-if="viewedProducts.length > 0">
      <h2 class="section-title">Ранее вы смотрели</h2>

      <div v-if="loading" class="loading">
        <div class="loading-spinner"></div>
      </div>

      <div v-else class="viewed-grid">
        <router-link
            v-for="product in viewedProducts"
            :key="product.id"
            :to="`/product/${product.id}`"
            class="viewed-card"
        >
          <div class="viewed-image">
            <img :src="product.image" :alt="product.name">
          </div>
          <div class="viewed-info">
            <h4 class="viewed-name">{{ product.name }}</h4>
            <div class="viewed-price">
              от {{ formatPrice(product.price) }} ₽
            </div>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<style scoped>
.main-section {
  max-width: 1366px;
  margin: 0 auto;
  padding: 20px 20px 0;
}

.banners-container {
  display: grid;
  grid-template-columns: 1fr 320px;
  gap: 20px;
  margin-bottom: 40px;
}

.main-banner-wrapper {
  position: relative;
  overflow: hidden;
}

.banner-item.main {
  width: 100%;
  height: 320px;
  overflow: hidden;
}

.banner-item.side {
  width: 100%;
  height: 150px;
  overflow: hidden;
  margin-bottom: 20px;
}

.banner-item.side:last-child {
  margin-bottom: 0;
}

.banner-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.banner-arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 44px;
  height: 44px;
  border-radius: 50%;
  border: none;
  background: linear-gradient(135deg, #9b59b6 0%, #667eea 100%);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  z-index: 2;
  transition: all 0.3s;
  box-shadow: 0 2px 8px rgba(155, 89, 182, 0.3);
}

.banner-arrow:hover {
  transform: translateY(-50%) scale(1.08);
  box-shadow: 0 4px 16px rgba(155, 89, 182, 0.5);
}

.banner-arrow svg {
  stroke: white;
  width: 20px;
  height: 20px;
}

.banner-arrow-left {
  left: 16px;
  background: linear-gradient(135deg, #c9a0dc 0%, #a8b8e8 100%);
}

.banner-arrow-right {
  right: 16px;
}

.products-section {
  margin-top: 40px;
  padding-top: 20px;
}

.sale-section {
  padding-bottom: 40px;
}

.new-section {
  margin-top: 20px;
}

.shops-section {
  margin-top: 20px;
}

.recommended-section {
  margin-top: 40px;
  padding-top: 20px;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
}

.section-title {
  font-size: 28px;
  font-weight: 700;
  color: #1a1a1a;
  margin: 0 0 24px 0;
}

.loading {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 300px;
}

.loading-spinner {
  width: 50px;
  height: 50px;
  border: 4px solid #f3f3f3;
  border-top: 4px solid #9b59b6;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 16px;
  margin-bottom: 30px;
}

.recommended-grid {
  grid-template-columns: repeat(6, 1fr);
}

.product-card {
  position: relative;
  background: white;
  overflow: visible;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.08);
  display: flex;
  flex-direction: column;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.product-card.is-hovered {
  transform: scale(1.15);
  box-shadow: 0 12px 32px rgba(0, 0, 0, 0.15);
  z-index: 100;
}

.product-badge {
  position: absolute;
  top: 10px;
  right: 10px;
  background: white;
  color: #333;
  padding: 4px 10px;
  font-size: 11px;
  font-weight: 600;
  z-index: 1;
  border: 1px solid #e0e0e0;
}

.product-image {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 180px;
}

.product-image img {
  max-width: 100%;
  max-height: 160px;
  object-fit: contain;
}

.product-info-default {
  padding: 12px 14px;
  flex: 1;
  display: flex;
  flex-direction: column;
}

.product-card.is-hovered .product-info-default {
  display: none;
}

.product-price {
  margin-bottom: 6px;
  display: flex;
  align-items: baseline;
  gap: 8px;
}

.current-price {
  font-size: 18px;
  font-weight: 700;
  color: #e74c3c;
}

.old-price {
  font-size: 13px;
  color: #bbb;
  text-decoration: line-through;
}

.product-name-short {
  font-size: 13px;
  color: #555;
  margin: 0;
  line-height: 1.4;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  font-weight: 400;
}

.product-info-expanded {
  display: none;
  padding: 12px 14px 14px;
  flex: 1;
  flex-direction: column;
  background: white;
}

.product-card.is-hovered .product-info-expanded {
  display: flex;
  position: absolute;
  top: 70%;
}

.product-rating {
  display: flex;
  align-items: center;
  gap: 4px;
  margin-bottom: 8px;
}

.rating-value {
  font-size: 14px;
  font-weight: 600;
  color: #1a1a1a;
}

.star-icon {
  color: #9b59b6;
  font-size: 16px;
}

.reviews-count {
  font-size: 12px;
  color: #999;
}

.product-brand {
  font-size: 16px;
  font-weight: 700;
  color: #1a1a1a;
  margin: 0 0 8px 0;
}

.product-name-full {
  font-size: 13px;
  color: #555;
  margin: 0 0 12px 0;
  line-height: 1.5;
  flex: 1;
}

.add-to-cart-btn {
  width: 100%;
  padding: 12px;
  background: linear-gradient(135deg, #9b59b6 0%, #667eea 100%);
  color: white;
  border: none;
  font-size: 15px;
  font-weight: 600;
  cursor: pointer;
  transition: opacity 0.2s;
  margin-top: auto;
}

.add-to-cart-btn:hover {
  opacity: 0.9;
}

.shops-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
  margin-bottom: 30px;
}

.shop-card {
  display: block;
  background: white;
  overflow: hidden;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.08);
  text-decoration: none;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  cursor: pointer;
  padding: 30px 15px 30px 15px;
  box-sizing: border-box;
  aspect-ratio: 4 / 3;
}

.shop-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
}

.shop-content {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
  height: 100%;
}

.shop-image {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.shop-brand {
  font-size: 16px;
  font-weight: 700;
  color: #1a1a1a;
  letter-spacing: 0.5px;
}

.shop-promo-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.info-section {
  margin-top: 60px;
  padding: 40px 0;
}

.info-title {
  font-size: 32px;
  font-weight: 700;
  color: #1a1a1a;
  margin: 0 0 24px 0;
}

.info-content {
  max-width: 100%;
}

.info-content p {
  font-size: 15px;
  line-height: 1.8;
  color: #555;
  margin: 0;
  text-align: justify;
}

.viewed-section {
  margin: 40px auto 0;
  padding: 40px;
  background: linear-gradient(135deg, #f6cbcb 0%, #bec2f6 100%);
}

.viewed-section .section-title {
  margin-bottom: 24px;
}

.viewed-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 16px;
}

.viewed-card {
  display: flex;
  flex-direction: row;
  align-items: center;
  background: white;
  padding: 16px;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.08);
  text-decoration: none;
  transition: all 0.3s;
  gap: 16px;
}

.viewed-card:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.12);
  transform: translateY(-2px);
}

.viewed-image {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  width: 100px;
}

.viewed-image img {
  max-width: 100%;
  max-height: 120px;
  object-fit: contain;
}

.viewed-info {
  display: flex;
  flex-direction: column;
  gap: 8px;
  flex: 1;
}

.viewed-name {
  font-size: 13px;
  color: #555;
  margin: 0;
  line-height: 1.4;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.viewed-price {
  font-size: 16px;
  font-weight: 700;
  color: #1a1a1a;
}

.view-all-wrapper {
  display: flex;
  justify-content: flex-end;
}

.view-all-btn {
  display: inline-block;
  padding: 12px 36px;
  background: white;
  border: 1px solid #e0e0e0;
  font-size: 15px;
  font-weight: 500;
  color: #1a1a1a;
  text-decoration: none;
  transition: all 0.3s;
  cursor: pointer;
}

.view-all-btn:hover {
  border-color: #9b59b6;
  color: #9b59b6;
}

@media (max-width: 1200px) {
  .main-section {
    padding: 15px;
  }

  .products-grid,
  .recommended-grid {
    grid-template-columns: repeat(4, 1fr);
  }

  .shops-grid {
    grid-template-columns: repeat(3, 1fr);
  }

  .viewed-grid {
    grid-template-columns: repeat(3, 1fr);
  }

  .banners-container {
    gap: 15px;
  }

  .side-banners {
    width: 280px;
  }
}

@media (max-width: 992px) {
  .products-grid,
  .recommended-grid {
    grid-template-columns: repeat(3, 1fr);
  }

  .shops-grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .viewed-grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .banners-container {
    grid-template-columns: 1fr;
  }

  .side-banners {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 15px;
    width: 100%;
  }

  .side-banners .banner-item.side {
    margin-bottom: 0;
    height: 180px;
  }
}

@media (max-width: 768px) {
  .products-grid,
  .recommended-grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .shops-grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .viewed-grid {
    grid-template-columns: 1fr;
  }

  .viewed-card {
    flex-direction: column;
    text-align: center;
  }

  .viewed-image {
    width: 100%;
    margin-bottom: 10px;
  }

  .section-title {
    font-size: 24px;
  }

  .info-title {
    font-size: 24px;
  }

  .side-banners {
    display: none;
  }
}

@media (max-width: 480px) {
  .products-grid,
  .recommended-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;
  }

  .shops-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;
  }

  .product-image {
    min-height: 130px;
  }

  .product-image img {
    max-height: 110px;
  }

  .current-price {
    font-size: 15px;
  }

  .old-price {
    font-size: 11px;
  }

  .product-name-short {
    font-size: 12px;
  }
}

@media (max-width: 375px) {
  .main-section {
    padding: 10px 10px 0;
  }

  .banner-item.main {
    height: 160px;
  }

  .banner-arrow {
    width: 32px;
    height: 32px;
  }

  .banner-arrow svg {
    width: 14px;
    height: 14px;
  }

  .banner-arrow-left {
    left: 10px;
  }

  .banner-arrow-right {
    right: 10px;
  }

  .section-title {
    font-size: 20px;
    margin-bottom: 16px;
  }

  .products-grid,
  .recommended-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 8px;
  }

  .product-card.is-hovered {
    transform: none;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  }

  .product-card.is-hovered .product-info-default {
    display: flex;
  }

  .product-card.is-hovered .product-info-expanded {
    display: none !important;
  }

  .product-image {
    min-height: 100px;
  }

  .product-image img {
    max-height: 85px;
  }

  .product-badge {
    padding: 2px 6px;
    font-size: 10px;
    top: 6px;
    right: 6px;
  }

  .product-info-default {
    padding: 8px;
  }

  .current-price {
    font-size: 13px;
  }

  .old-price {
    font-size: 10px;
  }

  .product-name-short {
    font-size: 11px;
    line-height: 1.3;
  }

  .shops-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 8px;
  }

  .shop-card {
    padding: 15px 8px;
  }

  .shop-brand {
    font-size: 11px;
  }

  .info-section {
    margin-top: 30px;
    padding: 20px 0;
  }

  .info-title {
    font-size: 18px;
    margin-bottom: 12px;
  }

  .info-content p {
    font-size: 13px;
    text-align: left;
    line-height: 1.6;
  }

  .viewed-section {
    margin-top: 20px;
    padding: 20px 12px;
  }

  .viewed-card {
    padding: 12px;
    gap: 12px;
  }

  .viewed-image img {
    max-height: 90px;
  }

  .viewed-name {
    font-size: 12px;
  }

  .viewed-price {
    font-size: 13px;
  }

  .view-all-wrapper {
    justify-content: center;
  }

  .view-all-btn {
    padding: 10px 20px;
    font-size: 13px;
    width: 100%;
    text-align: center;
  }
}
</style>