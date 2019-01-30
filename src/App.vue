<template>
  <div :class="$options.name">
    <transition v-if="list && list.length" name="fade" appear>
      <div class="list">
        <div
          v-for="item in list"
          :key="item.id"
          class="item"
          @click="showPerson(item)"
        >
          {{ item.name }}
        </div>
      </div>
    </transition>

    <div v-else class="spinner"></div>

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
    current: null,
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
  methods: {
    showPerson(person) {
      if (!this.current || this.current.name !== person.name) {
        this.current = person;
        this.current.shipsList = [];
        this.current.filmsList = [];
        let shipsActions = [];
        let filmsActions = [];
        person.starships.forEach(url => {
          shipsActions.push(this.getShip(url));
        });

        person.films.forEach(url => {
          filmsActions.push(this.getFilm(url));
        });
        Promise.all([...shipsActions, ...filmsActions])
          .then(() => {
            this.$modal.show('person', this.current);
          })
          .catch((e) => {
            console.log(e);
          });
      } else {
        this.$modal.show('person', this.current);
      }
    },
    async getShip(url) {
      await axios(url, {
        method: 'GET',
        mode: 'no-cors',
      }).then((resp) => {
        this.current.shipsList.push(resp.data.name);
      }).catch((e) => {
        console.log(e);
      });
    },
    async getFilm(url) {
      await axios(url, {
        method: 'GET',
        mode: 'no-cors',
      }).then((resp) => {
        this.current.filmsList.push(resp.data.title);
      }).catch((e) => {
        console.log(e);
      });
    },
  },
};
</script>

<style>
.app-view {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  color: #4c596f;
  background-color: #b9cadc;
  height: 100%;
}

.list {
  padding-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.item {
  padding: 12px;
  cursor: pointer;
  transition: color .3s ease-out;
}

.item:hover {
  color: #668ebb;
}

.spinner {
  position: absolute;
  width: 125px;
  height: 125px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.spinner:before, .spinner:after {
  content: "";
  display: block;
  position: absolute;
  border-width: 4px;
  border-style: solid;
  border-radius: 50%;
}

.spinner:before {
  width: 117px;
  height: 117px;
  border-bottom-color: #3d3339;
  border-right-color: #3d3339;
  border-top-color: rgba(33, 33, 33, 0);
  border-left-color: rgba(33, 33, 33, 0);
  top: 0;
  left: 0;
  -webkit-animation: rotate-animation 1s linear 0s infinite;
  animation: rotate-animation 1s linear 0s infinite;
}
.spinner:after {
  width: 81.9px;
  height: 81.9px;
  border-bottom-color: #ad846e;
  border-right-color: #ad846e;
  border-top-color: rgba(33, 33, 33, 0);
  border-left-color: rgba(33, 33, 33, 0);
  top: 17.55px;
  left: 17.55px;
  -webkit-animation: anti-rotate-animation 0.85s linear 0s infinite;
  animation: anti-rotate-animation 0.85s linear 0s infinite;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity .5s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

@-webkit-keyframes rotate-animation {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
@keyframes rotate-animation {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
    transform: rotate(360deg);
  }
}
@-webkit-keyframes anti-rotate-animation {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(-360deg);
    transform: rotate(-360deg);
  }
}
@keyframes anti-rotate-animation {
  0% {
    -webkit-transform: rotate(0deg);
    transform: rotate(0deg);
  }
  100% {
    -webkit-transform: rotate(-360deg);
    transform: rotate(-360deg);
  }
}
</style>
