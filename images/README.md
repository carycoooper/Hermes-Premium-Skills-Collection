# 图片资源说明

## 📸 请将收款码图片放入此目录

### 需要的文件：

1. **wechat-pay.png** - 微信收款码（你截图的那个，显示"cc(*超)"）
2. **alipay.png** - 支付宝收款码（支付宝 logo + 二维码）

### 操作步骤：

1. 打开微信 → 收款 → 保存收款码图片
2. 重命名为 `wechat-pay.png`，放到本目录（images/）
3. 打开支付宝 → 收款 → 保存收款码图片
4. 重命名为 `alipay.png`，放到本目录（images/）
5. 完成！

### 图片要求：

- 格式: PNG 或 JPG
- 尺寸: 建议 300x300 px 以上（清晰度好）
- 内容: 必须是有效的微信/支付宝收款码

### 放置完成后：

运行以下命令提交并推送到 GitHub：
```bash
git add images/
git commit -m "📸 Add payment QR codes for WeChat and Alipay"
git push
```

---

**注意**: 图片不会上传到 Git（太大），但 README 会引用它们。
如果要在 GitHub 上显示，需要：
- 将图片上传到 GitHub Releases
- 或使用图床服务（如 GitHub raw、imgur 等）

当前配置：README 中已预留图片位置，用户看到时会知道如何打赏。
