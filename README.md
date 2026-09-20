# 🚀 Sing-Box-Plus 一键管理脚本（18 节点：直连 9 + WARP 9）

开箱即用 18 个节点（直连 9 + WARP 9），含端口一键切换、BBR 加速、分享链接导出等。

* ✅ 已适配 **sing-box v1.12.x**
* ✅ 支持 ​**WARP 出站**（官方 warp-cli，更高兼容性，非 wgcf）​
* ✅ 一键生成证书（自签），一键 systemd 托管
* ✅ **更换端口**后自动重写配置与放行
* ✅ 分享链接分组打印（直连 9 / WARP 9），导入即用
* ✅ WARP 节点，将服务器 IP“变身”为 Cloudflare 的中性出口，Netflix/Disney+/YouTube 等流媒体解锁
* ✅ 支持 **VPN Gate OpenVPN 落地**：自动获取节点、按国家选择，并通过策略路由仅让落地进程走 VPN

---

## ✨ 默认部署内容（18 个节点）

**直连 9：**

* VLESS Reality（Vision 流）
* VLESS gRPC Reality
* Trojan Reality
* VMess WS
* Hysteria2（直连证书）
* Hysteria2 + OBFS(salamander)
* Shadowsocks 2022（2022-blake3-aes-256-gcm）
* Shadowsocks（aes-256-gcm）
* TUIC v5（ALPN h3，自签证书）

​**WARP 9：**（同上 9 种，出站经 Cloudflare WARP）

> WARP 出站更利于流媒体解锁与回程质量。

​**注意： Shadowsocks 2022 和 Shadowsocks 协议可能容易被封，不推荐使用。**

---

## ✅ 支持系统

支持系统：Debian 11+ / Ubuntu 20.04+ / CentOS Stream 9+ / Rocky 9+ / AlmaLinux 9+ / Fedora 38+ / Arch(rolling) / openSUSE Leap 15.4+

已在[Vultr](https://www.vultr.com/?ref=7048874) 上测试通过。

---

## 📥 一键安装 / 更新脚本

```bash
wget -O sing-box-plus.sh https://raw.githubusercontent.com/chengamki-tech/vps/main/sing-box-plus.sh  && chmod +x sing-box-plus.sh && bash sing-box-plus.sh
```
或者

```bash
curl -fsSL -o sing-box-plus.sh https://raw.githubusercontent.com/chengamki-tech/vps/main/sing-box-plus.sh  && chmod +x sing-box-plus.sh && bash sing-box-plus.sh

```

安装完成后，输入 bash sing-box-plus.sh 可进入管理页面。

---

## 🧭 功能菜单

```text
 🚀 Sing-Box-Plus 管理脚本 v3.3.0 🚀
 脚本更新地址: https://github.com/chengamki-tech/vps
=============================================================
系统加速状态：已启用 / 未启用 BBR
Sing-Box 启动状态：运行中 / 未运行 / 未安装
=============================================================
  1) 安装/部署（18 节点）
  2) 查看分享链接（IPv4）
  6) 查看分享链接（IPv6）
  3) 重启服务
  4) 一键更换所有端口
  5) 一键开启 BBR
  7) 配置/管理 SOCKS5 落地 IP
  8) 配置/管理 VPN Gate 落地
  9) 卸载
  0) 退出
=============================================================
```

---

## 🧩 版本更新日志

| 版本    | 日期     | 变更 |
|--------|----------|------|
| v3.3.0 | 2026-09 | - 新增 SOCKS5 落地 IP，以及 VPN Gate OpenVPN 落地（自动选节点、独立用户策略路由、IPv6 泄漏防护）。 |
| v3.2.0 | 2026-01 | - WARP采用官方 warp-cli，更高兼容性。 |


---

## 📂 文件与目录

| 路径                                             | 说明                                   |
| -------------------------------------------------- | ---------------------------------------- |
| `/usr/local/bin/sing-box`                    | sing-box 二进制                        |
| `/opt/sing-box/config.json`                  | 主配置（自动生成）                     |
| `/opt/sing-box/data/`                        | sing-box 数据目录                      |
| `/opt/sing-box/cert/{fullchain.pem,key.pem}` | 自签证书（按`REALITY_SERVER`生成） |
| `/opt/sing-box/ports.env`                    | 18 个端口持久化                        |
| `/opt/sing-box/env.conf`                     | 全局环境配置                           |
| `/opt/sing-box/creds.env`                    | 凭据（UUID、Reality Keypair、SS 等）   |
| `/opt/sing-box/warp.env`                     | WARP 关键参数（规范化后）              |
| `/opt/sing-box/wgcf/`                        | `wgcf`账号与 profile               |

---

## 🚦 使用步骤

1. **首次运行脚本** → 选择 `1) 安装/部署（18 节点）`
   * 自动安装 sing-box / jq / curl 等依赖
   * 自动生成凭据与证书、WARP 出站、写入 `config.json`
   * 自动注册 systemd 并启动
2. **查看分享链接** → `2) 查看分享链接`
   * 直连 9 与 WARP 9 **分组输出**
   * 可直接导入到 v2rayN / sing-box / Shadowrocket 等
3. **更换端口** → `4) 一键更换所有端口`
   * 18 个端口全部生成不冲突的新端口
   * 自动重写 `config.json` + 放行端口 + 重启服务
   * （已修复）**一次回车即可返回主菜单**
4. **开启 BBR** → `5) 一键开启 BBR`
   * 自动检测并设置 `fq + bbr`，提高拥塞控制与队列质量
5. **重启服务** → `3) 重启服务`
6. **配置 SOCKS5 落地 IP（可选）** → `7) 配置/管理 SOCKS5 落地 IP`
   * 落地侧需要先提供 **SOCKS5 代理**，仅填写一个裸 IP 无法直接作为出口
   * 输入地址、端口、用户名/密码，并选择应用范围：
     - `直连9`：原有 WARP 9 不受影响（推荐）
     - `WARP9`：原直连 9 不受影响
     - `全部18`：18 个节点统一从落地 IP 出去
   * 保存后脚本会自动重写配置、校验、重启，并尝试请求 `api.ipify.org` 显示实际出口 IP
7. **配置 VPN Gate 落地（可选）** → `8) 配置/管理 VPN Gate 落地`
   * 输入国家/地区代码或关键词，例如 `JP`、`Japan`、`KR`；留空默认 `JP`
   * 脚本从 VPN Gate 公共 API 选择评分最高的匹配节点，自动下载 OpenVPN 配置
   * 使用独立系统用户和策略路由，只让 VPN Gate 本地 SOCKS5 的流量进入 OpenVPN，不改入口 VPS 默认路由
   * 可选择 `直连9`、`WARP9` 或 `全部18` 使用 VPN Gate 出口
8. **卸载** → `9) 卸载`
   * 停止服务、移除 systemd、保留数据目录（如需全清自行删除 `/opt/sing-box`）

---

## 🔗 分享链接示例（片段）

脚本会为每个入站生成标准导入链接，例如：

<pre class="overflow-visible!" data-start="3484" data-end="3958"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-text"><span><span># 直连（示例）
vless://&lt;UUID&gt;@&lt;IP&gt;:&lt;PORT&gt;?encryption=none&amp;flow=xtls-rprx-vision&amp;security=reality&amp;sni=www.microsoft.com&amp;fp=chrome&amp;pbk=&lt;REALITY_PUB&gt;&amp;sid=&lt;SID&gt;&amp;type=tcp#vless-reality
vmess://&lt;Base64(JSON)&gt;
hy2://&lt;pwd_b64url&gt;@&lt;IP&gt;:&lt;PORT&gt;?insecure=1&amp;allowInsecure=1&amp;sni=&lt;REALITY_SERVER&gt;#hysteria2
ss://&lt;base64(method:password)&gt;@&lt;IP&gt;:&lt;PORT&gt;#ss / #ss2022
tuic://&lt;uuid&gt;:&lt;uuid&gt;@&lt;IP&gt;:&lt;PORT&gt;?congestion_control=bbr&amp;alpn=h3&amp;insecure=1&amp;allowInsecure=1&amp;sni=&lt;REALITY_SERVER&gt;#tuic-v5
</span></span></code></div></div></pre>

> **提示**
> 
> * VMess 采用 `ws + path=/vm`；
> * Hysteria2-OBFS：`obfs=salamander`，`alpn=h3`；
> * TUIC v5：默认 `insecure=1`，便于客户端快速导入（可自行改为严格证书校验）。

---

## 🔧 端口放行（云防火墙）

脚本会自动尝试使用 `ufw / firewalld / iptables` 放行本机端口。若你的云提供商​**额外有“安全组/云防火墙”**​，请把**下方命令打印出来的端口**放行到公网：

<pre class="overflow-visible!" data-start="4225" data-end="4748"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>echo</span><span> </span><span>&#34;=== 必须放行到云防火墙的端口 ===&#34;</span><span>
</span><span>echo</span><span> </span><span>&#34;[TCP]&#34;</span><span>
jq -r </span><span>&#39;.inbounds[]|[.listen_port, (if .type|test(&#34;hysteria2|tuic&#34;) then &#34;&#34; else &#34;tcp&#34; end)]|@tsv&#39;</span><span> /opt/sing-box/config.json \
| awk -F</span><span>&#39;\t&#39;</span><span> </span><span>&#39;$2==&#34;tcp&#34;{print $1}&#39;</span><span> | </span><span>sort</span><span> -n | </span><span>uniq</span><span> | </span><span>paste</span><span> -sd</span><span>&#39;,&#39;</span><span> -
</span><span>echo</span><span> </span><span>&#34;[UDP]&#34;</span><span>
jq -r </span><span>&#39;.inbounds[]|[.listen_port, (if .type|test(&#34;hysteria2|tuic&#34;) then &#34;udp&#34; else (if .type==&#34;shadowsocks&#34; then &#34;both&#34; else &#34;&#34; end) end)]|@tsv&#39;</span><span> /opt/sing-box/config.json \
| awk -F</span><span>&#39;\t&#39;</span><span> </span><span>&#39;$2==&#34;udp&#34;{print $1} $2==&#34;both&#34;{print $1}&#39;</span><span> | </span><span>sort</span><span> -n | </span><span>uniq</span><span> | </span><span>paste</span><span> -sd</span><span>&#39;,&#39;</span><span> -
</span></span></code></div></div></pre>

---

## 🛠 常见问题（FAQ）

### 1）WARP 报错：`illegal base64 data at input byte 40`

​**原因​**​：wgcf profile 中 `PublicKey/PrivateKey/Reserved` 含引号/回车/空格或缺失。
​**脚本处理**​：自动​**去引号/去 CR/去空格**​，Reserved 缺失回退 `0,0,0`。
​**仍有旧坏值**​？可一键重置：

<pre class="overflow-visible!" data-start="4965" data-end="5097"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>rm</span><span> -f /opt/sing-box/warp.env
</span><span>rm</span><span> -f /opt/sing-box/wgcf/wgcf-profile.conf   </span><span># 可选</span><span>
bash sing-box-plus.sh     </span><span># 重新选择 1) 安装/部署</span><span>
</span></span></code></div></div></pre>

### 2）更换端口后节点无法使用

* 请先确认**云防火墙**已放行新端口（见上节命令）
* 执行：

<pre class="overflow-visible!" data-start="5153" data-end="5251"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>ss -lntup | grep -E </span><span>&#39;sing-box|LISTEN&#39;</span><span>
journalctl -u sing-box.service --no-pager -n 100
</span></span></code></div></div></pre>

若日志中出现 `bind: address already in use`，说明新端口与其他进程冲突 → 再次 `4) 一键更换所有端口`。

### 3）菜单“更换端口”需要按两次回车

已在 v2.1.6 内修复：现在**一次回车**即可返回主菜单。

### 4）`curl: (22) 404` 下载 sing-box 失败

* 多因 GitHub API 变更或网络不可达；脚本内已做架构/版本回退逻辑。
* 可稍后重试或手动上传二进制到 `/usr/local/bin/sing-box` 并赋权 `0755`。

### 5）“legacy wireguard outbound is deprecated” 的警告

* 来自 sing-box 1.12.x 的​**提示**​，不影响当前用法；后续脚本会升级到新版 endpoint 结构。

---

## 🧹 卸载

在菜单选择 `9) 卸载`。若需​**彻底清理**​：

<pre class="overflow-visible!" data-start="5672" data-end="5868"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>systemctl stop sing-box.service
systemctl </span><span>disable</span><span> sing-box.service
</span><span>rm</span><span> -f /etc/systemd/system/sing-box.service
systemctl daemon-reload
</span><span>rm</span><span> -rf /opt/sing-box
</span><span>rm</span><span> -f /usr/local/bin/sing-box
</span></span></code></div></div></pre>

---

## 🌍 SOCKS5 落地 IP / 链式出口

启用后数据路径为：

```text
客户端 -> 入口 VPS（现有 18 个入站节点）-> 落地 SOCKS5 代理 -> 目标网站
```

适用前提：

* 落地服务器上已经运行 SOCKS5 代理，并允许入口 VPS 访问。
* 落地机及云安全组已放行对应代理端口。
* 建议在落地侧仅允许入口 VPS 的公网 IP 连接，避免代理端口裸露给全网。
* 脚本只负责把目标流量转发到落地代理；不会自动在裸 VPS 上创建代理服务。

菜单中的 `7) 配置/管理 SOCKS5 落地 IP` 支持：

1. 配置或修改 SOCKS5 落地代理
2. 测试 SOCKS5 落地代理实际出口 IP
3. 关闭 SOCKS5 落地出口，恢复原始路由

落地凭据保存在 `/opt/sing-box/landing.env`，文件权限会自动设为 `600`。

## 🛰 VPN Gate 落地（OpenVPN）

VPN Gate 提供的是 OpenVPN 配置，不是 SOCKS5 服务。脚本会把它包装成本地 SOCKS5 落地：

```text
客户端 -> 入口 VPS 的 sing-box -> 127.0.0.1:11080 本地 SOCKS5
       -> VPN Gate OpenVPN 隧道（独立 Linux 用户 + 策略路由）-> 目标网站
```

特点：

* 自动从 `https://www.vpngate.net/api/iphone/` 获取当前节点列表
* 支持按国家/地区代码或关键词选择，默认优先选择日本节点
* 只修改 `vpngate` 系统用户的策略路由，不影响入口 VPS 的 SSH 和系统默认路由
* VPN Gate 仅为公共免费节点，稳定性和隐私保障有限，不适合处理敏感流量
* 手动 SOCKS5 落地和 VPN Gate 落地两种菜单模式同时保留，配置时按需切换入口侧使用的出口

VPN Gate 相关文件：

| 路径 | 说明 |
| --- | --- |
| `/opt/sing-box/vpngate/current.ovpn` | 当前 VPN Gate OpenVPN 配置 |
| `/opt/sing-box/vpngate/sing-box.json` | 本地 SOCKS5 服务配置 |
| `/opt/sing-box/vpngate/route-up.sh` | 建立独立路由表和用户规则 |
| `/opt/sing-box/vpngate/route-down.sh` | 清理策略路由和 IPv6 泄漏防护 |

## ⚙️ 进阶：自定义（可选）

* `REALITY_SERVER` / `REALITY_SERVER_PORT` / `GRPC_SERVICE` / `VMESS_WS_PATH` 等可在 `/opt/sing-box/env.conf` 中修改，然后：

<pre class="overflow-visible!" data-start="6008" data-end="6066"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-bash"><span><span>bash sing-box-plus.sh   </span><span># 执行 3) 重启服务 或 1) 重新部署</span><span>
</span></span></code></div></div></pre>

* 修改证书（自签 → 正式证书）
  将你的 `fullchain.pem` / `key.pem` 放到 `/opt/sing-box/cert/` 并保持文件名一致，然后重启。

***

有问题可以[发帖](https://github.com/chengamki-tech/vps/issues)反馈，或者发邮件到海外邮箱rebeccalane27@gmail.com进行反馈。
