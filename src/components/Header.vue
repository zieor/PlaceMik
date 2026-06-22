<script setup>

</script>

<template>
  <div class="Upheader">
    <div class="customers">
      <div class="customers1">Покупателям</div>
      <div class="customers2">Поставщики</div>
      <div class="customers3">Частые вопросы</div>
    </div>

    <div class="language">
      <img class="language1" src="/images/Header/ru.svg">
      <div class="language2">Русский</div>
      <div class="language2">₽</div>
    </div>

  </div>

  <div>

  </div>
</template>

<style scoped>
.Upheader {
  display: flex;
  flex-direction: row;
}

.customers {
  margin: 0 auto;
  display: flex;
  width: 100%;
  flex-direction: row;
  justify-content: center;
}

.language {

  display: flex;
  flex-direction: row;
  justify-content: end;
}

.language1 {
  padding-right: 6px ;
}
</style>