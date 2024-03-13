<template>
  <div
    id="productModal"
    ref="productModal"
    class="modal fade"
    tabindex="-1"
    aria-labelledby="productModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog modal-xl">
      <div class="modal-content border-0">
        <div class="modal-header bg-dark text-white">
          <h5 id="productModalLabel" class="modal-title">
            <span v-if="isNew">新增產品</span>
            <span v-else>編輯產品</span>
          </h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          <div class="row">
            <div class="col-sm-4">
              <div class="mb-3">
                <label for="image" class="form-label">ImageUrl</label>
                <input
                  id="image"
                  v-model="editProduct.imageUrl"
                  type="text"
                  class="form-control mb-2"
                  placeholder="請輸入圖片連結"
                />
                <img class="img-fluid" :src="editProduct.imageUrl" />
              </div>
            </div>
            <div class="col-sm-8">
              <div class="mb-3">
                <label for="title" class="form-label">Product Name</label>
                <input
                  id="title"
                  v-model="editProduct.title"
                  type="text"
                  class="form-control"
                  placeholder="請輸入產品名稱"
                />
              </div>

              <div class="row">
                <div class="mb-3 col-md-4">
                  <label for="category" class="form-label">Category</label>
                  <input
                    id="category"
                    v-model="editProduct.category"
                    type="text"
                    class="form-control"
                    placeholder="請輸入分類"
                  />
                </div>
                <div class="mb-3 col-md-4">
                  <label for="origin_price" class="form-label">Original Price</label>
                  <input
                    id="origin_price"
                    v-model="editProduct.origin_price"
                    type="number"
                    min="0"
                    class="form-control"
                    placeholder="請輸入原價"
                  />
                </div>
                <div class="mb-3 col-md-4">
                  <label for="price" class="form-label">Price</label>
                  <input
                    id="price"
                    v-model.number="editProduct.price"
                    type="number"
                    min="0"
                    class="form-control"
                    placeholder="請輸入售價"
                  />
                </div>
              </div>

              <div class="row">
                <div class="mb-3 col-md-6">
                  <label for="inventory" class="form-label">Inventory</label>
                  <input
                    id="inventory"
                    v-model="editProduct.num"
                    class="form-control"
                    placeholder="請輸入存貨數量"
                  />
                </div>
                <div class="col-md-3 mt-5 mb-3">
                  <div class="form-check">
                    <input
                      id="is_enabled"
                      v-model="editProduct.is_enabled"
                      class="form-check-input"
                      type="checkbox"
                      :true-value="1"
                      :false-value="0"
                    />
                    <label class="form-check-label" for="is_enabled">In Stock</label>
                  </div>
                </div>
              </div>
              <hr />

              <div class="row mb-3">
                <div class="col-md-3">
                  <label for="unit" class="form-label">Unit</label>
                  <input
                    id="unit"
                    v-model.number="editProduct.unit"
                    type="text"
                    min="0"
                    class="form-control"
                    placeholder="請輸入數量"
                  />
                </div>
                <div class="col-md-3">
                  <label for="star" class="form-label">Stars</label>
                  <input
                    id="star"
                    v-model.number="editProduct.star"
                    type="number"
                    min="1"
                    max="5"
                    class="form-control"
                    placeholder="請輸入評分"
                  />
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-outline-secondary" data-bs-dismiss="modal">
            取消
          </button>
          <button type="button" class="btn btn-primary" @click="updateProduct">確認</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Modal } from 'bootstrap'
export default {
  props: ['tempProduct', 'updateProduct', 'isNew'],
  data () {
    return {
      modalProduct: null,
      editProduct: {
        image: ''
      }
    }
  },
  methods: {
    openModal () {
      this.modalProduct.show()
    },
    closeModal () {
      this.modalProduct.hide()
    }
  },
  mounted () {
    this.modalProduct = new Modal(this.$refs.productModal)
  },
  watch: {
    tempProduct () {
      this.editProduct = this.tempProduct
    }
  }
}
</script>
