<template>
    <ul>
      <li v-for="pagina in paginasRange" :key="pagina">
        <router-link :to="{query: query(pagina)}">{{ pagina }}</router-link>
      </li>
    </ul>
</template>

<script>
export default {
  name: 'ProdutosPaginacao',
  props: {
    produtosTotal: {
      type: Number,
      default: 1
    },
    produtosPorPagina: {
      type: Number,
      default: 1
    }
  },
  methods: {
    query(pagina) {
      return {
        ...this.$route.query,
        _page: pagina
      };
    }
  },
  computed: {
    paginasRange() {
      const current = Number(this.$route.query._page);
      const range = 9;
      const offset = Math.ceil(range / 2);
      const total = this.paginasTotal;
      const pagesArray = [];

      for (let i = 1; i <= total; i++) {
        pagesArray.push(i);
      }

      pagesArray.splice(0, current - offset);
      pagesArray.splice(range, total);

      return pagesArray;
    },
    paginasTotal() {
      return Math.ceil(this.produtosTotal / this.produtosPorPagina);
    }
  }
}

</script>

<style>
ul {
  grid-column: 1 / -1;
}

li {
  display: inline-block;
}

li a {
  padding: 2px 8px;
  border-radius: 2px;
  margin: 4px;
}

li a.router-link-exact-active,
li a:hover {
  background: #87f;
  color: #fff;
}
</style>
