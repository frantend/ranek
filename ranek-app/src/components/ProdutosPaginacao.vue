<template>
  <div>
    <p>Páginas {{ paginasTotal }} </p>  
    <ul>
      <li v-for="pagina in paginasTotal" :key="pagina">
        <router-link :to="{query: query(pagina)}">{{ pagina }}</router-link>
      </li>
    </ul>
  </div>
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
