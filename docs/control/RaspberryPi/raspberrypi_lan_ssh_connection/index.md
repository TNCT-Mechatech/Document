# Raspberry Pi 4B (Ubuntu 22.04 LTS) と Windows ノートPCを有線接続して VSCode から SSH 接続する手順

## 目的

- Raspberry Pi と Windows PC を **LANケーブルで直結**
- **VSCode の Remote-SSH 機能**を使って、Raspberry Pi に接続・開発を行う

---

## 構成概要

- Raspberry Pi 4B（Ubuntu 22.04 LTS、SSH有効）
- Windows ノートPC（VSCode、Remote-SSH 拡張、OpenSSH Client）
- 有線LANケーブル（ストレート/クロスどちらでも可）

---

## 1. Raspberry Pi のネットワーク設定

### 1.1 SSH サーバの有効化

```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

---

### 1.2 固定IPアドレスの設定（Netplan）

`/etc/netplan/01-network-manager-all.yaml` や `/etc/netplan/50-cloud-init.yaml` を編集

```yaml
network:
  version: 2
  renderer: NetworkManager # ubuntuServerの場合は"networkd"
  ethernets:
    eth0:
      dhcp4: no
      addresses: [192.168.137.2/24]
　　　# 上記のipv4アドレスの第3項はwindows側の設定でも使うので記憶又は記録しておく
```

> Ubuntu Server の場合は `renderer: networkd` に変更すること。

#### 設定適用

```bash
sudo netplan apply
```

#### ファイルパーミッションの修正（警告対処）

```bash
sudo chmod 600 /etc/netplan/01-network-manager-all.yaml
```

---

## 2. Windows ノートPCの設定（有線アダプター）

### 2.1 ネットワークアダプターに静的IPを設定

* 対象：使用中の有線LANアダプター
* 手順：

  1. 「ネットワークとインターネット」 > 「ネットワークの詳細設定」 > 「イーサネット」 > 「追加のプロパティを表示」
  2. 対象アダプターを右クリック → 「プロパティ」
  3. 「Internet Protocol Version 4 (TCP/IPv4)」を選択 → 「プロパティ」
  4. 以下のように入力：

| 設定項目              | 値             |
| ----------------- | ------------- |
| IP アドレス           | 192.168.137.1  |
| サブネットマスク          | 255.255.255.0 |
| デフォルトゲートウェイ / DNS | 空欄でOK         |

IP アドレスの欄は, 先ほどラズパイ側で記録、記憶したIPv4アドレスの第三項が一致するように書く。

---

## 3. 接続確認

### 3.1 Ping で疎通確認

Windows でコマンドプロンプトを開き：

```cmd
ping 192.168.137.2
```

---

## 4. VSCode の Remote-SSH 設定 (任意)

### 4.1 拡張機能をインストール

* `Remote - SSH` をインストール

### 4.2 接続手順

1. `Ctrl+Shift+P` → `Remote-SSH: Connect to Host`
2. 新しいホストを追加：

```
ssh ubuntu@192.168.10.2
```

3. `~/.ssh/config` を整備（任意）：

```ssh
Host rpi
    HostName 192.168.10.2
    User ubuntu
```

---

## 5. 注意点とトラブルシューティング

| 症状                  | 原因 / 対処                           |
| ------------------- | --------------------------------- |
| `connect timeout`   | IP設定・ケーブル接続不良                     　　　　　|
| `Permission denied` | ユーザー名 or パスワード誤り                  　　　　　|
| `ping` 応答なし     | Pi側のIP設定 or Windowsアダプター設定ミス　　　　      |
| `netplan` 警告      | YAMLファイルのパーミッション緩すぎ → `chmod 600`(任意) |

---

## 6. Windows 側の静的IPの弊害について

* 有線アダプターを静的IPにしたままだと、通常のLAN（インターネット）接続ができなくなる可能性あり
* 回避方法：

  * **USB-LANアダプターを追加**して Pi専用にする
  * 必要時だけ静的IP設定し、不要時は DHCP に戻す

---

## 参考

* Ubuntu Netplan: [https://netplan.io](https://netplan.io)
* VSCode Remote-SSH: [https://code.visualstudio.com/docs/remote/ssh](https://code.visualstudio.com/docs/remote/ssh)