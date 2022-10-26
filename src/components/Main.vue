<template>
  <div class="row">
    <div class="col-md-12 error" >
      <span v-show="usError || phError">{{error_msg}} </span>
    </div>
  </div>

  <div class="container row">   
      <div class="left col-md">        
        <span class="headers">Cities</span>
        <draggable v-model="bucket_1" transition="200" class="drop-zone">
          <template v-slot:item="{ item }">
            <div class="draggable-item">
              {{ item.city }}
            </div>
          </template>
        </draggable>
      </div>
      <div class="middle col-md">
        <span class="headers">United States</span>
        <draggable v-model="bucket_2" transition="200" class="drop-zone bucket-2" :class="usError ? 'danger' : ''">
          <template v-slot:item="{ item }">
            <div class="draggable-item">
              {{ item.city }}
            </div>
          </template>
        </draggable>
      </div>
      <div class="right col-md">
        <span class="headers">Philippines</span>
        <draggable v-model="bucket_3" transition="200" class="drop-zone bucket-3" :class="phError ? 'danger' : ''">
          <template v-slot:item="{ item }">
            <div class="draggable-item">
              {{ item.city }}
            </div>
          </template>
        </draggable>
      </div>
     </div>
</template>
<script>
import Draggable from "vue3-draggable";
export default {
  name: "Main",
  components: {
    Draggable,
  },
  data() {
    return {    
      showError: false,      
      bucket_1: [{
        country: 'US', 
        city: 'New York'
      },
      {
        country: 'US', 
        city: 'Los Angeles'
      },
      {
        country: 'US', 
        city: 'Chicago'
      },
      {
        country: 'PH', 
        city: 'Manila'
      },
      {
        country: 'PH', 
        city: 'Cebu'
      },
      {
        country: 'PH', 
        city: 'Dumaguete'
      },
    ],
    error_msg: '',
    bucket_2: [],
    bucket_3: [],
    };
  },
  watch: {
    bucket_2(data) {
      this.usError = false;
      Object.values(data).forEach((value)=> {
        this.usError = value.country !== 'US';    
        if(this.usError) 
          this.throwError('United States', value.city)     
      });
    },
    bucket_3(data) {
      this.phError = false;
      
      Object.values(data).forEach((value)=> {
        this.phError = value.country !== 'PH';   
        if(this.phError) 
          this.throwError('Philippines', value.city)         
      });
    }
  },
  methods: {
      throwError(country, city){
          this.error_msg = city + " doesn't belong to the " + country; 
      }
  }
};
</script>
