# Bilibili 动态批量删除工具

## 脚本简介

**Bilibili 动态批量删除工具** 是一个用于清理 Bilibili 动态的用户脚本。该脚本允许用户按类型批量删除动态，例如：
- 转发动态
- 文字动态
- 图片动态
- 视频动态

此外，本脚本支持导出删除日志及动态内容

## 功能特点

1. **动态类型筛选**：可以选择删除特定类型的动态（如文字、图片、视频等）
2. **操作日志导出**：删除完成后支持导出 JSON 格式的删除日志，记录删除详情
3. **删除进度显示**：实时更新删除进度，包括已删除数量和总数量
4. **自定义删除间隔**：可以设置删除动态的时间间隔，避免触发频率限制

## 安装与使用

### 1. 环境需求

- 浏览器需要安装 [Tampermonkey](https://www.tampermonkey.net/) 或其他用户脚本管理扩展
- 网络环境支持访问Bilibili即可

### 2. 脚本安装

点击以下链接直接安装脚本：
[B站动态批量删除工具](https://greasyfork.org/zh-CN/scripts/521110-b%E7%AB%99%E5%8A%A8%E6%80%81%E6%89%B9%E9%87%8F%E5%88%A0%E9%99%A4%E5%B7%A5%E5%85%B7)

### 3. 使用步骤

1. **访问个人动态页**  
   打开 Bilibili 个人空间的动态页面
   
2. **选择删除类型**  
   根据需要点击脚本面板上的按钮：
   - `删除转发动态`
   - `删除文字动态`
   - `删除图片动态`
   - `删除视频动态`

3. **导出日志**  
   删除完成后，可点击 `导出清除日志及内容` 按钮保存日志文件

## 主要功能代码

#### 1. 动态获取
通过 API 获取用户动态信息：
```javascript
async spaceHistory(offset = 0) {
    return this._api(
        `https://api.vc.bilibili.com/dynamic_svr/v1/dynamic_svr/space_history?visitor_uid=${uid}&host_uid=${uid}&offset_dynamic_id=${offset}`,
        {}, "get"
    );
}
```

#### 2. 动态删除
通过 API 删除指定动态：
```javascript
Copy code
async removeDynamic(id) {
    return this._api(
        "https://api.vc.bilibili.com/dynamic_svr/v1/dynamic_svr/rm_dynamic",
        { dynamic_id: id, csrf_token: csrfToken }
    );
}
```
#### 3. 删除日志导出
将删除记录保存为 JSON 文件：
```javascript
Copy code
function exportLogs() {
    const blob = new Blob([JSON.stringify(logs, null, 2)], { type: "application/json" });
    const link = document.createElement("a");
    link.href = URL.createObjectURL(blob);
    link.download = `deletion_logs_${Date.now()}.json`;
    link.click();
}
```
## 注意事项

#### 使用脚本时建议设置合理的删除间隔（如默认的 50 毫秒），避免触发 Bilibili 的频率限制机制
