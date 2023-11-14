<template>   
  <header>
        <h1> Welcome to the burgerWeb </h1>
    </header>
  <main>
    <section class="extra" id="BurgerChooser">
        <h2> Select you burger of choice</h2>
            <p>Here you choose your burger</p>
        <div class="Wrapper">
            <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"
              v-on:orderedBurger="addToOrder($event)"/>
        </div>
  </section>

  <section id="PaymentChooser">
        <h2> Customer information </h2>
            <p>Here you add the delivery/payment information</p>
        <h3> Delivery information</h3>
        <p id="mP1">
            <label for="firstlastname">Full name</label><br>
            <input type="text" id="firstlastname" v-model="fln" required="required" placeholder="First- and Last name">
        </p>
        <p>
            <label for="email">Email adress</label><br>
            <input type="email" id="email" v-model="the_email" required="required" placeholder="Email adress">

        </p>
        <div id="mapHolder">
            <div id="map" v-on:click="setLocation">
              <div id="dots">
                <div v-bind:style="{ left: location.x + 'px', top: location.y + 'px'}">
                T
                </div>
            </div>
            </div>
         </div>    
        <p>
            <label for="paymentMethod">Payment Method</label>
            <select id="paymentMethod" v-model="rcp">
                <option>Cash</option>
                <option selected="selected"> Card </option>     <!-- otherwise, first option is selected -->
                <option>Swish</option>
                <option>Credit</option>
            </select>
         </p>
         <!-- <p><textarea name="textarea" id="t1" cols="30" rows="10" maxlength="20"></textarea></p> text area-->
         <p>
            <label for="male">Male</label>
            <input type="radio" id="male" v-model="rbutton" value="male" checked="checked"><br>
            <label for="female">Female</label>
            <input type="radio" id="female" v-model="rbutton" value="female"><br>
            <label for="dnwta">Do not wish to answer</label>
            <input type="radio" id="dnwta" v-model="rbutton" value="donotwish">
         </p>
    </section>
    <button v-on:click="orderDone(key)">
        <img src="https://as2.ftcdn.net/v2/jpg/02/02/78/73/1000_F_202787336_gWqQjarJ6bOSoKe5UBuB69UFoQCP4wjb.jpg" alt="checkButton" style="width: 20px;">
        Send delivery
    </button>
    </main>
    <footer> <hr> &copy give me a date </footer>


</template>



<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

function MenuItem(name,img,kCal,lactose,gluten){
    this.name = name;
    this.img = img;
    this.kCal = kCal;
    this.lactose = lactose;
    this.gluten = gluten;
}
var infoArray = [];

const burger_array = menu

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: burger_array,
      fln: "",
      the_email: "",
      the_house: "",
      the_street: "",
      rcp: "",
      rbutton: "",
      orderedBurgers: [],
      location: { x: 0,
                  y: 0
                      }
    }
  },
  methods: {
    orderDone: function(){
    let burgerList = {};
    for (let bur in this.orderedBurgers){
      if(this.orderedBurgers[bur] != 0){
        burgerList[bur] = this.orderedBurgers[bur];
      }
    }
    console.log(burgerList)
    infoArray = {name: this.fln,
                email: this.the_email,
                rcp: this.rcp,
                gender: this.rbutton}

    socket.emit("addOrder", {  orderId: this.getOrderNumber(),
                                details: { x: this.location.x,
                                           y: this.location.y},
                                orderItems: burgerList,
                                personalInfo: infoArray
                              }
                 );
  },
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 10 - offset.y;
    },
    addToOrder: function (event) {
        this.orderedBurgers[event.name] = event.amount;
    },
    setLocation: function(){
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 10 - offset.y;
      
    }
  },
}


</script>



<style>
@import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';

  #mapHolder {
    overflow: scroll;
  }
  #map {
    width: 1920px;
    height: 1078px;
    background: url('../../public/img/polacks.jpg');
  }
  #dots {
    position: relative;
  }
  
  #dots div {
    position: absolute;
  }

  #paymentMethod{
    margin: 1em;
  }
  /* Your comment goes here */

header{
   background-image: url(https://i.pinimg.com/originals/ba/52/bc/ba52bc2a53b48881279601c3dbe3ef37.jpg);
   background-size: cover;
   overflow:hidden;
   width: 97%;
   height: 200px;
   margin-bottom: -2em;
   max-height: 7em;

}
header h1{
   text-align: center;
   margin-left: 50px;
}

body {
    font-family: arial;
 }

.extra{
    color: #6d6865;
}
#idtest{
    color: #cb7575;
    font-weight: bold;
}
#BurgerChooser{
    background-color: #000000;
    color: aliceblue;
}
button:hover {
    background-color: greenyellow;
    cursor: pointer;
 }

 section {
    margin: 50px;
    margin-bottom: 20px;
    border-style: dotted;
 }

 
 button {
    margin: 20px;
    margin-left: 5em;
 }
 .Wrapper{
    display: grid;
    grid-gap: 10px;
    grid-template-columns: 33% 33% 33%;
 }
 .box{
    padding-left: 50px;
    padding-right: 45px;
 }
 .a{
    grid-column: 1;
 }
 .b{
    grid-column: 2;
 }
 .c{
    grid-column: 3;
 }
 
 p,h2,h3,header {
    margin-left: 20px;
 }







</style>