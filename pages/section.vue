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
                      v-for="item in sections.data"
                      :key="item.id"
                    >
                      <td>{{ item.code_sec }}</td>
                      <td>{{ item.section }}</td>
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
                    v-model="code_section"
                    label="CODE SECTION"
                    required
                  ></v-text-field>
                  <v-text-field
                    v-model="section"
                    label="SECTION"
                    required
                  ></v-text-field>
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
      sections: [],
      code_section: null,
      section: null,
      message: {
				type: "alert-danger",
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
    async getSection(){
      try {
        let response = await this.$axios.get('/section');
        this.sections = response;
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
					"code_sec": this.code_section,
          "section": this.section
				}

        let response = await this.$axios.post('/section', payload)

        if (response) {
          this.message = {
						type: "success",
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
  mounted() {
    this.getSection()
  }
}
</script>

