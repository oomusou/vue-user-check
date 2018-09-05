<template>
  <div>
    <label>Username</label>
    <input type="text" v-model="username">
    <p></p>
    <span v-if="exists">{{ username }} is member</span>
  </div>
</template>

<script>
import axios from 'axios';
import _ from 'lodash';
import { API } from '../environment';

export default {
  name: 'UserCheck',
  data() {
    return {
      username: '',
      exists: false,
    };
  },
  methods: {
    submit() {
      if (_.isEmpty(this.username)) {
        return;
      }

      const endpoint = `${API}/api/exists/${this.username}`;

      const response = (res) => {
        this.exists = res.data.exists;
      };

      const error = e => console.log(e);

      axios
        .get(endpoint)
        .then(response)
        .catch(error);
    },
  },
  watch: {
    username() {
      this.debouncedSubmit();
    },
  },
  created() {
    this.debouncedSubmit = _.debounce(this.submit, 200);
  },
};
</script>

<style scoped>

</style>
