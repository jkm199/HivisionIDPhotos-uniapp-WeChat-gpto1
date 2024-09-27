# 明俞予证件照生成器

项目预览：

<div align="center">
  <img src="https://github.com/user-attachments/assets/0155554c-638a-4f2b-884d-d5a9eb5d9acf" width="300">
  <img src="https://github.com/user-attachments/assets/5f0d5429-5c7e-47bc-8ae4-709ec33723e7" width="300">
  <img src="https://github.com/user-attachments/assets/2e39a8c5-2db1-4326-a388-e91e095e3816" width="300">
  <img src="https://github.com/user-attachments/assets/82060fc4-f334-4c20-bd78-b0e67bd8b4cf" width="300">
</div>

---

## 项目介绍

基于 [Zeyi-Lin](https://github.com/Zeyi-Lin) 大佬的 [HivisionIDPhotos](https://github.com/Zeyi-Lin/HivisionIDPhotos) 项目作为后端 API，使用 uniapp 开发的一个前端调用页面源码。

---

## 运行方法

1. 下载文件压缩包，解压。
2. 将项目拖入 HBuilderX 编辑器中。
3. 替换 `index.vue` 文件中的 API 地址为自己搭建的：[HivisionIDPhotos](https://github.com/Zeyi-Lin/HivisionIDPhotos) 后端 API 地址。
4. 然后进行小程序发行。

<div align="center">
  <img src="https://github.com/user-attachments/assets/df6991f4-e2e1-4642-8b00-b6f97334b316" width="400">
  <img src="https://github.com/user-attachments/assets/677515ba-34ed-4adf-9fc0-677f94458619" width="400">
</div>

---

## 特别说明

- 此项目所有代码均为 GPT-4 和 Claude 结合生成，感谢：[gpt4中文镜像站](https://tz.zhufushipinzhizuo.com/17.html)。
- 本项目需要配合前面提到的后端 API 运行，出于安全考虑，目前代码中的 API 无效，务必替换为自己搭建的 API。
- API 搭建方法请参考 [HivisionIDPhotos](https://github.com/Zeyi-Lin/HivisionIDPhotos)。

---

甚至本描述也是使用gpt帮我排版的：# 对话记录

## 2024/9/27

### 用户 (2024/09/27 13:57:32)

  我想让这几个图片缩放显示：![image](https://github.com/user-attachments/assets/0155554c-638a-4f2b-884d-d5a9eb5d9acf)
  ![1e6e446180c4338457035d3457924b0](https://github.com/user-attachments/assets/5f0d5429-5c7e-47bc-8ae4-709ec33723e7)
  ![be675c760b2f6ac25172f9b9012892d](https://github.com/user-attachments/assets/2e39a8c5-2db1-4326-a388-e91e095e3816)
  ![6bb1cf2524940fa03bef8e83b242906](https://github.com/user-attachments/assets/82060fc4-f334-4c20-bd78-b0e67bd8b4cf)

### AI (2024/09/27 13:57:42)

  看起来你提供的图片链接是指向 GitHub 仓库的，但是这些图片链接无法直接显示。如果你希望缩放这些图片显示，你可以先将图片下载到本地或者将其上传到一个支持显示和缩放的服务。

  另外，你也可以使用HTML或CSS来缩放图片的显示。如果你是在网页中操作，以下是两种简单的方式：

  1. \*\*HTML的方式：\*\*

```html
<img src="你的图片链接" width="300" height="200">
```

  你可以调整 \`width\` 和 \`height\` 来设置图片的缩放比例。

  2. \*\*CSS方式：\*\*

```html
<img src="你的图片链接" style="width: 50%; height: auto;">
```

  这个方式可以通过CSS来控制图片的宽高比例。

  如果你是在某个代码环境中工作或是希望我帮助进行图片处理，你可以上传具体的图片文件，我可以帮你调整这些图片的缩放显示。

### 用户 (2024/09/27 13:58:03)

  这是仓库的README.md

### AI (2024/09/27 13:58:18)

  如果这些图片链接是用于GitHub仓库的\`README.md\`文件中，并且你希望在该文件中缩放这些图片显示，可以通过Markdown和HTML结合的方式实现。

  由于Markdown本身没有直接提供图片缩放的功能，因此可以通过内嵌HTML来实现控制图片尺寸的效果。以下是如何在\`README.md\`中实现图片缩放的几种方式：

  \#\#\# 1. 使用HTML调整图片大小

```markdown
<img src="https://github.com/user-attachments/assets/0155554c-638a-4f2b-884d-d5a9eb5d9acf" width="400">
<img src="https://github.com/user-attachments/assets/5f0d5429-5c7e-47bc-8ae4-709ec33723e7" width="400">
<img src="https://github.com/user-attachments/assets/2e39a8c5-2db1-4326-a388-e91e095e3816" width="400">
<img src="https://github.com/user-attachments/assets/82060fc4-f334-4c20-bd78-b0e67bd8b4cf" width="400">
```

  在上面的例子中，我将每张图片的宽度设为\`400px\`，你可以根据需要调整这个值。\`height\`属性可以根据需要添加，也可以通过设置\`auto\`保持图片的宽高比例。

  \#\#\# 2. Markdown语法和HTML结合

  你也可以继续使用Markdown语法来显示图片，但没有直接的缩放控制，所以推荐使用HTML的方式。如果你想坚持使用Markdown，可能需要手动调整图片的尺寸并重新上传调整后的图片。

```markdown
![image1](https://github.com/user-attachments/assets/0155554c-638a-4f2b-884d-d5a9eb5d9acf)
![image2](https://github.com/user-attachments/assets/5f0d5429-5c7e-47bc-8ae4-709ec33723e7)
![image3](https://github.com/user-attachments/assets/2e39a8c5-2db1-4326-a388-e91e095e3816)
![image4](https://github.com/user-attachments/assets/82060fc4-f334-4c20-bd78-b0e67bd8b4cf)
```

  但如前所述，Markdown自身不支持图片缩放，因此使用HTML方式嵌入图片是更直接有效的。

  如果你想要图片在不同设备上响应式缩放，建议结合使用CSS来进行更加灵活的控制。

### 用户 (2024/09/27 13:59:00)

  帮我优化一下排版：预览：明俞予证件照生成器

  <img src="https://github.com/user-attachments/assets/0155554c-638a-4f2b-884d-d5a9eb5d9acf" width="400">
  <img src="https://github.com/user-attachments/assets/5f0d5429-5c7e-47bc-8ae4-709ec33723e7" width="400">
  <img src="https://github.com/user-attachments/assets/2e39a8c5-2db1-4326-a388-e91e095e3816" width="400">
  <img src="https://github.com/user-attachments/assets/82060fc4-f334-4c20-bd78-b0e67bd8b4cf" width="400">

  
  项目介绍

  基于[Zeyi-Lin](https://github.com/Zeyi-Lin)大佬的[HivisionIDPhotos](https://github.com/Zeyi-Lin/HivisionIDPhotos)项目做为后端api，使用uniapp开发的一个前端调用页面源码。

  
  运行方法

  下载文件压缩包，解压，拖入hbuilderx编辑器，替换index.vue文件里面的api地址为自己搭建的：[HivisionIDPhotos](https://github.com/Zeyi-Lin/HivisionIDPhotos) 后端api地址即可，然后发行小程序。
  ![image](https://github.com/user-attachments/assets/df6991f4-e2e1-4642-8b00-b6f97334b316)
  ![image](https://github.com/user-attachments/assets/677515ba-34ed-4adf-9fc0-677f94458619)

  
  此项目所有代码均为gpto1和克劳德结合生成： [gpt4中文镜像站](https://tz.zhufushipinzhizuo.com/17.html)

  特别注意：本项目需要配合前面提到的后端api运行，出于安全考虑目前代码中的api无效，务必替换自己搭建的api。api搭建方法请参考[HivisionIDPhotos](https://github.com/Zeyi-Lin/HivisionIDPhotos)
  

### AI (2024/09/27 13:59:18)

  你提供的\`README.md\`文件内容较为清晰，但排版可以稍作优化，使得图片展示和文本内容的分割更加美观。以下是优化建议，主要针对排版、格式、及图片展示的布局。

  \#\#\# 优化后的\`README.md\`：

  \`\`\`markdown
  \# 明俞予证件照生成器

  项目预览：

  <div align="center">
  <img src="https://github.com/user-attachments/assets/0155554c-638a-4f2b-884d-d5a9eb5d9acf" width="300">
  <img src="https://github.com/user-attachments/assets/5f0d5429-5c7e-47bc-8ae4-709ec33723e7" width="300">
  <img src="https://github.com/user-attachments/assets/2e39a8c5-2db1-4326-a388-e91e095e3816" width="300">
  <img src="https://github.com/user-attachments/assets/82060fc4-f334-4c20-bd78-b0e67bd8b4cf" width="300">
  </div>

  ---

  \#\# 项目介绍

  基于 [Zeyi-Lin](https://github.com/Zeyi-Lin) 大佬的 [HivisionIDPhotos](https://github.com/Zeyi-Lin/HivisionIDPhotos) 项目作为后端 API，使用 uniapp 开发的一个前端调用页面源码。

  ---

  \#\# 运行方法

  1. 下载文件压缩包，解压。
  2. 将项目拖入 HBuilderX 编辑器中。
  3. 替换 \`index.vue\` 文件中的 API 地址为自己搭建的：[HivisionIDPhotos](https://github.com/Zeyi-Lin/HivisionIDPhotos) 后端 API 地址。
  4. 然后进行小程序发行。

  <div align="center">
  <img src="https://github.com/user-attachments/assets/df6991f4-e2e1-4642-8b00-b6f97334b316" width="400">
  <img src="https://github.com/user-attachments/assets/677515ba-34ed-4adf-9fc0-677f94458619" width="400">
  </div>

  ---

  \#\# 特别说明

  - 此项目所有代码均为 GPT-4 和 Claude 结合生成，感谢：[gpt4中文镜像站](https://tz.zhufushipinzhizuo.com/17.html)。
  - 本项目需要配合前面提到的后端 API 运行，出于安全考虑，目前代码中的 API 无效，务必替换为自己搭建的 API。
  - API 搭建方法请参考 [HivisionIDPhotos](https://github.com/Zeyi-Lin/HivisionIDPhotos)。

  ---

```

  \#\#\# 优化点：

  1. \*\*图片对齐和缩放\*\*：使用\`<div align="center">\`来居中图片，并统一调整图片大小为\`300px\`或\`400px\`，以确保页面视觉一致。
  2. \*\*标题层次分明\*\*：使用更清晰的层次结构，使用二级标题(\`\#\#\`)区分不同部分。
  3. \*\*间距和分隔线\*\*：通过使用\`---\`分隔线，增强视觉分隔，使内容更易于阅读。
  4. \*\*列表格式化\*\*：步骤说明采用有序列表格式，帮助用户清晰理解操作步骤。

  这样调整后的\`README.md\`看起来更加简洁，且易于浏览和理解。


