# Linuxでマウスのボタン割り当て

`/proc/bus/input/devices`からデバイスを探し、`eventN`の番号を覚えとく

```console
$ cat /proc/bus/input/devices

I: Bus=0003 Vendor=046d Product=c08f Version=0111
N: Name="Logitech G403 HERO Gaming Mouse"
P: Phys=usb-0000:00:14.0-1/input0
S: Sysfs=/devices/pci0000:00/0000:00:14.0/usb1/1-1/1-1:1.0/0003:046D:C08F.0008/input/input21
U: Uniq=047032613533
H: Handlers=event4 mouse0 
B: PROP=0
B: EV=17
B: KEY=ffe70000 0 c0000000 0 0
B: REL=1943
B: MSC=10
```

(Option) `BTN_EXTRA`や`BTN_SIDE`として認識されている。

```console
$ sudo libinput debug-events --device /dev/input/event4
-event4   DEVICE_ADDED            Logitech G403 HERO Gaming Mouse   seat0 default group1  cap:p left scroll-nat scroll-button
 event4   POINTER_BUTTON          +0.000s	BTN_EXTRA (276) pressed, seat count: 1
 event4   POINTER_BUTTON          +0.117s	BTN_EXTRA (276) released, seat count: 0
 event4   POINTER_BUTTON          +0.914s	BTN_SIDE (275) pressed, seat count: 1
 event4   POINTER_BUTTON          +1.033s	BTN_SIDE (275) released, seat count: 0
```

valueの値を覚えとく

```console
$ sudo evtest /dev/input/event4

Event: time 1670408004.593393, -------------- SYN_REPORT ------------
Event: time 1670408006.726532, type 4 (EV_MSC), code 4 (MSC_SCAN), value 90005
Event: time 1670408006.726532, type 1 (EV_KEY), code 159 (KEY_FORWARD), value 1
Event: time 1670408006.726532, -------------- SYN_REPORT ------------
Event: time 1670408006.843401, type 4 (EV_MSC), code 4 (MSC_SCAN), value 90005
Event: time 1670408006.843401, type 1 (EV_KEY), code 159 (KEY_FORWARD), value 0
Event: time 1670408006.843401, -------------- SYN_REPORT ------------
Event: time 1670408007.456355, type 4 (EV_MSC), code 4 (MSC_SCAN), value 90004
Event: time 1670408007.456355, type 1 (EV_KEY), code 158 (KEY_BACK), value 1
Event: time 1670408007.456355, -------------- SYN_REPORT ------------
Event: time 1670408007.567318, type 4 (EV_MSC), code 4 (MSC_SCAN), value 90004
Event: time 1670408007.567318, type 1 (EV_KEY), code 158 (KEY_BACK), value 0
Event: time 1670408007.567318, -------------- SYN_REPORT ------------
```

```conf
# /etc/udev/hwdb.d/99-logitech-g403-hero-gaming-mouse.hwdb
evdev:name:Logitech G403 HERO Gaming Mouse:*
  KEYBOARD_KEY_90004=forward
  KEYBOARD_KEY_90005=back
```

KeyCodeは[ここ](https://github.com/wayland-project/libinput/blob/master/include/linux/linux/input-event-codes.h)を参照

dbを再生成

```
sudo udevadm hwdb --update
sudo udevadm trigger
```

```console
$ sudo udevadm info /dev/input/event4

E: KEYBOARD_KEY_90004=back
E: KEYBOARD_KEY_90005=forward
```

変わってるか確認

反映されない場合は抜き差ししたら上手く行った

```console
$ sudo libinput debug-events --device /dev/input/event4

-event4   DEVICE_ADDED            Logitech G403 HERO Gaming Mouse   seat0 default group1  cap:kp left scroll-nat scroll-button
 event4   KEYBOARD_KEY            +0.000s	KEY_FORWARD (159) pressed
 event4   POINTER_MOTION          +0.029s	  0.30/  0.00 ( +1.00/ +0.00)
 event4   KEYBOARD_KEY            +0.099s	KEY_FORWARD (159) released
 event4   KEYBOARD_KEY            +0.577s	KEY_BACK (158) pressed
 event4   KEYBOARD_KEY            +0.726s	KEY_BACK (158) released
```
