<template>
    <v-form
        @submit.prevent="submit"
        ref="form"
        v-model="valid"
        lazy-validation
    >
        <v-container>
            <v-row dense>
                <v-col cols="12" md="6">
                    <v-text-field
                        v-model="personalInfoData.firstName"
                        label="First name"
                        filled
                        background-color="white"
                        color="black"
                        required
                        :rules="[v => !!v || 'First name is required']"
                    ></v-text-field>
                </v-col>
                <v-col cols="12" md="6">
                    <v-text-field
                        v-model="personalInfoData.lastName"
                        label="Last name"
                        filled
                        background-color="white"
                        color="black"
                        required
                        :rules="[v => !!v || 'Last name is required']"
                    ></v-text-field>
                </v-col>
            </v-row>
            <v-row dense >
                <v-col cols="12">
                    <v-text-field
                        v-model="personalInfoData.email"
                        label="Email"
                        filled
                        background-color="white"
                        color="black"
                        required
                        :rules="[v => !!v || 'Email is required', v=> !!v && !!v.match('[a-z0-9]+@[a-z]+.[a-z]{2,3}') || 'Email is not valid']"
                    ></v-text-field>
                </v-col>
            </v-row>
            <v-row dense>
                <v-col cols="12" md="6">
                    <v-select
                        v-model="personalInfoData.country"
                        :items="countries.map(x=>x.name)"
                        filled
                        label="Country"
                        background-color="white"
                        color="black"
                        @change="$refs.form.validate()"
                        :rules="[v => !!v || 'Country is required']"
                    ></v-select>
                </v-col>
                <v-col cols="12" md="6">
                    <v-text-field
                        v-model="personalInfoData.postalCode"
                        label="Postal code"
                        filled
                        background-color="white"
                        color="black"
                        required
                        v-mask="fullCountry?fullCountry.postalCodeMask:undefined"
                        :placeholder="fullCountry?fullCountry.postalCodeMask:undefined"
                        :rules="[v => !!v || 'Postal code is required', v=> postalCodeValid || 'Postal code is not valid']"
                    ></v-text-field>
                </v-col>
            </v-row>
            <v-row dense >
                <v-col cols="12">
                    <v-text-field
                        v-model="personalInfoData.phoneNumber"
                        label="Phone number"
                        filled
                        background-color="white"
                        color="black"
                        required
                        v-mask="fullCountry?fullCountry.phoneNumberMask:undefined"
                        :placeholder="fullCountry?fullCountry.phoneNumberMask:undefined"
                        :rules="[v => !!v || 'Phone number is required',v=>  fullCountry && v.length == fullCountry.phoneNumberMask.length || 'Phone is not full']"
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
                    NEXT
                    </v-btn>
                </v-col>
            </v-row>
        </v-container>
    </v-form>
</template>
  
  <script>
    export default {
      name: 'PersonalInfoForm',
  
      computed:{
        fullCountry(){
            return this.countries.find(x=>x.name == this.personalInfoData.country)
        },
        postalCodeValid(){
            if(this.fullCountry && this.personalInfoData.postalCode)
                return !!this.personalInfoData.postalCode.match("^"+this.fullCountry.postalCodeRegex+"$")
            else return false
        }
      },
  
      data: () => ({
        countries: [
            {name: 'Poland', postalCodeRegex: "[0-9]{2}-[0-9]{3}",postalCodeMask: '##-###', phoneNumberMask: '###-###-###' }, 
            {name: 'United States', postalCodeRegex: "[0-9]{5}([ -][0-9]{4})?",postalCodeMask: '#####-####', phoneNumberMask: '(###) ###-##-##'}
        ],
        personalInfoData: {
            firstName: "", 
            lastName: "",
            email: "",
            country: "",
            postalCode: "",
            phoneNumber: ""
        },
        valid: false
      }),

      methods: {
        submit(){
            if(this.$refs.form.validate())
                this.$emit('nextStep')
        },
        reset(){
            this.$refs.form.reset()
        }
      }
    }
  </script>
  