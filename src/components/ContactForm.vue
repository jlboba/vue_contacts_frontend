<template>
  <form @submit.prevent="handleSubmit">
    <input type="text" placeholder="name" v-model="name"/>
    <input type="number" placeholder="age" v-model="age"/>
    <input type="submit"/>
  </form>
</template>

<script>
export default {
  name: 'ContactForm',
  data: function() {
    return {
      name: '',
      age: 0,
      id: null
    }
  },
  props: {
    'currentName': String,
    'currentAge': Number,
    'contactId': Number
  },
  created: function() {
    // if there's a currentName prop sent done, prefill the values
    if(this.currentName) {
      this.name = this.currentName
      this.age = this.currentAge
      this.id = this.contactId
    }
  },
  methods: {
    handleSubmit: function() {
        this.$emit('form-submitted', {
          name: this.name,
          age: parseInt(this.age),
          id: this.id
        })
    }
  }
}
</script>

//  CSS (SCOPED)
// -- meaning it only applies to this component
<style scoped>
  input {
    border-radius: 5px;
    border: 1px solid #ccc;
  }
</style>
