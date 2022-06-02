<template>
  <div class="container">
    <form class="card" @submit.prevent="submit">
      <h1>Auth</h1>
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

    <UserList/>
  </div>
</template>

<script>
  import { useForm } from './use/form';
  import {UserList} from './components/UserList.vue'
  const required = (val) => !!val;
  const minLength = (num) => (val) => val.length >= num;

export default {
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

      function submit() {
        // console.log('email', form.email.value)
        // console.log('password', form.password.value)
      }

      return { form, submit };
    },
    components: {
      UserList 
    },
  };
</script>
