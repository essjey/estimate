<script setup>
    import IconCopy from './icons/IconCopy.vue'
    import estimatesData from './estimates.json'
</script>

<template>
  <div class="estimate-wrapper">
    <header>
      <h1>YGS::Tools</h1>
    </header>
    <main>
      <div class="left">
        <aside class="controls">
          <input v-model="searchText" placeholder="search..." required />
          <button @click="sortAZ">A-Z</button>
          <button @click="sortZA">Z-A</button>
          <button @click="sortLowest">Low</button>
          <button @click="sortHighest">High</button>
        </aside>
      </div>
      <div class="right">
        <h3>Current Estimates:</h3>
        <div
          v-for="estimate in filteredEstimates"
          :key="estimate.number"
          class="estimate"
        >
          <div class="estimate--info">
            <h4 @click="copyContent($event)">{{ estimate.number }}</h4>
            <p @click="copyContent($event)">{{ estimate.name }}</p>
            <p @click="copyContent($event)">{{ estimate.stock }}</p>
            <p @click="copyContent($event)">{{ estimate.size }}</p>
            <p @click="copyContent($event)">{{ estimate.color }}</p>
          </div>
          <button
            class="estimate--copy-btn"
            @click="copyEstInfo($event)"
            v-bind="{
              'data-estimate': estimate.number,
              'data-name': estimate.name,
              'data-stock': estimate.stock,
              'data-size': estimate.size,
              'data-color': estimate.color,
            }"
          >
            Copy
            <IconCopy />
          </button>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchText: '',
      estimates: estimatesData,
      copyMessage: 'the following information has been copied to your clipboard:',
    };
  },
  methods: {
    sortAZ() {
      this.estimates.sort((a, b) => (a.name > b.name ? 1 : -1))
    },
    sortZA() {
      this.estimates.sort((a, b) => (a.name < b.name ? 1 : -1))
    },
    sortLowest() {
      this.estimates.sort((a, b) => (a.number > b.number ? 1 : -1))
    },
    sortHighest() {
      this.estimates.sort((a, b) => (a.number < b.number ? 1 : -1))
    },
    copyContent(event) {
      let content = event.target.innerText
      console.log(this.copyMessage, content)
      navigator.clipboard.writeText(content)
    },
    copyEstInfo(event) {
      let est = event.target.getAttribute('data-estimate')
      let name = event.target.getAttribute('data-name')
      let stock = event.target.getAttribute('data-stock')
      let size = event.target.getAttribute('data-size')
      let color = event.target.getAttribute('data-color')

      console.log(this.copyMessage);

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

      return {
        estimate : est, 
        name: name, 
        stock: stock, 
        size : size, 
        color : color
      }
    },
  },
  computed: {
    filteredEstimates() {
      let filter = new RegExp(this.searchText, 'i');
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
    &--info {
      > * {
        cursor: pointer;

        &:after {
          content: 'Copy';
          display: inline;
          opacity: 0;
          position: relative;
          font-size: 12px;
          left: 10px;
          top: 0;
          background-color: var(--vt-c-black-soft);
          padding: 0px 5px 2px;
          border-radius: 4px;
          transition: .15s opacity ease;
        }
        &:hover::after {
          opacity: 1;
        }
      }
      p {
        font-size: 14px;
      }
    }
    &--copy-btn {
        font-size: 12px;
        font-weight: bold;
        line-height: 1em;
        margin-top: -2em;
        color: var(--text-color);
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
            margin-top: 5px;
            // removing this will break click event
            pointer-events: none;
        }

        &:hover {
            opacity: 1;
        }
    }
}
main {
  display: flex;
  flex-direction: column;
}
@media (min-width: 1024px) {
  h1 {
    padding: 0 20px;
  }
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
  main {
    flex-direction: row;
    
    .left, .right {
      flex: 0 0 50%;
      padding: 20px;
    }

    .left {
      max-width: 300px;
      aside {
        position: sticky;
        top: 20px;
      }
    }
    .right {
      min-width: 400px;
      flex-grow: 2;
    }
  }
}
</style>