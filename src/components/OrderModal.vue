<template>
  <div
    class="modal fade"
    id="productModal"
    tabindex="-1"
    role="dialog"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
    ref="productModal"
  >
    <div class="modal-dialog modal-xl" role="document">
      <div class="modal-content border-0">
        <div class="modal-header bg-dark text-white">
          <h5 class="modal-title" id="exampleModalLabel">
            <span>Order Detail '{{ tempOrder.id }}'</span>
          </h5>
          <button
            type="button"
            class="btn-close-white"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          <div class="row">
            <div class="mb-3 col-md-4">
              <label for="product" class="form-label">Product</label>
              <div v-for="(product, index) in tempOrder.products" :key="index">
                <input
                  v-model="tempOrder.products[index].product.title"
                  type="text"
                  placeholder="Product Title"
                />
              </div>
            </div>
            <div class="mb-3 col-md-4">
              <label for="price" class="form-label">Each Price</label>
              <div v-for="(product, index) in tempOrder.products" :key="index">
                <input
                  v-model.number="tempOrder.products[index].product.price"
                  type="text"
                  placeholder="Product price"
                />
              </div>
            </div>
            <div class="mb-3 col-md-4">
              <label for="qty" class="form-label">Product Quantity</label>
              <div v-for="(product, index) in tempOrder.products" :key="index">
                <input
                  v-model.number="tempOrder.products[index].qty"
                  type="text"
                  placeholder="Product Quantity"
                />
              </div>
            </div>
          </div>
          <hr />
          <div class="row">
            <div class="col">
              <label for="total" class="form-label">Total</label>
              <input
                id="total"
                v-model.number="tempOrder.total"
                type="text"
                min="0"
                class="form-control"
              />
            </div>
            <div class="col">
              <div class="form-check">
                <input
                  id="is_paid"
                  v-model="tempOrder.is_paid"
                  class="form-check-input"
                  type="checkbox"
                  :true-value="1"
                  :false-value="0"
                />
                <label class="form-check-label" for="is_paid">Is paid</label>
              </div>
            </div>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-outline-secondary"
              data-bs-dismiss="modal"
            >
              取消
            </button>
            <button
              type="button"
              class="btn btn-primary"
              @click="$emit('update-paid', tempOrder)"
            >
              修改付款狀態
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { Modal } from 'bootstrap'

export default {
  props: ['order', 'update-paid'],
  data () {
    return {
      modal: null,
      tempOrder: {},
      isPaid: false
    }
  },
  emits: ['update-paid'],
  methods: {
    openModal () {
      this.modal.show()
    },
    closeModal () {
      this.modal.hide()
    }
  },
  mounted () {
    this.modal = new Modal(this.$refs.productModal)
  },
  watch: {
    order () {
      this.tempOrder = this.order
    }
  }
}
</script>
