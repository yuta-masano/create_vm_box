create_vm_box
=============
VirtualBox ゲスト OS から vagrant box ファイルを作って `vagrant box add` までしてくれる playbook です。

使い方
------
1. CentOS 5 / CentOS 6 の ISO を用意して Virtualbox でゲストマシンを作る。
   - ゲストマシン名は **CentOS-5**, **CentOS-6** にすること。
   - ゲストマシンは外のネットワークに繋がるように設定しておくこと。
   - ゲストマシンの root ユーザのパスワードは vagrant にしておくこと。
   - ゲストマシンはホストマシンと SSH 通信できるように設定しておくこと。
1. このリポジトリを `git clone` する。
1. ./inventories/inventory.ini の `ansible_ssh_host` に、ホストマシンが名前解決可能なゲストマシンのホスト名、または IP アドレスを指定する。
1. ./inventories/{gropu_vars,host_vars}/\*.yml の変数を調整する。
1. ゲストマシンに proxy を通す場合は、./playbooks/main.yml の `environment` セクションを編集する。詳細は [Ansible 公式ドキュメント](http://docs.ansible.com/ansible/playbooks_environment.html)を参照。
1. リポジトリのルートで `ansible-playbook playbooks/main.yml` を実行する。
1. vagrant に `{{ vagrant_box_name }}` で指定した名前で box イメージが登録されている。また、./playbooks に `{{ vagrant_box_name }}.box` が生成されている。

制限事項
--------
- ansible 1.9.3 + vagrant 1.7.4 + VirtualBox 5.0.6 on Ubuntu 14.04 LTS で動作確認。
  - playbook の一箇所、[`yum: name=* state=latest` が失敗する](https://github.com/ansible/ansible-modules-core/issues/2013)ので command モジュールで回避している。
- CentOS 6 で動いた。
- CentOS 5 でも動くようにした。
  - ゲストマシンに予め python-simplejson をインストールしておく必要がある。  
    `ansible CentOS-5 -i inventories/inventory.ini -m raw 'yum -y install python-simplejson'`  
    詳細は [Qiita](http://qiita.com/yamasaki-masahide/items/4485a438125e6b1748ce)を参照。
- CentOS 7 ですか？ Sorry, 悪いが聞こえないよ。耳にバナナが入っててな。

開発の進め方
------------
### 開発向けの Issue リンク
- [機能追加 Issue はここから](https://github.com/yuta-masano/create_vm_box/issues/new?title=[feature]%20&body=%23%23%20現状%0A%0A%23%23%20理想%0A%0A%23%23%20実装方針&assignee=yuta-masano)
- [機能改善 Issue はここから](https://github.com/yuta-masano/create_vm_box/issues/new?title=[improve]%20&body=%23%23%20現状%0A%0A%23%23%20理想%0A%0A%23%23%20実装方針&assignee=yuta-masano)

### ご意見収集用 Issue リンク
- [不具合 Issue はここから](https://github.com/yuta-masano/create_vm_box/issues/new?title=[bug]%20&body=%23%23%20現状%0A%0A%23%23%20理想%0A%0A%23%23%20実装方針&assignee=yuta-masano)
- [要望 Issue はここから](https://github.com/yuta-masano/create_vm_box/issues/new?title=[idea]%20&body=%23%23%20現状%0A%0A%23%23%20理想%0A%0A%23%23%20実装方針&assignee=yuta-masano)
