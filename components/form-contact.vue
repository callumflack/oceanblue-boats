<template>
  <form action="https://formspree.io/vince@oceanblueboats.com.au" method="post" @submit.prevent="handleSubmit">
    <div class="form-group">
      <input placeholder="Your name*" name="name" v-model="name" required>
      <input placeholder="Your email*" type="email" name="email" v-model="email" required>
      <textarea rows="4" placeholder="How can we help make your dream boat?" name="message" v-model="message"/>
      <input type="hidden" name="_subject" value="New website contact!" />

      <div class="u-textRight">
        <transition name="fade">
          <span class="confirmation-message" v-if='formSent'>Thanks! We'll contact you within 48 hours.</span>
        </transition>

        <button class="lede button button-fill" type="submit" :disabled="loading || formSent">{{buttonText}}</button>
      </div>
    </div>
  </form>
</template>

<script>
export default {
  name: "form-contact",

  data() {
    return {
      name: "",
      email: "",
      message: "",
      formSent: false,
      loading: false
    };
  },

  computed: {
    buttonText() {
      if (this.loading) {
        return "Loading";
      } else if (this.formSent) {
        return "Sent";
      }

      return "Send ›";
    }
  },

  methods: {
    async handleSubmit(event) {
      this.loading = true;

      await fetch("https://formspree.io/vince@oceanblueboats.com.au", {
        method: "post",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json"
        },

        body: JSON.stringify({
          name: this.name,
          email: this.email,
          message: this.message
        })
      });

      this.loading = false;
      this.formSent = true;
    }
  }
};
</script>

<style scoped>
@import "../assets/vars.css";

.confirmation-message {
  margin-right: var(--grid-gutter);
}
</style>
