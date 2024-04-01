<script>
const columns = [{
  key: 'id',
  label: '#'
}, {
  key: 'email',
  label: 'Email',
  class: 'italic'
}, {
  key: 'name',
  label: 'Name'
}, {
    key: 'surname',
    label: 'Surname'
}]
const students = [{
    id: 1,
    email: 'danyil.maksymchuk@oa.edu.ua',
    name: 'Danyil',
    surname: 'Maksymchuk'
}, {
    id: 2,
    email: 'dmytro.kovinskyi@oa.edu.ua',
    name: 'Dmytro',
    surname: 'Kovinskyi'
}, {
    id: 3,
    email: 'vladyslav.oliinyk@oa.edu.ua',
    name: 'Vladyslav',
    surname: 'Oliinyk'
}]
const page = ref(1)
const pageCount = 2

const rows = computed(() => {
  return students.slice((page.value - 1) * pageCount, (page.value) * pageCount)
})
const q = ref('')

const filteredRows = computed(() => {
  if (!q.value) {
    return students
  }

  return students.filter((person) => {
    return Object.values(person).some((value) => {
      return String(value).toLowerCase().includes(q.value.toLowerCase())
    })
  })
})
</script>
<template>
<h1>Список студентів</h1>
</template>
<style>
</style>