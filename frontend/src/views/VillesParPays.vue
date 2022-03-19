
    <script setup>
        import { reactive} from 'vue' ;
        import VillesItem from "./VillesItem.vue";
                
        const listeV = reactive([]);

        // Fonction qui traite les erreurs de la requête
        function showError(error) {
            document.getElementById("tableCountries").innerHTML = "Erreur: " + error;
        }

        function printCountries(){
            fetch("api/countries")
                .then(response => response.json())
                .then((json )=> {
                    let listePays=json._embedded.countries
                    console.log(listePays)
                    let select = document.getElementById("select")
                    console.log(listePays)
                    for(let c=0; c<listePays.length; c++){
                        let listVilles = listePays[c]._links.cities.href
                        select.innerHTML=select.innerHTML + `<option value="${listVilles}"> ${listePays[c].name}</option>`
                    }})
                .catch(error => showError(error))
        
        }
        

    function deleteVille(ville, index) {
        const fetchOptions = { method: "DELETE"};
        fetch(`${ville._links.city.href}`, fetchOptions)
            .then((response)=>{
                return response.json();
            })
            .then((dataJSON)=>{
                console.log(dataJSON);
                listeV.splice(index,1)
            })
            .catch((error)=>console.log(error));
}

    function printVilles(url){
        const fetchOptions = { method: "GET"};
        fetch(url.target.value, fetchOptions)
        .then((response)=>{
            return response.json();
        })
        .then((json)=>{
            for(let c=0; c<listeV.length +2; c++){
                listeV.pop()
            }
            json._embedded.cities.forEach((v)=>{listeV.push(v)})
            document.getElementById("tab").style.display="block"
            return listeV
        })
        .catch((error)=>console.log(error));
}



        printCountries()

    </script>

<template>
    <div>

    <h1>Les Villes triées par Pays</h1>

    <select id="select" @change="printVilles($event)"></select>

    <div id="tab" style="display: none;">
        <h2>Liste des villes du pays selectionné</h2>
        <TABLE id="tableau">
        <TR><TH>Nom</TH><TH>Population</TH> <TH>Supprimé</TH></TR>
        <VillesItem v-for="(ville, index) in listeV" 
        :key="index"
        :index="index"
        :ville="ville"
        @deleteC="deleteVille"
    ></VillesItem>
        </TABLE>
    </div>
    </div>
    </template>

