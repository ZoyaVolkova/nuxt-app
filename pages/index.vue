<template>
  <div class="container">
    <div>
      <div class="title">Добавление товара</div>
      <Card
        @submit.prevent="sendForm"
        @inputName="name = $event"
        @inputLink="link = $event"
        @inputPrice="price = $event"
        :isActive="isActive"
      />
    </div>
    <div class="sort">
      <div
        class="select default"
        @click="toggleDropdown"
        v-click-outside="hideDropdown"
      >
        По умолчанию
      </div>
      <svg
        @click="toggleDropdown"
        class="arrow"
        width="8"
        height="6"
        viewBox="0 0 8 6"
        fill="none"
      >
        <path
          d="M7.48532 1.24264L4.24268 4.48528L1.00003 1.24264"
          stroke="#B4B4B4"
        />
      </svg>
      <div class="dropdown hidden">
        <div class="select default" @click="sortDefault">По умолчанию</div>
        <div class="select expensive" @click="sortExpensive">
          Сначала дорогие
        </div>
        <div class="select cheap" @click="sortCheap">Сначала дешёвые</div>
        <div class="select by-name" @click="sortByName">По наименованию</div>
      </div>
    </div>
    <Item :items="items" @delete="deleteItem" />
  </div>
</template>

<script>
import ClickOutside from "vue-click-outside";
export default {
  data() {
    return {
      items: [],
      name: "",
      link: "",
      price: "",
    };
  },

  directives: {
    ClickOutside,
  },
  mounted() {
    if (window.localStorage.length > 0) {
      const storage = JSON.parse(window.localStorage.getItem("items"));
      this.items = storage;
    }
  },
  methods: {
    sendForm(event) {
      const data = new FormData(event.target);
      const dataArr = Array.from(data).filter((item) => !!item[1]);
      this.items.push(Object.fromEntries(dataArr));
      window.localStorage.setItem("items", JSON.stringify(this.items));
      this.name = "";
      this.link = "";
      this.price = "";
      event.target.reset();
    },
    deleteItem(event) {
      this.items = this.items.filter((item) => {
        return item.name !== event.target.closest(".item").id;
      });
      window.localStorage.setItem("items", JSON.stringify(this.items));
    },
    toggleDropdown() {
      this.$el.querySelector(".dropdown").classList.toggle("hidden");
    },
    hideDropdown() {
      this.$el.querySelector(".dropdown").classList.add("hidden");
    },
    sortExpensive(e) {
      e.target.closest(".sort").querySelector(".default").textContent =
        e.target.textContent;

      this.items = this.items.sort((a, b) => {
        return b.price - a.price;
      });
    },
    sortCheap(e) {
      e.target.closest(".sort").querySelector(".default").textContent =
        e.target.textContent;
      this.items = this.items.sort((a, b) => {
        return a.price - b.price;
      });
    },
    sortByName(e) {
      e.target.closest(".sort").querySelector(".default").textContent =
        e.target.textContent;
      this.items = this.items.sort((a, b) => {
        return a.name.localeCompare(b.name);
      });
    },
    sortDefault(e) {
      e.target.closest(".sort").querySelector(".default").textContent =
        e.target.textContent;
      if (window.localStorage.length > 0) {
        const storage = JSON.parse(window.localStorage.getItem("items"));
        this.items = storage;
      }
    },
  },
  computed: {
    isActive() {
      return this.name !== "" && this.link !== "" && this.price !== "";
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Source+Sans+Pro:wght@400;600&display=swap");
body {
  background: #faf9f7;
}
.container {
  display: flex;
  font-family: "Source Sans Pro", sans-serif;
  width: 1440px;
  background: #faf9f7;
  position: relative;
}
.title {
  width: 247px;
  height: 35px;
  margin-left: 32px;
  margin-top: 32px;

  font-style: normal;
  font-weight: 600;
  font-size: 28px;
  line-height: 35px;

  color: #3f3f3f;
}
.sort {
  position: absolute;
  top: 31px;
  right: 32px;
}
.select {
  height: 36px;
  padding-left: 16px;
  padding-top: 10px;
  padding-bottom: 11px;
  line-height: 15px;
}
.select:hover {
  color: #868686;
}
.select,
.dropdown {
  box-sizing: border-box;
  width: 132px;
  font-size: 12px;
  color: #b4b4b4;
  font-weight: 400;
  background: #fffefb;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
  border: none;
  cursor: pointer;
}
.dropdown {
  list-style: none;
  display: flex;
  flex-direction: column;
  position: absolute;
  right: 0;
  top: 37px;
  opacity: 1;
  visibility: visible;
  transition: opacity 0.6s;
  z-index: 5;
}
.hidden {
  visibility: hidden;
  opacity: 0;
}
.cheap {
  border-radius: 0px;
}
.default {
  border-radius: 4px 4px 0px 0px;
}
.by-name {
  border-radius: 0px 0px 4px 4px;
}
.arrow {
  position: absolute;
  top: 16px;
  right: 8px;
}
</style>
