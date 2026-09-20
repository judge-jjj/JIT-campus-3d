# 金陵云校园 · 江宁校区

单个 HTML 的校园导览项目，内置用户提供的 GLB 贴图模型，另附官方航拍实景入口。电脑端支持鸟瞰、第一人称漫游、WASD、鼠标转向、空格跳跃、Shift 加速、建筑碰撞和地标定位。

## 三个板块与官方入口

- **贴图模型（新增，默认打开）**：内嵌用户提供的 GLB、地面影像和25张贴图，支持53个地标的搜索定位、日光／暖阳、精细／流畅画质、手动校正标注与JSON导入导出。采用鼠标环绕浏览。
- **原版鸟瞰／原版漫游**：Three.js、OSM 快照、53 处地标、程序化材质和代码全部内置，可离线运行。新增体块及建筑细节是简化重建，位置和楼高不是测绘结果。
- **官方实景 3D**：点击顶部按钮，新标签打开 https://jit3d.yufeiaero.com/#/main 。该地址由[学校官网](https://www.jit.edu.cn/)“学校地图”链接提供，已实际查看江宁校区 2024 年场景，包含真实立面、屋顶和校门。这部分需要联网；模型仍由原平台提供，没有复制进本项目。

用户模型与官方平台是两套独立数据；新增板块使用的是用户文件，不是从官方平台下载的模型。

当前内嵌环境测试中，跨站 iframe 没有显示场景，因此采用独立页面入口。原平台使用自己的交互；本页搜索、WASD、碰撞和新标注作用于本页模型，不会修改官方场景。入口可访问不代表已取得模型下载或再部署授权。

## 新增 GLB 模型：标注与优化

原始文件：`shapezo_model_area-001_enu-31.907054-118.8947-0.glb`。源文件保持不变，没有覆盖或重新导出到下载目录。

| 项目 | 结果 |
| --- | --- |
| 原始 GLB | 20,619,032 字节，约20.62 MB |
| 优化 GLB | 13,809,240 字节，约13.81 MB |
| 无损 gzip 后 | 6,478,952 字节，约6.48 MB，比原文件小68.6% |
| 单文件网页 | 约9.56 MB，包含Base64模型、引擎、数据、样式及所有代码 |
| 几何与贴图 | GLB内保留全部顶点、法线、UV、实例与贴图分辨率；25张图片转为90质量WebP（有损），透明通道保留 |

使用原始 ENU 根节点旋转，按原点差平移 X=-14.8733 m、Z=-7.1122 m。对照底图检查了图书馆、德业楼与宿舍区的匹配。标注来自原先两张用户导览图，向模型表面投影；这不是测绘级定位。部分组团中心落在庭院，北区科技楼1号等位置没有完整建筑，显示虚线参考标注，不自动补造为实景建筑。

渲染优化包括：正确的sRGB颜色处理、抗锯齿、斜视纹理过滤、阴影、两种光照、限制像素比、近远树木精度切换。剔除校区外、水面和建筑内部的树实例，调整偏黑的树木颜色。全景实测约24万三角面，原始直接显示约291万；近景会随距离增加细节。触屏默认“流畅”，电脑默认“精细”。只渲染当前板块，共用一个WebGL画布。

源模型把南操场当成了建筑。本页显示时移除对应体块的355个三角面，露出模型自带的操场地面影像。此修正不写回原始GLB，不改变原版导览数据。其他屋顶／立面仍按用户模型呈现，没有声称还原真实校门或缺失高楼。文件中的建筑采用模板贴图，地面包含影像，不能据此称为完整的倾斜摄影实景模型。

### 校正标注

1. 在“贴图模型”板块搜索并选择地标。
2. 点击“校正标注”，再点选建筑屋顶、入口附近或其他模型表面。
3. 修改立即保存在当前浏览器的localStorage。Esc取消；“恢复点位”只恢复当前地标。
4. “导出标注”保存校正JSON；“导入标注”可在另一浏览器合并这些校正。未校正的地标使用内置位置。

JSON包含模型标识、米制坐标原点和校正点；拒绝未知地标、无效坐标以及大于128 KB的文件。导入不会改名、改几何或执行代码。校正不修改原版地图和漫游碰撞。

浏览器校正不会自动写进index.html或同步到GitHub：搬到另一个域名、换浏览器或清理站点数据后，应重新导入导出的JSON。若要把校正作为所有访客的默认标注，在index.html搜索 `id="model-annotation-defaults"`，把其中的空对象 `{}` 替换为导出JSON的 `overrides` 对象，再上传index.html。这只更新贴图模型的默认点位；旧导览坐标不变。访客已有的本地校正优先于默认点位，点击“恢复点位”可回到默认。

### 保留的导览内容

- 依据两张用户导览图整理 **53 个地标**，增加宿舍、生活分类。
- 补入北区科技楼 **1、2、3、4号**；1号表现为高层及裙楼。旧 OSM 没有这片完整轮廓，补画的位置和尺寸为近似。
- 科技楼3号一楼改为 **赵一鸣零食**；“罗森”仅保留为旧名称搜索别名，结果显示新店名。
- 按用户最终确认，北侧保留 **德业楼1 / 德业楼2**；参考图2中图书馆西南侧的 **勤业楼**单独标注。
- 南区宾馆名称按用户确认修正为 **望山湖宾馆**，同步用于贴图模型、原版导览、搜索及导出数据。
- 补充知行楼、行政楼、大学生活动中心、南苑／北苑／科技园餐厅、菜鸟驿站、生活超市、南北区宿舍及东苑。
- 将旧底图错误显示成实心建筑的南操场修正为露天跑道、足球场；体育馆单列。
- 勤业楼组团拆成平行教学翼楼和连接体，释放庭院。
- 增加红瓦坡屋顶、白色立面、窗框、图书馆门廊、西门白色门柱及小坡顶；改善树冠、球场、材质和阴影。

部分标注以组团匹配。北06/北07B、教学楼分翼、科技园餐厅、科技楼4号与崇文楼对应、店铺入口仍需进一步现场核实。选中地标可查看来源与近似说明。楼高、草地和门体尺寸为示意，不用于测量和现实导航。

## 文件与运行

~~~text
index.html   完整网页，约9.56 MB，模型与贴图也已内置
README.md    说明、部署、标注校正与模型来源
.gitignore   本地临时文件忽略规则
~~~

电脑用新版 Chrome / Edge / Firefox 直接打开 index.html。鼠标锁定受限时，在三个文件所在目录启动本地服务：

~~~bash
python -m http.server 8000
~~~

浏览器打开 http://localhost:8000 。鼠标锁定仅在点击后申请，被拒绝时自动改为“拖拽转向 + WASD”。窗口失焦、页面隐藏、Esc 和切换视角时会清空按键，防止自动行走。

## 操作

| 模式 | 操作 | 效果 |
| --- | --- | --- |
| 贴图模型 / 鸟瞰 | 左键拖拽 | 旋转、调整俯仰 |
| 鸟瞰 | 滚轮、+ / − | 缩放 |
| 鸟瞰 | 右键拖拽 / Shift + 拖拽 | 平移 |
| 任意 | 搜索并点击地标 | 模型／鸟瞰飞到地标；原版漫游传送到附近安全位置 |
| 贴图模型 | 校正标注 / 恢复点位 | 在模型表面设置位置 / 恢复当前地标 |
| 贴图模型 | 导出 / 导入标注 | 备份及合并手动校正JSON |
| 贴图模型 | 精细 / 流畅、日光 / 暖阳 | 调整负载及光照 |
| 漫游 | 点击开始，WASD | 前后左右移动 |
| 漫游 | 鼠标 | 转向；受限环境用拖拽 |
| 漫游 | 空格 / Shift | 跳跃 / 按住加速 |
| 漫游 | Esc | 释放鼠标并暂停 |
| 任意 | 房屋图标 | 恢复全景 / 漫游返回西门 |
| 任意 | 问号 | 帮助、来源说明、GeoJSON 导出 |
| 任意 | 官方实景3D | 联网打开学校原平台 |

触屏支持方向键、跳跃、加速和拖拽；鸟瞰可双指缩放。放大地图会按屏幕空间显示更多标签。可搜索“科技楼”“赵一鸣”“罗森”“德业”“勤业”“南01”等。

以下物理参数仅用于原版漫游，新增GLB板块不启用WASD或碰撞行走。角色半径0.55 m，位移拆成不超过0.20 m的子步，支持沿墙滑动。物理固定120 Hz，步速4.5 m/s，跑速8 m/s，重力18 m/s²。斜向速度归一化，水域及校区边界限制通行，已标注桥梁可通过。不含室内、楼梯、屋顶行走和游泳。树木、门柱等装饰不单独碰撞，碰撞以主体轮廓为准。

## GitHub Pages

1. 新建仓库，例如 jinling-campus-3d。
2. 把三个文件直接放到仓库根目录，不要再套 outputs 文件夹。
3. Settings → Pages → Deploy from a branch → main / (root) → Save。
4. 部署完成后访问 https://你的用户名.github.io/jinling-campus-3d/ 。

上传不需要原始GLB或额外assets文件夹；资源全部在index.html中。无需 npm、API key 或构建流程。支持仓库子路径。本项目没有替你创建仓库或发布网站。离线导览不请求 CDN；GitHub Pages 和官方实景平台的可达性仍取决于网络及原站服务。

## 航拍模型获取：核查结果

已找到并验证官方实景平台；**尚未找到官方江宁校区航拍模型的公开下载入口或开放再部署许可。** 本项目现在已接入用户另行提供的GLB；它不等于官方平台的航拍数据。模型及影像的来源权利归其原提供方，公开部署时按该文件来源的使用许可处理。 其他城市的公开倾斜摄影数据不能代替这所学校的真实场景。

可通过学校 **信息化建设与管理中心**咨询负责单位及数据使用方式。官网[办公电话一览表](https://www.jit.edu.cn/xxgk1/dhylb.htm)提供公开联系方式。本次没有发送申请、联系他人或替用户接受条款。

申请时说明需要：

1. 江宁校区最新或2024年 **3D Tiles**：根 tileset.json、全部子 tileset、B3DM/GLB/纹理等依赖；或获得授权的在线服务接口。
2. 若只有 **OSGB**，询问是否允许转换为网页用3D Tiles；也可询问带纹理的OBJ/GLB。
3. 坐标系、垂直基准、原点或变换矩阵、单位、拍摄日期、覆盖范围、精度和体积。
4. 授权范围：个人开发、校内或公开网页展示、模型修改、再托管、再分发；特别说明是否允许资源进入公开GitHub仓库。
5. 在线服务的域名白名单、跨域、鉴权和流量规则，以及署名要求。

可调整后发送的咨询文字：

> 您好，我正在开发金陵科技学院江宁校区的三维校园导览网页，希望使用学校现有的航拍实景模型。官网链接的三维平台可查看江宁校区2024年场景，想咨询数据负责单位，以及能否申请用于网页展示的3D Tiles数据包或授权服务接口。
>
> 请问能否提供根tileset.json及模型、纹理、坐标系和拍摄日期？如果是OSGB，也希望了解是否允许转换。项目考虑通过GitHub Pages公开展示，烦请明确是否允许公开展示、资源再托管和模型修改，以及署名或其他授权要求。若暂不能提供数据，是否有可授权嵌入的页面或接口？谢谢。

取得数据后，可用Cesium或Three.js的3D Tiles加载器按需加载。前端可以继续保持单HTML，但整校航拍模型通常需要独立托管多个资源文件。普通卫星底图本身无法还原真实建筑立面。

## 数据与源码位置

原始OSM快照：校区边界 [way/92626947](https://www.openstreetmap.org/way/92626947)，获取日期2026-09-20，含46个建筑轮廓、62段道路、3处水面、3段水道、3个球场及命名点。米制坐标原点为WGS84 118.8948576°E、31.90699°N，X东、Y上、Z南。

本版有54个导览建筑体块。原始数据在 sourceFeatures，修改后几何在 features，53个注记在 annotations，参考依据在 referenceSources。新增体块标记 geometrySource 为 reference-approx。GeoJSON导出包含来源、注记和近似标志。

| 搜索位置 | 作用 |
| --- | --- |
| style | 界面布局和样式 |
| model-asset / model-vendor / model-view | 内嵌gzip模型、官方GLTFLoader/OrbitControls及新板块逻辑 |
| id="campus-data" | 地图、原始快照、注记和来源 |
| id="three-vendor" | Three.js r160.1完整库 |
| id="physics-core" | 碰撞、移动、跳跃 |
| id="campus-visuals" | 屋顶、材质、门廊、门柱、运动场 |
| id="campus-app" | 场景、搜索、视角、标签和导出 |

原版导览纹理由Canvas生成；贴图模型纹理来自用户GLB，已内嵌。未打包两张用户参考图整图、官方照片或官方航拍瓦片。[官网校园风景](https://www.jit.edu.cn/xxgk/xywh/xyfj.htm)用于原版建筑视觉参考。CSP只允许内联脚本、样式、data/blob图片及本地blob读取，阻止外网资源请求；官方页面只在主动点击链接时打开。没有Service Worker或首次联网缓存前置要求。

## 验证与限制

13项原版检查覆盖碰撞、移动、全部注记关联与安全落脚点。新增检查验证：原始GLB哈希不变、内嵌压缩包可解码、几何数据保留、标注坐标校验、图书馆坐标投影及操场修正。浏览器中核验了新旧板块切换、搜索定位、光照/画质切换、点选校正及刷新后保存。浏览器需要WebGL，完整Pointer Lock取决于运行环境。低性能设备可切换“流畅”。

程序化导览模型不具备摄影测量的照片细节；真实画质通过顶部官方实景入口查看。官方2024年模型也不保证反映用户提供的最新店铺变化。

地图与派生数据库使用ODbL 1.0，保留© OpenStreetMap contributors署名：
https://www.openstreetmap.org/copyright
https://opendatacommons.org/licenses/odbl/1-0/


## 代码与第三方许可

本项目自编的界面、交互、程序化景观和碰撞代码采用 MIT 许可。地图数据独立遵循上文 ODbL 1.0，不能因代码使用 MIT 而忽略地图许可。

```text
MIT License

Copyright (c) 2026 Jinling Campus Explorer contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

Three.js r160.1 由 three.js authors 开发，采用 MIT 许可；其官方完整版权与许可文本已嵌入 `index.html` 中的引擎脚本前。版本来源：<https://www.npmjs.com/package/three/v/0.160.1>。该版本的经典脚本构建适合直接内置单文件，不需要模块加载器。


## fflate 0.8.2 许可

用于单HTML内置模型的gzip解压。Three.js官方GLTFLoader与OrbitControls同引擎使用MIT许可。

~~~text
MIT License

Copyright (c) 2023 Arjun Barrett

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.~~~
