<script setup>
import TableRow from '../components/TableRow.vue'
import { ref , onMounted} from 'vue';
import { useRouter} from 'vue-router'

const router = useRouter()
  const persons = ref(JSON.parse(localStorage.getItem('persons')))

const editingHandler = (person) => {
  // console.log('click', person)
  localStorage.setItem('person', JSON.stringify(person))
  router.push('/add_form')
}
const deleteHandler = (person) => {
  const del = confirm('Запись будет удалена. Уверены?')
  if (del) {
    persons.value = persons.value.filter (p => p!== person)
  localStorage.setItem('persons', JSON.stringify(persons.value));
  }

}

</script>

<template>
  <main>
    <table >
      <tr>
        <th>
          Фамилия
        </th>
        <th>
          Имя
        </th>
        <th>
          Отчетсво
        </th>
        <th>
          Дата рождения
        </th>
        <th>
          Описание
        </th>
        <th>
          Действия
        </th>
      </tr>

        <TableRow 
          v-for="(person, id) in persons" 
          :key="id"
          :person="person"
          v-on:editing-person="editingHandler"
          v-on:delete-person="deleteHandler(person)"
        />


    </table>
  </main>
</template>

<style>
  table {
    width: 85vw;
    border-collapse: collapse;
    border: 2px ridge #2c3e50;
    empty-cells: hide;
    text-align: center;
  }
  th, td {
    border: 1px solid #2c3e50;
    padding: 5px 5px;
  }
  th {
    font-weight: 600;
  }
</style>