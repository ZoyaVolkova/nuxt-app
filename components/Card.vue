<template>
  <form
    class="card"
    @submit="$emit('submit', $event)"
    @keyup.enter="checkValidity"
    novalidate
  >
    <label for="name" class="required">Наименование товара</label>
    <input
      id="name"
      name="name"
      type="text"
      placeholder="Введите наименование товара"
      required
      @input="$emit('inputName', $event.target.value)"
    />
    <span class="validity-field name hidden">Поле является обязательным</span>
    <label for="description">Описание товара</label>
    <textarea
      id="description"
      name="description"
      placeholder="Введите описание товара"
    />
    <span class="validity-field hidden">Поле является обязательным</span>
    <label for="link" class="required">Ссылка на изображение товара</label>
    <input
      id="link"
      name="link"
      type="text"
      placeholder="Введите ссылку"
      required
      @input="$emit('inputLink', $event.target.value)"
    />
    <span class="validity-field link hidden">Поле является обязательным</span>
    <label for="price" class="required">Цена товара</label>
    <input
      id="price"
      name="price"
      type="number"
      placeholder="Введите цену"
      required
      @input="$emit('inputPrice', $event.target.value)"
    />
    <span class="validity-field price hidden">Поле является обязательным</span>
    <button type="submit" :class="{ active: isActive }" @click="checkValidity">
      Добавить товар
    </button>
  </form>
</template>
<script>
export default {
  name: "card",
  data() {
    return {};
  },
  props: {
    name: {
      type: String,
      default: "",
    },
    link: {
      type: String,
      default: "",
    },
    price: {
      type: String,
      default: "",
    },
    isActive: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    checkValidity(e) {
      this.$el.querySelectorAll("input").forEach((input) => {
        if (!input.validity.valid) {
          e.preventDefault();
          if (!input.classList.contains("invalid")) {
            this.$el.querySelector(`.${input.id}`).classList.remove("hidden");
            input.classList.add("invalid");
          }
        } else {
          if (input.classList.contains("invalid")) {
            this.$el.querySelector(`.${input.id}`).classList.add("hidden");
            input.classList.remove("invalid");
          }
        }
      });
    },
  },
};
</script>
<style scoped>
.card {
  width: 332px;
  height: 440px;
  background: #fffefb;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04),
    0px 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
  display: flex;
  flex-direction: column;
  font-family: "Source Sans Pro", sans-serif;
  padding: 24px;
  margin-left: 32px;
  margin-top: 16px;
  box-sizing: border-box;
}

input,
textarea {
  max-width: 284px;
  background: #fffefb;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border: none;
  border-radius: 4px;

  padding-left: 16px;
  padding-top: 10px;
  padding-bottom: 11px;
}
textarea {
  height: 108px;
  resize: none;
}

label {
  color: #49485e;
  font-size: 10px;
  line-height: 13px;
  letter-spacing: -0.02em;
  margin-bottom: 4px;
  align-self: start;
}
input::placeholder,
textarea::placeholder {
  color: #b4b4b4;
  font-size: 12px;
  line-height: 15px;
  font-family: "Source Sans Pro", sans-serif;
}
input:focus,
textarea:focus {
  outline: none;
}

.required {
  position: relative;
}
.required::after {
  content: " \25CF";
  color: #ff8484;
  width: 4px;
  height: 4px;
  position: absolute;
  top: -6px;
}
#price {
  margin-bottom: 2px;
}
button {
  width: 284px;
  background: #eeeeee;
  border: none;
  outline: none;
  border-radius: 10px;
  font-family: "Inter", sans-serif;
  font-size: 12px;
  line-height: 15px;
  text-align: center;
  letter-spacing: -0.02em;
  color: #b4b4b4;
  padding-top: 10px;
  padding-bottom: 11px;
  transition: background 0.6s;
  cursor: pointer;
}
button:hover {
  color: #868686;
}
input[type="number"]::-webkit-outer-spin-button,
input[type="number"]::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
.invalid {
  border: 1px solid #ff8484 !important;
}
.validity-field {
  color: #ff8484;
  font-weight: normal;
  font-size: 8px;
  line-height: 10px;
  letter-spacing: -0.02em;
  margin: 0;
  padding: 0;
  margin-bottom: 2px;
  margin-top: 4px;
  width: 100px;
  height: 10px;
}
.price {
  margin-bottom: 12px;
}
.active {
  background: #7bae73;
  color: #fff;
}
</style>
