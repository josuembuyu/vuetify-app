<template>
    <v-container>
        <v-row>
            <v-col cols="12" sm="8" md="12">
                <v-simple-table
                >
                    <template v-slot:default>
                    <thead>
                        <tr>
                        <th class="text-left">
                            Code cours
                        </th>
                        <th class="text-left">
                            Cours
                        </th>
                        <th class="text-left">
                            Volume horaire
                        </th>
                        <th class="text-left">
                            Ponderation
                        </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr
                        v-for="item in dataList.data"
                        :key="item.id"
                        >
                        <td>{{ item.matricule }}</td>
                        <td>{{ item.cours }}</td>
                        <td>{{ item.volume }}</td>
                        <td>{{ item.ponderation }}</td> 
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
                         <v-container>
                            <v-row>
                                <v-col cols="6">
                                    <v-text-field
                                    label="Code cours"
                                    v-model="matricule"
                                    type="text"
                                    required
                                    ></v-text-field>
                                </v-col>
                                <v-col cols="6">
                                    <v-text-field
                                    label="Cours"
                                    v-model="cours"
                                    type="text"
                                    required
                                    ></v-text-field>
                                </v-col>
                                    <v-col cols="6">
                                    <v-text-field
                                    label="Volume horaire"
                                    v-model="volume"
                                    type="text"
                                    required
                                    ></v-text-field>
                                </v-col>
                                <v-col cols="6">
                                    <v-text-field
                                    label="Ponderation"
                                    v-model="ponderation"
                                    type="text"
                                    required
                                    ></v-text-field>
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
</template>

<script>
export default {
    data(){
        return {
            dialog: false,
            matricule: null,
            cours: null,
            volume: null,
            ponderation: null,
            message: {
                type: "danger",
                text: null,
                state: false
            },
            loading: {
                text: "Sauvegarder",
                state: false
            },
            dataList: []
        }
    },
    methods: {
        async getEnseignant(){
            try {
                let response = await this.$axios.get('https://618c962dded7fb0017bb9603.mockapi.io/cours');
                this.dataList = response;
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
                    "matricule": this.matricule,
                    "cours": this.cours,
                    "volume": this.volume,
                    "ponderation": this.ponderation
                }

                let response = await this.$axios.post('https://618c962dded7fb0017bb9603.mockapi.io/cours', payload)

                if (response) {

                    this.message = {
                        type: "success",
                        text: "Le a été crée avec succès",
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
                        text: "Une erreur est survenue lors de la création du cours",
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
    mounted(){
        this.getEnseignant()
    }
}
</script>