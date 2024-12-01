<template>
  <div class="burger-item">
    <h2>{{ burger.name }}</h2>
    <img :src="getImageSrc(burger.name)" width="200" height="200" />
    <div>
      <ul class="components">
        <li>{{ burger.kCal }} kCal</li>
        <li>{{ burger.lactose ? 'Contains lactose' : 'Lactose free' }}</li>
        <li>{{ burger.gluten ? 'Contains gluten' : 'Gluten free' }}</li>
        <li>
          {{ amountOrdered }}
          <button @click="decreaseAmount">-</button>
          <button @click="increaseAmount">+</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
  data: function () {
    return {
      amountOrdered: 0,
    }
  },
  methods: {
    getImageSrc(burgerName) {
      switch (burgerName) {
        case 'Cheeseburger':
          return '/borgir2.jpg';
        case 'Krabby Patty':
          return '/krabby1.jpg';
        case 'Big Mac':
          return '/burger1.jpg';
        default:
          return '';
      }
    },
    increaseAmount() {
      this.amountOrdered++;
      this.$emit('orderedBurger', { name: this.burger.name, amount: this.amountOrdered });
    },
    decreaseAmount() {
      if (this.amountOrdered > 0) {
        this.amountOrdered--;
        this.$emit('orderedBurger', { name: this.burger.name, amount: this.amountOrdered });
      }
    },
  }
}
</script>

<style scoped>
body {
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
    margin: 0;
    padding: 0;
}

header h1 {
    text-align: center;
}

.ingredient {
    font-weight: bold;
}

#burgers {
    background-color: black;
    color: white;
}

button:hover {
    background-color: green;
    cursor: pointer;
}

section {
    text-align: center;
}

.header-container {
    position: relative;
    height: 400px;
    overflow: hidden;
}

.header-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
    opacity: 0.65;
}

.header-title {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: white;
    font-size: 2.5em;
    text-align: center;
}

.grid-container {
    display: grid; 
    grid-template-columns: repeat(3, 1fr); 
    gap: 20px;
}

.burger-item {
    border: 2px solid white
}

.components {
    list-style-type: none;
    padding: 0;
    margin: 0;
}

.components {
  list-style-type: none;
  padding: 0;
}

button {
  margin-left: 5px;
}
</style>