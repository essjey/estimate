<script setup>
    import IconCopy from './icons/IconCopy.vue'
</script>

<template>
  <div class="estimate-wrapper">
    <header>
      <h1>YGS::test</h1>
    </header>
    <main>
      <input v-model="searchText" placeholder="search..." required />
      <button @click="sortAZ">A-Z</button>
      <button @click="sortZA">Z-A</button>
      <button @click="sortLowest">Low</button>
      <button @click="sortHighest">High</button>
      <h3>Current Estimates:</h3>
      <div
        v-for="estimate in filteredEstimates"
        :key="estimate.number"
        class="estimate"
      >
        <div class="estimate--info">
          <h4>{{ estimate.number }}</h4>
          <p>{{ estimate.name }}</p>
          <p>{{ estimate.stock }}</p>
          <p>{{ estimate.size }}</p>
          <p>{{ estimate.color }}</p>
        </div>
        <button
          class="estimate--copy-btn"
          @click="getEstInfo($event)"
          v-bind="{
            'data-estimate': estimate.number,
            'data-name': estimate.name,
            'data-stock': estimate.stock,
            'data-size': estimate.size,
            'data-color': estimate.color,
          }"
        >
          <IconCopy />
        </button>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchText: "",
      estimates: [
        {
          number: "12345",
          name: "flyer 01",
          stock: "80lb White Linen",
          size: '8.5"x11"',
          color: "4/0",
        },
        {
          number: "83410",
          name: "flyer 02",
          stock: "25lb White Linen",
          size: '8.5"x11"',
          color: "4/4",
        },
        {
          number: "11112",
          name: "flyer 03",
          stock: "400lb White Linen",
          size: '8.5"x11"',
          color: "1/0",
        },
        {
          number: "54321",
          name: "postcard 01",
          stock: "80lb White Linen",
          size: '6"x9"',
          color: "6/4",
        },
      ],
    };
  },
  methods: {
    sortAZ() {
      this.estimates.sort((a, b) => (a.name > b.name ? 1 : -1));
    },
    sortZA() {
      this.estimates.sort((a, b) => (a.name < b.name ? 1 : -1));
    },
    sortLowest() {
      this.estimates.sort((a, b) => (a.number > b.number ? 1 : -1));
    },
    sortHighest() {
      this.estimates.sort((a, b) => (a.number < b.number ? 1 : -1));
    },
    getEstInfo(event) {
      let est = event.target.getAttribute("data-estimate");
      let name = event.target.getAttribute("data-name");
      let stock = event.target.getAttribute("data-stock");
      let size = event.target.getAttribute("data-size");
      let color = event.target.getAttribute("data-color");

      console.log(event);

      console.log(
        "the following information has been copied to your clipboard:"
      );

      console.table({
        Estimate: est,
        Name: name,
        Stock: stock,
        Size: size,
        Color: color,
      });

      navigator.clipboard.writeText(
        `${est}, ${name}, ${stock}, ${size}, ${color}`
      );

      return est, name, stock, size, color;
    },
  },
  computed: {
    filteredEstimates() {
      let filter = new RegExp(this.searchText, "i");
      return this.estimates.filter(
        (el) =>
          el.number.match(filter) ||
          el.name.match(filter) ||
          el.stock.match(filter) ||
          el.size.match(filter)
      );
    },
  },
};
</script>

<style lang="scss" scoped>
input,
button {
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
h1,
h3 {
  color: var(--color-heading);
  font-weight: bold;
  padding-bottom: 7px;
  margin-bottom: 7px;
  border-bottom: 1px solid var(--color-heading);
}
h1 {
  border-bottom: none;
  padding-bottom: 15px;
  margin-bottom: 15px;
}

.estimate {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 15px;
    padding-bottom: 15px;
    border-bottom: 1px solid #fff;

    &:last-child {
        border-bottom: none;
    }

    h4 {
        color: var(--color-heading);
        font-weight: 600;
        font-size: 18px;
    }

    p:last-child {
        margin-bottom: 0px;
    }

    &--info p {
        font-size: 14px;
    }
    &--copy-btn {
        cursor: pointer;
        box-sizing: content-box;
        padding: 0px;
        width: 40px;
        height: 40px;
        border-radius: 5px;
        margin-bottom: 0px;
        opacity: .5;
        transition: .1s opacity ease-out;
        background-color: transparent;

        svg {
            // removing this will break click event
            pointer-events: none;
        }

        &:hover {
            opacity: 1;
        }
    }
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