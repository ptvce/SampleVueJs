<template>
  <div>
    <!-- Toolbar -->
    <nav class="toolbar" role="banner">
      <span>{{appName}}</span>
    </nav>

    <div class="container">
      <div class="output-container">
        <div class="rial-price-output">Price: <strong>{{rialValue}}</strong> <small> rial</small></div>
        <div class="usd-price-output">USD: <strong>{{usdValue}}</strong> <small> USD</small></div>
      </div>
      <input type="number" class="usd-price-input" v-model="usdValueInput" placeholder="Enter USD price..." />
      <button class="btn-convert" @click="fetchPrice">Convert</button>
    </div>
  </div>
</template>


<script>
export default {
  name: "App",
  data() {
    return {
      appName: 'Vue Converter',
      rialValue: 0,
      usdValue: 0,
      usdValueInput: ''
    }
  },
  methods: {
    fetchPrice() {
      fetch('https://api.tgju.online/v1/data/sana/json')
      .then(result=>{
        return result.json();
      })
      .then(data=>{
        // find usd among all items
        let usdItem = data.sana.data.find(i => i.slug === 'sana_buy_usd');

        this.usdValue = parseInt(usdItem.p);
        this.rialValue = parseInt(this.usdValueInput | 0) * this.usdValue;
      })
      .catch(err=>{
        console.log(err);
      })
    }
  }
};
</script>


<style>
body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif,
    "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
  box-sizing: border-box;
}

.toolbar {
  position: sticky;
  top: 0;
  left: 0;
  right: 0;
  height: 60px;
  display: flex;
  align-items: center;
  background-color: #1976d2;
  color: white;
  font-weight: 600;
}

.container {
  margin: 120px 75px 0 75px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.rial-price-output {
  font-size: 45px;
}

.usd-price-output {
  font-size: 45px;
  margin-top: 20px;
}

.usd-price-input {
  height: 40px;
  width: 230px;
  margin-top: 50px;
  padding-left: 10px;
  padding-right: 10px;
  border: 2px solid rgba(100, 100, 100, 0.3);
  border-radius: 5px;
  outline: none;
  font-size: 16px;

  transition: 200ms;
}

.usd-price-input:hover {
  border: 2px solid rgba(117, 177, 226, 0.4);
}

.usd-price-input:focus {
  border: 2px solid rgba(0, 86, 156, 0.4);
}

.btn-convert {
  cursor: pointer;
  margin-top: 30px;
  width: 250px;
  height: 40px;
  border-radius: 5px;
  border: 0;
  outline: 0;
  font-size: 16px;
  color: white;
  background-color: #3ba5ec;

  transition: 200ms;
}

.btn-convert:hover {
  background-color: #2a8cce;
}

.btn-convert:active {
  background-color: #176da7;
}
</style>