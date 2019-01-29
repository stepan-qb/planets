<template>
  <div :class="$options.name">
    <modal
      adaptive
      height="360"
      name="planet"
      @before-open="beforeOpen"
    >
      <header>
        <h4 class="title">{{ name }}</h4>
        <span name="close" class="close" @click="$modal.hide('planet')">+</span>
      </header>

      <div class="content">
        <p class="description">{{ description }}</p>
      </div>

    </modal>
  </div>
</template>

<script>
export default {
  name: 'modal-detail-view',
  data: () => ({
    name: '',
    description: '',
  }),
  methods: {
    beforeOpen({ params }) {
      this.name = params.name;
      this.description = params.description;
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
  margin: 0;
  font-weight: 600;
}

.content {
  padding: 0 24px;
  margin: 24px 0;
  height: 240px;
  overflow-y: auto;
}

.close {
  width: 24px;
  height: 24px;
  font-size: 24px;
  flex-shrink: 0;
  transform: rotate(45deg);
  cursor: pointer;
}
</style>
