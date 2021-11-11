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
                        CODEDEPART
                      </th>
                      <th class="text-left">
                        DEP
                      </th>
                      <th class="text-left">
                        CODESEC
                      </th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr
                      v-for="item in departements.data"
                      :key="item.id"
                    >
                      <td>{{ item.code_departement }}</td>
                      <td>{{ item.departement }}</td>
                      <td>{{ item.section }}</td>
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
                  id="create-form"
                  @submit="createDepartement"
                >

                  <v-text-field
                    v-model="code_departement"
                    :rules="emailRules"
                    label="CODEDEPART"
                    required
                  ></v-text-field>
                  <v-text-field
                    v-model="departement"
                    :rules="emailRules"
                    label="DEPARTEMENT"
                    required
                  ></v-text-field>

                  <v-select
                    v-model="section"
                    :items="sections"
                    :rules="[v => !!v || 'Item is required']"
                    label="Section"
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
      code_departement: null,
      section: null,
      departement: null,
      message: {
				type: "alert-danger",
				text: null,
				state: false
			},
			loading: {
				text: "Sauvegarder",
				state: false
			},
      departements: [],
      sections: []
    }
  },
  methods: {
    async getDepartement(){
      try {
        let response = await this.$axios.get('/departement');
        this.departements = response;

      } catch (error) {
        alert("Une erreur est survenue, veuillez recharger la page")
      }
    },
    async getSection(){
      try {
        let response = await this.$axios.get('/section');
        if (response.data) {
          var items = response.data
          var newArray = []

          items.map(item => {
            newArray.push(item.section)
          })

          this.sections = newArray;

        }


      } catch (error) {
        alert("Une erreur est survenue, veuillez recharger la page")
      }
    },
    async createDepartement(e){
      e.preventDefault()

      this.loading = {
				text : "Patientez...",
				state : true
			}

      try {
        let payload = {
					"code_departement": this.code_departement,
          "departement": this.departement,
          "section": this.section
				}

        let response = await this.$axios.post('/departement', payload)

        if (response) {
          this.message = {
						type: "alert-success",
						text: "Le departement a été crée avec succès",
						state: true
					}
					this.loading = {
						text: "Sauvegarder",
						state: false
					}

          window.location.reload(true)

        } else {
          this.message = {
						type: "alert-danger",
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
  mounted() {
		this.getDepartement()
    this.getSection()
  }
}
</script>

