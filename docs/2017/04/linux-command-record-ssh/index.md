# Linux记录之ssh


## 名称
**ssh** — — OpenSSH SSH 客户端（远程登录程序）

## 概要
```
ssh [-1246AaCfGgKkMNnqsTtVvXxYy] [-b bind_address] [-c cipher_spec]
         [-D [bind_address:]port] [-E log_file] [-e escape_char]
         [-F configfile] [-I pkcs11] [-i identity_file]
         [-J [user@]host[:port]] [-L address] [-l login_name] [-m mac_spec]
         [-O ctl_cmd] [-o option] [-p port] [-Q query_option] [-R address]
         [-S ctl_path] [-W host:port] [-w local_tun[:remote_tun]]
         [user@]hostname [command]
```

## 描述
**ssh** (SSH客户端) 是一个登录到远程机器的程序，在远程机器上执行命令。它旨在通过不安全的网络在两个不受信任的主机之间提供安全的加密通信。X11连接，任意TCP端口和UNIX域套接字也可以通过安全通道转发。

**ssh**连接并登录到指定的**主机名**（具有可选的**用户名**）。用户必须使用以下几种方法之一向远程机器证明他/她的身份（见下文）。

如果指定了命令，它将在远程机器执行。

选项如下：

#####  -1    

> 强制**ssh**只尝试协议版本1.

##### -2

> 强制**ssh**只尝试协议版本2.

##### -4

> 强制**ssh**只使用IPv4.

##### -6

> 强制**ssh**只使用IPv6.

##### -A

> 启用认证代理连接的转发。这也可以在配置文件中的每个主机的基础上进行指定。

> 应该小心启用代理转发。能够绕过远程主机的文件权限的用户（对于代理的UNIX域套接字）可以通过转发的连接访问本地代理。 攻击者无法从代理获取密钥资料，但是他们可以对密钥执行操作，使其能够使用加载到代理中的身份进行身份验证。

##### -a

> 禁用认证代理连接的转发。

##### -b  

> bind_address

> 在本地机器上使用**bind_address**作为连接的源地址。 仅对具有多个地址的系统有用。

##### -C

> 请求压缩所有数据（包括stdin，stdout，stderr和转发的X11，TCP和UNIX域连接的数据）。 压缩算法与gzip使用的压缩算法相同，`level`可以由协议版本1的**CompressionLevel**选项控制。调制解调器线路和其他慢速连接需要压缩，但在快速网络上只会减慢。 默认值可以在配置文件中逐个主机设置; 请参阅**压缩**选项。

##### -c  

> cipher_spec

> 选择加密会话的密码规范。 协议版本1允许指定单个密码。 支持的值为`3des`，`blowfish`和`des`。 对于协议版本2，**cipher_spec**是以优先级顺序列出的以逗号分隔的密码列表。 有关详细信息，请参阅ssh_config中的**Ciphers**关键字。

##### -D  

> [bind_address:]port

> 指定本地`dynamic`应用级端口转发。 这可以通过分配一个套接字来侦听本地**端口**，可选地绑定到指定的**bind_address**。 无论何时连接到该端口，连接将通过安全通道转发，然后应用程序协议用于确定从远程机器连接到何处。 目前支持SOCKS4和SOCKS5协议，**ssh**将作为SOCKS服务器。 只有root可以转发特权端口。 动态端口转发也可以在配置文件中指定。

> 可以通过将地址括在方括号中来指定IPv6地址。 只有超级用户可以转发特权端口。 默认情况下，根据**GatewayPorts**设置绑定本地端口。 但是，可以使用显式的**bind_address**将连接绑定到特定的地址。 `localhost`的**bind_address**表示侦听端口仅限于本地使用，而空的地址或`*`表示端口应该从所有接口可用。

##### -E  log_file

> 将调试日志追加到**log_file**而不是标准错误。

##### -e  

> escape_char

> 使用pty（默认值：’〜’）设置会话的转义字符。转义字符只能在行的开始处被识别。 转义字符后跟一个点（’.’）关闭连接; 其次是control-Z挂起连接; 然后自己发送一次转义字符。 将字符设置为`none`可禁用任何转义，并使会话完全透明。

##### -F  

> configfile

> 指定一个替代每个用户的配置文件。 如果在命令行中给出了一个配置文件，系统范围的配置文件（**/etc/ssh/ssh_config**）将被忽略。 每个用户配置文件的默认值为**〜/.ssh/config**。

##### -f

> 请求**ssh**在命令执行之前转到后台。 如果**ssh**要求密码或密码短语，但用户在后台需要它时，这很有用。 这与 **-n** 参数相同。 在远程站点启动X11程序的推荐方法是像**ssh -f host xterm**。

> 如果**ExitOnForwardFailure**配置选项设置为`yes`，则以-f开头的客户端将在将其自身置于后台之前等待所有远程端口的转发成功建立。

##### -G

> 导致**ssh**在评估**主机**和**匹配**块并退出后打印其配置。

##### -g

> 允许远程主机连接到本地转发的端口。 如果在多路复用连接上使用，则必须在主进程上指定此选项。

##### -I  

> smartcard_device

> 指定**ssh**应使用的设备与用于存储用户私有RSA密钥的智能卡进行通信。此选项仅在对智能卡设备的支持进行编译时可用（默认为不支持）。

##### -i  

> identity_file

> 选择一个读取用于公钥认证的身份（私钥）的文件。协议版本1的默认路径为**〜/.ssh/identity**，协议版本号为**〜/.ssh/id_dsa**，**〜/.ssh/id_ecdsa**，**〜/.ssh/id_ed25519**和**〜/.ssh/id_rsa**。身份文件也可以在每个主机的基础上在配置文件中指定。可以有多个**-i**选项（在配置文件中指定多个标识）。如果**CertificateFile**指令没有明确指定任何证书，**ssh**还将尝试从通过将**-cert.pub**追加到身份文件名获取的文件名加载证书信息。

##### -J  [user@]host[:port]

> 首先通过与跳转**主机**建立**ssh**连接，然后从那里建立TCP转发到最终目的地，连接到目标主机。可以用逗号分隔指定多级跳跃。 这是指定**ProxyJump**配置指令的快捷方式。

##### -K

> 启用GSSAPI认证和转发（委派）GSSAPI凭据给服务器。

##### -k

> 禁止向服务器转发（委派）GSSAPI凭据。

##### -L  

> **[bind_address:]port:host:hostport**
> **[bind_address:]port:remote_socket**
> **local_socket:host:hostport**
> **local_socket:remote_socket**

> 指定与本地（客户端）主机上的给定TCP端口或Unix套接字的连接将转发到远程端的给定主机和端口或Unix套接字。这可以通过分配套接字来侦听本地的TCP**端口**，可选地绑定到指定的**bind_address**或Unix套接字。无论何时连接到本地端口或套接字，连接将通过安全通道转发，并从远程机器连接到**主机**端口**主机端口**或Unix套接字**remote_socket**。

> 端口转发也可以在配置文件中指定。 只有超级用户可以转发特权端口。 可以通过将地址括在方括号中来指定IPv6地址。

> 默认情况下，根据**GatewayPorts**设置绑定本地端口。 但是，可以使用显式的**bind_address**将连接绑定到特定的地址。`localhost`的**bind_address**表示侦听端口仅限于本地使用，而空的地址或`*`表示端口应该从所有接口可用。

##### -l  

> login_name

> 指定用户在远程机器上登录。 这也可以在每个主机的基础上在配置文件中指定。

##### -M

> 将**ssh**客户端置于`master`模式下进行连接共享。 多个**-M**选项将**ssh**置于`master`模式，在接受从属连接之前需要确认。 有关详细信息，请参阅ssh_config中的**ControlMaster**的说明。

##### -m  

> mac_spec

> 以逗号分隔的MAC（消息认证码）算法列表，以优先级顺序指定。 有关更多信息，请参阅**MACs**关键字。

##### -N

> 不要执行远程命令。 这对于转发端口是有用的。

##### -n

> 从**/dev/null**重定向stdin（实际上，阻止从stdin读取）。当ssh在后台运行时，必须使用它。 一个常见的技巧是使用它来在远程机器上运行X11程序。例如，**ssh -n shadows.cs.hut.fi emacs＆**将在shadows.cs.hut.fi上启动一个emacs，X11连接将通过加密通道自动转发。ssh程序将放在后台。 （如果ssh需要输入密码或口令，则此操作不起作用;另请参阅**-f**选项。）

##### -O  

> ctl_cmd

> 控制主动连接复用主控制程序。 当指定**-O**选项时，**ctl_cmd**参数被解释并传递给主进程。有效的命令是：`check`（检查主进程是否正在运行），`forward`（请求转发而不执行命令），`cancel`（取消转发），`exit` （主机退出）和`stop`（请求主机停止接受进一步的复用请求）。

##### -o  

> option

> 可用于以配置文件中使用的格式提供选项。 这对于指定没有单独的命令行标志的选项很有用。 有关下列选项的完整详细信息及其可能的值，请参阅ssh_config。
> 
* AddKeysToAgent
* AddressFamily
* BatchMode
* BindAddress
* CanonicalDomains
* CanonicalizeFallbackLocal
* CanonicalizeHostname
* CanonicalizeMaxDots
* CanonicalizePermittedCNAMEs
* CertificateFile
* ChallengeResponseAuthentication
* CheckHostIP
* Cipher
* Ciphers
* ClearAllForwardings
* Compression
* CompressionLevel
* ConnectionAttempts
* ConnectTimeout
* ControlMaster
* ControlPath
* ControlPersist
* DynamicForward
* EscapeChar
* ExitOnForwardFailure
* FingerprintHash
* ForwardAgent
* ForwardX11
* ForwardX11Timeout
* ForwardX11Trusted
* GatewayPorts
* GlobalKnownHostsFile
* GSSAPIAuthentication
* GSSAPIDelegateCredentials
* HashKnownHosts
* Host
* HostbasedAuthentication
* HostbasedKeyTypes
* HostKeyAlgorithms
* HostKeyAlias
* HostName
* IdentitiesOnly
* IdentityAgent
* IdentityFile
* Include
* IPQoS
* KbdInteractiveAuthentication
* KbdInteractiveDevices
* KexAlgorithms
* LocalCommand
* LocalForward
* LogLevel
* MACs
* Match
* NoHostAuthenticationForLocalhost
* NumberOfPasswordPrompts
* PasswordAuthentication
* PermitLocalCommand
* PKCS11Provider
* Port
* PreferredAuthentications
* Protocol
* ProxyCommand
* ProxyJump
* ProxyUseFdpass
* PubkeyAcceptedKeyTypes
* PubkeyAuthentication
* RekeyLimit
* RemoteForward
* RequestTTY
* RhostsRSAAuthentication
* RSAAuthentication
* SendEnv
* ServerAliveInterval
* ServerAliveCountMax
* StreamLocalBindMask
* StreamLocalBindUnlink
* StrictHostKeyChecking
* TCPKeepAlive
* Tunnel
* TunnelDevice
* UpdateHostKeys
* UsePrivilegedPort
* User
* UserKnownHostsFile
* VerifyHostKeyDNS
* VisualHostKey
* XAuthLocation

##### -p  

> port

> 要连接到远程主机的端口。 这可以在配置文件中的每个主机的基础上进行指定。

##### -Q  

> query_option

> 用于指定版本2支持的算法的查询**ssh**。可用的功能包括：**cipher**（支持的对称密码），**cipher-auth**（支持认证加密的支持的对称密码），**mac**（支持的消息完整性代码），**kex**（密钥交换算法），**key**（密钥类型），**key-cert** （证书密钥类型），**key-plain**（非证书密钥类型）和**protocol-version**（支持的SSH协议版本）。

##### -q

> 静音模式。 大多数警告和诊断消息被抑制。

##### -R

> **[bind_address:]port:host:hostport**
> **[bind_address:]port:local_socket**
> **remote_socket:host:hostport**
> **remote_socket:local_socket**

> 指定远程（服务器）主机上的给定端口将转发到本地的给定主机和端口。这可以通过分配一个套接字来监听远程端口，每当连接到该端口时，通过安全通道转发连接，并从本地机器连接到主机端口主机端口。
> 
> 端口转发也可以在配置文件中指定。只有在远程机器上以root身份登录时，才能转发特权端口。可以通过将地址括在方括号中或使用其他语法来指定IPv6地址： [bind_address /]host/port/hostport。
> 
> 默认情况下，服务器上的监听套接字将仅绑定到环回接口。这可以通过指定一个bind_address来覆盖。空的bind_address或地址'*'表示远程套接字应该侦听所有接口。如果启用了服务器的**GatewayPorts**选项，则指定远程bind_address将成功（请参阅sshd_config）。
> 
> 如果端口参数为“0”，则监听端口将在服务器上动态分配，并在运行时向客户端报告。

##### -S

> ctl_path

> 指定用于连接共享的控制套接字的位置。有关详细信息，请参阅ssh_config中的**ControlPath**和**ControlMaster**的说明。

###### -s

> 可用于请求在远程系统上调用子系统。子系统是SSH2协议的一个功能，它有助于将SSH用作其他应用程序的安全传输（例如sftp）。子系统被指定为远程命令。

##### -T

> 禁用伪tty分配。

##### -t

> 强制伪tty分配。这可以用于在远程机器上执行任意基于屏幕的程序，这可以是非常有用的，例如，实现菜单服务时。多个**-t**选项强制tty分配，即使**ssh**没有本地tty。

##### -V

> 显示版本号并退出。

##### -v

> 详细模式。导致**ssh**打印关于其进度的调试消息。这有助于调试连接，身份验证和配置问题。多个**-v**选项增加了详细程度。最大为3。

##### -W

> host:port

> 要求将客户端上的标准输入和输出转发到通过安全通道的端口上的主机。暗示**-N**，**-T**，**ExitOnForwardFailure**和**ClearAllForwardings**，尽管这些可以在配置文件中或使用**-o**命令行选项来覆盖。仅适用于协议版本2。

##### -w

> local_tun[:remote_tun]

> 请求在客户端（local_tun）和服务器（remote_tun）之间指定的**tun**设备的隧道设备转发。
> 
> 设备可以用数字ID或关键字“any”来指定，该关键字使用下一个可用的隧道设备。如果未指定remote_tun，则默认为“any”。另请参见ssh_config中的**Tunnel**和**TunnelDevice**指令。如果**Tunnel**指令未设置，则将其设置为“点对点”的默认隧道模式。

##### -X

> 启用X11转发。这也可以在配置文件中的每个主机的基础上进行指定。
> 
> X11转发应小心启用。能够绕过远程主机的文件权限的用户（对于用户的X授权数据库）可以通过转发的连接访问本地X11显示。然后，攻击者可以执行诸如按键监视的活动。
> 
> 因此，默认情况下，X11转发将受到X11 SECURITY扩展限制。有关详细信息，请参阅**ssh -Y**选项和ssh_config中的**ForwardX11Trusted**指令。

##### -x

> 禁用X11转发。

##### -Y

> 启用可信X11转发。受信任的X11转发不受X11 SECURITY扩展控制。

##### -y

> 使用syslog系统模块发送日志信息。默认情况下，此信息发送给stderr。
> 
> **ssh**还可以从每用户配置文件和系统范围的配置文件中获取配置数据。 ssh_config中描述了文件格式和配置选项。
> 
> **ssh**以远程命令的退出状态退出，如果发生错误则退出255。

## 验证

OpenSSH SSH客户端支持SSH协议1和2.默认是仅使用协议2，尽管可以通过ssh_config中的**Protocol**选项或**-1**和**-2**选项（见上文）来更改。 不过协议1不应该被使用，只用于支持旧设备。 它存在许多加密缺陷，并且不支持协议2可用的许多高级功能。

可用于认证的方法是：基于GSSAPI的身份验证，基于主机的身份验证，公钥认证，质询 - 响应认证和密码认证。 验证方法按照上面指定的顺序进行尝试，但可以使用**PreferredAuthentications**更改默认顺序。

基于主机的身份验证的工作原理如下：如果用户登录的机器在远程机器上的**/etc/hosts.equiv**或**/etc/shosts.equiv**中列出，并且用户名在两侧都相同，或者 文件**〜/.rhosts**或**〜/.shosts**存在于远程计算机上的用户主目录中，并包含一行包含客户端计算机的名称和该计算机上用户的名称，该用户被认为是登录。 另外，服务器**必须**能够验证客户端的主机密钥（请参阅下面的**/etc/ssh/ssh_known_hosts**和**〜/.ssh/known_hosts**的描述）以供登录才能被允许。 此认证方法由于IP欺骗，DNS欺骗和路由欺骗而关闭安全漏洞。 [管理员的注意事项：**/etc/hosts.equiv**，**〜/.rhosts**和rlogin/rsh协议一般来说，本质上是不安全的，如果需要安全性，则应该被禁用。]

公开密钥认证的工作原理如下：该方案基于公钥加密，使用密码系统，使用单独的密钥完成加密和解密，从加密密钥中导出解密密钥是不可行的。 这个想法是每个用户创建一个公钥/私钥对用于认证。 服务器知道公钥，只有用户知道私钥。 **ssh**使用DSA，ECDSA，Ed25519或RSA算法之一自动实现公钥认证协议。 ssl的HISTORY部分简要讨论了DSA和RSA算法。

文件**〜/.ssh/authorized_keys**列出允许登录的公钥。当用户登录时，**ssh**程序会告知服务器要使用哪个密钥对进行身份验证。 客户端证明它可以访问私钥，并且服务器检查相应的公钥是否被授权接受该帐户。

用户通过运行ssh-keygen创建他/她的密钥对。 这将私钥存储在**〜/.ssh/identity**（协议1），**〜/.ssh/id_dsa**（DSA），**〜/.ssh/id_ecdsa**（ECDSA），**〜/.ssh/id_ed25519**（Ed25519）或**〜/.ssh/id_rsa**（RSA），并将公钥存储在**〜/.ssh/identity.pub**（协议1），**〜/.ssh/id_dsa.pub**（DSA），**〜/.ssh/id_ecdsa.pub**（ECDSA） **〜/.ssh/id_ed25519.pub**（Ed25519）或**〜/.ssh/id_rsa.pub**（RSA）在用户的主目录中。 然后，用户应该将公钥复制到远程机器上的主目录中的**〜/.ssh/authorized_keys**。 **authorized_keys**文件对应于传统的**〜/.rhosts**文件，每行有一个键，尽管行可以很长。 此后，用户可以登录而不提供密码。

公共密钥认证的变体以证书身份验证的形式提供：不使用一组公钥/私钥，使用签名证书。 这具有以下优点：可以使用单个可信赖的证书颁发机构来代替许多公钥/私钥。 有关详细信息，请参阅ssh-keygen的CERTIFICATES部分。

使用公钥或证书认证的最方便的方法可以是认证代理。 有关详细信息，请参阅ssh-agent和（可选）ssh_config中的**AddKeysToAgent**指令。

挑战响应认证的工作原理如下：服务器发送任意的“挑战”文本，并提示响应。 挑战响应认证的示例包括BSD认证（参见login.conf）和PAM（一些非OpenBSD系统）。

最后，如果其他身份验证方法失败，**ssh**将提示用户输入密码。 密码发送到远程主机进行检查; 然而，由于所有通信都被加密，所以在网络上侦听的人不能看到密码。

**ssh**会自动维护和检查包含所有主机所使用的标识的数据库。 主机密钥存储在用户主目录中的**〜/.ssh/known_hosts**中。 此外，文件**/etc/ssh/ssh\_known\_hosts**会自动检查已知主机。 任何新的主机都将自动添加到用户的文件中。 如果主机的识别发生变化，**ssh**会对此进行警告，并禁用密码身份验证，以防止服务器欺骗或中间人攻击，否则可能会用来规避加密。 **StrictHostKeyChecking**选项可用于控制对主机密钥未知或已更改的计算机的登录。

当用户的身份被服务器接受时，服务器可以在非交互会话中执行给定的命令，或者如果没有指定命令，则登录到机器中，并将用户作为交互式会话给出正常的外壳。 与远程命令或shell的所有通信将被自动加密。

如果请求了一个交互式会话，**ssh**默认情况下只会在客户端具有一个交互会话时请求伪终端（pty）。 标志**-T**和**-t**可用于覆盖此行为。

如果已经分配了伪终端，则用户可以使用下面指出的转义字符。

如果没有分配伪终端，会话是透明的，可以用于可靠地传输二进制数据。 在大多数系统上，将转义字符设置为“none”也将使会话透明，即使使用tty。

当远程机器上的命令或shell退出并且所有X11和TCP连接都已关闭时，会话终止。

## 转义字符

当请求伪终端时，**ssh**通过使用转义字符支持多个功能。

单个波浪号字符可以以~~或通过除了下面描述的字符之外的字符跟随波浪号来发送。 转义字符必须始终按照换行符被解释为特殊的。 可以使用**EscapeChar**配置指令在命令行中使用**-e**选项来更改配置文件中的转义字符。

支持的转义（假设默认为`〜'）为：

##### ~.

> 断开

##### ~\^Z

> **ssh**背景

##### ~#

> 列出转发的连接。

##### ~&

> 在等待转发连接/ X11会话终止时，注销时出现背景信息。

##### ~?

> 显示转义字符列表。

##### ~B

> 发送一个BREAK到远程系统（只有当对端支持它时才有用）。

##### ~C

> 打开命令行。 目前，这允许使用**-L**，**-R**和**-D**选项添加端口转发（见上文）。它还允许使用-**KL [bind_address:]port**取消现有端口转发，用于远程的**-KR [bind_address:]port**和用于动态端口转发的**-KD [bind_address:]port**。 **!command**允许用户在ssh_config中启用**PermitLocalCommand**选项时执行本地命令。 使用**-h**选项提供基本帮助。

##### ~R

> 请求重新连接连接（仅在对端支持时才有用）。

##### ~V

> 当错误被写入stderr时，减小详细程度（**LogLevel**）。

##### ~v

> 当错误被写入stderr时，增加详细程度（**LogLevel**）。

## TCP转发

可以在命令行或配置文件中指定通过安全通道转发任意TCP连接。 TCP转发的一个可能应用是与邮件服务器的安全连接; 另一个是通过防火墙。

在下面的例子中，我们将介绍加密IRC客户端和服务器之间的通信，即使IRC服务器不直接支持加密通信。 这样做如下：用户使用**ssh**连接到远程主机，指定用于将连接转发到远程服务器的端口。 之后，可以启动要在客户机上加密的服务，连接到同一本地端口，**ssh**将加密和转发连接。

以下示例从客户机隧道传输IRC会话”127.0.0.1”（localhost）到远程服务器”server.example.com”：

```
$ ssh -f -L 1234:localhost:6667 server.example.com sleep 10
$ irc -c '#users' -p 1234 pinky 127.0.0.1
```

这个隧道连接到IRC服务器”server.example.com”，使用端口1234加入通道”#users”，昵称”pinky”。它不使用哪个端口，例如只要它大于1023（请记住，只有root可以在特权端口上打开套接字），并且不会与已在使用的任何端口冲突。 连接转发到远程服务器上的端口6667，因为这是IRC服务的标准端口。

指定**-f**选项背景**ssh**和远程命令”sleep 10”，以允许一段时间（在本例中为10秒）启动要被隧道传输的服务。 如果在指定的时间内没有连接，则ssh将退出。
