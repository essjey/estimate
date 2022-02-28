<template>
    <div>
        <header>
            <h1>YGS::test</h1>
        </header>
        <main>
            <input v-model="searchText" placeholder="search..." required/>
            <button @click="sortAZ">A-Z</button>
            <button @click="sortZA">Z-A</button>
            <button @click="sortLowest">Low</button>
            <button @click="sortHighest">High</button>
            <h3>Current Estimates:</h3>
            <!-- list estimates, show filtered list based on estimateNumber -->
            <div v-for="estimate in filteredEstimates" :key="estimate.number" class="found" @click="getEstInfo(estimate.number)">
                {{ estimate.name }} | {{ estimate.number }}
            </div>
        </main>
    </div>
</template>

<script>
export default {
  data() {
    return {
        searchText: '',
        estimates: [
            { number: '12345', name: 'flyer 01', stock: '80lb White Linen', size: '8.5"x11"' },
            { number: '83410', name: 'flyer 02', stock: '25lb White Linen', size: '8.5"x11"' },
            { number: '11112', name: 'flyer 03', stock: '400lb White Linen', size: '8.5"x11"' },
            { number: '54321', name: 'postcard 01', stock: '80lb White Linen', size: '6"x9"' }
        ]
    }
  }, 
  methods: {
      sortAZ() {
          this.estimates.sort((a, b) => a.name > b.name ? 1 : -1)
      },
      sortZA() {
          this.estimates.sort((a, b) => a.name < b.name ? 1 : -1)
      },
      sortLowest() {
          this.estimates.sort((a, b) => a.number > b.number ? 1 : -1)
      },
      sortHighest() {
          this.estimates.sort((a, b) => a.number < b.number ? 1 : -1)
      }, 
      getEstInfo(event) {
          console.log(this, event)
      }
  },
  computed: {
      filteredEstimates() {
          let filter = new RegExp(this.searchText, 'i')
          return this.estimates.filter(
              el=> el.number.match(filter) || 
                   el.name.match(filter) || 
                   el.stock.match(filter) || 
                   el.size.match(filter)
          );
      }
  }
}
</script>


<style lang="scss" scoped>
    input, button { 
        width: 100%;
        margin-bottom: 15px;
        padding: 15px;
        font-size: 16px;
        font-family: sans-serif;
    }
    form {
        max-width: 400px;
        margin: auto;
    }
    button {
        background-color: hsla(160, 100%, 37%, 1);
        border: none;
    }
    h1, h3 {
        color: var(--color-heading);
        font-weight: bold;
        padding-bottom: 7px;
        margin-bottom: 7px;
        border-bottom: 1px solid var(--color-heading)
    }
    h1 {
        border-bottom: none;
        padding-bottom: 15px;
        margin-bottom: 15px;
    }
    @media (min-width: 1024px) {
        header {
            display: flex;
            place-items: center;
            // padding-right: calc(var(--section-gap) / 2);
        }
        header .wrapper {
            display: flex;
            place-items: flex-start;
            flex-wrap: wrap;
        }
    }
</style>