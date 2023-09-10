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
- HTTPにする

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

## vault を初期化

- 以下のコマンドを実行

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

## vault unseal

- 以下のコマンドを実行（5つのUnseal Keyの内、3つのUnseal Keyを3回に分け入力）

    ```
    # vault operator unseal
    ```

-   Root Tokenを使ってログイン

    ```
    # vault login
    ```

## Concourse用のロールとtokenを作成

- 以下のコマンドを実行

    ```
    # pwd
    /root/vault-install
    ```

    ```
    # cat << EOF > concourse-policy.hcl
    path "concourse/*" {
            capabilities = ["read"]
    }   
    EOF
    ```

    ```
    # cat concourse-policy.hcl
    path "concourse/*" {
            capabilities = ["read"]
    } 
    ```

- そしたらそのファイルを読み込みます。

    ```
    # vault policy write concourse ./concourse-policy.hcl
    ```

    - 結果

        ```
        Success! Uploaded policy: concourse
        ```

        - これでconcourseポリシーを作ることができました。

- そしたらconcourseポリシーに基づいたtokenを発行します。

    ```
    # vault token create --policy concourse
    ```

    - 結果

        ```
        Key                  Value
        ---                  -----
        token                hvs.CAESII7rrevJiuWGuxuBao28rbs1OnmdLUMV2g05hdXd84uQGh4KHGh2cy5oeGwxeEZoUDFqUVBYU2JsMTRoc3pxSFg
        token_accessor       2g8wVMS41uLnyxDeqknv0kbn
        token_duration       768h
        token_renewable      true
        token_policies       ["concourse" "default"]
        identity_policies    []
        policies             ["concourse" "default"]
        ```

## concourseCIとvaultを接続

- 以下のサイトを参考にした

    https://gammalab.net/blog/3f7pudgk4zbcr/

    https://concourse-ci.org/vault-credential-manager.html

- 先程のトークンをconcourseCIに設定します！docker-composeファイルに以下を追記するだけです。
- To configure this, first configure the URL of your Vault server by setting the following env on the web node

    ```
    # cd ~/concourse-install
    ```

- concourseの`docker-compose.yml`（変更前）

    ```
    # cat docker-compose.yml 
    version: '3'

    services:
    db:
        image: postgres
        environment:
        POSTGRES_DB: concourse
        POSTGRES_USER: concourse_user
        POSTGRES_PASSWORD: concourse_pass
        logging:
        driver: "json-file"
        options:
            max-file: "5"
            max-size: "10m"

    web:
        image: concourse/concourse
        command: web
        links: [db]
        depends_on: [db]
        ports: ["8080:8080"]
        volumes: ["./keys/web:/concourse-keys"]
        environment:
        CONCOURSE_EXTERNAL_URL: http://localhost:8080
        CONCOURSE_POSTGRES_HOST: db
        CONCOURSE_POSTGRES_USER: concourse_user
        CONCOURSE_POSTGRES_PASSWORD: concourse_pass
        CONCOURSE_POSTGRES_DATABASE: concourse
        CONCOURSE_ADD_LOCAL_USER: test:test
        CONCOURSE_MAIN_TEAM_LOCAL_USER: test
        logging:
        driver: "json-file"
        options:
            max-file: "5"
            max-size: "10m"

    worker:
        image: concourse/concourse
        command: worker
        privileged: true
        depends_on: [web]
        volumes: ["./keys/worker:/concourse-keys"]
        links: [web]
        stop_signal: SIGUSR2
        environment:
        CONCOURSE_TSA_HOST: web:2222
        # enable DNS proxy to support Docker's 127.x.x.x DNS server
        CONCOURSE_GARDEN_DNS_PROXY_ENABLE: "true"
        logging:
        driver: "json-file"
        options:
            max-file: "5"
            max-size: "10m"
    ```

- concourseの`docker-compose.yml`（変更後）

    ```
    # cat docker-compose.yml 
    version: '3'

    services:
    db:
        image: postgres
        environment:
        POSTGRES_DB: concourse
        POSTGRES_USER: concourse_user
        POSTGRES_PASSWORD: concourse_pass
        logging:
        driver: "json-file"
        options:
            max-file: "5"
            max-size: "10m"

    web:
        image: concourse/concourse
        command: web
        links: [db]
        depends_on: [db]
        ports: ["8080:8080"]
        volumes: ["./keys/web:/concourse-keys"]
        environment:
        CONCOURSE_EXTERNAL_URL: http://localhost:8080
        CONCOURSE_POSTGRES_HOST: db
        CONCOURSE_POSTGRES_USER: concourse_user
        CONCOURSE_POSTGRES_PASSWORD: concourse_pass
        CONCOURSE_POSTGRES_DATABASE: concourse
        CONCOURSE_ADD_LOCAL_USER: test:test
        CONCOURSE_MAIN_TEAM_LOCAL_USER: test
        CONCOURSE_VAULT_URL: http://127.0.0.1:8200
        CONCOURSE_VAULT_CLIENT_TOKEN: hvs.CAESII7rrevJiuWGuxuBao28rbs1OnmdLUMV2g05hdXd84uQGh4KHGh2cy5oeGwxeEZoUDFqUVBYU2JsMTRoc3pxSFg
        logging:
        driver: "json-file"
        options:
            max-file: "5"
            max-size: "10m"

    worker:
        image: concourse/concourse
        command: worker
        privileged: true
        depends_on: [web]
        volumes: ["./keys/worker:/concourse-keys"]
        links: [web]
        stop_signal: SIGUSR2
        environment:
        CONCOURSE_TSA_HOST: web:2222
        # enable DNS proxy to support Docker's 127.x.x.x DNS server
        CONCOURSE_GARDEN_DNS_PROXY_ENABLE: "true"
        logging:
        driver: "json-file"
        options:
            max-file: "5"
            max-size: "10m"
    ```

- concourse を再起動

    ```
    # docker-compose down
    [+] Running 4/4
    ✔ Container concourse-install-worker-1  Removed                                                                   10.6s 
    ✔ Container concourse-install-web-1     Removed                                                                   10.3s 
    ✔ Container concourse-install-db-1      Removed                                                                    0.2s 
    ✔ Network concourse-install_default     Removed                                                                    0.1s 
    ```

    ```
    # docker-compose up -d
    [+] Running 4/4
    ✔ Network concourse-install_default     Created                                                                                              0.3s 
    ✔ Container concourse-install-db-1      Started                                                                                              0.1s 
    ✔ Container concourse-install-web-1     Started                                                                                              0.1s 
    ✔ Container concourse-install-worker-1  Started  
    ```

- ここでVMのチェックポイントを作成

## Concourse用のシークレットを作成

- 以下のコマンドを実行

    ```
    # vault secrets enable -version=1 -path concourse kv
    ```

    - 結果

        ```
        Success! Enabled the kv secrets engine at: concourse/
        ```

    - これでconcourse/以下に自由にkv型のデータを配置できるようになりました。

- 参考にしているサイト

    https://gammalab.net/blog/3f7pudgk4zbcr/

- のサンプルがよくわからないので、concourse の日本語チュートリアルでハンズオンを行ってみる
- 参考にしたサイトは以下（Docker イメージの作成・利用）

    https://concoursetutorial-ja.site.lkj.io/miscellaneous/docker-images


## Docker イメージの作成・利用（Vault版）

- 参考にしたサイトは以下（Docker イメージの作成・利用）

    https://concoursetutorial-ja.site.lkj.io/miscellaneous/docker-images

- 上記サイトの内容をVault で実装してみる

- 以下のコマンドを実行し、`pipeline.yml`の内容を確認

    ```
    # cd ~/concourse-tutorial/tutorials/miscellaneous/docker-images/
    ```

    ```
    # cat pipeline.yml 
    ---
    resources:
    - name: tutorial
        type: git
        source:
        uri: https://github.com/drnic/concourse-tutorial.git
        branch: develop

    - name: hello-world-docker-image
        type: docker-image
        source:
        email: ((docker-hub-email))
        username: ((docker-hub-username))
        password: ((docker-hub-password))
        repository: ((docker-hub-username))/concourse-tutorial-hello-world

    jobs:
    - name: publish
        public: true
        plan:
        - get: tutorial
        - put: hello-world-docker-image
            params:
            build: tutorial/tutorials/miscellaneous/docker-images/docker
        - task: run
            config:
            platform: linux
            image_resource:
                type: docker-image
                source:
                repository: ((docker-hub-username))/concourse-tutorial-hello-world
            run:
                path: /bin/hello-world
                args: []
            params:
                NAME: ((docker-hub-username))
    ```

- コマンドを実行する（Concourse CI のWebUIへのログイン）

    ```
    # fly --target tutorial login --concourse-url http://localhost:8080
    ```

    - 操作
    - `http://localhost:8080/login?fly_port=43269` でホストOSのブラウザにアクセスし、表示されたtokenを貼り付ける
    - ユーザID: `test`、パスワード: `test` とする
    - 上記操作をすると、Concourse CI のWeb UIにログインできる

        ```
        logging in to team 'main'

        navigate to the following URL in your browser:

        http://localhost:8080/login?fly_port=43269

        or enter token manually (input hidden): 
        target saved
        ```

- パイプラインを削除する

    ```
    # fly -t tutorial destroy-pipeline -p push-docker-image -n
    ```

- 資格管理マネージャにパラメータを追加する

    ```
    # vault kv put concourse/main/push-docker-image/docker-hub-email value=moriyama.kazuhiro@earthsys-lab.co.jp
    ```

    - 結果

        ```
        Success! Data written to: concourse/main/push-docker-image/docker-hub-email
        ```

    ```
    # vault kv put concourse/main/push-docker-image/docker-hub-username value=kuzumusen
    ```

    - 結果

        ```
        Success! Data written to: concourse/main/push-docker-image/docker-hub-username
        ```

    ```
    # vault kv put concourse/main/push-docker-image/docker-hub-password value=z@yaNa053
    ```

    - 結果

        ```
        Success! Data written to: concourse/main/push-docker-image/docker-hub-password
        ```

-　パイプラインを作成

    ```
    # fly -t tutorial set-pipeline -p push-docker-image -c pipeline.yml -n
    ```

    - 結果

        ```
        resources:
        resource tutorial has been added:
        + name: tutorial
        + source:
        +   branch: develop
        +   uri: https://github.com/drnic/concourse-tutorial.git
        + type: git
        
        resource hello-world-docker-image has been added:
        + name: hello-world-docker-image
        + source:
        +   email: ((docker-hub-email))
        +   password: ((docker-hub-password))
        +   repository: ((docker-hub-username))/concourse-tutorial-hello-world
        +   username: ((docker-hub-username))
        + type: docker-image
        
        jobs:
        job publish has been added:
        + name: publish
        + plan:
        + - get: tutorial
        + - params:
        +     build: tutorial/tutorials/miscellaneous/docker-images/docker
        +   put: hello-world-docker-image
        + - config:
        +     image_resource:
        +       name: ""
        +       source:
        +         repository: ((docker-hub-username))/concourse-tutorial-hello-world
        +       type: docker-image
        +     params:
        +       NAME: ((docker-hub-username))
        +     platform: linux
        +     run:
        +       path: /bin/hello-world
        +   task: run
        + public: true
        
        pipeline name: push-docker-image

        pipeline created!
        you can view your pipeline here: http://localhost:8080/teams/main/pipelines/push-docker-image

        the pipeline is currently paused. to unpause, either:
        - run the unpause-pipeline command:
            fly -t tutorial unpause-pipeline -p push-docker-image
        - click play next to the pipeline in the web ui        
        ```

- パイプラインのリソースをチェック

    ```
    # fly -t tutorial check-resource -r push-docker-image/tutorial
    ```

    - 結果

        ```
        # fly -t tutorial check-resource -r push-docker-image/tutorial
        checking push-docker-image/tutorial in build 1
        initializing check: tutorial
        selected worker: 107e5bdeec1f
        Cloning into '/tmp/git-resource-repo-cache'...
        succeeded
        ```

    ```
    # fly -t tutorial check-resource -r push-docker-image/hello-world-docker-image
    ```

    - 💀エラー発生

        ```
        # fly -t tutorial check-resource -r push-docker-image/hello-world-docker-image
        checking push-docker-image/hello-world-docker-image in build 2
        initializing check: hello-world-docker-image
        resource config creds evaluation: Get "http://127.0.0.1:8200/v1/sys/internal/ui/mounts/concourse/main/push-docker-image/docker-hub-username": dial tcp 127.0.0.1:8200: connect: connection refused (after 5 retries)
        errored        
        ```


<del>

---

    ```
    # vault token create --policy concourse
    ```





- パイプラインを実行

    ```
    # fly -t tutorial unpause-pipeline -p push-docker-image
    ```

    ```
    # fly -t tutorial trigger-job -j push-docker-image/publish -w
    ```

    - 結果


## 試行錯誤

    ```
    # export VAULT_ADDR='http://127.0.0.1:8200'
    ```


    ```
    # vault token create --policy concourse --period 1h
    ```

    - 結果

        ```
        Key                  Value
        ---                  -----
        token                hvs.CAESIACybIRJ6LjtNtZi4mGP3POwz_oHeIMun-qV48GGWyZFGh4KHGh2cy5GcEVncnRmclhMbnJsYk8yMGtiMDVaZ2U
        token_accessor       LPlhYsc6iO2MFATq96uAcIU3
        token_duration       1h
        token_renewable      true
        token_policies       ["concourse" "default"]
        identity_policies    []
        policies             ["concourse" "default"]
        ```


</del>

<del>

- 以下のサイト

    https://github.com/rahulkj/concourse-vault
    
    https://ik.am/entries/432

- より、`concourse-policy.hcl`を以下のように修正する

    ```
    path "concourse/*" {
        policy = "read"
        capabilities =  ["read", "list"]
    }
    ```

</del>

- 以下のサイトを参考にして`/etc/vault.d/vault.hcl`を修正

    [公式サイト](https://developer.hashicorp.com/vault/docs/configuration)

    https://gammalab.net/blog/3f7pudgk4zbcr/


- `/etc/vault.d/vault.hc1`に以下の設定がないことが原因と考えられる

    - `cluster_addr  = "https://127.0.0.1:8201"`
    - `api_addr      = "https://127.0.0.1:8200"`

- また、[公式サイト](https://developer.hashicorp.com/vault/docs/configuration)より、以下が推奨のようなのでこれも追加

    - `disable_mlock = true``

- vaultサービスのステータスを確認

    ```
    # systemctl status vault.service 
    ```

    - 結果

        ```
        ● vault.service - "HashiCorp Vault - A tool for managing secrets"
        Loaded: loaded (/usr/lib/systemd/system/vault.service; disabled; vendor preset: disabled)
        Active: inactive (dead)
            Docs: https://www.vaultproject.io/docs/

        9月 07 08:22:32 control-plane.minikube.internal systemd[1]: [/usr/lib/systemd/system/vault.service:7] Unknown lvalue 'StartLimitInterval...'Unit'
        9月 07 08:22:32 control-plane.minikube.internal systemd[1]: [/usr/lib/systemd/system/vault.service:8] Unknown lvalue 'StartLimitBurst' i...'Unit'
        Hint: Some lines were ellipsized, use -l to show in full.
        ```


- 修正後の`/etc/vault.d/vault.hcl`は以下

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
        disable_mlock = true

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

        cluster_addr  = "https://127.0.0.1:8201"
        api_addr      = "https://127.0.0.1:8200"
        ```

- オリジナルからの変更点

    ```
    # diff /etc/vault.d/vault.hcl_org /etc/vault.d/vault.hcl
    9c9
    < #disable_mlock = true
    ---
    > disable_mlock = true
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
    32a26,32
    > # HTTPS listener
    > #listener "tcp" {
    > #  address       = "0.0.0.0:8200"
    > #  tls_cert_file = "/opt/vault/tls/tls.crt"
    > #  tls_key_file  = "/opt/vault/tls/tls.key"
    > #}
    > 
    50a51,53
    > 
    > cluster_addr  = "https://127.0.0.1:8201"
    > api_addr      = "https://127.0.0.1:8200"
    ```

- ポートの使用状況を確認

    ```
    # sudo netstat -tuln | grep -e "8200" -e "8201" | wc -l
    0
    ```

- vaultコマンドに今どこのvaultを設定するのかってのを教えてやる。それは環境変数です
- HTTPにする

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
        Active: active (running) since 木 2023-09-07 09:00:06 JST; 10s ago
            Docs: https://www.vaultproject.io/docs/
        Main PID: 29994 (vault)
            Tasks: 8
        Memory: 247.5M
        CGroup: /system.slice/vault.service
                └─29994 /usr/bin/vault server -config=/etc/vault.d/vault.hcl

        9月 07 09:00:06 control-plane.minikube.internal vault[29994]: Listener 1: tcp (addr: "127.0.0.1:8200", cluster address: "127.0.0.1:8201"...bled")
        9月 07 09:00:06 control-plane.minikube.internal vault[29994]: Log Level:
        9月 07 09:00:06 control-plane.minikube.internal vault[29994]: Mlock: supported: true, enabled: false
        9月 07 09:00:06 control-plane.minikube.internal vault[29994]: Recovery Mode: false
        9月 07 09:00:06 control-plane.minikube.internal vault[29994]: Storage: file
        9月 07 09:00:06 control-plane.minikube.internal vault[29994]: Version: Vault v1.14.2, built 2023-08-24T13:19:12Z
        9月 07 09:00:06 control-plane.minikube.internal vault[29994]: Version Sha: 16a7033a0686eca50ee650880d5c55438d274489
        9月 07 09:00:06 control-plane.minikube.internal vault[29994]: ==> Vault server started! Log data will stream in below:
        9月 07 09:00:06 control-plane.minikube.internal vault[29994]: 2023-09-07T09:00:06.032+0900 [INFO]  proxy environment: http_proxy="" http...oxy=""
        9月 07 09:00:06 control-plane.minikube.internal vault[29994]: 2023-09-07T09:00:06.034+0900 [INFO]  core: Initializing version history ca...r core
        Hint: Some lines were ellipsized, use -l to show in full.
        ```


- ポートの使用状況を確認

    ```
    # sudo netstat -tuln | grep -e "8200" -e "8201" 
    tcp        0      0 127.0.0.1:8200          0.0.0.0:*               LISTEN 
    ```

- vault を初期化

    ```
    # vault operator init
    ```

    - 結果

        ```
        Error initializing: Error making API request.

        URL: PUT http://127.0.0.1:8200/v1/sys/init
        Code: 400. Errors:

        * Vault is already initialized
        ```

- `vault operator init`は通常実施しなそうなので、ここではこのまま作業続行とする

<del>

- vault unseal
- 以下のコマンドを実行（5つのUnseal Keyの内、3つのUnseal Keyを3回に分け入力）

    ```
    # vault operator unseal
    ```

-   Root Tokenを使ってログイン

    ```
    # vault login
    ```

- Concourse用のロールとtokenを作成
- 以下のコマンドを実行

    ```
    # pwd
    /root/vault-install
    ```

    ```
    # cat << EOF > concourse-policy.hcl
    path "concourse/*" {
        policy = "read"
        capabilities =  ["read", "list"]
    }
    EOF
    ```

    ```
    # cat concourse-policy.hcl
    ```

    ```
    # vault policy read concourse
    ```

    ```
    # vault policy delete concourse
    ```

- ここで一旦vaultのWebUIを立上げ、concourse のパラメータがどうなっているかを確認する


    ```
    # vault policy read concourse
    ```

- concourseポリシーを作る。

    ```
    # vault policy write concourse ./concourse-policy.hcl
    ```

- concourseポリシーに基づいたtokenを発行します。

    ```
    # vault token create --policy concourse
    ```

- 資格管理マネージャにパラメータを追加する

    ```
    # vault kv put concourse/main/push-docker-image/docker-hub-email value=moriyama.kazuhiro@earthsys-lab.co.jp
    ```

    - 結果

        ```
        ```

    ```
    # vault kv put concourse/main/push-docker-image/docker-hub-username value=kuzumusen
    ```

    - 結果

        ```
        ```

    ```
    # vault kv put concourse/main/push-docker-image/docker-hub-password value=z@yaNa053
    ```

    - 結果

        ```
        ```

</del>


- そしたらconcourseポリシーに基づいたtokenを発行します。

    ```
    # vault token create --policy concourse
    ```

    - 結果

        ```
        Error creating token: Error making API request.

        URL: POST http://127.0.0.1:8200/v1/auth/token/create
        Code: 503. Errors:

        * Vault is sealed
        ```

- unealed が必要模様

- 以下のコマンドを実行（5つのUnseal Keyの内、3つのUnseal Keyを3回に分け入力）

    ```
    # vault operator unseal
    ```

- そしたらconcourseポリシーに基づいたtokenを発行します。

    ```
    # vault token create --policy concourse
    ```

    - 結果

        ```
        Key                  Value
        ---                  -----
        token                hvs.CAESIPq9lJYziFyyw5_BdaaV8SqjKVkpTQwbsCxR9ZsXZPb_Gh4KHGh2cy5OZUJVS2JhTHZUaEZmYWw5VUlRMEdhTjg
        token_accessor       brEVjxrMb2nfheKwzuFjOeRN
        token_duration       768h
        token_renewable      true
        token_policies       ["concourse" "default"]
        identity_policies    []
        policies             ["concourse" "default"]
        ```

## concourseCIとvaultを接続

- 以下のサイトを参考にした

    https://gammalab.net/blog/3f7pudgk4zbcr/

    https://concourse-ci.org/vault-credential-manager.html

- 先程のトークンをconcourseCIに設定します！docker-composeファイルに以下を追記するだけです。
- To configure this, first configure the URL of your Vault server by setting the following env on the web node

    ```
    # cd ~/concourse-install
    ```

- concourseの`docker-compose.yml`（変更後）

    ```
    # cat docker-compose.yml
    version: '3'

    services:
    db:
        image: postgres
        environment:
        POSTGRES_DB: concourse
        POSTGRES_USER: concourse_user
        POSTGRES_PASSWORD: concourse_pass
        logging:
        driver: "json-file"
        options:
            max-file: "5"
            max-size: "10m"

    web:
        image: concourse/concourse
        command: web
        links: [db]
        depends_on: [db]
        ports: ["8080:8080"]
        volumes: ["./keys/web:/concourse-keys"]
        environment:
        CONCOURSE_EXTERNAL_URL: http://localhost:8080
        CONCOURSE_POSTGRES_HOST: db
        CONCOURSE_POSTGRES_USER: concourse_user
        CONCOURSE_POSTGRES_PASSWORD: concourse_pass
        CONCOURSE_POSTGRES_DATABASE: concourse
        CONCOURSE_ADD_LOCAL_USER: test:test
        CONCOURSE_MAIN_TEAM_LOCAL_USER: test
        CONCOURSE_VAULT_URL: http://127.0.0.1:8200
        CONCOURSE_VAULT_CLIENT_TOKEN: hvs.CAESIPq9lJYziFyyw5_BdaaV8SqjKVkpTQwbsCxR9ZsXZPb_Gh4KHGh2cy5OZUJVS2JhTHZUaEZmYWw5VUlRMEdhTjg

        logging:
        driver: "json-file"
        options:
            max-file: "5"
            max-size: "10m"

    worker:
        image: concourse/concourse
        command: worker
        privileged: true
        depends_on: [web]
        volumes: ["./keys/worker:/concourse-keys"]
        links: [web]
        stop_signal: SIGUSR2
        environment:
        CONCOURSE_TSA_HOST: web:2222
        # enable DNS proxy to support Docker's 127.x.x.x DNS server
        CONCOURSE_GARDEN_DNS_PROXY_ENABLE: "true"
        logging:
        driver: "json-file"
        options:
            max-file: "5"
            max-size: "10m"
    ```

- concourse を再起動

    ```
    # docker-compose down
    [+] Running 4/4
    ✔ Container concourse-install-worker-1  Removed                                                                                              0.0s 
    ✔ Container concourse-install-web-1     Removed                                                                                              0.0s 
    ✔ Container concourse-install-db-1      Removed                                                                                              0.0s 
    ✔ Network concourse-install_default     Removed 
    ```

    ```
    # docker-compose up -d
    [+] Running 4/4
    ✔ Network concourse-install_default     Created                                                                                              0.3s 
    ✔ Container concourse-install-db-1      Started                                                                                              0.1s 
    ✔ Container concourse-install-web-1     Started                                                                                              0.1s 
    ✔ Container concourse-install-worker-1  Started  
    ```

- コマンドを実行する（Concourse CI のWebUIへのログイン）

    ```
    # fly --target tutorial login --concourse-url http://localhost:8080
    ```

    - 操作
    - `http://localhost:8080/login?fly_port=43269` でホストOSのブラウザにアクセスし、表示されたtokenを貼り付ける
    - ユーザID: `test`、パスワード: `test` とする
    - 上記操作をすると、Concourse CI のWeb UIにログインできる

        ```
        logging in to team 'main'

        navigate to the following URL in your browser:

        http://localhost:8080/login?fly_port=43269

        or enter token manually (input hidden): 
        target saved
        ```

- パイプラインを削除する

    ```
    # fly -t tutorial destroy-pipeline -p push-docker-image -n


-　パイプラインを作成


    ```
    # cd ~/concourse-tutorial/tutorials/miscellaneous/docker-images/
    ```

    ```
    # fly -t tutorial set-pipeline -p push-docker-image -c pipeline.yml -n
    ```

    - 結果

        ```
        resources:
        resource tutorial has been added:
        + name: tutorial
        + source:
        +   branch: develop
        +   uri: https://github.com/drnic/concourse-tutorial.git
        + type: git
        
        resource hello-world-docker-image has been added:
        + name: hello-world-docker-image
        + source:
        +   email: ((docker-hub-email))
        +   password: ((docker-hub-password))
        +   repository: ((docker-hub-username))/concourse-tutorial-hello-world
        +   username: ((docker-hub-username))
        + type: docker-image
        
        jobs:
        job publish has been added:
        + name: publish
        + plan:
        + - get: tutorial
        + - params:
        +     build: tutorial/tutorials/miscellaneous/docker-images/docker
        +   put: hello-world-docker-image
        + - config:
        +     image_resource:
        +       name: ""
        +       source:
        +         repository: ((docker-hub-username))/concourse-tutorial-hello-world
        +       type: docker-image
        +     params:
        +       NAME: ((docker-hub-username))
        +     platform: linux
        +     run:
        +       path: /bin/hello-world
        +   task: run
        + public: true
        
        pipeline name: push-docker-image

        pipeline created!
        you can view your pipeline here: http://localhost:8080/teams/main/pipelines/push-docker-image

        the pipeline is currently paused. to unpause, either:
        - run the unpause-pipeline command:
            fly -t tutorial unpause-pipeline -p push-docker-image
        - click play next to the pipeline in the web ui        
        ```

- パイプラインのリソースをチェック

    ```
    # fly -t tutorial check-resource -r push-docker-image/hello-world-docker-image
    ```

    - エラー発生（結果は変わらず）

        ```
        # fly -t tutorial check-resource -r push-docker-image/hello-world-docker-image
        checking push-docker-image/hello-world-docker-image in build 1
        initializing check: hello-world-docker-image
        resource config creds evaluation: Get "http://127.0.0.1:8200/v1/sys/internal/ui/mounts/concourse/main/push-docker-image/docker-hub-username": dial tcp 127.0.0.1:8200: connect: connection refused (after 5 retries)
        errored
        ```

# vault-install (やり直し)
- VMをVaultインストール前に戻した

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

## インストール手順

- 以下の公式サイトを参考にした

    https://developer.hashicorp.com/vault/tutorials/getting-started/getting-started-install

- 以下のコマンドを実行

    ```
    # sudo yum install -y yum-utils
    ```

    - 結果

        ```
        読み込んだプラグイン:fastestmirror, langpacks
        Loading mirror speeds from cached hostfile
        * base: ftp-srv2.kddilabs.jp
        * elrepo: ftp.yz.yamagata-u.ac.jp
        * extras: ftp-srv2.kddilabs.jp
        * updates: ftp-srv2.kddilabs.jp
        base                                                                                                                                                                                                                  | 3.6 kB  00:00:00     
        docker-ce-stable                                                                                                                                                                                                      | 3.5 kB  00:00:00     
        elrepo                                                                                                                                                                                                                | 3.0 kB  00:00:00     
        extras                                                                                                                                                                                                                | 2.9 kB  00:00:00     
        ius                                                                                                                                                                                                                   | 1.3 kB  00:00:00     
        updates                                                                                                                                                                                                               | 2.9 kB  00:00:00     
        docker-ce-stable/7/x86_64/primary_db                                                                                                                                                                                  | 117 kB  00:00:00     
        パッケージ yum-utils-1.1.31-54.el7_8.noarch はインストール済みか最新バージョンです
        何もしません
        ```
    ```
    # sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
    ```
    
    - 結果

        ```
        読み込んだプラグイン:fastestmirror, langpacks
        adding repo from: https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
        grabbing file https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo to /etc/yum.repos.d/hashicorp.repo
        repo saved to /etc/yum.repos.d/hashicorp.repo
        ```

    ```
    # sudo yum -y install vault
    ```

    - 結果

        ```
        読み込んだプラグイン:fastestmirror, langpacks
        Loading mirror speeds from cached hostfile
        * base: ftp-srv2.kddilabs.jp
        * elrepo: ftp.yz.yamagata-u.ac.jp
        * extras: ftp-srv2.kddilabs.jp
        * updates: ftp-srv2.kddilabs.jp
        hashicorp                                                                                                  | 1.4 kB  00:00:00     
        hashicorp/7/x86_64/primary                                                                                 | 182 kB  00:00:00     
        hashicorp                                                                                                               1313/1313
        依存性の解決をしています
        --> トランザクションの確認を実行しています。
        ---> パッケージ vault.x86_64 0:1.14.2-1 を インストール
        --> 依存性解決を終了しました。

        依存性を解決しました

        ==================================================================================================================================
        Package                     アーキテクチャー             バージョン                        リポジトリー                     容量
        ==================================================================================================================================
        インストール中:
        vault                       x86_64                       1.14.2-1                          hashicorp                       129 M

        トランザクションの要約
        ==================================================================================================================================
        インストール  1 パッケージ

        総ダウンロード容量: 129 M
        インストール容量: 354 M
        Downloading packages:
        警告: /var/cache/yum/x86_64/7/hashicorp/packages/vault-1.14.2-1.x86_64.rpm: ヘッダー V4 RSA/SHA256 Signature、鍵 ID a621e701: NOKEY
        vault-1.14.2-1.x86_64.rpm の公開鍵がインストールされていません
        vault-1.14.2-1.x86_64.rpm                                                                                  | 129 MB  00:00:10     
        https://rpm.releases.hashicorp.com/gpg から鍵を取得中です。
        Importing GPG key 0xA621E701:
        Userid     : "HashiCorp Security (HashiCorp Package Signing) <security+packaging@hashicorp.com>"
        Fingerprint: 798a ec65 4e5c 1542 8c8e 42ee aa16 fcbc a621 e701
        From       : https://rpm.releases.hashicorp.com/gpg
        Running transaction check
        Running transaction test
        Transaction test succeeded
        Running transaction
        インストール中          : vault-1.14.2-1.x86_64                                                                             1/1Generating Vault TLS key and self-signed certificate...
        Generating a 4096 bit RSA private key
        ...................................................++
        .........................................................................++
        writing new private key to 'tls.key'
        -----
        Vault TLS key and self-signed certificate have been generated in '/opt/vault/tls'.
        検証中                  : vault-1.14.2-1.x86_64                                                                             1/1 

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

 - 以下のサイトを参考にして`/etc/vault.d/vault.hc1`を修正

    [公式サイト](https://developer.hashicorp.com/vault/docs/configuration)

    https://gammalab.net/blog/3f7pudgk4zbcr/


- 修正前の`/etc/vault.d/vault.hcl`は以下

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

    ``` sh
    > diff -u  etc/vault.d/vault.hcl_org /etc/vault.d/vault.hcl
    ```

    - 結果

        ```diff
        --- /etc/vault.d/vault.hcl_org  2023-09-07 20:38:03.816620134 +0900
        +++ /etc/vault.d/vault.hcl      2023-09-07 20:45:02.270817911 +0900
        @@ -6,7 +6,7 @@
        ui = true
        
        #mlock = true
        -#disable_mlock = true
        +disable_mlock = true
        
        storage "file" {
        path = "/opt/vault/data"
        @@ -18,18 +18,18 @@
        #}
        
        # HTTP listener
        -#listener "tcp" {
        -#  address = "127.0.0.1:8200"
        -#  tls_disable = 1
        -#}
        -
        -# HTTPS listener
        listener "tcp" {
        -  address       = "0.0.0.0:8200"
        -  tls_cert_file = "/opt/vault/tls/tls.crt"
        -  tls_key_file  = "/opt/vault/tls/tls.key"
        +  address = "127.0.0.1:8200"
        +  tls_disable = 1
        }
        
        +# HTTPS listener
        +#listener "tcp" {
        +#  address       = "0.0.0.0:8200"
        +#  tls_cert_file = "/opt/vault/tls/tls.crt"
        +#  tls_key_file  = "/opt/vault/tls/tls.key"
        +#}
        +
        # Enterprise license_path
        # This will be required for enterprise as of v1.8
        #license_path = "/etc/vault.d/vault.hclic"
        @@ -48,3 +48,6 @@
        #  key_label      = "vault-hsm-key"
        #  hmac_key_label = "vault-hsm-hmac-key"
        #}
        +
        +cluster_addr  = "https://127.0.0.1:8201"
        +api_addr      = "https://127.0.0.1:8200"
        ```
    
- 変更後

    ```sh
    cat /etc/vault.d/vault.hcl
    # Copyright (c) HashiCorp, Inc.
    # SPDX-License-Identifier: MPL-2.0

    # Full configuration options can be found at https://www.vaultproject.io/docs/configuration

    ui = true

    #mlock = true
    disable_mlock = true

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

    cluster_addr  = "https://127.0.0.1:8201"
    api_addr      = "https://127.0.0.1:8200"
    ```

- vaultコマンドに今どこのvaultを設定するのかってのを教えてやる。それは環境変数です
- HTTPにする

    ```sh
    export VAULT_ADDR='http://127.0.0.1:8200'
    ```

    ```sh
    echo $VAULT_ADDR
    http://127.0.0.1:8200
    ```

- vault を起動する。
- 参考サイトにはこのコマンドがない

    ```sh
    > systemctl start vault.service
    ```

    ```sh
    > systemctl status vault.service
    ```

    - 結果

        ```sh
        ● vault.service - "HashiCorp Vault - A tool for managing secrets"
        Loaded: loaded (/usr/lib/systemd/system/vault.service; disabled; vendor preset: disabled)
        Active: active (running) since 木 2023-09-07 21:27:00 JST; 9s ago
            Docs: https://www.vaultproject.io/docs/
        Main PID: 13661 (vault)
            Tasks: 7
        Memory: 53.8M
        CGroup: /system.slice/vault.service
                └─13661 /usr/bin/vault server -config=/etc/vault.d/vault.hcl

        9月 07 21:27:00 control-plane.minikube.internal vault[13661]: Log Level:
        9月 07 21:27:00 control-plane.minikube.internal vault[13661]: Mlock: supported: true, enabled: false
        9月 07 21:27:00 control-plane.minikube.internal vault[13661]: Recovery Mode: false
        9月 07 21:27:00 control-plane.minikube.internal vault[13661]: Storage: file
        9月 07 21:27:00 control-plane.minikube.internal vault[13661]: Version: Vault v1.14.2, built 2023-08-24T13:19:12Z
        9月 07 21:27:00 control-plane.minikube.internal vault[13661]: Version Sha: 16a7033a0686eca50ee650880d5c55438d274489
        9月 07 21:27:00 control-plane.minikube.internal systemd[1]: Started "HashiCorp Vault - A tool for managing secrets".
        9月 07 21:27:00 control-plane.minikube.internal vault[13661]: ==> Vault server started! Log data will stream in below:
        9月 07 21:27:00 control-plane.minikube.internal vault[13661]: 2023-09-07T21:27:00.401+0900 [INFO]  proxy environment: htt...y=""
        9月 07 21:27:00 control-plane.minikube.internal vault[13661]: 2023-09-07T21:27:00.418+0900 [INFO]  core: Initializing ver...core
        Hint: Some lines were ellipsized, use -l to show in full.        
        ```

- ポートの使用状況を確認

    ```sh
    $ sudo netstat -tuln | grep -e "8200" -e "8201" 
    tcp        0      0 127.0.0.1:8200          0.0.0.0:*               LISTEN 
    ```

- vault を初期化

    ```sh
    $ vault operator init
    ```

    - 結果

        ```sh
        Unseal Key 1: 8xo+4C5UI/rOeU7wZNi4Qs1S6JojwgOROhlbTrfis+eG
        Unseal Key 2: Lim8YfM2xWkzAz/ZoVmlrJwFdyX1CzVxP2HhdZluSzL5
        Unseal Key 3: ad6nN19vKJNUR6RFZ0Ysu5eoTbLgY8AbFYulJdmKFvmr
        Unseal Key 4: hN0xF1UXmhCmzPmYeDIq1oaVDh8qI1duGsEAbW6l4tR0
        Unseal Key 5: 80W1xofM4bYxwMqxYI9LbzxbB5sP1uSlpWo+EeaExaYZ

        Initial Root Token: hvs.suX0HFLMYSOPjrucW8SPNyMG

        Vault initialized with 5 key shares and a key threshold of 3. Please securely
        distribute the key shares printed above. When the Vault is re-sealed,
        restarted, or stopped, you must supply at least 3 of these keys to unseal it
        before it can start servicing requests.

        Vault does not store the generated root key. Without at least 3 keys to
        reconstruct the root key, Vault will remain permanently sealed!

        It is possible to generate new unseal keys, provided you have a quorum of
        existing unseal keys shares. See "vault operator rekey" for more information.
        ```

- 以下のコマンドを実行（5つのUnseal Keyの内、3つのUnseal Keyを3回に分け入力）

    ```sh 
    $ sudo vault operator unseal
    ```

    - 結果 (エラー)

        ```sh
        Unseal Key (will be hidden): 
        Error unsealing: Put "https://127.0.0.1:8200/v1/sys/unseal": http: server gave HTTP response to HTTPS client
        ```

- `/etc/vault.d/vault.hcl`に誤りを発見

    ```diff
    $ sudo diff -u /etc/vault.d/vault.hcl_old /etc//vault.d//vault.hcl
    --- /etc/vault.d/vault.hcl_old  2023-09-07 21:45:18.590872923 +0900
    +++ /etc//vault.d//vault.hcl    2023-09-07 21:46:19.481233421 +0900
    @@ -49,5 +49,5 @@
    #  hmac_key_label = "vault-hsm-hmac-key"
    #}
    
    -cluster_addr  = "https://127.0.0.1:8201"
    -api_addr      = "https://127.0.0.1:8200"
    +cluster_addr  = "http://127.0.0.1:8201"
    +api_addr      = "http://127.0.0.1:8200"
    ```

- 変更後

    ```sh
    $ cat  /etc/vault.d/vault.hcl
    # Copyright (c) HashiCorp, Inc.
    # SPDX-License-Identifier: MPL-2.0

    # Full configuration options can be found at https://www.vaultproject.io/docs/configuration

    ui = true

    #mlock = true
    disable_mlock = true

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

    cluster_addr  = "http://127.0.0.1:8201"
    api_addr      = "http://127.0.0.1:8200"
    ```

- vault を再起動

    ```sh
    $ sudo systemctl restart vault.service
    ```

    ```sh
    $ sudo systemctl status  vault.service
    ● vault.service - "HashiCorp Vault - A tool for managing secrets"
    Loaded: loaded (/usr/lib/systemd/system/vault.service; disabled; vendor preset: disabled)
    Active: active (running) since 木 2023-09-07 21:52:51 JST; 1min 11s ago
        Docs: https://www.vaultproject.io/docs/
    Main PID: 17011 (vault)
        Tasks: 7
    Memory: 25.1M
    CGroup: /system.slice/vault.service
            └─17011 /usr/bin/vault server -config=/etc/vault.d/vault.hcl

    9月 07 21:52:51 control-plane.minikube.internal vault[17011]: Log Level:
    9月 07 21:52:51 control-plane.minikube.internal vault[17011]: Mlock: supported: true, enabled: false
    9月 07 21:52:51 control-plane.minikube.internal systemd[1]: Started "HashiCorp Vault - A tool for managing secrets".
    9月 07 21:52:51 control-plane.minikube.internal vault[17011]: Recovery Mode: false
    9月 07 21:52:51 control-plane.minikube.internal vault[17011]: Storage: file
    9月 07 21:52:51 control-plane.minikube.internal vault[17011]: Version: Vault v1.14.2, built 2023-08-24T13:19:12Z
    9月 07 21:52:51 control-plane.minikube.internal vault[17011]: Version Sha: 16a7033a0686eca50ee650880d5c55438d274489
    9月 07 21:52:51 control-plane.minikube.internal vault[17011]: ==> Vault server started! Log data will stream in below:
    9月 07 21:52:51 control-plane.minikube.internal vault[17011]: 2023-09-07T21:52:51.866+0900 [INFO]  proxy environment: htt...y=""
    9月 07 21:52:51 control-plane.minikube.internal vault[17011]: 2023-09-07T21:52:51.867+0900 [INFO]  core: Initializing ver...core
    Hint: Some lines were ellipsized, use -l to show in full.
    ```

## vault unseal

- 以下のコマンドを実行（5つのUnseal Keyの内、3つのUnseal Keyを3回に分け入力）

    ```
    $ vault operator unseal
    ```

-   Root Tokenを使ってログイン

    ```
    $ vault login
    ```

- Concourse用のロールとtokenを作成
- 以下のコマンドを実行

    ```sh
    $ cd /root/vault-install
    ```

    ```sh
    $ cat << EOF > concourse-policy.hcl
    path "concourse/*" {
            capabilities = ["read"]
    }   
    EOF
    ```

    ```sh
    $ cat concourse-policy.hcl
    path "concourse/*" {
        capabilities = ["read"]
    }   
    ```

- concourseポリシーを作る。

    ```sh
    $ vault policy write concourse ./concourse-policy.hcl
    Success! Uploaded policy: concourse
    ```

    ```sh
    $ vault policy read concourse
    path "concourse/*" {
        capabilities = ["read"]
    }    
    ```

     - これでconcourseポリシーを作ることができました。

- そしたらconcourseポリシーに基づいたtokenを発行します。

    ```
    $ vault token create --policy concourse
    Key                  Value
    ---                  -----
    token                hvs.CAESIL5D30bmYnQZBbcpPu8AfJ9CAlaQSTF5cyhdYdex8muVGh4KHGh2cy5LRWRRREVQQTR3QkJOclc2eFQ1TGZRUVM
    token_accessor       bwG96AYBeRjFHbpPfj4boKvU
    token_duration       768h
    token_renewable      true
    token_policies       ["concourse" "default"]
    identity_policies    []
    policies             ["concourse" "default"]
    ```


## concourseCIとvaultを接続

- 以下のサイトを参考にした

    https://gammalab.net/blog/3f7pudgk4zbcr/

    https://concourse-ci.org/vault-credential-manager.html

- 先程のトークンをconcourseCIに設定します！docker-composeファイルに以下を追記するだけです。
- To configure this, first configure the URL of your Vault server by setting the following env on the web node

    ```sh
    $ cd ~/concourse-install
    ```

- concourseの`docker-compose.yml`（変更後）

    ```sh
    $ cat docker-compose.yml
    version: '3'

    services:
    db:
        image: postgres
        environment:
        POSTGRES_DB: concourse
        POSTGRES_USER: concourse_user
        POSTGRES_PASSWORD: concourse_pass
        logging:
        driver: "json-file"
        options:
            max-file: "5"
            max-size: "10m"

    web:
        image: concourse/concourse
        command: web
        links: [db]
        depends_on: [db]
        ports: ["8080:8080"]
        volumes: ["./keys/web:/concourse-keys"]
        environment:
        CONCOURSE_EXTERNAL_URL: http://localhost:8080
        CONCOURSE_POSTGRES_HOST: db
        CONCOURSE_POSTGRES_USER: concourse_user
        CONCOURSE_POSTGRES_PASSWORD: concourse_pass
        CONCOURSE_POSTGRES_DATABASE: concourse
        CONCOURSE_ADD_LOCAL_USER: test:test
        CONCOURSE_MAIN_TEAM_LOCAL_USER: test
        CONCOURSE_VAULT_URL: http://127.0.0.1:8200
        CONCOURSE_VAULT_CLIENT_TOKEN: hvs.CAESII7rrevJiuWGuxuBao28rbs1OnmdLUMV2g05hdXd84uQGh4KHGh2cy5oeGwxeEZoUDFqUVBYU2JsMTRoc3pxSFg

        logging:
        driver: "json-file"
        options:
            max-file: "5"
            max-size: "10m"

    worker:
        image: concourse/concourse
        command: worker
        privileged: true
        depends_on: [web]
        volumes: ["./keys/worker:/concourse-keys"]
        links: [web]
        stop_signal: SIGUSR2
        environment:
        CONCOURSE_TSA_HOST: web:2222
        # enable DNS proxy to support Docker's 127.x.x.x DNS server
        CONCOURSE_GARDEN_DNS_PROXY_ENABLE: "true"
        logging:
        driver: "json-file"
        options:
            max-file: "5"
            max-size: "10m"   
    ```

- concourse を起動

    ```sh
    $ docker-compose up -d
    [+] Running 3/3
    ✔ Container concourse-install-db-1      Started                                                                                                0.0s 
    ✔ Container concourse-install-web-1     Started                                                                                                0.1s 
    ✔ Container concourse-install-worker-1  Started 
    ```


## Concourse用のシークレットを作成

- 以下のコマンドを実行

    ```sh
    $ vault secrets enable -version=1 -path concourse kv
    Success! Enabled the kv secrets engine at: concourse/
    ```

    - これでconcourse/以下に自由にkv型のデータを配置できるようになりました。

- 参考にしているサイト

    https://gammalab.net/blog/3f7pudgk4zbcr/

- のサンプルがよくわからないので、concourse の日本語チュートリアルでハンズオンを行ってみる
- 参考にしたサイトは以下（Docker イメージの作成・利用）

    https://concoursetutorial-ja.site.lkj.io/miscellaneous/docker-images


## Docker イメージの作成・利用（Vault版）

- 参考にしたサイトは以下（Docker イメージの作成・利用）

    https://concoursetutorial-ja.site.lkj.io/miscellaneous/docker-images

- 上記サイトの内容をVault で実装してみる

- 以下のコマンドを実行し、`pipeline.yml`の内容を確認

    ```sh
    $ cd ~/concourse-tutorial/tutorials/miscellaneous/docker-images/
    ```

    ```
    $ cat pipeline.yml 
    ---
    resources:
    - name: tutorial
        type: git
        source:
        uri: https://github.com/drnic/concourse-tutorial.git
        branch: develop

    - name: hello-world-docker-image
        type: docker-image
        source:
        email: ((docker-hub-email))
        username: ((docker-hub-username))
        password: ((docker-hub-password))
        repository: ((docker-hub-username))/concourse-tutorial-hello-world

    jobs:
    - name: publish
        public: true
        plan:
        - get: tutorial
        - put: hello-world-docker-image
            params:
            build: tutorial/tutorials/miscellaneous/docker-images/docker
        - task: run
            config:
            platform: linux
            image_resource:
                type: docker-image
                source:
                repository: ((docker-hub-username))/concourse-tutorial-hello-world
            run:
                path: /bin/hello-world
                args: []
            params:
                NAME: ((docker-hub-username))
    ```

- 資格管理マネージャにパラメータを追加する

    ```sh
    $ vault kv put concourse/main/push-docker-image/docker-hub-email value=moriyama.kazuhiro@earthsys-lab.co.jp
    Success! Data written to: concourse/main/push-docker-image/docker-hub-email
    ```

    ```sh
    $ vault kv put concourse/main/push-docker-image/docker-hub-username value=kuzumusen
    Success! Data written to: concourse/main/push-docker-image/docker-hub-username
    ```

    ```sh
    $ vault kv put concourse/main/push-docker-image/docker-hub-password value=z@yaNa053
    Success! Data written to: concourse/main/push-docker-image/docker-hub-password
    ```

- コマンドを実行する（Concourse CI のWebUIへのログイン）

    ```
    $ fly --target tutorial login --concourse-url http://localhost:8080
    ```

    - 操作
    - `http://localhost:8080/login?fly_port=43269` でホストOSのブラウザにアクセスし、表示されたtokenを貼り付ける
    - ユーザID: `test`、パスワード: `test` とする
    - 上記操作をすると、Concourse CI のWeb UIにログインできる

        ```
        logging in to team 'main'

        navigate to the following URL in your browser:

        http://localhost:8080/login?fly_port=43269

        or enter token manually (input hidden): 
        target saved
        ```

- パイプラインを削除する

    ```
    $ fly -t tutorial destroy-pipeline -p push-docker-image -n
    ```

-　パイプラインを作成

    ```
    $ fly -t tutorial set-pipeline -p push-docker-image -c pipeline.yml -n
    resources:
    resource hello-world-docker-image has changed:
    name: hello-world-docker-image
    source:
    -   email: moriyama.kazuhiro@earthsys-lab.co.jp
    -   password: z@yaNa053
    -   repository: kuzumusen/concourse-tutorial-hello-world
    -   username: kuzumusen
    +   email: ((docker-hub-email))
    +   password: ((docker-hub-password))
    +   repository: ((docker-hub-username))/concourse-tutorial-hello-world
    +   username: ((docker-hub-username))
    type: docker-image
    
    jobs:
    job publish has changed:
    name: publish
    plan:
    - get: tutorial
    - params:
        build: tutorial/tutorials/miscellaneous/docker-images/docker
        put: hello-world-docker-image
    - config:
        image_resource:
            name: ""
            source:
    -         repository: kuzumusen/concourse-tutorial-hello-world
    +         repository: ((docker-hub-username))/concourse-tutorial-hello-world
            type: docker-image
        params:
    -       NAME: kuzumusen
    +       NAME: ((docker-hub-username))
        platform: linux
        run:
            path: /bin/hello-world
        task: run
    public: true
    
    pipeline name: push-docker-image

    configuration updated
    ```

- パイプラインのリソースをチェック

    ```
    $ fly -t tutorial check-resource -r push-docker-image/hello-world-docker-image
    resource config creds evaluation: Get "http://127.0.0.1:8200/v1/sys/internal/ui/mounts/concourse/main/push-docker-image/docker-hub-email": dial tcp 127.0.0.1:8200: connect: connection refused (after 5 retries)
    errored
    ```

## 上手くいかない。→ Vault のIPを 127.0.0.1 から 10.1.1.200 に変更してみる

-  変更内容

    ```diff
    # diff -u  /etc/vault.d/vault.hcl_old /etc/vault.d/vault.hcl
    --- /etc/vault.d/vault.hcl_old  2023-09-07 21:45:18.590872923 +0900
    +++ /etc/vault.d/vault.hcl      2023-09-09 22:35:18.308618724 +0900
    @@ -19,7 +19,8 @@
    
    # HTTP listener
    listener "tcp" {
    -  address = "127.0.0.1:8200"
    +#  address = "127.0.0.1:8200"
    +  address = "10.1.1.200:8200"
    tls_disable = 1
    }
    
    @@ -49,5 +50,7 @@
    #  hmac_key_label = "vault-hsm-hmac-key"
    #}
    
    -cluster_addr  = "https://127.0.0.1:8201"
    -api_addr      = "https://127.0.0.1:8200"
    +# cluster_addr  = "http://127.0.0.1:8201"
    +cluster_addr  = "http://10.1.1.200:8201"
    +# api_addr      = "http://127.0.0.1:8200"
    +api_addr      = "http://10.1.1.200:8200"
    ```

- 変更後

    ```sh
    cat /etc/vault.d/vault.hcl
    # Copyright (c) HashiCorp, Inc.
    # SPDX-License-Identifier: MPL-2.0

    # Full configuration options can be found at https://www.vaultproject.io/docs/configuration

    ui = true

    #mlock = true
    disable_mlock = true

    storage "file" {
    path = "/opt/vault/data"
    }

    #storage "consul" {
    #  address = "127.0.0.1:8500"
    #  path    = "vault"
    #}

    # HTTP listener
    listener "tcp" {
    #  address = "127.0.0.1:8200"
    address = "10.1.1.200:8200"
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

    # cluster_addr  = "http://127.0.0.1:8201"
    cluster_addr  = "http://10.1.1.200:8201"
    # api_addr      = "http://127.0.0.1:8200"
    api_addr      = "http://10.1.1.200:8200"
    ```

- vault を再起動


    ```sh
    $ systemctl status vault.service 
    ● vault.service - "HashiCorp Vault - A tool for managing secrets"
    Loaded: loaded (/usr/lib/systemd/system/vault.service; disabled; vendor preset: disabled)
    Active: inactive (dead)
        Docs: https://www.vaultproject.io/docs/

    9月 09 22:41:10 control-plane.minikube.internal systemd[1]: [/usr/lib/systemd/system/vault.service:7] Unknown lvalue 'StartLimitIntervalSec' in section 'Unit'
    9月 09 22:41:10 control-plane.minikube.internal systemd[1]: [/usr/lib/systemd/system/vault.service:8] Unknown lvalue 'StartLimitBurst' in section 'Unit'    
    ```

    ```sh
    systemctl start vault.service 
    ```

    ```sh
    systemctl status vault.service 
    ● vault.service - "HashiCorp Vault - A tool for managing secrets"
    Loaded: loaded (/usr/lib/systemd/system/vault.service; disabled; vendor preset: disabled)
    Active: active (running) since 土 2023-09-09 22:43:37 JST; 5s ago
        Docs: https://www.vaultproject.io/docs/
    Main PID: 16088 (vault)
        Tasks: 7
    Memory: 250.6M
    CGroup: /system.slice/vault.service
            └─16088 /usr/bin/vault server -config=/etc/vault.d/vault.hcl

    9月 09 22:43:37 control-plane.minikube.internal vault[16088]: Log Level:
    9月 09 22:43:37 control-plane.minikube.internal vault[16088]: Mlock: supported: true, enabled: false
    9月 09 22:43:37 control-plane.minikube.internal vault[16088]: Recovery Mode: false
    9月 09 22:43:37 control-plane.minikube.internal vault[16088]: Storage: file
    9月 09 22:43:37 control-plane.minikube.internal vault[16088]: Version: Vault v1.14.2, built 2023-08-24T13:19:12Z
    9月 09 22:43:37 control-plane.minikube.internal vault[16088]: Version Sha: 16a7033a0686eca50ee650880d5c55438d274489
    9月 09 22:43:37 control-plane.minikube.internal vault[16088]: ==> Vault server started! Log data will stream in below:
    9月 09 22:43:37 control-plane.minikube.internal vault[16088]: 2023-09-09T22:43:37.309+0900 [INFO]  proxy environment: http_proxy="" https_proxy="" no_proxy=""
    9月 09 22:43:37 control-plane.minikube.internal vault[16088]: 2023-09-09T22:43:37.317+0900 [INFO]  core: Initializing version history cache for core
    9月 09 22:43:37 control-plane.minikube.internal systemd[1]: Started "HashiCorp Vault - A tool for managing secrets".
    ```

    ```sh
    sudo netstat -tuln | grep 8200
    tcp        0      0 10.1.1.200:8200         0.0.0.0:*               LISTEN
    ```

    ```sh
    $ vault policy read concourse
    Error reading policy named concourse: Get "https://127.0.0.1:8200/v1/sys/policies/acl/concourse": dial tcp 127.0.0.1:8200: connect: connection refused
    ```

- 以下のコマンドを実行（5つのUnseal Keyの内、3つのUnseal Keyを3回に分け入力）

    ```sh 
    $ sudo vault operator unseal
    ```

    ```
    # vault operator unseal
    Unseal Key (will be hidden): 
    Error unsealing: Put "https://127.0.0.1:8200/v1/sys/unseal": dial tcp 127.0.0.1:8200: connect: connection refuse
    ```

- vaultは途中でIPアドレスを変えることができそう

# valte を再初期化してみる


- Vaultを停止します。


    ```sh
    $ systemctl stop vault.service
    ```

    ```sh
    $ systemctl status vault.service 
    ● vault.service - "HashiCorp Vault - A tool for managing secrets"
    Loaded: loaded (/usr/lib/systemd/system/vault.service; disabled; vendor preset: disabled)
    Active: inactive (dead)
        Docs: https://www.vaultproject.io/docs/

    9月 09 22:43:37 control-plane.minikube.internal vault[16088]: 2023-09-09T22:43:37.317+0900 [INFO]  core: Initializing version history cache for core
    9月 09 22:43:37 control-plane.minikube.internal systemd[1]: Started "HashiCorp Vault - A tool for managing secrets".
    9月 09 23:06:09 control-plane.minikube.internal systemd[1]: Stopping "HashiCorp Vault - A tool for managing secrets"...
    9月 09 23:06:09 control-plane.minikube.internal systemd[1]: Stopped "HashiCorp Vault - A tool for managing secrets".
    9月 09 23:06:09 control-plane.minikube.internal systemd[1]: [/usr/lib/systemd/system/vault.service:7] Unknown lvalue 'StartLimitIntervalSec' in section 'Unit'
    9月 09 23:06:09 control-plane.minikube.internal systemd[1]: [/usr/lib/systemd/system/vault.service:8] Unknown lvalue 'StartLimitBurst' in section 'Unit'
    9月 09 23:06:09 control-plane.minikube.internal systemd[1]: [/usr/lib/systemd/system/vault.service:7] Unknown lvalue 'StartLimitIntervalSec' in section 'Unit'
    9月 09 23:06:09 control-plane.minikube.internal systemd[1]: [/usr/lib/systemd/system/vault.service:8] Unknown lvalue 'StartLimitBurst' in section 'Unit'
    9月 09 23:07:03 control-plane.minikube.internal systemd[1]: [/usr/lib/systemd/system/vault.service:7] Unknown lvalue 'StartLimitIntervalSec' in section 'Unit'
    9月 09 23:07:03 control-plane.minikube.internal systemd[1]: [/usr/lib/systemd/system/vault.service:8] Unknown lvalue 'StartLimitBurst' in section 'Unit'
    ```


<deL>
- 既存のVaultデータディレクトリをバックアップします（必要な場合）。
</del>

- Vaultの初期化コマンドを実行して新しい初期化トークンを生成します

- vault を初期化する

    ```
    # vault operator init
    Get "https://127.0.0.1:8200/v1/sys/seal-status": dial tcp 127.0.0.1:8200: connect: connection refused    
    ```

- IPを変えると初期化もできない
- インストールしなおすしかない


# 【これが成功した手順】Vault をインストールしなおし（IPは、 10.1.1.200） 


## インストール手順

- 以下の公式サイトを参考にした

    https://developer.hashicorp.com/vault/tutorials/getting-started/getting-started-install

- 以下のコマンドを実行

    ```sh
    $ yum install -y yum-utils
    ```
    - 結果

        ```
        読み込んだプラグイン:fastestmirror, langpacks
        Loading mirror speeds from cached hostfile
        * base: ftp-srv2.kddilabs.jp
        * elrepo: ftp.yz.yamagata-u.ac.jp
        * extras: ftp-srv2.kddilabs.jp
        * updates: ftp-srv2.kddilabs.jp
        base                                                                                                                                                                     | 3.6 kB  00:00:00     
        docker-ce-stable                                                                                                                                                         | 3.5 kB  00:00:00     
        elrepo                                                                                                                                                                   | 3.0 kB  00:00:00     
        extras                                                                                                                                                                   | 2.9 kB  00:00:00     
        ius                                                                                                                                                                      | 1.3 kB  00:00:00     
        updates                                                                                                                                                                  | 2.9 kB  00:00:00     
        docker-ce-stable/7/x86_64/primary_db                                                                                                                                     | 117 kB  00:00:00     
        パッケージ yum-utils-1.1.31-54.el7_8.noarch はインストール済みか最新バージョンです
        何もしません
        ```

    ```sh
    $ yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
    ```

    - 結果

        ```
        読み込んだプラグイン:fastestmirror, langpacks
        adding repo from: https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
        grabbing file https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo to /etc/yum.repos.d/hashicorp.repo
        repo saved to /etc/yum.repos.d/hashicorp.repo
        ```

    ```
    $ yum -y install vault
    ```

    - 結果

        ```
        読み込んだプラグイン:fastestmirror, langpacks
        Loading mirror speeds from cached hostfile
        * base: ftp-srv2.kddilabs.jp
        * elrepo: ftp.yz.yamagata-u.ac.jp
        * extras: ftp-srv2.kddilabs.jp
        * updates: ftp-srv2.kddilabs.jp
        hashicorp                                                                                                                                   | 1.4 kB  00:00:00     
        hashicorp/7/x86_64/primary                                                                                                                  | 182 kB  00:00:00     
        hashicorp                                                                                                                                                1314/1314
        依存性の解決をしています
        --> トランザクションの確認を実行しています。
        ---> パッケージ vault.x86_64 0:1.14.2-1 を インストール
        --> 依存性解決を終了しました。

        依存性を解決しました

        ===================================================================================================================================================================
        Package                              アーキテクチャー                      バージョン                              リポジトリー                              容量
        ===================================================================================================================================================================
        インストール中:
        vault                                x86_64                                1.14.2-1                                hashicorp                                129 M

        トランザクションの要約
        ===================================================================================================================================================================
        インストール  1 パッケージ

        総ダウンロード容量: 129 M
        インストール容量: 354 M
        Downloading packages:
        警告: /var/cache/yum/x86_64/7/hashicorp/packages/vault-1.14.2-1.x86_64.rpm: ヘッダー V4 RSA/SHA256 Signature、鍵 ID a621e701: NOKEY 19 MB/s | 127 MB  00:00:00 ETA 
        vault-1.14.2-1.x86_64.rpm の公開鍵がインストールされていません
        vault-1.14.2-1.x86_64.rpm                                                                                                                   | 129 MB  00:00:07     
        https://rpm.releases.hashicorp.com/gpg から鍵を取得中です。
        Importing GPG key 0xA621E701:
        Userid     : "HashiCorp Security (HashiCorp Package Signing) <security+packaging@hashicorp.com>"
        Fingerprint: 798a ec65 4e5c 1542 8c8e 42ee aa16 fcbc a621 e701
        From       : https://rpm.releases.hashicorp.com/gpg
        Running transaction check
        Running transaction test
        Transaction test succeeded
        Running transaction
        インストール中          : vault-1.14.2-1.x86_64                                                                                                              1/1Generating Vault TLS key and self-signed certificate...
        Generating a 4096 bit RSA private key
        .........................++
        ..........................++
        writing new private key to 'tls.key'
        -----
        Vault TLS key and self-signed certificate have been generated in '/opt/vault/tls'.
        検証中                  : vault-1.14.2-1.x86_64                                                                                                              1/1 

        インストール:
        vault.x86_64 0:1.14.2-1                                                                                                                                          

        完了しました!     
        ```

- インストールの検証

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

    ```diff
    # diff -u  /etc/vault.d/vault.hcl_org /etc/vault.d/vault.hcl
    --- /etc/vault.d/vault.hcl_org  2023-09-09 23:45:24.316034782 +0900
    +++ /etc/vault.d/vault.hcl      2023-09-09 23:50:40.455861475 +0900
    @@ -6,7 +6,7 @@
    ui = true
    
    #mlock = true
    -#disable_mlock = true
    +disable_mlock = true
    
    storage "file" {
    path = "/opt/vault/data"
    @@ -18,17 +18,18 @@
    #}
    
    # HTTP listener
    -#listener "tcp" {
    +listener "tcp" {
    #  address = "127.0.0.1:8200"
    -#  tls_disable = 1
    -#}
    +  address = "10.1.1.200:8200"
    +  tls_disable = 1
    +}
    
    # HTTPS listener
    -listener "tcp" {
    -  address       = "0.0.0.0:8200"
    -  tls_cert_file = "/opt/vault/tls/tls.crt"
    -  tls_key_file  = "/opt/vault/tls/tls.key"
    -}
    +#listener "tcp" {
    +#  address       = "0.0.0.0:8200"
    +#  tls_cert_file = "/opt/vault/tls/tls.crt"
    +#  tls_key_file  = "/opt/vault/tls/tls.key"
    +#}
    
    # Enterprise license_path
    # This will be required for enterprise as of v1.8
    @@ -48,3 +49,7 @@
    #  key_label      = "vault-hsm-key"
    #  hmac_key_label = "vault-hsm-hmac-key"
    #}
    +
    +cluster_addr  = "http://10.1.1.200:8201"
    +api_addr      = "http://10.1.1.200:8200"
    +
    ```

## vaultのセットアップ

- 以下のサイトを参考にしている

    https://gammalab.net/blog/3f7pudgk4zbcr/


- 以下のコマンドを実行

    ```
    $ netstat -tuln | grep 8200 | wc -l
    0
    ```

- vaultコマンドに今どこのvaultを設定するのかってのを教えてやる。それは環境変数です
- HTTPにする

    ```sh
    $ export VAULT_ADDR='http://10.1.1.200:8200'
    ```

    ```sh
    $ echo $VAULT_ADDR 
    http://10.1.1.200:8200
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
        Active: active (running) since 土 2023-09-09 23:59:37 JST; 7s ago
            Docs: https://www.vaultproject.io/docs/
        Main PID: 17367 (vault)
            Tasks: 8
        Memory: 20.3M
        CGroup: /system.slice/vault.service
                └─17367 /usr/bin/vault server -config=/etc/vault.d/vault.hcl

        9月 09 23:59:37 control-plane.minikube.internal vault[17367]: Recovery Mode: false
        9月 09 23:59:37 control-plane.minikube.internal vault[17367]: Storage: file
        9月 09 23:59:37 control-plane.minikube.internal vault[17367]: Version: Vault v1.14.2, built 2023-08-24T13:19:12Z
        9月 09 23:59:37 control-plane.minikube.internal vault[17367]: Version Sha: 16a7033a0686eca50ee650880d5c55438d274489
        9月 09 23:59:37 control-plane.minikube.internal vault[17367]: ==> Vault server started! Log data will stream in below:
        9月 09 23:59:37 control-plane.minikube.internal vault[17367]: 2023-09-09T23:59:37.323+0900 [INFO]  proxy environment: http_proxy="" https_proxy="" no_proxy=""
        9月 09 23:59:37 control-plane.minikube.internal vault[17367]: 2023-09-09T23:59:37.324+0900 [INFO]  core: Initializing version history cache for core
        9月 09 23:59:43 control-plane.minikube.internal vault[17367]: 2023-09-09T23:59:43.277+0900 [INFO]  core: security barrier not initialized
        9月 09 23:59:43 control-plane.minikube.internal vault[17367]: 2023-09-09T23:59:43.277+0900 [INFO]  core: security barrier not initialized
        9月 09 23:59:43 control-plane.minikube.internal vault[17367]: 2023-09-09T23:59:43.280+0900 [INFO]  core: seal configuration missing, not initialized        
        ```

    ```sh
    $ netstat -tuln | grep 8200
    tcp        0      0 10.1.1.200:8200         0.0.0.0:*               LISTEN 
    ```
## vault を初期化

- 以下のコマンドを実行

    ```
    # vault operator init
    ```

    - 結果
    - この情報は超大事なので超大事に保管してください。
    - unseal keyが金庫そのものの凍結を解除するための鍵で、Root TokenがRootユーザー(ロール)としてログインんするのに必要な鍵です。


    - 結果

        ```
        Unseal Key 1: tYN2ZXR6UOJKiMZzhQvu1nZ+5bymI/B8nSM0zQBlP+cH
        Unseal Key 2: zRm7x5T4yjtLWTwwwWaY5K/YL/dplOd+KQ6VyykVeCFH
        Unseal Key 3: 7xXvhns+T4hp8YT4Pd38EqmURNIU20o92itb8PTNmlt4
        Unseal Key 4: Y7iHvD3EVISub6uSjqt4aVxtnC+0B8OF7m6TXmYL5f9+
        Unseal Key 5: K9/xHj95ermVqZTnQlk8ZHJ4xtu4e6d0x+ylMKB90G4r

        Initial Root Token: hvs.AkzMQJ2dOofPjOjsLK7pzmWz

        Vault initialized with 5 key shares and a key threshold of 3. Please securely
        distribute the key shares printed above. When the Vault is re-sealed,
        restarted, or stopped, you must supply at least 3 of these keys to unseal it
        before it can start servicing requests.

        Vault does not store the generated root key. Without at least 3 keys to
        reconstruct the root key, Vault will remain permanently sealed!

        It is possible to generate new unseal keys, provided you have a quorum of
        existing unseal keys shares. See "vault operator rekey" for more information.
        ```

## vault unseal


- 以下のコマンドを実行（5つのUnseal Keyの内、3つのUnseal Keyを3回に分け入力）

    ```sh
    $ vault operator unseal
    ```

-   Root Tokenを使ってログイン

    ```sh
    $ vault login
    ```

## Concourse用のロールとtokenを作成

- 以下のコマンドを実行

    ```sh
    $ cd /root/vault-install
    ```

    ```sh
    $ cat << EOF > concourse-policy.hcl
    path "concourse/*" {
            capabilities = ["read"]
    }   
    EOF
    ```

    ```sh
    $ cat concourse-policy.hcl
    path "concourse/*" {
            capabilities = ["read"]
    } 
    ```

- そしたらそのファイルを読み込みます。

    ```sh
    $ vault policy write concourse ./concourse-policy.hcl
    ```

    - 結果

        ```sh
        Success! Uploaded policy: concourse
        ```

    ```sh
    $ vault policy read concourse
    ```

    - 結果

        ```sh
        path "concourse/*" {
        capabilities = ["read"]
        }
        ```

    - これでconcourseポリシーを作ることができました。

- そしたらconcourseポリシーに基づいたtokenを発行します。

    ```
    $ vault token create --policy concourse
    ```

    - 結果

        ```
        Key                  Value
        ---                  -----
        token                hvs.CAESIC9wzsgw0a0W6cq1LG1UcqofJAcSxgzFEMrBnDTyECs8Gh4KHGh2cy5UQUZ2NHVINm1wb2pLNWRVSWNrQzNXUG8
        token_accessor       foHbcXNm4bBw7aq8qMK8nj2b
        token_duration       768h
        token_renewable      true
        token_policies       ["concourse" "default"]
        identity_policies    []
        policies             ["concourse" "default"]
        ```

- 以下のサイトを参考にした

    https://gammalab.net/blog/3f7pudgk4zbcr/

    https://concourse-ci.org/vault-credential-manager.html

- 先程のトークンをconcourseCIに設定します！docker-composeファイルに以下を追記するだけです。
- To configure this, first configure the URL of your Vault server by setting the following env on the web node

    ```sh
    $ cd ~/concourse-install
    ```

    ```sh
    $ cat docker-compose.yml
    ```

        ```sh
        version: '3'

        services:
        db:
            image: postgres
            environment:
            POSTGRES_DB: concourse
            POSTGRES_USER: concourse_user
            POSTGRES_PASSWORD: concourse_pass
            logging:
            driver: "json-file"
            options:
                max-file: "5"
                max-size: "10m"

        web:
            image: concourse/concourse
            command: web
            links: [db]
            depends_on: [db]
            ports: ["8080:8080"]
            volumes: ["./keys/web:/concourse-keys"]
            environment:
            CONCOURSE_EXTERNAL_URL: http://localhost:8080
            CONCOURSE_POSTGRES_HOST: db
            CONCOURSE_POSTGRES_USER: concourse_user
            CONCOURSE_POSTGRES_PASSWORD: concourse_pass
            CONCOURSE_POSTGRES_DATABASE: concourse
            CONCOURSE_ADD_LOCAL_USER: test:test
            CONCOURSE_MAIN_TEAM_LOCAL_USER: test
            CONCOURSE_VAULT_URL: http://10.1.1.200:8200
            CONCOURSE_VAULT_CLIENT_TOKEN: hvs.CAESIC9wzsgw0a0W6cq1LG1UcqofJAcSxgzFEMrBnDTyECs8Gh4KHGh2cy5UQUZ2NHVINm1wb2pLNWRVSWNrQzNXUG8

            logging:
            driver: "json-file"
            options:
                max-file: "5"
                max-size: "10m"

        worker:
            image: concourse/concourse
            command: worker
            privileged: true
            depends_on: [web]
            volumes: ["./keys/worker:/concourse-keys"]
            links: [web]
            stop_signal: SIGUSR2
            environment:
            CONCOURSE_TSA_HOST: web:2222
            # enable DNS proxy to support Docker's 127.x.x.x DNS server
            CONCOURSE_GARDEN_DNS_PROXY_ENABLE: "true"
            logging:
            driver: "json-file"
            options:
                max-file: "5"
                max-size: "10m"
        ```


- concourse を再起動

    ```sh
    $ docker-compose down
    ```

    ```sh
    $ docker-compose up -d
    ```

# Concourse用のシークレットを作成

- 以下のコマンドを実行

    ```sh
    $ vault secrets enable -version=1 -path concourse kv
    ```

    - 結果

        ```
        Success! Enabled the kv secrets engine at: concourse/
        ```

    - これでconcourse/以下に自由にkv型のデータを配置できるようになりました。

- 参考にしているサイト

    https://gammalab.net/blog/3f7pudgk4zbcr/

- のサンプルがよくわからないので、concourse の日本語チュートリアルでハンズオンを行ってみる
- 参考にしたサイトは以下（Docker イメージの作成・利用）

    https://concoursetutorial-ja.site.lkj.io/miscellaneous/docker-images


## Docker イメージの作成・利用（Vault版）

- 参考にしたサイトは以下（Docker イメージの作成・利用）

    https://concoursetutorial-ja.site.lkj.io/miscellaneous/docker-images

- 上記サイトの内容をVault で実装してみる

- 以下のコマンドを実行し、`pipeline.yml`の内容を確認

    ```sh
    $ cd ~/concourse-tutorial/tutorials/miscellaneous/docker-images/
    ```

    ```sh
    $ cat pipeline.yml 
    ---

    - 結果

        ```
        ---
        resources:
        - name: tutorial
            type: git
            source:
            uri: https://github.com/drnic/concourse-tutorial.git
            branch: develop

        - name: hello-world-docker-image
            type: docker-image
            source:
            email: ((docker-hub-email))
            username: ((docker-hub-username))
            password: ((docker-hub-password))
            repository: ((docker-hub-username))/concourse-tutorial-hello-world

        jobs:
        - name: publish
            public: true
            plan:
            - get: tutorial
            - put: hello-world-docker-image
                params:
                build: tutorial/tutorials/miscellaneous/docker-images/docker
            - task: run
                config:
                platform: linux
                image_resource:
                    type: docker-image
                    source:
                    repository: ((docker-hub-username))/concourse-tutorial-hello-world
                run:
                    path: /bin/hello-world
                    args: []
                params:
                    NAME: ((docker-hub-username))
        ```

- 資格管理マネージャにパラメータを追加する

    ```
    # vault kv put concourse/main/push-docker-image/docker-hub-email value=moriyama.kazuhiro@earthsys-lab.co.jp
    ```

    - 結果

        ```sh
        Success! Data written to: concourse/main/push-docker-image/docker-hub-email
        ```

    ```
    # vault kv put concourse/main/push-docker-image/docker-hub-username value=kuzumusen
    ```

    - 結果

        ```sh
        Success! Data written to: concourse/main/push-docker-image/docker-hub-username
        ```

    ```
    # vault kv put concourse/main/push-docker-image/docker-hub-password value=z@yaNa053
    ```

    - 結果

        ```sh
        Success! Data written to: concourse/main/push-docker-image/docker-hub-password        
        ```

- コマンドを実行する（Concourse CI のWebUIへのログイン）

    ```sh
    $ fly --target tutorial login --concourse-url http://localhost:8080
    ```

    - 操作
    - `http://localhost:8080/login?fly_port=43269` でホストOSのブラウザにアクセスし、表示されたtokenを貼り付ける
    - ユーザID: `test`、パスワード: `test` とする
    - 上記操作をすると、Concourse CI のWeb UIにログインできる

        ```
        logging in to team 'main'

        navigate to the following URL in your browser:

        http://localhost:8080/login?fly_port=43269

        or enter token manually (input hidden): 
        target saved
        ```

- パイプラインを削除する

    ```sh
    $ fly -t tutorial destroy-pipeline -p push-docker-image -n
    ```

- パイプラインを作成

    ```sh
    $ fly -t tutorial set-pipeline -p push-docker-image -c pipeline.yml -n
    ```

    - 結果

        ```
        resources:
        resource tutorial has been added:
        + name: tutorial
        + source:
        +   branch: develop
        +   uri: https://github.com/drnic/concourse-tutorial.git
        + type: git
        
        resource hello-world-docker-image has been added:
        + name: hello-world-docker-image
        + source:
        +   email: ((docker-hub-email))
        +   password: ((docker-hub-password))
        +   repository: ((docker-hub-username))/concourse-tutorial-hello-world
        +   username: ((docker-hub-username))
        + type: docker-image
        
        jobs:
        job publish has been added:
        + name: publish
        + plan:
        + - get: tutorial
        + - params:
        +     build: tutorial/tutorials/miscellaneous/docker-images/docker
        +   put: hello-world-docker-image
        + - config:
        +     image_resource:
        +       name: ""
        +       source:
        +         repository: ((docker-hub-username))/concourse-tutorial-hello-world
        +       type: docker-image
        +     params:
        +       NAME: ((docker-hub-username))
        +     platform: linux
        +     run:
        +       path: /bin/hello-world
        +   task: run
        + public: true
        
        pipeline name: push-docker-image

        pipeline created!
        you can view your pipeline here: http://localhost:8080/teams/main/pipelines/push-docker-image

        the pipeline is currently paused. to unpause, either:
        - run the unpause-pipeline command:
            fly -t tutorial unpause-pipeline -p push-docker-image
        - click play next to the pipeline in the web ui
        ```

- パイプラインのリソースをチェック

    ```
    $ fly -t tutorial check-resource -r push-docker-image/tutorial
    ```

    - 結果

        ```
        checking push-docker-image/tutorial in build 2
        initializing check: tutorial
        selected worker: f464a80266c7
        Cloning into '/tmp/git-resource-repo-cache'...
        succeeded 
        ```

    ```sh
    $ fly -t tutorial check-resource -r push-docker-image/hello-world-docker-image
    ```

    - 結果（成功した！！）

        ```sh
        checking push-docker-image/hello-world-docker-image in build 1
        initializing check: hello-world-docker-image
        selected worker: f464a80266c7
        succeeded
        ```

- <span style="color: red; ">上手くいかない原因は、Vault のIPアドレスを '127.0.0.1' にしていたためであった！！！</span>

- パイプラインを実行

    ```sh
    $ fly -t tutorial unpause-pipeline -p push-docker-image
    ```

    ```sh
    $ fly -t tutorial trigger-job -j push-docker-image/publish -w
    ```

    - 結果

        ```
        started push-docker-image/publish #1

        selected worker: f464a80266c7
        Cloning into '/tmp/build/get'...
        a3edcb3 restrict mkdocs packages until can make time to upgrade https://ci2.starkandwayne.com/teams/starkandwayne/pipelines/concourse-tutorial/jobs/website-master/builds/32
        selected worker: f464a80266c7
        waiting for docker to come up...
        WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
        Configure a credential helper to remove this warning. See
        https://docs.docker.com/engine/reference/commandline/login/#credentials-store

        Login Succeeded
        DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
                    BuildKit is currently disabled; enable it by removing the DOCKER_BUILDKIT=0
                    environment-variable.

        Sending build context to Docker daemon  3.072kB
        Step 1/4 : FROM busybox
        latest: Pulling from library/busybox
        3f4d90098f5b: Pulling fs layer
        3f4d90098f5b: Verifying Checksum
        3f4d90098f5b: Download complete
        3f4d90098f5b: Pull complete
        Digest: sha256:3fbc632167424a6d997e74f52b878d7cc478225cffac6bc977eedfe51c7f4e79
        Status: Downloaded newer image for busybox:latest
        ---> a416a98b71e2
        Step 2/4 : ADD hello-world /bin/hello-world
        ---> 716d042dc3d6
        Step 3/4 : ENV NAME=world
        ---> Running in 0e0a884b4eb9
        Removing intermediate container 0e0a884b4eb9
        ---> 826457d4a3d9
        Step 4/4 : ENTRYPOINT ["/bin/hello-world"]
        ---> Running in a0c07fde1024
        Removing intermediate container a0c07fde1024
        ---> 979325d18092
        Successfully built 979325d18092
        Successfully tagged kuzumusen/concourse-tutorial-hello-world:latest
        The push refers to repository [docker.io/kuzumusen/concourse-tutorial-hello-world]
        fe220aae2b74: Preparing
        3d24ee258efc: Preparing
        3d24ee258efc: Mounted from library/busybox
        fe220aae2b74: Pushed
        latest: digest: sha256:0c7e17470159c7b034b281083e38cb717b1cf32b6e720757b9913e80941d44c6 size: 735
        selected worker: f464a80266c7
        waiting for docker to come up...
        WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
        Configure a credential helper to remove this warning. See
        https://docs.docker.com/engine/reference/commandline/login/#credentials-store

        Login Succeeded
        Pulling kuzumusen/concourse-tutorial-hello-world@sha256:0c7e17470159c7b034b281083e38cb717b1cf32b6e720757b9913e80941d44c6...
        docker.io/kuzumusen/concourse-tutorial-hello-world@sha256:0c7e17470159c7b034b281083e38cb717b1cf32b6e720757b9913e80941d44c6: Pulling from kuzumusen/concourse-tutorial-hello-world
        3f4d90098f5b: Pulling fs layer
        ab73f1c7c633: Pulling fs layer
        3f4d90098f5b: Download complete
        3f4d90098f5b: Pull complete
        ab73f1c7c633: Download complete
        ab73f1c7c633: Pull complete
        Digest: sha256:0c7e17470159c7b034b281083e38cb717b1cf32b6e720757b9913e80941d44c6
        Status: Downloaded newer image for kuzumusen/concourse-tutorial-hello-world@sha256:0c7e17470159c7b034b281083e38cb717b1cf32b6e720757b9913e80941d44c6
        docker.io/kuzumusen/concourse-tutorial-hello-world@sha256:0c7e17470159c7b034b281083e38cb717b1cf32b6e720757b9913e80941d44c6

        Successfully pulled kuzumusen/concourse-tutorial-hello-world@sha256:0c7e17470159c7b034b281083e38cb717b1cf32b6e720757b9913e80941d44c6.

        initializing
        initializing check: image
        selected worker: f464a80266c7
        selected worker: f464a80266c7
        waiting for docker to come up...
        Pulling kuzumusen/concourse-tutorial-hello-world@sha256:0c7e17470159c7b034b281083e38cb717b1cf32b6e720757b9913e80941d44c6...
        docker.io/kuzumusen/concourse-tutorial-hello-world@sha256:0c7e17470159c7b034b281083e38cb717b1cf32b6e720757b9913e80941d44c6: Pulling from kuzumusen/concourse-tutorial-hello-world
        3f4d90098f5b: Pulling fs layer
        ab73f1c7c633: Pulling fs layer
        3f4d90098f5b: Verifying Checksum
        3f4d90098f5b: Download complete
        ab73f1c7c633: Verifying Checksum
        ab73f1c7c633: Download complete
        3f4d90098f5b: Pull complete
        ab73f1c7c633: Pull complete
        Digest: sha256:0c7e17470159c7b034b281083e38cb717b1cf32b6e720757b9913e80941d44c6
        Status: Downloaded newer image for kuzumusen/concourse-tutorial-hello-world@sha256:0c7e17470159c7b034b281083e38cb717b1cf32b6e720757b9913e80941d44c6
        docker.io/kuzumusen/concourse-tutorial-hello-world@sha256:0c7e17470159c7b034b281083e38cb717b1cf32b6e720757b9913e80941d44c6

        Successfully pulled kuzumusen/concourse-tutorial-hello-world@sha256:0c7e17470159c7b034b281083e38cb717b1cf32b6e720757b9913e80941d44c6.

        selected worker: f464a80266c7
        running /bin/hello-world
        hello kuzumusen
        succeeded
        ```


## approle認証バックエンドの使用

- 定期トークンは最も簡単に開始できる方法ですが、致命的な欠陥が 1 つあります。ノードがwebトークンの構成期間よりも長くダウンしている場合、トークンが期限切れになり、新しいトークンを作成して構成する必要があります。approleこれは、認証バックエンドを使用することで回避できます。 

- 以下のサイトを参考にしている

    [公式（このサイトを主に参照）](https://concourse-ci.org/vault-credential-manager.html)

    https://ik.am/entries/432


- vaultコマンドに今どこのvaultを設定するのかってのを教えてやる。それは環境変数です
- HTTPにする

    ```sh
    $ export VAULT_ADDR='http://10.1.1.200:8200'
    ```

    ```sh
    $ echo $VAULT_ADDR 
    http://10.1.1.200:8200
    ```

    - vault を起動する。

    ```
    $ sudo systemctl start vault.service
    ```

    ```
    $ sudo systemctl status vault.service
    ```

    - 結果

        ```sh
        ● vault.service - "HashiCorp Vault - A tool for managing secrets"
        Loaded: loaded (/usr/lib/systemd/system/vault.service; disabled; vendor preset: disabled)
        Active: active (running) since 日 2023-09-10 09:18:22 JST; 11s ago
            Docs: https://www.vaultproject.io/docs/
        Main PID: 6140 (vault)
            Tasks: 7
        Memory: 250.6M
        CGroup: /system.slice/vault.service
                └─6140 /usr/bin/vault server -config=/etc/vault.d/vault.hcl

        9月 10 09:18:22 control-plane.minikube.internal vault[6140]: Log Level:
        9月 10 09:18:22 control-plane.minikube.internal vault[6140]: Mlock: supported: true, enabled: false
        9月 10 09:18:22 control-plane.minikube.internal vault[6140]: Recovery Mode: false
        9月 10 09:18:22 control-plane.minikube.internal vault[6140]: Storage: file
        9月 10 09:18:22 control-plane.minikube.internal vault[6140]: Version: Vault v1.14.2, built 2023-08-24T13:19:12Z
        9月 10 09:18:22 control-plane.minikube.internal vault[6140]: Version Sha: 16a7033a0686eca50ee650880d5c55438d274489
        9月 10 09:18:22 control-plane.minikube.internal vault[6140]: ==> Vault server started! Log data will stream in below:
        9月 10 09:18:22 control-plane.minikube.internal vault[6140]: 2023-09-10T09:18:22.165+0900 [INFO]  proxy environment: http_proxy="" https_...oxy=""
        9月 10 09:18:22 control-plane.minikube.internal vault[6140]: 2023-09-10T09:18:22.176+0900 [INFO]  core: Initializing version history cach...r core
        9月 10 09:18:22 control-plane.minikube.internal systemd[1]: Started "HashiCorp Vault - A tool for managing secrets".
        Hint: Some lines were ellipsized, use -l to show in full.
        ```

    ```sh
    $ netstat -tuln | grep 8200
    tcp        0      0 10.1.1.200:8200         0.0.0.0:*               LISTEN 
    ```

## vault を初期化はしない

## vault unsealする


- unsealしないとエラーになる

    ```sh
    $ vault auth enable approle
    Error enabling approle auth: Error making API request.

    URL: POST http://10.1.1.200:8200/v1/sys/auth/approle
    Code: 503. Errors:

    * Vault is sealed
    ```

- ちなみに、前の手順より、Unseal Key と Root Token　は以下

    ```sh
    Unseal Key 1: tYN2ZXR6UOJKiMZzhQvu1nZ+5bymI/B8nSM0zQBlP+cH
    Unseal Key 2: zRm7x5T4yjtLWTwwwWaY5K/YL/dplOd+KQ6VyykVeCFH
    Unseal Key 3: 7xXvhns+T4hp8YT4Pd38EqmURNIU20o92itb8PTNmlt4
    Unseal Key 4: Y7iHvD3EVISub6uSjqt4aVxtnC+0B8OF7m6TXmYL5f9+
    Unseal Key 5: K9/xHj95ermVqZTnQlk8ZHJ4xtu4e6d0x+ylMKB90G4r

    Initial Root Token: hvs.AkzMQJ2dOofPjOjsLK7pzmWz
    ```


- 以下のコマンドを実行（5つのUnseal Keyの内、3つのUnseal Keyを3回に分け入力）

    ```sh
    $ vault operator unseal
    ```

## approle認証バックエンドの設定

- 以下のコマンドを実行

    ```sh
    $ vault auth enable approle
    ```

    - 結果

        ```sh
        Success! Enabled approle auth method at: approle/
        ```

    ```sh
    $ vault write auth/approle/role/concourse policies=concourse period=1h 
    ```

    - 結果

        ```sh
        Success! Data written to: auth/approle/role/concourse
        ```

    ```sh
    $ vault read auth/approle/role/concourse/role-id
    ```

    - 結果

        ```sh
        Key        Value
        ---        -----
        role_id    2bf6997c-c123-5e25-3e22-ebe3c539ff16 
        ```

    ```sh
    $ vault write -f auth/approle/role/concourse/secret-id
    ```

    - 結果

        ```sh
        Key                   Value
        ---                   -----
        secret_id             f9189289-baf8-368f-088a-c8c14bd484ea
        secret_id_accessor    49bb32d4-3bab-9eb2-4cba-5b9f4dae0db3
        secret_id_num_uses    0
        secret_id_ttl         0s
        ```

## 

- 先程のトークンをconcourseCIに設定します！docker-composeファイルに以下を追記するだけです。
- To configure this, first configure the URL of your Vault server by setting the following env on the web node

    ```sh
    $ cd ~/concourse-install
    ```

 - `docker-compose.yml`の修正内容

    ```diff
    $ diff -u docker-compose.yml_old docker-compose.yml
    --- docker-compose.yml_old      2023-09-10 09:54:42.557330403 +0900
    +++ docker-compose.yml  2023-09-10 09:59:09.584857957 +0900
    @@ -29,7 +29,8 @@
        CONCOURSE_ADD_LOCAL_USER: test:test
        CONCOURSE_MAIN_TEAM_LOCAL_USER: test
        CONCOURSE_VAULT_URL: http://10.1.1.200:8200
    -      CONCOURSE_VAULT_CLIENT_TOKEN: hvs.CAESIC9wzsgw0a0W6cq1LG1UcqofJAcSxgzFEMrBnDTyECs8Gh4KHGh2cy5UQUZ2NHVINm1wb2pLNWRVSWNrQzNXUG8
    +      CONCOURSE_VAULT_AUTH_BACKEND: "approle"
    +      CONCOURSE_VAULT_AUTH_PARAM: "role_id:2bf6997c-c123-5e25-3e22-ebe3c539ff16,secret_id:f9189289-baf8-368f-088a-c8c14bd484ea"
    
        logging:
        driver: "json-file"
    ```

    ```sh
    $ cat docker-compose.yml
    ```

    - 結果

        ```sh
        version: '3'

        services:
        db:
            image: postgres
            environment:
            POSTGRES_DB: concourse
            POSTGRES_USER: concourse_user
            POSTGRES_PASSWORD: concourse_pass
            logging:
            driver: "json-file"
            options:
                max-file: "5"
                max-size: "10m"

        web:
            image: concourse/concourse
            command: web
            links: [db]
            depends_on: [db]
            ports: ["8080:8080"]
            volumes: ["./keys/web:/concourse-keys"]
            environment:
            CONCOURSE_EXTERNAL_URL: http://localhost:8080
            CONCOURSE_POSTGRES_HOST: db
            CONCOURSE_POSTGRES_USER: concourse_user
            CONCOURSE_POSTGRES_PASSWORD: concourse_pass
            CONCOURSE_POSTGRES_DATABASE: concourse
            CONCOURSE_ADD_LOCAL_USER: test:test
            CONCOURSE_MAIN_TEAM_LOCAL_USER: test
            CONCOURSE_VAULT_URL: http://10.1.1.200:8200
            CONCOURSE_VAULT_AUTH_BACKEND: "approle"
            CONCOURSE_VAULT_AUTH_PARAM: "role_id:2bf6997c-c123-5e25-3e22-ebe3c539ff16,secret_id:f9189289-baf8-368f-088a-c8c14bd484ea"

            logging:
            driver: "json-file"
            options:
                max-file: "5"
                max-size: "10m"

        worker:
            image: concourse/concourse
            command: worker
            privileged: true
            depends_on: [web]
            volumes: ["./keys/worker:/concourse-keys"]
            links: [web]
            stop_signal: SIGUSR2
            environment:
            CONCOURSE_TSA_HOST: web:2222
            # enable DNS proxy to support Docker's 127.x.x.x DNS server
            CONCOURSE_GARDEN_DNS_PROXY_ENABLE: "true"
            logging:
            driver: "json-file"
            options:
                max-file: "5"
                max-size: "10m"
        ```

- concourse を再起動

    ```sh
    $ docker-compose down
    ```

    ```sh
    $ docker-compose up -d
    ```

- コマンドを実行する（Concourse CI のWebUIへのログイン）

    ```sh
    $ fly --target tutorial login --concourse-url http://localhost:8080
    ```

    - 操作
    - `http://localhost:8080/login?fly_port=43269` でホストOSのブラウザにアクセスし、表示されたtokenを貼り付ける
    - ユーザID: `test`、パスワード: `test` とする
    - 上記操作をすると、Concourse CI のWeb UIにログインできる

        ```
        logging in to team 'main'

        navigate to the following URL in your browser:

        http://localhost:8080/login?fly_port=35283

        or enter token manually (input hidden): 
        target saved
        ```

- パイプラインを削除する

    ```sh
    $ fly -t tutorial destroy-pipeline -p push-docker-image -n
    ```

- パイプラインを作成

    ```sh
    $ cd ~/concourse-tutorial/tutorials/miscellaneous/docker-images/
    ```


    ```sh
    $ fly -t tutorial set-pipeline -p push-docker-image -c pipeline.yml -n
    ```

    - 結果

        ```sh
        resources:
        resource tutorial has been added:
        + name: tutorial
        + source:
        +   branch: develop
        +   uri: https://github.com/drnic/concourse-tutorial.git
        + type: git
        
        resource hello-world-docker-image has been added:
        + name: hello-world-docker-image
        + source:
        +   email: ((docker-hub-email))
        +   password: ((docker-hub-password))
        +   repository: ((docker-hub-username))/concourse-tutorial-hello-world
        +   username: ((docker-hub-username))
        + type: docker-image
        
        jobs:
        job publish has been added:
        + name: publish
        + plan:
        + - get: tutorial
        + - params:
        +     build: tutorial/tutorials/miscellaneous/docker-images/docker
        +   put: hello-world-docker-image
        + - config:
        +     image_resource:
        +       name: ""
        +       source:
        +         repository: ((docker-hub-username))/concourse-tutorial-hello-world
        +       type: docker-image
        +     params:
        +       NAME: ((docker-hub-username))
        +     platform: linux
        +     run:
        +       path: /bin/hello-world
        +   task: run
        + public: true
        
        pipeline name: push-docker-image

        pipeline created!
        you can view your pipeline here: http://localhost:8080/teams/main/pipelines/push-docker-image

        the pipeline is currently paused. to unpause, either:
        - run the unpause-pipeline command:
            fly -t tutorial unpause-pipeline -p push-docker-image
        - click play next to the pipeline in the web ui
        ```

- パイプラインのリソースをチェック

    ```
    $ fly -t tutorial check-resource -r push-docker-image/tutorial
    ```

    - 結果

        ```sh
        checking push-docker-image/tutorial in build 2
        initializing check: tutorial
        selected worker: 367e9bba35ad
        Cloning into '/tmp/git-resource-repo-cache'...
        succeeded
        ```

    ```sh
    $ fly -t tutorial check-resource -r push-docker-image/hello-world-docker-image
    ```

    - 結果（成功）

        ```sh
        checking push-docker-image/hello-world-docker-image in build 1
        initializing check: hello-world-docker-image
        selected worker: 367e9bba35ad
        succeeded            
        ```

- パイプラインを実行

    ```sh
    $ fly -t tutorial unpause-pipeline -p push-docker-image
    ```

    - 結果

        ```sh
        unpaused 'push-docker-image'
        ```

    ```sh
    $ fly -t tutorial trigger-job -j push-docker-image/publish -w
    ```

    - 結果（成功）

        ```sh
        started push-docker-image/publish #1

        selected worker: 367e9bba35ad
        Cloning into '/tmp/build/get'...
        a3edcb3 restrict mkdocs packages until can make time to upgrade https://ci2.starkandwayne.com/teams/starkandwayne/pipelines/concourse-tutorial/jobs/website-master/builds/32
        selected worker: 367e9bba35ad
        waiting for docker to come up...
        WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
        Configure a credential helper to remove this warning. See
        https://docs.docker.com/engine/reference/commandline/login/#credentials-store

        Login Succeeded
        DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
                    BuildKit is currently disabled; enable it by removing the DOCKER_BUILDKIT=0
                    environment-variable.

        Sending build context to Docker daemon  3.072kB
        Step 1/4 : FROM busybox
        latest: Pulling from library/busybox
        3f4d90098f5b: Pulling fs layer
        3f4d90098f5b: Verifying Checksum
        3f4d90098f5b: Download complete
        3f4d90098f5b: Pull complete
        Digest: sha256:3fbc632167424a6d997e74f52b878d7cc478225cffac6bc977eedfe51c7f4e79
        Status: Downloaded newer image for busybox:latest
        ---> a416a98b71e2
        Step 2/4 : ADD hello-world /bin/hello-world
        ---> cb936a7205a9
        Step 3/4 : ENV NAME=world
        ---> Running in 53845428d0f9
        Removing intermediate container 53845428d0f9
        ---> 9c4bec084be3
        Step 4/4 : ENTRYPOINT ["/bin/hello-world"]
        ---> Running in c63526c40516
        Removing intermediate container c63526c40516
        ---> 976cc14f6779
        Successfully built 976cc14f6779
        Successfully tagged kuzumusen/concourse-tutorial-hello-world:latest
        The push refers to repository [docker.io/kuzumusen/concourse-tutorial-hello-world]
        110ac2d6958b: Preparing
        3d24ee258efc: Preparing
        3d24ee258efc: Mounted from library/busybox
        110ac2d6958b: Pushed
        latest: digest: sha256:ad795ce27d8effd0e42afad470a57282f2fb918a6e943ebe6253127c034be9cd size: 735
        selected worker: 367e9bba35ad
        waiting for docker to come up...
        WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
        Configure a credential helper to remove this warning. See
        https://docs.docker.com/engine/reference/commandline/login/#credentials-store

        Login Succeeded
        Pulling kuzumusen/concourse-tutorial-hello-world@sha256:ad795ce27d8effd0e42afad470a57282f2fb918a6e943ebe6253127c034be9cd...
        docker.io/kuzumusen/concourse-tutorial-hello-world@sha256:ad795ce27d8effd0e42afad470a57282f2fb918a6e943ebe6253127c034be9cd: Pulling from kuzumusen/concourse-tutorial-hello-world
        3f4d90098f5b: Pulling fs layer
        7afe31b2fdba: Pulling fs layer
        7afe31b2fdba: Download complete
        3f4d90098f5b: Verifying Checksum
        3f4d90098f5b: Download complete
        3f4d90098f5b: Pull complete
        7afe31b2fdba: Pull complete
        Digest: sha256:ad795ce27d8effd0e42afad470a57282f2fb918a6e943ebe6253127c034be9cd
        Status: Downloaded newer image for kuzumusen/concourse-tutorial-hello-world@sha256:ad795ce27d8effd0e42afad470a57282f2fb918a6e943ebe6253127c034be9cd
        docker.io/kuzumusen/concourse-tutorial-hello-world@sha256:ad795ce27d8effd0e42afad470a57282f2fb918a6e943ebe6253127c034be9cd

        Successfully pulled kuzumusen/concourse-tutorial-hello-world@sha256:ad795ce27d8effd0e42afad470a57282f2fb918a6e943ebe6253127c034be9cd.

        initializing
        initializing check: image
        selected worker: 367e9bba35ad
        selected worker: 367e9bba35ad
        waiting for docker to come up...
        Pulling kuzumusen/concourse-tutorial-hello-world@sha256:ad795ce27d8effd0e42afad470a57282f2fb918a6e943ebe6253127c034be9cd...
        docker.io/kuzumusen/concourse-tutorial-hello-world@sha256:ad795ce27d8effd0e42afad470a57282f2fb918a6e943ebe6253127c034be9cd: Pulling from kuzumusen/concourse-tutorial-hello-world
        3f4d90098f5b: Pulling fs layer
        7afe31b2fdba: Pulling fs layer
        7afe31b2fdba: Verifying Checksum
        7afe31b2fdba: Download complete
        3f4d90098f5b: Verifying Checksum
        3f4d90098f5b: Download complete
        3f4d90098f5b: Pull complete
        7afe31b2fdba: Pull complete
        Digest: sha256:ad795ce27d8effd0e42afad470a57282f2fb918a6e943ebe6253127c034be9cd
        Status: Downloaded newer image for kuzumusen/concourse-tutorial-hello-world@sha256:ad795ce27d8effd0e42afad470a57282f2fb918a6e943ebe6253127c034be9cd
        docker.io/kuzumusen/concourse-tutorial-hello-world@sha256:ad795ce27d8effd0e42afad470a57282f2fb918a6e943ebe6253127c034be9cd

        Successfully pulled kuzumusen/concourse-tutorial-hello-world@sha256:ad795ce27d8effd0e42afad470a57282f2fb918a6e943ebe6253127c034be9cd.

        selected worker: 367e9bba35ad
        running /bin/hello-world
        hello kuzumusen
        succeeded
        ```


# `~/concourse-tutorial/tutorials`をコピー

- 以下のコマンドを実行

    ```sh
    $ cp -r ~/concourse-tutorial/tutorials/ ~/concourse-install/
    ```

