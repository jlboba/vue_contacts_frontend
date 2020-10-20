// HTML TEMPLATE
<template>
  <h1>Contacts</h1>
  <ul>
    <li 
      v-for="contact in contacts" 
      :key="contact.id"
    >
      {{contact.name}}, {{contact.age}} 

      <button 
        @click="deleteContact" 
        :value="contact.id">
        Delete
      </button>

      <details><summary>Edit</summary>
        <ContactForm 
          :contact-id="contact.id" 
          :current-name="contact.name" 
          :current-age="contact.age" @form-submitted="editContact"
        />
      </details>

    </li>
  </ul>
  <h2>Add Contact</h2>
  <ContactForm @form-submitted="createContact"/>
</template>

// VUE APP
<script>
// -- dependencies 
import axios from 'axios'

// -- components 
import ContactForm from './components/ContactForm'

// -- app config
export default {
  name: 'App',
  components: {
    ContactForm
  },
  data: function() {
    return {
      contacts: []
    }
  },
  created: function() {
    this.getContacts()
  },
  methods: {
    getContacts: function() {
      axios.get(process.env.VUE_APP_API_URL).then(res => {
        this.contacts = res.data
      })
    },
    deleteContact: function(e) {
      axios.delete(`${process.env.VUE_APP_API_URL}/${e.target.value}`).then(() => {
        this.getContacts()
      });
    },
    createContact: function(contactInfo) {
      axios.post(process.env.VUE_APP_API_URL, contactInfo).then(res => {
        this.contacts.push(res.data)
      })
    }, 
    editContact: function(updateInfo) {
      axios.put(`${process.env.VUE_APP_API_URL}/${updateInfo.id}`, updateInfo).then(() => {
        this.getContacts()
      })
    }
  }
}
</script>

//  CSS (GLOBAL)
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
