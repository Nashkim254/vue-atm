<template>
  <div>
    <form @submit.prevent="submit">
      <h1>Welcome</h1>
      <v-field label="Card" :v="$v.card">
        <input
          v-model="card"
          v-cardformat:formatCardNumber
          @blur="$v.card.$touch()"
          placeholder="4242 4242 4242 4242"
        />
      </v-field>
      <v-field label="Pin" :v="$v.pin">
        <input v-model="pin" @blur="$v.pin.$touch()" />
      </v-field>
      <button type="submit" :class="{ invalid: $v.$invalid }">Enter</button>
    </form>
  </div>
</template>

<script>
import { required, maxLength, minLength } from "vuelidate/lib/validators";
import VField from "./V-field.vue";
import Vue from "vue";
import router from "../router/index";
import { mapActions } from "vuex";

export default {
  components: {
    VField
  },
  data() {
    return {
      card: "",
      pin: "",
      $cardFormat: Vue.prototype.$cardFormat
    };
  },
  validations: {
    card: {
      required,
      cardFormat(value, vm) {
        const res = vm.$cardFormat.validateCardNumber(value);
        return Boolean(res);
      }
    },
    pin: {
      required,
      minLength: minLength(4),
      maxLength: maxLength(4)
    }
  },
  methods: {
    ...mapActions({
      login: "login"
    }),
    submit() {
      this.$v.$touch();
      if (!this.$v.$error) {
        this.login();
        router.push({ name: "AccountOptions" });
      }
    }
  }
};
</script>

<style lang="scss" scoped>
form {
  padding: 40px;
  width: 400px;
  height: 400px;
  margin-bottom: 10px;
  display: inline-block;
  border: 1px solid grey;
}
h1 {
  margin-top: 0px;
}
input {
  display: block;
  width: 100%;
  box-sizing: border-box;
  border: 1px solid lightgrey;
  padding: 15px;
}

button {
  padding: 5px 15px;
}

button.invalid {
  opacity: 0.5;
}
</style>
