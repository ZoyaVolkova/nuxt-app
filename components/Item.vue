<template>
  <div class="items">
    <div v-for="item in items" :key="item.name" :id="item.name" class="item">
      <img class="image" :src="item.link" />
      <img
        class="delete"
        src="../static/delete.png"
        @click="$emit('delete', $event)"
      />
      <div class="name">{{ item.name }}</div>
      <div class="description">{{ item.description }}</div>
      <div class="price">
        {{ new Intl.NumberFormat("ru-RU").format(item.price) }} руб.
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "item",
  props: {
    items: {
      type: Array,
      default: () => [],
    },
  },

  methods: {},
};
</script>

<style scoped lang="scss">
.items {
  display: flex;
  flex-wrap: wrap;
  margin-top: 51px;
}
.item {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  width: 332px;
  height: 423px;
  background: #fffefb;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04),
    0px 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
  font-family: "Source Sans Pro", sans-serif;
  margin-left: 16px;
  margin-bottom: 16px;
  position: relative;
}
.item:hover {
  cursor: pointer;
  .delete {
    visibility: visible;
    opacity: 1;
  }
}
.delete {
  width: 32px;
  height: 32px;
  position: absolute;
  top: -10px;
  right: -10px;
  visibility: hidden;
  opacity: 0;
}
.item > div {
  margin-left: 16px;
  margin-right: 16px;
}
.name {
  margin-top: 16px;
  font-weight: 600;
  margin-bottom: 16px;
  font-size: 20px;
}
.description {
  flex: 1 0 auto;
  overflow-wrap: break-word;
  width: 300px;
  font-size: 16px;
}
.price {
  margin-top: 32px;
  margin-bottom: 24px;
  font-weight: 600;
  font-size: 24px;
}
.image {
  width: 100%;
  height: 200px;
  border-radius: 4px 4px 0px 0px;
}
</style>
