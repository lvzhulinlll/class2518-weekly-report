# GitHub Pages 部署详细操作指南

## 准备工作

### 1. 注册GitHub账号（如果还没有）
- 访问 [GitHub官网](https://github.com)
- 点击右上角"Sign up"按钮
- 填写用户名、邮箱、密码
- 验证邮箱完成注册

### 2. 安装Git工具（Windows系统）
1. 下载Git：https://git-scm.com/downloads
2. 双击安装包，一路点击"Next"安装
3. 安装完成后，在开始菜单中找到"Git Bash"

## 详细部署步骤

### 步骤1：创建GitHub仓库

1. **登录GitHub**
   - 访问 https://github.com
   - 点击右上角"Sign in"登录

2. **创建新仓库**
   - 点击右上角"+"图标
   - 选择"New repository"
   - 填写仓库信息：
     - **Owner**: 您的用户名（自动填充）
     - **Repository name**: `class2518-weekly-report`
     - **Description**: `2518班级学术周报系统`
     - **Public/Private**: 选择 `Public`（免费）
     - **Initialize this repository with**: 勾选 `Add a README file`
   - 点击"Create repository"

### 步骤2：使用Git命令上传文件

1. **打开Git Bash**
   - 在开始菜单中搜索"Git Bash"并打开

2. **进入项目目录**
   ```bash
   cd /c/Users/lvzhu/Desktop/班级周报
   ```

3. **初始化Git仓库**
   ```bash
   git init
   ```

4. **添加远程仓库地址**
   ```bash
   git remote add origin https://github.com/您的用户名/class2518-weekly-report.git
   ```
   > 注意：将"您的用户名"替换为实际的GitHub用户名

5. **拉取远程仓库内容**
   ```bash
   git pull origin main
   ```

6. **添加所有文件到暂存区**
   ```bash
   git add .
   ```

7. **提交文件**
   ```bash
   git commit -m "初始提交：班级周报系统"
   ```

8. **推送到GitHub**
   ```bash
   git push -u origin main
   ```

**完整命令序列**：
```bash
cd /c/Users/lvzhu/Desktop/班级周报
git init
git remote add origin https://github.com/您的用户名/class2518-weekly-report.git
git pull origin main
git add .
git commit -m "初始提交：班级周报系统"
git push -u origin main
```

### 步骤3：开启GitHub Pages功能

1. **进入仓库页面**
   - 在GitHub上找到您的仓库 `class2518-weekly-report`

2. **进入设置页面**
   - 点击仓库页面的"Settings"选项卡
   - 在左侧菜单中找到"Pages"

3. **配置Pages设置**
   - **Source**: 选择 `Deploy from a branch`
   - **Branch**: 选择 `main` 分支
   - **Folder**: 选择 `/ (root)`
   - 点击"Save"按钮

4. **等待部署完成**
   - 等待1-2分钟，页面会显示绿色的部署成功提示
   - 系统会生成访问地址：`https://您的用户名.github.io/class2518-weekly-report`

### 步骤4：测试访问

**教师编辑器地址**：
```
https://您的用户名.github.io/class2518-weekly-report/teacher-editor.html
```

**学生查看地址**：
```
https://您的用户名.github.io/class2518-weekly-report/student-view.html?gistId=您的GistID
```

## 后续维护

### 更新网站内容

当您修改了本地文件后，需要重新上传：

```bash
cd /c/Users/lvzhu/Desktop/班级周报
git add .
git commit -m "更新内容：描述修改内容"
git push origin main
```

### 查看部署状态

- 在仓库页面点击"Actions"选项卡
- 可以查看部署历史记录和状态

## 常见问题解决

### 1. 无法推送文件到GitHub
- **原因**：可能需要设置Git全局配置
- **解决方法**：
  ```bash
  git config --global user.name "您的GitHub用户名"
  git config --global user.email "您的GitHub邮箱"
  ```

### 2. 网站无法访问
- **原因**：部署可能失败
- **解决方法**：检查仓库的"Actions"选项卡查看错误信息

### 3. 文件上传失败
- **原因**：可能有冲突文件
- **解决方法**：
  ```bash
  git pull origin main --allow-unrelated-histories
  git push origin main
  ```

## 部署成功后的效果

- **公网访问**：任何有网络的人都可以访问
- **自动更新**：修改文件后重新推送即可更新网站
- **免费使用**：GitHub Pages完全免费
- **HTTPS安全**：自动提供HTTPS加密访问

## 快速验证部署是否成功

1. 访问您的网站地址
2. 打开浏览器开发者工具（F12）
3. 查看控制台是否有错误
4. 测试所有功能是否正常

---

**部署完成时间**：约10-15分钟
**维护成本**：几乎为零
**适用范围**：全球可访问

如果您在部署过程中遇到任何问题，可以重新查看本指南或搜索相关错误信息。