<template>
  <v-app :style="{background: $vuetify.theme.themes['light'].background}">
    <div class="d-flex ma-5">
        <v-chip color="black" text-color="white" size="large" class="mr-2">{{ step }}</v-chip><h3>{{ steps[step-1] }}</h3>
      </div>
    <v-container>
      <v-row>
        <v-col
          cols="12"
          md="8"
          v-show="step == 1"
          class="order-last order-md-first"
        >
          <PersonalInfoForm ref="personalInfo" @nextStep="step++"/>
        </v-col>
        <v-col
          cols="12"
          md="8"
          v-if="step == 2"
          class="order-last order-md-first"
        >
          <PaymentsDetailsForm ref="paymentsInfo"  @nextStep="complete" @prevStep="step--"/>
        </v-col>
        <v-col
          cols="12"
          md="4"
          class="mt-3 order-fist order-md-last"
        >
          <YourOrder/>
        </v-col>
      </v-row>
    </v-container>
    <v-snackbar
      v-model="snackbar"
      :timeout="2000"
      color="primary"
    >
      Purchase Complete !
    </v-snackbar>
  </v-app>
</template>

<script>
import YourOrder from './components/YourOrder';
import PersonalInfoForm from './components/PersonalInfoForm';
import PaymentsDetailsForm from './components/PaymentsDetailsForm';

export default {
  name: 'App',

  components: {
    YourOrder,
    PersonalInfoForm,
    PaymentsDetailsForm
  },

  data: () => ({
    step: 1,
    steps: ['PERSONAL INFORMATION','PAYMENTS DETAILS'],
    snackbar: false
  }),

  methods:{
    complete(){
      const fullData = Object.assign({},this.$refs.personalInfo.personalInfoData,this.$refs.paymentsInfo.paymentsDetailsData)
      console.log(JSON.stringify(fullData))
      this.$refs.personalInfo.reset()
      this.$refs.paymentsInfo.reset()
      this.step = 1
      this.snackbar = true
    }
  }
};
</script>

<style>
.v-input__slot::before {
  border-style: none !important;
}
.theme--light.v-input, .theme--light.v-input input, .theme--light.v-input textarea {
    color: grey;
}
.theme--light.v-label {
    color: black;
    font-weight: 600;
}
</style>
