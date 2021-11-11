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
                            Matricule
                        </th>
                        <th class="text-left">
                            Prénom
                        </th>
                        <th class="text-left">
                            Nom
                        </th>
                        <th class="text-left">
                            Postnom
                        </th>
                        <th class="text-left">
                            Grade
                        </th>
                        <th class="text-left">
                            Sexe
                        </th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr
                        v-for="item in enseignants.data"
                        :key="item.id"
                        >
                        <td>{{ item.matricule }}</td>
                        <td>{{ item.prenom }}</td>
                        <td>{{ item.nom }}</td>
                        <td>{{ item.postnom }}</td>
                        <td>{{ item.grade }}</td>
                        <td>{{ item.sexe }}</td>
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
                                    label="Matricule"
                                    v-model="matricule"
                                    type="text"
                                    required
                                    ></v-text-field>
                                </v-col>
                                <v-col cols="6">
                                    <v-text-field
                                    label="Prénom"
                                    v-model="prenom"
                                    type="text"
                                    required
                                    ></v-text-field>
                                </v-col>
                                    <v-col cols="6">
                                    <v-text-field
                                    label="Nom"
                                    v-model="nom"
                                    type="text"
                                    required
                                    ></v-text-field>
                                </v-col>
                                <v-col cols="6">
                                    <v-text-field
                                    label="Postnom"
                                    v-model="postnom"
                                    type="text"
                                    required
                                    ></v-text-field>
                                </v-col>
                                <v-col cols="12">
                                    <v-text-field
                                    label="Grade"
                                    v-model="grade"
                                    type="text"
                                    required
                                    ></v-text-field>
                                </v-col>
                                <v-col
                                    cols="12"
                                >
                                    <v-select
                                    v-model="sexe"
                                    :items="sexes"
                                    label="Sexe"
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
</template>

<script>
export default {
    data(){
        return {
            dialog: false,
            matricule: null,
            prenom: null,
            nom: null,
            postnom: null,
            grade: null,
            sexe: null,
            sexes: [],
            enseignants: [],
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
        async getEnseignant(){
            try {
                let response = await this.$axios.get('https://618c962dded7fb0017bb9603.mockapi.io/enseignant');
                this.enseignants = response;
            } catch (error) {
                alert("Une erreur est survenue, veuillez recharger la page")
            }
        },
        async getSexe(){
            try {
                let response = await this.$axios.get('/sexe');
                if (response.data) {
                    var items = response.data
                    var newArray = []

                    items.map(item => {
                        newArray.push(item.sexe)
                    })

                    this.sexes = newArray;
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
                    "matricule": this.matricule,
                    "prenom": this.prenom,
                    "postnom": this.postnom,
                    "nom": this.nom,
                    "sexe": this.sexe,
                    "grade": this.grade,
                }

                let response = await this.$axios.post('https://618c962dded7fb0017bb9603.mockapi.io/enseignant', payload)

                if (response) {

                    this.message = {
                        type: "success",
                        text: "L'enseignant a été crée avec succès",
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
                        text: "Une erreur est survenue lors de la création d'un enseignant",
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
        this.getSexe()
        this.getEnseignant()
    }
}
</script>