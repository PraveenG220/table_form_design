<template>
    <v-data-table
      :headers="headers"
      :items="userDetails"
      :items-per-page="5"
      class="elevation-1"
    ></v-data-table>
  </template>


<script>
  export default {
    data () {
      return {
        headers: [
          {
            text: 'serial Number',
            align: 'start',
            sortable: false,
            value: 'number',
          },
          { text: 'Name', value: 'name' },
          { text: 'Place', value: 'place' },
          { text: 'Email', value: 'email' },
          { text: 'Contacts', value: 'contacts' },
          { text: 'Age', value: 'age'},
        ],
        userDetails: [
          {
            number: '1',
            name: 'Praveenkumar',
            place: 'vijayapura',
            email: 'praveen@gmail.com',
            contacts: 9999999999,
            age: 27,
          },
          {
            number: '1',
            name: 'Praveenkumar',
            place: 'vijayapura',
            email: 'praveen@gmail.com',
            contacts: 9999999999,
            age: 27,
          },
          {
            number: '1',
            name: 'Praveenkumar',
            place: 'vijayapura',
            email: 'praveen@gmail.com',
            contacts: 9999999999,
            age: 27,
          },
          {
            number: '1',
            name: 'Praveenkumar',
            place: 'vijayapura',
            email: 'praveen@gmail.com',
            contacts: 9999999999,
            age: 27,
          },
          {
            number: '1',
            name: 'Praveenkumar',
            place: 'vijayapura',
            email: 'praveen@gmail.com',
            contacts: 9999999999,
            age: 27,
          },
          {
            number: '1',
            name: 'Praveenkumar',
            place: 'vijayapura',
            email: 'praveen@gmail.com',
            contacts: 9999999999,
            age: 27,
          },
          {
            number: '1',
            name: 'Praveenkumar',
            place: 'vijayapura',
            email: 'praveen@gmail.com',
            contacts: 9999999999,
            age: 27,
          },
          
          
        ],
      }
    },
  }
</script>