<template>
  <v-card class="pa-2">
    <div style="border: dotted 1px lightgrey">
      <v-card-title>YOUR ORDER</v-card-title>
      <v-card-text>
        <div v-for="order of orders" :key="order.title" class="d-flex mb-1">
          <span>{{ order.title }}</span><v-spacer/><span>$ {{ order.price }}</span>
        </div>
        <v-divider style="border-style: dotted;" class="mt-5 mb-1"/>
        <div class="d-flex mb-1">
          <span>Total purchases</span><v-spacer/><span>$ {{ orderTotal }}</span>
        </div>
        <div class="d-flex mb-1">
          <span>Estimated tax</span><v-spacer/><span>$ {{ estimatedTax }}</span>
        </div>
        <v-divider style="border-style: dotted;" class="mt-5 mb-1"/>
        <div class="d-flex mb-1">
          <span>Total</span><v-spacer/><h2 class="font-weight-bold">$ {{ totalTotal }}</h2>
        </div>
      </v-card-text>
    </div>
  </v-card>
</template>

<script>
  export default {
    name: 'YourOrder',

    computed:{
      orderTotal(){
        return this.orders.reduce((a,cV)=>a.price+cV.price)
      },
      estimatedTax(){
        if(this.taxPercent)return ( this.orderTotal / this.taxPercent)
        else return 0
      },
      totalTotal(){
        return this.orderTotal + this.estimatedTax
      }
    },

    data: () => ({
      taxPercent: 0,
      orders: [{title: "Apple Watch Sport", price: 580},{title: "Moder buckle", price: 380}]
    }),
  }
</script>
