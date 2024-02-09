<template>
    <main>
        <div>
            <h1>Les clients</h1>
        </div>
        <div>
            <table>
                <caption>La Liste des clients</caption>
                <tr>
                    <th>Code</th>
                    <th>Société</th>
                    <th>Contact</th>
                    <th>Ville</th>
                </tr>
                <!-- Si le tableau des clients est vide -->
                <tr v-if="data.listeClients.length === 0">
                    <td colspan="4">Veuillez patienter, chargement des catégories...</td>
                </tr>
                <!-- Si le tableau des <td>{{ clients.contact }}</td> n'est pas vide -->
                <tr v-for="clients in data.listeClients" :key="clients.code">
                    <td>{{ clients.code }}</td>
                    <td>{{ clients.societe }}</td>
                    <td>{{ clients.contact }}</td>
                    <td>{{ clients.adresse.ville }}</td>
                </tr>
            </table>
        </div>
    </main>
</template>

<script setup>
import { reactive, onMounted } from "vue";
import { doAjaxRequest } from "@/api";

let data = reactive({

    listeClients: []
});

function showError(error) {
    console.log("Erreur : status %d", error.status)
    console.log(error.body);
    alert(error.message);
}

function chargeClients() {
    
    doAjaxRequest("/api/clients")
        .then((json) => {
            data.listeClients= json._embedded.clients;
        })
        .catch(showError);
}


// A l'affichage du composant, on affiche la liste
onMounted(chargeClients);

</script>


<style scoped>
td,
th {
    border: 1px solid #ddd;
    padding: 8px;
}

th {
    padding-top: 12px;
    padding-bottom: 12px;
    text-align: left;
    background-color: #232623;
    color: rgb(255, 255, 255);
}
</style>

