<script setup>
import { ref, onMounted } from 'vue';
const isLoading = ref(true);
const personInfo = ref('')
const personImg = ref(null)

const props = defineProps({
  showModal: Boolean,
  person: {
    type: Object,
    required: true
  }
})

onMounted(() => {
  fetch('https://cdnapi.smotrim.ru/api/v1/persons/' + props.person.id, {
    method: 'get',
    headers: { 'Content-type': 'application/json' },
  }).then((response) => {
    if (!response.ok) {
      throw Error(response.statusText);
    }

    return response.json();
  }).then((json) => {
    personInfo.value = json.data?.body

    return null;
  })
  .then(() => {
    const img = new Image();
    img.src = 'https://api.smotrim.ru/api/v1/pictures/' + props.person.picId + '/bq/redirect';

    img.onload = () => {
      personImg.value = img;
      isLoading.value = false;
    };
  })
  .catch((error) => {
    console.log('Could not fetch person info: \n', error);

    return null;
  });
})
</script>
<template>
  <div 
    v-if="showModal"
    class="modal__overlay"
    @click="$emit('close')"
  >
    <div  class="modal">
      <div class="modal__content" v-if="!isLoading">
        <div class="modal__header">
          <div class="modal__header-wrapper">
            <div class='modal__img-wrapper'>
              <img class="modal__img"
                :src="'https://api.smotrim.ru/api/v1/pictures/' + person.picId + '/bq/redirect'"
              >
            </div>
            <div class="modal__person-name">
              {{ person.name }}
              <br>
              {{ person.surname }}
            </div>
          </div>
          <div class="modal__close-btn" @click="$emit('close')"></div>
        </div>
        <div class="modal__body">
          <div v-html="personInfo" />
        </div>
      </div>
      <div class="modal__content_empty" v-else>
        <img src="../assets/loading.gif" alt="">
      </div>
    </div>
  </div>
</template>
<style>
.modal__overlay {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(5px);
}
.modal {
  width: 600px;
  height: 600px;
  background-color: #FFF;
  padding: 30px;
  border-radius: 16px;
  font-family: sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  animation:fadein .5s;
}

@keyframes fadein {
	from {
		opacity:0;
	}
	to {
		opacity:1;
	}
}

.modal__header {
  position: relative;
  display: flex;
  justify-content: space-between;
  height: 200px;
  font-weight: bold;
}

.modal__body {
  height: 300px;
  overflow: auto;
}

.modal__content {
  display: flex;
  flex-direction: column;
  gap: 40px;
}

.modal__header-wrapper {
  display: flex;
  align-items: center;
  gap: 32px;
}

.modal__person-name {
  font-size: 40px;
  text-align: left;
}

.modal__img-wrapper {
  width: 200px;
  height: 200px;
}
.modal__img {
  height: 100%;
  width: 100%;
  border-radius: 8px;
}

.modal__close-btn {
  position: absolute;
  right: 0;
  top: 0;
  width: 32px;
  height: 32px;
  opacity: 0.3;
  cursor: pointer;
}
.modal__close-btn:hover {
  opacity: 1;
}
.modal__close-btn:before, .modal__close-btn:after {
  position: absolute;
  left: 15px;
  content: ' ';
  height: 33px;
  width: 2px;
  background-color: #333;
}
.modal__close-btn:before {
  transform: rotate(45deg);
}
.modal__close-btn:after {
  transform: rotate(-45deg);
}

@media only screen and (max-width: 768px) {
  .modal {
    width: 400px;
    height: 400px;
    padding: 10px;
  }

  .modal__content {
    gap: 15px;
  }

  .modal__header {
    height: 146px;
  }

  .modal__body {
    height: 219px;
    font-size: 12px;
  }

  .modal__person-name {
    font-size: 16px;
  }

  .modal__img-wrapper {
    width: 150px;
    height: 150px;
  } 
}
</style>