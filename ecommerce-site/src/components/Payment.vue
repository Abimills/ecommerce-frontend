<template>
  <div class="container">
    <div class="">
      <div class="payment-instruction-container">
        
          <p class="">Payment Methods</p>
       
      </div>
      <div class="">
        <div class="">
          <div class="pay-now-paypal">
            <p>
              <a
                class=" "
                data-bs-toggle="collapse"
                href="#collapseExample"
                role="button"
                aria-expanded="true"
                aria-controls="collapseExample"
              >
                <span class="paypal">PayPal</span>
                <span class="fab fa-cc-paypal"> </span>
              </a>
            </p>
            <div class="bill-info">
              <div class="row">
                <div class="col-8">
                  <p class="h4 mb-0">Summary</p>
                  <p class="mb-0">
                    <span class="c-green"
                      >You have selected {{ store.state.cart.length }} products
                      from our shop</span
                    >
                  </p>
                  <p class="mb-0">
                    <span class="fw-bold">Total Price:</span
                    ><span class="c-green">:${{ store.state.amount }}.00</span>
                  </p>
                </div>
              </div>
            </div>
            <div class="col-12">
              <div
                class="btn btn-primary payment"
                @click="handleSubmit('paypal')"
              >
              ${{ store.state.amount }}.00   Pay Now
              </div>
            </div>
          </div>
          <div class="payment-instruction-container">
            <p>
              <a
                class="btn btn-primary p-2 w-100 h-100 d-flex align-items-center justify-content-between"
                data-bs-toggle="collapse"
                href="#collapseExample"
                role="button"
                aria-expanded="true"
                aria-controls="collapseExample"
              >
                <span class="fw-bold">Credit Card</span>
                <span class="">
                  <span class="fab fa-cc-amex"></span>
                  <span class="fab fa-cc-mastercard"></span>
                  <span class="fab fa-cc-discover"></span>
                </span>
              </a>
            </p>
            <div class="bill-info" >
              <div class="row">
                <div class="col-lg-5 mb-lg-0 mb-3">
                  <p class="h4 mb-0">Summary</p>
                  <p class="mb-0">
                    <span class="c-green"
                      >You have selected {{ store.state.cart.length }} products
                      from our shop</span
                    >
                  </p>
                  <p class="mb-0">
                    <span class="fw-bold">Total Price: </span>
                    <span class="c-green fw-bold">
                      ${{ store.state.amount }}.00</span
                    >
                  </p>
                </div>
                <div class="col-lg-7">
                  <form
                    action=""
                    class="form"
                    @submit.prevent="handleSubmit('credit')"
                  >
                    <div class="row">
                      <div class="col-12">
                        <div class="form__div">
                          <input
                            type="text"
                            class="form-control"
                            placeholder=" "
                            v-model="cardNumber"
                          />
                          <label for="" class="form__label">Card Number</label>
                        </div>
                      </div>

                      <div class="col-6">
                        <div class="form__div">
                          <input
                            type="text"
                            class="form-control"
                            placeholder=" "
                            v-model="month"
                          />
                          <label for="" class="form__label">MM / yy</label>
                        </div>
                      </div>

                      <div class="col-6">
                        <div class="form__div">
                          <input
                            type="password"
                            class="form-control"
                            placeholder=" "
                            v-model="cvv"
                          />
                          <label for="" class="form__label">cvv code</label>
                        </div>
                      </div>
                      <div class="col-12">
                        <div class="form__div">
                          <input
                            type="text"
                            class="form-control"
                            placeholder=" "
                            v-model="cardName"
                          />
                          <label for="" class="form__label"
                            >name on the card</label
                          >
                        </div>
                      </div>
                      <div class="col-12">
                        <!-- <div class="btn btn-primary w-100">Sumbit</div> -->
                        <button type="submit" class="btn btn-primary payment">
                           ${{ store.state.amount }}.00 Pay
                        </button>
                      </div>
                    </div>
                  </form>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// import "bootstrap/dist/css/bootstrap.min.css"; // Bootstrap CSS
// import "bootstrap"; // Bootstrap JavaScript (if needed)
import { useStore } from "vuex";
const store = useStore();
const props = defineProps([]);
import { ref, onMounted } from "vue";
import { useRouter } from "vue-router";
import { useToast } from 'vue-toastification'
const router = useRouter();
const toast = useToast();
const receivedValue = ref("");
const cardNumber = ref("");
const cvv = ref("");
const month = ref("");
const cardName = ref("");
const handleSubmit = (type) => {
  if(store.state.cart?.length > 0){

  
  if (type === "credit") {
    if (cardNumber.value && cvv.value && month.value && cardName) {
      // alert("all payed");
      store.commit("payed");
      router.push('/');
    } else {
    toast.warning("Fill the payment details ", {
        timeout: 2000
      });
    }
  } else if (type === "paypal") {
    store.commit("payed");
    router.push('/');
  }
  }else {
      toast.warning("can not pay for $0 amount ", {
        timeout: 2000
      });
  }
};
// Accessing the value from the route parameters
onMounted(() => {
  const { amount } = router.currentRoute.value.params;
  receivedValue.value = amount || "No value received";
});
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800;900&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Caveat:wght@400;500;600;700&family=Croissant+One&family=Dosis:wght@200;300;400;500;600;700;800&family=Mooli&family=Outfit:wght@100;200;300;400;500;600;700;800;900&family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap");
/* font-family: 'Caveat', cursive; */
/* font-family: 'Croissant One', cursive; */
/* font-family: 'Dosis', sans-serif; */
/* font-family: 'Mooli', sans-serif; */
/* font-family: 'Outfit', sans-serif; */
/* font-family: 'Roboto', sans-serif; */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Dosis", sans-serif;
}

.container {
  margin: 30px auto;
}

.container .card {
  width: 100%;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
  
  border-radius: 0px;
}




.btn.btn-primary {
  background-color: #ddd;
  color: black;
  box-shadow: none;
  border: none;
  font-size: 20px;
  width: 100%;
  height: 100%;
}

.btn.btn-primary:focus {
  box-shadow: none;
}

.container .card .img-box {
  width: 80px;
  height: 50px;
}

.container .card img {
  width: 100%;
  object-fit: fill;
}

.container .card .number {
  font-size: 24px;
}

.container .card-body .btn.btn-primary .fab.fa-cc-paypal {
  font-size: 32px;
  color: #709290;
}

.fab.fa-cc-amex {
  color: #709290;;
  font-size: 32px;
}

.fab.fa-cc-mastercard {
  font-size: 32px;
  color: red;
}

.fab.fa-cc-discover {
  font-size: 32px;
  color: orange;
}

.c-green {
  color: green;
}

.box {
  height: 40px;
  width: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #ddd;
}

.btn.btn-primary.payment {
  background-color: rgb(115, 162, 45);;
  color: white;
  border-radius: 0px;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 24px;
}

.form__div {
  height: 50px;
  position: relative;
  margin-bottom: 24px;
}

.form-control {
  width: 100%;
  height: 45px;
  font-size: 14px;
  border: 1px solid #dadce0;
  border-radius: 0;
  outline: none;
  padding: 2px;
  background: none;
  z-index: 1;
  box-shadow: none;
}

.form__label {
  position: absolute;
  left: 16px;
  top: 10px;
  background-color: #fff;
  color: greenyellow;;
  font-size: 16px;
  transition: 0.3s;
  text-transform: uppercase;
}

.form-control:focus + .form__label {
  top: -8px;
  left: 12px;
  color: greenyellow;;
  font-size: 12px;
  font-weight: 500;
  z-index: 10;
}

.form-control:not(:placeholder-shown).form-control:not(:focus) + .form__label {
  top: -8px;
  left: 12px;
  font-size: 12px;
  font-weight: 500;
  z-index: 10;
}

.form-control:focus {
  border: 1.5px solid #709290;;
  box-shadow: none;
}
.payment-instruction-container{
  width: 100%;
  color:#709290;;
  margin-bottom:2rem;
  
}
.payment-instruction-container p{
  text-transform: uppercase;

}
.paypal{
  width: 200px;
  background:white;
  padding: 2px;;
  


}
.pay-now-paypal{
  width: 100%;
  color:#709290;;
}
.bill-info *{
  color:#709290;;
  margin:.3rem 0;
  
  
}
</style>