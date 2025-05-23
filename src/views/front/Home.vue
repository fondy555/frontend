<template>
  <div>
    <Nav />
    <Banner />
    <div class="container">
      <el-breadcrumb separator="/" class="top">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>当前</el-breadcrumb-item>
      </el-breadcrumb>
      <div class="introduce">
        <h1 class="center">{{ homeTitle }}</h1>
        <p class="center">{{ homeDescription }}</p>
      </div>
      <!-- 新增自动滚动模块 -->
      <el-card class="top">
        <div class="scrolling-container">
          <div class="scrolling-content" v-for="certificate in certificates" :key="certificate.id">
            <img :src="certificate.imageUrl" :alt="certificate.name" class="certificate-image" />
            <h3>{{ certificate.name }}</h3>
            <p>{{ certificate.description }}</p>
          </div>
        </div>
      </el-card>
      <!-- 新增的商品卡片轮播图组件 -->
      <!-- <ProductCarousel /> -->
    </div>
    <Footer class="top" />
  </div>
</template>

<script>
import Banner from './components/Banner'
import ProductCarousel from './components/ProductCarousel' // 引入新组件
import { getAllCertificates, getFooterData } from '@/api/index' // 引入API方法
export default {
  name: 'Home',
  components: {
    Banner, // 轮播图
    ProductCarousel // 横向商品
  },
  props: {},
  watch: {},
  data() {
    return {
      homeTitle: '',
      homeDescription: '',
      hotlineVisible: false,// 服务支持弹窗
      certificates: [], // 存储证书数据
      qrCodeVisible: false,
      footerData: null
    };
  },
  mounted() {
    this.fetchHomeData();
    this.fetchCertificates();
    this.fetchFooterData();
  },
  created() { },
  methods: {
    navigateTo(url) {
      // window.location.href = url;
      this.$router.push(url);
    },
    showHotline() {
      this.hotlineVisible = true;
    },
    copyToClipboard(text) {
      const input = document.createElement('input');
      input.value = text;
      document.body.appendChild(input);
      input.select();
      document.execCommand('copy');
      document.body.removeChild(input);
      this.$message.success('复制成功');
    },
    async fetchCertificates() {
      try {
        const response = await getAllCertificates();
        this.certificates = response.data;
      } catch (error) {
        console.error('Failed to fetch certificates:', error);
      }
    },
    async fetchHomeData() {
      try {
        const response = await getFooterData();
        // console.log("response", response);
        if (response && response.data) {
          this.homeTitle = response.data[0].homeTitle || 'XXXXXXXXXXXXXXXX';
          this.homeDescription = response.data[0].homeDescription || '一个XXXXXXXXXX、经典、文化的深度用户';
        }
      } catch (error) {
        console.error('获取首页数据失败:', error);
      }
    },
    showQRCode() {
      this.qrCodeVisible = true;
    },
    async fetchFooterData() {
      try {
        const response = await getFooterData();
        if (response && response.data && response.data.length > 0) {
          this.footerData = response.data[0];
        }
      } catch (error) {
        console.error('获取页脚数据失败:', error);
      }
    }
  },
};
</script>

<style lang="scss" scoped>
.hotline-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.hotline-text {
  font-size: 18px;
  /* Increase font size */
  user-select: text;
  /* Make text selectable */
}

.page {
  display: flex;
  flex-direction: column;
  min-height: 100vh; // Ensure the page takes at least the full height of the viewport
  padding: 20px;
  margin-top: 75px; // 添加这一行

  @media (max-width: 768px) {
    padding: 10px;
    margin-top: 60px; // 在移动设备上调整这个值
  }

  @media (max-width: 480px) {
    padding: 5px;
  }
}

.container {
  flex: 1; // Allow the container to grow and fill available space
  margin-bottom: 100px; // Add space between container and footer
}

.introduce {
  margin: 20px 0;
  text-align: center;
}

.center {
  text-align: center;
}

.about-us {
  margin: 40px 0;
  text-align: center;

  h2 {
    font-size: 24px;
    margin-bottom: 20px;
    position: relative;
  }

  h2::after {
    content: '';
    display: block;
    width: 50px;
    height: 2px;
    background: red;
    margin: 10px auto 0;
  }

  p {
    font-size: 16px;
    color: #666;
    margin-bottom: 40px;
  }
}

.features {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;

  .feature {
    width: 200px;
    text-align: center;
    margin-bottom: 20px;

    .feature-icon {
      width: 50px;
      height: 50px;
      margin-bottom: 10px;
    }

    h3 {
      font-size: 18px;
      margin-bottom: 10px;
    }

    p {
      font-size: 14px;
      color: #666;
    }
  }
}

/* 新增样式 */
.info-services {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
  padding: 20px 0;

  .service {
    width: 200px;
    text-align: center;
    cursor: pointer;
    transition: transform 0.3s;

    &:hover {
      transform: scale(1.05);
    }

    .service-icon {
      width: 50px;
      height: 50px;
      margin-bottom: 10px;
    }

    h3 {
      font-size: 18px;
      margin-bottom: 10px;
    }

    p {
      font-size: 14px;
      color: #666;
    }
  }
}

.full-image {
  width: 100%;
  height: 100%;
  object-fit: cover; // 保持图片的宽高比并裁剪以填充容器
}

.scrolling-container {
  overflow: hidden;
  white-space: nowrap;
  width: 100%;
  position: relative;
}

.scrolling-content {
  display: inline-block;
  animation: scroll 20s linear infinite;
  text-align: center;
  margin: 0 10px;
}

.certificate-image {
  width: 100px;
  height: 100px;
  object-fit: cover;
  display: block;
  margin: 0 auto;
}

@media (max-width: 768px) {
  .scrolling-content {
    animation: scroll 10s linear infinite;
  }
}

@keyframes scroll {
  0% {
    transform: translateX(100%);
  }

  100% {
    transform: translateX(-100%);
  }
}

.qr-code-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  
  p {
    margin-top: 10px;
    font-size: 14px;
    color: #666;
  }
}
</style>