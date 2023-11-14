<template>
    <div id="orders">
      <div id="orderList">
        <div v-for="(order, key, index) in orders" v-bind:key="'order'">
          #{{index + 1}}:

          <span v-for="(value,b) in order.orderItems" id="bOrder">
          {{b}} ({{value}}), 
          </span>
          <br>
          <span id="printer">
          {{order.personalInfo.name}} ({{order.personalInfo.email}}, {{order.personalInfo.rcp}}, {{order.personalInfo.gender}})
          </span>
          <hr>
        </div>
        <button v-on:click="clearQueue">Clear Queue</button>
      </div>
      <div id="dots" v-bind:style="{ background: 'url(' + require('../../public/img/polacks.jpg')+ ')' }">
          <div v-for="(order, key, index) in orders" v-bind:style="{ left: order.details.x + 'px', top: order.details.y + 'px'}" v-bind:key="'dots' + key">
            {{index+1}}
          </div>
      </div>
    </div>
  </template>





  <script>
  import { objectToString } from '@vue/shared';
import io from 'socket.io-client'
  const socket = io();
  
  
  export default {
    name: 'DispatcherView',
    data: function () {
      return {
        orders: null,
        count: 0
      }
    },
    created: function () {
      socket.on('currentQueue', data =>
        this.orders = data.orders);
    },
    methods: {
      clearQueue: function () {
        socket.emit('clearQueue');
      },
      test: function(){
        this.count++;
        console.log(this.count);
      }
    }
  }

  </script>
  <style>
  #orderList {
    top:1em;
    left:1em;
    position: absolute;
    z-index: 2;
    color:black;
    background: rgba(255,255,255, 0.5);
    padding: 1em;
  }
  #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }
  
  #dots div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
  #bOrder{
  }
  #printer{
    font-style: italic;
    font-size: 0.9em;
  }

  </style>
  