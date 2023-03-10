<script setup>
import {ref, watch, computed, onMounted} from 'vue'
import { RouterLink} from 'vue-router'

const personsArr = ref([])
const fullName = ref('')
const birthDate = ref('')
const description = ref('')
 const editPerson = ref()

 onMounted( () => {
  if (localStorage.getItem('person')) {
    editPerson.value = JSON.parse(localStorage.getItem('person'))
    console.log(editPerson.value);
    fullName.value = editPerson.value.firstName + ' ' + (editPerson.value.lastName ? editPerson.value.lastName : '') + ' ' +  (editPerson.value.middleName ? editPerson.value.middleName : '')
    birthDate.value = editPerson.value.birthDate
    description.value = editPerson.value.description
  }
})

const addPersonData = () => {
  const name = fullName.value.split(' ')
  if (localStorage.getItem('persons')) {
  JSON.parse(localStorage.getItem('persons')).map( person => {
    if (person.id === editPerson.value.id) {
      person. firstName = name[0]
      person.lastName = name[1]
      person.middleName = name[2]
      person.birthDate = birthDate.value
      person.description = description.value
      console.log(person);
      localStorage.removeItem('person')
    } 
  })
  } else if (!localStorage.getItem('person')){
    JSON.parse(localStorage.getItem('persons')).map(person => {
      personsArr.value.push(person)
    })
  personsArr.value.push({
    id: Math.random(),
    firstName: name[0],
    lastName: name[1],
    middleName: name[2],
    birthDate: birthDate.value,
    description: description.value
  })
  }

const cancelHandler = () => {
  localStorage.removeItem('person')
}

watch(personsArr, newArr => {
  localStorage.setItem('persons', JSON.stringify(newArr))
}, {deep: true})

</script>

<template>
  <form>
    <input 
      v-model="fullName"
      type="text" 
      name="fullName" 
      id="fullName" 
      placeholder="Введите ФИО"
      
      >
    <input 
      v-model="birthDate" 
      type="text" 
      name="birthDay" 
      id="birthDay" 
      placeholder=" введите дату рождения"
      />
    <textarea 
      v-model="description" 
      name="description" 
      id="description" 
      cols="30" 
      rows="5"
    ></textarea>
    <RouterLink 
      to="/" 
      @click="addPersonData"
    >
      Сохранить
    </RouterLink>
    <RouterLink 
      to="/"
      @click="cancelHandler"
    >
      Отменить
    </RouterLink>
  </form>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
