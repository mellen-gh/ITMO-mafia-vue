<template>

  <div class="container">
    <form @submit.prevent="changes">
    <div class="card">
      <h2>Работа с базой данных</h2>
      <div class="form-control">
      <label for="name">ВЫАЫВ</label>
      <input type="text" id="name" v-model.trim="name">
      </div>
      <button class="btn primary" @click="loadPeople">werwer</button>
    </div>
    </form>
    <app-people-list
        :people="people"
        @load="loadPeople"
        @remove="removePerson"
    ></app-people-list>
  </div>

</template>

<script>

import AppPeopleList from "@/AppPeopleList";
import axios from 'axios'

export default {
  data() {
    return {
      name: '',
      people: []
    }
  },
  mounted() {
    this.loadPeople()
  },
  methods: {
    async changes() {
      // const xhr = new XMLHttpRequest()
      // xhr.open('GET', 'http://itmomafia.herokuapp.com/player/')
      //
      // xhr.onload = () => {
      //   console.log(xhr.response)
      // }
      // xhr.send()

      const response = await fetch(
            'http://itmomafia.herokuapp.com/player/',{
            method: 'POST',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify({
              name: this.name
            })
          })
      const test = await response.json()
       this.people.push({
         name: this.name,
         id: test
       })
      console.log(test)
      this.name = ''
    },
    async loadPeople() {
      const {data} = await axios.get('http://itmomafia.herokuapp.com/player/')
      // const result = Object.keys(data).map(
      //   key => {
      //     return {
      //       id: data[id],
      //       name: data[name]
      //     }
      //   }
      // )

      this.people = data

    },
    async removePerson(id) {
      await axios.delete(`http://itmomafia.herokuapp.com/player/${id}`)
      this.people =  this.people.filter(person => person.id !== id)
    }
  },
  components: {AppPeopleList}
}
</script>

<style>

</style>
