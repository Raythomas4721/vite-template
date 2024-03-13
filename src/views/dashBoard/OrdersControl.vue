<template>
  <div>
    <table class="table mt-4">
      <thead>
        <tr>
          <th>購買時間</th>
          <th>購買品項</th>
          <th>Message</th>
          <th>應付金額</th>
          <th>是否付款</th>
          <th>編輯</th>
        </tr>
      </thead>
      <tbody>
        <template v-for="item in orders">
          <tr
            v-if="orders.length"
            :class="{ 'text-secondary': !item.is_paid }"
            :key="item + '123'"
          >
            <td>{{ formatDate(item.create_at) }}</td>
            <td>
              <span v-for="(productOut, index) in item.products" :key="index">
                {{ productOut.product.title }} |
              </span>
            </td>
            <td>{{ item.message }}</td>
            <td>{{ item.total }}</td>
            <td :style="{ color: item.is_paid ? 'green' : 'red' }">
              {{ item.is_paid ? "Paid" : "Not Paid" }}
            </td>
            <td>
              <div class="btn-group">
                <button
                  class="btn btn-outline-primary btn-sm"
                  type="button"
                  @click.prevent="openModal(item)"
                >
                  檢視
                </button>
                <button
                  class="btn btn-outline-danger btn-sm"
                  type="button"
                  @click.prevent="openDelOrderModal(item)"
                >
                  刪除
                </button>
              </div>
            </td>
          </tr>
        </template>
      </tbody>
      <Order-Modal
        ref="orderModal"
        :order="tempOrder"
        @update-paid="updatePaid"
      ></Order-Modal>
      <Del-Order-Modal
        :temp-order="tempOrder"
        :del-order="delOrder"
        ref="delOrderModal"
      ></Del-Order-Modal>
    </table>
  </div>
</template>

<script>
import axios from 'axios'
import OrderModal from '../../components/OrderModal.vue'
import DelOrderModal from '../../components/DelOrderModal.vue'
const { VITE_URL, VITE_PATH } = import.meta.env

export default {
  name: 'OrdersControlView',
  data () {
    return {
      orders: {},
      tempOrder: {},
      currentPage: 1
    }
  },
  components: {
    OrderModal,
    DelOrderModal
  },
  methods: {
    getOrders () {
      const api = `${VITE_URL}api/${VITE_PATH}/admin/orders`
      axios
        .get(api)
        .then((res) => {
          console.log(res.data.orders)
          this.orders = res.data.orders
        })
        .catch(() => {})
    },
    openModal (item) {
      this.tempOrder = { ...item }
      this.$refs.orderModal.openModal()
    },
    openDelOrderModal (item) {
      this.tempOrder = { ...item }
      this.$refs.delOrderModal.openDelModal()
    },
    updatePaid (item) {
      const api = `${VITE_URL}api/${VITE_PATH}/admin/order/${item.id}`
      const updatedItem = { is_paid: item.is_paid === 1 }
      const dataWithIsPaid = JSON.parse(JSON.stringify(updatedItem))
      console.log(dataWithIsPaid)
      axios
        .put(api, { data: dataWithIsPaid })
        .then(() => {
          this.getOrders()
          this.$refs.orderModal.closeModal()
        })
        .catch(() => {})
    },
    delOrder (item) {
      const api = `${VITE_URL}api/${VITE_PATH}/admin/order/${item.id}`
      axios
        .delete(api)
        .then((res) => {
          this.$refs.delOrderModal.closeDelModal()
          alert(res.data.message)
          this.getOrders()
        })
        .catch(() => {})
    },
    formatDate (createdAt) {
      const date = new Date(createdAt * 1000) // Convert seconds to milliseconds
      const year = date.getFullYear()
      const month = (date.getMonth() + 1).toString().padStart(2, '0')
      const day = date.getDate().toString().padStart(2, '0')
      return `${year}-${month}-${day}`
    }
  },
  mounted () {
    this.getOrders()
  }
}
</script>
