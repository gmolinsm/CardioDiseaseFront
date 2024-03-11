<template>
    <v-container class="fill-height">
        <v-responsive class="align-center text-center fill-height">
            <h1 class="text-h4 font-weight-bold">Cardiovascular Disease Test</h1>
            <v-sheet :elevation="12" class="mx-auto content" max-width="600px" rounded>
                <v-form v-model="form" fast-fail @submit.prevent="onSubmit">
                    <v-text-field
                        v-model="heightCmData"
                        :rules="rules"
                        label="Your height in centimeters"
                    ></v-text-field>
                    <v-text-field
                        v-model="weightKgData"
                        label="Your weight in kilograms"
                        :rules="rules"
                    ></v-text-field>

                    <div class="text-caption">Alcohol Consumption in a week (0 means no alcohol, 10 means daily use)</div>
                    <v-slider
                      v-model="alcoholConsumption"
                      :min="0"
                      :max="10"
                      class="align-center"
                      thumb-label
                      step="1"
                      tick-size="4"
                    ></v-slider>
                    <div class="text-caption">Fruit Consumption in a week (0 means no fruit, 10 means a lot of fruit)</div>
                    <v-slider
                      v-model="fruitConsumption"
                      :min="0"
                      :max="10"
                      class="align-center"
                      thumb-label
                      step="1"
                      tick-size="4"
                    ></v-slider>
                    <div class="text-caption">Green Vegetable Consumption in a week (0 no vegetables, 10 a lot of vegetables)</div>
                    <v-slider
                      v-model="greenVegetableConsumption"
                      :min="0"
                      :max="10"
                      class="align-center"
                      thumb-label
                      step="1"
                      tick-size="4"
                    ></v-slider>
                    <div class="text-caption">Fried Foods Consumption in a week (0 no fried foods, 10 daily McDonalds)</div>
                    <v-slider
                      v-model="friedPotatoConsumption"
                      :min="0"
                      :max="10"
                      class="align-center"
                      thumb-label
                      step="1"
                      tick-size="4"
                    ></v-slider>

                    <v-select
                      :items="age"
                      v-model="ageData"
                      label="Age"
                      :rules="[required]"
                    ></v-select>
                    <v-select
                      :items="generalHealth"
                      v-model="generalHealthData"
                      label="How would you rate your general health?"
                      :rules="[required]"
                    ></v-select>
                    <v-select
                      :items="checkup"
                      v-model="checkupData"
                      label="When was the last medical checkup you attended?"
                      :rules="[required]"
                    ></v-select>

                    <v-radio-group :rules="[required]" v-model="exerciseData" inline label="Do you exercise regularly?">
                      <v-radio label="Yes" value="Yes"></v-radio>
                      <v-radio label="No" value="No"></v-radio>
                    </v-radio-group>
                    <v-radio-group :rules="[required]" v-model="skinCancerData" inline label="Do you suffer skin cancer?">
                      <v-radio label="Yes" value="Yes"></v-radio>
                      <v-radio label="No" value="No"></v-radio>
                    </v-radio-group>
                    <v-radio-group :rules="[required]" v-model="otherCancerData" inline label="Do you suffer other forms of cancer?">
                      <v-radio label="Yes" value="Yes"></v-radio>
                      <v-radio label="No" value="No"></v-radio>
                    </v-radio-group>
                    <v-radio-group :rules="[required]" v-model="depressionData" inline label="Do you suffer from depression?">
                      <v-radio label="Yes" value="Yes"></v-radio>
                      <v-radio label="No" value="No"></v-radio>
                    </v-radio-group>
                    <v-radio-group :rules="[required]" v-model="diabetesData" inline label="Do you suffer from diabetes?">
                      <v-radio label="Yes" value="Yes"></v-radio>
                      <v-radio label="No" value="No"></v-radio>
                    </v-radio-group>
                    <v-radio-group :rules="[required]" v-model="arthritisData" inline label="Do you suffer from arthritis?">
                      <v-radio label="Yes" value="Yes"></v-radio>
                      <v-radio label="No" value="No"></v-radio>
                    </v-radio-group>
                    <v-radio-group :rules="[required]" v-model="smokingHistoryData" inline label="Have you smoked for long periods of time?">
                      <v-radio label="Yes" value="Yes"></v-radio>
                      <v-radio label="No" value="No"></v-radio>
                    </v-radio-group>
                    <v-radio-group :rules="[required]" v-model="sexData" inline label="Biological Sex">
                      <v-radio label="Male" value="Male"></v-radio>
                      <v-radio label="Female" value="Female"></v-radio>
                    </v-radio-group>
                    
                    <v-btn 
                        :disabled="!form"
                        :loading="loading"
                        type="submit"
                        variant="elevated"
                        class="mt-2" rounded="xl" 
                        color="secondary" block>Submit
                    </v-btn>
                    <p v-if="predictedClass" class="text-center message">According to our model, you have a {{predictedClass}}% chance of suffering cardiovascular disease</p>
                    <p v-if="errorMessage" class="text-center message">{{errorMessage}}</p>
                </v-form>
            </v-sheet>
        </v-responsive>
    </v-container>
    
</template>

<script>
export default {
  data: () => ({
    rules: [
      value => {
        if (value){
          if (!isNaN(parseFloat(value)) && isFinite(value)) {
            return true
          }
          return 'Enter a valid number'
        }
        return 'Field is required'
      },
    ],
    generalHealth: ["Poor", "Fair", "Good", "Very Good", "Excellent"],
    checkup: ["Within the past year", "Within the past 2 years", "Within the past 5 years", "5 or more years ago", "Never"],
    age: ["18-24", "25-29", "30-34", "35-39", "40-44", "45-49", "50-54", "55-59", "60-64", "65-69", "70-74", "75-79", "80+"],
    form: false,
    loading: false,
    heightCmData: null,
    weightKgData: null,
    alcoholConsumption: null,
    fruitConsumption: null,
    greenVegetableConsumption: null,
    friedPotatoConsumption: null,
    generalHealthData: null,
    sexData: null,
    checkupData: null,
    exerciseData: null,
    skinCancerData: null,
    otherCancerData: null,
    depressionData: null,
    diabetesData: null,
    arthritisData: null,
    ageData: null,
    smokingHistoryData: null,
    errorMessage: null,
    messageColor: null,
    predictedClass: null,
    body: null
  }),
  methods: {
    onSubmit () {
      if (!this.form) return
      this.messageColor = null
      this.loading = true
      this.body = JSON.stringify({
        general_health: this.generalHealthData,
        checkup: this.checkupData,
        exercise: this.exerciseData,
        skin_cancer: this.skinCancerData,
        other_cancer: this.otherCancerData,
        depression: this.depressionData,
        diabetes: this.diabetesData,
        arthritis: this.arthritisData,
        sex: this.sexData,
        age_category: this.ageData,
        height_cm: this.heightCmData,
        weight_kg: this.weightKgData,
        bmi: this.weightKgData / Math.pow(this.heightCmData/100, 2),
        smoking_history: this.smokingHistoryData,
        alcohol_consumption: (this.alcoholConsumption/10)*30,
        fruit_consumption: (this.fruitConsumption/10)*120,
        green_vegetables_consumption: (this.greenVegetableConsumption/10)*128,
        fried_potato_consumption: (this.friedPotatoConsumption/10)*128
      })
      console.log(this.body)
      fetch('https://cardio-disease-back.lm.r.appspot.com/predict', {
        method: 'post',
        headers: {
          'content-type': 'application/json'
        },
        body: this.body,
      }).then(res => {
        if (res.ok) {
          this.errorMessage = null
          return res.json()
        } else {
          this.predictedClass = null
          this.loading = false
          this.messageColor = "red"
          this.errorMessage = "Response invalid"
          throw new Error("Response invalid")
        }
      }).then(data => {
        this.loading = false
        this.predictedClass = data['class']
      }).catch((error) => {
        this.predictedClass = null
        this.loading = false
        this.messageColor = "red"
        this.errorMessage = "Something went wrong"
        console.error("FETCH ERROR:", error)
      })
    },
    required (v) {
      return !!v || 'Field is required'
    },
  },
}
</script>

<style>
.content {
  margin-top: 5%;
  padding: 2%;
}

.message {
  margin-top: 5%;
  color: v-bind(messageColor)
}
</style>