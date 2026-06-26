<script setup>
</script>

<template>
  <footer class="footer">
    <div class="container">
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

.column {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  gap: 40px;
  flex-wrap: wrap;
}

.spisok {
  display: flex;
  flex-direction: column;
  gap: 8px;
  min-width: 180px;
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


.subscribe {
  display: flex;
  flex-direction: column;
  gap: 14px;
  min-width: 260px;
  max-width: 320px;
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
  border-color: white;
  outline-color: white;
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


.socials-title {
  margin-top: 6px;
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
}



.copyright {
  font-size: 13px;
  color: white;
  opacity: 0.8;
  margin: 0;
}

.logo{
  display: flex;
  justify-content: start;

}

@media (max-width: 1024px) {
  .footer {
    padding: 30px 20px;
  }

  .column {
    gap: 30px;
  }

  .spisok {
    min-width: 150px;
  }
}

@media (max-width: 768px) {
  .footer {
    padding: 25px 15px;
  }

  .column {
    flex-direction: column;
    gap: 25px;
  }

  .spisok {
    width: 100%;
    min-width: auto;
  }

  .subscribe {
    width: 100%;
    max-width: 100%;
  }

  .subscribe-form {
    flex-direction: column;
  }

  .subscribe-input {
    width: 100%;
  }

  .subscribe-btn {
    width: 100%;
  }

  .socials {
    justify-content: center;
  }

  .footer-bottom {
    flex-direction: column;
    gap: 15px;
    text-align: center;
  }

  .logo {
    justify-content: center;
  }
}

@media (max-width: 480px) {
  .footer {
    padding: 20px 10px;
  }

  .name-spisok {
    font-size: 15px;
  }

  .text-spisok {
    font-size: 13px;
  }

  .subscribe-text {
    font-size: 13px;
    text-align: center;
  }

  .socials-title {
    text-align: center;
  }

  .copyright {
    font-size: 12px;
  }
}
</style>