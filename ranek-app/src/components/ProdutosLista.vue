<template>
 <section class="produtos-container">
 <transition mode="out-in">
  <div v-if="produtos && produtos.length" class="produtos" key="produtos">
    <div v-for="produto, index in produtos" :key="produto.id + index" class="produto">
      <router-link to="/">
        <img v-if="produto.foto" :src="produto.fotos[0].src" :alt="produto.fotos[0].titulo">
        <p class="preco">{{ produto.preco }}</p>
        <h2 class="titulo">{{ produto.nome }}</h2>
        <p>{{ produto.descricao }}</p>
      </router-link>
    </div>

    <ProdutosPaginacao  :produtosTotal='produtosTotal' :produtosPorPagina='perPage' />
  </div>

  <div v-else-if="produtos && !produtos.length" key="sem-resultados">
    <p class="sem-resultados">Busca sem resultados. Tente buscar outro termo.</p>
  </div>
  <PaginaCarregando key="carregando" v-else />

</transition>
 </section>
</template>

<script>
import ProdutosPaginacao from '@/components/ProdutosPaginacao.vue';
import { api } from '@/api';
import { serialize } from '@/helpers.js';

export default {
  name: 'ProdutosLista',
  components: {
    ProdutosPaginacao,
  },
  data() {
    return {
      produtos: null,
      perPage: 9,
      produtosTotal: 0,

    }
  },
  computed: {
    url() {
      return '/produto?_limit=' + this.perPage + serialize(this.$route.query);
    }

  },
  watch: {
    url() {
      this.getProdutos();
    }
  },
  methods: {
    getProdutos() {
      this.produtos = null;
      setTimeout(() => {
        api.get(this.url)
        .then(r => {
          this.produtos = r.data;
          this.produtosTotal = Number(r.headers['x-total-count']);
        });
      }, 1500)
    },
  },
  created() {
    this.getProdutos();
  },
}
</script>

<style scoped>
.produtos-container {
  max-width: 1000px;
  margin: 0 auto;
}

.produtos {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 30px;
  margin: 30px;
}

@media screen and (max-width: 500px) {
  .produtos {
    grid-template-columns: repeat(2, 1fr);
    grid-gap: 10px;
    margin: 10px;
  }
}

.produto {
  box-shadow: 0 4px 8px rgba(30, 60, 90, 0.1);
  padding: 10px;
  background: #fff;
  border-radius: 4px;
  transition: all 0.2s;
}

.produto:hover {
  box-shadow: 0 6px 12px rgba(30, 60, 90, 0.2);
  transform: scale(1.1);
  position: relative;
  z-index: 1;
}

.produto img {
  border-radius: 4px;
  margin-bottom: 20px;
}

.titulo {
  margin-bottom: 10px;
}

.preco {
  color: #e80;
  font-weight: bold;
}

.sem-resultados {
  text-align: center;
}
</style>
