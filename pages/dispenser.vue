<template>
  <v-container>
      <v-container>
        <v-row>
          
          <v-col cols="12" sm="8" md="12">
              <v-simple-table
              >
                <template v-slot:default>
                  <thead>
                    <tr>
                      <th class="text-left">
                        Date
                      </th>
                      <th class="text-left">
                        Enseignant
                      </th>
                      <th class="text-left">
                        Heure d'arrivé
                      </th>
                      <th class="text-left">
                        Heure depart
                      </th>
                      <th class="text-left">
                        Cours
                      </th>
                      <th class="text-left">
                        Matière
                      </th>
                      <th class="text-left">
                        Promotion
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
                      <td>{{ item.calories }}</td>
                      <td>{{ item.calories }}</td>
                      <td>{{ item.calories }}</td>
                      <td>{{ item.calories }}</td>
                      <td>{{ item.calories }}</td>
                    </tr>
                  </tbody>
                </template>
              </v-simple-table>
          </v-col>
        </v-row>
        <v-dialog
        v-model="dialog"
        persistent
        max-width="600px"
        >
            <template v-slot:activator="{ on, attrs }">
                <v-btn
                    v-bind="attrs"
                    v-on="on"
                    absolute
                    bottom
                    right
                    color="primary"
                    elevation="2"
                    fab
                    >
                        <v-icon>mdi-plus</v-icon>
                </v-btn>
            </template>
            <v-card>
                <v-card-title>
                <span class="text-h5">Enreigstrement</span>
                </v-card-title>
                <v-card-text>
                <v-container>
                    <v-row>
                    <v-col
                        cols="12"
                        sm="12"
                    >
                        <v-select
                        :items="['Skiing']"
                        label="Enseignant"
                        ></v-select>
                    </v-col>
                    <v-col
                        cols="12"
                        sm="6"
                        md="12"
                    >
                        <v-text-field
                        type="date"
                        label="Date"
                        required
                        ></v-text-field>
                    </v-col>
                    <v-col cols="12">
                        <v-text-field
                        label="Heure d'arrivée"
                        type="time"
                        required
                        ></v-text-field>
                    </v-col>
                    <v-col cols="12">
                        <v-text-field
                        label="Heure de depart"
                        type="time"
                        required
                        ></v-text-field>
                    </v-col>
                    <v-col
                        cols="12"
                        sm="6"
                        md="12"
                    >
                        <v-text-field
                        type="text"
                        label="Matière enseignée"
                        required
                        ></v-text-field>
                    </v-col>
                    <v-col
                        cols="12"
                        sm="6"
                    >
                        <v-select
                        :items="['Skiing']"
                        label="Promotion"
                        ></v-select>
                    </v-col>
                    <v-col
                        cols="12"
                        sm="6"
                    >
                        <v-select
                        :items="['Skiing']"
                        label="Cours"
                        ></v-select>
                    </v-col>
                    </v-row>
                </v-container>
                </v-card-text>
                <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                    color="danger"
                    text
                    @click="dialog = false"
                >
                    Fermer
                </v-btn>
                <v-btn
                    color="primary"
                    @click="dialog = false"
                >
                    Sauvergarder
                </v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>
        
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
      dialog: false,
    }
  }
}
</script>

