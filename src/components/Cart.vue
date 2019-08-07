<template>
  <div>
    <ul class="list-group">
      <li class="list-group-item" v-for="item in items" :key="item.id">
        {{ item.title }} - ₦ {{ item.price }}
        <button
          @click="$emit('remove-from-cart', item)"
          class="btn badge badge-danger float-right"
        >Remove from cart</button>
      </li>
    </ul>

    <div class="card p-3 my-5">
      <h4 class="text-center">₦ {{ total }}</h4>
    </div>

    <!-- <button
      :disabled="items.length === 0"
      @click="$emit('pay')"
      class="btn btn-info form-control"
    >Pay Now</button> -->

    <div>
      <Rave
       :is-production="false"
       style-class="btn btn-info form-control"
       :email="email"
       :amount="amount"
       :reference="reference"
       :rave-key="raveKey"
       :callback="callback"
       :close="close"
       currency="NGN">
       
       <!-- <i>Pay Me, My Money</i> -->
      <button 
        :disabled="items.length === 0"
        @click="$emit('pay')">Pay Now</button>
   </Rave>
    </div>
  </div>
</template>

<script>
import Rave from 'vue-ravepayment';
export default {
  props: ["items"],
  computed: {
    total() {
      this.amount = this.items.reduce((acc, item) => acc + Number(item.price), 0);
      this.raveAmount = this.items.reduce((acc, item) => acc + Number(item.price), 0);
      return this.amount;
    },
    reference(){
        let text = "";
        let possible = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";

        for( let i=0; i < 10; i++ )
          text += possible.charAt(Math.floor(Math.random() * possible.length));

        return text;
      }
  },
  components: {
    Rave
  },
  data(){
    return{
      raveKey: process.env.VUE_RAVE_KEY,
      email: "payments@cofundie.com",
      amount: 10000,
      raveAmount: this.raveAmount,
    }
  },
  methods: {
    callback(response){
      console.log(response);
    },
    close(){
      console.log("Payment closed");
    }
  }
};
</script>
<style> 
    .paymentbtn{
        color: #04193d;
        width: 250px;
        height: 50px;
        font-weight: 800;
    }
</style>