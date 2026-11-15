# 我的作品与访问统计

## 登录与查询

复用当前轻页设备凭证。首次发布会创建匿名身份，匿名设备也能管理自己作品；微信绑定后可在其他设备或网页恢复同一身份。没有凭证时说明需要 `python3 "/实际skill目录/scripts/litepage.py" login --wechat` 并按[账号与授权](account.md)展示二维码，由用户确认；查询请求本身不创建匿名账号。已有凭证失效时不能另建匿名身份绕过。

```bash
python3 "/实际skill目录/scripts/litepage.py" list --limit 10
python3 "/实际skill目录/scripts/litepage.py" list --limit 10 --cursor "上次返回的next_cursor"
python3 "/实际skill目录/scripts/litepage.py" status 作品UUID
python3 "/实际skill目录/scripts/litepage.py" usage
```

`list` 只返回当前身份的作品，包括下架或停用的管理记录；默认 10 条，最多 50 条，`next_cursor=null` 为末页。按用户需要翻页，不默认遍历全部作品。展示标题、`site_id`、状态和真实链接；管理命令使用完整 UUID，不能把公开短码当成 UUID。用户只给作品链接或标题时，在本人列表中定位；同名或目标不明确时先澄清，再执行写操作。

`status` 查看指定作品当前状态、revision、版本、可见性和 URL。`usage` 查看上传大小、文件数、存储及平台预算等资源信息，不能当成 PV/UV；字段为 null 表示该项无固定限额，不等于无限资源。每账号作品数与每作品版本数不限；下架保留作品记录。准确限制使用当前服务响应。

## 查看访问数据

```bash
python3 "/实际skill目录/scripts/litepage.py" analytics --days 7
python3 "/实际skill目录/scripts/litepage.py" analytics 作品UUID --days 1
python3 "/实际skill目录/scripts/litepage.py" analytics 作品UUID --days 30
```

不传 UUID 查询当前身份全部作品汇总（含下架、停用作品），不受列表分页影响；传 UUID 只查询本人指定作品。默认近 7 个北京时间自然日，`--days` 为 1–30，1 表示今日。响应包含期间 PV/UV、日期明细、设备和浏览器分布；最多 30 天、4 个设备类别、9 个浏览器类别，不返回访客记录，不自动扩大窗口。

PV 是记录到的页面浏览次数；UV 是期间去重浏览器数，不是真实人数。全部作品 UV 跨作品去重，不能相加单个作品或每日 UV 代替汇总。统计从功能启用后积累，未记录的日期为零；拦截脚本、DNT/GPC 或浏览器存储限制会影响结果，设备与浏览器类别是估算。零数据不能声称功能故障，也不通过打开或刷新线上作品、发送事件来制造数据。服务不支持统计时报告能力缺口，不能拿 usage 或模拟数字代替。

网页入口：[轻页首页](https://lightpage.hqdx.store/) → “我的作品” → 微信登录。也可直接打开[个人管理页](https://lightpage.hqdx.store/manage/)。页面显示全部作品与单作品今日、近 7／30 天 PV、UV；“详细统计”展开趋势、每日明细和设备／浏览器分布。读取失败使用页面重试入口。

## 重设页面密码

用户要求为已有页面重设、启用或移除分享密码时，复用当前所有者凭证；不需要旧分享密码，也不重新上传 HTML。用户只给链接或标题时按上面的本人列表定位作品 UUID，目标不明确时先澄清。新密码由用户指定；若未提供，也未授权生成，先询问所需密码。

将新密码放进私有目录的 UTF-8 文件（4–128 字符，末尾换行会移除），不要放在作品上传目录、命令字面量或分享 URL 中。使用完整安装包入口：

```bash
python3 "/实际skill目录/scripts/litepage.py" password 作品UUID --password-file "/私有目录/new-password.txt"
# 用户明确要求取消密码时：
python3 "/实际skill目录/scripts/litepage.py" password 作品UUID --remove
```

密码文件为空（或只有末尾换行）同样表示取消密码；仅重设命令接受这一写法，发布／更新时空密码文件会报错，省略密码选项的普通更新仍保留原密码。管理页也可将输入清空后点击“取消密码”。

命令先读取该作品当前 revision，再以版本前提提交修改；已有最新版本时可显式传 --revision 版本号省去读取。409 时重新读取 status 并核对当前改动，不自动覆盖；无所有者凭证、404 或失败时不能新建匿名身份、重新发布或猜测成功。

成功结果 password_protected=true 表示已设密，移除时为 false；核对 site_id 和实际 url，重设后交付“密码已重设，原链接不变，旧浏览器授权已失效”；移除后明确说明“密码已移除”。不在输出中回显密码；只按用户指定的私密渠道提供。移除后持有链接的人可访问，作品是否回到首页由 visibility 决定。保护边界见[密码分享](publishing.md#密码分享)。

## 管理操作与授权

登录只确定身份，不自动授权修改作品。用户要求更新时使用原 UUID 的 `update`，无需另建作品；修改可见性或下架前读取最新 status，核对标题、ID 和 revision 后提交。409 并发冲突时重新核对，不盲目重试覆盖。下架成功后说明链接停止提供内容、管理记录与历史统计保留；不自动下架其他作品。

退出使用 `python3 "/实际skill目录/scripts/litepage.py" logout`，只撤销当前 CLI 设备并删除本地凭证，不删除作品；网页退出管理自己的浏览器设备。
