- [基于 Cloudflare Workers 和 cloudflare-docker-proxy 搭建镜像加速服务 - 探索云原生 - 博客园](https://www.cnblogs.com/KubeExplorer/p/18264358#%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E)
  tags:: [[SendToLogseq]]
	- 本文主要介绍了如何基于 Cloudflare Workers 和 cloudflare-docker-proxy 搭建 dockerhub、gcr、quay 等镜像加速服务。
	- 最近，受限于各种情况，部分主流镜像站都关了，为了能够正常使用，建议自己搭建一个加速器。
	- 写文之前，也已经部署好了一个，可以直接使用，具体使用方法跳转 https://docker.lixd.xyz 或者 使用说明 查看。
	- ## 准备工作
		- 1）首先要注册一个 Cloudflare 账号
		- 2）Cloudflare 账号下需要添加一个域名，推荐到万网注册一个域名，再转移到 Cloudflare。
		  * 推荐 top、xyz 后缀应该都是首年 7 块
	- ## 部署流程
	- ### 修改配置
	- 首先 fork https://github.com/ciiiii/cloudflare-docker-proxy 仓库到自己账号下。
	- 然后修改 `src/index.js`，修改内容如下：
	- ```javascript
	  const routes = {
	  "docker.mydomain.com": "https://registry-1.docker.io",
	  "quay.mydomain.com": "https://quay.io",
	  "gcr.mydomain.com": "https://gcr.io",
	  "k8s-gcr.mydomain.com": "https://k8s.gcr.io",
	  "k8s.mydomain.com": "https://registry.k8s.io",
	  "ghcr.mydomain.com": "https://ghcr.io",
	  "cloudsmith.mydomain.com": "https://docker.cloudsmith.io",
	  };
	  ```
	- > 其中 mydomain.com 就是你的域名，比如我这里用的是 lixd.xyz
	- 再修改 `wrangler.toml`，同样是替换域名
	- ```toml
	  [env.production]
	  name = "cloudflare-docker-proxy"
	  routes = [
	  { pattern = "docker.mydomain.com", custom_domain = true },
	  { pattern = "quay.mydomain.com", custom_domain = true },
	  { pattern = "gcr.mydomain.com", custom_domain = true },
	  { pattern = "k8s-gcr.mydomain.com", custom_domain = true },
	  { pattern = "k8s.mydomain.com", custom_domain = true },
	  { pattern = "ghcr.mydomain.com", custom_domain = true },
	  { pattern = "cloudsmith.mydomain.com", custom_domain = true },
	  ]
	  
	  [env.production.vars]
	  MODE = "production"
	  TARGET_UPSTREAM = ""
	  
	  [env.staging]
	  name = "cloudflare-docker-proxy-staging"
	  route = { pattern = "docker-staging.mydomain.com", custom_domain = true }
	  ```
	- > 同样的 mydomain.com 就是你的域名
	- 最后修改 README.md 中的 ![image](https://deploy.workers.cloudflare.com/button){:width 184 :height 39} 图标对应的 Github 仓库地址为你 fork 后的仓库地址，比如 https://deploy.workers.cloudflare.com/?url=https://github.com/lixd/cloudflare-docker-proxy。
	- 这三个修改都完成后，提交代码。
	- ### 帮助文档
	- 为了便于使用，可以在访问根目录时返回一个帮助页面。
	- #### help.html
	- 新建一个`help.html` 文件，内容如下：
	- ```html
	  <!DOCTYPE html>
	  <html lang="zh-CN">
	  <head>
	    <meta charset="utf-8">
	    <meta name="viewport" content="width=device-width, initial-scale=1">
	    <title>镜像使用说明</title>
	    <style>
	        body {
	            font-family: 'Roboto', sans-serif;
	            margin: 0;
	            padding: 0;
	            background-color: #f4f4f4;
	        }
	        .header {
	            background: linear-gradient(135deg, #667eea, #764ba2);
	            color: #fff;
	            padding: 20px 0;
	            text-align: center;
	            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
	            position: relative;
	        }
	        .github-link {
	            position: absolute;
	            top: 10px;
	            right: 20px;
	            color: #fff;
	            text-decoration: none;
	        }
	        .github-icon {
	            width: 24px;
	            height: 24px;
	            vertical-align: middle;
	        }
	        .container {
	            max-width: 800px;
	            margin: 40px auto;
	            padding: 20px;
	            background-color: #fff;
	            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
	            border-radius: 10px;
	        }
	        .content {
	            margin-bottom: 20px;
	        }
	        .footer {
	            text-align: center;
	            padding: 20px 0;
	            background-color: #333;
	            color: #fff;
	        }
	        pre {
	            background-color: #272822;
	            color: #f8f8f2;
	            padding: 15px;
	            border-radius: 5px;
	            overflow-x: auto;
	        }
	        code {
	            font-family: 'Source Code Pro', monospace;
	        }
	        a {
	            color: #4CAF50;
	            text-decoration: none;
	        }
	        a:hover {
	            text-decoration: underline;
	        }
	        @media (max-width: 600px) {
	            .container {
	                margin: 20px;
	                padding: 15px;
	            }
	            .header {
	                padding: 15px 0;
	            }
	        }
	    </style>
	    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&family=Source+Code+Pro:wght@400;700&display=swap" rel="stylesheet">
	  </head>
	  <body>
	  <div class="header">
	    <h1>镜像使用说明</h1>
	    <a href="https://github.com/lixd/cloudflare-docker-proxy" target="_blank" class="github-link">
	        <img src="https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png" alt="GitHub" class="github-icon">
	    </a>
	  </div>
	  <div class="container">
	    <div class="content">
	        <p>为了加速 Docker 镜像拉取，你可以使用以下命令设置 registry mirror:</p>
	        <pre><code id="registry-config">sudo tee /etc/docker/daemon.json &lt;&lt;EOF
	  {
	    "registry-mirrors": ["https://{{host}}"]
	  }
	  EOF
	  # 配置完后需要重启 Docker 服务
	  sudo systemctl restart docker
	  </code></pre>
	        <p>使用该代理从不同的镜像仓库拉取镜像，请参考以下命令：</p>
	        <pre><code id="commands">
	  # docker pull nginx:latest
	  docker pull docker.{{host}}/library/nginx:latest  # 拉取 Docker 官方镜像
	  
	  # docker pull quay.io/coreos/etcd:latest
	  docker pull quay.{{host}}/coreos/etcd:latest  # 拉取 Quay 镜像
	  
	  # docker pull gcr.io/google-containers/busybox:latest
	  docker pull gcr.{{host}}/google-containers/busybox:latest  # 拉取 GCR 镜像
	  
	  # docker pull k8s.gcr.io/pause:latest
	  docker pull k8s-gcr.{{host}}/pause:latest  # 拉取 k8s.gcr.io 镜像
	  
	  # docker pull registry.k8s.io/pause:latest
	  docker pull k8s.{{host}}/pause:latest  # 拉取 registry.k8s.io 镜像
	  
	  # docker pull ghcr.io/github/super-linter:latest
	  docker pull ghcr.{{host}}/github/super-linter:latest  # 拉取 GitHub 容器镜像
	  
	  # docker pull docker.cloudsmith.io/public/repo/image:latest
	  docker pull cloudsmith.{{host}}/public/repo/image:latest  # 拉取 Cloudsmith 镜像
	  </code></pre>
	        <p>为了避免 Worker 用量耗尽，你可以手动 pull 镜像然后 re-tag 之后 push 至本地镜像仓库。</p>
	    </div>
	  </div>
	  <div class="footer">
	    <p>Powered by Cloudflare Workers</p>
	    <p><a href="https://lixueduan.com" target="_blank">访问博客 探索云原生</a></p>
	  </div>
	  <script>
	    document.addEventListener('DOMContentLoaded', function() {
	        const host = window.location.hostname;
	        const mainDomain = host.split('.').slice(-2).join('.');
	        const registryConfigElement = document.getElementById('registry-config');
	        const commandsElement = document.getElementById('commands');
	  
	        registryConfigElement.innerHTML = registryConfigElement.innerHTML.replace(/{{host}}/g, host);
	        commandsElement.innerHTML = commandsElement.innerHTML.replace(/{{host}}/g, mainDomain);
	    });
	  </script>
	  </body>
	  </html>
	  ```
	- #### 增加路由
	- `scr/index.js` 中增加一条路由,访问根目录时就返回这个帮助页面
	- ```javascript
	  import DOCS from './help.html'
	  
	  // return docs
	  if (url.pathname === "/") {
	  return new Response(DOCS, {
	    status: 200,
	    headers: {
	      "content-type": "text/html"
	    }
	  });
	  }
	  ```
	- 完整内容见：https://github.com/lixd/cloudflare-docker-proxy
	- ### 点击部署
	- 然后在 Github 界面点击这个 ![image](https://deploy.workers.cloudflare.com/button){:width 184 :height 39} 图标进行部署，会自动跳转到 cloudflare，按步骤操作即可，最终会在 Github 仓库中创建一个 Github Action 来将该仓库部署到 Cloudflare Workers。
	- 就像这样：
	- ![image](https://img.lixueduan.com/docker/mirror/deploy-to-cloudflare.png){:width 836 :height 717}
	- ### 等待部署完成
	- 可以到 Github 查看 Action 执行进度
	- ![image](https://img.lixueduan.com/docker/mirror/deploy-progress.png){:width 836 :height 437}
	- 执行完成后，切换到 Cloudflare Dashboard ，不出意外的话就可以看到刚创建的 Worker 了。
	- ![image](https://img.lixueduan.com/docker/mirror/cloudflare-workers.png){:width 836 :height 420}
	- 切换到 Setting，等待 SSL 证书签发完成即可
	- ![image](https://img.lixueduan.com/docker/mirror/cloudflare-workers-setting.png){:width 836 :height 398}
	- ## 使用说明
	- 部署完成后，访问 https://docker.mydomain.com 就可以看到使用说明了。
	- > 比如：https://docker.lixd.xyz
	- 为了加速 Docker 镜像拉取，你可以使用以下命令设置 registry mirror:
	- ```bash
	  sudo tee /etc/docker/daemon.json <<EOF
	  {
	    "registry-mirrors": ["https://docker.lixd.xyz"]
	  }
	  EOF
	  # 配置完后需要重启 Docker 服务
	  sudo systemctl restart docker
	  ```
	- 使用该代理从不同的镜像仓库拉取镜像，请参考以下命令：
	- ```bash
	  # docker pull nginx:latest
	  docker pull docker.lixd.xyz/library/nginx:latest  # 拉取 Docker 官方镜像
	  
	  # docker pull quay.io/coreos/etcd:latest
	  docker pull quay.lixd.xyz/coreos/etcd:latest  # 拉取 Quay 镜像
	  
	  # docker pull gcr.io/google-containers/busybox:latest
	  docker pull gcr.lixd.xyz/google-containers/busybox:latest  # 拉取 GCR 镜像
	  
	  # docker pull k8s.gcr.io/pause:latest
	  docker pull k8s-gcr.lixd.xyz/pause:latest  # 拉取 k8s.gcr.io 镜像
	  
	  # docker pull registry.k8s.io/pause:latest
	  docker pull k8s.lixd.xyz/pause:latest  # 拉取 registry.k8s.io 镜像
	  
	  # docker pull ghcr.io/github/super-linter:latest
	  docker pull ghcr.lixd.xyz/github/super-linter:latest  # 拉取 GitHub 容器镜像
	  
	  # docker pull docker.cloudsmith.io/public/repo/image:latest
	  docker pull cloudsmith.lixd.xyz/public/repo/image:latest  # 拉取 Cloudsmith 镜像
	  ```
	- 为了避免 Worker 用量耗尽，你可以手动 pull 镜像然后 re-tag 之后 push 至本地镜像仓库。
	- ## FAQ
	- **Build & Deploy The process '/usr/local/bin/yarn' failed with exit code 1 +1**
	- ```bash
	  ✘ [ERROR] A request to the Cloudflare API (/accounts/***/workers/scripts/cloudflare-docker-proxy/domains/records) failed.
	  
	    workers.api.error.origin_conflict_existing_dns_record [code: 100117]
	  ```
	- 如果提前添加了 DNS 解析则出现这个错误，部署之后会自动添加解析记录，因此部署前不要手动添加记录。
	- 如果出现该问题，可以把 DNS 解析记录删除后再试。
	- ***
	- 如果你对云原生技术充满好奇，想要深入了解更多相关的文章和资讯，欢迎关注微信公众号。
	- 搜索公众号【**探索云原生**】即可订阅
	- ![image](https://img.lixueduan.com/about/wechat/search.png){:width 836 :height 137}
	- ***
	- ## 参考
	- cloudflare-docker-proxy
	- 白嫖Cloudflare Workers 搭建 Docker Hub镜像加速服务|
	- 又发掘一个CF的新用法，利用Worker构建Docker Registry Mirror
	- Docker 镜像库国内加速的几种方法
-