<template>
  <div id="app" class="hello">

    <h1>Serverless Voting App</h1>
  

    <b-row align-h="center" class="mt-5">
      <b-card-group deck>
        <b-card bg-variant="success" text-variant="white" header="Vote Yes" class="text-center" footer-tag="footer">
          <b-card-text>Have You Had Cloud Training This Year?</b-card-text>
          <b-button size="lg" variant="primary"  @click="vote('yes')">Yes</b-button>
          <em slot="footer">{{ votesYes }} voted</em>
        </b-card>

        <b-card bg-variant="danger" text-variant="white" header="Vote No" class="text-center" footer-tag="footer">
          <b-card-text>Have You Had Cloud Training This Year?</b-card-text>
          <b-button size="lg" variant="primary" @click="vote('no')">No</b-button>
          <em slot="footer">{{ votesNo }} voted</em>
        </b-card>
      </b-card-group>
    </b-row>
    <b-row align-h="center" class="mt-5">
      
    </b-row>
  </div>  
</template>


<script>
import { API } from 'aws-amplify'
export default {
  name: 'app',
  data() {
    return {
      apiName: 'pollCounterAPI',
      votesYes: 0,
      votesNo: 0
    }
  },
  methods: {
    vote: async function (vote) {
      const init = {
        queryStringParameters: {
          vote
        }
      }
      const response = await API.post(this.apiName, '/votes', init)
      if (vote === 'yes') this.votesYes = response.data.Attributes.votesYes
      if (vote === 'no') this.votesNo = response.data.Attributes.votesNo
    },
    updateVotes: async function () {
      const response = await API.get(this.apiName, '/votes/poll-001')
      this.votesNo = response[0].votesNo
      this.votesYes = response[0].votesYes    
    }
  },
  created () {
    this.updateVotes()
    setInterval(this.updateVotes, 3000)
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
