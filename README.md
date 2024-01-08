<!--
 * @Author: Cross
 * @Date: 2022-04-25 16:03:46
 * @LastEditTime: 2024-01-08 15:57:45
 * @FilePath: /Blog/README.md
 * @Description: 
-->

项目初始化
npm install -g hexo-cli // 安装hexo
npm i // 安装项目依赖
git clone https://github.com/zhwangart/hexo-theme-ocean.git themes/ocean // 安装主题
hexo server // 启动项目

创建新增的文档
hexo new '$md-name'

发布文档的流程
git add .
git commit -m '$message'
git push
npm run deploy