# 软连接管理工具

一个基于Python的Windows软连接管理工具，用于批量创建、检测和删除软连接。

## 功能特点

- 读取TXT文件中的mklink命令并执行
- 批量创建软连接（目录连接、符号链接等）
- 检测软连接状态并自动修复，支持持续检测模式
- 一键删除指定文档中的所有软连接
- 扫描目录查找现有软连接并导出为命令文件
- 支持多文档选择和批量操作
- 多语言界面（支持中文、英文、日语、韩语和印地语）
- Win11风格的现代界面，带有自定义图标
- 内置文本编辑器，支持文件复制和重命名

## 应用截图

![image](https://github.com/user-attachments/assets/d00dca97-7da5-469a-876d-47b22d70459e)

![image](https://github.com/user-attachments/assets/3aed8e13-ebea-4cc1-8479-1cbee8cc53cf)

![image](https://github.com/user-attachments/assets/b4d692a1-535a-4148-9f4c-af3ed805c1f0)

## 安装使用

### 环境要求

- Windows 10/11 操作系统
- Python 3.8 或更高版本（源码运行时）
- 管理员权限（创建软连接必需）

### 安装依赖

如果从源码运行，需要安装以下依赖：

```
pip install -r requirements.txt
```

主要依赖项：（可以先不安装看下能否运行，不能运行再安装）
- customtkinter==5.2.0
- Pillow==10.0.0
- pywin32==305

### 运行方式

#### 方式一：可执行文件

下载并运行打包好的可执行文件。**必须以管理员身份运行**，否则无法创建软连接。

#### 方式二：从源码运行

以管理员身份运行命令提示符或PowerShell，然后执行：

```
python main.py
```

程序会自动检测权限并尝试提升到管理员权限。

## 使用说明

### 软连接命令文件格式

软连接命令文件应为TXT格式，包含标准的Windows mklink命令，例如：

```
mklink /j "C:\链接路径" "D:\目标路径"
mklink /d "C:\链接路径2" "D:\目标路径2"
mklink "C:\链接文件.txt" "D:\目标文件.txt"
mklink /h "C:\硬链接.txt" "D:\目标文件.txt"
```

支持的命令类型：
- `mklink`：创建文件符号链接（不带参数）
- `mklink /d`：创建目录符号链接（Directory Symbolic Link）
- `mklink /j`：创建目录连接（Junction Point）
- `mklink /h`：创建硬链接（Hard Link）

#### 各类型链接说明：

1. **文件符号链接（File Symbolic Link）**：指向文件的软链接，跨卷支持，可以指向不存在的目标，显示为快捷方式类型
   ```
   mklink "C:\链接文件.txt" "D:\目标文件.txt"
   ```

2. **目录符号链接（Directory Symbolic Link）**：指向目录的软链接，跨卷支持，可以指向不存在的目标，显示为快捷方式类型
   ```
   mklink /d "C:\链接目录" "D:\目标目录"
   ```

3. **目录连接（Junction Point）**：只能用于目录，不能跨卷，不能指向网络位置，对应用程序透明
   ```
   mklink /j "C:\链接目录" "C:\目标目录"
   ```

4. **硬链接（Hard Link）**：只能用于同一卷的文件，不能用于目录，无法跨卷，原始文件删除后链接仍可访问
   ```
   mklink /h "C:\硬链接.txt" "C:\目标文件.txt"
   ```

### 基本操作

1. **选择文件**：点击"选择文件"按钮，选择包含软连接命令的TXT文件。
2. **创建软连接**：点击"创建软连"按钮，程序将解析选中文件中的命令并创建软连接。
3. **检测与修复**：点击"检测修复"按钮，程序将检查已创建的软连接是否存在并正常工作，如有问题会自动修复。开始检测后，按钮会变为"停止检测"，再次点击可以停止持续检测。
4. **扫描链接**：点击"扫描链接"按钮，可以搜索指定目录中现有的软连接，并将结果保存为可导入的命令文件。

### 文件操作

每个文件右侧提供以下操作按钮：
- **编辑**：打开内置编辑器修改文件内容
- **复制**：创建文件的副本
- **移除**：从列表中移除文件（不删除磁盘文件）
- **清除**：删除文件中定义的所有软连接，并从列表和磁盘中删除该文件

### 多语言支持

通过界面右上角的语言选择下拉框，可以切换程序界面语言：
- 中文（默认）
- English（英文）
- 日本語（日语）
- 한국어（韩语）
- हिंदी（印地语）

### 持续检测模式

点击"检测修复"按钮后，程序会进入持续检测模式，定期（每5秒）检查所有软连接的状态，并自动重建失效的链接。检测过程中，进度条会显示当前检测进度。

## 常见问题

### 无法创建链接
- 确保程序以管理员身份运行
- 对于 Windows 10 家庭版，需要开启开发者模式才能创建符号链接

### 路径格式错误
- 确保命令格式正确，路径应使用双引号括起来
- 注意路径中的特殊字符，如空格、中文等

### 目标已存在
- 如果链接目标已存在，程序会尝试先删除现有文件/目录，然后创建链接
- 如果无法删除现有目标，请手动删除后再试

### 权限不足
- 即使以管理员身份运行，某些系统受保护的目录可能仍无法创建链接
- 尝试修改目标目录的权限或选择其他目录

## 项目特色

- **多语言支持**：无需重启应用即可切换界面语言
- **持续检测模式**：定期自动检测并修复软连接
- **内置文本编辑器**：方便直接编辑链接命令文件
- **自动保存设置**：记住上次使用的文件、窗口大小和语言选择

## 开发者信息

- 使用 Python 和 CustomTkinter 开发
- 项目遵循类 MVC 设计模式
- 完整依赖请参见 requirements.txt 文件
- 支持 PyInstaller 打包为单一可执行文件

## 如果觉得有用，可以请我喝杯奶茶啊！！（不请也能给个Starred啦！）

![image](https://github.com/user-attachments/assets/824455e5-923c-47c0-a399-88c2df41d9d2)

