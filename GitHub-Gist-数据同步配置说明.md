# GitHub Gist 数据同步配置说明

## 概述

本系统使用 GitHub Gist 作为远程数据存储，实现了教师编辑器和学生查看器之间的实时数据同步。教师编辑内容后，学生可以立即看到更新，无需刷新页面。

## 功能特点

- ✅ **实时同步**: 教师编辑后学生立即可见
- ✅ **自动同步**: 学生端每30秒自动检查更新
- ✅ **手动刷新**: 学生可以手动触发数据同步
- ✅ **数据安全**: 支持私有 Gist，数据安全可控
- ✅ **断线恢复**: 网络异常时使用本地数据备份
- ✅ **兼容性强**: 无需服务器，完全基于 GitHub API

## 配置步骤

### 步骤1：获取 GitHub Token

1. 访问 [GitHub Token 生成页面](https://github.com/settings/tokens)
2. 点击 "Generate new token"（Classic）
3. 设置以下权限：
   - ✅ `gist` (创建和管理 Gist)
4. 生成并复制 Token（妥善保存，只显示一次）

### 步骤2：配置教师编辑器

1. 打开 `teacher-editor.html`
2. 在页面底部的 "GitHub Gist配置" 区域：
   - 粘贴您的 GitHub Token
   - 点击 "创建新Gist" 按钮
3. 系统会自动创建私有 Gist 并显示 Gist ID
4. 点击 "保存配置" 完成设置

### 步骤3：生成学生分享链接

1. 在教师编辑器中，Gist 配置完成后
2. 点击 "更新链接" 按钮生成分享链接
3. 系统会自动生成包含 Gist ID 的学生分享链接
4. 格式示例：
   ```
   file:///C:/Users/lvzhu/Desktop/班级周报/student-view.html?gistId=xxxxxxxxxxxxxxxxxxxx
   ```
   
   **实际示例**：
   - 如果您的Gist ID是 `1234567890abcdef`
   - 分享链接将是：`student-view.html?gistId=1234567890abcdef`
   - 学生只需点击此链接即可查看最新内容

### 步骤4：分享给学生

1. 复制生成的分享链接
2. 通过微信、QQ、邮件等方式分享给学生
3. 学生点击链接即可查看最新内容

## 技术原理

### 数据流向

```
教师编辑 → 保存到本地存储 → 同步到 GitHub Gist
                                ↓
学生查看 ← 从 Gist 获取数据 ← 定时检查更新
```

### 同步机制

1. **时间戳比对**: 比较本地和远程数据的最后更新时间
2. **增量更新**: 仅同步更新的数据，减少带宽消耗
3. **自动重试**: 网络异常时自动重试，确保数据一致性
4. **本地缓存**: 确保在网络异常时仍能正常显示

## 故障排除

### 常见问题

**问题1：Gist 创建失败**
- ✅ 检查 GitHub Token 是否正确
- ✅ 确认 Token 有 gist 权限
- ✅ 检查网络连接

**问题2：学生看不到更新**
- ✅ 确认 Gist ID 正确
- ✅ 检查学生链接是否包含正确的 Gist ID
- ✅ 等待自动同步（最长30秒）
- ✅ 学生手动点击 "刷新" 按钮

**问题3：数据同步异常**
- ✅ 检查 GitHub API 限制（每小时5000次请求）
- ✅ 确认 Gist 文件存在且格式正确
- ✅ 查看浏览器控制台错误信息

### 性能优化

- **降低同步频率**: 学生端默认30秒同步一次
- **数据压缩**: JSON 数据自动压缩传输
- **缓存策略**: 合理使用浏览器缓存
- **错误降级**: 网络异常时使用本地数据

## 高级配置

### 自定义同步频率

修改 `student-view.html` 中的同步间隔：

```javascript
// 修改自动同步间隔（单位：毫秒）
setInterval(async () => {
    await loadDataFromGist();
}, 15000); // 改为15秒
```

### 多班级管理

每个班级使用不同的 Gist ID：

1. 为每个班级创建独立的 Gist
2. 生成对应的学生分享链接
3. 教师可以同时管理多个班级数据

### 数据备份与恢复

**手动备份**:
```javascript
// 在浏览器控制台执行
const data = localStorage.getItem('class2518WeeklyData');
console.log('备份数据:', data);
```

**手动恢复**:
```javascript
// 在浏览器控制台执行
localStorage.setItem('class2518WeeklyData', '粘贴备份数据');
```

## 安全注意事项

1. **GitHub Token 安全**
   - 不要将 Token 分享给他人
   - 定期更换 Token
   - 使用私有 Gist 保护数据

2. **数据隐私**
   - 敏感信息不要直接存储在内容中
   - 定期清理过期数据
   - 遵守相关隐私法规

3. **访问控制**
   - 通过不同的 Gist ID 控制访问权限
   - 定期检查 Gist 访问权限

## 公网部署指南

为了让分享链接在公共网络上可访问，需要将系统部署到服务器上。

### 部署方案推荐

#### 方案1：使用GitHub Pages（推荐）

**详细操作步骤：**

**步骤1：创建GitHub账号（如果还没有）**
1. 访问 [GitHub官网](https://github.com)
2. 点击右上角"Sign up"注册账号
3. 验证邮箱完成注册

**步骤2：创建新仓库**
1. 登录GitHub后，点击右上角"+"图标
2. 选择"New repository"
3. 填写仓库信息：
   - **Repository name**: `class2518-weekly-report`（建议使用英文）
   - **Description**: `2518班级学术周报系统`
   - **Public/Private**: 选择`Public`（免费）
   - **Initialize this repository with**: 勾选`Add a README file`
4. 点击"Create repository"

**步骤3：上传项目文件**

**方法A：使用Git命令（推荐）**
```bash
# 1. 安装Git：https://git-scm.com/downloads
# 2. 打开命令行，进入项目目录
cd "c:\Users\lvzhu\Desktop\班级周报"

# 3. 初始化Git仓库
git init

# 4. 添加远程仓库地址（替换为您的用户名）
git remote add origin https://github.com/您的用户名/class2518-weekly-report.git

# 5. 添加所有文件
git add .

# 6. 提交文件
git commit -m "初始提交：班级周报系统"

# 7. 推送到GitHub
git push -u origin main
```

**方法B：使用GitHub网页上传**
1. 进入仓库页面，点击"Add file" → "Upload files"
2. 将项目文件夹中的所有文件拖拽到上传区域
3. 点击"Commit changes"

**步骤4：开启GitHub Pages**
1. 进入仓库页面，点击"Settings"选项卡
2. 左侧菜单选择"Pages"
3. 在"Source"部分选择：
   - **Branch**: `main`
   - **Folder**: `/ (root)`
4. 点击"Save"
5. 等待1-2分钟，页面会显示部署成功的链接

**步骤5：获取访问地址**
- 您的网站地址：`https://您的用户名.github.io/class2518-weekly-report`
- 教师编辑器地址：在上述地址后添加 `/teacher-editor.html`
- 学生查看地址：在上述地址后添加 `/student-view.html?gistId=您的GistID`

**Netlify部署**
1. 注册Netlify账号
2. 拖拽项目文件夹上传
3. 自动生成访问链接

#### 方案2：自建服务器部署

**使用Nginx**
```bash
# 安装Nginx
sudo apt update
sudo apt install nginx

# 配置站点
sudo nano /etc/nginx/sites-available/class-weekly
```

```nginx
server {
    listen 80;
    server_name your-domain.com;
    
    root /var/www/class-weekly;
    index index.html student-view.html teacher-editor.html;
    
    location / {
        try_files $uri $uri/ =404;
    }
}
```

### 部署后的链接格式

部署到服务器后，分享链接将变为：
```
https://your-domain.com/student-view.html?gistId=xxxxxxxxxxxxxxxxxxxx
```

### 部署注意事项

1. **跨域问题**：确保服务器支持CORS
2. **HTTPS**：建议使用HTTPS确保数据安全
3. **缓存策略**：设置合理的缓存头避免数据过期
4. **文件权限**：确保静态文件权限正确

## 快速测试部署

### 本地服务器测试
```bash
# 使用Python简单HTTP服务器
cd "c:\Users\lvzhu\Desktop\班级周报"
python -m http.server 8080

# 访问地址：http://localhost:8080/teacher-editor.html
```

### 局域网访问测试
```bash
# 获取本机IP地址
ipconfig

# 其他人访问：http://192.168.1.100:8080/student-view.html?gistId=xxx
```

## 技术支持

如有技术问题，请检查：

1. 浏览器开发者工具控制台错误信息
2. 网络请求状态码
3. GitHub API 文档

---

**最后更新**: 2024年11月
**版本**: 1.1
**新增功能**: 公网访问支持、二维码分享、移动端优化
**技术支持**: 系统开发团队