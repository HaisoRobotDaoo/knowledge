# NVIDIAデバイスドライバ

```bash
nvidia-smi

# 入ってなければ
ubuntu-drivers devices
```

**`<driver-name>`はrecommendのものを選ぶ**

---

了解（ENTER）

![パスワード設定](./image/NVIDIAデバイスドライバ_01_setup_password.png)

任意のパスワードを登録（忘れないで！！）し、了解
困ったら `12345678` でOK！

---

![インストール成功](./image/NVIDIAデバイスドライバ_02_install_success.png)

成功したら再起動
```bash
sudo reboot
```

---

## Enroll MOK

![Enroll MOK画面](./image/NVIDIAデバイスドライバ_03_enroll_mok.jpg)

Continue

![Continue画面](./image/NVIDIAデバイスドライバ_04_continue.jpg)

Yes

![Yes画面](./image/NVIDIAデバイスドライバ_05_yes.jpg)

インストール時に登録したパスワードを入力
> **ここでパスワード忘れてたらがちでキレる　※前例あり**

![パスワード入力画面](./image/NVIDIAデバイスドライバ_06_enter_password.jpg)

Reboot

![Reboot画面](./image/NVIDIAデバイスドライバ_07_reboot.jpg)

*画像提供者：大槻 玲弥（おおつき れいや）*

---

### **F11連打！！！！！！！！！！！！！！！**
> **絶対わすれるなよおおおおおおおおおおおおおお？！**

再起動後、もう一度確認
→ GPUの情報がでれば成功
```bash
nvidia-smi
```
