# 沈硕

给不可信代码一个短生命周期的执行环境。限制 CPU、内存、进程、网络和权限，跑完不留实例，把创建、执行、销毁的延迟分开记下来。

中山大学 · 信息管理与信息系统 · 2028 届  
华为 2012 实验室 · 代码执行沙箱 · 2026.07–09 · 杭州

## 做成过的事

鲲鹏 ARM64 + openEuler，无外网。业务要一个可隔离、可压测的代码执行环境。

评估 E2B 的隔离、生命周期和执行接口后，确认离线 ARM 上依赖和内核都撑不住交付，切换到 Cube-Sandbox（v0.5.1-arm64）。离线打好包，打通运行时、TAP 网络池、权限，以及 CPU、内存、磁盘、网络配额和超时。做了 OLK-6.6 内核适配，调过 Zswap 和 KVM 参数。

压测脚本覆盖创建、销毁、代码执行，梯度 50 / 500。定位网络池耗尽、实例残留、回收不及时和 408 / 500，修完后留下性能基线。

旁边交过一个公有知识库：DeepSeek-Harness 上管理员投放、普通用户只读，提问前检索，索引可热更新，补了单测并提交 PR。检索是沙箱的配套，不是主线。

## 公开对照

实习仓库在内网，不能开源。[sandbench](https://github.com/shenshuo-maker/sandbench) 是同一类问题的公开实现：Docker + cgroup，`cap-drop ALL`、非 root、只读根、`no-new-privileges`，按 timeout / OOM / error 分类，压完按 label 扫残留。

本机（macOS arm64，Docker Desktop 8 CPU / 3.8 GiB）负载 `echo`，并发 1 → 50，各 50 次：成功率 100%，残留 0，创建 p95 从 264 ms 到 10.6 s。瓶颈在 dockerd 同时创建，不在命令本身。没有 gVisor、Kata、Firecracker，也没有排队调度。

## 范围

Linux 隔离与配额、Docker、离线交付、并发压测、失败分类。Python / Shell 写运行和压测，TypeScript 写过检索那次 PR。

## 其他

[algorithm-check-system](https://github.com/shenshuo-maker/algorithm-check-system)：算法备案文本合规检测，省级大创。BERT 微调、法规图谱、FastAPI。  
华数杯数学建模国家二等奖。

## 联系

abc300739@qq.com · 19731188248
