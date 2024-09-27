<template>


    <!-- 主要内容区 -->
    <scroll-view scroll-y="true" class="main-content">
      <!-- 图片预览区 -->
      <view class="image-preview" :class="{'image-selected': processedImageUrl || printImageUrl}">
        <view v-if="!processedImageUrl && !printImageUrl" class="placeholder">
          <image src="/static/images/icons/LucideCamera.png" class="camera-icon icon-indigo" />
          <text class="placeholder-text">
            请先选择尺寸和背景颜色，然后点击下方选择照片按钮开始
          </text>
        </view>
        <view v-else class="image-container">
          <view v-if="isLoading" class="loading-text">处理中...</view>
          <view v-else class="images-wrapper">
            <view v-if="processedImageUrl" class="image-section">
              <text class="image-label">标准版</text>
              <image
                :src="processedImageUrl"
                class="processed-image"
                mode="aspectFit"
              />
              <button
                class="action-button save-button small-button"
                @tap="savePhoto"
                :disabled="!processedImageUrl"
                :class="{ 'button-disabled': !processedImageUrl }"
              >
                <image src="/static/images/icons/LucideDownload.png" class="button-icon icon-white small-icon" />
                保存标准版
              </button>
            </view>
            <view v-if="printImageUrl" class="image-section">
              <text class="image-label">打印版</text>
              <image
                :src="printImageUrl"
                class="processed-image"
                mode="aspectFit"
              />
              <button
                class="action-button save-print-button small-button"
                @tap="savePrintPhoto"
                :disabled="!printImageUrl"
                :class="{ 'button-disabled': !printImageUrl }"
              >
                <image src="/static/images/icons/LucideDownload.png" class="button-icon icon-white small-icon" />
                保存打印版
              </button>
            </view>
          </view>
        </view>
      </view>

      <!-- 编辑选项区 -->
      <view class="options-container">
        <!-- 尺寸选择 -->
        <view class="option-item">
          <text class="option-label">选择尺寸</text>
          <picker mode="selector" :range="sizeOptions" @change="handleSizeChange">
            <view class="picker">
              <text>{{ selectedSizeName || '请选择尺寸' }}</text>
            </view>
          </picker>
        </view>
        <!-- 尺寸输入框 -->
        <view class="option-item size-inputs">
          <input
            type="text"
            placeholder="宽度（mm）"
            class="size-input"
            v-model="width"
            :disabled="selectedSizeName !== '自定义'"
          />
          <input
            type="text"
            placeholder="高度（mm）"
            class="size-input"
            v-model="height"
            :disabled="selectedSizeName !== '自定义'"
          />
        </view>

        <!-- 背景颜色选择 -->
        <view class="option-item">
          <text class="option-label">背景颜色</text>
          <view class="color-options">
            <view
              v-for="(color, index) in colors"
              :key="index"
              :style="{ backgroundColor: color }"
              class="color-circle"
              :class="{ 'color-selected': selectedColor === color }"
              @click="selectColor(color)"
            ></view>
          </view>
        </view>
        <!-- 提示信息 -->
        <view class="info-box">
          
          <text class="info-text">
            选择合适的尺寸和背景颜色，然后点击"选择照片"上传您的人像照片,勾选原图效果更好。我们将自动处理并生成符合要求的证件照。
          </text>
        </view>
      </view>

      <!-- 主要操作按钮 -->
      <view class="action-buttons">
        <button class="action-button select-button" @tap="handleImageSelect">
          <image src="/static/images/icons/LucideImage.png" class="button-icon icon-white" />
          选择照片
        </button>
        <button
          class="action-button print-button"
          @tap="generatePrintVersion"
          :disabled="!processedImageUrl"
          :class="{ 'button-disabled': !processedImageUrl }"
        >
          <image src="/static/images/icons/LucidePrinter.png" class="button-icon icon-white" />
          生成打印版
        </button>
      </view>
    </scroll-view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      selectedSizeName: '',
      width: '',
      height: '',
      isLoading: false,
      transparentImageBase64: '', // 原始透明人像Base64
      transparentImagePath: '', // 透明人像临时文件路径
      processedImageUrl: '', // 添加背景后的图片Base64
      printImageUrl: '', // 生成的打印版图片Base64
sizeOptions: [
    '一寸 (25x35mm)',
	'自定义',
    '驾驱证 (22x32mm)',
    '二寸 (35x49mm)',
    '小一寸 (22x32mm)',
    '小二寸 (35x45mm)',
    '简历照 (25x35mm)',
    '硕士研究生考试报名 (41x54mm)',
    '英语四六级 (33x48mm)',
    '在职研究生考试 (33x48mm)',
    '教师资格证 (41x54mm)',
    '全国英语等级考试 (33x48mm)',
    '英语AB级 (15x20mm)',
    '2023年英语专八 (25x35mm)',
    '成人本科学位考试 (33x48mm)',
    '全国职称英语等级考试 (36x49mm)',
    '全国翻译专业资格考试 (35x49mm)',
    '全国商务英语翻译考试 (25x35mm)',
    '医护英语考试 (35x49mm)',
    '学位英语报名 (35x53mm)',
    '普通话水平测试 (33x48mm)',
    '韩语能力考试 (30x41mm)',
    '托业考试 (32x41mm)',
    '托福考试 (25x35mm)',
    '日语鉴定考试 (29x40mm)',
    '法语学习考试 (35x49mm)',
    '外语口译证书 (33x48mm)',
    '雅思 (33x48mm)'
],

sizeDimensions: {
    '一寸': { width: '25', height: '35' },
	'自定义': {},
    '驾驱证': { width: '22', height: '32' },
    '二寸': { width: '35', height: '49' },
    '小一寸': { width: '22', height: '32' },
    '小二寸': { width: '35', height: '45' },
    '简历照': { width: '25', height: '35' },
    '硕士研究生考试报名': { width: '41', height: '54' },
    '英语四六级': { width: '33', height: '48' },
    '在职研究生考试': { width: '33', height: '48' },
    '教师资格证': { width: '41', height: '54' },
    '全国英语等级考试': { width: '33', height: '48' },
    '英语AB级': { width: '15', height: '20' },
    '2023年英语专八': { width: '25', height: '35' },
    '成人本科学位考试': { width: '33', height: '48' },
    '全国职称英语等级考试': { width: '36', height: '49' },
    '全国翻译专业资格考试': { width: '35', height: '49' },
    '全国商务英语翻译考试': { width: '25', height: '35' },
    '医护英语考试': { width: '35', height: '49' },
    '学位英语报名': { width: '35', height: '53' },
    '普通话水平测试': { width: '33', height: '48' },
    '韩语能力考试': { width: '30', height: '41' },
    '托业考试': { width: '32', height: '41' },
    '托福考试': { width: '25', height: '35' },
    '日语鉴定考试': { width: '29', height: '40' },
    '法语学习考试': { width: '35', height: '49' },
    '外语口译证书': { width: '33', height: '48' },
    '雅思': { width: '33', height: '48' }
},
      colors: ['#02A7F0', '#D9001B', '#FFFFFF', '#3492C4', '#3D99E2'],
      selectedColor: '#02A7F0',
    };
  },
  computed: {
    canSelectImage() {
      return this.width && this.height;
    },
  },
  methods: {
    // 将毫米转换为像素
    mmToPx(mm, dpi = 300) {
      return Math.round((mm / 25.4) * dpi);
    },

    handleSizeChange(e) {
      const index = e.detail.value;
      const selectedOption = this.sizeOptions[index];
      this.selectedSizeName = selectedOption;
      const sizeName = selectedOption.split(' ')[0];
      if (sizeName === '自定义') {
        this.width = '';
        this.height = '';
      } else {
        const dimensions = this.sizeDimensions[sizeName];
        if (dimensions) {
          this.width = dimensions.width;
          this.height = dimensions.height;
        }
      }
    },

    selectColor(color) {
      this.selectedColor = color;
      if (this.transparentImageBase64) {
        this.addBackgroundToImage();
      }
    },

    // 生成随机字符串用于文件命名
    generateRandomString(length = 8) {
      const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
      let result = '';
      for (let i = 0; i < length; i++) {
        result += chars.charAt(Math.floor(Math.random() * chars.length));
      }
      return result;
    },

    async handleImageSelect() {
      if (!this.canSelectImage) {
        uni.showToast({
          title: '请先选择尺寸',
          icon: 'none',
        });
        return;
      }

      // 验证宽度和高度是否为有效数字
      const widthMm = parseFloat(this.width);
      const heightMm = parseFloat(this.height);
      if (isNaN(widthMm) || isNaN(heightMm) || widthMm <= 0 || heightMm <= 0) {
        uni.showToast({
          title: '请输入有效的宽度和高度',
          icon: 'none',
        });
        return;
      }

      // 转换为像素
      const widthPx = this.mmToPx(widthMm);
      const heightPx = this.mmToPx(heightMm);

      uni.chooseImage({
        count: 1,
		sizeType: ['original'], //可以指定是原图还是压缩图，默认二者都有
        success: async (res) => {
          const tempFilePath = res.tempFilePaths[0];
          console.log('选择的图片路径：', tempFilePath);

          this.isLoading = true;
          console.log('准备调用 IDPhoto API');

          uni.uploadFile({
            url: 'https://tz.zhufushipinzhizuo.com//idphoto',
            filePath: tempFilePath,
            name: 'input_image',
            formData: {
              height: heightPx, // 使用像素尺寸
              width: widthPx,   // 使用像素尺寸
              human_matting_model: 'hivision_modnet',
              face_detect_model: 'mtcnn',
              hd: 'false', // 将布尔值改为字符串 'false'
              // 移除 background_color 参数，因为背景颜色将在后续步骤处理
            },
            success: (uploadFileRes) => {
              try {
                if (uploadFileRes.statusCode === 200) {
                  const data = JSON.parse(uploadFileRes.data);

                  if (data.status === true) {
                    if (data.image_base64_hd) {
                      // 移除可能的前缀
                      const base64Data = data.image_base64_hd.replace(/^data:image\/\w+;base64,/, '');
                      this.transparentImageBase64 = base64Data;
                      console.log('成功获取透明人像 Base64');
                      uni.showToast({
                        title: '照片处理成功',
                        icon: 'success',
                      });
                      // 将 Base64 保存为临时文件
                      this.saveBase64ToFile(this.transparentImageBase64, `transparent_hd_${this.generateRandomString()}.png`);
                    } else if (data.image_base64_standard) {
                      const base64Data = data.image_base64_standard.replace(/^data:image\/\w+;base64,/, '');
                      this.transparentImageBase64 = base64Data;
                      console.log('成功获取标准透明人像 Base64');
                      uni.showToast({
                        title: '照片处理成功（标准质量）',
                        icon: 'success',
                      });
                      // 将 Base64 保存为临时文件
                      this.saveBase64ToFile(this.transparentImageBase64, `transparent_standard_${this.generateRandomString()}.png`);
                    } else {
                      console.error('未获取到处理后的图片');
                      uni.showToast({
                        title: '未获取到处理后的图片',
                        icon: 'none',
                      });
                      this.isLoading = false;
                    }
                  } else {
                    console.error('API 返回错误状态');
                    uni.showToast({
                      title: '图片处理失败',
                      icon: 'none',
                    });
                    this.isLoading = false;
                  }
                } else {
                  console.error('API 响应状态码非 200：', uploadFileRes.statusCode);
                  uni.showToast({
                    title: '图片处理失败',
                    icon: 'none',
                  });
                  this.isLoading = false;
                }
              } catch (error) {
                console.error('处理 API 响应时出错：', error);
                uni.showToast({
                  title: '处理失败',
                  icon: 'none',
                });
                this.isLoading = false;
              }
            },
            fail: (err) => {
              console.error('API 调用失败：', err);
              uni.showToast({
                title: '上传失败',
                icon: 'none',
              });
              this.isLoading = false;
            },
          });
        },
        fail: (err) => {
          console.error('选择图片失败：', err);
          uni.showToast({
            title: '选择图片失败',
            icon: 'none',
          });
        },
      });
    },

    saveBase64ToFile(base64Data, fileName) {
      const filePath = `${uni.env.USER_DATA_PATH}/${fileName}`;
      console.log(`写入文件路径：${filePath}`);
      uni.getFileSystemManager().writeFile({
        filePath: filePath,
        data: base64Data,
        encoding: 'base64',
        success: (writeRes) => {
          console.log(`成功写入文件：${filePath}`);
          this.transparentImagePath = filePath;
          console.log(`透明人像临时文件路径已设置：${this.transparentImagePath}`);
          // 添加背景颜色
          this.addBackgroundToImage();
        },
        fail: (err) => {
          console.error('写入文件失败：', err);
          uni.showToast({
            title: '处理失败',
            icon: 'none',
          });
          this.isLoading = false;
        }
      });
    },

    addBackgroundToImage() {
      if (!this.transparentImagePath) {
        console.error('透明人像文件路径不存在');
        uni.showToast({
          title: '处理失败：无透明人像文件',
          icon: 'none',
        });
        this.isLoading = false;
        return;
      }

      console.log(`透明人像文件路径：${this.transparentImagePath}`);
      console.log(`背景颜色：${this.selectedColor.slice(1)}`);
      console.log('开始调用 AddBackground API');

      uni.uploadFile({
        url: 'https://tz.zhufushipinzhizuo.com/add_background',
        filePath: this.transparentImagePath,
        name: 'input_image', // 确保字段名正确
        formData: {
          color: this.selectedColor.slice(1), // 去掉 '#' 符号
          kb: 200, // 假设这是压缩质量参数
          render: 0, // 假设这是渲染参数
        },
        header: {
          'Content-Type': 'multipart/form-data', // 确保使用 multipart/form-data
        },
        success: (res) => {
          console.log('AddBackground API 响应：', res);
          try {
            if (res.statusCode === 200) {
              const data = JSON.parse(res.data);
              if (data.status === true && data.image_base64) {
                // 确保 Base64 数据格式正确
                const imageBase64 = data.image_base64.replace(/^data:image\/\w+;base64,/, '');
                this.processedImageUrl = `data:image/png;base64,${imageBase64}`;
                console.log('成功设置添加背景后的图片 URL');
                uni.showToast({
                  title: '背景已添加',
                  icon: 'success',
                });
              } else {
                console.error('AddBackground API 返回错误状态', data);
                uni.showToast({
                  title: '背景添加失败',
                  icon: 'none',
                });
              }
            } else {
              console.error('AddBackground API 响应状态码非 200：', res.statusCode);
              uni.showToast({
                title: '背景添加失败',
                icon: 'none',
              });
            }
          } catch (error) {
            console.error('处理 AddBackground API 响应时出错：', error);
            uni.showToast({
              title: '处理失败',
              icon: 'none',
            });
          } finally {
            this.isLoading = false;
          }
        },
        fail: (err) => {
          console.error('调用 AddBackground API 失败：', err);
          uni.showToast({
            title: '背景添加失败',
            icon: 'none',
          });
          this.isLoading = false;
        },
      });
    },

    async savePhoto() {
      if (!this.processedImageUrl) {
        uni.showToast({
          title: '暂无可保存的标准版照片',
          icon: 'none',
        });
        return;
      }

      this.isLoading = true;
      console.log('开始保存标准版照片');

      // 提取 Base64 数据
      const base64Data = this.processedImageUrl.split(',')[1];
      // 生成随机文件名
      const randomFileName = `standard_${Date.now()}_${this.generateRandomString()}.png`;
      // 生成临时文件路径
      const filePath = `${uni.env.USER_DATA_PATH}/${randomFileName}`;

      // 将 Base64 写入文件
      uni.getFileSystemManager().writeFile({
        filePath: filePath,
        data: base64Data,
        encoding: 'base64',
        success: () => {
          console.log(`成功写入文件：${filePath}`);
          // 保存到相册
          uni.saveImageToPhotosAlbum({
            filePath: filePath,
            success: () => {
              uni.showToast({
                title: '标准版照片已保存',
                icon: 'success',
              });
              this.isLoading = false;
            },
            fail: (err) => {
              console.error('保存失败：', err);
              uni.showToast({
                title: '保存失败',
                icon: 'none',
              });
              this.isLoading = false;
            },
          });
        },
        fail: (err) => {
          console.error('写文件失败：', err);
          uni.showToast({
            title: '保存失败',
            icon: 'none',
          });
          this.isLoading = false;
        },
      });
    },

    async generatePrintVersion() {
      if (!this.processedImageUrl) {
        uni.showToast({
          title: '请先处理照片',
          icon: 'none',
        });
        return;
      }

      this.isLoading = true;
      console.log('开始生成打印版');

      // 提取 Base64 数据
      const base64Data = this.processedImageUrl.split(',')[1];
      // 生成临时文件路径
      const printFileName = `print_version_${Date.now()}_${this.generateRandomString()}.png`;
      const printFilePath = `${uni.env.USER_DATA_PATH}/${printFileName}`;

      // 将 Base64 写入文件
      uni.getFileSystemManager().writeFile({
        filePath: printFilePath,
        data: base64Data,
        encoding: 'base64',
        success: () => {
          console.log(`成功写入打印版文件：${printFilePath}`);

          // 获取图片尺寸（像素）
          const widthPx = this.mmToPx(parseFloat(this.width));
          const heightPx = this.mmToPx(parseFloat(this.height));

          // 调用生成打印版 API
          uni.uploadFile({
            url: 'https://tz.zhufushipinzhizuo.com/generate_layout_photos',
            filePath: printFilePath,
            name: 'input_image',
            formData: {
              height: heightPx, // 使用像素尺寸
              width: widthPx,   // 使用像素尺寸
              kb: 200,
            },
            success: (res) => {
              console.log('GeneratePrintVersion API 响应：', res);
              try {
                if (res.statusCode === 200) {
                  const data = JSON.parse(res.data);
                  if (data.status === true && data.image_base64) {
                    const printBase64 = data.image_base64.replace(/^data:image\/\w+;base64,/, '');
                    this.printImageUrl = `data:image/png;base64,${printBase64}`;
                    console.log('成功生成打印版图片');
                    uni.showToast({
                      title: '打印版生成成功',
                      icon: 'success',
                    });
                  } else {
                    console.error('GeneratePrintVersion API 返回错误状态', data);
                    uni.showToast({
                      title: '打印版生成失败',
                      icon: 'none',
                    });
                  }
                } else {
                  console.error('GeneratePrintVersion API 响应状态码非 200：', res.statusCode);
                  uni.showToast({
                    title: '打印版生成失败',
                    icon: 'none',
                  });
                }
              } catch (error) {
                console.error('处理 GeneratePrintVersion API 响应时出错：', error);
                uni.showToast({
                  title: '处理失败',
                  icon: 'none',
                });
              } finally {
                this.isLoading = false;
              }
            },
            fail: (err) => {
              console.error('调用 GeneratePrintVersion API 失败：', err);
              uni.showToast({
                title: '生成失败',
                icon: 'none',
              });
              this.isLoading = false;
            },
          });
        },
        fail: (err) => {
          console.error('写入打印版文件失败：', err);
          uni.showToast({
            title: '生成失败',
            icon: 'none',
          });
          this.isLoading = false;
        },
      });
    },

    async savePrintPhoto() {
      if (!this.printImageUrl) {
        uni.showToast({
          title: '暂无可保存的打印版照片',
          icon: 'none',
        });
        return;
      }

      this.isLoading = true;
      console.log('开始保存打印版照片');

      // 提取 Base64 数据
      const base64Data = this.printImageUrl.split(',')[1];
      // 生成随机文件名
      const randomFileName = `print_${Date.now()}_${this.generateRandomString()}.png`;
      // 生成临时文件路径
      const filePath = `${uni.env.USER_DATA_PATH}/${randomFileName}`;

      // 将 Base64 写入文件
      uni.getFileSystemManager().writeFile({
        filePath: filePath,
        data: base64Data,
        encoding: 'base64',
        success: () => {
          console.log(`成功写入文件：${filePath}`);
          // 保存到相册
          uni.saveImageToPhotosAlbum({
            filePath: filePath,
            success: () => {
              uni.showToast({
                title: '打印版照片已保存',
                icon: 'success',
              });
              this.isLoading = false;
            },
            fail: (err) => {
              console.error('保存打印版失败：', err);
              uni.showToast({
                title: '保存失败',
                icon: 'none',
              });
              this.isLoading = false;
            },
          });
        },
        fail: (err) => {
          console.error('写文件失败：', err);
          uni.showToast({
            title: '保存失败',
            icon: 'none',
          });
          this.isLoading = false;
        },
      });
    },
  },
};
</script>

<style>
/* 容器样式 */
.container {
  display: flex;
  flex-direction: column;
  height: 100%;
  background: linear-gradient(to bottom, #ebf8ff, #e0e7ff);
}

/* 顶部导航栏样式 */
.header {
  background-color: rgba(255, 255, 255, 0.8);
  color: #4c51bf;
  padding: 10rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 2rpx 4rpx rgba(0, 0, 0, 0.1);
}
.header-title {
  font-size: 32rpx;
  font-weight: bold;
}

/* 主要内容区样式 */
.main-content {
  flex: 1;
  padding: 30rpx;
}

/* 图片预览区样式 */
.image-preview {
  background-color: #fff;
  border-radius: 30rpx;
  box-shadow: 0 4rpx 6rpx rgba(0, 0, 0, 0.1);
  overflow: hidden;
}
.placeholder {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 500rpx;
  background: linear-gradient(to bottom right, #ebf8ff, #d6bcfa);
}
.camera-icon {
  width: 128rpx;
  height: 128rpx;
  margin-bottom: 20rpx;
}
.placeholder-text {
  color: #5a67d8;
  font-size: 28rpx;
  text-align: center;
}
.image-container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 500rpx;
  background-color: #fff;
}
.loading-text {
  color: #5a67d8;
  font-size: 28rpx;
}
.images-wrapper {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-around;
  width: 100%;
}
.image-section {
  width: 45%;
  margin-bottom: 20rpx;
  text-align: center;
}
.image-label {
  font-size: 26rpx;
  color: #4c51bf;
  margin-bottom: 10rpx;
}
.processed-image {
  width: 100%;
  height: 300rpx;
  object-fit: contain;
  border-radius: 20rpx;
  box-shadow: 0 2rpx 4rpx rgba(0, 0, 0, 0.1);
}

/* 编辑选项区样式 */
.options-container {
  background-color: rgba(255, 255, 255, 0.8);
  border-radius: 30rpx;
  box-shadow: 0 4rpx 6rpx rgba(0, 0, 0, 0.1);
  padding: 30rpx;
  margin-top: 30rpx;
}
.option-item {
  margin-bottom: 20rpx;
}
.option-label {
  font-size: 28rpx;
  color: #4c51bf;
  margin-bottom: 10rpx;
}

.picker {
  height: 60rpx;
  line-height: 70rpx; /* 垂直居中文本 */
  padding: 0 20rpx; /* 保持左右padding，去掉上下padding */
  border: 1rpx solid #c3dafe;
  border-radius: 20rpx;
  background-color: #fff;
  color: #4c51bf;
}

/* 尺寸输入框样式 */
.size-input {
  width: 45%;
  height: 80rpx; /* 设置固定高度 */
  line-height: 60rpx; /* 垂直居中文本 */
  padding: 0 20rpx; /* 保持左右padding，去掉上下padding */
  border: 1rpx solid #c3dafe;
  border-radius: 20rpx;
  background-color: #fff;
  color: #4c51bf;
}
.size-inputs {
  flex-direction: row;
  display: flex;
  justify-content: space-between;
}

/* 背景颜色选择样式 */
.color-options {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.color-circle {
  width: 60rpx;
  height: 60rpx;
  border-radius: 50%;
  box-shadow: 0 2rpx 4rpx rgba(0, 0, 0, 0.1);
  margin-right: 20rpx;
  transition: transform 0.2s;
  position: relative;
}
.color-circle:last-child {
  margin-right: 0;
}
.color-selected::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  border: 4rpx solid #4c51bf;
  border-radius: 50%;
  box-sizing: border-box;
}

/* 提示信息样式 */
.info-box {
  background-color: #ebf4ff;
  padding: 20rpx;
  border-radius: 20rpx;
  display: flex;
  flex-direction: row;
  align-items: flex-start;
}
.info-icon {
  width: 22rpx;
  height: 22rpx;
  margin-right: 5rpx;
}
.info-text {
  color: #5a67d8;
  font-size: 24rpx;
}

/* 主要操作按钮样式 */
.action-buttons {
  margin-top: 30rpx;
}
.action-button {
  width: 100%;
  padding: 20rpx;
  border-radius: 20rpx;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20rpx;
  color: #fff;
  font-size: 28rpx;
  box-shadow: 0 4rpx 6rpx rgba(0, 0, 0, 0.1);
}
.button-disabled {
  background-color: #a0aec0;
  color: #cbd5e0;
}
.select-button {
  background: linear-gradient(to right, #667eea, #764ba2);
}
.print-button {
  background: linear-gradient(to right, #ed64a6, #f6ad55);
}
.save-button {
  background: linear-gradient(to right, #667eea, #764ba2);
}
.save-print-button {
  background: linear-gradient(to right, #ed64a6, #f6ad55);
}
.small-button {
  padding: 10rpx 15rpx; /* 缩小按钮的 padding */
  font-size: 24rpx; /* 缩小按钮的字体大小 */
  margin-top: 30rpx; /* 增加按钮与图片之间的间距 */
}
.button-icon {
  width: 30rpx; /* 缩小图标大小 */
  height: 30rpx;
  margin-right: 5rpx; /* 缩小图标与文字之间的间距 */
}
.small-icon {
  width: 25rpx; /* 进一步缩小图标大小 */
  height: 25rpx;
}

/* 图标颜色调整 */
.icon-white {
  filter: brightness(0) invert(1);
}
.icon-indigo {
  filter: invert(31%) sepia(94%) saturate(3489%) hue-rotate(226deg) brightness(96%) contrast(100%);
}
</style>
