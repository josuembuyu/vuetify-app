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
                      v-for="item in sexes.data"
                      :key="item.id"
                    >
                      <td>{{ item.code_sexe }}</td>
                      <td>{{ item.sexe }}</td>
                    </tr>
                  </tbody>
                </template>
              </v-simple-table>
          </v-col>
          <v-col cols="12" sm="8" md="6">
            <v-card style="padding: 2em" class="mx-auto" elevation="2">
                <v-alert
                  v-if="message.state"
                  :type="message.type"
                > {{ message.text }}</v-alert>
                <v-form
                  @submit="create"
                  id="create-form" 
                  ref="form"
                  v-model="valid"
                  lazy-validation
                >

                  <v-text-field
                    v-model="code_sexe"
                    label="CODE SEXE"
                    required
                  ></v-text-field>
                  <v-select
                    v-model="sexe"
                    :items="items"
                    :rules="[v => !!v || 'Item is required']"
                    label="SEXE"
                    required
                  ></v-select>
                  <v-row>
                    <v-col cols="12" sm="8" md="12">
                      <v-btn
                      :disabled="loading.state"
                      form="create-form" 
                      type="submit"
                      block
                      x-large
                      class="mr-4"
                      :color="loading.state ? '' : 'success'"
                    >
                      {{ loading.text }}
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
            code_sexe: null,
            sexe: null,
            sexes: [],
            message: {
				type: "danger",
				text: null,
				state: false
			},
			loading: {
				text: "Sauvegarder",
				state: false
			},
        }
    },
    methods: {
        async getSexe(){
            try {
                let response = await this.$axios.get('/sexe');
                this.sexes = response;
            } catch (error) {
                alert("Une erreur est survenue, veuillez recharger la page")
            }
        },
        async create(e){
            e.preventDefault()

            this.loading = {
                text : "Patientez...",
                state : true
            }

            try {
                let payload = {
                    "code_sexe": this.code_sexe,
                    "sexe": this.sexe
                }

                let response = await this.$axios.post('/sexe', payload)

                if (response) {
                    this.message = {
                        type: "success",
                        text: "Le sexe a été crée avec succès",
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
                    type: "danger",
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
    mounted() {
        this.getSexe()
    }
}
</script>

