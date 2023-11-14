<template>
    <section class="awesome-table">
        <table>
            <thead>
                <tr>
                    <th v-for="(obj, ind) in config" :key="ind">
                        {{ obj.title }}
                    </th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(row, index) in theData" :key="index">
                    <td v-for="(obj, ind) in config" :key="ind">
                        <span v-if="obj.type === 'text'">{{row[obj.key]}}</span>
                        <span v-if="obj.type === 'array'">
                            <button v-if="theData[index].pilots.length === 0" disabled>No pilots found</button>
                            <button v-else @click="getPilots(theData[index].pilots)"> {{ theData[index].pilots.length }} pilot(s) found</button>
                        </span>
                    </td>
                </tr>
            </tbody>
        </table>
        
    </section>
</template>

<script>
export default {
    props: ['theData', 'config'],
    methods: {
        getPilots(pilots){
            
            this.$emit('pilotsClicked', pilots)
            
        }
    }
}
</script>

<style lang="scss">

.awesome-table {
    border: 1px solid #999;
    border-radius: 4px;
    color: #333;
    overflow: auto;
    margin-top: 30px;

    figure {
        margin-block-start: 0;
        margin-block-end: 0;
        margin-inline-start: 0;
        margin-inline-end: 0;

        img {
            border: 1px solid #bbb;
            border-radius: 50%;
            overflow: hidden;
        }
    }

    table {
        border-collapse: collapse;
        width: 100.1%;

        th {
            position: sticky;
            top: 0;
            background: #f1f1f1;
            padding: 10px 5px;
            text-align: left;
            font-weight: bold;
            border-bottom: 1px solid #999;
        }
        
        td {
            background: white;
            padding: 5px 5px;
            text-align: left;
        }
    }
}


</style>