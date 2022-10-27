<script setup lang="ts">
import { onMounted, ref } from 'vue'
import Web3 from 'web3'

const address = ref('')
const balance = ref('')

async function getBalance() {
  let web3 = null
  if (window.ethereum) {
    console.log('window.ethereum:', window.ethereum)
    web3 = new Web3(window.ethereum)
    try {
      await window.ethereum.enable()
    } catch (error) {
      
    }
  } else if (window.web3) {
    console.log('window.web3:', window.web3)
    web3 = window.web3
  } else {
    const provider = new Web3.providers.HttpProvider('http://127.0.0.1:9545')
    web3 = new Web3(provider)
  }
  console.log('web3:', web3)
  web3.eth.getAccounts().then(res => {
    console.log('getAccounts:', res)
    address.value = res[0]

    web3.eth.getBalance(res[0]).then(res => {
      console.log('balance:', res)
      balance.value = res
    })
  })
  
}

</script>

<template>
  <header>

    <div class="wrapper">
      <div>
        <button @click="getBalance">获取钱包</button>
      </div>
      <div>钱包地址：{{address}}</div>
      <div>钱包余额：{{balance}}</div>
    </div>
  </header>

</template>

<style scoped>
.wrapper {
  display: flex;
  flex-direction: column;
}
</style>
