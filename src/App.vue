<template>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-12 col-md-9 col-lg-8 col-xl-6 ">
        <div class="min-vh-100 position-relative">
          <figure class="text-center">
            <img src="./assets/logo.png" class="logo-img" alt="">
          </figure>
          <h3 class="text-center my-3">Invoice Maker</h3>
          <form class="hide-in-print" action="" @submit.prevent="saveRecord">
            <div class="row g-2">
              <div class="col-6">
                <select v-model="selectedService" class="form-select">
                  <option value="" >Select Service</option>
                  <option v-for="service in services" :value="service.id" :key="service.id">
                    {{ service.name }}
                  </option>
                </select>
              </div>
              <div class="col-4">
                <input type="number" min="1" v-model="inputQuantity" class="form-control" placeholder="Quantity">
              </div>
              <div class="col-2">
                <button class="btn btn-primary w-100">
                  <i class="fa-solid fa-plus"></i>
                </button>
              </div>
            </div>
          </form>
          <table class="table table-bordered mt-3">
            <thead>
            <tr>
              <th>#</th>
              <th>Service</th>
              <th>Unit Price</th>
              <th>Quantity</th>
              <th>Cost</th>
            </tr>
            </thead>
            <tbody>
            <tr>
              <td v-if="records.length === 0" colspan="5" class="text-center">No record</td>
            </tr>
            <Row v-for="record in records" @to-del="del" :key="record.id" :record="record"></Row>
            </tbody>
            <tfoot>
            <tr v-if="records.length > 0">
              <td colspan="4">Total</td>
              <td class="text-end">{{ records.reduce((pv,cv)=>pv+cv.cost,0) }}</td>
            </tr>
            </tfoot>
          </table>
          <div class="position-absolute border p-3 rounded hide-in-print bg-white w-100" style="bottom: 10px">
            <div class="row">
              <div class="col-6">
                <input v-model="invoiceNumber" class="form-control">
              </div>
              <div class="col">
                <button @click="toPrint()" class="btn btn-secondary w-100">
                  <i class="fa-solid fa-print fa-fw"></i>
                  Print
                </button>
              </div>
              <div class="col">
                <button @click="toPrint()" class="btn btn-primary w-100">
                  <i class="fa-solid fa-file fa-fw"></i>
                  Save
                </button>
              </div>

            </div>

          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Row from "@/components/Row";
export default {
  components: {Row},
  data() {
    return {
      selectedService:"",
      inputQuantity:"",
      invoiceId : "",
      services: [
        {
          id:1,
          name : "Web Design",
          price : 100,
        },
        {
          id:2,
          name : "domain service",
          price: 10
        },
        {
          id:3,
          name: "SSL",
          price: 5
        },
        {
          id:4,
          name: "maintenance",
          price: 50
        }
      ],
      recordStart : 1,
      records : []
    }
  },
  computed: {
    invoiceNumber() {
      let d = new Date();
      let dateCode = d.getDate()+""+(d.getMonth()+1)+""+d.getFullYear()
      let random = Math.floor(Math.random()*10000);
      return dateCode+"-"+random
    }
  },
  methods: {
    saveRecord() {
      let currentService = this.services.find(service=>service.id===this.selectedService);
      let cost = currentService.price * this.inputQuantity;
      let record = {
        id : this.recordStart,
        service : currentService,
        quantity : this.inputQuantity,
        cost
      }
      this.records.push(record);
      this.selectedService = this.inputQuantity = ""
      this.recordStart++
    },
    del(recordId){
      console.log("listen event from child component")
      this.records = this.records.filter(record => record.id != recordId)
      console.log(recordId)
    },
    toPrint(){
      print();
    }
  },
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Oswald:wght@300;400;700&display=swap');
$font-family-sans-serif : 'Oswald', sans-serif;
$primary: #41b883;
$secondary : #35495e;

@import "~bootstrap/scss/bootstrap";
//@import "~bootstrap/dist/css/bootstrap.min.css";
@import "~@fortawesome/fontawesome-free/css/all.min.css";

.logo-img{
  height: 100px;
}

@media screen {
  .show-in-print{
    display: none;
  }
  .hide-in-print{
    //display: ;
  }
}
@media print {
  .hide-in-print{
    display: none !important;
  }
  .show-in-print{
    display: block;
  }
}
</style>