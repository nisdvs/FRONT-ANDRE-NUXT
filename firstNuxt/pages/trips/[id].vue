<script setup>
    const route = useRoute();
    console.log("route", route); 

    const {data: trip} = await useFetch(`http://localhost:8000/trip/${route.params.id}`)
    const {data: availabilities} = await useFetch(`http://localhost:8000/availability/`)
        query: {'tripFK: route.params.id, onlyAvailable: true'}

    //const { data: peopleFound } = await useFetch(`https://swapi.dev/api/people/${route.params.id}`)

    //let character;
    //const getCharacter = () => console.log("Character Chosen:", character);

    let showForm = false;
    const setshowForm = () => {
        showForm = true;
        console.log("showForm", showForm)
        refreshNuxtData() 

    }

    let startDate;
    let endDate;
    let peopleAmount;
    let hasPet;

    const sendForm = async () => {
        console.log("startDate", startDate)
        console.log("endDate", endDate)
        console.log("peopleAmount", peopleAmount)
        console.log("hasPet", hasPet)
    }

    await useFetch('http://localhost:8000/booking/', {
        method: 'POST',
        body: {
            customuserFK: 1,
            tripFK: route.params.id,
            startDate: startDate,
            endDate: endDate,
            people: peopleAmount,
            hasPet: hasPet
        }
    })

</script>

<template>
    <div>
        <h1>
            #{{ trip.id }}: {{ trip.title }} 
        </h1>
        <p>
            {{ trip.description }}
        </p>
        <span>
            Endereço:
        </span>
        <span>
            {{ trip.address }}
        </span>
        <br>
        <h3>
            Investimento: R$ {{ trip.daily }}
        </h3>

        <!-- <p
            v-for="availability in availabilities.results"
            :key="availability.id"> {{ availability.date }}
        </p> -->

        <select>
                <option
                    v-for="availability in availabilities.results"
                    :key="availability.id" :value="availability.date">
                    {{availability.date}}
                </option>
        </select>



        <button @click="setshowForm"> Reserve Já!</button>


        <section v-if="showForm == true">
            <div>
                <label for="">Data Inicial:</label> <input type="date" v-model="startDate"> <br>
            </div>
            <div>
                <label for="">Data Final:</label> <input type="date" v-model="endDate"> <br>
            </div>
            <div>
                <label for="">Qtd Pessoas:</label> <input type="number" v-model="peopleAmount"> <br>
            </div>
            <div>
                <label for="">Tem Pet?</label> <input type="checkbox" v-model="hasPet"> <br>
            </div>
            <button @click="sendForm"> ENVIAAAAAAAAAAAAAAAAAAA</button>
        </section>
        <!-- <p>
            Nome: {{ peopleFound.name }}
        </p>
        <p>
            Height: {{ peopleFound.height }}
        </p> 
        
        <h3>
            Do you want another character?
        </h3>
        <label for = "">Character Id: </label><input v-model="character" type="number">
        <button @click="getCharacter">Find</button> -->
    </div>
</template>