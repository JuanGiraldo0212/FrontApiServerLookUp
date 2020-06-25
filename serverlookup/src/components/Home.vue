<template>

<b-row align-h="center">
   
      <b-card
    title="Website LookUp"
    style="max-width: 50rem;"
    class="mt-3"
   
  >
  <b-form >
      <b-form-group
        id="input-group-1"
        
      >
        <b-form-input
          id="input-1"
          v-model="domain"
          required
          placeholder="Enter domain"
        ></b-form-input>
      </b-form-group>


      <b-button class="mr-1" @click="fetchItem" variant="primary">Look up</b-button>
      <b-button class="mr-1" @click="fetchArchive" variant="danger">Archive</b-button>
    </b-form>

    <b-card class="mt-3" header="Website Data Result" align="left" v-if="lookUp">

      <div v-if="showData">
      <b-img-lazy  :src="item.logo" alt="Image 6" center></b-img-lazy>
      <br>
        <pre class="m-0">Title: {{ item.title }}</pre>
        <pre class="m-0">Server down: {{ item.is_down }}</pre>
        <pre class="m-0">Servers Changed: {{ item.servers_changed}}</pre>
        <pre class="m-0">SLL Grade: {{ item.ssl_grade }}</pre>
        <pre class="m-0">Previous SLL Grade: {{ item.previous_ssl_grade }}</pre>

      <b-card class="mt-3" header="Server" align="left" v-for="server in item.servers" :key="server.address">
       <pre class="m-0">Address: {{ server.address }}</pre>
       <pre class="m-0">SSL Grade: {{ server.ssl_grade }}</pre>
       <pre class="m-0">Contry: {{ server.country }}</pre>
       <pre class="m-0">Owner: {{ server.owner }}</pre>
      </b-card>
      </div>

      <div v-else>
        <pre class="m-0">{{ item }}</pre>
      </div>
      
      
    </b-card>

    <b-card class="mt-3" header="Archive" align="left" v-if="archive">
      <div v-if="showData">
         <pre class="m-0" v-for="server in servers" :key="server">{{ server }}</pre>
      </div>

      <div v-else>
        <pre class="m-0">{{ servers}}</pre>
      </div>
     
    </b-card>

  </b-card>
  
    
  </b-row>
  

</template>

<script>
import axios from "axios";
export default {
  name: 'Home',
  data () {
    return {
      item: "Loading...",
      servers: "Loading...",
      domain:"",
      lookUp:false,
      archive:false,
      showData:false
    }
  },

  methods: {
    fetchItem: function () {
     this.showData=false
      this.item="Loading..."
      this.lookUp=true
      this.archive=false
      const baseURI = 'http://localhost:8282/servers/'+this.domain
       axios.get(baseURI).then((result) => {
         this.item=result.data
         this.showData=true
    })
    },

    fetchArchive: function () {
     this.showData=false
       this.servers="Loading..."
      this.lookUp=false
      this.archive=true
      const baseURI = 'http://localhost:8282/archive/'
      axios.get(baseURI).then((result) => {
        this.servers=result.data.items
        this.showData=true
    })
    
    }
  }

}
</script>

<style lang="scss" scoped>



</style>