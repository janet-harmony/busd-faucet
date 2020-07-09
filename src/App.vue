<template>
  <v-app>
    <v-main>
      <v-container fill-height fluid>
        <v-container align="center" justify="center">
          <v-row align="center" justify="center">
            <span class="name nunito">Harmony Testnet BUSD Faucet</span>
          </v-row>
          <v-row align="center" justify="center">
            <span class="nunito">Enter your ONE address to get {{ amount }} BUSD</span>
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
          <v-row v-if="loading" align="center" justify="center">
            <v-progress-circular class="mt-4"
                                 color="light-blue"
                                 indeterminate>
            </v-progress-circular>
          </v-row>
          <v-row v-if="successful && hash != null" align="center" justify="center" class="mt-2">
              <span class="nunito">Transaction successful!</span>
          </v-row>
          <v-row v-if="successful && hash != null" align="center" justify="center" class="mt-1">
            <a v-bind:href="link" target="_blank">
              <span class="nunito">{{ hash }}</span>
            </a>
          </v-row>
          <v-row v-if="!successful" align="center" justify="center" class="mt-2">
            <span class="nunito">Error sending transaction. Please try again.</span>
          </v-row>
        </v-container>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
  export default {
    name: 'App',
    data: () => ({
      amount: 1, // How much is a good amount to send out, change when done testing
      toAddress: "",
      loading: false,
      successful: true,
      hash: null,
      link: null,
    }),
    methods: {
      async sendTransaction() {
        console.log("sending transaction for " + this.amount + " to " + this.toAddress)
        let requestString = 'http://localhost:3000/tx?method=transfer&args=' + this.toAddress + ',' + (this.amount * 1e18)
        console.log(requestString)
        this.loading = true
        this.hash = null
        this.link = null

        fetch(requestString, { mode: 'cors', headers: {
            'Content-Type': 'application/json',
          }})
                .then(response => {
                  this.loading = false
                  if (!response.ok) {
                    this.successful = false
                    throw new Error('Network response was not ok');
                  }
                  this.successful = true
                  response.json().then(resp => {
                    this.hash = resp.hash
                    this.link = 'https://explorer.testnet.harmony.one/#/tx/' + resp.hash
                  })
                })
                .catch(error => {
                  console.error('Error:', error);
                })
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