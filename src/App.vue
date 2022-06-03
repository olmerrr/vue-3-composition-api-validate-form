<template>
  <div class="page">
  <div class="container">
    <form 
      class="card" 
      @submit.prevent="submit"
      >
      <h1>Auth</h1>

      <h2 v-if="error">{{error}}</h2>
      
      <div class="form-control">
        <label for="email">Email
        </label>
          <input 
            type="email" 
            id="email" 
            :class="{ invalid: !form.email.valid && form.email.touched}"
            v-model="form.email.value"
            @blur="form.email.blur" 
          />
    
        <small v-if="form.email.errors.required && form.email.touched">
            This field required
        </small>
      </div>

      <div class="form-control">
        <label for="password"
          >Password
        </label>

          <input 
            type="password" 
            id="password" 
            :class="{ invalid: !form.password.valid && form.password.touched }"
            v-model="form.password.value"
            @blur="form.password.blur" 
            />
         <small v-if="form.password.errors.required && form.password.touched">
            This field required
        </small>
         <small v-else-if="form.password.errors.minLength && form.password.touched ">
            Min password length 6 symbols. Now is <b>{{form.password.value.length}}</b>
        </small>
      </div>

      <button 
        class="btn primary" 
        type="submit"
        :disabled="!form.valid"
        >
        Submit
      </button>
    </form>

    <Suspense v-if="submitted">
        <user-list></user-list>
        <template #fallback>
          <div class="loader"></div>
        </template>
    </Suspense>
  </div>
  </div>
</template>

<script>
  import {ref, onErrorCaptured} from 'vue'

  import UserList from './components/UserList.vue';
  import { useForm } from './use/form';
  
  const required = (val) => !!val;
  const minLength = (num) => (val) => val.length >= num;

export default {
  components: { UserList },
    setup() {
      const form = useForm({
        email: {
          value: '',
          validators: { required },
        },
        password: {
          value: '',
          validators: {
            required,
            minLength: minLength(6),
          },
        },
      });

      const submitted = ref(false)
      const error = ref()

      function submit() {
        submitted.value = true
      }

      onErrorCaptured(e => {
        error.value = e.message

      })

      return { form, submit, submitted, error };
    },
  };
</script>
