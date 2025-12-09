# 🖼️ 新娘照片云存储配置指南

## 步骤1：上传图片到云存储

### 方法A：通过微信开发者工具上传

1. 打开微信开发者工具
2. 点击顶部「云开发」按钮
3. 选择「存储」标签
4. 点击「上传文件」
5. 创建文件夹路径：`wedding/couple/`
6. 选择新娘照片上传
7. 上传成功后，点击文件获取`文件ID`

### 方法B：通过代码上传

在小程序中添加上传功能：

```javascript
// 选择并上传新娘照片
wx.chooseImage({
  count: 1,
  sizeType: ['compressed'],
  sourceType: ['album', 'camera'],
  success: (res) => {
    const tempFilePath = res.tempFilePaths[0]
    
    // 上传到云存储
    wx.cloud.uploadFile({
      cloudPath: 'wedding/couple/wife.jpg', // 云存储路径
      filePath: tempFilePath,
      success: uploadRes => {
        console.log('上传成功，文件ID：', uploadRes.fileID)
        // 文件ID格式：cloud://环境ID/wedding/couple/wife.jpg
      }
    })
  }
})
```

## 步骤2：获取云存储环境ID

1. 在云开发控制台查看环境ID
2. 格式通常为：`wedding-xxx-123456`

## 步骤3：修改配置文件

编辑 `miniprogram/pages/index/index.js`：

```javascript
// 新娘独照 - 使用云存储文件ID
wife: 'cloud://wedding-xxx-123456/wedding/couple/wife.jpg'
```

将 `wedding-xxx-123456` 替换为你的实际环境ID。

## 步骤4：图片处理（可选）

微信云存储支持图片实时处理，在URL后添加参数：

```javascript
// 原始文件ID
const fileID = 'cloud://wedding-xxx-123456/wedding/couple/wife.jpg'

// 裁剪为600×800像素
const processedID = fileID + '?imageMogr2/thumbnail/600x800'

// 压缩质量到80%
const compressedID = fileID + '?imageMogr2/quality/80'
```

## 注意事项

1. **文件命名**：建议使用英文命名，避免特殊字符
2. **图片尺寸**：新娘独照建议尺寸 600×800px
3. **文件大小**：建议压缩到 300KB 以内
4. **环境ID**：确保使用正确的云开发环境ID

## 完整示例

```javascript
imgs: {
  // 其他图片配置...
  
  // 新郎独照（如果也要用云存储）
  husband: 'cloud://wedding-xxx-123456/wedding/couple/husband.jpg',
  
  // 新娘独照 - 使用云存储
  wife: 'cloud://wedding-xxx-123456/wedding/couple/wife.jpg',
  
  // 其他图片配置...
}
```

## 测试验证

1. 编译小程序
2. 检查新娘照片是否正常显示
3. 如果图片不显示，检查：
   - 环境ID是否正确
   - 文件是否已上传
   - 云开发权限设置

## 常见问题

**Q: 图片不显示怎么办？**
- 检查环境ID是否正确
- 确认文件已上传到云存储
- 检查云开发是否已初始化（app.js中的env配置）

**Q: 如何批量替换所有图片？**
- 可以写个脚本批量上传
- 或者手动逐个上传并记录文件ID
- 建议保持统一的命名规则