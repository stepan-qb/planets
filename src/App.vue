<template>
  <div :class="$options.name">
    <div class="list">
      <div
        v-for="item in list"
        :key="item.id"
        class="item"
        @click="$modal.show('planet', item)"
      >
        {{ item.name }}
      </div>
    </div>

    <planet-modal />
  </div>
</template>

<script>
import axios from 'axios';
import PlanetModal from "@/components/modal/DetailView.vue";

export default {
  name: 'app-view',
  components: {
    PlanetModal,
  },
  data: () => ({
    list: null,
  }),
  created() {
    axios('https://swapi.co/api/people/', {
      method: 'GET',
      mode: 'no-cors',
    }).then((resp) => {
      this.list = resp.data.results;
    }).catch((e) => {
      console.log(e);
    });
  },
};
</script>

<style>
.app-view {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  color: #2c3e50;
  margin-top: 60px;
}

.item {
  cursor: pointer;
  transition: color .3s ease-out;
}

.item:hover {
  color: #ccc;
}
</style>
