<template>
  <v-app>
    <v-main>
      <v-container fill-height fluid>
        <v-container align="center" justify="center">
          <v-row align="center" justify="center">
            <span class="name nunito">Harmony Testnet BUSD Faucet</span>
          </v-row>
          <v-row align="center" justify="center">
            <span class="nunito">Enter your ONE address to get {{ busdAmount }} BUSD</span>
          </v-row>
          <v-row align="center" justify="center">
            <v-card class="mt-3 pr-2"
                    min-width="450">
              <v-text-field
                      hide-details
                      single-line
                      filled
                      rounded
                      dense
                      label="ONE Address"
                      append-outer-icon="mdi-arrow-right-bold"
                      @click:append-outer="sendTransaction"
                      v-model="toAddress">
              </v-text-field>
            </v-card>
          </v-row>
        </v-container>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
  require('dotenv').config({path: '/Users/janetliang/WebstormProjects/busd-faucet/faucet/.env'})
  const { Harmony } = require('@harmony-js/core')
  const { ChainType } = require('@harmony-js/utils')

  const hmy = new Harmony(process.env.ENDPOINT,
          {
            chainType: ChainType.Harmony,
            chainID: process.env.CHAINID,
          }
  )

  console.log(process.env)
  console.log(hmy)
  export default {
    name: 'App',
    data: () => ({
      busdAmount: 1, // How much is a good amount to send out, change when done testing
      toAddress: "",
    }),
    methods: {
      sendTransaction() {
        console.log("sending transaction for " + this.busdAmount + " to " + this.toAddress + " from ")
      },
      oneToHexAddress(hmy, address) {
        return hmy.crypto.getAddress(address).basicHex
      },
    }
  };
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Nunito+Sans&display=swap');

  .name {
    font-size: 25px
  }

  .nunito {
    font-family: 'Nunito Sans', sans-serif;
  }
</style>