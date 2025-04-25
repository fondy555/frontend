<template>
  <div>
    <Nav />
    <Banner />
    <div class="container">
      <el-breadcrumb separator="/" class="top">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>当前</el-breadcrumb-item>
      </el-breadcrumb>
      <el-card class="top">
        <el-row :gutter="24" class="product-row">
          <!-- 左侧筛选栏 -->
          <el-col :span="6">
            <el-card>
              <div style="margin-bottom: 15px;">筛选</div>

              <!-- 原有的Model筛选 -->
              <div style="margin-bottom: 10px;">Model</div>
              <div v-for="i in testList" :key="i">
                <el-checkbox v-model="checked1" :label="i" style="margin-top: 5px;">{{ i }}</el-checkbox>
              </div>

              <!-- 颜色筛选 -->
              <div style="margin-top: 20px;">颜色</div>
              <div v-for="color in colors" :key="color">
                <el-checkbox v-model="selectedColors" :label="color" style="margin-top: 5px;">{{ color }}</el-checkbox>
              </div>

              <!-- 库存筛选 -->
              <div style="margin-top: 20px;">库存</div>
              <el-slider
                v-model="stockRange"
                :min="0"
                :max="100"
                :step="1"
                range
                show-stops
                style="margin-top: 10px;"
                :format-tooltip="formatTooltip"
              >
                <span slot="start">{{ stockRange[0] }}</span>
                <span slot="end">{{ stockRange[1] }}</span>
              </el-slider>
            </el-card>
          </el-col>

          <!-- 右侧产品展示 -->
          <el-col :span="18">
            <el-row :gutter="24" class="product-row" style="height: 1000px; overflow: hidden;">
              <el-col :span="8" :xs="24" :sm="12" :md="8" v-for="product in filteredProducts" :key="product.id" class="product-col">
                <el-card class="product-card">
                  <img :src="product.coverImage" alt="暂无图片" class="product-image"/>
                  <p style="text-align: left;">{{ product.name }}</p>
                  <p style="text-align: left; margin-top: 10px;">$ {{ product.salePrice }}</p>
                </el-card>
              </el-col>
            </el-row>
          </el-col>
        </el-row>
      </el-card>
    </div>
    <Footer class="top" />
  </div>
</template>

<script>
import Banner from './components/Banner'
import Nav from './components/Nav.vue'
import Footer from './components/Footer.vue'

export default {
  name: 'Products',
  components: { Nav, Footer, Banner },

  data() {
    return {
      products: [
        { id: 1, name: 'iPhone 13', model: 'iphone', color: 'Red', salePrice: 799, stock: 50, coverImage: 'https://i.ebayimg.com/images/g/EcIAAOSwropmIhRN/s-l1200.jpg' },
        { id: 2, name: 'iPad Air', model: 'ipad', color: 'Blue', salePrice: 599, stock: 30, coverImage: 'https://i.ebayimg.com/images/g/EcIAAOSwropmIhRN/s-l1200.jpg' },
        { id: 3, name: 'MacBook Pro', model: 'macbook', color: 'Black', salePrice: 1299, stock: 20, coverImage: 'https://i.ebayimg.com/images/g/EcIAAOSwropmIhRN/s-l1200.jpg' },
        { id: 4, name: 'Apple Watch', model: 'watch', color: 'Green', salePrice: 399, stock: 100, coverImage: 'https://i.ebayimg.com/images/g/EcIAAOSwropmIhRN/s-l1200.jpg' },
        { id: 5, name: 'Starlink Kit', model: 'starlink', color: 'White', salePrice: 499, stock: 5, coverImage: 'https://i.ebayimg.com/images/g/EcIAAOSwropmIhRN/s-l1200.jpg' },
        { id: 6, name: 'iPhone 12', model: 'iphone', color: 'Black', salePrice: 699, stock: 50, coverImage: 'https://i.ebayimg.com/images/g/EcIAAOSwropmIhRN/s-l1200.jpg' },
        { id: 7, name: 'MacBook Air', model: 'macbook', color: 'White', salePrice: 999, stock: 10, coverImage: 'https://i.ebayimg.com/images/g/EcIAAOSwropmIhRN/s-l1200.jpg' },
        { id: 8, name: 'iPad Pro', model: 'ipad', color: 'Red', salePrice: 799, stock: 60, coverImage: 'https://i.ebayimg.com/images/g/EcIAAOSwropmIhRN/s-l1200.jpg' },
        { id: 9, name: 'iPad Pro', model: 'ipad', color: 'Red', salePrice: 799, stock: 60, coverImage: 'https://i.ebayimg.com/images/g/EcIAAOSwropmIhRN/s-l1200.jpg' },
        { id: 10, name: 'iPad Pro', model: 'ipad', color: 'Red', salePrice: 799, stock: 60, coverImage: 'https://i.ebayimg.com/images/g/EcIAAOSwropmIhRN/s-l1200.jpg' },
        { id: 11, name: 'iPad Pro', model: 'ipad', color: 'Red', salePrice: 799, stock: 60, coverImage: 'https://i.ebayimg.com/images/g/EcIAAOSwropmIhRN/s-l1200.jpg' },
        // More products for testing
      ],
      testList: ['iphone', 'ipad', 'macbook', 'watch', 'starlink'],
      checked1: [],
      selectedColors: [],
      stockRange: [0, 100],
      colors: ['Red', 'Blue', 'Green', 'Black', 'White'],
      displayedProducts: [], // Initially shown products
      productsPerPage: 6, // Number of products to load at a time
    };
  },

  computed: {
    filteredProducts() {
      return this.products.filter(product => {
        const colorMatches = this.selectedColors.length === 0 || this.selectedColors.includes(product.color);
        const stockMatches = product.stock >= this.stockRange[0] && product.stock <= this.stockRange[1];
        const modelMatches = this.checked1.length === 0 || this.checked1.includes(product.model);
        return colorMatches && stockMatches && modelMatches;
      });
    }
  },

  mounted() {
    // this.loadMoreProducts();
  },

  methods: {
    formatTooltip(value) {
      return `${value}件`;
    },

    loadMoreProducts() {
      console.log("+++++++++++++++++")
      const nextProducts = this.filteredProducts.slice(this.displayedProducts.length, this.displayedProducts.length + this.productsPerPage);
      this.displayedProducts = [...this.displayedProducts, ...nextProducts];
    },

    handleScroll() {
      const productRow = this.$refs.productRow;
      const scrollHeight = productRow.scrollHeight;
      const scrollTop = productRow.scrollTop;
      const clientHeight = productRow.clientHeight;

      // 如果滚动到底部，就加载更多产品
      if (scrollTop + clientHeight >= scrollHeight - 10) {
        this.loadMoreProducts();
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.page {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  padding: 20px;

  @media (max-width: 768px) {
    padding: 10px;
  }

  @media (max-width: 480px) {
    padding: 5px;
  }
}

.container {
  flex: 1;
  margin-bottom: 100px;
}

.product-card {
  width: 100%;
  border: 1px solid #e1e1e1;
  padding: 10px;
  text-align: center;
  margin: 4px;
  transition: transform 0.3s ease-in-out;
}

.product-card:hover {
  transform: scale(1.05);
}

.product-image {
  width: 100%;
  height: auto;
}

/* 控制每行最多顯示3個卡片 */
.product-col {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
  height: 350px;
}

.el-row {
  display: flex;
  flex-wrap: wrap;
}

.product-row {
  max-height: 1000px;
  overflow: hidden;
  position: relative;
}

// .el-row::-webkit-scrollbar {
//   display: none; /* 隐藏滚动条 */
// }
</style>
