1. 开启多核压缩: 
uglifyjs-webpack-plugin 改用 terser-webpack-plugin

2. 监控面板, 打包速度及各个模块检测插件
speed-measure-webpack-plugin

3. 开启通知面板
webpack-build-notifier

4. 开启打包进度
progress-bar-webpack-plugin

5. 让开发面板更清晰
webpack-dashboard

6. 设置窗口标题
node-bash-title

## 上线阶段

1. 多核压缩，不编译 es6 
使用 <script type="module"></script>
cdn.polyfill.io

2. 前端缓存小负载
webpack-manifest-plugin

3. htmlWebpackPlugin 配置 loading，通过 webpack 注入进去

4. 分析打包结果
bundlesize, webpack-chart webpack-bundle-analyzer

5. test exculde include loader 设置

6. 压缩 js css 
hapypack 针对于 ts-loader
nano
optimize-css-assets-webpack-plugin
webpack-parallet-uglify-plugin

7. devtool eval cache-loader