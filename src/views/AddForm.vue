<script setup>
import {ref, watch, computed, onMounted} from 'vue'
import { RouterLink, useRouter} from 'vue-router'

const personsArr = ref([])
const fullName = ref('')
const birthDate = ref('')
const description = ref('')
const editPerson = ref()
const router = useRouter()

 onMounted( () => {
  if (localStorage.getItem('person')) {
    editPerson.value = JSON.parse(localStorage.getItem('person'))
    fullName.value = editPerson.value.firstName + ' ' + (editPerson.value.lastName ? editPerson.value.lastName : '') + ' ' +  (editPerson.value.middleName ? editPerson.value.middleName : '')
    birthDate.value = editPerson.value.birthDate
    description.value = editPerson.value.description
  }
})

const addPersonData = () => {
  const name = fullName.value.split(' ')
  if (name.length < 2 && !valideDate(birthDate.value)) {
    alert("Заполните все обязательные поля!")
    return
  }
  if (name.length < 2) {
    alert("Фамилия и Имя являются обязательными для заполнения!")
    return
  }
  if (!valideDate(birthDate.value)) {
    alert('Заполните поле датой рождения корректно!')
    return
  }
  if (localStorage.getItem('person')) {
  JSON.parse(localStorage.getItem('persons')).map( person => {
    if (person.id === editPerson.value.id) {
      person.id = editPerson.value.id
      person. firstName = name[0]
      person.lastName = name[1]
      person.middleName = name[2]
      person.birthDate = birthDate.value
      person.description = description.value
      localStorage.removeItem('person')
    } 
    personsArr.value.push(person)
  })
  router.push('/')
  return
  }
  if (localStorage.getItem('persons') && !localStorage.getItem('person')) {
  JSON.parse(localStorage.getItem('persons')).map( person => {
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
  } else {
      personsArr.value.push({
    id: Math.random(),
    firstName: name[0],
    lastName: name[1],
    middleName: name[2],
    birthDate: birthDate.value,
    description: description.value
  })
  }
  router.push('/')
}
const cancelHandler = () => {
  localStorage.removeItem('person')
  router.push('/')
}
function valideDate(date) {
  var valid = /^(19|20)\d\d-(0[1-9]|1[012])-(0[1-9]|[12][0-9]|3[01])/
  return valid.test(date)
}

watch(personsArr, newArr => {
  localStorage.setItem('persons', JSON.stringify(newArr))
}, {deep: true})

</script>

<template>
  <form>
    <label for="fullName"><span class="star">*</span>  Введите ФИО в формате "Иванов Иван Иванович"</label>
    <input 
      v-model="fullName"
      type="text" 
      name="fullName" 
      id="fullName" 
      placeholder="Введите ФИО"
      
      >
      <label for="birthDate"><span class="star">*</span>  Введите дату рождения в формате "ГГГГ-ММ-ДД"</label>
    <input 
      v-model="birthDate" 
      type="text" 
      name="birthDay" 
      id="birthDay" 
      placeholder="Введите дату рождения"
      />
      <label for="description">Описание</label>
    <textarea 
      v-model="description" 
      name="description" 
      id="description" 
      cols="30" 
      rows="5"
    ></textarea>
    <p>Все поля помеченные <span class="star">*</span>(звездочкой) являются обязательными для заполнения</p>
    <div class="form_btns">
        <button 
          @click="addPersonData"
        >
          Сохранить
        </button>
        <button
          @click="cancelHandler"
        >
          Отменить
        </button>
    </div>

  </form>
</template>

<style>
form {
  max-width: 562px;
  display: flex;
  flex-direction: column;
  border: 2px solid #2c3e50;
  border-radius: 5px;
  padding: 5px;
  -webkit-box-shadow: 4px 4px 8px 0px rgb(44 62 80 / 20%);
  -moz-box-shadow: 4px 4px 8px 0px rgba(44, 62, 80, 0.2);
  box-shadow: 4px 4px 8px 0px rgb(44 62 80 / 20%);
}
button {
  background: none;
  border: 1px solid #2c3e50;
  border-radius: 5px;
  margin-right: 10px;
  -webkit-box-shadow: -4px -5px 6px 3px rgb(44 62 80 / 60%) inset;
  -moz-box-shadow: -4px -5px 6px 3px rgba(44, 62, 80, 0.6) inset;
  box-shadow: -4px -5px 6px 3px rgb(44 62 80 / 60%) inset;
  padding: 6px;
}
.star {
  color: red;
  font-size: 20px;
}
</style>
