<template>
  <div class="container">
    <div class="form">
      <div class="title">Add product</div>
      <Card
        @submit.prevent="sendForm"
        @inputName="inputName"
        @inputLink="inputLink"
        @inputPrice="inputPrice"
        :isActive="isActive"
      />
    </div>
    <div class="sort" v-click-outside="hideDropdown">
      <div class="select default" @click="toggleDropdown">{{ sort }}</div>
      <svg class="arrow" width="8" height="6" viewBox="0 0 8 6" fill="none">
        <path
          d="M7.48532 1.24264L4.24268 4.48528L1.00003 1.24264"
          stroke="#B4B4B4"
        />
      </svg>
      <div class="dropdown" v-show="isDropdown">
        <div class="select default" @click="sortDefault">Default</div>
        <div class="select expensive" @click="sortExpensive">
          Price: high to low
        </div>
        <div class="select cheap" @click="sortCheap">Price: low to high</div>
        <div class="select by-name" @click="sortByName">Name</div>
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
      isDropdown: false,
      sort: "Sort by",
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
    inputName(e) {
      this.name = e;
    },
    inputLink(e) {
      this.link = e;
    },
    inputPrice(e) {
      this.price = e;
    },
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
      this.isDropdown === false
        ? (this.isDropdown = true)
        : (this.isDropdown = false);
    },
    hideDropdown() {
      this.isDropdown = false;
    },
    sortExpensive(e) {
      this.hideDropdown();
      this.sort = e.target.textContent;

      this.items = this.items.sort((a, b) => {
        return b.price - a.price;
      });
    },
    sortCheap(e) {
      this.hideDropdown();
      this.sort = e.target.textContent;
      this.items = this.items.sort((a, b) => {
        return a.price - b.price;
      });
    },
    sortByName(e) {
      this.hideDropdown();
      this.sort = e.target.textContent;
      this.items = this.items.sort((a, b) => {
        return a.name.localeCompare(b.name);
      });
    },
    sortDefault(e) {
      this.hideDropdown();
      this.sort = e.target.textContent;
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
  padding: 0;
  margin: 0;
  height: 100vh;
}
.container {
  display: flex;
  font-family: "Source Sans Pro", sans-serif;
  max-width: 1376px;
  background: #faf9f7;
  position: relative;
  margin: 32px;
}
.title {
  font-style: normal;
  font-weight: 600;
  font-size: 28px;
  line-height: 35px;
  color: #3f3f3f;
}
.sort {
  position: absolute;
  top: 0px;
  right: 0px;
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
