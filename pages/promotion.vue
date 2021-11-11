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
                        CODE PROMO
                      </th>
                      <th class="text-left">
                        PROMO
                      </th>
                      <th class="text-left">
                        CODE DEPART
                      </th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr
                      v-for="item in promotions.data"
                      :key="item.id"
                    >
                      <td>{{ item.code_pro }}</td>
                      <td>{{ item.promotion }}</td>
                      <td>{{ item.departement }}</td>
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
                  ref="form"
                  lazy-validation
                  id="create-form"
                  @submit="create"
                >

                  <v-text-field
                    v-model="code_promo"
                    label="CODE PROMO"
                    required
                  ></v-text-field>
                  <v-text-field
                    v-model="promotion"
                    label="PROMO"
                    required
                  ></v-text-field>
                  <v-select
                    v-model="departement"
                    :items="departements"
                    :rules="[v => !!v || 'Item is required']"
                    label="DEPARTEMENT"
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
        code_promo: null,
        promotion: null,
        departement: null,
        promotions: [],
        departements: [],
        message: {
            type: "alert-danger",
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
    async getPromo(){
        try {
            let response = await this.$axios.get('/promotion');
            this.promotions = response;

        } catch (error) {
            alert("Une erreur est survenue, veuillez recharger la page")
        }
    },
    async getDepartement(){
        try {
            let response = await this.$axios.get('/departement');
            
            if (response.data) {
                var items = response.data
                var newArray = []

                items.map(item => {
                    newArray.push(item.departement)
                })

                this.departements = newArray;

            }

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
			"code_pro": this.code_promo,
            "promotion": this.promotion,
            "departement": this.departement
		}

        let response = await this.$axios.post('/promotion', payload)

        if (response) {
            this.message = {
                type: "success",
                text: "La promotion a été crée avec succès",
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
	this.getDepartement()
    this.getPromo()
  }
}
</script>

