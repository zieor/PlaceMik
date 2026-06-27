<script setup>
</script>

<template>
  <footer class="footer">
    <div class="container">
      <div class="footer-top">
        <div class="column">
          <div class="spisok">
            <h3 class="name-spisok">Покупателям</h3>
            <router-link to="/work" class="text-spisok">Как это работает</router-link>
            <router-link to="/def" class="text-spisok">Защита покупателя</router-link>
            <router-link to="/pay" class="text-spisok">Условия оплаты</router-link>
            <router-link to="/us" class="text-spisok">Условия использования</router-link>
            <router-link to="/acc" class="text-spisok">Регистрация аккаунта</router-link>
          </div>

          <div class="spisok">
            <h3 class="name-spisok">Поставщикам</h3>
            <router-link to="/work" class="text-spisok">Как стать продавцом</router-link>
            <router-link to="/def" class="text-spisok">Правила участия</router-link>
            <router-link to="/pay" class="text-spisok">Личный кабинет продавца</router-link>
          </div>

          <div class="spisok">
            <h3 class="name-spisok">О компании</h3>
            <router-link to="/work" class="text-spisok">О PlaceMik</router-link>
            <router-link to="/def" class="text-spisok">Новости</router-link>
            <router-link to="/pay" class="text-spisok">Часто задаваемые вопросы</router-link>
            <router-link to="/us" class="text-spisok">Служба поддержки</router-link>
            <router-link to="/acc" class="text-spisok">Сообщить о нарушении авторских прав</router-link>
          </div>
        </div>

        <div class="subscribe-section">
          <div class="subscribe">
            <p class="subscribe-text">Хотите быть в курсе последних новостей и акций?</p>

            <div class="subscribe-form">
              <input
                  type="email"
                  class="subscribe-input"
                  placeholder="konstantinkonstantinov@yandex.ru"
              />
              <button type="button" class="subscribe-btn">Подписаться</button>
            </div>
          </div>

          <div class="socials-wrapper">
            <h3 class="name-spisok socials-title">Мы в соцсетях</h3>

            <div class="socials">
              <a href="#" class="social-link" aria-label="VK">
                <img src="/images/Footer/vk.png" alt="VK" />
              </a>
              <a href="#" class="social-link" aria-label="Instagram">
                <img src="/images/Footer/instagram.png" alt="Instagram" />
              </a>
              <a href="#" class="social-link" aria-label="Facebook">
                <img src="/images/Footer/facebook.png" alt="Facebook" />
              </a>
            </div>
          </div>
        </div>
      </div>

      <div class="footer-bottom">
        <div class="logo">
          <img src="/images/Footer/logo.png" alt="PlaceMik" />
        </div>
        <p class="copyright">© PlaceMik.ru, 2021</p>
      </div>
    </div>
  </footer>
</template>

<style scoped>
.footer {
  width: 100%;
  background-image: url("/images/Footer/Footer_back.png");
  background-size: cover;
  background-repeat: no-repeat;
  min-height: 300px;
  display: flex;
  justify-content: center;
  padding: 38px 42px;
  box-sizing: border-box;
}

.container {
  width: 100%;
  max-width: 1286px;
  display: flex;
  flex-direction: column;
  gap: 35px;
}

.footer-top {
  display: grid;
  grid-template-columns: 3fr 2fr;
  gap: 40px;
}

.column {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 40px;
}

.spisok {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.name-spisok {
  font-size: 16px;
  color: white;
  margin: 0 0 5px 0;
  font-weight: 600;
}

.text-spisok {
  text-decoration: none;
  font-size: 14px;
  color: white;
  padding-bottom: 1px;
  transition: opacity 0.2s;
}

.text-spisok:hover {
  opacity: 0.8;
}

.subscribe-section {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.subscribe {
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.subscribe-text {
  font-size: 14px;
  color: white;
  margin: 0;
  line-height: 1.4;
}

.subscribe-form {
  display: flex;
  gap: 8px;
}

.subscribe-input {
  flex: 1;
  padding: 10px 14px;
  font-size: 13px;
  color: white;
  background: transparent;
  border: 1px solid white;
  outline: none;
  min-width: 0;
}

.subscribe-input::placeholder {
  color: rgba(255, 255, 255, 0.7);
}

.subscribe-btn {
  padding: 10px 18px;
  font-size: 13px;
  font-weight: 600;
  color: #4a6cf7;
  background: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  white-space: nowrap;
  transition: opacity 0.2s;
}

.subscribe-btn:hover {
  opacity: 0.85;
}

.socials-wrapper {
  display: flex;
  flex-direction: column;
  gap: 14px;
}

.socials-title {
  margin: 0;
}

.socials {
  display: flex;
  gap: 16px;
  align-items: center;
}

.social-link {
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.2s;
}

.social-link:hover {
  transform: scale(1.15);
}

.social-link img {
  width: 24px;
  height: 24px;
  object-fit: contain;
}

.footer-bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 20px;
  border-top: 1px solid rgba(255, 255, 255, 0.2);
}

.logo {
  display: flex;
  justify-content: start;
}

.logo img {
  height: 40px;
  width: auto;
}

.copyright {
  font-size: 13px;
  color: white;
  opacity: 0.8;
  margin: 0;
}

@media (max-width: 992px) {
  .footer-top {
    grid-template-columns: 1fr;
  }

  .column {
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
  }
}

@media (max-width: 768px) {
  .column {
    grid-template-columns: repeat(2, 1fr);
  }

  .subscribe-form {
    flex-direction: column;
  }
}

@media (max-width: 480px) {
  .column {
    grid-template-columns: 1fr;
  }

  .footer-bottom {
    flex-direction: column;
    gap: 15px;
    text-align: center;
  }
}

@media (max-width: 375px) {
  .footer {
    padding: 25px 15px;
  }

  .container {
    gap: 25px;
  }

  .column {
    gap: 15px;
  }

  .name-spisok {
    font-size: 14px;
  }

  .text-spisok {
    font-size: 13px;
  }

  .subscribe-text {
    font-size: 13px;
  }

  .subscribe-input {
    font-size: 12px;
    padding: 8px 12px;
  }

  .subscribe-btn {
    font-size: 12px;
    padding: 8px 14px;
    width: 100%;
    box-sizing: border-box;
  }

  .social-link img {
    width: 22px;
    height: 22px;
  }

  .footer-bottom {
    padding-top: 15px;
  }

  .logo img {
    height: 35px;
  }

  .copyright {
    font-size: 12px;
  }
}
</style>