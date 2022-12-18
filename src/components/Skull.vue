<template>
    <div>
        <h1>Skull</h1>
        <p>{{ msg }}</p>
        <button type="button" class="btn btn-secondary" @click="getSkull()">
            Skull button
        </button>

    </div>
</template>
 
<script>
import axios from 'axios'
export default
{
    name: 'Skull',
    data()
    {
        return {
            msg: 'Hi! This is the skull component 💀'
        }
    },
    methods:
    {
        getSkull()
        {
            axios.get('http://localhost:5000/recipes')
            
            .then(response => (this.msg = response.data))
            .catch(error => console.log(error))
        }
    },
    created()
    {
        this.getSkull()
    }

    
}
</script>