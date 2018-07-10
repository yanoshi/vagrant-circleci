# Vagrantfile for CircleCI Local
Vagrant(VirtualBox)でCircleCI Localを動かす何か。

## Requirements
- Vagrant
  - https://www.vagrantup.com/
- VirtualBox
  - http://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html
- dotenv
  - `vagrant plugin install dotenv`

## Setup
```
vagrant up
```

## Usage
`CCI_TARGET_PATH` に対象のリポジトリパス( `.circleci` フォルダが存在しているディレクトリ )を設定して実行する感じ。

```
CCI_TARGET_PATH="target repogitory path" vagrant reload && vagrant provision
```

## Uninstall
```
yes | vagrant destroy
```

## License
MIT
