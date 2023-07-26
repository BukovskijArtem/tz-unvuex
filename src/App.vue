<template>
  <div>
    <div id="parent">
      <div>
        <input v-model="price" placeholder="price" type="number"/>
        <label >{{ lablePrice }}</label>
      </div>
      <div>
        <input v-model="qty" placeholder="qty" type="number"/>
        <label >{{ lableQty }}</label>
      </div>
      <div>
        <input v-model="amount" placeholder="amount" type="number"/>
        <label >{{ lableAmount }}</label>
      </div>
      <div>
        <button @click="send">Send</button>
        <label>{{ lc }}</label>
      </div>
    </div>
    <div id="log">
      <textarea rows="20" disabled>{{ log }}</textarea>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      log: '',
      lc: JSON.parse(localStorage.getItem('post')),
      nonce: 1,
      price: '',
      qty: '',
      amount: '',
      lablePrice: '',
      lableQty: '',
      lableAmount: '',
    }
  },
  methods: {
    send () {
      let lc = {nonce: this.nonce, price: this.price, qty: this.qty, amount: this.amount}
      this.logs("Отправлено: " + JSON.stringify(lc) + ", localStorage: " + localStorage.getItem('post'))
      this.isDisabled = true
      setTimeout(()=>{
        this.post(lc)
      },1000);
      this.nonce += 1
    },
    logs (str) {
      this.log = str + "\n" + this.log
    },
    post (lc) {
      if (this.amount && this.amount % 2 == 0) {
        localStorage.setItem('post', JSON.stringify(lc));
        this.lc = JSON.parse(localStorage.getItem('post'))
        this.logs("{success: true}" + ", отправлено: " + JSON.stringify(lc) + ", localStorage: " + localStorage.getItem('post'))
      } else {
        this.logs("{success: false}" + ", отправлено: " + JSON.stringify(lc)+ ", localStorage: " + localStorage.getItem('post'))
      }
    }
  },
  
  watch: {
    price: function (el) {
      setTimeout(()=>{
        this.logs("price:"+el)
        this.lablePrice = el
      },300);
    },
    qty: function (el) {
      setTimeout(()=>{
        this.logs("qty:"+el)
        this.lableQty = el
      },300);
    },
    amount: function (el) {
      setTimeout(()=>{
        this.logs("amount:"+el)
        this.lableAmount = el
      },300);
    }
  }
}
</script>

<style scoped>
#parent {
	flex-direction: row;
	display: flex;
}
#parent > div {
  margin-left:5%;
  flex-direction: column;
	display: flex;
}
#parent > div > label{
  margin-top:10px;
}
#log {
  margin-left:5%;
  margin-top:5%;
}
#log > textarea {
  width: 70%;
}
</style>
