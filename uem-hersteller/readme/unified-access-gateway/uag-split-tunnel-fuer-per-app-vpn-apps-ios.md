# UAG - Split Tunnel für per App VPN-Apps (iOS)

Standardmäßig ist ein per App VPNTunnel für iOS über das UAG ein Full Tunnel für die App. Möchte man das nicht aller Traffic durch den Tunnel geht, kann man hierfür Ausnahmen definieren.&#x20;

Dies geht hier: All Settings –> System –> Enterprise Integration –> VMware Tunnel –> Network Traffic Rules

Dort können unter „Device Traffic Rules“ Ausnahmen für einzelne Apps definiert werden.  Hierfür die Option „bypass“ verwenden.

Hier können Wildcards definiert werden oder komplette URls angegeben werden.&#x20;

Will man z.B. die Onedrive App so Tunneln, dass der private Traffic des privaten Onedrive Kontos direkt ins Internet geht, müssen folgende Wildcards verwendet werden. &#x20;

> \*io,
>
> \*ms,
>
> \*com,
>
> \*net

mehere URLs können mir Komma getrennt werden.&#x20;
