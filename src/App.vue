<script setup>
import { ref, onMounted } from 'vue'

import PersonList from './components/PersonList.vue'
import PersonModal from './components/PersonModal.vue'

const persons = ref([])
const showModal = ref(false)
const currentPerson = ref(null)

const onShowModal = (person) => {
  currentPerson.value = person
  showModal.value = true;
}

const onCloseModal = (person) => {
  currentPerson.value = null;
  showModal.value = true;
}

onMounted(() => {
  fetch('https://cdnapi.smotrim.ru/api/v1/boxes/vesti2', {
    method: 'get',
    headers: { 'Content-type': 'application/json' },
  }).then((response) => {
    if (!response.ok) {
      throw Error(response.statusText);
    }

    return response.json();
  }).then((json) => {
    const fetchedPersons = json.data?.content?.find(c => c.title === 'Персоны');
    persons.value = fetchedPersons.content;

    return null;
  })
  .catch((error) => {
    console.log('Could not fetch persons: \n', error);

    return null;
  });
});
</script>



<template>
  <div class="app-wrapper">
    <PersonList :persons="persons" @showModal="onShowModal"/>
    <PersonModal
      v-if="currentPerson"
      :showModal="showModal"
      :person="currentPerson"
      @close="onCloseModal"
    />
  </div>

</template>

<style scoped>
.app-wrapper {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
