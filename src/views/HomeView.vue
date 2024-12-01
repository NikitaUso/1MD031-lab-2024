<template>
  <div>
    <header class="header-container">
      <img src="/krusty.jpeg" alt="Header Image" class="header-image">
      <h1 class="header-title">Welcome to BurgerOnline</h1>
    </header>
    <main>
      <section>
        <h2>Choose your burger</h2>
      </section>

      <section id="burgers" class="grid-container" style="border: 2px dotted white;">
        <Burger v-for="burger in burgers"
                v-bind:burger="burger" 
                v-bind:key="burger.name"
                v-on:orderedBurger="addToOrder($event)"/>
      </section>

      <section id="customer-information" style="border: 2px dotted black;">
        <h2>Customer information</h2>
        <p>
          <label for="fullname">Full Name</label><br>
          <input type="text" id="fullname" v-model="fullname" required="required" placeholder="Full name">
        </p>
        <p>
          <label for="email">Email</label><br>
          <input type="email" id="email" v-model="email" required="required" placeholder="E-mail address">
        </p>
        <p>
          <label for="pay">Payment method</label><br>
          <select id="pay" v-model="paymentMethod">
            <option>Card</option>
            <option>PayPal</option>
            <option>Swish</option>
            <option>Klarna</option>
          </select>
        </p>
        <div id="map-container">
        <div id="map" @click="setLocation" style="position: relative;">
    <img src="/img/polacks.jpg" alt="Karta" style="width: 1920px; height: 1078px;">
    <div
      v-if="location.x && location.y"
      :style="{
        position: 'absolute',
        left: location.x + 'px',
        top: location.y + 'px'
      }"
    >
      T
    </div>
    </div>
  </div>
        <p>
          <label>Gender</label><br>
          <input type="radio" id="male" v-model="gender" value="male" required>
          <label for="male">Man</label><br>
        
          <input type="radio" id="female" v-model="gender" value="female">
          <label for="female">Woman</label><br>
        
          <input type="radio" id="other" v-model="gender" value="other">
          <label for="other">Other</label>
        </p>
        <button type="button" @click="addOrder">
          <img src="/foodora.jpg" width="15" height="15">
          Order
        </button>
      </section>
    </main>
    <hr>
    <footer>
      BurgerOnline
    </footer>

  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io("localhost:3000");

function MenuItem(name, kCal, lactose, gluten) {
  this.name = name;
  this.kCal = kCal;
  this.lactose = lactose;
  this.gluten = gluten;
}

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      fullname: '',
      email: '',
      paymentMethod: '',
      gender: 'male',
      orderedBurgers: {},
      location: { x: 0, y: 0 }
    };
  },
  methods: {
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
    setLocation(event) {
      const rect = event.target.getBoundingClientRect();
      this.location.x = event.clientX - rect.left;
      this.location.y = event.clientY - rect.top;
    },
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    addOrder: function () {
      let order = {
        fullname: this.fullname,
        email: this.email,
        paymentMethod: this.paymentMethod,
        gender: this.gender,
        orderId: this.getOrderNumber(),
        details: {
          x: this.location.x,
          y: this.location.y
        },
        orderItems: this.orderedBurgers
      };
      socket.emit("addOrder", order);
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

#map-container {
    width: 100%;
    height: 600px;
    overflow: scroll;
}

#map {
    background-repeat: no-repeat;
    width: 1920px;
    height: 1078px;
    cursor: crosshair;
    width: 1920px;
    height: 1080px;
    background: url('/img/polacks.jpg') no-repeat center center;
    background-size: cover;
}
</style>