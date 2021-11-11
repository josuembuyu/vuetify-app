<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-form
          ref="form"
          v-model="valid"
          lazy-validation
        >

          <v-text-field
            v-model="email"
            :rules="emailRules"
            label="CODEDEPART"
            required
          ></v-text-field>
          <v-text-field
            v-model="email"
            :rules="emailRules"
            label="DEPARTEMENT"
            required
          ></v-text-field>

          <v-select
            v-model="select"
            :items="items"
            :rules="[v => !!v || 'Item is required']"
            label="Section"
            required
          ></v-select>
          <v-row>
            <v-col cols="12" sm="8" md="12">
              <v-btn
              block
              x-large
              class="mr-4"
              @click="validate"
              color="success"
            >
              Enregistrer
              </v-btn>
            </v-col>
          </v-row>
          
          
      </v-form>
    </v-col>
  </v-row>
</template>
