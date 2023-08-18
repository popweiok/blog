- ## 特点
	- Wireguard相比传统VPN的核心优势是没有VPN网关，所有节点之间都可以点对点（P2P）连接，全互联模式（Full Mesh）
	- Wireguard目前最大的痛点就是上层应用的功能不够健全，本身只是个内核级别的模块，至于上层的更高级的功能（比如秘钥交换机制，UDP 打洞，ACL 等），需要通过用户空间的应用来实现。
-
- ## Zero Trust
	- ### Netmaker
		- 通过可视化界面来配置 WireGuard 的全互联模式，它支持 UDP 打洞、多租户等各种高端功能，几乎适配所有平台，非常强大。然而现实世界是复杂的，无法保证所有的 NAT 都能打洞成功，且 Netmaker 目前还***没有 fallback*** 机制，如果打洞失败，无法 fallback 改成走中继节点
	- ### Tailscal
		- ***支持 fallback*** 机制，可以尽最大努力实现全互联模式，部分节点即使打洞不成功，也能通过中继节点在这个虚拟网络中畅通无阻。
		  和 Netmaker 类似，最大的区别在于 Tailscale 是在用户态实现了 WireGuard 协议，而 Netmaker 直接使用了内核态的 WireGuard。所以 Tailscale 相比于内核态 WireGuard 性能会有所损失
		  Tailscale 是一款商业产品，但个人用户是可以白嫖的，个人用户在接入设备不超过 20 台的情况下是可以免费使用的（虽然有一些限制，比如子网网段无法自定义，且无法设置多个子网）。除 Windows 和 macOS 的图形应用程序外，其他 Tailscale 客户端的组件（包含 Android 客户端）是在 BSD 许可下以开源项目的形式开发的，你可以在他们的 GitHub 仓库找到各个操作系统的客户端源码。
		  对于大部份用户来说，白嫖 Tailscale 已经足够了，如果你有更高的需求，比如自定义网段，可以选择付费
	- ### Headscale
		- Tailscale 的控制服务器是不开源的，而且对免费用户有诸多限制，这是人家的摇钱树，可以理解。好在目前有一款开源的实现叫 Headscale，这也是唯一的一款，希望能发展壮大
		- 部署
			- [Tailscale 基础教程：Headscale 的部署方法和使用教程 – 云原生实验室 - Kubernetes|Docker|Istio|Envoy|Hugo|Golang|云原生 (icloudnative.io)](https://icloudnative.io/posts/how-to-set-up-or-migrate-headscale/)
		-
-