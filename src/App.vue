<template>
  <div id="app" class="containera">
    <div class="row">
      
    <div class="col s12 l6 xl10">
      <div class="row">
        <contact
         @delete-contact="deleteContact(contact)" 
         v-for="contact in contacts" 
         :key="contact.id" 
         :contact="contact"
         @edit-contact="editContact(contact)">
        </contact>
      </div>
    </div>

    <div class="col s12 l6 xl2">
        <div class="card-panel z-depth-1 contact adding">
          <div class="inner teal-text text-lighten-4">
           <i @click="addContact(model)" class="large material-icons">add_circle_outline</i>
          </div>
        </div>
      </div>
    </div>
    


    <!-- FORM YO Yo -->
    <div id="modal1" class="modal">
      <contact-form 
      :valid = "valid"
      :model="model" 
      :buttonText="buttonText"
      @update-contact="updateContact(model)"
      @cancel-contact="cancelContact(model)"
      @validate-contact="validateContact(model)"
      ></contact-form>
    </div>

  </div>
</template>

<script>
import axios from 'axios'
import Contact from './Contact.vue'
import ContactForm from './ContactForm.vue'
import {contacts} from './data.js'
export default {
  name: 'app',
  components: { Contact, ContactForm },
  data () {
    return {
      contacts: contacts,
      initModel: {},
      valid: false,
      buttonText: 'Contact',
        model: {
          id: '',
          name: '',
          skill: '',
          location: {},
          company: '',
          address: '',
          city: '',
          phone: '',
          img: 'alex_jonathan.jpg'
        }
      }
  },
  created() {
    if(window.localStorage.getItem('contacts')) {
      this.contacts = JSON.parse(window.localStorage.getItem('contacts'))
    }
    else {
      window.localStorage.setItem('contacts', JSON.stringify(this.contacts))
    }
    this.contacts.map(contact => this.getGps(contact))
  },
  methods: {
    editContact(contact) {
      this.buttonText = 'Edit Contact'
      this.initModel = Object.assign({}, contact) 
      this.model = contact
      this.valid = true
      this.validateContact(contact)
      $('#modal1').modal('open')
    },
    deleteContact(contact) {
      this.contacts.splice(this.contacts.indexOf(contact), 1);
      this.updateLocal()
    },
    addContact(contact) {
      this.buttonText = 'Create New Contact'
      this.model = contact
      $('#modal1').modal('open');
    },
    cancelContact(contact) {
      this.model = this.initModel
      if(this.model.id) {
        this.contacts.splice(this.contacts.indexOf(contact), 1, this.initModel);
      }
      this.initModel = {}
      this.model = {}
      $('#modal1').modal('close');
    },
    updateContact(contact) {
      if(this.validateContact(contact)) {
      this.getGps(contact)
      if(this.model.id) {
        this.contacts.splice(this.contacts.indexOf(contact), 1, this.model);
      }
      else {
        this.model.id = Number(Date.now())
        this.contacts.unshift(this.model);
      }
      this.initModel = {}
      this.model = {}
      this.updateLocal()
      $('#modal1').modal('close');
      }
    },
    getGps(contact) {
      contact.city = contact.city || ''
      contact.address = contact.address || ''
      let address = (contact.city + contact.address) || 'paris'
      const api = 'https://maps.googleapis.com/maps/api/geocode/json?address=' + address +'&key=AIzaSyDKvvBgAkSCugEbXckutuAFuqPzthsCnJ8'

      axios.get(api)
      .then((response) => {
        contact.location = response.data.results[0].geometry.location
      })
      .catch((error) => {
        console.log(error);
        contact.location = {
          lat: 'Shit',
          lng: 'Happened'
        }
      })
    },
    validateContact(contact) {
        const regex = /^[+]*[(]{0,1}[0-9]{1,3}[)]{0,1}[-\s\./0-9]*$/g;
        if (!regex.test(contact.phone)) {
            alert("Wrong phone no");
            return false
        }
        else return true
    },
    updateLocal() {
      window.localStorage.setItem('contacts', JSON.stringify(this.contacts))
    }
  }
}
</script>

<style lang="scss">
body {
  background-color: silver;
}
.modal {
  padding: 15px;
}
.contact {
  position: relative;
  height: 230px;
  &.adding {
    display: table;
    width: 100%;
    text-align: center;
    .inner {
      display: table-cell;
      vertical-align: middle;
    }
  }
  i {
    cursor: pointer;
  }
  h5, h4 {
    font-weight: bold;
    margin: 10px 0;
  }
  h4 {
    font-size: 20px;
  }
  h5 {
    font-size: 16px;
    margin-bottom: 0;
  }
  p {
    margin: 6px 0;
    line-height: 1;
    &.location {
      margin: 10px 0 15px;
      i, span {
        display: inline-block;
        vertical-align: middle;
      }
      i {
        margin-top: -2px;
        font-size: 15px;
        margin-right: 5px;
      }
    }
  }
}
.skill {
  display: inline-block;
  line-height: 1;
  font-weight: bold;
  margin-top: 8px;
  font-size: 16px;
}
.buttons-block {
  position: absolute;
  bottom: 10px;
  right: 15px;
  i {
    display: inline-block;
    font-size: 20px;
    margin-left: 3px;
    vertical-align: middle;
  }
}
</style>
