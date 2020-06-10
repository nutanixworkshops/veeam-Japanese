.. title:: Veeam and Nutanix

.. toctree::
   :maxdepth: 2
   :caption: Veeam
   :name: _veeam
   :hidden:

   veeam/veeam

.. toctree::
   :maxdepth: 2
   :caption: Appendix
   :name: _appendix
   :hidden:

   tools_vms/windows_tools_vm
   tools_vms/linux_tools_vm
   taskman/taskman
   wordpress/wordpress


.. _getting_started:

---------------
はじめに
---------------

VeeamおよびNutanixアドオンラボへようこそ！ このワークブックには、NutanixとVeeamソフトウェアの連携について紹介するインストラクター主導のセッションが含まれています。

それでは早速VeeamとNutanixを使ってみましょう。

更新情報
++++++++++
- Workshopは以下のソフトウェアバージョンにアップデートされました:
    - AOS & PC 5.11.2.x

- Optional Lab Updates:


アジェンダ
++++++

- Veeam

自己紹介
+++++++++++++

- 名前
- Nutanixをどのぐらい使っているか

初期セットアップ
+++++++++++++

- パスワードがあること注意してください
- 仮想デスクトップにログインします（ログイン情報は下記にあります）

環境詳細
+++++++++++++++++++

Nutanix Workshopsは、Nutanix Hosted POC環境で実行することを目的としています。 演習を完了するために必要なすべてのイメージ、ネットワーク、VMがクラスターにプロビジョニングされます。

ネットワーク
..........

HPOCクラスターは標準の命名規則に従います:

- **Cluster Name** - POC\ *XYZ*
- **Subnet** - 10.**42**.\ *XYZ*\ .0
- **Cluster IP** - 10.**42**.\ *XYZ*\ .37

もしマーケティングプールからプロビジョンした場合は下記になります:

- **Cluster Name** - MKT\ *XYZ*
- **Subnet** - 10.**20**.\ *XYZ*\ .0
- **Cluster IP** - 10.**20**.\ *XYZ*\ .37

例:

- **Cluster Name** - POC055
- **Subnet** - 10.42.55.0
- **Cluster IP** - 10.42.55.37

ワークショップ全体を通じて、たとえば、次のように* XYZ *を正しいオクテットに置き換える必要がある場合がいくつかあります。:

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - IP Address
     - Description
   * - 10.42.\ *XYZ*\ .37
     - Nutanix Cluster Virtual IP
   * - 10.42.\ *XYZ*\ .39
     - **PC** VM IP, Prism Central
   * - 10.42.\ *XYZ*\ .40
     - **DC** VM IP, NTNXLAB.local Domain Controller

各クラスターはVMに使用できる2つのVLANが構成されています:

.. list-table::
  :widths: 25 25 10 40
  :header-rows: 1

  * - Network Name
    - Address
    - VLAN
    - DHCP Scope
  * - Primary
    - 10.42.\ *XYZ*\ .1/25
    - 0
    - 10.42.\ *XYZ*\ .50-10.42.\ *XYZ*\ .124
  * - Secondary
    - 10.42.\ *XYZ*\ .129/25
    - *XYZ1*
    - 10.42.\ *XYZ*\ .132-10.21.\ *XYZ*\ .253

Credentials
...........

.. note::

  * <クラスタパスワード> *は各クラスタに固有であり、インストラクターによって提供されます。

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Credential
     - Username
     - Password
   * - Prism Element
     - admin
     - *<Cluster Password>*
   * - Prism Central
     - admin
     - *<Cluster Password>*
   * - Controller VM
     - nutanix
     - *<Cluster Password>*
   * - Prism Central VM
     - nutanix
     - *<Cluster Password>*

各クラスターには、**NTNXLAB.local**ドメインにADサービスを提供する専用のドメインコントローラーVM **AutoAD**があります。 ドメインには次のユーザーとグループが入力されています:

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Group
     - Username(s)
     - Password
   * - Administrators
     - Administrator
     - nutanix/4u
   * - SSP Admins
     - adminuser01-adminuser25
     - nutanix/4u
   * - SSP Developers
     - devuser01-devuser25
     - nutanix/4u
   * - SSP Consumers
     - consumer01-consumer25
     - nutanix/4u
   * - SSP Operators
     - operator01-operator25
     - nutanix/4u
   * - SSP Custom
     - custom01-custom25
     - nutanix/4u
   * - Bootcamp Users
     - user01-user25
     - nutanix/4u

アクセス手順
+++++++++++++++++++

NutanixのHPOC環境にはさまざまな方法でアクセスできます:

ラボアクセスユーザー情報
...........................

PHX Based Clusters:
**ユーザー名:** PHX-POCxxx-User01 (最大PHX-POCxxx-User20), **パスワード:** *<インストラクターから提示します>*

RTP Based Clusters:
**ユーザー名:** RTP-POCxxx-User01 (最大RTP-POCxxx-User20), **パスワード:** *<インストラクターから提示します>*

Frame VDI
.........

ログイン先: https://frame.nutanix.com/x/labs

**Nutanix社員** - 個人の **NUTANIXDC** IDとパスワードを利用してください
**その他のユーザー** - 上記の **ラボアクセスユーザー情報** を利用してください

Parallels VDI
.................

PHXクラスタのログイン先: https://xld-uswest1.nutanix.com

RTPクラスタのログイン先: https://xld-useast1.nutanix.com

**Nutanix社員** - 個人の **NUTANIXDC** IDとパスワードを利用してください
**その他のユーザー** - 上記の **ラボアクセスユーザー情報** を利用してください

Employee Pulse Secure VPN
..........................

クライアントソフトウェアのダウンロードが必要:

PHXクラスタのログイン先: https://xld-uswest1.nutanix.com

RTPクラスタのログイン先: https://xld-useast1.nutanix.com

**Nutanix社員** - 個人の **NUTANIXDC** IDとパスワードを利用してください
**その他のユーザー** - 上記の **ラボアクセスユーザー情報** を利用してください

クライアントソフトウェアのインストール

Pulse Secure Clientの中で接続先を **Add** してください:

PHXに接続する場合:

- **Type** - Policy Secure (UAC) または、 Connection Server
- **Name** - X-Labs - PHX
- **Server URL** - xlv-uswest1.nutanix.com

RTPに接続する場合:

- **Type** - Policy Secure (UAC) または Connection Server
- **Name** - X-Labs - RTP
- **Server URL** - xlv-useast1.nutanix.com


Nutanixソフトウェアバージョン
++++++++++++++++++++

- **AHV Version** - AHV 20170830.337
- **AOS Version** - 5.11.2.3
- **PC Version** - 5.11.2.1
