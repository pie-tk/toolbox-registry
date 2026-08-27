# toolbox-registry

ToolBox 的工具分发仓库。`registry.json` 是工具目录，`plugins/` 存放工具包
（zip：manifest.json + module.js + style.css），客户端下载后经 SHA-256 校验安装。

## 客户端源地址

| 源 | 地址 | 说明 |
| --- | --- | --- |
| GitHub Pages（默认） | `https://pie-tk.github.io/toolbox-registry/registry.json` | 推送后约 1 分钟自动生效，更新及时 |
| jsDelivr CDN（备用） | `https://cdn.jsdelivr.net/gh/pie-tk/toolbox-registry@main/registry.json` | 国内可达性好，但 `@main` 分支缓存更新慢（可能滞后数小时） |
| GitHub Raw | `https://raw.githubusercontent.com/pie-tk/toolbox-registry/main/registry.json` | 实时，无缓存；部分网络不可达 |

在 ToolBox 的「设置 → 工具源」中填入以上任一地址即可。

## 发布 / 更新工具

```bash
# 在 toolbox 项目中
npm run build:plugins          # 产出 public/registry.json + public/plugins/*.zip
# 拷贝到本仓库并推送
cp public/registry.json /path/to/toolbox-registry/
cp public/plugins/*.zip /path/to/toolbox-registry/plugins/
git add -A && git commit -m "publish: <tool> <version>" && git push
```

registry 中的 `package.file` 是相对 registry.json 所在目录的路径（`plugins/<id>-<version>.zip`），
`package.sha256` 是 zip 的 SHA-256，客户端安装时会强制校验。

## 新增工具

在 toolbox 项目的 `plugins/<id>/` 下创建 `manifest.json` + `src/main.tsx`
（导出 `mount(container)` / `unmount()`），然后走上面的发布流程。
