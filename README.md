create_vm_box
=============
VirtualBox ゲスト OS から vagrant box ファイルを作って vagrant box add までしてくれる playbook です。

使い方
------
1. 適当な ISO を用意して Virtualbox でゲストマシンを作る。
   - ゲストマシンは外のネットワークに繋がるように設定しておくこと。
   - ゲストマシンはホストマシンと通信できるように設定しとくこと。
1. このリポジトリを clone する。
1. ./inventories/inventory.ini の `ansible_ssh_host` をゲストマシンのホスト名、または IP アドレスにする。
1. ./inventories/host_vars/vm.yml の変数を調整する。
1. ゲストマシンに proxy を通す場合は、./playbooks/main.yml の `environment` セクションを編集する。詳細は[公式ドキュメント](http://docs.ansible.com/ansible/playbooks_environment.html)を参照。
1. リポジトリのルートで `ansible-playbook playbooks/main.yml` を実行する。
1. vagrant に `{{ vagrant_box_name }}` で box イメージが登録されている。また、./playbooks に `{{ vagrant_box_name }}.box` が生成されている。

制限事項
--------
- CentOS 6 でしか動作確認していない。
- たぶん CentOS 5 でも動くと思う。
- ansible 1.9.3 で動作確認。
  - playbook の一箇所、[`yum: name=* state=latest` が失敗する](https://github.com/ansible/ansible-modules-core/issues/2013)ので command モジュールで回避している。

開発の進め方
------------
### 開発向けの Issue リンク
- [機能追加 Issue はここから](https://github.com/yuta-masano/create_vm_box/issues/new?title=[feature/ROLE_NAME]%20&body=%23%23%20現状%0A%0A%23%23%20理想%0A%0A%23%23%20実装方針&assignee=yuta-masano)
- [機能改善 Issue はここから](https://github.com/yuta-masano/create_vm_box/issues/new?title=[improve/ROLE_NAME]%20&body=%23%23%20現状%0A%0A%23%23%20理想%0A%0A%23%23%20実装方針&assignee=yuta-masano)

### ご意見収集用 Issue リンク
- [不具合 Issue はここから](https://github.com/yuta-masano/create_vm_box/issues/new?title=[bug/ROLE_NAME]%20&body=%23%23%20現状%0A%0A%23%23%20理想%0A%0A%23%23%20実装方針&assignee=yuta-masano)
- [要望 Issue はここから](https://github.com/yuta-masano/create_vm_box/issues/new?title=[idea/ROLE_NAME]%20&body=%23%23%20現状%0A%0A%23%23%20理想%0A%0A%23%23%20実装方針&assignee=yuta-masano)
