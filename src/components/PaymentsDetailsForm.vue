<template>
    <v-form
        @submit.prevent="submit"
        ref="form"
        v-model="valid"
        lazy-validation
    >
        <v-container>
            <v-row dense >
                <v-col cols="12">
                    <v-text-field
                        v-model="paymentsDetailsData.creditCardNumber"
                        label="Credit card number"
                        filled
                        background-color="white"
                        color="black"
                        required
                        v-mask="'####-####-####-####'"
                        :rules="[v => !!v || 'Credit card number is required', v=> creditCardValid.isValid || 'Credit card not valid']"
                    >
                    <template v-slot:append>
                        <v-img v-if="creditCardValid.isValid" :src="'./cards/'+creditCardValid.card.type+'.svg'" @error="imageUrlAlt"   max-height="25" max-width="50"/>
                    </template>
                    </v-text-field>
                </v-col>
            </v-row>
            <v-row dense>
                <v-col cols="12" md="6">
                    <v-text-field
                        v-model="paymentsDetailsData.ccv"
                        label="Security code"
                        filled
                        :append-icon="!showCode ? 'mdi-help-circle-outline' : 'mdi-eye-off'"
                        background-color="white"
                        color="black"
                        required
                        v-mask="'###'"
                        :type="showCode ? 'text' : 'password'"
                        @click:append="showCode = !showCode"
                        :rules="[v => !!v || 'Security code is required', v=> !!v &&  v.length == 3 || 'Security code is not full']"
                    ></v-text-field>
                </v-col>
                <v-col cols="12" md="6">
                    <v-text-field
                        v-model="paymentsDetailsData.expirationDate"
                        label="Expiration date"
                        filled
                        background-color="white"
                        color="black"
                        required
                        v-mask="'##/##'"
                        placeholder="MM/YY"
                        :rules="[v => !!v || 'Expiration date is required', v=>expirationDateValid || 'Expiration date is not valid' , v=> !creditCardExpired || 'Card Expired !']"
                    ></v-text-field>
                </v-col>
            </v-row>
            <v-row dense >
                <v-col cols="12">
                    <v-btn
                        :disabled="!valid"
                        color="primary"
                        class="white--text"
                        type="submit"
                        x-large
                        width="100%"
                    >
                    <v-icon
                        left
                    >
                        mdi-cart
                    </v-icon>
                    COMPLETE PURCHASE
                    </v-btn>
                </v-col>
            </v-row>
            <v-row dense >
                <v-col cols="12">
                    <v-btn
                        text
                        class="grey--text"
                        x-large
                        width="100%"
                        @click="$emit('prevStep')"
                    >
                    <v-icon
                        left
                    >
                        mdi-arrow-left
                    </v-icon>
                    previous step
                    </v-btn>
                </v-col>
            </v-row>
        </v-container>
    </v-form>
</template>
  
  <script>
    import cardValidator from 'card-validator'

    export default {
      name: 'PersonalInfoForm',
  
      computed:{
        expirationDateValid(){
            return this.paymentsDetailsData.expirationDate && !!this.paymentsDetailsData.expirationDate.match('^(0[1-9]|1[0-2])/?([0-9]{4}|[0-9]{2})$')
        },
        creditCardExpired(){
            if(this.expirationDateValid){
                const someday = new Date();
                const [ month, year ] = this.paymentsDetailsData.expirationDate.split('/')
                someday.setFullYear('20'+year, month, 1)
                if (someday < new Date())return true
                else return false
            }
            else return false
        },
        creditCardValid(){
            return cardValidator.number(this.paymentsDetailsData.creditCardNumber)
        }
      },
  
      data: () => ({
        paymentsDetailsData: {
            creditCardNumber: "", 
            ccv: "",
            expirationDate: "",
        },
        valid: false,
        showCode: false
      }),

      methods: {
        submit(){
            if(this.$refs.form.validate())
                this.$emit('nextStep')
        },
        reset(){
            this.$refs.form.reset()
        },
        imageUrlAlt(event) {
            event.target.src = '@/assets/cards/generic.svg'
        }
      }
    }
  </script>
  