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

- 以下のサイトを参考にして`/etc/vault.d/vault.hc1`を修正

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