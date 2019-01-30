<template>
  <div :class="$options.name">
    <modal
      adaptive
      height="360"
      name="person"
      @before-open="beforeOpen"
    >
      <header>
        <h4 class="title">{{ person.name }}</h4>
        <span name="close" class="close" @click="$modal.hide('person')">+</span>
      </header>

      <div class="content">
        <div class="stats">
          <div class="stat">
            <span class="label">Возраст: </span>
            <span class="value">{{ person.birth_year }}</span>
          </div>

          <div class="stat">
            <span class="label">Пол: </span>
            <span class="value">{{ person.gender }}</span>
          </div>

          <div class="stat">
            <span class="label">Рост: </span>
            <span class="value">{{ person.height }}</span>
          </div>

          <div class="stat">
            <span class="label">Вес: </span>
            <span class="value">{{ person.mass }}</span>
          </div>

          <div v-if="person.shipsList.length" class="stat">
            <span class="label">Корабли: </span>
            <ul class="value">
              <li v-for="ship in person.shipsList" :key="ship" class="value-item"><span>{{ ship }}</span></li>
            </ul>
          </div>

          <div v-if="person.filmsList.length" class="stat">
            <span class="label">Фильмы: </span>
            <ul class="value">
              <li v-for="film in person.filmsList" :key="film" class="value-item"><span>{{ film }}</span></li>
            </ul>
          </div>
        </div>
      </div>

    </modal>
  </div>
</template>

<script>
const createDefaultData = () => ({
  name: null,
  birth_year: null,
  gender: null,
  mass: null,
  height: null,
  shipsList: [],
  filmsList: [],
});

export default {
  name: 'modal-detail-view',
  data: () => ({
    person:  createDefaultData(),
  }),
  methods: {
    beforeOpen({ params }) {
      this.person = {
        ...params,
        gender: params.gender === 'male' ? 'Мужчина' : 'Женщина',
      };
    },
  },
};
</script>

<style scoped>

.modal-detail-view {
  z-index: 1000;
}

header {
  display: flex;
  padding: 24px;
  justify-content: space-between;
}

.title {
  color: #120f12;
  margin: 0;
  font-weight: 600;
}

.content {
  padding: 0 24px;
  margin: 24px 0;
  height: 240px;
  overflow-y: auto;
}

.label {
  font-weight: 600;
  color: #4c596f;
}

.value {
  color: #120f12;
}

.value-item {
  padding-top: 6px;
}

.stat + .stat {
  margin-top: 12px;
}

.close {
  width: 24px;
  height: 24px;
  font-size: 24px;
  color: #120f12;
  flex-shrink: 0;
  transform: rotate(45deg);
  cursor: pointer;
}
</style>
