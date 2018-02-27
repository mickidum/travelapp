<template>

  <form class="">
        <div class="row">
          <div class="s12 col center-align">
            <h4><strong>{{ buttonText }}</strong></h4>
          </div>
        </div>

        <div class="row">
          <div class="input-field col s12 l6">
            <input placeholder="name" v-model="model.name" id="contact-name" type="text" class="validate">
            <label for="contact-name">Name</label>
          </div>
          <div class="input-field col s12 l6">
            <input placeholder="Profession" v-model="model.skill" id="contact-prof" type="text" class="validate">
            <label for="contact-prof">Profession</label>
          </div>
          <div class="input-field col s12 l6">
            <input placeholder="Company" v-model="model.company" id="contact-company" type="text" class="validate">
            <label for="contact-company">Company</label>
          </div>
          <div class="input-field col s12 l6">
            <input placeholder="Address" v-model="model.address" id="contact-address" type="text" class="validate">
            <label for="contact-address">Address</label>
          </div>
          <div class="input-field col s12 l6">
            <input placeholder="City" v-model="model.city" id="contact-city" type="text" class="validate">
            <label for="contact-city">City</label>
          </div>
          <div class="input-field col s12 l6">
            <input placeholder="Phone" data-error="wrong number bitch" v-model="model.phone" id="contact-phone" type="tel" class="validate">
            <label for="contact-phone">Phone</label>
          </div>
          <div class="input-field col s12 l6">
             <button @click.prevent="update" class="btn waves-effect waves-light" type="submit" name="action">{{ buttonText }}
              <i class="material-icons right">send</i>
            </button>
            <button @click.prevent="cancel" class="btn waves-effect waves-light" type="submit" name="action">Cancel
              <i class="material-icons right">cancel</i>
            </button>
          </div>
        </div>
      </form>

</template>

<script>
export default {
  name: 'contact-form',
  props: ['model', 'buttonText','valid'],
  // data() {
  //   return {
  //     valid: 1,
  //   }
  // },
  
  methods: {
    update() {
      this.$emit('update-contact');
    },
    cancel() {
      this.$emit('cancel-contact');
    },
    validate() {
      this.$emit('validate-contact');
    }
  }
}
</script>
