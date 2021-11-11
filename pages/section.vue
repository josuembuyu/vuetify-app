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
                        CODESEC
                      </th>
                      <th class="text-left">
                        SEC
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
                    label="CODE SECTION"
                    required
                  ></v-text-field>
                  <v-text-field
                    v-model="email"
                    :rules="emailRules"
                    label="SECTION"
                    required
                  ></v-text-field>
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
        'Item 1',
        'Item 2',
        'Item 3',
        'Item 4',
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

