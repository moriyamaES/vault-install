# vault-install
Vaultのチュートリアル

## 参考

- 以下のサイトを参考にしている

    https://gammalab.net/blog/3f7pudgk4zbcr/

    - 公式サイトのインストール手順

    https://developer.hashicorp.com/vault/tutorials/getting-started/getting-started-install


## 環境

    ```
    # uname -a
    Linux control-plane.minikube.internal 6.4.12-1.el7.elrepo.x86_64 #1 SMP PREEMPT_DYNAMIC Wed Aug 23 13:48:54 EDT 2023 x86_64 x86_64 x86_64 GNU/Linux
    ```


# ConcourseCI の起動

- ConcourseCI を起動しておく

    ```
    # docker-compose up -d
    [+] Running 3/3
    ✔ Container concourse-install-db-1      Started                                                                                             0.0s 
    ✔ Container concourse-install-web-1     Started                                                                                             0.0s 
    ✔ Container concourse-install-worker-1  Started  
    ```

## インストール手順

- 以下の公式サイトを参考にした

    https://developer.hashicorp.com/vault/tutorials/getting-started/getting-started-install

- 以下のコマンドを実行

    ```
    sudo yum install -y yum-utils
    ```

    - 結果

        ```
        読み込んだプラグイン:fastestmirror, langpacks
        Loading mirror speeds from cached hostfile
        * base: ftp-srv2.kddilabs.jp
        * elrepo: ftp.yz.yamagata-u.ac.jp
        * extras: ftp-srv2.kddilabs.jp
        * updates: ftp-srv2.kddilabs.jp
        base                                                                                      | 3.6 kB  00:00:00     
        docker-ce-stable                                                                          | 3.5 kB  00:00:00     
        elrepo                                                                                    | 3.0 kB  00:00:00     
        extras                                                                                    | 2.9 kB  00:00:00     
        ius                                                                                       | 1.3 kB  00:00:00     
        updates                                                                                   | 2.9 kB  00:00:00     
        パッケージ yum-utils-1.1.31-54.el7_8.noarch はインストール済みか最新バージョンです
        何もしません
        ```

    ```
    sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
    ```

    - 結果

        ```
        読み込んだプラグイン:fastestmirror, langpacks
        adding repo from: https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
        grabbing file https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo to /etc/yum.repos.d/hashicorp.repo
        repo saved to /etc/yum.repos.d/hashicorp.repo
        ```

    ```
    sudo yum -y install vault
    ```

    - 結果

        ```
        読み込んだプラグイン:fastestmirror, langpacks
        Loading mirror speeds from cached hostfile
        * base: ftp-srv2.kddilabs.jp
        * elrepo: ftp.yz.yamagata-u.ac.jp
        * extras: ftp-srv2.kddilabs.jp
        * updates: ftp-srv2.kddilabs.jp
        hashicorp                                                                                                                  | 1.4 kB  00:00:00     
        hashicorp/7/x86_64/primary                                                                                                 | 178 kB  00:00:00     
        hashicorp                                                                                                                               1306/1306
        依存性の解決をしています
        --> トランザクションの確認を実行しています。
        ---> パッケージ vault.x86_64 0:1.14.2-1 を インストール
        --> 依存性解決を終了しました。

        依存性を解決しました

        ==================================================================================================================================================
        Package                         アーキテクチャー                 バージョン                            リポジトリー                         容量
        ==================================================================================================================================================
        インストール中:
        vault                           x86_64                           1.14.2-1                              hashicorp                           129 M

        トランザクションの要約
        ==================================================================================================================================================
        インストール  1 パッケージ

        総ダウンロード容量: 129 M
        インストール容量: 354 M
        Downloading packages:
        警告: /var/cache/yum/x86_64/7/hashicorp/packages/vault-1.14.2-1.x86_64.rpm: ヘッダー V4 RSA/SHA256 Signature、鍵 ID a621e701: NOKEY  00:00:00 ETA 
        vault-1.14.2-1.x86_64.rpm の公開鍵がインストールされていません
        vault-1.14.2-1.x86_64.rpm                                                                                                  | 129 MB  00:00:04     
        https://rpm.releases.hashicorp.com/gpg から鍵を取得中です。
        Importing GPG key 0xA621E701:
        Userid     : "HashiCorp Security (HashiCorp Package Signing) <security+packaging@hashicorp.com>"
        Fingerprint: 798a ec65 4e5c 1542 8c8e 42ee aa16 fcbc a621 e701
        From       : https://rpm.releases.hashicorp.com/gpg
        Running transaction check
        Running transaction test
        Transaction test succeeded
        Running transaction
        インストール中          : vault-1.14.2-1.x86_64                                                                                             1/1Generating Vault TLS key and self-signed certificate...
        Generating a 4096 bit RSA private key
        ...............................................++
        .............................................................................................................................................................................++
        writing new private key to 'tls.key'
        -----
        Vault TLS key and self-signed certificate have been generated in '/opt/vault/tls'.
        検証中                  : vault-1.14.2-1.x86_64                                                                                             1/1 

        インストール:
        vault.x86_64 0:1.14.2-1                                                                                                                         

        完了しました!
        ```


- インストールの検証

    ```
    # vault
    ```

    - 結果

        ```
        Usage: vault <command> [args]

        Common commands:
            read        Read data and retrieves secrets
            write       Write data, configuration, and secrets
            delete      Delete secrets and configuration
            list        List data or secrets
            login       Authenticate locally
            agent       Start a Vault agent
            server      Start a Vault server
            status      Print seal and HA status
            unwrap      Unwrap a wrapped secret

        Other commands:
            audit                Interact with audit devices
            auth                 Interact with auth methods
            debug                Runs the debug command
            events               
            kv                   Interact with Vault's Key-Value storage
            lease                Interact with leases
            monitor              Stream log messages from a Vault server
            namespace            Interact with namespaces
            operator             Perform operator-specific tasks
            patch                Patch data, configuration, and secrets
            path-help            Retrieve API help for paths
            pki                  Interact with Vault's PKI Secrets Engine
            plugin               Interact with Vault plugins and catalog
            policy               Interact with policies
            print                Prints runtime configurations
            proxy                Start a Vault Proxy
            secrets              Interact with secrets engines
            ssh                  Initiate an SSH session
            token                Interact with tokens
            transform            Interact with Vault's Transform Secrets Engine
            transit              Interact with Vault's Transit Secrets Engine
            version-history      Prints the version history of the target Vault server
        ```

    ```
    # which vault 
    /usr/bin/vault
    ```


    ```
    # vault -v
    Vault v1.14.2 (16a7033a0686eca50ee650880d5c55438d274489), built 2023-08-24T13:19:12Z
    ```

## vaultのコンフィグ

- 以下のサイトを参考にした

    https://gammalab.net/blog/3f7pudgk4zbcr/


- 以下のコマンドを実行
- コンフィグ`vault.hcl`は変更しない
- `ui = true`にしてると、8200番ポートでwebUIが開けます！やったね。

    ```
    # cat /etc/vault.d/vault.hcl 
    ```

    - 結果

        ```
        # Copyright (c) HashiCorp, Inc.
        # SPDX-License-Identifier: MPL-2.0

        # Full configuration options can be found at https://www.vaultproject.io/docs/configuration

        ui = true

        #mlock = true
        #disable_mlock = true

        storage "file" {
        path = "/opt/vault/data"
        }

        #storage "consul" {
        #  address = "127.0.0.1:8500"
        #  path    = "vault"
        #}

        # HTTP listener
        #listener "tcp" {
        #  address = "127.0.0.1:8200"
        #  tls_disable = 1
        #}

        # HTTPS listener
        listener "tcp" {
        address       = "0.0.0.0:8200"
        tls_cert_file = "/opt/vault/tls/tls.crt"
        tls_key_file  = "/opt/vault/tls/tls.key"
        }

        # Enterprise license_path
        # This will be required for enterprise as of v1.8
        #license_path = "/etc/vault.d/vault.hclic"

        # Example AWS KMS auto unseal
        #seal "awskms" {
        #  region = "us-east-1"
        #  kms_key_id = "REPLACE-ME"
        #}

        # Example HSM auto unseal
        #seal "pkcs11" {
        #  lib            = "/usr/vault/lib/libCryptoki2_64.so"
        #  slot           = "0"
        #  pin            = "AAAA-BBBB-CCCC-DDDD"
        #  key_label      = "vault-hsm-key"
        #  hmac_key_label = "vault-hsm-hmac-key"
        #}
        ```

## vaultのセットアップ

- 以下のサイトを参考にしている

    https://gammalab.net/blog/3f7pudgk4zbcr/


<del>

- 以下のコマンドを実行

- vaultコマンドに今どこのvaultを設定するのかってのを教えてやる。それは環境変数です

    ```
    # export VAULT_ADDR='http://127.0.0.1:8200'
    ```

- 最初に初期化を行う必要があります




    ```
    # vault operator init
    Get "http://127.0.0.1:8200/v1/sys/seal-status": dial tcp 127.0.0.1:8200: connect: connection refused
    ```

    - 原因調査
    - 原因は、vault サーバが起動していないこと


- vault を起動する。
- 参考サイトにはこのコマンドがない

    ```
    # vault server -config=/etc/vault.d/vault.hcl &
    ```

- 以下のエラーが発生した

    ```
    [root@control-plane ~/concourse-install (main)]
    # vault server -config=/etc/vault.d/vault.hcl &
    [1] 9143
    [root@control-plane ~/concourse-install (main)]
    # ==> Vault server configuration:

    Administrative Namespace: 
                        Cgo: disabled
    Environment Variables: BROWSER, COLORTERM, DBUS_SESSION_BUS_ADDRESS, GIT_ASKPASS, GODEBUG, HISTCONTROL, HISTSIZE, HOME, HOSTNAME, LANG, LESSOPEN, LOGNAME, LS_COLORS, MAIL, OLDPWD, PATH, PS1, PWD, SELINUX_LEVEL_REQUESTED, SELINUX_ROLE_REQUESTED, SELINUX_USE_CURRENT_RANGE, SHELL, SHLVL, SSH_CLIENT, SSH_CONNECTION, TERM, TERM_PROGRAM, TERM_PROGRAM_VERSION, USER, VAULT_ADDR, VSCODE_GIT_ASKPASS_EXTRA_ARGS, VSCODE_GIT_ASKPASS_MAIN, VSCODE_GIT_ASKPASS_NODE, VSCODE_GIT_IPC_HANDLE, VSCODE_IPC_HOOK_CLI, XDG_DATA_DIRS, XDG_RUNTIME_DIR, XDG_SESSION_ID, _
                Go Version: go1.20.7
                Listener 1: tcp (addr: "0.0.0.0:8200", cluster address: "0.0.0.0:8201", max_request_duration: "1m30s", max_request_size: "33554432", tls: "enabled")
                Log Level: 
                    Mlock: supported: true, enabled: true
            Recovery Mode: false
                    Storage: file
                    Version: Vault v1.14.2, built 2023-08-24T13:19:12Z
                Version Sha: 16a7033a0686eca50ee650880d5c55438d274489

    ==> Vault server started! Log data will stream in below:

    2023-09-03T17:39:11.468+0900 [INFO]  proxy environment: http_proxy="" https_proxy="" no_proxy=""
    2023-09-03T17:39:11.468+0900 [WARN]  no `api_addr` value specified in config or in VAULT_API_ADDR; falling back to detection if possible, but this value should be manually set
    2023-09-03T17:39:11.563+0900 [INFO]  core: Initializing version history cache for core
    ^C
    [root@control-plane ~/concourse-install (main)]
    # vault operator init
    Error making API request.

    URL: GET http://127.0.0.1:8200/v1/sys/seal-status
    Code: 400. Raw Message:

    Client sent an HTTP request to an HTTPS server.
    ```

- コンフィグを見ると、HTTPSで接続するようになってので、HTTPでの接続にする

- 変更内容は以下

    ```
    # diff /etc/vault.d/vault.hcl_org /etc/vault.d/vault.hcl
    21,26d20
    < #listener "tcp" {
    < #  address = "127.0.0.1:8200"
    < #  tls_disable = 1
    < #}
    < 
    < # HTTPS listener
    28,30c22,23
    <   address       = "0.0.0.0:8200"
    <   tls_cert_file = "/opt/vault/tls/tls.crt"
    <   tls_key_file  = "/opt/vault/tls/tls.key"
    ---
    >   address = "127.0.0.1:8200"
    >   tls_disable = 1
    31a25,31
    > 
    > # HTTPS listener
    > #listener "tcp" {
    > #  address       = "0.0.0.0:8200"
    > #  tls_cert_file = "/opt/vault/tls/tls.crt"
    > #  tls_key_file  = "/opt/vault/tls/tls.key"
    > #}
    ```

- vault を再起動

    ```
    # ps -a | grep vault
    9143 pts/3    00:00:01 vault
    ```

    ```
    # kill 9143
    ```

    ```
    # ps -a | grep vault | wc -l
    0
    ```

    ```
    # vault server -config=/etc/vault.d/vault.hcl &
    ```

    - 結果

        ```
        # ==> Vault server configuration:

        Administrative Namespace: 
                            Cgo: disabled
        Environment Variables: BROWSER, COLORTERM, DBUS_SESSION_BUS_ADDRESS, GIT_ASKPASS, GODEBUG, HISTCONTROL, HISTSIZE, HOME, HOSTNAME, LANG, LESSOPEN, LOGNAME, LS_COLORS, MAIL, PATH, PS1, PWD, SELINUX_LEVEL_REQUESTED, SELINUX_ROLE_REQUESTED, SELINUX_USE_CURRENT_RANGE, SHELL, SHLVL, SSH_CLIENT, SSH_CONNECTION, TERM, TERM_PROGRAM, TERM_PROGRAM_VERSION, USER, VSCODE_GIT_ASKPASS_EXTRA_ARGS, VSCODE_GIT_ASKPASS_MAIN, VSCODE_GIT_ASKPASS_NODE, VSCODE_GIT_IPC_HANDLE, VSCODE_IPC_HOOK_CLI, XDG_DATA_DIRS, XDG_RUNTIME_DIR, XDG_SESSION_ID, _
                    Go Version: go1.20.7
                    Listener 1: tcp (addr: "127.0.0.1:8200", cluster address: "127.0.0.1:8201", max_request_duration: "1m30s", max_request_size: "33554432", tls: "disabled")
                    Log Level: 
                        Mlock: supported: true, enabled: true
                Recovery Mode: false
                        Storage: file
                        Version: Vault v1.14.2, built 2023-08-24T13:19:12Z
                    Version Sha: 16a7033a0686eca50ee650880d5c55438d274489

        ==> Vault server started! Log data will stream in below:

        2023-09-03T17:59:58.106+0900 [INFO]  proxy environment: http_proxy="" https_proxy="" no_proxy=""
        2023-09-03T17:59:58.107+0900 [WARN]  no `api_addr` value specified in config or in VAULT_API_ADDR; falling back to detection if possible, but this value should be manually set
        2023-09-03T17:59:58.165+0900 [INFO]  core: Initializing version history cache for core
        ^C
        ```

- 以下のエラーがでた、

sudo netstat -tuln | grep 8200

# ss -atn


vault operator rekey -config=/etc/vault.d/vault.hcl


```
# vault operator init
Get "https://127.0.0.1:8200/v1/sys/seal-status": http: server gave HTTP response to HTTPS client
```

# cat /etc/vault.d/vault.hcl
# Copyright (c) HashiCorp, Inc.
# SPDX-License-Identifier: MPL-2.0

# Full configuration options can be found at https://www.vaultproject.io/docs/configuration

ui = true

#mlock = true
#disable_mlock = true

storage "file" {
  path = "/opt/vault/data"
}

#storage "consul" {
#  address = "127.0.0.1:8500"
#  path    = "vault"
#}

# HTTP listener
listener "tcp" {
  address = "127.0.0.1:8200"
  tls_disable = 1
}

# HTTPS listener
#listener "tcp" {
#  address       = "0.0.0.0:8200"
#  tls_cert_file = "/opt/vault/tls/tls.crt"
#  tls_key_file  = "/opt/vault/tls/tls.key"
#}

# Enterprise license_path
# This will be required for enterprise as of v1.8
#license_path = "/etc/vault.d/vault.hclic"

# Example AWS KMS auto unseal
#seal "awskms" {
#  region = "us-east-1"
#  kms_key_id = "REPLACE-ME"
#}

# Example HSM auto unseal
#seal "pkcs11" {
#  lib            = "/usr/vault/lib/libCryptoki2_64.so"
#  slot           = "0"
#  pin            = "AAAA-BBBB-CCCC-DDDD"
#  key_label      = "vault-hsm-key"
#  hmac_key_label = "vault-hsm-hmac-key"
#}
[root@control-plane ~/concourse-install (main)]
# systemctl status vault.service
● vault.service - "HashiCorp Vault - A tool for managing secrets"
   Loaded: loaded (/usr/lib/systemd/system/vault.service; disabled; vendor preset: disabled)
   Active: activating (auto-restart) (Result: exit-code) since 日 2023-09-03 18:19:36 JST; 147ms ago
     Docs: https://www.vaultproject.io/docs/
  Process: 6314 ExecStart=/usr/bin/vault server -config=/etc/vault.d/vault.hcl (code=exited, status=1/FAILURE)
 Main PID: 6314 (code=exited, status=1/FAILURE)

 9月 03 18:19:36 control-plane.minikube.internal systemd[1]: Failed to start "HashiCorp Vault - A tool for managing secrets".
 9月 03 18:19:36 control-plane.minikube.internal systemd[1]: Unit vault.service entered failed state.
 9月 03 18:19:36 control-plane.minikube.internal systemd[1]: vault.service failed.
[root@control-plane ~/concourse-install (main)]
# vault operator init
Get "https://127.0.0.1:8200/v1/sys/seal-status": http: server gave HTTP response to HTTPS client


vault operator init


- vault を初期化

    ```
    # vault operator init
    ```

    - 結果
    - この情報は超大事なので超大事に保管してください。
    - unseal keyが金庫そのものの凍結を解除するための鍵で、Root TokenがRootユーザー(ロール)としてログインんするのに必要な鍵です。


</del>


## vaultのセットアップ（HTTPSでやり直し）

<del>

```
# sudo netstat -tuln | grep 8200 | wc -l
```

```
# ss -atn | wc -l
```


- コンフィグはHTTPSとした

    ```
    # cat /etc/vault.d/vault.hcl
    # Copyright (c) HashiCorp, Inc.
    # SPDX-License-Identifier: MPL-2.0

    # Full configuration options can be found at https://www.vaultproject.io/docs/configuration

    ui = true

    #mlock = true
    #disable_mlock = true

    storage "file" {
    path = "/opt/vault/data"
    }

    #storage "consul" {
    #  address = "127.0.0.1:8500"
    #  path    = "vault"
    #}

    # HTTP listener
    #listener "tcp" {
    #  address = "127.0.0.1:8200"
    #  tls_disable = 1
    #}

    # HTTPS listener
    listener "tcp" {
    address       = "0.0.0.0:8200"
    tls_cert_file = "/opt/vault/tls/tls.crt"
    tls_key_file  = "/opt/vault/tls/tls.key"
    }

    # Enterprise license_path
    # This will be required for enterprise as of v1.8
    #license_path = "/etc/vault.d/vault.hclic"

    # Example AWS KMS auto unseal
    #seal "awskms" {
    #  region = "us-east-1"
    #  kms_key_id = "REPLACE-ME"
    #}

    # Example HSM auto unseal
    #seal "pkcs11" {
    #  lib            = "/usr/vault/lib/libCryptoki2_64.so"
    #  slot           = "0"
    #  pin            = "AAAA-BBBB-CCCC-DDDD"
    #  key_label      = "vault-hsm-key"
    #  hmac_key_label = "vault-hsm-hmac-key"
    #}
    ```


- vaultコマンドに今どこのvaultを設定するのかってのを教えてやる。それは環境変数です
- HTTPSにする

    ```
    # export VAULT_ADDR='https://127.0.0.1:8200'
    ```

    ```
    # echo $VAULT_ADDR 
    https://127.0.0.1:8200
    ```

- vault を起動する。
- 参考サイトにはこのコマンドがない

    ```
    # vault server -config=/etc/vault.d/vault.hcl &
    ```

    - 結果

        ```
        # ==> Vault server configuration:

        Administrative Namespace: 
                            Cgo: disabled
        Environment Variables: BROWSER, COLORTERM, DBUS_SESSION_BUS_ADDRESS, GIT_ASKPASS, GODEBUG, HISTCONTROL, HISTSIZE, HOME, HOSTNAME, LANG, LESSOPEN, LOGNAME, LS_COLORS, MAIL, PATH, PS1, PWD, SELINUX_LEVEL_REQUESTED, SELINUX_ROLE_REQUESTED, SELINUX_USE_CURRENT_RANGE, SHELL, SHLVL, SSH_CLIENT, SSH_CONNECTION, TERM, TERM_PROGRAM, TERM_PROGRAM_VERSION, USER, VAULT_ADDR, VSCODE_GIT_ASKPASS_EXTRA_ARGS, VSCODE_GIT_ASKPASS_MAIN, VSCODE_GIT_ASKPASS_NODE, VSCODE_GIT_IPC_HANDLE, VSCODE_IPC_HOOK_CLI, XDG_DATA_DIRS, XDG_RUNTIME_DIR, XDG_SESSION_ID, _
                    Go Version: go1.20.7
                    Listener 1: tcp (addr: "0.0.0.0:8200", cluster address: "0.0.0.0:8201", max_request_duration: "1m30s", max_request_size: "33554432", tls: "enabled")
                    Log Level: 
                        Mlock: supported: true, enabled: true
                Recovery Mode: false
                        Storage: file
                        Version: Vault v1.14.2, built 2023-08-24T13:19:12Z
                    Version Sha: 16a7033a0686eca50ee650880d5c55438d274489

        ==> Vault server started! Log data will stream in below:

        2023-09-03T18:55:07.586+0900 [INFO]  proxy environment: http_proxy="" https_proxy="" no_proxy=""
        2023-09-03T18:55:07.587+0900 [WARN]  no `api_addr` value specified in config or in VAULT_API_ADDR; falling back to detection if possible, but this value should be manually set
        2023-09-03T18:55:07.653+0900 [INFO]  core: Initializing version history cache for core
        ```


    ```
    # sudo netstat -tuln | grep 8200 | wc -l
    1
    ```

    ```
    #  ss -atn | grep 8200 | wc -l
    1
    ```

    ```
    # vault operator init
    ```

```
# vault operator init
Error making API request.

URL: GET http://127.0.0.1:8200/v1/sys/seal-status
Code: 400. Raw Message:

Client sent an HTTP request to an HTTPS server.
```


```
# ss -atn | grep 8200 | wc -l
```

</del>

## vaultのセットアップ


- 以下のコマンドを実行

    ```
    # sudo netstat -tuln | grep 8200 | wc -l
    ```

    ```
    # ss -atn | wc -l
    ```

- コンフィグはHTTPとした

    ```
    # cat /etc/vault.d/vault.hc1
    # Copyright (c) HashiCorp, Inc.
    # SPDX-License-Identifier: MPL-2.0

    # Full configuration options can be found at https://www.vaultproject.io/docs/configuration

    ui = true

    #mlock = true
    #disable_mlock = true

    storage "file" {
    path = "/opt/vault/data"
    }

    #storage "consul" {
    #  address = "127.0.0.1:8500"
    #  path    = "vault"
    #}

    # HTTP listener
    listener "tcp" {
    address = "127.0.0.1:8200"
    tls_disable = 1
    }

    # HTTPS listener
    #listener "tcp" {
    #  address       = "0.0.0.0:8200"
    #  tls_cert_file = "/opt/vault/tls/tls.crt"
    #  tls_key_file  = "/opt/vault/tls/tls.key"
    #}

    # Enterprise license_path
    # This will be required for enterprise as of v1.8
    #license_path = "/etc/vault.d/vault.hclic"

    # Example AWS KMS auto unseal
    #seal "awskms" {
    #  region = "us-east-1"
    #  kms_key_id = "REPLACE-ME"
    #}

    # Example HSM auto unseal
    #seal "pkcs11" {
    #  lib            = "/usr/vault/lib/libCryptoki2_64.so"
    #  slot           = "0"
    #  pin            = "AAAA-BBBB-CCCC-DDDD"
    #  key_label      = "vault-hsm-key"
    #  hmac_key_label = "vault-hsm-hmac-key"
    #}
    ```

- vaultコマンドに今どこのvaultを設定するのかってのを教えてやる。それは環境変数です
- HTTPSにする

    ```
    # export VAULT_ADDR='http://127.0.0.1:8200'
    ```

    ```
    # echo $VAULT_ADDR 
    http://127.0.0.1:8200
    ```

- vault を起動する。
- 参考サイトにはこのコマンドがない

    ```
    # systemctl start vault.service
    ```

    ```
    # systemctl status vault.service
    ```

    - 結果

        ```
        ● vault.service - "HashiCorp Vault - A tool for managing secrets"
        Loaded: loaded (/usr/lib/systemd/system/vault.service; disabled; vendor preset: disabled)
        Active: active (running) since 日 2023-09-03 19:33:25 JST; 6s ago
            Docs: https://www.vaultproject.io/docs/
        Main PID: 29151 (vault)
            Tasks: 7
        Memory: 95.1M
        CGroup: /system.slice/vault.service
                └─29151 /usr/bin/vault server -config=/etc/vault.d/vault.hcl

        9月 03 19:33:25 control-plane.minikube.internal vault[29151]: Log Level:
        9月 03 19:33:25 control-plane.minikube.internal vault[29151]: Mlock: supported: true, enabled: true
        9月 03 19:33:25 control-plane.minikube.internal vault[29151]: Recovery Mode: false
        9月 03 19:33:25 control-plane.minikube.internal vault[29151]: Storage: file
        9月 03 19:33:25 control-plane.minikube.internal vault[29151]: Version: Vault v1.14.2, built 2023-08-24T13:19:12Z
        9月 03 19:33:25 control-plane.minikube.internal vault[29151]: Version Sha: 16a7033a0686eca50ee650880d5c55438d274489
        9月 03 19:33:25 control-plane.minikube.internal vault[29151]: ==> Vault server started! Log data will stream in below:
        9月 03 19:33:25 control-plane.minikube.internal vault[29151]: 2023-09-03T19:33:25.210+0900 [INFO]  proxy environment: http_proxy="" https_proxy="" no_proxy=""
        9月 03 19:33:25 control-plane.minikube.internal vault[29151]: 2023-09-03T19:33:25.210+0900 [WARN]  no `api_addr` value specified in config or in VAULT_API_ADDR; falling back to detection if possible, ... manually set
        9月 03 19:33:25 control-plane.minikube.internal vault[29151]: 2023-09-03T19:33:25.273+0900 [INFO]  core: Initializing version history cache for core
        Hint: Some lines were ellipsized, use -l to show in full.
        ```


    ```
    # sudo netstat -tuln | grep 8200 | wc -l
    1
    ```

- vault を初期化

    ```
    # vault operator init
    ```

    - 結果
    - この情報は超大事なので超大事に保管してください。
    - unseal keyが金庫そのものの凍結を解除するための鍵で、Root TokenがRootユーザー(ロール)としてログインんするのに必要な鍵です。


        ```
        Unseal Key 1: Ysxczpo5T3U7BJGsTM6nhHT16czKAByS/MSerudhd1ui
        Unseal Key 2: 7Q7xsTyAy8TSp4ocewWI57341Ev3XzomgjPf8JIi9QHw
        Unseal Key 3: CFMWi+MFo0vHwIy6G+RLY7I8sizBf7HFucNyKqExOYUT
        Unseal Key 4: qzLPG5WxTqVq9eimN7PcotQQXNSLJL0jie8tJKv7RwCI
        Unseal Key 5: 8El/1a8miryHIrsXayUz6ZglAOjqS2K2fEj4/jbJLmmu

        Initial Root Token: hvs.Gw5QtpOjwbUilvS0T3s13s6u

        Vault initialized with 5 key shares and a key threshold of 3. Please securely
        distribute the key shares printed above. When the Vault is re-sealed,
        restarted, or stopped, you must supply at least 3 of these keys to unseal it
        before it can start servicing requests.

        Vault does not store the generated root key. Without at least 3 keys to
        reconstruct the root key, Vault will remain permanently sealed!

        It is possible to generate new unseal keys, provided you have a quorum of
        existing unseal keys shares. See "vault operator rekey" for more information.
        ```



