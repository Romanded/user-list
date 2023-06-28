<template>
    <div>
        <table>
            <tr><th>Имя <button @click="nameSorting">&#9660;</button></th><th>Фамилия <button @click="surnameSorting">&#9660;</button></th><th>email</th></tr>
            <tr :key="field.id" v-for="field in table"><td>{{ field.name }}</td><td>{{ field.surname }}</td><td>{{ field.email }}</td><td><button @click="removeField(field)">удалить</button></td></tr>
        </table>
        <input type="text" v-model="searchValue">
    </div>
</template>

<script>
export default {
    data: () => ({
        fakeTable: [],
        table : [],
        nameSortingState: 'default',
        surnameSortingState: 'default',
        searchValue: ''
    }),
    methods: {
      search(value) {
          this.table = this.fakeTable.filter(e => e.name.toLowerCase().includes(value.toLowerCase()) || e.surname.toLowerCase().includes(value.toLowerCase()))
      },
      nameSorting() {
        if (this.nameSortingState === 'default') {
            this.table.sort((a,b) => a.name[0].localeCompare(b.name[0]))
            this.nameSortingState = 'reverse'
        } else {
            this.table.sort((a,b) => b.name[0].localeCompare(a.name[0]))
            this.nameSortingState = 'default'
        }
      },
      surnameSorting() {
        if (this.surnameSortingState === 'default') {
          this.table.sort((a,b) => a.surname[0].localeCompare(b.surname[0]))
          this.surnameSortingState = 'reverse'
        } else {
          this.table.sort((a,b) => b.surname[0].localeCompare(a.surname[0]))
          this.surnameSortingState = 'default'
        }
      },
      removeField(f) {
          this.fakeTable = this.table.filter(e => e !== f)
          this.table = this.fakeTable
      }
    },
    mounted() {
        fetch('https://jsonplaceholder.typicode.com/users')
            .then((response) => response.json())
            .then((json) => {
                json.forEach(e => {
                    this.table.push({
                        name: `${e.name}`.slice(0, `${e.name}`.indexOf(' ')),
                        surname: `${e.name}`.slice(`${e.name}`.indexOf(' ') + 1, `${e.name}`.length - 1),
                        email: e.email
                    })
                })
                this.fakeTable = this.table
            });
    },
    watch: {
        searchValue() {
            this.search(this.searchValue)
        }
    }
}
</script>

<style>

</style>