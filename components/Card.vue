<template>
  <form
    class="card"
    @submit="$emit('submit', $event)"
    @keyup.enter="checkValidity"
    novalidate
  >
    <div class="card_container">
      <label for="name" class="required">Name</label>
      <input
        id="name"
        name="name"
        type="text"
        placeholder="Enter name of product"
        required
        @input="$emit('inputName', $event.target.value)"
      />
      <span class="validity-field name">The field is required</span>
      <label for="description">Description</label>
      <textarea
        id="description"
        name="description"
        placeholder="Enter description of product"
      />
      <span class="validity-field">The field is required</span>
      <label for="link" class="required">Link</label>
      <input
        id="link"
        name="link"
        type="text"
        placeholder="Enter product image link"
        required
        @input="$emit('inputLink', $event.target.value)"
      />
      <span class="validity-field link">The field is required</span>
      <label for="price" class="required">Price</label>
      <input
        id="price"
        name="price"
        type="number"
        placeholder="Enter price of product"
        required
        @input="$emit('inputPrice', $event.target.value)"
      />
      <span class="validity-field price">The field is required</span>
      <button
        type="submit"
        :class="{ active: isActive }"
        @click="checkValidity"
      >
        Add
      </button>
    </div>
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
      console.log(e.target);
      this.$el.querySelectorAll("input").forEach((input) => {
        if (!input.validity.valid) {
          e.preventDefault();
          if (!input.classList.contains("invalid")) {
            input.classList.add("invalid");
          }
        } else {
          if (input.classList.contains("invalid")) {
            input.classList.remove("invalid");
          }
        }
      });
    },
  },
};
</script>
<style scoped lang="scss">
.card {
  width: 332px;
  height: 440px;
  background: #fffefb;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04),
    0px 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
  font-family: "Source Sans Pro", sans-serif;
  margin-top: 16px;
  box-sizing: border-box;
  padding: 24px;
}
.card_container {
  display: flex;
  flex-direction: column;
}
input,
textarea {
  position: relative;
  background: #fffefb;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border: none;
  border-radius: 4px;
  padding-left: 16px;
  padding-top: 10px;
  padding-bottom: 11px;
}
textarea {
  resize: none;
  height: 108px;
  box-sizing: border-box;
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
button:hover.active {
  color: #fff;
  background: #60885a;
}
input[type="number"]::-webkit-outer-spin-button,
input[type="number"]::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
.invalid {
  border: 1px solid #ff8484 !important;
}
input:valid {
  border: none !important;
}
.invalid + .validity-field {
  visibility: visible;
  opacity: 1;
}

input:valid + .validity-field {
  visibility: hidden;
  opacity: 0;
}
.validity-field {
  color: #ff8484;
  font-weight: normal;
  font-size: 10px;
  line-height: 10px;
  letter-spacing: -0.02em;
  margin: 0;
  padding: 0;
  margin-bottom: 2px;
  margin-top: 4px;
  width: 100px;
  height: 10px;
  visibility: hidden;
  opacity: 0;
  transition: opacity 0.4s;
}
.price {
  margin-bottom: 12px;
}
.active {
  background: #7bae73;
  color: #fff;
}
</style>
