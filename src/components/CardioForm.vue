<template>
    <v-container class="fill-height">
        <v-responsive class="align-center text-center fill-height">
            <h1 class="text-h4 font-weight-bold">Cardiovascular Disease Test</h1>
            <v-sheet :elevation="12" class="mx-auto content" max-width="600px" rounded>
                <v-form v-model="form" fast-fail @submit.prevent="onSubmit">
                    <v-text-field
                        v-model="heightCm"
                        :rules="[required]"
                        label="Height in centimeters"
                    ></v-text-field>
                    <v-text-field
                        v-model="weightKg"
                        label="Weight in kilograms"
                        :rules="[required]"
                    ></v-text-field>
                    <v-text-field
                        v-model="bmi"
                        label="BMI"
                        :rules="[required]"
                    ></v-text-field>
                    <v-text-field
                        v-model="alcoholConsumption"
                        label="Alcohol Consumption (from 0 to 30)"
                        :rules="[required]"
                    ></v-text-field>
                    <v-text-field
                        v-model="fruitConsumption"
                        label="Fruit Consumption (from 0 to 120)"
                        :rules="[required]"
                    ></v-text-field>
                    <v-text-field
                        v-model="greenVegetableConsumption"
                        label="Green Vegetable Consumption (from 0 to 128)"
                        :rules="[required]"
                    ></v-text-field>
                    <v-text-field
                        v-model="friedPotatoConsumption"
                        label="Fried Potato Consumption (from 0 to 128)"
                        :rules="[required]"
                    ></v-text-field>

                    <v-select
                      :items="generalHealth"
                      v-model="generalHealthData"
                      label="General Health"
                      :rules="[required]"
                    ></v-select>
                    <v-select
                      :items="checkup"
                      v-model="checkupData"
                      label="Last medical checkup attended"
                      :rules="[required]"
                    ></v-select>
                    <v-select
                      :items="exercise"
                      v-model="exerciseData"
                      label="Do you exercise regularly?"
                      :rules="[required]"
                    ></v-select>
                    <v-select
                      :items="skinCancer"
                      v-model="skinCancerData"
                      label="Do you suffer skin cancer?"
                      :rules="[required]"
                    ></v-select>
                    <v-select
                      :items="otherCancer"
                      v-model="otherCancerData"
                      label="Do you suffer other form of cancer?"
                      :rules="[required]"
                    ></v-select>
                    <v-select
                      :items="depression"
                      v-model="depressionData"
                      label="Do you suffer from depression?"
                      :rules="[required]"
                    ></v-select>
                    <v-select
                      :items="diabetes"
                      v-model="diabetesData"
                      label="Do you suffer from diabetes?"
                      :rules="[required]"
                    ></v-select>
                    <v-select
                      :items="arthritis"
                      v-model="arthritisData"
                      label="Do you suffer from arthritis?"
                      :rules="[required]"
                    ></v-select>
                    <v-select
                      :items="sex"
                      v-model="sexData"
                      label="Sex"
                      :rules="[required]"
                    ></v-select>
                    <v-select
                      :items="age"
                      v-model="ageData"
                      label="Age"
                      :rules="[required]"
                    ></v-select>
                    <v-select
                      :items="smokingHistory"
                      v-model="smokingHistoryData"
                      label="Smoking History"
                      :rules="[required]"
                    ></v-select>
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
    form: false,
    loading: false,
    heightCm: null,
    weightKg: null,
    bmi: null,
    alcoholConsumption: null,
    fruitConsumption: null,
    greenVegetableConsumption: null,
    friedPotatoConsumption: null,
    generalHealth: ["Poor", "Fair", "Good", "Very Good", "Excellent"],
    generalHealthData: null,
    checkup: ["Within the past year", "Within the past 2 years", "Within the past 5 years", "5 or more years ago", "Never"],
    checkupData: null,
    exercise: ["Yes", "No"],
    exerciseData: null,
    skinCancer: ["Yes", "No"],
    skinCancerData: null,
    otherCancer: ["Yes", "No"],
    otherCancerData: null,
    depression: ["Yes", "No"],
    depressionData: null,
    diabetes: ["Yes", "No"],
    diabetesData: null,
    arthritis: ["Yes", "No"],
    arthritisData: null,
    sex: ["Male", "Female"],
    sexData: null,
    age: ["18-24", "25-29", "30-34", "35-39", "40-44", "45-49", "50-54", "55-59", "60-64", "65-69", "70-74", "75-79", "80+"],
    ageData: null,
    smokingHistory: ["Yes", "No"],
    smokingHistoryData: null,
    errorMessage: null,
    messageColor: null,
    predictedClass: null
  }),
  methods: {
    onSubmit () {
      if (!this.form) return
      this.messageColor = null
      this.loading = true
      fetch('http://127.0.0.1:5000/predict', {
        method: 'post',
        headers: {
          'content-type': 'application/json'
        },
        body: JSON.stringify({
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
          height_cm: this.heightCm,
          weight_kg: this.weightKg,
          bmi: this.bmi,
          smoking_history: this.smokingHistoryData,
          alcohol_consumption: this.alcoholConsumption,
          fruit_consumption: this.fruitConsumption,
          green_vegetables_consumption: this.greenVegetableConsumption,
          fried_potato_consumption: this.friedPotatoConsumption
        })
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