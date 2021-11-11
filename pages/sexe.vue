<template>
  <v-container>
      <v-container>
        <v-row>
          <v-col cols="12" sm="8" md="6">
              <v-simple-table
              >
                <template v-slot:default>
                  <thead>
                    <tr>
                      <th class="text-left">
                        CODE SEXE
                      </th>
                      <th class="text-left">
                        SEXE
                      </th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr
                      v-for="item in desserts"
                      :key="item.name"
                    >
                      <td>{{ item.name }}</td>
                      <td>{{ item.calories }}</td>
                    </tr>
                  </tbody>
                </template>
              </v-simple-table>
          </v-col>
          <v-col cols="12" sm="8" md="6">
            <v-card style="padding: 2em" class="mx-auto" elevation="2">
                <v-form
                  ref="form"
                  v-model="valid"
                  lazy-validation
                >

                  <v-text-field
                    v-model="email"
                    :rules="emailRules"
                    label="CODE SEXE"
                    required
                  ></v-text-field>
                  <v-select
                    v-model="select"
                    :items="items"
                    :rules="[v => !!v || 'Item is required']"
                    label="SEXE"
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
            </v-card>
          </v-col>
        </v-row>
      </v-container>
      
  </v-container>
  
</template>

<script>

export default {
    data(){
    return {
      items: [
        'Masculin',
        'Feminin'
      ],
      desserts: [
            {
            name: 'Frozen Yogurt',
            calories: 159,
            },
            {
            name: 'Ice cream sandwich',
            calories: 237,
            }
        ],
    }
  }
}
</script>

