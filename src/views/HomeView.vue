<template>
   <main>
      <header>
         <h1>Welcome to Burger Shop</h1>
         <h4>Amazing burgers!</h4>
         <img src="img/header.jpg" alt="header" class="head">
      </header>
      <div>
         <div>
            <h1>Select burger:</h1>
            <div class="burgers">
            <Burger v-for="burger in burgers"
               v-bind:burger="burger" 
               v-bind:key="burger.name"
               v-on:orderedBurger="addToOrder($event)"/>
         </div>
      </div>
         <div class ="wrapmap">
            <div id="map" v-on:click="setLocation">
               <div id ="T">
               <div v-bind:style="{ left: location.x + 'px', 
                                    top: location.y + 'px' }">
                                          <div>
                                             T
                                          </div>
               </div> 
            </div>
            </div>
         </div>
      </div>
      <main>
         <section class="contact">
            <form>
               <h3>Costumer information</h3>
               <p>Please provide necessary information for delivery</p>
               <h4>Delevery information</h4>
               <p>
                  <label for="firstname">Firstname</label><br>
                  <input type="text" id="firstname" v-model="fn" required="required" placeholder="First name">
               </p>
               <p>
                  <label for="lastname">Lastname</label><br>
                  <input type="text" id="lastname" v-model="ln" required="required" placeholder="Last name">
               </p>
               <p>
                  <label for="email">E-mail address</label><br>
                  <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
               </p>
               <p>
                  <label for="payment">Payment options</label>
                  <select id="payment" v-model="pm">
                     <option>Credit cart</option>
                     <option>Debit card</option>
                     <option>Apple pay</option>
                     <option>Bank transfer</option>
                  </select>
               </p>
               <p>
                  <label for="Male">Man
                  <input type="radio" id="Male" v-model="sex" value="Male">
                  </label>
                  <label for="Female">Female
                  <input type="radio" id="Female" v-model="sex" value="Female">
                  </label>
                  <label for="do_not_want_to_provide">Do not want to provide
                  <input type="radio" id="do_not_want_to_provide" v-model="sex" value="Do not want to provide">
                  </label>
               </p>
               <button @click="markDone()">
               <img src="img/sendd.jpg" style="width: 20px;">
               Send Info
               </button>
            </form>
         </section>
      </main>
      <hr>
   </main>
   <footer>
      <p>© 2023, Burger Shop Ink.</p>
   </footer>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

/*
function MenuItems(name, contents1, url, all){
  this.name = name
  this.contents1 = contents1
  this.url = url
  this.all = all
}


const burgers = [
  {name: "Cheese Burger", url: "img/cheeseburger-11.jpg", contents1: "Burger with good meat", all: "Meat, gluten" }, 
  {name: "Chicken Burger", url: "img/Chickenburger-1.jpg", contents1: "Delicious chicken burger", all: "Gluten" }, 
  {name: "Halloumi Burger", url: "img/halloumiburger.jpg", contents1: "Vegetarian burger", all: "Gluten" }
]
console.log(burgers)
*/


export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
      return {
        burgers: menu,
        fn:"",
        ln:"",
        em:"", 
        pm:"",
        sex:"",
        orderedBurgers: {},
        orders: [],
        location: { 
            x: 0,
            y: 0
         },
      };
  },

  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },

    markDone() {
      socket.emit("addOrder", {  orderId: this.getOrderNumber(),
                                 details: {  x: this.location.x ,
                                             y: this.location.y},
                                 orderItems: this.orderedBurgers,
                                 costumerInfo: {fn:this.fn, 
                                                ln:this.ln,
                                                em:this.em,
                                                sex:this.sex,
                                                pm:this.pm}
                                 } 
      );
      this.orderedBurgers = {};
    },

    setLocation: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location.x = event.clientX - 10 - offset.x
      this.location.y = event.clientY - 10 - offset.y
                              
   },

    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location.x = event.clientX - 10 - offset.x
      this.location.y = event.clientY - 10 - offset.y
    },

    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
      console.log(this.orderedBurgers)
   },


  }
}

</script>

<style>

header {
    color:darkblue;
    height: 200px;
    padding-top: 150px;
    position: relative; 
    text-align: center; 
}

.head {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    opacity: 0.5
}

.contact {
    background-color:cadetblue;
    color:lightgoldenrodyellow;
    margin-right: 215px;
    margin-left: 100px;
    padding: 70px;
    border: 15px double lightgoldenrodyellow;
}


body {
    font-family:Georgia, 'Times New Roman', Times, serif;
    color:cadetblue;
    font-weight: bold;
    background-color: lightgoldenrodyellow;
    font-size: large;
} 

.burgers {
   display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 200px)); 
  gap: 100px;
}

footer { 
    font-family: Arial, Helvetica, sans-serif;
    font-size: small;
}

button:hover {
    background-color: rgb(160, 219, 221);
    cursor: pointer;
}

button {
    margin: 10px;
    color: cadetblue;
    font-family: Georgia, 'Times New Roman', Times, serif;
    font-size: medium;
}

.wrapper {
    display: grid;
    grid-gap: 10px;
    grid-template-columns: repeat(3, 1fr);
}

.box {
    border-radius: 5px;
    padding: 20px;
}

#map {
    width: 1920px;
    height: 1078px;
    background: url("../../public/img/polacks.jpg");
}

.wrapmap {
   overflow: scroll;
   height: 600px;
   width: 600px;
}

#T {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor:crosshair;
  }

#T div {
   background: black;
   color: white;
   border-radius: 10px;
   width:20px;
   height:20px;
   text-align: center;
   position: absolute;
}
</style>