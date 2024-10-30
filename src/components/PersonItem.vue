<script setup>
defineProps({
  person: {
    type: Object,
    required: true
  }
})
</script>

<template>
  <div class='person-item'>
    <div class='person-img-wrapper'>
      <img class="person-img"
        :src="'https://api.smotrim.ru/api/v1/pictures/' + person.picId + '/bq/redirect'"
      >
    </div>
    <span class="person-name">
      {{ person.name }}
      <br>
      {{ person.surname }}
    </span>
  </div>
</template>

<style scoped>
.person-item {
  width: 160px;
  height: 208px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  font-size: 14px;
  font-family: sans-serif;
  cursor: pointer;
}

.person-img-wrapper {
  width: 144px;
  height: 144px;
}
.person-img {
  height: 100%;
  width: 100%;
  border-radius: 50%;
  background-image: url('../assets//loading.gif');
  background-repeat: no-repeat;
  background-position: center;
}
.person-name {
  text-align: center;
}
</style>
