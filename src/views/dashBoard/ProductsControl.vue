<template>
  <div class="container">
    <div class="row">
      <h1 class="mt-3">{{ title }}</h1>
      <div class="text-end mt-4">
        <button class="btn btn-primary" @click.prevent="openModal('new')">
          建立新的產品
        </button>
      </div>
      <table class="table mt-4">
        <thead>
          <tr>
            <th width="150">分類</th>
            <th>產品名稱</th>
            <th width="150">售價</th>
            <th width="150">可購買</th>
            <th width="150">編輯</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in products" :key="item.id">
            <td>{{ item.category }}</td>
            <td>{{ item.title }}</td>
            <td>{{ item.price }}</td>
            <td :style="{ color: item.is_enabled ? 'green' : 'red' }">
              {{ item.is_enabled ? "in stock" : "out of stock" }}
            </td>
            <td>
              <div class="btn-group">
                <button
                  type="button"
                  class="btn btn-outline-primary btn-sm"
                  @click.prevent="openModal('edit', item)"
                >
                  編輯
                </button>
                <button
                  type="button"
                  class="btn btn-outline-danger btn-sm"
                  @click.prevent="openModal('delete', item)"
                >
                  刪除
                </button>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
      <!-- pagination -->
      <nav>
        <ul class="pagination">
          <li class="page-item" :class="{ disabled: !pages.has_pre }">
            <a
              class="page-link"
              @click.prevent="gotoPage(pages.current_page - 1)"
              >Previous</a
            >
          </li>
          <li
            class="page-item"
            v-for="page in pages.total_pages"
            :key="page"
            :class="{ active: pages.current_page === page }"
          >
            <a class="page-link" href="#" @click.prevent="gotoPage(page)">{{
              page
            }}</a>
          </li>
          <li class="page-item" :class="{ disabled: !pages.has_next }">
            <a
              class="page-link"
              href="#"
              @click.prevent="gotoPage(pages.current_page + 1)"
              >Next</a
            >
          </li>
        </ul>
      </nav>
    </div>
    <!-- Modal -->
    <Product-Modal
      :temp-product="tempProduct"
      :update-product="updateProduct"
      :is-New="isNew"
      ref="pModal"
    ></Product-Modal>
    <!-- 刪除 -->
    <Del-Modal
      :temp-product="tempProduct"
      :del-product="delProduct"
      ref="delModal"
    ></Del-Modal>
  </div>
</template>

<script>
import ProductModal from '../../components/ProductModal.vue'
import DelModal from '../../components/DelModal.vue'
import axios from 'axios'
const { VITE_URL, VITE_PATH } = import.meta.env

export default {
  name: 'ProductsControlView',
  components: {
    ProductModal,
    DelModal
  },
  data () {
    return {
      title: '商品管理',
      products: [],
      tempProduct: {
        image: ''
      },
      pages: {},
      modalProduct: null,
      delModalProduct: null,
      isNew: false,
      newProduct: {}
    }
  },
  mounted () {
    this.getData()
  },
  methods: {
    getData () {
      const url = `${VITE_URL}api/${VITE_PATH}/admin/products`
      axios
        .get(url)
        .then((res) => {
          console.log(res.data)
          this.products = res.data.products
          this.pages = res.data.pagination
        })
        .catch(() => {
          alert('取得產品資訊失敗')
        })
    },

    updateProduct () {
      let url = `${VITE_URL}api/${VITE_PATH}/admin/product`
      let http = 'post'

      if (!this.isNew) {
        url = `${VITE_URL}api/${VITE_PATH}/admin/product/${this.tempProduct.id}`
        http = 'put'
      }

      // 型別轉換
      this.newProduct = JSON.parse(JSON.stringify(this.tempProduct))
      console.log(this.newProduct)

      axios[http](url, { data: this.newProduct })
        .then((res) => {
          alert('新增/修改成功')
          this.getData()
          this.$refs.pModal.closeModal()
        })
        .catch(() => {
          alert('新增/修改失敗')
        })
    },
    openModal (status, item) {
      if (status === 'new') {
        this.tempProduct = {
          image: ''
        }
        this.isNew = true
        this.$refs.pModal.openModal()
      } else if (status === 'edit') {
        this.tempProduct = { ...item }
        // this.tempProduct.imagesUrl = [];
        this.isNew = false
        this.$refs.pModal.openModal()
      } else if (status === 'delete') {
        this.tempProduct = { ...item }
        this.$refs.delModal.openDelModal()
      }
    },

    delProduct () {
      const url = `${VITE_URL}api/${VITE_PATH}/admin/product/${this.tempProduct.id}`

      axios
        .delete(url)
        .then((res) => {
          alert('刪除成功')
          this.$refs.delModal.closeDelModal()
          this.getData()
        })
        .catch(() => {
          alert('刪除失敗')
        })
    },
    createImages () {
      this.tempProduct.imagesUrl = []
      this.tempProduct.imagesUrl.push('')
    },
    gotoPage (page) {
      // console.log(this.$router)
      const url = `${VITE_URL}api/${VITE_PATH}/admin/products?page=${page}`
      axios.get(url).then((res) => {
        console.log(res)
        this.products = res.data.products
        this.pages = res.data.pagination
      })
    }
  }
}
</script>
