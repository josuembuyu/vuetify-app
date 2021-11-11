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
                    <v-form
                    ref="form"
                    lazy-validation
                    id="create-form"
                    @submit="create"
                    >
                         <v-container>
                            <v-row>
                            <v-col
                                cols="12"
                                sm="12"
                            >
                                <v-select
                                v-model="enseignant"
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
                                v-model="date"
                                label="Date"
                                required
                                ></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field
                                label="Heure d'arrivée"
                                v-model="heurearriv"
                                type="time"
                                required
                                ></v-text-field>
                            </v-col>
                            <v-col cols="12">
                                <v-text-field
                                v-model="heuredepart"
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
                                v-model="matiere"
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
                                v-model="promotion"
                                :items="['Skiing']"
                                label="Promotion"
                                ></v-select>
                            </v-col>
                            <v-col
                                cols="12"
                                sm="6"
                            >
                                <v-select
                                v-model="cours"
                                :items="['Skiing']"
                                label="Cours"
                                ></v-select>
                            </v-col>
                            </v-row>
                        </v-container>
                    </v-form>
                   
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
                    :disabled="loading.state"
                    form="create-form" 
                    type="submit"
                    :color="loading.state ? '' : 'success'"
                >
                {{ loading.text }}
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
        date: null,
        enseignant: null,
        heurearriv: null,
        heuredepart: null,
        cours: null,
        matiere: null,
        promotion: null,
        dialog: false,
        dispenser: [],
        enseignants: [],
        promotions: [],
        cours: [],
        message: {
            type: "danger",
            text: null,
            state: false
        },
        loading: {
            text: "Sauvegarder",
            state: false
        }
    }
  },
  methods: {
    async getDispenser(){
        try {
            let response = await this.$axios.get('https://618c962dded7fb0017bb9603.mockapi.io/dispenser');
            this.dispenser = response;
        } catch (error) {
            alert("Une erreur est survenue lors de la recuperation des données, rechargez la page")
        }
    },
    async getEnseignant(){

    },
    async getPromotions(){
        
    },
    async getCours(){
        
    },
    async create(e){
        e.preventDefault()

        this.loading = {
            text : "Patientez...",
            state : true
        }

        try {
            let payload = {
                "date": this.date,
                "enseignant": this.enseignant,
                "heurearriv": this.heurearriv,
                "heuredepart": this.heuredepart,
                "cours": this.cours,
                "matiere": this.matiere,
                "promotion": this.promotion,
            }

            let response = await this.$axios.post('https://618c962dded7fb0017bb9603.mockapi.io/dispenser', payload)

            if (response) {
                this.message = {
                    type: "success",
                    text: "L'enregistrement a été crée avec succès",
                    state: true
                }
                this.loading = {
                    text: "Sauvegarder",
                    state: false
                }

                window.location.reload(true)

            } else {
                this.message = {
                    type: "danger",
                    text: "Une erreur est survenue lors de la création d'un departement",
                    state: true
                }
                this.loading = {
                    text: "Ressayez",
                    state: false
                }
            }

        } catch (error) {
                this.message = {
                    type: "alert-danger",
                    text: error,
                    state: true
                }
                this.loading = {
                    text: "Ressayez",
                    state: false
                }
        }
    }
  },
  mounted(){
      this.getDispenser()
      this.getEnseignant()
      this.getPromotions()
      this.getCours()
  }
}
</script>

