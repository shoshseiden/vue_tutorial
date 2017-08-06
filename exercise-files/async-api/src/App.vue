<template>
  <div>
    <header class="app-header">
      <app-nav></app-nav>
    </header>
    <main class="container">
      <router-view></router-view>
    </main>
  </div>
</template>

<script>
import AppNav from './components/AppNav';

export default {
  components: {
    AppNav
  },

  created () {
    this.$store.dispatch('fetchProducts')
  }
}

export function createProduct ({commit}, product) {
  return http.post('products', product)
    .then((response) => commit (CREATE_PRODUCT, response.body.data))
}

export function updateProduct ({commit}, product) {
  return http.put(`products/${product.id}`, product)
    .then((response) => commit(UPDATE_PRODUCT, response.body.data))
}

export function deleteProduct ({commit}, productId) {
  return http.delete(`products/${productId}`)
    .then((response) => commit(DELETE_PRODUCT, productId))
}

export function saveProduct({ commit, state }, product) {
  const index = state.all.findIndex((p) => p.id === product.id);

  // update product if it exists or create it if it doesn't
  if (index !== -1) {
    return updateProduct({ commit }, product)
  } else {
    return createProduct({ commit }, product)
  }
}
</script>
