<template>
  <div class="container">
    <div class="row">
      <div class="col-6 offset-3 pt-3 card mt-5 shadow">
        <div class="card-body">
          <h3>Ürün İşlemleri</h3>
          <hr>
          <div class="form-group">
            <label>Ürün Adı</label>
            <input type="text" class="form-control" v-model="product.title" placeholder="Ürün adını giriniz..">
          </div>
          <div class="form-group">
            <label>Adet</label>
            <input type="number" class="form-control" v-model="product.total" placeholder="Ürün adetini giriniz..">
          </div>
          <div class="form-group">
            <label>Fiyat</label>
            <input type="number" class="form-control" v-model="product.price" placeholder="Ürün fiyatı giriniz..">
          </div>
          <div class="form-group">
            <label>Açıklama</label>
            <textarea cols="30" rows="5" placeholder="Ürüne ait bir açıklama giriniz..."
                      v-model="product.description" class="form-control"></textarea>
          </div>
          <hr>
          <button @click="saveProduct" :disabled="saveEnabled" class="btn btn-primary">Kaydet</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import {mapGetters} from 'vuex';

  export default {
    name: "ProductPurchase",
    data() {
      return {
        product: {
          title: "",
          total: null,
          price: null,
          remaining : null,
          description: ""
        },
        saveButtonClicked: false
      }
    },
    methods: {
      saveProduct() {
        this.saveButtonClicked = true;
        this.product.remaining = this.product.total;
        this.$store.dispatch("saveProduct", this.product);
      }
    },
    computed: {
      saveEnabled() {
        if (this.product.title.length > 0 && this.product.description.length > 0 && this.product.price > 0 && this.product.total > 0) {
          return false;
        } else {
          return true;
        }
      },
    },
    beforeRouteLeave(to, from, next) {
      if ((this.product.title.length > 0 || this.product.description.length > 0 || this.product.price > 0 || this.product.total > 0) && !this.saveButtonClicked) {
        if (confirm("Kaydedilmemiş veriler var . Çıkmak İstediğinizden emin misiniz ? ")) {
          next()
        } else {
          next(false);
        }
      } else {
        next();
      }
    }
  }
</script>

<style scoped>

</style>
