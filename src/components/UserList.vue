<template>
  <div>
    <div class="wrapp">
      <ul>
        <li class="card card-item" v-for="user in users" :key="user.id">
          <div class="card-title">{{ user.name }}</div>
          <a 
            class="card-contact"
            :href="'mailto:' + user.email"
            >
            Email: {{ user.email }}
          </a>
          <a 
            class="card-contact"
            :href="user.website"
            >
            Website: {{ user.website }}
          </a>
          <a 
            class="card-contact phone"
            :href="'tel:' + user.phone"
            >
            Phone number: {{ user.phone }}
          </a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  import { useUsers } from '../use/users';

  export default {
    async setup() {
      const { users } = await useUsers();
      return { users };
    },
  };
</script>
<style scoped>
  .card-title {
    font-size: 22px;
    font-weight: 700;
    color: #333;
  }

  .card-contact {
    font-size: 16px;
    color: #000;
  }

  .card-item {
    display: flex;
    flex-direction: column;
  }
  .phone {
    font-weight: 500;
    color: rgb(84, 82, 82);
  }
</style>