看资料中无意间看到的，感觉最近挺需要这个的，故记录下来：

下面两条指令首先将防火墙的当前规则保存，如果因为规则混乱无法上网了可以还原：

~~~

iptables-save > iptables.rules
iptables-restore < iptables.rules

~~~