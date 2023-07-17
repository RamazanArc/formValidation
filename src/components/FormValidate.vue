<template>
  <div class="root">

    <h2> Hesap Oluştur</h2>
    <p>
      <input type="text" placeholder="Email" v-model="state.email" />
      <span v-if="v$.email.$error">
      {{ v$.email.$errors[0].$message}}
    </span>
    </p>
    <p>
      <input type="password" placeholder="Password" v-model="state.password.password" />
      <span v-if="v$.password.password.$error">
      {{ v$.password.password.$errors[0].$message}}
    </span>
    </p>
    <p>
      <input type="password" placeholder="Conifrm Password" v-model="state.password.confirm" />
      <span v-if="v$.password.confirm.$error">
      {{ v$.password.confirm.$errors[0].$message}}
    </span>
    </p>
    <button @click="submitForm">Onayla</button>

  </div>

</template>


<script>
import useVuelidate from "@vuelidate/core"
import { required, email, minLength, sameAs } from "@vuelidate/validators"
import { reactive, computed } from "vue"

export default {
  components: {},
  setup(){
  const state = reactive({
    email: '',
    password: {
      password: '',
      confirm: '',
    },
  })

    const rules = computed(() => {
    return{
      email: {required, email},
      password: {
        password: {required, minLength: minLength(8)},
        confirm: {required, sameAs: sameAs(state.password.password)},
      },
    }
    })

    const v$ = useVuelidate(rules,state)

    return{
    state,
      v$,
    }
  },
  methods:{
      submitForm() {
        this.v$.$validate()
        if (!this.v$.$error) {
        alert("Form Onaylandı")
        } else {
          alert("Form Onaylanmadı")
        }
      },
  },
}
</script>

<style scoped>

.root {
  width: 400px;
  margin: 0 auto;
  background-color: #fff;
  padding: 30px;
  margin-top: 100px;
  border-radius: 20px;
}

input {
  border: none;
  outline: none;
  border-bottom: 1px solid #ddd;
  font-size: 1em;
  padding: 5px 0;
  margin: 10px 0 5px 0;
  width: 100%;
}

button {
  background-color: #3498db;
  padding: 10px 20px;
  margin-top: 10px;
  border: none;
  color: white;
}
</style>