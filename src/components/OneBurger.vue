<template>
    <div class="box">
    <h3>{{burger.name}}</h3>
          <img v-bind:src="burger.img" alt="Span" title="Another in-line element" style="width: 200px;">
                <ul>
                  <li> {{burger.kCal}} </li>
                    <span v-if="burger.gluten === true" id="idtest">
                      <li>Gluten</li>
                    </span>
                    <span v-if="burger.lactose === true" id="idtest">
                      <li>Lactose</li>
                    </span>
                </ul>
          <div>
          <button v-on:click="addBurger(key)" class="buttonBox">+</button>
          {{amountOrdered}}
          <button v-on:click="removeBurger(key)" class="buttonBox">-</button>
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
        amountOrdered: 0
      }
    },
  methods: {
  removeBurger: function(){
    if(this.amountOrdered > 0){
          this.amountOrdered --;
    }
    this.$emit('orderedBurger', {name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
    );
  },
  addBurger: function () {
  this.amountOrdered += 1;
  this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
    );
  },
    }
  }
  </script>
  


  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  h3 {
      margin-left: 20px;
    }
  #idtest{
  color: #cb7575;
  font-weight: bold;
}
.buttonBox{
  margin-left: 15px;
  margin-right: 15px;
}

  
  </style>
  